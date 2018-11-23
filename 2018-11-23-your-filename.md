package com.dbs.edge.automationtest.test;

import com.dbs.edge.automationtest.test.kafka.ConsumerService;
import com.dbs.edge.automationtest.test.kafka.ProducerService;
import junit.framework.TestCase;
import lombok.AllArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.io.*;
import java.net.URL;
import java.util.*;
import java.util.concurrent.TimeUnit;

@RunWith(SpringRunner.class)
@SpringBootTest
@Slf4j
public class PortfolioMapperTest extends TestCase {
    @Autowired
    ConsumerService consumerService;

    @Autowired
    ProducerService producerService;

    private static SortedMap<String, List<LineData>> testCases = new TreeMap<>();
    private static List<ExpectData> expectOutputList = new ArrayList<>();
    public static List<ExpectData> realOutputList = new ArrayList<>();

    //
    public static String EDGE_PORTFOLIO_STREAM = "edge_portfolio_stream";
    public static String EDGE_SPLIT_DEAL_STREAM = "edge_split_deal_stream";
    public static String EDGE_PORTFOLIO_DEAL_STREAM = "edge_portfolio_deal_stream";
    public static String RESOURCE_DATA_FOLDER = "portfolio_mapper_data";
    public static int LATENCY_LIMIT = 500;

    public enum INPUT_TYPE {
        IN("INPUT"),
        OUT("OUTPUT");
        public String value;

        INPUT_TYPE(String type) {
            this.value = type;
        }
    }

    @AllArgsConstructor
    static public class LineData {
        String id;
        String type;
        String topicName;
        String key;
        String data;
    }

    @AllArgsConstructor
    static public class ExpectData {
        String id;
        String key;
        String data;
        long timestamp;
    }

    private String readFromInputStream(InputStream inputStream)
            throws IOException {
        StringBuilder resultStringBuilder = new StringBuilder();
        try (BufferedReader br
                     = new BufferedReader(new InputStreamReader(inputStream))) {
            String line;
            while ((line = br.readLine()) != null) {
                resultStringBuilder.append(line).append("\n");
            }
        }
        return resultStringBuilder.toString();
    }

    private static File[] getResourceFolderFiles(String folder) {
        ClassLoader loader = Thread.currentThread().getContextClassLoader();
        URL url = loader.getResource(folder);
        String path = url.getPath();
        return new File(path).listFiles();
    }

    private void getFileData(String folderName) {
        File[] files = getResourceFolderFiles(folderName);
        for (File f : files) {
            log.info("Reading file:{}", f.getName());
            InputStream inputStream = null;
            try {
                inputStream = new FileInputStream(f);
                String[] lines = readFromInputStream(inputStream).split("\n");
                for (int i = 1; i < lines.length; i++) {
                    String[] data = lines[i].split(";");
                    String[] jsonData = data[3].split("\\|");
                    LineData ld = new LineData(data[0], data[1], data[2], jsonData[0], jsonData[1]);

                    //
                    if (testCases.containsKey(ld.id)) {
                        testCases.get(ld.id).add(ld);
                    } else {
                        List<LineData> ls = new ArrayList<>();
                        ls.add(ld);
                        testCases.put(ld.id, ls);
                    }

                    if (ld.type.equals(INPUT_TYPE.OUT.value)) {
                        String key = jsonData[0];
                        String value = jsonData[1];
                        ExpectData expectData = new ExpectData(data[0], key, value, 0);
                        expectOutputList.add(expectData);
                    }
                }
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                if (inputStream != null) {
                    try {
                        inputStream.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }

    //

    /**
     * verify deal order
     * verify deal content
     *
     * @param expectOutputList
     * @param realOutputList
     */
    public void verify(List<ExpectData> expectOutputList, List<ExpectData> realOutputList) {
        int index = 0;
        for (ExpectData realData : realOutputList) {
            String key = realData.key;
            ExpectData expectData = expectOutputList.get(index);
            String tcId = expectData.id;
            long latency = realData.timestamp - expectData.timestamp;
            if (!key.equals(expectData.key)) {
                log.error("TestCase {} failed, mismatch order", tcId);
                ExpectData exDt = expectOutputList.stream().filter(p -> p.key.equals(key)).findAny().orElse(null);
                expectOutputList.remove(exDt);
            } else {
                if (!realData.data.equals(expectData.data)) {
                    log.error("TestCase {} failed, expected output and real output deals are not matched", tcId);
                    log.error("Expect deal: {}", expectData.data);
                    log.error("Real   deal: {}", realData.data);
                } else if (latency > LATENCY_LIMIT) { // 10 milliseconds
                    log.error("TestCase {} failed, latency {} is larger than 10ms", tcId, latency);
                } else {
                    log.info("TestCase {} passed, latency time {}", tcId, latency);
                }
            }
            index++;
        }
    }

    /**
     * This test will read all test data in folder: portfolio_mapper_data
     * Generating all necessary test data:
     *  - testCases is sorted list of test cases
     *  - expectOutputList is an list of expected deals
     *  - realOutputList is an list of real output deals from Kafka
     * @throws Exception
     */
    @Test
    public void Test1() throws Exception {
        int index = 1;
        log.info("Get portfolio_mapper_data from CSV file");
        this.getFileData(RESOURCE_DATA_FOLDER);

        //Waiting to Consumer and producer ready
        TimeUnit.SECONDS.sleep(10);

        log.info("Starting sending input to kafka");
        for (Map.Entry<String, List<LineData>> entry : testCases.entrySet()) {
            String tcId = entry.getKey();
            log.info("Executing testcase:{} - ID: {}", index, tcId);
            List<LineData> lines = entry.getValue();
            for (LineData l : lines) {
                String topic = l.topicName;
                String data = l.data;
                String key = l.key;
                String inOut = l.type;
                log.info("Line: tc id:{}, topic:{}, key:{}, type:{}, portfolio_mapper_data:{}", l.id, l.topicName, l.type, l.data);

                //
                if (inOut.equals(INPUT_TYPE.IN.value)) {
                    long timestamp = producerService.sendDeal(data, key, topic);
                    //Store timestamp when sending deal to kafka
                    if (l.topicName.equals(EDGE_SPLIT_DEAL_STREAM)) {
                        ExpectData expectData = expectOutputList.stream().filter(p -> p.key.equals(key)).findAny().orElse(null);
                        expectData.timestamp = timestamp;
                        expectOutputList.remove(expectData);
                        expectOutputList.add(expectData);
                    }
                } else if (inOut.equals(INPUT_TYPE.OUT.value)) {
                    //Ignore output
                }
            }
            index++;
        }

        //Waiting 60 seconds to make sure output available
        TimeUnit.SECONDS.sleep(10);

        //
        for (ExpectData expectData : expectOutputList) {
            String key = expectData.key;
            log.info("Expect: key {}, timestamp {}, value {}", key, expectData.timestamp, expectData.data);
        }

        //
        for (ExpectData realData : realOutputList) {
            String key = realData.key;
            log.info("Real: key {}, timestamp {}, value {}", key, realData.timestamp, realData.data);
        }

        verify(expectOutputList, realOutputList);
    }
}


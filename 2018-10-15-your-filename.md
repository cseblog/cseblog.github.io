## A New Post

Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
package com.company;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class Main {

    public static List<Integer> findBestTeam(int max, int limit, Integer[]s){
        Arrays.sort(s, Collections.reverseOrder());
        List<Integer> result = new ArrayList<>();
        int tmp = s[0];
        for (int i = 1; i <= max; i++){
            if(i <= limit){
                result.add(s[i-1]);
            } else {
                if (i > limit && tmp == s[i-1]) {
                    result.add(s[i-1]);
                }else {
                    break;
                }
            }

            tmp = s[i-1];
        }
        return result;
    }

    
    public static void main(String[] args) {
        // write your code here
        System.out.println("Hello world!");
        
        int a1 = 7;
        int b1 = 2;
        Integer []s1 = {3, 5,5,5, 2, 4,4, 5};

        List<Integer> roundAList = findBestTeam(a1, b1, s1);
        System.out.println(roundAList.size());

        int a2 = 6;
        int b2 = 4;
        Integer []s2 = {6, 5, 4, 3, 2, 1};
        
        List<Integer> roundBList = findBestTeam(a2, b2,s2);
        System.out.println(roundBList.size());

    }
}

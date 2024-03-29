---
title: Setting Dev Environment For Golang In Window
layout: post
author: Andrew HQT
categories: Engineering
tags: English
published: true
---

# Setting dev environment for Golang in Window

    - Install Golang compiler 
    - Install Goland IDE
    - Build helloworld app in Golang
    - Where to start learning?
    - Add 'go fmt' and checkstyle to Goland IDE
    - Create sample project 
    - Auto generating unit test with gotest

---

### 1. Install Golang compiler

- Go to the download page: [https://golang.org/dl/](https://golang.org/dl/).  
- Looking for the portable package for Window: go1.xx.windows-xxx.zip.  
- Extract the zip file into somewhere in your computer.  For example: C:\Users\andrew\go-1.10\bin.    
- Setting global GOROOT variable. You may want to know what is GOROOT and GOPATH.    
- Click here: []()
[https://stackoverflow.com/questions/7970390/what-should-be-the-values-of-gopath-and-goroot](https://stackoverflow.com/questions/7970390/what-should-be-the-values-of-gopath-and-goroot)

![Setting GOROOT in Window 10]({{site.baseurl}}/images/setGOROOT.PNG)

### 2. Install Goland IDE

To code Golang, you can use any IDE that is best convenient for you. 
For example Sublime, Vim, VS, Goland
In my case, I would like to suggest to use Goland IDE because it is best Golang IDE at the time I write this blog.
Link: [https://www.jetbrains.com/go/](https://www.jetbrains.com/go/)
    
    
### 3. Build helloworld app in Golang

- Open Goland IDE  
- Start new project  
- Create "src" folder  
- Create first Golang app: main.go  
  ![Hello World program]({{site.baseurl}}/images/hello_program.PNG)
- Setting GOROOT and GOPATH variables
  ![Setting GOROOT & GOPATH in Goland]({{site.baseurl}}/images/goroot-gopath.PNG)

### 4. Where to start learning?

- Golang tour will provide you some basic knowledges of Golang 
[https://tour.golang.org/welcome/1](https://tour.golang.org/welcome/1)
[https://golang.org/doc/](https://golang.org/doc/)

- Awesome list -> All of necessary libraries we may need then the link below can help you find a good enough library. 
Link: [https://awesome-go.com/](https://awesome-go.com/)

- Code review comment -> How to justify Golang code to review.  
Link: [https://github.com/golang/go/wiki/CodeReviewComments](https://github.com/golang/go/wiki/CodeReviewComments)

### 5. Add 'go fmt' to Goland
'go fmt' is a tool of Golang being used to format Golang code. We can add this tool to Goland IDE to run this tool every time the file of code is saved. 

- Add 'go fmt' to Goland
    File -> Settings -> Tools -> File Watchers
![fmt.PNG]({{site.baseurl}}/images/fmt.PNG)

              
### 6. Install 'checkstyle' to Goland
*checkstyle tool is a general tool to checking Golang code styles. We can change setting in it's config file.*  
Steps:
+ Clone project: [https://github.com/qiniu/checkstyle](https://github.com/qiniu/checkstyle)
+ Build a binary and put it somewhere
+ For example, I put it under C:\Users\huynhtrung\gotools\checkstyle
+ Add checkstyle to Goland
			
 ![checkstyle.PNG]({{site.baseurl}}/images/checkstyle.PNG)

Force common style rules:
* Maximize line count of the file
* Maximize line count of function
* Maximize parameter count of function
* Maximize return variable count
* Have code formatted by "gofmt"
* Package name should not contain _ or camel 
* const/var/function/import name should use camel name

~~~
    {
        "file_line":500,
        "func_line":70,
        "params_num":7,
        "results_num":3,
        "formated": true,
        "pkg_name": true,
        "camel_name": true,
        "ignore":[
            "testdata/*",
            "temp/*"
        ],
        "fatal":[
            "formated",
            "file_line",
            "func_line",
            "params_num",
            "results_num",
            "formated",
            "pkg_name",
            "camel_name"
        ]
    }
~~~

### 7. Setup 'gotest' and generate Unit test cases

Golang has built-in "testing" package. Which can help us creating unit test cases easily.  
To generate test cases template automatically. We use this library:  
[https://github.com/cweill/gotests](https://github.com/cweill/gotests)
    - Setup
    - Clone project
    - Build gotests
-> Put the binary into somewhere of GOPATH or GOROOT paths.
In my case, I put it in GOROOT folder: C:\Users\andrew\go-1.10\bin\gotests.exe

- To generate testcases in Goland:
    + Create test file with name <go filename>_test.go [ Ctrl + Shift + T]
    + Select the functions, right click -> Go To ->Test
    + Edit run Go test configuration:
            Run -> Edit configurations ->  Choose "gotest" -> "Go tool arguments" -> "-v -cover"
![watcher_gotests.PNG]({{site.baseurl}}/images/watcher_gotests.PNG)
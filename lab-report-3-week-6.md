# Lab Report 3

Chosen task: **Copy whole directories with scp -r**

1. **Copying whole directories to a remote server.** The `scp -r` flag (`-r` standing for recursive) lets us copy whole directories from a local computer to a server or vice versa. To copy the `markdown-parse` directory to the remote `ieng6` server, I used `scp -r markdown-parse cs15lwi22alx@ieng6.ucsd.edu:~/`:
![](3step1.png)

2. **Logging in and running tests.** After the folder is moved over, I logged in to the remote server with `ssh cs15lwi22alx@ieng6.ucsd.edu` and ran the tests:
![](3step2.png)

3. **Combining into one command.** To make running remote tests easier, we can combine the transfer of remote files and the running of tests into one command. First I ran `rm -rf markdown-parse/` in the remote directory to remove `markdown-parse`, so that we can make sure our transfer works again. Combining the commands from step 1 and 2 into one, we get: `scp -r markdown-parse cs15lwi22alx@ieng6.ucsd.edu:~/; ssh cs15lwi22alx@ieng6.ucsd.edu "cd markdown-parse/; make test"`. Running these command on my local machine, I get the following error: 

```
peter@Peters-MacBook-Pro Downloads % scp -r markdown-parse cs15lwi22alx@ieng6.ucsd.edu:~/; ssh cs15lwi22alx@ieng6.ucsd.edu "cd markdown-parse/; make test"
test-file5.md                                                                                  100%   39     0.3KB/s   00:00    
MarkdownParseTest.java                                                                         100% 4310   130.4KB/s   00:00    
test-file4.md                                                                                  100%   27     4.4KB/s   00:00    
breaking-file.md                                                                               100%  122    15.1KB/s   00:00    
test-file-4.md                                                                                 100%  153    18.7KB/s   00:00    
makefile                                                                                       100%  352    43.6KB/s   00:00    
mdparse                                                                                        100%   74     0.6KB/s   00:00    
test-file-1.md                                                                                 100%   73     8.9KB/s   00:00    
test-file.md                                                                                   100%   73    10.1KB/s   00:00    
test-file-5.md                                                                                 100%   37     5.2KB/s   00:00    
test-file-2.md                                                                                 100%  120    18.6KB/s   00:00    
MarkdownParse.java                                                                             100% 1947    79.5KB/s   00:00    
.gitignore                                                                                     100%    8     1.3KB/s   00:00    
main.yml                                                                                       100%  987     7.6KB/s   00:00    
test-file8.md                                                                                  100%   30     4.0KB/s   00:00    
junit-4.13.2.jar                                                                               100%  376KB   1.1MB/s   00:00    
hamcrest-core-1.3.jar                                                                          100%   44KB  75.2KB/s   00:00    
test-file-3.md                                                                                 100%   73    11.6KB/s   00:00    
test-file3.md                                                                                  100%   27     3.7KB/s   00:00    
test-file7.md                                                                                  100%    2     0.2KB/s   00:00    
test-file6.md                                                                                  100%   27     0.2KB/s   00:00    
test-file2.md                                                                                  100%  110     0.9KB/s   00:00    
javac MarkdownParse.java
MarkdownParse.java:48: error: cannot find symbol
		Path fileName = Path.of(args[0]);
		                    ^
  symbol:   method of(String)
  location: interface Path
MarkdownParse.java:49: error: cannot find symbol
	    String contents = Files.readString(fileName);
	                           ^
  symbol:   method readString(Path)
  location: class Files
2 errors
make: *** [MarkdownParse.class] Error 1
```

It's really not clear why this error appears, because running the `ssh` command first and then the commands in the quotes gives the correct result:

```
peter@Peters-MacBook-Pro Downloads % ssh cs15lwi22alx@ieng6.ucsd.edu
Last login: Fri Feb 11 14:45:41 2022 from 128.54.184.145
quota: No filesystem specified.
Hello cs15lwi22alx, you are currently logged into ieng6-202.ucsd.edu

You are using 0% CPU on this system

Cluster Status 
Hostname     Time    #Users  Load  Averages  
ieng6-201   14:45:01   48  1.37,  1.21,  0.96
ieng6-202   14:45:01   30  2.11,  2.22,  2.01
ieng6-203   14:45:01   48  1.86,  2.19,  2.28

 
Fri Feb 11, 2022  2:49pm - Prepping cs15lwi22
[cs15lwi22alx@ieng6-202]:~:93$ cd markdown-parse/; make test
javac MarkdownParse.java
javac -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar MarkdownParseTest.java
java -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar org.junit.runner.JUnitCore MarkdownParseTest
JUnit version 4.13.2
..............
Time: 0.024

OK (14 tests)
```

This issue may be caused by calling `ssh` in a one-liner not starting an interactive terminal shell, but it's very hard to tell without much more effort or having access to the server code itself (the server may, for example, run some scripts on login that it doesn't run when just using a one-liner `ssh`). Many reasons are discussed on the question [Difference between different invocations of ssh](https://superuser.com/questions/1525412/difference-between-different-invocations-of-ssh), but none of them lead to a fix that works or one that could be implemented on my end.
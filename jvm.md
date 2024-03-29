# JVM

## Tips

### [jps - Java Virtual Machine Process Status Tool](https://docs.oracle.com/javase/7/docs/technotes/tools/share/jps.html)

```
jps # jps: Print a list of Java processes and information about them
jps -h
jps -v # shows params
```

### [jcmd](https://docs.oracle.com/en/java/javase/11/tools/jcmd.html#GUID-59153599-875E-447D-8D98-0078A5778F05)

```
jmap -histo <pid>
```

### [jmap](https://docs.oracle.com/en/java/javase/11/tools/jmap.html#GUID-D2340719-82BA-4077-B0F3-2803269B7F41)


### [Flight Recorder (JFR)](https://docs.oracle.com/en/java/javase/11/troubleshoot/diagnostic-tools.html#GUID-D38849B6-61C7-4ED6-A395-EA4BC32A9FD6)


### [JDK Mission Control (JMC)](https://docs.oracle.com/en/java/javase/11/troubleshoot/diagnostic-tools.html#GUID-3FA1CF76-96FA-41A6-8F38-DC83171EE834)



## Links

[Diagnosing a Running JVM](https://www.baeldung.com/running-jvm-diagnose)

[Find out your Java heap memory size](https://github.com/andrzejsydor/psc/issues/55)

### Windows

```
java -XX:+PrintFlagsFinal -version | findstr /i "HeapSize PermSize ThreadStackSize"
````

### Linux

```
java -XX:+PrintFlagsFinal -version | grep -iE 'HeapSize|PermSize|ThreadStackSize'
```

### MacOS

```
java -XX:+PrintFlagsFinal -version | grep -iE 'heapsize|permsize|threadstacksize'
```

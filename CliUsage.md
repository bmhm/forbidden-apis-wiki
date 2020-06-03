# Command Line Usage Instructions #

_(since version 1.1)_ You can call the forbidden API checker from the command line:

```
$ java -jar forbiddenapis-3.0.1.jar
usage: java -jar forbiddenapis-3.0.1.jar [options]
Scans a set of class files for forbidden API usage.
    --allowmissingclasses                don't fail if a referenced class
                                         is missing on classpath
    --allowunresolvablesignatures        DEPRECATED: don't fail if a
                                         signature is not resolving
 -b,--bundledsignatures <name>           name of a bundled signatures
                                         definition (separated by commas
                                         or option can be given multiple
                                         times)
 -c,--classpath <path>                   class search path of directories
                                         and zip/jar files
 -d,--dir <directory>                    directory with class files to
                                         check for forbidden api usage;
                                         this directory is also added to
                                         classpath
 -e,--excludes <pattern>                 ANT-style pattern to exclude some
                                         files from checks (separated by
                                         commas or option can be given
                                         multiple times)
 -f,--signaturesfile <file>              path to a file containing
                                         signatures (option can be given
                                         multiple times)
 -h,--help                               print this help
 -i,--includes <pattern>                 ANT-style pattern to select class
                                         files (separated by commas or
                                         option can be given multiple
                                         times, defaults to '**/*.class')
    --ignoresignaturesofmissingclasses   if a class is missing while
                                         parsing signatures files, all
                                         methods and fields from this
                                         class are silently ignored
    --suppressannotation <classname>     class name or glob pattern of
                                         annotation that suppresses error
                                         reporting in
                                         classes/methods/fields (separated
                                         by commas or option can be given
                                         multiple times)
 -V,--version                            print product version and exit
Exit codes: 0 = SUCCESS, 1 = forbidden API detected, 2 = invalid command
line, 3 = unsupported JDK version, 4 = other error (I/O,...)
```

The command line parameters match those of the [Ant Task](AntUsage).

The detailed documentation (based on nightly snapshots) can be found here: https://jenkins.thetaphi.de/job/Forbidden-APIs/javadoc/

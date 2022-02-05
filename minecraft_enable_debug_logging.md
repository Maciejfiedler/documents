**Credit to u/RoyCurtis https://www.reddit.com/r/admincraft/comments/69271l/guide_controlling_console_and_log_output_with/**

## Create a log4j2.xml
Add the contents
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration monitorInterval="5" status="WARN" packages="com.mojang.util">
    
    <!-- Defines destinations of log messages -->
    <Appenders>
        <!-- Output to console (Windows compatiability) -->
        <Console name="WINDOWS_COMPAT" target="SYSTEM_OUT">
            <PatternLayout pattern="[%d{HH:mm:ss} %level (%c)]: %msg%n" />
        </Console>
        
        <!-- Output to console with pattern: [00:00:00 INFO]: Message -->
        <Queue name="TerminalConsole">
            <!-- https://logging.apache.org/log4j/2.x/manual/layouts.html#PatternLayout -->
            <PatternLayout pattern="[%d{HH:mm:ss} %level (%c)]: %msg%n" />
        </Queue>
        
        <!-- Output to rolling (latest.log) and auto-compressed files with more verbose pattern -->
        <RollingRandomAccessFile name="File" fileName="logs/latest.log" filePattern="logs/%d{yyyy-MM-dd}-%i.log.gz">
            <!-- https://logging.apache.org/log4j/2.x/manual/layouts.html#PatternLayout -->
            <PatternLayout pattern="[%d{HH:mm:ss}] [%t/%level]: %msg%n" />
            <Policies>
                <!-- Moves and compresses latest.log to new file at midnight -->
                <TimeBasedTriggeringPolicy />
                <!-- Moves and compresses latest.log to new file on startup -->
                <OnStartupTriggeringPolicy />
            </Policies>
            <DefaultRolloverStrategy max="1000"/>
        </RollingRandomAccessFile>
    </Appenders>
  
    <Loggers>
        <!-- Global logger; affects every other logger. Restricts messages to INFO or higher level -->
        <Root level="info">
            <!-- Global filters -->
            <Filters>
                <!-- Disables logging of network packet handling -->
                <MarkerFilter marker="NETWORK_PACKETS" onMatch="DENY" onMismatch="NEUTRAL" />
            </Filters>
            
            <!-- Logs to all 3 destinations -->
            
            <AppenderRef ref="WINDOWS_COMPAT"/>
            <AppenderRef ref="File"/>
            <AppenderRef ref="TerminalConsole"/>
            
        </Root>

        <Logger additivity="false" level="ALL" name="AmogusPlugin">
            <AppenderRef ref="WINDOWS_COMPAT"/>
            <AppenderRef ref="TerminalConsole"/>
        </Logger>
    </Loggers>
</Configuration>
```
The last occurence of `name` (here `AmogusPlugin`) set the name of your Java Plugin Package

For more configuration check out https://www.reddit.com/r/admincraft/comments/69271l/guide_controlling_console_and_log_output_with/

## Execute with the configuration
To execute the server now with the configuration add `-Dlog4j.configurationFile=log4j2.xml` to your JVM arguments

e.g. `java -Dlog4j.configurationFile=log4j2.xml -jar server.jar`

or `java -Xmx4G -Dlog4j.configurationFile=custom_log.xml -jar server.jar --nogui`
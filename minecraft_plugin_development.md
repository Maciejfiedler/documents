## Add gradle libraries
```kotlin
repositories {
    // Use Maven Central for resolving dependencies.
    mavenCentral()
    maven("https://hub.spigotmc.org/nexus/content/repositories/snapshots/")
}

dependencies {
    // Use JUnit Jupiter for testing.
    testImplementation("org.junit.jupiter:junit-jupiter:5.7.2")

    // This dependency is used by the application.
    implementation("com.google.guava:guava:30.1.1-jre")

    implementation("org.apache.logging.log4j", "log4j-api", "2.17.1")
    implementation("org.apache.logging.log4j", "log4j-core", "2.17.1")

    compileOnly("org.spigotmc:spigot-api:1.18.1-R0.1-SNAPSHOT")
}
```

## Add plugin.yml in resources folder of the package
```yml
main: AmogusPlugin.App
name: AmogusPlugin
version: 1.0.1
api-version: 1.18
```

## get Log4j Logger
Use the Log4j Logger for the class instead of `this.getLogger()`

```java
package AmogusPlugin;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.bukkit.plugin.java.JavaPlugin;

public class App extends JavaPlugin {
    Logger logger = LogManager.getLogger();
    @Override
    public void onEnable() {
        logger.debug("HELLO FROM DEBUG");
        logger.info("HELLO FROM INFO");
        logger.info(logger.getName().toString());
    }
    @Override
    public void onDisable() {}
}

```
## Combine Angular and Spring boot as one application


### Create a Java project using Spring initializer

```shell script
./gradlew clean build
./gradlew bootRun
```

Ensure the spring boot app starts without any error. 

### Create a angular UI project. 
```shell script
ng new fudo-ui
cd fudo-ui
ng serve --o
```

Hit the browser: 
http://localhost:4200/


### Combine the angular and spring boot app into one project. 
If gradle is not present in your machine, ensure you install gradle depending upon your OS, for mac issue the following
```shell script
brew install gradle
```

#### Run gradle init on the fudo-ui project 
```shell script
cd fudo-ui
gradle init
```

#### Run gradle init on the root project 
```shell script
cd ..
gradle init
```

Add the following in settings.gradle in the root project
#### fudo-app project changes
fudo-app/settings.gradle

```yaml
include 'fudo-service', 'fudo-ui'
```
fudo-app/build.gradle
```groovy
allprojects {
    group = 'cs.io'
    version = '0.0.1-SNAPSHOT'
}
```

Update build.gradle in fudo-ui with the following
#### fudo-ui project changes
```groovy
plugins {
  id 'java'
  id("com.github.node-gradle.node") version "2.2.0"
}

// 2
node {
  version = '10.16.3'
  npmVersion = '6.9.0'
  download = true
}

// 3
jar.dependsOn 'npm_run_build'

// 4
jar {
  from 'dist/fudo-ui' into 'static'
}

```

#### fudo-service project changes
build.gradle (Add the following in dependencies section)

```groovy
	implementation(project(':fudo-ui'))
```

### Build the application..
Run fodo-app/
```shell script
./gradlew clean build 
```

### Test the application
java -jar fudo-service/build/libs/fudo-service-0.0.1-SNAPSHOT.jar    

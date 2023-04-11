# Building Java Projects with Gradle

##### This guide walks you through using Gradle to build a simple Java project.

### What you’ll need
+ A favorite text editor or IDE
+ JDK 11 or later
+ Install Gradle

### Install Gradle
+ **On Unix**

```
$ sudo add-apt-repository ppa:cwchien/gradle
$ sudo apt-get update
$ sudo apt-get install gradle
```


### Build Java code

Now we are behind few step.

+ `cd HelloWorld` and run `gradle init`.

+ After finished init you can see new file and directory are created.

+ Open `build.gradle` file and add this line `apply plugin: 'java'`.

+ Now Run this command `gradle build`.

    To see the results of the build effort, take a look in the build folder. Therein you’ll find several directories, including these three notable folders:

    + classes. The project’s compiled .class files.
    + libs. Assembled project libraries (usually JAR and/or WAR files).


+ To make this code runnable, we can use gradle’s application plugin. Add this to your `build.gradle` file.Now Open `build.gradle` file and add this two line
 ```
apply plugin: 'application'
mainClassName = 'hello.HelloWorld'
```

+ We are almost done just run this command `gradle run`.
  Now you can see the output

```
  :compileJava UP-TO-DATE
  :processResources UP-TO-DATE
  :classes UP-TO-DATE
  :run

  Hello world!

  BUILD SUCCESSFUL
  Total time: 4.009 secs
```

Yes we have done .... :)

### Declare dependencies

The simple Hello World sample is completely self-contained and does not depend on any additional libraries. Most applications, however, depend on external libraries to handle common and/or complex functionality.

For example, suppose that in addition to saying "Hello World!", you want the application to print the current date and time. You could use the date and time facilities in the native Java libraries, but you can make things more interesting by using the Joda Time libraries.

+ First, change `HelloWorld.java` to look like this:
```
  package hello;

  import org.joda.time.LocalTime;

  public class HelloWorld {
    public static void main(String[] args) {
      LocalTime currentTime = new LocalTime();
      System.out.println("The current local time is: " + currentTime);

      Greeter greeter = new Greeter();
      System.out.println(greeter.sayHello());
    }
  }
```

+ Here `HelloWorld` uses Joda Time’s `LocalTime` class to get and print the current time.

+ If you ran `gradle build` to build the project now, the build would fail because you have not declared Joda Time as a compile dependency in the build.

+ For starters, you need to add a source for 3rd party libraries in your `build.gradle` file.
```
    repositories {
      mavenCentral()
    }
```
  
+ The `repositories` block indicates that the build should resolve its dependencies from the Maven Central repository.

+ Now that we’re ready for 3rd party libraries, let’s declare some in your `build.gradle` file.
```
  sourceCompatibility = 1.8
  targetCompatibility = 1.8

  dependencies {
      compile "joda-time:joda-time:2.2"
  }
```

  With the `dependencies` block, you declare a single dependency for Joda Time. Specifically, you’re asking for (reading right to left) version 2.2 of the joda-time library, in the joda-time group.

  Another thing to note about this dependency is that it is a `compile` dependency, indicating that it should be available during compile-time (and if you were building a WAR file, included in the /WEB-INF/libs folder of the WAR). Other notable types of dependencies include:
    + `providedCompile`. Required dependencies for compiling the project code, but that will be provided at runtime by a container running the code (for example, the Java Servlet API).
    + `testCompile`. Dependencies used for compiling and running tests, but not required for building or running the project’s runtime code.


+ Finally, let’s specify the name for our JAR artifact ( optional).
```
  jar {
      baseName = 'hello-world-gradle'
      version =  '0.1.0'
    }
```
  
The `jar` block specifies how the JAR file will be named. In this case, it will render `jb-hello-world-gradle-0.1.0.jar`.

Now if you run `gradle build`, Gradle should resolve the Joda Time dependency from the Maven Central repository and the build will succeed.


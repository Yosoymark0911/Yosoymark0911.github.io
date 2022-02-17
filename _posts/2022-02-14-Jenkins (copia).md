---

typora-copy-images-to: ../assets/img/
typora-root-url: ../
layout: post
categories: 
conToc: true
title: Heroku
---

# Práctica Heroku

Primero de todo deberemos crear la siguiente estructura:

```bash
Maven
├── pom.xml
└── src
    └── main
        └── java
            └── hello
                ├── Greeter.java
                └── HelloWorld.java

```

Ahora, deberemos editar el fichero **HelloWorld.java**

```java
package hello;

public class HelloWorld {
  public static void main(String[] args) {
    Greeter greeter = new Greeter();
    System.out.println(greeter.sayHello());
  }
}
```

Y el fichero **Greeter.java**

```java
package hello;

public class Greeter {
  public String sayHello() {
    return "Hello world!";
  }
}
```

## Construir código java

Para la construcción de nuestro código de java, deberemos editar el fichero **pom.xml **de la siguiente manera:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-maven</artifactId>
    <packaging>jar</packaging>
    <version>0.1.0</version>

    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>
    <dependencies>
		<dependency>
			<groupId>joda-time</groupId>
			<artifactId>joda-time</artifactId>
			<version>2.9.2</version>
		</dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.hamcrest</groupId>
            <artifactId>hamcrest-all</artifactId>
            <version>1.3</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.2.4</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>hello.HelloWorld</mainClass>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

Ahora ejecutamos la orden para la compilación

```
mvn compile
```

Y a continuación compilamos el paquete con código Java

```
mvn package
```

Ahora deberemos modificar el contenido de **HelloWorld.java**

```java
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

Ahora realizaremos una prueba, para ello, deberemos crear un nuevo directorio:

```
Maven
├── pom.xml
└── src
    ├── main
    │   └── java
    │       └── hello
    │           ├── Greeter.java
    │           └── HelloWorld.java
    └── test
        └── java
            └── hello
                └── GreeterTest.java
```

Y el fichero **GreeterTest.java**, lo deberemos rellenar con la siguiente información:

```java
package hello;

import static org.hamcrest.CoreMatchers.containsString;
import static org.junit.Assert.*;

import org.junit.Test;

public class GreeterTest {
  
  private Greeter greeter = new Greeter();

  @Test
  public void greeterSaysHello() {
    assertThat(greeter.sayHello(), containsString("Hello"));
  }

}
```

Y ahora haremos la prueba a ver si lo hemos hecho todo correcto:

```bash
mvn test
```

![image-20220213130551888](/assets/img/image-20220213130551888.png)

# First steps

In this book we will learn the principal techniques involved in developing 3D games. 
We will develop our samples in Java using the Lightweight Java Game Library \([LWJGL](http://www.lwjgl.org/)\). 
The LWJGL library enables access to low-level APIs \(Application Programming Interface\) such as OpenGL.

## LWJGL Overview
LWJGL is a low level API that acts like a wrapper around OpenGL. 
If your idea is to start creating 3D games in a short amount of time, you should probably consider alternatives like \[JmonkeyEngine\]. 
By using this low level API you will have to go through many concepts and write a lot of lines of code before you see the results. 
The benefit of doing it this way however is that you will get a much better understanding of 3D graphics and also have much more control.

## Getting Setup
As mentioned previously we will be using Java throughout this book, so you need to download the JDK \(Java Development Kit\) from Oracle’s pages. 
Note: We will be targeting and using features so need version at least Java 10.
Just choose the installer that suits your Operating System and install it. 
This book assumes that you have a moderate understanding of the Java language.

### Code Editor
You may want to use a code editor, generally called an IDE \(Integrated Development Enviroment\).
The accompanying code samples for each chapter have project files for an IDE developed by JetBrains, called IntelliJ IDEA, IDE.
JetBrains provides a free open source version, the IntelliJ Community Edition \(CE\), which you can download from here: [https://www.jetbrains.com/idea/download/](https://www.jetbrains.com/idea/download/ "Intellij").
Remember to download the 64 bits version of IntelliJ.

![](/chapter01/intellij.png)

### Maven
For building our samples we will be using [Maven](https://maven.apache.org/). 
Maven is already integrated in most IDEs and you can directly open the different samples inside them. 
Just open the folder that contains the chapter sample and IntelliJ will detect that it is a maven project.

![](/chapter01/maven_project.png)

Maven builds projects based on an XML file named `pom.xml` \(Project Object Model\) which manages project dependencies 
\(the libraries you need to use\) and the steps to be performed during the build process. 
Maven follows the principle of favouring convention over configuration, that is, there is a standard project structure and naming conventions. 
By following the conventions you won't need to explicitly say where source files are or where compiled classes should be located.

This book does not intend to be a maven tutorial, so please find more information about it on the web in case you need it.
The source code folder defines a parent project which defines the plugins to be used and collects the versions of the libraries employed.

## Modular LWJGL
LWJGL 3.1 introduced some changes to the way the project is build. The project is now split into several modules. 
This means that rather using one giant monolithic jar file with all possible dependencies that we don't need, we can be more selective and only package we need for our project. 
This does come at a cost: You now need to choose specifically what you need dependencies you need one by one. 

### LWJGL Dependencies
The [LWJGL download](https://www.lwjgl.org/download) page includes a fancy script that allows you to choose which dependencies you need and generate the pom file for you. 
In our case, we will just be using GLFW and OpenGL bindings. You can check what the pom file looks like in the source code.

### LWJGL Native Libraries
The LWJGL platform dependency takes care of unpacking native libraries that are needed to talk to the hardware for your platform, so there's no need to use other plugins \(such as `mavennatives`\). 
We do however have to decide which dependency to use. We do this by creating three profiles, the appropriate one being automatically based on the platform you are building on. The value associated with each profile is the corresponding native package for either Windows, Linux or Mac.

```xml
    <profiles>
        <profile>
            <id>windows-profile</id>
            <activation>
                <os>
                    <family>Windows</family>
                </os>
            </activation>
            <properties>
                <native.target>natives-windows</native.target>
            </properties>                
        </profile>
        <profile>
            <id>linux-profile</id>
            <activation>
                <os>
                    <family>Linux</family>
                </os>
            </activation>
            <properties>
                <native.target>natives-linux</native.target>
            </properties>                
        </profile>
        <profile>
            <id>OSX-profile</id>
            <activation>
                <os>
                    <family>mac</family>
                </os>
            </activation>
            <properties>
                <native.target>natives-osx</native.target>
            </properties>
        </profile>
    </profiles>
```

Inside each project, the LWJGL platform dependency will use the correct property established in the profile for the current platform.

```xml
        <dependency>
            <groupId>org.lwjgl</groupId>
            <artifactId>lwjgl-platform</artifactId>
            <version>${lwjgl.version}</version>
            <classifier>${native.target}</classifier>
        </dependency>
```

Every project generates a runnable jar \(one that can be executed by typing java -jar name\_of\_the\_jar.jar\). 
This is generated by the maven-jar-plugin which creates a jar with a `MANIFEST.MF` file with the correct values. 
The most important attribute for that file is `Main-Class`, which sets the entry point for the program. 
All the dependencies are included as entries in the `Class-Path` attribute for that file. 
In order to execute the programme on another computer, you just need to copy the main jar file and the lib directory \(with all the jars included there\) which are located under the target directory.

The jars that contain LWJGL classes, also contain the native libraries. 
LWJGL will also take care of extracting them and adding them to the path where the JVM will look for libraries.

## Chapter 1
Chapter 1 source code is taken directly from the `Getting Started` sample in the LWJGL site \([http://www.lwjgl.org/guide](http://www.lwjgl.org/guide)\). 
You will see that we are not using Swing or JavaFX as our GUI library. 
Instead of that we are using [GLFW](www.glfw.org) which is a library to handle GUI components \(Windows, etc.\) and events \(key presses, mouse movements, etc.\) with an OpenGL context attached in a straight-forward way. 
Previous versions of LWJGL provided a custom GUI API but, for LWJGL 3, GLFW is the preferred windowing API.

The samples source code is very well documented and straightforward so we won’t repeat the comments here.

If you have your environment correctly set up you should be able to execute it and see a window with a red background.

![Hello World](hello_world.png)

**The source code of this book is published in **[**GitHub**](https://github.com/lwjglgamedev/lwjglbook)**.**


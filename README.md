# sample-maven-project

Step 1: Install Jenkins
```
#!/bin/bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt install fontconfig openjdk-17-jre -y
sudo apt update
sudo apt-get install jenkins -y

```
Go to the Jenkins download page.
Choose your operating system and follow the specific installation steps.
Start Jenkins:

After installation, start Jenkins:
Linux: Run 
```
sudo systemctl start jenkins.
```
Windows: Jenkins will typically run as a Windows service after installation.
Access Jenkins:
Open a web browser and go to http://localhost:8080.
Enter the initial admin password, which is found in the Jenkins installation directory, to complete the setup.

Step 2: Install Maven
Download Maven:
```
apt install maven
```
Download the latest version of Maven and follow the instructions to install it on your system.
Verify Installation:
Open a terminal (or command prompt) and type:
```
mvn -version
```
This should display the Maven version, Java version, and other environment information.

Step 3: Configure Jenkins for Maven
Open Jenkins and Configure Tools:
Go to Manage Jenkins > Global Tool Configuration.
Add Maven:
Scroll to the Maven section.
Click Add Maven and set a name (e.g., Maven3.6).
If Maven is not installed on the Jenkins server, you can check the option Install automatically to let Jenkins manage it.
Add Java (if not already configured):
Scroll to the JDK section and configure the Java Development Kit (JDK) similarly.

Step 4: Set Up a Sample Maven Project
To create a simple Maven project, you need to initialize a Maven project structure and configure necessary files:
Create Project Directory:
Open a terminal and create a new directory:
```
mkdir sample-maven-project
cd sample-maven-project
```
Initialize Maven Project:
Run the following command to create a basic Maven project structure:
```
mvn archetype:generate -DgroupId=com.example -DartifactId=sample-maven-project -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
This command will create the necessary files and folders, including pom.xml (Maven configuration file) and basic Java source files.
```
File Structure:
After running the above command, the file structure should look like this:
```
sample-maven-project
├── pom.xml
└── src
    ├── main
    │   └── java
    │       └── com
    │           └── example
    │               └── App.java
    └── test
        └── java
            └── com
                └── example
                    └── AppTest.java
```
Edit App.java (optional):
Open App.java in a text editor and add a simple main method if it doesn’t have one:
```
package com.example;

public class App {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```
Edit AppTest.java (optional):
Make sure AppTest.java includes a simple test:
```
package com.example;

import static org.junit.Assert.assertTrue;
import org.junit.Test;

public class AppTest {
    @Test
    public void shouldAnswerWithTrue() {
        assertTrue(true);
    }
}
```

Step 5: Upload Project to GitHub
Initialize Git:
```
git init
git add .
git commit -m "Initial commit"
```
Create a GitHub Repository:
Go to GitHub, create a new repository, and follow the instructions to push your code:
```
git remote add origin https://github.com/your-username/sample-maven-project.git
git push -u origin main
```

Step 6: Create a Freestyle Project in Jenkins
Create a New Project:
In Jenkins, click New Item.
Enter a name for your project (e.g., SampleMavenProject).
Select Freestyle project and click OK.

Configure Source Code Management:
In the project configuration page, scroll to Source Code Management and select Git.
Enter the GitHub repository URL.
Add credentials if required (such as a GitHub token).

Step 7: Configure Build with Maven
Add a Build Step:
Scroll down to the Build section and click Add Build Step.
Select Invoke Top-Level Maven Targets.
For Goals, enter clean install (this will clean and compile your project).
Specify the POM File:

Sample pom.xml file
```

<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- Project metadata -->
    <groupId>com.example</groupId>
    <artifactId>sample-maven-project</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <!-- Project properties -->
    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <!-- Dependencies -->
    <dependencies>
        <!-- JUnit for testing -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <!-- Build settings -->
    <build>
        <plugins>
            <!-- Compiler plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>${maven.compiler.source}</source>
                    <target>${maven.compiler.target}</target>
                </configuration>
            </plugin>

            <!-- Surefire plugin for running tests -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.2</version>
            </plugin>
        </plugins>
    </build>
</project>
```

If your pom.xml file is in the root directory, leave this field blank. If it's in a subdirectory, specify the path (e.g., /var/lib/jenkins/workspace/Project-1/sample-maven-project/pom.xml/pom.xml).

Step 8: Save and Build the Project
Save the Configuration:
Click Save to save the project configuration.

Run the Build:
On the project page, click Build Now.
View the Console Output:
Go to Build History on the left panel, select the build, and click Console Output to view detailed logs of the build process.

Notes:
1. Use Ubuntu server version Ubuntu Server 22.04 LTS (HVM),EBS General Purpose (SSD)
2. Allow "All TCP" traffic in security group inbound rule.

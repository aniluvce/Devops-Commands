### To install Nexus on Linux, follow these steps:

1. Prerequisites:
   - Ensure that Java Development Kit (JDK) is installed on your Linux system. Nexus requires Java to run. You can check if Java is installed by running the command `java -version` in the terminal. If Java is not installed, you can install it using the package manager of your Linux distribution.

2. Download Nexus:
   - Visit the Sonatype website and go to the Nexus download page: https://www.sonatype.com/nexus/repository-oss-download
   - Download the latest version of Nexus Repository Manager OSS (Open Source Edition). Look for the "Unix/Linux" distribution package in the list.
   - Alternatively, you can use the following command in the terminal to download Nexus:
     ```
     wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
     ```

3. Extract Nexus:
   - Open a terminal and navigate to the directory where the downloaded Nexus package is located.
   - Extract the package using the following command:
     ```
     tar -xf latest-unix.tar.gz
     ```

4. Configure Nexus:
   - Open the extracted Nexus directory.
   - In the `bin` folder, you will find a script called `nexus` (or `nexus.exe` for Windows). This is the Nexus startup script.
   - Make the script executable by running the following command:
     ```
     chmod +x nexus
     ```

5. Start Nexus:
   - Start Nexus by running the following command in the terminal:
     ```
     ./nexus start
     ```

6. Access Nexus Web UI:
   - Open a web browser and navigate to `http://localhost:8081`. This is the default URL for accessing Nexus.
   - The first time you access Nexus, you will be prompted to set up the administrator credentials. Follow the on-screen instructions to create an admin user.

That's it! Nexus is now installed and running on your Linux system. You can proceed to configure Nexus, create repositories, and manage artifacts using the Nexus Web UI.

## Install Nexus using Docker

```
docker run -d --name nexus -p 8081:8081 sonatype/nexus3
```
In your POM file, you can include the following information :

```xml
<distributionManagement>
  <repository>
    <id>maven-releases</id>
    <!-- CHANGE HERE by your team Nexus server -->
    <url>http://13.126.124.159:8081/repository/maven-releases/</url>
  </repository>
  <snapshotRepository>
    <id>maven-snapshots</id>
    <!-- CHANGE HERE by your team Nexus server -->
    <url>http://13.126.124.159:8081/repository/maven-releases/</url>
  </snapshotRepository>
</distributionManagement>
```

## & 

```xml
<repository>
  <id>maven-releases</id>
  <name>maven-releases</name>
  <url>http://13.126.124.159:8081/repository/maven-releases/</url>
</repository>
```

Remember to replace the Nexus server URL (`http://13.126.124.159:8081/repository/maven-releases/`) with your team's actual Nexus server URL.

## For Nexus Auth use settings.xml file as shown in video

To authenticate with Nexus using a `settings.xml` file, you can include the following configuration in the file:

```xml
<settings>
  <servers>
    <server>
      <id>nexus-releases</id>
      <username>your-username</username>
      <password>your-password</password>
    </server>
    <server>
      <id>nexus-snapshots</id>
      <username>your-username</username>
      <password>your-password</password>
    </server>
  </servers>
</settings>
```

In the above configuration, replace `your-username` with your Nexus username and `your-password` with your Nexus password. 

Make sure to include this `settings.xml` file in the appropriate location on your system.

### After this just need to run commands for deploy as below

#### In case of maven on linux
```
mvn clean deploy
```

#### In case of Jenkins Pipeline
```
configFileProvider([configFile(fileId: '1c322f97-3d77-4302-abe0-7dd0d866eab0', variable: 'mavensettings')]) {
                  
                  sh "mvn -s $mavensettings clean deploy -DskipTests=true"
                  
                }
```

### Once the pipeline is success you will get the results in Nexus as below.

![alt text](https://github.com/jaiswaladi246/30-Days-Of-DevOps/blob/main/Images/4.png?raw=true)

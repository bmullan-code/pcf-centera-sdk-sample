# pcf-centera-sdk-sample
pcf-centera-sdk-sample

Instructions to push a java spring boot app that uses the EMC CAS Centera sdk

1. Download the Centera SDK (select the linux gcc4 version)
https://community.emc.com/docs/DOC-56344 

2. Extract the file into a directory. 
eg.
```
mkdir tmp
cd tmp
tar -zxvf ../Centera_SDK_Linux-gcc4.tgz
```

3. Copy the sdk lib directory to your project directory.
eg. 
```
mkdir <project-dir>/src/main/resources/Centera_SDK
cp -r Centera_SDK/lib <project-dir>/src/main/resources/Centera_SDK
```

4. Install the Centera FPLibary.jar as a local maven dependency

```
mvn install:install-file -Dfile=<project-dir>/src/main/resources/Centera_SDK/lib/FPLibrary.jar -DgroupId=fplibrary.group -DartifactId=fplibrary -Dversion=1.0.0 -Dpackaging=jar -DgeneratePom=true
```

5. Add the FPLibary dependency to your project pom.xml

```
		<dependency>
			<groupId>fplibrary.group</groupId>
			<artifactId>fplibrary</artifactId>
			<version>1.0.0</version>
		</dependency>
```

6. Rebuild your maven project
```
mvn clean package
```

7. Set the LD_LIBRARY_PATH to point to the sdk lib directory (in PCF)
Add the following environment variable to your manifest.yml file

```
applications:
- name: app 
...
...
  env:
    LD_LIBARY_PATH: /home/vcap/app/BOOT-INF/classes/Centera_SDK/lib
```

8. Push to PCF/TAS

```
cf push
```

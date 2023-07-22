<img src="https://companieslogo.com/img/orig/MDB_BIG-ad812c6c.png?t=1648915248" width="50%" title="Github_Logo"/> <br>

# MongoDB Atlas Handson Workshop for LGU Plus IPTV


### [&rarr; Spring Framework Project](#Project)
### [&rarr; Spring & MongoDB Driver](#MongoTemplates)

<br>

### Project
일반 Java Project로 생성하거나 Springframework 프로젝트를 사용 할 수 있습니다.       
(Spring Framework를 이용하는 경우 Spring Data MongoDB를 선택 할 수 있습니다.)

<img src="/01.SpringBoot/images/image02.png" width="90%" height="90%">   

SpringBoot에서 MongoDB를 이용한 개발 방법은 4 가지로 구분이 가능 합니다.

MongoDB Driver Only : Spring Framework 와 Spring Data를 이용하지 않는 개발 방법   
Spring Framework & MongoDB : Spring Framework를 이용하지만 Spring Data를 이용하지 않는 방법   
Spring Framework & Mongo Template : Spring Framework 와 Template를 이용한 Spring Data 활용    
Spring Framework & Spring Data : Spring Framework 와 Spring Data를 이용한 방법 

### Driver

MongoDB Driver만을 사용하여 개발이 단순한 장점이 있으나 Spring Framewok가 제공 되지 않아 해당 API를 사용하지 못하는 단점이 있습니다.    
Dependency를 POM에 추가 하여 줍니다.  

```` pom.xml
		<dependency>
      <groupId>org.mongodb</groupId>
      <artifactId>mongodb-driver-sync</artifactId>
    </dependency>
````

MongoClient를 생성 하여 Connection 한 후 Database, Collection을 선택 한 후 쿼리를 진행 합니다.


#### MongoTemplates

Spring Framework이 제공하는 Mongo Templates를 이용하여 개발 하게 됩니다.   

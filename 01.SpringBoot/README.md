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
Repository가 제공하는 기본 CRUD등을 사용하지 않기 때문에 일이 구현해야 합니다. 

Dependency를 추가 하여 줍니다.

```` pom.xml		
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-mongodb</artifactId>
		</dependency>
````

또한 application.properties 에 Atlas 접근 정보를 추가 하여야 합니다.


```` application.properties
spring.data.mongodb.uri=mongodb+srv://<<username>>:<<password>>@<<atlas address>>.mongodb.net
spring.data.mongodb.database=<<database>>
````

데이터는 model을 생성하고 MongoTemplate 객체를 이용하여 CRUD를 처리합니다. 

````

@Autowired
	private MongoTemplate mongoTemplate;

  ...

  User user = new User();
	user.setAge(50);
	user.setEmail("gildong.hong@email.com");
	user.setName("Gildong Hong");
			
	mongoTemplate.insert(user);

````

Update 처리
````

  @Autowired
	private MongoTemplate mongoTemplate;

  ...

  Query query = new Query();
  query.addCriteria(Criteria.where("ssn").is("123-456-7890"));
  query.addCriteria(Criteria.where("addresses.type").is("home"));
  
  Update update = new Update();
  update.set("addresses.$.postcode", "06230");
  
  UpdateResult rs = mongoTemplate.updateFirst(query, update, User.class);
  
  System.out.println("Update Result : "+ rs);

````


#### Repository

Spring Framework이 제공하는 Respository를 사용 하는 것으로 기본 CRUD외에 필요한 메서드를 repository interface에 작성하고 implementation 에 구현해 주어야 한다.   

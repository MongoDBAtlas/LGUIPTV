<img src="https://companieslogo.com/img/orig/MDB_BIG-ad812c6c.png?t=1648915248" width="50%" title="Github_Logo"/> <br>

# MongoDB Atlas Handson Workshop for LGU Plus IPTV


### [&rarr; Cluster Provision](#Provision)

### [&rarr; CRUD with Java](#Java)

### [&rarr; CRUD with Mongosh](#MONGOSH)

### [&rarr; Compass 를 이용한 데이터 확인](#Compass)

### [&rarr; 추가 Query](#option)

<br>

### Provision
Account에 나온 바와 같이 Atlas database를 배포 하여 줍니다.
- [Prew-Work](/01.account/README.md)


### Java

Github에서 소스를 다운 로드 한 후 02.Provision and CRUD/application 폴더로 이동 하여 줍니다. (Java  애플리케이션)
Maven을 이용한 프로젝트로 필요한 패키지는 pom.xml에 등록 되어 있습니다.

실행을 위해 Connection 관련 정보를 입력 하여야 합니다.  
AtlasSpringDriverApplication 클래스에 connectionString을 사용하고 있는 Atlas의 접속 주소로 변경 하여 줍니다.  

````
private String connectionString = "mongodb+srv://<<user>>:<<password>>@<<Atlas>>.mongodb.net";
````

#### Insert Test

MongoDB Atlas 와 연결하여 데이터를 생성 합니다.

실행을 위해 AtlasSpringDriverApplication 클래스에 ApplicationRunner 메서드에 실행할 메서드(insertNewUser())를 지정 하여 합니다.

실제 실행될 함수 확인을 위해 insertNewUser 함수로 이동 합니다.    

```` insertNewUser()
      List<Document> _addresses = new ArrayList<Document> ();
			_addresses.add(new Document("type", "office").append("address","서울시 강남구 삼성동").append("detail", "코엑스 6"));
			_addresses.add(new Document("type", "home").append("address","서울시 강남구 역삼동").append("detail", "역삼 한국 아파트 101동 101호"));
			
			List<String> _hobbies = new ArrayList<String> ();
			_hobbies.add("Martial arts");
			
			Document _user = new Document();
			_user.append("ssn", "123-456-7890")
			.append("name", "Gildong Hong")
			.append("email", "gildong@email.com")
			.append("DateOfBirth", "25th Dec")
			.append("Hobbies", _hobbies)
			.append("Addresses", _addresses);

````

AtlasSpringDriverApplication을 실행 하여 줍니다.  

<img src="/02.Provision and CRUD/images/image51.png" width="90%" height="90%">   

Query로 데이터를 확인 합니다.

````
Atlas handson> db.Users.find()
[
  {
    _id: ObjectId("64bbe8460f740053372d75e3"),
    ssn: '123-456-7890',
    name: 'Gildong Hong',
    email: 'gildong@email.com',
    DateOfBirth: '25th Dec',
    Hobbies: [ 'Martial arts' ],
    Addresses: [
      { type: 'office', address: '서울시 강남구 삼성동', detail: '코엑스 6' },
      {
        type: 'home',
        address: '서울시 강남구 역삼동',
        detail: '역삼 한국 아파트 101동 101호'
      }
    ]
  }
]
````

#### find Test

MongoDB Atlas 와 연결하여 데이터를 조회 합니다.

실행을 위해 AtlasSpringDriverApplication 클래스에 ApplicationRunner 메서드에 실행할 메서드(findUsers())를 지정 하여 합니다.

````
return args -> {
			//insertNewUser();
			findUsers("123-456-7890");
			//updateUserHobby("123-456-7890","Reading");
			//updateUserAddresses("123-456-7890","home", "06230");
			//deleteUser("123-456-7890");
			
		};
````


실제 실행될 함수 확인을 위해 findUsers 함수로 이동 합니다.   

````
Bson filter = eq("ssn", ssn);
			
List<Document> iterDoc = getMongoDatabase().getCollection("Users").find(filter).projection(fields(excludeId(),include("ssn","name","email"))).into(new ArrayList<Document>());
````

AtlasSpringDriverApplication을 실행 하여 줍니다.  

````
 :: Spring Boot ::                (v3.1.2)

2023-07-22T23:35:20.637+09:00  INFO 14366 --- [           main] c.m.spring.AtlasSpringDriverApplication  : Starting AtlasSpringDriverApplication using Java 17.0.4.1 with PID 14366 (/Users/kyudong.kim/works/spring/AtlasSpringDriver/target/classes started by kyudong.kim in /Users/kyudong.kim/works/spring/AtlasSpringDriver)
...
AtlasSpringDriverApplication  : Started AtlasSpringDriverApplication in 1.113 seconds (process running for 1.594)
...
Success Find Document : {"ssn": "123-456-7890", "name": "Gildong Hong", "email": "gildong@email.com"}

````

#### Update Hobbies Test

MongoDB Atlas 와 연결하여 데이터를 조회 합니다.

실행을 위해 AtlasSpringDriverApplication 클래스에 ApplicationRunner 메서드에 실행할 메서드(updateUserHobby())를 지정 하여 합니다.

Hobbies 필드에 Reading 을 추가 하여 줍니다.   

````
return args -> {
			//insertNewUser();
			//findUsers("123-456-7890");
			updateUserHobby("123-456-7890","Reading");
			//updateUserAddresses("123-456-7890","home", "06230");
			//deleteUser("123-456-7890");
			
		};
````

실제 실행될 함수 확인을 위해 updateUserHobby 함수로 이동 합니다.   

````
Document query = new Document().append("ssn",  ssn);
			
Bson updates = Updates.combine(
              Updates.push("Hobbies", hobby));
      
MongoCollection<Document> _collection = getMongoDatabase().getCollection("Users");
UpdateResult result = _collection.updateOne(query, updates);

````


AtlasSpringDriverApplication을 실행 하여 줍니다.  

````
 :: Spring Boot ::                (v3.1.2)

2023-07-22T23:40:47.408+09:00  INFO 14772 --- [           main] c.m.spring.AtlasSpringDriverApplication  : Starting AtlasSpringDriverApplication using Java 17.0.4.1 with PID 14772 (/Users/kyudong.kim/works/spring/AtlasSpringDriver/target/classes started by kyudong.kim in /Users/kyudong.kim/works/spring/AtlasSpringDriver)
...
2023-07-22T23:40:48.150+09:00  INFO 14772 --- [gex.mongodb.net] org.mongodb.driver.cluster               : SRV resolution completed with hosts: [ac-f8e5gwu-lb.9g4ngex.mongodb.net:27017]
Update Result : AcknowledgedUpdateResult{matchedCount=1, modifiedCount=1, upsertedId=null}

````

Query로 데이터를 확인 합니다.

````
Atlas handson> db.Users.find()
[
  {
    _id: ObjectId("64bbe8460f740053372d75e3"),
    ssn: '123-456-7890',
    name: 'Gildong Hong',
    email: 'gildong@email.com',
    DateOfBirth: '25th Dec',
    Hobbies: [ 'Martial arts', 'Reading' ],
    Addresses: [
      { type: 'office', address: '서울시 강남구 삼성동', detail: '코엑스 6' },
      {
        type: 'home',
        address: '서울시 강남구 역삼동',
        detail: '역삼 한국 아파트 101동 101호'
      }
    ]
  }
]
````

#### Update Address Test


MongoDB Atlas 와 연결하여 데이터를 조회 합니다.

실행을 위해 AtlasSpringDriverApplication 클래스에 ApplicationRunner 메서드에 실행할 메서드(updateUserAddresses())를 지정 하여 합니다.

Addresses 내용 중 type이 home인 것의 우편번호를 06230으로 수정 합니다.  

````
return args -> {
			//insertNewUser();
			//findUsers("123-456-7890");
			//updateUserHobby("123-456-7890","Reading");
			updateUserAddresses("123-456-7890","home", "06230");
			//deleteUser("123-456-7890");
			
		};
````

실제 실행될 함수 확인을 위해 updateUserAddresses 함수로 이동 합니다.   

````
Document query = new Document().append("ssn",  ssn).append("Addresses.type",  type);
			
Bson updates = Updates.combine(
              Updates.set("Addresses.$.Zip", zipcode)  );
      
MongoCollection<Document> _collection = getMongoDatabase().getCollection("Users");
UpdateResult result = _collection.updateOne(query, updates);

````


AtlasSpringDriverApplication을 실행 하여 줍니다.  

````
 :: Spring Boot ::                (v3.1.2)
...
Update Result : AcknowledgedUpdateResult{matchedCount=1, modifiedCount=1, upsertedId=null}

````

Query로 데이터를 확인 합니다.

````
Atlas handson> db.Users.find()
[
  {
    _id: ObjectId("64bbe8460f740053372d75e3"),
    ssn: '123-456-7890',
    name: 'Gildong Hong',
    email: 'gildong@email.com',
    DateOfBirth: '25th Dec',
    Hobbies: [ 'Martial arts', 'Reading' ],
    Addresses: [
      { type: 'office', address: '서울시 강남구 삼성동', detail: '코엑스 6' },
      {
        type: 'home',
        address: '서울시 강남구 역삼동',
        detail: '역삼 한국 아파트 101동 101호',
        Zip: '06230'
      }
    ]
  }
]
````

#### Remove Test

MongoDB Atlas 와 연결하여 데이터를 삭제 합니다.

실행을 위해 AtlasSpringDriverApplication 클래스에 ApplicationRunner 메서드에 실행할 메서드(deleteUser())를 지정 하여 합니다.


````
return args -> {
			//insertNewUser();
			//findUsers("123-456-7890");
			//updateUserHobby("123-456-7890","Reading");
			//updateUserAddresses("123-456-7890","home", "06230");
			deleteUser("123-456-7890");
			
		};
````

실제 실행될 함수 확인을 위해 deleteUser 함수로 이동 합니다.   

````
Bson filter = eq("ssn", ssn);
DeleteResult rs =  getMongoDatabase().getCollection("Users").deleteOne(filter);

````


AtlasSpringDriverApplication을 실행 하여 줍니다.  

````
  :: Spring Boot ::                (v3.1.2)
...
Delete Result : AcknowledgedDeleteResult{deletedCount=1}
````

Query로 데이터를 확인 합니다.

````
Atlas handson> db.Users.find()


````

### MONGOSH

Mongosh로 Atlas 에 접속 하고 MongoDB Query 를 이용하여 데이터를 생성, 조회, 삭제를 테스트 합니다. NodeJS에 익숙하지 않은 경우 이를 이용하여 테스트 합니다.


#### Connection

MongoDB Atlas 와 Mongosh을 이용하여 연결 합니다.    
MongoDB atlas Mongosh 접근 주소를 얻어야 합니다. 
접속 주소를 얻기 위해 Console에 로그인합니다. 
데이터베이스 클러스터의 Connect 버튼을 클릭 합니다.

<img src="/02.Provision and CRUD/images/image01.png" width="90%" height="90%">     


접근방법을 선택 하여 주는 단계에서 Shell을 선택 하면 접근 주소를 얻을 수 있습니다.   

<img src="/02.Provision and CRUD/images/image20.png" width="60%" height="60%">   

Mongosh이 설치 되어 있음으로 I have the MongoDB Shell installed를 선택하고 계정 접근은 암호로 접근할 것임으로 Password를 선택하면 접근 할 수 있는 주소를 얻을 수 있습니다.    

<img src="/02.Provision and CRUD/images/image21.png" width="70%" height="70%">     


Terminal을 열고 해당 주소를 이용하여 mongosh를 실행 하여 줍니다. (접근하기 위한 Account로 입력 하여 줍니다.)

````
 % mongosh "mongodb+srv://cluster0.5qjlg.mongodb.net/myFirstDatabase" --apiVersion 1 --username <<user account>>    
Enter password: **********
Current Mongosh Log ID:	64454459813babb209a83f4c
Connecting to:		mongodb+srv://cluster0.5qjlg.mongodb.net/myFirstDatabase
Using MongoDB:		6.0.5 (API Version 1)
Using Mongosh:		1.0.5

For mongosh info see: https://docs.mongodb.com/mongodb-shell/

Atlas atlas-t0pzlo-shard-0 [primary] myFirstDatabase> 
````

#### Insert Test

Mongosh을 이용하여 Atlas와 연결하여 데이터를 생성 합니다.

먼저 데이터베이스를 선택하여야 합니다.
````
Atlas atlas-t0pzlo-shard-0 [primary] myFirstDatabase> use samsung
switched to db samsung
Atlas atlas-t0pzlo-shard-0 [primary] samsung>
````

입력할 데이터를 생성하여 줍니다. (변수로 newUser를 만들어 줍니다)

````
Atlas atlas-t0pzlo-shard-0 [primary] samsung> let newUser=  {
        ssn:"123-456-0001", 
        email:"user@email.com", 
        name:"Gildong Hong", 
        DateOfBirth: "1st Jan.", 
        Hobbies:["Martial arts"],
        Addresses:[{"Address Name":"Work","Street":"431, Teheran-ro GangNam-gu ","City":"Seoul", "Zip":"06159"}], 
        Phones:[{"type":"mobile","number":"010-5555-1234"}]
      };
````

다음 데이터 베이스 명령으로 데이터를 생성 합니다.

````
Atlas atlas-t0pzlo-shard-0 [primary] samsung> db.user.insertOne(newUser)
{
  acknowledged: true,
  insertedId: ObjectId("64454591813babb209a83f4d")
}

````
Atlas Console 에서 데이터 생성 여부를 확인 합니다.


#### find Test

Mongosh을 이용하여 Atlas와 연결하여 데이터를 조회 합니다.

먼저 데이터베이스를 선택하여야 합니다. (이미 해당 데이터베이스를 사용 하고 있으면 생략 합니다)
````
Atlas atlas-t0pzlo-shard-0 [primary] myFirstDatabase> use samsung
switched to db samsung
Atlas atlas-t0pzlo-shard-0 [primary] samsung>
````

데이터를 조회 합니다
````
Atlas atlas-t0pzlo-shard-0 [primary] samsung> db.user.find({ssn:"123-456-0001"})
[
  {
    _id: ObjectId("64454591813babb209a83f4d"),
    ssn: '123-456-0001',
    email: 'user@email.com',
    name: 'Gildong Hong',
    DateOfBirth: '1st Jan.',
    Hobbies: [ 'Martial arts' ],
    Addresses: [
      {
        'Address Name': 'Work',
        Street: '431, Teheran-ro GangNam-gu ',
        City: 'Seoul',
        Zip: '06159'
      }
    ],
    Phones: [ { type: 'mobile', number: '010-5555-1234' } ]
  }
]
````

#### Update Test

Mongosh을 이용하여 Atlas와 연결하여 데이터를 업데이트 합니다.

먼저 데이터베이스를 선택하여야 합니다. (이미 해당 데이터베이스를 사용 하고 있으면 생략 합니다)
````
Atlas atlas-t0pzlo-shard-0 [primary] myFirstDatabase> use samsung
switched to db samsung
Atlas atlas-t0pzlo-shard-0 [primary] samsung>
````

수정할 데이터를 ssn을 입력 하여 줍니다.
수정 대상 데이터의 ssn 및 수정할 데이터 항목을 확인 수정 하여 줍니다.
`````
Atlas atlas-t0pzlo-shard-0 [primary] samsung> let query = {"ssn":"123-456-0001"}

Atlas atlas-t0pzlo-shard-0 [primary] samsung> let updatedata = { $set: { email: "gildong@email.com" } }

Atlas atlas-t0pzlo-shard-0 [primary] samsung> db.user.updateOne(query,updatedata)
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
      
`````

데이터를 수정 결과를 확인 합니다. (이메일 주소가 수정 된 것을 확인 합니다)
````
Atlas atlas-t0pzlo-shard-0 [primary] samsung> db.user.find({"ssn":"123-456-0001"})
[
  {
    _id: ObjectId("64454591813babb209a83f4d"),
    ssn: '123-456-0001',
    email: 'gildong@email.com',
    name: 'Gildong Hong',
    DateOfBirth: '1st Jan.',
    Hobbies: [ 'Martial arts' ],
    Addresses: [
      {
        'Address Name': 'Work',
        Street: '431, Teheran-ro GangNam-gu ',
        City: 'Seoul',
        Zip: '06159'
      }
    ],
    Phones: [ { type: 'mobile', number: '010-5555-1234' } ]
  }
]
````

#### Update Hobbies Test

Mongosh을 이용하여 Atlas와 연결하여 데이터를 업데이트 (Hobbies를 추가)합니다.

먼저 데이터베이스를 선택하여야 합니다. (이미 해당 데이터베이스를 사용 하고 있으면 생략 합니다)
````
Atlas atlas-t0pzlo-shard-0 [primary] myFirstDatabase> use samsung
switched to db samsung
Atlas atlas-t0pzlo-shard-0 [primary] samsung>
````

수정할 데이터를 ssn을 입력 하여 줍니다.
수정 대상 데이터의 ssn 및 Hobby 항목을 추가 하여 줍니다. (취미로 Reading 추가 하기)
`````
Atlas atlas-t0pzlo-shard-0 [primary] samsung> let query = {"ssn":"123-456-0001"}

Atlas atlas-t0pzlo-shard-0 [primary] samsung> let updatedata ={$push:{Hobbies:"Reading"}}

Atlas atlas-t0pzlo-shard-0 [primary] samsung> db.user.updateOne(query,updatedata)
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
          
`````

데이터를 수정 결과를 확인 합니다. (Hobby에 Reading이 추가되어 있음)
````
Atlas atlas-t0pzlo-shard-0 [primary] samsung> db.user.find({"ssn":"123-456-0001"})
[
  {
    _id: ObjectId("64454591813babb209a83f4d"),
    ssn: '123-456-0001',
    email: 'gildong@email.com',
    name: 'Gildong Hong',
    DateOfBirth: '1st Jan.',
    Hobbies: [ 'Martial arts', 'Reading' ],
    Addresses: [
      {
        'Address Name': 'Work',
        Street: '431, Teheran-ro GangNam-gu ',
        City: 'Seoul',
        Zip: '06159'
      }
    ],
    Phones: [ { type: 'mobile', number: '010-5555-1234' } ]
  }
]

````

#### Remove Test

Mongosh을 이용하여 Atlas와 연결하여 데이터를 삭제 합니다.

먼저 데이터베이스를 선택하여야 합니다. (이미 해당 데이터베이스를 사용 하고 있으면 생략 합니다)
````
Atlas atlas-t0pzlo-shard-0 [primary] myFirstDatabase> use samsung
switched to db samsung
Atlas atlas-t0pzlo-shard-0 [primary] samsung>
````

삭제할 데이터를 수정 하여 줍니다.
삭제할 데이터의 ssn 및 입력 하여줍니다.
`````
Atlas atlas-t0pzlo-shard-0 [primary] samsung> db.user.deleteOne({ssn:"123-456-0001"})
{ acknowledged: true, deletedCount: 1 }

`````

데이터를 확인 합니다.
````
Atlas atlas-t0pzlo-shard-0 [primary] samsung> db.user.findOne({ssn:"123-456-0001"})
null
Atlas atlas-t0pzlo-shard-0 [primary] samsung> 
````


### Compass

MongoDB Cluster에 접속하여 저장된 데이터 등을 볼 수 있는 개발자용 GUI툴입니다. 이를 이용하여 데이터를 조회 하고 변경 하여 줍니다. 다음 링크에서 다운로드가 가능 합니다.    
Compass :   
https://www.mongodb.com/products/compass

테스트를 위해 다음 방법으로 데이터를 생성 하여 줍니다.
````
% node insertMany.js 
A document was inserted with the _id: 63e32381541c67cc69d78977
A document was inserted with the _id: 63e32381541c67cc69d78978
A document was inserted with the _id: 63e32381541c67cc69d78979
A document was inserted with the _id: 63e32381541c67cc69d7897a
...
````

Mongosh을 이용하는 경우 다음과 같이 생성 하여 줍니다.

````
Atlas atlas-t0pzlo-shard-0 [primary] samsung> let i=0

Atlas atlas-t0pzlo-shard-0 [primary] samsung> for (i;i<100;i++) {
const newUser = {
            ssn:"123-456-000"+i, 
            email:"user"+i+"@email.com", 
            name:"Gildong Hong "+i, 
            age: Math.floor(Math.random()*100),
            DateOfBirth: "1st Jan.", 
            Hobbies:["Martial arts"],
            Addresses:[{"Address Name":"Work","Street":"431, Teheran-ro GangNam-gu ","City":"Seoul", "Zip":"06159"}], 
            Phones:[{"type":"mobile","number":"010-5555-1234"}]
          };
db.user.insertOne(newUser);
}
{
  acknowledged: true,
  insertedId: ObjectId("64454b5e813babb209a83fb1")
}
````

데이터가 100건이 생성이 되게 됩니다.


#### Connection
MongoDB atlas Console에 접근 주소를 얻어야 합니다. 
접속 주소를 얻기 위해 Console에 로그인합니다.    
데이터베이스 클러스터의 Connect 버튼을 클릭 합니다.

<img src="/02.Provision and CRUD/images/image01.png" width="90%" height="90%">     

접근방법을 선택 하여 주는 단계에서 Connect using MongoDB Compass를 선택 하면 접근 주소를 얻을 수 있습니다.    

<img src="/02.Provision and CRUD/images/image02.png" width="60%" height="60%">     

Connection String을 복사하여 줍니다. 이후 Compass를 실행 하여 줍니다.     
<img src="/02.Provision and CRUD/images/image03.png" width="70%" height="70%">     



복사한 Connection String을 입력하여 줍니다.   

<img src="/02.Provision and CRUD/images/image04.png" width="90%" height="90%">     


#### 데이터 조회
데이터베이스에서 생성한 samsung 탭을 클릭 하면 컬렉션 리스트를 볼 수 있습니다. 생성한 user컬렉션을 선택 합니다.    

<img src="/02.Provision and CRUD/images/image05.png" width="90%" height="90%">     

데이터 검색을 위해서 Filter 부분에 검색 조건을 입력 하여 줍니다.
ssn 이 123-456-0001 인 데이터를 찾기 위해 다음과 같이 입력 하여 줍니다.

````
{ssn: "123-456-0001"}
````

<img src="/02.Provision and CRUD/images/image06.png" width="90%" height="90%">     

나이(age)가 10 이상 40이하인 사람을 찾기를 합니다. 조건은 age >= 10 이고 age <=40으로 합니다.

````
{age: {$gte: 10, $lte: 40}}
````

<img src="/02.Provision and CRUD/images/image07.png" width="90%" height="90%">     



### option
생성된 데이터 베이스중 Movie 관련 데이터 컬렉션 (sample_mflix.movies)에서 다음 내용을 Query 합니다.

- 1987 년에 나온 데이터 조회 (Where year = 1987)

- 장르가 Comedy 에 속하는 영화 검색

- 장르가 Comedy 하나 만 있는 데이터 검색

- 장르가 Comedy 혹은 Drama 인 데이터 검색

- imdb 의 평가 점수가 8.0 이상이고 등급이 PG 인 영화 검색

- metacritic의 평점이 존재 하는 영화 검색

- Dr. Strangelove 로 시작하는 영화 검색

해당 쿼리는 다음과 같습니다.
- 1987 년에 나온 데이터 조회 (Where year = 1987)
````
db.movies.find({year:1987})
````
<img src="/02.Provision and CRUD/images/image11.png" width="90%" height="90%">     

- 장르가 Comedy 에 속하는 영화 검색
````
db.movies.find({genres: "Comedy"})

````
<img src="/02.Provision and CRUD/images/image12.png" width="90%" height="90%">     

- 장르가 Comedy 하나 만 있는 데이터 검색
````
db.movies.find({genres:["Comedy"]})

````
<img src="/02.Provision and CRUD/images/image13.png" width="90%" height="90%">     

- 장르가 Comedy 혹은 Drama 인 데이터 검색
````
db.movies.find({genres:{$in:["Comedy", "Drama"]}})

````
<img src="/02.Provision and CRUD/images/image14.png" width="90%" height="90%">     

- imdb 의 평가 점수가 8.0 이상이고 등급이 PG 인 영화 검색
````
db.movies.find({"imdb.rating" : {$gt: 8.0}, rated:"PG"})

````
<img src="/02.Provision and CRUD/images/image15.png" width="90%" height="90%">     

- metacritic의 평점이 존재 하는 영화 검색
````
db.movies.find({metacritic: {$exists: true}})

````
<img src="/02.Provision and CRUD/images/image16.png" width="90%" height="90%">     

- Dr. Strangelove 로 시작하는 영화 검색
````
db.movies.find({title: {$regex: '^Dr. Strangelove'}})

````
<img src="/02.Provision and CRUD/images/image17.png" width="90%" height="90%">     

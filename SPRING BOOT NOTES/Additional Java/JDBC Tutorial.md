- API that allows java programs to access and manipulate data in databases.
- JDBC drivers (types)
	1. JDBC-ODBC Bridge driver
	2. Native API Driver
	3. Network Protocol Driver
	4. Thin Driver

- JDBC Components
	1. Drivers
	2. Driver Manager <- Class
	3. Connection (Helps establish a connection)
	4. Statement (SQL queries in form of java codes)
	5. ResultSet (Retrieve and Modify data)
	6. SQL Exception (Database Exception class)

### Basic SYNTAX
`import java.sql.*;`

```java
Class.forName("com.mysql.jdbc.Driver"); // to load the drivers
Connection con = DriverManager.getConnection(url, username, password); // connection establishment
Statement stmt = con.createStatement(); // statement created
RuleSet rs = stmt.executeQuery(query); // execute SQL Query
```

Full Basic Code with example : 

```java
public static void main(String[] args) throws ClassNotFoundException {
	String url = "jdbc:mysql://localhost:3306/mydatabase"; // mydatabase -> database name
	String username = "root";
	String password = "Vulcan";

	try {
		class.forName("com.mysql.jdbc.Driver");
	}catch(ClassNotFoundException e){
		System.out.println(e.getMessage());
	}

	try{
		Connection con = DriverManager.getConnection(url, username, password);
		Statement stmt = con.createStatement();
		ResultSet rs = stmt.executeQuery("select * from School;");


		while(rs.next()){
			int id = rs.getInt("id"); // id -> column name in tables in db
			String name = rs.getString("name");
			System.out.println(id + " : " + name);
		}
		rs.close();
		stmt.close();
		con.close();
	}catch(SQLException e){
		System.out.println(e.getMessage());
	}
}
```

#### Insert Data into Database : 
`executeUpdate(q)` : to insert data (or update data); returns int i.e. number of rows affected
`executeQuery(q)` : to retrieve data : needs to stored in ResultSet

#### Delete Data from database
```java
String q = "DELETE from employees where id = 3;";
...
int r = executeUpdate(q);
```

#### Updating Data in Database
in mysql query :
```sql
UPDATE your_table_name
SET column1 = new_value1, column2 = new_value2
WHERE some_column = some_value;
```

doing same in java : 
```java
String query = "UPDATE employees \n" + "SET job_title = 'Full Stack Dev', salary = 70000.00" + "WHERE id = 2;";
executeUpdate(query);
```

## Hotel Reservation System 
- New Reservations
- Check reservations
- Get Room No.
- Update Reservations
- Quit/exit 
while(true) show main menu} quit : return

```
Database name : hotel_db
Table name : reservations

schema : 
reservation_id int Auto incr
guest_name varchar not_null
room_number int not_null
room_number int not_null
contact_number varchar not_null
reservation_date timestamp default
```

## Prepared Statement : 
- Used to execute SQL queries with placeholders for parameters
- Interfaces 
	1. Statement -> executeQuery()
	2. PreparedStatement -> executeQuery()

- In prepared statements we have placeholders and values

- Basically put values in the ? in the strings
#### To retrieve using PreparedStatement
```java
PreparedStatement preparedStatement = con.prepareStatement("select * from employees where name = ?");
preparedStatemen.setString(index, value);
// similarly setInt, setDouble : (Indexing starts from 1)
```

Ex : 
```java
String query = "select * from employees where name = ? AND job_title= ?";
PreparedStatement pst = con.prepareStatement(query);
pst.setString(1, "kunal");
pst.setString(2, "Developer");
ResultSet rs = pst.executeQuery();
```

#### To insert using PreparedStatement
```java
String query = "Insert into employees(id, name, job_title, salary) VALUES(?, ?, ?, ?)";
PreparedStatement pst = con.prepareStatement(query);
pst.setInt(1, 3);
pst.setString(2, "VULCAN");
pst.setString(3, "Java Developer");
pst.setDouble(4, 89000.0);

int rowsAffected = pst.executeQuery();
```


## Handling Images with JDBC

for image handling sql column code :
`image_data LONGBLOB NOT NULL` // Binary Large Object
#### Inserting image data into database
```java
String image_path = "D:/NSFW/...hello.jpg";
String query = "Insert into image)table(image_data) values(?);";
...
FileInputStream fs = new FileInputStream(image_path);
byte[] imageData = new byte[fs.available()];
fs.read(image.Data);

PreparedStatement ps = con.prepareStatement(query);
ps.setBytes(1, imageData);
int affectedRows = ps.executeUpdate();

if(affectedRows > 0){
	System.out.println("Inserted Successfully");
}else{
	System.out.println("Unsuccessfully");
}
ps.close();
con.close();
```
Exceptions to catch : SQLException, FileNotFoundException, IOException

#### Retrieving Image from database
```java
String folder_path = "D:/NSFW/.../images";
String query = "SELECT image_data from image_dable where image_id = (?);";

PreparedStatement ps = con.prepareStatement(query);
ps.setInt(1, 1);
ResultSet rs = ps.executeQuery();
if(rs.next()){
	byte[] image_data = rs.getBytes("image_data");
	String image_path = folder_path + "image.jpg";

	OutputStream os = new FileOutputStream(image_path);
	os.write(image_data);
	System.out.println("Image Download Successfull");
}else{
	System.out.println("Image download Failed");
}
os.close();
ps.close();
```

Exceptions to Catch : SQLException, FileNotFoundException, IOException
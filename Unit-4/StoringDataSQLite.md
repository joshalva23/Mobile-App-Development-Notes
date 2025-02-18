
# **Storing Data using SQLite in Android**

SQLite is a lightweight, serverless, and self-contained database engine. Android provides built-in support for SQLite databases, allowing developers to store structured data locally in their applications.

### **1. Setting Up SQLite in Android**

SQLite requires creating a `SQLiteOpenHelper` subclass that helps manage database creation, version management, and updates.

#### **Creating a Database Helper Class**

The `SQLiteOpenHelper` class simplifies database creation and version management.

```java
public class DBHelper extends SQLiteOpenHelper {

    // Database version and name
    private static final int DATABASE_VERSION = 1;
    private static final String DATABASE_NAME = "UserData.db";

    // Table name and columns
    private static final String TABLE_USER = "user";
    private static final String COLUMN_ID = "id";
    private static final String COLUMN_NAME = "name";
    private static final String COLUMN_AGE = "age";
    private static final String COLUMN_EMAIL = "email";

    // SQL query for creating the table
    private static final String CREATE_TABLE_USER = "CREATE TABLE " + TABLE_USER + " ("
            + COLUMN_ID + " INTEGER PRIMARY KEY AUTOINCREMENT, "
            + COLUMN_NAME + " TEXT, "
            + COLUMN_AGE + " INTEGER, "
            + COLUMN_EMAIL + " TEXT);";

    // Constructor
    public DBHelper(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    // Creating the database
    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL(CREATE_TABLE_USER);
    }

    // Upgrading the database (e.g., changing schema)
    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS " + TABLE_USER);
        onCreate(db);
    }
}
```

### **2. Inserting Data into SQLite Database**

To insert data into the database, you can use the `SQLiteDatabase.insert()` method.

#### **Example: Inserting Data**

```java
public void insertUser(String name, int age, String email) {
    // Get writable database
    SQLiteDatabase db = this.getWritableDatabase();

    // Create ContentValues object to store data
    ContentValues values = new ContentValues();
    values.put("name", name);
    values.put("age", age);
    values.put("email", email);

    // Insert data into the table
    db.insert("user", null, values);

    // Close the database
    db.close();
}
```

- `ContentValues` is used to store the data that you want to insert.
- `insert()` method is used to insert the values into the specified table.

### **3. Reading Data from SQLite Database**

To read data from the SQLite database, use the `SQLiteDatabase.query()` method or `rawQuery()`.

#### **Example: Retrieving Data**

```java
public List<User> getAllUsers() {
    List<User> userList = new ArrayList<>();
    String selectQuery = "SELECT * FROM user";

    SQLiteDatabase db = this.getReadableDatabase();
    Cursor cursor = db.rawQuery(selectQuery, null);

    // Loop through the result set
    if (cursor.moveToFirst()) {
        do {
            User user = new User();
            user.setId(cursor.getInt(cursor.getColumnIndex("id")));
            user.setName(cursor.getString(cursor.getColumnIndex("name")));
            user.setAge(cursor.getInt(cursor.getColumnIndex("age")));
            user.setEmail(cursor.getString(cursor.getColumnIndex("email")));
            userList.add(user);
        } while (cursor.moveToNext());
    }

    cursor.close();
    db.close();
    return userList;
}
```

- `rawQuery()` executes a raw SQL query and returns a `Cursor` object that you can use to access the data.
- You can iterate over the `Cursor` to retrieve each record and store it in a list or object.

### **4. Updating Data in SQLite Database**

To update existing records, you can use the `SQLiteDatabase.update()` method.

#### **Example: Updating Data**

```java
public int updateUser(int id, String name, int age, String email) {
    SQLiteDatabase db = this.getWritableDatabase();

    ContentValues values = new ContentValues();
    values.put("name", name);
    values.put("age", age);
    values.put("email", email);

    // Update the user where the id matches
    return db.update("user", values, "id = ?", new String[]{String.valueOf(id)});
}
```

- `update()` method takes the table name, `ContentValues` with the updated values, and the selection criteria (where the record matches a condition).
- It returns the number of rows affected by the update.

### **5. Deleting Data from SQLite Database**

To delete records, use the `SQLiteDatabase.delete()` method.

#### **Example: Deleting Data**

```java
public void deleteUser(int id) {
    SQLiteDatabase db = this.getWritableDatabase();
    
    // Delete the user where the id matches
    db.delete("user", "id = ?", new String[]{String.valueOf(id)});
    db.close();
}
```

- `delete()` method is used to remove a record that matches the condition (e.g., matching `id`).
  
### **6. Example of a Complete Activity Using SQLite**

Here’s a complete example where SQLite is used to store, read, update, and delete user data.

```java
public class MainActivity extends AppCompatActivity {

    DBHelper dbHelper;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        dbHelper = new DBHelper(this);

        // Insert data
        dbHelper.insertUser("John Doe", 28, "johndoe@example.com");

        // Read data
        List<User> users = dbHelper.getAllUsers();
        for (User user : users) {
            Log.d("User", "Name: " + user.getName() + ", Age: " + user.getAge());
        }

        // Update data
        dbHelper.updateUser(1, "John Updated", 29, "johnupdated@example.com");

        // Delete data
        dbHelper.deleteUser(1);
    }
}
```

- This example demonstrates inserting a user, retrieving all users, updating a user, and deleting a user.

### **7. Handling Database Version Upgrades**

SQLite database versioning is essential to handle schema changes (e.g., adding or modifying columns). You can manage database version upgrades in the `onUpgrade()` method of `SQLiteOpenHelper`.

#### **Example: Upgrading Database Schema**

```java
@Override
public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
    if (oldVersion < 2) {
        // Upgrade logic from version 1 to 2 (e.g., add a new column)
        db.execSQL("ALTER TABLE user ADD COLUMN phoneNumber TEXT");
    }
}
```

- `onUpgrade()` is called when the database version changes, allowing you to migrate the schema without losing data.

---

### **Conclusion**

- SQLite is an embedded database, ideal for storing structured data locally in Android applications.
- Use `SQLiteOpenHelper` to create, manage, and update the database.
- You can perform CRUD operations (Create, Read, Update, Delete) using `SQLiteDatabase`.
- Always handle database schema changes carefully using `onUpgrade()` to ensure backward compatibility.

---

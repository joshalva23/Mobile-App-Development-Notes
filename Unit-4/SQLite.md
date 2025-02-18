
# **SQLite Database in Android**

SQLite is a powerful and lightweight relational database management system that is used in Android for persistent data storage. Android provides built-in support for SQLite, making it easy to store structured data locally in applications.

### **1. Introduction to SQLite**

SQLite is an open-source, serverless, self-contained database engine that does not require a separate server process. It is ideal for storing small amounts of data like application settings, user preferences, and other data that needs to persist between app launches.

### **2. Setting Up SQLite in Android**

In Android, to use SQLite, you need to create a subclass of `SQLiteOpenHelper`. This helper class provides methods to manage database creation, upgrades, and version management.

#### **Creating a Database Helper Class**

```java
public class DBHelper extends SQLiteOpenHelper {

    // Database name and version
    private static final String DATABASE_NAME = "AppDatabase.db";
    private static final int DATABASE_VERSION = 1;

    // Table and column names
    private static final String TABLE_NAME = "users";
    private static final String COLUMN_ID = "id";
    private static final String COLUMN_NAME = "name";
    private static final String COLUMN_AGE = "age";
    private static final String COLUMN_EMAIL = "email";

    // SQL query to create the table
    private static final String CREATE_TABLE_QUERY =
            "CREATE TABLE " + TABLE_NAME + " (" +
            COLUMN_ID + " INTEGER PRIMARY KEY AUTOINCREMENT, " +
            COLUMN_NAME + " TEXT, " +
            COLUMN_AGE + " INTEGER, " +
            COLUMN_EMAIL + " TEXT);";

    public DBHelper(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    // Called when the database is created for the first time
    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL(CREATE_TABLE_QUERY);
    }

    // Called when the database needs to be upgraded
    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS " + TABLE_NAME);
        onCreate(db);
    }
}
```

- `DATABASE_NAME` is the name of the database.
- `DATABASE_VERSION` is used to handle database upgrades when the schema changes.
- `CREATE_TABLE_QUERY` is the SQL command to create the `users` table.

### **3. Inserting Data into SQLite Database**

To insert data into the database, we use `SQLiteDatabase.insert()` method. You can store data in a `ContentValues` object and then insert it into the table.

#### **Example: Inserting Data**

```java
public void insertUser(String name, int age, String email) {
    SQLiteDatabase db = this.getWritableDatabase();

    ContentValues values = new ContentValues();
    values.put(COLUMN_NAME, name);
    values.put(COLUMN_AGE, age);
    values.put(COLUMN_EMAIL, email);

    db.insert(TABLE_NAME, null, values);
    db.close();
}
```

- The `insertUser()` method stores a new user in the database.
- `ContentValues` is used to map column names to their respective values.

### **4. Reading Data from SQLite Database**

To read data from the SQLite database, you can use `SQLiteDatabase.query()` or `SQLiteDatabase.rawQuery()`. Both methods return a `Cursor` that can be used to iterate through the result set.

#### **Example: Retrieving Data**

```java
public List<User> getAllUsers() {
    List<User> userList = new ArrayList<>();
    SQLiteDatabase db = this.getReadableDatabase();

    // Query to select all rows from the table
    Cursor cursor = db.rawQuery("SELECT * FROM " + TABLE_NAME, null);

    if (cursor.moveToFirst()) {
        do {
            User user = new User();
            user.setId(cursor.getInt(cursor.getColumnIndex(COLUMN_ID)));
            user.setName(cursor.getString(cursor.getColumnIndex(COLUMN_NAME)));
            user.setAge(cursor.getInt(cursor.getColumnIndex(COLUMN_AGE)));
            user.setEmail(cursor.getString(cursor.getColumnIndex(COLUMN_EMAIL)));

            userList.add(user);
        } while (cursor.moveToNext());
    }

    cursor.close();
    db.close();
    return userList;
}
```

- The `rawQuery()` method is used to execute the SQL query, and the `Cursor` object is used to navigate the result set.
- In this example, all users are fetched and stored in a list.

### **5. Updating Data in SQLite Database**

To update existing data, use the `SQLiteDatabase.update()` method. This method allows you to specify which rows to update.

#### **Example: Updating Data**

```java
public int updateUser(int id, String name, int age, String email) {
    SQLiteDatabase db = this.getWritableDatabase();

    ContentValues values = new ContentValues();
    values.put(COLUMN_NAME, name);
    values.put(COLUMN_AGE, age);
    values.put(COLUMN_EMAIL, email);

    // Update the row with the specified id
    return db.update(TABLE_NAME, values, COLUMN_ID + " = ?", new String[]{String.valueOf(id)});
}
```

- The `update()` method takes a `ContentValues` object with updated values and a selection criterion to identify the row to be updated.

### **6. Deleting Data from SQLite Database**

To delete a specific row, use the `SQLiteDatabase.delete()` method.

#### **Example: Deleting Data**

```java
public void deleteUser(int id) {
    SQLiteDatabase db = this.getWritableDatabase();
    db.delete(TABLE_NAME, COLUMN_ID + " = ?", new String[]{String.valueOf(id)});
    db.close();
}
```

- The `delete()` method removes the row matching the specified condition (in this case, `id`).

### **7. Example of Complete Activity Using SQLite**

Here’s an example of using SQLite in an Android activity, demonstrating inserting, reading, updating, and deleting user data.

```java
public class MainActivity extends AppCompatActivity {

    DBHelper dbHelper;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        dbHelper = new DBHelper(this);

        // Insert data
        dbHelper.insertUser("John Doe", 28, "john.doe@example.com");

        // Get all users
        List<User> users = dbHelper.getAllUsers();
        for (User user : users) {
            Log.d("User", "Name: " + user.getName() + ", Age: " + user.getAge());
        }

        // Update data
        dbHelper.updateUser(1, "Jane Doe", 29, "jane.doe@example.com");

        // Delete user
        dbHelper.deleteUser(1);
    }
}
```

### **8. Handling Database Version Upgrades**

When you need to change the database schema, use the `onUpgrade()` method in `SQLiteOpenHelper`. This method is called when the database version is increased.

#### **Example: Upgrading Database Schema**

```java
@Override
public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
    if (oldVersion < 2) {
        // Add a new column to the table in version 2
        db.execSQL("ALTER TABLE " + TABLE_NAME + " ADD COLUMN phone_number TEXT");
    }
}
```

- `onUpgrade()` checks the old and new versions of the database and handles necessary schema changes.

---

### **Conclusion**

- **SQLite** is a great solution for local data storage in Android apps, especially when you need to manage structured data.
- The `SQLiteOpenHelper` class helps manage database creation and upgrades.
- You can perform **CRUD** (Create, Read, Update, Delete) operations with the `SQLiteDatabase` class.
- SQLite is suitable for small-to-medium datasets. For large datasets, consider other databases like **Room** or **Realm**.

---


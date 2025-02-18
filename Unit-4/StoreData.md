# Storing Data in Android

Android provides multiple ways to store and manage data based on the requirements of an application. The most common storage options include **SharedPreferences, Internal Storage, External Storage, Room Database, and DataStore**.

---

## 1. SharedPreferences (Key-Value Storage)
**SharedPreferences** is used to store small amounts of primitive data (key-value pairs) persistently. It is suitable for storing user preferences and settings.

### Example: Saving and Retrieving Data
```java
// Saving data
SharedPreferences sharedPreferences = getSharedPreferences("AppPrefs", MODE_PRIVATE);
SharedPreferences.Editor editor = sharedPreferences.edit();
editor.putString("username", "JohnDoe");
editor.putBoolean("dark_mode", true);
editor.apply();

// Retrieving data
String username = sharedPreferences.getString("username", "Guest");
boolean darkMode = sharedPreferences.getBoolean("dark_mode", false);
```

✅ **Best for:** Simple settings and user preferences.

---

## 2. Internal Storage (Private App Storage)
Internal storage allows saving private files that are accessible only to the application.

### Writing to Internal Storage
```java
String filename = "user_data.txt";
String content = "Hello, Android!";
FileOutputStream fos = openFileOutput(filename, Context.MODE_PRIVATE);
fos.write(content.getBytes());
fos.close();
```

### Reading from Internal Storage
```java
FileInputStream fis = openFileInput(filename);
InputStreamReader isr = new InputStreamReader(fis);
BufferedReader br = new BufferedReader(isr);
StringBuilder sb = new StringBuilder();
String line;
while ((line = br.readLine()) != null) {
    sb.append(line);
}
String content = sb.toString();
```

✅ **Best for:** Storing small private files (e.g., text logs, configs).

---

## 3. External Storage (Public File Storage)
External storage allows users to store larger files that can be accessed outside the app, such as images and documents.

### Writing to External Storage (Requires Permissions)
```java
if (Environment.MEDIA_MOUNTED.equals(Environment.getExternalStorageState())) {
    File file = new File(getExternalFilesDir(null), "example.txt");
    FileWriter writer = new FileWriter(file);
    writer.write("Hello External Storage");
    writer.close();
}
```

### Reading from External Storage
```java
File file = new File(getExternalFilesDir(null), "example.txt");
BufferedReader br = new BufferedReader(new FileReader(file));
String line;
while ((line = br.readLine()) != null) {
    Log.d("FileContent", line);
}
```

✅ **Best for:** Large files like images, videos, and documents.

---

## 4. Room Database (SQLite-based Storage)
Room is an abstraction over SQLite that provides a robust way to manage structured data in an Android app.

### Adding Dependencies
```gradle
dependencies {
    implementation "androidx.room:room-runtime:2.4.2"
    annotationProcessor "androidx.room:room-compiler:2.4.2"
}
```

### Defining a Data Entity
```java
@Entity
data class User(@PrimaryKey val id: Int, val name: String)
```

### Creating a DAO (Data Access Object)
```java
@Dao
interface UserDao {
    @Insert
    void insertUser(User user);
    
    @Query("SELECT * FROM User WHERE id = :id")
    User getUser(int id);
}
```

### Creating a Room Database
```java
@Database(entities = {User.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
    public abstract UserDao userDao();
}
```

✅ **Best for:** Complex data relationships and offline storage.

---

## 5. DataStore (Modern Key-Value Storage)
DataStore is an alternative to SharedPreferences, offering better performance and reliability.

### Adding Dependencies
```gradle
dependencies {
    implementation "androidx.datastore:datastore-preferences:1.0.0"
}
```

### Writing Data using DataStore
```java
val dataStore: DataStore<Preferences> = createDataStore(name = "settings")
val DARK_MODE_KEY = preferencesKey<Boolean>("dark_mode")

suspend fun saveDarkMode(value: Boolean) {
    dataStore.edit { preferences ->
        preferences[DARK_MODE_KEY] = value
    }
}
```

### Reading Data from DataStore
```java
val darkModeFlow: Flow<Boolean> = dataStore.data.map { preferences ->
    preferences[DARK_MODE_KEY] ?: false
}
```

✅ **Best for:** Storing simple key-value preferences efficiently.

---

## Summary Table

| Storage Method          | Best For | Supports Complex Data | Requires Permissions |
|-------------------------|----------|----------------------|---------------------|
| SharedPreferences       | User settings, flags | ❌ | ❌ |
| Internal Storage        | Private files, logs | ❌ | ❌ |
| External Storage        | Images, videos, large files | ❌ | ✅ |
| Room Database          | Structured data, caching | ✅ | ❌ |
| DataStore              | Modern key-value storage | ❌ | ❌ |

---

## Conclusion
Choosing the right storage solution depends on the nature of data and performance needs. **SharedPreferences and DataStore** are great for lightweight key-value storage, **Internal and External Storage** handle files, and **Room Database** is ideal for structured data.

Let me know if you need further clarification! 🚀



# **Shared Preferences in Android**

**SharedPreferences** is a simple way to store key-value pairs in Android. It's useful for storing small pieces of data such as user settings, preferences, or session information. SharedPreferences can be used for storing strings, integers, booleans, and other basic data types.

### **1. Storing Data with SharedPreferences**

To store data, you need to get an instance of `SharedPreferences` and use its `edit()` method to apply changes.

#### **Example: Storing Data**

```java
// Get the SharedPreferences instance
SharedPreferences sharedPreferences = getSharedPreferences("MyPreferences", MODE_PRIVATE);

// Edit the preferences
SharedPreferences.Editor editor = sharedPreferences.edit();
editor.putString("username", "JohnDoe"); // Store a string
editor.putInt("userAge", 25);            // Store an integer
editor.putBoolean("isLoggedIn", true);    // Store a boolean
editor.apply();  // Apply the changes
```

- `getSharedPreferences("MyPreferences", MODE_PRIVATE)` creates or opens a SharedPreferences file.
- `putString()`, `putInt()`, `putBoolean()` are used to store different types of data.
- `apply()` saves the changes asynchronously. Use `commit()` if you want to save synchronously, but `apply()` is preferred for performance.

### **2. Retrieving Data from SharedPreferences**

You can retrieve stored data using the corresponding `get` methods.

#### **Example: Retrieving Data**

```java
// Get the SharedPreferences instance
SharedPreferences sharedPreferences = getSharedPreferences("MyPreferences", MODE_PRIVATE);

// Retrieve stored data
String username = sharedPreferences.getString("username", "defaultUsername");
int userAge = sharedPreferences.getInt("userAge", 0);  // Default value is 0 if not found
boolean isLoggedIn = sharedPreferences.getBoolean("isLoggedIn", false); // Default is false

// Use the retrieved values
Log.d("SharedPreferences", "Username: " + username);
Log.d("SharedPreferences", "Age: " + userAge);
Log.d("SharedPreferences", "Logged In: " + isLoggedIn);
```

- `getString()`, `getInt()`, and `getBoolean()` are used to retrieve the corresponding data types.
- The second parameter in these methods is the default value if the key doesn’t exist.

### **3. Removing Data from SharedPreferences**

You can remove specific data using the `remove()` method.

#### **Example: Removing Data**

```java
// Get the SharedPreferences instance
SharedPreferences sharedPreferences = getSharedPreferences("MyPreferences", MODE_PRIVATE);

// Edit the preferences
SharedPreferences.Editor editor = sharedPreferences.edit();
editor.remove("username"); // Remove the stored 'username' value
editor.apply(); // Apply the changes
```

### **4. Clearing All Data from SharedPreferences**

To remove all data in SharedPreferences, use the `clear()` method.

#### **Example: Clearing All Data**

```java
// Get the SharedPreferences instance
SharedPreferences sharedPreferences = getSharedPreferences("MyPreferences", MODE_PRIVATE);

// Edit the preferences
SharedPreferences.Editor editor = sharedPreferences.edit();
editor.clear();  // Clears all stored data
editor.apply();  // Apply the changes
```

### **5. Using SharedPreferences in a PreferenceActivity**

Android provides a `PreferenceActivity` class to manage preferences, and you can use `PreferenceFragment` for a more modular approach.

#### **Example: PreferenceActivity**

```java
public class SettingsActivity extends PreferenceActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Add preferences from XML
        addPreferencesFromResource(R.xml.preferences);
    }
}
```

In this case, `preferences.xml` is an XML file that defines the preference items.

#### **Example: preferences.xml**

```xml
<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android">
    <EditTextPreference
        android:key="username"
        android:title="Username"
        android:summary="Enter your username"
        android:defaultValue=""
        android:inputType="text" />

    <CheckBoxPreference
        android:key="is_logged_in"
        android:title="Keep me logged in"
        android:defaultValue="false" />

    <PreferenceCategory
        android:title="User Settings">
        <ListPreference
            android:key="user_language"
            android:title="Language"
            android:defaultValue="en"
            android:entries="@array/language_options"
            android:entryValues="@array/language_values" />
    </PreferenceCategory>
</PreferenceScreen>
```

### **6. Example of a Complete Activity Using SharedPreferences**

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Get the SharedPreferences instance
        SharedPreferences sharedPreferences = getSharedPreferences("MyPreferences", MODE_PRIVATE);

        // Store some data
        SharedPreferences.Editor editor = sharedPreferences.edit();
        editor.putString("username", "JaneDoe");
        editor.putInt("userAge", 30);
        editor.putBoolean("isLoggedIn", true);
        editor.apply();

        // Retrieve and display the data
        String username = sharedPreferences.getString("username", "defaultUsername");
        int userAge = sharedPreferences.getInt("userAge", 0);
        boolean isLoggedIn = sharedPreferences.getBoolean("isLoggedIn", false);

        TextView textView = findViewById(R.id.textView);
        textView.setText("Username: " + username + "\nAge: " + userAge + "\nLogged In: " + isLoggedIn);
    }
}
```

### **7. Handling Multiple SharedPreferences**

You can use different files for various sets of data. Each call to `getSharedPreferences()` uses a different file.

#### **Example: Using Multiple SharedPreferences Files**

```java
// Create or access a different preferences file
SharedPreferences userPrefs = getSharedPreferences("UserSettings", MODE_PRIVATE);
SharedPreferences.Editor editor = userPrefs.edit();
editor.putString("theme", "dark");
editor.apply();
```

---

### **Conclusion**

- **SharedPreferences** is a simple and efficient way to store key-value pairs of primitive data types in Android.
- It is typically used for small amounts of data, such as user preferences or settings.
- For larger data, consider using databases like SQLite or Room.

---

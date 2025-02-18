# Preferences and Settings in Android

In Android, preferences and settings allow users to customize app behavior and store persistent data across sessions. The primary way to manage preferences in Android is through **SharedPreferences** and the **Preference API**.

---

## 1. SharedPreferences
**SharedPreferences** is used to store key-value pairs persistently. It is suitable for small amounts of data like user preferences, settings, and flags.

### 1.1. Creating SharedPreferences
To create or access a `SharedPreferences` file, use one of the following methods:

- **getSharedPreferences(name, mode)** → Used when multiple preferences files are needed.
- **getPreferences(mode)** → Used when only a single preferences file is required.

### 1.2. Storing Data in SharedPreferences
Use an `Editor` to write data.

#### Example: Saving User Preferences
```java
SharedPreferences sharedPreferences = getSharedPreferences("UserSettings", MODE_PRIVATE);
SharedPreferences.Editor editor = sharedPreferences.edit();
editor.putString("username", "JohnDoe");
editor.putInt("age", 25);
editor.putBoolean("notifications_enabled", true);
editor.apply();  // Use apply() for asynchronous saving
```

---

### 1.3. Retrieving Data from SharedPreferences
```java
SharedPreferences sharedPreferences = getSharedPreferences("UserSettings", MODE_PRIVATE);
String username = sharedPreferences.getString("username", "DefaultUser");
int age = sharedPreferences.getInt("age", 0);
boolean notifications = sharedPreferences.getBoolean("notifications_enabled", false);
```

---

### 1.4. Deleting Data in SharedPreferences
```java
SharedPreferences sharedPreferences = getSharedPreferences("UserSettings", MODE_PRIVATE);
SharedPreferences.Editor editor = sharedPreferences.edit();
editor.remove("username");  // Remove a single key
editor.clear();  // Remove all keys
editor.apply();
```

---

## 2. Preference API (Preference Screen UI)
The **Preference API** provides an easy way to create a settings screen in Android using `PreferenceFragmentCompat`.

### 2.1. Adding Dependencies
Ensure the `androidx.preference` dependency is included in your `build.gradle` file:

```gradle
dependencies {
    implementation "androidx.preference:preference:1.2.1"
}
```

---

### 2.2. Creating a Preference Screen
Define a settings screen using an XML resource.

#### `res/xml/preferences.xml`
```xml
<PreferenceScreen xmlns:app="http://schemas.android.com/apk/res-auto">

    <EditTextPreference
        app:key="username"
        app:title="Username"
        app:dialogTitle="Enter your username"
        app:defaultValue="Guest" />

    <SwitchPreferenceCompat
        app:key="notifications"
        app:title="Enable Notifications"
        app:defaultValue="true" />

    <ListPreference
        app:key="theme"
        app:title="Select Theme"
        app:entries="@array/theme_options"
        app:entryValues="@array/theme_values"
        app:defaultValue="light" />

</PreferenceScreen>
```

---

### 2.3. Creating the Settings Fragment
Create a fragment that loads the preference screen.

#### `SettingsFragment.java`
```java
public class SettingsFragment extends PreferenceFragmentCompat {
    @Override
    public void onCreatePreferences(Bundle savedInstanceState, String rootKey) {
        setPreferencesFromResource(R.xml.preferences, rootKey);
    }
}
```

---

### 2.4. Launching the Settings Screen
In your `SettingsActivity.java`, load the fragment.

#### `SettingsActivity.java`
```java
public class SettingsActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);
        getSupportFragmentManager()
                .beginTransaction()
                .replace(R.id.settings_container, new SettingsFragment())
                .commit();
    }
}
```

#### `activity_settings.xml`
```xml
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/settings_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

---

### 3. Reading Preference Values in Code
To retrieve settings values in an activity:

```java
SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
String username = sharedPreferences.getString("username", "Guest");
boolean notifications = sharedPreferences.getBoolean("notifications", true);
String theme = sharedPreferences.getString("theme", "light");
```

---

## 4. Best Practices
✅ Use `apply()` instead of `commit()` for non-blocking writes.  
✅ Use `PreferenceFragmentCompat` for modern settings UI.  
✅ Store complex objects using JSON (Gson) instead of SharedPreferences.  
✅ Use `EncryptedSharedPreferences` for sensitive data.  

---

## Conclusion
Preferences and settings in Android allow users to customize app behavior with `SharedPreferences` (for simple key-value storage) and the `Preference API` (for UI-based settings). The combination of these tools ensures a smooth user experience.


# **Styles and Themes in Android**  

Styles and themes are essential tools in Android development to ensure **consistency in design, maintainability, and reusability** of UI components. They help developers define and manage the **visual appearance** of their application elements.  

---

## **What are Styles and Themes?**  

### **Styles**  
A **style** is a collection of properties (**text color, font size, background color**) that can be applied to **individual UI elements** (e.g., `TextView`, `Button`) to ensure a **consistent look**.  

### **Themes**  
A **theme** is a **style applied to an entire activity or application**. It controls the overall **appearance of the app**, including **colors, widgets, and layouts**.  

---

## **Difference Between Styles and Themes**  

| Aspect    | Styles | Themes |
|-----------|--------|--------|
| **Scope** | Applied to **specific views** or widgets. | Applied to **an entire activity** or application. |
| **Usage** | Defines **appearance** for individual widgets. | Controls **overall app design** (e.g., colors, UI components). |
| **Inheritance** | Styles can **inherit** other styles. | Themes can **inherit** other themes. |

---

## **Subtopics with Code Examples**  

### **1. Defining a Style**  

📂 **File:** `res/values/styles.xml`  

```xml
<resources>
    <!-- Custom Button Style -->
    <style name="CustomButtonStyle">
        <item name="android:background">@color/purple_500</item>
        <item name="android:textColor">@android:color/white</item>
        <item name="android:padding">12dp</item>
        <item name="android:fontFamily">sans-serif-medium</item>
    </style>
</resources>
```

#### **Usage in Layout XML**  

```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Styled Button"
    style="@style/CustomButtonStyle" />
```

---

### **2. Inheriting a Style**  

You can base a new style on an existing one using the **parent attribute**.  

📂 **File:** `res/values/styles.xml`  

```xml
<resources>
    <!-- Base Style -->
    <style name="BaseTextStyle">
        <item name="android:textColor">@android:color/black</item>
        <item name="android:textSize">16sp</item>
    </style>

    <!-- Inherited Style -->
    <style name="LargeTextStyle" parent="BaseTextStyle">
        <item name="android:textSize">20sp</item>
        <item name="android:textStyle">bold</item>
    </style>
</resources>
```

#### **Usage in Layout XML**  

```xml
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Hello, Android!"
    style="@style/LargeTextStyle" />
```

---

### **3. Defining a Theme**  

A **theme** applies to the whole activity or app and is defined in `styles.xml`.  

📂 **File:** `res/values/styles.xml`  

```xml
<resources>
    <!-- Custom Theme -->
    <style name="CustomTheme" parent="Theme.Material3.DayNight">
        <item name="colorPrimary">@color/purple_500</item>
        <item name="colorPrimaryVariant">@color/purple_700</item>
        <item name="colorOnPrimary">@android:color/white</item>
        <item name="android:windowBackground">@color/white</item>
        <item name="android:statusBarColor">@color/purple_700</item>
    </style>
</resources>
```

#### **Applying the Theme in `AndroidManifest.xml`**  

```xml
<application
    android:theme="@style/CustomTheme"
    ... >
</application>
```

---

### **4. Dynamic Theme Switching**  

To allow users to **switch between themes** (e.g., **Light and Dark modes**):  

📂 **File:** `res/values/themes.xml`  

```xml
<resources>
    <style name="AppTheme.Light" parent="Theme.Material3.Light">
        <item name="colorPrimary">@color/blue_500</item>
        <item name="colorOnPrimary">@android:color/white</item>
    </style>

    <style name="AppTheme.Dark" parent="Theme.Material3.Dark">
        <item name="colorPrimary">@color/black</item>
        <item name="colorOnPrimary">@color/white</item>
    </style>
</resources>
```

#### **Java Code to Switch Themes**  

📂 **File:** `MainActivity.java`  

```java
import android.content.SharedPreferences;
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // Load saved theme
        SharedPreferences prefs = getSharedPreferences("AppPrefs", MODE_PRIVATE);
        boolean isDarkTheme = prefs.getBoolean("isDarkTheme", false);
        setTheme(isDarkTheme ? R.style.AppTheme_Dark : R.style.AppTheme_Light);

        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    private void toggleTheme(boolean darkTheme) {
        SharedPreferences.Editor editor = getSharedPreferences("AppPrefs", MODE_PRIVATE).edit();
        editor.putBoolean("isDarkTheme", darkTheme);
        editor.apply();

        recreate(); // Restart the activity to apply the new theme
    }
}
```

---

### **5. Overriding Theme for Specific Activities**  

You can apply a **different theme to individual activities** in your app.  

📂 **File:** `AndroidManifest.xml`  

```xml
<activity
    android:name=".SecondaryActivity"
    android:theme="@style/CustomTheme" />
```

# **Material Design in Android**  

**Material Design** is a design system developed by Google to create **visually appealing, consistent, and user-friendly interfaces**.  
It includes guidelines for **colors, typography, layouts, components, and animations**.  
Android provides built-in support for **Material Design** through the **Material Components** library.  

---

## **1. Setting Up Material Design in Android**  

To use **Material Design components**, add this dependency to **build.gradle (Module):**  

```gradle
dependencies {
    implementation 'com.google.android.material:material:1.9.0'
}
```

---

## **2. Material Theme**  

Material **themes define colors, typography, and shapes**.  
Define them in **res/values/themes.xml**:  

📂 **File:** `res/values/themes.xml`  

```xml
<resources>
    <style name="Theme.MyApp" parent="Theme.Material3.Light">
        <item name="colorPrimary">@color/teal_700</item>
        <item name="colorOnPrimary">@android:color/white</item>
        <item name="android:statusBarColor">@color/teal_700</item>
    </style>
</resources>
```

### **Applying the Theme in `AndroidManifest.xml`**  

```xml
<application
    android:theme="@style/Theme.MyApp">
</application>
```

---

## **3. Material Components**  

### **a) Material Button**  

Material buttons provide **ripple effects, elevation, and customization**.  

```xml
<com.google.android.material.button.MaterialButton
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Material Button"
    style="@style/Widget.MaterialComponents.Button.OutlinedButton"
    android:padding="12dp"
    android:backgroundTint="@color/teal_700"
    android:textColor="@android:color/white"/>
```

---

### **b) Floating Action Button (FAB)**  

Used for **primary actions** like adding a new item.  

```xml
<com.google.android.material.floatingactionbutton.FloatingActionButton
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@drawable/ic_add"
    android:contentDescription="Add"
    android:backgroundTint="@color/teal_700"/>
```

---

### **c) Material TextInputLayout (EditText with Floating Label)**  

Adds a **floating label and error messages** to EditText.  

```xml
<com.google.android.material.textfield.TextInputLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Enter your name">

    <com.google.android.material.textfield.TextInputEditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
</com.google.android.material.textfield.TextInputLayout>
```

---

### **d) Material CardView**  

Creates a **card with elevation and rounded corners**.  

```xml
<com.google.android.material.card.MaterialCardView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:cardCornerRadius="12dp"
    app:cardElevation="4dp"
    android:padding="16dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Material CardView"/>
</com.google.android.material.card.MaterialCardView>
```

---

### **e) Material Toolbar**  

A **modern App Bar** with **Material styling**.  

```xml
<com.google.android.material.appbar.MaterialToolbar
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:title="Material Toolbar"/>
```

---

### **f) Material Bottom Navigation**  

Provides **quick navigation** between app sections.  

```xml
<com.google.android.material.bottomnavigation.BottomNavigationView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:menu="@menu/bottom_nav_menu"/>
```

📂 **File:** `menu/bottom_nav_menu.xml`  

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/home"
        android:icon="@drawable/ic_home"
        android:title="Home" />
    <item
        android:id="@+id/profile"
        android:icon="@drawable/ic_profile"
        android:title="Profile" />
</menu>
```

---

## **4. Dark Mode Support**  

Material Design supports **light and dark themes**.  
Define both in `res/values/themes.xml` and `res/values-night/themes.xml`.  

### **Light Theme (`res/values/themes.xml`)**  

```xml
<style name="Theme.MyApp" parent="Theme.Material3.Light">
    <item name="colorPrimary">@color/teal_700</item>
    <item name="colorOnPrimary">@android:color/white</item>
</style>
```

### **Dark Theme (`res/values-night/themes.xml`)**  

```xml
<style name="Theme.MyApp" parent="Theme.Material3.Dark">
    <item name="colorPrimary">@color/black</item>
    <item name="colorOnPrimary">@color/white</item>
</style>
```

---

## **5. Implementing Dark Mode in Java**  

📂 **File:** `MainActivity.java`  

```java
import android.content.SharedPreferences;
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import androidx.appcompat.app.AppCompatDelegate;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        SharedPreferences prefs = getSharedPreferences("Settings", MODE_PRIVATE);
        boolean darkMode = prefs.getBoolean("darkMode", false);
        AppCompatDelegate.setDefaultNightMode(darkMode ? 
            AppCompatDelegate.MODE_NIGHT_YES : AppCompatDelegate.MODE_NIGHT_NO);
    }

    private void toggleDarkMode(boolean enable) {
        SharedPreferences.Editor editor = getSharedPreferences("Settings", MODE_PRIVATE).edit();
        editor.putBoolean("darkMode", enable);
        editor.apply();

        AppCompatDelegate.setDefaultNightMode(enable ? 
            AppCompatDelegate.MODE_NIGHT_YES : AppCompatDelegate.MODE_NIGHT_NO);
        recreate();
    }
}
```

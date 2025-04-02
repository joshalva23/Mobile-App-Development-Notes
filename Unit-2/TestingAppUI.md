
# **Testing App UI in Android**  

UI testing ensures that an **Android app’s interface functions correctly** across different devices and screen sizes.  
It helps verify that UI components **behave as expected**, improving user experience and preventing visual or functional bugs.

---

## **1. Types of UI Testing**  

1. **Manual Testing** – Checking UI interactions manually by navigating the app.  
2. **Automated UI Testing** – Writing test scripts to simulate user actions and verify UI behavior automatically.

---

## **2. Tools for UI Testing**  

- **Espresso** – Android’s official UI testing framework.  
- **UI Automator** – For testing across multiple apps.  
- **Robolectric** – For running tests without an emulator or device.  

---

## **3. Setting Up UI Testing with Espresso**  

### **Step 1: Add Dependencies**  

📂 **File:** `build.gradle (Module: app)`

```gradle
dependencies {
    androidTestImplementation 'androidx.test.ext:junit:1.1.5'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.5.1'
}
```

---

## **4. Writing a UI Test Using Espresso**  

### **Example: Testing a Button Click**  

#### **Layout (`activity_main.xml`)**  

```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello, World!"
        android:textSize="18sp"/>

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Click Me"/>
</LinearLayout>
```

#### **MainActivity (`MainActivity.java`)**  

```java
import android.os.Bundle;
import android.widget.Button;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        TextView textView = findViewById(R.id.textView);
        Button button = findViewById(R.id.button);

        button.setOnClickListener(v -> textView.setText("Button Clicked!"));
    }
}
```

#### **UI Test (`MainActivityTest.java`)**  

```java
import androidx.test.ext.junit.rules.ActivityScenarioRule;
import androidx.test.ext.junit.runners.AndroidJUnit4;
import androidx.test.espresso.Espresso;
import androidx.test.espresso.action.ViewActions;
import androidx.test.espresso.assertion.ViewAssertions;
import androidx.test.espresso.matcher.ViewMatchers;

import org.junit.Rule;
import org.junit.Test;
import org.junit.runner.RunWith;

@RunWith(AndroidJUnit4.class)
public class MainActivityTest {

    @Rule
    public ActivityScenarioRule<MainActivity> activityRule =
            new ActivityScenarioRule<>(MainActivity.class);

    @Test
    public void testButtonClickChangesTextView() {
        // Click the button
        Espresso.onView(ViewMatchers.withId(R.id.button))
                .perform(ViewActions.click());

        // Check if the TextView text changed
        Espresso.onView(ViewMatchers.withId(R.id.textView))
                .check(ViewAssertions.matches(ViewMatchers.withText("Button Clicked!")));
    }
}
```

---

## **5. Running the UI Test**  

### **Run from Android Studio**  
1. Open **`MainActivityTest.java`**  
2. Click the **Run icon** next to the test function.  

### **Run from Terminal**  
```sh
./gradlew connectedAndroidTest
```

---

## **6. Testing RecyclerView UI**  

This test checks if a **RecyclerView displays a specific item**.  

#### **RecyclerView Test (`RecyclerViewTest.java`)**  

```java
import androidx.test.ext.junit.runners.AndroidJUnit4;
import androidx.test.espresso.contrib.RecyclerViewActions;
import androidx.test.espresso.matcher.ViewMatchers;
import androidx.test.rule.ActivityTestRule;

import org.junit.Rule;
import org.junit.Test;
import org.junit.runner.RunWith;

import static androidx.test.espresso.Espresso.onView;
import static androidx.test.espresso.assertion.ViewAssertions.matches;
import static androidx.test.espresso.matcher.ViewMatchers.hasDescendant;
import static androidx.test.espresso.matcher.ViewMatchers.withId;

@RunWith(AndroidJUnit4.class)
public class RecyclerViewTest {

    @Rule
    public ActivityTestRule<MainActivity> activityRule =
            new ActivityTestRule<>(MainActivity.class);

    @Test
    public void testRecyclerViewItem() {
        // Scroll to position and check if item is displayed
        onView(withId(R.id.recyclerView))
                .perform(RecyclerViewActions.scrollToPosition(2))
                .check(matches(hasDescendant(ViewMatchers.withText("Item 3"))));
    }
}
```

---

## **7. Screenshot Testing with Espresso**  

Take a screenshot during a UI test for **debugging**.  

### **Example: Capture a screenshot on failure**  

```java
import android.graphics.Bitmap;
import android.os.Environment;
import androidx.test.espresso.Espresso;
import androidx.test.ext.junit.runners.AndroidJUnit4;
import androidx.test.rule.ActivityTestRule;

import org.junit.Rule;
import org.junit.Test;
import org.junit.runner.RunWith;
import java.io.File;
import java.io.FileOutputStream;

@RunWith(AndroidJUnit4.class)
public class ScreenshotTest {

    @Rule
    public ActivityTestRule<MainActivity> activityRule =
            new ActivityTestRule<>(MainActivity.class);

    @Test
    public void takeScreenshot() {
        Espresso.onView(ViewMatchers.withId(R.id.button)).perform(ViewActions.click());

        Bitmap screenshot = EspressoScreenshot.capture();
        saveScreenshot(screenshot, "test_screenshot.png");
    }

    private void saveScreenshot(Bitmap bitmap, String fileName) {
        File file = new File(Environment.getExternalStorageDirectory(), fileName);
        try (FileOutputStream out = new FileOutputStream(file)) {
            bitmap.compress(Bitmap.CompressFormat.PNG, 100, out);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

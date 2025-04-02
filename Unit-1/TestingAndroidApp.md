# **Testing Android Apps 🧪**  

Testing is a **crucial** part of the Android development process, ensuring that your app **functions correctly** and meets **quality standards**. Android provides **various testing strategies** and **tools** to test apps effectively.  

---

## **1. Types of Testing in Android**  
| **Type** | **Purpose** |
|----------|------------|
| **Unit Testing** | Tests **individual functions or classes** in isolation. |
| **UI Testing** | Tests the **user interface** (buttons, text, navigation, etc.). |
| **Integration Testing** | Ensures that **different app components** work together. |
| **End-to-End Testing** | Simulates **real user interactions** to test full functionality. |

---

## **2. Unit Testing in Android**  
Unit testing helps verify that individual components (such as **methods** or **classes**) work **as expected**.  

### **Setting Up Unit Tests in Android Studio**  
- Unit tests are located in **`src/test/java`** (not on actual devices).  
- Use the **JUnit** framework for unit testing.  
- Add **JUnit dependency** in `build.gradle`:  
  ```gradle
  testImplementation 'junit:junit:4.13.2'
  ```

### **Example - Unit Test with JUnit**  
```java
import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class CalculatorTest {

    @Test
    public void testAddition() {
        Calculator calculator = new Calculator();
        int result = calculator.add(2, 3);
        assertEquals(5, result);  // Test passes if result is 5
    }
}
```

### **Running Unit Tests**  
- **Right-click** on the test class or method → **Run ‘testClassName’**.  
- Results appear in the **Run window**.  

---

## **3. UI Testing in Android**  
UI testing ensures the **user interface behaves correctly**. The recommended framework for UI testing in Android is **Espresso**.  

### **Setting Up Espresso for UI Testing**  
1. Add Espresso dependencies to `build.gradle`:  
   ```gradle
   androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'
   androidTestImplementation 'androidx.test.espresso:espresso-intents:3.4.0'
   ```
2. Create a **UI test class** in `src/androidTest/java`.  

### **Example - Basic Espresso Test**  
```java
import androidx.test.ext.junit.rules.ActivityScenarioRule;
import androidx.test.espresso.Espresso;
import androidx.test.espresso.matcher.ViewMatchers;
import androidx.test.espresso.assertion.ViewAssertions;
import androidx.test.espresso.action.ViewActions;
import org.junit.Rule;
import org.junit.Test;

public class MainActivityTest {

    @Rule
    public ActivityScenarioRule<MainActivity> activityRule =
            new ActivityScenarioRule<>(MainActivity.class);

    @Test
    public void testButtonClick() {
        Espresso.onView(ViewMatchers.withId(R.id.my_button))
                .perform(ViewActions.click());
        Espresso.onView(ViewMatchers.withId(R.id.my_text))
                .check(ViewAssertions.matches(ViewMatchers.withText("Button clicked")));
    }
}
```
✔ This test **clicks a button** (`my_button`) and **checks if the text updates** (`my_text`).  

---

## **4. Running UI Tests**  
✔ **Right-click** the test class/method → **Run ‘testClassName’**.  
✔ Results appear in the **Run window**.  
✔ **Espresso tests** provide feedback (✔ Pass / ❌ Fail).  

---

## **5. Instrumented Tests (Real-Device Testing)**  
**Instrumented tests** run on **real devices** or **emulators**, interacting with the app’s UI.  

### **Setting Up Instrumented Tests**  
- Instrumented tests are located in **`src/androidTest/java`**.  
- Use **JUnit** or **Espresso** for writing instrumented tests.  

### **Example - Simple Instrumented Test**  
```java
import android.content.Context;
import androidx.test.core.app.ApplicationProvider;
import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class ContextTest {

    @Test
    public void testContext() {
        Context appContext = ApplicationProvider.getApplicationContext();
        assertEquals("com.example.myapp", appContext.getPackageName());
    }
}
```
✔ This test **checks the app's package name** on an actual device/emulator.  

---

## **6. Using the Android Emulator for Testing**  
The **Android Emulator** lets you test your app on **various devices**, screen sizes, and OS versions.  

### **Running Tests on the Emulator**  
1. **Launch the Emulator**: Select a device → Click **Run** ▶️.  
2. **Run the App**: Test the app **as if it were running on a real device**.  

---

## **7. Debugging Tests**  
If a test **fails**, you can use **Android Studio’s debugger**:  
✔ **Set breakpoints** in test methods.  
✔ **Inspect variables** while tests are running.  
✔ **Step through code** to find issues.  

---

## **8. Code Coverage 📊**  
Code coverage helps determine how much of your code is **covered by tests**.  

### **Enabling Code Coverage**  
✔ Click **Run with Coverage** ▶️ (green play icon with a pie chart).  
✔ View the **code coverage report** (shows tested vs. untested code).  

---

## **9. Continuous Integration (CI) for Automated Testing**  
For **large projects**, use **CI tools** like:  
✔ **Jenkins**  
✔ **GitHub Actions**  
✔ **Bitrise**  

These tools **automate testing** every time you **push code to GitHub**.  

---

## **Summary**  
✔ **Unit Tests** → Test individual methods/classes (**JUnit**).  
✔ **UI Tests** → Test user interface behavior (**Espresso**).  
✔ **Instrumented Tests** → Test app on **real devices/emulators**.  
✔ **Emulator Testing** → Simulate different device configurations.  
✔ **Debugging Tests** → Use breakpoints and Logcat to analyze test failures.  
✔ **Code Coverage** → Measure test coverage using built-in tools.  
✔ **CI/CD Integration** → Automate testing using tools like **Jenkins & GitHub Actions**.  

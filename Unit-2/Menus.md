# Menus in Android  

Menus in Android are a crucial part of app navigation and offer users options to perform specific actions. Android provides various types of menus: **Options Menu, Context Menu, and Popup Menu**. These menus are implemented using different classes and methods to display a list of items that users can interact with.  

## 1. Options Menu  
The **Options Menu** is the primary menu for an activity. It is typically displayed when the user taps the "Menu" button on the device or interacts with an action bar (if supported). The options menu is used to display items that provide global actions, such as **"Settings", "Help", or "Search"**.  

### Creating an Options Menu  
To create an **Options Menu**, you need to override the `onCreateOptionsMenu()` method in your activity. This method inflates a menu resource file that defines the items in the menu.  

### Example - Options Menu:  

#### Define the Menu XML (`res/menu/menu_main.xml`):  
```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/item_settings"
        android:title="Settings"
        android:icon="@drawable/ic_settings"
        android:showAsAction="ifRoom" />
    <item
        android:id="@+id/item_help"
        android:title="Help"
        android:showAsAction="never" />
</menu>
```

#### Inflate the Menu in Activity (`MainActivity.java`):  
```java
@Override
public boolean onCreateOptionsMenu(Menu menu) {
    // Inflate the menu; this adds items to the action bar if it is present.
    getMenuInflater().inflate(R.menu.menu_main, menu);
    return true;
}

@Override
public boolean onOptionsItemSelected(MenuItem item) {
    // Handle item selection
    switch (item.getItemId()) {
        case R.id.item_settings:
            Toast.makeText(this, "Settings clicked", Toast.LENGTH_SHORT).show();
            return true;
        case R.id.item_help:
            Toast.makeText(this, "Help clicked", Toast.LENGTH_SHORT).show();
            return true;
        default:
            return super.onOptionsItemSelected(item);
    }
}
```

In this example, the menu is displayed in the app, and when the user taps on **"Settings"** or **"Help"**, the corresponding toast message is shown.  

## 2. Context Menu  
A **Context Menu** is a floating menu that appears when the user long-presses on a view or UI element. Context menus provide additional options related to a specific view or item. They are useful for showing options that are relevant to a particular context, such as **"Delete", "Edit", or "Share"**.  

### Creating a Context Menu  
To create a **Context Menu**, you need to register the view that will trigger the context menu and override the `onCreateContextMenu()` method.  

### Example - Context Menu:  

#### Register View for Context Menu in Activity (`MainActivity.java`):  
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    
    View view = findViewById(R.id.myView);  // Assume myView is a UI element (e.g., TextView)
    registerForContextMenu(view);
}

@Override
public void onCreateContextMenu(ContextMenu menu, View v, ContextMenu.ContextMenuInfo menuInfo) {
    super.onCreateContextMenu(menu, v, menuInfo);
    MenuInflater inflater = getMenuInflater();
    inflater.inflate(R.menu.context_menu, menu); // Menu file for context menu
}

@Override
public boolean onContextItemSelected(MenuItem item) {
    switch (item.getItemId()) {
        case R.id.item_edit:
            Toast.makeText(this, "Edit clicked", Toast.LENGTH_SHORT).show();
            return true;
        case R.id.item_delete:
            Toast.makeText(this, "Delete clicked", Toast.LENGTH_SHORT).show();
            return true;
        default:
            return super.onContextItemSelected(item);
    }
}
```

#### Define Context Menu XML (`res/menu/context_menu.xml`):  
```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/item_edit"
        android:title="Edit" />
    <item
        android:id="@+id/item_delete"
        android:title="Delete" />
</menu>
```

In this example, when the user long-presses the **`myView`** view, the context menu will appear with options to **"Edit"** or **"Delete"**.  

## 3. Popup Menu  
A **Popup Menu** is a simple menu that is anchored to a specific view and appears when the user taps on that view. It is similar to a context menu but can be triggered by a normal click, not just a long press.  

### Creating a Popup Menu  
To create a **Popup Menu**, you use the `PopupMenu` class. You need to call `show()` on the `PopupMenu` object to display it when a user taps on the button or view.  

### Example - Popup Menu:  

#### Define the Popup Menu XML (`res/menu/popup_menu.xml`):  
```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/item_new"
        android:title="New" />
    <item
        android:id="@+id/item_open"
        android:title="Open" />
    <item
        android:id="@+id/item_save"
        android:title="Save" />
</menu>
```

#### Displaying the Popup Menu in Activity (`MainActivity.java`):  
```java
Button button = findViewById(R.id.buttonShowMenu);  // Assume this is a Button

button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        PopupMenu popupMenu = new PopupMenu(MainActivity.this, v);
        MenuInflater inflater = popupMenu.getMenuInflater();
        inflater.inflate(R.menu.popup_menu, popupMenu.getMenu());
        
        popupMenu.setOnMenuItemClickListener(new PopupMenu.OnMenuItemClickListener() {
            @Override
            public boolean onMenuItemClick(MenuItem item) {
                switch (item.getItemId()) {
                    case R.id.item_new:
                        Toast.makeText(MainActivity.this, "New clicked", Toast.LENGTH_SHORT).show();
                        return true;
                    case R.id.item_open:
                        Toast.makeText(MainActivity.this, "Open clicked", Toast.LENGTH_SHORT).show();
                        return true;
                    case R.id.item_save:
                        Toast.makeText(MainActivity.this, "Save clicked", Toast.LENGTH_SHORT).show();
                        return true;
                    default:
                        return false;
                }
            }
        });
        popupMenu.show();
    }
});
```

In this example, the **popup menu** is displayed when the user clicks the button, and different actions (**New, Open, Save**) are triggered based on the selected menu item.  

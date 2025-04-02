
# RecyclerView in Android  

RecyclerView is an advanced and flexible version of **ListView** used to display large data sets efficiently. It allows **reusing views** that are no longer visible to save memory and improve performance.  

## Key Features of RecyclerView:  
- ✅ **Efficient Memory Usage**: Reuses item views using the **ViewHolder** pattern.  
- ✅ **Flexible Layout Managers**: Supports **Linear, Grid, and Staggered Grid** layouts.  
- ✅ **Animations**: Built-in support for **item animations**.  
- ✅ **Customizability**: Allows **multiple view types** and **decorations**.  

---

## 1. Anatomy of a RecyclerView  

A **RecyclerView** setup consists of:  
- **Layout File**: Contains the RecyclerView widget.  
- **Adapter**: Binds data to the RecyclerView.  
- **ViewHolder**: Holds references to views for each item.  
- **Layout Manager**: Manages the layout of items (**Linear, Grid, etc.**).  
- **Data Source**: The data to be displayed.  

---

## 2. Step-by-Step Implementation of RecyclerView  

### **Step 1: XML Layout for RecyclerView**  

#### **`activity_main.xml`**  
This layout file contains the RecyclerView widget.  

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:padding="16dp" />
</RelativeLayout>
```

---

### **Step 2: Create a Layout for Each Item**  

#### **`item_layout.xml`**  
This defines the structure of each item in the RecyclerView.  

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="8dp"
    android:background="@android:color/white"
    android:elevation="2dp">

    <TextView
        android:id="@+id/itemTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Title"
        android:textSize="18sp"
        android:textColor="@android:color/black"
        android:padding="4dp" />

    <TextView
        android:id="@+id/itemSubtitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Subtitle"
        android:textSize="14sp"
        android:textColor="@android:color/darker_gray"
        android:padding="4dp" />

</LinearLayout>
```

---

### **Step 3: Create a Model Class**  

#### **`Item.java`**  
A simple model class to represent data for each RecyclerView item.  

```java
public class Item {
    private String title;
    private String subtitle;

    public Item(String title, String subtitle) {
        this.title = title;
        this.subtitle = subtitle;
    }

    public String getTitle() {
        return title;
    }

    public String getSubtitle() {
        return subtitle;
    }
}
```

---

### **Step 4: Create an Adapter for RecyclerView**  

#### **`MyAdapter.java`**  
The Adapter binds data to the RecyclerView and creates ViewHolders.  

```java
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;
import androidx.annotation.NonNull;
import androidx.recyclerview.widget.RecyclerView;
import java.util.List;

public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder> {

    private List<Item> items;

    public MyAdapter(List<Item> items) {
        this.items = items;
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.item_layout, parent, false);
        return new ViewHolder(view);
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        Item currentItem = items.get(position);
        holder.title.setText(currentItem.getTitle());
        holder.subtitle.setText(currentItem.getSubtitle());
    }

    @Override
    public int getItemCount() {
        return items.size();
    }

    public static class ViewHolder extends RecyclerView.ViewHolder {
        TextView title, subtitle;

        public ViewHolder(@NonNull View itemView) {
            super(itemView);
            title = itemView.findViewById(R.id.itemTitle);
            subtitle = itemView.findViewById(R.id.itemSubtitle);
        }
    }
}
```

---

### **Step 5: Implement RecyclerView in MainActivity**  

#### **`MainActivity.java`**  
The main activity initializes RecyclerView, sets the LayoutManager, and binds the Adapter.  

```java
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;
import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        RecyclerView recyclerView = findViewById(R.id.recyclerView);
        recyclerView.setLayoutManager(new LinearLayoutManager(this));

        // Sample data for RecyclerView
        List<Item> itemList = new ArrayList<>();
        itemList.add(new Item("Title 1", "Subtitle 1"));
        itemList.add(new Item("Title 2", "Subtitle 2"));
        itemList.add(new Item("Title 3", "Subtitle 3"));
        itemList.add(new Item("Title 4", "Subtitle 4"));

        // Setting the adapter
        MyAdapter adapter = new MyAdapter(itemList);
        recyclerView.setAdapter(adapter);
    }
}
```

---

### **Step 6: Build and Run**  

#### **Add RecyclerView dependency to your `build.gradle` file:**  
```gradle
implementation 'androidx.recyclerview:recyclerview:1.3.1'
```

---

### **Run the App**  
✅ Add sample data and run the app to see the **RecyclerView displaying a list of items**! 🚀  

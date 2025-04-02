# User Input Controls in Android  

User input controls are essential for gathering data from users, allowing them to interact with an application. Android provides a variety of input controls, including buttons, text fields, checkboxes, radio buttons, and spinners, that developers can use to create interactive forms and UIs. These controls are crucial in ensuring that users can input data and provide responses in a structured way.  

## 1. Text Input Controls  
Text input controls allow users to enter text, which is one of the most common forms of user input in mobile applications.  

#### EditText: Used for entering single-line or multi-line text.  
- **Single-line Input:** Used for input fields like name or email.  
- **Multi-line Input:** Used for text areas like message boxes or comments.  

#### Example - EditText (Single-line Input):  
```xml
<EditText
    android:id="@+id/editText"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Enter your name"
    android:inputType="text" />
```

#### Example - EditText (Multi-line Input):  
```xml
<EditText
    android:id="@+id/editTextMessage"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Enter your message"
    android:inputType="textMultiLine"
    android:minHeight="100dp"
    android:gravity="top|start" />
```

#### Handling Text Input Programmatically:  
You can listen for text changes in an `EditText` using `TextWatcher`.  
```java
EditText editText = findViewById(R.id.editText);
editText.addTextChangedListener(new TextWatcher() {
    @Override
    public void beforeTextChanged(CharSequence charSequence, int start, int count, int after) {}

    @Override
    public void onTextChanged(CharSequence charSequence, int start, int before, int count) {}

    @Override
    public void afterTextChanged(Editable editable) {
        // Handle the text input here
    }
});
```

## 2. Buttons  
Buttons are widely used in Android to trigger actions when tapped by the user.  

#### Example - Button in XML:  
```xml
<Button
    android:id="@+id/buttonSubmit"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Submit" />
```

#### Handling Button Click Programmatically:  
```java
Button button = findViewById(R.id.buttonSubmit);
button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        // Handle button click
        Toast.makeText(MainActivity.this, "Button clicked", Toast.LENGTH_SHORT).show();
    }
});
```

## 3. Radio Buttons  
Radio buttons allow the user to select one option from a predefined set of choices.  

#### Example - RadioButtons in XML:  
```xml
<RadioGroup
    android:id="@+id/radioGroupGender"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content">

    <RadioButton
        android:id="@+id/radioMale"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Male" />

    <RadioButton
        android:id="@+id/radioFemale"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Female" />
</RadioGroup>
```

#### Handling RadioButton Selection Programmatically:  
```java
RadioGroup radioGroup = findViewById(R.id.radioGroupGender);
radioGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(RadioGroup group, int checkedId) {
        if (checkedId == R.id.radioMale) {
            // Handle Male selection
        } else if (checkedId == R.id.radioFemale) {
            // Handle Female selection
        }
    }
});
```

## 4. Checkboxes  
Checkboxes allow users to select multiple options from a set.  

#### Example - Checkbox in XML:  
```xml
<CheckBox
    android:id="@+id/checkboxAgree"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="I agree to the terms and conditions" />
```

#### Handling Checkbox State Programmatically:  
```java
CheckBox checkBox = findViewById(R.id.checkboxAgree);
checkBox.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
        if (isChecked) {
            // Checkbox is checked
        } else {
            // Checkbox is unchecked
        }
    }
});
```

## 5. Spinners (Drop-down Lists)  
A Spinner is a UI component that allows users to select one item from a drop-down list.  

#### Example - Spinner in XML:  
```xml
<Spinner
    android:id="@+id/spinnerCities"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

#### Handling Spinner Selection Programmatically:  
```java
Spinner spinner = findViewById(R.id.spinnerCities);
ArrayAdapter<CharSequence> adapter = ArrayAdapter.createFromResource(this,
        R.array.cities_array, android.R.layout.simple_spinner_item);
adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
spinner.setAdapter(adapter);

spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
    @Override
    public void onItemSelected(AdapterView<?> parentView, View selectedItemView, int position, long id) {
        // Handle the item selection
        String selectedCity = parentView.getItemAtPosition(position).toString();
    }

    @Override
    public void onNothingSelected(AdapterView<?> parentView) {}
});
```

## 6. SeekBar (Slider)  
A SeekBar allows the user to select a value from a range using a slider.  

#### Example - SeekBar in XML:  
```xml
<SeekBar
    android:id="@+id/seekBarVolume"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:max="100" />
```

#### Handling SeekBar Changes Programmatically:  
```java
SeekBar seekBar = findViewById(R.id.seekBarVolume);
seekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
    @Override
    public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
        // Handle the progress change
        int volume = progress;
    }

    @Override
    public void onStartTrackingTouch(SeekBar seekBar) {}

    @Override
    public void onStopTrackingTouch(SeekBar seekBar) {}
});
```

## 7. ToggleButton  
A ToggleButton is a two-state button (on/off).  

#### Example - ToggleButton in XML:  
```xml
<ToggleButton
    android:id="@+id/toggleButton"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textOn="Enabled"
    android:textOff="Disabled" />
```

#### Handling ToggleButton State Programmatically:  
```java
ToggleButton toggleButton = findViewById(R.id.toggleButton);
toggleButton.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
        if (isChecked) {
            // Toggle button is ON
        } else {
            // Toggle button is OFF
        }
    }
});
```

# 이벤트처리 onClickListener 구현
```
1. Activity에서 이벤트 리스너 implements해서 사용
2. 익명 클래스를 생성하여 이벤트 리스너
3. 생성해 놓은 익명 클래스를 참조하는 이벤트 리스너
4. layout/activity_main.xml 에서 Button의 onClick 속성사용
```

> byungchulkang님 블로그
## activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.byungchulkang.clickapplication.MainActivity">

    <Button
        android:id="@+id/button1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"
        android:layout_marginLeft="31dp"
        android:layout_marginStart="31dp"
        android:layout_marginTop="23dp"
        android:text="Button1" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignLeft="@+id/button1"
        android:layout_alignStart="@+id/button1"
        android:layout_below="@+id/button1"
        android:layout_marginTop="29dp"
        android:text="Button2" />

    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignLeft="@+id/button2"
        android:layout_alignStart="@+id/button2"
        android:layout_below="@+id/button2"
        android:layout_marginTop="25dp"
        android:text="Button3" />

    <Button
        android:id="@+id/button4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignLeft="@+id/button3"
        android:layout_alignStart="@+id/button3"
        android:layout_below="@+id/button3"
        android:layout_marginTop="36dp"
        android:text="Button4"
        android:onClick="onClickBtn4"/>
</RelativeLayout>
```

## MainActivity.java
```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private Button button1;
    private Button button2;
    private Button button3;
    private Button button4;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        button1 = (Button)findViewById(R.id.button1);
        button2 = (Button)findViewById(R.id.button2);
        button3 = (Button)findViewById(R.id.button3);
        button4 = (Button)findViewById(R.id.button4);

        // 1.Activity에서 이벤트 리스너 implements해서 사용
        button1.setOnClickListener(this);

        // 2. 익명 클래스를 생성하여 이벤트 리스너
        button2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view){
                button2.setText("Button2 Click!");
            }
        });

        // 3. 생성해 놓은 익명 클래스를 참조하는 이벤트 리스너
        View.OnClickListener onClickListener = new View.OnClickListener() {
            @Override
            public void onClick(View view){
                switch (view.getId()) {
                    case R.id.button3 :
                        button3.setText("Button3 Click!");
                        break;
                }
            }
        };
        button3.setOnClickListener(onClickListener);
    }

    @Override
    public void onClick(View view) {
        button1.setText("Button1 Click!");
    }

    // 4. layout/activity_main.xml 에서 Button의 onClick 속성사용
    public void onClickBtn4(View view) {
        button4.setText("Button4 Click!");
    }
}
```

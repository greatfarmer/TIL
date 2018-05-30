# SQLite3

## 안드로이드: SQLite3 사용예제1
```
SQLite 를 활용한 자료 저장을 해보도록 하겠습니다.

안드로이드에는 모바일용으로 제작된 경량화된 DB인 SQLite3 가 있습니다.
C 언어로 엔진이 제작되어 가볍,고  안드로이드에선 SQLiteOpenHelper 클래스를 제공하여 이를 쉽게 다룰수 있게 합니다.

본 예제에선 SQLiteOpenHelper 를 상속한 별도의 클래스를 MySQLiteOpenHelper 를 만들고
MainActivity에서 이를 활용하여 테이블을 생성하고 insert / update / delete 를 해보겠습니다.
특별히 화면에는 보여주는건 없고, Log.d 를 사용하여 결과만 확인해보겠습니다.
```

### MySQLiteOpenHelper.java
```java
import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteDatabase.CursorFactory;
import android.database.sqlite.SQLiteOpenHelper;

public class MySQLiteOpenHelper extends SQLiteOpenHelper {
    // 안드로이드에서 SQLite 데이터 베이스를 쉽게 사용할 수 있도록 도와주는 클래스
    public MySQLiteOpenHelper(Context context, String name,
                              CursorFactory factory, int version) {
        super(context, name, factory, version);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        // 최초에 데이터베이스가 없을경우, 데이터베이스 생성을 위해 호출됨
        // 테이블 생성하는 코드를 작성한다
        String sql = "create table mytable(id integer primary key autoincrement, name text);";
        db.execSQL(sql);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        // 데이터베이스의 버전이 바뀌었을 때 호출되는 콜백 메서드
        // 버전 바뀌었을 때 기존데이터베이스를 어떻게 변경할 것인지 작성한다
        // 각 버전의 변경 내용들을 버전마다 작성해야함
        String sql = "drop table mytable;"; // 테이블 드랍
        db.execSQL(sql);
        onCreate(db); // 다시 테이블 생성
    }
}
```

### MainActivity.java
```java
public class MainActivity extends AppCompatActivity {

    private MySQLiteOpenHelper helper;
    String dbName = "st_file.db";
    int dbVersion = 1; // 데이터베이스 버전
    private SQLiteDatabase db;
    String tag = "SQLite"; // Log 에 사용할 tag

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

// sqLite3 : 모바일 용으로 제작된 경량화 DB
//         C언어로 엔진이 제작되어 가볍다
// 안드로이드에서 sqLite3 를 쉽게 사용할 수 있도록 SQLiteOpenHelper클래스제공
        helper = new MySQLiteOpenHelper(
                this,  // 현재 화면의 제어권자
                dbName,// db 이름
                null,  // 커서팩토리-null : 표준커서가 사용됨
                dbVersion);       // 버전

        try {
//         // 데이터베이스 객체를 얻어오는 다른 간단한 방법
//         db = openOrCreateDatabase(dbName,  // 데이터베이스파일 이름
//                          Context.MODE_PRIVATE, // 파일 모드
//                          null);    // 커서 팩토리
//
//         String sql = "create table mytable(id integer primary key autoincrement, name text);";
//        db.execSQL(sql);

            db = helper.getWritableDatabase(); // 읽고 쓸수 있는 DB
            //db = helper.getReadableDatabase(); // 읽기 전용 DB select문
        } catch (SQLiteException e) {
            e.printStackTrace();
            Log.e(tag, "데이터베이스를 얻어올 수 없음");
            finish(); // 액티비티 종료
        }

        insert (); // insert 문 - 삽입추가

        select(); // select 문 - 조회

        update(); // update 문 - 수정변경

        delete(); // delete 문 - 삭제 행제거

        select();
    } // end of onCreate

    void delete() {
        db.execSQL("delete from mytable where id=2;");
        Log.d(tag, "delete 완료");
    }

    void update() {
        db.execSQL("update mytable set name='Park' where id=5;");
        Log.d(tag, "update 완료");
    }

    void select() {
        Cursor c = db.rawQuery("select * from mytable;", null);
        while(c.moveToNext()) {
            int id = c.getInt(0);
            String name = c.getString(1);
            Log.d(tag,"id:"+id+",name:"+name);
        }
    }

    void insert () {
        db.execSQL("insert into mytable (name) values('Seo');");
        db.execSQL("insert into mytable (name) values('Choi');");
        db.execSQL("insert into mytable (name) values('Park');");
        db.execSQL("insert into mytable (name) values('Heo');");
        db.execSQL("insert into mytable (name) values('Kim');");
        Log.d(tag, "insert 성공~!");
    }
} // end of class
```

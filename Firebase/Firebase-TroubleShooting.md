# Firebase TroubleShooting

## 문제
Java Admin SDK를 사용하여 Firebase의 Realtime Database에 데이터를 입력했을 때,
정상적으로 입력이 되지 않았던 문제

## 해결
Java 클래스의 main함수를 통해 실행시킬 경우, Firebase의 Realtime DB에 수신되기 전에 main함수가 종료되어 버리는 것이 가장 큰 원인.
<br>
덧붙여 Firebase의 리스너는 비동기방식이므로, Firebase 서버와 통신하기위해서 자체 데몬 스레드를 관리해야한다.
<br>
따라서, Java 클래스에서는 Firebase의 리스너가 트리거될 때 까지 아래 코드와 같이 latch로 메인 스레드를 차단하여 리스너를 기다리는 방식으로 문제를 해결했다.

> 참고 <br>
https://stackoverflow.com/questions/44548932/grab-data-from-firebase-with-java
https://stackoverflow.com/questions/46906163/how-to-write-data-to-firebase-with-a-java-program

### 소스 코드
```java
// FirebasePut.java
package site.corin2.chatting;

import java.io.FileInputStream;
import java.io.IOException;
import java.util.concurrent.CountDownLatch;

import com.google.auth.oauth2.GoogleCredentials;
import com.google.firebase.FirebaseApp;
import com.google.firebase.FirebaseOptions;
import com.google.firebase.database.DataSnapshot;
import com.google.firebase.database.DatabaseError;
import com.google.firebase.database.DatabaseReference;
import com.google.firebase.database.FirebaseDatabase;
import com.google.firebase.database.ValueEventListener;

import site.corin2.model.User;

public class FirebasePut {
        private static final String SERVICE_ACCOUNT_JSON = "D://testcorin2-firebase-adminsdk.json";
        private static final String DATABASE_URL = "https://testcorin2.firebaseio.com/";

        private static DatabaseReference database;

        public static void addUsers() {
                DatabaseReference ref = database.child("user");
                ref.child("user01").setValueAsync((new User("test@gmail.com", "test.jpg", "test")));
        }

        public static void startListeners() {
                final CountDownLatch latch = new CountDownLatch(1); // important!
                database.addValueEventListener(new ValueEventListener() {
                        @Override
                        public void onDataChange(DataSnapshot snapshot) {
                                Object document = snapshot.getValue();
                                System.out.println("document: " + document);
                                latch.countDown(); // important!
                        }
                        @Override
                        public void onCancelled(DatabaseError error) {
                                System.out.println("Error: " + error.getMessage());
                                latch.countDown(); // important!
                        }
                });
                try {
                        latch.await(); // important!
                } catch (Exception e) {
                        e.printStackTrace();
                }
        }

        public static void main(String[] args) {
        // Initialize Firebase
        try {
                // [START initialize]
                FileInputStream serviceAccount = new FileInputStream(SERVICE_ACCOUNT_JSON);
                FirebaseOptions options = new FirebaseOptions.Builder()
                        .setCredentials(GoogleCredentials.fromStream(serviceAccount))
                    .setDatabaseUrl(DATABASE_URL)
                    .build();
                FirebaseApp.initializeApp(options);
                // [END initialize]
        } catch (IOException e) {
                e.getStackTrace();
        }

        // Shared Database reference
        database = FirebaseDatabase.getInstance().getReference();

        // Add Users
        addUsers();

        // Start listening to the Database
        startListeners();
        }
}
```

```java
// User.java
package site.corin2.model;

public class User {
    public String email;
    public String profileImg;
    public String userName;

    public User() {
    }

    public User(String email, String profileImg, String userName) {
        this.email = email;
        this.profileImg = profileImg;
        this.userName = userName;
    }
}
```

### Firebase Realtime Database 규칙
테스트 코드용으로 권한을 설정하지 않았다. (실 사용시, 반드시 권한 설정)
```json
{
  "rules": {
    ".read": "auth == null",
    ".write": "auth == null"
  }
}
```

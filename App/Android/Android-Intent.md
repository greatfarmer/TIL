# 인텐트(Intent)
```
액티비티 끼리 또는 서비스, BR과 통신을 하려면 인텐트를 사용하여야 한다.
다시 말하면 인텐트를 시스템으로 먼저 전달하여야 다른 구성요소와 통신이 가능하다.

다음은 인텐트의 생성자이다.
액션과 데이터를 인수로 할 수도 있고, 클래스 객체로도 인수를 할 수 있다.
Intent()
Intent(Intent o)
Intent(String action)
Intent(String action, Uri uri)
Intent(Context packageContext, Class<?> cls)
Intent(String action, Uri uri, Context packageContext, Class<?> cls)

인텐트를 전달하는 대표적인 메서드
startActivity()
startService 또는 bindService()
bloadcastingIntent()

1. 암시적 인텐트
액션이나 데이터를 지정하므로 대상이 달라질 수 있는 경우
예로 Intent(String action, Uri uri);
암시적 인텐트는 액션과 데이터로 구성되지만 추가적인 속성이 있다.
범주9Category), 타입(Type), 컴포넌트(Component), 부가데이터(Extra) 등이다.

2. 명시적 인텐트
클래스나 구성요소 이름을 지정하여 호출 대상을 확실하게 하는 경우
예로 Intent(Context packageContext, Class<?> cls);

3. 부가데이터(Extra) 함께 보낼 수도 있다.
```

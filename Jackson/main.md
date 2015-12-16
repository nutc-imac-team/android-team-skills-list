# Jaskson
### Development Environment
  - Android Studio
### Built Environment
  - 開啟 Android Srudio
  - 建立新的 project 叫做 Gson
  - 開啟 Gson 的 build.gradle(Module:app)，在 dependencies 區塊加入，例：
```java
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:22.2.0'

    //compile 代表會在應用程式執行階段，使用到這個函式庫
    //目前最新版本2.5
    compile 'com.fasterxml.jackson.core:jackson-annotations:2.7.0-rc1'
    compile 'com.fasterxml.jackson.core:jackson-core:2.7.0-rc1'
    compile 'com.fasterxml.jackson.core:jackson-databind:2.7.0-rc1'
}
```
  - 引入library裡的 NOTICE、LICENSE會產生衝突，解決辦法，例:
```java
 packagingOptions {
        exclude 'META-INF/LICENSE'
        exclude 'META-INF/NOTICE'
    }
```
### The Simplest Sample
```java
public class MainActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        try {
            //資料取值方法
            DataBinding();
            //樹狀取值方法
            TreeBinding();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private void DataBinding() throws IOException {
        //建立ObjectMapper
        ObjectMapper mapper = new ObjectMapper();
        ArrayList<String> interest = new ArrayList<>();
        interest.add("ride a bike");
        interest.add("sing a song");
        //值加入建構子
        Classmate classmate = new Classmate("Jerry", interest);
        //object to json in string
        String Json = mapper.writeValueAsString(classmate);
        System.out.println(Json);
        //json to java object
        classmate = mapper.readValue(Json, Classmate.class);
        System.out.println(mapper.writeValueAsString(classmate.getname()));
        System.out.println(mapper.writeValueAsString(classmate.getInterest()));
    }
    //建構子 of DataBinding
    //加static，讓程式執行時先抓到這個class
    static class Classmate {
        private String name;
        private ArrayList<String> interest;

        public Classmate(String name, ArrayList<String> interest) {
            this.name = name;
            this.interest = interest;
        }
        //傳值要加入一個空的建構子，避免錯誤
        public Classmate() {
        }

        public String getname() {
            return this.name;
        }

        public ArrayList<String> getInterest() {
            return this.interest;
        }
    }

    private void TreeBinding() throws IOException {
        //json of TreeBinding in string
        String classmateTree = "{\"classmate\":[{\"numberA\":\"Jerry\"},{\"numberB\":\"Ben\"},{\"numberC\":\"Jack\"}]}";
        String numberA = "";
        String numberB = "";
        String numberC = "";
        //建立ObjectMapper
        ObjectMapper mapper = new ObjectMapper();
        //建立JsonNode，拿到tree
        JsonNode root = mapper.readTree(classmateTree);
        //json 是陣列型態，先抓到子節點
        JsonNode classmeateNode = root.path("classmate");
        //path類似get，但找不到值會回傳空值，而不是null
        numberA = classmeateNode.get(0).path("numberA").asText();
        numberB = classmeateNode.get(1).path("numberB").asText();
        numberC = classmeateNode.get(2).path("numberC").asText();
        System.out.println("A:" + numberA);
        System.out.println("B:" + numberB);
        System.out.println("C:" + numberC);

    }
}
```
### Result
```java
DataBinding:
System.out﹕ {"interest":["ride a bike","sing a song"],"name":"Jerry"}
System.out﹕ "Jerry"
System.out﹕ ["ride a bike","sing a song"]

 TreeBinding:
System.out﹕ A:Jerry
System.out﹕ B:Ben
System.out﹕ C:Jack
```
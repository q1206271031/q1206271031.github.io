### 集合

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collection;

//集合
public class collection {
    public static void main(String[] args) {
        Collection<String> list = new ArrayList<>();
        System.out.println(list.size());
        System.out.println(list.isEmpty());
        list.add("a");
        list.add("b");
        list.add("c");
        System.out.println(list.size());
        System.out.println(list.isEmpty());
        Object[] array = list.toArray();
        System.out.println(Arrays.toString(array));
        for(String s:list){
            System.out.println(s);
        }
        System.out.println("------------------------------");
        list.remove("c");
        for(String s:list){
            System.out.println(s);
        }
        list.clear();
        System.out.println("------------------------------");
        System.out.println(list.size());
        System.out.println(list.isEmpty());
    }
}

```

### HashMap

```java
import java.util.HashMap;
import java.util.Map;
//哈希表（HashMap）
public class mapTest {
    public static void main(String[] args) {
        Map<String,String> map = new HashMap<>();
        System.out.println(map.size());
        System.out.println(map.isEmpty());
        System.out.println("----------------------------");
        map.get("作者");
        System.out.println(map.getOrDefault("作者","氐名"));
        System.out.println(map.containsValue("氐名")); //false

        System.out.println(map.getOrDefault("氐名","作者"));
        map.put("作者","鲁迅");
        map.put("著作","日记");
        map.put("时间","1983");
        System.out.println(map.getOrDefault("作者","氐名"));
        System.out.println(map.size());
        System.out.println(map.isEmpty());
        System.out.println(map.containsKey("作者"));
        for(Map.Entry<String,String> entry : map.entrySet()){
            System.out.println(entry.getKey() + " " + entry.getValue());
            System.out.println();
        }
    }
}

```

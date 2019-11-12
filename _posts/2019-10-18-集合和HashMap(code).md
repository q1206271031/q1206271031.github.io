---
layout:     post
title:      collection,Map
subtitle:   集合，表(哈希表)
date:       2019-10-18
author:     BY
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - java
---

![](https://github.com/q1206271031/photo/raw/master/%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6/%E9%9B%86%E5%90%88%E7%9A%84%E6%8E%A5%E5%8F%A3%E7%BB%A7%E6%89%BF%E5%85%B3%E7%B3%BB%E5%9B%BE.png)

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

### Map

***如果put没有的key值，返回null***

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
        //map.entrySet 将key和value打包
        for(Map.Entry<String,String> entry : map.entrySet()){
            System.out.println(entry.getKey() + " " + entry.getValue());
            System.out.println();
        }
    }
}

```

### 泛型

1.放的是集合
2.eg:boolean addAll(Collection<? extends E> c)
 放的是实例student->person，或者是E本身





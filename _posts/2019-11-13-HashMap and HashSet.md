---
layout:     post
title:      HashMap and HashSet
subtitle:   面试题
date:       2019-11-13
author:     BY
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - java
---

### 面试题

> 1.旧键盘 

[链接](https://www.nowcoder.com/questionTerminal/f88dafac00c8431fa363cd85a37c2d5e)

```java
    public class BrokenKeyBoard {
        // 先在 main 方法中写出基本的输入输出框架
        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            while (scanner.hasNext()) {
                // 1. 读取两个字符串
                //    expected 预期要输出的字符串 7_This_is_a_test
                //    actual 是实际输出的字符串  _hs_s_a_es
                //    核心思路是拿预期的内容去实际的内容中找
                String expected = scanner.next();
                String actual = scanner.next();
                // 2. 把两个字符串都改成大写
                expected = expected.toUpperCase();
                actual = actual.toUpperCase();
                // 3. 题目的主要任务判定 expected 中哪些字符在 actual 中不存在
                //    先搞一个 setActual 用来保存实际哪些字符输出了.
                Set<Character> setActual = new HashSet<>();
                for (int i = 0; i < actual.length(); i++) {
                    setActual.add(actual.charAt(i));
                }
                // 这个 setPrinted 是用于最终输出结果去重的. 已经被判定为坏的键不能输出两次
                // 输出过的坏键就放到这个 set 中
                Set<Character> setPrinted = new HashSet<>();
                // 4. [核心操作!] 然后使用 expected 来遍历, 判断这个字符是否在 actual 中存在
                for (int i = 0; i < expected.length(); i++) {
                    char c = expected.charAt(i);
                    if (setActual.contains(c)) {
                        // 如果当前字符已经实际输出了, 说明是好的键
                        continue;
                    }
                    // 如果当前字符 c 没有实际输出, 说明是坏的键
                    // 但是坏的键要看看之前是不是输出过, 如果曾经输出过, 也就不必输出
                    if (setPrinted.contains(c)) {
                        // 这是 c 曾经输出过
                        continue;
                    }
                    // 注意, 不要输出换行
                    System.out.print(c);
                    setPrinted.add(c);
                }  // end for
            }  // end while (scanner.hasNext())
        }  // end main
```        

> 2.前K个高频单词

[链接](https://leetcode-cn.com/problems/top-k-frequent-words/description/)

```java
public List<String> topKFrequent(String[] words, int k) {
        // 1. 统计每个单词出现的次数, Map 来搞定
        Map<String, Integer> map = new HashMap<>();
        for (String w : words) {
            int count = map.getOrDefault(w, 0);
            map.put(w, count + 1);
        }
        // 2. 把这些单词放在一个 ArrayList 中, 并进行排序
        //    自定制比较规则
        List<String> result = new ArrayList<>(map.keySet());
        // 在 sort 方法的第二个参数中指定一个比较器对象
        // 来描述自定制比较规则(按出现次数来排序)
        Collections.sort(result, new MyComp(map));
        // subList 能够返回一个 List 中的一个子区间
        // [0, k) 前闭后开区间
        return result.subList(0, k);
    }

    class MyComp implements Comparator<String> {
        private Map<String, Integer> map = null;

        public MyComp(Map<String, Integer> map) {
            this.map = map;
        }

        @Override
        public int compare(String o1, String o2) {
            int count1 = map.get(o1);
            int count2 = map.get(o2);
            if (count1 == count2) {
                // 就按字符串自身的大小来决定先后顺序
                return o1.compareTo(o2);
            }
            // 降序排序写成 count2 - count1
            // 升序排序写成 count1 - count2
            return count2 - count1;
        }
    }
```

> 3.只出现一次的数字

[链接](https://leetcode-cn.com/problems/single-number/submissions/)

```java
public int singleNumber(int[] nums) {
        //key某个数字
        //value这个数字出现的次数
        Map<Integer,Integer> map = new HashMap<>();
        //可迭代对象才能使用for:each循环（实现了Comparable接口）
        for(int x: nums){
            int count = map.getOrDefault(x,0);
            map.put(x,count + 1);
        }
        //这个循环结束后，相当于把每个数字出现的次数都统计好了
        for(Map.Entry<Integer,Integer> entry : map.entrySet()){
            if(entry.getValue() == 1){
                return entry.getKey();
            }
        }
        return 0;
    }

    //方法二
    public int singleNumber2(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int result = 0;
        for (int x : nums) {
            result ^= x;
        }
        return result;
    }
```

> 4.复制带随机指针的链表

[](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

```java
static class Node {
        public int val;
        public Node next;
        public Node random;

        public Node() {}

        public Node(int _val,Node _next,Node _random) {
            val = _val;
            next = _next;
            random = _random;
        }
    };

    public Node copyRandomList(Node head){
        // 1. 先创建一个 HashMap , key 是旧的节点, value 是旧的节点对应的新的节点
        HashMap<Node, Node> hashMap = new HashMap<>();
        for (Node cur = head; cur != null; cur = cur.next) {
            // 2. 把旧链表的节点依次以键值对的形式插入进来
            hashMap.put(cur, new Node(cur.val, null, null));
        }
        // 3. 再次遍历旧的链表, 根据刚才得到的 hashMap
        // 就能够方便的把 next 和 random 都指到正确的位置上
        for (Node cur = head; cur != null; cur = cur.next) {
            hashMap.get(cur).next = hashMap.get(cur.next);
            hashMap.get(cur).random = hashMap.get(cur.random);
        }
        return hashMap.get(head);
    }
```

> 5.宝石与石头

[链接](https://leetcode-cn.com/problems/jewels-and-stones/)

```java
public int numJewelsInStones(String J, String S) {
        // 1. 创建一个 Set 来保存所有的宝石
        Set<Character> set = new HashSet<>();
        // 2. 遍历 J , 把所有的宝石加入到 Set 中.
        for (int i = 0; i < J.length(); i++) {
            set.add(J.charAt(i));
        }
        // 3. 遍历 S, 取出每个字符, 看看是不是宝石
        //    如果是宝石, 就 count += 1
        int count = 0;
        for (int i = 0; i < S.length(); i++) {
            if (set.contains(S.charAt(i))) {
                count++;
            }
        }
        return count;
    }
```






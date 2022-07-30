

# 经典帖子

[Java 性能优化的 55 个细节（珍藏版）](https://baijiahao.baidu.com/s?id=1714704029456338725&wfr=spider&for=pc)



[Duplicate Objects in Java: Not Just Strings - DZone Java](https://dzone.com/articles/duplicate-objects-in-java-not-just-strings)

[Duplicate Strings: How to Get Rid of Them and Save Memory - DZone Performance](https://dzone.com/articles/duplicate-strings-how-to-get-rid-of-them-and-save)

[JVM Anatomy Quark #10: String.intern()](https://shipilev.net/jvm/anatomy-quarks/10-string-intern/)

[Java中的重复对象：不仅仅是字符串 - 简书](https://www.jianshu.com/p/0b658fdd1c84)

[(26条消息) Java中的重复对象：不仅仅是字符串\_weixin\_43923408的博客-CSDN博客](https://blog.csdn.net/weixin_43923408/article/details/87990199)



# 高效编码实践

## 1、使用set的contains替代list的contains

```java
public class Main {
    public static void main(String[] args) {
        List<Integer> list1 = new ArrayList<>();
        for (int i = 0; i < 1000000; i++) {
            list1.add(i);
        }

        List<Integer> list2 = new ArrayList<>();
        for (int i = 100000; i < 300000; i++) {
            list2.add(i);
        }

        long start = System.currentTimeMillis();
        System.out.println("start:" + start);
        for (Integer value : list2) {
            if (!list1.contains(value)) {
                System.out.println("not in list1");
            }
        }
        System.out.println("cost="+(System.currentTimeMillis()-start));

        long start2 = System.currentTimeMillis();
        System.out.println("start:" + start2);
        Set<Integer> set1 = new HashSet<>(list1);
        for (Integer integer : list2) {
            if (!set1.contains(integer)) {
                System.out.println("not in list1");
            }
        }
        System.out.println("cost="+(System.currentTimeMillis()-start2));
    }
}
```



运行结果如下：

```sh
start:1658756890198
cost=17824
start:1658756908022
cost=71
```

## 2、从十万数据中移除已选数据

方法: 将要删除的对象先放入HashSet

如下优化前的代码，效率不高

```java
        List<Integer> list1 = new ArrayList<>();
        for (int i = 0; i < 1000000; i++) {
            list1.add(i);
        }

        List<Integer> list2 = new ArrayList<>();
        for (int i = 100000; i < 300000; i++) {
            list2.add(i);
        }

        long start = System.currentTimeMillis();
        System.out.println("start:" + start);
        list1.removeAll(list2);
        System.out.println("list1.size="+list1.size());
        System.out.println("cost="+(System.currentTimeMillis()-start));
```

优化后的代码

```java
        long start2 = System.currentTimeMillis();
        System.out.println("start:" + start2);
        Set<Integer> set2 = new HashSet<>(list2);
        list1.removeAll(set2);
        System.out.println("list1.size="+list1.size());
        System.out.println("cost="+(System.currentTimeMillis()-start2));
```

效率差别：

```sh
# 优化前
start:1658758598646
list1.size=800000
cost=71021
# 优化后
start:1658758495486
list1.size=800000
cost=26
```





# 内存瘦身实践

## String.intern减少字符串重复造成的内存占用

测试程序如下

```java
    public static void main(String[] args) throws InterruptedException{
        long start = System.currentTimeMillis();
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 10000000; i++) {
            list.add(new String("bbbbb")); // 这里回在堆生成很多个重复的String对象
        }
        System.out.println("cost=" + (System.currentTimeMillis()-start));

        Thread.sleep(1000000);  // 为了方便jps查看内存
    }
```

以上代码，由于创建了10000000个String对象，耗时达5240毫秒，（输出结果  cost=5240）内存占用达506452K。（内存查看方法：由于程序会sleep住，此时直接cmd中jps查看java进程ID，然后在windows资源管理器中查看该PID的内存即可）

若将for循环中的 list.add(new String("bbbbb"));修改为list.add("bbbbb");此时执行时间和内存占用都有改善

输出结果  cost=90

同时内存占用：144512K


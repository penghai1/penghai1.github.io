

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

185296

182608

997008k





31596k


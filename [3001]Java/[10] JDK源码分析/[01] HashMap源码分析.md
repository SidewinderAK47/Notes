JDK1.7 源码分析:



Hashmap扩容的目的是什么呢？

扩容的目的：为了链表变短； 使得get和put效率提高；

直接转移：根据长度，计算的下表不一样；

```java
        HashMap<String,String> hashMap=new HashMap<String,String>(10,0.75f);
        String res1 = hashMap.put("1","1");//key ---> key.hasCode() 数字48690 ----> 48690% table.length ----> index (下表：会发生哈希碰撞)
        String res2 = hashMap.put("2","2");
        System.out.println(res1+" "+res2);
        Iterator var4 = hashMap.keySet().iterator();

        while(var4.hasNext()) {
            String key = (String)var4.next();
            if (key.equals("1")) {
                hashMap.remove(key);
            }
        }
```

   \* 修改次数，包括put,get,remove

   \* 快速失败机制？ 认为你失败；不让你执行下去了；

   \* 比如一个线程在遍历，一个线程在修改；

```output
Exception in thread "main" java.util.ConcurrentModificationException
Disconnected from the target VM, address: '127.0.0.1:5864', transport: 'socket'
	at java.util.HashMap$HashIterator.nextNode(HashMap.java:1445)
	at java.util.HashMap$KeyIterator.next(HashMap.java:1469)
	at org.language.ExceptionDemo.main(ExceptionDemo.java:42)
```

关键在于能否忍受 线程不安全问题；




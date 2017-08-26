---
layout: post
title: HashMap源码分析
---
### 构造函数

```java
	// 使用了一个初始化容量 (赋值给成员变量: threshold ) 和 一个loadFactor.
    public HashMap(int initialCapacity, float loadFactor)
	// 使用Map直接构造
	public HashMap(Map<? extends K, ? extends V> m)
	// 默认的情况下. initialCapacity为16, loadFactor= 0.75f
	public HashMap()
```


### Entry
key value 的一个对象 构造方法为:

````java 
	/**
	 * Creates new entry.
	 */
	Entry(int h, K k, V v, Entry<K,V> n) {
	    value = v;
	    next = n;
	    key = k;
	    hash = h;
	}
````
从Entry的构造方法看到, 有个next, 这个下面在解释.
先看看addEntry方法

````java
    /**
     * Adds a new entry with the specified key, value and hash code to
     * the specified bucket.  It is the responsibility of this
     * method to resize the table if appropriate.
     *
     * Subclass overrides this to alter the behavior of put method.
     */
    void addEntry(int hash, K key, V value, int bucketIndex) {
        if ((size >= threshold) && (null != table[bucketIndex])) {
            resize(2 * table.length);
            hash = (null != key) ? hash(key) : 0;
            bucketIndex = indexFor(hash, table.length);
        }

        createEntry(hash, key, value, bucketIndex);
    }
````
添加一个Entry时候, 如果size >= (initialCapacity * loadFactor) 并且当前的位置不为null, 则会重新分配hashMap的元素.

````java
    void createEntry(int hash, K key, V value, int bucketIndex) {
        Entry<K,V> e = table[bucketIndex];
        table[bucketIndex] = new Entry<>(hash, key, value, e);
        size++;
    }  
````
新的Entry会被添加到当前的数组的头的位置, 原有该位置的Entry添加到新Etrry的next.


### Hash

````java
	/**
	 * Returns index for hash code h.
	*/
	static int indexFor(int h, int length) {
		return h & (length-1);
	}
````
通过key的hash和长度相&得到key在table的位置. length为HashMap的table数组长度.




### 实现细节
>
	   使用构造函数默认构造是空表, 第一次put 或者 插入值的时候, 会对initialCapacity往上到Power2的值, 即2的倍率. 
	   然后把一个阈值(threshold)修改为 initialCapacity * loadFactor .
	   添加时候, 会判断size是否超出initialCapacity* loadFactor, 如果超出, 会对HashMap reSize.
	   通过hash 计算key,得到hash值, 计算后得到在table的index, 以链表结构存储.
	     


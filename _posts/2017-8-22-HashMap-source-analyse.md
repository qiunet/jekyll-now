---
layout: post
title: HashMap源码分析
---
### 构造函数
```java
	// 使用了一个初始化容量 和 一个loadFactor.
    public HashMap(int initialCapacity, float loadFactor)
	// 使用Map直接构造
	public HashMap(Map<? extends K, ? extends V> m)
	// 默认的情况下. initialCapacity为16, loadFactor= 0.75f
	public HashMap()
```


### Entry


### Hash
```java
/**
 * Returns index for hash code h.
*/
static int indexFor(int h, int length) {
	return h & (length-1);
}
```
通过该方法得到hash所在table的位置. length 为HashMap的table数组长度.

# HashMap 相关

## HashMap扩容过程怎么解决哈希冲突？

```java
// 扩容方法
final Node<K,V>[] resize() {
    // 保存原HashMap数据
    Node<K,V>[] oldTab = table;
    // 保存原HashMap桶容量
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    // 保存原HashMap的扩容阈值
    int oldThr = threshold;
    // 初始化新HashMap的桶容量和扩容阈值
    int newCap, newThr = 0;
    // 如果原HashMap存在数据
    if (oldCap > 0) {
        // 如果原HashMap桶容量大于等于最大：1 << 30，则将最新HashMap扩容阈值改为：2^31-1，并结束扩容流程
        if (oldCap >= MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return oldTab;
        }
        // 如果原HashMap桶容量*2 < 1 << 30 并且原桶容量>=16，则新HashMap的扩容阈值=HashMap的扩容阈值*2
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                    oldCap >= DEFAULT_INITIAL_CAPACITY)
            newThr = oldThr << 1; // double threshold
    }
    // 如果原HashMap不存在数据并且原HashMap扩容阈值>0，则新HashMap桶容量为原HashMap容量阈值
    else if (oldThr > 0) // initial capacity was placed in threshold
        newCap = oldThr;
    else {               // zero initial threshold signifies using defaults
        // oldCap <=0 && oldThr <= 0
        // 新HashMap桶容量=16&新HashMap扩容阈值=负载因子*16
        newCap = DEFAULT_INITIAL_CAPACITY;
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    //==============上面代码功能：初步确定新HashMap的桶容量和扩容阈值==================
    // 如果新HashMap扩容阈值等于0
    // 如果新HashMap桶容量小于 1 << 30 && 最新容量*负载因子小于 1 << 30，则新扩容阈值为：最新容量*负载因子，否则新扩容阈值为最大：2^31-1
    if (newThr == 0) {
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                    (int)ft : Integer.MAX_VALUE);
    }
    // 将最新的HashMap扩容阈值赋值给当前阈值变量
    threshold = newThr;
    @SuppressWarnings({"rawtypes","unchecked"})
    // 创建新的HashMap对象
    Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
    // 将新创建的HashMap对象赋值给当前HashMap
    table = newTab;
    // 如果原HashMap不为空
    if (oldTab != null) {
        // 根据桶下标遍历原HashMap记录
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
            // 如果当前HashMap桶下标value不为空，则备份对应的value，并清空原HashMap桶对象
            if ((e = oldTab[j]) != null) {
                oldTab[j] = null;
                // 如果当前桶下标不存在链表结构，则将对应下标的value迁移到新HashMap，且index=原hashCode & (新HashMap桶容量 - 1)
                if (e.next == null)
                    newTab[e.hash & (newCap - 1)] = e;
                // 如果当前桶下标存在链表（普通链表）    
                else if (e instanceof TreeNode)
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                else { // preserve order
                    // 循环遍历该下标的链表（红黑树）
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    do {
                        // 获取当前桶下标对象的next
                        next = e.next;
                        // 如果
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }
                        else {
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```


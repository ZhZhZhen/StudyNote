# SparseArray
- 用法与HashMap相同，不过key为int。每次扩容<=4时扩为8，之后翻倍扩容
- 实现：为了避免散列表产生的空间浪费，使用稀疏数组(指值不连续)来实现，分别用数组记录keys和values，通过gc()函数来把使用到的key值往数组前面移动(value也跟着位置改变)。这样key数组记录的便是紧凑的key值(如原稀释数组为[1,del,5,0,0,0]，紧凑后会变为[1,5,0,0,0,0]，value也会移动到相应位置)。
    - put():先通过二分查找查找元素，存在相同key值则替换value；否则判断key对应的value是否恰好为DELETE，是则替换value；否则判断是否需要gc()紧凑之后，通过函数插入(该函数会判断是否超过数组大小，超过会扩容后插入)。
    - get()：通过二分查找寻找key数组中是否存在对应key值，找到则通过该key值所在的数组下标去value数组中取值。找不到返回默认值。
    - delete()：二分查找对应key所在的下标，将对应下标的value标记为DELETE，并标记sparsearray为回收状态。该函数并不会使size减少，只有需要使用size时，才会通过gc()函数重新计算size。
    - gc()：通过循环将key[]和value[]的有效值尽可能往前移动(空值和DELETE值会被往后移动)，并维护size为有效的key的数量值。该方法在需要索引的地方，put()，append()时会调用一次
    - append()：优化插入key值大于现有key值的情况，当判断插入的key比现有的key都大时，会判断是否需要gc()和扩容，然后直接在key[size]/value[size]位插入新值。这里主要避免了二分查找效率低(因为插入这个最大值，二分查找会查找最大次数才发现没有该值)的问题
    - size()：因为删除的时候不会维护size值，所以该函数会通过gc()先把标记为DELETE的位置给紧凑后更新size，然后返回。
    - 所使用的二分法，在找到对应key后直接返回，未找到则在最后取反返回。因此put中没找到会将值取反(取反得到的是二分查找最后停留的那个位置)，判断value是否恰好为delete，是则替换key和value。
- 优点：避免了key的自动包装，紧凑的实现保证了内存不会过高。(虽然二分查找有时候效率较低，但是Android中数据并不会太多)。
- key为long可以使用LongSparseArray；key为Object可以使用ArrayMap来优化内存。
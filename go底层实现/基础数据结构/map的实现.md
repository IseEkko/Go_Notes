# Map的实现

在很多语言中都有这个map，也就是hashmap的使用，然后这里我们就来了解一下这个东西。

## Hash函数

hash函数简单的说就是一个东西，放进这个函数就会根据这个函数进行加密，然后映射到相应的地方

![image-20220517195113276](/Users/lizhongzheng/Library/Application Support/typora-user-images/image-20220517195113276.png)

在理想的状态下，我们hash的输入应该是远小于输出的，但是实际情况下肯定不是这样的，这个时候，我们就应该注意一下，这个时候我们就会产生hash冲突，也就是说两个数据生成的hash值是一样的，然后映射到了同一个地方，这个时候我们应该这么办呢。我们常用的方式有两种：

* 开放寻址法
* 拉链法

## 开放寻址法

这个开放寻址法，其实很简单，核心思想：依次探测和比较数组的元素，以判断目标键值对是否在数组中。

在出现hash冲突的时候，我们将出现冲突的数据按照数组的方向进行依次寻找，然后找到空位我们就将数据进行放入。

![image-20220517195632566](/Users/lizhongzheng/Library/Application Support/typora-user-images/image-20220517195632566.png)

但是呢，因为我们使用开放寻址法我们使用的是数组，所以数组的大小会越来越小，这时候我们的负载因子就会越来越大，然后这个时候hashmap的性能就会下降。到100%的时候，这个hashmap就完全失效了。

## 拉链法

拉链法其实是数组加上链表，首先我们在上层有一个数组，然后我们通过hash函数进行hash然后对hash值进行取模，然后对应数据的一个空格，我们这里称之为桶，然后将数据放入桶中，然后出现冲突的时候，我们直接将它放入链表的最后。

![image-20220517200024362](/Users/lizhongzheng/Library/Application Support/typora-user-images/image-20220517200024362.png)

## GO中map的实现

在go中map的使用是很广泛的

### 数据结构

```go
type hmap struct {
	count     int
	flags     uint8
	B         uint8
	noverflow uint16
	hash0     uint32

	buckets    unsafe.Pointer
	oldbuckets unsafe.Pointer
	nevacuate  uintptr

	extra *mapextra
}

type mapextra struct {
	overflow    *[]*bmap
	oldoverflow *[]*bmap
	nextOverflow *bmap
}
```

1. `count` 表示当前哈希表中的元素数量；
2. `B` 表示当前哈希表持有的 `buckets` 数量，但是因为哈希表中桶的数量都 2 的倍数，所以该字段会存储对数，也就是 `len(buckets) == 2^B`；
3. `hash0` 是哈希的种子，它能为哈希函数的结果引入随机性，这个值在创建哈希表时确定，并在调用哈希函数时作为参数传入；
4. `oldbuckets` 是哈希在扩容时用于保存之前 `buckets` 的字段，它的大小是当前 `buckets` 的一半；

### map的扩容机制

在使用map的时候，我们需要考虑到数据越来越多的时候，我们应该采取怎么样的扩容机制。

发生扩容机制的两种情况：

1. 当溢出桶太多的时候
2. 当负载因子太大的时候

#### 当溢出桶太多的时候

在溢出桶太多的时候，这个时候我们会创建桶，将数据拷贝过去。将原来溢出桶的数据进行一个迁移。

这里需要重新计算值，然后我们将之前的数据进行高位的比较就可以得到原来的值。

#### 当负载因子太大的时候

负载因子太大说明，要装满了，这个时候我们就会将桶数量扩展两倍。


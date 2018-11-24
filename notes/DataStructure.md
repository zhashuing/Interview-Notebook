<!-- GFM-TOC -->
* [一、数组](#一数组)
    * [Collection](#collection)
    * [Map](#map)
* [二、链表](#二链表)
    * [迭代器模式](#迭代器模式)
    * [适配器模式](#适配器模式)
* [三、栈](#三栈)
* [四、队列](#四队列)
* [五、递归](#[五递归)
* [六、二叉搜索树](#六二叉搜索树)
* [七、集合与映射](#七集合与映射)
* [八、堆](#八堆)
* [九、优先队列](#九优先队列)
* [十、线段树](#十线段树)
* [十一、Trie](#十一Trie)
* [十二、并查集](#十二并查集)
* [十三、AVL平衡树](#十三AVL平衡树)
* [十四、红黑树](#十四红黑树)
* [十五、哈希表](#十五哈希表)
* [附录](#附录)
* [参考资料](#参考资料)
<!-- GFM-TOC -->

# 一、数组
##　source code
public class DynamicArray<E> {

    /**
     * 泛型数组
     */
    private E[] data;
    /**
     * 数组的元素个数
     */
    private int size;
    /**
     * 数组有对泛型的至此,type变了用来保存泛型的类型
     */
    private Class type;

    // 构造函数，传入数组的容量capacity构造Array
    public DynamicArray(int capacity,Class type){
        data = (E[])Array.newInstance(type,capacity);
        size = 0;
    }

    //无参数的构造函数，默认数组的容量capacity=10
    public DynamicArray(Class type){
        this(10,type);
    }

    //获取数组的容量
    public int getCapacity(){
        return data.length;
    }

    //获取数组中的元素个数
    public int getSize(){
        return size;
    }

    //返回数组是否为空
    public boolean isEmpty(){
        return size == 0;
    }
    
    //向所有元素后添加一个新元素
    public void addLast(E e){
        add(size, e);
    }
    
    //在所有元素前添加一个新元素
    public void addFirst(E e){
        add(0, e);
    }

    //在index索引的位置插入一个新元素e
    public void add(int index, E e){

        if(size == data.length)
            throw new IllegalArgumentException("Add failed. Array is full.");

        if(index < 0 || index > size)
            throw new IllegalArgumentException("Add failed. Require index >= 0 and index <= size.");
        /*当数组空间不足时进行扩容*/
        if(size == data.length){
            resize(data.length * 2);
        }
        for(int i = size - 1; i >= index ; i --)
            data[i + 1] = data[i];
        data[index] = e;
        if((size == data.length / 4) && (data.length / 2 != 0)){
            resize(data.length / 2);
        }
        size ++;
    }

    //获取index索引位置的元素
    public E get(int index){
        if(index < 0 || index >= size)
            throw new IllegalArgumentException("Get failed. Index is illegal.");
        return data[index];
    }

    // 修改index索引位置的元素为e
    public void set(int index, E e){
        if(index < 0 || index >= size)
            throw new IllegalArgumentException("Set failed. Index is illegal.");
        data[index] = e;
    }

    // 查找数组中是否有元素e
    public boolean contains(E e){
        for(int i = 0 ; i < size ; i ++){
            if(data[i].equals(e))
                return true;
        }
        return false;
    }

    // 查找数组中元素e所在的索引,如果不存在元素e,则返回-1
    public int find(E e){
        for(int i = 0 ; i < size ; i ++){
            if(data[i].equals(e))
                return i;
        }
        return -1;
    }

    //从数组中删除index位置的元素, 返回删除的元素
    public E remove(int index){
        if(index < 0 || index >= size)
            throw new IllegalArgumentException("Remove failed. Index is illegal.");

        E ret = data[index];
        for(int i = index + 1 ; i < size ; i ++)
            data[i - 1] = data[i];
        size --;
        return ret;
    }

    //从数组中删除第一个元素, 返回删除的元素
    public E removeFirst(){
        return remove(0);
    }

    //从数组中删除最后一个元素, 返回删除的元素
    public E removeLast(){
        return remove(size - 1);
    }

    //从数组中删除元素e
    public void removeElement(E e){
        int index = find(e);
        if(index != -1)
            remove(index);
    }

    // 动态的进行数组的伸缩
    public void resize(int newCapacity){
        E[] newData = (E[])Array.newInstance(type,newCapacity);
        for (int i = 0; i < this.getSize(); i++) {
            newData[i] = data[i];
        }
        data = newData;
        newData = null;
    }

    @Override
    public String toString(){

        StringBuilder res = new StringBuilder();
        res.append(String.format("Array: size = %d , capacity = %d\n", size, data.length));
        res.append('[');
        for(int i = 0 ; i < size ; i ++){
            res.append(data[i]);
            if(i != size - 1)
                res.append(", ");
        }
        res.append(']');
        return res.toString();
    }

## 时间复杂度分析
分析时间复杂度我们一般考虑最坏的情况。
O(1):getCapacity、getSize、set、removeLast、addFirst、get、addFirst
O(n):resize、removeElement、removeFirst、remove、find、contains、add、addFirst
均摊复杂度（amortized time complexity）：在扩容的时候，不是每一次都会触发resize操作，因此根据均摊法我们们可以知道他的时间复杂度也是O(1)的。复杂度震荡解决方法：当前元素的个数为数组容量的１/4时（size = capacity/4），缩减数组容量为原来的1/2（capacity = capacity/2），就能有效解决复杂度震荡。
   
# 二、容器中的设计模式

## 迭代器模式

## 适配器模式

# 三、源码分析


### 1. 概览


             


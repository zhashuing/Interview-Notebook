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
## 均摊复杂度（amortized time complexity）
在扩容的时候，不是每一次都会触发resize操作，因此根据均摊法我们们可以知道他的时间复杂度也是O(1)的。复杂度震荡解决方法：当前元素的个数为数组容量的１/4时（size = capacity/4），缩减数组容量为原来的1/2（capacity = capacity/2），就能有效解决复杂度震荡。
   
# 三、栈（Last In First Out）
##　栈的应用
无处不在的 Undo 操作（撤销操作）
程序调用的系统栈
括号匹配
## 栈的实现
Interface Stack<E> 
### void push(E)        往栈中提交元素
### E pop()             删除栈顶元素
### E peek()            查看栈顶元素
### int getSize()     　获取栈元素个数
### boolean isEmpty()   栈是否为空

# 四、队列（First In First Out）
##ArrayQueue<E>                                              时间复杂度
### void enQueue(E)     向队列中提交元素                   O(1)
### E deQueue()         删除队列队首元素                   O(n)
### E getFront()        查看队列队首元素                   O(1)   
### int getSize()       获取队列元素个数                   O(1)
### boolean isEmpty()   队列是否为空                      O(1)
## source code

### Interface Queue<E>
public interface Queue<E> {
    void enQueue(E e);
    E deQueue();
    E getFront();
    int getSize();
    boolean isEmpty();
}

### ArrayQueue
public class ArrayQueue<E> implements Queue<E>{
    //动态数组
    private DynamicArray<E> array;
    
    public ArrayQueue(int capacity,Class type){
        array = new DynamicArray<>(capacity,type);
    }

    public ArrayQueue(Class type){
        this(10,type);
    }
    //获取队列中元素个数
    @Override
    public int getSize(){
        return array.getSize();
    }
    //获取队首元素
    @Override
    public E getFront(){
        return array.get(0);
    }
    //取出队首元素
    @Override
    public E deQueue(){
        return array.removeFirst();
    }
    //判断队列是否为空
    @Override
    public boolean isEmpty(){
        return array.getSize() == 0;
    }
    //向队列中添加元素
    @Override
    public void enQueue(E e){
        array.addLast(e);
    }
    @Override
    public String toString(){

        StringBuilder res = new StringBuilder();
        res.append(String.format("ArrayQueue: size = %d , capacity = %d", array.getSize(), array.getCapacity()));
        res.append("front [");
        for(int i = 0 ; i < array.getSize() ; i ++){
            res.append(array.get(i));
            if(i != array.getSize() - 1)
                res.append(",");
        }
        res.append(']');
        return res.toString();
    }

}

   
   
##　循环队列（因为在基于动态数组实现的队列中，每次出队操作时间复杂度都为O(n)，循环队列通过设置双指针的方法来达到出队入队的时间复杂度都为O(1)，因此，与动态数组的队列相比，循环队列具有更好的性能），循环队列要实现"循环"的效果，在移动 front 和　tail 指针的时候需要对数组的长度取余操作，下面的代码中需要可以维护　size，在resize方法中需要注意size的值。
## LoopQueue<E>                                              时间复杂度
### void enQueue(E)     向队列中提交元素                   O(1)
### E deQueue()         删除队列队首元素                   O(1)
### E getFront()        查看队列队首元素                   O(1)   
### int getSize()       获取队列元素个数                   O(1)
### boolean isEmpty()   队列是否为空                      O(1)
### source code
public class LoopQueue<E> implements Queue<E> {
    
    /**
     * 数组
     */
    private E[] data;
    /**
     * 头指针
     */
   
    private int front;
    /**
     * 头指针
     */
   
    private int tail;
    /**
     * 保存泛型的类型
     */
   
    private Class type;
    public LoopQueue(int capacity,Class type){
        this.type = type;
        data = (E[]) Array.newInstance(type,capacity + 1);
        front = 0;
        tail = 0;
    }
   
    public LoopQueue(Class type){
        this(10,type);
    }
   
    //获取元素个数
    @Override
    public int getSize(){
        if(isEmpty()){
            return 0;
        }
        /*如果tail在front的右边,直接相减得到size,如果 tail 在 front 右边,则size =(data.length - (front - tail ) */
        return tail > front ? (tail - front ) : (data.length - (front - tail ));
    }
   
    //循环队列是否为空
    @Override
    public boolean isEmpty(){
        return tail == front;
    }
   
    //循环队列可承载的元素个数
    public int getCapacity(){
        return data.length - 1;
    }
   
    //入队操作,可自动进行扩容
    @Override
    public void enQueue(E e){
        if((tail + 1) % data.length == front){  //队列为满时,扩大为原来的两倍
            resize(getCapacity() * 2);
        }
        data[tail] = e;
        tail = (tail + 1) % data.length;
    }

    @Override
    public E deQueue(){
        if(isEmpty()){//当前没有元素可以删除
            throw new RuntimeException("The LoopQueue is empty,cannot remove element again!");
        }
        E ret = data[front];
        data[front] = null;
        front = (front + 1) % data.length;
        //这里进行的的是缩容处理,我们使用的比较lazy的策略能够有效的避免复杂度震荡的情况
        if((getSize() == (getCapacity() / 4)) && (getCapacity() /2 != 0)){
            resize(getCapacity() / 2);
        }
        return ret;
    }

    private void resize(int capacity){
        int size = getSize();  //因为这个类中没有专门的变量维护size,需要先获取size
        //有一个额外的空间浪费
        E[] newData = (E[])Array.newInstance(type,capacity + 1);
        for (int i = 0; i < getSize(); i++) {
            newData[i] = data[(i+front) % data.length];
        }
        data = newData;
        /*扩容后修改 front tail 的指向*/
        front = 0;
        tail = size;
    }
    @Override
    public E getFront(){
        if(isEmpty()){
            return null;
        }
        return data[front];
    }

    @Override
    public String toString(){
        StringBuilder res = new StringBuilder();
        res.append(String.format("LoopQueue: size = %d , capacity = %d  ", getSize(), getCapacity()));
        res.append("front [");
        for(int i = front ; i != tail ; i = (i + 1) % data.length){
            res.append(data[i]);
            if((i + 1)% data.length != tail)
                res.append(", ");
        }
        res.append("] tail");
        return res.toString();
    }
}






             


# what i do in project2
## 第一步先学会如何和codex一起写作业，不是让他帮我写好，而是一起完成
![img.png](img.png)
![img_1.png](img_1.png)
什么是cli？
![img_2.png](img_2.png)

![img_3.png](img_3.png)

## project0先起步
![img_4.png](img_4.png)
## task1
### 我不理解这里面的key是什么意思啊？
![img_5.png](img_5.png)
key就是我们在索引什么！
![img_6.png](img_6.png)

这个project主要是在写leafnode这个文件
所以我们可以先把这个fie看懂
![img_7.png](img_7.png)
class里面定义一个构造函数construtor
这个this我还没有看懂啊？？？
![img_8.png](img_8.png)
### 先来做一下这个一星上将，我终于知道为什么这个为什么project里面字那么少了，因为提示和解释全部在代码里面
![img_9.png](img_9.png)
![img_10.png](img_10.png)
一个图片讲解了leafnode这个数据结构是如何存储的
但是c和d我没有看懂

题目让我去inner node里面去看如何找，如何从disk读取数据，
我不理解这里面读取的是哪个leafnode？
这个作业比我之前的难，是因为没有告诉我可以使用哪些函数（要我自己去找）
但是其实这些代码不难的，主要是要记忆，花时间去读完

![img_11.png](img_11.png)
确实，你会发现inner都有！主要是inner和leaf的结构不一样

![img_12.png](img_12.png)
这个文件里面主要是如何处理bytes

```java
public static InnerNode fromBytes(BPlusTreeMetadata metadata,
                                      BufferManager bufferManager, LockContext treeContext, long pageNum) {
        Page page = bufferManager.fetchPage(treeContext, pageNum);
        Buffer buf = page.getBuffer();

        byte nodeType = buf.get();
        assert(nodeType == (byte) 0);

        List<DataBox> keys = new ArrayList<>();//这个如何理解？databox是一个类型
        List<Long> children = new ArrayList<>();
        int n = buf.getInt();
        for (int i = 0; i < n; ++i) {
            keys.add(DataBox.fromBytes(buf, metadata.getKeySchema()));
            //这里像是使用了一个递归，但是我没有看懂metadata.getKeySchema()
            //原来不是递归！
        }
        for (int i = 0; i < n + 1; ++i) {
            children.add(buf.getLong());
        }
        return new InnerNode(metadata, bufferManager, page, keys, children, treeContext);
    }

```
```java
keys.add(DataBox.fromBytes(buf, metadata.getKeySchema()))
```
![img_13.png](img_13.png)
这个databox是一个很暧昧的变量
就是我也不知道是什么数据类型，就用databox统称吧！

我去这个task1我明白，赶紧速通睡觉了！
我主要是不熟悉一些变量类型，或者说我找不到，
但可以先点击command，然后会自动跳转到最初的定义去！
比如optional这个我感觉相当的抽象！
![img_14.png](img_14.png)

### 如何去测试test？
在IntelliJ setup for running tests里面有详细的教程
这个我先忽略
我先找一下测试一的错误
### mvn
![img_15.png](img_15.png)
这个说要在zsh下载mvn？
![img_18.png](img_18.png)
![img_19.png](img_19.png)

然后我希望你去学会读懂报错，去解决复杂的问题通过这些题目
![img_16.png](img_16.png)

这个一开始我都看不懂是哪里出现问题了
是我的assert被触发了，你要先看第一个问题
改了之后终于把这个一星的题目做对了！
![img_17.png](img_17.png)

### task2
![img_20.png](img_20.png)
6.16遇到特别特别悲伤的事情！我失恋了，连朋友也做不了了
#### get函数
![img_21.png](img_21.png)
在codex的带领下，我终于把第一个get写完了！没有那么难嘛
#### getleftmost函数
![img_22.png](img_22.png)
#### put
put 感觉很有难度啊
```java
private void sync() {
        page.pin();
        try {
            Buffer b = page.getBuffer();
            byte[] newBytes = toBytes();//注意，这里面的tobytes和buffer里面是什么是没有关系的！
            byte[] bytes = new byte[newBytes.length];
            b.get(bytes);//bytes代表了曾经的buffer里面存的数组
            if (!Arrays.equals(bytes, newBytes)) {//这个if减少了可能的写入
                page.getBuffer().put(toBytes());
            }
        } finally {
            page.unpin();
        }
    }

```

### Task 3: Scans
#### why we need a interator
为什么scanAll() 不能一次性把所有 RecordId 装进一个大 list 返回，因为项目要求 lazy scan
![img_23.png](img_23.png)

#### 一个经典的错误
```java
public boolean hasNext() {
            // TODO(proj2): implement
            if(currentIterator.hasNext()){
                return true;
            }
            if(!currentLeaf.getRightSibling().isPresent()){
                return false;//当前是最后一个leaf
            }
            BPlusTreeIterator next=new BPlusTreeIterator(currentLeaf.getRightSibling().get(),currentLeaf.getRightSibling().get().scanAll());
            return next.hasNext();

        }
```
####
这个interator很神奇，就像是贴在数组上的标签一样，next（）函数返回当前标签的数，然后标签往后移动一位

### Task 4: bulk load批量加载（我一开始都不知道中文
一开始我觉得很抽象的是我不知道这个函数在干什么事情，其实是批量加载
。然后每个结构体inner，leaf的bulkload都不一样，可以说是各司其职了


#### 太好了，project2杀青了，我写了有半个月，主要是中间有十天太伤心了，也不想写这个东西
![img_24.png](img_24.png)
2026.6.27

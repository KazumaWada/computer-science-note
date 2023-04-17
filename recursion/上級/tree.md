# tree

- graph(同じ頂点に戻る)、tree(同じ頂点に戻らない),頂点(または Node), 辺(linked-list でいう prev,next)
- 二本木(binary tree)を使って、sort と search を効果的に実装できるようになる。
- 親、子、兄弟、葉
- root node 深さ 0 高さ max

![](/上級/img01.png)

## Graph

- 頂点と辺を持つ構造体
- directed graph(next がある)と undirected graph(ただ繋がっている)がある。
- directed graph は、
- x∈S//x は s に属している。
- linked-list みたいに、綺麗に全部繋がっているわけではなく、途中で途切れたり、pointer がその要素自体に向いている(自己ループ)もある。
- 森:↑
- 木:全てが連結している森 頂点の数-1 == 辺の数 が成り立つ.
- root node: linked-list の head みたいなやつ。

## subtree 木構造の一部切り取った部分

## binary tree(二分木)

- rootNode 構造で、ある node が持つ子が 2 つの場合.
- 子は右と左と呼ばれている。含まれていない場合は、null tree と呼ばれる。
- 再帰を使って書かれている。

```java
class BinaryTree{
    public int data;
    // 左二分木
    public BinaryTree left;
    // 右二分木
    public BinaryTree right;

    public BinaryTree(int data){
        this.data = data;
    }
}

class Main{
    public static void main(String[] args){
        BinaryTree binaryTree = new BinaryTree(1);//binaryTree.data == 1;
        BinaryTree node2 = new BinaryTree(2);//node2.data == 2;
        BinaryTree node3 = new BinaryTree(3);//node3.data == 3;

        binaryTree.left = node2;
        binaryTree.right = node3;

        System.out.println("Root: " + binaryTree.data);
        System.out.println("Left: " + binaryTree.left.data);
        System.out.println("Right: " + binaryTree.right.data);
    }
}
// Root: 1
// Left: 2
// Right: 3
```

- 二分木の合計値を出す

```java
                  //どう考えても、これらの配列がどうやって右左に分かれているのか分からない。多分進めていくうちにわかるようになる。
//sumOfThreeNodes( toBinaryTree([0,-10,5,null,-3,null,9]) )--> -5
import java.util.ArrayList;

class BinaryTree<E>{
    public E data;
    public BinaryTree<E> left;//left.data;
    public BinaryTree<E> right;//right.data;

    public BinaryTree() {}
    public BinaryTree(E data) { this.data = data; }
    public BinaryTree(E data, BinaryTree<E> left, BinaryTree<E> right) {
        this.data = data;
        this.left = left;
        this.right = right;
    }
}
//root == this.data,right,left
class Solution{
    public static int sumOfThreeNodes(BinaryTree<Integer> root){
        // 関数を完成させてください
        if(root == null) return 0;
        int right = root.right != null ? root.right.data : 0;
        int left = root.left != null ? root.left.data : 0;
        return root.data + left + right;
    }
}
```

- full binary tree: 全て葉である or 二つの子を持っている。
- perfect binary tree: full binary tree の中でも、全ての葉ノードが同じ深さを持つ
- complete binary tree: 最下層を除いて、全ての深さが node で満たされ、最下層の葉ノードが可能な限り左に寄せられている。最大容量であることを表す。

## 二分探索木 binary search tree(bst)

簡単な探索のコード
見つけたい int があったら、i を返す。

```java
class Main{
    public static int linearSearch(int key, int[] haystack){
        for(int i = 0; i < haystack.length; i++){
            if(key == haystack[i]) return i;
        }
        return -1;
    }

    public static void main(String[] args){
        int[] l1 = new int[]{3,4,2,5,46,23,3,55,67,24,65};
        System.out.println(linearSearch(5,l1));
    }
}
```

これを木構造を使って表す。
BST

### 「左の子孫の値 ≤ 親(x)の値 ≤ 右の子孫の値」という制約を持つ二分木のことを指します

### 左下が、一番小さくなるようにできている。

### アルゴリズムは、根ノードから始まり、キー（目的の値）が現在のノードの値と等しいかどうかを判断します。キーが小さい場合は左の二分探索木を検索し、キーが大きい場合は右の二分探索木を検索します。これらのステップは、キーが見つかるまで再帰的に繰り返されます。

key より、右の方が大きかったら左へ進む。を再起的に行う。
↓
もし木が一本の枝だったら、検索するときに右を見ることができず、結局たくさんの計算時間がかかってしまう。
↓
よって、二分探索の探索効率が最も最高になるのは、木の高さが最も低いケース。(根ノードから node があまり伸びていない木構造)→ 木を作るときに、なるべく左右均等にする必要がある。
↓
平衡二分探索木と呼ばれる。

### 平衡二分探索木を実装する 1 番簡単な方法は、ソート済みのリストを用意して、そこから再帰的に BST を構築することです。

ソート済みのリストを BST(binary search tree) に移行するアルゴリズムを再帰的に実装 ↓
(木構造: list,分割統治法、再帰)

```java
class BinaryTree{
    public int data;
    public BinaryTree left;
    public BinaryTree right;
    // オーバーローディング
    public BinaryTree(int data){ this.data = data; }
    public BinaryTree(int data, BinaryTree left, BinaryTree right){
        this.data = data;
        this.left = left;
        this.right = right;
    }
}

class Main{
    public static BinaryTree sortedArrayToBSTHelper(int[] arr, int start, int end) {
        if(start == end) return new BinaryTree(arr[start], null,null);

        int mid = (int) Math.floor((start+end)/2);

        BinaryTree left = null;
        if(mid-1 >= start) left = sortedArrayToBSTHelper(arr, start, mid-1);

        BinaryTree right = null;
        if(mid+1 <= end) right = sortedArrayToBSTHelper(arr, mid+1, end);

        BinaryTree root = new BinaryTree(arr[mid], left, right);
        return root;//どうやって再帰されるんだろう。。
    }

    public static BinaryTree sortedArrayToBST(int[] nums) {
        if(nums.length == 0) return null;
        return sortedArrayToBSTHelper(nums, 0, nums.length-1);
    }

    // BSTリストの中にキーが存在かどうかによって、true、falseを返します。
    // 再帰
    public static boolean keyExist(int key, BinaryTree bst){
        if(bst == null) return false;
        if(bst.data == key) return true;

        // 現在のノードよりキーが小さければ左に、大きければ右に辿ります。
        //木が既に完成していて、右に行くか、左に行くか。
        if(bst.data > key) return keyExist(key, bst.left);
        else return keyExist(key, bst.right);
    }

    //or whileを使って求めることも出来る↓
        public boolean keyExist(int key){
        BinaryTree pointer =this.root;
        while(pointer != null){
            if(pointer.data == key) return true;
            // 現在のノードよりキーが小さければ左に、大きければ右に辿ります。
            if(pointer.data > key) pointer = pointer.left;
            else pointer = pointer.right;
        }

        return false;
    }
    //or whileを使って求めることも出来る↑

    public static void main(String[] args){
        BinaryTree balancedBST = sortedArrayToBST(new int[]{1,2,3,4,5,6,7,8,9,10,11});
        System.out.println(keyExist(6, balancedBST));
        System.out.println(keyExist(10, balancedBST));
        System.out.println(keyExist(45, balancedBST));
    }
}
```

二分探索木内探索

```java
import java.util.ArrayList;

class BinaryTree<E>{
    public E data;
    public BinaryTree<E> left;
    public BinaryTree<E> right;

    public BinaryTree() {}
    public BinaryTree(E data) { this.data = data; }
    public BinaryTree(E data, BinaryTree<E> left, BinaryTree<E> right) {
        this.data = data;
        this.left = left;
        this.right = right;
    }
}

class Solution{
    public static BinaryTree<Integer> bstSearch(BinaryTree<Integer> root, int key){
        BinaryTree<Integer> pointer = root;

        while(pointer != null){
            if(pointer.data == key)return pointer;
            else if(pointer.data < key)pointer = pointer.right;
            else if(pointer.data > key)pointer = pointer.left;
        }
        return pointer;
    }
}


```

二分探索木内のキー

```java
class Solution{
    public static boolean exists(BinaryTree<Integer> root, int key){
        // 関数を完成させてください
        BinaryTree<Integer> pointer = root;
        while(pointer != null){
            if(pointer.data == key)return true;
            else if(pointer.data < key)pointer = pointer.right;
            else if(pointer.data > key)pointer = pointer.left;
        }
        return false;
    }
}
```

最小値

```java
class Solution{
    public static BinaryTree<Integer> minimumNode(BinaryTree<Integer> root){
        // 関数を完成させてください
        BinaryTree<Integer> pointer = root;
        while(pointer != null){
            if(pointer.left == null)return pointer;
            pointer = pointer.left;
        }
        return pointer;
    }
}
```

最大値

```java
class Solution{
    public static BinaryTree<Integer> maximumNode(BinaryTree<Integer> root){
        // 関数を完成させてください
        BinaryTree<Integer> pointer = root;
        while(pointer != null){
            if(pointer.right == null)return pointer;
            pointer = pointer.right;
        }
        return pointer;
    }
}
```

sort 済み配列を二分探索木へと変換
これは、配列の合体、分割統治法というアルゴリズムというか、問題の解き方(大きな問題を分散して考える。)を応用している ↓(中級、リスト、分割統治法)

```java
//merge([3,4,7,9,15],[0,1,2,8,19]) --> [0,1,2,3,4,7,8,9,15,19]
import java.util.Arrays;

class Solution{
    public static int[] merge(int[] leftArr, int[] rightArr){
        // 関数を完成させてください
        int[] result = new int[leftArr.length + rightArr.length];

        int[] left = new int[leftArr.length+1];
        int[] right = new int[rightArr.length+1];

        if(leftArr.length == rightArr.length){
            for(int i = 0; i < leftArr.length; i++){
                left[i] = leftArr[i];
                right[i] = rightArr[i];
            }
        }
        else{
            for(int i = 0; i < leftArr.length; i++){
                left[i] = leftArr[i];
            }

            for(int i = 0; i < rightArr.length; i++){
                right[i] = rightArr[i];
            }
        }
        ////↑ここまでは、動的配列を作成している。////

        left[left.length-1] = Integer.MAX_VALUE;
        right[right.length-1] = Integer.MAX_VALUE;

        int l = leftArr.length + rightArr.length;
        int li = 0;
        int ri = 0;
        int index = 0;
        //merge(left,rightを足したlength)まで。
        while(li + ri < l){
            if(left[li] <= right[ri]){
                result[index] = left[li];
                li++;
                index++;
            }
            else{
                result[index] = right[ri];
                ri++;
                index++;
            }
        }
        return result;
    }
}
```

sort 済みを二分探索木へと変換のコード(分割統治法のアルゴリズムを使って考える。(大きな問題を 2 つに分けて考える))

```java
import java.util.Arrays;
import java.util.ArrayList;

class BinaryTree<E>{
    public E data;
    public BinaryTree<E> left;
    public BinaryTree<E> right;

    public BinaryTree() {}
    public BinaryTree(E data) { this.data = data; }
    public BinaryTree(E data, BinaryTree<E> left, BinaryTree<E> right) {
        this.data = data;
        this.left = left;
        this.right = right;
    }
}
//アルゴリズムである分割統治法を使って、左右順番に求める
class Solution{
    public static BinaryTree<Integer> helper(int[] numberList, int start, int end){
        //ベースケース
        //nullは最後だからわかるけど、なぜnumberlist[start]??
        //このstart,endは、0,ength-1では無く、左右それぞれの0~中間, 中間~最後までの意味。
        //全体としては、start,endは、0~length-1だけど、分けて考えた時にそうなる。
        if(start == end)return new BinaryTree(numberList[start], null,null);

        int mid = (int)Math.floor((start+end)/2);

        BinaryTree left = null;
        //5-1,4-1,3-1,2-1,1-1
        //左をざーッと全部決める。
        //0~4まで
        if(mid-1>=start) left = helper(numberList, start, mid-1);
        BinaryTree right = null;
        //6~10まで
        //右をざーッと全部決める
        if(mid+1 <= end) right = helper(numberList, start, mid+1);

        //左右決まったやつとrootを合体させる。
        //ということは、leftで変数は更新されるわけでは無く、積み重なっている??
        BinaryTree root = new BinaryTree(numberList[mid], left, right);
        return root;
    }
    public static BinaryTree<Integer> sortedArrToBST(int[] numberList){
        // 関数を完成させてください
        if(numberList.length == 0) return null;
        return helper(numberList, 0, numberList.length-1);
    }
}

/////////////////////////////もっとわかりやすく書かれたコード///////////////////////////////////////////////////////////////
class Solution{
    public static BinaryTree<Integer> sortedArrToBST(int[] numberList){
        // 配列のstart,endを引数としてヘルパー関数を使います。
        return sortedArrToBSTHelper(numberList, 0, numberList.length - 1);
    }

    public static BinaryTree<Integer> sortedArrToBSTHelper(int[] list, int start, int end) {
        // ベースケースは要素が1つになった時
        if (start == end) return new BinaryTree<Integer>(list[start]);
        // 配列の中心を決めます。
        int mid = (int)Math.floor((start + end) / 2);

        // startからmid-1までを左側、mid+1からendまでを右側の部分木にします。
        BinaryTree<Integer> left = start == mid ? null : sortedArrToBSTHelper(list, start, mid - 1);
        BinaryTree<Integer> right = end == mid ? null : sortedArrToBSTHelper(list, mid + 1, end);

        // 配列の中心を根ノードとして、左右の部分木を合わせて新しいノードにします。
        return new BinaryTree<Integer>(list[mid], left, right);
    }
}
```

後続 node を返す。(後続 node. key よりも大きく、木の中で最小の値)

- 重要なコードのみ（key の右側に子があるかどうかでコードが分かれてる）

```java
class Solution{
    public static BinaryTree<Integer> successor(BinaryTree<Integer> root, int key){
        // keyのノードを探し、変数targetNodeに代入します。
        BinaryTree<Integer> targetNode = findNode(root, key);
        // keyがBST内に存在しない場合、nullを返します。省略
        // targetNodeに右側の子がある場合
        if (targetNode.right != null) return minimumNode(targetNode.right);
        //targetNodeに右側の子がない場合
        BinaryTree<Integer> successor = null;
        BinaryTree<Integer> iterator = root;
        // rootをiteratorに代入し、探索と同じ要領でtargetNodeがiteratorよりも小さい時には左へ、大きい時は右へ進みます。
        while (iterator != null) {

            //????keyと現在のiteratorが同じだったら、その時のsuccessorを返す。。後続node見つかったの??how?
            if (targetNode.data == iterator.data) return successor;

            // 左側に進むときは、現在のiteratorが後続ノードである可能性があるのでsuccessorを更新します。
            if (targetNode.data < iterator.data) {
                successor = iterator;
                iterator = iterator.left;
            }
            // 右側に進むときはsuccessorを更新する必要はありません。
            else iterator = iterator.right;
        }
        return successor;
    }

    // keyのノードを探す関数は省略
    //public static BinaryTree<Integer> findNode(BinaryTree<Integer> root, int key)

    // 最小値を探す関数
    public static BinaryTree<Integer> minimumNode(BinaryTree<Integer> root){
        BinaryTree<Integer> iterator = root;
        while (iterator != null && iterator.left != null) iterator = iterator.left;
        return iterator;
    }
}
```

- コード

```java
import java.util.ArrayList;

class Solution{
    public static BinaryTree<Integer> successor(BinaryTree<Integer> root, int key){
        // keyのノードを探し、変数targetNodeに代入します。
        BinaryTree<Integer> targetNode = findNode(root, key);

        // keyがBST内に存在しない場合、nullを返します。
        if (targetNode == null) return null;

        // targetNodeに右側の子がある場合は、その部分木の最小値を返します。
        if (targetNode.right != null) return minimumNode(targetNode.right);

        BinaryTree<Integer> successor = null;
        BinaryTree<Integer> iterator = root;
        // rootをiteratorに代入し、探索と同じ要領でtargetNodeがiteratorよりも小さい時には左へ、大きい時は右へ進みます。
        while (iterator != null) {

            // ターゲットとiteratorのdataが同じだったら、その時のsuccessorを返します。
            if (targetNode.data == iterator.data) return successor;

            //keyより大きい!これの可能性がある!
            // 左側に進むときは、現在のiteratorが後続ノードである可能性があるのでsuccessorを更新しておく。
            if (targetNode.data < iterator.data) {
                successor = iterator;
                iterator = iterator.left;
            }
            // 右側に進むときはsuccessorを更新する必要はありません。
            else iterator = iterator.right;
        }
        return successor;
    }

    // keyのノードを探す関数
    public static BinaryTree<Integer> findNode(BinaryTree<Integer> root, int key) {
        BinaryTree<Integer> iterator = root;

        while (iterator != null) {
            if (iterator.data == key) return iterator;
            if (iterator.data < key) iterator = iterator.right;
            else iterator = iterator.left;
        }
        return iterator;
    }

    // 最小値を探す関数
    public static BinaryTree<Integer> minimumNode(BinaryTree<Integer> root){
        BinaryTree<Integer> iterator = root;
        //nullではなく、左の子もnullではない間、左に行き続ける。
        while (iterator != null && iterator.left != null) iterator = iterator.left;
        return iterator;
    }
}
```

- 先行 node を探すコード
- 先行 node: key よりも小さい最大の値を持つノードのことを指します。

```java
class Solution{
    public static BinaryTree<Integer> predecessor(BinaryTree<Integer> root, int key){
        // 関数を完成させてください
        //とりあえず、左側に子を持っている場合を考えてみる。
        // keyのノードを探し、変数targetNodeに代入します。
        BinaryTree<Integer> targetNode = findNode(root, key);

        // keyがBST内に存在しない場合、nullを返します。
        if (targetNode == null) return null;

        // targetNodeに右側の子がある場合は、その部分木の最小値を返します。
        if (targetNode.right != null) return maxNode(targetNode.right);

    // 最大値を探す関数
    public static BinaryTree<Integer> maxNode(BinaryTree<Integer> root){
        BinaryTree<Integer> iterator = root;
        //nullではなく、左の子もnullではない間、左に行き続ける。
        while (iterator != null && iterator.left != null) iterator = iterator.left;
        return iterator;
    }
    }
}


```

# 木構造の走査(traverse)

- linkedList の走査

```java
class Node{
    public Node next;
    public int data;

    public Node(int data){
        this.data = data;
    }
}

class SinglyLinkedList{
    // 配列を受け取り、連結リストを作成します。
    public Node head;

    public SinglyLinkedList(int[] arr){
        // 先頭を初期化します。
        this.head = arr.length > 0 ? new Node(arr[0]) : null;

        Node currentNode = this.head;
        for(int i = 1; i < arr.length; i++){
            currentNode.next = new Node(arr[i]);
            currentNode = currentNode.next;
        }
    }

    public Node at(int index){
        Node iterator = this.head;
        // indexの終わりまで反復します。iteratorがnullになる時は、indexが範囲外を意味します。
        for(int i = 0; i < index; i++){
            // nextがnullの場合、nullを返します。これはindexが範囲外を意味します。
            iterator = iterator.next;
            if(iterator == null) return null;
        }

        return iterator;
    }

    public void printList(){
        Node iterator = this.head;
        String str = "";
        while(iterator != null){
            str += iterator.data + " ";
            iterator = iterator.next;
        }
        System.out.println(str);
    }
}
class Main{
    public static void main(String[] args){
        SinglyLinkedList numList = new SinglyLinkedList(new int[]{35,23,546,67,86,234,56,767,34,1,98,78,555});
        numList.printList();
    }
}
```

#### 木構造の走査は、全て再帰関数で実装することができる。

- 木構造の走査 2 つ
- 深さ優先: root から最下層まで。枝分かれしたところから戻って、最下層まで、、を繰り返す。(よく使う)
  - pre-order(前順): 根 -> 左の部分木 -> 右の部分木の順で調査を行う。(上で述べたのと同じ。)
  - in-order(間順): 左の部分木->根->右の部分木で順に行う。(配列で sort された順みたいになる。)
  - post-order(後順): 左の部分木->右の部分木->根の順で行う。(最初に子ノードを読み、徐々に上に上がっていく。)
  - reverse-order(逆間): 右の部分木->根->左の部分木(配列で sort された逆順みたいになる。)
- 幅優先: 根ノードから始まり、横の node を全て調べる、その下の横の node を全て調べる、、というように徐々に下に降りていく。

再帰で、root,root.left が null になるまで、root.right が null になるまで走査している。他のやつは、3 つの順番が入れ替わっただけ。

```java
import java.util.Arrays;

class BinaryTree{
    public int data;
    public BinaryTree left;
    public BinaryTree right;

    public BinaryTree(int data){ this.data = data;}
    public BinaryTree(int data, BinaryTree left, BinaryTree right){
        this.data = data;
        this.left = left;
        this.right = right;

    }

    public void printPreOrder(){
        this.preOrderWalk(this);
        System.out.println("");
    }

    // 前順（pre-order）（NLR）
    public void preOrderWalk(BinaryTree tRoot){
        if(tRoot!=null){
            System.out.print(tRoot.data + " ");
            this.preOrderWalk(tRoot.left);
            this.preOrderWalk(tRoot.right);
        }
    }
}

class BinarySearchTree{
    public BinaryTree root;

    public BinarySearchTree(int[] arr){
        Arrays.sort(arr);
        this.root = sortedArrayToBSTHelper(arr, 0, arr.length-1);
    }


    public static BinaryTree sortedArrayToBSTHelper(int[] arr, int start, int end){
        if(start == end) return new BinaryTree(arr[start], null, null);

        int mid = (int)Math.floor((start+end)/2);

        BinaryTree left = null;
        if(start <= mid-1) left = sortedArrayToBSTHelper(arr, start, mid-1);

        BinaryTree right = null;
        if(end >= mid+1) right = sortedArrayToBSTHelper(arr, mid+1, end);

        BinaryTree root = new BinaryTree(arr[mid], left, right);

        return root;
    }

    public boolean keyExist(int key){
        BinaryTree iterator = this.root;
        while(iterator != null){
            if(iterator.data == key) return true;
            if(iterator.data < key) iterator = iterator.right;
            else iterator = iterator.left;
        }
        return false;
    }

    public BinaryTree search(int key){
        BinaryTree iterator = this.root;
        while(iterator != null){
            if(iterator.data == key) return iterator;
            if(iterator.data < key) iterator = iterator.right;
            else iterator = iterator.left;
        }
        return null;
    }

    public void printSorted(){
        this.root.printPreOrder();
    }
}

class Main{
    public static void main(String[] args){
        BinarySearchTree balancedBST = new BinarySearchTree(new int[]{1,2,3,4,5,6,7,8,9,10,11});
        balancedBST.printSorted();
    }
}
```

in-order を走査する

```java
class BinaryTree{
    constructor(data, left = null, right = null){
        this.data = data;
        this.left = left;
        this.right = right;
    }

    printInOrder(){
        //このthisはどこから始まるの??
        this.inOrderWalk(this);
        console.log("");
    }

    //再帰
    //これがどうやってin-order順に走査されるのかわからん。。。
　　inOrderWalk(tRoot){//=this,,でも、どこから始まるのかわからん。
        if(tRoot != null){
            this.inOrderWalk(tRoot.left);
            process.stdout.write(tRoot.data + " ");
            this.inOrderWalk(tRoot.right);
        }
    }
}

class BinarySearchTree{
    constructor(arrList){
        // JS ソートライブラリの sort() は文字列に対しては問題なくソートしますが、数値に対しては特別なルールが必要です。sort関数は引数に関数を指定でき、関数でソートのルールを定義できます。aとbを比較する関数で、a-bが負の場合はa < b、0の場合はa == b、そうでない場合はa > bとなります。自分でソート関数を作成することができます。

        //arrListをsortする。
        let sortedList = arrList.sort(function(a, b) {return a - b;});
        this.root = BinarySearchTree.sortedArrayToBST(sortedList);
    }

    static sortedArrayToBST(array) {
        if(array.length == 0) return null;
        return BinarySearchTree.sortedArrayToBSTHelper(array, 0, array.length-1);
    }

    static sortedArrayToBSTHelper(arr, start, end) {
        if(start === end) return new BinaryTree(arr[start], null,null);

        let mid = Math.floor((start+end)/2);

        let left = null;
        if(mid-1 >= start) left = BinarySearchTree.sortedArrayToBSTHelper(arr, start, mid-1);

        let right = null;
        if(mid+1 <= end) right = BinarySearchTree.sortedArrayToBSTHelper(arr, mid+1, end);

        let root = new BinaryTree(arr[mid], left, right);
        return root;
    }

    keyExist(key){
        let iterator = this.root;
        while(iterator != null){
            if(iterator.data == key) return true;
            if(iterator.data > key) iterator = iterator.left;
            else iterator = iterator.right;
        }

        return false;
    }

    search(key){
        let iterator = this.root;
        while(iterator != null){
            if(iterator.data == key) return iterator;
            if(iterator.data > key) iterator = iterator.left;
            else iterator = iterator.right;
        }

        return null;
    }

    printSorted(){
        this.root.printInOrder();
    }
}

let balancedBST = new BinarySearchTree([1,2,3,4,5,6,7,8,9,10,11]);
balancedBST.printSorted();
```

pre-order(根->左->右)

```java
import java.util.Arrays;

class BinaryTree{
    public int data;
    public BinaryTree left;
    public BinaryTree right;

    public BinaryTree(int data){ this.data = data;}
    public BinaryTree(int data, BinaryTree left, BinaryTree right){
        this.data = data;
        this.left = left;
        this.right = right;

    }

    public void printPreOrder(){
        this.preOrderWalk(this);
        System.out.println("");
    }

    // 前順（pre-order）（NLR）//再帰
    public void preOrderWalk(BinaryTree tRoot){
        if(tRoot!=null){
            System.out.print(tRoot.data + " ");
            //左がnullになるまで
            this.preOrderWalk(tRoot.left);
            //右がnullになるまで
            this.preOrderWalk(tRoot.right);
        }
    }
}

class BinarySearchTree{
    public BinaryTree root;

    public BinarySearchTree(int[] arr){
        Arrays.sort(arr);
        this.root = sortedArrayToBSTHelper(arr, 0, arr.length-1);
    }


    public static BinaryTree sortedArrayToBSTHelper(int[] arr, int start, int end){
        if(start == end) return new BinaryTree(arr[start], null, null);

        int mid = (int)Math.floor((start+end)/2);

        BinaryTree left = null;
        if(start <= mid-1) left = sortedArrayToBSTHelper(arr, start, mid-1);

        BinaryTree right = null;
        if(end >= mid+1) right = sortedArrayToBSTHelper(arr, mid+1, end);

        BinaryTree root = new BinaryTree(arr[mid], left, right);

        return root;
    }

    public boolean keyExist(int key){
        BinaryTree iterator = this.root;
        while(iterator != null){
            if(iterator.data == key) return true;
            if(iterator.data < key) iterator = iterator.right;
            else iterator = iterator.left;
        }
        return false;
    }

    public BinaryTree search(int key){
        BinaryTree iterator = this.root;
        while(iterator != null){
            if(iterator.data == key) return iterator;
            if(iterator.data < key) iterator = iterator.right;
            else iterator = iterator.left;
        }
        return null;
    }

    public void printSorted(){
        this.root.printPreOrder();
    }
}

class Main{
    public static void main(String[] args){
        BinarySearchTree balancedBST = new BinarySearchTree(new int[]{1,2,3,4,5,6,7,8,9,10,11});
        balancedBST.printSorted();
    }
}
```

# 木構造 BST 挿入 insert

```java
　　　　　　　　　　　　//insertしたいnode
    public BinaryTree insert(int data){
        BinaryTree iterator = this.root;
        while(iterator != null){
            //insertしたいdataが現在のiteratorより小さかったら左に行く
            if(iterator.data > data && iterator.left == null) iterator.left = new BinaryTree(data);
            //insertしたいdataが現在のiteratorより大きかったら右に行く
            else if(iterator.data < data && iterator.right == null) iterator.right = new BinaryTree(data);
            iterator = (iterator.data > data)? iterator.left: iterator.right;
        }
        return this.root;
    }
```

```java
import java.util.Arrays;

class BinaryTree{
    public int data;
    public BinaryTree left;
    public BinaryTree right;

    public BinaryTree(int data){
        this.data = data;
    }

    public BinaryTree(int data, BinaryTree left, BinaryTree right){
        this.data = data;
        this.left = left;
        this.right = right;
    }

    public void printInOrder(){
        this.inOrderwalk(this);
        System.out.println("");
    }

    public void inOrderwalk(BinaryTree tRoot){
        if(tRoot != null){
            inOrderwalk(tRoot.left);
            System.out.print(tRoot.data+ " ");
            inOrderwalk(tRoot.right);
        }
    }
}

class BinarySearchTree{
    public BinaryTree root;

    public BinarySearchTree(int[] arr){
        Arrays.sort(arr);
        this.root = sortedBSTHelper(arr, 0, arr.length-1);
    }

    public static BinaryTree sortedBSTHelper(int[] arr, int start, int end){
        if(start == end) return new BinaryTree(arr[start], null, null);

        int mid = (int)Math.floor((start+end)/2);
        BinaryTree left=null;
        if(start<mid) left = sortedBSTHelper(arr, start, mid-1);
        BinaryTree right=null;
        if(end>mid) right = sortedBSTHelper(arr, mid+1, end);
        BinaryTree root = new BinaryTree(arr[mid], left, right);
        return root;
    }

    public boolean existKey(int key){
        BinaryTree iterator = this.root;
        while(iterator != null){
            if(iterator.data == key) return true;
            else if(iterator.data > key) iterator = iterator.left;
            else iterator = iterator.right;
        }
        return false;
    }

    public BinaryTree search(int key){
        BinaryTree iterator = this.root;
        while(iterator != null){
            if(iterator.data == key) return iterator;
            if(iterator.data > key) iterator = iterator.left;
            else iterator = iterator.right;
        }
        return null;
    }

    public BinaryTree insert(int data){
        BinaryTree iterator = this.root;
        while(iterator != null){
            if(iterator.data > data && iterator.left == null) iterator.left = new BinaryTree(data);
            else if(iterator.data < data && iterator.right == null) iterator.right = new BinaryTree(data);
            iterator = (iterator.data > data)? iterator.left: iterator.right;
        }
        return this.root;
    }

    public void printSorted(){
        this.root.printInOrder();
    }
}

class Main{
    public static void main(String[] args){

        BinarySearchTree balancedBST = new BinarySearchTree(new int[]{4,43,36,46,32,7,97,95,34,8,96,35,85,1010,232});
        // balancedBST.printSorted();
        // balancedBST.insert(5);
        // balancedBST.printSorted();
        // balancedBST.insert(47);
        balancedBST.printSorted();
        // 0をinsertします。
        balancedBST.insert(0);
        balancedBST.printSorted();
    }
}
```

- 二分探索木への挿入

```java
//大きかったら右、小さかったら左で再帰していく。
//挿入するkeyが最下層まで来たor行き場所がなくなったら、
//return する。
import java.util.ArrayList;

class BinaryTree<E>{
    public E data;
    public BinaryTree<E> left;
    public BinaryTree<E> right;

    public BinaryTree() {}
    public BinaryTree(E data) { this.data = data; }
    public BinaryTree(E data, BinaryTree<E> left, BinaryTree<E> right) {
        this.data = data;
        this.left = left;
        this.right = right;
    }
}

class Solution{
    public static BinaryTree<Integer> bstInsert(BinaryTree<Integer> root, int key){

        // nullまでたどり着いた時、またはkeyがBSTに存在した時は、key を入れた新しいノードを作ります。
        if (root == null) return new BinaryTree<>(key);

        //そもそもkeyしか木の要素がなかったら、
        if (root.data == key) return root;
        // key と ノードの値を比較し、keyが小さければ左へ、大きければ右へめぐります。
        if (key > root.data) root.right = bstInsert(root.right, key);
        else root.left = bstInsert(root.left, key);
        //keyの位置が比較できなくなった == 場所が決まったら、
        return root;
    }

}

```

# 木構造　削除 BST remove

```java
import java.util.Arrays;

class BinaryTree{
    public int data;
    public BinaryTree left;
    public BinaryTree right;

    public BinaryTree(int data){
        this.data = data;
    }

    public BinaryTree(int data, BinaryTree left, BinaryTree right){
        this.data = data;
        this.left = left;
        this.right = right;
    }

    public void printInOrder(){
        this.inOrderwalk(this);
        System.out.println("");
    }

    public void inOrderwalk(BinaryTree tRoot){
        if(tRoot != null){
            inOrderwalk(tRoot.left);
            System.out.print(tRoot.data+ " ");
            inOrderwalk(tRoot.right);
        }
    }
}

class BinarySearchTree{
    public BinaryTree root;

    public BinarySearchTree(int[] arr){
        Arrays.sort(arr);
        this.root = sortedBSTHelper(arr, 0, arr.length-1);
    }

    public static BinaryTree sortedBSTHelper(int[] arr, int start, int end){
        if(start == end) return new BinaryTree(arr[start], null, null);

        int mid = (int)Math.floor((start+end)/2);
        BinaryTree left=null;
        if(start < mid) left = sortedBSTHelper(arr, start, mid-1);
        BinaryTree right = null;
        if(end > mid) right = sortedBSTHelper(arr, mid+1, end);
        BinaryTree root = new BinaryTree(arr[mid], left, right);
        return root;
    }

    public boolean keyExist(int key){
        BinaryTree iterator = this.root;
        while(iterator != null){
            if(iterator.data == key) return true;
            else if(iterator.data > key) iterator = iterator.left;
            else iterator = iterator.right;
        }
        return false;
    }

    public BinaryTree search(int key){
        BinaryTree iterator = this.root;
        while(iterator != null){
            if(iterator.data == key) return iterator;
            if(iterator.data > key) iterator = iterator.left;
            else iterator = iterator.right;
        }
        return null;
    }

    public BinaryTree insert(int data){
        BinaryTree iterator = this.root;
        while(iterator != null){
            if(iterator.data > data && iterator.left == null) iterator.left = new BinaryTree(data);
            else if(iterator.data < data && iterator.right == null) iterator.right = new BinaryTree(data);
            iterator = (iterator.data > data)? iterator.left: iterator.right;
        }
        return this.root;
    }

    public void transplant(BinaryTree nodeParent, BinaryTree node, BinaryTree target){
        if (nodeParent == null) this.root = target;
        else if (nodeParent.left == node) nodeParent.left = target;
        else nodeParent.right = target;
    }

    public void deleteNode(int key){
        if (this.root == null) return;
        BinaryTree node = this.search(key);
        if (!this.keyExist(key)) return;

        BinaryTree parent = this.findParent(node);
        // case 1: ノードNの左が空
        if (node.left == null) this.transplant(parent, node, node.right);
        // case 2: ノードNの右が空
        else if (node.right == null) this.transplant(parent, node, node.left);
        // case 3: 2つの子を持っている場合
        else{
            BinaryTree successor = this.findSuccessor(node);
            BinaryTree successorP = this.findParent(successor);

            // case 3 後続ノードSがすぐ右側にいる場合 : この場合、ノードNが後続ノードSの親になっているため、case4は必要ありません。単純にNの親であるPの部分木とSを移植すればokです。
            // 特別なケース (case 4) 後続ノードSがすぐ右側にいない場合 : この場合、後続Sの親も変更しなければいけません。
            if (successor != node.right){
                // 後続ノードSをSの右部分木で移植します。Sをアップデートします。
                this.transplant(successorP, successor, successor.right);
                // Sの右側はノードNの右側になっている必要があります。
                successor.right = node.right;

            }
            // ノードNを後続Sで移植します。Sの左部分木をノードNの左部分木にします。
            this.transplant(parent, node, successor);
            successor.left = node.left;
        }
    }


    public BinaryTree findParent(BinaryTree node){
        BinaryTree iterator = this.root;
        BinaryTree parent = null;
        while (iterator != node){
            parent = iterator;
            iterator = iterator.data > node.data ? iterator.left: iterator.right;
        }
        return parent;
    }

    public BinaryTree findSuccessor(BinaryTree node){
        // 部分木
        BinaryTree targetNode = node;
        // keyがBST内に存在しない場合、nullを返します。
        if (targetNode == null) return null;
        // keyのノードの右にある最小値を探します。
        if (targetNode.right != null) return this.minimumNode(targetNode.right);

        BinaryTree successor = null;
        BinaryTree iterator = this.root;

        while (iterator != null) {
            if (targetNode.data == iterator.data) return successor;
            // successorを左方向へずらしていきます。
            if (targetNode.data < iterator.data && (successor == null || iterator.data < successor.data)) successor = iterator;
            if (targetNode.data < iterator.data) iterator = iterator.left;
            else iterator = iterator.right;
        }
        return successor;
    }

    public BinaryTree minimumNode(BinaryTree node){
        BinaryTree iterator = node;
        while (iterator != null && iterator.left != null) iterator = iterator.left;
        return iterator;
    }


    public void printSorted(){
        this.root.printInOrder();
    }
}

class Main{
    public static void main(String[] args){

        BinarySearchTree balancedBST = new BinarySearchTree(new int[]{4,43,36,46,32,7,97,95,34,8,96,35,85,1010,232});
        balancedBST.printSorted();
        balancedBST.deleteNode(43);
        balancedBST.printSorted();
        balancedBST.deleteNode(7);
        balancedBST.printSorted();
        balancedBST.deleteNode(4);
        balancedBST.printSorted();
        balancedBST.deleteNode(1010);
        balancedBST.printSorted();
        // 存在しない0をdeleteNodeします。
        balancedBST.deleteNode(0);
        balancedBST.printSorted();
    }
}
```

# binary heap（complete binary tree の事を指す)

- 最大ヒープ(max heap):
  根ノードが常にリスト全体の最大値となっていて、すべてのノードについて「ノード N の親 >= ノード N」が成立している二分木データ構造
  70
  50 40
  30 20 10

- 最小ヒープ(min heap): 上の真逆。root が最小になっていて、下に行くにつれて最大になってくる。
  最大、最小ヒープは、対照的であり、それぞれ同じロジックを使うことができる。

complete binary tree: 最下層を除いてすべての深さがノードで満たされ、最下層の葉ノードが可能な限り左に寄せられているような木を完全二分木（complete binary tree）と呼びました。
↓
この性質を利用すれば、heap=complete binary tree を配列として扱うことができる。

```java
import java.util.Arrays;
import java.util.ArrayList;

class Heap{
    //binary heap == 完全二分木の左右は、以下の関数で導き出すことができる。
    public static int left(int i){
        return 2*i + 1;
    }

    public static int right(int i){
        return 2*i + 2;
    }

    public static void maxHeapify(ArrayList<Integer> arr, int i){

        int l = Heap.left(i);
        int r = Heap.right(i);
        //i,l,rの中で、一番大きい要素を決める。
        int biggest = i;
        if(l < arr.size() && arr.get(l) > arr.get(biggest)) biggest = l;
        if(r < arr.size() && arr.get(r) > arr.get(biggest)) biggest = r;

        if(biggest != i ){
            int temp = arr.get(i);
            arr.set(i, arr.get(biggest));
            arr.set(biggest, temp);
            Heap.maxHeapify(arr, biggest);
        }
    }
}

class Main{
    public static void main(String[] args){
        //ただ木構造を作った。
        ArrayList<Integer> heap1 = new ArrayList<>(Arrays.asList(new Integer[]{2,42,11,30,10,7,6,5,9}));
        System.out.println(heap1);
        //木構造を最大heap化する。
        Heap.maxHeapify(heap1,0); // 根ノードが2で、2 < 42のため、最大ヒープではありません。
        System.out.println(heap1);

    }
}
```

# heap sort わからない。。あとでやる。

list の並び替えを二分ヒープを用いて実装

```java
import java.util.Arrays;
import java.util.ArrayList;

class Heap{
    public static int left(int i){
        return 2*i + 1;
    }

    public static int right(int i){
        return 2*i + 2;
    }

    public static int parent(int i){
        return (int)Math.floor((i-1)/2);
    }
    //////////////////////////////////////////////////////////////////////////////////

    // ヒープのサイズを追跡するために maxHeapify を拡張します
    //最大値を取得
    //↓
    //最後の葉nodeと最大値node(根node)を入れ替える
    //↓
    //最大値(根node)を切り離し、listに格納する

    //最大ヒープにする
    public static void maxHeapify(ArrayList<Integer> arr, int heapEnd, int i){
        　　　　//完全二分木の左
        int l = Heap.left(i);
        　　　　//完全二分木の左
        int r = Heap.right(i);

        int biggest = i;
        // heapEnd より後ろはすでにソートされているので、l と　r のインデックスは heapEnd までを比較します。
        //heapEndとは?
        if(l <= heapEnd && arr.get(l) > arr.get(biggest)) biggest = l;
        if(r <= heapEnd && arr.get(r) > arr.get(biggest)) biggest = r;

        if(biggest != i ){
            int temp = arr.get(i);
            arr.set(i, arr.get(biggest));
            arr.set(biggest, temp);
            Heap.maxHeapify(arr, heapEnd, biggest);
        }
    }

    //最大ヒープを探す
    public static void buildMaxHeap(ArrayList<Integer> arr){
        int middle = Heap.parent(arr.size()-1);
        for(int i = middle; i >=0; i--){
            Heap.maxHeapify(arr, arr.size()-1, i);
        }
    }

    // ヒープソートを実装します。
    public static void heapSort(ArrayList<Integer> arr){
        // まずは buildMaxHeap で arr をヒープ構造にします。1番上は最大値になっています。
        Heap.buildMaxHeap(arr);

        // ヒープサイズを追跡するため heapEnd を配列の最後の要素にします。
        int heapEnd = arr.size() - 1;
        while(heapEnd > 0){
            //  最大値であるヒープの根ルートと葉ノード heapEnd を入れ替えます。
            int temp = arr.get(heapEnd);
            arr.set(heapEnd, arr.get(0));
            arr.set(0, temp);

            //　一番最後はソートされているので、heapEnd から 1 引きます。
            heapEnd--;
            Heap.maxHeapify(arr, heapEnd, 0);
        }
    }
}

class Main{
    public static void main(String[] args){

        ArrayList<Integer> heap1 = new ArrayList<>(Arrays.asList(new Integer[]{2,42,11,30,10,7,6,5,9}));
        System.out.println(heap1);
        Heap.heapSort(heap1);
        System.out.println(heap1);// 昇順にソートされた。
        //[2, 42, 11, 30, 10, 7, 6, 5, 9]
　　　　　//[2, 5, 6, 7, 9, 10, 11, 30, 42]
    }
}
```

# 優先度付きキュー

- 復習として、そもそも queue とは、linked-list に出てきたやつ。それに優先度が付いたやつ。
- 最も優先度が高い要素を queue から取り除く。

---

## binary tree

```javascript
class BinaryTree {
  constructor(data) {
    this.data = data;
    // 左二分木
    this.left = null;
    // 右二分木
    this.right = null;
  }
}
//3つともdataだけ送り込まれて、左右はまだnull
let binaryTree = new BinaryTree(1);
let node2 = new BinaryTree(2);
let node3 = new BinaryTree(3);
//左は2.右は3.とここで設定される。
binaryTree.left = node2;
binaryTree.right = node3;

console.log("Root: " + binaryTree.data);
console.log("Left: " + binaryTree.left.data);
console.log("Right: " + binaryTree.right.data);
```

### binary search tree(BST)

-　木構造を使わずに index0 から一つずつ見ていくパターン。

```javascript
function linearSearch(key, haystack) {
  //(5,[3,4,2,5,46,23,3,55,67,24,65])
  for (let i = 0; i < haystack.length; i++) {
    if (key === haystack[i]) return i;
  }
  return -1;
}

let l1 = [3, 4, 2, 5, 46, 23, 3, 55, 67, 24, 65];
console.log(linearSearch(5, l1));
console.log(linearSearch(24, l1));
```

- その前に完全木の作成(ソート済みのリスト.完全木(=root がリストの要素のちょうど半分))

```javascript
class BinaryTree {
  constructor(data, left = null, right = null) {
    this.data = data;
    // 左の二分木
    this.left = left;
    // 右の二分木
    this.right = right;
  }
}

function sortedArrayToBSTHelper(arr, start, end) {
  //全て左の木、右の木それぞれstartとendが一緒になったらそれを根ノードにして左右nullを返す。
  if (start === end) return new BinaryTree(arr[start], null, null);

  let mid = Math.floor((start + end) / 2);

  let left = null; //0~真ん中まで
  if (mid - 1 >= start) left = sortedArrayToBSTHelper(arr, start, mid - 1); //左の木からさらに左、真ん中、右と分かれる。
  //↓
  let right = null; //真ん中~最後まで
  if (mid + 1 <= end) right = sortedArrayToBSTHelper(arr, mid + 1, end);
  //↓
  let root = new BinaryTree(arr[mid], left, right); //左、右で再帰される毎にrootが定義されて、treeが出来上がってくる。
  //↓左右のstartendが同じだった場合。↑または、Helperで左右再度ループ。->rootに合体
  return root; //完成された木が返ってくる。
}

function sortedArrayToBST(nums) {
  if (nums.length == 0) return null;
  return sortedArrayToBSTHelper(nums, 0, nums.length - 1);
}

let balancedBST = sortedArrayToBST([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]); //Helperに飛ばされ、startとendを定義され、完全木にする関数へ。
console.log(balancedBST);
```

- 上のコードで完全二本木にしてから、探索すると

```javascript
// BSTリストの中にキーが存在かどうかによって、true、falseを返します。
// 再帰
function keyExist(key, bst) {
  if (bst == null) return false;
  if (bst.data == key) return true;

  // 現在のノードよりキーが小さければ左に、大きければ右に辿ります。
  if (bst.data > key) return keyExist(key, bst.left);
  else return keyExist(key, bst.right);
}

let balancedBST = sortedArrayToBST([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]);
console.log(balancedBST);
console.log(keyExist(6, balancedBST));
console.log(keyExist(10, balancedBST));
console.log(keyExist(45, balancedBST));
```

- 連結リストと while 使って探索することもできる。

```javascript
// BSTリストの中にキーが存在かどうかによって、true、falseを返します。
function keyExist(key, bst) {
  let iterator = bst;
  while (iterator != null) {
    if (iterator.data == key) return true;
    // 現在のノードよりキーが小さければ左に、大きければ右に辿ります。
    if (iterator.data > key) iterator = iterator.left;
    else iterator = iterator.right;
  }

  return false;
}

let balancedBST = sortedArrayToBST([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]);
console.log(keyExist(6, balancedBST));
console.log(keyExist(10, balancedBST));
console.log(keyExist(45, balancedBST));
```

- BinarySearchTree という構造体を作る

```javascript
class BinaryTree {
  constructor(data, left = null, right = null) {
    this.data = data;
    this.left = left;
    this.right = right;
  }
}

// BinarySearchTreeという構造体を作成してください。
class BinarySearchTree {
  constructor(arrList) {
    let sortedList = arrList.sort(function (a, b) {
      return a - b;
    });
    this.root = BinarySearchTree.sortedArrayToBST(sortedList);
  }

  static sortedArrayToBST(array) {
    if (array.length == 0) return null;
    return BinarySearchTree.sortedArrayToBSTHelper(array, 0, array.length - 1);
  }

  static sortedArrayToBSTHelper(arr, start, end) {
    if (start === end) return new BinaryTree(arr[start], null, null);

    let mid = Math.floor((start + end) / 2);

    let left = null;
    if (mid - 1 >= start)
      left = BinarySearchTree.sortedArrayToBSTHelper(arr, start, mid - 1);

    let right = null;
    if (mid + 1 <= end)
      right = BinarySearchTree.sortedArrayToBSTHelper(arr, mid + 1, end);

    let root = new BinaryTree(arr[mid], left, right);
    return root;
  }

  keyExist(key) {
    let iterator = this.root;
    while (iterator != null) {
      if (iterator.data == key) return true;
      if (iterator.data > key) iterator = iterator.left;
      else iterator = iterator.right;
    }

    return false;
  }

  search(key) {
    let iterator = this.root;
    while (iterator != null) {
      if (iterator.data == key) return iterator;
      if (iterator.data > key) iterator = iterator.left;
      else iterator = iterator.right;
    }

    return null;
  }
}

let balancedBST = new BinarySearchTree([1, 2, 3, 4, 5, 6, 7]);
//6があるかどうか
console.log(balancedBST.keyExist(6)); //true
console.log(balancedBST.search(6)); //6と6から下のtreeも全て帰ってくる。
// BinaryTree {
//   data: 6,
//   left: BinaryTree { data: 5, left: null, right: null },
//   right: BinaryTree { data: 7, left: null, right: null }
// }
```

- 二分探索木内探索

```javascript
class BinaryTree {
  constructor(data, left = null, right = null) {
    this.data = data;
    this.left = left;
    this.right = right;
  }
}

function bstSearch(root, key) {
  // 関数を完成させてください
  let currentNode = root;
  while (currentNode != null) {
    if (currentNode.data == key) return currentNode;
    if (currentNode.data > key) currentNode = currentNode.left;
    else currentNode = currentNode.right;
  }
  return null;
}
//bstSearch( toBinaryTree([0,-10,5,null,-3,null,9]), 5 )--> [5,null,9]
```

- 二分探索木内の最小値

```javascript
class BinaryTree {
  constructor(data, left = null, right = null) {
    this.data = data;
    this.left = left;
    this.right = right;
  }
}

function minimumNode(root) {
  //左にいけなかっら
  let currentNode = root;
  while (currentNode != null) {
    if (currentNode.left == null) return currentNode;
    currentNode = currentNode.left;
  }
  return null;
}
//minimumNode( toBinaryTree([0,-10,5,null,-3,null,9]) )--> [-10,null,-3]
```

### 木構造の走査(traverse)

全 node を一回ずつ調べる(連結リストでいう currentNode=currentNode.next).一方、木構造では少し異なったやり方で走査する。
->深さ(currentNode = currentNode.next のように、子ノードへ降りていく。)
->幅(同じ高さの node を調べていく。横同士の走査、下へ降りる、横、、、)

- 深さ優先走査(pre-order(前順), in-order(間順), post-order(後順), reverse-order(逆間順), )//再帰で表せる。

- in-order//左、根、右の順 ベースケース:根ノードが null になったら。
- 間順 inOrderWalk が今回大事な関数
- https://www.momoyama-usagi.com/entry/info-algo-tree-traverse#i-8

```javascript
class BinaryTree {
  constructor(data, left = null, right = null) {
    this.data = data;
    this.left = left;
    this.right = right;
  }

  printInOrder() {
    this.inOrderWalk(this);
    console.log("");
  }
  //間順 inOrder
  inOrderWalk(tRoot) {
    if (tRoot != null) {
      //左がnullになるまで再帰
      this.inOrderWalk(tRoot.left);
      process.stdout.write(tRoot.data + " ");
      //右がnullになるまで再帰
      this.inOrderWalk(tRoot.right);
    }
  }
}

class BinarySearchTree {
  constructor(arrList) {
    // JS ソートライブラリの sort() は文字列に対しては問題なくソートしますが、数値に対しては特別なルールが必要です。sort関数は引数に関数を指定でき、関数でソートのルールを定義できます。aとbを比較する関数で、a-bが負の場合はa < b、0の場合はa == b、そうでない場合はa > bとなります。自分でソート関数を作成することができます。
    let sortedList = arrList.sort(function (a, b) {
      return a - b;
    });
    this.root = BinarySearchTree.sortedArrayToBST(sortedList);
  }

  static sortedArrayToBST(array) {
    if (array.length == 0) return null;
    return BinarySearchTree.sortedArrayToBSTHelper(array, 0, array.length - 1);
  }

  static sortedArrayToBSTHelper(arr, start, end) {
    if (start === end) return new BinaryTree(arr[start], null, null);

    let mid = Math.floor((start + end) / 2);

    let left = null;
    if (mid - 1 >= start)
      left = BinarySearchTree.sortedArrayToBSTHelper(arr, start, mid - 1);

    let right = null;
    if (mid + 1 <= end)
      right = BinarySearchTree.sortedArrayToBSTHelper(arr, mid + 1, end);

    let root = new BinaryTree(arr[mid], left, right);
    return root;
  }

  keyExist(key) {
    let iterator = this.root;
    while (iterator != null) {
      if (iterator.data == key) return true;
      if (iterator.data > key) iterator = iterator.left;
      else iterator = iterator.right;
    }

    return false;
  }

  search(key) {
    let iterator = this.root;
    while (iterator != null) {
      if (iterator.data == key) return iterator;
      if (iterator.data > key) iterator = iterator.left;
      else iterator = iterator.right;
    }

    return null;
  }

  printSorted() {
    this.root.printInOrder();
  }
}

let balancedBST = new BinarySearchTree([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]);
balancedBST.printSorted();
```

- pre-order(前順)// 根->左->右
  ,,,一番左の node まで行って、null だったら、どうやって次の枝に移っているの???

```javascript
    // 前順（pre-order）（NLR）
    preOrderWalk(tRoot){
        if(tRoot != null){
            process.stdout.write(tRoot.data + " ");
            this.preOrderWalk(tRoot.left);
            this.preOrderWalk(tRoot.right);
        }
    }
```

- post-order//左->右->根

```javascript
  // 後順（post-order）（LRN）
    pastOrderWalk(tRoot){
        if(tRoot != null){
            this.pastOrderWalk(tRoot.left);
            this.pastOrderWalk(tRoot.right);
            process.stdout.write(tRoot.data + " ");
        }
    }
```

- reverse-order//右->根->左

```javascript
    // 逆間順（reverse-order）（RNL）
    reverseOrderWalk(tRoot){
        if(tRoot != null){

            this.reverseOrderWalk(tRoot.right);
            process.stdout.write(tRoot.data +" ");
            this.reverseOrderWalk(tRoot.left);

        }
    }
```

---

-　前順(二分木)
helper(root.left, arr);は根 node だけではなく、左右が null だった場合、呼び出し元に戻る->そこで今までの node を push すると言う役割がある。だから、左右ちゃんと arr に push されている。。

```javascript
class BinaryTree {
  constructor(data, left = null, right = null) {
    this.data = data;
    this.left = left;
    this.right = right;
  }
}

function preorderTraversal(root) {
  // ヘルパー関数を使います。
  return helper(root, []);
}

function helper(root, arr) {
  // ベースケース　根ノードがnullになると終了します。
  if (root == null) return null;

  // 根ノードのdataをarrにpushします。
  arr.push(root.data);
  // 左側のノードへ移動します。
  helper(root.left, arr);
  // 右側のノードへ移動します。
  helper(root.right, arr);

  return arr;
}
//preorderTraversal( toBinaryTree([0,-10,5,null,-3,null,9]) )--> [0,-10,-3,5,9]
```

- 間順(二分木)//左、根、右// Inorder Traversal

```javascript
class BinaryTree {
  constructor(data, left = null, right = null) {
    this.data = data;
    this.left = left;
    this.right = right;
  }
}

function inorderTraversal(root) {
  return helper(root, []);
}

function helper(root, arr) {
  if (root == null) return null; //arr[], null,null?
  //どうやってarrにpushされてるの??
  helper(root.left, arr);
  //これは、根ノードではなく、nullだった場合、呼び出し元へ戻るコードか。。ここでpushされている。
  arr.push(root.data);
  helper(root.right, arr);

  return arr;
}
//inorderTraversal( toBinaryTree([0,-10,5,null,-3,null,9]) )--> [-10,-3,0,5,9]
```

- 完全二分木のノード(木構造 node のカウント)

```javascript
class BinaryTree {
  constructor(data, left = null, right = null) {
    this.data = data;
    this.left = left;
    this.right = right;
  }
}

function countNodes(root) {
  // ヘルパー関数を使います。
  return helper(root, []);
}

let count = 0;
function helper(root, arr) {
  // ベースケース　根ノードがnullになると終了します。
  if (root == null) return null;

  // 根ノードのdataをarrにpushします。
  arr.push(root.data);
  count++;
  // 左側のノードへ移動します。
  if (root == root.left) sameNode = root;
  helper(root.left, arr);
  // 右側のノードへ移動します。
  helper(root.right, arr);

  return count;
}
```

### 木構造の削除

```javascript
function transplant(root, nodeParent, node, target)
    if(nodeParent == null) root = target
    else if(nodeParent.left == node) nodeParent.left = target
    else nodeParent.right = target

function deleteNode(root, node)
    if(root == null) return null
    if(!exists(root,node)) return root

    parent = findParent(root, node)
    // case 1: ノード N が葉ノード 親ノードから node への参照を null に変更して node を削除します。
    if (node.left == null && node.right == null) {
      if(parent.left.data == key) parent.left = null;
      else if(parent.right.data == key) parent.right = null;
      // return;
    }

    // case 2: ノード N の左が空
    if(node.left == null) transplant(root, parent, node, node.right)
    // case 3: ノード N の右が空
    else if(node.right == null) transplant(root, parent, node, node.left)
    // case 4: 2 つの子を持っている場合
    else
        S = findSuccessor(root, node)
        SP = findParent(root, successor)

    // case 3 後続ノードSがすぐ右側にいる場合:
    // この場合、ノード N が後続ノード S の親になっているため、case 4 は必要ありません。
    // 単純に N の親である P の部分木と S を移植すれば ok です。

    // 特別なケース (case 4) 後続ノード S がすぐ右側にいない場合:
    // この場合、後続 S の親も変更しなければいけません。
    if(successor != node.right)
        // 後続ノード S を S の右部分木で移植します。S をアップデートします。
        transplant(root, SP, S, S.right)
        // S の右側はノード N の右側になっている必要があります。
        S.right = node.right

    // ノード N を後続 S で移植します。S の左部分木をノード N の左部分木にします。
    transplant(root, parent, node, S)
    S.left = node.left
```

- 二分探索木への挿入

```javascript
class BinaryTree {
  constructor(data, left = null, right = null) {
    this.data = data;
    this.left = left;
    this.right = right;
  }
}

function bstInsert(root, key) {
  // nullまでたどりついた?
  if (root == null) return new BinaryTree(key);
  // keyがrootにあったら、rootを返す//この問題は、そういうルール
  if (root.data == key) return root;
  //keyの方が大きかったら、右へ進む。
  if (key > root.data) root.right = bstInsert(root.right, key);
  //左へ進む
  else root.left = bstInsert(root.left, key);
  return root;
}
//bstInsert( toBinaryTree([0,-10,5,null,-3,null,9]), 2 )--> [0,-10,5,null,-3,2,9]
```

- 有効な二分木わからん

### binary heap

#### max heapfi

- (arr,target)を受け取り、target が max heap を維持するような場所に挿入する。

```javascript
class Heap {
  static left(i) {
    return 2 * i + 1;
  }

  static right(i) {
    return 2 * i + 2;
  }

  static maxHeapify(arr, i) {
    //([2,42,11,30,10,7,6,5,9],0);
    let l = Heap.left(i); //Heap.left(0)//1
    let r = Heap.right(i); //Heap.right(0)//2

    let biggest = i; //最大の要素ではなく、最大の要素のindex//こう表したいからarr[biggest];
    //arr[biggest],左,右の中で最大値を出している
    if (arr[l] > arr[biggest]) biggest = l; //arr[1]>arr[0]//42>2
    if (arr[r] > arr[biggest]) biggest = r; //arr[2]>arr[0]//11>2

    //最大値が根ノードindex(根ノード)ではなかったら、
    //それを最大値にして、Heap.maxHeapify(arr, biggest);で再び同じことを実行する。
    if (biggest != i) {
      let temp = arr[i];
      //根ノード = 最大値とスワップ
      arr[i] = arr[biggest];
      //根ノードを最大値とスワップ
      arr[biggest] = temp;
      Heap.maxHeapify(arr, biggest);
    }
  }
}

let heap1 = [2, 42, 11, 30, 10, 7, 6, 5, 9];
//0をarr内に挿入して、max heapfyに近づける
//(arr,ターゲットindex)//つまり、2を最大ヒープの場所に移動させる。
//そもそもこのtreeは最大ヒープになっていることを前提としている。

Heap.maxHeapify(heap1, 0); // 根ノードが2で、2 < 42のため、最大ヒープではありません。
console.log(heap1); //[42, 30, 11, 9, 10,7,  6,  5, 2]

let heap2 = [56, 4, 51, 10, 12, 5, 12, 4, 6, 5];
Heap.maxHeapify(heap2, 1); // インデックス1が4で、4 < 10のため、最大ヒープではありません。
console.log(heap2); //[56, 12, 51, 10, 5,5, 12,  4,  6, 4]
//これで最大ヒーププロパティを満たしていないが、左右の部分木が最大ヒープである根ノードを持つ二分木を「ヒープ化」した。
```

#### 最大ヒープ(max heap)の構築

- 与えられた配列から最大ヒープを構築する。
- 葉ノードの数は全体のノード数の半分
- 葉ノードの親ノードから max heapfy し、頂上まで登っていく。

```javascript
class Heap {
  static left(i) {
    return 2 * i + 1;
  }

  static right(i) {
    return 2 * i + 2;
  }
  //何?
  static parent(i) {
    return Math.floor((i - 1) / 2);
  }

  static maxHeapify(arr, i) {
    let l = Heap.left(i);
    let r = Heap.right(i);

    let biggest = i;
    if (l < arr.length && arr[l] > arr[biggest]) biggest = l;
    if (r < arr.length && arr[r] > arr[biggest]) biggest = r;

    if (biggest != i) {
      let temp = arr[i];
      arr[i] = arr[biggest];
      arr[biggest] = temp;
      Heap.maxHeapify(arr, biggest);
    }
  }

  // builMaxHeapを実装します。
  static buildMaxHeap(arr) {
    let middle = Heap.parent(arr.length);
    // 最後の葉ノードの親ノードから根ノードまで maxHeapfy します。
    for (let i = middle; i >= 0; i--) {
      Heap.maxHeapify(arr, i);
    }
  }
}

let heap1 = [2, 42, 11, 30, 10, 7, 6, 5, 9];
console.log(heap1);
Heap.buildMaxHeap(heap1);
// 根ノードが最大値となり木全体が最大ヒープになりました。
console.log(heap1); // 42, 30, 11, 9, 10, 7, 6, 5, 2

let heap2 = [56, 4, 51, 10, 12, 5, 12, 4, 6, 5];
console.log(heap2);
Heap.buildMaxHeap(heap2);
console.log(heap2); // 56, 12, 51, 10, 5, 5, 12, 4, 6, 4
```

- ヒープを使った配列のソート(二分ツリーを max heapfy を使ってソートした配列にする)

```javascript
class Heap {
  //省略
  //BST作って、,,コードを
  static left(i) {
    return 2 * i + 1;
  }

  static right(i) {
    return 2 * i + 2;
  }

  static parent(i) {
    return Math.floor((i - 1) / 2);
  }

  // ヒープのサイズを追跡するために maxHeapify を拡張します
  static maxHeapify(arr, heapEnd, i) {
    let l = Heap.left(i);
    let r = Heap.right(i);
    let biggest = i;

    // heapEnd より後ろはすでにソートされているので、l と　r のインデックスは heapEnd までを比較します。
    if (l <= heapEnd && arr[l] > arr[biggest]) biggest = l;
    if (r <= heapEnd && arr[r] > arr[biggest]) biggest = r;

    if (biggest != i) {
      let temp = arr[i];
      arr[i] = arr[biggest];
      arr[biggest] = temp;
      Heap.maxHeapify(arr, heapEnd, biggest);
    }
  }

  static buildMaxHeap(arr) {
    let middle = Heap.parent(arr.length);
    //葉ノードは全体のノードの数の半分だから。
    for (let i = middle; i >= 0; i--) {
      Heap.maxHeapify(arr, arr.length - 1, i);
    }
  }

  // ヒープソートを実装します。
  static heapSort(arr) {
    //ここでmax heapが出来た。
    Heap.buildMaxHeap(arr);
    // ヒープサイズ == 配列の長さ
    let heapEnd = arr.length - 1;
    //ヒープサイズ回whileする。
    while (heapEnd > 0) {
      //ここでarrはすでに最大ヒープ。

      //根ノード(==最大値かつarr[0])と一番右下の葉ノード(==最小値かつarr[arr.length-1]）を入れ替える。
      let temp = arr[heapEnd];
      arr[heapEnd] = arr[0];
      arr[0] = temp;

      //　一番最後はソートされているので、heapEnd から 1 引く
      heapEnd--;
      //頂上と現時点で右下のノードを最大ヒープして直す。
      Heap.maxHeapify(arr, heapEnd, 0);
    }
  }
}

let heap1 = [2, 42, 11, 30, 10, 7, 6, 5, 9];
console.log(heap1);
Heap.heapSort(heap1);
console.log(heap1); // 昇順にソートされました。

let heap2 = [56, 4, 51, 10, 12, 5, 12, 4, 6, 5];
console.log(heap2);
Heap.heapSort(heap2);
console.log(heap2);
```

#### 優先度付きキュー

ヒープは、優先度付きキューを表すのに最適な構造

- 最大ヒープ(問題)

```javascript
function buildMaxHeap(intArr) {
  // 関数を完成させてください
}
//buildMaxHeap([7,8,2,3,1,4,3,2]) --> [8,7,4,3,1,2,3,2]
```

- 最大ヒープ化やり方

```javascript
class Heap {
  static left(i) {
    return 2 * i + 1;
  }

  static right(i) {
    return 2 * i + 2;
  }

  static parent(i) {
    return Math.floor((i - 1) / 2);
  }
  //Heap.maxHeapify([2,42,11,30,10,7,6,5,9],4);
  //Heap.maxHeapify([2,42,11,30,10,7,6,5,9],3);
  //Heap.maxHeapify([2,42,11,30,10,7,6,5,9],2);
  //Heap.maxHeapify([2,42,11,30,10,7,6,5,9],1);
  //Heap.maxHeapify([2,42,11,30,10,7,6,5,9],0);
  static maxHeapify(arr, i) {
    let l = Heap.left(i);
    let r = Heap.right(i);

    let biggest = i; //4
    //childrenが!null && 左の方が親より大きい
    if (l < arr.length && arr[l] > arr[biggest])
      //最大nodeを左の木にする
      biggest = l;
    //childrenが!null && 右の方が親より大きい
    if (r < arr.length && arr[r] > arr[biggest])
      //最大nodeを右の木にする
      biggest = r;

    //上の結果、最大が親ではなく左右の木どちらかだったら
    //arr[i] = arr[biggest];でswap
    if (biggest != i) {
      let temp = arr[i];
      arr[i] = arr[biggest];
      arr[biggest] = temp;
      Heap.maxHeapify(arr, biggest);
    }
  }

  // builMacHeapを実装します。
  //Heap.buildMaxHeap([2,42,11,30,10,7,6,5,9]);
  static buildMaxHeap(arr) {
    //二分ヒープは完全二分木（complete binary tree）であり、これは全ノードの半分が葉ノードであることを意味します。
    //↓
    //一番右下の葉ノードから左に登っていく。
    let middle = Heap.parent(arr.length);
    // 葉ノードを飛ばしてからループスタート
    //[2,42,11,30,10,]//10入る?入らない?わからん。
    for (let i = middle; i >= 0; i--) {
      //これせっかく半分にしたのにまた全部arr渡すの??
      Heap.maxHeapify(arr, i);
    }
  }
}

let heap1 = [2, 42, 11, 30, 10, 7, 6, 5, 9];
console.log(heap1);
Heap.buildMaxHeap(heap1);
// 根ノードが最大値となり木全体が最大ヒープになりました。
console.log(heap1); // 42, 30, 11, 9, 10, 7, 6, 5, 2

let heap2 = [56, 4, 51, 10, 12, 5, 12, 4, 6, 5];
console.log(heap2);
Heap.buildMaxHeap(heap2);
console.log(heap2); // 56, 12, 51, 10, 5, 5, 12, 4, 6, 4
```

- 最大ヒープ

```javascript
function buildMaxHeap(intArr) {
  const arr = intArr;

  // 配列の長さ（全ノード）から半分の値を求める（葉ノード）
  let middle = Math.floor((intArr.length - 1) / 2);

  // 最後の葉ノードの親ノードから根ノードまで maxHeapfy を繰り返す
  for (let i = middle; i >= 0; i--) {
    maxHeapify(arr, i);
  }

  return arr;
}

function maxHeapify(arr, i) {
  let l = 2 * i + 1;
  let r = 2 * i + 2;

  let biggest = i;
  if (l < arr.length && arr[l] > arr[biggest]) biggest = l;
  if (r < arr.length && arr[r] > arr[biggest]) biggest = r;

  if (biggest !== i) {
    let temp = arr[i];
    arr[i] = arr[biggest];
    arr[biggest] = temp;
    maxHeapify(arr, biggest);
  }
}
//buildMaxHeap([8,4,13,10,18]) --> [18,10,13,8,4]
```

- 最小ヒープ

```javascript
//最大ヒープ
function maxHeapify(heap, i) {
  let left = 2 * i + 1 < heap.length ? 2 * i + 1 : null;
  let right = 2 * i + 2 < heap.length ? 2 * i + 2 : null;
  if (left === null && right === null) return;

  let biggest = i;
  if (heap[left] > heap[biggest]) {
    biggest = left;
  }
  if (heap[right] > heap[biggest]) {
    biggest = right;
  }

  if (biggest !== i) {
    let tmp = heap[biggest];
    heap[biggest] = heap[i];
    heap[i] = tmp;
    maxHeapify(heap, biggest);
  }
}

function buildMaxHeap(intArr) {
  let current = Math.floor((intArr.length - 1 - 1) / 2);
  while (0 <= current) {
    maxHeapify(intArr, current);
    current--;
  }
  return intArr;
}
```

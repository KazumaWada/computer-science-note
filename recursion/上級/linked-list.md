# Data Structure

紙のノート: https://kazumawada.notion.site/Linked-list-1a2d450fb35d80eeb24ac6329f123eca?pvs=4

data structure

- list[]
- linked list

## 連結リスト(メモリアドレスが等しく連続していないリスト。pointer で繋がっている。)

## queue と stack では、list より linkedList の方が効果的に計算できる。

- list は一度作ってしまうと、削除追加が難しいが、linked list は pointer の先を変えるだけで簡単にそれらを実装することができる。

https://ja.wikipedia.org/wiki/%E9%80%A3%E7%B5%90%E3%83%AA%E3%82%B9%E3%83%88#%E9%80%A3%E7%B5%90%E3%83%AA%E3%82%B9%E3%83%88%E3%81%A8%E9%85%8D%E5%88%97

- メモリ上のリストを表現することができる.
- list で言う array[0]は、連結リストでは node と呼ぶ。
- node と辺で出来ている。
- node の pointer を辿っていけば、欲しい情報がある node にたどり着く。

リストとは違い、データが連続したメモリに保存されないデータ構造。つまり、for,index でとってくることができないって事かな？

![](/上級//FireShot Capture 269 - Recursion - コンピュータサイエンスを基礎から学べるプラットフォーム - recursionist.io.png)

> 連結リストはポインタによってデータどうしがリンクされているシーケンスを持つデータ構造です。したがって、配列のときのようにデータは連続したメモリには格納されません。

list: adress が連続している配列
連結 list:pointar で data 同士がリンクされているので、連続してメモリに保存されているわけではない。メモリ上のリストを表現できる。

- 連結リストの各要素は node と呼ばれる。node は object である。
- グラフ理論と一緒に使われることがある。枝のように node と次の node が繋がっている。つまり、node が次の node と繋がっている。従って、リンクを辿るだけで、nodeX から nodeY へと移動づることができる。

### 連結リストの中の singly linked list という種類のリスト

- 最も単純な連結リスト. node = そのメモリに入る data + 次の node がどこなのかを示す Next(= pointer))の 2 つが object によって表現されている。node は object なので、次の node を示すコードは、メモリアドレスである事に注意。
  ↓
  pointer は、node 内の関数に書かれているため、メモリアドレスを参照している。
- 先頭は、インデックスの 0 ではない可能性もある。
- 末尾は pointer に null を当てる。

```java
class Node{
    public int data;
    public Node next;

    public Node(int data){
        this.data = data;
    }
}

class SinglyLinkedList{
    public Node head;
    public SinglyLinkedList(Node head){
        this.head = head;
    }
}

class Main{
    public static void main(String[] args){
    Node node1 = new Node(4);
    Node node2 = new Node(65);
    Node node3 = new Node(24);

    SinglyLinkedList numList = new SinglyLinkedList(node1);
    }

}
```

```java
class Node {
    public int data;
    public Node next;

    public Node(int data) {
        this.data = data;
        // nextを割り当てないでください。オブジェクト変数はヒープアドレスへの参照を保持するだけで、アクセス演算子「.」を使ってデータにアクセスします。オブジェクトが何も代入されていない場合は、何も指していないのでnullを保持します。
    }
}

// 先頭のノードを保持するコンテナで、リスト自体を表します。
class SinglyLinkedList{
    public Node head;

    public SinglyLinkedList(Node head){
        this.head = head;
    }
}

class Main{
    public static void main(String[] args){
        // 3つのノードを作成します。
        Node node1 = new Node(4);
        Node node2 = new Node(65);
        Node node3 = new Node(24);

        SinglyLinkedList numList = new SinglyLinkedList(node1);

        // リストに他のリストを追加します。
        // nodeはオブジェクトなので、=は値ではなく、メモリアドレスを指している点に注意してください。
        numList.head.next = node2;
        numList.head.next.next = node3;

        // 連結リストを反復します。
        // 反復によって、現在のノードは次のノードになります。この処理を最後のノードまで繰り返します。
        Node currentNode = numList.head;
        while(currentNode != null){
            // 現在のノードを出力します。
            System.out.println(currentNode.data);
            currentNode = currentNode.next;
        }
    }
}
```

```java
class Node{
    public int data;
    public Node next;

    public Node(int data){
        this.data = data;
    }
}

class SinglyLinkedList{
    public Node head;

    public SinglyLinkedList(Node head){
        this.head = head;
    }
}

class Main{
    public static void main(String[] args){

        int[] arr = new int[]{35,23,546,67,86,234,56,767,34,1,98,78,555};

        SinglyLinkedList numlist = new SinglyLinkedList(new Node(arr[0]));

        Node currentNode = numlist.head;
        for(int i = 1; i < arr.length; i++) {
            currentNode.next = new Node(arr[i]);
            currentNode = currentNode.next;
        }

        currentNode = numlist.head;
        while(currentNode != null){
            System.out.println(currentNode.data);
            currentNode = currentNode.next;
        }

    }
}
```

## 片方向リスト

- 行える操作(index, 挿入, 削除)
- list はデータの変更のみできるが、linkedList は、変更、削除、追加ができる。
  https://ja.wikipedia.org/wiki/%E9%80%A3%E7%B5%90%E3%83%AA%E3%82%B9%E3%83%88#%E7%89%87%E6%96%B9%E5%90%91%E3%83%AA%E3%82%B9%E3%83%88_2

### index

配列は、O(1)でアクセスできるが、片方向リストは、O(n)かかってしまう。取得したい要素まで、pointer を辿る必要がある。

連結リストから index を探すコード。
このコードを見ていて思ったけど、リストは全て繋がって一つだけど、連結リストは全て散らばっていて pointer で繋がっているってことかな??

- その Node の index にある data を特定する&&その Node の index を特定する

```java
class Node{
    public int data;
    public Node next;

    public Node(int data){
        this.data = data;
    }
}

class SinglyLinkedList{
    public Node head;
    //配列をlinkedListにする。
    public SinglyLinkedList(int[] arr){
        this.head = arr.length > 0 ? new Node(arr[0]) : null;

        Node currentNode = this.head;
        for(int i = 1; i < arr.length; i++){
            currentNode.next = new Node(arr[i]);
            currentNode = currentNode.next;
            System.out.print(currentNode.data + " ");
        }
    }

    //linkedListのindexを辿る
    public Node at(int index){
        Node iterator = this.head;
        for(int i = 0; i < index; i++){
            iterator = iterator.next;
            if(iterator == null) return null;
        }
        return iterator;
    }

    //linkedListの要素のindexを特定する
        public int findNode(int key){
        int count = 0;
        Node iterator = this.head;
        while(iterator != null){
            if(iterator.data == key) return count;
            iterator = iterator.next;
            count++;
        }
        return -1;
    }
}

class Main{
    public static void main(String[] args){
        int[] arr = new int[]{35,23,546,67,86,234,56,767,34,1,98,78,555};
        SinglyLinkedList numList = new SinglyLinkedList(arr);
        // 連結リストを反復します。
        System.out.println(numList.at(2).data);
        System.out.println(numList.at(12).data);
        System.out.println(numList.findNode(67));
        System.out.println(numList.findNode(767));

        // System.out.println(numList.at(13).data); // a(13)はnullを返すので、エラーになります。
    }
}
```

### insert

- list は、サイズが固定されているため、新しく増やそうとしたら、また新たに配列を作成することが必要。o(n)
- linked list は、前後の pointer の先を変更するだけで要素を insert することができる。o(1)

```java
// 入力のデータ型： SinglyLinkedListNode<integer> head, integer position, integer data

// 出力のデータ型： SinglyLinkedListNode<integer>

// insertAtPosition(singlyLinkedList([2,10,34,45,67,356]),2,5) --> 2➡10➡34➡5➡45➡67➡356➡END


class SinglyLinkedListNode<E>{
    public E data;
    public SinglyLinkedListNode<E> next;

    public SinglyLinkedListNode(E data){
        this.data = data;
        this.next = null;
    }
}

class Solution{
    public static SinglyLinkedListNode<Integer> insertAtPosition(SinglyLinkedListNode<Integer> head, int position, int data){

        SinglyLinkedListNode<Integer> currentNode = head;

        for(int i = 0; i<position; i++){
            if(currentNode.next == null)return head;
            currentNode = currentNode.next;
        }

        SinglyLinkedListNode newData =new SinglyLinkedListNode(data);
        SinglyLinkedListNode<Integer> temp = currentNode.next;
        currentNode.next = newData;
        newData.next = temp;

        return head;
    }
}

```

- index が偶数番目の要素を 2 倍にする。それを要素の後ろに追加する。

```javascript
class SinglyLinkedListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
  addNextNode(newNode) {
    let tempNode = this.next;
    this.next = newNode;
    newNode.next = tempNode;
  }
}

function doubleEvenNumber(head) {
  // 関数を完成させてください
  let currentNode = head;
  console.log("currentNode" + currentNode.data); //なぜ全ての配列ではなく、配列の最初が出力されるのか。。== 連結リストだからでしょ笑笑
  let i = 0;
  while (currentNode != null) {
    //なぜわざわざ変数に格納するのか。
    //変数にしないと、メソッドチェーンが使えないからかな?
    let pointer = currentNode;
    console.log("pointer" + pointer.data);
    console.log("currentNode" + currentNode.data);
    currentNode = currentNode.next;
    if (i % 2 == 0)
      pointer.addNextNode(new SinglyLinkedListNode(pointer.data * 2));

    i++;
    //console.log(i);
  }
  return head;
}

//doubleEvenNumber(singlyLinkedList([3,1,5]))
```

- 連結リストの先頭に挿入

```javascript
class SinglyLinkedListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

function insertAtHead(head, data) {
  let newNode = new SinglyLinkedListNode(data);
  newNode.next = head;
  head = newNode;
  return head;
}

//insertAtHead(singlyLinkedList([3,3,2,10,34,45,67,356]) ,367)
```

- 連結リストの末尾に挿入(.next == null になるまで loop させて、その node の次に挿入する)

```javascript
class Node{
    constructor(data){
        this.next = null;
        this.data = data;
    }
}
    append(newNode) {
        let pointer = this.head;
        while (pointer.next !== null) {
            pointer = pointer.next;
        }
        pointer.next = newNode;
    }


let numList = new SinglyLinkedList([35,23,546,67,86,234,56,767,34,1,98,78,555]);

numList.printList();
numList.append(new Node(45));
numList.append(new Node(236));
numList.printList();
```

### delete

- 片方向リストの先頭は o(1)で削除できるが、それ以外は時間がかかる。片方向にしか進めないから。だから、先頭から順番に調べていく必要がある。
- 先頭の削除は、b 該当する pointer まで行って、pointer.next = pointer.next.next をする。

```javascript
class SinglyLinkedList{
    public Node head;

    public SinglyLinkedList(int[] arr){

    // リストの先頭の要素をポップします。O(1)
    //先頭の削除は、pointerを飛ばして次に当てればいいだけ。
    public void popFront(){
        this.head = this.head.next;
    }

    public void delete(int index){
        if (index == 0){
            this.popFront();
            return;
        }
        Node iterator = this.head;
        // 目的のデータの手前のインデックスまで、リストの中を反復します。
        for(int i = 0; i < index-1; i++){
            // もし、次のノードがなかった場合、処理を中断します。
            if(iterator.next == null) return;
            iterator = iterator.next;
        }
        //目的のindexまで行ったら、それを飛ばして次に当てればいいだけ。
        iterator.next = iterator.next.next;
    }
}

class Main{

    public static void main(String[] args){

        int[] arr = new int[]{35,23,546,67,86,234,56,767,34,1,98,78,555};
        SinglyLinkedList numList = new SinglyLinkedList(arr);

        numList.printList();

        System.out.println("popFront");
        numList.popFront();
        System.out.println("popFront");
        numList.popFront();
        numList.printList();

        System.out.println("delete index:4");
        numList.delete(4);
        numList.printList();

        System.out.println("delete index: 9");
        numList.delete(9);
        numList.printList();
    }
}
```

- リストを逆順にする。

```java
class Node{
    public int data;
    public Node next;

    public Node(int data){
        this.data = data;
    }

    public void addNextNode(Node newNode){
        Node tempNode = this.next;
        this.next = newNode;
        newNode.next = tempNode;
    }
}

class SinglyLinkedList{
    public Node head;

    public SinglyLinkedList(int[] arr){
        this.head = arr.length > 0? new Node(arr[0]) : null;

        Node currentNode = this.head;
        for(int i=1; i < arr.length; i++){
            currentNode.next = new Node(arr[i]);
            currentNode = currentNode.next;
        }
    }

    public Node at(int index){
        Node iterator = this.head;
        for(int i=1; i < index; i++){
            iterator = iterator.next;
            if(iterator == null) return null;
        }
        return iterator;
    }

    public void preappend(Node newNode){
        newNode.next = this.head;
        this.head = newNode;
    }

    public void popFront(){
        this.head = this.head.next;
    }

    public void delete(int index){
        if (index == 0) this.popFront();
        Node iterator = this.head;
        for(int i = 0; i < index-1; i++){
            if(iterator.next == null) return;
            iterator = iterator.next;
        }
        iterator.next = iterator.next.next;
    }

    public void reverse(){
        if(this.head == null || this.head.next == null) return;

        // オブジェクトなので、=は実際の値を格納しているわけではなく、メモリアドレスを指している点に十分注意ください。
        // A -> B -> C を、C -> B -> Aに変更する場合は、向きに少し混乱するのでゆっくり解読しましょう。

        //末尾がnullだったのを、先頭をnullにする。
        Node reverse = this.head;
        this.head = this.head.next;
        reverse.next = null;

        while(this.head != null){
            // =はメモリアドレスを指します。紙に書いてロジックを確認しましょう。
            Node temp = this.head;
            this.head = this.head.next;
            temp.next = reverse;
            reverse = temp;
        }
        // 処理が終わったら、headのnextを反転したリストを含むtempHeadに割り当てましょう。
        this.head = reverse;
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

        int[] arr = new int[]{35,23,546,67,86,234,56,767,34,1,98,78,555};
        SinglyLinkedList numList = new SinglyLinkedList(arr);

        numList.printList();
        numList.reverse();
        numList.printList();
    }
}
```

## doubly linked list

- 次の pointer に加えて、前の pointer も持つ連結リスト。

```java
class Node{
    // 前後を追跡します。
    public int data;
    public Node prev;
    public Node next;

    public Node(int data){
        this.data = data;
    }
}

// リストは少なくとも1つのノードを持っている必要があります。
// ヌルリストをサポートしたい場合は、それに応じてコードを追加してください。
class DoublyLinkedList{
    public Node head;
    public Node tail;

    public DoublyLinkedList(int[] arr){
        // 今回は末尾を追跡します。
        if(arr.length <= 0){
            this.head = null;
            this.tail = this.head;
            return;
        }

        this.head = new Node(arr[0]);
        Node currentNode = this.head;
        for(int i=1; i < arr.length; i++){
            currentNode.next = new Node(arr[i]);
            // 次のノードの前のノードをcurrent Nodeに割り当てます。
            currentNode.next.prev = currentNode;
            currentNode = currentNode.next;
        }
        // このcurrent Nodeは最後のnodeです。
        this.tail = currentNode;
    }

    public void printList(){
        Node iterator = this.head;
        String str = "";
        while(iterator != null){
            str += iterator.data + " ";
            iterator = iterator.next;
        }
        System.out.println("[" + str + "]");
    }
}

class Main{
    public static void main(String[] args){
        int[] arr = new int[]{35,23,546,67,86,234,56,767,34,1,98,78,555};
        DoublyLinkedList numList = new DoublyLinkedList(arr);

        numList.printList();//[35 23 546 67 86 234 56 767 34 1 98 78 555 ]
        System.out.println(numList.head.data);//35
        System.out.println(numList.head.next.data);//23
        System.out.println(numList.head.next.prev.data);//35
        System.out.println(numList.tail.data);//555
        System.out.println(numList.tail.prev.data);//78

    }
}
```

### index

- singly linked list の時と同じで、線形探索(o(n))を行います。

```java
class Node{
    public int data;
    public Node prev;
    public Node next;

    public Node(int data){
        this.data = data;
    }
}

class DoublyLinkedList{
    public Node head;
    public Node tail;

    public DoublyLinkedList(int[] arr){
        if(arr.length <= 0){
            this.head = null;
            this.tail = this.head;
            return;
        }

        this.head = new Node(arr[0]);
        Node currentNode = this.head;
        for(int i=1; i < arr.length; i++){
            currentNode.next = new Node(arr[i]);
            currentNode.next.prev = currentNode;
            currentNode = currentNode.next;
        }
        this.tail = currentNode;
    }

    public Node at(int index){
        Node iterator = this.head;
        // 片方向リストと同じ処理
        for(int i=0; i < index; i++){
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
        System.out.println("[" + str + "]");
    }
}

class Main{
    public static void main(String[] args){
        int[] arr = new int[]{35,23,546,67,86,234,56,767,34,1,98,78,555};
        DoublyLinkedList numList = new DoublyLinkedList(arr);

        numList.printList();
        System.out.println(numList.at(0).data);
        System.out.println(numList.at(2).data);
        System.out.println(numList.at(12).data);

    }
}
```

- 逆表示

```java
class Node{
    public int data;
    public Node prev;
    public Node next;

    public Node(int data){
        this.data = data;
    }
}

class DoublyLinkedList{
    public Node head;
    public Node tail;

    public DoublyLinkedList(int[] arr){
        if(arr.length <= 0){
            this.head = null;
            this.tail = this.head;
            return;
        }

        this.head = new Node(arr[0]);
        Node currentNode = this.head;
        for(int i=1; i < arr.length; i++){
            currentNode.next = new Node(arr[i]);
            currentNode.next.prev = currentNode;
            currentNode = currentNode.next;
        }
        this.tail = currentNode;
    }

    public Node at(int index){
        Node iterator = this.head;
        for(int i=0; i < index; i++){
            iterator = iterator.next;
            if(iterator == null) return null;
        }
        return iterator;
    }

    public void reverse(){
        Node reverse = this.tail;
        Node iterator = this.tail.prev;

        Node currentNode = reverse;
        while(iterator != null){
            currentNode.next = iterator;

            iterator = iterator.prev;
            if(iterator != null) iterator.next = null;

            currentNode.next.prev = currentNode;
            currentNode = currentNode.next;

        }
        this.tail = currentNode;
        this.head = reverse;
        this.head.prev = null;
    }

    public void printInReverse(){
        String str = "";
        Node iterator = this.tail;
        while(iterator != null){
            str += iterator.data + " ";
            iterator = iterator.prev;
        }
        System.out.println("[" + str + "]");
    }

    public void printList(){
        Node iterator = this.head;
        String str = "";
        while(iterator != null){
            str += iterator.data + " ";
            iterator = iterator.next;
        }
        System.out.println("[" + str + "]");
    }
}

class Main{
    public static void main(String[] args){
        int[] arr = new int[]{35,23,546,67,86,234,56,767,34,1,98,78,555};
        DoublyLinkedList numList = new DoublyLinkedList(arr);

        numList.printList();
        numList.printInReverse();

        numList.printList();
        numList.reverse();
        numList.printList();
        numList.printInReverse();
    }
}
```

---

###############################javascript###########################################################
###############################javascript###########################################################
###############################javascript###########################################################
###############################javascript###########################################################

# data structure with javascript

# linked list(メモリアドレスが連続していない)

- node と pointer で構成されている

## singly linked list

```javascript
class Node {
  constructor(data) {
    this.next = null;
    this.data = data;
  }
}
// 片方向リスト
class SinglyLinkedList {
  constructor(node) {
    // 先頭を定義します。
    this.head = node; //this.head = 4
  }
}
// 3つのノードを作成します。
let node1 = new Node(4); //next,data
let node2 = new Node(65);
let node3 = new Node(24);
let numList = new SinglyLinkedList(node1);

numList.head.next = node2;
numList.head.next.next = node3;

let currentNode = numList.head;
while (currentNode !== null) {
  // 現在のノードを出力します。
  console.log(currentNode.data); //numList.head.data//new SinglyLinkedList(node1).head.data//4,65,24
  currentNode = currentNode.next;
}
```

- SinglyLinkedList クラス

```javascript
class Node {
  constructor(data) {
    this.data = data; //data = 7にするため。//nodeではない
    this.next = null;
  }
}
class SinglyLinkedList {
  constructor(node) {
    this.head = node; //¥node.head = ¥node;
  }
}
let node1 = new Node(7);
let node2 = new Node(99);
let node3 = new Node(45);
node1.next = node2;
node2.next = node3;
let numList = new SinglyLinkedList(node1);
let currentNode = numList.head;
while (currentNode != null) {
  console.log(currentNode.data);
  currentNode = currentNode.next;
}
```

- 連結リスト化

```javascript
class SinglyLinkedListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

function getLinkedList(arr) {
  let numList = new SinglyLinkedListNode(arr[0]);
  currentNode = numList;
  for (let i = 1; i < arr.length; i++) {
    currentNode.next = new SinglyLinkedListNode(arr[i]);
    currentNode = currentNode.next;
  }
  return numList;
}
console.log(getLinkedList([3, 2, 1, 5, 6, 4])); //次のarr[i]をnextでつないでいるから、numList(arr[0])(dataとnext)を返した時に、全部つながって返ってくる。

//getLinkedList([3,2,1,5,6,4]) --> 3➡2➡1➡5➡6➡4➡END
// SinglyLinkedListNode {
//   data: 3,
//   next: SinglyLinkedListNode {
//     data: 2,
//     next: SinglyLinkedListNode { data: 1, next: [SinglyLinkedListNode] }
//   }
// }
// 23➡18➡17➡23➡21➡END
```

- singlyLinkedList index(list と違い、pointer を辿る必要がある)

```javascript
class Node {
  constructor(data) {
    this.next = null;
    this.data = data;
  }
}

class SinglyLinkedList {
  constructor(arr) {
    this.head = arr.length > 0 ? new Node(arr[0]) : new Node(null);

    // =はメモリアドレスを指すので注意してください。
    let currentNode = this.head;
    for (let i = 1; i < arr.length; i++) {
      currentNode.next = new Node(arr[i]);
      currentNode = currentNode.next;
    }
  }

  //ポイントここ
  //indexの分headから辿っていくだけ。
  at(index) {
    let iterator = this.head;
    for (let i = 0; i < index; i++) {
      iterator = iterator.next;
      if (iterator == null) return null;
    }
    return iterator;
  }

  //先頭からループさせて、dataと仮引数が一致したらcountを返す。
  findNode(key) {
    let iterator = this.head; //arr[0]
    let count = 0;
    while (iterator != null) {
      if (iterator.data == key) return count;
      iterator = iterator.next;
      count++;
    }
    return null;
  }
}

let numList = new SinglyLinkedList([
  35, 23, 546, 67, 86, 234, 56, 767, 34, 1, 98, 78, 555,
]);

// 連結リストを反復します。
console.log(numList.at(2).data);
console.log(numList.at(2).next); //node(data,next)が返る。
console.log(numList.at(12).data);
//console.log(numList.at(13).data); // a(13)はnullを返すので、エラーになります。
console.log(numList.findNode(67)); //indexを返す。

// 546
// Node {
//   next: Node { next: Node { next: [Node], data: 234 }, data: 86 },
//   data: 67
// }
// 555
```

- 連結リストのインデックス検索

```javascript
class SinglyLinkedListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

function getIndexValue(head, index) {
  // head == nullの場合
  let count = 0;
  let iterator = head;
  while (iterator != null) {
    if (iterator.data == index) return count;
    count++;
    iterator = iterator.next;
  }
  return count;
}

//0ではなく1からスタート
//getIndexValue(singlyLinkedList([3,2,1,5,6,4]),1) --> 3
```

- 挿入 insert
  前後の pointer の先を変更することで実現できる。

```javascript
class Node {
  constructor(data) {
    this.next = null;
    this.data = data;
  }
}

class SinglyLinkedList {
  constructor(arr) {
    this.head = arr.length > 0 ? new Node(arr[0]) : new Node(null);

    let currentNode = this.head;
    for (let i = 1; i < arr.length; i++) {
      currentNode.next = new Node(arr[i]);
      currentNode = currentNode.next;
    }
  }

  at(index) {
    let pointer = this.head;
    for (let i = 0; i < index; i++) {
      pointer = pointer.next;
      if (pointer == null) return null;
    }

    return pointer;
  }

  // ノードのデータを出力します。
  printList() {
    let pointer = this.head;
    let str = "";
    while (pointer != null) {
      str += pointer.data + " ";
      pointer = pointer.next;
    }
    console.log(str);
  }
}

let numList = new SinglyLinkedList([
  35, 23, 546, 67, 86, 234, 56, 767, 34, 1, 98, 78, 555,
]);

console.log(numList.at(2).data);
console.log(numList.at(3).data);
console.log(numList.at(4).data);

numList.printList();

// インデックス2と3の間に40という値を挿入します。
let thirdEle = numList.at(2);
let tempNode = thirdEle.next; //numList.at(2).next;
let newNode = new Node(40);
tempNode = newNode; //40 == numList.at(2).next;//2.next ==40
newNode.next = tempNode; //2->40->3(newNode.next)

console.log(numList.at(2).data);
console.log(numList.at(3).data);
console.log(numList.at(4).data);
numList.printList();
```

```javascript
class Node {
  constructor(data) {
    this.next = null;
    this.data = data;
  }

  // 新しいノードを受けとって、次のノードに設定する
  addNextNode(newNode) {
    let tempNode = this.next;
    this.next = newNode;
    newNode.next = tempNode;
  }
}

class SinglyLinkedList {
  constructor(arr) {
    this.head = arr.length > 0 ? new Node(arr[0]) : new Node(null);

    let currentNode = this.head;
    for (let i = 1; i < arr.length; i++) {
      currentNode.next = new Node(arr[i]);
      currentNode = currentNode.next;
    }
  }

  //連結リストの出力用関数
  printList() {
    let pointer = this.head;
    let str = "";
    while (pointer != null) {
      str += pointer.data + " ";
      pointer = pointer.next;
    }
    console.log(str);
  }
}

let numList = new SinglyLinkedList([2, 54, 2, 76, 2]);
numList.printList(); //2->54->2->76->2

let pointer = numList.head; //2
let i = 0;
while (pointer != null) {
  let currentNode = pointer; //numList.head;
  pointer = pointer.next;
  if (i % 2 == 0) currentNode.addNextNode(new Node(currentNode.data * 2));
  i++;
}
//indexが偶数だったら、次のnodeに追加する。
numList.printList();
```

- 偶数番目を 2 倍

```javascript
class SinglyLinkedListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

function doubleEvenNumber(head) {
  // 関数を完成させてください
  let iterator = head;
  let i = 0;
  while (iterator != null) {
    //headをわざわざ変数に格納するのは意味がある?参照がアドレスやら何やら。。なぜpointer,currentNodeを変数に格納したりするの?なんの意味があるんだろう。ややこしい。
    //↓
    //currentNode: 今を指している。
    //iterator: 次を指している。
    //↑同じメモリから今のnode,次のnodeを定義する必要があるから。
    let currentNode = iterator;
    iterator = iterator.next;
    if (i % 2 == 0) {
      let tempNode = new SinglyLinkedListNode(currentNode.data * 2);
      currentNode.next = tempNode;
      tempNode.next = iterator;
    }
    i++;
  }
  return head;
}
```

- 先頭に挿入

```javascript
    preappend(newNode){
        newNode.next = this.head;
        this.head = newNode;
    }

let numList = new SinglyLinkedList([35,23,546,67,86,234,56,767,34,1,98,78,555]);

numList.preappend(new Node(45));
//45,35,23,546,67,86,234,56,767,34,1,98,78,555
```

- 先頭に挿入(問題)

```javascript
class SinglyLinkedListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

function insertAtHead(head, data) {
  // 関数を完成させてください
  let newData = new SinglyLinkedListNode(data);
  newData.next = head;
  head = newData;
  return head;
}
```

- 末尾に挿入

```javascript
class SinglyLinkedListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

function insertAtTail(head, data) {
  let currentNode = head;
  while (currentNode.next != null) {
    currentNode = currentNode.next;
  }
  currentNode.next = new SinglyLinkedListNode(data);
  return head;
}
//insertAtTail(singlyLinkedList([3,3,2,10,34,45,67,356]),367) --> 3➡3➡2➡10➡34➡45➡67➡356➡367➡END
```

- 先頭と末尾に挿入

```javascript
class SinglyLinkedListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

function insertHeadTail(head, data) {
  // 先頭
  let tempNode = head;
  head = new SinglyLinkedListNode(data);
  head.next = tempNode; //head.next <- head
  //tempNode = head.nextにしたら、先頭と末尾だけの出力になった。。//head <- head.next
  //末尾
  let currentNode = head;
  while (currentNode.next != null) {
    currentNode = currentNode.next;
  }
  currentNode.next = new SinglyLinkedListNode(data);
  return head;
}
//insertHeadTail(singlyLinkedList([3,3,2,10,34,45,67,356]),367) --> 367➡3➡3➡2➡10➡34➡45➡67➡356➡367➡END
```

- 真ん中のノード

```javascript
class SinglyLinkedListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

function middleNode(head) {
  let count = 0;
  let currentNode = head;
  while (currentNode.next != null) {
    currentNode = currentNode.next;
    count++;
  }
  //singlyLinkedListの長さ(count)が偶数の場合、真ん中は2つある。+1する
  let m = 0;
  //[1,2,3,4,5,6,7]count6
  if (count % 2 == 0) m = Math.floor(count / 2);
  //[1,2,3,4,5,6,7,8]count7
  else m = Math.floor(count / 2) + 1;

  currentNode = head; //末尾からheadに戻す?
  for (let i = 0; i < m; i++) {
    currentNode = currentNode.next;
  }
  return currentNode;
}
//middleNode(singlyLinkedList([1,2,3,4,5,6,7,8])) --> 4->5➡END
//正解
//middleNode(singlyLinkedList([1,2,3,4,5,6,7]))
//4➡5➡6➡7➡END
//不正解
//middleNode(singlyLinkedList([1,2,3,4,5,6,7,8]))
//4➡5➡6➡7➡8➡END//count7だった。

//片方向リストの先頭 head が与えられるので、真ん中のノードを返す、middleNode という関数を作成してください。真ん中のノードが 2 つ存在する場合は 2 つ目のノードを返してください。
//例えば、入力が [1,2,3,4,5,6,7,8] の場合、真ん中のノードは 4 と 5 の 2 つになります。この場合、2 つ目を優先させるので、答えは [5,6,7,8] になります。
//↑
//つまり5を返す。5より先に続くnodeも同様に返される。
```

- 真ん中のコード(別解)

```javascript
class SinglyLinkedListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

function middleNode(head) {
  let slow = head;
  let fast = head;

  while (fast != null && fast.next != null) {
    fast = fast.next.next;
    slow = slow.next;
  }

  return slow;
}
```

- 削除 remove
  削除したい前の Node まで行き、pointer を一つ前の Node に当てる.

```javascript
    //removeしたい値が先頭の場合
    popFront(){
        this.head = this.head.next;
    }

    delete(index){
        //removeしたい値が先頭の場合
        if(index == 0) return this.popFront();
        let iterator = this.head;
        //forで削除したい前の値まで行く。
        for(let i = 0; i < index-1; i++){
        //削除したい前のNodeまで連結リストがそもそも無かったらnull
            if(iterator.next == null) return null;
            iterator = iterator.next;
        }
        //iterator == 削除したいNodeの前のNode
        //つまり、そのNodeから伸びるpointerを一つ前のNodeに指す
        iterator.next = iterator.next.next;
    }

let numList = new SinglyLinkedList([35,23,546,67,86,234,56,767,34,1,98,78,555]);
numList.printList();
//35が消える
numList.delete(0);
numList.printList();
```

- 連結リストの逆表示 わからない!

```javascript
class SinglyLinkedList {
  constructor(arr) {
    this.head = arr.length > 0 ? new Node(arr[0]) : new Node(null);

    let currentNode = this.head;
    for (let i = 1; i < arr.length; i++) {
      currentNode.next = new Node(arr[i]);
      currentNode = currentNode.next;
    }
  }

  reverse() {
    //そもそも無かったり、一つのNodeしか無かったら。
    if (this.head === null || this.head.next === null) return;

    // オブジェクトなので、=は実際の値を格納しているわけではなく、メモリアドレスを指している点に十分注意ください。
    // A -> B -> C を、C -> B -> Aに変更する場合は、向きに少し混乱するのでゆっくり解読しましょう。
    let reverse = this.head;
    this.head = this.head.next;
    reverse.next = null;

    while (this.head !== null) {
      // =はメモリアドレスを指します。紙に書いてロジックを確認しましょう。
      let temp = this.head;
      this.head = this.head.next;
      temp.next = reverse;
      reverse = temp;
    }
    // 処理が終わったら、headのnextを反転したリストを含むtempHeadに割り当てましょう。
    this.head = reverse;
  }
}

let numList = new SinglyLinkedList([35, 23, 546, 67]);
numList.reverse();
numList.printList();
```

- 片方向リストのノードの削除

```javascript
class SinglyLinkedListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

function removeNthNode(head, n) {
  // 関数を完成させてください
  let newNode = new SinglyLinkedList(n);
}
//末尾から n 番目のノードを削除、連結し、新しい連結リストの根ノードを返す
//removeNthNode(singlyLinkedList([0,1,2,3,4]),2) --> 0➡1➡2➡4➡END
```

## doubly linked list

- 片方向に比べて、どっちからもたどれる。しかし、2 倍の pointer を使用するため、、多くのメモリを必要とする。

```javascript
class DoublyLinkedList {
  constructor(arr) {
    //そもそもarr何も無かったら。
    if (arr.length <= 0) {
      this.head = new Node(null);
      this.tail = this.head;
      return;
    }
    //else
    this.head = new Node(arr[0]);
    let currentNode = this.head;
    for (let i = 1; i < arr.length; i++) {
      //next
      currentNode.next = new Node(arr[i]);
      //prev
      currentNode.next.prev = currentNode;
      currentNode = currentNode.next;
    }

    // このcurrent Nodeは最後のnodeです。
    this.tail = currentNode;
  }
}

let numList = new DoublyLinkedList([
  35, 23, 546, 67, 86, 234, 56, 767, 34, 1, 98, 78, 555,
]);
numList.printList();
console.log(numList.tail.data); //555
console.log(numList.tail.prev.data); //78
```

- DoublyLinkedList クラス　わからん。(エラー:TypeError: Cannot read property 'data' of null)

```javascript
class Node {
  // 前後を追跡します。
  constructor(data) {
    this.data = data;
    this.prev = null;
    this.next = null;
  }
}
class DoublyLinkedList {
  constructor(arr) {
    if (arr.length <= 0) {
      this.head = new Node(null);
      this.tail = this.head; //今回のheadは末尾
      return;
    }

    this.head = new Node(arr[0]);
    let currentNode = this.head;
    for (let i = 1; i < arr.length; i++) {
      //next→
      currentNode.next = new Node(arr[i]);
      //prev→
      //    ←
      currentNode.next.prev;

      currentNode = currentNode.next;
    }
    //tailはそもそもどこで定義されているのか。
    this.tail = currentNode;
  }
}
let numList = new DoublyLinkedList([1, 2, 3, 4, 5, 6, 7]);
console.log(numList.head.data);
console.log(numList.head.next.data);
console.log(numList.head.next.prev.data);
console.log(numList.tail.data);
console.log(numList.tail.prev.data);
console.log(numList.tail.prev.prev.data);
// numList.head.data --> 1
// numList.head.next.data --> 2
// numList.head.next.prev.data --> 1
// numList.tail.data --> 7
// numList.tail.prev.data --> 6
// numList.tail.prev.prev.data --> 5
```

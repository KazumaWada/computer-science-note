# doubly linked list

- singly linked list の機能 + 逆方向へも行ける。一つの node に pointer を 2 つ持っている。
- コードで表すとこう ↓

```javascript
class Node {
  // 前後を追跡します。
  constructor(data) {
    this.data = data;
    this.prev = null;
    this.next = null;
  }
}

// リストは少なくとも1つのノードを持っている必要があります。
// ヌルリストをサポートしたい場合は、それに応じてコードを追加してください。
class DoublyLinkedList {
  constructor(arr) {
    //arrが無かったら。
    // 今回は末尾を追跡します。
    if (arr.length <= 0) {
      this.head = new Node(null);
      this.tail = this.head; //今回のheadは末尾
      return;
    }

    //見るコードは基本的にここからだな。
    this.head = new Node(arr[0]);
    let currentNode = this.head;
    for (let i = 1; i < arr.length; i++) {
      currentNode.next = new Node(arr[i]);
      // 次のノードの前のノードをcurrent Nodeに割り当てます。
      currentNode.next.prev = currentNode;
      currentNode = currentNode.next;
    }

    // このcurrent Nodeは最後のnodeです。
    this.tail = currentNode;
  }

  printList() {
    let iterator = this.head;
    let str = "";
    while (iterator != null) {
      str += iterator.data + " ";
      iterator = iterator.next;
    }
    console.log(str);
  }
}

let numList = new DoublyLinkedList([
  35, 23, 546, 67, 86, 234, 56, 767, 34, 1, 98, 78, 555,
]);

numList.printList();

console.log(numList.head.data);
console.log(numList.head.next.data);

console.log(numList.tail.data);
console.log(numList.tail.prev.data); //78
```

## DoublyLinkedList クラス

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
    this.head = new Node(arr[0]);
    let currentNode = this.head;
    for (let i = 1; i < arr.length; i++) {
      //next→
      currentNode.next = new Node(arr[i]);
      //prev→
      //    ←
      currentNode.next.prev = currentNode;
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
```

### index

片方向リストとやり方は同じ。for で index 分辿る。

```javascript
    at(index){
        let iterator = this.head;
        // 片方向リストと同じ処理
        for(let i = 0; i < index; i++){
            iterator = iterator.next;
            if(iterator == null) return null;
        }

        return iterator;
    }
}

let numList = new DoublyLinkedList([35,23,546,67,86,234,56,767,34,1,98,78,555]);

numList.printList();
console.log(numList.at(2).data)
```

### 逆向き reverse

```javascript
    reverse(){
        let reverse = this.tail;
        let iterator = this.tail.prev;

        let currentNode = reverse;//tail
        while(iterator != null){//this.tail.prev != null. 左から右にに進む
            currentNode.next = iterator;//this.tail.next = this.tail.prev;//矢印を逆にしている。

            iterator = iterator.prev;//????
            if(iterator != null) iterator.next = null;

            currentNode.next.prev = currentNode;
            currentNode = currentNode.next;
        }

        this.tail = currentNode;
        this.head = reverse;
        this.head.prev = null;
    }

    //tailをheadにして、prevを辿っていく。
    printInReverse(){
        let iterator = this.tail;
        let str = "";
        while(iterator != null){
            str += iterator.data + " ";
            iterator = iterator.prev;
        }
        console.log(str)
    }
}

let numList = new DoublyLinkedList([35,23,546,67,86]);

numList.printList();//35->23->546->67->86
numList.printInReverse();
numList.printList();//86->67->546->23->35
numList.reverse();
numList.printList();//35->23->546->67->86
numList.printInReverse();//86->67->546->23->35
```

### doubleyLinkedList insert 挿入

- singly linked list とほとんど同じ。違うのは、末尾(tail)にデータが挿入された場合、末尾を更新しなければいけない。

```javascript
    // リストの先頭に追加します。
    preappend(newNode){
        this.head.prev = newNode;
        newNode.next = this.head;
        newNode.prev = null;
        this.head = newNode;
    }

    // リストの最後に追加します。
    append(newNode){
        this.tail.next = newNode;
        newNode.next = null;
        newNode.prev = this.tail;
        // 末尾をアップデート
        this.tail = newNode;
    }

    // 与えられたノードの次に追加します。必要であれば末尾を更新してください。
    // 処理を紙に書いて確認しましょう。オブジェクトなので、=はメモリアドレスを指します。
    addNextNode(node, newNode){
        let tempNode = node.next;
        node.next = newNode;
        newNode.next = tempNode;
        newNode.prev = node;

        // もし与えられたノードが末尾なら、その後ろに新しいノードが追加されるので、末尾をアップデートしてください。
        // それ以外の場合は、tempNodeの前をnewNodeに設定してください。
        if(node === this.tail) this.tail = newNode;
        else tempNode.prev = newNode;
    }

let numList = new DoublyLinkedList([35,23,546,67,86,234,56,767,34,1,98,78,555]);
numList.printList();

// 45をpreappend
numList.preappend(new Node(45));
console.log(numList.head.data);
numList.printList();

// 71をappend
numList.append(new Node(71));
console.log(numList.tail.data);
numList.printList();
console.log("");

// ノードの後に新しいノードを追加
numList.addNextNode(numList.at(3), new Node(4));
numList.printList();
console.log(numList.tail.data);

numList.addNextNode(numList.at(15), new Node(679));
numList.printList();
console.log(numList.tail.data);

numList.printInReverse();
```

### doublyLinkedList 削除 remove

```javascript
    // リストの先頭から要素をpopします。O(1)
    popFront(){
        this.head = this.head.next;
        this.head.prev = null;
    }

    // リストの末尾から要素をpopします。O(1)
    pop(){
        this.tail = this.tail.prev;
        this.tail.next = null;
    }

    preappend(newNode){
        this.head.prev = newNode;
        newNode.next = this.head;
        newNode.prev = null;
        this.head = newNode;
    }

    append(newNode){
        this.tail.next = newNode;
        newNode.next = null;
        newNode.prev = this.tail;
        this.tail = newNode;
    }

    addNextNode(node, newNode){
        let tempNode = node.next;
        node.next = newNode;
        newNode.next = tempNode;
        newNode.prev = node;

        if(node === this.tail) this.tail = newNode;
        else tempNode.prev = newNode;
    }

    // 与えられたノードをO(1)で削除します。
    deleteNode(node){
        if(node === this.tail) return this.pop();
        if(node === this.head) return this.popFront();

        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    reverse(){
        let reverse = this.tail;
        let iterator = this.tail.prev;

        let currentNode = reverse;
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

    printInReverse(){
        let iterator = this.tail;
        let str = "";
        while(iterator != null){
            str += iterator.data + " ";
            iterator = iterator.prev;
        }
        console.log(str)
    }

    printList(){
        let iterator = this.head;
        let str = "";
        while(iterator != null){
            str += iterator.data + " ";
            iterator = iterator.next;
        }
        console.log(str)
    }
}

let numList = new DoublyLinkedList([35,23,546,67,86,234,56,767,34,1,98,78,555]);

// pop
numList.printList();

numList.popFront();
numList.pop();

numList.printList();
numList.printInReverse();

// 要素を削除します
console.log("Deleting in O(1)");
numList.printList();

numList.deleteNode(numList.at(3));
numList.deleteNode(numList.at(9));
numList.deleteNode(numList.at(0));

numList.printList();
numList.printInReverse();
```

---

- doublyLinkedList の index を求めるコードは、linkedList と同じ

- 逆向き表示 pointer が複数必要になる
  pointer = this.tail;
  pointer = pointer.prev;
- reverse と printInreverse の違いは?

```java
class Node{
    constructor(data){
        this.data = data;
        this.prev = null;
        this.next = null;
    }
}

class DoublyLinkedList{
    constructor(arr){
        if(arr.length <= 0){
            this.head =  new Node(null);
            this.tail = this.head;
            return;
        }

        this.head = new Node(arr[0]);
        let currentNode = this.head;
        for(let i = 1; i < arr.length; i++){
            currentNode.next = new Node(arr[i]);
            currentNode.next.prev = currentNode;
            currentNode = currentNode.next;
        }

        this.tail = currentNode;
    }
    ////prev,nextを両方ともセットしながら移動している
    reverse(){
        let reverse = this.tail;
        let iterator = this.tail.prev;

        let currentNode = reverse;
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

    //prevのpointerをtailから通っている
    printInReverse(){
        let iterator = this.tail;
        let str = "";
        while(iterator != null){
            str += iterator.data + " ";
            iterator = iterator.prev;
        }
        console.log(str)
    }
}

let numList = new DoublyLinkedList([35,23,546,67,86,234,56,767,34,1,98,78,555]);

numList.printList();
numList.printInReverse();

numList.printList();
numList.reverse();
numList.printList();
numList.printInReverse();
```

- 挿入: やり方は片方向リストと同じ

```java
class DoublyLinkedList{
    public Node head;
    public Node tail;

    public DoublyLinkedList(int[] arr){
    //省略
    public Node at(int index){
        Node iterator = this.head;
        for(int i=0; i < index; i++){
            iterator = iterator.next;
            if(iterator == null) return null;
        }
        return iterator;
    }

    // 先頭に挿入
    public void preappend(Node newNode){
        this.head.prev = newNode;
        newNode.next = this.head;
        newNode.prev = null;
        //先頭をアップデート
        this.head = newNode;
    }

    // 末尾に挿入
    public void append(Node newNode){
        this.tail.next = newNode;
        newNode.prev = this.tail;
        newNode.next = null;
        // 末尾をアップデート
        this.tail = newNode;
    }

    // 与えられたノードの次に追加します。必要であれば末尾を更新してください。
    // 処理を紙に書いて確認しましょう。オブジェクトなので、=はメモリアドレスを指します。
    public void addNextNode(Node node, Node newNode){
        Node tempNode = node.next;
        node.next = newNode;
        newNode.next = tempNode;
        newNode.prev = node;

        // もし与えられたノードが末尾なら、その後ろに新しいノードが追加されるので、末尾をアップデートしてください。
        // それ以外の場合は、tempNodeの前をnewNodeに設定してください。
        if(node == this.tail) this.tail = newNode;
        else tempNode.prev = newNode;
    }

  class Main{

    public static void main(String[] args){
        int[] arr = new int[]{35,23,546,67,86,234,56,767,34,1,98,78,555};
        DoublyLinkedList numList = new DoublyLinkedList(arr);

        numList.printList();

        // 45をpreappend
        numList.preappend(new Node(45));//先頭に挿入

        // 71をappend
        numList.append(new Node(71));

        // ノードの後に新しいノードを追加
        numList.addNextNode(numList.at(3), new Node(4));
        numList.addNextNode(numList.at(15), new Node(679));
    }
}
```

- 削除

```java
    // 末尾を切り離すO(1)
    public void popFront(){
        this.head = this.head.next;
        this.head.prev = null;
    }

    // 先頭を切り離すO(1)
    public void pop(){
        this.tail = this.tail.prev;
        this.tail.next = null;
    }
    public void deleteNode(Node node){
        //末尾だったらpop
        if(node == this.tail) this.pop();
        //先頭のみだったら先頭pop
        else if(node == this.head) this.popFront();

        else {
            //nodeの左右にあるnext,prev計4本をそれぞれnode.prev,node.nextに当て直している。
            //つまりnodeに付いているpointerは無くなる。
            node.prev.next = node.next;
            node.next.prev = node.prev;
        }
    }
```

- 連結リストの交わる点

```javascript
class SinglyLinkedListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}
// リストの長さを取得する関数
function getLinkedListLength(head) {
  let iterator = head;
  let length = 0;
  while (iterator != null) {
    length++;
    iterator = iterator.next;
  }
  return length;
}

// インデックスのノードを取得する関数
function getNodeAt(head, position) {
  let iterator = head;
  for (let i = 0; i < position; i++) {
    if (iterator === null) return null;
    iterator = iterator.next;
  }
  return iterator;
}

function findMergeNode(headA, headB) {
  // リストの長さをそれぞれ取得します。
  let lA = getLinkedListLength(headA);
  let lB = getLinkedListLength(headB);
  // 二つのリストの長さを比較し、長い方を先頭から切って同じ長さにします。
  headA = lA >= lB ? getNodeAt(headA, lA - lB) : headA;
  headB = lB >= lA ? getNodeAt(headB, lB - lA) : headB;

  let answer = null;

  let iteratorA = headA;
  let iteratorB = headB;
  // 二つのiteratorを走査しつつ同じ値になるノードを探します。
  while (iteratorA != null) {
    if (iteratorA.data !== iteratorB.data) answer = null;
    else if (answer === null) answer = iteratorA.data;

    iteratorA = iteratorA.next;
    iteratorB = iteratorB.next;
  }
  // 三項演算子　answertがnullだったら-1を返します。
  return answer === null ? -1 : answer;
}
```

- 片方向リスト n 倍

```javascript
class SinglyLinkedListNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

function reproduceByN(head, n) {
  // 関数を完成させてください
  let currentNode = head;
  let count = 0;

  while (currentNode != null) {
    count++;
    currentNode = currentNode.next;
  }
  //初期化
  currentNode = head;
  for (let i = 0; i < count * n; i++) {
    if (currentNode.next == null) currentNode.next = head;
    currentNode = currentNode.next;
  }
  return currentNode;
}
```

# stack : push,pop,peek

そもそも stuck は再帰の時にやったように、頂上に stuck して、一番最初に取り出されるものも頂上からだった。

- push: 頂上に挿入
- peek: 頂上にある物を読み取る
- pop: 頂上にある要素を取り出す
  > push、peek、pop の 3 つを見てわかる通り、スタックの頂上の要素だけが自由に読み書きできます。

```java
class Node{
    public int data;
    public Node next;

    public Node(int data){
        this.data = data;
        this.next = null;
    }
}

class Stack {
    public Node head;

    public Stack(){
        this.head = null;
    }

    //dataがheadになる。headがdata.nextになる。
    public void push(int data){
        Node temp = this.head;
        this.head = new Node(data);
        this.head.next = temp;
    }

    //頂点.nextを頂点にする
    public Integer pop(){
        if (this.head == null) return null;
        Node temp = this.head;
        this.head = this.head.next;
        return temp.data;
    }

    //今あるstackの頂点を返す。
    public Integer peek(){
        if (this.head == null) return null;
        return this.head.data;
    }
}

class Main{

    public static void main(String[] args){

        Stack stack = new Stack();
        stack.push(2);//2->head->
        System.out.println(stack.peek());//stackの頂点を返す。

        stack.push(4);
        stack.push(3);
        stack.push(1);
        stack.pop();//先頭のnextを外す
        System.out.println(stack.peek());
    }
}
```

```javascript
// diceStreakGamble([1,2,3],[3,4,2],[4,2,4],[6,16,4])
// --> Winner: Player 1 won $12 by rolling [1,2,3]
function diceStreakGamble(player1, player2, player3, player4) {
  const scores = [
    consecutiveWalk(player1),
    consecutiveWalk(player2),
    consecutiveWalk(player3),
    consecutiveWalk(player4),
  ];

  let maxScore = scores[0].length;
  let index = 0;

  for (let i = 0; i < scores.length; i++) {
    if (maxScore < scores[i].length) {
      maxScore = scores[i].length;
      index = i;
    }
  }

  return `Winner: Player ${index + 1} won $${maxScore * 4} by rolling [${
    scores[index]
  }]`;
}

function consecutiveWalk(arr) {
  stack = [];
  stack.push(arr[0]);

  for (let i = 1; i < arr.length; i++) {
    if (stack[stack.length - 1] > arr[i]) {
      let max = 0;
      while (stack.pop() != undefined);
    }
    stack.push(arr[i]);
  }

  return stack;
}
```

---

# stack

- push: 頂上に挿入
- peek: 頂上にある物を読み取る
- pop: 頂上にある要素を取り出す
  > push、peek、pop の 3 つを見てわかる通り、スタックの頂上の要素だけが自由に読み書きできます。

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class Stack {
  constructor() {
    this.head = null;
  }

  //dataをheadにする。元のheadをdata.nextにする。
  push(data) {
    let temp = this.head;
    this.head = new Node(data);
    this.head.next = temp;
  }

  pop() {
    if (this.head == null) return null;
    let temp = this.head;
    this.head = this.head.next;
    return temp.data;
  }
  //ただheadを表示するだけ。
  peek() {
    if (this.head === null) return null;
    return this.head.data;
  }
}

let s = new Stack();

s.push(2);
console.log(s.peek());

s.push(4);
s.push(3);
s.push(1);
s.pop();
console.log(s.peek());
```

- 配列の逆表示

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class Stack {
  constructor() {
    this.head = null;
  }

  //dataをheadにする。元のheadをdata.nextにする。
  push(data) {
    let temp = this.head;
    this.head = new Node(data);
    this.head.next = temp;
  }

  pop() {
    if (this.head == null) return null;
    let temp = this.head;
    this.head = this.head.next;
    return temp.data;
  }
  //ただheadを表示するだけ。
  peek() {
    if (this.head === null) return null;
    return this.head.data;
  }
}
//全ての要素をスタック内に一度 push し、そこから全てを pop させると、要素を逆の順番で取り出すことができます。
let s = new Stack();
function reverse(arr) {
  // 全てstackに突っ込む
  for (let i = 0; i < arr.length; i++) {
    s.push(arr[i]);
  }
  //全て取り出す=先頭から出るから、自然に逆になる
  let result = [];
  for (let i = 0; i < arr.length; i++) {
    //pop()は先頭を取るだけだから、仮引数なし
    //popしたものをresultに格納
    result.push(s.peek()); //ただhead.dataを見る
    s.pop(); //先頭を取り出す
  }
}
//ここではlistでpushする所で連結リストに変換されている。
//reverse([3,2,1,5,6,4]) --> [4,6,5,1,2,3]
```

## Queue(キュー)

- 先頭、末尾に入れて、その次からは末尾を更新していく。先頭を削除する
  //4,50,64 の順で入れると、
  4
  50
  64
  最初に削除されるのは 4

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class Queue {
  constructor() {
    this.head = null;
    this.tail = null;
  }
  //ただhead.dataを見る
  peekFront() {
    if (this.head == null) return null;
    return this.head.data;
  }
  //ただtail.dataを見る
  peekBack() {
    if (this.tail == null) return this.peekFront();
    return this.tail.data;
  }
  //そもそもhead空?->headにdata入れる
  //そもそもtail空?->tailにdata入れる
  //末尾tailに入れる
  enqueue(data) {
    if (this.head == null) {
      this.head = new Node(data);
    } else if (this.tail == null) {
      this.tail = new Node(data);
      this.head.next = this.tail;
    } else {
      this.tail.next = new Node(data);
      this.tail = this.tail.next;
    }
  }

  //先頭がなかったら->null返す
  //先頭あって、先頭.nextがなかったら==tailもnullと決定する->null返す
  //先頭を先頭.nextにする->切り取られた先頭を返す
  dequeue() {
    //5->8->2
    if (this.head == null) return null;
    let temp = this.head; //5

    if (this.head.next == null) {
      this.head = null;
      this.tail = null;
    } else this.head = this.head.next;

    return temp.data; //5
  }
}
//4,50,64の順で入れると、
//4
//50
//64
let q = new Queue();
console.log(q.peekFront()); //null
console.log(q.peekBack()); //null

q.enqueue(4);
console.log(q.peekFront()); //4
console.log(q.peekBack()); //4

q.enqueue(50);
console.log(q.peekFront()); //4
console.log(q.peekBack()); //50

q.enqueue(64);
console.log(q.peekFront()); //4
console.log(q.peekBack()); //64

console.log("dequeued :" + q.dequeue()); //dequeued :4
console.log(q.peekFront()); //50
console.log(q.peekBack()); //64
```

## Deque(両端キュー)先頭、末尾どちらからも挿入、削除可能

-

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
    this.prev = null;
  }
}

class Deque {
  constructor() {
    this.head = null;
    this.tail = null;
  }

  //先頭なかったらnullを返す
  //あったらそのdataを返す
  peekFront() {
    if (this.head == null) return null;
    return this.head.data;
  }

  //末尾なかったらnullを返す
  //あったらそのdataを返す
  peekBack() {
    if (this.tail == null) return null;
    return this.tail.data;
  }

  //先頭がなかったら、挿入して、それを先頭と末尾に設定する
  enqueueFront(data) {
    if (this.head == null) {
      this.head = new Node(data);
      this.tail = this.head;
    } else {
      this.head.prev = new Node(data);
      this.head.prev.next = this.head;
      this.head = this.head.prev;
    }
  }

  //先頭がなかったら、挿入して、それを先頭と末尾に設定する
  //末尾に挿入する
  enqueueBack(data) {
    if (this.head == null) {
      this.head = new Node(data);
      this.tail = this.head;
    } else {
      this.tail.next = new Node(data);
      this.tail.next.prev = this.tail;
      this.tail = this.tail.next;
    }
  }

  //先頭.nextが無かったら、先頭を切り取って、先頭.nextを先頭にする.

  dequeueFront() {
    //先頭なかったら→null返す
    if (this.head == null) return null;
    let temp = this.head;
    this.head = this.head.next;
    //切り取った先頭の他にnodeが残っていなかったら->末尾=null
    if (this.head == null) this.tail = null;
    else this.head.prev = null;
    return temp.data;
  }

  dequeueBack() {
    if (this.tail == null) return null;

    let temp = this.tail;
    this.tail = this.tail.prev;
    if (this.tail == null) this.head = null;
    else this.tail.next = null;
    return temp.data;
  }
}
```

- 両端キュー最大値

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
    this.prev = null;
  }
}

class Deque {
  constructor() {
    this.head = null;
    this.tail = null;
  }

  peekFront() {
    if (this.head == null) return null;
    return this.head.data;
  }

  peekBack() {
    if (this.tail == null) return null;
    return this.tail.data;
  }

  enqueueFront(data) {
    if (this.head == null) {
      this.head = new Node(data);
      this.tail = this.head;
    } else {
      this.head.prev = new Node(data);
      this.head.prev.next = this.head;
      this.head = this.head.prev;
    }
  }

  enqueueBack(data) {
    if (this.head == null) {
      this.head = new Node(data);
      this.tail = this.head;
    } else {
      this.tail.next = new Node(data);
      this.tail.next.prev = this.tail;
      this.tail = this.tail.next;
    }
  }

  dequeueFront() {
    if (this.head == null) return null;

    let temp = this.head;
    this.head = this.head.next;
    if (this.head == null) this.tail = null;
    else this.head.prev = null;
    return temp.data;
  }

  dequeueBack() {
    if (this.tail == null) return null;

    let temp = this.tail;
    this.tail = this.tail.prev;
    if (this.tail == null) this.head = null;
    else this.tail.next = null;
    return temp.data;
  }
}
function getMax(arr) {
  let deque = new Deque();
  //先頭に仮のmaxを設定
  deque.enqueueFront(arr[0]);
  for (let i = 0; i < arr.length; i++) {
    if (peekFront() < arr[i]) dequeue.enqueueFront(arr[i]);
    else deque.enqueueBack(arr[i]);
  }
  return deque.peekFront();
}
```

- 賭けサイコロゲーム

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class Stack {
  constructor() {
    this.head = null;
  }

  //dataをheadにする。元のheadをdata.nextにする。
  push(data) {
    let temp = this.head;
    this.head = new Node(data);
    this.head.next = temp;
  }

  pop() {
    if (this.head == null) return null;
    let temp = this.head;
    this.head = this.head.next;
    return temp.data;
  }
  //ただheadを表示するだけ。
  peek() {
    if (this.head === null) return null;
    return this.head.data;
  }
}

function diceStreakGamble(player1, player2, player3, player4) {
  // 関数を完成させてください
  let players = [player1, player2, player3, player4];
  let countArray = [];
  for (let i = 0; i < players.length; i++) {
    //Stack,countの更新
    let count = 0;
    let c = new Stack();
    for (let j = 0; j < players[i].length; j++) {
      if (j == 0) c.push(players[i][j]);
      if (c.peek() < players[i][j]) {
        c.push(players[i][j]);
        count = count + 4;
      }
      //前のnodeだから、小さくてもpushはする
      else {
        c.push(players[i][j]);
        count = 0;
      }
    }
    countArray.push(count);
  }

  //最大値のインデックスを求める
  let max = countArray[0];
  let maxIndex = 0;
  for (let i = 1; i < countArray.length; i++) {
    if (max < countArray[i]) {
      max = countArray[i];
      maxIndex = i;
    }
  }

  // [1,2,3],[3,4,2],[4,2,4],[6,16,4]
  //countArray8ではなく12
  return (
    "Winner: Player " +
    max +
    "won $" +
    countArray[max] +
    "by rolling " +
    players[max]
  );
}
```

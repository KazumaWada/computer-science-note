# Top interviewed questions

## Array

- 26. Remove Duplicates from Sorted Array

```javascript
//なぜかarray関数を定義してそこにpushしようと思ったら、numsでしかreturnできないようになっていた。
var removeDuplicates = function (nums) {
  for (i = 0; i < nums.length; i++) {
    if (nums[i] === nums[i + 1]) {
      //splice(配列のi番目から, 削除する個数)//配列のi番目から1個削除する。
      nums.splice(i, 1);
      i--;
    }
  }
  return nums.length;
};
```

- plus one

```javascript
/**
 * @param {number[]} digits
 * @return {number[]}
 */
var plusOne = function (digits) {
  // first join the digits with no space and add 1
  let str = digits.join("");
  let num = BigInt(str) + BigInt(1);
  // convert the num into array
  num = num
    .toString()
    .split("")
    .map((num) => parseInt(num));
  return num;
};

//[9,9]
//[9,1,0]

//supposed to be [1,0,0] bc 99+1
//[6,1,4,5,3,9,0,1,9,5,1,8,6,7,0,5,5,4,3]
```

- Single Number

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
//every element appears twice except for one

var singleNumber = function(nums) {
    let hashmap = {};
for(let i=0; i<nums.length; i++){
if(hashmap[i]=== undefined)hashmap[i]=1;
else hashmap[i]++;
}

console.log(Object.values(hashmap).length);
//forで1だったら、その時点でreturn
//2個あったら減ってるから、hashmapのlength
for(hashmap.length){

if(hashmap[i] == 1)return Objecthashmap
};
// Input: nums = [2,2,1]
// Output: 1
```

- rotate array(計算量多くてタイムアウト)

```javascript
///////time limit exceeded/////////////
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var rotate = function (nums, k) {
  for (let i = 0; i < k; i++) {
    //arrayの先頭に最後を挿入
    nums.unshift(nums[nums.length - 1]);
    //末尾を削除
    nums.splice(nums.length - 1, 1);
  }
  return nums;
};
// Input: nums = [1,2,3,4,5,6,7], k = 3
// Output: [5,6,7,1,2,3,4]
///////time limit exceeded/////////////
var rotate = function (nums, k) {
  let temp = [...nums]; //copied the original array in temp

  for (let i = 0; i < temp.length; i++) {
    //loop through each element index.
    let p = (i + k) % temp.length; //for each element index, find the index position where you want to move that element. e.g if your current position is i=1, then you want to move nums[i] to i+k position. and if suppose for first example, if your i= 5, then i+k = 5+3 = 8. now 8 becomes out of range index. so 8 % nums.length = 8 % 7 = 1. means that whent i=5, then nums[5] will move to 1 index position or nums[1] = temp[5]
    nums[p] = temp[i]; // pasting elemnt in the original numss from the copied temp array
  }
};
// Input: nums = [1,2,3,4,5,6,7], k = 3
// Output: [5,6,7,1,2,3,4]
```

- Delete Node in a Linked List

```javascript
//  削除したい前の Node まで行き、pointer を一つ前の Node に当てる.
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} node
 * @return {void} Do not return anything, modify node in-place instead.
 */
var deleteNode = function(node) {
    let currentNode = node;
    while(currentNode != null){
        if()
        currentNode = currentNode.next;
    }
};
```

- Reverse Linked List

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function (head) {};

// Input: head = [1,2,3,4,5]
// Output: [5,4,3,2,1]
```

- First Unique Character in a String

```javascript
//first non-repeating character: 最初の被らないcharacter
/**
 * @param {string} s
 * @return {number}
 */
var firstUniqChar = function (s) {
  let hashmap = {};
  let indexhash = {};
  //hashmap作る
  for (let i = 0; i < s.length; i++) {
    indexhash[s[i]] = i;
    if (hashmap[s[i]] == undefined) hashmap[s[i]] = 1;
    else hashmap[s[i]]++;
  }
  //最初に1があるsの配列のindexを返す
  for (let i = 0; i < Object.values(hashmap).length; i++) {
    if (Object.values(hashmap)[i] == 1)
      return indexhash[Object.keys(hashmap)[i]];
  }
  return -1;
};
```

- Maximum Depth of Binary Tree

```javascript
   //前順にヒントがあるかも??
    if(root === undefined || root===null){
        return 0;
    }
    return Math.max(maxDepth(root.left),maxDepth(root.right)) + 1;
};
```

- Pascal's Triangle

```javascript
/**
 * @param {number} numRows
 * @return {number[][]}
 */
var generate = function (numRows) {
  var pascal = [];
  for (var i = 0; i < numRows; i++) {
    pascal[i] = [];
    pascal[i][0] = 1;
    for (var j = 1; j < i; j++) {
      pascal[i][j] = pascal[i - 1][j - 1] + pascal[i - 1][j];
    }
    pascal[i][i] = 1;
  }
  return pascal;
};
```

```javascript
//何だただBSTにすればいいのか。そもそもhight-balanced BSTもBSTと同じようにこうやって書けばいいって事なのか??
var sortedArrayToBST = function (nums) {
  //recursionでやった(nums,starts,end)???
  return ConvToBST(nums, 0, nums.length - 1);
};

var ConvToBST = function (nums, start, end) {
  if (start > end) return null;
  //最初の根ノード設定&new TreeNodeする
  var mid = Math.ceil((start + end) / 2);
  var root = new TreeNode(nums[mid]);
  //BST作る
  root.left = ConvToBST(nums, start, mid - 1);
  //BST作る
  root.right = ConvToBST(nums, mid + 1, end);
  return root;
};
```

- Binary Tree Inorder Traversal

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

- Binary Tree Inorder Traversal

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[]}
 */
//間順 左根右
var inorderTraversal = function (root) {
  const result = [];

  function helper(node) {
    if (node != null) {
      //左
      helper(node.left);
      //根
      result.push(node.val);
      //右
      helper(node.right);
    }
  }
  helper(root);
  return result;
};
```

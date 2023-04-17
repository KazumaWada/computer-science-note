# software test

- error: コード中のミス
- bag: 開発者が気づいたエラー
- 人間は必ずミスが出る。それを防ぐために、体型的な戦略がある。

- 構文エラー: プログラミング言語がコードを実行する前に、出してくれるテストエラー。このおかげで機械語に適切に翻訳される
- 論理エラー: 開発者が望まない出力結果が出た場合.エラーを一切出さずに誤った結果が出た場合。(税金の計算の仕方を間違えたなど。)

```javascript
class Student{
    constructor(studentId, grade, name, age, height){
        this.studentId = studentId;
        this.grade = grade;
        this.name = name;
        this.age = age;
        this.height = height;
    }
}

let studentList1 = [
    new Student(1000,9,"Matt Verdict", 14, 5.5),
    new Student(1001,9,"Amy Lam", 14, 5.5),
    new Student(1002,10,"Bryant Gonzales", 15, 5.9),
    new Student(1003,9,"Kimberly York", 15, 5.3),
    new Student(1004,11,"Christine Bryant", 15, 5.8),
    new Student(1005,10,"Mike Allen", 16, 6.2),
];

//  学生のリストが与えられるので、最年少で最も身長の高い学生を返す、関数を作成してください。もし、複数該当する場合は若いIDを持つ学生を優先してください。
function chooseStudent(studentList){
    //  関数を実装してください。
    if(studentList[i])
    //console.log(studentList1[0].grade);//9
}

console.log(chooseStudent(studentList1).studentId == 1000);
console.log(chooseStudent(studentList1).studentId == 1001);
//  テストケースを以下作成してください。
```

### white box test

- black box になっている全ての内部コードをテストする。

```javascript
class Student {
  constructor(studentId, grade, name, age, height) {
    this.studentId = studentId;
    this.grade = grade;
    this.name = name;
    this.age = age;
    this.height = height;
  }

  toString() {
    return `ID: ${this.studentId}...${this.name}, grade:${this.grade}, age ${this.age}, height ${this.height}`;
  }
}

let studentList1 = [
  new Student(1000, 9, "Matt Verdict", 14, 5.5),
  new Student(1001, 9, "Amy Lam", 14, 5.5),
  new Student(1002, 10, "Bryant Gonzales", 15, 5.9),
  new Student(1003, 9, "Kimberly York", 15, 5.3),
  new Student(1004, 11, "Christine Bryant", 15, 5.8),
  new Student(1005, 10, "Mike Allen", 16, 6.2),
];
// 最年少かつ最も高い生徒をid順に並べると、[1000, 1001, 1002, 1004, 1003, 1005]

let studentList2 = [
  new Student(1000, 9, "Matt Verdict", 14, 5.5),
  new Student(1001, 9, "Amy Lam", 13, 5.5), // 変更され、13歳
  new Student(1002, 10, "Bryant Gonzales", 15, 5.9),
  new Student(1003, 9, "Kimberly York", 15, 5.3),
  new Student(1004, 11, "Christine Bryant", 15, 5.8),
  new Student(1005, 10, "Mike Allen", 16, 6.2),
];
// 最年少かつ最も高い生徒をid順に並べると、[1001, 1000, 1002, 1004, 1003, 1005]

function printStudents(students) {
  console.log("----Total students: " + students.length + "----");
  for (let student of students) console.log(student);
  console.log("---END---");
}

// 最年少かつ最も高い生徒をk人返します。kはオプションでデフォルトは1になります。
function chooseStudent(studentList, k = 1) {
  // ラムダの比較
  // s1がs2より若く、背が高いかどうかを返します。もし、同じならs1とs2のIDを比較します。
  let studentCompare = (s1, s2) => {
    if (s1.age == s2.age) {
      return s1.height === s2.height
        ? s1.studentId < s2.studentId
        : s1.height > s2.height;
    }
    return s1.age < s2.age;
  };

  // studentListをheapifyし、最初のk個の要素をpopします。
  let heapify = (l) => {
    for (let index = l.length / 2; index >= 0; index--) {
      minHeap(l, index);
    }
  };

  let swap = (arr, i, j) => {
    temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
  };

  let minHeap = (l, index) => {
    let lengthL = l.length;
    let curr = index;
    let flag = true;
    while (flag) {
      let left = curr * 2 + 1;
      let right = curr * 2 + 2;
      let smallest = curr;

      if (lengthL > left && !studentCompare(l[smallest], l[left]))
        smallest = left;
      if (lengthL > right && !studentCompare(l[smallest], l[right]))
        smallest = right;

      if (smallest === curr) flag = false;
      else swap(studentList, curr, smallest);

      curr = smallest;
    }
  };

  // Heapify studentList
  heapify(studentList);
  let results = [];
  for (let i = 0; i < k; i++) {
    // minを最後のノードとswapし、削除します。O(1)
    swap(studentList, 0, studentList.length - 1);
    results.push(studentList.pop());

    if (studentList.length > 0) minHeap(studentList, 0);
    else break;
  }
  return results;
}

// リスト1に対してテストを実行します
printStudents(studentList1);
// ブラックボックステスト
console.log(chooseStudent(studentList1)[0].studentId == 1000);
// 副作用。popにより、リストから一人が減り、idでソートされていた配列もheapifyされてバラバラになりました。
// 関数内のin-placeアルゴリズムによって、入力の配列に影響を与えました。
printStudents(studentList1);

// リスト2
printStudents(studentList2);
// ブラックボックステスト
console.log(chooseStudent(studentList2)[0].studentId == 1001);
// 副作用。popにより、リストから一人が減り、idでソートされていた配列もheapifyされてバラバラになりました。
// 関数内のin-placeアルゴリズムによって、入力の配列に影響を与えました。
printStudents(studentList2);

let studentList3 = [
  new Student(1000, 9, "Matt Verdict", 11, 5.5), // 変更、11歳
  new Student(1001, 9, "Amy Lam", 13, 5.5),
  new Student(1002, 10, "Bryant Gonzales", 13, 5.5), // 変更、13歳
  new Student(1003, 9, "Kimberly York", 15, 5.3),
  new Student(1004, 11, "Christine Bryant", 15, 5.3), // 変更、5.3高さ
  new Student(1005, 10, "Mike Allen", 16, 6.2),
];

// 最年少かつ最も高い生徒をid順に並べると、[1000, 1001, 1002, 1003, 1004, 1005]

printStudents(studentList3);
// リスト3から4人を出力します。
printStudents(chooseStudent(studentList3, 4));
// 副作用。Christine BryantとMike Allenしか残っていません。
// 関数内のin-placeアルゴリズムによって、入力の配列に影響を与えました。
printStudents(studentList3);
```

-

```javascript
class Student {
  constructor(studentId, grade, name, age, height) {
    this.studentId = studentId;
    this.grade = grade;
    this.name = name;
    this.age = age;
    this.height = height;
  }

  toString() {
    return `ID: ${this.studentId}...${this.name}, grade:${this.grade}, age ${this.age}, height ${this.height}`;
  }
}

let studentList1 = [
  new Student(1000, 9, "Matt Verdict", 14, 5.5),
  new Student(1001, 9, "Amy Lam", 14, 5.5),
  new Student(1002, 10, "Bryant Gonzales", 15, 5.9),
  new Student(1003, 9, "Kimberly York", 15, 5.3),
  new Student(1004, 11, "Christine Bryant", 15, 5.8),
  new Student(1005, 10, "Mike Allen", 16, 6.2),
];
// 最年少かつ最も高い生徒: [1000, 1001, 1002, 1004, 1003,1005]

let studentList2 = [
  new Student(1000, 9, "Matt Verdict", 14, 5.5),
  new Student(1001, 9, "Amy Lam", 13, 5.5), // 変更され、13歳
  new Student(1002, 10, "Bryant Gonzales", 15, 5.9),
  new Student(1003, 9, "Kimberly York", 15, 5.3),
  new Student(1004, 11, "Christine Bryant", 15, 5.8),
  new Student(1005, 10, "Mike Allen", 16, 6.2),
];
// 最年少かつ最も高い生徒: [1001, 1000, 1002, 1004, 1003,1005]

function printStudents(students) {
  console.log("----Total students: " + students.length + "----");
  for (let student of students) console.log(student);
  console.log("---END---");
}

// 2 つの学生リストが等しいかどうかを返す述語関数
function areStudentListsEquals(studentList1, studentList2) {
  if (studentList1.length != studentList2.length) return false;
  for (let i = 0; i < studentList1.length; i++) {
    if (
      studentList1[i].studentId != studentList2[i].studentId ||
      studentList1[i].grade != studentList2[i].grade ||
      studentList1[i].name != studentList2[i].name ||
      studentList1[i].age != studentList2[i].age ||
      studentList1[i].height != studentList2[i].height
    )
      return false;
  }
  return true;
}

// 最年少かつ最も高い生徒をk人返します。kはオプションでデフォルトは1になります。
function chooseStudent(studentListMain, k = 1) {
  studentList = [...studentListMain];
  let studentCompare = (s1, s2) => {
    if (s1.age == s2.age) {
      return s1.height === s2.height
        ? s1.studentId < s2.studentId
        : s1.height > s2.height;
    }
    return s1.age < s2.age;
  };

  // studentListをheapifyし、最初のk個の要素をpopします。
  let heapify = (l) => {
    for (let index = l.length / 2; index >= 0; index--) {
      minHeap(l, index);
    }
  };

  let swap = (arr, i, j) => {
    temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
  };

  let minHeap = (l, index) => {
    let lengthL = l.length;
    let curr = index;
    let flag = true;
    while (flag) {
      let left = curr * 2 + 1;
      let right = curr * 2 + 2;
      let smallest = curr;

      if (lengthL > left && !studentCompare(l[smallest], l[left]))
        smallest = left;
      if (lengthL > right && !studentCompare(l[smallest], l[right]))
        smallest = right;

      if (smallest === curr) flag = false;
      else swap(studentList, curr, smallest);

      curr = smallest;
    }
  };

  // Heapify studentList
  heapify(studentList);
  let results = [];
  for (let i = 0; i < k; i++) {
    // minを最後のノードとswapし、削除します。O(1)
    swap(studentList, 0, studentList.length - 1);
    results.push(studentList.pop());

    if (studentList.length > 0) minHeap(studentList, 0);
    else break;
  }
  return results;
}

// studentListをコピーします。
copyStudentList1 = [...studentList1];
console.log(chooseStudent(studentList1, 6)[0].studentId == 1000);
console.log(areStudentListsEquals(studentList1, copyStudentList1));

copyStudentList2 = [...studentList2];
console.log(chooseStudent(studentList2)[0].studentId == 1001);
console.log(areStudentListsEquals(studentList2, copyStudentList2));

let studentList3 = [
  new Student(1000, 9, "Matt Verdict", 11, 5.5), // 変更、11歳
  new Student(1001, 9, "Amy Lam", 13, 5.5),
  new Student(1002, 10, "Bryant Gonzales", 13, 5.5), // 変更、13歳
  new Student(1003, 9, "Kimberly York", 15, 5.3),
  new Student(1004, 11, "Christine Bryant", 15, 5.3), // 変更、5.3高さ
  new Student(1005, 10, "Mike Allen", 16, 6.2),
];
// 最年少かつ最も高い生徒: [1000, 1001, 1002, 1003, 1004, 1005]

printStudents(studentList3);
printStudents(chooseStudent(studentList3, 4));
printStudents(studentList3);
```

### assertion

- 開発者がコードの途中で true,false を使って、コードを意図的に止める行為
  こういう書き方をする

```javascript
// ここから開発してください。
function formatDecimal(num) {
  // Math.round関数は、小数点第一位を四捨五入します。
  let result = Math.round(num * 100);

  console.log("rounding " + num + "...." + result);
  return result;
}

console.assert(formatDecimal(86.258) == 8626);
console.assert(formatDecimal(86.253) == 8625);

// 四捨五入が目標です
// JavaScriptのround関数は四捨五入を行うので、assert関数はエラーを吐きません
console.assert(formatDecimal(20.355) == 2036);
console.assert(formatDecimal(20.345) == 2035);
console.assert(formatDecimal(54.075) == 5408);
console.assert(formatDecimal(54.065) == 5407);
console.assert(formatDecimal(54.775) == 5478);

// error
console.assert(formatDecimal(54.775) == 5477); //Assertion failed
```

- 関数の中にも書く事ができる

```javascript
function equalAssertion(a, b) {
  let equality = a == b;
  console.log(
    `Comparing ${a} and ${b}...` +
      (equality ? "They are equal." : "Error, they are NOT equal.")
  );
  // 等しくない場合はクラッシュします。
  console.assert(equality);
  return true;
}
function formatDecimal(num) {
  // 四捨五入します。
  let result = Math.round(num * 100);
  return result;
}

equalAssertion(formatDecimal(86.258), 8626);
equalAssertion(formatDecimal(86.253), 8625);
equalAssertion(formatDecimal(20.445), 2045);
equalAssertion(formatDecimal(45.465), 4547);
equalAssertion(formatDecimal(45.555), 4556);
// equalAssertion(formatDecimal(31.135), 0); // Error
equalAssertion(formatDecimal(30.125), 3013);
```

### イコールだけでテストを判断できない場合

上記の場合+文字列配列はどのようにテストをすればいいのか。
↓
//文字列で構成される配列同士を比較する場合を考えてみましょう。
//- 2 つの配列の要素とその順序が等しい
//- 2 つの配列の要素のみが一致している ← 今回はこれ。
// //のケースが考えられます。
// //それでは、
// 比較する 2 つの要素
// データを比較するコールバック関数
// を入力としてテストを作成してみましょう。これによって、ラムダ関数を作成したり、関数の参照を渡してテストを行うことができます。

// 今回の例では、

// 数値によって構成される複数の配列
// 順番を考慮するラムダ orderedArrayEquality
// 順序は考慮しないラムダ unorderedArrayEquality
// を用いて、配列の比較を行っていきます。equalAssertion(a,b, lambda) の 入力 lambda を変更することによって、1 つのテスト関数で異なるタイプの比較を行うことができます。

```javascript
// オプションとしてのコールバック。コールバックは、aがbと等しいかチェックします。
function equalAssertion(a, b, callback = null) {
  //ここで、orderedArrayEquality(a,b)を呼んでいる。
  let equality = callback == null ? a == b : callback(a, b);
  console.log(
    `Comparing ${a} and ${b}...` +
      (equality ? "They are equal." : "Error, they are NOT equal.")
  );
  // 等しくない場合はクラッシュします。
  console.assert(equality);
  return true;
}

let arr1 = [3, 4, 5, 10, 2, 8, 12];
let arr2 = [4, 5, 3, 12, 10, 8, 2];
let arr3 = [4, 5, 3, 12, 10, 8, 2];

let orderedArrayEquality = (a, b) => {
  if (a.length != b.length) return false;
  for (let i = 0; i < a.length; i++) {
    if (a[i] != b[i]) return false;
  }
  return true; //なぜtrueが出力されないのか。
};

let unorderedArrayEquality = (a, b) => {
  if (a.length != b.length) return false;
  let aHash = {};
  let bHash = {};

  // カウントアルゴリズムを適用します。
  //a,b配列内で同じ要素があったらカウント
  for (let i = 0; i < a.length; i++) {
    if (aHash[a[i]] != undefined) aHash[a[i]] += 1;
    else aHash[a[i]] = 1;

    if (bHash[b[i]] != undefined) bHash[b[i]] += 1;
    else bHash[b[i]] = 1;
  }
  //for( 変数 in オブジェクト ) //オブジェクトの中身を取り出したいときに使う。
  //aHashから取得したデータがkeyに一つづつ代入されていく。
  //aHashから取得するデータがなくなったら、自動的に処理が終了する。
  //例
  // let objPC = {
  //   CPU:'Core i9',
  //   Memory:'16GB',
  //   Storage:'256GB'
  // }

  // for (let item in objPC){
  //   console.log(item);
  // }
  // CPU
  // Memory
  // Storage
  for (let key in aHash) {
    //[3,4,5,10,2~]
    //そもそもbにaの要素がなかったら。
    if (bHash[key] == undefined) return false;
    //配列全体のa[i]の要素の個数 !=
    if (aHash[key] != bHash[key]) return false;
  }
  return true;
};

// 順不同のチェック。pass
//arrを渡す+callbackでarrを関数に渡す。
equalAssertion(arr1, arr2, unorderedArrayEquality);

// 順序を考慮したチェック。pass
equalAssertion(arr2, arr3, orderedArrayEquality);

// 順序を考慮したチェック。fail
// equalAssertion(arr1, arr2, orderedArrayEquality)

//Comparing 3,4,5,10,2,8,12 and 4,5,3,12,10,8,2...They are equal.
//Comparing 4,5,3,12,10,8,2 and 4,5,3,12,10,8,2...They are equal.
```

```javascript
function equalAssertion(a, b, callback = null) {
  let equality = callback == null ? a == b : callback(a, b);
  //callback関数がtrueか?
  console.log(
    `Comparing ${a} and ${b}...` +
      (equality ? "They are equal." : "Error, they are NOT equal.")
  );
  // 等しくない場合はクラッシュします。
  console.assert(equality);
  return true;
}

// 重複していないメールのみを全て返す関数。
let createSetList = (arr) => {
  arrUnique = Array.from(new Set(arr));
  return arrUnique;
};

let unorderedArrayEquality = (a, b) => {
  aHash = {};
  bHash = {};

  //copyArr
  for (let i = 0; i < a.length; i++) {
    if (aHash[a[i]] != undefined) aHash[a[i]] += 1;
    else aHash[a[i]] = 1; //++ではなく1にする。
  } //{"aaa@bbb.com":1, "bbb@ccc.com":1, "ccc@ddd.com":1};

  //setArr
  for (let i = 0; i < b.length; i++) {
    if (bHash[b[i]] != undefined) bHash[b[i]] += 1;
    else bHash[b[i]] = 1;
  } ////{"aaa@bbb.com":1, "bbb@ccc.com":1, "ccc@ddd.com":1};

  for (let key in aHash) {
    if (bHash[key] == undefined) return false;
  }
  return true;
};

let emailArr = [
  "aaa@bbb.com",
  "bbb@ccc.com",
  "ccc@ddd.com",
  "aaa@bbb.com",
  "ccc@bbb.com",
];
//emailArr//setArrで状態が変わる前に、標準のemailArrをコピーしておく。
let copyArr = [...emailArr];
//emailArrの重複していないemailのみを返す
let setArr = createSetList(emailArr); //["aaa@bbb.com", "bbb@ccc.com", "ccc@ddd.com"];
equalAssertion(copyArr, setArr, unorderedArrayEquality);

//copyArrとsetArrが同じ???->同じだった。hashmapで++せずに1にし続けるから。。
//↓
//結局何のテストをしたの??
//↓
//setArrが予想外の動きをしていないかチェックしたのでは?->yes
//詳しくはこれ↓
/////////////////////////////////////////////////////////////////////////////////
//文字列で構成される配列同士を比較する場合を考えてみましょう。
//- 2 つの配列の要素とその順序が等しい
//- 2 つの配列の要素のみが一致している←今回はこれ。
// //のケースが考えられます。
// //それでは、
// 比較する 2 つの要素
// データを比較するコールバック関数
// を入力としてテストを作成してみましょう。これによって、ラムダ関数を作成したり、関数の参照を渡してテストを行うことができます。

// 今回の例では、

// 数値によって構成される複数の配列
// 順番を考慮するラムダ orderedArrayEquality
// 順序は考慮しないラムダ unorderedArrayEquality
// を用いて、配列の比較を行っていきます。equalAssertion(a,b, lambda) の 入力 lambda を変更することによって、1 つのテスト関数で異なるタイプの比較を行うことができます。
/////////////////////////////////////////////////////////////////////////////////////
//aaa@bbb.com,bbb@ccc.com,ccc@ddd.com,aaa@bbb.com,ccc@bbb.com and aaa@bbb.com,bbb@ccc.com,ccc@ddd.com,ccc@bbb.com...They are equal.
```

```javascript
// 関数 : higestDonation
// 寄付のリストを配列として受け取り、最も高額な寄付金を返す関数。同じ価格の寄付金が複数ある場合は、いずれかの寄付金を返します。
// それぞれの寄付金には、name、price、donationNumber、購入日の状態が含まれています。正しい寄付金は、寄付金の価格に基づいて決定されます。したがって、a.price と b.price が等しいとき、同じ寄付金とみなされます。
function equalAssertion(a, b, callback = null) {
  let equality = callback == null ? a == b : callback(a, b);
  console.log(
    `Comparing ${a} and ${b}...` +
      (equality ? "They are equal." : "Error, they are NOT equal.")
  );
  // 等しくない場合はクラッシュします。
  console.assert(equality);
  return true;
}

// ここから開発してください。
class Donation {
  constructor(name, price, donationNumber, date) {
    //this: 仮引数のnameのこと
    this.name = name;
    this.price = price;
    this.donationNumber = donationNumber;
    this.date = date;
  }
}

function higestDonation(donatorsArr) {
  let highest = 0;
  for (let i = 0; i < donators.length; i++) {
    if (highest < donatorsArr[i].price) highest = donatorsArr[i].price;
  }
  return highest;
}

//a: donators
//b: 最も高額なdonatorの額
//どうやって比較するの。。aをそのまま配列を渡すのではなく、priceを渡す。
let unorderedArrayEquality = (a, b) => {
  aHash = {};
  bHash = {};
  //copyArr
  for (let i = 0; i < a.length; i++) {
    if (aHash[a[i]] != undefined) aHash[a[i]] += 1;
    else aHash[a[i]] = 1; //++ではなく1にする。
  } //{"aaa@bbb.com":1, "bbb@ccc.com":1, "ccc@ddd.com":1};
  //setArr
  for (let i = 0; i < b.length; i++) {
    if (bHash[b[i]] != undefined) bHash[b[i]] += 1;
    else bHash[b[i]] = 1;
  } ////{"aaa@bbb.com":1, "bbb@ccc.com":1, "ccc@ddd.com":1};
  for (let key in aHash) {
    if (bHash[key] == undefined) return false;
  }
  return true;
};

let donator01 = new Donation("tyler", 100, 01, "11/1");
let donator02 = new Donation("rocky", 200, 02, "11/2");
let donator03 = new Donation("kanye", 1000, 03, "11/3");

let donators = [donator01, donator02, donator03];
//let copyArr = donators;と思ったけど、price以外使わないし。。
let copyArr = [donator01.price, donator02.price, donator03.price];
let setArr = highestDonation(donators);

//前回は配列a,b全て要素があった。今回は最も高額な要素以外はない。さあどうするか。
//↓
//copyArrの中にsetArr(kanyeの1000が見つかったら即trueを返す。)

console.log(higestDonation(donators));
//テスト関数(定義された関数,定義されて実行された関数, 2つを比較する関数);
console.log(equalAssertion(copyArr, setArr, unorderedArrayEquality));
```

- 上の答え
  ポイント ↓

```javascript
// 寄付金が同じ額か調べる関数
function checkSameDonation(d1, d2) {
  return d1.price == d2.price;
}
//金額をsortして、一番後ろ(高額な値の要素)とhighest変数を比べている。
equalAssertion(sortedList[donationList.length - 1], highest, checkSameDonation);
```

```javascript
class Donation {
  constructor(name, price, donationNumber, day, month, year) {
    this.name = name;
    this.price = price;
    this.donationNumber = donationNumber;
    this.donationDay = new Date(year, month, day);
  }

  toString() {
    return `name: ${this.name}, price:${this.price}, donationNumber ${this.donationNumber}, day ${this.donationDay}`;
  }
}

let donationList = [
  new Donation("Steve Jobs", 50000, 1, 21, 3, 2021),
  new Donation("Bill Gatess", 40000, 2, 2, 9, 2021),
  new Donation("Mark Elliot Zuckerberg", 40000, 3, 29, 4, 2021),
  new Donation("Jeffrey Preston Bezos", 60000, 4, 1, 2, 2021),
  new Donation("Steve Jobs", 10000, 5, 19, 5, 2021),
];

function equalAssertion(a, b, callback = null) {
  let equality = callback == null ? a == b : callback(a, b);
  console.log(
    `Comparing ${a} and ${b}...` +
      (equality ? "They are equal." : "Error, they are NOT equal.")
  );
  // 等しくない場合はクラッシュします。
  console.assert(equality);
  return true;
}

// 一番高い寄付金を決める関数
function highestDonation(arr) {
  let highestPrice = 0;
  let highestIndex = 0;
  for (let i = 0; i < arr.length; i++) {
    if (arr[i].price > highestPrice) {
      highestPrice = arr[i].price;
      highestIndex = i;
    }
  }
  return arr[highestIndex];
}

// 寄付金が同じ額か調べる関数
function checkSameDonation(d1, d2) {
  return d1.price == d2.price;
}

// 寄付金の金額で昇順にソートしたリスト
let copyDonationList = [...donationList];
let sortedList = copyDonationList.sort((a, b) => a.price - b.price);

let highest = highestDonation(donationList);
//
equalAssertion(sortedList[donationList.length - 1], highest, checkSameDonation);
```

### a,b のデータ比較だけでなく、データの範囲/大小等の比較ロジックなどをテストしたい場合。

- 与えられた文字列がメールであるかどうかチェックするテスト
  関数同士を callback

```javascript
function assertionTest(a, callback) {
  let result = callback(a);
  console.log(`Checking against ${a}, is it valid?...${result}`);
  console.assert(result);
  return true;
}
// emailが有効かテストする述語
// 有効なemailとは空白のスペースがなく、@を1つ含み、@の後に.が含まれる文字列を指します。
function isValidEmail(email) {
  if (
    email.indexOf(" ") >= 0 ||
    email.indexOf("@") == -1 ||
    email.substring(email.indexOf("@") + 1).indexOf("@") != -1
  )
    return false;
  if (email.substring(email.indexOf("@") + 1).indexOf(".") == -1) return false;
  return true;
}
assertionTest("johnnyTest@test.com", isValidEmail);
// assertionTest("John Test",isValidEmail) // Error

//Checking against johnnyTest@test.com, is it valid?...true
```

## manual test(↑ 今までのテストの事を指す)

手動テスト 02

```javascript
//getStudentGPA関数では、ある入力によっては関数は予期せぬ出力（エラー）を返すようになっています。
function getStudentGPA(points) {
  let output = "";
  if (points < 0) output = "F";
  if (points >= 0 && points < 50) output = "D";
  if (points >= 50 && points <= 70) output = "C";
  if (points >= 70 && points <= 80) output = "B";
  if (points > 80) output = "A";

  return output;
}
//述語(isValid~みたいな)を受け取り、その結果を出力する汎用テスト関数 unitTestCheck という関数
//predicateはただの仮引数?それともメソッド?
function unitTestCheck(predicate) {
  //unitTestCheck(getStudentGPA(-1))で何でもいいからoutputがreturnされたらtrue.仮引数が何も無かったらelseでerrorって事かな。。
  if (predicate) {
    console.log("The test passed!!");
  } else {
    console.log("ERROR! The test failed!!");
  }
}

function unitTests(a, callback) {
  //assertionTestかな
  let result = callback(a);
  console.log();
}
//通常の出力
console.log(getStudentGPA(-1));
//1 どんな出力をすればいいのか。
//を用意しています。unitTestCheck 関数を上手く活用し、以下の値の整合性をチェックしてください
//2 何をテストするのか
```

- 手動テスト 01

```javascript
function fibonacci(n) {
  let cache = Array(n + 1).fill(0);
  cache[0] = 0;
  cache[1] = 1;

  for (let i = 2; i <= n; i++) {
    cache[i] = cache[i - 1] + cache[i - 2];
  }

  return cache[n];
}
//このpredicate仮引数は、trueだったら実行される。falseだったらされずにelseに行く。
function unitTestCheck(predicate) {
  if (predicate) {
    console.log("The test passed!!");
  } else {
    console.log("ERROR! The test failed!!");
  }
}

const check = (a, b) => a === b;
//何のテスト?
//fibonacci(4), 3がイコールであればok.
//fibonacci(4)が3であればいい。
//どういう意味?

function unitTests() {
  // ここから書いてください
  console.assert(unitTestCheck(true)); //"The test passed!!"
  console.assert(unitTestCheck(false)); //"ERROR! The test failed!!"
  console.assert(unitTestCheck(check(fibonacci(4), 3)));
  console.assert(unitTestCheck(check(fibonacci(6), 8)));
  console.assert(unitTestCheck(check(fibonacci(9), 34)));
  console.assert(unitTestCheck(check(fibonacci(10), 55)));
}
```

- n 番目のフィボナッチを返す

```javascript
// n 番目のフィボナッチ数を返します。
function fibonacciNumber(n) {
  // ベースケース
  if (n == 0) {
    return 0;
  } else if (n == 1) {
    return 1;
  }

  return fibonacciNumber(n - 1) + fibonacciNumber(n - 2);
}

console.log(fibonacciNumber(3));
console.log(fibonacciNumber(5));
console.log(fibonacciNumber(8));
console.log(fibonacciNumber(10));
```

- アサーション 01

```javascript
function assertPredicate(predicate) {
  if (predicate) {
    console.log("Passed the assertion test.");
  } else {
    console.assert(predicate);
    throw new Error("Assertion Failed.");
  }
}

function cube(x) {
  // assertPredicate関数を使って、意図した値を返すかチェックし、修正しましょう
  return x * x * x;
}

function assertionTests() {
  // テストケースを記述しましょう
  console.assert(assertPredicate(cube(2) == 8));
  console.assert(assertPredicate(cube(3) == 27));
  console.assert(assertPredicate(cube(4) == 64));
  console.assert(assertPredicate(cube(0) == 0));
  console.assert(assertPredicate(cube(-2) == -8));
  console.assert(assertPredicate(cube(-3) == -27));
}
```

- アサーション 02

```javascript
function assertPredicate(predicate) {
  if (predicate) {
    console.log("Passed the assertion test.");
  } else {
    console.assert(predicate);
    throw new Error("Assertion Failed.");
  }
}
function applyDiscount(originalPrice, discount) {
  // assertPredicate関数を使って、意図した値を返すかチェックし、修正しましょう
  let discountedPrice = originalPrice * (1.0 - discount);

  return discountedPrice;
}

function assertionTests() {
  // テストケースを記述しましょう
  console.assert(assertPredicate(applyDiscount(30, 0.5) == 15));
  console.assert(assertPredicate(applyDiscount(50, 0.2) == 40));
  console.assert(assertPredicate(applyDiscount(70, 1.0) == 0));
  console.assert(assertPredicate(applyDiscount(70, 0.0) == 70));
  console.assert(assertPredicate(applyDiscount(90, 2.0) == -90));
  console.assert(assertPredicate(applyDiscount(30, -3.0) == 120));
}
```

- アサーション 03

```javascript
function assertPredicate(predicate) {
  if (predicate) {
    console.log("Passed the assertion test.");
  } else {
    console.assert(predicate);
    throw new Error("Assertion Failed.");
  }
}

function fizzBuzz(n) {
  // assertPredicate関数を使って、意図した値を返すかチェックし、修正しましょう
  let output = "";
  if (n % 3 == 0) output = "Fizz";
  else if (n % 5 == 0) output = "Buzz";
  else if (n % 15 == 0) output = "FizzBuzz";
  else output = "not covered";

  return output;
}

function assertionTests() {
  // ここから書きましょう
}
```

## automatic test

- メールの farm 画面など。

- 100 までの素数のチェック

```javascript
// リスト内のすべての項目が素数であるかどうか
// すべてが重複していないか
// すべてが n 以下であるかどうか
// k 個の素数が含まれているかどうか
// のチェックを行います。


// では、100 までの素数をテストしてみましょう。100 未満の素数は全部で 25 個あるはずです。
//callbackってprimeCheck(6,15)でtrueが返ってくる。。
//true(a)っておかしい。。
//primeCheck(a)なの??
function assertionTest(a, callback){
    let result = callback(a);
    console.log(`Checking against ${a}, is it valid?...${result}`)
    console.assert(result);
    return true;
}

function sieveOfPrimes(n){//15

    let cache = [];
    for(let i = 0; i < n; i++) cache.push(true);
    //[true,true,true,,,,,]//15のtrue
    //Math.ceil->//0.95->1//1.34->2//-4->-4
    //Math.sqrt//仮引数の平方根を求める。
    for (let currentPrime = 2; currentPrime < Math.ceil(Math.sqrt(n)); currentPrime++){
        if (!cache[currentPrime]) continue;
        let i = 2;
        let ip = i * currentPrime;
        while (ip < n){
            cache[ip] = false;
            i += 1;
            ip = i * currentPrime;
        }
    }

    let primeNumbers = [];
    for (let i = 2; i < cache.length; i++){
        if (cache[i]) primeNumbers.push(i);
    }

    return primeNumbers;
}

// kとnを受け取り、リストAを取り込み、A内の全ての要素が重複しておらず、かつn以下の素数であり、合計でk個存在するかチェックする関数を返します。
// 返されるデータは、クロージャー関数です。
function primeCheck(k, n){
    let isPrime = num =>{
        if(num > 1){
            for(let i = 2; i <= Math.floor(Math.sqrt(num)); i++){
                if (num % i == 0) return false;
            }
            return true;
        }
        return false;
    }
    /
    let script = aList =>{
        // set() はリストを受け取り、重複していない要素のみを返します。
        if (new Set(aList).size !== aList.length) return false;
        if (aList.length !== k) return false;
        for(let i = 0; i < aList.length; i++){
            if(aList[i] > n || !isPrime(aList[i])) return false;
        }
        return true;

    }
    return script;

}
//Checking against 2,3,5,7,11,13, is it valid?...true
assertionTest([2,3,5,7,11,13],primeCheck(6, 15));
              //引数の素数を返す, //callbackで、primeCheck(6,15)sieveOfPrimes(15)になる。
assertionTest(sieveOfPrimes(15),primeCheck(6, 15));
assertionTest(sieveOfPrimes(100),primeCheck(25, 100));
assertionTest(sieveOfPrimes(10000),primeCheck(1229, 10000));

assertionTest([2,3,5,7,11,13,15],primeCheck(6, 15)); // Error
assertionTest([2,3,5,7,11,12],primeCheck(6, 15)); // Error
assertionTest([2,3,5,7,11,19],primeCheck(6, 15)); // Error
```

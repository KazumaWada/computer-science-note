- 関数名がない。一行で return まで書かれている。(Supplier<String> lambda1 = () -> { return "A new world";};)

```java
import java.util.function.*; // ユーティリティライブラリの読み込み

class Main{
   public static void main(String[] args){

      // では、ラムダ式を使ってその場で関数を作成してみましょう。呼び出し可能オブジェクトの参照が返されるので、データを返す呼び出しを行うことができます。見ての通り、名前がないので匿名関数と呼ばれています。
      // シンタックス:
      // 入力 -> { 式 };
      // (入力1, 入力2,.....入力n) -> { 式 };
      // ラムダ式は呼び出し可能オブジェクトを返します

      // Javaには、ラムダ関数を定義するためのライブラリが付属しています。ラムダ式を保存するか、出力や入力として渡す必要があります。
      // その一つが、零項関数（引数を取らない関数）を表し、データを返す「Supplier」型と呼ばれるものです。この「Supplier」はデータを供給します。
      Supplier<String> lambda1 = () -> { return "A new world";};//関数。出力はされない。

      // 零項関数を実行するにはget関数を使用します。
      System.out.println(lambda1);//正しく出力されない。
      System.out.println(lambda1.get());//A new world

      Integer p = 40;

      //ラムダ式スコープ外の変数にアクセスします。
      Supplier<Integer> lambda3 = () -> { return 4+p;};
      System.out.println(lambda3.get());//44
      Supplier<String> lambda4 = () -> { return "P is " + p;};
      System.out.println(lambda4.get());//P is 40

      // 特定の入力を受け取る匿名関数を作成することができます。通常の関数と同じように、呼び出すときに入力を渡すことができます。
      // 単項関数（入力を1つ取り込んで出力を返す関数）である関数型を使って、出力を返します。
      Function<Integer, Integer> squaredF = (x)->{ return x * x;};//xは仮引数。

      // apply関数で、仮引数を使うことができる。
      System.out.println("Squaring...." + squaredF.apply(4)); // Squaring...16

      //他のすべての関数と同様に、呼び出されたときに独自のローカルスコープを作成します。
      Function<Integer, String> sheepF = (x)->{
         String sheeps = "";
         for(int i = 1; i <=x;i++) sheeps += i + "sheep~";
         return sheeps;
      };
      System.out.println("looping..." + sheepF.apply(5)); //looping...1sheep~2sheep~3sheep~4sheep~5sheep~

      // --------OOP 匿名クラス------
      // また、クラス、クラスの抽象化、またはインターフェースで指定された関数を実装したり、オーバーライドしたりするために、匿名クラスを作成することもできます。これらのクラスはインスタンス化してからオブジェクトに格納するか、入力として渡すか出力として返す必要があります。
      // utils.functionsライブラリからの関数と他のインポートは、実際にはすべてインターフェースです。インターフェースはオブジェクトが実装しなければならないメソッドのセットを提供します。例えば、Function テンプレートは apply() 関数を実装しなければなりませんが、ここでは匿名で実装しています。
      // OOPコースでは、クラスの抽象化とインターフェースについて学びます。
      Function<Integer, Integer> innerObj = new Function<Integer, Integer>(){
         public Integer apply(Integer x) {
            // pはこの匿名関数のスコープ外でアクセスされます。
            return x + p;
         }
      };
      System.out.println();
      System.out.println("Method defined through an anonymous class..." + innerObj.apply(10));
   }
}
```

- ラムダ関数をオブジェクトで呼び出してみる。

```java
// Javaではインターフェースを実装した無名クラスのオブジェクトをラムダ式で表します。
// インターフェースについてはOOPコースで詳しく学習します。
import java.util.function.*;// パッケージfunctionをimportします。

class Main{
    public static Integer callIntBiFunc(BiFunction<Integer, Integer, Integer> f, Integer x, Integer y){
        return f.apply(x,y);
    }

   public static void main(String [] argt){
      // BiFunctionインターフェースを使用します。引数を2つ持つことができ、戻り値があります。
      // apply()で引数を渡すことができます。
      BiFunction<Integer, Integer, Integer> biFunction = new BiFunction<>(){
         public Integer apply(Integer x, Integer y){
            return x + y;
         }
      };
      System.out.println(biFunction.apply(3, 5));// 8

      // この関数は、どこにも保存されておらず、渡されてもいないので、ガベージコレクタによって後に消去されます。
      System.out.println(callIntBiFunc((x, y) -> x + y, 3,5));
      // 変数に格納して呼び出してみましょう。呼び出し可能オブジェクトはオブジェクトなのですから。
      BiFunction<Integer, Integer, Integer> myCallable = (x, y) -> x + y;

      System.out.println(myCallable.apply(3, 5));//8
      System.out.println(myCallable.apply(10,10)); //20
      System.out.println(myCallable.apply(150,5)); //155
      System.out.println(myCallable);

   }
}
```

# 高階関数

関数を入力として受け取り、関数を出力として返す関数

```java
// funcAdd = function(a, b){return a + b};
// useFunction(funcAdd, 10, 5);

// Javaでは上記のような関数を入力として受け取り、関数を出力する関数は構造上できない仕様になっていますが、インターフェースを使って匿名関数を作成し、呼び出し可能オブジェクトの参照を受け取ることができます。

//ラムダ式に実装できる主な関数型インターフェース

//  　　　　　　引数あり　　引数なし
//  戻り値あり　Function   Supplier　
//　戻り値なし  Consumer

//  引数あり 戻り値boolean　 Predicate
//  引数2つ  戻り値あり　　　 BiFunction

// 他にもたくさんあるのでドキュメンテーションを確認しましょう。

import java.util.function.*;

class Main{

    // この関数は関数の参照を受け取り、ローカルスコープ内で呼び出します。
    public static String functionInputTest(Supplier<String> func){
        return func.get() + ".... called from another function!";
    }

    public static Integer fSquaredX(Function<Integer, Integer> f, int x){
        int p = x * x;
        return f.apply(p);
    }

    public static String fSquaredX2(Function<Integer, String> f, int x){
        int p = x * x;
        return f.apply(p);
    }

    public static void main(String[] args){

        Supplier<String> myCallable = () -> {return "hello world";};
        System.out.println(myCallable.get());// hello world
        System.out.println(functionInputTest(myCallable));

        Function<Integer, Integer> callable1 = (p) -> {return p + 30;};
        // f(a^2) = a^2 + 30;
        System.out.println(fSquaredX(callable1, 5));// 25 + 30 = 55

        // 呼び出し可能オブジェクトを変数内に格納します。
        Function<Integer, String> callable2 = (p) -> {return "p is " + p;};

        System.out.println(fSquaredX2(callable2, 10));// p is 100
        System.out.println(fSquaredX2(callable2, 8));// p is 64
    }
}
```

- ラムダで表現する再帰(a,b の他に g(i)が加わった)

```java
import java.util.function.*;

class Main{
    //summation: 引数なし function: 引数あり

    public static int summation(Function<Integer,Integer> g, int a, int b){
        if(b < a) return 0;
        //identity i return iだから、1,2,3,4,5,って返される。
        return g.apply(b) + summation(g, a, b-1);
    }

    //オーバーロード                             //i,      1,   10
    public static int summation(Supplier<Integer> g, int a, int b){
        if(b < a) return 0;
        //getでg関数が実行される。//1とか2とか100とかになるはず。
        return g.get() + summation(g, a, b-1);
    }

    public static int pPi(Function<Integer, Integer> g, int a, int b){
        if(b < a) return 1;
        return g.apply(b) * pPi(g, a, b-1);
    }

    //オーバーロード
    public static int pPi(Supplier<Integer> g, int a, int b){
        if(b < a) return 1;
        return g.get() * pPi(g, a, b-1);
    }

    public static void main(String[] args){

        // 10までの総和
        // 1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + 10 = 55
        //supplierのsummation//いや、iという引数を持つからfunctionなのでは?
        Function<Integer, Integer> identity = i -> i;
        System.out.println(summation(identity, 1, 10));//55

        // 10 * 100 の計算//functionのsummatation?
        Supplier<Integer> function = () -> 10;
        System.out.println(summation(function, 1, 100));//1000

        // 10の階乗(10!)
        System.out.println(pPi(identity, 1, 10));//3,628,800

        // 5^10 の計算
        Supplier<Integer> function5 = () -> 5;
        System.out.println(pPi(function5, 1, 10));// 9765625
    }
}
```

- 円の面積を求める

```java
import java.util.function.*;

class MyClass{
    public static void main(String[] args){
        //半半3.14
        Function<Integer, Double> circleArea = (a) ->{return a*a*3.14;};
        System.out.println(circleArea.apply(1));
        System.out.println(circleArea.apply(5));
    }
}

```

- ゼロの増殖

```java
//duplicateZero(5) --> 00000
import java.util.function.*;

class MyClass{
    public static void main(String[] args){
        Function<Integer, String> duplicateZero = (x)->{
            String count = "";
            for(int i = 0; i<x; i++)count += 0;
            return count;
        };
    System.out.println(duplicateZero.apply(5));
    System.out.println(duplicateZero.apply(10));
    }
}
```

### ラムダ関数で、再帰を表現する。

- バリデーション
  Java の Predicate 関数が使われている。test メソッドを持つ.引数を一つ受け取り、boolean を返す。
  if 文で、&&とか||を多用すると見づらくなってしまうのを防ぐためにも使われている。

- 例

```java
public class Man {
    private int hight;
    private int income;
    private String looks;
    private int age;

    public Man(int hight, int income, String looks, int age) {
        this.hight = hight;
        this.income = income;
        this.looks = looks;
        this.age = age;
    }
    public int getHight() {
        return hight;
    }
    public int getIncome() {
        return income;
    }
    public String getLooks() {
        return looks;
    }
    public int getAge() {
        return age;
    }
}


//ifを使うと、
Man man = new Man(170,900,"hot",20);
if(getHight > 170 && getIncome > 800||
   getLooks == hot && age <30){
}
//predicateを用いると、
import java.util.function.Predicate;

public class Main {
  public static void main (String[] args) {

     Predicate<Man> isHight = m -> m.getHight() > 170;
     Predicate<Man> isIncome = m -> m.getIncome()<1000;
     Predicate<Man> isLooks = m -> m.getLooks()=="hot" ;
     Predicate<Man> isAge = m ->m.getAge()<35;
     Predicate<Man> type = isHight.and(isIncome).or(isLooks.and(isAge));
     Man man = new Man(170,900,"hot",20);
    if(type.test(man)) {
        System.out.println("OK");
    }else {
        System.out.println("NG");
    }
  }
}
```

```java
//validation(predicate1, "@gmail.com") --> false

import java.util.function.*;
class MyClass{

    public static void main(String[] args){
        //このPredicateはclass?//そういうメソッドが元々備わっている。
        //＠から始まっていない
        Predicate<String> predicate1 = s -> s.indexOf("@") != 0;
        System.out.println(predicate1.test("@gmail.com"));//f
        System.out.println(predicate1.test("kkk@gmail.com"));//t
        //空白がない
        Predicate<String> predicate2 = s -> s.indexOf(" ") == -1;
        System.out.println(predicate2.test("Hello world"));//f
        System.out.println(predicate2.test("Helloworld"));//t

        Predicate<String> predicate3 = s -> {
            //isUpperというからの変数に、falseを入れる。後で、if文が通ったら、trueに変わる。
            boolean isUpper = false;
            boolean isLower = false;
            for(int i = 0; i < s.length(); i++){
                //Character.isUpperCaseという備え付けの関数がある。
                //大文字があれば、trueそうでなければfalseを返す。
                if(Character.isUpperCase(s.charAt(i))) isUpper = true;
                if(Character.isLowerCase(s.charAt(i))) isLower = true;
            }
            //大文字と小文字両方混ざっていれば、true&&trueでtrueを返す。
            return isUpper && isLower;
        };
        System.out.println(predicate3.test("hello world"));
        System.out.println(predicate3.test("HELLO WORLD"));
        System.out.println(predicate3.test("Hello world"));
    }
}
```

- バリデーション(他の解き方)これいいかも!!

```java
import java.util.function.*;

class MyClass{
    public static void main(String[] args){
        Function<String, Boolean> predicate1 = (str) -> {
            return str.charAt(0) != '@';
        };

        Function<String, Boolean> predicate2 = (str) -> {
            return str.indexOf(" ") == -1;
        };

        Function<String, Boolean> predicate3 = (str) -> {
            if (str.toUpperCase().equals(str)) {
                return false;
            } else if (str.toLowerCase().equals(str)) {
                return false;
            }

            return true;
        };

        System.out.println(validation(predicate1, "@gmail.com"));
        System.out.println(validation(predicate1, "kkk@gmail.com"));
        System.out.println(validation(predicate2, "Hello world"));
        System.out.println(validation(predicate2, "Helloworld"));
        System.out.println(validation(predicate3, "hello world"));
        System.out.println(validation(predicate3, "HELLO WORLD"));
        System.out.println(validation(predicate3, "Hello world"));
    }

    public static boolean validation(Function<String, Boolean> f, String word) {
        return f.apply(word);
    }
}
```

- ラムダ総和(validation で再帰をしているため、それぞれの関数に for を書かなくて良い。総和の和は別だが、、)

```java
import java.util.function.*;

class MyClass{
    public static void main(String[] args){
        Function<Integer, Boolean> odd = (x) -> {
            if (x % 2 == 0) return false;
            else return true;
        };
        System.out.println(sum(odd, 3));
        System.out.println(sum(odd, 10));
        System.out.println(sum(odd, 25));

        Function<Integer, Boolean> multipleOf3Or5 = (x) -> {
            if (x % 3 == 0 || x % 5 == 0) return true;
            else return false;
        };
        System.out.println(sum(multipleOf3Or5, 3));
        System.out.println(sum(multipleOf3Or5, 10));
        System.out.println(sum(multipleOf3Or5, 100));

        Function<Integer, Boolean> prime = (x) -> {
            for (int i = 2; i < x; i++) if (x % i == 0) return false;//割れてしまったらfalse
            if (x == 1) return false;//1だったらfalse
            return true;
        };
        System.out.println(sum(prime, 2));
        System.out.println(sum(prime, 10));
        System.out.println(sum(prime, 100));
    }

    public static int sum(Function<Integer, Boolean> f, int n) {
        if (n <= 0) return 0;
        //if(true)だったら、nが足される。再帰。。。いや天才すぎんこのコード?!
        if (f.apply(n)) return n + sum(f, n - 1);
        return sum(f, n - 1);
    }
}
```

- 動物と人間の年齢

```java
import java.util.function.*;
//comparison(predicate1, 20, 1) --> false
class MyClass{

    public static void main(String[] args){

        //dog age & human age//比較結果を返す//犬の方が高い->true
          Function<Integer, Integer> predicate1 = (human,dog) -> {
            return human < 20 + (dog-2)*7;
        };

        //cat age & human age
        Function<Integer, Integer> predicate2 = (human,cat) -> {
            return human < 24 + (cat-2)*4;
        };

        public static boolean comparison(Function<Integer, Integer> f, int humanAge, int animalAge){
            return f.apply(humanAge,animalAge);
        };

};

}
```

- 動物と人間の年齢(ユーザー回答)

```java
//PredicateとBiPredicateの違いは?
//BiPredicate is same with the Predicate, instead, it takes 2 arguments for the test.
//https://mkyong.com/java8/java-8-bipredicate-examples/
import java.util.function.*;

class MyClass{
    public static void main(String[] args){
        BiPredicate<Integer, Integer> predicate1 = (humanAge, dogAge) -> {
            return 20 + (dogAge - 2) * 7 > humanAge;
        };

        System.out.println(comparison(predicate1, 20, 1));
        System.out.println(comparison(predicate1, 25, 3));
        System.out.println(comparison(predicate1, 50, 7));

        BiPredicate<Integer, Integer> predicate2 = (humanAge, catAge) -> {
            return 24 + (catAge - 2) * 4 > humanAge;
        };

        System.out.println(comparison(predicate2, 20, 1));
        System.out.println(comparison(predicate2, 25, 3));
        System.out.println(comparison(predicate2, 50, 7));
    }

    public static boolean comparison (BiPredicate<Integer, Integer> f1, Integer humanAge, Integer dogAge) {
        return f1.test(humanAge, dogAge);
    }


}
```

### ラムダ関数(関数を返すケース)

````java
import java.util.function.*;

class Main{

    public static void main(String[] args){

        Supplier<Supplier<String>> helloFunction = () -> () -> {return "hello world";};

        //この関数は関数を返します
        System.out.println(helloFunction.get());//Main$$Lambda$2/0x0000000800b4c040@4b1210e
        // 戻り値としてのこの関数を実行
        System.out.println(helloFunction.get().get());//hello world
        //戻り値としてこの関数を保存
        Supplier<String> outputF = helloFunction.get();//この場合は、.get().get()で保存しなくていいのか。。
        System.out.println(outputF.get());//hello world

        // 数値xを取り込み、その後xと入力を乗算する関数を返します。
        Function<Integer, Function<Integer, Integer>> constantMultiplication = x -> y -> {return y*x;};

        System.out.println(constantMultiplication.apply(4).apply(3));

        // xを取り込んだ関数を変数に代入し、入力yを渡すこともできます。
        //関数を変数に代入して、その変数から実行することができる。
        Function<Integer, Integer> multiplyBy4 = constantMultiplication.apply(4);

        System.out.println(multiplyBy4.apply(3));//3*4 = 12
        System.out.println(multiplyBy4.apply(10));//10*4 = 40
        System.out.println(multiplyBy4.apply(5));//5*4 = 20
    }
}
- 上のように、ラムダ関数の関数を渡すときは、必ずしも無名関数で渡す必要はない。
```java
import java.util.function.*;
import java.util.Random;
//suplierって何だっけ?
//「Supplier」はデータを供給します。
Supplier<String> lambda1 = () -> { return "A new world";};//関数。出力はされない。
//suplierを実行するときは、getなのかな。
//Functionはデータ(仮引数)を受け取ってデータを返す。

class Main{

    public static String multiCall(Function<String, String> f, Supplier<String> fInputF, String message) {
        //greeting(nameGenerator()) + "......." + message;
        //"Hello there " + name(nameGenerator())+ "......." + message;
        return f.apply(fInputF.get()) + "......" + message;
    }

    //nameGeneratorはsupplierなので、データを供給するのみ。
    public static void main(String[] args){
        //name変数は、greeting(nameGenarator())ってなってるから、name = strって事かな?
        Function<String, String> greeting = name -> "Hello there " + name;

        Supplier<String> nameGenerator = () ->{
            String str = "";
            for(int i = 0; i < 10; i++){
                Random r = new Random();
                str += (char)(r.nextInt(26) + 'a');
            }
            return str;
        };

        System.out.println(multiCall(greeting, nameGenerator, " Thank you"));
        //Hello there randomName..... Thank you
    }
}
````

- 挨拶????????????????????

```java
import java.util.function.*;
import java.util.Random;

class MyClass{
                               //morning           //john
    public static String greet(Supplier<String> name, Function<String> f) {
        //morning()
        return f.apply(name.get()); //+ "How are you?";
    }

    public static void main(String[] args){

        Function<String> greet = name -> name + "How are you?";

        Supplier<String> morning = ()->{return "Good Morning";};
        Supplier<String> afternoon = ()->{return "Good Afternoon";};
        Supplier<String> evening = ()->{return "Good Evening";};

        System.out.println(greet(morning, "John"));
        System.out.println(greet(evening, "John"));
        //--> Good Morning John. How are you?
    }
}
```

# call back

- 同期型 call back(synchronous)
  関数 F が関数内で関数 C を呼び出したときに、関数 C の処理が終わってから、続きの処理をする場合。
- 同期型 call back

```java
import java.util.function.*;

class Main{
    public static void main(String[] args){
        BiFunction<Function<Integer, Integer>, Integer, Integer> synchronousFunction = (f, x)->{
            int result = f.apply(10);//下の関数を呼んだ//5が返ってきた?
            System.out.println("help01");//3個目
            //xは元々254だから、systemoutで254
            return f.apply(x) + f.apply(x*x) + result;
        };

        Function<Integer, Integer> tempFunc = x ->{
            System.out.println("Call on " + x);//1個目:10//4個目:254
            System.out.println("help02");//2個目//5個目
            return x/2;//一番最初の10/2はスルー or resultに保存されている?
        };

        System.out.println(synchronousFunction.apply(tempFunc,254));
    }
}
// Call on 10
// help02
// help01
// Call on 254
// help02
// Call on 64516
// help02
// 32390
```

- カスタム配列

```java
//int[] customArray(int(int), int[])
//int cube(int n
class MyClass{
    public static void main(String[] args){

    }
}

```

- カスタム配列

```java
import java.util.function.*;
class MyClass{
    public static int[] customArray(Function<Integer, Integer> f, int[] arr){
        //1,cubeを呼ぶ
        int[] res = new int[arr.length];//4
        for(int i = 0; i < res.length; i++){
            //res[0] = cube(3)
            //res[0] = 27
            res[i] = f.apply(arr[i]);//ここが同期ポイント。
            //[0]になった時点で、終わりなのかそれとも最後まで配列を作って、[0]を出すのか。
            //ループだから、最後まで計算される。。。
            System.out.println(res);
        }
        return res;//[27,]
    }
    public static void main(String[] args){
        //2,nとは、、//3,customArrayに戻り、仮引数に取り掛かる。
        Function<Integer, Integer> cube = (n) -> n * n * n;

        Function<Integer, Integer> splitAndAdd = (n) -> {
            int total = 0;
            while(n > 0){
                total += n % 10;//total = 0 + 3
                n /= 10;// n = n /10 //n =
            }
            return total;
        };
        //customArray(関数,配列);
        //cube(n) nを受け取って3乗する
        //splitAndAdd(n) nを受け取って、全ての桁数の合計を返す。

        //System.out.println(customArray(cube, new int[]{3, 11, 24, 31})[0]);//27
        // System.out.println(customArray(cube, new int[]{3, 11, 24, 31})[1]);
        // System.out.println(customArray(cube, new int[]{1, 2, 3, 4})[3]);
        System.out.println(customArray(splitAndAdd, new int[]{3, 11, 24, 31})[2]);
        // System.out.println(customArray(splitAndAdd, new int[]{105, 19, 912, 643})[1]);
        // System.out.println(customArray(splitAndAdd, new int[]{105, 19, 912, 643})[3]);
    }
}
```

- 過半数

```java
//bool: majority(bool(int), int[]) 過半数がtrueであるかどうか
//bool isOdd(int n) 奇数かどうか
//bool isEven(int n)　偶数かどうか

//majority(isOdd, [1,2,3,4,5]) --> true

//majority(isOdd, [2,4,6,7,8]) --> false


//booleanとBooleanの違い
//booleanは型
//Booleanは、型の固まりを囲ったclass

//odd,evenはboolだけでいいけど、majorityでは、halfとかでintが必要。。でも、どこに書けばいいんだ。。
class MyClass{
    public static Boolean Integer majority(Function<Boolean> f, int[] arr){
        int[] res = new int[arr.length];
        int count = 0;
        int half = Math.floor(arr.length);
        for(int i = 0; i<res.length; i++){
            //このコードがラムダであり、callBackでもある。
            //ここで使われているintはどこに定義すればいいの??
            res[i] = f.apply(arr[i]);

        };
        //res[true,false,false,false,,,,]
        for(int i = 0; i<res.length; i++){
            if(res[i] == true)count++;
        }
        return count > half ? true : false;
    }
    public static void main(String[] args){
        Function<Boolean> isOdd = (n)->{
            if(n%2 != 0)return true;
            else return false;
        };
        Function<Boolean> isEven = (n)->{
            if(n%2 == 0)return true;
            else return false;
        };
    System.out.println(majority(isOdd, new int[]{1,2,3,4,5}));
    }
}

Main.java:2: error: cannot find symbol
    public static Integer majority(Function<Boolean> f, int[] arr){
                                   ^
  symbol:   class Function
  location: class MyClass
Main.java:5: error: incompatible types: possible lossy conversion from double to int
        int half = Math.floor(arr.length);
                             ^
Main.java:14: error: incomparable types: int and boolean
            if(res[i] == true)count++;
                      ^
Main.java:16: error: incompatible types: boolean cannot be converted to Integer
        return count > half ? true : false;
                            ^
Main.java:19: error: cannot find symbol
        Function<Boolean> isOdd = (n)->{
        ^
  symbol:   class Function
  location: class MyClass
Main.java:23: error: cannot find symbol
        Function<Boolean> isEven = (n)->{
        ^
  symbol:   class Function
  location: class MyClass
6 errors























//上記問題の意味がわからない。。
//Java Multiple Typesとかでググるしかないのか。
//単純に、使われている型が2つだから、2つ必要なだけって話でしょ??
//でもエラー文を見ても、booleanがダメBooleanがダメって言われて何が何だかわからない。
```

### リスト反復処理

- forEach

```javascript
function forEach(f, list) {
  for (let i = 0; i < list.length; i++) f(list[i]);
}

forEach((x) => console.log(x), [2, 3, 4, 5]);

//////// 通常のfor loop
function simpleLoop() {
  let l = [3, 4, 5, 6, 6, 10];
  let counter = 0;

  for (let i = 0; i < l.length; i++) {
    counter += l[i] * l[i];
  }

  return counter;
}
//////////

function loopDifferent() {
  let l = [3, 4, 5, 6, 6, 10];
  let counter = 0;

  let forEach = (f, list) => {
    for (let i = 0; i < list.length; i++) {
      // 親スコープ変数にアクセスすることができます。自由度はありますが、この方法でラムダを使うと副作用が出ることがあるので注意が必要です。
      // ここではfを毎度呼び出しています。fに変化があった場合、副作用が発生します。

      //list[i]*list[i];
      f(list[i]);
    }
  };
  //forEach(,[3,4,5,6,6,10])
  //forEach(x=>)ではなく、forEach(function(x))なのか。。
  //forEachは、functionを使って、map,filterはfunctionが既に含まれているから、書かなくてもいいのか。
  forEach(function (x) {
    counter += x * x;
  }, l);

  return counter;
}

console.log(simpleLoop()); //通常のloop
console.log(loopDifferent());

// JavaScriptの配列には、この反復処理を行うforEachメソッドが付属しています。
function loopDifferentLibrary() {
  let l = [3, 4, 5, 6, 6, 10];
  let counter = 0;

  l.forEach(function (x) {
    counter += x * x;
  });

  return counter;
}

console.log(loopDifferentLibrary());
```

list 内に関数を適用する事ができる。

- stream を使うと、簡単に実装できるようになる。

## stream + map or filter

```java
import java.util.Arrays;
import java.util.ArrayList;
import java.util.List;
import java.util.function.*;
import java.util.stream.*;

class Main{

    public static void main(String[] args){
                                                                                //(x*x, nums)
        BiFunction<Function<Integer, Integer>, int[], ArrayList<Integer>> myMap = (f, list) ->{

            ArrayList<Integer> result = new ArrayList<>();
            //nums.length
            for(int i = 0; i < list.length; i++){
                result.add(f.apply(list[i]));//f == x*x//result.add(list[i]*list[i]);
            }
            return result;
        };

        Function<Integer, Integer> squaredX = x -> x * x;
        int[] nums = {1,2,3,4,5,6,7};
        System.out.println(Arrays.toString(nums));//[1,2,3,4,5,6,7]
        System.out.println(myMap.apply(x -> x * x, nums));//[1, 4, 9, 16, 25, 36, 49]
        System.out.println(myMap.apply(squaredX, nums));//[1, 4, 9, 16, 25, 36, 49]

        //streamを使うと、簡単になる。↓

        // StreamAPIのmapメソッドを実装します。
        // ストリームは、データソース => 中間操作 => 終端操作　という一連の流れで処理をします。
        // mapメソッドは中間操作の1つで、Functionインターフェースを引数に持ち、指定された処理を適用した結果から構成されるストリームを返します。
        // 中間操作は取得したストリームに何らかの処理をしてストリームを返すので、メソッドチェーンにより別の処理を続けることも可能です。

        List<Integer> numList = Arrays.asList(1,2,3,4,5,6,7);
        numList.stream()
            .map(x -> x*x)
            .forEach(System.out::println);//値の方をわかって出力しているって事?すご。


        numList.stream()
            .map(squaredX)
            .forEach(System.out::println);

        }
}
```

- 文字のスワップ

```java
import java.util.Arrays;
import java.util.function.*;

class Solution{
    public static char[] swapCase(char[] charList){
        //                                             (swap,['l','A','m','b','D','A'])
        BiFunction<Predicate<Character>, char[], char[]> myMap = (p, list) ->{
            char[] result = new char[list.length];//charList.length
            for(int i = 0; i < list.length; i++){
                //大文字だったら小文字へ、小文字だったら大文字へ.
                if(p.test(list[i])) result[i] = Character.toLowerCase(list[i]);
                else result[i] = Character.toUpperCase(list[i]);
            }
            return result;
        };

        Predicate<Character> swap = c -> {
            if(Character.isUpperCase(c)) return true;
            else return false;
        };

        return myMap.apply(swap, charList);

    }
}
//swapCase(['l','A','m','b','D','A'])
```

- 文字のスワップ

```javascript
//map内に関数を書く方法
function swapCase(charList) {
  // 関数を完成させてください
  return charList.map((x) => {
    if (x === x.toUpperCase()) return x.toLowerCase();
    else return x.toUpperCase();
  });
}
//map内に関数名を入れる方法
function swapCase(charList) {
  swap = (x) => (/[A-Z]/.test(x) ? x.toLowerCase() : x.toUpperCase());
  return charList.map(swap);
}
```

- filter 高階関数
  ->filter(boolean,list[])を受け取り、どれを filter,削除するかを決定する

```javascript
function myFilter(predicateF, list) {
  let results = [];
  for (let i = 0; i < list.length; i++) {
    //(x=>x%2!==0(list[i]) === true)
    if (predicateF(list[i]) === true) results.push(list[i]);
  }

  return results;
}

let list1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(myFilter((x) => x % 2 !== 0, list1)); //[ 1, 3, 5, 7, 9 ]

// すべてのJS配列オブジェクトには、この処理を行うフィルタ関数が含まれています。
//map: 2で割った値を全て返す。
//filter: 2で割れる値を全て返す。
console.log(list1.filter((x) => x % 2 !== 0)); //[ 1, 3, 5, 7, 9 ]
```

```java
import java.util.Arrays;
import java.util.ArrayList;
import java.util.List;
import java.util.function.*;
import java.util.stream.*;

class Main{

  public static void main(String[] args){

    BiFunction<Predicate<Integer>, int[], ArrayList<Integer>> myFilter = (f, list) ->{

        ArrayList<Integer> result = new ArrayList<>();
        for(int i = 0; i < list.length; i++){
            if(f.test(list[i])) result.add(list[i]);
        }
        return result;
    };

    int[] nums = {1,2,3,4,5,6,7,8,9,10};
    System.out.println(myFilter.apply(x -> x%2!=0, nums));//奇数かどうか

    ////////////////////////上記をStreamAPIのfilterメソッドを実装//////////////////////////
    // filterメソッドはPredicateインターフェースを引数に持ち、指定した条件に合った要素で構成されるストリームを返す中間操作です。

    List<Integer> numList = Arrays.asList(1,2,3,4,5,6,7,8,9,10);
    numList.stream()
           .filter(x -> x%2!=0)
           .forEach(System.out::println);//すでにPredicate引数を持っているからok
    }
}
```

- 年齢チェック

```javascript
function ageCheck(ages) {
  return ages.filter((x) => x >= 18);
}
```

```java
import java.util.Arrays;
import java.util.ArrayList;
import java.util.List;
import java.util.function.*;
import java.util.stream.*;

class Solution{
    public static int[] ageCheck(int[] ages){
        // 関数を完成させてください
        BiFunction<Predicate<Integer>, int[], ArrayList<Integer>> myFilter = (f, list) ->{
            //ArrayList[] result = new ArrayList<>();
            List<Integer> result = Arrays.asList(ages);
            for(int i = 0; i<list.length; i++){
                                   //ここで、listはint[], resultはArrayList.
                if(f.test(list[i]))result.add(list[i]);
            }
            return result;
        };
        //return myFilter.apply(x -> x>18, ages);
    }
}
```

- 回文配列
  filter は、true の要素を返す。

```javascript
function palindromeArray(stringList) {
  let palindromeHelper = (w) => {
    for (let i = 0; i < w.length / 2; i++) {
      if (w[i] != w[w.length - 1 - i]) {
        return false;
      }
    }
    return true;
  };

  return stringList.filter((w) =>
    palindromeHelper(w.replace(/ /g, "").toUpperCase())
  );
}
```

### reduce

reduce(リスト、コールバック、初期値)

```javascript

///////////////////////javascriptで使われるreduce関数の中身/////////////////////////
function myReduce(reduceCallback, list, initial){
  let lastResult = initial;//1
  for(let i = 0; i < list.length; i++){
      let result = reduceCallback(list[i], lastResult);
      lastResult = result;
  }
  return lastResult;
}

let list1 = [1,2,3];
let list2 = [1,2,3,4,5,6,7,8,9,10];

//3!
//1*1 ->1
//1*2 ->2
//2*3 ->6
//reduceをわかりやすく関数にしたもの
console.log(myReduce((x,total)=>x*total, list1, 1));
//10!
console.log(myReduce((x,total)=>x*total, list2, 1));

/////////////////////////reduce関数////////////////////////////////////////////////////
// すべてのJS配列オブジェクトには、この処理を行うreduce関数が含まれています。
// 初期値が与えられていない場合は、最初の要素の値を初期値とし、2番目の要素から反復を開始します。
//↑つまり、初期値が与えられていない場合、[1,2,3,4,5,6,7,...]の1が初期値になる。
// reduce(累積値, 現在の値)
console.log(list2.reduce((total,x)=>total*x));//3628800
console.log(list2.reduce((total,x)=>total*x, 1));//3628800

//reduce関数を使って、文字列を連結させる
let list1 =  ["hello","world","and","hello","jupiter"];
//reduce(累積値, 現在の値)
//count(累積値) += i(現在の値)みたいなのに似ている。
console.log(list1.reduce(totalStr, curStr)=> totalStr + "," + currStr);
//↑join, split関数でも実装できる。
/////////////////////////////////////////////////////////////////////////////////////


///////////reduceは単一のデータ型を返す。つまり、2次元配列を1次元配列にreduceすることもできる。////////
let array2d = [[2,3,4,5],[5,22,34,4,5],[12,13,45,67,84]];

// 平坦化して1次元配列にします。
let flatten = array2d.reduce((flattenList,list)=>flattenList.concat(list));

//2->1次元配列に。つまり、全ての配列をつなげる。
console.log(flatten);
//全て繋がった配列の1番目の要素 == 3
console.log(flatten[1]);

// [
//    2,  3, 4,  5,  5, 22,
//   34,  4, 5, 12, 13, 45,
//   67, 84
// ]
// 3


```

### ラムダ出力(これまでは入力だった)

```javascript
// この関数は通常の関数として定義すればよいので、これは無意味です。
// どのような入力であっても、常に同じ関数が返されます。
function lambdaHelloWorld(randomInput) {
  console.log(
    randomInput +
      " was passed in but this function always returns the same lambda function"
  ); //lalilulelo was passed in but this function always returns the same lambda function
  return function () {
    return "Hello World";
  }; //Hello World
}

function helloWorld() {
  return "Hello World";
}

console.log(helloWorld()); //Hello World
console.log(lambdaHelloWorld("lalilulelo")());
```

```javascript
class LambdaMachine {
  constructor() {
    this.handler = {};
  }

  insert(stringLambda, lambda) {
    this.handler[stringLambda] = lambda;
  }

  retrieve(stringLambda) {
    return this.handler[stringLambda];
  }
}

let pythagora = function (x, y) {
  return Math.sqrt(x * x + y * y);
};
let addition = function (x, y) {
  return x + y;
};
let multiplication = function (x, y) {
  return x * y;
};

let lambdaMachine = new LambdaMachine();
//hashmap["文字列", その文字列と同じ名前の関数名];
lambdaMachine.insert("pythagora", pythagora);
//hashmap[文字列](== ラムダ関数)(3,4);
//つまり、pythagora(3,4);
console.log(lambdaMachine.retrieve("pythagora")(3, 4)); //return pythagora(3,4);

lambdaMachine.insert("addition", addition);
console.log(lambdaMachine.retrieve("addition")(2, 5));

lambdaMachine.insert("multiplication", multiplication);
console.log(lambdaMachine.retrieve("multiplication")(4, 10));

//lambdaMachine = new LambdaMachine()//作る。
//lambdaMachine.insert("pythagora", pythagora)//関数を呼び出す
//関数(3,4)という形になるのか！
//lambdaMachine.retrieve("pythagora")(3,4) --> 5
```

- ラウンドロビン(元々は、ドメインに複数の ip アドレスを持たせて、アクセス負荷を分散させる仕組み)

```javascript
// 問題 447 で、ラムダを自由に出し入れすることができるデータ構造 Lambda Machine を作成しました。retrieve では特定のキーを受け取り、それに基づいてラムダを出力しました。今回はルックアップテーブルに基づいて関数を返す場合とは別に、ラウンドロビンによって返す roundRobinRetrieve() メソッドを作成します。

// ラウンドロビンを用いるので、Lambda Machine の状態に配列 lambdaStorage を追加します。また。配列から特定の文字列を指定できるように、handler には関数名を示す文字列と配列のインデックスをペアとして挿入します。

class LambdaMachine {
  constructor() {
    this.handler = {};
  }

  insert(stringLambda, lambda) {
    this.handler[stringLambda] = lambda;
  }

  retrieve(stringLambda) {//lambdaMachine.retrieve("addition")(2, 5)
    return this.handler[stringLambda];
  }

  roundRobinRetrieve() {//
      //ここでaddictionが仮引数なしで呼ばれるようにすればいい。
      //addiction関数を返せばいい。
  }
}

let pythagora = function (x, y) {
  return Math.sqrt(x * x + y * y);
};
let addition = function (x, y) {
  return x + y;
};
let multiplication = function (x, y) {
  return x * y;
};

// let lambdaMachine = new LambdaMachine();
// lambdaMachine.insert("pythagora", pythagora);
// console.log(lambdaMachine.retrieve("pythagora")(3, 4)); //return pythagora(3,4);

// lambdaMachine.insert("addition", addition);
// console.log(lambdaMachine.retrieve("addition")(2, 5));

// lambdaMachine.insert("multiplication", multiplication);
// console.log(lambdaMachine.retrieve("multiplication")(4, 10));

lambdaMachine = new LambdaMachine()
lambdaMachine.insert("pythagora", pythagora)
lambdaMachine.insert("addition", addition)
lambdaMachine.insert("multiplication", multiplication)

lambdaMachine.roundRobinRetrieve()(6, 8) --> 10
lambdaMachine.roundRobinRetrieve()(6, 8) --> 14
lambdaMachine.roundRobinRetrieve()(6, 8) --> 48
lambdaMachine.roundRobinRetrieve()(6, 8) --> 10
```

### ラムダクロージャ(lambda closure) 新しい関数を生成する方法

- ユーザーは入力を関数に渡すことで、より多くの関数を生成することができ、関数は入力に応じて変化します。これは、何があっても同じ関数が返されるステートレス関数とは異なるのです。← 関数の入力を変えると、返ってくる値は変わる。これはどんな関数でもそうじゃん。。いまいちよく分からん。
- ある関数の状態を保存するためだけにクラスを作成する必要がない場合、これは非常に便利な機能です。

```javascript
// 連邦税を設定します。
const federalTaxes = 0.2;

// 州の税率を受け取って、所得を受け取り所得税を計算する関数を返す、関数を作成します。
function taxLambda(stateTax, state) {
  //なぜreturnが2回も書かれているのか。
  return function (income) {
    // 連邦税、州税の変数はどちらもスコープ外です。
    // この関数が作成されると、stateTax および federalTaxes の状態が何であれ、この関数にバインドされます。作成時の stateTax が 0.15 である場合、この関数の stateTax の状態は 0.15 になります。
    let taxes = federalTaxes + stateTax; //税 = 連邦税+州税
    // 何かが出力されたときにログを記録し、stateの文字列もバインドします。
    console.log("Computing taxes for state..." + state);
    return income - taxes * income;
  };
}

let californiaF = taxLambda(0.0725, "California");
let texasF = taxLambda(0.0625, "Texas");
let hawaiiF = taxLambda(0.04, "Hawaii");

// 税金を計算します。
let income = 40000;
console.log("Calculating income using lambdas"); // Calculating income using lambdas
//関数に、新しい関数を入れて、実行されている?
//どうやって、function(income)に40000が挿入されてるのかが分からん。
console.log(californiaF(income)); // Computing taxes for state...California// 29100
console.log(texasF(income)); // Computing taxes for state...Texas// 29500
console.log(hawaiiF(income)); // Computing taxes for state...Hawaii// 30400

let income2 = 500000;
console.log("------Calculating more income using lambdas------"); // ------Calculating more income using lambdas------
//どうやってfunction(income)に500000が挿入されてるのか分からん。
console.log(californiaF(income2)); // Computing taxes for state...California// 363750
console.log(texasF(income2)); // Computing taxes for state...Texas// 368750
console.log(hawaiiF(income2)); // Computing taxes for state...Hawaii// 380000
```

```javascript
//powerOfNLambda: 整数 n を受け取り、x を受け取り、xn を計算する関数を返してください。
// 整数nを受け取り、xを受け取り、x^nを計算する関数を返してください。
function powerOfNLambda(n) {
  return function (x) {
    return Math.pow(x, n);
  };
}
console.log(powerOfNLambda(3)(2));

//nDuplicateListLambda: 整数 n を受け取り、リストを受け取り、リストを n 回複製する関数を返します。
function nDuplicateListLambda(n) {
  let result = [];
  return function (list) {
    for (let i = 0; i < n; i++) {
      result.push(list);
    }
    return result;
  };
}

console.log(nDuplicateListLambda(2)([1, 2, 3, 4, 5])); //listを2回複製する
```

- タスクリスト

```javascript
// ToDo リストを文字列として格納した配列 toDoArr が与えられるので、ラムダクロージャーを利用し、toDoArr の各要素を順番に出力してみましょう。

//ラムダクロージャーを用いると内側の関数から外側の関数のスコープにアクセスできるようになります。
function todoCall(arr) {
  let index = 0;
  return function () {
    index++;
    return index - 1 < arr.length ? arr[index - 1] : "All done!";
  };
}
let todoCaller = todoCall(["Read a Book", "Work out", "Recursion"]);
console.log(todoCaller());
console.log(todoCaller());
console.log(todoCaller());
console.log(todoCaller());

// todoCaller() --> Read a Book
// todoCaller() --> Work out
// todoCaller() --> Recursion
// todoCaller() --> All done!
```

- 為替

```javascript
function convertDenomination(currency, rate) {
  return function (amount) {
    let result = Math.floor(amount / rate);
    return "Approx. " + result + " " + currency;
  };
}

let dollar = convertDenomination("USD", 110);
let euro = convertDenomination("Euro", 125);
let pound = convertDenomination("Pound", 150);
console.log(dollar(1000));
console.log(euro(1000));
console.log(pound(1000));
```

↑ 違う関数名でも使い回しができるのがラムダ関数ってことなのかな?

- 寄付

```javascript
//特定の国名とその国による今までの総寄付金額を返す
//全ての国ではなく、特定の国。
//全て合計されてしまっている。
function donor(country) {
  return function (moneyDonate) {
    let moneyDonateSofar = 0;
    moneyDonateSoFar = moneyDonateSofar + moneyDonate;
    return country + ": $ " + moneyDonateSoFar;
  };
}
let japan = donor("Japan");
let italy = donor("Italy");
console.log(japan(4000000)); //4000000
console.log(italy(7000000)); //7000000
console.log(japan(1000000)); //5000000
// italy = donor("Italy")
// canada = donor("Canada")
// japan(4000000) --> Japan: $4000000
// italy(7000000) --> Italy: $7000000
// canada(3000000) --> Canada: $3000000
// japan(1000000) --> Japan: $5000000
// canada(3000000) --> Canada: $6000000
// italy(0) --> Italy: $7000000
```

### decorator pattern(ある関数を呼び出す際にデコレーターを使うと、呼び出し先の関数を修正することなく、機能を追加することができます。)

design pattern(開発の中でよく起こる問題に対して対処するパターン、ライブラリやフレームワークで提供されている。)これらの一つに decorator pattern というものがある。

- デコレータを使って呼び出し可能オブジェクトに機能を追加したコード

```javascript
function simpleDecorator(f) {
  return function () {
    console.log("Running f......");
    return f(); //return helloWorld();
  };
}

function helloWorld() {
  return "Hello world";
}

let newFunc1 = simpleDecorator(helloWorld); //関数の仮引数に関数を入れている。
console.log(newFunc1());

let newFunc2 = simpleDecorator(() => "Hello Jupiter"); //仮引数が無い。どうやって、"Hello Jupiter"が返されているんだ??
console.log(newFunc2());

// Running f......
// Hello world
// Running f......
// Hello Jupiter
```

- デコレータ(分からん)

```javascript
//整数によって構成される配列の合計値を返す単項関数 sumOfArray を受け取り、配列の各要素が 10 未満であれば、その個数とエラーメッセージを返し、全て条件を満たしていれば sumOfArray の結果を返す validateDecorator という関数を作成し、テストケースを出力してください。

//要素が10未満だったら、その要素の数を計算してエラーメッセージ。
//全て10以上であればsumを出す。
function validateDecorator(arr){//[10,20,30,40]
　　 let i = 0;
    let sum = 0;
    return function(){
        i++;
        //10以下だったら,下の関数に行って、二度とreturn ?:に侵入しない。
        if(arr[i-1] < 10)countError(arr);
        return i-1 > arr.length? "Sum of array is " + sum = sum+arr[i-1]; //10以下かどうか&&そうでなかったら足す
    }
}

let index = 0;
let count = 0;
function countError(arr){//再帰するなら、ここがarr->arr
if(index > arr.length)return count + " error found";
index++;
return arr[index-1] > 10? countError(arr[index]) : count++;
}

let sum = validateDecorator(sumOfArray);
console.log(sum[10,20,30,40]);//Sum of array is 100
// sum = validateDecorator(sumOfArray)
// sum([9,10,20,30]) --> 1 error found
// sum([3,5,40,50]) --> 2 error found
```

# 非同期処理

- 下のコードでもわかるように、非同期の問題点は、データの流れがより複雑に見えてしまう。

```javascript
function runAfterXMs(f, ms) {
  // JavaScriptのsetTimeInterval関数はコールバックを受け取り、xミリ秒後に実行します。この関数は非同期関数です。この関数はコールバックをどこか別の場所に送り、残りの関数が実行を終了する間に実行します。
  console.log("running the function.....");
  setTimeout(f, ms); //return より先に実行されたけど、出力に1000ms待つ。その間に下が出力されている。
  return "This function has finished....";
}

// 1秒後にコールバックを実行します。
console.log(
  runAfterXMs(function () {
    console.log("Hello World!!!!");
  }, 1000)
);

//running the function.....
// This function has finished....
// Hello World!!!!
```

//////////////////////////////////javascript↓ /////////////////////////////////////////////////
//////////////////////////////////javascript↓ ///////////////////////////////////////////////////////////////////////////////////
//////////////////////////////////javascript↓ /////////////////////////////////////////////////////////////////////////////////////
//////////////////////////////////javascript↓ /////////////////////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////////////////////////////

//////////////////////////////////javascript↓ /////////////////////////////////////////////////
//////////////////////////////////javascript↓ ///////////////////////////////////////////////////////////////////////////////////
//////////////////////////////////javascript↓ /////////////////////////////////////////////////////////////////////////////////////
//////////////////////////////////javascript↓ /////////////////////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////////////////////////////

```javascript
// ①通常の関数
function asc(x) {
  return x + 1;
}
console.log(asc(1));

// ②無名関数(callable object): functionの後の関数名がない
let ascLambda1 = function (x) {
  return x + 1;
};
console.log(ascLambda1(1));

// アロー関数 // functionすらない。やってることは同じ。
//関数に渡したり、関数から返したり、変数に格納したりする事ができる。
//関数ではなくオブジェクトとして扱われる。
//stackは、一時的な値を格納するために使用
//heapは、プログラム実行時にデータ構造を格納する場所
//heap内にascLambda2がない場合、関数ではないので、ガベージコレクションとしてメモリから削除される。
let ascLambda2 = (x) => {
  return x + 1;
};
console.log(ascLambda2(1));
```

- もっと詳しく

```javascript
// その場でリテラルデータ型を作成することができます。
console.log(4);
console.log(4 + 6);
console.log("Hello " + " World");

// では、ラムダ式を使ってその場で関数を作成してみましょう。呼び出し可能オブジェクトの参照が返されるので、データを返す呼び出しを行うことができます。見ての通り、名前がないので匿名関数と呼ばれています。
console.log(function () {
  return "A new world";
});
console.log(
  (function () {
    return "A new world";
  })()
);

// ラムダ式は呼び出し可能オブジェクトを返します。
// これらの呼び出し可能オブジェクトを呼び出すと、匿名関数の戻り値と同じように評価される式になります。
console.log(
  (function () {
    return 4 + 5;
  })()
); //9

// 匿名関数スコープ外の変数にアクセスします。
let p = 40;
console.log(
  (function () {
    return p + 10;
  })()
); //50
console.log(
  (function () {
    return "P is " + p;
  })() + " ......"
); // P is 40.....

// 特定の入力を受け取る匿名関数を作成することができます。通常の関数と同じように、呼び出すときに入力を渡すことができます。
console.log(
  "squaring..." +
    (function (x) {
      return x * x;
    })(4)
); //squaring...16

// 匿名関数は、他のすべての関数と同様に、呼び出されたときに独自のローカルスコープを作成します。
console.log(
  "looping..." +
    (function (x) {
      let sheeps = "";
      for (let i = 1; i <= x; i++) sheeps += i + "sheep~";
      return sheeps;
    })(5)
); //looping...1sheep~2sheep~3sheep~4sheep~5sheep~

// また、JavaScriptのarrow構文を使用してラムダ関数を作成することもできます。
// ((inputs)=>output)
console.log((() => 4 + 4)()); //8

// 1つの入力 (input=>output)
console.log(((x) => x + 3)(4)); //7

// 複数の入力
console.log(((x, y) => x + y)(10, 15)); //25

// 複数の入力、複数の行
console.log(
  ((a, b) => {
    a = Math.pow(a, 2);
    b = Math.pow(b, 2);
    return Math.sqrt(a + b);
  })(3, 4)
); //5
```

- fizzbuzz の出力

```javascript
let fizzbuzz = (x) => {
  let bar = "-";
  let result = "";
  for (let i = 1; i <= x; i++) {
    if (i == x && i % 15 == 0) result += "FizzBuzz";
    else if (i == x && i % 3 == 0) result += "Fizz";
    else if (i == x && i % 5 == 0) result += "Buzz";
    else if (i % 15 == 0) result += "FizzBuzz" + bar;
    else if (i % 3 == 0) result += "Fizz" + bar;
    else if (i % 5 == 0) result += "Buzz" + bar;
    else result += i + bar;
  }
  return result;
};

console.log(fizzbuzz(9));
console.log(fizzbuzz(20));
```

- 高階関数

```javascript
// 呼び出し可能オブジェクトは出力として返すことができる。
function createPrintFn(word) {
  return function () {
    console.log(word);
  };
}

// 変数に格納することができる。
let callableFn = createPrintFn("Hello!");

// オブジェクトは下記の状態では実行されず、
console.log(callableFn);

// ()で実行されます。//この()ってラムダ式の仮引数がない()で実行される関数のことだったんだ。。
callableFn();

// 呼び出し可能オブジェクトは入力として渡すことができる。
function repeat(times, fn) {
  for (let i = 0; i < times; i++) {
    fn(); //"Hello!""Hello!""Hello!""Hello!"
  }
}

repeat(4, callableFn);
```

```javascript
function fSquaredX(f, x) {
  return f(x * x); //f(5*5)//f(25);
}

// f(a^2) = a^2 + 30;
console.log(
  fSquaredX(function (a) {
    return a + 30;
  }, 5)
); // 25 + 30 = 55

// 呼び出し可能オブジェクトを変数内に格納します。
let callable = function (p) {
  console.log("p is " + p);
};

fSquaredX(callable, 10); //p is 100
fSquaredX(callable, 8); //p is 64
```

```javascript
function summation(g, a, b) {
  if (b < a) return 0;
  return g(b) + summation(g, a, b - 1);
}

// 10までの総和
// 1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + 10 = 55
let identity = function (i) {
  return i;
};
console.log(summation(identity, 1, 10)); //10,9,8,7,6,5,4,3,2,1 = 55;

// 10 * 100 の計算
console.log(
  summation(
    function () {
      return 10;
    },
    1,
    100
  )
); // 100 * 10

function pPi(g, a, b) {
  if (b < a) return 1;
  return g(b) * pPi(g, a, b - 1);
}

// 10の階乗(10!)
console.log(pPi(identity, 1, 10)); //10*9*8*7*6*5*4*3*2*1//3,628,800

// 5^10 の計算
console.log(
  pPi(
    function () {
      return 5;
    },
    1,
    10
  )
);
```

- バリデーション

```javascript
//ラムダなしだと、下の関数のコピーを名前を変えて作らなければいけなくなる。
//callbackを使うと、一つの関数から複数の関数へ情報を飛ばす事ができる。
//これは仮引数を関数化することで実現している。
//なるほどな。callback == 仮引数を関数化
function emailValidation(callback, email) {
  return callback(email);
}

let doesNotStartWithAt = (email) => {
  if (email[0] == "@") return "Email is not correct.";
  else return "Email is correct.";
};

let doesNotHaveSpace = (email) => {
  for (let i = 0; i < email.length; i++) {
    if (email[i] == " ") return "Email is not correct.";
  }
  return "Email is correct.";
};

let hasUppercaseAndLowercase = (email) => {
  if (email != email.toUpperCase() && email != email.toLowerCase())
    return "Email is correct.";
  else return "Email is not correct.";
};

console.log(emailValidation(doesNotStartWithAt, "@gmail.com"));
console.log(emailValidation(doesNotStartWithAt, "kkk@gmail.com"));

console.log(emailValidation(doesNotHaveSpace, "Hello world"));
console.log(emailValidation(doesNotHaveSpace, "Helloworld"));

console.log(emailValidation(hasUppercaseAndLowercase, "hello world"));
console.log(emailValidation(hasUppercaseAndLowercase, "hello world"));
console.log(emailValidation(hasUppercaseAndLowercase, "Hello world"));
```

- コードを重複させることなく、同じ関数を複数の場所で使用できる

```javascript
function helloFunction() {
  return function () {
    return "hello world";
  };
}
// この関数は関数を返します。
console.log(helloFunction()); //[function]//function its selfを返す。
// 戻り値としてのこの関数を実行するか、保存することができます。
console.log(helloFunction()()); //"hello world"

//上のコードは[function]を返した。これは、function内のhello worldを返した。。
//これは変数から呼び出した。->hello world
//上のコードは、そのfunctionをそのまま呼んだ->[function]
let outputF = helloFunction();
console.log("Running a function that was generated...." + outputF()); //Running a function that was generated....hello world

// 数値xを取り込み、その後xと入力を乗算する関数を返します。
function constantMultiplication(x) {
  return function (y) {
    return y * x;
  };
}

let multiplyBy4 = constantMultiplication(4);
console.log(multiplyBy4(3));
//multiplyBy4(3)//constantMultiplication(4)(3)//3*4 == 12;
```

- 名前付き関数や変数に格納された関数を高階関数の入力として使用することができます。

```javascript
// nameを受け取り、文字列を返す関数
function greetingFn(name) {
  return "Hello there " + name + " from Fn.";
}

// name を受け取り文字列を返す無名関数を変数へ代入
let greetingLambdaFn = (name) => {
  return "Hello there " + name + " from Lambda.";
};

// 文字をランダムに選び、名前を作る関数
function nameGenerator() {
  return Math.random().toString(36).substring(2);
}

// 入力として関数への参照を受け取ることができます。
//(greetingFn, nameGenerator, "Thank you")
function multiCall(f, fInputF, message) {
  //greetingFn(nameGenerator()) + "....." + "Thank you";
  //greetingLambdaFn(nameGenerator)+ "......" + "Thank you";
  return f(fInputF()) + "......" + message;
}

console.log(multiCall(greetingFn, nameGenerator, "Thank you")); //hello there random name from fn, Thank you

console.log(multiCall(greetingLambdaFn, nameGenerator, "Thank you")); ////hello there random name from Lambda, Thank you
```

- 挨拶

```javascript
let loud = (name) => {
  return name.toUpperCase();
};
let quiet = (name) => {
  return name.toLowerCase();
};
let reverse = (name) => {
  return name.split("").reverse().join("");
};
let repeat = (name) => {
  return name + " " + name;
};
function whichGreeting(time) {
  if (time > 0 && time < 12) return "Good Morning ";
  else if (time >= 12 && time < 18) return "Good Afternoon ";
  else return "Good Evening ";
}
function greet(time, name, formOfName) {
  return whichGreeting(time) + formOfName(name);
}
console.log(greet(1, "John", loud));
console.log(greet(2, "John", quiet));
console.log(greet(13, "John", reverse));
console.log(greet(19, "John", repeat));
console.log(greet(13, "Leslie Emmanuel Beadon", loud));
console.log(greet(19, "Leslie Emmanuel Beadon", quiet));
console.log(greet(5, "Leslie Emmanuel Beadon", reverse));
console.log(greet(1, "Leslie Emmanuel Beadon", repeat));
```

## callback

- まさにこれだな ↓
  異なる種類のデータに対して同様の処理を行う関数を複数実装する場合、それぞれの処理に対して個別の関数を記述する必要があります。これは、コードの保守を困難にし、関数の動作方法を変更する必要がある場合にエラーを引き起こす可能性があります。

- 同期型 callback//callback 関数が終わるまで下の関数は実行されない。

```javascript
// フィボナッチ数を返す関数
function fib(n) {
  if (n === 0) return 0;
  else if (n === 1) return 1;
  else return fib(n - 1) + fib(n - 2);
}

// 2乗を返すラムダ関数
let square = function (n) {
  return n * n;
};

// 引数に関数を受け取ります。
function printFnResult(callback, n) {
  console.log("----- start -----");
  console.log("n: " + n);
  // ここで受け取った関数を実行します。
  console.log("result: " + callback(n));
  console.log("----- end -----");
}

// いずれかの方法で関数を渡します。
printFnResult(fib, 20); //start,20,fib(20),end
printFnResult(square, 10); //start,10,square(10),end
printFnResult((n) => n * n * n, 10); //start//10,
```

- 非同期 callback

```javascript
let storage = "old data";

// 指定したURLからデータを取得しますが、1秒かかります。
function getData(url) {
  setTimeout(() => {
    console.log("getting from " + url);
    storage = "data at " + url;
  }, 1000);
}

function render() {
  console.log("rendering " + storage);
}

// 最初にURLからデータを取得します。
getData("https://recursionist.io/");

// localに保存されたデータからrenderingします。
// しかし、データが更新される前にRenderしてしまいました。
// 本来はデータを取得->renderの順

render();

//rendering old data
//getting from https://recursionist.io
```

- 文字列の最大値

```javascript
let compareLength = (arr) => {
  let max = "";
  for (let i = 0; i < arr.length; i++) {
    if (max <= arr[i]) max = arr[i];
  }
  return max;
};

function maxByCriteria(f, arr) {
  return f(arr);
}
console.log(
  maxByCriteria(compareLength, [
    "apple",
    "yumberry",
    "grape",
    "banana",
    "mandarin",
  ])
);
```

- カスタム配列

```javascript
let cube = (arr) => {
  let cubedArr = [];
  for (let i = 0; i < arr.length; i++) {
    cubedArr.push(arr[i] * arr[i] * arr[i]);
  }
  return cubedArr;
};

let splitAndAdd = (arr) => {
  let splittedArr = [];

  for (let i = 0; i < arr.length; i++) {
    let numSum = 0;

    while (String(arr[i]).length) {
      numSum += Math.floor(arr[i] % 10); //3
      arr[i] = Math.floor(arr[i] / 10); //12
      console.log(numSum);
    }
    splittedArr.push(numSum);
  }
  return splittedArr;
};

function customArray(f, arr) {
  return f(arr);
}
console.log(customArray(cube, [3, 11, 24, 31]));
console.log(customArray(splitAndAdd, [3, 11, 24, 31]));
```

- カスタム配列

```javascript
function customArray(f, arr) {
  for (let i = 0; i < arr.length; i++) {
    arr[i] = f(arr[i]);
  }
  return arr;
}

function cube(n) {
  return Math.pow(n, 3);
}

function splitAndAdd(n) {
  let sum = 0;
  while (n / 10 > 0) {
    sum += n % 10;
    n = Math.floor(n / 10);
  }
  return sum;
}

console.log(customArray(cube, [3, 11, 24, 31])[0]);
console.log(customArray(cube, [3, 11, 24, 31])[1]);
console.log(customArray(cube, [1, 2, 3, 4])[3]);
console.log(customArray(splitAndAdd, [3, 11, 24, 31])[2]);
console.log(customArray(splitAndAdd, [105, 19, 912, 643])[1]);
console.log(customArray(splitAndAdd, [105, 19, 912, 643])[3]);
```

- リスト反復処理
  最後の関数 forEach は、javacript のメソッド

```javascript
function forEach(f, list) {
  for (let i = 0; i < list.length; i++) f(list[i]); //console.log(list[i]);
}

forEach((x) => console.log(x), [2, 3, 4, 5]);

// 通常のfor loop
function simpleLoop() {
  let l = [3, 4, 5, 6, 6, 10];
  let counter = 0;

  for (let i = 0; i < l.length; i++) {
    counter += l[i] * l[i];
  }

  return counter;
}

function loopDifferent() {
  let l = [3, 4, 5, 6, 6, 10];
  let counter = 0;

  let forEach = (f, list) => {
    for (let i = 0; i < list.length; i++) {
      // 親スコープ変数にアクセスすることができます。自由度はありますが、この方法でラムダを使うと副作用が出ることがあるので注意が必要です。
      // ここではfを毎度呼び出しています。fに変化があった場合、副作用が発生します。
      f(list[i]); //counter += 3*3;
    }
  };

  forEach(function (x) {
    counter += x * x;
  }, l);

  return counter;
}

console.log(simpleLoop());
console.log(loopDifferent());

// JavaScriptの配列には、この反復処理を行うforEachメソッドが付属しています。
function loopDifferentLibrary() {
  let l = [3, 4, 5, 6, 6, 10];
  let counter = 0;

  l.forEach(function (x) {
    counter += x * x;
  });

  return counter;
}

console.log(loopDifferentLibrary());
```

### map/filter(for 文がメソッド内に含まれている) arr.map(for 文を使って arr に行いたい処理,またはその変数)

- map

```javascript
function myMap(f, list) {
  let results = [];
  for (let i = 0; i < list.length; i++) results.push(f(list[i]));
  return results;
}

let nums = [1, 2, 3, 4, 5, 6, 7];
console.log(nums);
console.log(myMap((x) => x * x, nums));

// すべてのJSの配列オブジェクトには、ラムダを受け取りマッピングを返すmap関数が含まれています。
//つまり、上の関数は必要ない。
console.log(nums.map((x) => x * x)); //forを自動的にやってくれている。
// [
//   1, 2, 3, 4,
//   5, 6, 7
// ]
// [
//    1,  4,  9, 16,
//   25, 36, 49
// ]
// [
//    1,  4,  9, 16,
//   25, 36, 49
// ]
```

- filter

```javascript
function myFilter(predicateF, list) {
  let results = [];
  for (let i = 0; i < list.length; i++) {
    if (predicateF(list[i]) === true) results.push(list[i]);
  }

  return results;
}

let list1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(myFilter((x) => x % 2 !== 0, list1));

// すべてのJS配列オブジェクトには、この処理を行うフィルタ関数が含まれています。
console.log(list1.filter((x) => x % 2 !== 0));

// [ 1, 3, 5, 7, 9 ]
// [ 1, 3, 5, 7, 9 ]
```

- 年齢チェック

```javascript
function ageCheck(ages) {
  return ages.filter((x) => x >= 18);
}
console.log(ageCheck([3, 11, 24, 31]));
```

### reduce
- reduce() 関数は map() や filter() 関数よりも少し複雑で、関数、シーケンス、初期値という 3 つの引数を受け取ります。シーケンスの最初の 2 つの要素に関数を適用し、その演算結果とシーケンスの 3 番目の要素に関数を適用し、といった具合にシーケンスの最後に達するまで繰り返します。初期値が指定された場合は、その値が削減の開始値として使われます。


コールバックは、累積値を処理し、次の要素に対して、新たな累積値を返す必要があることに注意してください。reduce 関数を作成して、階乗の処理を実行してみましょう。
```javascript
function myReduce(reduceCallback, list, initial) {
  let lastResult = initial; //1
  for (let i = 0; i < list.length; i++) {
    let result = reduceCallback(list[i], lastResult);
    lastResult = result;
  }
  return lastResult;
}

let list1 = [1, 2, 3];
let list2 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

//3!
//1*1 ->1
//1*2 ->2
//2*3 ->6
console.log(myReduce((x, total) => x * total, list1, 1));

//10!
console.log(myReduce((x, total) => x * total, list2, 1));

// すべてのJS配列オブジェクトには、この処理を行うreduce関数が含まれています。
// 初期値が与えられていない場合は、最初の要素の値を初期値とし、2番目の要素から反復を開始します。
// reduce(累積値, 現在の値)
console.log(list2.reduce((total, x) => total * x));
console.log(list2.reduce((total, x) => total * x, 1));

// reduce関数を使って、1からnまでの総和を作成してください。
```

- reduce はニ次元配列を一次元配列にすることもできる

```javascript
let array2d = [
  [2, 3, 4, 5],
  [5, 22, 34, 4, 5],
  [12, 13, 45, 67, 84],
];

// すべてのJS配列オブジェクトには、この処理を行うreduce関数が含まれています。
// 初期値が与えられていない場合は、最初の要素の値を初期値とし、2番目の要素から反復を開始します。
// reduce(累積値, 現在の値)

// 平坦化して1次元配列にします。
let flatten = array2d.reduce((flattenList, list) => flattenList.concat(list));

console.log(flatten);
console.log(flatten[1]);
//[
//    2,  3, 4,  5,  5, 22,
//   34,  4, 5, 12, 13, 45,
//   67, 84
// ]

// 3
```

## ラムダの出力

```javascript
// この関数は通常の関数として定義すればよいので、これは無意味です。
// どのような入力であっても、常に同じ関数が返されます。
function lambdaHelloWorld(randomInput) {
  console.log(
    randomInput +
      " was passed in but this function always returns the same lambda function"
  );
  return function () {
    return "Hello World";
  };
}

function helloWorld() {
  return "Hello World";
}

console.log(helloWorld());
console.log(lambdaHelloWorld("lalilulelo")());

// Hello World
// lalilulelo was passed in but this function always returns the same lambda function
// Hello World
```

### currying(カリー化)

- 複数の引数を取る関数を、それぞれ単一の引数を取る一連の関数に変換する手法のことです。これは、より特化した関数や再利用可能な関数を作成するのに便利で、関数型プログラミングでは一般的な手法です。

```javascript
// 引数を2つもつ通常の関数をカリー化します
function addFn(x, y) {
  return x + y;
}
console.log(addFn(1, 2));

// 関数を2つ使いカリー化します。
function addCarry(x) {
  return function (y) {
    return x + y;
  };
}
console.log(addCarry(1)(2));

// lambda関数、特にアロー関数だと簡潔に記述可能です。//上とイコール
let addCarryLambda = (x) => (y) => x + y;
console.log(addCarryLambda(1)(2));
```

### 部分適応: 3 つ仮引数があり、そのうち 2 つはずっと同じデータを使う場合、その 2 つを固定する。== 関数の共通化

- 部分適用とは、複数の引数を取る関数のうち、一部の引数をデフォルト値に固定し、より少ない引数を取る新しい関数を作成することである。これにより、関数が定義された時点で引数の一部を指定し、関数が呼び出された時点で、返された関数を使用して残りの引数を指定することができます。

```javascript
// ドルを日本円に両替するとき手数料が課されます。
// 手数料を含めて両替にかかる料金を計算する関数は以下のようになります。
function usdTojpy(USDJPY, paymentUSD, handlingCharge) {
  return USDJPY * paymentUSD + handlingCharge;
}

// 同じレート(USDJPY)、手数料(handlingCharge)なのに毎回記述するのは冗長です。
console.log(usdTojpy(139.85, 200, 3000));
console.log(usdTojpy(139.85, 32, 3000));

// 関数の部分適用は、ラムダ出力で実現できます。
// 部分適用すると、関数の一部を適用した新しい関数を作ることができます。
let usdTojpyVisaNov17th = function (paymentUSD) {
  return usdTojpy(139.85, paymentUSD, 3000);
};
console.log(usdTojpyVisaNov17th(200));
console.log(usdTojpyVisaNov17th(32));

// カリー化するとさらに簡潔に記述できます。
let usdTojpyCarry = function (handlingCharge) {
  //毎回同じ
  return function (USDJPY) {
    //毎回同じ
    return function (paymentUSD) {
      return USDJPY * paymentUSD + handlingCharge;
    };
  };
};

let usdTojpyVisaNov17thCarry = usdTojpyCarry(3000)(139.85);
console.log(usdTojpyVisaNov17thCarry(200));
console.log(usdTojpyVisaNov17thCarry(32));

// アロー関数を使用するとさらに簡潔に記述できます。
let usdTojpyCarryArrow = (handlingCharge) => (USDJPY) => (paymentUSD) =>
  USDJPY * paymentUSD + handlingCharge;
let usdTojpyVisaNov17thCarryArrow = usdTojpyCarryArrow(3000)(139.85);

console.log(usdTojpyVisaNov17thCarryArrow(200));
console.log(usdTojpyVisaNov17thCarryArrow(32));
```

### currying と部分適応 //関数の共通化の更なる例

```javascript
const customerList = [
  {
    id: 1,
    company: "MH Corp.",
    name: "Makenzie Hibbert",
    rank: "A",
    email: "makenzie@example.com",
  },
  {
    id: 2,
    company: "MH Corp.",
    name: "Abram Martinho Fleming",
    rank: "B",
    email: "abram@example.com",
  },
  {
    id: 3,
    company: "Best Inc.",
    name: "Trey Best",
    rank: "A",
    email: "trey@example.com",
  },
  {
    id: 4,
    company: "Best Inc.",
    name: "Joshua Charnley",
    rank: "B",
    email: "joshua@example.com",
  },
  {
    id: 5,
    company: "Best Inc.",
    name: "Sue Rodger",
    rank: "C",
    email: "sue@example.com",
  },
];

// rankAの顧客を抽出したいとき、下記のようにフィルター関数を使用すると該当データが抽出ができます。
//listとforを使わなくてもいい。
//////////再利用性が無い/////////////////
let rankAList = customerList.filter((customer) => customer.rank === "A");
console.log(rankAList);
////////再利用性がある///////////////
//引数にListを参照し、filter関数をラップすることで対応が可能です。
let extractionByRank = (list, rank) => {
  return list.filter((customer) => customer.rank !== rank);
};
console.log(extractionByRank(customerList, "A"));

// 上記関数に独自のFilter条件を加えたい場合、カリー化を使用すると関数の責務を分離し、再利用性も向上します。
let filterByRank = (rank) => (personal) => personal.rank === rank;
let filterByCompany = (company) => (personal) => personal.company === company;

let versatileExtraction = (list, filterCriteria, value) => {
  return list.filter(filterCriteria(value));
};
console.log(versatileExtraction(customerList, filterByCompany, "MH Corp."));
console.log(versatileExtraction(customerList, filterByRank, "A"));
```

- lambda machine

```javascript
class LambdaMachine {
  constructor() {
    this.handler = {};
  }
  //hashmapに格納
  insert(stringLambda, lambda) {
    this.handler[stringLambda] = lambda;
  }
  //hashmap[key]に該当する関数に飛ばす。
  retrieve(stringLambda) {
    return this.handler[stringLambda];
  }
}

function pythagora(x, y) {
  return Math.sqrt(x * x + y * y);
}
//let pythagora = function(x,y){return Math.sqrt(x*x + y*y)};
let addition = function (x, y) {
  return x + y;
};
let multiplication = function (x, y) {
  return x * y;
};

lambdaMachine = new LambdaMachine();
//functionではなく変数でないと、handler[p]pは呼ばれない?
//いや試したけど、どちらも正しく呼ばれた。。pyをclass内に書いたから
//なぜpyをclass内に書いたらエラーになるのか。。
lambdaMachine.insert("pythagora", pythagora);
console.log(lambdaMachine.retrieve("pythagora")(3, 4)); //5
```

# わからん

- ラウンドロビン

```javascript
//ラムダ関数を格納した配列を管理する lambdaStorage という配列を追加し、その配列からラムダ関数を順番に取り出すことで実現します。
class LambdaMachine{
    constructor(){
        this.handler = {};
    }
    //hashmapに格納
    insert(string, f){
        this.handler[s] = f;
    }
    //hashmap[key]に該当する関数に飛ばす。
    // retrieve(s){
    //     return this.handler[s];
    // }
}

let roundRobinRetrive(){
  return function(a,b){

  }
}

// lambdaMachine = new LambdaMachine()
// lambdaMachine.insert("pythagora", pythagora)
// lambdaMachine.insert("addition", addition)
// lambdaMachine.insert("multiplication", multiplication)

//keyに関数名の文字列を渡さずにどうやって分かるんだ？zzzz
// lambdaMachine.roundRobinRetrieve()(6, 8) --> 10//py
// lambdaMachine.roundRobinRetrieve()(6, 8) --> 14//add
// lambdaMachine.roundRobinRetrieve()(6, 8) --> 48//mul
// lambdaMachine.roundRobinRetrieve()(6, 8) --> 10//py
```

## lambda closure

- 関数内から別の関数を呼び出して返ってきた後もその外側の関数にアクセスできる関数の事を指す。何が便利かというと、とりあえずコードを見てみよう。private,非同期に便利とか言ってるけど、、とりあえずコードを見てみよう。

```javascript
function counterFn() {
  let count = 0;

  function increase() {
    // 外部の変数にアクセスできます。
    count++;
    return count;
  }
  // inner関数を返します。
  return increase;
}

// inner関数がグローバル変数に格納されます。
let counter = counterFn();

// これにより、counterが存在する限りオブジェクトが保持されます。
// そのため、ローカル変数countの値が保持されてます。
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
console.log(counter()); // 4
```

- なるほど、、counter の値の情報を保持できている。。だから何だ?

- 下の getValue がクロージャー

```javascript
function myFun() {
  const value = 5;

  return {
    //closure(クロージャー)↓//hashmapのkeyなの??
    getValue: function () {
      return value;
    },
  };
}

const v = myFun();
//myFun.getValue()
console.log(v.getValue()); // 5
```

- module パターン/inner 関数で実装

```javascript
function closure() {
  // 関数外部から直接pivateValueにアクセスはできません。
  let privateValue = "private";

  // inner関数を経由することで取得できます。
  function getPrivateValue() {
    return privateValue;
  }

  // 内部privateValueの値を取得するための関数を返し、データの出入り口を作ります。
  return getPrivateValue;
}

let closureFn = closure();
////////////確かに今ままで通りに考えればそうやな/////////////////
// 直接はclosure内の変数にアクセスできない
console.log(closureFn.privateValue);

// closureFnから返ってきた関数を実行することで、closure内の変数にアクセスできる
console.log(closureFn());
//////////////////////////////////////////////////////

// さらに、関数内の変数を取得・編集するinner関数を作成し、
// hashMapなどで複数の関数を返すことにより、関数内の変数・関数へのアクセスを制御することができます。
function createObject() {
  // 関数外部から直接privateValueにアクセスできませんが、
  // 外部に公開したinner関数経由で編集が可能です。
  let privateValue = "private";
  // inner関数を経由することで編集できます。
  function setPrivateValue(newValue) {
    accessAlert();
    privateValue = newValue;
    //newValueの変数には"newValue"が入っている。
  }
  // inner関数を経由することで取得できます。
  function getPrivateValue() {
    return privateValue;
  }

  // inner関数も外部から隠蔽することができます。
  function accessAlert() {
    console.log("data changed!");
  }

  // hashMapを使用し、複数の関数を返すことも可能です。
  return {
    setValue: setPrivateValue,
    getValue: getPrivateValue,
  };
}

let newObj = createObject();

// hashMapのキーgetValueから呼び出した関数を実行します。
console.log(newObj.getValue()); //"private"
//createObject.setPrivateValue
newObj.setValue("newValue"); //"data changed!"
console.log(newObj.getValue()); //"new value"

// また、accessAlert()は外部からは呼べません。
// newObj.accessAlert(); -> error
```

- 上の module パターン inner 関数で実装したのを、lambda でもできる。↓
- これは lambda closure と呼ばれる
  ラムダクロージャとは、ラムダ式によって作成された、変数の状態をラムダのスコープにバインドするステートフル関数のことを指します。ラムダ式がスコープ外の変数を含む場合、これらの変数とそのデータは作成された関数オブジェクトにカプセル化されます。
  関数の呼び出しが終わった後、ローカル変数がコールスタックからポップされるときのように、元の変数がメモリからクリアされたとしても、その変数の状態はラムダ関数の中に残ります。それはラムダ関数の中に刷り込まれているためです。
  ↓
- 外部の変数がクリアされたとしても、その内部の関数に、その変数のデータが残り続ける、

```javascript
const federalTaxes = 0.2;
//典型的なlambda
function taxLambda(stateTax, state) {
  return function (income) {
    // global変数を含むスコープ外の変数にアクセスが可能です。
    let taxes = federalTaxes + stateTax;
    console.log("Computing taxes for state..." + state);
    return income - taxes * income;
  };
}

// taxLambdaで生成されたラムダ関数がグローバル変数にバインドされます。
let californiaF = taxLambda(0.0725, "California");
let texasF = taxLambda(0.0625, "Texas");
let hawaiiF = taxLambda(0.04, "Hawaii");

//典型的なlambda
// incomeを引数にそれぞれ設定した税率で税金を計算できます。
let income = 40000;
console.log("Calculating income using lambdas");
console.log(californiaF(income)); //taxLambda(0.0725, "California")(40000);
console.log(texasF(income));
console.log(hawaiiF(income));

// このように再利用性が向上します。//lambda closureとlambdaって何が違うん??外部にアクセスできるって事??
let income2 = 500000;
console.log("------Calculating more income using lambdas------");
console.log(californiaF(income2));
console.log(texasF(income2));
console.log(hawaiiF(income2));
```

- タスクリスト

```javascript
function todoCall(arr) {
  let i = 0;
  return function () {
    i++;
    if (i - 1 == arr.length) return "All done!";
    return arr[i - 1];
  };
}

let todoCaller = todoCall(["Read a Book", "Work out", "Recursion"]);
//todoCall(arr)()
console.log(todoCaller()); // Read a Book
console.log(todoCaller()); // Work out
console.log(todoCaller()); // Recursion
console.log(todoCaller()); // All done!
```

### デコレータパターン(デザインパターンの中のデコレータパターン)

既にあるオブジェクトに高階関数、ラムダを使って新しいコードを追加すること
↓
コードを修正する事なくコードを追加することができることがメリット。
コードの構造を変えなくて済むから便利
何か機能を追加したいとき、関数 f を変えなくて済む. 内部だけをラムダを使って変えればいい。

```javascript
//既存の関数に新しい振る舞いを追加するデコレータパターン
//関数 f を引数に取り、元の f 関数をラップした新しい関数を返します
function simpleDecorator(f) {
  return function () {
    console.log("Running f......");
    return f; //仮引数を返している。
  };
}

function helloWorld() {
  return "Hello world";
}
let newFunc1 = simpleDecorator(helloWorld);
//simpleDecorator(helloWorld)();
console.log(newFunc1()); //"Running f......"/"Hello world"ではなく[function: helloWorld]

let newFunc2 = simpleDecorator(() => "Hello Jupiter");
//simpleDecorator(()=>"Hello Jupiter")();
console.log(newFunc2()); //"Running f......"/"Hello Jupiter"ではなく[function]
```

- デコレータ

```javascript
//"配列の合計値を返す sumOfArray関数"を受け取り、配列の各要素が 10 未満であればその個数とエラーメッセージを返し、全て条件を満たしていれば sumOfArray の結果を返す validateDecorator 関数を定義し、テストケースを出力するプログラムを作成してくださいなお、validateDecorator 関数は、デコレータとして利用されるためのラッパー関数で、既存の sumOfArray 関数を受け取り、新しい機能である「エラーメッセージの出力」を付加してください。

//sumOfArrayって何?仮引数定義されてないじゃん。。
function validateDecorator(sumOfArray) {
  //sumOfArray = sumOfArray.filter();
  return function (arr) {
    //要素が10未満->その個数+message;
    //要素全て10以上->message+合計値
    for (let i = 0; i < arr.length; i++) {
      //10以下だった場合
      if (arr[i] <= 10) count++;
      //10以上だった場合
      else sum = sum + arr[i];
    }
  };
}

sum = validateDecorator(sumOfArray);
//validateDecorator(sumOfArray)([10,20,30,40]);
console.log(sum([10, 20, 30, 40])); //Sum of array is 100
console.log(sum([9, 10, 20, 30])); //1 error found
console.log(sum([3, 5, 40, 50])); //2 error found
```

//////////////////////////////////javascript2 週目 ↓ /////////////////////////////////////////////////
//////////////////////////////////javascript2 週目 ↓ ///////////////////////////////////////////////////////////////////////////////////
//////////////////////////////////javascript2 週目 ↓ /////////////////////////////////////////////////////////////////////////////////////
//////////////////////////////////javascript2 週目 ↓ /////////////////////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////////////////////////////

//////////////////////////////////javascript2 週目 ↓ /////////////////////////////////////////////////
//////////////////////////////////javascript2 週目 ↓ ///////////////////////////////////////////////////////////////////////////////////
//////////////////////////////////javascript2 週目 ↓ /////////////////////////////////////////////////////////////////////////////////////
//////////////////////////////////javascript2 週目 ↓ /////////////////////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////////////////////////////

```javascript
// ①通常の関数
function asc(x) {
  return x + 1;
}
console.log(asc(1));

// ②無名関数
let ascLambda1 = function (x) {
  return x + 1;
};
console.log(ascLambda1(1));

// アロー関数で表すとさらに簡潔に記述できます。
let ascLambda2 = (x) => {
  return x + 1;
};
console.log(ascLambda2(1));
```

```javascript
//参照なし->ガベージコレクタ
console.log(function (x, y) {
  return x + y;
});
//上のコードはすでにガベージコレクタ。もう一度新しい関数を書く必要がある。
console.log(
  (function (x, y) {
    return x + y;
  })(15, 35)
); //50
// 呼び出しがされた後->削除される　なので、もう一度作り直す必要がある。
// 今回は、変数に格納して呼び出してみましょう。呼び出し可能オブジェクトはオブジェクトなのですから。
let myCallable = function (x, y) {
  return x + y;
};
console.log(myCallable(3, 5)); //8
console.log(myCallable(10, 10)); //20
console.log(myCallable(150, 5)); //155
console.log(myCallable);

// では、ラムダ式を使ってその場で関数を作成してみましょう。呼び出し可能オブジェクトの参照が返されるので、データを返す呼び出しを行うことができます。見ての通り、名前がないので匿名関数と呼ばれています。
console.log(function () {
  return "A new world";
});
console.log(
  (function () {
    return "A new world";
  })()
);

// ラムダ式は呼び出し可能オブジェクトを返します。
// これらの呼び出し可能オブジェクトを呼び出すと、匿名関数の戻り値と同じように評価される式になります。
console.log(
  (function () {
    return 4 + 5;
  })()
); //9

// 匿名関数スコープ外の変数にアクセスします。
let p = 40;
console.log(
  (function () {
    return p + 10;
  })()
); //50
console.log(
  (function () {
    return "P is " + p;
  })() + " ......"
); // P is 40.....

// 特定の入力を受け取る匿名関数を作成することができます。通常の関数と同じように、呼び出すときに入力を渡すことができます。
console.log(
  "squaring..." +
    (function (x) {
      return x * x;
    })(4)
); //squaring...16

// 匿名関数は、他のすべての関数と同様に、呼び出されたときに独自のローカルスコープを作成します。
console.log(
  "looping..." +
    (function (x) {
      let sheeps = "";
      for (let i = 1; i <= x; i++) sheeps += i + "sheep~";
      return sheeps;
    })(5)
); //looping...1sheep~2sheep~3sheep~4sheep~5sheep~

// また、JavaScriptのarrow構文を使用してラムダ関数を作成することもできます。
// ((inputs)=>output)
console.log((() => 4 + 4)()); //8

// 1つの入力 (input=>output)
console.log(((x) => x + 3)(4)); //7

// 複数の入力
console.log(((x, y) => x + y)(10, 15)); //25

// 複数の入力、複数の行
console.log(
  ((a, b) => {
    a = Math.pow(a, 2);
    b = Math.pow(b, 2);
    return Math.sqrt(a + b);
  })(3, 4)
); //5
```

- ゼロの増殖(ラムダ関数)

```javascript
//通常
// function duplicateZero(num){
//   let result = "";
//   for(let i=0; i<num; i++){
//     result += "0";
//   }
//   return result;
// }

//ラムダ
let duplicateZero = (num) => {
  let result = "";
  for (let i = 0; i < num; i++) {
    result += "0";
  }
  return result;
};
console.log(duplicateZero(5));
console.log(duplicateZero(10));
//duplicateZero(5) --> 00000
```

- なるほどなー

```javascript
// 呼び出し可能オブジェクトは出力として返すことができる。
function createPrintFn(word) {
  return function () {
    console.log(word);
  };
}

// 変数に格納することができる。
let callableFn = createPrintFn("Hello!");

// オブジェクトは下記の状態では実行されず、
console.log(callableFn); //オブジェクトは後に()は付けなくても実行される
callableFn(); //これは()で実行//関数ではなく、オブジェクトを呼び出している

// 呼び出し可能オブジェクトは入力として渡すことができる。
function repeat(times, fn) {
  for (let i = 0; i < times; i++) {
    fn();
  }
}

repeat(4, callableFn);
```

```javascript
// 関数を入力として受け取り、関数を出力として返す関数
function createHighOrderFn(word, fn) {
  return function () {
    return fn() + word; //step0Fn+"step1: "
  };
}

// 入力に渡される関数
let step0Fn = function () {
  return "step0: ";
};

// 出力された関数を変数へ代入。続けて入力へ渡すことができます
let step1Fn = createHighOrderFn("step1: ", step0Fn);
let step2Fn = createHighOrderFn("step2: ", step1Fn);

console.log(step0Fn()); //step0:
console.log(step1Fn()); //step0: step1:
//上のstep1Fn()の実行結果がこのコードに反映されている。
console.log(step2Fn()); //step0: step1:
```

```javascript
function fSquaredX(f, x) {
  //(function(a){ return a + 30}(5*5))//return 25+30;
  return f(x * x); //f(5*5)//f(25);
}

// f(a^2) = a^2 + 30;
console.log(
  fSquaredX(function (a) {
    return a + 30;
  }, 5)
); // 25 + 30 = 55

// 呼び出し可能オブジェクトを変数内に格納します。
let callable = function (p) {
  console.log("p is " + p);
};
fSquaredX(callable, 10); //(function(p){console.log("p is " + p)};,(100))//p is 100
```

- 再帰とラムダ
  高階関数: 関数を引数や戻り値とする関数(ラムダ)
  高階関数を使うと、関数を分解して表現できる。利点: 何か似たような再帰が必要になった時に、g(b)のところを変えるだけで再利用が可能になる。(再利用性が高い)また、g(b)の関数にバグがあった場合、再帰と g(b)を分けて考えることができるので、デバックが容易になる。

```javascript
function summation(g, a, b) {
  //(function(i){return i},1,10);
  if (b < a) return 0; //10<1
  return g(b) + summation(g, a, b - 1); //function(i){return i}(10) + summation(省略, 10-1)
}

// 10までの総和
// 1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + 10 = 55
let identity = function (i) {
  return i;
};
console.log(summation(identity, 1, 10));
```

- ラムダ総和

```javascript
let count = 0;
function summation(fn, num) {
  if (num == 0) {
    //countが更新されなかったから。
    result = count;
    count = 0;
    return result;
  }
  if (fn(num)) count += num;
  return summation(fn, num - 1);
}
let isOdd = function (num) {
  return num % 2 !== 0;
};
let isMultipleOf3Or5 = function (num) {
  return num % 3 === 0 || num % 5 === 0;
};
let isPrime = function (num) {
  for (let i = 2; i < num; i++) {
    if (num == 1 || num == 0) return false;
    if (num % i == 0) return false;
  }
  return true;
};
console.log(summation(isOdd, 3));
console.log(summation(isOdd, 10));
console.log(summation(isMultipleOf3Or5, 3));
console.log(summation(isMultipleOf3Or5, 10));
console.log(summation(isMultipleOf3Or5, 100));
console.log(summation(isPrime, 2));
console.log(summation(isPrime, 10));
console.log(summation(isPrime, 100));
```

```javascript
function helloFunction() {
  return function () {
    return "hello world";
  };
}
// この関数は関数を返します。
console.log(helloFunction()); //[function]//function its selfを返す。
// 戻り値としてのこの関数を実行するか、保存することができます。
console.log(helloFunction()()); //"hello world"

let outputF = helloFunction();
console.log("Running a function that was generated...." + outputF()); //"hello world"

// 数値xを取り込み、その後xと入力を乗算する関数を返します。
function constantMultiplication(x) {
  return function (y) {
    return y * x;
  };
}

let multiplyBy4 = constantMultiplication(4);
console.log(multiplyBy4(3));
//multiplyBy4(3)//constantMultiplication(4)(3)//3*4 == 12;                                        //3*4 = 12
```

- 名前付き関数や変数に格納された関数を高階関数の入力として使用することができます。同じ関数を複数の場所で再利用したい場合に便利

```javascript
let loud = (name) => {
  return name.toUpperCase();
};
let quiet = (name) => {
  return name.toLowerCase();
};
let reverse = (name) => {
  return name.split("").reverse().join("");
};
let repeat = (name) => {
  return name + " " + name;
};
function greetFn(time) {
  if (time > 0 && time < 12) return "Good Morning ";
  else if (time >= 12 && time < 18) return "Good Afternoon ";
  else return "Good Evening ";
}
function greet(time, name, formOfName) {
  return greetFn(time) + formOfName(name);
}
console.log(greet(1, "John", loud));
console.log(greet(2, "John", quiet));
console.log(greet(13, "John", reverse));
console.log(greet(19, "John", repeat));
console.log(greet(13, "Leslie Emmanuel Beadon", loud));
console.log(greet(19, "Leslie Emmanuel Beadon", quiet));
console.log(greet(5, "Leslie Emmanuel Beadon", reverse));
console.log(greet(1, "Leslie Emmanuel Beadon", repeat));

// greet(1, "John", loud) --> Good Morning JOHN
// greet(2, "John", quiet) --> Good Morning john
// greet(13, "John", reverse) --> Good Afternoon nhoJ
// greet(19, "John", repeat) --> Good Evening John John
// greet(13, "Leslie Emmanuel Beadon", loud) --> Good Afternoon LESLIE EMMANUEL BEADON
// greet(19, "Leslie Emmanuel Beadon", quiet) --> Good Evening leslie emmanuel beadon
// greet(5, "Leslie Emmanuel Beadon", reverse) --> Good Morning nodaeB leunammE eilseL
// greet(1, "Leslie Emmanuel Beadon", repeat) --> Good Morning Leslie Emmanuel Beadon Leslie Emmanuel Beadon
```

### call back

```javascript
// 2乗を返すラムダ関数
let square = function (n) {
  return n * n;
};

// 引数に関数を受け取ります。
function printFnResult(callback, n) {
  console.log("----- start -----");
  console.log("n: " + n);
  // ここで受け取った関数を実行します。
  console.log("result: " + callback(n));
  console.log("----- end -----");
}

// いずれかの方法で関数を渡します。
printFnResult(square, 10); //start,10,square(10),end
printFnResult((n) => n * n * n, 10); //start//10,
```

- 過半数

```javascript
function isOdd(n) {
  if (n % 2 !== 0) return true;
  else return false;
}
function isEven(n) {
  if (n % 2 === 0) return true;
  else return false;
}
function majority(callback, arr) {
  let trueCount = 0;
  for (let i = 0; i < arr.length; i++) {
    if (callback(arr[i]) == true) trueCount++;
  }
  if (Math.floor(arr.length / 2) < trueCount) return true;
  else return false;
}
console.log(majority(isOdd, [1, 2, 3, 4, 5]));
console.log(majority(isOdd, [2, 4, 6, 7, 8]));
console.log(majority(isEven, [3, 6, 8, 12, 15]));
console.log(majority(isEven, [4, 5, 7, 11, 14]));
```

###　リスト反復処理

- リストとラムダ式を一緒に使うことで、list の要素事ごとに関数を適用することができます。

- list の 2 乗を返す関数を違うパターンで。

```javascript
function forEach(f, list) {
  for (let i = 0; i < list.length; i++) f(list[i]); //console.log(list[i]);
}

forEach((x) => console.log(x), [2, 3, 4, 5]);

// 通常のfor loop
function simpleLoop() {
  let l = [3, 4, 5, 6, 6, 10];
  let counter = 0;

  for (let i = 0; i < l.length; i++) {
    counter += l[i] * l[i];
  }

  return counter;
}

//ラムダだと、役割を分けて色々できると言ったよね。
function loopDifferent() {
  let l = [3, 4, 5, 6, 6, 10];
  let counter = 0;
  //反復する関数
  let forEach = (f, list) => {
    for (let i = 0; i < list.length; i++) {
      // 親スコープ変数にアクセスすることができます。自由度はありますが、この方法でラムダを使うと副作用が出ることがあるので注意が必要です。
      // ここではfを毎度呼び出しています。fに変化があった場合、副作用が発生します。
      f(list[i]);
    }
  };
  //2乗する関数
  forEach(function (x) {
    counter += x * x;
  }, l);

  return counter;
}

console.log(simpleLoop());
console.log(loopDifferent());

// JavaScriptの配列には、この反復処理を行うforEachメソッドが付属しています。
//めっちゃ簡単になった。
function loopDifferentLibrary() {
  let l = [3, 4, 5, 6, 6, 10];
  let counter = 0;
  //forEach内に反復の関数が入っている。
  l.forEach(function (x) {
    counter += x * x;
  });

  return counter;
}

console.log(loopDifferentLibrary());
```

#### map/filter

```javascript
function myMap(f, list) {
  let results = [];
  for (let i = 0; i < list.length; i++) results.push(f(list[i]));
  return results;
}

let nums = [1, 2, 3, 4, 5, 6, 7];
console.log(nums); //[1,2,3,4,5,6,7]
console.log(myMap((x) => x * x, nums)); //[1,4,9,16,25,36,49]

// すべてのJSの配列オブジェクトには、ラムダを受け取りマッピングを返すmap関数が含まれています。
//つまり、上の関数は必要ない。
//listにmap関数を適応するだけで、"ラムダ内にfor文を実装"をやってくれている。仮引数に実装したいメソッドを書けばいいだけ。
console.log(nums.map((x) => x * x)); //[1,4,9,16,25,36,49]
// [
//    1,  4,  9, 16,
//   25, 36, 49
// ]
// [
//    1,  4,  9, 16,
//   25, 36, 49
// ]
```

- 文字のスワップ(map を使う)

```javascript
function swapCase(charList) {
  let result = [];
  charList.map((x) =>
    x == x.toUpperCase()
      ? result.push(x.toLowerCase())
      : result.push(x.toUpperCase())
  );
  return result;
}
```

#### filter(map と違い、全てではなく引数に該当する要素のみを出力する)

```javascript
function myFilter(predicateF, list) {
  let results = [];
  for (let i = 0; i < list.length; i++) {
    if (predicateF(list[i]) === true) results.push(list[i]);
  }

  return results;
}

let list1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
//奇数だったら、
console.log(myFilter((x) => x % 2 !== 0, list1));

// すべてのJS配列オブジェクトには、この処理を行うフィルタ関数が含まれています。
//え、mapと同じじゃね?
console.log(list1.filter((x) => x % 2 !== 0));
```

- 距離リスト(map,filter 組み合わせ)

```javascript
function over100m(l) {
  let mArr = [];
  //全てmに合わせる
  //string->Number
  l.map((x) => {
    //mmだったら                           xをstring->numberへ//後ろの2文字消す-2
    if (x[x.length - 2] == "m") mArr.push(Number(x.slice(0,-2)) * 1000);
    //kmだったら
    if (x[x.length - 2] == "k") mArr.push(Number(x.slice(0,-2)) / 1000);
    //m//1文字消す
    else mArr.push(Number(x.slice(0,-1)));
  });
  console.log(mArr);//[ 111, 2222222000, NaN, 0.003, 4 ]//なぜ配列が増えている???

  //比較するために"m"を削除して、数値にする
  //mArr.map((x) => Number(x.slice(0, -1)));
  //100以上を返す
  mArr.map((x) => x > 100);
  return mArr;
}
//console.log(over100m(["111m","2222222mm","3km","4m"]));//
```
#### reduce//for文の中に"=>(この条件)"が++する感じ
- この関数の何がreduceなん???なんか累乗みたいになっていっているけど、、
```javascript
               //(x,total)=>x*total, list1, 1)
function myReduce(reduceCallback, list, initial){
   let lastResult = initial;//1
   for(let i = 0; i < list.length; i++){
      //(x,total)=>x*total//list1[i]*lastResult//1*1//2*1//3*2
      let result = reduceCallback(list[i], lastResult);
      lastResult = result;//1//2//6
   }
   return lastResult;
}

let list1 = [1,2,3];
let list2 = [1,2,3,4,5,6,7,8,9,10];

//3!//←やってること
//1*1 ->1
//1*2 ->2
//2*3 ->6
//reduceの仮引数はどこで定義されているの?^ 
console.log(myReduce((x,total)=>x*total, list1, 1));//6
//10!//←やってること
console.log(myReduce((x,total)=>x*total, list2, 1));
// すべてのJS配列オブジェクトには、この処理を行うreduce関数が含まれています。
// 初期値が与えられていない場合は、最初の要素の値を初期値とし、2番目の要素から反復を開始します。
// reduce(累積値, 現在の値)
                         
console.log(list2.reduce((total,x)=>total*x));//3628800
console.log(list2.reduce((total,x)=>total*x, 1));//3628800
// reduce関数を使って、1からnまでの総和を作成してください。
```
- 再びreduce関数の例
```javascript
let list1 = ["hello","world","and","hello","jupiter"];
// reduce(累積値, 現在の値)
//for文の中に仮引数として入れた条件が++する感じ
console.log(list1.reduce((totalStr, currStr)=>totalStr+ ", " + currStr));

// JSには、すべての配列を1つの文字列に結合するためのjoin関数も用意されています。
console.log(list1.join(", "));

// joinの逆であるsplitは区切りを取り、区切りに基づいて文字列を配列要素に分割します。
console.log(list1.join(", ").split(", "));
// hello, world, and, hello, jupiter
// hello, world, and, hello, jupiter
// [ 'hello', 'world', 'and', 'hello', 'jupiter' ]
```
- 更にredude.　二次関数を一次関数へ
```javascript
let array2d = [[2,3,4,5],[5,22,34,4,5],[12,13,45,67,84]];

// 平坦化して1次元配列にします。
let flatten = array2d.reduce((flattenList,list)=>flattenList.concat(list));

console.log(flatten);
console.log(flatten[1]);
// [
//    2,  3, 4,  5,  5, 22,
//   34,  4, 5, 12, 13, 45,
//   67, 84
// ]
// 3
``` 
- 複利計算(reduce関数を使う)
```javascript
function calculateFinalMoney(interests,capital){
    // 関数を完成させてください
}
//初期値が100円で年利が0.3%,0.5%、、、と変化していく
100 * (1 + 0.03) * (1 + 0.05) * (1 + 0.02) * (1 + 0.04) = 120 円
console.log(calculateFinalMoney([3,5,2,5,4],100));//120
```
```javascript
function calculateFinalMoney(interests,capital){
    interests.reduce(total,x)=>total*(1+(x/100));
    return 100*total;
}
console.log(calculateFinalMoney([3,5,2,5,4],100));//120
```





























### ラムダ出力
```javascript
// この関数は通常の関数として定義すればよいので、これは無意味です。
// どのような入力であっても、常に同じ関数が返されます。
function lambdaHelloWorld(randomInput){
    console.log(randomInput + " was passed in but this function always returns the same lambda function");
    return function(){ return "Hello World";};
}

function helloWorld(){
    return "Hello World";
}

console.log(helloWorld());
console.log(lambdaHelloWorld("lalilulelo")());
// Hello World
// lalilulelo was passed in but this function always returns the same lambda function
// Hello World
```


- curry化(ラムダ出力)//複数の引数を取る関数を、それぞれ単一の引数を取る関数に値を渡す
```javascript
//関数を使ったcurry化
function addCarry (x) {
   return function (y) {
      return x + y;
   }
}
console.log(addCarry(1)(2));//1+2//3
 
// ラムダ関数のアロー関数を使ったcurry化
let addCarryLambda = x => y => x + y;
console.log(addCarryLambda(1)(2));//1+2//3
```
- ラムダ出力(curry化の部分適応, 特定の仮引数の値を固定)
複数の仮引数を使うが、その中のいくつかの値が毎回同じ場合(tax rateなど)それを何回も書かないために。より少ない引数で新しい関数を作れるというメリットもある、
```javascript
//////////////////部分適応なしの場合////////////////////////////
function usdTojpy (USDJPY, paymentUSD, handlingCharge) {
   return USDJPY * paymentUSD + handlingCharge;
}

let usdTojpyCarry = 
function(handlingCharge) {
   return function(USDJPY) {
      return function(paymentUSD) {
         return USDJPY * paymentUSD + handlingCharge;
      }
   }
}
//139.85,3000を何回も書く必要がある。
console.log(usdTojpy(139.85, 200, 3000));
console.log(usdTojpy(139.85, 32, 3000));
 
///////////////////部分適応の場合/////////////////////////////
let usdTojpyCarry = 
function(handlingCharge) {
   return function(USDJPY) {
      return function(paymentUSD) {
         return USDJPY * paymentUSD + handlingCharge;
      }
   }
}
//パターン1//curry化ではないが部分適応
let usdTojpyVisaNov17th = function(paymentUSD) {
   return usdTojpy(139.85, paymentUSD, 3000);
}
console.log(usdTojpyVisaNov17th(200));//仮引数paymentUSDに格納される
console.log(usdTojpyVisaNov17th(32));
//パターン2curry化かつ部分適応
let usdTojpyCarry = 
function(handlingCharge) {
   return function(USDJPY) {
      return function(paymentUSD) {
         return USDJPY * paymentUSD + handlingCharge;
      }
   }
}
let usdTojpyVisaNov17thCarry = usdTojpyCarry(3000)(139.85);
console.log(usdTojpyVisaNov17thCarry(200));
console.log(usdTojpyVisaNov17thCarry(32));
//パターン3curru化, 部分適応をアロー関数を使って表す
let usdTojpyCarryArrow = handlingCharge => USDJPY => paymentUSD => USDJPY * paymentUSD + handlingCharge;
let usdTojpyVisaNov17thCarryArrow = usdTojpyCarryArrow(3000)(139.85);
 
console.log(usdTojpyVisaNov17thCarryArrow(200));
console.log(usdTojpyVisaNov17thCarryArrow(32));
```

### クロージャ(returnされても内部の変数countのデータは消えない。)
```javascript
function counterFn() {
   let count = 0;
   
   function increase() {
      // 外部の変数にアクセスできます。
      count ++;
      return count;
   };
   // inner関数を返します。
   return increase;
}
 
// inner関数がグローバル変数に格納されます。
let counter = counterFn();
 
// これにより、counterが存在する限りオブジェクトが保持されます。
// そのため、ローカル変数countの値が保持されてます。
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
console.log(counter()); // 4
```
### ラムダクロージャ
- 複数仮引数があり、そのうちの一つを変更して新しいデータを作りたいときにラムダクロージャを使うと、同じ関数を使用しているが違うデータを出力できる、再利用できるようになる。
```javascript
const federalTaxes = 0.2;
 
function taxLambda(stateTax, state){//2: taxLambda(0.0725, "California")(40000)
   return function(income){
       // global変数を含むスコープ外の変数にアクセスが可能です。
       let taxes = federalTaxes + stateTax;
       console.log("Computing taxes for state..." + state);
       return income - (taxes * income);
   }
}

// taxLambdaで生成されたラムダ関数がグローバル変数にバインドされます。
let californiaF = taxLambda(0.0725, "California");
// let texasF = taxLambda(0.0625, "Texas");
// let hawaiiF = taxLambda(0.04, "Hawaii");
 
// incomeを引数にそれぞれ設定した税率で税金を計算できます。
let income = 40000;
console.log("Calculating income using lambdas");
console.log(californiaF(income));//1: taxLambda(0.0725, "California")(40000)
// console.log(texasF(income));
// console.log(hawaiiF(income));

// このように再利用性が向上します。
let income2 = 500000;
console.log("------Calculating more income using lambdas------");
console.log(californiaF(income2));
// console.log(texasF(income2));
// console.log(hawaiiF(income2));


//Calculating income using lambdas
// Computing taxes for state...California
// 29100
// Computing taxes for state...Texas
// 29500
// Computing taxes for state...Hawaii
// 30400
// ------Calculating more income using lambdas------
// Computing taxes for state...California
// 363750
// Computing taxes for state...Texas
// 368750
// Computing taxes for state...Hawaii
// 380000


```

- タスクリスト
```javascript
function todoCall(arr){
  let i=0;
  function walkThroughArr(){
    i++;
    return i==arr.length+1? "All done!" : arr[i-1];
  }
  return walkThroughArr;
}
todoCaller = todoCall(["Read a Book", "Work out", "Recursion"])
console.log(todoCaller());
console.log(todoCaller());
console.log(todoCaller());
console.log(todoCaller());
// todoCaller() --> Read a Book
// todoCaller() --> Work out
// todoCaller() --> Recursion
// todoCaller() --> All done!
```

### デコレータパターン
- 既存のオブジェクトに高階関数、ラムダを使って新しい振る舞いを動的に追加する。
```javascript
// 以下のコードでは、既存の関数に新しい振る舞いを追加するデコレータパターンを使用する simpleDecorator() 関数を定義しています。simpleDecorator() 関数は関数 f を引数に取り、元の f 関数をラップした新しい関数を返します。


// 返された関数は、元の f 関数を呼び出す前にコンソールにメッセージを記録し、f() を呼び出した結果を返します。これにより、simpleDecorator() 関数は、元の f 関数のソースコードを変更することなく、新しい動作を追加することができます。

function simpleDecorator(f){
   return function(){
       console.log("Running f......");
       return f;
   }
}
 
function helloWorld(){
   return "Hello world";
}
 
//return function名だから、普通にhelloworld関数を返す
let newFunc1 = simpleDecorator(helloWorld);
console.log(newFunc1());
// Running f......
// [Function: helloWorld]
 
//return functionでも無名関数だから何も返されない
let newFunc2 = simpleDecorator(()=>"Hello Jupiter");
console.log(newFunc2());
// Running f......
// [Function]
```
- これがデコレータパターンだ
#### 全く違う内容の入力でも、お互いが同じ構造で処理される場合、デコレータパターンによって再利用性、モジュール化が出来るようになる。
```javascript
// 単項関数（unary function）fを受け取り、新しい機能が追加された関数fを返します。実行するたびにタイマーを使用し、fの実行時間がどれぐらいかを計算します。
function timerDecorator(f){//timerDecorator(fibonacci)(40)
    return function(arg){//40
       let start = Date.now();
       let result = f(arg);//fibonacci(40)
       let end = Date.now();
       console.log("This function took: " + (end-start) + "ms");
       return result;
   }
}
 
// O(1)
console.log(timerDecorator(x=>x*2)(2424));
 
// O(n^2)
console.log(timerDecorator(x=>{
    let finalResult = 1;
    for(let i = 1; i < x; i++){
        let result = i;
        for(let j = 1; j < i; j++){
            result += j;
        }
        finalResult += result;
    }
    return finalResult;
})(10000));
 
// O(2^n)
function fibonacci(n){
    if(n <= 0) return 0;
    if(n == 1) return 1;
    return fibonacci(n-1) + fibonacci(n-2);
}
 
// O(n)
function fibonacciFast(fib1,fib2,n){
    if(n <= 0) return fib1;
    return fibonacciFast(fib2,fib1+fib2,n-1);
}
 
let timedFibonacci = timerDecorator(fibonacci);
console.log(timedFibonacci(40));//timerDecorator(fibonacci)(40)
 
let timedFibonacciFast = timerDecorator(n=>fibonacciFast(0,1,n));
console.log(timedFibonacciFast(40));
```
- デコレータパターンにさらにクロージャをつけたパターン
```javascript
const USDtoJPY = 138.31;
 
// 普段下記関数を使用して給与計算を行っています。
function calcDollerSalary(baseSalary, hourWage) {//100,12
   let salary = baseSalary;//100
 
   function getSalary() {
      return "$" + salary;
   }
 
   function work(time) {
      salary += hourWage * time;
   }
 
   return {
      getSalary: getSalary,
      work: work
   }
}
 
// 普段記録はドルベースで行っています。
let mikeSalary = calcDollerSalary(100, 12);
mikeSalary.work(10);
mikeSalary.work(3);//13ってこと
console.log("Mike's income: " + mikeSalary.getSalary());
 
// しかし、日本人Ryoが入社し、給与明細に日本円を併記することにしました。
// 今後日本人が入社することは少ないと考え、calcDollerSalaryを変更せずにデコレータパターンを使って対応することにしました。
 
// 円に変換して出力する関数にデコレートします。
function changeToYenUnit(f) {
   return function() {
      let moneyWithUnit = f();
 
      // 小数点付き数値を取得する正規表現を使用し、数値に変換します。
      let yenUnitMoney = moneyWithUnit.match(/d+.?d+/)[0] * USDtoJPY;
      return "¥" + yenUnitMoney;
   }
}
  
// 記録はドルベースで行っています。
let ryoSalary = calcDollerSalary(80, 10);
ryoSalary.work(10);
ryoSalary.work(6);
console.log("Ryo's income: " + ryoSalary.getSalary());
 
// ここでgetSalaly関数に円へ変換する関数をデコレートします。
let getRyoSalaryYen = changeToYenUnit(ryoSalary.getSalary);
// このように円に変換された値を出力することができます。
console.log("Ryo's income(yen): " + getRyoSalaryYen());
 
 
console.log("------------");
// calcDollerSalary関数自体を変更したわけではないため、引き続きcalcDollerSalaryは使用できます。
mikeSalary.work(19);
ryoSalary.work(28);
 
console.log("Mike's income: " + mikeSalary.getSalary());
console.log("Ryo's income: " + ryoSalary.getSalary());
console.log("Ryo's income(yen): " + getRyoSalaryYen());
```
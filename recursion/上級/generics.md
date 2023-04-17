```java
Main.java:27: error: <identifier> expected
Main.java:27: error: illegal start of type
```

上記のエラーに遭遇するたびに、何とか適当に修正して、難を逃れてきたけど、今回このエラーにあった時に、流石に逃げるのはやめてジェネリクスを学ぼうと決めた。

型には、
プリミティブ型(int, double, boolean, char)
と
クラス型変数(String, List, Map, Set)がある

- これ簡潔だけどわかりやすい
  int や char はプリミティブ。
  String はクラス。
  String や Integer のクラスは int や char をラップしている。
  String は文字列を頻繁に使うため、唯一例外として、型宣言のように new やコンストラクタの実行部分がなくてもエラーがでないクラス。

クラス型変数とジェネリクスは違うの??

ジェネリクス　 java list
//ArrayList<型パラメータ> 変数名 = new ArrayList<>()
//型パラメータ == 変数
//型パラメータ: オブジェクトにだけ使用する事ができる。

- コンパイル前に正しい型が入っているか事前に知る事ができる。

これは、ArrayList のパッケージ内に入っている変数なので、T など何でも入れられる訳では無く、Integer と入力しなければいけない。

```java
import java.util.ArrayList;// ArrayListライブラリを読み込みます。
import java.util.Arrays; // Arraysライブラリを読み込みます。

class Main{
    // 動的配列を取得し，その内容を出力します。
    public static void printArray(ArrayList<Integer> intArr){
        for (int i = 0; i < intArr.size(); ++i) {
            System.out.print(intArr.get(i) + " ");
        }
        System.out.println();
    }

    public static void main(String[] args){
        ArrayList<Integer> dArr = new ArrayList<Integer>();

        // 初期配列を2つの項目に設定します。
        dArr.add(2);
        dArr.add(3);
        printArray(dArr);

        // 配列の末尾に要素を追加します。動的配列は内部に配列を管理しているので問題なく処理します。
        dArr.add(30);
        dArr.add(645);
        dArr.add(23);
        dArr.add(-35);
        dArr.add(325);
        dArr.add(1425);
        dArr.add(0);
        dArr.add(98);

        printArray(dArr);

        // 配列の最後に多くの値を追加します。
        dArr.addAll(Arrays.asList(3,34,3542,10,202,34,203,-75,-56,45,0,43,1132));
        printArray(dArr);
    }
}
```

# Java Generics with Multiple Types

https://www.tutorialspoint.com/java_generics/java_generics_multiple_type.htm

```java
package com.tutorialspoint;

public class GenericsTester {
   public static void main(String[] args) {
      Box<Integer, String> box = new Box<Integer, String>();
      box.add(Integer.valueOf(10),"Hello World");
      System.out.printf("Integer Value :%d\n", box.getFirst());
      System.out.printf("String Value :%s\n", box.getSecond());

      Box<String, String> box1 = new Box<String, String>();
      box1.add("Message","Hello World");
      System.out.printf("String Value :%s\n", box1.getFirst());
      System.out.printf("String Value :%s\n", box1.getSecond());
   }
}

class Box<T, S> {
   private T t;
   private S s;

   public void add(T t, S s) {
      this.t = t;
      this.s = s;
   }

   public T getFirst() {
      return t;
   }

   public S getSecond() {
      return s;
   }
}

//Integer Value :10
// String Value :Hello World
// String Value :Message
// String Value :Hello World
```

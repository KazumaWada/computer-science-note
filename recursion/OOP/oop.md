# Object Oriented Programming

- 全ての関数とデータがオブジェクト内にまとめられている。
- OOP を構成する 3 つの柱(カプセル化,継承,ポリモーフィズム)
- OOP のパラダイム(概念)をサポートする機能 ↓
  クラスの抽象化、インターフェース、メソッドのオーバーロード、メソッドの上書き、動的バインディング（Dynamic binding）、可視性修飾子（Visibility modifiers）、アップキャスト＆ダウンキャスト、不変性、ジェネリクス（Generics）
- java は全てオブジェクトで構成されている。
- java -> javascript と学ぶと、何が javascript でできないのかが明確になる。

---

- クラスのオブジェクトを作成するとき、データ型はクラス名になることに注意してください。

```java
class RGB24{
    public int red;
    public int green;
    public int blue;

    // RGB24のコンストラクタ。thisは現在のインスタンスの変数へアクセスできるキーワードです。
    // Javaでは、コンストラクタはクラス名と同じ名前のメソッドです。
    // オブジェクトは、thisキーワードで自分自身を参照することができます。thisキーワードは、現在のインスタンスのオブジェクトです。メソッドの内部でアクセスすることができます。
    public RGB24(int red, int green, int blue){
        this.red = red;
        this.green = green;
        this.blue = blue;
    }

    public String getHex(){
        String hex = Integer.toHexString(this.red);
        hex+=Integer.toHexString(this.green);
        hex+=Integer.toHexString(this.blue);

        return hex;
    }

    public String getBits(){
        return Integer.toBinaryString(Integer.parseInt(this.getHex(), 16));
    }

    // RGBが赤、青、緑の中でどの色が濃いのか、もしくはgrayなのか決定することができます。
    public String getColorShade(){
        if(this.red == this.green && this.green == this.blue) return "grayscale";
        String greatestString = "red";
        int greatest = this.red;

        if(greatest <= this.green){
            greatestString = "green";
            greatest = this.green;
        }

        if(greatest <= this.blue){
            greatestString = "blue";
            greatest = this.blue;
        }

        return greatestString;
    }
}

class Main{
    public static void main(String[] args){
        // 3つのRGB24インスタンスを作成します。
        RGB24 color1 = new RGB24(0, 153, 255);
        RGB24 color2 = new RGB24(255, 153, 204);
        RGB24 color3 = new RGB24(153, 255, 51);

        System.out.println(color1.getHex() + " <-> " + color1.getBits() + ". Color: " + color1.getColorShade());
        System.out.println(color2.getHex() + " <-> " + color2.getBits() + ". Color: " + color2.getColorShade());
        System.out.println(color3.getHex() + " <-> " + color3.getBits() + ". Color: " + color3.getColorShade());

        RGB24 gray = new RGB24(123, 123, 123);
        System.out.println(gray.getHex() + " <-> " + gray.getBits() + ". Color: " + gray.getColorShade());
    }
}
```

### リファクタリング(他のコードに影響を与えることなく、修正する)

- 関数のオーバーロード(同じ名前で複数の関数を作成すること)

```java
class RGB24{
    public int red;
    public int green;
    public int blue;

    public RGB24(int red, int green, int blue){
        this.red = red;
        this.green = green;
        this.blue = blue;
    }

    // 文字列のみを入力とするRGB24用のコンストラクタ。これには関数のオーバーロードを使用しています。
    // コンストラクタは、新しいクラスのインスタンスを作成するときに最初に実行しなければならない関数であることを覚えておいてください。ここでは、状態を初期化するために必要なことは何でもできます。
    public RGB24(String inputString){
        int l = inputString.length();

        if(l == 6) this.setColorsByHex(inputString);
        else if(l == 24) this.setColorsByBin(inputString);
        else this.setAsBlack();
    }

    // RGBの状態を16進数で指定します。
    public void setColorsByHex(String hex){
        if(hex.length() != 6) this.setAsBlack();
        else{
            this.red = Integer.parseInt(hex.substring(0,2), 16);
            this.green = Integer.parseInt(hex.substring(2,4), 16);
            this.blue = Integer.parseInt(hex.substring(4,6), 16);
        }
    }

    // RGBの状態を2進数で指定します。
    public void setColorsByBin(String bin){
        if(bin.length() != 24) this.setAsBlack();
        else{
            this.red = Integer.parseInt(bin.substring(0,8), 2);
            this.green = Integer.parseInt(bin.substring(8,16), 2);
            this.blue = Integer.parseInt(bin.substring(16), 2);
        }
    }

    // 状態を黒に設定します。
    public void setAsBlack(){
        this.red = 0;
        this.green = 0;
        this.blue = 0;
    }

    public String getHex(){
        String hex = Integer.toHexString(this.red);
        hex+=Integer.toHexString(this.green);
        hex+=Integer.toHexString(this.blue);

        return hex;
    }

    public String getBits(){
        return Integer.toBinaryString(Integer.parseInt(this.getHex(), 16));
    }

    // RGBが赤、青、緑の中でどの色が濃いのか、もしくはgrayなのか決定することができます。
    public String getColorShade(){
        if(this.red == this.green && this.green == this.blue) return "grayscale";
        String[] stringTable = new String[]{"red","green","blue"};
        int[] values = {this.red, this.green, this.blue};

        // max algorithm。maxへの線形探索。
        int max = values[0];
        int maxIndex = 0;
        for(int i = 1; i < values.length; i++){
            if(max <= values[i]){
                max = values[i];
                maxIndex = i;
            }
        }

        return stringTable[maxIndex];
    }

    // 文字列変換メソッドtoString()。Javaは、System.out.println(data)関数のように、オブジェクトを文字列に変換する必要がある場合、この関数を自動的に呼び出します。
    public String toString(){
        return "The color is rgb(" + this.red + "," + this.green + "," + this.blue + "). Hex: " + this.getHex() + ", binary: " + this.getBits();
    }
}

class Main{
    public static void main(String[] args){
        RGB24 color1 = new RGB24(0, 153, 255);
        RGB24 color2 = new RGB24("ff99cc"); //rgb(255, 153, 204)
        RGB24 color3 = new RGB24("100110011111111100110011"); //rgb(153, 255, 51)
        RGB24 gray = new RGB24("7b7b7b"); //rgb(123, 123, 123)

        System.out.println(color1);
        System.out.println(color2);
        System.out.println(color3);
        System.out.println(gray);

        System.out.println();
        System.out.println("Changing the state of colors");
        System.out.println();

        // 状態の変更
        gray.setAsBlack();
        System.out.println(gray);
        color1.setColorsByHex("2EB656");
        System.out.println(color1);
    }
}
// The color is rgb(0,153,255). Hex: 099ff, binary: 1001100111111111
// The color is rgb(255,153,204). Hex: ff99cc, binary: 111111111001100111001100
// The color is rgb(153,255,51). Hex: 99ff33, binary: 100110011111111100110011
// The color is rgb(123,123,123). Hex: 7b7b7b, binary: 11110110111101101111011
// Changing the state of colors
// The color is rgb(0,0,0). Hex: 000, binary: 0
// The color is rgb(46,182,86). Hex: 2eb656, binary: 1011101011011001010110
```

### メンバ変数とメモリ 01

```java
class Battery{
    public String manufacturer;
    public String model;
    public double voltage;
    public double ampHours;
    public double weightKg;
    public double[] dimensionMm;

    public Battery(String manufacturer, String model, double voltage, double ampHours, double weightKg, double wMm, double hMm, double dMm){
        this.manufacturer = manufacturer;
        this.model = model;
        this.voltage = voltage;
        this.ampHours = ampHours;
        this.weightKg = weightKg;
        this.dimensionMm = new double[]{wMm, hMm, dMm};
    }

    public String toString(){
        // このオブジェクトの参照を取得します。
        String referenceHash = Integer.toHexString(this.hashCode());

        return this.manufacturer + " " + this.model + ": " + this.getPowerCapacity() + "Wh (" + this.voltage + "V/" + this.ampHours + "Ah) - " + this.dimensionMm[0] + "(W)x" + this.dimensionMm[1] + "(H)x" + this.dimensionMm[2] + "(D) " + this.weightKg + "kg .... Instance Reference:" + referenceHash;
    }

    public double getPowerCapacity(){
        return this.voltage * this.ampHours;
    }
}

class Main {
    public static void main(String[] args) {
        Battery mc96 = new Battery("VTec", "MC96", 14.4, 6.6, 0.55, 72, 97, 51.5);
        Battery mc60 = new Battery("VTec", "MC60", 14.4, 4.2, 0.35, 52, 77, 40.5);
        Battery mdPL140 = new Battery("BowserPower", "MD-PL140", 14.4, 9.9, 1.18, 89, 119, 85);
        Battery zlD72 = new Battery("MT-Dell Tech", "ZL-D72", 7.2, 9.9, 1.18, 38, 80, 70);

        // オブジェクト参照をプリントアウトし、その文字列のバージョンを表示します。
        // デフォルトでは、toString()が定義されていない場合、javaはオブジェクト参照を出力します。今回私たちはすでに定義しているので、Javaのすべてのオブジェクトが持っているhashCode()メソッドを使います。これは、オブジェクトの参照を整数として返し、それを16進数に変換します。
        System.out.println("Instance Reference: " + Integer.toHexString(mc96.hashCode()));
        System.out.println(mc96);

        System.out.println();
        // メンバ変数(アンペア時)を出力します。
        System.out.println(mc96.ampHours + " are its amps");

        System.out.println();
        System.out.println(mc60);
        System.out.println();
        System.out.println(mdPL140);
        System.out.println();
        System.out.println(zlD72);
    }
}
```

### メンバ変数とメモリ 02 オブジェクト同士を比較する場合はどうなる?

```java
class Battery{
    public String manufacturer;
    public String model;
    public double voltage;
    public double ampHours;
    public double weightKg;
    public double[] dimensionMm;

    public Battery(String manufacturer, String model, double voltage, double ampHours, double weightKg, double wMm, double hMm, double dMm){
        this.manufacturer = manufacturer;
        this.model = model;
        this.voltage = voltage;
        this.ampHours = ampHours;
        this.weightKg = weightKg;
        this.dimensionMm = new double[]{wMm, hMm, dMm};
    }

    public String toString(){
        // このオブジェクトの参照を取得します。
        String referenceHash = Integer.toHexString(this.hashCode());

        return this.manufacturer + " " + this.model + ": " + this.getPowerCapacity() + "Wh (" + this.voltage + "V/" + this.ampHours + "Ah) - " + this.dimensionMm[0] + "(W)x" + this.dimensionMm[1] + "(H)x" + this.dimensionMm[2] + "(D) " + this.weightKg + "kg .... Instance Reference:" + referenceHash;
    }

    public double getPowerCapacity(){
        return this.voltage * this.ampHours;
    }
}

class Main {
    public static void main(String[] args) {
        Battery mc96 = new Battery("VTec", "MC96", 14.4, 6.6, 0.55, 72, 97, 51.5);
        // 参照がコピーされます。シャローコピー。
        Battery mc96Second = mc96;
        // ディープコピー。全てのメンバ変数は、mc96と同じように構築されます。
        Battery mc96Third = new Battery("VTec", "MC96", 14.4, 6.6, 0.55, 72, 97, 51.5);
        Battery mdLs95 = new Battery("Atomic Units", "MD-LS95", 14.4, 6.6, 0.55, 72, 97, 51.5);

        System.out.println(mc96);
        System.out.println();
        System.out.println(mc96Second);
        System.out.println();
        System.out.println(mc96Third);
        System.out.println();
        System.out.println(mdLs95);

        System.out.println();
        System.out.println(mc96 == mc96); //True
        System.out.println(mc96 == mc96Second); //True
        System.out.println(mc96 == mc96Third); //False
        System.out.println(mc96 == mdLs95); //False
    }
}
```

### scope(スコープ)01

```java
class Wallet{
    public int bill1;
    public int bill5;
    public int bill10;
    public int bill20;
    public int bill50;
    public int bill100;

    public Wallet(){}

    public int getTotalMoney(){
        return (1*this.bill1) + (5*this.bill5) + (10*this.bill10) + (20*this.bill20) + (50*this.bill50) + (100*this.bill100);
    }
}

class Person{
    public String firstName;
    public String lastName;
    public int age;
    public double heightM;
    public double weightKg;
    public Wallet wallet;

    public Person(String firstName){
        this.firstName = firstName;
    }

    public int getCash(){
        // オブジェクト型なので、walletはnullになります。
        if(this.wallet == null){
            System.out.println("NO WALLET");
            return 0;
        }
        return this.wallet.getTotalMoney();
    }
}

class Main{
    public static void main(String[] args){
        Person p = new Person("Ryu");
        System.out.println("firstname - " + p.firstName);
        System.out.println("lastname - " + p.lastName); //Stringはオブジェクト型で、デフォルトはnullになります。
        System.out.println("age - " + p.age);
        System.out.println("height - " + p.heightM);
        System.out.println("weight - " + p.weightKg);
        System.out.println("Current Money - " + p.getCash());

        p.lastName = "Poolhopper";
        p.age = 40;
        p.heightM = 180;
        p.weightKg = 140;

        System.out.println();
        System.out.println("firstname - " + p.firstName);
        System.out.println("lastname - " + p.lastName);
        System.out.println("age - " + p.age);
        System.out.println("height - " + p.heightM);
        System.out.println("weight - " + p.weightKg);
    }
}
```

```java
class Wallet{
    public int bill1 = 3;
    public int bill5 = 1;
    public int bill10;
    public int bill20;
    public int bill50;
    public int bill100;

    public Wallet(){}

    public int getTotalMoney(){
        return (1*this.bill1) + (5*this.bill5) + (10*this.bill10) + (20*this.bill20) + (50*this.bill50) + (100*this.bill100);
    }
}

class Person{
    public String firstName;
    public String lastName = "?????";
    public int age = 20;
    public double heightM;
    public double weightKg;
    public Wallet wallet;

    public Person(String firstName){
        this.firstName = firstName;
        this.wallet = new Wallet(); // デフォルトのwalletを設定します。
    }

    public int getCash(){
        if(this.wallet == null){
            System.out.println("NO WALLET");
            return 0;
        }
        return this.wallet.getTotalMoney();
    }
}

class Main{
    public static void main(String[] args){
        Person p = new Person("Ryu");
        System.out.println("firstname - " + p.firstName);
        System.out.println("lastname - " + p.lastName);
        System.out.println("age - " + p.age);
        System.out.println("height - " + p.heightM);
        System.out.println("weight - " + p.weightKg);
        System.out.println("Current Money - " + p.getCash());

        p.lastName = "Poolhopper";
        p.age = 40;
        p.heightM = 180;
        p.weightKg = 140;

        System.out.println();
        System.out.println("firstname - " + p.firstName);
        System.out.println("lastname - " + p.lastName);
        System.out.println("age - " + p.age);
        System.out.println("height - " + p.heightM);
        System.out.println("weight - " + p.weightKg);
    }
}
```

# scope02

```java
class Wallet{
    public int bill1;
    public int bill5;
    public int bill10;
    public int bill20;
    public int bill50;
    public int bill100;

    public Wallet(){}

    public int getTotalMoney(){
        return (1*bill1) + (5*bill5) + (10*bill10) + (20*bill20) + (50*bill50) + (100*bill100);
    }

    // moneyを挿入し、財布に入れた金額を返します。
    public int insertBill(int bill, int amount){
        switch(bill){
            case(1):
                bill1 += amount;
                break;
            case(5):
                bill5 += amount;
                break;
            case(10):
                bill10 += amount;
                break;
            case(20):
                bill20 += amount;
                break;
            case(50):
                bill50 += amount;
                break;
            case(100):
                bill100 += amount;
                break;
            default:
                return 0;
        }

        return bill*amount;
    }
}

class Person{
    public String firstName;
    public String lastName;
    public int age;
    public double heightM;
    public double weightKg;
    public Wallet wallet;

    public Person(String firstName, String lastName, int x, double y, double z){
        this.firstName = firstName;
        this.lastName = lastName;
        age = x; // ageの状態がxへアップデートされます。
        heightM = y;
        weightKg = z;
        wallet = new Wallet();
    }

    public int getCash(){
        if(this.wallet == null){
            System.out.println("NO WALLET");
            return 0;
        }
        return this.wallet.getTotalMoney();
    }

    public void printState(){
        // thisキーワードは必要ではありません。スコープのルールが使われます。
        System.out.println("firstname - " + firstName);
        System.out.println("lastname - " + lastName);
        System.out.println("age - " + age);
        double weightKg = 495; //weightKg ローカル変数が優先度が高いです。
        System.out.println("height - " + heightM + ", joking it is: " + this.weightKg);
        System.out.println("weight - " + weightKg);
        System.out.println("Current Money - " + getCash());
        System.out.println();
    }
}

class Main{
    public static void main(String[] args){
        Person p = new Person("Ryu","Poolhopper", 40, 180, 140);
        p.printState();

        p.wallet.insertBill(5,3);
        p.wallet.insertBill(100,2);

        p.printState();
    }
}
```

### クラス変数(static, オブジェクト外に定義されている変数)01

- もし全てのクラスが同じメンバ変数を使いたい場合どうする?
  ↓
  静的メモリ(グローバル変数より安全)
  "静的メモリ割り当て:静的メモリ割り当ては、プログラムが実行される前であるコンパイル時に行われます。割り当てられるサイズは固定されており、静的に割り当てされた変数はプログラムの開始から終了まで存続します。"
- 静的メンバ変数 == クラス変数と呼ぶ。//static int S = 5;
  ↓
- 他のメンバ変数: オブジェクト内(インスタンス内)に含まれているのでインスタンス変数と呼ばれる。//int i = 3;

```java
class Battery7v{
    public String manufacturer;
    public String model;
    public static double voltage = 7.2;
    public static String type = "Lithium-Ion";
    public static int manufacturedCount;
    public double ampHours;
    public double weightKg;
    public double[] dimensionMm;

    public Battery7v(String manufacturer, String model, double ampHours, double weightKg, double wMm, double hMm, double dMm){
        this.manufacturer = manufacturer;
        this.model = model;
        this.ampHours = ampHours;
        this.weightKg = weightKg;
        this.dimensionMm = new double[]{wMm, hMm, dMm};

        // カウントを増加させます。
        this.manufacturedCount+=1;
    }

    public String toString(){
        return this.manufacturer + " " + this.model + " " + this.type + " Battery: " + this.getPowerCapacity() + "Wh (" + this.voltage + "V/" + this.ampHours + "Ah) - " + this.dimensionMm[0] + "(W)x" + this.dimensionMm[1] + "(H)x" + this.dimensionMm[2] + "(D) " + this.getVolume() + " volume " + this.weightKg + "kg";
    }

    public double getPowerCapacity(){
        return this.voltage * this.ampHours;
    }

    public double getVolume(){
        return this.dimensionMm[0] * this.dimensionMm[1] * this.dimensionMm[2];
    }
}

class Main {
    public static void main(String[] args) {
        Battery7v zlD72 = new Battery7v("MT-Dell Tech", "ZL-D72", 9.9, 1.18, 38, 80, 70);
        Battery7v zlD50 = new Battery7v("MT-Dell Tech", "ZL-D50", 6.6, 0.9, 28, 50, 65);
        Battery7v zlD40 = new Battery7v("MT-Dell Tech", "ZL-D40", 5.3, 1.18, 38, 80, 70);

        System.out.println(zlD72);
        System.out.println(zlD50);
        System.out.println(zlD40);

        System.out.println();
        System.out.println("Accessing class member variables...");
        System.out.println(Battery7v.manufacturedCount);
        System.out.println(zlD40.manufacturedCount);
        System.out.println();

        System.out.println("Changing the internal structure of Battery7v!");

        Battery7v mdPL140 = new Battery7v("BowserPower", "MD-PL140", 9.9, 1.18, 89, 119, 85);
        // クラス変数を上書きします。危険: 全ての電池・インスタンスが同じ静的メモリアドレスを指しているため、他の全てのクラスの状態が更新されます。
        mdPL140.voltage = 14.4;

        // 全てのBattery7vは、7.2vではなく、14.4vです。
        System.out.println(mdPL140);
        System.out.println(zlD72.voltage);
        System.out.println(zlD50.voltage);
        System.out.println(zlD40.voltage);

        System.out.println();
        System.out.println("Total batteries manufactured: " + Battery7v.manufacturedCount);
    }
}
```

### クラス変数 02

```java
class Battery7v{
    public String manufacturer;
    public String model;
    public static final double VOLTAGE = 7.2;
    public static final String TYPE = "Lithium-Ion";
    public static int manufacturedCount;
    public double ampHours;
    public double weightKg;
    public double[] dimensionMm;

    public Battery7v(String manufacturer, String model, double ampHours, double weightKg, double wMm, double hMm, double dMm){
        this.manufacturer = manufacturer;
        this.model = model;
        this.ampHours = ampHours;
        this.weightKg = weightKg;
        this.dimensionMm = new double[]{wMm, hMm, dMm};

        this.manufacturedCount+=1;
    }

    public String toString(){
        return this.manufacturer + " " + this.model + " " + Battery7v.TYPE + " Battery: " + this.getPowerCapacity() + "Wh (" + Battery7v.VOLTAGE + "V/" + this.ampHours + "Ah) - " + this.dimensionMm[0] + "(W)x" + this.dimensionMm[1] + "(H)x" + this.dimensionMm[2] + "(D) " + this.getVolume() + " volume " + this.weightKg + "kg";
    }

    public double getPowerCapacity(){
        return Battery7v.VOLTAGE * this.ampHours;
    }

    public double getVolume(){
        return this.dimensionMm[0] * this.dimensionMm[1] * this.dimensionMm[2];
    }
}

class Main {
    public static void main(String[] args) {
        Battery7v zlD72 = new Battery7v("MT-Dell Tech", "ZL-D72", 9.9, 1.18, 38, 80, 70);
        Battery7v zlD50 = new Battery7v("MT-Dell Tech", "ZL-D50", 6.6, 0.9, 28, 50, 65);
        Battery7v zlD40 = new Battery7v("MT-Dell Tech", "ZL-D40", 5.3, 1.18, 38, 80, 70);

        System.out.println(zlD72);
        System.out.println(zlD50);
        System.out.println(zlD40);

        Battery7v mdPL140 = new Battery7v("BowserPower", "MD-PL140", 9.9, 1.18, 89, 119, 85);

        // コンピュータは実行できません。
        //mdPL140.voltage = 14.4;

        System.out.println();
        System.out.println("Voltages:");
        System.out.println(zlD72.VOLTAGE);
        System.out.println(zlD50.VOLTAGE);
        System.out.println(zlD40.VOLTAGE);
        System.out.println(mdPL140.VOLTAGE);

        System.out.println();
        System.out.println("Total batteries manufactured: " + Battery7v.manufacturedCount);
    }
}
```

### static メソッド 01

```java
class Card{
    public String rank;
    public String suit;

    public Card(String rank, String suit){
        this.rank = rank;
        this.suit = suit;
    }

    public String toString(){
        return this.rank + this.suit;
    }
}

class Deck{
    public static final String[] SUITS = {"♠","♡","♢","♣"};
    public static final String[] RANKS = {"A","2","3","4","5","6","7","8","9","10","J","Q","K"};
    public Card[] cards;

    public Deck(){
        this.cards = Deck.createDeck(); // クラスメソッドへのアクセス
    }

    public static Card[] createDeck(){
        int s = Deck.SUITS.length;
        int r = Deck.RANKS.length;
        Card[] cards = new Card[s * r];

        for(int i = 0; i < s; i++){
            for(int j = 0; j < r; j++){
                cards[i*r+j] = new Card(Deck.RANKS[j], Deck.SUITS[i]);
            }
        }

        return cards;
    }

    // フィッシャーズアルゴリズムを使ってシャッフル
    public void shuffleDeck(){
        int deckSize = this.cards.length;

        // 最後から始めて、ランダムにスワップを選択してから左の要素(i--)に移動します。
        for (int i = deckSize-1; i >= 0 ; i--) {
            int j = (int) Math.floor(Math.random() * (i + 1));

            Card temp = this.cards[i];

            // in-placeスワップ
            this.cards[i] = this.cards[j];
            this.cards[j] = temp;
        }
    }

    public String toString(){
        return Deck.cardsToString(this.cards);
    }

    public static String cardsToString(Card[] inputCards){
        String s = "";

        for(int i = 0; i < inputCards.length;i++){
            s+=inputCards[i].toString();
            if(i%5 == 4) s+="";
        }
        return s;
    }
}

class Main{
    public static void main(String[] args){
        Deck d = new Deck();
        System.out.println(d);

        Card[] myCards = Deck.createDeck();

        System.out.println();
        System.out.println(Deck.cardsToString(myCards));
        System.out.println();

        d.shuffleDeck();
        System.out.println(d);
    }
}
```

### static メソッド 02

- MathThings を定義するステートレスライブラリ

```java
// MathThingsの全てのメソッドはどの状態にも依存しません。これらのメソッドはMathThingsのインスタンスを作成することなく、いつでも自由に使うことができます。
class MathThings {
    // クラスメンバ定数を定義します
    public static final double PIAPPROX = 3.14159265359;

    // staticでアプリケーションが存続する限り、どこでもアクセスすることができます。
    public static double circleSurfaceArea (int x)  {
        return PIAPPROX * x * x;
    }

    public static double boxVolume(double x){
        return x*x*x;
    }
}

class Main{

    public static void main(String[] args){
        System.out.println("πは" + MathThings.PIAPPROX);

        System.out.println("1辺が2の立方体の体積は" + MathThings.boxVolume(2));
        System.out.println("半径4の円の面積は" + MathThings.circleSurfaceArea(4));

        System.out.println("1辺が5の立方体の体積は" + MathThings.boxVolume(5));
    }
}
```

### カプセル化 01

```java
class Battery7v{
    public String manufacturer;
    public String model;
    public static final double VOLTAGE = 7.2;
    public static final String TYPE = "Lithium-Ion";
    public static int manufacturedCount;
    public double ampHours;
    public double weightKg;
    public double[] dimensionMm;

    public Battery7v(String manufacturer, String model, double ampHours, double weightKg, double wMm, double hMm, double dMm){
        this.manufacturer = manufacturer;
        this.model = model;
        this.ampHours = ampHours;
        this.weightKg = weightKg;
        this.dimensionMm = new double[]{wMm, hMm, dMm};

        // カウントの増加
        this.manufacturedCount+=1;
    }

    public String toString(){
        return this.manufacturer + " " + this.model + " " + Battery7v.TYPE + " Battery: " + this.getPowerCapacity() + "Wh (" + Battery7v.VOLTAGE + "V/" + this.ampHours + "Ah) - " + this.dimensionMm[0] + "(W)x" + this.dimensionMm[1] + "(H)x" + this.dimensionMm[2] + "(D) " + this.getVolume() + " volume " + this.weightKg + "kg";
    }

    public double getPowerCapacity(){
        return Battery7v.VOLTAGE * this.ampHours;
    }

    public double getVolume(){
        return this.dimensionMm[0] * this.dimensionMm[1] * this.dimensionMm[2];
    }
}

class ExternalModule{
    // この関数は意図しない副作用を引き起こします。
    public static void dangerousMethod(String customerId, Battery7v battery){
        System.out.println("Processing data....internals");
        System.out.println("Client " + customerId + " purchased a " + battery.toString());

        // 状態を変更します。
        battery.manufacturedCount += 4234;
    }

    // 意図しない副作用
    public static void otherDangerousMethod(){
        Battery7v.manufacturedCount += 10000;
    }
}

class Main {
    public static void main(String[] args) {
        Battery7v zlD72 = new Battery7v("MT-Dell Tech", "ZL-D72", 9.9, 1.18, 38, 80, 70);
        Battery7v zlD50 = new Battery7v("MT-Dell Tech", "ZL-D50", 6.6, 0.9, 28, 50, 65);
        Battery7v zlD40 = new Battery7v("MT-Dell Tech", "ZL-D40", 5.3, 1.18, 38, 80, 70);

        System.out.println("Total batteries manufactured: " + Battery7v.manufacturedCount);

        System.out.println();
        ExternalModule.dangerousMethod("AD515221", zlD40);
        ExternalModule.otherDangerousMethod();

        System.out.println();
        // 突然、他のモジュールがコードを実行した後に14237個のBattery7vのインスタンスが作成されたと言います。
        System.out.println("Total batteries manufactured: " + Battery7v.manufacturedCount);
    }
}
```

```java
import java.util.Calendar;

class Person{
    private String firstName;
    private String lastName;
    private double heightM;
    private double weightKg;
    private int birthYear;

    public Person(String firstName, String lastName, double heightM, double weightKg, int birthYear){
        this.firstName = firstName;
        this.lastName = lastName;
        this.heightM = heightM;
        this.weightKg = weightKg;
        this.birthYear = birthYear;
    }

    public String toString(){
        // このクラス自体だけが、内部のprivate変数/メソッドにアクセスを持っています。
        return this.getFullName() + ", heightM: " + this.heightM + ", weightKg: " + this.weightKg + ", age: " + this.getAge();
    }

    // 利用者が人物の名前を変更できるようにします。このメソッドを呼び出すことでのみ名前を変更することができ、それ以外の方法では変更することはできません。
    public void changeName(String firstName, String lastName){
        this.firstName = firstName;
        this.lastName = lastName;
    }

    private String getFullName(){
        return this.firstName + " " + this.lastName;
    }

    public int getAge(){
        int currentYear = Calendar.getInstance().get(Calendar.YEAR);
        return currentYear - this.birthYear;
    }
}

class Main{
    public static void main(String[] args){
        Person carly = new Person("Carly", "Angelo", 1.72, 85.5, 1996);

        System.out.println(carly);

        // getAgeはpublicです。
        System.out.println(carly.getAge());

        // Personの全ての内部private定義は、クラス外では利用できません。これらは、内部プロセスを実装するため、クラス内部にカプセル化されたものです。クラスの利用者は、利用可能なパブリック定義のみを使用しなければなりません。
        // ERROR, getFullNameはprivateです。クラスの利用者は利用できません。
        // System.out.println(carly.getFullName());

        // ERROR, lastNameはprivateです。クラスの利用者は利用できません。
        // carly.lastName = "Bardernson";

        carly.changeName("Carly","Bardernson");
        System.out.println();
        System.out.println(carly);

        // ERROR. Note the class definition does not allow any way to update the birthYear after instantiation. This is just like real life, it is illegal to change one's birthYear. クラスの定義では、インスタンス化後にbirthYearを更新する方法は一切許可されていないことに注意してください。これは現実の生活と同じで、自分の誕生年を変更することはあってはなりません。
        // carly.birthYear = 1976
    }
}
```

### カプセル化 02

```java
import java.util.Calendar;

class Person{
    private String firstName;
    private String lastName;
    private double heightM;
    private double weightKg;
    private int birthYear;

    public Person(String firstName, String lastName, double heightM, double weightKg, int birthYear){
        this.firstName = firstName;
        this.lastName = lastName;
        this.heightM = heightM;
        this.weightKg = weightKg;
        this.birthYear = birthYear;
    }

    // getter/setterの始まり
    public String getFirstName(){
        return this.firstName;
    }

    public void setFirstName(String firstName){
        this.firstName = firstName;
    }

    public String getLastName(){
        return this.lastName;
    }

    public void setLastName(String lastName){
        this.lastName = lastName;
    }

    public double getHeightM(){
        return this.heightM;
    }

    public void setHeightM(double heightM){
        this.heightM = heightM;
    }

    public double getWeightKg(){
        return this.weightKg;
    }

    public void setWeightKg(double weightKg){
        this.weightKg = weightKg;
    }

    public int getBirthYear(){
        return this.birthYear;
    }

    // getter/setterの終わり

    public String toString(){
        return this.getFullName() + ", heightM: " + this.heightM + ", weightKg: " + this.weightKg + ", age: " + this.getAge();
    }

    private String getFullName(){
        return this.firstName + " " + this.lastName;
    }

    public int getAge(){
        int currentYear = Calendar.getInstance().get(Calendar.YEAR);
        return currentYear - this.birthYear;
    }
}

class Main{
    public static void main(String[] args){
        Person carly = new Person("Carly", "Angelo", 1.72, 85.5, 1996);

        System.out.println(carly);
        System.out.println(carly.getAge());

        carly.setFirstName("Carly II");
        carly.setLastName("Bardernson");
        System.out.println();
        System.out.println(carly);

        System.out.println();
        System.out.println("Weight:" + carly.getWeightKg());
        System.out.println("Height:" + carly.getHeightM());
        System.out.println("Birth:" + carly.getBirthYear());

        // ERROR
        // carly.lastName = "Bardernson";
        // carly.birthYear = 1976;
        // System.out.println(carly.firstName);
    }
}
```

```java
class Battery7v{
    public String manufacturer;
    public String model;
    public static final double VOLTAGE = 7.2;
    public static final String TYPE = "Lithium-Ion";
    public static int manufacturedCount;
    public double ampHours;
    public double weightKg;
    public double[] dimensionMm;

    public Battery7v(String manufacturer, String model, double ampHours, double weightKg, double wMm, double hMm, double dMm){
        this.manufacturer = manufacturer;
        this.model = model;
        this.ampHours = ampHours;
        this.weightKg = weightKg;
        this.dimensionMm = new double[]{wMm, hMm, dMm};

        this.manufacturedCount+=1;
    }

    public String toString(){
        return this.manufacturer + " " + this.model + " " + Battery7v.TYPE + " Battery: " + this.getPowerCapacity() + "Wh (" + Battery7v.VOLTAGE + "V/" + this.ampHours + "Ah) - " + this.dimensionMm[0] + "(W)x" + this.dimensionMm[1] + "(H)x" + this.dimensionMm[2] + "(D) " + this.getVolume() + " volume " + this.weightKg + "kg";
    }

    public double getPowerCapacity(){
        return Battery7v.VOLTAGE * this.ampHours;
    }

    public double getVolume(){
        return this.dimensionMm[0] * this.dimensionMm[1] * this.dimensionMm[2];
    }
}

class ExternalModule{
    public static void dangerousMethod(String customerId, Battery7v battery){
        System.out.println("Processing data....internals");
        System.out.println("Client " + customerId + " purchased a " + battery.toString());

        //battery.manufacturedCount += 4234;
    }

    public static void otherDangerousMethod(){
        //Battery7v.manufacturedCount += 10000;
    }
}

class Main {
    public static void main(String[] args) {
        Battery7v zlD72 = new Battery7v("MT-Dell Tech", "ZL-D72", 9.9, 1.18, 38, 80, 70);
        Battery7v zlD50 = new Battery7v("MT-Dell Tech", "ZL-D50", 6.6, 0.9, 28, 50, 65);
        Battery7v zlD40 = new Battery7v("MT-Dell Tech", "ZL-D40", 5.3, 1.18, 38, 80, 70);

        System.out.println("Total batteries manufactured: " + Battery7v.manufacturedCount);

        System.out.println();
        ExternalModule.dangerousMethod("AD515221", zlD40);
        ExternalModule.otherDangerousMethod();

        System.out.println();
        System.out.println("Total batteries manufactured: " + Battery7v.manufacturedCount);
    }
}
```

### クラスの抽象化

```java
class RGB24{
    private int red;
    private int green;
    private int blue;

    public RGB24(int red, int green, int blue){
        this.red = red;
        this.green = green;
        this.blue = blue;
    }

    public RGB24(String inputString){
        int l = inputString.length();

        if(l == 6) this.setColorsByHex(inputString);
        else if(l == 24) this.setColorsByBin(inputString);
        else this.setAsBlack();
    }

    public void setColorsByHex(String hex){
        if(hex.length() != 6) this.setAsBlack();
        else{
            this.red = Integer.parseInt(hex.substring(0,2), 16);
            this.green = Integer.parseInt(hex.substring(2,4), 16);
            this.blue = Integer.parseInt(hex.substring(4,6), 16);
        }
    }

    public void setColorsByBin(String bin){
        if(bin.length() != 24) this.setAsBlack();
        else{
            this.red = Integer.parseInt(bin.substring(0,8), 2);
            this.green = Integer.parseInt(bin.substring(8,16), 2);
            this.blue = Integer.parseInt(bin.substring(16), 2);
        }
    }

    public void setAsBlack(){
        this.red = 0;
        this.green = 0;
        this.blue = 0;
    }

    public String getHex(){
        String hex = Integer.toHexString(this.red);
        hex+=Integer.toHexString(this.green);
        hex+=Integer.toHexString(this.blue);

        return hex;
    }

    public String getBits(){
        return Integer.toBinaryString(Integer.parseInt(this.getHex(), 16));
    }

    public String getColorShade(){
        if(this.red == this.green && this.green == this.blue) return "greyscale";
        String[] stringTable = new String[]{"red","green","blue"};
        int[] values = {this.red, this.green, this.blue};

        int max = values[0];
        int maxIndex = 0;
        for(int i = 1; i < values.length; i++){
            if(max <= values[i]){
                max = values[i];
                maxIndex = i;
            }
        }

        return stringTable[maxIndex];
    }

    public String toString(){
        return "The color is rgb(" + this.red + "," + this.green + "," + this.blue + "). Hex: " + this.getHex() + ", binary: " + this.getBits();
    }
}

class Main{
    public static void main(String[] args){
        RGB24 color1 = new RGB24(0, 153, 255);
        RGB24 color2 = new RGB24("ff99cc");//rgb(255, 153, 204)
        RGB24 color3 = new RGB24("100110011111111100110011");//rgb(153, 255, 51)
        RGB24 grey = new RGB24("7b7b7b");//rgb(123, 123, 123)
    }
}
```

```java
class RGB24{
    private int red;
    private int green;
    private int blue;

    public RGB24(int red, int green, int blue){
        this.red = red;
        this.green = green;
        this.blue = blue;
    }

    public RGB24(String inputString){
        int l = inputString.length();

        if(l == 6) this.setColorsByHex(inputString);
        else if(l == 24) this.setColorsByBin(inputString);
        else this.setAsBlack();
    }

    public void setColorsByHex(String hex){
        if(hex.length() != 6) this.setAsBlack();
        else{
            this.red = Integer.parseInt(hex.substring(0,2), 16);
            this.green = Integer.parseInt(hex.substring(2,4), 16);
            this.blue = Integer.parseInt(hex.substring(4,6), 16);
        }
    }

    public void setColorsByBin(String bin){
        if(bin.length() != 24) this.setAsBlack();
        else{
            this.red = Integer.parseInt(bin.substring(0,8), 2);
            this.green = Integer.parseInt(bin.substring(8,16), 2);
            this.blue = Integer.parseInt(bin.substring(16), 2);
        }
    }

    public void setAsBlack(){
        this.red = 0;
        this.green = 0;
        this.blue = 0;
    }

    public String getHex(){
        String hex = Integer.toHexString(this.red);
        hex+=Integer.toHexString(this.green);
        hex+=Integer.toHexString(this.blue);

        return hex;
    }

    public String getBits(){
        return Integer.toBinaryString(Integer.parseInt(this.getHex(), 16));
    }

    public String getColorShade(){
        if(this.red == this.green && this.green == this.blue) return "greyscale";
        String[] stringTable = new String[]{"red","green","blue"};
        int[] values = {this.red, this.green, this.blue};

        int max = values[0];
        int maxIndex = 0;
        for(int i = 1; i < values.length; i++){
            if(max <= values[i]){
                max = values[i];
                maxIndex = i;
            }
        }

        return stringTable[maxIndex];
    }

    public String toString(){
        return "The color is rgb(" + this.red + "," + this.green + "," + this.blue + "). Hex: " + this.getHex() + ", binary: " + this.getBits();
    }
}

class Main{
    public static void main(String[] args){
        RGB24 color1 = new RGB24(0, 153, 255);
        RGB24 color2 = new RGB24("ff99cc");//rgb(255, 153, 204)
        RGB24 color3 = new RGB24("100110011111111100110011");//rgb(153, 255, 51)
        RGB24 grey = new RGB24("7b7b7b");//rgb(123, 123, 123)
    }
}
```

### 不変オブジェクトの作成

- 不変: static で定義された後は変更できないみたいなやつ。

```java
class RGB24Immutable{
    private int red;
    private int green;
    private int blue;

    public RGB24Immutable(int red, int green, int blue){
        this.red = red;
        this.green = green;
        this.blue = blue;
    }

    public RGB24Immutable(String inputString){
        int l = inputString.length();

        if(l == 6) this.setColorsByHex(inputString);
        else if(l == 24) this.setColorsByBin(inputString);
        else this.setAsBlack();
    }

    private void setColorsByHex(String hex){
        if(hex.length() != 6) this.setAsBlack();
        else{
            this.red = Integer.parseInt(hex.substring(0,2), 16);
            this.green = Integer.parseInt(hex.substring(2,4), 16);
            this.blue = Integer.parseInt(hex.substring(4,6), 16);
        }
    }

    private void setColorsByBin(String bin){
        if(bin.length() != 24) this.setAsBlack();
        else{
            this.red = Integer.parseInt(bin.substring(0,8), 2);
            this.green = Integer.parseInt(bin.substring(8,16), 2);
            this.blue = Integer.parseInt(bin.substring(16), 2);
        }
    }

    private void setAsBlack(){
        this.red = 0;
        this.green = 0;
        this.blue = 0;
    }

    public String getHex(){
        String hex = Integer.toHexString(this.red);
        hex+=Integer.toHexString(this.green);
        hex+=Integer.toHexString(this.blue);

        return hex;
    }

    public String getBits(){
        return Integer.toBinaryString(Integer.parseInt(this.getHex(), 16));
    }

    public String getColorShade(){
        if(this.red == this.green && this.green == this.blue) return "greyscale";
        String[] stringTable = new String[]{"red","green","blue"};
        int[] values = {this.red, this.green, this.blue};

        int max = values[0];
        int maxIndex = 0;
        for(int i = 1; i < values.length; i++){
            if(max <= values[i]){
                max = values[i];
                maxIndex = i;
            }
        }

        return stringTable[maxIndex];
    }

    public String toString(){
        return "The color is rgb(" + this.red + "," + this.green + "," + this.blue + "). Hex: " + this.getHex() + ", binary: " + this.getBits();
    }
}

class Main{
    public static void main(String[] args){
        RGB24Immutable color1 = new RGB24Immutable(0, 153, 255);
        RGB24Immutable color2 = new RGB24Immutable("ff99cc");//rgb(255, 153, 204)
        RGB24Immutable color3 = new RGB24Immutable("100110011111111100110011");//rgb(153, 255, 51)
        RGB24Immutable grey = new RGB24Immutable("7b7b7b");//rgb(123, 123, 123)

        System.out.println(grey);
        System.out.println();
        System.out.println(color1);
        System.out.println();

        // コンストラクタ後、RGB24Immutableの状態を変更することはできません。
        //color1.red = 200;//ERROR
        //color1.setAsBlack();//ERROR
        //color1.setColorsByHex("ff99cc");//ERROR
        System.out.println(color1);
    }
}
```

### 不変文字列

```java
class Main{
    public static void printChars(char[] charArr){
        System.out.print("Printing character array: ");
        for(int i = 0; i < charArr.length; i++) System.out.print(charArr[i]);
        System.out.println();
    }

    public static void main(String[] args){
        char[] str1Chars = new char[]{'h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd', '!'};
        printChars(str1Chars);

        // 文字の配列の状態を変更することができます。

        str1Chars[6] = 'E';
        str1Chars[7] = 'a';
        str1Chars[8] = 'r';
        str1Chars[9] = 't';
        str1Chars[10] = 'h';
        printChars(str1Chars);

        // 文字の配列の文字列を作成することができます。
        String s1 = new String(str1Chars);

        // Stringオブジェクト型の状態を変更することはできません。
        // ERROR
        // s1[3] = 'e';
        // Stringオブジェクトの状態を変更するミューテータメソッドは存在しません。不変性。

        // 文字列を比較してみましょう。
        System.out.println();
        System.out.println("Comparing strings....");
        String s2 = new String(str1Chars);

        String s3 = "Hello World!";
        String s4 = "Hello World!";
        String s5 = new String("Hello World!");


        //Note that java may do some optimizations like assign string literals to the same location, or strings that it can guarantee to be the same to a similar location. It can only do this due to  immutability.
        System.out.println(System.identityHashCode(s1));
        System.out.println(System.identityHashCode(s2));
        //On concatination, a completely new string object is made.
        System.out.println(System.identityHashCode((s1+s2)));
        System.out.println(System.identityHashCode(s3));

        //Even though they are the same strings, == by default does a reference comparison in Java to determine equality. The equals method on the other hand does a comparison of state to determine equality.
        System.out.println(s1 == s2);//False
        System.out.println(s1.equals(s2));//True
        System.out.println(s3 == s4);//True
        System.out.println(s4 == s5);//False
        System.out.println(s5 == s5);//True

        System.out.println();
        System.out.println("Doing operations....");

        // ループ自体に連結演算がある場合、つまり =, += のような場合は、不変の配列ではなく文字配列を使用する方が良いでしょう。

        // 文字列の反転 O(n^2)
        String reverseS = "";
        for(int i = s5.length()-1; i >= 0; i--){
            // O(n)
            reverseS += s5.charAt(i);
        }
        System.out.println(s5);
        System.out.println(reverseS);

        // 文字の配列の使用
        // 文字列の反転 O(n)
        char[] reverseChar = new char[s5.length()];
        int l = s5.length()-1;
        for(int i = l; i >= 0; i--){
            // O(1)
            reverseChar[l-i] = s5.charAt(i);
        }

        String reverseS2 = new String(reverseChar);
        System.out.println(reverseS2);
        System.out.println(reverseS2.equals(reverseS));//true
    }
}
```

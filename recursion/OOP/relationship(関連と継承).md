# relationship

- クラスとオブジェクトの関係性。関連、集約、継承(ソフトウェアの再利用性に役立つ)

### association(関連)

- 2 つの class 間の関係。双方向か単方向か。
  例 01:
  会社 class<-従業員 class //従業員が会社のために働いているから単方向。
  会社が従業員のデータへアクセスできる<->従業員が会社へアクセスできる->双方向性
  例 02\*:
  単方向性: person Class -> 財布 Class// 財布 Class !-> person Class

```java
class Wallet{
//お札の単位
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

    public Person(String firstName, String lastName, int age, double heightM, double weightKg){
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
        this.heightM = heightM;
        this.weightKg = weightKg;
        this.wallet = new Wallet();
    }

    public int getCash(){
        if(this.wallet == null) return 0;
        return this.wallet.getTotalMoney();
    }

    public void printState(){
        System.out.println("firstname - " + this.firstName);
        System.out.println("lastname - " + this.lastName);
        System.out.println("age - " + this.age);
        System.out.println("height - " + this.heightM);
        System.out.println("weight - " + this.weightKg);
        System.out.println("Current Money - " + this.getCash());
        System.out.println();
    }
}

class Main{
    public static void main(String[] args){
        Person p = new Person("Ryu","Poolhopper", 40, 1.8, 140);
        //Walletはどうやって定義されている???
        p.printState();

        p.wallet.insertBill(5,3);
        p.wallet.insertBill(100,2);

        p.printState();
    }
}

// firstname - Ryu
// lastname - Poolhopper
// age - 40
// height - 1.8
// weight - 140.0
// Current Money - 0

// firstname - Ryu
// lastname - Poolhopper
// age - 40
// height - 1.8
// weight - 140.0
// Current Money - 215
```

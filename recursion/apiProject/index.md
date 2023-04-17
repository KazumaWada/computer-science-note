# JSON

- 基本

```json
// JSONはkeyとvalueによって構成されます。
const jsonString = `
    {
        "model": "Tesla X",
        "brand": "Tesla",
        "price": "$100k",
        "year": 2018
    }
`;

// JSON.parse()メソッドは文字列をJSONとして解析し、文字列によって記述されているJavaScriptの値やオブジェクトを構築します。
const car = JSON.parse(jsonString);

// carという変数とアクセス演算子を用いて、JSONのデータにアクセス
console.log(car.model);//Tesla X
console.log(car.year);//2018
```

- 配列の index でも取ってこれる

```json
const jsonString = `
    [{
        "model": "Tesla X",
        "brand": "Tesla",
        "price": "$100k",
        "year": 2018
    },
    {
        "model": "Civic",
        "brand": "Honda",
        "price": "$30k",
        "year": 2016
    },
    {
        "model": "Cayenne",
        "brand": "Porsche",
        "price": "$80k",
        "year": 2020
    }]
`;

const cars = JSON.parse(jsonString);
console.log(cars);
console.log(cars[0]);
console.log(cars[0].model);
console.log(cars[2].brand);
```

-

```json
const jsonString = `
    [{
        "model": "Tesla X",
        "brand": "Tesla",
        "price": "$100k",
        "year": 2018
    },
    {
        "model": "Civic",
        "brand": "Honda",
        "price": "$30k",
        "year": 2016
    },
    {
        "model": "Cayenne",
        "brand": "Porsche",
        "price": "$80k",
        "year": 2020
    }]
`;

const cars = JSON.parse(jsonString);

// for-of構文を使って、配列の全要素のJSONデータにアクセスを行うことができます。
// ドキュメンテーションを読むことをおすすめします。
// https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/for...of
for (let car of cars) {//forで全てインデックスずつ回している。
    console.log(car);//インデックスの要素全て取得
    console.log(car.model);//インデックスのmodel要素取得
    console.log(car.brand);
    console.log(car.price);
    console.log(car.year);
}
// { model: 'Tesla X', brand: 'Tesla', price: '$100k', year: 2018 }
// Tesla X
// Tesla
// $100k
// 2018
// { model: 'Civic', brand: 'Honda', price: '$30k', year: 2016 }
// Civic
// Honda
// $30k
// 2016
// { model: 'Cayenne', brand: 'Porsche', price: '$80k', year: 2020 }
// Cayenne
// Porsche
// $80k
// 2020
```

- ネスト構造

```json
const jsonString = `
    {
        "TeslaX" : {
            "brand": "Tesla",
            "price": "$100k",
            "year": 2018
        },
        "Civic" : {
            "brand": "Honda",
            "price": "$30k",
            "year": 2016
        },
        "Cayenne" : {
            "brand": "Porsche",
            "price": "$80k",
            "year": 2020
        }
    }
`;
const cars = JSON.parse(jsonString);

// Tesla Xの値段にアクセス
console.log(cars.TeslaX.price);

// Cayenneのブランドにアクセス
console.log(cars.Cayenne.brand);
```

- ネストされた JSON データを配列に保存(上のコードを[]でくくった。)

```json
const jsonString = `
    [
        {
            "TeslaX" : {
                "brand": "Tesla",
                "price": "$100k",
                "year": 2018
            }
        },
        {
            "Civic" : {
                "brand": "Honda",
                "price": "$30k",
                "year": 2016
            }
        },
        {
            "Cayenne" : {
                "brand": "Porsche",
                "price": "$80k",
                "year": 2020
            }
        }
    ]
`;

const cars = JSON.parse(jsonString);

// Tesla Xの値段にアクセス
console.log(cars[0].TeslaX.price);

// Cayenneのブランドにアクセス
console.log(cars[2].Cayenne.brand);
```

# fetch(取ってくる)

- サーバからデータを取ってきて、コンソールに表示する。
- URL と初期データを受け取る非同期関数(サーバーからデータを取ってくる際、同期だと取ってくるまで全てのコードはそれを待たなくてはいけないから。)

## promise

非同期の処理がせいこうしたかどうかを知らせる関数。

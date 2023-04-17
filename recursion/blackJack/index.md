```javascript
class Card {
  /*
       String suit : {"H", "D", "C", "S"}から選択
       String rank : {"A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"}から選択
    */
  constructor(suit, rank) {
    // スート
    //this.suit = suit
    // ランク
    //this.rank = rank
  }

  /*
       return Number : カードのランクを基準とした整数のスコア。
       
        2-10はそのまま数値を返します。
    　  {"J", "Q", "K"}を含む、フェースカードは10を返します。
        "A」が1なのか11なのかを判断するには手札全体の知識が必要なので、「A」はとりあえず11を返します。
    */
  //"Deck.getHandScore()で使用するカードのランクの数値を取得"
  getRankNumber(card) {
    let hashmap = {};
    let rank = [
      "A",
      "2",
      "3",
      "4",
      "5",
      "6",
      "7",
      "8",
      "9",
      "10",
      "J",
      "Q",
      "K",
    ];
    for (let i = 1; i < rank.length; i++) {
      //J,Q,Kは10とする。
      if (i == 11 || i == 12 || i == 13) hashmap[card[i]] = 10;
      //Aはとりあえず11を返す。
      if (i == 1) hashmap[card[i]] = 11;
      else hashmap[rank[i]] = i;
    }
    return hashmap[card];
    // 今のところ、"A"のランクを11と数えていますが、"A"が1なのか11なのかを区別するには、手札全体を把握しておく必要があります。
  }
}

class Deck {
  /*
       String gameType : ゲームタイプの指定。{'blackjack'}から選択。
    */
  constructor(gameType) {
    // このデッキが扱うゲームタイプ
    this.gameType = gameType;
    // カードの配列//初期化
    this.cards = [];
  }

  /*
       return null : このメソッドは、デッキの状態を更新します。

       カードがランダムな順番になるようにデッキをシャッフルします。
    */
  shuffle(cards) {
    //1以上55未満
    return Math.floor(Math.random() * 55) + 1;
  }

  /*
       String gameType : どのゲームにリセットするか
       return null : このメソッドは、デッキの状態を更新します。
    

    */
  resetDeck(cards) {
    //TODO: ここから挙動をコードしてください。
    //初期化
    cards = [];
    //52枚をまた追加する。
    for (let i = 1; i < 53; i++) {
      cards.push(i);
    }
    return cards;
  }

  /*
       return Card : ポップされたカードを返します。
       カード配列から先頭のカード要素をポップして返します。
    */
  //なぜ仮引数を持たないのか分からない。
  // drawOne()
  // {
  //     //TODO: ここから挙動をコードしてください。

  // }
  drawOne(cards) {
    return cards.pop();
  }
}

class Player {
  /*
        String name : プレイヤーの名前
        String type : プレイヤータイプ。{'ai', 'user', 'house'}から選択。
        String gameType : {'blackjack'}から選択。プレイヤーの初期化方法を決定するために使用されます。
        ?Number chips : ゲーム開始に必要なチップ。デフォルトは400。
    */
  constructor(name, type, gameType, chips = 400) {
    // プレイヤーの名前
    //this.name = name;

    // プレイヤーのタイプ
    //this.type = type;

    // 現在のゲームタイプ
    //this.gameType = gameType;

    // プレイヤーの手札
    this.hand = [];

    // プレイヤーが所持しているチップ。
    //this.chips = chips;

    // 現在のラウンドでのベットしているチップ
    //this.bet = 0

    // 勝利金額。正の数にも負の数にもなります。
    //this.winAmount = 0

    // プレイヤーのゲームの状態やアクションを表します。
    // ブラックジャックの場合、最初の状態は「betting」です。
    this.gameStatus = "betting";
  }

  /*
       ?Number userData : モデル外から渡されるパラメータ。nullになることもあります。
       return GameDecision : 状態を考慮した上で、プレイヤーが行った決定。

        このメソッドは、どのようなベットやアクションを取るべきかというプレイヤーの決定を取得します。プレイヤーのタイプ、ハンド、チップの状態を読み取り、GameDecisionを返します。パラメータにuserData使うことによって、プレイヤーが「user」の場合、このメソッドにユーザーの情報を渡すことができますし、プレイヤーが 「ai」の場合、 userDataがデフォルトとしてnullを使います。
    */
  promptPlayer(userData) {
    //TODO: ここから挙動をコードしてください。
  }

  /*
       return Number : 手札の合計

       合計が21を超える場合、手札の各エースについて、合計が21以下になるまで10を引きます。
    */
  getHandScore() {
    //TODO: ここから挙動をコードしてください。
  }
}

class GameDecision {
  /*
       String action : プレイヤーのアクションの選択。（ブラックジャックでは、hit、standなど。）
       Number amount : プレイヤーが選択する数値。

       これはPlayer.promptPlayer()は常にreturnする、標準化されたフォーマットです。
    */
  constructor(action, amount) {
    // アクション
    //this.action = action
    // プレイヤーが選択する数値
    //this.amount = amount
  }
}
```

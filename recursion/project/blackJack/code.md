```javascript
//Deck.getHandScore()で使用するカードのランクの数値を取得します。
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
      if (i == 11 || i == 12 || i == 13) hashmap[card[i]] = 10;
      if (i == 1) hashmap[card[i]] = 11; //Aはとりあえず11を返す。
      else hashmap[rank[i]] = i;
    }
    return hashmap[card];
    //TODO: ここから挙動をコードしてください。
    //Number getRankNumber():
    // Deck.getHandScore()で使用するカードのランクの数値を取得します。
    // 今のところ、"A"のランクを11と数えていますが、"A"が1なのか11なのかを区別するには、手札全体を把握しておく必要があります。
  }
}
```

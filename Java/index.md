# HTTPレスポンス
ステータスコード、ヘッダー、ボディ
この3つが含まれたデータのこと。
ステータスコードは必ず含まれるが、ヘッダー、ボディーは省略されることもある(ex, 204)

# ResponseEntity
HTTPレスポンス(ステータスコード、ヘッダー、ボディー)を1つのオブジェクトとしてまとめたクラス。

# ジェネリクス
<T>: ここになんでも後から入れれるように、変数としておいてある。
ジェネリクスという仕組みを使って作られたクラスたち。
ResponseEntity<hogehoge>, 
optional<T>:  

```java
// つまりこれは、ジェネリクス<...>の中にHTTPレスポンスのボディーを書いている。
public ResponseEntity<MendanLogFeedbacksResponse> show(...) {
```


# ❯ return ResponseEntity.noContet().build(); なんでこれがokでreturn ResponseEntity.noContet.build();これがダメなの？

// noContentという変数を探すが存在しないため。
ResponseEntity.noContent.build()

// それぞれのメソッドが実行される。(ResponseEntityクラスのnoteContentメソッドが呼び出した先のbuildメソッド。入れ子ではない。)
// メソッドチェーンは基本的に入れ子ではなくて、メソッドごとにバトン渡しで繋がっている。
ResponseEntity.noContent().build()

例えばこんな感じ:
```java
list.stream()      // ① Listに対してstream()を呼ぶ → Streamオブジェクトが返る
    .filter(x -> x > 0)  // ② Streamに対してfilterを呼ぶ → 別のStreamオブジェクトが返る
    .map(x -> x * 2)      // ③ さらに別のStreamオブジェクトが返る
    .collect(Collectors.toList()); // ④ Streamに対してcollectを呼ぶ → Listが返る

```


# デバック
## 種類

- logger: 層ごと

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class MendanLogsController {
  // 定義
  private static final Logger log = LoggerFactory.getLogger(MendanLogsController.class);


  public ResponseEntity<List<MendanLogResponse>> list(@AuthenticationPrincipal Jwt jwt) {
    // 使用
    log.info("jwt claims: {}", jwt.getClaims());
    Long companyId = jwt.getClaim("company_id");
    // 使用 companyIdという変数がプレースホルダー{}の中に入って表示されるようになっている。
    log.info("company_id: {}", companyId);
```

- IDEのデバック
dockerの場合は設定がやや面倒だが、これが一番強力。

- System.out.println(
一箇所だけすぐに見たい時など。
loggerはその場所の具体的なクラス名、本番では表示しない切り替えなどが備わっているが、それ以外はまあ一緒。

- HTTPレベルでの可視化
```java
logging:
  level:
    //  普段は省略されているSpring Securityのログをこれで表示させることができるようになる。
    org.springframework.security: DEBUG
    // これも、securityが通った後に正しくcontrollerにroutingされたかどうかを見ることができるようになる。
    org.springframework.web: DEBUG
```

6. テストコード（MockMvc / 単体テスト）


- 方法
直接そのログが貼ってある場所のAPIを叩いて、発火させて出力させる必要がある。


## loggerの実際のデバック方法

```java
package com.kodamaize.mendan.api.v1.controller.mendan_log;


import lombok.extern.slf4j.Slf4j;

// ...import省略

// LEARNING: これでprivate static final Logger log = LoggerFactory.getLogger(...)を書かなくていい。
@Slf4j
@RestController
@RequestMapping("/api/v1/mendan/mendan-logs")
public class MendanLogsController {
  private final MendanLogService mendanLogService;

  // 省略

  @PatchMapping("/{id}")
  public ResponseEntity<MendanLogResponse> edit(@PathVariable Long id, @AuthenticationPrincipal Jwt jwt){
    Long company_id = jwt.getClaim("company_id");
    // 方法1: これでもいい。
    System.out.println("company_idはこれだよ👉" + company_id);
    // 方法2: 今回はこれを使用。
    log.info("company_id = {}", company_id);
  }

実際の出力結果
---------------------------------------------------------------------------------
api-1  | 2026-08-29T02:58:17.365Z  INFO 456 --- [mendan] [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 1 ms
api-1  | company_idはこれだよ👉1
api-1  | 2026-08-29T02:58:17.456Z  INFO 456 --- [mendan] [nio-8080-exec-1] c.k.m.a.v.c.m.MendanLogsController       : company_id = 1


```


























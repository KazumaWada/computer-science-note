# C001(10/8/2026)

```c
#include <stdio.h>

int main() {
  printf("Program in C!");
  return 0;// 0: "nothing bad happened"
} 


// TOPIC: startが出力される前にエラーになる。コンパイルする言語だから。 //
// interprited: 動的 compiled: 静的
print("starting")
func_that_doesnt_exist("uh oh")
print("finished")

// TOPIC: commentsの書き方 //
// This is a single-line comment

/*
This is a multi-line comment
I can just keep adding lines
and it will still be a comment
*/

// TOPIC: types //
#include <stdio.h>

int main() {
  int max_recursive_calls = 100;
  char io_mode = 'w';
  float throttle_speed = 0.2;

  // don't touch below this line
  printf("Max recursive calls: %d\n", max_recursive_calls);
  printf("IO mode: %c\n", io_mode);
  printf("Throttle speed: %f\n", throttle_speed);
  return 0;
}

// TOPIC: String in C //
#include <stdio.h>

int main() {
char  *will_never_hear_again =
      "Hey TJ, when is the memory course in C gonna be done?";

  // don't touch below this line
  printf("%s\n", will_never_hear_again);
  return 0;
}

// TOPIC:  変数をPrintfするとき //

/*
%d – digit (integer)
%c – character
%f – floating point number
%s – string (char *)
*/

#include <stdio.h>

int main() {
  int sneklang_default_max_threads = 8;
  char sneklang_default_perms = 'r';
  float sneklang_default_pi = 3.141592;
  char *sneklang_title = "Sneklang";
  // don't touch above this line

  printf("Default max threads: %d\n", sneklang_default_max_threads);
  printf("Custom perms: %c\n", sneklang_default_perms);
  printf("Constant pi value: %f\n", sneklang_default_pi);
  printf("Sneklang title: %s\n", sneklang_title);
  
  return 0;
}

// TOPIC: Cは変数を一回定義したら、それを変更することは許されない(他の言語でも変えるのはありえないけど可能)
int main() {
    char *max_threads = "5";

    // call badcop
    // this is illegal
    max_threads = 5;
}

```

# C002(10/11/2026)

```C
// TOPIC: constの書き方

int main() {
    int x = 5;
    x = 10; // this is ok
}

int main() {
    const int x = 5;
    x = 10; // error
}

// TOPIC: fucntionの書き方
#include <stdio.h>

// ?
int max_sneklang_memory(int max_threds, int memory_per_thread){
  return (int)(memory_per_thread * max_threds);
}

// don't touch below this line

void init_sneklang(int max_threads, int memory_per_thread) {
  printf("Initializing Sneklang\n");
  printf("Max threads: %d\n", max_threads);
  printf("Memory per thread: %d\n", memory_per_thread);
  int max_memory = max_sneklang_memory(max_threads, memory_per_thread);
  printf("Max memory: %d\n", max_memory);
  printf("====================================\n");
}

int main() {
  init_sneklang(4, 512);
  init_sneklang(8, 1024);
  init_sneklang(16, 2048);
  return 0;
}

// TOPIC: 平均を出す関数の書き方
#include "exercise.h"
#include <stdio.h>
  
float get_average(int x, int y, int z){
  float result = (x + y + z) / 3.0;
  printf("%f\n", result);  
  return result;  
}

// TOPIC: 条件分岐
#include "exercise.h"
#include <stdio.h>

char *get_temperature_status(int temp){
  if(temp < 70){
    return "too cold";
  }else if(temp > 90){
    return "too hot";
  }else{
    return "just right";
  }
};

// TOPIC: If statementでは、return: 1がtruedで0がfalseになっている。
#include "exercise.h"

int can_access_registry(int is_premium, int reputation, int has_2fa){
  if(is_premium == 1 || reputation >= 100 && has_2fa == 1){
    return 1;
  }
  return 0;
};

// TOPIC: それぞれのtypeのサイズが見れる
#include <stdbool.h>
#include <stdio.h>

int main() {
  // Use %zu for printing `sizeof` result
  printf("sizeof(char)   = %zu\n", sizeof(char));
    printf("sizeof(bool)   = %zu\n", sizeof(bool));
  printf("sizeof(int)   = %zu\n", sizeof(int));
    printf("sizeof(float)   = %zu\n", sizeof(float));

  printf("sizeof(double)   = %zu\n", sizeof(double));
  printf("sizeof(size_t)   = %zu\n", sizeof(size_t));
}


sizeof(char)   = 1
sizeof(bool)   = 1
sizeof(int)   = 4
sizeof(float)   = 4
sizeof(double)   = 8
sizeof(size_t)   = 4
```

# C003(10/13/2026)

```C

// TOPIC: Loop

#include <stdio.h>
void print_numbers(int start, int end);


void print_numbers(int start, int end){
  for(int i = start; i <= end; i++){
    printf("%d\n", i);
  }
}

// TOPIC: while loop
#include <stdio.h>

void print_numbers_reverse(int start, int end){
  int i = start;
  while(i >= end){
    printf("%d\n", i);
    i--;
  }
  
}

// TOPIC: struct(データの塊。JSでいうオブジェクトみたいな感じ。)挙動はない。ただのデータの集まりだけ。
// structureを短くしただけのもの。だからそのまま「構造」」の意味になっている。

struct Human {
    int age;
    char *name;
    int is_alive;
};

// TOPIC: structにデータを入れるときの書き方
#include "coord.h"

struct Coordinate new_coord(int x, int y, int z) {
return (struct Coordinate){.x = x, .y = y, .z = z};
}

// または
struct Coordinate new_coord(int x, int y, int z) {
  struct Coordinate c;
  c.x = x;
  c.y = y;
  c.z = z;
  return c;
}

```

# C004(10/21/2026)

```c
#pragma once

struct Coordinate {
  int x;
  int y;
  int z;
};

struct Coordinate new_coord(int x, int y, int z);

struct Coordinate scale_coordinate(struct Coordinate coord, int factor);

↓

#include "coord.h"

struct Coordinate new_coord(int x, int y, int z) {
  struct Coordinate coord = {.x = x, .y = y, .z = z};
  return coord;
}

struct Coordinate scale_coordinate(struct Coordinate coord, int factor) {
  struct Coordinate scaled = coord;
  scaled.x *= factor;
  scaled.y *= factor;
  scaled.z *= factor;
  return scaled;
}

// 一度宣言したらstructを省略して書くことができる。typedef struct .. coordinate_t
#pragma once

typedef struct {
  int x;
  int y;
  int z;
} coordinate_t;

coordinate_t new_coord(int x, int y, int z);
coordinate_t scale_coordinate(coordinate_t coord, int factor);

↓

#include "coord.h"

coordinate_t new_coord(int x, int y, int z) {
  coordinate_t coord = {.x = x, .y = y, .z = z};

  return coord;
}

coordinate_t scale_coordinate(coordinate_t coord, int factor) {
  coordinate_t scaled = coord;
  scaled.x *= factor;
  scaled.y *= factor;
  scaled.z *= factor;

  return scaled;
}

// 4byteごとにpaddingで調節している。1byteだったら3byte分paddingが作成されて調節される。
// (https://en.wikipedia.org/wiki/Data_structure_alignment)


```

# C005(10/22/2026)
```C
// なぜPaddingが存在しているのか
- CPUがメモリを読む。
- そしてCPUはメモリを単位ごとにしか読まない。1byteずつ読むわけではない。(4byteずつ、8byteずつ etc..)
- だからメモリにデータが入った時に、次の単位までの空きがある場合、paddingを入れてCPUが次のデータを読めるようにしている

番地4から置いた場合(区切りに揃っている)
番地:   0  1  2  3 | 4  5  6  7
                     [  int   ]   ← 1回読めば取れる

番地1から置いた場合(区切りをまたいでいる)
番地:   0  1  2  3 | 4  5  6  7
           [  int  ...  ]          ← 2回読んで、つなぎ合わせる必要がある

区切りをまたぐと、次のどれかになります。
- 2回読んでつなぎ合わせることになり、遅くなる
- CPU によってはそもそも読めず、プログラムが落ちる(古い ARM など)

↓
だからコンパイラが自動でずらす。

、、つまり、メモリを少し無駄にする代わり、CPUが高速で早く読めるようにしたものがpadding

// なぜCPUは「区切り単位」でしか読まないのか？
物理的な作りの話で、CPUとメモリをつなぐハードウェアが、「区切り単位」で運ぶように最初から作られているから。
「ハードはシンプルで速くしておいて、置き場所の調整はソフト側(コンパイラ)に任せる。paddingで」

> Compilers + modern hardware + optimizations + skill issues means that sometimes what you think the computer is going to do isn't exactly what it actually does. That said, C is designed to get you close to the machine and allows you to dig in and figure out what's going on if you want to for a specific compiler or architecture.

> As a rule of thumb, ordering your fields from largest to smallest will help the compiler minimize padding
経験則として、大きいサイズの型から、小さいサイズの型の順番に並べると、paddingが少ない数使われて良い。
大体大きい型は、CPUが読むサイズになっている塊のサイズになっていることが多いから。(8,4,2,1Byte)

↓

typedef struct {
  char* a;
  double b;
  char c;
  char d;
  long e;
  char f;
} poorly_aligned_t;

typedef struct {
  double b;
  long e;
  char* a;
  char c;
  char d;
  char f;
} better_t;


// TOPIC: Memory
// &: このデータが入っているメモリのアドレスを表示することができる。
// %p: pointer
#include <stdio.h>

int main() {
  int age = 37;
  printf("The address of age is: %p\n", (void *)&age);
  return 0;
}
// The address of age is: 0xfff8
```

# C006(10/23/2026)
```c
// &でアドレスの位置を取得して、

// *
  char* a;
  double b;
  char c;
  char d;
  long e;
  char f;
// Cには「文字列」という概念がないから、charだとメモリにある最初のaddressを取得してきて持っておくように書かれている。(そもそもcharには一文字しか入らない)


#include "snek.h"
#include <stdio.h>
// boot.devでは、住所（ポインタ）が4Byteで格納されることになっている。
// そしてiのaddressをpで取得している。
// だから結局returnでpを見ると毎回4byteになる。
// 全く意味がわからない関数になっているけど、これは学習用に彼らが設定していることだから仕方がない。
unsigned long size_of_addr(long long i) {
  long long *p = &i;
  return sizeof(p);
}

// Addressに書かれている数字の話
- addressはただの数字。でもhexadecimalって記法で書かれているから、「0xfff8」みたいな書き方になる。
見慣れている数字に直すと65,528になる。

// Virtual Memory
- 自分が書いたコードは直接RAMにアクセスはせずにOSが指示してVirtual Memoryが間に挟まっている。
- Physivcal Memory: 実際にRAMに書かれているメモリのデータ。
- でもCでOSを返さなかったら、直接RAMを触っているという例外も存在する。

コード->OS
コード<-OS(コイツやばい返す)
コード->VM->RAM

OSがやってるcoolなこと
- メモリは使っていたら、必ず他のメモリを割り当てる
- 特定の大事なメモリにはアクセスさせない。
- 開発者はこれらのことを一切気にしなくていい。
- メモリにただ入れているわけではなくて、調節しながらよしなにやってくれる。


// Pointer
Put simply: a pointer is just a variable that stores a memory address. 
pointerはメモリのアドレスを指した変数。シンプル。指してる。ポインティングしてる。シンプル。

```
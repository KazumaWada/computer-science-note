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


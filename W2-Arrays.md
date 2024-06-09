# Class 2. Array

# 課程與連結
- [CS50 Array](https://cs50.harvard.edu/x/2024/weeks/2/)

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/4vU4aEFmTSo/0.jpg)](https://www.youtube.com/watch?v=4vU4aEFmTSo)
# 目錄：

# 1. Compile

> 資料與圖片來源：[Compiling C files with gcc, step by step](https://medium.com/@laura.derohan/compiling-c-files-with-gcc-step-by-step-8e78318052)


![[2. Array-20240413165721603.png]]

## 1-1 Compile 4 階段
Compile 有以下4個階段
- The preprocessing
- The compiling
- The assembling
- The linking

### Step 0. Source Code
在這裡指 `.c` file，代表c的原始檔案，ex:
```c
#include <stdio.h>

// Macro definition
#define LIMIT 5

/*
 * I am comment
 */
int main(int argc, char **argv) {
    for (int i = 0; i < argc; i++) {
        printf("%s ", argv[i]);
    }
    printf("\n");

    // Print the value of macro defined
    printf(
        "The value of LIMIT"
        " is %d",
        LIMIT);
    return 0;
}
```

### Step 1. pre-processing
preprocessor執行以下步驟：
- 去除comment
- include header file (`#include <stdio.h>`)
- 將macro (`#define LIMIT 5`)直接用原本的數字取代文字
- 產出文件為`.i`

> 指令：

```
gcc -E main.c
```

```c
# 更上面還會有stdio.h的東西
# 2 "argc.c" 2

# 9 "argc.c"
int main(int argc, char **argv) {
    for (int i = 0; i < argc; i++) {
        printf("%s ", argv[i]);
    }
    printf("\n");

    printf(
        "The value of LIMIT"
        " is %d",
        5); // 這裡的limit變成5了
    return 0;
}
```

### Step2. Compile
compiler執行以下步驟：
- 產生IR code (Intermediate Representation)
- 生成`.s`檔案
- 有的compiler會直接轉出assembly code


> 指令：

```
gcc -S main.c
```

```assemble
	.file	"argc.c"
	.text
	.section	.rodata
.LC0:
	.string	"%s "
.LC1:
	.string	"The value of LIMIT is %d"
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	endbr64
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	subq	$32, %rsp
	movl	%edi, -20(%rbp)
	movq	%rsi, -32(%rbp)
	movl	$0, -4(%rbp)
	jmp	.L2
.L3:
	movl	-4(%rbp), %eax
	cltq
	leaq	0(,%rax,8), %rdx
	movq	-32(%rbp), %rax
	addq	%rdx, %rax
	movq	(%rax), %rax
	movq	%rax, %rsi
	leaq	.LC0(%rip), %rax
	movq	%rax, %rdi
	movl	$0, %eax
	call	printf@PLT
	addl	$1, -4(%rbp)
.L2:
	movl	-4(%rbp), %eax
	cmpl	-20(%rbp), %eax
	jl	.L3
	movl	$10, %edi
	call	putchar@PLT
	movl	$5, %esi
	leaq	.LC1(%rip), %rax
	movq	%rax, %rdi
	movl	$0, %eax
	call	printf@PLT
	movl	$0, %eax
	leave
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0"
	.section	.note.GNU-stack,"",@progbits
	.section	.note.gnu.property,"a"
	.align 8
	.long	1f - 0f
	.long	4f - 1f
	.long	5
0:
	.string	"GNU"
1:
	.align 8
	.long	0xc0000002
	.long	3f - 2f
2:
	.long	0x3
3:
	.align 8
4:

```

### Step3:  assembling
 assembler執行以下動作
 - 將IR code轉成object code
 - 生成`.o`檔
> 指令
```
gcc -c main.c
```

```
ELF����������>.....
```

### Step4: link
linker執行以下動作：
- linker會把所有source file link在一起(像是import 別的`.c`文件時會黏在一起)
- 將function call 與他們的definition連在一起，包含`static libraries`和 `dynamic libraries`
- 產生執行檔`a.out`

> 指令

```
gcc -o main.c
```

# 2. Array
每一種type在記憶體中都佔據一定空間，例如：
- `bool` 1 byte
- `int` 4 bytes
- `long` 8 bytes
- `float` 4 bytes
- `double` 8 bytes
- `char` 1 byte

Array就是許多相同的type依序排在一起，以下是syntex
```c
int array1[5];
int array2[] = {1, 2, 3, 4, 5};
int array3[5] = {1, 2, 3, 4, 5};
```
在 `int array1` 中`array1`其實是這一排數字中的第一個位置的pointer，如果使用 `array1[i]`的方法可以取值，意思是告訴程式我要取得`array1` 向後加上 `i * sizeof(int)`位置之後的那個值。

像是以下的code可以看到 `array[i]` 的數字的pointer其實和 `array + i` 一樣，而且每個pointer都會往後挪 4 bytes, 也就是int的大小


```c
#include <stdio.h>

int main() {
    int array[5] = {1, 2, 3, 4, 5};

    for (int i = 0; i < 5; i++) {
        printf("pointer of array[%d]: %p ", i, &array[i]);
        printf("pointer of array + %d: %p \n", i, array + i);
    }
}
```

```
pointer of array[0]: 0x7ffd897e6360 pointer of array + 0: 0x7ffd897e6360 
pointer of array[1]: 0x7ffd897e6364 pointer of array + 1: 0x7ffd897e6364 
pointer of array[2]: 0x7ffd897e6368 pointer of array + 2: 0x7ffd897e6368 
pointer of array[3]: 0x7ffd897e636c pointer of array + 3: 0x7ffd897e636c 
pointer of array[4]: 0x7ffd897e6370 pointer of array + 4: 0x7ffd897e6370 
```

# 3. String

string是一種由一堆 char 組成的array，並在最後面加上 `\0`表示結束 `\0`其實就是0，如下所示。而這些char是數字表示某個ascii字母

![](https://cs50.harvard.edu/x/2024/notes/2/cs50Week2Slide116.png)

像是以下會print出 `Hi!`
```c
#include <stdio.h>

int main(void)
{
    char c1 = 'H';
    char c2 = 'I';
    char c3 = '!';

    printf("%c%c%c\n", c1, c2, c3);
}
```

下面也可以print出 `Hi` 其中 `s[3]` 是 `\0`
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string s = "HI!";
    printf("%c%c%c\n", s[0], s[1], s[2]); //Hi!
    printf("%i %i %i %i\n", s[0], s[1], s[2], s[3]); // 72 73 33 0
}
```

## 3-1 String 長度
可以用  `<string.h>` 中的 `strlen`取出來，但要注意搭回傳的長度其實是long ( type `size_t`)
```c
#include <stdio.h>
#include <string.h>

int main(void) {
    char *s = "HI!";
    printf("%ln\n", strlen(s));
}
```

也可以自己寫while 到 `\0`停止
```c
#include <stdio.h>

int main(void)
{
    // Prompt for user's name
    char *name = "Hello";

    // Count number of characters up until '\0' (aka NUL)
    int n = 0;
    while (name[n] != '\0')
    {
        n++;
    }
    printf("%i\n", n);
}
```

## 3-2 `<ctype.c>`

以下是`<ctype.c>` 中的function，就可以不用自己寫

|Sr.No.|Function & Description|
|---|---|
|1|[int isalnum(int c)](https://www.tutorialspoint.com/c_standard_library/c_function_isalnum.htm)<br><br>This function checks whether the passed character is alphanumeric.|
|2|[int isalpha(int c)](https://www.tutorialspoint.com/c_standard_library/c_function_isalpha.htm)<br><br>This function checks whether the passed character is alphabetic.|
|3|[int iscntrl(int c)](https://www.tutorialspoint.com/c_standard_library/c_function_iscntrl.htm)<br><br>This function checks whether the passed character is control character.|
|4|[int isdigit(int c)](https://www.tutorialspoint.com/c_standard_library/c_function_isdigit.htm)<br><br>This function checks whether the passed character is decimal digit.|
|5|[int isgraph(int c)](https://www.tutorialspoint.com/c_standard_library/c_function_isgraph.htm)<br><br>This function checks whether the passed character has graphical representation using locale.|
|6|[int islower(int c)](https://www.tutorialspoint.com/c_standard_library/c_function_islower.htm)<br><br>This function checks whether the passed character is lowercase letter.|
|7|[int isprint(int c)](https://www.tutorialspoint.com/c_standard_library/c_function_isprint.htm)<br><br>This function checks whether the passed character is printable.|
|8|[int ispunct(int c)](https://www.tutorialspoint.com/c_standard_library/c_function_ispunct.htm)<br><br>This function checks whether the passed character is a punctuation character.|
|9|[int isspace(int c)](https://www.tutorialspoint.com/c_standard_library/c_function_isspace.htm)<br><br>This function checks whether the passed character is white-space.|
|10|[int isupper(int c)](https://www.tutorialspoint.com/c_standard_library/c_function_isupper.htm)<br><br>This function checks whether the passed character is an uppercase letter.|
|11|[int isxdigit(int c)](https://www.tutorialspoint.com/c_standard_library/c_function_isxdigit.htm)<br><br>This function checks whether the passed character is a hexadecimal digit.|

## 3-3 備註: 手寫getString
以下是問chatgpt後獲得一個蠻好用的getString的function，但記得使用完之後要手動free sting 的記憶體。

這個function可以接受 `1024`的字長，並且get 到字之後會縮短
```c
char* getString(char* consoleWording) {
    char* s;
    s = malloc(1024 * sizeof(char));  // 為字符串分配足夠的空間
    if (s == NULL) {
        printf("Memory allocation failed\n");
        return NULL;  // 如果分配失敗，返回 NULL
    }

    printf("%s", consoleWording); // 給使用者在cmd中看的字

    // 使用 fgets 代替 scanf
    // fgets 讀取一行，直到遇到換行符或達到緩衝區限制
    // stdin 表示標準輸入
    if (fgets(s, 1024, stdin) == NULL) {
        free(s);  // 如果讀取失敗，釋放記憶體並返回 NULL
        return NULL;
    }

    // 移除可能讀取的換行符
    // strcspn 找到第一個出現的文字偏移量
    s[strcspn(s, "\n")] = 0;

    // 根據實際讀取的內容大小重新分配記憶體
    s = realloc(s, strlen(s) + 1);
    if (s == NULL) {
        printf("Memory reallocation failed\n");
        return NULL;  // 如果重新分配失敗，返回 NULL
    }

    return s;
}
```

## 4. Command-Line Arguments

用 `int argc, chat *argv[]` 在 `main()`中可以取得使用者的輸入，如下：
```c
#include <stdio.h>

int main(int argc, chat *argv[])
{
    if (argc == 2)
    {
        printf("hello, %s\n", argv[1]);
    }
    else
    {
        printf("hello, world\n");
    }
}
```

由於 argv指的是使用者在cmd中輸入的string字串，用空格分開，而string本身是 `char *`，因此`chat *argv[]`可以寫成 `char **argv`


```c
#include <stdio.h>


/*
 * I am comment
 */
int main(int argc, char **argv) {
    for (int i = 0; i < argc; i++) {
        printf("%s ", argv[i]);
    }
    printf("\n");
    return 0;
}
```

```
 ./a.out a b c <- 輸入
./a.out a b c <- 輸出
```

## 5. Exit Status
- C用return 0表示正常
- return其他數字表示不正常，也可以用這些數字來debug
```c
#include <cs50.h>
#include <stdio.h>

int main(int argc, string argv[])
{
    if (argc != 2)
    {
        printf("Missing command-line argument\n");
        return 1;
    }
    printf("hello, %s\n", argv[1]);
    return 0;
}
```

如果使用以下指令可以看到上一個程式離開時是什麼return code
```bash
echo $?
```
# 6. 作業

## 6-1 Scrabble
- [Scrabble](https://cs50.harvard.edu/x/2024/psets/2/scrabble/#scrabble)
題目：有兩組文字比賽，每個文字會值多少分存在 `scores` array, 大小寫不計，只紀錄字母，其他特殊符號不計，算哪斷文字分數高

```
Player 1: Question?                                                         Player 2: Question!                                                         Tie!                                                                        $ ./scrabble                                                                Player 1: red                                                               Player 2: wheelbarrow                                                       Player 2 wins!                                                              $ ./scrabble                                                                Player 1: COMPUTER                                                          Player 2: science                                                           Player 1 wins! 
```

計算分數的function (我是先寫作業所以沒有很會用 `#include <ctype>`)
```c
#include <ctype.h>
#include <string.h>
const int scores[] = {1, 3, 3, 2, 1, 4, 2, 4, 1, 8, 5, 1, 3, 1, 1, 3, 10, 1, 1, 1, 1, 4, 4, 8, 4, 10};
int wordToValue(char* word) {
    int score = 0;
    int length = strlen(word);
    int idxOfA = (int)'a'; // 這邊其實不用cast

    for (int i = 0; i < length; i++) {
        // int tolower(int argument); 本來就會把值轉成int
        int idx = tolower(word[i]) - idxOfA;
        if (idx < 0 || idx >= 26) {
            continue;
        }
        score += scores[idx];
    }
    return score;
}
```

## 6-2 Readability
- [Readability](https://cs50.harvard.edu/x/2024/psets/2/readability/#readability)
題目主旨是利用有多少字母, word, 句子來算出閱讀難度
- 字母指的是a-z, A-Z
- word 可以用 空格數量 + 1計算
- 句子用`{'!', '.', '?'}`做計算

> <math.h>要用 -lm指令 `gcc -o readability readability.c -lm`

```c
#include <ctype.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
int countStuff(char* text) {
    int length = strlen(text);
    int letters = 0;
    int words = 1;
    int sentences = 0;

    for (int i = 0; i < length; i++) {
        letters += isLetter(text[i]);
        words += isSpace(text[i]);
        sentences += isEndOfSentence(text[i]);
    }
    return getScore(letters, words, sentences);
}

int isLetter(char c) {
    int idx = tolower(c) - 'a';

    if (idx < 0 || idx >= 26) {
        return 0;
    }
    return 1;
}

int isSpace(char c) {
    return c == ' ';
}

int isEndOfSentence(char c) {
    char EFL[] = {'!', '.', '?'};

    for (int i = 0; i < sizeof(EFL) / sizeof(EFL[0]); i++) {
        if (c == EFL[i]) {
            return 1;
        }
    }
    return 0;
}

int getScore(int nOfLetters, int nOfwords, int nOfSentences) {
    float L = ((float)nOfLetters / nOfwords) * 5.88;
    float S = ((float)nOfSentences / nOfwords) * 29.6;
    float score = L - S - 15.8;
    int result = (int)round(score);
    return result;
};

```

## 6-3 Substitution
- [Substitution](https://cs50.harvard.edu/x/2024/psets/2/substitution/#substitution)
收到26個字母的轉換，將原本的文字轉會成encrypt的文字
- 需要檢查文字是否合規定
	- 要是26字母
	- 不可以有特殊符號
	- 字不可有重複
- 需要大小寫敏感

> 檢查輸入是否合規定，經過多次修改所以邏輯有點怪
```c
#include <ctype.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
// Check if the key is valid
int validateKey(char* key) {
    int keyLength = strlen(key);

    if (keyLength != 26) {
        printf("Key must contain 26 characters.\n");
        return 1;
    }

    for (int i = 0; i < keyLength; i++) {
        if (!isalpha(key[i])) {
            printf("Key must only contain alphabetic characters.\n");
            return 1;
        }

        // 變成大寫
        key[i] = toupper(key[i]);
    }

    // 如果有字母沒有出現，也是1 (和出現相同字母同一個意思)
    int A = 'A';

    for (int i = 0; i < 26; i++) {
        char targetAlpha = A + i;
        if (strchr(key, targetAlpha) == NULL) {
            printf("Key must not contain repeated characters.\n");
            return 1;
        }
    }

    char* plainText = getString("plaintext: ");
    stringEncipher(plainText, key);

    printf("ciphertext: %s\n", plainText);
    return 0;
}

```

> 把文字轉成加密文字
```c
#include <ctype.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
char charEncipher(char c, char* key) {
    if (!isalpha(c)) {
        return c;
    }
    int isLowerCase = islower(c);
    if (!isLowerCase) {
        c = tolower(c);
    }

    char cipher = key[c - 'a'];

    if (isLowerCase) {
        cipher = tolower(cipher);
    }
    return cipher;
}

void stringEncipher(char* text, char* key) {
    int textLength = strlen(text);
    for (int i = 0; i < textLength; i++) {
        text[i] = charEncipher(text[i], key);
    }
}
```

# 7. Grumpy Bookstore Owner
- Leetcode: [Grumpy Bookstore Owner](https://leetcode.com/problems/grumpy-bookstore-owner/description/)

```c
int maxSatisfied(int* customers, int customersSize, int* grumpy, int grumpySize, int minutes) {

    int alreadySatisfy = 0;
    for (int i = 0; i < customersSize; i++) {
        if (grumpy[i] == 0) {
            alreadySatisfy += customers[i];
            customers[i] = 0;
        }
    }

    int best = 0;
    int current = 0;

    for (int i = 0; i < customersSize; i++) {
        current += customers[i];
        if (i >= minutes) {
            current -= customers[i-minutes];
        }

        if (current > best) {
            best = current;
        }
    }
    return (best + alreadySatisfy);
}
```

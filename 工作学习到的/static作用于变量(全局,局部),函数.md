### static作用于变量(全局,局部),函数

C语言：static作用（修饰函数、局部变量、全局变量）

一、 static全局变量与普通的全局变量有什么区别 ?
　　全局变量(外部变量)的说明之前再冠以static 就构成了静态的全局变量。
　　  全局变量本身就是静态存储方式， 静态全局变量当然也是静态存储方式。 这两者在存储方式上并无不同。
　　  这两者的区别在于非静态全局变量的作用域是整个源程序， 当一个源程序由多个源文件组成时，非静态的全局变量在各个源文件中都是有效的。 而静态全局变量则限制了其作用域， 即只在定义该变量的源文件内有效， 在同一源程序的其它源文件中不能使用它。由于静态全局变量的作用域局限于一个源文件内，只能为该源文件内的函数公用，因此可以避免在其它源文件中引起错误。 
　　  static全局变量只初使化一次，防止在其他文件单元中被引用; 　 
二、static局部变量和普通局部变量有什么区别 ？
 　　把局部变量改变为静态变量后是改变了它的存储方式即改变了它的生存期。把全局变量改变为静态变量后是改变了它的作用域，限制了它的使用范围。  
       static局部变量只被初始化一次，下一次依据上一次结果值； 　 
三、static函数与普通函数有什么区别？
　　 static函数与普通函数作用域不同,仅在本文件。只在当前源文件中使用的函数应该说明为内部函数(static修饰的函数)，内部函数应该在当前源文件中说明和定义。对于可在当前源文件以外使用的函数，应该在一个头文件中说明，要使用这些函数的源文件要包含这个头文件.
　　static函数在内存中只有一份，普通函数在每个被调用中维持一份拷贝。
四、static的三条重要作用，首先static的最主要功能是隐藏，其次因为static变量存放在静态存储区，所以它具备持久性和默认值0。
       1、隐藏
          1.1当我们同时编译多个文件时，所有未加static前缀的全局变量和函数都具有全局可见性。为理解这句话，我举例来说明。我们要同时编译两个源文件，一个是static_extern.c，另一个是static_main.c。
          1.2static_main.c
#include<stdio.h>

void main(void)
{
	extern char i;    // extern variable must be declared before use
    printf("%c ", i);
    msg();
    return 0;
}
          1.3static_extern.c
char i = 'A'; // global variable
void msg() 
{
    printf("I Love Beijing!I Love hanyue!\n"); 
}
        1.4编译&执行

       1.5你可能会问：为什么在static_extern.c中定义的全局变量i和函数msg能在static_main.c中使用？前面说过，所有未加static前缀的全局变量和函数都具有全局可见性，其它的源文件也能访问。此例中，i是全局变量，msg是函数，并且都没有加static前缀，因此对于另外的源文件static_main.c是可见的。如果加了static，就会对其它源文件隐藏。例如在i和msg的定义前加上static，static_main.c就看不到它们了。利用这一特性可以在不同的文件中定义同名函数和同名变量，而不必担心命名冲突。Static可以用作函数和变量的前缀，对于函数来讲，static的作用仅限于隐藏。
      
     2、static的第二个作用是保持变量内容的持久。存储在静态数据区的变量会在程序刚开始运行时就完成初始化，也是唯一的一次初始化。共有两种变量存储在静态存储区：全局变量和static变量，只不过和全局变量比起来，static可以控制变量的可见范围，说到底static还是用来隐藏的。虽然这种用法不常见，但我还是举一个例子。
       2.1 static_main.c
#include <stdio.h>

int fun(void){
    static int count = 10;    // 事实上此赋值语句从来没有执行过
    return count--;
}

int count = 1;

int main(void)
{    
    printf("global\t\tlocal static\n");
    for(; count <= 10; ++count)
        printf("%d\t\t%d\n", count, fun());    
    
    return 0;
}
     2.2编译&执行

3、static的第三个作用是默认初始化为0。其实全局变量也具备这一属性，因为全局变量也存储在静态数据区。在静态数据区，内存中所有的字节默认值都是0x00，某些时候这一特点可以减少程序员的工作量。比如初始化一个稀疏矩阵，我们可以一个一个地把所有元素都置0，然后把不是0的几个元素赋值。如果定义成静态的，就省去了一开始置0的操作。再比如要把一个字符数组当字符串来用，但又觉得每次在字符数组末尾加’\0’太麻烦。如果把字符串定义成静态的，就省去了这个麻烦，因为那里本来就是’\0’。不妨做个小实验验证一下。
   3.1 static_main.c
#include <stdio.h>

int a;

int main(void)
{
    int i;
    static char str[10];

    printf("integer: %d;  string: (begin)%s(end)\n", a, str);
     
    return 0;
}
     2.2编译&执行

![img](https://img-blog.csdn.net/20180115155122107?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzc4NTgzODY=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

---------------------

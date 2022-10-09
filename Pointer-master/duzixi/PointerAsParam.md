# 指针和引用的传参区别

函数的参数应该传引用还是指针是个蛮令人纠结的问题。
纯个人经验，传引用成功率会更高。（如：文件对象ofile, ifile等）
本篇试图从原理上剖析该问题的根源。

## 指针与引用

### 相同点
都是地址的概念：指针指向一块内存，它的内容是所指内存的地址；而引用则是某块内存的别名。

### 不同点
* 指针是一个实体，而引用仅是个别名
* 引用只能在定义时被初始化一次，之后不可变；指针可变
* 引用没有const，指针有const，const的指针不可变
* 引用不能为空，指针可以为空；
* “sizeof 引用”得到的是所指向的变量(对象)的大小，而“sizeof 指针”得到的是指针本身的大小；
* 指针和引用的自增(++)运算意义不一样；
* 引用是类型安全的，而指针不是 (引用比指针多了类型检查）
* 编译原理不同

	程序在编译时分别将指针和引用添加到符号表上，符号表上记录的是变量名及变量所对应地址。
	指针变量在符号表上对应的地址值为指针变量的地址值，而引用在符号表上对应的地址值为引用对象的地址值。
	符号表生成后就不会再改，因此指针可以改变其指向的对象（指针变量中的值可以改），而引用对象则不能修改。

## 指针传参

本质上是【值传递】的方式，它所传递的是一个地址值。

【值传递】过程中，被调函数的形参作为被调函数的【局部变量】处理，即在栈中开辟了内存空间以存放由主调函数放进来的实参的值，从而成为了实参的一个副本。

【值传递】的特点是被调函数对形式参数的任何操作不会影响主调函数的实参变量的值。（实参指针本身的地址值不会变）
 

## 引用传参

被调函数的形式参数虽然也作为局部变量在栈中开辟了内存空间，但是这时存放的是由主调函数放进来的实参变量的地址。

被调函数对形参的任何操作都被处理成【间接寻址】，即通过栈中存放的地址访问主调函数中的实参变量。正因为如此，被调函数对形参做的任何操作都影响了主调函数中的实参变量。
 
'''C
 #include<iostream>

 using namespace std;
 
 // 值传递
 void passValue(int n){
     cout << "值传递--形参地址" << &n << endl;         //显示的是拷贝的地址而不是源地址 
     n++;
 }
 
 // 引用传递
void passRefer(int &n){
    cout << "引用传递--形参地址"<< &n << endl; 
    n++;
}

// 指针传递
void passPointer(int *n){
    cout << "指针传递--形参地址" << n << endl; 
    *n = *n + 1;
} 

int  main() {
    int n = 10;
    cout << "实参的地址" << &n << endl;
    passValue(n);
    cout << "after passValue() n=" << n << endl;
    passRefer(n);
    cout << "after passRefer() n=" << n << endl;
    passPointer(&n);
    cout << "after passPointer() n=" << n << endl;
    return true;
}
'''

## 小结

如果不用引用的话，被传递的参数本身是不能被修改的，  
即使你传递的是指针，也只能修改指针指向的内容，不能修改指针本身。  
如果要修改当前被传递的参数的话，要么再加一级指针，要么用引用。


## 参考文献

https://www.cnblogs.com/x_wukong/p/5712345.html
https://www.cnblogs.com/dingxiaoqiang/p/8012578.html
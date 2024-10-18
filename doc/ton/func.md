## func 

* [func 开发手册](https://docs.ton.org/mandarin/develop/func/cookbook)

### func 合约案例

* [func 智能合约](https://docs.ton.org/mandarin/develop/smart-contracts/examples)
* [在线教程demo ](https://github.com/romanovichim/TonFunClessons_Eng/blob/main/lessons/smartcontract/2lesson/secondlesson.md)

### 关键知识点

* 类型 
    * int： 257位有符号整数
    * cell： 持久数据存储在cell，最多1023位
    * slice: cell 转换为切片，用来读取数据
    * tuple: tvm 元组，有序集合，最大可容纳255个组件，有点类似数组的感觉: []
    * cont： 底层对象

* 没有bool类型，true=-1,false=0


* 字符串字面量 
    ```
        var a = "string value"u;
    ```
    * s: 定义原始slice常量
    * a：指定地址创建 MsgAddressInt 结构的slice常量
    * u： 创建 对应Ascii字符串的十六进制 int 常量
    * h： 创建对应字符串的 SHA256 哈西的 前32位的int常量
    * H： 创建对应字符串的 SHA256 哈西的 所有256位的int常量
    * c: 创建对应字符串的crc32 的int常量


* 变量

```c
int x = 2;
var x = 2;
(int, int) p = (1, 2);
(int, var) p = (1, 2);
(int, int, int) (x, y, z) = (1, 2, 3);
(int x, int y, int z) = (1, 2, 3);
var (x, y, z) = (1, 2, 3);
(int x = 1, int y = 2, int z = 3);
[int, int, int] [x, y, z] = [1, 2, 3];
[int x, int y, int z] = [1, 2, 3];
var [x, y, z] = [1, 2, 3];
```

* 判空 empty_slice.slice_empty?();

* 运算符
    * 一元运算
        * `~ x`是x的位的非运算
        * `- x` 是x的整数取反
    * 二元运算
        * 是整数乘法
        * / 是整数除法（向下取整）
        * ~/ 是整数除法（四舍五入）
        * ^/ 是整数除法（向上取整）
        * % 是整数取模运算（向下取整）
        * ~% 是整数取模运算（四舍五入）
        * ^% 是整数取模运算（向上取整）
        * /% 返回商和余数
        * & 是按位与

* 函数
    * int -> cell： 指的是接收一个int，并返回 cell
    * int -> (int,int): 接收一个整数，返回两个
    * `int add(int x, int y) asm "ADD";` 指的是 `int add(int x,int y)`的实现是汇编的`ADD`指令
    * `.` 与 `~`
        * `.` 非修改方法,前提是不发生状态改变
        ```c
            builder b = begin_cell(); 
            b = store_uint(b,100,8);
            // 与下面相同
            builder b = begin_cell().store_uint(100,8);
        ```
        * 修改方法 `~`
        ```c
            (cs,int x) = load_uint(cs,8);
            // 相同
            (cs, int x) = cs.load_uint(8);
            // 相同
            int x = cs~load_uint(8);
            // 以及如下用法
                (int, ()) inc(int x) {
                return (x + 1, ());
                }
            x~inc() 相当于调用了 inc函数
        ```
        * 函数名的 `.` 与 `~`

        ```c
            int inc(int x) {
                return x + 1;
            }
            (int, ()) ~inc(int x) {
                return (x + 1, ());
            }
            // 然后调用
            x~inc(); // 修改x
            int y = inc(x); // 不修改x
            int z = x.inc(); // 不修改x
        ```
        
        > 总结一下，当以非修改或修改方法（即使用 .foo 或 ~foo 语法）调用名为 foo 的函数时，如果存在 .foo 或 ~foo 的定义，FunC 编译器将分别使用 .foo 或 ~foo 的定义，如果没有，则使用 foo 的定义。



* 函数后面的修饰符 `impure`
    * impure 不纯函数，意味着可以修改状态
        * 比如修改存储空间、发送信息、抛出异常
    * inline 内联修饰符 
        * 被inline修饰的函数，编译之后，会尝试替换为具体的代码逻辑，从而抵消函数调用的开销
        * 不可递归
        * 代码膨胀，可能会导致代码体积过大
    * inline_ref 
        * 被修饰的函数，会把函数体存储在单独的cell内，从而减少了代码重复存储
        * 每次调用，通过`CALLREF`命令执行，相当于持有了这个函数的句柄
        * 适合代码量较大的函数
        * 不可递归
        * 存在一定调用开销（相比inline而言）

    * method_id
        * 可以给函数分配一个整数id ，在外部就可以对这个函数进行调用了


* forall 函数头部声明
    * 

## 编译指令

* #include 引入
* #pragma 
    * #pragma version 指定特定版本的FunC编译器

## 函数调用

* foo(int,int,int)
* bar (int)-> (int,int,int)

可以这么结合使用： for(bar(40))

*  函数调用中的 `.` 与 `~`


## 运算符

* `~` 是按位非（优先级 75）
* `-` 是整数取反（优先级 20）

--- 
* 是整数乘法
* / 是整数除法（向下取整）
* ~/ 是整数除法（四舍五入）
* ^/ 是整数除法（向上取整）
* % 是整数取模运算（向下取整）
* ~% 是整数取模运算（四舍五入）
* ^% 是整数取模运算（向上取整）
* /% 返回商和余数
* & 是按位与


## 循环

* repeat
    ```c
        // 这将会循环10遍
        repeat(10){
            x *=2;
        }
    ```
* while
    ```c
    while(x < 100) {
        x ++;
    }
    ```
* do {} until
    ```c
    do {
        x +=1;
    }until (x % 5 == 0);
    ```

## if 

```c
    ifnot(flag){
        // ...
    }

    if (x ==1){

    }else{

    }
```

## try catch 
* 所有状态都会被撤销，但是gas费用，等tvm状态参数，将不会回滚

```c
try {
  do_something();
} catch (x, n) {
    // x 是一场参数，n是错误代码（整数型）
  handle_exception();
}
```


## 转储

* 变量转储 ~dump
* 字符串转储 ~strdump

## 原语
* muldiv 先乘后除
* divmod 

* null? 是否为null 
* touch 或者 touch~ 是将变量移动到栈顶
* at 是获取指定位置的元祖值
* `::`双冒号用法
    * global int db::db_name 这是db作为一个域

## 抛异常

* throw_if
* throw_unless 


# Func 标准库
* Lisp 类型

## 发消息

* https://docs.ton.org/develop/smart-contracts/messages

```c
//  这是消息体
 cell msg = begin_cell()
    .store_uint(0x18, 6)
    .store_slice(addr)
    .store_coins(amount)
    // 1 = 空的extra-currencies字典。
    // 4 = ihr_fee和fwd_fee
    // created_lt = 64长度，create_at = 32长度
    // 1 没有init 1 消息就地序列化
    .store_uint(0, 1 + 4 + 4 + 64 + 32 + 1 + 1)
    .store_slice(message_body)
  .end_cell();
```
* 消息类型：
    * 外部消息
    * 内部消息
    * 日志
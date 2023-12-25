
1.

a）
<type_expression> ::= boolean | char | integer | real | type_error | void | <array_expression> | <cartesian_expression> | <record_expression> | <pointer_expression> | <function_expression>

<array_expression> ::= array <type_expression> <index_set>

<index_set> ::= <type_expression>

<cartesian_expression> ::= <type_expression> X <type_expression>

<record_expression> ::= record <field_list> end

<field_list> ::= <field> | <field> ; <field_list>

<field> ::= <identifier> : <type_expression>

<pointer_expression> ::= pointer <type_expression>

<function_expression> ::= <type_expression> -> <type_expression>

其中，<index_set> 是一个递归产生式，用于描述数组的索引集合。

b) 为了实现构造类型表达式对应的二进制编码，可以为每个类型表达式分配一个唯一的二进制编码。例如，可以为 boolean 分配二进制编码 0000，为 char 分配二进制编码 0001，以此类推。对于类型构造符，可以使用特殊的二进制编码。例如，可以为 array 分配二进制编码 1000，为 X 分配二进制编码 1001，以此类推。这样，就可以将类型表达式转换为二进制编码，以便计算机进行处理。


2.

a) 指向实数数组的指针，数组下标从51到150 的类型表达式为：

pointer (array real [51..150])


b) 二维整数数组(数组的数组)，行下标从-5到5，列下标从0到127 的类型表达式为：

array (array integer [0..127]) [-5..5]


c) 函数，其参数类型是一个函数(该函数接受一个字符型指针和一个字符、返回一个整数)、一个10个元素的字符指针数组和一个字符，返回值类型是一个10个元素的整数数组 的类型表达式为：

(function (pointer (function (pointer char, char) -> integer), array (pointer char) [1..10], char) -> array integer [1..10])
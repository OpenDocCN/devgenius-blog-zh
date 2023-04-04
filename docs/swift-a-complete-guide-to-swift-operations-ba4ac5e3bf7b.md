# Swift Essentials | Swift 运营完整指南

> 原文：<https://blog.devgenius.io/swift-a-complete-guide-to-swift-operations-ba4ac5e3bf7b?source=collection_archive---------7----------------------->

![](img/55cf04af0abe4d4e8ffa7f0ce1a833d4.png)

# 操作数和运算符

```
X+Y
```

*   x 和 Y 是**操作数**
*   +是一个**运算符**

# **操作分类**

有两种方式对操作进行分类

*   是否有 1、2 或 3 个操作数
*   运算符相对于操作数的位置

```
 TYPE               Description                      Example 
___________________________________________________________________**Unary   Operations**    Contains one operand                  !A**Binary  Operations** Contains two operands                 A+B**Ternary Operations**    Contains three operands              A?B:C___________________________________________________________________**Prefix  Operations**    Operator appears before               !A
                      the operand**Infix   Operations** Operator appears in                   A+B 
                      between operands**Postfix Operations**    Operator appears after                A!
                      operand
```

# **操作类型**

**算术**

*   操作数几乎总是整数，所以这些操作几乎总是返回整数值。
*   然而，有时我们可以添加字符串或列表，这样在这种情况下，这些操作符将返回操作数包含的任何值。

```
TYPE                        Example               Description
___________________________________________________________________**Addition(+)**                  A+B                Adds A and B **Subtraction(-)  **             A-B                Subtracts A and B **Multiplication(*) **           A*B                Multiplies A and B **Division(/)**                  A/B                Divides A and B **Modulus(%) **                  A%B                Divides A and B and 
                                                return the leftover 
                                                value 
```

**比较**

*   比较总是返回布尔值。
*   操作数可以是任何类型。

```
TYPE                           Example               Description
___________________________________________________________________**Equal(==)**                        A==B               Checks if A and
                                                    B are the same 
                                                    and returns 
                                                    boolean value 
                                                    accordingly**Not Equal (!=)**                   A!=B               Checks if A and
                                                    B are not
                                                    the same 
                                                    and returns 
                                                    boolean value 
                                                    accordingly**Greater Than Or Equal (>=)** A>=B               Checks if A is 
                                                    greater than or
                                                    equal to B and 
                                                    returns boolean
                                                    value 
                                                    accordingly**Less Than Or Equal (<=)**          A<=B               Checks if A is 
                                                    less than or
                                                    equal to B and 
                                                    returns boolean
                                                    value 
                                                    accordingly**Greater(>)**                       A>B                Checks if A is 
                                                    greater than 
                                                    B and returns
                                                    boolean value
                                                    accordingly**Less(<) **                         A<B                Checks if A is 
                                                    less than B
                                                    and returns
                                                    boolean value
                                                    accordingly___________________________________________________________________**Reference is the Same(===) **      A===B              Checks if A and 
                                                    B have the same 
                                                    reference type **Reference is not the Same(!==)**   A!==B              Checks if A and 
                                                    B don't have the
                                                    same reference 
                                                    type
```

**三元条件**

*   这里， **A 必须是布尔类型。**
*   b 和 C 可以是任何类型。
*   返回类型将是 B 或 c。

```
TYPE                           Example               Description
___________________________________________________________________**Ternary Conditional**             A?B:C           Checks if A is true.
                                                If A is true, it 
                                                return B, otherwise, 
                                                it returns C. 
```

**范围**

*   a 和 B 必须是整数值
*   返回类型将是一个整数值范围。

```
TYPE                        Example               Description
___________________________________________________________________**Closed Range**              A...B                 [A,B]. All values
                                                from A to B,
                                                including B.**Half Range   **             A..<B                 [A,B). All values    
                                                from A to B,
                                                excluding B.**Partial Range**             A...                  All values greater
                                                than A ...A                  All values smaller        
                                                than A, excluding A. ..<A                  All values smaller        
                                                than A, excluding A.
```

**布尔**

*   a 和 B 必须是布尔值
*   返回类型也是一个布尔值

```
TYPE                        Example               Description
___________________________________________________________________**Not(!)**                     !A                    Returns Opposite 
                                                 of bolean value 

**And(&&) **                  A && B                 Returns Logical 
                                                 And operation of 
                                                 A and B **Or(||)**                    A || B                 Returns Logical 
                                                 Or operation of 
                                                 A or B
```

**按位**

*   a 和 B 必须是整数。
*   你必须考虑上溢和下溢
*   返回类型也是整数。

```
TYPE                        Example               Description
___________________________________________________________________**Not(~)**                     ~A                  Flips all the bits 
                                               and returns its 
                                               value **And(&)**                     A&B                 Returns Bitwise
                                               And operation of 
                                               A and B**Or(|)**                      A|B                 Returns Bitwise
                                               Or operation of 
                                               A or B**XOR(^)**                     A^B                 Returns Bitwise
                                               XOR operation of 
                                               A xor B**ShiftLeft(<<)  **            A<<B                Shifts left by B
                                               bits **Shift Right(>>)**            A<<B                Shifts right by B
                                               bits
```

**溢出**

*   a 和 B 必须是整数。
*   返回类型也是整数。

```
TYPE                        Example               Description
___________________________________________________________________**Overflow Addition(&+)**        A &+ B            Adds A and B.
                                               Takes overflow into
                                               consideration **Overflow Subtraction(&-)**     A &- B            Subtracts A and B.
                                               Takes overflow into
                                               consideration**Overflow Multiplication(&*)**  A &* B            Multiplies A and B.
                                               Takes overflow into
                                               consideration**Overflow Division(&/)**        A &/ B            Divides A and B.
                                               Takes overflow into
                                               consideration**Overflow Modulo(&%)**          A &% B            Takes modulus of A
                                               and B. Takes overflow 
                                               into consideration.
```
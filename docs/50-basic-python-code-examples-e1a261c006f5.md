# 50 多个基本 Python 代码示例

> 原文：<https://blog.devgenius.io/50-basic-python-code-examples-e1a261c006f5?source=collection_archive---------0----------------------->

## 列表、字符串、分数计算等等..

![](img/341f3c4e70a253c2b1662272a370e183.png)

作者图片

## 1.如何在 Python 上打印“Hello World”？

```
print("Hello World")
```

## 2.如何在 Python 上打印带有用户名的“Hello + Username”？

```
usertext = input("What is your name? ")
print("Hello", usertext)
```

## 3.如何将 Python 上输入的 2 个数相加？

```
num1 = input('Enter first number: ')
num2 = input('Enter second number: ')

sum = float(num1) + float(num2)

print('The sum of {0} and {1} is {2}'.format(num1, num2, sum))
```

[](https://medium.com/dev-genius/understanding-principles-of-python-1c01eadd4e2f) [## 理解 Python 的原理

### Python 基础循序渐进

medium.com](https://medium.com/dev-genius/understanding-principles-of-python-1c01eadd4e2f) 

## 4.如何在 Python 上求 2 个输入数字的平均值？

```
num1 = input('Enter first number: ')
num2 = input('Enter second number: ')

average =(int(num1) + int(num2))

print('average:{0} '.format(average))
```

## 5.如何在 Python 上计算输入的签证和最终成绩平均分？

```
visagrade = input('enter your visa grade : ')
finalgrade = input('enter your final grade : ')average =(float(visagrade)*0.3)+(float(finalgrade)*0.7)print("average :{0} ".format(average))
```

## 6.如何求 Python 上输入的 3 个笔试成绩的平均值？

```
firstexam = input('your first exam : ')
secondexam = input('your second exam : ')
thirdexam = input('your third exam : ')average =(float(firstexam)+float(secondexam)+float(thirdexam))/3print("average :{0} ".format(average))
```

## 7.如何在 Python 上显示已录入书面平均分的学生的班级通过状态(通过—未通过)？

```
average = input('enter average : ')if(int(ort)>=50):
print("Passed")else:
print("Failed")
```

[](https://medium.com/dev-genius/top-20-coding-software-languages-67083e04a1a0) [## 前 20 种编码软件语言

### 从 C 到 Python，每种语言都有许多共同的特性，也有独特的特性。

medium.com](https://medium.com/dev-genius/top-20-coding-software-languages-67083e04a1a0) 

## 8.如何在 Python 上查出输入的数字是奇数还是偶数？

```
num = int(input("Enter a number: "))if (num % 2) == 0:
   print("{0} is Even".format(num))else:
   print("{0} is Odd".format(num))
```

## 9.如何在 Python 上查出输入的数字是正数，负数，还是 0？

```
num = float(input("Enter a number: "))if num > 0:
   print("Positive number")elif num == 0:
   print("Zero")else:
   print("Negative number")
```

## 10.如何在 Python 上计算体重指数？

```
print("body mass index calculation program")height = float(input("enter height (m):"))
weight = int(input("enter weight (kg):"))

index  = weight/(height*height)

if index <=18:
    print("\n underweight BMİ:{}".format(index))
elif index > 18 and index <=25 :
    print("\n overweight BMİ:{}".format(index))
elif index > 25 and index <=30:
    print("\n obese BMİ:{}".format(index))
elif index > 30:
    print("\n severely obese BMİ:{}".format(index))
```

## 11.如何在 Python 上显示输入年龄的人能否拿到驾照？

```
age = input('enter age : ')if(int(age)<18):
print("Your Age Is Not Eligible To Get A Driver's License")else:
print("Your Age Is Eligible To Get Your License")
```

## 12.如何在 Python 上的屏幕上列出数字 1-100？

```
for i in range(1,101):
print(i)
```

## 13.如何在 Python 上列出 1–100 的偶数？

```
for i in range(1,101):
if i%2==0:
print(i)
```

## 14.如何在 Python 上列出 1–100 的奇数？

```
for i in range(1,101):
if i%2!=0:
print(i)
```

[](https://medium.com/dev-genius/the-change-of-coding-language-softwares-by-time-cef89802afc6) [## 编码语言软件随时间的变化

### 从过去到现在的历史解释

medium.com](https://medium.com/dev-genius/the-change-of-coding-language-softwares-by-time-cef89802afc6) 

## 15.如何在 Python 上找到 1 到 100 之间被 3 和 5 除的数？

```
for i in range(1,101):
if i%3==0 or i%5==0:
print(i)
```

## 16.如何在 Python 上列出从 1 到用户输入数字的数字？

```
num = input('enter number : ')
for i in range(1,int(num)+1):print(i)
```

## 17.如何在 Python 上求有边矩形的面积和周长？

```
short = input('Enter short side : ')
tall = input('Enter tall side : ')area = int(short)*int(tall)
perimeter =2*(int(short)+int(tall))print("area: {0}".format(alan))
print("perimeter: {0}".format(cevre))
```

## 18.如何在 Python 上把输入文本的字母一个接一个打印出来？

```
word = 'mrhuseyin'
for char in word:
    print(char)
```

## 19.如何显示用户在 Python 上输入的两个数之和？

```
sumofnumbers=0;num1 = input('first number: ')
num2 = input('second number: ')for i in range(int(sayi1)+1,int(sayi2)):
sumofnumbers+=iprint("Sum of numbers between {0} and {1} : {2}".format(num1,num2,sumofnumbers))
```

## 20.例如，让我们询问用户对电影院或剧院的选择。看电影要 10 块钱，看戏要 5 块钱。我们认为学生可以得到 50%的折扣。如果学生打折；如果他不是学生，我们就写一个文档，计算不打折的金额，打印出来。

```
selection = input("Press (1) for Cinema, (2) for Theater : ")
student = input("Are you student(Y/N) : ")
price = 0#non-discounted fee calculation
if selection == '1':
price = 10 #cinema
elif selection == '2':
price = 5 #theatre#student discount
if student =='Y' or student =='y':
price=price / 2  #%50print(" The fee you have to pay :{}".format(price))
```

## 21.如何在 Python 上查出输入的数是不是质数？

```
num = int(input("Enter a number: "))  

if num > 1:  
   for i in range(2,num):  
       if (num % i) == 0:  
           print(num,"is not a prime number")  
           print(i,"times",num//i,"is",num)  
           break  
   else:  
       print(num,"is a prime number")  

else:  
   print(num,"is not a prime number")
```

[](https://medium.com/dev-genius/what-is-matlab-why-we-need-it-d61e405ef419) [## Matlab 是什么？我们为什么需要它？

### 了解 Matlab 的基础知识

medium.com](https://medium.com/dev-genius/what-is-matlab-why-we-need-it-d61e405ef419) 

## 22.如何分别求出奇数和偶数的和，直到用户在 Python 上输入的数字？

```
NumList = []
Even_Sum = 0
Odd_Sum = 0

Number = int(input("Please enter the Total Number of List Elements: "))
for i in range(1, Number + 1):
    value = int(input("Please enter the Value of %d Element : " %i))
    NumList.append(value)

for j in range(Number):
    if(NumList[j] % 2 == 0):
        Even_Sum = Even_Sum + NumList[j]
    else:
        Odd_Sum = Odd_Sum + NumList[j]

print("\nThe Sum of Even Numbers in this List =  ", Even_Sum)
print("The Sum of Odd Numbers in this List =  ", Odd_Sum)
```

## 23.如何计算在 Python 上输入了工资和加薪率的员工的加薪？

```
newsalary = 0salary = input("enter new salary : ")
raise = input("salary raise rate(%) : ")newsalary = int(salary)+(int(salary)*int(raise)/100)print("increased salary :",newsalary)
```

## 24.如何计算用 Python 上的函数输入半径的圆的面积和周长？

```
import math

def find_Diameter(radius):
    return 2 * radius

def find_Circumference(radius):
    return 2 * math.pi * radius

def find_Area(radius):
    return math.pi * radius * radius

r = float(input(' Please Enter the radius of a circle: '))

diameter = find_Diameter(r)
circumference = find_Circumference(r)
area = find_Area(r)

print("\n Diameter Of a Circle = %.2f" %diameter)
print(" Circumference Of a Circle = %.2f" %circumference)
print(" Area Of a Circle = %.2f" %area)
```

## 25.如何计算矩形的面积，其宽度和高度是使用 Python 上的函数输入的？

```
def areaRectangle(a, b): 
    return (a * b) 

def perimeterRectangle(a, b): 
    return (2 * (a + b)) a = 5; 
b = 6; print ("Area = ", areaRectangle(a, b))

print ("Perimeter = ", perimeterRectangle(a, b))
```

## 26.用 Python 制作猜数字游戏

```
import random
import math# Taking Inputs
lower = int(input("Enter Lower bound:- "))

# Taking Inputs
upper = int(input("Enter Upper bound:- "))

# generating random number between
# the lower and upper
x = random.randint(lower, upper)
print("\n\tYou've only ", 
       round(math.log(upper - lower + 1, 2)),
      " chances to guess the integer!\n")

# Initializing the number of guesses.
count = 0

# for calculation of minimum number of
# guesses depends upon range
while count < math.log(upper - lower + 1, 2):
    count += 1

    # taking guessing number as input
    guess = int(input("Guess a number:- "))

    # Condition testing
    if x == guess:
        print("Congratulations you did it in ",
              count, " try")
        # Once guessed, loop will break
        break
    elif x > guess:
        print("You guessed too small!")
    elif x < guess:
        print("You Guessed too high!")

# If Guessing is more than required guesses,
# shows this output.
if count >= math.log(upper - lower + 1, 2):
    print("\nThe number is %d" % x)
    print("\tBetter Luck Next time!")
```

## 27.如何在 Python 上找出给定日期是一年中的哪一天？

```
import datetime

date=str(input('Enter the date(for example:09 02 2019):'))day_name= ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday','Sunday']day = datetime.datetime.strptime(date, '%d %m %Y').weekday()print(day_name[day])
```

## 28.如何在 Python 上找到一个排序列表范围内缺失的数字？

```
def find_missing(lst): 
    return [x for x in range(lst[0], lst[-1]+1)  
                               if x not in lst] 

# Driver code 
lst = [1, 2, 4, 6, 7, 9, 10] 
print(find_missing(lst))**Output:
[3, 5, 8]**
```

## 29.如何在 Python 上检查一个字符串中是否有指定的字符？

```
char_list = ["a", "b" ,"c"]
string = "abcd"matched_list = [characters in char_list for characters in string]print(matched_list)**OUTPUT**
[True, True, True, False]string_contains_chars = all(matched_list)print(string_contains_chars)
```

## 30.如何在 Python 上求整数的奇偶平均值的平均值？

```
total = 0
evenSums = 0
oddSums = 0
done = Falsewhile(not done):
    user_in = input("Give me an integer or type 'done' to be done.")
    if( user_in.lower() == "done"):
        done = True
    else:
        # assuming they've typed in an integer
        total += int(user_in)
        if user_in % 2 == 0:
            evenSums += user_in
            evenAverage = evenSums / user_in
        else:
            oddSums += user_in
            oddAverage = oddSums / user_inprint(total)
print("Even Average: " + str(evenAverage))
print("Odd Average: " + str(oddAverage))
```

## 31.如何将数据库中的记录放到 Python 上？

```
import pymysql.cursors# Database connection sentenceconnection = pymysql.connect(host='localhost',
                             user='root',
                             password='',
                             db='students',
                             charset='utf8mb4',
                             cursorclass=pymysql.cursors.DictCursor)try:
    with connection.cursor() as cursor: # single line reading
        sql = "SELECT `id`, `firstname`,`lastname` FROM `users`"
        cursor.execute(sql) for row in cursor.fetchall():
            #read all lines
            firstname = str(row["firstname"])
            lastname = str(row["lastname"]) #print on screen
             print("firstname : " + firstname)
             print("lastname : " + lastname)finally:
        connection.close()
```

## 32.如何在 Python 上使用 Tkinter Form？

```
from tkinter import *
window = Tk()
# add widgets here

window.title('Hello Python')
window.geometry("300x200+10+20")
window.mainloop()
```

## 33.如何在 Python 上创建 Python 表单入口？

```
from tkinter import *
from tkinter import messageboxwindow = Tk()window.title("[mrhuseyin.medium.com](https://mrhuseyin.medium.com/)")
window.geometry("400x300")#plotting grid forms
application = Frame(window)application.grid()
L1 = Label(uygulama, text="Enter your name")
L1.grid(padx=110, pady=10)E1 = Entry(application, bd =2)
E1.grid(padx=110, pady=3)#draw form
window.mainloop()
```

## 34.如何在 Python 上创建 Python Tkinter ListBox？

```
from tkinter import *

from tkinter import messagebox

window = Tk()

window.title("[mrhuseyin.medium.com](https://mrhuseyin.medium.com/)")
window.geometry("400x300")

#grid form çizdirme
application = Frame(window)
application.grid()

Lb1 = Listbox(application)
Lb1.insert(1, "Python")
Lb1.insert(2, "C#")
Lb1.insert(3, "JAVA")
Lb1.insert(4, "JAVASCRIPT")
Lb1.grid(padx=110, pady=10)

#draw form
pencere.mainloop()
```

## 35.如何在 Python 上用 Python Tkinter 制作一个简单的注册表单？

```
# import openpyxl and tkinter modules 
from openpyxl import *
from tkinter import *

# globally declare wb and sheet variable 

# opening the existing excel file 
wb = load_workbook('C:\\Users\\Admin\\Desktop\\excel.xlsx') 

# create the sheet object 
sheet = wb.active 

def excel(): 

    # resize the width of columns in 
    # excel spreadsheet 
    sheet.column_dimensions['A'].width = 30
    sheet.column_dimensions['B'].width = 10
    sheet.column_dimensions['C'].width = 10
    sheet.column_dimensions['D'].width = 20
    sheet.column_dimensions['E'].width = 20
    sheet.column_dimensions['F'].width = 40
    sheet.column_dimensions['G'].width = 50

    # write given data to an excel spreadsheet 
    # at particular location 
    sheet.cell(row=1, column=1).value = "Name"
    sheet.cell(row=1, column=2).value = "Course"
    sheet.cell(row=1, column=3).value = "Semester"
    sheet.cell(row=1, column=4).value = "Form Number"
    sheet.cell(row=1, column=5).value = "Contact Number"
    sheet.cell(row=1, column=6).value = "Email id"
    sheet.cell(row=1, column=7).value = "Address"

# Function to set focus (cursor) 
def focus1(event): 
    # set focus on the course_field box 
    course_field.focus_set() 

# Function to set focus 
def focus2(event): 
    # set focus on the sem_field box 
    sem_field.focus_set() 

# Function to set focus 
def focus3(event): 
    # set focus on the form_no_field box 
    form_no_field.focus_set() 

# Function to set focus 
def focus4(event): 
    # set focus on the contact_no_field box 
    contact_no_field.focus_set() 

# Function to set focus 
def focus5(event): 
    # set focus on the email_id_field box 
    email_id_field.focus_set() 

# Function to set focus 
def focus6(event): 
    # set focus on the address_field box 
    address_field.focus_set() 

# Function for clearing the 
# contents of text entry boxes 
def clear(): 

    # clear the content of text entry box 
    name_field.delete(0, END) 
    course_field.delete(0, END) 
    sem_field.delete(0, END) 
    form_no_field.delete(0, END) 
    contact_no_field.delete(0, END) 
    email_id_field.delete(0, END) 
    address_field.delete(0, END) 

# Function to take data from GUI  
# window and write to an excel file 
def insert(): 

    # if user not fill any entry 
    # then print "empty input" 
    if (name_field.get() == "" and
        course_field.get() == "" and
        sem_field.get() == "" and
        form_no_field.get() == "" and
        contact_no_field.get() == "" and
        email_id_field.get() == "" and
        address_field.get() == ""): 

        print("empty input") 

    else: 

        # assigning the max row and max column 
        # value upto which data is written 
        # in an excel sheet to the variable 
        current_row = sheet.max_row 
        current_column = sheet.max_column 

        # get method returns current text 
        # as string which we write into 
        # excel spreadsheet at particular location 
        sheet.cell(row=current_row + 1, column=1).value = name_field.get() 
        sheet.cell(row=current_row + 1, column=2).value = course_field.get() 
        sheet.cell(row=current_row + 1, column=3).value = sem_field.get() 
        sheet.cell(row=current_row + 1, column=4).value = form_no_field.get() 
        sheet.cell(row=current_row + 1, column=5).value = contact_no_field.get() 
        sheet.cell(row=current_row + 1, column=6).value = email_id_field.get() 
        sheet.cell(row=current_row + 1, column=7).value = address_field.get() 

        # save the file 
        wb.save('C:\\Users\\Admin\\Desktop\\excel.xlsx') 

        # set focus on the name_field box 
        name_field.focus_set() 

        # call the clear() function 
        clear() 

# Driver code 
if __name__ == "__main__": 

    # create a GUI window 
    root = Tk() 

    # set the background colour of GUI window 
    root.configure(background='light green') 

    # set the title of GUI window 
    root.title("registration form") 

    # set the configuration of GUI window 
    root.geometry("500x300") 

    excel() 

    # create a Form label 
    heading = Label(root, text="Form", bg="light green") 

    # create a Name label 
    name = Label(root, text="Name", bg="light green") 

    # create a Course label 
    course = Label(root, text="Course", bg="light green") 

    # create a Semester label 
    sem = Label(root, text="Semester", bg="light green") 

    # create a Form No. lable 
    form_no = Label(root, text="Form No.", bg="light green") 

    # create a Contact No. label 
    contact_no = Label(root, text="Contact No.", bg="light green") 

    # create a Email id label 
    email_id = Label(root, text="Email id", bg="light green") 

    # create a address label 
    address = Label(root, text="Address", bg="light green") 

    # grid method is used for placing 
    # the widgets at respective positions 
    # in table like structure . 
    heading.grid(row=0, column=1) 
    name.grid(row=1, column=0) 
    course.grid(row=2, column=0) 
    sem.grid(row=3, column=0) 
    form_no.grid(row=4, column=0) 
    contact_no.grid(row=5, column=0) 
    email_id.grid(row=6, column=0) 
    address.grid(row=7, column=0) 

    # create a text entry box 
    # for typing the information 
    name_field = Entry(root) 
    course_field = Entry(root) 
    sem_field = Entry(root) 
    form_no_field = Entry(root) 
    contact_no_field = Entry(root) 
    email_id_field = Entry(root) 
    address_field = Entry(root) 

    # bind method of widget is used for 
    # the binding the function with the events 

    # whenever the enter key is pressed 
    # then call the focus1 function 
    name_field.bind("<Return>", focus1) 

    # whenever the enter key is pressed 
    # then call the focus2 function 
    course_field.bind("<Return>", focus2) 

    # whenever the enter key is pressed 
    # then call the focus3 function 
    sem_field.bind("<Return>", focus3) 

    # whenever the enter key is pressed 
    # then call the focus4 function 
    form_no_field.bind("<Return>", focus4) 

    # whenever the enter key is pressed 
    # then call the focus5 function 
    contact_no_field.bind("<Return>", focus5) 

    # whenever the enter key is pressed 
    # then call the focus6 function 
    email_id_field.bind("<Return>", focus6) 

    # grid method is used for placing 
    # the widgets at respective positions 
    # in table like structure . 
    name_field.grid(row=1, column=1, ipadx="100") 
    course_field.grid(row=2, column=1, ipadx="100") 
    sem_field.grid(row=3, column=1, ipadx="100") 
    form_no_field.grid(row=4, column=1, ipadx="100") 
    contact_no_field.grid(row=5, column=1, ipadx="100") 
    email_id_field.grid(row=6, column=1, ipadx="100") 
    address_field.grid(row=7, column=1, ipadx="100") 

    # call excel function 
    excel() 

    # create a Submit Button and place into the root window 
    submit = Button(root, text="Submit", fg="Black", 
                            bg="Red", command=insert) 
    submit.grid(row=8, column=1) 

    # start the GUI 
    root.mainloop()
```

## 36.如何在 Python 上制作一个猜中用户所持数字的游戏？

```
from random import randint

rand = randint(1, 100)
counter = 0

while True:
    counter+=1
    num=int(input("Enter values between 1 and 100 (0 exit):"))
    if(num==0):
        print("You canceled the game.")
        break
    elif num < rand:
        print("Enter a Higher Number.")
        continue
    elif num > rand:
        print("Enter a Lower Number.")
        continue
    else:
        print("Randomly selected number {0}!".format(rand))
        print("Your guess number {0}".format(counter))
```

## 37.如何在 Python 上把自己喜欢的水果列成清单打印在屏幕上？

```
favorite_fruits = ['apples', 'erics', 'oranges']
print("My favourite fruits {}".format(favorite_fruits))
```

## 38.如何在 Python 上打印“圆周率的个数按顺序排列，相当于 cm 中的英制单位，微处理器的缩写，你正在使用的操作系统的名称”？

```
list=[3.14,2.54,"CPU","WINDOWS 10"]

print(list)**Output:** [3.14, 2.54, 'CPU', 'WINDOWS 10']
```

## 39.如何列出一周中从星期一开始到星期五结束的日子？还有，如何在 Python 上把你创建的索引为 4 的列表的元素打印在屏幕上？

```
weekdays=["Monday","Tuesday","Wednesday","Thursday","Friday"]
print(weekdays[4])**output**
Thursday
```

## 40.如何在 Python 上将列表从 3，1，2 值分别改为 1，1，2？

```
list=[3,1,2]

list[0]=1
list[1]=1
list[2]=2

print(list)**output** [1, 1, 2]
```

## 41.如何在 Python 上制作一个星期名称列表，并检查某些日子是否在列表中？

```
weekdays=["Monday","Tuesday","Wednesday","Thursday","Friday"]

print("Friday" in weekdays)
print("Saturday" in weekdays)**output**
true
false
```

## 42.如何在 Python 上打印列表中的偶数？

```
list1 = [10, 21, 4, 45, 66, 93] 

for num in list1: 

    # checking condition 
    if num % 2 == 0: 
       print(num, end = " ")
```

## 43.创建一个由 3-18 个数字组成的名为 numbers 的列表。将创建的列表与“numbers2 = [21，22，23，24，25]”列表合并，然后在屏幕上打印除以 4 的数字。

```
numbers = [3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18]
numbers2 = [21,22,23,24,25]
combine = numbers + numbers2

for number in combine:
 if sayi%4==0:
  print(number)
```

## 44.如何用 Python 上的 while 循环写出屏幕输出如下的代码？

```
1 . class
2 . class
3 . class
4 . class
5 . class
6 . class
7 . class
8 . class
9 . class
10 . class
11 . class
12 . class***Solution;***i = 1
while i <= 12:
  print(" {}. class".format(i))
  i += 1
```

## 45.如何对用户在 Python 上输入的数字的位数求和？

```
number = input("enter a number: ")
sum=0
for numberofdigit in number:
  sum += int(numberofdigit)

print("the sum of the digits of the number:",sum)
```

## 46.Python 上用户输入的数字(零除外)的位数如何相乘？

```
number = input("enter an number: ")
carpim=1
for digitofnumber in str(numaber):
  if int(digitofnumber) != 0:
    multiplication*= int(digitofnumber)print("digits of the number: ",multiplication)
```

## 47.如何使用 Python 上的“for 循环”将程序员的名字打印在屏幕上？

```
text = input("enter a name: ")

for i in range(10):
  print(text)
```

## 48.如何使用 Python 上的“while loop”将程序员的名字打印在屏幕上？

```
text = input("enter a name: ")

i=0
while i<10:
  i+=1
  print(text)
```

## 49.如何在 Python 上用“sum()和 len()”求均值？

```
list=[35,48,52,57,68,84]

total = sum(list)
piece = len(list)print(total/piece)
```

## 50.如何在 Python 上求数字的和，直到在键盘上输入 0？

```
total=0
while True:
  number = float(input("enter a number: "))
  if number ==0:
    break
  total+=number
print("The sum of the numbers: ",total)
```

## 51.Python abs 示例

```
number = input("enter a number: ") 
number = int(number) 
print("The absolute value of the number", abs(number))
```

## 52.Python 上如何计算数字的绝对值？

```
number = input("enter an number: ")
number = int(number)
if number<0:
  number*=-1
print("The absolute value of the number: ",number)
```

希望这些能帮到你..

来源:[1](https://www.tutsmake.com/)[2](https://www.tutorialgateway.org/)[3](https://www.geeksforgeeks.org/)[4](https://stackoverflow.com/)[5](https://www.yazilimkodlama.com/)[6](https://pythonbasics.org/)[7](https://www.tutorialsteacher.com/)
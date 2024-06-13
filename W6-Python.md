# Week6-Python

## [Course link](https://cs50.harvard.edu/x/2024/weeks/6/)</br>
[![Week5 - Data structure](https://img.youtube.com/vi/EHi0RDZ31VA/0.jpg)](https://www.youtube.com/watch?v=EHi0RDZ31VA)

## Introduction
In 1989, computer scientist Guido Van Rossum discovered that various programming languages​of his time had flaws, so he wanted to improve and create a brand-new language that was easy for users to read and had a short learning curve. It was Python.

Python supports a variety of third-party function libraries and is suitable for many fields. It includes statistics, graphics, astronomy, game development,..., and more.

* Indentation 
Indentation is a significant feature of Python, which avoids any kind of braces whenever possible. An indentation marks the end of a block or statement instead.

For example, the for loop statement in C is written as:

```clike=
for(int counter = 0; counter < 5; counter++)
{
    int result = 0;
    result = pow(counter, 2);
    printf("%d\n",result);
}
```

Translate it to Python:
```python=
for counter in range(5):
    result = counter * couner;    
    print(result); 
```
Braces and data type declaration dissipate, and indentation takes place.

* Comment
Comment in C:
```clike=
/*Calculate power of two for the variable counter*/
```

Comment in Python:
```Python=
# Calculate power of two for the variable counter
```


* Basic Syntax
1. Variables:
Declaring variables in C is much more complicated than in Python and requires additional specification of the correct data type: e.g. int counter = 0; In Python, there is no need to declare the type of the variable, just declare it as counter = 0;
2. Types:
Types in C and Python are highly overlapping, and some commonly used types include bool, float, int, and str. Certain types, such as long and double, have been eliminated in Python. New types are introduced: list, tuple, dict, set.

3. Data Structures
Introducing some unique data structures in Python.
* Tuple: A tuple is a group composed of many elements, which can contain different types. Once declared, the direction of data is fixed.

```python=
# Demonstrate tuple
tuple1 = ()

# if only single element , add coma before element
tuple1 = (12,)
print(tuple1[0])

# concact string using "+"
tuple2 = tuple1 + ('month',)
print(tuple2)
```

* Dictionary
Dict consists of keys and corresponding values. The dictionary is in a non-order sequence.

```python=
#Dictionary
dict1 = {'starsign1':'Aries', 'starsign2':'Taurus', 'starsign3':'Gemini'}
print("Whole dictionary is: ", dict1)
print("The third starsign is: ", dict1['starsign3'])

del dict1['starsign1']
print("After deleting starsign1: dict1 becomes: ", dict1)

dict1['starsign1'] = 'Aries'
print("After adding starsign1: dict1 becomes: ", dict1)

del dict1
print("Delet the whole dictionary: ", dict1)
#cause an error because dict1 is no longer existing
```

* Output result:
![image](https://hackmd.io/_uploads/S15vLpbSC.png)



* Set
The set stores non-order sequence data and will automatically remove repeated elements.

```python=
s1 = set();
s1={1, 2, 3, 2, 1};
# s1 will remove repeated element: 1, 2 automatically
```

4. Conditionals:
Conditionals in Python are similar to another language, but they still have subtle differences.
Take the if-else statement as an example:
```python=
# if-else statement
x = 4;
y = 6;

if x < y:
    print("x is less than y")
elif x > y:
    print("x is larger than y")
else:
    print("x is equal to y")
```

Lacking correct indentation will raise an error as follows:
```python=
# Statements without correct indentation
x = 4;
y = 6;

if x < y:
print("x is less than y")
elif x > y:
print("x is larger than y")
else:
print("x is equal to y")
```

5. Calculator:
Python can also be a calculator, but be aware of the truncation and floating point imprecision. For example:

```python=
# Calculate devision
x = int (input("x: "))
y = int (input("y: "))
z = x/y

# lack of truncation:
print(z)
# floating-point precision with n digits decimal
print(f"{z:.4f}")
```
Output result:
![image](https://hackmd.io/_uploads/Bk7bwB3NR.png)
    
6. OOP(Object-Oriented Programming) 
From previous weeks' lectures, we've learned about functional programming. From this week, Python introduces a new method: Object-Oriented programming(short as OOP), accessing function property by treating the function as an object.

```python=
s = input("Do you agree? ")

if s.lower() in ["y", "yes"]:
    print("Agreed.")
    
else s.lower() in ["n", "no"]:
    print("Not agreed.")
```
The character "." dot is a method to access the function lower( ), which changes strings into lowercase.


## Application
* web scrawling: Beautiful soup
    * Web page
![image](https://hackmd.io/_uploads/rJxByChZSA.png)

    * Parse result: 
![image](https://hackmd.io/_uploads/SJyzChZrC.png)


```python=
import urllib.parse, urllib.request
from bs4 import BeautifulSoup
urlstr = 'http://www.gotop.com.tw/'
responseObj = urllib.request.urlopen(urlstr)
htmlContent = responseObj.read()
bs = BeautifulSoup(htmlContent.decode(), 'html.parser')
```

* Computer vision: openCV
OpenCV is short for Open Sourcer Computer Vision Library, is used for computer vision, machine learning, image processing and video processing. 
Some applications of OpenCV: 
    * Face recognition
    * Image Enhancement
    * Automated inspection and surveillance
    * Object recognition
    * Video/Image search and retrieval
    * ...and more

```python=
# demo read and show an image
import cv2
img = cv2.imread("pic.jpg", cv2.IMREAD_COLOR)
cv2.imshow("image", img)
cv2.waitKey(0)
print(img)
```
* Result of "print( )":

![image](https://hackmd.io/_uploads/SyaxBjVBC.png)

</br>

* Result of "cv2.imgshow( )"

![ImgshowCV](https://hackmd.io/_uploads/Bk5-l1_r0.jpg)

* Data analysis: Numpy,Pytorch, Tensorflow...
***Learn more usage on the bottom link: Python documentation***

---

## P-sets
* [DNA](https://cs50.harvard.edu/x/2024/psets/6/dna/)
In this problem, the goal is to find the corresponding person from his/her nucleotides DNA of chosen file, and searching in the database.
The usage and expected results are shown below:

![image](https://hackmd.io/_uploads/S19acB2VR.png)

How to solve it?
By the method of recognizing a person by counting short Tandem Repeats(STRs). For example, the picture below shows Alice has the STR "AGAT" repeated four times in her DNA. And Bob has five repeated times. Counting multiple STRs rather than one can improve the accuracy of DNA profiling.

![image](https://hackmd.io/_uploads/rJAye834A.png)
0. Open database file as .csv
1. Find all STRs in the first row in database. Database has a dictionary data structure.

![image](https://hackmd.io/_uploads/B1EwoHa4A.png)
.csv data in the file database, the first row contains the name of nucleotides  DNA.

```python=
# List stores STRs which obtained by function key()
dictkeys = list(row[0].keys())
STRs = dictkeys[1:len(dictkeys)]
```
1. Count repeated times for each STR from the input .txt file. (The person we are trying to recognize)

```python=
for i in range(len(STRs)):
    # Set STRs in list above as subsequence
    subsequence = STRs[i]
    # use function long_match() to find repeated times of the input file's STRs
    STRmatch.append(longest_match(sequence, subsequence))
```
append() is used to add numbers returning from longest_match() into the STRmatch[ ] list. 

2. The function longest_match() finds the repeated times of STRs in the database and uses the input profile to find the matched person.
```python=
# Stored each set of numbers (from rows in database)
# into a 2-dimensional list
for i in range(len(rows)):
    dna = [STRs[i] for i in range(len(STRs))]
    findmatch = []
    for j in range(len(dna)):
        num = 0
        num = int(rows[i][dna[j]])
        findmatch.append(num)
# Search correspondant set from the list
```

3. Be careful of indent!
In this problem, when you use "with open( )" the code below should be indented while still using the input file.

## Problems encounter while installing python and packages...
* Install python
    * from python website + IDE setup
    * from Anaconda
        * Include many python packages
        
* Enviroment setup:

* Packages management: to avoid dependencies' conflicts
    * conda, pip, pypi
* Introduce a package management tool: [**poetry**](https://hackmd.io/zBjSwpKaSAmXVdvv8raJgw?view)

---

#### Python Documentation
1.  [Python Doc]([docs.python.org](https://docs.python.org/3/))
2.  [An Informal Introduction to Python](https://docs.python.org/3/tutorial/introduction.html#text) 

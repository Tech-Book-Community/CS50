[TOC]

## 2024/03/17 - Week 0 Scratch

### Attendees
Yun, Tommy, Murky
Absent: Chris

### Slides
(Chris)
[CS50_Week_0_chriswang.pptx](https://github.com/Tech-Book-Community/CS50/files/14636028/CS50_Week_0_chriswang.pptx)
(Yun)
[2024CS50_week0_yun_github.pptx](https://github.com/Tech-Book-Community/CS50/files/14636021/2024CS50_week0_yun_github.pptx)

### Scratch project
(Murky) 
Maze(https://scratch.mit.edu/projects/984715599)

(Yun)
hippo's journey(https://scratch.mit.edu/projects/951100864)

(Chris)
滑鼠點擊遊戲(https://scratch.mit.edu/projects/984234855)


---
> Note starts here
---

### Week 0 Scratch
>  Computer science

![image](https://github.com/Tech-Book-Community/CS50/assets/149700108/8e100360-1bf7-4bf9-9cee-f6a8e0069315)

Computer science is the art of solving complex problems. 
Assuming your problem needs to be solved has many factors, i.e. input, you put it into a black box and wish it can figure out a solution, i.e. output.  
For input data, you need to determine which data type it needs, depending on data size or for the sake of convenience in computing it.  
Inside the black box, a logical statement should be invented to solve the problem you are concerned about. Recall the process in the course, Professor David showed a good demonstration of binary search using the example of a phonebook. After splitting half and another half of the phone book, we finally found the person we were searching for.

> Representation
Many types of data can be represented using mathematical methods and converted into numerical form. For example, images, texts, video, audio files, ..., etc.
    > ASCll[^1]
ASCll table contains alphabets, characters, and symbols, each of which corresponds to a unique number
    > Binary
Representing data in zero and one.
    >  Image
zeros and ones can also be used to represent colors. Color in a pixel can be represented by 3 values, Red, Green, and Blue. And for bitmap, its value rises from 0 to 255, which means each color has 256 color depths. 
 
> Algorithm: When you try to solve a problem, you might consider the size of the problem and how many times to solve it. Then break it down into small pieces, and solve them one by one.</br>
    >  Binary tree: 
```typescript
    function binary_search_rightmost(tradesBeenSearch: FormattedTradeDto[], endTime: number): number {
      let l = 0;
      let r = tradesBeenSearch.length;

      let infinityLoopCount = 0;
      while (l < r) {
        let m = Math.floor((l + r) / 2);
        if (tradesBeenSearch[m].second > endTime) {
          r = m;
        } else {
          l = m + 1;
        }
      }
      return r - 1;
    }
```

> Scratch
A graphical and intuitive programming language, you can build an anemi, game, ..., etc, using Scratch.
In course week 0, it is used to demonstrate basic programming concepts.

> Introduce how to use CS50 web resources.
Navigated through blocks of CS50 website and demonstrated where to find lecture videos and notes.


---

### Resources

Guide for Scratch beginner(http://scratch.mit.edu.ideas)


[^1]:(<https://learn.microsoft.com/en-us/host-integration-server/core/character-tables2>)


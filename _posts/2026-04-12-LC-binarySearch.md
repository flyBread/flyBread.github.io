---  
layout: post
title: BinarySearch-LC-Summary
categories: BinarySearch
description: Summary
keywords: Algorithm
---  

# Binary Search
let’s first look at this array of numbers from 4 to 50.
![tu1](/images/image-1.png)
If I asked you what index the number 27 is at, you could probably tell just by looking at it.
![2](/images/image.png)  
In that situation, the simplest way would be to start at the beginning and check each number until we find 27, keeping track of how many numbers we’ve checked.
This approach or algorithm is called linear search, which is an appropriate name because the algorithm searches an array in a linear or sequential manner until it finds the result it’s looking for.   

if we use binary search with an array of 1 million items, how many steps do you think it would take? Maybe 100,000 or 10,000?
Well, the real answer is 20. Only 20.
I repeat that: for 1 million elements, binary search takes only 20 steps in the worst case.

This is because it continuously halves the search space on each iteration, meaning it follows a logarithmic function.   
In fact, because it’s logarithmic, we can calculate the steps for any size array using the formula log n + 1, where n is the size of the array we’re searching.

To use binary search, we must meet two requirements.  
The first is random access, meaning we can jump to any index of our collection in constant time.
This is possible when using an array or vector, but not in other data structures like linked lists.  
The second condition,is that our collection of elements must be sorted.  

binary search can be found in some unexpected places, like the Git version control system, which provides the git bisect command.  

pseudocode code like
~~~
positionTobeFind Search(array , target) {
   index from , end;
   index mid = from+(end-from)/2;
   while(from <= end) { 
        if target == array[mid]
            return mid;
        if target > array[mid]
            from = mid+1;
        else
            end = mid-1;
   }
}
~~~

> Note: is any different in <= and < ? 

then consider the condition: 
>  The second condition,is that our collection of elements must be sorted.

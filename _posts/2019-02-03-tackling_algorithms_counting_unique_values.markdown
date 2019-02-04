---
layout: post
title:      "Tackling Algorithms: Counting Unique Values"
date:       2019-02-04 01:05:58 +0000
permalink:  tackling_algorithms_counting_unique_values
---


### Problem:  Given an array of integers, count the number of unique values.

For example, if we have the following array:

[1, 1, 1, 1, 1, 1, 3, 5]

the count of original values is 3 => 1, 3, and 5.

#### First Solution: Frequency Counter

The first time I attempted this problem I decided to: 
**1. Create an empty object**, and for every integer encountered either
**2.  Add that integer to the object** or, if that integer is already in the object 
**3. Increment its value by 1**.  
		 
Here is my code:

```
function countUniqueValues(arr){
  
  let newObj = {};                  //created new empty object to hold integer values.
	
  for (let i=0; i<arr.length; i++){ //iterate over the array
                                  
      
      let char = arr[i];
      
      if ( newObj[char] > 0 ) {                //if the item is already in newObj 
          newObj[char]++            //increment its value by 1
      } else {
          newObj[char] = 1          //if the integer is not already in newObj put it there with a value of 1
      }
      
  }
  return Object.keys(newObj).length;   //return length of array returned by Object.keys(newObj)
}
```

Running **countUniqueValues([3,3,3,4,4,4,5,5,5,5,5,6])** returns 4. because there are 3 unique values => 3, 4, 5, and 6.

If we want to see the value of newObj at the end of the function we can put *console.log(newObj) before the final return:
```
...
}
  console.log(newObj)
  return Object.keys(newObj).length;
}
```

Running **countUniqueValues([3,3,3,4,4,4,5,5,5,5,5,6])** prints a newObj value of 

```

newObj = {
     3: 3, 
     4: 3, 
     5: 5,
     6: 1
}
```

As you can see there are 4 keys in the object: 3, 4, 5, and 6. 

At this point all that is left to do is count the number of keys which we accomplish with Object.keys(newObj).length.


#### Second Solution: Multiple Pointers
(*Taken from [this course](https://www.udemy.com/js-algorithms-and-data-structures-masterclass/learn/v4/overview)*)

*To use this method you must start with a sorted array.*

1. Compare the value at the  index of i with the value at the index of j.

2. If they are equal move j ahead until we find something that is not equal to i.

3. If they are not equal move i up 1 space and put the value of j into i's new place.

4. Continue to do this until you have gone through the array completely.  'i' will be at the index of the final unique value. If we add 1 to this index number we will have the total number of unique values.

We will  use the same array as above: **[3,3,3,4,4,4,5,5,5,5,5,6]**

Let's walk through the code:

```
function countUniqueValues(arr) {
    var i=0;

    for (var j=1; j < arr.length; j++) {
        if(arr[i] !== arr[j]){
            i++;
            arr[i] = arr[j]
        }
    console.log(i,j)
    }
    return i + 1;
}
```

![img](https://i.imgur.com/UUJpmbB.png)

![img](https://i.imgur.com/yo7Lobw.png)
	 
	 
After we have gone completely through the array one time every unique value is at the beginning of the array and 'i' is in the position of the final unique value:

![img](https://i.imgur.com/kvUCdF5.png)

We see that i is at index 3 so we just add 1 to this to get 4 which is the total number of unique values: 

```
return i + 1
```

Because we are adding 1 to i this become a problem when we are given an empty array, [].  If we were to use the above solution we would get a return value of 1.  To combat this we can add a check to the beginning of the function to check for empty arrays:

```
if (arr.length === 0 ) return 0;

```

The final function is:

```
function countUniqueValues(arr) {
  if (arr.length === 0 ) return 0;
    var i=0;

    for (var j=1; j < arr.length; j++) {
        if(arr[i] !== arr[j]){
            i++;
            arr[i] = arr[j]
        }
    console.log(i,j)
    }
    return i + 1;
}
```

As you can see, the second solution to this problem only runs through the array once and there was no need to create an additional object or count the number of keys of that object so this second solution is much more efficient than the first.  Although both solutions should have time complexity of O(n), the second solution is an improvement when it comes to space cmplexity.

How would you approach this problem?
 
 
 




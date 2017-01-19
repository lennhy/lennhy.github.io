---
layout: post
title:  "An example of a Binary Search algorithm in Javascript "
date:   2017-01-19 03:25:50 +0000
---


If you have an array and you want to search throug it for a particular value it can be quite inefficient to check each index value from the first to the last.  We call this Linear Search. But there is a much more efficient approach called Binary Search. it is an algorythim that saves greater computing power and time by making less inquiries to find the particular value in question. 

Here is an example of a Binary Search algorythm that I made in Javascript:

```
var array = [2, 4, 6, 9, 13, 18, 39, 40, 100, 188, 300];
var value = 42;


function search(arr, val){
  var min = 0; 
  var max = arr.length -1;
  var indAvg;
  
  while( max < arr.length){
    console.log("this is the min value "+ min + " which is equal to " + arr[min]);
    console.log("this is the max value "+ max + " which is equal to " + arr[max]);
    console.log("this is the average value "+ indAvg + " which is equal to " + arr[indAvg]);
    if(max < min){
      return "val is not present in array";
    }
      indAvg = Math.round((min + max)/2);
       
      if (arr[indAvg] !== val){
          if (arr[indAvg] < val){
            min = indAvg + 1;
          }
          else if (arr[indAvg] > val){
            max = indAvg - 1;
          }
      }
    else{
      return indAvg;
    }
    
  }
  
}

search(array, value);

```

First I created a variable array.
```
var array = [2, 4, 6, 9, 13, 18, 39, 40, 100, 188, 300];
```

Then I created a variable for the value argument that we want to search for.

```
var value = 42;
```

Next comes the function where I stored the other variables involved in the search that only change from inside the while loop. 

```
function search(arr, val){......
```

The while loop runs only if the min value is always lower than the lenght of the array which in this case has 11 indices.
```
  while( max < arr.length){......
```

For every loop I console.log the min, max and average values on every loop to see if the algorithm works. The algorthm should change the values of min or max if index Average or indAvg is not equal to the value argument (42). 

```
  console.log("this is the min value "+ min + " which is equal to " + arr[min]);
    console.log("this is the max value "+ max + " which is equal to " + arr[max]);
    console.log("this is the average value "+ indAvg + " which is equal to " + arr[indAvg]);
```

Then the algorthim uses the min and max values to find the average or center index of the array (the mean)

```
      indAvg = Math.round((min + max)/2);
```

and checks to see if the value of the average's index is greater than or less than the value argument. If the arr[indAvg]  is less than the value then we check the right half of the array starting with the minmum value which is set to equal the indAgvg +1.

```
  if (arr[indAvg] < val){
            min = indAvg + 1;
          }
```

We do this becasue we now know that the left half of the array of 5 indeces do not hold the value argument we are looking for. So we cut the work into half. Now we are left with one half of the array to query. If it were the other way around then we would make the max value equal to indeAvg -1 and query the left half of the array of indeces instead. 

```
 else if (arr[indAvg] > val){
            max = indAvg - 1;
          }
```

The last statemtent (the else statement) would only run if none of the prior options (if and if else statements were true). Then we would return the index value that holds the value argument (42).

```
    else{
      return indAvg;
    }
    
```

If this else statemtnent does not run either one of the if or else statement prior to this would run  this time splitting the other half of the new array to be querried. 

As an edge case at the beginning of the while loop we first check to see if the min value is larger than the max value. This eventually happens whenever the value argument is not present in the array.

```
   if(max < min){
      return "val is not present in array";
    }
```

The explanation being that because the values on either side of the half of the array do not contain the value parameter  
and tthe min value is always incrementing while the max decrements, at some point the values will overlap if there is no value argument to equal to the average index. In turn it will cause the min value to be greater than the max value.

It should be noted that Binary Search is not a perect soution as it can only work if the array is sorted first from lowest to largest value.



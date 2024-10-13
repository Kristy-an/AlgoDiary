# Python 刷题常用技巧/函数

 



`map()` Function



map(fun, iter) 

Parameters:

`fun`: 函数，将`iter`里面的每个元素pass给该函数

`iter`: An iterable object to be mapped.

 ***\*Parameters:\**** 



-  ***\*fun:\****  It is a function to which map passes each element of given iterable. 
-  ***\*iter:\****  It is iterable which is to be mapped. 

 ***\*NOTE:\****  You can pass one or more iterable to the map() function. 



 ***\*Returns:\****  Returns a list of the results after applying the given function to each item of a given iterable (list, tuple etc.) 



 ***\*NOTE :\****  The returned value from map() (map object) then can be passed to functions like list() (to create a list), set() (to create a set) . 

返回值是map object，需要用`list()`转换成list

```python
num = '131253'
print(list(map(int, num)))
```

Output: 

```
[1, 3, 1, 2, 5, 3]
```







# Python String find() Method

## Definition and Usage

The `find()` method finds the first occurrence of the specified value.

The `find()` method returns -1 if the value is not found.

The `find()` method is almost the same as the `index()` method, the only difference is that the `index()` method raises an exception if the value is not found. (See example below)

------

## Syntax	

*string*.find(*value, start, end*)

## Parameter Values

| Parameter | Description                                                  |
| :-------- | ------------------------------------------------------------ |
| *value*   | Required. The value to search for                            |
| *start*   | Optional. Where to start the search. Default is 0            |
| *end*     | Optional. Where to end the search. Default is to the end of the string |
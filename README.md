
### A cute trick about overflow

```java
(left + right) / 2 = left + (right-left)/2 
```
```java
left * right < target = left < target / right 
```

Left ones are prone to overflow for big left and right values, right ones are safe

```java
Integer.MAX_VALUE = -2^31 = 2147483647
Integer.MIN_VALUE = 2^31 - 1 = -2147483648
```

So when you are converting an Integer to its negative, you should handle the edge case of Integer.MIN_VALUE separately

### Constant time sorting
if you know the range of elements in an array. you can sort it in constant time by mapping counts to indices. for n = [0,50) you need an array of 50 elements You map values to indices and kep count.

```java
int[] exists = new int[50];
for (int i = 0; i < nums.length; i++) {
    exists[nums[i]]++;
}
```

# practise checklist

| subject | details |
| ------- | ------- |
| binary search | Study edge cases

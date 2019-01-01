## Maximum Sum Subarray
https://leetcode.com/problems/maximum-subarray/

[-2,1,-3,4,-1,2,1,-5,4]  

* Keep a running sum
* At each new element 
  * `sum = Math.max(sum + nums[i], nums[i])` : Discard old running sum if nums[i] is bigger.
  * `best = Math.max(best, sum)` : Update if best result
  

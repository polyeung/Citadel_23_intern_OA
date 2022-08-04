Citadel OA 求加米！！！加米不会耗费大家的积分的，求加米，让孩子解锁解锁阅读权限吧，感谢感谢

1.	Longest subarray


思路：sliding window， L,R pointer， 代码如下：
````C++

int maxLength(vector<int> &nums,int k){
    
    int L = 0;
    int R = 0;
    int sum = 0;
    
    int min  = *min_element(nums.begin(), nums.end());
    if(min > k){return 0;}
    else if(min == k){return 1;}
    
    int max_len = 0;
    while( R < nums.size() && L <= R){
        if(sum <= k){
            //expand the window
            max_len = max(R-L ,max_len);
            sum += nums[R++];
        }else{
            max_len = max(R-L-1,max_len);
            sum -= nums[L++];
   
        }
    }
    if(sum <= k){max_len = max(R-L,max_len);}
    
    return max_len;

}
````

2.	Leetcode 910, same as `Sprint training!`
https://leetcode.com/problems/smallest-range-ii/

3.	Do they belong:
   

 
4.	 









5.	Get the groups (https://www.1point3acres.com/bbs/thread-915475-1-1.html)
 
 


Visiting cities:
 
 

#Leetcode 974


 


 



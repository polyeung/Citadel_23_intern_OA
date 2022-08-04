Citadel OA 求加米！！！加米不会耗费大家的积分的，求加米，让孩子解锁解锁阅读权限吧，感谢感谢
求加米！！
求加米！！
求加米！！
求加米！！
求加米！！
求加米！！
OA拿下！！
OA拿下！！
OA拿下！！
OA拿下！！

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


   
<img width="672" alt="image" src="https://user-images.githubusercontent.com/81163933/182755183-51766762-dd6b-4702-a2cf-77e42a91f22e.png">
<img width="411" alt="image" src="https://user-images.githubusercontent.com/81163933/182755217-6622b059-d932-4fd0-beca-33f00d9e5093.png">

`Idea :` 面积法 ，点在三角形内，这个点和其余三点分别形成的三角形 = 总面积
判断是否是三角形，可以用向量判断两条边是否平行
````C++
class my_tri{
private:
    
public:
    my_tri(){std::setprecision(5);}
    
    double cal_tri(pair<int,int> &p_a,pair<int,int> &p_b,pair<int,int> &p_c){
        int x1 = p_a.first;
        int y1 = p_a.second;
        int x2 = p_b.first;
        int y2 = p_b.second;
        int x3 = p_c.first;
        int y3 = p_c.second;
        return 0.5*double(abs(x1*(y2-y3) + x2*(y3-y1) + x3*(y1-y2)));
    }
    
    double get_dist(pair<int,int> &p_a,pair<int,int> &p_b){
        int x1 = p_a.first;
        int y1 = p_a.second;
        int x2 = p_b.first;
        int y2 = p_b.second;
        return sqrt((x1-x2)*(x1-x2) + (y1-y2)*(y1-y2));
    }
    bool can_form(pair<int,int> &p_a,pair<int,int> &p_b,pair<int,int> &p_c){
        
        /*
        double dist_ab = get_dist(p_a,p_b);
        double dist_ac = get_dist(p_a,p_c);
        double dist_cb = get_dist(p_c,p_b);
        
        
        
        if(dist_ab + dist_ac <= dist_cb){return false;}
        if(dist_cb + dist_ab <= dist_ac){return false;}
        if(dist_ac + dist_cb <= dist_ab){return false;}
        return true;
        */
        
        pair<int,int> v_1 = {p_a.first - p_c.first, p_a.second - p_c.second};
        pair<int,int> v_2 = {p_b.first - p_c.first, p_b.second - p_c.second};
        
        if(v_1.first*v_2.second - v_1.second*v_2.first == 0){return false;}
        return true;
        
    }
    
    bool inside_or_not(pair<int,int> &p_a,pair<int,int> &p_b,pair<int,int> &p_c,pair<int,int> &p_d){
        if(!can_form(p_a,p_b,p_c)){cout << "cannot form tri\n";return false;}
        cout << "SIZE of triangle: "<< cal_tri(p_a,p_b,p_c)<<endl;
        if(cal_tri(p_a,p_b,p_c) == cal_tri(p_a,p_b,p_d) + cal_tri(p_a,p_c,p_d) + cal_tri(p_b,p_c,p_d)){
            return true;
        }
        
        
        
        return false;
        
        
        
    }
````

 
4.	Get the Groups




	<img width="692" alt="image" src="https://user-images.githubusercontent.com/81163933/182755427-08a56fb2-ece1-45b7-8bed-417b6656e507.png">
 
<img width="860" alt="image" src="https://user-images.githubusercontent.com/81163933/182755466-3f344920-664f-4274-a936-afdde5c40329.png">

idea: union-find, then use hash_map to count group size for each representitive
you can practice count connected component in leetcode first

````C++
class GROUP{
    vector<int> students;
    
public:
    GROUP(int n){for(int i = 0;i < n + 1;++i){students.push_back(i);}}
    
    
    int count(int & stu_a, int &stu_b,bool debug){
        unordered_map<int,int> my_map; // pres, group size
        
        
        int pre_a = 0;
        int pre_b = 0;
        for(int i = 1;i < students.size();++i){
            
            int pres = find_pres(i);
            if(i == stu_a){pre_a = pres;}
            if(i == stu_b){pre_b = pres;}
            my_map[pres]++;
        }
        
        int ret = 0;
        if(pre_a == pre_b){
            ret = my_map[pre_a];
        }else{
            ret = my_map[pre_a] + my_map[pre_b];
        }
        
        if(debug){
            cout << "students vec: \n";
            for(auto & n : students){cout << n<<" ";
            }
            cout << endl;
            
            for(auto & i : my_map){
                cout << "{Pre : " << i.first << " Count: "<<i.second <<"} ";
            }
            cout << endl;
        }
        
        
        return ret;
        
        
    }
    
    
    
    void execute(vector<string> &q_type, vector<int> &stu1,vector<int> &stu2,bool debug){
        for(int i = 0;i < q_type.size(); ++i){
            int stu_a = stu1[i];
            int stu_b = stu2[i];
            
            if(q_type[i] == "Friend"){
                //union
                union_s(stu_a,stu_b);
            }else if(q_type[i] == "Total"){
                //count total
                int num = count(stu_a,stu_b,debug);
                cout << "group size for "<<stu_a << " and  "<< stu_b<<" is "<<num<<endl;
            }
        }
    }//func
    
    int find_pres(int &a){
        if(students[a] == a){
            return a;
        }
        
        int pres =  find_pres(students[a]);
        students[a] = pres;
        return pres;
    }
    
    void union_s(int & a, int &b ){
        int pre_a = find_pres(a);
        int pre_b = find_pres(b);
        if(pre_a != pre_b){students[pre_a] = pre_b;}
    }
    
    
    
};
````


5.Visiting cities:



<img width="597" alt="image" src="https://user-images.githubusercontent.com/81163933/182755756-f363f4d8-3e1f-451f-87dc-b8d678e15f69.png">

<img width="653" alt="image" src="https://user-images.githubusercontent.com/81163933/182755734-3e583136-0bcc-4595-a6b0-ee920f6a7a86.png">

用两个dp， blue_dp and red_dp,
对于每个blue_dp[i] ： 1. 可以从 red line 到这（加blue_cost）或者直接从blue_dp[i - 1] 过来
注意： 当从red 或 blue取数值的时候应当是 i-1， 因为dp array 是比input array 长度多1的

````C++
vector<int> visiting(vector<int> &red,vector<int>& blue, int bluecost){
    
    vector<int> blue_dp(red.size() + 1, bluecost);
    vector<int> red_dp(red.size() + 1, 0);
    vector<int> ret(blue_dp.size());
    for(int i = 1;i < blue_dp.size();++i){
        blue_dp[i] = min(blue_dp[i-1] + blue[i - 1] , red_dp[i - 1] + blue[i - 1] + bluecost);
        red_dp[i] = min(blue_dp[i-1],red_dp[i-1]) + red[i - 1];
        ret[i] = min(blue_dp[i],red_dp[i]);
    }//for
    
    return ret;
          
        
    
}
````




 
 

6. Leetcode 974

7. odd string or even string

<img width="375" alt="image" src="https://user-images.githubusercontent.com/81163933/182757126-fbf2dc0d-5ada-4f66-919f-9025b83b64c7.png">

思路：利用奇偶性，只需要数有多少个odd number， 偶数个则结果为奇数（偶数个奇数的和为奇数），反之
````C++
string solve(int m, vector<string> s) {
    // fill this function
    
    int count_odd = 0;
    
    for(auto & str : s){
        bool have_even = false;
        for(auto & c : str){
            if((c - 'a') % 2 == 0){
                have_even = true;
            }
        }//for
        
        if(!have_even){count_odd++;}
    }
    cout << count_odd << endl;
    if(count_odd % 2 == 0){
        return "EVEN";
    }
    return "ODD";

}
````


 


 



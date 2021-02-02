[toc]

# :book:LeetCode 2021

<font color=gray size=2>Think twice, code once.</font>

### 双指针

#### 滑动窗口

##### LeetCode 424 <font color=orange>Medium</font>

:bulb: 维护一个滑动窗口，统计当前窗口下出现次数最多的字符，设其出现次数为`max_cnt`，若满足不等式$cnt_{max}+k\geq size_{window}$，则替换k个字符后可以获得连续子串，窗口可以右移，否则窗口滑动

:large_orange_diamond: 使用双指针分别指向窗口的头部和尾部，遍历整个字符串，动态统计窗口内的字符的出现次数

:page_facing_up: 

```c++
class Solution {
public:
    int characterReplacement(string s, int k) {
        vector<int> count(26);
        int left=0,right=0,max_string=0;
        while(right<s.length())
        {
            count[s[right]-'A']++;
            max_string=max(max_string,count[s[right]-'A']);
            if(max_string+k<right-left+1)
            {
                count[s[left]-'A']--;
                left++;
            }
            right++;
        }
        return right-left;
    }
};
```

### 哈希表

##### LeetCode 1 <font color=green>Easy</font>

:bulb: 遍历数组，对遍历的第`i`个数字`nums[i]`，寻找数组中是否有`target-nums[i]`存在

:large_orange_diamond: 维护一个`unordered_map`，遍历到`nums[i]`时在哈希表中搜寻`target-nums[i]`，若不存在则将`nums[i]`加入表中，时间复杂度为`O(N)`

:page_facing_up: 

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map <int,int> mp;
        for(int i=0;i<nums.size();++i)
        {
            if(mp.find(target-nums[i])!=mp.end())
                return {mp.find(target-nums[i])->second,i};
            else(mp[nums[i]]=i);
        }
        return {};
    }
};
```

##### LeetCode 888 <font color=green>Easy</font>

:bulb: 寻找满足等式$sum_A-a+b=sum_B-b+a$的数值对

:large_orange_diamond:上述等式即$a=b+\frac{sum_A-sum_B}{2}$，维护一个`unordered_map`，遍历数组`B`将所有$b+\frac{sum_A-sum_B}{2}$存储起来，再遍历数组`A`，寻找满足的数值对

:page_facing_up:

```c++
class Solution {
public:
    vector<int> fairCandySwap(vector<int>& A, vector<int>& B) {
        int sum_A=accumulate(A.begin(),A.end(),0);
        int sum_B=accumulate(B.begin(),B.end(),0);
        unordered_map<int,int> mp;
        for(auto b:B)
        {
            mp[(sum_A-sum_B)/2+b]++;
        }
        for(auto a:A)
        {
            if(mp.find(a)!=mp.end())
                return vector<int>{a,a-(sum_A-sum_B)/2};
        }
        return {};
    }
};
```

* 补充
  * `for(auto a:b)`用法
    * `for(auto a:b)`中`b`为一个**容器**，效果是利用`a`遍历并获得`b`容器中的每一个值，但是`a`无法影响到`b`容器中的元素
    * `for(auto &a:b)`中加了**引用符号**，可以对容器中的内容进行赋值，即可通过对a赋值来做到容器b的内容填充。
  * [`unordered_map`使用](https://zhuanlan.zhihu.com/p/296360525)
# LeetCode

## Arrays & Hashing

#### [**217 Contains Duplicate**](https://leetcode.com/problems/contains-duplicate/)

This question needs to find out whether a vector includes a duplicate. Note that using two for-loops will cause a timeout for a large dataset($O(n^2)$). In this question, we need to use the **set** to minimize the running time. Here is the [set doc](https://www.cplusplus.com/reference/set/set/).

```c++
#include <set>

class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        std::set<int> check;
        for(int i = 0; i < nums.size(); i++){
            if(check.count(nums[i])){
                return true;
            }
            check.insert(nums[i]);
        }
        return false;
    }
};

//timeout example
bool containsDuplicate(vector<int>& nums) {
  for(int i = 0; i < nums.size() -1; i++){
    for (int j = i + 1; j < nums.size(); j ++){
      if(nums[i] == nums[j]){
        return true;
      }
    }
  }
  return false;
}
```

#### [**242 Valid Anagram**](https://leetcode.com/problems/valid-anagram/)

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

In this question, we choose to use the function **str.find_first_of(key)** to check the char in **t** is in **s**. Then, we erase the according to the character in **t**. However, we use **erase** function in the coding. It is not ideal since removing an element from a string cost about $O(n)$ time. (String is an array, and deleting an element need to resize the string).

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.length() != t.length()) return false;
        for(auto i : t){
            size_t idx = s.find_first_of(i);
            if(idx==std::string::npos){
                return false;
            }
            s.erase(s.begin()+idx);
        }
        return true;
    }
};
```

#### [**1. Two Sum**](https://leetcode.com/problems/two-sum/)

Basic question, here is the code.

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for(int i = 0; i < nums.size() -1; i++){
            for(int j = i+1; j < nums.size(); j++){
                if(nums[i] + nums[j] == target){
                    return vector<int>{i, j};
                }
            }
        }
        return vector<int>();
    }
};
```

#### [**49. Group Anagrams**](https://leetcode.com/problems/group-anagrams/)

Trying to use one additional vector to consist of a map causes a timeout.

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> out;
        vector<string> head;
        for(auto i : strs){
            bool a = false;
            size_t index = 0;
            for(int j = 0; j < head.size(); j ++){
                if(isAnagram(i,head[j])){
                    a = true;
                    index = j;
                    break;
                }
            }
            if(a){
                out[index].push_back(i);
            } else {
                head.push_back(i);
                vector<string> c;
                c.push_back(i);
                out.push_back(c);
            }
        }
        return out;
    }
    
    bool isAnagram(string s, string t) {
        if(s.length() != t.length()) return false;
        for(auto i : t){
            size_t idx = s.find_first_of(i);
            if(idx==std::string::npos){
                return false;
            }
            s.erase(s.begin()+idx);
        }
        return true;
    }
};
```

We can utilize more in-build functions to minimize the running time of our coding. We can change the string into a list of char. Then, we sort the list and compare each key in the map to find the word which is anamgrams.

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> out;
        map<list<char>,vector<string>> check;
        for(auto i : strs){
            list<char> a(i.begin(), i.end());
            a.sort();
            if(check.find(a) != check.end()){
                check[a].push_back(i);
            } else {
                check[a] = vector<string>();
                check[a].push_back(i);
            }
        }
        for(auto & i : check){
            out.push_back(i.second);
        }
        return out;
    }
    
};
```

#### [**347. Top K Frequent Elements**](https://leetcode.com/problems/top-k-frequent-elements/)

When we need to find the frequency, we need to count the variable. Therefore, the map is a good choice. We just need to spend $O(n)$ time to finish the count. Using a map, we can correspond the number and its frequency. Also, we need to find the top k frequency in the map, which cannot be completed using the in-build function. In this question, we use **std::max_element** to find the largest value.

```c++
bool compare(const pair<int, int>&a, const pair<int, int>&b){
    return a.second<b.second;
}
class Solution {
public:
    
    vector<int> topKFrequent(vector<int>& nums, int k) {
        map<int, int> check;
        for( auto i : nums){
            if(check.find(i) != check.end()){
                check[i] += 1;
            } else {
                check[i] = 1;
            }
        }
        vector<int> out;
        for(int i = 0; i < k; i++) {
             auto a = max_element(check.begin(), check.end(), compare)->first;
            check[a] = 0;
            out.push_back(a);
        }
        return out;
    }
};
```

#### [**238. Product of Array Except Self**](https://leetcode.com/problems/product-of-array-except-self/)

We need to implement a solution without division, given $O(n)$ time. This means that we cannot use double for-loop. Therefore, we need to save the answer for each time in back and forth. The **answer[i]** will be the according element in two vector.

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> front(nums.size());
        front[0] = nums[0];
        vector<int> back(nums.size());
        back[nums.size()-1] = nums[nums.size()-1];
        
        for(int i = 1; i < nums.size(); i++) {
            front[i] = (front[i-1]*nums[i]);
        }
        for(int i = nums.size()-2; i > 0; i--) {
            back[i] = (back[i+1]*nums[i]);
        }
        vector<int> out;
        for (int i = 0; i < nums.size(); i++){
            if(i == 0)  out.push_back(back[i+1]);
            else if(i == nums.size()-1) out.push_back(front[i-1]);
            else out.push_back(front[i-1]*back[i+1]);
        }
        return out;
    }
};
```


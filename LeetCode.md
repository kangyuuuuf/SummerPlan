# LeetCode

## Arrays & Hashing

#### [**217 Contains Duplicate**](https://leetcode.com/problems/contains-duplicate/)

This question needs to find out whether a vector includes a duplicate. Note that using two for-loops will cause a timeout for a large dataset($O(n^2)$). In this question, we need to use the **set** to minimize the running time. Here is the [set doc](https://www.cplusplus.com/reference/set/set/).

##### CPP

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

##### JAVA

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> numbers = new HashSet<Integer>();
        for (int num : nums) {
            if (numbers.contains(num)) return true;
            numbers.add(num);
        }
        return false;
    }
}
```

##### PYTHON

```py
class Solution(object):
    def containsDuplicate(self, nums):
        hashset = set()
        for n in nums:
            if n in hashset:
                return True
            hashset.add(n)
        return False
```



#### [**242 Valid Anagram**](https://leetcode.com/problems/valid-anagram/)

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

In this question, we choose to use the function **str.find_first_of(key)** to check the char in **t** is in **s**. Then, we erase the according to the character in **t**. However, we use **erase** function in the coding. It is not ideal since removing an element from a string cost about $O(n)$ time. (String is an array, and deleting an element need to resize the string).

Second way: the easiest way to do this is sort two strings so that will be much eaiser to compare. The sort time complexity will be $O(nlogn)$ which will be quick. 

##### CPP

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

//second way
#include<bits/stdc++.h>
using namespace std;

class Solution {
public:
    bool isAnagram(string s, string t) {
        sort(s.begin(), s.end());
        sort(t.begin(), t.end());
        cout << s;
        cout << t;
        if (s != t) {
            return false;
        }
        return true;
    }
};
```



##### JAVA

```java
import java.util.Arrays;

class Solution {
    public boolean isAnagram(String s, String t) {
        char Array1[] = s.toCharArray();
        char Array2[] = t.toCharArray();
        Arrays.sort(Array1);
        Arrays.sort(Array2);
        String str1 = new String(Array1);
        String str2 = new String(Array2);
        
        return str1.equals(str2);
    }
}
```

##### PYTHON

```pyth
class Solution(object):
    def isAnagram(self, s, t):
        a = sorted(s)
        b = sorted(t)
        if a == b:
            return True
        return False
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

#### [**36. Valid Sudoku**](https://leetcode.com/problems/valid-sudoku/)

Since it is likely that there is a sparse matrix given from the question, we could check the valid number cell each time.  When we iterate a valid cell, check its row and column. And We check the sub-box.

```c++
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        for(int i = 0; i < 9; i++){
            for(int j =0; j < 9; j++){
               if(board[i][j] != '.'){
                   for(int k = 0; k < 9; k++){
                       if(board[i][k] == board[i][j] && j != k){
                           return false;
                       }
                       if(board[k][j] == board[i][j] && i != k){
                           return false;
                       }
                       
                   }
                   int xp = i/3*3;
                   int yp = j/3*3;
                   for(int p =0; p < 3; p++) {
                       for(int q = 0; q < 3; q++){
                           int x =p + xp;
                           int y =q + yp;
                           if(board[x][y] == board[i][j] && (j != y || i != x)){
                               return false;
                            }
                       }
                   }
               } 
            }
        }
        return true;
    }
};
```



#### [**Encode and Decode Strings**](https://www.lintcode.com/problem/659/)

Although we could choose the divided mark by ourselves,  the outcome after encoding and decoding must be the same. For example, if we use “:” to divide the string, we need to handle the situation that “:” is already existed in string.

**substr** takes two arguments, the start position, and the substring length! **NOT THE END POSITION!!!!**

The difference between **find** and **find_first_of**: **find** is to find the first appeared given string, while **find_first_of** is to find the first appeared character among the given string.

```c++
class Solution {
public:
    /*
     * @param strs: a list of strings
     * @return: encodes a list of strings to a single string.
     */
    string encode(vector<string> &strs) {
        // write your code here
        string out = "";
        if(strs.size() == 0) return out;
        if(strs.size() == 1) return strs[0];
        out += strs[0];
        size_t idx = 1;
        for(int i = 1; i < strs.size(); i++){
            out += ":;";
            string check = strs[i];
            size_t found = check.find(':');
            while(found != string::npos){
                check.replace(found,1,"::");
                found = check.find(':', found+2);
            }
            out += check;
        }
        cout << out << endl;
        return out;
    }

    /*
     * @param str: A string
     * @return: dcodes a single string to a list of strings
     */
    vector<string> decode(string &str) {
        // write your code here
        if(str.length() == 0) return vector<string>();
        vector<string> out;
        size_t start = 0;
        size_t found = str.find(":;");
        while(found != string::npos){
            string input = str.substr(start,found-start);
            size_t check = input.find("::");
            if(check != string::npos){
                input.replace(check,2,":");
            }
            start = found + 2;
            out.push_back(input);
            found = str.find(":;", found+2);
            cout<< input << endl;
        }
        string input = str.substr(start,str.length());
        size_t check = input.find("::");
        if(check != string::npos){
            input.replace(check,2,":");
        }
        out.push_back(input);
        return out;
    }
};
```

#### [**128. Longest Consecutive Sequence**](https://leetcode.com/problems/longest-consecutive-sequence/)

We need to find the longest consecutive sequence given $O(n)$ time. This means that we can not use double loops, but we can sort the given vector and traverse the sorted vector to gain $O(2n)$ time. Use **count** to check the largest count number and **check** to trace each consecutive sequence. There may exist some improvement for the variable boolean **start**. 

**sort** function is in STL library, which can sort the vector.

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int count = 0;
        int check = 0;
        bool start = true;
        for(int i = 0; i < nums.size(); i++){
            if(start || nums[i-1]+1 == nums[i]){
                check++;
                start = false;
                
            } else if(!start && nums[i-1] == nums[i]){
                start = false;
            } else{
                if(count < check) {
                    count = check;
                }
                check = 1;
                start= false;
            }
        }
         if(count < check) {
                    count = check;
                }
        return count;
    }
};
```

## Two Pointers

**Two Pointers Technique**	Two pointers is really an easy and effective technique that is typically used for searching pairs in a **sorted** array.

#### [**125. Valid Palindrome**](https://leetcode.com/problems/valid-palindrome/)

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

In this question, we need to do two preparations: change the string to lower case and remove all non-alphanumeric characters. We need to use the function **remove_if** to get the index of characters and use **erase** to delete the corresponding characters. Note that we can put the function as a parameter. Then, we can use the function **remove_if** in various conditions. For example:

```c++
auto it = remove_if(a,b,c);
//a as the start of the object
//b as the end of the object
//c as the judgement function, determine which elements need to remove.
```

**c** could be a boolean function to check the lowercase or non-alphanumeric characters.

Then, we need to get a new string that is the original reverse. Using the in-build function and string constructor, we can get the reverse. **rbegin()** and **rend()** is the reverse iteration of string.

```c++
//s is the orginal string
string rev = string(s.rbegin(), s.rend());
```

 Then, we check wether they are equal.

```c++
class Solution {
public:
    bool isPalindrome(string s) {
        auto it = remove_if(s.begin(),s.end(), [](char const & c){
            return !isalnum(c);
        });
        s.erase(it,s.end());
        std::transform(s.begin(), s.end(), s.begin(),
        [](unsigned char c){ return std::tolower(c); });
        string rev = string(s.rbegin(), s.rend());
        return rev == s;
    }
};
```

#### [**167. Two Sum II**](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

Similar to Two Sum. Note that it is a not-decreasing order vector, which means it has more information than the original question. To utilize this info, we can set the check celling for the vector and find the other number from the back. Since we know that the previous element is smaller or equal to the current element, the back elements checked from the previous loop are 100% invalid for the current one. Here is the example

```c++
a = [1,2,3,4,6,7,9];
target = 6;
//In the first loop, we will find the pair of number 1. We need to find the number of 5
//Check from the back, we know that sub-array = [6,7,9] are impossible since it is larger than 5
//This means for the following loop, we do not need to consider this sub-array because the number we need to find must be less than 5. Then, we set celling as 4.
//In the second loop, we will find the pair of number 2. We need to find the number of 4.
//Because we have the celling, we will hit the answer for the first find, which is 4.
```

This algorithm is beneficial. The running time will become $O(n)$. We do not need to consider the number hit (**may not** use the same element twice.) since we will find the right answer before the number hit.

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int end  = numbers.size()-1;
        for(int i = 0; i < numbers.size(); i++) {
            int check = target - numbers[i];
            for(int j = end; j > i; j--){
                if(numbers[j] == check){
                    auto a = vector<int>();
                    a.push_back(i+1);
                    a.push_back(j+1);
                    return a;
                } else if(numbers[j] < check){
                    break;
                }
                end--;
            } 
        }
        return vector<int>();
    }
};
```

#### [**15. 3Sum**](https://leetcode.com/problems/3sum/)

We need to find a solution that is less than $O(n^3)$ or a time Exceeded will occur.  We can get the idea from **Two Sum II**, sorting the vector and using two pointers to find the possible solution. Then, the question will become $n$ times **Two Sum** questions. (Iterate each element in the vector and regard it as the **target**).

Also, we need to consider the duplicate problem. After the first iteration, we need to check whether the current element equals the last element. If the condition is true, pass the current iteration. This solution is not ideal, but it is the best answer I can get.

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        if(nums.size() < 3) return vector<vector<int>>();
        sort(nums.begin(), nums.end());
        vector<vector<int>> out;
        for(int i = 0; i < nums.size()-2; i++) {
            if(i > 0){
                if(nums[i] == nums[i-1]){
                    continue;
                }
            }
            int front = i + 1;
            int back = nums.size()-1;
            int current = 0;
            while(front < back){
                if(front > i + 1){
                    if(nums[front] == nums[front-1]){
                        front++;
                        continue;
                    }
                }                
                if(back < (nums.size()-1)){
                    if(nums[back] == nums[back+1]){
                        back--;
                        continue;
                    }
                }
                current = -(nums[front] + nums[back]);
                if(current > nums[i]){
                    front++;
                } else if(current < nums[i]){
                    back--;
                } else {
                    out.push_back({nums[i],nums[front],nums[back]});
                    back--;
                    front++;
                }
            }
        }
        return out;
    }
};
```

#### [**11. Container with Most Water**](https://leetcode.com/problems/container-with-most-water/)

In this question, of course, we can use two for-loops to solve the question, making a $O(n^2)$ running time. However, there is an algorithm that can reduce the running time to $O(n)$.

1. Set **front** as 0 and **back** as the last index of height.
2. calculate the area, change the max value if necessary
3. find the min(**front**, **back**), moving the index with a smaller value inward. (front++/back- -).
4. Repeat step 2 until **front** >= **back**.

As far as I could say,  each step we move is based on the current best situation. However, it is hard to prove.

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int front = 0;
        int back = height.size() - 1;
        int current = 0;
        int maxv = 0;
        while (front < back) {
            current = (back - front) * min(height[front], height[back]);
            maxv = max(maxv,current);
            if(height[front] > height[back]){
                back--;
            } else {
                front++;
            }
        }
        return maxv;
    }
    int min(int a, int b) {
        if (a > b) return b;
        return a;
    }
    int max(int a, int b) {
        if (a > b) return a;
        return b;
    }
};
```

#### [**42. Trapping Rain Water**](https://leetcode.com/problems/trapping-rain-water/)

The same type of question. We need two-pointers and choose the next step base on the current situation. When we need to solve this type  of question, we need to control our running time as $O(n)$. When we have two pointers, front and back, we can move one pointer each time to solve the piece of the problem.

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int level = 0;
        int front = 0;
        int back = height.size()-1;
        int sum = 0;
        while(front < back){
            if(height[front] < level) sum += (level - height[front]);
            if(height[back] < level) sum += (level - height[back]);
            if(height[front] < height[back]){
                level = max(height[front], level);
                front++;
            } else {
                level = max(height[back], level);
                back--;
            }
        }
        return sum;
    }
    int max(int a, int b){
        if(a > b) {
            return a;
        }
        return b;
    }
};
```

## Window Sliding

**Window Sliding Technique**	Window sliding is a computational technique that aims to reduce the use of nested loops and replace it with a single loop, thereby reducing the time complexity.

**Prerequisite to use Sliding window technique**	The use of Sliding Window technique can be done in a very specific scenario, where the **size of window** for computation is **fixed** throughout the complete nested loop. Only then the time complexity can be reduced. 

1. Find the size of window required 
2. Compute the result for 1st window, i.e. from start of data structure
3. Then use a loop to slide the window by 1, and keep computing the result window by window.

#### [**121. Best Time to Buy & Sell Stock**](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

This question is not an ideal window sliding question. However, the running time can be reduced to $O(n)$ ;

Since we know that the selling date is after buying date, we can set our current selling date as the last day. When we iterate the array from behind. If we find another higher price(greater than the current max selling price), we can replace the max price. Or, we can regard the current day as buying date and calculate the profit.

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxi = prices[prices.size() -1];
        int profit = 0;
        for(int i = prices.size() -2; i > -1; i--) {
            if(prices[i] > maxi){
                maxi = prices[i];
            }
            int cur = maxi - prices[i];
            profit = max(cur, profit);
        }
        return profit;
    }
};
```

Also, we can set the current buying date as the first day and iterate the array. It seems that it is more efficient than we iterate backward.

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int mini = prices[0];
        int profit = 0;
        for(int i = 1; i < prices.size(); i++) {
            if(prices[i] < mini){
                mini = prices[i];
            }
            int cur = prices[i] - mini;
            profit = max(cur, profit);
        }
        return profit;
    }
};
```

#### [**3. Longest Substring Without Repeating Characters**](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

The window can be changed in this question. When we use **std::string::find** to find a repeating character, we need to set our window to correct value. 

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        if(s.length() == 0) return 0;
        int a = 0;
        int b = 1;
        int l = 1;
        int count = 1;
        while (b < s.length()) {
            size_t c = s.substr(a,b-a).find(s[b]);
            if( c == std::string::npos){
                count++;
                if(count > l) l = count;
                b++;
            } else {
                if(count > l) {
                    l = count;
                }
                a = a+c+1;
                count = b-a + 1;
                b++;
            }
        }
        return l;

    }
};
```

#### [**424. Longest Repeating Character Replacement**](https://leetcode.com/problems/longest-repeating-character-replacement/) (Unable to Solve)

The solution to this problem is written based on the discussion from [link](https://leetcode.com/problems/longest-repeating-character-replacement/discuss/2130806/C%2B%2Beasy-to-understand). However, his solution is complicated and includes some meaningless lines. The code is improved to increase the readability and it can be divided into the following steps.

1. set the window (left and right), a number to count the current most frequent char, a number to count the length, and a map to count the frequency of chars.
2. Update the frequency in the map for the current char on the right.
3. check whether the current window is valid
   - if it is valid, check the current window length and update the outcome if it is smaller. Right++
   - if it is invalid, the current window needs to shrink. Left++, Right++, and update the frequency due to left change
4. Repeat step 2 if Right is less than size of string

```c++
class Solution {
public:
    int characterReplacement(string s, int k) {
        int l = 0;
        int r = 0;
        int out = 0;
        int count = 0;
        unordered_map<char,int> check;
        while (r < s.length()){
            check[s[r]]++;
            count = max(count, check[s[r]]);
            if((r-l+1-count)>k) {
                check[s[l]]--;
                l++;
                r++;
            } else {
                out = max(out, r-l+1);
                r++;
            }
        }
        return out;
    }
};
```

#### [**567. Permutation in String**](https://leetcode.com/problems/permutation-in-string/)

In this question, we still use unordered_map to solve the problem. We set a variable **count**, using **count** to check whether the current window is a premutation of **s1**. If **count** drops to zero, it means that we find the correct window. Note that in this question, the window is fixed. But we still use changeable variables, l and r, to set the window because we need to start to count at **s2** start.

```c++
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        unordered_map<char,int> check;
        for(auto a : s1){
            check[a]++;
        }
        int count = check.size();
        int l = 0;
        int r = 0;
        while(r < s2.size()){
            if(check.find(s2[r])  != check.end()){
                check[s2[r]]--;
                if(check[s2[r]] == 0){
                    count--;
                }
            }
            if((r-l+1) < s1.size()) r++;
            else if ((r-l+1) == s1.size()){
                if(count == 0) return true;
                if(check.find(s2[l]) != check.end()){
                    if(check[s2[l]] == 0) count++;
                    check[s2[l]]++;
                }
                l++;
                r++;
                
            }
        }
        return false;
    }
};
```

#### [**76. Minimum Window Substring**](https://leetcode.com/problems/minimum-window-substring/)

We can still use Sliding Window to solve this problem. However, there are a few adjustments to the window boundary. Here is the strategy:

- If the current window does not contain all the chars in **s**, Right++. Next iteration checks the right boundary.
- If the current window does contain all the chars in **s**, Left++. Next iteration checks the left boundary.
- When the **count** change from 0 to 1, it means that Left++ in the last iteration will cause an invalid window string. Therefore, the window in the last iteration is the valid optimized substring. Check it with the current minimum **len**.

```c++
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char,int> check;
        for(auto a : t) check[a]++;
        int l = 0;
        int r = 0;
        int ls = 0;
        int len = numeric_limits<int>::max();
        int count = check.size();
        int rc = true;
        int had = false;
        while (r < s.length()) {
            if(rc){
                if(check.find(s[r]) != check.end()){
                    check[s[r]]--;
                    if(check[s[r]] == 0){
                        count--;
                    }
                }
            } else {
                if(check.find(s[l-1]) != check.end()){
                    check[s[l-1]]++;
                    if(check[s[l-1]] == 1){
                        count++;
                        if(count == 1){
                            if (r-l+2 < len) {
                                len = r-l+2;
                                ls = l-1;
                                had = true;
                            }
                        }
                    }
                }
            }
            if(count == 0) {
                l++;
                rc = false;
                
            } else {
                r++;
                rc = true;
            }
        }
        if(had) return s.substr(ls,len);
        return "";
    }
};
```

#### [**239. Sliding Window Maximum**](https://leetcode.com/problems/sliding-window-maximum/)

Using **priority_queue** and **pair<int,int>**.

```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        priority_queue<pair<int, int> > pq;
        vector<int> out;
        for(int i = 0; i < k; i++) {
            pq.push(make_pair(nums[i],i));
            
        }
        out.push_back(pq.top().first);
        for(int i = k; i < nums.size(); i++){
            pq.push(make_pair(nums[i],i));
            while(true){
                pair<int, int> top = pq.top();
                if(top.second < (i-k+1)) {
                    pq.pop();
                } else {
                    out.push_back(top.first);
                    break;
                }
            } 
        }
        return out;
    }
};
```

## Stack

Stacks are a type of container adaptors with **LIFO**(Last In First Out) type of working, where a new element is added at one end (top) and an element is removed from that end only.

#### [**20. Valid Parentheses**](https://leetcode.com/problems/valid-parentheses/)

We can divide chars in the string into 2 parts: “(, {, [” and “), }, ]”. When we iterate the first part, we add the element to the stack. When we iterate the second part, we check the top of the stack. If it is not the same group of parentheses, return false. Also, we need to check whether the stack is empty to make sure the action of pop/top is valid.

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> a;
        for(auto b : s) {
            if(b == '(' || b == '[' || b == '{') {
                a.push(b);
            } else {
                if(a.empty()) return false;
                if(b == ')') {
                    if(a.top() != '(') {
                        return false;
                    }
                } else if(b == ']') {
                    if(a.top() != '[') {
                        return false;
                    }
                } else {
                    if(a.top() != '{') {
                        return false;
                    }
                }
                a.pop();
            }
        }
        if(!a.empty()) return false;
        return true;
    }
};
```

#### [**155. Min Stack**](https://leetcode.com/problems/min-stack/)

In this question, we need to implement all the functions and constructors in constant time. Most of the implementations can use an in-built **stack** to build. However, we need to handle the relation between **pop** and **getMin**. The max element will be removed when we called **pop**, and it is unrealistic that we find the second large element in the stack. Therefore, we need to use another stack to record the largest element for each iteration.  When the current max element is popped, we also pop the second stack and check the top. The current top will be the second-largest element that existed in the stack.

```c++
class MinStack {
public:
    MinStack() {
       b.push(INT_MAX); 
    }
    
    void push(int val) {
        a.push(val);
        if(val <= m) {
            m = val;
            b.push(m);
        }
    }
    
    void pop() {
        if(a.top() == b.top()) {
            b.pop();
            m = b.top();
        }
        a.pop();
    }
    
    int top() {
        return a.top();
    }
    
    int getMin() {
        return m;
    }
private:
    stack<int> a;
    stack<int> b;
    int m = INT_MAX;
};
```

#### [**150. Evaluate Reverse Polish Notation**](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

Simple stack question.

```c++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> b;
        for(auto a : tokens) {
            if(a == "+") {
                int s = b.top();
                b.pop();
                int f = b.top();
                b.pop();
                b.push(f+s);
            } else if(a == "-") {
                int s = b.top();
                b.pop();
                int f = b.top();
                b.pop();
                b.push(f-s);
            } else if(a == "*") {
                int s = b.top();
                b.pop();
                int f = b.top();
                b.pop();
                b.push(f*s);
            } else if(a == "/") {
                int s = b.top();
                b.pop();
                int f = b.top();
                b.pop();
                b.push(f/s);
            } else {
                b.push(stoi(a));
            }
        }
        return b.top();
    }
};
```

#### [**22. Generate Parentheses**](https://leetcode.com/problems/generate-parentheses/) (Solved by Recursion)

In this question, we need to follow the basic rule to create a valid parenthesis.

- The number of “(” must be larger or equal to the number of “)”.

Since the variety of the string is not linear (string can add “(” or “)” when the above condition is true), we choose to use recursion.

```
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> out;
        
        help(out,"",0,0,n);
        return out;
    }
    void help(vector<string> & out, string cur, int a, int b,int n) {
        if(a == b) {
            if(a == n) {
                out.push_back(cur);
                return;
            }
            help(out, cur + '(', a+1, b, n);
        } else {
            if(a < n) {
                help(out, cur + '(', a+1, b, n);
            }
            help(out, cur + ')', a, b+1, n);
        }
        return;
    }
};
```

After we browsed the discussion website, we found that this method is called **Backtracking Algorithm**(The key words: find all the possible solutions). A backtracking algorithm is a problem-solving algorithm that uses a **brute force approach** for finding the desired output. The term backtracking suggests that if the current solution is not suitable, then backtrack and try other solutions. Thus, recursion is used in this approach. This approach is used to solve problems that have multiple solutions. Here is the algorithm:

```pseudocode
Backtrack(x)
    if x is not a solution
        return false
    if x is a new solution
        add to list of solutions
    backtrack(expand x)
```

#### [**739. Daily Temperatures**](https://leetcode.com/problems/daily-temperatures/)(Solved by Priority_queue)

Due to the feature of priority_queue, we can find the day that has a colder temperature in the previous compared to the current temperature. 

```c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>> > pq;
        vector<int> out(temperatures.size());
        for(int i = 0; i < temperatures.size(); i++) {
            while(!pq.empty()) {
                auto a = pq.top();
                if(a.first < temperatures[i]) {
                    out[a.second] = i - a.second;
                    pq.pop();
                } else {
                    break;
                }
            }
            pq.push(make_pair(temperatures[i],i));
        }
        while(!pq.empty()) {
            auto a = pq.top();
            out[a.second] = 0;
            pq.pop();             
        }
        return out;
    }
};
```

We found that it is the subquestion of **Question 84**. Let’s try to solve it with the stack.

```c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> a;
        vector<int> out(temperatures.size());
        for(int i = temperatures.size()-1; i >= 0 ; i--) {
            while(!a.empty() && temperatures[a.top()] <= temperatures[i]) a.pop();
            out[i] = a.empty()? 0:a.top()-i;
            a.push(i);
        }
        return out;
    }
};
```

#### [**853. Car Fleet**](https://leetcode.com/problems/car-fleet/)

In this question, the current car could only catch up with the car fleet that has the closest car that is also closer to the target. Then, for each car, we just need to find the right car fleet to check. Since it depends on the position, we need a sorted vector with pairs to store all the information. After the vector is descending, we use a stack to get the last car fleet for the current iteration.

```c++
class Solution {
public:
    int carFleet(int target, vector<int>& position, vector<int>& speed) {
        stack<pair<int,int>> a;
        vector<pair<int,int>> b;
        for(int i = 0; i < position.size(); i++) {
            b.push_back({position[i],speed[i]});
        }
        sort(b.begin(),b.end(),greater<pair<int,int>>());
        for(auto c : b) {
            if(a.empty()){
                a.push(c);
                continue;
            } else {
                auto d = a.top();
                if(((double)(target-d.first)/d.second) >= ((double)(target-c.first)/c.second)){
                    continue;
                } else {
                    a.push(c);
                }
            }
        }
        return a.size();
    }
};
```

#### [**84. Largest Rectangle in Histogram**](https://leetcode.com/problems/largest-rectangle-in-histogram/)(Unable to Solve)

The solution is from the discussion. It follows the basic rule: for each element, find the position of the nearest smaller element on the right and left. Then, we will get two vectors to show the width of the rectangle that can make for the current height. Finally, we just need to iterate all the elements to find the max area.

```c++
class Solution {
public:
    vector<int> left(vector<int> & a) {
        stack<int> b;
        vector<int> out(a.size());
        for(int i = 0; i < a.size(); i++) {
            while(!b.empty() && a[i] <= a[b.top()]) b.pop();
            
            int top=(b.empty())?-1:b.top();
            out[i] = top;
            b.push(i);
        }
        return out;
    }
    vector<int> right(vector<int> & a) {
        stack<int> b;
        vector<int> out(a.size());
        for(int i = a.size()-1; i >= 0; i--) {
            while(!b.empty() && a[i] <= a[b.top()]) b.pop();
            
            int top=(b.empty())?a.size():b.top();
            out[i] = top;
            b.push(i);
        }
        return out;
    }
    int largestRectangleArea(vector<int>& heights) {
        vector<int> a = left(heights);
        vector<int> b = right(heights);
        int out = 0;
        for(int i = 0; i < heights.size(); i++) {
            out = max(out,heights[i] * (b[i] - a[i] -1));
        }
        return out;
    }
};
```

## Binary Search

Binary Search can be used in a sorted vector/array. For each iteration, we will choose the middle of the scope to check. If it is larger than the target, the scope changes from the middle to the end. If it is smaller than the target, the scope changes from the beginning to the middle. The time cost of Binary Search is $O(\log n)$.

#### [**704. Binary Search**](https://leetcode.com/problems/binary-search/)

Basic Binary Search.

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0;
        int r = nums.size() -1;
        while(r >= l){
          	int idx = (r+l)/2;
            if(nums[idx] > target) {
                r = idx - 1;
            } else if(nums[idx] < target) {
                l = idx + 1;
            } else {
                return idx;
            }
        }
        return -1;
    }
};
```

#### [**74. Search a 2D Matrix**](https://leetcode.com/problems/search-a-2d-matrix/)

It is just a 2D Binary Search. My original solution is not ideal since I need to check the boundary separately. In the new answer, I handle the change of boundary, making it more effective.

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int n = matrix.size();
        int m = matrix[0].size();
        int cell = 0;
        int floor = n - 1;
        while(floor >=cell) {
            int idxy = (floor + cell)/2;
            if(matrix[idxy][m-1] < target ) {
                cell = idxy +1;
            } else if(matrix[idxy][0] > target) {
                floor = idxy-1;
            } else {
                return search(matrix[idxy],target);
            }
        }
        return false;
    }
    bool search(vector<int>& nums, int target) {
        int idx = nums.size()/2;
        int l = 0;
        int r = nums.size() -1;
        while(r-l > 1){
            if(nums[idx] > target) {
                r = idx;
                idx = (idx+l)/2;
            } else if(nums[idx] < target) {
                l = idx;
                idx = (idx+r)/2;
            } else {
                return true;
            }
        }
        if(nums[r] == target) return true;
        if(nums[l] == target) return true;
        return false;
    }
};
```

#### [**875. Koko Eating Bananas**](https://leetcode.com/problems/koko-eating-bananas/)

Let's consider in what situation we need to use the binary search: Given a sorted array. In this question, the vector is not sorted. However, we do not need to find the answer within the given vector. We need to find the smallest speed of eating given the amount of banana. Then, we can infer that the speed is $0<x\leq\max$. Within this scope, all the elements are possible, and it is sorted. Therefore, we can utilize the binary search.

```c++
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int l = 1;
        int r = *max_element(piles.begin(), piles.end());
        while(r > l) {
            int mid = (r+l)/2;
            cout<<l << " " << r << endl;
            if(check(piles,mid,h)){
                r = mid;
            } else {
                l = mid+1;
            }
        }
        return r;
    }
    bool check(vector<int>& a, int speed, int h) {
        int timecost = 0;
        for(auto & b : a) {
            timecost+= b/speed + (b % speed != 0);
        }
        return timecost <= h;
    }
};
```

#### [**33. Search Rotated Sorted Array**](https://leetcode.com/problems/search-in-rotated-sorted-array/)

There are two steps of this question: 

1. Find the rotation index
2. Binary Search

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int maxElementIndex = std::max_element(nums.begin(),nums.end()) - nums.begin();
        int index = nums.size() - 1 - maxElementIndex;
        int l = 0;
        int r = nums.size() - 1;
        while(r >= l) {
            int mid = (r+l)/2;
            int currentidx = changeidx(mid, maxElementIndex, index);
            if(nums[currentidx] > target) {
                r = mid - 1;
            } else if (nums[currentidx] < target) {
                l = mid + 1;    
            } else {
                return currentidx;
            }
        }
        return -1;
    }
    int changeidx(int & idx, int & maxElementIndex,int & index) { 
            if(idx >= index) return idx - index;
            else return idx + maxElementIndex + 1;
    }
};
```

#### [**153. Find Minimum in Rotated Sorted Array**](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

In this question, we need to check when does the boundary need to change given a rotated sorted array. When we know that the mid element is within the scope of both sides of the element, the left side is the min element. 

``` c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0;
        int r = nums.size()-1;
        while(r >= l) {
            int mid = (r+l)/2;
            if(check(nums, mid, r, l)){
                return nums[l];
            }
        } 
        return nums[0];
    }
    bool check(vector<int>& nums, int & idx, int & r, int & l) {
        if(nums[idx] >= nums[l] && nums[idx] > nums[r]) {
            l = idx + 1;
        } else if(nums[idx] >= nums[l] && nums[idx] <= nums[r]) {
            return true;
        } else if (nums[idx] <= nums[l] && nums[idx] < nums[r]) {
            r = idx;
        }
        return false;
    }
};
```

#### [**981. Time Based Key-Value Store**](https://leetcode.com/problems/time-based-key-value-store/)

Remember the timestamp is ascending, it means the vector we push_back is always sorted. This means we can use binary search.

```c++
class TimeMap {
public:
    TimeMap() {
        
    }
    
    void set(string key, string value, int timestamp) {
        a[key].push_back({timestamp,value});
    }
    
    string get(string key, int timestamp) {
        if(a.find(key) == a.end()) {
            return "";
        }
        int idx = search(a[key],timestamp);
        if(idx == -1) return "";
        return a[key][idx].second;
    }
private:
    unordered_map<string,vector<pair<int, string>>> a;
    int search(vector<pair<int, string>> & nums, int target) {
        int l = 0;
        int r = nums.size() -1;
        while(r >= l){
          	int idx = (r+l)/2;
            if(nums[idx].first > target) {
                r = idx - 1;
            } else if(nums[idx].first < target) {
                l = idx + 1;
            } else {
                return idx;
            }
            if(r < l) {
                if(idx == 0 && nums[idx].first > target) return -1;
                if( nums[idx].first > target) return idx-1;
                return idx;
            }
        }
        return l;
    }
    
};
```


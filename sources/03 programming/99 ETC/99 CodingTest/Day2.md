# Day2

## 문제 
https://leetcode.com/problems/remove-element/description/?envType=study-plan-v2&envId=top-interview-150
* Remove Element

## 풀이

### Initial
``` cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int r = 0; //num removed
        const int n = nums.size();

        for (int i=0; i<n-r; ++i)
        {
            if (nums[i] != val) continue;

            while( (n-r-1 >= 0) && (nums[n-r-1] == val) )
            {
                r++;
            }

            if ( n-r-1 <= i ) break;            

            swap(nums[i], nums[n-r-1]);
            r++;
        }
        
        return n-r;         
    }
};
```
개선점
* swap 을 해야 된다는 생각으로 코드가 복잡해졌다.
  * 처음부터 다시 배열을 inplace 로 작성한다는 생각을 했으면 훨씬 간단한 코드를 작성할 수 있다. ( Simple 참고)
  * 단, write 횟수가 swap-tail 방식 보다는 많다.
* head 와 tail 을 동시에 하나씩 옮기려고 해서 코드가 복잡해졌다.
  * head - tail swap 후 tail 만 옮기고 head 는 다시 평가하면 코드가 훨씬 간단해진다. ( Swap Tail Simple 참고 )

### Simple
``` cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int k = 0;
        for (int num : nums)
        {
            if ( num == val ) continue;
            nums[k++] = num;
        }

        return k;
    }
};
```

### Swap Tail Simple
``` cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int i = 0;             // element index
        int l = nums.size()-1; // last element index
        while( i <= l )
        {
            if (nums[i] != val) {
                i++; 
                continue;
            } 

            swap(nums[i], nums[l]);
            l--;
        }
        return i;
    }
};
```
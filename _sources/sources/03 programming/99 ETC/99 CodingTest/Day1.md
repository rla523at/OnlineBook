# Day1
2025.12.24

## 문제 
https://leetcode.com/problems/merge-sorted-array/description/?source=submission-ac
* Merge Sorted Array

## 풀이
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m-1;
        int j = n-1;
        int k = m+n-1;

        while(0<=j)
        {
            if( (0<=i) && (nums2[j] < nums1[i]) )
                nums1[k--] = nums1[i--];
            else 
                nums1[k--] = nums2[j--];
        }
    }
};
```

## 코멘트
nums1 의 앞에서 부터 채우려고 하니 상당히 복잡해졌다.
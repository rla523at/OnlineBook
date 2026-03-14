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

* nums1 의 앞에서 부터 채우려고 하니 상당히 복잡해져서 풀지 못했다. 
* while 문의 종료조건을 k 로 두니 예외 처리가 상당히 복잡해졌다.
  * 어떤 종료조건을 선택하냐에 따라서 코드 구현 복잡도가 달라진다.
  * j 를 종료조건으로 두면 코드가 간단해진다.



# PROGRAMMING PROBLEMS

1. ## Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.</h2>[Leetcode](https://leetcode.com/problems/search-in-rotated-sorted-array/)

    (i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).

    You are given a target value to search. If found in the array return its index, otherwise return -1.

    You may assume no duplicate exists in the array.

    Your algorithm's runtime complexity must be in the order of O(log n).

    **Example 1:**
    **Input**: nums = [4,5,6,7,0,1,2], target = 0.
    **Output**: 4 </br>

    **Example 2:**
    **Input:** nums = [4,5,6,7,0,1,2], target = 3 **Output:** -1

    ```C++
        class Solution {
        public:
            int search(vector<int>& nums, int target) {
                int start = 0, end = nums.size()-1;
                while(start <= end){
                    cout<<start<<" "<<end<<endl;
                    int mid = (start + end)/2;
                    if(target == nums[mid]){
                        return mid;
                    }
                    else if(nums[mid] >= nums[start]){ //unrotated part of the array
                        if(target >= nums[start] && target < nums[mid])
                            end = mid-1;
                        else
                            start = mid+1;
                    }
                    else{ //rotated part of the array
                        if(target <= nums[end] && target > nums[mid])
                            start = mid+1;
                        else
                            end = mid-1;
                    }
                }
                cout<<start<<" "<<end<<endl;
                return -1;
            }
        };
    ```

    ### Notes
    * Binary Search Problem
    * Figure out what part of the array the middle element and target belong to and accordingly change start and end of the binary search.
  
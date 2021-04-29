# Using two pointers

### Given an array nums and a value val, remove all instances of that value in-place and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with `O(1)` extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**example**

> **Input**: nums = [3,2,2,3], val = 3
**Output**: 2, nums = [2,2]
**Explanation**: Your function should return length = 2, with the first two elements of nums being 2.
It doesn't matter what you leave beyond the returned length. For example if you return 2 with nums = [2,2,3,3] or nums = [2,2,0,0], your answer will be accepted.

**constraints**
> 0 <= nums.length <= 100
0 <= nums[i] <= 50
0 <= val <= 100

**test case**
> nums = [3,2,4,3]
val = 3

**solution**

```typescript
function removeElement(nums: number[], val: number): number{
    if (nums.length === 0) return 0;

    let p1 = 0;

    for (let p2 = 0; p2 < nums.length; p2++){
        if (val !== nums[p2]){
            nums[p1] = nums[p2]
            p1++
        }
    }
    return p1
}
```

**explanation**

So, the question clearly asks us to directly modify the array and there's no way for us to remove the found numbers. So we have an option of just sending them to the back. We can also just change those numbers to something else like `0`, but nah.

We first create a pointer 1 called `p1` and give it a `0` indicating that it's index 0.
We iterate through the `nums` array and check if the `nums[p2]` number is not equal to the `val`. If it's not equal, we assign the `nums[p2]` value at `nums[p1]` and increment `p1`.
If we console log it, it will look something like this
```
[ 3, 2, 4, 3 ]
[ 2, 2, 4, 3 ]
[ 2, 4, 4, 3 ]
[ 2, 4, 4, 3 ]
```
Notice that it's not pushing the numbers, but rather it just replaces the `p1`'s index value with `p2`'s index value.

Pretty straight forward huh?
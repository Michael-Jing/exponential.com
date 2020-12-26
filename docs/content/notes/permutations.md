+++
+++
The answer is from https://leetcode.com/problems/permutations/discuss/18241/One-Liners-in-Python, I put it here for my own reference

# Solution 1: Recursive, take any number as first
Take any number as the first number and append any permutation of the other numbers.
```python
def permute(self, nums):
    return [[n] + p for i, n in enumerate(nums) for p in self.permute(nums[:i] + nums[i + 1: ])] or [[]]
```
when nums is empty, the first term will be [] and [] or [[]] will be [[]] which is the correct answer

# Solution 2: Recursive, insert first number anywhere
insert the first number anywhere in any permutation of the remaining numbers.
```python
def permute(self, nums: 'List[int]') -> 'List[List[int]]':
    return nums and [p[:i] + [nums[0]] + p[i:] for p in self.permute(nums[1:]) for i in range(len(nums))] or [[]]
```
# Solution 3: Reduce, insert next number anywhere
Use reduce to insert the next number anywhere in the already built permutations.
```python
def permute(self, nums):
    return reduce(lambda P, n: [p[:i] + [n] + p[i:] for p in P for i in range(len(p) + 1)], nums, [[]])
```
# Solution 4: lexicographical order, this approach can also deal with the situation that there are duplicates in the input numbers
the idea is to start with the ascending ordered numbers and change it into the next leicographical order
there are 4 steps to change a sequence to it's next lexicographical order

1. iterate through the sequence from the back to the begining, find the first index k such that nums[k] is smaller than nums[k+1]
2. iterate through the elements from nums[k+1] to the last element of the sequence, find the smallest element that is bigger than nums[k]
3. swap the element found in the last step with nums[k]
4. reverse the part of sequence start from num[k+1]

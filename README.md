# create-target-array-in-the-given-order

## 題目解讀：

### 題目來源:
[create-target-array-in-the-given-order](https://leetcode.com/problems/create-target-array-in-the-given-order/)

### 原文:
Given two arrays of integers nums and index. Your task is to create target array under the following rules:

Initially target array is empty.
From left to right read nums[i] and index[i], insert at index index[i] the value nums[i] in target array.
Repeat the previous step until there are no elements to read in nums and index.
Return the target array.

It is guaranteed that the insertion operations will be valid.

### 解讀：

給定一個正整數陣列nums 每個元素nums[i]代表輸入的數值
給定一個正整數陣列index 每個元素index[i]代表nums[i]放置在新的陣列的位置

求出新的正整數陣列target
舉例來說：
Input: nums = [0,1,2,3,4], index = [0,1,2,2,1]

nums   index             target
0       0                [0]
1       1                [0, 1]
2       2                [0, 1, 2]
3       2                [0, 1, 3, 2]
4       1                [0, 4, 1, 3, 2]   
## 初步解法:
### 初步觀察:
觀察 每個後來插入的位置如果有值

原本位置上的值 以及後面的值 都回被往後挪一位 

所以只要找到要出入的位置

先copy 該位置之前的所有值 到新的陣列

然後 放入輸入的直到新的陣列

最後 放入該位置及以後的值到新的陣列
### 初步設計:
Given a integer array nums, an integer array index

step1: create an integer array with length of nums

step2: let putIdx =0

step3: if putIdx > length of nums go to step 7

step4: if putIdx!==0 loop moveIdx = index[putIdx] to  putIdx, set target[moveIdx+1] = target[moveIdx]

step5: set target[index[putIdx]] = nums[putIdx] go to step 3

step6: return target
## 遇到的困難
### 題目上理解的問題
因為英文不是筆者母語

所以在題意解讀上 容易被英文用詞解讀給搞模糊

### pseudo code撰寫

一開始不習慣把pseudo code寫下來

因此 不太容易把自己的code做解析

### golang table driven test不熟
對於table driven test還不太熟析

所以對於寫test還是耗費不少時間
## 我的github source code
```golang
package target_array

func createTargetArray(nums []int, index []int) []int {
	target := make([]int, len(nums))
	for idx := range nums {
		movedIndx := index[idx]
		if idx != movedIndx {
			for mdx := idx; mdx > movedIndx; mdx-- {
				target[mdx] = target[mdx-1]
			}
		}
		target[movedIndx] = nums[idx]
	}
	return target
}
```
## 測資的撰寫
```golang
package target_array

import (
	"reflect"
	"testing"
)

func Test_createTargetArray(t *testing.T) {
	type args struct {
		nums  []int
		index []int
	}
	tests := []struct {
		name string
		args args
		want []int
	}{
		{
			name: "Example1",
			args: args{
				nums:  []int{0, 1, 2, 3, 4},
				index: []int{0, 1, 2, 2, 1},
			},
			want: []int{0, 4, 1, 3, 2},
		},
		{
			name: "Example2",
			args: args{
				nums:  []int{1, 2, 3, 4, 0},
				index: []int{0, 1, 2, 3, 0},
			},
			want: []int{0, 1, 2, 3, 4},
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			if got := createTargetArray(tt.args.nums, tt.args.index); !reflect.DeepEqual(got, tt.want) {
				t.Errorf("createTargetArray() = %v, want %v", got, tt.want)
			}
		})
	}
}

```
## my self code
[golang ironman 30  13th day](https://hackmd.io/wwb3PDIfTvam9zoAMwMLJw?view)

## 參考文章

[golang test](https://ithelp.ithome.com.tw/articles/10204692)
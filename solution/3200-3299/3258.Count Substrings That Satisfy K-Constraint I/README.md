---
comments: true
difficulty: 简单
edit_url: https://github.com/doocs/leetcode/edit/main/solution/3200-3299/3258.Count%20Substrings%20That%20Satisfy%20K-Constraint%20I/README.md
rating: 1258
source: 第 411 场周赛 Q1
tags:
    - 字符串
    - 滑动窗口
---

<!-- problem:start -->

# [3258. 统计满足 K 约束的子字符串数量 I](https://leetcode.cn/problems/count-substrings-that-satisfy-k-constraint-i)

[English Version](/solution/3200-3299/3258.Count%20Substrings%20That%20Satisfy%20K-Constraint%20I/README_EN.md)

## 题目描述

<!-- description:start -->

<p>给你一个 <strong>二进制</strong> 字符串 <code>s</code> 和一个整数 <code>k</code>。</p>

<p>如果一个 <strong>二进制字符串</strong> 满足以下任一条件，则认为该字符串满足 <strong>k 约束</strong>：</p>

<ul>
	<li>字符串中 <code>0</code> 的数量最多为 <code>k</code>。</li>
	<li>字符串中 <code>1</code> 的数量最多为 <code>k</code>。</li>
</ul>

<p>返回一个整数，表示 <code>s</code> 的所有满足 <strong>k 约束 </strong>的<span data-keyword="substring-nonempty">子字符串</span>的数量。</p>

<p>&nbsp;</p>

<p><strong class="example">示例 1：</strong></p>

<div class="example-block">
<p><strong>输入：</strong><span class="example-io">s = "10101", k = 1</span></p>

<p><strong>输出：</strong><span class="example-io">12</span></p>

<p><strong>解释：</strong></p>

<p><code>s</code> 的所有子字符串中，除了 <code>"1010"</code>、<code>"10101"</code> 和 <code>"0101"</code> 外，其余子字符串都满足 k 约束。</p>
</div>

<p><strong class="example">示例 2：</strong></p>

<div class="example-block">
<p><strong>输入：</strong><span class="example-io">s = "1010101", k = 2</span></p>

<p><strong>输出：</strong><span class="example-io">25</span></p>

<p><strong>解释：</strong></p>

<p><code>s</code> 的所有子字符串中，除了长度大于 5 的子字符串外，其余子字符串都满足 k 约束。</p>
</div>

<p><strong class="example">示例 3：</strong></p>

<div class="example-block">
<p><strong>输入：</strong><span class="example-io">s = "11111", k = 1</span></p>

<p><strong>输出：</strong><span class="example-io">15</span></p>

<p><strong>解释：</strong></p>

<p><code>s</code> 的所有子字符串都满足 k 约束。</p>
</div>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 50</code></li>
	<li><code>1 &lt;= k &lt;= s.length</code></li>
	<li><code>s[i]</code> 是 <code>'0'</code> 或 <code>'1'</code>。</li>
</ul>

<!-- description:end -->

## 解法

<!-- solution:start -->

### 方法一：滑动窗口

我们用两个变量 $\textit{cnt0}$ 和 $\textit{cnt1}$ 分别记录当前窗口内的 $0$ 和 $1$ 的个数，用 $\textit{ans}$ 记录满足 $k$ 约束的子字符串的个数，用 $l$ 记录窗口的左边界。

当我们右移窗口时，如果窗口内的 $0$ 和 $1$ 的个数都大于 $k$，我们就需要左移窗口，直到窗口内的 $0$ 和 $1$ 的个数都不大于 $k$。此时，窗口内所有以 $r$ 作为右端点的子字符串都满足 $k$ 约束，个数为 $r - l + 1$，其中 $r$ 是窗口的右边界。我们将这个个数累加到 $\textit{ans}$ 中。

最后，我们返回 $\textit{ans}$ 即可。

时间复杂度 $O(n)$，其中 $n$ 是字符串 $s$ 的长度。空间复杂度 $O(1)$。

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countKConstraintSubstrings(self, s: str, k: int) -> int:
        cnt = [0, 0]
        ans = l = 0
        for r, x in enumerate(map(int, s)):
            cnt[x] += 1
            while cnt[0] > k and cnt[1] > k:
                cnt[int(s[l])] -= 1
                l += 1
            ans += r - l + 1
        return ans
```

#### Java

```java
class Solution {
    public int countKConstraintSubstrings(String s, int k) {
        int[] cnt = new int[2];
        int ans = 0, l = 0;
        for (int r = 0; r < s.length(); ++r) {
            ++cnt[s.charAt(r) - '0'];
            while (cnt[0] > k && cnt[1] > k) {
                cnt[s.charAt(l++) - '0']--;
            }
            ans += r - l + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countKConstraintSubstrings(string s, int k) {
        int cnt[2]{};
        int ans = 0, l = 0;
        for (int r = 0; r < s.length(); ++r) {
            cnt[s[r] - '0']++;
            while (cnt[0] > k && cnt[1] > k) {
                cnt[s[l++] - '0']--;
            }
            ans += r - l + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func countKConstraintSubstrings(s string, k int) (ans int) {
	cnt := [2]int{}
	l := 0
	for r, c := range s {
		cnt[c-'0']++
		for ; cnt[0] > k && cnt[1] > k; l++ {
			cnt[s[l]-'0']--
		}
		ans += r - l + 1
	}
	return
}
```

#### TypeScript

```ts
function countKConstraintSubstrings(s: string, k: number): number {
    const cnt: [number, number] = [0, 0];
    let [ans, l] = [0, 0];
    for (let r = 0; r < s.length; ++r) {
        cnt[+s[r]]++;
        while (cnt[0] > k && cnt[1] > k) {
            cnt[+s[l++]]--;
        }
        ans += r - l + 1;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_k_constraint_substrings(s: String, k: i32) -> i32 {
        let mut cnt = [0; 2];
        let mut l = 0;
        let mut ans = 0;
        let s = s.as_bytes();

        for (r, &c) in s.iter().enumerate() {
            cnt[(c - b'0') as usize] += 1;
            while cnt[0] > k && cnt[1] > k {
                cnt[(s[l] - b'0') as usize] -= 1;
                l += 1;
            }
            ans += r - l + 1;
        }

        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->

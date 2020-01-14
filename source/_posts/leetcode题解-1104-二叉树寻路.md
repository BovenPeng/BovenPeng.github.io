---
title: leetcode题解-1104-二叉树寻路
mathjax: true
date: 2019-12-27 21:40:33
categories:
tags:
- 位运算
---



## [题目](https://leetcode-cn.com/problems/path-in-zigzag-labelled-binary-tree/)

### [MY way](https://leetcode-cn.com/problems/path-in-zigzag-labelled-binary-tree/solution/pythonwei-yun-suan-by-mai-mai-mai-mai-zi/)

我的思路是，自顶向下遍历到`label`所在那一层的结点，赋值出来，然后先序遍历输出结果。感觉就不是最优解。

### [麦麦麦麦子way](https://leetcode-cn.com/problems/path-in-zigzag-labelled-binary-tree/solution/pythonwei-yun-suan-by-mai-mai-mai-mai-zi/)

```python
    def pathInZigZagTree(self, label: int) -> List[int]:
        res = []
        while label != 1:
            res.append(label)
            label >>= 1
            # 这里我采用异或实现
            label = label ^(1 << (label.bit_length() - 1)) - 1
        return [1]+res[::-1]
```

因为以1为根节点层次编号的满二叉树可以对应到位的表示，所以用位运算的思路即可。

因为每层的顺序在变，所以每次需要对首位外的其它位取反。

举例14=1110b，

先将14右移，变为111b，然后对除第一位外所有位取反变为100b，即它的根节点4，

同理100b，右移变为10b，对除第一位外所有位取反变为11b，即它的根节点3

一直到1结束。



## 总结

- 这是一道具有数学规律的题，不是很容易地可以发现根节点和两个孩子结点的规律。

  - `根节点label = (孩子结点label / 2)除最高的1位之外后的所有位按位取反`

- 怎么在C++里面统计一个数以二进制时，最后一位到最高的"1"位的位数。

  - ```C++
    // 循环统计位数
    unsigned bits, var = (x < 0) ? -x : x;
    for (bits = 0; var != 0; ++bits) var >>= 1;
    ```

  - ```C++
    // 2^n > label >= 2^(n-1) - 1
    // n是label结点的层数
    int numberOfBits = floor(log2(label)) + 1;
    ```

    



## Reference

https://stackoverflow.com/questions/25754082/how-to-take-twos-complement-of-a-byte-in-c

https://stackoverflow.com/questions/29388711/c-how-to-get-length-of-bits-of-a-variable

[std::floor向下取整](https://en.cppreference.com/w/cpp/numeric/math/floor)


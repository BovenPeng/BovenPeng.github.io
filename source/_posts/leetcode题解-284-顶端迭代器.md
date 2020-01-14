---
title: leetcode题解-284-顶端迭代器
mathjax: true
date: 2019-12-17 16:09:42
categories:
tags:
- ×
---

## [题目](https://leetcode-cn.com/problems/peeking-iterator/)

### [大力王way](https://leetcode-cn.com/problems/peeking-iterator/solution/c-huan-cun-tou-bu-yuan-su-by-da-li-wang/)

```C++
// Below is the interface for Iterator, which is already defined for you.
// **DO NOT** modify the interface for Iterator.

class Iterator {
	struct Data;
	Data* data;
public:
	Iterator(const vector<int>& nums);
	Iterator(const Iterator& iter);
	virtual ~Iterator();
	// Returns the next element in the iteration.
	int next();
	// Returns true if the iteration has more elements.
	bool hasNext() const;
};


class PeekingIterator : public Iterator {
public:
    int curr;
    bool hit_end;
	PeekingIterator(const vector<int>& nums) : Iterator(nums) {
	    // Initialize any member here.
	    // **DO NOT** save a copy of nums and manipulate it directly.
	    // You should only use the Iterator interface methods.
	    hit_end = false;
        if (Iterator::hasNext()) {
            curr = Iterator::next();
        } else {
            hit_end = true;
        }
	}

    // Returns the next element in the iteration without advancing the iterator.
	int peek() {
        return curr;
	}

	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	int next() {
        if (hit_end) {
            return -1;
        }
        int res = curr;
	    if (Iterator::hasNext()) {
            curr = Iterator::next();
        } else {
            hit_end = true;
        }
        return res;
	}

	bool hasNext() const {
	    return !hit_end;
	}
};
```

简单修改了一下，把`cur`, `hit_end`放到`private`作用域里了。

```C++
// Below is the interface for Iterator, which is already defined for you.
// **DO NOT** modify the interface for Iterator.

class Iterator {
    struct Data;
	Data* data;
public:
	Iterator(const vector<int>& nums);
	Iterator(const Iterator& iter);
	virtual ~Iterator();
	// Returns the next element in the iteration.
	int next();
	// Returns true if the iteration has more elements.
	bool hasNext() const;
};


class PeekingIterator : public Iterator {
private:
    int cur;
    bool hit_end;

public:
	PeekingIterator(const vector<int>& nums) : Iterator(nums) {
	    // Initialize any member here.
	    // **DO NOT** save a copy of nums and manipulate it directly.
	    // You should only use the Iterator interface methods.
        hit_end = false;

        if (Iterator::hasNext())
        {
            cur = Iterator::next();
        }
        else
        {
            hit_end = true;
        }
	}

    // Returns the next element in the iteration without advancing the iterator.
	int peek() 
    {
        return cur;    
	}

	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	int next() 
    {
        int res = -1;
        if (hit_end)    return -1;
        res = cur;
        if (Iterator::hasNext())
        {
            cur = Iterator::next();
        }
        else
        {
            hit_end = true;
        }

        return res;
    }

	bool hasNext() const 
    {
        return !hit_end;
	}
};
```



## 总结

- 不能拷贝一个基类对象进行直接操作，会涉及到子类调用父类接口的一些写法：
  - 这种没有加`virtual`的`override`是通过`class::function_name`来调用的
- 用`cur`表示最前面元素，`hit_end`表示是否到了尾部
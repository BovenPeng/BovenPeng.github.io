---
title: leetcode题解-297-二叉树的序列化与反序列化
date: 2019-12-09 16:16:06
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- 树的遍历
- 序列化
- 字符串
- stringstream
- ×
---



## [题目](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

### My way

#### 手动队列序列化 + 手动出队反序列化

因为C++ STL没有`split`, `boost`里面有。

所以考虑自己实现一个`split`函数。

```C++
vector<string> splitStrToStrArray(string s, string delimiter)
{
	string token;
	vector<string> vs;
    
    while(token != s){
      token = s.substr(0,s.find_first_of(delimiter));
      s = s.substr(s.find_first_of(delimiter) + 1);
      //printf("%s ",token.c_str());
      vs.push_back(token);
    }

	return vs;
}
```

根据题意要求写了个如下的出来，包括测试用例：

```C++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>
#include <deque>

using namespace std;

struct TreeNode {
      int val;
      TreeNode *left;
      TreeNode *right;
      TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 };

class Codec {
private:
vector<string> splitStrToStrArray(const string& s, const string& delimiter)
{
    // remove "[" and "]"
    string str = s.substr(1, s.size() - 2);
	string token;
	vector<string> vs;
    
    while(token != str){
      token = str.substr(0,str.find_first_of(delimiter));
      str = str.substr(str.find_first_of(delimiter) + 1);
      //printf("%s ",token.c_str());
      vs.push_back(token);
    }
	return vs;
}

TreeNode* genNodebyStr(const string& s)
{
    if (s == "null")    return nullptr;
    
    TreeNode* node = new TreeNode(stoi(s));
    return node;
}

public:
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) 
    {
        if (!root)  return "[null,]";

        string res = to_string(root->val) + ",";
        deque<TreeNode*> q;
        q.push_back(root);

        while (!q.empty())
        {
            auto top = q.front();    q.pop_front();
            if (top->left)     
            {
                q.push_back(top->left);     
                res += to_string(top->left->val) + ",";
            }
            else
            {
                res += "null,";
            }

            if (top->right)     
            {
                q.push_back(top->right);    
                res += to_string(top->right->val) + ",";
            }
            else
            {
                res += "null,";
            }
        }
        res = "[" + res + "]";
        
//        cout << res << endl;
        return res;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(const string& data) 
    {
        vector<string> vs = splitStrToStrArray(data, ",");
        int index = 0;
        TreeNode* root = genNodebyStr(vs[index++]);
        deque<TreeNode*> q;

        if (!root)  q.push_back(root);
        TreeNode* node = nullptr;

        while(!q.empty())
        {
            node = q.front(); q.pop_front();
            node->left = genNodebyStr(vs[index++]);
            node->right = genNodebyStr(vs[index++]);

            if (node->left)     q.push_back(node->left);
            if (node->right)    q.push_back(node->right);
        }

        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));


int main(void)
{
    TreeNode* root = new TreeNode(1);
    TreeNode* n1 = new TreeNode(2);
    TreeNode* n2 = new TreeNode(3);
    TreeNode* n4 = new TreeNode(4);
    TreeNode* n5 = new TreeNode(5);
    
    root->left = n1;    root->right = n2;
    n2->left = n4;      n2->right = n5;
    
    
    
    
    
    Codec c;
    c.deserialize(c.serialize(root));
    
    cout << stoi("12") << endl;
    return 0;
}

```

但是发现这个过不了测试用例，然后输出后发现为

`[1,2,3,null,null,4,5,null,null,null,null,]`

而不是要求的:

`[1,2,3,null,null,4,5]`

所以应该是说，最下面一层叶子要序列化它的`null`孩子。



### [Ting Yu way](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/solution/c-version-by-zzyuting/)

```C++
class Codec {
public:
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) 
    {
        ostringstream out;
        serialize(root,out);
        return out.str();
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) 
    {
        istringstream in(data);
        return deserialize(in);
    }
private:
    void serialize(TreeNode* root,ostringstream& out)
    {
        if(root)
        {
            out<<root->val<<' ';
            serialize(root->left,out);
            serialize(root->right,out);
        }else
        {
            // 这里一定要带着空格，因为在输入的时候用来表示当前输入结束
            out<<"# ";
        }
        
    }

    TreeNode* deserialize(istringstream& in)
    {
        string val;
        in>>val;
        if(val=="#")
        {
            return nullptr;
        }
        TreeNode* root=new TreeNode(stoi(val));
        root->left=deserialize(in);
        root->right=deserialize(in);
        return root;
    }
};

```



## [官方](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/solution/er-cha-shu-de-xu-lie-hua-yu-fan-xu-lie-hua-by-leet/)

{% asset_img serial_deserial.png serial_to_deserial %}

`Binary Search Tree` 的序列化本质上是对其值进行编码，更重要的是对其结构进行编码。

可以遍历树来完成上述任务。众所周知，我们有两个一般策略：

- 广度优先搜索（BFS）
  我们按照高度的顺序从上到下逐级扫描树。更高级别的节点将先于较低级别的节点访问。
- 深度优先搜索（DFS)
  - 在这个策略中，我们采用深度作为优先顺序，这样我们就可以从一个根开始，一直延伸到某个叶，然后回到根，到达另一个分支。
  - 根据根节点、左节点和右节点之间的相对顺序，可以进一步将DFS策略区分为 preorder、inorder 和 postorder 。



然而，在这个任务中，DFS 策略更适合我们的需要，因为相邻节点之间的链接自然地按顺序编码，这对后面的反序列化任务非常有帮助。

因此，在这个解决方案中，我们用 preorder DFS 策略演示了一个示例。您可以在 leetcode explore上查看有关二叉搜索树的更多教程。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
// Serialization
public class Codec {
  public String rserialize(TreeNode root, String str) {
    // Recursive serialization.
    if (root == null) {
      str += "null,";
    } else {
      str += str.valueOf(root.val) + ",";
      str = rserialize(root.left, str);
      str = rserialize(root.right, str);
    }
    return str;
  }

  // Encodes a tree to a single string.
  public String serialize(TreeNode root) {
    return rserialize(root, "");
  }


public TreeNode rdeserialize(List<String> l) {
    // Recursive deserialization.
    if (l.get(0).equals("null")) {
      l.remove(0);
      return null;
    }

    TreeNode root = new TreeNode(Integer.valueOf(l.get(0)));
    l.remove(0);
    root.left = rdeserialize(l);
    root.right = rdeserialize(l);

    return root;
  }

  // Decodes your encoded data to tree.
  public TreeNode deserialize(String data) {
    String[] data_array = data.split(",");
    List<String> data_list = new LinkedList<String>(Arrays.asList(data_array));
    return rdeserialize(data_list);
  }
};

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```



## 总结

- 利用`sstream`里的`istringstream`和`ostringstream`来解决字符串相关问题，是我之前没想到的
- 



## Reference

https://stackoverflow.com/questions/14265581/parse-split-a-string-in-c-using-string-delimiter-standard-c

https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/solution/c-version-by-zzyuting/
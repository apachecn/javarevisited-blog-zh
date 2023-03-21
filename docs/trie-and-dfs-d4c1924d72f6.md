# 使用 Trie 和 DFS 映射和对 LeetCode 解

> 原文：<https://medium.com/javarevisited/trie-and-dfs-d4c1924d72f6?source=collection_archive---------2----------------------->

一个很好的问题可以通过使用 Trie、DFS 和哈希表来解决。

## 677。映射求和对

您需要设计一个允许您执行以下操作的地图:

*   将字符串键映射到给定值。
*   返回前缀等于给定字符串的关键字为的值的总和。

实现`MapSum`类:

*   `MapSum()`初始化`MapSum`对象。
*   `void insert(String key, int val)`将`key-val`对插入[地图](https://www.java67.com/2013/02/10-examples-of-hashmap-in-java-programming-tutorial.html)。如果`key`已经存在，原来的`key-value`对将被新的覆盖。
*   `int sum(string prefix)`返回所有`key`以`prefix`开头的对的值之和。

**例 1:**

```
**Input**
["MapSum", "insert", "sum", "insert", "sum"]
[[], ["apple", 3], ["ap"], ["app", 2], ["ap"]]
**Output**
[null, null, 3, null, 5]**Explanation**
MapSum mapSum = new MapSum();
mapSum.insert("apple", 3);  
mapSum.sum("ap");           // return 3 (apple = 3)
mapSum.insert("app", 2);    
mapSum.sum("ap");           // return 5 (apple + app = 3 + 2 = 5)
```

**约束:**

*   `1 <= key.length, prefix.length <= 50`
*   `key`和`prefix`仅由小写英文字母组成。
*   `1 <= val <= 1000`
*   最多`50`个电话会打给`insert`和`sum`。

## 参考码

```
struct TrieNode{
    public:
    bool is_word = false;
    vector<TrieNode*> children;
    TrieNode(){
        children.assign(26, nullptr);
    }
};
class MapSum {
public:
    unordered_map<string, int> m;
    TrieNode* root;
    /** Initialize your data structure here. */
    MapSum() {
        root = new TrieNode();
    }

    void insert(string key, int val) {
        if (m.find(key) != m.end()) {
            m[key] = val;
            return;
        }
        m[key] = val;
        // insert this word to TrieNode
        auto node = root;
        const int n = key.size();
            for (int i = 0; i < n; ++i){
                int ch_idx = key[i] - 'a';
                if (node -> children[ch_idx] == nullptr){
                    node -> children[ch_idx] = new TrieNode();
                }
                node = node -> children[ch_idx];
                if (i == n - 1) node -> is_word = true;
            }
        return;
    }

    int dfs(TrieNode* node, string prefix){
        int ret = 0;
        if (node -> is_word) ret += m[prefix];
        for (char ch = 'a'; ch <= 'z'; ch++){
            if (node -> children[ch - 'a'] != nullptr){
                ret += dfs(node -> children[ch - 'a'], prefix + ch);
            }
        }
        return ret; 
    }

    int sum(string prefix) {
        auto node = root;
        for (int i = 0; i < prefix.size(); ++i){
            int ch_idx = prefix[i] - 'a';
            if (node -> children[ch_idx] == nullptr) return 0;
            node = node -> children[ch_idx];
        }
        return dfs(node, prefix);
    }
};
/**
 * Your MapSum object will be instantiated and called as such:
 * MapSum* obj = new MapSum();
 * obj->insert(key,val);
 * int param_2 = obj->sum(prefix);
 */
```

# 问题

472。串联词

# 解决办法

```
struct TrieNode{
    public:
    bool is_word = false;
    vector<TrieNode*> children;
    TrieNode(){
        children.assign(26, nullptr);
    }
};
class Trie{
    public:
    TrieNode* root;
    Trie(){
        root = new TrieNode();
    }
    void insert(string &word){
        if (word.empty())return;
        auto curr = root;
        for (auto c: word){
            int idx = c - 'a';
            if (curr->children[idx] == nullptr){
                curr->children[idx] = new TrieNode();
            }
            curr = curr->children[idx];
        }
        curr -> is_word = true;
    }
};

class Solution {
public:

    unordered_map<string, int> memo;
    int dfs(int i, string& word, TrieNode* node){
        string sub_str = word.substr(i);
        if (memo.find(sub_str) !=memo.end())return memo[sub_str];
        if (i == word.size()){
            return memo[sub_str] = 0;
        }
        auto curr = node;
        int max_words = -1;
        for (int j = i; j < (int) word.size(); ++j){
            curr = curr->children[word[j] - 'a'];
            if (curr == nullptr)break;
            if (curr -> is_word){
                int this_ret = dfs(j + 1, word, node);
                if (this_ret == -1) continue;
                else {
                    max_words = max(max_words, 1 + this_ret); 
                }
            }
        }
        return memo[sub_str] = max_words;
    }
    vector<string> findAllConcatenatedWordsInADict(vector<string>& words) {
        Trie trie;
        for (auto &word: words){
            trie.insert(word);
        }
        vector<string> ret;
        for (auto &word: words){
            int num_of_concat_words = dfs(0, word, trie.root);
            if (num_of_concat_words >= 2){
                ret.push_back(word);
            }
        }
        return ret;
    }
};
```

## 其他数据结构问题和资源

 [## 50 大数据结构和算法程序员面试问题

### 有很多计算机科学毕业生和程序员申请编程、编码和软件…

medium.com](/javarevisited/50-data-structure-and-algorithms-interview-questions-for-programmers-b4b1ac61f5b0) [](/javarevisited/top-10-free-data-structure-and-algorithms-courses-for-beginners-best-of-lot-ad807cc55f7a) [## 面向初学者的 10 大免费数据结构和算法课程——最好的

### 算法和数据结构是计算机科学的两个最基本和最重要的课题，是计算机科学的基础

medium.com](/javarevisited/top-10-free-data-structure-and-algorithms-courses-for-beginners-best-of-lot-ad807cc55f7a)
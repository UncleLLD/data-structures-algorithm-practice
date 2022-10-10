### 208. 实现 Trie (前缀树)
来源：力扣（LeetCode）

链接: [https://leetcode.cn/problems/implement-trie-prefix-tree/](https://leetcode.cn/problems/implement-trie-prefix-tree)
Trie（发音类似 "try"）或者说 前缀树 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补完和拼写检查。

请你实现 Trie 类：
* `Trie()` 初始化前缀树对象。
* `void insert(String word)` 向前缀树中插入字符串 word 。
* `boolean search(String word)` 如果字符串 word 在前缀树中，返回 true（即，在检索之前已经插入）；否则，返回 false 。
* ` boolean startsWith(String prefix) `如果之前已经插入的字符串 word 的前缀之一为 prefix ，返回 true ；否则，返回 false 。


**示例：**
```
输入
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
输出
[null, null, true, false, true, null, true]
```

**解释**
```
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // 返回 True
trie.search("app");     // 返回 False
trie.startsWith("app"); // 返回 True
trie.insert("app");
trie.search("app");     // 返回 True
```

**提示：**
* 1 <= word.length, prefix.length <= 2000
* word 和 prefix 仅由小写英文字母组成
* insert、search 和 startsWith 调用次数 总计 不超过 $3 * 10^4$ 次



### 解法
* 字典树： 每个字符都作为字典树的key，不断的更新字典树，如果是单词的话，就将末尾添加一个符号进行标注，python中使用的字典嵌套，c++使用的是子树结构。
	从字典树的根开始，插入字符串。对于当前字符对应的子节点，有两种情况：
	* 子节点存在。沿着指针移动到子节点，继续处理下一个字符。
	* 子节点不存在。创建一个新的子节点，记录在 $\textit{children}$ 数组的对应位置上，然后沿着指针移动到子节点，继续搜索下一个字符。
### 代码实现
#### 字典树
**python实现**
```python
class Trie:
    def __init__(self):
        self.tree = dict()

    def insert(self, word: str) -> None:
        tree = self.tree
        for a in word:
            if a not in tree:
                tree[a] = dict()
            tree = tree[a]
        tree['#'] = dict()  # 结束符

    def search(self, word: str) -> bool:
        tree = self.tree
        for a in word:
            if a not in tree:
                return False
            tree = tree[a]
        if '#' in tree:
            return True
        return False

    def startsWith(self, prefix: str) -> bool:
        tree = self.tree
        for a in prefix:
            if a not in tree:
                return False
            tree = tree[a]
        return True


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```


**c++实现**
```cpp
class TrieNode {
public:
    vector<TrieNode*> children;
    bool isWord;
    TrieNode() : isWord(false), children(26, nullptr) {
    }
    ~TrieNode() {
        for (auto& c : children)
            delete c;
    }
};

class Trie {

private:
    TrieNode* root;

public:
    /** Initialize your data structure here. */
    Trie() {
        root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        TrieNode* p = root;
        for (char a : word) {
            int i = a - 'a';
            if (!p->children[i])
                p->children[i] = new TrieNode();
            p = p->children[i];
        }
        p->isWord = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        TrieNode* p = root;
        for (char a : word) {
            int i = a - 'a';
            if (!p->children[i])
                return false;
            p = p->children[i];
        }
        return p->isWord;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        TrieNode* p = root;
        for (char a : prefix) {
            int i = a - 'a';
            if (!p->children[i])
                return false;
            p = p->children[i];
        }
        return true;
    }
};
```


**复杂度分析**
* 时间复杂度： $O(字符串长度)$ 
* 空间复杂度： $O(字符串长度之和)$

### 参考
* [https://leetcode.cn/problems/implement-trie-prefix-tree/solution/shi-xian-trie-qian-zhui-shu-by-leetcode-ti500/](https://leetcode.cn/problems/implement-trie-prefix-tree/solution/shi-xian-trie-qian-zhui-shu-by-leetcode-ti500/)
* [https://leetcode.cn/problems/implement-trie-prefix-tree/solution/fu-xue-ming-zhu-cong-er-cha-shu-shuo-qi-628gs/](https://leetcode.cn/problems/implement-trie-prefix-tree/solution/fu-xue-ming-zhu-cong-er-cha-shu-shuo-qi-628gs/)
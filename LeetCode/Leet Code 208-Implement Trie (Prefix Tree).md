
实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。
```
示例:

Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
```
说明:

你可以假设所有的输入都是由小写字母 a-z 构成的。
保证所有输入均为非空字符串。  


字典树的特性：  
1.根节点不包含字符，除根节点外的每一个子节点都包含一个字符。  
2.从根节点到某一节点的路径上的字符连接起来，就是该节点对应的字符串。  
3.每个节点的所有子节点包含的字符都不相同。  


```python
class Trie:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = {}
        self.isWord = '#'

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        node = self.root
        for s in word:
            node = node.setdefault(s, {})
        node[self.isWord] = self.isWord

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        node = self.root
        for s in word:
            if s not in node:
                return False
            node = node[s]
        return self.isWord in node

    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        node = self.root
        for pre in prefix:
            if pre not in node:
                return False
            node = node[pre]
        return True
        
```


```python
obj = Trie()
word = 'apple'
```


```python
obj.insert(word)
```


```python
param_2 = obj.search(word)
param_3 = obj.startsWith('app')
```


```python
param_2
```




    True




```python
param_3
```




    True



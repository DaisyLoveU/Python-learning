
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

  
  
思路1：直接使用字典来模拟

如添加apple和apply，bad
```
{a:
  {p:
    {p:
      {l:
        {e:{isWord:#}, 
        y:{isWord:#}
        }
      }
    }
  }, 
b:
  {a:
    {d:{isWord:#}
    }
  }

}
```


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

思路二：创建节点类


```python
class TrieNode(object):
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.childs = dict()
        self.isWord = False
        
        

class Trie(object):

    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        """
        Inserts a word into the trie.
        :type word: str
        :rtype: void
        """
        node = self.root
        for letter in word:
            child = node.childs.get(letter)
            if child is None:
                child = TrieNode()
                node.childs[letter] = child
            node = child
        node.isWord = True

    def search(self, word):
        """
        Returns if the word is in the trie.
        :type word: str
        :rtype: bool
        """
        node = self.root
        for i in word:
            child = node.childs.get(i)
            if child is None:
                return False
            node = child
        return node.isWord
        

    def startsWith(self, prefix):
        """
        Returns if there is any word in the trie
        that starts with the given prefix.
        :type prefix: str
        :rtype: bool
        """
        node = self.root
        for letter in prefix:
            child = node.childs.get(letter)
            if child is None:
                return False
            node = child
        return True
        
```


设计一个支持以下两种操作的数据结构：
```
void addWord(word)
bool search(word)
search(word) 可以搜索文字或正则表达式字符串，字符串只包含字母 . 或 a-z 。 . 可以表示任何一个字母。

示例:

addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```

思路一：使用Trie，遇到```.```则需要遍历当前节点的所有子节点


```python
class WordDictionary:
#  使用字典实现的Trie，最终的节点是{'#':'#'}, 注意判断条件
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = {}
        self.isWord = '#'

    def addWord(self, word: str) -> None:
        """
        Adds a word into the data structure.
        """
        node = self.root
        for s in word:
            node = node.setdefault(s, {})
        node[self.isWord] = self.isWord
        

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter.
        """
        def dfs(node, word):
            # if type(node) != dict:
            #     return False
            
            if node == '#':
                return False
            
            if len(word) == 0: 
                return self.isWord in node
            if word[0] == '.':
                for it in node:
                    if dfs(node[it], word[1:]):
                        return True
                return False
            else:  
                
                tmp = node.get(word[0])
                if tmp is None: return False
                return dfs(tmp, word[1:])
        return dfs(self.root, word)
```

另一种实现方式


```python
class TrieNode():
#     使用节点实现Trie
    def __init__(self):
        self.childs = dict()
        self.isWord = False
        
class WordDictionary():
    def __init__(self):
        """
        initialize your data structure here.
        """
        self.root = TrieNode()
        

    def addWord(self, word):
        """
        Adds a word into the data structure.
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
        Returns if the word is in the data structure. A word could
        contain the dot character '.' to represent any one letter.
        :type word: str
        :rtype: bool
        """
        def dfs(root, word):
            if len(word) == 0:
                return root.isWord
            elif word[0] == '.':
                for node in root.childs:
                    if dfs(root.childs[node], word[1:]):
                        return True
                return False
            else:
                node = root.childs.get(word[0])
                if node is None:
                    return False
                return dfs(node, word[1:])

        return dfs(self.root, word)
```

思路二：根据len(word)来保存单词，然后继续根据长度来检索单词  
  
  
很巧妙的使用了for...else...语句


```python
class WordDictionary:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        """
        wordDict维护单词长度与单词list的映射关系
        """
        self.wordDict = collections.defaultdict(list)
        

    def addWord(self, word: str) -> None:
        """
        Adds a word into the data structure.
        """
        if word:
            self.wordDict[len(word)].append(word)
        

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter.
        """
        if not word:
            return False
        if '.' not in word:
            return word in self.wordDict[len(word)]
        for v in self.wordDict[len(word)]:
            for i, c in enumerate(word):
                if c != v[i] and c != '.':
                    break;
            else:
                return True
        return False
```


```python
class WordDictionary:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.dyct = {}

    def addWord(self, word: str) -> None:
        """
        Adds a word into the data structure.
        """
        self.dyct.setdefault(len(word), []).append(word)

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter.
        """
        if len(word) not in self.dyct:
            return False
        if '.' not in word:
            return word in self.dyct[len(word)]
        for i in self.dyct[len(word)]:
            if re.match(word, i):
                return True
        return False
        
```

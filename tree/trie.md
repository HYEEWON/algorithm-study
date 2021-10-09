# 트라이 (Trie)

## 1) 정의

* 문자열 검색을 위한 `이진 검색 트리`
* 노드에 `한 문자`씩 저장

## 2) 삽입
* 루트는 비워져 있음
* 추가하려는 문자열의 첫글자가 루트의 자식이라면 탐색, 없다면 첫글자를 자식으로 추가
* 동일한 문자가 있다면 다음 글자로 넘어감, 동일한 문자가 없으면 새로운 문자를 자식으로 추가
* 반복

## 3) 탐색
* 루트의 자식부터 탐색 시작
* 자식 아래로 탐색 계속
* 문자열의 길이 == 리프 레벨 -> 검색 성공

## 4) 구현

![trie](https://user-images.githubusercontent.com/38900338/130806414-e7586e7a-4111-4131-ba19-44cf77ec0ab9.JPG)

```python
class Node(object):
    def __init__(self, key, end = None):
        self.key = key
        self.end = end
        self.child = {}

class Trie:
    def __init__(self):
        self.head = Node(self)
    
    def insert(self, word):
        cur = self.head

        for w in word:
            if w not in cur.child:
                cur.child[w] = Node(w)
            cur = cur.child[w]
        cur.end = True

    def search(self, word):
        cur = self.head

        for w in word:
            if w in cur.child:
                cur = cur.child[w]
            else:
                return False

        if cur.end == True:
            return True

trie = Trie()
trie.insert(word)
trie.update(word)
```

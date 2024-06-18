# 141. Linked List Cycle

## step1

思いつかなかったので、全探索で書いてみた
空間計算量がnとかなり大きくなってしまい、多くのメモリを占領してしまう

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        checkedElements = []
        target = head
        if not target:
            return False

        while(target not in checkedElements):
            checkedElements.append(target)
            if not target.next:
                return False
            target = target.next
        return True
```

## step2

問題のあるの解法だったので、解説を参考に別解を覚える

参考にした[解説](https://leetcode.com/problems/linked-list-cycle/solutions/4829782/python3-3-solutions-and-are-with-math-proofs-hashset-modify-visited-nodes/)

Tortoise and Hare algorithmを使用した回答

時間計算量: O(n)

空間計算量: O(1)

初めこのアルゴリズムを見た時は、どうしてスピードの異なるポインタを使うのかわからなかった。

他の解き方に比べて空間計算量が節約できて、破壊的変更がないのが利点。詳細は下記

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        fast = slow = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        return False
```

### 講師目線セルフツッコミ

**どうしてこの回答を選んだか？**
理由1 空間計算量が少なくなるから
全探索以外のポインタ一つので下記のようなものがあるらしい

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        seen = set()
        current = head

        while current:
            if current in seen:
                return True
            seen.add(current)
            current = current.next

        return False
```

これだと時間計算量O(n)、空間計算量O(n)で自分が書いた回答と比べても旨みがない

理由2 破壊的変更がないから
確認した要素の値を変更していく解法があるらしい。

これは時間計算量O(N), 空間計算量O(1)と性能面は問題ない

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        current = head

        while current:
            if current.val is None:
                return True
            current.val = None
            current = current.next

        return False
```

ただ、データの状態が変わると読みずらいし、不具合の原因にもなりかねないので不採用
(curに参照渡ししているから)

よって、Tortoise and hare algorythmの解法を覚えた

**なぜSetにListNodeが追加できるのか**

SetにListNodeのインスタンスを入れられるのか？という旨の疑問を持っている方がいらっしゃった。

普段から何気なく書いていたので、詳しくみてみた。

[要素はハッシュ可能である必要があるらしい](https://docs.python.org/ja/3/library/stdtypes.html#set)

ハッシュ可能とは、不変のハッシュ値をもつということ

[そして自前で定義したクラスはハッシュ可能だそう](https://docs.python.org/ja/3/library/stdtypes.html#set)

よって、Setに追加することはできる。

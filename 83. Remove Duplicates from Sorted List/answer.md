# 83. Remove Duplicates from Sorted List

## step1

setに入れたらすぐ解けることに気がつけた

ただpythonのsetにするときの実装どうなっているのかや、他の方法ないのかなど気になった

time complexityもspace complexityもO(n)なのでもっといい方法ある気がした

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> List[Optional[ListNode]]:
        answer = set()
        target = head
        while target:
            answer.add(target)
            target = target.next
        return list(answer)
```

勢いで書いてしまったが、重複排除したリストを返すことを期待されているのだと理解した。
leetcodeはもともと定義されているメソッドの返り値は変えてはいけないのか
問題文だけ読むとソートされた重複ないリストを返却したらOKっぽく見えるが、それだけで判断してはだめ

問題の理解自体間違っていたので5m追加で考えてみる。

→メソッド名からして重複削除の実装期待しているのはわかるが、返り値が` Optional[ListNode] `だと良くわからないな

このメソッド自体が何かのプログラムで呼び出されて結果的にOutputに重複排除したListが出てくるのか？

問題自体よくわからなかったが、とりあえず参照渡することからnextの値を書き換えるというものを書いてみた（多分違うのだろうけど。。。）

```python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        target = head

        while target:
            if target != target.next:
                target = target.next
                continue
            target.next = target.next.next
        return target
```

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

問題理解したので１から解き直してみた

```python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        target = head

        while target and target.next:
            if target.val == target.next.val:
                target.next = target.next.next
            target = target.next
        return head
```

` [1,1,1] `のように最後のノードの値が重複の時、重複値が残ってしまう実装をしてしまった。

はじめと終わりとかエッジケース考えないといかん

## step2

問題を理解できた。ようは、重複削除した上でLinkedList形式で返せってことか。だから模範解答だと加工した上でhead返しているのか

### leetcodeの解答例

[参考](https://leetcode.com/problems/remove-duplicates-from-sorted-list/solutions/943403/python-simple-solution)

ぱっと見の印象は、コードが処理の羅列になってる。mapなどを使用して文章のように読めるコードにできるのではないか？と思った。

あと、while統合できないか？

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        cur=head
        while cur:
            while cur.next and cur.next.val==cur.val:
                cur.next=cur.next.next
            cur=cur.next
        return head
        
```

### セルフツッコミ

再帰で書けないか？

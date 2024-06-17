# 142. Linked List Cycle II

## step1

Pythonに慣れておらず` return null `と書いてしまった。Pythonではnullは` None `と書く

解法は直前にやった問題のsetを用いるものをすぐ思いつけた。

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        seen = set()
        cur = head
        while cur:
            if cur in seen:
                return cur
            seen.add(cur)
            cur = cur.next
        return null // 正しくはNone
```

## step2

None使ってかけた

Time　complexity: O(n)

Space complexity: O(n)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        seen = set()
        cur = head
        while cur:
            if cur in seen:
                return cur
            seen.add(cur)
            cur cur.next
        return None
```

### セルフツッコミ

**Tortoise and hare algorithmで解けないか？**

自分で解いてみたら手が止まってしまったので、[解説みた](https://leetcode.com/problems/linked-list-cycle-ii/solutions/3274329/clean-codes-full-explanation-floyd-s-cycle-finding-algorithm-c-java-python3/)

ループが検出されたら、slowを先頭に戻し、両方のポインタを1ずつ動かしていくと出会った場所がlinkedListの始まりの場所となる

→この理由がよくわからない。両方１ずつ動かしていたら次に合流することないのでは？合流したとしてもそれってたまたまでは？

解説を紐解いてみる。

L: サイクルの長さ

T: リストの先頭からサイクルに入るまでのノードの数

C: サイクル内で slow と fast が初めて出会うまでのノードの数（サイクル内の距離）

とする

slowが進んだ距離は、T+Cと表せる

fastは2倍の速度で動いていたから、2(T+C)と表せる。またfastはslowと出会うまでサイクルの中をk周したとすると、kL+T+Cと表せる

すると、kl = T+Cと言える。というところまでは理解できたが、それが何を指しているのかがわからない。

↓

[解説いただき何を意味しているかが理解できた。](https://discord.com/channels/1084280443945353267/1246383603122966570/1252209488815984710)

衝突地点からslow, fastに来た道を１ステップずつ戻ってもらう

同じ速度で戻るからサイクルのスタート地点までは同伴している。（Cの距離分一緒に戻った）

その後Tの距離分slowが戻る。するとfastは衝突地点に戻っている。なぜならT+C=kLで亀の移動分動かすと、サイクルを何周かしたのと同じになるからちょうどスタートポジションである衝突地点に戻る

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        collision = self.findCollision(head)
        if not collision:
            return None

        fromCollision = collision
        fromHead = head

        while fromHead != fromCollision:
            fromHead = fromHead.next
            fromCollision = fromCollision.next
        return fromHead
        

    def findCollision(self, head) -> Optional[ListNode]:
        fast = slow = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if(fast == slow):
                return fast
        return None

```

## step3

[currentという変数名について](https://discord.com/channels/1084280443945353267/1196472827457589338/1235587411879268445)

省略形は変数名につけると可読性的に微妙かと思っていた。しかもcurrentはグローバルな設定で使われることが多いのか

curも避けたほうがいいとなると、targetや単にnode, checkedNodeとかか？

[関数切り出しの仕方が勉強になった](https://github.com/goto-untrapped/Arai60/pull/22#pullrequestreview-2088058080)

→setの解法は切り出すところがないのでtortoise and hareで試した

確認対象になっているものを入れる変数だと考え、targetにした。

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        seen = set()
        target = head
        while target:
            if target in seen:
                return target
            seen.add(target)
            target = target.next
        return None
```

findと書いたが、素直にgetCollisionで良いと思ったので、メソッド名変更

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getCollision(self, head: Optional[ListNode]) -> Optional[ListNode]:
        fast = slow = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                return fast
        return None

    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        collision = self.getCollision(head)

        if not collision:
            return None

        fromCollision = collision
        fromHead = head
        while fromCollision != fromHead:
            fromCollision = fromCollision.next
            fromHead = fromHead.next
        return fromHead
```

## step4

### Setの解法

step3のコードと同じもの描いたのでコードは省略

1回目 1m

2回目　2m(英語でボソボソ説明しながら)

3回目 2m

### tortoise and hare

1回目 5m
2回目 5m
3回目 5m
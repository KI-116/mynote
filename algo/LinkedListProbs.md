# 总论
双指针在链表中的应用相当广泛。

## 1.合并两个有序链表[Lc21](https://leetcode.cn/problems/merge-two-sorted-lists/)

***初始化：***
使用虚拟头节点，也就是`dummy`节点，简化代码。

如果不使用dummy节点，需要对头节点进行特殊处理以，避免出现空指针。

> 需要使用dummy的情况：创造一条新链表。

```cpp
class Solution {
    public:
        ListNode* mergeTwoLists(ListNode* l1, ListNode* l2){
            ListNode dummy(0);
            ListNode* p = &dummy;
            ListNode* p1 = l1;
            ListNode* p2 = l2;

            while(p1 && p2){
                // 比较两个链表的值
                // 将较小的值加入到新链表中
                if(p1->val < p2->val){
                    p->next = p1;
                    p1 = p1->next;
                } else {
                    p->next = p2;
                    p2 = p2->next;
                }
                p = p->next;
            }
            if(p1){
                p->next = p1;
            }
            if(p2){
                p->next = p2;
            }
            return dummy.next;

        }
}
```

## 2.单链表分解为两个链表[Lc86](https://leetcode.cn/problems/partition-list/)

本题需要你把链表一分为二，最后再把这两条链表合并起来。

```cpp
class Solution {
    public:
        ListNode* partition(ListNode* head, int x){
            //小于x的节点
            ListNode dummy1(0);
            //大于等于x的节点
            ListNode dummy2(0);
            ListNode* p1 = &dummy1;
            ListNode* p2 = &dummy2;
            ListNode* p = head;
            while(p){
                if(p->val >= x){
                    p2->next = p;
                    p2 = p2->next;
                } else {
                    p1->next = p;
                    p1 = p1->next;
                }
                //p = p->next;
                //如果需要把原链表的节点接到新链表上，需要断开新节点和原链表之间的连接。否则会形成环。
                ListNode* tmp = p->next;
                p->next = nullptr;
                p = tmp;
            }
            p1->next = dummy2.next;
            return dummy1.next;
        }
}
```
## 3.合并k个有序链表[Lc23](https://leetcode.cn/problems/merge-k-sorted-lists/)

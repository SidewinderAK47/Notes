## 0061 Rotate List 右旋链表

Medium

Given a linked list, rotate the list to the right by *k* places, where *k* is non-negative.

**Example 1:**

```
Input: 1->2->3->4->5->NULL, k = 2
Output: 4->5->1->2->3->NULL
Explanation:
rotate 1 steps to the right: 5->1->2->3->4->NULL
rotate 2 steps to the right: 4->5->1->2->3->NULL
```

**Example 2:**

```out
Input: 0->1->2->NULL, k = 4
Output: 2->0->1->NULL
Explanation:
rotate 1 steps to the right: 2->0->1->NULL
rotate 2 steps to the right: 1->2->0->NULL
rotate 3 steps to the right: 0->1->2->NULL
rotate 4 steps to the right: 2->0->1->NULL
```



## 代码实现：

单链表右旋  k = 单链表左旋 length - k 这个思路做的。

对k首先进行取余操作，如果长度为 length则去空整个链表是不存在的。  

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(head==NULL||k==0)
            return head;
        int len=0;
        ListNode * p=head,*tail=NULL;
        while(p != NULL){
            len++;
            tail=p;
            p = p->next;
        }
        int rotateR=k%len;
        p=head;
        if(rotateR){
            int rotateL=len-rotateR;
            for(int i=0; i<rotateL; i++){
                ListNode * t=p->next;
                tail -> next =p;
                p->next=NULL;
                p=t;
                tail=tail->next;
            }
        }
        return p;
    }
};
```


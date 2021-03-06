### [707. 设计链表](https://leetcode-cn.com/problems/design-linked-list/)

```cpp
class MyLinkedList {

public:
    struct ListNode {
        int val;
        ListNode* next;
        ListNode(int x) {
            val = x;
            next = nullptr;
        }
    };
    /** Initialize your data structure here. */
    MyLinkedList() {
        size = 0;
        dynamic = new ListNode(0);
    }

    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    int get(int index) {
        if (index >= size || index < 0) {
            return -1;
        }
        ListNode* root = dynamic;
        for (int i = 0; i <= index; i++) {	//当root = dynamic,当i==index时实际就是索引为index的节点
            root = root->next;
        }
        return root->val;
    }

    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    void addAtHead(int val) {
        ListNode* cur = new ListNode(val);
        cur->next = dynamic->next;
        dynamic->next = cur;
        size++;
    }

    /** Append a node of value val to the last element of the linked list. */
    void addAtTail(int val) {
        ListNode* cur = new ListNode(val);
        ListNode* root = dynamic;
        while (root->next) {
            root = root->next;
        }
        root->next = cur;
        size++;
    }

    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    void addAtIndex(int index, int val) {
        if (index > size || index < 0) {	//当index等于size时就将节点插到尾
            return;
        }
        ListNode* cur = new ListNode(val);
        ListNode* root = dynamic;
        for (int i = 0; i < index; i++) {
            root = root->next;
        }
        cur->next = root->next;
        root->next = cur;
        size++;
    }

    /** Delete the index-th node in the linked list, if the index is valid. */
    void deleteAtIndex(int index) {
        if (index >= size || index < 0)return;
        ListNode* root = dynamic;
        for (int i = 0; i < index; i++) {
            root = root->next;
        }
        ListNode* temp = root->next;
        root->next = temp->next;
        delete temp;
        size--;
    }
    void print() {//打印链表
        ListNode* cur = dynamic->next;
        while (cur) {
            cout << cur->val << endl;
            cur = cur->next;
        }
        cout << endl;
    }
private:
    int size;
    ListNode* dynamic;
};
```

### [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

``` cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if(!head) return head;
        head->next=removeElements(head->next,val);
        if(head->val==val){
            return head->next;
        }else{
            return head;
        }
    }
};
```



### [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

```cpp
//并查集
class Solution {
public:
    unordered_map<int,int> fa;
    int Find(int x){
        return fa.find(x) == fa.end()? x  : fa[x] = Find(fa[x]);
    }
    int longestConsecutive(vector<int>& nums) {
        fa.clear();
        for(auto p:nums) fa[p] = p + 1;
        int k,ans = 0;
        for(auto p:nums){
            int k = Find(p);
            ans = max(ans,k - p);
        }
        return ans;
    }
};
//哈希表
```



### [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

```cpp
class Solution {
public:
    //只要要改变头节点就定义虚拟头节点，该题关键是要找到要删除节点的前一个节点，所以要保证快慢指针的节点相差n个节点就可以
    //所以让快指针先走n步就行
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dynamic=new ListNode(-1);
        dynamic->next=head;
        ListNode* slow=dynamic;
        ListNode* fast=dynamic;
        while(n--){
            fast=fast->next;
        }
        while(fast->next){
            slow=slow->next;
            fast=fast->next;
        }
        slow->next=slow->next->next;
        return dynamic->next;
    }
};
```



### [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)



```cpp
class Solution {
public:
    //该题和19题相似
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode *dynamic=new ListNode(-1);
        dynamic->next=head;
        ListNode *slow=dynamic;
        ListNode*fast=dynamic;
        while(k--){
            fast=fast->next;
        }
        while(fast->next){
            fast=fast->next;
            slow=slow->next;
        }
        return slow->next;
    }
};
```



### [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

```cpp
//前序遍历
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        int i;
        ListNode *temp=head;
        for(i=1;i<k&&head;i++){
            head=head->next;
        }
        if(!head)return temp;
        ListNode*nextHead=head->next;
        head->next=nullptr;
        ListNode *newHead=reverseNode(temp,nullptr);
        temp->next=reverseKGroup(nextHead,k);
        return newHead;
    }
    ListNode *reverseNode(ListNode *cur,ListNode *pre){
        if(cur==nullptr)return pre;
        ListNode *temp=cur->next;
        cur->next=pre;
        return reverseNode(temp,cur);
    }
};
//后序遍历
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode *temp=head;
        for(int i=1;i<k&&head;i++){
            head=head->next;
        }
        if(!head)return temp;
        ListNode *nextHead=reverseKGroup(head->next,k);
        head->next=nullptr;
        ListNode *newHead=reverseNode(temp,nullptr);
        temp->next=nextHead;
        return newHead;
    }
    ListNode *reverseNode(ListNode *cur,ListNode *pre){
        if(cur==nullptr)return pre;
        ListNode *temp=cur->next;
        cur->next=pre;
        return reverseNode(temp,cur);
    }
};
```



### [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

```cpp
//本质和25差不多
//利用递归进行后序遍历
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
       if(head==nullptr||head->next==nullptr)return head;
       ListNode *newHead=head->next;
       ListNode *nextHead=swapPairs(newHead->next);
       newHead->next=head;
       head->next=nextHead;
        return newHead;
    }
};
//前序遍历
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
       if(head==nullptr||head->next==nullptr)return head;
        ListNode *next=head->next;
        ListNode *nextNode=next->next;
        next->next=head;
        head->next=swapPairs(nextNode);
       return next;
    }
};
```



### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

```cpp
//递归代码简洁
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(!l1)return l2;
        if(!l2)return l1;
        if(l1->val<l2->val){
            l1->next=mergeTwoLists(l1->next,l2);
            return l1;
        }else{
            l2->next=mergeTwoLists(l1,l2->next);
            return l2;
        }
    }
};
//迭代，模拟归并写法	比较麻烦
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(!l1)return l2;
        if(!l2)return l1;
        ListNode *newHead=nullptr;
        ListNode *cur=nullptr;
        while(l1&&l2){
            if(!newHead){
                if(l1->val>l2->val){
                    newHead=l2;
                    cur=l2;
                    l2=l2->next;
                }else{
                    newHead=l1;
                    cur=l1;
                    l1=l1->next;
                }
            }else{
                if(l1->val>l2->val){
                    cur->next=l2;
                    l2=l2->next;
                    cur=cur->next;
                }else{
                    cur->next=l1;
                    l1=l1->next;
                    cur=cur->next;
                }
            }
        }
        while(l1){
            cur->next=l1;
            l1=l1->next;
            cur=cur->next;
        }
        while(l2){
            cur->next=l2;
            l2=l2->next;
            cur=cur->next;
        }
        return newHead;
    }
};

```



### [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

```cpp
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        ListNode *dynamic=new ListNode(-1);
        dynamic->next=head;
        ListNode *cur=dynamic;
        ListNode *pre=nullptr;
        ListNode *prenext=nullptr;
        ListNode *next=nullptr;
        for(int i=0;i<=n;i++){
            if(i==m-1){
                pre=cur;
                prenext=pre->next;
            }else if(i==n){
                next=cur->next;
                cur->next=nullptr;
                pre->next=reverseList(pre,nullptr);
                prenext->next=next;
                break;
            }
            cur=cur->next;
        }
        return dynamic->next;
    }
    //反转链表
    ListNode *reverseList(ListNode *cur,ListNode *pre){
        if(!cur) return pre;
        ListNode *temp=cur->next;
        cur->next=pre;
        return reverseList(temp,cur);
    }
};
```



### [剑指 Offer 18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

```cpp
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
        if(!head)return head;
        ListNode *dynamic=new ListNode(-1);
        dynamic->next=head;
        ListNode *cur=dynamic;
        while(cur&&cur->next){//注意判断空指针
            if(cur->next->val==val){
                cur->next=cur->next->next;
            }
            cur=cur->next;
        }
        return dynamic->next;
    }
};
```



### [23. 合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

```cpp
//解法1：每次找数组中的最小元素然后手动模拟
//时间复杂度：O(nk)-->k=lists.size()
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if(lists.size()==0)return nullptr;
        int size=lists.size();
        ListNode *dynamic=new ListNode(0);
        ListNode *temp=dynamic;
        while(true){
            int minIndex=-1;
            ListNode *minNode=nullptr;
            for(int i=0;i<size;i++){
                if(lists[i]==nullptr)continue;
                if(minNode==nullptr||minNode->val>lists[i]->val){
                    minNode=lists[i];
                    minIndex=i;
                }
            }
            if(minIndex==-1) break;
            temp->next=minNode;
            temp=temp->next;
            lists[minIndex]=lists[minIndex]->next;
        }
        return dynamic->next;
    }
};

//解法二：使用小根堆优先队列输出 时间复杂度:O(nlongk)
class Solution {
public:
    struct cmp{
        bool operator()(const ListNode* l1,const ListNode* l2){
            return l1->val > l2->val;
        }
    };
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue<ListNode*,vector<ListNode*>,cmp> pri_que;
        for(auto list:lists){
            if(list!=nullptr)
                pri_que.push(list);
        }
        if(pri_que.empty()) return nullptr;
        ListNode *res=pri_que.top();
        pri_que.pop();
        if(res->next) pri_que.push(res->next);
        ListNode*cur=res;
        while(!pri_que.empty()){
            ListNode *temp=pri_que.top();
            pri_que.pop();
            if(temp->next) pri_que.push(temp->next);
            cur->next=temp;
            cur=cur->next;
        }
        return res;
    }
};
```


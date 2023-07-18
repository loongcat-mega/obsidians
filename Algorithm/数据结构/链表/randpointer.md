Node结构多一个随机指针，指向链表任一节点，求拷贝此链表的方法
## pair<Node*,int>hashmap+连接随机指针
```c++
List&copy()  
{  
    pair<Node*,int>hashmap[cur];  
    Node*p=head;  
    for(int i=0;p!= nullptr;i++)  
    {  
        hashmap[i].first=p;  
        p=p->next;  
    }  
    p=head;  
    for(int i=0;p!= nullptr;i++)  
    {  
        for(int k=0;k<cur;k++)  
            if(p->rand==hashmap[k].first)  
            {  
                hashmap[i].second=k;  
                break;            }  
        p=p->next;  
    }  
    List *new_head=new List;  
    for(int i=0;i<cur;i++)  
        new_head->push_back(hashmap[i].first->data);  
    p=new_head->head;  
    for(int i=0;i<cur;i++)  
    {  
        if(hashmap[i].second>=i)//往右走  
        {  
            Node*q=p;  
            for(int j=0;j<hashmap[i].second-i;j++)  
                q=q->next;  
            p->rand=q;  
        }  
        else//往前走  
        {  
            Node *q = p;  
            for (int j = 0; j < i - hashmap[i].second; j++)  
                q = q->pre;  
            p->rand = q;  
        }  
        p=p->next;  
    }  
    return *new_head;  
}

```
## map<Node*,Node*>hashmap边走边连

```c++
List &copy_1()  
{  
    map<Node*,Node*>hashmap;  
    Node*p=head;  
    for(int i=0;p;i++, p=p->next)  
        hashmap.insert(make_pair(p,new Node(p->data)));  
    p=head;  
    List *l=new List();  
    while(p)  
    {  
        if(p->next)  
            hashmap.find(p)->second->next=hashmap.find(p->next)->second;  
        if(p->pre)    
hashmap.find(p)->second->pre=hashmap.find(p->pre)->second;  
        hashmap.find(p)->second->rand=hashmap.find(p->rand)->second;  
        p=p->next;  
    }  
    l->head=hashmap.find(head)->second;  
    return *l;  
}

```
## 在原链表中穿插拷贝结点，rand指针则有相同规律,不用容器，只用有限几个变量


$$11'22'33'44'55'66'$$

```c++
List &copy_3()  
{  
    Node*p=head;  
    Node*front=nullptr;  
    while(p)  
    {  
        front=p;  
        p=p->next;  
        insert(front,front->data);  
    }  
    p=head;  
    Node*new_p=p->next;  
    Node*new_head=head->next;  
    while(new_p->next)  
    {  
        new_p->rand = p->rand->next;  
        p = new_p->next;  
        new_p=p->next;  
    }  
    p=head;  
    new_p=p->next;  
    while(true)  
    {  
        cur--;  
        p->next = new_p->next;  
        if (new_p->next)  
            new_p->next->pre = p;  
  
        p = new_p->next;  
        if(!p)  
        {  
            new_p->next=nullptr;  
            break;        }  
        if(p->next)  
            p->next->pre=new_p;  
        new_p=p->next;  
    }  
    List*l=new List();  
    l->head=new_head;  
    return *l;  
}
```

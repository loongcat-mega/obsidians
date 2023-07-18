空间复杂度为常数级的情况下，不申请额外空间，只用有限几个变量，实现partition

使用六个变量，小于区，等于区，大于区的左右指针
```c++
void partition(int value)  
{  
    Node*smallL=nullptr,*smallR=nullptr;  
    Node*equL=nullptr,*equR=nullptr;  
    Node*bigL=nullptr,*bigR=nullptr;  
    Node*p=head;  
    while(p)  
    {  
        if(p->data<value)  
        {  
            if(smallL==nullptr)  
            {  
                smallL=p;  
                smallR=p;  
            }  
            else  
            {  
                smallR->next=p;  
                smallR=p;  
            }  
        }  
        else if(p->data==value)  
        {  
            if(equL==nullptr)  
            {  
                equL=p;  
                equR=p;  
            }  
            else  
            {  
                equR->next=p;  
                equR=p;  
            }  
        }  
        else if(p->data>value)  
        {            if(bigL==nullptr)  
            {                bigL=p;  
                bigR=p;  
            }  
            else  
            {  
                bigR->next=p;  
                bigR=p;  
            }  
        }  
        p=p->next;  
    }  
    head=smallL==nullptr?(equL==nullptr?bigL:equL):smallL;  
    if(head==bigL)  
    {  
        bigR->next=nullptr;  
        rear=bigR;  
    }  
    else if(head==equL)  
    {  
        equR->next=bigL;  
        if(bigL==nullptr)  
            rear=equR;  
        else        {  
            bigR->next=nullptr;  
            rear=bigR;  
        }  
    }  
    else   
{  
        if (equL == nullptr)   
{  
            if (bigL != nullptr)   
{  
                smallR->next = bigL;  
                bigR->next = nullptr;  
                rear = bigR;  
            } else  
            {  
                smallR->next = nullptr;  
                rear = smallR;  
            }  
  
        }   
else   
{  
            if (bigL == nullptr)   
{  
                smallR->next = equL;  
                equR->next = nullptr;  
                rear = equR;  
            } else   
{  
                smallR->next = equL;  
                equR->next = bigL;  
                bigR->next = nullptr;  
                rear = bigR;  
            }  
  
        }  
    }  
}
```

优化版本
```c++
void partition(int value)  
{  
    Node*smallL=nullptr,*smallR=nullptr;  
    Node*equL=nullptr,*equR=nullptr;  
    Node*bigL=nullptr,*bigR=nullptr;  
    Node*p=head,*after=nullptr;  
    while(p)  
    {  
        after=p->next;  
        p->next=nullptr;  
        if(p->data<value)  
        {  
            if(smallL==nullptr)  
            {  
                smallL=p;  
                smallR=p;  
            }  
            else  
            {  
                smallR->next=p;  
                smallR=p;  
            }  
        }  
        else if(p->data==value)  
        {  
            if(equL==nullptr)  
            {  
                equL=p;  
                equR=p;  
            }  
            else  
            {  
                equR->next=p;  
                equR=p;  
            }  
        }  
        else if(p->data>value)  
        {            if(bigL==nullptr)  
            {                bigL=p;  
                bigR=p;  
            }  
            else  
            {  
                bigR->next=p;  
                bigR=p;  
            }  
        }  
        p=after;  
    }  
    head=smallL==nullptr?(equL==nullptr?bigL:equL):smallL;  
    rear=bigL==nullptr?(equL==nullptr?smallR:equR):bigR;  
    if(smallL!=nullptr)  
    {  
        smallR->next=equL;  
        equR=equR==nullptr?smallR:equR;  
    }  
    if(equR!=nullptr)  
        equR->next=bigL;  
}
```
## Leetcode.480    sliding_window_median

https://leetcode-cn.com/problems/sliding-window-median/

https://blog.csdn.net/weixin_36888577/article/details/79937886

```c++
class DualHeap {
private:
    // 大根堆，维护较小的一半元素
    priority_queue<int> small;
    // 小根堆，维护较大的一半元素
    priority_queue<int, vector<int>, greater<int>> large;
    // 哈希表，记录「延迟删除」的元素，key 为元素，value 为需要删除的次数
    unordered_map<int, int> delayed;

    int k;
    // small 和 large 当前包含的元素个数，需要扣除被「延迟删除」的元素
    int smallSize, largeSize;

public:
    DualHeap(int _k): k(_k), smallSize(0), largeSize(0) {}

private:
    // 不断地弹出 heap 的堆顶元素，并且更新哈希表
    template<typename T>
    void shift(T& heap)
    {
        while(!heap.empty())
        {
            int num=heap.top();
            if(delayed.count(num))
            {
                delayed[num]--;
                if(delayed[num]==0) delayed.erase(num);
                heap.pop();
            }
            else break;
        }
    }

    void balance()
    {
        if(smallSize>largeSize+1)
        {
            large.push(small.top());
            small.pop();
            --smallSize;
            ++largeSize;
            shift(small);
        }
        else if(smallSize<largeSize)
        {
            small.push(large.top());
            large.pop();
            smallSize++;
            largeSize--;
            shift(large);
        }
        
    }
public:
    void insert(int num)
    {
        if(small.empty()||num<=small.top())
        {
            small.push(num);
            smallSize++;
        }
        else{
            large.push(num);
            largeSize++;
        }
        balance();
    }

    void erase(int num)
    {
        delayed[num]++;
        if(num<=small.top())
        {
            --smallSize;
            if(num==small.top()) shift(small);
        }
        else
        {
            --largeSize;
            if(num==large.top()) shift(large);
        }
        balance();
    }

    double median()
    {
        return k&1? small.top():((double)small.top()+large.top())/2.0;
    }
};

```


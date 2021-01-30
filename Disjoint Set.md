## Template

```c++
vector<int> par;
    int cnt;
    int getRoot(int x){
        int root = x;
        while(par[root]!=root){
            root = par[root];                                
        }
        while(par[x]!=root){
            int tmp = par[x];
            par[x] = root;
            x = tmp;
        }
        return root;
    }
    void merge(int x,int y){
        int _x = getRoot(x);
        int _y = getRoot(y);
        if(_x!=_y){
            par[_x]=_y;
            cnt--;
        }
    }
    //初始化
    void init(int n){
        //初始化每个结点视为一个集合
        cnt = n;
        for(int i =1;i<=n;i++){
            par[i] = i;
        }
```


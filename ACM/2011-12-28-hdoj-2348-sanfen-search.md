```c++
       double min = 0, max = Pi/2,mid, midmid;
       while(max-min > ex)
       {
           mid = (min + max) / 2;
           midmid = (mid + max) / 2;
           if(calc(mid)>=calc(midmid))
               max=midmid;
           else
               min=mid;
       }
       if(calc(min) <= y)
           cout<<"yes"<<endl;
       else cout<<"no"<<endl;
```
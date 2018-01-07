第三周的第一阶段：学习了深广搜，以及自己看了一下pick定理。
题不好做，根据每个题的不同写出的代码也大同小异，说明并没有固定的模板。
感触颇深，叹深搜广搜有趣之处。
感觉自己还是做的不够好，仍需努力。




//pick 定理
//S=a+b/2-1
//所有的未知量竟然都有对应的公式，长知识了。
/*struct Point
{
    int x,y;
}p[300];
int gcd(int a,int b)
{
    if(b==0)  return a;
    return gcd(b,a%b);
}

int OnEdge(int n)//计算b
{
    int L=0;
    for(int i=0;i<n;i++)
       L+=gcd(fabs(p[i].x-p[(i+1)%n].x),fabs(p[i].y-p[(i+1)%n].y));
    return L;
}

int Area(int n)//计算S
{
    int S=0;
    for(int i=0;i<n;i++)
        S+=p[i].x*p[(i+1)%n].y-p[i].y*p[(i+1)%n].x;
    return fabs(0.5*S);
}
int InSide(int n,point *p)//计算a   
{  
    return 带入公式即可;   
}  

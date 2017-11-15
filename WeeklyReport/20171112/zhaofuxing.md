因为趣味编程比赛题的准备，所以上周学习较少，但是又学了两种种博弈：威佐夫博弈，尼姆博弈，加上之前学的巴什博弈，一共三种，现在做一下总结。

巴什博弈：一个式子;n%(m+1)是否为0

威佐夫博弈：奇异态，先手必输，非奇异态，先手赢。

列出前几个奇异态的局面：（0 ， 0），（ 1  ， 2  ），（ 3 ， 5 ），（4  ， 7），（ 6 ，10  ）， （ 8 ，13），（ 9 ， 15），（ 11 ，18）

规律：第一个值 = 差值 * 1.618 ，而1.618 = （sqrt（5）+ 1） /  2 。

尼姆博弈：异或定理

#include <cstdio>


#include <cmath>  


#include <iostream>  


using namespace std;  


int main()  


{  


    int n,ans,temp;  
    
    
    while(cin>>n)  
    
    
    {  
    
    
        temp=0;  
        
        
        for(int i=0;i<n;i++)  
        
        
        {  
        
        
            cin>>ans;  
            
            
            temp^=ans;  
            
            
        }  
        
        
        if(temp==0)  cout<<"后手必胜"<<endl;  
        
        
        else cout<<"先手必胜"<<endl;  
        
        
    }  
    
    
    return 0;  
    
    
}  




实验项目名称：
一、程序设计（10分）
1、用图的形式说明程序流程。
用户登录注册（可退出）->进入到用户专属界面->选项卡12（可退出）
1选择难度->进入练习->练习后计入历史记录
2查看历史记录
2、文字说明程序主要数据结构设计。
主要用到std模板库的string，enum枚举类型，文件输入输出流，以及颜色表示（这个主要借鉴示范代码）
3、程序分别有哪些功能，以及这些功能对应的函数头。

补充一下choose_level也主要是练习进行的主要部分，正如表格中所说的联系逻辑在choose_level内部
二、程序运行截图（截取实验设计中写明的功能运行截图，10分）
功能1：进行练习

功能2：查看历史记录

功能3：登录

功能4：注册

三、程序编写、测试过程中遇到的问题及解决方法（10分）
问题1：在计算打字速度时，如果时间很短，可能会出现除以零
解决方法：在计算速度时增加判断，当时间大于0时才进行计算，否则速度记为0

问题2：如果用户输入错误选项，程序没有补救机制
解决方法：增加错误提示信息，例如提示用户“请输入1、2或3”，并让用户重新输入
问题3：打字练习过程中屏幕刷新频繁，影响用户体验
方法：在字符输入循环中使用 SetConsoleCursorPosition() 将光标移动到固定位置输出内容
问题4：程序文字一闪而过，用户反应不过来
加入system（“pause”）；
四、程序代码
例如：
#include<ctime>
#include<cstring>
#include <conio.h>
#include <windows.h>
#include<iostream>
#include<string>
#include<fstream>
#include<map>
#include<vector>
#include<cstdlib>
#include<algorithm>

using namespace std;
enum level{
easy,medium,hard
};
void sign_up();
void user_register();
void user_practise(string ID);
void choose_level(string ID);
void check_history(string ID);
int main(){

    
   
    while(1){
    system("cls");
    cout<<"this is the typing practice,and you have many choices"<<endl;
    cout<<"1:log in"<<endl<<"2:register"<<endl<<"3:exit"<<endl;
    char choice_sign_up_page;
 
    cin>>choice_sign_up_page;
    

    if(choice_sign_up_page=='1'){
        sign_up();
    }
    else if(choice_sign_up_page=='2'){
        user_register();
    }
    else if(choice_sign_up_page=='3'){
        break;
    }
    else{
        cout<<"wrong input!please cin 1,2 or 3!"<<endl;
    }}
    return 0;
}

void sign_up(){
    system("cls");

    string ID;
    string password;
    cout<<"please cin your ID"<<endl;
    cin>>ID;
    ifstream user_search("user.txt");
    if(!user_search){
    ofstream fout("user.txt");  
    fout.close();
    user_search.clear();
    user_search.open("user.txt");
    }
    string s;
    while(user_search>>s){
        if(s==ID){
            cout<<"there is a correct ID! cin your password"<<endl;
            
            user_search>>s;
            for(int i=3;i>0;i--){
                cin>>password;
                if(s==password){
                    cout<<"password is correct!loging in..."<<endl;
                    system("pause");
                    system("cls");
                    user_practise(ID);
                    user_search.close();

                    return;

                }
                else{
                    cout<<"password is wrong,you have "<<i-1<<" chances"<<endl;

                }

            }
            cout<<"password is wrong"<<endl;
            return;

            

            
        }
        user_search>>s;
        
    }
    cout<<"your ID maybe wrong"<<endl;
    user_search.close();
        

    
    
}

void user_register(){
    system("cls");

    string ID;
    string password;
    cout<<"please cin your ID"<<endl;
    cin>>ID;

    ifstream user_search("user.txt");
    if(!user_search){
        ofstream fout("user.txt");
        fout.close();
        user_search.clear();
        user_search.open("user.txt");
    }

    string s;
    while(user_search>>s){
        if(s==ID){
            cout<<"this ID has existed"<<endl;
            user_search.close();
            return;
        }
        user_search>>s;
    }
    user_search.close();

    cout<<"please cin your password"<<endl;
    cin>>password;

    ofstream fout("user.txt",ios::app);
    fout<<ID<<" "<<password<<endl;
    fout.close();
    
    cout<<"register successfully!"<<endl;
    system("pause");
    system("cls");
    user_practise(ID);

}
 
void user_practise(string ID){
    while(1){
        system("cls");
        cout<<"you have entered in"<<endl;
        cout<<"1:choose level"<<endl;
        cout<<"2:check history logs"<<endl;
        cout<<"3:exit"<<endl;
        char choice_sign_up_page;
        cin>>choice_sign_up_page;
    

        if(choice_sign_up_page=='1'){
            choose_level(ID);
        }
        else if(choice_sign_up_page=='2'){
            check_history(ID);
        }
        else if(choice_sign_up_page=='3'){
            break;
        }
        else{
        cout<<"wrong input!please cin 1,2 or 3!"<<endl;
        }
    }
}
void check_history(string ID){
    system("cls");
    string filename=ID+".txt";
    ifstream search_user(filename);
    if(!search_user){
        ofstream fout(filename);  
        fout.close();
        search_user.clear();
        search_user.open(filename);
    }
    string user_level;
    double correct_rate;
    double speed;
    double total_time;
    while(search_user>>user_level){
        search_user>>correct_rate>>speed>>total_time;
        cout<<user_level<<" ";
        cout<<"correct_rate:"<<correct_rate<<" ";
        cout<<"speed:"<<speed<<" alphabet/second  ";
        cout<<"time:"<<total_time<<" second"<<endl;

        
    }
    cout<<"all records have been shown"<<endl;
    system("pause");
    search_user.close();

    system("cls");

}
void choose_level(string ID){
    ifstream call_txt;
    level user_level;
    while(1){
    
    system("cls");
    cout<<"choose your choice"<<endl;
    cout<<"1:easy"<<endl;
    cout<<"2:medium"<<endl;
    cout<<"3:hard"<<endl;
    char choice_sign_up_page;
    cin>>choice_sign_up_page;
    if(choice_sign_up_page=='1'){
        call_txt.open("easy.txt");
        user_level=easy;

        break;
    }
    else if(choice_sign_up_page=='2'){
        call_txt.open("medium.txt");
        user_level=medium;
        break;
    }
    else if(choice_sign_up_page=='3'){
        call_txt.open("hard.txt");
        user_level=hard;
        break;
    }
    else{
        cout<<"wrong input!please cin 1,2 or 3!"<<endl;
    }
    }
    double correct_rate;
    double speed;
    double total_time;
    int correct_num=0;
    int total_num=0;
    system("pause");
    double start=clock();
    string text;
    while(getline(call_txt,text)){
        
        string user_type="";
        int len=text.length();

        for(int i=0;i<len;i++){
            
            
            
            char ch=_getch();
            system("cls");
            user_type+=ch;
            
             cout<<text<<endl;
             cout<<user_type<<endl;
            
            if(text[i]==ch){
                SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_GREEN | FOREGROUND_INTENSITY);
                
               
                total_num++;
                correct_num++;
            }
            else{
                SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE),FOREGROUND_RED | FOREGROUND_INTENSITY);
               
                
                total_num++;
            }
            
      
        SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), FOREGROUND_RED | FOREGROUND_GREEN | FOREGROUND_BLUE);

        double now=clock();
        cout<<"correct rate:"<<((double)correct_num/(double)total_num)*100<<endl;
        total_time=(now-start)/CLOCKS_PER_SEC;
        cout<<"time passed:"<<total_time<<"second"<<endl;
        cout<<"speed:"<<(total_time>0?correct_num/total_time:0)<<" alphabet/second"<<endl;
        cout<<endl;
        }
    }
    
    double end=clock();
    correct_rate=((double)correct_num/(double)total_num)*100;
    total_time=(end-start)/CLOCKS_PER_SEC;
    speed=correct_num/total_time;
    call_txt.close();
    string filename=ID+".txt";
    ofstream record_history(filename,ios::app);
    switch(user_level){
        case easy:record_history<<"easy"<<"  ";break;
        case medium:record_history<<"medium"<<"  ";break;
        case hard:record_history<<"hard"<<"  ";break;
    }
    record_history<<correct_rate<<" ";
    record_history<<speed<<" ";
    record_history<<total_time<<endl;
    record_history.close(); 
    system("pause");
    system("cls");
}


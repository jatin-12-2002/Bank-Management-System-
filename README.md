# Bank-Management-System-
//MADE BY --> JATIN SAREEN
//BANK MANAGEMENT SYSTEM
//ROLL NO.--> 16
//CLASS--> XII B
#include<iostream>
#include<iomanip>
#include<conio.h>
#include<fstream>
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
using namespace std;
//HEADER FILES

//CLASS FOR DETAILS OF CUSTOMER

class customer
{
        int acc_no;              //ACCOUNT NO.
        char name[20];           //NAME
        char addre[25];          //ADDRESS
        int phon;                //PHONE NO.
        int bal;                 //BALANCE
        char ac_type[20];        //ACCOUNT TYPE
        public:
        customer()
        {
            acc_no=1000;
            strcpy(name,"no name");
            strcpy(addre,"no address");
            phon=0;
            bal=0;
            strcpy(addre,"no type");
        }
    public:
        void input();      //FUNCTION TO INPUT DETAILS
        void display();    //FUNCTION TO DISPLAY DETAILS
        void store();      //FUNCTION TO STORE DETAILS
        void view();       //FUNCTION TO VIEW DETAILS
        void searchacc(int b);    //FUNCTION TO SEARCH DETAILS OF ACCOUNT HAVING BALANCE GREATER THAN ENTERED
        void deleteacc(int acc);     //FUNCTION TO DELETE DETAILS
        void generation(int b);      //FUNCTION TO SHOW DETAILS OF A PARTICULAR ACCOUNT
        void updateacc(int acc);     //FUNCTION TO UPDATE DETAILS
        void withdrawl(int acc);     //FUNCTION TO WITHDRAW MONEY
        void deposit(int acc);       //FUNCTION TO DEPOSIT MONEY
        void transfer(int f1,int f2);    //FUNCTION TO TRANSFER MONEY
        void interest(char *b);              //FUNCTION TO CALCULATE INTEREST
        int checkac(int b);              //FUNCTION TO CHECKK EXISTENCE OF ACCOUNT TYPE
        void allocate();               //FUNCTION TO ALLOCATE ACCOUNT NUMBER
        int setcode();                 //FUNCTION TO SET ACCOUNT NUMBER
};

//FUNCTION TO INPUT DETAILS

void customer::input()
{
    cout<<"Enter account holder's name\n";
    cin.ignore();
    gets(name);
    cout<<"Enter address\n";
    gets(addre);
    cout<<"Enter phone no.\n";
    cin>>phon;
    cout<<"Enter balance\n";
    cin>>bal;
    cout<<"Enter account type(rd/fd/savings)\n";
    cin.ignore();
    gets(ac_type);
    getch();
}

//FUNCTION TO DISPLAY DETAILS

void customer::display()
{
    //cout<<"Account holder's name"<<"|"<<"Account number"<<"|"<<"Balance"<<"|"<<"Type"<<endl;
    cout<<name<<setw(15)<<"|"<<setw(7)<<acc_no<<setw(7)<<"|"<<setw(7)<<bal<< setw(5)<<"|"<<setw(1)<<ac_type<<endl;
    getch();
}

//FUNCTION TO STORE DETAILS

void customer:: store()
{
    customer c;
    ofstream f;
    if(phon==0&&bal==0)
    {
        cout<<"File not initialized\n";
    }
    else
    {
    f.open("bank.txt",ios::app|ios::binary);
    f.write((char*)this,sizeof(*this));
    f.close();
    }
}

//FUNCTION TO VIEW DETAILS

void customer:: view()
{
    ifstream f;
    f.open("bank.txt",ios::in|ios::binary);
    if(!f)
        cout<<"File not found\n";
    else
    {
    f.read((char*)this,sizeof(*this));
    while(!f.eof())
    {
        display();
        f.read((char*)this,sizeof(*this));
    }
    f.close();
    }
}

//FUNCTION TO UPDATE DETAILS

void customer:: updateacc(int acc)
{
    fstream f;
    f.open("bank.txt",ios::in|ios::out|ios::ate|ios::binary);
    f.seekg(0);
    int count=0;
    if(!f)
        cout<<"File not found";
    else
    {
    while(!f.eof())
    {
        f.read((char*)this,sizeof(*this));
        if(acc==acc_no)
        {
        input();
        count ++;
        break;
        }
    }
    }
    if(count==0)
        cout<<"Account doesnot exist\n";
    else
    {
    cout<<"Updated successfully\n";
    f.seekg(f.tellp()-sizeof(*this));
    f.write((char*)this,sizeof(*this));
    }
    f.close();
}

//FUNCTION TO DELETE DETAILS

void customer:: deleteacc(int acc)
{
    ofstream of;
    ifstream f;
    f.open("bank.txt",ios::in|ios::binary);
    of.open("temp.txt",ios::out|ios::binary);
    if(!f)
        cout<<"File not found";
    else
    {
            f.read((char*)this,sizeof(*this));
        while(!f.eof())
        {
            if(acc!=acc_no)
            of.write((char*)this,sizeof(*this));
            f.read((char*)this,sizeof(*this));
        }
        f.close();
        of.close();
    remove("bank.txt");
    rename("temp.txt","bank.txt");
    cout<<"Deleted successfully\n";
    }
}

//FUNCTION TO SEARCH DETAILS OF ACCOUNT HAVING BALANCE GREATER THAN ENTERED

void customer::searchacc(int b)
{
    int counter=0;
    ifstream f;
    f.open("bank.txt",ios::in|ios::binary);
    if(!f)
        cout<<"File not found\n";
    else{
        f.read((char*)this,sizeof(*this));
        while(!f.eof())
        {
            if(bal>=b)
            {
                display();
            counter++;
            }
            f.read((char*)this,sizeof(*this));
        }
        if(counter==0)
            cout<<"No records\n";
    }
}

//FUNCTION TO DEPOSIT MONEY

void customer::deposit(int acc)
{
    int amount;
    cout<<"Enter amount you want to deposit\n";
    cin>>amount;
    fstream f;
    f.open("bank.txt",ios::in|ios::out|ios::ate|ios::binary);
    f.seekg(0);
    if(!f)
        cout<<"File doesnot exist\n";
    else{
    while(!f.eof())
    {
        f.read((char*)this,sizeof(*this));
            if(acc==acc_no)
            {
                bal=bal+amount;
                break;
            }
    }
    f.seekp(f.tellp()-sizeof(*this));
                f.write((char*)this,sizeof(*this));
    cout<<"Your balance = "<<bal<<"\n";
    }
}

//FUNCTION TO WITHDRAW MONEY

void customer::withdrawl(int acc)
{
    int amount;
    cout<<"Enter amount you want to withdraw\n";
    cin>>amount;
    fstream f;
    f.open("bank.txt",ios::in|ios::out|ios::ate|ios::binary);
    f.seekg(0);
    int temp=0;
    int flag=0;
    if(!f)
        cout<<"File doesnot exist\n";
    else
    {
    while(!f.eof())
    {
        f.read((char*)this,sizeof(*this));
            if(acc==acc_no)
            {
                temp=bal;
                if(temp<amount)
                    flag=1;
                else
                {
                temp=temp-amount;
                break;
                }
            }
    }
    bal=temp;
    if(flag=1)
        cout<<"Amount withdrawl not possible\n";
    f.seekp(f.tellp()-sizeof(*this));
                f.write((char*)this,sizeof(*this));
    cout<<"Your balance = "<<temp<<"\n";
    }
}

//FUNCTION TO TRANSFER MONEY

void customer:: transfer(int f1,int f2)
{
    int amount;
    cout<<"Enter amount to be transferred\n";
    cin>>amount;
    fstream f;
    f.open("bank.txt",ios::in|ios::out|ios::ate|ios::binary);
    f.seekg(0);
    int temp;
    if(!f)
        cout<<"File doesnot exist\n";
    else
    {
        while(!f.eof())
        {
        f.read((char*)this,sizeof(*this));
            if(f1==acc_no)
            {
                temp=bal;
                if(bal<amount)
                    cout<<"Amount withdrawl not possible\n";
                else
                {
                temp=temp-amount;
                break;
                }
            }
        }
    }
    bal=temp;
    f.seekp(f.tellp()-sizeof(*this));
                f.write((char*)this,sizeof(*this));
    f.close();
    fstream of;
    of.open("bank.txt",ios::in|ios::out|ios::ate|ios::binary);
    of.seekg(0);
    int tem;
    if(!of)
        cout<<"File doesnot exist\n";
    else{
    while(!of.eof())
    {
        of.read((char*)this,sizeof(*this));
            if(f2==acc_no)
            {
                tem=bal;
                tem=tem+amount;
                break;
            }
    }
    bal=tem;
    of.seekp(of.tellp()-sizeof(*this));
                of.write((char*)this,sizeof(*this));
    cout<<endl;
    cout<<f1<<">>>>>"<<"Rs."<<amount<<">>>>>"<<f2;
    cout<<endl;
    of.close();
    }
}

//FUNCTION TO ALLOCATE ACCOUNT NUMBER

void customer::allocate()
{
    customer c;
    ifstream f;
    int cd=1000;
    f.open("bank.txt",ios::in|ios::binary);
    while(f.read((char*)this,sizeof(*this)))
    {
        cd=c.setcode();
    }
    acc_no=cd;
}

//FUNCTION TO SET ACCOUNT NUMBER

int customer::setcode()
{
    customer c;
    ++acc_no;
    return acc_no;
}

//FUNCTION TO CHECK AVAILABILITY OF ACCOUNT

int customer::checkac(int b)
{
    ifstream f;
    int flag;
    f.open("bank.txt",ios::in|ios::binary);
    f.read((char*)this,sizeof(*this));
    while(!f.eof())
    {
        if(b==acc_no)
        {
          flag=1;
          break;
        }
        else
        {
            flag=0;
        }
        f.read((char*)this,sizeof(*this));
    }
    return flag;
}

//FUNCTION TO SEARCH DETAILS

void customer::generation(int b)
{
    ifstream f;
    f.open("bank.txt",ios::in|ios::binary);
    if(!f)
        cout<<"File not found\n";
    else
    {
        f.read((char*)this,sizeof(*this));
        while(!f.eof())
        {
            if(b==acc_no)
            {
                display();
            }
            f.read((char*)this,sizeof(*this));
        }
    }
}

void welcome()
{
    cout<<"\n";
    cout<<"\n";
    cout<<"\n";
    cout<<"\n";
    cout<<"###############################################################################################\n";
    cout<<"##                             PROJECT ON BANK MANAGEMENT SYSTEM                             ##\n";
    cout<<"##                                 MADE BY --> JATIN SAREEN                                  ##\n";
    cout<<"##                                   CLASS --> XII B                                         ##\n";
    cout<<"###############################################################################################\n";
    cout<<"\n";
    cout<<"\n";
    cout<<"\n";
    cout<<"\n";
    cout<<"PRESS ANY KEY TO CONTINUE.........";
    getch();
    system("cls");
    cout<<"\n";
    cout<<"\n";
    cout<<"\n";
    cout<<"\n";
    cout<<"                THIS PROGRAM IS BASED ON BANK MANAGEMENT SYSTEM.BANK MANAGEMENT SYSTEM IS BASED\n";
    cout<<"        ON THE CONCEPT OF RECORDING CUSTOMER'S ACCOUNT DETAILS LIKE CREATING AN ACCOUNT, DEPOSIT\n";
    cout<<"        AMOUNT, WITHDRAW AMOUNT, CHECK BALANCE, VIEW ALL, ACCOUNT HOLDERS DETAIL, CLOSE AN ACCOUNT\n";
    cout<<"        AND MODIFY AN ACCOUNT. THERE'S NO LOGIN SYSTEM FOR THIS PROJECT. ALL THE MAIN FEATURES FOR\n";
    cout<<"                                 BANKING SYSTEM ARE SET IN THIS PROJECT.                      \n";
    cout<<"\n";
    cout<<"\n";
    cout<<"PRESS ANY KEY TO CONTINUE.........";
    getch();
    system("cls");
}
void menu()
{
    int choice;
    char ans;
    do
    {
    cout<<"\n";
    cout<<"                               THE MAIN MENU\n";
    cout<<"\n";
    cout<<"\n";
    cout<<"                       1. Add a customer\n";
    cout<<"                       2. Account to account transfer\n";
    cout<<"                       3. Account statement Generation\n";
    cout<<"                       4. Cash Deposit\n";
    cout<<"                       5. Cash Withdrawl\n";
    cout<<"                       6. Modify details of customer\n";
    cout<<"                       7. Delete details of customer\n";
    cout<<"                       8. Display lists of customers\n";
    cout<<"                       9. Display lists of customers whose maximum balance is given by user\n";
    cout<<"                       10. EXIT\n";
    cout<<"\n";
    cout<<"                ENTER YOUR CHOICE : \n";
    customer c;
   cin>>choice;
    switch(choice)
   {
                case 5:
                system("cls");
                int acc;
                cout<<"Enter account no. to withdraw money\n";
                cin>>acc;
                if(c.checkac(acc))
                {
                    c.withdrawl(acc);
                    break;
                }
                else
                cout<<"ACCOUNT DOESNOT EXIST\n";
                break;
                case 4:
                    system("cls");
                    int ac;
                cout<<"Enter account no. to deposit money\n";
                cin>>ac;
                if(c.checkac(ac))
                {
                    c.deposit(ac);
                    break;
                }
                else
                    cout<<"ACCOUNT DOESNOT EXIST\n";
                break;
              case 3:
                  system("cls");
                  int a;
              cout<<"Enter the account no. to generate\n";
              cin>>a;
              if(c.checkac(a)==0)
                {
                    cout<<"ACCOUNT DOESNOT EXIST\n";
                    break;
                }
                    cout<<"Account holder's name"<<"|"<<"Account number"<<"|"<<"Balance"<<"|"<<"Type"<<endl;
                  //cout<<"account holder's name"<< setw(2)<<"|" <<setw(3)<<"account number"<<setw(3)<<"|"<<setw(3)<<"balance"<< setw(3)<<"|"<<setw(3)<<"type"<<endl;
                  c.generation(a);
               break;
                    case 2:
                    system("cls");
                    int f1;
                    int f2;
                    cout<<"Enter account from where money  is transferred\n";
                    cin>>f1;
                    if(c.checkac(f1)==0)
                    {
                      cout<<"ACCOUNT DOESNOT EXIST\n";
                      break;
                    }
                    cout<<"enter account to which money  is transferred\n";
                    cin>>f2;
                    if(c.checkac(f2)==0)
                    {
                      cout<<"ACCOUNT DOESNOT EXIST\n";
                      break;
                    }
                        c.transfer(f1,f2);
                break;
                case 1:
                    system("cls");
                    char p;
                    do
                    {
                        c.allocate();
                c.input();
                c.store();
                cout<<"Account holder's name"<<"|"<<"Account number"<<"|"<<"Balance"<<"|"<<"Type"<<endl;
                c.display();
                cout<<"Do you want to add more records(Y/N)\n";
                    cin>>p;
                    }while(p=='y'||p=='Y');
                break;
              case 6:
                system("cls");
                int ch;
                cout<<"Enter account no. to be updated\n";
                cin>>ch;
               c.updateacc(ch);
              break;
              case 7:
                system("cls");
                int d;
                cout<<"Enter account no. to be deleted\n";
                cin>>d;
                if(c.checkac(d)==0)
                {
                    cout<<"ACCOUNT DOESNOT EXIST\n";
                    break;
                }
                c.deleteacc(d);
                cout<<"After deletion\n";
                cout<<"Account holder's name"<<"|"<<"Account number"<<"|"<<"Balance"<<"|"<<"Type"<<endl;
                c.view();
            break;
                case 8:
                    system("cls");
                cout<<"Account holder's name"<<"|"<<"Account number"<<"|"<<"Balance"<<"|"<<"Type"<<endl;
                    c.view();
            break;
               case 9:
                   system("cls");
                   int l;
                    cout<<"Enter max. balance, you want to see which account owns\n";
                    cin>>l;
                    c.searchacc(l);
                break;
                case 10:
                    system("cls");
                    exit(0);
            break;
                default : cout<<"WRONG CHOICE!!!!!!!!!\n";
          }
          cout<<"Do you want to continue?\n";
          cin>>ans;
          system("cls");
}while(ans =='y'||ans =='Y');
    getch();
}
int main()
{
    welcome();
    menu();
}

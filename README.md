# project
#include<iostream>  
#include<conio.h>  
#include<string.h>  
#include<windows.h>  

using namespace std;


void mainmenu(void);
void AddMembers(void); 
void EditMembers(void);
void DeleteMembers(void);
void ViewMembers(void); 
void SearchMember(void);
void mainmenu_return(void);
int  Pass(void);
int  data(void); 
int  idchecker(int);

char list[][20]={"Member","Coach","Staff"};
FILE *fp,*ft,*fs;
int s,i=0;
char Finding;

void gotoxy (int x, int y)      
{	
	COORD c;  
	c.X = x; c.Y = y;  
	SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE),c); 
}

struct data
{
	int id;
	char name[20];
	char Address[20];
	char since[10];
	int contact;
	char *ptr;
}a;

int Pass(void) 
{	
	system("cls");
	i=0;
	char password[10]={"hello"};
	char ch,pass[10];
	cout<<"\n\n\n\t\t\t\tGYM MANAGEMENT SYSTEM\n";
	cout<<"\n\t\t\t\tEnter Password: ";
	while(ch!=13)
	{	
		ch=getch();
		if(ch!=13&&ch!=8){
			putch('*'); 
			pass[i]=ch; 
			i++;
		}
	}
	pass[i]='\0';
	if(strcmp(pass,password)==0){
		cout<<"\n\n\n\t\t\t\tLogged In.";
		cout<<"\n\n\n\t\tPress any key to continue!";
		getch();
		return 0;
	}
	else{
		cout<<"\n\n\n\t\t\t\tIncorrect Password!";
		getch();
		Pass();
	}
}

void mainmenu() 
{	
	system("cls"); 
	int i;
	gotoxy(40,3);
	cout<<" \t*MAIN MENU* \n ";
	gotoxy(40,5);
	cout<<"(1) Add Members   ";
	gotoxy(40,7);
	cout<<"(2) Remove Members";
	gotoxy(40,9);
	cout<<"(3) Search Members";
	gotoxy(40,11);
	cout<<"(4) View Member's list";
	gotoxy(40,13);
	cout<<"(5) Edit Members Record";
	gotoxy(40,15);
	cout<<"(6) Close Application";
	gotoxy(40,18);
	cout<<"Enter your choice:";
	switch(getch())
	{
		case '1':
		AddMembers();
		break;
		case '2':
		DeleteMembers();
		break;
		case '3':
		SearchMember();
		break;
		case '4':
		ViewMembers();
		break;
		case '5':
		EditMembers();
		break;
		case '6':{
			system("cls");    
			exit(0);
		}
		default:{
			gotoxy(10,25);
			cout<<"Wrong choice please, Please Choose option number: ";
			if(getch())
			mainmenu();
		}
	} 
} 

void AddMembers(void) 
{
	system("cls");
	int i;
	gotoxy(40,5);
	cout<<"\tSELECT CATEGORIES";
	gotoxy(40,7);
	cout<<"(1) New Member";
	gotoxy(40,9);
	cout<<"(2) Coach";
	gotoxy(40,11);
	cout<<"(3) Staff";
	gotoxy(40,13);
	cout<<"(4) Back to main menu";
	gotoxy(40,21);
	cout<<"Enter your choice: ";
	cin>>s;
	if(s==4)
	mainmenu();
	system("cls");
	fp=fopen("details.txt","a+");
	if(data()==1){
		a.ptr=list[s-1];
		fseek(fp,0,SEEK_END);
		fwrite(&a,sizeof(a),1,fp);
		fclose(fp);
		gotoxy(21,14);
		cout<<"The record is sucessfully saved";
		gotoxy(21,15);
		cout<<"Save any more?(Y / N):";
		if(getch()=='n')
		mainmenu();
		else
		system("cls");
		AddMembers();
	}
}

void DeleteMembers()    
{
	system("cls");
	int d;
	char another='y';
	while(another=='y'){
		system("cls");
		gotoxy(10,5);
		cout<<"Enter the ID to  remove: ";
		cin>>d;
		fp=fopen("details.txt","r+");
		rewind(fp);
	while(fread(&a,sizeof(a),1,fp)==1){
		if(a.id==d){
			gotoxy(10,7);
			cout<<"The record is available";
			gotoxy(10,8);
			cout<<"Name is %s"<<a.name;
			gotoxy(10,9);
			Finding='t';
		}
	}
	if(Finding!='t'){
		gotoxy(10,10);
		cout<<"No record is found modify the search";
		if(getch())
		mainmenu();
	}
	if(Finding=='t' ){
		gotoxy(10,9);
		cout<<"Do you want to delete it?(Y/N):";
		if(getch()=='y'){
			ft=fopen("details.txt","w+");  
			rewind(fp);
			while(fread(&a,sizeof(a),1,fp)==1){
				if(a.id!=d){
					fseek(ft,0,SEEK_CUR);
					fwrite(&a,sizeof(a),1,ft); 
				}                              
			}
			fclose(ft);
			fclose(fp);
			fp=fopen("details.txt","r+");    
			if(Finding=='t'){
				gotoxy(10,10);
				cout<<"The record is sucessfully deleted";
				gotoxy(10,11);
				cout<<"\n\tDelete another record?(Y/N)";
			}
		}
		else
		mainmenu();
		fflush(stdin);
		another=getch();
		}
	}
	gotoxy(10,15);
	mainmenu();
}

void SearchMember()
{
	system("cls");
	int d;
	cout<<"\t\tSearch Member";
	gotoxy(20,10);
	cout<<"Search By ID";
	gotoxy(20,14);
	cout<<"Press 1 to continues";
	fp=fopen("details.txt","r+"); 
	rewind(fp); 
	char se='1';
	switch(se){
	case '1':{
		system("cls");
		gotoxy(25,4);
		gotoxy(20,5);
		cout<<"Enter id to be searched: ";
		cin>>d;
		gotoxy(20,7);
		while(fread(&a,sizeof(a),1,fp)==1){
			if(a.id==d){
				gotoxy(20,6);
				cout<<"The Record is available\n";
				gotoxy(20,8);
				cout<<"ID:%d"<<a.id;
				gotoxy(20,9);
				cout<<"Category:%s"<<a.ptr;
				gotoxy(20,10);
				cout<<"Name:%s"<<a.name;
				gotoxy(20,11);
				cout<<"Address:%s "<<a.Address;
				gotoxy(20,12);
				cout<<"Contact:%i "<<a.contact;
				gotoxy(20,13);
				cout<<"Member Since:%s"<<a.since;
				Finding='t';
			}
		}
		if(Finding!='t'){
			cout<<"\aNo Record Found";
		}
		gotoxy(20,17);
		cout<<"Try another search?(Y/N)";
		if(getch()=='y')
		SearchMember();
		else
		mainmenu();
		break;
	}

	default :
	getch();
	SearchMember();
	}
fclose(fp);
}

void ViewMembers(void)  
{
	int i=0,j;
	system("cls");
	gotoxy(1,1);
	cout<<"\t\t\t\t\t\tMember List";
	gotoxy(2,2);
	cout<<"\n\t\t CATEGORY\tID \tNAME\tADDRESS\t\tCONTACT\t\tJOINED DATE";
	j=4;
	fp=fopen("details.txt","r");
	while(fread(&a,sizeof(a),1,fp)==1)
	{
		gotoxy(16,j);
		cout<<"%s"<<a.ptr;
		gotoxy(30,j);
		cout<<"%d"<<a.id;
		gotoxy(40,j);
		cout<<"%s"<<a.name;
		gotoxy(48,j);
		cout<<"%s"<<a.Address;
		gotoxy(65,j);
		cout<<"%i"<<a.contact;
		gotoxy(80,j);
		cout<<"%s"<<a.since;
		cout<<"\n\n";
		j++;
	}
		fclose(fp);
		gotoxy(35,25);
		mainmenu_return();
}  

void EditMembers(void) 
{
	system("cls");
	int c=0, d;
	gotoxy(20,4);
	cout<<"\t\tEdit Member's Record \t";
	char another='y';
	while(another=='y'){
		system("cls");
		gotoxy(15,6);
		cout<<"Enter Id to be edited:";
		cin>>d;
		fp=fopen("details.txt","r+");
		while(fread(&a,sizeof(a),1,fp)==1){
				if(idchecker(d)==0){
					gotoxy(15,7);
					cout<<"Member Found";
					gotoxy(15,8);
					cout<<"The ID: %d"<<a.id;
					gotoxy(15,9);
					cout<<"Enter new name: ";
					cin>>a.name;
					gotoxy(15,10);
					cout<<"Enter new Address: ";
					cin>>a.Address;
					gotoxy(15,11);
					cout<<"Enter new Contact: ";
					cin>>a.contact;
					gotoxy(15,12);
				    cout<<"Enter New Membership date: ";
					cin>>a.since;
					gotoxy(15,13);
					cout<<"The record is updated";
					fseek(fp,ftell(fp)-sizeof(a),0);
					fwrite(&a,sizeof(a),1,fp);
					fclose(fp);
					c=1;
				}
				if(c==0){
					gotoxy(15,9);
					cout<<"No record found";
				}
		}
		gotoxy(15,16);
		cout<<"Modify another Record?(Y/N)";
		fflush(stdin);
		another=getch();
	}
	mainmenu_return();
} 

void mainmenu_return(void) 
{
	gotoxy(15,20);
	cout<<"Press ENTER to return to main menu";
	a:
	if(getch()==13) 
	mainmenu();
	else
	goto a;
} 

int data() 
{
	int co;
	gotoxy(20,3);
	cout<<"Enter the Information Below";
	gotoxy(20,4);
	cout<<"Category: ";
	gotoxy(31,5);
	cout<<"%s"<<list[s-1];
	gotoxy(21,6);
	cout<<"ID: \t";
	gotoxy(30,6);
	cin>>co;
	if(idchecker(co) == 0){
		gotoxy(21,13);
		cout<<"\aThe id already exists\a";
		getch();
		mainmenu();
		return 0;
	}
	a.id=co;
	gotoxy(21,7);
	cout<<"Name: ";
	gotoxy(31,7);
	cin>>a.name;
	gotoxy(21,8);
	cout<<"Address: ";
	gotoxy(30,8);
	fflush(stdin);
	gets(a.Address);
	gotoxy(21,9);
	cout<<"Contact: ";
	gotoxy(31,9);
	cin>>a.contact;
	gotoxy(21,10);
	cout<<"Member Since: ";
	cin>>a.since;
	gotoxy(31,17);
	return 1;
}
  
int idchecker(int co)  
{
	rewind(fp);
	while(fread(&a,sizeof(a),1,fp)==1)
	if(a.id==co)
	return 0;  
	return 1; 
} 

int main(void)
{
	if(Pass()==0)
	mainmenu();
}

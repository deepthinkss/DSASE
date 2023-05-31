#include<iostream>
#include<stdio.h>
#include<fstream>
#include<cstring>
#include<stdlib.h>
using namespace std;
class Student
{
	int no;
	string name;
	string addr;
	long mob;
	public:
	void get();
	void print();
	friend fstream& operator <<(fstream &out ,Student &s);
	friend fstream& operator >>(fstream &in ,Student &s);
	friend class StudentInfo;
};
void Student::get()
	{
		cout<<"Enter roll number."<<endl;
		cin>>no;
		cout<<"Enter name."<<endl;
		cin>>name;
		cout<<"Enter address."<<endl;
		cin.ignore();
		getline(cin,addr);  /*he C++ getline() is a standard library function that is used to read a string or a line from an input stream. It is a part of the <string> header. The getline() function extracts characters from the input stream and appends it to the string object until the delimiting character is encountered. */
		cout<<"Enter mobile number."<<endl;
		cin>>mob;
	}
fstream& operator <<(fstream &out ,Student &s)
	{
		out<<s.no<<"\t";
		out<<s.name<<"\n";
		out<<s.addr<<"\n";
		out<<s.mob<<endl;
		return(out);
	}
void Student::print()
	{
		cout<<"Roll no. : "<<no<<"\t";
		cout<<"Name : "<<name<<endl;
		cout<<"Address : "<<addr<<endl;
		cout<<"Mobile no. : "<<mob<<endl;
	}

fstream& operator >>(fstream &in ,Student &s)
{
	in>>s.no;
	in>>s.name;
	in.ignore();              /*he cin.ignore() function is used which is used to ignore or clear one or more characters from the input buffer.*/
	getline(in,s.addr);
	in>>s.mob;
	return(in);
}
class StudentInfo
{
	fstream f;
	public:
	void add();
	void search();
	void del();
};
void StudentInfo::del()
{
	Student s;
	string s1;
	fstream f1;
	f1.open("Student1.text",  ios::out);      //ios class − This class is the base class for all stream classes.
	cout<<"Enter name to be deleted."<<endl;
	cin>>s1;
	f.open("Student.text",  ios::in);
	while(1)
	{
		f>>s;
		if(f.eof())   /*eof( ), that returns nonzero (meaning TRUE) when there are no more data to be read from an input file stream, and zero (meaning FALSE) otherwise.*/
			break;
		if(s.name!=s1)
			f1<<s;
	}
	f.close();
	f1.close();
	remove("Student.text");             /*deletes the file*/
	rename("Student1.text","Student.text");           /*rename(old file,new file)*/
}
			
			
void StudentInfo::add()
{
	Student s;
	s.get();
	f.open("Student.text",ios::app | ios::out | ios::in );
	f<<s;
	f.close();
}
void StudentInfo::search()
{
	Student s;
	string s1;
	cout<<"Enter the name of student to be searched."<<endl;
	cin>>s1;
	f.open("Student.text",ios::app | ios::out | ios::in );
	while(!f.eof())
	{
		f>>s;
		if(s.name==s1)
		{
			cout<<"Details :"<<endl;
			s.print();
			f.close();
			return;
		}
	}
	cout<<"Record of student not present."<<endl;
	f.close();
}
	
int main()
{
	StudentInfo Si;
	int op;
	do
	{
		cout<<"Select an option.\n1.Add record of new student.\n2.Delete record of a student.\n3.Display record of a student.\n4.Exit."<<endl;
		cin>>op;
		switch(op)
		{
			case 1:Si.add();
				break;
				
			case 2:Si.del();
				break;
				
			case 3:Si.search();
				break;
				
			case 4:break;
			
			default:cout<<"Enter a valid option."<<endl;
		}
	}while(op!=4);
	
	return(0);
}
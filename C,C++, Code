#include<fstream>
#include<iostream>
#include<string>
#include<stdlib.h>
#include <math.h>
using namespace std;
void setdata();
string bintohex(string buf);
void i2c_parse(string inputf, string outf);
//global variables
ifstream infile; //object infile of class ifstream
ofstream outfile; //object outfile of class ofstream
string item; //variable to store the line
int firstcount = 0;//variable to ignore the first line
int itemlen;
char scl, sda;//stores scl and sda data
char readwrite[20];//to store read or write W=0,R=1
string addresses[20];//to store adresses
string datas[20];//to store data's
//--------------------------------------------------------------------------
int main()
{
	string filei;//to call i2c_parse
	string fileo;
	cout << "enter name of input text file with .txt extension" << endl;
	cin >> filei;
	cout << "enter name of output text file with .txt extension" << endl;
	cin >> fileo;
	i2c_parse(filei, fileo);
	system("pause");
}
//--------------------------------i2c_parse---------------------------------
void i2c_parse(string inputf, string outf)
{
	int ps;//present state
	infile.open(inputf);
	if (infile.fail())
	{
		cerr << "Error opening file" << endl;
		exit(1);
	}
	int r = 0;//index count to store read or write
	int j = 0; //index to store addresses in array
	int i = 7; //address bit count
	int k = 8; //data bit count
	int l = 0;
	int check = 0;
	int ack = 0;
	int nack = 0;//to store nacks and acks
	string buffer = "0";
	string bufferd = "";
	int transactions = 0;
	ps = 1;//set present state to ideal
	while (!infile.eof())
	{
		setdata();
		switch (ps)
		{
		case 1://idle case
			if (scl == '1' & sda == '1')
				ps = 1;//present state is IDEAL
			if (scl == '1'& sda == '0')
				ps = 3;//present state is START
			break;
		case 2://start case
			if (scl == '0')
				ps = 2;
			if (scl == '1')
				ps = 3;//present state is ADDRESS
			break;
		case 3://address case
			if (i != 0)
			{
				if (scl == '1')
				{
					buffer = buffer + sda; //collect all the address bits
					i--;
				}
				ps = 3;
			}
			else if (i == 0)
			{
				string addre = bintohex(buffer);
				addre = "0x" + addre;
				addresses[j] = addre;
				j++;
				addre = "";//reset address
				buffer = "";//reset buffer
				i = 7;//reset adress bit count
				ps = 4;
			}
			break;
		case 4://read or write case
			if (scl == '1')
			{
				if (sda == '0')
				{
					readwrite[r] = 'W';
					r++;
					ps = 5;
				}
				if (sda == '1')
				{
					readwrite[r] = 'R';
					r++;
					ps = 5;
				}
			}
			break;
		case 5://ACK case
			if (scl == '1')
			{
				if (sda == '0')
				{
					ack++;
					ps = 6;
				}
				else
				{
					nack++;
					r--;
					j--;
					ps = 1;
				}
			}
			break;
		case 6://data case
			if (k != 0)
			{
				if (scl == '1')
				{
					bufferd = bufferd + sda; //collect all the address bits
					k--;
				}
				ps = 6;
			}
			else if (k == 0)
			{
				string addre = bintohex(bufferd);
				addre = "0x" + addre;
				datas[l] = addre;
				l++;
				addre = "";//reset address
				bufferd = "";//reset buffer
				check = 0;//reset check
				k = 8;//reset adress bit count
				ps = 7;
			}
			break;
		case 7://ACK case
			if (scl == '1')
			{
				if (sda == '0')
				{
					ack++;
					ps = 8;
				}
				else
				{
					transactions++;
					nack++;
					ps = 1;
				}
			}
			break;
		case 8: //more data
			check++;
			if (scl == '1' || check == 4)
			{
				if (check == 3)
				{
					bufferd = bufferd + sda;
					k = 7;
				}
				if (check == 4)
				{
					if (scl == '1' & sda == '1')
					{
						ps = 9;
					}
					else
					{
						j++;
						r++;//increment datas index and address index whenwver there is data load
						ps = 6;
					}
				}
			}
			break;
		case 9://stop
			transactions++;
			bufferd = "";
			k = 8;
			ps = 1;
			break;
		} //end of switch

	}//--------end of while loop----AKA state machine-------------
	infile.close();
	//----------Load to output file-----------------------------------
	int read = 0;
	int write = 0;
	outfile.open(outf);
	outfile << "Total number of transactions(including NACK): " << transactions << endl;
	for (int m = 0; m < 20; m++)
	{
		if (readwrite[m] == 'R')
			read++;
		if (readwrite[m] == 'W')
			write++;
	}
	outfile << "Toatal number of Master writes: " << write << endl;
	outfile << "Toatal number of Master reads: " << read << endl;
	outfile << "Toatal number of ACK transactions: " << ack << endl;
	outfile << "Toatal number of NACK transaction: " << nack << endl;
	outfile << "List of all read/write operations" << endl;
	outfile << "    TYPE       ADDRESS     DATA" << endl;
	for (int m = 0; m < 20; m++)
	{
		outfile << "      " << readwrite[m] << "          " << addresses[m] << "          " << datas[m] << endl;
	}
	outfile.close();
	cin.get();
}

//-----------------------setdata function to set SCL and SDA-----------------------
void setdata()
{
	getline(infile, item);//get each line
	firstcount++; //ignore the first line
	if (firstcount != 1)
	{
		itemlen = item.length(); //to check for different string lenghts
		if (itemlen == 9) //when string length
		{
			scl = item[4];
			sda = item[8];

		}
		if (itemlen == 10)
		{
			scl = item[5];
			sda = item[9];

		}
		if (itemlen == 11)
		{
			scl = item[6];
			sda = item[10];

		}

	}

}

//-----------------------BINARY TO HEX----------------------------
string bintohex(string buf)
{
	long int longint = 0;
	int len = buf.size();
	for (int i = 0; i < len; i++)
	{
		longint += (buf[len - i - 1] - 48) * pow(2, i);//decimal
	}
	//decimal to hex
	long int remainder, quotient;
	int i = 1, j, temp;
	char hexadecimalNumber[100];
	quotient = longint;
	while (quotient != 0)
	{
		temp = quotient % 16;

		//To convert integer into character
		if (temp < 10)
			temp = temp + 48;
		else
			temp = temp + 55;

		hexadecimalNumber[i++] = temp;
		quotient = quotient / 16;
	}
	string finals = "";
	for (j = i - 1; j > 0; j--)
		finals = finals + hexadecimalNumber[j];
	return finals;
}

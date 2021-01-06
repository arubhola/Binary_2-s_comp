#include <iostream>
#include <cmath>
#include <string>
using namespace std;

string flip (string num)
{
	for(i = 0; i<len; i++)
		if(num[i] == '0')
			num[i] = '1';
		else
			num[i] = '0';
	
	return num;
}

string addOne (string num)
{
	for(int i=len-1; i>=0; i--)
       	{
		if(num[i] == '0')
		{
			num[i] = '1';
			break;
		}
		else if(num[i] == '1')
			num[i] = '0';
	}

	return num;
}

string twosComplementStringsAddition(string a, string b)
{
	int slen = a.length(), i, flag = 0;
    string x;
	
	for(i = slen - 1; i >= 0; i--)
	{
		if(a[i] == '0' && b[i] == '0')
		{
			if(flag)
			{
				x = '1' + x;
				flag = 0;
			}
			else
			{
				x = '0' + x;
			}
		}
		else if((a[i] == '1' && b[i] == '0')||(a[i] == '0' && b[i] == '1'))
		{
			if(flag)
			{
				x = '0' + x;
				flag = 1;
			}
			else
			{
				x = '1' + x;
			}
		}
		else if(a[i] == '1' && b[i] == '1')
		{
			if(flag)
			{
				x = '1' + x;
				flag = 1;
			}
			else
			{
				x = '0' + x;
				flag = 1;
			}
		}
	}

	return x;
}


string decimalToTwosComplementString(int x, int len)
{
	int flag = 0, i, count_len = 0, temp, slen;
	char elem;
	string num;
	if(x < 0)
	{
		flag++;
		x = abs(x);
	}

	while(x!= 0 &&  (count_len < len))
	{
		elem = (x%2) + 48;
		num = num + elem;
		x = x/2;
		count_len++;
	}
	count_len--;
	
	slen = num.length();
		
	if(count_len != (len-1))
	{
		for(i = count_len+1; i<len; i++)
		{
			num = num + '0';
			slen++;
		}
	}

	for(i = 0; i < (slen/2); i++)	
	{
		temp = num[i];
		num[i] = num[slen-i-1];
		num[slen - i - 1] = temp;
	}

	if(flag)
	{
		
		num = flip(num);
		num = addOne(num);	
		
	}

	return num;
}


int twosComplementStringToDecimal(string num)

{
	int sum=0, slen = num.length(), flag = 0;
	double i=0;
	if(num[0] == '1')
	{
		flag ++;

		num = flip(num);

		num = addOne(num);
	}

	for(i=0; i<slen; i++)
	{
		if(num[i]=='1')
			sum+=pow(2.0,(slen - i - 1));
	}


	if(flag)
		return -sum;
	else
		return sum;
}

int main()
{
	int L, num;

	do
	{
		cout << "Enter positive integer for the bit pattern size ";
		cin >> L;
	}while (L <= 0);

	//Read in two integers a and b 
	int a, b;
	cout << "Enter an integer a ";
	cin >> a;
	cout << "Enter an integer b ";
	cin >> b;

	//Calculate the decimal arithmetic sum of a and b and print the result
	int c1 = a + b;
	cout << "\nIn decimal " << a << " + " << b << " is " << c1 << endl;

	//Compute the two's complement representations of a and b
	//Each integer must be represented in L-bits pattern
	//Also these two's complement representations must be returned as string data types
	string A = decimalToTwosComplementString(a, L);
	string B = decimalToTwosComplementString(b, L);

	//Print the two's complement representations of a and b
	cout << "\nThe two's complement of " << a << " is\t " << A << endl;
	cout << "The two's complement of " << b << " is\t " << B << endl;

	//Compute the binary sum of the two's complement representations of a and b
	//The result must be returned as L-bit pattern string data type
	string C = twosComplementStringsAddition(A, B);

	//Print the two's complement representation binary sum
	cout << "\nThe binary sum of " << A << " and " << B << " is " << C << endl;

	//Convert the two's complement representation binary sum to decimal and print
	int c2 = twosComplementStringToDecimal(C);
	cout << "\nIn two's complement arithmetic, " << a << " + " << b << " is " << c2 << endl<<endl;

	//Print some concluding results
	if (c1 == c2)
		cout << c1 << " is equal to " << c2 << ". Good Job!" << endl;
	else
	{
		cout << c1 << " is not equal to " << c2 << endl;
		cout << "Either " << c1 << " cannot be represented by the given bit pattern OR we have made a mistake!" << endl;
	}

	cout<<endl;

	system("Pause");
	return 0;
}

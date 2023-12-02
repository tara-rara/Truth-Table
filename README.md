#include <iostream>
#include<string>
#include <windows.h>
#include <stdlib.h>
using namespace std;
class Check
{
public:
	
	string str;      //for string
	int len;         //for length of string
	int conc;        //for conclusion
	int p[10];       //for premises

	//operators
	int OR(int a, int b)
	{
		if (a == 0 && b == 0)
			return 0;
		else
			return 1;
	}

	int AND(int a, int b)
	{
		if (a == 1 && b == 1)
			return 1;
		else
			return 0;
	}

	int C1(int a, int b)
	{
		if (a == b)
			return 1;
		else
			return 0;
	}

	int C2(int a, int b)
	{
		if (a == 1 && b == 0)
			return 0;
		else
			return 1;
	}
	//for 3 variables
	int var3(string s, int a, int b, int c, int d, int p)
	{
		p = 0;   //answer of premises
		int fp = 5, lp = 5;     //for brackets
		if (s[0] == '(')
		{
			if (s[2] == 'v')
				fp = OR(a, b);
			else if (s[2] == '^')
				fp = AND(a, b);
			else if (s[2] == '<')
				fp = C1(a, b);
			else if (s[2] == '>')
				fp = C2(a, b);
		}
		int i = 1;
		while (s[i] > 0)
		{
			if (s[i] == '(')
			{
				if (s[i + 2] == 'v')
					lp = OR(b, c);
				else if (s[i + 2] == '^')
					lp = AND(b, c);
				else if (s[i + 2] == '<')
				{
					lp = C1(b, c);
					i = i + 1;
				}
				else if (s[i + 2] == '>')
					lp = C2(b, c);
			}
			if (s[i] == ')')
			{
				if (s[i + 1] == 'v')
					p = OR(fp, c);
				else if (s[i + 1] == '^')
					p = AND(fp, c);
				else if (s[i + 1] == '<')
				{
					p = C1(fp, c);
					i = i + 1;
				}
				else if (s[i + 1] == '>')
					p = C2(fp, c);
			}
			i++;
		}
		if ((s[0] <= 90 && s[0] >= 65) || (s[1] <= 90 && s[1] >= 65))
		{
			if (s[1] == 'v')
				p = OR(a, lp);
			else if (s[1] == '^')
				p = AND(a, lp);
			else if (s[1] == '<')
				p = C1(a, lp);
			else if (s[1] == '>')
				p = C2(a, lp);
		}


		else if (lp < 4 && fp < 4)
		{
			if (s[5] == 'v')
				p = OR(fp, lp);
			else if (s[5] == '^')
				p = AND(fp, lp);
			else if (s[5] == '<')
				p = C1(fp, lp);
			else if (s[5] == '>')
				p = C2(fp, lp);
		}
		cout << "   " << p << "  |  ";
		return p;

	}
	//for 2 variables
	int var2(string s, int c, int d, int p)
	{
		int i = 0;

		while (s[i] > 0)
		{
			if (s[i] == 'v')
			{
				p = OR(c, d);
			}
			else if (s[i] == '^')
			{
				p = AND(c, d);
			}
			else if (s[i] == '<')
			{
				p = C1(c, d);
				i = i + 1;
			}
			else if (s[i] == '>')
			{
				p = C2(c, d);
			}
			i++;
		}
		cout << "   " << p << "    | ";
		return p;
	}
	//for 1 variable
	int var1(string s, int c, int p)
	{
		if (s[0] == '~')
		{
			if (c == 0)
				c = 1;
			else
				c = 0;
			p = c;
		}
		else
			p = c;
		cout << "   " << p << "    | ";
		return p;
	}
	//to combine all functions
	void table(string m, int a, int b, int c, int d, int g)
	{
		int am=0;
		len = m.length();
		int count = 0;
		int q = 0;
		while (m[count] > 0)
		{
			if (m[count] >= 65 && m[count] <= 90)
				q++;
			count++;
		}
		if (q == 1)
		{
			p[g]=var1(m, a, am);
		}
		else if (q == 2)
		{

			p[g]=var2(m, a, b, am);

		}
		else if (q == 3 || q == 4)
		{
			p[g]=var3(m, a, b, c, d, am);
		}


	}

};
void ex(string d, char var[], int i, int j, int k, int p, Check &c1);
 
int main()
{
	system("Color FD");
	cout << "----------TRUTHT TABLE LITE---------" << endl ;
	cout << "Please enter capital letters for variables!" << endl << endl << endl ;
	int n, t;
	char var[6];
	cout << "Enter no of variables:";
	cin >> t;
	cout << "Enter variables:";
	for (int i = 0; i < t; i++)
	{
		cin >> var[i];
	}
	cout << "Enter number of premises: ";
	cin >> n;
	cout << "     COMMANDS     " << endl << "Enter v for OR" << endl << "Enter ^ for AND" << endl << "Enter > for Conditional";
	cout << endl << "Enter <> for Biconditional" << endl;
	string m[10];
	for (int i = 0; i < n; i++)
	{
		cout << "Premise " << i + 1<<" : ";
		cin >> m[i];
	}
	string c;
	cout << "Enter conclusion: ";
	cin >> c;
	Check c1[100];
	int con = 0;
	//if 2 variables
	if (t == 2)
	{
		int p = 0;
		string d;
		cout << var[0] << " | " << var[1] << " |  ";
		for (int i = 0; i < n; i++)
		{
			cout << m[i] << "    |   ";
		}
		cout << c << "   |" << endl;
		for (int i = 0; i < 2; i++)
		{
			for (int j = 0; j < 2; j++)
			{
				cout << i << " | " << j << " | ";
				p = 0;
				while (p < n)
				{
					d = m[p];

					if (d[0] == var[0])
						c1[con].table(d, i, j, 0, 0, p);
					else
						c1[con].table(d, j, i, 0, 0, p);
					p++;
					
				}
				if (c[0] == var[0])
					c1[con].table(c, i, j, 0, 0, p);
				else
					c1[con].table(c, j, i, 0, 0, p);

				cout << endl ;
				con++;
			}

		}
	}
	//if 3 variables
	if (t == 3)
	{
		int p = 0;
		string d;
		
		cout << var[0] << " | " << var[1] << " | " << var[2] << " | ";
		for (int i = 0; i < n; i++)
		{
			cout << m[i] << "     | ";
		}
		cout << c << " |  " << endl;
		for (int i = 0; i < 2; i++)
		{
			for (int j = 0; j < 2; j++)
			{
				for (int k = 0; k < 2; k++)
				{
					cout << i << " | " << j << " | " << k << " | ";
					p = 0;
					while (p < n)
					{
						d = m[p];
						ex(d, var, i, j, k, p, c1[con]);
						
						p++;

					}
					ex(c, var, i, j, k, p, c1[con]);
					cout << endl;
					con++;
				}
			}

		}
	}
	//to  check all premises and conclusions
	bool chq1 = 1, chq2[20];
	for (int i = 0; i <= con; i++)
	{
		chq1 = 1;
		for (int j = 0; j < n - 1; j++)
		{
			if (c1[i].p[j] == 1 && c1[i].p[j + 1] == 1)
				continue;
			else
				chq1 = 0;
		}
		cout << endl;
		if (chq1 == 1 && c1[i].p[n] == 0)
			chq2[i] = 0;
		else
			chq2[i] = 1;
	}
	chq1 = 1;
	for (int i = 0; i < con; i++)
	{
		if (chq2[i] == 0)
			chq1 = 0;
	}
	//check if valid or not
	if (chq1 == 0)
		cout << "Invalid";
	else
		cout << "Valid";
	

}

// to check variables
void ex(string d, char var[], int i, int j, int k, int p, Check &c1) {

	if ((d[0] == var[0] || d[1] == var[0]) && (d[2] == var[1] || d[3] == var[1]))
		c1.table(d, i, j, k, 0, p);
	else if ((d[0] == var[0] || d[1] == var[0]) && (d[2] == var[2] || d[3] == var[2]))
		c1.table(d, i, k, j, 0, p);
	else if ((d[0] == var[1] || d[1] == var[1]) && (d[2] == var[0] || d[3] == var[0]))
		c1.table(d, j, i, k, 0, p);
	else if ((d[0] == var[1] || d[1] == var[1]) && (d[2] == var[2] || d[3] == var[2]))
		c1.table(d, j, k, i, 0, p);
	else if ((d[0] == var[2] || d[1] == var[2]) && (d[2] == var[0] || d[3] == var[0]))
		c1.table(d, k, i, j, 0, p);
	else if ((d[0] == var[2] || d[1] == var[2]) && (d[2] == var[1] || d[3] == var[1]))
		c1.table(d, k, j, i, 0, p);
	else if (d[0] == var[0] || d[1] == var[0])
		c1.table(d, i, j, 0, 0, p);
	else if (d[0] == var[1] || d[1] == var[1])
		c1.table(d, j, i, 0, 0, p);
	else
		c1.table(d, k, i, 0, 0, p);
}

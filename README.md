# GenderPrediction
A Simple Bayesian Gender Prediction System
## `引言`

**根据朴素贝叶斯分类算法而作的性别预测系统**

**数据来源：网络爬虫**

## `完整代码`

```c++
#include<vector>
#include<locale>
#include<fstream>
#include<cstring>
#include<iostream>
#include<comutil.h>
#include<windows.h>
#pragma comment(lib,"comsuppw.lib")
#define MAX 21000
using namespace std;

double p1, p0;						//先验概率
double sum1, sum0;					//总和统计
int statMan[MAX];					//统计数组(男)
int statWoman[MAX];					//统计数组(女)
vector <wstring> manName;			//Name向量(男)
vector <wstring> womanName;			//Name向量(女)

void interFace();					//界面
void duceDate();					//引入数据
void statDate();					//统计数据
void handleDate();					//处理数据
double pMan(wchar_t w);				//概率函数
double pWoman(wchar_t w);			//概率函数
void judgeDate(string s);			//判断数据
wstring converse(const string& s);	//string转wstring

int main()
{
	duceDate();
	statDate();
	interFace();
	return 0;
}

void interFace()
{
	cout << "系统加载中";
	for (int i = 1; i <= 3; i++)
	{
		Sleep(700);
		cout << ".";
	}
	cout << endl << "系统加载成功！\n" << endl;
	cout << "****************************" << endl;
	cout << "*  朴素贝叶斯性别预测系统  *" << endl;
	cout << "*     输入姓名预测性别     *" << endl;
	cout << "*     输入over程序结束     *" << endl;
	cout << "****************************" << endl;
	string s = "hello";
	while (s != "over")
	{
		cout << "--> ";
		cin >> s;
		if (s == "over")
			break;
		else
			judgeDate(s);
	}
	cout << "已结束,下次再见" << endl;
}
void duceDate()
{
	for (int i = 1; i < MAX; i++)
		statMan[i] = statWoman[i] = 1;
	ifstream f("train.txt");
	if (!f.is_open())
		cout << "The file can not open" << endl;
	else
	{
		int temp;
		string name;
		int gender;
		for (int i = 1; i <= 12000; i++)
		{
			f >> temp >> name >> gender;
			if (gender == 1)
				manName.push_back(converse(name));
			else
				womanName.push_back(converse(name));
		}
	}
	f.close();
}
void statDate()
{
	p1 = manName.size() * 1.0 / (manName.size() + womanName.size());
	p0 = womanName.size() * 1.0 / (manName.size() + womanName.size());
	//统计(男)
	for (int i = 0; i < manName.size(); i++)
	{
		int len = manName[i].size();
		if (len == 1)
		{
			wchar_t t = manName[i][0];
			statMan[t - 19967]++;
		}
		else
		{
			wchar_t t1 = manName[i][0];
			wchar_t t2 = manName[i][1];
			statMan[t1 - 19967]++;
			statMan[t2 - 19967]++;
		}
	}
	//统计(女)
	for (int i = 0; i < womanName.size(); i++)
	{
		int len = womanName[i].size();
		if (len == 1)
		{
			wchar_t t = womanName[i][0];
			unsigned short c = t;
			statWoman[c - 19967]++;
		}
		else
		{
			wchar_t t1 = womanName[i][0];
			wchar_t t2 = womanName[i][1];
			unsigned short c = t1;
			statWoman[c - 19967]++;
			c = t2;
			statWoman[c - 19967]++;
		}
	}
	for (int i = 1; i < MAX; i++)
	{
		sum1 += statMan[i];
		sum0 += statWoman[i];
	}
}
double pMan(wchar_t w)
{
	unsigned short c = w;
	return statMan[c - 19967] / sum1;
}
double pWoman(wchar_t w)
{
	unsigned short c = w;
	return statWoman[c - 19967] / sum0;
}
void judgeDate(string s)
{
	wstring ws = converse(s);
	int wsl = ws.size();
	if (wsl == 2)
	{
		double pa = pMan(ws[1]) * p1;
		double pb = pWoman(ws[1]) * p0;
		cout << "概率：" << "男 " << pa / (pa + pb) << " 女 " << pb / (pa + pb);
		cout << "\n结果：";
		if (pa >= pb)
			cout << "男\n";
		else
			cout << "女\n";
	}
	else
	{
		double pa = pMan(ws[1]) * pMan(ws[2]) * p1;
		double pb = pWoman(ws[1]) * pWoman(ws[2]) * p0;
		cout << "概率：" << "男 " << pa / (pa + pb) << " 女 " << pb / (pa + pb);
		cout << "\n结果：";
		if (pa >= pb)
			cout << "男\n";
		else
			cout << "女\n";
	}
}
wstring converse(const string& s)
{
	_bstr_t t = s.c_str();
	wchar_t* pwchar = (wchar_t*)t;
	wstring result = pwchar;
	return result;
}
```

## `实例测试结果`

**样本数据：男50人，女50人**

**男性预测正确率：90.23%**

**女性预测正确率：81.33%**

**综合正确率：85.78%**

## `感谢名录`

<font size=5 color=#D1DAF2>**Do_r**</font> 　　　　　　　<font size=5 color=#0E0E0E>**Arthur**</font>　　　　　　　　　<font size=5 color=#007CFF>**阳**</font>

<font size=5 color=#8FD1EC>**Ttkx**</font> 　　　　　　　<font size=5 color=#9D87D2>**Mine`♡**</font>　　　　　　　　　<font size=5 color=#D1DAF2>**沫殇心**</font>

<font size=5 color=#007CFF>**Tremor.**</font> 　　　　　　<font size=5 color=#8FD1EC>**太阳**</font>　　　　　　　　　　<font size=5 color=#0E0E0E>**yeyu**</font>

<font size=5 color=#8FD1EC>**不如去散步**</font>　　　　　<font size=5 color=#9D87D2>**偷偷藏不住**</font>　　　　　　<font size=5 color=#B884F2>**有一点疼的小耳朵**</font>

<font size=5 color=#0E0E0E>**ECCENTRIC**</font> 　　　　　<font size=5 color=#B884F2>**salad  days**</font>　　　<font size=5 color=#D1DAF2>**桃李不言**</font> 　　　　<font size=5 color=#D1DAF2>**Maid**</font>			

<font size=5 color=#8FD1EC>**Kneel**</font>　　　　　　　　　<font size=5 color=#9D87D2>**程喻吖**</font>　　　　　　　　　<font size=5 color=#D1DAF2>**咿呀咿呀呦**</font>


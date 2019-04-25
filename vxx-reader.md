## Visual Studio 2017 64bitのC++プログラムから直接読む方法

Hayakawa氏が、fvxxおよびbvxxを直接読むためのライブラリを用意してくれました。

`E:\udd\hayakawa\VxxReader`

詳細はreadme.txtを読んで下さい。

# C++から直接読む方法

VS2017 64bit以外からVXX形式を汎用的に読む方法は現状ありません。dumpしてバイナリで読んで下さい。サンプルコードは以下の通りです。

```cpp
#include <string>
#include <fstream>
#include <iostream>
#include <vector>
using namespace std;

class micro_track_subset_t {
public:
	double ax, ay, z;
	int ph, pos, col, row, zone, isg;
	int64_t rawid;
};

class base_track_t {
public:
	double ax, ay, x, y, z;
	int pl, isg, zone, dmy;
	int64_t rawid;
	micro_track_subset_t m[2];
};

int main() {
	string filepath = "b001.dmp";
	vector<base_track_t> vin;

	std::ifstream ifs(filepath, std::ios::binary);
	if (!ifs) { std::cout << "cannot open " << filepath << std::endl; throw 0xff; }
	while (true)
	{
		base_track_t p;
		ifs.read((char*)& p, sizeof(base_track_t));
		if (ifs.eof()) break;
		vin.push_back(p);
	}
	ifs.close();
    
    for(const auto &p: vin){
        cout<<p.x<<" "<<p.y<<endl;
    }
}

```
# Pythonから直接読む方法

VXX形式をpythonから直接読む方法は現状ありません。dumpしてバイナリで読んで下さい。サンプルコードは以下の通りです。

```python
import struct
import matplotlib.pyplot as plt

#dはdouble
#iは32bit signed int
#qは64bit signed int
struct_fmt = 'dddddiiiiqdddiiiiiiqdddiiiiiiq'
struct_len = struct.calcsize(struct_fmt)
struct_unpack = struct.Struct(struct_fmt).unpack_from
i = 0
x = []
y = []
with open(r"b001.dmp", "rb") as f:
    while True:
        data = f.read(struct_len)
        if not data: break
        s = struct_unpack(data)
        if i % 1000000 == 0:
            print("Read",i,"tracks")
        x.append(s[2] / 1000)
        y.append(s[3] / 1000)
        i+=1

# XYの飛跡密度分布を描画
plt.hist2d(x,y, bins=[100,100],range=[[0,100],[0,100]])
plt.colorbar()
plt.show()
```
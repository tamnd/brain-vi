---
title: "CF 102437J - Robot giao hàng"
description: "Robot di chuyển trong mặt phẳng nguyên và có hai tâm quay cố định là các điểm ((0,0)) và ((1,0)). Mỗi lệnh xoay vị trí hiện tại một cách chính xác (90^circ), theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ, xung quanh một trong hai tâm này."
date: "2026-08-09T18:01:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "J"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 868
verified: false
draft: false
---

[CF 102437J - Robot giao hàng](https://codeforces.com/problemset/problem/102437/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14 phút 28 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Robot di chuyển trong mặt phẳng nguyên và có hai tâm quay cố định là các điểm ((0,0)) và ((1,0)). Mỗi lệnh xoay vị trí hiện tại một cách chính xác (90^\circ), theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ, xung quanh một trong hai tâm này. Do đó, bốn lệnh chỉ là bốn phép biến đổi affine của tọa độ hiện tại. 

Nếu điểm hiện tại là ((x,y)), các phép biến đổi là 

[ 
1:(x,y)\mapsto(y,-x), 
] 

[ 
2:(x,y)\mapsto(-y,x), 
] 

[ 
3:(x,y)\mapsto(y+1,1-x), 
] 

[ 
4:(x,y)\mapsto(1-y,x-1). 
] 

Chúng ta được cho một điểm xuất phát và một điểm đến khác. Nhiệm vụ là xuất ra bất kỳ chuỗi nào gồm nhiều nhất (10^6) lệnh chuyển đổi điểm đầu tiên thành điểm thứ hai hoặc xuất ra (-1) khi không tồn tại chuỗi đó. 

Các tọa độ được giới hạn bởi (100000) về giá trị tuyệt đối, do đó độ dịch chuyển giữa hai điểm tối đa là (200000) ở một trong hai tọa độ. Một cách tiếp cận khám phá rõ ràng một biểu đồ trạng thái khổng lồ là không cần thiết và có khả năng nguy hiểm vì bản thân câu trả lời được yêu cầu có thể chứa hàng trăm nghìn lệnh. Chúng tôi muốn một công trình có thời gian chạy về cơ bản tỷ lệ thuận với độ dài đầu ra, ở đây dễ dàng đủ nhanh. 

Có hai trường hợp nguy hiểm mà việc xây dựng bất cẩn có thể xử lý sai. Đầu tiên, sự bình đẳng là cần thiết. Ví dụ,```
0 1
1 1
```phải sản xuất`-1`. Mọi lệnh đều bảo toàn tính chẵn lẻ của (x+y), do đó hai điểm không thể kết nối được. Một tìm kiếm chỉ kiểm tra xem tọa độ có "gần" hay không có thể bỏ sót bất biến này. 

Thứ hai, đích đến được đảm bảo khác với điểm bắt đầu, nhưng các bản dịch trung gian có thể có hệ số bằng 0. Ví dụ,```
0 0
1 1
```chỉ cần một bản dịch đơn vị theo hướng ((1,1)). Việc xây dựng phải cho phép một hệ số bằng 0 thay vì vô tình đưa ra câu trả lời trống khi chỉ cần một hướng. 

Mẫu trong câu lệnh được cung cấp có lỗi định dạng ở đầu vào thứ hai, nhưng vấn đề ban đầu đưa ra mẫu thứ hai là```
0 1
1 1
```với đầu ra`-1`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là coi mọi điểm nguyên là một đỉnh của đồ thị và thử tất cả bốn lệnh từ mọi điểm có thể tiếp cận. Mỗi bước di chuyển đều mang tính xác định, do đó tìm kiếm theo chiều rộng cuối cùng sẽ tìm thấy chuỗi ngắn nhất bất cứ khi nào có thể tiếp cận được mục tiêu. Điều này đúng vì mọi lệnh hợp pháp đều được biểu thị bằng một cạnh. 

Vấn đề là mặt phẳng không bị giới hạn và ngay cả trong phạm vi tọa độ liên quan đến đầu vào cũng có khoảng (400000^2) vị trí nguyên có thể có. Đó là khoảng (1.6\cdot10^{11}), vượt xa mọi thứ mà việc triển khai trong hai giây có thể khám phá. Ngay cả bán kính tìm kiếm nhỏ hơn nhiều cũng không thể chấp nhận được. 

Quan sát hữu ích là việc kết hợp các phép quay quanh hai tâm khác nhau sẽ tạo ra các phép tịnh tiến. Nhận lệnh (1), tiếp theo là lệnh (4). Lệnh (1) quay theo chiều kim đồng hồ quanh điểm gốc và lệnh (4) quay ngược chiều kim đồng hồ quanh ((1,0)). Về mặt đại số, 

[ 
(x,y)\xrightarrow{1}(y,-x) 
\xrightarrow{4}(x+1,y-1). 
] 

Do đó, chuỗi hai lệnh`14`di chuyển mọi điểm một cách chính xác ((1,-1)), bất kể tọa độ hiện tại của nó. 

nghịch đảo của nó là`32`, di chuyển mọi điểm theo ((-1,1)). 

Bây giờ hãy xoay bản dịch này theo (90^\circ). Trình tự`1142`là liên hợp của`14`và di chuyển mọi điểm theo ((1,1)). nghịch đảo của nó là`1322`, di chuyển mọi điểm theo ((-1,-1)). 

Điều này làm giảm bài toán hình học thành số học số nguyên thông thường. Nếu chuyển vị yêu cầu là 

[ 
(dx,dy)=(x_2-x_1,y_2-y_1), 
] 

chúng ta có thể viết nó như 

a(1,1)+b(1,-1), 
] 

ở đâu 

[ 
a=\frac{dx+dy}{2}, 
\qquad 
b=\frac{dx-dy}{2}. 
] 

Các hệ số này chính xác là số nguyên khi (dx) và (dy) có cùng tính chẵn lẻ, tương đương khi (x_1+y_1) và (x_2+y_2) có cùng tính chẵn lẻ. 

Điều kiện chẵn lẻ đó cũng cần thiết. Theo lệnh (1), 

[ 
x+y\mapsto y-x, 
] 

có cùng tính chẵn lẻ với (x+y). Tính toán tương tự áp dụng cho cả bốn lệnh. Do đó tính chẵn lẻ của (x+y) là bất biến của toàn bộ quá trình. 

Vậy điều kiện vừa cần vừa đủ. Khi nó được giữ, chúng tôi chỉ cần lặp lại hai bản dịch có sẵn theo số lần yêu cầu. 

Giá trị tuyệt đối lớn nhất có thể có của (a) hoặc (b) là (200000). Bản dịch chéo sử dụng bốn lệnh cho mỗi đơn vị và bản dịch ((1,-1)) sử dụng hai lệnh cho mỗi đơn vị. Trường hợp xấu nhất là nhiều nhất (800000) lệnh, do đó giới hạn (10^6) bắt buộc được tôn trọng một cách thoải mái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(V+E)), trạng thái tiềm năng (10^{11}) | (O(V)) | Quá chậm | 
| Dịch thuật xây dựng | (O( | a | + | b | )) | (O( | a | + | b | )) cho câu trả lời | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc điểm bắt đầu ((x_1,y_1)) và điểm đến ((x_2,y_2)), sau đó tính toán 

[ 
dx=x_2-x_1,\qquad dy=y_2-y_1. 
] 

Toàn bộ công trình chỉ phụ thuộc vào sự dịch chuyển này vì các chuyển dịch được tạo ra hoạt động giống hệt nhau ở mọi nơi. 

1. Kiểm tra xem (dx) và (dy) có cùng tính chẵn lẻ hay không. Tương tự, hãy kiểm tra xem 

[ 
(dx+dy)\bmod 2=0. 
] 

Nếu điều này là sai, hãy in`-1`. Mọi lệnh hợp lệ đều bảo toàn tính chẵn lẻ của (x+y), do đó không có chuỗi nào có thể đến đích. 

1. Tính toán 

[ 
a=\frac{dx+dy}{2},\qquad b=\frac{dx-dy}{2}. 
] 

Sau đó 

# (a+b,a-b) 

(dx,dy). 
] 

Sự dịch chuyển hiện đã được phân tách thành hai bản dịch mà chúng tôi biết cách tạo ra. 

1. Nếu (a>0), nối thêm`1142`chính xác (a) lần. Trình tự này dịch điểm hiện tại theo ((1,1)). Nếu (a<0), nối thêm`1322`chính xác (-a) lần, dịch bằng ((-1,-1)). 

Bốn lệnh trong`1142`không phải là một thủ thuật tùy tiện. Chúng là liên hợp của bản dịch cơ bản`14`bằng một phép quay (90^\circ) nên độ dịch chuyển của nó là vectơ quay ((1,-1)), cụ thể là ((1,1)). 

1. Nếu (b>0), nối thêm`14`chính xác (b) lần. Nếu (b<0), nối thêm`32`chính xác (-b) lần. Những bản dịch này đóng góp (b(1,-1)). 
2. Chuỗi lệnh thu được có độ dịch chuyển chính xác 

[ 
a(1,1)+b(1,-1)=(dx,dy), 
] 

vì vậy robot kết thúc ở ((x_2,y_2)). In chiều dài và chuỗi của nó. 

### Tại sao nó hoạt động 

Bất biến trung tâm là tính chẵn lẻ của (x+y). Mỗi lệnh riêng lẻ đều bảo tồn nó, chứng tỏ rằng các lớp chẵn lẻ khác nhau không bao giờ có thể giao tiếp với nhau. Khi chẵn lẻ trùng khớp, (dx) và (dy) có cùng chẵn lẻ, vì vậy (a) và (b) là số nguyên. Các chuỗi lệnh`14`,`32`,`1142`, Và`1322`thực hiện các bản dịch lần lượt theo ((1,-1)), ((-1,1)), ((1,1)) và ((-1,-1)). Sự kết hợp của chúng tạo ra mọi chuyển vị có hai tọa độ có cùng độ chẵn lẻ. Do đó, thuật toán thành công chính xác đối với các cặp điểm có thể tiếp cận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x1, y1 = map(int, input().split())
    x2, y2 = map(int, input().split())

    dx = x2 - x1
    dy = y2 - y1

    # Every operation preserves the parity of x + y.
    if (dx + dy) & 1:
        print(-1)
        return

    a = (dx + dy) // 2
    b = (dx - dy) // 2

    ans = []

    if a > 0:
        ans.append("1142" * a)
    elif a < 0:
        ans.append("1322" * (-a))

    if b > 0:
        ans.append("14" * b)
    elif b < 0:
        ans.append("32" * (-b))

    s = "".join(ans)

    print(len(s))
    print(s)

if __name__ == "__main__":
    solve()
```Kiểm tra tính chẵn lẻ đầu tiên tương ứng trực tiếp với bất biến khả năng tiếp cận. sử dụng`(dx + dy) & 1`cũng an toàn cho các số nguyên Python âm, vì nó kiểm tra tính chẵn lẻ của số nguyên mà không dựa vào số học dấu phẩy động. 

Các hệ số chỉ được tính bằng phép chia số nguyên sau khi kiểm tra tính chẵn lẻ đã xác định rằng cả hai tử số đều là số chẵn. Không có làm tròn có liên quan. 

Đối với tích cực (a),`1142`được lặp lại vì nó dịch theo ((1,1)). Đối với âm (a),`1322`là nghịch đảo của nó. Tương tự như vậy,`14`Và`32`là các bản dịch nghịch đảo dọc theo hướng ((1,-1)). 

Câu trả lời được tập hợp dưới dạng chuỗi thay vì nối thêm các ký tự riêng lẻ bên trong các phép toán Python lồng nhau. Câu trả lời lớn nhất là dưới (10^6) ký tự, vì vậy đây là mức thoải mái trong giới hạn bộ nhớ bình thường. 

Thứ tự của hai nhóm dịch không quan trọng vì các bản dịch có tính chất đi lại. Việc áp dụng tất cả các phép dịch chéo trước và tất cả các phép dịch ngược đường chéo thứ hai sẽ cho độ dịch chuyển cuối cùng giống hệt như bất kỳ thứ tự nào khác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên bắt đầu tại ((0,1)) và muốn đạt ((1,-2)). 

Sự dịch chuyển và sự phân hủy của nó là: 

| Biến | Giá trị | 
| --- | --- | 
| (dx) | (1) | 
| (dy) | (-3) | 
| (dx+dy) | (-2) | 
| (a=(dx+dy)/2) | (-1) | 
| (b=(dx-dy)/2) | (2) | 

Vì (a=-1), thuật toán phát ra`1322`, di chuyển theo ((-1,-1)). Vì (b=2) nên nó phát ra`1414`, di chuyển theo ((2,-2)). 

Tổng chuyển vị là 

[ 
(-1,-1)+(2,-2)=(1,-3), 
] 

vậy 

[ 
(0,1)+(1,-3)=(1,-2). 
] 

Do đó, thuật toán đưa ra một chuỗi tám lệnh hợp lệ. Mẫu chính thức tình cờ sử dụng trình tự ngắn hơn nhiều`24`, nhưng việc giảm thiểu độ dài là không cần thiết. Sự cố chấp nhận bất kỳ chuỗi lệnh hợp lệ nào có tối đa (10^6). 

### Mẫu 2 

Mẫu thứ hai ban đầu là```
0 1
1 1
```Độ dịch chuyển là ((1,0)). 

| Biến | Giá trị | 
| --- | --- | 
| (dx) | (1) | 
| (dy) | (0) | 
| (dx+dy) | (1) | 
| kiểm tra tính chẵn lẻ | thất bại | 
| kết quả |`-1`| 

Hai tọa độ dịch chuyển có độ chẵn lẻ khác nhau nên không có cặp số nguyên (a,b) thỏa mãn 

[ 
(dx,dy)=a(1,1)+b(1,-1). 
] 

Về cơ bản hơn, điểm bắt đầu có (x+y=1), trong khi đích đến có (x+y=2). Vì mọi lệnh đều duy trì tính chẵn lẻ đó nên không thể truy cập được mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O( | a | + | b | )) | Mỗi lệnh được tạo sẽ được thêm một lần, do đó công việc sẽ tuyến tính theo độ dài đầu ra. | 
| Không gian | (O( | a | + | b | )) | Bản thân chuỗi lệnh yêu cầu không gian tuyến tính. | 

Bởi vì mỗi tọa độ khác nhau tối đa (200000), cả hai hệ số đều có giá trị tuyệt đối nhiều nhất (200000). Cấu trúc sử dụng bốn lệnh cho mỗi đơn vị của (a) và hai lệnh cho mỗi đơn vị của (b), đưa ra tối đa (800000) lệnh. Giá trị này nằm dưới giới hạn bắt buộc (10^6) và cả thời gian chạy cũng như mức sử dụng bộ nhớ đều có thể quản lý dễ dàng. 

## Trường hợp thử nghiệm 

Vì một câu trả lời hợp lệ không phải là duy nhất nên các bài kiểm tra nên xác minh chuỗi lệnh được tạo ra thay vì so sánh chuỗi lệnh chính xác. Trình trợ giúp bên dưới mô phỏng độc lập tất cả bốn phép biến đổi và kiểm tra xem vị trí cuối cùng có chính xác hay không. Nó cũng kiểm tra giới hạn lệnh (10^6).```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        x1, y1 = map(int, input().split())
        x2, y2 = map(int, input().split())

        dx = x2 - x1
        dy = y2 - y1

        if (dx + dy) & 1:
            return "-1\n"

        a = (dx + dy) // 2
        b = (dx - dy) // 2

        ans = []

        if a > 0:
            ans.append("1142" * a)
        elif a < 0:
            ans.append("1322" * (-a))

        if b > 0:
            ans.append("14" * b)
        elif b < 0:
            ans.append("32" * (-b))

        s = "".join(ans)
        return f"{len(s)}\n{s}\n"
    finally:
        sys.stdin = old_stdin

def run(inp: str) -> str:
    return solve_data(inp)

def verify(inp: str):
    x1, y1 = map(int, inp.splitlines()[0].split())
    x2, y2 = map(int, inp.splitlines()[1].split())

    out = run(inp).strip().splitlines()

    if out[0] == "-1":
        return False, "reported impossible"

    k = int(out[0])
    s = out[1]

    assert k == len(s)
    assert 0 < k <= 10**6
    assert all(c in "1234" for c in s)

    x, y = x1, y1

    for c in s:
        if c == "1":
            x, y = y, -x
        elif c == "2":
            x, y = -y, x
        elif c == "3":
            x, y = y + 1, 1 - x
        else:
            x, y = 1 - y, x - 1

    return (x, y) == (x2, y2), (x, y)

# Provided sample 1. Any valid sequence is accepted.
ok, _ = verify("0 1\n1 -2\n")
assert ok, "sample 1"

# Provided sample 2 from the original statement.
assert run("0 1\n1 1\n").strip() == "-1", "sample 2"

# Minimum-size displacement that is reachable.
ok, _ = verify("0 0\n1 1\n")
assert ok, "unit diagonal translation"

# Negative diagonal displacement.
ok, _ = verify("0 0\n-1 -1\n")
assert ok, "negative diagonal translation"

# Boundary-sized reachable displacement.
ok, _ = verify("-100000 -100000\n100000 100000\n")
assert ok, "maximum diagonal displacement"

# Boundary-sized unreachable displacement.
assert run("-100000 -100000\n100000 99999\n").strip() == "-1", \
    "boundary parity case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 1 / 1 -2`| Bất kỳ chuỗi hợp lệ nào | Mẫu có thể tiếp cận chính thức và dịch chuyển hỗn hợp không cần thiết | 
|`0 1 / 1 1`|`-1`| Bất biến chẵn lẻ | 
|`0 0 / 1 1`| Bất kỳ chuỗi hợp lệ nào | Một bản dịch chéo tích cực | 
|`0 0 / -1 -1`| Bất kỳ chuỗi hợp lệ nào | Dịch chéo ngược | 
|`-100000 -100000 / 100000 100000`| Bất kỳ chuỗi hợp lệ nào | Chênh lệch tọa độ tối đa và giới hạn lệnh | 
|`-100000 -100000 / 100000 99999`|`-1`| Kiểm tra tính chẵn lẻ ranh giới | 

## Vỏ cạnh 

Trường hợp không rõ ràng đầu tiên là một điểm không thể tiếp cận được do tính chẵn lẻ. Vì```
0 1
1 1
```chúng ta có (dx=1) và (dy=0), vì vậy (dx+dy=1) là số lẻ. Thuật toán dừng ngay lập tức và in`-1`. Điều này tốt hơn là cố gắng xây dựng một chuỗi và chỉ phát hiện ra lỗi sau một thời gian dài tìm kiếm. 

Trường hợp thứ hai là sự dịch chuyển hoàn toàn theo một hướng được tạo ra. Vì```
0 0
1 1
```chúng ta nhận được (a=1) và (b=0). Thuật toán phát ra chính xác`1142`. Áp dụng bốn lệnh sẽ mang lại 

[ 
(0,0)\xrightarrow{1}(0,0) 
\xrightarrow{1}(0,0) 
\xrightarrow{4}(1,-1) 
\xrightarrow{2}(1,1). 
] 

Hai vòng quay đầu tiên bị hủy vì robot bắt đầu ở tháp đầu tiên và chuỗi hoàn chỉnh vẫn thực hiện bản dịch được yêu cầu. 

Hệ số âm được xử lý bằng cách sử dụng chuỗi nghịch đảo. Vì```
0 0
-1 -1
```chúng ta nhận được (a=-1), do đó thuật toán phát ra`1322`. Độ dịch chuyển ròng của nó là ((-1,-1)), lấy điểm gốc trực tiếp đến đích. 

Sự dịch chuyển lớn nhất có thể tiếp cận cũng an toàn. Vì```
-100000 -100000
100000 100000
```chúng ta có (dx=dy=200000), do đó (a=200000) và (b=0). Đầu ra chứa các lệnh (4\cdot200000=800000), dưới giới hạn (10^6). Đây là trường hợp xấu nhất đối với quy mô đầu ra của công trình. 

Cuối cùng, hệ số 0 không được khiến thuật toán in ra chuỗi lệnh trống. Ví dụ, nếu chỉ (b) khác 0 thì khối dịch chéo sẽ bị bỏ qua và`14`hoặc`32`khối cung cấp toàn bộ chuyển vị. Vì các điểm ban đầu được đảm bảo khác nhau nên ít nhất một trong (a) và (b) khác 0, nên câu trả lời cuối cùng luôn là chuỗi lệnh có độ dài dương bất cứ khi nào có thể truy cập được mục tiêu.

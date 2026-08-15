---
title: "CF 102437J - Robot giao hàng"
description: "Robot di chuyển trong mặt phẳng nguyên và có hai tâm quay cố định tại ((0,0)) và ((1,0)). Mỗi lệnh xoay vị trí hiện tại một cách chính xác (90^circ), theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ, xung quanh một trong hai tâm này."
date: "2026-08-15T09:27:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "J"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 391
verified: false
draft: false
---

[CF 102437J - Robot giao hàng](https://codeforces.com/problemset/problem/102437/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 31 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Robot di chuyển trong mặt phẳng nguyên và có hai tâm quay cố định tại ((0,0)) và ((1,0)). Mỗi lệnh xoay vị trí hiện tại một cách chính xác (90^\circ), theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ, xung quanh một trong hai tâm này. Do đó, mọi lệnh đều là một phép quay cố định và nhiệm vụ là tìm một chuỗi gồm nhiều nhất (10^6) phép quay như vậy để di chuyển robot từ ((x_1,y_1)) đến ((x_2,y_2)) hoặc chứng minh rằng không tồn tại chuỗi như vậy. 

Bốn lệnh có thể được viết trực tiếp dưới dạng phép biến đổi tọa độ: 

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

Sự cố chính thức có giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. Các tọa độ được giới hạn bởi (100000), do đó, một thuật toán tỷ lệ với phạm vi tọa độ là dễ dàng khả thi, trong khi mọi thứ bậc hai có kích thước của bình phương tọa độ đều quá lớn. Một công trình chỉ sử dụng vài trăm nghìn lệnh cũng nằm trong giới hạn (10^6). 

Vấn đề không rõ ràng đầu tiên là không phải mọi điểm đều có thể truy cập được. Coi như```
0 1
1 1
```Giá trị ban đầu của (x+y) là (1), trong khi mục tiêu có (x+y=2). Đầu ra đúng là`-1`. Một tìm kiếm bất cẩn chỉ kiểm tra khoảng cách hình học hoặc giả sử hai tâm quay tạo ra toàn bộ mặt phẳng nguyên có thể cho rằng điểm có thể truy cập được một cách không chính xác. 

Vấn đề thứ hai là câu trả lời có thể yêu cầu nhiều lệnh mặc dù bản thân việc xây dựng rất đơn giản. Ví dụ,```
-100000 -100000
100000 100000
```có thể truy cập được, nhưng độ dịch chuyển là (200000) ở cả hai tọa độ. Một công trình di chuyển từng đơn vị một có thể cần hàng trăm nghìn thao tác, do đó việc triển khai phải xây dựng câu trả lời một cách hiệu quả thay vì thực hiện nhiều lần các tìm kiếm tốn kém. 

Ngoài ra còn có một trường hợp biên nhỏ trong đó độ dịch chuyển chính xác là đường chéo:```
0 0
1 1
```Trình tự đúng là`23`. Việc thiếu thứ tự của hai lệnh này sẽ tạo ra một phép biến đổi khác, do đó việc coi các lệnh là các thao tác không có thứ tự là không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là coi mọi điểm nguyên là một đỉnh của đồ thị và kết nối mỗi điểm với bốn điểm thu được bằng một lệnh. Tìm kiếm theo chiều rộng là chính xác vì mọi lệnh đều có đơn vị giá trị, do đó, lần đầu tiên BFS đạt được mục tiêu, nó đã tìm thấy một chuỗi hợp lệ. Vấn đề là kích thước của biểu đồ. Ngay cả khi chúng ta hạn chế sự chú ý vào ô vuông (200001\times200001) chứa tất cả các tọa độ đầu vào được phép, thì vẫn có (40.000.400.001) điểm có thể và việc kiểm tra bốn lần chuyển đổi trên mỗi điểm có nghĩa là có tới (160.001.600.004) chuyển đổi lân cận. Một BFS thực sự không bị hạn chế không có hộp tọa độ hữu hạn nào để dựa vào. Việc liệt kê các chuỗi lệnh thay vào đó thậm chí còn tệ hơn, vì độ sâu (k) chứa các chuỗi (4^k). 

Brute-force hoạt động vì mọi lệnh đều dễ mô phỏng nhưng nó hoàn toàn bỏ qua cấu trúc đại số của các phép biến đổi. Quan sát quan trọng là hai lệnh có thể hủy phép quay của chúng trong khi vẫn để lại bản dịch thuần túy. 

Áp dụng lệnh 2 rồi đến lệnh 3. Bắt đầu từ ((x,y)), 

[ 
(x,y)\xrightarrow{2}(-y,x)\xrightarrow{3}(x+1,y+1). 
]

Vì thế`23`dịch mọi điểm theo ((1,1)). 

Tương tự, 

[ 
(x,y)\xrightarrow{1}(y,-x)\xrightarrow{4}(x+1,y-1), 
] 

vậy`14`dịch mọi điểm theo ((1,-1)). 

nghịch đảo của chúng là`41`, dịch theo ((-1,-1)) và`32`, được dịch bởi ((-1,1)). 

Điều này làm giảm toàn bộ vấn đề về việc biểu diễn độ dịch chuyển mong muốn dưới dạng kết hợp của hai vectơ đường chéo ((1,1)) và ((1,-1)). hãy để 

[ 
dx=x_2-x_1,\qquad dy=y_2-y_1. 
] 

Ta cần các số nguyên (a,b) thỏa mãn 

[ 
a(1,1)+b(1,-1)=(dx,dy). 
] 

Giải hai phương trình ta có 

[ 
a=\frac{dx+dy}{2},\qquad b=\frac{dx-dy}{2}. 
] 

Những số nguyên như vậy tồn tại chính xác khi (dx) và (dy) có cùng tính chẵn lẻ. Tương đương, (x_1+y_1) và (x_2+y_2) phải có cùng tính chẵn lẻ. 

Việc xây dựng cần các lệnh (2|a|+2|b|). sử dụng 

[ 
|dx+dy|+|dx-dy|=2\max(|dx|,|dy|), 
] 

số lượng lệnh nhiều nhất là (400000), vì mọi chênh lệch tọa độ đều có giá trị tuyệt đối nhiều nhất (200000). Đây là mức an toàn dưới mức yêu cầu (10^6). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS vũ phu | (O(R^2)) trạng thái trong hộp tọa độ bán kính (R) | (O(R^2)) | Quá chậm | 
| Xây dựng tối ưu | (O( | dx | + | dy | )) để xây dựng đầu ra | (O( | dx | + | dy | )) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính chuyển vị 

[ 
dx=x_2-x_1,\qquad dy=y_2-y_1. 
] 

Toàn bộ công trình chỉ cần tái tạo lại sự dịch chuyển này, bởi vì chuỗi hai lệnh`23`Và`14`là những bản dịch hoạt động từ mọi điểm. 

1. Kiểm tra xem (dx) và (dy) có cùng tính chẵn lẻ hay không. Nếu không, hãy in`-1`. 

Mọi lệnh đều bảo toàn tính chẵn lẻ của (x+y). Một phép quay quanh điểm gốc thay đổi ((x,y)) thành ((-y,x)) hoặc ((y,-x)), có tổng tọa độ có cùng tính chẵn lẻ với (x+y). Xoay quanh ((1,0)) có cùng tính chất. Do đó, các số chẵn lẻ khác nhau của (x+y) không bao giờ có thể được kết nối. 

1. Tính toán 

[ 
a=\frac{dx+dy}{2},\qquad b=\frac{dx-dy}{2}. 
] 

Hệ số đầu tiên cho chúng ta biết số lần sử dụng bản dịch ((1,1)), trong khi hệ số thứ hai cho chúng ta biết số lần sử dụng ((1,-1)). 

1. Nếu (a>0), nối thêm`23`chính xác (a) lần. Nếu (a<0), nối thêm`41`chính xác (-a) lần.`23`thêm ((1,1)), trong khi`41`thêm nghịch đảo của nó ((-1,-1)). 

1. Nếu (b>0), nối thêm`14`chính xác (b) lần. Nếu (b<0), nối thêm`32`chính xác (-b) lần.`14`thêm ((1,-1)), trong khi`32`thêm ((-1,1)). 

Thứ tự của các khối dịch này không quan trọng vì các bản dịch được chuyển qua lại. Việc nhóm các bản dịch giống nhau lại với nhau cũng làm cho việc xây dựng trở nên dễ thực hiện. 

1. Xuất ra độ dài của chuỗi được tạo và chính chuỗi đó. 

Nguồn và đích được đảm bảo là khác nhau, do đó, bất cứ khi nào có thể truy cập được mục tiêu thì ít nhất một trong (a,b) khác 0 và số lượng lệnh thu được là dương. 

### Tại sao nó hoạt động 

Việc xây dựng duy trì tính bất biến rằng vị trí hiện tại bằng vị trí ban đầu cộng với tổng của tất cả các bản dịch được tạo cho đến nay. Mỗi`23`đóng góp ((1,1)), mỗi`41`đóng góp ((-1,-1)), mỗi`14`đóng góp ((1,-1)), và mỗi`32`đóng góp ((-1,1)). Do đó độ dịch chuyển cuối cùng là 

[ 
a(1,1)+b(1,-1) 
=(a+b,a-b) 
=(dx,dy), 
] 

nên vị trí cuối cùng chính xác là ((x_2,y_2)). 

Nếu kiểm tra tính chẵn lẻ thất bại thì không có giải pháp nào tồn tại vì mọi lệnh hợp pháp đều được giữ nguyên (x+y\bmod 2). Nếu nó vượt qua, (a) và (b) là các số nguyên và việc xây dựng tạo ra mục tiêu một cách rõ ràng, do đó điều kiện chẵn lẻ cũng đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x1, y1 = map(int, input().split())
    x2, y2 = map(int, input().split())

    dx = x2 - x1
    dy = y2 - y1

    # The parity of x + y is invariant.
    if (dx - dy) % 2 != 0:
        print(-1)
        return

    a = (dx + dy) // 2
    b = (dx - dy) // 2

    ans = []

    # 23 = translation by (1, 1)
    # 41 = translation by (-1, -1)
    if a > 0:
        ans.append("23" * a)
    elif a < 0:
        ans.append("41" * (-a))

    # 14 = translation by (1, -1)
    # 32 = translation by (-1, 1)
    if b > 0:
        ans.append("14" * b)
    elif b < 0:
        ans.append("32" * (-b))

    result = ''.join(ans)

    print(len(result))
    print(result)

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính toán chuyển vị của mục tiêu thay vì thao tác điều khiển vị trí của robot bằng lệnh. Việc kiểm tra tính chẵn lẻ sử dụng`(dx - dy) % 2`, tương đương với việc kiểm tra xem (dx) và (dy) có tính chẵn lẻ bằng nhau hay không. Hoạt động modulo của Python an toàn đối với các giá trị âm, do đó, hoạt động này hoạt động với mọi tọa độ được phép. 

Hai hệ số chỉ là số nguyên sau khi kiểm tra tính chẵn lẻ thành công. Điều này tránh việc cắt bớt một cách âm thầm hệ số nửa số nguyên và tạo ra một chuỗi không hợp lệ. 

Các khối lệnh được nối dưới dạng chuỗi thay vì một ký tự mỗi lần. Điều này giúp việc triển khai đơn giản và tránh lưu trữ từng lệnh dưới dạng một đối tượng Python riêng biệt. Câu trả lời lớn nhất có (400000) ký tự, do đó chuỗi kết quả đủ nhỏ cho giới hạn bộ nhớ. 

Thứ tự lệnh bên trong mỗi cặp đều quan trọng.`23`phải có nghĩa là lệnh 2 theo sau là lệnh 3 và`14`phải có nghĩa là lệnh 1 theo sau là lệnh 4. Đảo ngược một trong hai cặp sẽ thay đổi bản dịch kết quả. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn mặc dù giới hạn tọa độ thực tế là nhỏ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
0 1
1 -2
```Độ dịch chuyển là (dx=1), (dy=-3). 

| Biến | Giá trị | 
| --- | --- | 
| (x_1) | 0 | 
| (y_1) | 1 | 
| (x_2) | 1 | 
| (y_2) | -2 | 
| (dx) | 1 | 
| (dy) | -3 | 
| (a=(dx+dy)/2) | -1 | 
| (b=(dx-dy)/2) | 2 | 

Vì (dx) và (dy) đều là số lẻ nên mục tiêu có thể đạt được. Hệ số (a=-1) đóng góp một`41`và (b=2) đóng góp hai bản sao của`14`. 

Trình tự kết quả là`411414`. 

| Các lệnh được thực thi | Vị trí hiện tại | 
| --- | --- | 
| không | ((0,1)) | 
|`41`| ((-1,0)) | 
| Đầu tiên`14`| ((0,-1)) | 
| thứ hai`14`| ((1,-2)) | 

Vị trí cuối cùng chính xác là mục tiêu được yêu cầu. Mẫu chính thức sử dụng trình tự ngắn hơn`24`, nhưng câu lệnh cho phép bất kỳ chuỗi có độ dài hợp lệ nào tối đa (10^6). 

### Mẫu 2 

Mẫu thứ hai thực tế trên tuyên bố chính thức là```
0 1
1 1
```Độ dịch chuyển là (dx=1), (dy=0). 

| Biến | Giá trị | 
| --- | --- | 
| (x_1) | 0 | 
| (y_1) | 1 | 
| (x_2) | 1 | 
| (y_2) | 1 | 
| (dx) | 1 | 
| (dy) | 0 | 
| (dx\bmod2) | 1 | 
| (dy\bmod2) | 0 | 

Hai thành phần chuyển vị có tính chẵn lẻ khác nhau, vì vậy (a) và (b) đều là nửa số nguyên. Về cơ bản hơn, điểm ban đầu có (x+y=1), trong khi mục tiêu có (x+y=2). Vì mọi lệnh đều bảo toàn tính chẵn lẻ này nên không thể đạt được mục tiêu. 

Thuật toán in ngay lập tức```
-1
```phù hợp với mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O( | dx | + | dy | )) | Bản thân đầu ra chứa tối đa (400000) ký tự và việc xây dựng nó cần có thời gian tuyến tính theo độ dài của nó. | 
| Không gian | (O( | dx | + | dy | )) | Chuỗi lệnh được lưu trữ trước khi in và có tối đa (400000) ký tự. | 

Vì (|dx|,|dy|\le200000), chuỗi được tạo có tối đa (400000) lệnh. Do đó, việc xây dựng nằm dưới mức yêu cầu (10^6) lệnh một cách thoải mái và tránh được không gian trạng thái khổng lồ của tìm kiếm. 

## Trường hợp thử nghiệm 

Đầu ra của một vấn đề mang tính xây dựng không phải là duy nhất, vì vậy các thử nghiệm bên dưới so sánh đầu ra xác định được tạo ra bởi việc triển khai này. Đối với kết quả đầu ra lớn hơn, chuỗi dự kiến ​​được tạo từ cùng một cấu trúc toán học thay vì được viết ra theo nghĩa đen.```python
import sys
import io

def solve():
    x1, y1 = map(int, input().split())
    x2, y2 = map(int, input().split())

    dx = x2 - x1
    dy = y2 - y1

    if (dx - dy) % 2 != 0:
        print(-1)
        return

    a = (dx + dy) // 2
    b = (dx - dy) // 2

    ans = []

    if a > 0:
        ans.append("23" * a)
    elif a < 0:
        ans.append("41" * (-a))

    if b > 0:
        ans.append("14" * b)
    elif b < 0:
        ans.append("32" * (-b))

    result = ''.join(ans)

    print(len(result))
    print(result)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1.
assert run("0 1\n1 -2\n") == "6\n411414\n", "sample 1"

# Provided sample 2.
assert run("0 1\n1 1\n") == "-1\n", "sample 2"

# Minimum-size displacement: (0, 0) -> (1, 1).
assert run("0 0\n1 1\n") == "2\n23\n", "diagonal +1"

# The other diagonal direction: (0, 0) -> (1, -1).
assert run("0 0\n1 -1\n") == "2\n14\n", "anti-diagonal +1"

# Equal source coordinates, also checks a larger diagonal displacement.
assert run("5 5\n7 7\n") == "4\n2323\n", "equal coordinates"

# Maximum coordinate range in both dimensions.
expected = "23" * 200000
assert run("-100000 -100000\n100000 100000\n") == (
    str(len(expected)) + "\n" + expected + "\n"
), "maximum-size reachable case"

# Boundary parity mismatch.
assert run("100000 100000\n99999 100000\n") == "-1\n", (
    "boundary parity mismatch"
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 / 1 1`|`2 / 23`| Thứ tự dịch và lệnh theo đường chéo dương nhỏ nhất | 
|`0 0 / 1 -1`|`2 / 14`| Hướng dịch thứ hai | 
|`5 5 / 7 7`|`4 / 2323`| Tọa độ nguồn bằng nhau và dịch chéo lặp đi lặp lại | 
|`-100000 -100000 / 100000 100000`|`400000 / 23...23`| Chuyển vị tối đa và giới hạn số lượng lệnh | 
|`100000 100000 / 99999 100000`|`-1`| Trường hợp ranh giới có tính chẵn lẻ khác nhau | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là trường hợp không khớp chẵn lẻ không thể truy cập được. Vì```
0 1
1 1
```chúng ta có (dx=1) và (dy=0), do đó việc kiểm tra tính chẵn lẻ thất bại ngay lập tức. Thuật toán không cố gắng chia cho hai hoặc xây dựng các lệnh. Điều này ngăn ngừa một lỗi phổ biến khi phép chia số nguyên sẽ biến các hệ số bán nguyên bắt buộc thành 0 hoặc một số nguyên không chính xác khác. 

Trường hợp cạnh thứ hai là độ dịch chuyển đường chéo đơn vị:```
0 0
1 1
```Ở đây (a=1) và (b=0). Thuật toán phát ra chính xác`23`. Lệnh 2 thay đổi ((0,0)) thành ((0,0)), sau đó lệnh 3 thay đổi thành ((1,1)). Cùng một cặp hoạt động từ mọi điểm bắt đầu, đó là lý do tại sao việc xây dựng không cần xử lý đặc biệt đối với robot được đặt tại một tòa tháp. 

Trường hợp cạnh thứ ba là chuyển động theo hướng chéo ngược lại:```
0 0
1 -1
```Ở đây (a=0) và (b=1), do đó thuật toán phát ra`14`. Lệnh 1 gửi ((0,0)) đến ((0,0)) và lệnh 4 gửi nó tới ((1,-1)). Điều này xác nhận rằng hai phép tịnh tiến chéo thực sự tạo thành cơ sở cho mọi chuyển vị có thể tiếp cận được. 

Trường hợp cạnh thứ tư là độ dịch chuyển tối đa có thể:```
-100000 -100000
100000 100000
```Các hệ số là (a=200000) và (b=0). Thuật toán tạo ra`23`lặp lại (200000) lần, đưa ra lệnh (400000). Độ dài chuỗi vẫn ở mức dưới (10^6) và không có giá trị tọa độ hoặc số học nào đạt đến phạm vi số nguyên nguy hiểm. 

Trường hợp cạnh cuối cùng là mục tiêu có giới hạn tọa độ chính xác nhưng tính chẵn lẻ sai:```
100000 100000
99999 100000
```Độ dịch chuyển là ((-1,0)), có các thành phần có tính chẵn lẻ khác nhau. Thuật toán in`-1`. Điều này chứng tỏ rằng việc ở liền kề trong mặt phẳng không hàm ý khả năng tiếp cận. Bất biến (x+y\bmod2) là vật cản thực tế và việc xây dựng đạt đến mọi điểm thỏa mãn nó.

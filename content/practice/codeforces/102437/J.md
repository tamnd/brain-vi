---
title: "CF 102437J - Robot giao hàng"
description: "Robot di chuyển trên lưới số nguyên. Có hai tâm quay cố định, tâm thứ nhất ở (0,0) và tâm thứ hai ở (1,0). Mỗi lệnh xoay vị trí hiện tại chính xác 90 ∘, theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ, xung quanh một trong hai tâm đó."
date: "2026-08-12T08:03:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "J"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 228
verified: false
draft: false
---

[CF 102437J - Robot giao hàng](https://codeforces.com/problemset/problem/102437/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Robot di chuyển trên lưới số nguyên. Có hai tâm quay cố định, tâm thứ nhất ở (0,0) và tâm thứ hai ở (1,0). Mỗi lệnh xoay vị trí hiện tại chính xác 90 ∘, theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ, xung quanh một trong hai tâm đó. 

Bốn lệnh có thể được viết trực tiếp dưới dạng phép biến đổi tọa độ. Xung quanh (0,0), lệnh 1 và 2 là các phép quay theo chiều kim đồng hồ và ngược chiều kim đồng hồ. Xung quanh (1,0), lệnh 3 và 4 là các phép quay tương ứng. Chúng ta bắt đầu tại (x 1​ ,y 1​ ) và cần đạt tới (x 2 ​ ,y 2 ​ ), tạo ra tối đa 10 6 lệnh hoặc báo cáo rằng mục tiêu không thể truy cập được. 

Tất cả các tọa độ đều có giá trị tuyệt đối nhiều nhất là 100000. Giới hạn đầu ra lớn hơn nhiều so với phạm vi tọa độ, do đó, việc xây dựng sử dụng số lượng thao tác không đổi trên một đơn vị chuyển vị dễ dàng đủ nhanh. Những gì chúng ta không thể thực hiện được là tìm kiếm thông qua một số chuỗi lệnh theo cấp số nhân hoặc khám phá một vùng có kích thước bậc hai của mặt phẳng với bán kính lớn. 

Vấn đề không rõ ràng đầu tiên là khả năng tiếp cận. Mọi phép quay được phép đều bảo toàn tính chẵn lẻ của x+y. Ví dụ: từ (0,0), quay quanh một trong hai tháp luôn tạo ra một điểm có tổng tọa độ chẵn. Như vậy (0,0) không thể đạt tới (1,0), vì tổng của chúng có tính chẵn lẻ khác nhau. Việc xây dựng bất cẩn chỉ kiểm tra xem sự khác biệt về tọa độ có phải là số nguyên hay không sẽ cho rằng mục tiêu này có thể đạt được một cách không chính xác. 

Vấn đề thứ hai là hai tòa tháp không tạo ra các bản dịch đơn vị tùy ý một cách trực tiếp. Các bản dịch hữu ích có hướng chéo. Ví dụ, các lệnh`23`di chuyển mọi điểm theo (1,1), trong khi các lệnh`14`di chuyển mọi điểm theo (1,−1). Do đó, các hệ số của hai bản dịch này phải là số nguyên. Đây chính xác là điều kiện chẵn lẻ, vì việc giải 

a(1,1)+b(1,−1)=(dx,dy) 

cho 

a= 2 dx+dy ​ ,b= 2 dx−dy ​ . 

Ví dụ, đầu vào```
0 0
1 0
```phải sản xuất`-1`, bởi vì các số chẵn lẻ của tổng tọa độ là khác nhau. Một phương pháp cố gắng cố định x và y một cách độc lập bằng cách di chuyển một đơn vị sẽ âm thầm xây dựng một chuỗi thao tác không tồn tại. 

Nguồn và đích được đảm bảo khác nhau. Vì vậy chúng ta không bao giờ cần xuất ra lệnh 0. Một trường hợp như```
5 5
5 -5
```là hợp lệ và có thể truy cập được vì cả hai tổng tọa độ đều là số chẵn. Nó cũng nắm bắt các triển khai giả định cả hai tọa độ đều phải thay đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là liệt kê các chuỗi lệnh và mô phỏng robot cho đến khi tìm thấy mục tiêu. Điều này đúng vì mọi trình tự có thể đều được xem xét, nhưng nó gần như trở nên vô dụng ngay lập tức. Nếu chúng ta liệt kê tất cả các chuỗi có độ dài tối đa là k thì số lượng các chuỗi là 

1+4+4 2 +⋯+4 k = 3 4 k+1 −1 ​ . 

Ở giới hạn cho phép k=10 6, giá trị này xấp xỉ 4 10 6/3, vượt xa mọi tính toán khả thi. Ngay cả việc chỉ kiểm tra chuỗi vài chục lệnh cũng là không thực tế. 

BFS trên các vị trí sẽ tốt hơn vì nhiều chuỗi lệnh khác nhau có thể đạt đến cùng một vị trí. Nó sẽ khám phá chính xác thành phần có thể truy cập, nhưng nó vẫn không có lý do gì để dừng lại sau một số ít trạng thái. Các tọa độ mục tiêu có thể cách nhau 100000 đơn vị, do đó, tìm kiếm dạng lưới chung có thể truy cập vào một khu vực rộng lớn trước khi tìm được đường đi phù hợp. 

Quan sát quan trọng là hai lệnh có thể được ghép nối thành một bản dịch thuần túy. Áp dụng lệnh 2, xoay ngược chiều kim đồng hồ quanh (0,0), tiếp theo là lệnh 3, xoay theo chiều kim đồng hồ quanh (1,0). Đối với một điểm (x,y), lệnh 2 cho 

(−y,x), 

và lệnh 3 sau đó đưa ra 

(1+x,1+y). 

Như vậy`23`dịch chính xác mọi điểm (1,1). 

Tương tự, lệnh 1 theo sau là lệnh 4 sẽ cho 

(x,y)→(y,−x)→(x+1,y−1), 

vậy`14`dịch mọi điểm theo (1,−1). 

Lực lượng vũ phu hoạt động vì bốn lệnh mô tả biểu đồ chuyển tiếp hoàn chỉnh. Nó thất bại vì biểu đồ đó rất lớn. Quan sát cho thấy sự kết hợp hai lệnh là bản dịch cho phép chúng ta thay thế tìm kiếm đồ thị bằng việc giải phương trình tuyến tính hai biến. 

hãy để 

dx=x 2 ​ −x 1 ​ ,dy=y 2 ​ −y 1 ​ . 

chúng tôi muốn 

dx=a+b,dy=a−b, 

trong đó sử dụng bản sao dịch (1,1) và b bản dịch (1,−1). Điều này mang lại 

a= 2 dx+dy ​ ,b= 2 dx−dy ​ . 

Các giá trị này là số nguyên chính xác khi dx và dy có cùng tính chẵn lẻ, tương đương với điểm bắt đầu và điểm kết thúc có cùng tính chẵn lẻ là x+y. 

Hệ số âm không gây khó khăn gì. Nghịch đảo của`23`là`41`, Vì thế`41`dịch theo (−1,−1). Nghịch đảo của`14`là`32`, Vì thế`32`dịch theo (−1,1). 

Số lượng lệnh chỉ 

2(∣a∣+∣b∣). 

sử dụng 

∣dx+dy∣+∣dx−dy∣=2max(∣dx∣,∣dy∣), 

chúng tôi nhận được 

2(∣a∣+∣b∣)=2max(∣dx∣,∣dy∣)≤400000. 

Vì vậy, việc xây dựng thoải mái dưới giới hạn 10 6. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4 k ) cho độ sâu k | O(k) cho DFS | Quá chậm | 
| Tối ưu | (O( | dx | + | 

## Hướng dẫn thuật toán 

1. Tính chuyển vị 

dx=x 2 ​ −x 1 ​ ,dy=y 2 ​ −y 1 ​ . 

Toàn bộ vấn đề bây giờ có thể được coi là xây dựng sự dịch chuyển này bằng cách sử dụng hai phép tịnh tiến theo đường chéo. 
2. Kiểm tra xem x 1 ​ +y 1 ​ và x 2 ​ +y 2 ​ có cùng tính chẵn lẻ hay không. Tương tự, hãy kiểm tra xem cả dx+dy và dx−dy có chẵn hay không. Nếu không thì xuất`-1`. 

Đây không chỉ đơn thuần là một tài sản xây dựng của chúng tôi. Mọi phép quay được phép đều bảo toàn tính chẵn lẻ của x+y, do đó tính chẵn lẻ khác nhau có nghĩa là mục tiêu thực sự không thể truy cập được. 
3. Tính toán 

a= 2 dx+dy ​ ,b= 2 dx−dy ​ . 

Giá trị a cho chúng ta biết số lần áp dụng dịch (1,1), trong khi b cho chúng ta biết số lần áp dụng dịch (1,−1). 
4. Nếu a>0, nối thêm`23`đúng một lần. Nếu a<0, nối thêm`41`chính xác −a lần.`23`dịch theo (1,1) và`41`là nghịch đảo của nó, vì vậy nó dịch theo (−1,−1). 
5. Nếu b>0, nối thêm`14`đúng b lần. Nếu b<0, nối thêm`32`chính xác −b lần.`14`dịch theo (1,−1), trong khi`32`dịch theo nghịch đảo của nó (−1,1). 
6. Xuất chuỗi lệnh kết quả. Vì vị trí ban đầu và vị trí đích khác nhau nên ít nhất một hệ số khác 0, do đó số lệnh là dương. 

### Tại sao nó hoạt động 

Bất biến là sự dịch chuyển được biểu thị bằng tiền tố lệnh được tạo. Mỗi cặp`23`đóng góp chính xác (1,1), mỗi cặp`41`đóng góp chính xác (−1,−1), mọi cặp`14`đóng góp chính xác (1,−1) và mọi cặp`32`đóng góp chính xác (−1,1). Các hệ số được chọn thỏa mãn 

a(1,1)+b(1,−1)=(dx,dy), 

như vậy sau khi thực hiện toàn bộ chuỗi, robot đã di chuyển từ (x 1​ ,y 1 ​ ) đúng độ dịch chuyển cần thiết và đạt tới (x 2​ ,y 2 ​ ). 

Nếu kiểm tra tính chẵn lẻ thất bại thì không có chuỗi nào có thể hoạt động vì mỗi phép quay riêng lẻ bảo toàn x+y modulo 2. Do đó, thuật toán sẽ loại bỏ chính xác các trường hợp không thể truy cập được. 

## Giải pháp Python 

Chỉnh sửa```python
import sys
input = sys.stdin.readline

def solve():
    x1, y1 = map(int, input().split())
    x2, y2 = map(int, input().split())

    dx = x2 - x1
    dy = y2 - y1

    # Every operation preserves (x + y) modulo 2.
    if ((x1 + y1) & 1) != ((x2 + y2) & 1):
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

    s = "".join(ans)

    print(len(s))
    print(s)

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc hai vị trí và chuyển đổi bài toán thành độ dịch chuyển (dx,dy). Làm việc với sự dịch chuyển đơn giản hơn việc cố gắng thao tác vị trí hiện tại sau mỗi lệnh riêng lẻ. 

Việc kiểm tra tính chẵn lẻ xảy ra trước khi chia. Nếu các số chẵn lẻ khác nhau, dx+dy là số lẻ, do đó hệ số yêu cầu a sẽ không phải là số nguyên. Về cơ bản hơn, mục tiêu là không thể tiếp cận được nên không nên cố gắng xây dựng. 

Các biểu thức cho`a`Và`b`chỉ sử dụng phép chia số nguyên sau khi điều kiện chẵn lẻ đã được thiết lập rằng cả hai tử số đều là số chẵn. Số nguyên Python cũng có độ chính xác tùy ý, mặc dù các giới hạn đã cho đủ nhỏ để số học số nguyên có chiều rộng cố định thông thường sẽ đủ trong các ngôn ngữ khác. 

Mỗi hệ số được chuyển đổi trực tiếp thành các bản dịch hai lệnh lặp lại. Thứ tự giữa hai họ dịch không quan trọng vì các bản dịch đi lại, do đó việc triển khai có thể tạo ra tất cả các bản sao của một loại, theo sau là tất cả các bản sao của loại kia. 

Chuỗi đầu ra được lưu trữ rõ ràng vì bài toán yêu cầu trình tự thực tế. Độ dài tối đa của nó là 400000, do đó, điều này chỉ sử dụng vài trăm kilobyte cộng với chi phí chuỗi Python thông thường. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
0 1
1 -2
```chúng ta có dx=1 và dy=−3. Các hệ số là 

a= 2 1−3 ​ =−1,b= 2 1+3 ​ =2. 

| Bước | một | b | Đã thêm lệnh | Chuyển vị hiện tại | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | -1 | 2 | trống | (0,0) | 
| Phủ định | -1 | 2 |`41`| (−1,−1) | 
| Tích cực b, đầu tiên | -1 | 2 |`14`| (0,−2) | 
| Tích cực b, giây | -1 | 2 |`14`| (1,−3) | 

Bắt đầu từ (0,1), vị trí cuối cùng là 

(0,1)+(1,−3)=(1,−2). 

Trình tự được tạo ra là`411414`, khác với mẫu`24`, nhưng cả hai đều hợp lệ. Dấu vết chứng tỏ rằng các hệ số dịch âm được xử lý bằng cách sử dụng các cặp lệnh nghịch đảo. 

Đối với mẫu 2,```
0 1
1 1
```tổng tọa độ là 1 và 2. 

| Bước | Bắt đầu tổng chẵn lẻ | Tổng mục tiêu chẵn lẻ | Quyết định | 
| --- | --- | --- | --- | 
| Kiểm tra tính chẵn lẻ | 1mod2=1 | 2mod2=0 | không thể truy cập | 

Thuật toán in ngay lập tức`-1`. Không có chuỗi lệnh nào có thể thay đổi tính chẵn lẻ của x+y, do đó sự bác bỏ này là chính xác chứ không phải là một hạn chế của cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O( | dx | 
| Không gian | (O( | dx | 

Với tọa độ được giới hạn bởi 100000, mỗi chênh lệch tọa độ tối đa là 200000. Chuỗi được xây dựng có tối đa 400000 ký tự, thấp hơn nhiều so với giới hạn lệnh 10 6. Thuật toán chỉ thực hiện công việc tuyến tính trong kích thước đầu ra và không khám phá lưới. 

## Trường hợp thử nghiệm 

Vì các đầu ra hợp lệ không phải là duy nhất nên trình trợ giúp kiểm tra bên dưới sẽ xác thực chuỗi lệnh được trả về bằng cách mô phỏng mọi lệnh. Điều này mạnh hơn so với việc so sánh với một chuỗi dự kiến cụ thể. 

Chỉnh sửa```python
import sys
import io

def solve():
    input = sys.stdin.readline

    x1, y1 = map(int, input().split())
    x2, y2 = map(int, input().split())

    dx = x2 - x1
    dy = y2 - y1

    if ((x1 + y1) & 1) != ((x2 + y2) & 1):
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

    s = "".join(ans)
    print(len(s))
    print(s)

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

def validate(inp: str, out: str):
    data = list(map(int, inp.split()))
    x1, y1, x2, y2 = data

    lines = out.strip().splitlines()

    if lines[0] == "-1":
        assert (x1 + y1) % 2 != (x2 + y2) % 2
        return

    k = int(lines[0])
    s = lines[1].strip()

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
            x, y = 1 + y, 1 - x
        else:
            x, y = 1 - y, x - 1

    assert (x, y) == (x2, y2)

# Sample 1
sample1 = "0 1\n1 -2\n"
out = run(sample1)
validate(sample1, out)

# Sample 2
sample2 = "0 1\n1 1\n"
out = run(sample2)
assert out.strip() == "-1"
validate(sample2, out)

# Minimum displacement, both coordinates change by one.
case1 = "0 0\n1 1\n"
out = run(case1)
validate(case1, out)

# Same x coordinate, negative y displacement.
case2 = "5 5\n5 -5\n"
out = run(case2)
validate(case2, out)

# Maximum coordinate difference in both dimensions.
case3 = "-100000 -100000\n100000 100000\n"
out = run(case3)
validate(case3, out)

# Boundary case with different parity, must be unreachable.
case4 = "-100000 100000\n100000 99999\n"
out = run(case4)
assert out.strip() == "-1"
validate(case4, out)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 / 1 1`| Một chuỗi hợp lệ có độ dài 2 | Dịch chuyển tối thiểu không cần thiết có thể tiếp cận | 
|`5 5 / 5 -5`| Một chuỗi hợp lệ | Một tọa độ không thay đổi và xử lý hệ số âm | 
|`-100000 -100000 / 100000 100000`| Một chuỗi hợp lệ có độ dài 400000 | Chuyển vị tối đa và giới hạn chiều dài đầu ra | 
|`-100000 100000 / 100000 99999`|`-1`| Tọa độ biên và bác bỏ tính chẵn lẻ | 

## Vỏ cạnh 

Đối với tính chẵn lẻ khác nhau, hãy xem xét```
0 0
1 0
```Giá trị bắt đầu của x+y là 0, trong khi giá trị đích là 1. Thuật toán từ chối ngay lập tức. Điều này là cần thiết vì lệnh 1 thay đổi (x,y) thành (y,−x), có tổng có cùng tính chẵn lẻ với x+y và cùng thuộc tính cho ba lệnh còn lại. 

Đối với chuyển vị chéo âm, hãy xem xét```
0 0
-3 -3
```đây 

a= 2 −3−3 ​ =−3,b=0. 

Thuật toán phát ra`41`ba lần. Mỗi`41`dịch theo (−1,−1), nên tổng độ dịch chuyển là (−3,−3). Điều này mắc phải sai lầm phổ biến là chỉ có cách xây dựng các hệ số dương. 

Đối với chuyển vị sử dụng hướng chéo khác, hãy xem xét```
0 0
3 -3
```Bây giờ a=0 và b=3, vậy đáp án là`141414`. Mỗi`14`đóng góp (1,−1), cho chính xác (3,−3). 

Để có được sự dịch chuyển tối đa có thể đạt được,```
-100000 -100000
100000 100000
```chúng ta thu được a=200000 và b=0. Câu trả lời chứa 200000 bản sao`23`, đưa ra 400000 lệnh. Giá trị này thấp hơn giới hạn yêu cầu 10 6 và chứng tỏ tại sao công trình tỷ lệ với khoảng cách tọa độ là an toàn. 

Đối với trường hợp cùng x```
5 5
5 -5
```chúng ta nhận được dx=0, dy=−10, do đó a=−5 và b=5. Việc xây dựng kết hợp năm`41`cặp với năm`14`cặp. Tổng độ dời của chúng là 

5(−1,−1)+5(1,−1)=(0,−10), 

do đó tọa độ x không thay đổi được xử lý một cách tự nhiên mà không yêu cầu trường hợp đặc biệt.

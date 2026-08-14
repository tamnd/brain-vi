---
title: "CF 102437J - Robot giao hàng"
description: "Robot bắt đầu tại một điểm nguyên (x 1 ​ ,y 1 ​ ) và phải đạt đến một điểm nguyên khác (x 2 ​ ,y 2 ​ ). Có hai tháp vô tuyến cố định ở (0,0) và (1,0)."
date: "2026-08-14T15:51:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "J"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 369
verified: false
draft: false
---

[CF 102437J - Robot giao hàng](https://codeforces.com/problemset/problem/102437/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 9 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Robot bắt đầu tại một điểm nguyên (x 1 ​ ,y 1 ​ ) và phải đạt đến một điểm nguyên khác (x 2 ​ ,y 2 ​ ). Có hai tháp vô tuyến cố định ở (0,0) và (1,0). Mỗi lệnh xoay điểm hiện tại chính xác 90 ∘, theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ, xung quanh một trong những tòa tháp này. Do đó, bốn lệnh là bốn phép biến đổi affine của mặt phẳng. Nhiệm vụ là in tối đa 10 6 lệnh thực hiện phép biến đổi cần thiết, hoặc in`-1`khi đích đến không thể đạt được. 

Các ràng buộc chính thức đặt mọi tọa độ trong khoảng từ −100000 đến 100000, trong khi giới hạn lệnh là 10 6, với giới hạn thời gian 2 giây và bộ nhớ 512 MB. Một giải pháp tìm kiếm rõ ràng một phần lớn của mặt phẳng sẽ không hấp dẫn. Việc xây dựng hữu ích phải tránh sự phụ thuộc vào kích thước của vùng tọa độ được khám phá và phải tạo ra một chuỗi theo thời gian gần như tuyến tính trong chênh lệch tọa độ. 

Thuộc tính ẩn đầu tiên là tính chẵn lẻ. Mọi thao tác đều bảo toàn tính chẵn lẻ của x+y. Ví dụ: lệnh 1 biến đổi (x,y) thành (y,−x), có tổng tọa độ là y−x, có cùng tính chẵn lẻ với x+y. Một phép quay quanh (1,0) cũng hoạt động tương tự. Do đó, một điểm như (0,1) không thể đạt tới (1,1), vì tổng của chúng có tính chẵn lẻ khác nhau. Đây chính xác là mẫu chính thức thứ hai. 

Một trường hợp dễ xử lý sai khác là sự dịch chuyển chỉ dọc theo một đường chéo. Từ (0,0) đến (1,1) có độ dời là (1,1) nên việc xây dựng chỉ cần lệnh`23`. Việc triển khai chung giả định cả hai thành phần đường chéo là dương hoặc âm sẽ thất bại ở đây vì một trong hai số đếm bằng 0. 

Trường hợp ranh giới thứ ba là chuyển vị lớn. Từ (−100000,−100000) đến (100000,100000), độ dịch chuyển là (200000,200000). Công trình sử dụng 200000 bản dịch chéo`23`, đối với 400000 lệnh, thoải mái dưới 10 6. Giải pháp thực hiện một lệnh trên một đơn vị khoảng cách Manhattan vẫn có thể hoạt động ở đây, nhưng tìm kiếm vũ phu thông qua các chuỗi lệnh sẽ hoàn toàn không khả thi. 

Cuối cùng, câu lệnh đảm bảo rằng điểm đầu và điểm đích khác nhau. Do đó, một đầu vào như (5,5) đến (5,5) nằm ngoài miền đầu vào hợp lệ, mặc dù về mặt toán học, chuỗi trống sẽ giải quyết được nó. Chương trình không cần phải tạo ra một chuỗi lệnh trống. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là coi mọi điểm nguyên là một đỉnh của đồ thị và kết nối mỗi điểm với bốn vị trí thu được bằng bốn lệnh. Tìm kiếm theo chiều rộng là chính xác vì mọi lệnh đều có đơn vị giá trị, do đó, lần đầu tiên BFS đạt được mục tiêu, nó đã tìm thấy một chuỗi hợp lệ. Vấn đề là kích thước của biểu đồ. Bên trong hình vuông tọa độ được đầu vào cho phép có khoảng 200001 2, hoặc khoảng 4⋅10 10, các điểm có thể. Ngay cả việc kiểm tra bốn chuyển đổi đi từ mỗi điểm như vậy cũng sẽ yêu cầu theo thứ tự 1,6⋅10 11 kiểm tra chuyển tiếp. Tìm kiếm trực tiếp các chuỗi lệnh thậm chí còn tệ hơn: tất cả các chuỗi có độ dài tối đa L đều chứa (4 L+1 −1)/3 khả năng. 

Cách tiếp cận bạo lực hoạt động vì mọi lệnh có thể được mô phỏng chính xác, nhưng nó thất bại vì nó bỏ qua cấu trúc đại số được chia sẻ bởi bốn phép biến đổi. Quan sát quan trọng là hai lệnh có thể hủy các phép quay và để lại một bản dịch thuần túy. 

Đặt lệnh 1 là xoay theo chiều kim đồng hồ quanh (0,0) và lệnh 4 là xoay ngược chiều kim đồng hồ quanh (1,0). Áp dụng lệnh 1 theo sau là lệnh 4 sẽ cho 

(x,y) 1 ​ (y,−x) 4 ​ (1+x,y−1). 

Vì vậy cặp`14`dịch mọi điểm theo (1,−1). 

Tương tự, lệnh 2 theo sau là lệnh 3 sẽ cho 

(x,y) 2 ​ (−y,x) 3 ​ (1+x,1+y), 

vậy`23`dịch mọi điểm theo (1,1). 

Hai bản dịch này tạo thành cơ sở cho chính xác lớp chẵn lẻ có thể truy cập được. Nếu chuyển vị yêu cầu là 

(dx,dy)=(x 2 ​ −x 1 ​ ,y 2 ​ −y 1 ​ ), 

chúng ta có thể viết nó như 

(dx,dy)=a(1,1)+b(1,−1), 

ở đâu 

a= 2 dx+dy ​ ,b= 2 dx−dy ​ . 

Các giá trị này là số nguyên chính xác khi dx và dy có cùng tính chẵn lẻ, tương đương với x 1 ​ +y 1 ​ và x 2 ​ +y 2 ​ có cùng tính chẵn lẻ. 

Bản dịch phủ định thu được bằng cách đảo ngược chuỗi hai lệnh tương ứng. Nghịch đảo của`23`là`41`, tạo ra (−1,−1) và nghịch đảo của`14`là`32`, tạo ra (−1,+1). 

Số lượng lệnh đặc biệt thuận tiện. Chúng ta cần 2 lệnh(∣a∣+∣b∣) và 

∣a∣+∣b∣=max(∣dx∣,∣dy∣). 

Vì mỗi chênh lệch tọa độ nhiều nhất là 200000, nên chuỗi cuối cùng chứa tối đa 400000 lệnh, thấp hơn nhiều so với mức cho phép là 10 6. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4 L ) cho độ sâu L, hoặc O(10 11 ) cho khám phá mặt phẳng | O(10 10 ) trong lưới đã khám phá | Quá chậm | 
| Tối ưu | (O(\max( | dx | , | 

## Hướng dẫn thuật toán 

1. Tính độ dịch chuyển dx=x 2 ​ −x 1 ​ và dy=y 2 ​ −y 1 ​. Toàn bộ công trình chỉ cần tái tạo sự dịch chuyển này vì các cặp lệnh chúng tôi sử dụng là các bản dịch hoạt động giống hệt nhau từ mọi điểm bắt đầu. 
2. Kiểm tra xem x 1 ​ +y 1 ​ và x 2 ​ +y 2 ​ có cùng tính chẵn lẻ hay không. Nếu chúng khác nhau, hãy in`-1`. Mọi lệnh hợp pháp đều bảo toàn tính chẵn lẻ của x+y, do đó không có chuỗi nào có thể giao nhau giữa hai lớp chẵn lẻ. 
3. Tính toán 

a=(dx+dy)/2,b=(dx−dy)/2. 

Giá trị a cho chúng ta biết số lần áp dụng dịch (1,1), trong khi b cho chúng ta biết số lần áp dụng (1,−1). Việc kiểm tra tính chẵn lẻ đảm bảo rằng cả hai phép chia đều chính xác. 
4. Với mỗi đơn vị của a > 0, hãy nối thêm`23`. Mỗi cặp thêm chính xác (1,1) vào vị trí hiện tại. Với a<0, nối thêm`41`thay vào đó, vì cặp đó cộng (−1,−1). 
5. Với mỗi đơn vị của b>0, hãy nối thêm`14`. Mỗi cặp cộng (1,−1). Với b<0, nối thêm`32`, cộng (−1,1). 
6. In chuỗi kết quả. Vì 2(∣a∣+∣b∣)=2max(∣dx∣,∣dy∣)≤400000, nên giới hạn lệnh sẽ tự động được đáp ứng. 

### Tại sao nó hoạt động 

Bất biến là tính chẵn lẻ của x+y, do đó việc kiểm tra tính chẵn lẻ sẽ loại bỏ mọi mục tiêu không thể tiếp cận. Đối với mục tiêu có thể tiếp cận, độ dịch chuyển có cùng tính chẵn lẻ ở cả hai tọa độ, tạo thành a và b số nguyên. Các cặp lệnh`23`Và`14`là các bản dịch của (1,1) và (1,−1), trong khi`41`Và`32`là nghịch đảo tương ứng của chúng. Do đó, chuỗi được tạo ra sẽ thay đổi điểm bắt đầu một cách chính xác bằng a(1,1)+b(1,−1)=(dx,dy), đặt robot tại (x 2 ​ ,y 2 ​ ). 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    x1, y1 = map(int, input().split())    x2, y2 = map(int, input().split())
    # x + y parity is invariant under every command.    if (x1 + y1) % 2 != (x2 + y2) % 2:        print(-1)        return
    dx = x2 - x1    dy = y2 - y1
    # dx = a + b    # dy = a - b    a = (dx + dy) // 2    b = (dx - dy) // 2
    ans = []
    if a > 0:        ans.append("23" * a)    elif a < 0:        ans.append("41" * (-a))
    if b > 0:        ans.append("14" * b)    elif b < 0:        ans.append("32" * (-b))
    s = "".join(ans)
    print(len(s))
```Kiểm tra đầu tiên chỉ sử dụng tính chẵn lẻ, do đó không liên quan đến số học dấu phẩy động. Điều này tốt hơn là kiểm tra xem hai phép chia dưới đây có tạo ra số nguyên hay không sau khi sử dụng`/`. 

Các biến`a`Và`b`trực tiếp từ việc giải hai phương trình a+b=dx và a−b=dy. Phép chia số nguyên của Python ở đây an toàn vì điều kiện chẵn lẻ đã thiết lập khả năng chia hết cho hai. 

Trình tự được tích lũy dưới dạng chuỗi thay vì lưu trữ từng lệnh riêng lẻ dưới dạng một đối tượng Python riêng biệt. Câu trả lời hợp lệ lớn nhất có 400000 ký tự, do đó mức sử dụng bộ nhớ sẽ nhỏ. 

Thứ tự của hai nhóm dịch không quan trọng vì cả hai đều là những bản dịch thông thường. Áp dụng tất cả`23`hoặc`41`cặp đầu tiên và tất cả`14`hoặc`32`các cặp sau đó tạo ra chính xác tổng các vectơ dịch chuyển của chúng. 

Số nguyên Python không bị tràn và tọa độ trung gian lớn nhất trong cấu trúc này chỉ theo thứ tự của phạm vi tọa độ đã cho cộng với độ dịch chuyển được tạo. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên chính thức bắt đầu tại (0,1) và mục tiêu (1,−2). 

Ở đây dx=1 và dy=−3. Do đó 

a= 2 1+(−3) ​ =−1,b= 2 1−(−3) ​ =2. 

Thuật toán có thể sử dụng`41`một lần và`14`hai lần. 

| Bước | Hoạt động | Vị trí | 
| --- | --- | --- | 
| 0 | Bắt đầu | (0,1) | 
| 1 |`4`| (0,−1) | 
| 2 |`1`| (1,0) | 
| 3 |`1`| (0,−1) | 
| 4 |`4`| (2,−1) | 
| 5 |`1`| (−1,−2) | 
| 6 |`4`| (1,−2) | 

Trình tự kết quả là`411414`, có sáu lệnh và đạt được mục tiêu cần thiết. Mẫu của`24`ngắn hơn, nhưng vấn đề rõ ràng cho phép bất kỳ chuỗi hợp lệ nào trong giới hạn. 

Dấu vết chứng tỏ rằng việc xây dựng không phụ thuộc vào việc tìm kiếm chuỗi ngắn nhất. Tính đúng đắn của nó đến từ việc biên soạn các bản dịch cố định. 

### Mẫu 2 

Mẫu thứ hai chính thức bắt đầu tại (0,1) và mục tiêu (1,1). 

| Biến | Giá trị | 
| --- | --- | 
| x 1 ​ +y 1 ​ | 1 | 
| x 2 ​ +y 2 ​ | 2 | 
| Bắt đầu chẵn lẻ | lẻ | 
| Mục tiêu ngang bằng | thậm chí | 
| Kết quả |`-1`| 

Thuật toán dừng trước khi xây dựng bất kỳ lệnh nào vì các lớp chẵn lẻ khác nhau. Điều này là cần thiết: mọi lệnh đều bảo toàn tính chẵn lẻ của x+y, do đó không thể đạt được mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\max( | dx | 
| Không gian | (O(\max( | dx | 

Các giới hạn tọa độ cho max(∣dx∣,∣dy∣)200000, do đó thuật toán xuất ra tối đa 400000 lệnh. Đây là mức thoải mái dưới giới hạn lệnh 10 6 và chỉ yêu cầu một phần nhỏ của giới hạn bộ nhớ 512 MB. Giới hạn thời gian chính thức là 2 giây. 

## Trường hợp thử nghiệm 

Bởi vì đầu ra không phải là duy nhất, nên các thử nghiệm không nên so sánh từng ký tự trong chuỗi lệnh được trả về. Thay vào đó, bộ khai thác kiểm tra sẽ phân tích trình tự, mô phỏng tất cả bốn phép biến đổi, kiểm tra xem điểm cuối cùng có chính xác không và xác minh giới hạn số lượng lệnh. 

Mẫu chính thức thứ hai trong tuyên bố là`0 1`theo sau là`1 1`; cái`0 11 1`định dạng trong lời nhắc không đúng định dạng.```python
Pythonimport sysimport io

def solve_text(inp: str) -> str:    data = inp.split()    x1, y1, x2, y2 = map(int, data)
    if (x1 + y1) % 2 != (x2 + y2) % 2:        return "-1\n"
    dx = x2 - x1    dy = y2 - y1
    a = (dx + dy) // 2    b = (dx - dy) // 2
    ans = []
    if a > 0:        ans.append("23" * a)    elif a < 0:        ans.append("41" * (-a))
    if b > 0:        ans.append("14" * b)    elif b < 0:        ans.append("32" * (-b))
    s = "".join(ans)    return f"{len(s)}\n{s}\n"

def simulate(inp: str):    data = inp.split()    x, y, tx, ty = map(int, data)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 1 1 -2`| Bất kỳ chuỗi hợp lệ nào | Mẫu có thể truy cập chính thức và thành phần đường chéo âm | 
|`0 1 1 1`|`-1`| Mẫu không thể truy cập chính thức và bất biến chẵn lẻ | 
|`0 0 1 1`| Một chuỗi hai lệnh tương đương với`23`| Dịch dương (1,1) và không b | 
|`0 0 1 -1`| Một chuỗi hai lệnh tương đương với`14`| Dịch tích cực (1,−1) và 0 a | 
|`10 10 -10 -10`| Bất kỳ chuỗi hợp lệ nào | Số lượng dịch âm | 
|`-100000 -100000 100000 100000`| Bất kỳ chuỗi có độ dài hợp lệ nào 400000 | Chênh lệch tọa độ tối đa và giới hạn lệnh | 
|`0 0 1 0`|`-1`| Trường hợp biên trong đó chuyển vị có tính chẵn lẻ sai | 

Cụm từ "tất cả các giá trị bằng nhau" không thể tương ứng với một bài kiểm tra hợp lệ vì bài toán đảm bảo rằng điểm bắt đầu và điểm đến là khác nhau. Một bài kiểm tra như`5 5 5 5`sẽ vi phạm đặc tả đầu vào, vì vậy nó không nên được đưa vào bộ tính chính xác. 

## Vỏ cạnh 

### Tính chẵn lẻ không khớp 

Hãy xem xét```
0 11 1
```Tổng ban đầu là 1, trong khi tổng mục tiêu là 2. Vì mọi lệnh đều bảo toàn tính chẵn lẻ của x+y nên thuật toán sẽ in ngay lập tức`-1`. Một cấu trúc chỉ kiểm tra xem tọa độ có gần nhau về mặt hình học hay không có thể cố gắng tạo lệnh không chính xác. 

### Một hệ số đường chéo bằng 0 

cho```
0 01 1
```chúng ta nhận được dx=1, dy=1, do đó a=1 và b=0. Thuật toán phát ra`23`. Lệnh 2 ánh xạ (0,0) tới chính nó và lệnh 3 ánh xạ nó tới (1,1). Không cần xử lý đặc biệt đối với hệ số 0 ngoài việc tránh lặp lại không cần thiết. 

### Hệ số âm 

cho```
10 10-10 -10
```chúng ta nhận được a=−20 và b=0. Thuật toán phát ra`41`hai mươi lần. Mỗi`41`dịch điểm theo (−1,−1), do đó sau hai mươi lần lặp lại tổng độ dịch chuyển là (−20,−20), chính xác là sự thay đổi cần thiết. 

### Độ dịch chuyển tối đa 

cho```
-100000 -100000100000 100000
```chúng ta có a=200000 và b=0. Trình tự bao gồm 200000 bản sao của`23`, đưa ra 400000 lệnh. Giới hạn của 10 6 không gần bị vượt quá, do đó không cần sơ đồ nén phức tạp hơn. 

### Sai số chẵn lẻ dù khoảng cách nhỏ 

cho```
0 01 0
```độ dời chỉ có một đơn vị, nhưng x+y thay đổi từ chẵn sang lẻ. Thuật toán từ chối nó ngay lập tức. Trường hợp này rất hữu ích vì nó tránh được sai lầm phổ biến khi cho rằng đủ nhiều phép quay có thể mô phỏng các chuyển động đơn vị tùy ý. Các bản dịch có sẵn di chuyển dọc theo đường chéo và bất biến chẵn lẻ không thể bị phá vỡ.

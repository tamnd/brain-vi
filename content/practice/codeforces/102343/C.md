---
title: "CF 102343C - Lắp trên giường"
description: "Giường là một hình chữ nhật thẳng hàng có góc dưới bên trái là (0, 0) và góc trên bên phải là (W, L). Anya được mô hình hóa như một đoạn thẳng có độ dài H."
date: "2026-08-17T18:06:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102343
codeforces_index: "C"
codeforces_contest_name: "UCF Locals 2019"
rating: 0
weight: 102343
solve_time_s: 816
verified: true
draft: false
---

[CF 102343C - Lắp trên giường](https://codeforces.com/problemset/problem/102343/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Giường là một hình chữ nhật thẳng hàng với góc dưới bên trái là`(0, 0)`và góc trên bên phải của nó là`(W, L)`. Anya được mô hình hóa như một đoạn thẳng có độ dài`H`. Đầu của cô ấy là một điểm cuối của đoạn đó, nằm ở`(x, y)`, và góc`a`mô tả hướng từ đầu đến chân cô ấy. Một góc của`0`chỉ về bên phải, trong khi`90`hướng lên trên, do đó áp dụng quy ước toán học thông thường. 

Nhiệm vụ là xác định xem toàn bộ đoạn đó có nằm bên trong giường hay không. Được phép chạm vào ranh giới, do đó, điểm cuối có tọa độ chính xác`0`,`W`,`0`, hoặc`L`vẫn còn hiệu lực. 

Đầu vào chỉ chứa sáu số nguyên, với`L`Và`W`nhiều nhất`500`,`H`nhiều nhất`200`và tọa độ đầu bị ràng buộc vào giường. Những giới hạn này rất nhỏ, nhưng sự đơn giản hóa thực sự là ở cấu trúc hơn là tính toán. Không có biểu đồ, mảng hoặc không gian tìm kiếm lớn để xử lý. Một số phép tính số học không đổi là đủ. 

Các trường hợp cạnh chính xuất phát từ việc đầu được đảm bảo nằm bên trong giường, nhưng điểm cuối còn lại thì không. Ví dụ,```
100 100 20
90 50 0
```đưa ra điểm cuối thứ hai`(110, 50)`, vậy câu trả lời là`NO`. Việc thực hiện bất cẩn mà chỉ kiểm tra vị trí đầu sẽ chấp nhận nó một cách sai lầm. 

Điểm cuối ranh giới là hợp lệ. Ví dụ,```
100 100 20
80 50 0
```đặt điểm cuối thứ hai tại`(100, 50)`, chính xác ở cạnh phải. Đầu ra đúng là`YES`. Sử dụng các bất đẳng thức chặt chẽ như`0 < x < W`sẽ từ chối nó một cách không chính xác. 

Hướng có thể hướng xuống dưới hoặc sang trái, vì vậy chỉ cần thêm`H * cos(a)`Và`H * sin(a)`mà không xem xét các dấu hiệu của chúng là một sai lầm phổ biến khác. Ví dụ,```
100 100 20
50 50 270
```kết thúc tại`(50, 30)`, ở bên trong giường. Đầu ra đúng là`YES`. 

Đoạn này cũng có thể vừa với đường chéo ngay cả khi một hình chiếu ngang hoặc dọc trông lớn. Ví dụ,```
100 100 50
50 75 225
```kết thúc vào khoảng`(14.64, 39.64)`, vậy cả hai điểm cuối đều ở bên trong và câu trả lời là`YES`. 

## Phương pháp tiếp cận 

Phương pháp hình học trực tiếp có thể thử kiểm tra nhiều điểm dọc theo thân Anya và kiểm tra xem mỗi điểm có nằm bên trong hình chữ nhật hay không. Nếu chúng ta chia cơ thể thành những mảnh có kích thước 1 cm thì có nhiều nhất`H + 1 <= 201`điểm, vì vậy phiên bản này thực sự sẽ chạy thoải mái trong giới hạn. Điểm yếu của nó mang tính khái niệm hơn là thực tế: một đoạn thẳng liên tục chứa vô số điểm và việc kiểm tra một mẫu hữu hạn bản thân nó không chứng minh được rằng các phần không được lấy mẫu nằm ở bên trong. 

Quan sát hữu ích là chiếc giường là một tập hợp lồi. Đoạn đường có hai điểm cuối nằm trong một tập lồi thì hoàn toàn nằm trong tập đó. Một hình chữ nhật là lồi nên chúng ta không cần kiểm tra phần bên trong cơ thể của Anya. Phần đầu đã được cho sẵn nên chỉ cần tính toán và kiểm tra điểm cuối ở chân cô ấy. 

Ý tưởng vũ lực hoạt động vì nó cố gắng xác minh toàn bộ từng điểm một, nhưng nó không cần thiết và không tự nhiên đưa ra bằng chứng chính xác liên tục. Việc quan sát độ lồi làm giảm toàn bộ vấn đề thành việc tính toán một điểm cuối và thực hiện bốn phép so sánh tọa độ. 

Điểm cuối thu được từ chuyển vị tọa độ cực tiêu chuẩn:`dx = H * cos(a)`

`dy = H * sin(a)`vậy là bàn chân đang ở`(x + dx, y + dy)`. 

Vì các hàm lượng giác sử dụng radian nên góc đầu vào trước tiên phải được chuyển đổi bằng`a * pi / 180`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Rời rạc hóa phân khúc | O(H) | O(1) | Được chấp nhận cho các giới hạn này, nhưng không cần thiết về mặt khái niệm | 
| Kiểm tra điểm cuối thứ hai | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước giường`L`Và`W`, chiều cao của Anya`H`, và vị trí đầu`(x, y)`cùng với góc`a`. Bản thân phần đầu đã được biết là nằm trên giường nên không cần tìm kiếm hình học riêng biệt. 
2. Chuyển đổi góc từ độ sang radian. của Python`math.sin`Và`math.cos`mong đợi radian, vì vậy việc sử dụng trực tiếp giá trị độ sẽ tạo ra một hướng hoàn toàn khác. 
3. Tính độ dịch chuyển của chân Anya so với đầu cô ấy bằng cách sử dụng`H * cos(a)`theo chiều ngang và`H * sin(a)`theo chiều dọc. Giá trị âm là hợp lệ vì Anya có thể quay mặt sang trái hoặc hướng xuống dưới. 
4. Thêm chuyển vị vào tọa độ đầu để thu được điểm cuối thứ hai`(fx, fy)`. 
5. Kiểm tra xem`fx`nằm giữa`0`Và`W`và liệu`fy`nằm giữa`0`Và`L`, sử dụng so sánh bao hàm. Dung sai dấu phẩy động nhỏ được sử dụng vì các giá trị như`cos(90°)`được biểu diễn gần đúng chứ không phải là số 0 chính xác. 
6. In`YES`nếu cả hai tọa độ đều nằm trong phạm vi tương ứng của chúng. Nếu không thì in`NO`. 

Thuộc tính quan trọng là trong suốt thuật toán, chúng ta chỉ cần duy trì thực tế là cả hai điểm cuối của đoạn Anya đều nằm bên trong hình chữ nhật. Phần đầu nằm bên trong theo thông số kỹ thuật đầu vào và sau lần kiểm tra tọa độ cuối cùng, phần chân cũng ở bên trong. Vì hình chữ nhật là lồi nên mọi điểm giữa hai điểm cuối đó cũng nằm bên trong hình chữ nhật đó. Do đó, toàn bộ phân đoạn khớp chính xác khi điểm cuối thứ hai được tính toán khớp. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    L, W, H = map(int, input().split())
    x, y, angle = map(int, input().split())

    rad = math.radians(angle)

    foot_x = x + H * math.cos(rad)
    foot_y = y + H * math.sin(rad)

    eps = 1e-9

    if -eps <= foot_x <= W + eps and -eps <= foot_y <= L + eps:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Dòng đầu tiên cung cấp kích thước hình chữ nhật và chiều dài đoạn. Dòng thứ hai cung cấp tọa độ và hướng của đầu.`math.radians(angle)`thực hiện chuyển đổi độ sang radian cần thiết. Việc tính toán điểm cuối được thực hiện trực tiếp từ chuyển vị hình học của một vectơ có chiều dài`H`. 

Các so sánh mang tính bao hàm vì chạm vào ranh giới giường vẫn được coi là phù hợp. Dung sai xử lý nhiễu dấu phẩy động gần các trường hợp biên chính xác. Ví dụ, về mặt toán học`cos(90°) = 0`, nhưng giá trị được tính toán có thể là một con số rất nhỏ chẳng hạn như`6.12e-17`. 

Không có vấn đề tràn số nguyên trong Python và các giá trị dấu phẩy động nhỏ vì mỗi tọa độ và độ dài tối đa là vài trăm. Thứ tự thực hiện cũng quan trọng: góc phải được chuyển đổi trước khi gọi`sin`Và`cos`. 

## Ví dụ đã hoạt động 

Vì trang bài toán Codeforces đã lưu trữ không hiển thị các giá trị mẫu ban đầu nên các dấu vết sau đây sử dụng các đầu vào đại diện thực hiện cùng một hình học. 

### Ví dụ 1 

đầu vào:```
100 100 20
90 50 0
```Thân hướng thẳng về bên phải nên tọa độ ngang tăng thêm`20`. 

| Bước | L | W | H | x | y | góc | foot_x | foot_y | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 100 | 100 | 20 | 90 | 50 | 0° | | | | 
| Chuyển đổi góc | 100 | 100 | 20 | 90 | 50 | 0 rad | | | | 
| Tính toán điểm cuối | 100 | 100 | 20 | 90 | 50 | 0 rad | 110 | 50 | | 
| Kiểm tra giới hạn | 100 | 100 | 20 | 90 | 50 | 0 rad | 110 | 50 |`NO`| 

Đầu ở trong giường nhưng chân ở bên trong`x = 110`, ngoài mép phải. Vì một điểm cuối ở bên ngoài nên đoạn này không thể vừa. 

### Ví dụ 2 

đầu vào:```
100 100 50
50 75 225
```góc`225°`chỉ xuống và sang trái. 

| Bước | L | W | H | x | y | góc | foot_x | foot_y | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 100 | 100 | 50 | 50 | 75 | 225° | | | | 
| Chuyển đổi góc | 100 | 100 | 50 | 50 | 75 |`3.927...`| | | | 
| Tính toán điểm cuối | 100 | 100 | 50 | 50 | 75 |`3.927...`|`14.64...`|`39.64...`| | 
| Kiểm tra giới hạn | 100 | 100 | 50 | 50 | 75 |`3.927...`|`14.64...`|`39.64...`|`YES`| 

Cả hai điểm cuối đều nằm bên trong hình chữ nhật. Tính lồi khi đó cung cấp miễn phí toàn bộ phân đoạn nên không có lý do gì để kiểm tra các điểm trung gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ cần một chuyển đổi góc, hai đánh giá lượng giác và số lượng so sánh không đổi | 
| Không gian | O(1) | Chỉ một số lượng biến vô hướng không đổi được lưu trữ | 

Các ràng buộc vốn đã nhỏ nhưng lời giải hằng số thời gian không phụ thuộc vào`L`,`W`, Và`H`. Nó thoải mái phù hợp với giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solve():
    input = sys.stdin.readline

    L, W, H = map(int, input().split())
    x, y, angle = map(int, input().split())

    rad = math.radians(angle)

    foot_x = x + H * math.cos(rad)
    foot_y = y + H * math.sin(rad)

    eps = 1e-9

    if -eps <= foot_x <= W + eps and -eps <= foot_y <= L + eps:
        print("YES")
    else:
        print("NO")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Custom cases

assert run("1 1 0\n0 0 0\n") == "YES\n", "zero-length segment"

assert run("100 100 20\n90 50 0\n") == "NO\n", \
    "feet pass through the right boundary"

assert run("100 100 20\n80 50 0\n") == "YES\n", \
    "feet land exactly on the right boundary"

assert run("100 100 20\n50 50 270\n") == "YES\n", \
    "downward direction"

assert run("100 100 50\n50 75 225\n") == "YES\n", \
    "diagonal segment"

assert run("500 500 200\n0 0 180\n") == "NO\n", \
    "maximum-size boundary case with feet outside"

assert run("500 500 200\n250 250 45\n") == "YES\n", \
    "maximum dimensions with a diagonal segment"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 0`có đầu`(0,0)`|`YES`| Kích thước tối thiểu và đoạn có độ dài bằng 0 | 
|`100 100 20`, cái đầu`(90,50)`, góc`0`|`NO`| Điểm cuối vượt qua ranh giới bên phải | 
|`100 100 20`, cái đầu`(80,50)`, góc`0`|`YES`| Cho phép liên hệ ranh giới chính xác | 
|`100 100 20`, cái đầu`(50,50)`, góc`270`|`YES`| Chuyển vị dọc âm | 
|`100 100 50`, cái đầu`(50,75)`, góc`225`|`YES`| Vị trí đường chéo | 
|`500 500 200`, cái đầu`(0,0)`, góc`180`|`NO`| Kích thước tối đa và tràn sang trái | 
|`500 500 200`, cái đầu`(250,250)`, góc`45`|`YES`| Vị trí đường chéo hợp lệ lớn | 

## Vỏ cạnh 

Một cái đầu nằm trong giường không có nghĩa là Anya vừa vặn. Vì```
100 100 20
90 50 0
```sự dịch chuyển là`(20, 0)`, vậy bàn chân ở`(110, 50)`. Tọa độ x vi phạm`0 <= x <= 100`và thuật toán in`NO`. 

Liên hệ ranh giới chính xác phải được chấp nhận. Vì```
100 100 20
80 50 0
```bàn chân đang ở`(100, 50)`. điều kiện`foot_x <= W`được thỏa mãn chính xác, vì vậy câu trả lời là`YES`. Epsilon nhỏ cũng bảo vệ sự so sánh này khỏi lỗi biểu diễn dấu phẩy động vô hại. 

Các góc ở nửa dưới của đường tròn tạo ra sự dịch chuyển âm theo phương thẳng đứng. Vì```
100 100 20
50 50 270
```điểm cuối là về mặt toán học`(50, 30)`. Cả hai tọa độ đều nằm trong hình chữ nhật, do đó thuật toán sẽ in`YES`. Việc xử lý góc như thể nó luôn tạo ra chuyển động dương sẽ thất bại ở đây. 

Hướng chéo có thể thay đổi cả hai tọa độ cùng một lúc. Vì```
100 100 50
50 75 225
```điểm cuối là khoảng`(14.64, 39.64)`. Cả hai tọa độ vẫn nằm bên trong giường và độ lồi đảm bảo rằng mọi điểm giữa đầu và chân cũng vẫn ở bên trong. Câu trả lời là`YES`. 

Cuối cùng, một đoạn có độ dài bằng 0 đã được xác định hoàn toàn bởi phần đầu của nó. Với```
1 1 0
0 0 0
```cả hai điểm cuối đều`(0, 0)`, nằm ở góc dưới bên trái của giường. Điểm biên là hợp lệ, vì vậy đầu ra chính xác là`YES`.

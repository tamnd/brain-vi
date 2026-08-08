---
title: "CF 102672B - Điệu nhảy cuồng nhiệt"
description: "Chúng ta có n vũ công được đặt trên tọa độ nguyên. Một số vũ công có thể có cùng tọa độ. Trong khi nhảy, mỗi vũ công sẽ di chuyển độc lập một đơn vị sang trái hoặc phải với xác suất bằng nhau."
date: "2026-08-07T21:35:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102672
codeforces_index: "B"
codeforces_contest_name: "Selection of tasks from Internet olympiads season 2019-20"
rating: 0
weight: 102672
solve_time_s: 74
verified: true
draft: false
---

[CF 102672B - Điệu nhảy điên cuồng](https://codeforces.com/problemset/problem/102672/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`vũ công được đặt trên tọa độ nguyên. Một số vũ công có thể có cùng tọa độ. Trong khi nhảy, mỗi vũ công sẽ di chuyển độc lập một đơn vị sang trái hoặc phải với xác suất bằng nhau. 

Một điệu nhảy thành công nếu tập hợp nhiều vị trí được chiếm giữ sau tất cả các bước di chuyển giống hệt như trước. Chúng ta được phép chọn vị trí ban đầu của các vũ công và cần tối đa hóa khả năng khiêu vũ thành công. Đầu ra là logarit cơ số 2 của xác suất tối đa này. 

Giá trị của`n`có thể lớn tới 40000. Điều này ngay lập tức loại trừ việc mô phỏng, lập trình động theo số lượng vũ công hoặc bất kỳ cách tiếp cận nào phụ thuộc vào số lượng vị trí có thể có. Giải pháp phải đến từ việc tìm ra một mô hình toán học rút gọn câu trả lời thành một công thức trực tiếp. 

Ràng buộc ẩn đầu tiên xuất phát từ tính chẵn lẻ. Mỗi vũ công di chuyển qua đúng một khoảng trống giữa hai tọa độ nguyên. Để cấu hình cuối cùng khớp với cấu hình ban đầu, mỗi khoảng trống phải có cùng số lượng vũ công băng qua nó theo cả hai hướng. Điều đó có nghĩa là tổng số người nhảy phải gấp đôi tổng số lần vượt qua cân bằng. Kết quả là một số lẻ vũ công không bao giờ có thể hoạt động được. 

Ví dụ: với một vũ công:```
1
```câu trả lời là`0`, bởi vì người nhảy duy nhất phải di chuyển ra khỏi tọa độ ban đầu. 

Đối với một ví dụ chẵn:```
4
```xác suất tối ưu là`1/8`, do đó đầu ra là:```
-3
```Một sai lầm phổ biến là chỉ đặt tất cả các vũ công vào hai tọa độ liền kề. Đối với bốn vũ công, việc đặt hai người ở mỗi bên chỉ cho một kiểu giao nhau hợp lệ, với xác suất`1/16`. Sự sắp xếp tốt hơn sử dụng ba tọa độ có số lượng`1,2,1`, trong đó cặp ở giữa có thể trao đổi theo hai cách khác nhau, mang lại xác suất`2/16`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử các vị trí khác nhau của vũ công và đếm tất cả các lựa chọn chuyển động có thể có để giữ cho cấu hình không thay đổi. Vấn đề là có vô số lựa chọn tọa độ có thể xảy ra, và ngay cả khi chúng ta giới hạn bản thân trong một khoảng nhỏ, thì số lượng kết quả chuyển động vẫn là`2^n`. Vì`n = 40000`, thậm chí không thể kiểm tra một vị trí duy nhất. 

Quan sát hữu ích là hãy ngừng suy nghĩ về từng vũ công mà thay vào đó hãy nghĩ về những khoảng trống. Giả sử một khoảng cách giữa hai tọa độ chiếm đóng lân cận có`x`các vũ công băng qua nó từ trái sang phải. Một điệu nhảy thành công đòi hỏi phải chính xác`x`các vũ công băng qua cùng một khoảng cách từ phải sang trái. Do đó, mỗi khoảng trống đều gắn liền với một số lượng giao cắt cân bằng. 

Gọi số lần đi qua các khoảng trống liên tiếp là:```
g1, g2, ..., gk
```Tổng số người nhảy chính xác là:```
2 * (g1 + g2 + ... + gk)
```bởi vì mỗi lần vượt biển đều có một vũ công từ mỗi bên. 

Đối với một chuỗi khoảng trống cố định, số lượng lựa chọn chuyển động hợp lệ được xác định bằng số cách mà mỗi nhóm vũ công có thể chọn hướng của mình. Tại vị trí giữa hai khoảng trống`a`Và`b`, có`a + b`vũ công, và chúng ta phải chọn cái nào`a`trong số họ đi bên trái. Điều này góp phần:```
C(a + b, a)
```cách. 

Câu hỏi còn lại là làm thế nào để chọn kích thước khoảng cách để tối đa hóa tích của các hệ số nhị thức này. Việc chia số lần vượt thành các khoảng trống nhỏ hơn luôn có ích. Đóng góp lớn nhất đến từ việc làm cho mọi khoảng cách đều có quy mô`1`. 

Nếu có`n / 2`tổng số điểm giao cắt và mỗi khoảng trống có kích thước bằng một, sự sắp xếp sẽ trở thành:```
1, 2, 2, ..., 2, 1
```Hai đầu đóng góp một lựa chọn có thể có, trong khi mỗi vị trí ở giữa đều đóng góp:```
C(2,1) = 2
```có`n / 2 - 1`vị trí ở giữa nên số lượt phân công di chuyển thành công là:```
2^(n/2 - 1)
```Tổng số lần nhảy có thể thực hiện được là:```
2^n
```vậy xác suất lớn nhất là:```
2^(n/2 - 1) / 2^n = 2^(-n/2 - 1)
```Vì vậy câu trả lời chỉ đơn giản là:```
-n/2 - 1
```thậm chí`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng vũ công`n`. Thông tin duy nhất cần thiết là tính chẵn lẻ của`n`, bởi vì xác suất tối ưu chỉ phụ thuộc vào việc số lượng vũ công có thể được chia thành các đường giao cắt cân bằng hay không. 
2. Nếu`n`là số lẻ, đầu ra`0`. Một điệu nhảy thành công sẽ yêu cầu mỗi vũ công phải bắt cặp với một vũ công vượt qua cùng một khoảng cách theo hướng ngược lại, điều này là không thể với số lượng vũ công lẻ. 
3. Nếu`n`chẵn, đầu ra`-n/2 - 1`. Đây là logarit của xác suất thu được bằng cách sử dụng các khoảng trống có kích thước đơn vị ở mọi nơi. 

Tại sao nó hoạt động: 

Mỗi điệu nhảy thành công đều tương ứng với sự vượt qua cân bằng trên mọi khoảng cách. Tổng số đường ngang được cố định tại`n/2`. Việc chia những lối đi này thành nhiều khoảng trống hơn sẽ làm tăng số lượng lựa chọn độc lập. Cách phân chia tốt nhất có thể là làm cho mỗi khoảng trống có chính xác một đường giao nhau. Điều này tạo ra số lượng nhiệm vụ di chuyển hợp lệ tối đa có thể, cụ thể là`2^(n/2-1)`. Vì tất cả`2^n`các lựa chọn hướng có khả năng như nhau, xác suất chính xác là`2^(-n/2-1)`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    if n % 2:
        print(0)
    else:
        print(-n // 2 - 1)

solve()
```Việc thực hiện chỉ kiểm tra xem số lượng vũ công có chẵn hay không. Không cần lưu trữ vị trí hoặc mô phỏng bất kỳ chuyển động nào vì xác suất tối ưu được xác định hoàn toàn bởi tính chẵn lẻ và quy mô của`n`. 

biểu hiện`-n // 2 - 1`hoạt động vì phép chia số nguyên của Python là chính xác cho số chẵn`n`. Không cần tính toán dấu phẩy động, điều này tránh được các vấn đề về độ chính xác khi xuất logarit. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

đầu vào:```
4
```Số lượng người nhảy là chẵn nên: 

| Biến | Giá trị | 
| --- | --- | 
| n | 4 | 
| n/2 | 2 | 
| trả lời | -2 - 1 = -3 | 

Xác suất là`2^-3 = 1/8`, phù hợp với mẫu 

Đối với mẫu thứ hai: 

đầu vào:```
1
```Số lượng vũ công là số lẻ. 

| Biến | Giá trị | 
| --- | --- | 
| n | 1 | 
| n % 2 | 1 | 
| trả lời | 0 | 

Một điệu nhảy thành công là không thể, vì vậy sản lượng cần thiết là`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có kiểm tra tính chẵn lẻ và phép tính số học được thực hiện. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung được sử dụng. | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì nó thực hiện một lượng công việc không đổi ngay cả với giá trị lớn nhất của`n`. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    if n % 2:
        out = "0\n"
    else:
        out = str(-n // 2 - 1) + "\n"

    sys.stdin = old_stdin
    return out

assert solve_io("4\n") == "-3\n", "sample 1"
assert solve_io("1\n") == "0\n", "sample 2"

assert solve_io("2\n") == "-2\n", "two dancers"
assert solve_io("6\n") == "-4\n", "three crossing pairs"
assert solve_io("40000\n") == "-20001\n", "maximum size"
assert solve_io("3\n") == "0\n", "odd number of dancers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`-2`| Trường hợp chẵn nhỏ nhất có thể | 
|`6`|`-4`| Công thức cho các giá trị chẵn lớn hơn | 
|`40000`|`-20001`| Xử lý hạn chế tối đa | 
|`3`|`0`| Không thể có sự chẵn lẻ lẻ | 

## Vỏ cạnh 

cho`n = 1`, không có khả năng cân bằng. Người nhảy duy nhất phải di chuyển đến tọa độ khác nên xác suất bằng 0 và thuật toán in ra`0`. 

Đối với các giá trị lẻ như:```
5
```một công thức đơn giản vẫn có thể cố gắng tính giá trị logarit. Lập luận vượt qua cho thấy điều này không thể xảy ra bởi vì các điệu nhảy thành công đòi hỏi mỗi lần vượt qua phải được ghép đôi, nghĩa là tổng số vũ công phải chẵn. Thuật toán phát hiện điều này ngay lập tức và in`0`. 

Đối với trường hợp chẵn nhỏ nhất:```
2
```công thức cho:```
-2 / 2 - 1 = -2
```tương ứng với xác suất`1/4`. Hai vũ công đứng cạnh nhau phải hoán đổi vị trí và đúng một trong bốn lựa chọn chuyển động có thể thực hiện thành công. 

Đối với đầu vào lớn như:```
40000
```giải pháp không xây dựng vị trí hoặc liệt kê các khả năng. Nó tính trực tiếp:```
-20000 - 1 = -20001
```vì vậy không có vấn đề tràn hoặc hiệu suất. 

Điều này cũng có thể được rút ngắn thành định dạng biên tập theo phong cách cuộc thi nếu bạn muốn có một phiên bản gần giống với những gì sẽ xuất hiện trên Codeforces.

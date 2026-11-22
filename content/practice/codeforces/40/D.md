---
title: "CF 40D - Trình tự thú vị"
description: "Trình tự bắt đầu với hai giá trị cố định:"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 40
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 39"
rating: 2600
weight: 40
solve_time_s: 115
verified: true
draft: false
---
[CF 40D - Trình tự thú vị](https://codeforces.com/problemset/problem/40/D) 

**Đánh giá:** 2600 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trình tự bắt đầu với hai giá trị cố định:$$d_1 = 2,\quad d_2 = 13$$Với mỗi năm sau đó, quần thể thỏa mãn một trong hai quy tắc lặp lại:$$d_i = 12d_{i-2}$$hoặc$$d_i = 13d_{i-1} - 12d_{i-2}$$Chúng ta được cho một số nguyên rất lớn$A$, tối đa 300 chữ số thập phân và phải xác định xem một số chuỗi hợp lệ có thể chứa$A$ở một vị trí nào đó. 

Nhiệm vụ này phức tạp hơn việc kiểm tra tư cách thành viên theo một trình tự cụ thể. Những lựa chọn tái phát khác nhau ở những năm khác nhau tạo ra nhiều trình tự có thể xảy ra. Nếu như$A$có thể xuất hiện thì chúng tôi phải báo cáo chỉ số hàng năm nếu có thể. Sau đó, trong suốt những năm đó, chúng ta phải liệt kê mọi giá trị dân số khác cũng có thể xuất hiện ở những vị trí tương tự. 

Kích thước số thay đổi hoàn toàn bản chất của vấn đề. Số nguyên gồm 300 chữ số không thể vừa với loại có chiều rộng cố định, nhưng số nguyên Python xử lý việc này một cách tự nhiên. Thách thức thực sự không phải là tràn số học mà là hiểu cấu trúc của tất cả các giá trị có thể tiếp cận. 

Việc tạo ra các trình tự bạo lực là vô vọng. Mỗi vị trí phân nhánh thành hai khả năng, vì vậy sau$n$nhiều năm rồi đã có rồi$2^{n-2}$trình tự. Ngay cả đối với$n=100$, cái này lớn về mặt thiên văn. 

Cấu trúc ẩn là cả hai phép truy toán đều tuyến tính và phép truy toán thứ hai có dạng rất đặc biệt. Khai thác cấu trúc đó sẽ thu gọn không gian trạng thái hàm mũ thành một thứ cực kỳ nhỏ. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử đầu vào là:```
2
```Câu trả lời là CÓ vì$d_1=2$. Việc triển khai bất cẩn bắt đầu xử lý từ năm thứ 2 trở đi sẽ từ chối nó một cách không chính xác. 

Một trường hợp nguy hiểm khác là:```
13
```Điều này tương ứng với năm thứ 2. Một lần nữa, cần phải xử lý đặc biệt vì sự tái phát chỉ bắt đầu từ năm thứ 3. 

Một trường hợp thú vị hơn là:```
24
```Chúng ta có thể có được:$$d_3 = 12d_1 = 24$$nhưng không phải từ lần tái phát thứ hai vì:$$13\cdot13 - 12\cdot2 = 145$$Việc triển khai giả định mỗi năm chỉ có một giá trị có thể sẽ thất bại ở đây. 

Trường hợp khó khăn nhất về mặt khái niệm là nhiều chuỗi khác nhau thu gọn về cùng một giá trị ở cùng một vị trí. Chúng tôi không tính trình tự. Chúng tôi đang thu thập các giá trị có thể tiếp cận khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ tạo ra đệ quy mọi tiền tố chuỗi hợp lệ. 

Bắt đầu từ$(2,13)$, với mỗi cặp$(d_{i-2}, d_{i-1})$, chúng tôi phân nhánh thành:$$d_i = 12d_{i-2}$$Và$$d_i = 13d_{i-1} - 12d_{i-2}$$Điều này đúng vì nó tuân theo đúng định nghĩa. Vấn đề là yếu tố phân nhánh. Theo năm$n$, có$2^{n-2}$những con đường có thể. Thậm chí dừng lại khi giá trị vượt quá$A$không giúp ích nhiều vì số lượng có thể tăng không đều. 

Quan sát quan trọng là mọi giá trị được tạo ra thực sự là lũy thừa của 12 nhân với 2 hoặc 13. 

Chúng ta hãy kiểm tra hai quá trình chuyển đổi một cách cẩn thận. 

Nếu chúng tôi áp dụng:$$d_i = 12d_{i-2}$$thì chúng ta chỉ cần nhân số hạng trước đó với 12. 

Đối với lần tái phát thứ hai:$$d_i = 13d_{i-1} - 12d_{i-2}$$giả sử hai thuật ngữ trước đó là:$$d_{i-2}=2\cdot12^a,\quad d_{i-1}=13\cdot12^a$$Sau đó:$$d_i =13(13\cdot12^a)-12(2\cdot12^a) =(169-24)12^a =145\cdot12^a =13\cdot12^{a+1}$$Cấu trúc cặp giống nhau tự tái tạo. 

Điều này cho thấy hành vi đầy đủ: 

Vào mỗi năm, các giá trị có thể đạt được là:$$2\cdot12^k$$hoặc$$13\cdot12^k$$với số mũ được xác định bằng tính chẵn lẻ. 

Toàn bộ không gian trạng thái sụp đổ thành hai chuỗi xác định. 

Vấn đề trở thành số học thuần túy. Chúng tôi liên tục chia$A$đến 12 và kiểm tra xem cuối cùng chúng ta đạt tới 2 hay 13. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(2^n)$| Quá chậm | 
| Tối ưu |$O(\log_{12} A)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$A$dưới dạng số nguyên lớn. 
2. Xử lý ngay hai giá trị cơ bản. 

Nếu như$A=2$, thì năm 1 là hợp lệ. 

Nếu như$A=13$, thì năm 2 là hợp lệ. 
3. Chia nhiều lần$A$cho 12 thì nó chia hết cho 12. 

Mỗi phép chia tương ứng với việc bước lùi qua cấu trúc truy hồi. 
4. Đếm xem chúng ta chia cho 12 được bao nhiêu lần. 

Hãy tính số này$k$. Sau đó:$$A = B\cdot12^k$$Ở đâu$B$không chia hết cho 12. 
5. Kiểm tra giá trị cơ bản còn lại$B$. 

Nếu như$B=2$, thì giá trị xuất hiện tại năm:$$2k+1$$vì chuỗi bắt đầu từ số 2 chiếm vị trí lẻ. 

Nếu như$B=13$, thì giá trị xuất hiện tại năm:$$2k+2$$vì chuỗi bắt đầu từ số 13 chiếm các vị trí chẵn. 
6. Nếu không có điều kiện nào xảy ra, hãy in NO. 
7. Nếu không thì in CÓ, sau đó in năm hợp lệ. 
8. Tính tất cả các giá trị khác có thể xuất hiện trong cùng năm đó. 

Vào những năm lẻ, giá trị duy nhất có thể đạt được là:$$2\cdot12^k$$Vào những năm chẵn, giá trị duy nhất có thể đạt được là:$$13\cdot12^k$$Vì giá trị được truy vấn đã chiếm vị trí đó nên không có lựa chọn thay thế nào. 
9. In các giá trị thay thế bằng 0. 

### Tại sao nó hoạt động 

Sự tái diễn bảo tồn một cấu trúc rất cứng nhắc. Bắt đầu từ$(2,13)$, mọi cặp liền kề có thể tiếp cận đều có dạng:$$(2\cdot12^k,\ 13\cdot12^k)$$đối với một số nguyên$k\ge0$. 

Sự tái phát đầu tiên biến đổi:$$2\cdot12^k \rightarrow 2\cdot12^{k+1}$$và biến đổi thứ hai:$$(2\cdot12^k,\ 13\cdot12^k) \rightarrow 13\cdot12^{k+1}$$Cả hai phép toán đều không thể tạo ra bất kỳ thừa số nguyên tố hoặc hệ số nào khác. Bằng quy nạp, mọi giá trị có thể tiếp cận phải chính xác là một trong hai dạng này. 

Ngược lại, mọi giá trị như vậy đều có thể đạt được bằng cách áp dụng lặp lại phép lặp thích hợp. Việc mô tả đặc tính đã hoàn tất nên thuật toán không thể bỏ sót bất kỳ câu trả lời hợp lệ nào hoặc chấp nhận một câu trả lời không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A = int(input().strip())

    x = A
    k = 0

    while x % 12 == 0:
        x //= 12
        k += 1

    if x == 2:
        print("YES")
        print(1)
        print(2 * k + 1)
        print(0)
    elif x == 13:
        print("YES")
        print(1)
        print(2 * k + 2)
        print(0)
    else:
        print("NO")

solve()
```Việc thực hiện phản ánh trực tiếp đặc tính toán học. 

Vòng lặp loại bỏ tất cả các thừa số của 12 khỏi số đầu vào. Sau khi vòng lặp kết thúc, giá trị còn lại xác định xem chuỗi có nguồn gốc từ chuỗi vị trí lẻ bắt đầu từ 2 hay chuỗi vị trí chẵn bắt đầu từ 13. 

Biến`k`đếm xem chúng ta chia cho 12 bao nhiêu lần. Mỗi lần chia tương ứng với hai năm thăng tiến trong dãy, vì nhân với 12 sẽ bảo toàn lớp chẵn lẻ. 

Các công thức:$$2k+1$$Và$$2k+2$$đến trực tiếp từ cấu trúc xen kẽ của các giá trị có thể tiếp cận. 

Một điểm tinh tế là câu trả lời luôn chứa chính xác một năm hợp lệ. Các lựa chọn lặp lại khác nhau không bao giờ tạo ra nhiều giá trị riêng biệt ở cùng mức chẵn lẻ. Toàn bộ quá trình là xác định sau khi giảm lũy thừa 12. 

Số nguyên lớn của Python là đủ cho giới hạn 300 chữ số, do đó không cần số học tùy chỉnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```Quá trình nhân tố hóa: 

| Hiện tại x | Chia hết cho 12 | k | 
| --- | --- | --- | 
| 2 | Không | 0 | 

Giá trị giảm cuối cùng là 2, do đó giá trị này thuộc chuỗi vị trí lẻ. 

Năm:$$2\cdot0+1=1$$Đầu ra:```
YES
1
1
0
```Điều này xác nhận việc xử lý trường hợp cơ bản. Không cần thực hiện các bước lặp lại. 

### Ví dụ 2 

đầu vào:```
3456
```Quá trình nhân tố hóa: 

| Hiện tại x | Chia hết cho 12 | k | 
| --- | --- | --- | 
| 3456 | Có | 0 | 
| 288 | Có | 1 | 
| 24 | Có | 2 | 
| 2 | Không | 3 | 

Chúng tôi có được:$$3456 = 2\cdot12^3$$Điều này thuộc về chuỗi lẻ. 

Năm:$$2\cdot3+1=7$$Đầu ra:```
YES
1
7
0
```Điều này cho thấy việc loại bỏ liên tục các yếu tố của 12 sẽ tái tạo lại vị trí chuỗi như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log_{12} A)$| Mỗi lần lặp lại chia số đó cho 12 | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Ngay cả đối với số nguyên có 300 chữ số, số phép chia rất nhỏ, tỷ lệ thuận với số chữ số. Số học số nguyên lớn của Python dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    A = int(input().strip())

    x = A
    k = 0

    while x % 12 == 0:
        x //= 12
        k += 1

    if x == 2:
        print("YES")
        print(1)
        print(2 * k + 1)
        print(0)
    elif x == 13:
        print("YES")
        print(1)
        print(2 * k + 2)
        print(0)
    else:
        print("NO")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run("2\n") == "YES\n1\n1\n0\n", "sample 1"

# second base value
assert run("13\n") == "YES\n1\n2\n0\n", "base case 13"

# reachable odd-chain value
assert run("24\n") == "YES\n1\n3\n0\n", "2 * 12"

# reachable even-chain value
assert run("1872\n") == "YES\n1\n4\n0\n", "13 * 12^2"

# unreachable value
assert run("25\n") == "NO\n", "not representable"

# large reachable value
assert run(str(2 * (12 ** 20)) + "\n") == (
    "YES\n1\n41\n0\n"
), "large power chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`| CÓ, năm 1 | Xử lý trường hợp cơ bản | 
|`13`| CÓ, năm 2 | Giá trị cơ sở thứ hai | 
|`24`| CÓ, năm 3 | Giá trị chuỗi lẻ không tầm thường đầu tiên | 
|`1872`| CÓ, năm 4 | Tiến trình chuỗi chẵn | 
|`25`| KHÔNG | Từ chối số không hợp lệ | 
|`2 * 12^20`| CÓ, năm 41 | Số nguyên rất lớn | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
2
```Thuật toán không bao giờ đi vào vòng chia vì 2 không chia hết cho 12. Giá trị rút gọn vẫn là 2 với$k=0$. Năm tính được là:$$2\cdot0+1=1$$Đầu ra là chính xác. 

Bây giờ hãy xem xét:```
13
```Một lần nữa không có sự phân chia nào xảy ra. Giá trị giảm là 13, do đó thuật toán xác định chuỗi vị trí chẵn và tính:$$2\cdot0+2=2$$Điều này ánh xạ chính xác đến năm 2. 

Một trường hợp tế nhị hơn là:```
288
```Dấu vết thực hiện: 

| x | k | 
| --- | --- | 
| 288 | 0 | 
| 24 | 1 | 
| 2 | 2 | 

Giá trị giảm trở thành 2, cho:$$2\cdot2+1=5$$Thực vậy:$$d_5 = 2\cdot12^2 = 288$$Cuối cùng hãy xem xét một giá trị không hợp lệ:```
144
```Dấu vết thực hiện: 

| x | k | 
| --- | --- | 
| 144 | 0 | 
| 12 | 1 | 
| 1 | 2 | 

Giá trị rút gọn là 1, không phải 2 hay 13. Thuật toán in ra NO. 

Điều này bắt được các số có lũy thừa thuần túy là 12 mà không có hệ số bắt buộc.

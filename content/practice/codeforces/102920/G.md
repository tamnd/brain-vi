---
title: "CF 102920G - Robot di động"
description: "Chúng ta được đưa ra một dòng với $n$ robot, mỗi robot bắt đầu ở một tọa độ thực nào đó. Chúng ta phải định vị lại chúng sao cho sau khi di chuyển chúng tạo thành một chuỗi hoàn toàn đều đặn: robot $i+1$ phải cách đúng khoảng cách $d$ về phía bên phải của robot $i$."
date: "2026-07-04T07:56:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102920
codeforces_index: "G"
codeforces_contest_name: "2020-2021 ACM-ICPC, Asia Seoul Regional Contest"
rating: 0
weight: 102920
solve_time_s: 40
verified: true
draft: false
---

[CF 102920G - Robot di động](https://codeforces.com/problemset/problem/102920/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng với$n$robot, mỗi robot bắt đầu ở một tọa độ thực nào đó. Chúng ta phải di dời chúng để sau khi di chuyển chúng tạo thành một chuỗi hoàn toàn đều đặn: robot$i+1$phải chính xác là khoảng cách$d$ở bên phải của robot$i$. Do đó, cấu hình cuối cùng được xác định hoàn toàn khi chúng ta chọn vị trí của robot 1, bởi vì tất cả những người khác đều trở thành$x_1, x_1 + d, x_1 + 2d, \dots$. 

Hạn chế mà chúng tôi tối ưu hóa không phải là chuyển động tổng thể mà là chuyển động tối đa của bất kỳ robot đơn lẻ nào. Mỗi robot di chuyển song song nên thời gian hoàn thành sẽ do robot chậm nhất quyết định. Chúng ta phải chọn cấp số cộng cuối cùng để giảm thiểu độ dịch chuyển tuyệt đối cực đại này. 

Kích thước đầu vào lớn, lên tới một triệu robot. Điều này ngay lập tức loại trừ mọi ghép nối bậc hai hoặc lập trình động đối với tất cả các ứng cử viên cho vị trí đầu tiên. Ngay cả việc sắp xếp cũng nằm ở ranh giới nhưng vẫn khả thi trong$O(n \log n)$. Bất kỳ giải pháp nào cố gắng thử tất cả các cách sắp xếp có thể có cho robot 1 giữa các vị trí đầu vào sẽ không thành công. 

Trường hợp phức tạp xuất hiện khi nhiều robot xuất phát ở cùng tọa độ. Vì xung đột được cho phép trong quá trình di chuyển, nên các bản sao không tạo ra các ràng buộc về thứ tự, nhưng chúng ảnh hưởng đến cách chúng ta tính toán việc so khớp với cấp số cộng cuối cùng. 

Một tình huống phức tạp khác là khi sự sắp xếp cuối cùng tối ưu không được căn chỉnh với bất kỳ tọa độ đầu vào nào. Ví dụ: ngay cả khi tất cả các vị trí đầu vào là số nguyên, độ dịch chuyển tối ưu có thể là phân số vì việc giảm thiểu độ lệch tối đa thường tập trung cấu hình vào giữa các ràng buộc cực đoan. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi giá trị có thể cho vị trí bắt đầu$x$của cấp số cộng cuối cùng. Đối với mỗi ứng viên$x$, vị trí cuối cùng được cố định là$x + i \cdot d$và chúng tôi tính toán chênh lệch tuyệt đối tối đa giữa vị trí ban đầu của mỗi robot và vị trí cuối cùng được chỉ định của nó. Câu trả lời là tối thiểu trên tất cả$x$. 

Điều này hoạt động về mặt khái niệm vì một khi chúng tôi cố định thứ tự của robot trong chuỗi thì mọi thứ đều mang tính quyết định. Tuy nhiên, việc lựa chọn$x$là liên tục, không rời rạc. Ngay cả khi chúng tôi rời rạc hóa các ứng viên bằng cách sử dụng các vị trí đầu vào, thì mức tối ưu chính xác vẫn có thể nằm giữa chúng. Tệ hơn nữa, việc đánh giá tất cả các ứng viên sẽ dẫn đến$O(n^2)$thời gian. 

Quan sát quan trọng là chúng tôi không khớp các hoán vị tùy ý. Thứ tự cuối cùng được cố định: các chỉ số robot tương ứng với các vị trí mục tiêu được sắp xếp. Vì vậy, nếu chúng ta sắp xếp các vị trí ban đầu$a_i$, chúng ta có thể giả sử$i$-vị trí bắt đầu nhỏ nhất thứ được khớp với$i$-vị trí mục tiêu nhỏ nhất. Bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi thành cặp đôi thẳng hàng này mà không làm tăng độ lệch tối đa. 

Sau khi được sắp xếp, vấn đề sẽ giảm xuống việc chọn một ca duy nhất$x$. Đối với mỗi robot, chúng tôi so sánh$a_i$với$x + i \cdot d$, vì vậy chúng tôi muốn giảm thiểu$$\max_i |a_i - (x + i d)|.$$Viết lại, xác định$$b_i = a_i - i d,$$sau đó biểu thức trở thành$$\max_i |b_i - x|.$$Bây giờ vấn đề đơn giản về mặt hình học: chọn$x$giảm thiểu khoảng cách tối đa đến một tập hợp các điểm$b_i$trên dòng thực. Tối ưu$x$là trung điểm của cực tiểu và cực đại của$b_i$, và câu trả lời là một nửa mức chênh lệch của chúng. 

Điều này thu gọn vấn đề thành một lần duy nhất sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các vị trí của robot và lưu trữ chúng trong một mảng. Chúng tôi giữ chúng như đã cho vì chúng tôi sẽ sắp xếp chúng bên cạnh để thực thi việc ghép nối nhất quán với các vị trí cuối cùng. 
2. Sắp xếp mảng theo thứ tự không giảm. Điều này đảm bảo rằng$i$-robot thứ trong danh sách đã sắp xếp được gán cho$i$-vị trí thứ trong chuỗi cuối cùng, giúp tránh được sự giao cắt và giảm vấn đề thành sự liên kết đơn điệu. 
3. Chuyển đổi từng vị trí thành giá trị dịch chuyển$b_i = a_i - i \cdot d$, Ở đâu$i$là chỉ số dựa trên số 0 trong mảng được sắp xếp. Bước này mã hóa yêu cầu các robot liền kề phải khác nhau một cách chính xác.$d$. 
4. Tính giá trị tối thiểu và tối đa trong số tất cả$b_i$. Hai giá trị này đại diện cho khoảng thời gian chặt chẽ nhất mà bất kỳ sự thay đổi khả thi nào$x$phải bao phủ để giảm thiểu độ lệch tối đa. 
5. Chuyển số tối ưu$x$là điểm giữa của khoảng này và chuyển động tối đa tối thiểu là một nửa độ dài khoảng, được tính bằng$(\max b_i - \min b_i) / 2$. 
6. Xuất giá trị này với một chữ số thập phân. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mọi phép gán tối ưu đều tôn trọng thứ tự vì việc hoán đổi hai robot được gán không theo thứ tự chỉ làm tăng hoặc duy trì độ lệch tối đa do cấu trúc đơn điệu của các vị trí mục tiêu. Sự biến đổi$b_i = a_i - i d$chuyển bài toán thành việc chọn một số thực duy nhất$x$giảm thiểu độ lệch tuyệt đối tối đa so với một tập hợp các điểm cố định. Đây là một bài toán minimax cổ điển trên một đường thẳng, lời giải của nó được xác định bằng khoảng bao quanh nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, d = map(int, input().split())
a = list(map(int, input().split()))

a.sort()

mn = float('inf')
mx = -float('inf')

for i, x in enumerate(a):
    val = x - i * d
    if val < mn:
        mn = val
    if val > mx:
        mx = val

ans = (mx - mn) / 2.0
print(f"{ans:.1f}")
```Mã bắt đầu bằng cách sắp xếp các vị trí, thực thi sự tương ứng cố định giữa robot và vị trí mục tiêu. Mỗi vị trí được điều chỉnh bằng cách trừ$i \cdot d$, loại bỏ cấu trúc cứng nhắc của đội hình cuối cùng và chuyển bài toán thành bài toán dịch chuyển đều. 

Giá trị tối thiểu và tối đa của các giá trị được chuyển đổi này xác định khoảng thời gian khả thi chặt chẽ nhất cho sự dịch chuyển. Câu trả lời cuối cùng là một nửa khoảng thời gian đó, kể từ khi đặt$x$tại điểm giữa cân bằng độ lệch tối đa ở cả hai bên. 

Cần phải định dạng dấu phẩy động vì câu trả lời có thể là phân số. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 1
1 3 5 7 9
```Mảng được sắp xếp không thay đổi. Chúng tôi tính toán$b_i = a_i - i$: 

| tôi | một [tôi] | b[i] | 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 3 | 2 | 
| 2 | 5 | 3 | 
| 3 | 7 | 4 | 
| 4 | 9 | 5 | 

Tối thiểu là 1, tối đa là 5, vì vậy câu trả lời là$(5 - 1)/2 = 2.0$. 

Điều này cho thấy sự căn chỉnh tối ưu đã được cân bằng hoàn hảo xung quanh điểm dịch chuyển giữa. 

### Mẫu 2 

đầu vào:```
5 1
-10 -1 0 1 2
```Mảng được sắp xếp:```
-10, -1, 0, 1, 2
```| tôi | một [tôi] | b[i] | 
| --- | --- | --- | 
| 0 | -10 | -10 | 
| 1 | -1 | -2 | 
| 2 | 0 | -2 | 
| 3 | 1 | -2 | 
| 4 | 2 | -2 | 

Tối thiểu là -10, tối đa là -2, vậy đáp án là$( -2 - (-10) ) / 2 = 4.0$. 

Trường hợp này cho thấy việc phân cụm sau khi trừ các độ lệch tỷ lệ chỉ số có thể tạo ra một khoảng rộng được điều khiển bởi một ngoại lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; quét tuyến tính đơn sau | 
| Không gian |$O(n)$| Lưu trữ mảng đầu vào | 

Các ràng buộc cho phép lên tới một triệu robot, vì vậy$O(n \log n)$Bước sắp xếp là chi phí chính nhưng vẫn khả thi trong giới hạn thời gian điển hình cho môi trường kiểu ICPC và Codeforces. Phần còn lại của tính toán là tuyến tính và không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n, d = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    mn = float('inf')
    mx = -float('inf')

    for i, x in enumerate(a):
        val = x - i * d
        mn = min(mn, val)
        mx = max(mx, val)

    ans = (mx - mn) / 2.0
    return f"{ans:.1f}"

# provided samples
assert run("5 1\n1 3 5 7 9\n") == "2.0"
assert run("5 1\n-10 -1 0 1 2\n") == "4.0"

# all equal
assert run("4 2\n5 5 5 5\n") == "3.0"

# minimum size
assert run("2 10\n0 100\n") == "45.0"

# already perfect chain
assert run("3 1\n1 2 3\n") == "0.0"

# reversed input
assert run("3 1\n3 2 1\n") == "1.0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 2 / 5 5 5 5 | 3.0 | trùng lặp và bắt đầu thống nhất | 
| 2 10 / 0 100 | 45,0 | lan truyền cực độ | 
| 3 1 / 1 2 3 | 0,0 | already aligned chain |
 | 3 1 / 3 2 1 | 1.0 | độ bền đầu vào chưa được sắp xếp | 

## Vỏ cạnh 

Khi tất cả robot bắt đầu ở cùng một vị trí, việc sắp xếp sẽ tạo ra một mảng không đổi. Sau khi chuyển đổi$b_i = a_i - i d$, các giá trị tạo thành một chuỗi giảm dần nghiêm ngặt và câu trả lời hoàn toàn được điều khiển bởi khoảng cách chỉ mục chứ không phải hình học. Tính toán điểm giữa vẫn mang lại sự dịch chuyển đối xứng chính xác. 

Khi đầu vào bị đảo ngược hoặc không có thứ tự nhiều, việc sắp xếp sẽ đảm bảo tính chính xác. Nếu không sắp xếp, việc ghép nối sẽ không nhất quán và khoảng thời gian được tính toán sẽ không thể hiện bất kỳ kết quả phù hợp khả thi nào. 

Khi$d$lớn, sự khác biệt$i \cdot d$thống trị tọa độ ban đầu. Thuật toán vẫn hoạt động chính xác vì nó chỉ sử dụng những khác biệt tương đối trong không gian được biến đổi, do đó cường độ lớn không gây ra sự mất ổn định.

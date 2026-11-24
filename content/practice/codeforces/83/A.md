---
title: "CF 83A - Mảng Ma Thuật"
description: "Chúng ta được cấp một mảng số nguyên và cần đếm xem có bao nhiêu mảng con liền kề là \"ma thuật\". Một mảng con thật kỳ diệu khi giá trị tối thiểu của nó bằng giá trị tối đa của nó. Giá trị tối thiểu và tối đa chỉ có thể bằng nhau nếu mọi phần tử bên trong mảng con đều giống nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 83
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 72 (Div. 1 Only)"
rating: 1300
weight: 83
solve_time_s: 88
verified: true
draft: false
---

[CF 83A - Ma trận ma thuật](https://codeforces.com/problemset/problem/83/A) 

**Đánh giá:** 1300 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng số nguyên và cần đếm xem có bao nhiêu mảng con liền kề là "ma thuật". Một mảng con thật kỳ diệu khi giá trị tối thiểu của nó bằng giá trị tối đa của nó. 

Giá trị tối thiểu và tối đa chỉ có thể bằng nhau nếu mọi phần tử bên trong mảng con đều giống nhau. Sự quan sát đó làm thay đổi hoàn toàn vấn đề. Thay vì nghĩ về các mảng con tùy ý, chúng ta chỉ quan tâm đến các khối liên tiếp có số bằng nhau. 

Ví dụ: trong mảng:```
2 1 1 4
```các khối có giá trị bằng nhau liên tiếp là:```
[2]
[1 1]
[4]
```Mỗi mảng con một phần tử đều huyền diệu và khối`[1 1]`cũng đóng góp mảng con`[1, 1]`có chiều dài 2. 

Độ dài mảng có thể lớn bằng$10^5$. Một thuật toán bậc hai kiểm tra mọi mảng con sẽ kiểm tra đại khái:$$\frac{n(n+1)}{2}$$mảng con, trở thành về$5 \times 10^9$khi$n = 10^5$. Điều đó vượt xa những gì phù hợp với giới hạn 2 giây. 

Các ràng buộc gợi ý rõ ràng rằng chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Có một số trường hợp khó bỏ sót. 

Hãy xem xét một mảng trong đó tất cả các phần tử đều khác nhau:```
5
1 2 3 4 5
```Câu trả lời đúng là`5`, bởi vì chỉ các mảng con một phần tử mới đủ điều kiện. Việc triển khai bất cẩn có thể vô tình đếm phạm vi dài hơn nếu nó chỉ kiểm tra điểm cuối. 

Bây giờ hãy xem xét một mảng trong đó tất cả các phần tử đều bằng nhau:```
4
7 7 7 7
```Mỗi mảng con đều kỳ diệu. Câu trả lời là:$$\frac{4 \cdot 5}{2} = 10$$Việc triển khai có lỗi có thể chỉ đếm các phân đoạn bằng nhau tối đa một lần thay vì đếm tất cả các mảng con bên trong chúng. 

Một trường hợp tinh vi khác là khi các khối bằng nhau xuất hiện nhiều lần:```
6
1 1 2 2 2 1
```cuối cùng`1`không kết nối với khối đầu tiên của`1`s vì các mảng con phải liền kề nhau. Câu trả lời đúng xuất phát từ việc xử lý từng đoạn bằng nhau một cách độc lập:```
2 + 6 + 1 = 9
```Ở đâu:$$\frac{2 \cdot 3}{2} = 3$$cho khối đầu tiên,$$\frac{3 \cdot 4}{2} = 6$$cho khối giữa, 

và$$1$$cho phần tử cuối cùng. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liệt kê mọi mảng con có thể có, tính giá trị tối thiểu và tối đa của nó và đếm nếu chúng bằng nhau. 

Đối với một mảng con`a[l:r]`, điều kiện:```
min(a[l:r]) == max(a[l:r])
```có nghĩa là tất cả các yếu tố đều giống hệt nhau. 

Việc triển khai đơn giản sẽ sử dụng ba thao tác lồng nhau: 

1. Chọn điểm cuối bên trái. 
2. Chọn điểm cuối phù hợp. 
3. Quét mảng con để tính giá trị tối thiểu và tối đa. 

Chi phí đó$O(n^3)$. 

Ngay cả khi chúng tôi tối ưu hóa mức tối thiểu và tối đa tăng dần, chúng tôi vẫn kiểm tra tất cả$O(n^2)$mảng con. Với$10^5$các yếu tố, điều đó vẫn còn quá chậm. 

Quan sát quan trọng là một mảng con là kỳ diệu khi và chỉ khi mọi phần tử bên trong nó bằng nhau. 

Điều đó có nghĩa là các mảng con ma thuật không bao giờ giao nhau giữa các giá trị khác nhau. Chúng tồn tại hoàn toàn bên trong các dãy số giống hệt nhau liền kề nhau. 

Giả sử chúng ta có một khối có chiều dài$k$:```
x x x x ... x
```Mỗi mảng con bên trong khối này đều kỳ diệu. 

Số lượng mảng con bên trong một chiều dài-$k$đoạn là:$$\frac{k(k+1)}{2}$$bởi vì: 

- có$k$mảng con có độ dài 1, 
-$k-1$có chiều dài 2, 
-$k-2$có chiều dài 3, 
- vân vân. 

Vì vậy, toàn bộ vấn đề giảm xuống: 

1. Tìm độ dài lớn nhất của các đoạn bằng nhau liền kề nhau. 
2. Tổng$\frac{k(k+1)}{2}$trên tất cả các phân khúc. 

Chúng ta có thể quét mảng một lần trong khi theo dõi độ dài đoạn hiện tại. Điều đó đưa ra một giải pháp thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2) đến O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước mảng và chính mảng đó. 
2. Khởi tạo biến trả lời thành`0`. 
3. Bắt đầu quét mảng từ trái sang phải trong khi vẫn duy trì độ dài của khối hiện tại có giá trị bằng nhau. 
4. Đặt độ dài khối hiện tại thành`1`cho phần tử đầu tiên. 
5. Đối với mỗi phần tử tiếp theo: 

1. Nếu nó bằng phần tử trước đó, hãy mở rộng khối hiện tại bằng cách tăng độ dài. 
2. Ngược lại, khối trước đó đã kết thúc. Thêm vào:$$\frac{len \cdot (len + 1)}{2}$$cho câu trả lời, sau đó bắt đầu một khối mới có độ dài`1`. 

Công thức đếm tất cả các mảng con hoàn toàn bên trong phân đoạn có giá trị bằng nhau đó. 

1. Sau khi vòng lặp kết thúc, hãy thêm phần đóng góp của khối cuối cùng, vì mảng có thể kết thúc khi chúng ta vẫn ở trong một phân đoạn. 
2. In câu trả lời tổng thể. 

### Tại sao nó hoạt động 

Mọi phân mảng ma thuật đều bao gồm các giá trị hoàn toàn bằng nhau. Một mảng con như vậy phải nằm hoàn toàn bên trong một đoạn bằng nhau liền kề tối đa. 

Đối với một đoạn có độ dài$k$, mọi mảng con liền kề có thể có bên trong nó đều thật kỳ diệu và chính xác là:$$\frac{k(k+1)}{2}$$của họ. 

Thuật toán phân chia mảng thành các phân đoạn bằng nhau tối đa và đếm tất cả các phân đoạn con bên trong mỗi phân đoạn chính xác một lần. Không có mảng con ma thuật nào bị bỏ sót và không có mảng con không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

ans = 0
length = 1

for i in range(1, n):
    if a[i] == a[i - 1]:
        length += 1
    else:
        ans += length * (length + 1) // 2
        length = 1

ans += length * (length + 1) // 2

print(ans)
```Chương trình theo dõi đoạn liền kề hiện tại có giá trị bằng nhau. 

Biến`length`lưu trữ kích thước của phân khúc hiện tại. Bất cứ khi nào giá trị tiếp theo khớp với giá trị trước đó, chúng tôi sẽ mở rộng phân khúc. Nếu không, phân đoạn sẽ kết thúc và chúng tôi thêm phần đóng góp của nó. 

Công thức:$$\frac{k(k+1)}{2}$$được thực hiện như:```
length * (length + 1) // 2
```Phép chia số nguyên là cần thiết vì kết quả phải là số nguyên. 

Phần bổ sung cuối cùng sau vòng lặp rất dễ bị quên. Không có nó, phân đoạn cuối cùng sẽ không bao giờ đóng góp vào câu trả lời. 

Giải pháp chỉ sử dụng bộ nhớ bổ sung liên tục và quét mảng một lần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
2 1 1 4
```| Chỉ mục | Giá trị | Độ dài phân đoạn hiện tại | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | Bắt đầu phân đoạn | 0 | 
| 1 | 1 | 1 | Đoạn trước kết thúc, thêm 1 | 1 | 
| 2 | 1 | 2 | Mở rộng phân khúc | 1 | 
| 3 | 4 | 1 | Đoạn trước kết thúc, thêm 3 | 4 | 
| Kết thúc | - | 1 | Thêm phân đoạn cuối cùng | 5 | 

Các phân đoạn là:```
[2]
[1 1]
[4]
```Đóng góp của họ là:```
1 + 3 + 1 = 5
```Dấu vết này cho thấy mỗi khối bằng nhau được xử lý độc lập. 

### Mẫu 2 

đầu vào:```
5
1 1 1 2 3
```| Chỉ mục | Giá trị | Độ dài phân đoạn hiện tại | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | Bắt đầu phân đoạn | 0 | 
| 1 | 1 | 2 | Mở rộng phân khúc | 0 | 
| 2 | 1 | 3 | Mở rộng phân khúc | 0 | 
| 3 | 2 | 1 | Đoạn trước kết thúc, thêm 6 | 6 | 
| 4 | 3 | 1 | Đoạn trước kết thúc, thêm 1 | 7 | 
| Kết thúc | - | 1 | Thêm phân đoạn cuối cùng | 8 | 

Các phân đoạn là:```
[1 1 1]
[2]
[3]
```Những đóng góp là:```
6 + 1 + 1 = 8
```Ví dụ này chứng tỏ một đoạn dài bằng nhau đóng góp nhiều mảng con cùng một lúc như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mảng được quét một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Quét tuyến tính dễ dàng phù hợp với giới hạn cho$n = 10^5$. Việc sử dụng bộ nhớ rất nhỏ so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    ans = 0
    length = 1

    for i in range(1, n):
        if a[i] == a[i - 1]:
            length += 1
        else:
            ans += length * (length + 1) // 2
            length = 1

    ans += length * (length + 1) // 2

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("4\n2 1 1 4\n") == "5\n", "sample 1"

# all elements different
assert run("5\n1 2 3 4 5\n") == "5\n", "all distinct"

# all elements equal
assert run("4\n7 7 7 7\n") == "10\n", "all equal"

# minimum size
assert run("1\n42\n") == "1\n", "single element"

# multiple separated blocks
assert run("6\n1 1 2 2 2 1\n") == "10\n", "multiple segments"

# alternating values
assert run("6\n1 2 1 2 1 2\n") == "6\n", "no long magical subarrays"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3 4 5`|`5`| Chỉ tính các phần tử đơn lẻ | 
|`7 7 7 7`|`10`| Công thức cho một phân khúc lớn | 
|`42`|`1`| Kích thước đầu vào tối thiểu | 
|`1 1 2 2 2 1`|`10`| Các khối bằng nhau riêng biệt được xử lý độc lập | 
|`1 2 1 2 1 2`|`6`| Các giá trị thay thế chỉ tạo ra các mảng con có độ dài 1 | 

## Vỏ cạnh 

Hãy xem xét trường hợp mọi giá trị đều khác nhau:```
5
1 2 3 4 5
```Thuật toán tạo ra năm đoạn riêng biệt có độ dài 1. 

Đóng góp của họ là:```
1 + 1 + 1 + 1 + 1 = 5
```Không còn phân mảng nào được tính vì phân đoạn bị ngắt ngay lập tức bất cứ khi nào giá trị thay đổi. 

Bây giờ hãy xem xét một mảng trong đó tất cả các giá trị đều bằng nhau:```
4
7 7 7 7
```Quá trình quét không bao giờ phá vỡ phân khúc. Cuối cùng:```
length = 4
```Thuật toán bổ sung:$$\frac{4 \cdot 5}{2} = 10$$khớp với tổng số của tất cả các mảng con có thể có. 

Cuối cùng, hãy xem xét các khối bằng nhau được phân tách:```
6
1 1 2 2 2 1
```Các quá trình thuật toán:```
[1 1]
[2 2 2]
[1]
```một cách độc lập. 

Đầu tiên và cuối cùng`1`không bao giờ được hợp nhất bởi vì`2`s phá vỡ sự tiếp giáp. Điều đó ngăn chặn việc đếm các mảng con không hợp lệ như:```
1 1 2 2 2 1
^           ^
```đó không phải là phép thuật.

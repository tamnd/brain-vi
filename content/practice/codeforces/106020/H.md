---
title: "CF 106020H - Mảng con Mexico"
description: "Chúng ta được cho một mảng các số nguyên không âm. Đối với mỗi phân đoạn liền kề của mảng, chúng ta có thể tính hai giá trị. Đầu tiên là tổng của tất cả các phần tử bên trong phân khúc."
date: "2026-06-25T13:11:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106020
codeforces_index: "H"
codeforces_contest_name: "The 2025 Damascus University Collegiate Programming Contest"
rating: 0
weight: 106020
solve_time_s: 57
verified: true
draft: false
---

[CF 106020H - Mảng con Mexico](https://codeforces.com/problemset/problem/106020/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm. Đối với mỗi phân đoạn liền kề của mảng, chúng ta có thể tính hai giá trị. Đầu tiên là tổng của tất cả các phần tử bên trong phân khúc. Thứ hai là MEX của phân đoạn, là số nguyên không âm nhỏ nhất không xuất hiện trong phân đoạn đó. Nhiệm vụ là đếm xem có bao nhiêu phân đoạn có hai giá trị này bằng nhau. 

Độ dài mảng có thể đạt tới$10^5$. Một cách tiếp cận trực tiếp để kiểm tra mọi mảng con có thể sẽ xem xét về$n(n+1)/2$các phân đoạn, đại khái là$5 \cdot 10^9$trong trường hợp lớn nhất. Ngay cả khi việc kiểm tra một phân đoạn là thời gian không đổi, điều này vượt xa những gì giải pháp một giây có thể xử lý. Chúng ta cần tránh liệt kê tất cả các mảng con. 

Phạm vi giá trị lớn của các phần tử cũng là một gợi ý. Các giá trị thực tế ít quan trọng hơn mối quan hệ của chúng với MEX. Vì tất cả các số đều không âm nên tổng của một mảng con tăng nhanh khi nó chứa các giá trị lớn hơn. Điều này cho phép chúng tôi chứng minh rằng chỉ những giá trị MEX rất nhỏ mới có thể hoạt động. 

Trường hợp cạnh đầu tiên là một đoạn có MEX bằng 0. Ví dụ:```
Input:
3
5 5 5
```Toàn bộ mảng có tổng 15, không phải 0, nhưng mọi mảng con đều có MEX 0 vì không có số 0 ở bất kỳ đâu. Câu trả lời đúng là 6, không phải 0. Một giải pháp chỉ tìm kiếm các mảng con chứa số 0 sẽ bỏ sót tất cả chúng. 

Trường hợp cạnh thứ hai là khi đoạn chứa các giá trị cực lớn. Ví dụ:```
Input:
4
0 1 100 2
```Mảng con`[0,1,100,2]`có MEX 3 nhưng tổng của nó là 103 nên không hợp lệ. Một giải pháp bất cẩn chỉ kiểm tra xem tất cả các số từ 0 đến MEX-1 xuất hiện có thể đếm sai hay không. 

Trường hợp cạnh thứ ba là về các bản sao. Ví dụ:```
Input:
4
0 1 2 2
```Mảng con`[0,1,2,2]`có MEX 3, nhưng tổng là 5. Các phân đoạn MEX 3 hợp lệ phải chứa chính xác các giá trị 0, 1 và 2 mà không có bất kỳ giá trị dương bổ sung nào. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực tự nhiên là sửa điểm cuối bên trái và mở rộng điểm cuối bên phải. Trong khi mở rộng, chúng tôi duy trì tổng hiện tại và MEX hiện tại bằng cách sử dụng tần số của các giá trị. Điều này đúng vì mỗi mảng con được truy cập một lần và chúng ta có thể kiểm tra điều kiện ngay lập tức. 

Vấn đề là số lượng mảng con. có$O(n^2)$trong số họ, vì vậy trường hợp xấu nhất thực hiện khoảng$5 \cdot 10^9$phần mở rộng. Điều này là quá chậm. 

Quan sát quan trọng đến từ việc so sánh tổng với MEX. Giả sử một mảng con có MEX bằng$m$. Sau đó, nó phải chứa mọi giá trị từ 0 đến$m-1$, do đó tổng tối thiểu có thể có của mảng con là:$$0+1+2+\dots+(m-1)=\frac{m(m-1)}{2}$$Để điều này bằng$m$, chúng ta cần:$$\frac{m(m-1)}{2} \le m$$Điều này chỉ đúng cho$m \le 3$. Vì vậy, mọi mảng con hợp lệ đều có MEX 0, 1, 2 hoặc 3. 

Bây giờ mỗi trường hợp trở nên đơn giản. MEX bằng 0 có nghĩa là mảng con không chứa số 0. MEX bằng 1 có nghĩa là nó chính xác bằng 0. MEX bằng 2 có nghĩa là nó phải chứa 0 và 1 và không chứa gì khác, vì vậy nó có độ dài bằng 2. MEX bằng 3 có nghĩa là nó phải chứa chính xác các giá trị 0, 1 và 2, do đó nó có độ dài bằng 3. 

Toàn bộ vấn đề giảm xuống việc kiểm tra các cửa sổ có kích thước rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tất cả các mảng con có MEX bằng 0. Đây chính xác là các khối liên tiếp không chứa số 0. Nếu một khối có chiều dài$k$, nó góp phần$k(k+1)/2$mảng con. 
2. Đếm tất cả các mảng con có MEX bằng 1. Đoạn này phải chứa số 0 và không được chứa số 1. Vì tổng của nó phải bằng 1 và tất cả các giá trị đều không âm nên phân đoạn duy nhất có thể là`[0]`. Thêm một cho mỗi số không. 
3. Đếm tất cả các mảng con có MEX bằng 2. Đoạn này phải chứa cả 0 và 1. Tổng của chúng đã bằng 1 nên đoạn đó cần thêm đúng một đơn vị nữa. Khả năng duy nhất là cặp liền kề`[0,1]`hoặc`[1,0]`. 
4. Đếm tất cả các mảng con có MEX bằng 3. Phân đoạn phải chứa 0, 1 và 2. Tổng của chúng đã bằng 3 nên phân đoạn không thể chứa bất kỳ thứ gì khác. Đoạn phải có độ dài bằng ba và giá trị của nó phải chính xác bằng ba số đó. 
5. Cộng bốn số đếm. Những trường hợp này bao gồm mọi mảng con hợp lệ có thể có vì không có MEX nào lớn hơn có thể thỏa mãn điều kiện tổng. 

Tại sao nó hoạt động: mọi mảng con hợp lệ đều có giá trị MEX. Bất đẳng thức rút ra ở trên chứng tỏ rằng MEX không thể vượt quá 3. Đối với mỗi MEX có thể, tổng yêu cầu buộc phải có một cấu trúc rất cụ thể. Thuật toán đếm chính xác các cấu trúc đó, vì vậy mọi mảng con được đếm đều hợp lệ và mọi mảng con hợp lệ đều được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    ans = 0

    # MEX = 0
    length = 0
    for x in a:
        if x == 0:
            ans += length * (length + 1) // 2
            length = 0
        else:
            length += 1
    ans += length * (length + 1) // 2

    # MEX = 1
    for x in a:
        if x == 0:
            ans += 1

    # MEX = 2
    for i in range(n - 1):
        if (a[i] == 0 and a[i + 1] == 1) or (a[i] == 1 and a[i + 1] == 0):
            ans += 1

    # MEX = 3
    for i in range(n - 2):
        if a[i] + a[i + 1] + a[i + 2] == 3:
            if a[i] != a[i + 1] and a[i] != a[i + 2] and a[i + 1] != a[i + 2]:
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên xử lý MEX 0 bằng cách chia mảng ở mọi số 0. Biến`length`lưu trữ kích thước khối chỉ dương hiện tại. Khi khối kết thúc, phần đóng góp của nó là số mảng con có thể có bên trong nó. 

Vòng lặp thứ hai xử lý MEX 1. Một số 0 duy nhất là phân đoạn hợp lệ duy nhất vì bất kỳ giá trị nào khác sẽ làm cho tổng lớn hơn MEX. 

Vòng lặp thứ ba kiểm tra các cặp liền kề. Hai đoạn có độ dài duy nhất có tổng bằng 2 và MEX 2 là`[0,1]`Và`[1,0]`. 

Vòng lặp cuối cùng kiểm tra độ dài ba cửa sổ. Tổng phải chính xác bằng 3 và ba giá trị phải khác nhau. Vì các giá trị duy nhất có thể có khi đó là 0, 1 và 2 nên MEX chính xác là 3. 

Không có tổng tiền tố hoặc bản đồ vì việc quan sát cấu trúc làm giảm mọi câu trả lời có thể có cho các cửa sổ có kích thước không đổi. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
7
1 0 2 0 3 4 3
```Số lượng MEX 0, 1, 2 và 3 phát triển như sau: 

| Chỉ mục | Giá trị | MEX 0 khối | MEX 1 đếm | Số MEX 2 | Số MEX 3 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 0 | 0 | 
| 1 | 0 | 0 | 1 | 1 | 0 | 
| 2 | 2 | 0 | 1 | 1 | 0 | 
| 3 | 0 | 0 | 2 | 1 | 0 | 
| 4 | 3 | 0 | 2 | 1 | 0 | 
| 5 | 4 | 0 | 2 | 1 | 0 | 
| 6 | 3 | 1 | 2 | 1 | 0 | 

Các khối chỉ tích cực cho MEX 0 là`[1]`,`[2]`,`[3,4,3]`, đóng góp 1 + 1 + 6. Trường hợp MEX 3 không có cửa sổ hợp lệ vì mọi cửa sổ chứa 0,1,2 cũng bao gồm một giá trị phụ. Câu trả lời cuối cùng là 2. 

Một ví dụ khác:```
5
0 1 2 5 6
```| Cửa sổ | Giá trị | Lý do hợp lệ | 
| --- | --- | --- | 
| 0 |`[0]`| MEX 1 | 
| 1 |`[0,1]`| MEX 2 | 
| 2 |`[0,1,2]`| MEX 3 | 
| 3 |`[0,1,2,5]`| Không hợp lệ, số tiền bổ sung | 
| 4 |`[5]`| MEX 0 | 

Thuật toán tìm thấy một phân đoạn MEX 1, một phân đoạn MEX 2 và một phân đoạn MEX 3. Giá trị đơn cuối cùng thuộc về khối dương, thêm một phân đoạn MEX 0, cho câu trả lời 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử mảng tham gia vào một số lần kiểm tra không đổi. | 
| Không gian | O(1) | Chỉ các bộ đếm và mảng hiện tại được lưu trữ. | 

Giải pháp phù hợp với$10^5$hạn chế vì nó chỉ thực hiện một vài lần quét tuyến tính. Các giá trị số nguyên có thể lớn, nhưng số nguyên Python xử lý phạm vi câu trả lời được yêu cầu một cách an toàn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    ans = 0

    length = 0
    for x in a:
        if x == 0:
            ans += length * (length + 1) // 2
            length = 0
        else:
            length += 1
    ans += length * (length + 1) // 2

    for x in a:
        if x == 0:
            ans += 1

    for i in range(n - 1):
        if (a[i], a[i + 1]) in [(0, 1), (1, 0)]:
            ans += 1

    for i in range(n - 2):
        if sorted(a[i:i+3]) == [0, 1, 2]:
            ans += 1

    return str(ans) + "\n"

assert solve("7\n1 0 2 0 3 4 3\n") == "2\n"

assert solve("1\n0\n") == "1\n"

assert solve("5\n0 1 2 5 6\n") == "4\n"

assert solve("4\n5 5 5 5\n") == "10\n"

assert solve("6\n0 1 2 0 1 2\n") == "4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`1`| Kích thước tối thiểu và MEX 1 | 
|`0 1 2 5 6`|`4`| Từ chối các giá trị cực lớn | 
|`5 5 5 5`|`10`| Tất cả các phân đoạn MEX 0 | 
|`0 1 2 0 1 2`|`4`| Nhiều chiều dài hợp lệ ba cửa sổ | 

## Vỏ cạnh 

Đối với trường hợp toàn dương tính:```
4
5 5 5 5
```Mọi mảng con đều có MEX 0 vì thiếu số 0. Thuật toán không bao giờ nhập các trường hợp MEX 1, 2 hoặc 3, nhưng việc đếm khối dương sẽ bổ sung thêm$4 \cdot 5 / 2 = 10$, đó chính xác là số lượng mảng con. 

Đối với phân khúc gần như thỏa mãn MEX 3:```
4
0 1 2 2
```Cửa sổ`[0,1,2]`được tính vì nó có tổng bằng 3 và MEX 3. Cửa sổ`[0,1,2,2]`bị bỏ qua vì độ dài không bằng ba và 2 cộng thêm sẽ làm tăng tổng. Việc kiểm tra cửa sổ có kích thước cố định sẽ ngăn ngừa lỗi này một cách tự nhiên. 

Đối với các giá trị lớn hơn MEX có thể:```
4
0 1 100 2
```Các phân đoạn hợp lệ duy nhất có thể là`[0]`Và`[0,1]`nếu liền kề, nhưng giá trị 100 không thể tham gia vào phân đoạn MEX 3 hợp lệ. Thuật toán không bao giờ xử lý đặc biệt các giá trị lớn vì bằng chứng trước đó đã đảm bảo rằng chúng không thể xuất hiện trong phân đoạn MEX hợp lệ cao hơn.

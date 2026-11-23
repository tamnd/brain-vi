---
title: "CF 486E - LIS của trình tự"
description: "Chúng ta được cung cấp một mảng và chúng ta quan tâm đến các dãy con tăng dài nhất của nó. Một chỉ mục thuộc về dãy con tăng dài nhất nếu giá trị tại vị trí đó được chọn là một trong các phần tử của dãy con đó. Nhiệm vụ không phải là đếm LIS."
date: "2026-06-07T17:29:17+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy", "hashing", "math"]
categories: ["algorithms"]
codeforces_contest: 486
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 277 (Div. 2)"
rating: 2200
weight: 486
solve_time_s: 143
verified: true
draft: false
---

[CF 486E - LIS của trình tự](https://codeforces.com/problemset/problem/486/E) 

**Đánh giá:** 2200 
**Tags:** cấu trúc dữ liệu, dp, tham lam, băm, toán 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và chúng ta quan tâm đến các dãy con tăng dài nhất của nó. Một chỉ mục thuộc về dãy con tăng dài nhất nếu giá trị tại vị trí đó được chọn là một trong các phần tử của dãy con đó. 

Nhiệm vụ không phải là đếm LIS. Thay vào đó, đối với mỗi vị trí, chúng ta phải xác định nó thuộc về loại nào trong ba loại. 

Một vị trí nằm trong nhóm 1 nếu nó không bao giờ xuất hiện trong bất kỳ LIS nào. 

Một vị trí nằm trong nhóm 2 nếu nó xuất hiện trong ít nhất một LIS, nhưng tồn tại một LIS khác không sử dụng nó. 

Một vị trí nằm trong nhóm 3 nếu mọi LIS có thể đều phải chứa vị trí đó. 

Độ dài mảng có thể đạt tới$10^5$. Bất kỳ thuật toán nào liệt kê rõ ràng các chuỗi con tăng dần đều là vô vọng vì số lượng LIS có thể là số mũ. Thậm chí một$O(n^2)$giải pháp lập trình động thực hiện khoảng$10^{10}$so sánh trong trường hợp xấu nhất, vượt xa giới hạn. Độ phức tạp của mục tiêu là khoảng$O(n \log n)$. 

Phần khó khăn là việc phân biệt nhóm 2 với nhóm 3. Biết rằng một chỉ mục thuộc về một số LIS là chưa đủ. Chúng ta phải biết liệu có cách nào khác để xây dựng LIS mà không cần sử dụng vị trí đó hay không. 

Hãy xem xét mảng:```
1 3 2 5
```Độ dài LIS là 3. Có hai LIS:```
1 3 5
1 2 5
```Các vị trí chứa 3 và 2 thuộc về một số LIS, nhưng không phải tất cả các LIS. Câu trả lời của họ phải là`2`. 

Một trường hợp tế nhị khác là:```
1 2 3
```Chỉ có một LIS. Mọi vị trí đều thuộc về nó nên đáp án là:```
333
```Một giải pháp bất cẩn chỉ kiểm tra xem một vị trí có thể xuất hiện trong LIS hay không sẽ in sai`222`. 

Một ví dụ quan trọng hơn là:```
2 2 2
```Độ dài LIS là 1. Mỗi vị trí riêng lẻ tạo thành một LIS. Không có vị trí nào là bắt buộc vì chúng ta có thể chọn bất kỳ vị trí nào trong ba vị trí đó. Câu trả lời đúng là:```
222
```Việc coi tất cả các vị trí LIS-length-1 là bắt buộc sẽ sai. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản về mặt khái niệm. Tạo mọi dãy con tăng dài nhất, ghi lại chỉ mục nào xuất hiện, sau đó phân loại từng chỉ mục theo số lượng LIS chứa nó. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. 

Vấn đề là quy mô. Ngay cả đối với đầu vào vừa phải, số lượng LIS có thể rất lớn. Một mảng như```
1 2 1 2 1 2 ...
```đã tạo ra nhiều sự lựa chọn. Việc liệt kê tất cả các LIS sẽ trở thành hàm mũ. 

Quan sát quan trọng là chúng ta không cần đến LIS. Chúng tôi chỉ cần thông tin cấu trúc về vị trí của mỗi vị trí trong LIS. 

Cho phép$L_i$là độ dài của dãy con tăng dài nhất kết thúc ở vị trí$i$. 

Cho phép$R_i$là độ dài của dãy con tăng dài nhất bắt đầu từ vị trí$i$. 

Nếu chiều dài LIS tổng thể là$K$, sau đó định vị$i$có thể thuộc về một số LIS chính xác khi$$L_i + R_i - 1 = K.$$Lý do là vậy$L_i$mô tả chuỗi tăng dần tốt nhất$i$,$R_i$mô tả chuỗi rời đi tăng dần tốt nhất$i$, và vị trí$i$được tính ở cả hai phần. 

Điều này đã tách nhóm 1 khỏi những nhóm khác. 

Câu hỏi còn lại là quyết định liệu một vị trí có bắt buộc hay không. 

Mỗi vị trí nằm trên LIS chiếm một lớp cụ thể, cụ thể là lớp$L_i$. Dọc theo bất kỳ LIS nào, chúng ta phải chọn chính xác một vị trí từ mỗi lớp$1,2,\ldots,K$. 

Giả sử trong số tất cả các vị trí thỏa mãn$$L_i + R_i - 1 = K$$chỉ có một vị trí trong lớp$t$. 

Mỗi LIS phải chiếm một vị trí từ lớp$t$, và chỉ có một ứng cử viên. Vị trí đó là tất yếu và thuộc nhóm 3. 

Nếu một lớp chứa hai hoặc nhiều ứng cử viên thì không có ứng cử viên nào trong số đó là bắt buộc riêng lẻ. Chúng ta có thể chọn một ứng cử viên khác từ cùng lớp khi xây dựng LIS. Các vị trí đó thuộc nhóm 2. 

Điều này dẫn đến một$O(n \log n)$giải pháp sử dụng thuật toán LIS sắp xếp kiên nhẫn tiêu chuẩn hai lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán$L_i$, độ dài LIS kết thúc tại mỗi vị trí. 

Xử lý mảng từ trái sang phải bằng kỹ thuật sắp xếp kiên nhẫn. Với mỗi giá trị, hãy tìm vị trí chèn của nó trong mảng tails bằng tìm kiếm nhị phân. Chỉ số chèn cộng một là$L_i$. 
2. Tính toán$R_i$, độ dài LIS bắt đầu từ mỗi vị trí. 

Xử lý mảng từ phải sang trái. Áp dụng quy trình LIS tương tự cho các giá trị$-a_i$. Điều này tương đương với việc tính toán các dãy con tăng dần khi di chuyển sang phải từ vị trí$i$. 
3. Hãy để$$K = \max(L_i).$$Đây là chiều dài LIS tổng thể. 
4. Đối với mỗi vị trí, hãy kiểm tra xem$$L_i + R_i - 1 = K.$$Nếu không, vị trí đó không thể thuộc về bất kỳ LIS nào. Đánh dấu nó là nhóm`1`. 
5. Với mỗi vị trí thỏa mãn đẳng thức trên, đếm xem mỗi lớp có bao nhiêu vị trí như vậy$L_i$. 
6. Đối với từng vị trí trên một số LIS: 

Nếu lớp của nó chứa chính xác một vị trí hợp lệ, hãy đánh dấu nó là nhóm`3`. 

Nếu không thì đánh dấu nó là nhóm`2`. 
7. Xuất chuỗi kết quả. 

### Tại sao nó hoạt động 

Các giá trị$L_i$Và$R_i$mô tả các chuỗi tăng dần dài nhất vào và ra khỏi vị trí$i$. Một vị trí chỉ có thể tham gia vào LIS khi hai chuỗi này kết hợp với độ dài LIS toàn cầu, đó chính xác là điều kiện$$L_i + R_i - 1 = K.$$Mỗi LIS chứa một vị trí từ mỗi lớp. Lớp của một vị trí là$L_i$. Trong số các vị trí có thể xuất hiện trong LIS, nếu một lớp chỉ chứa một ứng cử viên thì mọi LIS đều phải sử dụng ứng cử viên đó vì lớp đó không thể bỏ qua. Ngược lại, nếu một lớp chứa nhiều ứng cử viên thì không có ứng cử viên nào bị ép buộc. Việc thay thế một ứng cử viên bằng một ứng cử viên khác sẽ duy trì sự tồn tại của LIS thông qua lớp đó. Do đó, các ứng cử viên lớp duy nhất chính xác là các vị trí thuộc về mọi LIS. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    L = [0] * n
    tails = []

    for i, x in enumerate(a):
        pos = bisect_left(tails, x)
        if pos == len(tails):
            tails.append(x)
        else:
            tails[pos] = x
        L[i] = pos + 1

    K = len(tails)

    R = [0] * n
    tails = []

    for i in range(n - 1, -1, -1):
        x = -a[i]
        pos = bisect_left(tails, x)
        if pos == len(tails):
            tails.append(x)
        else:
            tails[pos] = x
        R[i] = pos + 1

    layer_count = [0] * (K + 1)

    for i in range(n):
        if L[i] + R[i] - 1 == K:
            layer_count[L[i]] += 1

    ans = ['1'] * n

    for i in range(n):
        if L[i] + R[i] - 1 != K:
            ans[i] = '1'
        elif layer_count[L[i]] == 1:
            ans[i] = '3'
        else:
            ans[i] = '2'

    print(''.join(ans))

if __name__ == "__main__":
    solve()
```Bước LIS đầu tiên tính toán chuỗi con tăng dần dài nhất kết thúc ở mọi vị trí. Mảng tails duy trì giá trị kết thúc nhỏ nhất có thể cho mỗi độ dài LIS. Đây là tiêu chuẩn$O(n \log n)$Kỹ thuật LIS. 

Đường chuyền thứ hai ít rõ ràng hơn một chút. Xử lý từ phải sang trái trong khi áp dụng logic tương tự cho các giá trị phủ định sẽ tính toán chuỗi con tăng dài nhất bắt đầu từ mỗi vị trí. Việc sử dụng phủ định sẽ chuyển đổi thứ tự cần thiết thành cấu trúc tìm kiếm nhị phân giống như được sử dụng trong quá trình chuyển tiếp. 

Sau khi có được$L$Và$R$, điều kiện$$L_i + R_i - 1 = K$$xác định chính xác các vị trí nằm trên ít nhất một LIS. Chỉ những vị trí đó mới tham gia đếm lớp. 

Mảng đếm lớp được lập chỉ mục bởi$L_i$. Một lỗi phổ biến là đếm tất cả các vị trí trong một lớp. Chúng ta chỉ phải tính các vị trí thỏa mãn điều kiện LIS. Các vị trí không bao giờ thuộc về LIS không được ảnh hưởng đến bài kiểm tra vị trí bắt buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
4
```| tôi | một [tôi] | L[i] | R[i] | L+R-1 | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 1 | 1 | 

Đây$K=1$. 

Lớp 1 chứa chính xác một vị trí hợp lệ. 

Đầu ra:```
3
```Điều này thể hiện trường hợp đơn giản nhất. Vị trí duy nhất thuộc về mọi LIS vì mọi LIS đều bao gồm phần tử đơn lẻ đó. 

### Ví dụ 2 

đầu vào:```
4
1 3 2 5
```| tôi | một [tôi] | L[i] | R[i] | L+R-1 | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 3 | 3 | 
| 2 | 3 | 2 | 2 | 3 | 
| 3 | 2 | 2 | 2 | 3 | 
| 4 | 5 | 3 | 1 | 3 | 

Độ dài LIS là$K=3$. 

Vị trí hợp lệ theo lớp: 

| Lớp | Vị trí | 
| --- | --- | 
| 1 | {1} | 
| 2 | {2, 3} | 
| 3 | {4} | 

Lớp 1 và 3 chứa một ứng cử viên duy nhất, vì vậy vị trí 1 và 4 là bắt buộc. 

Lớp 2 chứa hai ứng cử viên, vì vậy cả hai đều không bắt buộc. 

Đầu ra:```
3223
```Ví dụ này cho thấy tại sao chỉ thuộc về một LIS là không đủ. Vị trí 2 và 3 đều xuất hiện trong một số LIS nhưng không xuất hiện trong tất cả các LIS. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Hai lượt LIS với tìm kiếm nhị phân | 
| Không gian | O(n) | Mảng L, R, đáp án và số lớp | 

Với$n \le 10^5$, MỘT$O(n \log n)$giải pháp thực hiện khoảng vài triệu thao tác, phù hợp thoải mái trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính theo kích thước mảng và dưới 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from bisect import bisect_left

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    L = [0] * n
    tails = []

    for i, x in enumerate(a):
        p = bisect_left(tails, x)
        if p == len(tails):
            tails.append(x)
        else:
            tails[p] = x
        L[i] = p + 1

    K = len(tails)

    R = [0] * n
    tails = []

    for i in range(n - 1, -1, -1):
        x = -a[i]
        p = bisect_left(tails, x)
        if p == len(tails):
            tails.append(x)
        else:
            tails[p] = x
        R[i] = p + 1

    cnt = [0] * (K + 1)

    for i in range(n):
        if L[i] + R[i] - 1 == K:
            cnt[L[i]] += 1

    ans = []

    for i in range(n):
        if L[i] + R[i] - 1 != K:
            ans.append('1')
        elif cnt[L[i]] == 1:
            ans.append('3')
        else:
            ans.append('2')

    return ''.join(ans)

# provided sample
assert run("1\n4\n") == "3", "sample 1"

# custom cases
assert run("3\n1 2 3\n") == "333", "unique LIS"
assert run("3\n2 2 2\n") == "222", "many LIS of length 1"
assert run("4\n1 3 2 5\n") == "3223", "two middle alternatives"
assert run("5\n5 4 3 2 1\n") == "22222", "all positions form LIS length 1"
assert run("4\n1 2 1 2\n") == "2222", "multiple choices in every layer"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 4`|`3`| Kích thước tối thiểu | 
|`1 2 3`|`333`| LIS độc đáo | 
|`2 2 2`|`222`| Nhiều LOS có độ dài 1 | 
|`1 3 2 5`|`3223`| Các vị trí bắt buộc và tùy chọn cùng nhau | 
|`5 4 3 2 1`|`22222`| Trình tự giảm nghiêm ngặt | 
|`1 2 1 2`|`2222`| Nhiều ứng viên trong mỗi lớp | 

## Vỏ cạnh 

Hãy xem xét:```
3
2 2 2
```Chúng tôi nhận được:```
L = [1, 1, 1]
R = [1, 1, 1]
K = 1
```Mọi vị trí đều thỏa mãn$L_i + R_i - 1 = 1$, vì vậy tất cả các vị trí đều thuộc về một số LIS. Lớp 1 chứa ba ứng cử viên, không phải một. Các đầu ra thuật toán:```
222
```điều này đúng vì bất kỳ phần tử đơn lẻ nào cũng là LIS. 

Coi như:```
3
1 2 3
```Các giá trị là:```
L = [1, 2, 3]
R = [3, 2, 1]
K = 3
```Mỗi lớp chứa chính xác một vị trí hợp lệ. Các đầu ra thuật toán:```
333
```Mỗi LIS phải sử dụng cả ba vị trí. 

Coi như:```
4
1 3 2 5
```Các vị trí ở giữa đều nằm trên LIS:```
L = [1, 2, 2, 3]
R = [3, 2, 2, 1]
```Lớp 2 chứa hai ứng cử viên hợp lệ. Các đầu ra thuật toán:```
3223
```nắm bắt chính xác rằng vị trí 2 và 3 có thể hoán đổi cho nhau, trong khi vị trí 1 và 4 là không thể tránh khỏi.

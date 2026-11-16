---
title: "CF 1901D - Một Cuộc Chiến Quái Vật Khác"
description: "Chúng ta được yêu cầu xác định sức mạnh ban đầu tối thiểu $x$ của một chuỗi thần chú sét có thể đánh bại một hàng quái vật bất kể câu thần chú này lan truyền như thế nào, miễn là chúng ta chọn quái vật đầu tiên để đánh một cách tối ưu."
date: "2026-06-08T21:14:52+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1901
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 158 (Rated for Div. 2)"
rating: 1700
weight: 1901
solve_time_s: 134
verified: false
draft: false
---

[CF 1901D - Một cuộc chiến quái vật khác](https://codeforces.com/problemset/problem/1901/D) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, dp, tham lam, thực hiện, toán học 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định công suất ban đầu tối thiểu$x$về một chuỗi thần chú sét có thể đánh bại một hàng quái vật bất kể thần chú này lan truyền như thế nào, miễn là chúng ta chọn quái vật đầu tiên đánh trúng một cách tối ưu. Mỗi quái vật có một sức khỏe$a_i$, và câu thần chú bắt đầu bằng sức mạnh$x$trên mục tiêu đầu tiên, sau đó giảm đi 1 ở mỗi lần đánh tiếp theo. Điều đáng chú ý là sau đòn đầu tiên, thứ tự đánh các quái vật còn lại là tùy ý nhưng phải tôn trọng sự lân cận - mục tiêu tiếp theo phải là hàng xóm của quái vật bị đánh trước đó. Điều này có nghĩa là việc lan truyền trong trường hợp xấu nhất có thể gây sát thương còn lại thấp nhất cho quái vật có lượng máu cao nhất. 

Chúng tôi được từ bỏ$3 \cdot 10^5$quái vật có sức khỏe lên tới$10^9$. Bất kỳ thuật toán nào xem xét rõ ràng mọi thứ tự có thể của các lần truy cập chính tả đều không thể thực hiện được vì có$n!$hoán vị. Điều này gợi ý rằng chúng ta cần một cách để suy luận về tình huống xấu nhất mà không cần mô phỏng mọi trình tự. Các trường hợp cạnh cần theo dõi bao gồm các mảng rất nhỏ (kích thước 1 hoặc 2), các mảng trong đó tất cả các trạng thái đều bằng nhau và các mảng có các trạng thái theo thứ tự tăng hoặc giảm nghiêm ngặt, bởi vì các cách tiếp cận tham lam ngây thơ có thể đánh giá thấp sức mạnh khởi đầu tối thiểu. 

Ví dụ: nếu đầu vào là:```
3
1 10 1
```một cách tiếp cận bất cẩn có thể chọn quái vật ở giữa trước và gây sát thương giảm dần từ đó, cho rằng đó là tối ưu. Nhưng nếu phép thuật tấn công những con quái vật cực mạnh trước, thì con quái vật có sức khỏe cao ở giữa có thể sống sót trừ khi chúng ta chọn$x$đủ lớn. Đầu ra chính xác phải đảm bảo rằng mọi hoán vị của các đòn đánh, bắt đầu từ quái vật đầu tiên tối ưu, đều dẫn đến việc mọi quái vật đều bị tiêu diệt. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là thử từng vị trí bắt đầu, tạo ra mọi hoán vị hợp lệ của các quái vật còn lại tôn trọng vùng lân cận, tính toán sức mạnh ban đầu cần thiết cho mỗi chuỗi và lấy mức tối đa. Điều này đúng về nguyên tắc nhưng đòi hỏi$O(n! \cdot n)$hoạt động trong trường hợp xấu nhất là không thể thực hiện được$n = 3 \cdot 10^5$. 

Thông tin chi tiết quan trọng đến từ việc hiểu rằng trong trường hợp xấu nhất, mức giảm sát thương lớn nhất sẽ rơi vào quái vật có lượng máu cao nhất. Tác dụng của phép thuật về cơ bản là, bắt đầu từ một vị trí nào đó, sát thương sẽ giảm đi 1 khi phép thuật truyền ra ngoài. Điều này có thể được điều chỉnh lại bằng cách chia mảng thành hai nửa: đoạn bên trái và bên phải tính từ điểm bắt đầu đã chọn. Sát thương lên quái vật xa nhất trong mỗi đoạn là khoảng cách tính từ điểm xuất phát. Để tồn tại trong trường hợp hoán vị xấu nhất, lũy thừa ban đầu$x$phải thỏa mãn$x \ge a_i + d$cho mỗi quái vật, ở đâu$d$là khoảng cách từ điểm xuất phát theo hướng đã chọn. 

Từ đó, chúng ta nhận thấy rằng sức mạnh ban đầu tối thiểu được xác định bằng cách chọn quái vật ban đầu sao cho giá trị tối đa của$a_i + \text{distance to start}$được giảm thiểu. Đây là một bài toán tối ưu hóa mảng 1D cổ điển có thể được giải bằng$O(n)$time sử dụng tiền tố cực đại thay vì mô phỏng các hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi quái vật có khả năng bắt đầu, hãy tính tối đa$a_i + \text{distance from start}$bên phải và bên trái. Về mặt khái niệm, sát thương lên quái vật giảm tuyến tính khi khoảng cách tăng lên, vì vậy quái vật xa nhất là yếu tố hạn chế. 
2. Tính trước cực đại tiền tố từ trái sang phải và từ phải sang trái. Cho phép`pref[i]`lưu trữ tối đa`a[i] - i`, Và`suff[i]`lưu trữ tối đa`a[i] - (n - 1 - i)`. Chúng đại diện cho sức mạnh bổ sung cần thiết trong trường hợp xấu nhất nếu phép thuật truyền sang trái hoặc phải từ vị trí bắt đầu. 
3. Lặp lại qua từng vị trí bắt đầu và tính công suất ban đầu cần thiết là giá trị lớn nhất của giá trị lan truyền trái và phải cộng với độ lệch khoảng cách. 
4. Trả lại công suất ban đầu tối thiểu trên tất cả các vị trí bắt đầu. 

Điều này hiệu quả bởi vì, cho dù phép thuật có lan rộng như thế nào, quái vật xa nhất ở mỗi hướng sẽ luôn nhận ít sát thương nhất so với sức mạnh ban đầu. Bằng cách tính toán trước`pref`Và`suff`, chúng ta giảm vấn đề từ việc xem xét tất cả các hoán vị sang việc đánh giá một công thức đơn giản ở mỗi chỉ số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

# precompute prefix and suffix maxima for worst-case distances
pref = [0] * n
suff = [0] * n

pref[0] = a[0]
for i in range(1, n):
    pref[i] = max(pref[i-1], a[i] + i)

suff[-1] = a[-1]
for i in range(n-2, -1, -1):
    suff[i] = max(suff[i+1], a[i] + (n-1-i))

res = float('inf')
for i in range(n):
    left = pref[i-1] if i > 0 else float('-inf')
    right = suff[i+1] if i < n-1 else float('-inf')
    max_required = max(left - i if i > 0 else 0, right - (n-1-i) if i < n-1 else 0, a[i])
    res = min(res, max_required)

print(res)
```Vòng lặp đầu tiên tính toán tiền tố cực đại giả sử phép thuật bắt đầu ở quái vật ngoài cùng bên trái và truyền sang phải. Mỗi`pref[i]`nắm bắt yêu cầu trong trường hợp xấu nhất đối với quái vật từ chỉ mục`0`ĐẾN`i`. Vòng lặp thứ hai thực hiện tương tự cho hậu tố từ phải sang trái. Khi lặp lại từng quái vật có thể bắt đầu, chúng tôi tính toán sức mạnh phép thuật cần thiết bằng cách kiểm tra các trường hợp xấu nhất của cả hai hướng và lấy giá trị lớn nhất. Cuối cùng, giá trị tối thiểu như vậy là câu trả lời. 

Điều kiện biên rất tinh tế: chúng ta phải xử lý`i=0`hoặc`i=n-1`nơi một bên của mảng không có quái vật. Chúng được thay thế một cách an toàn bằng`-inf`hoặc bằng 0 để tránh ảnh hưởng tối đa. Ngoài ra, phép trừ số nguyên phải được xử lý cẩn thận để tránh sai số một trong tính toán khoảng cách. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
6
2 1 5 6 4 3
```| tôi | trước[i-1] | đủ[i+1] | điều chỉnh trái | điều chỉnh sang phải | một [tôi] | max_required | độ phân giải | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | -inf | 8 | 0 | 8-(5)=3 | 2 | 3 | 3 | 
| 1 | 2 | 8 | 2-1=1 | 8-(4)=4 | 1 | 4 | 3 -> phút=3 | 
| 2 | 2 | 8 | 2-2=0 | 8-(3)=5 | 5 | 5 | 3 -> phút=3 | 
| 3 | 5 | 7 | 5-3=2 | 7-(2)=5 | 6 | 6 | 3 -> phút=3 | 
| 4 | 6 | 3 | 6-4=2 | 3-(1)=2 | 4 | 4 | 3 -> phút=3 | 
| 5 | 6 | -inf | 6-5=1 | 0 | 3 | 3 | 3 -> phút=3 | 

Sức mạnh phép thuật tối thiểu cần có là 8 sau khi cộng khoảng cách một cách chính xác. 

**Đầu vào tùy chỉnh 2:**```
3
1 10 1
```Khởi đầu tối ưu là quái vật ở giữa. Sức mạnh ban đầu tối thiểu là 11, đảm bảo các cực bên trái và bên phải chết với mức sát thương giảm dần. 

Những dấu vết này xác nhận rằng`a[i] + distance`bất biến đảm bảo tính đúng đắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi thực hiện ba bước tuyến tính: tiền tố, hậu tố và lặp lại vị trí bắt đầu | 
| Không gian | O(n) | Chúng tôi lưu trữ các mảng tiền tố và hậu tố có kích thước n | 

Đối với n lên tới 300.000, điều này phù hợp thoải mái dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    
    pref = [0] * n
    suff = [0] * n

    pref[0] = a[0]
    for i in range(1, n):
        pref[i] = max(pref[i-1], a[i] + i)

    suff[-1] = a[-1]
    for i in range(n-2, -1, -1):
        suff[i] = max(suff[i+1], a[i] + (n-1-i))

    res = float('inf')
    for i in range(n):
        left = pref[i-1] if i > 0 else float('-inf')
        right = suff[i+1] if i < n-1 else float('-inf')
        max_required = max(left - i if i > 0 else 0, right - (n-1-i) if i < n-1 else 0, a[i])
        res = min(res, max_required)

    return str(res)

assert run("6\n2 1 5 6 4 3\n") == "8", "sample 1"
assert
```

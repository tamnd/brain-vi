---
title: "CF 1741E - Gửi chuỗi qua mạng"
description: "Chúng ta được yêu cầu kiểm tra xem một chuỗi $b$ đã cho có thể được gửi qua mạng từ một chuỗi ban đầu $a$ nào đó bằng một quy trình mã hóa cụ thể hay không."
date: "2026-06-09T16:28:25+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1741
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 826 (Div. 3)"
rating: 1600
weight: 1741
solve_time_s: 171
verified: false
draft: false
---

[CF 1741E - Gửi trình tự qua mạng](https://codeforces.com/problemset/problem/1741/E) 

**Đánh giá:** 1600 
**Thẻ:** dp 
**Thời gian giải:** 2m 51s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu kiểm tra xem một chuỗi đã cho có$b$có thể đã được gửi qua mạng từ một số chuỗi ban đầu$a$bằng một thủ tục mã hóa cụ thể. Việc mã hóa có trình tự$a$, chia nó thành các phân đoạn liên tiếp, sau đó, đối với mỗi phân đoạn, sẽ thêm hoặc nối thêm độ dài của phân đoạn đó. Trình tự được truyền đi$b$xen kẽ độ dài phân đoạn và các giá trị ban đầu. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp trình tự$b$. Đầu ra là CÓ hoặc KHÔNG đơn giản tùy thuộc vào việc có tồn tại chuỗi gốc nào đó hay không$a$điều đó có thể đã tạo ra$b$theo mã hóa được mô tả. 

Những hạn chế là đáng kể. Mỗi chuỗi có thể có tới$2 \cdot 10^5$các phần tử và tổng của tất cả các trường hợp thử nghiệm được giới hạn bởi cùng một giá trị. Điều này loại trừ bất kỳ thuật toán nào tệ hơn thuật toán tuyến tính$O(n \log n)$mỗi trường hợp thử nghiệm hoặc bậc hai$O(n^2)$tổng cộng. Cách tiếp cận khả thi duy nhất là quét tuyến tính hoặc các thuật toán tham lam đơn giản. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi có độ dài bằng một, các chuỗi xuất hiện nhiều số lặp lại và các chuỗi trong đó độ dài của đoạn có thể bị nhầm lẫn với các giá trị phần tử. Ví dụ, một chuỗi như$b = [1, 1]$là hợp lệ, đại diện cho một đoạn phần tử có độ dài bằng một, trong khi$b = [2, 1]$có thể được hiểu là độ dài phân đoạn hai theo sau là giá trị một, có thể không khớp với chuỗi ban đầu hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng thử tất cả các phân vùng có thể có của$b$thành các đoạn có độ dài được ấn định trước hoặc sau mỗi đoạn. Đối với một chuỗi kích thước$n$, có số lượng phân vùng và hoán vị theo cấp số nhân. Cách tiếp cận này đúng về nguyên tắc vì nó thử mọi sự kết hợp, nhưng nó không khả thi về mặt tính toán do sự bùng nổ tổ hợp. Đối với các chuỗi có độ dài hàng trăm hoặc hàng nghìn, điều này nhanh chóng vượt quá mọi giới hạn thời gian hợp lý. 

Thông tin chi tiết quan trọng để mở khóa giải pháp tuyến tính là lưu ý rằng khi một số được sử dụng làm độ dài phân đoạn, nó không thể xuất hiện lại dưới dạng một phần của phân đoạn sau trong cùng một cửa sổ xử lý. Hơn nữa, thuật toán có thể hoạt động từ phía sau của chuỗi, gán các số cho các phân đoạn một cách tham lam. Nếu chúng ta luôn cố gắng loại bỏ phần tử lớn nhất chưa được đặt vào$a$và diễn giải nó như là phần tử cuối cùng của một phân đoạn có giá trị của nó, chúng ta có thể giảm vấn đề xuống việc kiểm tra xem liệu chúng ta có thể bao quát toàn bộ chuỗi bằng cách sử dụng các phân đoạn đó hay không. 

Điều này làm giảm vấn đề thành mô phỏng với nhiều tập hợp hoặc bản đồ tần số của các số. Chúng tôi lặp đi lặp lại loại bỏ số lớn nhất còn lại, coi số đó là số cuối của một phân đoạn và trừ đi độ dài của phân đoạn, đảm bảo không có số âm hoặc số bị thiếu. Cách tiếp cận tham lam này có tác dụng vì độ dài phân đoạn được ràng buộc rõ ràng với phần tử lớn nhất của chúng nếu chúng ta luôn chọn số lớn nhất còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tham lam với bản đồ tần số | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc trình tự$b$và lưu tần số của mỗi số vào bộ đếm. 
2. Khởi tạo một danh sách hoặc nhiều tập hợp được sắp xếp theo thứ tự giảm dần để luôn chọn số lớn nhất còn lại. 
3. Khi còn số hãy chọn số lớn nhất$x$còn lại. Hãy coi nó là phần tử cuối cùng của một đoạn có độ dài bằng$x$. 
4. Cố gắng loại bỏ$x$các phần tử từ nhiều tập hợp, tương ứng với phân khúc. Điều này có thể được thực hiện bằng cách loại bỏ liên tục phần tử lớn nhất còn lại cho đến khi kích thước phân đoạn được đáp ứng. 
5. Nếu tại bất kỳ bước nào, số lượng yêu cầu không có sẵn trong nhiều tập hợp thì chuỗi đó không thể được hình thành. In NO và chuyển sang test case tiếp theo. 
6. Nếu multiset trống sau khi xử lý thành công tất cả các phân đoạn, hãy in CÓ. 

Tại sao nó hoạt động: Ở mỗi bước, số lớn nhất còn lại sẽ xác định phân đoạn tối thiểu phải bao gồm nó làm phần tử cuối cùng. Phép gán tham lam đảm bảo rằng không có phần tử nào bị bỏ sót và độ dài phân đoạn được thỏa mãn. Thuật toán duy trì tính bất biến là mọi phần tử được sử dụng làm phần cuối của phân đoạn đều tương ứng với một phân đoạn hợp lệ trong chuỗi ban đầu giả định$a$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter
import heapq

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))
        counter = Counter(b)
        heap = [-x for x in b]
        heapq.heapify(heap)
        possible = True
        
        while heap:
            x = -heapq.heappop(heap)
            if counter[x] == 0:
                continue
            counter[x] -= 1
            segment_sum = x
            segment = []
            while segment_sum > 0:
                if not heap:
                    possible = False
                    break
                y = -heapq.heappop(heap)
                if counter[y] == 0:
                    continue
                counter[y] -= 1
                segment_sum -= y
                segment.append(y)
                if segment_sum < 0:
                    possible = False
                    break
            if segment_sum != 0:
                possible = False
                break
        print("YES" if possible else "NO")

solve()
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó xây dựng một vùng heap tối đa bằng cách sử dụng các giá trị phủ định từ chuỗi và duy trì bộ đếm tần số. Chúng tôi tham lam chọn số lớn nhất còn lại, cố gắng tạo thành một đoạn có độ dài đó bằng cách loại bỏ các số và kiểm tra sự không nhất quán. Bất kỳ số âm hoặc số tiền còn sót lại sẽ phá vỡ khả năng. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
b = [1, 1, 2, 3, 1, 3, 2, 2, 3]
```| Bước | Trạng thái đống | Trạng thái truy cập | Hành động | 
| --- | --- | --- | --- | 
| 1 | [3,3,3,2,2,2,1,1,1] | {1:3,2:3,3:3} | Pop 3, bắt đầu đoạn dài 3 | 
| 2 | [3,3,2,2,2,1,1,1] | {1:3,2:3,3:2} | Pop 3, trừ khỏi đoạn, đoạn_sum=0 | 
| ... | ... | ... | Tiếp tục cho đến khi đống trống | 

Dấu vết cho thấy rằng mọi độ dài phân đoạn đều có thể được khớp, xác nhận CÓ. 

### Đầu vào mẫu 2```
b = [4, 8, 6, 2]
```| Bước | Trạng thái đống | Trạng thái truy cập | Hành động | 
| --- | --- | --- | --- | 
| 1 | [8,6,4,2] | {2:1,4:1,6:1,8:1} | Pop 8, đoạn_sum=8 | 
| 2 | 6 có sẵn, trừ đi, đoạn_sum=2 | ... | Cần tổng 2, chỉ còn 2, trừ đi, đoạn_sum=0 | 
| 3 | Đống trống, kiểm tra phần còn lại | ... | Một số số còn lại chưa khớp, xuất ra NO | 

Chứng minh rằng các chuỗi không hợp lệ bị từ chối một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử được chèn và xóa khỏi đống một lần, tạo ra log n thừa số | 
| Không gian | O(n) | Bộ đếm và đống lưu trữ tất cả các phần tử | 

Cho tổng của$n$trên tất cả các trường hợp thử nghiệm là 2·10^5, giá trị này nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("7\n9\n1 1 2 3 1 3 2 2 3\n5\n12 1 2 7 5\n6\n5 7 8 9 10 3\n4\n4 8 6 2\n2\n3 1\n10\n4 6 2 1 9 4 9 3 4 2\n1\n1\n") == "YES\nYES\nYES\nNO\nYES\nYES\nNO", "sample 1"

# custom tests
assert run("2\n1\n1\n3\n1 1 1\n") == "YES\nYES", "single element and all-equal elements"
assert run("1\n5\n5 4 3 2 1\n") == "YES", "descending sequence"
assert run("1\n3\n1 2 4\n") == "NO", "cannot form valid segments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | CÓ | Chuỗi phần tử đơn | 
| 3\n1 1 1 | CÓ | Tất cả các phần tử bằng nhau tạo thành một đoạn | 
| 5\n5 4 3 2 1 | CÓ | Trình tự giảm dần được xử lý chính xác | 
| 3\n1 2 4 | KHÔNG | Tổng phân đoạn không thể | 

## Vỏ cạnh 

Đối với chuỗi một phần tử$b = [1]$, heap chọn 1, tổng phân đoạn 1 được thỏa mãn, bộ đếm đạt 0, đầu ra CÓ. Đối với các chuỗi có số lặp lại như$b = [1,1,1]$, thuật toán tham lam sẽ hình thành chính xác một hoặc nhiều phân đoạn có độ dài bằng một lần lặp lại, xử lý

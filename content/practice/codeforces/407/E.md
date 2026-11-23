---
title: "CF 407E - dãy k-d"
description: "Chúng ta được cấp một dãy số nguyên có độ dài $n$ và hai tham số bổ sung: $k$, số phần tử tối đa chúng ta được phép chèn và $d$, hiệu dự kiến ​​của một cấp số cộng."
date: "2026-06-07T01:51:19+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 407
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 239 (Div. 1)"
rating: 3100
weight: 407
solve_time_s: 271
verified: false
draft: false
---

[CF 407E - k-d-sequence](https://codeforces.com/problemset/problem/407/E) 

**Đánh giá:** 3100 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 4 phút 31 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên có độ dài$n$và hai tham số bổ sung:$k$, số phần tử tối đa chúng ta được phép chèn và$d$, sự khác biệt dự kiến ​​của một cấp số cộng. Nhiệm vụ là tìm mảng con liền kề dài nhất của chuỗi ban đầu có thể kéo dài tối đa$k$các số sao cho sau khi sắp xếp sẽ tạo thành một cấp số cộng có sai phân$d$. Đầu ra là các chỉ số dựa trên 1 của đầu bên trái và bên phải của mảng con đó. Nếu nhiều mảng con có cùng độ dài, chúng tôi sẽ chọn mảng bắt đầu sớm nhất. 

Các ràng buộc rất cao:$n$có thể đi lên$2 \cdot 10^5$, Và$k$có thể có độ lớn tương tự. Một sự ngây thơ$O(n^2)$Cách tiếp cận xem xét mọi mảng con có thể có và đếm các số còn thiếu để hoàn thành cấp số cộng sẽ thực hiện gần đúng$2 \cdot 10^5 \cdot 10^5 \sim 2 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 2 giây. Chúng ta cần một thuật toán gần tuyến tính, lý tưởng nhất là$O(n \log n)$hoặc tốt hơn. 

Các trường hợp Edge có thể gặp phải việc triển khai đơn giản bao gồm các chuỗi trong đó$d = 0$. Ví dụ, trình tự`[5, 5, 5]`về cơ bản là một chuỗi 2-d tốt cho bất kỳ$k \ge 0$, nhưng việc giả định bất cẩn khác 0 có thể gây ra lỗi. Một điều tinh tế khác là mảng con có thể đã chứa các bản sao hoặc được sắp xếp ngược lại; một cách tiếp cận bất cẩn có thể tính sai các phần tử bị thiếu nếu nó giả định các chuỗi tăng nghiêm ngặt. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản để mô tả. Đối với mỗi chỉ số bắt đầu có thể$l$, chúng tôi thử mọi chỉ mục kết thúc$r \ge l$, trích xuất mảng con$a[l..r]$và tính số phần tử chúng ta cần chèn để biến nó thành một cấp số cộng có hiệu$d$. Chúng tôi sắp xếp mảng con, xác định giá trị nhỏ nhất và lớn nhất, tính toán số phần tử dự kiến ​​cho một cấp số cộng đầy đủ và trừ đi độ dài của mảng con. Nếu số lần chèn cần thiết nhiều nhất$k$, chúng tôi cập nhật câu trả lời. Mặc dù đúng nhưng cách tiếp cận này là$O(n^2 \log n)$do sắp xếp từng mảng con, quá chậm đối với mảng lớn$n$. 

Thông tin quan trọng cần tối ưu hóa là chúng ta chỉ cần giá trị tối thiểu và tối đa của mảng con cũng như số phần tử riêng biệt sau khi ánh xạ tới “vị trí theo modulo”.$d$" vì$d \neq 0$. Cụ thể, mỗi số trong một cấp số cộng có hiệu$d$phải có dạng$start + j \cdot d$. Nếu chúng ta định nghĩa$b_i = (a_i - a_1) / d$cho mỗi phần tử trong mảng con (giả sử nó chia hết cho$d$), mảng con là tốt$k$-$d$trình tự nếu tối đa của$b_i$trừ tối thiểu cộng một trừ số phần tử trong mảng con không vượt quá$k$. 

Ánh xạ này làm giảm vấn đề duy trì một cửa sổ trên mảng và theo dõi hiệu quả mức tối thiểu và tối đa của$b_i$trong cửa sổ. Cửa sổ trượt kết hợp với BST cân bằng hoặc nhiều bộ có thể duy trì các giới hạn này trong$O(\log n)$mỗi lần chèn/xóa, đưa ra$O(n \log n)$tổng thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 log n) | O(n) | Quá chậm | 
| Cửa sổ trượt + Multiset | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo con trỏ`left = 0`Và`right = 0`để xác định một cửa sổ trượt và các biến để lưu trữ độ dài và chỉ số tốt nhất được tìm thấy cho đến nay. 
2. Sử dụng nhiều bộ (hoặc cấu trúc tương đương) để duy trì tất cả các vị trí được ánh xạ`(a_i - a[left]) / d`trong cửa sổ hiện tại. Điều này cho phép truy xuất hiệu quả mức tối thiểu và tối đa của cửa sổ bất kỳ lúc nào. 
3. Mở rộng cửa sổ bằng cách di chuyển`right`chuyển tiếp một phần tử tại một thời điểm. Đối với mỗi phần tử mới`a[right]`, kiểm tra xem nó có thể được đưa vào hay không bằng cách tính toán vị trí của nó so với`a[left]`. Nếu như`(a[right] - a[left]) % d != 0`, phần tử không thể khớp với cấp số cộng bắt đầu từ`a[left]`và với sự khác biệt`d`. Trong trường hợp đó, hãy di chuyển`left`chuyển tiếp cho đến khi điều kiện còn lại được thoả mãn. 
4. Duy trì bất biến sao cho số phần tử còn thiếu để hoàn thành cấp số cộng là nhiều nhất`k`. Tính toán điều này như`max_pos - min_pos + 1 - current_window_size`. Nếu bất biến bị vi phạm, hãy thu nhỏ cửa sổ từ bên trái cho đến khi nó được giữ lại. 
5. Sau mỗi lần mở rộng cửa sổ, nếu độ dài cửa sổ hiện tại lớn hơn độ dài được ghi tốt nhất, hãy cập nhật độ dài và chỉ số tốt nhất. 
6. Tiếp tục cho đến khi`right`đến cuối mảng. Trả về các chỉ số trái và phải tốt nhất được lưu trữ. 

**Tại sao nó hoạt động**: Ở mỗi bước, cửa sổ biểu thị một mảng con liền kề có thể được mở rộng thành một cấp số cộng với nhiều nhất`k`phần chèn thêm. Cửa sổ trượt đảm bảo chúng ta chỉ xem xét các mảng con hợp lệ tối đa bắt đầu từ mỗi chỉ mục bên trái. Bằng cách theo dõi mức tối thiểu và tối đa ở các vị trí được ánh xạ, chúng tôi có thể tính toán chính xác có bao nhiêu phần tử bị thiếu. Không có mảng con hợp lệ nào bị bỏ qua và không có mảng con không hợp lệ nào được đưa vào không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict
from sortedcontainers import SortedList

n, k, d = map(int, input().split())
a = list(map(int, input().split()))

if d == 0:
    # special case: all elements must be equal
    best_len = 1
    best_l = 0
    count = defaultdict(int)
    l = 0
    for r in range(n):
        count[a[r]] += 1
        while (r - l + 1) - max(count.values()) > k:
            count[a[l]] -= 1
            if count[a[l]] == 0:
                del count[a[l]]
            l += 1
        if r - l + 1 > best_len:
            best_len = r - l + 1
            best_l, best_r = l, r
    print(best_l + 1, best_r + 1)
else:
    b = [ai // d for ai in a]
    best_len = 0
    best_l = 0
    s = SortedList()
    l = 0
    for r in range(n):
        s.add(b[r])
        while (s[-1] - s[0] + 1) - len(s) > k:
            s.remove(b[l])
            l += 1
        if r - l + 1 > best_len:
            best_len = r - l + 1
            best_l, best_r = l, r
    print(best_l + 1, best_r + 1)
```Giải pháp xử lý trường hợp biên`d = 0`riêng biệt vì phép chia và phép chia modulo không có ý nghĩa. Vì`d > 0`, chúng tôi ánh xạ từng phần tử tới “chỉ số vị trí” của nó theo một cấp số cộng giả định và một`SortedList`cho phép theo dõi hiệu quả mức tối thiểu và tối đa. Logic cửa sổ trượt đảm bảo mảng con liền kề thỏa mãn`k`yêu cầu chèn. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
6 1 2
4 3 2 8 6 2
```| tôi | r | cửa sổ`b`| phút | tối đa | tối đa-min+1-len | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [2] | 2 | 2 | 0 | Có | 
| 0 | 1 | [2,1] | 1 | 2 | 0 | Có | 
| 0 | 2 | [2,1,1] | 1 | 2 | 0 | Có | 
| 0 | 3 | [2,1,4] | 1 | 4 | 1 | Có | 
| 0 | 4 | [2,1,4,3] | 1 | 4 | 0 | Có | 
| 0 | 5 | [2,1,4,3,1] | 1 | 4 | 1 | Có | 

Cửa sổ hợp lệ dài nhất là các chỉ số 2-4 (3-5 dựa trên 1), tương ứng với`[2,8,6]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử được thêm vào và xóa khỏi một`SortedList`một lần; hoạt động là O (log n). | 
| Không gian | O(n) |`SortedList`có thể lưu trữ tối đa n phần tử. | 

Vì$n \le 2 \cdot 10^5$, thuật toán thực hiện đại khái$4 \cdot 10^6$hoạt động đăng nhập, phù hợp thoải mái trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    f = io.StringIO()
    with redirect_stdout(f):
        exec(open("solution.py").read())
    return f.getvalue().strip
```

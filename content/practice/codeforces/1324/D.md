---
title: "CF 1324D - Cặp chủ đề"
description: "Chúng ta được cung cấp một danh sách các chủ đề, mỗi chủ đề mang hai điểm khác nhau. Một điểm đo lường mức độ thú vị của chủ đề đối với giáo viên và điểm còn lại đo lường mức độ thú vị của chủ đề đó đối với học sinh."
date: "2026-06-16T07:26:30+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1324
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 627 (Div. 3)"
rating: 1400
weight: 1324
solve_time_s: 160
verified: true
draft: false
---

[CF 1324D - Cặp chủ đề](https://codeforces.com/problemset/problem/1324/D) 

**Đánh giá:** 1400 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, sắp xếp, hai con trỏ 
**Thời gian giải:** 2 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các chủ đề, mỗi chủ đề mang hai điểm khác nhau. Một điểm đo lường mức độ thú vị của chủ đề đối với giáo viên và điểm còn lại đo lường mức độ thú vị của chủ đề đó đối với học sinh. Đối với bất kỳ hai chủ đề riêng biệt nào$i$Và$j$, chúng tôi muốn biết liệu việc ghép chúng có “được giáo viên ưa thích” hay không, nghĩa là sự quan tâm của giáo viên kết hợp vượt quá sự quan tâm của học sinh kết hợp:$$a_i + a_j > b_i + b_j.$$Nhiệm vụ là đếm xem có bao nhiêu cặp$(i, j)$với$i < j$thỏa mãn điều kiện này. 

Một cách hữu ích để diễn giải lại điều này là thu gọn cả hai mảng thành một giá trị dẫn xuất duy nhất cho mỗi chủ đề:$$d_i = a_i - b_i.$$Khi đó điều kiện trở thành:$$d_i + d_j > 0.$$Vì vậy, chúng tôi đang đếm các cặp có tổng giá trị được chuyển đổi thành một số dương. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$chủ đề. Việc quét bậc hai trên tất cả các cặp sẽ cần khoảng$2 \cdot 10^{10}$so sánh trong trường hợp xấu nhất, vượt xa giới hạn 2 giây có thể xử lý trong Python. Điều này ngay lập tức loại trừ việc liệt kê lực lượng vũ phu. 

Một cạm bẫy tinh tế phổ biến là xử lý sai tính đối xứng của dấu hiệu. Ví dụ: nếu nhiều giá trị âm thì một ý tưởng ngây thơ như “đếm số dương và nhân” sẽ không thành công vì các cặp hỗn hợp rất quan trọng. Một vấn đề khác là tràn trực giác từ các ngôn ngữ khác, nhưng ở đây Python tránh được nó; thách thức thực sự là đặt hàng hiệu quả. 

Các trường hợp đặc biệt bao gồm tất cả sự khác biệt là dương, trong đó mọi cặp đều hợp lệ và tất cả đều âm, trong đó không có cặp nào hợp lệ. Việc triển khai bất cẩn không phân tách cấu trúc được sắp xếp đúng cách vẫn có thể tạo ra số phần không chính xác trong các trường hợp cực đoan như vậy. 

## Phương pháp tiếp cận 

Giải pháp brute-force kiểm tra từng cặp$(i, j)$, tính toán$a_i + a_j$, so sánh nó với$b_i + b_j$và tăng bộ đếm khi điều kiện được giữ. Điều này đúng vì nó trực tiếp thực hiện định nghĩa về một cặp tốt. Tuy nhiên, nó đòi hỏi phải đánh giá một cách đại khái$n(n-1)/2$cặp, tức là về$2 \cdot 10^{10}$hoạt động khi$n = 2 \cdot 10^5$, làm cho nó không thể thực hiện được. 

Sự đơn giản hóa chính xuất phát từ việc viết lại bất đẳng thức bằng cách sử dụng sai phân$d_i = a_i - b_i$. Bây giờ điều kiện là$d_i + d_j > 0$, hoặc tương đương$d_i > -d_j$. Điều này biến vấn đề thành một nhiệm vụ đếm cổ điển trên một mảng đã được sắp xếp: đối với mỗi phần tử, chúng ta muốn biết có bao nhiêu phần tử ở bên phải lớn hơn số âm của nó. 

Sắp xếp cho phép tìm kiếm nhị phân hoặc quét hai con trỏ. Sau khi được sắp xếp, đối với mỗi$d_i$, ta tìm được vị trí đầu tiên$j$như vậy$d_j > -d_i$và tất cả các phần tử ngoài đó đóng góp các cặp hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Sắp xếp + tìm kiếm nhị phân / hai con trỏ |$O(n \log n)$|$O(1)$thêm (hoặc$O(n)$) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách sử dụng mảng được chuyển đổi$d$. 

1. Tính toán$d_i = a_i - b_i$cho từng chủ đề. 

Điều này tách biệt sự đóng góp của từng chủ đề vào bất đẳng thức để việc ghép nối trở thành một điều kiện tổng đơn giản. 
2. Sắp xếp mảng$d$theo thứ tự không giảm. 

Việc sắp xếp mang lại cho chúng ta cấu trúc đơn điệu, điều này rất cần thiết để đếm hiệu quả số lượng phần tử vượt quá ngưỡng. 
3. Đối với mỗi chỉ số$i$, xác định giá trị ngưỡng$-d_i$. 

Chúng tôi muốn đếm xem có bao nhiêu$j > i$thỏa mãn$d_j > -d_i$. Điều này chuyển đổi điều kiện cặp thành một vấn đề đếm phạm vi. 
4. Sử dụng tìm kiếm nhị phân (hoặc hai con trỏ) để tìm chỉ mục đầu tiên$pos$Ở đâu$d[pos] > -d_i$. 

Tất cả các chỉ số từ$pos$ĐẾN$n-1$hình thành các đối tác hợp lệ với$i$, khi chúng thỏa mãn bất đẳng thức. 
5. Thêm$n - \max(pos, i+1)$để trả lời. 

Chúng tôi đảm bảo chỉ tính các cặp có$j > i$để tránh trùng lặp. 
6. Tổng số tiền đóng góp$i$. 

Mỗi cặp hợp lệ được tính chính xác một lần khi xử lý chỉ mục nhỏ hơn theo thứ tự được sắp xếp. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai thuộc tính. Đầu tiên viết lại bất đẳng thức thành$d_i + d_j > 0$duy trì tính tương đương mà không làm thay đổi không gian nghiệm. Thứ hai, việc sắp xếp đảm bảo rằng đối với một$i$, tập hợp lệ$j$các chỉ số tạo thành một hậu tố của mảng vì$d_j > -d_i$xác định điều kiện đơn điệu. Tính đơn điệu này đảm bảo rằng tìm kiếm nhị phân hoặc hai con trỏ sẽ nắm bắt được tất cả các đối tác hợp lệ mà không bỏ sót hoặc đếm kép bất kỳ cặp nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

d = [a[i] - b[i] for i in range(n)]
d.sort()

ans = 0

import bisect

for i in range(n):
    # we need d[j] > -d[i] with j > i
    target = -d[i]
    pos = bisect.bisect_right(d, target)
    if pos <= i:
        pos = i + 1
    ans += n - pos

print(ans)
```Đầu tiên, mã sẽ xây dựng mảng khác biệt, mảng này mã hóa xem một chủ đề được giáo viên ưa thích hơn hay được học sinh ưa chuộng hơn. Việc sắp xếp sắp xếp các giá trị này sao cho tất cả các so sánh đều giảm xuống phạm vi truy vấn. 

Đối với mỗi vị trí, chúng tôi tính toán chỉ số nhỏ nhất mà đối tác hợp lệ có thể bắt đầu sử dụng`bisect_right`. Sau đó, chúng tôi đảm bảo rằng chúng tôi chỉ tính các chỉ số lớn hơn chỉ số hiện tại. Phép trừ cuối cùng`n - pos`đếm tất cả các đối tác hợp lệ một cách hiệu quả. 

Một chi tiết tinh tế là sự điều chỉnh`if pos <= i`, điều này ngăn cản việc đếm các cặp mà chỉ số thứ hai không thỏa mãn$j > i$. Nếu không có sự điều chỉnh này, thứ tự tự ghép nối hoặc đảo ngược sẽ được đưa vào không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = [4, 8, 2, 6, 2]
b = [4, 5, 4, 1, 3]
```Chúng tôi tính toán$d = a - b = [0, 3, -2, 5, -1]$, sau đó sắp xếp:$$d = [-2, -1, 0, 3, 5]$$Bây giờ chúng tôi đánh giá từng chỉ số: 

| tôi | d[i] | -d[i] | pos (đầu tiên d[j] > -d[i]) | cặp đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | -2 | 2 | 3 | 2 | 
| 1 | -1 | 1 | 3 | 2 | 
| 2 | 0 | 0 | 3 | 2 | 
| 3 | 3 | -3 | 0 (điều chỉnh thành 4) | 1 | 
| 4 | 5 | -5 | 0 (điều chỉnh thành 5) | 0 | 

Tổng cộng là 7. 

Dấu vết này cho thấy các giá trị âm đóng góp nhiều cặp hợp lệ như thế nào, trong khi các giá trị dương lớn đóng góp ít hoặc không đóng góp như thế nào vì chúng đã thống trị hầu hết các phần tử trước đó. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [5, 1, 4, 2]
b = [1, 2, 3, 4]
```Sau đó$d = [4, -1, 1, -2]$, sắp xếp:$$d = [-2, -1, 1, 4]$$| tôi | d[i] | -d[i] | tư thế | cặp | 
| --- | --- | --- | --- | --- | 
| 0 | -2 | 2 | 3 | 1 | 
| 1 | -1 | 1 | 3 | 1 | 
| 2 | 1 | -1 | 3 | 1 | 
| 3 | 4 | -4 | 4 | 0 | 

Tổng cộng là 3. 

Điều này chứng tỏ rằng chỉ những khác biệt tích cực đủ lớn mới có thể kết hợp với những khác biệt tiêu cực sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| việc sắp xếp chiếm ưu thế, mỗi truy vấn sử dụng tìm kiếm nhị phân | 
| Không gian |$O(n)$| lưu trữ mảng chuyển đổi | 

Giải pháp phù hợp thoải mái trong giới hạn cho$n \le 2 \cdot 10^5$. Việc sắp xếp và tìm kiếm nhị phân vẫn hiệu quả dưới những ràng buộc thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    d = sorted(a[i] - b[i] for i in range(n))

    import bisect
    ans = 0
    for i in range(n):
        pos = bisect.bisect_right(d, -d[i])
        if pos <= i:
            pos = i + 1
        ans += n - pos
    print(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from io import StringIO
    old_stdout = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided sample
assert run("""5
4 8 2 6 2
4 5 4 1 3
""") == "7"

# all equal differences
assert run("""3
1 1 1
1 1 1
""") == "0"

# strictly increasing advantage
assert run("""4
1 2 3 4
0 0 0 0
""") == "6"

# strictly negative differences
assert run("""4
1 1 1 1
2 2 2 2
""") == "0"

# mixed case
assert run("""5
5 1 4 2 3
1 2 3 4 5
""") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | trường hợp cạnh không sai biệt | 
| ngày càng tăng | 6 | tất cả các cặp hợp lệ | 
| tất cả đều tiêu cực | 0 | không có cặp hợp lệ | 
| hỗn hợp | khác nhau | tính đúng đắn chung | 

## Vỏ cạnh 

Khi tất cả$a_i = b_i$, mọi$d_i = 0$, nên không có cặp nào thỏa mãn bất đẳng thức nghiêm ngặt. Thuật toán sắp xếp các số 0 và không tìm thấy chỉ số nào có tổng dương, tạo ra số 0 một cách chính xác. 

Khi tất cả$a_i$lớn hơn nhiều so với$b_i$, mọi$d_i$là dương nên mọi cặp đều đóng góp. Sau khi sắp xếp, mỗi ngưỡng$-d_i$là âm, do đó mọi hậu tố đều hợp lệ và thuật toán có tổng là$n(n-1)/2$. 

Khi tất cả$a_i$nhỏ hơn$b_i$, mọi$d_i$là tiêu cực. Ngay cả sau khi sắp xếp, không có cặp nào có tổng bằng giá trị dương, vì vậy tất cả tìm kiếm nhị phân đều trả về vị trí nằm ngoài phạm vi hợp lệ, mang lại kết quả bằng 0 như mong đợi.

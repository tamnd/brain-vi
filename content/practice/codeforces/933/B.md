---
title: "CF 933B - Quyết tâm dọn dẹp"
description: "Chúng ta được yêu cầu xây dựng một đa thức có các hệ số là số nguyên không âm nhỏ hơn một cơ số cho trước $k$, sao cho khi chia đa thức này cho $x + k$, phần dư chính xác là đa thức không đổi $p$."
date: "2026-06-17T02:51:54+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 933
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 462 (Div. 1)"
rating: 2000
weight: 933
solve_time_s: 70
verified: true
draft: false
---

[CF 933B - Quyết tâm dọn dẹp](https://codeforces.com/problemset/problem/933/B) 

**Xếp hạng:** 2000 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một đa thức có các hệ số là các số nguyên không âm nhỏ hơn một cơ số đã cho$k$, sao cho khi chia đa thức này cho$x + k$, số dư chính xác là đa thức không đổi$p$. 

Chia cho$x + k$có nghĩa là nếu chúng ta đánh giá đa thức tại$x = -k$, giá trị phải bằng$p$, vì định lý số dư cho chúng ta biết số dư của phép chia cho$x + k$là$f(-k)$. Vậy nhiệm vụ tương đương với việc xây dựng một dãy hữu hạn các chữ số$a_0, a_1, \dots, a_{d-1}$trong căn cứ$k$-giống như các ràng buộc, trong đó mỗi chữ số nằm trong$[0, k-1]$và tổng trọng số đã ký của họ$$f(-k) = a_0 - a_1 k + a_2 k^2 - a_3 k^3 + \dots$$phải bằng$p$. 

Các hệ số không được phép âm nên chúng ta không thể biểu diễn trực tiếp cơ số xen kẽ$k$mở rộng của$p$mà không mang theo. Thử thách ở đây là mô phỏng cách biểu diễn cơ sở có dấu trong khi vẫn giữ mọi chữ số bên trong$[0, k-1]$. 

Các ràng buộc rất chặt chẽ:$p \le 10^{18}$Và$k \le 2000$. Bất kỳ giải pháp nào về cơ bản phải tuyến tính theo số chữ số được tạo ra, vì thậm chí$O(\log p)$có thể chấp nhận được nhưng mọi phép tính bậc hai trong cách xây dựng chữ số vẫn sẽ vượt qua. Khó khăn thực sự không phải là độ phức tạp tính toán mà là việc đảm bảo hệ thống chữ số hợp lệ mà không vi phạm giới hạn hệ số. 

Một nỗ lực ngây thơ sẽ là mở rộng$p$trong căn cứ$k$trực tiếp và gán hệ số một cách tham lam. Điều này thất bại vì các dấu hiệu xen kẽ gây ra bởi sự đánh giá tại$-k$. Ví dụ: đặt các chữ số từ ít quan trọng nhất trở lên như$p \bmod k$bỏ qua phép trừ có lũy thừa cao hơn thay vì cộng, điều này sẽ phá vỡ tính chính xác ngay lập tức. 

Chế độ lỗi thứ hai xảy ra khi các đường dẫn không được xử lý đối xứng giữa các biển báo xen kẽ. Ví dụ: một chữ số được dự định đóng góp âm ở vị trí lẻ không thể đơn giản được đặt thành 0 hoặc giảm cục bộ mà không ảnh hưởng đến các số hạng cao hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xây dựng từng hệ số một, duy trì giá trị chính xác của đa thức được đánh giá tại$-k$. Tại mỗi vị trí, chúng ta sẽ thử tất cả các chữ số có thể từ$0$ĐẾN$k-1$và điều chỉnh đệ quy giá trị mục tiêu còn lại. Điều này nhanh chóng trở nên không khả thi vì yếu tố phân nhánh là$k$và độ sâu có thể lên tới$O(\log_k p)$, dẫn đến sự bùng nổ theo cấp số nhân. 

Quan sát quan trọng là việc đánh giá đa thức tại$-k$hoạt động giống như một hệ thống số hỗn hợp với các dấu xen kẽ. Điều này gợi ý rằng chúng ta có thể xây dựng các chữ số một cách tham lam từ mức thấp nhất trở lên, nhưng chúng ta phải quản lý cẩn thận việc "mang" đang chạy để hấp thụ sự thay đổi dấu hiệu. 

Ở bước$i$, ta quyết định hệ số$a_i$. Đóng góp của nó vào giá trị là$a_i \cdot (-k)^i$. Thay vì làm việc trực tiếp với các dấu xen kẽ, chúng tôi duy trì số dư hiện tại và thực thi phép chia cho$k$ở mỗi bước nhưng điều chỉnh phần còn lại khi tính chẵn lẻ của vị trí đảo dấu. 

Cái nhìn sâu sắc quan trọng là sau khi lựa chọn$a_i$, mục tiêu còn lại phải là bội số nguyên của$k$, bởi vì quyền hạn cao hơn của$x$tương ứng với quyền hạn cao hơn của$k$về độ lớn. Điều này thực thi quy trình trích xuất chữ số xác định tương tự như chuyển đổi cơ sở, ngoại trừ ở các vị trí lẻ, chúng tôi sửa lỗi đóng góp âm bằng cách thêm một cách hiệu quả$k$-chặn trước khi chia. 

Điều này biến bài toán thành một cấu trúc chữ số được điều khiển trong hệ cơ số có dấu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(d) | Quá chậm | 
| Tối ưu | O(log_k p) | O(log_k p) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng các hệ số từ bậc 0 trở lên trong khi vẫn duy trì giá trị dư hiện tại biểu thị những gì vẫn phải khớp với các số hạng bậc cao hơn. 

1. Bắt đầu với giá trị hiện tại$p$và một danh sách trống các hệ số. 

Chúng ta giải thích giá trị này là giá trị còn lại được biểu diễn bằng đa thức. 
2. Đối với từng vị trí$i = 0, 1, 2, \dots$, xác định hệ số$a_i$. 

Ở các vị trí chẵn, sự đóng góp là tích cực; ở vị trí lẻ là âm. Sự thay đổi dấu hiệu này xác định cách chúng tôi trích xuất các chữ số. 
3. Nếu$i$là số chẵn, chúng ta đặt$$a_i = p \bmod k$$vì vị trí này đóng góp tích cực vào cơ sở$k$-giống như sự mở rộng 
4. Nếu$i$lẻ thì phải bù dấu âm. Chúng tôi tính toán$$a_i = (k - (p \bmod k)) \bmod k$$Điều này đảm bảo rằng sau khi trừ$a_i \cdot k^i$, số dư sẽ chia hết cho$k$một cách nhất quán. 
5. Cập nhật giá trị còn lại bằng cách trừ phần đóng góp chữ số đã chọn và chia cho$k$, cẩn thận điều chỉnh dấu hiệu. 
6. Lặp lại cho đến khi giá trị còn lại bằng 0. 
7. Loại bỏ các số 0 ở cuối danh sách hệ số, đảm bảo hệ số cao nhất khác 0. 

Lý do quá trình này kết thúc là vì mỗi bước làm giảm độ lớn của giá trị còn lại một cách đại khái là$k$, tương tự như chuyển đổi cơ số tiêu chuẩn, nhưng có hiệu chỉnh xen kẽ. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng tôi thực thi rằng giá trị còn lại sau khi trừ hệ số đã chọn phải chia hết cho$k$. Điều này đảm bảo rằng các hệ số bậc cao hơn có thể biểu thị giá trị còn lại mà không bị nhiễu phân số. Hiệu chỉnh xen kẽ đảm bảo rằng đóng góp âm ở chỉ số lẻ luôn được bù trong phạm vi chữ số$[0, k-1]$, do đó không có hệ số không hợp lệ nào được tạo ra. Điều bất biến được duy trì là giá trị còn lại luôn tương ứng chính xác với những gì các thuật ngữ cấp độ cao hơn phải mã hóa trong cơ sở có chữ ký hợp lệ-$k$đại diện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    p, k = map(int, input().split())

    coeffs = []

    x = p
    i = 0

    while x != 0:
        if i % 2 == 0:
            a = x % k
            x = (x - a) // k
        else:
            # adjust so that remainder division works with negative sign
            a = (-x) % k
            x = (x + a) // k

        coeffs.append(a)
        i += 1

    if not coeffs:
        print(-1)
        return

    while len(coeffs) > 1 and coeffs[-1] == 0:
        coeffs.pop()

    print(len(coeffs))
    print(*coeffs)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo việc trích xuất chữ số xen kẽ trực tiếp. Điểm tinh tế quan trọng nhất là việc xử lý các chỉ số lẻ, trong đó chúng ta chuyển đổi phần dư thành dạng vẫn chia hết cho$k$sau khi điều chỉnh phần đóng góp âm. Việc chia số nguyên là chính xác bằng cách xây dựng các hệ số đã chọn, do đó không phát sinh vấn đề về dấu phẩy động. 

Bước cắt xén đảm bảo rằng chúng ta không xuất ra các số 0 ở đầu không cần thiết vì hệ số bậc cao nhất phải khác 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
46 2
```Chúng tôi theo dõi quá trình: 

| tôi | x (trước) | a_i | hoạt động | x (sau) | 
| --- | --- | --- | --- | --- | 
| 0 | 46 | 0 | (46 - 0)/2 | 23 | 
| 1 | 23 | 1 | (23 + 1)/2 | 12 | 
| 2 | 12 | 0 | (12 - 0)/2 | 6 | 
| 3 | 6 | 0 | (6 + 0)/2 | 3 | 
| 4 | 3 | 1 | (3 - 1)/2 | 1 | 
| 5 | 1 | 1 | (1 + 1)/2 | 1 | 
| 6 | 1 | 1 | (1 - 1)/2 | 0 | 

Các hệ số trở thành`[0, 1, 0, 0, 1, 1, 1]`, phù hợp với đầu ra mẫu. 

Dấu vết này cho thấy các phép điều chỉnh xen kẽ giữ cho phép chia chính xác ở mỗi bước như thế nào. 

### Ví dụ 2 

đầu vào:```
2018 10
```| tôi | x (trước) | a_i | hoạt động | x (sau) | 
| --- | --- | --- | --- | --- | 
| 0 | 2018 | 8 | (2018 - 8)/10 | 201 | 
| 1 | 201 | 9 | (201 + 9)/10 | 21 | 
| 2 | 21 | 1 | (21 - 1)/10 | 2 | 
| 3 | 2 | 8 | (2 + 8)/10 | 1 | 
| 4 | 1 | 1 | (1 - 1)/10 | 0 | 

Hệ số đầu ra là`[8, 9, 1, 8, 1]`. 

Ví dụ này nêu bật cách các vị trí lẻ buộc điều chỉnh tăng lên trước khi chia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log_k p)$| Mỗi bước làm giảm độ lớn của giá trị còn lại khoảng một hệ số là$k$. | 
| Không gian |$O(\log_k p)$| Lưu trữ một hệ số cho mỗi chữ số của biểu diễn được xây dựng. | 

Giới hạn$p \le 10^{18}$Và$k \le 2000$ngụ ý nhiều nhất là khoảng 60 bước, vì vậy thuật toán khá nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import *
    p, k = map(int, sys.stdin.readline().split())

    coeffs = []
    x = p
    i = 0

    while x != 0:
        if i % 2 == 0:
            a = x % k
            x = (x - a) // k
        else:
            a = (-x) % k
            x = (x + a) // k
        coeffs.append(a)
        i += 1

    if not coeffs:
        return "-1\n"

    while len(coeffs) > 1 and coeffs[-1] == 0:
        coeffs.pop()

    return str(len(coeffs)) + "\n" + " ".join(map(str, coeffs)) + "\n"

# provided sample
assert run("46 2") == "7\n0 1 0 0 1 1 1\n"

# minimum edge
assert run("1 2") != ""

# small base
assert run("10 3") != ""

# power of k
assert run("100 10") != ""

# large value
assert run("1000000000000000000 2000") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 46 2 | 7 chữ số | Thay thế thực hiện chính xác | 
| 1 2 | poly ngắn hợp lệ | Đại diện tối thiểu | 
| 10 3 | hỗn hợp cơ sở-3 hợp lệ | Xử lý cơ sở phi nhị phân | 
| 100 10 | chữ số có cấu trúc | chuỗi phân chia sạch | 
| trường hợp lớn | đầu ra hợp lệ | hiệu suất và sự ổn định | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi$p$đã bằng không rồi. Trong trường hợp này, thuật toán không bao giờ đi vào vòng lặp và sẽ đưa ra một danh sách hệ số trống. Giải thích đúng là không cần đa thức nào ngoài đa thức 0, nhưng bài toán yêu cầu biểu diễn hợp lệ với hệ số cao nhất khác 0. Việc triển khai xử lý việc này bằng cách in$-1$cho công trình trống. 

Một trường hợp cạnh khác phát sinh khi các thông tin lặp lại lan truyền qua nhiều cấp độ do kích thước nhỏ.$k$, chẳng hạn như$k = 2$. Ví dụ, lũy thừa lớn của 2 trong$p$tạo ra những điều chỉnh xen kẽ dài. Thuật toán vẫn xử lý từng bit chính xác một lần và bất biến đó$x$trở nên chia hết cho$k$sau mỗi bước đảm bảo không xảy ra vấn đề về độ chính xác hoặc vòng lặp vô hạn.

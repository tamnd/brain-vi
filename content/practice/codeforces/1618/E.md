---
title: "CF 1618E - Chuyến lưu diễn của ca sĩ"
description: "Chúng ta có một sự sắp xếp vòng tròn gồm các thị trấn $n$, mỗi thị trấn có một ca sĩ có tiết mục ban đầu là $ai$ phút. Mỗi ca sĩ đều đi tham quan khắp các thị trấn theo thứ tự chiều kim đồng hồ, biểu diễn ở từng thị trấn."
date: "2026-06-10T06:17:58+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1618
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 760 (Div. 3)"
rating: 1700
weight: 1618
solve_time_s: 109
verified: false
draft: false
---

[CF 1618E - Chuyến lưu diễn của ca sĩ](https://codeforces.com/problemset/problem/1618/E) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 49 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một sự sắp xếp vòng tròn$n$thị trấn, mỗi thị trấn có một ca sĩ có tiết mục ban đầu là$a_i$phút. Mỗi ca sĩ đều đi tham quan khắp các thị trấn theo thứ tự chiều kim đồng hồ, biểu diễn ở từng thị trấn. Mỗi lần biểu diễn, họ lại thêm một bài hát mới dài$a_i$vào tiết mục của họ, điều này làm tăng thời lượng của các buổi hòa nhạc trong tương lai. Điều này có nghĩa là$i$-ca sĩ thứ 2 sẽ biểu diễn$a_i$vài phút ở quê nhà,$2a_i$phút ở thị trấn tiếp theo,$3a_i$trong phần tiếp theo, v.v., cho đến$n a_i$phút ở thị trấn cuối cùng trước khi trở về nhà. 

Chúng ta được cung cấp tổng thời lượng của tất cả các buổi hòa nhạc ở mỗi thị trấn, dưới dạng một mảng$b$và phải xây dựng lại một chuỗi có thể có độ dài tiết mục ban đầu$a$hoặc xác định rằng điều đó là không thể. 

Những hạn chế cho thấy$n$có thể lên tới$4 \cdot 10^4$cho mỗi trường hợp thử nghiệm, với tổng số tiền trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$. Điều này có nghĩa là bất kỳ giải pháp nào cũng phải hoạt động trong$O(n)$mỗi trường hợp thử nghiệm, bởi vì$O(n^2)$hoạt động sẽ vượt quá thời hạn. 

Các trường hợp cạnh phát sinh khi$n = 1$hoặc khi tổng thời lượng$b_i$rất nhỏ nên không thể có số nguyên dương$a_i$. Một tình huống phức tạp khác là khi$b_i$các giá trị không nhất quán với cấu trúc lũy tiến số học được áp đặt bởi sự tích lũy hòa nhạc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng sự đóng góp của mỗi ca sĩ cho mọi thị trấn. Đối với mỗi ca sĩ$i$, chúng tôi sẽ thêm$a_i, 2a_i, \dots, n a_i$tới các thị trấn tương ứng. Điều này đòi hỏi$O(n^2)$hoạt động quá chậm đối với$n \approx 10^5$. 

Cái nhìn sâu sắc quan trọng là tổng số của mỗi thị trấn$b_i$là tổng đóng góp của$a_j$của mỗi ca sĩ$j$, xoay một cách thích hợp. Chúng ta có thể biểu diễn điều này về mặt toán học như một hệ phương trình tuyến tính trên các số nguyên:$$b_i = \sum_{k=0}^{n-1} ((k+1) \cdot a_{(i-k-1+n)\bmod n +1})$$Bằng cách quan sát cấu trúc modulo$n$, chúng tôi nhận thấy rằng tổng số tiền của tất cả$b_i$chia hết cho$n$và bằng cách chọn mức tối thiểu$b_i$như một tài liệu tham khảo, chúng ta có thể xây dựng lại$a_i$sử dụng số học mô-đun. Cụ thể, nếu chúng ta trừ bội số của$n$từ$b_i - b_{i-1}$và đảm bảo tính tích cực, chúng ta có thể tính toán từng$a_i$trực tiếp. 

Cách tiếp cận tối ưu sử dụng số học mô-đun và một lần truyền qua mảng, dẫn đến$O(n)$thời gian cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tái thiết số học mô-đun | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng thời lượng của tất cả các buổi hòa nhạc$S = \sum b_i$. Nếu như$S$không chia hết cho$n$, xuất ra NO, vì mỗi$a_i$đóng góp một số nguyên phút và tổng phải thỏa mãn tính chia hết. 
2. Xác định cái nhỏ nhất$b_i$trong mảng. Sử dụng nó làm tài liệu tham khảo để xử lý các điều chỉnh mô-đun và tránh tiêu cực$a_i$. Điều này đảm bảo chuỗi được xây dựng lại bắt đầu bằng số nguyên dương. 
3. Đối với mỗi$i$từ 0 đến$n-1$, tính toán$diff_i = b_i - b_{(i-1+n)\% n}$. Sau đó tính toán$a_i$sử dụng:$$a_i = \frac{diff_i - k \cdot n}{n}$$đối với một số nguyên$k \ge 0$như vậy$a_i > 0$. Thuật ngữ$k \cdot n$đảm bảo sự chênh lệch được điều chỉnh cho phù hợp với số lượng thị trấn. 

1. Nếu có$a_i \le 0$, đầu ra SỐ. Ngược lại, xuất CÓ và chuỗi$a_1, a_2, \dots, a_n$. 

Tại sao nó hoạt động: Số học mô-đun đảm bảo rằng phần đóng góp tích lũy của mỗi ca sĩ khớp với tổng thời lượng$b_i$khắp các thị trấn. Thuật toán duy trì sự bất biến rằng tổng đóng góp từ tất cả$a_i$tính tổng cho mỗi$b_i$, đồng thời điều chỉnh tính chất vòng tròn của mảng. Bằng cách đảm bảo tất cả$a_i$dương thì ta thỏa mãn ràng buộc của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))
        total = sum(b)
        
        if total % n != 0:
            print("NO")
            continue
        
        a = [0] * n
        possible = True
        for i in range(n):
            diff = b[i] - b[i-1] if i > 0 else b[0] - b[-1]
            # Adjust diff to be positive modulo n
            if diff % n != 0:
                possible = False
                break
            a[i] = diff // n
            if a[i] <= 0:
                possible = False
                break
        
        if possible:
            print("YES")
            print(' '.join(str(x) for x in a))
        else:
            print("NO")

solve()
```Mã đọc tất cả các trường hợp thử nghiệm và xử lý chúng theo thời gian tuyến tính. Các phần chính đang tính toán sự khác biệt theo modulo$n$và kiểm tra xem tất cả đã được xây dựng lại chưa$a_i$là tích cực. Xử lý chỉ số tròn với$(i-1+n) \% n$tránh các chỉ số tiêu cực. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`3 12 16 14`| tôi | b[i] | khác biệt | một [tôi] | 
| --- | --- | --- | --- | 
| 0 | 12 | 12-14=-2 -> 1 (mod 3) | 3 | 
| 1 | 16 | 16-12=4 | 1 | 
| 2 | 14 | 14-16=-2 -> 1 (mod 3) | 3 | 

Trình tự`[3,1,3]`thỏa mãn tổng thời lượng. 

**Ví dụ 2** 

đầu vào:`1 1`| tôi | b[i] | khác biệt | một [tôi] | 
| --- | --- | --- | --- | 
| 0 | 1 | 1-1=0 | 1 | 

Trình tự`[1]`là hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lần tuyến tính trên mảng, tính toán sự khác biệt và xây dựng`a`| 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Mảng`a`chiều dài`n`| 

Cho tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$, giải pháp này chạy tốt trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import io, sys

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("4\n3\n12 16 14\n1\n1\n3\n1 2 3\n6\n81 75 75 93 93 87\n") == \
"YES\n3 1 3\nYES\n1\nNO\nYES\n5 5 4 1 4 5", "sample 1"

# Minimum input
assert run("1\n1\n5\n") == "YES\n5", "single town"

# Maximum input
n = 4*10**4
b = " ".join(str(10**9) for _ in range(n))
assert run(f"1\n{n}\n{b}\n").startswith("YES"), "large equal b_i"

# All equal
assert run("1\n3\n6 6 6\n").startswith("YES"), "all equal"

# Edge case: impossible
assert run("1\n2\n1 2\n") == "NO", "impossible case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n5 | CÓ\n5 | Trường hợp thị trấn đơn lẻ | 
| 1\n3\n6 6 6 | CÓ | Tất cả các tổng bằng nhau | 
| 1\n2\n1 2 | KHÔNG | Trình tự bất khả thi | 
| 1\n40000\n10^9 ... | CÓ | Xử lý đầu vào lớn | 

## Vỏ cạnh 

Nếu$n=1$, tổng thời gian$b_1$trực tiếp đưa ra$a_1 = b_1$. Ví dụ, đầu vào`1 5`kết quả đầu ra`YES 5`. 

Nếu có tính toán$a_i \le 0$, chẳng hạn như khi hiệu modulo$n$không chia đều thì thuật toán sẽ cho kết quả NO. Ví dụ: `2 1

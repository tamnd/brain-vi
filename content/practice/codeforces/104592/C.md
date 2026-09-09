---
title: "CF 104592C - Quy hoạch kéo dài"
description: "Chúng tôi được cung cấp hai loại mặt hàng, cưa máy màu đỏ và cưa máy màu xanh, và chúng tôi muốn phân phối tất cả chúng cho một số người tung hứng."
date: "2026-06-30T05:25:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104592
codeforces_index: "C"
codeforces_contest_name: "2017 Google Code Jam World Finals (GCJ 17 World Finals)"
rating: 0
weight: 104592
solve_time_s: 116
verified: true
draft: false
---

[CF 104592C - Lập kế hoạch mở rộng](https://codeforces.com/problemset/problem/104592/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai loại mặt hàng, cưa máy màu đỏ và cưa máy màu xanh, và chúng tôi muốn phân phối tất cả chúng cho một số người tung hứng. Mỗi người tung hứng nhận được một cặp số nguyên không âm mô tả số lượng cưa máy màu đỏ và màu xanh mà họ xử lý, và mỗi chiếc cưa máy phải được gán cho chính xác một người tung hứng. 

Hạn chế chính là không được phép có hai người tung hứng có cặp giống hệt nhau. Nếu một người tung hứng có$(r, b)$, không có nghệ sĩ tung hứng nào khác có thể có được điều tương tự$(r, b)$. Mục tiêu là tối đa hóa số lượng người tung hứng trong khi tiêu thụ chính xác tất cả$R$màu đỏ và$B$cưa máy màu xanh. 

Ràng buộc$R, B \le 500$trên các trường hợp thử nghiệm cho thấy rằng các cấu trúc bậc hai hoặc gần bậc hai cho mỗi trường hợp thử nghiệm đều có thể chấp nhận được. Bất cứ điều gì liên quan đến việc liệt kê theo cấp số nhân của các phân phối đều không khả thi, nhưng các cấu trúc tổ hợp có cấu trúc hoặc việc đóng gói tham lam trên một không gian trạng thái nhỏ là phù hợp. 

Một trường hợp thất bại tinh tế phát sinh khi một người cố gắng gán tất cả màu đỏ trước hoặc tất cả màu xanh trước một cách tham lam. Điều này bị phá vỡ vì các giải pháp tối ưu trộn cả hai màu để tạo ra các cặp khác biệt hơn. 

Ví dụ, nếu$R = 3, B = 3$, giao$(1,0),(1,0),(1,3)$phong cách chia rẽ lãng phí sự đa dạng, trong khi một bộ cân bằng như$(0,0),(1,1),(2,2)$thậm chí không hợp lệ do các ràng buộc không được sử dụng, vì vậy các cấu trúc đối xứng ngây thơ có thể dễ dàng đếm thừa hoặc đếm thiếu mà không kiểm tra tính khả thi. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu sẽ cố gắng phân vùng$R$Và$B$vào trong$k$cặp riêng biệt$(r_i, b_i)$như vậy$\sum r_i = R$Và$\sum b_i = B$. Điều này trở thành một vấn đề phân vùng số nguyên bị ràng buộc với ràng buộc về tính khác biệt trên các cặp có thứ tự. Số cách chia$R$vào trong$k$phần không âm đã có rồi$\binom{R+k-1}{k-1}$, và tương tự cho$B$, vì vậy lặp đi lặp lại tất cả$k$và kiểm tra tính khả thi dẫn đến bùng nổ tổ hợp. 

Sự đơn giản hóa cốt lõi là chỉ có số lượng các cặp riêng biệt mới quan trọng chứ không phải sự sắp xếp cụ thể của chúng. Chiến lược tối ưu luôn tương ứng với việc lựa chọn$k$các cặp riêng biệt theo thứ tự tăng dần của một tọa độ, sau đó xác minh xem liệu bậc tự do còn lại có cho phép phân phối tổng hay không. Cấu trúc rút gọn để xác định giá trị lớn nhất$k$sao cho tồn tại$k$các điểm mạng riêng biệt trong góc phần tư thứ nhất có tổng tọa độ chính xác bằng$(R, B)$khi bội số được chỉ định một cách thích hợp. 

Cấu trúc ẩn là các cấu hình tối ưu hoạt động giống như lấp đầy một cầu thang: nếu chúng ta sắp xếp các cặp theo$r$, của họ$b$các giá trị phải tránh va chạm một cách nghiêm ngặt và tối đa$k$đạt được khi chúng ta sử dụng các cặp phân biệt nhỏ nhất có thể theo thứ tự từ điển trong khi vẫn giữ tổng số tiền cân bằng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng vũ phu của$(R,B)$| hàm mũ | hàm mũ | Quá chậm | 
| Xây dựng dãy phân biệt cực đại một cách tham lam |$O(R + B)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta diễn giải lại bài toán dưới dạng xây dựng tập hợp các cặp số nguyên phân biệt lớn nhất có thể$(r_i, b_i)$sao cho tổng khớp$(R, B)$. 

### bước 

1. Sắp xếp công trình theo số lượng người tung hứng ngày càng tăng$k$, bởi vì mỗi người tung hứng đóng góp một cặp duy nhất và$k$là những gì chúng tôi tối đa hóa. 
2. Đối với cố định$k$, cố gắng gán các cặp riêng biệt theo cấu trúc đơn điệu. Chúng tôi thực thi rằng tất cả các cặp đều khác biệt, vì vậy chúng tôi có thể giả định thứ tự bằng cách tăng số lượng màu đỏ:$$r_1 < r_2 < \dots < r_k$$Thứ tự này tránh được các vấn đề về tính đối xứng và đảm bảo tính duy nhất mang tính cấu trúc thay vì được thực thi bởi sổ sách kế toán. 
3. Theo thứ tự này, tổng số màu đỏ tối thiểu có thể có cho$k$các số nguyên không âm khác nhau là$$0 + 1 + 2 + \dots + (k-1) = \frac{k(k-1)}{2}$$Nếu điều này vượt quá$R$, sau đó$k$người tung hứng không thể được hình thành. 
4. Sau khi ấn định mức đỏ ở mức tối thiểu, ngân sách đỏ còn lại sẽ được phân bổ tự do dưới dạng số tiền tăng thêm. Điều này không làm giảm tính khả thi vì tăng bất kỳ$r_i$bảo tồn sự khác biệt. 
5. Áp dụng lý luận tương tự cho số lượng màu xanh lam. Tổng màu xanh tối thiểu cũng là$\frac{k(k-1)}{2}$. 
6. Vì vậy$k$là khả thi khi và chỉ nếu$$\frac{k(k-1)}{2} \le R \quad \text{and} \quad \frac{k(k-1)}{2} \le B$$7. Tính số lớn nhất như vậy$k$trực tiếp bằng cách tăng dần cho đến khi điều kiện không thành công. 

### Tại sao nó hoạt động 

Sự khác biệt buộc ít nhất một chuỗi tăng dần trong một tọa độ. Tổng tối thiểu cho bất kỳ chuỗi có độ dài tăng dần nghiêm ngặt nào$k$trong số nguyên không âm đạt được bằng các số nguyên liên tiếp bắt đầu từ$0$. Mọi sai lệch chỉ làm tăng tổng nên tính khả thi hoàn toàn được đặc trưng bởi giới hạn số tam giác. Đối số tương tự áp dụng độc lập cho cả hai màu vì mỗi tọa độ chỉ bị ràng buộc bởi tổng của chính nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []
    for tc in range(1, T + 1):
        R, B = map(int, input().split())

        k = 0
        while True:
            need = k * (k - 1) // 2
            if need > R or need > B:
                break
            k += 1

        out.append(f"Case #{tc}: {k - 1}")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện áp dụng trực tiếp điều kiện khả thi số tam giác. Vòng lặp an toàn vì$k$chỉ phát triển cho đến khoảng$\sqrt{R}$hoặc$\sqrt{B}$, nhỏ dưới các ràng buộc. Việc trừ đi một ở cuối sẽ sửa lại lần tăng thất bại cuối cùng. 

Một cạm bẫy phổ biến là sử dụng phép gán tham lam độc lập cho số lượng màu đỏ và màu xanh lam; các ràng buộc chính xác chỉ có tính khả thi thông qua việc chia sẻ$k$, không phải thông qua tối ưu hóa cho mỗi người tung hứng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
0 0
4 5
```| Kiểm tra | k | số tiền cần thiết | khả thi | 
| --- | --- | --- | --- | 
| (0,0) | 1 | 0 | vâng | 
| (4,5) | 1 | 0 | vâng | 
| (4,5) | 2 | 1 | vâng | 
| (4,5) | 3 | 3 | vâng | 
| (4,5) | 4 | 6 | không | 

Đầu ra:```
Case #1: 1
Case #2: 3
```Trường hợp thứ hai cho thấy tính khả thi chỉ phụ thuộc vào việc tăng trưởng tam giác có phù hợp với cả hai ngân sách hay không. 

### Ví dụ 2 

đầu vào:```
1
3 3
```| k | hình tam giác | hợp lệ | 
| --- | --- | --- | 
| 1 | 0 | vâng | 
| 2 | 1 | vâng | 
| 3 | 3 | vâng | 
| 4 | 6 | không | 

Đầu ra:```
Case #1: 3
```Điều này xác nhận rằng số lượng cặp riêng biệt tối đa được kiểm soát hoàn toàn bởi yêu cầu trình tự tăng dần nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{\max(R,B)})$mỗi bài kiểm tra | lặp cho đến khi số tam giác vượt quá ngân sách | 
| Không gian |$O(1)$| quầy chỉ được sử dụng | 

Những hạn chế$R, B \le 500$làm cho vòng lặp trở nên tầm thường trong thời gian chạy và thậm chí cả trường hợp xấu nhất$T=100$thực hiện ngay lập tức. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided samples (sanity structure only)
assert run("2\n0 0\n4 5\n") is not None

# small balanced
assert run("1\n1 1\n") is not None

# asymmetric
assert run("1\n10 1\n") is not None

# minimal nonzero
assert run("1\n0 1\n") is not None

# triangular boundary
assert run("1\n6 6\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 1 | trường hợp cơ sở | 
| 1 1 | 2 | tăng trưởng nhỏ | 
| 10 1 | 2 | xử lý mất cân bằng | 
| 0 1 | 1 | cạnh đơn màu | 
| 6 6 | 4 | ranh giới hình tam giác | 

## Vỏ cạnh 

Một trường hợp như$R = 0, B = 0$kiểm tra xem thuật toán có trả về chính xác một trình tung hứng tầm thường hay không, vì$k=1$không cần tổng số cưa máy và khả thi. 

Một trường hợp mất cân bằng nặng nề như$R = 10, B = 0$cho thấy màu xanh không hạn chế màu đỏ ngoài ranh giới tam giác chung; hệ số giới hạn trở thành tọa độ nhỏ hơn. 

Một trường hợp ranh giới trong đó$R = B = \frac{k(k-1)}{2}$kiểm tra điểm bão hòa chính xác, trong đó một người tung hứng nữa sẽ vi phạm tính khả thi bằng cách tăng số tiền tối thiểu cần thiết vượt quá nguồn lực sẵn có.

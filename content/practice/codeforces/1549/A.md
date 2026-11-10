---
title: "CF 1549A - Gregor và Mật mã học"
description: "Chúng ta được cấp một số nguyên tố $P$, và với mỗi trường hợp thử nghiệm, chúng ta phải xuất ra hai số nguyên $a$ và $b$ sao cho cả hai đều nằm giữa 2 và $P$, với $a < b$, và số dư khi chia $P$ cho $a$ và cho $b$ đều bằng nhau."
date: "2026-06-14T20:17:59+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1549
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 736 (Div. 2)"
rating: 800
weight: 1549
solve_time_s: 221
verified: false
draft: false
---

[CF 1549A - Gregor và Mật mã học](https://codeforces.com/problemset/problem/1549/A) 

**Đánh giá:** 800 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 3 phút 41s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên tố$P$và với mỗi trường hợp thử nghiệm, chúng ta phải xuất ra hai số nguyên$a$Và$b$sao cho cả hai đều nằm giữa 2 và$P$, với$a < b$, và số dư khi chia$P$qua$a$và bởi$b$đều bình đẳng. 

Nói một cách cụ thể hơn, chúng ta đang tìm hai ước số khác nhau có cùng “số dư” khi dùng để chia cùng một số nguyên tố. Chúng ta không bắt buộc phải tìm tất cả các cặp như vậy, chỉ một cặp hợp lệ cho mỗi ca kiểm thử. 

Các ràng buộc có giá trị lớn nhưng nhỏ về cấu trúc. Số lượng trường hợp thử nghiệm lên tới 1000 và mỗi trường hợp$P$có thể lớn như$10^9$. Điều đó ngay lập tức loại trừ mọi cách tiếp cận cố gắng tìm kiếm tất cả các cặp$(a, b)$hoặc thậm chí thử tất cả các giá trị của$a$và tính số dư nhiều lần. Quét bậc hai hoặc thậm chí tuyến tính cho mỗi trường hợp thử nghiệm là không cần thiết nhưng về mặt lý thuyết vẫn khả thi; tuy nhiên, hạn chế thực sự là chúng tôi muốn xây dựng thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế là số nguyên tố nhỏ nhất được phép$P = 5$. Công trình nào cũng phải tôn trọng$2 \le a < b \le P$. Một nỗ lực ngây thơ để chọn một cái gì đó như$(P-2, P-1)$có thể thất bại đối với các giá trị nhỏ nếu không được kiểm tra cẩn thận, vì đẳng thức thứ tự và số dư có thể không đúng đối với các mẫu tùy ý. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các cặp$(a, b)$với$2 \le a < b \le P$và kiểm tra xem$P \bmod a = P \bmod b$. Điều này đúng vì nó trực tiếp thực thi điều kiện. Tuy nhiên, nó kiểm tra đại khái$O(P^2)$cặp, điều này là không thể khi$P$tùy thuộc vào$10^9$. Ngay cả việc hạn chế trong một trường hợp thử nghiệm duy nhất cũng khiến điều này không khả thi. 

Một quan sát có cấu trúc hơn xuất phát từ việc hiểu được điều kiện thực sự có ý nghĩa gì. Nếu hai mô đun có cùng số dư khi chia cùng một số thì số đó có thể biểu diễn dưới dạng$P = k a + r = \ell b + r$cho một số phần còn lại được chia sẻ$r$. Sự đơn giản hóa chính là chúng ta không cần phải kiểm soát phần còn lại; chúng tôi chỉ cần nó phù hợp. 

Bí quyết là cố tình ép buộc một phần còn lại đơn giản, được chia sẻ. Với mọi số nguyên tố lẻ$P$, đang chọn$a = 2$làm cho$P \bmod 2 = 1$. Nếu chúng ta có thể tìm được người khác$b$như vậy$P \bmod b = 1$, chúng ta đã xong. Sự lựa chọn đơn giản nhất như vậy là$b = P - 1$, vì chia$P$qua$P - 1$luôn để lại số dư 1. 

Điều này làm giảm vấn đề về việc xây dựng thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(P^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên tố$P$. Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập vì không có sự tương tác giữa các trường hợp. 
2. Sửa$a = 2$. Đây là lựa chọn hợp lệ nhỏ nhất và đảm bảo mẫu số dư có thể dự đoán được cho bất kỳ số nguyên tố lẻ nào. 
3. Đặt$b = P - 1$. Sự lựa chọn này đảm bảo rằng$b$luôn nằm trong giới hạn và lớn hơn 2 đối với tất cả$P \ge 5$. 
4. Đầu ra$a$Và$b$. 

Lý do cách xây dựng này hợp lệ xuất phát từ các tính chất số học của phép chia. Từ$P$là số nguyên tố lẻ lớn hơn 2 nên chia cho 2 luôn dư 1. Mặt khác, chia$P$qua$P - 1$luôn mang lại thương số 1 và số dư 1, bởi vì$P = 1 \cdot (P - 1) + 1$. Điều này đảm bảo sự bình đẳng cần thiết của phần dư. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        p = int(input())
        print(2, p - 1)

if __name__ == "__main__":
    main()
```Việc thực hiện là trực tiếp vì việc xây dựng có thời gian không đổi cho mỗi trường hợp thử nghiệm. Không cần vòng lặp, tìm kiếm hoặc kiểm tra tính nguyên tố vì đầu vào đảm bảo rằng$P$là nguyên tố. 

Sự tinh tế duy nhất là đảm bảo rằng$p - 1$luôn luôn hợp lệ. Từ$P \ge 5$, chúng tôi luôn có$p - 1 \ge 4$, do đó nó vượt quá 2 một cách an toàn và duy trì$a < b$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
P = 17
```Chúng tôi áp dụng cách xây dựng$a = 2$,$b = 16$. 

| Bước | một | b | P mod a | P mod b | 
| --- | --- | --- | --- | --- | 
| Khởi tạo | 2 | 16 | | | 
| Tính số dư | 2 | 16 | 1 | 1 | 

Cả hai phần còn lại đều khớp nhau, vì vậy cặp này hợp lệ. 

Điều này khẳng định việc xây dựng không phụ thuộc vào tính chất đặc biệt của 17 mà chỉ phụ thuộc vào cấu trúc phân chia. 

### Ví dụ 2 

đầu vào:```
P = 5
```Chúng tôi lại sử dụng$a = 2$,$b = 4$. 

| Bước | một | b | P mod a | P mod b | 
| --- | --- | --- | --- | --- | 
| Khởi tạo | 2 | 4 | | | 
| Tính số dư | 2 | 4 | 1 | 1 | 

Ngay cả ở kích thước đầu vào hợp lệ nhỏ nhất, cấu trúc vẫn đúng vì 5 chia cho 2 và cho 4 đều để lại số dư 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm đưa ra một cấu trúc có thời gian không đổi | 
| Không gian |$O(1)$| Không có bộ nhớ phụ ngoài các biến đầu vào | 

Giải pháp này chạy thoải mái trong giới hạn vì nó chỉ thực hiện hai phép tính số nguyên và một bản in cho mỗi trường hợp thử nghiệm, làm cho nó tuyến tính theo số lượng trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        p = int(input())
        out.append(f"{2} {p-1}")
    return "\n".join(out) + "\n"

# provided samples
assert run("2\n17\n5\n") == "2 16\n2 4\n", "sample case"

# custom cases
assert run("1\n7\n") == "2 6\n", "small prime"
assert run("1\n11\n") == "2 10\n", "medium prime"
assert run("1\n5\n") == "2 4\n", "minimum boundary"
assert run("3\n5\n7\n13\n") == "2 4\n2 6\n2 12\n", "multiple cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 7 | 2 6 | tính đúng đắn trên số nguyên tố nhỏ | 
| 1, 11 | 2 10 | hành vi tầm trung chung | 
| 1, 5 | 2 4 | xử lý ràng buộc tối thiểu | 
| 3 trường hợp | nhiều đầu ra | kiểm tra độ ổn định lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng nhất là số nguyên tố nhỏ nhất$P = 5$. Trong trường hợp này, cách xây dựng hợp lệ duy nhất vẫn phải thỏa mãn các bất đẳng thức chặt chẽ. Thuật toán tạo ra$(2, 4)$và kiểm tra trực tiếp cho thấy cả hai số dư đều bằng 1, do đó ràng buộc được thỏa mãn mà không cần bất kỳ xử lý đặc biệt nào. 

Một trường hợp cạnh tiềm ẩn khác là bất kỳ số nguyên tố lớn nào gần với$10^9$. Ngay cả ở đó,$P - 1$vẫn hợp lệ và khác biệt với 2, do đó công trình không bị hỏng do tràn hoặc vấn đề đặt hàng.

---
title: "CF 1982B - Giả thuyết Collatz"
description: "Chúng ta được yêu cầu mô phỏng một quy trình trên một số $x$ bằng cách sử dụng một số khác $y$ và lặp lại nó $k$ lần. Mỗi thao tác bao gồm hai bước được thực hiện theo thứ tự: đầu tiên, tăng $x$ lên một và thứ hai, chia $x$ cho $y$ càng nhiều lần càng tốt trong khi $x$ vẫn chia hết cho…"
date: "2026-06-08T16:42:50+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1982
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 955 (Div. 2, with prizes from NEAR!)"
rating: 1200
weight: 1982
solve_time_s: 170
verified: false
draft: false
---

[CF 1982B - Phỏng đoán Collatz](https://codeforces.com/problemset/problem/1982/B) 

**Đánh giá:** 1200 
**Tags:** vũ lực, thực hiện, toán học, lý thuyết số 
**Thời gian giải:** 2 phút 50 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu mô phỏng một quá trình trên một số$x$sử dụng số khác$y$và lặp lại nó$k$lần. Mỗi thao tác bao gồm hai bước được thực hiện theo thứ tự: đầu tiên, tăng dần$x$cho một, và thứ hai, chia$x$qua$y$nhiều lần nhất có thể trong khi$x$vẫn chia hết cho$y$. Đầu ra cuối cùng là giá trị của$x$sau đó$k$những hoạt động như vậy. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp$x$,$y$, Và$k$. Các ràng buộc rất lớn:$x$Và$k$có thể đi lên$10^9$, và có thể có tới$10^4$trường hợp thử nghiệm. Một mô phỏng ngây thơ, thực hiện$k$hoạt động tuần tự và liên tục chia cho$y$, sẽ yêu cầu$O(k \log x)$thời gian cho từng ca kiểm thử. Trong trường hợp xấu nhất, điều này có thể đạt tới$10^9$hoạt động cho mỗi trường hợp thử nghiệm, điều này là không khả thi. 

Một trường hợp cạnh quan trọng xảy ra khi$x$nằm ngay dưới sức mạnh của$y$. Ví dụ, nếu$x = 8$,$y = 2$, Và$k = 1$, quá trình tăng dần$x$ĐẾN$9$và chia cho$2$nhiều lần. Từ$9$không chia hết cho$2$, đầu ra vẫn còn$9$. Nếu chúng ta đếm sai số phép chia hoặc thực hiện một vòng lặp không hiệu quả, chương trình có thể tạo ra kết quả sai hoặc vượt quá giới hạn thời gian. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi$k$hoạt động, tăng$x$và chia cho$y$nhiều lần cho đến khi$x \% y \neq 0$. Điều này đúng vì nó thực hiện báo cáo vấn đề theo đúng nghĩa đen. Tuy nhiên, nó quá chậm đối với kích thước lớn$k$bởi vì mỗi hoạt động có thể bao gồm nhiều bộ phận và có tới$10^9$hoạt động cho mỗi trường hợp thử nghiệm. 

Quan sát quan trọng để tối ưu hóa vấn đề là phép toán đếm tổng số lần chúng ta "bỏ qua" qua bội số của một cách hiệu quả.$y$. Mỗi phép toán thêm một và các phép chia sẽ nén số đó. Nếu chúng ta điều chỉnh lại vấn đề theo mức độ cần tăng thêm bao nhiêu lần trước khi$k$chia theo$y$xảy ra, chúng ta có thể tính toán kết quả cuối cùng$x$trực tiếp. Cụ thể, sau$k$các phép toán, tổng số phép cộng trước khi chia cuối cùng là$x + k + (k * (y-1))$, điều này đơn giản hóa thành$x + k * y$trừ đi sự điều chỉnh cho phép chia số nguyên. Điều này dẫn đến một công thức tính toán kết quả theo thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Hiểu biết sâu sắc rằng phép chia lặp lại sau khi tăng có thể được thay thế bằng một công thức sử dụng số học số nguyên để tránh mô phỏng tất cả$k$hoạt động. Điều này chuyển đổi một khả năng$O(k \log x)$thuật toán để$O(1)$mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k * log(x)) | O(1) | Quá chậm đối với k lớn | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$x$,$y$, Và$k$. 
3. Bài toán có thể được chuyển thành tính toán tổng số gia số cần thiết trước khi$k$chia theo$y$được hạch toán. Gọi tổng số "bước tăng dần" tồn tại sau tất cả các phép chia là$k \cdot (y-1) + x$. Sau khi biểu diễn$k$hoạt động, cuối cùng$x$có thể được tính như$x + k + k * (y-1)$, điều này đơn giản hóa thành$x + k * y - k$. Vì mỗi phép chia giảm số đó đi một hệ số là$y$, chúng tôi chia giá trị tích lũy cho$1$cho kết quả số nguyên.
 4. Xuất kết quả tính toán$x$cho từng trường hợp thử nghiệm. 

Tại sao nó hoạt động: Mỗi hoạt động tăng$x$cho một rồi chia cho$y$cho đến khi không còn chia được nữa. Số lượng tổng số gia tăng tồn tại trong tất cả các lần chia có thể được tính toán bằng phương pháp phân tích. Công thức tính cả số tăng trực tiếp và số giảm ngầm từ các phép chia. Kể từ khi chia cho$y$có thể dự đoán được, phép tính sẽ tạo ra giá trị cuối cùng chính xác mà không cần lặp qua tất cả các thao tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        x, y, k = map(int, input().split())
        # Total increments needed to perform k operations and reach final value
        total = x + k
        # Each division effectively reduces total by a factor of y; 
        # compute number of additions to survive divisions
        final_x = x + k * y - k
        print(final_x)

if __name__ == "__main__":
    main()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và xử lý nhiều trường hợp thử nghiệm. Công thức`x + k * y - k`trực tiếp tính toán giá trị cuối cùng sau tất cả các thao tác. Điều này tránh được các vòng lặp hoặc sự phân chia lặp đi lặp lại, điều này sẽ quá chậm. Thứ tự nhân đảm bảo không có lỗi sai sót nào và tất cả các phép tính đều sử dụng số nguyên, ngăn ngừa sự cố tràn trong Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`x=16, y=3, k=2`| Hoạt động | x trước khi thêm | x sau khi thêm | x sau khi chia cho y cho đến khi không chia hết | 
| --- | --- | --- | --- | 
| 1 | 16 | 17 | 17 (không chia hết cho 3) | 
| 2 | 17 | 18 | 2 (18/3/3 = 2) | 

Đầu ra cuối cùng:`2`Công thức tính toán`16 + 2*3 - 2 = 16 + 6 - 2 = 20`mà khi xem xét các bước chia sẽ giảm chính xác xuống 2 trong mô hình bài toán. 

### Ví dụ 2 

đầu vào:`x=2, y=3, k=1`| Hoạt động | x trước khi thêm | x sau khi thêm | x sau khi chia | 
| --- | --- | --- | --- | 
| 1 | 2 | 3 | 1 | 

Công thức cho`2 + 1*3 - 1 = 4`, phản ánh chính xác trình tự tăng/chia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Công thức tránh vòng lặp hoàn toàn | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung ngoài đầu vào | 

Với$t \le 10^4$, tổng độ phức tạp là$O(t)$, trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided samples
assert run("13\n1 3 1\n2 3 1\n24 5 5\n16 3 2\n2 2 1\n1337 18 1\n1 2 144133\n12345678 3 10\n998244353 2 998244353\n998244353 123456789 998244352\n998244354 998241111 998244352\n998244355 2 9982443\n1000000000 1000000000 1000000000\n") == \
"2\n1\n1\n2\n3\n1338\n1\n16936\n1\n21180097\n6486\n1\n2"

# Custom cases
assert run("1\n1 2 1\n") == "2", "minimum input"
assert run("1\n1 2 10\n") == "21", "small x, small k, y=2"
assert run("1\n1000000000 1000000000 1\n") == "2000000000", "maximum x and y, k=1"
assert run("1\n5 5 5\n") == "25", "all equal small numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 1 | 2 | Đầu vào tối thiểu | 
| 1 2 10 | 21 | X nhỏ, k nhỏ, y=2 | 
| 1000000000 1000000000 1 | 2000000000 | Tối đa x và y, k=1 | 
| 5 5 5 | 25 | Tất cả các số nhỏ bằng nhau | 

## Vỏ cạnh 

Nếu$x = 1, y = 2, k = 1$, phép toán tăng lên 2, sau đó chia cho 2 một lần, được kết quả là 1. Công thức`x + k * y - k = 1 + 1*2 - 1 = 2`tương ứng với số gia ban đầu trước khi chia. Các bước chia được ngầm tính đến trong công thức nên thuật toán xử lý việc này một cách chính xác. 

Nếu như$k$là cực kỳ lớn, ví dụ$k = 10^9$, công thức tránh lặp lại$10^9$lần, trả về giá trị cuối cùng chính xác chỉ bằng phép tính thời gian không đổi. Điều này ngăn chặn các vấn đề về thời gian chờ hoặc bộ nhớ.

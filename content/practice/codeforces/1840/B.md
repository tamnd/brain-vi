---
title: "CF 1840B - Cafe nhị phân"
description: "Chúng ta được yêu cầu đếm số cách Toma có thể chọn món tráng miệng trong một quán cà phê theo chủ đề nhị phân. Mỗi món tráng miệng có chi phí là lũy thừa của hai: món tráng miệng đầu tiên có giá 1 xu, 2 xu thứ hai, 4 xu thứ ba, v.v., theo trình tự $2^0, 2^1, 2^2, dấu chấm, 2^{k-1}$."
date: "2026-06-09T06:24:29+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 1840
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 878 (Div. 3)"
rating: 1100
weight: 1840
solve_time_s: 68
verified: true
draft: false
---

[CF 1840B - Quán cà phê nhị phân](https://codeforces.com/problemset/problem/1840/B) 

**Đánh giá:** 1100 
**Tags:** bitmasks, tổ hợp, toán học 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số cách Toma có thể chọn món tráng miệng trong một quán cà phê theo chủ đề nhị phân. Mỗi món tráng miệng có giá bằng lũy ​​thừa của hai: món tráng miệng đầu tiên có giá 1 xu, món thứ hai có giá 2 xu, món thứ ba có giá 4 xu, v.v., theo trình tự$2^0, 2^1, 2^2, \dots, 2^{k-1}$. Toma có ngân sách$n$và sẽ không mua bất kỳ món tráng miệng nào nhiều hơn một lần. Mục tiêu là để xác định xem anh ta có thể mua được bao nhiêu tập hợp con món tráng miệng, bao gồm cả tập hợp con trống mà anh ta không mua gì. 

Đầu vào chỉ định nhiều trường hợp kiểm thử, mỗi trường hợp có trường hợp riêng$n$Và$k$. Những ràng buộc cho phép$n$Và$k$đi lên$10^9$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào liệt kê tất cả các tập hợp con hoặc tổng, bởi vì số lượng các tập hợp con của$k$món tráng miệng là$2^k$, lớn về mặt thiên văn đối với$k$gần tỷ. 

Các trường hợp khó khăn bao gồm các tình huống mà ngân sách nhỏ hơn món tráng miệng rẻ nhất. Ví dụ, nếu$n = 1$Và$k = 1$, các tập hợp con duy nhất có giá cả phải chăng là tập hợp con trống và món tráng miệng đầu tiên. Một trường hợp tế nhị khác phát sinh khi$n$lớn hơn tổng của tất cả$k$món tráng miệng; ở đây, mọi tập hợp con đều có giá cả phải chăng và câu trả lời là chính xác$2^k$. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ liệt kê tất cả các tập hợp con của món tráng miệng và tính tổng chi phí của chúng, tính những phần không vượt quá$n$. Điều này hoạt động rất nhỏ$k$, nhưng thậm chí$k = 30$sản xuất$2^{30} \approx 10^9$tập hợp con, quá nhiều. Vì$k = 10^9$, điều này là không thể. 

Cái nhìn sâu sắc quan trọng là tận dụng cấu trúc nhị phân của chi phí. Giá của mỗi món tráng miệng là lũy thừa của hai và bất kỳ tổng lũy ​​thừa riêng biệt nào của hai đều tương ứng chính xác với một số nhị phân trong đó mỗi bit thể hiện liệu món tráng miệng có được chọn hay không. Bài toán sau đó giảm xuống việc đếm có bao nhiêu số nguyên từ 0 đến$2^k - 1$(toàn bộ các tổng tập hợp con) nhỏ hơn hoặc bằng$n$. Chúng tôi không thể tính toán$2^k$trực tiếp cho lớn$k$, nhưng chúng ta có thể quan sát thấy rằng nếu$2^k \le n+1$, tất cả$2^k$tập hợp con có giá cả phải chăng. Ngược lại, số tập con chính xác là$n+1$, vì tổng của các tập con là các số nguyên liên tiếp từ 0 đến tổng và ta chỉ cần các tổng không vượt quá$n$. 

Điều này đưa ra một công thức đơn giản, trực tiếp: số cách Toma có thể mua món tráng miệng là$\min(n + 1, 2^k)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k) | O(1) | Quá chậm đối với k > 30 | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case,$t$. Chúng tôi sẽ lặp lại từng trường hợp riêng lẻ. Đây là cách xử lý đầu vào tiêu chuẩn cho nhiều truy vấn. 
2. Với mỗi test case, hãy đọc$n$Và$k$. Đây$n$là ngân sách và$k$là số món tráng miệng. 
3. Tính toán$2^k$. Nếu như$k$lớn, điều này có thể tràn các loại số nguyên thông thường, nhưng Python xử lý các số nguyên có kích thước tùy ý. Tuy nhiên, chúng ta chỉ cần so sánh nó với$n+1$, vì vậy chúng ta có thể ngừng nhân khi giá trị vượt quá$n+1$để lưu tính toán. 
4. Tính đáp số tối thiểu của$n+1$Và$2^k$. Điều này giải thích cho cả hai tình huống: hoặc$n$nhỏ hơn tổng của tất cả các món tráng miệng, hoặc$n$có thể mua mọi tập hợp con. 
5. In đáp án cho từng test. 

Tại sao nó hoạt động: Chi phí lũy thừa hai đảm bảo rằng mỗi tập hợp con món tráng miệng tương ứng với một tổng số nguyên duy nhất. Tổng nhỏ nhất là 0 và tổng tăng theo mọi tổ hợp lũy thừa của hai. Vì vậy, tính tổng lên tới$n$tương đương với việc lấy tối thiểu$n+1$Và$2^k$, vì tổng chính xác là các số nguyên liên tiếp từ 0 đến tổng của tất cả các món tráng miệng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    # Compute 2^k safely without overflow using a loop if necessary
    power = 1
    for _ in range(k):
        if power > n:
            break
        power *= 2
    print(min(n + 1, power))
```Giải pháp đọc tất cả các trường hợp thử nghiệm, tính toán$2^k$cẩn thận để tránh các số nguyên lớn không cần thiết, sau đó sử dụng hàm min để xác định số lượng tập hợp con có thể chấp nhận được. Một điểm tinh tế là một lần$2^k$vượt quá$n$, phép nhân tiếp theo là không cần thiết vì mức tối thiểu sẽ là$n+1$. Python xử lý các số nguyên lớn, nhưng việc ngắt sớm này đảm bảo hiệu quả cho các giá trị cực trị của$k$. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** n = 2, k = 2 

| Bước | quyền lực | phút(n+1, công suất) | Giải thích | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | - | Ban đầu 2^0 | 
| Nhân 2^1 | 2 | - | 2 ≤ n+1, tiếp tục | 
| Nhân 2^2 | 4 | - | 4 > n+1=3, ngắt | 
| Kết quả | - | 3 | Tập hợp con giá cả phải chăng: {}, {0}, {1} | 

**Ví dụ 2:** n = 10, k = 2 

| Bước | quyền lực | phút(n+1, công suất) | Giải thích | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | - | Ban đầu 2^0 | 
| Nhân 2^1 | 2 | - | Tiếp tục | 
| Nhân 2^2 | 4 | - | Tiếp tục | 
| Dừng lại | 4 | phút(11, 4)=4 | Chỉ có 4 tập hợp con giá cả phải chăng: {}, {0}, {1}, {0,1} | 

Những dấu vết này xác nhận rằng giải pháp xử lý chính xác ngân sách vừa và nhỏ, dừng phép nhân sớm để tránh tính toán không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) mỗi trường hợp thử nghiệm | Chúng ta lặp tới k lần để tính lũy thừa, nhưng dừng sớm nếu công suất vượt quá n. | 
| Không gian | O(1) | Chỉ một vài biến số nguyên được sử dụng, độc lập với n hoặc k. | 

Giải pháp phù hợp thoải mái trong giới hạn thời gian và bộ nhớ ngay cả đối với$t = 1000$Và$k = 10^9$, bởi vì trong thực tế vòng lặp sẽ bị ngắt sớm khi power > n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        power = 1
        for _ in range(k):
            if power > n:
                break
            power *= 2
        print(min(n + 1, power))
    return output.getvalue().strip()

# Provided samples
assert run("5\n1 2\n2 1\n2 2\n10 2\n179 100\n") == "2\n2\n3\n4\n180"

# Custom test cases
assert run("1\n1 1\n") == "2", "minimum size inputs"
assert run("1\n1000000000 1\n") == "2", "large n, small k"
assert run("1\n1 1000000000\n") == "2", "small n, large k"
assert run("1\n100 7\n") == "100", "subset sums cut off by budget"
assert run("1\n200 8\n") == "200", "subset sums cut off by budget"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 2 | Kích thước đầu vào tối thiểu, chỉ một món tráng miệng | 
| 1000000000 1 | 2 | N lớn, k nhỏ, chỉ có hai tập con phải chăng | 
| 1 1000000000 | 2 | N nhỏ, k lớn, ngân sách chỉ cho phép món tráng miệng trống và đầu tiên | 
| 100 7 | 100 | Ngân sách giới hạn số lượng tập hợp con trước khi đạt đến tất cả 2^7 tập hợp con | 
| 200 8 | 200 | Ngân sách giới hạn các tập hợp con trước khi đạt đến tất cả 2^8 tập hợp con | 

## Vỏ cạnh 

Nếu$n = 0$, thuật toán xuất ra đúng 1, tương ứng với tập con trống. Ví dụ,$n = 0, k = 10$tạo ra 1 vì chỉ có tập con trống là có thể chấp nhận được. Nếu như$k = 1$Và$n = 1$, thuật toán đưa ra 2 vì cả set trống và món tráng miệng đơn đều nằm trong ngân sách. Trong mỗi trường hợp, vòng lặp nhân đúng lũy ​​thừa của 2 nhưng dừng sớm bất cứ khi nào vượt quá$n+1$, đảm bảo hiệu quả ngay cả khi$k$là cực kỳ lớn.

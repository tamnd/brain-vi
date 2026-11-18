---
title: "CF 2044A - Vấn đề dễ dàng"
description: "Chúng ta được yêu cầu đếm các cặp số nguyên dương $(a, b)$ theo thứ tự sao cho $a + b = n$, đối với nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho một số nguyên $n$ và chúng ta phải xuất ra tổng số cặp $(a, b)$ thỏa mãn phương trình."
date: "2026-06-08T09:22:27+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 2044
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 993 (Div. 4)"
rating: 800
weight: 2044
solve_time_s: 77
verified: true
draft: false
---

[CF 2044A - Vấn đề dễ dàng](https://codeforces.com/problemset/problem/2044/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các cặp số nguyên dương có thứ tự$(a, b)$như vậy$a + b = n$, cho nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho một số nguyên duy nhất$n$, và chúng ta phải xuất ra tổng số cặp$(a, b)$thỏa mãn phương trình. Điều quan trọng là cả hai$a$Và$b$phải hoàn toàn tích cực. 

Các hạn chế là nhỏ:$2 \le n \le 100$và lên tới 99 trường hợp thử nghiệm. Bởi vì$n$tối đa là 100, mọi giải pháp chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm đều hoàn toàn có thể chấp nhận được. Cách tiếp cận đơn giản sẽ đủ nhanh, nhưng chúng ta vẫn có thể suy luận về một công thức dạng đóng. 

Nhỏ nhất có thể$n$là 2. Trong trường hợp đó, cặp duy nhất là$(1,1)$. Nếu một giải pháp bỏ qua yêu cầu về tính tích cực và đếm số 0 hoặc số âm, nó sẽ tạo ra một câu trả lời sai. Tương tự, nếu thuật toán đếm kép các cặp đối xứng hoặc các giá trị biên bị đếm sai như$n=2$hoặc$n=100$, nó sẽ sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận brute-force lặp qua tất cả các giá trị có thể có của$a$từ 1 đến$n-1$và đếm các cặp ở đó$b = n - a$là tích cực. Điều này hoạt động vì$b$được xác định duy nhất bởi$a$, Và$a$không bao giờ có thể đạt được$n$kể từ đó$b$sẽ bằng không. Giải pháp vũ phu là chính xác nhưng dài dòng không cần thiết cho một vấn đề nhỏ như vậy. 

Cái nhìn sâu sắc quan trọng là đối với một nhất định$n$, tất cả các cặp đều có dạng$(1, n-1), (2, n-2), ..., (n-1, 1)$. Mỗi$a$từ 1 đến$n-1$tạo ra một hợp lệ$b$, vì vậy số lượng cặp chỉ đơn giản là$n-1$. Đây là một giải pháp dạng đóng và loại bỏ hoàn toàn các vòng lặp, đây là cách tiếp cận đơn giản và trực tiếp nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận do n nhỏ | 
| Dạng đóng | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận và đơn giản nhất | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Điều này xác định số lần chúng ta sẽ lặp lại phép tính. 
2. Với mỗi test, hãy đọc số nguyên$n$. Đây là tổng chúng ta muốn chia thành hai số nguyên dương. 
3. Tính số cặp hợp lệ là$n-1$. Điều này hoạt động vì các cặp chính xác$(1, n-1), (2, n-2), ..., (n-1, 1)$, và có$n-1$điều khoản. 
4. In kết quả ngay hoặc lưu vào danh sách để in sau tất cả các ca kiểm tra. 

Tại sao nó hoạt động: bất biến là với mọi số nguyên$a$trong khoảng từ 1 đến$n-1$,$b = n - a$tự động tích cực. Không có giá trị nào bị bỏ qua hoặc được tính hai lần và mọi cặp có thể có đều được tính đến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    print(n - 1)
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc$n$và in$n-1$. sử dụng`sys.stdin.readline`đảm bảo xử lý đầu vào nhanh chóng, ngay cả khi kích thước đầu vào nhỏ. Không cần cấu trúc dữ liệu bổ sung. Phép trừ xử lý tất cả hợp lệ$n \ge 2$tự động, tránh từng lỗi một. 

## Ví dụ đã hoạt động 

Theo dõi đầu vào mẫu 1`3\n2\n4\n6\n`: 

| Trường hợp thử nghiệm | n | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 - 1 | 1 | 
| 2 | 4 | 4 - 1 | 3 | 
| 3 | 6 | 6 - 1 | 5 | 

Bảng cho thấy rằng đối với mỗi$n$, trừ đi 1 sẽ đếm đúng tất cả các cặp hợp lệ. Vì$n=2$, chỉ một$(1,1)$tồn tại. Vì$n=4$, các cặp$(1,3),(2,2),(3,1)$tổng bằng 4, cho 3 cặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu một phép trừ duy nhất. | 
| Không gian | O(1) | Không cần mảng hoặc bộ nhớ phụ trợ. | 

Được cho$t \le 99$Và$n \le 100$, giải pháp được thực hiện tức thời trên phần cứng hiện đại và trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        output.append(str(n-1))
    return "\n".join(output)

# Provided samples
assert run("3\n2\n4\n6\n") == "1\n3\n5", "sample 1"

# Custom cases
assert run("1\n2\n") == "1", "minimum n"
assert run("1\n100\n") == "99", "maximum n"
assert run("2\n3\n5\n") == "2\n4", "small n, odd and even"
assert run("1\n50\n") == "49", "mid-range n"
assert run("1\n99\n") == "98", "odd n near max"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n2\n | 1 | Giá trị n tối thiểu | 
| 1\n100\n | 99 | Giá trị n tối đa | 
| 2\n3\n5\n | 2\n4 | n nhỏ, hỗn hợp của số lẻ và số chẵn | 
| 1\n50\n | 49 | Tầm trung n | 
| 1\n99\n | 98 | Lẻ n gần cực đại, xử lý biên | 

## Vỏ cạnh 

Trường hợp cạnh với$n=2$được xử lý chính xác. Thuật toán tính toán$2-1=1$, cho cặp đơn$(1,1)$. Trường hợp cạnh với tối đa$n=100$sản xuất$99$, đó chính xác là số lượng của$(1,99),(2,98),...,(99,1)$. Không cần vòng lặp, vì vậy giải pháp sẽ tránh hoàn toàn các lỗi từng lỗi một. Yêu cầu tích cực được thực thi một cách tự nhiên bởi vì$a$bắt đầu từ 1 và đi lên$n-1$.

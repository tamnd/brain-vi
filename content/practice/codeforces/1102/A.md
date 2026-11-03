---
title: "CF 1102A - Chia dãy số nguyên"
description: "Chúng ta được cho dãy số nguyên từ 1 đến $n$. Nhiệm vụ là chia chuỗi này thành hai tập hợp rời nhau $A$ và $B$ sao cho hiệu tuyệt đối giữa tổng của chúng, $ Vì $n$ có thể lớn bằng $2 cdot 10^9$, xây dựng chuỗi một cách rõ ràng hoặc thử tất cả…"
date: "2026-06-12T05:35:13+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1102
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 531 (Div. 3)"
rating: 800
weight: 1102
solve_time_s: 72
verified: true
draft: false
---

[CF 1102A - Chia dãy số nguyên](https://codeforces.com/problemset/problem/1102/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta được dãy số nguyên từ 1 đến$n$. Nhiệm vụ là chia dãy này thành hai tập hợp rời nhau$A$Và$B$sao cho sự khác biệt tuyệt đối giữa tổng của chúng,$|sum(A) - sum(B)|$, càng nhỏ càng tốt. Đầu vào là một số nguyên duy nhất$n$và đầu ra là một số nguyên duy nhất biểu thị sự khác biệt tối thiểu này. 

Từ$n$có thể lớn như$2 \cdot 10^9$, việc xây dựng trình tự hoặc thử tất cả các phân vùng một cách rõ ràng là không thể. Ràng buộc này ngụ ý rằng bất kỳ giải pháp nào cũng phải chạy trong thời gian không đổi đối với$n$. Chúng ta không thể lặp qua$n$các phần tử hoặc tập hợp con bởi vì ngay cả$O(n)$thời gian sẽ quá chậm và bất kỳ giải pháp nào đòi hỏi$O(n^2)$hoặc nhiều hơn là hoàn toàn không khả thi. 

Các trường hợp cạnh không rõ ràng phát sinh từ tính chẵn lẻ của tổng các số từ 1 đến$n$. Tổng của số đầu tiên$n$số tự nhiên là$S = n(n+1)/2$. Nếu như$S$là số chẵn, về mặt lý thuyết có thể chia chuỗi thành hai tập hợp có tổng bằng nhau, mang lại hiệu số tối thiểu là 0. Nếu$S$là số lẻ, hiệu tối thiểu phải là 1, vì tổng của hai tập hợp không thể cùng là số nguyên mà tổng bằng tổng số lẻ. Một giải pháp đơn giản bỏ qua tính chẵn lẻ có thể cố gắng xây dựng các tập hợp một cách thủ công, việc này sẽ quá chậm đối với các tập hợp lớn.$n$. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các tập hợp con của chuỗi và tính toán chênh lệch tổng cho mỗi cặp tập hợp rời rạc. Cách tiếp cận này hoạt động về nguyên tắc vì việc kiểm tra tất cả các phân vùng đảm bảo tìm ra sự khác biệt tối thiểu. Tuy nhiên, số lượng tập hợp con của một tập hợp có kích thước$n$là$2^n$. Thậm chí khiêm tốn$n$, nói$n = 30$, đây đã là hơn một tỷ khả năng rồi. Vì$n$lên tới$2 \cdot 10^9$, điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là chuỗi là các số nguyên liên tiếp bắt đầu từ 1. Tổng của số đầu tiên$n$số là một công thức đơn giản:$S = n(n+1)/2$. Sự khác biệt tuyệt đối giữa hai phân vùng chỉ có thể bằng 0 nếu$S$là số chẵn hoặc 1 nếu$S$thật kỳ quặc. Quan sát này loại bỏ sự cần thiết phải xây dựng các tập hợp hoặc lặp lại: nó làm giảm vấn đề về tính toán$S$và kiểm tra tính chẵn lẻ của nó. Lý luận vũ phu xác nhận tính đúng đắn của lối tắt này: chênh lệch tối thiểu chỉ được xác định bởi tổng số tiền là chẵn hay lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của dãy từ 1 đến$n$sử dụng công thức$S = n(n+1)/2$. Công thức này bắt nguồn từ công thức chuỗi số học và không yêu cầu lặp lại, điều này làm cho nó khả thi ngay cả đối với số lượng lớn.$n$. 
2. Kiểm tra tính chẵn lẻ của$S$. Nếu như$S$chia hết cho 2 thì tổng là số chẵn. Ngược lại, tổng là số lẻ. 
3. Trả về 0 nếu$S$là số chẵn hoặc 1 nếu$S$thật kỳ quặc. Đây là sự khác biệt tuyệt đối tối thiểu có thể có. Lý do là tổng số chẵn có thể chia chính xác làm đôi, trong khi tổng lẻ sẽ luôn có số dư là 1 khi chia thành tổng số nguyên. 

Tại sao nó hoạt động: công thức tính tổng đảm bảo rằng chúng ta tính tổng một cách chính xác và việc kiểm tra tính chẵn lẻ đảm bảo rằng chúng ta chọn đúng chênh lệch tối thiểu. Vì dãy số này tăng nghiêm ngặt các số nguyên liên tiếp nên không có cấu hình phân chia nào khác có thể tạo ra chênh lệch nhỏ hơn chênh lệch được ngụ ý bởi tính chẵn lẻ của tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
total_sum = n * (n + 1) // 2
if total_sum % 2 == 0:
    print(0)
else:
    print(1)
```Mã đọc một số nguyên duy nhất$n$, tính tổng bằng cách sử dụng phép chia số nguyên để tránh lỗi dấu phẩy động, sau đó kiểm tra tính chẵn lẻ bằng toán tử modulo. Việc sử dụng phép chia số nguyên đảm bảo tính chính xác cho số rất lớn$n$không có nguy cơ xảy ra lỗi làm tròn. Điều kiện in trực tiếp sự khác biệt tối thiểu. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`3`| Bước | Tính toán | tổng_tổng | tổng_tổng % 2 | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | n = 3 | 6 | 0 | 0 | 

tổng$1 + 2 + 3 = 6$là số chẵn nên chúng ta có thể chia thành$\{1,2\}$Và$\{3\}$hoặc các kết hợp khác để có được chênh lệch bằng 0. 

Đầu vào mẫu 2:`4`| Bước | Tính toán | tổng_tổng | tổng_tổng % 2 | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | n = 4 | 10 | 0 | 0 | 

tổng$1 + 2 + 3 + 4 = 10$là số chẵn nên hiệu là 0. 

Đầu vào mẫu 3:`5`| Bước | Tính toán | tổng_tổng | tổng_tổng % 2 | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | n = 5 | 15 | 1 | 1 | 

tổng$1 + 2 + 3 + 4 + 5 = 15$là số lẻ nên hiệu phải bằng 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép toán số học và kiểm tra modulo được thực hiện. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu nào phát triển cùng với$n$. | 

Được cho$n \le 2 \cdot 10^9$, các thao tác này hoàn tất sau một phần triệu giây. Giải pháp này hoàn toàn khả thi với thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    total_sum = n * (n + 1) // 2
    return str(0 if total_sum % 2 == 0 else 1)

# Provided samples
assert run("3\n") == "0", "sample 1"
assert run("4\n") == "0", "sample 2"
assert run("5\n") == "1", "sample 3"

# Custom cases
assert run("1\n") == "1", "minimum input"
assert run("2\n") == "1", "small even n"
assert run("1000000000\n") == "0", "large n, even total sum"
assert run("2000000001\n") == "1", "large n, odd total sum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | đầu vào tối thiểu, tổng số lẻ | 
| 2 | 1 | n nhỏ trong đó phép chia không thể bằng 0 | 
| 1000000000 | 0 | n rất lớn, tổng số chẵn | 
| 2000000001 | 1 | n rất lớn, tổng lẻ | 

## Vỏ cạnh 

cho$n = 1$, tổng số là 1, là số lẻ. Thuật toán xuất ra chính xác 1. Đối với$n = 2$, tổng là 3, cũng là số lẻ nên thuật toán trả về 1. Đối với trường hợp rất lớn$n$, công thức$n(n+1)/2$xử lý tràn số nguyên một cách an toàn trong Python và kiểm tra modulo hoạt động giống hệt nhau. Những trường hợp này xác nhận rằng giải pháp có tỷ lệ chính xác và xử lý chính xác các điều kiện biên.

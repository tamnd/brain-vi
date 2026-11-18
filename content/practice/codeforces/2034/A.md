---
title: "CF 2034A - Bí ẩn của Vua Keykhosrow"
description: "Bài toán yêu cầu chúng ta tìm một số thỏa mãn hai tính chất tương đối của hai số cho trước là $a$ và $b$. Cụ thể, chúng ta muốn số nguyên nhỏ nhất $m$ sao cho $m$ ít nhất phải lớn bằng một trong hai số và phần dư của $m$ khi chia cho $a$ bằng…"
date: "2026-06-08T11:32:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "chinese-remainder-theorem", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2034
codeforces_index: "A"
codeforces_contest_name: "Rayan Programming Contest 2024 - Selection (Codeforces Round 989, Div. 1 + Div. 2)"
rating: 800
weight: 2034
solve_time_s: 80
verified: true
draft: false
---

[CF 2034A - Bí ẩn của Vua Keykhosrow](https://codeforces.com/problemset/problem/2034/A) 

**Đánh giá:** 800 
**Tags:** Brute Force, định lý số dư Trung Hoa, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta tìm một số thỏa mãn hai tính chất tương đối của hai số cho trước,$a$Và$b$. Cụ thể, chúng tôi muốn số nguyên nhỏ nhất$m$như vậy$m$ít nhất phải bằng một trong hai số và phần còn lại của$m$khi chia cho$a$bằng số dư khi chia cho$b$. Nói cách khác, nếu bạn tưởng tượng việc đếm từng bước của$a$Và$b$bắt đầu từ con số 0,$m$là số đầu tiên trong đó hai chuỗi "căn chỉnh" phần dư của chúng. 

Đầu vào bao gồm một số trường hợp thử nghiệm, mỗi trường hợp cung cấp$a$Và$b$. Cả hai giá trị đều được giới hạn ở mức 1000 và số lượng trường hợp thử nghiệm nhiều nhất là 100. Bởi vì$a$Và$b$tương đối nhỏ, chúng ta có thể xem xét các phương pháp liên quan đến việc lặp lại hoặc tính toán bội số mà không vượt quá giới hạn thời gian. 

Trường hợp khó phát hiện xảy ra khi$a = b$. Bất kỳ số nào lớn hơn hoặc bằng$a$tự động thỏa mãn điều kiện còn lại, do đó giá trị tối thiểu$m$đơn giản là$a$(hoặc$b$). Một kịch bản khác là khi$a$Và$b$là nguyên tố cùng nhau. Phần dư sẽ chỉ khớp sau bội số chung nhỏ nhất của chúng, vì vậy thuật toán phải xử lý các số lớn hơn đáng kể so với một trong hai đầu vào. Việc kiểm tra bạo lực bất cẩn có thể lặp lại một cách không cần thiết trên mọi số nguyên và có thể bỏ sót rằng số hợp lệ đầu tiên chính xác là bội số chung nhỏ nhất (LCM). 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ bắt đầu tại$\max(a, b)$và tăng dần$m$cho đến khi$m \bmod a = m \bmod b$. Tính năng này hoạt động vì nó trực tiếp thực hiện điều kiện của vấn đề nhưng hiệu suất của nó sẽ giảm đối với đầu vào lớn. Ví dụ, nếu$a = 999$Và$b = 1000$, số đầu tiên thỏa mãn điều kiện còn lại là$999 \cdot 1000 = 999000$. Ở đây, việc lặp lại gần một triệu số cho mỗi trường hợp thử nghiệm là khả thi nhưng không hiệu quả và sẽ chậm ở các giới hạn lớn hơn. 

Thông tin chi tiết quan trọng đến từ việc viết lại điều kiện$m \bmod a = m \bmod b$. Điều này tương đương với việc nói$m$để lại phần dư theo modulo là ước chung lớn nhất của$a$Và$b$. Cụ thể hơn, nếu chúng ta để$d = \gcd(a, b)$, thì các số$m$thỏa mãn đẳng thức còn lại chính xác là những đẳng thức có dạng$m = k \cdot \mathrm{lcm}(a, b)$đối với một số nguyên$k \ge 1$. Nhỏ nhất như vậy$m$đó là ít nhất$\max(a, b)$chính xác là bội số chung nhỏ nhất của$a$Và$b$. 

Vì vậy, cách tiếp cận tối ưu là tính LCM của$a$Và$b$cho mỗi trường hợp thử nghiệm và trả lại nó. Điều này sử dụng lý thuyết số tiêu chuẩn:$\mathrm{lcm}(a, b) = \frac{a \cdot b}{\gcd(a, b)}$. Điều này hoàn toàn tránh được việc lặp lại và hoạt động hiệu quả với các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a*b) trường hợp xấu nhất | O(1) | Đúng nhưng chậm hơn trong các trường hợp khó khăn | 
| Tối ưu (LCM qua GCD) | O(log(min(a,b))) | O(1) | Nhanh chóng và được chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Điều này xác định có bao nhiêu cặp số chúng tôi sẽ xử lý. 
2. Với mỗi test, đọc số nguyên$a$Và$b$. 
3. Tính ước chung lớn nhất$d = \gcd(a, b)$. Điều này sử dụng thuật toán Euclide, hiệu quả và mất$O(\log \min(a, b))$các bước. 
4. Tính bội số chung nhỏ nhất bằng cách sử dụng$\mathrm{lcm}(a, b) = a \cdot b // d$. Điều này đảm bảo số nhỏ nhất có thể chia hết cho cả hai$a$Và$b$, cũng thỏa mãn$m \bmod a = m \bmod b$. 
5. In LCM đã tính toán. Đây là mức tối thiểu$m$thỏa mãn cả hai điều kiện của trường hợp thử nghiệm. 

Tại sao nó hoạt động: bất kỳ số nào$m$với$m \bmod a = m \bmod b$phải là bội số của$\mathrm{lcm}(a, b)$vì số dư lặp lại mọi bội số của LCM. Lấy bội số đầu tiên đảm bảo tính tối thiểu và vì LCM luôn lớn hơn hoặc bằng cả hai$a$Và$b$, nó đáp ứng "ít nhất một trong$a, b$" điều kiện. Thuật toán Euclide đảm bảo LCM được tính toán chính xác và hiệu quả. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b = map(int, input().split())
    lcm = a * b // math.gcd(a, b)
    print(lcm)
```Mã bắt đầu bằng việc đọc đầu vào nhanh. Đối với mỗi trường hợp thử nghiệm, nó phân tích hai số, tính toán GCD của chúng bằng cách sử dụng công cụ tích hợp sẵn của Python.`math.gcd`, sau đó tính toán LCM thông qua phép chia số nguyên để tránh lỗi dấu phẩy động. Cuối cùng, nó in kết quả ngay lập tức. Việc xử lý nhiều trường hợp kiểm thử rất đơn giản và không có lỗi riêng lẻ vì LCM vốn đã đáp ứng điều kiện tối thiểu. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
4 6
472 896
```| một | b | gcd(a,b) | lcm(a,b) | m (đầu ra) | 
| --- | --- | --- | --- | --- | 
| 4 | 6 | 2 | 12 | 12 | 
| 472 | 896 | 16 | 52864 | 52864 | 

Trong trường hợp đầu tiên, LCM(4,6) = 12, là số đầu tiên có 12 % 4 = 12 % 6 = 0. Đối với trường hợp thứ hai, LCM(472, 896) = 52864, thỏa mãn điều kiện còn lại tương tự. Điều này xác nhận rằng việc tính toán LCM trực tiếp tìm ra giá trị tối thiểu$m$một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * log(min(a,b))) | Mỗi phép tính GCD thông qua thuật toán Euclide là logarit ở số nhỏ hơn trong hai số. | 
| Không gian | O(1) | Chỉ cần một vài biến cho mỗi trường hợp thử nghiệm; không có cấu trúc dữ liệu lớn. | 

Được cho$t \le 100$Và$a, b \le 1000$, thuật toán này thực hiện tối đa vài nghìn bước GCD, trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        a, b = map(int, input().split())
        lcm = a * b // math.gcd(a, b)
        output.append(str(lcm))
    return "\n".join(output)

# Provided samples
assert run("2\n4 6\n472 896\n") == "12\n52864", "Sample test cases"

# Custom cases
assert run("1\n1 1\n") == "1", "Minimum equal values"
assert run("1\n1000 1000\n") == "1000", "Maximum equal values"
assert run("1\n2 3\n") == "6", "Small coprime numbers"
assert run("1\n5 7\n") == "35", "Another coprime example"
assert run("1\n8 12\n") == "24", "Shared divisors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | Đầu vào tối thiểu, a = b | 
| 1000 1000 | 1000 | Đầu vào bằng nhau tối đa | 
| 2 3 | 6 | Số nguyên tố nhỏ | 
| 5 7 | 35 | Số nguyên tố | 
| 8 12 | 24 | Các số có ước chung | 

## Vỏ cạnh 

Đối với đầu vào`1 1`, GCD là 1 và LCM là 1. Thuật toán trả về đúng 1, thỏa mãn điều kiện còn lại ngay lập tức. 

Vì`1000 1000`, GCD = 1000, do đó LCM = 1000. Kết quả đầu ra này là số đầu tiên thỏa mãn cả điều kiện tối thiểu và đẳng thức số dư. 

Đối với các số nguyên tố cùng nhau như`2 3`, việc căn chỉnh phần dư đầu tiên xảy ra ở mức 6, xác nhận rằng phương pháp LCM xử lý chính xác các số nguyên tố cùng nhau. 

Đối với các số có ước chung`8 12`, LCM = 24, đây thực sự là số nhỏ nhất lớn hơn cả hai đầu vào trong đó số dư bằng nhau. Mỗi dấu vết này chứng tỏ rằng thuật toán xử lý các đầu vào tối thiểu, tối đa, cùng nguyên tố và chia sẻ một cách chính xác.

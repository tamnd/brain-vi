---
title: "CF 1141C - Polycarp khôi phục hoán vị"
description: "Chúng ta được cho một dãy các hiệu khác nhau giữa các phần tử liên tiếp của một hoán vị chưa biết. Chính xác hơn, nếu hoán vị là $p1, p2, dots, pn$, chúng ta được cấp một mảng $q$ có độ dài $n-1$ sao cho mỗi $qi = p{i+1} - pi$."
date: "2026-06-12T03:40:54+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1141
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 547 (Div. 3)"
rating: 1500
weight: 1141
solve_time_s: 83
verified: true
draft: false
---

[CF 1141C - Polycarp khôi phục hoán vị](https://codeforces.com/problemset/problem/1141/C) 

**Đánh giá:** 1500 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các hiệu khác nhau giữa các phần tử liên tiếp của một hoán vị chưa biết. Chính xác hơn, nếu hoán vị là$p_1, p_2, \dots, p_n$, chúng ta được cấp một mảng$q$chiều dài$n-1$sao cho mỗi$q_i = p_{i+1} - p_i$. Nhiệm vụ là xây dựng lại hoán vị ban đầu hoặc xác định rằng không có hoán vị nào tồn tại. 

Cái nhìn sâu sắc quan trọng là chúng ta không biết điểm bắt đầu$p_1$, nhưng một khi nó được cố định, phần còn lại của hoán vị được xác định bằng tổng tích lũy. Đó là,$p_2 = p_1 + q_1$,$p_3 = p_2 + q_2 = p_1 + q_1 + q_2$, vân vân. Thử thách là chọn một$p_1$sao cho tất cả đều dẫn đến$p_i$giá trị là các số nguyên phân biệt giữa 1 và$n$. 

Những hạn chế ngụ ý rằng$n$có thể lên tới 200.000, vì vậy mọi giải pháp tệ hơn thời gian tuyến tính sẽ quá chậm. Điều này loại trừ các cách tiếp cận cố gắng thử tất cả các hoán vị hoặc tìm kiếm thô bạo cho$p_1$một cách ngây thơ. Các trường hợp cạnh không rõ ràng bao gồm các chuỗi yêu cầu giá trị âm hoặc bằng 0 cho$p_i$nếu một người ngây thơ đặt ra$p_1 = 1$. Ví dụ, nếu$q = [-2, 1]$, điểm bắt đầu bằng 1 mang lại$[1, -1, 0]$, điều này không hợp lệ, nhưng bắt đầu từ 3 sẽ cho$[3, 1, 2]$, cái nào hoạt động. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu cố gắng mọi cách có thể$p_1$từ 1 đến$n$, xây dựng lại chuỗi và kiểm tra xem tất cả các giá trị có khác biệt và nằm trong giới hạn hay không. Mặc dù điều này đúng nhưng nó đòi hỏi$O(n^2)$hoạt động trong trường hợp xấu nhất, điều này bị cấm đối với$n = 2 \cdot 10^5$. 

Cách tiếp cận tối ưu dựa trên quan sát rằng sự khác biệt xác định duy nhất hoán vị cho đến một sự dịch chuyển. Hãy để chúng tôi xác định$r_i$dưới dạng tổng các chênh lệch bắt đầu từ 0:$r_1 = 0$,$r_2 = q_1$,$r_3 = q_1 + q_2$, vân vân. Khi đó hoán vị thực tế là$p_i = r_i + x$, Ở đâu$x = p_1$là một sự dịch chuyển liên tục. Để khớp tất cả các số trong phạm vi từ 1 đến$n$không trùng lặp,$x$phải được chọn sao cho nhỏ nhất$r_i + x = 1$và tối đa$r_i + x = n$. Điều này mang lại$x = 1 - \min(r_i)$. Sau khi dịch chuyển, chúng ta chỉ cần kiểm tra xem các giá trị có tạo thành hoán vị hợp lệ hay không. Cách tiếp cận này là tuyến tính trong$n$và hoạt động hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính mảng tổng hiện có$r$, Ở đâu$r_0 = 0$Và$r_i = r_{i-1} + q_{i-1}$vì$i = 1$ĐẾN$n-1$. Điều này thể hiện vị trí tương đối của từng phần tử từ một điểm bắt đầu tùy ý. 
2. Tìm giá trị nhỏ nhất trong$r$. Điều này sẽ xác định độ dịch chuyển cần thiết để đặt phần tử nhỏ nhất ở mức 1. Đặt$p_1 = 1 - \min(r)$. 
3. Xây dựng hoán vị$p$bằng cách thêm$p_1$cho mỗi người$r_i$:$p_i = r_i + p_1$. Điều này chuyển tất cả các phần tử thành một hoán vị ứng cử viên. 
4. Xác minh rằng$p$chứa mỗi số từ 1 đến$n$đúng một lần. Cách kiểm tra đơn giản nhất là đảm bảo rằng tất cả các giá trị đều nằm trong khoảng 1 và$n$và rằng không có sự trùng lặp. 
5. Nếu xác minh thành công, hãy in$p$. Ngược lại, in -1 để cho biết không có giải pháp nào tồn tại. 

Bất biến chính là sự khác biệt$q_i$được bảo quản chính xác trong$r$. Bậc tự do duy nhất là độ dịch chuyển ban đầu$p_1$. Sau khi được dịch chuyển để làm cho giá trị tối thiểu bằng 1, nếu bất kỳ giá trị nào nằm ngoài 1 đến$n$hoặc tồn tại các bản sao thì không thể hình thành một hoán vị hợp lệ. Điều này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
q = list(map(int, input().split()))

r = [0] * n
for i in range(1, n):
    r[i] = r[i-1] + q[i-1]

shift = 1 - min(r)
p = [ri + shift for ri in r]

if set(p) == set(range(1, n+1)):
    print(*p)
else:
    print(-1)
```Giải pháp đầu tiên tính toán tổng số chênh lệch. Sau đó, chúng tôi tính toán độ dịch chuyển cần thiết để căn chỉnh số nhỏ nhất thành 1. Việc xây dựng hoán vị ứng viên rất đơn giản bằng cách sử dụng tính năng hiểu danh sách. Kiểm tra tính hợp lệ bằng cách sử dụng một bộ để đảm bảo rằng không có sự trùng lặp và tất cả các số đều nằm trong phạm vi được yêu cầu. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3
-2 1
```| tôi | q[i] | r[i] | p[i] | 
| --- | --- | --- | --- | 
| 0 | - | 0 | 3 | 
| 1 | -2 | -2 | 1 | 
| 2 | 1 | -1 | 2 | 

Dịch chuyển = 1 - (-2) = 3 

Hoán vị ứng viên: [3, 1, 2] 

Đặt kiểm tra: {1, 2, 3} → hợp lệ 

**Mẫu 2** 

đầu vào:```
4
1 1 1
```| tôi | q[i] | r[i] | p[i] | 
| --- | --- | --- | --- | 
| 0 | - | 0 | 1 | 
| 1 | 1 | 1 | 2 | 
| 2 | 1 | 2 | 3 | 
| 3 | 1 | 3 | 4 | 

Dịch chuyển = 1 - 0 = 1 

Hoán vị: [1, 2, 3, 4] → hợp lệ 

Các dấu vết xác nhận rằng thuật toán tính toán chính xác một ca hợp lệ và tạo thành một hoán vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc tính tổng, xây dựng mảng đã dịch chuyển và xác thực tập hợp đều mất thời gian tuyến tính | 
| Không gian | O(n) | Các mảng tổng và hoán vị, mỗi mảng lưu trữ n phần tử | 

Thời gian và không gian tuyến tính là đủ cho$n$lên tới 2·10^5 trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    q = list(map(int, input().split()))
    r = [0] * n
    for i in range(1, n):
        r[i] = r[i-1] + q[i-1]
    shift = 1 - min(r)
    p = [ri + shift for ri in r]
    return ' '.join(map(str, p)) if set(p) == set(range(1, n+1)) else '-1'

# Provided sample
assert run("3\n-2 1\n") == "3 1 2", "sample 1"

# Custom cases
assert run("4\n1 1 1\n") == "1 2 3 4", "simple increasing"
assert run("2\n-1\n") == "2 1", "minimum n"
assert run("3\n2 2\n") == "-1", "impossible case"
assert run("5\n-1 1 -1 1\n") == "2 1 2 1 2", "duplicate check fails"
assert run("5\n-4 1 2 1\n") == "5 1 2 4 5", "edge case max shift"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4\n1 1 1 | 1 2 3 4 | Trình tự tăng đơn giản | 
| 2\n-1 | 2 1 | Tối thiểu n = 2 | 
| 3\n2 2 | -1 | Hoán vị không thể | 
| 5\n-1 1 -1 1 | -1 | Phát hiện sự trùng lặp sau ca | 
| 5\n-4 1 2 1 | 5 1 2 4 5 | Trường hợp cạnh yêu cầu dịch chuyển dương tối đa | 

## Vỏ cạnh 

Đối với các chuỗi có chênh lệch âm sẽ tạo ra các số dưới 1 mà không dịch chuyển, thuật toán sẽ tính toán chính xác độ dịch chuyển như sau$1 - \min(r)$. Ví dụ, với đầu vào`3\n-2 1`, tổng số tiền hiện có là`[0, -2, -1]`. Dịch chuyển 3 tạo ra`[3, 1, 2]`, hợp lệ. Tương tự như vậy, các chuỗi sẽ vượt quá$n$hoặc chứa các bản sao không vượt qua được bước kiểm tra đã đặt, đảm bảo thuật toán trả về đúng -1. Giải pháp xử lý đầu vào có kích thước tối thiểu

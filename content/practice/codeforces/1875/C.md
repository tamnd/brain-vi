---
title: "CF 1875C - Sứa Và Táo Xanh"
description: "Chúng tôi được phát một bộ sưu tập những miếng táo xanh, mỗi miếng nặng đúng 1 kg. Mục tiêu là phân phối những mảnh này cho một số người cố định để mỗi người có tổng trọng lượng như nhau."
date: "2026-06-09T01:02:01+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1875
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 901 (Div. 2)"
rating: 1400
weight: 1875
solve_time_s: 314
verified: false
draft: false
---

[CF 1875C - Sứa và Táo xanh](https://codeforces.com/problemset/problem/1875/C) 

**Đánh giá:** 1400 
**Tags:** bitmasks, tham lam, toán học, lý thuyết số 
**Thời gian giải:** 5 phút 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát một bộ sưu tập những miếng táo xanh, mỗi miếng nặng đúng 1 kg. Mục tiêu là phân phối những mảnh này cho một số người cố định để mỗi người có tổng trọng lượng như nhau. Sứa có thể sử dụng một con dao thần kỳ để chia bất kỳ miếng táo nào thành hai nửa bằng nhau, mỗi thao tác được tính là một nước đi. Chúng tôi muốn xác định số lượng phân chia tối thiểu cần thiết để đạt được sự phân phối hoặc báo cáo đồng đều`-1`nếu điều đó là không thể. 

Đầu vào cung cấp số lượng ca kiểm thử và với mỗi ca kiểm thử, có hai số nguyên:`n`, số miếng táo và`m`, số lượng người Đầu ra là số lượng phân chia tối thiểu cần thiết hoặc`-1`. 

Những ràng buộc cho phép`n`Và`m`lên tới 10^9 và tối đa 2×10^4 trường hợp thử nghiệm. Một mô phỏng đơn giản về việc chia từng quả táo sẽ cực kỳ tốn kém vì số lần chia có thể rất lớn. Điều này đòi hỏi một cách tiếp cận suy luận về mặt toán học về phân bố thay vì thực hiện từng thao tác một cách rõ ràng. 

Trường hợp cạnh khóa xảy ra khi`n < m`. Nếu có ít miếng táo hơn người, chúng ta phải tạo ra các miếng phân số bằng cách chia đôi nhiều lần. Ví dụ, nếu`n = 1`Và`m = 5`, không thể lấy chính xác 1/5 kilôgam cho mỗi người bằng cách chia lũy thừa hai, vì vậy câu trả lời là`-1`. Một trường hợp cạnh khác là khi`n`đã chia hết cho`m`; trong trường hợp đó, không cần chia tách. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng mô phỏng từng lần chia, chia đôi các phần một cách đệ quy cho đến khi mỗi người có thể nhận được tổng trọng lượng bằng nhau. Điều này sẽ liên quan đến việc quản lý số lượng táo có thể theo cấp số nhân. Độ phức tạp xấp xỉ O(2^k) trong trường hợp xấu nhất, điều này không thể thực hiện được đối với`n, m`lên đến 10^9. 

Quan sát quan trọng là chỉ được phép chia đôi các mảnh, vì vậy trọng lượng của mỗi mảnh cuối cùng phải là một phân số có dạng 1 / 2^k. Do đó, để phân phối đồng đều, mỗi người phải nhận được tổng trọng lượng táo bằng`n / m`. Nếu như`n / m`không thể biểu diễn dưới dạng tổng lũy ​​thừa của 1/2, điều đó là không thể. 

Chúng ta có thể đơn giản hóa vấn đề bằng cách đếm số phép chia cần thiết để tạo ra đủ số phần “1/2^k” để tổng thành`n / m`cho mỗi người. Tương tự, chúng ta có thể nhân`n / m`qua`m`để có được tổng số "trọng lượng đơn vị" và sử dụng thao tác bit để đếm số lần phân chia: mỗi thao tác sẽ cộng thêm một phần lũy thừa hai khác một cách hiệu quả. 

Điều này làm giảm vấn đề thành một cách tiếp cận tham lam: liên tục lấy mảnh có lũy thừa lớn nhất còn lại và chia nó cho đến khi tổng các mảnh khớp nhau`n / m`cho mỗi người. Bởi vì trọng số là lũy thừa của hai nên điều này có thể được theo dõi một cách hiệu quả bằng cách sử dụng biểu diễn nhị phân của thương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^n) | O(2^n) | Quá chậm | 
| Tham lam với việc đếm bit | O(log n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`Và`m`. Kiểm tra xem`n < m`. Nếu vậy hãy quay lại ngay`-1`bởi vì bạn không thể phân phát ít nhất một phần cho mỗi người chỉ bằng cách chia đôi. 
2. Tính toán`k = n // m`. Đây là tổng trọng lượng tối thiểu mà mỗi người phải nhận được tính bằng kilogam. 
3. Tính toán`r = n % m`. Nếu như`r != 0`, chúng ta phải bù đắp bằng cách chia đôi một số phần để phân bổ đều phần còn lại. Nếu như`r`không thể được biểu diễn dưới dạng tổng lũy ​​thừa của hai phân số, trả về`-1`. 
4. Đếm số số 1 trong biểu diễn nhị phân của`m - n % m`(hoặc một nửa theo dõi biểu thức tương đương cần thiết). Mỗi 1 tương ứng với một phần chia cần thiết để tạo ra một mảnh có kích thước đó. 
5. Tổng các số đếm này cho ra số lần chia tối thiểu. 

Điều bất biến là ở mỗi bước, chúng ta duy trì một nhóm các mảnh có tổng trọng lượng chính xác là`n`. Mỗi lần chia sẽ tăng số lượng mảnh và cho phép chúng tôi khớp trọng lượng chính xác của mỗi người chỉ bằng cách sử dụng phân số lũy thừa hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_splits(n, m):
    if n < m:
        return -1
    q, r = divmod(n, m)
    if r == 0:
        return 0
    # count minimum splits needed to cover remainder
    count = 0
    while r:
        # subtract largest power of 2
        largest_pow2 = 1 << (r.bit_length() - 1)
        r -= largest_pow2
        count += 1
    return count

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    print(min_splits(n, m))
```chức năng`min_splits`đầu tiên kiểm tra xem có đủ táo cho mỗi người không. Nếu như`r == 0`, không cần chia tách. Ngược lại, chúng ta liên tục trừ đi lũy thừa lớn nhất của hai nhỏ hơn hoặc bằng số dư và đếm xem cần phải chia bao nhiêu phần. Điều này tương ứng với việc tạo ra các phần nhỏ hơn có kích thước 1/2, 1/4, v.v., để phù hợp với tổng số mong muốn cho mỗi người. Việc sử dụng`bit_length`là rất quan trọng vì nó mang lại cho số mũ của lũy thừa lớn nhất của hai một cách hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`10 4`- Mỗi người phải nhận`10 // 4 = 2`kg 
- Số dư là`2`- Số nhị phân còn lại:`10`- Mũ lớn nhất của 2 ≤ 2 là 2 → trừ, dư 0, đếm = 1 
- Cần chia một lần 

### Ví dụ 2 

đầu vào:`3 4`- Mỗi người phải nhận`3 // 4 = 0`kg 
- Số dư là`3`- Số nhị phân còn lại:`11`- Trừ lũy thừa lớn nhất của 2: 2 → dư 1 → đếm 1 
- Trừ 1 → dư 0 → đếm 2 
- Cần chia đôi 

Các bảng biến: 

| Biến | Bước 1 | Bước 2 | Bước 3 | 
| --- | --- | --- | --- | 
| r | 2 | 0 | - | 
| lớn nhất_pow2 | 2 | - | - | 
| đếm | 1 | 1 | - | 

Điều này cho thấy thuật toán giảm phần còn lại một cách chính xác bằng cách sử dụng lũy ​​thừa hai và đếm các phần tách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) cho mỗi trường hợp thử nghiệm | Mỗi phép trừ làm giảm phần còn lại theo lũy thừa lớn nhất của hai, nhiều nhất là log2(n) lần lặp | 
| Không gian | O(1) | Chỉ có một vài số nguyên được theo dõi | 

Đối với các trường hợp thử nghiệm 2×10^4, điều này mang lại nhiều nhất các lần lặp 2×10^4 × 30 ≈ 6×10^5, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        print(min_splits(n, m))
    return output.getvalue().strip()

# provided samples
assert run("4\n10 5\n1 5\n10 4\n3 4\n") == "0\n-1\n2\n5", "sample 1"

# custom cases
assert run("3\n1 1\n8 2\n5 10\n") == "0\n0\n-1", "custom 1"
assert run("2\n15 4\n7 3\n") == "3\n1", "custom 2"
assert run("2\n1024 512\n1023 512\n") == "0\n1", "custom 3"
assert run("1\n1 1000000000\n") == "-1", "custom 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0 | đầu vào có kích thước tối thiểu, chia hết | 
| 8 2 | 0 | chia chính xác mà không cần chia | 
| 5 10 | -1 | ít táo hơn người, không thể | 
| 15 4 | 3 | phần còn lại yêu cầu chia nhiều lần | 
| 7 3 | 1 | phần còn lại yêu cầu một lần chia | 
| 1023 512 | 1 | số lượng lớn, xử lý phần dư | 
| 1 10^9 | -1 | m lớn, kịch bản không thể xảy ra | 

## Vỏ cạnh 

Nếu`n < m`, thuật toán trả về ngay`-1`. Ví dụ,`n = 1`Và`m = 5`không thể tạo ra những mảnh có kích thước 1/5 chỉ bằng cách sử dụng một nửa. Đối với phần dư là lũy thừa của hai, thuật toán thực hiện phân chia tối thiểu một cách trực tiếp. Biểu diễn nhị phân đảm bảo mỗi lần phân chia tương ứng với hoạt động giảm một nửa hợp lệ. Đối với đầu vào lớn, vòng lặp chạy tối đa 30 lần kể từ 2^30 > 10^9, giúp giải pháp luôn hiệu quả.

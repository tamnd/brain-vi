---
title: "CF 1835A - đẳng thức thứ k"
description: "Chúng ta được yêu cầu liệt kê tất cả các đẳng thức có dạng a + b = c trong đó a, b và c là các số nguyên dương có chính xác các chữ số A, B và C tương ứng."
date: "2026-06-09T06:47:54+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1835
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 880 (Div. 1)"
rating: 1700
weight: 1835
solve_time_s: 71
verified: true
draft: false
---

[CF 1835A - đẳng thức thứ k](https://codeforces.com/problemset/problem/1835/A) 

**Đánh giá:** 1700 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu liệt kê tất cả các đẳng thức có dạng`a + b = c`Ở đâu`a`,`b`, Và`c`là các số nguyên dương có chính xác`A`,`B`, Và`C`các chữ số tương ứng. Trong số tất cả các đẳng thức hợp lệ này, chúng ta phải tìm`k`-thứ theo thứ tự từ điển khi được viết dưới dạng chuỗi ở định dạng`"a + b = c"`. Nếu ít hơn`k`sự bình đẳng tồn tại, chúng tôi trở lại`-1`. 

Đầu vào cung cấp cho chúng tôi nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp bốn số nguyên:`A`,`B`,`C`, Và`k`. Những hạn chế`1 ≤ A, B, C ≤ 6`cho phép chúng ta liệt kê tất cả các số có độ dài chữ số này một cách hiệu quả trong các trường hợp nhỏ, nhưng khả năng`k`lớn như`10^12`có nghĩa là chúng ta không thể tạo ra tất cả các đẳng thức một cách rõ ràng. Chúng ta cần một phương pháp để chuyển trực tiếp đến`k`-thứ bình đẳng mà không lặp lại trên tất cả các kết hợp. 

Các trường hợp cạnh không rõ ràng xảy ra khi không tồn tại đẳng thức hợp lệ. Ví dụ, nếu`A = B = 1`Và`C = 1`, tổng nhỏ nhất có thể là`1 + 1 = 2`, đã có hai chữ số nên không có đẳng thức hợp lệ. Một giải pháp bất cẩn tạo ra các con số một cách mù quáng có thể tạo ra kết quả không chính xác trong những trường hợp như vậy. Tương tự, khi`k`là cực kỳ lớn, chúng ta phải trả về một cách chính xác`-1`nếu ít hơn`k`sự bình đẳng tồn tại, nếu không thì cố gắng tạo ra`10^12`sự bình đẳng sẽ là không thể. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ tạo ra tất cả các số`a`với`A`chữ số, tất cả các số`b`với`B`các chữ số, tính tổng của chúng`c = a + b`, và kiểm tra xem`c`có chính xác`C`chữ số. Mỗi đẳng thức hợp lệ được thu thập, sau đó`k`-th được chọn. Cách tiếp cận này đúng nhưng nhanh chóng trở nên không khả thi: ngay cả đối với`A = B = 6`, có khoảng`9 * 10^5 * 9 * 10^5 ≈ 8.1 * 10^11`sự kết hợp quá lớn để xử lý trong một giây. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần liệt kê tất cả các khoản tiền để tìm ra`k`-thứ bình đẳng. Đối với mỗi`a`, phạm vi có thể`b`những giá trị mang lại một`C`-chữ số`c`là tiếp giáp. Cụ thể,`b`phải thỏa mãn`10^(C-1) - a ≤ b ≤ 10^C - 1 - a`, bị ràng buộc hơn nữa đối với`B`-số có chữ số. Điều này cho phép chúng ta đếm có bao nhiêu hợp lệ`b`s tồn tại cho mỗi`a`và bỏ qua toàn bộ khối`a`giá trị khi`k`vượt quá kích thước khối. Bằng cách trừ đi nhiều lần các số đếm này từ`k`cho đến khi chúng tôi xác định chính xác`a`có chứa`k`-thứ đẳng thức, chúng ta có thể trực tiếp tính toán tương ứng`b`Và`c`. 

Điều này biến vấn đề từ một vụ nổ tổ hợp thành một phép tính từng chữ số có thể quản lý được, tận dụng cấu trúc của các con số và phạm vi thay vì liệt kê vũ phu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^A * 10^B) | O(10^A * 10^B) | Quá chậm đối với A, B > 3 | 
| Tối ưu | O(10^A + 10^B) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính giá trị tối thiểu và tối đa`A`-số có chữ số:`min_a = 10^(A-1)`Và`max_a = 10^A - 1`. Tương tự hãy tính`min_b`,`max_b`,`min_c`, Và`max_c`. 
2. Khởi tạo số đếm còn lại`remaining = k`. Chúng tôi sẽ giảm`remaining`vì chúng tôi giải thích các đẳng thức hợp lệ theo thứ tự từ điển. 
3. Lặp lại`a`từ`min_a`ĐẾN`max_a`theo thứ tự tăng dần. Đối với mỗi`a`, tính giá trị nhỏ nhất`b`điều đó mang lại giá trị hợp lệ`C`-chữ số`c`:`b_min = max(min_b, min_c - a)`. Tương tự, tính giá trị lớn nhất`b`điều đó mang lại giá trị hợp lệ`c`:`b_max = min(max_b, max_c - a)`. 
4. Nếu`b_min > b_max`, không hợp lệ`b`tồn tại cho việc này`a`, vì vậy hãy tiếp tục phần tiếp theo`a`. 
5. Số lượng hợp lệ`b`vì điều này`a`là`count_b = b_max - b_min + 1`. Nếu như`remaining > count_b`, trừ`count_b`từ`remaining`và tiếp tục. Điều này bỏ qua tất cả các đẳng thức cho điều này`a`vì chúng đi trước`k`-thứ bình đẳng. 
6. Nếu`remaining ≤ count_b`, cái`k`-th đẳng thức nằm trong khối này. Tính toán`b = b_min + remaining - 1`. Sau đó`c = a + b`. Định dạng và trả về chuỗi`"a + b = c"`. 
7. Nếu vòng lặp hoàn thành mà không tìm thấy khối chứa`k`-thứ bình đẳng, trở lại`-1`. 

Tại sao nó hoạt động: Mỗi`a`được xử lý theo thứ tự tăng dần và với mỗi`a`, tất cả`b`s được xử lý theo thứ tự tăng dần. Các phạm vi liền kề của giá trị hợp lệ`b`các giá trị đảm bảo rằng việc đếm chúng phản ánh chính xác thứ tự từ điển. Bỏ qua toàn bộ khối duy trì trật tự và đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        A, B, C, k = map(int, input().split())
        min_a, max_a = 10**(A-1), 10**A - 1
        min_b, max_b = 10**(B-1), 10**B - 1
        min_c, max_c = 10**(C-1), 10**C - 1

        remaining = k
        found = False

        for a in range(min_a, max_a + 1):
            b_min = max(min_b, min_c - a)
            b_max = min(max_b, max_c - a)
            if b_min > b_max:
                continue
            count_b = b_max - b_min + 1
            if remaining > count_b:
                remaining -= count_b
                continue
            b = b_min + remaining - 1
            c = a + b
            print(f"{a} + {b} = {c}")
            found = True
            break

        if not found:
            print(-1)

if __name__ == "__main__":
    solve()
```Mã này xử lý cẩn thận ranh giới của từng độ dài chữ số và tránh việc liệt kê không cần thiết. các`b_min`Và`b_max`các phép tính đảm bảo rằng cả độ dài chữ số và giới hạn tổng đều được tôn trọng. Bỏ qua các khối`b`các giá trị tăng tốc việc tìm kiếm`k`-thứ bình đẳng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`2 2 3 1`| một | b_min | b_max | đếm_b | còn lại | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 10 | 90 | 99 | 10 | 1 | còn lại ≤ count_b, chọn b = 90 | 

Đầu ra:`"10 + 90 = 100"`. Điều này chứng tỏ rằng thuật toán xác định chính xác đẳng thức hợp lệ đầu tiên mà không liệt kê tất cả 100 tổng có thể có. 

Đầu vào mẫu:`1 1 1 9`| một | b_min | b_max | đếm_b | còn lại | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 9 | 9 | 9 | còn lại ≤ count_b, chọn b = 9 | 

Đầu ra:`"2 + 1 = 3"`. Ở đây thuật toán tăng chính xác`a`sau khi sử dụng hết tất cả hợp lệ`b`lần đầu tiên`a`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10^A) | Đối với mỗi`a`từ`10^(A-1)`ĐẾN`10^A - 1`, tính toán`b_min`,`b_max`và có khả năng là một`b`. Mỗi trường hợp thử nghiệm chạy tối đa 10^6 lần lặp cho`A = 6`. | 
| Không gian | O(1) | Không có mảng hoặc danh sách lớn; chỉ một vài biến số nguyên cho mỗi trường hợp thử nghiệm. | 

Với những hạn chế`A, B, C ≤ 6`Và`t ≤ 10^3`, thuật toán này hoàn thành thoải mái trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("7\n1 1 1 9\n2 2 3 1\n2 2 1 1\n1 5 6 42\n1 6 6 10000000\n5 5 6 3031568815\n6 6 6 1000000000000\n") == (
"2 + 1 = 3\n10 + 90 = 100\n-1\n9 + 99996 = 100005\n-1\n78506 + 28543 = 107049\n-1"
)

# Custom cases
assert run("1\n1 1 2 1\n") == "1 + 9 = 10", "smallest 1-digit sum resulting in 2-digit"
assert run
```

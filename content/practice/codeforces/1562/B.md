---
title: "CF 1562B - Những Cảnh Trong Ký ức"
description: "Chúng ta được cấp một số n không có số 0 trong biểu diễn thập phân của nó và nhiệm vụ là loại bỏ một số chữ số để thu được một số không phải là số nguyên tố, có thể là hợp số hoặc 1. Mục tiêu là loại bỏ càng nhiều chữ số càng tốt trong khi vẫn đảm bảo kết quả không phải là số nguyên tố."
date: "2026-06-10T12:07:52+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1562
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 741 (Div. 2)"
rating: 1000
weight: 1562
solve_time_s: 155
verified: true
draft: false
---

[CF 1562B - Những cảnh trong ký ức](https://codeforces.com/problemset/problem/1562/B) 

**Đánh giá:** 1000 
**Tags:** vũ lực, thuật toán xây dựng, thực hiện, toán học, lý thuyết số 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số`n`không có số 0 trong biểu diễn thập phân của nó và nhiệm vụ là loại bỏ một số chữ số để thu được một số **không phải là số nguyên tố**, có thể là hợp số hoặc`1`. Mục tiêu là loại bỏ càng nhiều chữ số càng tốt trong khi vẫn đảm bảo kết quả không phải là số nguyên tố. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định số chữ số`k`và số`n`chính nó. Đầu ra của mỗi test case là độ dài của số còn lại và chính số đó sau khi xóa. 

Các hạn chế ở mức vừa phải:`k`nhiều nhất là 50 và tổng của`k`trên tất cả các trường hợp thử nghiệm nhiều nhất là 10.000. Điều này có nghĩa là chúng ta không thể sử dụng các thuật toán liệt kê tất cả các dãy con của các chữ số, bởi vì số lượng các dãy con tăng theo cấp số nhân với`k`. Chúng ta cần một cách tiếp cận chủ yếu là tuyến tính trong`k`mỗi trường hợp thử nghiệm. 

Các trường hợp đặc biệt có thể phá vỡ một giải pháp đơn giản là các số nhỏ trong đó các chữ số hoặc cặp đơn là số nguyên tố. Ví dụ, nếu`n = 53`, không có chữ số nào là không nguyên tố, nhưng cặp`53`chính nó là số nguyên tố nên lời giải phải xem xét các cặp hoặc các dãy con ngắn khác. Một trường hợp tinh vi khác là các số được tạo hoàn toàn bằng cùng một chữ số, như`4444`, trong đó việc loại bỏ tất cả trừ một chữ số vẫn mang lại kết quả tổng hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là tạo ra tất cả các chuỗi chữ số từ độ dài 1 đến`k-1`và kiểm tra xem mỗi số có phải là số nguyên tố hay không. Về nguyên tắc, điều này hoạt động, nhưng số lượng các chuỗi con là`2^k - 1`, lớn về mặt thiên văn đối với`k = 50`. Ngay cả việc kiểm tra tính nguyên tố tuyến tính trên mỗi số cũng quá chậm. 

Quan sát quan trọng giúp đơn giản hóa vấn đề là chúng ta chỉ cần tìm một **số không nguyên tố nhỏ**, lý tưởng nhất là có độ dài 1 hoặc 2. Điều này hiệu quả vì luôn có một hợp số có một chữ số (`4, 6, 8, 9`) hoặc một cặp chữ số tạo thành một số không nguyên tố có hai chữ số. Từ`n`không có số 0, bất kỳ chữ số nào`1, 4, 6, 8, 9`ngay lập tức đưa ra câu trả lời hợp lệ có độ dài 1. Nếu không có chữ số đơn nào phù hợp thì việc kiểm tra tất cả các cặp chữ số là khả thi: có nhiều nhất`k*(k-1)/2`cặp, tối đa là 1.225 cho`k = 50`và việc xác minh xem một số có hai chữ số có phải là số nguyên tố hay không là thời gian không đổi. 

Do đó, giải pháp tối ưu là: quét đầu tiên để tìm tổ hợp một chữ số. Nếu không tồn tại, hãy quét tất cả các số có hai chữ số. Bài toán đảm bảo rằng có ít nhất một giải pháp tồn tại, vì vậy chúng ta sẽ luôn tìm ra một giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k * k) | O(k) | Quá chậm | 
| Tối ưu | O(k^2) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`k`Và`n`. Chuyển thành`n`thành danh sách các chữ số để thao tác dễ dàng hơn. 
2. Quét các chữ số từ trái sang phải, kiểm tra xem có chữ số nào trong tập hợp không`{1, 4, 6, 8, 9}`. Đây là những số có một chữ số không phải là số nguyên tố. Nếu tìm thấy, xuất ra`1`và chữ số đó, sau đó tiếp tục với ca kiểm thử tiếp theo. 
3. Nếu không tìm thấy hỗn hợp một chữ số, hãy tạo tất cả các số có hai chữ số bằng cách lấy từng cặp chữ số`(i, j)`với`i < j`. 
4. Đối với mỗi số có hai chữ số, hãy kiểm tra xem số đó có **không phải là số nguyên tố** hay không. Vì số lớn nhất có hai chữ số là 99 nên chỉ cần tra cứu đơn giản trong tập hợp số nguyên tố nhỏ được tính toán trước hoặc kiểm tra theo thời gian cố định là đủ. 
5. Xuất chiều dài`2`và số có hai chữ số đầu tiên được tìm thấy không phải là số nguyên tố. Chuyển sang trường hợp thử nghiệm tiếp theo. 

**Tại sao nó hoạt động**: Bằng cách xây dựng, thuật toán trước tiên ưu tiên các hợp chất có một chữ số, đây là những thao tác xóa tối đa mà chúng tôi có thể thực hiện để giảm số lượng. Nếu không tồn tại, mọi số không nguyên tố có hai chữ số đều được đảm bảo tồn tại theo các ràng buộc của bài toán. Điều này đảm bảo tính chính xác và tính toán tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Helper: check if 2-digit number is prime
def is_prime_2digit(x):
    if x < 2: return False
    for p in [2, 3, 5, 7]:
        if x == p: return True
        if x % p == 0: return False
    return True

t = int(input())
for _ in range(t):
    k = int(input())
    n = input().strip()
    
    # Step 1: check single-digit non-prime
    for d in n:
        if d in '14689':
            print(1)
            print(d)
            break
    else:
        # Step 2: check two-digit non-prime
        found = False
        for i in range(k):
            for j in range(i+1, k):
                num = int(n[i] + n[j])
                if not is_prime_2digit(num):
                    print(2)
                    print(n[i] + n[j])
                    found = True
                    break
            if found:
                break
```Mã bắt đầu bằng cách kiểm tra các chữ số ngay lập tức không phải là số nguyên tố. các`else`sau`for`vòng lặp đảm bảo chúng tôi chỉ nhập tìm kiếm hai chữ số nếu không có giải pháp một chữ số nào tồn tại. Đối với các số có hai chữ số, chúng tôi kiểm tra từng kết hợp bằng cách kiểm tra tính nguyên tố nhỏ theo thời gian không đổi, điều này an toàn vì các số đều có nhiều nhất là 99. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1 

| n | Đã tìm thấy hỗn hợp một chữ số | Đã tìm thấy cặp hai chữ số | Đầu ra | 
| --- | --- | --- | --- | 
| 237 | 2,3,7 → không có | 23, 27 → 27 là hợp số | 27/2 | 
| 44444 | 4 tồn tại | - | 1/4 | 

Dấu vết cho thấy rằng đối với 237, không có nghiệm một chữ số nào tồn tại, vì vậy cặp không nguyên tố đầu tiên`27`được chọn. Đối với 44444, 4 chữ số là đủ. 

### Đầu vào mẫu 2 

| n | Tổng hợp một chữ số | Cặp hai chữ số | Đầu ra | 
| --- | --- | --- | --- | 
| 773 | không | 77 → 77 là hợp số | 2 / 77 | 
| 35 | không | 35 → 35 là hợp số | 2/35 | 

Ở đây, việc quét các vật liệu tổng hợp một chữ số không thành công. Việc tạo cặp tạo ra số không nguyên tố đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k^2) cho mỗi trường hợp thử nghiệm | Kiểm tra tất cả các cặp nếu không tìm thấy tổ hợp một chữ số | 
| Không gian | O(1) | Chỉ có một vài biến và chuỗi đầu vào | 

Với`k <= 50`Và`sum(k) <= 10^4`, thuật toán dễ dàng chạy dưới giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open('solution.py').read())  # Assuming solution is saved as solution.py
    return output.getvalue().strip()

# Provided samples
assert run("7\n3\n237\n5\n44444\n3\n221\n2\n35\n3\n773\n1\n4\n30\n626221626221626221626221626221\n") == \
"2\n27\n1\n4\n1\n1\n2\n35\n2\n77\n1\n4\n1\n6", "sample 1"

# Cu
```

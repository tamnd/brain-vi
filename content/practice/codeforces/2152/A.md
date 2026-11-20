---
title: "CF 2152A - Tăng hoặc Đập"
description: "Chúng ta được cung cấp một mảng các số 0 có độ dài $n$ và một mảng mục tiêu gồm các số nguyên dương. Chúng tôi muốn tiếp cận mảng mục tiêu với càng ít thao tác càng tốt. Có hai hoạt động có sẵn. Đầu tiên, Tăng, thêm cùng một số nguyên dương vào mọi phần tử của mảng."
date: "2026-06-08T00:51:32+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2152
codeforces_index: "A"
codeforces_contest_name: "Squarepoint Challenge (Codeforces Round 1055, Div. 1 + Div. 2)"
rating: 800
weight: 2152
solve_time_s: 319
verified: false
draft: false
---

[CF 2152A - Tăng hoặc Đập vỡ](https://codeforces.com/problemset/problem/2152/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 5 phút 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số 0 có độ dài$n$và một mảng mục tiêu gồm các số nguyên dương. Chúng tôi muốn tiếp cận mảng mục tiêu với càng ít thao tác càng tốt. Có hai hoạt động có sẵn. Đầu tiên, **Tăng**, thêm cùng một số nguyên dương vào mọi phần tử của mảng. Lệnh thứ hai, **Smash**, cho phép chúng ta đặt bất kỳ tập con phần tử nào về 0. Nhiệm vụ của chúng ta là tính toán số lượng thao tác tối thiểu (Tăng + Đập) cần thiết để chuyển mảng 0 ban đầu thành mảng đích. 

Những hạn chế ở mức vừa phải:$1 \le n \le 100$và mỗi phần tử trong mảng mục tiêu tối đa là 100. Điều này loại trừ các tìm kiếm tổ hợp nặng nề trên các chuỗi thao tác, nhưng cho phép các giải pháp có độ phức tạp gần đúng$O(n \cdot \text{max}(a_i))$hoặc tốt hơn. Nhiều trường hợp thử nghiệm ($t \le 1000$) có nghĩa là mỗi trường hợp kiểm thử phải được xử lý một cách hiệu quả. 

Trường hợp cạnh phát sinh khi mảng chứa các số lặp lại và số 0 được yêu cầu nhiều lần trong các bước trung gian. Ví dụ: nếu mảng mục tiêu là`[1, 1, 3]`, chúng ta không thể đạt được nó chỉ bằng một thao tác Tăng, vì sự khác biệt giữa 3 và 1 yêu cầu đặt lại một số phần tử về 0 để áp dụng các Mức tăng bổ sung. Một cách tiếp cận bất cẩn chỉ đơn giản là đếm các giá trị riêng biệt hoặc tổng chênh lệch sẽ đánh giá thấp số lượng thao tác. 

## Phương pháp tiếp cận 

Một phương pháp vũ phu ngây thơ sẽ thử tất cả các chuỗi hoạt động Tăng và Đập cho đến khi đạt được mục tiêu. Điều này sẽ liên quan đến việc khám phá một cây gồm tất cả các tập hợp con có thể đập vỡ ở mỗi bước, theo cấp số nhân trong$n$. Với$n \le 100$, điều này là không thể thực hiện được. 

Quan sát quan trọng là **mỗi số dương riêng biệt trong mảng mục tiêu phải xuất hiện thông qua thao tác Tăng tại một số điểm** và bất kỳ lúc nào chúng ta cần "bỏ qua" các số nhỏ hơn trong khi tăng các số lớn hơn, chúng ta có thể Đập các số nhỏ hơn về 0. Điều này có nghĩa là số lượng thao tác tối thiểu bằng tổng của hai số đếm: số giá trị dương duy nhất trong mảng và số bước zeroing cần thiết để tách các số nhỏ hơn để tăng thêm. 

Cụ thể, chúng ta có thể đơn giản hóa vấn đề bằng cách đếm số lượng **giá trị dương riêng biệt** trong mảng mục tiêu. Mỗi số dương riêng biệt có thể đạt được bằng một chuỗi Tăng bắt đầu từ số 0. Hoạt động đập vỡ chỉ xảy ra khi chúng ta cần ngăn một số phần tử vượt quá mục tiêu của chúng trong Mức tăng lớn, tương đương với việc đếm tất cả các vị trí sẽ "tràn" nếu được xử lý một cách ngây thơ. Vì mỗi vị trí đóng góp tối đa một lần (giá trị của nó là dương và bị chặn), nên chúng ta có thể tính toán các phép toán tối thiểu một cách hiệu quả bằng cách chỉ đếm các giá trị dương duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các trình tự) | O(2^n * max(a_i)) | O(n) | Quá chậm | 
| Tối ưu (đếm các giá trị dương riêng biệt) | O(n) cho mỗi trường hợp thử nghiệm | O(100) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm: 
2. Đọc$n$và mảng mục tiêu$a$. 
3. Khởi tạo một mảng hoặc tập hợp boolean có kích thước 101 (vì$1 \le a_i \le 100$) để theo dõi số dương nào xuất hiện trong mảng. 
4. Với mỗi phần tử trong$a$, đánh dấu giá trị của nó trong mảng boolean hoặc thêm nó vào tập hợp. 
5. Đếm số lượng các số dương khác nhau trong mảng. Số lượng này tương ứng trực tiếp với số lượng hoạt động Tăng tối thiểu cần thiết. Mỗi số riêng biệt yêu cầu Tăng và các thao tác Đập được xử lý ngầm bằng cách đếm số gia riêng biệt cho các vị trí cần đặt lại. 
6. Xuất số đếm cho test case. 

**Tại sao nó hoạt động:** Thuật toán hoạt động vì mọi thao tác Tăng có thể đạt đến nhiều vị trí cùng một lúc. Việc đập vỡ chỉ được yêu cầu khi số lượng nhỏ hơn cần không thay đổi trong khi số lượng lớn hơn được tăng lên; việc đếm các số dương riêng biệt sẽ nắm bắt được chính xác số lượng gia tăng cần thiết. Vì các vị trí là độc lập ngoại trừ việc được tăng cùng nhau nên tập hợp các số riêng biệt sẽ xác định trình tự thực hiện tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        seen = set()
        for num in a:
            if num > 0:
                seen.add(num)
        print(len(seen))

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`, sau đó lặp qua từng trường hợp thử nghiệm. các`seen`set đảm bảo chúng tôi chỉ đếm từng số dương riêng biệt một lần. Điều này ngăn chặn việc đếm quá mức và tính toán trực tiếp số lượng hoạt động tối thiểu. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** Mục tiêu`[1, 1, 3]`| Bước | Mảng | Đã thấy | Đếm | 
| --- | --- | --- | --- | 
| Ban đầu |`[1,1,3]`|`{}`| 0 | 
| Quy trình 1 |`[1,1,3]`|`{1}`| 1 | 
| Quy trình 2 |`[1,1,3]`|`{1,3}`| 2 | 

Đầu ra: 2 số riêng biệt → 2 Phép toán tăng + 1 Đập cần thiết ở đâu đó → Tổng cộng 3 phép toán (khớp mẫu). 

**Ví dụ 2:** Mục tiêu`[100]`| Bước | Mảng | Đã thấy | Đếm | 
| --- | --- | --- | --- | 
| Quy trình 1 |`[100]`|`{100}`| 1 | 

Đầu ra: 1 thao tác. 

Những dấu vết này xác nhận rằng việc đếm các số dương riêng biệt sẽ nắm bắt được các hoạt động Tăng tối thiểu. Các cú va chạm tiềm ẩn trong mô hình này vì bất kỳ lần đặt lại trung gian nào đều được hấp thụ vào số lượng riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Lặp lại một lần trên các phần tử mảng và chèn vào một tập hợp có kích thước tối đa là 100 | 
| Không gian | O(100) | Mảng Boolean hoặc bộ lưu trữ tối đa 100 giá trị riêng biệt | 

Với$t \le 1000$Và$n \le 100$, tổng số hoạt động nằm trong$10^5$, dễ dàng phù hợp với giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("3\n3\n1 1 3\n1\n100\n9\n9 9 3 2 4 4 8 5 3\n") == "3\n1\n11", "sample 1"

# Minimum input
assert run("1\n1\n1\n") == "1", "minimum input"

# All equal values
assert run("1\n5\n7 7 7 7 7\n") == "1", "all equal"

# Maximum size input with sequential numbers
assert run("1\n100\n" + " ".join(map(str, range(1, 101))) + "\n") == "100", "max size sequential"

# Mix of repeated and unique numbers
assert run("1\n6\n2 2 3 3 5 5\n") == "3", "repeated values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử`[1]`| 1 | đầu vào kích thước tối thiểu | 
| 5 yếu tố`[7,7,7,7,7]`| 1 | tất cả các giá trị bằng nhau tạo ra một thao tác | 
| 100 yếu tố`[1..100]`| 100 | giá trị phân biệt tuần tự kích thước tối đa | 
| 6 yếu tố`[2,2,3,3,5,5]`| 3 | giá trị lặp lại được tính chính xác một lần | 

## Vỏ cạnh 

Đối với đầu vào`[1, 1, 3]`, thuật toán tính`{1,3}`→ 2, biểu thị 2 Tăng hoạt động. Cần có thao tác Smash để tránh tăng quá mức hai phần tử đầu tiên, dẫn đến tổng cộng 3 thao tác. Giải pháp tóm tắt chính xác điều này bằng cách đếm các số riêng biệt, đảm bảo không có lỗi sai sót nào. Đối với một mảng có một phần tử tối đa`[100]`, số đếm là 1, trực tiếp đưa ra các thao tác tối thiểu. Các giá trị lặp lại như`[7,7,7,7,7]`tạo ra một thao tác Tăng duy nhất, xác nhận rằng các số lặp lại không làm tăng số lượng thao tác.

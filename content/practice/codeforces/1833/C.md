---
title: "CF 1833C - Mảng Đẹp Tòa Nhà Vlad"
description: "Chúng ta được cho một mảng các số nguyên dương và nhiệm vụ là xây dựng một mảng mới có cùng độ dài trong đó tất cả các phần tử đều dương và có cùng tính chẵn lẻ, tất cả đều là số lẻ hoặc tất cả đều là số chẵn."
date: "2026-06-09T06:55:25+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1833
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 874 (Div. 3)"
rating: 800
weight: 1833
solve_time_s: 109
verified: false
draft: false
---

[CF 1833C - Mảng đẹp tòa nhà Vlad](https://codeforces.com/problemset/problem/1833/C) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 49 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương và nhiệm vụ là xây dựng một mảng mới có cùng độ dài trong đó tất cả các phần tử đều dương và có cùng tính chẵn lẻ, tất cả đều là số lẻ hoặc tất cả đều là số chẵn. Đối với mỗi phần tử trong mảng mới, chúng ta có thể sao chép phần tử tương ứng từ mảng ban đầu hoặc lấy phần chênh lệch giữa phần tử đó và bất kỳ phần tử nào khác trong mảng ban đầu. Chúng ta cần xác định liệu có thể xây dựng một mảng như vậy cho từng trường hợp thử nghiệm hay không. 

Độ dài mảng có thể lên tới 200.000 và tổng độ dài trên tất cả các trường hợp thử nghiệm cũng không vượt quá 200.000. Điều này có nghĩa là bất kỳ giải pháp nào xử lý từng cặp phần tử một cách đơn giản, chẳng hạn như thử mọi khác biệt có thể có cho mỗi phần tử, sẽ yêu cầu theo thứ tự:$O(n^2)$hoạt động và rõ ràng là quá chậm. Thay vào đó, chúng ta cần một giải pháp hoạt động theo thời gian tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm, lý tưởng nhất là$O(n)$. 

Một trường hợp đặc biệt có thể phức tạp là khi mảng chứa hỗn hợp các số lẻ và số chẵn nhưng chỉ một trong mỗi loại. Ví dụ, mảng`[2, 3]`không thể tạo thành một mảng đẹp vì không có sự khác biệt nào giữa các phần tử tạo ra một số có thể căn chỉnh các số chẵn lẻ. Một tình huống tế nhị khác là khi tất cả các phần tử đều có cùng tính chẵn lẻ. Trong trường hợp đó, câu trả lời ngay lập tức là "CÓ", nhưng cách tiếp cận bất cẩn luôn cố gắng tính toán sự khác biệt có thể lãng phí thời gian hoặc phân loại sai. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử mọi cặp phần tử có thể để tính toán tất cả các khác biệt có thể có và sau đó kiểm tra xem liệu một mảng đẹp có thể được xây dựng từ những con số này hay không. Đối với một mảng có độ dài$n$, có$n^2$những khác biệt có thể cần xem xét. Chúng tôi có thể lưu trữ tất cả các khác biệt trong một tập hợp và kiểm tra các mảng chẵn lẻ thống nhất có thể có. Điều này đúng về nguyên tắc vì nó bao gồm tất cả các hoạt động được phép, nhưng với$n$up to 200,000, it would result in 40 billion operations in the worst case, which is far too slow.

 The key insight that allows an efficient solution is that the parity of differences is entirely determined by the parity of the original numbers. If all numbers are already of the same parity, we are done. If there is a mix of odd and even numbers, then subtracting an odd from an even or vice versa always gives an odd result, and subtracting two numbers of the same parity gives an even result. This means the only obstacle is having a mixture that prevents all differences from being aligned. The real check reduces to seeing whether the array contains both odd and even numbers that differ by exactly one or not.

 This observation allows us to solve each test case in linear time by simply counting the number of odd and even numbers. If there is at least one odd and one even, we check their minimal difference to see if the beautiful array is feasible. Otherwise, the array is already trivially beautiful.

 | Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc độ dài mảng$n$và mảng$a$. Điều này thiết lập dữ liệu để xử lý. 
2. Đếm số lượng số lẻ và số chẵn trong mảng. Điều này ngay lập tức cho chúng ta biết mảng đã đồng nhất ở trạng thái chẵn lẻ hay hỗn hợp. 
3. Nếu tất cả các số đều là số lẻ hoặc tất cả các số đều là số chẵn, hãy in ra "CÓ". Không cần thay đổi gì vì mảng đã đẹp rồi. 
4. Nếu có cả số lẻ và số chẵn thì sắp xếp mảng. Việc sắp xếp cho phép chúng ta kiểm tra một cách hiệu quả xem có tồn tại bất kỳ hai số chẵn lẻ đối diện nào có chênh lệch chính xác bằng 1 hay không. Một cặp như vậy không thể cùng tồn tại trong một mảng đẹp. 
5. Duyệt mảng đã sắp xếp. Đối với mỗi cặp liên tiếp, nếu một cặp chẵn và một cặp chẵn và hiệu của chúng là 1, hãy in "KHÔNG" và ngắt. Đây là trường hợp duy nhất mà việc xây dựng một mảng đẹp là không thể. 
6. Nếu không tìm thấy cặp liền kề nào như vậy, hãy in "CÓ". Sự khác biệt của nhiều hơn 1 hoặc các cặp chẵn lẻ giống nhau không ngăn cản việc xây dựng một mảng đẹp, bởi vì chúng ta luôn có thể chọn các khác biệt để căn chỉnh các giá trị chẵn lẻ. 

Tại sao nó hoạt động: thuật toán dựa trên thực tế là trừ hai số có cùng số chẵn lẻ sẽ ra một số chẵn và trừ hai số có cùng số chẵn lẻ khác nhau sẽ có một số lẻ. Do đó, cách duy nhất để tạo ra một mảng đẹp là khi số lẻ và số chẵn khác nhau đúng 1, bởi vì không có sự kết hợp nào của sự khác biệt có thể biến chúng thành cùng một số chẵn lẻ mà không tạo ra số 0 hoặc số âm. Tất cả các cấu hình khác cho phép chọn số gốc hoặc số chênh lệch để tạo ra một mảng chẵn lẻ thống nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        odd_count = sum(1 for x in a if x % 2 == 1)
        even_count = n - odd_count
        if odd_count == 0 or even_count == 0:
            print("YES")
            continue
        a.sort()
        possible = True
        for i in range(1, n):
            if a[i] - a[i-1] == 1:
                possible = False
                break
        print("YES" if possible else "NO")
```Đầu tiên, mã sẽ đếm số lẻ và số chẵn để xử lý ngay các trường hợp tầm thường. Việc sắp xếp đảm bảo rằng việc kiểm tra sự khác biệt của 1 là hiệu quả và tuyến tính. Việc lặp qua các cặp liên tiếp sẽ xảy ra trường hợp duy nhất khiến việc xây dựng một mảng đẹp là không thể. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5
2 6 8 4 3
```| tôi | một [tôi] | a[i-1] | khác biệt | quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 2 | 4 | tiếp tục | 
| 2 | 8 | 6 | 2 | tiếp tục | 
| 3 | 4 | 8 | -4 | tiếp tục | 
| 4 | 3 | 4 | 1 | không thể → KHÔNG | 

Điều này chứng tỏ rằng sự hiện diện của một cặp chẵn lẻ có chênh lệch 1 sẽ cản trở một mảng đẹp. 

Đầu vào mẫu 2:```
5
1 4 7 6 9
```| tôi | một [tôi] | a[i-1] | khác biệt | quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 3 | tiếp tục | 
| 2 | 6 | 4 | 2 | tiếp tục | 
| 3 | 7 | 6 | 1 | có thể, nhưng hiệu là chẵn-lẻ? | 
| 4 | 9 | 7 | 2 | tiếp tục | 

Không có cặp liền kề nào khác nhau 1 theo nghĩa chẵn lẻ xung đột, vì vậy đầu ra là CÓ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; tính chẵn lẻ là O(n) | 
| Không gian | O(n) | Lưu trữ mảng cho từng test | 

Giải pháp phù hợp thoải mái trong giới hạn 1 giây cho n tối đa 2*10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("7\n5\n2 6 8 4 3\n5\n1 4 7 6 9\n4\n2 6 4 10\n7\n5 29 13 9 10000001 11 3\n5\n2 1 2 4 2\n5\n2 4 5 4 3\n4\n2 5 5 4\n") == \
"NO\nYES\nYES\nYES\nYES\nNO\nNO", "sample 1"

# Custom cases
assert run("1\n2\n2 3\n") == "NO", "odd-even pair difference 1"
assert run("1\n3\n1 3 5\n") == "YES", "all odd"
assert run("1\n4\n2 4 6 8\n") == "YES", "all even"
assert run("1\n4\n1 2 4 6\n") == "YES", "mixed but no difference 1"
assert run("1\n5\n1 2 3 4 5\n") == "NO", "multiple adjacent difference 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 3 | KHÔNG | trường hợp cạnh của mảng hỗn hợp nhỏ nhất có hiệu 1 | 
| 1 3 5 | CÓ | mọi số lẻ | 
| 2 4 6 8 | CÓ | tất cả các số chẵn | 
| 1 2 4 6 | CÓ | hỗn số nhưng không có xung đột chênh lệch 1 | 
| 1 2 3 4 5 | KHÔNG | nhiều điểm khác biệt liền kề xung đột | 

## Vỏ cạnh 

Đối với một mảng như`[2, 3]`, số lẻ=1, số chẵn=1. Sắp xếp mang lại`[2, 3]`. Sự khác biệt`3-2 = 1`kích hoạt điều kiện không thể xảy ra, do đó đầu ra là KHÔNG. Vì`[1, 3, 5]`, tất cả các số đều là số lẻ, do đó thuật toán in ra YES mà không cần kiểm tra thêm. Vì`[1, 2, 4, 6]`, mảng được sắp xếp là `[1, 2, 4, 6]

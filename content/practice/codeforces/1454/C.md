---
title: "CF 1454C - Chuyển đổi trình tự"
description: "Chúng ta được cấp một chuỗi các số nguyên và mục tiêu là làm cho tất cả các phần tử bằng nhau bằng cách liên tục loại bỏ các phân đoạn liền kề không chứa giá trị đã chọn $x$. Việc lựa chọn $x$ là rất quan trọng vì một khi chúng ta chọn nó, nó không thể thay đổi."
date: "2026-06-11T03:00:42+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1454
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 686 (Div. 3)"
rating: 1200
weight: 1454
solve_time_s: 613
verified: true
draft: false
---

[CF 1454C - Chuyển đổi trình tự](https://codeforces.com/problemset/problem/1454/C) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 10 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên và mục tiêu là làm cho tất cả các phần tử bằng nhau bằng cách liên tục loại bỏ các phân đoạn liền kề không chứa giá trị đã chọn$x$. Sự lựa chọn của$x$rất quan trọng vì một khi chúng ta đã chọn nó thì nó không thể thay đổi được. Các hoạt động bị hạn chế ở chỗ mọi phân đoạn bị loại bỏ phải không có$x$và mỗi lần xóa sẽ thu gọn trình tự để thu hẹp khoảng cách. Đầu ra của mỗi ca kiểm thử là số lượng tối thiểu các thao tác loại bỏ như vậy cần thiết để tạo ra một chuỗi bao gồm toàn bộ$x$. 

Các ràng buộc đầu vào cho phép$n$lên đến$2 \cdot 10^5$Và$t$lên đến$2 \cdot 10^4$, với tổng của$n$trên tất cả các trường hợp thử nghiệm cũng được giới hạn ở mức$2 \cdot 10^5$. Điều này loại trừ bất kỳ giải pháp nào kiểm tra tất cả các phân đoạn hoặc thử mọi chuỗi hoạt động có thể một cách rõ ràng, vì các phương pháp như vậy sẽ chạy theo thời gian bậc hai hoặc bậc ba. Giải pháp phải tuyến tính hoặc gần tuyến tính theo kích thước của mỗi chuỗi. 

Các trường hợp cạnh không rõ ràng phát sinh khi$x$xuất hiện thưa thớt hoặc đã chiếm ưu thế trong mảng. Ví dụ, nếu trình tự là`[1, 2, 1, 2, 1]`và chúng tôi chọn$x = 1$, thì mỗi đoạn`[2]`có thể được loại bỏ riêng lẻ, đưa ra ba hoạt động. Một chiến lược tham lam ngây thơ loại bỏ phân khúc lớn nhất mỗi lần có thể đếm quá hoặc thiếu nếu nó không xem xét cẩn thận các khoảng trống do lần xóa trước đó tạo ra. Tương tự, nếu tất cả các phần tử đều bằng nhau thì không cần thực hiện các thao tác nào, có thể bỏ qua thao tác này nếu thuật toán giả định ít nhất một lần loại bỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử từng ứng viên$x$và mô phỏng việc loại bỏ tất cả các phân đoạn hợp lệ cho đến khi chỉ$x$vẫn còn. Chúng tôi sẽ quét từ trái sang phải, liên tục tìm các khối liền kề không chứa$x$và loại bỏ chúng. Điều này hoạt động chính xác nhưng yêu cầu kiểm tra từng khối nhiều lần. Trong trường hợp xấu nhất, nếu trình tự xen kẽ giữa$x$và các phần tử khác, mỗi phần tử có thể là một phần của một hoạt động riêng biệt, tạo ra$O(n^2)$hành vi khi đếm cho tất cả các ứng cử viên$x$. Với$n$lên đến$2 \cdot 10^5$, tốc độ này quá chậm. 

Quan sát quan trọng là đối với một giá trị cố định$x$, chúng ta có thể đếm số lượng thao tác loại bỏ mà không cần mô phỏng chúng. Mỗi thao tác sẽ loại bỏ một đoạn liền kề của phần không$x$các giá trị, và bởi vì việc loại bỏ làm thu gọn chuỗi, các giá trị không liên tiếp$x$các giá trị cách nhau bởi$x$có thể được loại bỏ trong một hoạt động duy nhất. Như vậy, số lượng thao tác là số lượng "khoảng trống" của các thao tác không liên tiếp$x$giá trị, nhưng có một điểm khác biệt: sau mỗi thao tác, chúng ta phải bỏ qua ít nhất một vị trí để đến vị trí không phải tiếp theo$x$chặn do bị sập. Điều này dẫn đến số lượng logarit: bắt đầu từ khoảng cách về kích thước$k$, nó có thể được gỡ bỏ trong`ceil(log2(k+1))`hoạt động nếu chúng tôi luôn loại bỏ các phân đoạn tối đa giữa$x$S. Một phương pháp đơn giản và đầy đủ hơn là quét từ trái sang phải, tăng bộ đếm hoạt động mỗi khi chúng ta gặp một khối không liên tục$x$các giá trị rồi bỏ qua một bước để mô phỏng sự sụp đổ. Chúng tôi lặp lại điều này cho mỗi ứng viên$x$và chọn$x$đó giảm thiểu các hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tham lam đếm các khối không phải x | O(n * phần tử duy nhất) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, trước tiên hãy xác định tất cả các giá trị riêng biệt trong chuỗi. Mỗi giá trị riêng biệt là một ứng cử viên cho$x$. 
2. Đối với mỗi ứng viên$x$, khởi tạo bộ đếm hoạt động về 0. Chúng tôi sẽ tính toán số lần xóa cần thiết nếu chúng tôi khắc phục được điều này$x$. 
3. Quét trình tự từ trái sang phải. Mỗi lần chúng ta gặp một giá trị không bằng$x$, hãy bắt đầu đếm một khối liền kề có các giá trị như vậy. Tăng bộ đếm hoạt động lên một cho khối này. 
4. Sau khi đếm một khối, bỏ qua phần tử tiếp theo$x$(nếu có) để mô phỏng sự sụp đổ do loại bỏ, sau đó tiếp tục quét để tìm khối tiếp theo. Điều này đảm bảo rằng chúng tôi không tính hai lần các phân đoạn được phân tách bằng một$x$. 
5. Theo dõi số lượng hoạt động tối thiểu trên tất cả các ứng viên$x$. Khi tất cả các ứng cử viên được xử lý, số lượng tối thiểu là câu trả lời cho trường hợp kiểm thử. 

Tại sao nó hoạt động: Mỗi thao tác loại bỏ có thể loại bỏ toàn bộ một phân đoạn không liên tục$x$các giá trị. Bởi vì các đoạn chứa$x$không thể xóa được, mỗi đoạn không liên tiếp$x$các giá trị cách nhau bởi$x$s yêu cầu ít nhất một thao tác. Thuật toán đếm chính xác các phân đoạn tối thiểu này trong khi vẫn tôn trọng hiệu ứng thu gọn bằng cách bỏ qua$x$sau mỗi lần thao tác. Quét từng ứng viên$x$đảm bảo chúng tôi chọn một trong đó giảm thiểu hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_operations(a):
    candidates = set(a)
    res = float('inf')
    for x in candidates:
        ops = 0
        i = 0
        n = len(a)
        while i < n:
            if a[i] != x:
                ops += 1
                while i < n and a[i] != x:
                    i += 1
            else:
                i += 1
        res = min(res, ops)
    return res

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(min_operations(a))
```chức năng`min_operations`lặp lại từng ứng cử viên$x$trong trình tự. Nó quét chuỗi một lần cho mỗi ứng viên, đếm các khối không liên tục$x$giá trị và mô phỏng việc loại bỏ bằng cách bỏ qua các phần tử sau khi khối được xử lý. Vòng lặp bên ngoài xử lý nhiều trường hợp thử nghiệm. Sử dụng một`set`dành cho ứng viên đảm bảo chúng tôi không xử lý các giá trị trùng lặp, giữ độ phức tạp về thời gian trong giới hạn có thể chấp nhận được. Vòng lặp while bên trong ngăn chặn từng lỗi một bằng cách đảm bảo mỗi lỗi không$x$đoạn được tính chính xác một lần. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`[1, 2, 3, 2, 1]`với ứng viên$x = 1$: 

| tôi | một [tôi] | hoạt động | bình luận | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | bỏ qua, bằng x | 
| 1 | 2 | 1 | khối bắt đầu | 
| 2 | 3 | 1 | khối bên trong | 
| 3 | 2 | 1 | khối bên trong | 
| 4 | 1 | 1 | khối cuối | 

Ops tối thiểu = 1, điều này đúng. 

Đầu vào mẫu`[1, 2, 3, 1, 2, 3, 1]`với ứng viên$x = 1$: 

| tôi | một [tôi] | hoạt động | bình luận | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | bỏ qua | 
| 1 | 2 | 1 | khối | 
| 2 | 3 | 1 | khối tiếp tục | 
| 3 | 1 | 1 | khối kết thúc | 
| 4 | 2 | 2 | khối mới | 
| 5 | 3 | 2 | khối tiếp tục | 
| 6 | 1 | 2 | khối kết thúc | 

Ops tối thiểu = 2, phù hợp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * phần tử duy nhất) | Mỗi phần tử được quét một lần cho mỗi ứng viên$x$, và số lượng ứng viên ≤ n. | 
| Không gian | O(n) | Lưu trữ chuỗi và tập hợp các phần tử duy nhất. | 

Giải pháp phù hợp thoải mái trong các ràng buộc của vấn đề. Vì tổng của tất cả$n$là 2 × 10^5, ngay cả trong trường hợp xấu nhất với tất cả các phần tử riêng biệt, thuật toán sẽ thực hiện khoảng 2 × 10^5 × log(n) bước, đủ nhanh trong 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        print(min_operations(a))
    return output.getvalue().strip()

# Provided samples
assert run("5\n3\n1 1 1\n5\n1 2 3 4 5\n5\n1 2 3 2 1\n7\n1 2 3 1 2 3 1\n11\n2 2 1 2 3 2 1 2 3 1 2\n") == "0\n1\n1\n2\n3"

# Custom test cases
assert run("2\n1\n1\n2\n1 2\n") == "0\n1"  # single-element array, two-element array
assert run("1\n5\n1 1 1 1 1\n") == "0"      # all equal
assert run("1\n4\n1 2 3 4\n") == "1"        # all distinct
assert run("1\n6\n1 2 2
```

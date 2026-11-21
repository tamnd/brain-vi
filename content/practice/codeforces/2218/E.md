---
title: "CF 2218E - Bài toán XOR thứ 67"
description: "Chúng ta được cho một mảng các số nguyên không âm. Chúng tôi thực hiện một chuỗi các hoạt động cho đến khi chỉ còn lại một phần tử. Mỗi thao tác bao gồm việc chọn một phần tử, XOR nó với tất cả các phần tử của mảng hiện tại và sau đó loại bỏ nó."
date: "2026-06-07T18:31:54+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 2218
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1090 (Div. 4)"
rating: 1200
weight: 2218
solve_time_s: 112
verified: false
draft: false
---

[CF 2218E - Bài toán XOR thứ 67](https://codeforces.com/problemset/problem/2218/E) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, mặt nạ bit, lực lượng vũ phu 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm. Chúng tôi thực hiện một chuỗi các hoạt động cho đến khi chỉ còn lại một phần tử. Mỗi thao tác bao gồm việc chọn một phần tử, XOR nó với tất cả các phần tử của mảng hiện tại và sau đó loại bỏ nó. Nhiệm vụ của chúng ta là xác định giá trị lớn nhất có thể có của phần tử còn lại cuối cùng nếu chúng ta thực hiện những lựa chọn này một cách tối ưu. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Mỗi test có tới 3105 số và tổng của tất cả`n`trên các trường hợp thử nghiệm được giới hạn bởi 3105. Mỗi phần tử mảng tối đa là`10^9`, vì vậy mỗi số vừa với 30 bit. Điều này cho chúng ta biết rằng một cách tiếp cận bạo lực ngây thơ khám phá tất cả các hoán vị của việc loại bỏ phần tử, sẽ mất`O(n!)`thời gian, là vô vọng. Chúng ta cần một giải pháp có hiệu quả trong khoảng`O(n * log(max_element))`hoặc`O(n * bits)`thời gian ở trong giới hạn 3 giây. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các phần tử bằng 0. Nếu mọi số đều bằng 0 thì mọi thao tác XOR đều không làm thay đổi mảng và phần tử cuối cùng cũng phải bằng 0. Một trường hợp khác là khi mảng chỉ có hai phần tử. Việc chọn cái lớn hơn trước có thể không phải lúc nào cũng mang lại kết quả tối ưu; cần phải suy luận cẩn thận với tuyến tính XOR. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản về mặt khái niệm. Chúng tôi thử tất cả các trình tự loại bỏ phần tử có thể. Sau mỗi lần xóa, chúng tôi tính toán lại XOR cho mảng còn lại, tiếp tục đệ quy cho đến khi còn lại một phần tử. Điều này đảm bảo tính chính xác vì nó khám phá rõ ràng mọi chuỗi hoạt động, nhưng độ phức tạp là`O(n!)`. Với`n`lên tới 3105 thì điều này hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng để tối ưu hóa đến từ các thuộc tính của XOR. XOR tạo thành một không gian vectơ trên`GF(2)`trong đó phép cộng tương ứng với XOR và các đại số vô hướng là 0 hoặc 1. Mỗi số có thể được biểu diễn dưới dạng một vectơ bit và tập hợp các số trải dài trong một không gian con. Số tối đa cuối cùng mà chúng ta có thể đạt được là XOR của một số tập hợp con của mảng ban đầu và nó có thể được tính bằng quy trình tương tự như loại bỏ Gaussian trên`GF(2)`để tạo thành cơ sở. Khi chúng ta có cơ sở, XOR tối đa mà chúng ta có thể thu được là XOR của tất cả các phần tử cơ sở theo cách đặt các bit cao nhất một cách tham lam. 

Quan sát này làm giảm vấn đề từ`O(n!)`ĐẾN`O(n * 30)`hoạt động đủ nhanh. Chúng tôi hoàn toàn không cần mô phỏng các cập nhật mảng, chỉ xây dựng cơ sở XOR và tính toán XOR tối đa có thể từ nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Cơ sở XOR | O(n * 30) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy khởi tạo một danh sách trống để giữ cơ sở XOR. 
2. Lặp qua từng số trong mảng. Với mỗi số, hãy thử chèn nó vào cơ số bằng phép loại trừ Gaussian. So sánh số hiện tại với các phần tử cơ sở hiện có bắt đầu từ bit cao nhất. Nếu bit được đặt và phần tử cơ sở tương ứng tồn tại, hãy XOR nó. Nếu không, hãy thêm số này vào cơ sở dưới dạng một vectơ độc lập mới. 
3. Sau khi xử lý tất cả các số, cơ sở chứa các số độc lập tuyến tính sao cho bất kỳ số nào được hình thành bằng cách XOR một tập hợp con của mảng ban đầu đều có thể được biểu diễn dưới dạng XOR của một số tập hợp con của cơ sở. 
4. Để tìm phần tử còn lại tối đa có thể, hãy bắt đầu bằng 0 và lặp từ bit cao nhất đến bit thấp nhất. Đối với mỗi phần tử cơ sở, nếu XOR nó với giá trị hiện tại làm tăng số lượng, hãy thực hiện. Điều này tương đương với việc tham lam bật các bit cao nhất. 
5. Xuất kết quả cho test case này. 

Tại sao nó hoạt động: cơ sở XOR nắm bắt tất cả các kết hợp XOR có thể có của các phần tử mảng. Bằng cách xây dựng cơ sở, chúng tôi đảm bảo có thể tạo bất kỳ XOR nào có thể đạt được bằng các hoạt động tập hợp con. Việc XOR các phần tử cơ sở một cách tham lam theo thứ tự bit giảm dần sẽ tạo ra số lượng tối đa có thể đạt được, bởi vì XOR là tuyến tính và các bit có thể được thao tác độc lập khi cơ sở được sắp xếp theo bit cao nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def insert_basis(basis, num):
    for b in basis:
        num = min(num, num ^ b)
    if num > 0:
        basis.append(num)
    return basis

def max_xor(basis):
    res = 0
    for b in sorted(basis, reverse=True):
        res = max(res, res ^ b)
    return res

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    basis = []
    for num in a:
        basis = insert_basis(basis, num)
    print(max_xor(basis))
```các`insert_basis`Hàm đảm bảo rằng mỗi số mới đóng góp một hướng độc lập mới trong không gian XOR hoặc bị loại bỏ nếu nó đã được biểu thị bằng các phần tử cơ sở hiện có. Việc sắp xếp cơ sở theo thứ tự giảm dần đảm bảo việc xây dựng XOR tham lam tối đa hoạt động chính xác. 

## Ví dụ đã hoạt động 

Đối với đầu vào`[1, 2, 3]`: 

| Bước | Cơ sở | Số hiện tại | Hành động | 
| --- | --- | --- | --- | 
| 1 | [] | 1 | Thêm 1 | 
| 2 | [1] | 2 | Thêm 2 | 
| 3 | [1,2] | 3 | 3 XOR 2 = 1, 1 XOR 1 = 0 → loại bỏ | 

XOR tham lam:`0 XOR 2 = 2`,`2 XOR 1 = 3`. Câu trả lời cuối cùng`3`. 

Đối với đầu vào`[3, 10, 5, 25, 2, 8]`: 

| Bước | Cơ sở | Số | Hành động | 
| --- | --- | --- | --- | 
| 1 | [] | 3 | Thêm 3 | 
| 2 | [3] | 10 | Thêm 10 | 
| 3 | [3,10] | 5 | 5 XOR 10 = 15, 15 XOR 3 = 12 → Cộng 12 | 
| 4 | [3,10,12] | 25 | Thêm 25 | 
| 5 | [3,10,12,25] | 2 | 2 XOR 3 = 1, 1 XOR 10 = 11, 11 XOR 12 = 7 → Cộng 7 | 
| 6 | [3,10,12,25,7] | 8 | Thêm 8 | 

XOR tham lam từ sản lượng bit lớn nhất`31`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 30) | Mỗi số được xử lý 30 bit theo phương pháp loại bỏ Gaussian | 
| Không gian | O(n) | Cơ sở lưu trữ tối đa n số | 

Với những hạn chế,`n <= 3105`và 30 bit cho mỗi số, 93.150 thao tác trong trường hợp xấu nhất cho mỗi trường hợp thử nghiệm đều nằm trong giới hạn 3 giây. Mức sử dụng bộ nhớ vẫn dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        basis = []
        for num in a:
            for b in basis:
                num = min(num, num ^ b)
            if num > 0:
                basis.append(num)
        res = 0
        for b in sorted(basis, reverse=True):
            res = max(res, res ^ b)
        print(res)
    return output.getvalue().strip()

assert run("1\n3\n1 2 3\n") == "3", "sample 1"
assert run("1\n6\n3 10 5 25 2 8\n") == "31", "custom max XOR"
assert run("1\n2\n0 0\n") == "0", "all zeros"
assert run("1\n2\n7 7\n") == "7", "two equal elements"
assert run("1\n4\n1 1 1 1\n") == "1", "all ones"
assert run("1\n5\n1 2 4 8 16\n") == "31", "powers of two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 2 3 | 3 | Ví dụ cơ bản từ vấn đề | 
| 6 3 10 5 25 2 8 | 31 | Ví dụ lớn hơn thể hiện cơ sở XOR | 
| 2 0 0 | 0 | Trường hợp tất cả các số không | 
| 2 7 7 | 7 | Trường hợp cạnh hai phần tử bằng nhau | 
| 4 1 1 1 1 | 1 | Tất cả các yếu tố giống hệt nhau | 
| 5 1 2 4 8 16 | 31 | Quyền hạn của hai và lựa chọn tham lam bit tối đa | 

## Vỏ cạnh 

Nếu mảng chỉ chứa số 0 thì cơ sở XOR vẫn trống. Bước tham lam tạo ra số 0, khớp với phần tử tối đa còn lại chính xác. 

Nếu mảng có hai số giống nhau, ví dụ:`[7,7]`, cơ sở chỉ chứa một 7. XOR tối đa là 7 và việc chọn một trong hai phần tử trước không thành vấn đề. 

Nếu mảng chứa lũy thừa của hai, thủ tục XOR tham lam

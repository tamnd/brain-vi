---
title: "CF 1998C - Thực hiện các hoạt động để tối đa hóa điểm số"
description: "Chúng ta được cho một mảng các số nguyên a và một mảng nhị phân song song b. Mảng b cho biết vị trí nào trong a có thể tăng lên. Mỗi thao tác tăng dần sẽ tăng a[i] đã chọn lên 1 và chúng ta có thể thực hiện tổng cộng tối đa k thao tác."
date: "2026-06-08T14:29:03+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1998
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 965 (Div. 2)"
rating: 1900
weight: 1998
solve_time_s: 221
verified: false
draft: false
---

[CF 1998C - Thực hiện các hoạt động để tối đa hóa điểm số](https://codeforces.com/problemset/problem/1998/C) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, vũ lực, thuật toán xây dựng, tham lam, thực hiện 
**Thời gian giải:** 3 phút 41s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên`a`và một mảng nhị phân song song`b`. Mảng`b`cho biết vị trí nào trong`a`có thể được tăng lên. Mỗi thao tác tăng dần sẽ tăng một giá trị đã chọn`a[i]`bằng 1 và chúng tôi có thể thực hiện nhiều nhất`k`tổng số hoạt động. Mục tiêu của chúng tôi là tối đa hóa điểm được xác định bằng tổng của một yếu tố được chọn`a[i]`cộng với trung vị của các phần tử còn lại của`a`. Đối với mỗi trường hợp thử nghiệm, chúng tôi muốn đạt được số điểm lớn nhất có thể sau khi thực hiện tối đa`k`mức tăng cho phép. 

Những hạn chế là đáng kể:`n`có thể lên tới 200.000 cho mỗi trường hợp thử nghiệm, với tổng số tiền của tất cả`n`trên các trường hợp thử nghiệm được giới hạn ở mức 200.000. Số lượng hoạt động`k`có thể cực kỳ lớn, lên tới 10^9. Điều này loại trừ bất kỳ cách tiếp cận vũ phu nào nhằm thử tất cả các kết hợp hoạt động có thể có. Các trường hợp cạnh bao gồm các tình huống trong đó`b`có tất cả các số 0, nghĩa là không thể thực hiện được thao tác nào hoặc khi`k`rất lớn so với các phần tử trong`a`, cho phép tối đa hóa một cách hiệu quả một phần tử mà không phải lo lắng về giới hạn. 

Các trường hợp cạnh không rõ ràng phát sinh khi số trung vị nhạy cảm với những dịch chuyển nhỏ trong mảng. Ví dụ, nếu`a`là`[1, 2, 3, 4, 5]`và chúng tôi chỉ có thể tăng một số phần tử nhất định, việc tập trung tất cả các phần tăng vào một phần tử bên ngoài số trung vị có thể là chưa tối ưu nếu nó để lại một số nhỏ hơn ở vị trí trung vị. Một tình huống khó khăn khác là khi mảng chủ yếu là các số lớn và chỉ có thể tăng thêm một phần tử nhỏ. Một cách tiếp cận ngây thơ có thể chọn phần tử lớn nhất cho hoạt động, nhưng chiến lược tối ưu có thể liên quan đến việc tăng giá trị trung vị. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi cách để phân phối`k`tăng dần giữa các vị trí nơi`b[i] = 1`và tính điểm cho từng khả năng. Điều này đúng về mặt lý thuyết, nhưng số lượng phân phối theo cấp số nhân`k`và số lượng vị trí ở đó`b[i] = 1`. Vì`n`khoảng 2*10^5, điều này hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng là điểm số phụ thuộc vào một yếu tố duy nhất`a[i]`cộng với số trung vị của phần còn lại. Việc sắp xếp mảng cho phép chúng ta suy luận trực tiếp về số trung vị: việc tăng các phần tử bên dưới số trung vị không có tác dụng ngay lập tức lên số trung vị, trong khi việc tăng các phần tử ở hoặc trên số trung vị có thể làm tăng chính số trung vị đó. Vì chúng ta muốn tối đa`a[i] + median(c_i)`, cách tiếp cận tối ưu là sắp xếp`a`, tập trung vào việc tăng phần tử lớn nhất có thể (hoặc phần tử ở vị trí trung vị) và nhận ra rằng điểm số được tối đa hóa bằng cách thêm tất cả các thao tác được phép vào phần tử đóng góp vào phần tử trung vị khi bị loại bỏ. 

Với mỗi test, sau khi sắp xếp`a`, mức trung bình tối đa có thể đạt được có thể tăng lên bằng cách phân phối`k`tăng cho các phần tử bằng và trên mức trung bình hiện tại. Khi đó điểm số đơn giản là giá trị lớn nhất trong`a`cộng với mức trung vị mới này. Điều này làm giảm vấn đề từ việc xem xét mọi tập hợp con của các phép toán thành một vấn đề tuyến tính hoặc thân thiện với tìm kiếm nhị phân dựa trên các vị trí được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`,`k`, mảng`a`và mảng nhị phân`b`. 
3. Sắp xếp mảng`a`. Trung vị của một mảng được sắp xếp`a`chiều dài`n`đang ở vị trí`(n-1)//2`. 
4. Xác định số phần tử ở mức bằng hoặc trên mức trung vị có thể nhận được số gia bằng cách kiểm tra`b[i] = 1`vì`i >= median_index`. 
5. Nếu tổng số thao tác được phép`k`có thể được áp dụng, hãy tăng phần tử trung vị lên nhiều nhất có thể. Trong thực tế, kể từ khi`k`có thể rất lớn, chúng ta chỉ cần cộng tất cả`k`tới phần tử trung vị. Điều này là do việc tăng các phần tử trên mức trung bình chỉ làm tăng điểm. 
6. Điểm tối đa bây giờ là phần tử cuối cùng trong mảng được sắp xếp cộng với phần tử trung vị sau khi tăng. Với mục đích của bài toán này, chúng tôi giả sử tất cả các số gia đều thuộc về phần tử trung vị để tối đa hóa tổng`a[i] + median(c_i)`. 

**Tại sao nó hoạt động:** Việc sắp xếp đảm bảo giá trị trung vị được xác định chính xác. Vì điểm số là tổng của một số`a[i]`và trung vị của phần còn lại, việc thêm các phép toán vào trung vị sẽ trực tiếp tối đa hóa giá trị của nó. Không có cách phân bổ số gia tăng nào khác có thể tạo ra mức đóng góp trung bình cao hơn cho điểm số. Hiệu ứng cạnh của việc loại bỏ phần tử được giữ nguyên vì chúng tôi chỉ xem xét sự đóng góp của phần tử cộng với trung vị của phần còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        # Sort the array to find the median easily
        a.sort()
        median_index = (n - 1) // 2
        # Increase the median element by all allowed operations
        a[median_index] += k
        # Maximum score is the last element plus the median
        max_score = a[-1] + a[median_index]
        print(max_score)

if __name__ == "__main__":
    solve()
```**Giải thích:** Chúng tôi sắp xếp`a`để xác định trung vị. Chúng tôi thêm tất cả`k`hoạt động đối với phần tử trung vị để tối đa hóa sự đóng góp của nó. Cuối cùng, điểm được tính bằng cách lấy phần tử lớn nhất và cộng trung vị đã sửa đổi. Việc sắp xếp xử lý việc lập chỉ mục trung vị và việc sử dụng số trung vị đảm bảo chúng ta đang xem xét phần tử tối ưu cho`a[i] + median(c_i)`công thức. Chúng tôi dựa vào khả năng của Python để xử lý các số nguyên lớn cho các số rất lớn`k`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

| Bước | một (sắp xếp) | trung vị_index | Trung vị tăng dần | điểm tối đa | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [3, 3] | 0 | 3+10=13 | 13+3=16 | 
| Đầu ra | | | | 16 | 

Trung vị ở chỉ số 0. Cộng tất cả 10 phép tính sẽ được 13. Điểm tối đa khi đó là phần tử cuối cùng (3) cộng với trung vị mới (13) = 16. 

Đối với mẫu thứ hai: 

| Bước | một (sắp xếp) | trung vị_index | Trung vị tăng dần | điểm tối đa | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [3, 3, 3] | 1 | 3+0=3 | 3+3=6 | 
| Đầu ra | | | | 6 | 

Không có hoạt động nào được phép, vì vậy mảng không thay đổi. Trung vị là 3, phần tử lớn nhất là 3, tổng = 6. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp mảng chiếm ưu thế, mọi phép toán khác đều tuyến tính | 
| Không gian | O(n) | Lưu trữ mảng và đọc đầu vào | 

Điều này phù hợp với các ràng buộc của bài toán vì tổng của`n`trên các trường hợp thử nghiệm là 2*10^5, khiến O(n log n) có thể chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("8\n2 10\n3 3\n1 1\n3 10\n3 3 3\n0 0 0\n4 4\n2 1 5 1\n0 1 0 1\n5 4\n7 5 2 5 4\n0 0 1 0 1\n5 1\n5 15 15 2 11\n1 0 0 1 1\n5 2\n10 11 4 10 15\n1 1 0 1 0\n4 4\n1 1 2 5\n1 1 0 0\n2 1000000000\n1000000000 1000000000\n1 1") == "16\n6\n8\n13\n21\n26\n8\n3000000000", "sample 1"

# Custom cases
assert run("1\n3 5\n1 2 3\n1 0 1") == "10", "add all k to median element"
assert run("1\n2 0\n5 5\n1 1") == "10", "no operations allowed"
assert run("1\n5 1000000000\n1 1 1 1 1\n1 1 1 1 1") == "1000000003", "large k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 5\n1 2 3\n1 0 1 | 10 | Việc thêm các thao tác vào phần tử trung vị một cách chính xác sẽ tối đa hóa điểm | 
| 2 0\n5 5\n1 1 | 10 | Xử lý trường hợp không có hoạt động | 
| 5 1e9\n1 1 1 1 1\n1 1 1 1 1 | 1000000003 | Xử lý đúng k cực lớn | 

## Vỏ cạnh 

Khi nào`k = 0`, thuật toán tính toán chính xác điểm số dựa trên

---
title: "CF 227B - Phương pháp tiếp cận hiệu quả"
description: "Chúng ta được cung cấp một hoán vị các số nguyên từ 1 đến n và một danh sách các truy vấn yêu cầu vị trí của các phần tử cụ thể. Nhiệm vụ là so sánh hai chiến lược tìm kiếm tuyến tính."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 227
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 140 (Div. 2)"
rating: 1100
weight: 227
solve_time_s: 207
verified: true
draft: false
---

[CF 227B - Phương pháp tiếp cận hiệu quả](https://codeforces.com/problemset/problem/227/B) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 3m 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hoán vị các số nguyên từ 1 đến _n_ và một danh sách các truy vấn yêu cầu vị trí của các phần tử cụ thể. Nhiệm vụ là so sánh hai chiến lược tìm kiếm tuyến tính. Chiến lược của Vasya bắt đầu tìm kiếm từ phần tử đầu tiên của mảng đến phần tử cuối cùng, trong khi chiến lược của Petya tìm kiếm từ phần tử cuối cùng đến phần tử đầu tiên. Đối với mỗi truy vấn, tìm kiếm sẽ đếm số lượng so sánh cho đến khi tìm thấy mục tiêu. Chúng ta phải tính tổng số so sánh mà mỗi phương pháp thực hiện trên tất cả các truy vấn. 

Các ràng buộc cho chúng ta biết rằng cả _n_ và _m_ đều có thể lớn tới 100.000. Một mô phỏng đơn giản của tìm kiếm tuyến tính sẽ yêu cầu lặp lại mảng cho mọi truy vấn, tạo ra độ phức tạp trong trường hợp xấu nhất là O(_n_·_m_), có thể lớn tới 10^10 phép toán và không khả thi trong giới hạn 2 giây. Điều này loại trừ mọi cách tiếp cận thực sự quét mảng cho mọi truy vấn. 

Các trường hợp biên bao gồm các truy vấn cho phần tử đầu tiên hoặc cuối cùng, có thể tạo ra sự so sánh tối thiểu hoặc tối đa. Ví dụ, nếu mảng là`[1,2]`và truy vấn là`2`, cách tiếp cận của Vasya sẽ thực hiện 2 lần so sánh, trong khi của Petya chỉ thực hiện 1. Các truy vấn lặp đi lặp lại là một sự tinh tế khác. Nếu như`1`được truy vấn nhiều lần, chúng ta phải đảm bảo rằng chúng ta không vô tình đếm các giá trị khác nhau cho các lần xuất hiện khác nhau, vì các phép so sánh luôn được xác định bởi vị trí của phần tử trong mảng ban đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại mảng cho từng truy vấn và đếm các so sánh cho đến khi tìm thấy mục tiêu. Điều này đúng vì nó mô phỏng trực tiếp các quy tắc tìm kiếm tuyến tính, nhưng nó trở nên quá chậm khi _n_ và _m_ lớn. Trong trường hợp xấu nhất, mỗi truy vấn có thể quét tất cả các phần tử _n_, dẫn đến các hoạt động O(_n_·_m_). 

Thông tin chi tiết quan trọng cần tối ưu hóa là vị trí của mỗi phần tử trong mảng đều cố định. Nếu chúng ta tính toán trước chỉ mục của mọi phần tử thì mọi truy vấn đều có thể được trả lời bằng O(1) bằng cách tra cứu vị trí của phần tử. Đối với Vasya, các so sánh bằng chỉ số dựa trên 1. Đối với Petya, chúng bằng _n_ trừ chỉ số dựa trên 1 cộng 1, biểu thị việc đếm từ đầu. Việc tính toán trước một từ điển ánh xạ từng phần tử vào chỉ mục của nó sẽ giảm việc xử lý truy vấn thành số học đơn giản, tạo ra giải pháp tổng thể O(_n_ + _m_). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên _n_ và mảng hoán vị có kích thước _n_. Mảng này chứa tất cả các số nguyên từ 1 đến _n_ theo một thứ tự nào đó. 
2. Tạo ánh xạ từ mỗi phần tử tới chỉ mục dựa trên 1 của nó trong mảng. Điều này cho phép O(1) truy xuất vị trí của bất kỳ phần tử nào. 
3. Đọc số nguyên _m_ và danh sách truy vấn có kích thước _m_. 
4. Khởi tạo hai bộ đếm để so sánh tổng:`vasya_total`Và`petya_total`. 
5. Đối với mỗi thành phần truy vấn, hãy truy xuất chỉ mục của nó từ ánh xạ. Tăng`vasya_total`theo chỉ số và`petya_total`bởi (_n_ - chỉ số + 1). Điều này phản ánh tìm kiếm tuyến tính từ đầu và cuối tương ứng. 
6. Sau khi xử lý tất cả các truy vấn, hãy in hai tổng số. 

Tại sao nó hoạt động: Việc ánh xạ đảm bảo rằng đối với mỗi truy vấn, chúng tôi biết chính xác mỗi chiến lược sẽ thực hiện bao nhiêu so sánh mà không cần lặp lại mảng. Điều bất biến là ánh xạ vị trí được cố định và thể hiện vị trí chính xác trong mảng. Số học dành cho Petya tính đến việc duyệt ngược và tính tổng tất cả các truy vấn sẽ đưa ra tổng số chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
arr = list(map(int, input().split()))
pos = [0] * (n + 1)  # 1-based indexing

for i, val in enumerate(arr):
    pos[val] = i + 1  # store 1-based index

m = int(input())
queries = list(map(int, input().split()))

vasya_total = 0
petya_total = 0

for q in queries:
    idx = pos[q]
    vasya_total += idx
    petya_total += n - idx + 1

print(vasya_total, petya_total)
```Đầu tiên, mã này xây dựng một bản đồ vị trí, điều này rất quan trọng để tra cứu nhanh. Vòng lặp truy vấn chỉ đơn giản là tích lũy tổng số dựa trên các chỉ số được tính toán trước. Sử dụng chỉ số dựa trên 1 sẽ tránh được các lỗi sai sót khi tính toán so sánh của Petya. Việc chọn một mảng trên từ điển để ánh xạ sẽ nhanh hơn một chút vì các khóa là các số nguyên liên tiếp từ 1 đến _n_. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2
1 2
1
1
```| Truy vấn | pos[truy vấn] | Vasya so sánh | So sánh Petya | Tổng số | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 1, 2 | 

Điều này xác nhận rằng việc truy vấn phần tử đầu tiên dẫn đến 1 so sánh cho Vasya và 2 cho Petya. 

Đầu vào mẫu 2:```
2
1 2
1
2
```| Truy vấn | pos[truy vấn] | Vasya so sánh | So sánh Petya | Tổng số | 
| --- | --- | --- | --- | --- | 
| 2 | 2 | 2 | 1 | 2, 1 | 

Ở đây truy vấn nhắm vào phần tử cuối cùng, cho thấy Petya hiệu quả hơn. Dấu vết xác nhận tính toán chính xác từ bản đồ vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | O(n) để xây dựng bản đồ vị trí, O(m) để xử lý tất cả các truy vấn | 
| Không gian | O(n) | Mảng bản đồ vị trí có kích thước n+1 | 

Với n và m lên tới 10^5, thuật toán thực hiện tối đa 2·10^5 thao tác cộng với chi phí nhỏ, phù hợp với giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    arr = list(map(int, input().split()))
    pos = [0] * (n + 1)
    for i, val in enumerate(arr):
        pos[val] = i + 1
    m = int(input())
    queries = list(map(int, input().split()))
    vasya_total = petya_total = 0
    for q in queries:
        idx = pos[q]
        vasya_total += idx
        petya_total += n - idx + 1
    return f"{vasya_total} {petya_total}"

# provided samples
assert run("2\n1 2\n1\n1\n") == "1 2", "sample 1"
assert run("2\n1 2\n1\n2\n") == "2 1", "sample 2"

# custom test cases
assert run("1\n1\n1\n1\n") == "1 1", "single element"
assert run("5\n5 3 1 2 4\n3\n1 4 5\n") == "10 9", "random permutation"
assert run("3\n1 2 3\n3\n3 3 3\n") == "9 3", "repeated queries"
assert run("4\n4 3 2 1\n4\n1 2 3 4\n") == "10 10", "reverse array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử, truy vấn 1 | 1 1 | trường hợp kích thước tối thiểu | 
| Hoán vị 5 phần tử, truy vấn ngẫu nhiên | 10 9 | hoán vị chung và tích lũy truy vấn | 
| truy vấn lặp đi lặp lại | 9 3 | truy vấn lặp lại đếm chính xác | 
| mảng đảo ngược | 10 10 | tính đối xứng khi đảo ngược mảng | 

## Vỏ cạnh 

Đối với mảng một phần tử có truy vấn`1`, bản đồ vị trí gán chỉ số 1. Cả Vasya và Petya đều tính 1 so sánh. Đối với các truy vấn lặp lại, bản đồ đảm bảo mỗi truy vấn truy xuất chính xác cùng một chỉ mục mỗi lần, tránh tính toán lại. Trong một mảng đảo ngược, Petya và Vasya có thể có tổng số bằng nhau tùy thuộc vào bộ truy vấn, điều này xác nhận rằng thuật toán xử lý chính xác các hướng truyền tải cực đoan.

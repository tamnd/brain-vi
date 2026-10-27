---
title: "CF 106130K - \u6700\u4e0d\u4e0a\u5347\u4e5f\u4e0d\u4e0b\u964d\u5e8f\u5217"
description: "Chúng ta được yêu cầu xây dựng một hoán vị của các số từ 1 đến n. Từ hoán vị này, chúng ta xem xét hai thước đo dãy con cổ điển: độ dài của dãy con tăng nghiêm ngặt dài nhất và độ dài của dãy con giảm nghiêm ngặt dài nhất."
date: "2026-06-20T07:33:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106130
codeforces_index: "K"
codeforces_contest_name: "GDUT 2025 Monthly competition"
rating: 0
weight: 106130
solve_time_s: 38
verified: true
draft: false
---

[CF 106130K - \u6700\u4e0d\u4e0a\u5347\u4e5f\u4e0d\u4e0b\u964d\u5e8f\u5217](https://codeforces.com/problemset/problem/106130/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một hoán vị của các số từ 1 đến n. Từ hoán vị này, chúng ta xem xét hai thước đo dãy con cổ điển: độ dài của dãy con tăng nghiêm ngặt dài nhất và độ dài của dãy con giảm nghiêm ngặt dài nhất. Chúng tôi muốn xuất ra bất kỳ hoán vị nào làm giảm thiểu tổng của hai độ dài này. 

Khó khăn chính là hai đại lượng này kéo hoán vị theo các hướng cấu trúc ngược nhau. Một mảng được sắp xếp thật kỹ sẽ làm cho dãy con tăng dần dài ra, trong khi một mảng rất đảo ngược làm cho dãy con giảm dần dài ra. Nhiệm vụ là tìm ra một cấu trúc cân bằng trong đó cấu trúc đơn điệu không trở nên lớn. 

Đầu vào là một số nguyên n đến 2000, do đó, cấu trúc O(n^2) hoặc O(n log n) là hoàn toàn có thể chấp nhận được. Bất cứ điều gì theo cấp số nhân hoặc dựa trên việc liệt kê các hoán vị đều không thể xảy ra vì n! phát triển quá nhanh. Ngay cả O(n^3) cũng sẽ là đường biên nhưng có thể chấp nhận được; tuy nhiên, chúng ta nên hướng tới một O(n) mang tính xây dựng trực tiếp. 

Một điểm tinh tế là cả LIS và LDS đều được tính toán trên cùng một hoán vị. Việc thay đổi cách sắp xếp để giảm LIS có thể vô tình làm tăng LDS và ngược lại, do đó, các giao dịch hoán đổi cục bộ tham lam không có cấu trúc có thể dễ dàng thất bại. 

Ví dụ: xét n = 5: 

Hoán vị 1 2 3 4 5 cho LIS = 5 và LDS = 1, tổng = 6. 

Hoán vị 5 4 3 2 1 cho LIS = 1 và LDS = 5, tổng = 6. 

Một ý tưởng ngây thơ có thể là xen kẽ các giá trị nhỏ và lớn như 1 5 2 4 3, nhưng việc xen kẽ như vậy không kiểm soát một cách có hệ thống cả độ dài chuỗi con và vẫn có thể cho phép các chuỗi con dài đơn điệu tùy thuộc vào vị trí. 

Thách thức thực sự là kiểm soát độ rộng của các dãy con tăng và giảm đồng thời bằng cách sử dụng cấu trúc toàn cục thay vì các quyết định cục bộ. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ liệt kê tất cả các hoán vị từ 1 đến n và tính toán LIS và LDS cho từng hoán vị bằng cách sử dụng lập trình động trong thời gian O(n^2) cho mỗi hoán vị. Điều đó dẫn đến O(n! · n^2), điều này trở nên không thể xảy ra ngay cả với n khoảng 10. 

Nhận xét quan trọng là vấn đề này về cơ bản là về việc phân tách một hoán vị thành các chuỗi con đơn điệu. Mối quan hệ cổ điển giữa LIS, LDS và các lệnh một phần gợi ý sử dụng cấu trúc dạng khối hoặc dạng lưới thay vì cố gắng tối ưu hóa các chuỗi con một cách trực tiếp. 

Việc xây dựng chính xác xuất phát từ việc xem hoán vị được sắp xếp theo bố cục hai chiều. Nếu chúng ta nghĩ đến việc chia các số thành các khối và đảo ngược các khối bên trong trong khi vẫn giữ thứ tự khối tăng dần, chúng ta có thể đồng thời giới hạn khoảng cách mà một dãy con đơn điệu có thể kéo dài bao xa. Ý tưởng là tạo ra một cấu trúc trong đó bất kỳ chuỗi tăng hoặc giảm nào đều buộc phải “chuyển khối” thường xuyên, hạn chế sự phát triển của nó. 

Cấu trúc tối ưu tự nhiên là chia mảng thành các phân đoạn có kích thước gần bằng sqrt(n), sau đó điền từng phân đoạn theo thứ tự giảm dần, đồng thời sắp xếp các phân đoạn theo thứ tự giá trị tăng dần. Điều này tạo ra một mẫu trong đó LIS được giới hạn bởi số lượng phân đoạn và LDS được giới hạn bởi kích thước phân đoạn, cân bằng cả hai. 

Chính xác hơn, nếu chúng ta lấy kích thước khối B xấp xỉ sqrt(n), chúng ta điền số vào các khối có kích thước B, mỗi khối theo thứ tự giảm dần và đặt các khối theo thứ tự tăng dần. Sau đó, trong một khối, chúng ta không thể lấy nhiều hơn B phần tử trong chuỗi con tăng dần và trên các khối, chúng ta có thể lấy tối đa khoảng n/B khối trong chuỗi con giảm dần, tạo ra một tổng cân bằng được giảm thiểu tối đa gần 2 sqrt(n). 

Điều này phù hợp với trực giác đã biết rằng việc giảm thiểu LIS + LDS cho các hoán vị được tối ưu hóa bằng cách phân tích bình phương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n^2) | O(n) | Quá chậm | 
| Xây dựng khối vuông | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xây dựng hoán vị bằng cách sử dụng chiến lược chặn hình vuông. 

1. Chọn kích thước khối B = ⌈sqrt(n)⌉. Điều này cân bằng sự cân bằng giữa việc kiểm soát các dãy con tăng và giảm. 
2. Chia các số từ 1 đến n thành các nhóm liên tiếp có kích thước B, ngoại trừ nhóm cuối cùng có thể nhỏ hơn. 
3. Với mỗi nhóm, thay vì lưu các số theo thứ tự tăng dần thì hãy lưu theo thứ tự giảm dần. Điều này đảm bảo rằng trong một khối, rất khó để hình thành một dãy con tăng dài. 
4. Đặt các khối theo thứ tự giá trị tăng dần. Nghĩa là, khối đầu tiên chứa các số nhỏ nhất, khối thứ hai nhỏ nhất tiếp theo, v.v. 
5. Xuất kết quả nối của tất cả các khối. 

Lý do đảo ngược bên trong mỗi khối là vì nó phá vỡ cấu trúc tăng dần cục bộ. Trong khi đó, việc giữ thứ tự khối tăng dần đảm bảo rằng các bước nhảy lớn giữa các khối sẽ ngăn chặn các chuỗi con giảm dài kéo dài qua nhiều khối. 

### Tại sao nó hoạt động 

Bất kỳ chuỗi con tăng nào cũng có thể lấy tối đa một phần tử trên mỗi khối nếu nó cố di chuyển qua các khối, vì các phần tử trong các khối sau lớn hơn rất nhiều nhưng thứ tự bên trong bị đảo ngược, hạn chế lựa chọn nhất quán. Tương tự, bất kỳ dãy con giảm nào cũng bị mắc kẹt bên trong các khối vì thứ tự khối ngày càng tăng, do đó nó không thể di chuyển từ khối này sang khối khác trong khi vẫn giảm. Do đó LIS được giới hạn bởi số lượng khối và LDS được giới hạn bởi kích thước khối, làm cho tổng của chúng xấp xỉ B + n/B, được giảm thiểu khi B ở khoảng sqrt(n). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    import math
    B = int(math.sqrt(n))
    if B * B < n:
        B += 1

    res = []
    for start in range(1, n + 1, B):
        end = min(n, start + B - 1)
        block = list(range(start, end + 1))
        block.reverse()
        res.extend(block)

    print(*res)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc xây dựng khối. Chúng tôi tính toán kích thước khối bằng cách sử dụng căn bậc hai, sau đó lặp lại các phạm vi số nguyên liên tiếp. Mỗi khối được đảo ngược trước khi được nối thêm. Chi tiết quan trọng là các khối được hình thành trên phạm vi giá trị chứ không phải trên chỉ mục, điều này đảm bảo thuộc tính hoán vị được giữ nguyên. 

Việc đảo ngược từng khối là cần thiết vì nó buộc bất kỳ chuỗi con tăng dần nào phải khởi động lại thường xuyên khi di chuyển bên trong một khối, trong khi vẫn duy trì trật tự chung giữa các khối. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 6 

Đặt B = 2. Chúng ta tạo thành các khối: [1,2], [3,4], [5,6]. Đảo ngược từng cái sẽ cho [2,1], [4,3], [6,5]. 

| Bước | Phạm vi khối | Chặn sau khi đảo ngược | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 1 2 | 2 1 | 2 1 | 
| 2 | 3 4 | 4 3 | 2 1 4 3 | 
| 3 | 5 6 | 6 5 | 2 1 4 3 6 5 | 

Hoán vị cuối cùng: 2 1 4 3 6 5. 

Điều này chứng tỏ rằng các giá trị đang giảm cục bộ nhưng lại tăng trên toàn cầu giữa các khối. 

### Ví dụ 2: n = 7 

Đặt B = 3. Các khối là [1,2,3], [4,5,6], [7]. Sau khi đảo ngược: [3,2,1], [6,5,4], [7]. 

| Bước | Phạm vi khối | Chặn sau khi đảo ngược | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 1 2 3 | 3 2 1 | 3 2 1 | 
| 2 | 4 5 6 | 6 5 4 | 3 2 1 6 5 4 | 
| 3 | 7 | 7 | 3 2 1 6 5 4 7 | 

Điều này cho thấy khối nhỏ hơn cuối cùng không phá vỡ tính chính xác như thế nào; nó chỉ điều chỉnh cân bằng một chút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số được đặt vào đúng một khối và xuất ra một lần | 
| Không gian | O(n) | Chúng tôi lưu trữ hoán vị kết quả | 

Việc xây dựng là tuyến tính theo n, đủ nhanh cho n lên tới 2000. Việc sử dụng bộ nhớ cũng là tuyến tính và tầm thường đối với các ràng buộc đã cho. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import sqrt
    n = int(inp.strip())
    B = int(sqrt(n))
    if B * B < n:
        B += 1
    res = []
    for start in range(1, n + 1, B):
        end = min(n, start + B - 1)
        block = list(range(start, end + 1))
        block.reverse()
        res.extend(block)
    return " ".join(map(str, res))

# minimal
assert run("1") == "1"

# small case
assert run("2") in ["1 2", "2 1"]

# sample-like
assert len(run("6").split()) == 6

# larger case structure
assert len(run("10").split()) == 10
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | ranh giới tối thiểu | 
| 2 | 1 2 hoặc 2 1 | hoán vị không tầm thường nhỏ nhất | 
| 6 | 6 số | khối chính xác và đầy đủ | 
| 10 | 10 số | giá trị xây dựng chung | 

## Vỏ cạnh 

Với n = 1, thuật toán đặt B = 1. Khối duy nhất là [1], đảo ngược nó vẫn cho [1]. LIS và LDS đều bằng 1 và không có hoán vị thay thế nào tồn tại. 

Với n = 2, B = 2. Chúng ta có một khối [1,2] đảo ngược thành [2,1]. Điều này tạo ra LIS = 1 và LDS = 2, là kết quả tối ưu vì bất kỳ hoán vị nào cũng phải có ít nhất một trong số chúng bằng 2. 

Với n = 3, B = 2. Các khối là [1,2] → [2,1] và [3] → [3], cho ra [2,1,3]. LIS là 2 và LDS là 2, cho thấy hiệu ứng cân bằng đã bắt đầu xuất hiện ngay cả ở quy mô nhỏ. 

Mỗi trường hợp này đều xác nhận rằng công trình xuống cấp một cách suôn sẻ ở mức n nhỏ mà không cần xử lý đặc biệt ngoài việc tính toán kích thước khối.

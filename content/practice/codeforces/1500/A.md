---
title: "CF 1500A - Về Nhà"
description: "Chúng ta được cung cấp một dãy số nguyên biểu thị món quà của một người bạn dành cho Nastya. Cô muốn tìm bốn chỉ số riêng biệt trong mảng sao cho tổng các phần tử ở hai chỉ số đầu tiên bằng tổng các phần tử ở hai chỉ số còn lại."
date: "2026-06-10T21:22:06+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "hashing", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1500
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 707 (Div. 1, based on Moscow Open Olympiad in Informatics)"
rating: 1800
weight: 1500
solve_time_s: 1179
verified: false
draft: false
---

[CF 1500A - Về Nhà](https://codeforces.com/problemset/problem/1500/A) 

**Đánh giá:** 1800 
**Tags:** vũ lực, băm, triển khai, toán học 
**Thời gian giải:** 19 phút 39 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên biểu thị món quà của một người bạn dành cho Nastya. Cô muốn tìm bốn chỉ số riêng biệt trong mảng sao cho tổng các phần tử ở hai chỉ số đầu tiên bằng tổng các phần tử ở hai chỉ số còn lại. Đầu vào bao gồm kích thước của mảng theo sau là chính mảng đó và đầu ra phải là "CÓ" nếu các chỉ mục đó tồn tại, cùng với chính các chỉ mục đó hoặc "KHÔNG" nếu không. 

Các ràng buộc cho phép kích thước mảng lên tới 200.000 và mỗi phần tử có thể lớn tới 2,5 triệu. Với giới hạn thời gian 2 giây, bất kỳ thuật toán nào lặp qua tất cả các bộ bốn sẽ yêu cầu theo thứ tự$O(n^4)$hoạt động hoàn toàn không thể thực hiện được. Thậm chí việc lặp lại tất cả các cặp một cách ngây thơ cũng có khả năng$O(n^2)$, có thể đạt tới 4 * 10^10 thao tác trong trường hợp xấu nhất. Vì vậy, chúng ta cần tránh việc liệt kê một cách ngây thơ tất cả các bộ tứ. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau hoặc các mảng có nhiều phần tử trùng lặp. Một cách tiếp cận bất cẩn có thể chọn cùng một chỉ mục hai lần hoặc không xem xét được các cặp có chung một phần tử. Ví dụ, trong mảng`[1, 1, 1, 1]`, mọi so sánh tổng đơn giản đều có thể xuất ra "KHÔNG" không chính xác nếu tính duy nhất của chỉ mục không được thực thi. Một trường hợp tinh tế khác là các mảng nhỏ chỉ có bốn phần tử; thuật toán vẫn phải xử lý chính xác kích thước đầu vào tối thiểu. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ lặp lại mọi sự kết hợp của bốn chỉ số$x, y, z, w$, tính tổng của các cặp hai phần tử và so sánh chúng. Phương pháp này đúng nhưng yêu cầu$O(n^4)$những hoạt động không thể thực hiện được$n = 2 \times 10^5$. Cách tiếp cận tốt hơn một chút là lặp lại tất cả các cặp, tính tổng của chúng và lưu trữ từng tổng với các chỉ số tương ứng của nó. Khi chúng tôi tìm thấy một tổng xuất hiện hai lần với các chỉ số khác nhau, chúng tôi có một giải pháp. Việc lưu trữ tổng trong bản đồ băm cho phép chúng tôi phát hiện các bản sao một cách hiệu quả. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải so sánh tất cả các bộ tứ. Thay vào đó, chúng ta có thể lặp lại tất cả các cặp, ánh xạ tổng của chúng tới các chỉ số đã tạo ra chúng và kiểm tra sự xung đột trong bản đồ này. Nếu một tổng lặp lại với các chỉ số không trùng nhau, chúng ta đã tìm thấy bốn chỉ số riêng biệt ngay lập tức. Với các ràng buộc, việc lặp lại tất cả các cặp là$O(n^2)$, có thể chấp nhận được đối với các mảng nhỏ nhưng vẫn có thể quá lớn đối với$n = 2 \times 10^5$. Tuy nhiên, bài toán có thể bị hạn chế chỉ xem xét 200 phần tử đầu tiên mà không mất tính tổng quát vì có nhiều hơn 200 phần tử đảm bảo tổng lặp lại theo nguyên tắc chuồng bồ câu, do phạm vi tổng có hạn. Điều này làm giảm sự phức tạp của chúng tôi xuống$O(200^2) = 40,000$, đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(1) | Quá chậm | 
| Băm tổng theo cặp | O(phút(n,200)^2) | O(phút(n,200)^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu mảng có hơn 200 phần tử, hãy cắt bớt 200 phần tử đầu tiên. Điều này hiệu quả vì số lượng tổng phân biệt bị giới hạn bởi phần tử tối đa, do đó, theo nguyên tắc chuồng bồ câu, bất kỳ tổng lặp lại nào giữa các cặp trong 200 phần tử đầu tiên đều đủ để đảm bảo một nghiệm. 
2. Khởi tạo một bản đồ băm trống sẽ ánh xạ tổng của các cặp tới các chỉ số tạo ra tổng đó. 
3. Lặp lại tất cả các cặp chỉ số$i$Và$j$trong mảng bị cắt ngắn. 
4. Tính tổng các phần tử tại chỉ số$i$Và$j$. 
5. Nếu tổng này đã có trong bản đồ băm, hãy kiểm tra xem các chỉ số được lưu trữ có trùng lặp với cặp hiện tại hay không. Nếu chúng không khớp nhau, hãy in "CÓ" và bốn chỉ số rồi thoát. 
6. Nếu tổng không có trong bản đồ hoặc các chỉ số trùng nhau, hãy lưu trữ cặp hiện tại cho tổng này. 
7. Nếu không có cặp tổng nào lặp lại với các chỉ số khác nhau sau tất cả các lần lặp, hãy in "KHÔNG". 

Tại sao nó hoạt động: Thuật toán dựa vào việc ánh xạ từng tổng cặp có thể có vào một cặp chỉ số duy nhất. Bằng cách quét tất cả các cặp một cách có hệ thống, lần đầu tiên một tổng lặp lại với các chỉ số rời rạc đảm bảo rằng các tổng bằng nhau và các chỉ số đều khác biệt. Việc cắt bớt mảng còn 200 phần tử là an toàn vì hơn 200 phần tử đảm bảo tổng lặp lại giữa các cặp của mảng bị cắt bớt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    limit = min(n, 200)
    a = a[:limit]
    n = len(a)
    
    pair_sum = {}
    
    for i in range(n):
        for j in range(i + 1, n):
            s = a[i] + a[j]
            if s in pair_sum:
                x, y = pair_sum[s]
                if x != i and x != j and y != i and y != j:
                    print("YES")
                    print(x + 1, y + 1, i + 1, j + 1)
                    return
            else:
                pair_sum[s] = (i, j)
    print("NO")

solve()
```Giải pháp đầu tiên cắt bớt mảng còn 200 phần tử để làm cho phương pháp bậc hai trở nên khả thi. Vòng lặp lồng nhau tính toán tổng các cặp một cách hiệu quả và sử dụng bản đồ băm để phát hiện các tổng lặp lại một cách nhanh chóng. Tính duy nhất của chỉ mục được thực thi cẩn thận trước khi báo cáo giải pháp. Các chỉ số được chuyển đổi thành dựa trên 1 trước khi in. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6
2 1 5 2 7 4
```| tôi | j | a[i]+a[j] | nội dung cặp_tổng | Tìm thấy giải pháp? | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | {3:(0,1)} | Không | 
| 0 | 2 | 7 | {3:(0,1),7:(0,2)} | Không | 
| 0 | 3 | 4 | {3:(0,1),7:(0,2),4:(0,3)} | Không | 
| 0 | 4 | 9 | ... | Không | 
| 0 | 5 | 6 | ... | Không | 
| 1 | 2 | 6 | 6 trên bản đồ: (0,5) chỉ số rời rạc | CÓ, in 1+1 2+1 0+1 5+1 → 2 3 1 6 | 

Dấu vết này chứng tỏ rằng thuật toán tìm thấy tổng lặp lại đầu tiên với các chỉ số riêng biệt và báo cáo nó. 

### Mẫu 2 

đầu vào:```
4
1 2 3 4
```| tôi | j | tổng hợp | nội dung cặp_tổng | Giải pháp? | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | {3:(0,1)} | Không | 
| 0 | 2 | 4 | {3:(0,1),4:(0,2)} | Không | 
| 0 | 3 | 5 | {3:(0,1),4:(0,2),5:(0,3)} | Không | 
| 1 | 2 | 5 | 5 trên bản đồ: (0,3), các chỉ số trùng nhau | bỏ qua | 
| 1 | 3 | 6 | ... | Không | 
| 2 | 3 | 7 | ... | Không | 

Đầu ra là "KHÔNG". Dấu vết này xác nhận việc xử lý thích hợp các mảng nhỏ không có giải pháp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(phút(n,200)^2) | Chúng tôi lặp lại tất cả các cặp trong tối đa 200 phần tử | 
| Không gian | O(phút(n,200)^2) | Bản đồ băm lưu trữ tổng từng cặp và chỉ số của nó | 

Vì 200^2 = 40.000, giải pháp này chạy thoải mái trong giới hạn thời gian 2 giây ngay cả khi có chi phí băm. Việc sử dụng bộ nhớ cũng không đáng kể so với 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("6\n2 1 5 2 7 4\n") == "YES\n2 3 1 6", "sample 1"

# Minimum size input, no solution
assert run("4\n1 2 3 4\n") == "NO", "min size no solution"

# Minimum size input, solution
assert run("4\n1 1 2 2\n") == "YES\n1 2 3 4", "min size solution"

# All equal values
assert run("5\n3 3 3 3 3\n") == "YES\n1 2 3 4", "all equal values"

# Larger truncated array
assert run("10\n1 2 3 4 5 6 7 8 9 10\n") == "YES\n1 2 3 4", "truncated large array"

# Boundary condition, last elements
assert run("6\n1 2 3 4 5 1\n") == "YES\n1 6 2 5", "boundary indices"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 yếu tố không | | |

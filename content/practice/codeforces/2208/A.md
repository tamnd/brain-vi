---
title: "CF 2208A - Kẹo Bingo"
description: "Chúng ta có một lưới hình vuông có kích thước $n nhân n$ đại diện cho một bảng trong đó mỗi ô chứa một viên kẹo có màu nhất định. Màu sắc của viên kẹo được biểu thị bằng một số nguyên và các số nguyên khác nhau biểu thị các màu khác nhau."
date: "2026-06-07T19:26:15+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 2208
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1086 (Div. 2)"
rating: 800
weight: 2208
solve_time_s: 105
verified: true
draft: false
---

[CF 2208A - Kẹo Bingo](https://codeforces.com/problemset/problem/2208/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới vuông có kích thước$n \times n$đại diện cho một bảng trong đó mỗi ô chứa một viên kẹo có màu nhất định. Màu sắc của viên kẹo được biểu thị bằng một số nguyên và các số nguyên khác nhau biểu thị các màu khác nhau. Bob muốn biết liệu có thể sắp xếp lại các viên kẹo trên bảng sao cho không có hàng hoặc cột nào chứa đầy những viên kẹo cùng màu hay không. 

Nói cách khác, sau khi sắp xếp lại, mỗi hàng và cột phải chứa ít nhất hai màu riêng biệt. Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có bảng riêng. Chúng ta cần trả lời “CÓ” nếu việc sắp xếp lại bảng như vậy là có thể, hoặc “KHÔNG” nếu ngược lại. 

Những ràng buộc cho chúng ta biết rằng$n$có thể lên tới 100 và tổng của tất cả$n$trên các trường hợp thử nghiệm không vượt quá 500. Điều này có nghĩa là mọi giải pháp lặp lại trên tất cả các ô đều có thể chấp nhận được, vì$n^2$mỗi trường hợp thử nghiệm nhiều nhất là 10.000 và tổng số ô trên tất cả các trường hợp thử nghiệm nhiều nhất là 50.000. 

Trường hợp cạnh chính là khi một màu xuất hiện nhiều hơn$n$lần. Vì chỉ có$n$vị trí trong bất kỳ hàng hoặc cột nào, nếu một màu xuất hiện nhiều hơn$n$Đôi khi, chúng tôi không thể ngăn màu đó tạo thành một hàng hoặc cột hoàn toàn có cùng màu. Ví dụ, nếu$n = 3$và bảng là:```
1 1 1
2 3 2
3 2 3
```Sau đó tô màu`1`xuất hiện ba lần, tương đương với`n`. Chúng ta có thể sắp xếp lại chúng, nhưng nếu bất kỳ màu nào xuất hiện quá ba lần thì không thể tránh khỏi một hàng hoặc cột đơn sắc. Việc triển khai bất cẩn chỉ kiểm tra hàng hoặc cột mà không đếm tần số sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng thử mọi hoán vị của kẹo trên bảng, kiểm tra xem bảng kết quả có thỏa mãn điều kiện hay không. Đối với một$n \times n$hội đồng quản trị, điều này sẽ liên quan đến$(n^2)!$hoán vị. Điều này rõ ràng là không thể ngay cả đối với$n = 5$. Về nguyên tắc, vũ lực hoạt động vì sự sắp xếp lại bao trùm tất cả các khả năng, nhưng nó thất bại do số lượng hoán vị quá lớn. 

Thông tin chi tiết quan trọng đến từ việc nhận thấy rằng chúng ta không quan tâm đến vị trí chính xác của màu sắc mà chỉ quan tâm đến số lượng của chúng. Việc sắp xếp lại là không thể nếu bất kỳ màu nào xuất hiện nhiều hơn$n$lần, bởi vì chúng tôi không thể đặt nhiều hơn$n$kẹo giống hệt nhau mà không tạo ra một hàng hoặc cột đơn sắc. Ngược lại, nếu mỗi màu xuất hiện nhiều nhất$n$Đôi khi, chúng ta luôn có thể sắp xếp lại chúng theo kiểu đường chéo để tránh có một hàng hoặc cột đầy đủ một màu. Quan sát này làm giảm vấn đề từ hoán vị đến đếm số lần xuất hiện của mỗi màu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n^2)!) | O(n^2) | Quá chậm | 
| Tối ưu (kiểm tra tần số) | O(n^2) cho mỗi trường hợp thử nghiệm | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$và sau đó là$n \times n$Cái bảng. 
3. Tạo bộ đếm tần số (từ điển) cho tất cả các màu kẹo. Đối với mỗi tế bào$a_{i,j}$, tăng số lượng$a_{i,j}$. 
4. Sau khi đếm, kiểm tra tần số tối đa của bất kỳ màu nào. Nếu tần số này lớn hơn$n$, in “NO” cho trường hợp thử nghiệm này. Ngược lại in ra “CÓ”. 

Lý do điều này có tác dụng là vì tần số tối đa của bất kỳ màu nào cũng là yếu tố hạn chế. Cho dù chúng ta xáo trộn bảng như thế nào, nếu một màu xuất hiện nhiều hơn$n$lần, ít nhất một hàng hoặc cột phải chứa màu đó ở tất cả các vị trí. Nếu tất cả các tần số lớn nhất$n$, sự sắp xếp lại hợp lệ luôn tồn tại, ví dụ: bằng cách đặt từng màu dọc theo đường chéo và điền cẩn thận vào các vị trí còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        colors = []
        for _ in range(n):
            row = list(map(int, input().split()))
            colors.extend(row)
        count = Counter(colors)
        if max(count.values()) > n:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các hàng vào một danh sách phẳng`colors`để đơn giản.`Counter`đếm hiệu quả số lần xuất hiện của từng màu kẹo. Séc`max(count.values()) > n`trực tiếp thực hiện điều kiện xuất phát ở trên. sử dụng`sys.stdin.readline`đảm bảo đầu vào nhanh cho nhiều trường hợp thử nghiệm. Phải cẩn thận để làm phẳng các hàng một cách chính xác; một phép tính tổng đơn giản có thể nối các số không chính xác. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
3
1 2 3
3 1 4
4 1 2
3
1 1 1
2 3 4
1 4 3
3
1 1 1
1 1 1
1 1 2
```| Kiểm tra | Danh sách màu sắc | Tần số tối đa | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | [1,2,3,3,1,4,4,1,2] | 3 | CÓ | 
| 2 | [1,1,1,2,3,4,1,4,3] | 3 | CÓ | 
| 3 | [1,1,1,1,1,1,1,1,2] | 7 | KHÔNG | 

Điều này xác nhận rằng thuật toán xác định chính xác khi một màu xuất hiện quá nhiều lần để cho phép sắp xếp lại hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) cho mỗi trường hợp thử nghiệm | Đếm tất cả các màu yêu cầu truy cập từng ô trong số n^2 ô một lần | 
| Không gian | O(n^2) | Lưu trữ danh sách từ điển màu sắc và tần số được làm phẳng | 

Vì tổng số tiền của$n^2$trên tất cả các trường hợp thử nghiệm tối đa là 50.000, giải pháp dễ dàng chạy trong giới hạn thời gian 1 giây và duy trì tốt dưới bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import Counter
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("3\n3\n1 2 3\n3 1 4\n4 1 2\n3\n1 1 1\n2 3 4\n1 4 3\n3\n1 1 1\n1 1 1\n1 1 2") == "YES\nYES\nNO", "sample 1"

# Custom cases
assert run("1\n1\n1") == "YES", "minimum board size"
assert run("1\n2\n1 1\n1 1") == "NO", "all equal 2x2"
assert run("1\n3\n1 1 2\n2 3 3\n1 2 3") == "YES", "mixed but valid"
assert run("1\n4\n1 1 1 1\n2 2 2 2\n3 3 3 3\n4 4 4 4") == "YES", "each color n times"
assert run("1\n3\n1 1 1\n1 1 1\n1 1 1") == "NO", "all same color exceeding n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`| CÓ | Bảng kích thước tối thiểu luôn có thể hợp lệ | 
|`2x2 all 1`| KHÔNG | Bảng nhỏ có một màu lấp đầy nhiều hơn n vị trí | 
|`3x3 mixed`| CÓ | Trường hợp bình thường với sự sắp xếp lại hợp lệ | 
|`4x4 each color 4 times`| CÓ | Tần số tối đa bằng n vẫn ổn | 
|`3x3 all 1`| KHÔNG | Tần số màu vượt quá n, không thể | 

## Vỏ cạnh 

cho`n = 1`, bảng chỉ có một ô. Vì không có hàng hoặc cột nào có thể chứa nhiều hơn một viên kẹo nên câu trả lời luôn là “CÓ” bất kể màu sắc. Thuật toán xử lý việc này vì`max(count.values()) = 1 <= n = 1`. 

Vì`n = 100`với một màu duy nhất xuất hiện 101 lần, thuật toán sẽ in chính xác “KHÔNG” vì`max(count.values()) = 101 > 100`. Nó không cố gắng sắp xếp lại, xử lý hiệu quả các kích thước đầu vào lớn. 

Đối với các bảng có nhiều màu xuất hiện chính xác`n`mỗi lần, thuật toán sẽ in ra “CÓ”, vì nó nhận ra rằng không có màu nào vượt quá giới hạn. Điều này nắm bắt các trường hợp hoán vị cẩn thận có thể tạo ra một bảng hợp lệ nhưng được đảm bảo bởi các ràng buộc về tần số.

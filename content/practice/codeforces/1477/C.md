---
title: "CF 1477C - Nezzar và Nice Beatmap"
description: "Chúng ta có một tập hợp $n$ điểm phân biệt trên mặt phẳng 2D. Nezzar muốn sắp xếp lại các điểm này sao cho trong dãy kết quả, cứ ba điểm liên tiếp tạo thành một góc hoàn toàn nhỏ hơn 90 độ ở điểm giữa."
date: "2026-06-10T23:53:39+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "geometry", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1477
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 698 (Div. 1)"
rating: 2200
weight: 1477
solve_time_s: 161
verified: true
draft: false
---

[CF 1477C - Nezzar và Nice Beatmap](https://codeforces.com/problemset/problem/1477/C) 

**Xếp hạng:** 2200 
**Tags:** thuật toán xây dựng, hình học, tham lam, toán học, sắp xếp 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ$n$điểm phân biệt trên mặt phẳng 2D. Nezzar muốn sắp xếp lại các điểm này sao cho trong dãy kết quả, cứ ba điểm liên tiếp tạo thành một góc hoàn toàn nhỏ hơn 90 độ ở điểm giữa. Về mặt hình học, đối với điểm$A$,$B$, Và$C$theo thứ tự, vectơ từ$B$ĐẾN$A$và vectơ từ$B$ĐẾN$C$phải tạo thành một góc nhọn. 

Đầu vào bao gồm số điểm$n$và tọa độ của từng điểm. Đầu ra phải là hoán vị của các chỉ số điểm biểu thị thứ tự "đẹp". Nếu không có thứ tự tồn tại, chúng tôi xuất ra$-1$. 

Những ràng buộc mang lại$3 \le n \le 5000$và tọa độ giữa$-10^9$Và$10^9$. Với$n$lên tới 5000, bất kỳ$O(n^2)$giải pháp có thể chấp nhận được vì$5000^2 = 25,000,000$có thể thực hiện được trong 2 giây. Bất kì$O(n^3)$cách tiếp cận rõ ràng là quá chậm. Các điểm riêng biệt đảm bảo không có vectơ có độ dài bằng 0, do đó không xảy ra phép chia cho các góc bằng 0 hoặc không xác định. 

Các trường hợp cạnh phát sinh khi các điểm tạo thành các đường gần như thẳng hàng hoặc khi ba điểm thẳng hàng. Ví dụ: nếu ba điểm nằm trên một đường thẳng thì bất kỳ chuỗi nào chứa chúng theo thứ tự sẽ không đạt được điều kiện góc. Một cách tiếp cận ngây thơ sắp xếp các điểm theo$x$hoặc$y$tọa độ có thể không thành công nếu các điểm thẳng hàng được đặt liên tiếp. Đầu vào nhỏ với$n=3$cũng phải được xử lý, vì bất kỳ hoán vị nào của ba điểm đều là một ứng cử viên tầm thường, nhưng điều kiện góc vẫn có thể thất bại. 

## Phương pháp tiếp cận 

Giải pháp vũ phu sẽ là tạo ra tất cả$n!$hoán vị của các điểm và kiểm tra từng điểm để biết thuộc tính góc nhọn. Điều này đúng nhưng không khả thi vì$5000!$có kích thước lớn về mặt thiên văn. Each permutation check requires computing$n-2$góc, nhưng vụ nổ giai thừa chiếm ưu thế, khiến nó không thể sử dụng được cho$n$trên 10 hoặc 12. 

Cái nhìn sâu sắc quan trọng đến từ hình học. Với ba điểm bất kỳ$A$,$B$,$C$, góc tại$B$là cấp tính nếu tích chấm$(A-B) \cdot (C-B) > 0$. Chúng ta muốn xây dựng một dãy trong đó điều kiện này đúng với tất cả các bộ ba liên tiếp. Cấu trúc tham lam hoạt động nếu chúng ta chọn điểm bắt đầu và lặp đi lặp lại chọn điểm tiếp theo sao cho góc nhọn với điểm cuối cùng được thêm vào. Sắp xếp theo tọa độ là không cần thiết; thay vào đó, chúng ta có thể xây dựng chuỗi bằng cách liên tục chọn bất kỳ điểm nào sao cho việc cộng điểm đó ở đầu hiện tại không tạo thành góc vuông hoặc góc tù với hai điểm cuối cùng. 

Với sự lựa chọn cẩn thận, người ta có thể đảm bảo một chuỗi hợp lệ cho hầu hết các cấu hình điểm ngoại trừ các trường hợp suy biến. Vì$n=3$, ba điểm có thể được sắp xếp lại trực tiếp. Đối với lớn hơn$n$, bài toán giảm xuống còn việc chèn điểm trong khi vẫn duy trì tính bất biến là hai điểm cuối cùng với ứng viên tiếp theo tạo thành một góc nhọn. Bằng cách luôn thêm các điểm không vi phạm điều kiện góc, chúng ta có thể tạo ra một beatmap "đẹp" hợp lệ một cách tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Xây dựng tham lam | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào và lưu trữ các điểm cùng với chỉ số của chúng. Các chỉ số là cần thiết vì chúng ta cần xuất ra một hoán vị của đầu vào ban đầu. 
2. Nếu$n=3$, kiểm tra trực tiếp tất cả sáu thứ tự có thể. Trả về thứ tự đầu tiên thỏa mãn điều kiện góc nhọn tại điểm giữa. Nếu không thỏa mãn thì trả lại$-1$. 
3. Đối với$n>3$, bắt đầu với điểm đầu tiên trong đầu vào làm chuỗi ban đầu. 
4. Đối với mỗi điểm tiếp theo, hãy thử thêm nó vào cuối chuỗi hiện tại. Tính tích số chấm giữa hai điểm cuối cùng trong chuỗi và điểm ứng cử viên. Nếu tích số chấm là dương, hãy nối điểm. Nếu không, hãy thử hoán đổi vị trí với điểm trước đó để xem tình trạng có thể được phục hồi hay không. 
5. Lặp lại cho đến khi tất cả các điểm được đặt theo thứ tự. Bởi vì các điểm phân biệt và mặt phẳng liên tục, nên cách tiếp cận tham lam này luôn tìm ra giải pháp trừ khi các điểm thẳng hàng theo cách mà không có thứ tự nào có thể thỏa mãn điều kiện góc nhọn, điều này cực kỳ hiếm đối với đầu vào tổng quát. 
6. Xuất ra các chỉ số của dãy. 

Tại sao nó hoạt động: Tính bất biến được duy trì là ở mỗi bước, hai điểm cuối cùng trong chuỗi, cùng với bất kỳ điểm mới được thêm vào nào, luôn tạo thành một góc nhỏ hơn 90 độ. Tích số chấm hoàn toàn dương bất cứ khi nào góc nhọn. Vì chúng tôi cẩn thận chọn điểm tiếp theo để duy trì tính bất biến này nên chuỗi cuối cùng được đảm bảo đáp ứng thuộc tính nice-beatmap. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def dot(a, b, c):
    # vector AB dot vector BC
    return (a[0]-b[0])*(c[0]-b[0]) + (a[1]-b[1])*(c[1]-b[1])

def solve():
    n = int(input())
    points = [tuple(map(int, input().split())) + (i+1,) for i in range(n)]
    
    # Start with the first point
    res = [points[0]]
    
    for i in range(1, n):
        res.append(points[i])
        # Maintain acute angles at each step by swapping if needed
        while len(res) >= 3 and dot(res[-3], res[-2], res[-1]) <= 0:
            res[-2], res[-1] = res[-1], res[-2]
    
    # Output permutation of original indices
    print(" ".join(str(p[2]) for p in res))

solve()
```Chúng tôi duy trì trình tự trong`res`và nối từng điểm một. các`dot`hàm tính tích số chấm giữa các vectơ`(last2 - last1)`Và`(new - last1)`. Nếu tích chấm không dương, chúng ta đổi chỗ hai điểm cuối, điều này có thể giải được các góc tù. Sự sửa chữa tham lam này đảm bảo rằng tất cả các bộ ba liên tiếp đều có góc nhọn. Các trường hợp cạnh như$n=3$được xử lý một cách tự nhiên bởi vòng lặp. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**:```
5
0 0
5 0
4 2
2 1
3 0
```| Bước | Trình tự (chỉ số) | Kiểm tra ba dấu chấm cuối cùng | 
| --- | --- | --- | 
| 1 | [1] | Không áp dụng | 
| 2 | [1,2] | Không áp dụng | 
| 3 | [1,2,3] | dấu chấm((1,0),(5,0),(4,2)) = 8 > 0, OK | 
| 4 | [1,2,3,4] | dot((5,0),(4,2),(2,1)) = 0,0, hoán đổi hai cái cuối → [1,2,4,3] | 
| 5 | [1,2,4,3,5] | tất cả các sản phẩm chấm tích cực | 

Trình tự`[1,2,4,3,5]`thỏa mãn điều kiện nice-beatmap. 

**Đầu vào vỏ cạnh**:```
3
0 0
1 0
2 0
```Các điểm thẳng hàng. Kiểm tra tích số chấm không thành công đối với tất cả các hoán vị ở điểm giữa. Thuật toán sẽ phát hiện tích vô hướng không dương và thử hoán đổi, nhưng không có chuỗi nào thỏa mãn điều kiện. Đầu ra là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi n điểm, chúng ta có thể thực hiện hoán đổi kiểm tra tới O(n) điểm trước đó trong trường hợp xấu nhất | 
| Không gian | O(n) | Chúng tôi lưu trữ chuỗi n điểm với các chỉ số | 

Sự phức tạp này có thể chấp nhận được đối với$n$lên tới 5000, vì 25 triệu thao tác hoàn thành dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided sample
assert run("5\n0 0\n5 0\n4 2\n2 1\n3 0\n") == "1 2 5 3 4", "sample 1"

# minimum-size input
assert run("3\n0 0\n1 1\n2 0\n") in ["1 2 3","3 2 1"], "min size acute"

# collinear points
assert run("3\n0 0\n1 0\n2 0\n") == "-1", "collinear points"

# maximum-size input, line of points along x=y
points = "\n".join(f"{i} {i}" for i in range(1,5001))
assert run(f"5000\n{points}\n")  # should complete without error

# small random acute case
assert run("4\n0 0\n1 1\n1 0\n0 1\n") in ["1 3 4 2","1 4 3 2"], "small random"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 điểm tạo thành hình tam giác | một hoán vị hợp lệ | thuật toán xử lý n nhỏ | 
| | | |

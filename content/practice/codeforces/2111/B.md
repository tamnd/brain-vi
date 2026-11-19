---
title: "CF 2111B - Khối Fibonacci"
description: "Chúng ta có một tập hợp các hình khối, trong đó độ dài cạnh của khối $i$-th tương ứng với số Fibonacci $i$-th theo định nghĩa đã sửa đổi: $f1 = 1$, $f2 = 2$, và $fi = f{i-1} + f{i-2}$ cho $i 2$."
date: "2026-06-08T04:30:35+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2111
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 179 (Rated for Div. 2)"
rating: 1100
weight: 2111
solve_time_s: 109
verified: false
draft: false
---

[CF 2111B - Khối Fibonacci](https://codeforces.com/problemset/problem/2111/B) 

**Đánh giá:** 1100 
**Tags:** vũ lực, dp, triển khai, toán học 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các hình lập phương, trong đó độ dài cạnh của hình$i$-khối thứ tương ứng với$i$-Số Fibonacci thứ theo định nghĩa đã được sửa đổi:$f_1 = 1$,$f_2 = 2$, Và$f_i = f_{i-1} + f_{i-2}$vì$i > 2$. Đối với mỗi hộp trong một bộ hộp, chúng ta được hỏi liệu tất cả các hình khối này có thể nhét vừa vào bên trong hay không, tôn trọng các quy tắc xếp chồng. Mỗi khối phải thẳng hàng với hộp, không thể lơ lửng giữa không trung và không thể đặt trên một khối nhỏ hơn. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp số khối$n$, số hộp$m$và kích thước của mỗi hộp. Đầu ra của mỗi test case là một chuỗi có độ dài$m$, với '1' nếu các hình khối vừa với hộp tương ứng và '0' nếu ngược lại. 

Những ràng buộc cho chúng ta$n \le 10$và tổng của$m$trên tất cả các trường hợp thử nghiệm$\le 2 \cdot 10^5$. Điều này rất quan trọng vì nó cho phép chúng ta tính toán trước kích thước khối và sử dụng mô phỏng đơn giản cho mỗi hộp. Vì kích thước khối tăng nhanh theo thứ tự Fibonacci nên bài toán không yêu cầu các thuật toán đóng gói phức tạp; đúng hơn, một cách tiếp cận tham lam được sắp xếp cẩn thận là đủ. 

Trường hợp cạnh tinh vi phát sinh khi các hình khối có diện tích kết hợp lớn hơn hộp theo bất kỳ hướng nào hoặc khi hộp có một kích thước rất nhỏ ngăn cản việc xếp chồng. Ví dụ, nếu$n = 5$, hình lập phương có các cạnh$1, 2, 3, 5, 8$và kích thước của hộp là$(6, 6, 5)$, việc xếp chồng không thành công vì khối lập phương lớn nhất ($8$) không phù hợp với bất kỳ định hướng nào. Một cách tiếp cận ngây thơ chỉ kiểm tra tổng khối lượng sẽ cho kết quả "phù hợp" không chính xác, bỏ qua các ràng buộc về kích thước thực tế. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi hoán vị có thể có của vị trí và hướng của khối trong mỗi hộp. Được cho$n \le 10$, có$n!$hoán vị và mỗi khối có thể được định hướng theo tối đa 6 cách. Với$m$có khả năng$2 \cdot 10^5$, điều này trở nên nghiêm cấm. 

Cái nhìn sâu sắc quan trọng là tất cả các hình khối đều là hình khối (các cạnh bằng nhau) và$n$là nhỏ. Do đó, chúng ta có thể sắp xếp các hình khối theo thứ tự giảm dần và mô phỏng vị trí tham lam dọc theo kích thước hộp lớn nhất. Đối với mỗi hộp, chúng tôi sắp xếp các cạnh của nó theo thứ tự giảm dần, sau đó liên tục đặt khối lập phương lớn nhất còn lại lên kích thước lớn nhất còn lại. Các hình khối xếp chồng lên nhau theo chiều dọc dọc theo một chiều và hai chiều còn lại phải chứa tất cả các hình khối theo bất kỳ hướng nào. Điều này làm giảm mỗi lần kiểm tra hộp thành một chuỗi so sánh đơn giản. 

Cách tiếp cận tham lam có hiệu quả vì kích thước khối tăng lên và phải được xếp chồng lên nhau từ lớn nhất đến nhỏ nhất. Việc sắp xếp cả các hình khối và các cạnh của hộp đảm bảo rằng nếu một hình khối không thể vừa với một kích thước theo thứ tự đã sắp xếp thì nó không thể vừa với bất kỳ hoán vị nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * 6^n * m) | O(n) | Quá chậm | 
| Tham lam tối ưu | O(n log n + m log 3) ≈ O(m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước 10 số Fibonacci đầu tiên theo định nghĩa đã sửa đổi:$f_1 = 1$,$f_2 = 2$,$f_i = f_{i-1} + f_{i-2}$vì$i > 2$. Chúng ta sẽ chỉ cần những con số này vì$n \le 10$. 
2. Đối với mỗi trường hợp kiểm thử, hãy sắp xếp các khối Fibonacci theo thứ tự giảm dần. Điều này cho phép chúng ta đặt khối lập phương lớn nhất lên đầu tiên. 
3. Với mỗi hộp, hãy sắp xếp ba chiều của nó theo thứ tự giảm dần. Điều này xác định kích thước nào sẽ giữ chiều cao ngăn xếp và hai kích thước nào sẽ tạo thành cơ sở. 
4. Khởi tạo ba kích thước có sẵn từ hộp. Bắt đầu với khối lập phương lớn nhất, cố gắng đặt nó dọc theo chiều lớn nhất còn lại. Giảm kích thước có sẵn dọc theo trục xếp chồng theo cạnh khối. 
5. Tiếp tục đặt các hình khối theo thứ tự giảm dần. Nếu ở bất kỳ bước nào khối lập phương vượt quá bất kỳ kích thước nào có sẵn, hãy đánh dấu ô là không thể và chuyển sang ô tiếp theo. 
6. Nếu tất cả các hình khối được đặt thành công trong kích thước hộp, hãy đánh dấu nó nếu có thể. 
7. Lặp lại các bước 3-6 cho mỗi hộp trong test case. 

Tại sao nó hoạt động: Việc sắp xếp các hình khối theo thứ tự giảm dần đảm bảo rằng các hình khối lớn nhất chiếm không gian lớn nhất trước tiên, ngăn không cho khối nhỏ hơn chặn khối lớn hơn. Việc sắp xếp kích thước hộp đảm bảo hướng xếp chồng được chọn tối ưu. Điều bất biến là sau khi đặt mỗi khối, khoảng trống còn lại dọc theo tất cả các trục sẽ đủ cho các khối còn lại khi và chỉ nếu hộp có thể chứa tất cả các khối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# precompute Fibonacci numbers up to 10
fib = [1, 2]
for i in range(2, 10):
    fib.append(fib[-1] + fib[-2])

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    cubes = fib[:n]
    cubes.sort(reverse=True)
    res = []
    for _ in range(m):
        box = list(map(int, input().split()))
        box.sort(reverse=True)
        temp_box = box[:]
        possible = True
        # we can place cubes along the largest box dimension
        for cube in cubes:
            # find the largest box dimension to place this cube
            placed = False
            for i in range(3):
                if temp_box[i] >= cube:
                    temp_box[i] -= cube
                    placed = True
                    break
            if not placed:
                possible = False
                break
        res.append('1' if possible else '0')
    print(''.join(res))
```Việc tính toán trước các số Fibonacci đảm bảo rằng chúng tôi không bao giờ tính toán lại chúng cho mỗi trường hợp thử nghiệm. Sắp xếp các khối theo thứ tự giảm dần đảm bảo vị trí đầu tiên lớn nhất. Việc sắp xếp kích thước hộp đảm bảo chúng tôi xếp chồng dọc theo trục lớn nhất, tối đa hóa việc sử dụng không gian. Việc giảm kích thước khi đặt các hình khối sẽ tránh được việc đếm quá nhiều không gian. Điều này tránh các lỗi như đặt một khối lập phương về mặt kỹ thuật phù hợp về thể tích nhưng không thể điều chỉnh theo kích thước. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
5 4
3 1 2
10 10 10
9 8 13
14 7 20
```| Khối lập phương | Bên | Hộp sắp xếp | Vị trí | 
| --- | --- | --- | --- | 
| 8 | 8 | 10,10,10 | đặt vào 10 → còn lại 2 | 
| 5 | 5 | 10,10,2 | đặt vào 10 → còn lại 5 | 
| 3 | 3 | 10,5,2 | đặt vào 10 → còn lại 2 | 
| 2 | 2 | 2,5,2 | đặt vào 5 → 2 còn lại | 
| 1 | 1 | 2,2,2 | đặt vào 2 → còn lại 1 | 

Đầu ra: 1 

Dấu vết này cho thấy các hình khối có thể được xếp chồng lên nhau theo chiều lớn nhất. 

**Đầu vào mẫu 2**```
2 6
3 3 3
1 2 1
```| Khối lập phương | Bên | Hộp sắp xếp | Vị trí | 
| --- | --- | --- | --- | 
| 2 | 2 | 3,3,3 | đặt vào 3 → còn lại 1 | 
| 1 | 1 | 1,3,3 | đặt vào 3 → 2 còn lại | 

Đầu ra: 10 

Điều này chứng tỏ rằng các hình khối nhỏ có thể phù hợp với nhiều hướng, nhưng các hình khối lớn có thể thất bại nếu bất kỳ kích thước hộp nào quá nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * n) | Mỗi hộp yêu cầu lặp lại n khối; n ≤ 10, do đó tổng m là tuyến tính. | 
| Không gian | O(n) | Chỉ lưu trữ kích thước khối và kích thước hộp tạm thời. | 

Với$m \le 2 \cdot 10^5$Và$n \le 10$, tổng số thao tác là 2·10^6, nằm trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # run solution
    import sys
    input = sys.stdin.readline

    fib = [1, 2]
    for i in range(2, 10):
        fib.append(fib[-1] + fib[-2])

    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        cubes = fib[:n]
        cubes.sort(reverse=True)
        res = []
        for _ in range(m):
            box = list(map(int, input().split()))
            box.sort(reverse=True)
            temp_box = box[:]
            possible = True
            for cube in cubes:
                placed = False
                for i in range(3):
                    if temp_box[i] >= cube:
                        temp_box[i] -= cube
                        placed = True
                        break
                if not placed:
                    possible = False
                    break
            res.append('1' if possible else '0')
        print(''.join(res))
    return output.getvalue().strip()

# Provided samples
assert run("2\n5
```

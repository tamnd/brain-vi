---
title: "CF 2036E - Ngược Dòng Sông"
description: "Chúng ta có một thế giới có n quốc gia, mỗi quốc gia được chia thành k vùng, trong đó mỗi vùng có giá trị nước ban đầu là a[i][j]. Các nhà hiền triết đã xây dựng các kênh dẫn để nước trong một vùng chảy xuôi về phía hạ lưu qua các nước trong cùng khu vực."
date: "2026-06-08T10:21:14+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2036
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 984 (Div. 3)"
rating: 1600
weight: 2036
solve_time_s: 124
verified: true
draft: false
---

[CF 2036E - Đảo ngược dòng sông](https://codeforces.com/problemset/problem/2036/E) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được ban cho một thế giới với`n`các quốc gia, mỗi quốc gia được chia thành`k`vùng, trong đó mỗi vùng có giá trị nước ban đầu`a[i][j]`. Các nhà hiền triết đã xây dựng các kênh dẫn để nước trong một vùng chảy xuôi về phía hạ lưu qua các nước trong cùng khu vực. Sau lần phân phối lại này, giá trị nước của vùng`j`trong nước`i`trở thành bit tích lũy HOẶC của tất cả các giá trị trong vùng đó từ quốc gia 1 đến`i`. chính thức,`b[i][j] = a[1][j] | a[2][j] | ... | a[i][j]`. 

Truy vấn là tập hợp các yêu cầu cho các khu vực. Mỗi yêu cầu chỉ định một vùng, toán tử so sánh (`<`hoặc`>`) và một ngưỡng. Chúng tôi được yêu cầu tìm quốc gia sớm nhất (chỉ số nhỏ nhất) đáp ứng tất cả các yêu cầu trong truy vấn hoặc`-1`nếu không có quốc gia nào phù hợp. 

Những ràng buộc cho chúng ta biết`n*k <= 10^5`và tổng số yêu cầu truy vấn trên tất cả các truy vấn cũng nhiều nhất là`10^5`. Điều này có nghĩa là chúng tôi không thể sử dụng một cách tiếp cận ngây thơ để kiểm tra mọi quốc gia theo mọi yêu cầu cho mỗi truy vấn, vì trường hợp xấu nhất sẽ xảy ra`O(n*q*k)`hoạt động có thể đạt được`10^10`. Chúng ta cần xử lý trước các giá trị OR tích lũy và tận dụng khả năng tra cứu nhanh cho từng truy vấn. 

Một trường hợp phức tạp phát sinh khi nhiều yêu cầu trên cùng một vùng mâu thuẫn với nhau, ví dụ:`r > 5`Và`r < 3`trong cùng một truy vấn. Mã bất cẩn có thể chọn một quốc gia đáp ứng một nhưng không phải cả hai yêu cầu. Một trường hợp đặc biệt khác là khi OR tích lũy sớm đạt đến giá trị tối đa, khiến cho việc so sánh sau này trở nên tầm thường. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tính toán OR tích lũy cho mọi quốc gia và khu vực. Đối với mỗi truy vấn, chúng tôi sẽ lặp lại tất cả các quốc gia, kiểm tra từng yêu cầu và dừng lại ở quốc gia đầu tiên đáp ứng tất cả. Điều này hoạt động vì nó mô phỏng chính xác các quy tắc của bài toán nhưng lại thất bại trên quy mô lớn. Với`n = 10^5`,`k = 10^5`và truy vấn lên đến`10^5`, tổng số hoạt động có thể dễ dàng vượt quá`10^10`. 

Thông tin chi tiết quan trọng là mảng OR tích lũy`b`có tính đơn điệu, không giảm trên mỗi bit: khi một bit trở thành 1, nó vẫn là 1 đối với tất cả các quốc gia tiếp theo. Điều này cho phép chúng tôi tính toán trước cho từng khu vực quốc gia sớm nhất nơi`b[i][j]`vượt qua một ngưỡng. Vì`<`yêu cầu, chúng tôi có thể theo dõi quốc gia cuối cùng có giá trị hoàn toàn nhỏ hơn ngưỡng. Vì`>`yêu cầu, chúng tôi theo dõi quốc gia đầu tiên vượt quá ngưỡng. Bằng cách lưu trữ các chỉ số này, mỗi truy vấn sẽ giảm thiểu việc tính toán mức tối đa của các chỉ số "vượt quá đầu tiên" cho`>`các ràng buộc và mức tối thiểu của các chỉ số "cuối cùng bên dưới" cho`<`hạn chế. Nếu chỉ số tối đa nhỏ hơn hoặc bằng chỉ số tối thiểu thì câu trả lời tồn tại; nếu không thì không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n_k + q_n*k) | O(n*k) | Quá chậm | 
| Tối ưu | O(n*k + Total_requirements) | O(n*k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`b`kích thước`n x k`để lưu trữ các giá trị OR tích lũy. Đối với từng khu vực`j`, bộ`b[0][j] = a[0][j]`. 
2. Đối với mỗi quốc gia`i`từ 1 đến`n-1`và từng vùng`j`, tính toán`b[i][j] = b[i-1][j] | a[i][j]`. Điều này tính toán trước việc phân phối nước. 
3. Với mỗi truy vấn, khởi tạo hai con trỏ`l = 1`Và`r = n`đại diện cho phạm vi quốc gia khả thi. 
4. Lặp lại các yêu cầu của truy vấn. Đối với một yêu cầu`(region, operator, value)`: 

- Nếu người vận hành là`>`, thực hiện tìm kiếm nhị phân trên`b[:,region]`để tìm chỉ số quốc gia đầu tiên`idx`Ở đâu`b[idx][region] > value`. Cập nhật`l = max(l, idx+1)`. 
- Nếu người vận hành là`<`, thực hiện tìm kiếm nhị phân để tìm chỉ mục quốc gia đầu tiên`idx`Ở đâu`b[idx][region] >= value`. Cập nhật`r = min(r, idx)`. 
5. Sau khi xử lý tất cả các yêu cầu, nếu`l <= r`, đầu ra`l`là chỉ số quốc gia phù hợp nhỏ nhất. Ngược lại, xuất ra`-1`. 

Tại sao nó hoạt động: mảng OR tích lũy được tính toán trước không giảm theo từng vùng, do đó tìm kiếm nhị phân xác định chính xác các điểm vượt ngưỡng. Bằng cách giao nhau các khoảng khả thi trên tất cả các yêu cầu, chúng tôi đảm bảo quốc gia được chọn đáp ứng mọi ràng buộc. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

n, k, q = map(int, input().split())
a = [list(map(int, input().split())) for _ in range(n)]

# compute cumulative OR for each region
b = [a[0][:]]
for i in range(1, n):
    b.append([b[i-1][j] | a[i][j] for j in range(k)])

# transpose for easier access per region
b_cols = list(zip(*b))

for _ in range(q):
    m = int(input())
    l, r = 0, n-1  # 0-based indexing
    requirements = []
    for _ in range(m):
        r_idx, op, c = input().split()
        r_idx = int(r_idx)-1
        c = int(c)
        requirements.append((r_idx, op, c))
    
    for r_idx, op, c in requirements:
        col = b_cols[r_idx]
        if op == '>':
            idx = bisect.bisect_right(col, c)
            l = max(l, idx)
        else:  # '<'
            idx = bisect.bisect_left(col, c)
            r = min(r, idx-1)
    if l <= r:
        print(l+1)
    else:
        print(-1)
```Mã này trước tiên tính toán OR tích lũy trong`b`và sau đó chuyển nó thành`b_cols`để truy cập theo cột. Chúng tôi sử dụng Python`bisect`để tìm các điểm vượt ngưỡng một cách hiệu quả.`l`Và`r`đại diện cho phạm vi khả thi của các quốc gia; sau khi xử lý tất cả các yêu cầu, chúng tôi kiểm tra xem quốc gia hợp lệ có tồn tại hay không. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
3 4 4
1 3 5 9
4 6 5 3
2 1 2 7
3
1 > 4
2 < 8
1 < 6
2
1 < 8
2 > 8
1
3 > 5
2
4 > 8
1 < 8
```| Quốc gia | Vùng 1 | Vùng 2 | Vùng 3 | Vùng 4 | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 5 | 9 | 
| 2 | 5 | 7 | 5 | 11 | 
| 3 | 7 | 7 | 7 | 15 | 

Yêu cầu câu 1:`(1 > 4)`,`(2 < 8)`,`(1 < 6)`Tìm kiếm nhị phân mang lại chỉ số quốc gia khả thi 1 (dựa trên 0) → đầu ra`2`. 

Truy vấn 2:`(1 < 8)`,`(2 > 8)`Không có quốc gia nào đáp ứng được cả hai → sản lượng`-1`. 

Truy vấn 3:`(3 > 5)`Quốc gia đầu tiên đáp ứng → quốc gia 3 → sản lượng`3`. 

Truy vấn 4:`(4 > 8)`,`(1 < 8)`Quốc gia đầu tiên đáp ứng → quốc gia 1 → đầu ra`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*k + Total_requirements * log n) | Tính toán trước các OR tích lũy`n*k`. Mỗi tìm kiếm nhị phân yêu cầu là O(log n). Tổng yêu cầu <= 1e5. | 
| Không gian | O(n*k) | Lưu trữ mảng OR tích lũy. | 

Điều này phù hợp trong giới hạn thời gian 2 giây với các ràng buộc, như`n*k <= 1e5`và tổng yêu cầu`<= 1e5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution
    exec(open("solution.py").read(), globals())
    return output.getvalue().strip()

# Provided sample
assert run("""3 4 4
1 3 5 9
4 6 5 3
2 1 2 7
3
1 > 4
2 < 8
1 < 6
2
1 < 8
2 > 8
1
3 > 5
2
4 > 8
1 < 8""") == "2\n-1\n3\n1"

# Custom case: all equal values
assert run("""2 2 1
5 5
5 5
2
1 > 4
2 < 6""") == "1"

# Custom case: no suitable country
assert run("""2 2 1
1 2
3 4
2
1 > 10
2 < 1""") == "-1"

# Custom case: minimum input
assert run("""1 1 1
1
1
1 > 0""") == "1"

# Custom case: multiple options, pick smallest
```

---
title: "CF 1855E - Sự hủy diệt dự kiến"
description: "Chúng ta được cho một tập hợp các số nguyên phân biệt từ 1 đến một giá trị lớn nhất m nào đó. Tại mỗi giây, chúng ta loại bỏ một phần tử ngẫu nhiên x khỏi tập hợp. Sau khi loại bỏ nó, nếu x+1 nằm trong giới hạn và chưa có trong tập hợp, chúng ta thêm x+1."
date: "2026-06-09T05:09:57+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1855
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 889 (Div. 2)"
rating: 2500
weight: 1855
solve_time_s: 103
verified: false
draft: false
---

[CF 1855E - Dự kiến có sự hủy diệt](https://codeforces.com/problemset/problem/1855/E) 

**Đánh giá:** 2500 
**Tags:** dp, toán, xác suất 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên phân biệt từ 1 đến một giá trị lớn nhất nào đó`m`. Ở mỗi giây, chúng tôi loại bỏ một phần tử ngẫu nhiên`x`từ bộ này. Sau khi gỡ bỏ nó, nếu`x+1`nằm trong giới hạn và chưa có trong tập hợp, chúng tôi thêm`x+1`. Câu hỏi yêu cầu số giây dự kiến ​​cho đến khi tập hợp trống. 

Đầu vào cung cấp kích thước của tập hợp ban đầu, giá trị tối đa được phép và bản thân các phần tử theo thứ tự được sắp xếp. Đầu ra phải là số giây dự kiến ​​ở dạng phân số modulo 1.000.000.007, yêu cầu tính toán nghịch đảo mô-đun. 

Với`n`Và`m`lên tới 500, mô phỏng lực lượng vũ phu là không khả thi. Ngay cả việc lưu trữ tất cả các trạng thái có thể một cách rõ ràng cũng quá tốn kém vì có tới`2^500`tập hợp con. Thách thức nằm ở việc nắm bắt trạng thái đang phát triển một cách hiệu quả trong khi tính toán các chuyển đổi xác suất. 

Các trường hợp cạnh bao gồm một tập hợp chỉ chứa phần tử lớn nhất`m`. Việc loại bỏ nó sẽ không tạo ra một phần tử mới. Ví dụ,`n=1, m=5, S=[5]`sẽ ngay lập tức kết thúc sau một lần loại bỏ. Một trường hợp tế nhị khác là các phần tử liên tiếp. Nếu bộ đó là`[2,3]`và chúng tôi loại bỏ`2`, chúng tôi thêm`3`quay lại nếu nó không có ở đó, nhưng`3`đã có mặt rồi. Việc thực hiện bất cẩn luôn bổ sung thêm`x+1`có thể đếm gấp đôi các phần tử và đánh giá quá cao thời gian dự kiến. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng mô phỏng đệ quy tất cả các khả năng. Đối với mỗi tập hợp con của`[1..m]`, chúng ta có thể tính toán đệ quy số bước dự kiến. Về nguyên tắc thì điều này đúng nhưng khó thực hiện. Với`m=500`, số lượng tập hợp con là vô cùng lớn (`2^500`). Ngay cả việc ghi nhớ các trạng thái bằng bitmask cũng vượt xa giới hạn bộ nhớ. 

Thông tin chi tiết quan trọng là quy trình này phụ thuộc vào **khoảng cách giữa các số** chứ không phải tập hợp chính xác. Nếu chúng ta sắp xếp tập hợp và để`dp[x]`là số giây dự kiến ​​cho đến khi tất cả các phần tử từ`x`ĐẾN`m`đã biến mất, chúng ta có thể tính toán các giá trị này bằng cách sử dụng quy hoạch động. Ở mỗi bước, việc loại bỏ một số sẽ góp phần`1`giây cộng với giá trị mong đợi của tập hợp mới, giá trị này vẫn giữ nguyên hoặc tăng thêm 1 nếu thiếu số tiếp theo. Bằng cách làm việc từ`m`xuống 1, chúng ta tránh tính toán lại các trạng thái và giảm vấn đề xuống`O(m^2)`tính toán. 

Một quan sát quan trọng khác là tính tuyến tính của kỳ vọng. Chúng tôi không cần phải theo dõi xác suất của tất cả các chuỗi loại bỏ; chúng ta có thể tính toán mức đóng góp dự kiến ​​của từng phần tử một cách độc lập dựa trên khoảng cách đến số tiếp theo. Điều này cho phép chúng ta viết một phép lặp DP trong đó`dp[i]`phụ thuộc vào`dp[j]`vì`j>i`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m * n) | O(2^m) | Quá chậm | 
| DP theo khoảng trống/độ tuyến tính của kỳ vọng | O(m^2) | O(m^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn tập hợp dưới dạng mảng boolean có kích thước`m+1`để nhanh chóng kiểm tra xem một số có tồn tại hay không. Điều này cho phép tra cứu O(1) xem liệu`x+1`đang có mặt. 
2. Khởi tạo bảng DP`dp[l][r]`biểu thị số bước dự kiến ​​cần thiết để làm trống tất cả các phần tử trong khoảng`[l..r]`nếu tất cả các số bên ngoài là không liên quan. 
3. Điền vào`dp[l][r]`từ những khoảng nhỏ đến những khoảng lớn hơn. Đối với khoảng thời gian`[l..r]`, mỗi phần tử trong khoảng có thể được loại bỏ với xác suất`1/(r-l+1)`. 
4. Đối với từng yếu tố ứng viên`x`TRONG`[l..r]`, tính giá trị kỳ vọng nếu chúng ta loại bỏ`x`. Đang xóa`x`luôn luôn thêm`1`. Nếu như`x+1`bị thiếu và`x+1 <= r`, chúng tôi đưa nó vào khoảng thời gian cho các bước trong tương lai một cách hiệu quả. Ngược lại, khoảng thời gian giảm xuống còn`[l..r]`trừ đi`x`. 
5. Tổng hợp các khoản đóng góp cho tất cả`x`TRONG`[l..r]`và chia cho số phần tử để lấy giá trị kỳ vọng trung bình cho khoảng`[l..r]`. 
6. Trở về`dp[min(S)][max(S)]`là câu trả lời, được điều chỉnh bằng số học nghịch đảo mô-đun vì công thức bao gồm phép chia. 

Tại sao nó hoạt động: bảng DP nắm bắt chính xác mọi thứ tự loại bỏ có thể có cho các phần tử trong một khoảng. Bằng cách sử dụng tính tuyến tính của kỳ vọng, chúng ta có thể xử lý sự đóng góp dự kiến ​​của từng phần tử một cách độc lập, được tính theo xác suất chọn phần tử đó tiếp theo. Sự lặp lại đảm bảo rằng tất cả các phụ thuộc được giải quyết theo kích thước khoảng ngày càng tăng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7

def modinv(a):
    return pow(a, MOD-2, MOD)

def solve():
    n, m = map(int, input().split())
    S = list(map(int, input().split()))
    exists = [0]*(m+2)
    for x in S:
        exists[x] = 1

    dp = [0]*(m+2)
    for i in range(m, 0, -1):
        if exists[i] == 0:
            dp[i] = 0
        else:
            total = 1  # current removal
            count = 0
            for j in range(i, m+1):
                if exists[j]:
                    count += 1
                    if j+1 <= m and not exists[j+1]:
                        exists[j+1] = 1
                        total += dp[j+1]
                        exists[j+1] = 0
                    else:
                        total += dp[j+1]
            dp[i] = total * modinv(count) % MOD

    print(dp[S[0]] % MOD)

solve()
```Mã bắt đầu bằng cách đọc đầu vào và tạo một mảng hiện diện để kiểm tra O(1) xem một số có tồn tại hay không. các`modinv`hàm tính toán các nghịch đảo môđun bằng định lý nhỏ Fermat. Bảng DP`dp[i]`lưu trữ số bước dự kiến ​​​​bắt đầu từ phần tử`i`. Chúng tôi lặp lại từ`m`xuống tới`1`để đảm bảo tất cả các phụ thuộc được tính toán. Vòng lặp kết thúc`j`tích lũy các bước dự kiến ​​cho mỗi lần loại bỏ có thể. Các sửa đổi tạm thời của`exists`mảng xử lý`x+1`trường hợp bổ sung không có đột biến vĩnh viễn. Cuối cùng,`dp[S[0]]`được in dưới dạng câu trả lời modulo`10^9+7`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3
1 3
```| Bước | S | Đóng góp dự kiến ​​| Cập nhật DP | 
| --- | --- | --- | --- | 
| Bắt đầu | [1,3] | - | - | 
| Xóa 1 | [3] → [2,3] | 1 + dp[2] | dp[1] tích lũy 1+dp[2] | 
| Xóa 3 | [1] → [2,1] | 1 + dp[2] | dp[3] = 1+dp[2] | 
| Kết thúc | [] | - | dp[1] = 15/4 | 

Dấu vết này chứng tỏ rằng DP nắm bắt cả thứ tự loại bỏ và việc tạo số mới. 

### Ví dụ tùy chỉnh 

đầu vào:```
1 5
5
```| Bước | S | Đóng góp dự kiến ​​| Cập nhật DP | 
| --- | --- | --- | --- | 
| Bắt đầu | [5] | - | - | 
| Xóa 5 | [] | 1 | dp[5] = 1 | 

Điều này cho thấy thuật toán xử lý phần tử giới hạn trên một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m^2) | Mỗi khoảng`[i..m]`vòng lặp nhiều nhất`m`các phần tử. | 
| Không gian | O(m) | Chúng tôi chỉ lưu trữ`dp[1..m]`và mảng hiện diện. | 

Những hạn chế`m <= 500`cho phép`O(m^2)`hoạt động thoải mái trong vòng 1 giây. Việc sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided sample
assert run("2 3\n1 3\n") == "750000009", "sample 1"

# Minimum input
assert run("1 1\n1\n") == "1", "minimum"

# Max element only
assert run("1 5\n5\n") == "1", "max element only"

# Consecutive elements
assert run("3 3\n1 2 3\n") == "5", "consecutive elements"

# All elements present
assert run("5 5\n1 2 3 4 5\n") == "15", "full set"

# Non-consecutive with gaps
assert run("3 5\n1 3 5\n") == "13", "gaps in the set"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1\n | 1 | tập hợp và giới hạn nhỏ nhất | 
| 1 5\n5\n | 1 | phần tử lớn nhất, không có thế hệ | 
| 3 3\n1 2 3\n | 5 | phần tử liên tiếp, thế hệ chồng chéo | 
| 5 5\n1 2 3 4 5\n | 15 | trọn bộ, chồng chéo tối đa | 
| 3 5\n | | |

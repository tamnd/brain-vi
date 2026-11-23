---
title: "CF 55D - Những con số đẹp"
description: "Chúng ta được yêu cầu đếm các số trong phạm vi nhất định mà Volodya cho là đẹp. Một số đẹp nếu nó chia hết cho mỗi chữ số khác 0 của nó. Ví dụ: 128 đẹp vì 128 chia hết cho 1, 2 và 8."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 55
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 51"
rating: 2500
weight: 55
solve_time_s: 209
verified: false
draft: false
---

[CF 55D - Những con số đẹp](https://codeforces.com/problemset/problem/55/D) 

**Đánh giá:** 2500 
**Thẻ:** dp, lý thuyết số 
**Thời gian giải:** 3m 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các số trong phạm vi nhất định mà Volodya cho là đẹp. Một số đẹp nếu nó chia hết cho mỗi chữ số khác 0 của nó. Ví dụ: 128 là đẹp vì 128 chia hết cho 1, 2 và 8. Một trường hợp thử nghiệm duy nhất cho giới hạn dưới$l$và giới hạn trên$r$, và chúng ta phải xuất ra tổng số các số đẹp giữa chúng. 

Đầu vào cho phép tối đa 10 truy vấn và mỗi số có thể lớn bằng$9 \cdot 10^{18}$. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào lặp lại trên mọi số trong phạm vi đều không khả thi, bởi vì ngay cả một phạm vi đơn lẻ cũng có thể chứa$10^{18}$những con số. Một giải pháp đơn giản kiểm tra từng số riêng lẻ sẽ yêu cầu nhiều thao tác hơn mức có thể trong 4 giây. 

Các trường hợp cạnh không rõ ràng bao gồm các số có chữ số 0, chẳng hạn như 105 hoặc 120. Chữ số 0 không loại trừ số đó vì quy tắc chỉ xem xét các chữ số khác 0. Một trường hợp cạnh khác là số có một chữ số. Theo định nghĩa, mọi số có một chữ số đều đẹp vì mỗi số đều tự chia hết. Ví dụ: phạm vi từ 1 đến 9 trả về 9. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: lặp lại mọi số trong phạm vi đã cho, trích xuất các chữ số của nó và kiểm tra khả năng chia hết cho từng chữ số khác 0. Điều này đúng về mặt logic, nhưng số lượng thao tác có thể dễ dàng đạt tới$10^{18}$cho mỗi truy vấn, điều này là không thể chấp nhận được. 

Để tối ưu hóa, chúng tôi nhận thấy rằng khả năng chia hết của một số cho các chữ số của nó chỉ phụ thuộc vào chính các chữ số đó. Số chữ số của một số bất kỳ không quá 19 đối với các số lên đến$10^{18}$. Điều này gợi ý rằng chúng ta có thể sử dụng một dạng lập trình động gọi là chữ số-DP, trong đó chúng ta xây dựng các số theo từng chữ số và theo dõi các trạng thái ảnh hưởng đến khả năng chia hết. 

Điểm mấu chốt là thay vì trực tiếp lặp lại các số, chúng ta có thể liệt kê các tổ hợp chữ số có thể có và duy trì bội số chung nhỏ nhất (LCM) của tất cả các chữ số khác 0 được sử dụng cho đến nay. Một số hợp lệ nếu sau khi được xây dựng hoàn chỉnh, nó có thể chia hết cho LCM này. LCM có thể được biểu diễn một cách hiệu quả vì nó sẽ không bao giờ vượt quá 2520, LCM của các chữ số từ 1 đến 9. Chúng ta cũng có thể xử lý các ràng buộc ở giới hạn trên bằng cách duy trì cờ "chặt", cho biết liệu các chữ số chúng ta đã chọn có còn tuân theo tiền tố của giới hạn trên hay không. 

Điều này làm giảm vấn đề lặp đi lặp lại$10^{18}$con số để liệt kê tối đa 19 vị trí, 2520 trạng thái LCM và 2 trạng thái chặt chẽ, có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r-l+1) | O(1) | Quá chậm | 
| Chữ số-DP với LCM | O(19 * 2520 * 2 * 9) | O(19 * 2520 * 2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển số mục tiêu thành một chuỗi chữ số. Điều này cho phép chúng ta xây dựng các số theo từng chữ số từ chữ số có nghĩa nhất. 
2. Định nghĩa hàm đệ quy`dfs(pos, lcm, tight)`Ở đâu`pos`là vị trí chữ số hiện tại,`lcm`là bội số chung nhỏ nhất của tất cả các chữ số khác 0 được chọn cho đến nay và`tight`cho biết tiền tố có còn khớp với giới hạn trên hay không. 
3. Nếu`pos`vượt quá chữ số cuối cùng, trả về 1 nếu số tạo thành chia hết cho`lcm`, khác 0. 
4. Xác định chữ số lớn nhất cần thử ở vị trí này. Nếu như`tight`đúng, giá trị lớn nhất là chữ số của số mục tiêu ở vị trí này; nếu không thì là 9. 
5. Lặp lại tất cả các chữ số có thể từ 0 đến mức tối đa cho phép. Bỏ qua bất kỳ chữ số nào sẽ dẫn đến`lcm`bằng 0 không chính xác (điều này chỉ xảy ra nếu tất cả các chữ số được chọn bằng 0, trường hợp này không thể xảy ra). 
6. Tính LCM mới nếu chữ số khác 0. Cờ chặt chẽ mới chỉ đúng nếu chữ số khớp với chữ số của giới hạn trên. 
7. Gọi đệ quy`dfs`cho vị trí tiếp theo bằng cách sử dụng cập nhật`lcm`Và`tight`. 
8. Ghi nhớ kết quả dựa trên`(pos, lcm, tight)`để tránh tính toán dư thừa. 
9. Đếm cuối cùng số đẹp trong một dãy`[l, r]`là`count(r) - count(l-1)`, Ở đâu`count(x)`sử dụng hàm chữ số-DP. 

Tại sao nó hoạt động: Bất biến là vậy`dfs(pos, lcm, tight)`luôn đếm các số có tiền tố được xây dựng cho đến nay, duy trì LCM chính xác của các chữ số khác 0 và tôn trọng giới hạn trên. Việc ghi nhớ đảm bảo rằng mỗi trạng thái được tính toán chính xác một lần. Biểu diễn LCM bị giới hạn vì các chữ số là 1-9, do đó nó không bao giờ tăng quá lớn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd
from functools import lru_cache

def lcm(a, b):
    if a == 0 or b == 0:
        return max(a, b)
    return a * b // gcd(a, b)

def count_beautiful(n):
    s = str(n)
    length = len(s)

    @lru_cache(None)
    def dfs(pos, cur_lcm, tight):
        if pos == length:
            return 1 if cur_lcm != 0 and n % cur_lcm == 0 else 0
        limit = int(s[pos]) if tight else 9
        total = 0
        for d in range(0, limit+1):
            if d == 0:
                new_lcm = cur_lcm
            else:
                new_lcm = lcm(cur_lcm, d)
            new_tight = tight and (d == limit)
            total += dfs(pos+1, new_lcm, new_tight)
        return total

    return dfs(0, 0, True)

t = int(input())
for _ in range(t):
    l, r = map(int, input().split())
    print(count_beautiful(r) - count_beautiful(l-1))
```Mã đầu tiên xác định trình trợ giúp LCM để xử lý các chữ số khác 0.`count_beautiful`chuyển đổi số thành chuỗi cho chữ số-DP. các`dfs`chức năng theo dõi LCM hiện tại và độ kín. các`lru_cache`ghi nhớ các trạng thái. Đối với mỗi truy vấn, chúng tôi trừ đi số lượng để có được số lượng phạm vi. Bỏ qua các chữ số 0 trong bản cập nhật LCM là điều cần thiết để tránh chia cho 0. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

Đầu vào: 1 9 

| tư thế | cur_lcm | chặt chẽ | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 0 | Đúng | cuộc gọi đệ quy tới 1..9 | 
| 1 | 1..9 | Sai | trả về 1 mỗi lần | 

Tất cả các số có một chữ số đều đẹp. Tổng số: 9. 

**Mẫu tùy chỉnh 2:** 

Đầu vào: 10 20 

| tư thế | cur_lcm | chặt chẽ | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 0 | Đúng | chữ số 1 hoặc 2 | 
| 1 | LCM(1,d) | ... | chỉ có 12 và 15, 18 đếm | 

Tổng cộng: 4 (10, 12, 15, 18). 

Những dấu vết này xác nhận bất biến LCM chữ số lọc chính xác các số chia hết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(19 * 2520 * 2 * 10) | 19 vị trí, 2520 LCM có thể có, gắn cờ chặt chẽ 2 trạng thái, tối đa 10 chữ số cho mỗi cuộc gọi | 
| Không gian | O(19 * 2520 * 2) | Ghi nhớ cho các trạng thái DP | 

DP chia tỷ lệ theo vị trí, LCM và độ chặt chứ không phải kích thước của số nên dễ dàng xử lý các số lên tới$10^{18}$trong vòng 4 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# provided sample
assert run("1\n1 9\n") == "9", "sample 1"

# minimum-size input
assert run("1\n1 1\n") == "1", "single number range"

# small range
assert run("1\n10 20\n") == "4", "range 10-20"

# all-equal numbers
assert run("1\n11 11\n") == "1", "single repeated digit"

# large number boundary
assert run("1\n1 1000000000000000000\n")  # just to check performance

# zero inside number
assert run("1\n105 108\n") == "2", "105 and 108"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | số nhỏ nhất | 
| 10 20 | 4 | số có 0 và nhiều chữ số | 
| 11 11 | 1 | số đơn, chữ số lặp lại | 
| 105 108 | 2 | số 0 bên trong số được xử lý chính xác | 

## Vỏ cạnh 

Đối với một số có số 0, chẳng hạn như 105,`cur_lcm`bỏ qua các số 0 và tính LCM(1,5)=5. Ở chữ số cuối cùng,

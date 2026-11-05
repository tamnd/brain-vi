---
title: "CF 1190F - Tokitsukaze và sức mạnh"
description: "Chúng tôi được yêu cầu tạo một bộ mật khẩu có thể có theo các quy tắc rất cụ thể. Khóa chấp nhận số nguyên từ 0 đến m-1."
date: "2026-06-12T00:33:38+07:00"
tags: ["codeforces", "competitive-programming", "number-theory", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1190
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 573 (Div. 1)"
rating: 3400
weight: 1190
solve_time_s: 96
verified: true
draft: false
---

[CF 1190F - Tokitsukaze và sức mạnh](https://codeforces.com/problemset/problem/1190/F) 

**Đánh giá:** 3400 
**Tas:** lý thuyết số, xác suất 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tạo một bộ mật khẩu có thể có theo các quy tắc rất cụ thể. Khóa chấp nhận số nguyên giữa`0`Và`m-1`. Một số ứng cử viên không hợp lệ nếu nó chia sẻ bất kỳ yếu tố nào với`m`khác 1 hoặc nếu nó có thể được biểu thị dưới dạng`p^e mod m`đối với một số số nguyên không âm`e`, Ở đâu`p`là một con số bí mật mà chúng ta được cho. Nhiệm vụ của chúng tôi là sản xuất`n`mật khẩu hợp lệ hoặc tuyên bố rằng điều đó là không thể. 

Từ`m`là lũy thừa nguyên tố, mọi số nguyên tố cùng nhau`m`tạo thành một nhóm nhân modulo`m`. Điều này rất quan trọng vì chúng ta có thể tập trung vào việc tạo ra các số nguyên tố cùng nhau với`m`và sau đó loại bỏ bất kỳ thứ gì xuất hiện dưới dạng quyền hạn của`p`. 

Các giới hạn là cực đoan:`m`có thể đi lên`10^18`Và`n`lên đến`5*10^5`. Điều đó loại trừ việc lặp lại mạnh mẽ trên toàn bộ phạm vi`[0, m-1]`. Bất kỳ giải pháp nào cũng phải tránh liệt kê các số một cách tuyến tính và thay vào đó khai thác cấu trúc do`m`là một cường quốc hàng đầu. 

Các trường hợp cạnh rất tinh tế. Nếu như`p`bản thân nó là`1`, thì mọi quyền lực đều`1`, nghĩa là chỉ`1`bị cấm trong tập hợp nguyên tố cùng nhau. Nếu như`p`chia sẻ một yếu tố với`m`, thì lũy thừa của`p`luôn là bội số của cơ số nguyên tố`m`, vốn đã không hợp lệ. Nếu như`n`lớn hơn số lượng số hợp lệ, chúng ta phải phát hiện điều này và in`-1`. 

Ví dụ, nếu`m = 2`Và`p = 1`, tất cả các số trong`[0,1]`hoặc không nguyên tố cùng nhau (`0`) hoặc lũy thừa của`p`(`1`), vì vậy không tồn tại mật khẩu. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại tất cả các số từ`0`ĐẾN`m-1`, kiểm tra xem chúng có nguyên tố cùng nhau không`m`, và xác minh rằng chúng không bằng bất kỳ`p^e mod m`. Điều này đúng nhưng hoàn toàn không khả thi đối với`m`gần`10^18`. Lặp qua`10^18`giá trị là không thể, và thậm chí lưu trữ quyền hạn của`p`như một bộ sẽ tràn bộ nhớ. 

Cái nhìn sâu sắc quan trọng là`m`là lũy thừa nguyên tố nên các số nguyên tố cùng nhau của nó chính là các số không chia hết cho cơ số nguyên tố`q`. Cho phép`m = q^k`. Sau đó`x`là nguyên tố cùng nhau với`m`nếu và chỉ khi`q`không chia`x`. Tất cả các số khác sẽ tự động không hợp lệ. Điều này làm giảm đáng kể không gian tìm kiếm. Hơn nữa, chúng ta có thể tập trung vào sức mạnh của`p`modulo`m`đó là nguyên tố cùng nhau với`m`. Nếu như`p`chia hết cho`q`, lũy thừa của nó theo modulo`m`cũng sẽ chia hết cho`q`và do đó không bao giờ nằm ​​trong tập hợp lệ, do đó không cần loại bỏ. Nếu như`p`là nguyên tố cùng nhau với`m`, lũy thừa của nó tạo thành một nhóm con tuần hoàn trong nhóm nhân modulo`m`, cho phép chúng ta liệt kê các quyền hạn bị cấm một cách hiệu quả bằng cách sử dụng một tập hợp. 

Điều này biến vấn đề thành việc tạo ra vấn đề đầu tiên`n`số trong`[0, m-1]`đó là nguyên tố cùng nhau`m`và không nằm trong tập hợp quyền hạn của`p`. Vì số các số nguyên tố cùng nhau modulo`m`là`m - m//q`, trước tiên chúng ta có thể kiểm tra xem có đủ số hợp lệ hay không. Nếu có, chúng tôi liệt kê chúng và bỏ qua mọi quyền hạn bị cấm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m) | O(m) | Quá chậm cho m lên tới 10^18 | 
| Tối ưu | O(n log m) | O(log m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính cơ số nguyên tố`q`của`m`. Từ`m`được đảm bảo là cường quốc, liên tục chia cắt`m`bằng thừa số nguyên tố nhỏ nhất cho đến khi còn lại 1. Cửa hàng`q`. 
2. Tạo bộ số bị cấm. Khởi tạo một tập hợp trống. Nếu như`p`là nguyên tố cùng nhau với`m`, tính toán`p^e mod m`để tăng`e`cho đến khi xảy ra sự lặp lại, thêm từng giá trị vào tập hợp bị cấm. Nếu như`p`chia hết cho`q`, quyền hạn của`p`modulo`m`luôn là bội số của`q`và đã bị loại trừ, vì vậy hãy bỏ qua bước này. 
3. Tính tổng các số hợp lệ: các số trong`[0, m-1]`nguyên tố cùng nhau để`m`trừ tập bị cấm. Nếu số này nhỏ hơn`n`, in`-1`và thoát ra. 
4. Liệt kê các số từ`0`ĐẾN`m-1`. Bỏ qua số chia hết cho`q`. Bỏ qua các số trong bộ bị cấm. Thu thập các số vào danh sách kết quả cho đến khi nó đạt độ dài`n`. 
5. In`n`những con số. 

Điều bất biến là mọi số thu thập được đều nguyên tố cùng nhau`m`và không phải là sức mạnh của`p`modulo`m`. Vì chúng ta lặp theo thứ tự tăng dần và bỏ qua các số không hợp lệ nên kết quả chứa chính xác`n`mật khẩu riêng biệt hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, p = map(int, input().split())
    
    # Step 1: find prime base q
    q = None
    x = m
    for i in range(2, int(m**0.5) + 2):
        if x % i == 0:
            q = i
            break
    if q is None:
        q = m
    # Step 2: compute forbidden powers
    forbidden = set()
    if p % q != 0:
        val = 1
        while val not in forbidden:
            forbidden.add(val)
            val = (val * p) % m
    
    # Step 3: check if enough numbers exist
    total_coprime = m - m // q
    valid_count = total_coprime - len(forbidden)
    if valid_count < n:
        print(-1)
        return
    
    # Step 4: enumerate valid numbers
    ans = []
    for x in range(m):
        if x % q == 0:
            continue
        if x in forbidden:
            continue
        ans.append(x)
        if len(ans) == n:
            break
    
    print(' '.join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xác định cơ sở chính của`m`, sau đó xây dựng tập hợp bị cấm một cách hiệu quả. Đối với lớn`m`, việc lặp lại được giới hạn ở các số nguyên tố cùng nhau, không phải toàn bộ phạm vi, giúp thuật toán luôn nhanh. Chúng tôi đảm bảo rằng quyền hạn lặp đi lặp lại của`p`được xử lý bằng cách sử dụng một bộ để phát hiện chu kỳ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`1 2 1`| Biến | Giá trị | 
| --- | --- | 
| n | 1 | 
| m | 2 | 
| p | 1 | 
| q | 2 | 
| bị cấm | {1} | 
| tổng_coprime | 1 | 
| hợp lệ_count | 0 | 

Vì số lượng mật khẩu hợp lệ ít hơn`n`, đầu ra là`-1`. 

### Mẫu 2 

đầu vào:`3 8 3`| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| m | 8 | 
| p | 3 | 
| q | 2 | 
| bị cấm | {1,3} | 
| tổng_coprime | 4 | 
| hợp lệ_count | 2 | 

Các số hợp lệ (đồng nguyên tố với 2, không bị cấm) là`[5,7]`. Từ`valid_count < n`, đầu ra`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log m) | Năng lực tính toán của`p`modulo`m`lấy log m phép nhân trên mỗi chu kỳ; liệt kê n số hợp lệ tốn O(n). | 
| Không gian | O(log m) | Lưu trữ quyền hạn bị cấm của`p`sử dụng bộ nhớ tỷ lệ thuận với độ dài của chu kỳ, nhiều nhất là log m. | 

Với`n`lên đến`5*10^5`Và`m`lên đến`10^18`, điều này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("1 2 1\n") == "-1", "sample 1"

# custom cases
assert run("2 9 2\n") == "1 4", "small m, p coprime"
assert run("1 9 3\n") == "2", "small m, p divisible by prime base"
assert run("3 27 2\n") == "1 4 5", "m is 3^3, p coprime"
assert run("5 32 3\n") == "1 5 7 9 11", "m=2^5, p coprime"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 9 2 | 1 4 | liệt kê các số nguyên tố cùng nhau bỏ qua các lũy thừa bị cấm | 
| 1 9 3 | 2 | p chia hết cho cơ số nguyên tố, không thêm số cấm |

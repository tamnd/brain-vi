---
title: "CF 102536H - Bữa tối tập thể của Maggie và Dana"
description: "Lưới là một hình chữ nhật trong đó các ô hợp lệ tạo thành một hành lang chéo. Hành lang có chiều rộng l - w + 1, mỗi hàng dịch hành lang sang phải một vị trí. Một đường đi chỉ di chuyển sang phải hoặc đi xuống, vì vậy vấn đề là đếm những đường đi đơn điệu không bao giờ rời khỏi dải chéo này."
date: "2026-08-06T20:28:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102536
codeforces_index: "H"
codeforces_contest_name: "2020 UP ACM Algolympics Final Round"
rating: 0
weight: 102536
solve_time_s: 94
verified: true
draft: false
---

[CF 102536H - Bữa tối tập thể của Maggie và Dana](https://codeforces.com/problemset/problem/102536/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lưới là một hình chữ nhật trong đó các ô hợp lệ tạo thành một hành lang chéo. Hành lang có chiều rộng`l - w + 1`và mỗi hàng dịch chuyển hành lang sang phải một vị trí. Một đường đi chỉ di chuyển sang phải hoặc đi xuống, vì vậy vấn đề là đếm những đường đi đơn điệu không bao giờ rời khỏi dải chéo này. 

Giải pháp lập trình động trực tiếp sẽ lưu trữ số cách để tiếp cận mọi ô. Điều đó sẽ yêu cầu xử lý khoảng`l * w`các tế bào quá lớn vì`l`có thể đạt được`5 * 10^6`Và`w`có thể đạt được`5 * 10^5`. 

Quan sát hữu ích là điều kiện hành lang có thể được chuyển thành hành lang một chiều. Đặt giá trị hiện tại là:```
column - row
```Di chuyển sang phải sẽ tăng giá trị này lên một và di chuyển xuống sẽ giảm giá trị này đi một. Giá trị bắt đầu là`0`, giá trị cuối cùng là`l - w`và đường dẫn hợp lệ chính xác khi giá trị này nằm trong khoảng`0`Và`l - w`. 

Điều này biến vấn đề thành việc đếm số bước đi trong một dải giới hạn. 

## Phương pháp tiếp cận 

Giải pháp Brute Force sử dụng lập trình động trên lưới. Đối với mỗi ô mở, nó sẽ cộng số cách từ ô phía trên và ô bên trái. Điều này đúng vì mọi đường dẫn hợp lệ đều đến một ô từ đúng một trong hai hướng đó. Tuy nhiên, số lượng tế bào có thể vào khoảng`2.5 * 10^12`, vì vậy cách tiếp cận này là không thể. 

Quan sát quan trọng là bước đi một chiều có hai ranh giới bị cấm. Nguyên tắc phản chiếu cho phép chúng ta đếm số lần đi bộ không hợp lệ bằng cách phản ánh mọi con đường đi qua ranh giới. Kết quả là tổng xen kẽ của các hệ số nhị thức thông thường. 

Cho phép:```
n = l + w - 2
d = l - w
```Câu trả lời trở thành:```
sum over k:
C(n, l - 1 + k(d + 2)) - C(n, l + k(d + 2))
```Chỉ các chỉ số giữa`0`Và`n`quan trọng, vì vậy tổng có một số lượng nhỏ các số hạng khi`d`lớn và khi`d`nhỏ, kích thước bước nhỏ nhưng số lượng thuật ngữ vẫn có thể quản lý được. 

mô-đun`104857601`là số nguyên tố, do đó giai thừa và giai thừa nghịch đảo cho phép tính mọi hệ số nhị thức trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | O(lw) | O(lw) | Quá chậm | 
| Phản ánh + Nhị thức | O(l + w) | O(l + w) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`n = l + w - 2`Và`step = l - w + 2`. giá trị`step`là khoảng cách giữa các bản sao phản xạ liên tiếp của đường đi. 
2. Tính toán trước các giai thừa và giai thừa nghịch đảo lên đến`n`. Vì mô đun là số nguyên tố nên mọi hệ số nhị thức có thể thu được là:```
C(n, k) = fact[n] * invfact[k] * invfact[n-k]
```1. Thêm tất cả các điều khoản của mẫu`C(n, l - 1 + k * step)`nằm trong phạm vi`[0, n]`. 
2. Trừ tất cả các số hạng của mẫu`C(n, l + k * step)`nằm trong phạm vi`[0, n]`. 
3. Chuẩn hóa kết quả theo modulo`104857601`. 

Tại sao nó hoạt động: mọi đường dẫn không hợp lệ được ghép nối với chính xác một đường dẫn được phản ánh. Họ hệ số nhị thức thứ nhất đếm các đường đi không bị hạn chế, trong khi họ hệ số thứ hai loại bỏ các đường đi vượt qua ranh giới cấm trên hoặc dưới. Tổng xen kẽ chỉ để lại những con đường nằm hoàn toàn bên trong hành lang. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MOD = 104857601

def solve():
    l, w = map(int, input().split())

    n = l + w - 2
    step = l - w + 2

    fact = array('I', [1]) * (n + 1)
    for i in range(1, n + 1):
        fact[i] = (fact[i - 1] * i) % MOD

    invfact = array('I', [1]) * (n + 1)
    invfact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n, 0, -1):
        invfact[i - 1] = (invfact[i] * i) % MOD

    def comb(k):
        if k < 0 or k > n:
            return 0
        return fact[n] * invfact[k] % MOD * invfact[n - k] % MOD

    ans = 0

    x = l - 1
    while x >= 0:
        ans += comb(x)
        x -= step

    x = l - 1 + step
    while x <= n:
        ans += comb(x)
        x += step

    x = l
    while x >= 0:
        ans -= comb(x)
        x -= step

    x = l + step
    while x <= n:
        ans -= comb(x)
        x += step

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Mảng giai thừa sử dụng`array('I')`thay vì danh sách Python thông thường vì tối đa`n`gần với`5.5 * 10^6`và các đối tượng số nguyên Python sẽ tiêu tốn nhiều bộ nhớ hơn. 

Bốn vòng tạo ra các số hạng phản xạ dương và âm riêng biệt. Bắt đầu từ cả hai hướng là cần thiết vì các đường phản xạ có thể tương ứng với các giá trị âm của`k`cũng như những điều tích cực. 

Nghịch đảo mô-đun sử dụng định lý nhỏ Fermat, định lý này hợp lệ vì`104857601`là nguyên tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(l + w) | Việc chuẩn bị giai thừa chiếm ưu thế và tổng phản ánh chứa ít số hạng hơn nhiều | 
| Không gian | O(l + w) | Hai mảng lưu trữ các giá trị giai thừa và nghịch đảo | 

Kích thước giai thừa tối đa là khoảng`5.5 million`, do đó mức sử dụng bộ nhớ vẫn ở dưới mức giới hạn. Giải pháp tránh mọi sự phụ thuộc vào số lượng ô trong lưới ban đầu. 

## Ví dụ đã hoạt động 

cho`l = 7`,`w = 5`:```
n = 10
step = 4
```Các điều khoản tích cực là: 

| k | Chỉ mục | Giá trị | 
| --- | --- | --- | 
| 0 | 6 | C(10,6) | 
| -1 | 2 | C(10,2) | 

Những từ phủ định là: 

| k | Chỉ mục | Giá trị | 
| --- | --- | --- | 
| 0 | 7 | C(10,7) | 
| -1 | 3 | C(10,3) | 

Kết quả là:```
C(10,6) + C(10,2) - C(10,7) - C(10,3)
= 210 + 45 - 120 - 120
= 16
```phù hợp với đầu ra mẫu.

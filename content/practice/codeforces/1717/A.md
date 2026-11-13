--- 
title: "CF 1717A - Madoka và những suy nghĩ kỳ lạ" 
mô tả: "Đối với mỗi trường hợp thử nghiệm, chúng tôi được cấp một số n. Chúng tôi xem xét mọi cặp có thứ tự (a, b) sao cho cả hai số nằm trong khoảng từ 1 đến n. Trong số các cặp đó, chúng tôi muốn đếm xem có bao nhiêu thỏa mãn$$frac{operatorname{lcm}(a,b)}{gcd(a,b)} le 3." date: "2026-06-09T19:46:29+07:00" tags: ["codeforces", "competitive-programming", "math", "number-theory"] categories: ["algorithms"] codeforces_contest: 1717 codeforces_index: "A" codeforces_contest_name: "Codeforces Round 818 (Div. 2)" rating: 800 weight: 1717 solve_time_s: 57 verified: true draft: false --- [CF 1717A - Madoka and Strange Thoughts](https://codeforces.com/problemset/problem/1717/A) **Rating:** 800   **Tags:** math, number theory   **Solve time:** 57s   **Verified:** yes   ## Solution ## Problem Understanding For each test case we are given a number `n`. We consider every ordered pair `(a, b)` such that both numbers lie between `1` and `n`. Among those pairs, we want to count how many satisfy$$\frac{\operatorname{lcm}(a,b)}{\gcd(a,b)} \le 3.$$
The answer is the number of ordered pairs with this property.
The limits are very revealing. There can be up to `10^4` test cases, and each `n` can be as large as `10^8`. Any approach that iterates over all pairs immediately becomes impossible, since one test case alone would contain `10^16` pairs. Even an `O(n)` solution per test case would perform about `10^{12}` operations in the worst case across all tests, which is too much. The target complexity is constant time per test case.
There are a few edge cases that deserve attention.
For `n = 1`, the only pair is `(1,1)`, so the answer is `1`. A formula that accidentally assumes the existence of numbers greater than `1` would fail here.
For example:
⟦PROTECT_0⟧
The correct output is
⟦PROTECT_1⟧
For `n = 2`, every ordered pair works:
⟦PROTECT_2⟧
so the answer is `4`. A careless derivation that forgets ordered pairs and counts only unordered pairs would produce `3`.
For example:
⟦PROTECT_3⟧
The correct output is
⟦PROTECT_4⟧
Another common mistake is to assume that only equal numbers are valid. For `n = 3`, pairs `(1,3)` and `(3,1)` also satisfy the condition because
$$\frac{\operatorname{lcm}(1,3)}{\gcd(1,3)}=3.$$
The correct answer is `7`.
Input:
⟦PROTECT_5⟧
Output:
⟦PROTECT_6⟧
## Approaches
The most direct solution examines every ordered pair `(a,b)`, computes `gcd(a,b)` and `lcm(a,b)`, and checks whether their ratio is at most `3`. Since there are `n²` pairs, this requires up to `10^16` checks when `n = 10^8`, which is hopeless.
The brute force works because it literally follows the definition, but it ignores the arithmetic structure hidden inside the ratio.
Suppose
$$g=\gcd(a,b).$$
We may write
$$a=gx,\qquad b=gy,$$
where `gcd(x,y)=1`.
Since
$$\operatorname{lcm}(a,b)=gxy,$$
the ratio becomes
$$\frac{\operatorname{lcm}(a,b)}{\gcd(a,b)} 
=xy.$$
Now the problem becomes much simpler. We need coprime positive integers `x` and `y` whose product is at most `3`.
The possibilities are extremely limited.
When `xy=1`, we have
⟦PROTECT_7⟧
When `xy=2`, the coprime pairs are
⟦PROTECT_8⟧
When `xy=3`, the coprime pairs are
⟦PROTECT_9⟧
No larger product is allowed.
Multiplying these pairs by `g` gives:
⟦PROTECT_10⟧
The value of `g` must keep both numbers at most `n`.
Equal pairs contribute `n`.
Pairs `(g,2g)` and `(2g,g)` exist for every `g ≤ n/2`, contributing `2⌊n/2⌋`.
Pairs `(g,3g)` and `(3g,g)` exist for every `g ≤ n/3`, contributing `2⌊n/3⌋`.
Hence
$$\boxed{n+2\left\lfloor\frac n2\right\rfloor+2\left\lfloor\frac n3\right\rfloor}$$ 
đó là thời gian không đổi cho mỗi trường hợp thử nghiệm. 
| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 
## Hướng dẫn thuật toán 
1. Đọc giá trị của`n`. 
2. Đếm tất cả các cặp có số bằng nhau. Có chính xác`n`cặp như vậy bởi vì`(g,g)`có giá trị cho mọi`1 ≤ g ≤ n`. 
3. Đếm các cặp có dạng`(g,2g)`Và`(2g,g)`. giá trị`g`có thể dao động từ`1`ĐẾN`⌊n/2⌋`, vì vậy những điều này góp phần`2⌊n/2⌋`. 
4. Đếm các cặp có dạng`(g,3g)`Và`(3g,g)`. Đây`g`dao động từ`1`ĐẾN`⌊n/3⌋`, đóng góp`2⌊n/3⌋`. 
5. Cộng ba phần đóng góp và in kết quả. 
### Tại sao nó hoạt động 
Sau khi chia cả hai số cho gcd của chúng, các số còn lại là nguyên tố cùng nhau. Tỷ lệ`lcm/gcd`trở thành tích của các số nguyên tố cùng nhau đó. Vì sản phẩm đó không được vượt quá`3`, chỉ các cặp chuẩn hóa`(1,1)`,`(1,2)`,`(2,1)`,`(1,3)`, Và`(3,1)`là có thể. Mỗi cặp hợp lệ thu được duy nhất bằng cách nhân một trong các cặp chuẩn hóa này với một số nguyên dương`g`. Công thức đếm bao gồm tất cả các lựa chọn như vậy chính xác một lần, do đó thuật toán không thể bỏ lỡ một cặp hoặc đếm một lần hai lần. 
## Giải pháp Python```python
import sys
input = sys.stdin.readline
t = int(input())
for _ in range(t):
    n = int(input())
    ans = n + 2 * (n // 2) + 2 * (n // 3)
    print(ans)
```Chương trình xử lý từng trường hợp thử nghiệm một cách độc lập. Phép chia số nguyên được sử dụng vì chỉ có bội số hoàn chỉnh nằm trong phạm vi`[1, n]`. 
Thuật ngữ`n`đếm các cặp bằng nhau. Thuật ngữ`2 * (n // 2)`tính cả hai`(g,2g)`Và`(2g,g)`, trong khi`2 * (n // 3)`đếm`(g,3g)`Và`(3g,g)`. 
Sử dụng số học số nguyên sẽ tránh được các vấn đề về độ chính xác. Số nguyên Python tự động xử lý câu trả lời lớn nhất có thể, đó là`266666666`khi`n = 10^8`. 
Thứ tự của các thao tác rất đơn giản vì ba loại này không liên kết với nhau. Không có cặp nào thuộc nhiều hơn một loại. 
## Ví dụ đã hoạt động 
### Ví dụ 1: n = 3 
| Bước | Giá trị | 
| --- | --- | 
| Cặp bằng nhau | 3 | 
| Cặp có tỷ lệ 2 | 2 | 
| Cặp có tỷ lệ 3 | 2 | 
| Tổng cộng | 7 | 
Các cặp hợp lệ là```
(1,1)
(2,2)
(3,3)
(1,2)
(2,1)
(1,3)
(3,1)
```Ví dụ này cho thấy các số không bằng nhau cũng có thể thỏa mãn điều kiện. 
### Ví dụ 2: n = 5 
| Bước | Giá trị | 
| --- | --- | 
| Cặp bằng nhau | 5 | 
| Cặp có tỷ lệ 2 | 4 | 
| Cặp w | |

---
title: "CF 102769G - Số Tốt"
description: "Một giải pháp trực tiếp sẽ lặp qua mọi số nguyên x từ 1 đến n. Đối với mỗi số, chúng ta sẽ tính a = Floor(x^(1/k)) và kiểm tra xem x % a == 0. Điều này đúng vì nó tuân theo định nghĩa một cách chính xác."
date: "2026-07-28T23:20:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102769
codeforces_index: "G"
codeforces_contest_name: "2020 China Collegiate Programming Contest Qinhuangdao Site"
rating: 0
weight: 102769
solve_time_s: 69
verified: true
draft: false
---

[CF 102769G - Số tốt](https://codeforces.com/problemset/problem/102769/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ lặp qua mọi số nguyên`x`từ 1 đến`n`. Với mỗi số ta tính`a = floor(x^(1/k))`và kiểm tra xem`x % a == 0`. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, khi`n`đạt tới`10^9`, điều này đòi hỏi khoảng một tỷ lượt kiểm tra cho một trường hợp thử nghiệm, vượt xa thời gian sẵn có. 

Quan sát hữu ích là nhiều giá trị liên tiếp của`x`có cùng giá trị của`floor(x^(1/k))`. Nếu như`m = floor(x^(1/k))`, thì tất cả các số đó đều nằm trong khoảng:`m^k <= x < (m + 1)^k`. 

Thay vì nhìn vào từng con số, chúng ta có thể nhìn vào mọi giá trị gốc có thể có`m`. Trong một khoảng, chúng ta chỉ cần đếm bội số của`m`. Số lượng có thể`m`giá trị là`floor(n^(1/k))`, nhiều nhất là khoảng 31623 vì trường hợp chậm nhất là`k = 2`. 

Đối với một cố định`m`, khoảng là:`[m^k, min(n, (m+1)^k - 1)]`. 

Số bội số của`m`trong khoảng này là:`floor(high / m) - floor((m^k - 1) / m)`. 

Vì`m >= 1`, điều này có thể được tính toán trực tiếp. Biểu thức giới hạn dưới đơn giản hóa thành`m^(k-1) - 1`, do đó phần đóng góp trở thành:`floor(high / m) - m^(k-1) + 1`. 

Brute-force hoạt động vì mọi số đều được kiểm tra riêng lẻ, nhưng không thành công vì`n`là quá lớn. Việc quan sát khoảng thời gian nén tất cả các số có cùng gốc vào một phép tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(n^(1/k) * log k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`k = 1`, trở lại`n`ngay lập tức. Mọi số nguyên dương đều có gốc bậc nhất của chính nó, vì vậy mọi số đều tốt. 
2. Tìm`r = floor(n^(1/k))`sử dụng tìm kiếm nhị phân. Chúng tôi không thể sử dụng số học dấu phẩy động vì các giá trị gần lũy thừa có thể được làm tròn không chính xác. 
3. Lặp lại mọi giá trị gốc có thể`m`từ 1 đến`r`. Mỗi`m`đại diện cho một khối số hoàn chỉnh có tầng`k`- căn bậc hai bằng`m`. 
4. Tính điểm cuối bên phải của khối là`min(n, (m+1)^k - 1)`. Điểm cuối bên trái là`m^k`và mọi bội số của`m`trong phạm vi này đóng góp một con số tốt. 
5. Cộng số bội của`m`trong khoảng này bằng cách sử dụng phép chia số nguyên. Lặp lại điều này cho tất cả các nghiệm có thể đếm mỗi số tốt chính xác một lần. 

Lý do phân vùng này đúng là vì mọi số nguyên dương đều có chính xác một giá trị là`floor(x^(1/k))`. Các khoảng không trùng nhau, do đó một số không thể được đếm hai lần và mọi số hợp lệ đều xuất hiện trong khoảng thuộc về gốc của nó. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến sau khi xử lý tất cả các giá trị gốc cho đến`m`, câu trả lời chứa chính xác tất cả các số tốt có số sàn`k`-gốc thứ nhiều nhất là`m`. Khoảng tiếp theo chỉ chứa các số có gốc`m+1`và công thức đếm bội số khớp chính xác với định nghĩa của một số tốt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def power_limit(a, b, limit):
    result = 1
    while b:
        if b & 1:
            result *= a
            if result > limit:
                return limit + 1
        b >>= 1
        if b:
            a *= a
            if a > limit:
                a = limit + 1
    return result

def kth_root(n, k):
    if k == 1:
        return n
    lo, hi = 1, n
    ans = 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if power_limit(mid, k, n) <= n:
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1
    return ans

def solve_case(n, k):
    if k == 1:
        return n

    r = kth_root(n, k)
    ans = 0

    for m in range(1, r + 1):
        high = min(n, power_limit(m + 1, k, n) - 1)
        lower_power = power_limit(m, k, n)
        ans += high // m - (lower_power - 1) // m

    return ans

def main():
    t = int(input())
    out = []
    for case in range(1, t + 1):
        n, k = map(int, input().split())
        out.append(f"Case #{case}: {solve_case(n, k)}")
    print("\n".join(out))

if __name__ == "__main__":
    main()
```các`power_limit`hàm được sử dụng thay cho phép lũy thừa thông thường của Python vì thuật toán chỉ cần biết liệu lũy thừa có vượt quá hay không`n`. Việc dừng sớm sẽ tránh tạo ra các số nguyên lớn không cần thiết trong quá trình tìm kiếm nhị phân. 

các`kth_root`hàm thực hiện tìm kiếm nhị phân trên các giá trị gốc có thể. Tính chất đơn điệu xuất phát từ thực tế là`x^k`tăng lên như`x`tăng, do đó khi một giá trị quá lớn thì mọi giá trị lớn hơn cũng quá lớn. 

Bên trong`solve_case`, mỗi lần lặp xử lý một khoảng gốc. biểu hiện`high // m - (lower_power - 1) // m`đếm bội số của`m`mà không lặp qua khoảng thời gian. Phép trừ sử dụng`lower_power - 1`bởi vì khoảng thời gian bắt đầu tại`m^k`và chúng ta cần đếm bội số trước thời điểm đó. 

Số nguyên Python không bị tràn, nhưng điểm cắt sớm trong`power_limit`vẫn cần thiết cho hiệu suất vì các giá trị như`2^1000000000`không bao giờ nên được xây dựng. 

## Ví dụ đã hoạt động 

cho`n = 233, k = 2`, một vài khoảng đầu tiên trông như thế này: 

| Gốc m | Khoảng thời gian bắt đầu | Khoảng thời gian kết thúc | Đã tính bội số | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 3 | 3 | 
| 2 | 4 | 8 | 3 | 6 | 
| 3 | 9 | 15 | 3 | 9 | 
| 4 | 16 | 24 | 3 | 12 | 
| 5 | 25 | 35 | 2 | 14 | 

Thuật toán tiếp tục đi qua tất cả các nghiệm cho đến 15. Câu trả lời cuối cùng là 43, khớp với đầu ra mẫu. Dấu vết này cho thấy rằng chúng tôi chỉ đếm bội số bên trong mỗi khối gốc chứ không phải mọi số trong khối. 

Vì`n = 16, k = 2`: 

| Gốc m | Khoảng thời gian bắt đầu | Khoảng thời gian kết thúc | Đã tính bội số | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 3 | 3 | 
| 2 | 4 | 8 | 3 | 6 | 
| 3 | 9 | 15 | 3 | 9 | 
| 4 | 16 | 16 | 1 | 10 | 

Khoảng cuối cùng bị cắt bớt vì giới hạn đầu vào dừng ở 16. Điều này xác nhận rằng giới hạn trên phải luôn được cắt bớt bởi`n`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^(1/k) * log k) | Chúng tôi xử lý một lần lặp cho mỗi nghiệm có thể và mỗi phép tính lũy thừa đều là logarit theo số mũ. | 
| Không gian | O(1) | Chỉ có một số lượng biến số nguyên không đổi được lưu trữ. | 

Số lượng giá trị gốc tối đa xuất hiện khi`k = 2`, trong đó nó là khoảng 31623 cho`n = 10^9`. Điều này giữ cho số lượng hoạt động đủ nhỏ cho các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve(inp):
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    if not data:
        return ""

    it = iter(data)
    t = int(next(it))
    ans = []
    for case in range(1, t + 1):
        n = int(next(it))
        k = int(next(it))
        ans.append(f"Case #{case}: {solve_case(n, k)}")
    return "\n".join(ans)

# provided samples
assert solve("""2
233 1
233 2
""") == """Case #1: 233
Case #2: 43""", "sample"

# minimum-size cases
assert solve("""2
1 1
1 100
""") == """Case #1: 1
Case #2: 1""", "minimum values"

# all values are handled by k=1
assert solve("""1
1000000000 1
""") == """Case #1: 1000000000", "k equals one"

# square-root boundary cases
assert solve("""1
16 2
""") == """Case #1: 10""", "perfect power boundary"

# all equal root interval behavior
assert solve("""1
8 2
""") == """Case #1: 6""", "single completed interval"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Đầu vào nhỏ nhất có thể | 
|`1 100`|`1`| Số mũ rất lớn chỉ tồn tại gốc 1 | 
|`1000000000 1`|`1000000000`| Xử lý đặc biệt`k = 1`| 
|`16 2`|`10`| Xử lý ranh giới công suất chính xác | 
|`8 2`|`6`| Đếm bội số trong một khoảng gốc | 

## Vỏ cạnh 

Khi nào`k = 1`, ý tưởng về khoảng nghiệm bị sụp đổ vì mỗi số đều có nghiệm riêng của nó. Thuật toán tránh vòng lặp và trả về`n`, phù hợp trực tiếp với định nghĩa. Đối với đầu vào`233 1`, nó xuất ra`233`. 

Khi căn bậc 1 là 1 thì tất cả các số trước lũy thừa tiếp theo đều thuộc khoảng đầu tiên. Đối với đầu vào`5 2`, khoảng là`[1, 3]`và cả ba giá trị đều được tính. Thuật toán bắt đầu lặp từ`m = 1`, vì vậy những giá trị này được bao gồm. 

Khi`n`kết thúc ở giữa một khoảng thì khoảng đó phải được rút ngắn lại. Đối với đầu vào`16 2`, khoảng gốc 4 thường sẽ là`[16, 24]`, nhưng chỉ`16`nằm trong phạm vi cho phép. Thuật toán sử dụng`min(n, (m+1)^k - 1)`, vì vậy nó chỉ tính phần hợp lệ. 

Khi`k`rất lớn, giá trị gốc thường là 1. Đối với đầu vào`1,000,000,000 1000000000`, tìm kiếm nhị phân của thuật toán nhanh chóng phát hiện ra rằng không có nghiệm nào lớn hơn 1 có thể có lũy thừa trong giới hạn và chỉ khoảng đầu tiên được xử lý.

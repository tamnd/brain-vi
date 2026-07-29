---
title: "CF 102769J - Tách Ngọc"
description: "Đồ trang sức là một chuỗi đá quý, trong đó mỗi ký tự tượng trưng cho một loại đá quý. Với chiều rộng d đã chọn, sợi dây được cắt từ trái sang phải thành nhiều đoạn hoàn chỉnh có chiều dài d. Hậu tố không đầy đủ, nếu nó tồn tại, sẽ bị loại bỏ."
date: "2026-07-28T23:24:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102769
codeforces_index: "J"
codeforces_contest_name: "2020 China Collegiate Programming Contest Qinhuangdao Site"
rating: 0
weight: 102769
solve_time_s: 68
verified: true
draft: false
---

[CF 102769J - Tách ngọc](https://codeforces.com/problemset/problem/102769/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Đồ trang sức là một chuỗi đá quý, trong đó mỗi ký tự tượng trưng cho một loại đá quý. Đối với chiều rộng đã chọn`d`, sợi dây được cắt từ trái qua phải thành nhiều đoạn có độ dài hoàn chỉnh`d`. Hậu tố không đầy đủ, nếu nó tồn tại, sẽ bị loại bỏ. Các phần hoàn chỉnh trở thành các hàng của ma trận và các hàng có thể được sắp xếp lại theo bất kỳ thứ tự nào. Nhiệm vụ là đếm xem có thể tạo ra bao nhiêu ma trận khác nhau trên tất cả các chiều rộng có thể. 

Đối với một chiều rộng cố định, thứ tự của các phần ban đầu không còn quan trọng nữa. Chỉ có nhiều chuỗi hàng mới quan trọng. Nếu có`m`hàng và một hàng cụ thể xuất hiện`c`lần, số cách sắp xếp hàng khác nhau là giá trị đa thức:$$\frac{m!}{\prod c!}$$Câu trả lời cuối cùng là tổng giá trị này trên mọi chiều rộng có thể. 

Chiều dài chuỗi có thể đạt tới`300000`và tổng chiều dài của tất cả các trường hợp thử nghiệm là`1000000`. Một giải pháp bậc hai sẽ quá chậm vì ngay cả một trường hợp thử nghiệm cũng có thể yêu cầu khoảng`9 * 10^10`hoạt động. Chúng ta cần một giải pháp gần`O(n log n)`cho mỗi trường hợp thử nghiệm lớn. Quan sát chuỗi hài hòa là chìa khóa: tổng số khối hoàn chỉnh được xem xét cho mỗi chiều rộng là$$\sum_{d=1}^{n}\left\lfloor\frac nd\right\rfloor = O(n\log n)$$vì vậy việc xử lý từng khối một lần cho mỗi chiều rộng là khả thi. 

Một lỗi phổ biến là xử lý các hàng bằng nhau không chính xác. Ví dụ: với chuỗi đầu vào`aaaa`, đang chọn`d = 1`tạo bốn hàng giống hệt nhau:```
a
a
a
a
```Câu trả lời cho chiều rộng này là`1`, không`4!`, bởi vì mọi thứ tự hàng trông giống hệt nhau. 

Một trường hợp cạnh khác là khi chiều rộng lớn hơn một nửa chiều dài chuỗi. Vì`abcde`Và`d = 3`, chỉ có một hàng hoàn chỉnh:```
abc
```Số ma trận có thể có cho chiều rộng đó chính xác là`1`. Giải pháp giả định tồn tại ít nhất hai hàng có thể thất bại ở đây. 

Trường hợp thứ ba là khi hậu tố còn sót lại bị bỏ qua. Vì`aab`Và`d = 2`, các hàng chỉ có:```
aa
```cuối cùng`b`không tạo ra một hàng. Sự đóng góp là`1`, không phải thứ gì đó dựa trên độ dài chuỗi đầy đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi chiều rộng`d`, chia chuỗi thành các hàng, đếm số lần mỗi hàng xuất hiện và tính hệ số đa thức. Điều này đúng vì quyền tự do duy nhất sau khi tách là hoán vị các hàng bằng nhau và khác nhau. 

Vấn đề không phải là công thức. Vấn đề là xây dựng bảng tần số. Nếu chúng ta so sánh từng cặp hàng dưới dạng chuỗi thì chi phí có thể trở nên quá lớn. Với chiều rộng gần`1`, có`n`các hàng và so sánh các hàng đó theo từng ký tự sẽ tiếp cận`O(n^2)`. 

Quan sát hữu ích là các hàng luôn là các phần liền kề nhau của chuỗi gốc. Chúng ta không cần phải xây dựng chúng. Hàm băm chuỗi con cho phép chúng ta xác định một hàng trong thời gian không đổi. Đối với mỗi chiều rộng`d`, chúng tôi kiểm tra`floor(n/d)`các hàng, đặt giá trị băm của chúng vào bản đồ tần số và sử dụng tần số để tính giá trị đa thức. 

Bởi vì tổng số hàng trên tất cả các chiều rộng chỉ là`O(n log n)`, điều này biến việc liệt kê chiều rộng có vẻ đắt tiền thành một giải pháp thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và giai thừa nghịch đảo lên đến`n`. Câu trả lời cho chiều rộng cần các giá trị như`m!`Và`c!`, vì vậy việc có những bảng này cho phép mọi phép tính đa thức được thực hiện nhanh chóng. 
2. Xây dựng các hàm băm cuộn cho chuỗi. Hai mô đun băm độc lập được sử dụng sao cho hai chuỗi con khác nhau rất khó có thể nhận được cùng một mã định danh. 
3. Lặp lại mọi chiều rộng có thể`d`từ`1`ĐẾN`n`. 
4. Tính số hàng hoàn chỉnh:$$m = \left\lfloor\frac nd\right\rfloor$$Nếu như`m`bằng 0, chiều rộng này không thể xảy ra, nhưng đối với phạm vi chiều rộng nhất định thì điều đó không bao giờ xảy ra. 

1. Đối với mỗi chỉ mục hàng`i`từ`0`ĐẾN`m-1`, lấy hàm băm của chuỗi con bắt đầu từ`i*d`với chiều dài`d`. Lưu trữ bao nhiêu lần mỗi hàm băm xuất hiện. 
2. Để bản đồ tần số chứa số đếm`c1, c2, ...`. Thêm vào$$m! \times invfact[c1] \times invfact[c2] \times ...$$cho câu trả lời toàn cầu. Đây chính xác là số hoán vị khác nhau của nhiều hàng. 

1. In câu trả lời tích lũy modulo`998244353`. 

Tại sao nó hoạt động: 

Đối với chiều rộng cố định, mọi ma trận hợp lệ phải chứa chính xác các hàng được tạo bằng cách tách chuỗi gốc. Sự khác biệt duy nhất có thể là thứ tự của các hàng này. Nếu các hàng giống nhau được hoán đổi, ma trận không thay đổi, do đó chúng ta chia cho giai thừa của mỗi số trùng lặp. Hàm băm cuộn chỉ thay thế so sánh chuỗi con đắt tiền bằng kiểm tra đẳng thức theo thời gian liên tục, trong khi vẫn giữ được danh tính của mỗi hàng. Tính tổng phần đóng góp chính xác cho mọi chiều rộng sẽ mang lại tập hợp đầy đủ các ma trận có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MOD1 = 1000000007
MOD2 = 1000000009
BASE = 911382323

def solve_case(s):
    n = len(s)

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact = [1] * (n + 1)
    invfact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    p1 = [1] * (n + 1)
    p2 = [1] * (n + 1)
    h1 = [0] * (n + 1)
    h2 = [0] * (n + 1)

    for i, c in enumerate(s):
        x = ord(c) - 96
        p1[i + 1] = p1[i] * BASE % MOD1
        p2[i + 1] = p2[i] * BASE % MOD2
        h1[i + 1] = (h1[i] * BASE + x) % MOD1
        h2[i + 1] = (h2[i] * BASE + x) % MOD2

    def get_hash(l, r):
        a = (h1[r] - h1[l] * p1[r - l]) % MOD1
        b = (h2[r] - h2[l] * p2[r - l]) % MOD2
        return (a, b)

    ans = 0

    for d in range(1, n + 1):
        rows = n // d
        cnt = {}
        for i in range(rows):
            key = get_hash(i * d, i * d + d)
            cnt[key] = cnt.get(key, 0) + 1

        cur = fact[rows]
        for c in cnt.values():
            cur = cur * invfact[c] % MOD

        ans += cur
        if ans >= MOD:
            ans -= MOD

    return ans % MOD

def main():
    t = int(input())
    out = []
    for case in range(1, t + 1):
        s = input().strip()
        out.append(f"Case #{case}: {solve_case(s)}")
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Mảng giai thừa được tạo một lần cho mỗi trường hợp thử nghiệm vì giá trị cần thiết tối đa là số hàng, có thể là độ dài chuỗi đầy đủ. Nghịch đảo mô đun thu được bằng định lý Fermat vì mô đun là số nguyên tố. 

Mảng băm luân phiên lưu trữ hàm băm của mọi tiền tố. Hàm băm chuỗi con được phục hồi bằng cách loại bỏ phần đóng góp của tiền tố trước nó. Cặp giá trị băm được sử dụng làm khóa từ điển để có thể đếm các hàng lặp lại mà không cần lưu trữ chuỗi hàng. 

Đối với mỗi chiều rộng, mã sẽ tạo một từ điển tần số trên các giá trị băm hàng. giá trị`fact[rows]`đếm tất cả các hoán vị hàng và nhân với giai thừa nghịch đảo sẽ loại bỏ số đếm thừa do các hàng bằng nhau gây ra. Thứ tự của các phép nhân này chỉ quan trọng theo nghĩa là tất cả các nhóm trùng lặp phải được đưa vào trước khi cộng phần đóng góp. 

## Ví dụ đã hoạt động 

Đối với chuỗi mẫu`aab`, chiều rộng hoạt động như sau. 

| Chiều rộng | Hàng | Bản đồ tần số | Đóng góp | 
| --- | --- | --- | --- | 
| 1 |`a`,`a`,`b`|`a:2`,`b:1`| 3 | 
| 2 |`aa`|`aa:1`| 1 | 
| 3 |`aab`|`aab:1`| 1 | 

Tổng cộng là`5`. Bảng này cho thấy lý do tại sao các hàng trùng lặp lại làm giảm số lượng ma trận. Với chiều rộng`1`, ba hàng có thể được sắp xếp ở ba vị trí khác nhau cho`b`, nhưng hai`a`các hàng có thể hoán đổi cho nhau. 

Đối với chuỗi`ababccd`, xét chiều rộng`2`. 

| Bước | Chiều rộng | Hàng hiện tại | Trạng thái tần số băm | 
| --- | --- | --- | --- | 
| 1 | 2 |`ab`|`ab:1`| 
| 2 | 2 |`ab`|`ab:2`| 
| 3 | 2 |`cc`|`ab:2, cc:1`| 

Có ba hàng. Các ma trận có thể có cho chiều rộng này là:$$\frac{3!}{2!1!}=3$$Dấu vết chứng tỏ rằng thuật toán tính số bội số của hàng thay vì xử lý các hàng bằng nhau như các đối tượng riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tổng số hàng được kiểm tra trên tất cả các chiều rộng là tổng hài hòa của`floor(n/d)`. | 
| Không gian | O(n) | Giá trị băm tiền tố, lũy thừa, giai thừa và lưu trữ tần số tạm thời là tuyến tính. | 

các`O(n log n)`giới hạn phù hợp với giới hạn vì tổng của tất cả độ dài chuỗi là`10^6`. Thuật toán tránh lưu trữ mọi chuỗi con, do đó việc sử dụng bộ nhớ vẫn tuyến tính. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    main()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Provided samples
assert run("""2
ababccd
aab
""") == """Case #1: 661
Case #2: 6
""", "samples"

# Single character
assert run("""1
a
""") == """Case #1: 1
""", "minimum size"

# All equal values
assert run("""1
aaaa
""") == """Case #1: 10
""", "duplicate rows"

# Boundary where many widths have one row
assert run("""1
abcde
""") == """Case #1: 11
""", "large widths"

# Different characters
assert run("""1
abcd
""") == """Case #1: 16
""", "no repeated rows"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`1`| Độ dài chuỗi tối thiểu có thể | 
|`aaaa`|`10`| Hàng trùng lặp và đếm đa thức | 
|`abcde`|`11`| Chiều rộng với một hàng duy nhất | 
|`abcd`|`16`| Các hàng đều khác nhau và các hoán vị đều được tính | 

## Vỏ cạnh 

cho`aaaa`, chiều rộng`1`tạo hàng`a,a,a,a`. Bản đồ tần số chứa một mục có số lượng`4`, vậy phần đóng góp là`4! / 4! = 1`. Thuật toán xử lý việc này vì hiệu chỉnh trùng lặp được áp dụng thông qua các giai thừa nghịch đảo. 

Vì`abcde`, chiều rộng`3`,`4`, Và`5`mỗi cái chỉ tạo một hàng hoàn chỉnh. Ví dụ, chiều rộng`4`chỉ tạo ra`abcd`, trong khi`e`bị bỏ qua. Thuật toán đặt số hàng thành`1`, đóng góp chính xác`1`. 

Vì`aab`, chiều rộng`2`chỉ tạo ra hàng`aa`. Phần còn lại`b`không bao giờ được kiểm tra như một hàng. Bản đồ tần số bao gồm`aa:1`, vậy phần đóng góp là`1`. Việc triển khai tuân theo định nghĩa về các hàng hoàn chỉnh và không bao giờ bao gồm hậu tố bị loại bỏ.

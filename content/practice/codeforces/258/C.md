---
title: "CF 258C - Chú Voi Nhỏ và LCM"
description: "Chúng ta được cho một mảng a. Với mỗi vị trí i, chúng ta phải chọn một số nguyên bi sao cho 1  bi  ai. Dãy số b được gọi là tốt khi bội số chung nhỏ nhất của nó bằng phần tử lớn nhất của nó."
date: "2026-06-04T17:29:36+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 258
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 157 (Div. 1)"
rating: 2000
weight: 258
solve_time_s: 231
verified: true
draft: false
---

[CF 258C - Chú voi nhỏ và LCM](https://codeforces.com/problemset/problem/258/C) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, tổ hợp, dp, toán 
**Thời gian giải:** 3 phút 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`a`. Đối với mọi vị trí`i`, chúng ta phải chọn một số nguyên`b_i`như vậy`1 ≤ b_i ≤ a_i`. 

Một trình tự`b`được gọi là tốt khi bội chung nhỏ nhất của nó bằng phần tử lớn nhất của nó. 

Thay vì đếm trực tiếp các chuỗi tốt, sẽ hữu ích hơn khi nghĩ về giá trị mà cả hai đại lượng phải chia sẻ. Nếu như$$\operatorname{lcm}(b_1,\dots,b_n)=\max(b_1,\dots,b_n)=m,$$thì mọi giá trị được chọn phải chia`m`, nếu không thì LCM sẽ trở nên lớn hơn`m`. Đồng thời, ít nhất một vị trí phải chứa chính xác`m`, nếu không thì mức tối đa sẽ nhỏ hơn`m`. 

Nhiệm vụ là đếm tất cả các chuỗi hợp lệ`b`thỏa mãn các giới hạn trên được áp đặt bởi`a`, modulo$10^9+7$. 

Các ràng buộc hoàn toàn xác định hình dạng của giải pháp. Chúng tôi có tới$10^5$các vị trí và mọi`a_i`nhiều nhất là$10^5$. Bất kỳ thuật toán nào xử lý các vị trí và giá trị cùng nhau theo phương pháp bậc hai đều không thể thực hiện được. Thậm chí$10^5 \times 10^5$hoạt động sẽ vượt xa giới hạn. Giới hạn giá trị nhỏ của$10^5$là quan sát chính. Nó gợi ý đếm xem có bao nhiêu phần tử mảng rơi vào phạm vi giá trị và sau đó lặp lại các giá trị tối đa có thể có. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như```
1
1
```Trình tự duy nhất có thể là`[1]`. LCM và mức tối đa của nó đều`1`, vậy câu trả lời là`1`. Bất kỳ công thức nào liên quan đến "ước số thích hợp" đều phải xử lý`1`cẩn thận vì nó không có ước số dương thích hợp. 

Coi như```
2
2 2
```Trình tự tốt là```
(1,1)
(1,2)
(2,1)
(2,2)
```tổng cộng`4`. Một lỗi phổ biến là chỉ đếm các dãy có giá trị lớn nhất bằng`2`, quên mức tối đa đó`1`cũng có thể. 

Coi như```
2
1 100000
```Vị trí đầu tiên buộc phải là`1`. Vị trí thứ hai có thể có nhiều giá trị. Bất kỳ cách tiếp cận nào giả định tất cả các vị trí đều có quyền tự do tương tự sẽ được tính quá mức. Mức đóng góp tối đa của mỗi ứng cử viên phụ thuộc vào số lượng chỉ số thực sự có thể đạt tới mức đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê mọi trình tự có thể`b`, tính LCM và giá trị lớn nhất của nó rồi kiểm tra xem chúng có khớp không. 

Điều này đúng vì nó tuân theo định nghĩa theo đúng nghĩa đen. Thật không may, ngay cả khi tất cả`a_i = 10`, đã có rồi$10^n$các trình tự có thể. Với$n = 10^5$, vũ lực là vô vọng. 

Ý tưởng tiếp theo là cố định giá trị chung$$m=\operatorname{lcm}(b)=\max(b).$$Đối với một cố định`m`, mọi giá trị được chọn phải là ước số của`m`. Ngoài ra, ít nhất một giá trị được chọn phải chính xác`m`. 

Bây giờ vấn đề trở thành phép gán đếm dưới ràng buộc số chia. 

Giả sử chúng ta biết tất cả các ước của`m`theo thứ tự tăng dần:$$d_1 < d_2 < \dots < d_k=m.$$Đối với một vị trí có giới hạn là`x`, số lựa chọn được phép bằng số ước của`m`điều đó không vượt quá`x`. 

Điều này chuyển đổi vấn đề thành một sản phẩm đếm. 

Thách thức còn lại là đánh giá các sản phẩm này một cách hiệu quả cho mọi`m ≤ 100000`. 

Quan sát quan trọng là số lượng ước số không vượt quá`x`chỉ thay đổi khi`x`đi qua một ước của`m`. 

Nếu như$$d_r \le x < d_{r+1},$$thì chính xác`r`ước số của`m`có sẵn. 

Thay vì xử lý từng vị trí mảng một cách riêng biệt, chúng ta đếm xem có bao nhiêu`a_i`rơi vào từng khoảng giữa các ước số liên tiếp. Vì tất cả các giá trị lớn nhất`100000`, một mảng tần số tiền tố cho phép chúng ta thu được các số đếm này trong thời gian không đổi. 

Tổng số lần xuất hiện ước số trong số tất cả các số nguyên lên đến`100000`là$$\sum_{i=1}^{100000}\tau(i)\approx 1.17\times10^6,$$có thể dễ dàng quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`n`| O(n) | Quá chậm | 
| Tối ưu | O(M log M) ở đâu`M = max(a)`| O(M log M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và tính bảng tần số các giá trị. 
2. Xây dựng mảng đếm tiền tố`pref`, Ở đâu`pref[x]`bằng số lượng chỉ số với`a_i ≤ x`. 
3. Tính toán trước danh sách ước số cho mọi số nguyên lên đến`100000`dùng sàng chia. 
4. Hãy để`m`là giá trị ứng viên của cả LCM và giá trị lớn nhất. 
5. Lấy các ước số đã sắp xếp của`m`:$$d_1<d_2<\dots<d_k=m.$$6. Đối với mỗi khoảng thời gian$$[d_r,\ d_{r+1}-1],$$đếm xem có bao nhiêu phần tử mảng nằm trong nó:$$E_r = \text{pref}[d_{r+1}-1] - \text{pref}[d_r-1].$$Mỗi vị trí như vậy có chính xác`r`các ước số có sẵn của`m`. 
7. Nhân lên$$\prod_{r=1}^{k-1} r^{E_r}.$$Điều này tính các lựa chọn cho tất cả các vị trí có giới hạn trên nằm dưới`m`. 
8. Hãy để$$C=n-\text{pref}[m-1].$$Đây chính xác là những vị trí có`a_i ≥ m`. 

Mỗi người trong số họ có thể sử dụng bất kỳ`k`ước số của`m`. 
9. Trong số những bài tập đó, hãy trừ những bài không bao giờ sử dụng ước số`m`. 

Số bài tập hợp lệ là$$k^C-(k-1)^C.$$Đây là bước loại trừ tiêu chuẩn. 
10. Nhân kết quả của bước 7 với kết quả của bước 9 và cộng nó vào đáp án chung. 
11. Lặp lại cho mỗi lần`m`từ`1`ĐẾN`100000`. 

### Tại sao nó hoạt động 

Sửa một giá trị`m`. 

Mọi chuỗi tốt có LCM và cực đại bằng nhau`m`phải thỏa mãn hai điều kiện. Mỗi phần tử phải chia`m`, nếu không thì LCM vượt quá`m`. Ít nhất một phần tử phải bằng`m`, nếu không thì giá trị cực đại nhỏ hơn`m`. 

Sự phân tách khoảng đếm chính xác có bao nhiêu ước của`m`có sẵn ở mỗi vị trí. Tích số trong các khoảng thời gian sẽ đếm tất cả các phép gán bằng cách sử dụng các ước số của`m`. yếu tố$$k^C-(k-1)^C$$giữ chính xác các phép gán trong đó có ít nhất một vị trí sử dụng số chia`m`. 

Mỗi chuỗi tốt đóng góp vào đúng một giá trị của`m`, cụ thể là LCM chung và cực đại của nó. Các giá trị khác nhau của`m`tạo ra các tập hợp trình tự rời rạc. Tổng hợp tất cả các đóng góp sẽ tính mỗi chuỗi tốt chính xác một lần. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MOD = 1000000007
MAXV = 100000

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    freq = [0] * (MAXV + 1)
    for x in a:
        freq[x] += 1

    pref = [0] * (MAXV + 1)
    for i in range(1, MAXV + 1):
        pref[i] = pref[i - 1] + freq[i]

    divisors = [[] for _ in range(MAXV + 1)]
    for d in range(1, MAXV + 1):
        for multiple in range(d, MAXV + 1, d):
            divisors[multiple].append(d)

    max_tau = max(len(divisors[i]) for i in range(1, MAXV + 1))

    pw = [None] * (max_tau + 1)
    pw[0] = array('I', [0] * (n + 1))

    for base in range(1, max_tau + 1):
        arr = array('I', [0] * (n + 1))
        arr[0] = 1
        for e in range(1, n + 1):
            arr[e] = (arr[e - 1] * base) % MOD
        pw[base] = arr

    ans = 0

    for m in range(1, MAXV + 1):
        ds = divisors[m]
        k = len(ds)

        cur = 1

        for r in range(1, k):
            left = ds[r - 1]
            right = ds[r] - 1

            cnt = pref[right] - pref[left - 1]
            cur = (cur * pw[r][cnt]) % MOD

        c = n - pref[m - 1]

        add = pw[k][c]
        if k > 1:
            add -= pw[k - 1][c]

        cur = (cur * add) % MOD
        ans += cur

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Mảng tần số và mảng tiền tố cho phép chúng ta trả lời các truy vấn đếm phạm vi trong thời gian không đổi. Mỗi khoảng giữa các ước số liên tiếp trở thành phép trừ đơn giản của hai giá trị tiền tố. 

Sàng chia là tiêu chuẩn$$O(M \log M)$$sự thi công. Mỗi số nguyên được chèn vào danh sách chia của bội số của nó. 

Bảng lũy ​​thừa được lập chỉ mục theo thứ hạng chia thay vì theo giá trị. Số ước của bất kỳ số nào lên đến`100000`không bao giờ vượt quá`128`, do đó chỉ một số lượng nhỏ cơ số phải được tính toán trước. Điều này tránh thực hiện hơn một triệu phép tính lũy thừa mô-đun bên trong vòng lặp chính. 

Phần tế nhị nhất chính là yếu tố$$k^C-(k-1)^C.$$Nó đếm các phép gán trong đó có ít nhất một vị trí sử dụng số chia`m`. Việc quên phép trừ này sẽ đếm nhiều dãy có giá trị cực đại thực sự nhỏ hơn`m`. 

Một trường hợp tế nhị khác là`m = 1`. Sau đó`k = 1`, và số hạng trừ phải bằng 0. Mã xử lý việc này một cách riêng biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
1 4 3 2
```Sau khi sắp xếp theo khái niệm, các giới hạn trên được phân bổ như sau: 

| Giá trị | Đếm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Vì`m = 2`: 

Số chia là`[1, 2]`. 

| Khoảng thời gian | Số chia có sẵn | Các phần tử trong khoảng | 
| --- | --- | --- | 
| [1,1] | 1 | 1 | 
| ≥ 2 | được xử lý bởi C | 3 | 

Như vậy$$1^1 \cdot (2^3-1^3)=7.$$Vì`m = 3`: 

| Khoảng thời gian | Số chia có sẵn | Yếu tố | 
| --- | --- | --- | 
| [1,2] | 1 | 2 | 
| ≥ 3 | được xử lý bởi C | 2 | 

Sự đóng góp:$$1^2 \cdot (2^2-1^2)=3.$$Vì`m = 4`: 

Ước số`[1,2,4]`. 

| Khoảng thời gian | Đếm | 
| --- | --- | 
| [1,1] | 1 | 
| [2,3] | 2 | 
| ≥4 | 1 | 

Sự đóng góp:$$1^1\cdot2^2\cdot(3^1-2^1)=4.$$Thêm tất cả các đóng góp mang lại`15`. 

Ví dụ này chứng minh rằng các giá trị khác nhau của`m`đóng góp độc lập và được tóm tắt đơn giản. 

### Ví dụ 2 

đầu vào:```
2
2 2
```Vì`m = 1`: 

| Số lượng | Giá trị | 
| --- | --- | 
| C | 2 | 
| Đóng góp |$1^2 = 1$| 

Vì`m = 2`: 

| Số lượng | Giá trị | 
| --- | --- | 
| C | 2 | 
| Đóng góp |$2^2-1^2=3$| 

Tổng số câu trả lời:$$1+3=4.$$Bốn chuỗi được tính chính xác```
(1,1)
(1,2)
(2,1)
(2,2)
```xác nhận bước bao gồm-loại trừ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log M) | Sàng chia cộng với xử lý tất cả các lần xuất hiện của số chia | 
| Không gian | O(M log M) | Lưu trữ danh sách ước số và bảng lũy ​​thừa | 

Đây`M = 100000`. 

Tổng số lần xuất hiện của số chia lên đến`100000`đại khái là`1.17 million`, giúp giữ vòng lặp chính thoải mái trong giới hạn. Việc sử dụng bộ nhớ cũng phù hợp với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    from array import array

    MOD = 1000000007
    MAXV = 100000

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    freq = [0] * (MAXV + 1)
    for x in a:
        freq[x] += 1

    pref = [0] * (MAXV + 1)
    for i in range(1, MAXV + 1):
        pref[i] = pref[i - 1] + freq[i]

    divisors = [[] for _ in range(MAXV + 1)]
    for d in range(1, MAXV + 1):
        for m in range(d, MAXV + 1, d):
            divisors[m].append(d)

    max_tau = max(len(divisors[i]) for i in range(1, MAXV + 1))

    pw = [None] * (max_tau + 1)
    pw[0] = array('I', [0] * (n + 1))

    for base in range(1, max_tau + 1):
        arr = array('I', [0] * (n + 1))
        arr[0] = 1
        for e in range(1, n + 1):
            arr[e] = (arr[e - 1] * base) % MOD
        pw[base] = arr

    ans = 0

    for m in range(1, MAXV + 1):
        ds = divisors[m]
        k = len(ds)

        cur = 1

        for r in range(1, k):
            cnt = pref[ds[r] - 1] - pref[ds[r - 1] - 1]
            cur = (cur * pw[r][cnt]) % MOD

        c = n - pref[m - 1]

        add = pw[k][c]
        if k > 1:
            add -= pw[k - 1][c]

        ans = (ans + cur * add) % MOD

    return str(ans) + "\n"

# provided sample
assert run("4\n1 4 3 2\n") == "15\n"

# minimum size
assert run("1\n1\n") == "1\n"

# all equal
```

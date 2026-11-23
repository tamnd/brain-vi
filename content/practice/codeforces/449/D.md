---
title: "CF 449D - Jzzhu và số"
description: "Cho ta một dãy n số nguyên không âm. Một nhóm là bất kỳ tập hợp con không trống nào của các chỉ mục mảng. Đối với một nhóm được chọn, chúng tôi lấy AND theo bit của tất cả các giá trị tại các chỉ mục đó. Nhiệm vụ là đếm xem có bao nhiêu nhóm không trống có bit AND bằng 0."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 449
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 257 (Div. 1)"
rating: 2400
weight: 449
solve_time_s: 114
verified: true
draft: false
---

[CF 449D - Jzzhu và các con số](https://codeforces.com/problemset/problem/449/D) 

**Đánh giá:** 2400 
**Tags:** bitmasks, tổ hợp, dp 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một loạt`n`số nguyên không âm. Một nhóm là bất kỳ tập hợp con không trống nào của các chỉ mục mảng. Đối với một nhóm được chọn, chúng tôi lấy AND theo bit của tất cả các giá trị tại các chỉ mục đó. 

Nhiệm vụ là đếm xem có bao nhiêu nhóm không trống có bit AND bằng`0`. Câu trả lời phải được báo cáo modulo`1000000007`. 

Chi tiết quan trọng là chúng tôi đang chọn chỉ số chứ không phải giá trị. Nếu cùng một số xuất hiện nhiều lần thì các lựa chọn chỉ mục khác nhau sẽ được tính là các nhóm khác nhau. 

Các ràng buộc hoàn toàn xác định loại giải pháp chúng ta cần. Độ dài mảng có thể lớn bằng`10^6`, điều này ngay lập tức loại trừ mọi thứ kiểm tra trực tiếp các tập hợp con. có`2^n - 1`các nhóm không trống, một số lượng lớn về mặt thiên văn. 

Ràng buộc thú vị hơn là phạm vi giá trị. Mọi`a[i]`nhiều nhất là`10^6`, phù hợp trong phạm vi 20 bit vì:$$2^{20} = 1048576$$Điều này có nghĩa là mọi số có thể được xem dưới dạng mặt nạ tối đa 20 bit. Không gian trạng thái có kích thước`2^20 ≈ 10^6`lớn nhưng có thể quản lý được. Bất cứ khi nào có vấn đề lớn`n`nhưng độ rộng bit nhỏ, nó thường là tín hiệu cho thấy chúng ta nên làm việc trong không gian mặt nạ thay vì không gian tập hợp con. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
1
0
```Nhóm không trống duy nhất là`{0}`và AND của nó bằng`0`, vậy câu trả lời là`1`. Giải pháp quên loại trừ tập hợp con trống vào đúng thời điểm có thể tạo ra số đếm sai. 

Coi như:```
2
1 1
```Các nhóm có thể là`{1}`,`{1}`, Và`{1,1}`. AND của mọi nhóm đều bằng`1`, không bao giờ`0`, vậy câu trả lời là`0`. Việc đếm mặt nạ thay vì tập hợp con chỉ mục có thể vô tình hợp nhất các giá trị trùng lặp và đếm thiếu hoặc đếm thừa. 

Coi như:```
3
0 7 7
```Bất kỳ nhóm nào chứa phần tử 0 đều có AND bằng`0`. Có bốn nhóm như vậy:```
{0}
{0,7(first)}
{0,7(second)}
{0,7(first),7(second)}
```Câu trả lời đúng là`4`. Điều này minh họa tại sao bội số chỉ số lại quan trọng. 

Một tình huống khó phát hiện khác là khi không có số riêng lẻ nào bằng 0 nhưng có sự kết hợp nào đó tạo ra số 0:```
2
1 2
```Các nhóm đơn có giá trị AND`1`Và`2`, nhưng cặp này có:```
1 & 2 = 0
```Câu trả lời là`1`. Chỉ nhìn vào các giá trị riêng lẻ là không đủ. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là liệt kê mọi tập hợp con không trống của các chỉ số, tính AND của tất cả các số đã chọn và đếm xem có bao nhiêu kết quả bằng 0. 

Điều này đúng vì nó kiểm tra chính xác định nghĩa của một nhóm hợp lệ. Thật không may, có`2^n - 1`các nhóm không trống. Ngay cả đối với`n = 50`, điều này đã không thể thực hiện được. Với`n = 10^6`, cách tiếp cận này là hoàn toàn không thể. 

Để tìm ra giải pháp tốt hơn, chúng ta cần suy nghĩ về ý nghĩa của việc AND của một tập hợp con chứa các bit nhất định. 

Giả sử chúng ta sửa một chiếc mặt nạ`m`. Hãy đếm xem có bao nhiêu phần tử mảng chứa tất cả các bit của`m`. Nói cách khác:```
(a[i] & m) = m
```Nếu AND của một tập hợp con chứa tất cả các bit của`m`, thì mọi phần tử trong tập hợp con đó phải chứa tất cả các bit của`m`. 

Điều này biến bài toán thành việc đếm các tập con rút ra từ các số là tập con của mặt nạ. 

Cho phép:```
cnt[m]
```là số phần tử mảng có mặt nạ là tập hợp siêu của`m`. 

Khi đó số tập con không trống có AND chứa`m`bằng:```
2^(cnt[m]) - 1
```bởi vì bất kỳ lựa chọn nào không trống trong số đó`cnt[m]`các phần tử bảo toàn từng bit của`m`. 

Bây giờ chúng ta có một tình huống bao gồm-loại trừ cổ điển. 

Định nghĩa:```
F(m) = number of non-empty subsets whose AND contains m
```Chúng tôi muốn:```
G(0) = number of non-empty subsets whose AND is exactly 0
```Sử dụng nghịch đảo Möbius trên mạng tập hợp con:```
G(0) = Σ (-1)^(popcount(m)) * F(m)
```trong đó tổng chạy trên tất cả các mặt nạ. 

Thách thức còn lại là tính toán`cnt[m]`cho mỗi chiếc mặt nạ. Đây chính xác là vấn đề SOS DP (Tổng trên tập hợp con DP). Bắt đầu từ việc đếm tần số của các giá trị chính xác, chúng tôi tính toán cho mỗi mặt nạ số phần tử mảng có giá trị là tập hợp siêu của mặt nạ đó. 

Toàn bộ giải pháp trở thành sự kết hợp của: 

1. Đếm tần suất đeo khẩu trang. 
2. SOS DP để lấy số lượng superset. 
3. Loại trừ bao gồm tất cả các mặt nạ. 

### So sánh phương pháp tiếp cận 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(1) | Quá chậm | 
| Tối ưu | O(20 · 2^20 + n) | O(2^20) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Đếm số lần xuất hiện của mỗi mặt nạ chính xác 

Tạo một mảng`freq`kích thước`2^20`. 

Với mỗi số đầu vào`x`, tăng:```
freq[x]
```Điều này ghi lại số lần mỗi mặt nạ chính xác xuất hiện. 

### 2. Xây dựng số lượng superset bằng SOS DP 

Tạo:```
dp = freq
```Ban đầu`dp[m]`đếm các phần tử bằng`m`. 

Chúng tôi muốn:```
dp[m] = number of elements whose masks are supersets of m
```Đối với mỗi bit từ`0`ĐẾN`19`và với mọi mặt nạ: 

Nếu bit không được đặt trong mặt nạ, hãy cộng số lượng mặt nạ khi bit đó được bật. 

Chính thức:```
dp[mask] += dp[mask | (1 << bit)]
```Sau tất cả 20 lần vượt qua,`dp[m]`trở thành số phần tử mảng thỏa mãn:```
(value & m) = m
```### 3. Tính trước lũy thừa của 2 

Đối với mọi`i`từ`0`ĐẾN`n`tính toán:```
pow2[i] = 2^i mod MOD
```Chúng tôi sẽ thường xuyên cần:```
2^(dp[m]) - 1
```đó là số tập hợp con không trống được hình thành từ các phần tử đó. 

### 4. Áp dụng bao gồm-loại trừ 

Đối với mỗi mặt nạ:```
ways = 2^(dp[mask]) - 1
```Điều này đếm các tập con không trống có AND chứa tất cả các bit của`mask`. 

Nếu mặt nạ có số bit được đặt chẵn, hãy thêm`ways`. 

Nếu mặt nạ có số bit được đặt là lẻ, hãy trừ đi`ways`. 

Về mặt toán học:```
answer += (-1)^(popcount(mask)) * ways
```Tất cả các hoạt động được thực hiện modulo`1000000007`. 

### 5. Chuẩn hóa kết quả 

Tổng bao gồm loại trừ có thể trở thành số âm trong quá trình tính toán. 

Trở lại:```
(answer % MOD + MOD) % MOD
```### Tại sao nó hoạt động 

Sau SOS DP,`dp[m]`đếm chính xác các phần tử mảng có mặt nạ chứa mọi bit`m`. Bất kỳ tập hợp con không trống nào được chọn từ các phần tử đó đều có AND cũng chứa mọi bit của`m`, cho`2^(dp[m]) - 1`tập hợp con. 

Mỗi tập hợp con có AND bằng một mặt nạ nào đó`A`góp phần vào mọi`F(m)`Ở đâu`m`là một mặt nạ con của`A`. Loại trừ bao gồm trên mạng Boolean sẽ loại bỏ việc đếm quá mức và cô lập các tập hợp con có AND chính xác bằng 0. Đây chính xác là công thức đảo ngược Möbius trên các mặt nạ tập hợp con, đảm bảo tổng cuối cùng bằng số nhóm có AND là`0`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007
BITS = 20
MAX_MASK = 1 << BITS

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    freq = [0] * MAX_MASK
    for x in arr:
        freq[x] += 1

    dp = freq[:]

    for bit in range(BITS):
        for mask in range(MAX_MASK):
            if (mask & (1 << bit)) == 0:
                dp[mask] += dp[mask | (1 << bit)]

    pow2 = [1] * (n + 1)
    for i in range(1, n + 1):
        pow2[i] = (pow2[i - 1] * 2) % MOD

    ans = 0

    for mask in range(MAX_MASK):
        ways = pow2[dp[mask]] - 1

        if mask.bit_count() & 1:
            ans -= ways
        else:
            ans += ways

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng bảng tần số của mặt nạ chính xác. Vì giá trị không bao giờ vượt quá`10^6`, phân bổ một mảng có kích thước`2^20`là khả thi. 

SOS DP chuyển đổi tần số chính xác thành tần số siêu tập hợp. Khi xử lý một bit, mỗi mặt nạ sẽ nhận được sự đóng góp từ mặt nạ có chứa một bit bắt buộc bổ sung. Sau khi tất cả 20 thứ nguyên được xử lý, mỗi trạng thái chứa số phần tử mảng là tập hợp siêu của mặt nạ đó. 

Bảng công suất cho phép đánh giá thời gian liên tục của`2^k`. Từ`k`có thể lớn như`n`, tính toán trước một lần nhanh hơn nhiều so với tính lũy thừa lặp lại. 

Vòng lặp cuối cùng thực hiện đảo ngược Möbius. Mặt nạ chẵn lẻ đóng góp tích cực và mặt nạ chẵn lẻ đóng góp tiêu cực. Tổng kết quả chính xác là số lượng các nhóm không trống có AND bằng 0. 

Một lỗi triển khai phổ biến là cố gắng sử dụng loại trừ bao gồm trên các tần số chính xác thay vì số lượng siêu tập hợp. Công thức đảo ngược chỉ hoạt động sau khi tính toán các giá trị SOS DP. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
2 3 3
```Các mặt nạ là:```
2 = 10
3 = 11
3 = 11
```Các trạng thái liên quan: 

| Mặt nạ | Nhị phân | dp[mặt nạ] | 
| --- | --- | --- | 
| 0 | 00 | 3 | 
| 1 | 01 | 2 | 
| 2 | 10 | 3 | 
| 3 | 11 | 2 | 

Bây giờ tính toán đóng góp: 

| Mặt nạ | Số lượng | 2^(dp)-1 | Ký tên | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 7 | + | +7 | 
| 1 | 1 | 3 | - | -3 | 
| 2 | 1 | 7 | - | -7 | 
| 3 | 2 | 3 | + | +3 | 

Tổng cộng:```
7 - 3 - 7 + 3 = 0
```Trả lời:```
0
```Điều này chứng tỏ rằng mọi tập hợp con không trống đều có giá trị AND dương, do đó, việc loại trừ bao gồm sẽ hủy bỏ mọi thứ. 

### Ví dụ 2 

đầu vào:```
2
1 2
```Mặt nạ:```
1 = 01
2 = 10
```Kết quả SOS: 

| Mặt nạ | Nhị phân | dp[mặt nạ] | 
| --- | --- | --- | 
| 0 | 00 | 2 | 
| 1 | 01 | 1 | 
| 2 | 10 | 1 | 
| 3 | 11 | 0 | 

Đóng góp: 

| Mặt nạ | Số lượng | 2^(dp)-1 | Ký tên | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 3 | + | +3 | 
| 1 | 1 | 1 | - | -1 | 
| 2 | 1 | 1 | - | -1 | 
| 3 | 2 | 0 | + | 0 | 

Tổng cộng:```
3 - 1 - 1 = 1
```Tập hợp con hợp lệ duy nhất là:```
{1, 2}
```có AND bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 20 · 2^20) | Đếm tần số cộng với SOS DP và loại trừ bao gồm | 
| Không gian | O(2^20) | Mảng tần số và DP | 

Chi phí chủ yếu là SOS DP. Có 20 vị trí bit và`2^20`mặt nạ, mang lại khoảng 20 triệu lần chuyển đổi. Điều này thoải mái phù hợp với giới hạn Codeforces điển hình trong Python được tối ưu hóa. Việc sử dụng bộ nhớ cũng có thể chấp nhận được vì các mảng có kích thước khoảng một triệu nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 1000000007

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    arr = list(map(int, input().split()))

    BITS = 20
    MAX_MASK = 1 << BITS

    freq = [0] * MAX_MASK
    for x in arr:
        freq[x] += 1

    dp = freq[:]

    for bit in range(BITS):
        for mask in range(MAX_MASK):
            if (mask & (1 << bit)) == 0:
                dp[mask] += dp[mask | (1 << bit)]

    pow2 = [1] * (n + 1)
    for i in range(1, n + 1):
        pow2[i] = (pow2[i - 1] * 2) % MOD

    ans = 0
    for mask in range(MAX_MASK):
        ways = pow2[dp[mask]] - 1
        if mask.bit_count() & 1:
            ans -= ways
        else:
            ans += ways

    return str(ans % MOD)

# provided sample
assert run("3\n2 3 3\n") == "0", "sample 1"

# single zero
assert run("1\n0\n") == "1", "single element equal to zero"

# single non-zero
assert run("1\n5\n") == "0", "single non-zero element"

# pair whose AND becomes zero
assert run("2\n1 2\n") == "1", "only the pair works"

# all zeros
assert run("3\n0 0 0\n") == "7", "every non-empty subset works"

# duplicate values
assert run("2\n1 1\n") == "0", "duplicates with non-zero AND"

# mixed case
assert run("3\n0 7 7\n") == "4", "all subsets containing zero"
```### Tóm tắt bài kiểm tra 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`1`| Câu trả lời tích cực hợp lệ nhỏ nhất | 
|`1 / 5`|`0`| Phần tử đơn khác 0 | 
|`2 / 1 2`|`1`| AND chỉ trở thành 0 sau khi kết hợp | 
|`3 / 0 0 0`|`7`| Mọi tập hợp con không trống đều hợp lệ | 
|`2 / 1 1`|`0`| Các giá trị trùng lặp được xử lý bởi các chỉ số | 
|`3 / 0 7 7`|`4`| Đếm đúng với các giá trị lặp lại và số 0 | 

## Vỏ cạnh 

### Một số 0 duy nhất 

đầu vào:```
1
0
```SOS DP cung cấp:```
dp[0] = 1
```Tất cả các mặt nạ khác đều có số 0. Bao gồm-loại trừ đánh giá:```
2^1 - 1 = 1
```Đầu ra của thuật toán`1`, tương ứng với nhóm duy nhất có thể. 

### Giá trị trùng lặp 

đầu vào:```
2
1 1
```Có ba nhóm không trống:```
{first}
{second}
{first, second}
```AND của mọi nhóm đều bằng`1`. Bảng tần số lưu trữ bội số, do đó SOS DP tính riêng cả hai lần xuất hiện. Bao gồm-loại trừ cuối cùng tạo ra`0`, điều đó đúng. 

### Số 0 chỉ được tạo ra bằng cách kết hợp các phần tử 

đầu vào:```
2
1 2
```Cả nhóm đơn đều không có AND bằng 0:```
1
2
```Cặp đôi này mang lại:```
1 & 2 = 0
```Thuật toán đếm các tập hợp con thông qua tần số siêu tập hợp thay vì kiểm tra các giá trị riêng lẻ. Loại trừ bao gồm tách biệt chính xác một tập hợp con có AND bằng 0, tạo ra câu trả lời đúng`1`. 

### Tất cả các phần tử đều bằng 0 

đầu vào:```
3
0 0 0
```Mọi tập con không trống đều có AND bằng 0. Có:```
2^3 - 1 = 7
```các tập hợp con như vậy. Vì mọi giá trị đều bằng 0,`dp[0] = 3`và tất cả các mặt nạ khác đều có số lượng bằng 0. Tổng bao gồm-loại trừ ước tính là`7`, khớp chính xác với số tổ hợp.

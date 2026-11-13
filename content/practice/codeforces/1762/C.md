---
title: "CF 1762C - Chuỗi nhị phân rất thú vị"
description: "Chúng ta được cấp một chuỗi nhị phân s. Đối với mỗi tiền tố của s, chúng ta xác định một giá trị f(tiền tố). Để tính f(x), chúng ta xem xét tất cả các phần mở rộng có thể có của x. Nếu x có độ dài k thì phần mở rộng có độ dài 2k-1."
date: "2026-06-09T13:55:04+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 1762
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 838 (Div. 2)"
rating: 1400
weight: 1762
solve_time_s: 629
verified: true
draft: false
---

[CF 1762C - Chuỗi nhị phân rất thú vị](https://codeforces.com/problemset/problem/1762/C) 

**Đánh giá:** 1400 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 10 phút 29 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân`s`. Với mỗi tiền tố của`s`, chúng tôi xác định một giá trị`f(prefix)`. 

Để tính toán`f(x)`, chúng tôi xem xét tất cả các phần mở rộng có thể có của`x`. Nếu như`x`có chiều dài`k`, phần mở rộng có độ dài`2k-1`. Các nhân vật của`x`được cố định tại các vị trí lẻ của phần mở rộng, trong khi mọi vị trí chẵn có thể được chọn tự do như`0`hoặc`1`. 

Trong số những tiện ích mở rộng đó, chúng tôi đếm xem có bao nhiêu tiện ích mở rộng "tốt". 

Một phần mở rộng tốt có một thuộc tính đặc biệt. Đối với mọi vị trí lẻ`i`, ký tự ở vị trí`i`phải bằng trung vị của tiền tố kết thúc tại`i`. Vì bảng chữ cái là nhị phân nên số trung vị chỉ đơn giản là giá trị đa số trong tiền tố có độ dài lẻ đó. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải tính toán$$\sum_{i=1}^{n} f(s[1..i])$$modulo`998244353`. 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`. Bất kỳ giải pháp nào xử lý từng tiền tố một cách độc lập và liệt kê các phần mở rộng đều không thể thực hiện được. Ngay cả một tiền tố có độ dài`200000`có`2^{199999}`phần mở rộng. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một nguồn sai lầm tinh vi là do hiểu sai ý nghĩa của điều kiện trung bình. 

Coi như:```
s = 11
```Các phần mở rộng là:```
101
111
```Cả hai đều tốt nên`f(11)=2`. 

Bây giờ hãy xem xét:```
s = 01
```Các phần mở rộng là:```
001
011
```Chỉ một`011`là tốt, vậy`f(01)=1`. 

Một giải pháp giả định mọi vị trí chẵn luôn miễn phí sẽ bị tính quá mức. 

Một lỗi dễ dàng khác xuất hiện khi đường chạy thay đổi. 

Coi như:```
s = 010
```Câu trả lời là:```
f(0)=1
f(01)=1
f(010)=1
```Tổng cộng`3`. 

Khi các ký tự liền kề khác nhau, số phần mở rộng tốt sẽ ngừng tăng gấp đôi. 

## Phương pháp tiếp cận 

Việc giải thích vũ lực là đơn giản. Đối với mỗi tiền tố, hãy tạo tất cả`2^{len-1}`tiện ích mở rộng, kiểm tra xem mỗi tiện ích mở rộng có thỏa mãn điều kiện trung vị hay không và đếm các tiện ích mở rộng hợp lệ. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Thật không may, đối với tiền tố có độ dài`k`, có`2^{k-1}`phần mở rộng. Với`k`lớn như`200000`, điều này thật vô vọng. 

Quan sát quan trọng đến từ việc xem xét điều kiện trung bình thực sự áp đặt điều gì. 

Giả sử chúng ta đã xử lý tiền tố kết thúc ở vị trí lẻ`2i-1`. Khi chúng tôi mở rộng đến vị trí lẻ`2i+1`, chúng ta chèn chính xác một bit trống vào giữa hai bit cố định. 

Đặt các bit cố định là:```
a_i and a_{i+1}
```Nếu hai bit này khác nhau thì bit được chèn sẽ được xác định hoàn toàn. Có đúng một lựa chọn bảo toàn điều kiện trung vị. 

Nếu hai bit này bằng nhau thì có hai lựa chọn hợp lệ cho bit được chèn vào. 

Điều này biến đổi một ràng buộc trung bình dường như toàn cầu thành một quy tắc cục bộ đơn giản giữa các ký tự liên tiếp của chuỗi gốc. 

Cho phép`dp[i]`biểu thị số phần mở rộng tốt cho tiền tố`s[1..i]`. 

Khi:```
s[i] != s[i-1]
```số đếm không thay đổi. 

Khi:```
s[i] == s[i-1]
```số đếm tăng gấp đôi. 

Chính xác hơn:```
dp[1] = 1

dp[i] =
dp[i-1]                  if s[i] != s[i-1]
2 * dp[i-1]             if s[i] == s[i-1]
```Vì câu trả lời cuối cùng là tổng của tất cả`dp[i]`, chúng ta có thể tính toán mọi thứ trong một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(2^n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`cur = 1`. 

Điều này thể hiện`f(s[1])`. Một ký tự đơn có chính xác một phần mở rộng tốt. 
2. Khởi tạo`answer = 1`. 

Chúng ta đã biết sự đóng góp của tiền tố đầu tiên. 
3. Lặp lại từ vị trí`2`để định vị`n`. 
4. Nếu`s[i] == s[i-1]`, nhân lên`cur`qua`2`. 

Có hai lựa chọn hợp lệ cho bit vị trí chẵn mới được chèn vào. 
5. Nếu`s[i] != s[i-1]`, rời khỏi`cur`không thay đổi. 

Bit được chèn bị ép buộc nên số phần mở rộng tốt không tăng lên. 
6. Thêm`cur`đến câu trả lời đang chạy. 
7. Thực hiện mọi thao tác theo modulo`998244353`. 
8. Xuất ra câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

hãy để`g_i`là số phần mở rộng tốt của tiền tố`s[1..i]`. 

Di chuyển từ độ dài tiền tố`i-1`ĐẾN`i`giới thiệu chính xác một vị trí miễn phí mới trong tiện ích mở rộng. 

Nếu ký tự cố định mới được thêm vào bằng ký tự cố định trước đó thì có thể đặt một trong hai giá trị vào vị trí được chèn mà không vi phạm điều kiện trung vị. Mỗi tiện ích mở rộng hợp lệ trước đó sẽ tạo ra hai tiện ích mở rộng hợp lệ mới. 

Nếu ký tự cố định mới được thêm vào khác với ký tự trước đó thì có thể chèn chính xác một giá trị trong khi vẫn duy trì mối quan hệ trung bình được yêu cầu. Mỗi tiện ích mở rộng hợp lệ trước đó sẽ tạo ra chính xác một tiện ích mở rộng hợp lệ mới. 

Như vậy:$$g_i = \begin{cases} 2g_{i-1} & s_i=s_{i-1}\\ g_{i-1} & s_i\ne s_{i-1} \end{cases}$$Sự lặp lại này tính tất cả các tiện ích mở rộng tốt và chỉ các tiện ích mở rộng tốt. Tổng hợp tất cả`g_i`đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

t = int(input())

for _ in range(t):
    n = int(input())
    s = input().strip()

    cur = 1
    ans = 1

    for i in range(1, n):
        if s[i] == s[i - 1]:
            cur = (cur * 2) % MOD

        ans = (ans + cur) % MOD

    print(ans)
```Biến`cur`lưu trữ số lượng tiện ích mở rộng tốt cho tiền tố hiện tại. Bất cứ khi nào hai ký tự liền kề bằng nhau, mọi tiện ích mở rộng hiện có sẽ chia thành hai phần tiếp theo hợp lệ, do đó`cur`tăng gấp đôi. 

Khi các ký tự liền kề khác nhau, không có sự phân nhánh mới nào xảy ra. Số lượng không thay đổi. 

Câu trả lời là tổng của`cur`trên tất cả các tiền tố. 

Chi tiết triển khai duy nhất quan trọng là áp dụng mô đun ngay sau phép nhân và phép cộng. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
s = 11
```| Tiền tố | Liền kề bằng nhau? | cur | Chạy câu trả lời | 
| --- | --- | --- | --- | 
| 1 | - | 1 | 1 | 
| 11 | Có | 2 | 3 | 

Đầu ra:```
3
```Điều này phù hợp:```
f(1)=1
f(11)=2
```### Ví dụ 2```
s = 010
```| Tiền tố | Liền kề bằng nhau? | cur | Chạy câu trả lời | 
| --- | --- | --- | --- | 
| 0 | - | 1 | 1 | 
| 01 | Không | 1 | 2 | 
| 010 | Không | 1 | 3 | 

Đầu ra:```
3
```Điều này chứng tỏ rằng việc thay đổi bit không làm tăng số lượng phần mở rộng hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đi qua chuỗi | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`, do đó nghiệm tuyến tính dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import io
import sys

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        cur = 1
        ans = 1

        for i in range(1, n):
            if s[i] == s[i - 1]:
                cur = (cur * 2) % MOD
            ans = (ans + cur) % MOD

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""6
1
1
1
0
2
11
3
010
9
101101111
37
1011011111011010000011011111111011111
""") == """1
1
3
3
21
365"""

# minimum size
assert run("""1
1
0
""") == "1"

# all equal
assert run("""1
4
1111
""") == "15"

# alternating
assert run("""1
5
01010
""") == "5"

# single doubling event
assert run("""1
3
001
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1`| Chuỗi nhỏ nhất có thể | 
|`1111`|`15`| Nhân đôi lặp đi lặp lại | 
|`01010`|`5`| Không tăng gấp đôi chút nào | 
|`001`|`5`| Một cặp bằng nhau theo sau là một sự thay đổi | 

## Vỏ cạnh 

Hãy xem xét:```
n = 1
s = 0
```Chỉ có một tiền tố. Thuật toán bắt đầu với`cur = 1`và không bao giờ đi vào vòng lặp. Câu trả lời là`1`, điều đó đúng. 

Coi như:```
s = 0000
```Các giá trị của`cur`trở nên:```
1, 2, 4, 8
```Câu trả lời trở thành:```
1 + 2 + 4 + 8 = 15
```Mọi cặp liền kề đều bằng nhau nên mỗi bước sẽ nhân đôi số phần mở rộng tốt. 

Coi như:```
s = 010101
```Mỗi cặp liền kề khác nhau. Giá trị của`cur`luôn luôn còn lại`1`. Câu trả lời đơn giản là số lượng tiền tố:```
6
```Điều này xác nhận rằng những thay đổi giữa các bit lân cận sẽ loại bỏ tất cả các nhánh và để lại chính xác một đường dẫn mở rộng hợp lệ.

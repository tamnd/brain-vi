---
title: "CF 1183E - Dãy số (bản dễ)"
description: "Chúng tôi bắt đầu bằng một chuỗi và muốn xây dựng một tập hợp chứa chính xác k chuỗi con riêng biệt. Việc thêm một dãy con có độ dài L sẽ làm mất số lượng ký tự bị loại bỏ, là n - L. Cùng một dãy con không thể được thêm hai lần."
date: "2026-06-12T01:22:43+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "implementation", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1183
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 570 (Div. 3)"
rating: 2000
weight: 1183
solve_time_s: 100
verified: true
draft: false
---

[CF 1183E - Chuỗi tiếp theo (phiên bản dễ)](https://codeforces.com/problemset/problem/1183/E) 

**Xếp hạng:** 2000 
**Tags:** dp, đồ thị, triển khai, đường dẫn ngắn nhất 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu bằng một chuỗi và muốn xây dựng một bộ chứa chính xác`k`dãy số riêng biệt. Thêm một dãy con có độ dài`L`tính số lượng ký tự bị loại bỏ, tức là`n - L`. Cùng một dãy con không thể cộng hai lần. Nhiệm vụ là giảm thiểu tổng chi phí hoặc báo cáo rằng không có đủ các chuỗi con riêng biệt. 

Cấu trúc chi phí ngay lập tức gợi ý rằng các chuỗi dài hơn sẽ thích hợp hơn. Một dãy con có độ dài`n`chi phí`0`, chiều dài`n-1`chi phí`1`, vân vân. Nếu chúng ta biết có bao nhiêu dãy con riêng biệt tồn tại cho mỗi độ dài thì chiến lược tối ưu là hiển nhiên: lấy càng nhiều dãy con càng tốt từ độ dài`n`, sau đó từ chiều dài`n-1`, và tiếp tục đi xuống cho đến khi chúng ta thu thập được`k`của họ. 

Phiên bản dễ dàng có`n ≤ 100`, cho phép lập trình động với khoảng`O(n²)`hoặc`O(n³)`hoạt động. Việc liệt kê tất cả các dãy con là không thể vì một chuỗi có độ dài`100`có`2^100`những hậu quả, vượt xa mọi giới hạn khả thi. 

Một số tình huống rất dễ xử lý sai. 

Coi như```
1 2
a
```Các dãy con riêng biệt là`"a"`Và`""`. Chi phí của họ là`0`Và`1`, vậy câu trả lời là```
1
```Giải pháp quên dãy con trống sẽ in sai`-1`. 

Một ví dụ khác là```
3 8
aaa
```Các dãy con riêng biệt là```
"aaa"
"aa"
"a"
""
```Chỉ có bốn chuỗi con khác nhau tồn tại vì nhiều mẫu xóa tạo ra các chuỗi giống hệt nhau. Đầu ra đúng là```
-1
```Việc đếm các mặt nạ xóa thay vì các chuỗi riêng biệt sẽ kết luận nhầm rằng có sẵn 8 lựa chọn. 

Ví dụ thứ ba là```
4 5
abcd
```Tất cả các nhân vật đều khác nhau. Có nhiều dãy con khác nhau và năm dãy con rẻ nhất là```
abcd  cost 0
abc   cost 1
abd   cost 1
acd   cost 1
bcd   cost 1
```Câu trả lời là```
4
```Việc chọn các chuỗi con ngắn hơn quá sớm sẽ làm tăng tổng chi phí một cách không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực tạo ra mọi chuỗi con, lưu trữ các chuỗi kết quả trong một tập hợp để loại bỏ các chuỗi trùng lặp, sắp xếp chúng theo độ dài và mất nhiều thời gian nhất`k`. Phương pháp này đúng vì nó tuân theo định nghĩa trực tiếp. Vấn đề là số lượng mặt nạ. Vì`n = 100`, có`2^100 ≈ 10^30`hậu quả, điều này làm cho phương pháp này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần các chuỗi con. Chúng ta chỉ cần biết mỗi độ dài có thể có bao nhiêu dãy con riêng biệt. 

Giả sử chúng ta xử lý chuỗi từ trái sang phải. Cho phép`dp[i][j]`biểu thị số dãy con riêng biệt có độ dài`j`có thể được hình thành bằng cách sử dụng tiền tố kết thúc ở vị trí`i`. Nếu chúng ta nối thêm`s[i]`, chúng ta phải tránh tạo các bản sao đã được tạo bởi sự xuất hiện trước đó của cùng một ký tự. Lần xuất hiện cuối cùng của mỗi ký tự cho phép chúng ta trừ đi chính xác những ký tự trùng lặp đó. 

Điều này biến một bài toán liệt kê hàm mũ thành một bài toán quy hoạch động đa thức. Khi đã biết số lượng, chúng tôi tham lam lấy các chuỗi con từ độ dài lớn hơn đến độ dài nhỏ hơn vì mỗi ký tự bị xóa bổ sung sẽ làm tăng chi phí thêm một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(2^n) | Quá chậm | 
| Tối ưu | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định`dp[i][j]`là số dãy con riêng biệt có độ dài`j`có thể được hình thành bằng cách sử dụng đầu tiên`i`nhân vật. 
2. Khởi tạo`dp[0][0] = 1`, bởi vì chuỗi trống có một chuỗi con, đó là chính nó. 
3. Xử lý từng ký tự một. Đối với vị trí`i`, đầu tiên sao chép tất cả các giá trị từ`dp[i-1][j]`, đại diện cho các chuỗi con không sử dụng ký tự mới. 
4. Đối với mọi chiều dài`j ≥ 1`, nối ký tự hiện tại vào tất cả các chuỗi có độ dài riêng biệt`j-1`từ tiền tố trước đó. Điều này góp phần`dp[i-1][j-1]`. 
5. Nếu ký tự hiện tại xuất hiện trước đó thì vị trí trước đó của nó là`p`. Nối ký tự hiện tại vào các chuỗi con đã tồn tại trước vị trí`p`tạo ra các bản sao. Trừ`dp[p-1][j-1]`từ sự đóng góp. 

Phép trừ sẽ loại bỏ chính xác các chuỗi con đã được tạo khi ký tự này xuất hiện trước đó. 

1. Lưu vị trí hiện tại là lần xuất hiện mới nhất của ký tự này. 
2. Sau khi điền vào bảng,`dp[n][j]`đưa ra số dãy con riêng biệt có độ dài`j`. 
3. Bắt đầu từ chiều dài`n`và di chuyển xuống dưới, lấy càng nhiều dãy con càng tốt trong khi`k > 0`. 
4. Nếu chúng ta lấy`x`dãy số có độ dài`j`, đóng góp của họ cho câu trả lời là```
x × (n - j)
```1. Nếu sau khi xét độ dài`0`chúng tôi vẫn có`k > 0`, ít hơn`k`tồn tại các dãy con riêng biệt, vì vậy hãy in`-1`. 

### Tại sao nó hoạt động 

Đối với mọi tiền tố và mọi độ dài,`dp[i][j]`đếm các chuỗi con riêng biệt chính xác một lần. Bất cứ khi nào một ký tự xuất hiện trở lại, các chuỗi con trùng lặp duy nhất được tạo ra là những chuỗi xuất hiện cuối cùng của ký tự đó đến từ bản sao trước đó. Việc trừ phần đóng góp trước lần xuất hiện trước đó sẽ loại bỏ chính xác những phần trùng lặp đó và không có gì khác. 

Vì chi phí giảm khi độ dài dãy con tăng nên mọi giải pháp tối ưu đều phải sử dụng càng nhiều dãy con dài càng tốt trước khi xem xét các dãy con ngắn hơn. Do đó, giai đoạn thu thập tham lam tạo ra tổng chi phí tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
s = input().strip()

dp = [[0] * (n + 1) for _ in range(n + 1)]
dp[0][0] = 1

last = [-1] * 26

for i in range(1, n + 1):
    c = ord(s[i - 1]) - ord('a')

    for j in range(n + 1):
        dp[i][j] = dp[i - 1][j]

    prev = last[c]

    for j in range(1, i + 1):
        add = dp[i - 1][j - 1]
        if prev != -1:
            add -= dp[prev][j - 1]
        dp[i][j] += add

    last[c] = i - 1

ans = 0

for length in range(n, -1, -1):
    take = min(k, dp[n][length])
    ans += take * (n - length)
    k -= take

if k > 0:
    print(-1)
else:
    print(ans)
```Bàn có kích thước`(n+1) × (n+1)`. Hàng ngang`i`đại diện cho lần đầu tiên`i`nhân vật. Mỗi hàng bắt đầu bằng cách sao chép hàng trước đó vì mọi dãy con tồn tại trước đó vẫn tồn tại sau khi thêm ký tự mới. 

Phần tinh tế là phép trừ. Giả sử ký tự hiện tại trước đó đã xuất hiện tại chỉ mục`prev`. Bất kỳ dãy con nào được hình thành bằng cách mở rộng thứ gì đó đã tồn tại trước vị trí`prev`đã được tạo khi lần xuất hiện trước đó được sử dụng. Trừ`dp[prev][j-1]`loại bỏ chính xác những sự lặp lại đó. 

Giai đoạn tham lam bắt đầu từ chiều dài`n`, bởi vì những chuỗi con này có chi phí thấp nhất. Di chuyển xuống đảm bảo tổng chi phí tối thiểu. 

Không có vấn đề tràn số nguyên nào tồn tại trong Python. Trong phiên bản dễ, tổng số dãy con riêng biệt không bao giờ vượt quá`2^100`, mà Python xử lý một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào```
4 5
asdf
```Tất cả các nhân vật đều khác nhau. 

| Chiều dài | Đếm các dãy con riêng biệt | Chi phí | Đã chụp | 
| --- | --- | --- | --- | 
| 4 | 1 | 0 | 1 | 
| 3 | 4 | 1 | 4 | 
| 2 | 6 | 2 | 0 | 
| 1 | 4 | 3 | 0 | 
| 0 | 1 | 4 | 0 | 

Tổng chi phí:```
1×0 + 4×1 = 4
```Ví dụ này cho thấy tại sao việc lấy các chuỗi con dài hơn trước là tối ưu. 

### Ví dụ 2 

đầu vào```
3 8
aaa
```| Chiều dài | Đếm các dãy con riêng biệt | Chi phí | Đã chụp | 
| --- | --- | --- | --- | 
| 3 | 1 | 0 | 1 | 
| 2 | 1 | 1 | 1 | 
| 1 | 1 | 2 | 1 | 
| 0 | 1 | 3 | 1 | 

Chỉ có bốn dãy con riêng biệt tồn tại. 

Sau khi lấy hết bốn cái, chúng ta vẫn cần thêm bốn cái nữa, vậy đáp án là`-1`. 

Ví dụ này chứng minh rằng các ký tự lặp lại làm giảm đáng kể số lượng các chuỗi con riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | điền một`(n+1) × (n+1)`bảng DP | 
| Không gian | O(n²) | Lưu trữ tất cả các trạng thái DP | 

Với`n ≤ 100`, nhiều nhất cần khoảng mười nghìn trạng thái. Thuật toán chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    s = input().strip()

    dp = [[0] * (n + 1) for _ in range(n + 1)]
    dp[0][0] = 1
    last = [-1] * 26

    for i in range(1, n + 1):
        c = ord(s[i - 1]) - ord('a')

        for j in range(n + 1):
            dp[i][j] = dp[i - 1][j]

        prev = last[c]

        for j in range(1, i + 1):
            add = dp[i - 1][j - 1]
            if prev != -1:
                add -= dp[prev][j - 1]
            dp[i][j] += add

        last[c] = i - 1

    ans = 0
    k = int(k)

    for length in range(n, -1, -1):
        take = min(k, dp[n][length])
        ans += take * (n - length)
        k -= take

    if k > 0:
        print(-1)
    else:
        print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("4 5\nasdf\n") == "4"

# custom cases
assert run("1 1\na\n") == "0", "minimum size"
assert run("1 2\na\n") == "1", "empty subsequence needed"
assert run("3 8\naaa\n") == "-1", "not enough distinct subsequences"
assert run("2 4\nab\n") == "4", "all subsequences are used"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / a`| 0 | Kích thước tối thiểu | 
|`1 2 / a`| 1 | Xử lý chuỗi tiếp theo trống | 
|`3 8 / aaa`| -1 | Các chuỗi con trùng lặp gây ra bởi các chữ cái lặp đi lặp lại | 
|`2 4 / ab`| 4 | Sử dụng mọi dãy con có thể | 

## Vỏ cạnh 

Hãy xem xét```
1 2
a
```DP đếm một chuỗi có độ dài`1`và một chiều dài`0`. Giai đoạn tham lam đòi hỏi cả hai. Chi phí của họ là`0`Và`1`, đưa ra câu trả lời`1`. Dãy con trống được xử lý một cách tự nhiên bởi`dp[0][0]=1`. 

Bây giờ hãy xem xét```
3 8
aaa
```Bảng kết thúc với```
length 3 : 1
length 2 : 1
length 1 : 1
length 0 : 1
```Mặc dù có tám mặt nạ xóa nhưng nhiều mặt nạ tạo ra các chuỗi giống nhau. Sau khi sử dụng tất cả bốn dãy con phân biệt,`k`vẫn dương nên thuật toán in đúng`-1`. 

Cuối cùng, hãy xem xét```
4 5
abcd
```Tất cả các ký tự đều khác nhau nên không xảy ra phép trừ. Số đếm trở thành```
1, 4, 6, 4, 1
```cho độ dài`0`bởi vì`4`. Pha tham lam chọn một dãy con có độ dài`4`và bốn chiều dài`3`, tạo ra tổng chi phí`4`, đó là mức tối thiểu có thể.

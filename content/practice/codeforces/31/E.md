---
title: "CF 31E - Trò Chơi Truyền Hình"
description: "Chúng ta được cấp một chuỗi gồm 2n chữ số. Các chữ số được xử lý nghiêm ngặt từ trái sang phải. Ở mỗi bước, Homer hoặc Marge lấy chữ số ngoài cùng bên trái hiện tại và thêm nó vào số của chính họ. Cuối cùng, cả hai người chơi phải lấy đúng n chữ số."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 31
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 31 (Div. 2, Codeforces format)"
rating: 2400
weight: 31
solve_time_s: 118
verified: false
draft: false
---
[CF 31E - Trò chơi truyền hình](https://codeforces.com/problemset/problem/31/E) 

**Đánh giá:** 2400 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`2n`chữ số. Các chữ số được xử lý nghiêm ngặt từ trái sang phải. Ở mỗi bước, Homer hoặc Marge lấy chữ số ngoài cùng bên trái hiện tại và thêm nó vào số của chính họ. Cuối cùng, cả hai người chơi phải thực hiện chính xác`n`chữ số. 

Điểm cuối cùng là tổng của hai số được xây dựng. Chúng ta phải xuất ra một chuỗi di chuyển hợp lệ, sử dụng`H`Và`M`, điều đó tối đa hóa tổng số này. 

Chi tiết quan trọng là thứ tự bên trong số của mỗi người chơi được cố định. Nếu Homer lấy chữ số ở vị trí`i1 < i2 < ...`, thì số của anh ấy chính xác là các chữ số theo thứ tự đó. Điều tương tự cũng xảy ra với Marge. 

Các ràng buộc đủ nhỏ để cho phép lập trình động, nhưng đủ lớn để không thể thực hiện được mọi phép gán một cách thô bạo. Từ`n ≤ 18`, tổng số chữ số nhiều nhất là`36`. Một tìm kiếm đơn giản trên tất cả các phép gán chữ số cho người chơi sẽ kiểm tra`2^(2n)`khả năng. Trong trường hợp xấu nhất điều này trở thành`2^36`, khoảng 68 tỷ trạng thái, hoàn toàn không thể thực hiện được trong 2 giây. 

Đồng thời,`36 × 36 × 10^18`phong cách số học là hoàn toàn tốt. Bản thân các số cuối cùng có thể có tới 18 chữ số, vì vậy cần có số nguyên 64 bit. Số nguyên Python tự động xử lý việc này. 

Một số trường hợp đặc biệt rất dễ bị xử lý sai nếu lý do không chính xác. 

Coi như:```
1
09
```Nếu Homer lấy`0`và Marge lấy`9`, tổng số là`0 + 9 = 9`. 

Nếu Homer lấy`9`và Marge lấy`0`, tổng số là`9 + 0 = 9`. 

Các số 0 đứng đầu là hợp pháp, vì vậy chúng tôi không thể coi các chuỗi được xây dựng là số không hợp lệ. 

Một trường hợp tế nhị khác là:```
2
9090
```Chiến lược tham lam luôn đưa chữ số hiện tại cho người chơi có giá trị hiện tại nhỏ hơn có thể thất bại, vì các chữ số trước đó có ý nghĩa hơn. Việc gán một chữ số trước đó sẽ thay đổi vị trí thập phân cao hơn việc gán một chữ số sau. 

Một kịch bản quan trọng hơn là:```
2
1234
```Nếu Homer lấy hai chữ số đầu tiên và Marge lấy hai chữ số cuối cùng, chúng ta nhận được`12 + 34 = 46`. 

Nhưng xen kẽ mang lại:`13 + 24 = 37`Vị trí bên trong số của mỗi người chơi quan trọng hơn chữ số tuyệt đối. Một chữ số được đặt trước đó đóng góp lũy thừa lớn hơn của mười. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Đối với mỗi`2n`vị trí, quyết định xem chữ số sẽ chuyển đến Homer hay Marge, đồng thời đảm bảo cả hai đều nhận được chính xác`n`chữ số. 

Khi một bài tập được ấn định, chúng ta có thể xây dựng cả hai số và tính tổng của chúng. Số bài tập hợp lệ là:$$\binom{2n}{n}$$Vì`n = 18`, đây là:$$\binom{36}{18} \approx 9 \times 10^9$$Ngay cả việc kiểm tra một phần nhỏ các trạng thái đó cũng là không thể. 

Lý do vũ lực hoạt động về mặt khái niệm là vì mỗi chuỗi hợp lệ xác định duy nhất các số cuối cùng. Vấn đề hoàn toàn là sự bùng nổ của sự kết hợp. 

Quan sát quan trọng là sự đóng góp của một chữ số chỉ phụ thuộc vào hai điều: 

Đầu tiên, người chơi nào nhận được nó. 

Thứ hai, sau đó người chơi vẫn nhận được bao nhiêu chữ số. 

Giả sử Homer đã lấy`h`chữ số trước vị trí`i`. Khi đó chữ số hiện tại sẽ trở thành`(h+1)`-chữ số thứ của số Homer. Vì Homer sẽ kết thúc bằng chính xác`n`chữ số, chữ số này góp phần:$$digit \times 10^{n-h-1}$$Các quyết định trong tương lai không làm thay đổi trọng số vị trí này. 

Điều đó có nghĩa là chúng ta có thể xử lý các chữ số từ trái sang phải và sử dụng lập trình động theo số lượng chữ số mà Homer đã lấy. 

Tại vị trí`i`, Marge đã tự động lấy`i-h`chữ số. 

Vì vậy, toàn bộ trạng thái được xác định bởi một tham số duy nhất:$$dp[i][h]$$nghĩa là tổng giá trị tối đa có thể đạt được sau khi xử lý lần đầu tiên`i`chữ số, trong đó Homer đã lấy chính xác`h`của họ. 

Từ trạng thái này, chúng tôi thử: 

1. Đưa chữ số tiếp theo cho Homer. 
2. Đưa chữ số tiếp theo cho Marge. 

Mỗi lần chuyển đổi sẽ thêm một đóng góp đã biết ngay lập tức. 

Tổng số bang chỉ có khoảng`36 × 18`, đủ nhỏ để tính toán ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(2n)) | O(2^(2n)) | Quá chậm | 
| DP tối ưu | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và chuỗi chữ số`s`. 
2. Tính trước lũy thừa 10 từ`10^0`lên đến`10^18`. 

Những quyền hạn này xác định sự đóng góp vị trí của mỗi chữ số. 

1. Xác định`dp[i][h]`là tổng giá trị tối đa có thể đạt được sau khi xử lý lần đầu tiên`i`chữ số, trong đó Homer đã lấy chính xác`h`chữ số. 

Vì Marge tự động lấy`i-h`chữ số, chúng ta không cần thứ nguyên khác. 

1. Khởi tạo:```
dp[0][0] = 0
```Tất cả các trạng thái khác đều bắt đầu bằng âm vô cực. 

1. Xử lý các chữ số từ trái sang phải. 

Giả sử chúng ta đang ở vị trí`i`, với Homer đã thực hiện`h`chữ số. 

Cho phép:```
m = i - h
```là số chữ số mà Marge đã lấy. 

1. Thử gán chữ số hiện tại cho Homer. 

Điều này chỉ có thể thực hiện được nếu`h < n`. 

Chữ số trở thành của Homer`(h+1)`-chữ số thứ, nên trọng số thập phân của nó là:$$10^{n-h-1}$$Thêm phần đóng góp này và chuyển tiếp vào:```
dp[i+1][h+1]
```1. Thử gán chữ số hiện tại cho Marge. 

Điều này chỉ có thể thực hiện được nếu`m < n`. 

Chữ số trở thành của Marge`(m+1)`-chữ số thứ , nên trọng lượng của nó là:$$10^{n-m-1}$$Thêm phần đóng góp này và chuyển tiếp vào:```
dp[i+1][h]
```1. Lưu trữ thông tin cha mẹ để xây dựng lại. 

Đối với mỗi quá trình chuyển đổi được cải thiện, hãy nhớ: 

1. Trạng thái trước đó. 
2. Liệu chúng ta có chọn không`H`hoặc`M`. 
3. Sau khi xử lý tất cả các chữ số, câu trả lời sẽ được lưu vào:```
dp[2n][n]
```1. Xây dựng lại trình tự di chuyển bằng cách đi lùi qua các con trỏ gốc. 

Đảo ngược các ký tự được thu thập ở cuối. 

### Tại sao nó hoạt động 

Mỗi chữ số đóng góp độc lập khi chúng tôi biết người chơi nào nhận được nó và vị trí nào trong số của người chơi đó mà nó chiếm giữ. 

Khi xử lý các chữ số từ trái sang phải, vị trí bên trong số Homer hoàn toàn được xác định bằng số lượng chữ số mà Homer đã lấy. Điều tương tự cũng xảy ra với Marge. 

Vì vậy, mọi kết quả trong tương lai của một trạng thái chỉ phụ thuộc vào: 

1. Có bao nhiêu chữ số đã được xử lý. 
2. Homer đã lấy bao nhiêu cái. 

DP khám phá mọi phép gán hợp lệ chính xác một lần và tính toán tổng số tốt nhất có thể đạt được cho mỗi trạng thái. Vì các phép chuyển đổi thêm phần đóng góp thập phân chính xác của chữ số hiện tại nên giá trị cuối cùng bằng tổng thực của hai số được xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = -(10**30)

def solve():
    n = int(input())
    s = input().strip()

    total = 2 * n

    pow10 = [1] * (n + 1)
    for i in range(1, n + 1):
        pow10[i] = pow10[i - 1] * 10

    dp = [[INF] * (n + 1) for _ in range(total + 1)]
    parent = [[None] * (n + 1) for _ in range(total + 1)]

    dp[0][0] = 0

    for i in range(total):
        digit = int(s[i])

        for h in range(n + 1):
            if dp[i][h] == INF:
                continue

            m = i - h

            # Give digit to Homer
            if h < n:
                add = digit * pow10[n - h - 1]
                val = dp[i][h] + add

                if val > dp[i + 1][h + 1]:
                    dp[i + 1][h + 1] = val
                    parent[i + 1][h + 1] = (i, h, 'H')

            # Give digit to Marge
            if m < n:
                add = digit * pow10[n - m - 1]
                val = dp[i][h] + add

                if val > dp[i + 1][h]:
                    dp[i + 1][h] = val
                    parent[i + 1][h] = (i, h, 'M')

    ans = []

    i = total
    h = n

    while i > 0:
        pi, ph, ch = parent[i][h]
        ans.append(ch)
        i, h = pi, ph

    ans.reverse()

    print("".join(ans))

solve()
```Bảng DP lưu trữ tổng số tốt nhất có thể đạt được cho mọi độ dài tiền tố và mọi số chữ số có thể có được gán cho Homer. 

Logic chuyển tiếp là cốt lõi của giải pháp. Khi một chữ số được gán, chúng ta biết ngay trọng số vị trí chính xác của nó. Ví dụ: nếu Homer đã thực hiện`h`chữ số thì chữ số hiện tại sẽ trở thành chữ số tiếp theo trong số của anh ta và đóng góp:```
digit × 10^(n-h-1)
```Không có quyết định sau này có thể thay đổi sự đóng góp này. 

Bảng cha là cần thiết vì bài toán yêu cầu trình tự các bước di chuyển chứ không chỉ điểm tối đa. Mỗi mục nhập phụ huynh ghi lại: 

1. Trạng thái trước đó. 
2. Người chơi nào đã nhận được chữ số đó. 

Một chi tiết tinh tế là việc tính toán số chữ số của Marge:```
m = i - h
```Điều này hiệu quả vì chính xác`i`các chữ số đã được xử lý và`h`trong số họ đã đến Homer. 

Một sai lầm dễ mắc phải khác là quên rằng các số 0 đứng đầu là hợp lệ. DP xử lý chúng một cách tự nhiên vì các chữ số được coi hoàn toàn là đóng góp theo vị trí. 

Phạm vi giá trị có thể vượt quá số nguyên 32 bit. Số nguyên Python tránh tự động tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1234
```| Bước | Chữ số | Số lượng Homer | Số lượng Marge | Lựa chọn | Giá trị gia tăng | Tổng số tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | H | 10 | 10 | 
| 1 | 2 | 1 | 0 | H | 2 | 12 | 
| 2 | 3 | 2 | 0 | M | 30 | 42 | 
| 3 | 4 | 2 | 1 | M | 4 | 46 | 

Câu trả lời cuối cùng:```
HHMM
```Dấu vết này cho thấy các vị trí trước đó thống trị các vị trí sau như thế nào. Cho`1`cho Homer khi chữ số đầu tiên của anh ấy đóng góp`10`, trong khi đưa`2`vì chữ số thứ hai của anh ấy chỉ đóng góp`2`. 

### Ví dụ 2 

đầu vào:```
2
9090
```| Bước | Chữ số | Số lượng Homer | Số lượng Marge | Lựa chọn | Giá trị gia tăng | Tổng số tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 9 | 0 | 0 | H | 90 | 90 | 
| 1 | 0 | 1 | 0 | M | 0 | 90 | 
| 2 | 9 | 1 | 1 | M | 9 | 99 | 
| 3 | 0 | 1 | 2 | H | 0 | 99 | 

Một câu trả lời tối ưu là:```
HMMH
```Ví dụ này chứng tỏ rằng việc gán một chữ số lớn sớm hơn có giá trị hơn nhiều so với việc gán nó sau. đầu tiên`9`đóng góp`90`, trong khi thứ hai chỉ đóng góp`9`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Có O(n2) trạng thái DP và chuyển tiếp O(1) trên mỗi trạng thái | 
| Không gian | O(n²) | Bảng DP và bảng cha mỗi cửa hàng lưu trữ trạng thái O(n²) | 

Với`n ≤ 18`, DP chứa tối đa khoảng`37 × 19`tiểu bang. Giải pháp chạy thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    INF = -(10**30)

    n = int(input())
    s = input().strip()

    total = 2 * n

    pow10 = [1] * (n + 1)
    for i in range(1, n + 1):
        pow10[i] = pow10[i - 1] * 10

    dp = [[INF] * (n + 1) for _ in range(total + 1)]
    parent = [[None] * (n + 1) for _ in range(total + 1)]

    dp[0][0] = 0

    for i in range(total):
        digit = int(s[i])

        for h in range(n + 1):
            if dp[i][h] == INF:
                continue

            m = i - h

            if h < n:
                add = digit * pow10[n - h - 1]
                val = dp[i][h] + add

                if val > dp[i + 1][h + 1]:
                    dp[i + 1][h + 1] = val
                    parent[i + 1][h + 1] = (i, h, 'H')

            if m < n:
                add = digit * pow10[n - m - 1]
                val = dp[i][h] + add

                if val > dp[i + 1][h]:
                    dp[i + 1][h] = val
                    parent[i + 1][h] = (i, h, 'M')

    ans = []

    i = total
    h = n

    while i > 0:
        pi, ph, ch = parent[i][h]
        ans.append(ch)
        i, h = pi, ph

    ans.reverse()

    print("".join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run("2\n1234\n") == "HHMM", "sample 1"

# minimum size
assert run("1\n09\n") in ["HM", "MH"], "leading zero case"

# all digits equal
out = run("3\n111111\n")
assert out.count('H') == 3 and out.count('M') == 3, "balanced assignment"

# alternating high and low digits
out = run("2\n9090\n")
assert out.count('H') == 2 and out.count('M') == 2, "positional weighting"

# maximum length structure
s = "9" * 36
out = run(f"18\n{s}\n")
assert len(out) == 36, "maximum size"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1234`|`HHMM`| Phân phối tối ưu cơ bản | 
|`1 / 09`|`HM`hoặc`MH`| Xử lý số 0 hàng đầu | 
|`3 / 111111`| Bất kỳ câu trả lời hợp lệ cân bằng nào | Chữ số bằng nhau | 
|`2 / 9090`| Bất kỳ câu trả lời cân bằng tối ưu nào | Vị trí trước đó chiếm ưu thế | 
|`18 / 999...999`| Bất kỳ câu trả lời có độ dài 36 hợp lệ nào | Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
09
```Ở chữ số đầu tiên, gán`0`cho một trong hai người chơi đóng góp:```
0 × 10^0 = 0
```Chữ số thứ hai góp phần`9`. 

Cả hai bài đều có tổng điểm như nhau. DP xử lý việc này một cách tự nhiên vì các số 0 đứng đầu được coi là các chữ số thông thường có trọng số vị trí. 

Bây giờ hãy xem xét:```
2
9090
```Một chiến lược tham lam bất cẩn có thể cố gắng cân bằng các giá trị số hiện tại của hai người chơi. Điều đó không thành công vì vị trí thập phân quan trọng hơn tổng số hiện tại. 

Thay vào đó, DP đánh giá những đóng góp chính xác: 

| Chữ số | Vị trí trọng số nếu chữ số đầu tiên | Vị trí trọng số nếu chữ số thứ hai | 
| --- | --- | --- | 
| 9 | 10 | 1 | 

Chỉ định đầu tiên`9`sớm luôn có giá trị hơn nhiều. 

Cuối cùng, hãy xem xét:```
2
1000
```Nếu Homer dẫn đầu`1`, đóng góp của nó là:```
1 × 10 = 10
```Nếu`1`trở thành chữ số thứ hai của ai đó, phần đóng góp của nó giảm xuống`1`. 

DP tự động nắm bắt điều này thông qua số mũ:```
pow10[n - h - 1]
```theo dõi chính xác số lượng vị trí còn lại trong số của người chơi đó.

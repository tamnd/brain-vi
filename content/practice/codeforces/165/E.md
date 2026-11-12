---
title: "CF 165E - Số tương thích"
description: "Chúng ta được cung cấp một mảng các số nguyên và với mỗi phần tử, chúng ta phải tìm một phần tử mảng khác có AND theo từng bit bằng 0. Hai số tương thích chính xác khi chúng không chia sẻ bất kỳ bit nào được đặt thành 1."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dfs-and-similar", "dp"]
categories: ["algorithms"]
codeforces_contest: 165
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 112 (Div. 2)"
rating: 2200
weight: 165
solve_time_s: 107
verified: true
draft: false
---

[CF 165E - Số tương thích](https://codeforces.com/problemset/problem/165/E) 

**Xếp hạng:** 2200 
**Tags:** bitmasks, brute Force, dfs và tương tự, dp 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và với mỗi phần tử, chúng ta phải tìm một phần tử mảng khác có AND theo từng bit bằng 0. Hai số tương thích chính xác khi chúng không chia sẻ bất kỳ bit nào được đặt thành`1`. 

Ví dụ, nếu chúng ta nhìn vào`90`Và`36`:```
90  = 1011010
36  = 0100100
AND = 0000000
```Không có vị trí bit nào chứa`1`ở cả hai số, vì vậy chúng tương thích. 

Nhiệm vụ không phải là đếm các cặp tương thích. Với mỗi giá trị mảng`a[i]`, chúng ta chỉ cần xuất ra một phần tử mảng`x`như vậy:```
a[i] & x = 0
```Nếu không có phần tử như vậy tồn tại, chúng tôi in`-1`. 

Các ràng buộc hoàn toàn xác định hướng đi của giải pháp. Mảng có thể chứa tới`10^6`số và mỗi giá trị nhiều nhất là`4 * 10^6`. Từ`4 * 10^6 < 2^22`, mọi số đều nằm trong 22 bit. 

Một giải pháp bậc hai sẽ yêu cầu kiểm tra tất cả các cặp:```
10^6 * 10^6 = 10^12
```hoạt động, điều đó là không thể. 

Giới hạn 22 bit là đầu mối thực sự. Bất cứ khi nào số lượng bit nhỏ, có thể lập trình động bitmask. Không gian trạng thái có kích thước`2^22`là khoảng 4,2 triệu, một con số lớn nhưng vẫn có thể quản lý được cả về thời gian và bộ nhớ khi triển khai hệ số không đổi thấp. 

Có một số trường hợp nghiêm trọng phá vỡ các giải pháp bất cẩn. 

Giả sử đầu vào là:```
1
7
```Số duy nhất là`7`. Từ`7 & 7 != 0`, câu trả lời là`-1`. Việc triển khai có lỗi có thể vô tình cho phép một số tự khớp mà không kiểm tra tính tương thích. 

Một trường hợp phức tạp khác là các giá trị trùng lặp:```
3
0 5 5
```số`0`tương thích với mọi thứ vì:```
0 & x = 0
```cho tất cả`x`. Đầu ra đúng có thể là:```
5 0 0
```Một giải pháp chỉ tìm kiếm các mặt nạ con nghiêm ngặt có thể bỏ sót`0`. 

Một vấn đề tinh vi hơn xuất hiện khi nhiều bit được đặt:```
2
4194303 1
```

`4194303 = 2^22 - 1`có mọi bit được đặt, vì vậy nó không thể tương thích với bất kỳ số dương nào. Các câu trả lời đúng là:```
-1 -1
```Tính toán mặt nạ bổ sung sai có thể dễ dàng vượt quá phạm vi 22 bit hợp lệ và tạo ra trạng thái không hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất sẽ kiểm tra từng cặp số. 

Đối với mỗi`a[i]`, chúng tôi lặp qua toàn bộ mảng và kiểm tra:```
a[i] & a[j] == 0
```Bản thân hoạt động bitwise AND là thời gian không đổi, do đó tổng độ phức tạp trở thành`O(n^2)`. 

Phương pháp bạo lực này là chính xác vì khả năng tương thích được xác định hoàn toàn bằng cách so sánh từng cặp. Nếu có bất kỳ số tương thích nào tồn tại, việc tìm kiếm toàn diện cuối cùng sẽ tìm thấy nó. 

Vấn đề là quy mô. Với`n = 10^6`, phương pháp này yêu cầu khoảng`10^12`kiểm tra, vượt quá giới hạn hàng ngàn lần. 

Quan sát quan trọng là tính tương thích chỉ phụ thuộc vào mẫu bit chứ không phụ thuộc vào vị trí trong mảng. 

Đối với một số cố định`x`, chúng tôi muốn một số số`y`như vậy:```
x & y = 0
```Điều đó có nghĩa là mỗi bit được đặt trong`y`phải nằm bên trong vị trí bit 0 của`x`. 

Nếu chúng ta xác định:```
FULL = (1 << 22) - 1
```sau đó các bit được phép cho`y`chính xác là:```
FULL ^ x
```Vì vậy, thay vì tìm kiếm toàn bộ mảng, chúng ta có thể hỏi: 

"Mảng có chứa bất kỳ mặt nạ con nào của`FULL ^ x`?" 

Điều này biến vấn đề thành một SOS DP cổ điển, còn được gọi là Sum Over Subsets DP. 

Chúng tôi tạo ra một mảng`dp[mask]`Ở đâu:```
dp[mask] = some array value that is a submask of mask
```Ban đầu, nếu một số`v`xuất hiện trong mảng, chúng tôi đặt:```
dp[v] = v
```Sau đó, chúng tôi truyền bá thông tin lên trên thông qua mặt nạ. Đối với mỗi mặt nạ và mỗi bit được đặt, chúng tôi kế thừa câu trả lời từ các mặt nạ con nhỏ hơn. 

Sau khi tiền xử lý, việc truy vấn trở nên dễ dàng:```
answer(x) = dp[FULL ^ x]
```bởi vì bất kỳ mặt nạ con nào của mặt nạ bổ sung không chia sẻ bit nào với`x`. 

Chi phí tiền xử lý khoảng:```
22 * 2^22
```hoạt động, khoảng 92 triệu lần chuyển đổi. Trong Python được tối ưu hóa, điều này có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| SOS DP tối ưu | O(22 · 22² + n) | O(2²²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định độ rộng bit tối đa là 22 vì mọi số đầu vào đều nhỏ hơn`2^22`. 
2. Tạo một mảng`dp`kích thước`2^22`và khởi tạo mọi mục nhập vào`-1`. 
3. Với mọi giá trị`x`trong mảng đầu vào, đặt:```
dp[x] = x
```Điều này có nghĩa là mặt nạ chính xác`x`tồn tại trong mảng. 
4. Chạy SOS DP trên tất cả các mặt nạ. 

Đối với mọi mặt nạ và mọi vị trí bit: 

Nếu bit được đặt trong mặt nạ hiện tại, hãy thử xóa nó:```
smaller = mask ^ (1 << bit)
```Nếu như`dp[mask]`vẫn còn`-1`, kế thừa:```
dp[mask] = dp[smaller]
```Lý do điều này hoạt động là vì mọi mặt nạ con đều có thể đạt được bằng cách loại bỏ liên tục các bit đã đặt. 
5. Với mỗi giá trị mảng`x`, tính:```
complement = FULL ^ x
```Mỗi số tương thích phải là một mặt nạ con của mặt nạ bổ sung này. 
6. Đầu ra:```
dp[complement]
```Nếu không có mặt nạ con hợp lệ tồn tại trong mảng thì giá trị vẫn giữ nguyên`-1`. 

### Tại sao nó hoạt động 

Bất biến DP là:```
dp[mask] stores some array value that is a submask of mask,
or -1 if none exists.
```Ban đầu điều này đúng với mặt nạ chính xác vì chúng tôi lưu trữ rõ ràng các số hiện có. 

Trong quá trình chuyển đổi, chúng tôi chỉ sao chép các giá trị từ các mặt nạ nhỏ hơn thu được bằng cách loại bỏ các bit. Bất kỳ mặt nạ con nào của`smaller`cũng là một mặt nạ con của`mask`, do đó bất biến vẫn hợp lệ. 

Đối với một giá trị truy vấn`x`, mặt nạ`FULL ^ x`chứa chính xác các vị trí bit trong đó`x`có số không. Bất kỳ mặt nạ con nào`y`của phần bù này thỏa mãn:```
x & y = 0
```bởi vì`y`không bao giờ sử dụng một chút đã được thiết lập sẵn`x`. 

Vậy nếu`dp[FULL ^ x] = y`, sau đó`y`được đảm bảo tương thích với`x`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

BITS = 22
SIZE = 1 << BITS
FULL = SIZE - 1

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    dp = [-1] * SIZE

    for x in a:
        dp[x] = x

    for bit in range(BITS):
        for mask in range(SIZE):
            if mask & (1 << bit):
                if dp[mask] == -1:
                    dp[mask] = dp[mask ^ (1 << bit)]

    ans = []

    for x in a:
        ans.append(str(dp[FULL ^ x]))

    print(" ".join(ans))

solve()
```Giải pháp bắt đầu bằng cách phân bổ mảng DP trên tất cả các mặt nạ 22 bit. Con số này lớn, khoảng 4,2 triệu mục nhập, nhưng vẫn vừa vặn thoải mái trong giới hạn bộ nhớ trong Python khi lưu trữ số nguyên. 

Bước khởi tạo lưu trữ mọi giá trị mảng trực tiếp vào vị trí mặt nạ của chính nó. Tại thời điểm này, chỉ có mặt nạ chính xác được biết đến. 

Pha SOS DP truyền thông tin từ các mặt nạ con nhỏ hơn tới các mặt nạ lớn hơn. Thứ tự quan trọng. Đối với mỗi bit, chúng tôi xử lý tất cả các mặt nạ chứa bit đó và thử kế thừa từ phiên bản đã loại bỏ bit đó. 

Một lỗi phổ biến là đảo ngược hướng chuyển tiếp. Chúng tôi muốn:```
mask -> mask without one bit
```bởi vì chúng tôi đang thu thập thông tin về mặt nạ phụ. 

Việc tính toán phần bù là một nơi khác xuất hiện lỗi. Chúng ta phải hạn chế phần bù ở 22 bit:```
FULL ^ x
```sử dụng`~x`sẽ tạo số âm trong Python vì số nguyên không bị chặn. 

Việc tra cứu cuối cùng là thời gian không đổi. Sau khi tiền xử lý, mọi truy vấn sẽ giảm xuống còn một truy cập mảng duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
90 36
```Các dạng nhị phân:```
90 = 1011010
36 = 0100100
```AND của họ bằng 0. 

| Bước | Mặt nạ/Giá trị | Kết quả | 
| --- | --- | --- | 
| Chèn giá trị | dp[90] = 90 | được lưu trữ | 
| Chèn giá trị | dp[36] = 36 | được lưu trữ | 
| Truy vấn 90 | ĐẦY ĐỦ ^ 90 chứa 36 dưới dạng mặt nạ con | đáp án = 36 | 
| Truy vấn 36 | ĐẦY ĐỦ ^ 36 chứa 90 dưới dạng mặt nạ con | đáp án = 90 | 

Đầu ra:```
36 90
```Dấu vết này cho thấy ý tưởng cốt lõi của thuật toán. Chúng tôi không tìm kiếm trực tiếp các số tương thích. Chúng tôi tìm kiếm các mặt nạ con của phần bổ sung. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 7
```Các dạng nhị phân:```
1 = 001
2 = 010
3 = 011
7 = 111
```| Số | Bit bổ sung | Mặt nạ con tương thích hiện có | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 110 | 2 | 2 | 
| 2 | 101 | 1 | 1 | 
| 3 | 100 | không | -1 | 
| 7 | 000 | không | -1 | 

Đầu ra có thể:```
2 1 -1 -1
```Ví dụ này chứng minh rằng thuật toán phân biệt chính xác giữa "một số bit trùng lặp" và "không có bit nào trùng lặp". số`3`không thể ghép nối với một trong hai`1`hoặc`2`bởi vì cả hai đều chia sẻ một phần nhất định với nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(22 · 22² + n) | SOS DP trên tất cả các mặt nạ và bit, cộng với đầu vào/đầu ra tuyến tính | 
| Không gian | O(2²²) | Bảng DP cho mỗi mặt nạ 22 bit |`2^22`là khoảng 4,2 triệu. Nhân với 22 sẽ có khoảng 92 triệu lần chuyển đổi, con số này lớn nhưng khả thi trong Python được tối ưu hóa vì mỗi lần chuyển đổi cực kỳ nhỏ và thân thiện với bộ đệm. Việc sử dụng bộ nhớ cũng phù hợp với 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

BITS = 22
SIZE = 1 << BITS
FULL = SIZE - 1

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    dp = [-1] * SIZE

    for x in a:
        dp[x] = x

    for bit in range(BITS):
        for mask in range(SIZE):
            if mask & (1 << bit):
                if dp[mask] == -1:
                    dp[mask] = dp[mask ^ (1 << bit)]

    ans = []

    for x in a:
        ans.append(str(dp[FULL ^ x]))

    print(" ".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run("2\n90 36\n") == "36 90", "sample 1"

# minimum size, no compatible value
assert run("1\n7\n") == "-1", "single element"

# zero-compatible structure
res = run("3\n1 2 4\n").split()
assert res[0] in {"2", "4"}
assert res[1] in {"1", "4"}
assert res[2] in {"1", "2"}

# all equal values
assert run("4\n5 5 5 5\n") == "-1 -1 -1 -1", "all equal"

# full-mask boundary
assert run("2\n4194303 1\n") == "-1 -1", "22-bit boundary"

# mixed compatibility
res = run("4\n1 2 3 7\n").split()
assert res[0] == "2"
assert res[1] == "1"
assert res[2] == "-1"
assert res[3] == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`-1`| Vỏ cạnh đơn | 
|`1 2 4`| Bất kỳ câu trả lời tương thích lẫn nhau nào | Nhiều đầu ra hợp lệ | 
|`5 5 5 5`| Tất cả`-1`| Xử lý trùng lặp | 
|`4194303 1`|`-1 -1`| Độ chính xác ranh giới 22-bit | 
|`1 2 3 7`|`2 1 -1 -1`| Các trường hợp tương thích và không tương thích hỗn hợp | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
7
```Số duy nhất có sẵn là`7`chính nó. Từ:```
7 & 7 = 7
```nó không tương thích với chính nó. 

Mặt nạ bổ sung chỉ chứa các bit 0 ở các vị trí mà`7`có những cái đó và không có mặt nạ con hiện có nào trong mảng thỏa mãn nó. Việc tra cứu DP trả về`-1`. 

Bây giờ hãy xem xét các bản sao:```
4
5 5 5 5
```Nhị phân:```
5 = 101
```Bất kỳ cặp nào`5`s vẫn chia sẻ bit:```
101 & 101 = 101
```Trong quá trình tiền xử lý, chỉ`dp[5]`trở nên đông dân cư. Không có mặt nạ bổ sung nào có thể tiếp cận nó thông qua chuyển đổi mặt nạ con, vì vậy mọi câu trả lời đều được giữ nguyên`-1`. 

Cuối cùng, kiểm tra trường hợp mặt nạ tối đa:```
2
4194303 1
```

`4194303`bằng:```
1111111111111111111111
```với tất cả 22 bit được đặt. 

Phần bù của nó bên trong 22 bit bằng 0:```
FULL ^ 4194303 = 0
```Giá trị DP ở mặt nạ`0`là`-1`, vì không tồn tại số 0 trong mảng. Thuật toán xuất ra chính xác`-1`. 

Đối với số`1`, phần bù của nó chứa tất cả các bit ngoại trừ bit thấp nhất, nhưng`4194303`không phải là mặt nạ con của phần bù đó vì nó vẫn chứa bit thấp nhất bị cấm. Vậy câu trả lời cho`1`cũng là`-1`.

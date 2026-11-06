---
title: "CF 1245C - Máy của Constanze"
description: "Chúng tôi nhận được một chuỗi được cho là do một chiếc máy viết bị hỏng nhẹ tạo ra. Máy thường in từng chữ cái được nói như chính nó, nhưng hai chữ cái đặc biệt hoạt động khác nhau: nếu người dùng nói w, máy sẽ viết uu và nếu người dùng nói m, nó sẽ viết nn."
date: "2026-06-15T21:34:46+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1245
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 597 (Div. 2)"
rating: 1400
weight: 1245
solve_time_s: 126
verified: true
draft: false
---

[CF 1245C - Máy của Constanze](https://codeforces.com/problemset/problem/1245/C) 

**Đánh giá:** 1400 
**Thẻ:** dp 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được một chuỗi được cho là do một chiếc máy viết bị hỏng nhẹ tạo ra. Máy thường in từng chữ cái được nói như chính nó, nhưng hai chữ cái đặc biệt hoạt động khác nhau: nếu người dùng nói`w`, máy ghi`uu`và nếu người dùng nói`m`, nó viết`nn`. Tất cả các chữ cái khác được in không thay đổi. 

Chúng tôi được cung cấp một chuỗi văn bản cuối cùng và được yêu cầu đếm xem có bao nhiêu chuỗi văn bản gốc có thể tạo ra nó theo các quy tắc này. 

Nói lại, chúng tôi đang cố gắng phân tách chuỗi đã cho thành các phân đoạn, trong đó mọi`u`có thể đến từ một thực tế`u`hoặc từ một ẩn`w`, và mọi`n`có thể đến từ một thực tế`n`hoặc từ một ẩn`m`. Nhiệm vụ là đếm tất cả các phân tách hợp lệ. 

Độ dài đầu vào có thể lên tới 100.000. Bất kỳ giải pháp nào cố gắng liệt kê tất cả các cách diễn giải hoặc thực hiện phân nhánh theo cấp số nhân sẽ ngay lập tức thất bại vì ngay cả một chuỗi nhỏ các ký tự lặp lại như`uuuuuu...`sẽ bùng nổ thành những tổ hợp hàm mũ. Điều này buộc phải áp dụng cách tiếp cận quy hoạch động tuyến tính hoặc gần tuyến tính. 

Một trường hợp khó nhận thấy là các ký tự không hợp lệ bên trong một lần chạy sẽ phá vỡ mọi khả năng. Ví dụ: nếu chuỗi chứa một ký tự khác ngoài`u`,`n`,`w`, hoặc`m`, vẫn ổn vì các chữ cái đó đã được cố định, nhưng nếu việc giải thích buộc phải phân tách không chính xác, một số phân đoạn có thể trở nên không thể thực hiện được. Cụ thể hơn, hãy xem xét`"amanda"`. lá thư`m`trong ngôn ngữ gốc luôn tạo ra`"nn"`, do đó, một thiết bị độc lập`m`ở đầu ra không bao giờ có thể tồn tại. Bất kỳ sự xuất hiện của`m`trong chuỗi đầu ra ngay lập tức loại bỏ tất cả các diễn giải hợp lệ, bởi vì không có chữ cái nguồn nào tạo ra một`m`. 

Một trường hợp quan trọng khác là chạy ngắn. Ví dụ`"u"`có chính xác một cách giải thích, nhưng`"uu"`có hai: hoặc là`u u`hoặc`w`. Vì`"uuu"`chúng tôi đã cần một sự lặp lại, bởi vì các vị trí chồng chéo của`w`tạo ra sự phụ thuộc. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực cố gắng coi mọi vị trí đều đến từ một chữ cái bình thường hoặc một phần của phép biến đổi được hợp nhất. Điều này nhanh chóng trở thành một vấn đề quay lui trong đó mỗi lần chạy các ký tự giống hệt nhau`u`hoặc`n`nhân đôi quyết định ở mỗi bước. Trong một khoảng thời gian dài`k`, số lượng phân đoạn tương ứng với mức tăng trưởng giống như Fibonacci, nhưng phép liệt kê ngây thơ vẫn sẽ phân nhánh rõ ràng, dẫn đến độ phức tạp theo cấp số nhân. 

Nhận xét quan trọng là chỉ có hai chữ cái thể hiện sự mơ hồ và cả hai sự mơ hồ đều mang tính cục bộ:`w → uu`Và`m → nn`. Điều này có nghĩa là sự mơ hồ chỉ xuất hiện trong các lần chạy liên tiếp của`u`hoặc`n`. Mọi thứ khác đều được cố định từng cái một. 

Vì vậy, chuỗi có thể được xử lý tuyến tính, nén nó thành các lần chạy liên tiếp tối đa. Đối với bất kỳ lần chạy nào`u`chiều dài`k`, chúng ta đếm có bao nhiêu cách để chia nó thành các phần có kích thước 1 hoặc 2, vì mỗi phần tương ứng với một chữ cái`u`hoặc một ẩn`w`. Điều tương tự cũng áp dụng cho việc chạy`n`. 

Đây chính xác là Fibonacci DP trên mỗi độ dài chạy, được nhân trên các phân đoạn độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| DP tối ưu (chạy + Fibonacci) | O(n) | O(n) hoặc O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi và nén nó thành các khối ký tự giống hệt nhau liên tiếp. 

1. Quét chuỗi từ trái sang phải và xác định số lần chạy tối đa của các ký tự giống hệt nhau. Mỗi lần chạy được xử lý độc lập vì không có phép biến đổi nào vượt qua ranh giới ký tự khác nhau. 
2. Nếu chúng ta gặp một nhân vật`m`hoặc`w`trong chuỗi cuối cùng, chúng ta ngay lập tức trả về 0. Điều này là do cả hai`m`cũng không`w`có thể xuất hiện dưới dạng một ký tự được in ở đầu ra; chúng luôn được mở rộng thành chuỗi hai ký tự. 
3. Đối với mỗi lần chạy`u`với chiều dài`k`, tính số cách phân chia nó thành các phân đoạn có kích thước 1 hoặc 2. Phân đoạn có kích thước 1 tương ứng với một chữ`u`và phân đoạn có kích thước-2 tương ứng với một phân đoạn ẩn`w`. 
4. Đối với mỗi lần chạy`n`với chiều dài`k`, tính toán cùng loại số lượng phân vùng, vì`n`có thể đến từ`n`hoặc từ`m`. 
5. Nhân phần đóng góp của mỗi lần chạy thành một câu trả lời tổng thể theo modulo$10^9+7$. 
6. Sử dụng DP kiểu Fibonacci trong đó`dp[i]`đại diện cho số cách để bao gồm một chiều dài`i`. Sự chuyển tiếp là`dp[i] = dp[i-1] + dp[i-2]`, vì phân đoạn cuối cùng có kích thước 1 hoặc kích thước 2. 

### Tại sao nó hoạt động 

Mỗi cách giải thích hợp lệ tương ứng duy nhất với một ô của mọi giá trị tối đa`u`hoặc`n`phân đoạn thành các ô có độ dài 1 hoặc 2. Các ô này độc lập giữa các phân đoạn vì các ký tự khác nhau không thể trùng lặp trong cách diễn giải. Sự lặp lại đếm chính xác tất cả các lựa chọn nhị phân của việc tách hoặc hợp nhất các ký tự liền kề và không có cấu hình nào được tính hai lần vì mỗi ô tương ứng với chính xác một phân tách thành`u`Và`w`(hoặc`n`Và`m`). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

s = input().strip()

# invalid characters that cannot appear in output
# since m and w are always expanded, they never appear alone
for ch in s:
    if ch == 'm' or ch == 'w':
        print(0)
        sys.exit(0)

n = len(s)

# precompute fibonacci up to n
dp = [0] * (n + 1)
dp[0] = 1
if n >= 1:
    dp[1] = 1

for i in range(2, n + 1):
    dp[i] = (dp[i - 1] + dp[i - 2]) % MOD

ans = 1

i = 0
while i < n:
    j = i
    while j < n and s[j] == s[i]:
        j += 1
    length = j - i

    if s[i] == 'u' or s[i] == 'n':
        ans = (ans * dp[length]) % MOD

    i = j

print(ans)
```Mã đầu tiên từ chối các trường hợp không thể liên quan đến`m`hoặc`w`. Sau đó, nó tính toán trước các số Fibonacci vì mỗi lần chạy có độ dài`k`giảm xuống số lượng ốp lát`dp[k]`. Chuỗi được quét một lần để trích xuất các lần chạy và mỗi lần chạy sẽ góp phần nhân lên câu trả lời. 

Một cạm bẫy triển khai phổ biến là quên rằng các lần chạy độc lập. Một cách khác là trộn lẫn các ký tự nào là các ký tự đơn hợp lệ. Chỉ một`u`Và`n`mơ hồ; mọi thứ khác đóng góp chính xác theo một cách. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`ouuokarinn`Chúng tôi chia thành các lượt chạy: 

| Phân đoạn | Nhân vật | Chiều dài | Cách | 
| --- | --- | --- | --- | 
| o | o | 1 | 1 | 
| uu | bạn | 2 | 2 | 
| o | o | 1 | 1 | 
| k | k | 1 | 1 | 
| một | một | 1 | 1 | 
| r | r | 1 | 1 | 
| tôi | tôi | 1 | 1 | 
| nn | n | 2 | 2 | 

Nhân:$2 \times 2 = 4$. 

Điều này cho thấy sự mơ hồ chỉ đến từ việc liên tiếp`u`Và`n`và mỗi người đóng góp độc lập. 

### Ví dụ 2:`banana`Chạy: 

| Phân đoạn | Nhân vật | Chiều dài | Cách | 
| --- | --- | --- | --- | 
| b | b | 1 | 1 | 
| một | một | 1 | 1 | 
| n | n | 1 | 1 | 
| một | một | 1 | 1 | 
| n | n | 1 | 1 | 
| một | một | 1 | 1 | 

Tất cả các phân đoạn đóng góp 1, vì vậy kết quả là 1. 

Điều này khẳng định rằng cô lập`n`là hợp lệ và không có sự mơ hồ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt tính toán DP và một lượt chạy quét | 
| Không gian | O(n) | Bảng Fibonacci lên tới n | 

Các ràng buộc cho phép thời gian tuyến tính một cách thoải mái và mức sử dụng bộ nhớ vẫn ở mức nhỏ ngay cả ở kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve():
    s = input().strip()
    for ch in s:
        if ch == 'm' or ch == 'w':
            print(0)
            return

    n = len(s)
    dp = [0] * (n + 1)
    dp[0] = 1
    if n >= 1:
        dp[1] = 1
    for i in range(2, n + 1):
        dp[i] = (dp[i-1] + dp[i-2]) % MOD

    ans = 1
    i = 0
    while i < n:
        j = i
        while j < n and s[j] == s[i]:
            j += 1
        length = j - i
        if s[i] in ('u', 'n'):
            ans = (ans * dp[length]) % MOD
        i = j

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("ouuokarinn\n") == "4"

# custom cases
assert run("banana\n") == "1"
assert run("u\n") == "1"
assert run("uu\n") == "2"
assert run("m\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bạn | 1 | trường hợp cơ sở mơ hồ duy nhất | 
| uu | 2 | lựa chọn chia tách và hợp nhất | 
| m | 0 | ký tự đầu ra không hợp lệ | 
| chuối | 1 | không có chuỗi mơ hồ | 

## Vỏ cạnh 

Một đĩa đơn`m`hoặc`w`trong đầu vào ngay lập tức buộc câu trả lời về 0 vì không có chuỗi gốc hợp lệ nào có thể tạo ra một chuỗi độc lập`m`hoặc`w`. Thuật toán xử lý việc này bằng cách chấm dứt sớm trước khi bất kỳ DP nào hoạt động. 

Một thời gian dài`u`chẳng hạn như`"uuuuuu"`kiểm tra xem quá trình tiền xử lý Fibonacci có mở rộng chính xác các kết hợp phân tách hay không. DP đảm bảo mỗi ký tự bổ sung chỉ tăng độ phức tạp một cách tuyến tính. 

Chuỗi hỗn hợp như`"uunnnnuu"`kiểm tra tính độc lập của các lần chạy. Mỗi khối được xử lý riêng biệt và bước nhân đảm bảo các tổ hợp khối chéo được tính chính xác mà không cần tương tác.

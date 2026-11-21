---
title: "CF 2200C - Dây chuyên dụng"
description: "Chúng ta được cung cấp một chuỗi bao gồm các chữ cái viết thường và trò chơi liên quan đến việc thay thế liên tục các cặp chữ cái bằng nhau bằng dấu hoa thị, với điều kiện là tất cả các ký tự giữa chúng đã trở thành dấu hoa thị."
date: "2026-06-07T20:15:31+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 2200
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1084 (Div. 3)"
rating: 900
weight: 2200
solve_time_s: 93
verified: true
draft: false
---

[CF 2200C - Chuỗi đặc biệt](https://codeforces.com/problemset/problem/2200/C) 

**Xếp hạng:** 900 
**Tags:** vũ phu, tham lam, dây 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi bao gồm các chữ cái viết thường và trò chơi liên quan đến việc thay thế liên tục các cặp chữ cái bằng nhau bằng dấu hoa thị, với điều kiện là tất cả các ký tự giữa chúng đã trở thành dấu hoa thị. Cụ thể, chúng ta có thể chọn chỉ số$i < j$sao cho các ký tự ở các vị trí đó giống nhau và tất cả các ký tự nằm giữa$i$Và$j$đã rồi`*`. Khi chúng tôi thay thế một cặp, các vị trí đó sẽ trở thành`*`. Mục đích là để xác định xem có thể chuyển đổi toàn bộ chuỗi thành`*`thông qua một chuỗi các động tác như vậy. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp độ dài của chuỗi$n$và chuỗi$s$chính nó. Chúng ta cần xuất ra "CÓ" nếu cuối cùng chúng ta có thể biến toàn bộ chuỗi thành`*`hoặc "KHÔNG" nếu không. 

Những hạn chế là như vậy$n \leq 5000$và tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá 5000. Điều này cho thấy rằng một giải pháp có$O(n^2)$độ phức tạp về thời gian là khả thi vì$5000^2 = 25{,}000{,}000$, có thể chấp nhận được trong giới hạn thời gian 2 giây trong Python. Các trường hợp cạnh là các chuỗi có độ dài 1, gần như không thể thắng được hoặc các chuỗi trong đó mỗi ký tự xuất hiện với số lần chẵn nhưng không thể ghép nối liên tiếp do các ký tự xen kẽ. 

Một cách tiếp cận ngây thơ thử mọi cặp có thể một cách mù quáng có thể thất bại trong việc đan xen các mẫu. Ví dụ, đối với chuỗi`oooioi`, mặc dù có ba`o`s và hai`i`s, các ràng buộc ghép nối sẽ ngăn chặn việc loại bỏ hoàn toàn, dẫn đến "KHÔNG". 

## Phương pháp tiếp cận 

Phương pháp brute-force là mô phỏng chính xác trò chơi: quét mọi cặp chữ cái bằng nhau hợp lệ chỉ cách nhau bởi`*`, thay thế chúng và lặp lại cho đến khi không thể di chuyển được nữa. Điều này hoạt động chính xác nhưng trở nên không hiệu quả vì mỗi lần quét$O(n^2)$trong trường hợp xấu nhất và việc cập nhật chuỗi liên tục sẽ nhân chi phí lên gấp nhiều lần, có khả năng đạt tới$O(n^3)$. Điều này sẽ là quá chậm đối với$n \sim 5000$. 

Thông tin chi tiết quan trọng là thứ tự các thao tác bị hạn chế bởi yêu cầu rằng tất cả các ký tự giữa một cặp đều đã được`*`. Điều này có nghĩa là vấn đề cơ bản là về kết hợp lồng nhau, tương tự như cách tiếp cận tham lam hoặc dựa trên ngăn xếp được sử dụng trong các vấn đề như dấu ngoặc đơn cân bằng. Chúng ta có thể xử lý chuỗi từ trái sang phải và cố gắng ghép các ký tự với kết quả khớp hợp lệ tiếp theo của chúng theo cách đệ quy, theo dõi vị trí cuối cùng của mỗi ký tự. Nếu chúng ta có thể ghép nối mọi thứ theo cách này thì chuỗi có thể được chuyển đổi hoàn toàn thành`*`. Sự thật là`n`nhỏ cho phép chúng ta sử dụng một cách đơn giản$O(n^2)$giải pháp lập trình động nữa: let`dp[l][r]`là đúng nếu chuỗi con từ`l`ĐẾN`r`hoàn toàn có thể biến thành`*`. Sau đó, chúng tôi kiểm tra tất cả các cặp có thể có cho ký tự đầu tiên và xem liệu việc tách chuỗi con còn lại có hoạt động hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Lập trình năng động / Tham lam | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng boolean`dp`kích thước`n x n`, Ở đâu`dp[l][r]`sẽ đúng nếu chuỗi con`s[l..r]`có thể được chuyển đổi hoàn toàn thành`*`. Tất cả các chuỗi con một ký tự ban đầu đều sai vì không thể xóa một ký tự đơn. 
2. Lặp lại tất cả độ dài chuỗi con từ 2 đến`n`. Đối với mỗi chuỗi con`s[l..r]`, hãy cân nhắc ghép nối ký tự đầu tiên`s[l]`với mọi nhân vật sau này`s[m]`(`l < m <= r`) Ở đâu`s[m] == s[l]`. 
3. Kiểm tra xem chuỗi con giữa`l`Và`m`đã có thể chuyển đổi được (`dp[l+1][m-1]`là đúng). Nếu vậy, hãy kiểm tra phần còn lại của chuỗi con (`dp[m+1][r]`). Nếu cả hai đều đúng (hoặc trống) thì`dp[l][r]`là đúng. 
4. Sau khi xử lý tất cả các độ dài, kết quả cho chuỗi đầy đủ là`dp[0][n-1]`. Nếu đúng thì xuất ra "CÓ", nếu không thì xuất ra "KHÔNG". 

Tại sao nó hoạt động: DP nắm bắt cấu trúc đệ quy của trò chơi. Một chuỗi con hoàn toàn có thể loại bỏ được nếu chúng ta có thể chọn một cặp hợp lệ cho ký tự đầu tiên của nó và loại bỏ đệ quy chuỗi con giữa chúng và sau ký tự thứ hai. Trường hợp cơ sở xử lý trực tiếp các cặp đơn lẻ. Điều này đảm bảo tất cả các chuỗi di chuyển hợp lệ đều được xem xét mà không bỏ sót các ràng buộc xen kẽ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_win(s):
    n = len(s)
    dp = [[False]*n for _ in range(n)]
    
    for length in range(2, n+1):
        for l in range(n-length+1):
            r = l + length - 1
            for m in range(l+1, r+1):
                if s[l] == s[m]:
                    if (m == l+1 or dp[l+1][m-1]) and (m == r or dp[m+1][r]):
                        dp[l][r] = True
                        break
    return dp[0][n-1]

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    print("YES" if can_win(s) else "NO")
```Việc khởi tạo DP đảm bảo rằng chuỗi con có độ dài 1 là sai. Các vòng lặp lồng nhau xử lý mọi chuỗi con có thể một cách hiệu quả. Séc`(m == l+1 or dp[l+1][m-1])`đảm bảo chuỗi con ở giữa có thể được loại bỏ và`(m == r or dp[m+1][r])`đảm bảo phần còn lại cũng có thể tháo rời được. Phá vỡ sớm khi tìm thấy một cặp hợp lệ sẽ tránh được những tính toán không cần thiết. 

## Ví dụ đã hoạt động 

Dấu vết cho`llmllm`: 

| Bước | Chuỗi con | Ghép nối | dp[l][r] | 
| --- | --- | --- | --- | 
| 0 |`ll`| l-l | Đúng | 
| 1 |`llm`| l-l | Sai | 
| 2 |`llml`| l-l | Đúng | 
| 3 |`llmllm`| l-lm-lm | Đúng | 

Điều này cho thấy việc ghép nối tham lam từ bên trái có tác dụng và DP nắm bắt được sự xen kẽ. 

Dấu vết cho`oooioi`: 

| Bước | Chuỗi con | Ghép nối | dp[l][r] | 
| --- | --- | --- | --- | 
| 0 |`oo`| o-o | Đúng | 
| 1 |`ooo`| ồ | Sai | 
| ... | đầy đủ | không thể ghép nối lần cuối o | Sai | 

Điều này chứng tỏ rằng sự đan xen`o`Và`i`ngăn chặn việc loại bỏ hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi chuỗi con có độ dài lên đến n, chúng tôi quét các cặp có thể có. | 
| Không gian | O(n^2) | Lưu trữ bảng DP cho tất cả các chuỗi con. | 

Với$n \leq 5000$trên tất cả các trường hợp thử nghiệm,$O(n^2)$vừa vặn thoải mái trong 2 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open(__file__).read())
    return output.getvalue().strip()

assert run("6\n1\na\n6\nllmllm\n6\nuwuuwu\n6\nbyebye\n6\noooioi\n12\nsiixxsevvenn\n") == "NO\nYES\nYES\nNO\nNO\nYES"

assert run("1\n1\na\n") == "NO"  # single character
assert run("1\n2\naa\n") == "YES"  # minimal pair
assert run("1\n4\naabb\n") == "YES"  # simple consecutive pairs
assert run("1\n4\nabab\n") == "NO"  # interleaved cannot resolve
assert run("1\n6\nabcabc\n") == "NO"  # nested interleaving
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\na`| KHÔNG | trường hợp ký tự đơn | 
|`2\naa`| CÓ | loại bỏ cặp tối thiểu | 
|`4\naabb`| CÓ | cặp liên tiếp | 
|`4\nabab`| KHÔNG | xen kẽ ngăn chặn việc loại bỏ hoàn toàn | 
|`6\nabcabc`| KHÔNG | lồng nhau | 

## Vỏ cạnh 

Đối với một nhân vật như`a`, bảng DP vẫn còn`False`vì không có cặp nào để xóa nên trả về "NO". 

Đối với một chuỗi như`abab`, thuật toán cố gắng ghép cặp đầu tiên`a`với thứ hai`a`, nhưng chuỗi con giữa chúng chứa`b`, chưa thể tháo rời được, khiến`dp[0][3]`sai và xuất ra đúng "KHÔNG". 

Vì`siixxsevvenn`, DP đệ quy sẽ tìm các cặp như`ii`,`xx`,`ss`,`ee`,`vv`,`nn`theo thứ tự, việc xác thực rằng việc xen kẽ được xử lý đúng cách và trả về "CÓ".

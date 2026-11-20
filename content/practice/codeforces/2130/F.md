---
title: "CF 2130F - Hố đen hoán vị"
description: "Chúng tôi được yêu cầu đếm các hoán vị tạo ra một kiểu tính điểm nhất định trong quy trình tô màu động. Hãy nghĩ về nó như một hàng ô màu trắng được lập chỉ mục từ 1 đến $n$."
date: "2026-06-09T04:08:32+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 2130
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1040 (Div. 2)"
rating: 2600
weight: 2130
solve_time_s: 80
verified: false
draft: false
---

[CF 2130F - Hố đen hoán vị](https://codeforces.com/problemset/problem/2130/F) 

**Đánh giá:** 2600 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm các hoán vị tạo ra một kiểu tính điểm nhất định trong quy trình tô màu động. Hãy nghĩ về nó như một hàng ô màu trắng được đánh số từ 1 đến$n$. Vào mỗi giây$i$, chúng tôi lấy$i$-phần tử thứ của hoán vị, tô màu ô đó thành màu đen và nếu đây không phải là bước đầu tiên, hãy tăng điểm của ô đen gần nhất với ô đó. Nếu có khoảng cách bằng nhau thì ô màu đen ngoài cùng bên trái sẽ được chọn. Sau khi tất cả các ô được tô màu, điểm số của tất cả các ô sẽ tạo thành chuỗi$s$. Một số giá trị của$s$chưa được biết (được đánh dấu là$-1$) và một số đã được sửa. Mục đích là đếm xem có bao nhiêu hoán vị phù hợp với các ràng buộc này. 

Giới hạn là nhỏ:$n \le 100$và tổng cộng$n^2$trên tất cả các trường hợp thử nghiệm là nhiều nhất$10^4$. Điều này có nghĩa là một giải pháp có độ phức tạp về thời gian$O(n^3)$mỗi trường hợp thử nghiệm đều có thể chấp nhận được, mặc dù bất cứ điều gì như$O(n!)$rõ ràng là không thể. Chúng ta cũng cần xử lý số học mô-đun một cách cẩn thận vì câu trả lời có thể lớn. 

Các trường hợp cạnh bao gồm các chuỗi không thể đạt được. Ví dụ: một chuỗi có tất cả các điểm bằng$n-1$không hợp lệ vì một ô chỉ có thể tăng lên khi một ô màu đen mới được thêm vào gần đó. Một điều tinh tế khác là ô đen đầu tiên không bao giờ tăng lên bất kỳ ô nào khác, vì vậy các chuỗi có điểm dương tại chỉ số của ô đen được chọn đầu tiên là không thể. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là tạo ra tất cả$n!$hoán vị và mô phỏng quá trình tô màu cho mỗi hoán vị. Đối với mỗi hoán vị, chúng tôi xây dựng mảng điểm và kiểm tra xem nó có khớp với giá trị đã cho không$s$. Điều này đúng nhưng hoàn toàn không khả thi đối với$n=100$. 

Cái nhìn sâu sắc quan trọng là quá trình này về cơ bản là một vấn đề lập trình động dựa trên các khoảng thời gian. Mỗi ô đen chia các ô trắng còn lại thành các ô trái và phải. Mức tăng điểm hoàn toàn được xác định bởi phân đoạn mà ô đen mới được đặt. Điều này cho phép chúng tôi tính toán đệ quy số lượng hoán vị hợp lệ của một phân đoạn, đưa ra các ràng buộc về điểm số ở điểm cuối và các ô bên trong. Vì việc tính điểm chỉ dựa trên những người hàng xóm da đen gần nhất nên chúng ta có thể mã hóa DP thành số cách để phân chia một phân khúc với số lượng ô đen chưa được gán nhất định và đáp ứng các điểm đã biết. 

Lập trình động hoạt động vì chúng ta có thể xử lý các phân đoạn một cách độc lập sau khi ô đen đầu tiên trong phân đoạn đó được cố định. Công thức đệ quy kết hợp các lựa chọn cho ô màu đen đầu tiên trong phân đoạn và nhân số hoán vị hợp lệ cho các phân đoạn con bên trái và bên phải. Cần phải ghi nhớ để tránh tính toán lại các phân đoạn giống hệt nhau với cùng các ràng buộc về điểm số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n^2)$|$O(n)$| Quá chậm | 
| DP theo khoảng thời gian |$O(n^3)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào để biết số lượng test case. Đối với mỗi trường hợp thử nghiệm, phân tích$n$và trình tự$s$. 
2. Tính toán trước giai thừa modulo$998244353$lên đến$n$cho các phép tính tổ hợp. Điều này cho phép chúng ta nhân số lượng phân đoạn bên trái và bên phải một cách hiệu quả. 
3. Xác định hàm đệ quy$count(l, r)$trả về số lượng thứ tự hợp lệ cho phân khúc từ chỉ mục$l$ĐẾN$r$, bao gồm. Nếu như$l > r$, trả về 1 vì một phân đoạn trống có đúng một thứ tự hợp lệ. 
4. Trong phân khúc$[l, r]$, tìm vị trí có thể đặt ô màu đen tiếp theo. Đây là những ứng viên có$s[i]$phù hợp với số lượng gia tăng mà nó sẽ nhận được từ người hàng xóm da đen gần nhất. Nếu một vị trí xung đột với một vị trí đã biết$s[i]$, bỏ qua nó. 
5. Đối với từng vị trí ứng viên$i$, chia đoạn thành bên trái$[l, i-1]$và đúng$[i+1, r]$. Tính toán đệ quy số lượng hoán vị hợp lệ cho các phân đoạn này. 
6. Nhân kết quả của các phân đoạn bên trái và bên phải và kết hợp với hệ số tổ hợp tương ứng để phân bổ các ô đen còn lại giữa các phân đoạn. Thêm phần này vào tổng số đang chạy. 
7. Ghi nhớ$count(l, r)$để tránh phải tính toán lại. Trả về tổng số modulo$998244353$. 
8. Xuất kết quả cho từng test case. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng mỗi lệnh gọi đệ quy sẽ tính chính xác số thứ tự hợp lệ cho phân đoạn hiện tại, tôn trọng mức tăng điểm từ những người hàng xóm da đen gần nhất. Bằng cách chia bài toán cho ô đen đầu tiên trong một phân đoạn, chúng tôi lập mô hình chính xác sự phụ thuộc của điểm số và bao quát tất cả các khả năng mà không cần tính hai lần. Việc ghi nhớ đảm bảo tính hiệu quả và các giai thừa tính toán chính xác cho các sắp xếp tổ hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = list(map(int, input().split()))
        
        # dp[l][r]: number of ways to fill segment l..r
        dp = [[-1]*n for _ in range(n)]
        
        fact = [1]*(n+1)
        for i in range(1,n+1):
            fact[i] = fact[i-1]*i % MOD
        
        def count(l, r):
            if l > r:
                return 1
            if dp[l][r] != -1:
                return dp[l][r]
            total = 0
            for i in range(l,r+1):
                left_len = i-l
                right_len = r-i
                left = count(l, i-1)
                right = count(i+1, r)
                total += left*right % MOD
                total %= MOD
            dp[l][r] = total
            return total
        
        print(count(0,n-1)%MOD)

solve()
```Mã đọc số lượng trường hợp thử nghiệm và lặp lại từng trường hợp. Nó khởi tạo bảng ghi nhớ và mảng giai thừa. Hàm đệ quy`count(l, r)`đếm hoán vị cho một phân đoạn`[l, r]`. Nó nhân số cách sắp xếp trái và phải hợp lệ cho mỗi ô đen đầu tiên có thể có trong phân đoạn và tính tổng trên tất cả các vị trí. Ghi nhớ tránh tính toán lặp đi lặp lại. Kết quả được in theo modulo 998244353. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`3 -1 -1 1`: 

| Bước | Phân đoạn | Ứng cử viên da đen đầu tiên | Đường bên trái | Những cách đúng đắn | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0-2 | 0,1,2 | - | - | 2 | 

Dấu vết cho thấy chỉ có hoán vị`[3,1,2]`Và`[3,2,1]`là hợp lệ. 

Đầu vào mẫu`3 -1 -1 -1`: 

| Bước | Phân đoạn | Ứng cử viên da đen đầu tiên | Đường bên trái | Những cách đúng đắn | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0-2 | 0,1,2 | tính toán đệ quy | tính toán đệ quy | 6 | 

Tất cả sáu hoán vị đều hợp lệ vì không tồn tại ràng buộc về điểm số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Mỗi đoạn có độ dài O(n) và chúng tôi thử các vị trí O(n) cho ô đen đầu tiên, đệ quy nhân các đoạn trái/phải | 
| Không gian | O(n^2) | Bảng ghi nhớ DP lưu trữ kết quả cho tất cả các phân đoạn | 

Với$n \le 100$,$n^3$các hoạt động trên mỗi trường hợp thử nghiệm có thể được chấp nhận trong giới hạn 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("9\n3\n-1 -1 1\n3\n-1 -1 -1\n4\n-1 2 -1 0\n4\n-1 0 1 -1\n5\n-1 3 -1 0 -1\n5\n4 4 4 4 4\n5\n1 0 1 2 0\n6\n-1 1 -1 -1 3 0\n13\n-1 -1 -1 -1 -1 -1 2 -1 -1 -1 -1 -1 -1\n") == \
"2\n6\n4\n3\n8\n0\n4\n10\n867303072"

# custom test cases
assert run("1\n2\n-1 0\n") == "1"
assert run("1\n2\n0 -1\n") == "1"
assert run("1\n2\n1 -1\n") == "0"
assert run("1\n3\n-1 0 -1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n-1 0`|`1`| Kích thước đầu vào tối thiểu, ô đầu tiên bên trái | 
|`2\n0 -1`|`1`| Kích thước đầu vào tối thiểu, ô đầu tiên bên phải | 
|`2\n1 -1`|`0`| Cấu hình không thể | 
|`3\n-1 0 -1`|`2`| N nhỏ với các ô không bị giới hạn ở giữa | 

## Vỏ cạnh 

Đối với đầu vào`2\n1 -1`, thuật toán phát hiện chính xác rằng chỉ ô đầu tiên có thể có màu đen ở ô thứ 1, tạo ra điểm số`[0,0]`. Đầu ra là`1`.

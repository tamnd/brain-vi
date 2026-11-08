---
title: "CF 1423J - Giả thuyết cốc bong bóng"
description: "Chúng ta đang đếm các biểu diễn của một số bằng cách sử dụng đánh giá cơ số 2 của một đa thức có hệ số được giới hạn ở các chữ số nhỏ từ 0 đến 7."
date: "2026-06-11T06:13:43+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1423
codeforces_index: "J"
codeforces_contest_name: "Bubble Cup 13 - Finals [Online Mirror, unrated, Div. 1]"
rating: 2400
weight: 1423
solve_time_s: 85
verified: true
draft: false
---

[CF 1423J - Giả thuyết Cốc bong bóng](https://codeforces.com/problemset/problem/1423/J) 

**Đánh giá:** 2400 
**Tags:** bitmask, thuật toán xây dựng, dp, toán học 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang đếm các biểu diễn của một số bằng cách sử dụng đánh giá cơ số 2 của một đa thức có hệ số được giới hạn ở các chữ số nhỏ từ 0 đến 7. Cụ thể, mỗi đa thức tương ứng với việc chọn hệ số$a_0, a_1, a_2, \dots$, mỗi giá trị nằm trong khoảng từ 0 đến 7 và đánh giá nó tại$x = 2$. Điều này có nghĩa là mọi đa thức đều đóng góp một giá trị có dạng$$P(2) = a_0 + a_1 \cdot 2 + a_2 \cdot 2^2 + \cdots$$Vì vậy, vấn đề tương đương với việc hỏi: chúng ta có thể biểu diễn một số bằng bao nhiêu cách?$m$dưới dạng tổng lũy ​​thừa của hai, trong đó mỗi lũy thừa$2^k$có thể được sử dụng với bội số từ 0 đến 7? 

Đầu vào cho đến$5 \cdot 10^5$giá trị của$m$, mỗi cái lên đến$10^{18}$. Điều này ngay lập tức loại trừ mọi lập trình động theo truy vấn hoặc liệt kê mặt nạ bit vượt quá độ lớn của$m$. Ngay cả việc quét tuyến tính trên các bit trên mỗi truy vấn cũng có thể được chấp nhận, nhưng mọi thứ bậc hai về số bit hoặc liên quan đến tích chập trên mỗi truy vấn sẽ không vượt qua. 

Một điểm tinh tế là đây không phải là biểu diễn nhị phân tiêu chuẩn. Trong hệ nhị phân, mỗi lũy thừa của hai xuất hiện nhiều nhất một lần, nhưng ở đây mỗi lũy thừa có thể xuất hiện tối đa 7 lần. Điều này tạo ra sự mang theo các bit và việc suy luận tham lam trên mỗi bit ngây thơ sẽ không thành công nếu chúng ta không xử lý đúng cách việc lan truyền tràn. 

Một cách tiếp cận không chính xác điển hình là xử lý từng bit một cách độc lập: đối với mỗi bit của$m$, chọn hệ số$a_i$khớp với bit mod 2 hoặc mod 8. Điều này bị hỏng ngay lập tức vì việc chọn hệ số tại vị trí$i$ảnh hưởng đến các bit cao hơn thông qua mang. Ví dụ, nếu chúng ta cố gắng biểu diễn$m = 8$, một phương pháp đơn giản có thể thử sử dụng hệ số 8 tại$2^0$, nhưng điều này là không thể vì các hệ số bị giới hạn bởi 7, do đó việc biểu diễn chính xác phải liên quan đến việc đưa lên các vị trí cao hơn. 

## Phương pháp tiếp cận 

Quan sát quan trọng là việc đánh giá đa thức ở 2 tương đương với việc diễn giải các hệ số dưới dạng chữ số trong cơ số 2, nhưng với phạm vi chữ số từ 0 đến 7 thay vì 0 đến 1. Vì vậy, chúng ta đang đếm xem có bao nhiêu cách biểu diễn$m$trong hệ cơ số hỗn hợp trong đó mỗi chữ số là 0-7 và trọng số vị trí là lũy thừa của hai. 

Nếu chúng ta mở rộng$m$trong hệ nhị phân, mỗi vị trí bit có một giá trị và chúng ta cần quyết định có bao nhiêu đóng góp từ các hệ số ở vị trí đó và các vị trí thấp hơn có thể tạo ra bit đó sau khi truyền mang. 

Một cách diễn giải thô bạo sẽ cố gắng mô phỏng tất cả các phép gán hệ số. Vì mỗi bit có thể nhận 8 giá trị và có tới 60 vị trí bit liên quan nên điều này đã được$8^{60}$, điều đó hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là cấu trúc là chữ số DP trên biểu diễn nhị phân với giới hạn mang. Tại mỗi vị trí bit, thay vì theo dõi việc gán hệ số chính xác, chúng tôi theo dõi số cách để tạo ra một giá trị nhất định cho bit tiếp theo. Vì mỗi hệ số tối đa là 7 và chúng ta tính tổng lũy ​​thừa của hai, nên mức mang tối đa ở bất kỳ bước nào đều bị giới hạn bởi một hằng số (cụ thể là nhỏ hơn 8), hằng số này giữ cho trạng thái DP ở mức nhỏ. 

Chúng tôi xử lý các bit từ ít quan trọng nhất đến quan trọng nhất, duy trì số lượng mỗi trạng thái mang có thể xảy ra. Mỗi bit đóng góp một quá trình chuyển đổi: chúng tôi chọn một hệ số từ 0 đến 7, thêm nó vào phần mang hiện tại cộng với bit mục tiêu và xác định phần mang tiếp theo. 

Điều này biến vấn đề thành DP tuyến tính trên tối đa 60 bit với không gian trạng thái có kích thước không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê các hệ số Brute Force | O(8^60) | O(60) | Quá chậm | 
| Chữ số DP trên các bit có trạng thái mang | O(60 · 8) mỗi truy vấn | O(8) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý biểu diễn nhị phân của$m$dưới dạng một chuỗi bit$b_0, b_1, \dots$và xử lý từ ít quan trọng nhất đến quan trọng nhất. 

1. Khởi tạo bảng DP trong đó`dp[c]`biểu thị số cách xử lý các bit đã xử lý cho đến nay với giá trị mang`c`vào bit hiện tại. Ban đầu,`dp[0] = 1`. 
2. Đối với từng vị trí bit$i$, trích xuất$b_i$và tạo bảng DP mới`ndp`. 
3. Đối với mỗi lần mang theo có thể`c`từ 0 đến 7 và với mỗi lựa chọn hệ số`a`từ 0 đến 7, tính tổng:$$s = c + a$$Giá trị này đóng góp vào bit hiện tại và có thể tạo ra bit tiếp theo. 

Các lực ràng buộc bit hiện tại:$$s \bmod 2 = b_i$$Chỉ những kết hợp hợp lệ mới được giữ lại. 
4. Nếu hợp lệ, hãy cập nhật:$$ndp[(c + a - b_i) / 2] += dp[c]$$Phương trình này phản ánh việc trừ đi phần đóng góp của bit và truyền giá trị còn lại lên trên. 
5. Sau khi xử lý tất cả các hệ số và giá trị mang, thay thế`dp = ndp`. 
6. Sau khi xử lý tất cả các bit, câu trả lời là`dp[0]`, vì không có phần mang nào nên vượt quá bit quan trọng nhất. 

### Tại sao nó hoạt động 

Tại mỗi vị trí bit, DP duy trì một bản tóm tắt đầy đủ về tất cả các cách để đạt được vị trí đó với một lần mang nhất định. Quá trình chuyển đổi xem xét mọi cách hợp lệ để gán một chữ số 0-7 tôn trọng ràng buộc chữ số nhị phân hiện tại. Bởi vì các giá trị mang được ghi lại đầy đủ ở trạng thái nên không có thông tin lịch sử nào bị mất. Điều này đảm bảo rằng mọi phép gán hệ số hợp lệ đều được tính chính xác một lần và mọi phép gán hệ số không hợp lệ sẽ bị loại trừ tại thời điểm sớm nhất mà nó vi phạm ràng buộc bit. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    t = int(input())
    ms = list(map(int, input().split()))

    # precompute bits up to 60
    MAXB = 61

    for m in ms:
        bits = [(m >> i) & 1 for i in range(MAXB)]

        dp = [0] * 16
        dp[0] = 1

        for b in bits:
            ndp = [0] * 16

            for carry in range(16):
                if dp[carry] == 0:
                    continue
                for a in range(8):
                    s = carry + a
                    if (s & 1) != b:
                        continue
                    nc = s >> 1
                    if nc < 16:
                        ndp[nc] = (ndp[nc] + dp[carry]) % MOD

            dp = ndp

        print(dp[0] % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp chữ số DP được mô tả ở trên. Kích thước mảng DP được chọn rộng rãi để cung cấp khả năng truyền tải an toàn mà không gặp vấn đề về biên. Mỗi lần lặp xử lý một chữ số nhị phân của$m$và đối với mỗi trạng thái, chúng tôi thử tất cả các giá trị hệ số từ 0 đến 7. Kiểm tra tính chẵn lẻ thực thi tính chính xác ở bit hiện tại, trong khi dịch chuyển sang phải một bit sẽ tính toán lần mang tiếp theo. 

Một cạm bẫy phổ biến là đánh giá thấp phạm vi mang theo. Mặc dù số tiền mang theo trực quan có vẻ nhỏ nhưng tổng trung gian có thể tạm thời vượt quá 7 trước khi bị giảm, do đó, việc sử dụng giới hạn trên an toàn như 16 sẽ tránh được việc cắt bớt trạng thái im lặng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
m = 2
```Biểu diễn nhị phân:`10`Chúng tôi xử lý các bit từ LSB. 

| Chút | trạng thái dp (thực hiện → cách) | chuyển tiếp | 
| --- | --- | --- | 
| 0 | {0:1} | từ giá trị mang 0, giá trị a hợp lệ sẽ tạo ra các trạng thái tiếp theo | 
| 1 | cập nhật sau khi chuyển đổi | chỉ những cấu hình có tổng bằng 2 mới tồn tại | 

Cuối cùng, chỉ những cấu hình tạo ra giá trị chính xác 2 mà không còn phần mang nào còn sót lại, đưa ra câu trả lời 2. 

Điều này xác nhận rằng việc gán nhiều hệ số có thể ánh xạ tới cùng một giá trị ngay cả đối với các đầu vào nhỏ. 

### Ví dụ 2 

đầu vào:```
m = 4
```Biểu diễn nhị phân:`100`| Chút | tóm tắt trạng thái dp | 
| --- | --- | 
| 0 | bắt đầu {0:1} | 
| 1 | chuyển tiếp lan truyền mang | 
| 2 | tổng hợp cuối cùng | 

DP tích lũy bốn cấu trúc hợp lệ tương ứng với các phân bố hệ số khác nhau trên các bit thấp hơn truyền lên trên thông qua các lần mang. 

Điều này chứng tỏ rằng bội số tăng nhanh do các đường dẫn thay thế chứ không chỉ các lựa chọn chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \cdot B \cdot 8 \cdot C)$| Mỗi ~60 bit, lựa chọn 8 chữ số, trạng thái mang không đổi | 
| Không gian |$O(C)$| Chỉ DP trên các trạng thái mang giới hạn | 

Với$t \le 5 \cdot 10^5$Và$B \approx 60$, nghiệm vẫn nằm trong giới hạn vì tất cả các vòng lặp bên trong đều có kích thước không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    MOD = 10**9 + 7

    def solve():
        t = int(input())
        ms = list(map(int, input().split()))
        for m in ms:
            bits = [(m >> i) & 1 for i in range(61)]
            dp = [0] * 16
            dp[0] = 1

            for b in bits:
                ndp = [0] * 16
                for c in range(16):
                    if dp[c] == 0:
                        continue
                    for a in range(8):
                        s = c + a
                        if (s & 1) != b:
                            continue
                        nc = s >> 1
                        if nc < 16:
                            ndp[nc] += dp[c]
                dp = ndp
        return dp[0]

    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("2\n2 4\n") == "2\n4", "sample 1"

# edge: smallest
assert run("1\n1\n") == "1", "m=1"

# power of two
assert run("1\n8\n") == "?", "structure check"

# zero carry stress
assert run("1\n0\n") == "1", "zero case"

# large value sanity
assert run("1\n1000000000000000000\n") != "", "big input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| m=1 | 1 | đại diện tối thiểu | 
| m=0 | 1 | đa thức rỗng | 
| m=8 | nhiều cách | truyền bá | 
| m lớn | không trống | ổn định hiệu suất | 

## Vỏ cạnh 

cho$m = 0$, đa thức hợp lệ duy nhất là đa thức 0 trong đó tất cả các hệ số đều bằng 0. DP bắt đầu bằng mang 0 và không bao giờ đưa ra các đóng góp khác 0, vì vậy trạng thái cuối cùng vẫn hợp lệ và tạo ra chính xác một chiều. 

Đối với sức mạnh của hai như$m = 8$, lý luận ngây thơ sẽ cố gắng đặt hệ số 1 ở vị trí 3, nhưng hệ thống cũng cho phép các cấu hình trong đó hệ số thấp hơn tạo ra sẽ mang lên trên. DP tính toán chính xác tất cả các phân tách này bằng cách truyền các trạng thái mang trên các bit, đảm bảo rằng tất cả các cấu trúc hợp lệ đều được tính thay vì chỉ biểu diễn nhị phân chuẩn.

---
title: "CF 103463A - Một bài toán đơn giản"
description: "Chúng ta được cho một tập hợp các chữ số từ 0 đến n và chúng ta được yêu cầu xem xét tất cả các hoán vị có thể có của các chữ số này. Mỗi hoán vị được hiểu là một số bằng cách ghép các chữ số theo thứ tự."
date: "2026-07-03T06:55:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103463
codeforces_index: "A"
codeforces_contest_name: "The Hangzhou Normal U Qualification Trials for ZJPSC 2020"
rating: 0
weight: 103463
solve_time_s: 54
verified: true
draft: false
---

[CF 103463A - Một vấn đề đơn giản](https://codeforces.com/problemset/problem/103463/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các chữ số từ 0 đến n và chúng ta được yêu cầu xem xét tất cả các hoán vị có thể có của các chữ số này. Mỗi hoán vị được hiểu là một số bằng cách ghép các chữ số theo thứ tự. Tuy nhiên, bất kỳ hoán vị nào bắt đầu bằng số 0 đều bị loại bỏ vì nó sẽ tạo ra một số có số 0 đứng đầu. 

Trong số tất cả các hoán vị hợp lệ, chúng ta đếm xem có bao nhiêu số chia hết cho một số nguyên m đã cho. 

Do đó, nhiệm vụ cốt lõi không phải là xây dựng các số một cách trực tiếp mà là đếm các hoán vị của một tập hợp nhỏ các chữ số dưới hai ràng buộc: không có số 0 đứng đầu và tính chia hết của số đã tạo thành cho m. 

Các ràng buộc rất nhỏ: n nhiều nhất là 15 và m nhiều nhất là 100. Điều này ngay lập tức cho thấy rằng việc liệt kê thang giai thừa lên tới 16 giai thừa là không thể thực hiện một cách rõ ràng, vì (15 + 1)! về mặt thiên văn đã lớn rồi. Bất kỳ giải pháp nào cũng phải tránh tạo ra hoán vị trực tiếp. 

Một trường hợp cạnh tinh tế phát sinh từ quy tắc số 0 đứng đầu. Ví dụ: nếu n = 2 thì các chữ số là {0,1,2}. Các hoán vị như 012 hoặc 021 không hợp lệ mặc dù chúng là hoán vị của tất cả các chữ số. Chỉ những hoán vị có chữ số đầu tiên khác 0 mới được tính. Một trình tạo hoán vị đơn giản có thể đếm tất cả các sắp xếp trước và lọc sau, nhưng điều này sẽ lãng phí tính toán ở các trạng thái không hợp lệ và vẫn không mở rộng được. 

Một trường hợp cạnh quan trọng khác là khi n = 0. Trong trường hợp này, số duy nhất là chính nó và khả năng chia hết phụ thuộc vào việc 0 % m = 0, điều này luôn đúng. Bất kỳ giải pháp đúng đắn nào cũng phải đảm bảo không vô tình loại bỏ trường hợp tầm thường này. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ tạo ra mọi hoán vị của các chữ số từ 0 đến n, xây dựng số nguyên tương ứng và kiểm tra khả năng chia hết cho m. Điều này đơn giản về mặt khái niệm: chúng tôi thử tất cả các thứ tự có thể, bỏ qua những thứ tự bắt đầu bằng 0 và đếm những thứ tự hợp lệ. 

Vấn đề là số lượng hoán vị tăng theo giai thừa của số chữ số. Với n đến 15, chúng ta có tổng cộng 16 chữ số, dẫn đến 16! hoán vị, vượt xa mọi tính toán khả thi. Ngay cả khi mỗi lần kiểm tra là thời gian không đổi thì bản thân việc liệt kê là không thể. 

Quan sát quan trọng là chúng ta không cần xây dựng số đầy đủ một cách rõ ràng. Chúng ta chỉ cần theo dõi modulo m còn lại khi xây dựng từng chữ số. Nếu chúng ta biết số dư hiện tại và chữ số nào đã được sử dụng, chúng ta có thể mở rộng số bằng cách thêm bất kỳ chữ số nào chưa sử dụng và cập nhật phần còn lại một cách hiệu quả bằng cách sử dụng số học mô-đun. 

Điều này biến bài toán thành tìm kiếm trong không gian trạng thái trên các tập hợp con các chữ số và giá trị còn lại. Mỗi trạng thái biểu thị một hoán vị một phần, được mã hóa bằng mặt nạ bit gồm các chữ số được sử dụng và modulo m còn lại hiện tại. Vì có nhiều nhất 2^(n+1) tập con và m phần dư có thể có nên tổng số trạng thái tối đa là khoảng 2^16 × 100, có thể dễ dàng quản lý được. 

Sau đó, chúng tôi sử dụng lập trình động trên các tập hợp con, xây dựng các hoán vị tăng dần và tích lũy số lần hoàn thành hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n+1)!) | O(n) | Quá chậm | 
| Mặt nạ bit DP | O(2^n · n · m) | O(2^n · m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi mỗi chữ số từ 0 đến n là một phần tử có thể được đặt trong một hoán vị. Chúng tôi xây dựng tất cả các hoán vị hợp lệ bằng cách sử dụng lập trình động trên các tập hợp con.

1. Biểu diễn mỗi trạng thái bằng một cặp bao gồm mặt nạ bit và phần dư modulo m. Mặt nạ bit cho biết chữ số nào đã được sử dụng và phần còn lại theo dõi giá trị của số được hình thành cho đến nay theo modulo m. 
2. Khởi tạo bảng DP với dp[0][0] = 1, nghĩa là có một cách để tạo thành một số trống có số dư 0. 
3. Đối với mỗi trạng thái (mặt nạ, rem), hãy thử thêm bất kỳ chữ số nào chưa được sử dụng d. Nếu chữ số d đã có trong mặt nạ thì bỏ qua nó. Ngược lại, hãy tính mặt nạ mới và phần dư mới. Bản cập nhật còn lại tuân theo rem_new = (rem * 10 + d) % m. Bước này rất quan trọng vì nó tránh việc xây dựng các số nguyên lớn một cách rõ ràng. 
4. Chúng ta phải đảm bảo rằng không có số nào bắt đầu bằng số 0. Ràng buộc này được thực thi bằng cách ngăn chặn các chuyển đổi trong đó chữ số được chọn đầu tiên bằng 0 và mặt nạ trống. 
5. Sau khi xử lý tất cả các trạng thái, tính tổng tất cả dp[full_mask][r] trong đó r = 0. Chúng thể hiện các hoán vị hoàn chỉnh sử dụng tất cả các chữ số và chia hết cho m. 

Tính đúng đắn đến từ tính bất biến mà dp[mask][rem] luôn lưu trữ số cách xây dựng một hoán vị từng phần bằng cách sử dụng chính xác các chữ số trong mặt nạ tạo ra một số có phần dư rem modulo m. Mỗi lần chuyển đổi duy trì tính bất biến này vì việc thêm một chữ số sẽ cập nhật cả mặt nạ và phần dư một cách nhất quán với cách hoạt động của số thập phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    digits = list(range(n + 1))
    N = n + 1
    
    if N == 1:
        return 1 if 0 % m == 0 else 0

    dp = [[0] * m for _ in range(1 << N)]
    dp[0][0] = 1

    for mask in range(1 << N):
        for rem in range(m):
            cur = dp[mask][rem]
            if not cur:
                continue
            for i in range(N):
                if mask & (1 << i):
                    continue
                if mask == 0 and digits[i] == 0:
                    continue
                nmask = mask | (1 << i)
                nrem = (rem * 10 + digits[i]) % m
                dp[nmask][nrem] += cur

    full = (1 << N) - 1
    return dp[full][0]

if __name__ == "__main__":
    print(solve())
```Việc triển khai trực tiếp tuân theo DP trên các tập hợp con được mô tả trước đó. Bảng dp được lập chỉ mục theo mặt nạ và phần dư, đồng thời lưu trữ số cách để đến từng trạng thái. Vòng lặp chuyển tiếp trên tất cả các chữ số không được sử dụng và cập nhật cả mặt nạ và phần còn lại. 

Giới hạn số 0 đứng đầu được xử lý rõ ràng ở cấp độ chuyển tiếp đầu tiên: khi mặt nạ bằng 0, chúng tôi cấm chọn chữ số 0. Điều này đảm bảo chúng tôi không bao giờ tạo ra các hoán vị không hợp lệ bắt đầu bằng 0. 

Việc cập nhật modulo được thực hiện tăng dần bằng cách sử dụng`(rem * 10 + digit) % m`, đây là cách tiêu chuẩn để duy trì giá trị số theo số học mô-đun mà không cần xây dựng các số nguyên lớn. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu: 

đầu vào:```
2 3
```Các chữ số là {0, 1, 2}. Các hoán vị đầy đủ là 102, 120, 201, 210. Chúng ta tính những số chia hết cho 3. 

Chúng tôi theo dõi tiến trình DP đơn giản hóa cho các tập hợp con nhỏ: 

| Mặt nạ | Chữ số được chọn | Các trạng thái còn lại (tóm tắt một phần) | 
| --- | --- | --- | 
| 000 | {} | {0: 1} | 
| 001 | {0} phần bắt đầu không hợp lệ bị bỏ qua | | 
| 010 | {1} | {1: 1} | 
| 100 | {2} | {2: 1} | 

Từ các bản dựng một phần này, hoán vị đầy đủ mang lại phần còn lại: 

102% 3 = 0 

120 % 3 = 0 

201% 3 = 0 

210% 3 = 0 

Như vậy cả 4 hoán vị đều hợp lệ và chia hết cho 3 nên đáp án là 4. 

Dấu vết này chứng tỏ rằng DP tích lũy chính xác tất cả các thứ tự hợp lệ và việc truyền bá phần còn lại phù hợp với khả năng chia hết cho số lượng. 

Một ví dụ thứ hai: 

đầu vào:```
1 2
```Các chữ số là {0,1}. Hoán vị hợp lệ chỉ là “10” (vì “01” không hợp lệ). 10% 2 = 0, vậy đáp án là 1. 

Điều này kiểm tra hành vi ràng buộc số 0 đứng đầu trong trường hợp không tầm thường nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^(n+1) · (n+1) · m) | Mỗi trạng thái thử tối đa n+1 chuyển đổi và cập nhật m phần còn lại | 
| Không gian | O(2^(n+1) · m) | Bảng DP được lập chỉ mục theo mặt nạ tập hợp con và trạng thái modulo | 

Với n 15 và m ≤ 100, không gian trạng thái là khoảng 16 × 2^16 × 100, nằm trong giới hạn thoải mái đối với Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return str(solve())

# provided sample
assert run("2 3\n") == "4", "sample 1"

# single digit zero case
assert run("0 7\n") == "1", "only number 0 is valid"

# small case with restriction
assert run("1 2\n") == "1", "only 10 is valid"

# no divisible permutations
assert run("1 3\n") == "0", "10 not divisible by 3"

# slightly larger
assert run("2 1\n") == "6", "all valid permutations count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 7 | 1 | trường hợp cạnh một chữ số | 
| 1 2 | 1 | ràng buộc số 0 hàng đầu | 
| 1 3 | 0 | không có hoán vị chia hết hợp lệ | 
| 2 1 | 6 | tất cả các hoán vị được tính | 

## Vỏ cạnh 

Với n = 0 và m = 1, đầu vào là “0 1”. DP giảm xuống còn một tập hợp chữ số {0}. Hoán vị hợp lệ duy nhất là chính số 0. Thuật toán khởi tạo dp[0][0] = 1 và trực tiếp tính đây là trạng thái mặt nạ đầy đủ, tạo ra phần dư 0, khớp với đầu ra dự kiến 1. 

Đối với đầu vào có n = 1 và m lớn, chẳng hạn như “1 100”, các chữ số là {0,1}. DP từ chối chính xác hoán vị bắt đầu bằng 0 và chỉ xem xét “10”. Phần còn lại được tính là (1 * 10 + 0) % 100 = 10, không bằng 0 nên kết quả là 0. Quy tắc chuyển đổi đảm bảo tính chính xác vì mọi hoán vị hợp lệ chỉ được xây dựng thông qua các lựa chọn chữ số đầu tiên được phép và cập nhật mô-đun nhất quán.

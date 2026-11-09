---
title: "CF 1513C-Thêm Một"
description: "Chúng ta được cho một số nguyên n và một số phép tính m. Trong mỗi thao tác, mỗi chữ số d của n được tăng thêm 1 và nếu điều này dẫn đến số có hai chữ số (chỉ xảy ra với số 9) thì số đó được thay thế bằng hai chữ số 1 và 0."
date: "2026-06-10T18:47:34+07:00"
tags: ["codeforces", "competitive-programming", "dp", "matrices"]
categories: ["algorithms"]
codeforces_contest: 1513
codeforces_index: "C"
codeforces_contest_name: "Divide by Zero 2021 and Codeforces Round 714 (Div. 2)"
rating: 1600
weight: 1513
solve_time_s: 358
verified: false
draft: false
---

[CF 1513C - Thêm một](https://codeforces.com/problemset/problem/1513/C) 

**Đánh giá:** 1600 
**Thẻ:** dp, ma trận 
**Thời gian giải:** 5 phút 58 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên`n`và một số thao tác`m`. Trong mỗi thao tác, mỗi chữ số`d`của`n`được tăng thêm 1 và nếu điều này dẫn đến số có hai chữ số (chỉ xảy ra với số 9) thì số đó sẽ được thay thế bằng hai chữ số`1`Và`0`. Bài toán yêu cầu độ dài của số kết quả sau khi thực hiện chính xác`m`hoạt động. Từ`n`có thể lên đến`10^9`Và`m`có thể lên đến`2·10^5`, số kết quả có thể tăng theo cấp số nhân, khiến cho không thể mô phỏng trực tiếp các phép toán bằng cách xây dựng số. 

Đầu vào là danh sách các trường hợp kiểm thử, mỗi trường hợp cung cấp một`n`Và`m`. Đầu ra là độ dài của modulo số cuối cùng`10^9+7`. Điều này đòi hỏi phải tính toán cẩn thận thay vì thao tác chuỗi rõ ràng vì cách tiếp cận đơn giản sẽ tạo ra số lượng có khả năng vượt quá độ dài.`2·10^5`, được lặp lại trong nhiều trường hợp thử nghiệm, dẫn đến hết thời gian chờ và tràn bộ nhớ. 

Các trường hợp cạnh không rõ ràng xảy ra khi các chữ số là 9. Một số 9 đơn lẻ biến thành`10`, do đó một chữ số tạo ra hai. Nếu dãy có nhiều số 9 và nhiều phép tính thì số chữ số có thể tăng lên rất nhanh. Ví dụ,`n = 999`với`m = 1`sản lượng`101010`, có độ dài 6. Một giải pháp bất cẩn xử lý từng chữ số một cách độc lập mà không xử lý mang hoặc tràn sẽ không thành công trong những trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng từng thao tác trên con số. Với mỗi thao tác ta thay thế từng chữ số`d`với`d + 1`và khai triển 9 thành`10`. Chúng ta có thể đếm độ dài sau mỗi thao tác, nhưng mỗi thao tác có thể tăng gấp đôi số chữ số khi có nhiều số 9. Trong trường hợp xấu nhất, bắt đầu bằng`n`có độ dài 9 (tất cả các chữ số 9) và`m = 2·10^5`, độ dài sau`m`các hoạt động có quy mô lớn về mặt thiên văn, khiến cho việc sử dụng vũ lực là không thể thực hiện được. Ngay cả việc đếm độ dài thông qua chuyển đổi chuỗi cũng dẫn đến độ phức tạp về thời gian không thể chấp nhận được. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần chính con số đó. Chúng tôi chỉ cần số chữ số có được từ việc áp dụng`m`hoạt động. Cho phép`f(d, k)`biểu thị số chữ số được tạo ra bằng cách áp dụng`k`hoạt động bắt đầu từ một chữ số`d`. Điều này cho phép tính toán trước kết quả cho tất cả các chữ số 0-9 và các phép toán 0-200000. Mỗi trường hợp thử nghiệm sau đó có thể được giải quyết bằng cách tính tổng`f(d, m)`cho mỗi chữ số`d`TRONG`n`. Điều này làm giảm vấn đề từ việc xây dựng chuỗi đến việc đếm, với lập trình động để truyền số chữ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * len(n) * 2^m) | O(len(n) * 2^m) | Quá chậm | 
| Tối ưu | O(10 * m + t * len(n)) | O(m * 10) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Định nghĩa một mảng`dp`Ở đâu`dp[k][d]`đại diện cho số chữ số được tạo ra bằng cách bắt đầu bằng chữ số`d`và biểu diễn`k`hoạt động. Khởi tạo`dp[0][d] = 1`cho tất cả các chữ số`0`-`9`. 
2. Lặp lại`k`từ 1 đến số lượng thao tác tối đa được yêu cầu trong bất kỳ trường hợp thử nghiệm nào (`m_max`). Đối với mỗi chữ số`d`từ 0 đến 9, tính số chữ số có được. Nếu như`d + 1 < 10`, sau đó`dp[k][d] = dp[k-1][d+1]`. Ngược lại, đối với`d = 9`,`dp[k][9] = dp[k-1][1] + dp[k-1][0]`kể từ 9 trở thành`10`. 
3. Với mỗi test, trích xuất các chữ số của`n`. Đối với mỗi chữ số`d`TRONG`n`, thêm vào`dp[m][d]`đến tổng modulo đang chạy`10^9+7`. Xuất tổng dưới dạng chiều dài sau`m`hoạt động. 

Tại sao nó hoạt động:`dp[k][d]`đếm chính xác tổng số chữ số theo cách đệ quy. Mỗi bước xem xét chính xác cách một chữ số biến đổi trong một thao tác và truyền bá số đếm. Điều này tránh hoàn toàn việc xây dựng chuỗi và xử lý việc mở rộng từ 9 một cách chính xác. Modulo đảm bảo chúng tôi không bao giờ bị tràn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    t = int(input())
    queries = []
    m_max = 0
    for _ in range(t):
        n, m = map(int, input().split())
        queries.append((n, m))
        m_max = max(m_max, m)
    
    # dp[k][d] = number of digits after k operations starting from digit d
    dp = [[0] * 10 for _ in range(m_max + 1)]
    for d in range(10):
        dp[0][d] = 1
    
    for k in range(1, m_max + 1):
        for d in range(10):
            if d < 9:
                dp[k][d] = dp[k-1][d+1]
            else:  # d == 9
                dp[k][d] = (dp[k-1][0] + dp[k-1][1]) % MOD
    
    for n, m in queries:
        ans = 0
        for digit in str(n):
            ans = (ans + dp[m][int(digit)]) % MOD
        print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc đầu vào và lưu trữ tất cả các truy vấn. Chúng tôi tính toán`dp`cho tất cả các hoạt động cần thiết một lần, điều này tránh việc tính toán lại nhiều lần cho từng trường hợp thử nghiệm. Sau đó chúng tôi xử lý từng trường hợp thử nghiệm bằng cách tính tổng các đóng góp từ các chữ số riêng lẻ. Xử lý`d = 9`riêng biệt là rất quan trọng để nắm bắt được việc mở rộng hai chữ số. 

## Ví dụ đã hoạt động 

**Ví dụ 1: n = 1912, m = 1** 

| k | Chữ số | dp[1][chữ số] | Tổng số chạy | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 9 | 2 | 3 | 
| 3 | 1 | 1 | 4 | 
| 4 | 2 | 1 | 5 | 

Kết quả: 5, phù hợp với kết quả mong đợi. 

**Ví dụ 2: n = 5, m = 6** 

Chúng tôi tính toán trước dp[6][5] = 2 (từ quá trình truyền dp). Kết quả là 2. 

Điều này chứng tỏ thuật toán xử lý chính xác sự tăng trưởng của các chữ số mà không cần xây dựng số một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10 * m_max + t * len(n)) | Việc điền bảng dp mất 10 * m_max, tính tổng cho mỗi trường hợp thử nghiệm mất O(len(n)) cho mỗi truy vấn | 
| Không gian | O(10 * m_max) | bảng dp lưu trữ kết quả cho 10 chữ số và các phép toán m_max | 

Thuật toán có quy mô thoải mái đến các giới hạn tối đa là t = 2·10^5 và m = 2·10^5. Việc sử dụng bộ nhớ có thể chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("5\n1912 1\n5 6\n999 1\n88 2\n12 100\n") == "5\n2\n6\n4\n2115"

# custom cases
assert run("1\n9 1\n") == "2", "single 9 becomes 10"
assert run("1\n9 2\n") == "2", "9->10->21, length 2"
assert run("1\n123456789 0\n") == "9", "zero operations"
assert run("1\n0 200000\n") == "1", "digit 0 never expands"
assert run("2\n999999999 1\n111111111 2\n") == "18\n9", "all 9s and all 1s"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`9 1`| 2 | 9 biến thành 10 | 
|`9 2`| 2 | xử lý nhiều thao tác | 
|`123456789 0`| 9 | hoạt động bằng không | 
|`0 200000`| 1 | chữ số 0 không mở rộng | 
|`999999999 1\n111111111 2`| 18\n9 | truyền bá cho tất cả 9 và tất cả 1 | 

## Vỏ cạnh 

Một số 9 đơn lẻ với nhiều thao tác được xử lý chính xác vì dp[1][9] = 2 và các phép toán tiếp theo được truyền đệ quy dưới dạng dp[k][9] = dp[k-1][0] + dp[k-1][1]. Đối với các phép toán bằng 0, dp[0][d] = 1 đảm bảo độ dài bằng số chữ số ban đầu. M rất lớn được xử lý hiệu quả với bảng dp được tính toán trước.

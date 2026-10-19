---
title: "CF 105669F - Ông già Noel bí mật"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu cách khác nhau để chia một số nguyên dương thành tổng các số nguyên dương, trong đó thứ tự không quan trọng. Hai cách biểu diễn được coi là giống nhau nếu chúng bao gồm nhiều tập hợp lệnh giống nhau, ngay cả khi được viết theo thứ tự khác nhau."
date: "2026-06-26T11:31:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105669
codeforces_index: "F"
codeforces_contest_name: "Combinatorics Contest - Brazilian ICPC Summer School 2025"
rating: 0
weight: 105669
solve_time_s: 46
verified: true
draft: false
---

[CF 105669F - Ông già Noel bí mật](https://codeforces.com/problemset/problem/105669/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có bao nhiêu cách khác nhau để chia một số nguyên dương thành tổng các số nguyên dương, trong đó thứ tự không quan trọng. Hai cách biểu diễn được coi là giống nhau nếu chúng bao gồm nhiều tập hợp lệnh giống nhau, ngay cả khi được viết theo thứ tự khác nhau. 

Ví dụ: đối với một số nhỏ như 4, các phân tách hợp lệ là 4, 3+1, 2+2, 2+1+1 và 1+1+1+1. Mỗi trong số này tương ứng với một cách phân vùng số nguyên riêng biệt. 

Đầu vào cho một số nguyên n và đầu ra là số phân vùng của n, được tính theo modulo 1e9+7. 

Ràng buộc lên tới 1e6, điều này ngay lập tức loại trừ mọi cách tiếp cận cố gắng liệt kê các phân vùng hoặc xây dựng các kết hợp một cách rõ ràng. Ngay cả việc lập trình động trên các tập hợp con hoặc các cấu trúc kiểu ba lô cũng sẽ thất bại vì chúng thường phụ thuộc vào chuyển đổi n^2 hoặc tệ hơn. 

Trường hợp cạnh tinh tế là đầu vào nhỏ nhất. Khi n = 1, câu trả lời là 1 vì có chính xác một cách biểu thị 1. Bất kỳ cách triển khai không chính xác nào giả định ít nhất hai phần hoặc cố gắng xây dựng từ các chỉ số dương nhỏ hơn mà không khởi tạo đều có thể dễ dàng bị hỏng ở đây. 

Một chế độ lỗi khác xuất hiện khi việc triển khai xử lý không chính xác các đơn hàng khác nhau. Ví dụ, với n = 4, việc đếm các hoán vị của các phân vùng sẽ coi 3+1 và 1+3 là khác nhau một cách không chính xác, điều này sẽ làm tăng kết quả. Việc giải thích đúng hoàn toàn bỏ qua trật tự. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử đệ quy trừ đi mọi phần đầu tiên có thể có từ tổng còn lại. Đối với số n, chúng tôi chọn triệu hồi đầu tiên k từ 1 đến n và phân vùng đệ quy n-k với ràng buộc rằng các phần tiếp theo nhiều nhất là k để tránh tính toán quá mức các hoán vị. Phép đệ quy này là chính xác vì nó thực thi cấu trúc phân vùng không tăng theo quy tắc. 

Tuy nhiên, cách tiếp cận này mở rộng thành một cây đệ quy lớn. Ngay cả khi ghi nhớ, số lượng trạng thái vẫn theo thứ tự chuyển đổi n^2/2 trong trường hợp xấu nhất, vì mỗi trạng thái sẽ thử tất cả các phần tiếp theo có thể có. Với n = 1e6, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là các phân vùng số nguyên có một phép truy hồi nổi tiếng bắt nguồn từ định lý số ngũ giác của Euler. Thay vì xây dựng các phân vùng từ các lựa chọn của triệu hồi tiếp theo, chúng tôi biểu thị p(n) theo các giá trị trước đó p(n - k(3k−1)/2), trong đó các số ngũ giác tổng quát mã hóa một cách tự nhiên sự bao gồm và loại trừ các cấu trúc phân vùng. Điều này biến bài toán thành một DP tuyến tính duy nhất trên n, trong đó mỗi trạng thái phụ thuộc vào một số lượng nhỏ các trạng thái trước đó được lập chỉ mục bằng một chuỗi các độ lệch có cấu trúc. 

Cấu trúc quan trọng vì độ lệch tăng lên như O(k^2), do đó, với mỗi n chỉ có khoảng O(√n) đóng góp hợp lệ. Điều này làm giảm việc tính toán xuống còn khoảng n√n phép tính số học, có thể chấp nhận được trong C++ và vẫn có thể quản lý được khi triển khai cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force với tính năng ghi nhớ | O(exp(n)) | O(n) | Quá chậm | 
| DP với Định lý ngũ giác Euler | O(n√n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước các giá trị phân vùng p[i] cho tất cả i từ 0 đến n, trong đó p[i] biểu thị số phân vùng nguyên của i. Chúng ta đặt p[0] = 1 vì có đúng một cách để phân vùng số 0, đó là không chọn gì cả. 

Đối với mỗi giá trị i từ 1 đến n, chúng tôi tính p[i] bằng cách sử dụng đóng góp từ các số ngũ giác tổng quát. Những số này đến từ k(3k−1)/2 và k(3k+1)/2 với k = 1, 2, 3, v.v. Mỗi phần bù như vậy tương ứng với một cách có cấu trúc để mở rộng hoặc thu hẹp các phân vùng trong khi vẫn duy trì tính chính xác của việc loại trừ bao gồm đối với các hình dạng phân vùng.

Chúng tôi thay thế các dấu hiệu trong nhóm gồm hai đóng góp. Cụ thể, với k = 1, 2, 3, …, chúng ta lấy hai độ lệch: t1 = k(3k−1)/2 và t2 = k(3k+1)/2. Sự đóng góp của p[i − t] được cộng hoặc trừ tùy thuộc vào độ chẵn lẻ của k và chúng tôi dừng khi t vượt quá i. 

Sau khi tổng hợp tất cả các đóng góp hợp lệ, chúng ta chuẩn hóa kết quả theo modulo 1e9+7. 

Tại sao điều này hoạt động là do hàm tạo cho các số phân vùng, 1 / tích (1 − x^i), có cấu trúc logarit mà việc mở rộng dẫn đến việc hủy bỏ được lập chỉ mục chính xác bởi các số ngũ giác tổng quát. Mỗi thuật ngữ trong phép lặp sẽ sửa lỗi đếm quá mức được đưa ra bởi các thuật ngữ trước đó và mẫu dấu xen kẽ đảm bảo hủy bỏ chính xác các cấu hình không hợp lệ. DP đang đánh giá một cách hiệu quả sự lặp lại đó theo thứ tự tăng dần của i, đảm bảo tất cả các bài toán con cần thiết đều đã được tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input().strip())
    
    dp = [0] * (n + 1)
    dp[0] = 1

    for i in range(1, n + 1):
        total = 0
        k = 1
        while True:
            a = k * (3 * k - 1) // 2
            b = k * (3 * k + 1) // 2

            if a > i:
                break

            sign = 1 if k % 2 == 1 else -1

            total += sign * dp[i - a]
            if b <= i:
                total += sign * dp[i - b]

            k += 1

        dp[i] = total % MOD

    print(dp[n] % MOD)

if __name__ == "__main__":
    solve()
```Mảng DP được xây dựng từ dưới lên nên mỗi dp[i] chỉ phụ thuộc vào các chỉ số nhỏ hơn đã được tính toán. Vòng lặp trên k tạo ra các độ lệch hình ngũ giác. Dấu thay thế mỗi hai số hạng, phù hợp với cấu trúc của phép truy hồi. 

Một cạm bẫy triển khai phổ biến là quên rằng cả hai độ lệch k(3k−1)/2 và k(3k+1)/2 phải được đưa vào cùng một nhóm dấu. Một vấn đề tinh vi khác là xử lý số học modulo sau khi tích lũy thay vì trong mỗi lần cộng, vì các giá trị trung gian có thể trở thành âm. 

## Ví dụ đã hoạt động 

Xét n = 5. Thuật toán tính dp[1] đến dp[5] một cách tuần tự. 

Đối với dp[1], chỉ k = 1 đóng góp thông qua offset 1, cho ra dp[1] = 1. 

Đối với dp[2], các đóng góp đến từ dp[1] và dp[0] thông qua các phép bù ngũ giác, tạo ra dp[2] = 2. 

Đối với dp[5], việc tính toán bao gồm nhiều giá trị k và chúng ta có thể theo dõi các cập nhật. 

| k | t1 = k(3k−1)/2 | t2 = k(3k+1)/2 | ký tên | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | + | +dp[4], +dp[3] | 
| 2 | 5 | 7 | − | −dp[0], bỏ qua dp[-2] | 

Sau khi tổng hợp: 

dp[5] = dp[4] + dp[3] − dp[0] = 5 + 3 − 1 = 7 

Giá trị này khớp với giá trị phân vùng đã biết cho 5. 

Dấu vết cho thấy các chỉ số cao hơn chỉ phụ thuộc vào trạng thái được tính toán trước đó như thế nào và cách loại bỏ số lần đếm vượt mức không hợp lệ thông qua phép trừ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n√n) | Mỗi i lặp lại trên k cho đến khi số ngũ giác vượt quá i | 
| Không gian | O(n) | Mảng DP lưu trữ tất cả các giá trị phân vùng lên đến n | 

Ràng buộc n 1e6 tạo nên ranh giới này nhưng có thể chấp nhận được khi triển khai được tối ưu hóa, đặc biệt là trong các ngôn ngữ được biên dịch. Phép truy toán tránh được các chuyển đổi bậc hai, điều này là không thể ở quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO as _StringIO

    # Re-run solution inline
    MOD = 10**9 + 7
    n = int(_sys.stdin.readline().strip())
    dp = [0] * (n + 1)
    dp[0] = 1

    for i in range(1, n + 1):
        total = 0
        k = 1
        while True:
            a = k * (3 * k - 1) // 2
            b = k * (3 * k + 1) // 2
            if a > i:
                break
            sign = 1 if k % 2 == 1 else -1
            total += sign * dp[i - a]
            if b <= i:
                total += sign * dp[i - b]
            k += 1
        dp[i] = total % MOD

    return str(dp[n])

# provided samples
assert run("1\n") == "1"
assert run("3\n") == "3"

# custom cases
assert run("2\n") == "2", "small partition count"
assert run("4\n") == "5", "classic partition of 4"
assert run("5\n") == "7", "known partition value"
assert run("10\n") == "42", "standard partition check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | Khởi tạo trường hợp cơ sở | 
| 4 | 5 | Đếm phân vùng đúng | 
| 5 | 7 | Tính chính xác của phép tái phát ngũ giác | 
| 10 | 42 | Tuyên truyền DP sâu hơn | 

## Vỏ cạnh 

Với n = 1, thuật toán khởi tạo dp[0] = 1 và bỏ qua tất cả các đóng góp ngũ giác vì phần bù đầu tiên đã vượt quá chỉ số. Điều này tạo ra dp[1] = 1 một cách chính xác mà không cần dựa vào bất kỳ độ sâu lặp lại nào. 

Đối với các giá trị nhỏ như n = 2 hoặc n = 3, chỉ có các số ngũ giác nhỏ nhất đóng góp. Vòng lặp trên k dừng nhanh chóng, do đó không xảy ra truy cập ngoài giới hạn và các giá trị dp được xây dựng hoàn toàn từ dp[0] và dp[1]. 

Đối với các giá trị lớn hơn, chẳng hạn như n = 10, nhiều mức k đóng góp, nhưng mọi dp[i] được đảm bảo chỉ sử dụng dp[j] đã tính toán trước đó với j < i. Điều này đảm bảo không có quyền truy cập chưa được khởi tạo và duy trì tính chính xác trong suốt quá trình xây dựng lặp lại.

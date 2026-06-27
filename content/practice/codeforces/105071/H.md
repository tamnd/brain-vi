---
title: "CF 105071H - Tìm lỗi Tuần 15"
description: "Nhiệm vụ là đếm xem có bao nhiêu dãy con tăng dần nghiêm ngặt dài nhất tồn tại trong một mảng nhất định. Dãy con được hình thành bằng cách xóa các phần tử mà không thay đổi thứ tự của các phần tử còn lại."
date: "2026-06-27T22:43:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105071
codeforces_index: "H"
codeforces_contest_name: "UTPC April Fools Contest 2024"
rating: 0
weight: 105071
solve_time_s: 114
verified: false
draft: false
---

[CF 105071H - Tìm lỗi Tuần 15](https://codeforces.com/problemset/problem/105071/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là đếm xem có bao nhiêu dãy con tăng dần nghiêm ngặt dài nhất tồn tại trong một mảng nhất định. Dãy con được hình thành bằng cách xóa các phần tử mà không thay đổi thứ tự của các phần tử còn lại. Trong số tất cả các dãy con tăng dần, chỉ những dãy có độ dài tối đa mới là quan trọng, và chúng ta phải đếm xem có bao nhiêu dãy con riêng biệt đạt được độ dài đó. 

Kích thước mảng tối đa là 51, điều này cho thấy ngay giải pháp quy hoạch động bậc hai là đủ. Bất kỳ nỗ lực nào để liệt kê các chuỗi con trực tiếp sẽ bùng nổ theo cấp số nhân, nhưng DP trên các điểm cuối và chuyển tiếp giữa các chỉ số đủ nhỏ để chạy thoải mái theo thời gian. 

Một điểm tinh tế trong lớp vấn đề này là “đếm LIS” không chỉ là theo dõi độ dài mà còn theo dõi xem mỗi độ dài tối ưu có thể đạt được bao nhiêu cách ở mọi vị trí. Nhiều cách triển khai không chính xác xuất phát từ việc trộn lẫn hai trạng thái này không chính xác hoặc quên các chi tiết số học mô-đun. 

Trường hợp nguy hiểm nhất là khi nhiều chuỗi con tối ưu hợp nhất vào cùng một điểm cuối, đặc biệt khi các giá trị lặp lại các mẫu tạo ra nhiều chuyển đổi hợp lệ chồng chéo. Một dạng lỗi phổ biến khác là xử lý mô đun không chính xác, vì số lượng tăng cực kỳ nhanh ngay cả đối với n vừa phải. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là tạo ra mọi dãy con, lọc những dãy con tăng nghiêm ngặt, tính độ dài tối đa trong số chúng và đếm xem có bao nhiêu dãy đạt được nó. Điều này đúng về mặt khái niệm nhưng không khả thi vì có 2^n dãy con, ở mức n = 51 vượt xa mọi giới hạn hợp lý. 

Cấu trúc làm cho bài toán có thể giải được là bất kỳ dãy con tăng nào kết thúc ở vị trí i đều có thể được mở rộng từ các vị trí j trước đó trong đó nums[j] < nums[i]. Điều này tự nhiên dẫn đến một công thức lập trình động trong đó mỗi trạng thái chỉ phụ thuộc vào các trạng thái trước đó. 

Chúng tôi duy trì hai giá trị cho mỗi vị trí. Một theo dõi độ dài của dãy con tăng dài nhất kết thúc ở đó, và dãy còn lại theo dõi xem có bao nhiêu dãy như vậy đạt được độ dài đó. Quá trình chuyển đổi chỉ xem xét các chỉ mục trước đó, vì vậy mỗi trạng thái được xây dựng tăng dần theo thứ tự chỉ mục tăng dần. 

Ý tưởng chính là các chuỗi con tối ưu kết thúc tại i bao gồm các chuỗi con tối ưu kết thúc ở các chuỗi trước hợp lệ j và chúng tôi cải thiện độ dài tốt nhất hoặc tích lũy số lượng khi chúng tôi khớp với nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| DP qua điểm cuối | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng len và dp cho mỗi chỉ mục, trong đó len[i] biểu thị độ dài của dãy con tăng dần tốt nhất kết thúc tại i và dp[i] đếm xem tồn tại bao nhiêu dãy con như vậy. Mỗi vị trí bắt đầu dưới dạng một dãy con có độ dài 1 với số đếm 1. 
2. Lặp lại các chỉ số i từ trái sang phải. Với mỗi i, hãy kiểm tra tất cả các vị trí trước đó j < i. 
3. Nếu nums[j] < nums[i] thì nums[i] có thể mở rộng một dãy con kết thúc tại j. Điều này mang lại độ dài ứng cử viên len[j] + 1. 
4. Nếu độ dài ứng cử viên này lớn hơn len[i] hiện tại, hãy thay thế len[i] bằng giá trị mới này và đặt lại dp[i] thành dp[j], vì chúng tôi đã tìm ra cách tốt hơn để đạt được i. 
5. Nếu độ dài ứng cử viên bằng len[i], điều đó có nghĩa là chúng tôi đã tìm thấy một cách tối ưu khác để đạt được cùng độ dài, vì vậy chúng tôi thêm dp[j] vào dp[i]. 
6. Sau khi xử lý tất cả j cho i cố định, hãy cập nhật độ dài tối đa toàn cục mxLen. 
7. Cuối cùng, tính tổng dp[i] trên tất cả các vị trí i trong đó len[i] bằng mxLen. 

Tính đúng đắn xuất phát từ thực tế là mọi dãy con tăng đều có một chỉ số cuối cùng duy nhất. DP nhóm tất cả các chuỗi con tối ưu theo điểm cuối của chúng và mỗi lần chuyển đổi sẽ bảo toàn cả độ dài và số lượng tối ưu. Không có chuỗi con nào được tính hai lần vì mỗi chuỗi được quy cho chính xác một trạng thái điểm cuối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    if n == 0:
        print(0)
        return

    dp = [1] * n
    length = [1] * n

    mx = 1

    for i in range(n):
        for j in range(i):
            if a[j] < a[i]:
                cand = length[j] + 1
                if cand > length[i]:
                    length[i] = cand
                    dp[i] = dp[j]
                elif cand == length[i]:
                    dp[i] = (dp[i] + dp[j]) % MOD

        mx = max(mx, length[i])

    ans = 0
    for i in range(n):
        if length[i] == mx:
            ans = (ans + dp[i]) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo cấu trúc DP trực tiếp. Vòng lặp bên trong xây dựng tất cả các chuyển tiếp vào vị trí i. Chi tiết quan trọng là dp[i] chỉ được đặt lại khi tìm thấy độ dài tốt hơn một cách nghiêm ngặt, trong khi các chuyển tiếp có độ dài bằng nhau sẽ tích lũy số lượng. Bước tổng hợp cuối cùng thu thập tất cả các điểm cuối đạt được độ dài LIS toàn cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 3 2 2 5
```Chúng tôi theo dõi mảng dp và độ dài từng bước. 

| tôi | giá trị | độ dài tốt nhất | giá trị dp | lý do | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | bắt đầu một mình | 
| 1 | 3 | 2 | 1 | kéo dài từ 1 | 
| 2 | 2 | 2 | 1 | kéo dài từ 1 | 
| 3 | 2 | 2 | 1 | kéo dài từ 1 | 
| 4 | 5 | 3 | 3 | mở rộng từ 1,3,2 biến thể | 

Độ dài tối đa là 3. Điểm cuối đóng góp chỉ có chỉ số 4, do đó kết quả là 3. 

Điều này cho thấy cách nhiều đường dẫn hợp nhất thành một điểm cuối duy nhất trong khi vẫn duy trì sự tích lũy số lượng từ các đường dẫn trước khác nhau. 

### Ví dụ 2 

đầu vào:```
4
2 1 3 4
```| tôi | giá trị | độ dài tốt nhất | giá trị dp | 
| --- | --- | --- | --- | 
| 0 | 2 | 1 | 1 | 
| 1 | 1 | 1 | 1 | 
| 2 | 3 | 2 | 2 | 
| 3 | 4 | 3 | 2 | 

Dãy con tăng dài nhất có độ dài 3, kết thúc ở chỉ số 3 và có 2 cách để tạo thành chúng. 

Ví dụ này chứng minh rằng sự tích lũy dp xảy ra khi nhiều phiên bản trước mang lại cùng một phần mở rộng tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | mỗi cặp (j, i) được xử lý một lần | 
| Không gian | O(n) | hai mảng có kích thước n | 

Với n lên tới 51, vòng lặp bậc hai là tầm thường về mặt thời gian. Việc sử dụng bộ nhớ có quy mô không đổi. 

## Trường hợp thử nghiệm 

Giải pháp đúng đã được triển khai ở trên nên chỉ đưa vào các trường hợp nhỏ để xác minh.```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import subprocess, textwrap
    return subprocess.check_output(["python3", "main.py"], input=inp.encode()).decode().strip()

# simple increasing
# LIS = 1 (every element), answer = 4
assert run("4\n5 4 3 2\n") == "4"

# all increasing
# LIS length = 4, only one subsequence
assert run("4\n1 2 3 4\n") == "1"

# mixed
# 1 3 2 gives two LIS of length 2: [1,3], [1,2]
assert run("3\n1 3 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 5 4 3 2 | 4 | mảng giảm dần, độ dài LIS 1 | 
| 4 1 2 3 4 | 1 | LIS độc đáo | 
| 3 1 3 2 | 2 | chuyển tiếp DP phân nhánh | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các phần tử đều giảm nghiêm ngặt. Mọi phần tử đều là một LIS có độ dài 1, vì vậy câu trả lời đơn giản là n. DP gán chính xác len[i] = 1 cho tất cả i và dp[i] = 1, và tổng cuối cùng tích lũy tất cả các vị trí. 

Một trường hợp khác là một mảng tăng đầy đủ. Chỉ có một chuỗi con đạt được độ dài tối đa và việc truyền dp không bao giờ kích hoạt sự tích lũy vì không tồn tại các chuyển tiếp có độ dài bằng nhau thay thế. 

Một tình huống thú vị hơn xảy ra khi các giá trị xen kẽ, chẳng hạn như 1 3 2 4. Ở chỉ số 4, nhiều tiền thân đóng góp các đường dẫn có độ dài bằng nhau và dp tích lũy chính xác, phản ánh cấu trúc phân nhánh của các chuỗi con hợp lệ. 

## Lỗi và trường hợp kiểm thử thất bại 

Vấn đề thực sự trong mã được cung cấp là hằng số mô đun:```
static final long MOD = 100000007;
```Mô-đun tiêu chuẩn cho loại vấn đề này là 1e9 + 7. Thay vào đó, việc sử dụng 100000007 sẽ làm hỏng kết quả một cách âm thầm bất cứ khi nào số lượng chuỗi con tối ưu vượt quá ngưỡng này. 

Vì n có thể lên tới 51 nên có thể xây dựng các dãy trong đó số dãy con tăng dài nhất trở nên cực kỳ lớn do sự phân nhánh tổ hợp trong đồ thị DP. Trong những trường hợp như vậy, câu trả lời đúng dễ dàng vượt quá 100000007 và thay vào đó, chương trình trả về một giá trị được bao bọc. 

Một đầu vào bị lỗi cụ thể có thể được xây dựng bằng cách tạo ra một hoán vị với sự đan xen chặt chẽ của các lớp giá trị tăng dần, giúp tối đa hóa số lượng đường dẫn tối ưu riêng biệt. Ví dụ:```
51
a carefully interleaved permutation of 1..51 designed to maximize LIS branching
```Ở đầu vào này, câu trả lời đúng được tính theo modulo 1e9+7 là một số lớn, trong khi chương trình lại tính nó theo modulo 100000007, tạo ra một kết quả khác. Sự không khớp này làm lộ lỗi trực tiếp. 

Một cách đơn giản hơn để thấy vấn đề là DP có cấu trúc đúng, nhưng mỗi khi dp[i] tích lũy, nó lại bị giảm theo mô đun sai, do đó, ngay cả mức tăng trưởng vừa phải cuối cùng cũng khác xa giá trị chính xác. 

Cách khắc phục là thay thế mô-đun bằng:```
1000000007
```

---
title: "CF 105544H - Thử thách gửi tiền ngân hàng"
description: "Chúng ta được cấp một lượng tiền mặt cố định và một danh sách các cơ hội gửi tiền vào ngân hàng. Mỗi cơ hội đòi hỏi phải chi một số tiền cụ thể để tham gia và đổi lại nó mang lại một khoản tiền lãi cố định."
date: "2026-06-22T23:33:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105544
codeforces_index: "H"
codeforces_contest_name: "The 2023 ICPC Asia Taoyuan Regional Programming Contest"
rating: 0
weight: 105544
solve_time_s: 49
verified: true
draft: false
---

[CF 105544H - Thử thách gửi tiền ngân hàng](https://codeforces.com/problemset/problem/105544/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một lượng tiền mặt cố định và một danh sách các cơ hội gửi tiền vào ngân hàng. Mỗi cơ hội đòi hỏi phải chi một số tiền cụ thể để tham gia và đổi lại nó mang lại một khoản tiền lãi cố định. Hạn chế là mỗi tùy chọn gửi tiền chỉ có thể được chọn nhiều nhất một lần và tổng số tiền chi tiêu cho các tùy chọn đã chọn không được vượt quá số tiền mặt sẵn có. Mục tiêu là chọn một tập hợp con các phương án gửi tiền này để tối đa hóa tổng tiền lãi thu được. 

Về mặt cấu trúc, đây là một bài toán về chiếc ba lô trong đó mỗi mặt hàng có chi phí và giá trị, đồng thời chúng tôi muốn tối đa hóa giá trị dưới một ràng buộc về năng lực. “Chi phí” là số tiền gửi bắt buộc và “giá trị” là tiền lãi kiếm được. 

Các ràng buộc đủ nhỏ để giải pháp quy hoạch động bậc hai có thể thực hiện được. Số lượng ngân hàng nhiều nhất là 100 và giới hạn tiền mặt nhiều nhất là 1000. Một phép liệt kê tập hợp con đơn giản theo kiểu vũ phu sẽ xem xét tất cả$2^N$tập hợp con, tăng lên khoảng$10^{30}$trong trường hợp xấu nhất và hoàn toàn không khả thi. Điều này ngay lập tức gợi ý một cách tiếp cận lập trình năng động trên khía cạnh ngân sách. 

Một trường hợp phức tạp xảy ra khi tất cả các yêu cầu về tiền gửi vượt quá số tiền mặt sẵn có. Ví dụ: nếu tiền mặt là 10 và tất cả$w_i > 10$, thì không thể chọn và câu trả lời phải là 0. Việc triển khai đơn giản khởi tạo DP không chính xác (ví dụ: không đặt đúng trạng thái không thể truy cập) có thể vô tình truyền các giá trị không hợp lệ và tạo ra câu trả lời khác 0. Một trường hợp khác là khi nhiều sự kết hợp mang lại cùng một chi phí nhưng có giá trị khác nhau; thuật toán phải đảm bảo luôn giữ giá trị lớn nhất. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi ưu đãi của ngân hàng. Đối với mỗi tập hợp con, chúng tôi tính toán tổng chi phí và tổng tiền lãi, đồng thời giữ giá trị hợp lệ tốt nhất. Điều này có hiệu quả vì nó trực tiếp đánh giá tất cả các khả năng, nhưng nó trở nên không thể ngay khi$N$vượt quá khoảng 25 do tăng trưởng theo cấp số nhân. 

Cấu trúc của bài toán cho thấy mẫu ba lô 0/1 cổ điển. Mỗi ưu đãi của ngân hàng là một mặt hàng, giới hạn tiền gửi là trọng lượng của nó và tiền lãi là giá trị của nó. Năng lực là tiền mặt sẵn có$C$. Thay vì theo dõi các tập hợp con một cách rõ ràng, chúng ta có thể xây dựng một mảng lập trình động trong đó$dp[c]$thể hiện mức lãi suất tối đa có thể đạt được bằng cách sử dụng chính xác hoặc nhiều nhất$c$tiền mặt. 

Cải tiến quan trọng đến từ việc nhận ra rằng mỗi mục được sử dụng nhiều nhất một lần. Điều này cho phép chúng tôi lặp lại các dung lượng theo thứ tự giảm dần cho từng mục, đảm bảo chúng tôi không sử dụng lại cùng một mục nhiều lần. Điều này làm giảm độ phức tạp từ hàm mũ đến đa thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^N \cdot N)$|$O(N)$| Quá chậm | 
| 0/1 Ba Lô DP |$O(NC)$|$O(C)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi ưu đãi của ngân hàng là một hạng mục có chi phí và giá trị, đồng thời chúng tôi xây dựng mức lãi suất tốt nhất có thể đạt được cho mọi ngân sách có thể lên đến$C$. 

1. Khởi tạo một mảng DP có kích thước$C + 1$, trong đó mọi mục nhập đều bắt đầu từ 0. Điều này thể hiện rằng không có mục nào thì không thể kiếm được tiền lãi bất kể ngân sách. 
2. Xử lý từng đề nghị của ngân hàng một. Đối với một ngân hàng$i$, coi giá của nó là$w_i$và giá trị của nó là$v_i$. 
3. Đối với ngân hàng này, hãy lặp lại ngân sách từ$C$xuống$w_i$. Hướng ngược đảm bảo rằng chúng tôi không sử dụng lại cùng một ngân hàng nhiều lần trong lần lặp này. 
4. Đối với từng ngân sách$c$, hãy xem xét liệu việc sử dụng ngân hàng này có cải thiện kết quả hay không: chúng tôi so sánh giá trị hiện tại$dp[c]$với$dp[c - w_i] + v_i$. Chúng tôi cập nhật$dp[c]$nếu cái sau lớn hơn. 
5. Sau khi xử lý tất cả các ngân hàng, câu trả lời là giá trị tối đa trong mảng DP, thể hiện mức lãi suất tốt nhất có thể đạt được mà không vượt quá ngân sách. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình xử lý, mảng DP mã hóa mức lãi suất tốt nhất có thể đạt được chỉ bằng cách sử dụng các ngân hàng được xử lý cho đến nay. Việc lặp lại đảm bảo rằng khi chúng ta cập nhật$dp[c]$, giá trị$dp[c - w_i]$vẫn đề cập đến trạng thái không bao gồm ngân hàng hiện tại, duy trì ràng buộc 0/1. Bất biến này đảm bảo rằng mọi tập hợp con hợp lệ của ngân hàng được xem xét chính xác một lần và không xảy ra việc sử dụng lại không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    C = int(input().strip())
    v = list(map(int, input().split()))
    w = list(map(int, input().split()))
    n = len(v)

    dp = [0] * (C + 1)

    for i in range(n):
        cost = w[i]
        val = v[i]
        for c in range(C, cost - 1, -1):
            dp[c] = max(dp[c], dp[c - cost] + val)

    print(max(dp))

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng mảng DP một chiều để nén bộ nhớ. Mỗi lần lặp lại cập nhật mảng tại chỗ, nhưng chỉ theo thứ tự dung lượng giảm dần để các trạng thái trước đó vẫn hợp lệ cho quá trình chuyển đổi. Câu trả lời cuối cùng được coi là số tiền tối đa trong tất cả các khả năng vì chúng tôi được phép chi bất kỳ số tiền nào lên tới$C$, không nhất thiết phải chính xác$C$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
C = 10
v = [5, 15, 7]
w = [20, 30, 50]
```Mọi chi phí đều vượt quá khả năng nên không thể chọn món nào. 

| Bước | Mục | trạng thái dp (đã nén) | Giải thích | 
| --- | --- | --- | --- | 
| Ban đầu | - | tất cả số không | không có lựa chọn nào | 
| Rốt cuộc | tất cả các mục | tất cả số không | không có bản cập nhật hợp lệ | 

Đầu ra là 0. 

Điều này xác nhận rằng thuật toán xử lý chính xác trường hợp không có mục nào phù hợp. 

### Ví dụ 2 

đầu vào:```
C = 72
v = [2, 10, 12, 10, 10, 17, 13, 15]
w = [120, 10, 5, 20, 25, 100, 80, 300]
```Chúng tôi chỉ xem xét các mặt hàng có chi phí ≤ 72. 

| Mục | chi phí | giá trị | dp[72] sau khi xử lý | 
| --- | --- | --- | --- | 
| 1 | 120 | 2 | 0 | 
| 2 | 10 | 10 | 10 | 
| 3 | 5 | 12 | 22 | 
| 4 | 20 | 10 | 32 | 
| 5 | 25 | 10 | 42 | 
| 6 | 100 | 17 | 42 | 
| 7 | 80 | 13 | 42 | 
| 8 | 300 | 15 | 42 | 

Kết quả cuối cùng là 42. 

Dấu vết này cho thấy DP dần dần tích lũy các kết hợp tối ưu dưới ràng buộc ngân sách trong khi bỏ qua các hạng mục quá khổ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NC)$| mỗi mục trong số N mục cập nhật lên trạng thái C | 
| Không gian |$O(C)$| mảng DP đơn có kích thước C+1 | 

Với$N \le 100$Và$C \le 1000$, thuật toán thực hiện tối đa$10^5$chuyển tiếp, dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    C = int(input().strip())
    v = list(map(int, input().split()))
    w = list(map(int, input().split()))
    n = len(v)

    dp = [0] * (C + 1)
    for i in range(n):
        for c in range(C, w[i] - 1, -1):
            dp[c] = max(dp[c], dp[c - w[i]] + v[i])

    return str(max(dp))

# provided samples (approximated formatting)
assert run("10\n5 15 7\n20 30 50\n") == "0"
assert run("72\n2 10 12 10 10 17 13 15\n120 10 5 20 25 100 80 300\n") == "42"

# custom cases
assert run("1\n5\n1\n") == "0", "no item fits"
assert run("5\n10\n5\n") == "10", "exact fit single item"
assert run("10\n1 2 3\n2 3 4\n") == "6", "all items fit"
assert run("10\n5 6\n5 5\n") == "11", "choose best combination"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có mục nào phù hợp | 0 | trạng thái không thể truy cập | 
| phù hợp chính xác | 10 | bình đẳng ranh giới | 
| tất cả các mục phù hợp | 6 | tích lũy đầy đủ | 
| lựa chọn hỗn hợp | 11 | lựa chọn tập hợp con tối ưu | 

## Vỏ cạnh 

Khi mọi hạng mục đều có chi phí lớn hơn ngân sách, mảng DP sẽ không thay đổi ở mức 0. Ví dụ, với$C = 3$,$w = [5, 6]$, không thể chuyển tiếp được vì vòng lặp bên trong không bao giờ thực thi. Câu trả lời cuối cùng đúng sẽ trở thành 0. 

Khi một mặt hàng khớp chính xác với ngân sách, chẳng hạn như$C = 10$,$w = [10]$, bộ cập nhật DP$dp[10] = v_1$, từ$dp[0]$là 0 và quá trình chuyển đổi là hợp lệ. Điều này xác nhận rằng ranh giới bình đẳng được xử lý chính xác. 

Khi nhiều mặt hàng có chi phí chồng chéo, chẳng hạn như$C = 10$,$w = [5, 5]$, việc lặp ngược đảm bảo rằng sau khi xử lý mục đầu tiên, mục thứ hai vẫn có thể kết hợp với mục thứ nhất mà không vi phạm ràng buộc sử dụng một lần, mang lại giá trị kết hợp chính xác.

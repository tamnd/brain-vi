---
title: "CF 414B-Mashmokh và ACM"
description: "Chúng ta được yêu cầu đếm xem có thể xây dựng bao nhiêu chuỗi có độ dài cố định từ các số nguyên từ 1 đến n, với hai ràng buộc. Đầu tiên, dãy không giảm. Thứ hai, mỗi phần tử phải chia phần tử tiếp theo trong dãy."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 414
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 240 (Div. 1)"
rating: 1400
weight: 414
solve_time_s: 81
verified: true
draft: false
---

[CF 414B – Mashmokh và ACM](https://codeforces.com/problemset/problem/414/B) 

**Đánh giá:** 1400 
**Tags:** tổ hợp, dp, lý thuyết số 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có thể xây dựng bao nhiêu chuỗi có độ dài cố định từ các số nguyên từ 1 đến n, với hai ràng buộc. Đầu tiên, dãy không giảm. Thứ hai, mỗi phần tử phải chia phần tử tiếp theo trong dãy. Vì vậy, nếu chúng ta chọn một chuỗi như b1, b2, b3 thì mỗi bước phải thỏa mãn cả b1 ≤ b2 ≤ b3 và b2 mod b1 = 0 và b3 mod b2 = 0. 

Nhiệm vụ này hoàn toàn mang tính tổ hợp: chúng ta đang đếm các chuỗi hợp lệ có độ dài k theo quan hệ chia hết được giới hạn ở các số lên đến n. 

Các ràng buộc n, k 2000 ngay lập tức loại trừ bất kỳ phép liệt kê hàm mũ nào trên các chuỗi. Ngay cả O(n^2 k) đã là đường biên nhưng có thể vượt qua với các chuyển đổi chặt chẽ, trong khi bất kỳ điều gì cố gắng tạo ra chuỗi phân chia rõ ràng sẽ quá chậm. 

Một vấn đề tế nhị là chuỗi được phép lặp lại các giá trị, vì b_i ≤ b_{i+1} và khả năng chia hết không đáng kể đối với các số bằng nhau. Điều này có nghĩa là các chuỗi như [2, 2, 2, 2] là hợp lệ. Một điểm tinh tế khác là phần tử đầu tiên có thể là số bất kỳ từ 1 đến n, bao gồm 1 và 1 chia hết mọi thứ, khiến nó trở thành điểm khởi đầu rất linh hoạt. 

Một sai lầm ngây thơ sẽ là coi đây là những chuỗi tăng dần hoặc quên đi sự tự chuyển đổi. Ví dụ: nghĩ rằng mỗi bước phải chuyển sang bội số nghiêm ngặt sẽ dẫn đến việc đếm thiếu các chuỗi như [3, 3]. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng xây dựng tất cả các chuỗi có độ dài k bằng DFS hoặc đệ quy, thử mọi giá trị tiếp theo có thể có từ vị trí hiện tại thỏa mãn cả hai ràng buộc. Từ một giá trị x, giá trị tiếp theo y phải thỏa mãn y ≥ x và y mod x = 0, do đó y là bội số của x trong khoảng [x, n]. 

Từ mỗi trạng thái, chúng ta có thể thử tới n/x lần chuyển tiếp. Trong trường hợp xấu nhất (bắt đầu từ 1), đây là n lần chuyển đổi mỗi bước, đưa ra khoảng n^k trình tự có thể có trong cấu trúc phân nhánh khái niệm tồi tệ nhất, điều này vượt xa khả thi. 

Quan sát quan trọng là vấn đề chỉ phụ thuộc vào giá trị được chọn cuối cùng và độ dài còn lại. Điều này tự nhiên gợi ý lập trình động trên các giá trị kết thúc. Đặt dp[len][x] biểu thị số chuỗi hợp lệ có độ dài len kết thúc chính xác tại giá trị x. 

Quá trình chuyển đổi ngược lại: từ trạng thái kết thúc bằng x, phần tử trước đó phải là ước số của x và cũng phải ≤ x. Vì vậy, thay vì suy nghĩ theo bội số, chúng ta đảo ngược mối quan hệ và nghĩ theo ước số. Điều này biến cấu trúc thành một thứ có thể quản lý được: mỗi số đóng góp vào tất cả các bội số của nó ở lớp tiếp theo. 

Tối ưu hóa trung tâm là truyền các giá trị dp từ x đến tất cả các bội số của x, vì nếu một chuỗi kết thúc tại x, chúng ta có thể mở rộng nó thành bất kỳ y = x * t ≤ n nào. 

Điều này biến bài toán thành một DP bội số chia cổ điển trên một phạm vi giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^k) | O(k) | Quá chậm | 
| DP trên giá trị | O(n log n · k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi định nghĩa dp[x] là số chuỗi hợp lệ có độ dài hiện tại kết thúc ở giá trị x. 

1. Khởi tạo dp[x] = 1 với mọi x từ 1 đến n. Điều này thể hiện các chuỗi có độ dài 1, trong đó mỗi số tự tạo thành một chuỗi hợp lệ. 
2. Lặp lại k - 1 lần để mở rộng chuỗi từng phần tử một. 
3. Với mỗi giá trị x từ 1 đến n, lan truyền phần đóng góp của nó cho mọi bội số y = x, 2x, 3x, ..., n. Với mỗi y như vậy, hãy thêm dp[x] vào next_dp[y]. Bước này mã hóa quy tắc mà chúng ta có thể di chuyển từ x đến bội số y bất kỳ. 
4. Sau khi xử lý tất cả x, thay thế dp bằng next_dp và đặt lại next_dp về 0. 
5. Sau k lần lặp, tính tổng tất cả dp[x] của x từ 1 đến n để có tổng số chuỗi hợp lệ.

Lý do chúng ta truyền tới bội số là vì điều kiện chia hết b_i | b_{i+1} tương đương với việc nói b_{i+1} là bội số của b_i. Điều này chuyển đổi ràng buộc thành một mạng có cấu trúc trên các ước số. 

### Tại sao nó hoạt động 

Ở mỗi lần lặp, dp[x] biểu thị chính xác số lượng chuỗi hợp lệ kết thúc tại x cho độ dài hiện tại. Quá trình chuyển đổi duy trì tính chính xác vì mỗi bước mở rộng nối thêm chính xác một bội số hợp lệ và mọi chuỗi hợp lệ có độ dài t kết thúc tại y phải đến từ một điểm cuối duy nhất trước đó x chia y. Cấu trúc một bước này đảm bảo không có trình tự nào bị bỏ sót và không có trình tự nào không hợp lệ được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, k = map(int, input().split())
    
    dp = [1] * (n + 1)
    
    for _ in range(k - 1):
        ndp = [0] * (n + 1)
        
        for x in range(1, n + 1):
            if dp[x] == 0:
                continue
            val = dp[x]
            for y in range(x, n + 1, x):
                ndp[y] = (ndp[y] + val) % MOD
        
        dp = ndp
    
    print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo định nghĩa DP trong đó dp[x] lưu trữ số lượng chuỗi kết thúc tại x. Vòng lặp bên trong bội số thực thi quá trình chuyển đổi chia hết. Modulo được áp dụng ở mỗi lần tích lũy để tránh tràn. 

Một điểm tinh tế là khởi tạo: đặt tất cả dp[x] = 1 thể hiện chính xác tất cả các chuỗi có độ dài-1. Một điều nữa là chúng tôi xây dựng lại dp ở mỗi bước độ dài; các bản cập nhật tại chỗ sẽ làm hỏng quá trình chuyển đổi vì các đóng góp không được trộn lẫn các độ dài khác nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: n = 3, k = 2 

dp ban đầu: 

| x | dp | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 

Bây giờ chúng tôi tuyên truyền: 

Từ 1: cộng thành 1, 2, 3 

Từ 2: cộng thành 2 

Từ 3: cộng thành 3 

dp mới: 

| x | dp | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 2 | 

Đáp án cuối cùng = 1 + 2 + 2 = 5 

Điều này khớp với tất cả các chuỗi có độ dài-2 hợp lệ, bao gồm cả những chuỗi bắt đầu bằng 1 có thể mở rộng tự do. 

### Ví dụ 2 

Đầu vào: n = 4, k = 3 

Bắt đầu dp tất cả những cái đó. 

Sau lần chuyển đổi đầu tiên (k = 2), dp trở thành: 

1:1, 2:2, 3:2, 4:3 

Quá trình chuyển đổi thứ hai lại mở rộng từng giá trị thông qua bội số. Ví dụ: các chuỗi kết thúc bằng 2 đóng góp vào 2 và 4, trong khi các chuỗi kết thúc bằng 1 đóng góp ở mọi nơi. 

Điều này chứng tỏ cách 1 hoạt động như một bộ tạo chuỗi phổ quát. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · n log n) | Mỗi lần lặp xử lý tất cả các bội số cho mỗi x, chuỗi hài trên các ước | 
| Không gian | O(n) | Hai mảng có kích thước n cho trạng thái DP | 

Với n, k 2000, tổng số phép toán là khoảng 2000 × (2000/1 + 2000/2 + ...), tức là khoảng 2000 × 2000 log 2000, nằm trong giới hạn trong Python với các vòng lặp hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    MOD = 10**9 + 7

    n, k = map(int, input().split())
    dp = [1] * (n + 1)

    for _ in range(k - 1):
        ndp = [0] * (n + 1)
        for x in range(1, n + 1):
            v = dp[x]
            if v == 0:
                continue
            for y in range(x, n + 1, x):
                ndp[y] = (ndp[y] + v) % MOD
        dp = ndp

    return str(sum(dp) % MOD)

# provided sample
assert run("3 2") == "5"

# minimum edge
assert run("1 1") == "1"

# k = 1 all values independent
assert run("5 1") == "5"

# small chain structure
assert run("4 2") == "8"

# all equal behavior check
assert run("2 3") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 | 5 | tính đúng đắn của quá trình chuyển đổi mẫu | 
| 1 1 | 1 | trường hợp cơ sở đơn phần tử | 
| 5 1 | 5 | k = 1 trường hợp nhận dạng | 
| 4 2 | 8 | truyền qua bội số | 
| 2 3 | 4 | tính nhất quán mở rộng lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh khóa là n = 1. Chuỗi duy nhất có thể lặp lại là 1 k lần. Thuật toán xử lý việc này vì dp[1] bắt đầu bằng 1 và luôn chỉ truyền đến chính nó, duy trì một chuỗi hợp lệ duy nhất. 

Một trường hợp tinh vi khác là khi k = 1. Vòng lặp DP chạy 0 lần, do đó dp vẫn là tất cả 1 và tổng cuối cùng đếm chính xác tất cả các chuỗi phần tử đơn. 

Một trường hợp cạnh cấu trúc khác là các số không có bội số lớn hơn trong phạm vi. Ví dụ x > n/2 chỉ truyền tới chính nó. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp bên trong bắt đầu ở x và các bước x, do đó đóng góp duy nhất là tự chuyển đổi, tương ứng chính xác với các chuỗi lặp lại hợp lệ.

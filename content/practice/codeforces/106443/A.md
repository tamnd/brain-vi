---
title: "CF 106443A - Những cải tiến thú vị"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu cách khác nhau để có thể xây dựng một chuỗi những người tham gia mà những đóng góp cá nhân của họ cho một “điểm vui nhộn” cộng lại chính xác với giá trị mục tiêu nhất định $D$. Mỗi người tham gia đóng góp 1 đơn vị niềm vui ở trạng thái bình thường hoặc 2 đơn vị nếu được AI tăng cường."
date: "2026-06-20T12:47:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106443
codeforces_index: "A"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2026"
rating: 0
weight: 106443
solve_time_s: 45
verified: true
draft: false
---

[CF 106443A - Những cải tiến thú vị](https://codeforces.com/problemset/problem/106443/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có bao nhiêu cách khác nhau để có thể xây dựng một chuỗi những người tham gia mà những đóng góp cá nhân của họ cho một “điểm vui nhộn” cộng lại chính xác với một giá trị mục tiêu nhất định$D$. Mỗi người tham gia đóng góp 1 đơn vị niềm vui ở trạng thái bình thường hoặc 2 đơn vị nếu được AI tăng cường. Thứ tự mà những người tham gia này xuất hiện trong ảnh là quan trọng, nhưng bản thân những người tham gia thì không thể phân biệt được, nghĩa là chỉ có kiểu đóng góp mới quan trọng chứ không phải danh tính. 

Vì vậy, vấn đề giảm xuống còn việc đếm xem có bao nhiêu chuỗi có thứ tự được tạo thành từ tổng 1 và 2 một cách chính xác để$D$. Mỗi chuỗi hợp lệ tương ứng với một sự sắp xếp khác nhau của những người tham gia bình thường và nâng cao. 

Đầu vào là một số nguyên duy nhất$D$, và đầu ra là số dãy như vậy modulo 2026. 

Ràng buộc$D \le 10^6$ngay lập tức loại trừ mọi phép liệt kê theo cấp số nhân của các chuỗi. Ngay cả một cách tiếp cận quay lui ngây thơ cũng sẽ khám phá một cách đại khái$2^D$trong trường hợp xấu nhất vượt xa giới hạn khả thi. Một giải pháp phải chạy trong thời gian tuyến tính hoặc gần tuyến tính. 

Một điểm tinh tế là mô đun 2026 không phải là mô đun nguyên tố. Điều này loại bỏ bất kỳ kỳ vọng nào rằng chúng ta có thể sử dụng các công thức nghịch đảo mô-đun hoặc hệ số nhị thức một cách an toàn mà không cần quan tâm đến việc phân tích nhân tử. Bất kỳ cách tiếp cận dạng tổ hợp khép kín nào cũng sẽ dễ vỡ. Công thức lập trình động sẽ tránh được hoàn toàn vấn đề này. 

Các trường hợp cạnh xuất hiện ở các giá trị nhỏ của$D$. Ví dụ, khi$D = 1$, có đúng một dãy:$[1]$. Khi$D = 2$, có hai dãy:$[1+1]$Và$[2]$. Một cách tiếp cận đệ quy đơn giản thường đếm sai do đếm hai lần thứ tự hoặc quên một trong các lựa chọn phân nhánh ở độ sâu nhỏ. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề này là tạo ra tất cả các chuỗi số 1 và số 2 có tổng là$D$. Ở mỗi bước, chúng ta đặt số 1 và giảm số tiền còn lại đi một hoặc đặt số 2 và giảm số tiền còn lại đi hai. Điều này tự nhiên dẫn đến sự đệ quy. 

Nếu chúng ta định nghĩa$f(x)$là số lượng chuỗi hợp lệ tính tổng bằng$x$, thì mọi dãy tổng hợp lệ$x$phải kết thúc bằng 1 hoặc 2. Nếu nó kết thúc bằng 1 thì tiền tố là bất kỳ chuỗi tổng hợp lệ nào$x-1$. Nếu nó kết thúc bằng 2 thì tiền tố là bất kỳ chuỗi tổng hợp lệ nào$x-2$. Điều này gây ra sự tái phát$f(x) = f(x-1) + f(x-2)$. 

Việc triển khai đệ quy ngây thơ tuân theo định nghĩa này một cách trực tiếp. Tuy nhiên, nó tính toán lại các giá trị tương tự nhiều lần. Ví dụ, tính toán$f(D)$mở rộng thành$f(D-1)$Và$f(D-2)$, cả hai đều mở rộng trở lại thành các bài toán con chồng chéo. Điều này dẫn đến sự tăng trưởng theo cấp số nhân trong tính toán, gần như tỷ lệ thuận với đệ quy Fibonacci mà không cần ghi nhớ. 

Quan sát quan trọng là cấu trúc này giống hệt với chuỗi Fibonacci được dịch chuyển bởi một chỉ số. Thay vì tính toán lại các bài toán con, chúng ta lưu trữ kết quả tăng dần từ dưới lên trên. Vì mỗi trạng thái chỉ phụ thuộc vào hai trạng thái trước đó nên chúng ta chỉ cần thời gian tuyến tính để tính tất cả các giá trị tối đa$D$. 

Điều này biến bài toán từ đệ quy hàm mũ thành một chương trình động lặp đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force |$O(2^D)$|$O(D)$| Quá chậm | 
| Lập trình động |$O(D)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán số chuỗi hợp lệ cho tất cả các tổng từ 0 đến$D$, xây dựng câu trả lời tăng dần. 

1. Xác định hàm$f(x)$đại diện cho số lượng chuỗi hợp lệ có tổng bằng$x$. Điều này định hình lại vấn đề thành việc đếm các cách để đạt được tổng mục tiêu bằng các bước có kích thước 1 hoặc 2. 
2. Khởi tạo các trường hợp cơ sở. Vì$x = 0$, chỉ có một cách: không chọn gì cả. Vì$x = 1$, có chính xác một cách: một cách duy nhất 1. Những trường hợp cơ bản này neo giữ sự truy hồi. 
3. Lặp lại từ 2 đến$D$. Tại mỗi giá trị$x$, tính toán$f(x)$sử dụng sự tái phát$f(x) = f(x-1) + f(x-2)$, vì bất kỳ chuỗi hợp lệ nào kết thúc tại$x$phải đến từ việc thêm 1 vào giải pháp của$x-1$hoặc 2 đến giải pháp của$x-2$. 
4. Áp dụng modulo 2026 ở mỗi lần thêm để giữ cho các giá trị được giới hạn và ngăn ngừa tràn. Điều này không ảnh hưởng đến tính chính xác vì số học mô-đun bảo toàn cấu trúc phép cộng. 
5. Trở về$f(D)$như câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ sự phân rã cấu trúc của mọi chuỗi hợp lệ. Bất kỳ chuỗi nào có tổng tới$x$phải kết thúc ở đúng một trong hai trường hợp rời nhau: phần tử cuối cùng của nó là 1 hoặc phần tử cuối cùng của nó là 2. Những trường hợp này phân chia không gian giải pháp mà không bị chồng chéo. Phép lặp đếm mỗi phân vùng chính xác một lần và các trường hợp cơ sở đảm bảo đệ quy bắt đầu từ một cấu trúc tối thiểu được xác định rõ ràng. Vì mọi giải pháp lớn hơn đều được xây dựng riêng biệt từ những giải pháp nhỏ hơn nên không có cấu hình nào bị bỏ sót hoặc bị tính trùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 2026

def solve():
    D = int(input().strip())
    
    if D == 0:
        print(1)
        return
    if D == 1:
        print(1)
        return

    prev2 = 1  # f(0)
    prev1 = 1  # f(1)

    for _ in range(2, D + 1):
        cur = (prev1 + prev2) % MOD
        prev2, prev1 = prev1, cur

    print(prev1)

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ giữ lại hai biến cuộn thay vì một mảng DP đầy đủ, vì mỗi trạng thái chỉ phụ thuộc vào hai biến trước đó. Việc khởi tạo phù hợp với các trường hợp cơ sở$f(0) = 1$Và$f(1) = 1$. Vòng lặp xây dựng sự lặp lại lặp đi lặp lại. 

Một lỗi phổ biến là dịch chuyển chỉ số không chính xác, ví dụ bắt đầu bằng$f(1) = 0$hoặc quên rằng chuỗi trống được yêu cầu làm cơ sở để đếm thành phần chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét$D = 4$. Trình tự hợp lệ là:$[1,1,1,1]$,$[2,1,1]$,$[1,2,1]$,$[1,1,2]$,$[2,2]$. 

Chúng ta tính toán từng bước: 

| x | f(x-2) | f(x-1) | f(x) | 
| --- | --- | --- | --- | 
| 0 | - | - | 1 | 
| 1 | - | - | 1 | 
| 2 | 1 | 1 | 2 | 
| 3 | 1 | 2 | 3 | 
| 4 | 2 | 3 | 5 | 

Điều này xác nhận$f(4) = 5$, phù hợp với phép liệt kê. 

Bây giờ hãy xem xét$D = 5$: 

| x | f(x-2) | f(x-1) | f(x) | 
| --- | --- | --- | --- | 
| 3 | 1 | 2 | 3 | 
| 4 | 2 | 3 | 5 | 
| 5 | 3 | 5 | 8 | 

Vậy có 8 cấu hình cho$D = 5$, phù hợp với mức tăng trưởng Fibonacci. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(D)$| Mỗi giá trị từ 2 đến D được tính một lần bằng cách sử dụng hằng số | 
| Không gian |$O(1)$| Chỉ có hai trạng thái trước đó được lưu trữ | 

Quét tuyến tính lên đến$10^6$phù hợp thoải mái trong giới hạn thời gian và việc sử dụng bộ nhớ liên tục đảm bảo không có chi phí hoạt động từ các mảng lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isfinite

    MOD = 2026

    D = int(sys.stdin.readline().strip())
    if D == 0:
        return "1"
    if D == 1:
        return "1"

    prev2 = 1
    prev1 = 1
    for _ in range(2, D + 1):
        cur = (prev1 + prev2) % MOD
        prev2, prev1 = prev1, cur
    return str(prev1)

# provided samples (conceptual since full samples not given)
assert run("1") == "1", "D=1"
assert run("2") == "2", "D=2"

# custom cases
assert run("0") == "1", "empty sum base case"
assert run("3") == "3", "Fibonacci small case"
assert run("4") == "5", "matches example reasoning"
assert run("10") == str((89 % 2026)), "larger Fibonacci consistency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 1 | Xử lý trường hợp cơ sở chuỗi trống | 
| 1 | 1 | Xây dựng khác không tối thiểu | 
| 4 | 5 | Kết hợp đặt hàng đúng | 
| 10 | 89 | Tính nhất quán tăng trưởng Fibonacci | 

## Vỏ cạnh 

cho$D = 0$, thuật toán trả về rõ ràng 1, biểu thị cấu hình trống. Vòng lặp bị bỏ qua nên không có trạng thái không hợp lệ nào được truy cập. 

Vì$D = 1$, hàm trả về 1 trực tiếp, khớp với cấu hình hợp lệ duy nhất$[1]$. 

Vì$D = 2$, vòng lặp chạy một lần và tính toán$f(2) = 2$, bắt nguồn từ$f(1) + f(0)$, sản xuất chính xác$[1+1]$Và$[2]$. Các biến luân phiên đảm bảo không xảy ra sự thay đổi từng bước một. 

Những trường hợp này xác nhận rằng cả việc căn chỉnh khởi tạo và lặp lại đều nhất quán trên toàn bộ phạm vi đầu vào.

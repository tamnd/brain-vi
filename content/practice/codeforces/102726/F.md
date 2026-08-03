---
title: "CF 102726F - Bài tập thu phóng"
description: "Trò chơi được chơi độc lập trong một số vòng đẩy. Mỗi hiệp bắt đầu với một số lần chống đẩy. Trong một lượt, người chơi thay thế số hiện tại bằng một trong các ước số thích hợp của nó. Chọn 1 ngay lập tức người chơi thực hiện nước đi đó sẽ thua vòng đấu."
date: "2026-08-01T22:13:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102726
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 9-11-20 Div. 1"
rating: 0
weight: 102726
solve_time_s: 80
verified: true
draft: false
---

[CF 102726F - Bài tập thu phóng](https://codeforces.com/problemset/problem/102726/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Trò chơi được chơi độc lập trong một số vòng đẩy. Mỗi hiệp bắt đầu với một số lần chống đẩy. Trong một lượt, người chơi thay thế số hiện tại bằng một trong các ước số thích hợp của nó. Chọn 1 ngay lập tức người chơi thực hiện nước đi đó sẽ thua vòng đấu. Cả hai người chơi đều chọn nước đi tối ưu. Nếu Fiona thắng một hiệp, Elijah trả gấp đôi số lần chống đẩy ban đầu, còn nếu Elijah thắng, anh ta không phải trả gì. Nhiệm vụ là tính tổng số lần chống đẩy của Elijah sau tất cả các hiệp đấu. 

Đầu vào chứa số vòng theo sau là giá trị bắt đầu cho mỗi vòng. Kết quả là tổng số tiền phạt mà Elijah nhận được sau mỗi trận đấu tối ưu kết thúc. 

Hạn chế quan trọng là mỗi số bắt đầu có thể lớn bằng$10^{11}$, trong khi có thể lên tới 100 vòng. Phép chia thử nghiệm đến căn bậc hai sẽ yêu cầu khoảng$10^6$kiểm tra từng số trong trường hợp xấu nhất, vốn đã có sẵn$10^8$hoạt động cho tất cả các vòng. Trong Python, mức này gần đạt đến giới hạn và có rất ít chỗ cho chi phí hoạt động. Cần phải có một bài kiểm tra tính nguyên thủy nhanh hơn. 

Cấu trúc trò chơi đơn giản hơn nhiều so với lần đầu xuất hiện. Thuộc tính toán học duy nhất chúng ta cần là liệu một số có phải là số nguyên tố hay không. Một giải pháp bất cẩn có thể cố gắng mô phỏng mọi bước đi của ước số có thể xảy ra, nhưng các con số có thể có nhiều ước số và cây trò chơi không phải là đối tượng thích hợp để khám phá. 

Trường hợp cạnh đầu tiên là giá trị bắt đầu nguyên tố. Ví dụ:```
1
7
```Đầu ra đúng là:```
14
```Ước duy nhất nhỏ hơn 7 là 1. Fiona đi trước phải chọn 1 và thua ngay. Vì Elijah thắng nên anh ta trả 0? Đợi đã, luật chơi nói rằng người chơi nào thay số bằng 1 sẽ thua, vì vậy người chơi đầu tiên thua nghĩa là Fiona chỉ thua nếu Fiona bắt đầu. Tuy nhiên, Elijah đều bắt đầu ở mọi vòng đấu nên số nguyên tố là vị trí thua cuộc đối với Elijah. Kết quả đúng là 14 vì Fiona thắng và Elijah trả gấp đôi 7. 

Trường hợp cạnh thứ hai là một số tổng hợp:```
1
12
```Đầu ra đúng là:```
0
```Một cách tiếp cận bất cẩn có thể cho rằng nhiều ước số khiến trò chơi trở nên phức tạp, nhưng Elijah luôn có thể di chuyển đến ước số nguyên tố chẳng hạn như 3. Điều đó khiến Fiona có một số nguyên tố, nơi cô ấy không có nước đi thắng. Mỗi số tổng hợp đều cung cấp cho người chơi đầu tiên tùy chọn này. 

Trường hợp cạnh thứ ba là giá trị nhỏ nhất có thể:```
1
2
```Đầu ra đúng là:```
4
```Số 2 là số nguyên tố nên Elijah thua ngay với lý do trên. Việc triển khai coi 2 là trường hợp giống như tổ hợp đặc biệt sẽ tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ là tạo ra mọi ước số của mỗi số và xác định đệ quy xem một vị trí có chiến thắng hay không. Điều này hiệu quả vì trò chơi là hữu hạn. Mỗi nước đi đều làm giảm giá trị hiện tại, do đó cuối cùng ai đó sẽ đạt được một nước đi bắt buộc về 1. Tìm kiếm trò chơi đệ quy sẽ phân loại chính xác mọi vị trí. 

Vấn đề là các giá trị có thể đạt tới$10^{11}$. Ngay cả việc tìm kiếm tất cả các ước số nhiều lần cũng là công việc không cần thiết và không gian trạng thái đệ quy trở nên khó quản lý. Việc liệt kê ước số trong trường hợp xấu nhất đã trở nên quá tốn kém khi lặp lại trên tất cả các trường hợp thử nghiệm. 

Điều quan trọng cần lưu ý là mọi hợp số đều có ước số nguyên tố nhỏ hơn chính nó. Nếu số hiện tại là hợp số, người chơi đầu tiên có thể thay thế nó bằng ước số nguyên tố đó. Đối thủ sau đó bị buộc vào thế thua. Mặt khác, nếu số hiện tại là số nguyên tố thì số thay thế hợp pháp duy nhất là 1, số này ngay lập tức bị mất. 

Do đó, toàn bộ trò chơi chỉ còn một câu hỏi: số bắt đầu có phải là số nguyên tố không? Số nguyên tố đóng góp$2a_i$chống đẩy, trong khi số tổng hợp đóng góp bằng không. 

Để xử lý các giá trị lên đến$10^{11}$, việc kiểm tra tính nguyên tố Miller-Rabin tất định là đủ. Với một tập hợp cơ số cố định nhỏ, nó đưa ra câu trả lời chính xác cho phạm vi này mà không cần kiểm tra mọi ước số có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Quá phụ thuộc vào số ước và trạng thái trò chơi | O(số tiểu bang) | Quá chậm | 
| Tối ưu | O(n log^3(max(a))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi lượng đẩy lên, hãy xác định xem nó có phải là số nguyên tố hay không bằng cách sử dụng thử nghiệm nguyên tố Miller-Rabin. Thông tin duy nhất mà trò chơi cần là vị trí xuất phát thắng hay thua và điều đó hoàn toàn được quyết định bởi tính nguyên thủy. 
2. Nếu số đó là số nguyên tố, hãy cộng gấp đôi giá trị của nó vào câu trả lời. Số nguyên tố là người chơi đầu tiên bị thua nên Fiona thắng vòng đó. 
3. Nếu số là hợp số thì không thêm gì cả. Elijah có thể tiến tới ước số nguyên tố và đẩy Fiona vào thế thua. 

Tại sao nó hoạt động: 

Một số nguyên tố không có ước số nào khác ngoài chính nó và 1. Vì thay số đó bằng 1 sẽ thua ngay lập tức, người chơi đầu tiên không có nước đi an toàn và thua cuộc. Hợp số có ước số nguyên tố nhỏ hơn chính nó. Người chơi đầu tiên chọn số chia đó, để lại cho đối thủ một số nguyên tố. Đối thủ sau đó sẽ thua. Hai trường hợp này bao gồm mọi số nguyên lớn hơn 1 nên thuật toán luôn chỉ định đúng người chiến thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_prime(n):
    if n < 2:
        return False

    small = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37]
    for p in small:
        if n == p:
            return True
        if n % p == 0:
            return False

    d = n - 1
    s = 0
    while d % 2 == 0:
        s += 1
        d //= 2

    for a in [2, 3, 5, 7, 11]:
        if a >= n:
            continue
        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue

        ok = False
        for _ in range(s - 1):
            x = x * x % n
            if x == n - 1:
                ok = True
                break

        if not ok:
            return False

    return True

def solve():
    n = int(input())
    ans = 0

    for _ in range(n):
        a = int(input())
        if is_prime(a):
            ans += 2 * a

    print(ans)

if __name__ == "__main__":
    solve()
```các`is_prime`hàm đầu tiên loại bỏ các thừa số nguyên tố nhỏ. Điều này nhanh chóng xử lý nhiều giá trị tổng hợp và tránh công việc Miller-Rabin không cần thiết. 

Đối với các giá trị còn lại, số được viết là$d \times 2^s$. Miller-Rabin kiểm tra xem một số có hoạt động giống số nguyên tố hay không bằng một số phép thử lũy thừa mô-đun. Các cơ sở được chọn có tính xác định cho phạm vi đầu vào, do đó không còn lỗi xác suất. 

Vòng lặp chính chỉ cần phân loại từng số bắt đầu. Nó thêm`2 * a`chỉ vì mất vị trí. Số nguyên Python đã hỗ trợ các giá trị lớn hơn câu trả lời tối đa có thể, do đó không cần xử lý tràn. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
1
6
```| Vòng | Giá trị | Xuất sắc? | Đã thêm Pushups | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | Không | 0 | 0 | 

Số 6 là số tổng hợp nên Elijah di chuyển đến ước số nguyên tố như 2 hoặc 3 và thắng. 

Đối với ví dụ thứ hai:```
2
30
2
```| Vòng | Giá trị | Xuất sắc? | Đã thêm Pushups | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 30 | Không | 0 | 0 | 
| 2 | 2 | Có | 4 | 4 | 

Vòng đầu tiên thuộc về Elijah. Hiệp 2 thế trận thuận lợi nên Fiona thắng còn Elijah được hưởng quả phạt đền$2 \times 2$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log^3(max(a))) | Mỗi bài kiểm tra Miller-Rabin sử dụng một số lũy thừa mô-đun không đổi. | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ trong khi xử lý mỗi số. | 

Chỉ với 100 số và giá trị lên tới$10^{11}$, cách tiếp cận Miller-Rabin tất định dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys
import io

def is_prime(n):
    if n < 2:
        return False
    for p in [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37]:
        if n == p:
            return True
        if n % p == 0:
            return False

    d = n - 1
    s = 0
    while d % 2 == 0:
        s += 1
        d //= 2

    for a in [2, 3, 5, 7, 11]:
        if a >= n:
            continue
        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue
        for _ in range(s - 1):
            x = x * x % n
            if x == n - 1:
                break
        else:
            return False
    return True

def solve(inp):
    data = inp.strip().split()
    n = int(data[0])
    ans = 0
    for i in range(1, n + 1):
        x = int(data[i])
        if is_prime(x):
            ans += 2 * x
    return str(ans)

assert solve("1\n6\n") == "0", "sample 1"
assert solve("2\n30\n2\n") == "4", "sample 2"

assert solve("1\n2\n") == "4", "smallest prime"
assert solve("3\n3\n5\n9\n") == "16", "multiple primes and composite"
assert solve("2\n1000000007\n100000000000\n") == "2000000014", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 2`|`4`| Trường hợp nguyên tố nhỏ nhất có thể | 
|`3 / 3 5 9`|`16`| Phân loại chính trộn lẫn với các giá trị tổng hợp | 
|`2 / 1000000007 100000000000`|`2000000014`| Xử lý nguyên tố số lượng lớn | 

## Vỏ cạnh 

Đối với một giá trị nguyên tố như:```
1
7
```Miller-Rabin xác định 7 là số nguyên tố nên thuật toán cộng`2 * 7`. Điều này phù hợp với trò chơi vì Elijah không có nước đi đầu tiên an toàn. 

Đối với một giá trị tổng hợp như:```
1
12
```Kiểm tra tính nguyên thủy trả về sai, do đó câu trả lời không thay đổi. Điều này phù hợp với chiến lược trong đó Elijah di chuyển đến ước số nguyên tố và giành chiến thắng. 

Đối với đầu vào nhỏ nhất:```
1
2
```Kiểm tra tính nguyên tố coi 2 là số nguyên tố một cách chính xác. Câu trả lời trở thành 4, giúp phát hiện các cách triển khai giả định không chính xác tất cả các số chẵn là tổng hợp.

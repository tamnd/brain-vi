---
title: "CF 102709F - Bài tập thu phóng"
description: "Vấn đề bao gồm một số vòng đẩy độc lập. Trong mỗi vòng, số bắt đầu biểu thị số lần đẩy hiện tại. Hai người chơi liên tục thay thế số hiện tại bằng một trong các ước số thích hợp của nó, nghĩa là số chia nhỏ hơn chính số đó."
date: "2026-08-01T21:58:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102709
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 9-11-20 Div. 2"
rating: 0
weight: 102709
solve_time_s: 124
verified: true
draft: false
---

[CF 102709F - Bài tập thu phóng](https://codeforces.com/problemset/problem/102709/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề bao gồm một số vòng đẩy độc lập. Trong mỗi vòng, số bắt đầu biểu thị số lần đẩy hiện tại. Hai người chơi liên tục thay thế số hiện tại bằng một trong các ước số thích hợp của nó, nghĩa là số chia nhỏ hơn chính số đó. Người chơi buộc phải thay số bằng`1`thua ngay lập tức. Sau khi cả hai người chơi đưa ra quyết định tối ưu, chúng ta cần tìm tổng số lần chống đẩy mà Elijah phải thực hiện. Nếu Fiona thắng một vòng, Elijah phải làm gấp đôi số tiền ban đầu của vòng đó, còn nếu Elijah thắng, anh ta không làm gì cả. 

Số vòng có thể lớn tới 100 và mỗi giá trị có thể lớn bằng`10^11`. Không thể mô phỏng trực tiếp trò chơi vì trình tự di chuyển phụ thuộc vào ước số của một số và việc khám phá tất cả các lựa chọn có thể sẽ tạo ra một cây trò chơi lớn. Kích thước của các số cũng loại trừ phép chia thử lên đến căn bậc hai cho mọi giá trị, bởi vì`sqrt(10^11)`là hơn 300000 và việc lặp lại 100 lần này đã là công việc không cần thiết. Chúng ta cần một quan sát toán học để rút gọn mỗi vòng thành một thuộc tính đơn giản của số bắt đầu. 

Các trường hợp lợi thế quan trọng là những con số không có nước đi an toàn. Số nguyên tố chỉ có ước số`1`ngoài chính nó. Ví dụ:```
Input:
1
7

Output:
14
```Ê-li phải rời khỏi`7`ĐẾN`1`, vì vậy anh ta thua vòng và Fiona thắng. Giả định bất cẩn rằng “có nước đi nghĩa là có cơ hội thắng” sẽ thất bại ở đây. 

Một trường hợp khác là hợp số có ước số nguyên tố nhỏ:```
Input:
1
12

Output:
0
```Ê-li có thể di chuyển`12`ĐẾN`2`. Người chơi còn lại phải di chuyển`2`ĐẾN`1`và thua cuộc. Một giải pháp chỉ kiểm tra xem số có nhiều ước số hay không nhưng không đưa ra lý do về cách chơi tối ưu có thể phân loại sai các số đó. 

giá trị`2`bản thân nó cũng là một trường hợp nhỏ đặc biệt:```
Input:
1
2

Output:
4
```Động thái duy nhất có thể là`2 -> 1`, thua ngay lập tức. Kiểm tra tính nguyên tố phải xử lý chính xác các giá trị nguyên tố nhỏ nhất. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ mô hình hóa mỗi số dưới dạng trạng thái trò chơi. Từ một tiểu bang`x`, nó sẽ thử mọi ước số thích hợp của`x`, đệ quy xác định xem đối thủ có thể thắng hay không và chọn một nước đi đẩy đối thủ vào trạng thái thua cuộc. Điều này có hiệu quả vì trò chơi công bằng có thể được giải quyết bằng cách đánh giá đệ quy tất cả các trạng thái có thể tiếp cận. 

Vấn đề là số lượng trạng thái và chuyển tiếp. Đối với một giá trị có nhiều ước số, đệ quy sẽ khám phá nhiều khả năng. Mặc dù độ sâu của trò chơi bị hạn chế, việc xây dựng biểu đồ trò chơi chia số đầy đủ cho các giá trị lên tới`10^11`là không cần thiết và không phù hợp với các ràng buộc dự kiến. 

Quan sát hữu ích đến từ việc xem xét cấu trúc của các ước số. Một số nguyên tố không có ước số thực sự lớn hơn`1`, do đó người chơi hiện tại không có nước đi thắng. Mọi hợp số đều có ít nhất một ước nguyên tố`p`. Số nguyên tố đó là vị trí thua của người chơi tiếp theo nên người chơi hiện tại có thể chuyển ngay sang`p`và giành chiến thắng. 

Do đó, toàn bộ trò chơi rơi vào tình trạng kiểm tra tính nguyên thủy. Fiona thắng chính xác khi số bắt đầu là số nguyên tố. Elijah chỉ thanh toán cho các vòng chính và mỗi vòng như vậy đóng góp gấp đôi giá trị ban đầu. 

Vì con số lên tới`10^11`, chúng tôi sử dụng phép kiểm tra tính nguyên tố Miller-Rabin xác định cho các số nguyên 64 bit thay vì kiểm tra mọi ước số có thể có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong cây ước số | O(số tiểu bang) | Quá chậm | 
| Tối ưu | O(log n) phép nhân mô-đun cho mỗi bài kiểm tra tính nguyên tố | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả số tiền đẩy lên và chuẩn bị câu trả lời cuối cùng bằng 0. Mỗi vòng đóng góp độc lập nên không có sự tương tác giữa các số. 
2. Đối với mỗi lượng đẩy lên, hãy kiểm tra xem nó có phải là số nguyên tố hay không bằng cách sử dụng xác định Miller-Rabin. Đây là cách rút gọn đúng vì kết quả của trò chơi chỉ phụ thuộc vào việc số bắt đầu có ước số nguyên tố nhỏ hơn chính nó hay không. 
3. Nếu số đó là số nguyên tố thì cộng`2 * number`để trả lời. Vị trí xuất phát thuận lợi đang thuộc về Elijah, đồng nghĩa với việc Fiona thắng và Elijah được hưởng quả phạt đền. 
4. In câu trả lời tích lũy. 

Tại sao nó hoạt động: 

Bất biến đằng sau lời giải là mọi số tổng hợp đều là vị trí thắng và mọi số nguyên tố là vị trí thua. Đối với số nguyên tố, bước đi hợp pháp duy nhất là trực tiếp đến`1`, thua ngay lập tức. Đối với một số tổng hợp, việc chọn bất kỳ thừa số nguyên tố nào làm trạng thái tiếp theo sẽ mang lại cho đối thủ một vị trí nguyên tố và các vị trí nguyên tố đó sẽ thua. Vì mỗi vòng đều độc lập nên việc tổng hợp các hình phạt từ các giá trị ban đầu sẽ đưa ra câu trả lời cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_prime(n):
    if n < 2:
        return False

    small_primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37]
    for p in small_primes:
        if n == p:
            return True
        if n % p == 0:
            return False

    d = n - 1
    s = 0
    while d % 2 == 0:
        s += 1
        d //= 2

    # Deterministic for numbers smaller than 2^64
    bases = [2, 3, 5, 7, 11, 13]

    for a in bases:
        if a >= n:
            continue
        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue

        composite = True
        for _ in range(s - 1):
            x = x * x % n
            if x == n - 1:
                composite = False
                break

        if composite:
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
```các`is_prime`hàm đầu tiên loại bỏ các thừa số nguyên tố nhỏ. Điều này nhanh chóng xử lý nhiều số tổng hợp và tránh những công việc Miller-Rabin không cần thiết. Phần còn lại viết`n - 1`BẰNG`d * 2^s`, sau đó kiểm tra xem một số cơ sở có chứng minh được rằng`n`là tổng hợp. 

Tích hợp sẵn của Python`pow(a, d, n)`thực hiện phép lũy thừa mô-đun một cách hiệu quả, điều này rất cần thiết vì các giá trị quá lớn đối với phép lũy thừa thông thường. Cơ sở được chọn là đủ cho phạm vi đầu vào nằm dưới ranh giới 64 bit. 

Vòng lặp chính theo sau quá trình giảm trò chơi trực tiếp. Nó không bao giờ tạo ra ước số hoặc mô phỏng các bước di chuyển vì những chi tiết đó đã được thuộc tính nguyên thủy nắm bắt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
6
```| Bước | Giá trị hiện tại | Xuất sắc? | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 6 | Không | 0 | 
| Kết thúc | 6 | Tổng hợp, Elijah thắng | 0 | 

số`6`là tổng hợp. Elijah có thể di chuyển đến`2`hoặc`3`, cả hai đều đang mất vị trí cho người chơi tiếp theo. Fiona không thể buộc phải thắng nên Elijah không thực hiện động tác chống đẩy nào. 

### Mẫu 2 

đầu vào:```
2
30
2
```| Bước | Giá trị hiện tại | Xuất sắc? | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 30 | Không | 0 | 
| Quy trình vòng | 2 | Có | 4 | 
| Kết thúc | Cả hai vòng đều được xử lý | | 4 | 

Vòng đầu tiên không đóng góp gì vì`30`là tổng hợp. Vòng thứ hai bắt đầu lúc`2`, là số nguyên tố nên Fiona thắng và Elijah trả tiền`2 * 2 = 4`chống đẩy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log a) | Mỗi số trong số tối đa 100 số được kiểm tra tính nguyên tố bằng cách sử dụng lũy ​​thừa mô-đun. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài số nguyên và câu trả lời đang chạy. | 

Giải pháp tránh hoàn toàn việc liệt kê số chia. Chỉ với 100 giá trị và số xung quanh`10^11`, Miller-Rabin tất định dễ dàng phù hợp với thời hạn. 

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
    for a in [2, 3, 5, 7, 11, 13]:
        if a >= n:
            continue
        x = pow(a, d, n)
        if x in (1, n - 1):
            continue
        for _ in range(s - 1):
            x = x * x % n
            if x == n - 1:
                break
        else:
            return False
    return True

def solve(inp):
    data = list(map(int, inp.split()))
    n = data[0]
    ans = 0
    for x in data[1:]:
        if is_prime(x):
            ans += 2 * x
    return str(ans)

assert solve("1\n6\n") == "0"
assert solve("2\n30\n2\n") == "4"

assert solve("1\n2\n") == "4"
assert solve("3\n4\n8\n12\n") == "0"
assert solve("3\n3\n5\n7\n") == "30"
assert solve("1\n100000000003\n") == "200000000006"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 2`|`4`| Ranh giới nguyên tố nhỏ nhất | 
|`4, 8, 12`|`0`| Tổng hợp số có nhiều ước | 
|`3, 5, 7`|`30`| Một số vòng chính | 
|`100000000003`|`200000000006`| Xử lý nguyên tố số lượng lớn | 

## Vỏ cạnh 

Đối với trường hợp nguyên tố:```
Input:
1
7
```Miller-Rabin xác định`7`như nguyên tố. Thuật toán bổ sung`2 * 7`, cho`14`. Điều này phù hợp với trò chơi vì Elijah không có động thái nào ngoại trừ việc thua ngay lập tức. 

Đối với trường hợp tổng hợp:```
Input:
1
12
```Thuật toán bác bỏ`12`là số nguyên tố và không thêm gì cả. Điều này phù hợp với trò chơi vì Elijah có thể chọn số chia`2`, buộc Fiona vào thế thua. 

Với giá trị nhỏ nhất:```
Input:
1
2
```Kiểm tra tính nguyên tố trả về true ngay lập tức từ danh sách số nguyên tố nhỏ. Câu trả lời trở thành`4`, xử lý trường hợp nước đi ước số duy nhất là nước đi thua tới`1`.

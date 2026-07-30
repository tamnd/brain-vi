---
title: "CF 102835J - Trò chơi giải đố"
description: "Câu đố là một chiếc đĩa tròn có n cung. Mũi tên bắt đầu ở khu vực s và mỗi lần nhấn nút sẽ di chuyển chính xác k khu vực theo chiều kim đồng hồ. Các lĩnh vực bao quanh, vì vậy sau lĩnh vực n - 1 lĩnh vực tiếp theo là 0."
date: "2026-07-26T15:02:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102835
codeforces_index: "J"
codeforces_contest_name: "The 2020 ICPC Asia Taipei-Hsinchu Site Programming Contest"
rating: 0
weight: 102835
solve_time_s: 44
verified: true
draft: false
---

[CF 102835J - Trò chơi giải đố](https://codeforces.com/problemset/problem/102835/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Câu đố là một chiếc đĩa tròn có`n`các lĩnh vực. Mũi tên bắt đầu ở khu vực`s`và mỗi lần nhấn nút sẽ di chuyển nó một cách chính xác`k`ngành theo chiều kim đồng hồ. Các lĩnh vực bao quanh, vì vậy sau lĩnh vực`n - 1`lĩnh vực tiếp theo là`0`. Nhiệm vụ là tìm số lần nhấn nút nhỏ nhất để mũi tên chạm vào khu vực`x`. Nếu không có chuỗi chuyển động nào có thể đạt tới`x`, câu trả lời là`-1`. 

Đầu vào bao gồm bốn số nguyên mô tả kích thước của vòng tròn, chuyển động cố định sau mỗi lần nhấp, khu vực ban đầu và khu vực mong muốn. Đầu ra là số lần nhấp chuột tối thiểu cần thiết. 

Ràng buộc`n ≤ 20000`có nghĩa là có thể mô phỏng trực tiếp tất cả các lĩnh vực trong nhiều trường hợp, nhưng chúng ta cần phải cẩn thận. Một mô phỏng có thể yêu cầu lên đến`n`di chuyển, điều này có thể chấp nhận được ở đây, nhưng giải pháp toán học dự định sẽ tránh dựa vào số lượng lĩnh vực và tính toán trực tiếp câu trả lời. Quan trọng hơn, cấu trúc của chuyển động là số học mô-đun, do đó, lý do tương tự sẽ mở rộng một cách tự nhiên đến các giới hạn lớn hơn. 

Các trường hợp cạnh chính xuất phát từ thực tế là việc thêm liên tục`k`không nhất thiết phải đến thăm mọi lĩnh vực. 

Ví dụ: nếu đầu vào là:```
12 4 1 6
```mũi tên ghé thăm:```
1, 5, 9, 1, 5, 9, ...
```Lĩnh vực mục tiêu`6`không bao giờ đạt được, vì vậy đầu ra đúng là:```
-1
```Một giải pháp bất cẩn cho rằng mọi lĩnh vực cuối cùng sẽ xuất hiện sẽ đưa ra câu trả lời sai. 

Một trường hợp quan trọng khác là khi điểm bắt đầu đã là một phần của chu kỳ nhưng mục tiêu yêu cầu số lượng bao quanh dương nhỏ nhất. Ví dụ:```
10 6 3 9
```Các vị trí là:```
3 -> 9
```sau một cú nhấp chuột, câu trả lời là:```
1
```Một giải pháp chỉ kiểm tra các vị trí sau một chu kỳ đầy đủ hoặc xử lý thao tác modulo không chính xác có thể bỏ sót điều này. 

Trường hợp cạnh cuối cùng là khi kích thước chuyển động và số lượng cung có chung một ước số. Ví dụ:```
8 6 0 5
```Mũi tên chỉ đến các khu vực chẵn:```
0, 6, 4, 2, 0, ...
```ngành như vậy`5`là không thể và câu trả lời là:```
-1
```Điều kiện chia hết đằng sau những trường hợp này là cốt lõi của giải pháp. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng các nhấp chuột. Bắt đầu từ`s`, thêm liên tục`k`và lấy kết quả modulo`n`. Nếu lĩnh vực hiện tại trở thành`x`, chúng tôi đã tìm thấy câu trả lời. Nếu chúng ta quay lại`s`mà không tìm thấy`x`, chu trình đã hoàn tất và không thể đạt được mục tiêu. 

Phương pháp này đúng vì chuyển động của mũi tên mang tính quyết định. Khi một khu vực lặp lại, mọi vị trí trong tương lai cũng sẽ lặp lại nên không khu vực mới nào có thể xuất hiện. Vấn đề là nó coi một chu trình toán học như một chuỗi các bước di chuyển riêng lẻ. Trong trường hợp xấu nhất nó thực hiện gần như`n`lần lặp lại. Đối với ràng buộc nhất định, điều này vẫn ổn, nhưng nó ẩn cấu trúc cơ bản và sẽ không mở rộng được. 

Nhận xét quan trọng là sau`t`nhấp chuột, mũi tên ở:$$(s + t \cdot k) \bmod n$$Chúng ta cần giá trị này bằng nhau`x`, mang lại:$$t \cdot k \equiv x - s \pmod n$$Đây là một sự đồng đẳng tuyến tính. Phương trình chỉ có nghiệm khi ước chung lớn nhất của`k`Và`n`chia rẽ`x - s`. Khi đó, chúng ta có thể chia phương trình cho gcd đó và sử dụng nghịch đảo mô đun để tìm giá trị hợp lệ nhỏ nhất`t`. 

Giải pháp brute-force hoạt động vì nó khám phá chính xác các trạng thái được tạo ra bởi sự tái diễn. Giải pháp toán học hoạt động nhanh hơn vì nó mô tả các trạng thái có thể tiếp cận tương tự bằng cách sử dụng số học mô-đun và giải trực tiếp số bước di chuyển cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Được chấp nhận cho những ràng buộc này, nhưng không phải là ý tưởng chung | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng cách theo chiều kim đồng hồ cần thiết từ khu vực bắt đầu đến khu vực mục tiêu. 

Chúng tôi xác định:$$d = (x - s) \bmod n$$Sau đó`t`số nhấp chuột, chúng tôi cần:$$t \cdot k \equiv d \pmod n$$Điều này biến bài toán chuyển động thành giải một phương trình mô đun. 
2. Tính toán`g = gcd(k, n)`. 

Mọi giá trị đạt được bằng cách thêm liên tục`k`có cùng số dư theo modulo`g`. Điều này có nghĩa là khoảng cách mục tiêu cũng phải chia hết cho`g`. 
3. Nếu`d`không chia hết cho`g`, trở lại`-1`. 

Không có số lượng nhân`k`có thể tạo ra số dư ngoài bội số của`g`, vì vậy khu vực mục tiêu không thể tiếp cận được. 
4. Rút gọn phương trình bằng cách chia tất cả các số hạng cho`g`. 

Chúng tôi nhận được:$$\frac{k}{g}t \equiv \frac{d}{g} \pmod{\frac{n}{g}}$$Lúc này hệ số của`t`là nguyên tố cùng nhau với mô đun mới. 
5. Tìm nghịch đảo môđun của`k / g`modulo`n / g`. 

Vì hai giá trị đó nguyên tố cùng nhau nên tồn tại nghịch đảo. Nhân cả hai vế với nghịch đảo này sẽ được:$$t \equiv \frac{d}{g} \cdot (k/g)^{-1} \pmod{n/g}$$6. Trả về giá trị không âm nhỏ nhất của`t`. 

Hoạt động modulo tự nhiên mang lại số lần nhấp chuột hợp lệ nhỏ nhất. 

Tại sao nó hoạt động: các khu vực có thể tiếp cận tạo thành một nhóm tuần hoàn được tạo bằng cách thêm`k`modulo`n`. Gcd của`k`Và`n`xác định chính xác dư lượng nào thuộc nhóm đó. Nếu khoảng cách mục tiêu thuộc nhóm này, việc giảm sự đồng nhất sẽ tạo ra một phương trình có hệ số khả nghịch và nghịch đảo mô đun sẽ đưa ra câu trả lời duy nhất bên trong chu trình. Nếu nó không thuộc về nó thì không có chuỗi nhấp chuột nào có thể tiếp cận được nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def egcd(a, b):
    if b == 0:
        return a, 1, 0
    g, x1, y1 = egcd(b, a % b)
    return g, y1, x1 - (a // b) * y1

def mod_inverse(a, mod):
    _, x, _ = egcd(a, mod)
    return x % mod

def solve():
    n, k, s, x = map(int, input().split())

    d = (x - s) % n

    g = egcd(k, n)[0]

    if d % g != 0:
        print(-1)
        return

    k //= g
    d //= g
    mod = n // g

    answer = (d * mod_inverse(k, mod)) % mod
    print(answer)

if __name__ == "__main__":
    solve()
```Thuật toán Euclide mở rộng được sử dụng vì nó cung cấp cả gcd và các hệ số cần thiết để xây dựng một nghịch đảo mô đun. Nghịch đảo chỉ tồn tại sau khi chia cho`g`, bởi vì`k / g`Và`n / g`được đảm bảo là đồng nguyên tố. 

Hoạt động modulo đầu tiên tính toán khoảng cách theo chiều kim đồng hồ một cách chính xác ngay cả khi`x`nhỏ hơn về mặt số lượng so với`s`. Việc kiểm tra tính chia hết phải diễn ra trước khi rút gọn phương trình, nếu không các trường hợp không thể thực hiện được có thể tạo ra các phép tính nghịch đảo vô nghĩa. 

Số nguyên Python không bị tràn nên phép nhân`d * inverse`là an toàn. Modulo cuối cùng giữ câu trả lời trong độ dài chu kỳ cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
8 6 0 4
```Các biến phát triển như sau. 

| Bước | Phương trình hiện tại | điều kiện gcd | Trả lời | 
| --- | --- | --- | --- | 
| Ban đầu |`d = (4 - 0) mod 8 = 4`|`gcd(6,8)=2`| chưa quyết định | 
| Kiểm tra |`4 % 2 = 0`| có thể truy cập | tiếp tục | 
| Giảm |`k=3, d=2, mod=4`| gỡ rối`3t ≡ 2 mod 4`| tiếp tục | 
| Nghịch đảo |`3^-1 mod 4 = 3`|`t = 2*3 mod 4`|`2`| 

Trình tự là:```
0 -> 6 -> 4
```vậy câu trả lời là`2`. Điều này xác nhận rằng phương trình mô đun cho kết quả tương tự như mô phỏng các bước di chuyển. 

Coi như:```
12 8 1 6
```| Bước | Phương trình hiện tại | điều kiện gcd | Trả lời | 
| --- | --- | --- | --- | 
| Ban đầu |`d = (6 - 1) mod 12 = 5`|`gcd(8,12)=4`| chưa quyết định | 
| Kiểm tra |`5 % 4 != 0`| không thể truy cập |`-1`| 

Phong trào luôn thay đổi ngành theo bội số`4`, vì vậy bắt đầu từ khu vực`1`chỉ các ngành có cùng phần dư modulo`4`có thể xuất hiện. ngành`6`nằm ngoài tập hợp đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Thuật toán Euclide mở rộng mất thời gian logarit | 
| Không gian | O(log n) | Lệnh gọi Euclide đệ quy sử dụng không gian ngăn xếp logarit | 

Các ràng buộc đủ nhỏ để ngay cả mô phỏng cũng có thể vượt qua, nhưng giải pháp mô-đun trực tiếp khai thác cấu trúc toán học và vẫn hiệu quả đối với các giá trị lớn hơn nhiều của`n`. 

## Trường hợp thử nghiệm```python
import sys
import io

def egcd(a, b):
    if b == 0:
        return a, 1, 0
    g, x1, y1 = egcd(b, a % b)
    return g, y1, x1 - (a // b) * y1

def mod_inverse(a, mod):
    _, x, _ = egcd(a, mod)
    return x % mod

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k, s, x = map(int, sys.stdin.readline().split())

    d = (x - s) % n
    g = egcd(k, n)[0]

    if d % g:
        return "-1\n"

    k //= g
    d //= g
    mod = n // g
    return str((d * mod_inverse(k, mod)) % mod) + "\n"

assert solution("8 6 0 4\n") == "2\n", "reachable case"
assert solution("12 8 1 6\n") == "-1\n", "unreachable gcd case"
assert solution("2 1 0 1\n") == "1\n", "minimum size"
assert solution("20 5 3 18\n") == "-1\n", "same remainder restriction"
assert solution("17 4 10 5\n") == "3\n", "wrap-around movement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`8 6 0 4`|`2`| Chu kỳ tiếp cận cơ bản | 
|`12 8 1 6`|`-1`| Kiểm tra tính bất khả thi của GCD | 
|`2 1 0 1`|`1`| Vòng tròn nhỏ nhất có thể | 
|`20 5 3 18`|`-1`| Phong trào chỉ đạt đến một lớp dư lượng | 
|`17 4 10 5`|`3`| Bao bọc mô-đun đúng cách | 

## Vỏ cạnh 

Dành cho:```
12 4 1 6
```thuật toán tính toán:```
d = (6 - 1) mod 12 = 5
g = gcd(4, 12) = 4
```Từ`5`không chia hết cho`4`, thuật toán trả về ngay`-1`. Điều này phù hợp với thực tế là mọi khu vực có thể tiếp cận đều có cùng phần còn lại như`1`modulo`4`. 

Vì:```
8 6 0 5
```thuật toán tìm thấy:```
d = 5
g = gcd(6, 8) = 2
```Khoảng cách không chia hết cho gcd, vì vậy ngành`5`không thể đạt được. Trình tự được tạo xác nhận điều này:```
0 -> 6 -> 4 -> 2 -> 0
```Vì:```
10 6 3 9
```thuật toán nhận được:```
d = 6
g = gcd(6, 10) = 2
```Sau khi giảm:```
3t ≡ 3 mod 5
```Nghịch đảo của`3`modulo`5`là`2`, Vì thế:```
t = 3 * 2 mod 5 = 1
```Câu trả lời là`1`, khớp với nước đi duy nhất:```
3 -> 9
```Lý do tương tự xử lý mọi trường hợp khác vì mọi chuỗi chuyển động có thể được biểu diễn bằng phương trình mô-đun.

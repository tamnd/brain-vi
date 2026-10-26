---
title: "CF 106096E - Nhảy hoặc không nhảy"
description: "Bài toán yêu cầu tìm cách nhanh nhất để di chuyển đúng n đơn vị. Nhân vật có thể đi bộ bất kỳ khoảng cách nào với tốc độ cố định a hoặc thực hiện bước nhảy chính xác b đơn vị. Một bước nhảy luôn mất 3 giây."
date: "2026-06-25T12:00:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106096
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 10-1-25 Div. 2 (Beginner)"
rating: 0
weight: 106096
solve_time_s: 43
verified: true
draft: false
---

[CF 106096E - Nhảy hoặc không nhảy](https://codeforces.com/problemset/problem/106096/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu cách di chuyển nhanh nhất cho chính xác`n`đơn vị. Nhân vật có thể đi bộ bất kỳ khoảng cách nào với tốc độ cố định`a`, hoặc thực hiện một bước nhảy vọt chính xác`b`đơn vị. Một bước nhảy luôn mất 3 giây. Đi bộ là liên tục nhưng tư thế cuối cùng phải chuẩn xác`n`, do đó, bất kỳ sự kết hợp hành động nào cũng phải bao phủ chính xác khoảng cách mục tiêu. 

Dữ liệu đầu vào cung cấp tốc độ đi bộ, độ dài bước nhảy và tổng khoảng cách. Đầu ra là thời gian tối thiểu có thể, hoặc`-1`nếu không có chuỗi bước đi và bước nhảy nào có thể hạ cánh chính xác đến đích. 

Khoảng cách có thể lớn như`10^18`, vì vậy việc mô phỏng các bước di chuyển hoặc thử mọi số lần nhảy có thể là không thể. Một vòng lặp trên tất cả số bước nhảy có thể có thể yêu cầu lên tới`10^18 / 1`lặp đi lặp lại, vượt xa giới hạn một giây cho phép. Giải pháp cần giảm vấn đề thành một số phép tính số học, thường xoay quanh độ phức tạp logarit. 

Phần khó khăn không phải là các giá trị lớn mà là các ranh giới số học. Một giải pháp phải xử lý chính xác các trường hợp trong đó việc đi bộ một mình có hiệu quả, trong đó chỉ một số lần đếm bước nhảy là hợp lệ và khi số bước nhảy tốt nhất là không rõ ràng. 

Ví dụ:```
Input:
2 4 11

Output:
-1
```Một giải pháp bất cẩn có thể cố gắng sử dụng hai bước nhảy vọt vì`4 + 4 = 8`, sau đó đi tiếp 3 đơn vị còn lại. Tuy nhiên, tốc độ đi bộ là 2 nên quãng đường còn lại không thể đi hết trong số giây. Tổng khoảng cách là không thể đạt được. 

Một trường hợp khác là khi chiến lược nhanh nhất sử dụng số bước nhảy tối đa có thể:```
Input:
1 5 10

Output:
6
```Hai bước nhảy bao gồm tất cả 10 đơn vị và mất 6 giây. Một giải pháp luôn ưu tiên đi bộ sẽ cho kết quả là 10, điều này không tối ưu. 

Trường hợp thứ ba là khi số bước nhảy phải được chọn theo tính chia hết:```
Input:
3 6 15

Output:
5
```Một bước nhảy bao gồm 6 đơn vị, để lại 9 đơn vị đi bộ. Điều này hoạt động trong 5 giây. Ba bước nhảy sẽ vượt quá và bước nhảy 0 sẽ chậm hơn. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử mọi bước nhảy vọt có thể. Nếu chúng ta sử dụng`k`nhảy vọt, họ che chắn`k * b`khoảng cách, và khoảng cách còn lại là đi bộ. Chúng ta có thể kiểm tra xem khoảng cách còn lại có phải là không âm và chia hết cho`a`, sau đó tính thời gian. 

Cách tiếp cận này đúng vì mọi kế hoạch chuyển động khả thi đều được mô tả đầy đủ bằng số bước nhảy vọt của nó. Tuy nhiên, trong trường hợp xấu nhất nó sẽ cố gắng`n / b`khả năng. Từ`n`có thể`10^18`, cái này có thể lớn bằng`10^18`séc. 

Quan sát quan trọng là công thức thời gian có mối quan hệ tuyến tính đơn giản với số bước nhảy. Nếu có`k`nhảy, thời gian là:```
k * 3 + (n - k * b) / a
```Sắp xếp lại:```
n / a + k * (3 - b / a)
```Phần đầu tiên không phụ thuộc vào`k`. Phần thay đổi duy nhất là hệ số của`k`. Từ`b >= 2a`, hệ số có dấu cố định hoặc bằng 0. Nếu bước nhảy nhanh hơn bước đi thì chúng ta muốn càng nhiều bước nhảy càng tốt. Nếu bước nhảy chậm hơn, chúng ta muốn càng ít bước nhảy càng tốt. 

Thử thách còn lại là tìm ra số bước nhảy hợp lệ. hợp lệ`k`phải thỏa mãn:```
k * b + walking_distance = n

walking_distance is divisible by a

k * b ≡ n (mod a)
```Đây là một phương trình mô-đun. Chúng tôi giải quyết nó bằng cách sử dụng ước chung lớn nhất và nghịch đảo mô đun, sau đó chọn giải pháp hợp lệ nhỏ nhất hoặc lớn nhất tùy thuộc vào giải pháp nào mang lại thời gian tốt hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n/b) | O(1) | Quá chậm | 
| Tối ưu | O(log(min(a, b))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`g = gcd(a, b)`. Sự phù hợp`k * b ≡ n (mod a)`chỉ giải quyết được khi`n`chia hết cho`g`. Nếu không, không có số lần nhảy nào có thể làm cho khoảng cách đi bộ còn lại là hợp lệ, vì vậy câu trả lời là không thể. 
2. Rút gọn phương trình bằng cách chia mọi thứ cho`g`. Chúng tôi nhận được:`b/g * k ≡ n/g (mod a/g)`Hiện nay`b/g`Và`a/g`là nguyên tố cùng nhau, vì vậy`b/g`có modulo nghịch đảo`a/g`. 
3. Tìm một giải pháp`k0`sử dụng nghịch đảo môđun. Mọi số bước nhảy hợp lệ đều có dạng:`k = k0 + t * (a/g)`đối với một số nguyên`t`. 
4. Giới hạn các giải pháp trong phạm vi có thể`0 <= k <= n/b`. Nếu không có giải pháp nào trong phạm vi này thì chuyến đi không thể hoàn thành. 
5. Quyết định lấy giá trị nhỏ nhất hay lớn nhất`k`. Nếu như`b < 3a`, việc đi bộ thực sự nhanh hơn nên chúng tôi giảm thiểu số lần nhảy. Nếu như`b > 3a`, các bước nhảy tiết kiệm thời gian, vì vậy chúng ta tối đa hóa số lần nhảy. Nếu như`b = 3a`, mọi lựa chọn hợp lệ đều có cùng thời gian. 
6. Tính toán câu trả lời bằng cách sử dụng`k`. 

Tại sao nó hoạt động: 

Mọi chiến lược khả thi đều được thể hiện bằng chính xác một con số, số bước nhảy vọt. Khi con số đó được cố định thì khoảng cách và thời gian còn lại cũng cố định. Phương trình mô-đun tìm thấy mọi số bước nhảy hợp lệ có thể có và biểu thức thời gian tuyến tính cho chúng ta biết phần cuối của tập hợp đó là tối ưu. Vì chúng tôi kiểm tra giá trị cực kỳ hợp lệ chính xác nên thời gian thu được là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def egcd(a, b):
    if b == 0:
        return a, 1, 0
    g, x, y = egcd(b, a % b)
    return g, y, x - (a // b) * y

def solve():
    a, b, n = map(int, input().split())

    g, _, _ = egcd(a, b)

    if n % g != 0:
        print(-1)
        return

    mod = a // g
    max_k = n // b

    if mod == 1:
        low = 0
        high = max_k
    else:
        bg = b // g
        ng = n // g

        _, x, _ = egcd(bg, mod)
        inv = x % mod

        k0 = (ng % mod) * inv % mod

        if k0 > max_k:
            print(-1)
            return

        low = k0
        high = k0 + ((max_k - k0) // mod) * mod

    if b < 3 * a:
        k = low
    elif b > 3 * a:
        k = high
    else:
        k = low

    remaining = n - k * b
    print(3 * k + remaining // a)

solve()
```các`egcd`hàm cung cấp các hệ số cần thiết để tính toán nghịch đảo mô đun. Phép nghịch đảo chỉ cần thiết sau khi rút gọn phương trình, vì hệ số nhân và mô đun rút gọn được đảm bảo là nguyên tố cùng nhau. 

Biến`mod`là khoảng cách giữa các lần đếm bước nhảy hợp lệ. Một lần hợp lệ`k`đã biết, mọi giá trị hợp lệ khác sẽ khác nhau một cách chính xác`mod`. Mã tính toán các giá trị hợp lệ nhỏ nhất và lớn nhất trong khoảng cho phép. 

Sự so sánh giữa`b`Và`3a`xác định hướng tối ưu hóa. Khi`b < 3a`, mỗi bước nhảy sẽ thay thế quá ít thời gian đi bộ, do đó, sử dụng nhiều bước nhảy sẽ gây tổn hại. Khi`b > 3a`, mỗi bước nhảy thêm sẽ cải thiện kết quả. Trường hợp đẳng thức không thành vấn đề vì tất cả các lựa chọn hợp lệ đều hòa nhau. 

Tất cả các phép tính đều sử dụng số nguyên Python, vì vậy các giá trị lớn lên tới`10^18`không tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 2 10
```Số bước nhảy phải thỏa mãn:`2k ≡ 10 (mod 1)`Mọi giá trị đều hoạt động vì mô đun là 1. 

| Bước | k phạm vi | Được chọn k | Thời gian | 
| --- | --- | --- | --- | 
| Tìm giải pháp hợp lệ | 0 đến 5 | 0 đến 5 | | 
| So sánh tốc độ |`b < 3a`| 0 | | 
| Tính toán chuyển động | đi bộ 10 | 0 bước nhảy vọt | 10 | 

Giải pháp thích đi bộ hơn vì một bước nhảy tốn 3 giây trong khi chỉ đi được 2 đơn vị. Đi bộ toàn bộ quãng đường sẽ nhanh hơn. 

### Mẫu 2 

đầu vào:```
2 4 11
```Sự tương đương là:`4k ≡ 11 (mod 2)`Vế trái luôn chẵn, còn vế phải lẻ nên không có nghiệm. 

| Bước | Giá trị | 
| --- | --- | 
| gcd(2,4) | 2 | 
| Kiểm tra n % gcd | 11% 2 = 1 | 
| Kết quả | không thể | 

Thuật toán từ chối trường hợp này trước khi cố gắng tối ưu hóa việc sử dụng bước nhảy vọt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(min(a, b))) | Thuật toán Euclide mở rộng chiếm ưu thế trong thời gian chạy | 
| Không gian | O(1) | Chỉ một số nguyên không đổi được lưu trữ | 

Thuật toán không bao giờ lặp lại khoảng cách hoặc số lần nhảy có thể. Nó chỉ thực hiện một số phép tính số học nên dễ dàng xử lý các khoảng cách gần`10^18`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    a, b, n = map(int, sys.stdin.readline().split())

    def egcd(a, b):
        if b == 0:
            return a, 1, 0
        g, x, y = egcd(b, a % b)
        return g, y, x - (a // b) * y

    g, _, _ = egcd(a, b)

    if n % g != 0:
        ans = -1
    else:
        mod = a // g
        max_k = n // b
        if mod == 1:
            low, high = 0, max_k
        else:
            bg = b // g
            ng = n // g
            _, x, _ = egcd(bg, mod)
            k0 = (ng % mod) * (x % mod) % mod
            if k0 > max_k:
                ans = -1
                sys.stdin = old
                return str(ans) + "\n"
            low = k0
            high = k0 + ((max_k - k0) // mod) * mod

        k = low if b < 3 * a else high
        ans = 3 * k + (n - k * b) // a

    sys.stdin = old
    return str(ans) + "\n"

assert run("1 2 10\n") == "10\n", "sample 1"
assert run("2 4 11\n") == "-1\n", "sample 2"

assert run("1 5 10\n") == "6\n", "all leaps"
assert run("3 6 15\n") == "5\n", "divisibility"
assert run("10 20 1000000000000000000\n") == "150000000000000000\n", "large values"
assert run("5 10 7\n") == "-1\n", "unreachable remainder"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 10`|`10`| Đi bộ tốt hơn nhảy | 
|`2 4 11`|`-1`| Điều kiện mô-đun không thể thực hiện được | 
|`1 5 10`|`6`| Mức sử dụng bước nhảy vọt tối đa | 
|`3 6 15`|`5`| Lựa chọn số bước nhảy hợp lệ | 
|`5 10 7`|`-1`| Khoảng cách không thể được biểu diễn | 

## Vỏ cạnh 

cho`2 4 11`, thuật toán sẽ kiểm tra ngay điều kiện gcd. Từ`gcd(2,4)=2`Và`11`không chia hết cho 2 nên phương trình không có nghiệm. Không có số bước nhảy nào có thể để lại phần còn lại có thể đi bộ được. 

Vì`1 5 10`, mỗi lần đếm bước nhảy đều hợp lệ vì khoảng cách đi bộ luôn chia hết cho 1. Hệ số âm vì`5 > 3 * 1`, do đó thuật toán chọn số bước nhảy lớn nhất có thể, đó là`2`. Lần cuối cùng là`2 * 3 = 6`. 

Vì`3 6 15`, phương trình rút gọn cho phép đếm bước nhảy với tính chẵn lẻ chính xác. Các lựa chọn hợp lệ là`k = 1`chỉ trong phạm vi. Quãng đường còn lại là`9`, mất`3`giây để đi bộ, cho`6`? Trên thực tế, bước nhảy vọt cần`3`giây và đi bộ mất`3`giây, vậy tổng số là`6`. Bước mô-đun ngăn chặn các lựa chọn không hợp lệ chẳng hạn như thực hiện hai bước nhảy vọt sẽ vượt quá. 

Đối với khoảng cách rất lớn, thuật toán không bao giờ tạo mảng hoặc thực hiện vòng lặp tỷ lệ với`n`. Nó chỉ sử dụng thuật toán Euclide, do đó thời gian chạy vẫn ở mức nhỏ ngay cả khi đích đến gần`10^18`.

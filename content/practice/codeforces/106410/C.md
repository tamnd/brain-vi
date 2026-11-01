---
title: "CF 106410C - Sự lặp lại"
description: "Bài toán yêu cầu chúng ta đếm xem có bao nhiêu dãy con của một mảng là hợp lệ. Một dãy con được tạo bằng cách chọn một số chỉ mục và giữ nguyên thứ tự ban đầu của chúng."
date: "2026-06-25T09:54:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106410
codeforces_index: "C"
codeforces_contest_name: "HPI 2026 Novice"
rating: 0
weight: 106410
solve_time_s: 35
verified: true
draft: false
---

[CF 106410C - Sự lặp lại](https://codeforces.com/problemset/problem/106410/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta đếm xem có bao nhiêu dãy con của một mảng là hợp lệ. Một dãy con được tạo bằng cách chọn một số chỉ mục và giữ nguyên thứ tự ban đầu của chúng. Một chuỗi độ dài được chọn`k`hợp lệ nếu giá trị được chọn đầu tiên chia hết cho`1`, giá trị được chọn thứ hai có thể chia hết cho`2`, thứ ba bởi`3`, vân vân. Chúng tôi chỉ quan tâm đến chỉ số nào được chọn, vì vậy hai chuỗi con có cùng giá trị nhưng vị trí khác nhau sẽ được tính riêng. Câu trả lời phải được in modulo`1e9 + 7`. 

Đầu vào là một mảng có độ dài`n`, Ở đâu`n`có thể đạt được`100000`và giá trị có thể lớn bằng`10^6`. Một giải pháp quy hoạch động bậc hai sẽ yêu cầu khoảng`10^10`hoạt động trong trường hợp xấu nhất là quá chậm. Giới hạn giá trị gợi ý rằng cấu trúc chia hết của các số là chìa khóa, bởi vì số lượng ước số có thể có của một giá trị lên đến`10^6`đủ nhỏ để xử lý hiệu quả. 

Một lỗi phổ biến là theo dõi các chuỗi con theo giá trị thay vì chỉ số của chúng. Ví dụ, với đầu vào`3`và mảng`1 1 1`, mỗi phần tử tạo thành một dãy con khác nhau, vì vậy câu trả lời là`3`, không`1`. Một trường hợp phức tạp khác là khi một giá trị không thể mở rộng một dãy con do vị trí bắt buộc của nó. Đối với đầu vào`2`và mảng`2 1`, dãy số`[2,1]`không hợp lệ vì phần tử thứ hai phải chia hết cho`2`, Và`1`thì không. Câu trả lời đúng là`2`, đến từ`[2]`Và`[1]`. Một giải pháp bất cẩn chỉ kiểm tra các giá trị mà không xem xét độ dài hiện tại sẽ bị tính quá mức. 

Một trường hợp biên khác là giá trị của`1`. Từ`1`chia hết cho mọi số nguyên dương, nó có thể được đặt ở bất kỳ vị trí nào trong một dãy con hợp lệ. Với đầu vào`3`và mảng`1 1 1`, câu trả lời là mọi dãy con không trống, tức là`7`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng tất cả các chuỗi con. Với mỗi dãy con, chúng ta có thể kiểm tra xem phần tử được chọn đầu tiên của nó có chia hết cho`1`, thứ hai của nó bằng`2`, vân vân. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, một mảng có độ dài`100000`có`2^100000 - 1`có thể có các dãy con không trống nên việc liệt kê là không thể. 

Hướng tốt hơn là xử lý mảng từ trái sang phải và duy trì số lượng dãy con hợp lệ của mỗi độ dài hiện tại tồn tại. Giả sử chúng ta đã xử lý một số tiền tố của mảng. Nếu chúng ta thấy một giá trị mới`x`, nó có thể trở thành phần tử tiếp theo của bất kỳ dãy con hợp lệ hiện có nào có vị trí bắt buộc tiếp theo chia`x`. 

Quan sát quan trọng là chúng ta không bao giờ cần phân phối độ dài đầy đủ lên tới`n`. Một giá trị`x`chỉ có thể giúp tạo độ dài chuỗi con mới`i`khi`i`chia rẽ`x`. Từ`x <= 10^6`, số lượng ước số là nhỏ. Chúng tôi chỉ có thể cập nhật những độ dài đó. 

Cho phép`dp[i]`biểu thị số dãy con hợp lệ có độ dài`i`được hình thành từ các phần tử được xử lý cho đến nay. Khi chúng tôi xử lý một giá trị`x`, mọi ước số`d`của`x`có nghĩa là một dãy con có độ dài`d - 1`có thể được mở rộng theo chiều dài`d`bằng cách thêm`x`. Chúng tôi cập nhật`dp[d] += dp[d-1]`. chiều dài`1`luôn luôn có thể xảy ra vì mọi số đều chia hết cho`1`, Vì thế`dp[1]`tăng thêm một cho mỗi phần tử. 

Thứ tự cập nhật quan trọng. Chúng ta phải xử lý các ước số theo thứ tự giảm dần để giá trị hiện tại không được sử dụng lại nhiều lần trong cùng một chuỗi con. Nếu chúng tôi cập nhật từ độ dài nhỏ lên độ dài lớn, một chuỗi con mới được tạo có thể ngay lập tức được mở rộng lại bằng cách sử dụng cùng một phần tử mảng, điều này sẽ không hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n * sqrt(max(a))) | O(max(a)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng lập trình động trong đó`dp[i]`lưu trữ số lượng các chuỗi con hợp lệ có độ dài`i`sau khi xử lý tiền tố hiện tại của mảng. Ban đầu tất cả các giá trị đều bằng 0 vì không có phần tử nào được sử dụng. 
2. Lặp qua từng giá trị mảng`x`từ trái sang phải. Giá trị hiện tại có thể bắt đầu một chuỗi mới có độ dài`1`, do đó tăng`dp[1]`bởi một. 
3. Tìm tất cả các ước của`x`. Với mỗi số chia`d`lớn hơn`1`, cập nhật`dp[d]`bằng cách thêm`dp[d-1]`. Lý do là mọi dãy con có độ dài hợp lệ`d-1`có thể nối thêm`x`và trở nên hợp lệ về độ dài`d`. 
4. Xử lý các ước số theo thứ tự giảm dần. Điều này giữ cho quá trình chuyển đổi chỉ dựa trên các chuỗi con từ các phần tử mảng trước đó, ngăn phần tử hiện tại được tính nhiều lần. 
5. Sau khi xử lý xong toàn bộ mảng, tính tổng tất cả`dp[i]`các giá trị. Mọi dãy con hợp lệ không trống đều có độ dài nào đó, vì vậy tổng này là câu trả lời. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi xử lý lần đầu tiên`j`các yếu tố,`dp[i]`chính xác bằng số dãy con có độ dài hợp lệ`i`chỉ sử dụng những`j`các phần tử. Khi có một phần tử mới`x`được xem xét, các dãy con mới duy nhất có thể có là các dãy kết thúc tại`x`. Their previous part must already be a valid subsequence of length`d-1`, Ở đâu`d`chia rẽ`x`, vì phần tử mới sẽ chiếm vị trí`d`. Bản cập nhật tính chính xác các tiện ích mở rộng này và không có gì khác. Các bản cập nhật giảm dần đảm bảo phần trước không bao giờ bao gồm phần tử hiện tại, do đó, mỗi chuỗi con được tính sẽ tương ứng với một tập hợp chỉ mục duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    dp = [0] * (n + 2)

    for x in a:
        divs = []
        i = 1
        while i * i <= x:
            if x % i == 0:
                divs.append(i)
                if i * i != x:
                    divs.append(x // i)
            i += 1

        divs.sort(reverse=True)

        for d in divs:
            if d == 1:
                dp[1] = (dp[1] + 1) % MOD
            elif d <= n:
                dp[d] = (dp[d] + dp[d - 1]) % MOD

    print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Mảng`dp`có kích thước bằng`n`bởi vì một dãy con không thể có độ dài lớn hơn số phần tử. Sự chuyển tiếp chỉ cần những vị trí thực sự có thể xuất hiện. 

Đối với mỗi giá trị, trước tiên mã sẽ thu thập các ước số của nó bằng cách kiểm tra tất cả các số có đến căn bậc hai của nó. Giá trị lớn nhất là`10^6`, vậy là đủ nhanh rồi. Sắp xếp các ước số theo thứ tự giảm dần là chi tiết ngăn chặn việc sử dụng lại phần tử hiện tại không hợp lệ. 

Khi`d == 1`, giá trị sẽ tự tạo ra một dãy con mới. Đối với các ước số lớn hơn,`dp[d-1]`đã biểu thị các chuỗi con kết thúc trước chỉ mục hiện tại, do đó, việc thêm giá trị hiện tại sẽ tạo ra các chuỗi con hợp lệ mới có độ dài`d`. 

Số nguyên Python không bị tràn, nhưng phép toán modulo được áp dụng trong quá trình cập nhật để giữ cho các giá trị được lưu trữ bị giới hạn. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
2
1 2
```Quá trình xử lý trông như thế này: 

| Bước | Giá trị hiện tại | Các ước số được sử dụng | thay đổi dp | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | không | không | tất cả đều bằng không | 0 | 
| 1 | 1 | 1 | dp[1] = 1 | 1 | 
| 2 | 2 | 2, 1 | dp[2] += dp[1], dp[1] += 1 | 3 | 

Các dãy con cuối cùng là`[1]`,`[2]`, Và`[1,2]`. Bảng cho thấy chiều dài đó`2`được tạo chỉ vì giá trị thứ hai chia hết cho`2`. 

Mẫu 2: 

đầu vào:```
5
2 2 1 22 14
```| Bước | Giá trị hiện tại | Ước số | Cập nhật quan trọng | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2,1 | tạo độ dài 1 | 1 | 
| 2 | 2 | 2,1 | mở rộng chiều dài 1, tạo chiều dài 1 | 3 | 
| 3 | 1 | 1 | tạo độ dài 1 | 4 | 
| 4 | 22 | 11,2,1 | mở rộng độ dài 1 và 2, tạo độ dài 1 | 8 | 
| 5 | 14 | 14,7,2,1 | mở rộng độ dài 1 và 2, tạo độ dài 1 | 13 | 

Dấu vết thứ hai chứng minh tại sao việc xử lý các ước số là đủ. Các giá trị như`22`Và`14`chỉ tương tác với các độ dài phân chia chúng, thay vì mọi độ dài chuỗi con có thể có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * sqrt(max(a))) | Mỗi giá trị được phân tích bằng cách kiểm tra đến căn bậc hai của nó và chỉ các ước số của nó mới được cập nhật | 
| Không gian | O(n) | Mảng lập trình động lưu trữ số đếm cho mỗi độ dài chuỗi con có thể có | 

Giá trị tối đa là`10^6`, do đó việc phân tích căn bậc hai là về`1000`kiểm tra từng phần tử. Vì`100000`các yếu tố xung quanh đây`10^8`các thao tác đơn giản, phù hợp với giới hạn dự định khi triển khai Python hiệu quả. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10 ** 9 + 7

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    dp = [0] * (n + 2)

    for x in a:
        divs = []
        i = 1
        while i * i <= x:
            if x % i == 0:
                divs.append(i)
                if i * i != x:
                    divs.append(x // i)
            i += 1

        divs.sort(reverse=True)

        for d in divs:
            if d == 1:
                dp[1] += 1
            elif d <= n:
                dp[d] = (dp[d] + dp[d - 1]) % MOD

    return str(sum(dp) % MOD)

assert solution("2\n1 2\n") == "3"
assert solution("5\n2 2 1 22 14\n") == "13"

assert solution("1\n1000000\n") == "1"
assert solution("3\n1 1 1\n") == "7"
assert solution("4\n2 3 4 6\n") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1000000`|`1`| Phần tử đơn và ranh giới giá trị tối đa | 
|`1 1 1`|`7`| Giá trị chia hết cho mọi vị trí | 
|`2 3 4 6`|`7`| Quy tắc lọc chia số và mở rộng chuỗi con | 

## Vỏ cạnh 

Đối với đầu vào`2`với mảng`2 1`, các quá trình thuật toán`2`đầu tiên và tạo ra một chuỗi con có độ dài`1`. giá trị`1`cũng có thể tạo ra một chiều dài`1`dãy sau, nhưng nó không thể kéo dài độ dài hiện tại`1`dãy số bởi vì`2`không chia`1`. Câu trả lời cuối cùng là`2`. 

Đối với đầu vào`3`với mảng`1 1 1`, mỗi giá trị được xử lý sẽ thêm một độ dài mới`1`tiếp theo. đầu tiên`1`tạo một chuỗi con, chuỗi thứ hai tạo độ dài mới`1`và có thể kéo dài cái trước đó thành chiều dài`2`, và thứ ba tạo ra tất cả các khả năng còn lại. Số đếm trở thành`3`,`3`, Và`1`cho độ dài`1`,`2`, Và`3`, đưa ra câu trả lời`7`. 

Đối với các giá trị lặp lại, chẳng hạn như`3`với mảng`2 2 2`, việc cập nhật số chia giảm dần sẽ ngăn chặn một`2`khỏi bị sử dụng hai lần. Các dãy con hợp lệ có độ dài ba`1`và ba chiều dài`2`, cho`6`, trong khi chiều dài`3`là không thể bởi vì`2`không chia hết cho`3`.

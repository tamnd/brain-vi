---
title: "CF 267A - Phép trừ"
description: "Chúng ta áp dụng lặp lại thao tác tương tự cho hai số nguyên dương. Ở mỗi bước, chúng tôi trừ giá trị nhỏ hơn khỏi giá trị lớn hơn. Quá trình dừng lại ngay khi một số trở thành số 0. Nhiệm vụ là đếm xem có bao nhiêu phép trừ được thực hiện cho mỗi cặp."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 267
codeforces_index: "A"
codeforces_contest_name: "Codeforces Testing Round 5"
rating: 900
weight: 267
solve_time_s: 161
verified: true
draft: false
---

[CF 267A - Phép trừ](https://codeforces.com/problemset/problem/267/A) 

**Xếp hạng:** 900 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta áp dụng lặp lại thao tác tương tự cho hai số nguyên dương. Ở mỗi bước, chúng tôi trừ giá trị nhỏ hơn khỏi giá trị lớn hơn. Quá trình dừng lại ngay khi một số trở thành số 0. Nhiệm vụ là đếm xem có bao nhiêu phép trừ được thực hiện cho mỗi cặp. 

Ví dụ, bắt đầu từ`(4, 17)`:`(4,17) → (4,13) → (4,9) → (4,5) → (4,1) → (3,1) → (2,1) → (1,1) → (0,1)`Trình tự này chứa`8`hoạt động. 

Đầu vào chứa tối đa`1000`các cặp độc lập và mỗi giá trị có thể lớn bằng`10^9`. Mô phỏng trực tiếp thực hiện một phép trừ cho mỗi thao tác có thể trở nên cực kỳ chậm. Coi như`(1, 10^9)`. Quá trình thực hiện gần một tỷ thao tác vì số lượng lớn hơn chỉ giảm đi một`1`mỗi lần. Bất kỳ thuật toán nào mô phỏng theo nghĩa đen mọi phép trừ sẽ vượt quá giới hạn thời gian. 

Cấu trúc của quy trình rất giống với thuật toán Euclide cho ước số chung lớn nhất. Thay vì trừ một lần trong mỗi lần lặp, chúng ta có thể đếm xem có bao nhiêu phép trừ xảy ra cùng một lúc bằng cách sử dụng phép chia số nguyên. 

Có một số trường hợp góc dễ bị bỏ sót. 

Khi các số bằng nhau, một thao tác ngay lập tức biến một giá trị thành 0. Ví dụ:```
5 5
```Câu trả lời là`1`, không`5`. Việc thực hiện bất cẩn liên tục trừ đi cho đến khi số lớn hơn trở nên nhỏ hơn có thể vô tình bị tính quá mức. 

Các cặp trong đó một số lớn hơn nhiều so với số kia là một cái bẫy khác. Ví dụ:```
1 1000000000
```Câu trả lời đúng là`1000000000`. Một vòng lặp ngây thơ sẽ mất một tỷ lần lặp. 

Một trường hợp tinh tế khác xuất hiện khi số lớn hơn là bội số chính xác của số nhỏ hơn:```
3 12
```Trình tự là:`(3,12) → (3,9) → (3,6) → (3,3) → (0,3)`Câu trả lời là`4`. Chỉ sử dụng phép toán còn lại mà không đếm số lần trừ sẽ làm mất thông tin. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng quy trình chính xác như mô tả. Ở mỗi bước, hãy so sánh hai số và trừ số nhỏ hơn cho số lớn hơn. Tăng câu trả lời sau mỗi lần trừ. 

Điều này hoạt động vì các quy tắc là xác định. Mọi thao tác đều làm giảm tổng của hai số, do đó quá trình cuối cùng sẽ kết thúc. 

Vấn đề là hiệu suất. Trong trường hợp xấu nhất, chẳng hạn như`(1, 10^9)`, thuật toán thực hiện một tỷ lần lặp. Điều đó vượt xa những gì phù hợp với giới hạn thời gian một giây. 

Quan sát quan trọng là phép trừ lặp đi lặp lại tương đương với phép chia. 

Giả sử chúng ta có`(a, b)`với`a < b`. Phép trừ liên tục`a`từ`b`tiếp tục cho đến khi`b < a`. Số phép trừ được thực hiện chính xác là`b // a`, và giá trị còn lại trở thành`b % a`. 

Ví dụ:`(4,17)`Trừ`4`nhiều lần:`17 → 13 → 9 → 5 → 1`Việc này mất`17 // 4 = 4`hoạt động và giá trị mới là`17 % 4 = 1`. 

Vì vậy, thay vì thực hiện bốn thao tác riêng biệt, chúng ta có thể chuyển thẳng đến kết quả trong một bước. 

Điều này biến đổi quá trình thành thuật toán Euclide. Mỗi lần lặp sẽ giảm một số thành số dư và tổng số phép trừ được tích lũy thông qua phép chia số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời) | O(1) | Quá chậm | 
| Tối ưu | O(log(max(a,b))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Cho mỗi cặp`(a, b)`, khởi tạo`ans = 0`. 
3. Trong khi cả hai số đều dương, hãy làm như sau: 

Nếu`a < b`, sau đó`b`có thể trừ`a`chính xác`b // a`lần trước khi trở nên nhỏ hơn`a`. 

Thêm vào`b // a`để trả lời và thay thế`b`với`b % a`. 
4. Ngược lại, nếu`a >= b`, sau đó`a`có thể trừ`b`chính xác`a // b`lần trước khi trở nên nhỏ hơn`b`. 

Thêm vào`a // b`để trả lời và thay thế`a`với`a % b`. 
5. Tiếp tục cho đến khi một số trở thành số 0. 
6. In câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Ở mọi giai đoạn, quy trình thực hiện số phép trừ giống hệt nhau tối đa có thể trong một lần nhảy. 

Nếu như`b > a`, trừ đi nhiều lần`a`từ`b`về mặt toán học giống hệt với:```
b = b - a * (b // a)
```Giá trị còn lại chính xác là`b % a`và số thao tác được thực hiện là`b // a`. 

Thuật toán duy trì số lượng trình tự chính xác trong khi bỏ qua các trạng thái trung gian không cần thiết. Vì mỗi lần lặp lại thay thế một giá trị bằng phần còn lại nhỏ hơn rất nhiều nên các số giảm nhanh chóng, giống hệt như thuật toán Euclide. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    a, b = map(int, input().split())
    
    ans = 0
    
    while a > 0 and b > 0:
        if a < b:
            ans += b // a
            b %= a
        else:
            ans += a // b
            a %= b
    
    print(ans)
```Giải pháp tuân theo cấu trúc thuật toán Euclide chặt chẽ. 

Vòng lặp tiếp tục trong khi cả hai số vẫn dương. Khi một giá trị trở thành 0 thì không thể thực hiện phép trừ nào nữa, do đó quá trình sẽ kết thúc một cách tự nhiên. 

Chi tiết quan trọng nhất là cộng thương số trước khi lấy phần dư. Thương số biểu thị số lượng phép trừ riêng lẻ bị bỏ qua. Việc quên bước này sẽ chỉ tính toán quá trình gcd cuối cùng chứ không tính tổng số thao tác. 

Sử dụng modulo là an toàn vì phép trừ và modulo lặp đi lặp lại tạo ra cùng một phần dư. Phiên bản modulo chỉ đơn giản nén nhiều phép tính thành một bước số học. 

Số nguyên Python xử lý các giá trị lên tới`10^9`một cách dễ dàng, do đó không có mối lo ngại tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 17
```| một | b | thương số được thêm vào | tổng hoạt động | 
| --- | --- | --- | --- | 
| 4 | 17 | 17 // 4 = 4 | 4 | 
| 4 | 1 | 4 // 1 = 4 | 8 | 
| 0 | 1 | dừng lại | 8 | 

Lần lặp đầu tiên nén bốn phép trừ cùng một lúc. Lần lặp thứ hai nén bốn lần giảm một bước còn lại. Câu trả lời cuối cùng phù hợp với mô phỏng thủ công đầy đủ. 

### Ví dụ 2 

đầu vào:```
7 987654321
```| một | b | thương số được thêm vào | tổng hoạt động | 
| --- | --- | --- | --- | 
| 7 | 987654321 | 141093474 | 141093474 | 
| 7 | 3 | 2 | 141093476 | 
| 1 | 3 | 3 | 141093479 | 
| 1 | 0 | dừng lại | 141093479 | 

Ví dụ này cho thấy tại sao phương pháp tối ưu hóa lại cần thiết. Chỉ riêng lần lặp đầu tiên đã bỏ qua hơn một trăm triệu phép trừ riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(max(a,b))) | Mỗi lần lặp hoạt động giống như thuật toán Euclide và giảm nhanh các số | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Các ràng buộc cho phép các giá trị lên tới`10^9`, nhưng thuật toán Euclide kết thúc với rất ít lần lặp, thường dưới vài chục bước cho mỗi trường hợp thử nghiệm. Điều này dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    
    out = []
    
    for _ in range(t):
        a, b = map(int, input().split())
        
        ans = 0
        
        while a > 0 and b > 0:
            if a < b:
                ans += b // a
                b %= a
            else:
                ans += a // b
                a %= b
        
        out.append(str(ans))
    
    print("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided samples
assert run("2\n4 17\n7 987654321\n") == "8\n141093479\n", "sample 1"

# minimum values
assert run("1\n1 1\n") == "1\n", "minimum case"

# exact multiple
assert run("1\n3 12\n") == "4\n", "exact multiple"

# consecutive numbers
assert run("1\n8 5\n") == "5\n", "fibonacci-like behavior"

# large imbalance
assert run("1\n1 1000000000\n") == "1000000000\n", "large quotient"

# equal large numbers
assert run("1\n1000000000 1000000000\n") == "1\n", "equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Chấm dứt ngay lập tức sau một lần trừ | 
|`3 12`|`4`| Xử lý nhiều chính xác | 
|`8 5`|`5`| Rút gọn nhiều lần Euclide | 
|`1 1000000000`|`1000000000`| Nén thương số lớn | 
|`1000000000 1000000000`|`1`| Trường hợp ranh giới có giá trị bằng nhau | 

## Vỏ cạnh 

Xét các số bằng nhau:```
5 5
```Dấu vết thực hiện: 

| một | b | hoạt động được thêm vào | tổng cộng | 
| --- | --- | --- | --- | 
| 5 | 5 | 1 | 1 | 
| 0 | 5 | dừng lại | 1 | 

Thuật toán đếm chính xác một phép trừ đơn lẻ vì`5 // 5 = 1`. 

Bây giờ hãy xem xét một sự mất cân bằng lớn:```
1 1000000000
```Dấu vết thực hiện: 

| một | b | hoạt động được thêm vào | tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 1000000000 | 1000000000 | 1000000000 | 
| 1 | 0 | dừng lại | 1000000000 | 

Phương pháp tối ưu hóa kết thúc trong một lần lặp thay vì một tỷ lần lặp. 

Cuối cùng, hãy xem xét bội số chính xác:```
3 12
```Dấu vết thực hiện: 

| một | b | hoạt động được thêm vào | tổng cộng | 
| --- | --- | --- | --- | 
| 3 | 12 | 4 | 4 | 
| 3 | 0 | dừng lại | 4 | 

Thương số ghi lại tất cả các phép trừ lặp lại một cách chính xác. Việc triển khai bất cẩn chỉ sử dụng modulo mà không thêm thương số sẽ trả về không chính xác các phép toán ở đây.

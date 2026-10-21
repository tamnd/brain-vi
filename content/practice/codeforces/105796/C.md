---
title: "CF 105796C - Thuật toán Euclide"
description: "Nhiệm vụ là đếm xem có bao nhiêu lần lặp được thực hiện bởi một phiên bản chậm của thuật toán Euclide. Thuật toán bắt đầu với hai số nguyên dương. Mặc dù hai giá trị này khác nhau, nhưng nó liên tục trừ giá trị nhỏ hơn khỏi giá trị lớn hơn và tăng bộ đếm."
date: "2026-06-25T15:37:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105796
codeforces_index: "C"
codeforces_contest_name: "UNICAMP Selection Contest 2024"
rating: 0
weight: 105796
solve_time_s: 39
verified: true
draft: false
---

[CF 105796C - Thuật toán Euclides](https://codeforces.com/problemset/problem/105796/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là đếm xem có bao nhiêu lần lặp được thực hiện bởi một phiên bản chậm của thuật toán Euclide. Thuật toán bắt đầu với hai số nguyên dương. Mặc dù hai giá trị này khác nhau, nhưng nó liên tục trừ giá trị nhỏ hơn khỏi giá trị lớn hơn và tăng bộ đếm. Câu trả lời là giá trị cuối cùng của bộ đếm này, không phải là ước số chung lớn nhất. 

Ví dụ, bắt đầu bằng`15 10`, quá trình này là`15 10 -> 5 10 -> 5 5`, vậy câu trả lời là`2`. Thách thức là các giá trị đầu vào có thể cực kỳ lớn nên việc mô phỏng mọi phép trừ là không thể. 

Đầu vào chứa tối đa 500 cặp số nguyên dương và mỗi số nguyên có thể lớn bằng`10^18`. Một mô phỏng trực tiếp có thể yêu cầu gần như`10^18`hoạt động. Một cặp như`1 1000000000000000`sẽ trừ`1`từ con số lớn hơn gần một triệu triệu lần, không thể phù hợp với bất kỳ giới hạn thời gian thực tế nào. Thay vào đó, giải pháp phải xử lý toàn bộ nhóm các bước trừ giống hệt nhau. 

Các trường hợp phức tạp đến từ điều kiện dừng chính xác và từ các thương số rất lớn. Nếu cả hai số đều bằng nhau thì không có phép trừ nào xảy ra.```
Input
7 7
```Đầu ra đúng là:```
0
```Việc thực hiện bất cẩn luôn thực hiện ít nhất một phép trừ sẽ tạo ra kết quả không chính xác. 

Một trường hợp cạnh khác là khi một số chia hết cho số kia.```
Input
1 1000000000000000
```Đầu ra đúng là:```
999999999999999
```Phép trừ cuối cùng rất đặc biệt vì sau nhiều lần trừ`1`, các số trở nên bằng nhau. Đếm`1000000000000000`phép trừ sẽ là một quá nhiều. 

Trường hợp thứ ba là khi phép chia thứ nhất để lại phần dư.```
Input
15 10
```Đầu ra đúng là:```
2
```Phép trừ đầu tiên thay đổi cặp thành`5 10`, và cái thứ hai thay đổi nó thành`5 5`. Xử lý số lượng hoạt động chỉ là thương số`15 // 10`sẽ bỏ lỡ bước Euclide sau này. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là thực hiện chính xác thuật toán đã cho. Trong khi hai số khác nhau, hãy trừ số nhỏ hơn từ số lớn hơn và tăng câu trả lời. Điều này đúng vì mỗi lần lặp vòng lặp khớp với một phép trừ trong quy trình ban đầu. 

Vấn đề là số lần lặp lại. Nếu các giá trị là`1`Và`10^18`, vòng lặp thực hiện`999999999999999`hoạt động. Ngay cả một ca kiểm thử như vậy cũng quá lớn và có thể có hàng trăm ca kiểm thử. 

Quan sát quan trọng là nhiều lần lặp vòng lặp liên tiếp đều thực hiện điều tương tự. Khi số lớn hơn nhiều so với số nhỏ, thuật toán sẽ liên tục trừ đi cùng một giá trị. Thay vì thực hiện các phép trừ này một cách riêng lẻ, chúng ta có thể đếm xem có bao nhiêu phép trừ xảy ra trong một nhóm. 

Giả sử các giá trị hiện tại là`a > b`. Viết:```
a = q * b + r
```Nếu như`r`không bằng 0, thuật toán chậm sẽ trừ`b`chính xác`q`lần trước khi giá trị lớn hơn trở thành`r`. Trạng thái thay đổi từ`(a, b)`ĐẾN`(b, r)`. 

Nếu như`r`bằng 0, sau`q - 1`phép trừ các giá trị trở nên bằng nhau. các`q`- Phép trừ thứ không được thực hiện vì vòng lặp dừng ngay khi đạt đến đẳng thức. Nhà nước trở thành`(b, b)`và đóng góp`q - 1`hoạt động. 

Đây là ý tưởng nén tương tự đằng sau thuật toán Euclide, nhưng thay vì chỉ tìm gcd, chúng tôi theo dõi số bước trừ mà phiên bản chậm hơn sẽ thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời) | O(1) | Quá chậm | 
| Tối ưu | O(log(max(a, b))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc một cặp số và khởi tạo bộ đếm câu trả lời về 0. 
2. Nếu hai số đã bằng nhau thì trả về 0 vì điều kiện vòng lặp ban đầu là sai ngay lập tức. 
3. Dù các số khác nhau, hãy tạo số lớn hơn`a`và số nhỏ hơn`b`. 
4. Tính toán`a // b`Và`a % b`. Thương số cho biết thuật toán chậm sẽ trừ bao nhiêu lần`b`từ`a`trước khi giá trị giảm xuống dưới`b`hoặc trở nên bằng nhau. 
5. Thêm`a // b`vào câu trả lời khi còn dư. Nếu không có số dư, hãy thêm một số ít hơn thương vì phép trừ cuối cùng sẽ làm cho các số bằng nhau và kết thúc vòng lặp. 
6. Thay thế cặp bằng trạng thái Euclide tiếp theo`(b, a % b)`và tiếp tục cho đến khi cả hai giá trị bằng nhau. 

Lý do điều này có hiệu quả là vì mỗi bước nén biểu thị một chuỗi các phép trừ giống hệt nhau liên tiếp. Điều chỉnh duy nhất cần thiết là khi số lớn hơn là bội số chính xác của số nhỏ hơn, vì thuật toán ban đầu dừng một phép trừ sớm hơn quá trình chia thông thường. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, thuật toán chậm chỉ thay đổi số lớn hơn bằng cách trừ đi số nhỏ hơn. Việc nhóm các thao tác giống hệt nhau đó lại với nhau sẽ không thay đổi giá trị trung gian sau khi nhóm kết thúc. 

Vì`a > b`, thương số`a // b`chính xác là số lần tối đa`b`có thể bị trừ trước khi giá trị nhỏ hơn`b`hoặc đạt tới số không. Nếu có số dư, tất cả các phép trừ đó sẽ xảy ra trước cặp tiếp theo`(b, remainder)`xuất hiện. Nếu không có phần dư thì phép trừ cuối cùng sẽ tạo ra đẳng thức nên bị loại trừ. Việc lặp lại phép biến đổi này tuân theo các trạng thái giống như thuật toán ban đầu, chỉ bỏ qua các thao tác riêng lẻ bên trong mỗi nhóm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_operations(a, b):
    ans = 0

    while a != b:
        if a < b:
            a, b = b, a

        q, r = divmod(a, b)

        if r == 0:
            ans += q - 1
            break
        else:
            ans += q
            a, b = b, r

    return ans

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        a, b = map(int, input().split())
        out.append(str(count_operations(a, b)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```chức năng`count_operations`giữ cặp giá trị hiện tại và số bước trừ đã nén. Việc hoán đổi làm cho`a`giá trị lớn hơn trước khi áp dụng phép chia, khớp với điều kiện trong vòng lặp ban đầu. 

các`divmod`call tính cả thương và số dư cùng một lúc. Điều này tránh việc tính toán chúng theo cách thủ công và trực tiếp hiển thị số lần trừ lặp lại. 

các`r == 0`nhánh xử lý điều kiện biên trong đó hai số trở nên bằng nhau. Thêm`q`ở đây sẽ đếm quá một vì vòng lặp ban đầu không thực hiện phép trừ tạo ra đẳng thức. 

Số nguyên Python có thể lưu trữ các giá trị lớn hơn`10^18`, do đó không có nguy cơ tràn. Vòng lặp chỉ tuân theo phép rút gọn Euclide nên số lần lặp vẫn còn nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
15 10
```| Bước | một | b | Thương số | Phần còn lại | Đã thêm hoạt động | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 15 | 10 | 1 | 5 | 1 | 1 | 
| Tiếp tục | 10 | 5 | 2 | 0 | 1 | 2 | 
| Kết thúc | 5 | 5 | - | - | - | 2 | 

Bước nén đầu tiên thể hiện phép trừ`15 - 10 = 5`. Bước thứ hai có phép chia chính xác nên chỉ tính một phép trừ vì phép trừ tiếp theo sẽ làm cho các số bằng nhau. 

### Ví dụ 2 

đầu vào:```
1 1000000000000000
```| Bước | một | b | Thương số | Phần còn lại | Đã thêm hoạt động | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 1000000000000000 | 1 | 1000000000000000 | 0 | 999999999999999 | 999999999999999 | 
| Kết thúc | 1 | 1 | - | - | - | 999999999999999 | 

Dấu vết này cho thấy tại sao việc mô phỏng quá trình là không thể. Toàn bộ chuỗi gần một triệu triệu phép trừ giống hệt nhau được biểu diễn bằng một phép chia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(max(a, b))) | Mỗi lần lặp lại tuân theo mô hình giảm phần dư giống như thuật toán Euclid. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Giá trị tối đa là`10^18`, do đó số lượng rút gọn Euclide là nhỏ. Với tối đa 500 trường hợp thử nghiệm, điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def count_operations(a, b):
    ans = 0
    while a != b:
        if a < b:
            a, b = b, a
        q, r = divmod(a, b)
        if r == 0:
            ans += q - 1
            break
        ans += q
        a, b = b, r
    return ans

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    t = int(sys.stdin.readline())
    ans = []
    for _ in range(t):
        a, b = map(int, sys.stdin.readline().split())
        ans.append(str(count_operations(a, b)))
    sys.stdin = old_stdin
    return "\n".join(ans)

assert run("""4
1 1
15 10
1 1000000000000000
1234500 54321001234500
""") == """0
2
999999999999999
44002456""", "provided samples"

assert run("""1
1 2
""") == "1", "minimum values"

assert run("""1
1000000000000000000 1
""") == "999999999999999999", "maximum quotient"

assert run("""1
42 42
""") == "0", "equal values"

assert run("""1
100 25
""") == "3", "exact division boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Vòng lặp không thực hiện thao tác nào khi các giá trị bắt đầu bằng nhau. | 
|`1 2`|`1`| Cặp không bằng nhau nhỏ nhất được xử lý chính xác. | 
|`1000000000000000000 1`|`999999999999999999`| Số lượng rất lớn được nén thay vì mô phỏng. | 
|`42 42`|`0`| Điều kiện chấm dứt đẳng thức. | 
|`100 25`|`3`| Phép chia chính xác yêu cầu trừ một ít hơn thương. | 

## Vỏ cạnh 

Đối với các giá trị bằng nhau, chẳng hạn như:```
Input
7 7
```thuật toán bước vào kiểm tra điều kiện vòng lặp và ngay lập tức trả về 0. Không có thương số nào được tính toán vì không có phép trừ nào tồn tại trong quy trình ban đầu. 

Đối với bội số lớn, chẳng hạn như:```
Input
1 1000000000000000
```thuật toán hoán đổi các giá trị để giá trị lớn hơn được chia cho`1`. thương số là`1000000000000000`và phần còn lại bằng không. Nó thêm`999999999999999`, khớp với số lần vòng trừ chạy trước khi đạt`(1, 1)`. 

Đối với một cặp có số dư:```
Input
15 10
```thương số là`1`và phần còn lại là`5`, do đó một thao tác được tính và cặp này trở thành`(10, 5)`. Bước tiếp theo phát hiện phép chia chính xác với thương số`2`, chỉ thêm`1`, và kết thúc tại`(5, 5)`. Tổng cộng là`2`, khớp chính xác với quá trình trừ ban đầu.

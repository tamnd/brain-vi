---
title: "CF 102881G - Tất nhiên là bé Ehab và vấn đề về GCD"
description: "Chúng tôi có một bộ sưu tập các hình khối được đánh số ban đầu trống. Mỗi thao tác cộng mọi khối lập phương có số nằm trong một khoảng bao hàm cho trước [l, r]. Sau mỗi thao tác, chúng ta cần in ước chung lớn nhất của tất cả các số đã xuất hiện trong tập hợp cho đến nay."
date: "2026-07-25T12:40:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102881
codeforces_index: "G"
codeforces_contest_name: "ECPC 2019 Kickoff"
rating: 0
weight: 102881
solve_time_s: 40
verified: true
draft: false
---

[CF 102881G - Tất nhiên là em bé Ehab và vấn đề về GCD](https://codeforces.com/problemset/problem/102881/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các hình khối được đánh số ban đầu trống. Mỗi thao tác cộng mọi khối lập phương có số nằm trong một khoảng bao hàm nhất định`[l, r]`. Sau mỗi thao tác, chúng ta cần in ước chung lớn nhất của tất cả các số đã xuất hiện trong tập hợp cho đến nay. 

Đầu vào là một chuỗi các phép cộng khoảng. Chi tiết quan trọng là bộ sưu tập chỉ phát triển nên mỗi câu trả lời phụ thuộc vào tất cả các khoảng thời gian trước đó cũng như khoảng thời gian hiện tại. Đầu ra sau mỗi thao tác là GCD toàn cầu hiện tại. 

Số lượng hoạt động có thể đạt tới`10^5`và các giá trị bên trong các khoảng có thể lớn bằng`10^18`. Điều này ngay lập tức loại trừ việc lặp lại các số trong một khoảng. Một khoảng có thể chứa tới`10^18`nên việc xử lý một truy vấn lớn bằng cách truy cập mọi giá trị là không thể. Chúng tôi cần cập nhật theo thời gian không đổi hoặc theo thời gian logarit cho mỗi truy vấn. 

Các trường hợp cạnh chính xuất phát từ các khoảng trông có vẻ lớn nhưng đơn giản về mặt toán học. Khoảng một giá trị và khoảng nhiều giá trị hoạt động hoàn toàn khác nhau. Ví dụ: với đầu vào:```
1
2250 2250
```câu trả lời là:```
2250
```bởi vì khối duy nhất được thêm vào có giá trị`2250`. 

Một trường hợp khác là:```
2
10 10
15 15
```Câu trả lời là:```
10
5
```Câu trả lời thứ hai là không`15`, bởi vì các hình khối hiện tại là`10`Và`15`, GCD của nó là`5`. 

Trường hợp ranh giới quan trọng nhất là khoảng có hai giá trị liên tiếp:```
1
7 8
```Câu trả lời là:```
1
```Một giải pháp bất cẩn có thể cố gắng tính toán GCD chỉ bằng cách sử dụng các điểm cuối và nhận được`gcd(7, 8) = 1`, điều này xảy ra ở đây, nhưng lý do sâu xa hơn: mọi khoảng chứa các số nguyên liên tiếp đều có GCD`1`. Thuộc tính này cho phép chúng ta tránh xử lý khoảng thời gian. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ duy trì tất cả các số đã được thêm vào và tính toán GCD trên toàn bộ bộ sưu tập sau mỗi truy vấn. Điều này đúng vì GCD của một tập hợp chính xác là giá trị mà bài toán yêu cầu. Tuy nhiên, nó thất bại ngay lập tức trên các ràng buộc. Một khoảng như`[1, 10^18]`chứa một triệu tỷ số, do đó, ngay cả việc lưu trữ các giá trị cũng không thể thực hiện được và việc tính toán lại GCD sau mỗi thao tác có thể yêu cầu tới`10^23`hoạt động trên tất cả các truy vấn. 

Quan sát làm thay đổi vấn đề là chúng ta không bao giờ cần biết từng số riêng lẻ bên trong một khoảng. Chúng tôi chỉ cần GCD được đóng góp trong khoảng thời gian đó. 

Đối với bất kỳ khoảng nào chứa ít nhất hai số, có hai số nguyên liên tiếp bên trong nó. GCD của hai số nguyên liên tiếp luôn bằng`1`và việc thêm nhiều số hơn không thể làm tăng GCD. Vì vậy mỗi khoảng`[l, r]`Ở đâu`l < r`đóng góp GCD của`1`. 

Nếu khoảng chỉ chứa một số thì phần đóng góp của nó chỉ đơn giản là số đó. 

Toàn bộ vấn đề giảm xuống còn việc duy trì GCD của câu trả lời trước đó và sự đóng góp của khoảng thời gian mới. Một khi câu trả lời trở thành`1`, nó không bao giờ có thể thay đổi nữa bởi vì mọi GCD trong tương lai với`1`còn lại`1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số giá trị được chèn) | O(tổng số giá trị được chèn) | Quá chậm | 
| Tối ưu | O(q log A) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo GCD hiện tại như`0`. GCD của`0`và một giá trị`x`là`x`, cho phép bản cập nhật đầu tiên hoạt động một cách tự nhiên. 
2. Đối với mỗi khoảng thời gian`[l, r]`, hãy xác định GCD mà khoảng này thêm vào. Nếu như`l`Và`r`khác nhau, khoảng chứa các số liên tiếp nên đóng góp của nó là`1`. Mặt khác, sự đóng góp của nó là`l`. 
3. Kết hợp đóng góp mới với đáp án hiện tại bằng thao tác GCD. Giá trị cập nhật là GCD của mọi số được thấy cho đến nay. 
4. Xuất GCD đã cập nhật. 

Tại sao nó hoạt động: bất biến được duy trì sau mỗi truy vấn là`current_gcd`bằng GCD của mọi khối đã xuất hiện cho đến thời điểm đó. Khi một khoảng mới được thêm vào, GCD của tất cả các số trong khoảng đó là giá trị đơn hoặc`1`. Lấy GCD của tập hợp cũ và khoảng mới này sẽ cho ra chính xác GCD của tập hợp kết hợp, vì vậy bất biến vẫn đúng. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    q = int(input())
    ans = 0
    out = []

    for _ in range(q):
        l, r = map(int, input().split())

        if l != r:
            add = 1
        else:
            add = l

        ans = gcd(ans, add)
        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Biến`ans`lưu trữ GCD của mọi khối được xử lý cho đến nay. Bắt đầu lúc`0`tránh cần một trường hợp đặc biệt cho truy vấn đầu tiên vì`gcd(0, x)`trả lại`x`. 

Đối với mỗi khoảng, mã sẽ kiểm tra xem nó có chứa nhiều hơn một số hay không. điều kiện`l != r`là đủ vì khoảng này đã bao hàm, nên mọi điểm cuối khác nhau đều có nghĩa là có ít nhất hai số nguyên liên tiếp bên trong. 

Bản cập nhật sử dụng triển khai thuật toán Euclide tích hợp của Python thông qua`math.gcd`, xử lý các giá trị lên tới`10^18`một cách hiệu quả. Không cần lưu trữ mảng hoặc khoảng thời gian vì thông tin trong quá khứ được tóm tắt đầy đủ bằng một số nguyên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
2250 2250
126 126
1 6
6 8
```| Bước | Khoảng thời gian | Khoảng thời gian GCD | GCD hiện tại | 
| --- | --- | --- | --- | 
| 1 | [2250, 2250] | 2250 | 2250 | 
| 2 | [126, 126] | 126 | 18 | 
| 3 | [1, 6] | 1 | 1 | 
| 4 | [6, 8] | 1 | 1 | 

Hai thao tác đầu tiên chỉ cộng các số riêng lẻ nên câu trả lời thay đổi theo GCD của các giá trị đó. Phép toán thứ ba chứa các số liên tiếp, buộc câu trả lời trở thành`1`. Sau đó, những khoảng thời gian sau đó không thể thay đổi được. 

### Mẫu 2 

đầu vào:```
3
8 8
20 20
30 30
```| Bước | Khoảng thời gian | Khoảng thời gian GCD | GCD hiện tại | 
| --- | --- | --- | --- | 
| 1 | [8, 8] | 8 | 8 | 
| 2 | [20, 20] | 20 | 4 | 
| 3 | [30, 30] | 30 | 2 | 

Dấu vết này chứng minh rằng các khoảng số đơn được xử lý chính xác như việc chèn thêm một giá trị vào GCD hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log A) | Mỗi truy vấn thực hiện một thao tác GCD trên các số lên tới`10^18`. | 
| Không gian | O(1) | Chỉ có GCD hiện tại và lưu trữ đầu ra được duy trì. | 

Với`q`lên đến`10^5`, cách tiếp cận này chỉ thực hiện một số lượng nhỏ các phép toán số học trên mỗi truy vấn và dễ dàng đáp ứng các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    q = int(sys.stdin.readline())
    ans = 0
    out = []

    for _ in range(q):
        l, r = map(int, sys.stdin.readline().split())
        if l != r:
            ans = gcd(ans, 1)
        else:
            ans = gcd(ans, l)
        out.append(str(ans))

    print("\n".join(out))

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result.strip()

assert run("""4
2250 2250
126 126
1 6
6 8
""") == """2250
18
1
1""", "sample 1"

assert run("""3
8 8
20 20
30 30
""") == """8
4
2""", "sample 2"

assert run("""1
1 1
""") == "1", "minimum value"

assert run("""3
1000000000000000000 1000000000000000000
1000000000000000000 1000000000000000000
1 1000000000000000000
""") == """1000000000000000000
1000000000000000000
1""", "large values and interval collapse"

assert run("""4
42 42
42 42
42 42
42 42
""") == """42
42
42
42""", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giá trị đơn`[1,1]`|`1`| Xử lý khoảng thời gian nhỏ nhất có thể. | 
| Các giá trị đơn lớn theo sau là một khoảng lớn |`1`ở cuối | Xác nhận rằng phạm vi lớn không được lặp đi lặp lại. | 
| lặp đi lặp lại`[42,42]`khoảng thời gian | Luôn luôn`42`| Kiểm tra các đóng góp lặp đi lặp lại giống hệt nhau. | 
| Khoảng thời gian mẫu | Phù hợp với đầu ra mẫu | Xác nhận quá trình chuyển đổi chính từ bản cập nhật GCD thông thường sang bản cập nhật vĩnh viễn`1`tình trạng. | 

## Vỏ cạnh 

Đối với trường hợp một giá trị:```
1
2250 2250
```thuật toán nhìn thấy`l == r`, do đó khoảng đóng góp`2250`. GCD trước đó là`0`, đưa ra câu trả lời mới`gcd(0, 2250) = 2250`. 

Đối với nhiều giá trị riêng lẻ:```
2
10 10
15 15
```bản cập nhật đầu tiên mang lại`10`. Thứ hai đóng góp`15`, vì vậy câu trả lời trở thành`gcd(10, 15) = 5`. Thuật toán không bao giờ cần lưu trữ khối nào vì GCD hiện tại chứa tất cả thông tin cần thiết. 

Đối với một khoảng chứa các giá trị liên tiếp:```
1
7 8
```khoảng thời gian đóng góp`1`bởi vì nó chứa cả hai`7`Và`8`, Và`gcd(7, 8) = 1`. Câu trả lời toàn cầu trở thành`1`. Bất kỳ hoạt động nào trong tương lai sẽ giữ câu trả lời tại`1`, vì không có số nguyên nào có GCD lớn hơn`1`khi kết hợp với GCD hiện có của`1`.

---
title: "CF 106486D - \u65e0\u4eba\u80fd\u8bc1\u7684\u731c\u60f3"
description: "Chúng ta bắt đầu từ số nguyên dương $x$. Ở mỗi bước, chúng tôi áp dụng phép biến đổi Collatz cổ điển. Nếu giá trị hiện tại là số lẻ, nó sẽ trở thành $3x+1$. Nếu giá trị hiện tại là số chẵn, nó sẽ trở thành $x/2$. Nhiệm vụ không phải là mô phỏng mãi mãi."
date: "2026-06-19T17:31:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106486
codeforces_index: "D"
codeforces_contest_name: "Dalian University of Technology, Software College 2025 Freshman Contest"
rating: 0
weight: 106486
solve_time_s: 42
verified: true
draft: false
---

[CF 106486D - \u65e0\u4eba\u80fd\u8bc1\u7684\u731c\u60f3](https://codeforces.com/problemset/problem/106486/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu từ một số nguyên dương$x$. Ở mỗi bước, chúng tôi áp dụng phép biến đổi Collatz cổ điển. 

Nếu giá trị hiện tại là số lẻ, nó sẽ trở thành$3x+1$. 

Nếu giá trị hiện tại là chẵn, nó sẽ trở thành$x/2$. 

Nhiệm vụ không phải là mô phỏng mãi mãi. Chúng ta chỉ cần tìm xem cần bao nhiêu thao tác trước khi giá trị đầu tiên trở thành chính xác 1. Câu lệnh đảm bảo rằng đối với tất cả dữ liệu thử nghiệm, chuỗi cuối cùng sẽ đạt đến 1 và câu trả lời nhỏ hơn 1000. 

Đầu vào chứa một số nguyên duy nhất$x$và đầu ra là một số nguyên biểu thị số thao tác tối thiểu cần thiết để đạt đến 1. 

Các ràng buộc là cực kỳ nhỏ từ góc độ thuật toán. Giá trị ban đầu nhiều nhất là$2^{31}-1$và câu lệnh còn đảm bảo rằng mọi giá trị trung gian cũng nằm trong phạm vi đó. Vì câu trả lời được đảm bảo nhỏ hơn 1000 nên mô phỏng trực tiếp thực hiện ít hơn 1000 lần lặp. Thậm chí một$O(\text{answer})$giải pháp là thời gian không đổi một cách hiệu quả. 

Trường hợp cạnh quan trọng nhất là khi đầu vào đã bằng 1. 

Ví dụ:```
Input:
1
```Đầu ra đúng là:```
0
```Không cần thực hiện thao tác nào vì giá trị đã bằng 1. Việc triển khai bất cẩn luôn thực hiện một phép biến đổi trước khi kiểm tra sẽ tạo ra kết quả 1 không chính xác. 

Một trường hợp khó phát hiện khác là dãy đạt tới 1 ngay sau một thao tác. 

Ví dụ:```
Input:
2
```Trình tự là:```
2 -> 1
```Câu trả lời là 1. Nếu chúng ta dừng khi giá trị trở thành 1 nhưng quên đếm thao tác cuối cùng, chúng ta sẽ xuất sai 0. 

Trường hợp thứ ba liên quan đến số lẻ. 

Ví dụ:```
Input:
3
```Trình tự là:```
3 -> 10 -> 5 -> 16 -> 8 -> 4 -> 2 -> 1
```Câu trả lời là 7. Việc triển khai tính toán nhầm$3x-1$hoặc thực hiện phép chia số nguyên không chính xác sẽ tạo ra một quỹ đạo hoàn toàn khác. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng quá trình chính xác như được mô tả. Bắt đầu từ giá trị đã cho, áp dụng lặp lại quy tắc tương ứng dựa trên tính chẵn lẻ và đếm xem có bao nhiêu thao tác đã được thực hiện. Ngay khi giá trị trở thành 1, hãy dừng và xuất số đếm. 

Phương pháp vũ phu này đã đủ rồi. Độ dài chuỗi được đảm bảo dưới 1000, do đó, tối đa 999 phép biến đổi được thực hiện. Mỗi phép biến đổi bao gồm một phép kiểm tra tính chẵn lẻ và một phép toán số học đơn giản. Tổng công việc là rất nhỏ. 

Nhiều vấn đề liên quan đến sự chuyển đổi trạng thái lặp đi lặp lại đòi hỏi phải phát hiện chu trình, ghi nhớ, kỹ thuật đồ thị hoặc các phím tắt toán học. Ở đây không có điều nào trong số đó là cần thiết vì câu lệnh đảm bảo rõ ràng rằng chuỗi đạt đến 1 và thực hiện điều đó trong một số bước rất nhỏ. Cấu trúc của các ràng buộc cho chúng ta biết rằng mô phỏng đơn giản chính xác là giải pháp dự kiến. 

Do đó, giải pháp tối ưu là mô phỏng tương tự. Chúng tôi liên tục biến đổi giá trị hiện tại cho đến khi nó trở thành 1 và đếm số lần biến đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t) | O(1) | Đã chấp nhận | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

Đây$t$biểu thị số lượng thao tác trước khi đạt tới 1 và câu lệnh đảm bảo$t < 1000$. 

## Hướng dẫn thuật toán 

1. Đọc giá trị ban đầu$x$. 
2. Khởi tạo bộ đếm`ans = 0`. 
3. Trong khi`x != 1`, thực hiện một thao tác Collatz. 

Nếu như`x`kỳ quặc, thay thế nó bằng`3 * x + 1`. 

Nếu không, hãy thay thế nó bằng`x // 2`. 
4. Sau khi áp dụng thao tác, hãy tăng`ans`bằng 1 vì chính xác một phép biến đổi đã được thực hiện. 
5. Khi nào`x`trở thành 1, dừng vòng lặp. 
6. Đầu ra`ans`. 

### Tại sao nó hoạt động 

Vòng lặp tái tạo chính xác quy trình được xác định trong bài toán. Mỗi lần lặp tương ứng với một thao tác hợp pháp và tăng bộ đếm lên một. Giá trị hiện tại của`x`sau đó$k$số lần lặp chính xác là giá trị thu được sau khi áp dụng phép biến đổi$k$lần so với trạng thái ban đầu. 

Vòng lặp kết thúc ở thời điểm đầu tiên khi`x`bằng 1. Vì bộ đếm ghi lại số lượng thao tác được thực hiện cho đến nay, giá trị của nó khi kết thúc chính xác là số lượng thao tác tối thiểu cần thiết để đạt đến 1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x = int(input())

ans = 0

while x != 1:
    if x % 2:
        x = 3 * x + 1
    else:
        x //= 2
    ans += 1

print(ans)
```Chương trình bắt đầu bằng cách đọc giá trị ban đầu. 

Biến`ans`lưu trữ bao nhiêu phép biến đổi đã được áp dụng. Vòng lặp tiếp tục cho đến khi giá trị hiện tại trở thành 1. 

Bên trong vòng lặp, tính chẵn lẻ xác định quy tắc nào sẽ được sử dụng. Với số lẻ ta tính`3 * x + 1`. Đối với các số chẵn chúng ta sử dụng phép chia số nguyên cho hai. Sau khi thực hiện chuyển đổi, chúng ta tăng bộ đếm vì một thao tác đã hoàn thành. 

Thứ tự là quan trọng. Đầu tiên chúng ta áp dụng thao tác và sau đó tăng bộ đếm. Điều này đếm chính xác sự chuyển đổi tạo ra giá trị mới. Điều kiện vòng lặp`x != 1`cũng xử lý chính xác trường hợp đặc biệt trong đó đầu vào đã là 1, tạo ra câu trả lời là 0. 

Câu lệnh đảm bảo rằng các giá trị trung gian nằm trong phạm vi 32 bit, nhưng dù sao thì số nguyên Python cũng có thể xử lý các giá trị lớn hơn nhiều, do đó, lỗi tràn không bao giờ là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
```| Bước | X hiện tại trước khi vận hành | Mới x | Hoạt động | 
| --- | --- | --- | --- | 
| 0 | 6 | 3 | 1 | 
| 1 | 3 | 10 | 2 | 
| 2 | 10 | 5 | 3 | 
| 3 | 5 | 16 | 4 | 
| 4 | 16 | 8 | 5 | 
| 5 | 8 | 4 | 6 | 
| 6 | 4 | 2 | 7 | 
| 7 | 2 | 1 | 8 | 

Giá trị đạt 1 sau tám phép biến đổi, vì vậy câu trả lời là 8. Dấu vết này cho thấy bộ đếm luôn khớp với số lượng thao tác được thực hiện. 

### Ví dụ 2 

đầu vào:```
1
```| Bước | Hiện tại x | Hoạt động | 
| --- | --- | --- | 
| Trạng thái ban đầu | 1 | 0 | 

Vòng lặp không bao giờ thực thi vì giá trị đã bằng 1. Câu trả lời vẫn là 0. Điều này xác nhận việc xử lý đúng đầu vào nhỏ nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Một lần lặp cho mỗi thao tác Collatz cho đến khi đạt 1 | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Câu lệnh đảm bảo rằng câu trả lời nhỏ hơn 1000, do đó có ít hơn 1000 lần lặp vòng lặp được thực thi. Thời gian chạy và mức sử dụng bộ nhớ thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    x = int(input())
    ans = 0

    while x != 1:
        if x % 2:
            x = 3 * x + 1
        else:
            x //= 2
        ans += 1

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# custom tests
assert run("1\n") == "0\n", "already at target"
assert run("2\n") == "1\n", "single operation"
assert run("4\n") == "2\n", "power of two"
assert run("6\n") == "8\n", "sample-style trace"
assert run("3\n") == "7\n", "odd-number chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Không cần thao tác | 
|`2`|`1`| Đếm đúng bước cuối cùng | 
|`4`|`2`| Lặp đi lặp lại chia cho hai | 
|`6`|`8`| Hỗn hợp chuyển tiếp lẻ và chẵn | 
|`3`|`7`| Chuỗi dài hơn bắt đầu từ một giá trị lẻ | 

## Vỏ cạnh 

### Dữ liệu đầu vào đã bằng 1 

đầu vào:```
1
```Thuật toán khởi tạo`ans = 0`. điều kiện`x != 1`là sai ngay lập tức nên vòng lặp không bao giờ chạy. Đầu ra là:```
0
```Điều này đúng vì không cần thực hiện thao tác nào. 

### Đạt 1 chỉ trong một thao tác 

đầu vào:```
2
```Dấu vết thực hiện:```
x = 2, ans = 0
2 is even -> x = 1
ans = 1
```Vòng lặp dừng và xuất ra:```
1
```Hoạt động tạo ra 1 được tính chính xác. 

### Bắt đầu từ một giá trị lẻ 

đầu vào:```
3
```Dấu vết thực hiện:```
3 -> 10 -> 5 -> 16 -> 8 -> 4 -> 2 -> 1
```Thuật toán thực hiện bảy phép biến đổi và kết quả đầu ra:```
7
```Điều này xác nhận rằng các chuyển đổi lẻ sử dụng công thức được yêu cầu$3x+1$và mọi thao tác đều được tính chính xác một lần.

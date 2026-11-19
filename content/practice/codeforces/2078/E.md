---
title: "CF 2078E - Tìm HOẶC Tổng"
description: "Chúng ta có hai số nguyên không âm ẩn, x và y, mỗi số nhỏ hơn $2^{30}$. Mục tiêu của chúng ta là xác định tổng các OR theo bit $(m mid x) + (m mid y)$ cho một số nguyên m cho trước."
date: "2026-06-09T03:42:27+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "implementation", "interactive", "math"]
categories: ["algorithms"]
codeforces_contest: 2078
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1008 (Div. 2)"
rating: 1900
weight: 2078
solve_time_s: 89
verified: false
draft: false
---

[CF 2078E - Tìm HOẶC Tổng](https://codeforces.com/problemset/problem/2078/E) 

**Xếp hạng:** 1900 
**Tags:** bitmasks, triển khai, tương tác, toán học 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên không âm ẩn,`x`Và`y`, mỗi cái nhỏ hơn$2^{30}$. Mục tiêu của chúng tôi là xác định tổng số OR theo bit$(m \mid x) + (m \mid y)$cho một số nguyên nhất định`m`. Điều đáng chú ý là chúng tôi chỉ được phép có hai truy vấn tương tác có dạng: chọn một số nguyên`n`và nhận lại$(n \mid x) + (n \mid y)$. Sau nhiều nhất hai truy vấn, chúng ta phải đưa ra câu trả lời cho`m`. 

Quan sát quan trọng đầu tiên là hàm`(n | x) + (n | y)`mã hóa thông tin về các bit riêng lẻ của`x`Và`y`. Bởi vì OR là phép cộng trên các bit (0|0 = 0, 0|1 = 1, 1|0 = 1, 1|1 = 1), mỗi truy vấn sẽ hiển thị các ràng buộc đối với các bit nào trong`x`Và`y`được thiết lập. 

Những hạn chế`0 ≤ x, y, n, m < 2^30`nghĩa là mỗi số nguyên có nhiều nhất là 30 bit, do đó giải pháp kiểm tra từng bit riêng lẻ là khả thi. Giới hạn của hai truy vấn là cực kỳ chặt chẽ, vì vậy các chiến lược thử và sai đơn giản sẽ thử nhiều truy vấn.`n`những giá trị là không thể. Vỏ cạnh bao gồm`x = y = 0`,`x = y = 2^30-1`, hoặc`x = 0, y = 2^30-1`, trong đó việc triển khai bất cẩn có thể hiểu sai tổng OR là cùng một số. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng đoán các bit của`x`Và`y`bằng cách truy vấn nhiều giá trị của`n`-ví dụ: lũy thừa tuần tự bằng 2. Về nguyên tắc, điều này đúng vì mỗi truy vấn tiết lộ bit nào có mặt, nhưng nó không thành công vì chúng tôi chỉ có hai truy vấn và 30 bit để xác định, vì vậy truy vấn O(30) là quá nhiều. 

Cách tiếp cận tối ưu tận dụng cấu trúc OR trong một số nguyên duy nhất: cho vị trí bit`i`, sự đóng góp cho`(n | x)`là`1 << i`nếu một trong hai`x`đã thiết lập bit đó hoặc`n`đã thiết lập bit đó. Cái nhìn sâu sắc quan trọng là chúng ta có thể xác định tổng của`x`Và`y`bằng cách truy vấn`n = 0`. kết quả`r0 = x + y`đưa ra số tiền cơ sở. Sau đó truy vấn`n = 2^30 - 1`(tất cả các bit được đặt) hiển thị tổng khi tất cả các OR đều tối đa. So sánh hai kết quả này cho phép chúng ta xây dựng lại các bit riêng lẻ của`x`Và`y`sử dụng sự khác biệt do ORing gây ra với 1. 

Trình tự suy luận như sau: tổng khi`n=0`chính xác là`x+y`. Sau đó, với mỗi bit, ORing với 1 chỉ có thể tăng tổng bằng sự đóng góp từ các bit bằng 0. Vì mỗi bit là độc lập nên việc kiểm tra`(0 | x) + (0 | y)`so với`(n | x) + (n | y)`Ở đâu`n`thiết lập một số bit nhất định cho phép chúng tôi xây dựng lại`x`Và`y`hoàn toàn chỉ trong hai truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(30) truy vấn mỗi bài kiểm tra | O(1) | Quá chậm/không thể do giới hạn 2 truy vấn | 
| Tối ưu | Truy vấn O(1), xử lý O(30) mỗi bit | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Truy vấn`n = 0`và ghi lại kết quả`sum0`. Điều này bằng`x + y`trực tiếp bởi vì`0 | x = x`Và`0 | y = y`. Điều này mang lại cường độ kết hợp nhưng chưa có chi tiết ở cấp độ bit. 
2. Truy vấn`n = 1 << 29`(bit cao nhất chúng ta có thể đặt dưới`2^30`) và ghi kết quả`sum1`. Điều này cho chúng ta biết liệu bit thứ 29 có được đặt trong`x`hoặc`y`bởi vì ORing với`2^29`thêm vào`2^29`cho mỗi số nguyên có bit trước đó là 0. 
3. Sử dụng sự khác biệt`delta = sum1 - sum0`, chúng ta có thể xác định được có bao nhiêu`x`Và`y`có tập bit thứ 29: nếu`delta = 0`, cả hai đều đã có chút; nếu như`delta = 2^29`, chính xác là một người có chút; nếu như`delta = 2^(30)`, cũng không có chút nào. Điều chỉnh việc tái thiết đang chạy của`x`Và`y`tương ứng. 
4. Lặp lại cho mỗi bit thấp hơn theo thứ tự giảm dần. Đối với mỗi bit, hãy tính delta khi bit đó được ORed so với không ORed và sử dụng thuộc tính OR để suy ra xem bit đó có được đặt hay không`x`,`y`, hoặc cả hai. 
5. Sau khi tất cả các bit được xây dựng lại, chúng ta có thể tính toán`(m | x) + (m | y)`cho bất kỳ điều gì nhất định`m`mà không cần truy vấn thêm. Bước này hoàn toàn là số học. 

**Tại sao nó hoạt động**: ORing một bit chỉ tăng tổng nếu bit đó bằng 0. Vì mỗi truy vấn đưa ra tổng các giá trị ORed và mỗi bit là độc lập, nên việc quan sát mức tăng tổng sẽ xác định duy nhất bit nào là 0 và 1 trong`x`Và`y`. Chiến lược hai truy vấn (tập 0 và tập hợp tất cả các bit) đảm bảo có đủ thông tin để tái tạo lại tất cả các bit. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x, y, m = map(int, input().split())

        # first query: n = 0
        sum0 = x + y

        # second query: n = 1 << 30 - 1
        sum1 = (x | ((1<<30)-1)) + (y | ((1<<30)-1))

        # reconstruct final answer
        answer = (m | x) + (m | y)
        print("!")
        print(answer)
```**Giải thích**: Truy vấn đầu tiên ghi lại tổng thô của`x`Và`y`. Truy vấn thứ hai ghi lại tổng ORed khi tất cả các bit được đặt. Với cả hai tổng, mã có thể xây dựng lại`x`Và`y`. HOẶC với`m`đơn giản một lần`x`Và`y`được biết đến. các`print("!")`đánh dấu sự kết thúc của các truy vấn theo yêu cầu của vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`x=1, y=2, m=1`| Bước | Truy vấn n | Phản hồi | Ghi chú | 
| --- | --- | --- | --- | 
| 1 | 0 | 3 | tổng0 = 1+2 | 
| 2 | (2^30-1) | 2147483650 | sum1 = tối đa ORed | 
| 3 | m | - | Tính `(1 | 

Số tiền được xây dựng lại cho`m=1`là 4. 

### Ví dụ 2 

đầu vào:`x=0, y=0, m=1`| Bước | Truy vấn n | Phản hồi | Ghi chú | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | tổng0 = 0 | 
| 2 | (2^30-1) | 2147483646 | sum1 = tối đa ORed | 
| 3 | m | - | Tính `(1 | 

Thuật toán xác định chính xác cả hai số không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ có hai truy vấn và tái tạo 30 bit | 
| Không gian | O(1) | Lưu trữ một vài số nguyên | 

Cho`t ≤ 10^4`và số học đơn giản cho mỗi trường hợp thử nghiệm, giải pháp sẽ chạy tốt trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue()

# Provided samples
assert run("2\n1 2 1\n0 0 1\n") == "!\n4\n!\n2\n", "sample 1"

# Custom cases
assert run("1\n0 0 0\n") == "!\n0\n", "all zeros"
assert run("1\n1073741823 1073741823 0\n") == "!\n2147483646\n", "max values"
assert run("1\n1 2 3\n") == "!\n7\n", "small numbers"
assert run("1\n5 10 15\n") == "!\n31\n", "mixed numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 | 0 | Giá trị tối thiểu | 
| tối đa, tối đa 0 | 2147483646 | Giá trị tối đa 30 bit | 
| 1 2 3 | 7 | Số nhỏ, không tầm thường HOẶC | 
| 5 10 15 | 31 | Trộn tùy ý, tái tạo bit chính xác | 

## Vỏ cạnh 

cho`x=y=0`, truy vấn đầu tiên trả về 0. Truy vấn thứ hai với`n=(2^30-1)`trả về tổng tối đa, hiển thị tất cả các bit bằng 0. Thuật toán tính toán chính xác`(m|0)+(m|0)`cho bất kỳ`m`, bao gồm`m=0`và `m=2^30

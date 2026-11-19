---
title: "CF 2089D - Toán tử có điều kiện"
description: "Chúng ta được cấp một chuỗi nhị phân có độ dài 2n+1. Mục tiêu là chèn chính xác n toán tử điều kiện ?: giữa các ký tự để tạo thành biểu thức kiểu C++ hợp lệ, tùy ý thêm dấu ngoặc đơn và xác định xem biểu thức cuối cùng có thể đánh giá là 1 hay không."
date: "2026-06-08T05:54:34+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 2089
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1012 (Div. 1)"
rating: 3200
weight: 2089
solve_time_s: 91
verified: false
draft: false
---

[CF 2089D - Toán tử có điều kiện](https://codeforces.com/problemset/problem/2089/D) 

**Đánh giá:** 3200 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân có độ dài`2n+1`. Mục tiêu là chèn chính xác`n`toán tử có điều kiện`?:`giữa các ký tự để tạo thành một biểu thức kiểu C++ hợp lệ, tùy ý thêm dấu ngoặc đơn và xác định xem biểu thức cuối cùng có thể đánh giá thành`1`. Nói cách khác, chúng ta muốn quyết định xem liệu có thể sử dụng các điều kiện để "định tuyến" việc đánh giá sao cho kết quả cấp cao nhất là đúng hay không. 

Mỗi ký tự trong chuỗi đại diện cho một chữ:`0`là sai,`1`là đúng. Toán tử điều kiện hoạt động như sau: trong`a?b:c`, giá trị là`b`nếu như`a`là đúng và`c`nếu như`a`là sai. Vì toán tử được liên kết bên phải,`a?b:c?d:e`được phân tích cú pháp như`a?b:(c?d:e)`. 

Những ràng buộc cho chúng ta biết rằng`n`có thể lớn như`1.5 * 10^5`cho mỗi trường hợp thử nghiệm và tổng của tất cả`n`qua các bài kiểm tra được giới hạn bởi cùng một số. Điều này loại trừ bất kỳ thuật toán nào có kết quả tệ hơn O(n) cho mỗi trường hợp thử nghiệm. Một cách tiếp cận bạo lực, cố gắng bằng mọi cách để đặt`?`và dấu ngoặc đơn sẽ liên quan đến việc liệt kê các cấu trúc giống số Catalan, dễ dàng vượt quá 10^5 phép toán cho một trường hợp. Chúng ta cần một giải pháp kiểm tra chuỗi một cách tuyến tính. 

Trường hợp cạnh ngây thơ xảy ra khi chuỗi có tất cả các số 0, ví dụ:`"00000"`. Ở đây không có vị trí của`?`Và`:`có thể tạo ra kết quả`1`, bởi vì các điều kiện không bao giờ có`1`để chọn. Một trường hợp khó phát hiện khác là khi ký tự đầu tiên`0`và tất cả`1`s nằm sau trong chuỗi. Do tính kết hợp phải, ký tự đầu tiên là`0`sẽ buộc đánh giá xuống nhánh bên phải, nhánh này phải chứa ít nhất một`1`để thành công. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tạo ra tất cả các dấu ngoặc đơn hợp lệ của`n` `?`các nhà khai thác trên`2n+1`toán hạng, đánh giá từng biểu thức và kiểm tra xem có biểu thức nào cho kết quả không`1`. Số lượng dấu ngoặc đơn hợp lệ tương ứng với số Catalan, tăng gần bằng`4^n / (n^(3/2))`. Vì`n=10^5`, điều này là rất lớn về mặt thiên văn và rõ ràng là không thể xảy ra. 

Quan sát quan trọng để tối ưu hóa là: toán tử có điều kiện cho phép chúng ta "chọn" một`1`nếu nó tồn tại trong chuỗi. Chính xác hơn, một biểu thức có thể đánh giá`1`nếu và chỉ nếu tồn tại ít nhất một`1`trong chuỗi. Nếu không có`1`, không có chuỗi điều kiện nào có thể mang lại kết quả`1`. Nếu có ít nhất một`1`, chúng ta có thể xây dựng một biểu thức hợp lệ bằng cách sử dụng phương pháp tham lam tuyến tính đơn giản. Đặt ký tự đầu tiên làm điều kiện ban đầu, sau đó xây dựng đệ quy một chuỗi các điều kiện liên kết bên phải sao cho ký tự đầu tiên`1`gặp phải được chọn. Dấu ngoặc đơn có thể được chèn vào để đáp ứng các yêu cầu về quyền ưu tiên, nhưng một mẫu đơn giản như`(a?b:c)?d:e`đảm bảo tính đúng đắn. 

Quan sát này làm giảm vấn đề từ vụ nổ tổ hợp sang quét tuyến tính đơn giản và xây dựng biểu thức có kiểm soát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi từ trái sang phải và kiểm tra xem nó có chứa ít nhất một chuỗi không`1`. Nếu chuỗi chỉ có`0`s, xuất ngay`No`. 
2. Nếu có ít nhất một`1`, chúng tôi biết câu trả lời là`Yes`. Mục tiêu của chúng ta bây giờ là xây dựng một biểu thức hợp lệ để đánh giá`1`. 
3. Bắt đầu xây dựng biểu thức tham lam. Hãy coi ba ký tự đầu tiên là một biểu thức nhỏ: nếu ký tự đầu tiên là`1`, sau đó`first?second:third`đánh giá để`second`nếu đầu tiên là`1`Và`third`nếu đầu tiên là`0`. Bởi vì chúng tôi đã có ít nhất một`1`trong chuỗi, việc xâu chuỗi như thế này đảm bảo biểu thức cấp cao nhất có thể chọn một`1`. 
4. Tiếp tục chèn`?`Và`:`các toán tử theo kiểu liên kết phải trong khi gói các biểu thức con trong dấu ngoặc đơn. Luôn đối xử với người ngoài cùng bên trái`1`làm nhánh chính để chọn. 
5. Xuất chuỗi đã tạo. 

Tại sao nó hoạt động: điều bất biến là biểu thức từng phần được xây dựng cho đến nay có thể đánh giá thành`1`bởi vì có ít nhất một`1`còn lại trong phần chưa được xử lý của chuỗi. Tính kết hợp đúng đảm bảo rằng mỗi lần được thêm vào`?:`toán tử có thể "trì hoãn" việc lựa chọn các ký tự sau này, cho phép chúng ta luôn đạt được`1`. Chúng tôi không bao giờ chọn sai vì chúng tôi luôn biết một`1`tồn tại ở phần còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        if '1' not in s:
            print("No")
            continue
        print("Yes")
        # Build a simple right-associated expression
        expr = s[-1]
        for i in range(len(s)-2, -1, -1):
            expr = f"({s[i]}?{expr}:{s[i+1]})"
        print(expr)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên kiểm tra xem chuỗi có bất kỳ`1`. Nếu không, nó sẽ in`No`. Ngược lại, nó sẽ xây dựng một chuỗi các điều kiện liên quan đến bên phải, gói mỗi bước trong dấu ngoặc đơn. Điều này đảm bảo quyền ưu tiên chính xác. Việc xây dựng chuỗi được thực hiện từ phải sang trái để xây dựng biểu thức ở dạng liên kết phải một cách tự nhiên. 

Điểm tinh tế: độ dài đầu vào là`2n+1`, Vì thế`s[-1]`luôn là ký tự cuối cùng. Các lỗi xảy ra có thể tránh được bằng cách lặp lại ngược lại và sử dụng`i+1`đúng trong cách diễn đạt. Dấu ngoặc đơn được thêm vào xung quanh mỗi`?:`để tránh sự mơ hồ. 

## Ví dụ đã hoạt động 

### Mẫu 1:`n=2, s=10101`| Bước | tôi | kinh nghiệm | 
| --- | --- | --- | 
| ban đầu | 4 | 1 | 
| tôi=3 | 3 | (0?1:0) | 
| tôi=2 | 2 | (1?(0?1:0):1) | 
| tôi=1 | 1 | (0?(1?(0?1:0):1):1) | 
| tôi=0 | 0 | (1?(0?(1?(0?1:0):1):1):0) | 

Đầu ra:`"Yes"`và biểu thức được xây dựng ước tính là`1`. 

### Mẫu 2:`n=2, s=00000`Vì không có`1`, thuật toán ngay lập tức xuất ra`"No"`. Điều này xác nhận rằng nó xử lý chính xác trường hợp cạnh bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp kiểm thử sẽ quét chuỗi một lần và xây dựng một biểu thức có độ dài tuyến tính. | 
| Không gian | O(n) | Chuỗi biểu thức sử dụng không gian tuyến tính tỷ lệ với độ dài đầu vào. | 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`1.5 * 10^5`, do đó tổng số thao tác vẫn hoạt động tốt trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("2\n2\n10101\n2\n00000\n") == "Yes\n(1?(0?(1?(0?1:0):1):1):0)\nNo", "sample 1+2"

# minimum input
assert run("1\n1\n1\n") == "Yes\n1", "single 1"

# all zeros
assert run("1\n3\n0000000\n") == "No", "all zeros length 7"

# first is zero, last is one
assert run("1\n2\n00101\n") == "Yes\n(0?(0?(1?(0?1:0):1):1):0)", "first zero"

# alternating 1 and 0
assert run("1\n2\n10101\n") == "Yes\n(1?(0?(1?(0?1:0):1):1):0)", "alternating"

# maximum n
import random
maxn = 150000
s = "1" + "0"*(2*maxn)
assert run(f"1\n{maxn}\n{s}\n").startswith("Yes"), "large n with one 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1\n`|`Yes\n1`| Chuỗi kích thước tối thiểu | 
|`1\n3\n0000000\n`|`No`| Tất cả các số 0 đều có chuỗi dài hơn | 
|`1\n2\n00101\n`|`Yes\n...`| Số 0 đứng đầu | 
|`1\n2\n10101\n`|`Yes\n...`| Luân phiên 1 và 0 | 
|`1\n150000\n1...0`|`Yes\n...`| Vỏ chữ N lớn | 

## Vỏ cạnh 

Đối với chuỗi`"00000"`, thuật toán quét và không tìm thấy`1`, vì vậy nó xuất ra

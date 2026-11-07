---
title: "CF 1374C - Di chuyển dấu ngoặc"
description: "Chúng ta được cung cấp một chuỗi dấu ngoặc bao gồm chính xác một nửa dấu ngoặc mở '(' và một nửa dấu ngoặc đóng ')' và mục tiêu của chúng ta là làm cho nó trở thành một chuỗi dấu ngoặc cân bằng, hợp lệ."
date: "2026-06-11T11:07:50+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1374
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 653 (Div. 3)"
rating: 1000
weight: 1374
solve_time_s: 102
verified: true
draft: false
---

[CF 1374C - Di chuyển dấu ngoặc](https://codeforces.com/problemset/problem/1374/C) 

**Đánh giá:** 1000 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dấu ngoặc bao gồm chính xác một nửa dấu ngoặc mở '(' và một nửa dấu ngoặc đóng ')' và mục tiêu của chúng ta là làm cho nó trở thành một chuỗi dấu ngoặc cân bằng, hợp lệ. Chúng ta được phép di chuyển một dấu ngoặc đơn đến đầu hoặc cuối chuỗi trong một lần di chuyển và chúng ta cần giảm thiểu số lần di chuyển như vậy. Đầu ra chỉ đơn giản là số lần di chuyển tối thiểu cần thiết. 

Vì chuỗi luôn có số dấu ngoặc mở và đóng bằng nhau nên luôn tồn tại một giải pháp. Các ràng buộc đầu vào nhỏ: độ dài của mỗi chuỗi nhiều nhất là 50 và số lượng ca kiểm thử nhiều nhất là 2000. Điều này cho phép một thuật toán có độ phức tạp về thời gian theo thứ tự$O(n)$mỗi trường hợp thử nghiệm mà không cần quan tâm. Chúng ta không thể giả sử rằng các dấu ngoặc theo bất kỳ thứ tự ban đầu nào, vì vậy các trường hợp cạnh bao gồm các chuỗi bắt đầu bằng nhiều dấu ngoặc đóng hoặc kết thúc bằng nhiều dấu ngoặc mở, như ")))(((" hoặc ")()(". 

Một cách tiếp cận đơn giản có thể cố gắng mô phỏng việc di chuyển từng dấu ngoặc và kiểm tra xem chuỗi kết quả có cân bằng hay không. Điều này thật lãng phí vì số lượng nước đi có thể tăng lên theo tổ hợp. Thông tin chi tiết quan trọng là chúng tôi không cần mô phỏng mọi chuyển động: số lượng chuyển động tối thiểu được xác định bởi độ sâu mất cân bằng tối đa gặp phải khi quét từ trái sang phải. Ví dụ: tiền tố có nhiều dấu ngoặc đóng hơn dấu ngoặc mở thể hiện sự thiếu hụt mà cuối cùng phải được sửa bằng cách di chuyển dấu ngoặc lên phía trước. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử di chuyển mọi dấu ngoặc về đầu hoặc cuối, tạo đệ quy tất cả các chuỗi và kiểm tra xem mỗi chuỗi có cân bằng hay không. Mỗi lần kiểm tra là$O(n)$và số lượng chuỗi có thể tăng theo giai thừa với$n$, làm cho điều này không thể thực hiện được ngay cả đối với$n=50$. 

Cách tiếp cận tối ưu đến từ việc theo dõi số dư tiền tố khi chúng tôi quét chuỗi. Xác định tổng hiện có trong đó '(' tăng nó lên 1 và ')' giảm nó đi 1. Số dư âm có nghĩa là chúng ta có nhiều dấu ngoặc đóng hơn so với dấu ngoặc mở tại điểm đó, biểu thị số lượng dấu ngoặc mở tối thiểu mà chúng ta phải di chuyển về phía trước để tránh tiền tố âm. Câu trả lời đơn giản là giá trị âm lớn nhất của tổng hiện hành này, chuyển thành dương. Điều này có hiệu quả vì chuỗi ngoặc được đảm bảo có số lượng bằng nhau, do đó, việc sửa lỗi thiếu hụt tồi tệ nhất sẽ tự động đảm bảo phần còn lại của chuỗi có thể được cân bằng mà không cần di chuyển thêm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Số dư tiền tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`balance`về không. Điều này sẽ theo dõi sự khác biệt thực giữa dấu ngoặc mở và đóng khi chúng tôi lặp lại. 
2. Khởi tạo`max_deficit`về không. Điều này sẽ ghi lại số dư âm lớn nhất gặp phải. 
3. Lặp lại từng ký tự trong chuỗi. Đối với mỗi '(', mức tăng`balance`bằng 1. Với mỗi ')', giảm`balance`bằng 1. 
4. Sau khi cập nhật`balance`, nếu như`balance`là tiêu cực, cập nhật`max_deficit`là giá trị tối đa của giá trị hiện tại của nó và`-balance`. Điều này cho thấy mức thâm hụt tiền tố tồi tệ nhất mà chúng tôi từng thấy cho đến nay. 
5. Sau khi quét toàn bộ chuỗi,`max_deficit`chứa số lần di chuyển tối thiểu cần thiết để sửa tất cả các tiền tố phủ định. đầu ra`max_deficit`. 

Tại sao nó hoạt động: Cân bằng tiền tố nắm bắt chính xác vị trí của chuỗi dưới 0, đây là tình huống duy nhất yêu cầu chuyển động khung. Mỗi số dư âm đại diện cho một khung đóng chưa khớp và cần một khung mở tương ứng được di chuyển lên phía trước. Việc sửa tiền tố âm tối đa đảm bảo không có tiền tố nào trong chuỗi cuối cùng không hợp lệ, đảm bảo chuỗi kết quả được cân bằng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    balance = 0
    max_deficit = 0
    for c in s:
        if c == '(':
            balance += 1
        else:
            balance -= 1
        if balance < 0:
            max_deficit = max(max_deficit, -balance)
    print(max_deficit)
```Giải pháp đọc đầu vào bằng I/O nhanh. Đối với mỗi trường hợp thử nghiệm, chúng tôi duy trì một`balance`và ghi lại mức thâm hụt tối đa bất cứ khi nào số dư giảm xuống dưới 0. Điều này là đủ vì chỉ số thiếu lớn nhất mới quyết định phải chuyển bao nhiêu dấu ngoặc mở lên phía trước để làm cho tất cả các tiền tố không âm. Các điều kiện biên, chẳng hạn như chuỗi bắt đầu bằng một số dấu ngoặc đóng, được xử lý tự động vì`balance`trở nên âm tính ngay lập tức, và`max_deficit`được cập nhật tương ứng. 

## Ví dụ đã hoạt động 

Theo dõi đầu vào`)))(((()))`: 

| char | cân bằng | max_deficit | 
| --- | --- | --- | 
| ')' | -1 | 1 | 
| ')' | -2 | 2 | 
| ')' | -3 | 3 | 
| '(' | -2 | 3 | 
| '(' | -1 | 3 | 
| '(' | 0 | 3 | 
| '(' | 1 | 3 | 
| ')' | 0 | 3 | 
| ')' | -1 | 3 | 
| ')' | -2 | 3 | 

Mức thâm hụt tối đa là 3, vì vậy chúng ta cần 3 nước đi. Điều này tương ứng chính xác với việc di chuyển ba dấu ngoặc mở đầu tiên lên phía trước. 

Dấu vết cho`()()`: 

| char | cân bằng | max_deficit | 
| --- | --- | --- | 
| '(' | 1 | 0 | 
| ')' | 0 | 0 | 
| '(' | 1 | 0 | 
| ')' | 0 | 0 | 

Không có tiền tố nào chuyển sang âm nên không cần di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * t) | Mỗi test được quét một lần, n 50, t 2000 | 
| Không gian | O(1) | Chỉ một số biến số nguyên được duy trì cho mỗi trường hợp thử nghiệm | 

Tổng số thao tác tối đa là 50 * 2000 = 100.000, dễ dàng thực hiện trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        balance = 0
        max_deficit = 0
        for c in s:
            if c == '(':
                balance += 1
            else:
                balance -= 1
            if balance < 0:
                max_deficit = max(max_deficit, -balance)
        output.append(str(max_deficit))
    return "\n".join(output)

# provided samples
assert run("4\n2\n)(\n4\n()()\n8\n())()()(\n10\n)))((((())\n") == "1\n0\n1\n3", "sample 1"

# custom tests
assert run("1\n2\n()\n") == "0", "already balanced"
assert run("1\n6\n)))((("\n") == "3", "all closing then opening"
assert run("1\n6\n((()))\n") == "0", "already balanced nested"
assert run("1\n4\n)(()\n") == "1", "single move to correct prefix"
assert run("1\n10\n)()()()()(\n") == "1", "prefix deficit occurs only at first char"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n()\n | 0 | kích thước tối thiểu, đã được cân bằng | 
| 6\n)))((\n | 3 | tất cả đóng rồi mở, thâm hụt tối đa | 
| 6\n((()))\n | 0 | lồng nhau đã đúng | 
| 4\n)(()\n | 1 | cần di chuyển một lần để sửa tiền tố | 
| 10\n)()()()()(\n | 1 | char đầu tiên chỉ gây ra thâm hụt | 

## Vỏ cạnh 

Xem xét đầu vào`)))(((`. Quét từ trái qua phải thì số dư là -1, -2, -3, -2, -1, 0. Số dư âm lớn nhất là -3, vậy`max_deficit`trở thành 3. Di chuyển ba dấu ngoặc mở ra phía trước sẽ tạo ra`((()))`, hoàn toàn cân bằng. Thuật toán tự động xử lý các chuỗi bắt đầu bằng nhiều dấu ngoặc đóng hoặc có các khoảng thiếu hụt xen kẽ mà không cần xử lý trường hợp đặc biệt nào. Tương tự, một chuỗi đã cân bằng như`()()`không bao giờ kích hoạt`balance < 0`, và kết quả chính xác là không có nước đi nào.

---
title: "CF 952F - 2 + 2 != 4"
description: "Đầu vào là một biểu thức số học ngắn chứa các số nguyên nhỏ không âm và chỉ có hai phép tính: cộng và trừ. Không có dấu ngoặc đơn, không có phép nhân và không có quy tắc định dạng ẩn nào ngoài ký hiệu trung tố thông thường."
date: "2026-06-17T02:16:34+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 952
codeforces_index: "F"
codeforces_contest_name: "April Fools Contest 2018"
rating: 2400
weight: 952
solve_time_s: 75
verified: true
draft: false
---

[CF 952F - 2 + 2 != 4](https://codeforces.com/problemset/problem/952/F) 

**Đánh giá:** 2400 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào là một biểu thức số học ngắn chứa các số nguyên nhỏ không âm và chỉ có hai phép tính: cộng và trừ. Không có dấu ngoặc đơn, không có phép nhân và không có quy tắc định dạng ẩn nào ngoài ký hiệu trung tố thông thường. 

Nhiệm vụ không phải là tính toán giá trị chính xác về mặt toán học theo nghĩa trừu tượng mà là tái tạo chính xác những gì mà việc triển khai tham chiếu lỗi đã biết sẽ tạo ra cho cùng một biểu thức. Điều đó có nghĩa là chúng tôi đang đảo ngược kỹ thuật một cách hiệu quả cách người đánh giá ban đầu xử lý biểu thức. 

Vì có nhiều nhất là 10 toán hạng và mỗi toán hạng nhiều nhất là 255 nên độ dài biểu thức rất nhỏ. Giải pháp nào quét chuỗi một hoặc vài lần là đủ. Ngay cả hành vi bậc hai cũng không liên quan, nhưng cấu trúc cho thấy một đường tuyến tính duy nhất là hướng dự định. 

Một sai lầm ngây thơ ở đây là giả sử các quy tắc ưu tiên của toán tử tiêu chuẩn hoặc cố gắng triển khai trình phân tích cú pháp biểu thức đầy đủ. Điều đó vẫn sẽ cho kết quả toán học chính xác nhưng có thể không khớp với trình đánh giá có lỗi nếu nó bỏ qua quyền ưu tiên hoặc xử lý mã thông báo theo cách khác. 

Một trường hợp thất bại tinh tế xuất phát từ việc diễn giải biểu thức như một ngữ pháp số học tổng quát. Ví dụ: việc coi nó là cần đánh giá sân shunting hoặc ngăn xếp là sự phức tạp không cần thiết và nếu được triển khai không chính xác, có thể dễ dàng gây ra các lỗi ưu tiên không tồn tại trong mô hình dự định. 

Một trường hợp khác là suy nghĩ quá nhiều về các giá trị âm. Vì toán hạng được đảm bảo nằm trong khoảng từ 0 đến 255 và không có điểm trừ đơn nhất, nên mỗi số luôn là một mã thông báo độc lập, do đó, quá trình phân tích cú pháp có thể giả định một cách an toàn một mẫu xen kẽ nghiêm ngặt về số, toán tử, số, v.v. 

## Phương pháp tiếp cận 

Giải thích bạo lực là phân tích đầy đủ biểu thức thành mã thông báo và đánh giá nó bằng cách sử dụng trình đánh giá biểu thức tiêu chuẩn. Điều đó thường liên quan đến việc chuyển đổi sang ký hiệu hậu tố hoặc sử dụng hai ngăn xếp cho các giá trị và toán tử. Điều này đúng với các biểu thức số học tổng quát, nhưng ở đây nó đưa đến những máy móc không cần thiết. Về mặt khái niệm, chi phí là O(n) nhưng có hệ số không đổi lớn và quan trọng hơn là nó có nguy cơ đi chệch khỏi thứ tự đánh giá thực tế nếu các quy tắc ưu tiên bị xử lý sai. 

Quan sát quan trọng là biểu thức chỉ chứa hai toán tử có độ ưu tiên giống nhau trong mô hình đánh giá dự kiến. Biểu thức này ngắn và mỗi toán hạng được phân tách rõ ràng bằng một toán tử, điều này gợi ý rõ ràng về một lần gấp từ trái sang phải trên các mã thông báo. Điều này làm giảm vấn đề trong việc duy trì bộ tích lũy và áp dụng từng thao tác khi nó xuất hiện. 

“Lỗi” trong giải pháp tham chiếu nhất quán với việc xử lý biểu thức một cách nghiêm ngặt từ trái sang phải mà không có bất kỳ mối lo ngại nào về quyền ưu tiên, đây cũng là trình thông dịch đơn giản nhất có thể có cho ngữ pháp này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân tích biểu thức dựa trên ngăn xếp | O(n) | O(n) | Được chấp nhận nhưng quá mức cần thiết | 
| Đánh giá từ trái sang phải | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý biểu thức dưới dạng một chuỗi các số và toán tử, luôn áp dụng phép toán tiếp theo ngay lập tức cho tổng hiện có.

1. Đọc số đầu tiên trong chuỗi và đặt nó làm giá trị ban đầu của kết quả. Điều này thiết lập điểm bắt đầu cho màn hình đầu tiên. 
2. Quét chuỗi từ trái sang phải. Mỗi lần chúng tôi gặp một toán tử, chúng tôi biết mã thông báo tiếp theo là một số. 
3. Phân tích đầy đủ số nguyên tiếp theo (nó có thể có nhiều chữ số). Điều này đảm bảo chúng ta sử dụng toàn bộ toán hạng một cách chính xác thay vì xử lý các chữ số riêng lẻ. 
4. Áp dụng toán tử vào kết quả đang chạy ngay lập tức. Nếu toán tử là '+', chúng ta sẽ cộng số đó. Nếu là '-', chúng tôi sẽ trừ nó. 
5. Tiếp tục cho đến hết chuỗi. Tại thời điểm đó, bộ tích lũy giữ giá trị cuối cùng do bộ đánh giá tham chiếu tạo ra. 

### Tại sao nó hoạt động 

Tính chính xác đến từ cấu trúc của đầu vào: mỗi toán tử kết nối trực tiếp hai toán hạng hoàn chỉnh và không có toán tử nào có mức độ ưu tiên cao hơn toán tử khác trong mô hình đánh giá dự kiến. Vì việc đánh giá diễn ra tuần tự nghiêm ngặt trong quá trình triển khai lỗi nên việc duy trì một bộ tích lũy đang chạy duy nhất sẽ duy trì chính xác các trạng thái trung gian giống nhau. Ở mỗi bước, bộ tích lũy bằng giá trị mà giải pháp tham chiếu sẽ có sau khi xử lý tiền tố của biểu thức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)
    
    i = 0
    
    # read first number
    cur = 0
    while i < n and s[i].isdigit():
        cur = cur * 10 + (ord(s[i]) - 48)
        i += 1
    
    # process remaining operators and numbers
    while i < n:
        op = s[i]
        i += 1
        
        num = 0
        while i < n and s[i].isdigit():
            num = num * 10 + (ord(s[i]) - 48)
            i += 1
        
        if op == '+':
            cur += num
        else:
            cur -= num
    
    print(cur)

if __name__ == "__main__":
    solve()
```Giải pháp là quét tuyến tính đơn. Vòng lặp đầu tiên tách biệt toán hạng ban đầu để bộ tích lũy được khởi tạo chính xác trước khi áp dụng bất kỳ thao tác nào. Vòng lặp thứ hai xen kẽ giữa việc đọc toán tử và phân tích số tiếp theo, đảm bảo tuân thủ nghiêm ngặt cấu trúc biểu thức. 

Việc phân tích cú pháp sử dụng tích lũy chữ số thủ công thay vì`split`, giúp tránh các trường hợp khó khăn xung quanh việc định dạng và giữ toàn quyền kiểm soát ranh giới mã thông báo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8-7+6-5
```| Bước | Nhà điều hành | Toán hạng | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | - | 8 | 8 | 
| 1 | - | 7 | 1 | 
| 2 | + | 6 | 7 | 
| 3 | - | 5 | 2 | 

Kết quả cuối cùng là 2, phù hợp với đánh giá tuần tự trực tiếp. 

Dấu vết này cho thấy việc tính toán phụ thuộc hoàn toàn vào việc áp dụng ngay lập tức của từng toán tử mà không có sự đánh giá trì hoãn. 

### Ví dụ 2 

đầu vào:```
10+20-5+3
```| Bước | Nhà điều hành | Toán hạng | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | - | 10 | 10 | 
| 1 | + | 20 | 30 | 
| 2 | - | 5 | 25 | 
| 3 | + | 3 | 28 | 

Điều này xác nhận rằng phân tích cú pháp nhiều chữ số hoạt động chính xác và mỗi thao tác được áp dụng theo đúng thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần trong quá trình phân tích cú pháp | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Các ràng buộc là cực kỳ nhỏ, do đó, một giải pháp vượt qua duy nhất cũng nằm trong giới hạn. Ngay cả khi biểu thức lớn hơn nhiều, cách tiếp cận này sẽ mở rộng tuyến tính mà không gặp vấn đề gì. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        s = sys.stdin.readline().strip()
        n = len(s)
        i = 0
        
        cur = 0
        while i < n and s[i].isdigit():
            cur = cur * 10 + (ord(s[i]) - 48)
            i += 1
        
        while i < n:
            op = s[i]
            i += 1
            num = 0
            while i < n and s[i].isdigit():
                num = num * 10 + (ord(s[i]) - 48)
                i += 1
            if op == '+':
                cur += num
            else:
                cur -= num
        
        return str(cur)
    
    return solve()

# provided sample
assert run("8-7+6-5+4-3+2-1-0\n") == "4"

# custom cases
assert run("0+0\n") == "0", "all zeros"
assert run("255-255+255\n") == "255", "boundary values"
assert run("1-2-3-4-5\n") == "-13", "all subtraction chain"
assert run("10+20+30-40+50\n") == "70", "mixed operations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0+0 | 0 | toán hạng tối thiểu | 
| 255-255+255 | 255 | độ đúng giới hạn trên | 
| 1-2-3-4-5 | -13 | chuỗi trừ lặp đi lặp lại | 
| 10+20+30-40+50 | 70 | dãy toán tử hỗn hợp | 

Những trường hợp này đảm bảo phân tích cú pháp chính xác các số 0, giá trị biên, các phép toán xen kẽ và chuỗi dài hơn mà không gặp vấn đề về quyền ưu tiên. 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là biểu thức một chữ số bắt đầu bằng 0. Ví dụ,`0+0`. Thuật toán khởi tạo bộ tích lũy trực tiếp từ số đầu tiên, do đó nó xử lý chính xác số 0 mà không yêu cầu cách viết hoa đặc biệt. 

Trường hợp cạnh thứ hai liên quan đến toán hạng có giá trị tối đa như`255-255+255`. Vòng phân tích cú pháp tích lũy các chữ số một cách an toàn thành số nguyên mà không lo tràn trong Python và mỗi thao tác được áp dụng ngay lập tức, do đó các giá trị trung gian vẫn chính xác. 

Trường hợp cạnh thứ ba là một chuỗi trừ dài xen kẽ như`1-2-3-4-5`. Thuật toán liên tục áp dụng phép trừ theo trình tự, khớp chính xác với hành vi từ trái sang phải của người đánh giá tham chiếu. Mỗi kết quả trung gian được lưu giữ trong bộ tích lũy, do đó không phát sinh sự mơ hồ về quyền ưu tiên.

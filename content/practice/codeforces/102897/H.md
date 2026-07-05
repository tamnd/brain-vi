---
title: "CF 102897H - Hsueh- Tiến độ rút thăm"
description: "Nhiệm vụ là hiển thị chỉ báo tiến trình dòng lệnh cho nhiều tình huống. Mỗi kịch bản mô tả tổng số lượng công việc và bao nhiêu trong số đó đã được hoàn thành."
date: "2026-07-04T09:21:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102897
codeforces_index: "H"
codeforces_contest_name: "The 3rd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102897
solve_time_s: 48
verified: true
draft: false
---

[CF 102897H - Hsueh- Tiến trình vẽ](https://codeforces.com/problemset/problem/102897/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là hiển thị chỉ báo tiến trình dòng lệnh cho nhiều tình huống. Mỗi kịch bản mô tả tổng số lượng công việc và bao nhiêu trong số đó đã được hoàn thành. Từ đó, chúng ta phải xây dựng một thanh trực quan trong đó các đơn vị đã hoàn thành được hiển thị dưới dạng ký tự băm và các đơn vị còn lại được hiển thị dưới dạng dấu gạch nối. Bên cạnh thanh này, chúng tôi cũng in giá trị phần trăm biểu thị phần công việc đã hoàn thành được chia tỷ lệ thành 100 và làm tròn xuống. 

Mỗi trường hợp thử nghiệm cho hai số nguyên, tổng số đơn vị và số lượng đơn vị hoàn thành. Đầu ra cho mỗi trường hợp là một dòng duy nhất bao gồm một thanh trong ngoặc theo sau là khoảng trắng và phần trăm nguyên có dấu phần trăm. 

Chi tiết cấu trúc chính là thanh tiến trình có chính xác một ký tự trên mỗi đơn vị tổng công việc. Điều này có nghĩa là đầu ra không được trừu tượng hóa hoặc chia tỷ lệ, nó phản ánh trực tiếp kích thước đầu vào. 

Các ràng buộc cho phép tối đa mười trường hợp thử nghiệm và mỗi trường hợp thử nghiệm có thể có tới một trăm nghìn đơn vị. Điều này có nghĩa là một giải pháp đơn giản xây dựng các chuỗi theo thời gian bậc hai sẽ quá chậm, nhưng bất kỳ giải pháp nào xây dựng từng dòng đầu ra theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm đều nhanh chóng. Tổng công việc trên tất cả các trường hợp thử nghiệm tối đa là một triệu ký tự, nằm trong giới hạn đầu ra thông thường trong một giây. 

Một trường hợp phức tạp phát sinh khi không có tiến triển nào được thực hiện hoặc khi mọi thứ đã hoàn tất. Khi hoàn thành bằng 0, thanh chỉ được chứa dấu gạch nối và tỷ lệ phần trăm sẽ bằng 0. Khi hoàn thành bằng tổng, thanh chỉ được chứa giá trị băm và tỷ lệ phần trăm phải chính xác là một trăm. Một trường hợp cạnh khác là khi tổng bằng một, trong đó thanh thoái hóa thành một ký tự đơn và hành vi làm tròn vẫn phải chính xác. 

Một lỗi phổ biến là cố gắng tính tỷ lệ phần trăm bằng cách sử dụng phép chia và làm tròn dấu phẩy động, điều này có thể gây ra các vấn đề về độ chính xác hoặc hành vi làm tròn không chính xác. Một sai lầm khác là quên rằng phép chia số nguyên phải dựa trên sàn chứ không được làm tròn đến gần nhất. 

## Phương pháp tiếp cận 

Một cách trực tiếp để xây dựng đầu ra là mô phỏng từng ký tự của thanh tiến trình. Đối với mỗi trường hợp thử nghiệm, chúng tôi lặp lại từ 0 đến n trừ một, thêm hàm băm nếu chỉ số nhỏ hơn m, nếu không thì thêm dấu gạch nối. Sau khi xây dựng thanh, chúng tôi tính tỷ lệ phần trăm bằng cách sử dụng phép chia số nguyên khi m nhân với 100 chia cho n. Cách tiếp cận này đúng vì nó tuân theo định nghĩa của định dạng đầu ra. 

Mối lo ngại về tính kém hiệu quả chỉ xuất phát từ việc nối chuỗi lặp đi lặp lại nếu được triển khai kém. Nếu chúng ta liên tục nối thêm các chuỗi bất biến bên trong một vòng lặp, thì độ phức tạp có thể giảm xuống theo hành vi bậc hai. Tuy nhiên, nếu chúng ta xây dựng một danh sách các ký tự và nối một lần hoặc sử dụng phép nhân trực tiếp các chuỗi thì việc xây dựng vẫn tuyến tính. 

Quan sát quan trọng là cả hai phần của đầu ra đều được xác định đầy đủ bằng số học đơn giản và sự lặp lại. Không có sự phụ thuộc giữa các vị trí ngoài việc chỉ số có dưới m hay không. Điều này loại bỏ mọi nhu cầu mô phỏng ngoài việc xây dựng trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Nối từng ký tự (chắp thêm chuỗi đơn giản) | O(n^2) trường hợp xấu nhất | O(n) | Quá chậm | 
| Xây dựng trực tiếp bằng cách lặp lại hoặc nối danh sách | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi test, đọc tổng độ dài n và số lượng hoàn thành m. Hai giá trị này xác định đầy đủ cả thanh và tỷ lệ phần trăm. 
2. Xây dựng thanh tiến trình bằng cách tạo một chuỗi gồm m ký tự băm theo sau là n trừ m dấu gạch ngang. Điều này hiệu quả vì mỗi vị trí tương ứng trực tiếp với một đơn vị công việc và các đơn vị đã hoàn thành luôn chiếm tiền tố. 
3. Tính tỷ lệ phần trăm khi chia số nguyên của m nhân với 100 cho n. Điều này đảm bảo việc cắt bớt về 0, phù hợp với hành vi sàn được yêu cầu. 
4. Định dạng đầu ra bằng cách gói thanh trong dấu ngoặc vuông, thêm khoảng trắng, sau đó in phần trăm được tính toán theo sau là dấu phần trăm. 

Tại sao nó hoạt động: thanh tiến trình là mã hóa trực tiếp của điều kiện tiền tố trên một mảng có độ dài cố định. Mọi biểu diễn hợp lệ phải đặt tất cả các đơn vị đã hoàn thành trước để khớp với cấu trúc mẫu và mọi đơn vị còn lại xếp sau. Tính toán tỷ lệ phần trăm là một tỷ lệ tuyến tính có cùng tỷ lệ, do đó cả hai kết quả đầu ra đều là các chế độ xem nhất quán của cùng một phân số cơ bản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        bar = "#" * m + "-" * (n - m)
        percent = (m * 100) // n if n > 0 else 0
        sys.stdout.write(f"[{bar}] {percent}%\n")

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên sự lặp lại chuỗi gốc của Python, điều này hiệu quả vì nó xây dựng chuỗi cuối cùng theo thời gian tuyến tính so với độ dài của nó. biểu thức`"#" * m`tạo ra chính xác m ký tự mà không cần thêm chi phí nối. Điều tương tự cũng áp dụng cho đoạn gạch nối. 

Tính toán tỷ lệ phần trăm sử dụng phép nhân số nguyên trước khi chia để tránh các vấn đề về độ chính xác của dấu phẩy động. Bảo vệ có điều kiện cho`n > 0`về mặt kỹ thuật là không cần thiết theo các ràng buộc, nhưng nó làm cho việc tính toán trở nên mạnh mẽ trước các dữ liệu đầu vào không đúng định dạng. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp n là 10 và m là 2. Thanh phải hiển thị hai đơn vị hoàn chỉnh theo sau là tám đơn vị chưa hoàn chỉnh. 

| Bước | n | m | Xây dựng quán bar | Tỷ lệ phần trăm | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 2 | "" | - | 
| 2 | 10 | 2 | "##--------" | - | 
| 3 | 10 | 2 | "##--------" | 20 | 

Thanh đặt chính xác hai giá trị băm ở phía trước và tỷ lệ phần trăm là sàn (20). 

Bây giờ coi n bằng 5 và m bằng 0. 

| Bước | n | m | Xây dựng quán bar | Tỷ lệ phần trăm | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | "" | - | 
| 2 | 5 | 0 | "------" | - | 
| 3 | 5 | 0 | "------" | 0 | 

Điều này xác nhận rằng tiến trình bằng 0 mang lại một thanh nét đứt hoàn toàn và không phần trăm. 

Những ví dụ này xác nhận cả hành vi ranh giới và tính nhất quán giữa các biểu diễn trực quan và số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ký tự của thanh được tạo chính xác một lần bằng cách lặp lại chuỗi | 
| Không gian | O(n) | Chuỗi đầu ra lưu trữ một ký tự trên mỗi đơn vị n | 

Tổng công việc trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng của tất cả n giá trị, vừa vặn trong giới hạn vì n tối đa là 100000 cho mỗi trường hợp thử nghiệm và có nhiều nhất là 10 trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        bar = "#" * m + "-" * (n - m)
        percent = (m * 100) // n
        sys.stdout.write(f"[{bar}] {percent}%\n")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    try:
        solve_io()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# provided sample-style cases
assert run("1\n10 2\n") == "[##--------] 20%\n"
assert run("1\n5 0\n") == "[-----] 0%\n"

# minimum size
assert run("1\n1 0\n") == "[-] 0%\n"
assert run("1\n1 1\n") == "[#] 100%\n"

# full progress
assert run("1\n6 6\n") == "[######] 100%\n"

# mixed case
assert run("2\n3 1\n4 3\n") == "[#--] 33%\n[###-] 75%\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1,m=0 |`[-] 0%`| thanh không tầm thường nhỏ nhất | 
| n=1,m=1 |`[#] 100%`| trường hợp cạnh hoàn thành đầy đủ | 
| n=m | băm đầy đủ | độ chính xác bão hòa | 
| nhiều trường hợp | nhiều dòng | xử lý T | 

## Vỏ cạnh 

Khi m bằng 0, cấu trúc tạo ra một đoạn băm trống và một đoạn gạch nối đầy đủ. Ví dụ: với đầu vào n bằng 5 và m bằng 0, biểu thức`"#" * 0 + "-" * 5`sản lượng`"-----"`, và tỷ lệ phần trăm`(0 * 100) // 5`đánh giá bằng không. Thuật toán xử lý việc này một cách tự nhiên mà không cần phân nhánh đặc biệt vì việc lặp lại chuỗi bằng 0 sẽ tạo ra một chuỗi trống. 

Khi m bằng n, đoạn gạch nối sẽ trống. Với n bằng 4 và m bằng 4, thanh trở thành`"####"`và tỷ lệ phần trăm ước tính là 100. Điều này có hiệu quả vì trừ m cho n mang lại kết quả bằng 0 và việc lặp lại một chuỗi 0 lần một cách chính xác không đóng góp gì vào kết quả đầu ra. 

Khi n bằng 1, thanh sẽ thoái hóa thành một ký tự đơn. Nếu m bằng 0, nó trở thành`"-"`, và nếu m là một, nó trở thành`"#"`. Phép chia số nguyên vẫn tạo ra 0 hoặc 100, phù hợp với hành vi mong đợi mà không cần số học dấu phẩy động.

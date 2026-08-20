---
title: "CF 102212A - Cộng hai số nguyên"
description: "Nhiệm vụ rất đơn giản: một dòng chứa hai số nguyên a và b và chương trình phải in số nguyên thu được bằng cách cộng chúng."
date: "2026-08-18T00:22:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102212
codeforces_index: "A"
codeforces_contest_name: "Amazalgo Uni 2019 Practice Contest"
rating: 0
weight: 102212
solve_time_s: 149
verified: false
draft: false
---

[CF 102212A - Cộng hai số nguyên](https://codeforces.com/problemset/problem/102212/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ rất đơn giản: một dòng chứa hai số nguyên,`a`Và`b`, và chương trình phải in số nguyên thu được bằng cách cộng chúng. Các giá trị có thể là dương, 0 hoặc âm, do đó thao tác này là phép cộng số nguyên có dấu thông thường thay vì bất kỳ xử lý đặc biệt nào đối với các giá trị không âm. 

Các giới hạn đủ nhỏ để bản thân số học là tầm thường. Mỗi giá trị đầu vào nằm giữa`-1,000,000,000`Và`1,000,000,000`, vậy tổng của chúng nằm giữa`-2,000,000,000`Và`2,000,000,000`. Không cần một thuật toán phụ thuộc vào độ lớn của một trong hai số. Một lượng công việc không đổi là đủ và kiểu số nguyên của Python có thể biểu thị mọi giá trị trong phạm vi đã nêu mà không bị tràn. 

Các trường hợp cạnh chính đến từ các dấu hiệu và giá trị biên. Ví dụ, với đầu vào`50 -26`, đầu ra đúng là`24`. Một giải pháp bất cẩn giả định cả hai giá trị đều dương có thể xử lý sai toán hạng âm. Với đầu vào`-1000000000 -1000000000`, đầu ra đúng là`-2000000000`, do đó việc triển khai phải bảo toàn cả dấu và độ lớn đầy đủ. Tương tự,`1000000000 1000000000`phải sản xuất`2000000000`, giúp nắm bắt việc triển khai bằng cách sử dụng loại số nguyên hẹp không cần thiết trong các ngôn ngữ có thể tràn. 

Zero là một trường hợp ranh giới đơn giản khác. Đối với đầu vào`0 7`, câu trả lời là`7`, và cho`-5 0`, câu trả lời là`-5`. Không có nhánh thuật toán đặc biệt nào cần thiết cho số 0, đây là một trong những quan sát hữu ích trong bài toán này. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực theo nghĩa đen có thể coi phép cộng là sự tăng dần lặp đi lặp lại. Bắt đầu từ`a`, nó có thể thực hiện một mức tăng cho mỗi đơn vị được biểu thị bằng một số dương`b`hoặc giảm một đơn vị cho mỗi đơn vị được biểu thị bằng số âm`b`. Điều này đúng vì mỗi thao tác thay đổi giá trị hiện tại đúng một, vì vậy sau số thao tác cần thiết, kết quả là`a + b`. Vấn đề là số lượng hoạt động. Ví dụ, thêm`1,000,000,000`sẽ yêu cầu một tỷ lần lặp và trong trường hợp xấu nhất độ lớn của toán hạng thứ hai là`1,000,000,000`. Điều đó từ bỏ`1,000,000,000`lặp đi lặp lại, vượt xa giới hạn một giây của Codeforces có thể đáp ứng. 

Cấu trúc của bài toán cho chúng ta một quan sát đơn giản hơn nhiều. Thao tác đang được yêu cầu đã được hỗ trợ trực tiếp bởi ngôn ngữ lập trình: phép cộng số nguyên sẽ tính kết quả mong muốn trong thời gian không đổi. Không có không gian tìm kiếm ẩn, không có mảng để xử lý và không có mối quan hệ giữa nhiều trường hợp thử nghiệm cần khai thác. 

Phương pháp brute-force hoạt động vì nó xây dựng lại phép cộng mỗi lần một đơn vị, nhưng không thành công vì số lượng đơn vị có thể lên tới một tỷ. Quan sát rằng bản thân thao tác được yêu cầu là một phép toán số nguyên nguyên thủy cho phép chúng ta giảm toàn bộ tác vụ thành một phép cộng sau khi phân tích cú pháp hai giá trị đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( | b | ) | O(1) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên từ một dòng đầu vào. Chúng được phân tách bằng khoảng trắng, do đó việc tách dòng sẽ cho hai giá trị trực tiếp. 
2. Tính toán`a + b`. Đây chính xác là đại lượng toán học được yêu cầu và Python xử lý trực tiếp số học số nguyên có dấu. 
3. In số nguyên thu được. In giá trị dưới dạng số nguyên giữ nguyên dấu của nó khi kết quả âm. 

### Tại sao nó hoạt động 

Giá trị được duy trì duy nhất là tổng toán học chính xác của hai toán hạng được phân tích cú pháp. Phép cộng thay thế hai giá trị đầu vào bằng tổng số học của chúng, do đó giá trị kết quả chính xác là đại lượng mà bài toán yêu cầu. Vì không có phép tính gần đúng, phép lặp hoặc phép biến đổi nào được thực hiện nên không có trạng thái trung gian nào có thể gây ra lỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())
    print(a + b)

if __name__ == "__main__":
    solve()
```Dòng đầu tiên nhập`sys`, cho phép giải pháp sử dụng yêu cầu`sys.stdin.readline`phương pháp nhập liệu. các`input`bí danh đọc dòng đầu vào hoàn chỉnh một cách hiệu quả và khớp với mẫu lập trình cạnh tranh tiêu chuẩn. 

Bên trong`solve`,`map(int, input().split())`chuyển đổi hai giá trị văn bản thành số nguyên Python. Số âm được xử lý tự động bởi`int`, do đó không cần phân tích dấu hiệu riêng biệt. 

biểu hiện`a + b`thực hiện phép tính duy nhất được yêu cầu. Số nguyên Python có độ chính xác tùy ý, do đó, ngay cả kết quả lớn nhất có thể,`2,000,000,000`, được biểu diễn chính xác Không có vòng lặp, chỉ mục mảng hoặc điều kiện biên nào có thể xảy ra lỗi riêng lẻ. 

Bài toán chứa chính xác một cặp số nguyên, do đó không có số lượng trường hợp kiểm thử cần đọc và không có vòng lặp trên nhiều trường hợp. các`if __name__ == "__main__"`bảo vệ chỉ đơn giản là làm cho`solve()`chạy khi tập tin được thực thi bình thường. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào chứa hai số nguyên dương. 

| Bước |`a`|`b`|`a + b`| 
| --- | --- | --- | --- | 
| Đọc đầu vào | 5 | 9 | 14 | 
| Thêm giá trị | 5 | 9 | 14 | 
| In kết quả | 5 | 9 | 14 | 

Giá trị tính toán là`5 + 9 = 14`, do đó chương trình in`14`. Điều này chứng tỏ trường hợp số nguyên dương thông thường. 

Đối với Mẫu 2, toán hạng thứ hai là số âm. 

| Bước |`a`|`b`|`a + b`| 
| --- | --- | --- | --- | 
| Đọc đầu vào | 50 | -26 | 24 | 
| Thêm giá trị | 50 | -26 | 24 | 
| In kết quả | 50 | -26 | 24 | 

Dấu âm được giữ nguyên khi Python phân tích cú pháp`-26`và phép cộng có dấu thông thường sẽ cho`50 + (-26) = 24`. Điều này khẳng định rằng thuật toán không cần trường hợp đặc biệt cho các dấu hỗn hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Giải pháp phân tích hai số nguyên và thực hiện một phép cộng. | 
| Không gian | O(1) | Chỉ có hai số nguyên đầu vào và số nguyên kết quả được lưu trữ. | 

Đầu vào chỉ chứa hai số nguyên giới hạn, do đó giải pháp thời gian không đổi thấp hơn rất nhiều so với giới hạn thời gian một giây. Việc sử dụng bộ nhớ của nó cũng không đáng kể so với giới hạn 64 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    a, b = map(int, input().split())
    print(a + b)

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else ""
    finally:
        sys.stdin = old_stdin
        input = old_input

# A version of run that captures stdout correctly.
def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

# Provided samples
assert run("5 9\n") == "14\n", "sample 1"
assert run("50 -26\n") == "24\n", "sample 2"

# Custom cases
assert run("0 0\n") == "0\n", "both operands are zero"
assert run("-1000000000 -1000000000\n") == "-2000000000\n", "minimum possible sum"
assert run("1000000000 1000000000\n") == "2000000000\n", "maximum possible sum"
assert run("-1 1\n") == "0\n", "opposite values cancel exactly"
assert run("-1000000000 1000000000\n") == "0\n", "maximum magnitudes cancel"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`0`| Giá trị cường độ nhỏ nhất và kết quả bằng 0 | 
|`-1000000000 -1000000000`|`-2000000000`| Giới hạn dưới của tổng có thể | 
|`1000000000 1000000000`|`2000000000`| Ranh giới trên của tổng có thể | 
|`-1 1`|`0`| Hủy bỏ chính xác giữa các dấu hiệu trái ngược nhau | 
|`-1000000000 1000000000`|`0`| Hủy bỏ ở cường độ lớn nhất cho phép | 

Trình trợ giúp kiểm tra tạm thời thay thế đầu vào và đầu ra tiêu chuẩn để`solve()`có thể được thực hiện chính xác như trên Codeforces. Sau đó, các thử nghiệm sẽ khôi phục các luồng ban đầu, ngăn không cho thử nghiệm này ảnh hưởng đến thử nghiệm khác. 

## Vỏ cạnh 

Trường hợp dấu hỗn hợp`50 -26`được xử lý bằng cách phân tích cú pháp cả hai mã thông báo dưới dạng số nguyên đã ký và đánh giá`50 + (-26)`, tạo ra`24`. Không cần phân nhánh để phân biệt phép cộng và phép trừ vì dấu âm đã là một phần của giá trị nguyên. 

Đối với các toán hạng nhỏ nhất có thể, đầu vào`-1000000000 -1000000000`được phân tích thành hai số nguyên âm. Việc bổ sung tạo ra`-2000000000`, đó chính xác là số tiền tối thiểu được phép. Biểu diễn số nguyên của Python không tràn ở ranh giới này. 

Đối với các toán hạng lớn nhất có thể,`1000000000 1000000000`sản xuất`2000000000`. Thuật toán thực hiện phép cộng đơn tương tự như đối với các giá trị nhỏ, do đó không có nhánh giới hạn trên đặc biệt hoặc điều kiện ngoài một. 

Để hủy bỏ chính xác, hãy xem xét`-1000000000 1000000000`. Hai toán hạng có độ lớn bằng nhau và trái dấu nên phép cộng cho`0`. Điều này kiểm tra xem việc triển khai có bảo toàn chính xác cả hai dấu hiệu hay không thay vì xử lý các giá trị đầu vào dưới dạng cường độ tuyệt đối. 

Cuối cùng,`0 7`cho`7`, trong khi`-5 0`cho`-5`. Số 0 không yêu cầu xử lý đặc biệt vì việc thêm số 0 sẽ giữ nguyên toán hạng kia, đây chính xác là những gì phép cộng số nguyên trực tiếp tính toán.

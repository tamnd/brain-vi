---
title: "CF 171E - NGÔN NGỮ BÍ ẨN"
description: "Đây là một trong những vấn đề \"đặc biệt\" kinh điển của Codeforces. Không giống như các tác vụ thuật toán thông thường, không có đầu vào có ý nghĩa nào để xử lý và không có thách thức về cấu trúc dữ liệu hoặc tối ưu hóa nào cần giải quyết. Cuộc thi cung cấp quyền truy cập vào ngôn ngữ có tên Bí mật thông qua môi trường thử nghiệm tùy chỉnh."
date: "2026-06-02T08:48:05+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 171
codeforces_index: "E"
codeforces_contest_name: "April Fools Day Contest"
rating: 2000
weight: 171
solve_time_s: 61
verified: true
draft: false
---

[CF 171E - NGÔN NGỮ BÍ ẨN](https://codeforces.com/problemset/problem/171/E) 

**Xếp hạng:** 2000 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đây là một trong những vấn đề “đặc biệt” kinh điển của Codeforces. Không giống như các tác vụ thuật toán thông thường, không có đầu vào có ý nghĩa nào để xử lý và không có thách thức về cấu trúc dữ liệu hoặc tối ưu hóa nào cần giải quyết. 

Cuộc thi cung cấp quyền truy cập vào ngôn ngữ có tên **Bí mật** thông qua môi trường thử nghiệm tùy chỉnh. Thách thức thực sự là xác định ngôn ngữ lập trình "Bí mật" thực sự là gì, sau đó gửi chương trình được viết bằng ngôn ngữ đó in tên ngôn ngữ đó. 

Luồng đầu vào trống. Chương trình không bao giờ cần phải đọc bất cứ điều gì. Toàn bộ đầu ra bao gồm một chuỗi cố định duy nhất, cụ thể là tên ngôn ngữ mà Secret tương ứng. 

Độ khó của bài toán không nằm ở thuật toán. Giải pháp dự định là thử nghiệm môi trường ngôn ngữ Bí mật và xác định nó từ cú pháp và hành vi của nó. 

Vì không có kích thước đầu vào và không có tính toán nên các ràng buộc về độ phức tạp là không liên quan. Bất kỳ chương trình nào in thành công tên ngôn ngữ được yêu cầu đều được chấp nhận. 

Nguồn gốc thực sự duy nhất của sai lầm là việc xác định ngôn ngữ không chính xác. Ví dụ: hai ngôn ngữ có thể chia sẻ cú pháp tương tự cho các khai báo biến hoặc vòng lặp. Thí sinh đoán sớm có thể xuất ra tên ngôn ngữ sai mặc dù bản thân chương trình chạy đúng. 

Để minh họa, giả sử Secret thực sự là Python và một thí sinh kết luận đó là Ruby. Chương trình của họ có thể thực thi thành công trong môi trường nếu nó sử dụng cú pháp được cả hai ngôn ngữ chấp nhận, nhưng kết quả đầu ra sẽ sai vì câu trả lời bắt buộc là tên ngôn ngữ chính xác. 

## Phương pháp tiếp cận 

Tư duy bạo lực sẽ là liên tục gửi các chương trình thử nghiệm nhỏ và quan sát các lỗi biên dịch, hành vi thời gian chạy, các thư viện có sẵn, quy tắc cú pháp và các manh mối khác. Bằng cách thu thập đủ bằng chứng, người ta có thể thu hẹp các khả năng cho đến khi ngôn ngữ được xác định duy nhất. 

Một khi ngôn ngữ đã được xác định thì không còn công việc thuật toán nào nữa. Giải pháp tối ưu đơn giản là in tên ngôn ngữ. 

Đối với vấn đề cụ thể này, ngôn ngữ Bí mật được phát hiện là **Brainfuck**. Do đó, bài gửi được chấp nhận là một chương trình Brainfuck xuất ra chuỗi:```
Brainfuck
```Toàn bộ thách thức là kỹ thuật đảo ngược môi trường thực thi thay vì thiết kế một thuật toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Điều tra vũ lực | thí nghiệm O(k) | O(1) | Dùng để nhận dạng ngôn ngữ | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra môi trường ngôn ngữ bí mật và xác định ngôn ngữ lập trình nào nó thực sự triển khai. 
2. Phát hiện ra ngôn ngữ đó là Brainfuck. 
3. Viết chương trình Brainfuck in văn bản`Brainfuck`. 
4. Gửi chương trình đó. 

### Tại sao nó hoạt động 

Thẩm phán không cung cấp đầu vào và mong đợi một đầu ra cố định duy nhất. Khi ngôn ngữ đã được xác định chính xác là Brainfuck, một chương trình Brainfuck phát ra chuỗi`Brainfuck`khớp chính xác với đầu ra được yêu cầu. Vì chỉ có một trường hợp thử nghiệm và không có hành vi phụ thuộc vào đầu vào nên tính đúng đắn sẽ được thực hiện ngay lập tức. 

## Giải pháp Python 

Không có giải pháp Python có ý nghĩa nào vì bản thân bài nộp phải được viết bằng Brainfuck. Nếu chúng ta biểu diễn hành vi tương đương trong Python thì nó sẽ đơn giản là:```python
import sys
input = sys.stdin.readline

print("Brainfuck")
```Nội dung gửi được chấp nhận thực tế trên Codeforces là chương trình Brainfuck, không phải chương trình Python. Phiên bản Python ở trên thể hiện hành vi cần thiết: tạo đầu ra cố định và chấm dứt. 

## Ví dụ đã hoạt động 

Sự cố không chứa đầu vào. Mỗi lần thực thi đều đi theo cùng một con đường. 

### Ví dụ 1 

đầu vào:```

```| Bước | Hành động | Đầu ra | 
| --- | --- | --- | 
| 1 | Bắt đầu chương trình | "" | 
| 2 | Tên ngôn ngữ in | "Chết tiệt" | 
| 3 | Chấm dứt | "Chết tiệt" | 

Dấu vết này cho thấy đầu ra không phụ thuộc vào bất kỳ dữ liệu bên ngoài nào. 

### Ví dụ 2 

đầu vào:```

```| Bước | Hành động | Đầu ra | 
| --- | --- | --- | 
| 1 | Bắt đầu chương trình | "" | 
| 2 | Tên ngôn ngữ in | "Chết tiệt" | 
| 3 | Chấm dứt | "Chết tiệt" | 

Lần thực thi thứ hai giống hệt nhau vì tác vụ không có đầu vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một chuỗi cố định được in | 
| Không gian | O(1) | Chỉ sử dụng bộ nhớ không đổi | 

Chương trình thực hiện một lượng công việc không đổi bất kể môi trường thực thi. Nó dễ dàng đáp ứng mọi giới hạn về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    print("Brainfuck")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue()

# provided sample
assert run("") == "Brainfuck\n", "sample"

# custom cases
assert run("") == "Brainfuck\n", "empty input"
assert run("\n") == "Brainfuck\n", "extra newline ignored"
assert run("random data") == "Brainfuck\n", "input unused"
assert run("1 2 3 4 5") == "Brainfuck\n", "still fixed output"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đầu vào trống | Đồ ngu | Hành vi chính thức | 
| chỉ dòng mới | Đồ ngu | Đầu vào bị bỏ qua | 
| dữ liệu ngẫu nhiên | Đồ ngu | Đầu ra không đổi | 
| 1 2 3 4 5 | Đồ ngu | Không phụ thuộc vào đầu vào | 

## Vỏ cạnh 

Trường hợp cạnh chính là hoàn toàn không có đầu vào. 

đầu vào:```

```Thuật toán ngay lập tức thực hiện hành động đơn lẻ của nó, in`Brainfuck`. Vì không có gì cần phải đọc nên không có khả năng xảy ra lỗi phân tích cú pháp đầu vào. 

Một trường hợp tinh tế khác là cung cấp thông tin đầu vào không mong muốn trong quá trình thử nghiệm cục bộ:```
hello world
```Một giải pháp đúng vẫn in:```
Brainfuck
```Đặc tả nhiệm vụ đảm bảo rằng thẩm phán không cung cấp đầu vào, nhưng dù sao thì giải pháp đầu ra cố định được viết đúng không phụ thuộc vào nội dung của stdin. 

Chế độ thất bại tiềm ẩn cuối cùng là xác định sai ngôn ngữ Bí mật. Nếu thí sinh kết luận rằng Bí mật là một số ngôn ngữ khác và kết quả đầu ra:```
Python
```việc gửi bị từ chối mặc dù chương trình có thể thực hiện thành công. Xác định chính xác ngôn ngữ là toàn bộ thách thức.

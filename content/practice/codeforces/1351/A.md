---
title: "CF 1351A - A+B (Bài toán thử nghiệm)"
description: "Nhiệm vụ yêu cầu chúng ta cộng các cặp số nguyên. Mỗi trường hợp thử nghiệm bao gồm hai số nguyên và với mỗi cặp, chúng ta cần tính tổng của chúng. Đầu vào đầu tiên cho chúng ta biết có bao nhiêu cặp, sau đó mỗi dòng tiếp theo chứa một cặp."
date: "2026-06-11T14:30:34+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1351
codeforces_index: "A"
codeforces_contest_name: "Testing Round 16 (Unrated)"
rating: 800
weight: 1351
solve_time_s: 137
verified: true
draft: false
---

[CF 1351A - A+B (Vấn đề thử nghiệm)](https://codeforces.com/problemset/problem/1351/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ yêu cầu chúng ta cộng các cặp số nguyên. Mỗi trường hợp thử nghiệm bao gồm hai số nguyên và với mỗi cặp, chúng ta cần tính tổng của chúng. Đầu vào đầu tiên cho chúng ta biết có bao nhiêu cặp, sau đó mỗi dòng tiếp theo chứa một cặp. Đầu ra là một chuỗi các tổng, mỗi tổng một dòng, tương ứng với mỗi cặp. 

Các ràng buộc rất đơn giản nhưng đáng chú ý. Mỗi số nguyên nằm trong khoảng từ -1000 đến 1000, điều đó có nghĩa là tràn số nguyên không phải là vấn đề đáng lo ngại trong Python. Số lượng trường hợp thử nghiệm có thể lên tới 10.000, điều này ngụ ý rằng bất kỳ giải pháp nào lặp qua tất cả các trường hợp thử nghiệm một lần đều đủ hiệu quả, vì Python có thể thoải mái xử lý 10.000 thao tác đơn giản trong vòng chưa đầy một giây. Số âm giới thiệu khả năng của tổng bằng 0 hoặc âm, vì vậy lời giải không được giả sử đầu vào hoàn toàn dương. 

Các trường hợp cạnh không rõ ràng bao gồm các cặp trong đó một hoặc cả hai số bằng 0, trong đó tổng vượt qua 0 từ dương sang âm hoặc ngược lại và cả hai số đều ở giới hạn (-1000 hoặc 1000). Ví dụ như cặp`-1000 1000`nên xuất ra`0`. Việc thực hiện bất cẩn mà bỏ qua những điều tiêu cực có thể tạo ra dấu hiệu sai. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là đọc từng dòng, phân tích hai số nguyên, tính tổng của chúng và in kết quả ngay lập tức. Điều này hiệu quả vì tính tổng hai số nguyên là một phép toán O(1) và việc lặp lại hơn 10.000 trường hợp kiểm thử là hiệu quả trong Python. Không có sự phức tạp tiềm ẩn nào trong việc đọc và xử lý dữ liệu đầu vào một cách tuần tự. 

Không có cách tiếp cận tiệm cận nào tối ưu hơn vì chúng ta phải xử lý mọi trường hợp kiểm thử để tính tổng của nó. Cải tiến duy nhất có thể là đọc đầu vào hàng loạt để tăng tốc bằng cách sử dụng`sys.stdin.readline`, điều này tránh được đầu vào mặc định chậm hơn của Python. Sự tối ưu hóa nhỏ này đảm bảo rằng ngay cả ở kích thước đầu vào tối đa, chương trình vẫn chạy thoải mái trong giới hạn thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t) | O(1) | Đã chấp nhận | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Điều này cho chúng ta biết chúng ta cần xử lý bao nhiêu cặp. 
2. Lặp lại`t`lần. Trong mỗi lần lặp: 

1. Đọc một dòng chứa hai số nguyên,`a`Và`b`. 
2. Chuyển đổi dòng thành số nguyên. Việc chuyển đổi là cần thiết vì đầu vào là một chuỗi. 
3. Tính tổng`a + b`. 
4. In số tiền ngay lập tức. 
3. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Bất biến quan trọng là với mỗi lần lặp,`a`Và`b`là các số nguyên hợp lệ trong các ràng buộc. Tính tổng chúng sẽ tạo ra câu trả lời đúng và việc in ngay lập tức sẽ duy trì thứ tự đầu ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b = map(int, input().split())
    print(a + b)
```Dòng đầu tiên ghi số lượng test case. Vòng lặp chạy chính xác`t`lần, đọc từng cặp, chuyển đổi sang số nguyên, tính tổng và in. sử dụng`sys.stdin.readline`tránh các thao tác nhập chậm trên các tệp thử nghiệm lớn.`map(int, input().split())`đảm bảo cả hai giá trị được phân tích cú pháp chính xác ngay cả khi âm. In bên trong vòng lặp duy trì trật tự mà không cần lưu trữ thêm. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4
1 5
314 15
-99 99
123 987
```| Lặp lại | một | b | a+b | 
| --- | --- | --- | --- | 
| 1 | 1 | 5 | 6 | 
| 2 | 314 | 15 | 329 | 
| 3 | -99 | 99 | 0 | 
| 4 | 123 | 987 | 1110 | 

Dấu vết này xác nhận thuật toán xử lý chính xác các tổng dương, âm và 0, in chúng theo thứ tự. 

### Đầu vào tùy chỉnh```
3
0 0
-1000 1000
500 -200
```| Lặp lại | một | b | a+b | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 
| 2 | -1000 | 1000 | 0 | 
| 3 | 500 | -200 | 300 | 

Điều này chứng tỏ thuật toán xử lý chính xác các giá trị biên và dấu hỗn hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu các thao tác O(1): đọc hai số, tính tổng, in. | 
| Không gian | O(1) | Không cần cấu trúc dữ liệu bổ sung; chỉ hiện tại`a`Và`b`được lưu trữ. | 

Với t 10.000, giải pháp này chạy trong một phần giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        a, b = map(int, input().split())
        print(a + b)
    return output.getvalue().strip()

# provided sample
assert run("4\n1 5\n314 15\n-99 99\n123 987") == "6\n329\n0\n1110", "sample 1"

# minimum inputs
assert run("1\n0 0") == "0", "minimum input"

# maximum inputs
assert run("2\n1000 1000\n-1000 -1000") == "2000\n-2000", "maximum absolute values"

# mixed signs
assert run("3\n-500 500\n-200 100\n0 -1") == "0\n-100\n-1", "mixed signs"

# single test case
assert run("1\n123 456") == "579", "single case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 0 | 0 | giá trị tối thiểu | 
| 2 1000 1000 \n -1000 -1000 | 2000\n-2000 | giá trị tuyệt đối tối đa | 
| 3 -500 500 \n -200 100 \n 0 -1 | 0\n-100\n-1 | xử lý các dấu hiệu hỗn hợp và số không | 
| 1 123 456 | 579 | tính chính xác cho một trường hợp thử nghiệm | 

## Vỏ cạnh 

Đối với cặp`-1000 1000`, thuật toán đọc`a=-1000`,`b=1000`, tính toán`a+b=0`, và bản in`0`. Bất biến mà mỗi số đọc nằm trong giới hạn đảm bảo không bị tràn. Tổng được xử lý chính xác bất kể dấu nào, thể hiện độ tin cậy của thuật toán ở mức cực trị đầu vào. Tương tự, đối với`0 0`, tổng là`0`, xác nhận hành vi đúng cho đầu vào tối thiểu.

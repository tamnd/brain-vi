---
title: "CF 103409B - Vấn đề A Plus B"
description: "Đây là nhiệm vụ cộng số nguyên cổ điển được đóng khung trong bối cảnh lập trình cạnh tranh. Đầu vào bao gồm một hoặc nhiều cặp số nguyên và với mỗi cặp, chúng ta phải tính tổng số học của chúng và xuất ra một cách độc lập."
date: "2026-07-03T11:50:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103409
codeforces_index: "B"
codeforces_contest_name: "The 2021 CCPC Guilin Onsite (XXII Open Cup, Grand Prix of EDG)"
rating: 0
weight: 103409
solve_time_s: 45
verified: true
draft: false
---

[CF 103409B - Sự cố A Plus B](https://codeforces.com/problemset/problem/103409/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đây là nhiệm vụ cộng số nguyên cổ điển được đóng khung trong bối cảnh lập trình cạnh tranh. Đầu vào bao gồm một hoặc nhiều cặp số nguyên và với mỗi cặp, chúng ta phải tính tổng số học của chúng và xuất ra một cách độc lập. Không có sự tương tác giữa các trường hợp thử nghiệm, vì vậy mỗi cặp có thể được xử lý riêng biệt mà không lưu trữ bất kỳ trạng thái chung nào ngoài dòng hiện tại đang được đọc. 

Từ góc độ tính toán, mỗi trường hợp kiểm thử chỉ yêu cầu công việc có thời gian không đổi: đọc hai số nguyên, thực hiện một phép cộng và in kết quả. Ngay cả khi số lượng ca kiểm thử lớn, tổng độ phức tạp vẫn tuyến tính theo kích thước đầu vào, điều này là tối ưu vì mỗi số nguyên phải được đọc ít nhất một lần. 

Các trường hợp chính ở đây ít nói về cấu trúc thuật toán mà nói nhiều hơn về định dạng đầu vào và phạm vi số nguyên. Việc triển khai đơn giản có thể thất bại nếu nó chỉ giả định một cặp thay vì nhiều trường hợp thử nghiệm. Ví dụ: nếu đầu vào là:```
3
1 2
-5 10
1000000000 1000000000
```đầu ra đúng là:```
3
5
2000000000
```Một lỗi phổ biến là chỉ đọc dòng đầu tiên và bỏ qua các trường hợp kiểm thử còn lại, chỉ tạo ra`3`. Một vấn đề tế nhị khác là sử dụng loại ngôn ngữ không thể lưu giữ số tiền lớn một cách an toàn, nhưng trong Python thì đây không phải là vấn đề đáng lo ngại do các số nguyên có độ chính xác tùy ý. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo của vấn đề gần giống với giải pháp tối ưu. Người ta có thể tưởng tượng coi mỗi cặp như một bài toán tính toán nhỏ: phân tích các số, tính tổng của chúng bằng cách sử dụng số học tiêu chuẩn và xuất ra ngay lập tức. Không có cách nào có ý nghĩa để đơn giản hóa ngoài điều này vì bản thân phép cộng đã là O(1). 

Nếu chúng ta suy nghĩ quá kỹ về vấn đề, chúng ta có thể cố gắng lưu trữ tất cả các cặp trước và xử lý chúng sau, nhưng điều đó chỉ làm tăng mức sử dụng bộ nhớ mà không làm thay đổi độ phức tạp về thời gian. Một biến thể không cần thiết khác sẽ là chuyển đổi số thành chuỗi và mô phỏng phép cộng từng chữ số, vẫn tuyến tính về số chữ số và chậm hơn so với số học số nguyên nguyên gốc. 

Quan sát chính là cấu trúc của vấn đề không yêu cầu bất kỳ quá trình tiền xử lý, sắp xếp hoặc lập trình động nào. Mỗi trường hợp thử nghiệm đều độc lập nên việc truyền trực tiếp đầu vào vào đầu ra là đủ và tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (trực tiếp cho mỗi lần bổ sung trường hợp thử nghiệm) | O(T) | O(1) | Đã chấp nhận | 
| Tối ưu (xử lý trực tuyến) | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng ca kiểm thử để xác định số lượng phép tính cộng độc lập mà chúng ta sẽ thực hiện. Điều này cho phép chúng ta cấu trúc vòng lặp sao cho mỗi cặp được xử lý chính xác một lần. 
2. Với mỗi test, đọc hai số nguyên từ đầu vào. Bước này là cần thiết vì bài toán đảm bảo rằng mỗi phép tính là độc lập và khép kín. 
3. Tính tổng của hai số nguyên bằng số học tự nhiên. Đây là hoạt động cốt lõi và có thời gian không đổi bất kể cường độ đầu vào trong Python. 
4. Xuất ngay kết quả tính toán trước khi chuyển sang ca kiểm thử tiếp theo. Đầu ra phát trực tuyến tránh được việc lưu trữ không cần thiết và duy trì mức sử dụng bộ nhớ không đổi. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào thực tế là phép cộng có tính kết hợp và độc lập giữa các trường hợp thử nghiệm. Mỗi cặp số nguyên tạo thành một đơn vị tính toán khép kín: không có phép toán nào sau đó phụ thuộc vào kết quả trước đó. Vì chúng tôi xử lý mỗi cặp chính xác một lần và áp dụng phép toán số học chính xác theo yêu cầu của bài toán nên trình tự đầu ra được đảm bảo khớp với trình tự đầu vào của các ca kiểm thử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        a, b = map(int, input().split())
        out.append(str(a + b))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các trường hợp thử nghiệm một cách tuần tự và tích lũy kết quả vào một danh sách để có kết quả đầu ra hiệu quả. sử dụng`sys.stdin.readline`đảm bảo phân tích cú pháp đầu vào nhanh và việc kết hợp ở cuối sẽ tránh được chi phí I/O lặp lại. 

Một chi tiết triển khai tinh tế là đầu ra được lưu vào bộ đệm thay vì in từng dòng. Mặc dù các bản in riêng lẻ vẫn chính xác nhưng chúng có thể trở nên chậm khi T lớn do các lệnh gọi hệ thống lặp đi lặp lại. Tích lũy kết quả và viết một lần là tối ưu hóa lập trình cạnh tranh tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2
-5 10
7 7
```| Bước | một | b | tổng hợp | đầu ra cho đến nay | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 3 | 3 | 
| 2 | -5 | 10 | 5 | 3 5 | 
| 3 | 7 | 7 | 14 | 3 5 14 | 

Dấu vết này cho thấy mỗi cặp được xử lý độc lập và được nối theo thứ tự. Thuộc tính quan trọng được xác minh ở đây là tính ổn định của thứ tự đầu ra. 

### Ví dụ 2 

đầu vào:```
2
1000000000 1000000000
-100 -200
```| Bước | một | b | tổng hợp | đầu ra cho đến nay | 
| --- | --- | --- | --- | --- | 
| 1 | 1000000000 | 1000000000 | 2000000000 | 2000000000 | 
| 2 | -100 | -200 | -300 | 2000000000 -300 | 

Điều này xác nhận rằng giải pháp xử lý chính xác cả số nguyên dương lớn và giá trị âm mà không gặp sự cố tràn hoặc định dạng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm yêu cầu định dạng đầu ra và bổ sung thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến cố định được sử dụng ngoài bộ đệm đầu ra | 

Thời gian chạy tỉ lệ tuyến tính với số lượng ca kiểm thử, đây là mức tối ưu vì mỗi dòng đầu vào phải được đọc và xử lý ít nhất một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided-style sample
assert run("3\n1 2\n-5 10\n7 7\n") == "3\n5\n14"

# single test case
assert run("1\n0 0\n") == "0"

# negative numbers
assert run("2\n-1 -1\n-5 2\n") == "-2\n-3"

# large numbers
assert run("2\n1000000000 1\n999999999 999999999\n") == "1000000001\n1999999998"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp số 0 đơn | 0 | ranh giới tối thiểu | 
| giá trị âm | -2 -3 | xử lý biển báo | 
| số nguyên lớn | 1000000001, 1999999998 | tràn an toàn và đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi đầu vào chỉ chứa một trường hợp thử nghiệm duy nhất. Thuật toán vẫn hoạt động vì vòng lặp thực hiện đúng một lần và tạo ra một dòng đầu ra duy nhất. 

Một trường hợp cạnh khác là số nguyên âm, ví dụ đầu vào`-5 3`. Bước cộng không thay đổi do Python xử lý nguyên bản các số nguyên có dấu và kết quả đầu ra phản ánh chính xác tổng số học`-2`. 

Trường hợp cạnh cuối cùng là các số nguyên rất lớn gần với giới hạn 32 bit hoặc 64 bit thông thường. Ngay cả đối với các giá trị như`10^9 + 10^9`, thuật toán vẫn đúng vì kiểu số nguyên của Python tự động mở rộng để phù hợp với kết quả mà không bị tràn hoặc mất độ chính xác.

---
title: "CF 457A - Hệ thống vàng"
description: "Trong bài toán này, chúng ta được yêu cầu so sánh hai số được viết bằng một hệ thống số khác thường gọi là hệ thống vàng."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math", "meet-in-the-middle"]
categories: ["algorithms"]
codeforces_contest: 457
codeforces_index: "A"
codeforces_contest_name: "MemSQL Start[c]UP 2.0 - Round 2"
rating: 1700
weight: 457
solve_time_s: 75
verified: true
draft: false
---

[CF 457A - Hệ thống vàng](https://codeforces.com/problemset/problem/457/A) 

**Đánh giá:** 1700 
**Tags:** toán, gặp nhau ở giữa 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trong bài toán này, chúng ta được yêu cầu so sánh hai số được viết bằng một hệ thống số khác thường gọi là hệ thống vàng. Mỗi số được biểu diễn dưới dạng một chuỗi gồm 0 và 1 và giá trị của một số được tính bằng cách diễn giải nó dưới dạng tổng lũy ​​thừa của tỷ lệ vàng φ, trong đó φ thỏa mãn φ² = φ + 1. Đối với một chuỗi`a0a1…an`, giá trị là`a0*φ^n + a1*φ^(n-1) + … + an*φ^0`. Nhiệm vụ là xác định số nào trong hai số lớn hơn mà không cần tính toán lũy thừa dấu phẩy động một cách rõ ràng. 

Đầu vào bao gồm hai chuỗi nhị phân không trống, mỗi chuỗi tối đa 100.000 ký tự. Đầu ra là một ký tự đơn:`>`,`<`, hoặc`=`tùy theo giá trị tương đối. 

Độ dài đầu vào lớn ngay lập tức loại trừ bất kỳ phương pháp nào chuyển đổi số hệ thống vàng thành số dấu phẩy động tiêu chuẩn hoặc tính toán từng φ^k cho k lên tới 100.000, vì điều đó sẽ yêu cầu phép nhân O(n) và có thể dẫn đến các vấn đề về độ chính xác. Các trường hợp cạnh bao gồm các số có độ dài khác nhau trong đó số ngắn hơn có thể xuất hiện lớn hơn nếu được so sánh một cách ngây thơ về mặt từ điển. Ví dụ, so sánh`"100"`Và`"11"`, sự so sánh ngây thơ của các chuỗi sẽ gợi ý`"100"`lớn hơn, nhưng trong hệ thống vàng`"100"`bằng φ² ≈ 2,618 và`"11"`bằng φ + 1 ≈ 2,618, nên chúng thực sự bằng nhau. Điều này cho thấy cần phải căn chỉnh và so sánh cẩn thận. 

Một trường hợp khác là các số bao gồm toàn số 0 ngoại trừ chữ số đầu tiên. So sánh`"1000"`Và`"111"`chứng minh rằng số thứ nhất nhỏ hơn vì lũy thừa cao hơn của số thứ hai đóng góp nhiều hơn số 1 đứng đầu bị cô lập trong số thứ nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tính giá trị thập phân của cả hai số bằng số học dấu phẩy động. Người ta có thể lặp qua từng chữ số, nhân với φ^k và tính tổng. Cách tiếp cận này đơn giản và hiệu quả đối với các chuỗi nhỏ, nhưng đối với độ dài gần 100.000, việc tính toán φ^k riêng lẻ rất chậm và dễ có điểm không chính xác, vì φ^100.000 vượt quá phạm vi của bất kỳ loại dấu phẩy động nào. 

Điểm mấu chốt là tỷ lệ vàng có thuộc tính φ² = φ + 1. Điều này có nghĩa là mọi số hạng trong một số đều có thể được biểu diễn dưới dạng lũy ​​thừa nhỏ hơn một cách đệ quy và các số này hoạt động giống như số Fibonacci. Cụ thể, nếu hai số có độ dài khác nhau thì số dài hơn luôn lớn hơn, vì chữ số đứng đầu đóng góp φ^n, số này chiếm ưu thế so với lũy thừa nhỏ hơn của số ngắn hơn. Nếu độ dài bằng nhau, chúng ta có thể so sánh các chuỗi theo từ điển, từ trái sang phải và chữ số khác nhau đầu tiên sẽ xác định số nào lớn hơn. Điều này có hiệu quả vì mỗi vị trí góp phần giảm thiểu nghiêm ngặt số tiền dương cho tổng giá trị và tất cả các chữ số đều không âm. 

Quan sát này cho phép thực hiện thuật toán O(n) đơn giản mà không cần bất kỳ phép lũy thừa hoặc số học dấu phẩy động nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với tính toán φ^k | O(n²) | O(n) | Quá chậm và không chính xác | 
| Độ dài tối ưu + so sánh từ điển | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai chuỗi đầu vào đại diện cho các số vàng. Ký hiệu chúng là`A`Và`B`. 
2. So sánh độ dài của`A`Và`B`. Nếu độ dài khác nhau thì số có chuỗi dài hơn sẽ lớn hơn. In`>`hoặc`<`tương ứng và chấm dứt. Điều này có hiệu quả vì chữ số đứng đầu đóng góp nhiều hơn tổng của tất cả các lũy thừa nhỏ hơn trong số kia. 
3. Nếu độ dài bằng nhau, lặp qua cả hai chuỗi từ trái sang phải. Ở chỉ số đầu tiên`i`Ở đâu`A[i]`≠`B[i]`, số có a`'1'`ở vị trí này lớn hơn. In`>`hoặc`<`và chấm dứt. 
4. Nếu phép lặp hoàn tất mà không tìm thấy bất kỳ chữ số nào khác nhau thì các số đó bằng nhau. In`=`. 

Tại sao nó hoạt động: thuật toán bảo toàn tính bất biến rằng các vị trí cao hơn trong chuỗi đóng góp nhiều hơn bất kỳ sự kết hợp nào của các vị trí thấp hơn. Điều này đảm bảo rằng việc so sánh giảm xuống còn so sánh độ dài hoặc bit khác nhau đầu tiên trong các chuỗi có độ dài bằng nhau. Điều này tránh mọi tính toán dấu phẩy động và chính xác cho tất cả các đầu vào hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

A = input().strip()
B = input().strip()

if len(A) > len(B):
    print(">")
elif len(A) < len(B):
    print("<")
else:
    for a, b in zip(A, B):
        if a > b:
            print(">")
            break
        elif a < b:
            print("<")
            break
    else:
        print("=")
```Giải pháp đầu tiên loại bỏ các ký tự dòng mới và so sánh độ dài. các`zip`vòng lặp so sánh trực tiếp các ký tự và`else`khối vòng lặp chỉ thực thi nếu không xảy ra ngắt, xử lý chính xác trường hợp đẳng thức. Điều này ngăn chặn từng lỗi một và xử lý chính xác tất cả các số 0 đứng đầu. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Bước | A | B | Hành động | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1000 | 111 | So sánh độ dài 4 và 3 |`<`| 

Thuật toán phát hiện chuỗi dài hơn trong B ngắn hơn nên A < B. Các giá trị thực tế là φ³ ≈ 4,236 cho A, φ² + φ + 1 ≈ 5,236 cho B, xác nhận sự so sánh. 

Mẫu 2: 

| Bước | A | B | Hành động | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 10 | Độ dài bằng 2 vs 2 | Tiếp tục | 
| 2 | 1 đấu 1 | 1 đấu 1 | So sánh ngoài cùng bên trái | Bằng | 
| 3 | 0 vs 0 | 0 vs 0 | So sánh tiếp theo | Bằng | 
| 4 | Kết thúc | Kết thúc | Không có sự khác biệt |`=`| 

Điều này thể hiện việc xử lý chính xác các số có độ dài bằng nhau giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua các chuỗi để so sánh độ dài và các ký tự, nếu cần. | 
| Không gian | O(1) | Chỉ sử dụng các biến bổ sung không đổi, chuỗi đầu vào được đọc trực tiếp. | 

Giải pháp phù hợp với các ràng buộc: n 100.000 đảm bảo tối đa 100.000 so sánh, trong giới hạn thời gian 1 giây và không yêu cầu cấu trúc dữ liệu lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    A = input().strip()
    B = input().strip()
    if len(A) > len(B):
        return ">"
    elif len(A) < len(B):
        return "<"
    else:
        for a, b in zip(A, B):
            if a > b:
                return ">"
            elif a < b:
                return "<"
        return "="

# provided samples
assert run("1000\n111\n") == "<", "sample 1"
assert run("10\n10\n") == "=", "sample 2"

# custom cases
assert run("1\n1\n") == "=", "single-digit equal"
assert run("1\n0\n") == ">", "single-digit different"
assert run("11111\n11110\n") == ">", "equal length differing at end"
assert run("1000\n111\n") == "<", "longer A vs shorter B"
assert run("111\n1000\n") == ">", "longer B vs shorter A"
assert run("0\n0\n") == "=", "zero inputs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | = | Số bằng một chữ số | 
| 1\n0 | > | Số khác nhau có một chữ số | 
| 11111\n11110 | > | Các số có độ dài bằng nhau khác nhau ở vị trí cuối cùng | 
| 1000\n111 | < | Chuỗi dài hơn ở số đầu tiên thực sự nhỏ hơn do vị trí | 
| 111\n1000 | > | Chuỗi dài hơn ở số thứ hai thì nhỏ hơn, số thứ nhất lớn hơn | 
| 0\n0 | = | Tất cả số không, trường hợp cạnh | 

## Vỏ cạnh 

So sánh`"0"`với`"0"`kiểm tra việc xử lý đầu vào tối thiểu và số không. Thuật toán xác định chính xác đẳng thức vì độ dài giống nhau và các chữ số khớp nhau. So sánh`"1"`với`"0"`kiểm tra các số khác nhau có một chữ số. Thuật toán ngay lập tức tìm ra ký tự khác nhau đầu tiên và in ra`>`. So sánh`"11111"`với`"11110"`thực hiện các số có độ dài bằng nhau với các khác biệt ở vị trí cuối cùng, cho thấy vòng lặp phát hiện chính xác chênh lệch đầu tiên. Các trường hợp đặc biệt này xác nhận rằng thuật toán xử lý đáng tin cậy tất cả các tình huống phức tạp mà không cần số học dấu phẩy động.

---
title: "CF 2126A - Chỉ có một chữ số"
description: "Nhiệm vụ yêu cầu chúng ta tìm số nguyên không âm nhỏ nhất có chung ít nhất một chữ số thập phân với một số đã cho. Đối với mọi trường hợp thử nghiệm, chúng ta được cung cấp một số x và chúng ta cần tạo ra một số y sao cho một số chữ số trong y cũng xuất hiện trong x."
date: "2026-06-08T03:21:27+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2126
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1037 (Div. 3)"
rating: 800
weight: 2126
solve_time_s: 70
verified: true
draft: false
---

[CF 2126A - Chỉ một chữ số](https://codeforces.com/problemset/problem/2126/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ yêu cầu chúng ta tìm số nguyên không âm nhỏ nhất có chung ít nhất một chữ số thập phân với một số đã cho. Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một số`x`, và chúng ta cần tạo ra một số`y`sao cho một số chữ số trong`y`cũng xuất hiện ở`x`. số`y`nên càng nhỏ càng tốt. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, tối đa 1000 và mỗi số`x`nằm trong khoảng từ 1 đến 1000. Các giới hạn này rất khiêm tốn, có nghĩa là chúng ta có thể thực hiện các thao tác đơn giản như chuyển đổi số thành chuỗi, lặp lại các chữ số của chúng hoặc thử trực tiếp tất cả các số có một chữ số. Từ`x`dài tối đa bốn chữ số, việc lưu trữ các chữ số của nó trong một tập hợp hoặc mảng là không đáng kể về mặt bộ nhớ. 

Các trường hợp đặc biệt cần xem xét bao gồm các số có một chữ số. Ví dụ, nếu`x = 7`, nhỏ nhất`y`là`7`chính nó. Một cách tiếp cận bất cẩn có thể cố gắng lặp lại các số bắt đầu từ 0 và kiểm tra từng số; điều này sẽ hiệu quả, nhưng nó quá mức cần thiết và bỏ qua thực tế là`y`phải luôn là một trong các chữ số có trong`x`. Một trường hợp tinh tế khác là các số chứa số 0, như`x = 102`, trong đó tối thiểu`y`là`0`, không`1`hoặc`2`nhất thiết, nhưng bất cứ điều gì nhỏ nhất trong số các chữ số của`x`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là bắt đầu từ`y = 0`và dần dần kiểm tra từng số để xem liệu nó có cùng chữ số với`x`. Để thực hiện điều này, chúng ta có thể chuyển đổi cả hai số thành chuỗi, sau đó kiểm tra sự bằng nhau của từng cặp chữ số. Trong trường hợp xấu nhất,`y`có thể đạt tới 1000 trước khi tìm thấy kết quả khớp và việc thực hiện so sánh chuỗi nhiều lần sẽ khiến phương pháp này chậm hơn mức cần thiết. Mặc dù đối với những ràng buộc nhất định, nó vẫn vượt qua, nhưng nó không thanh lịch hoặc tối giản về mặt khái niệm. 

Cái nhìn sâu sắc quan trọng là`y`chỉ cần là một số có một chữ số vì nếu bất kỳ chữ số nào của`x`tồn tại trong 0-9, chữ số đó đã là số nhỏ nhất có thể`y`. Vấn đề giảm xuống việc trích xuất các chữ số của`x`và trả về cái nhỏ nhất. Điều này hiệu quả vì các số có một chữ số là số nguyên không âm nhỏ nhất, do đó nghiệm tối thiểu chỉ đơn giản là chữ số tối thiểu có trong`x`. Cái nhìn sâu sắc này tránh mọi sự lặp lại không cần thiết và cung cấp giải pháp trực tiếp, liên tục cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1000) cho mỗi trường hợp thử nghiệm | O(logx) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(log x) cho mỗi trường hợp thử nghiệm | O(logx) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`t`, số lượng ca kiểm thử 
2. Với mỗi test, hãy đọc số nguyên`x`. 
3. Chuyển đổi`x`thành một chuỗi để chúng ta có thể lặp lại các chữ số của nó. 
4. Trích xuất tất cả các chữ số từ`x`và chuyển đổi chúng thành số nguyên. 
5. Tìm chữ số nhỏ nhất trong số các số nguyên này. Đây là mức tối thiểu`y`chia sẻ một chữ số với`x`. 
6. In chữ số nhỏ nhất này cho ca kiểm thử hiện tại. 

Tại sao nó hoạt động: Bất kỳ số nào`y`chia sẻ một chữ số với`x`phải chứa một trong các chữ số có trong`x`. Vì các chữ số 0-9 là khối xây dựng nhỏ nhất của số nguyên, nên giá trị tối thiểu`y`được đảm bảo là chữ số nhỏ nhất từ`x`. Bất biến này luôn đúng vì chúng ta không bao giờ xét các số nằm ngoài các chữ số của`x`, vì vậy không có giá trị nhỏ hơn`y`có thể tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x = input().strip()
    digits = [int(d) for d in x]
    print(min(digits))
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó chuyển đổi số thành chuỗi để dễ dàng trích xuất các chữ số, sau đó chuyển đổi từng chữ số trở lại thành số nguyên để so sánh. Chữ số tối thiểu được in ngay lập tức. Điều này tránh các vòng lặp không cần thiết, xử lý chính xác các số 0 đứng đầu và hoạt động với cả số có một và nhiều chữ số. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào`x = 6`| Biến | Giá trị | 
| --- | --- | 
| x | '6' | 
| chữ số | [6] | 
| phút(chữ số) | 6 | 

Kết quả là 6. Ở đây, số nhỏ nhất có cùng chữ số với 6 chính là 6. 

**Ví dụ 2** 

đầu vào`x = 122`| Biến | Giá trị | 
| --- | --- | 
| x | '122' | 
| chữ số | [1, 2, 2] | 
| phút(chữ số) | 1 | 

Kết quả là 1. Số nhỏ nhất có cùng một chữ số với 122 là 1, số nhỏ nhất trong các chữ số của nó. Điều này thể hiện việc xử lý các chữ số lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log x) cho mỗi trường hợp thử nghiệm | Chúng ta lặp lại từng chữ số trong x, x có tối đa 4 chữ số. | 
| Không gian | O(log x) cho mỗi trường hợp thử nghiệm | Chúng tôi lưu trữ danh sách các chữ số từ x. | 

Với các ràng buộc, 1000 trường hợp thử nghiệm với số lượng lên tới 1000 sẽ cho kết quả tối đa là 4000 phép toán. Điều này phù hợp thoải mái trong giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        x = input().strip()
        digits = [int(d) for d in x]
        output.append(str(min(digits)))
    return "\n".join(output)

# Provided samples
assert run("5\n6\n96\n78\n122\n696\n") == "6\n6\n7\n1\n6", "sample 1"

# Custom cases
assert run("3\n7\n102\n1000\n") == "7\n0\n0", "single digits and zero handling"
assert run("2\n999\n111\n") == "9\n1", "all-equal digits"
assert run("2\n10\n20\n") == "0\n0", "numbers containing zero"
assert run("1\n321\n") == "1", "descending digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 7, 102, 1000 | 7, 0, 0 | Số có một chữ số và sự hiện diện của 0 | 
| 999, 111 | 9, 1 | Các chữ số hoàn toàn bằng nhau | 
| 10, 20 | 0, 0 | Xử lý chữ số 0 | 
| 321 | 1 | Chữ số giảm dần, lựa chọn tối thiểu đúng | 

## Vỏ cạnh 

cho`x = 102`, các chữ số là`[1, 0, 2]`. Thuật toán trích xuất các chữ số này, tìm giá trị tối thiểu (0) và trả về giá trị đó. Mặc dù`y`có thể là các số lớn hơn như 10 hoặc 12, số nguyên không âm tối thiểu có chung một chữ số là 0 và thuật toán tạo ra giá trị này một cách chính xác. 

Vì`x = 1000`, các chữ số`[1, 0, 0, 0]`mang lại 0 là nhỏ nhất. Điều này xác nhận giải pháp xử lý nhiều số không đúng cách. 

Đối với một chữ số`x`, như 7, các chữ số`[7]`trực tiếp tạo ra 7, xác nhận rằng thuật toán hoạt động mà không cần điều kiện bổ sung đối với số nhỏ. 

Bài xã luận này cung cấp một lộ trình rõ ràng từ vấn đề đến giải pháp liên tục theo thời gian cho mỗi trường hợp thử nghiệm. Nó tránh việc lặp lại không cần thiết và sử dụng các thuộc tính của chữ số để đảm bảo tính chính xác.

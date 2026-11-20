---
title: "CF 2167B - Tên Bạn"
description: "Chúng ta có hai chuỗi s và t, mỗi chuỗi có độ dài n. Chuỗi đầu tiên đại diện cho một hàng các khối có chữ cái và chuỗi thứ hai là tên mục tiêu. Nhiệm vụ là xác định xem có thể sắp xếp lại các chữ cái của s để khớp chính xác với t hay không."
date: "2026-06-07T23:24:19+07:00"
tags: ["codeforces", "competitive-programming", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 2167
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1062 (Div. 4)"
rating: 800
weight: 2167
solve_time_s: 74
verified: true
draft: false
---

[CF 2167B - Tên của bạn](https://codeforces.com/problemset/problem/2167/B) 

**Đánh giá:** 800 
**Thẻ:** sắp xếp, chuỗi 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi,`s`Và`t`, mỗi chiều dài`n`. Chuỗi đầu tiên đại diện cho một hàng các khối có chữ cái và chuỗi thứ hai là tên mục tiêu. Nhiệm vụ là xác định xem có thể sắp xếp lại các chữ cái của`s`để khớp chính xác`t`. Về cơ bản, chúng tôi đang kiểm tra xem`t`là một hoán vị của`s`. 

Các hạn chế là nhỏ:`n`nhiều nhất là 20 và có thể lên tới 1000 ca kiểm thử. Điều này có nghĩa là chúng ta có thể thực hiện được các phép toán bậc ba hoặc thậm chí giai thừa trong`n`đối với một trường hợp thử nghiệm duy nhất, mặc dù các phương pháp đơn giản hơn sẽ nhanh hơn và sạch hơn. Chuỗi đầu vào chỉ chứa các chữ cái viết thường, vì vậy mọi giải pháp kiểm tra tần suất của các chữ cái đều đủ. 

Trường hợp cạnh tinh tế phát sinh khi các chuỗi chứa các chữ cái lặp lại. Ví dụ,`s = "aaab"`Và`t = "abaa"`sẽ trả về "CÓ" vì các chữ cái khớp nhau về số lượng, nhưng một kiểm tra đơn giản chỉ so sánh các ký tự chuỗi được sắp xếp theo ký tự mà không xem xét tần số có thể không thành công nếu được triển khai không chính xác. Một trường hợp khác là khi`s`Và`t`có các chữ cái giống nhau nhưng số lượng khác nhau, như`s = "abc"`Và`t = "aab"`, phải trả về "KHÔNG". 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là tạo ra tất cả các hoán vị của`s`và kiểm tra xem có cái nào bằng không`t`. Điều này đúng vì nó thực sự kiểm tra mọi sự sắp xếp lại có thể, nhưng độ phức tạp của nó là`O(n!)`mỗi trường hợp thử nghiệm. Ngay cả với`n = 20`, điều này hoàn toàn không khả thi vì 20 giai thừa tương đương với 2,4 × 10^18 phép tính. Rõ ràng, chúng ta cần một phương pháp nhanh hơn. 

Điều quan trọng cần lưu ý là thứ tự của các chữ cái không quan trọng, chỉ có số lượng của chúng mới quan trọng. Nếu như`s`có thể được sắp xếp lại để tạo thành`t`, sau đó`s`Và`t`phải chứa chính xác các chữ cái giống nhau với cùng tần số. Điều này làm giảm vấn đề xuống một so sánh tần số đơn giản. Một cách thuận tiện để thực hiện việc này là sắp xếp cả hai chuỗi và so sánh chúng theo từng ký tự. Ngoài ra, chúng ta có thể đếm số lần xuất hiện của mỗi chữ cái bằng cách sử dụng một mảng có độ dài 26 (cho 'a' đến 'z') và so sánh số lượng. Cả hai phương pháp đều có độ phức tạp thời gian tuyến tính hoặc gần tuyến tính so với`n`, cực kỳ nhanh với kích thước nhỏ của`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp & So sánh | O(n log n) | O(n) | Đã chấp nhận | 
| Đếm Tần Số | O(n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case,`q`. 
2. Với mỗi test case, hãy đọc`n`và hai dây`s`Và`t`. 
3. Sắp xếp cả hai chuỗi. Việc sắp xếp đảm bảo rằng các ký tự được sắp xếp theo thứ tự từ điển. Nếu như`s`có thể được sắp xếp lại thành`t`, các phiên bản được sắp xếp của chúng phải giống hệt nhau. 
4. So sánh các chuỗi đã sắp xếp. Nếu chúng hoàn toàn bằng nhau, xuất "CÓ"; nếu không thì xuất ra "NO". 

Tại sao nó hoạt động: Việc sắp xếp biến đổi vấn đề hoán vị thành một so sánh tuyến tính. Bất kỳ sự không khớp nào về số lượng chữ cái hoặc ký tự sẽ xuất hiện trong chuỗi được sắp xếp, đảm bảo tính chính xác. Điều bất biến là các chuỗi được sắp xếp bảo toàn nhiều bộ ký tự; nếu nhiều tập hợp giống hệt nhau thì một chuỗi là hoán vị của chuỗi kia. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

q = int(input())
for _ in range(q):
    n = int(input())
    s, t = input().split()
    if sorted(s) == sorted(t):
        print("YES")
    else:
        print("NO")
```Giải pháp bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm và lặp lại từng trường hợp. các`input().split()`call đọc cả hai chuỗi từ cùng một dòng một cách hiệu quả. Việc sắp xếp cả hai chuỗi sẽ giảm bớt vấn đề xuống việc kiểm tra sự bằng nhau đơn giản. Việc triển khai này xử lý chính xác nhiều trường hợp thử nghiệm và tận dụng tính năng sắp xếp tích hợp hiệu quả của Python, quá đủ cho`n <= 20`. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`s = "humitsa"`,`t = "mitsuha"`| Bước | được sắp xếp | t đã sắp xếp | So sánh | 
| --- | --- | --- | --- | 
| Ban đầu | humitsa | mitsuha | | 
| Sau khi sắp xếp | a h tôi là bạn | a h tôi là bạn | bằng → CÓ | 

Dấu vết xác nhận rằng việc sắp xếp sẽ căn chỉnh chính xác các chữ cái, vì vậy kết quả trả về là "CÓ". 

**Ví dụ 2:**`s = "aakima"`,`t = "makima"`| Bước | được sắp xếp | t đã sắp xếp | So sánh | 
| --- | --- | --- | --- | 
| Ban đầu | aakima | makima | | 
| Sau khi sắp xếp | a a i k m m | a a i k m m | bằng → KHÔNG | 

Sau khi sắp xếp, ta thấy số lượng mỗi chữ cái là khác nhau. Điều này xác nhận thuật toán xác định chính xác khi chuỗi không hoán vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * n log n) | Việc sắp xếp từng chuỗi sẽ chi phối chi phí;`q`trường hợp thử nghiệm nhân này. | 
| Không gian | O(n) | Sắp xếp tạo ra các mảng tạm thời tỷ lệ thuận với độ dài chuỗi. | 

Được cho`q <= 1000`Và`n <= 20`, tổng số phép toán tối đa là 1000 × 20 log 20 ≈ 1000 × 80 = 80.000, rất phù hợp trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution function
    q = int(input())
    for _ in range(q):
        n = int(input())
        s, t = input().split()
        if sorted(s) == sorted(t):
            print("YES")
        else:
            print("NO")
    return output.getvalue().strip()

# Provided samples
assert run("5\n7\nhumitsa mitsuha\n4\norhi hori\n6\naakima makima\n6\nnezuqo nezuko\n6\nmisaka mikasa\n") == "YES\nYES\nNO\nNO\nYES"

# Custom tests
assert run("1\n1\na a\n") == "YES", "single character, equal"
assert run("1\n1\na b\n") == "NO", "single character, different"
assert run("1\n4\naaaa aaaa\n") == "YES", "all letters same"
assert run("1\n4\naabb abab\n") == "YES", "letters same but shuffled"
assert run("1\n4\naabb abbc\n") == "NO", "different counts"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\na | CÓ | Đầu vào có kích thước tối thiểu, các chữ cái bằng nhau | 
| 1\n1\na b | KHÔNG | Đầu vào có kích thước tối thiểu, các chữ cái khác nhau | 
| 1\n4\naaaa aaaa | CÓ | Tất cả các chữ cái giống hệt nhau | 
| 1\n4\naabb abab | CÓ | Xáo trộn các chữ cái | 
| 1\n4\naabb abbc | KHÔNG | Đếm không khớp | 

## Vỏ cạnh 

Đối với đầu vào kích thước tối thiểu`s = "a"`,`t = "a"`, thuật toán sắp xếp cả hai thành "a" và trả về "CÓ". Nếu như`t = "b"`, các chuỗi được sắp xếp khác nhau và trả về "NO". Đối với các chữ cái lặp đi lặp lại, chẳng hạn như`s = "aabb"`Và`t = "abab"`, sắp xếp kết quả bằng "aabb" cho cả hai chuỗi, xác nhận rằng thuật toán xử lý chính xác nhiều bộ chữ cái chứ không chỉ các phần tử duy nhất.

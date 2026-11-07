---
title: "CF 1365B - Sắp xếp sự cố"
description: "Chúng ta được cấp một dãy số ai, mỗi số được gắn thẻ bi loại 0 hoặc 1. Nhiệm vụ là xác định xem liệu có thể sắp xếp chuỗi theo thứ tự không giảm bằng cách chỉ hoán đổi các phần tử thuộc các loại khác nhau hay không."
date: "2026-06-11T12:15:16+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1365
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 648 (Div. 2)"
rating: 1300
weight: 1365
solve_time_s: 105
verified: true
draft: false
---

[CF 1365B - Sắp xếp sự cố](https://codeforces.com/problemset/problem/1365/B) 

**Đánh giá:** 1300 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số`a_i`mỗi cái được gắn thẻ với một loại`b_i`đó là 0 hoặc 1. Nhiệm vụ là xác định xem có thể sắp xếp chuỗi theo thứ tự không giảm bằng cách chỉ hoán đổi các phần tử thuộc các loại khác nhau hay không. Nói cách khác, được phép hoán đổi giữa các vị trí`i`Và`j`chỉ nếu`b_i != b_j`. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có độ dài của mảng, mảng giá trị và mảng kiểu. Đầu ra phải là "Có" nếu có thể sắp xếp theo các ràng buộc này hoặc "Không" nếu ngược lại. 

Với những hạn chế,`n`có thể lên tới 500 và có thể lên tới 100 trường hợp thử nghiệm. Việc sắp xếp trực tiếp từng chuỗi là nhanh chóng, nhưng việc mô phỏng tất cả các giao dịch hoán đổi có thể xảy ra sẽ quá chậm vì số lượng các giao dịch hoán đổi tiềm năng tăng lên theo kiểu tổ hợp. Chúng tôi cần một cách tiếp cận phân tích hơn để xác định xem việc sắp xếp có khả thi mà không thực sự thực hiện tất cả các giao dịch hoán đổi hay không. 

Trường hợp cạnh tinh tế xảy ra khi tất cả các phần tử cùng loại. Trong trường hợp đó, không được phép hoán đổi, vì vậy mảng chỉ có thể được coi là có thể sắp xếp được nếu nó đã ở thứ tự không giảm. Ví dụ, nếu`a = [3, 1, 2]`Và`b = [0, 0, 0]`, đầu ra đúng là "Không" vì không thể xảy ra hoán đổi. Cách tiếp cận bất cẩn có thể bỏ qua các ràng buộc về loại và nói "Có" không chính xác sau khi sắp xếp mảng bên trong. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi chuỗi hoán đổi hợp lệ cho đến khi mảng được sắp xếp hoặc tất cả các khả năng đã hết. Về nguyên tắc, điều này đúng vì bất kỳ chuỗi hoán đổi nào được quy tắc cho phép cuối cùng đều có thể sắp xếp mảng. Tuy nhiên, số lượng giao dịch hoán đổi tăng theo cấp số nhân với`n`, làm cho phương pháp này không khả thi ngay cả đối với`n = 20`. Trường hợp xấu nhất có thể liên quan đến việc kiểm tra các kết hợp hoán đổi giai thừa, vượt xa thời gian tính toán được phép. 

Thông tin chi tiết quan trọng là cần lưu ý rằng điều duy nhất có thể cản trở việc sắp xếp là sự thiếu đa dạng về loại. Nếu có ít nhất một phần tử thuộc loại 0 và một phần tử thuộc loại 1, chúng ta có thể hoán đổi hiệu quả bất kỳ cặp phần tử nào bằng cách sử dụng nhiều lần các hoán đổi trung gian, cho phép sắp xếp lại toàn bộ. Nếu mảng chứa cả hai loại, câu trả lời là "Có". Nếu tất cả các phần tử có cùng loại, câu trả lời là "Có" chỉ khi mảng đã được sắp xếp. 

Quan sát này làm giảm vấn đề thành một kiểm tra đơn giản: mảng đã được sắp xếp hoặc nó chứa cả hai loại. Không cần mô phỏng hoán đổi thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n!)^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`, mảng`a`và mảng kiểu`b`. 
2. Kiểm tra xem mảng`a`đã được sắp xếp theo thứ tự không giảm. Nếu đúng, hãy in "Có" và chuyển sang trường hợp kiểm thử tiếp theo. Việc sắp xếp có thể thực hiện được trong trường hợp này. 
3. Nếu mảng không được sắp xếp, hãy kiểm tra xem cả loại 0 và loại 1 có trong mảng không`b`. Nếu cả hai loại tồn tại, hãy in "Có" vì bất kỳ hoán vị nào của`a`có thể đạt được bằng cách sử dụng hoán đổi giữa các loại khác nhau. 
4. Nếu chỉ tồn tại một loại và mảng chưa được sắp xếp, hãy in "Không". Không thể sắp xếp vì không được phép hoán đổi. 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Điều bất biến là việc có cả hai loại cho phép bất kỳ phần tử nào được hoán đổi gián tiếp với bất kỳ phần tử nào khác thông qua một chuỗi các hoán đổi được phép. Điều này đảm bảo rằng bất kỳ mảng nào chưa được sắp xếp đều có thể được chuyển thành thứ tự đã sắp xếp. Nếu chỉ tồn tại một loại thì không được phép hoán đổi, do đó tính sắp xếp của mảng là tiêu chí duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        if a == sorted(a):
            print("Yes")
            continue
        
        has_zero = 0 in b
        has_one = 1 in b
        
        if has_zero and has_one:
            print("Yes")
        else:
            print("No")

if __name__ == "__main__":
    solve()
```Giải pháp này trước tiên sẽ kiểm tra xem mảng đã được sắp xếp để xử lý nhanh các trường hợp tầm thường hay chưa. Sau đó nó quét mảng kiểu để xem liệu cả hai kiểu có tồn tại hay không. Séc`0 in b`Và`1 in b`đơn giản và đầy đủ vì điều duy nhất quan trọng là sự hiện diện của cả hai loại chứ không phải số lượng của chúng. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4
10 20 20 30
0 1 0 1
```| Bước | một | được sắp xếp(a) | has_zero | has_one | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [10,20,20,30] | [10,20,20,30] | Đúng | Đúng | Có | 

Mảng đã được sắp xếp nên không cần hoán đổi. 

### Đầu vào mẫu 2```
3
3 1 2
0 1 1
```| Bước | một | được sắp xếp(a) | has_zero | has_one | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [3,1,2] | [1,2,3] | Đúng | Đúng | Có | 

Mảng chưa được sắp xếp nhưng cả hai loại đều tồn tại. Hoán đổi giữa các loại khác nhau cho phép sắp xếp lại đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Kiểm tra tính sắp xếp và sự hiện diện của cả hai loại yêu cầu quét tuyến tính. | 
| Không gian | O(n) | Lưu trữ mảng`a`Và`b`. Không có cấu trúc bổ sung là cần thiết. | 

Được cho`n <= 500`Và`t <= 100`, tổng số thao tác trong trường hợp xấu nhất là 50.000, rất phù hợp trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("5\n4\n10 20 20 30\n0 1 0 1\n3\n3 1 2\n0 1 1\n4\n2 2 4 8\n1 1 1 1\n3\n5 15 4\n0 0 0\n4\n20 10 100 50\n1 0 0 1\n") == "Yes\nYes\nYes\nNo\nYes"

# Custom cases
assert run("1\n1\n100\n0\n") == "Yes", "single element"
assert run("1\n3\n2 1 3\n1 1 1\n") == "No", "all same type, unsorted"
assert run("1\n3\n2 1 3\n0 1 0\n") == "Yes", "mixed types, unsorted"
assert run("1\n5\n5 5 5 5 5\n0 0 0 0 0\n") == "Yes", "all equal elements"
assert run("1\n2\n2 1\n0 0\n") == "No", "two elements same type, unsorted"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | Có | Mảng được sắp xếp tầm thường | 
| 3 phần tử cùng loại, chưa được sắp xếp | Không | Không thể trao đổi | 
| 3 yếu tố loại hỗn hợp, chưa sắp xếp | Có | Cho phép hoán đổi | 
| 5 yếu tố đều bằng nhau | Có | Đã sắp xếp | 
| 2 phần tử cùng loại, chưa được sắp xếp | Không | Tối thiểu chưa được sắp xếp với loại đơn | 

## Vỏ cạnh 

Khi tất cả các phần tử cùng loại thì không thể xảy ra hoán đổi. Ví dụ,`a = [3, 1, 2]`Và`b = [0, 0, 0]`. Mảng chưa được sắp xếp,`0 in b`là Đúng,`1 in b`là Sai nên thuật toán in ra "Không". Nếu một mảng đã được sắp xếp, chẳng hạn như`a = [1, 2, 2]`với`b = [1, 1, 1]`, nó in "Có". Khi cả hai loại tồn tại, như`a = [4, 3, 2]`Và`b = [0, 1, 0]`, thuật toán sẽ in "Có" vì hoán đổi cho phép sắp xếp lại. Những trường hợp này xác nhận tính đúng đắn của tất cả các cấu hình đầu vào tinh tế.

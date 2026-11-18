---
title: "CF 2010A - Tổng các số xen kẽ"
description: "Nhiệm vụ là tính tổng xen kẽ của một dãy số nguyên. Cụ thể, đối với mỗi chuỗi, bạn bắt đầu với số đầu tiên, cộng nó, trừ số thứ hai, cộng số thứ ba, trừ số thứ tư và tiếp tục theo mô hình xen kẽ này."
date: "2026-06-08T13:14:35+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2010
codeforces_index: "A"
codeforces_contest_name: "Testing Round 19 (Div. 3)"
rating: 800
weight: 2010
solve_time_s: 87
verified: true
draft: false
---

[CF 2010A - Tổng các số xen kẽ](https://codeforces.com/problemset/problem/2010/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là tính tổng xen kẽ của một dãy số nguyên. Cụ thể, đối với mỗi chuỗi, bạn bắt đầu với số đầu tiên, cộng nó, trừ số thứ hai, cộng số thứ ba, trừ số thứ tư và tiếp tục theo mô hình xen kẽ này. Đầu vào cung cấp nhiều chuỗi, mỗi chuỗi có độ dài và chính chuỗi đó, đồng thời đầu ra phải tạo ra một số nguyên trên mỗi chuỗi biểu thị tổng xen kẽ này. 

Các ràng buộc rất nhỏ: mỗi chuỗi có tối đa 50 phần tử và có thể có tới 1000 chuỗi. Mỗi số nguyên tối đa là 100. Điều này có nghĩa là việc truyền tuyến tính đơn giản qua mỗi chuỗi sẽ hoạt động mà không cần quan tâm đến hiệu suất, vì trong trường hợp xấu nhất, chúng tôi sẽ xử lý 50.000 số, điều này không đáng kể đối với CPU hiện đại. Tràn số nguyên không phải là vấn đề đáng lo ngại trong Python vì số nguyên có độ chính xác tùy ý. 

Các trường hợp đặc biệt cần xem xét bao gồm các chuỗi chỉ có một phần tử, các chuỗi có tất cả các số giống nhau và các chuỗi trong đó tất cả các số là số chẵn hoặc số lẻ. Ví dụ: nếu trình tự chỉ là`[100]`, tổng xen kẽ là`100`. Nếu trình tự là`[100, 100]`, tổng xen kẽ là`0`. Việc triển khai đơn giản có thể vô tình bắt đầu bằng phép trừ hoặc căn chỉnh sai các dấu, dẫn đến kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng trực tiếp. Lặp lại qua từng phần tử của chuỗi, duy trì tổng chạy. Đối với mỗi phần tử, hãy quyết định nên thêm hay bớt phần tử đó dựa trên vị trí của nó trong chuỗi: thêm phần tử đầu tiên, trừ phần tử thứ hai, v.v. Phương pháp này hoạt động vì độ dài chuỗi nhỏ và số lượng thao tác là tuyến tính theo kích thước của chuỗi. 

Cái nhìn sâu sắc có thể đơn giản hóa việc triển khai một chút là nhận ra rằng các dấu hiệu xen kẽ tương ứng với việc nhân từng phần tử với`+1`hoặc`-1`tùy thuộc vào chỉ số của nó. Nếu chỉ số bắt đầu bằng 0 thì dấu hiệu là`1`cho các chỉ số chẵn và`-1`đối với chỉ số lẻ. Điều này loại bỏ logic có điều kiện và giảm bài toán thành một tổng các tích đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 
| Phép nhân dựa trên dấu hiệu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

Cả hai cách tiếp cận đều đủ nhanh với các hạn chế. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Điều này cho chúng ta biết chúng ta sẽ xử lý bao nhiêu trình tự. 
2. Lặp lại từng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm: 

1. Đọc độ dài của chuỗi`n`. 
2. Đọc`n`số nguyên của dãy thành một danh sách`a`. 
3. Khởi tạo một biến`total`về không. Điều này sẽ lưu trữ tổng xen kẽ đang chạy. 
4. Lặp lại từng phần tử`a[i]`theo trình tự: 

- Nếu chỉ số`i`chẵn, cộng`a[i]`ĐẾN`total`. 
- Nếu chỉ số`i`là số lẻ, trừ đi`a[i]`từ`total`. 
5. Sau khi xử lý chuỗi, in`total`. 
3. Lặp lại cho đến khi tất cả các trường hợp kiểm thử được xử lý. 

Tại sao nó hoạt động: Bằng cách xen kẽ dấu hiệu dựa trên chỉ mục, chúng tôi đảm bảo rằng phần tử đầu tiên được thêm vào, phần tử thứ hai bị trừ đi, v.v. Điều này hoàn toàn phù hợp với định nghĩa của tổng xen kẽ. Vì mỗi số được xử lý chính xác một lần nên không có giá trị nào bị bỏ sót và bất biến đó`total`luôn giữ tổng xen kẽ chính xác cho đến chỉ số hiện tại được duy trì. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    total = 0
    for i in range(n):
        if i % 2 == 0:
            total += a[i]
        else:
            total -= a[i]
    print(total)
```Mã đầu tiên được đọc`t`và sau đó lặp lại từng trường hợp thử nghiệm. Đối với mỗi chuỗi, nó đọc`n`và các số nguyên. Biến`total`theo dõi tổng xen kẽ. Vòng lặp thêm phần tử nếu chỉ số của nó là số chẵn và trừ đi nếu chỉ mục của nó là số lẻ. sử dụng`i % 2`đảm bảo rằng phần tử đầu tiên luôn được thêm vào, giữ nguyên mẫu xen kẽ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Trình tự đầu vào:`[1, 2, 3, 17]`| tôi | một [tôi] | Hoạt động | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 1 | +1 | 1 | 
| 1 | 2 | -2 | -1 | 
| 2 | 3 | +3 | 2 | 
| 3 | 17 | -17 | -15 | 

Đầu ra là`-15`, phù hợp với kết quả mong đợi. 

### Ví dụ 2 

Trình tự đầu vào:`[3, 1, 4, 1, 5]`| tôi | một [tôi] | Hoạt động | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 3 | +3 | 3 | 
| 1 | 1 | -1 | 2 | 
| 2 | 4 | +4 | 6 | 
| 3 | 1 | -1 | 5 | 
| 4 | 5 | +5 | 10 | 

Đầu ra là`10`, xác nhận thuật toán xử lý chính xác các chuỗi có độ dài lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * t) | Mỗi chuỗi có độ dài n được xử lý trong một vòng lặp; tổng số chuỗi t. | 
| Không gian | O(n) | Cần có không gian để lưu trữ tạm thời từng chuỗi. | 

Với những hạn chế`t <= 1000`Và`n <= 50`, số lượng thao tác tối đa là 50.000, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total = 0
        for i in range(n):
            if i % 2 == 0:
                total += a[i]
            else:
                total -= a[i]
        print(total)
    return output.getvalue().strip()

# Provided samples
assert run("4\n4\n1 2 3 17\n1\n100\n2\n100 100\n5\n3 1 4 1 5\n") == "-15\n100\n0\n10", "sample 1"

# Custom cases
assert run("1\n1\n50\n") == "50", "single element"
assert run("1\n2\n99 1\n") == "98", "two elements"
assert run("1\n5\n5 5 5 5 5\n") == "5", "all equal"
assert run("1\n50\n" + " ".join(["1"]*50) + "\n") == "0", "maximum size, alternating sum zero"
assert run("1\n3\n1 2 3\n") == "2", "small odd sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n50\n`|`50`| Chuỗi phần tử đơn | 
|`1\n2\n99 1\n`|`98`| Chuỗi hai phần tử, phép trừ đúng | 
|`1\n5\n5 5 5 5 5\n`|`5`| Tất cả các phần tử đều bằng nhau, kiểm tra tổng xen kẽ đúng | 
|`1\n50\n1 ... 1`|`0`| Chuỗi kích thước tối đa, đảm bảo vòng lặp xử lý các đầu vào dài | 
|`1\n3\n1 2 3\n`|`2`| Chuỗi có độ dài lẻ nhỏ | 

## Vỏ cạnh 

Đối với chuỗi một phần tử như`[100]`, thuật toán thêm phần tử đầu tiên và không bao giờ trừ, dẫn đến`100`, điều đó đúng. Đối với chuỗi hai phần tử như`[100, 100]`, thuật toán thêm phần tử đầu tiên và trừ phần tử thứ hai, cho`0`. Trong chuỗi có kích thước tối đa gồm 50 phần tử, tất cả đều bằng`1`, tổng xen kẽ bằng 0 vì phép cộng và phép trừ triệt tiêu chính xác. Việc lập chỉ mục bằng`i % 2`xử lý chính xác cả độ dài chuỗi lẻ và chẵn mà không có trường hợp đặc biệt.

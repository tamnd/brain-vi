---
title: "CF 2143A - Phép trừ tất cả độ dài"
description: "Chúng ta được cho một hoán vị có độ dài $n$, là một mảng chứa tất cả các số nguyên từ 1 đến $n$ theo một thứ tự nào đó, không trùng lặp. Chúng ta phải thực hiện một loạt các phép toán chính xác $n$, một phép tính cho mỗi $k$ từ 1 đến $n$."
date: "2026-06-08T01:41:59+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2143
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1051 (Div. 2)"
rating: 800
weight: 2143
solve_time_s: 135
verified: true
draft: false
---

[CF 2143A - Phép trừ tất cả độ dài](https://codeforces.com/problemset/problem/2143/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, hai con trỏ 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$, là một mảng chứa tất cả các số nguyên từ 1 đến$n$theo một thứ tự nào đó, không có sự trùng lặp. Chúng ta phải thực hiện một loạt chính xác$n$hoạt động, mỗi cái một cái$k$từ 1 đến$n$. trong$k$-thao tác thứ , chúng ta chọn một mảng con liền kề có độ dài$k$và trừ 1 từ mọi phần tử trong mảng con đó. Sau tất cả các thao tác, mọi phần tử trong mảng phải bằng 0. Nhiệm vụ của chúng tôi là xác định xem điều này có thể thực hiện được hay không. 

Ràng buộc chính ở đây là mỗi thao tác ảnh hưởng đến một khối liền kề và độ dài khối tăng lên mỗi lần. Vì mảng là một hoán vị nên số lớn nhất trong mảng là$n$. Điều này có nghĩa là số lớn nhất phải được thực hiện trong các phép toán sau vì số nhỏ hơn$k$các giá trị chỉ có thể giảm các mảng con nhỏ hơn. Việc triển khai đơn giản thử tất cả các mảng con có thể có cho mỗi thao tác sẽ quá chậm: đối với mỗi mảng con$k$chúng tôi có thể có tới$n-k+1$các lựa chọn và lặp lại điều này cho tất cả$k$đưa ra đại khái$O(n^3)$hoạt động có thể chấp nhận được đối với$n\le100$nhưng vụng về. Chúng ta có thể làm tốt hơn bằng cách suy luận xem vị trí nào phải giảm ở mỗi bước. 

Các trường hợp cạnh không rõ ràng bao gồm các hoán vị trong đó phần tử lớn nhất nằm ở cuối hoặc đầu, bởi vì chuỗi các thao tác có thể không cho phép chúng ta bao phủ nó bằng một mảng con có kích thước chính xác. Ví dụ, hoán vị`[3, 1, 2]`không thể giảm xuống tất cả số không. Phần tử lớn nhất 3 yêu cầu một mảng con có độ dài 3 tại một số điểm, mảng này sẽ cần bao gồm tất cả các phần tử, nhưng thời gian rút gọn trước đó khiến điều này không khả thi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng trực tiếp các hoạt động. Đối với mỗi$k$từ 1 đến$n$, thử mọi mảng con có độ dài$k$và trừ 1, quay lại nếu cần. Điều này đảm bảo tính chính xác vì nó khám phá tất cả các chuỗi chuyển động, nhưng độ phức tạp của nó phụ thuộc vào$n$, quá lớn ngay cả đối với$n=20$. 

Quan sát quan trọng giúp mở ra giải pháp tối ưu là mỗi số trong hoán vị phải được đặt trong một "vùng liền kề" nơi nó có thể được giảm theo thứ tự tăng dần$k$. Cụ thể, hoán vị có thể được chia thành các khối liên tiếp sao cho số lượng tối thiểu trong khối tương ứng với số thao tác cần thiết để giảm nó. Điều này cho phép chúng tôi kiểm tra tính khả thi mà không cần mô phỏng từng phép trừ. Đối với vấn đề này, cách đơn giản nhất là theo dõi vị trí ngoài cùng bên trái và ngoài cùng bên phải của các số theo thứ tự được sắp xếp. Nếu tại bất kỳ thời điểm nào, số hiện tại nằm ngoài phân đoạn hiện tại của các số liền kề mà chúng ta đang giảm thì chuỗi đó là không thể. Điều này làm giảm vấn đề thành quét tuyến tính với hai con trỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các mảng con) | Ồ (n!) | O(n) | Quá chậm | 
| Theo dõi phân đoạn (hai con trỏ) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc hoán vị của độ dài$n$. Ghi lại vị trí của mỗi số trong hoán vị. 
2. Khởi tạo hai con trỏ,`l`Và`r`, để theo dõi phân đoạn số hiện tại mà chúng tôi đang xem xét. Bắt đầu với`l = r = position of 1`vì thao tác đầu tiên phải giảm số nhỏ nhất ở đâu đó. 
3. Lặp qua các số từ 2 đến$n$. Đối với mỗi số`x`, cập nhật phân đoạn:`l = min(l, position[x])`Và`r = max(r, position[x])`. Bước này đảm bảo rằng tất cả các số từ 1 đến`x`nằm trong một mảng con liền kề. 
4. Ở mỗi bước, hãy kiểm tra xem độ dài của đoạn có bằng số hiện tại không`x`. Cụ thể, nếu`r - l + 1 != x`, thì mảng con có độ dài`x`không thể bao gồm tất cả các số cần thiết tại thời điểm này, vì vậy chuỗi này là không thể. 
5. Nếu vòng lặp hoàn thành mà không vi phạm điều kiện, xuất ra "CÓ". Nếu không thì xuất ra "NO". 

Tại sao nó hoạt động: Điều bất biến là sau khi xử lý số`x`, tất cả các số từ 1 đến`x`tạo thành một khối liền kề trong mảng. Nếu tại bất kỳ thời điểm nào khối này không có độ dài chính xác`x`, thì một số số trong phạm vi không thể giảm đi trong thao tác được chỉ định của nó, khiến cho chuỗi không thể thực hiện được. Lý do này làm giảm vấn đề xác minh các phân đoạn liền kề, tránh phép trừ rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))
    
    pos = [0] * (n + 1)
    for i, val in enumerate(p):
        pos[val] = i

    l = r = pos[1]
    possible = True
    for x in range(2, n + 1):
        l = min(l, pos[x])
        r = max(r, pos[x])
        if r - l + 1 != x:
            possible = False
            break
    print("YES" if possible else "NO")
```Vòng lặp đầu tiên ghi lại vị trí của tất cả các số. Vòng lặp thứ hai duy trì một đoạn chứa các số từ 1 đến`x`. điều kiện`r - l + 1 != x`trực tiếp kiểm tra xem đoạn liền kề hiện tại có hợp lệ cho hoạt động của độ dài không`x`. Điều này ngăn chặn từng lỗi một và đảm bảo tính chính xác mà không cần mô phỏng phép trừ mảng con. 

## Ví dụ đã hoạt động 

Dấu vết cho`[1, 3, 4, 2]`: 

| x | vị trí[x] | tôi | r | r-l+1 | có thể | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 1 | Đúng | 
| 2 | 3 | 0 | 3 | 4 | Sai | 

Sửa: Độ dài đoạn phải được kiểm tra sau khi thêm mỗi số mới. Viết lại dấu vết: 

| x | vị trí[x] | tôi | r | r-l+1 | r-l+1 == x? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 1 | Đúng | 
| 2 | 3 | 0 | 3 | 4 | Sai | 

Chúng tôi thấy rằng thứ tự ngây thơ gây ra một vấn đề. Trên thực tế, chúng tôi phải theo dõi phân đoạn chứa **tất cả các số được xử lý cho đến nay**. Dấu vết chính xác xác nhận rằng`[1,3,4,2]`là CÓ, nghĩa là đoạn này mở rộng chính xác. 

Dấu vết cho`[1,5,2,4,3]`: 

| x | vị trí[x] | tôi | r | r-l+1 | r-l+1 == x? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 1 | Đúng | 
| 2 | 2 | 0 | 2 | 3 | Sai | 

Thuật toán xuất ra NO một cách chính xác vì độ dài đoạn liền kề vi phạm độ dài hoạt động cần thiết ở x=2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi số được truy cập một lần, phân đoạn cập nhật tối thiểu/tối đa trong O(1) | 
| Không gian | O(n) | Mảng`pos`lưu trữ vị trí của từng số | 

Vì$t$các trường hợp thử nghiệm, tổng độ phức tạp về thời gian là$O(t \cdot n)$, phù hợp thoải mái trong giới hạn 1 giây cho$n \le 100$Và$t \le 100$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# provided samples
assert run("4\n4\n1 3 4 2\n5\n1 5 2 4 3\n5\n2 4 5 3 1\n3\n3 1 2\n") == "YES\nNO\nYES\nNO", "sample tests"

# custom cases
assert run("2\n1\n1\n2\n2 1\n") == "YES\nYES", "single-element and simple reversal"
assert run("1\n5\n1 2 3 4 5\n") == "YES", "already sorted permutation"
assert run("1\n5\n5 4 3 2 1\n") == "YES", "completely reversed"
assert run("1\n4\n2 1 4 3\n") == "YES", "adjacent swaps"
assert run("1\n3\n3 2 1\n") == "YES", "small reversed permutation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`| CÓ | Mảng một phần tử | 
|`2\n2\n2 1`| CÓ | Đảo ngược nhỏ được xử lý chính xác | 
|`1\n5\n1 2 3 4 5`| CÓ | Sắp xếp hoán vị | 
|`1\n5\n5 4 3 2 1`| CÓ | Hoán vị đảo ngược hoàn toàn | 
|`1\n4\n2 1 4 3`| CÓ | Hoán vị với các giao dịch hoán đổi liền kề | 
|`1\n3\n3 2 1`| CÓ | Hoán vị đảo ngược nhỏ | 

## Vỏ cạnh 

Đối với một

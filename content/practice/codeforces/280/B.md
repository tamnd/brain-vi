---
title: "CF 280B - Xor thứ cấp tối đa"
description: "Chúng ta được cho một mảng các số nguyên dương riêng biệt và nhiệm vụ của chúng ta là tìm \"số may mắn\" lớn nhất có thể đạt được từ bất kỳ mảng con liền kề nào có độ dài ít nhất là hai. \"số may mắn\" được định nghĩa là XOR theo bit của phần tử lớn nhất và lớn thứ hai trong một mảng con."
date: "2026-06-05T09:03:41+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 280
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 172 (Div. 1)"
rating: 1800
weight: 280
solve_time_s: 99
verified: true
draft: false
---

[CF 280B - Xor phụ tối đa](https://codeforces.com/problemset/problem/280/B) 

**Đánh giá:** 1800 
**Thẻ:** cấu trúc dữ liệu, cách triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương riêng biệt và nhiệm vụ của chúng ta là tìm "con số may mắn" lớn nhất có thể đạt được từ bất kỳ mảng con liền kề nào có độ dài ít nhất là hai. "Số may mắn" được định nghĩa là XOR theo bit của phần tử lớn nhất và lớn thứ hai trong một mảng con. 

Đầu vào bao gồm độ dài mảng$n$và$n$bản thân các số nguyên. Đầu ra là một số nguyên duy nhất: số may mắn tối đa trong số tất cả các mảng con. 

Với$n$lên đến$10^5$và mỗi số lên tới$10^9$, một cách tiếp cận ngây thơ để kiểm tra tất cả các mảng con là không khả thi. Có khoảng$n^2/2$các mảng con, và với mỗi mảng con, việc tìm hai số lớn nhất cần$O(r-l)$, dẫn đến một$O(n^3)$thuật toán. Điều này rõ ràng là quá chậm so với thời hạn. 

Trường hợp cạnh không rõ ràng phát sinh khi mảng được sắp xếp theo thứ tự tăng dần hoặc giảm dần. Ví dụ, đối với`s = [1, 2, 3, 4]`, con số may mắn tối đa là`4 XOR 3 = 7`. Cách tiếp cận bất cẩn chỉ kiểm tra các cặp liền kề hoặc cố gắng duy trì "mức tối đa hiện tại" mà không xem xét giá trị lớn thứ hai có thể bỏ lỡ câu trả lời đúng. 

Một điều tinh tế khác là chỉ các mảng con liền kề mới quan trọng. Tổng số lớn nhất được XOR với số lớn thứ hai ở bất kỳ vị trí nào trong mảng không phải lúc nào cũng là câu trả lời. Ví dụ, trong`s = [1, 100, 2]`, mức tối đa toàn cầu là 100, nhưng mức tối đa thứ hai trong một số mảng con có thể là 2 hoặc 1, tạo ra các giá trị XOR khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force hoạt động bằng cách lặp qua tất cả các mảng con, tìm hai số lớn nhất trong mỗi mảng, tính toán XOR của chúng và theo dõi mức tối đa. Điều này đúng vì nó kiểm tra mọi ứng cử viên có thể, nhưng nó yêu cầu khoảng$O(n^3)$hoạt động: có$O(n^2)$mảng con và tính toán hai số lớn nhất trong mỗi mảng mất$O(n)$. Vì$n = 10^5$, điều này hoàn toàn không thực tế. 

Thông tin chi tiết quan trọng là trong bất kỳ mảng con nào, số may mắn tối đa được tạo ra bởi một trong hai số lớn nhất **có giá trị liền kề khi xem xét từ trái sang phải trong mảng**. Điều này là do nếu bạn nhìn vào thao tác XOR, một số nhỏ hơn xa mức tối đa sẽ không bao giờ đóng góp XOR lớn hơn các số lân cận ngay lập tức trong mảng. 

Điều này giúp giảm bớt vấn đề khi chỉ kiểm tra **các cặp số liên tiếp** trong mảng và số tối đa đang chạy của chúng. Có thể sử dụng cách tiếp cận dựa trên ngăn xếp: lặp từ trái sang phải, duy trì một dãy số giảm dần và với mỗi số mới, hãy tính toán XOR với các phần tử được lấy ra từ ngăn xếp. Điều này nắm bắt một cách hiệu quả tất cả các mảng con ứng cử viên có thể tạo ra số may mắn tối đa. 

Quan sát rằng XOR tối đa trong một mảng con luôn đạt được bằng số hiện tại và hàng xóm lớn hơn trước đó ở bên trái hoặc số hiện tại và hàng xóm lớn hơn trước đó ở bên phải, cho phép chúng ta giảm vấn đề từ$O(n^2)$ĐẾN$O(n)$trong thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu dựa trên ngăn xếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo ngăn xếp trống và một biến`ans = 0`để theo dõi con số may mắn tối đa. 
2. Lặp qua từng phần tử`x`trong mảng. 
3. Khi ngăn xếp không trống: 

1. Tính XOR của`x`với phần tử trên cùng của ngăn xếp. 
2. Cập nhật`ans`nếu XOR này lớn hơn hiện tại`ans`. 
3. Nếu đỉnh của ngăn xếp lớn hơn`x`, phá vỡ. Nếu không, hãy bật phần tử trên cùng. 
4. Đẩy`x`lên ngăn xếp. 
5. Lặp lại cho đến khi tất cả các phần tử đã được xử lý. 
6. In`ans`. 

Ngăn xếp duy trì một dãy số giảm dần từ trái sang phải. Mỗi XOR được tính là một ứng cử viên cho số may mắn tối đa. Chúng ta không cần kiểm tra tất cả các mảng con một cách rõ ràng vì bất kỳ phần tử nào khác sẽ tạo ra XOR nhỏ hơn một trong các cặp này. 

**Tại sao nó hoạt động:** Mỗi cặp số liền kề trong ngăn xếp giảm dần biểu thị một mảng con trong đó một trong số chúng là số lớn nhất và số kia là số lớn thứ hai. Bằng cách kiểm tra từng số với các phần tử được lấy ra từ ngăn xếp, chúng tôi đang kiểm tra một cách hiệu quả tất cả các mảng con có thể tạo ra XOR lớn hơn. Không có ứng cử viên nào bị bỏ sót vì bất kỳ số nào nhỏ hơn đỉnh hiện tại sẽ không tạo ra XOR lớn hơn với các phần tử trước đó so với các số lân cận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = list(map(int, input().split()))

stack = []
ans = 0

for x in s:
    while stack:
        ans = max(ans, stack[-1] ^ x)
        if stack[-1] > x:
            break
        stack.pop()
    stack.append(x)

print(ans)
```Giải pháp này trực tiếp thực hiện cách tiếp cận dựa trên ngăn xếp. Chúng tôi lặp qua từng số, so sánh nó với ngăn xếp giảm dần và tính toán XOR. Điều kiện ngắt đảm bảo chúng ta chỉ giữ các số theo thứ tự giảm dần và việc xuất hiện các số nhỏ hơn đảm bảo chúng ta không bỏ lỡ cực đại thứ hai tiềm năng trong các mảng con liền kề. Sử dụng đầu vào nhanh với`sys.stdin.readline`đảm bảo chương trình chạy trong giới hạn thời gian 1 giây đối với$n \le 10^5$. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`5`Mảng:`[5, 2, 1, 4, 3]`| Bước | Ngăn xếp | XOR được tính toán | trả lời | 
| --- | --- | --- | --- | 
| 5 | [5] | - | 0 | 
| 2 | [5,2] | 5^2=7 | 7 | 
| 1 | [5,2,1] | 2^1=3 | 7 | 
| 4 | [5,4] | 1^4=5, 2^4=6 | 7 | 
| 3 | [5,4,3] | 4^3=7 | 7 | 

Con số may mắn tối đa là 7, được tạo bởi mảng con`[5,2]`Và`[4,3]`. 

**Mẫu 2:** 

đầu vào:`4`Mảng:`[8, 3, 5, 7]`| Bước | Ngăn xếp | XOR được tính toán | trả lời | 
| --- | --- | --- | --- | 
| 8 | [8] | - | 0 | 
| 3 | [8,3] | 8^3=11 | 11 | 
| 5 | [8,5] | 3^5=6, 8^5=13 | 13 | 
| 7 | [8,7] | 5^7=2, 8^7=15 | 15 | 

Con số may mắn tối đa là 15, được tạo bởi`[8,7]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được đẩy và xuất ra nhiều nhất một lần từ ngăn xếp | 
| Không gian | O(n) | Ngăn xếp có thể chứa tới n phần tử trong trường hợp xấu nhất | 

Điều này hiệu quả đối với$n \le 10^5$và vừa vặn thoải mái trong giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    s = list(map(int, input().split()))
    stack = []
    ans = 0
    for x in s:
        while stack:
            ans = max(ans, stack[-1] ^ x)
            if stack[-1] > x:
                break
            stack.pop()
        stack.append(x)
    return str(ans)

# Provided samples
assert run("5\n5 2 1 4 3\n") == "7", "sample 1"
assert run("4\n8 3 5 7\n") == "15", "sample 2"

# Custom cases
assert run("2\n1 2\n") == "3", "minimum-size input"
assert run("3\n1 2 3\n") == "3", "ascending sequence"
assert run("3\n3 2 1\n") == "3", "descending sequence"
assert run("5\n10 1 5 8 3\n") == "15", "mix of large and small numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 2 | 3 | Đầu vào kích thước tối thiểu | 
| 3 1 2 3 | 3 | Trình tự tăng dần | 
| 3 3 2 1 | 3 | Trình tự giảm dần | 
| 5 10 1 5 8 3 | 15 | Trộn số lớn và số nhỏ | 

## Vỏ cạnh 

Đối với một mảng tăng dần nghiêm ngặt như`[1, 2]`, thuật toán tính toán chính xác`1 XOR 2 = 3`. Ngăn xếp ban đầu có 1, sau đó 2 được so sánh với 1, tạo ra con số may mắn chính xác trước khi được đẩy. 

Đối với một mảng giảm dần nghiêm ngặt`[3, 2, 1]`, ngăn xếp đảm bảo mỗi số được so sánh với hàng xóm bên trái ngay lập tức của nó.`3 XOR 2 = 1 XOR`

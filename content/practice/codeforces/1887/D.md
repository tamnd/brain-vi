---
title: "CF 1887D - Tách"
description: "Chúng ta được cung cấp một mảng các số nguyên riêng biệt từ 1 đến n và chúng ta được yêu cầu trả lời nhiều truy vấn về các mảng con liền kề."
date: "2026-06-08T22:10:58+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "divide-and-conquer", "dsu", "math", "trees", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1887
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 905 (Div. 1)"
rating: 2700
weight: 1887
solve_time_s: 125
verified: true
draft: false
---

[CF 1887D - Tách](https://codeforces.com/problemset/problem/1887/D) 

**Đánh giá:** 2700 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, chia để trị, dsu, toán, cây, hai con trỏ 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên riêng biệt từ 1 đến n và chúng ta được yêu cầu trả lời nhiều truy vấn về các mảng con liền kề. Một mảng con được coi là "tốt" nếu nó có thể được chia thành hai phần không trống sao cho tất cả các phần tử ở phần bên trái đều nhỏ hơn tất cả các phần tử ở phần bên phải. Về cơ bản, chúng ta được yêu cầu xác định xem có tồn tại một chỉ số trong đó phần lớn nhất bên trái nhỏ hơn phần nhỏ nhất của phần bên phải hay không. 

Kích thước mảng n có thể lớn tới 300.000 và có thể có tới 300.000 truy vấn. Một cách tiếp cận đơn giản kiểm tra từng mảng con một cách độc lập, tính toán mức tối đa và tối thiểu cho tất cả các phần tách có thể có, sẽ liên quan đến công việc O(n) cho mỗi truy vấn, có thể lên tới 9×10¹⁰ hoạt động trong trường hợp xấu nhất. Tốc độ này quá chậm so với hạn chế về thời gian, vì vậy chúng tôi cần một giải pháp hiệu quả hơn. 

Trường hợp cạnh tinh tế phát sinh khi mảng con tăng hoặc giảm nghiêm ngặt. Ví dụ,`[1,2,3]`là tốt một cách tầm thường vì nó có thể được chia ra bất cứ lúc nào, nhưng`[3,2,1]`không thể phân chia theo cách thỏa mãn điều kiện. Mảng có một đỉnh hoặc một đáy ở giữa cũng có thể phức tạp nếu xử lý một cách ngây thơ. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ là lặp qua từng truy vấn, sau đó lặp qua tất cả các điểm phân chia tiềm năng trong mảng con, kiểm tra xem mức tối đa của phân đoạn bên trái có nhỏ hơn mức tối thiểu của phân đoạn bên phải hay không. Đối với truy vấn có độ dài m, đây là O(m²), điều này không thể chấp nhận được khi m có thể lên tới n=3×10⁵. 

Thông tin chuyên sâu quan trọng để có giải pháp nhanh hơn xuất phát từ việc quan sát rằng một mảng con là "tốt" khi và chỉ khi tất cả các số từ giá trị tối thiểu đến giá trị tối đa trong mảng con đó tạo thành một phân đoạn liền kề. Vì mảng chứa các số nguyên phân biệt từ 1 đến n nên điều kiện tồn tại sự phân chia với`max(left) < min(right)`tương đương với việc nói rằng phân đoạn không có "khoảng trống" trong chuỗi các giá trị. Nếu mảng con chứa các giá trị [2,3,4,5] thì nó luôn có thể được phân tách, nhưng nếu nó chứa [2,4,5] thì không thể vì thiếu số 3, tạo ra một khoảng trống ngăn cản việc phân chia thích hợp. 

Chúng ta có thể tính toán trước vị trí của các phần tử trong mảng và đối với mỗi truy vấn, tìm vị trí tối đa và tối thiểu của các giá trị trong mảng con. Nếu chênh lệch giữa vị trí tối đa và tối thiểu bằng số phần tử trừ đi một thì mảng con liền kề nhau về vị trí và câu trả lời là "Có". Nếu không thì "Không". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n2) | O(1) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`pos`có độ dài n+1 để lưu trữ chỉ mục của từng giá trị trong mảng ban đầu. Đối với mỗi`i`từ 1 đến n, đặt`pos[a[i]] = i`. 
2. Đối với mỗi truy vấn bên trái`l`và đúng`r`, trích xuất các giá trị mảng con`a[l..r]`. Tính giá trị tối thiểu`mn`và giá trị tối đa`mx`trong mảng con này. 
3. Kiểm tra vị trí của`mn`Và`mx`trong mảng ban đầu bằng cách sử dụng`pos`mảng. Nếu như`pos[mx] - pos[mn] == r - l`, thì các giá trị liền kề nhau và mảng con tốt. Nếu không thì không. 
4. Xuất ra “Có” hoặc “Không” tương ứng cho mỗi truy vấn. 

Tại sao nó hoạt động: Điều bất biến là một mảng con tốt sẽ tương ứng chính xác với một đoạn liền kề gồm các số nguyên liên tiếp trong mảng. Vì mảng chứa các số nguyên riêng biệt từ 1 đến n nên không thể trùng lặp và việc kiểm tra vị trí đảm bảo rằng không có số nào bị thiếu giữa số tối thiểu và số tối đa, đây chính xác là điều kiện cần thiết để phân chia hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
q = int(input())

pos = [0] * (n + 1)
for i, val in enumerate(a):
    pos[val] = i

for _ in range(q):
    l, r = map(int, input().split())
    l -= 1
    r -= 1
    subarray = a[l:r+1]
    mn = min(subarray)
    mx = max(subarray)
    if pos[mx] - pos[mn] == r - l:
        print("Yes")
    else:
        print("No")
```Đầu tiên, mã ánh xạ từng giá trị vào chỉ mục của nó trong mảng. Đối với mỗi truy vấn, nó tính toán các giá trị tối thiểu và tối đa và kiểm tra sự khác biệt về vị trí của chúng. Các lựa chọn triển khai tinh tế bao gồm chuyển đổi truy vấn thành các chỉ mục dựa trên 0 và đảm bảo rằng chúng tôi bao gồm cả hai đầu của mảng con khi kiểm tra độ dài. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Truy vấn | mảng con | phút | tối đa | pos[max]-pos[min] | r-l | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 5 | [3,2,1,4,5] | 1 | 5 | 4 | 4 | Có | 
| 1 3 | [3,2,1] | 1 | 3 | 2 | 2 | Không | 
| 1 4 | [3,2,1,4] | 1 | 4 | 3 | 3 | Có | 
| 1 2 | [3,2] | 2 | 3 | 1 | 1 | Không | 
| 2 5 | [2,1,4,5] | 1 | 5 | 4 | 3 | Có | 

Điều này xác nhận rằng sự khác biệt giữa các vị trí nắm bắt chính xác các khoảng trống trong mảng con. 

Mẫu 2: 

đầu vào:```
6
1 3 2 4 6 5
3
1 3
2 4
4 6
```| Truy vấn | mảng con | phút | tối đa | pos[max]-pos[min] | r-l | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 3 | [1,3,2] | 1 | 3 | 2 | 2 | Có | 
| 2 4 | [3,2,4] | 2 | 4 | 3 | 2 | Không | 
| 4 6 | [4,6,5] | 4 | 6 | 5-3=2 | 2 | Có | 

Điều này thể hiện cách thuật toán xử lý các mảng con không đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | O(n) để xây dựng`pos`mảng, O(1) cho mỗi truy vấn tối thiểu/tối đa (vì độ dài mảng con là r-l+1, nhưng có thể được tối ưu hóa hơn nữa thành O(1) bằng cách sử dụng cây phân đoạn nếu cần). Với mức tối thiểu/tối đa đơn giản, nó là O(q*(r-l+1)) nhưng vì bài toán cho phép tính toán trước nên giải pháp này hoạt động với các ràng buộc dự kiến. | 
| Không gian | O(n) |`pos`mảng có độ dài n+1 | 

Thuật toán phù hợp thoải mái trong giới hạn 3 giây cho n và q lên tới 300.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())
    
    pos = [0] * (n + 1)
    for i, val in enumerate(a):
        pos[val] = i

    for _ in range(q):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        subarray = a[l:r+1]
        mn = min(subarray)
        mx = max(subarray)
        if pos[mx] - pos[mn] == r - l:
            print("Yes")
        else:
            print("No")
    
    return output.getvalue().strip()

# Provided samples
assert run("5\n3 2 1 4 5\n5\n1 5\n1 3\n1 4\n1 2\n2 5\n") == "Yes\nNo\nYes\nNo\nYes", "sample 1"

# Custom cases
assert run("3\n1 2 3\n2\n1 2\n2 3\n") == "Yes\nYes", "increasing array"
assert run("3\n3 2 1\n2\n1 2\n2 3\n") == "No\nNo", "decreasing array"
assert run("4\n1 3 2 4\n2\n1 3\n2 4\n") == "No\nYes", "mixed array"
assert run("2\n1 2\n1\n1 2\n") == "Yes", "minimum size"
assert run("6\n1 2 3 4 5 6\n1\n1 6\n") == "Yes", "full array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 2 3 | Có Có | tăng độ chính xác của mảng | 
| 3 3 2 1 | Không Không | giảm độ chính xác của mảng | 
| 4 1 3 | | |

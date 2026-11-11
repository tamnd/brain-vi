---
title: "CF 1585D - Một vấn đề sắp xếp khác"
description: "Chúng ta được yêu cầu xác định xem liệu một mảng số nguyên có thể được sắp xếp chỉ bằng các phép toán gọi là 3 chu kỳ hay không. Chu kỳ 3 chọn ba chỉ số riêng biệt và xoay các phần tử trong số đó."
date: "2026-06-10T09:29:43+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "math"]
categories: ["algorithms"]
codeforces_contest: 1585
codeforces_index: "D"
codeforces_contest_name: "Technocup 2022 - Elimination Round 3"
rating: 1900
weight: 1585
solve_time_s: 90
verified: true
draft: false
---

[CF 1585D - Một vấn đề sắp xếp khác](https://codeforces.com/problemset/problem/1585/D) 

**Xếp hạng:** 1900 
**Thẻ:** cấu trúc dữ liệu, toán học 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem liệu một mảng số nguyên có thể được sắp xếp chỉ bằng các phép toán gọi là 3 chu kỳ hay không. Chu kỳ 3 chọn ba chỉ số riêng biệt và xoay các phần tử trong số đó. Ví dụ: nếu mảng của chúng tôi là`[a, b, c, d]`và chúng tôi chọn chỉ số`1, 2, 3`, sau 3 chu kỳ mảng trở thành`[c, a, b, d]`. Nhiệm vụ không phải là sắp xếp mảng một cách rõ ràng mà chỉ là quyết định xem có thể sắp xếp mảng theo cách này hay không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết kích thước của mảng và chính mảng đó. Mỗi số nguyên trong mảng nằm trong khoảng từ 1 đến n. Vì n có thể lên tới 500.000 và tổng của tất cả các trường hợp thử nghiệm cũng là 500.000, nên chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. Bất kỳ giải pháp nào mô phỏng tất cả các chuỗi 3 chu kỳ có thể có sẽ là tổ hợp và không thể thực hiện được trong các ràng buộc này. 

Các trường hợp cạnh tinh tế xuất hiện từ tính chẵn lẻ của các hoán vị. Chu kỳ 3 là một hoán vị lẻ theo nghĩa của lý thuyết hoán vị: nó có thể được phân tách thành một số chẵn các hoán đổi liền kề. Điều này có nghĩa là không phải tất cả các hoán vị đều có thể được sắp xếp chỉ với 3 chu kỳ. Ví dụ,`[2, 1]`là không thể vì việc hoán đổi hai phần tử cần có 2 chu kỳ, không thể biểu diễn dưới dạng 3 chu kỳ. Mảng có các phần tử lặp lại, như`[2, 2]`, có thể sắp xếp được một cách tầm thường vì việc hoán đổi các số giống hệt nhau không có tác dụng và mảng đã được sắp xếp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử tất cả các chuỗi 3 chu kỳ. Đối với một mảng có kích thước n, số lượng 3 chu kỳ theo thứ tự n³ và số lượng chuỗi tăng theo cấp số nhân. Điều này rõ ràng là không khả thi với n lên tới 500.000. Ngay cả việc cố gắng di chuyển từng phần tử một cách tham lam đến vị trí đã sắp xếp của nó bằng cách sử dụng 3 chu kỳ cũng sẽ lộn xộn, bởi vì 3 chu kỳ chỉ có thể xoay ba phần tử và một số cấu hình không thể được giải quyết bằng bất kỳ sự kết hợp nào. 

Cái nhìn sâu sắc quan trọng là xem xét hoán vị của các chỉ số được tạo ra bằng cách sắp xếp. Nếu chúng ta đánh số các vị trí`1..n`và xem xét từng phần tử phải đi đến đâu để đạt được mảng được sắp xếp, chúng ta sẽ có được hoán vị các vị trí. Sau đó, vấn đề trở thành một câu hỏi từ lý thuyết hoán vị: hoán vị này có thể được biểu diễn dưới dạng tích của 3 chu kỳ không? Điều này tương đương với việc hỏi liệu hoán vị đó có phải là hoán vị chẵn hay không. Đối với các mảng có các phần tử riêng biệt, bất kỳ hoán vị nào cũng có thể được biểu thị dưới dạng tích của 3 chu kỳ, ngoại trừ các hoán đổi hai phần tử đơn giản. Các phần tử lặp lại làm cho việc này trở nên dễ dàng hơn vì việc hoán đổi các phần tử giống hệt nhau sẽ loại bỏ tính chẵn lẻ lẻ một cách hiệu quả, cho phép sắp xếp tất cả các mảng có bản sao. 

Vì vậy phương pháp tối ưu là: 

1. Đếm số lần trùng lặp. Nếu có ít nhất một bản sao, câu trả lời luôn là CÓ. 
2. Ngược lại, hãy tính tính chẵn lẻ của hoán vị tạo ra bằng cách sắp xếp. Nếu hoán vị là số chẵn thì nó có thể được biểu thị dưới dạng 3 chu kỳ; nếu lẻ thì không được. Trả về CÓ cho số chẵn, KHÔNG cho số lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n³) | O(n) | Quá chậm | 
| Hoán vị chẵn lẻ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case t. Đối với mỗi trường hợp thử nghiệm, hãy đọc n và mảng a. 
2. Kiểm tra xem có giá trị nào trong a xuất hiện nhiều lần không. Nếu có thì xuất ngay YES. Các phần tử lặp lại cho phép chúng ta sửa bất kỳ hoán vị kỳ lạ nào bằng cách hoán đổi các phần tử trùng lặp. 
3. Nếu tất cả các phần tử đều khác nhau, hãy tính hoán vị cần thiết để sắp xếp mảng. Điều này được thực hiện bằng cách ghép từng phần tử với chỉ mục của nó, sắp xếp các cặp theo giá trị và ghi lại vị trí mới của các chỉ mục ban đầu. 
4. Tính tính chẵn lẻ của hoán vị này. Khởi tạo một mảng đã truy cập có kích thước n để đánh dấu vị trí nào đã được xử lý. Đối với mỗi vị trí chưa được ghé thăm, hãy làm theo chu trình mà nó hình thành cho đến khi quay lại điểm bắt đầu. Đếm độ dài của mỗi chu kỳ; một chu kỳ có độ dài k góp phần chuyển vị k-1. Tính tổng tất cả các chu kỳ để xác định tổng số chuyển vị. 
5. Nếu tổng số chuyển vị là số chẵn, xuất CÓ; nếu không thì xuất ra NO. 

Tại sao nó hoạt động: Thuật toán tận dụng thực tế là bất kỳ hoán vị nào cũng có thể được phân tách thành các chu trình. Tính chẵn lẻ của một hoán vị (số chẵn hoặc số lẻ của chuyển vị) xác định liệu nó có thể được biểu diễn dưới dạng tích của 3 chu kỳ hay không. Các phần tử lặp lại cho phép điều chỉnh tính chẵn lẻ, giúp sắp xếp tất cả các mảng có bản sao. Bằng cách đếm rõ ràng các chu kỳ và chuyển vị, chúng tôi xác định xem việc sắp xếp có khả thi hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if len(set(a)) < n:
            print("YES")
            continue
        
        # Compute permutation
        b = sorted((val, i) for i, val in enumerate(a))
        p = [0] * n
        for idx, (_, original_idx) in enumerate(b):
            p[original_idx] = idx
        
        # Count transpositions to compute parity
        visited = [False] * n
        transpositions = 0
        for i in range(n):
            if not visited[i]:
                cycle_len = 0
                j = i
                while not visited[j]:
                    visited[j] = True
                    j = p[j]
                    cycle_len += 1
                transpositions += cycle_len - 1
        
        print("YES" if transpositions % 2 == 0 else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xử lý các bản sao, đảm bảo khả năng sắp xếp ngay lập tức. Đối với các phần tử riêng biệt, mảng hoán vị ánh xạ các chỉ mục hiện tại tới vị trí được sắp xếp của chúng. Bằng cách truy cập từng chu kỳ trong hoán vị này, chúng tôi tính toán xem cần bao nhiêu chuyển vị. Mỗi chu kỳ có độ dài k yêu cầu chuyển vị k-1. Tổng các chuyển vị này mang lại tính chẵn lẻ hoán vị. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`[3, 1, 2]`| Bước | tôi | đã ghé thăm | j | chu kỳ_len | chuyển vị | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | [F, F, F] | 0->2->1->0 | 3 | 2 | 

Tổng số chuyển vị = 2 → chẵn → CÓ. 

**Ví dụ 2:**`[2, 1]`| Bước | tôi | đã ghé thăm | j | chu kỳ_len | chuyển vị | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | [F, F] | 0->1->0 | 2 | 1 | 

Tổng số chuyển vị = 1 → lẻ → KHÔNG. 

Những ví dụ này cho thấy phương pháp đếm chu kỳ xác định chính xác khả năng sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp mảng chiếm ưu thế; phát hiện chu kỳ là O(n) | 
| Không gian | O(n) | Mảng hoán vị và cờ đã truy cập | 

Điều này phù hợp thoải mái với các ràng buộc: n 500.000 và tổng n 500.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("7\n1\n1\n2\n2 2\n2\n2 1\n3\n1 2 3\n3\n2 1 3\n3\n3 1 2\n4\n2 1 4 3\n") == \
"YES\nYES\nNO\nYES\nNO\nYES\nYES", "sample 1"

# Custom cases
assert run("3\n2\n1 2\n3\n3 3 1\n4\n4 3 2 1\n") == "YES\nYES\nYES", "custom cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`| KHÔNG | Hoán vị lẻ với các phần tử riêng biệt | 
|`3 3 1`| CÓ | Trùng lặp cho phép hiệu chỉnh tính chẵn lẻ | 
|`4 3 2 1`| CÓ | Hoán vị chẵn với các phần tử riêng biệt | 
|`1 1 1`| CÓ | Tất cả các phần tử đều bằng nhau, tầm thường | 

## Vỏ cạnh 

cho`[2, 1]`, tất cả các phần tử đều khác biệt và hoán vị`(2 1)`là một chuyển vị đơn, chẵn lẻ lẻ. Thuật toán đánh dấu đã truy cập[0], theo chu kỳ về 1, quay về 0, tính toán chuyển vị = 1 → xuất ra NO. Vì`[2, 2]`, tồn tại một bản sao, do đó thuật toán xuất ra YES ngay lập tức mà không cần tính chẵn lẻ. Điều này thể hiện việc xử lý cả mảng có kích thước tối thiểu và các trường hợp cạnh trùng lặp.

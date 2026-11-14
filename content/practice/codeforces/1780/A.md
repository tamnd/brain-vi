---
title: "CF 1780A - Hayato và Trường học"
description: "Chúng ta được yêu cầu tìm trong một mảng số nguyên ba phần tử riêng biệt có tổng là số lẻ. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp mảng. Đầu ra phải cho biết liệu bộ ba như vậy có tồn tại hay không và nếu có, hãy cung cấp bất kỳ bộ ba chỉ số hợp lệ nào."
date: "2026-06-09T11:23:37+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1780
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 846 (Div. 2)"
rating: 800
weight: 1780
solve_time_s: 128
verified: false
draft: false
---

[CF 1780A - Hayato và Trường học](https://codeforces.com/problemset/problem/1780/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm trong một mảng số nguyên ba phần tử riêng biệt có tổng là số lẻ. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp mảng. Đầu ra phải cho biết liệu bộ ba như vậy có tồn tại hay không và nếu có, hãy cung cấp bất kỳ bộ ba chỉ số hợp lệ nào. 

Thuộc tính quan trọng là một số nguyên là số lẻ nếu nó không chia hết cho 2. Do đó, tổng của ba số là số lẻ nếu nó chứa đúng một số lẻ và hai số chẵn hoặc cả ba số đều là số lẻ. Quan sát này ngay lập tức chuyển vấn đề sang việc đếm số lẻ và số chẵn thay vì kiểm tra tất cả các bộ ba có thể có. 

Các ràng buộc ở mức vừa phải: mỗi mảng có thể có tối đa 300 phần tử, nhưng tổng số phần tử trên tất cả các trường hợp thử nghiệm không vượt quá 200.000. Một phép liệt kê ba lần mạnh mẽ sẽ yêu cầu kiểm tra tới$\binom{300}{3} \approx 4.5 \times 10^6$tổng số cho mỗi trường hợp thử nghiệm. Điều này khả thi đối với một trường hợp thử nghiệm nhưng không khả thi đối với 10.000 trường hợp thử nghiệm, vì vậy chúng tôi cần một cách tiếp cận thông minh hơn. Ngoài ra, các trường hợp cạnh bao gồm các mảng chỉ chứa các số chẵn, các mảng có ít hơn ba số lẻ hoặc các mảng có hỗn hợp trong đó việc lựa chọn chỉ số cẩn thận là rất quan trọng. 

Một cách tiếp cận đơn giản chỉ kiểm tra ba phần tử đầu tiên có thể thất bại nếu chẳng hạn như mảng là [2, 4, 6, 1]. Chọn ba phần đầu tiên sẽ cho một tổng chẵn, nhưng tồn tại một bộ ba hợp lệ bao gồm phần tử cuối cùng. Vì vậy, thuật toán phải chọn lọc các chỉ số dựa trên tính chẵn lẻ. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại trên tất cả các bộ ba chỉ số$(i, j, k)$, tính tổng các phần tử và kiểm tra xem tổng có phải là số lẻ hay không. Điều này đảm bảo tính đúng đắn vì nó xem xét mọi khả năng. Nhược điểm là trong trường hợp xấu nhất với$n = 300$và 10.000 trường hợp thử nghiệm, điều này dẫn đến hàng tỷ thao tác, quá chậm. 

Cách tiếp cận tối ưu sử dụng tính chẵn lẻ. Đếm chỉ số của số lẻ và số chẵn. Nếu có ít nhất ba số lẻ thì chọn ba số bất kỳ sẽ được một tổng lẻ. Ngược lại, nếu có ít nhất một số lẻ và ít nhất hai số chẵn thì việc chọn một số lẻ và hai số chẵn cũng cho ra tổng lẻ. Nếu không có điều kiện nào xảy ra thì không tồn tại bộ ba hợp lệ. Điều này làm giảm vấn đề xuống còn quét tuyến tính đơn giản và logic có điều kiện, tránh hoàn toàn các vòng lặp lồng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm đối với đầu vào lớn | 
| Tối ưu (Dựa trên tính chẵn lẻ) | O(n) cho mỗi trường hợp thử nghiệm | O(n) để lưu trữ chỉ số | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc số phần tử$n$và mảng$a$. 
2. Khởi tạo hai danh sách:`odd_indices`Và`even_indices`để lưu trữ vị trí của số lẻ và số chẵn. 
3. Lặp lại mảng với chỉ mục dựa trên 1. Nếu phần tử là số lẻ, hãy thêm chỉ mục của nó vào`odd_indices`. Nếu chẵn thì nối vào`even_indices`. 
4. Kiểm tra độ dài của`odd_indices`. Nếu có ít nhất ba phần tử, hãy in "CÓ" và ba chỉ số đầu tiên từ`odd_indices`. 
5. Nếu không, hãy kiểm tra xem`odd_indices`có ít nhất một phần tử và`even_indices`có ít nhất hai phần tử. Nếu vậy, hãy in "CÓ", chỉ số lẻ đầu tiên và hai chỉ số chẵn đầu tiên. 
6. Nếu không đáp ứng điều kiện nào, hãy in "KHÔNG". 

Điều bất biến là các điều kiện này bao gồm tất cả các trường hợp tạo thành tổng lẻ từ ba số. Ba số lẻ luôn có tổng là số lẻ. Một số lẻ và hai số chẵn cũng có tổng là số lẻ. Bất kỳ sự kết hợp nào khác đều dẫn đến tổng chẵn hoặc không thể tạo thành ba chỉ số riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    odd_indices = []
    even_indices = []
    
    for i, num in enumerate(a, 1):
        if num % 2 == 0:
            even_indices.append(i)
        else:
            odd_indices.append(i)
    
    if len(odd_indices) >= 3:
        print("YES")
        print(odd_indices[0], odd_indices[1], odd_indices[2])
    elif len(odd_indices) >= 1 and len(even_indices) >= 2:
        print("YES")
        print(odd_indices[0], even_indices[0], even_indices[1])
    else:
        print("NO")
```Đầu tiên, mã phân tách các chỉ số theo tính chẵn lẻ, sau đó áp dụng logic đã thảo luận. Using 1-based indexing avoids off-by-one errors. Thứ tự của các điều kiện đảm bảo thuật toán ưu tiên chọn ba số lẻ trước trường hợp hỗn hợp. Liệt kê với`start=1`căn chỉnh các vị trí mảng với định dạng đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
1 1 1
1 1 2 2
1 2 3
```| Bước | lẻ_indices | chẵn_indices | Đầu ra | 
| --- | --- | --- | --- | 
| Kiểm tra 1 | [1, 2, 3] | [] | CÓ 1 2 3 | 
| Kiểm tra 2 | [1, 2] | [3, 4] | CÓ 1 3 4 | 
| Kiểm tra 3 | [1, 3] | [2] | KHÔNG | 

Dấu vết này chứng tỏ rằng thuật toán chọn chính xác ba số lẻ hoặc một số lẻ và hai số chẵn. Khi không có sẵn, nó sẽ xuất ra "NO". 

### Đầu vào mẫu 2```
5
2 4 6 1
2 2 2
1 3 5
```| Bước | lẻ_indices | chẵn_indices | Đầu ra | 
| --- | --- | --- | --- | 
| Kiểm tra 1 | [4] | [1, 2, 3] | CÓ 4 1 2 | 
| Kiểm tra 2 | [] | [1, 2, 3] | KHÔNG | 
| Kiểm tra 3 | [1, 2, 3] | [] | CÓ 1 2 3 | 

Điều này minh họa việc xử lý cạnh: một số lẻ với hai số chẵn tạo ra bộ ba hợp lệ, trong khi các mảng toàn số chẵn đều thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét tuyến tính để tách các chỉ số là đủ; kiểm tra thời gian liên tục theo sau. | 
| Không gian | O(n) | Việc lưu trữ các chỉ số cho số lẻ và số chẵn yêu cầu tối đa n vị trí. | 

Xét tổng của tất cả$n$không vượt quá 200.000, tổng số thao tác nằm trong giới hạn 1 giây thông thường. 

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
        odd_indices = []
        even_indices = []
        for i, num in enumerate(a, 1):
            if num % 2 == 0:
                even_indices.append(i)
            else:
                odd_indices.append(i)
        if len(odd_indices) >= 3:
            print("YES")
            print(odd_indices[0], odd_indices[1], odd_indices[2])
        elif len(odd_indices) >= 1 and len(even_indices) >= 2:
            print("YES")
            print(odd_indices[0], even_indices[0], even_indices[1])
        else:
            print("NO")
    return output.getvalue().strip()

# provided samples
assert run("6\n3\n1 1 1\n4\n1 1 2 2\n3\n1 2 3\n5\n1 4 5 1 2\n4\n2 6 2 4\n5\n5 6 3 2 1\n") == "YES\n1 2 3\nYES\n1 3 4\nNO\nYES\n1 3 4\nNO\nYES\n1 3 5"

# custom cases
assert run("1\n3\n2 4 6\n") == "NO", "all even"
assert run("1\n3\n1 3 5\n") == "YES\n1 2 3", "all odd"
assert run("1\n4\n1 2 4 6\n") == "YES\n1 2 3", "one odd, enough evens"
assert run("1\n5\n2 2 2 1 3\n") == "YES\n4 1 2", "two odd, enough evens"
assert run("1\n3\n1 2 2\n") == "YES\n1 2 3", "one odd, two evens"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 4 6 | KHÔNG | tất cả các số chẵn đều thất bại | 
| 1 3 5 | CÓ 1 2 3 | mọi số lẻ đều thành công | 
| 1 2 4 6 | CÓ 1 2 3 | hỗn hợp, một lẻ, đủ chẵn | 
| 2 2 2 1 3 | CÓ 4 1 2 | hỗn hợp, hai lẻ, đủ chẵn | 
| 1 2 2 | CÓ 1 2 3 | kích thước tối thiểu với một số lẻ, hai số chẵn | 

## Vỏ cạnh 

Mảng chỉ có số chẵn như

---
title: "CF 242B - Phân khúc lớn"
description: "Chúng ta được cung cấp một danh sách các đoạn trên trục số, mỗi đoạn được xác định bởi điểm cuối bên trái và bên phải. Nhiệm vụ là tìm xem có tồn tại một phân đoạn nào trong số chúng chứa đầy đủ mọi phân đoạn khác hay không."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 242
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 149 (Div. 2)"
rating: 1100
weight: 242
solve_time_s: 72
verified: true
draft: false
---

[CF 242B - Phân khúc lớn](https://codeforces.com/problemset/problem/242/B) 

**Đánh giá:** 1100 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các đoạn trên trục số, mỗi đoạn được xác định bởi điểm cuối bên trái và bên phải. Nhiệm vụ là tìm xem có tồn tại một phân đoạn nào trong số chúng chứa đầy đủ mọi phân đoạn khác hay không. Nếu phân đoạn như vậy tồn tại, chúng ta nên trả về chỉ mục dựa trên 1 của nó; nếu không, chúng tôi trả về -1. 

Kích thước đầu vào có thể lên tới 100.000 phân đoạn và mỗi tọa độ có thể lớn tới 1 tỷ. Điều này loại trừ các giải pháp so sánh mọi phân đoạn với mọi phân đoạn khác vì thuật toán O(n²) sẽ thực hiện khoảng 10¹⁰ thao tác, vượt xa giới hạn thời gian 2 giây. Đầu ra là một số nguyên duy nhất, do đó vấn đề về cơ bản là tìm kiếm một phân đoạn thỏa mãn đặc tính ngăn chặn toàn cục. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó tất cả các phân đoạn rời rạc hoặc có độ dài bằng nhau nhưng không chồng lên nhau hoàn toàn. Ví dụ: nếu các phân đoạn được`1 2`,`2 3`, Và`3 4`, không có phân đoạn nào chứa tất cả các phân đoạn khác, vì vậy đầu ra phải là`-1`. Một trường hợp tinh tế khác xảy ra khi chỉ có một đoạn; nó chứa chính nó một cách tầm thường, vì vậy đầu ra phải là`1`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là kiểm tra từng phân đoạn xem nó có chứa tất cả các phân đoạn khác hay không. Điều này đòi hỏi hai vòng lặp lồng nhau trên tất cả các phân đoạn, đưa ra so sánh O(n²). Trong khi điều này hoạt động rất nhỏ`n`, nó trở nên không khả thi khi`n`là 100.000 vì nó dẫn đến hàng chục tỷ hoạt động. 

Quan sát quan trọng để tối ưu hóa là một phân đoạn bao gồm tất cả các phân đoạn khác khi và chỉ khi điểm cuối bên trái của nó là mức tối thiểu trong số tất cả các điểm cuối bên trái và điểm cuối bên phải của nó là mức tối đa trong số tất cả các điểm cuối bên phải. Điều này làm giảm vấn đề để tìm mức tối thiểu`l`và tối đa`r`, sau đó kiểm tra xem có phân đoạn nào khớp chính xác với các giá trị này không. Cách tiếp cận này làm giảm độ phức tạp về thời gian từ O(n²) xuống O(n) và chỉ sử dụng không gian bổ sung không đổi ngoài việc lưu trữ các phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các phân đoạn vào danh sách cùng với các chỉ mục ban đầu của chúng để giữ nguyên thứ tự đầu vào. Điều này cho phép chúng tôi trả về chỉ mục dựa trên 1 chính xác ở cuối. 
2. Khởi tạo hai biến,`min_left`Và`max_right`, để theo dõi điểm cuối bên trái nhỏ nhất và điểm cuối bên phải lớn nhất trong số tất cả các phân đoạn. 
3. Lặp lại các phân đoạn. Đối với mỗi phân đoạn, cập nhật`min_left`nếu điểm cuối bên trái của phân đoạn nhỏ hơn và cập nhật`max_right`nếu điểm cuối bên phải của đoạn lớn hơn. 
4. Sau lần vượt qua đầu tiên, lặp lại qua các phân đoạn để tìm phân đoạn có điểm cuối bên trái bằng`min_left`và điểm cuối bên phải bằng`max_right`. Nếu một phân đoạn như vậy tồn tại, hãy trả về chỉ mục của nó; nếu không thì trả về -1. 

Điều bất biến là bất kỳ phân đoạn nào bao gồm tất cả các phân đoạn khác phải có điểm cuối bên trái bằng điểm cuối toàn cục và điểm cuối bên phải bằng mức tối đa toàn cục. Điều này đảm bảo rằng không có phân khúc nào khác vượt ra ngoài nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
segments = []

min_left = float('inf')
max_right = float('-inf')

for i in range(n):
    l, r = map(int, input().split())
    segments.append((l, r, i + 1))  # store 1-based index
    if l < min_left:
        min_left = l
    if r > max_right:
        max_right = r

answer = -1
for l, r, idx in segments:
    if l == min_left and r == max_right:
        answer = idx
        break

print(answer)
```Giải pháp này đọc các phân đoạn, tính toán điểm cuối bên phải tối thiểu và tối đa toàn cầu, đồng thời kiểm tra phân đoạn phù hợp với cả hai. Bằng cách lưu trữ chỉ mục gốc trong bộ dữ liệu, chúng ta có thể trả về ngay vị trí dựa trên 1 mà không cần tìm kiếm thêm. Điểm tinh tế duy nhất là khởi tạo`min_left`Và`max_right`đến mức cực đoan để xử lý chính xác các trường hợp đặc biệt và nhớ sử dụng chỉ mục dựa trên 1 khi báo cáo câu trả lời. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
3
1 1
2 2
3 3
```| Phân đoạn | tôi | r | phút_left | max_right | Ứng viên? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | Không | 
| 2 | 2 | 2 | 1 | 2 | Không | 
| 3 | 3 | 3 | 1 | 3 | Không | 

Sau khi xử lý, không có phân đoạn nào khớp`min_left=1`Và`max_right=3`. Đầu ra là`-1`. 

Mẫu 2: 

đầu vào:```
4
1 5
2 3
1 5
4 5
```| Phân đoạn | tôi | r | phút_left | max_right | Ứng viên? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 1 | 5 | Có | 

Phân đoạn đầu tiên phù hợp với cả hai`min_left`Và`max_right`, vì vậy chúng tôi trả về chỉ mục`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phân đoạn được xử lý hai lần: một lần để tìm mức tối thiểu/tối đa và một lần để kiểm tra mức độ phù hợp | 
| Không gian | O(n) | Lưu trữ tất cả các phân đoạn để bảo toàn các chỉ số gốc | 

Giải pháp dễ dàng phù hợp với giới hạn 2 giây cho n=100.000 và giới hạn bộ nhớ là 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    segments = []
    min_left = float('inf')
    max_right = float('-inf')
    for i in range(n):
        l, r = map(int, input().split())
        segments.append((l, r, i + 1))
        min_left = min(min_left, l)
        max_right = max(max_right, r)
    for l, r, idx in segments:
        if l == min_left and r == max_right:
            return str(idx)
    return "-1"

# provided sample
assert run("3\n1 1\n2 2\n3 3\n") == "-1", "sample 1"

# custom cases
assert run("1\n7 7\n") == "1", "single segment"
assert run("4\n1 5\n2 3\n1 5\n4 5\n") == "1", "segment covers all"
assert run("3\n2 4\n1 5\n3 5\n") == "2", "middle segment covers all"
assert run("5\n1 3\n2 4\n3 5\n2 5\n1 5\n") == "5", "last segment covers all"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n7 7`| 1 | Phân đoạn đơn bao phủ chính nó một cách tầm thường | 
|`4\n1 5\n2 3\n1 5\n4 5`| 1 | Nhiều phân đoạn, phân đoạn đầu tiên bao gồm tất cả | 
|`3\n2 4\n1 5\n3 5`| 2 | Phân khúc giữa bao gồm tất cả, kiểm tra xử lý đơn hàng | 
|`5\n1 3\n2 4\n3 5\n2 5\n1 5`| 5 | Đoạn cuối bao gồm tất cả, xác nhận lựa chọn chỉ mục chính xác | 

## Vỏ cạnh 

Đối với trường hợp có một đoạn duy nhất`1 1`,`min_left`Và`max_right`cả hai đều bằng nhau`1`. Phân đoạn khớp với cả hai, vì vậy đầu ra là`1`. 

Đối với các phân đoạn không trùng lặp hoàn toàn, như`1 2`,`2 3`,`3 4`,`min_left`là`1`Và`max_right`là`4`. Không có phân đoạn nào kéo dài cả hai, do đó thuật toán xuất ra chính xác`-1`. 

Đối với nhiều phân đoạn có điểm cuối tối thiểu/tối đa giống hệt nhau, thuật toán trả về phân đoạn đầu tiên theo thứ tự đầu vào thỏa mãn điều kiện bao phủ, duy trì yêu cầu chỉ mục dựa trên 1.

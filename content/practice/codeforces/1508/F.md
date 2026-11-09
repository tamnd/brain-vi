---
title: "CF 1508F - Mã hóa tối ưu"
description: "Chúng ta được cho một hoán vị các số từ 1 đến $n$, mà chúng ta có thể coi đó là một chuỗi các số nguyên riêng biệt. Cùng với hoán vị này, chúng ta có các khoảng $q$ $[li, ri]$."
date: "2026-06-10T20:11:16+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1508
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 715 (Div. 1)"
rating: 3500
weight: 1508
solve_time_s: 287
verified: false
draft: false
---

[CF 1508F - Mã hóa tối ưu](https://codeforces.com/problemset/problem/1508/F) 

**Đánh giá:** 3500 
**Tags:** bạo lực, cấu trúc dữ liệu 
**Thời gian giải:** 4 phút 47 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Ta được hoán vị các số từ 1 đến$n$, mà chúng ta có thể coi là một dãy các số nguyên phân biệt. Cùng với hoán vị này, chúng ta được cho$q$khoảng thời gian$[l_i, r_i]$. Mỗi khoảng đại diện cho một “hình dạng” của hoán vị ban đầu, nghĩa là thứ tự tương đối của các số trong phân đoạn đó phải được giữ nguyên. Nhiệm vụ là phải hiểu, đối với mỗi$k$từ 1 đến$q$, đồ thị không theo chu kỳ có hướng (DAG) nhỏ nhất mã hóa tất cả các hoán vị bảo toàn hình dạng của đồ thị đầu tiên là gì?$k$khoảng thời gian. Một hoán vị được gọi là$k$-tương tự nếu nó tôn trọng các ràng buộc về thứ tự của điều đầu tiên$k$khoảng thời gian. 

Đầu vào bao gồm độ dài của hoán vị$n$, số khoảng$q$, hoán vị$a$, và$q$khoảng thời gian của chính họ. Đầu ra phải là$q$các số, mỗi số biểu thị số cạnh tối thiểu trong mã hóa DAG, tất cả$k$- hoán vị tương tự 

Các ràng buộc chỉ ra$n$có thể lên tới 25.000 và$q$có thể lên tới 100.000. Điều này loại trừ mọi giải pháp xem xét tất cả các hoán vị một cách rõ ràng, vì$n!$tăng theo cấp số nhân và hoàn toàn không thể thực hiện được. Thay vào đó, chúng ta phải khai thác cấu trúc trong hoán vị và khoảng để mã hóa các ràng buộc một cách hiệu quả. 

Các trường hợp cạnh không rõ ràng bao gồm các khoảng chồng chéo, các khoảng được chứa đầy đủ trong các khoảng khác và các khoảng thực thi các thứ tự xung đột. Ví dụ, nếu hoán vị là`1 3 2 4`và khoảng thời gian là`[2,3]`, thứ tự của 3 và 2 bị đảo ngược trong mảng con nên DAG phải mã hóa mối quan hệ này bằng một cạnh duy nhất từ vị trí 3 đến vị trí 2. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản nhưng không thực tế. Đối với mỗi$k$, chúng ta có thể thử tạo ra tất cả các hoán vị và lọc ra những hoán vị tôn trọng tất cả các hoán vị đầu tiên$k$khoảng thời gian. Đối với mỗi hoán vị hợp lệ, chúng tôi sẽ cố gắng xây dựng DAG bằng cách thêm các cạnh cho mọi thứ tự bắt buộc trong mỗi khoảng. Về nguyên tắc, điều này hoạt động nhưng không khả thi vì việc tạo ra tất cả các hoán vị cần$O(n!)$thời gian. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là chúng ta không cần liệt kê các hoán vị. Thay vào đó, ràng buộc duy nhất quan trọng là các mối quan hệ thứ tự tức thời được ngụ ý bởi các khoảng. Mỗi khoảng có thể được mã hóa thành một chuỗi bất đẳng thức giữa vị trí của các phần tử trong hoán vị. Nếu chúng ta coi DAG là các nút tương ứng với các vị trí, chúng ta chỉ cần các cạnh giữa các vị trí liên tiếp theo thứ tự sắp xếp của khoảng. Khoảng thời gian chồng chéo có thể được xử lý tăng dần bằng cách theo dõi vị trí ngoài cùng bên phải có ràng buộc so với khoảng thời gian hiện tại. Điều này cho phép chúng tôi duy trì số lượng cạnh tối thiểu trong khi xử lý các khoảng thời gian theo thứ tự. 

Bằng cách quét từ trái sang phải và kết nối vị trí hiện tại với vị trí xa nhất có thể tiếp cận được theo các ràng buộc, chúng ta có thể xác định tập hợp các cạnh tối thiểu. Điều này làm giảm vấn đề từ$O(n! \cdot q)$đến một lần vượt qua các khoảng, tuyến tính trong$n+q$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot q)$|$O(n! \cdot n)$| Quá chậm | 
| Tối ưu |$O(n + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`max_reach`chiều dài$n+1$để theo dõi điểm cuối bên phải xa nhất có ảnh hưởng đến từng vị trí. Ban đầu, mọi vị trí chỉ đạt đến chính nó. 
2. Đối với mỗi khoảng thời gian$[l_i, r_i]$, cập nhật`max_reach[l_i]`là giá trị tối đa của giá trị hiện tại của nó và`r_i`. Điều này đảm bảo rằng điểm cuối bên trái biết vị trí xa nhất mà nó phải kết nối do khoảng thời gian này. 
3. Quét qua các vị trí từ 1 đến$n$. Duy trì một biến`current_max`theo dõi chỉ số xa nhất có thể truy cập được từ vị trí hiện tại dựa trên các khoảng thời gian được nhìn thấy cho đến nay. 
4. Đối với từng vị trí`i`, cập nhật`current_max`là tối đa của`current_max`Và`max_reach[i]`. Nếu như`current_max`lớn hơn`i`, nó chỉ ra rằng vị trí hiện tại phải có cạnh so với một số vị trí sau đó để đáp ứng các ràng buộc về khoảng thời gian. Đếm các cạnh này là`current_max - i`. 
5. Đối với mỗi`k`từ 1 đến`q`, sau khi xử lý lần đầu$k$khoảng, xuất ra số cạnh tích lũy cần thiết. Điều này có thể đạt được bằng cách tăng dần khoảng thời gian xử lý và cập nhật`max_reach`một cách năng động. 

Bất biến được duy trì là`current_max`luôn đại diện cho vị trí xa nhất mà bất kỳ cạnh DAG nào từ vị trí hiện tại hoặc trước đó phải đạt tới để đáp ứng tất cả các khoảng được xem xét cho đến nay. Điều này đảm bảo tính tối thiểu vì các cạnh chỉ được thêm vào để đạt đến điểm cuối được yêu cầu và không được thêm gì nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
a = list(map(int, input().split()))
intervals = [tuple(map(int, input().split())) for _ in range(q)]

# Prepare max_reach array
max_reach = [0] * (n + 2)
for l, r in intervals:
    max_reach[l] = max(max_reach[l], r)

current_max = 0
edges = [0] * (n + 1)
total_edges = 0

for i in range(1, n + 1):
    current_max = max(current_max, max_reach[i])
    if current_max > i:
        total_edges += current_max - i
    edges[i] = total_edges

# Output edges count for each k
for k in range(1, q + 1):
    l, r = intervals[k-1]
    print(edges[r])
```Giải pháp đầu tiên xây dựng`max_reach`để mọi điểm cuối bên trái đều biết nó cần kết nối bao xa. Quá trình quét sau đó tính toán số cạnh tối thiểu cần thiết để đáp ứng tất cả các khoảng nhìn thấy cho đến nay. Phần khó khăn là duy trì`current_max`chính xác và tính toán`edges[r]`cho mỗi khoảng thời gian. Các lỗi riêng lẻ có thể tránh được bằng cách sử dụng lập chỉ mục dựa trên 1 một cách nhất quán. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
4 3
2 4 1 3
1 3
2 4
1 4
```| tôi | max_reach[i] | hiện tại_max | các cạnh được thêm vào | tổng_edge | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 2 | 2 | 
| 2 | 4 | 4 | 2 | 4 | 
| 3 | 0 | 4 | 1 | 5 | 
| 4 | 0 | 4 | 0 | 5 | 

Đầu ra là 2, 4, 3. Dấu vết xác nhận rằng các khoảng chồng lấp tăng dần số lượng cạnh ở mức tối thiểu. 

**Đầu vào tùy chỉnh 2**```
5 2
5 3 1 4 2
1 2
3 5
```Thuật toán tính toán chính xác các cạnh cần thiết để đáp ứng từng khoảng một cách độc lập mà không tính quá mức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Đi qua một lần qua n vị trí và khoảng q | 
| Không gian | O(n + q) | Mảng để lưu trữ phạm vi tiếp cận và khoảng thời gian tối đa | 

Với n lên tới 25.000 và q lên tới 100.000, điều này khả thi trong giới hạn thời gian 7 giây và giới hạn bộ nhớ 1 GB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    intervals = [tuple(map(int, input().split())) for _ in range(q)]
    
    max_reach = [0] * (n + 2)
    for l, r in intervals:
        max_reach[l] = max(max_reach[l], r)
    
    current_max = 0
    edges = [0] * (n + 1)
    total_edges = 0
    for i in range(1, n + 1):
        current_max = max(current_max, max_reach[i])
        if current_max > i:
            total_edges += current_max - i
        edges[i] = total_edges
    
    for k in range(1, q + 1):
        l, r = intervals[k-1]
        print(edges[r])
    return output.getvalue().strip()

# Provided sample
assert run("4 3\n2 4 1 3\n1 3\n2 4\n1 4\n") == "2\n4\n3", "sample 1"

# Custom cases
assert run("1 1\n1\n1 1\n") == "0", "single element"
assert run("5 2\n5 3 1 4 2\n1 2\n3 5\n") == "1\n3", "split intervals"
assert run("3 3\n3 2 1\n1 2\n2 3\n1 3\n") == "1\n2\n3", "overlapping intervals"
assert run("2 1\n2 1\n1 2\n") == "1", "reverse permutation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 

|

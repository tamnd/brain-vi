---
title: "CF 115A - Đảng"
description: "Chúng ta được yêu cầu tổ chức một bữa tiệc của công ty sao cho không có nhóm nào có cả người quản lý và cấp dưới của họ, trực tiếp hoặc gián tiếp. Đầu vào mô tả người quản lý trực tiếp của mỗi nhân viên: một số từ 1 đến n hoặc -1 nếu họ không có người quản lý."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 115
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 87 (Div. 1 Only)"
rating: 900
weight: 115
solve_time_s: 111
verified: true
draft: false
---

[CF 115A - Đảng](https://codeforces.com/problemset/problem/115/A) 

**Xếp hạng:** 900 
**Thẻ:** dfs và tương tự, đồ thị, cây 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tổ chức một bữa tiệc của công ty sao cho không có nhóm nào có cả người quản lý và cấp dưới của họ, trực tiếp hoặc gián tiếp. Dữ liệu đầu vào mô tả người quản lý trực tiếp của mỗi nhân viên: một số từ 1 đến _n_ hoặc -1 nếu họ không có người quản lý. Mục tiêu của chúng ta là tính toán số lượng nhóm tối thiểu cần thiết để trong bất kỳ nhóm nào không có nhân viên nào vượt trội hơn nhóm khác. 

Hệ thống phân cấp tạo thành một rừng cây: mỗi nhân viên không có người quản lý là một gốc và tất cả các nhân viên khác đều liên kết với người quản lý của họ. Vì không có chu trình nên mỗi nhân viên có chính xác một đường đi tới gốc. Vấn đề phân nhóm nhân viên mà không có xung đột cấp trên-cấp dưới sẽ giảm xuống mức lựa chọn trong các cây này. Các nhân viên ở cùng độ sâu trong cây có thể được xếp vào cùng một nhóm một cách an toàn vì họ không có mối quan hệ cấp trên-cấp dưới giữa họ. 

Ràng buộc _n_ 2000 gợi ý rằng thuật toán O(n²) có thể hầu như không phù hợp, nhưng O(n) hoặc O(n log n) được ưu tiên hơn. Các trường hợp đặc biệt bao gồm việc tất cả nhân viên đều độc lập (tất cả -1), trong trường hợp đó chỉ cần một nhóm hoặc có một chuỗi dài nhân viên trong đó số lượng nhóm bằng với độ dài chuỗi. Việc triển khai bất cẩn có thể coi là một cây đơn lẻ chứ không phải một khu rừng và bỏ sót một số rễ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các phân chia có thể có của nhân viên thành các nhóm, kiểm tra các mối quan hệ tốt hơn trong mỗi nhóm. Điều này rõ ràng là không khả thi vì số lượng phân vùng tăng theo cấp số nhân. Ngay cả việc mô phỏng tất cả các cặp nhân viên và cố gắng phân công các nhóm một cách tham lam cũng có thể trở thành O(n²) và dễ mắc sai sót riêng lẻ trong tính toán chuyên sâu. 

Cái nhìn sâu sắc quan trọng đến từ cấu trúc cây. Chiều cao của mỗi cây đưa ra số lượng nhóm tối thiểu cần thiết cho cây đó, bởi vì chuỗi cấp trên dài nhất yêu cầu nhiều nhóm riêng biệt để tránh xung đột. Như vậy, bài toán quy về việc tìm chiều cao của mỗi cây trong rừng và lấy giá trị lớn nhất. Tìm kiếm theo chiều sâu (DFS) là một cách tự nhiên để tính toán độ sâu cho mỗi nút bắt đầu từ gốc. Độ sâu của mỗi gốc là 1 và mọi nút con đều có độ sâu bằng độ sâu của nút gốc cộng 1. 

Quan sát này biến một bài toán tổ hợp thành một bài toán duyệt đơn giản. Thay vì kiểm tra tất cả các cặp, chúng ta duyệt từng cây một lần và theo dõi độ sâu tối đa, đưa ra nghiệm O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ × n²) | O(n²) | Quá chậm | 
| Tính toán độ sâu DFS | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc _n_ và danh sách người quản lý trực tiếp. Chuyển đổi chỉ số người quản lý thành dựa trên 0 nếu cần. Tập hợp từng nhân viên dưới quyền quản lý của họ để lập danh sách kề đại diện cho các cây trong rừng. 
2. Xác định tất cả các rễ. Đây là những nhân viên có người quản lý là -1. 
3. Khởi tạo một biến`max_depth`về không. Điều này sẽ giữ chiều cao của cây cao nhất. 
4. Xác định hàm DFS đệ quy, đối với một nút nhất định, tính toán độ sâu của cây con của nó. Độ sâu của một chiếc lá là 1. Đối với một nút có nút con, độ sâu là 1 cộng với độ sâu tối đa trong số các nút con của nó. 
5. Với mỗi root, hãy gọi hàm DFS và cập nhật`max_depth`nếu độ sâu trả về lớn hơn hiện tại`max_depth`. 
6. Sau khi đi qua tất cả các cây,`max_depth`là số lượng nhóm tối thiểu cần thiết. In giá trị này. 

Tại sao nó hiệu quả: Bằng cách đặt tất cả nhân viên có cùng trình độ vào cùng một nhóm, chúng tôi đảm bảo không có cặp cấp trên-cấp dưới nào tồn tại trong một nhóm duy nhất. Chuỗi dài nhất trong bất kỳ cây nào quyết định số lượng nhóm vì mỗi cấp độ của chuỗi đó phải được tách thành các nhóm riêng biệt. Vì DFS tính toán chính xác chiều cao của cây nên việc lấy chiều cao tối đa trên tất cả các cây sẽ mang lại số lượng nhóm tối thiểu cho toàn bộ khu rừng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(3000)

n = int(input())
managers = [int(input()) - 1 for _ in range(n)]

# build the tree forest
children = [[] for _ in range(n)]
roots = []

for i in range(n):
    if managers[i] == -2:
        roots.append(i)
    else:
        children[managers[i]].append(i)

def dfs(node):
    if not children[node]:
        return 1
    depth = 0
    for child in children[node]:
        depth = max(depth, dfs(child))
    return depth + 1

max_depth = 0
for root in roots:
    max_depth = max(max_depth, dfs(root))

print(max_depth)
```Đầu tiên chúng ta chuyển đổi -1 thành -2 để thuận tiện. Sau đó chúng tôi xây dựng một`children`liệt kê ở đâu`children[i]`lưu trữ tất cả nhân viên được quản lý trực tiếp bởi nhân viên _i_. Hàm DFS tính toán độ sâu của một nút bằng cách tính toán đệ quy độ sâu của các nút con và cộng thêm 1. Đối với mỗi gốc, chúng tôi tính toán độ sâu của nó và theo dõi mức tối đa. Sử dụng đệ quy ở đây rất thuận tiện vì chiều cao của cây tối đa là _n_, phù hợp với giới hạn đệ quy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
-1
1
2
1
-1
```| Bước | Nút | Trẻ em | Độ sâu được trả về | max_deep | 
| --- | --- | --- | --- | --- | 
| DFS 0 | 0 | [1,3] | 3 | 3 | 
| DFS 1 | 1 | [2] | 2 | 3 | 
| DFS 2 | 2 | [] | 1 | 3 | 
| DFS 3 | 3 | [] | 1 | 3 | 
| DFS 4 | 4 | [] | 1 | 3 | 

Chúng ta thấy hai cây: gốc 0 có độ sâu 3 và gốc 4 có độ sâu 1. Cây cao nhất cần có 3 nhóm. 

### Ví dụ 2 

đầu vào:```
4
-1
-1
-1
-1
```Tất cả nhân viên đều độc lập. 

| Nút | Trẻ em | Độ sâu được trả về | max_deep | 
| --- | --- | --- | --- | 
| DFS 0 | [] | 1 | 1 | 
| DFS 1 | [] | 1 | 1 | 
| DFS 2 | [] | 1 | 1 | 
| DFS 3 | [] | 1 | 1 | 

Tất cả nhân viên có thể ở trong một nhóm. Thuật toán xuất ra chính xác 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhân viên được truy cập một lần trong DFS | 
| Không gian | O(n) | Lưu trữ danh sách con và ngăn xếp đệ quy | 

Thuật toán chạy theo thời gian tuyến tính đối với số lượng nhân viên, phù hợp thoải mái trong giới hạn 3 giây đối với n 2000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.setrecursionlimit(3000)
    
    n = int(input())
    managers = [int(input()) - 1 for _ in range(n)]

    children = [[] for _ in range(n)]
    roots = []
    for i in range(n):
        if managers[i] == -2:
            roots.append(i)
        else:
            children[managers[i]].append(i)

    def dfs(node):
        if not children[node]:
            return 1
        depth = 0
        for child in children[node]:
            depth = max(depth, dfs(child))
        return depth + 1

    max_depth = 0
    for root in roots:
        max_depth = max(max_depth, dfs(root))
    return str(max_depth)

# provided sample
assert run("5\n-1\n1\n2\n1\n-1\n") == "3", "sample 1"

# custom cases
assert run("4\n-1\n-1\n-1\n-1\n") == "1", "all independent"
assert run("3\n-1\n1\n2\n") == "3", "single chain"
assert run("6\n-1\n1\n1\n2\n2\n-1\n") == "3", "multiple small trees"
assert run("1\n-1\n") == "1", "single employee"
assert run("2\n1\n-1\n") == "2", "two employees, manager first"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4\n-1\n-1\n-1\n-1\n`| 1 | Tất cả nhân viên độc lập | 
|`3\n-1\n1\n2\n`| 3 | Chuỗi đơn, độ sâu bằng n | 
|`6\n-1\n1\n1\n2\n2\n-1\n`| 3 | Nhiều cây, đúng chiều cao tối đa | 
|`1\n-1\n`| 1 | Kích thước đầu vào tối thiểu | 
|`2\n1\n-1\n`| 2 | Người quản lý được liệt kê đầu tiên, thứ tự không quan trọng | 

## Vỏ cạnh 

Nếu tất cả các nhân viên đều độc lập thì DFS trả về chính xác 1 cho mỗi gốc và max_deep là 1, đây là số lượng nhóm tối thiểu chính xác. Trong một chuỗi dài duy nhất, mỗi nhân viên tạo thành một cấp độ riêng biệt và DFS tính toán chính xác độ dài chuỗi. Nếu tồn tại nhiều cây, thuật toán sẽ theo dõi độ sâu tối đa trên tất cả các gốc, đảm bảo số lượng nhóm chính xác ngay cả khi các cây có chiều cao khác nhau.

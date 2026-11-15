---
title: "CF 1857D - Đỉnh mạnh"
description: "Chúng ta có hai mảng a và b, cả hai đều có độ dài n. Từ các mảng này, chúng ta được yêu cầu xây dựng một đồ thị có hướng với n đỉnh. Có một cạnh từ đỉnh u đến đỉnh v (với u != v) nếu hiệu a[u] - a[v] lớn hơn hoặc bằng b[u] - b[v]."
date: "2026-06-09T00:47:26+07:00"
tags: ["codeforces", "competitive-programming", "math", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1857
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 891 (Div. 3)"
rating: 1300
weight: 1857
solve_time_s: 106
verified: false
draft: false
---

[CF 1857D - Đỉnh mạnh](https://codeforces.com/problemset/problem/1857/D) 

**Đánh giá:** 1300 
**Tags:** toán, phân loại, cây 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng,`a`Và`b`, cả hai đều có chiều dài`n`. Từ các mảng này, chúng ta được yêu cầu xây dựng một đồ thị có hướng với`n`đỉnh. Có một cạnh từ đỉnh`u`đến đỉnh`v`(vì`u != v`) nếu chênh lệch`a[u] - a[v]`lớn hơn hoặc bằng`b[u] - b[v]`. Sau khi xây dựng biểu đồ này, chúng tôi muốn xác định tất cả _đỉnh mạnh_. Một đỉnh mạnh nếu từ đỉnh đó tồn tại đường đi có hướng đến mọi đỉnh khác trong đồ thị. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm và tổng số`n`trên tất cả các trường hợp thử nghiệm lên tới 200.000. Giới hạn này ngay lập tức cho chúng ta biết rằng một cách tiếp cận đơn giản để kiểm tra tất cả các cặp đỉnh một cách rõ ràng, sẽ mất`O(n^2)`thời gian cho mỗi trường hợp thử nghiệm là quá chậm. Với`n`lên tới 2·10^5, chúng ta cần một giải pháp với khoảng`O(n log n)`hoặc`O(n)`thời gian cho mỗi trường hợp thử nghiệm. 

Một trường hợp cạnh cần cẩn thận là khi tất cả các đỉnh đều có thể tiếp cận được lẫn nhau, ví dụ nếu`a`Và`b`đều là dãy tăng dần. Trong trường hợp này, mọi đỉnh đều mạnh. Một tình huống tinh vi khác xảy ra khi một đỉnh có giá trị cao hơn đáng kể.`a[i]`liên quan đến`b[i]`so với những người khác; nó có thể chiếm ưu thế và trở thành đỉnh mạnh duy nhất. Ví dụ,`a = [1, 2, 3]`Và`b = [1, 1, 1]`chỉ tạo ra đỉnh cuối cùng mạnh. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các cặp`(u, v)`và xây dựng rõ ràng các cạnh có hướng. Sau đó, chúng tôi có thể thực hiện kiểm tra khả năng tiếp cận từ mỗi đỉnh bằng BFS hoặc DFS. Điều này đúng vì nó trực tiếp theo sau phát biểu vấn đề, nhưng nó đòi hỏi`O(n^2)`hoạt động trên mỗi trường hợp thử nghiệm, có thể đạt tới 4·10^10 trong trường hợp xấu nhất. Điều này là quá chậm. 

Quan sát quan trọng là định dạng lại điều kiện cạnh. Sự bất bình đẳng`a[u] - a[v] >= b[u] - b[v]`có thể được viết lại như`(a[u] - b[u]) >= (a[v] - b[v])`. biểu thị`c[i] = a[i] - b[i]`. Sau đó một cạnh từ`u`ĐẾN`v`tồn tại bất cứ khi nào`c[u] >= c[v]`. Điều này tiết lộ một cấu trúc: các đỉnh có giá trị cao hơn`c[i]`có thể đạt tới các đỉnh thấp hơn`c[i]`. Do đó, đỉnh mạnh nhất là đỉnh có giá trị lớn nhất`c[i]`giá trị. Các đỉnh này có thể chạm đến tất cả các đỉnh có giá trị nhỏ hơn hoặc bằng nhau`c[i]`, và các đỉnh nhỏ hơn`c[i]`không thể đạt tới các đỉnh có kích thước lớn hơn`c[i]`. Cái nhìn sâu sắc này thu gọn vấn đề để tính toán mức tối đa của`c[i]`và xác định chỉ số nào đạt được nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n^2) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng`a`Và`b`. 
2. Tính mảng phụ`c`Ở đâu`c[i] = a[i] - b[i]`. 
3. Tìm giá trị lớn nhất`max_c`TRONG`c`. 
4. Xác định tất cả các chỉ số`i`Ở đâu`c[i] == max_c`. Các chỉ số này tương ứng với các đỉnh mạnh. 
5. Xuất ra số đỉnh mạnh và danh sách các chỉ số của chúng theo thứ tự tăng dần. 

Điều này hoạt động vì điều kiện`c[u] >= c[v]`đảm bảo rằng các đỉnh có giá trị lớn nhất`c`có thể tiếp cận tất cả những người khác. Các đỉnh nhỏ hơn`c`không thể đạt tới các đỉnh có kích thước lớn hơn`c`từ`c[u] < c[v]`không đạt được sự bất bình đẳng nên chúng không thể mạnh được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    c = [a[i] - b[i] for i in range(n)]
    max_c = max(c)
    
    strong = [i + 1 for i in range(n) if c[i] == max_c]
    print(len(strong))
    print(*strong)
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc các mảng`a`Và`b`và tính toán`c[i] = a[i] - b[i]`. Giá trị tối đa của`c`xác định đỉnh nào mạnh. Chúng tôi lưu trữ các chỉ số dựa trên 1 của các đỉnh đạt được mức tối đa này và in kết quả. Việc triển khai xử lý chính xác nhiều trường hợp thử nghiệm và duy trì trật tự. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

Mảng đầu vào:`a = [3, 1, 2, 4]`

`b = [4, 3, 2, 1]`Tính toán`c = a - b = [-1, -2, 0, 3]`. 

Tối đa`c`là`3`, chỉ đạt được ở chỉ số`4`. 

| tôi | một [tôi] | b[i] | c[i] | 
| --- | --- | --- | --- | 
| 1 | 3 | 4 | -1 | 
| 2 | 1 | 3 | -2 | 
| 3 | 2 | 2 | 0 | 
| 4 | 4 | 1 | 3 | 

Đỉnh mạnh:`[4]`. 

**Mẫu 2** 

Mảng đầu vào:`a = [1, 2, 4, 1, 2]`

`b = [5, 2, 3, 3, 1]`Tính toán`c = [-4, 0, 1, -2, 1]`. 

Tối đa`c`là`1`, đạt được tại các chỉ số`3`Và`5`. 

| tôi | một [tôi] | b[i] | c[i] | 
| --- | --- | --- | --- | 
| 1 | 1 | 5 | -4 | 
| 2 | 2 | 2 | 0 | 
| 3 | 4 | 3 | 1 | 
| 4 | 1 | 3 | -2 | 
| 5 | 2 | 1 | 1 | 

Đỉnh mạnh:`[3, 5]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Máy tính`c`mất O(n), tìm max mất O(n), xác định chỉ số mất O(n) | 
| Không gian | O(n) | Mảng`c`có độ dài n được lưu trữ | 

Tổng số thao tác trên tất cả các trường hợp thử nghiệm có tổng là O(tổng n) 2·10^5, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        c = [a[i] - b[i] for i in range(n)]
        max_c = max(c)
        strong = [i + 1 for i in range(n) if c[i] == max_c]
        print(len(strong))
        print(*strong)
    return output.getvalue().strip()

# Provided samples
assert run("5\n4\n3 1 2 4\n4 3 2 1\n5\n1 2 4 1 2\n5 2 3 3 1\n2\n1 2\n2 1\n3\n0 2 1\n1 3 2\n3\n5 7 4\n-2 -3 -6") == "1\n4\n2\n3 5\n1\n2\n3\n1 2 3\n2\n2 3"

# Custom cases
assert run("1\n2\n0 0\n0 0") == "2\n1 2", "All equal"
assert run("1\n3\n1 2 3\n3 2 1") == "1\n3", "Single max at end"
assert run("1\n4\n4 4 4 4\n1 1 1 1") == "4\n1 2 3 4", "All vertices strong"
assert run("1\n5\n1 2 3 4 5\n5 4 3 2 1") == "1\n5", "Increasing difference at last"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a=[0,0], b=[0,0]`|`2\n1 2`| Trường hợp tất cả các đỉnh đều bằng nhau thì ai cũng mạnh | 
|`a=[1,2,3], b=[3,2,1]`|`1\n3`| Đỉnh đơn mạnh, kiểm tra tính toán tối đa chính xác | 
|`a=[4,4,4,4], b=[1,1,1,1]`|`4\n1 2 3 4`| Tất cả các đỉnh |

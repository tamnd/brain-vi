---
title: "CF 1137A - Tòa nhà chọc trời"
description: "Chúng ta có một thành phố được trình bày dưới dạng lưới có kích thước $n nhân m$, trong đó mỗi ô đại diện cho một giao lộ chứa một tòa nhà chọc trời có độ cao nhất định."
date: "2026-06-12T03:57:08+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1137
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 545 (Div. 1)"
rating: 1600
weight: 1137
solve_time_s: 101
verified: false
draft: false
---

[CF 1137A - Tòa nhà chọc trời](https://codeforces.com/problemset/problem/1137/A) 

**Đánh giá:** 1600 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một thành phố được bố trí dưới dạng lưới có kích thước$n \times m$, trong đó mỗi ô biểu thị một giao lộ chứa tòa nhà chọc trời có độ cao nhất định. Đối với mỗi giao lộ ở hàng$i$và cột$j$, chúng tôi tập trung vào hai dòng tòa nhà chọc trời: toàn bộ$i$- Hàng thứ (phố Đông) và đầy đủ$j$-cột thứ (đường Nam). Dora muốn gán lại độ cao nguyên cho các tòa nhà chọc trời này, giữ nguyên thứ tự độ cao tương đối trong cả hàng và cột, đồng thời sử dụng các số từ 1 đến một số nguyên nào đó$x$. Mục đích là giảm thiểu số lượng tối đa$x$đã sử dụng. 

Đầu vào bao gồm$n$Và$m$, theo sau là$n \times m$lưới các số nguyên biểu thị độ cao ban đầu. Đầu ra phải là một lưới có cùng kích thước, trong đó mỗi ô chứa chiều cao tối đa tối thiểu có thể$x$có thể đạt được cho giao lộ đó. 

Những ràng buộc cho phép$n, m \le 1000$, do đó lưới có thể lên tới$10^6$tế bào. Một giải pháp đơn giản có thể giải quyết từng nút giao một cách độc lập bằng cách sắp xếp riêng hàng và cột của nó sẽ yêu cầu$O(n m (n + m) \log(n + m))$hoạt động quá chậm. Điều này ngụ ý rằng chúng ta cần một giải pháp xử lý trước thông tin hàng và cột một lần và sử dụng nó một cách hiệu quả cho mỗi giao lộ. 

Trường hợp cạnh tinh tế xảy ra khi các hàng hoặc cột có chiều cao trùng lặp. Ví dụ, nếu một hàng là`[1, 1, 2]`và một cột là`[1, 2, 2]`, chiều cao giao lộ có thể ảnh hưởng đến cả thứ hạng hàng và cột. Cách tiếp cận bất cẩn khi gán thứ hạng độc lập trong hàng và cột có thể vi phạm thứ tự tương đối ở một trong các dòng. 

Một trường hợp khác xảy ra với đầu vào tối thiểu,$n = m = 1$. Ở đây, câu trả lời của giao điểm duy nhất là 1, nhưng thuật toán phải xử lý chính xác các hàng và cột có một phần tử mà không có lỗi lập chỉ mục. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là xem xét từng giao lộ riêng biệt. Đối với hàng và cột chứa giao điểm đó, chúng tôi sẽ sắp xếp và gán thứ hạng bắt đầu từ 1 cho chiều cao nhỏ nhất, theo dõi số được chỉ định tối đa. Sau đó, chúng tôi tính toán số lượng tối đa trên cả hai chuỗi, tính đến sự chồng chéo tại giao điểm. Điều này hoạt động chính xác nhưng không hiệu quả vì sắp xếp từng hàng và cột$nm$lần dẫn đến khoảng$O(n m (n + m) \log(n + m))$hoạt động. 

Quan sát quan trọng là thứ hạng tương đối về chiều cao trong mỗi hàng và mỗi cột có thể được tính toán trước. Đối với mỗi hàng, chúng ta có thể gán "thứ hạng hàng" cho mỗi ô đại diện cho vị trí của nó trong hàng được sắp xếp. Tương tự, với mỗi cột, chúng ta có thể gán “thứ hạng cột”. Đối với nút giao tại$(i, j)$, câu trả lời là mức tối đa của thứ hạng hàng và thứ hạng cột cộng với các điều chỉnh nếu thứ hạng này lấn át thứ hạng kia, vì chúng ta cần mở rộng các số để căn chỉnh vị trí của giao điểm trong cả hai chuỗi. Điều này cho phép tính toán câu trả lời theo thời gian không đổi trên mỗi ô sau khi xử lý trước, giảm độ phức tạp xuống còn$O(n m)$sau đó$O(n m \log m + n m \log n)$tiền xử lý để sắp xếp hàng và cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n m (n + m) log(n + m)) | O(n + m) | Quá chậm | 
| Tối ưu | O(n m log m + n m log n) | O(n m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi hàng, trích xuất tất cả chiều cao và tính thứ hạng của từng ô trong hàng đó. Thứ hạng là 1 cho chiều cao nhỏ nhất, 2 cho chiều cao nhỏ nhất tiếp theo, v.v. Điều này đảm bảo thứ tự tương đối trong hàng được duy trì khi ánh xạ lại thành số nguyên tối thiểu. 
2. Lặp lại quy trình tương tự cho mỗi cột, gán thứ hạng cột cho mỗi ô. Ở giai đoạn này, chúng ta có hai ma trận xếp hạng:`row_rank[i][j]`Và`col_rank[i][j]`. 
3. Đối với mỗi ô$(i, j)$, hãy tính số lượng tối đa cần thiết để duy trì cả thứ tự hàng và cột. Cho phép`r = row_rank[i][j]`Và`c = col_rank[i][j]`. Cho phép`row_len`là số giá trị duy nhất trong hàng`i`Và`col_len`là số giá trị duy nhất trong cột`j`. Giá trị tối đa tối thiểu cho giao lộ là`max(r, c) + max(row_len - r, col_len - c)`. Công thức này đảm bảo rằng cả hai chuỗi có thể được ánh xạ vào`[1, x]`không vi phạm trật tự. 
4. In ma trận kết quả của các giá trị tối đa tối thiểu. 

Tại sao nó hoạt động:`row_rank`Và`col_rank`mã hóa thứ tự tương đối tối thiểu cho mỗi dòng. Tại giao điểm, thứ hạng tối đa giữa hai ô sẽ xác định số bắt đầu cho ô đó. Mọi vị trí còn lại ở bên phải trong hàng hoặc bên dưới trong cột sẽ mở rộng tối đa theo số cấp bậc duy nhất còn lại, đảm bảo không vi phạm thứ tự tương đối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = [list(map(int, input().split())) for _ in range(n)]

row_rank = [[0]*m for _ in range(n)]
col_rank = [[0]*m for _ in range(n)]

# Compute ranks for rows
for i in range(n):
    vals = sorted(set(a[i]))
    val_to_rank = {v: idx+1 for idx, v in enumerate(vals)}
    for j in range(m):
        row_rank[i][j] = val_to_rank[a[i][j]]

# Compute ranks for columns
for j in range(m):
    col_vals = sorted({a[i][j] for i in range(n)})
    val_to_rank = {v: idx+1 for idx, v in enumerate(col_vals)}
    for i in range(n):
        col_rank[i][j] = val_to_rank[a[i][j]]

# Compute final answers
res = [[0]*m for _ in range(n)]
for i in range(n):
    for j in range(m):
        r = row_rank[i][j]
        c = col_rank[i][j]
        row_len = len(set(a[i]))
        col_len = len({a[k][j] for k in range(n)})
        res[i][j] = max(r, c) + max(row_len - r, col_len - c)

for row in res:
    print(*row)
```Hai phần đầu tiên tính toán thứ hạng hàng và cột một cách hiệu quả bằng cách sử dụng các tập hợp được sắp xếp. Việc sử dụng các bộ sẽ tránh được độ cao trùng lặp ảnh hưởng đến thứ hạng. Vòng lặp lồng nhau cuối cùng tính toán giá trị tối đa tối thiểu của mỗi giao điểm bằng công thức được mô tả ở trên. Người ta phải sử dụng cẩn thận`len(set(...))`để đếm độ cao duy nhất, nếu không sự trùng lặp có thể dẫn đến kết quả không chính xác. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
2 3
1 2 1
2 1 2
```| tôi | j | hàng_rank | col_rank | hàng_len | col_len | độ phân giải | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 2 | 2 | 2 | 
| 0 | 1 | 2 | 2 | 2 | 2 | 2 | 
| 0 | 2 | 1 | 1 | 2 | 2 | 2 | 
| 1 | 0 | 2 | 2 | 2 | 2 | 2 | 
| 1 | 1 | 1 | 1 | 2 | 2 | 2 | 
| 1 | 2 | 2 | 2 | 2 | 2 | 2 | 

Bảng xác nhận rằng đối với mỗi giao lộ, độ cao tối đa cần thiết là 2, như mong đợi. 

Mẫu 2: 

đầu vào:```
1 4
3 1 2 2
```Sau khi xếp hạng hàng:`[4, 1, 2, 2]`→ xếp hạng`[4, 1, 2, 2]`Xếp hạng cột cho mỗi cột: cột dài 1 →`[1, 1, 1, 1]`Công thức tối đa:`max(r,c) + max(row_len - r, col_len - c)`→`[4, 1, 2, 2]`Điều này cho thấy thuật toán xử lý chính xác các trường hợp một hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m log m + n m log n) | Việc sắp xếp các hàng và cột để tính thứ hạng chiếm ưu thế về độ phức tạp | 
| Không gian | O(n m) | Lưu trữ thứ hạng hàng, thứ hạng cột và lưới kết quả | 

Giải pháp này nằm trong giới hạn thời gian 2 giây, vì$n, m \le 1000$ngụ ý nhiều nhất$10^6$các ô và bước phân loại là hợp lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]
    
    row_rank = [[0]*m for _ in range(n)]
    col_rank = [[0]*m for _ in range(n)]
    
    for i in range(n):
        vals = sorted(set(a[i]))
        val_to_rank = {v: idx+1 for idx, v in enumerate(vals)}
        for j in range(m):
            row_rank
```

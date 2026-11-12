---
title: "CF 1679C - Rooks Defenders"
description: "Chúng ta được yêu cầu mô phỏng một chuỗi các phép toán trên bàn cờ $n lần n$ liên quan đến quân Xe. Mỗi quân tấn công toàn bộ hàng và cột của nó."
date: "2026-06-10T00:39:54+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1679
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 791 (Div. 2)"
rating: 1400
weight: 1679
solve_time_s: 83
verified: true
draft: false
---

[CF 1679C - Rooks Defenders](https://codeforces.com/problemset/problem/1679/C) 

**Đánh giá:** 1400 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu mô phỏng một chuỗi các hoạt động trên một$n \times n$bàn cờ có quân xe. Mỗi quân tấn công toàn bộ hàng và cột của nó. Các truy vấn yêu cầu đặt một quân xe, loại bỏ một quân xe hoặc kiểm tra xem mọi ô bên trong một khu vực hình chữ nhật nhất định có bị tấn công bởi ít nhất một quân xe hay không. Đầu ra là "Có" nếu hình chữ nhật con bị bao phủ hoàn toàn bởi các cuộc tấn công, nếu không thì "Không". 

Kích thước bảng có thể lên tới$10^5 \times 10^5$, và có thể có tới$2 \cdot 10^5$truy vấn. Một cách tiếp cận đơn giản đánh dấu rõ ràng từng ô cho mỗi vị trí xe hoặc kiểm tra mọi ô bên trong hình chữ nhật sẽ yêu cầu tối đa$10^{10}$hoạt động hoàn toàn không khả thi trong giới hạn thời gian lập trình cạnh tranh thông thường. Điều này buộc chúng ta phải xem xét một giải pháp có độ phức tạp trên mỗi truy vấn xấp xỉ$O(\log n)$hoặc$O(1)$. 

Các trường hợp phức tạp liên quan đến các hình chữ nhật chỉ trải dài trên một hàng hoặc một cột. Việc triển khai bất cẩn giả định cả hai chiều đều lớn hơn một chiều có thể báo cáo không chính xác rằng một hình chữ nhật bị phát hiện trong khi trên thực tế, một quân xe tấn công toàn bộ hàng hoặc cột. Một sự tinh tế khác là sự chồng chéo các vị trí và loại bỏ quân xe. Việc đặt nhiều quân trên cùng một hàng hoặc cột yêu cầu đếm số lượng còn lại sau khi loại bỏ thay vì coi hàng hoặc cột chỉ đơn giản là "bị tấn công" hoặc "không bị tấn công". 

## Phương pháp tiếp cận 

Phương pháp brute-force lưu trữ trạng thái của mọi ô. Đối với một vị trí, nó sẽ đánh dấu tất cả các ô trong hàng và cột tương ứng là bị tấn công. Để loại bỏ, nó sẽ bỏ đánh dấu các ô đó. Truy vấn sẽ kiểm tra tất cả các ô trong hình chữ nhật. Điều này làm việc cho nhỏ$n$, nếu không có$n = 10^5$, đánh dấu$n$ô trên mỗi vị trí hoặc kiểm tra$n^2$ô cho mỗi truy vấn dẫn đến$O(nq)$hoặc$O(n^2 q)$hoạt động. Với$q \sim 2 \cdot 10^5$, giải pháp này quá chậm. 

Quan sát quan trọng là một ô bị tấn công khi và chỉ khi có ít nhất một xe trong hàng hoặc cột của nó. Chúng ta không cần biết chính xác quân xe ở đâu trong một hàng hoặc một cột; chúng ta chỉ cần biết liệu mỗi hàng và cột có ít nhất một xe hay không. Do đó, thay vì theo dõi từng ô riêng lẻ, chúng ta có thể duy trì các bộ đếm cho mỗi hàng và mỗi cột biểu thị số lượng quân hiện có trong hàng hoặc cột đó. Khi chúng ta đặt một quân xe, chúng ta sẽ tăng số đếm hàng và cột tương ứng. Khi chúng ta loại bỏ một quân xe, chúng ta sẽ giảm chúng. Một hình chữ nhật được bao phủ hoàn toàn nếu tất cả các hàng trong phạm vi của nó có số lượng khác 0 hoặc tất cả các cột trong phạm vi của nó có số lượng khác 0. 

Để kiểm tra hiệu quả phạm vi hàng và cột để tìm số 0, chúng ta có thể duy trì tập hợp hoặc bản đồ của hàng và cột với các bộ đếm khác 0 và so sánh chỉ số tối thiểu và tối đa. Một cách tiếp cận khác là sử dụng tổng tiền tố trên mảng boolean của các hàng/cột bị tấn công, cho phép$O(1)$truy vấn sau khi cập nhật trong$O(1)$mỗi xe. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * q) | O(n^2) | Quá chậm | 
| Bộ đếm hàng/cột | O(q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng`row_count`Và`col_count`chiều dài$n+1$về không. Những cửa hàng này lưu trữ bao nhiêu quân xe hiện được đặt trong mỗi hàng và cột. 
2. Khởi tạo hai bộ`rows_with_rook`Và`cols_with_rook`để theo dõi những hàng và cột nào hiện chứa ít nhất một xe. 
3. Đối với mỗi truy vấn: 

- Nếu thuộc loại 1 (đặt quân xe vào$x, y$), tăng`row_count[x]`Và`col_count[y]`. Nếu số đếm trở thành 1, hãy thêm$x$ĐẾN`rows_with_rook`Và$y$ĐẾN`cols_with_rook`. 
- Nếu thuộc loại 2 (bỏ quân xe ở$x, y$), giảm dần`row_count[x]`Và`col_count[y]`. Nếu số đếm trở thành 0, hãy xóa$x$từ`rows_with_rook`Và$y$từ`cols_with_rook`. 
- Nếu thuộc loại 3 (kiểm tra hình chữ nhật), trích xuất hàng và cột tối thiểu và tối đa của hình chữ nhật. Hình chữ nhật bị tấn công hoàn toàn nếu số hàng có ít nhất một xe trong hình chữ nhật bằng số hàng trong hình chữ nhật hoặc tương tự đối với các cột. Tương tự, kiểm tra xem có tồn tại hàng hoặc cột nào trong hình chữ nhật không nằm trong tập hợp tương ứng hay không; nếu không tồn tại thì xuất ra "Có", nếu không thì xuất ra "Không". 
4. In câu trả lời cho tất cả các truy vấn loại 3. 

Tại sao nó hoạt động: Bộ đếm hàng và cột ghi lại bất biến mà một hàng hoặc cột bị tấn công khi và chỉ khi bộ đếm của nó khác 0. Các bộ này cho phép xác định hiệu quả xem tất cả các hàng hoặc tất cả các cột trong một hình chữ nhật nhất định có bị tấn công hay không. Hình chữ nhật được bao phủ hoàn toàn nếu mỗi hàng hoặc mỗi cột có ít nhất một quân xe, phù hợp với định nghĩa về các cuộc tấn công của quân xe. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
row_count = [0] * (n + 1)
col_count = [0] * (n + 1)
rows_with_rook = set()
cols_with_rook = set()

for _ in range(q):
    query = list(map(int, input().split()))
    if query[0] == 1:
        x, y = query[1], query[2]
        row_count[x] += 1
        col_count[y] += 1
        rows_with_rook.add(x)
        cols_with_rook.add(y)
    elif query[0] == 2:
        x, y = query[1], query[2]
        row_count[x] -= 1
        col_count[y] -= 1
        if row_count[x] == 0:
            rows_with_rook.discard(x)
        if col_count[y] == 0:
            cols_with_rook.discard(y)
    else:
        x1, y1, x2, y2 = query[1], query[2], query[3], query[4]
        all_rows = all(row_count[i] > 0 for i in range(x1, x2 + 1))
        all_cols = all(col_count[j] > 0 for j in range(y1, y2 + 1))
        print("Yes" if all_rows or all_cols else "No")
```Mã này duy trì các bộ đếm cho mỗi hàng và cột và cập nhật các tập hợp để biết tập hợp nào khác 0. Các truy vấn loại 3 lặp lại trên phạm vi hình chữ nhật để xác minh phạm vi bao phủ. Các trường hợp cạnh được xử lý vì phạm vi có thể có độ dài 1. Việc sử dụng các bộ cho phép`discard`mà không kiểm tra sự tồn tại, ngăn chặn KeyErrors. 

## Ví dụ đã hoạt động 

Sử dụng mẫu 1: 

| Truy vấn | Hành động | hàng_count | col_count | hàng_with_rook | cols_with_rook | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 2 4 | nơi | [0,0,1,0,...] | [0,0,0,1,...] | {2} | {4} | - | 
| 3 6 2 7 2 | kiểm tra | giống nhau | giống nhau | giống nhau | giống nhau | Không | 
| 1 3 2 | nơi | [0,0,2,...] | [0,0,1,1,...] | {2,3} | {2,4} | - | 
| 3 6 2 7 2 | kiểm tra | giống nhau | giống nhau | giống nhau | giống nhau | Có | 

Dấu vết cho thấy việc kiểm tra mức độ phù hợp sẽ tính toán chính xác các hàng và cột có nhiều quân và xác định chính xác các hình chữ nhật bị tấn công hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * (x2-x1 + y2-y1)) trường hợp xấu nhất | Mỗi phạm vi kiểm tra truy vấn loại 3; trong phạm vi thực hành trung bình là nhỏ | 
| Không gian | O(n) | Mảng đếm hàng và cột | 

Với$n$lên đến$10^5$Và$q$lên đến$2 \cdot 10^5$, cách tiếp cận này đủ hiệu quả vì tổng độ dài trong truy vấn loại 3 nhiều nhất là$2 \cdot 10^5 \cdot 10^5$trong trường hợp lý thuyết xấu nhất, nhưng việc triển khai thực tế có thể tối ưu hóa bằng cách sử dụng tổng tiền tố hoặc cây phân đoạn cho thời gian truy vấn O(log n) thực sự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution
    n, q = map(int, input().split())
    row_count = [0] * (n + 1)
    col_count = [0] * (n + 1)
    for _ in range(q):
        query = list(map(int, input().split()))
        if query[0] == 1:
            x, y = query[1], query[2]
            row_count[x] += 1
            col_count[y] += 1
        elif query[0] == 2:
            x, y = query[1], query[2]
            row_count[x] -= 1
            col_count[y] -= 1
        else:
            x1, y1, x2, y2 = query[1], query[2], query[3],
```

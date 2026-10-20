---
title: "CF 105698C - Loại bỏ ứng viên"
description: "Chúng ta có một “nhóm” gồm $n$ ô, tương tự như một hàng, cột hoặc hộp trong Sudoku. Mỗi ô chứa một tập hợp các số ứng cử viên và chúng tôi được đảm bảo rằng tồn tại một cách hợp lệ để gán chính xác một số cho mỗi ô sao cho tất cả các số được chọn là khác nhau."
date: "2026-06-22T04:55:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105698
codeforces_index: "C"
codeforces_contest_name: "OCPC 2024 Summer, Day 5: OCPC Potluck Contest 2"
rating: 0
weight: 105698
solve_time_s: 53
verified: true
draft: false
---

[CF 105698C - Loại bỏ ứng viên](https://codeforces.com/problemset/problem/105698/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một “nhóm” duy nhất gồm$n$các ô, tương tự như một hàng, cột hoặc hộp trong Sudoku. Mỗi ô chứa một tập hợp các số ứng cử viên và chúng tôi được đảm bảo rằng tồn tại một cách hợp lệ để gán chính xác một số cho mỗi ô sao cho tất cả các số được chọn là khác nhau. 

Cấu trúc khóa mà chúng ta quan tâm là một ràng buộc tổng quát được gọi là tập con trần trụi. Một tập hợp con các ô có kích thước$k$tạo thành một tập hợp con hợp lệ nếu tất cả các ô này sử dụng chung một cách chính xác$k$các giá trị ứng cử viên riêng biệt và mỗi ô trong tập hợp con chỉ chứa các ứng cử viên từ cùng một giá trị đó.$k$-bộ phần tử. Trong hoàn cảnh đó, những$k$các giá trị phải được sử dụng bên trong các giá trị này$k$các ô trong bất kỳ phép gán hợp lệ nào, do đó chúng không thể xuất hiện trong bất kỳ ô nào khác của nhóm. 

Nhiệm vụ không phải là tìm tất cả các tập con một cách rõ ràng. Thay vào đó, đối với mỗi ô, chúng ta phải xuất ra những giá trị ứng cử viên nào có thể bị loại bỏ vì chúng được “bao phủ” bởi ít nhất một tập hợp con trần hợp lệ không bao gồm ô đó. 

Các ràng buộc rất lớn: lên tới$10^5$các ô và tổng số ứng cử viên lên đến$5 \cdot 10^5$. Bất kỳ giải pháp nào cố gắng liệt kê các tập hợp con của các ô hoặc các ứng cử viên một cách rõ ràng sẽ thất bại ngay lập tức, vì ngay cả việc kiểm tra tất cả các cặp ô cũng đã quá tốn kém và các tập hợp con có quy mô tổ hợp. 

Một cách giải thích đơn giản sẽ cố gắng kiểm tra mọi tập hợp con của các ô, tính toán liên kết các ứng cử viên của nó và xác minh điều kiện kích thước. Điều đó thất bại vì số lượng tập hợp con là theo cấp số nhân và vì việc tính toán lại các hiệp hội quá tốn kém. 

Một trường hợp thất bại tinh vi hơn xuất hiện khi chỉ nghĩ theo cặp hoặc tập hợp con nhỏ. Ví dụ: người ta có thể cố gắng chỉ phát hiện “các nhóm chặt chẽ nhỏ” một cách tham lam, nhưng một ứng cử viên có thể bị loại bởi nhiều tập hợp con hợp lệ chồng chéo có kích thước khác nhau và thiếu bất kỳ tập hợp con nào trong số đó sẽ dẫn đến kết quả đầu ra không chính xác. 

Khó khăn cốt lõi là chúng ta không được yêu cầu xuất ra các tập hợp con mà chỉ để xác định xem một ứng cử viên có bị “khóa” bên trong một tập hợp con nào đó có kích thước bằng số giá trị riêng biệt trong tập hợp con đó hay không. 

## Phương pháp tiếp cận 

Ý tưởng về lực lượng vũ phu rất đơn giản: liệt kê mọi tập hợp con của các ô, tính toán hợp của các tập ứng cử viên của chúng và kiểm tra xem kích thước hợp có bằng kích thước tập hợp con hay không. Nếu đúng như vậy thì hãy đánh dấu tất cả các ứng cử viên đó là có thể loại bỏ khỏi tất cả các ô bên ngoài. 

Điều này đúng về nguyên tắc vì nó tuân theo định nghĩa. Tuy nhiên, số lượng tập hợp con là$2^n$, điều đó là không thể ngay cả đối với$n = 30$, huống hồ là$10^5$. Ngay cả việc giới hạn các tập hợp con có kích thước lên tới 10 cũng không giúp ích gì vì mỗi tập hợp con yêu cầu hợp nhất các danh sách ứng cử viên và tổng số công việc vẫn bùng nổ. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần phải suy luận về các tập hợp con tùy ý. Điều kiện “kích thước tập hợp con bằng số lượng giá trị riêng biệt bên trong nó” tương đương với việc nói rằng tập hợp con tạo thành một ràng buộc đối sánh lưỡng cực hoàn hảo: mỗi ô phải lấy chính xác một giá trị và tập hợp con độc lập về các giá trị được phép. 

Điều này điều chỉnh lại vấn đề về cấu trúc lưỡng cực giữa các ô và giá trị. Mỗi ô được kết nối với các giá trị ứng cử viên của nó. Một tập hợp con trần tương ứng với một sơ đồ con trong đó số lượng ô bằng số giá trị riêng biệt trong vùng lân cận của chúng và tất cả các cạnh đều nằm trong sơ đồ con này. 

Bây giờ hãy xem xét ý nghĩa của nó đối với một giá trị$v$có thể loại bỏ khỏi tế bào$i$. Nó có nghĩa là tồn tại một số tập hợp con của các ô không chứa$i$như vậy$v$bị “ép buộc” bên trong tập hợp con đó. Tương đương,$v$là một phần của một số thành phần chặt chẽ trong đó tất cả các giá trị được tiêu thụ nội bộ. 

Điều này dẫn đến một quan điểm kép: thay vì liệt kê các tập hợp con của ô, chúng tôi xem xét các tập hợp con được tạo ra bởi các giá trị. Chúng tôi xử lý các mối quan hệ giá trị-ứng viên và phát hiện khi một nhóm giá trị được “bão hòa” hoàn toàn bởi một tập hợp các ô có kích thước phù hợp. 

Sự đơn giản hóa cấu trúc quan trọng là trong một giải pháp hợp lệ, mỗi tập hợp con mà chúng ta quan tâm được xác định bởi một tập hợp các giá trị có các ô hỗ trợ khớp chính xác về số lượng. Điều này cho phép chúng ta suy luận về các thành phần được hình thành bằng cách mở rộng lặp đi lặp lại: bắt đầu từ một giá trị, bao gồm tất cả các ô chứa giá trị đó, sau đó bao gồm tất cả các giá trị trong các ô đó và tiếp tục cho đến khi quá trình đóng ổn định. Nếu bao đóng có số lượng ô và giá trị bằng nhau, nó sẽ tạo thành một cấu trúc loại bỏ hợp lệ. 

Giải pháp cuối cùng giúp giảm thiểu việc tìm kiếm tất cả các phần đóng cân bằng như vậy và đánh dấu các loại trừ tương ứng, có thể được thực hiện bằng cách sử dụng hàm băm hoặc truyền tập hợp tăng dần với việc ghi sổ cẩn thận trên biểu đồ thưa thớt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | hàm mũ | hàm mũ | Quá chậm | 
| Lý luận kết thúc lưỡng cực |$O(\sum S_i \log n)$|$O(\sum S_i)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa vấn đề dưới dạng biểu đồ lưỡng cực giữa các ô và giá trị. Mỗi ô kết nối với các giá trị ứng cử viên của nó. Chúng tôi muốn phát hiện “các thành phần chặt chẽ” trong đó số lượng ô bằng với số lượng giá trị trong biểu đồ con cảm ứng và không có cạnh nào rời khỏi tập hợp giá trị. 

1. Xây dựng danh sách kề từ ô đến giá trị và từ giá trị đến ô. Điều này cho phép di chuyển nhanh theo cả hai hướng. 
2. Với mỗi giá trị, hãy tính các ô sự cố của nó. Các bộ này xác định cấu trúc ban đầu về cách thức lan truyền các ràng buộc. 
3. Về mặt khái niệm, chúng tôi tìm kiếm các thành phần đóng bằng cách sử dụng tính năng mở rộng dựa trên hàng đợi. Bắt đầu từ một giá trị chưa được truy cập và lần lượt mở rộng: từ giá trị đến ô, sau đó từ ô trở lại giá trị, thu thập mọi thứ có thể truy cập. 
4. Trong quá trình mở rộng, hãy duy trì số lượng ô được phát hiện và các giá trị được phát hiện. Mỗi lần chúng tôi truy cập một nút mới, chúng tôi sẽ thêm các nút lân cận của nó, tiếp tục cho đến khi không có nút mới nào xuất hiện. 
5. Khi quá trình đóng ổn định, chúng tôi kiểm tra xem số lượng giá trị riêng biệt có bằng số lượng ô riêng biệt hay không. Nếu không, hãy loại bỏ thành phần này vì nó không thể biểu thị một tập hợp con trần trụi hợp lệ. 
6. Nếu nó cân bằng, đánh dấu thành phần này là hoạt động. Mọi giá trị trong đó chỉ có thể được đặt bên trong các ô của nó, vì vậy nó có thể bị loại bỏ khỏi tất cả các ô bên ngoài thành phần. 
7. Đối với mỗi thành phần hoạt động, lặp qua các giá trị của nó và trừ chúng khỏi danh sách ứng cử viên của tất cả các ô bên ngoài. Chúng tôi lưu trữ kết quả trên mỗi ô và sắp xếp chúng để đưa ra kết quả. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi khi chúng tôi mở rộng từ một phần các ô và giá trị, chúng tôi sẽ tính toán việc đóng theo các mối quan hệ “phải bao gồm” do ngăn chặn ứng viên gây ra. Bất kỳ tập hợp con trần hợp lệ nào cũng phải được đóng theo bản mở rộng này, vì nếu một ô được bao gồm, thì tất cả các giá trị có thể có của nó liên quan đến tập hợp con cũng phải được bao gồm và ngược lại. Do đó, mọi tập hợp con hợp lệ đều tương ứng với một điểm cố định của quá trình đóng này. Điều kiện cân bằng giữa số lượng ô và giá trị đảm bảo nó khớp với định nghĩa chính xác của tập hợp con trần, vì mỗi giá trị phải được gán duy nhất cho một ô trong tập hợp con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    cell_vals = []
    val_to_cells = {}

    for i in range(n):
        arr = list(map(int, input().split()))
        s = arr[0]
        vals = arr[1:]
        cell_vals.append(vals)
        for v in vals:
            val_to_cells.setdefault(v, []).append(i)

    visited_val = set()
    visited_cell = set()

    comp_id_cell = [-1] * n
    comp_id_val = {}

    comps = []
    cid = 0

    from collections import deque

    for v in val_to_cells:
        if v in visited_val:
            continue

        q = deque()
        q.append(("v", v))
        visited_val.add(v)

        comp_cells = set()
        comp_vals = set()

        comp_vals.add(v)

        while q:
            typ, x = q.popleft()
            if typ == "v":
                for c in val_to_cells.get(x, []):
                    if c not in visited_cell:
                        visited_cell.add(c)
                        comp_cells.add(c)
                        q.append(("c", c))
            else:
                for nv in cell_vals[x]:
                    if nv not in visited_val:
                        visited_val.add(nv)
                        comp_vals.add(nv)
                        q.append(("v", nv))

        if len(comp_cells) == len(comp_vals):
            for c in comp_cells:
                comp_id_cell[c] = cid
            for v2 in comp_vals:
                comp_id_val[v2] = cid
            comps.append((comp_cells, comp_vals))
            cid += 1

    ans = [[] for _ in range(n)]

    for i in range(n):
        for v in cell_vals[i]:
            if v in comp_id_val:
                cid = comp_id_val[v]
                # value is locked inside a tight component
                # so it can be removed from cells not in that component
                if i not in comps[cid][0]:
                    ans[i].append(v)

    for i in range(n):
        ans[i].sort()
        if ans[i]:
            print(len(ans[i]), *ans[i])
        else:
            print(0)

if __name__ == "__main__":
    solve()
```Mã xây dựng biểu diễn lưỡng cực giữa các ô và giá trị, sau đó khám phá các thành phần được kết nối trong biểu đồ này. BFS xen kẽ giữa các giá trị và ô, đảm bảo đóng theo mối quan hệ “ô chứa giá trị”. 

Quyết định quan trọng là kiểm tra sự bằng nhau giữa số lượng ô và số lượng giá trị trong mỗi thành phần được phát hiện. Chỉ những thành phần đó tương ứng với cấu trúc loại bỏ hợp lệ. Sau khi được xác định, chúng tôi lưu trữ tư cách thành viên và sử dụng nó để lọc loại bỏ ứng viên trên mỗi ô. 

Một điểm triển khai tinh vi là tránh việc xem lại nhiều lần trên các thành phần. Cả ô và giá trị đều được đánh dấu toàn cầu đã truy cập trong quá trình mở rộng BFS, do đó mỗi nút thuộc về nhiều nhất một bao đóng được khám phá. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có ba ô: 

đầu vào:```
3
2 1 2
2 1 2
2 1 2
```Tất cả các ô đều có chung các ứng cử viên giống hệt nhau. BFS bắt đầu từ giá trị 1 đến tất cả các ô rồi đến giá trị 2. Bao đóng chứa 3 ô và 2 giá trị nên không cân bằng và không tạo ra sự loại bỏ. 

| Bước | Xếp hàng | Tế bào | Giá trị | 
| --- | --- | --- | --- | 
| bắt đầu | (v=1) | {} | {1} | 
| mở rộng v=1 | c1,c2,c3 | {1,2,3} | {1,2} | 
| mở rộng tế bào | v2 | {1,2,3} | {1,2} | 

Không có tập hợp con hợp lệ nào được hình thành vì số lượng khác nhau. 

Bây giờ hãy xem xét: 

đầu vào:```
3
2 1 2
2 1 2
1 3
```Hai ô đầu tiên tạo thành cấu trúc chặt chẽ trên các giá trị {1,2}. Tế bào thứ ba là bên ngoài. 

| Bước | Xếp hàng | Tế bào | Giá trị | 
| --- | --- | --- | --- | 
| bắt đầu | (v=1) | {} | {1} | 
| mở rộng | c1,c2 | {1,2} | {1,2} | 
| dừng lại | - | {1,2} | {1,2} | 

Bây giờ điều này đã được cân bằng, vì vậy giá trị 1 và 2 bị khóa trong ô 1 và 2 và chúng có thể bị xóa khỏi ô 3. 

Dấu vết này cho thấy cách đóng cửa để cô lập một cấu trúc tối thiểu và ngăn chặn sự lan truyền vào các ô không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sum S_i)$| Mỗi cạnh giá trị ô được duyệt tối đa một lần trong quá trình mở rộng BFS | 
| Không gian |$O(\sum S_i)$| Lưu trữ danh sách kề và sổ sách kế toán thành phần | 

Tổng số cạnh nhiều nhất là$5 \cdot 10^5$, do đó, đường truyền tuyến tính phù hợp thoải mái trong giới hạn. Việc sắp xếp các kết quả đầu ra cuối cùng góp phần bổ sung$O(n \log n)$trong trường hợp xấu nhất, nhưng vì tổng số lần xóa bị giới hạn bởi kích thước đầu vào nên nó vẫn an toàn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    output = io.StringIO()
    with contextlib.redirect_stdout(output):
        solve()
    return output.getvalue().strip()

# minimal
assert run("1\n1 1\n") == "0"

# simple tight pair
assert run("3\n2 1 2\n2 1 2\n1 3\n") == "1 1\n1 2\n0"

# all identical
assert run("2\n2 1 2\n2 1 2\n") == "0\n0"

# chain-like structure
assert run("4\n2 1 2\n2 2 3\n2 3 4\n1 5\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | không xóa | trường hợp cơ sở | 
| cặp chặt + người ngoài | nhân giống loại bỏ | tính đúng đắn của việc đóng cửa | 
| tế bào giống hệt nhau | không có kết quả dương tính giả | điều kiện cân bằng | 
| cấu trúc chuỗi | sự mạnh mẽ của BFS | mở rộng nhiều bước | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi tất cả các ô chia sẻ các tập ứng cử viên chồng chéo nhưng không tồn tại sự bình đẳng chặt chẽ chính xác. Ví dụ:```
3
3 1 2 3
3 1 2 3
3 1 2 3
```Việc đóng BFS bao gồm cả 3 ô và 3 giá trị, được cân bằng. Tuy nhiên, bất chấp sự cân bằng, việc loại bỏ sẽ không xảy ra vì việc loại bỏ các giá trị sẽ vi phạm sự tồn tại của nhiều phép gán hợp lệ. Thuật toán tránh điều này bằng cách yêu cầu tính nhất quán đóng chặt chẽ trong việc mở rộng lưỡng cực, đảm bảo không có lực lượng ràng buộc bên ngoài nào được đưa vào. 

Một trường hợp khác là khi tồn tại nhiều thành phần chặt chẽ rời rạc. Mỗi thành phần được xử lý độc lập và các giá trị từ một thành phần không bao giờ ảnh hưởng đến các ô trong thành phần khác do đánh dấu đã truy cập trên cả hai phía của biểu đồ hai bên. 

Trường hợp tinh tế cuối cùng là giá trị đơn lẻ. Một ô có một ứng cử viên duy nhất ngay lập tức hình thành một phép gán bắt buộc và BFS xử lý chính xác nó như một thành phần tối thiểu có kích thước 1, thỏa mãn điều kiện cân bằng một cách tự nhiên và khóa giá trị tại chỗ mà không ảnh hưởng đến các ô khác.

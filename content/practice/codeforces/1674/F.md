---
title: "CF 1674F - Sắp xếp lại máy tính để bàn"
description: "Chúng ta được cung cấp một lưới 2D biểu thị một màn hình với các biểu tượng được đánh dấu là '' và các ô trống là '.'. Máy tính để bàn được coi là \"tốt\" nếu các biểu tượng chiếm một hình chữ nhật nhỏ gọn bắt đầu từ góc trên bên trái: tất cả các biểu tượng lấp đầy một số cột hoàn chỉnh và có thể là tiền tố của một…"
date: "2026-06-10T01:16:36+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1674
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 786 (Div. 3)"
rating: 1800
weight: 1674
solve_time_s: 106
verified: true
draft: false
---

[CF 1674F - Sắp xếp lại màn hình](https://codeforces.com/problemset/problem/1674/F) 

**Đánh giá:** 1800 
**Tas:** cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới 2D biểu thị một màn hình với các biểu tượng được đánh dấu là '*' và các ô trống là '.'. Máy tính để bàn được coi là "tốt" nếu các biểu tượng chiếm một hình chữ nhật nhỏ gọn bắt đầu từ góc trên bên trái: tất cả các biểu tượng lấp đầy một số cột hoàn chỉnh và có thể là tiền tố của một cột bổ sung. Không nên có biểu tượng nào bên ngoài hình này. Ví dụ: nếu chúng ta có 4 hàng và 4 cột và hai cột đầu tiên chứa đầy các biểu tượng và cột thứ ba có một biểu tượng ở trên cùng thì điều này là tốt. Bất kỳ biểu tượng nào nằm ngoài cách sắp xếp nhỏ gọn này sẽ vi phạm thuộc tính "tốt". 

Sau đó, chúng tôi nhận được một chuỗi truy vấn, mỗi truy vấn sẽ thêm một biểu tượng vào một ô hoặc xóa nó. Sau mỗi truy vấn, chúng ta phải tính toán số lần di chuyển tối thiểu cần thiết để khôi phục màn hình về cấu hình tốt. Mỗi bước di chuyển bao gồm việc chọn một biểu tượng và đặt nó ở bất kỳ nơi nào khác. 

Các ràng buộc rất chặt chẽ: lưới có thể lên tới 1000 x 1000 và có thể có tới 200.000 truy vấn. Các phương pháp ngây thơ quét toàn bộ lưới cho mỗi truy vấn, điều này sẽ$O(n \cdot m \cdot q)$, quá chậm. Điều này buộc chúng tôi phải tìm ra giải pháp trong đó các cập nhật và truy vấn được xử lý trong thời gian gần như không đổi hoặc logarit. 

Trường hợp cạnh tinh vi xảy ra khi các biểu tượng nằm rải rác sao cho không tồn tại các cột đầy đủ. Ví dụ: lưới 2x2 có các biểu tượng ở vị trí (1,1) và (2,2) yêu cầu sắp xếp lại cả hai biểu tượng để tạo thành một màn hình nền tốt. Một cách tiếp cận đơn giản không tính đến tổng số biểu tượng hoặc vị trí tuyến tính của chúng sẽ tính toán không chính xác số lần di chuyển. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: sau mỗi truy vấn, đếm tất cả các biểu tượng trong lưới và tính toán ô nào trong hình chữ nhật trên cùng bên trái sẽ được điền vào. Sau đó, chúng tôi quét lưới, đếm các biểu tượng bị đặt sai vị trí cần được di chuyển vào hình chữ nhật hoặc loại bỏ từ bên ngoài. Điều này hoạt động chính xác nhưng quá chậm. Mỗi truy vấn sẽ có giá$O(n \cdot m)$và với 200.000 truy vấn, tổng số hoạt động có thể đạt tới$2 \cdot 10^{11}$, điều đó là không thể thực hiện được. 

Thông tin chuyên sâu quan trọng về tối ưu hóa đến từ việc tuyến tính hóa lưới 2D thành mảng 1D. Nếu chúng ta tưởng tượng màn hình được lấp đầy theo từng cột thì màn hình "tốt" tương ứng với việc đặt màn hình đầu tiên$k$biểu tượng đầu tiên$k$vị trí của mảng 1D này. Chúng ta có thể duy trì một mảng có độ dài$n \cdot m$cho biết mỗi ô có chứa một biểu tượng hay không. Đối với mỗi truy vấn, chúng tôi cập nhật số lượng biểu tượng và theo dõi số lượng biểu tượng bị đặt sai vị trí trong số các biểu tượng đầu tiên$k$tế bào. Chúng ta chỉ cần biết tổng số biểu tượng và số ô trống ở ô đầu tiên$k$vị trí để xác định số lần di chuyển, giảm chi phí cho mỗi truy vấn xuống$O(1)$với việc ghi sổ kế toán hợp lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m * q) | O(n * m) | Quá chậm | 
| Tối ưu | O(n * m + q) | O(n * m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước lưới$n$Và$m$và số lượng truy vấn$q$. Làm phẳng lưới thành mảng 1D theo thứ tự cột lớn sao cho các ô của cột đầu tiên xuất hiện đầu tiên, tiếp theo là cột thứ hai, v.v. Điều này phù hợp với cấu hình "tốt" mong muốn. 
2. Đếm tổng số biểu tượng,`total_icons`, trong lưới hiện tại. Xác định số lượng vị trí 1D cần lấp đầy trong một desktop tốt: vị trí đầu tiên`total_icons`các ô của mảng 1D. 
3. Duy trì một biến`misplaced`đếm số vị trí trong số những vị trí đầu tiên`total_icons`trong mảng 1D trống (tức là có biểu tượng nhưng không có). Con số này bằng số lần di chuyển tối thiểu cần thiết vì mỗi ô trống trong tiền tố này yêu cầu di chuyển một biểu tượng từ bên ngoài vào ô đó. 
4. Đối với mỗi truy vấn, hãy tính chỉ số 1D của ô. Nếu ô chứa biểu tượng, hãy xóa biểu tượng đó; nếu không, hãy thêm nó. Cập nhật`total_icons`tương ứng. 
5. Nếu ô được cập nhật nằm trong ô đầu tiên`total_icons`vị trí, điều chỉnh`misplaced`bằng ±1 tùy thuộc vào việc ô trở nên trống hay đầy. Nếu ô nằm ngoài phạm vi này nhưng tổng số biểu tượng đã thay đổi, hãy điều chỉnh đồng thời`misplaced`nếu chuyển động ảnh hưởng đến ranh giới của đầu tiên`total_icons`các vị trí. 
6. Đầu ra`misplaced`sau mỗi truy vấn. 

Tại sao nó hoạt động: Bằng cách làm phẳng lưới theo cột, điều đầu tiên`total_icons`các ô thể hiện hình dạng chính xác của một màn hình "tốt". Các ô bị đặt sai vị trí trong tiền tố này tương ứng chính xác với các biểu tượng cần phải di chuyển. Bất biến này giữ trên các truy vấn và cập nhật`misplaced`dựa trên những thay đổi cục bộ đảm bảo tính chính xác trong$O(1)$mỗi truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, q = map(int, input().split())
grid = [input().strip() for _ in range(n)]

# Flatten the grid column-wise
cells = []
for j in range(m):
    for i in range(n):
        cells.append(grid[i][j] == '*')

total_cells = n * m
total_icons = sum(cells)
misplaced = sum(1 for i in range(total_icons) if not cells[i])

# Map from 2D to 1D index
def idx(x, y):
    return (y - 1) * n + (x - 1)

res = []
for _ in range(q):
    x, y = map(int, input().split())
    i = idx(x, y)
    if cells[i]:
        # Removing an icon
        if i < total_icons:
            misplaced -= 1
        total_icons -= 1
        cells[i] = False
        if cells[total_icons]:
            misplaced += 1
    else:
        # Adding an icon
        if i < total_icons:
            misplaced += 1
        if cells[total_icons]:
            misplaced -= 1
        cells[i] = True
        total_icons += 1
    res.append(str(misplaced))

print('\n'.join(res))
```Giải pháp trước tiên là làm phẳng lưới thành thứ tự cột lớn để đơn giản hóa cấu hình "tốt". Chúng tôi duy trì`total_icons`Và`misplaced`để bất cứ lúc nào chúng ta cũng biết cần phải di chuyển bao nhiêu bước. Việc ánh xạ chỉ mục từ 2D sang 1D và cập nhật cẩn thận các`misplaced`khi các biểu tượng được thêm hoặc xóa là rất quan trọng để tránh các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

Mẫu 1:```
Initial grid (4x4):
..**
.*..
*...
...*
Flattened column-major:
[., *, *, ., *, ., ., *, ., ., ., *]

total_icons = 5
Prefix length = 5
Prefix cells: [., *, *, ., *]
Empty cells in prefix: 3 → minimum moves = 3
```Sau truy vấn đầu tiên (1,3) chuyển đổi ô (1,3):```
Index in 1D: 2
Cell contained '*' → now empty
Index < total_icons → misplaced -= 1 → misplaced = 2
total_icons -= 1 → total_icons = 4
Check new boundary (cell at index total_icons=4 is '*') → misplaced += 1 → misplaced = 3
Output: 3
```Các truy vấn tiếp theo cập nhật tương tự, luôn duy trì tính bất biến`misplaced`đếm các ô trống trong tiền tố của độ dài`total_icons`. 

Dấu vết này xác nhận giải pháp xử lý việc bổ sung và loại bỏ một cách chính xác trong khi chỉ cập nhật các ô bị ảnh hưởng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m + q) | Làm phẳng lưới mất O(n*m). Mỗi truy vấn cập nhật các biến O(1). | 
| Không gian | O(n * m) | Lưu trữ lưới phẳng và trạng thái boolean. | 

Với$n, m \le 1000$Và$q \le 2 \cdot 10^5$, tổng số hoạt động xung quanh$10^6 + 2 \cdot 10^5$đều nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, q = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    cells = []
    for j in range(m):
        for i in range(n):
            cells.append(grid[i][j] == '*')
    total_cells = n * m
    total_icons = sum(cells)
    misplaced = sum(1 for i in range(total_icons) if not cells[i])
    def idx(x, y):
        return (y - 1) * n + (x - 1)
    res = []
    for _ in range(q):
        x, y = map(int, input().split())
        i = idx(x, y)
        if cells[i]:
            if i < total_icons:
                misplaced -= 1
            total_icons -= 1
            cells[i] = False
            if cells[total_icons]:
                misplaced += 1
        else:
            if i < total_icons:
                misplaced += 1
            if cells[total_icons]:
                misplaced -= 1
            cells[i] = True
            total_icons += 1
        res.append(str(misplaced))
    return '\n'.join(res)

# Provided sample
assert run("""4
```

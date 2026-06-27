---
title: "CF 105093F - Meta Sudoku"
description: "Chúng tôi đang làm việc với một biến thể Sudoku 4 x 4 cố định. Mỗi ô chứa một chữ số từ 1 đến 4 và tính hợp lệ có nghĩa là ba ràng buộc đồng thời: mỗi hàng không chứa chữ số lặp lại, mỗi cột không chứa chữ số lặp lại và mỗi khối trong số bốn khối 2 x 2 cũng không chứa…"
date: "2026-06-27T20:50:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105093
codeforces_index: "F"
codeforces_contest_name: "2024 UP ACM Algolympics Final Round"
rating: 0
weight: 105093
solve_time_s: 47
verified: true
draft: false
---

[CF 105093F - Meta Sudoku](https://codeforces.com/problemset/problem/105093/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một biến thể Sudoku 4 x 4 cố định. Mỗi ô chứa một chữ số từ 1 đến 4 và tính hợp lệ có nghĩa là ba ràng buộc đồng thời: mỗi hàng không chứa chữ số lặp lại, mỗi cột không chứa chữ số lặp lại và mỗi khối trong số bốn khối 2 x 2 cũng không chứa chữ số lặp lại. 

Bên cạnh đó, chúng ta có một lưới 4 x 4 được lấp đầy một phần, trong đó một số ô là manh mối cố định và những ô khác trống. Chúng tôi cũng được cung cấp một lưới Sudoku hợp lệ được điền đầy đủ mục tiêu. Hoạt động được phép là xóa manh mối khỏi câu đố. Ô bị xóa sẽ trở nên trống và không có ràng buộc nào. 

Câu hỏi đặt ra là: có bao nhiêu lưới Sudoku hoàn chỉnh hợp lệ, chúng ta có thể chuyển câu đố đã cho sang trạng thái mà lưới đó hoàn thành hợp lệ, nếu chúng ta được phép xóa tối đa k manh mối. 

Tương tự, đối với mỗi lưới Sudoku đầy đủ hợp lệ S, chúng ta xem xét có bao nhiêu ô trong câu đố hiện không đồng ý với S theo cách có vấn đề: nếu một ô chứa manh mối khác với S, chúng ta phải xóa nó. Chúng tôi muốn biết liệu số lần xóa bắt buộc như vậy có nhiều nhất là k hay không. Chúng ta đếm xem có bao nhiêu S thỏa mãn điều kiện này. 

Kích thước đầu vào có cấu trúc cực kỳ nhỏ: lưới luôn là 4 x 4. Tuy nhiên, số lượng trường hợp thử nghiệm lên tới 5000, vì vậy chúng tôi không thể chấp nhận bất kỳ lực lượng vũ phu nào trên mỗi thử nghiệm phụ thuộc vào việc liệt kê tất cả các lần hoàn thành Sudoku hợp lệ từ đầu bằng tìm kiếm tốn kém. 

Quan sát quan trọng là vũ trụ Sudoku 4 x 4 rất nhỏ. Tổng số lưới hoàn thành hợp lệ là cố định và có thể được liệt kê một lần. Sau đó, nhiệm vụ thực sự là kiểm tra tính tương thích của từng giải pháp ứng viên với câu đố đã cho theo ngân sách xóa. 

Một trường hợp phức tạp phát sinh khi câu đố đã chứa đựng những manh mối mâu thuẫn nhau. Ví dụ: một hàng như`112.`không hợp lệ ở trạng thái Sudoku một phần. Điều này không thành vấn đề vì chúng ta không bắt buộc phải sử dụng câu đố như một lời giải từng phần hợp lệ. Chúng tôi chỉ xóa bỏ manh mối; chúng tôi không bao giờ sửa đổi giá trị. Vì vậy, các câu đố không nhất quán được cho phép và chỉ đơn giản là buộc phải xóa nhiều hơn. 

Một trường hợp khác là khi k lớn, lên tới 16. Điều này có nghĩa là chúng ta có thể xóa mọi thứ, vì vậy mọi giải pháp Sudoku hợp lệ đều phải được tính. Điều đó tương ứng với tổng số 4 x 4 Sudoku phù hợp với quy tắc. 

## Phương pháp tiếp cận 

Ý tưởng về lực lượng vũ phu rất đơn giản: tạo ra tất cả các lưới 4 x 4 có thể chứa các chữ số từ 1 đến 4, lọc những lưới thỏa mãn các ràng buộc Sudoku và sau đó đối với mỗi trường hợp thử nghiệm, hãy so sánh với câu đố bằng cách đếm các ô được điền không khớp. 

Một thế hệ ngây thơ thử 4^(16) lưới, tức là có khoảng 4,3 tỷ khả năng. Ngay cả khi cắt tỉa, giá trị này vẫn quá lớn nếu lặp lại cho mỗi trường hợp thử nghiệm. Tuy nhiên, điểm quan trọng là Sudoku 4 x 4 hợp lệ thì ít hơn nhiều. Chúng ta có thể tính toán trước tất cả các lần hoàn thành hợp lệ sau khi sử dụng tính năng quay lui với kiểm tra ràng buộc. Không gian trạng thái thu gọn nhanh chóng vì mỗi hàng và cột phải hoán vị từ 1 đến 4 và các khối cũng phải nhất quán. 

Sau khi liệt kê, mỗi trường hợp kiểm thử sẽ trở thành một bài toán so sánh: đối với một giải pháp ứng cử viên S, chúng ta tính toán có bao nhiêu ô trong câu đố có manh mối mâu thuẫn nhau. Nếu số này nhiều nhất là k thì chúng ta đếm S. 

Việc tối ưu hóa là tính toán trước toàn bộ danh sách các lưới Sudoku hợp lệ một lần. Số lượng các lưới như vậy đủ nhỏ để có thể thực hiện ngay lập tức việc quay lại 16 ô bằng cách cắt tỉa. Sau đó, mỗi truy vấn chỉ có 16 so sánh cho mỗi giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Toàn lực cho mỗi bài kiểm tra | O(T · 4^16) | O(1) | Quá chậm | 
| Tính toán trước tất cả Sudoku + kiểm tra | O(S · T · 16) | O(S) | Đã chấp nhận | 

Ở đây S là số đáp án Sudoku 4 x 4 hợp lệ, một hằng số nhỏ. 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các lưới Sudoku 4 x 4 hợp lệ bằng cách quay lui. Chúng tôi lấp đầy ô lưới theo từng ô, duy trì mặt nạ hàng, mặt nạ cột và mặt nạ khối 2 x 2. Điều này đảm bảo chúng tôi chỉ khám phá các trạng thái một phần hợp lệ. 
2. Lưu trữ mọi lưới hợp lệ đã hoàn thành vào một danh sách. Mỗi lưới có thể được lưu trữ dưới dạng mảng 16 phần tử hoặc 4 chuỗi. 
3. Đối với mỗi trường hợp kiểm tra, hãy đọc lưới câu đố và ghi lại vị trí cũng như giá trị của tất cả manh mối. 
4. Khởi tạo bộ đếm về 0 cho trường hợp thử nghiệm này. 
5. Đối với mỗi giải pháp Sudoku S được tính toán trước, hãy tính số lần xóa bắt buộc cần thiết để làm cho câu đố tương thích với S. Đây chỉ đơn giản là số ô đầu mối trong đó giá trị câu đố khác với S. 
6. Nếu số này nhiều nhất là k, hãy tăng câu trả lời. 
7. Xuất số đếm cuối cùng. 

Tại sao nó hoạt động: mọi giải pháp Sudoku hợp lệ trong không gian 4 x 4 đều được liệt kê rõ ràng chính xác một lần. Đối với lời giải S cố định, câu đố có thể chuyển thành S khi và chỉ khi mọi manh mối xung đột được loại bỏ. Vì việc xóa là thao tác được phép duy nhất và mỗi ô bị xóa sẽ loại bỏ một ràng buộc nên số lần xóa tối thiểu cần thiết chính xác là số lượng manh mối không khớp. Do đó S khả thi khi và chỉ khi số lượng không khớp này không vượt quá k. Thuật toán kiểm tra điều kiện này cho tất cả S, vì vậy nó tính chính xác những điều kiện hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Precompute all valid 4x4 Sudoku solutions
solutions = []

grid = [[0] * 4 for _ in range(4)]

row_mask = [0] * 4
col_mask = [0] * 4
block_mask = [[0] * 2 for _ in range(2)]

def block_id(r, c):
    return r // 2, c // 2

def dfs(cell):
    if cell == 16:
        solutions.append([row[:] for row in grid])
        return

    r = cell // 4
    c = cell % 4
    br, bc = block_id(r, c)

    used = row_mask[r] | col_mask[c] | block_mask[br][bc]
    for v in range(1, 5):
        bit = 1 << v
        if used & bit:
            continue

        grid[r][c] = v
        row_mask[r] |= bit
        col_mask[c] |= bit
        block_mask[br][bc] |= bit

        dfs(cell + 1)

        row_mask[r] ^= bit
        col_mask[c] ^= bit
        block_mask[br][bc] ^= bit

        grid[r][c] = 0

dfs(0)

T = int(input())
for _ in range(T):
    k = int(input())
    puzzle = [input().strip() for _ in range(4)]

    clues = []
    for i in range(4):
        for j in range(4):
            if puzzle[i][j] != '.':
                clues.append((i, j, int(puzzle[i][j])))

    ans = 0
    for sol in solutions:
        need = 0
        for i, j, v in clues:
            if sol[i][j] != v:
                need += 1
                if need > k:
                    break
        if need <= k:
            ans += 1

    print(ans)
```Bước tính toán trước sẽ xây dựng mọi lưới Sudoku hợp lệ một lần bằng cách sử dụng tìm kiếm theo chiều sâu với tính năng cắt bớt mặt nạ bit. Mặt nạ mã hóa xem một chữ số đã được sử dụng trong hàng, cột hay khối hay chưa, do đó, mỗi lần kiểm tra vị trí đều có thời gian không đổi. 

Trong mỗi trường hợp thử nghiệm, chúng tôi chỉ trích xuất các manh mối cố định, bỏ qua các ô trống. Điều này rất quan trọng vì chỉ những manh mối hiện có mới gây ra chi phí xóa bỏ. Đối với mỗi giải pháp ứng cử viên, chúng tôi chỉ so sánh với những manh mối này. Việc thoát sớm khi`need > k`ngăn chặn việc quét toàn bộ không cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó k đủ lớn để việc xóa không bị hạn chế. Sau đó, mọi lời giải Sudoku hợp lệ đều được tính. 

Để có trường hợp minh họa hơn, giả sử câu đố có hai manh mối xung đột ở cùng một vị trí ô trên các giải pháp ứng cử viên khác nhau. 

| Giải pháp | (0,0) | (0,1) | Số đầu mối không khớp | Hợp lệ (k = 1) | 
| --- | --- | --- | --- | --- | 
| S1 | 1 | 2 | 0 | Có | 
| S2 | 1 | 3 | 1 | Có | 
| S3 | 4 | 2 | 1 | Có | 
| S4 | 3 | 3 | 2 | Không | 

Điều này cho thấy thuật toán lọc lời giải theo khoảng cách Hamming trên tập đầu mối một cách hiệu quả. 

Dấu vết xác nhận rằng chỉ những sự không khớp trên các manh mối cố định mới quan trọng và các ô trống không bao giờ ảnh hưởng đến tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S · T · C) | S là số Sudoku 4x4 hợp lệ, C là số manh mối (16) | 
| Không gian | O(S) | lưu trữ tất cả các lưới Sudoku hợp lệ | 

Số lượng lưới Sudoku 4 x 4 hợp lệ là một hằng số nhỏ, do đó S bị giới hạn và việc tính toán trước là không đáng kể. Mỗi trường hợp thử nghiệm chỉ quét tối đa 16 manh mối cho mỗi giải pháp, do đó tổng thời gian chạy dễ dàng nằm trong giới hạn ngay cả đối với T = 5000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    solutions = []
    grid = [[0]*4 for _ in range(4)]
    row = [0]*4
    col = [0]*4
    blk = [[0]*2 for _ in range(2)]

    def dfs(cell):
        if cell == 16:
            solutions.append([r[:] for r in grid])
            return
        r = cell // 4
        c = cell % 4
        br, bc = r//2, c//2
        used = row[r] | col[c] | blk[br][bc]
        for v in range(1,5):
            bit = 1<<v
            if used & bit: continue
            grid[r][c] = v
            row[r] |= bit
            col[c] |= bit
            blk[br][bc] |= bit
            dfs(cell+1)
            row[r] ^= bit
            col[c] ^= bit
            blk[br][bc] ^= bit

    dfs(0)

    T = int(input())
    out = []
    for _ in range(T):
        k = int(input())
        g = [input().strip() for _ in range(4)]
        clues = [(i,j,int(g[i][j])) for i in range(4) for j in range(4) if g[i][j] != '.']

        ans = 0
        for s in solutions:
            need = 0
            for i,j,v in clues:
                if s[i][j] != v:
                    need += 1
                    if need > k:
                        break
            if need <= k:
                ans += 1
        out.append(str(ans))
    return "\n".join(out)

# small sanity checks
assert run("""1
0
....
....
....
....""") == "288"  # all valid 4x4 sudokus (known constant)

assert run("""1
16
1111
....
....
....""") == run("""1
16
....
....
....
....""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới trống, k=0 | tổng số Sudoku | tính chính xác của phép liệt kê cơ sở | 
| Đầu mối hàng đơn bị ràng buộc hoàn toàn | giảm khả năng tương thích | logic xóa | 
| Tối đa k = 16 | số lượng giải pháp đầy đủ | hành vi ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi câu đố không có manh mối nào cả. Trong trường hợp này, mọi Sudoku hợp lệ đều có thể đạt được mà không cần xóa, vì vậy câu trả lời bằng tổng số lưới Sudoku hợp lệ 4 x 4. Thuật toán xử lý việc này một cách tự nhiên vì danh sách đầu mối trống, vì vậy mọi giải pháp đều có số lượng không khớp bằng 0. 

Một trường hợp khác là khi k bằng 16, số manh mối tối đa có thể có. Ngay cả khi mọi manh mối xung đột với một giải pháp đề xuất, chúng ta vẫn có thể xóa tất cả. Thuật toán đếm chính xác tất cả các lưới Sudoku hợp lệ vì mỗi số không khớp là 16. 

Trường hợp tinh tế cuối cùng là vị trí đầu mối không nhất quán. Ví dụ: một hàng có thể chứa các chữ số lặp lại. Điều này không phá vỡ tính chính xác vì thuật toán không bao giờ xác thực chính câu đố đó; nó chỉ so sánh các giải pháp ứng viên với những manh mối hiện có. Mỗi manh mối xung đột chỉ đơn giản là tăng chi phí xóa một cách độc lập, phù hợp chính xác với định nghĩa vấn đề.

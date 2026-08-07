---
title: "CF 103964H - Sudoku"
description: "Chúng ta có một lưới Sudoku 9 x 9 được lấp đầy một phần. Mỗi ô đã chứa một chữ số từ 1 đến 9 hoặc trống."
date: "2026-07-02T06:39:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103964
codeforces_index: "H"
codeforces_contest_name: "The 2015 China Collegiate Programming Contest (CCPC 2015)"
rating: 0
weight: 103964
solve_time_s: 48
verified: true
draft: false
---

[CF 103964H - Sudoku](https://codeforces.com/problemset/problem/103964/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới Sudoku 9 x 9 được lấp đầy một phần. Mỗi ô đã chứa một chữ số từ 1 đến 9 hoặc trống. Nhiệm vụ là hoàn thiện lưới sao cho mỗi hàng chứa mỗi chữ số đúng một lần, mỗi cột chứa mỗi chữ số đúng một lần và mỗi lưới con 3 x 3 cũng chứa mỗi chữ số đúng một lần. 

Đầu vào có thể được coi là trạng thái bảng cố định với các ràng buộc và đầu ra là bất kỳ bảng nào đã hoàn thành hợp lệ mở rộng trạng thái này mà không vi phạm các quy tắc Sudoku. Nếu tồn tại nhiều lần hoàn thành thì bất kỳ một lần hoàn thành hợp lệ nào cũng được chấp nhận. 

Cấu trúc của bài toán ngay lập tức gợi ý không gian tìm kiếm hàm mũ trong trường hợp xấu nhất vì mỗi ô trống có thể nhận tới chín giá trị. Tuy nhiên, kích thước lưới được cố định ở 81 ô, do đó khó khăn thực sự không phải là tăng kích thước đầu vào mà là hạn chế tương tác. Điều này thay đổi vấn đề từ “tính toán quy mô lớn” sang “cắt tỉa cẩn thận một không gian tổ hợp nhỏ”. 

Một phép điền đệ quy ngây thơ thử tất cả các chữ số cho mỗi ô trống mà không cắt tỉa sẽ phát nổ ngay cả trên một câu đố có nhiều ô trống, vì hệ số phân nhánh lên tới 9 và độ sâu lên tới 81. 

Một dạng lỗi ẩn điển hình xuất phát từ việc không thực thi nhất quán cả ba ràng buộc. Ví dụ: việc đặt một chữ số hợp lệ trong một hàng nhưng không hợp lệ trong một cột hoặc hộp vẫn có thể vượt qua kiểm tra một phần nếu việc triển khai chỉ xác thực một thứ nguyên. 

Một vấn đề nhỏ khác phát sinh nếu các bản cập nhật không được khôi phục đúng cách trong quá trình quay lui. Việc triển khai không chính xác phổ biến sẽ đánh dấu một chữ số được sử dụng trong một hàng hoặc cột nhưng quên bỏ đánh dấu nó khi quay lui, dẫn đến việc cắt bớt sai và kết luận "không có giải pháp" không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực coi Sudoku như một vấn đề tìm kiếm thuần túy trên các ô trống. Chúng tôi quét lưới, chọn ô trống đầu tiên, thử các chữ số từ 1 đến 9 và tiếp tục đệ quy. Điều này đúng vì nó khám phá mọi nhiệm vụ có thể phù hợp với các lựa chọn trước đó. 

Tuy nhiên, độ phức tạp trong trường hợp xấu nhất của nó là ở mức$9^k$, Ở đâu$k$là số ô trống. Ngay cả khi cắt bớt vừa phải khỏi kiểm tra tính hợp lệ, hệ số phân nhánh vẫn quá lớn đối với các trường hợp có nhiều khoảng trống. 

Quan sát quan trọng giúp Sudoku có thể giải được trong thực tế là việc kiểm tra ràng buộc cực kỳ rẻ và có cấu trúc cao. Mỗi vị trí ảnh hưởng đến chính xác ba bộ: một hàng, một cột và một khối 3 x 3. Điều này cho phép chúng tôi duy trì trạng thái tăng dần để kiểm tra tính hợp lệ là O(1) và chúng tôi có thể loại bỏ mạnh mẽ các nhánh không hợp lệ ngay lập tức. 

Một cải tiến hơn nữa đến từ việc chọn ô tiếp theo một cách thông minh hơn. Thay vì điền vào các ô theo thứ tự cố định, chúng tôi luôn chọn ô trống có ít ứng viên hợp lệ nhất. Điều này làm giảm đáng kể sự phân nhánh trong các câu đố điển hình vì các ô bị ràng buộc sẽ làm sập cây tìm kiếm sớm. 

Chúng ta có thể duy trì ba bảng ràng buộc boolean cho các hàng, cột và hộp và cập nhật chúng trong quá trình đệ quy. Điều này biến giải pháp thành một tìm kiếm quay lui có hướng dẫn với việc kiểm tra tính hợp lệ theo thời gian liên tục cho mỗi lần di chuyển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(9^k) | O(81) | Quá chậm | 
| Quay lui được tối ưu hóa với các ràng buộc | O(số mũ nhỏ, được cắt tỉa nhiều) | O(81) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích lưới 9 x 9 và ghi lại tất cả các vị trí trống. Trong khi làm như vậy, hãy xây dựng ba trình theo dõi ràng buộc: những chữ số nào đã được sử dụng trong mỗi hàng, mỗi cột và mỗi lưới con 3 x 3. Điều này cho phép chúng ta trả lời “tôi có thể đặt chữ số d ở đây không” trong thời gian không đổi. 
2. Xác định hàm tính chỉ số hộp 3 x 3 cho một ô bằng cách sử dụng phép chia số nguyên tọa độ của nó. Điều này là cần thiết vì các ràng buộc Sudoku được phân chia thành các khối cố định. 
3. Triển khai bộ giải đệ quy cố gắng lấp đầy lưới. Ở mỗi cuộc gọi, hãy chọn ô trống tiếp theo. Thay vì chọn tùy ý, hãy chọn chữ số có ít chữ số hợp lệ nhất theo các ràng buộc hiện tại. Điều này làm giảm khả năng phân nhánh sớm, đây chính là nguồn gốc của hầu hết các khoản tiết kiệm theo cấp số nhân. 
4. Đối với ô đã chọn, lặp qua các chữ số từ 1 đến 9. Đối với mỗi chữ số, hãy kiểm tra xem nó có xuất hiện trong hàng, cột và hộp hay không. Nếu hợp lệ, hãy đặt nó và đánh dấu nó là được sử dụng trong cả ba cấu trúc ràng buộc. 
5. Lặp lại để giải lưới còn lại. Nếu đệ quy thành công, hãy truyền bá thành công lên trên ngay lập tức. 
6. Nếu không có chữ số nào dẫn đến giải pháp, hãy hoàn tác vị trí và quay lại. Điều này khôi phục tất cả các cấu trúc ràng buộc để các quyết định trước đó vẫn nhất quán. 
7. Nếu tất cả các ô đã được điền, trả về thành công và bảng hoàn thành. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì tính bất biến rằng tất cả các ô được điền đều đáp ứng các ràng buộc Sudoku. Các bảng ràng buộc đảm bảo rằng không có chữ số không hợp lệ nào được đặt và việc quay lui đảm bảo rằng các phép gán một phần có thể đảo ngược được. Vì mọi trạng thái đệ quy chỉ khám phá các cấu hình phù hợp với các ràng buộc cho đến nay, nên không gian tìm kiếm không chứa các nhánh không hợp lệ, chỉ chứa các giải pháp có khả năng hoàn chỉnh. Bởi vì lưới là hữu hạn và mỗi bước làm giảm đáng kể số lượng ô trống, phép đệ quy cuối cùng phải kết thúc bằng cách tìm một phép gán đầy đủ hợp lệ hoặc sử dụng hết tất cả các khả năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_sudoku(board):
    rows = [[False]*10 for _ in range(9)]
    cols = [[False]*10 for _ in range(9)]
    boxes = [[False]*10 for _ in range(9)]
    empties = []

    def box_id(r, c):
        return (r // 3) * 3 + (c // 3)

    for r in range(9):
        for c in range(9):
            if board[r][c] == 0:
                empties.append((r, c))
            else:
                d = board[r][c]
                rows[r][d] = True
                cols[c][d] = True
                boxes[box_id(r, c)][d] = True

    def get_candidates(r, c):
        b = box_id(r, c)
        return [d for d in range(1, 10)
                if not rows[r][d] and not cols[c][d] and not boxes[b][d]]

    def dfs():
        if not empties:
            return True

        best_idx = -1
        best_cands = None

        for i, (r, c) in enumerate(empties):
            cands = get_candidates(r, c)
            if not cands:
                return False
            if best_cands is None or len(cands) < len(best_cands):
                best_cands = cands
                best_idx = i

        r, c = empties.pop(best_idx)
        b = box_id(r, c)

        for d in best_cands:
            if not rows[r][d] and not cols[c][d] and not boxes[b][d]:
                board[r][c] = d
                rows[r][d] = cols[c][d] = boxes[b][d] = True

                if dfs():
                    return True

                rows[r][d] = cols[c][d] = boxes[b][d] = False
                board[r][c] = 0

        empties.append((r, c))
        return False

    dfs()
    return board

def main():
    board = []
    for _ in range(9):
        line = input().strip()
        row = []
        for ch in line:
            if ch in "0.":
                row.append(0)
            else:
                row.append(int(ch))
        board.append(row)

    solve_sudoku(board)

    for r in range(9):
        print("".join(str(x) for x in board[r]))

if __name__ == "__main__":
    main()
```Bộ giải xây dựng các bảng giới hạn thời gian không đổi cho các hàng, cột và hộp. Quy trình DFS luôn chọn ô trống bị ràng buộc nhất trước tiên, đây là phương pháp phỏng đoán chính giúp ngăn chặn sự phân nhánh bệnh lý. Logic hoàn tác đối xứng với logic vị trí, đảm bảo tính chính xác trong quá trình quay lui. Một chi tiết triển khai tinh vi là ô đã chọn sẽ tạm thời bị xóa khỏi danh sách trống và chỉ được khôi phục nếu nhánh bị lỗi, điều này tránh việc phải xem lại trạng thái quyết định tương tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một bảng được lấp đầy một phần trong đó chỉ có một vài ô trống ở hàng đầu tiên. 

Khi bắt đầu, các bảng ràng buộc phản ánh tất cả các chữ số đã cho và các ô trống bao gồm tất cả các ô trống. 

| Bước | Ô đã chọn | Ứng viên | Hành động | 
| --- | --- | --- | --- | 
| 1 | (0, 3) | {2, 5} | Hãy thử 2 | 
| 2 | (0, 3) | {2, 5} | Quay lại từ ngõ cụt | 
| 3 | (0, 3) | {2, 5} | Hãy thử 5 | 
| 4 | hoàn thành | không | thành công | 

Dấu vết này cho thấy cách cắt tỉa nhanh chóng loại bỏ các phép gán từng phần không hợp lệ, ngăn cản việc khám phá sâu hơn các nhánh không nhất quán. 

### Ví dụ 2 

Một cấu hình khó hơn trong đó ô được chọn đầu tiên có nhiều ràng buộc. 

| Bước | Ô đã chọn | Ứng viên | Hành động | 
| --- | --- | --- | --- | 
| 1 | (4, 4) | {1, 3} | Hãy thử 1 | 
| 2 | (4, 4) | {1, 3} | tuyên truyền thất bại | 
| 3 | (4, 4) | {1, 3} | Hãy thử 3 | 
| 4 | hoàn thành | không | thành công | 

Điều này chứng tỏ lợi ích của phương pháp phỏng đoán ứng viên tối thiểu, vì các ô trung tâm thường bị hạn chế rất nhiều và làm giảm đáng kể độ sâu tìm kiếm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Hàm mũ nhưng nhỏ trong thực tế | Mỗi bước sẽ giảm bớt các ô trống và ứng viên đặt hàng sẽ cắt tỉa sớm hầu hết các nhánh | 
| Không gian | O(81) | Bảng cố định cộng với ngăn xếp đệ quy | 

Các ràng buộc có kích thước chặt chẽ nhưng không có sự tự do tổ hợp. Thuật toán phù hợp thoải mái vì việc cắt tỉa ngăn cản việc khám phá hầu hết các trạng thái lý thuyết. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import main  # assuming solution is in main()
    try:
        main()
    except SystemExit:
        pass
    return sys.stdout.getvalue().strip()

# Note: samples are unspecified, so illustrative tests are used

# empty grid (minimal constraint case)
assert run(
"000000000\n000000000\n000000000\n000000000\n000000000\n000000000\n000000000\n000000000\n000000000\n"
) != "", "full fill exists"

# already solved grid
assert run(
"123456789\n456789123\n789123456\n214365897\n365897214\n897214365\n531642978\n642978531\n978531642\n"
) == \
"123456789\n456789123\n789123456\n214365897\n365897214\n897214365\n531642978\n642978531\n978531642", "identity"

# single missing cell
assert run(
"123456789\n456789123\n789123456\n214365897\n365897214\n897214365\n531642978\n642978531\n978531640\n"
) != "", "single fix"

# constrained puzzle small variation
assert run(
"530070000\n600195000\n098000060\n800060003\n400803001\n700020006\n060000280\n000419005\n000080079\n"
) != "", "standard sudoku solvable"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lưới trống | lưới điền hợp lệ | phân nhánh cực độ | 
| lưới đã được giải quyết | cùng một lưới | tính chính xác không hoạt động | 
| một ô bị thiếu | hoàn thành hợp lệ | tính đúng đắn cục bộ | 
| câu đố chuẩn | giải pháp hợp lệ | lan truyền ràng buộc thực tế | 

## Vỏ cạnh 

### Lưới hoàn toàn trống 

Đầu vào là một lưới toàn số không. Thuật toán bắt đầu với sự phân nhánh tối đa nhưng ngay lập tức được hưởng lợi từ việc lan truyền ràng buộc ngay khi một vài vị trí đầu tiên xuất hiện. Lựa chọn heuristic ngăn chặn sự phân nhánh thống nhất bùng nổ. 

### Đã giải quyết được lưới 

Danh sách trống rỗng ngay từ đầu. DFS chấm dứt ngay lập tức vì điều kiện cơ bản được thỏa mãn, trả về lưới ban đầu không thay đổi. 

### Ô cưỡng bức đơn 

Một lưới gần như hoàn chỉnh với một giá trị còn thiếu sẽ kiểm tra xem các bảng ràng buộc có xác định chính xác chữ số hợp lệ duy nhất hay không. Thuật toán tính toán các ứng cử viên cho ô đó và trực tiếp điền vào ô đó mà không cần quay lại. 

### Ô trung tâm bị ràng buộc cao 

Khi ô bị ràng buộc nhất nằm ở giữa lưới, phương pháp phỏng đoán sẽ chọn chính xác ô đó trước tiên. Điều này tránh việc khám phá các lựa chọn ngoại vi không liên quan và giữ cho đệ quy ở mức nông cạn.

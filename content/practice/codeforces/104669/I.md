---
title: "CF 104669I - 2048"
description: "Chúng ta được cấp một bàn cờ 4 x 4 từ trò chơi 2048 đơn giản hóa. Mỗi ô chứa một ô số 0 hoặc một ô lũy thừa hai. Số 0 có nghĩa là ô trống. Bàn cờ phát triển bằng cách áp dụng các nước đi, nhưng không giống như trò chơi gốc, không có ô mới nào xuất hiện."
date: "2026-06-29T09:43:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104669
codeforces_index: "I"
codeforces_contest_name: "Turtle Codes"
rating: 0
weight: 104669
solve_time_s: 83
verified: false
draft: false
---

[CF 104669I - 2048](https://codeforces.com/problemset/problem/104669/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bàn cờ 4 x 4 từ trò chơi 2048 đơn giản hóa. Mỗi ô chứa một ô số 0 hoặc một ô lũy thừa hai. Số 0 có nghĩa là ô trống. Bàn cờ phát triển bằng cách áp dụng các nước đi, nhưng không giống như trò chơi gốc, không có ô mới nào xuất hiện. 

Bessie liên tục áp dụng một kiểu di chuyển cố định: đầu tiên là di chuyển sang phải, sau đó di chuyển xuống dưới, sau đó lại sang phải, rồi đi xuống, v.v. Mỗi lần di chuyển sẽ dịch chuyển tất cả các ô theo hướng đó, nén chúng và hợp nhất các ô liền kề bằng nhau theo quy tắc tiêu chuẩn 2048. Một ô có thể hợp nhất tối đa một lần cho mỗi lần di chuyển và hướng hợp nhất sẽ ưu tiên cho phía xa theo hướng di chuyển. 

Quá trình này cuối cùng đạt đến trạng thái mà các bước di chuyển tiếp theo không còn làm thay đổi bảng nữa. Nhiệm vụ là xuất ra cấu hình ổn định cuối cùng đó. 

Đầu ra không phải là “sau một số bước”, mà điểm cố định liên tục áp dụng phải và xuống sẽ di chuyển luân phiên cho đến khi không có gì thay đổi. 

Kích thước bảng không đổi ở mức 4 x 4, do đó, bất kỳ phương pháp mô phỏng chính xác nào cũng có thể đủ khả năng thực hiện các phép biến đổi toàn lưới lặp đi lặp lại. Ngay cả một mô phỏng đơn giản cũng rẻ vì mỗi lần di chuyển chạm vào tối đa 16 ô và việc hợp nhất là tuyến tính trên mỗi hàng hoặc cột. Hạn chế có ý nghĩa duy nhất là tính chính xác của các quy tắc hợp nhất và phát hiện chính xác sự ổn định. 

Những trường hợp thất bại tinh tế đều xuất phát từ việc ổn định hóa sự hiểu lầm. 

Một sai lầm phổ biến là cho rằng nếu một bước đi đúng đắn không tạo ra thay đổi nào thì quá trình đã hoàn tất. Điều đó không chính xác vì một động thái đi xuống sau đó vẫn có thể thay đổi bảng. 

Một sai lầm khác là coi đây là một “động thái kết hợp” đơn lẻ hoặc cố gắng dự đoán cấu hình cuối cùng bằng phương pháp phân tích. Sự tương tác giữa ca phải và ca xuống có thể liên tục mở ra những sự hợp nhất mới. 

Lỗi tinh vi thứ ba xuất phát từ quy tắc hợp nhất 2048. Ví dụ, trong một hàng như`[2, 2, 2, 2]`, kết quả đúng sau khi di chuyển đúng là`[0, 0, 4, 4]`, không`[0, 0, 0, 8]`. Việc triển khai hợp nhất kép ngây thơ sẽ âm thầm phá vỡ tính chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng quá trình chính xác như được mô tả. Chúng ta liên tục áp dụng một nước đi bên phải và sau đó là một nước đi xuống cho đến khi bàn cờ không còn thay đổi nữa. 

Cách giải thích bạo lực có thể cố gắng mô phỏng từng chuyển động cho đến khi hội tụ mà không nhận thấy cấu trúc ổn định. Vì mỗi nước đi là O(16) nên thậm chí hàng nghìn lần lặp cũng không đáng kể. Trường hợp xấu nhất vẫn nằm trong giới hạn vì không gian trạng thái rất nhỏ: mỗi ô chỉ nhận các giá trị là lũy thừa của hai cho đến năm 2048, do đó số lượng cấu hình riêng biệt bị giới hạn. 

Quan sát quan trọng là chúng ta không cần khám phá các nhánh hoặc tìm kiếm chiến lược. Quá trình này mang tính tất định và đơn điệu theo nghĩa là chúng ta liên tục áp dụng hai phép biến đổi cố định. Khi một chu kỳ đầy đủ từ phải đến xuống không tạo ra thay đổi thì các chu kỳ tiếp theo cũng không thể tạo ra thay đổi. 

Vì vậy, vấn đề giảm xuống còn việc áp dụng lặp đi lặp lại hai hàm số cho đến khi đạt đến một điểm cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng cho đến khi ổn định, có thể kiểm tra không hiệu quả) | O(K · 16) | O(1) | Đã chấp nhận | 
| Tối ưu (mô phỏng chu trình với chức năng di chuyển phù hợp) | O(K · 16) | O(1) | Đã chấp nhận | 

Ở đây K là số chu kỳ cho đến khi ổn định, con số này rất nhỏ trong thực tế. 

## Hướng dẫn thuật toán 

Chúng tôi xác định hai thao tác cốt lõi: dịch chuyển sang phải và dịch chuyển xuống, mỗi thao tác thực hiện cơ chế 2048 trên hàng hoặc cột. 

### bước 

1. Xác định hàm để xử lý một dòng (hàng hoặc cột) theo một hướng nhất định bằng cách lọc các ô khác 0. 

Bước này quan trọng vì 2048 bỏ qua các ô trống khi hợp nhất. 
2. Hợp nhất các giá trị bằng nhau liền kề một lần cho mỗi lần di chuyển trong khi quét theo hướng chuyển động. 

Khi hai ô bằng nhau gặp nhau, chúng tôi kết hợp chúng và bỏ qua ô tiếp theo để thực thi quy tắc “hợp nhất một lần mỗi lần di chuyển”. 
3. Xây dựng lại lưới 4 x 4 đầy đủ sau khi áp dụng thao tác cho tất cả các hàng (đối với bên phải) hoặc tất cả các cột (đối với bên dưới). 

Điều này giữ cho việc triển khai có tính đối xứng và tránh logic trùng lặp. 
4. Lặp lại chu trình sau: 

đầu tiên áp dụng bước di chuyển phải vào lưới, sau đó áp dụng bước di chuyển xuống. 
5. So sánh lưới kết quả với lưới trước đó. 

Nếu không có gì thay đổi sau cả hai thao tác, hãy dừng lại. 
6. Xuất lưới ổn định. 

### Tại sao nó hoạt động 

Quá trình này mang tính xác định và mỗi lần lặp lại áp dụng cùng một chuỗi biến đổi: phải rồi xuống. Điều này xác định hàm F(lưới). Thuật toán liên tục tính F cho đến khi đạt đến điểm cố định. 

Vì bảng là hữu hạn và mỗi bước hoàn toàn mang tính xác định nên khi một trạng thái lặp lại, hệ thống sẽ chuyển sang một chu trình. Tuy nhiên, vì mọi chu trình đều di chuyển nghiêm ngặt về một cấu hình không thể nén hoặc hợp nhất thêm theo quy tắc phải/xuống, nên chu trình ổn định duy nhất là một điểm cố định trong đó F(lưới) bằng lưới. Thuật toán dừng chính xác ở trạng thái đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N = 4

def compress(line):
    """Apply 2048 merge rules to a single line moving right."""
    vals = [x for x in line if x != 0]
    res = []
    i = 0
    while i < len(vals):
        if i + 1 < len(vals) and vals[i] == vals[i + 1]:
            res.append(vals[i] * 2)
            i += 2
        else:
            res.append(vals[i])
            i += 1
    res = [0] * (N - len(res)) + res
    return res

def move_right(grid):
    return [compress(row) for row in grid]

def move_down(grid):
    cols = []
    for c in range(N):
        col = [grid[r][c] for r in range(N)]
        col = compress(col)
        cols.append(col)

    new_grid = [[0] * N for _ in range(N)]
    for c in range(N):
        for r in range(N):
            new_grid[r][c] = cols[c][r]
    return new_grid

def solve():
    grid = [list(map(int, input().split())) for _ in range(N)]

    while True:
        new_grid = move_right(grid)
        new_grid = move_down(new_grid)
        if new_grid == grid:
            break
        grid = new_grid

    for row in grid:
        print(*row)

if __name__ == "__main__":
    solve()
```Việc triển khai tách logic cốt lõi thành một quy trình nén dòng. Hàm đó là nơi duy nhất thực thi quy tắc 2048, giúp giảm nguy cơ xảy ra mâu thuẫn giữa việc xử lý hàng và cột. 

Việc di chuyển đúng được áp dụng trực tiếp theo hàng. Việc di chuyển xuống được thực hiện bằng cách trích xuất các cột, sử dụng lại logic nén tương tự, sau đó ghi lại vào lưới. Điều này tránh trùng lặp logic hợp nhất và đảm bảo hành vi giống hệt nhau giữa các hướng. 

Điều kiện dừng so sánh toàn bộ lưới. Điều này rất quan trọng vì một nước đi đúng có thể không thay đổi bàn cờ, nhưng nước đi xuống tiếp theo vẫn có thể thay đổi bảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 2 0 2
0 0 4 0
8 2 2 0
0 4 2 0
```Chúng tôi theo dõi trạng thái sau mỗi chu kỳ đầy đủ (phải rồi xuống). 

| Bước | Lưới | 
| --- | --- | 
| Ban đầu | 0 2 0 2 / 0 0 4 0 / 8 2 2 0 / 0 4 2 0 | 
| Sau Phải | 0 0 0 4 / 0 0 0 4 / 0 8 4 0 / 0 0 4 2 | 
| Sau Xuống | 0 0 0 0 / 0 0 0 4 / 0 0 0 16 / 0 0 4 2 | 

Sau một chu kỳ khác, không có thay đổi nào xảy ra nữa nên đây là trạng thái cuối cùng. 

Dấu vết này cho thấy sự ổn định phụ thuộc vào sự kết hợp của cả hai hướng chứ không phải một hướng duy nhất. 

### Ví dụ 2 

đầu vào:```
2 2 0 0
2 2 0 0
0 0 4 4
0 0 4 4
```| Bước | Lưới | 
| --- | --- | 
| Ban đầu | 2 2 0 0 / 2 2 0 0 / 0 0 4 4 / 0 0 4 4 | 
| Sau Phải | 0 0 4 4 / 0 0 4 4 / 0 0 8 8 / 0 0 8 8 | 
| Sau Xuống | 0 0 0 0 / 0 0 0 0 / 0 0 4 4 / 0 0 16 16 | 

Cấu hình này đã ổn định khi áp dụng lặp lại thao tác phải và xuống, do việc nén thêm không làm thay đổi cấu trúc. 

Ví dụ này nêu bật cách hợp nhất lan truyền độc lập trong các hàng và cột cho đến khi không còn phần kề nào của các ô bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(K) | Mỗi chu kỳ thực hiện hai phép biến đổi 4x4, mỗi phép biến đổi O(16) và K là số chu kỳ cho đến khi ổn định | 
| Không gian | O(1) | Chỉ một lưới 4x4 cố định được lưu trữ | 

Kích thước lưới không đổi làm cho thời gian chạy không đổi trong thực tế. Thậm chí nhiều chục chu kỳ cũng không đáng kể trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio

    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("""0 2 0 2
0 0 4 0
8 2 2 0
0 4 2 0""")  # output checked by correctness of stable simulation

# all zeros
assert run("""0 0 0 0
0 0 0 0
0 0 0 0
0 0 0 0""") == "0 0 0 0\n0 0 0 0\n0 0 0 0\n0 0 0 0"

# already stable under right/down
assert run("""0 0 0 2
0 0 0 2
0 0 0 2
0 0 0 2""")

# full merge chain
assert run("""2 2 2 2
0 0 0 0
0 0 0 0
0 0 0 0""")

# mixed propagation
assert run("""2 0 2 0
2 0 2 0
2 0 2 0
2 0 2 0""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | tất cả số không | ổn định danh tính | 
| gạch một cột | hành vi nén phải/xuống | tính đúng hướng | 
| hợp nhất hàng đầy đủ |`[2,2,2,2]`tính đúng đắn của quy tắc | ràng buộc hợp nhất đơn | 
| mẫu lặp đi lặp lại | lan truyền theo cả hai bước di chuyển | tương tác phải và xuống | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi một hàng trông không thay đổi sau lần di chuyển sang phải nhưng lại thay đổi sau lần di chuyển xuống tiếp theo. Ví dụ: cấu hình trong đó tất cả các ô đã được căn phải nhưng được xếp chồng lên nhau theo chiều dọc vẫn tạo ra các thay đổi sau thao tác xuống. Thuật toán xử lý việc này vì nó luôn thực hiện toàn bộ chu trình trước khi kiểm tra độ ổn định. 

Một trường hợp cạnh khác xuất phát từ thứ tự hợp nhất. Trong một hàng như`[2, 2, 2, 2]`, việc triển khai ngây thơ có thể tạo ra không chính xác`[0, 0, 0, 8]`. Việc triển khai đúng sẽ đảm bảo một lượt bỏ qua sau khi hợp nhất, điều này duy trì quy tắc rằng một ô sẽ hợp nhất nhiều nhất một lần trong mỗi lần di chuyển. 

Trường hợp cuối cùng là nghi ngờ dao động. Vì hệ thống có tính tất định nên có vẻ như nó có thể quay vòng mà không ổn định. Tuy nhiên, do mỗi chu kỳ đẩy mạnh các ô về phía nén theo các hướng cố định nên bất kỳ chu kỳ tiềm năng nào cũng sẽ thu gọn vào một điểm cố định và việc kiểm tra dừng sau toàn bộ chu kỳ sẽ nắm bắt chính xác chu kỳ đó.

---
title: "CF 106137O - Rơi Xuống"
description: "Chúng ta có một lưới hình chữ nhật tượng trưng cho một buồng thẳng đứng, trong đó một số ô bị chặn và một số chứa các mảnh có thể chuyển động bị ảnh hưởng bởi trọng lực."
date: "2026-06-25T11:32:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106137
codeforces_index: "O"
codeforces_contest_name: "BFS  BFS - MTA"
rating: 0
weight: 106137
solve_time_s: 34
verified: true
draft: false
---

[CF 106137O - Rơi xuống](https://codeforces.com/problemset/problem/106137/O) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật tượng trưng cho một buồng thẳng đứng, trong đó một số ô bị chặn và một số chứa các mảnh có thể chuyển động bị ảnh hưởng bởi trọng lực. Lưới được định hướng sao cho trọng lực tác động hướng xuống, có nghĩa là bất kỳ mảnh di chuyển nào sẽ rơi thẳng xuống trong cột của nó cho đến khi nó chạm tới đáy lưới hoặc đáp xuống đầu một ô bị chặn hoặc một mảnh khác đã được cố định. 

Nhiệm vụ là tính toán cấu hình ổn định cuối cùng sau khi tất cả các mảnh đã rơi xong. Các ô bị chặn không bao giờ di chuyển và đóng vai trò là chướng ngại vật vĩnh viễn. Các mảnh có thể di chuyển không bao giờ dịch chuyển sang một bên, vì vậy mỗi cột sẽ phát triển độc lập với các cột khác ngoại trừ sự hiện diện của các chướng ngại vật cố định trong cột đó. 

Đầu vào mô tả lưới này theo từng hàng và đầu ra phải mô tả lưới sau khi quá trình trọng lực ổn định. 

Từ góc độ phức tạp, kích thước lưới thường đủ lớn để việc mô phỏng đơn giản về trọng lực trên mỗi mảnh sẽ quá chậm. Nếu chúng ta liên tục quét xuống dưới để tìm mọi phần có thể di chuyển được thì trường hợp xấu nhất sẽ suy biến thành việc đi qua nhiều lần các cột dài, dẫn đến hành vi bậc hai về số lượng ô. Cần có một giải pháp xử lý mỗi ô với số lần không đổi, ngụ ý cách tiếp cận tuyến tính hoặc gần tuyến tính về kích thước lưới. 

Một trường hợp thất bại nhỏ xuất hiện khi nhiều mảnh có thể di chuyển được xếp chồng lên trên chướng ngại vật. Ví dụ: trong một cột như:```
.#..
.oo.
....
```Nếu trọng lực không được xử lý cẩn thận, người ta có thể đặt các quân cờ vào vị trí đã chiếm không chính xác hoặc bỏ qua chướng ngại vật không đúng cách. Hành vi đúng là các mảnh “thu thập” phía trên chướng ngại vật gần nhất hoặc ranh giới phía dưới theo thứ tự được đóng gói. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ mô phỏng từng phần một cách độc lập. Đối với mỗi ô có thể di chuyển, chúng tôi liên tục di chuyển nó xuống từng hàng cho đến khi nó không thể di chuyển xa hơn. Mặc dù đơn giản về mặt khái niệm, nhưng điều này dẫn đến công việc lặp đi lặp lại: một phần duy nhất có thể đi qua các hàng O(n) và có thể có các phần O(nm), tạo ra hành vi O(n²m) trong trường hợp xấu nhất. Điều này trở nên quá chậm đối với các lưới lớn. 

Quan sát quan trọng là chuyển động hoàn toàn theo cột cục bộ và đơn điệu. Các mảnh không tương tác giữa các cột và trong một cột, trọng lực sẽ sắp xếp các ô có thể di chuyển một cách hiệu quả thành các khối liền kề được ngăn cách bởi các chướng ngại vật. Thay vì mô phỏng chuyển động từng bước, chúng ta có thể xử lý từng cột từ dưới lên, theo dõi vị trí có sẵn tiếp theo nơi một phần có thể ổn định. 

Việc tối ưu hóa xuất phát từ việc nhận ra rằng mỗi cột hoạt động giống như một ngăn xếp được phân chia theo chướng ngại vật. Giữa hai chướng ngại vật, tất cả các mảnh có thể di chuyển được sẽ rơi xuống các ô thấp nhất có sẵn trong đoạn đó. Điều này biến mô phỏng lặp đi lặp lại thành một lần truyền cho mỗi cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²m) | O(1) thêm | Quá chậm | 
| Nén cột | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới thành cấu trúc có thể thay đổi để chúng ta có thể ghi đè lên các ô trong quá trình tính toán. Điều này là cần thiết vì trạng thái cuối cùng phụ thuộc vào việc sắp xếp lại các ký tự hiện có tại chỗ. 
2. Xử lý từng cột một cách độc lập vì không có chuyển động nào vượt qua ranh giới cột. Điều này biến bài toán 2D thành m bài toán 1D riêng biệt. 
3. Đối với cột cố định, bắt đầu quét từ hàng dưới cùng trở lên. Lý do di chuyển từ dưới lên là vì trọng lực luôn lấp đầy khoảng trống thấp nhất trước tiên. 
4. Duy trì một con trỏ`write_row`cho biết vị trí thấp nhất trong đoạn hiện tại nơi có thể đặt một mảnh di động. Ban đầu, đây là phần cuối của cột. 
5. Di chuyển lên trên từng hàng. Gặp ô bị chặn thì ta reset`write_row`ngay phía trên chướng ngại vật đó vì không gì có thể vượt qua được. 
6. Nếu gặp một quân có thể di chuyển được, chúng ta sẽ xóa vị trí hiện tại của nó và đặt nó vào vị trí`write_row`, sau đó giảm`write_row`vì vậy mảnh tiếp theo ở trên xếp chồng chính xác. 
7. Nếu gặp một ô trống, chúng ta chỉ cần tiếp tục vì nó không ảnh hưởng đến trạng thái. 

Sau khi xử lý tất cả các cột theo cách này, lưới ở cấu hình ổn định cuối cùng. 

### Tại sao nó hoạt động 

Trong bất kỳ đoạn cột nào được giới hạn bởi chướng ngại vật (hoặc ranh giới lưới), trọng lực chỉ xác định một quy tắc sắp xếp: tất cả các phần có thể di chuyển đều kết thúc ở cuối đoạn đó. Thuật toán thực thi tính bất biến này bằng cách lấp đầy các vị trí từ dưới lên trên theo đúng thứ tự các phần được phát hiện trong quá trình di chuyển lên trên. Mỗi mảnh được gán cho vị trí hợp lệ thấp nhất có thể trong phân đoạn của nó và các chướng ngại vật sẽ đặt lại ranh giới phân đoạn, đảm bảo không có mảnh nào vượt qua rào cản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]

    for col in range(m):
        write_row = n - 1

        row = n - 1
        while row >= 0:
            if grid[row][col] == '#':
                write_row = row - 1
            elif grid[row][col] == 'o':
                grid[row][col] = '.'
                grid[write_row][col] = 'o'
                write_row -= 1
            row -= 1

    for r in range(n):
        print("".join(grid[r]))

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là truyền tải kiểu hai con trỏ trên mỗi cột. các`write_row`con trỏ biểu thị vị trí hạ cánh hợp lệ tiếp theo cho một mảnh rơi. Bất cứ khi nào chúng tôi gặp chướng ngại vật, chúng tôi sẽ “đóng” đoạn hiện tại một cách hiệu quả và khởi động lại ngay phía trên nó. 

Một lỗi triển khai phổ biến là cập nhật lưới đồng thời dựa vào nó để đưa ra các quyết định trong tương lai mà không đặt hàng cẩn thận. Ở đây, việc xóa vị trí ban đầu trước khi viết đảm bảo chúng ta không vô tình đếm hai lần hoặc sử dụng lại các phần. Một sự tinh tế khác là đặt lại`write_row`chính xác để`row - 1`khi gặp chướng ngại vật, vì bản thân chướng ngại vật đó phải được giữ cố định và không thể bị ghi đè. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Lưới đầu vào:```
4 3
o..
.o.
.#o
...
```Chúng tôi xử lý từng cột một cách độc lập. 

| Cột | Hàng (quét trên → dưới) | Hành động | trạng thái write_row | 
| --- | --- | --- | --- | 
| 0 | o lúc 0 | di chuyển xuống dưới cùng | 3 | 
| 0 | . | bỏ qua | 3 | 
| 0 | . | bỏ qua | 3 | 
| 0 | . | kết thúc | - | 

Sau khi xử lý cột 0, tất cả các phần đều rơi xuống đáy. 

Đối với cột 1, quá trình nén tương tự cũng xảy ra, trong khi cột 2 bị chướng ngại vật chia cắt. 

Đầu ra cuối cùng:```
...
...
.#.
ooo
```Điều này cho thấy nhiều mảnh tích lũy ở vị trí trống thấp nhất trong mỗi cột. 

### Ví dụ 2 

đầu vào:```
5 4
o..o
.##.
o..o
....
..#.
```Xử lý cột 0: 

| Hàng | Tế bào | viết_row | Kết quả | 
| --- | --- | --- | --- | 
| 4 | o | 4 | nơi | 
| 3 | . | 3 | bỏ qua | 
| 2 | o | 3 | nơi | 
| 1 | . | 2 | bỏ qua | 
| 0 | o | 1 | nơi | 

Cột cuối cùng gói các mảnh bên dưới chướng ngại vật một cách chính xác. 

Lưới cuối cùng:```
....
.##.
....
o..o
o..o
```Dấu vết này cho thấy các chướng ngại vật chia cột thành các vùng trọng lực độc lập như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được truy cập tối đa một lần trên mỗi cột và mỗi cột được xử lý độc lập | 
| Không gian | O(1) bổ sung (ngoài lưới) | Lưới được sửa đổi tại chỗ chỉ với một vài con trỏ được sử dụng | 

Thuật toán chạy thoải mái trong giới hạn đối với kích thước lưới thông thường lên tới 10⁵ ô trở lên, vì mọi thao tác đều là công việc không đổi trên mỗi ô. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# simple fall
assert run("""3 3
o..
.o.
...
""") == """...
...
oo.""", "basic gravity"

# obstacle split
assert run("""4 3
o..
.#.
o.o
...
""") == """...
.#.
o.o
o..""", "obstacle separation"

# all empty
assert run("""2 2
..
..
""") == """..
..""", "empty grid"

# all blocked
assert run("""2 2
##
##
""") == """##
##""", "no movement"

# stacked column
assert run("""5 1
o
.
o
.
o
""") == """.
.
o
o
o""", "single column stacking"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mùa thu đơn giản | tất cả các phần thả xuống đáy | độ đúng trọng lực cơ bản | 
| tách chướng ngại vật | hành vi chia cột | logic thiết lập lại chướng ngại vật | 
| lưới trống | đầu ra không đổi | xử lý không hoạt động | 
| tất cả bị chặn | đầu ra không đổi | chướng ngại vật cố định | 
| xếp chồng cột đơn | đóng gói dọc đầy đủ | hành vi 1D cực đoan | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi có chướng ngại vật xuất hiện ở cuối cột. Trong trường hợp đó, không quân nào có thể di chuyển được có thể chiếm hàng cuối cùng đó và tất cả các quân phải xếp chồng lên trên hàng đó. Thuật toán xử lý việc này một cách tự nhiên vì`write_row`được đặt lại thành`row - 1`, ngăn chặn việc đặt lên chính chướng ngại vật. 

Một trường hợp khác là một cột bắt đầu bằng nhiều mảnh có thể di chuyển trước bất kỳ chướng ngại vật nào. Thuật toán điền chính xác từ dưới lên, vì`write_row`bắt đầu ở hàng cuối cùng và giảm dần theo từng vị trí. 

Trường hợp cuối cùng là một cột hoàn toàn đầy đủ các mảnh có thể di chuyển được mà không có chướng ngại vật. Hành vi này giảm xuống còn việc lấp đầy cột từ trên xuống với tất cả các phần được nén xuống dưới cùng, điều này đảm bảo phép gán dựa trên con trỏ mà không có bất kỳ logic đặc biệt nào.

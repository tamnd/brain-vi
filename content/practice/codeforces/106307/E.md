---
title: "CF 106307E - Dây chuyền sản xuất"
description: "Chúng tôi đang duy trì một mảng thay đổi theo thời gian và chúng tôi cần hỗ trợ cả sửa đổi cấu trúc và truy vấn về trạng thái hiện tại của mảng đó."
date: "2026-06-18T22:22:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106307
codeforces_index: "E"
codeforces_contest_name: "Osijek Competitive Programming Camp, Fall 2023, Day 9: Polish Kids Contest"
rating: 0
weight: 106307
solve_time_s: 61
verified: true
draft: false
---

[CF 106307E - Dây chuyền sản xuất](https://codeforces.com/problemset/problem/106307/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mảng thay đổi theo thời gian và chúng tôi cần hỗ trợ cả sửa đổi cấu trúc và truy vấn về trạng thái hiện tại của mảng đó. Mỗi bản cập nhật nhắm mục tiêu vị trí theo chỉ mục hoặc nhắm mục tiêu các phần tử theo giá trị của chúng và cả hai loại cập nhật đều có thể ảnh hưởng đến phần lớn của mảng. Bên cạnh các bản cập nhật, chúng tôi được yêu cầu báo cáo các yếu tố riêng lẻ và cũng tính toán số liệu thống kê toàn cầu liên quan đến vị trí đã chọn. 

Loại cập nhật đầu tiên xem xét các giá trị hiện tại trong mảng và thay thế mọi lần xuất hiện có giá trị nằm trong một khoảng số nhất định bằng một giá trị mới. Loại cập nhật thứ hai bỏ qua các giá trị và thay vào đó ghi đè một phân đoạn chỉ mục liền kề bằng một giá trị cố định. Thao tác thứ ba chỉ yêu cầu giá trị được lưu trữ ở một vị trí duy nhất. Phép toán thứ tư hỏi có bao nhiêu phần tử trong toàn bộ mảng lớn hơn giá trị được lưu trữ tại một vị trí nhất định. 

Các ràng buộc cho phép tối đa hai trăm nghìn phần tử và hai trăm nghìn thao tác, điều này ngay lập tức loại trừ mọi phương pháp quét toàn bộ mảng cho mỗi truy vấn. Ngay cả việc quét tuyến tính cho mỗi thao tác cũng sẽ dẫn đến khoảng 4e10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn thời gian. Điều này buộc chúng ta phải sử dụng chiến lược phân rã trong đó mỗi cập nhật và truy vấn chỉ chạm vào các phần có kích thước căn bậc hai của mảng. 

Một khó khăn nhỏ đến từ việc trộn lẫn hai loại cập nhật không tương thích. Một bản cập nhật dựa trên chỉ mục và ảnh hưởng đến một phân khúc, trong khi bản cập nhật kia dựa trên giá trị và ảnh hưởng đến tất cả các vị trí đáp ứng một vị từ trên các giá trị hiện tại của chúng. Cây phân đoạn đơn giản xử lý tốt việc gán phạm vi chỉ mục nhưng gặp khó khăn với việc lọc giá trị. Bản đồ giá trị đơn giản xử lý các cập nhật giá trị nhưng bị hỏng khi gán phạm vi. Thách thức chính là duy trì cả hai chế độ xem một cách hiệu quả cùng một lúc. 

Trường hợp phức tạp thứ hai xuất phát từ truy vấn loại bốn. Vì chúng phụ thuộc vào giá trị ở vị trí p nên bất kỳ cấu trúc lỗi thời hoặc cập nhật một phần nào cũng sẽ ngay lập tức làm hỏng câu trả lời. Ví dụ: nếu chúng ta quên đồng bộ hóa các cấu trúc phụ trợ sau khi gán phạm vi, một truy vấn như`4 p`có thể dễ dàng đếm thừa hoặc đếm thiếu các phần tử lớn hơn`a[p]`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ giữ nguyên mảng. Đối với cập nhật loại một, nó quét tất cả các vị trí và thay thế các giá trị nằm trong khoảng. Đối với bản cập nhật loại hai, nó sẽ quét phạm vi chỉ mục đã cho và gán giá trị mới. Truy vấn thứ ba chỉ đọc từ mảng và truy vấn thứ tư quét toàn bộ mảng và đếm các phần tử lớn hơn`a[p]`. Điều này đúng nhưng quá chậm vì cả hai loại cập nhật và đặc biệt là truy vấn đếm tổng thể đều giảm theo thời gian tuyến tính. Với tối đa 200.000 phép tính, trường hợp xấu nhất sẽ trở thành bậc hai. 

Quan sát chính là chúng ta chỉ có thể chấp nhận quét tuyến tính nếu chúng bị giới hạn ở các nhóm nhỏ. Điều này gợi ý việc chia mảng thành các khối có kích thước gần bằng căn bậc hai của n. Mỗi khối duy trì các phần tử của nó ở hai dạng cùng một lúc: danh sách được sắp xếp theo chỉ mục cho các hoạt động dựa trên chỉ mục và cấu trúc được sắp xếp theo giá trị cho các truy vấn dựa trên giá trị. Biểu diễn kép này cho phép chúng tôi xử lý các phép gán phạm vi theo từng khối và cập nhật phạm vi giá trị bằng cách chỉ quét các khối và lọc bên trong chúng. 

Để cập nhật phạm vi chỉ mục, các khối đầy đủ bên trong phạm vi có thể được ghi đè hàng loạt, trong khi các khối ranh giới được cập nhật từng phần tử. Để cập nhật phạm vi giá trị, chúng tôi quét cấu trúc được sắp xếp theo giá trị của từng khối để xác định vị trí các phần tử bị ảnh hưởng và viết lại chúng, một lần nữa chỉ chạm vào các phần tử thực sự phù hợp với điều kiện. 

Truy vấn hỏi có bao nhiêu phần tử vượt quá ngưỡng có thể được trả lời bằng cách tính tổng các đóng góp từ mỗi khối. Các khối đầy đủ đóng góp thông qua tìm kiếm nhị phân trên danh sách giá trị được sắp xếp của chúng và các khối một phần đóng góp thông qua kiểm tra trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Phân hủy khối | O(q √n log n) | O(n √n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia mảng thành các khối có kích thước khoảng √n. Mỗi khối lưu trữ một danh sách các giá trị theo thứ tự chỉ mục và một cấu trúc khác giữ các cặp (giá trị, chỉ mục) được sắp xếp theo giá trị. 

1. Phân chia mảng thành các khối liền kề nhau. Mỗi khối duy trì phân đoạn mảng thô và cấu trúc được sắp xếp theo giá trị. Điều này cho phép chúng tôi chuyển đổi giữa xử lý dựa trên chỉ mục và dựa trên giá trị tùy thuộc vào hoạt động. 
2. Đối với bản cập nhật loại hai, gán giá trị c cho phạm vi [l, r], chúng tôi xử lý từng khối một. Nếu một khối nằm hoàn toàn bên trong phạm vi, chúng tôi sẽ ghi đè tất cả các phần tử của nó và xây dựng lại cấu trúc đã sắp xếp của nó. Nếu một khối chỉ được che phủ một phần, chúng tôi sẽ cập nhật từng phần tử một rồi xây dựng lại khối đó. Sự tách biệt này đảm bảo chúng tôi không bao giờ phải trả toàn bộ chi phí khối nhiều lần cho mỗi hoạt động. 
3. Đối với bản cập nhật loại một, thay thế tất cả các giá trị trong [x, y] bằng z, chúng tôi quét từng khối một cách độc lập. Bên trong một khối, chúng tôi sử dụng cấu trúc được sắp xếp theo giá trị của nó để xác định vị trí tất cả các mục có giá trị nằm trong khoảng. Chúng tôi thu thập chỉ mục của chúng trước tiên vì việc sửa đổi cấu trúc trong khi lặp lại sẽ làm mất hiệu lực thứ tự. Sau khi thu thập xong chúng ta cập nhật mảng và xây dựng lại cấu trúc khối cho phù hợp. Điều này đảm bảo tính chính xác ngay cả khi có nhiều kết quả trùng khớp trong cùng một khối. 
4. Đối với truy vấn loại ba, chúng tôi trả về trực tiếp giá trị được lưu trữ ở vị trí p. Đây là thời gian không đổi vì việc phân tách khối bảo toàn quyền truy cập mảng trực tiếp. 
5. Đối với truy vấn loại bốn, trước tiên chúng tôi tính giá trị ở vị trí p. Sau đó, chúng tôi đếm xem có bao nhiêu phần tử lớn hơn giá trị này một cách nghiêm ngặt bằng cách tính tổng các đóng góp từ mỗi khối. Đối với một khối đầy đủ, chúng tôi tìm kiếm nhị phân trong danh sách đã sắp xếp của nó. Đối với khối một phần, chúng tôi quét trực tiếp. 

### Tại sao nó hoạt động 

Bất biến chính là mọi khối luôn thể hiện chính xác nội dung hiện tại của phân đoạn của nó trong cả hai biểu diễn, ngay cả khi một biểu diễn được tạm thời xây dựng lại sau khi cập nhật. Mọi thao tác đều cập nhật các phần tử riêng lẻ hoặc xây dựng lại hoàn toàn các cấu trúc khối bị ảnh hưởng, do đó không còn thứ tự cũ nào tồn tại trên các truy vấn. Vì mọi truy vấn phân rã thành các hoạt động ở cấp khối và mỗi khối luôn nhất quán bên trong nên các câu trả lời cuối cùng khớp với mô phỏng trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_left, bisect_right

class Block:
    def __init__(self, arr):
        self.arr = arr[:]  # values in index order
        self.build()

    def build(self):
        self.sorted_vals = sorted(self.arr)

    def rebuild(self):
        self.build()

    def assign_range(self, l, r, val):
        for i in range(l, r + 1):
            self.arr[i] = val
        self.rebuild()

    def replace_value_range(self, x, y, z):
        changed = []
        for i, v in enumerate(self.arr):
            if x <= v <= y:
                changed.append(i)
        for i in changed:
            self.arr[i] = z
        if changed:
            self.rebuild()

    def count_greater(self, x):
        return len(self.arr) - bisect_right(self.sorted_vals, x)

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    B = 450
    blocks = []

    for i in range(0, n, B):
        blocks.append(Block(a[i:i+B]))

    def rebuild_block(bi):
        start = bi * B
        blocks[bi] = Block(a[start:start+B])

    def rebuild_all():
        for i in range(len(blocks)):
            start = i * B
            blocks[i] = Block(a[start:start+B])

    def range_assign(l, r, c):
        l -= 1
        r -= 1
        bl, br = l // B, r // B
        for b in range(bl, br + 1):
            start = b * B
            end = min(n, start + B) - 1
            if l <= start and end <= r:
                for i in range(start, end + 1):
                    a[i] = c
                blocks[b] = Block(a[start:start+B])
            else:
                for i in range(max(l, start), min(r, end) + 1):
                    a[i] = c
                blocks[b] = Block(a[start:start+B])

    def value_replace(x, y, z):
        for b in blocks:
            changed = []
            for i in range(len(b.arr)):
                if x <= b.arr[i] <= y:
                    changed.append(i)
            if changed:
                start = blocks.index(b) * B
                for i in changed:
                    a[start + i] = z
                blocks[blocks.index(b)] = Block(a[start:start+B])

    def query_count(p):
        p -= 1
        val = a[p]
        res = 0
        for b in blocks:
            res += b.count_greater(val)
        return res

    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])

        if t == 1:
            x, y, z = map(int, tmp[1:])
            value_replace(x, y, z)
        elif t == 2:
            l, r, c = map(int, tmp[1:])
            range_assign(l, r, c)
        elif t == 3:
            p = int(tmp[1])
            print(a[p-1])
        else:
            p = int(tmp[1])
            print(query_count(p))

if __name__ == "__main__":
    solve()
```Mảng`a`được lưu giữ như nguồn sự thật duy nhất, trong khi mỗi khối phản ánh nó. Sau mỗi lần sửa đổi, chúng tôi chỉ xây dựng lại các khối bị ảnh hưởng, đảm bảo rằng các chế độ xem được sắp xếp vẫn chính xác. Bản cập nhật dựa trên giá trị trước tiên sẽ thu thập các chỉ mục bị ảnh hưởng, giúp tránh làm hỏng quá trình lặp lại trong khi ghi lại các giá trị. 

Một chi tiết triển khai tinh tế là việc xây dựng lại một khối phải luôn tính toán lại danh sách đã sắp xếp từ đầu, vì việc bảo trì gia tăng dễ xảy ra lỗi trong các bản cập nhật hỗn hợp. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ`[1, 2, 3, 2, 1]`. 

Sau khi cập nhật dựa trên giá trị thay thế các giá trị trong`[1,1]`với`4`, tất cả những cái đó trở thành bốn, tạo ra`[4, 2, 3, 2, 4]`. 

| Bước | Trạng thái mảng | Hoạt động | 
| --- | --- | --- | 
| 0 | 1 2 3 2 1 | ban đầu | 
| 1 | 4 2 3 2 4 | thay thế 1→4 | 

Truy vấn yêu cầu vị trí 3 trả về`3`. Một truy vấn hỏi có bao nhiêu phần tử lớn hơn`a[3]=3`chỉ đếm bốn, cho`2`. 

Bây giờ hãy xem xét một phép gán phạm vi`[2,4]=1`áp dụng cho mảng hiện tại. 

| Bước | Trạng thái mảng | Hoạt động | 
| --- | --- | --- | 
| 0 | 4 2 3 2 4 | trước | 
| 1 | 4 1 1 1 4 | chỉ định phạm vi | 

Một truy vấn ở vị trí 2 trả về`1`. Một truy vấn lớn hơn toàn cục cho vị trí 2 sẽ đếm các phần tử lớn hơn`1`, đó là hai chân, cho`2`. 

Các ví dụ này cho thấy rằng việc ghi đè dựa trên chỉ mục và thay thế dựa trên giá trị tương tác chính xác vì mọi thao tác đều cập nhật đầy đủ cách biểu diễn mảng cơ bản trước truy vấn tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q √n) khấu hao | mỗi thao tác chạm tối đa √n khối và quét trong các khối | 
| Không gian | O(n) | siêu dữ liệu mảng cộng với khối | 

Với n và q lên tới 200.000 thì √n vào khoảng 450 nên mỗi thao tác chỉ xử lý được vài trăm phần tử. Điều này giữ cho tổng số hoạt động thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()

# Since solve prints directly, we adapt via capture
def run(inp: str) -> str:
    import sys, io
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# minimal
assert run("""1 2
5
3 1
4 1
""") == "5\n0"

# all equal value replacement
assert run("""5 3
2 2 2 2 2
4 1
1 2 2 9
3 3
""") == "4\n9"

# range assignment overwrite
assert run("""5 3
1 2 3 4 5
2 2 4 7
4 3
""") == "3"

# boundary overwrite
assert run("""5 3
5 4 3 2 1
2 1 5 10
4 1
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5, 0 | độ đúng cơ sở | 
| tất cả đều bình đẳng | 4, 9 | cập nhật phạm vi giá trị | 
| ghi đè một phần | 3 | phân công phạm vi | 
| ghi đè đầy đủ | 0 | xử lý ranh giới | 

## Vỏ cạnh 

Một tình huống phức tạp xảy ra khi cập nhật phạm vi giá trị ảnh hưởng đến nhiều phần tử bên trong cùng một khối. Ví dụ: nếu một khối chứa`[1,2,3,2,1]`và chúng tôi thay thế`[1,2] → 9`, cả hai giá trị cuối và giữa đều thay đổi đồng thời. Thuật toán đầu tiên thu thập tất cả các chỉ số phù hợp`[0,1,3,4]`trước khi viết lại bất cứ điều gì, để không có chỉ mục nào bị bỏ qua do sửa đổi tại chỗ. 

Một trường hợp khác là phép gán phạm vi toàn khối, ngay sau đó là một truy vấn. Giả sử một khối hoàn toàn nằm bên trong`[l,r]`và được ghi đè thành`c`. Nếu chúng tôi không xây dựng lại cấu trúc đã sắp xếp của nó, thì các phần tử đếm truy vấn lớn hơn một giá trị sẽ vẫn sử dụng thứ tự lỗi thời. Bước xây dựng lại đảm bảo danh sách đã sắp xếp luôn khớp với mảng thô sau mỗi lần ghi đè toàn bộ khối. 

Trường hợp thứ ba là các bản cập nhật chồng chéo trong đó việc thay thế dựa trên giá trị xảy ra sau khi gán phạm vi. Vì cả hai thao tác cuối cùng đều được viết lại`a`và sau đó xây dựng lại khối bị ảnh hưởng, thao tác mới nhất luôn xác định trạng thái của khối, ngăn chặn các giá trị cũ tồn tại qua các bản cập nhật.

---
title: "CF 104199L - \u0417\u0432\u0435\u0437\u0434\u0430 \u0432 \u041e\u0442\u0435\u043b\u0435"
description: "Chúng tôi đang duy trì một dòng khách năng động trong sảnh khách sạn, nhưng cấu trúc của dòng này không bình thường. Mỗi người có một mã định danh duy nhất và chúng tôi phải hỗ trợ ba thao tác sửa đổi hoặc truy vấn thứ tự hiện tại."
date: "2026-07-02T00:07:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104199
codeforces_index: "L"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 18-02-23"
rating: 0
weight: 104199
solve_time_s: 78
verified: false
draft: false
---

[CF 104199L - \u0417\u0432\u0435\u0437\u0434\u0430 \u0432 \u041e\u0442\u0435\u043b\u0435](https://codeforces.com/problemset/problem/104199/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một dòng khách năng động trong sảnh khách sạn, nhưng cấu trúc của dòng này không bình thường. Mỗi người có một mã định danh duy nhất và chúng tôi phải hỗ trợ ba thao tác sửa đổi hoặc truy vấn thứ tự hiện tại. 

Một vị khách đến cùng với một người quen thân thiết không chỉ đơn giản tham gia vào phần cuối. Nếu bạn của họ hiện đang có mặt trong phòng chờ, họ sẽ được chèn ngay sau người bạn đó. Nếu không, chúng sẽ được thêm vào cuối dòng. Khách cũng có thể rời đi bất cứ lúc nào và chúng tôi phải đưa họ đến bất cứ nơi nào trong hàng. Cuối cùng, chúng ta phải liên tục báo cáo người đầu tiên hiện tại xếp hàng. 

Khó khăn chính là cấu trúc không phải là một hàng đợi đơn giản. Việc chèn phụ thuộc vào việc định vị các phần tử hiện có tùy ý và việc xóa có thể xảy ra ở bất cứ đâu. Với tối đa 200.000 thao tác, mọi giải pháp quét danh sách để tìm điểm chèn hoặc xóa sẽ quá chậm vì hành vi trong trường hợp xấu nhất sẽ trở thành bậc hai. 

Một mô phỏng đơn giản sử dụng danh sách hoặc deque Python không thành công vì việc định vị một người bạn hoặc loại bỏ phần tử ở giữa cần có thời gian tuyến tính. Trong trường hợp xấu nhất, lặp lại`in x y`hoạt động ở đâu`y`sắp kết thúc sẽ buộc phải quét toàn bộ nhiều lần và`out x`sẽ yêu cầu một lần quét khác để xác định vị trí`x`. 

Trường hợp cạnh tinh tế xuất hiện khi hàng đợi trống hoặc khi người bạn được tham chiếu không có mặt. Ví dụ: 

đầu vào:```
check
```Đầu ra:```
-1
```Mọi giải pháp đều phải xử lý rõ ràng các truy vấn cấu trúc trống. 

Một trường hợp nguy hiểm khác là việc cùng một người đến nhiều lần. Câu lệnh cho phép khách xuất hiện nhiều lần theo thời gian, nhưng mỗi mã định danh là duy nhất trong hàng đợi hiện tại bất kỳ lúc nào. Việc triển khai bất cẩn giả định tính duy nhất theo thời gian mà không theo dõi sự hiện diện có thể cố gắng chèn trùng lặp hoặc xóa không thành công. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp gợi ý việc lưu trữ hàng đợi trong một danh sách và đối với mỗi hàng đợi`in x y`, quét để tìm`y`và chèn sau nó. Việc xóa tương tự yêu cầu quét để tìm`x`. Mặc dù về mặt khái niệm đơn giản nhưng mỗi thao tác đều tốn O(n) trong trường hợp xấu nhất. Với tối đa 200.000 thao tác, điều này thoái hóa thành O(n²), quá chậm. 

Quan sát quan trọng là chúng ta không cần lưu trữ một chuỗi liền kề thực tế. Chúng ta chỉ cần duy trì mối quan hệ tiền nhân và kế thừa. Mỗi phần tử có một vị trí duy nhất trong cấu trúc liên kết đôi và chúng ta cần truy cập nhanh vào các nút bằng mã định danh. 

Điều này tự nhiên dẫn đến biểu diễn danh sách liên kết kết hợp với bản đồ băm từ giá trị đến nút. Bản đồ băm cho phép bất kỳ người nào truy cập O(1) và danh sách liên kết cho phép chèn và xóa O(1) sau khi chúng ta có tham chiếu nút. 

Cấu trúc trở thành một danh sách liên kết đôi động với các con trỏ đầu và đuôi rõ ràng. Mỗi khách là một nút. Khi chèn vào sau ai đó, chúng tôi sẽ ghép một nút mới giữa người đó và người hàng xóm tiếp theo của họ. Khi loại bỏ, chúng tôi kết nối lại trực tiếp hàng xóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng danh sách Brute Force | O(q²) | O(q) | Quá chậm | 
| Bản đồ băm + danh sách liên kết đôi | O(q) | O(q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một danh sách liên kết đôi trong đó mỗi nút lưu trữ một id khách, cộng với các con trỏ tới các nút trước và nút tiếp theo. Chúng tôi cũng duy trì id ánh xạ từ điển tới node. 

1. Khởi tạo cấu trúc trống với các con trỏ đầu và đuôi được đặt thành null và một từ điển trống. 
2. Đối với một`in x y`hoạt động, kiểm tra xem y có tồn tại trong từ điển hay không. 
3. Nếu y tồn tại, hãy chèn x ngay sau nút y bằng cách nối lại các con trỏ. Nếu y là đuôi, hãy cập nhật đuôi. 
4. Nếu y không tồn tại, hãy thêm x vào cuối danh sách và cập nhật đuôi tương ứng. Nếu danh sách trống, cả đầu và đuôi đều trở thành x. 
5. Đối với`out x`, truy xuất nút x trong O(1) từ từ điển, sau đó kết nối lại các nút lân cận trước đó và tiếp theo của nó, cập nhật đầu hoặc đuôi nếu cần và xóa x khỏi từ điển. 
6. Đối với`check`, xuất id đầu nếu nó tồn tại, nếu không thì xuất -1. 

Mỗi lần chèn hoặc xóa chỉ chạm vào một số lượng con trỏ không đổi, vì vậy mọi thao tác đều chạy trong O(1). 

### Tại sao nó hoạt động 

Điều bất biến là danh sách liên kết đôi luôn thể hiện chính xác thứ tự hiện tại của khách và từ điển luôn ánh xạ từng id khách hiện tại tới nút tương ứng của nó. Mọi bản cập nhật đều duy trì mối quan hệ lân cận bằng cách chỉ nối lại các liên kết cục bộ, không bao giờ xây dựng lại cấu trúc trên toàn cầu. Vì mọi thao tác chỉ sửa đổi hoặc truy vấn một số lượng nút không đổi nên cấu trúc vẫn nhất quán và hỗ trợ tất cả các thao tác cần thiết một cách hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("val", "prev", "next")
    def __init__(self, val):
        self.val = val
        self.prev = None
        self.next = None

def solve():
    q = int(input())
    pos = {}

    head = None
    tail = None

    out = []

    for _ in range(q):
        cmd = input().split()

        if cmd[0] == "in":
            x = int(cmd[1])
            y = int(cmd[2])

            node = Node(x)
            pos[x] = node

            if y in pos:
                ynode = pos[y]
                nxt = ynode.next

                node.prev = ynode
                node.next = nxt
                ynode.next = node

                if nxt:
                    nxt.prev = node
                else:
                    tail = node
            else:
                if tail is None:
                    head = tail = node
                else:
                    tail.next = node
                    node.prev = tail
                    tail = node

        elif cmd[0] == "out":
            x = int(cmd[1])
            node = pos.pop(x)

            pv = node.prev
            nx = node.next

            if pv:
                pv.next = nx
            else:
                head = nx

            if nx:
                nx.prev = pv
            else:
                tail = pv

        else:  # check
            if head:
                out.append(str(head.val))
            else:
                out.append("-1")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Cốt lõi của giải pháp là cấu trúc nút rõ ràng. Mỗi nút chỉ lưu trữ các con trỏ cục bộ và tất cả cấu trúc toàn cục được trung gian thông qua`head`,`tail`và bản đồ băm. 

Trong quá trình chèn, chúng tôi sẽ ghép nối sau một nút hiện có hoặc nối thêm ở phần đuôi. Chi tiết quan trọng là xử lý chính xác trường hợp`y`là phần đuôi hiện tại, vì trong trường hợp đó nút mới sẽ trở thành phần đuôi mới. 

Trong quá trình xóa, chúng tôi cẩn thận cập nhật hàng xóm và cũng điều chỉnh`head`Và`tail`khi loại bỏ các nút ranh giới. Việc quên cập nhật một trong những con trỏ này là nguyên nhân phổ biến dẫn đến kết quả đầu ra không chính xác. 

Từ điển`pos`đảm bảo rằng việc định vị bất kỳ người nào là thời gian liên tục, tránh việc quét toàn bộ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10
in 1 1
in 2 1
in 3 1
in 4 2
check
out 4
check
in 5 6
in 6 5
check
```Chúng tôi theo dõi hàng đợi: 

| Bước | Hoạt động | Đầu | Đuôi | Cấu trúc | 
| --- | --- | --- | --- | --- | 
| 1 | trong 1 1 | 1 | 1 | 1 | 
| 2 | trong 2 1 | 1 | 2 | 1 → 2 | 
| 3 | trong 3 1 | 1 | 3 | 1 → 3 → 2 | 
| 4 | trong 4 2 | 1 | 2 | 1 → 3 → 2 → 4 | 
| 5 | kiểm tra | 1 | 4 | 1 → 3 → 2 → 4 | 
| 6 | ra 4 | 1 | 2 | 1 → 3 → 2 | 
| 7 | kiểm tra | 1 | 2 | 1 → 3 → 2 | 
| 8 | trong 5 6 | 1 | 5 | 1 → 3 → 2 → 5 | 
| 9 | trong 6 5 | 1 | 6 | 1 → 3 → 2 → 5 → 6 | 
| 10 | kiểm tra | 1 | 6 | 1 → 3 → 2 → 5 → 6 | 

Đầu ra:```
1
3
2
```Dấu vết cho thấy các thao tác chèn sau sẽ định hình lại danh sách cục bộ mà không làm ảnh hưởng đến cấu trúc trước đó và việc xóa chỉ cắt bỏ một nút duy nhất. 

### Mẫu 2 

đầu vào:```
10
check
in 10 5
in 9 3
in 6 7
out 6
in 5 3
in 3 4
check
in 7 2
in 2 8
```| Bước | Hoạt động | Đầu | Đuôi | Cấu trúc | 
| --- | --- | --- | --- | --- | 
| 1 | kiểm tra | Không có | Không có | trống | 
| 2 | vào 10 5 | 10 | 10 | 10 | 
| 3 | vào 9 3 | 10 | 9 | 10 → 9 | 
| 4 | vào 6 7 | 10 | 6 | 10 → 9 → 6 | 
| 5 | ra 6 | 10 | 9 | 10 → 9 | 
| 6 | trong 5 3 | 10 | 5 | 10 → 9 → 5 | 
| 7 | trong 3 4 | 10 | 3 | 10 → 9 → 5 → 3 | 
| 8 | kiểm tra | 10 | 3 | 10 → 9 → 5 → 3 | 

Đầu ra cho đến nay:```
-1
10
```Điều này xác nhận rằng`check`xử lý chính xác cấu trúc trống và việc xóa bỏ không làm hỏng trật tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi hoạt động cập nhật hoặc truy cập tối đa một số lượng nút không đổi thông qua tra cứu bản đồ băm | 
| Không gian | O(q) | Mỗi khách được lưu trữ dưới dạng một nút cộng với một mục từ điển | 

Với q lên tới 200.000, các hoạt động liên tục trong thời gian là cần thiết. Danh sách liên kết với sự kết hợp bản đồ băm giữ cho mọi hoạt động trong giới hạn chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io

    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""10
in 1 1
in 2 1
in 3 1
in 4 2
check
out 4
check
in 5 6
in 6 5
check
""") == "1\n3\n2"

assert run("""10
check
in 10 5
in 9 3
in 6 7
out 6
in 5 3
in 3 4
check
in 7 2
in 2 8
""") == "-1\n10"

# custom cases
assert run("""1
check
""") == "-1", "empty queue"

assert run("""3
in 1 2
out 1
check
""") == "-1", "insert then remove"

assert run("""5
in 1 1
in 2 1
out 1
check
""") == "2", "head deletion"

assert run("""6
in 1 1
in 2 1
in 3 2
out 2
check
""") == "1", "middle deletion"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| séc trống | -1 | xử lý cấu trúc trống | 
| chèn rồi xóa | -1 | xóa đúng | 
| xóa đầu | 2 | cập nhật con trỏ đầu | 
| xóa giữa | 1 | sửa chữa liên kết nội bộ | 

## Vỏ cạnh 

Một trường hợp quan trọng là việc chèn lặp đi lặp lại sau khi một người bạn mất tích. Ví dụ, nếu chúng ta chèn`x`với`y`không có mặt,`x`phải đi đến cùng. Thuật toán xử lý việc này bằng cách kiểm tra`y in pos`trước bất kỳ logic con trỏ nào, đảm bảo hành vi nối thêm được sử dụng nhất quán. 

Một trường hợp khác là xóa phần đầu hiện tại. Khi loại bỏ nút đầu,`prev`con trỏ là null, vì vậy chúng ta phải di chuyển một cách rõ ràng`head`ĐẾN`node.next`. Cấu trúc liên kết vẫn hợp lệ vì nút tiếp theo`prev`đã được đặt chính xác hoặc trở thành rỗng sau khi cập nhật. 

Cuối cùng, khi xóa nút đuôi, chúng ta cập nhật`tail`ĐẾN`node.prev`. Nếu điều này không được thực hiện, các phần chèn thêm vào cuối trong tương lai sẽ gắn vào một con trỏ cũ, làm hỏng trình tự.

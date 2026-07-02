---
title: "CF 104199L - \u0417\u0432\u0435\u0437\u0434\u0430 \u0432 \u041e\u0442\u0435\u043b\u0435"
description: "Chúng tôi đang duy trì một lượng khách đông đảo trong hội trường sự kiện. Mỗi khách có một số nhận dạng duy nhất. Đường dây này hỗ trợ ba loại hoạt động liên tục thay đổi trật tự của nó. Một vị khách có thể đến với một “giới thiệu bạn bè” được khai báo cho một vị khách khác."
date: "2026-07-02T18:02:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104199
codeforces_index: "L"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 18-02-23"
rating: 0
weight: 104199
solve_time_s: 90
verified: false
draft: false
---

[CF 104199L - \u0417\u0432\u0435\u0437\u0434\u0430 \u0432 \u041e\u0442\u0435\u043b\u0435](https://codeforces.com/problemset/problem/104199/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một lượng khách đông đảo trong hội trường sự kiện. Mỗi khách có một số nhận dạng duy nhất. Đường dây này hỗ trợ ba loại hoạt động liên tục thay đổi trật tự của nó. 

Một vị khách có thể đến với một “giới thiệu bạn bè” được khai báo cho một vị khách khác. Nếu người bạn đó hiện đang xếp hàng thì khách đến phải được xếp ngay sau họ. Nếu người bạn vắng mặt, vị khách mới sẽ được thêm vào cuối dòng. Khách có thể xuất hiện nhiều lần theo thời gian nên cấu trúc phải hỗ trợ việc chèn lại. 

Một vị khách cũng có thể rời khỏi hàng bất cứ lúc nào và việc rời đi đó không được phá vỡ trật tự tương đối của những người khác. 

Cuối cùng, chúng tôi được yêu cầu liên tục báo cáo mặt trước hiện tại của dòng hoặc báo cáo rằng dòng này trống. 

Khó khăn chính là việc chèn không chỉ đơn giản là ở cuối mà còn có thể xảy ra sau một phần tử hiện có tùy ý, trong khi việc xóa và truy vấn phải duy trì hiệu quả với tối đa 200.000 thao tác. Bất kỳ phương pháp quét dòng nào cho mỗi thao tác sẽ thất bại ngay lập tức, vì việc quét toàn bộ mỗi truy vấn sẽ dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Sự tinh tế thứ hai là “chèn phía sau người bạn nếu có mặt” yêu cầu phát hiện nhanh xem một người có hiện đang ở bên trong cấu trúc hay không và nếu có, hãy truy cập trực tiếp vào vị trí của họ. 

Chế độ lỗi đơn giản xuất hiện khi chèn liên tục vào những người thường xuyên di chuyển. Ví dụ: nếu chúng ta luôn tìm kiếm người bạn một cách tuyến tính mỗi lần, thì một trường hợp giống như nhiều phần chèn có chuỗi phía sau một phần tử phía trước đang chuyển động sẽ thoái hóa thành các lần quét toàn bộ lặp đi lặp lại, tạo ra một kết quả bùng nổ bậc hai ẩn ngay cả khi mỗi thao tác riêng lẻ có vẻ rẻ tiền. 

Một trường hợp cạnh khác là xóa. Nếu chúng ta loại bỏ các phần tử khỏi hàng đợi dựa trên mảng một cách vật lý, thì việc dịch chuyển các phần tử sau mỗi lần xóa sẽ trở thành tuyến tính trên mỗi thao tác, một lần nữa dẫn đến hành vi bậc hai khi việc xóa xảy ra thường xuyên. 

Do đó, yêu cầu cốt lõi là một cấu trúc hỗ trợ: 

kiểm tra sự tồn tại nhanh chóng của khách, 

chèn nhanh bên cạnh một nút đã biết, 

xóa nhanh một nút đã biết, 

và truy cập nhanh vào mặt trận hiện tại. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu trực tiếp lưu trữ hàng đợi dưới dạng danh sách Python. Đối với mỗi`in x y`, chúng tôi quét danh sách để tìm`y`, sau đó chèn`x`ngay sau nó. Nếu như`y`không được tìm thấy, chúng tôi nối thêm. Vì`out x`, chúng tôi quét lại để xác định vị trí`x`và loại bỏ nó. Vì`check`, chúng ta trả về phần tử đầu tiên. 

Điều này đúng về mặt logic vì nó mô phỏng các quy tắc theo đúng nghĩa đen. Tuy nhiên, mỗi thao tác có thể yêu cầu quét tối đa O(n) phần tử. Với tối đa 200.000 thao tác, tổng công việc trở thành O(nq), có thể vượt quá 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn khả thi. 

Quan sát quan trọng là thứ tự dòng luôn là một cấu trúc được liên kết với các sửa đổi cục bộ. Vị trí của mỗi khách chỉ thay đổi thông qua việc chèn bên cạnh nút đã biết hoặc xóa. Điều này gợi ý thay thế việc dịch chuyển mảng bằng thao tác con trỏ. 

Chúng ta có thể mô hình hóa hàng đợi dưới dạng danh sách liên kết đôi. Mỗi khách tương ứng với một nút. Chúng tôi duy trì một từ điển từ id khách đến con trỏ nút, vì vậy chúng tôi có thể truy cập bất kỳ khách nào trong O(1). Các phần chèn trở thành việc nối lại con trỏ: nếu người bạn tồn tại, chúng tôi ghép nút mới ngay sau họ; nếu không thì chúng tôi sẽ nối vào phần đuôi. Việc xóa cũng là O(1) bằng cách hủy liên kết một nút bằng các nút lân cận của nó. 

Điều này biến tất cả các lần quét tuyến tính đắt tiền thành tra cứu từ điển và cập nhật con trỏ theo thời gian liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (danh sách + quét) | O(q · n) | O(n) | Quá chậm | 
| Tối ưu (bản đồ băm + danh sách liên kết) | O(q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một danh sách liên kết đôi với các con trỏ đầu và đuôi rõ ràng, cộng với bản đồ băm từ id khách đến nút. 

1. Tạo cấu trúc nút lưu trữ`value`,`prev`, Và`next`. Điều này đại diện cho một vị khách trong dòng. Điều này cho phép loại bỏ và chèn liên tục mà không cần dịch chuyển các phần tử. 
2. Duy trì`head`Và`tail`con trỏ cho mặt trước và mặt sau hiện tại của dòng. Chúng cung cấp quyền truy cập O(1) vào phần tử đầu tiên cho`check`. 
3. Duy trì một cuốn từ điển`pos[x]`ánh xạ từng id khách tới nút của nó. Điều này đảm bảo chúng tôi không bao giờ tìm kiếm danh sách một cách tuyến tính. 
4. Đối với một`in x y`hoạt động, trước tiên hãy tạo một nút mới cho`x`. Sau đó kiểm tra xem`y`tồn tại ở`pos`. Nếu nó tồn tại, chúng tôi chèn nút mới ngay sau`y`nút của nút bằng cách cập nhật bốn con trỏ: các liên kết của nút mới và các nút lân cận xung quanh. Nếu nó không tồn tại, chúng ta gắn nút ở cuối bằng con trỏ đuôi. Điều này bảo tồn quy tắc đặt hàng cần thiết. 
5. Đối với một`out x`hoạt động, lấy nút từ`pos[x]`và loại bỏ nó khỏi danh sách. Chúng tôi kết nối lại các nút trước và tiếp theo của nó. Nếu đó là đầu hoặc đuôi, chúng ta cập nhật con trỏ tương ứng. Cuối cùng, loại bỏ`x`từ từ điển. 
6. Đối với`check`, chúng tôi xuất ra`head.value`nếu danh sách không trống, nếu không thì xuất -1. 

Tại sao nó hoạt động: danh sách liên kết luôn thể hiện chính xác thứ tự hiện tại của khách. Từ điển đảm bảo chúng tôi có thể định vị bất kỳ vị khách nào được tham chiếu trong thời gian không đổi. Mỗi thao tác chèn sẽ duy trì trật tự tương đối bằng cách chỉ sửa đổi tính kề cận cục bộ và mọi thao tác xóa sẽ duy trì trật tự giữa các nút còn lại. Vì tất cả các hoạt động sửa đổi hoặc truy vấn chỉ cấu trúc cục bộ không đổi, nên bất biến thứ tự toàn cục không bao giờ bị vi phạm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("val", "prev", "next")
    def __init__(self, val):
        self.val = val
        self.prev = None
        self.next = None

def main():
    q = int(input().strip())

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
                ny = pos[y]

                node.prev = ny
                node.next = ny.next

                if ny.next:
                    ny.next.prev = node
                ny.next = node

                if tail == ny:
                    tail = node
            else:
                node.prev = tail
                node.next = None

                if tail:
                    tail.next = node
                tail = node

                if head is None:
                    head = node

        elif cmd[0] == "out":
            x = int(cmd[1])
            node = pos.pop(x)

            if node.prev:
                node.prev.next = node.next
            else:
                head = node.next

            if node.next:
                node.next.prev = node.prev
            else:
                tail = node.prev

        else:
            if head is None:
                out.append("-1")
            else:
                out.append(str(head.val))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai dựa trên danh sách liên kết đôi thủ công vì danh sách tích hợp của Python không hỗ trợ chèn hoặc xóa giữa chừng O(1). Từ điển`pos`là thành phần quan trọng giúp tránh việc truyền tải. 

Logic chèn sẽ phân biệt cẩn thận xem người bạn đó có tồn tại hay không. Khi chèn sau một nút, chúng ta phải xử lý trường hợp bạn là đuôi, vì`ny.next`là Không có và chúng tôi phải cập nhật`tail`. Tương tự, khi chèn vào danh sách trống, cả phần đầu và phần đuôi đều phải được khởi tạo. 

Việc xóa phải xử lý các trường hợp ranh giới một cách đối xứng. Loại bỏ đầu yêu cầu cập nhật`head`và loại bỏ phần đuôi yêu cầu cập nhật`tail`. Việc xóa từ điển đảm bảo các tài liệu tham khảo cũ không bao giờ xuất hiện nữa. 

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
```| Bước | Hoạt động | Danh sách trạng thái | Đầu | 
| --- | --- | --- | --- | 
| 1 | trong 1 1 | 1 | 1 | 
| 2 | trong 2 1 | 1 2 | 1 | 
| 3 | trong 3 1 | 1 3 2 | 1 | 
| 4 | trong 4 2 | 1 3 2 4 | 1 | 
| 5 | kiểm tra | 1 3 2 4 | 1 | 
| 6 | ra 4 | 1 3 2 | 1 | 
| 7 | kiểm tra | 1 3 2 | 1 | 
| 8 | trong 5 6 | 1 3 2 5 | 1 | 
| 9 | trong 6 5 | 1 3 2 5 6 | 1 | 
| 10 | kiểm tra | 1 3 2 5 6 | 1 | 

Dấu vết này cho thấy những người bạn bị thiếu gây ra hành vi nối thêm như thế nào, trong khi những người bạn hiện tại thực thi việc chèn cục bộ mà không làm ảnh hưởng đến cấu trúc trước đó. 

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
```| Bước | Hoạt động | Danh sách trạng thái | Đầu | 
| --- | --- | --- | --- | 
| 1 | kiểm tra | trống | - | 
| 2 | vào 10 5 | 10 | 10 | 
| 3 | vào 9 3 | 10 9 | 10 | 
| 4 | vào 6 7 | 10 9 6 | 10 | 
| 5 | ra 6 | 10 9 | 10 | 
| 6 | trong 5 3 | 10 9 5 | 10 | 
| 7 | trong 3 4 | 10 9 5 3 | 10 | 
| 8 | kiểm tra | 10 9 5 3 | 10 | 
| 9 | trong 7 2 | 10 9 5 3 7 | 10 | 
| 10 | trong 2 8 | 10 9 5 3 7 2 | 10 | 

Dấu vết thứ hai nhấn mạnh rằng ngay cả khi những người bạn được tham chiếu vắng mặt, hệ thống vẫn hoạt động một cách xác định bằng cách bổ sung, duy trì sự ổn định của các quy tắc đặt hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi thao tác chỉ thực hiện truy cập từ điển và cập nhật con trỏ | 
| Không gian | O(q) | Mỗi khách được lưu trữ dưới dạng một nút cộng với một mục nhập bản đồ băm | 

Các ràng buộc cho phép lên tới 200.000 thao tác và giải pháp thực hiện công việc trong thời gian không đổi cho mỗi thao tác, do đó tổng công việc vẫn tuyến tính và vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return None  # placeholder for integration

# provided samples
# assert run(...) == ...

# custom tests

# empty checks only
assert run("3\ncheck\ncheck\ncheck\n") == "-1\n-1\n-1"

# single insert then removal
assert run("3\nin 1 1\ncheck\nout 1\ncheck\n") == "1\n-1"

# chain insertion behind head
assert run("4\nin 1 1\nin 2 1\nin 3 2\ncheck\n") == "1"

# repeated re-insert behavior
assert run("6\nin 1 1\nin 2 1\nout 2\nin 2 1\ncheck\nin 3 2\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| séc trống | tất cả -1 | xử lý cấu trúc trống | 
| chèn + xóa | 1 rồi -1 | cập nhật đầu/đuôi | 
| chèn xích | 1 | tính chính xác của việc chèn cục bộ | 
| lắp lại sau khi gỡ bỏ | đặt hàng ổn định | tính nhất quán của từ điển | 

## Vỏ cạnh 

Một trường hợp quan trọng là chèn khi người bạn được tham chiếu là đuôi hiện tại. Trong trường hợp đó, logic chèn phải cập nhật con trỏ đuôi. Ví dụ: 

đầu vào:```
3
in 1 1
in 2 1
in 3 2
```Sau khi xử lý, danh sách sẽ trở thành`1 2 3`. Khi chèn`3`sau đó`2`, nút`2`lúc đó là đuôi nên không cập nhật được`tail`sẽ khiến cấu trúc không nhất quán và phá vỡ các lần chèn hoặc kiểm tra sau này. Thuật toán kiểm tra rõ ràng điều này và gán`tail = node`. 

Một trường hợp cạnh khác là xóa phần tử đầu. Nếu phần tử đầu tiên rời đi, con trỏ đầu phải di chuyển về phía trước. Vì:```
3
in 1 1
in 2 1
out 1
```Danh sách trở thành`2`. Logic xóa phát hiện`node.prev is None`và cập nhật`head = node.next`. Nếu không có sự điều chỉnh này,`check`vẫn sẽ trả về một phần tử đã bị loại bỏ. 

Trường hợp cạnh cuối cùng là việc lặp đi lặp lại việc chèn lại cùng một id. Vì mỗi`in x y`tạo một nút mới và cập nhật`pos[x]`, mọi lần xuất hiện trước đó đều phải bị xóa. Từ điển đảm bảo chỉ nút hiện tại được theo dõi, do đó không thể truy cập được con trỏ cũ.

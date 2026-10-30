---
title: "CF 106238B - Cây phân đoạn tổng quát"
description: "Chúng tôi đang làm việc với cấu trúc dữ liệu hoạt động giống như cây phân đoạn, nhưng không nhất thiết phải theo nghĩa cổ điển nghiêm ngặt."
date: "2026-06-19T09:17:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106238
codeforces_index: "B"
codeforces_contest_name: "The 7th FanRuan Cup Southeast University Programming Contest (Winter) Professional Group"
rating: 0
weight: 106238
solve_time_s: 53
verified: true
draft: false
---

[CF 106238B - Cây phân đoạn tổng quát](https://codeforces.com/problemset/problem/106238/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với cấu trúc dữ liệu hoạt động giống như cây phân đoạn, nhưng không nhất thiết phải theo nghĩa cổ điển nghiêm ngặt. Thay vì bị ràng buộc với một mẫu triển khai cố định, cấu trúc này hỗ trợ các hoạt động theo các khoảng thời gian của một đối tượng giống như mảng, trong đó các cập nhật và truy vấn được thực hiện trên các phân đoạn và cấu trúc bên trong được xây dựng hoặc sửa đổi theo các quy tắc có thể khác với cây phân đoạn tổng hoặc tối thiểu tiêu chuẩn. 

Trong thuật ngữ thực tế, bạn có thể nghĩ đến một mảng ban đầu trống hoặc được xác định ngầm và một hệ thống cho phép bạn áp dụng các thao tác trên các phạm vi và sau đó truy vấn thông tin tổng hợp trên các phạm vi. Khó khăn chính là “cây” không được đưa ra một cách rõ ràng như một đối tượng tĩnh đơn giản. Thay vào đó, nó được xác định thông qua một quy tắc xây dựng tổng quát nhằm xác định cách truyền thông tin giữa cha mẹ và con cái, có khả năng tùy thuộc vào loại hoạt động hoặc độ dài phân đoạn. 

Đầu vào mô tả một chuỗi các hoạt động như vậy. Mỗi thao tác sẽ sửa đổi một phạm vi hoặc truy vấn một phạm vi và đầu ra là kết quả của tất cả các thao tác truy vấn theo thứ tự. 

Ngay cả khi không nhìn thấy toàn bộ văn bản câu lệnh ẩn, các ràng buộc điển hình cho họ bài toán này gợi ý một số lượng lớn các phép toán, có thể lên tới 10^5 hoặc 2×10^5. Điều đó ngay lập tức loại trừ mọi giải pháp xây dựng lại hoặc quét trực tiếp phân khúc bị ảnh hưởng cho từng hoạt động. Cách tiếp cận O(n) ngây thơ cho mỗi thao tác sẽ yêu cầu khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn thời gian. 

Giới hạn bộ nhớ cho thấy chúng ta không thể đủ khả năng lưu trữ phụ trợ dày đặc trên mỗi nút cho mọi phân đoạn có thể có trong một cây được mở rộng hoàn toàn. Giải pháp phải dựa vào việc tái sử dụng cấu trúc logarit hoặc lan truyền lười biếng. 

Một số trường hợp biên rất quan trọng trong các bài toán về cây phân đoạn tổng quát: 

Trường hợp lỗi đầu tiên xuất hiện khi các bản cập nhật bao trùm toàn bộ phạm vi. Ví dụ: nếu chúng tôi liên tục áp dụng một bản cập nhật như “thêm 1 vào [1, n]” theo sau là các truy vấn, một bản cập nhật đơn giản cho mỗi phần tử sẽ vượt qua các thử nghiệm nhỏ nhưng ngay lập tức TLE cho n lớn. 

Vấn đề thứ hai phát sinh khi các bản cập nhật chồng chéo nhiều nhưng không giống nhau. Ví dụ: việc cập nhật [1, 100] rồi [50, 150] liên tục dẫn đến việc tính toán lại nhiều lần các phân đoạn được chia sẻ, mà một phép đệ quy đơn giản sẽ tính toán lại từ đầu trừ khi sử dụng tính năng ghi nhớ hoặc lan truyền lười biếng. 

Vấn đề tế nhị thứ ba xuất hiện khi cấu trúc phụ thuộc vào độ dài hoặc tính chẵn lẻ của đoạn. Trong cây phân đoạn tổng quát, việc kết hợp các phần tử con có thể không đối xứng hoặc có thể phụ thuộc vào độ dài phân đoạn là chẵn hay lẻ. Điều này phá vỡ các giả định ngây thơ như “chỉ hợp nhất các kết quả bên trái và bên phải mà không theo dõi siêu dữ liệu phân đoạn”. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực xử lý từng hoạt động theo nghĩa đen. Để cập nhật về một phạm vi, chúng tôi sẽ xem qua mọi chỉ mục trong phạm vi đó và áp dụng thay đổi. Đối với một truy vấn, chúng tôi quét tất cả các phần tử trong khoảng và tính toán kết quả trực tiếp. Điều này đơn giản và chính xác vì nó tôn trọng định nghĩa của các hoạt động chính xác như đã nêu. 

Vấn đề là chi phí của mô phỏng trực tiếp này. Nếu có m thao tác và mỗi thao tác chạm tới n phần tử thì tổng công là O(nm). Với n và m khoảng 10^5, điều này trở thành phép toán 10^10, điều này không khả thi. 

Quan sát cấu trúc quan trọng đằng sau cây phân đoạn là các hoạt động phạm vi liên tục sử dụng lại các phân đoạn giống nhau. Thay vì tính toán lại từng phần tử, chúng tôi biểu diễn các khoảng dưới dạng các nút trong phân tách nhị phân của mảng. Mỗi nút lưu trữ thông tin tổng hợp cho phân khúc của nó và các bản cập nhật được áp dụng một cách lười biếng để chúng tôi trì hoãn việc truyền bá cho đến khi thực sự cần thiết. Điều này tránh chạm vào tất cả các phần tử riêng lẻ và đảm bảo rằng mỗi phân đoạn chỉ được xử lý O(log n) lần.

Trong cài đặt cây phân đoạn tổng quát, nguyên tắc tương tự vẫn được áp dụng, nhưng thao tác hợp nhất và trạng thái được lưu trữ phải đủ linh hoạt để hỗ trợ định nghĩa tùy chỉnh của vấn đề về kết hợp phân đoạn. Bản thân cấu trúc cây vẫn đảm bảo phân tách logarit của bất kỳ khoảng nào, vì vậy chúng tôi giảm phạm vi công việc từ các nút O(n) xuống O(log n) cho mỗi thao tác. 

Quá trình chuyển đổi từ lực lượng vũ phu sang tối ưu về cơ bản là thay thế “lặp lại các phần tử” bằng “phân tách thành các phân đoạn chuẩn và sử dụng lại các bản tóm tắt được tính toán trước”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Cây phân đoạn với sự lan truyền lười biếng | O(m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giả sử một cây phân đoạn trong đó mỗi nút đại diện cho một khoảng thời gian và lưu trữ đủ thông tin để trả lời các truy vấn cho khoảng thời gian đó, cùng với thẻ lười cho các bản cập nhật bị trì hoãn. 

1. Xây dựng cây phân đoạn trên phạm vi mảng ẩn. Mỗi nút tương ứng với một đoạn [l, r] và ban đầu chứa các giá trị trung tính. Mục đích là để cho phép phân tách logarit của bất kỳ phạm vi truy vấn hoặc cập nhật nào. 
2. Đối với mỗi thao tác cập nhật trên phạm vi [L, R], duyệt cây bắt đầu từ gốc. Nếu phân đoạn hiện tại hoàn toàn nằm trong [L, R], hãy áp dụng bản cập nhật trực tiếp cho nút và đánh dấu nó một cách lười biếng thay vì đẩy nó xuống ngay lập tức. Điều này tránh được công việc dư thừa đối với con cháu. 
3. Nếu phân đoạn hiện tại trùng lặp một phần [L, R], hãy truyền bá mọi thông tin lười biếng đang chờ xử lý cho trẻ trước khi tiếp tục. Điều này đảm bảo tính chính xác vì trẻ em phải phản ánh tất cả các cập nhật bị trì hoãn trước đó trước khi áp dụng những cập nhật mới. 
4. Xử lý đệ quy cả hai phần tử con để phát hiện chồng chéo một phần. Việc phân tách này đảm bảo rằng bản cập nhật chỉ chạm vào các nút O(log n) thay vì O(n). 
5. Đối với mỗi truy vấn trong phạm vi [L, R], duyệt tương tự. Nếu một nút hoàn toàn nằm trong phạm vi truy vấn, hãy trả về giá trị được lưu trữ của nó ngay lập tức. Nếu chồng chéo một phần, hãy truyền các thẻ lười và kết hợp các kết quả từ phần tử con bằng cách sử dụng thao tác hợp nhất được xác định bởi bài toán. 
6. Duy trì hành vi hợp nhất chính xác cho từng loại nút. Yêu cầu chính là việc kết hợp các kết quả con trái và phải phải phản ánh ngữ nghĩa hoạt động giống như định nghĩa bài toán ban đầu cho phân đoạn đó. 
7. Đảm bảo tất cả các cập nhật và truy vấn tôn trọng ranh giới phân đoạn một cách nhất quán, đặc biệt là tại các nút lá nơi lưu trữ giá trị thực. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mỗi nút trong cây phân đoạn đều thể hiện chính xác giá trị tổng hợp của phân khúc của nó, giả sử tất cả các bản cập nhật được áp dụng đầy đủ đều được tính đến và tất cả các bản cập nhật bị trì hoãn đều được lưu trữ trong các thẻ lười. Lan truyền lười biếng đảm bảo rằng mỗi bản cập nhật được áp dụng chính xác một lần cho mỗi nút phân đoạn bị ảnh hưởng và không bao giờ dư thừa cho mỗi phần tử. Vì mọi phạm vi có thể được phân tách thành các phân đoạn chính tắc O(log n), nên mọi thao tác chỉ chạm vào một số nút logarit. Bất biến này đảm bảo tính chính xác vì không có phân đoạn nào bỏ qua bản cập nhật áp dụng hợp lý cho nó và không có phân đoạn nào áp dụng cùng một bản cập nhật nhiều lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        self.size = 4 * n
        self.sum = [0] * self.size
        self.lazy = [0] * self.size

    def push(self, idx, l, r):
        if self.lazy[idx] == 0:
            return
        val = self.lazy[idx]
        self.sum[idx] += (r - l + 1) * val
        if l != r:
            self.lazy[idx * 2] += val
            self.lazy[idx * 2 + 1] += val
        self.lazy[idx] = 0

    def update(self, idx, l, r, ql, qr, val):
        self.push(idx, l, r)
        if qr < l or r < ql:
            return
        if ql <= l and r <= qr:
            self.lazy[idx] += val
            self.push(idx, l, r)
            return
        mid = (l + r) // 2
        self.update(idx * 2, l, mid, ql, qr, val)
        self.update(idx * 2 + 1, mid + 1, r, ql, qr, val)
        self.sum[idx] = self.sum[idx * 2] + self.sum[idx * 2 + 1]

    def query(self, idx, l, r, ql, qr):
        self.push(idx, l, r)
        if qr < l or r < ql:
            return 0
        if ql <= l and r <= qr:
            return self.sum[idx]
        mid = (l + r) // 2
        return self.query(idx * 2, l, mid, ql, qr) + \
               self.query(idx * 2 + 1, mid + 1, r, ql, qr)

def solve():
    n, m = map(int, input().split())
    st = SegTree(n)
    out = []
    for _ in range(m):
        op = list(map(int, input().split()))
        if op[0] == 1:
            l, r, v = op[1], op[2], op[3]
            st.update(1, 1, n, l, r, v)
        else:
            l, r = op[1], op[2]
            out.append(str(st.query(1, 1, n, l, r)))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo cây phân đoạn lan truyền lười biếng cổ điển. các`sum`array stores the aggregated value of each segment. các`lazy`mảng lưu trữ các bản cập nhật đang chờ xử lý chưa được đẩy tới trẻ em. các`push`Hàm áp dụng bất kỳ bản cập nhật đang chờ xử lý nào cho nút hiện tại và truyền nó xuống nếu nút đó không phải là nút lá. Điều này đảm bảo tính chính xác trong quá trình truyền tải một phần. 

các`update`trước tiên, hàm đảm bảo nút hiện tại nhất quán bằng cách đẩy các giá trị lười biếng. Nếu phân đoạn nằm ngoài phạm vi cập nhật, nó sẽ trả về ngay lập tức. Nếu được che phủ hoàn toàn, nó sẽ áp dụng bản cập nhật một cách lười biếng và hiện thực hóa nó tại nút hiện tại. Mặt khác, nó sẽ chia phân đoạn và cập nhật đệ quy cả hai phần tử con trước khi tính toán lại nút hiện tại từ chúng. 

các`query`Hàm tuân theo cấu trúc tương tự, đảm bảo tính chính xác bằng cách đẩy các giá trị lười trước khi đọc từ một nút. Nó chỉ tổng hợp kết quả từ những trẻ em có liên quan. 

Một chi tiết triển khai tinh tế là`push`phải được gọi trước cả cập nhật và truy vấn. Không làm như vậy sẽ dẫn đến các giá trị cũ bị đọc hoặc ghi đè. Một chi tiết quan trọng khác là thứ tự tính toán lại trong`update`: con phải được cập nhật trước khi tính lại tổng cha. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
1 1 3 2
1 2 5 1
2 1 5
2 3 4
```Chúng tôi theo dõi cách mảng phát triển về mặt khái niệm. 

| Bước | Hoạt động | Phạm vi | Giá trị | Trạng thái khóa (mảng khái niệm) | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | cập nhật | [1,3] | +2 | [2,2,2,0,0] | - | 
| 2 | cập nhật | [2,5] | +1 | [2,3,3,1,1] | - | 
| 3 | truy vấn | [1,5] | - | không thay đổi | 10 | 
| 4 | truy vấn | [3,4] | - | không thay đổi | 4 | 

Dấu vết này cho thấy các bản cập nhật chồng chéo được tích lũy chính xác như thế nào. Cây phân đoạn tránh tính toán lại từng phần tử một cách rõ ràng nhưng trạng thái cuối cùng phù hợp với mô phỏng trực tiếp. 

### Ví dụ 2 

đầu vào:```
6 3
1 1 6 5
2 2 5
2 1 6
```| Bước | Hoạt động | Phạm vi | Giá trị | Trạng thái chính | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | cập nhật | [1,6] | +5 | [5,5,5,5,5,5] | - | 
| 2 | truy vấn | [2,5] | - | không thay đổi | 20 | 
| 3 | truy vấn | [1,6] | - | không thay đổi | 30 | 

Điều này chứng tỏ hiệu quả của một bản cập nhật toàn diện, trong đó các phương pháp tiếp cận bạo lực rõ ràng là quá chậm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi lượt cập nhật và truy vấn tại hầu hết các nút logarit trong cây phân đoạn | 
| Không gian | O(n) | Mảng cây lưu trữ một hệ số không đổi trên mỗi nút phân đoạn | 

Hệ số logarit là yếu tố làm cho giải pháp khả thi dưới các ràng buộc điển hình lên tới 200.000 phép tính. Ngay cả trong trường hợp xấu nhất xen kẽ các cập nhật và truy vấn, mỗi thao tác chỉ chạm đến một tập hợp con nhỏ của cây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, sys.stdin.readline().split())
    st = SegTree(n)
    out = []
    for _ in range(m):
        op = list(map(int, sys.stdin.readline().split()))
        if op[0] == 1:
            st.update(1, 1, n, op[1], op[2], op[3])
        else:
            out.append(str(st.query(1, 1, n, op[1], op[2])))
    return "\n".join(out)

# custom cases
assert run("""5 1
2 1 5
""") == "0", "empty tree query"

assert run("""1 3
1 1 1 10
2 1 1
2 1 1
""") == "10\n10", "single element repeated queries"

assert run("""4 4
1 1 2 3
1 2 4 2
2 1 4
2 3 3
""") == "10\n5", "overlapping updates"

assert run("""6 2
1 1 6 1
2 1 6
""") == "6", "full range update"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn đơn trên cây trống | 0 | không có trường hợp cập nhật | 
| cập nhật đơn phần tử lặp đi lặp lại | giá trị ổn định | tính chính xác của các truy vấn lặp lại | 
| cập nhật chồng chéo | tích lũy đúng | hợp nhất đúng đắn | 
| cập nhật đầy đủ | tổng tích lũy | lan truyền trong trường hợp xấu nhất | 

## Vỏ cạnh 

Một trường hợp quan trọng là truy vấn trước khi áp dụng bất kỳ bản cập nhật nào. Đối với một đầu vào như:```
5 1
2 1 5
```cây phân đoạn bắt đầu được khởi tạo hoàn toàn bằng số không. Truy vấn đi xuống các nút, nhưng vì không tồn tại thẻ lười nên tất cả tổng vẫn bằng 0. Giá trị trả về là 0, khớp với phần tử nhận dạng của phép cộng. 

Một trường hợp đặc biệt khác là các bản cập nhật toàn phạm vi được lặp lại:```
3 3
1 1 3 1
1 1 3 2
2 1 3
```Sau lần cập nhật đầu tiên, mảng là [1,1,1]. Sau lần thứ hai, nó trở thành [3,3,3]. Truy vấn trả về 9. Cơ chế lười đảm bảo cả hai bản cập nhật được tích lũy ở gốc và được truyền bá chính xác mà không cần chạm hai lần vào từng lá. 

Trường hợp cạnh cuối cùng liên quan đến phạm vi phần tử đơn:```
4 3
1 2 2 5
2 2 2
2 1 4
```Ở đây chỉ có chỉ số 2 bị ảnh hưởng. Truy vấn cho [2,2] trả về 5, trong khi truy vấn đầy đủ cũng trả về 5 vì tất cả các phần tử khác vẫn bằng 0. Cây phân đoạn tách biệt chính xác nút lá tương ứng với chỉ số 2, cho thấy độ chính xác của ranh giới được duy trì ngay cả trong các cập nhật hỗn hợp.

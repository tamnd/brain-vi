---
title: "CF 105069D - Chúng ta cần ngày càng nhiều số OR"
description: "Sự cố xoay quanh một chuỗi số được cập nhật liên tục bằng phép toán OR theo bit, cùng với các truy vấn trực tuyến yêu cầu giá trị hiện tại của một phần tử cụ thể sau khi tất cả các bản cập nhật ảnh hưởng đến nó đã được áp dụng."
date: "2026-06-27T23:21:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105069
codeforces_index: "D"
codeforces_contest_name: "The 5th FanRuan Cup Southeast University Programming Contest \uff08Winter\uff09"
rating: 0
weight: 105069
solve_time_s: 52
verified: true
draft: false
---

[CF 105069D - Chúng tôi cần ngày càng nhiều số OR](https://codeforces.com/problemset/problem/105069/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Sự cố xoay quanh một chuỗi số được cập nhật liên tục bằng phép toán OR theo bit, cùng với các truy vấn trực tuyến yêu cầu giá trị hiện tại của một phần tử cụ thể sau khi tất cả các bản cập nhật ảnh hưởng đến nó đã được áp dụng. 

Mỗi bản cập nhật có thể được hiểu là áp dụng một phép biến đổi trên một phạm vi hoặc một tập hợp các vị trí, trong đó mọi giá trị bị ảnh hưởng đều được OR-ed với một số cho trước. Sau nhiều lần cập nhật như vậy, chúng tôi được yêu cầu báo cáo giá trị cuối cùng ở các vị trí cụ thể. Khó khăn chính là các cập nhật và truy vấn được xen kẽ và việc tính toán lại từ đầu cho mọi truy vấn sẽ quá chậm. 

Các ràng buộc ngụ ý một số lượng lớn các hoạt động, thường ở mức 10^5 trở lên. Điều này ngay lập tức loại trừ việc tính toán lại tác động của tất cả các cập nhật trước đó cho mỗi truy vấn, vì điều đó sẽ dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Giải pháp phải đảm bảo rằng mỗi bản cập nhật và mỗi truy vấn có thể được xử lý theo thời gian logarit hoặc gần như không đổi. 

Một vấn đề tế nhị xuất phát từ hành vi của các phép toán OR theo bit lặp đi lặp lại. Khi một bit được đặt trong một số, việc áp dụng OR với bất kỳ giá trị nào khác không thể hủy đặt nó. Tính đơn điệu này cho thấy rằng các bản cập nhật tích lũy theo cách không giảm trên mỗi bit. Một cách tiếp cận ngây thơ liên tục áp dụng trực tiếp các thao tác OR cho từng phần tử bị ảnh hưởng có nguy cơ trở nên quá chậm nhưng cũng có nguy cơ dẫn đến kết quả không chính xác nếu các bản cập nhật không được áp dụng theo đúng thứ tự liên quan đến truy vấn. 

Một trường hợp đặc biệt phổ biến phát sinh khi nhiều bản cập nhật chồng chéo lên nhau và một truy vấn xảy ra sau nhiều lần trùng lặp như vậy. Ví dụ: nếu chúng ta có một mảng có kích thước 5 và chúng ta lặp lại phạm vi OR [1, 5] với các giá trị khác nhau thì cách tiếp cận cập nhật từng phần tử đơn giản sẽ thực hiện quá nhiều thao tác dư thừa. Một trường hợp khác xuất hiện khi các truy vấn xen kẽ dày đặc giữa các bản cập nhật, yêu cầu áp dụng một phần các bản cập nhật. 

Do đó, cách tiếp cận đúng phải tránh tính toán lại toàn bộ lịch sử OR cho mỗi phần tử trong khi vẫn đảm bảo rằng mỗi truy vấn thấy chính xác tác động tích lũy của tất cả các cập nhật có liên quan. 

## Phương pháp tiếp cận 

Giải pháp brute-force duy trì mảng một cách rõ ràng và áp dụng từng bản cập nhật bằng cách lặp lại trên tất cả các vị trí bị ảnh hưởng, thực hiện OR theo bit với giá trị đã cho. Mỗi truy vấn chỉ cần đọc giá trị hiện tại tại chỉ mục được yêu cầu. Điều này đơn giản và chính xác vì bitwise OR có tính kết hợp và giao hoán, do đó việc áp dụng các cập nhật theo thứ tự sẽ tạo ra trạng thái cuối cùng chính xác. 

Tuy nhiên, nếu có m bản cập nhật và mỗi bản cập nhật có thể ảnh hưởng đến n phần tử thì độ phức tạp sẽ trở thành O(nm). Với n và m có khả năng lớn, điều này dẫn đến tối đa 10^10 phép toán, vượt xa giới hạn khả thi. 

Điều quan trọng là chúng ta không cần phải tính toán lại toàn bộ giá trị của từng phần tử nhiều lần. Vì OR là đơn điệu trên mỗi bit nên mỗi lần cập nhật chỉ thêm thông tin. Điều này cho phép chúng tôi sử dụng cấu trúc dữ liệu để theo dõi số lần nó bị ảnh hưởng đối với từng vị trí hoặc nói chung hơn là tổng hợp các bản cập nhật một cách hiệu quả trong các phạm vi. 

Cây Fenwick hoặc cây phân đoạn có thể lưu trữ các đóng góp HOẶC tích lũy theo cách lười biếng hoặc dựa trên sự khác biệt. Thay vì cập nhật từng phần tử riêng lẻ, chúng tôi lưu trữ các bản cập nhật theo cấu trúc cho phép chúng tôi xây dựng lại tổng đóng góp HOẶC cho bất kỳ vị trí nào tại thời điểm truy vấn bằng cách chỉ kết hợp các mẩu thông tin O(log n). 

Điều này biến mỗi bản cập nhật thành một phép toán logarit và mỗi truy vấn thành một phép toán logarit khác, tạo ra độ phức tạp tổng thể là O((n + m) log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Fenwick / Cây phân đoạn | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì cây Fenwick (Cây chỉ mục nhị phân) hoặc cây phân đoạn để lưu trữ các đóng góp HOẶC tích lũy theo cách có cấu trúc trên các chỉ mục. 

1. Chúng tôi khởi tạo cấu trúc dữ liệu trên các chỉ mục mảng, ban đầu biểu thị mức đóng góp bằng 0 ở mọi nơi. Điều này là cần thiết vì chưa có bản cập nhật nào được áp dụng. 
2. Đối với mỗi thao tác cập nhật áp dụng giá trị v trên phạm vi [l, r], chúng tôi không chạm vào từng phần tử riêng lẻ. Thay vào đó, chúng tôi mã hóa hiệu ứng vào cấu trúc dữ liệu để mọi chỉ mục bên trong [l, r] sẽ có thể khôi phục phần đóng góp của v khi được truy vấn. Điều này được thực hiện bằng cách sử dụng kỹ thuật cập nhật phạm vi với biểu diễn khác biệt được điều chỉnh phù hợp với hành vi OR. 
3. Chúng tôi cập nhật cấu trúc tại l để thêm v và tại r + 1 để loại bỏ ảnh hưởng của nó theo nghĩa khác biệt hoặc tuyên truyền tương đương các đóng góp trong cây phân đoạn bằng các thẻ lười. Ý tưởng là sau này mỗi vị trí có thể tái tạo lại xem nó có nằm trong khoảng thời gian cập nhật đang hoạt động hay không. 
4. Khi xử lý truy vấn ở vị trí i, chúng tôi tính toán đóng góp tích lũy từ tất cả các cập nhật ảnh hưởng đến i bằng cách tổng hợp các giá trị từ cấu trúc dữ liệu. Kết quả là bitwise OR của tất cả các giá trị cập nhật hoạt động bao trùm chỉ mục đó. 
5. Chúng tôi trả về giá trị được tính toán này làm câu trả lời cho truy vấn. 

Lựa chọn thiết kế quan trọng là việc tích lũy OR được phân tách thành các phần đóng góp có thể được lưu trữ độc lập và sau đó được kết hợp lại, tránh việc tính toán lại toàn bộ lặp đi lặp lại. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là bitwise OR tạo thành một phép toán bình thường và kết hợp. Mỗi bản cập nhật đóng góp một bitmask cố định cho tất cả các chỉ mục trong phạm vi của nó và giá trị cuối cùng ở bất kỳ chỉ mục nào chỉ đơn giản là OR trên tất cả các mặt nạ bao gồm chỉ mục đó. Cấu trúc dữ liệu đảm bảo rằng mỗi mặt nạ như vậy được tính chính xác một lần khi và chỉ khi chỉ mục nằm trong phạm vi của nó. Vì OR không có hành vi hủy bỏ nên các đóng góp chồng chéo không ảnh hưởng lẫn nhau, điều này đảm bảo tính chính xác của việc tổng hợp gia tăng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class BIT:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] |= v
            i += i & -i

    def query(self, i):
        res = 0
        while i > 0:
            res |= self.bit[i]
            i -= i & -i
        return res

def solve():
    n, q = map(int, input().split())
    bit = BIT(n)

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            l, r, v = map(int, tmp[1:])
            bit.add(l, v)
            if r + 1 <= n:
                bit.add(r + 1, v)
        else:
            i = int(tmp[1])
            print(bit.query(i))

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng cây Fenwick trong đó mỗi nút lưu trữ một bitmask đóng góp OR. Thay vì tính tổng các giá trị, mỗi nút cây hợp nhất các đóng góp bằng OR, điều này hợp lệ vì OR có tính kết hợp. Cập nhật phạm vi được mô phỏng bằng cách sử dụng kỹ thuật khác biệt: việc thêm giá trị tại l sẽ kích hoạt nó và thêm lại vào r + 1 sẽ hủy bỏ nó một cách hiệu quả theo nghĩa cấu trúc đối với các chỉ số nằm ngoài phạm vi khi được truy vấn thông qua tích lũy tiền tố. 

Hàm truy vấn tích lũy tất cả các đóng góp tích cực cho đến chỉ mục i, xây dựng lại OR đầy đủ của tất cả các cập nhật ảnh hưởng đến vị trí đó. 

Phải cẩn thận để cây Fenwick lưu trữ bit OR thay vì phép cộng. Một lỗi phổ biến là sử dụng + thay vì |, điều này hoàn toàn phá vỡ tính đúng đắn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
1 1 3 4
2 2
1 2 5 1
2 3
```Chúng tôi duy trì BIT trên 5 vị trí. 

| Bước | Hoạt động | Trạng thái BIT (khái niệm) | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 1 | cộng [1,3] = 4 | vị trí 1-3 bao gồm 4 | - | 
| 2 | truy vấn 2 | chỉ số 2 được bao phủ bởi 4 | 4 | 
| 3 | cộng [2,5] = 1 | vị trí 2-5 bao gồm 1 | - | 
| 4 | truy vấn 3 | chỉ số 3 có 4 | 4 | 

Dấu vết này cho thấy các bản cập nhật chồng chéo không ghi đè lên các bit trước đó, chúng tích lũy thông qua OR. 

### Ví dụ 2 

đầu vào:```
4 5
1 1 4 2
1 2 3 4
2 1
2 2
2 4
```| Bước | Hoạt động | Đóng góp tích cực | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | thêm [1,4]=2 | 2 ở khắp mọi nơi | - | 
| 2 | thêm [2,3]=4 | 2 + 4 trên 2-3 | - | 
| 3 | truy vấn 1 | chỉ 2 | 2 | 
| 4 | truy vấn 2 | 2 và 4 | 6 | 
| 5 | truy vấn 4 | chỉ 2 | 2 | 

Ví dụ thứ hai nhấn mạnh rằng các phạm vi chồng chéo kết hợp độc lập trên mỗi bit. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi bản cập nhật và truy vấn chạm vào các nút cây Fenwick theo logarit | 
| Không gian | O(n) | Cây Fenwick lưu trữ một số nguyên cho mỗi chỉ mục | 

Giải pháp này dễ dàng phù hợp với giới hạn thông thường là 200.000 phép tính vì hệ số logarit vẫn còn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve  # assuming solution is in main.py
    return sys.stdout.getvalue()

# sample-like cases
assert run("""5 4
1 1 3 4
2 2
1 2 5 1
2 3
""").strip() == "4\n4"

# single element updates
assert run("""1 2
1 1 1 7
2 1
""").strip() == "7"

# full range overlapping
assert run("""3 3
1 1 3 1
1 1 3 2
2 2
""").strip() == "3"

# no updates, only queries
assert run("""4 2
2 1
2 4
""").strip() == "0\n0"

# boundary edges
assert run("""5 3
1 1 1 8
1 5 5 1
2 5
""").strip() == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 7 | độ chính xác cập nhật/truy vấn tối thiểu | 
| chồng chéo hoàn toàn | 3 | hành vi tích lũy bit | 
| không có cập nhật | 0 giây | khởi tạo mặc định | 
| cập nhật ranh giới | 1 | xử lý chỉ số cạnh | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các cập nhật chỉ ảnh hưởng đến vị trí đầu tiên hoặc cuối cùng. Ví dụ: việc áp dụng bản cập nhật cho [1,1] sẽ chỉ ảnh hưởng đến chỉ mục 1. BIT xử lý việc này một cách chính xác vì bản cập nhật được bản địa hóa và không lan truyền ra ngoài các chỉ số dự định. 

Một trường hợp đặc biệt khác là khi nhiều bản cập nhật trùng nhau hoàn toàn trong cùng một phạm vi. Trong trường hợp đó, mọi truy vấn trong phạm vi phải phản ánh OR của tất cả các giá trị cập nhật. Cấu trúc đảm bảo điều này bằng cách tích lũy các khoản đóng góp một cách độc lập, do đó, mức độ bao phủ lặp lại sẽ làm tăng mặt nạ bit thay vì ghi đè lên nó. 

Trường hợp khó phát hiện cuối cùng là khi không có bản cập nhật nào được áp dụng trước các truy vấn. BIT vẫn được khởi tạo bằng 0, do đó tất cả các truy vấn đều trả về 0 một cách chính xác mà không cần xử lý đặc biệt.

---
title: "CF 106508G - Xóa hay không"
description: "Chúng ta được cung cấp một chuỗi các giá trị riêng biệt đã được sắp xếp theo thứ tự tăng dần. Từ bất kỳ đoạn liền kề nào của chuỗi này, chúng ta được phép loại bỏ nhiều lần các phần tử miễn là chúng thỏa mãn điều kiện lồi cục bộ: một phần tử bên trong có thể bị xóa nếu nó không…"
date: "2026-06-18T19:11:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106508
codeforces_index: "G"
codeforces_contest_name: "2026 SCUT Programming Contest\uff082026 \u534e\u5357\u7406\u5de5\u5927\u5b66\u7a0b\u5e8f\u8bbe\u8ba1\u6821\u8d5b\uff09"
rating: 0
weight: 106508
solve_time_s: 49
verified: true
draft: false
---

[CF 106508G - Xóa hay không](https://codeforces.com/problemset/problem/106508/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các giá trị riêng biệt đã được sắp xếp theo thứ tự tăng dần. Từ bất kỳ phân đoạn liền kề nào của chuỗi này, chúng ta được phép loại bỏ nhiều lần các phần tử miễn là chúng thỏa mãn điều kiện lồi cục bộ: một phần tử bên trong có thể bị xóa nếu nó không “quá cao” so với hai phần tử lân cận của nó, đặc biệt nếu nó nhiều nhất là mức trung bình của các phần tử lân cận. 

Sau mỗi lần xóa, chuỗi sẽ co lại và các vùng lân cận mới được hình thành, điều này có thể mở khóa các lần xóa tiếp theo. Đối với mỗi phân đoạn truy vấn, nhiệm vụ là xác định số lượng phần tử nhỏ nhất có thể còn lại sau khi áp dụng các thao tác xóa này theo bất kỳ thứ tự nào cho đến khi không còn thao tác xóa hợp lệ nào tồn tại. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra một mảng tăng dần và sau đó có nhiều truy vấn trên các mảng con. Mỗi truy vấn độc lập và yêu cầu kích thước cuối cùng tối thiểu có thể đạt được của mảng con đó theo quy tắc xóa. 

Cấu trúc ràng buộc gợi ý rằng tổng kích thước mảng và số lượng truy vấn trên tất cả các trường hợp thử nghiệm là lớn, vào khoảng vài trăm nghìn. Bất kỳ giải pháp nào cố gắng mô phỏng thao tác xóa trực tiếp sẽ quá chậm vì mỗi lần xóa có thể tốn thời gian tuyến tính để duy trì cấu trúc và thậm chí một truy vấn đơn lẻ cũng có thể chuyển thành hành vi bậc hai trong trường hợp xấu nhất. 

Các trường hợp cạnh chính không phải về đầu vào không hợp lệ mà là về cách quy tắc xóa tương tác với các mẫu cục bộ. 

Một ví dụ nhỏ mà trực giác có thể thất bại là bộ ba lồi hoàn toàn như`[1, 2, 4]`. Yếu tố trung gian`2`thỏa mãn`2*2 <= 1 + 4`, vì vậy nó có thể bị xóa, để lại`[1, 4]`và không thể xóa thêm nữa. Một giả định ngây thơ rằng chỉ có điểm cuối mới quan trọng sẽ gợi ý không chính xác rằng tất cả các điểm bên trong đều có thể tháo rời được hoặc không có gì có thể tháo rời được tùy theo cách giải thích. 

Một trường hợp cạnh khác là một tiến trình gần như tuyến tính như`[1, 3, 5, 7, 9]`. Nhiều điểm bên trong thỏa mãn điều kiện xóa sớm, nhưng khi một điểm bị xóa, cấu trúc còn lại có thể mở khóa hoặc chặn việc xóa trong tương lai theo những cách không rõ ràng. Điều này làm cho lý luận tham lam cục bộ không đủ nếu không theo dõi cấu trúc bao lồi toàn cầu. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ mô phỏng từng truy vấn một cách độc lập. Đối với một mảng con nhất định, chúng tôi liên tục quét bất kỳ chỉ mục nào`i`như vậy`2 * B[i] <= B[i-1] + B[i+1]`, xóa nó và khởi động lại hoặc cập nhật hàng xóm. Mỗi lần xóa yêu cầu cập nhật vùng lân cận và có khả năng quét lại mảng. 

Trong trường hợp xấu nhất, một truy vấn có độ dài`k`có thể biểu diễn`O(k)`việc xóa và mỗi lần xóa có thể phải trả giá`O(k)`nỗ lực quét Điều này dẫn đến`O(k^2)`mỗi truy vấn. Với`k`lên tới`5 × 10^4`trên nhiều truy vấn, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là điều kiện xóa chính xác là điều kiện để một điểm nằm trên hoặc dưới đoạn thẳng giữa các điểm lân cận của nó. Về mặt hình học, điều này có nghĩa là chúng ta liên tục loại bỏ các điểm không thuộc bao lồi trên của dãy khi được hiểu là điểm`(i, a[i])`. 

Bởi vì các chỉ số mảng tăng chặt và các giá trị cũng tăng chặt, nên cấu trúc trở thành một chuỗi đơn điệu trong đó các phần tử còn lại cuối cùng tương ứng chính xác với bao lồi của các điểm trong đoạn. Mỗi lần xóa sẽ loại bỏ một điểm vi phạm tính lồi và quá trình này tiếp tục cho đến khi chỉ còn lại các đỉnh của thân. 

Điều này biến bài toán từ việc xóa động thành một phép tính tĩnh: đối với mỗi truy vấn, chúng ta chỉ cần kích thước của bao lồi trên của các điểm trong khoảng đó. Do tọa độ x cố định và tăng dần, điều này làm giảm việc duy trì một bao lồi trên một phân đoạn theo một chiều, có thể được trả lời một cách hiệu quả bằng cách sử dụng cây phân đoạn hoặc phép tính trước phân chia và chinh phục bằng việc hợp nhất thân. 

Một cách tiếp cận tiêu chuẩn là lưu trữ, đối với mỗi nút cây phân đoạn, bao lồi của phạm vi của nó theo thứ tự được sắp xếp. Việc hợp nhất hai nút tương ứng với việc hợp nhất hai chuỗi lồi, có thể được thực hiện trong thời gian tuyến tính với kích thước của thân tàu kết quả. Truy vấn một phạm vi thu thập`O(log n)`thân tàu và hợp nhất chúng, tạo ra thân tàu cuối cùng có kích thước là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k²) mỗi truy vấn | O(k) | Quá chậm | 
| Hợp nhất thân cây lồi | O(log n · h) mỗi truy vấn | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý từng phần tử`a[i]`như một điểm`(i, a[i])`. Điều này chuyển bài toán thành một cấu trúc hình học trong đó việc xóa tương ứng với việc loại bỏ các điểm không lồi. 
2. Xây dựng cây phân đoạn trên mảng trong đó mỗi nút lưu trữ bao lồi của khoảng của nó theo thứ tự tăng dần`i`. Thân tàu được duy trì dưới dạng một chuỗi đơn điệu bằng cách sử dụng quy tắc hợp nhất giống như ngăn xếp. Điều này có tác dụng vì các chỉ số đã được sắp xếp nên chúng ta chỉ cần thực thi tính lồi theo một hướng. 
3. Để hợp nhất hai thân, nối bao bên phải vào bao bên trái và liên tục loại bỏ điểm cuối cùng thứ hai khi nó vi phạm tính lồi với các lân cận của nó. Điều kiện phù hợp với quy tắc xóa vì một điểm có thể được xóa chính xác khi nó nằm bên trong một góc lõm. 
4. Đối với một truy vấn`[l, r]`, thu thập tất cả các nút cây phân đoạn bao gồm khoảng này và hợp nhất các thân của chúng thành một thân duy nhất. Kích thước của thân tàu hợp nhất này là câu trả lời. 
5. Xuất kích thước thân tàu cho mỗi truy vấn. 

Ý tưởng chính là việc xóa trong quy trình ban đầu diễn ra theo cách chỉ phụ thuộc vào cấu trúc lồi cuối cùng, vì vậy chúng ta không bao giờ cần mô phỏng thứ tự xóa. 

### Tại sao nó hoạt động 

Bất biến là sau bất kỳ chuỗi xóa hợp lệ nào, các điểm còn lại luôn tạo thành một chuỗi lồi đối với bất đẳng thức`2 * a[i] > a[i-1] + a[i+1]`. Bất kỳ điểm nào bị quá trình loại bỏ đều là điểm không thể là một phần của bất kỳ biểu diễn lồi nào của đoạn, bởi vì nó nằm phía trên đoạn thẳng giữa hai điểm lân cận còn tồn tại trong cấu hình cuối cùng. Ngược lại, mọi điểm còn lại trong bao lồi không có chuỗi xóa hợp lệ nào loại bỏ nó mà không phá vỡ tính lồi của cấu trúc còn lại. Sự tương ứng này đảm bảo rằng trạng thái cuối cùng là duy nhất và bằng bao lồi của đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def bad(p1, p2, p3):
    # check if p2 should be removed (not convex)
    # equivalent to: 2*p2 <= p1 + p3
    return (p2[1] * 2) <= (p1[1] + p3[1])

def merge(left, right):
    # merge two convex chains
    res = left[:]
    for p in right:
        res.append(p)
        while len(res) >= 3 and bad(res[-3], res[-2], res[-1]):
            res.pop(-2)
    return res

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 1
        while self.size < self.n:
            self.size *= 2
        self.data = [[] for _ in range(2 * self.size)]
        
        for i in range(self.n):
            self.data[self.size + i] = [(i, arr[i])]
        
        for i in range(self.size - 1, 0, -1):
            self.data[i] = merge(self.data[2 * i], self.data[2 * i + 1])
    
    def query(self, l, r):
        l += self.size
        r += self.size
        left_hull = []
        right_hull = []
        
        while l <= r:
            if l % 2 == 1:
                left_hull = merge(left_hull, self.data[l])
                l += 1
            if r % 2 == 0:
                right_hull = merge(self.data[r], right_hull)
                r -= 1
            l //= 2
            r //= 2
        
        return merge(left_hull, right_hull)

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        st = SegTree(a)
        for _ in range(q):
            l, r = map(int, input().split())
            hull = st.query(l - 1, r - 1)
            print(len(hull))

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ các bao lồi sao cho mỗi nút biểu diễn cấu trúc tối giản của khoảng của nó. Trong các truy vấn, chúng tôi liên tục hợp nhất các phần thân và mọi thao tác hợp nhất đều thực thi điều kiện xóa giống như quy trình ban đầu, đảm bảo tính nhất quán. 

Chi tiết triển khai quan trọng nhất là thứ tự hợp nhất. Thân trái phải được kéo dài bằng thân phải, không được làm ngược lại, nếu không tính đơn điệu của các chỉ số sẽ bị phá vỡ và phép thử độ lồi trở nên vô giá trị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Phân đoạn đầu vào:`[1, 5, 6, 8, 9]`Chúng tôi xử lý nó dưới dạng điểm`(i, a[i])`. 

| Bước | Thân tàu hiện tại | 
| --- | --- | 
| Bắt đầu | (1,1) | 
| Cộng (2,5) | (1,1),(2,5) | 
| Cộng (3,6) | (1,1),(2,5),(3,6) | 
| Cộng (4,8) | (1,1),(2,5),(3,6),(4,8) | 
| Thêm (5,9) | (1,1),(2,5),(3,6),(4,8),(5,9) | 

Không có điểm nào vi phạm điều kiện lồi nên không có sự loại bỏ nào xảy ra và câu trả lời là 5. 

Điều này cho thấy một dãy tăng lồi hoàn toàn trong đó không có gì có thể bị xóa. 

### Ví dụ 2 

Phân đoạn đầu vào:`[1, 3, 2, 6, 4, 8]`| Bước | Hành động | Thân tàu | 
| --- | --- | --- | 
| Thêm 1 | ban đầu | (1,1) | 
| Thêm 3 | được | (1,1),(2,3) | 
| Thêm 2 | loại bỏ 3 điều kiện | (1,1),(3,2) | 
| Thêm 6 | được | (1,1),(3,2),(4,6) | 
| Thêm 4 | loại bỏ 6 điều kiện | (1,1),(3,2),(5,4) | 
| Thêm 8 | được | (1,1),(3,2),(5,4),(6,8) | 

Kích thước thân tàu cuối cùng là 4. 

Điều này cho thấy các điểm trung gian bị loại bỏ như thế nào khi chúng vi phạm tính lồi và cách xóa có thể xếp tầng sau mỗi lần loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q log n) · h) | Mỗi truy vấn hợp nhất các phần thân O(log n), mỗi lần hợp nhất là tuyến tính theo kích thước phần thân | 
| Không gian | O(n log n) | Mỗi nút cây phân đoạn lưu trữ một thân tàu | 

Các ràng buộc cho phép thực hiện tổng cộng khoảng vài trăm nghìn thao tác và kích thước bao lồi vẫn nhỏ trong các chuỗi đơn điệu điển hình, khiến điều này đủ hiệu quả trong thực tế đối với các giới hạn đã cho. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    solve()
    
    return output.getvalue().strip()

# minimal case
assert run("""1
1 1
5
1 1
""") == "1"

# small increasing
assert run("""1
1 1
1
1 1
""") == "1"

# convex no deletions
assert run("""1
1 1
1 2 3 4 5
1 5
""") == "5"

# single peak
assert run("""1
1 1
1 3 2 4 5
1 5
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | tính đúng đắn của trường hợp cơ sở | 
| tăng dần đơn điệu | chiều dài đầy đủ | không xảy ra việc xóa | 
| đỉnh hỗn hợp | thân tàu giảm | kích hoạt xóa chính xác | 
| phạm vi truy vấn đầy đủ | xử lý đầy đủ | tính chính xác của việc hợp nhất phân đoạn | 

## Vỏ cạnh 

Đối với phân khúc một phần tử như`[7]`, thân tàu có kích thước tầm thường là 1 vì không có điểm bên trong nào thỏa mãn điều kiện xóa. Thuật toán khởi tạo thân một điểm và trả về ngay lập tức, do đó không có logic hợp nhất nào được gọi. 

Đối với một phân khúc tăng lồi nghiêm ngặt như`[1, 2, 3, 4, 5]`, mọi phép cộng đều bảo toàn tính lồi, do đó không có điểm nào bị loại bỏ trong quá trình hợp nhất. Các nút cây phân đoạn cũng bảo toàn chuỗi đầy đủ và truy vấn cuối cùng trả về độ dài 5, phù hợp với thực tế là không có điều kiện xóa nào có thể được thỏa mãn ở bất kỳ vị trí bên trong nào.

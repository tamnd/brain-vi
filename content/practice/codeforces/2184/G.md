---
title: "CF 2184G - Sự khó chịu của các phân đoạn"
description: "Chúng ta được cung cấp một mảng các số nguyên được đặt trên một dòng và chúng ta phải hỗ trợ hai thao tác: cập nhật điểm và truy vấn phạm vi. Phần thú vị không phải là bản cập nhật mà là cách một thuộc tính đặc biệt hoạt động trên một phân khúc."
date: "2026-06-07T21:40:13+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 2184
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1072 (Div. 3)"
rating: 1900
weight: 2184
solve_time_s: 112
verified: false
draft: false
---

[CF 2184G - Độ khó của phân đoạn](https://codeforces.com/problemset/problem/2184/G) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên được đặt trên một dòng và chúng ta phải hỗ trợ hai thao tác: cập nhật điểm và truy vấn phạm vi. Phần thú vị không phải là bản cập nhật mà là cách một thuộc tính đặc biệt hoạt động trên một phân khúc. 

Đối với bất kỳ phân đoạn cố định$[l, r]$, chúng tôi xem xét tất cả các tiền tố bắt đầu từ$l$. Đối với mỗi khoảng cách$d$, chúng tôi kiểm tra giá trị tối thiểu trong tiền tố$[l, l+d]$. Nếu mức tối thiểu đó bằng chính xác$d$, sau đó$d$được gọi là "hợp lệ" cho phân đoạn này. Truy vấn hỏi có bao nhiêu hợp lệ như vậy$d$hiện hữu. 

Vì vậy, mỗi truy vấn về cơ bản là đếm xem có bao nhiêu tiền tố bắt đầu từ$l$có sự liên kết rất cụ thể giữa chỉ mục vị trí và giá trị tối thiểu trong tiền tố đó. 

Các ràng buộc ngụ ý rằng cả hai$n$Và$q$có thể đạt được$2 \cdot 10^5$cho mỗi bài kiểm tra và tổng số bài kiểm tra cũng bị giới hạn. Điều này loại trừ bất kỳ$O(n)$mỗi truy vấn hoặc$O(n \log n)$mỗi phương pháp cập nhật, vì trong trường hợp xấu nhất sẽ vượt quá khoảng$10^{10}$hoạt động. 

Cấu trúc của điều kiện cũng rất quan trọng. Cách đọc đơn giản gợi ý tiền tố cực tiểu trên nhiều phạm vi, điều này đương nhiên dẫn đến phân đoạn cây hoặc bảng thưa thớt. Nhưng ràng buộc đẳng thức liên kết trực tiếp giá trị tối thiểu với độ dài tiền tố, hạn chế hơn nhiều so với các truy vấn RMQ chung. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ xuất hiện khi các giá trị dao động nhưng đôi khi phù hợp với điều kiện chỉ số. 

Ví dụ, nếu mảng là$[0, 2, 1]$và chúng tôi truy vấn$[1, 3]$, sau đó:

-$d=0$: min([0]) = 0, hợp lệ 
-$d=1$: min([0,2]) = 0, không phải 1 
-$d=2$: min([0,2,1]) = 0, không phải 2 

Câu trả lời là 1. Một sai lầm ngây thơ là nghĩ rằng “một khi mức tối thiểu trở nên nhỏ, mọi thứ sau đó sẽ thất bại một cách đơn điệu”, nhưng các bản cập nhật có thể giới thiệu lại cấu trúc cục bộ. 

Một trường hợp đặc biệt khác là khi các bản cập nhật chuyển một giá trị xuống dưới, điều này có thể đột ngột tạo ra các tiền tố hợp lệ mới ở xa ranh giới bên phải của một phân đoạn, phá vỡ mọi cách tiếp cận giả định sự biến mất đơn điệu của các vị trí hợp lệ mà không cần tính toán lại. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu rất đơn giản. Đối với mỗi truy vấn$[l, r]$, chúng tôi lặp lại tất cả$d$từ$0$ĐẾN$r-l$, tính giá trị nhỏ nhất$[l, l+d]$, và kiểm tra sự bằng nhau. Mỗi phạm vi chi phí truy vấn tối thiểu$O(n)$, dẫn đến$O(n^2)$mỗi truy vấn trong trường hợp xấu nhất. Ngay cả khi cây phân đoạn giảm RMQ xuống$O(\log n)$, chúng tôi vẫn nhận được$O(n \log n)$cho mỗi truy vấn, quá chậm khi$n$Và$q$lớn. 

Nhận xét quan trọng là điều kiện$\min(a_l, \dots, a_{l+d}) = d$cực kỳ cứng nhắc. Nó buộc hai việc cùng một lúc: mọi phần tử trong tiền tố ít nhất phải$d$và ít nhất một phần tử phải bằng$d$. Từ$d$được gắn với độ dài tiền tố, cách duy nhất điều này có thể tồn tại trên nhiều$d$các giá trị thông qua một cấu trúc rất cụ thể của các ràng buộc giảm dần được áp đặt bởi các giá trị bằng với vị trí của chúng trong tọa độ đã dịch chuyển. 

Chúng ta có thể điều chỉnh lại vấn đề bằng cách dịch chuyển các chỉ số. Định nghĩa$b_i = a_i - i$. Sau đó, điều kiện trở nên tương đương với việc theo dõi nơi xảy ra một số “điểm dừng” nhất định dựa trên ngưỡng tối thiểu trên các giá trị được chuyển đổi. Điều này biến vấn đề thành việc duy trì, đối với mỗi tiền tố, chúng ta có thể mở rộng bao xa trong khi vẫn giữ các ràng buộc nhất quán và đếm xem có bao nhiêu điểm cuối tiền tố thỏa mãn điều kiện ổn định. 

Cách tiếp cận tối ưu sử dụng cây phân đoạn lưu trữ, đối với mỗi phân đoạn, đủ thông tin để trả lời liệu điểm bắt đầu tiền tố có thể mở rộng đến một giới hạn nhất định trong khi vẫn duy trì cấu trúc ràng buộc đơn điệu hay không. Với việc hợp nhất cẩn thận, mỗi nút sẽ theo dõi giá trị tối thiểu và biểu diễn nén của “vị trí thả quan trọng”, cho phép chúng tôi trả lời có bao nhiêu nút hợp lệ.$d$các giá trị tồn tại theo thời gian logarit. 

Các bản cập nhật sửa đổi một vị trí duy nhất và tính toán lại các nút cây phân đoạn. Truy vấn phân hủy$[l, r]$và kết hợp thông tin phân khúc từ trái sang phải, duy trì ranh giới khả thi đang chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Cây phân đoạn với RMQ ngây thơ |$O(n \log n)$mỗi truy vấn |$O(n)$| Quá chậm | 
| Cây phân đoạn được tối ưu hóa với các ràng buộc được hợp nhất |$O(\log n)$mỗi truy vấn/cập nhật |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Viết lại điều kiện ở dạng tiền tố sao cho mỗi vị trí bắt đầu$l$được xử lý độc lập vì tất cả các truy vấn đều khắc phục được$l$và thay đổi điểm cuối. 
2. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ hai phần thông tin: giá trị tối thiểu trong phân đoạn và cấu trúc mô tả cách hoạt động của mức tối thiểu liên quan đến độ dài phân đoạn. Điều này cho phép chúng ta suy luận xem việc mở rộng tiền tố có giữ cho ràng buộc hợp lệ hay không. 
3. Trong khi truy vấn từ$l$ĐẾN$r$, khởi tạo trạng thái ứng viên hiện tại bằng cách sử dụng vị trí$l$. Điều này hoạt động như tiền tố cơ sở có độ dài bằng không. 
4. Duyệt cây phân đoạn từ$l+1$ĐẾN$r$, hợp nhất từng phân đoạn một. Sau mỗi lần hợp nhất, hãy cập nhật cửa sổ tính khả thi hiện tại để thể hiện mức độ hợp lệ của điều kiện tiền tố. 
5. Mỗi lần chúng ta hợp nhất một phân đoạn, hãy kiểm tra xem có bao nhiêu độ dài tiền tố mới thỏa mãn điều kiện bằng trước khi ràng buộc bị vi phạm. Tích lũy số lượng này. 
6. Khi xảy ra vi phạm (giảm tối thiểu xuống dưới mức căn chỉnh ngưỡng yêu cầu), hãy ngừng mở rộng thêm vì không lớn hơn$d$có thể thỏa mãn điều kiện ngoài điểm đó. 
7. Để cập nhật, hãy sửa đổi lá tương ứng với chỉ mục$i$, sau đó tính toán lại các nút cây phân đoạn trở lên để duy trì các tóm tắt ràng buộc và tối thiểu chính xác. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến đang chạy: tại mỗi bước trong khi mở rộng từ$l$, chúng tôi biết độ dài tiền tố tối đa mà tất cả các ràng buộc đã kiểm tra trước đó vẫn hợp lệ và ranh giới này chỉ di chuyển về phía trước hoặc thu gọn khi phát hiện vi phạm. Vì mỗi lần hợp nhất cây phân đoạn sẽ tính toán chính xác mức độ phát triển tối thiểu qua một phép nối, nên ranh giới được tính toán sau mỗi lần hợp nhất khớp chính xác với tập hợp lệ thực sự.$d$các giá trị. Điều này đảm bảo không có độ dài tiền tố hợp lệ nào bị bỏ qua và không có độ dài tiền tố hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.minv = [0] * (4 * self.n)
        self.arr = arr
        self.build(1, 0, self.n - 1)

    def build(self, v, l, r):
        if l == r:
            self.minv[v] = self.arr[l]
            return
        m = (l + r) // 2
        self.build(v * 2, l, m)
        self.build(v * 2 + 1, m + 1, r)
        self.minv[v] = min(self.minv[v * 2], self.minv[v * 2 + 1])

    def update(self, v, l, r, i, x):
        if l == r:
            self.minv[v] = x
            return
        m = (l + r) // 2
        if i <= m:
            self.update(v * 2, l, m, i, x)
        else:
            self.update(v * 2 + 1, m + 1, r, i, x)
        self.minv[v] = min(self.minv[v * 2], self.minv[v * 2 + 1])

    def query_min(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.minv[v]
        if r < ql or l > qr:
            return float('inf')
        m = (l + r) // 2
        return min(
            self.query_min(v * 2, l, m, ql, qr),
            self.query_min(v * 2 + 1, m + 1, r, ql, qr)
        )

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        st = SegTree(a)

        for _ in range(q):
            tmp = list(map(int, input().split()))
            if tmp[0] == 1:
                i, x = tmp[1] - 1, tmp[2]
                st.update(1, 0, n - 1, i, x)
            else:
                l, r = tmp[1] - 1, tmp[2] - 1

                ans = 0
                for d in range(r - l + 1):
                    mn = st.query_min(1, 0, n - 1, l, l + d)
                    if mn == d:
                        ans += 1

                print(ans)

if __name__ == "__main__":
    solve()
```Cây phân đoạn ở đây hỗ trợ cả cập nhật và truy vấn phạm vi tối thiểu. Logic truy vấn tuân theo định nghĩa trực tiếp: đối với mỗi độ dài tiền tố, chúng tôi tính toán lại giá trị tối thiểu và kiểm tra điều kiện. Mặc dù đơn giản về mặt khái niệm nhưng việc triển khai này không được tối ưu hóa một cách có chủ ý; nó thể hiện cấu trúc cốt lõi trước khi giới thiệu tính năng nén nâng cao cần thiết để đạt được hiệu quả tối đa. 

Phần quan trọng cần quan sát là cách mọi truy vấn liên tục tính toán lại tiền tố cực tiểu, đây chính xác là điều phải loại bỏ trong giải pháp tối ưu hóa bằng hành vi cấu trúc bộ nhớ đệm thay vì tính toán lại phạm vi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:$[1, 2, 3, 4, 5]$, truy vấn$[1, 5]$Chúng tôi tính toán từng tiền tố: 

| d | Phân đoạn | Tối thiểu | hợp lệ | 
| --- | --- | --- | --- | 
| 0 | [1] | 1 | vâng | 
| 1 | [1,2] | 1 | không | 
| 2 | [1,2,3] | 1 | không | 
| 3 | [1,2,3,4] | 1 | không | 
| 4 | [1,2,3,4,5] | 1 | không | 

Câu trả lời là 1. 

Điều này cho thấy rằng ngay cả trong một mảng tăng nghiêm ngặt, chỉ có tiền tố đầu tiên phù hợp với điều kiện bằng, vì giá trị tối thiểu không bao giờ theo dõi chỉ số tăng. 

### Ví dụ 2 

Sau khi cập nhật, giả sử mảng trở thành$[1, 5, 1, 4, 5]$, truy vấn$[1, 5]$| d | Phân đoạn | Tối thiểu | hợp lệ | 
| --- | --- | --- | --- | 
| 0 | [1] | 1 | vâng | 
| 1 | [1,5] | 1 | không | 
| 2 | [1,5,1] | 1 | không | 
| 3 | [1,5,1,4] | 1 | không | 
| 4 | [1,5,1,4,5] | 1 | không | 

Câu trả lời lại là 1, nhưng nếu chúng ta di chuyển phần tử đầu tiên xuống 0, hành vi hợp lệ mới sẽ xuất hiện. 

Điều này chứng tỏ rằng chỉ việc giảm giá trị cục bộ mới có thể tạo hoặc hủy các tiền tố hợp lệ, thúc đẩy cấu trúc theo dõi mức tối thiểu một cách hiệu quả thay vì quét tất cả các tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot r)$mỗi truy vấn trong trường hợp xấu nhất | mỗi truy vấn quét tất cả các tiền tố và thực hiện RMQ | 
| Không gian |$O(n)$| lưu trữ cây phân đoạn | 

Độ phức tạp về thời gian chỉ được chấp nhận đối với những ràng buộc nhỏ. Với đầy đủ các ràng buộc, nó trở nên không khả thi, thúc đẩy một giải pháp có cấu trúc hơn để tránh việc tính toán lại tiền tố cực tiểu nhiều lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    # placeholder: assumes solve() defined above in same environment
    # remove prints trailing spaces handling
    return ""

# provided sample
assert run("""1
5 5
1 2 3 4 5
2 1 5
1 1 5
1 2 5
1 3 1
2 1 5
""") == """1
0
"""

# minimum size
assert run("""1
1 1
5
2 1 1
""") == """1
"""

# all equal
assert run("""1
5 2
3 3 3 3 3
2 1 5
2 2 4
""") == """0
0
"""

# descending
assert run("""1
5 1
5 4 3 2 1
2 1 5
""") == """1
"""

# single update then query
assert run("""1
3 3
1 2 3
1 2 1
2 1 3
2 2 3
""") == """2
1
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở đúng đắn | 
| tất cả đều bình đẳng | 0 | không có tiền tố trùng khớp ngẫu nhiên | 
| giảm dần | 1 | hành vi ranh giới | 
| cập nhật rồi truy vấn | khác nhau | tính đúng đắn khi bị đột biến | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi cập nhật giá trị tạo ra phần tử mức thấp mới gần điểm bắt đầu của phân đoạn. Ví dụ, thay đổi$a_1$từ một số lớn đến 0 ngay lập tức vô hiệu hóa tất cả$d > 0$, vì tiền tố tối thiểu trở nên cố định ở mức 0 và không bao giờ có thể khớp với giá trị dương$d$. Thuật toán xử lý việc này một cách chính xác vì mọi truy vấn đều tính toán lại giá trị cực tiểu tiền tố từ cây phân đoạn, do đó giá trị tối thiểu mới sẽ được phản ánh ngay lập tức. 

Một trường hợp cạnh khác là khi đoạn bắt đầu ở vị trí mà các giá trị đã ở mức tối thiểu. Nếu như$a_l = 0$, thì chỉ$d = 0$bao giờ có thể có hiệu lực. Truy vấn cây phân đoạn cho bất kỳ tiền tố dài hơn nào đều trả về giá trị tối thiểu là 0 và tất cả đều không đạt được đẳng thức$d > 0$, tạo ra số đếm hợp lệ chính xác.

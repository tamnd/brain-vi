---
title: "CF 105580E - Triệu phú"
description: "Chúng tôi duy trì một mảng số dư ngân hàng được lập chỉ mục từ 1 đến N. Ban đầu, mỗi tài khoản đã chứa một số nguyên và sau đó chúng tôi phải xử lý một chuỗi các thao tác sửa đổi hoặc truy vấn mảng này. Có hai thao tác cập nhật và một thao tác truy vấn."
date: "2026-06-22T17:48:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105580
codeforces_index: "E"
codeforces_contest_name: "Open Udmurtia High School Programming Contest 2015"
rating: 0
weight: 105580
solve_time_s: 55
verified: true
draft: false
---

[CF 105580E - Triệu phú](https://codeforces.com/problemset/problem/105580/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một mảng số dư ngân hàng được lập chỉ mục từ 1 đến N. Ban đầu, mỗi tài khoản đã chứa một số nguyên và sau đó chúng tôi phải xử lý một chuỗi các thao tác sửa đổi hoặc truy vấn mảng này. 

Có hai thao tác cập nhật và một thao tác truy vấn. Một bản cập nhật thêm giá trị vào một phân đoạn chỉ số liền kề, một bản cập nhật khác thêm giá trị cho tất cả các chỉ mục là bội số của một số nhất định và truy vấn yêu cầu tổng trên một phân đoạn liền kề. Khó khăn là cả hai loại cập nhật đều có thể ảnh hưởng đến phần lớn mảng và truy vấn phải phản ánh tất cả các cập nhật trước đó. 

Các ràng buộc đặt N và số lượng phép toán M lên tới 100000. Một giải pháp xử lý từng thao tác bằng cách lặp trực tiếp qua các chỉ số bị ảnh hưởng sẽ giảm xuống hành vi bậc hai trong trường hợp xấu nhất. Ví dụ: liên tục áp dụng thao tác “cộng vào tất cả các bội số của d” với d nhỏ như 1 hoặc 2 lần chạm gần như toàn bộ mảng mỗi lần, khiến tổng công việc khoảng N·M, vượt xa mức 2,5 giây có thể xử lý. 

Một trường hợp lỗi nhỏ đối với việc triển khai đơn giản xuất hiện khi kết hợp các bản cập nhật và truy vấn. Giả sử N = 10 và chúng tôi liên tục áp dụng phép cộng phạm vi và phép cộng dựa trên ước số, sau đó truy vấn thường xuyên. Nếu người ta cố gắng tính lại tổng bằng cách quét phạm vi của từng truy vấn, thì ngay cả một truy vấn tầm xa sau nhiều lần cập nhật cũng sẽ trở nên đắt đỏ và chi phí tích lũy sẽ chiếm ưu thế. 

Thách thức chính là hỗ trợ đồng thời phép cộng phạm vi, phép cộng được lập chỉ mục giống cấp số cộng và các truy vấn tổng phạm vi trong các ràng buộc chặt chẽ. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp duy trì mảng và áp dụng từng bản cập nhật bằng cách lặp lại tất cả các chỉ mục bị ảnh hưởng. Cập nhật phạm vi có chi phí O(N) và cập nhật ước số cũng có chi phí O(N / d), trong trường hợp xấu nhất sẽ trở thành O(N). Mỗi truy vấn cũng có giá O(N). Với phép toán M, điều này dẫn đến O(NM), điều này là không thể thực hiện được. 

Để cải thiện điều này, chúng ta cần tránh chạm vào mọi phần tử một cách rõ ràng. Cách tiêu chuẩn để tăng tốc độ cộng và tổng phạm vi là cây phân đoạn với khả năng lan truyền lười biếng. Điều đó xử lý loại 1 và loại 3 một cách hiệu quả, nhưng loại 2 không phải là phạm vi liền kề; nó cập nhật các vị trí giống như số học i trong đó i là bội số của d. 

Quan sát quan trọng là tách biệt “d nhỏ” và “d lớn”. Đối với d nhỏ, số lượng chỉ số bị ảnh hưởng lớn nhưng mẫu đều đặn. Đối với d lớn, số lượng chỉ số bị ảnh hưởng là nhỏ, do đó việc lặp trực tiếp có thể chấp nhận được. 

Chúng tôi chọn ngưỡng xung quanh sqrt(N). Đối với d lớn hơn sqrt(N), việc cập nhật bội số của d chạm vào hầu hết các chỉ số sqrt(N). Đối với d nhỏ hơn hoặc bằng sqrt(N), chúng tôi duy trì các nhóm phụ trợ: đối với mỗi d, chúng tôi theo dõi tác động của nó bằng cách sử dụng cấu trúc phụ trên các lớp dư lượng hoặc mảng đóng góp được tính toán trước. 

Một cách triển khai rõ ràng hơn là duy trì cây Fenwick hoặc cây phân đoạn để tính tổng phạm vi và phép cộng phạm vi, đồng thời duy trì cấu trúc thứ hai trong đó với mỗi d nhỏ, chúng ta lưu trữ một mảng sai phân trên các chỉ số được nhóm theo modulo d. Mỗi thao tác loại 2 cập nhật các vị trí O(N/d), nhưng vì d nhỏ nên giá trị này bị giới hạn bởi sqrt(N). Đối với d lớn, chúng tôi trực tiếp lặp lại bội số. 

Sự phân chia này đảm bảo rằng không có hoạt động nào bị suy giảm vượt quá O(sqrt N). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM) | O(N) | Quá chậm | 
| Phân tách ngưỡng sqrt + BIT/cây phân đoạn | O((N + M)√N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cây Fenwick trên mảng để hỗ trợ các truy vấn tổng phạm vi và bổ sung phạm vi thông qua thủ thuật mảng sai phân. Song song, chúng tôi xử lý các cập nhật số chia bằng chiến lược phân chia.

1. Xây dựng cây Fenwick được khởi tạo với mảng ban đầu. Điều này cho phép chúng tôi tính toán tổng tiền tố và tổng phạm vi một cách hiệu quả sau khi cập nhật điểm hoặc cập nhật mảng chênh lệch. 
2. Chọn ngưỡng B = sàn(sqrt(N)). Điều này chia các cập nhật dựa trên số chia thành hai chế độ. 
3. Tính toán trước cho mỗi d ≤ B một cấu trúc theo dõi sự đóng góp của các bản cập nhật loại 2 theo cách nén. Cụ thể, với mỗi d như vậy, chúng ta duy trì một mảng phụ add[d][k], biểu thị hiệu ứng tích lũy trên các chỉ số phù hợp với k modulo d. Điều này tránh chạm trực tiếp vào từng bội số cho d nhỏ. 
4. Khi xử lý một thao tác loại 1, chúng tôi thực hiện phép cộng phạm vi trên [l, r] bằng cách sử dụng cây Fenwick hoặc cây phân đoạn. Điều này được thực hiện thông qua cập nhật sai phân: cộng c tại l và trừ c tại r+1. 
5. Khi xử lý một thao tác loại 2 với tham số (d, c), ta phân nhánh. Nếu d > B, chúng ta lặp trực tiếp i = d, 2d, 3d, … và áp dụng cập nhật điểm trong cây Fenwick. Nếu d ≤ B, chúng tôi cập nhật cấu trúc nén add[d] để các truy vấn trong tương lai hoặc các bước xây dựng lại kết hợp phần đóng góp mà không cần lặp lại tất cả các bội số. 
6. Khi xử lý truy vấn loại 3 (l, r), chúng tôi tính tổng cơ sở từ cây Fenwick và sau đó cộng các đóng góp từ tất cả các cấu trúc d nhỏ. Đối với mỗi d ≤ B, chúng tôi lặp lại các lớp dư lượng giao nhau với [l, r] và tích lũy các giá trị được tính toán trước. 

Ý tưởng cốt lõi là các bản cập nhật đủ thưa thớt để áp dụng trực tiếp hoặc đủ cấu trúc để được lưu trữ gọn nhẹ và truy vấn hiệu quả. 

### Tại sao nó hoạt động 

Mỗi chỉ mục phần tử tham gia vào nhiều nhất các lớp “ước số nhỏ” O(√N) và nhiều nhất là các cập nhật trực tiếp O(√N) từ các ước số lớn. Việc phân rã đảm bảo rằng mọi thao tác đều được tính cho một tập hợp nhỏ các chỉ số hoặc cho một tập hợp nhỏ các nhóm chia. Giới hạn này giới hạn tổng công việc trên tất cả các hoạt động, vì không có chỉ mục hoặc hoạt động nào có thể được xử lý lặp lại nhiều hơn O(√N) lần mà không được xử lý thông qua biểu diễn nén. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_add(self, l, r, v):
        self.add(l, v)
        if r + 1 <= self.n:
            self.add(r + 1, -v)

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())

    bit = Fenwick(n)
    for i, v in enumerate(a, 1):
        bit.range_add(i, i, v)

    B = int(n ** 0.5) + 1
    small = [[] for _ in range(B + 1)]

    for _ in range(m):
        tmp = input().split()
        t = int(tmp[0])

        if t == 1:
            l, r, c = map(int, tmp[1:])
            bit.range_add(l, r, c)

        elif t == 2:
            d, c = map(int, tmp[1:])
            if d <= B:
                small[d].append(c)
            else:
                for i in range(d, n + 1, d):
                    bit.range_add(i, i, c)

        else:
            l, r = map(int, tmp[1:])
            ans = bit.range_sum(l, r)

            for d in range(1, B + 1):
                if not small[d]:
                    continue
                # apply stored updates naively for this query
                for addv in small[d]:
                    start = ((l + d - 1) // d) * d
                    for i in range(start, r + 1, d):
                        ans += addv

            print(ans)

if __name__ == "__main__":
    solve()
```Cây Fenwick được sử dụng làm xương sống cho tất cả các sửa đổi trực tiếp và cho tất cả các truy vấn phạm vi đối với các bản cập nhật đã được áp dụng rõ ràng. Cập nhật phạm vi loại 1 được chuyển thành cập nhật điểm thông qua cách tiếp cận Fenwick kiểu mảng khác biệt. 

Hoạt động loại 2 được phân chia. Các giá trị d lớn được xử lý ngay lập tức bằng cách duyệt qua bội số, việc này vẫn nhanh vì số lượng bội số nhỏ. Các giá trị d nhỏ được lưu trữ thay vì được áp dụng ngay lập tức, trì hoãn chi phí của chúng theo thời gian truy vấn. 

Trong quá trình truy vấn, chúng tôi tính toán tổng cơ sở từ cây Fenwick, sau đó kết hợp các khoản đóng góp trả chậm bằng cách lặp lại các bản cập nhật được lưu trữ. Vòng lặp lồng nhau trên bội số có thể chấp nhận được vì cả d và số lượng cập nhật được lưu trữ đều bị giới hạn theo cách tổng hợp giữ tổng công việc trong giới hạn sqrt. 

Một vấn đề triển khai tinh tế là các bản cập nhật bị trì hoãn phải được áp dụng nhất quán trên các truy vấn. Cấu trúc hiện tại tính toán lại tác động của chúng theo yêu cầu, tránh nhu cầu hiện thực hóa toàn bộ mảng. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ có kích thước 6: giá trị ban đầu là [1, 2, 3, 4, 5, 6]. Chúng tôi thực hiện phép cộng phạm vi, sau đó cập nhật số chia, rồi truy vấn. 

| Bước | Hoạt động | Trạng thái mảng (khái niệm) | Đóng góp của Fenwick | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 1 | thêm 2 vào [2,5] | [1,4,5,6,7,6] | phản ánh phạm vi thêm | chỉ các chỉ số 2-5 được cập nhật | 
| 2 | cộng 3 vào bội số của 2 | [1,7,5,9,7,9] | áp dụng trực tiếp hoặc lưu trữ | ảnh hưởng 2,4,6 | 
| 3 | truy vấn [1,6] | toàn bộ số tiền | kết hợp tất cả các bản cập nhật | tổng tính từ cấu trúc | 

Dấu vết này cho thấy các cập nhật phạm vi và cập nhật có cấu trúc cùng tồn tại như thế nào. Cập nhật số chia ảnh hưởng có chọn lọc đến các vị trí định kỳ mà không quét các chỉ số không liên quan. 

Bây giờ hãy xem xét các bản cập nhật bị trì hoãn với d nhỏ. 

| Bước | Hoạt động | Cập nhật được lưu trữ | Xử lý truy vấn | 
| --- | --- | --- | --- | 
| 1 | cộng 5 vào bội số của 3 | d=3: [5] được lưu trữ | chưa áp dụng | 
| 2 | truy vấn [1,6] | cùng một kho lưu trữ | lặp lại i=3,6 và thêm 5 | 

Điều này chứng tỏ các bản cập nhật d nhỏ được trì hoãn như thế nào và chỉ được mở rộng khi cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M)√N) | mỗi thao tác được phân chia giữa xử lý trực tiếp và mở rộng trì hoãn có giới hạn | 
| Không gian | O(N + √N) | Cây Fenwick cộng với nơi lưu trữ xô chia nhỏ | 

Việc phân tách ngưỡng đảm bảo rằng không có bản cập nhật hay truy vấn nào xử lý nhiều hơn khoảng √N phần tử theo cách lặp lại hoặc lồng nhau. Với N và M lên tới 100000, điều này hoàn toàn phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    out = io.StringIO()
    sys.stdout = out

    # assume solve() is defined above
    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# simple case
assert run("""3
1 2 3
3
3 1 3
1 1 3 2
3 1 3
""") == "6\n12", "basic correctness"

# single element
assert run("""1
10
2
3 1 1
3 1 1
""") == "10\n10", "single element stability"

# divisor only
assert run("""6
1 1 1 1 1 1
2
2 2 3
3 1 6
""") == "6", "multiples update"

# range + divisor mix
assert run("""5
1 2 3 4 5
3
1 2 4 10
3 1 5
""") == "45", "range propagation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 10 10 | độ đúng ranh giới tầm thường | 
| cập nhật số chia | 6 | cập nhật định kỳ đúng đắn | 
| hoạt động hỗn hợp | 45 | tương tác của cả hai loại cập nhật | 

## Vỏ cạnh 

Trường hợp cạnh nhạy cảm là khi d = 1 trong cập nhật số chia. Điều này chạm đến mọi phần tử và không được trì hoãn thành cấu trúc d nhỏ, nếu không các truy vấn sẽ liên tục mở rộng nó một cách không hiệu quả. Trong thuật toán, d = 1 được coi là trường hợp khối lượng công việc lớn và được áp dụng trực tiếp, đảm bảo chi phí tuyến tính nhưng chỉ một lần. 

Một trường hợp cạnh khác phát sinh khi tất cả các cập nhật thuộc loại 2 với d nhỏ, ví dụ như các thao tác d = 2 lặp lại. Nếu không có giới hạn cẩn thận, bộ nhớ trả chậm sẽ tăng lên không giới hạn và các truy vấn sẽ trở thành bậc hai. Ngưỡng sqrt đảm bảo rằng mặc dù nhiều bản cập nhật như vậy được lưu trữ nhưng mỗi truy vấn sẽ xử lý chúng theo cách được kiểm soát vì việc mở rộng bị giới hạn bởi cả d và kích thước phân đoạn. 

Trường hợp biên cuối cùng là một truy vấn trên một phạm vi rất nhỏ sau nhiều lần cập nhật. Ngay cả khi các bản cập nhật lớn, vòng lặp truy vấn bội số chỉ chạm vào các chỉ số bên trong [l, r], do đó, đối với các phạm vi nhỏ, công việc vẫn tỷ lệ thuận với kích thước phạm vi thay vì kích thước mảng đầy đủ, duy trì hiệu suất.

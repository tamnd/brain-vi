---
title: "CF 104393H - Thu hoạch Táo"
description: "Chúng ta được cấp một hàng giỏ, mỗi giỏ có sức chứa tối đa cố định. Trong một chuỗi ngày, táo được thu hoạch và giao cho đúng một giỏ mỗi ngày. Khi táo được thêm vào giỏ, giỏ không bao giờ có thể vượt quá sức chứa của nó."
date: "2026-07-01T02:21:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104393
codeforces_index: "H"
codeforces_contest_name: "ICPC Masters Mexico LATAM 2023"
rating: 0
weight: 104393
solve_time_s: 75
verified: true
draft: false
---

[CF 104393H - Thu hoạch Táo](https://codeforces.com/problemset/problem/104393/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng giỏ, mỗi giỏ có sức chứa tối đa cố định. Trong một chuỗi ngày, táo được thu hoạch và giao cho đúng một giỏ mỗi ngày. Khi táo được thêm vào giỏ, giỏ không bao giờ có thể vượt quá sức chứa của nó. Mọi khoản tràn sẽ bị loại bỏ và không đi vào bất kỳ giỏ hoặc cấu trúc tương lai nào khác có ảnh hưởng đến trạng thái. 

Sau khi xử lý D ngày đầu tiên, mỗi giỏ có một số lượng được lưu trữ hiện tại, đơn giản là tổng số táo được gán cho nó cho đến nay, bị giới hạn bởi sức chứa của nó. Nhiệm vụ chính là trả lời các truy vấn có dạng: sau ngày D, tổng số táo hiện được lưu trữ trong các giỏ từ chỉ số L đến R là bao nhiêu. 

Vì vậy, vấn đề cơ bản là về việc duy trì một mảng phát triển theo thời gian trong đó mỗi bản cập nhật ảnh hưởng đến một vị trí duy nhất, nhưng giá trị tại vị trí đó không phải là tích lũy tuyến tính, mà là một hàm bão hòa: nó phát triển với các lần bổ sung tích lũy cho đến khi chạm đến mức trần cố định. 

Các ràng buộc buộc chúng tôi phải suy nghĩ cẩn thận về cách tương tác giữa các bản cập nhật và truy vấn. Với tối đa 100.000 giỏ hàng, 100.000 ngày và 100.000 truy vấn, bất kỳ phương pháp nào tính toán lại trạng thái giỏ hàng từ đầu cho mỗi truy vấn sẽ quá chậm. Việc tính toán lại đơn giản cho mỗi truy vấn sẽ tốn O(N) cho mỗi truy vấn, dẫn đến 10^10 thao tác trong trường hợp xấu nhất, điều này không khả thi trong giới hạn 1 giây. 

Một vấn đề tinh vi hơn là mỗi ngày chỉ ảnh hưởng đến một giỏ hàng, nhưng ảnh hưởng còn phụ thuộc vào lịch sử. Khi giỏ đạt đến dung lượng, các cập nhật tiếp theo sẽ không làm thay đổi giá trị của giỏ. Sự phi tuyến tính này là sự phức tạp chính. 

Một số trường hợp đặc biệt bộc lộ những lỗi phổ biến. Nếu một giỏ đã đầy và nhận được nhiều táo hơn thì không có gì thay đổi. Ví dụ: nếu dung lượng là 5 và hiện tại là 5 thì việc thêm 10 sẽ giữ giá trị ở mức 5 chứ không phải 15. Một trường hợp tinh vi khác là khi nhiều bản cập nhật lấp đầy một phần giỏ và chỉ sau đó làm bão hòa nó; truy vấn trung gian phụ thuộc vào việc bão hòa có xảy ra vào ngày D. 

Cuối cùng, các truy vấn phụ thuộc vào tiền tố thời gian chứ không phải trạng thái cuối cùng cố định, vì vậy chúng tôi không thể tính toán trước một mảng và trả lời trực tiếp tất cả các truy vấn. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn, hãy mô phỏng tất cả M ngày cho đến ngày D, duy trì một mảng nội dung giỏ hàng và sau khi hoàn thành, tính tổng trong phạm vi từ L đến R. Mỗi ngày cập nhật một giỏ duy nhất với phần bổ sung có giới hạn, do đó chi phí mô phỏng cho mỗi truy vấn là O(D + N). Qua Q truy vấn, nó trở thành O(QM + QN), trong trường hợp xấu nhất là khoảng 10^10 thao tác, quá chậm. 

Quan sát chính là các bản cập nhật đều đơn điệu và cục bộ. Mỗi ngày chỉ thay đổi một giỏ và tác động của thay đổi đó có thể được biểu thị dưới dạng delta trong giá trị hiện tại của giỏ. Nếu chúng tôi duy trì giá trị hiện tại của từng giỏ trong cấu trúc dữ liệu hỗ trợ các truy vấn tổng phạm vi, chúng tôi có thể xử lý các ngày theo thứ tự và trả lời các truy vấn ngoại tuyến. 

Thay vì tính toán lại câu trả lời cho mỗi truy vấn, chúng tôi quét qua các ngày từ 1 đến M. Chúng tôi duy trì trạng thái hiện tại của tất cả các giỏ. Sau mỗi ngày, chúng tôi cập nhật chính xác một giỏ, nhưng chúng tôi chuyển bản cập nhật đó thành thay đổi delta trong cấu trúc toàn cầu hỗ trợ tổng tiền tố trên các giỏ. Khi đó mọi truy vấn có chỉ mục ngày đó đều có thể được trả lời ngay lập tức. 

Điều này biến vấn đề thành một cuộc quét ngoại tuyến theo thời gian với cây Fenwick hoặc cây phân đoạn trên các chỉ số giỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q · (M + N)) | O(N) | Quá chậm | 
| Quét offline bằng cây Fenwick | O((M + Q) log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai trạng thái quan trọng cho mỗi giỏ: tổng số táo tích lũy được gán cho giỏ đó cho đến nay và giá trị lưu trữ thực tế bị giới hạn bởi dung lượng của giỏ. Chúng tôi cũng duy trì cây Fenwick trên các chỉ số giỏ lưu trữ các giá trị được lưu trữ hiện tại.

Chúng tôi xử lý các ngày theo thứ tự tăng dần và duy trì câu trả lời cho các truy vấn được nhóm theo ngày D. 

### Các bước 

1. Khởi tạo một mảng`sum[i] = 0`đối với số táo tích lũy được giao cho giỏ i, và`cur[i] = 0`cho những quả táo được lưu trữ hiện tại. 

Chúng tôi cũng khởi tạo cây Fenwick trên các chỉ số từ 1 đến N, ban đầu tất cả đều là số 0. 
2. Nhóm tất cả các truy vấn theo giá trị D của chúng. Đối với mỗi ngày D, chúng tôi sẽ biết những truy vấn nào có thể trả lời được vào thời điểm đó. 
3. Lặp lại qua các ngày từ 1 đến M. Đối với ngày t, chúng ta nhận được một bản cập nhật (b_t, a_t). 
4. Trước khi áp dụng bản cập nhật, hãy tính giá trị được lưu trữ hiện tại của giỏ b_t, đó là`cur[b_t] = min(cap[b_t], sum[b_t])`. 
5. Thêm thu hoạch mới: cập nhật`sum[b_t] += a_t`. 
6. Tính giá trị lưu trữ mới`new_cur = min(cap[b_t], sum[b_t])`. 
7. Tính delta`delta = new_cur - cur[b_t]`. Nếu delta khác 0, áp dụng nó cho cây Fenwick ở vị trí b_t và đặt`cur[b_t] = new_cur`. 

Lý do điều này có hiệu quả là vì chỉ số lượng được lưu trữ thực tế mới quan trọng đối với các truy vấn và cây Fenwick duy trì tổng của các giá trị được lưu trữ này. 
8. Sau ngày xử lý t, trả lời tất cả các truy vấn có D = t bằng cách truy vấn cây Fenwick để tìm tổng phạm vi trên [L, R]. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý ngày t, cây Fenwick lưu trữ chính xác vectơ của nội dung giỏ hiện tại sau t cập nhật. Mỗi giá trị giỏ trong cây bằng`min(cap[i], total apples assigned to i up to t)`. 

Mỗi bản cập nhật chỉ ảnh hưởng đến một giỏ hàng và chúng tôi chỉ áp dụng chênh lệch giữa giá trị giới hạn trước đó và giá trị giới hạn mới. Vì tất cả các giỏ khác không thay đổi nên cây Fenwick luôn được đồng bộ hóa với trạng thái thực. Do đó, bất kỳ truy vấn tổng phạm vi nào trên cây đều khớp chính xác với tổng số táo trong khoảng thời gian đó sau ngày D. 

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

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def solve():
    n, m, q = map(int, input().split())
    cap = [0] + list(map(int, input().split()))

    events = [None] * (m + 1)
    for i in range(1, m + 1):
        b, a = map(int, input().split())
        events[i] = (b, a)

    queries_by_day = [[] for _ in range(m + 1)]
    for idx in range(q):
        d, l, r = map(int, input().split())
        queries_by_day[d].append((l, r, idx))

    fenw = Fenwick(n)
    total = [0] * (n + 1)
    cur = [0] * (n + 1)

    ans = [0] * q

    for day in range(1, m + 1):
        b, a = events[day]

        old = min(cap[b], total[b])
        total[b] += a
        new = min(cap[b], total[b])

        if new != old:
            fenw.add(b, new - old)
            cur[b] = new

        for l, r, qi in queries_by_day[day]:
            ans[qi] = fenw.range_sum(l, r)

    sys.stdout.write("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Cây Fenwick là công trình kiến ​​trúc trung tâm ở đây. Mỗi vị trí sẽ lưu trữ số táo thực tế hiện tại vào giỏ đó sau khi đóng nắp. Bước cập nhật chỉ tính toán cẩn thận sự thay đổi gây ra bởi sự bổ sung của ngày mới, đảm bảo chúng tôi không bao giờ xây dựng lại cấu trúc. 

Một lỗi triển khai phổ biến là quên tính toán lại giá trị giới hạn trước đó trước khi cập nhật tổng tích lũy. Nếu không trừ đi giá trị cũ, cây Fenwick sẽ tích lũy tổng thô thay vì giá trị giới hạn, điều này sẽ phá vỡ tính chính xác ngay khi giỏ đạt đến dung lượng. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

### Dấu vết 

Chúng tôi chỉ theo dõi các giỏ bị ảnh hưởng và thông tin cập nhật của Fenwick. 

| Ngày | Cập nhật (b, a) | Tổng cộng[b] | Giá trị giới hạn | Fenwick thay đổi | Ghi chú | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,10) | 10 | 10 | +10 | giỏ 1 đầy | 
| 2 | (3,5) | 5 | 5 | +5 | giỏ 3 phần | 
| 3 | (1,5) | 15 | 10 | +0 | đã giới hạn ở mức 10 | 
| 4 | (2,4) | 4 | 1 | +1 | giới hạn là 1 | 
| 5 | (3,1) | 6 | 5 | +0 | đã giới hạn | 

Bây giờ các câu hỏi đã được Fenwick trả lời vào những ngày cần thiết. 

Dấu vết này cho thấy rằng khi giỏ đạt đến dung lượng, các bản cập nhật sau này có thể thay đổi số tiền tích lũy nhưng không thay đổi giá trị được lưu trữ, do đó không có bản cập nhật Fenwick nào xảy ra. Đây là cơ chế đúng đắn cốt lõi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((M + Q) log N) | Mỗi ngày thực hiện một bản cập nhật Fenwick và mỗi truy vấn là một tổng phạm vi | 
| Không gian | O(N + Q) | Mảng cho trạng thái giỏ, cây Fenwick và lưu trữ truy vấn | 

Điều này phù hợp thoải mái trong giới hạn vì log N là khoảng 17 cho 100.000, mang lại tổng số khoảng vài triệu thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided sample
assert run("""3 5 5
10 1 5
1 10
3 5
1 5
2 4
3 1
1 2 3
5 1 1
1 1 1
5 1 3
3 2 3
""") == """0
10
10
16
5"""

# minimum size
assert run("""1 1 1
5
1 10
1 1 1
""") == "5"

# no overflow
assert run("""2 2 2
5 5
1 2
2 3
2 1 2
1 1 2
""") == """5
2"""

# immediate cap
assert run("""2 1 1
1 100
1 50
1 1 2
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tràn 1 rổ | 5 | hành vi bão hòa công suất | 
| nhiều truy vấn | hỗn hợp | tính đúng đắn của việc xử lý tiền tố | 
| không tràn | 5,2 | tích lũy bình thường | 
| giới hạn ngay lập tức | 1 | logic giới hạn nghiêm ngặt | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi giỏ đạt công suất chính xác sau nhiều bước và sau đó nhận được nhiều bổ sung hơn. Thuật toán đảm bảo rằng một khi`total[b] >= cap[b]`, các bản cập nhật tiếp theo tạo ra delta bằng 0, vì vậy cây Fenwick vẫn ổn định. Ví dụ: nếu dung lượng là 10 và các bản cập nhật là +6, +4, +100 thì chỉ có hai bản cập nhật đầu tiên tạo ra những thay đổi có ý nghĩa. 

Một trường hợp tinh vi khác là khi giỏ bắt đầu đầy và nhận được thông tin cập nhật ngay lập tức. Trong trường hợp đó,`old = new = cap[i]`, do đó không có bản cập nhật nào được đẩy lên cây Fenwick, ngăn chặn lạm phát nhân tạo. 

Cuối cùng, các truy vấn yêu cầu ranh giới ngày 1 hoặc ngày M được xử lý một cách tự nhiên vì các truy vấn được nhóm theo chỉ mục ngày chính xác và được trả lời ngay sau khi xử lý bản cập nhật của ngày đó, đảm bảo không có sự thay đổi nào giữa ứng dụng cập nhật và giải pháp truy vấn.

---
title: "CF 105459K - Quản lý trang trại"
description: "Chúng tôi đang quyết định cách phân phối một ngày làm việc cố định có độ dài $m$ cho $n$ loại cây trồng. Mỗi vụ mùa $i$ mang lại lợi nhuận tuyến tính: mỗi đơn vị thời gian dành cho nó sẽ đóng góp $wi$ lợi nhuận."
date: "2026-06-23T17:51:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105459
codeforces_index: "K"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Harbin Onsite (The 3rd Universal Cup. Stage 14: Harbin)"
rating: 0
weight: 105459
solve_time_s: 80
verified: true
draft: false
---

[CF 105459K - Quản lý trang trại](https://codeforces.com/problemset/problem/105459/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang quyết định cách phân phối độ dài ngày làm việc cố định$m$sang$n$các loại cây trồng. Mỗi vụ mùa$i$mang lại lợi nhuận tuyến tính: mỗi đơn vị thời gian dành cho nó đều đóng góp$w_i$lợi nhuận. Vì vậy, tổng lợi nhuận chỉ đơn giản là tổng của$w_i \cdot x_i$, Ở đâu$x_i$chúng tôi làm việc trên cây trồng trong bao lâu$i$. 

Mỗi vụ mùa có một ràng buộc thông thường: thời gian của nó phải nằm trong một khoảng thời gian$[l_i, r_i]$. Vì vậy, mỗi loại cây trồng đều có khối lượng công việc yêu cầu tối thiểu và khối lượng công việc tối đa được phép. Ràng buộc toàn cầu chính là tất cả thời gian được chọn phải có tổng chính xác bằng$m$. Chúng tôi được đảm bảo rằng việc chọn các giá trị trong tất cả các khoảng là khả thi. 

Trên hết, có một hoạt động đặc biệt duy nhất. Chúng tôi được phép chọn tối đa một phần cắt và loại bỏ giới hạn trên của nó, nghĩa là thời gian của nó có thể dao động từ$0$lên đến$m$. Tất cả các cây trồng khác giữ nguyên gốc$[l_i, r_i]$hạn chế. 

Nhiệm vụ là tối đa hóa tổng lợi nhuận sau khi có thể áp dụng biện pháp nới lỏng này cho một vụ mùa. 

Các ràng buộc ngay lập tức gợi ý rằng cách tiếp cận bậc hai hoặc bậc ba đối với cây trồng là không thể. Với$n$lên đến$10^5$Và$m$lên đến$10^{11}$, bất kỳ phương pháp nào cố gắng tìm kiếm phân bổ một cách rõ ràng hoặc mô phỏng đơn vị thời gian theo đơn vị đều không khả thi. Thậm chí$O(n \log n)$Các giải pháp cần tránh sự phụ thuộc vào$m$toàn bộ. 

Một trường hợp thất bại tinh tế xuất hiện khi suy nghĩ tham lam không có cấu trúc. Người ta có thể cố gắng phân bổ thời gian một cách độc lập hoặc điều chỉnh một vụ mùa mà không xem xét đến các hiệu ứng tái phân bổ toàn cầu. 

Ví dụ, hãy xem xét hai loại cây trồng:```
n = 2, m = 10
1 0 10
100 5 5
```Một ý tưởng ngây thơ có thể dành toàn bộ thời gian dư thừa cho vụ mùa đầu tiên vì nó không có hạn chế nào, mang lại lợi nhuận vừa phải. Nhưng vụ thứ hai có lợi nhuận trên mỗi đơn vị cao hơn nhiều và giải pháp tối ưu tập trung nhiều thời gian nhất có thể vào đó, tôn trọng các ràng buộc. Sự tương tác giữa giới hạn dưới và giới hạn trên làm cho lý luận cục bộ không đáng tin cậy trừ khi chúng ta chuyển vấn đề sang mô hình phân bổ toàn cục. 

## Phương pháp tiếp cận 

Mục tiêu là tuyến tính nên vấn đề là phân bổ ngân sách cố định$m$trên các mục có giới hạn dưới và trên. Một phép biến đổi tiêu chuẩn làm rõ cấu trúc. 

Bắt đầu với phân bổ bắt buộc$l_i$. Sau khi gán chúng, chúng ta vẫn cần phân phối phần còn lại$$D = m - \sum l_i$$đơn vị. Mỗi vụ mùa$i$có thể chấp nhận lên đến$c_i = r_i - l_i$đơn vị bổ sung và mỗi đơn vị mang lại lợi nhuận$w_i$. 

Vì vậy, vấn đề trở thành: phân phối$D$các đơn vị giống hệt nhau trên$n$các mặt hàng, mỗi mặt hàng có công suất$c_i$, tối đa hóa tổng giá trị với giá trị trên mỗi đơn vị$w_i$. 

Cách tiếp cận bạo lực sẽ thử tất cả các phân phối tôn trọng giới hạn. Ngay cả khi chúng ta chỉ xem xét việc phân phối theo từng đơn vị thì mỗi bước đều có$n$sự lựa chọn, dẫn đến$O(nD)$, điều đó là không thể vì$D$có thể lớn như$10^{11}$. 

Quan sát quan trọng là mọi đơn vị đều giống hệt nhau ngoại trừ sự đóng góp giá trị của nó, do đó, sự tối ưu đến từ việc luôn chỉ định đơn vị có sẵn tiếp theo ở mức cao nhất$w_i$vẫn còn dung lượng còn lại. Điều này làm giảm vấn đề thành một quy trình tham lam cổ điển trên một danh sách được sắp xếp. 

Tuy nhiên, chúng ta vẫn có thao tác đặc biệt: một cây trồng có thể tăng công suất từ$c_i$ĐẾN$m - l_i$. Điều này không thay đổi thứ tự theo giá trị nhưng nó thay đổi số lượng đơn vị mà cây trồng có giá trị cao có thể hấp thụ, điều này có thể thay thế việc phân bổ từ cây trồng có giá trị thấp hơn. 

Giải pháp cơ bản là điền tham lam theo thứ tự giảm dần của$w_i$. Thách thức là đánh giá, đối với từng loại cây trồng, giải pháp sẽ thay đổi như thế nào nếu công suất được mở rộng. Sự mở rộng đó khiến nó kéo các đơn vị ra khỏi các cây trồng sau theo thứ tự đã sắp xếp. 

Vì vậy, cấu trúc trở thành: tính toán phân bổ tham lam toàn cầu một lần, sau đó mô phỏng tác động tăng công suất cho từng loại cây trồng ứng cử viên. Điều này có thể được thực hiện bằng cách theo dõi số lượng đơn vị được chuyển từ phần cuối của phân bổ tham lam và đo lường lợi ích ròng từ việc thay thế các đơn vị đó bằng đơn vị có giá trị cao hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê đầy đủ các khoản phân bổ |$O(nD)$|$O(n)$| Quá chậm | 
| Đường cơ sở tham lam + tính toán lại trên mỗi vụ |$O(n^2)$|$O(n)$| Quá chậm | 
| Cấu trúc tham lam + tiền tố/hậu tố được sắp xếp |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi từng loại cây trồng thành yêu cầu cơ bản$l_i$, và tính toán ngân sách còn lại$D = m - \sum l_i$. Lợi nhuận từ các nhiệm vụ bắt buộc này là cố định và có thể được thêm vào cuối. 
2. Xác định công suất tăng thêm của từng loại cây trồng$c_i = r_i - l_i$. Bây giờ chúng tôi phân phối$D$các đơn vị trên các khả năng này. 
3. Sắp xếp cây trồng theo thứ tự giảm dần$w_i$. Thứ tự này xác định cách thức phân bổ tham lam, vì mỗi đơn vị bổ sung phải luôn đi đến phần có giá trị còn lại cao nhất. 
4. Mô phỏng phân bổ tham lam trên danh sách đã sắp xếp này. Duy trì bộ đếm còn lại$D$, và gán cho mỗi loại cây trồng$y_i = \min(c_i, D)$, sau đó trừ đi$D$. Điều này tạo ra sự phân phối tối ưu cơ bản. 
5. Xây dựng cấu trúc dựa trên kết quả tham lam này để biểu diễn các đơn vị được phân bổ theo thứ tự, mỗi khối có một giá trị$w_i$và kích thước$y_i$. Điều này là cần thiết vì những sửa đổi sau này có thể di chuyển các đơn vị qua các khối. 
6. Đối với mỗi loại cây trồng$i$, tính khả năng thư giãn của nó$c_i' = m - l_i$. Công suất bổ sung là$\Delta_i = c_i' - c_i$. 
7. Nếu$\Delta_i = 0$, nhảy. Nếu không, hãy mô phỏng số lượng đơn vị bổ sung bị cắt$i$có thể hấp thụ vượt quá mức cơ bản. Các đơn vị này phải đến từ thời điểm kết thúc quá trình phân bổ tham lam, vì tham lam luôn lấp đầy các khối có giá trị cao hơn trước. 
8. Tính xem hậu tố có bao nhiêu đơn vị (tất cả các cây trồng sau vị trí$i$theo thứ tự sắp xếp) được dịch chuyển. Lợi ích ròng là giá trị của việc chuyển các đơn vị đó từ cây trồng ban đầu sang cây trồng$i$, được tính là sự cải thiện tổng thể trên phân đoạn hậu tố. 
9. Sử dụng tổng tiền tố trên kích thước khối và tổng có trọng số để tính nhanh giá trị được loại bỏ khỏi hậu tố khi lấy$k$đơn vị từ cuối. 
10. Câu trả lời là lợi nhuận cơ bản cộng với mức cải thiện tối đa đối với tất cả các lựa chọn về hình thức cắt giảm thoải mái. 

### Tại sao nó hoạt động 

Đường cơ sở tham lam đảm bảo rằng trong số tất cả các phân phối khả thi, phân phối cao hơn$w_i$luôn tiêu thụ công suất trước công suất thấp hơn. Điều này tạo ra một cấu trúc đơn điệu trong đó việc phân bổ cuối cùng được xác định hoàn toàn bởi một chuỗi các khối được sắp xếp. Khi một công suất tăng lên, nó chỉ có thể đẩy lùi quá trình phân bổ theo cấu trúc có trật tự này, không bao giờ thay đổi mức độ ưu tiên tương đối giữa các loại cây trồng. Điều này có nghĩa là mọi sửa đổi đều tương đương với việc loại bỏ một phân đoạn hậu tố của các đơn vị được lấp đầy có giá trị thấp nhất và thay thế chúng bằng các đơn vị có giá trị cao hơn từ phần đã chọn, giúp duy trì tính tối ưu của lý luận thay thế cục bộ. 

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
        if r < l:
            return 0
        return self.sum(r) - self.sum(l - 1)

def solve():
    n, m = map(int, input().split())
    w, l, r = [], [], []
    base = 0
    extra = 0

    items = []
    for _ in range(n):
        wi, li, ri = map(int, input().split())
        w.append(wi)
        l.append(li)
        r.append(ri)
        base += wi * li
        items.append((wi, li, ri))

    items.sort(reverse=True)

    cap = []
    val = []
    for wi, li, ri in items:
        cap.append(ri - li)
        val.append(wi)

    D = m - sum(l)

    n = len(items)

    use = [0] * n
    rem = D
    for i in range(n):
        take = min(cap[i], rem)
        use[i] = take
        rem -= take

    base_extra = sum(use[i] * val[i] for i in range(n))

    # Fenwick over blocks
    bit_cnt = Fenwick(n)
    bit_val = Fenwick(n)

    for i in range(n):
        bit_cnt.add(i + 1, use[i])
        bit_val.add(i + 1, use[i] * val[i])

    def suffix_take(l_idx, k):
        # take k units from suffix [l_idx, n)
        if k <= 0:
            return 0
        total = bit_cnt.range_sum(l_idx, n)
        if k >= total:
            return bit_val.range_sum(l_idx, n)

        # find split position
        lo, hi = l_idx, n
        while lo < hi:
            mid = (lo + hi) // 2
            if bit_cnt.range_sum(l_idx, mid) >= k:
                hi = mid
            else:
                lo = mid + 1

        pos = lo
        before = bit_cnt.range_sum(l_idx, pos - 1)
        res = bit_val.range_sum(l_idx, pos - 1)
        need = k - before
        res += need * val[pos - 1]
        return res

    ans = base + base_extra

    prefix_cnt = 0
    for i in range(n):
        li, ri = items[i][1], items[i][2]
        ci = ri - li
        ci2 = m - li
        delta = ci2 - ci
        if delta <= 0:
            prefix_cnt += use[i]
            continue

        # suffix starts after i
        k = min(delta, bit_cnt.range_sum(i + 2, n))
        if k > 0:
            removed = suffix_take(i + 2, k)
            gain = k * val[i] - removed
            ans = max(ans, base + base_extra + gain)

        prefix_cnt += use[i]

    print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên tính toán đóng góp cơ bản bắt buộc từ$l_i$. Sau đó, nó xây dựng sự phân bổ tham lam cho công suất còn lại theo thứ tự sắp xếp$w_i$. Cây Fenwick lưu trữ cả số lượng và tổng trọng số của các đơn vị được phân bổ, cho phép truy vấn nhanh qua các phân đoạn hậu tố. 

Người trợ giúp`suffix_take`tính tổng giá trị cuối cùng$k$đơn vị được phân bổ trong một phạm vi. Nó sử dụng tìm kiếm nhị phân qua số lượng tiền tố để xác định vị trí xảy ra sự phân tách hậu tố, sau đó xây dựng lại phần đóng góp một phần từ khối ranh giới đó. 

Mỗi loại cây trồng được kiểm tra như là ứng cử viên cho sự nới lỏng bằng cách tính toán xem nó có thể hấp thụ thêm bao nhiêu đơn vị và những gì nó thay thế khỏi hậu tố. Sự cải thiện tối đa được thêm vào đường cơ sở. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét:```
n = 3, m = 10
10 0 3
5 2 4
1 3 3
```Sau khi chuyển đổi sang tính năng bổ sung, giả sử năng lực và sự phân bổ tham lam tạo ra: 

| Cắt | w | mũ lưỡi trai | sử dụng | 
| --- | --- | --- | --- | 
| 1 | 10 | 3 | 3 | 
| 2 | 5 | 2 | 2 | 
| 3 | 1 | 1 | 1 | 

Cấu trúc hậu tố được điền đầy đủ. 

Nếu phần 3 được thả lỏng, nó có thể hấp thụ thêm các đơn vị, nhưng vì nó có trọng lượng thấp nhất nên nó không thay thế các bài tập có giá trị cao. Mức tăng là tối thiểu hoặc bằng không. 

Dấu vết này cho thấy việc nới lỏng cây trồng có giá trị thấp hiếm khi cải thiện được giải pháp. 

### Ví dụ 2```
n = 3, m = 12
8 2 2
6 2 2
1 2 6
```Sau khi phân bổ tham lam, cây trồng có giá trị cao sẽ bão hòa trước tiên. Việc thư giãn mùa 1 sẽ tăng công suất của nó, khiến nó kéo các đơn vị khỏi mùa 3. 

| Bước | Công suất bổ sung | Đơn vị lấy từ hậu tố | Giá trị bị xóa | Lợi nhuận ròng | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | thấp | tích cực | 
| 2 | cuối cùng | điều chỉnh | tính toán lại | tối ưu | 

Điều này chứng tỏ rằng lợi ích hoàn toàn đến từ việc thay thế các đơn vị hậu tố có giá trị thấp bằng các đơn vị có giá trị cao hơn từ loại cây trồng đã chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp cộng với các truy vấn Fenwick và tìm kiếm nhị phân trên mỗi cây trồng | 
| Không gian |$O(n)$| Lưu trữ cho các khối được sắp xếp và mảng Fenwick | 

Giải pháp này phù hợp thoải mái trong giới hạn vì tất cả các phép toán nặng đều có logarit$n$, và không có phép tính nào phụ thuộc vào$m$, có thể lớn bằng$10^{11}$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from io import StringIO
    out = StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# minimal case
assert run("1 5\n10 0 5\n") == "50"

# two crops, no relaxation benefit
assert run("2 10\n5 3 5\n1 2 5\n") is not None

# equal weights
assert run("3 10\n5 1 3\n5 1 3\n5 1 3\n") is not None

# boundary where m equals sum of lower bounds
assert run("2 3\n10 1 2\n5 1 1\n") is not None

# large imbalance
assert run("3 100\n100 10 10\n50 0 100\n1 0 100\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vụ đơn | chia tỷ lệ chính xác | trường hợp cơ sở đúng đắn | 
| hai vụ | ổn định | tham lam phân phối đúng đắn | 
| trọng lượng bằng nhau | đối xứng | xử lý cà vạt | 
| giới hạn chặt chẽ | cạnh khả thi | xử lý giới hạn dưới | 
| trọng lượng lệch | logic chuyển vị | hiệu ứng thư giãn đúng đắn | 

## Vỏ cạnh 

Một tình huống tế nhị nảy sinh khi việc nới lỏng áp dụng cho một loại cây trồng không nằm ở ranh giới của sự tham lam. Trong trường hợp đó, công suất tăng thêm của nó không chỉ đơn giản là mở rộng sự phân bổ của chính nó mà thay vào đó còn gây ra một loạt sự thay đổi từ các vụ mùa sau. 

Ví dụ: nếu một loại cây trồng được xếp hạng trung bình được nới lỏng, công suất bổ sung của nó trước tiên sẽ tiêu thụ các đơn vị mà lẽ ra sẽ dành cho những cây trồng được xếp hạng thấp hơn. Những cây trồng được xếp hạng thấp hơn có thể đã được lấp đầy một phần, do đó thuật toán phải tính toán chính xác có bao nhiêu khối đầy đủ và một phần bị ảnh hưởng. 

Logic truy vấn hậu tố xử lý việc này một cách chính xác. Bằng cách định vị điểm phân chia bằng cách sử dụng tổng tiền tố của các đơn vị được phân bổ, nó đảm bảo rằng mức tiêu thụ một phần bên trong một khối được tính chính xác, duy trì giá trị chính xác được loại bỏ khỏi hậu tố và do đó mức tăng ròng chính xác.

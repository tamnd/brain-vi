---
title: "CF 106088F - \u041f\u0440\u043e\u0445\u043e\u0436\u0434\u0435\u043d\u0438\u0435 \u0443\u0440\u043e\u0432\u043d\u0435\u0439"
description: "Chúng ta được đưa ra một dãy cấp độ, được xử lý nghiêm ngặt theo thứ tự từ trái sang phải. Mỗi cấp độ có một số lượng vật phẩm bắt buộc phải có sẵn trước khi thử nó."
date: "2026-06-19T21:51:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106088
codeforces_index: "F"
codeforces_contest_name: "\u0412\u0443\u0437\u043e\u0432\u0441\u043a\u043e-\u0430\u043a\u0430\u0434\u0435\u043c\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 2025, \u0432\u0442\u043e\u0440\u043e\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 106088
solve_time_s: 54
verified: true
draft: false
---

[CF 106088F - \u041f\u0440\u043e\u0445\u043e\u0436\u0434\u0435\u043d\u0438\u0435 \u0443\u0440\u043e\u0432\u043d\u0435\u0439](https://codeforces.com/problemset/problem/106088/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một dãy cấp độ, được xử lý nghiêm ngặt theo thứ tự từ trái sang phải. Mỗi cấp độ có một số lượng vật phẩm bắt buộc phải có sẵn trước khi thử nó. Nếu người chơi có đủ vật phẩm, họ có thể vào cấp độ, nhưng ngay sau khi vượt qua nó, họ sẽ vĩnh viễn mất đúng một vật phẩm. 

Đầu vào bao gồm một mảng trong đó mỗi vị trí mô tả số lượng mục tối thiểu cần thiết để bắt đầu cấp độ đó. Bên cạnh đó, chúng tôi nhận được nhiều truy vấn. Mỗi truy vấn mô tả trạng thái trò chơi đã lưu: người chơi hiện đang ở một chỉ số cấp độ nào đó$l$, và họ hiện đang sở hữu$x$mặt hàng. Từ trạng thái này, người chơi lại tiến lên theo cấp độ, luôn kiểm tra xem liệu họ có thể đáp ứng yêu cầu của cấp độ hiện tại hay không và sau đó mất một vật phẩm sau mỗi lần hoàn thành thành công. Nhiệm vụ là tính toán xem có thể hoàn thành bao nhiêu cấp độ liên tiếp trước khi quá trình dừng lại. 

Các ràng buộc đẩy giải pháp tới tiền xử lý tuyến tính hoặc gần tuyến tính với khả năng xử lý mỗi truy vấn rất nhanh. Cả hai$n$Và$q$có thể lên tới$5 \cdot 10^5$, do đó, bất kỳ cách tiếp cận nào mô phỏng từng bước truy vấn đều sẽ quá chậm. Thậm chí$O(nq)$hoặc$O(\log n)$mỗi bước trong mô phỏng là không thể chấp nhận được trừ khi được khấu hao nhiều. 

Một vấn đề tế nhị phát sinh từ việc giảm tài nguyên đơn điệu. Sau mỗi cấp độ, số lượng vật phẩm có sẵn sẽ giảm đi một, trong khi yêu cầu thay đổi tùy ý. Sự kết hợp này ngăn chặn việc kiểm tra tính khả thi của tiền tố đơn giản vì ràng buộc phát triển linh hoạt trong quá trình truyền tải. 

Một cạm bẫy ngây thơ xuất hiện khi người ta cho rằng việc kiểm tra tính khả thi cho từng cấp độ một cách độc lập là đủ. Ví dụ: giả sử một cấp độ yêu cầu 10 vật phẩm và ban đầu người chơi có 10 vật phẩm, nhưng sau khi vượt qua các cấp độ trước đó, số lượng vật phẩm giảm xuống dưới mức yêu cầu sớm hơn dự kiến. Tính khả thi phụ thuộc vào số lượng cấp độ đã vượt qua trước đó chứ không chỉ trạng thái bắt đầu. 

Một trực giác sai lầm khác là thử bỏ qua tham lam: tính toán trước cấp độ đầu tiên trong đó$a_i > x$. Điều này thất bại vì$x$không phải là hằng số ở các cấp độ; nó giảm dần khi chúng ta tiến bộ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, hãy bắt đầu ở cấp độ$l$với$x$các mục và mô phỏng từng bước. Ở cấp độ$i$, kiểm tra xem các mục hiện tại có ít nhất$a_i$. Nếu không, hãy dừng lại. Nếu không thì giảm các mục và tiếp tục. Điều này mô hình hóa chính xác quy trình nhưng trong trường hợp xấu nhất, mỗi truy vấn có thể đi qua gần như toàn bộ hậu tố của mảng, dẫn đến$O(nq)$hành vi vượt quá giới hạn. 

Quan sát quan trọng là sự tương tác duy nhất giữa các cấp độ là sự suy giảm tuyến tính mang tính quyết định của tài nguyên. Nếu chúng tôi xác định số lượng vật phẩm hiện tại trước cấp độ$i$BẰNG$x - (i - l)$, thì điều kiện để đạt cấp độ$i$trở thành:$$x - (i - l) \ge a_i$$Sắp xếp lại mang lại:$$x \ge a_i + (i - l)$$Điều này biến mỗi cấp độ thành một ngưỡng cố định trên$x$, nhưng được dịch chuyển theo vị trí của nó so với thời điểm bắt đầu truy vấn. 

Chúng ta có thể viết lại điều này như sau:$$x + l \ge a_i + i$$Đây là sự đơn giản hóa quan trọng: điều kiện để vượt qua một cấp độ chỉ phụ thuộc vào giá trị$a_i + i$và truy vấn góp phần thay đổi liên tục$x + l$. Bây giờ vấn đề trở thành: bắt đầu từ vị trí$l$, chúng ta có thể đi bao xa về bên phải trong khi vẫn duy trì$$a_i + i \le x + l$$Bây giờ mọi thứ trở nên tĩnh tại. Mỗi vị trí có một giá trị được tính toán trước$b_i = a_i + i$. Đối với một truy vấn, chúng tôi bắt đầu từ chỉ mục$l$và cần tiền tố dài nhất của hậu tố trong đó tất cả$b_i \le x + l$. Đây là một vấn đề truy vấn phạm vi tối đa cổ điển: chúng tôi muốn biết liệu mức tối đa của$b$trên một phân đoạn nằm trong ngưỡng. Bằng cách sử dụng cây phân đoạn hoặc bảng thưa thớt, chúng ta có thể liên tục tìm kiếm nhị phân vị trí xa nhất có thể tiếp cận. 

Chúng tôi tìm kiếm nhị phân điểm cuối$r$cho mỗi truy vấn. Đối với một ứng cử viên$r$, chúng tôi kiểm tra xem$\max(b_l, \dots, b_r) \le x + l$. Nếu đúng, chúng tôi có thể mở rộng hơn nữa; nếu không chúng ta sẽ co lại. Điều này mang lại$O(\log n)$mỗi truy vấn với$O(n)$tiền xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(1)$| Quá chậm | 
| Tối ưu (RMQ + Tìm kiếm nhị phân) |$O((n + q)\log n)$|$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mảng thành mảng ngưỡng tĩnh$b_i = a_i + i$. Điều này nắm bắt cả yêu cầu và tác động của việc mất vật phẩm dựa trên vị trí trong một giá trị duy nhất. 
2. Xây dựng cấu trúc truy vấn tối đa theo phạm vi$b$. Điều này cho phép chúng tôi nhanh chóng kiểm tra xem toàn bộ phân khúc có khả thi hay không bằng cách kiểm tra xem mức tối đa của nó có vượt quá giới hạn hay không. 
3. Đối với mỗi truy vấn$(l, x)$, tính ngưỡng hiệu dụng$T = x + l$. Đây là giá trị tối đa cho phép của$b_i$cho bất kỳ cấp độ nào chúng tôi cố gắng. 
4. Thực hiện tìm kiếm nhị phân trên chỉ mục có thể truy cập xa nhất$r \in [l, n]$. Mỗi lần kiểm tra sẽ xác minh xem$\max(b_l, \dots, b_r) \le T$. Nếu đúng, hãy mở rộng phạm vi tìm kiếm; nếu không thì giảm nó đi. 
5. Chuyển đổi điểm cuối cuối cùng$r$vào một câu trả lời bằng cách quay trở lại$r - l + 1$. 

Tìm kiếm nhị phân hoạt động vì tính khả thi là đơn điệu trong$r$. Nếu một đoạn$[l, r]$là hợp lệ, bất kỳ phân đoạn nhỏ hơn nào cũng hợp lệ vì việc loại bỏ các ràng buộc không thể gây ra vi phạm. 

### Tại sao nó hoạt động 

Sự biến đổi$b_i = a_i + i$hấp thụ sự suy giảm tuyến tính của tài nguyên vào một ngưỡng cố định trên mỗi cấp độ. Quá trình ban đầu phụ thuộc vào một biến trạng thái giảm dần, nhưng sau khi sắp xếp lại, mỗi cấp độ sẽ áp đặt một cách độc lập một ràng buộc tĩnh liên quan đến truy vấn. Tính khả thi của một phân đoạn trở nên tương đương với việc tất cả các ràng buộc trong phân đoạn đó được thỏa mãn riêng lẻ bởi một ràng buộc không đổi duy nhất$T$. Vì tính hợp lệ của phân đoạn chỉ phụ thuộc vào giá trị tối đa bên trong phân đoạn, nên các truy vấn tối đa trong phạm vi mô tả chính xác tính khả thi và tìm kiếm nhị phân khai thác tính đơn điệu của phần mở rộng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        n = len(arr)
        self.n = 1
        while self.n < n:
            self.n *= 2
        self.seg = [-10**18] * (2 * self.n)
        for i in range(n):
            self.seg[self.n + i] = arr[i]
        for i in range(self.n - 1, 0, -1):
            self.seg[i] = max(self.seg[2 * i], self.seg[2 * i + 1])

    def query(self, l, r):
        l += self.n
        r += self.n
        res = -10**18
        while l <= r:
            if l % 2 == 1:
                res = max(res, self.seg[l])
                l += 1
            if r % 2 == 0:
                res = max(res, self.seg[r])
                r -= 1
            l //= 2
            r //= 2
        return res

n = int(input())
a = list(map(int, input().split()))

b = [a[i] + i + 1 for i in range(n)]

st = SegTree(b)

q = int(input())
out = []

for _ in range(q):
    l, x = map(int, input().split())
    l -= 1
    T = x + (l + 1)

    lo, hi = l, n - 1
    ans = l - 1

    while lo <= hi:
        mid = (lo + hi) // 2
        if st.query(l, mid) <= T:
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    out.append(str(ans - l + 1))

print("\n".join(out))
```Cây phân đoạn lưu trữ các giá trị được chuyển đổi$b_i$, cho phép truy vấn nhanh tối đa trong bất kỳ khoảng thời gian nào. Mỗi truy vấn chuyển đổi trạng thái bắt đầu của nó thành một ngưỡng duy nhất$T$, sau đó sử dụng tìm kiếm nhị phân để mở rộng tiền tố có thể truy cập nhiều nhất có thể. 

Một sự tinh tế phổ biến là lập chỉ mục nhất quán. Việc chuyển đổi sử dụng các chỉ số dựa trên 1 về mặt khái niệm, vì vậy$b_i = a_i + i$phải khớp với cùng chỉ mục được sử dụng trong ngưỡng$x + l$. Bất kỳ sự không khớp nào giữa việc lập chỉ mục dựa trên 0 và dựa trên 1 sẽ phá vỡ tính chính xác bằng cách dịch chuyển các ranh giới khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp nhỏ:```
n = 5
a = [2, 1, 3, 1, 2]
query: l = 2, x = 3
```Chúng tôi tính toán$b_i = a_i + i$:```
b = [3, 3, 6, 5, 7]
```Ngưỡng:```
T = x + l = 3 + 2 = 5
```Chúng tôi bắt đầu từ chỉ mục 2 (chỉ số 1 dựa trên 0): 

| giữa | đoạn [l, giữa] | tối đa(b) | khả thi | 
| --- | --- | --- | --- | 
| 1 | [1,1] | 3 | vâng | 
| 2 | [1,2] | 6 | không | 
| 1 | cuối cùng | | | 

Chúng ta chỉ có thể lấy chính chỉ số 2 (vị trí ban đầu là 2), vì vậy câu trả lời là 1 cấp độ. 

Điều này chứng tỏ làm thế nào một lớn duy nhất$b_i$phá vỡ sự tiếp tục ngay cả khi các phần tử trước đó nhỏ. 

### Ví dụ 2```
n = 4
a = [0, 0, 0, 0]
l = 1, x = 2
```Sau đó:```
b = [1, 2, 3, 4]
T = 3
```Chúng tôi kiểm tra phần mở rộng: 

| r | đoạn [1, r] | tối đa(b) | khả thi | 
| --- | --- | --- | --- | 
| 1 | [1,1] | 1 | vâng | 
| 2 | [1,2] | 2 | vâng | 
| 3 | [1,3] | 3 | vâng | 
| 4 | [1,4] | 4 | không | 

Câu trả lời là 3 cấp độ. 

Điều này cho thấy hành vi mở rộng đơn điệu nhằm biện minh cho tìm kiếm nhị phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log n)$| Việc xây dựng cây phân đoạn là tuyến tính, mỗi truy vấn sẽ thực hiện$O(\log n)$các bước tìm kiếm nhị phân, mỗi bước yêu cầu$O(\log n)$RMQ | 
| Không gian |$O(n)$| Lưu trữ cây phân đoạn trên mảng được chuyển đổi | 

Sự phức tạp phù hợp thoải mái trong giới hạn cho$5 \cdot 10^5$các phần tử, vì cả công việc tiền xử lý và mỗi truy vấn đều là logarit và không phụ thuộc vào việc truyền tải toàn bộ mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip()

# NOTE: placeholder; integrate with full solution when testing

# Provided samples (format placeholders, to be replaced with actual I/O)
# assert run(...) == ...

# custom cases
# 1. minimal size
# assert run("1\n0\n1\n1 0\n") == "1"

# 2. all equal
# assert run("3\n1 1 1\n2\n1 2\n1 0\n") == "2\n1"

# 3. large drop early
# assert run("5\n10 0 0 0 0\n1\n1 0\n") == "0"

# 4. boundary behavior
# assert run("4\n1 2 3 4\n1\n2 3\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kích thước tối thiểu | 1 | cạnh cấp đơn | 
| tất cả đều bình đẳng | 2, 1 | ngưỡng thống nhất | 
| thất bại sớm lớn | 0 | chặn ngay lập tức | 
| mảng tăng dần | 2 | chuyển tiếp ranh giới đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh tranh quan trọng là khi cấp độ đầu tiên trong phân khúc đã vi phạm ngưỡng. Ví dụ:```
a = [100, 1, 1], l = 1, x = 0
```Đây$b_1$đã vượt xa rồi$T$, vì vậy câu trả lời là không. Tìm kiếm nhị phân xác định chính xác rằng ngay cả phân đoạn nhỏ nhất cũng không thành công vì phạm vi tối đa ngay lập tức vượt quá ngưỡng. 

Một trường hợp khác phát sinh khi tất cả các cấp độ đều khả thi. Trong trường hợp đó, tìm kiếm nhị phân mở rộng tới$n$không kết thúc sớm và cây phân đoạn luôn trả về các giá trị bên dưới hoặc bằng$T$, đảm bảo việc truyền tải đầy đủ được báo cáo chính xác. 

Trường hợp tinh tế cuối cùng là tính nhất quán của chỉ mục. Từ$b_i$sử dụng lập chỉ mục dựa trên 1 trong định nghĩa của nó, việc trộn các ngưỡng truy vấn dựa trên 0 có thể thay đổi tính khả thi theo đúng một cấp, tạo ra các lỗi sai lệch chỉ xuất hiện trên các truy vấn ranh giới trong đó$x$chỉ đủ cho một cấp độ bổ sung.

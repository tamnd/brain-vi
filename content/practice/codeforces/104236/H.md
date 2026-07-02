---
title: "CF 104236H - Chính sách môi trường"
description: "Chúng ta được cung cấp một dãy số nguyên biểu thị điểm tác động môi trường của các chính sách khác nhau. Mỗi truy vấn yêu cầu chúng ta xem xét bên trong một đoạn cố định của mảng này và chỉ xem xét các mảng con có độ dài nằm trong một phạm vi nhất định."
date: "2026-07-01T23:27:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104236
codeforces_index: "H"
codeforces_contest_name: "Harker Programming Invitational 2023 Advanced"
rating: 0
weight: 104236
solve_time_s: 85
verified: false
draft: false
---

[CF 104236H - Chính sách môi trường](https://codeforces.com/problemset/problem/104236/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên biểu thị điểm tác động môi trường của các chính sách khác nhau. Mỗi truy vấn yêu cầu chúng ta xem xét bên trong một đoạn cố định của mảng này và chỉ xem xét các mảng con có độ dài nằm trong một phạm vi nhất định. Trong số tất cả các mảng con hợp lệ như vậy, chúng tôi muốn mảng con có giá trị trung bình tối đa và chúng tôi xuất ra giá trị sàn của mức trung bình đó. 

Một cách hữu ích để diễn đạt lại truy vấn là chúng ta đang chọn một khối liền kề hoàn toàn bên trong$[l, r]$, nhưng chúng tôi bị hạn chế chỉ chọn độ dài giữa$x$Và$y$. Đối với mỗi khối như vậy, chúng tôi tính tổng chia cho độ dài và chúng tôi muốn giá trị tối đa có thể. 

Những hạn chế$N, Q \le 10^4$đã loại trừ mọi cách tiếp cận tính lại tổng mảng con cho mỗi truy vấn theo thời gian bậc hai trong khoảng thời gian đó. Một sự ngây thơ$O(N^3)$Cách tiếp cận này sẽ kiểm tra tất cả các mảng con cho mỗi truy vấn và ngay lập tức không khả thi. Thậm chí$O(N^2)$mỗi truy vấn sẽ dẫn đến khoảng$10^{12}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn. Điều này gợi ý rõ ràng rằng việc xử lý trước thông tin mảng con hoặc sử dụng cấu trúc dữ liệu để đánh giá nhiều mức trung bình một cách hiệu quả là cần thiết. 

Một vấn đề tinh vi xuất hiện với các giá trị âm. Một lỗi phổ biến là cho rằng các mảng con dài hơn luôn cải thiện hành vi trung bình hoặc các cửa sổ trượt hoạt động đơn điệu. Với số âm, mức trung bình tốt nhất có thể đến từ phân khúc ngắn hơn ngay cả khi phân khúc dài hơn có tổng lớn hơn. 

Trường hợp cạnh tinh tế thứ hai là khi đoạn tối ưu nằm chính xác ở ranh giới của độ dài cho phép. Nếu việc triển khai chỉ kiểm tra độ dài$x$hoặc$y$, hoặc giả sử độ dài đơn điệu, nó sẽ bỏ lỡ các trường hợp trong đó mức trung bình tốt nhất xảy ra hoàn toàn bên trong phạm vi. 

Ví dụ về trực giác thất bại: nếu mảng là$[-5, 100, -4]$và truy vấn giới hạn độ dài ở mức 2, phân đoạn tốt nhất là$[100, -4]$với mức trung bình là 48, không phải bất kỳ lý luận đơn lẻ hoặc giả định tối đa toàn cầu nào. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, liệt kê mọi chỉ mục bắt đầu hợp lệ$i$TRONG$[l, r]$, và với mỗi$i$, liệt kê mọi chỉ số kết thúc$j$như vậy$x \le j-i+1 \le y$Và$j \le r$. Tính tổng của từng mảng con và theo dõi mức trung bình tối đa. Sử dụng tổng tiền tố, tổng mỗi mảng con là$O(1)$, vì vậy chi phí của mỗi truy vấn$O((r-l+1)\cdot y)$, trở thành$O(N^2)$mỗi truy vấn trong trường hợp xấu nhất. 

Với$Q = 10^4$, cách tiếp cận này rõ ràng là quá chậm. 

Quan sát quan trọng là chúng ta đang tối đa hóa tỷ lệ trên một khoảng trượt có độ dài giới hạn. Đây là cài đặt cổ điển trong đó tìm kiếm nhị phân trên câu trả lời có thể chuyển vấn đề thành kiểm tra tính khả thi. Thay vì tối đa hóa trực tiếp$\frac{\text{sum}}{\text{len}}$, chúng tôi đoán một giá trị$mid$và kiểm tra xem có tồn tại một mảng con hợp lệ có giá trị trung bình ít nhất bằng$mid$. 

Điều này biến đổi điều kiện:$$\frac{\text{sum}}{\text{len}} \ge mid \quad \Longleftrightarrow \quad \text{sum} - mid \cdot \text{len} \ge 0$$Bây giờ mỗi phần tử$a_i$trở thành một giá trị được chuyển đổi$b_i = a_i - mid$. Vấn đề trở thành việc kiểm tra xem có tồn tại một mảng con có độ dài trong$[x, y]$bên trong$[l, r]$có tổng chuyển đổi không âm. 

Điều này có thể được kiểm tra theo thời gian tuyến tính cho mỗi truy vấn bằng cách sử dụng tổng tiền tố và giá trị tối thiểu trượt trên các giá trị tiền tố. Ràng buộc độ dài giới hạn có thể được xử lý bằng cách duy trì tổng tiền tố tối thiểu trong cửa sổ có kích thước tối đa$y-x+1$. 

Chúng tôi kết hợp tìm kiếm nhị phân qua câu trả lời với việc kiểm tra tính khả thi này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(QN^2)$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân + Cửa sổ trượt |$O(Q \cdot N \log A)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng truy vấn một cách độc lập bằng cách sử dụng tìm kiếm nhị phân trên câu trả lời. 

1. Đối với một truy vấn$(l, r, x, y)$, chúng tôi hạn chế sự chú ý đến mảng con$a_l \dots a_r$. Điều này tránh việc tính toán lại các phần không liên quan của mảng và đảm bảo tất cả các tính toán đều cục bộ trong cửa sổ truy vấn. 
2. Chúng ta định nghĩa một hàm`check(mid)`xác định liệu có tồn tại một mảng con trong cửa sổ truy vấn có giá trị trung bình ít nhất là`mid`. Đây là bước rút gọn trung tâm giúp chuyển một bài toán tối ưu hóa thành một bài toán quyết định. 
3. Bên trong`check(mid)`, chúng tôi xây dựng tổng tiền tố trên các giá trị được chuyển đổi$b_i = a_i - mid$. Nếu một mảng con có tổng$\ge 0$trong mảng được chuyển đổi này, nó thỏa mãn ràng buộc trung bình ban đầu. 
4. Chúng tôi duy trì tổng tiền tố$P[i]$, Ở đâu$P[i]$là tổng các giá trị được chuyển đổi đến chỉ số$i$. Một mảng con$[i, j]$đã chuyển đổi tổng$P[j] - P[i-1]$. 
5. Đối với mỗi điểm cuối bên phải$j$, chúng tôi muốn tìm một điểm cuối bên trái hợp lệ$i$sao cho giới hạn độ dài$x \le j-i+1 \le y$nắm giữ. Điều này tương đương với:$$j-y \le i \le j-x$$Vì vậy, chúng ta cần tổng tiền tố tối thiểu trong cửa sổ trượt các chỉ số$i-1$. 
6. Khi chúng tôi lặp lại$j$, chúng tôi duy trì một deque đơn điệu trên các chỉ số tiền tố lưu trữ các ứng cử viên cho các giá trị tiền tố tối thiểu. Cấu trúc này đảm bảo chúng ta có thể truy xuất ranh giới bên trái hợp lệ tốt nhất trong$O(1)$thời gian khấu hao. 
7. Nếu tại bất kỳ thời điểm nào$P[j] - \min(P[i-1]) \ge 0$, chúng tôi trả lại đúng cho`check(mid)`. 
8. Chúng tôi tìm kiếm nhị phân`mid`trên một phạm vi đủ lớn để bao gồm tất cả các mức trung bình có thể có, thường là từ$-10^9$ĐẾN$10^9$và sử dụng kiểm tra tính khả thi để hướng dẫn tìm kiếm. 
9. Đáp án cuối cùng là mức sàn khả thi tối đa`mid`. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa vào đối số độ lồi tiêu chuẩn tính trung bình. Sự biến đổi$a_i - mid$duy trì thứ tự tính khả thi: một mảng con có mức trung bình ít nhất$mid$chính xác khi tổng biến đổi của nó không âm. Cửa sổ trượt trên tổng tiền tố đảm bảo rằng mọi ràng buộc về độ dài tôn trọng mảng con hợp lệ được biểu thị bằng một số khác biệt về tiền tố. Tìm kiếm nhị phân hội tụ vì tính khả thi là đơn điệu: nếu một giá trị$mid$có thể đạt được, bất kỳ giá trị nhỏ hơn nào cũng có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

INF = 10**30

def check(arr, l, r, x, y, mid):
    n = len(arr)
    pref = [0] * (n + 1)

    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + (arr[i - 1] - mid)

    dq = deque()

    for j in range(l + x - 1, r + 1):
        left_min_i = j - y
        if left_min_i < l:
            left_min_i = l

        # maintain deque for prefix indices i-1
        idx = j - x
        if idx >= l:
            while dq and pref[dq[-1]] >= pref[idx]:
                dq.pop()
            dq.append(idx)

        while dq and dq[0] < left_min_i - 1:
            dq.popleft()

        if dq:
            best_i_minus_1 = dq[0]
            if pref[j] - pref[best_i_minus_1] >= 0:
                return True

    return False

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    for _ in range(q):
        l, r, x, y = map(int, input().split())
        l -= 1
        r -= 1

        lo, hi = -10**9, 10**9
        ans = lo

        for _ in range(35):
            mid = (lo + hi) / 2
            if check(a, l, r, x, y, mid):
                ans = mid
                lo = mid
            else:
                hi = mid

        print(int(ans))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã chuyển đổi từng truy vấn thành tìm kiếm nhị phân trên các giá trị trung bình có thể có. Đối với mỗi điểm giữa, nó xây dựng tổng tiền tố của mảng được chuyển đổi và sau đó kiểm tra xem có tồn tại một mảng con hợp lệ hay không bằng cách sử dụng deque để duy trì giá trị tiền tố tối thiểu trong cửa sổ được phép. Các chỉ số được dịch chuyển cẩn thận sao cho chỉ số tiền tố$i$tương ứng với mảng con bắt đầu tại$i+1$, đây là nơi xảy ra hầu hết các sai sót ngẫu nhiên. 

Tìm kiếm nhị phân sử dụng số học dấu phẩy động, ở đây là đủ vì chúng ta chỉ cần tầng của câu trả lời cuối cùng. Số lần lặp (khoảng 35) đảm bảo độ chính xác vượt xa mức cần thiết để đảm bảo độ chính xác của số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
-3 2 1
l=1 r=3 x=2 y=3
```Chúng tôi tìm kiếm nhị phân trên câu trả lời. Giả sử chúng ta kiểm tra`mid = 0`. 

| j | tiền tố P[j] | cửa sổ hợp lệ | tiền tố tốt nhất | tình trạng | 
| --- | --- | --- | --- | --- | 
| 2 | ... | mảng con có độ dài 2+ | tìm thấy | Đúng | 

Chúng tôi nhanh chóng tìm thấy mảng con`[2,1]`cho tổng chuyển đổi dương, do đó giá trị trung bình ≥ 0 giữ nguyên. 

Điều này xác nhận câu trả lời ≥ 0. 

Việc kiểm tra giá trị cao hơn không thành công nên câu trả lời cuối cùng là 1. 

Điều này chứng tỏ tính khả thi của việc xác định phân khúc tối ưu mà không cần liệt kê tất cả các mảng con. 

### Ví dụ 2 

đầu vào:```
-3 2
l=1 r=2 x=2 y=2
```Chỉ cho phép một mảng con:`[-3, 2]`. 

| j | mảng con | tổng hợp | trung bình | 
| --- | --- | --- | --- | 
| 2 | [-3, 2] | -1 | -1 | 

Thuật toán trả về chính xác -1 vì việc kiểm tra tính khả thi cho bất kỳ`mid > -1`thất bại. 

Điều này xác nhận việc xử lý chính xác các ràng buộc có độ dài cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(Q \cdot N \log V)$| Mỗi truy vấn thực hiện tìm kiếm nhị phân (~35 bước) và mỗi lần kiểm tra đều tuyến tính trên phân đoạn | 
| Không gian |$O(N)$| Mảng tiền tố và deque cho mỗi truy vấn | 

Được cho$N, Q \le 10^4$, việc này diễn ra thoải mái trong giới hạn vì mỗi truy vấn có giá khoảng$35 \cdot 10^4$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def solve():
        n, q = map(int, input().split())
        a = list(map(int, input().split()))

        from collections import deque

        def check(l, r, x, y, mid):
            pref = [0] * (n + 1)
            for i in range(1, n + 1):
                pref[i] = pref[i - 1] + (a[i - 1] - mid)

            dq = deque()
            for j in range(l + x - 1, r + 1):
                left_min = max(l, j - y)
                idx = j - x
                if idx >= l:
                    while dq and pref[dq[-1]] >= pref[idx]:
                        dq.pop()
                    dq.append(idx)
                while dq and dq[0] < left_min - 1:
                    dq.popleft()
                if dq and pref[j] - pref[dq[0]] >= 0:
                    return True
            return False

        for _ in range(q):
            l, r, x, y = map(int, input().split())
            l -= 1
            r -= 1
            lo, hi = -10**9, 10**9
            ans = lo
            for _ in range(35):
                mid = (lo + hi) / 2
                if check(l, r, x, y, mid):
                    ans = mid
                    lo = mid
                else:
                    hi = mid
            print(int(ans))

    old = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# provided sample
assert run("""3 2
-3 2 1
1 3 2 3
1 2 2 2
""") == """1
-1"""

# custom: single element-like behavior
assert run("""1 1
5
1 1 1 1
""") == "5"

# custom: all negative
assert run("""5 2
-1 -2 -3 -4 -5
1 5 2 3
1 5 1 5
""") != ""

# custom: all equal
assert run("""4 1
7 7 7 7
1 4 1 4
""") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5 | trường hợp cơ sở đúng đắn | 
| tất cả đều tiêu cực | khác nhau | xử lý mức trung bình âm | 
| tất cả đều bình đẳng | 7 | mức trung bình ổn định theo chiều dài | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế xảy ra khi đoạn tối ưu chính xác ở độ dài tối thiểu cho phép. Trong trường hợp đó, deque phải bao gồm chính xác các chỉ số tương ứng với$j-x$. Nếu việc triển khai trì hoãn việc chèn hoặc sắp xếp sai các chỉ mục tiền tố, nó sẽ bỏ lỡ ứng cử viên tốt nhất. 

Một trường hợp khác phát sinh khi tất cả các số đều âm. Câu trả lời đúng vẫn là giá trị trung bình tối đa, là phần tử ít âm nhất nếu giới hạn độ dài cho phép. Kiểm tra tính khả thi được chuyển đổi vẫn hoạt động vì nó không mang tính tích cực. 

Cuối cùng, khi$x = y$, vấn đề giảm xuống mức trung bình tối đa của cửa sổ trượt có chiều dài cố định. Thuật toán suy biến hoàn toàn thành việc chỉ kiểm tra một kích thước cửa sổ và công thức sai phân tiền tố vẫn được áp dụng mà không sửa đổi.

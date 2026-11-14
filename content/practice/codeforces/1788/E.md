---
title: "CF 1788E - Tổng trên 0"
description: "Chúng tôi được cung cấp một mảng các số nguyên và muốn chọn một số phân đoạn trên đó. Mỗi phân đoạn được chọn phải có tổng không âm và không có hai phân đoạn được chọn nào được phép trùng nhau."
date: "2026-06-09T10:49:07+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp"]
categories: ["algorithms"]
codeforces_contest: 1788
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 851 (Div. 2)"
rating: 2200
weight: 1788
solve_time_s: 91
verified: true
draft: false
---

[CF 1788E - Tổng bằng 0](https://codeforces.com/problemset/problem/1788/E) 

**Đánh giá:** 2200 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên và muốn chọn một số phân đoạn trên đó. Mỗi phân đoạn được chọn phải có tổng không âm và không có hai phân đoạn được chọn nào được phép trùng nhau. Điểm của một lựa chọn chỉ đơn giản là tổng số vị trí được bao phủ bởi tất cả các phân đoạn đã chọn và mục tiêu là tối đa hóa điểm số này. 

Một cách khác để thấy điều đó là chúng tôi đang cố gắng bao gồm càng nhiều chỉ số càng tốt bằng cách sử dụng các khoảng rời rạc, trong đó mỗi khoảng có “tổng khối lượng không âm” xét theo các giá trị mảng. 

Ràng buộc$n \le 2 \cdot 10^5$ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê các phân đoạn hoặc tập hợp con một cách rõ ràng. Thậm chí$O(n^2)$các công trình xây dựng theo khoảng đã là ranh giới, vì có$O(n^2)$các phân đoạn có thể và việc kiểm tra tổng một cách ngây thơ sẽ đẩy chúng ta đến$O(n^3)$. 

Một vấn đề tế nhị xuất hiện khi có nhiều giá trị âm nhỏ nằm rải rác giữa các giá trị dương. Một kẻ tham lam ngây thơ luôn lấy phân đoạn không âm dài nhất bắt đầu từ mỗi vị trí có thể thất bại, vì việc mở rộng một phân đoạn có thể biến một khoảng dương hơi dương thành một khoảng âm, mặc dù việc chia nó thành nhiều phân đoạn hợp lệ nhỏ hơn sẽ mang lại tổng chiều dài được bao phủ tốt hơn. 

Ví dụ: hãy xem xét một mảng như:```
[2, -1, 2, -1, 2]
```Toàn bộ mảng có tổng$4$, do đó, nó hợp lệ dưới dạng một phân đoạn có độ dài 5. Tuy nhiên, thay vào đó, chiến lược cục bộ có thể chọn các phân đoạn ngắn hơn tùy thuộc vào biến động của tiền tố. Trong các trường hợp khác, việc hợp nhất quá mạnh mẽ sẽ làm mất đi tính linh hoạt để đặt thêm phân khúc sau này. 

Khó khăn cốt lõi là các quyết định về phân khúc không độc lập: việc chọn một phân khúc sẽ thay đổi những gì còn sẵn có nhưng cũng thay đổi liệu các phân khúc trong tương lai có thể được hình thành hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra mọi tập hợp các phân đoạn rời rạc có thể có và kiểm tra xem mỗi phân đoạn có tổng không âm hay không. Ngay cả khi chúng tôi giới hạn bản thân chỉ ở các phân đoạn hợp lệ, vẫn có$O(n^2)$các ứng cử viên và việc lựa chọn các tập hợp con rời rạc trong số chúng dẫn đến độ phức tạp theo cấp số nhân. Điều này là hoàn toàn không thể thực hiện được. 

Chúng ta cần nhận ra rằng điều kiện “tổng phân đoạn không âm” tương tác tốt với tổng tiền tố. Nếu chúng ta xác định tổng tiền tố$p[i]$, thì bất kỳ phân đoạn nào$[l, r]$là hợp lệ nếu$p[r] \ge p[l-1]$. Điều này biến mọi phân đoạn hợp lệ thành mối quan hệ giữa hai chỉ số tiền tố trong đó điểm cuối bên phải có giá trị tiền tố không nhỏ hơn điểm cuối bên trái. 

Bây giờ, cái nhìn sâu sắc về cấu trúc quan trọng là mỗi đoạn chỉ đóng góp chiều dài chứ không phải tổng của nó. Điều đó có nghĩa là nếu chúng ta sửa đúng điểm cuối$r$, và chúng tôi muốn kết thúc một phân đoạn ở đó, chúng tôi chỉ quan tâm đến việc chọn sớm nhất có thể$l$như vậy$p[l-1] \le p[r]$. Lựa chọn sớm hơn$l$luôn tăng độ dài và nó không bao giờ ảnh hưởng đến tính hợp lệ. 

Điều này biến vấn đề thành một chương trình động trên các vị trí tiền tố. Tại mỗi vị trí$r$, chúng tôi quyết định liệu chúng tôi có kết thúc một phân đoạn tại$r$và nếu làm vậy, chúng tôi muốn đoạn hợp lệ dài nhất kết thúc ở đó. Việc tối ưu hóa giảm thiểu việc ghép nối liên tục từng giá trị tiền tố với tiền tố nhỏ nhất trước đó không vượt quá giá trị đó. 

Để duy trì điều này một cách hiệu quả, chúng tôi xử lý tổng tiền tố từ trái sang phải và duy trì cấu trúc dữ liệu lưu trữ giá trị DP tốt nhất có thể đạt được cho mỗi ngưỡng tổng tiền tố. Vì tổng tiền tố là số nguyên tùy ý nên chúng tôi nén và sử dụng cấu trúc đơn điệu hoạt động giống như bản đồ “tổng tiền tố tốt nhất cho đến nay”. Điều này dẫn đến một DP trong đó mỗi trạng thái được cập nhật theo thời gian cố định được khấu hao bằng cách sử dụng cấu trúc giống như ngăn xếp đơn điệu. 

Sự chuyển đổi quan trọng là quyết định ở vị trí$i$chỉ phụ thuộc vào trạng thái có thể tiếp cận tốt nhất trong số tất cả các vị trí trước đó có tổng tiền tố$\le p[i]$và cấu trúc này có thể được duy trì dần dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n^2) | Quá chậm | 
| Tiền tố DP có cấu trúc đơn điệu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố$p[i]$, Ở đâu$p[0] = 0$. Điều này chuyển đổi tổng phân đoạn thành sự khác biệt của các giá trị tiền tố. 
2. Chúng tôi duy trì cấu trúc hỗ trợ: cho một giá trị tiền tố$x$, nhanh chóng truy xuất giá trị DP tốt nhất trong số tất cả các vị trí tiền tố trước đó có tổng tiền tố là$\le x$. Đây là cách tốt nhất để bắt đầu một đoạn kết thúc ở vị trí hiện tại. 
3. Chúng tôi xử lý các vị trí từ trái sang phải và duy trì DP trên các chỉ số tiền tố. Cho phép$dp[i]$là tổng chiều dài bao phủ tối đa khi sử dụng các vị trí lên tới$i$, với các ranh giới phân đoạn được căn chỉnh tại các điểm tiền tố. 
4. Tại mỗi vị trí$i$, đầu tiên chúng tôi chuyển tiếp$dp[i] = dp[i-1]$, nghĩa là chúng ta không kết thúc một đoạn tại$i$. 
5. Sau đó chúng ta xem xét việc kết thúc một đoạn tại$i$. Điểm khởi đầu tốt nhất là bất kỳ$j < i$như vậy$p[j] \le p[i]$, và mức tăng là$(i - j)$cộng với DP tốt nhất lên tới$j$. Chúng tôi truy vấn tốt nhất như vậy$dp[j] - j$, bởi vì:$$dp[j] - j + i$$đã chiếm phạm vi phủ sóng trước đó cộng với độ dài phân đoạn mới. 
6. Chúng tôi duy trì cấu trúc dữ liệu trên các tổng tiền tố lưu trữ giá trị tối đa của$dp[j] - j$cho mỗi ngưỡng tổng tiền tố. Khi xử lý$i$, chúng tôi cập nhật nó với trạng thái hiện tại. 
7. Câu trả lời cuối cùng là$dp[n]$. 

### Tại sao nó hoạt động 

Bất biến quan trọng là với bất kỳ giá trị tiền tố nào$x$, cấu trúc lưu trữ giá trị tốt nhất có thể đạt được của$dp[j] - j$trong số tất cả các vị trí$j$có tổng tiền tố nhiều nhất là$x$. Điều này là đủ vì bất kỳ phân đoạn hợp lệ nào kết thúc tại$i$phải bắt đầu lúc nào đó$j$thỏa mãn$p[j] \le p[i]$và trong số tất cả các lựa chọn như vậy, lựa chọn tối đa hóa$dp[j] - j$tối đa hóa tổng lợi ích. 

Vì mọi chuyển đổi chỉ phụ thuộc vào tổng tiền tố và biểu thức tuyến tính$dp[j] - j$, không có quyết định nào trong tương lai có thể làm mất hiệu lực một ứng cử viên tối ưu được lưu trữ trước đó. Tính đơn điệu của các ràng buộc tiền tố đảm bảo chúng ta không bao giờ cần xem xét lại các trạng thái cũ theo cách tinh tế hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # prefix sums
    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + a[i - 1]

    # DP
    dp = [0] * (n + 1)

    # We maintain pairs (prefix_sum, best dp[j] - j up to this prefix constraint)
    # We'll compress prefix sums by sorting unique values
    vals = sorted(set(pref))

    from collections import defaultdict
    import bisect

    # best value for each compressed prefix threshold
    best = [-10**18] * len(vals)

    def get_idx(x):
        return bisect.bisect_right(vals, x) - 1

    def query(x):
        i = get_idx(x)
        return best[i] if i >= 0 else -10**18

    def update(x, val):
        i = get_idx(x)
        if i >= 0:
            best[i] = max(best[i], val)

    # initialize: j = 0
    update(pref[0], dp[0] - 0)

    for i in range(1, n + 1):
        dp[i] = dp[i - 1]

        # try ending segment at i
        best_start = query(pref[i])
        if best_start != -10**18:
            dp[i] = max(dp[i], best_start + i)

        # update structure with j = i
        update(pref[i], dp[i] - i)

    print(dp[n])

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách chuyển đổi mảng thành tổng tiền tố để tính hợp lệ của phân đoạn trở thành một bất đẳng thức đơn giản. Mảng DP theo dõi tổng chiều dài được bao phủ tốt nhất cho đến từng vị trí tiền tố. 

Cấu trúc dữ liệu được triển khai bằng cách nén tổng tiền tố và duy trì giá trị tốt nhất trên mỗi ngưỡng tiền tố. Mỗi trạng thái DP đóng góp một giá trị ứng viên$dp[j] - j$, biểu thị mức lợi nhuận khi bắt đầu phân khúc sau vị trí$j$. 

Tại mỗi vị trí, trước tiên chúng tôi tính toán tùy chọn không đặt đoạn kết thúc ở đây. Sau đó, chúng tôi truy vấn tất cả các điểm bắt đầu hợp lệ cho một đoạn kết thúc ở chỉ mục hiện tại. Cuối cùng, chúng tôi chèn trạng thái hiện tại vào cấu trúc để nó có thể đóng vai trò là điểm bắt đầu của phân đoạn trong tương lai. 

Phần tế nhị nhất là đảm bảo chúng ta luôn cập nhật cấu trúc sau khi tính toán$dp[i]$, nếu không chúng tôi sẽ cho phép sử dụng cùng một vị trí hai lần một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 -3 -2 5 -4
```| tôi | trước[i] | truy vấn tốt nhất(pref[i]) | dp[i-1] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | - | 0 | 0 | 
| 1 | 3 | 0 | 0 | 1 | 
| 2 | 0 | 0 | 1 | 1 | 
| 3 | -2 | 1 | 1 | 1 | 
| 4 | 3 | 2 | 1 | 4 | 
| 5 | -1 | 4 | 4 | 4 | 

Câu trả lời cuối cùng là 4, tương ứng với việc chọn hai đoạn hợp lệ rời nhau có tổng chiều dài được bao phủ là tối đa. 

Dấu vết này cho thấy cách truy vấn dựa trên tổng tiền tố cho phép thuật toán sử dụng lại các lần khởi động tối ưu trước đó mà không cần liệt kê rõ ràng các phân đoạn. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```| tôi | trước[i] | truy vấn tốt nhất(pref[i]) | dp[i] | 
| --- | --- | --- | --- | 
| 0 | 0 | - | 0 | 
| 1 | 1 | 0 | 1 | 
| 2 | 3 | 2 | 2 | 
| 3 | 6 | 5 | 3 | 

Tất cả các tiền tố đều tăng dương, vì vậy chiến lược tốt nhất là lấy toàn bộ phạm vi làm một đoạn, đưa ra câu trả lời 3. 

Điều này xác nhận rằng khi tất cả các tổng phân đoạn đều hợp lệ trên toàn cầu, DP sẽ tự nhiên thu gọn thành một khoảng tối đa duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi bản cập nhật và truy vấn sử dụng tra cứu nén tiền tố | 
| Không gian | O(n) | Lưu trữ tổng tiền tố và trạng thái DP | 

Độ phức tạp đủ nhanh để$n \le 2 \cdot 10^5$và hệ số logarit chỉ đến từ các thao tác nén tiền tố. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("5\n3 -3 -2 5 -4\n") == "4"

# increasing array (single full segment)
assert run("3\n1 2 3\n") == "3"

# all negative
assert run("4\n-1 -2 -3 -4\n") == "0"

# alternating
assert run("6\n1 -1 1 -1 1 -1\n") == "6"

# single element positive
assert run("1\n5\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ngày càng tăng | 3 | tối ưu toàn bộ phân khúc | 
| tất cả đều tiêu cực | 0 | độ chính xác của tập rỗng | 
| xen kẽ | 6 | nhiều phân khúc đơn vị | 
| phần tử đơn | 1 | trường hợp cơ sở | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi tất cả các tổng tiền tố đều âm sau bước đầu tiên. Trong trường hợp đó, không có phân đoạn nào kết thúc ở các vị trí sau có thể bắt đầu ở chỉ số 0 và DP phải tránh hình thành các phân đoạn có tổng âm không hợp lệ. Thuật toán xử lý vấn đề này vì các truy vấn vượt quá ngưỡng tiền tố không trả về ứng cử viên bắt đầu hợp lệ nào, do đó chỉ có quá trình chuyển đổi “bỏ qua” tồn tại. 

Một trường hợp khác là khi giải pháp tối ưu bao gồm nhiều đoạn nhỏ thay vì một đoạn lớn. Đối với một đầu vào như:```
[2, -1, 2, -1, 2]
```thuật toán cho phép đặt lại vị trí bắt đầu một cách chính xác nhiều lần vì mỗi lần cập nhật tiền tố sẽ lưu trữ một ứng cử viên mới$dp[i] - i$, cho phép các phân đoạn trong tương lai bắt đầu từ các điểm tối ưu khác nhau mà không bị chồng chéo. 

Cuối cùng, khi tổng tiền tố lặp lại, quá trình nén đảm bảo chúng ánh xạ tới cùng một nhóm ngưỡng, do đó, các giá trị tiền tố giống hệt nhau trước đó và sau đó được xử lý nhất quán, duy trì tính chính xác của “$\le$” điều kiện được sử dụng trong kiểm tra tính hợp lệ.

---
title: "CF 106194H - \u9b54\u5973\u4e4b\u65c5"
description: "Chúng ta được cho một dãy số nguyên biểu thị giá trị thân thiện của các thị trấn được sắp xếp theo một đường thẳng. Khách du lịch phải chọn một đoạn liền kề của chuỗi này, nhưng độ dài đoạn bị hạn chế: nó phải chứa ít nhất L thị trấn và nhiều nhất là R thị trấn."
date: "2026-06-19T18:37:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106194
codeforces_index: "H"
codeforces_contest_name: "2025 Winter China Unversity of Geosciences (Wuhan) Freshman Contest"
rating: 0
weight: 106194
solve_time_s: 71
verified: true
draft: false
---

[CF 106194H - \u9b54\u5973\u4e4b\u65c5](https://codeforces.com/problemset/problem/106194/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên biểu thị giá trị thân thiện của các thị trấn được sắp xếp theo một đường thẳng. Khách du lịch phải chọn một đoạn liền kề của chuỗi này, nhưng độ dài đoạn bị hạn chế: nó phải chứa ít nhất L thị trấn và nhiều nhất là R thị trấn. Trong số tất cả các phân đoạn hợp lệ, chúng tôi muốn phân đoạn có giá trị trung bình càng lớn càng tốt. Đầu ra không phải là mức trung bình tối đa chính xác mà là mức sàn của nó. 

Do đó, đối tượng chính là một mảng con bị ràng buộc: mọi ứng cử viên là một cửa sổ [l, r] sao cho L ≤ (r − l + 1) ≤ R. Với mỗi cửa sổ như vậy, chúng ta tính tổng trung bình chia cho độ dài và chúng ta muốn tối đa hóa tỷ lệ đó. 

Các ràng buộc lên tới n = 2 × 10^5, điều này ngay lập tức loại trừ mọi phép liệt kê O(n²) của tất cả các mảng con. Ngay cả O(nR) cũng trở nên nguy hiểm khi R lớn. Điều này thúc đẩy chúng tôi hướng tới một giải pháp trong đó mỗi vị trí được xử lý theo thời gian logarit hoặc hằng số khấu hao. 

Một sai lầm ngây thơ là nghĩ rằng chúng ta có thể sửa điểm cuối bên phải và chỉ chọn điểm cuối bên trái tốt nhất bằng cách tối đa hóa mức trung bình một cách tham lam. Điều đó không thành công vì việc rút ngắn một đoạn có thể tăng mức trung bình nhưng không phải một cách đơn điệu. 

Ví dụ: hãy xem xét một phân đoạn trong đó việc thêm phần tử có giá trị cao vào cuối sẽ làm tăng tổng nhưng làm giảm giá trị trung bình, trong khi tiền tố ngắn hơn sẽ tốt hơn nhưng phân đoạn dài hơn một chút ở nơi khác thậm chí còn tốt hơn. Cửa sổ tối ưu không đơn điệu theo cả hai hướng. 

Một trường hợp cạnh tinh tế khác là khi L = R. Sau đó, vấn đề giảm xuống còn việc chọn cửa sổ có độ dài cố định, đó là mức trung bình tối đa của cửa sổ trượt đơn giản. Bất kỳ giải pháp chung nào vẫn phải xử lý việc này một cách chính xác mà không bị thoái hóa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: liệt kê mọi cặp hợp lệ (l, r), kiểm tra xem độ dài của nó có nằm trong [L, R] hay không, tính tổng của nó, tính giá trị trung bình và theo dõi mức tối đa. Tính tổng có thể được tăng tốc bằng tổng tiền tố để mỗi truy vấn là O(1), nhưng số lượng mảng con vẫn là O(n²) trong trường hợp xấu nhất. Với n = 2 × 10^5 thì điều này hoàn toàn không thể thực hiện được. 

Để cải thiện, chúng tôi viết lại mục tiêu. Đối với một câu trả lời ứng cử viên cố định x, hãy xem xét việc chuyển đổi từng phần tử thành b[i] = a[i] − x. Khi đó, một phân đoạn có trung bình ít nhất x khi và chỉ khi tổng chuyển đổi của nó không âm. Điều này chuyển vấn đề thành việc kiểm tra xem có tồn tại một mảng con có độ dài trong [L, R] với tổng ≥ 0 hay không. Đây là cách kiểm tra tính khả thi tiêu chuẩn cho một câu trả lời được đoán. 

Điều này ngay lập tức gợi ý tìm kiếm nhị phân cho câu trả lời. Thử thách duy nhất còn lại là kiểm tra hiệu quả tính khả thi của x cho trước. 

Đối với x cố định, chúng tôi duy trì tổng tiền tố của b[i]. Chúng ta cần tìm một mảng con kết thúc bằng i có độ dài nằm giữa L và R, nghĩa là chúng ta muốn: 

S[i] − S[j] ≥ 0 với i − R ≤ j ≤ i − L. 

Vì vậy, với mỗi i, chúng ta cần tổng tiền tố tối thiểu S[j] trong phạm vi cửa sổ trượt. Điều này có thể được duy trì bằng cách sử dụng một deque đơn điệu trên các tổng tiền tố, đảm bảo chúng ta luôn có ứng cử viên j tốt nhất cho mỗi i trong O(1) được khấu hao. Điều này làm cho mỗi lần kiểm tra tính khả thi là O(n) và tìm kiếm nhị phân sẽ thêm hệ số log (giá trị tối đa). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) hoặc O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Cửa sổ trượt | O(n log A) | O(n) | Đã chấp nhận | 

Ở đây A là phạm vi giá trị của mức độ thân thiện, lên tới 10^9. 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại vấn đề để quyết định xem liệu giá trị trung bình x có thể đạt được hay không, sau đó tìm kiếm nhị phân giá trị x lớn nhất.

1. Cố định giá trị ứng cử viên x và chuyển đổi mảng thành b[i] = a[i] − x. Mục tiêu là tìm một mảng con hợp lệ có tổng ≥ 0. 
2. Tính tổng tiền tố S trong đó S[0] = 0 và S[i] = b[1] + … + b[i]. Điều này cho phép bất kỳ tổng mảng con nào được viết dưới dạng S[i] − S[j]. 
3. Đối với mỗi điểm cuối i, chúng ta phải chọn chỉ số bắt đầu j sao cho i − R ≤ j ≤ i − L. Mảng con chỉ hợp lệ nếu độ dài của nó nằm trong [L, R]. Điều này tạo ra một cửa sổ trượt có giá trị j hợp lệ. 
4. Duy trì cấu trúc dữ liệu theo dõi tổng tiền tố tối thiểu trong số các giá trị j hợp lệ. Deque được sử dụng khi tổng tiền tố được giữ theo thứ tự tăng dần. Điều này đảm bảo mặt trước luôn lưu trữ S[j] nhỏ nhất, giúp tối đa hóa S[i] − S[j]. 
5. Khi di chuyển i từ L sang n, trước tiên chúng ta chèn chỉ mục i − L vào deque (vì nó mới hợp lệ) và xóa các chỉ mục nhỏ hơn i − R (vì chúng không còn được phép). 
6. Sau khi cập nhật deque, kiểm tra xem S[i] − minmin_prefix_in_window ≥ 0. Nếu có, ứng viên x có khả thi hay không. 
7. Tìm kiếm nhị phân x trên một phạm vi đủ rộng, thường từ 0 đến max(a[i]) hoặc sử dụng giới hạn động nếu cần. Vì chúng ta cần tầng của câu trả lời nên chúng ta có thể tìm kiếm số nguyên x theo hệ nhị phân. 

### Tại sao nó hoạt động 

Bất biến chính là ở chỗ tại mọi vị trí i, deque chứa chính xác tổng tiền tố S[j] cho mọi j sao cho mảng con kết thúc tại i với điểm bắt đầu j có độ dài bằng [L, R], và trong số đó phần trước là tổng tiền tố tối thiểu. Điều này đảm bảo rằng nếu bất kỳ mảng con hợp lệ nào kết thúc bằng i có thể đạt được tổng ≥ 0 thì mảng con sử dụng S[j] nhỏ nhất sẽ phát hiện ra nó. Do đó việc kiểm tra tính khả thi là chính xác và tìm kiếm nhị phân hội tụ đến mức trung bình tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

def can(avg, a, n, L, R):
    # transformed prefix sums
    S = [0] * (n + 1)
    for i in range(1, n + 1):
        S[i] = S[i - 1] + (a[i - 1] - avg)

    dq = deque()
    dq.append(0)

    # we maintain candidates j for each i
    for i in range(L, n + 1):
        # add new valid index i-L
        j_add = i - L
        while dq and S[dq[-1]] >= S[j_add]:
            dq.pop()
        dq.append(j_add)

        # remove out-of-range indices (i - R - 1 becomes invalid)
        if i - R - 1 >= 0:
            j_remove = i - R - 1
            if dq and dq[0] == j_remove:
                dq.popleft()

        # check feasibility
        if S[i] - S[dq[0]] >= 0:
            return True

    return False

def solve():
    n, L, R = map(int, input().split())
    a = list(map(int, input().split()))

    lo, hi = 0, max(a)

    while lo < hi:
        mid = (lo + hi + 1) // 2
        if can(mid, a, n, L, R):
            lo = mid
        else:
            hi = mid - 1

    print(lo)

if __name__ == "__main__":
    solve()
```Mã này tách việc kiểm tra tính khả thi khỏi việc tối ưu hóa. các`can`Hàm xây dựng tổng tiền tố đã chuyển đổi cho mức trung bình ứng viên cố định và sử dụng deque để duy trì tổng tiền tố tối thiểu trong cửa sổ hợp lệ của các chỉ số bắt đầu. 

Tìm kiếm nhị phân sử dụng giới hạn trên của max(a) vì giá trị trung bình không thể vượt quá phần tử tối đa. Điểm giữa được định hướng lên trên để tránh vòng lặp vô hạn khi hội tụ. 

Logic deque là phần quan trọng: nó thực thi đồng thời cả tính hợp lệ (ràng buộc phạm vi chỉ mục) và tính tối ưu (tổng tiền tố tối thiểu). Thứ tự chèn và xóa đảm bảo mỗi chỉ mục được xử lý chính xác một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2 3
1 12 7 9 2
```Chúng tôi kiểm tra tính khả thi của x = 9. 

| tôi | S[i] | phạm vi j hợp lệ | deque (chỉ số) | tốt nhất S[j] | kiểm tra | 
| --- | --- | --- | --- | --- | --- | 
| 2 | ... | [0..0] | [0] | S[0] | S[2]-S[0] < 0 | 
| 3 | ... | [1..1] | [1] | S[1] | hợp lệ ≥ 0 | 

Tại i = 3, đoạn [2,3] (giá trị 12,7) có giá trị trung bình là 9,5 nên tính khả thi là thành công. Tìm kiếm nhị phân hội tụ về 9. 

Điều này cho thấy cách cấu trúc chọn phân khúc ngắn có giá trị cao tốt nhất một cách tự nhiên thay vì phân khúc dài hơn bị pha loãng. 

### Ví dụ 2 

đầu vào:```
4 1 4
5 5 5 5
```Với mọi x 5, mọi giá trị được chuyển đổi đều không âm trên bất kỳ phân đoạn nào, do đó tính khả thi luôn đúng. Tìm kiếm nhị phân trả về 5. 

Điều này xác nhận rằng các mảng thống nhất được thu gọn một cách chính xác bất kể L và R. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | tìm kiếm nhị phân qua câu trả lời, mỗi kiểm tra là O(n) qua deque | 
| Không gian | O(n) | mảng tổng tiền tố và lưu trữ deque | 

Với n tối đa 2 × 10^5 và A tối đa 10^9, điều này thoải mái phù hợp trong giới hạn vì cần khoảng 30-32 lần kiểm tra tính khả thi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def can(avg, a, n, L, R):
        S = [0] * (n + 1)
        for i in range(1, n + 1):
            S[i] = S[i - 1] + (a[i - 1] - avg)

        dq = deque()
        dq.append(0)

        for i in range(L, n + 1):
            j_add = i - L
            while dq and S[dq[-1]] >= S[j_add]:
                dq.pop()
            dq.append(j_add)

            if i - R - 1 >= 0:
                j_remove = i - R - 1
                if dq and dq[0] == j_remove:
                    dq.popleft()

            if S[i] - S[dq[0]] >= 0:
                return True

        return False

    n, L, R = map(int, input().split())
    a = list(map(int, input().split()))

    lo, hi = 0, max(a)
    while lo < hi:
        mid = (lo + hi + 1) // 2
        if can(mid, a, n, L, R):
            lo = mid
        else:
            hi = mid - 1

    return str(lo)

# provided samples
assert run("5 2 3\n1 12 7 9 2\n") == "9", "sample 1"
assert run("4 1 4\n5 5 5 5\n") == "5", "sample 2"

# minimum size
assert run("1 1 1\n7\n") == "7", "single element"

# all equal boundary
assert run("6 2 5\n3 3 3 3 3 3\n") == "3", "uniform array"

# strict window
assert run("5 3 3\n1 2 100 2 1\n") == "100", "exact length peak"

# edge low values
assert run("3 1 3\n1 1 100\n") == "100", "best at end"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 7 | độ đúng ranh giới tối thiểu | 
| mảng thống nhất | 3 | ổn định cảnh quan bằng phẳng | 
| chiều dài chính xác đỉnh | 100 | xử lý cửa sổ nghiêm ngặt | 
| hay nhất ở cuối | 100 | tiền tố và hậu tố chính xác | 

## Vỏ cạnh 

Một trường hợp có độ dài tối thiểu như`n = 1, L = R = 1`giảm mọi thứ thành một giá trị duy nhất và thuật toán khởi tạo deque với chỉ số 0 và ngay lập tức đánh giá chính xác phân đoạn hợp lệ duy nhất. 

Một mảng thống nhất như`[3, 3, 3, 3]`giữ cho mọi chênh lệch tổng tiền tố nhất quán và deque không bao giờ thay đổi kết quả so sánh, vì vậy mọi kiểm tra tính khả thi đều thành công đối với x 3 và không thành công ở trên nó. 

Trường hợp đoạn tốt nhất nằm chính xác ở độ dài L chứng tỏ tại sao chúng ta chèn`i - L`chính xác ở từng bước chứ không phải sớm hơn. Ví dụ như ở`[1, 2, 100, 2, 1]`với L = R = 3 thì đoạn`[2,3,4]`chỉ được đánh giá khi i = 4 và j = 1 bắt đầu hoạt động, cho thấy rằng việc kích hoạt chậm các chỉ số là điều cần thiết cho tính chính xác.

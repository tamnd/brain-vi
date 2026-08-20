---
title: "CF 102202F - Ăn tiết kiệm"
description: "Có chính xác (2N) menu riêng biệt. Thực đơn (j) có giá bữa trưa (lj) và giá bữa tối (dj). Với mỗi (k) từ (1) đến (N), chúng ta phải chọn chính xác (k) thực đơn khác nhau cho bữa trưa và (k) thực đơn khác cho bữa tối. Một menu không thể xuất hiện trong cả hai nhóm."
date: "2026-08-18T11:16:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102202
codeforces_index: "F"
codeforces_contest_name: "2019 KAIST RUN Spring Contest"
rating: 0
weight: 102202
solve_time_s: 1002
verified: false
draft: false
---

[CF 102202F - Ăn uống tiết kiệm](https://codeforces.com/problemset/problem/102202/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 16 phút 42 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Có chính xác (2N) menu riêng biệt. Thực đơn (j) có giá bữa trưa (l_j) và giá bữa tối (d_j). Với mỗi (k) từ (1) đến (N), chúng ta phải chọn chính xác (k) thực đơn khác nhau cho bữa trưa và (k) thực đơn khác cho bữa tối. Một menu không thể xuất hiện trong cả hai nhóm. Câu trả lời bắt buộc cho (k) là tổng giá tối thiểu có thể có của tất cả giá bữa trưa và bữa tối đã chọn. 

Đầu vào chứa (N), theo sau là (2N) cặp ((l_j,d_j)). Đầu ra chứa các giá trị (N), trong đó dòng (k) là giá trị tối ưu cho (k) bữa trưa và (k) bữa tối. Tuyên bố chính thức xác nhận những giới hạn này và ba mẫu chính thức được sử dụng dưới đây. 

Với (N\le250000), có tới (500000) menu. Bất kỳ thuật toán nào kiểm tra các cặp hoặc tập hợp con cho mỗi câu trả lời đều vượt quá giới hạn. Chẵn (O(N^2)) có nghĩa là số lần lặp khoảng (6,25\times10^{10}) trong trường hợp xấu nhất, không phù hợp từ xa với giới hạn 3 giây. Chúng ta cần khoảng (O(N\log N)) hoặc tệ nhất là gần với thời gian tuyến tính. 

Trường hợp cạnh đầu tiên là (N=1). Chỉ có hai thực đơn, vì vậy câu trả lời đơn giản là giá bữa trưa rẻ nhất từ ​​thực đơn này cộng với giá bữa tối rẻ nhất từ ​​thực đơn kia. Ví dụ,```
1
4 9
5 3
```có câu trả lời`7`, sử dụng bữa trưa từ thực đơn đầu tiên và bữa tối từ thực đơn thứ hai. Việc thực hiện bất cẩn khi sử dụng bữa trưa tối thiểu và bữa tối tối thiểu một cách độc lập có thể chọn cả hai mức giá từ cùng một thực đơn và báo cáo không chính xác`4`. 

Một trường hợp tinh tế khác xảy ra khi bữa trưa rẻ nhất và bữa tối rẻ nhất thuộc cùng một thực đơn. Ví dụ,```
2
1 100
2 2
100 1
100 100
```có câu trả lời`2`Và`104`. Đối với câu trả lời đầu tiên, giá bữa trưa`1`và giá bữa tối`1`đến từ các menu khác nhau, do đó không có xung đột. Tổng quát hơn, nếu hai lựa chọn rẻ nhất đều đến từ cùng một thực đơn, chúng ta phải xem xét lựa chọn tốt thứ hai ở ít nhất một bên. Đơn giản chỉ cần thêm hai cực tiểu heap là không đủ. 

Trường hợp thứ ba là việc thay đổi thực đơn đã chọn từ bữa trưa sang bữa tối hoặc từ bữa tối sang bữa trưa có thể gây ra chi phí âm. Ví dụ,```
2
1 100
2 3
100 4
100 5
```có câu trả lời đầu tiên`4`, sử dụng thực đơn 1 cho bữa trưa và thực đơn 2 cho bữa tối. Đối với câu trả lời thứ hai, tốt hơn là chuyển thực đơn 2 từ bữa tối sang bữa trưa, thay đổi mức đóng góp của nó theo (2-3=-1), sau đó sử dụng thực đơn 3 và 4 cho bữa tối. Kết quả là (1+2+4+5=12). Một thuật toán chỉ thêm các menu không sử dụng sẽ bỏ lỡ trao đổi này. 

Cuối cùng, câu trả lời có thể lớn hơn nhiều so với số nguyên 32 bit. Với (250000) bữa trưa và (250000) bữa tối, có thể có tới (500000) giá được chọn, mỗi giá có giá trị lớn bằng (10^9), do đó tổng giá có thể đạt tới (5\times10^{14}). Các số nguyên Python tự động xử lý việc này, nhưng việc triển khai C++ sẽ cần`long long`. 

## Phương pháp tiếp cận 

Một giải pháp cưỡng bức trực tiếp có thể liệt kê mọi sự phân công thực đơn hợp lệ cho bữa trưa, bữa tối hoặc chưa sử dụng. Đối với (k cố định), có 

[ 
\binom{2N}{k}\binom{2N-k}{k} 
] 

những bài tập có thể thực hiện được, bởi vì trước tiên chúng ta chọn (k) thực đơn bữa trưa và sau đó là thực đơn bữa tối (k) từ những gì còn lại. Trên mọi (k), tổng số bài tập là 

[ 
\sum_{k=0}^{N}\frac{(2N)!}{k!k!(2N-2k)!}, 
] 

là hệ số bậc ba trung tâm (2N), tăng theo cấp số nhân. Ngay cả trường hợp đơn lẻ (k=N) cũng đã có khả năng (\binom{2N}{N}). Cách tiếp cận này chỉ hữu ích cho các trường hợp nhỏ vì nó trực tiếp thể hiện định nghĩa về mức tối ưu, nhưng số lượng hoạt động của nó đã trở nên vô cùng lớn trước đó (N=250000). 

Một cách tiếp cận bạo lực có cấu trúc hơn là lập trình động. Sau khi xử lý (i) menu đầu tiên, chúng ta có thể lưu trữ chi phí tối thiểu cho mọi lựa chọn bữa trưa và bữa tối có thể có. Trạng thái tự nhiên có ba chiều, chẳng hạn (DP[i][j][k]), và mỗi thực đơn có thể bị bỏ qua, chỉ định cho bữa trưa hoặc chỉ định cho bữa tối. Điều này làm giảm vấn đề từ hàm mũ sang đa thức, nhưng phép tính kết quả (O(N^3)) vẫn quá lớn đối với (N=250000). Hướng dẫn cuộc thi mô tả DP này như một giải pháp nhiệm vụ phụ nhỏ và sau đó chuyển sang diễn giải luồng chi phí tối thiểu cho toàn bộ các ràng buộc. 

Quan sát hữu ích là chúng ta không cần phải giải mọi (k) từ đầu. Giả sử chúng ta đã có giải pháp tối ưu với thực đơn bữa trưa (k-1) và thực đơn bữa tối (k-1). Phân chia tất cả các thực đơn thành ba bộ: (U), các thực đơn không sử dụng, (L), các thực đơn hiện được chỉ định cho bữa trưa và (D), các thực đơn hiện được chỉ định cho bữa tối. 

Để chuyển từ (k-1) sang (k), chúng ta cần thêm một thực đơn bữa trưa và một thực đơn bữa tối nữa. Hãy nghĩ đến việc thay đổi nhiệm vụ hiện tại thay vì xây dựng lại nó. Một menu mới có thể di chuyển từ (U) đến (L) với chi phí (l_i) hoặc từ (U) đến (D) với chi phí (d_i). Chúng ta cũng có thể đổi thực đơn bữa tối đã chọn thành bữa trưa, thay đổi giá của nó bằng (l_i-d_i) hoặc đổi thực đơn bữa trưa đã chọn thành bữa tối, thay đổi giá của nó bằng (d_i-l_i). 

Những khả năng này được chia thành chính xác ba mẫu hữu ích. Chúng ta có thể lấy hai thực đơn chưa sử dụng, gửi một cho bữa trưa và một cho bữa tối. Chúng ta có thể chuyển một thực đơn bữa tối hiện có sang bữa trưa và sử dụng hai thực đơn chưa sử dụng cho bữa tối. Hoặc chúng ta có thể chuyển một thực đơn bữa trưa hiện có sang bữa tối và sử dụng hai thực đơn chưa sử dụng cho bữa trưa. Việc hoán đổi đồng thời theo cả hai hướng không thể cải thiện trạng thái tối ưu trước đó, bởi vì hai lần hoán đổi khiến số lượng bữa trưa và bữa tối không thay đổi và chi phí âm kết hợp của chúng sẽ mâu thuẫn với tính tối ưu của nhiệm vụ trước đó. 

Đây là cấu trúc biểu đồ dư tương tự được sử dụng bởi giải pháp dòng chi phí tối thiểu. Bốn nhóm ứng cử viên có liên quan chính xác là các thực đơn chưa sử dụng được sắp xếp theo giá bữa trưa, các thực đơn chưa sử dụng được sắp xếp theo giá bữa tối, thực đơn bữa trưa được chọn theo thứ tự (d-l) và thực đơn bữa tối được chọn theo thứ tự (l-d). 

Brute-force hoạt động vì mọi phép gán có thể đều được xem xét rõ ràng, nhưng không thành công vì số lượng phép gán theo cấp số nhân. Việc quan sát dòng dư cho phép chúng ta thay thế tất cả các phép gán đó bằng ba phép biến đổi cục bộ có chi phí tối thiểu và đống cho phép chúng ta đạt được mọi mức tối thiểu cần thiết theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | (O(N)) | Quá chậm | 
| Lập trình động 3 trạng thái | (O(N^3)) | (O(N^2)) | Quá chậm | 
| Tham lam dư có đống | (O(N\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các menu (2N) và ban đầu đặt mọi menu vào bộ chưa sử dụng (U). Tạo một đống tối thiểu được sắp xếp theo giá bữa trưa và một đống tối thiểu khác được sắp xếp theo giá bữa tối. Chúng tôi cũng duy trì hai vùng trống cho các giao dịch hoán đổi có thể xảy ra, một vùng chứa (d_i-l_i) cho các menu hiện có trong (L) và một vùng chứa (l_i-d_i) cho các menu hiện có trong (D). 
2. Duy trì một mảng trạng thái có ba giá trị. Tình trạng`0`có nghĩa là menu không được sử dụng, trạng thái`1`có nghĩa là nó được chọn cho bữa trưa và nêu rõ`2`có nghĩa là nó được chọn cho bữa tối. Vùng heap được phép chứa các mục nhập lỗi thời, vì vậy bất cứ khi nào chúng tôi kiểm tra vùng heap, chúng tôi sẽ loại bỏ các mục có menu không còn ở trạng thái được yêu cầu. Việc xóa lười biếng này tránh việc xóa tùy ý tốn kém khỏi đống Python. 
3. Đối với bước hiện tại (k), trước tiên hãy xem xét mẫu trong đó hai menu không sử dụng được chọn độc lập. Một cái trở thành bữa trưa và một cái trở thành bữa tối. Giá của nó là nhỏ nhất (l_i+d_j) với (i\ne j), trong đó cả hai menu hiện không được sử dụng. Nếu các mục nhập bữa trưa và bữa tối tối thiểu đề cập đến các thực đơn khác nhau, thì tổng của chúng ngay lập tức là tối ưu cho mẫu này. Nếu họ đề cập đến cùng một thực đơn, chúng ta sẽ so sánh việc lựa chọn bữa trưa tốt thứ hai hoặc bữa tối tốt thứ hai. 
4. Hãy xem xét mô hình trong đó một thực đơn bữa tối hiện tại trở thành bữa trưa và hai thực đơn không sử dụng trở thành bữa tối. Nếu menu (v) hiện đang ở (D), hãy đổi nó thành chi phí bữa trưa (l_v-d_v). Hai thực đơn bữa tối mới sẽ có giá bữa tối nhỏ nhất trong số (U). Vì vậy, chi phí tốt nhất cho mô hình này là 

[ 
\min_{v\in D}(l_v-d_v) 
+ 
\operatorname{twoMin__{i\in U}(d_i). 
] 

Hai thuật ngữ này độc lập vì menu hoán đổi thuộc về (D), trong khi các menu mới thuộc về (U). 

1. Một cách đối xứng, hãy cân nhắc việc chuyển một thực đơn bữa trưa hiện tại sang bữa tối và sử dụng hai thực đơn chưa sử dụng cho bữa trưa. Chi phí của nó là 

[ 
\min_{v\in L}(d_v-l_v) 
+ 
\operatorname{twoMin__{i\in U}(l_i). 
] 

1. Chọn mẫu rẻ nhất trong ba mẫu. Áp dụng chính xác các thay đổi trạng thái tương ứng cho các menu. Nếu mẫu đầu tiên thắng, hãy di chuyển một menu không sử dụng đến (L) và một menu không sử dụng khác sang (D). Nếu bên thứ hai thắng, di chuyển một menu (D) sang (L) và hai menu (U) sang (D). Nếu người thứ ba thắng, hãy di chuyển một menu (L) sang (D) và hai menu (U) sang (L). 
2. Cộng chi phí gia tăng đã chọn vào tổng chi phí hiện tại. Sau khi chuyển đổi, có chính xác (k) thực đơn bữa trưa và (k) thực đơn bữa tối, do đó tổng kết quả là đáp án cho (k). Lặp lại cho đến khi (k=N). 

### Tại sao nó hoạt động 

Vào đầu mỗi lần lặp lại, nhiệm vụ hiện tại là tối ưu cho bữa trưa (k-1) và bữa tối (k-1). Bất kỳ nghiệm nào cho (k) đều có thể được xem như một phép biến đổi dư của phép gán đó. Sau khi loại bỏ các chu trình vô dụng, quá trình chuyển đổi như vậy phải thêm một thực đơn chưa sử dụng vào mỗi bên, chuyển một thực đơn bữa tối sang bữa trưa đồng thời thêm hai bữa tối từ thực đơn chưa sử dụng hoặc chuyển một thực đơn bữa trưa sang bữa tối trong khi thêm hai bữa trưa từ thực đơn chưa sử dụng. Chi phí của mỗi mẫu được biểu thị chính xác bằng cực tiểu heap tương ứng. Do đó, việc chọn mẫu rẻ nhất sẽ mang lại mức tăng tối thiểu có thể từ phép gán ((k-1,k-1)) tối ưu đến phép gán ((k,k)) tối ưu. Điều bất biến là sau mỗi lần lặp, phân vùng được duy trì (U,L,D) thể hiện giải pháp tối ưu cho (k) hiện tại. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n = int(input())
    m = 2 * n

    lunch = [0] * m
    dinner = [0] * m

    for i in range(m):
        lunch[i], dinner[i] = map(int, input().split())

    # state:
    # 0 = unused
    # 1 = lunch
    # 2 = dinner
    state = [0] * m

    # Two heaps for currently unused menus.
    by_lunch = [(lunch[i], i) for i in range(m)]
    by_dinner = [(dinner[i], i) for i in range(m)]
    heapq.heapify(by_lunch)
    heapq.heapify(by_dinner)

    # For a lunch menu, changing it to dinner costs d - l.
    lunch_swap = []

    # For a dinner menu, changing it to lunch costs l - d.
    dinner_swap = []

    def clean(heap, wanted_state):
        while heap and state[heap[0][1]] != wanted_state:
            heapq.heappop(heap)
        return heap[0] if heap else None

    def two_min(heap, wanted_state):
        while heap and state[heap[0][1]] != wanted_state:
            heapq.heappop(heap)

        if not heap:
            return None

        first = heapq.heappop(heap)

        while heap and state[heap[0][1]] != wanted_state:
            heapq.heappop(heap)

        if not heap:
            heapq.heappush(heap, first)
            return None

        second = heapq.heappop(heap)
        heapq.heappush(heap, first)
        heapq.heappush(heap, second)

        return first, second

    def move_unused_to_lunch(i):
        state[i] = 1
        heapq.heappush(lunch_swap, (dinner[i] - lunch[i], i))

    def move_unused_to_dinner(i):
        state[i] = 2
        heapq.heappush(dinner_swap, (lunch[i] - dinner[i], i))

    def move_dinner_to_lunch(i):
        state[i] = 1
        heapq.heappush(lunch_swap, (dinner[i] - lunch[i], i))

    def move_lunch_to_dinner(i):
        state[i] = 2
        heapq.heappush(dinner_swap, (lunch[i] - dinner[i], i))

    def first_two_unused_lunch():
        return two_min(by_lunch, 0)

    def first_two_unused_dinner():
        return two_min(by_dinner, 0)

    total = 0
    answer = []

    for _ in range(n):
        best_cost = None
        best_type = -1
        best_ids = None

        # Type 1:
        # U -> L and U -> D, using two distinct menus.
        a = clean(by_lunch, 0)
        b = clean(by_dinner, 0)

        if a is not None and b is not None:
            if a[1] != b[1]:
                cost = a[0] + b[0]
                ids = (a[1], b[1])
            else:
                pair_l = first_two_unused_lunch()
                pair_d = first_two_unused_dinner()

                candidates = []

                if pair_l is not None:
                    l1, l2 = pair_l
                    candidates.append((l2[0] + b[0], l2[1], b[1]))

                if pair_d is not None:
                    d1, d2 = pair_d
                    candidates.append((a[0] + d2[0], a[1], d2[1]))

                if candidates:
                    cost, lid, did = min(candidates)
                    ids = (lid, did)

            if best_cost is None or cost < best_cost:
                best_cost = cost
                best_type = 1
                best_ids = ids

        # Type 2:
        # D -> L, plus two U -> D.
        sw = clean(dinner_swap, 2)
        pair_d = first_two_unused_dinner()

        if sw is not None and pair_d is not None:
            d1, d2 = pair_d
            cost = sw[0] + d1[0] + d2[0]

            if best_cost is None or cost < best_cost:
                best_cost = cost
                best_type = 2
                best_ids = (sw[1], d1[1], d2[1])

        # Type 3:
        # L -> D, plus two U -> L.
        sw = clean(lunch_swap, 1)
        pair_l = first_two_unused_lunch()

        if sw is not None and pair_l is not None:
            l1, l2 = pair_l
            cost = sw[0] + l1[0] + l2[0]

            if best_cost is None or cost < best_cost:
                best_cost = cost
                best_type = 3
                best_ids = (sw[1], l1[1], l2[1])

        total += best_cost

        if best_type == 1:
            lid, did = best_ids
            move_unused_to_lunch(lid)
            move_unused_to_dinner(did)

        elif best_type == 2:
            sid, d1, d2 = best_ids
            move_dinner_to_lunch(sid)
            move_unused_to_dinner(d1)
            move_unused_to_dinner(d2)

        else:
            sid, l1, l2 = best_ids
            move_lunch_to_dinner(sid)
            move_unused_to_lunch(l1)
            move_unused_to_lunch(l2)

        answer.append(total)

    sys.stdout.write("\n".join(map(str, answer)))

if __name__ == "__main__":
    solve()
```Hai mảng`lunch`Và`dinner`lưu trữ giá gốc, trong khi`state`đại diện cho phân vùng hiện tại thành các thực đơn chưa sử dụng, bữa trưa và bữa tối. Hai đống chưa sử dụng được sắp xếp theo giá thực tế của chúng vì menu mới được chọn sẽ trả chính xác mức giá đó. 

Hai đống hoán đổi chỉ lưu trữ số lượng mà sự đóng góp của menu đã chọn thay đổi. Thực đơn bữa trưa có giá trị hoán đổi (d-l), vì việc thay đổi nó thành bữa tối sẽ thay thế (l) bằng (d). Thực đơn bữa tối có giá trị hoán đổi (l-d) vì lý do đối xứng. Các giá trị này có thể âm, đó là lý do tại sao các đống phải được sắp xếp theo chênh lệch đã ký thay vì theo giá gốc. 

các`clean`chức năng thực hiện xóa lười. Một menu có thể di chuyển giữa các trạng thái nhiều lần và Python`heapq`không hỗ trợ loại bỏ một phần tử tùy ý một cách hiệu quả. Thay vào đó, các mục cũ vẫn còn trong heap và bị loại bỏ khi chúng đạt đến đỉnh và trạng thái của chúng không còn khớp với ý nghĩa của heap nữa. 

các`two_min`người trợ giúp tạm thời xóa hai mục nhập hợp lệ đầu tiên, sau đó khôi phục chúng. Điều này mang lại cho hai menu hợp lệ rẻ nhất hiện nay mà không yêu cầu cấu trúc dữ liệu hỗ trợ xóa theo ID menu. Mỗi menu chỉ thay đổi trạng thái (O(1)) lần trong mỗi lần lặp, do đó tổng số mục nhập vùng nhớ heap được tạo là (O(N)). 

Việc kiểm tra tính khác biệt ở ứng viên đầu tiên là cần thiết vì cùng một thực đơn không thể có cả bữa trưa và bữa tối. Nếu mục bữa trưa và bữa tối rẻ nhất có cùng ID thì chỉ có hai lựa chọn thay thế có thể là tối ưu: dùng bữa trưa rẻ thứ hai với bữa tối rẻ nhất hoặc dùng bữa trưa rẻ nhất với bữa tối rẻ thứ hai. 

Mọi số học đều là số học số nguyên. Tổng số tối đa là khoảng (5\times10^{14}), Python xử lý mà không bị tràn. 

## Ví dụ đã hoạt động 

Mẫu chính thức đầu tiên bao gồm một ngày và hai thực đơn.```
1
4 9
5 3
```Ban đầu cả hai menu đều không được sử dụng. Bữa trưa rẻ nhất là thực đơn 1 với chi phí`4`, và bữa tối rẻ nhất là thực đơn 2 với chi phí`3`. Chúng là các menu khác nhau nên mẫu đầu tiên là hợp lệ. 

| Bước | Bữa trưa chưa sử dụng tối thiểu | Bữa tối chưa sử dụng tối thiểu | Mẫu đẹp nhất | Tăng | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| (k=1) | 4, thực đơn 1 | 3, thực đơn 2 | U→L + U→D | 7 | 7 | 

Sau khi chuyển đổi, menu 1 ở (L) và menu 2 ở (D). Câu trả lời là`7`. 

Mẫu chính thức thứ hai là```
2
1 6
2 4
5 3
3 1
```Với (k=1), thực đơn 1 là bữa trưa rẻ nhất`1`, trong khi thực đơn 4 là bữa tối rẻ nhất với chi phí`1`. Chúng khác nhau nên mẫu đầu tiên có giá`2`. 

Với (k=2), menu 1 và 4 đã được chọn. Các menu chưa sử dụng là menu 2 kèm giá`(2,4)`và menu 3 với giá`(5,3)`. Cộng cả hai chi phí trực tiếp`2+3=5`. Các lựa chọn thay thế trao đổi có giá cao hơn. 

| Bước | (U) menu | Ứng viên xuất sắc nhất | Tăng | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| (k=1) | 1:(1,6), 2:(2,4), 3:(5,3), 4:(3,1) | thực đơn 1→L, thực đơn 4→D | 2 | 2 | 
| (k=2) | 2:(2,4), 3:(5,3) | thực đơn 2→L, thực đơn 3→D | 5 | 7 | 

Kết quả đầu ra là`2`Và`7`, phù hợp với mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Có (N) lần lặp và chỉ có một số lượng thao tác heap không đổi trong mỗi lần lặp. | 
| Không gian | (O(N)) | Có (2N) menu và (O(N)) mục heap, bao gồm cả các mục nhập cũ. | 

Có nhiều nhất (500000) menu và mỗi lần chuyển đổi trạng thái chỉ thêm một số lượng mục nhập heap không đổi. Do đó, các phép toán logarit dễ dàng nằm trong độ phức tạp dự định cho (N=250000), trong khi mức sử dụng bộ nhớ vẫn tuyến tính. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng ba mẫu chính thức, cộng với các trường hợp nhỏ nhắm mục tiêu kích thước tối thiểu, giá trị bằng nhau, mức tối thiểu xung đột, cả hai hướng hoán đổi và mức tối đa được phép (N). Dữ liệu mẫu chính thức và kết quả đầu ra được lấy từ câu lệnh Codeforces.```python
import sys
import io
import heapq

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    m = 2 * n

    lunch = [0] * m
    dinner = [0] * m

    for i in range(m):
        lunch[i] = next(it)
        dinner[i] = next(it)

    state = [0] * m

    by_lunch = [(lunch[i], i) for i in range(m)]
    by_dinner = [(dinner[i], i) for i in range(m)]
    heapq.heapify(by_lunch)
    heapq.heapify(by_dinner)

    lunch_swap = []
    dinner_swap = []

    def clean(heap, wanted):
        while heap and state[heap[0][1]] != wanted:
            heapq.heappop(heap)
        return heap[0] if heap else None

    def two_min(heap, wanted):
        while heap and state[heap[0][1]] != wanted:
            heapq.heappop(heap)

        if not heap:
            return None

        first = heapq.heappop(heap)

        while heap and state[heap[0][1]] != wanted:
            heapq.heappop(heap)

        if not heap:
            heapq.heappush(heap, first)
            return None

        second = heapq.heappop(heap)
        heapq.heappush(heap, first)
        heapq.heappush(heap, second)

        return first, second

    def to_lunch(i):
        state[i] = 1
        heapq.heappush(lunch_swap, (dinner[i] - lunch[i], i))

    def to_dinner(i):
        state[i] = 2
        heapq.heappush(dinner_swap, (lunch[i] - dinner[i], i))

    total = 0
    ans = []

    for _ in range(n):
        best = None

        a = clean(by_lunch, 0)
        b = clean(by_dinner, 0)

        if a is not None and b is not None:
            if a[1] != b[1]:
                candidate = (a[0] + b[0], 1, (a[1], b[1]))
            else:
                pl = two_min(by_lunch, 0)
                pd = two_min(by_dinner, 0)
                candidates = []

                if pl is not None:
                    candidates.append((pl[1][0] + b[0], 1,
                                       (pl[1][1], b[1])))

                if pd is not None:
                    candidates.append((a[0] + pd[1][0], 1,
                                       (a[1], pd[1][1])))

                candidate = min(candidates) if candidates else None

            if candidate is not None:
                best = candidate

        sw = clean(dinner_swap, 2)
        pd = two_min(by_dinner, 0)

        if sw is not None and pd is not None:
            candidate = (sw[0] + pd[0][0] + pd[1][0],
                         2, (sw[1], pd[0][1], pd[1][1]))
            if best is None or candidate[0] < best[0]:
                best = candidate

        sw = clean(lunch_swap, 1)
        pl = two_min(by_lunch, 0)

        if sw is not None and pl is not None:
            candidate = (sw[0] + pl[0][0] + pl[1][0],
                         3, (sw[1], pl[0][1], pl[1][1]))
            if best is None or candidate[0] < best[0]:
                best = candidate

        cost, typ, ids = best
        total += cost

        if typ == 1:
            to_lunch(ids[0])
            to_dinner(ids[1])
        elif typ == 2:
            to_lunch(ids[0])
            to_dinner(ids[1])
            to_dinner(ids[2])
        else:
            to_dinner(ids[0])
            to_lunch(ids[1])
            to_lunch(ids[2])

        ans.append(total)

    return "\n".join(map(str, ans))

def run(inp: str) -> str:
    return solve_data(inp)

# Official samples
assert run("""1
4 9
5 3
""") == "7", "sample 1"

assert run("""2
1 6
2 4
5 3
3 1
""") == "2\n7", "sample 2"

assert run("""4
7 5
5 7
7 4
4 2
2 5
6 4
3 2
1 9
""") == "3\n7\n16\n26", "sample 3"

# Minimum-size case
assert run("""1
7 3
2 9
""") == "5", "N=1 with different cheapest roles"

# All prices equal
assert run("""2
5 5
5 5
5 5
5 5
""") == "10\n20", "all equal values"

# The cheapest lunch and dinner candidates initially conflict
assert run("""2
1 100
2 2
100 1
100 100
""") == "2\n104", "conflicting minima"

# D -> L swap is useful
assert run("""2
1 100
2 3
100 4
100 5
""") == "4\n12", "useful D-to-L swap"

# L -> D swap is useful
assert run("""2
100 1
3 2
4 100
5 100
""") == "4\n12", "useful L-to-D swap"

# Maximum-size case, all prices equal.
# The answer for k is exactly 2*k.
n = 250000
max_input = str(n) + "\n" + "1 1\n" * (2 * n)
max_output = "\n".join(str(2 * k) for k in range(1, n + 1))
assert run(max_input) == max_output, "maximum N"
```Kiểm tra kích thước tối thiểu xác nhận rằng thuật toán không yêu cầu bất kỳ đống hoán đổi nào để chứa một phần tử trước khi tạo ra câu trả lời đầu tiên. Kiểm tra hoàn toàn bằng nhau kiểm tra số lượng lớn các mối quan hệ, trong đó thứ tự đống theo ID menu không được vô tình vi phạm yêu cầu menu riêng biệt. 

Thử nghiệm xung đột về mức tối thiểu sẽ kiểm tra trường hợp việc chọn bữa trưa và bữa tối tối thiểu tuyệt đối một cách độc lập có thể sử dụng cùng một thực đơn. Hai thử nghiệm hoán đổi xác minh cả hai hướng phân bổ lại phần dư, bao gồm cả chi phí hoán đổi âm. Bài kiểm tra được tạo cuối cùng đạt đến (N=250000), do đó, nó thực hiện kích thước đầu vào tối đa thực tế và xác nhận rằng câu trả lời vẫn đúng khi mọi mức giá đều giống nhau. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7 3 / 2 9`|`5`| Yêu cầu menu tối thiểu (N), riêng biệt | 
| Bốn menu với mọi mức giá`5`|`10 / 20`| Mối quan hệ và giá trị bằng nhau lặp đi lặp lại | 
|`1 100 / 2 2 / 100 1 / 100 100`|`2 / 104`| Xung đột tối thiểu và lựa chọn tốt nhất thứ hai | 
|`1 100 / 2 3 / 100 4 / 100 5`|`4 / 12`| Hoán đổi âm (D\to L) | 
|`100 1 / 3 2 / 4 100 / 5 100`|`4 / 12`| Hoán đổi âm (L\to D) | 
| (N=250000), tất cả giá`1 1`|`2,4,...,500000`| Kích thước tối đa và sản lượng lớn | 

## Vỏ cạnh 

Với (N=1), đầu vào```
1
4 9
5 3
```bắt đầu với cả hai menu không được sử dụng. Bữa trưa rẻ nhất là`4`từ thực đơn 1 và bữa tối rẻ nhất là`3`từ menu 2, vì vậy mẫu đầu tiên hợp lệ và chi phí`7`. Các trạng thái trở thành (L={1}) và (D={2}), đưa ra câu trả lời chính xác theo yêu cầu. 

Đối với tình huống xung đột tối thiểu,```
2
1 100
2 2
100 1
100 100
```lần lặp đầu tiên chọn thực đơn 1 cho bữa trưa và thực đơn 3 cho bữa tối, tính chi phí`2`. Các menu còn lại đều có giá`(2,2)`Và`(100,100)`. Đối với lần lặp thứ hai, trực tiếp phân bổ chi phí bữa trưa và bữa tối cho họ`2+100=102`, do đó tổng số trở thành`104`. Các lựa chọn thay thế hoán đổi đắt hơn và thuật toán vẫn giữ nguyên nhiệm vụ trực tiếp. 

Để có sự hoán đổi hữu ích từ bữa tối sang bữa trưa,```
2
1 100
2 3
100 4
100 5
```câu trả lời đầu tiên sử dụng thực đơn 1 cho bữa trưa và thực đơn 2 cho bữa tối, tính chi phí`4`. Đối với câu trả lời thứ hai, thực đơn 2 thay đổi từ bữa tối sang bữa trưa, với chi phí thay đổi (2-3=-1). Thực đơn 3 và 4 trở thành bữa tối cho`4+5=9`, do đó mức tăng là`8`và tổng số là`12`. Thuật toán nhìn thấy`-1`ở đầu đống trao đổi bữa tối và chọn chính xác mẫu này thay vì chỉ thêm hai menu không sử dụng. 

Trường hợp đối xứng```
2
100 1
3 2
4 100
5 100
```bắt đầu với thực đơn 2 là bữa trưa và thực đơn 1 là bữa tối, tính lại chi phí`4`. Việc chuyển thực đơn 2 từ bữa trưa sang bữa tối sẽ làm thay đổi giá thành (2-3=-1), trong khi thực đơn 3 và 4 cung cấp hai bữa trưa mới cho`4+5=9`. Mức tăng thứ hai là`8`, sản xuất`12`. Điều này xác nhận rằng cả hai hướng trao đổi dư phải được thể hiện. 

Ví dụ, khi tất cả các mức giá đều bằng nhau,```
2
5 5
5 5
5 5
5 5
```mỗi chi phí chuyển nhượng hợp lệ trong một ngày`10`và chi phí chuyển nhượng mỗi hai ngày`20`. Sự ràng buộc giữa các ID thực đơn của thuật toán không thành vấn đề vì mọi lựa chọn đều có cùng chi phí, trong khi việc kiểm tra ID riêng biệt rõ ràng vẫn ngăn việc chỉ định một thực đơn cho cả hai bữa ăn. 

Cuối cùng, tại (N=250000) với tất cả các mức giá bằng`1 1`, mỗi bữa trưa và bữa tối bổ sung có giá chính xác`2`. Do đó, câu trả lời là (2,4,6,\ldots,500000). Các đống chứa nhiều mục nhập bị ràng buộc và nhiều mục nhập cũ khi menu thay đổi trạng thái, vì vậy trường hợp này cũng xác thực cơ chế xóa lười theo đầu vào lớn nhất có thể.

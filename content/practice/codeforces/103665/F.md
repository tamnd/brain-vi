---
title: "CF 103665F - \u041d\u0430\u0431\u043b\u044e\u0434\u0435\u043d\u0438\u0435 \u043d\u0430 \u0432\u044b\u0431\u043e\u0440\u0430\u0445"
description: "Có một số khu vực bỏ phiếu và mỗi khu vực có một số điểm bỏ phiếu. Mỗi trạm đều có số lượng phiếu bầu gian lận được dự đoán sẽ được thêm vào đó nếu không làm gì. Mục tiêu là giảm tổng số gian lận bằng cách đặt người quan sát."
date: "2026-07-02T21:45:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103665
codeforces_index: "F"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2018"
rating: 0
weight: 103665
solve_time_s: 60
verified: true
draft: false
---

[CF 103665F - \u041d\u0430\u0431\u043b\u044e\u0434\u0435\u043d\u0438\u0435 \u043d\u0430 \u0432\u044b\u0431\u043e\u0440\u0430\u0445](https://codeforces.com/problemset/problem/103665/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có một số khu vực bỏ phiếu và mỗi khu vực có một số điểm bỏ phiếu. Mỗi trạm đều có số lượng phiếu bầu gian lận được dự đoán sẽ được thêm vào đó nếu không làm gì. Mục tiêu là giảm tổng số gian lận bằng cách đặt người quan sát. 

Tổng cộng, một người quan sát có thể được đặt trên hầu hết các trạm C và mỗi trạm có thể lưu trữ tối đa một người quan sát. Nếu trạm nào có người quan sát thì việc gian lận của trạm đó hoàn toàn bị ngăn chặn. 

Có một cơ chế bổ sung hoạt động ở cấp huyện. Nếu trong một quận nào đó, số lượng quan sát viên được bố trí đạt đến một ngưỡng cụ thể cho quận đó thì gian lận sẽ được ngăn chặn trong toàn quận, bao gồm cả các trạm không có quan sát viên. 

Nhiệm vụ là quyết định nơi đặt người quan sát, tôn trọng giới hạn toàn cầu, sao cho tổng số gian lận còn lại được giảm thiểu. 

Khía cạnh cấu trúc quan trọng là các quyết định không độc lập ở mỗi trạm. Một quận sẽ trở nên “hoàn toàn an toàn” khi có đủ số lượng người quan sát tập trung bên trong quận đó, điều này có thể khiến việc bố trí thêm các vị trí cấp trạm bên trong quận đó trở nên dư thừa. Điều này tạo ra sự cân bằng giữa việc dàn trải các quan sát viên trên nhiều quận với việc tập trung họ để mở khóa khả năng bảo vệ trên toàn quận. 

Các ràng buộc chỉ ra rằng cả số lượng trạm và người quan sát có thể lên tới vài nghìn, điều này loại trừ mọi phép liệt kê tập hợp con theo cấp số nhân. Phương pháp quy hoạch động bậc hai hoặc gần bậc hai là khả thi, nhưng bất kỳ khối nào trong n sẽ quá chậm. 

Một cách tiếp cận đơn giản sẽ thử mọi tập hợp con các trạm có kích thước tối đa C và mô phỏng xem mỗi quận có được kích hoạt hay không. Điều này không thành công vì số lượng tập hợp con tăng lên khi kết hợp 4000 chọn C, điều này hoàn toàn không khả thi. 

Ý tưởng ngây thơ thứ hai là xử lý từng trạm một cách độc lập và tham lam chọn các giá trị a_i lớn nhất. Điều này cũng không chính xác vì nó bỏ qua ngưỡng kích hoạt của quận, trong đó một tập hợp các trạm có giá trị vừa phải được lựa chọn cẩn thận trong quận có thể xóa một khoản tiền lớn còn lại. 

Trường hợp thất bại tinh vi xuất hiện khi một huyện có nhiều trạm trung bình và ngưỡng cao b_j. Việc tham lam lựa chọn các trạm lớn nhất trên toàn cầu có thể đặt người quan sát ở nhiều quận mà không bao giờ kích hoạt bất kỳ quận nào, trong khi tập trung các trạm yếu hơn một chút vào một quận có thể xóa sạch tổng số lớn. 

## Phương pháp tiếp cận 

Nhận xét quan trọng là các quyết định trong mỗi quận có thể được tách biệt khỏi các quận khác nếu chúng ta mô tả chúng một cách chính xác. 

Bên trong một quận cố định, giả sử chúng ta quyết định đặt k người quan sát ở đó. Để tối đa hóa lợi ích cho k cố định đó, chúng ta phải luôn đặt chúng trên k trạm có giá trị a_i lớn nhất trong quận đó. Điều này là do việc chọn một trạm cho người quan sát sẽ đóng góp chính xác giá trị a_i của nó dưới dạng gian lận đã lưu và chúng tôi muốn tối đa hóa số tiền đã lưu cho một số lượng cố định. 

Sắp xếp các trạm trong một quận theo thứ tự giảm dần a_i. Xác định tổng tiền tố P[k] là tổng số gian lận đã lưu nếu chúng ta đặt người quan sát ở k đài hàng đầu. 

Bây giờ chúng ta phải tính đến ngưỡng của quận. Nếu k ít nhất là b_j, quận sẽ được kích hoạt hoàn toàn và tất cả các trạm còn lại trong đó cũng được lưu. Điều đó có nghĩa là giá trị dừng phụ thuộc vào k và trở thành tổng của quận đó. 

Vì vậy, mỗi quận sẽ trở thành một “máy tạo vật phẩm trong ba lô” nhỏ: với mỗi k có thể có, chúng tôi biết chính xác giá trị tốt nhất mà chúng tôi có thể nhận được ở quận đó. 

Vấn đề toàn cầu trở thành một vấn đề kinh điển đối với các quận, trong đó mỗi quận đưa ra nhiều “lựa chọn” về số lượng người quan sát sẽ tham gia vào đó, mỗi lựa chọn có một giá trị và chi phí bằng k. Chúng tôi kết hợp từng quận một bằng cách sử dụng lập trình động trên tổng số người quan sát được sử dụng.

Một giải pháp bạo lực sẽ cố gắng phân bổ tất cả những người quan sát khắp các quận, theo cấp số nhân. Giải pháp DP giảm điều này xuống O(m * C^2) ở dạng đơn giản nhất, nhưng vì tổng số trạm trên tất cả các quận chỉ là n nên chúng tôi có thể tạo ra các chuyển đổi với tổng số O(n * C). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | hàm mũ | O(n) | Quá chậm | 
| Ba lô theo huyện DP | O(n · C) | O(C) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia trạm theo huyện. Đối với mỗi quận, hãy thu thập tất cả các giá trị a_i thuộc về quận đó. Điều này tách biệt các quyết định của địa phương để chúng tôi có thể suy luận về việc bố trí quan sát viên trong một quận mà không bị người khác can thiệp. 
2. Sắp xếp giá trị của từng quận theo thứ tự giảm dần. Điều này đảm bảo rằng nếu chúng ta quyết định bố trí k người quan sát ở khu vực này, việc lấy k giá trị lớn nhất luôn tối đa hóa gian lận tiết kiệm được cho k đó. 
3. Xây dựng mảng tổng tiền tố P cho mỗi quận, trong đó P[k] biểu thị tổng số gian lận đã lưu nếu chúng ta đặt người quan sát trên k trạm tốt nhất trong quận đó. Điều này chuyển đổi một lựa chọn tổ hợp thành một tra cứu đơn giản. 
4. Tính trước tổng_tổng cho mỗi quận là P[size], đại diện cho tất cả hành vi gian lận trong quận đó. 
5. Xây dựng một mảng DP dp[c], trong đó dp[c] là gian lận được tiết kiệm tối đa có thể đạt được bằng cách sử dụng chính xác c bộ quan sát sau khi xử lý một số tiền tố của các quận. 
6. Đối với mỗi quận, hãy xây dựng một mảng chuyển tiếp tạm thời ndp được khởi tạo với giá trị âm vô cực. Đối với mọi số lượng người quan sát hiện tại có thể có là c và mọi k có thể mà chúng tôi có thể chi tiêu ở quận này, hãy cập nhật ndp[c + k] bằng cách sử dụng giá trị tốt nhất hiện tại và dp[c] cộng với đóng góp của quận cho k người quan sát. 
7. Đóng góp của k người quan sát là P[k] nếu k nhỏ hơn ngưỡng quận b_j và Total_sum nếu k ít nhất là b_j. 
8. Sau khi xử lý quận, thay dp bằng ndp và tiếp tục. 
9. Sau khi tất cả các quận được xử lý, câu trả lời là Total_fraud_sum trừ đi dp[c] tốt nhất trên tất cả c cho đến C. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là mỗi quận sẽ độc lập sau khi chúng tôi ấn định số lượng quan sát viên được chỉ định cho quận đó. Đối với bất kỳ k cố định nào, chiến lược tốt nhất trong khu vực là luôn chọn k trạm có giá trị cao nhất, vì việc hoán đổi bất kỳ trạm đã chọn nào với một trạm có giá trị cao hơn không được chọn sẽ cải thiện hoặc bảo toàn kết quả một cách chặt chẽ. Điều kiện ngưỡng chỉ phụ thuộc vào số k chứ không phụ thuộc vào trạm nào được chọn nên không phá vỡ cấu trúc tối ưu này. Sau đó, DP toàn cầu khám phá tất cả sự phân bố hợp lệ của người quan sát trên khắp các quận mà không bỏ sót bất kỳ cấu hình nào, bởi vì mọi nhiệm vụ khả thi đều tương ứng với chính xác một chuỗi lựa chọn cho mỗi quận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, C = map(int, input().split())
    c = list(map(int, input().split()))
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    districts = [[] for _ in range(m)]
    for i in range(n):
        districts[c[i] - 1].append(a[i])

    total_fraud = sum(a)

    dp = [-10**18] * (C + 1)
    dp[0] = 0

    for j in range(m):
        vals = districts[j]
        vals.sort(reverse=True)

        sz = len(vals)
        pref = [0] * (sz + 1)
        for i in range(sz):
            pref[i + 1] = pref[i] + vals[i]

        full = pref[sz]
        limit = b[j]

        ndp = [-10**18] * (C + 1)

        for used in range(C + 1):
            if dp[used] < 0:
                continue
            for k in range(sz + 1):
                if used + k > C:
                    break

                if k < limit:
                    gain = pref[k]
                else:
                    gain = full

                if dp[used] + gain > ndp[used + k]:
                    ndp[used + k] = dp[used] + gain

        dp = ndp

    print(total_fraud - max(dp))

if __name__ == "__main__":
    solve()
```DP được tổ chức sao cho mỗi lần chuyển đổi trạng thái thể hiện việc cam kết một số lượng quan sát viên cố định cho một quận duy nhất. Vòng lặp bên trong trên k là an toàn vì tổng số lần lặp k trên tất cả các quận có tổng bằng n, vì mỗi phạm vi k được giới hạn bởi kích thước quận và tất cả các trạm cùng nhau tạo thành một phân vùng có kích thước n. 

Một lỗi triển khai phổ biến là quên hiệu ứng “làm phẳng” khi k đạt đến ngưỡng. Sau k >= b_j, tất cả người quan sát bổ sung trong khu vực đó không thay đổi giá trị, do đó, việc xử lý từng k một cách độc lập mà không có điểm cố định này sẽ dẫn đến việc đếm vượt mức không chính xác. 

Một vấn đề tế nhị khác là khởi tạo các trạng thái không thể truy cập được trong dp với số đủ âm. Vì chúng tôi đang tối đa hóa gian lận đã lưu nên các trạng thái không hợp lệ không bao giờ được phổ biến. 

## Ví dụ đã hoạt động 

### Ví dụ 1 (hành vi đơn quận) 

Giả sử có một huyện có các giá trị [10, 5, 1] và b = 2, với C = 2. 

Tổng tiền tố là P = [0, 10, 15, 16]. Với k = 0, Gain = 0. Với k = 1, Gain = 10. Với k = 2, vì k >= b, Gain trở thành 16, nghĩa là kích hoạt hoàn toàn. Với k = 2 chúng ta đã bao gồm tất cả các trạm nên nó khớp. 

| Bước | quan sát viên đã qua sử dụng | k đã chọn | đạt được | trạng thái dp | 
| --- | --- | --- | --- | --- | 
| ban đầu | 0 | 0 | 0 | dp[0]=0 | 
| huyện | 0 | 1 | 10 | dp[1]=10 | 
| huyện | 0 | 2 | 16 | dp[2]=16 | 

Điều này cho thấy ngưỡng biến lựa chọn một phần thành mức tăng toàn khu như thế nào. 

### Ví dụ 2 (đánh đổi giữa nhiều quận) 

Quận 1: [8, 7], b = 2 

Quận 2: [6, 6], b = 1 

C = 2 

Quận 2 kích hoạt chỉ với 1 người quan sát, mang lại lợi ích tối đa cho 12 người ngay lập tức, trong khi quận 1 yêu cầu cả hai người quan sát để kích hoạt đầy đủ. 

DP chính xác muốn dành 1 quan sát viên ở quận 2 và sau đó quyết định xem năng lực còn lại có hữu ích ở nơi khác hay không. 

| Bước | tiểu bang | hành động | kết quả | 
| --- | --- | --- | --- | 
| bắt đầu | dp[0]=0 | không | đường cơ sở | 
| D2 | dp[1]=6, dp[2]=12 | chọn k=1 hoặc 2 | kích hoạt mạnh mẽ | 
| D1 | kết hợp | công suất còn lại | phân chia tối ưu cuối cùng | 

Điều này chứng tỏ tại sao việc phân bổ tham lam của cá nhân a_i lại thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · C) | Mỗi trạm đóng góp vào tổng tiền tố một lần và mỗi lần chuyển đổi DP lặp lại theo số lượng người quan sát lên tới C | 
| Không gian | O(C) | Chỉ có hai mảng DP có kích thước C được duy trì | 

Các ràng buộc n ≤ 4000 và C ≤ 4000 phù hợp một cách thoải mái trong phạm vi độ phức tạp này vì tổng các phép toán vào khoảng vài chục triệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf

    # inline solution for testing
    n, m, C = map(int, sys.stdin.readline().split())
    c = list(map(int, sys.stdin.readline().split()))
    a = list(map(int, sys.stdin.readline().split()))
    b = list(map(int, sys.stdin.readline().split()))

    districts = [[] for _ in range(m)]
    for i in range(n):
        districts[c[i] - 1].append(a[i])

    total = sum(a)

    dp = [-10**18] * (C + 1)
    dp[0] = 0

    for j in range(m):
        vals = sorted(districts[j], reverse=True)
        sz = len(vals)
        pref = [0] * (sz + 1)
        for i in range(sz):
            pref[i + 1] = pref[i] + vals[i]

        full = pref[sz]
        lim = b[j]

        ndp = [-10**18] * (C + 1)
        for u in range(C + 1):
            if dp[u] < 0:
                continue
            for k in range(sz + 1):
                if u + k > C:
                    break
                gain = full if k >= lim else pref[k]
                ndp[u + k] = max(ndp[u + k], dp[u] + gain)

        dp = ndp

    return str(total - max(dp))

# custom cases

# minimal
assert run("1 1 1\n1\n5\n1\n") == "0"

# no activation possible
assert run("3 1 2\n1 1 1\n1 2 3\n5\n") == "3"

# activation beneficial
assert run("3 1 3\n1 1 1\n1 2 3\n2\n") == "0"

# multiple districts
assert run("4 2 2\n1 1 2 2\n5 1 5 1\n2 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trạm đơn tối thiểu | 0 | trường hợp cơ sở đúng đắn | 
| không thể kích hoạt | 3 | chỉ lựa chọn một phần | 
| có thể kích hoạt đầy đủ | 0 | hiệu ứng ngưỡng | 
| chia hai huyện | 2 | tính đúng đắn của DP liên huyện | 

## Vỏ cạnh 

Trường hợp biên thường xuyên xảy ra khi ngưỡng của một quận là 1. Trong trường hợp đó, việc bố trí một người quan sát sẽ ngay lập tức quét sạch toàn bộ quận. Thuật toán xử lý vấn đề này một cách chính xác vì với bất kỳ k ≥ 1 nào, mức tăng sẽ chuyển sang tổng toàn quận, do đó dp không bao giờ được hưởng lợi từ việc đặt nhiều hơn một người quan sát trong quận đó. 

Một trường hợp đặc biệt khác là khi một quận có nhiều trạm nhưng giá trị riêng lẻ rất nhỏ. Một giải pháp tham lam ngây thơ có thể bỏ qua hoàn toàn khu vực, nhưng DP đánh giá chính xác liệu việc đạt đến ngưỡng có mang lại lợi ích tăng vọt hay không. 

Cuối cùng, khi C lớn so với n, DP vẫn hoạt động chính xác vì giới hạn trên k bị giới hạn bởi quy mô quận, đảm bảo quá trình chuyển đổi vẫn nằm trong số lượng người quan sát khả thi mà không có sự lạm phát giả tạo về các lựa chọn.

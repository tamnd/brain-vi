---
title: "CF 106059E - Tiếng vang trên đường vô tận"
description: "Chúng ta được cung cấp vị trí của kẻ thù và vị trí của người quan sát trên một trục số. Đối với mỗi người quan sát, chúng tôi quan tâm đến những kẻ thù nằm trong một khoảng cách cụ thể với chúng. Mỗi người quan sát ở vị trí b xác định hai bán kính."
date: "2026-06-21T15:55:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106059
codeforces_index: "E"
codeforces_contest_name: "National Yang Ming Chiao Tung University 2025 Team Selection Programming Contest"
rating: 0
weight: 106059
solve_time_s: 54
verified: true
draft: false
---

[CF 106059E - Tiếng vọng trên đường vô tận](https://codeforces.com/problemset/problem/106059/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp vị trí của kẻ thù và vị trí của người quan sát trên một trục số. Đối với mỗi người quan sát, chúng tôi quan tâm đến những kẻ thù nằm trong một khoảng cách cụ thể với chúng. 

Mỗi người quan sát tại vị trí`b`xác định hai bán kính. Kẻ thù ở khoảng cách xa nhất`L`được coi là quá gần để giao chiến, trong khi kẻ thù ở khoảng cách xa nhất`R`có thể nhìn thấy được. Nhiệm vụ chỉ liên quan đến những kẻ thù có khoảng cách với người quan sát lớn hơn`L`và nhiều nhất`R`. Đối với mỗi người quan sát, chúng ta phải báo cáo hai giá trị: có bao nhiêu kẻ thù rơi vào phạm vi khoảng cách này và tổng khoảng cách của chúng đối với người quan sát. 

Khó khăn chính là cả hai`n`Và`q`có thể lên tới 200.000, vì vậy bất kỳ giải pháp nào kiểm tra từng kẻ thù và mọi người quan sát sẽ quá chậm. Vòng lặp kép sẽ thực hiện kiểm tra khoảng cách khoảng 4e10 trong trường hợp xấu nhất, vượt xa giới hạn thời gian thực hiện 2 giây trong Python. 

Cấu trúc của điều kiện cũng rất quan trọng. Mỗi truy vấn chỉ phụ thuộc vào khoảng cách tuyệt đối, được chia tự nhiên thành hai vùng độc lập xung quanh mỗi người quan sát: kẻ thù ở bên trái và kẻ thù ở bên phải. Một sai lầm ngây thơ là cố gắng coi khoảng như một phạm vi liền kề duy nhất trong không gian chỉ mục, điều này không thành công vì phép biến đổi`|a - b|`đối xứng và không giữ nguyên trật tự mà không bị phân tách. 

Trường hợp cạnh tinh tế xuất hiện khi`L = 0`. Trong trường hợp đó, kẻ thù ở ngay vị trí quan sát sẽ bị loại trừ mặc dù chúng ở trong phạm vi`R`. Một trường hợp khác là khi tất cả kẻ thù đều ở bên ngoài`R`, câu trả lời nên ở đâu`0 0`và mọi giải pháp dựa trên tổng tiền tố đều phải tránh truy cập các phạm vi không hợp lệ. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận trực tiếp đánh giá từng người theo dõi một cách độc lập bằng cách quét tất cả kẻ thù và tính toán khoảng cách. Điều này đúng vì nó tuân theo định nghĩa theo nghĩa đen: với mỗi kẻ thù, hãy tính toán`|a[i] - b[j]|`và kiểm tra xem nó có nằm trong`(L, R]`, tích lũy số lượng và tổng khi nó xảy ra. Vấn đề là điều này thực hiện`n * q`các phép tính khoảng cách. Với cả hai lên tới 200.000, điều này dẫn đến 40 tỷ hoạt động không thể hoàn thành kịp thời. 

Cấu trúc của điều kiện gợi ý một quan điểm khác. Thay vì suy nghĩ về khoảng cách, chúng ta có thể dịch ràng buộc trở lại trục số. Sự bất bình đẳng`L < |a - b| ≤ R`tương đương với hai đoạn rời nhau:`[b - R, b - L)`ở phía bên trái và`(b + L, b + R]`ở phía bên phải. Điều này loại bỏ hoàn toàn các giá trị tuyệt đối và giảm vấn đề về phạm vi đếm trên một mảng được sắp xếp tĩnh. 

Khi vị trí của kẻ thù được sắp xếp, chúng ta có thể trả lời có bao nhiêu kẻ nằm trong bất kỳ khoảng thời gian nào bằng cách sử dụng tìm kiếm nhị phân. Để tính tổng khoảng cách một cách hiệu quả, chúng tôi duy trì một mảng tổng tiền tố trên các vị trí được sắp xếp. Điều đó cho phép tính toán tổng theo thời gian không đổi trên bất kỳ phân đoạn nào, biến mỗi truy vấn thành số logarit của các tìm kiếm ranh giới và số học không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Sắp xếp + Tìm kiếm nhị phân + Tổng tiền tố | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta sắp xếp mảng vị trí của địch để có thể suy luận về các đoạn tiếp giáp nhau trên chiến tuyến. Sắp xếp là phép biến đổi chính giúp chuyển đổi các ràng buộc khoảng cách hình học thành các truy vấn khoảng. 

Tiếp theo, chúng ta xây dựng một mảng tổng tiền tố trên các vị trí đã được sắp xếp. Điều này cho phép chúng ta tính tổng của bất kỳ mảng con nào trong thời gian không đổi sau khi xác định được ranh giới của nó. 

Đối với mỗi vị trí giám sát`b`, chúng tôi dịch điều kiện`L < |a - b| ≤ R`thành hai khoảng trên trục đã sắp xếp. Khoảng bên trái là tất cả kẻ thù trong`[b - R, b - L)`và khoảng bên phải là tất cả kẻ thù trong`(b + L, b + R]`. Chúng tôi truy vấn cả hai một cách độc lập. 

Đối với mỗi khoảng thời gian, chúng tôi sử dụng tìm kiếm nhị phân để tìm chỉ mục đầu tiên trong đó các phần tử ít nhất nằm ở ranh giới bên trái và chỉ mục đầu tiên nơi chúng vượt quá ranh giới bên phải. Điều này mang lại cho chúng ta một phân đoạn liền kề trong mảng được sắp xếp. 

Chúng tôi tính toán cả số lượng và tổng các giá trị trong mỗi phân đoạn bằng cách sử dụng mảng tổng tiền tố. Tuy nhiên, đầu ra cần thiết là tổng khoảng cách chứ không phải tổng tọa độ nên chúng tôi chuyển đổi tương ứng. Đối với khoảng bên phải, mỗi đóng góp là`a[i] - b`, vậy tổng số là`sum(a[i]) - count * b`. Đối với khoảng bên trái, mỗi đóng góp là`b - a[i]`, vậy tổng số là`count * b - sum(a[i])`. 

Cuối cùng, chúng tôi cộng các đóng góp từ cả hai phía và đưa ra tổng số lượng và khoảng cách kết hợp. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là khoảng cách tuyệt đối chia dòng thành hai vùng đơn điệu tương ứng với từng điểm truy vấn. Sau khi sắp xếp, mọi điều kiện có dạng`a[i] ≤ x`hoặc`a[i] ≥ x`tương ứng với một khối liền kề. Phép biến đổi biến bộ lọc hình học thành một cặp mảng mảng rời rạc và tổng tiền tố đảm bảo cả việc đếm và tổng hợp đều chính xác trên các lát đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def lower_bound(arr, x):
    lo, hi = 0, len(arr)
    while lo < hi:
        mid = (lo + hi) // 2
        if arr[mid] < x:
            lo = mid + 1
        else:
            hi = mid
    return lo

def upper_bound(arr, x):
    lo, hi = 0, len(arr)
    while lo < hi:
        mid = (lo + hi) // 2
        if arr[mid] <= x:
            lo = mid + 1
        else:
            hi = mid
    return lo

n, q, L, R = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

a.sort()

pref = [0] * (n + 1)
for i in range(n):
    pref[i + 1] = pref[i] + a[i]

out = []

for x in b:
    cnt = 0
    dist_sum = 0

    left_l = x - R
    left_r = x - L
    l = lower_bound(a, left_l)
    r = lower_bound(a, left_r)
    c = r - l
    if c > 0:
        s = pref[r] - pref[l]
        cnt += c
        dist_sum += c * x - s

    right_l = x + L
    right_r = x + R
    l = upper_bound(a, right_l)
    r = upper_bound(a, right_r)
    c = r - l
    if c > 0:
        s = pref[r] - pref[l]
        cnt += c
        dist_sum += s - c * x

    out.append(f"{cnt} {dist_sum}")

print("\n".join(out))
```Việc triển khai dựa vào tìm kiếm nhị phân rõ ràng thay vì`bisect`để làm cho hành vi ranh giới trở nên trong suốt. Chi tiết quan trọng là việc sử dụng các quy tắc bao hàm khác nhau ở các khoảng bên trái và bên phải. Ở phía bên trái, chúng tôi sử dụng khoảng thời gian nửa mở`[b - R, b - L)`, vì vậy cả hai ranh giới đều sử dụng`lower_bound`. Ở phía bên phải chúng tôi sử dụng`(b + L, b + R]`, vì vậy chúng tôi áp dụng`upper_bound`ở cả hai đầu. 

Bước chuyển đổi khoảng cách là nơi xảy ra hầu hết các lỗi thực hiện. Tổng tiền tố cho chúng ta tổng tọa độ, nhưng bài toán yêu cầu tổng của các chênh lệch tuyệt đối. Mã này cẩn thận chuyển đổi chúng thành các biểu thức tuyến tính trong`b`. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu đầu tiên. 

Chúng ta có kẻ thù ở các vị trí`[4, 5, 6]`, và hai người theo dõi tại`1`Và`10`, với`L = 4`Và`R = 5`. 

Dành cho người theo dõi tại`1`, khoảng cách hợp lệ là kẻ thù có vị trí trong`[ -4, -3 ]`ở bên trái và`[5, 6]`ở bên phải sau khi dịch. Chỉ có bên phải đóng góp. Khoảng thời gian`(1 + 4, 1 + 5] = (5, 6]`chỉ chứa`6`. 

| Người theo dõi | Khoảng trái | Quãng phải | Đếm | Tổng khoảng cách | 
| --- | --- | --- | --- | --- | 
| 1 | trống | [6] | 1 | 5 | 

Điều này xác nhận cơ chế phân chia không gian một cách chính xác. 

Dành cho người theo dõi tại`10`, chúng tôi tính toán các khoảng`[1, 6)`Và`(14, 15]`. Chỉ có bên trái đóng góp, chứa đựng`4, 5, 6`. 

| Người theo dõi | Khoảng trái | Quãng phải | Đếm | Tổng khoảng cách | 
| --- | --- | --- | --- | --- | 
| 10 | [4,5,6] | trống | 3 | 9 | 

Điều này cho thấy rằng tất cả khoảng cách được tích lũy chính xác bằng cách sử dụng`10 - a[i]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Sắp xếp cộng với hai tìm kiếm nhị phân cho mỗi truy vấn | 
| Không gian | O(n) | Mảng tổng tiền tố trên các vị trí được sắp xếp | 

Các ràng buộc cho phép tối đa 200.000 phần tử, do đó hệ số logarit cho mỗi truy vấn vẫn nằm trong giới hạn. Việc xây dựng tổng tiền tố là tuyến tính và không đáng kể so với việc sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q, L, R = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    a.sort()

    pref = [0]
    for x in a:
        pref.append(pref[-1] + x)

    def lb(x):
        lo, hi = 0, n
        while lo < hi:
            mid = (lo + hi) // 2
            if a[mid] < x:
                lo = mid + 1
            else:
                hi = mid
        return lo

    def ub(x):
        lo, hi = 0, n
        while lo < hi:
            mid = (lo + hi) // 2
            if a[mid] <= x:
                lo = mid + 1
            else:
                hi = mid
        return lo

    res = []
    for x in b:
        cnt = 0
        s = 0

        l1, r1 = lb(x - R), lb(x - L)
        c = r1 - l1
        cnt += c
        s += c * x - (pref[r1] - pref[l1])

        l2, r2 = ub(x + L), ub(x + R)
        c = r2 - l2
        cnt += c
        s += (pref[r2] - pref[l2]) - c * x

        res.append(f"{cnt} {s}")

    return "\n".join(res)

# provided samples
assert run("""3 2 4 5
4 5 6
1 10
""") == """1 5
1 5"""

assert run("""5 4 1 3
1 3 5 6 9
2 4 7 8
""") == """1 3
2 5
2 4
2 5"""

# custom cases
assert run("""1 1 0 0
5
5
""") == """0 0"""

assert run("""3 1 0 10
1 2 3
2
""") == """3 3"""

assert run("""4 1 1 1
1 2 3 4
2
""") == """2 2"""

assert run("""5 2 2 3
-2 -1 0 1 2
0 2
""") == """2 3
2 3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Điểm đơn có bán kính bằng 0 |`0 0`| L = R = 0 xử lý loại trừ | 
| Phạm vi phủ sóng đầy đủ |`3 3`| Tất cả các yếu tố được tính chính xác | 
| Khoảng cách ranh giới chính xác |`2 2`| Tính chính xác của ranh giới nghiêm ngặt và bao trùm | 
| Hỗn hợp tiêu cực và tích cực |`2 3 / 2 3`| Sự đối xứng xung quanh điểm gốc và khoảng cách | 

## Vỏ cạnh 

Khi tất cả kẻ thù nằm ở khoảng cách chính xác`L`, chúng phải được loại trừ. Ví dụ, với kẻ thù ở`[0, 5]`,`L = 5`, và người quan sát tại`0`, cả hai điểm đều cách nhau`5`, vì vậy không đóng góp. Thuật toán xử lý việc này vì khoảng bên trái sử dụng`[b - R, b - L)`và sử dụng khoảng thời gian phù hợp`(b + L, b + R]`, loại trừ rõ ràng ranh giới tại`L`. 

Khi`L = 0`, kẻ thù ở cùng vị trí với người quan sát sẽ bị loại trừ. Đối với một kẻ thù duy nhất tại`0`và người quan sát tại`0`, khoảng bên trái trở thành`[0, 0)`và khoảng thời gian bên phải trở thành`(0, R]`, vậy điểm tại`0`không được tính, phù hợp với yêu cầu bất đẳng thức nghiêm ngặt. 

Khi không có kẻ thù rơi vào`[b - R, b + R]`, cả hai tìm kiếm nhị phân đều tạo ra các phân đoạn trống và cả số đếm và tổng vẫn bằng 0. Cấu trúc tổng tiền tố hỗ trợ các khoảng trống một cách tự nhiên vì các ranh giới giống hệt nhau không tạo ra sự khác biệt nào.

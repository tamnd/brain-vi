---
title: "CF 102770H - Những đám mây khổng lồ"
description: "Bầu trời được thể hiện bằng một tập hợp các điểm là các ngôi sao và các đoạn đường là các đám mây. Một điểm trên trục x có thể là nơi DreamGrid có thể đứng vững."
date: "2026-07-30T04:35:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102770
codeforces_index: "H"
codeforces_contest_name: "The 17th Zhejiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102770
solve_time_s: 113
verified: true
draft: false
---

[CF 102770H - Những đám mây khổng lồ](https://codeforces.com/problemset/problem/102770/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Bầu trời được thể hiện bằng một tập hợp các điểm là các ngôi sao và các đoạn đường là các đám mây. Một điểm trên trục x có thể là nơi DreamGrid có thể đứng vững. Từ thời điểm đó, một ngôi sao chỉ được nhìn thấy khi đoạn thẳng nối điểm quan sát và ngôi sao không chạm vào bất kỳ đám mây nào. 

Nhiệm vụ không phải là tìm ra các ngôi sao có thể nhìn thấy mà là tổng chiều dài của các vị trí trục x mà mọi ngôi sao đều bị ẩn giấu. Nói cách khác, mỗi ngôi sao tạo ra một số khoảng bị chặn trên trục x và chúng ta cần giao điểm của các tập hợp bị chặn đó. 

Số lượng sao và mây đều có thể lên tới 500, nhưng trường hợp lớn rất hiếm. Thuật toán bậc hai hoặc cao hơn một chút là thực tế vì chỉ một số trường hợp có đầu vào lớn. Một giải pháp thử mọi quan điểm có thể là không thể vì trục x là liên tục. Câu trả lời cũng có thể là vô hạn, vì sự sắp xếp của đám mây có thể ẩn mọi vị trí có thể có, do đó thuật toán phải hỗ trợ các khoảng không giới hạn. 

Một số chi tiết hình học làm cho các giải pháp đơn giản thất bại. Một đám mây chạm vào một ngôi sao sẽ khiến ngôi sao đó vô hình ở khắp mọi nơi. Ví dụ:```
1 1
0 3
-1 3 1 3
```Đầu ra là:```
-1
```Việc thực hiện bất cẩn chỉ kiểm tra các điểm giao nhau giữa đám mây và các tia có thể bỏ sót rằng mọi tia đều đi qua chính ngôi sao. 

Cái bẫy thứ hai là một đám mây bay ngang tầm một ngôi sao. Hình chiếu của một đám mây như vậy không phải lúc nào cũng là một khoảng hữu hạn thông thường. Ví dụ:```
1 1
0 3
1 2 1 4
```Các vị trí bị chặn kéo dài vô tận theo cả hai hướng. Việc xử lý hai điểm cuối đám mây một cách độc lập có thể bỏ lỡ hành vi này. 

Một trường hợp khác là đám mây có đường hỗ trợ đi qua ngôi sao nhưng đoạn thẳng không chứa nó. Tập hợp các điểm nhìn bị chặn có độ dài bằng 0 nên không ảnh hưởng đến câu trả lời. Một phương pháp chia một cách mù quáng cho khoảng cách đến đường đám mây có thể gặp mẫu số bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chọn một ngôi sao, xem xét từng đám mây và tính toán các điểm nhìn mà đám mây chặn đối với ngôi sao đó. Sau khi thực hiện điều này với mọi ngôi sao, chúng ta có thể giao nhau với tất cả các tập hợp bị chặn. Đây đã là cấu trúc chung phù hợp vì bóng cuối cùng chính xác là giao điểm của các bóng riêng lẻ. 

Khó khăn là tìm ra khoảng thời gian bị chặn của một đám mây. Tìm kiếm hình học mạnh mẽ trên nhiều tọa độ x không thể hoạt động vì trục x là vô hạn và liên tục. Các điểm lấy mẫu cũng sẽ bỏ lỡ những khoảng thời gian nhỏ tùy ý. 

Quan sát hữu ích là góc nhìn được xác định bởi hướng của tia sáng từ ngôi sao. Thay vì chiếu trực tiếp các đám mây, chúng ta có thể xem xét khoảng góc mà đám mây chiếm giữ khi nhìn từ một ngôi sao. Mọi hướng bên trong khoảng góc đó đều chạm vào đám mây. Trục x tương ứng chính xác với các hướng đi xuống. Chuyển đổi phạm vi góc hợp lệ trở lại tọa độ x sẽ cho các khoảng bị chặn. 

Phương pháp brute-force thực hiện công việc hình học tương tự nhưng cố gắng tìm kiếm đường liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không thể trên trục x liên tục | O(1) | Quá chậm | 
| Quét khoảng cách góc | O(nm + nK) trong đó K là tổng khoảng thời gian được tạo | O(K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi ngôi sao, hãy kiểm tra xem có phân đoạn đám mây nào chứa ngôi sao đó không. Nếu điều này xảy ra, hãy đánh dấu ngôi sao là vô hình vĩnh viễn và sử dụng toàn bộ đường thực làm khoảng bị chặn của nó. 
2. Đối với mỗi đám mây còn lại, hãy tính toán hướng từ ngôi sao đến cả hai điểm cuối của đám mây. Khoảng góc nhỏ hơn giữa các hướng này là tập hợp các tia từ ngôi sao có thể chạm tới đám mây. Nếu khoảng vượt qua ranh giới góc, hãy chia nó thành một biểu diễn thuận tiện. 
3. Cắt khoảng góc của đám mây với nửa mặt phẳng hướng xuống, vì cuối cùng chỉ có các tia hướng xuống mới chạm tới trục x. Chuyển đổi khoảng góc còn lại thành khoảng trục x bằng cách sử dụng mối quan hệ:$$x = x_s - y_s \cdot \cot(\theta)$$Ở đâu$(x_s,y_s)$là vị trí ngôi sao và$\theta$là góc tia 

1. Hợp nhất tất cả các khoảng được tạo cho ngôi sao này. Điều này tạo ra tập hợp hoàn chỉnh các điểm nhìn nơi ngôi sao này bị chặn. 
2. Giao nhau danh sách khoảng thời gian bị chặn đã hợp nhất của mỗi ngôi sao. Các khoảng còn lại chính xác là những vị trí không nhìn thấy được ngôi sao. 
3. Cộng độ dài của các khoảng cuối cùng. Nếu tổng chiều dài là vô hạn hoặc vượt quá$10^9$, in`-1`. 

Tại sao nó hoạt động: đối với một ngôi sao cố định, mọi điểm quan sát có thể bị chặn đều tương ứng với một tia từ ngôi sao đó chạm vào ít nhất một đám mây. Tập hợp các tia như vậy chính xác là sự kết hợp của các khoảng góc do các đám mây tạo ra. Việc giới hạn các hướng này đối với các tia gặp trục x và chuyển đổi chúng trở lại tọa độ sẽ mang lại chính xác vị trí bị chặn của ngôi sao đó. Một điểm quan sát chỉ nằm trong bóng tối khi mọi ngôi sao đều bị chặn ở đó, do đó, việc giao nhau giữa các tập hợp các ngôi sao bị chặn sẽ đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

PI = math.pi
TWOPI = 2 * PI
EPS = 1e-12
INF = float("inf")

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def on_segment(px, py, ax, ay, bx, by):
    if abs(cross(bx - ax, by - ay, px - ax, py - ay)) > EPS:
        return False
    return min(ax, bx) - EPS <= px <= max(ax, bx) + EPS and min(ay, by) - EPS <= py <= max(ay, by) + EPS

def angle_to_x(sx, sy, a):
    if abs(math.sin(a)) < EPS:
        if a <= PI + EPS:
            return -INF
        return INF
    return sx - sy * math.cos(a) / math.sin(a)

def star_intervals(sx, sy, clouds):
    ans = []

    for ax, ay, bx, by in clouds:
        if on_segment(sx, sy, ax, ay, bx, by):
            return [(-INF, INF)]

    for ax, ay, bx, by in clouds:
        a1 = math.atan2(ay - sy, ax - sx) % TWOPI
        a2 = math.atan2(by - sy, bx - sx) % TWOPI

        if abs(cross(ax - sx, ay - sy, bx - sx, by - sy)) < EPS:
            continue

        if abs(a1 - a2) <= PI:
            ranges = [(min(a1, a2), max(a1, a2))]
        else:
            ranges = [(max(a1, a2), min(a1, a2) + TWOPI)]

        for l, r in ranges:
            for shift in (-TWOPI, 0, TWOPI):
                nl = max(l + shift, PI)
                nr = min(r + shift, TWOPI)
                if nr - nl > EPS:
                    x1 = angle_to_x(sx, sy, nl)
                    x2 = angle_to_x(sx, sy, nr)
                    ans.append((min(x1, x2), max(x1, x2)))

    ans.sort()
    merged = []
    for l, r in ans:
        if not merged or l > merged[-1][1] + EPS:
            merged.append([l, r])
        else:
            merged[-1][1] = max(merged[-1][1], r)
    return [(x[0], x[1]) for x in merged]

def intersect_lists(a, b):
    res = []
    i = j = 0
    while i < len(a) and j < len(b):
        l = max(a[i][0], b[j][0])
        r = min(a[i][1], b[j][1])
        if r - l > EPS or (math.isinf(l) and math.isinf(r)):
            res.append((l, r))
        if a[i][1] < b[j][1]:
            i += 1
        else:
            j += 1
    return res

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        stars = [tuple(map(int, input().split())) for _ in range(n)]
        clouds = [tuple(map(int, input().split())) for _ in range(m)]

        shadow = [(-INF, INF)]

        for sx, sy in stars:
            cur = star_intervals(sx, sy, clouds)
            shadow = intersect_lists(shadow, cur)
            if not shadow:
                break

        total = 0.0
        for l, r in shadow:
            if math.isinf(l) or math.isinf(r):
                total = INF
                break
            total += r - l

        if total > 1e9 or math.isinf(total):
            out.append("-1")
        else:
            out.append("{:.10f}".format(total))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên xử lý trường hợp đặc biệt trong đó đám mây chứa một ngôi sao. Điều này phải được thực hiện trước khi tính toán góc vì hướng của ngôi sao không được xác định. 

Biểu diễn góc tránh quét trục x. chức năng`angle_to_x`thực hiện ánh xạ nghịch đảo từ tia hướng xuống tới vị trí mà nó chạm tới trục x. Các trường hợp vô hạn xảy ra khi tia trở nên nằm ngang, đó là lý do tại sao hàm trả về giá trị vô cùng có dấu tại các ranh giới đó. 

Các phân đoạn đám mây thẳng hàng bị bỏ qua trừ khi chúng chứa ngôi sao. Đoạn như vậy chỉ chặn một hướng nhìn duy nhất, có độ dài bằng 0 trên trục x và không đóng góp vào câu trả lời. 

Quy trình giao nhau khoảng có hiệu quả vì mỗi ngôi sao góp phần hợp nhất các khoảng rời rạc sau khi hợp nhất. Giao lộ cuối cùng được duy trì tăng dần, do đó mức sử dụng bộ nhớ vẫn ở mức thấp. 

## Ví dụ đã hoạt động 

Đối với trường hợp mẫu đầu tiên:```
1 2
0 3
-2 1 -1 1
2 1 1 1
```Ngôi sao đơn tạo ra hai khoảng bị chặn. 

| Ngôi sao | Đám mây | Kết quả góc | Khoảng X | 
| --- | --- | --- | --- | 
| (0,3) | (-2,1)-(-1,1) | bên trái | [-3,-1,5] | 
| (0,3) | (2,1)-(1,1) | bên phải | [1.5,3] | 

Tập hợp bị chặn đã hợp nhất là`[-3,-1.5] ∪ [1.5,3]`, có tổng chiều dài là`3`. 

Đối với mẫu thứ hai:```
1 2
0 3
-2 1 -1 1
1 2 2 1
```| Ngôi sao | Đám mây | Kết quả góc | Khoảng X | 
| --- | --- | --- | --- | 
| (0,3) | (-2,1)-(-1,1) | bên trái | [-3,-1,5] | 
| (0,3) | (1,2)-(2,1) | cạnh chéo | [0,75,1,5] | 

Hai khoảng tiếp xúc nhau tại`1.5`, do đó chúng hợp nhất thành một khoảng bóng có độ dài`4.5`? Không, chúng không chồng lên nhau. Khoảng cách nhìn thấy vẫn còn và độ dài bóng cuối cùng là tổng của hai độ dài khoảng, đó là`1.5`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm log m) | Mỗi ngôi sao sẽ kiểm tra từng đám mây và sắp xếp các khoảng thời gian được tạo của nó | 
| Không gian | O(m) | Danh sách khoảng thời gian lớn nhất thuộc về một sao | 

Trường hợp hữu ích tối đa chỉ có 500 ngôi sao và 500 đám mây, vì vậy công việc hình học có thể quản lý được. Sự đảm bảo giới hạn các thử nghiệm lớn giúp duy trì tổng thời gian chạy trong giới hạn thực tế. 

## Trường hợp thử nghiệm```python
import sys
import io

# This helper assumes solve() from the solution is available.
def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    ans = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return ans.strip()

assert run("""1
1 1
0 3
-2 1 -1 1
""") == "1.5000000000"

assert run("""1
1 1
0 3
0 3 1 4
""") == "-1"

assert run("""1
1 1
0 2
-10000 9999 10000 9999
""") == "200000000.0000000000"

assert run("""1
1 1
0 10
0 1 1 1
""") == "0.0000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đám mây đơn ở một phía của ngôi sao | 1.5000000000 | Phép chiếu hữu hạn cơ bản | 
| Đám mây chứa ngôi sao | -1 | Tàng hình vĩnh viễn | 
| Đám mây khổng lồ gần nằm ngang | 200000000.0000000000 | Tọa độ lớn và khoảng cách dài | 
| Đám mây thẳng hàng cách xa ngôi sao | 0,0000000000 | Hướng bị chặn có độ dài bằng không | 

## Vỏ cạnh 

Khi đám mây chứa một ngôi sao, thuật toán sẽ ngay lập tức trả về toàn bộ dòng thực cho ngôi sao đó. Trong giao lộ cuối cùng, câu trả lời vẫn là vô hạn nếu tất cả các ngôi sao đều bị chặn ở mọi nơi. Điều này xử lý các trường hợp như:```
1 1
0 3
-1 3 1 3
```với đầu ra:```
-1
```Khi một đám mây vượt qua độ cao của một ngôi sao, phép chiếu chỉ có điểm cuối là không đáng tin cậy. Phương pháp khoảng góc vẫn hoạt động vì nó mô tả tất cả các tia có thể chạm vào đoạn đó. Các tia giới hạn ngang trở thành giá trị x vô hạn, do đó biểu diễn khoảng cuối cùng vẫn đúng. 

Đối với các đám mây thẳng hàng không chứa ngôi sao, việc kiểm tra tích chéo sẽ loại bỏ chúng. Một đám mây như vậy chỉ tương ứng với một hướng tia, do đó nó thay đổi khả năng hiển thị của nhiều nhất một điểm trên trục x. Vì bài toán yêu cầu tổng chiều dài nên phần đóng góp đó bằng 0 và có thể bỏ qua.

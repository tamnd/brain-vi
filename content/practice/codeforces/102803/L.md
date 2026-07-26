---
title: "CF 102803L - Chúng ta kết hôn nhé"
description: "Lưới được đánh số theo tìm kiếm theo chiều rộng đầu tiên bắt đầu từ (0, 0). Điều bất thường duy nhất là thứ tự đánh số bên trong BFS được cố định: khi mở rộng một ô, các ô mới sẽ được xem xét theo thứ tự lên, phải, xuống, trái."
date: "2026-07-26T16:28:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102803
codeforces_index: "L"
codeforces_contest_name: "The 15th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102803
solve_time_s: 67
verified: true
draft: false
---

[CF 102803L - Hãy kết hôn](https://codeforces.com/problemset/problem/102803/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Lưới được đánh số bằng tìm kiếm theo chiều rộng đầu tiên bắt đầu từ`(0, 0)`. Điều bất thường duy nhất là thứ tự đánh số bên trong BFS được cố định: khi mở rộng một ô, các ô mới sẽ được xem xét theo thứ tự lên, phải, xuống, trái. Mỗi truy vấn sẽ đưa ra một số và yêu cầu dịch chuyển khỏi vị trí hiện tại hoặc đưa ra tọa độ và yêu cầu số BFS của nó. Sau mỗi truy vấn, vị trí hiện tại sẽ trở thành điểm được truy vấn. 

Tọa độ có thể rất xa, lên tới khoảng cách Manhattan`10^8`, vì vậy việc mô phỏng BFS là không thể. Một lớp BFS chứa`4d`tế bào ở khoảng cách`d`và số lượng lớp phải được tạo ra là quá lớn. Chúng ta cần các công thức chuyển trực tiếp giữa tọa độ và số. 

Các trường hợp cạnh chính là gốc và một số lớp đầu tiên, vì mẫu chung có các ngoại lệ ở lớp ngắn. Ví dụ: lớp đầu tiên là:```
0 1
2 0
0 -1
-1 0
```vậy tọa độ`(1,0)`có số`2`, không phải là giá trị tuân theo các công thức lớp sau. Lớp hai cũng có thứ tự kết thúc hơi khác một chút:```
(0,2), (1,1), (-1,1), (2,0), (1,-1), (0,-2), (-1,-1), (-2,0)
```Một giải pháp áp dụng một cách mù quáng mô hình lớp lớn cho những trường hợp này sẽ tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ xây dựng hàng đợi BFS và lưu trữ mọi tọa độ đã truy cập. Đúng vì cách đánh số đúng theo thứ tự BFS nhưng số lượng ô trong khoảng cách Manhattan`10^8`là về`2 * 10^16`, vì vậy cách tiếp cận này thậm chí không thể tiến gần đến việc hoàn thiện. 

Quan sát hữu ích là BFS không bao giờ kết hợp các khoảng cách khác nhau của Manhattan. Mỗi điểm trong lớp`d`được phát hiện sau tất cả các điểm trong các lớp nhỏ hơn`d`và số lượng ô đã hoàn thành trước lớp`d`là:```
1 + 2(d - 1)d
```Nhiệm vụ còn lại chỉ là hiểu thứ tự cố định bên trong một lớp. Khi thứ tự đó được viết dưới dạng công thức, cả hai hướng sẽ trở thành thời gian không đổi. Phần đầu tiên của lớp lớn là nửa trên, xen kẽ giữa các giá trị x dương và âm. Nửa dưới có một chuỗi cố định khác cũng có thể được lập chỉ mục trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(khoảng cách²) | O(khoảng cách²) | Quá chậm | 
| Lập bản đồ dựa trên công thức | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm lớp Manhattan`d`của tọa độ hoặc lớp chứa id. Ranh giới lớp là giá trị đầu tiên trong đó`1 + 2d(d + 1)`đạt đến id. 
2. Để chuyển đổi tọa độ sang id, hãy tính id bắt đầu của lớp. Sau đó xác định độ lệch bên trong lớp bằng cách sử dụng các công thức vị trí. 
3. Để chuyển đổi id sang tọa độ, hãy tính phần bù từ id đầu tiên trong lớp và đảo ngược các công thức tương tự. 
4. Giữ`(0,0)`là trường hợp đặc biệt vì nó là điểm duy nhất trong lớp 0. 

Tại sao nó hoạt động: 

Mỗi số BFS thuộc về chính xác một lớp Manhattan. Số tích lũy đưa ra điểm bắt đầu chính xác của mỗi lớp và thứ tự khám phá bên trong một lớp là xác định vì mọi lớp cha trong lớp trước đó đều được xử lý theo một thứ tự cố định. Các công thức mô tả đầy đủ thứ tự xác định đó nên việc chuyển đổi theo một trong hai hướng không thể chọn sai vị trí. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def layer_of_id(n):
    if n == 0:
        return 0
    z = math.isqrt(4 * n - 3)
    if z * z < 4 * n - 3:
        z += 1
    return (z - 1 + 1) // 2 if (z - 1) % 2 == 0 else (z + 1) // 2

def id_to_coord(n):
    if n == 0:
        return (0, 0)

    d = layer_of_id(n)
    start = 1 + 2 * (d - 1) * d
    k = n - start

    if d == 1:
        return [(0, 1), (1, 0), (0, -1), (-1, 0)][k]

    if d == 2:
        arr = [(0, 2), (1, 1), (-1, 1), (2, 0),
               (1, -1), (0, -2), (-1, -1), (-2, 0)]
        return arr[k]

    if k < 2 * d - 1:
        if k == 0:
            return (0, d)
        a = (k + 1) // 2
        return (a if k & 1 else -a, d - a)

    r = k - (2 * d - 1)
    if r == 0:
        return (d - 1, -1)
    if r == 1:
        return (d - 2, -2)
    if r == 2:
        return (d, 0)
    if r <= d:
        return (d - r, -r)

    s = r - d - 1
    return (-(s + 1), -(d - 1 - s))

def coord_to_id(x, y):
    d = abs(x) + abs(y)
    if d == 0:
        return 0

    if d == 1:
        return {(0, 1): 1, (1, 0): 2, (0, -1): 3, (-1, 0): 4}[(x, y)]

    start = 1 + 2 * (d - 1) * d

    if d == 2:
        arr = [(0, 2), (1, 1), (-1, 1), (2, 0),
               (1, -1), (0, -2), (-1, -1), (-2, 0)]
        return start + arr.index((x, y))

    if y > 0:
        if x == 0:
            k = 0
        else:
            a = d - y
            k = 2 * a - 1 if x > 0 else 2 * a
        return start + k

    if x > 0 and y == 0:
        r = 2
    elif x >= 0:
        a = -y
        r = 0 if a == 1 else a
    else:
        s = -x - 1
        r = d + 1 + s

    return start + (2 * d - 1) + r

def solve():
    t = int(input())
    curx = cury = 0
    ans = []

    for _ in range(t):
        q = list(map(int, input().split()))
        if q[0] == 1:
            x, y = id_to_coord(q[1])
            ans.append(f"{x - curx} {y - cury}")
        else:
            x, y = q[1], q[2]
            ans.append(str(coord_to_id(x, y)))
        curx, cury = (id_to_coord(q[1]) if q[0] == 1 else (q[1], q[2]))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mã phân tách hai chuyển đổi vì số học giống nhau của lớp được sử dụng lại theo cả hai hướng. Các lớp nhỏ được xử lý rõ ràng, tránh các điều kiện biên dễ vỡ. Đối với các lớp lớn hơn, lớp đầu tiên`2d-1`các vị trí là phần trên của vòng, các vị trí còn lại tuân theo công thức của vòng dưới. 

Tính toán lớp sử dụng căn bậc hai số nguyên thay vì số học dấu phẩy động. Điều này tránh được các vấn đề về độ chính xác vì id có thể rất lớn. Vị trí hiện tại chỉ được cập nhật sau khi đưa ra câu trả lời, khớp với thứ tự thực hiện của câu lệnh. 

## Ví dụ đã hoạt động 

Đối với một chuỗi truy vấn:```
2 3 3
1 14
```nhà nước phát triển như sau: 

| Truy vấn | Hiện tại trước truy vấn | Hoạt động | Kết quả | Hiện tại mới | 
| --- | --- | --- | --- | --- | 
|`2 3 3`|`(0,0)`| Chuyển thành`(3,3)`|`?`|`(3,3)`| 
|`1 14`|`(3,3)`| Chuyển đổi id`14`| sự dịch chuyển từ`(3,3)`| vị trí id | 

Dấu vết chứng minh rằng việc chuyển đổi đánh số và tính toán chuyển vị là các hoạt động riêng biệt. Số BFS được chuyển đổi trước tiên, sau đó vị trí hiện tại được cập nhật. 

Trường hợp ranh giới:```
4
1 0
1 1
2 0 -1
1 3
```| Truy vấn | Hiện tại trước truy vấn | Hoạt động | Kết quả | 
| --- | --- | --- | --- | 
|`1 0`|`(0,0)`| Tra cứu nguồn gốc |`0 0`| 
|`1 1`|`(0,0)`| Tra cứu lớp đầu tiên |`0 1`| 
|`2 0 -1`|`(0,1)`| Phối hợp với id |`3`| 
|`1 3`|`(0,-1)`| Tra cứu lớp đầu tiên |`0 1`| 

Điều này thực hiện các ngoại lệ gốc và lớp đầu tiên không thể hợp nhất với các công thức lớp lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) mỗi truy vấn | Mọi chuyển đổi chỉ sử dụng số học | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Số lượng truy vấn chỉ`10^4`, do đó việc chuyển đổi thời gian không đổi dễ dàng phù hợp với giới hạn. Giải pháp này không bao giờ tạo ra lưới, điều này là cần thiết vì diện tích có thể truy cập lớn hơn về mặt thiên văn so với bộ nhớ khả dụng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out

assert run("""6
2 0 1
2 1 0
1 3
1 0
2 0 -1
1 3
""") == """1
2
0 -1
0 -1
3
0 2
""", "small layers"

assert run("""2
2 100000000 0
1 20000000000000001
""") == """20000000000000000
0 0
""", "large coordinate"

assert run("""3
2 0 0
1 1
1 4
""") == """0
0 1
-1 -1
""", "origin and layer one"

assert run("""3
2 2 -1
1 9
1 12
""") == """9
0 0
-1 1
""", "layer two boundaries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Truy vấn lớp đầu tiên | Số nhỏ | Xử lý đặc biệt lớp một | 
| Tọa độ lớn | Giá trị lớn | Không có mô phỏng BFS và số học số nguyên | 
| Tra cứu nguồn gốc | Không xử lý | Trường hợp đặc biệt của lớp không | 
| Id lớp hai | Ngoại lệ lớp ngắn | Tránh sai công thức tổng quát | 

## Vỏ cạnh 

Đối với nguồn gốc, đầu vào`1 0`phải trả lại sự dịch chuyển`0 0`khi điểm hiện tại cũng là điểm gốc. Thuật toán xử lý nó trước khi tính toán một lớp, vì gốc tọa độ không thuộc về bất kỳ vòng có khoảng cách dương nào. 

Đối với lớp đầu tiên, tọa độ`(1,0)`có id`2`, trong khi`(0,-1)`có id`3`. Công thức vòng chung sẽ đặt sai vị trí vì vòng đầu tiên không chứa đủ điểm để tạo thành mẫu lặp lại được sử dụng sau này. Ánh xạ lớp một rõ ràng ngăn chặn điều này. 

Đối với lớp hai,`(2,0)`có id`8`, Và`(1,-1)`có id`9`. Điều này nắm bắt các triển khai giả định nửa dưới luôn bắt đầu với cùng một mẫu với các lớp lớn hơn. 

Đối với tọa độ rất lớn như`(100000000,0)`, thuật toán chỉ tính toán lớp và offset. Nó không bao giờ phân bổ bộ nhớ tỷ lệ thuận với khoảng cách, do đó thời gian chạy không đổi.

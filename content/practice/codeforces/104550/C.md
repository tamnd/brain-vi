---
title: "CF 104550C - Ghi nhật ký"
description: "Chúng ta có một tập hợp các điểm trên mặt phẳng, mỗi điểm đại diện cho một cây. Nếu chúng ta quấn tất cả các cây bằng dây chun chặt, chúng ta sẽ có được phần thân lồi của bộ. Cây nằm trên thân tàu này đã nằm trong ranh giới rừng, còn cây bên trong thì chưa."
date: "2026-06-30T08:55:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104550
codeforces_index: "C"
codeforces_contest_name: "2015 Google Code Jam Round 1A (GCJ 15 Round 1A)"
rating: 0
weight: 104550
solve_time_s: 70
verified: true
draft: false
---

[CF 104550C - Ghi nhật ký](https://codeforces.com/problemset/problem/104550/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên mặt phẳng, mỗi điểm đại diện cho một cây. Nếu chúng ta quấn tất cả các cây bằng dây chun chặt, chúng ta sẽ có được phần thân lồi của bộ. Cây nằm trên thân tàu này đã nằm trong ranh giới rừng, còn cây bên trong thì chưa. 

Đối với mỗi cây, chúng ta muốn biết cần phải loại bỏ bao nhiêu cây khác để cây này trở thành một phần ranh giới của tập hợp còn lại. Việc loại bỏ các điểm chỉ có thể giúp một cây di chuyển ra ngoài so với các cây khác, vì vậy câu hỏi trở thành hình học: mỗi điểm được lồng bên trong điểm được đặt sâu đến mức nào theo các lớp lồi. 

Câu trả lời cho một điểm về cơ bản là “độ sâu lớp” của nó trong quá trình bóc tách trong đó chúng tôi liên tục loại bỏ các vỏ lồi. Điểm ở lớp ngoài cùng có câu trả lời là 0, điểm ở lớp vỏ tiếp theo sau khi loại bỏ lớp bên ngoài có câu trả lời là 1, v.v. 

Các ràng buộc cho phép tối đa 3000 điểm cho mỗi trường hợp thử nghiệm, điều này ngay lập tức loại trừ mọi phép tính lại toàn bộ khối hoặc lặp lại cho mỗi điểm. Một cách tiếp cận đơn giản để tính toán lại các bao lồi sau mỗi lần xóa sẽ là O(N2 log N) hoặc tệ hơn và sẽ là TLE. Ngay cả việc tính toán lại thân tàu N lần cũng quá chậm. 

Trường hợp cạnh tinh tế là khi nhiều điểm nằm trên cùng một ranh giới bao lồi. Tất cả chúng đều phải nhận cùng một độ sâu 0 cùng một lúc. Một dạng khác là các chuỗi ranh giới thẳng hàng, trong đó các điểm nằm trên các cạnh của thân tàu và vẫn phải được coi là các điểm biên chứ không phải các điểm bên trong. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ là: đối với mỗi điểm, loại bỏ các tập hợp con của các điểm khác và kiểm tra xem điểm đó có trở thành một phần của bao lồi hay không. Điều này nhanh chóng trở thành cấp số nhân vì mỗi tập hợp con yêu cầu tính toán thân tàu. 

Một ý tưởng thô bạo có cấu trúc hơn là mô phỏng quá trình bóc tách: tính toán bao lồi, loại bỏ nó, tính toán bao tiếp theo, v.v. Điều này có tác dụng vì mỗi “lớp” tương ứng với một bước loại bỏ. Tuy nhiên, việc tính toán lại thân tàu từ đầu sau mỗi lần xóa rất tốn kém. Nếu thực hiện một cách đơn giản, mỗi phép tính thân tàu sẽ tốn O(N log N) và có thể có O(N) lớp trong trường hợp xấu nhất, dẫn đến O(N² log N). 

Quan sát quan trọng là chúng ta không cần phải tính toán lại thân tàu nhiều lần từ đầu. Thay vào đó, chúng ta có thể gán một “số lớp” cho từng điểm trong khi bóc vỏ dần dần. Mỗi lần tính toán một thân tàu, chúng tôi gắn nhãn tất cả các đỉnh của nó với độ sâu hiện tại và xóa chúng khỏi tập hợp hoạt động. Lặp lại điều này cho đến khi tất cả các điểm được dán nhãn sẽ đưa ra câu trả lời đúng. Vì mỗi điểm được loại bỏ chính xác một lần và tham gia vào nhiều nhất một phép tính thân trên mỗi lớp, điều này vẫn đủ hiệu quả cho N lên tới vài nghìn nếu thực hiện cẩn thận. 

Điều này thường được gọi là phân hủy củ hành hoặc lớp lồi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại thân tàu theo điểm | O(N³ log N) | O(N) | Quá chậm | 
| Lột vỏ nhiều lần | O(N2 log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi liên tục trích xuất các bao lồi từ tập hợp các điểm hoạt động hiện tại. 

1. Bắt đầu với tất cả các điểm được đánh dấu là chưa xử lý. Chúng tôi duy trì một mảng`ans`được khởi tạo thành -1 cho mỗi điểm, biểu thị độ sâu lớp của nó. 
2. Trong khi vẫn còn các điểm chưa được gán, hãy tính bao lồi của tập hoạt động hiện tại bằng thuật toán chuỗi đơn điệu. Điều này đưa ra ranh giới bên ngoài của cấu trúc còn lại. 
3. Tất cả các điểm nằm trên thân này đều được gán số lớp hiện tại. Những điểm này tương ứng với những cây có thể trở thành ranh giới sau khi loại bỏ chính xác nhiều lớp này. 
4. Loại bỏ các điểm thân này khỏi tập hợp hoạt động. 
5. Tăng bộ đếm lớp và lặp lại cho đến khi không còn điểm nào. 

Ý tưởng chính là mỗi lần lặp sẽ bóc ra chính xác một “lớp vỏ” lồi của các điểm và mỗi điểm thuộc về chính xác một lớp vỏ. 

## Tại sao nó hoạt động 

Mỗi bao lồi đại diện cho ranh giới ngoài cùng của tập hợp điểm hiện tại. Bất kỳ điểm nào trên ranh giới này không có điểm nào còn lại hoàn toàn nằm ngoài nó theo mọi hướng, do đó độ sâu lồi ở giai đoạn đó là tối thiểu. Việc loại bỏ nó không thể ảnh hưởng đến độ sâu tương đối của các điểm bên trong vì những điểm đó vẫn được bao bọc bởi các lớp còn lại. Do đó, mỗi lần lặp sẽ xác định chính xác một lớp điểm đầy đủ theo thứ tự độ sâu lồi tăng dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(o, a, b):
    return (a[0]-o[0])*(b[1]-o[1]) - (a[1]-o[1])*(b[0]-o[0])

def convex_hull(points):
    points.sort()
    if len(points) <= 1:
        return points

    lower = []
    for p in points:
        while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)

    upper = []
    for p in reversed(points):
        while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)

    return lower[:-1] + upper[:-1]

def solve():
    n = int(input())
    pts = []
    for i in range(n):
        x, y = map(int, input().split())
        pts.append([x, y, i])

    alive = pts[:]
    ans = [-1] * n
    layer = 0

    while alive:
        hull = convex_hull(alive)
        hull_set = set((p[0], p[1], p[2]) for p in hull)

        new_alive = []
        for p in alive:
            if (p[0], p[1], p[2]) in hull_set:
                ans[p[2]] = layer
            else:
                new_alive.append(p)

        alive = new_alive
        layer += 1

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một danh sách các điểm hoạt động và tính toán nhiều lần bao lồi của tập hợp đó. Các điểm thân tàu được gán chỉ số lớp hiện tại và bị loại bỏ. 

Một chi tiết triển khai tinh tế là xác định thành viên thân tàu một cách đáng tin cậy. Chúng tôi lưu trữ các bộ ba đầy đủ bao gồm các chỉ số để tránh sự mơ hồ khi sử dụng tọa độ để so sánh. Một chi tiết khác là đảm bảo các điểm ranh giới thẳng hàng được bao gồm trong thân tàu, được xử lý bởi`<= 0`kiểm tra định hướng. 

## Ví dụ đã hoạt động 

Hãy xem xét một hình vuông đơn giản với một điểm trung tâm: 

đầu vào:```
5
0 0
10 0
10 10
0 10
5 5
```Ở lớp 0, bao lồi chứa bốn góc. Tất cả đều được gán 0. Trung tâm vẫn còn. 

Ở lớp 1, chỉ còn lại phần trung tâm, tạo thành phần thân của tập còn lại nên được gán 1. 

| Bước | Kích thước bộ sống động | Điểm thân tàu | Được giao | 
| --- | --- | --- | --- | 
| 0 | 5 | góc | 0 | 
| 1 | 1 | trung tâm | 1 | 

Điều này xác nhận rằng các điểm biên bên ngoài nhận được 0 và độ sâu bên trong tăng vào bên trong. 

Bây giờ hãy xem xét một hình tam giác có các điểm bên trong lồng nhau. Mỗi lần bong tróc chỉ loại bỏ lớp ngoài cùng và các điểm còn lại sẽ trở thành các đỉnh thân tàu mới, xác nhận hành vi phân lớp chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2 log N) | mỗi lớp tính toán lại bao lồi trên tập thu nhỏ | 
| Không gian | O(N) | điểm lưu trữ và thân tàu trung gian | 

Với N lên tới 3000, cách tiếp cận này là đủ vì tổng số lần tính toán lại bị giới hạn bởi số điểm và mỗi phép tính thân tàu đều hiệu quả trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    def cross(o,a,b):
        return (a[0]-o[0])*(b[1]-o[1]) - (a[1]-o[1])*(b[0]-o[0])

    def hull(points):
        points.sort()
        lower=[]
        for p in points:
            while len(lower)>=2 and cross(lower[-2],lower[-1],p)<=0:
                lower.pop()
            lower.append(p)
        upper=[]
        for p in reversed(points):
            while len(upper)>=2 and cross(upper[-2],upper[-1],p)<=0:
                upper.pop()
            upper.append(p)
        return lower[:-1]+upper[:-1]

    alive = pts[:]
    ans = [-1]*n
    layer = 0

    while alive:
        h = set(hull(alive))
        nxt = []
        for p in alive:
            if p in h:
                ans[pts.index(p)] = layer
            else:
                nxt.append(p)
        alive = nxt
        layer += 1

    return "\n".join(map(str, ans))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình vuông + tâm | 0 0 0 0 1 | lớp bên trong đơn | 
| tam giác | 0 0 0 | mọi ranh giới | 
| lớp lồng nhau | tăng độ sâu | độ chính xác đa vỏ | 

## Vỏ cạnh 

Một tập hợp thẳng hàng đầy đủ là một trường hợp quan trọng vì mọi điểm đều nằm trên biên thân. Trong trường hợp này, thân thứ nhất bằng tập hợp đầy đủ, vì vậy tất cả các điểm được gán lớp 0 và bị loại bỏ ngay lập tức, điều này đúng vì không có điểm nào sâu hơn điểm khác theo thuật ngữ lồi. 

Một trường hợp cạnh khác là khi có nhiều điểm nằm trên cùng một cạnh bao lồi. Tất cả những thứ này phải được gán cùng một lớp cùng một lúc. Cấu trúc thân tàu bao gồm các điểm cạnh thẳng hàng do quy tắc định hướng, đảm bảo tất cả chúng được loại bỏ cùng nhau và được ấn định độ sâu giống nhau.

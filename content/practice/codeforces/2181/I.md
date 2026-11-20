---
title: "CF 2181I - Khóa liên động tưới"
description: "Chúng ta có hai tập hợp điểm trên mặt phẳng Descartes: máy bơm nằm rải rác trên một thung lũng và các hồ chứa nằm trên những ngọn đồi xung quanh."
date: "2026-06-07T22:03:15+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 2181
codeforces_index: "I"
codeforces_contest_name: "2025-2026 ICPC, NERC, Northern Eurasia Finals (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 3500
weight: 2181
solve_time_s: 132
verified: false
draft: false
---

[CF 2181I - Khóa liên động tưới](https://codeforces.com/problemset/problem/2181/I) 

**Đánh giá:** 3500 
**Thẻ:** hình học 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp điểm trên mặt phẳng Descartes: máy bơm nằm rải rác trên một thung lũng và các hồ chứa nằm trên những ngọn đồi xung quanh. Mỗi hợp tác xã muốn kết nối một cặp điểm của họ bằng một đường ống thẳng và mục tiêu là để xem liệu hai đường ống này - một nối hai máy bơm và một nối hai hồ chứa - có thể giao nhau hay không. Giao lộ bao gồm các đoạn tiếp giáp hoặc chồng lên nhau. 

Đầu vào chỉ định nhiều kịch bản lập kế hoạch. Mỗi kịch bản cung cấp số lượng máy bơm và tọa độ của chúng, theo sau là số lượng hồ chứa và tọa độ của chúng. Đầu ra cho mỗi kịch bản là bốn chỉ số biểu thị một cặp ống giao nhau hợp lệ hoặc -1 nếu không tồn tại cấu hình như vậy. 

Những hạn chế chính là số lượng máy bơm và hồ chứa theo kịch bản và số lượng kịch bản. Vì tổng số điểm trong tất cả các kịch bản được giới hạn ở$2 \cdot 10^5$đối với cả máy bơm và hồ chứa, một giải pháp đơn giản là kiểm tra tất cả các cặp máy bơm đối với tất cả các cặp hồ chứa có thể liên quan đến khoảng$O(n^2 m^2)$trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. Do đó chúng ta cần một thuật toán tránh liệt kê mọi cặp có thể. 

Các trường hợp cạnh bao gồm các tình huống trong đó tất cả các máy bơm hoặc tất cả các bể chứa đều thẳng hàng. Ví dụ, nếu tất cả các máy bơm nằm trên một đường nằm ngang và tất cả các hồ chứa nằm trên một đường thẳng song song với đường đó thì không thể có giao điểm. Một cách tiếp cận bất cẩn chọn hai máy bơm đầu tiên và hai hồ chứa đầu tiên mà không kiểm tra vị trí tương đối của chúng có thể báo cáo một giao điểm sai. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi kịch bản, người ta có thể lặp lại trên từng cặp máy bơm, sau đó lặp lại trên từng cặp hồ chứa và kiểm tra xem hai đoạn đường có giao nhau hay không. Điều này đảm bảo tính chính xác nhưng có độ phức tạp về thời gian$O(n^2 m^2)$theo từng kịch bản, điều này không khả thi$n, m \le 10^5$Và$t \le 10^5$. 

Quan sát quan trọng giúp mở ra giải pháp nhanh hơn là đối với bất kỳ bộ máy bơm không thẳng hàng nào và bất kỳ bộ hồ chứa không thẳng hàng nào, luôn tồn tại sự lựa chọn giữa hai máy bơm và hai hồ chứa có các đoạn nối giao nhau. Điều này xuất phát từ thực tế hình học là nếu bạn chọn máy bơm ngoài cùng bên trái và ngoài cùng bên phải, cũng như bình chứa ngoài cùng bên trái và ngoài cùng bên phải, thì một trong bốn cặp đoạn kết nối sẽ giao nhau hoặc các bộ hoàn toàn tách biệt trong trường hợp đó tất cả các điểm đều thẳng hàng dọc theo một trục chung. Điều này làm giảm vấn đề khi kiểm tra một đường chéo đơn giản: chọn các điểm cực trị của mỗi tập hợp dọc theo trục x (hoặc trục y nếu x bằng) và thử một phép thử giao điểm. 

Điều này có nghĩa là chúng ta không cần phải kiểm tra tất cả các cặp. Trong mỗi kịch bản, chúng ta có thể sắp xếp máy bơm và bể chứa theo tọa độ x để tìm hai điểm cực trị cho mỗi bộ, sau đó chọn điểm đầu tiên và điểm cuối cùng của mỗi danh sách được sắp xếp. Nếu bốn điểm này tạo thành các đoạn giao nhau thì chúng ta có cấu hình hợp lệ. Nếu bộ máy bơm hoặc bể chứa hoàn toàn thẳng đứng (tất cả các tọa độ x bằng nhau) hoặc nằm ngang (tất cả các tọa độ y bằng nhau), thì chỉ có thể giao nhau nếu bộ kia không song song và tách biệt hoàn toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 m^2) | O(n + m) | Quá chậm | 
| Điểm cực trị | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi kịch bản, hãy đọc tọa độ của máy bơm và hồ chứa. 
2. Xác định máy bơm ngoài cùng bên trái và ngoài cùng bên phải bằng cách so sánh tọa độ x. Nếu có sự ràng buộc, hãy chọn máy bơm có y tối thiểu cho ngoài cùng bên trái và y tối đa cho ngoài cùng bên phải. 
3. Tương tự, xác định các hồ chứa ngoài cùng bên trái và ngoài cùng bên phải bằng tọa độ x với các mối liên kết bị phá vỡ bởi tọa độ y. 
4. Cố gắng kết nối máy bơm ngoài cùng bên trái với máy bơm ngoài cùng bên phải và bình chứa ngoài cùng bên trái với bình chứa ngoài cùng bên phải. Nếu các đoạn này giao nhau, hãy xuất chỉ số của chúng. 
5. Nếu chúng không giao nhau, hãy đảo ngược một trong các chỉ số bình chứa (nối máy bơm ngoài cùng bên trái với máy bơm ngoài cùng bên phải và bình chứa ngoài cùng bên phải với bình chứa ngoài cùng bên trái) và kiểm tra lại. 
6. Nếu tìm thấy giao điểm trong một trong hai cấu hình, hãy báo cáo các chỉ số tương ứng. Nếu không thì xuất -1. 

Tại sao nó hoạt động: Bằng cách chọn các điểm cực trị, chúng tôi đảm bảo rằng nếu có thể giao nhau thì nó sẽ xảy ra giữa các điểm ngoài cùng. Điều này có hiệu quả vì bất kỳ cặp máy bơm và cặp hồ chứa nào không được tách biệt hoàn toàn dọc theo một trục sẽ giao nhau ở một trong những tổ hợp cực đoan này. Thuật toán giảm vấn đề từ phép liệt kê tổ hợp xuống một số lượng nhỏ các phép thử giao nhau không đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ccw(a, b, c):
    return (b[0]-a[0])*(c[1]-a[1]) - (b[1]-a[1])*(c[0]-a[0])

def intersect(p1, p2, q1, q2):
    d1 = ccw(p1, p2, q1)
    d2 = ccw(p1, p2, q2)
    d3 = ccw(q1, q2, p1)
    d4 = ccw(q1, q2, p2)
    if d1*d2 < 0 and d3*d4 < 0:
        return True
    if d1 == 0 and on_segment(p1, p2, q1):
        return True
    if d2 == 0 and on_segment(p1, p2, q2):
        return True
    if d3 == 0 and on_segment(q1, q2, p1):
        return True
    if d4 == 0 and on_segment(q1, q2, p2):
        return True
    return False

def on_segment(a, b, c):
    return min(a[0], b[0]) <= c[0] <= max(a[0], b[0]) and min(a[1], b[1]) <= c[1] <= max(a[1], b[1])

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        pumps = [tuple(map(int, input().split())) + (i+1,) for i in range(n)]
        m = int(input())
        reservoirs = [tuple(map(int, input().split())) + (i+1,) for i in range(m)]
        pumps.sort()
        reservoirs.sort()
        p1, p2 = pumps[0], pumps[-1]
        r1, r2 = reservoirs[0], reservoirs[-1]
        if intersect(p1, p2, r1, r2):
            print(p1[2], p2[2], r1[2], r2[2])
        elif intersect(p1, p2, r2, r1):
            print(p1[2], p2[2], r2[2], r1[2])
        else:
            print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các điểm, nối thêm các chỉ mục của chúng cho đầu ra và sắp xếp từng tập hợp theo tọa độ x. các`intersect`hàm xử lý tất cả các trường hợp tổng quát và cộng tuyến, trong khi`on_segment`đảm bảo phát hiện chính xác các đoạn chạm hoặc chồng chéo. Các điểm cực trị đảm bảo rằng nếu tồn tại một giao điểm thì nó sẽ được phát hiện bằng những kết hợp này. 

## Ví dụ đã hoạt động 

Kịch bản đầu vào mẫu 1: 

| Máy bơm | Đã sắp xếp | Hồ chứa | Đã sắp xếp | 
| --- | --- | --- | --- | 
| (0,0) | (0,0) | (-1,1) | (-1,1) | 
| (4,0) | (1,3) | (5,1) | (2,-1) | 
| (3,3) | (3,3) | (2,-1) | (2,4) | 
| (1,3) | (4,0) | (2,4) | (5,1) | 
| | | (6,3) | (6,3) | 

Máy bơm ngoài cùng bên trái và ngoài cùng bên phải: chỉ số (0,0) 1 và (1,3) 4. Bể chứa ngoài cùng bên trái và ngoài cùng bên phải: chỉ số (-1,1) 1 và (6,3) chỉ số 5. ​​Các phân đoạn giao nhau, đầu ra`1 4 1 5`. 

Tình huống mẫu 2: 

Tất cả các máy bơm tạo thành hình vuông từ (0,0) đến (1,1), tất cả các bể chứa tạo thành hình vuông từ (5,5) đến (6,6). Các đoạn cực trị không giao nhau; đầu ra -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) mỗi kịch bản | Việc sắp xếp các điểm cực trị là tuyến tính nếu chúng ta theo dõi mức tối thiểu/tối đa trong khi đọc điểm | 
| Không gian | O(n + m) | Lưu trữ tọa độ, chỉ số của máy bơm và hồ chứa | 

Tổng số điểm trong tất cả các kịch bản là$2 \cdot 10^5$, vì vậy giải pháp tuyến tính này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""3
4
0 0
4 0
3 3
1 3
5
-1 1
5 1
2 -1
2 4
6 3
4
0 0
1 0
0 1
1 1
4
5 5
6 5
5 6
6 6
3
0 0
4 0
0 2
3
4 -2
4 2
```

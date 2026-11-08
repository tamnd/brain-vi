---
title: "CF 1450B - Bi Thép"
description: "Chúng ta được cho một số quả cầu thép đặt ở những tọa độ khác nhau trên một mặt phẳng. Một quả bóng tích điện hoạt động giống như một nam châm có bán kính đo bằng khoảng cách Manhattan."
date: "2026-06-11T03:47:30+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1450
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 12"
rating: 1000
weight: 1450
solve_time_s: 709
verified: true
draft: false
---

[CF 1450B - Quả bóng thép](https://codeforces.com/problemset/problem/1450/B) 

**Đánh giá:** 1000 
**Tags:** vũ phu, hình học, tham lam 
**Thời gian giải:** 11 phút 49 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số quả cầu thép đặt ở những tọa độ khác nhau trên một mặt phẳng. Một quả bóng tích điện hoạt động giống như một nam châm có bán kính đo bằng khoảng cách Manhattan. Khi chúng ta tích điện vào một quả bóng, mọi quả bóng có khoảng cách từ Manhattan đến nó nhiều nhất`k`ngay lập tức di chuyển đến vị trí của quả bóng đã tích điện. 

Mục tiêu là làm cho tất cả các quả bóng chiếm một vị trí. Chúng tôi có thể thực hiện bất kỳ số lượng hoạt động tính phí nào. Đối với mỗi trường hợp thử nghiệm, chúng tôi phải xác định số lượng thao tác tối thiểu được yêu cầu hoặc báo cáo rằng việc đó không thể thực hiện được. 

Những hạn chế nhỏ một cách bất thường. Mỗi trường hợp kiểm thử chứa tối đa 100 điểm và có tối đa 100 trường hợp kiểm thử. Thậm chí một`O(n²)`hoặc`O(n³)`giải pháp dễ dàng đủ nhanh. Với`n = 100`, MỘT`O(n²)`thuật toán chỉ thực hiện 10.000 lần kiểm tra khoảng cách cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh đầu tiên là khi tất cả các điểm đã nằm trong khoảng cách`k`của một số điểm. Ví dụ:```
3 3
6 7
8 8
6 9
```Mọi điểm đều nằm trong khoảng cách 3 của mọi điểm khác. Sạc bất kỳ quả bóng nào sẽ tập hợp tất cả các quả bóng ngay lập tức, vì vậy câu trả lời là`1`, không`0`. Bài toán yêu cầu số thao tác tối thiểu cần thiết để làm cho tất cả các quả bóng trùng nhau và ban đầu chúng khác nhau. 

Một trường hợp tinh vi khác xảy ra khi các điểm tạo thành một chuỗi được kết nối nhưng không một điểm nào có thể tiếp cận được tất cả các điểm khác:```
4 1
0 0
0 1
0 2
0 3
```Mỗi điểm đều gần với các điểm lân cận của nó, nhưng không có điểm nào có khoảng cách đến mọi điểm khác tối đa bằng 1. Một giải pháp bất cẩn có thể cho rằng các thao tác lặp đi lặp lại có thể dần dần hợp nhất mọi thứ. Họ không thể. Câu trả lời đúng là`-1`. 

Trường hợp cạnh thứ ba là`k = 0`. Vì tất cả các tọa độ đều khác nhau nên không quả bóng nào có thể hút bất kỳ quả bóng nào khác. Câu trả lời luôn là`-1`. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ thử các chuỗi hoạt động sạc và theo dõi cách các điểm di chuyển. Việc tìm kiếm như vậy nhanh chóng trở nên phức tạp vì trạng thái thay đổi sau mỗi thao tác và số lượng chuỗi có thể tăng theo cấp số nhân. 

Quan sát quan trọng là câu trả lời không bao giờ lớn hơn một. 

Giả sử tất cả các quả bóng cuối cùng đều ở một vị trí nào đó. Vị trí cuối cùng đó phải là một trong những vị trí bóng ban đầu. Bóng không bao giờ tạo tọa độ mới. Mọi chuyển động đều sao chép vị trí của một quả bóng tích điện. 

Hãy xem xét hoạt động cuối cùng trong một chuỗi thành công. Lúc đó, mọi quả bóng còn lại phải di chuyển vào vị trí của quả bóng đã tích điện. Vì vậy mọi quả bóng đều phải ở trong khoảng cách`k`của quả bóng tích điện đó. 

Vì các quả bóng chuyển động chỉ thay đổi vị trí của chúng thành các vị trí bóng hiện có, nên nếu một quả bóng ban đầu nào đó có thể hút trực tiếp mọi quả bóng ban đầu khác thì một thao tác là đủ. Nếu không có quả bóng như vậy tồn tại thì không có chuỗi thao tác nào có thể tạo ra một chuỗi sau này. 

Điều này làm giảm toàn bộ vấn đề để kiểm tra xem có tồn tại một điểm có khoảng cách từ Manhattan đến mọi điểm khác nhiều nhất không`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng không gian trạng thái | Hàm mũ | Hàm mũ | Quá chậm | 
| Kiểm tra mọi trung tâm ứng viên | O(n²) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm của test case. 
2. Đối với mọi điểm`i`, hãy coi nó như một điểm tập hợp có thể. 
3. Tính khoảng cách Manhattan từ điểm`i`đến mọi điểm khác. 
4. Nếu tất cả các khoảng cách lớn nhất`k`, sau đó là điểm sạc`i`tập hợp tất cả các quả bóng trong một hoạt động duy nhất. đầu ra`1`. 
5. Nếu không có điểm nào thỏa mãn điều kiện này thì xuất ra`-1`. 

### Tại sao nó hoạt động 

Giả sử tồn tại một chuỗi thành công. Nhìn vào hoạt động cuối cùng. Quả bóng tích điện có một vị trí nào đó`P`, là một trong những tọa độ bóng ban đầu. Mỗi quả bóng trở thành một phần của vị trí hợp nhất cuối cùng phải ở trong khoảng cách`k`của`P`vào thời điểm đó. 

Vì tất cả các vị trí trong suốt quá trình đều được sao chép từ các vị trí bóng ban đầu nên điểm tập hợp cuối cùng phải tương ứng với điểm ban đầu. Nếu điểm ban đầu đó không thể trực tiếp thu hút một số điểm ban đầu khác, thì không có cách nào để điểm đó tham gia vào quá trình hợp nhất cuối cùng. Do đó, một nghiệm tồn tại khi và chỉ nếu một số điểm ban đầu nằm trong khoảng cách`k`của mọi điểm ban đầu. 

Khi tồn tại một điểm như vậy, một thao tác sẽ ngay lập tức thu thập tất cả các quả bóng. Nếu không thì không có trình tự nào có thể thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, k = map(int, input().split())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    possible = False

    for x1, y1 in pts:
        ok = True

        for x2, y2 in pts:
            if abs(x1 - x2) + abs(y1 - y2) > k:
                ok = False
                break

        if ok:
            possible = True
            break

    print(1 if possible else -1)
```Vòng ngoài kiểm tra mọi điểm như một trung tâm thu thập tiềm năng. Vòng lặp bên trong xác minh xem mọi điểm khác có nằm trong khoảng cách Manhattan hay không`k`. 

Sớm`break`báo cáo là hữu ích nhưng không bắt buộc. Ngay khi vượt quá một khoảng cách`k`, ứng viên đó không thể làm việc được nữa. Ngay khi tìm thấy một ứng cử viên hợp lệ, câu trả lời sẽ được biết. 

Khoảng cách Manhattan phải được tính như sau:```
abs(x1 - x2) + abs(y1 - y2)
```Sử dụng khoảng cách Euclide hoặc khoảng cách bình phương sẽ giải quyết được một vấn đề khác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
6 7
8 8
6 9
```| Ứng viên | Khoảng cách với người khác | hợp lệ | 
| --- | --- | --- | 
| (6,7) | 3, 2 | Có | 
| (8,8) | 3, 3 | Có | 
| (6,9) | 2, 3 | Có | 

Thí sinh đầu tiên đã đạt đủ điểm nên câu trả lời là`1`. 

Ví dụ này chứng tỏ rằng chỉ cần một trung tâm thành công. Chúng ta không cần tìm kiếm câu trả lời tốt hơn bởi vì`1`đã là tối thiểu rồi. 

### Ví dụ 2 

đầu vào:```
4 1
0 0
0 1
0 2
0 3
```| Ứng viên | Khoảng cách xa nhất | hợp lệ | 
| --- | --- | --- | 
| (0,0) | 3 | Không | 
| (0,1) | 2 | Không | 
| (0,2) | 2 | Không | 
| (0,3) | 3 | Không | 

Không có điểm nào có thể tiếp cận tất cả các điểm khác có bán kính 1. 

Câu trả lời là`-1`. 

Ví dụ này chứng minh tại sao kết nối là không đủ. Các điểm lân cận không thể dần dần kéo toàn bộ chuỗi lại với nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Kiểm tra từng cặp điểm | 
| Không gian | O(1) không bao gồm lưu trữ đầu vào | Chỉ có một vài biến được sử dụng | 

Với`n ≤ 100`, tối đa 10.000 phép tính khoảng cách được thực hiện cho mỗi trường hợp thử nghiệm. Ngay cả với 100 trường hợp thử nghiệm, điều này vẫn dễ dàng xảy ra trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n, k = map(int, input().split())
        pts = [tuple(map(int, input().split())) for _ in range(n)]

        possible = False

        for x1, y1 in pts:
            ok = True
            for x2, y2 in pts:
                if abs(x1 - x2) + abs(y1 - y2) > k:
                    ok = False
                    break
            if ok:
                possible = True
                break

        ans.append("1" if possible else "-1")

    return "\n".join(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# sample
assert run(
"""3
3 2
0 0
3 3
1 1
3 3
6 7
8 8
6 9
4 1
0 0
0 1
0 2
0 3
"""
) == "-1\n1\n-1"

# minimum size, impossible
assert run(
"""1
2 0
0 0
1 0
"""
) == "-1"

# minimum size, possible
assert run(
"""1
2 1
0 0
1 0
"""
) == "1"

# center point exists
assert run(
"""1
5 2
0 0
1 0
0 1
1 1
0 2
"""
) == "1"

# far separated points
assert run(
"""1
3 1
0 0
100 100
200 200
"""
) == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai điểm, k = 0 | -1 | Không có sức hấp dẫn | 
| Hai điểm, k = 1 | 1 | Trường hợp thành công nhỏ nhất | 
| Cụm có điểm trung tâm | 1 | Trung tâm thu thập hợp lệ tồn tại | 
| Điểm cách biệt rộng rãi | -1 | Cấu hình không thể | 
| Mẫu chính thức | -1 1 -1 | Tuyên bố trận đấu | 

## Vỏ cạnh 

Hãy xem xét:```
1
2 0
0 0
1 0
```Khoảng cách giữa các điểm là 1, vượt quá`k = 0`. Thuật toán kiểm tra cả hai điểm và loại bỏ cả hai ứng cử viên. Nó xuất ra`-1`. 

Coi như:```
1
2 5
0 0
3 2
```Khoảng cách Manhattan chính xác là 5. Vì điều kiện tối đa là khoảng cách`k`, cả hai điểm đều đủ điều kiện. Thuật toán xuất ra chính xác`1`. 

Hãy xem xét chuỗi:```
1
4 1
0 0
0 1
0 2
0 3
```Các điểm ở giữa có vẻ đầy hứa hẹn vì chúng nằm gần các điểm lân cận. Thuật toán tính toán khoảng cách đến mọi điểm, phát hiện khoảng cách từ 2 đến một điểm cuối, loại bỏ từng ứng cử viên và xuất ra`-1`. 

Hãy xem xét trường hợp một điểm chiếm ưu thế:```
1
5 10
0 0
1 1
2 2
3 3
4 4
```điểm`(0,0)`có khoảng cách Manhattan nhiều nhất là 8 từ mọi điểm khác. Thuật toán chấp nhận nó ngay lập tức và xuất ra`1`. Điều này xác nhận logic thoát sớm là chính xác.

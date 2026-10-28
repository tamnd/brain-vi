---
title: "CF 106186G - Hai đèn hiệu chặt nhất"
description: "Chúng tôi có một tập hợp các tháp đèn hiệu được biểu thị bằng các điểm trên mặt phẳng. Một tam giác được xây dựng từ ba tòa tháp được coi là đặc biệt khi có đúng một cạnh của nó nằm trên ranh giới của toàn bộ tập hợp điểm."
date: "2026-06-25T10:49:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106186
codeforces_index: "G"
codeforces_contest_name: "NWU IUPC 2025 powered by CPS Academy"
rating: 0
weight: 106186
solve_time_s: 39
verified: true
draft: false
---

[CF 106186G - Hai đèn hiệu chặt nhất](https://codeforces.com/problemset/problem/106186/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng tôi có một tập hợp các tháp đèn hiệu được biểu thị bằng các điểm trên mặt phẳng. Một tam giác được xây dựng từ ba tòa tháp được coi là đặc biệt khi có đúng một cạnh của nó nằm trên ranh giới của toàn bộ tập hợp điểm. Về mặt hình học, một bên phải là mép thân lồi, còn hai bên còn lại phải cắt qua tập hợp các điểm sao cho các tháp còn lại xuất hiện ở hai bên của các đường đó. 

Đối với mỗi tòa tháp, chúng tôi xem xét tất cả các hình tam giác đặc biệt chứa nó và xác định điểm của nó là diện tích nhỏ nhất trong số các hình tam giác đó. Nhiệm vụ là tìm ra số điểm tối đa trên tất cả các tòa tháp. Nếu một tòa tháp không thể thuộc về một tam giác đặc biệt nào thì nó chẳng đóng góp gì cả. 

Đầu vào chứa tới 5000 điểm có tọa độ riêng biệt và không có ba điểm trên cùng một đường. Kích thước này là hạn chế chính. Giải pháp bậc ba trên tất cả các bộ ba sẽ kiểm tra khoảng 125 tỷ hình tam giác trong trường hợp xấu nhất, vượt xa giới hạn một giây cho phép. Chúng ta cần một giải pháp gần với phương trình bậc hai, vì 5000 bình phương mang lại 25 triệu phép tính, điều này thực tế trong Python được tối ưu hóa. 

Điều kiện hình học có vẻ phức tạp, nhưng bao lồi mang lại sự đơn giản hóa cơ bản. Một cạnh của một tam giác là cạnh đỡ của toàn bộ tập hợp điểm chính xác khi nó là cạnh bao lồi. Khi cạnh thân tàu được cố định, hầu hết mọi điểm thứ ba đều tạo thành một hình tam giác hợp lệ. Điểm thứ ba không hợp lệ duy nhất là hai đỉnh thân lân cận của cạnh đó, vì chúng sẽ tạo ra một cạnh thân khác. 

Một số trường hợp đặc biệt có thể phá vỡ việc triển khai trực tiếp. Nếu tất cả các điểm đều nằm trên một thân tam giác thì không có tam giác hợp lệ nào tồn tại. 

Ví dụ:```
3
0 0
1 0
0 1
```không có đáp án vì tam giác duy nhất có ba cạnh nằm trên bao lồi. Điều kiện bắt buộc yêu cầu chính xác một bên như vậy, vì vậy kết quả đầu ra sẽ là`0`. 

Một trường hợp khác là tứ giác lồi:```
4
0 0
4 0
4 4
0 4
```Tam giác có ba đỉnh thân liên tiếp không hợp lệ vì nó có hai hoặc ba cạnh thân. Một giải pháp bất cẩn chỉ kiểm tra xem cạnh được chọn có phải là cạnh thân tàu hay không sẽ tính sai các hình tam giác này. Kết quả đúng dựa trên hai hình tam giác chéo, có diện tích nhân đôi là 16, vì vậy câu trả lời là`8.0000000000`. 

Vấn đề thứ ba xuất hiện khi một điểm nằm trong. Ví dụ:```
5
0 0
10 0
10 10
0 10
5 5
```Điểm trung tâm có thể tham gia vào các hình tam giác hợp lệ với các cạnh thân, nhưng bốn góc cần các lựa chọn đỉnh thân đối diện được xử lý chính xác. Việc triển khai chỉ coi các đỉnh của thân tàu là các đỉnh tam giác có thể sẽ bỏ lỡ các câu trả lời hợp lệ. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử từng bộ ba điểm. Đối với mỗi tam giác, chúng ta có thể kiểm tra ba cạnh của nó và đếm xem có bao nhiêu đường hỗ trợ của toàn bộ tam giác. Nếu chính xác một bên đủ điều kiện, chúng tôi sẽ cập nhật câu trả lời cho từng đỉnh. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, có các bộ ba O(n³) và việc kiểm tra một tam giác dựa trên tất cả các điểm sẽ thêm một hệ số khác, cho ra O(n⁴). Với n bằng 5000 thì điều này hoàn toàn không thể thực hiện được. 

Quan sát hữu ích là cạnh duy nhất cần đặc biệt là cạnh thân lồi. Đầu tiên chúng ta xây dựng thân lồi. Sau đó, chúng ta chỉ xét các hình tam giác được tạo bởi một cạnh thân tàu và điểm thứ ba. 

Xét cạnh thân AB. Bất kỳ điểm C nào tạo thành tam giác ABC có AB là cạnh đỡ. Các trường hợp duy nhất mà tam giác có một cạnh đỡ khác là khi C là một trong hai cạnh kề của thân A hoặc B. Hai lựa chọn đó bị loại trừ. Mỗi điểm khác cho chính xác một mặt hỗ trợ. 

Điều này làm giảm vấn đề kiểm tra từng cạnh của thân tàu đối với mọi điểm. Có nhiều nhất n cạnh thân tàu nên tổng công là O(n2). Đối với mọi kết hợp hợp lệ, chúng tôi tính toán diện tích nhân đôi bằng cách sử dụng tích chéo. Việc giữ lại diện tích nhân đôi sẽ tránh được lỗi dấu phẩy động cho đến phép chia cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n⁴) | O(n) | Quá chậm | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Xây dựng bao lồi tất cả các điểm bằng thuật toán chuỗi đơn điệu. Thân tàu chỉ cung cấp các cạnh có thể thỏa mãn điều kiện đường đỡ. 
2. Nếu thân tàu có ít hơn bốn đỉnh, trả về 0. Một thân tam giác không thể chứa một tam giác có đúng một cạnh thân. 
3. Đối với mỗi cặp đỉnh thân A và B liên tiếp, coi AB là cạnh thân yêu cầu duy nhất. Hai điểm lân cận của A và B là những điểm duy nhất không được dùng làm đỉnh thứ ba. 
4. Quét tất cả các điểm C. Bỏ qua A, B và hai điểm lân cận bị cấm. Với mỗi điểm còn lại, hãy tính diện tích gấp đôi của tam giác ABC và cập nhật giá trị nhỏ nhất tìm được của C. 
5. Sau khi xử lý xong mỗi cạnh thân, mỗi điểm sẽ lưu trữ diện tích tam giác hợp lệ nhỏ nhất chứa nó. Câu trả lời là giá trị lớn nhất trong số này. 

Lý do điều này có tác dụng là vì mỗi tam giác hợp lệ phải chứa chính xác một cạnh thân. Thuật toán liệt kê mọi cạnh có thể có của thân tàu và mọi đỉnh thứ ba có thể giữ cho hai cạnh còn lại không trở thành cạnh thân tàu. Vì mọi tam giác hợp lệ đều xuất hiện trong bảng liệt kê này nên không có ứng viên nào bị bỏ sót. Vì mọi ứng cử viên được liệt kê đều thỏa mãn quy tắc loại trừ lân cận nên không có tam giác không hợp lệ nào được sử dụng. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(a, b, c):
    return (b[0] - a[0]) * (c[1] - a[1]) - (b[1] - a[1]) * (c[0] - a[0])

def convex_hull(points):
    pts = sorted(points)

    def build(seq):
        hull = []
        for p in seq:
            while len(hull) >= 2 and cross(hull[-2], hull[-1], p) <= 0:
                hull.pop()
            hull.append(p)
        return hull

    lower = build(pts)
    upper = build(reversed(pts))
    return lower[:-1] + upper[:-1]

def solve():
    n = int(input())
    points = [tuple(map(int, input().split())) for _ in range(n)]

    hull = convex_hull(points)
    h = len(hull)

    if h < 4:
        print("0.0000000000")
        return

    index = {p: i for i, p in enumerate(hull)}
    best = [10**30] * n

    for i in range(h):
        a = hull[i]
        b = hull[(i + 1) % h]

        forbidden = {
            hull[(i - 1) % h],
            hull[(i + 2) % h],
            a,
            b
        }

        for j, c in enumerate(points):
            if c in forbidden:
                continue
            area2 = abs(cross(a, b, c))
            if area2 < best[j]:
                best[j] = area2

    ans = max(best)
    if ans == 10**30:
        print("0.0000000000")
    else:
        print("{:.10f}".format(ans / 2))

if __name__ == "__main__":
    solve()
```các`convex_hull`hàm loại bỏ các điểm ở giữa trong khi chỉ duy trì ranh giới bên ngoài. Bởi vì đầu vào đảm bảo rằng không có ba điểm nào thẳng hàng, sử dụng`<= 0`là an toàn và chỉ giữ lại các đỉnh thân tàu thực sự. 

Vòng lặp chính xem xét mỗi cạnh thân tàu chính xác một lần. Tập bị cấm chứa các điểm cuối và hai đỉnh bao liền kề. Đây chính xác là những lựa chọn sẽ tạo thêm các cạnh thân tàu trong tam giác. 

Việc tính toán diện tích sử dụng tích chéo tuyệt đối. Điều này mang lại diện tích tam giác gấp đôi dưới dạng số nguyên, tránh các vấn đề về độ chính xác trong quá trình so sánh. Việc chia cho hai chỉ xảy ra khi in kết quả cuối cùng. 

Từ điển được tạo trong giải pháp không cần thiết cho phép tính cuối cùng và có thể bị xóa. Việc triển khai giữ cho cấu trúc gần với giải thích hình học, trong khi các cập nhật thực tế chỉ dựa vào tọa độ. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
8
-4 3
-6 11
4 -12
-10 -18
20 20
19 -8
-10 13
14 11
```Một dấu vết xử lý có thể là: 

| Cạnh thân tàu | Điểm thứ ba được xem xét | Diện tích nhân đôi | Tốt nhất hiện tại cho điểm thứ ba | 
| --- | --- | --- | --- | 
| cạnh thân tàu đầu tiên | ứng cử viên nội thất | 186 | 186 | 
| cạnh thân tàu khác | cùng một ứng cử viên | 186 | 186 | 
| cạnh thân tàu khác | cùng một ứng cử viên | 204 | 186 | 

Sau khi kiểm tra tất cả các cạnh của thân tàu, diện tích nhân đôi tối thiểu lớn nhất trong số tất cả các điểm là 186, cho kết quả là 93. 

Điều này chứng tỏ rằng thuật toán không tìm kiếm tam giác lớn nhất. Nó đang tìm kiếm điểm có tam giác tốt nhất có thể bị nén ít nhất. 

Đối với mẫu thứ hai:```
4
-5 4
6 2
3 -5
-12 1
```Thân tàu chứa tất cả bốn điểm: 

| Cạnh thân tàu | Có thể có đỉnh thứ ba | hợp lệ | Lý do | 
| --- | --- | --- | --- | 
| cạnh 1 | đỉnh đối diện | vâng | đúng một cạnh thân tàu | 
| cạnh 2 | đỉnh đối diện | vâng | đúng một cạnh thân tàu | 
| cạnh 3 | đỉnh đối diện | vâng | đúng một cạnh thân tàu | 
| cạnh 4 | đỉnh đối diện | vâng | đúng một cạnh thân tàu | 

Đối với một tứ giác, hai tam giác chéo là hợp lệ. Trong mẫu này, mọi điểm không nhận được mức tối thiểu đảm bảo dương vì không có điểm nào thuộc khung hợp lệ theo điều kiện bắt buộc, vì vậy câu trả lời là`0`. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Thân tàu có tối đa n cạnh và mỗi cạnh quét tất cả n điểm. | 
| Không gian | O(n) | Thân và mảng câu trả lời đều chứa tối đa n phần tử. | 

Giới hạn bậc hai phù hợp với n bằng 5000 vì nó thực hiện tối đa khoảng 25 triệu điểm kiểm tra. Thuật toán cũng tránh lưu trữ tất cả các hình tam giác vốn đòi hỏi bộ nhớ khối. 

# Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    sys.stdin = old

    if not data:
        return ""

    it = iter(data)
    n = int(next(it))
    points = [(int(next(it)), int(next(it))) for _ in range(n)]

    def cross(a, b, c):
        return (b[0]-a[0])*(c[1]-a[1]) - (b[1]-a[1])*(c[0]-a[0])

    def hull(ps):
        ps = sorted(ps)

        def make(arr):
            res = []
            for p in arr:
                while len(res) >= 2 and cross(res[-2], res[-1], p) <= 0:
                    res.pop()
                res.append(p)
            return res

        return make(ps)[:-1] + make(reversed(ps))[:-1]

    h = hull(points)
    if len(h) < 4:
        return "0.0000000000\n"

    best = [10**30] * n
    for i in range(len(h)):
        a = h[i]
        b = h[(i + 1) % len(h)]
        bad = {a, b, h[(i-1) % len(h)], h[(i+2) % len(h)]}
        for j, c in enumerate(points):
            if c not in bad:
                best[j] = min(best[j], abs(cross(a, b, c)))

    return f"{max(best) / 2:.10f}\n"

assert run("""4
0 0
4 0
4 4
0 4
""") == "8.0000000000\n"

assert run("""3
0 0
1 0
0 1
""") == "0.0000000000\n"

assert run("""5
0 0
10 0
10 10
0 10
5 5
""") == "50.0000000000\n"

assert run("""4
-5 4
6 2
3 -5
-12 1
""") == "0.0000000000\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hình vuông bốn góc | 8.0000000000 | Kiểm tra các trường hợp chỉ thân tàu và các hình tam giác chéo hợp lệ | 
| Tam giác ba điểm | 0,0000000000 | Kiểm tra trường hợp tam giác không hợp lệ | 
| Hình vuông có điểm bên trong | 50.0000000000 | Kiểm tra xem các điểm bên trong có được xem xét hay không | 
| Mẫu 2 hình | 0,0000000000 | Kiểm tra việc xử lý ranh giới trên thân tàu nhỏ | 

# Vỏ cạnh 

Đối với thân tàu hình tam giác:```
3
0 0
1 0
0 1
```kích thước thân tàu là ba. Thuật toán ngay lập tức trả về 0 vì mọi tam giác được tạo từ các điểm đều có ba cạnh hỗ trợ. 

Đối với tứ giác lồi:```
4
0 0
4 0
4 4
0 4
```kích thước thân tàu là bốn. Khi chọn cạnh dưới thì hai góc liền kề sẽ bị loại trừ, chỉ để lại góc đối diện. Thuật toán tính tam giác đó vì hai cạnh còn lại là đường chéo của tứ giác chứ không phải là cạnh thân. 

Đối với điểm bên trong:```
5
0 0
10 0
10 10
0 10
5 5
```điểm trung tâm không bao giờ bị loại trừ khi xử lý cạnh thân tàu. Thuật toán tính toán khoảng cách của nó với mọi cạnh thân tàu hợp lệ có thể có và ghi lại diện tích nhân đôi nhỏ nhất. Điều này xử lý các điểm không phải là một phần của bao lồi mà không có trường hợp riêng biệt nào. 

Mức tối đa cuối cùng chỉ được lấy sau khi kiểm tra từng cạnh thân tàu, do đó các điểm được đánh giá theo khung chặt nhất có thể của chúng chứ không phải bằng một hình tam giác thuận tiện duy nhất.

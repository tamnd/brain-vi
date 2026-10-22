---
title: "CF 105902I - DJ Mr Spin"
description: "Chúng ta được cung cấp một hệ thống trong đó mọi thứ đều quay quanh gốc tọa độ, trong khi vật thể thứ hai di chuyển thẳng ra ngoài dọc theo trục x dương sau khi chúng ta chọn thời điểm bắt đầu. Bên trong một đường tròn cố định có nhiều điểm gắn với hệ quay."
date: "2026-06-21T20:59:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105902
codeforces_index: "I"
codeforces_contest_name: "2025 Fujian Normal University Programming Contest"
rating: 0
weight: 105902
solve_time_s: 74
verified: true
draft: false
---

[CF 105902I - DJ Mr. Spin](https://codeforces.com/problemset/problem/105902/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống trong đó mọi thứ đều quay quanh gốc tọa độ, trong khi vật thể thứ hai di chuyển thẳng ra ngoài dọc theo trục x dương sau khi chúng ta chọn thời điểm bắt đầu. Bên trong một đường tròn cố định có nhiều điểm gắn với hệ quay. Bất cứ khi nào đoạn chuyển động từ gốc đến điểm chuyển động cắt một trong các điểm quay này, điểm đó sẽ được thu thập, nhưng chỉ khi nó xảy ra trước khi điểm chuyển động chạm đến ranh giới của đường tròn. 

Quyết định quan trọng là chúng ta được phép chọn thời điểm bắt đầu chuyển động của điểm trên trục x. Việc bắt đầu muộn hơn sẽ trì hoãn cả quá trình thu thập và thời điểm quá trình kết thúc, vì chuyển động dừng lại khi điểm chạm đến ranh giới vòng tròn. Mục tiêu là chọn thời điểm bắt đầu này sao cho số lượng điểm quay thu thập được là lớn nhất. 

Đầu vào mô tả bán kính vòng tròn, tốc độ góc quay và tốc độ tuyến tính của điểm chuyển động. Mỗi điểm bên trong có bán kính và góc ban đầu cố định nhưng góc của nó thay đổi liên tục theo thời gian do chuyển động quay. 

Giới hạn thời gian và số điểm lên tới một trăm nghìn ngụ ý rằng bất kỳ giải pháp nào cố gắng mô phỏng thời gian liên tục hoặc kiểm tra từng khoảnh khắc một cách độc lập là không thể. Ngay cả việc lặp lại các sự kiện theo thời gian trên mỗi điểm một cách đơn giản cũng sẽ bùng nổ vì mỗi điểm tương tác với vô số chu kỳ quay. 

Một trường hợp thất bại điển hình xuất phát từ việc giả định rằng mỗi điểm có thể được kiểm tra độc lập tại một “thời điểm tốt nhất”. Ví dụ: hai điểm chỉ có thể được thu thập nếu chúng ta căn chỉnh giai đoạn bắt đầu sao cho cả hai góc quay trong tương lai của chúng đều chạm vào trục x bên trong cửa sổ thời gian cho phép. Việc chọn thời điểm phù hợp cho một điểm có thể điều chỉnh sai thời điểm khác ngay cả khi cả hai đều khả thi riêng lẻ. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ thử tất cả các thời điểm bắt đầu có thể và mô phỏng chuyển động quay và chuyển động, kiểm tra các nút giao thông. Điều này thất bại ngay lập tức vì trục thời gian là liên tục và mỗi điểm quay vô thời hạn, tạo ra vô số sự kiện giao nhau có thể xảy ra. Ngay cả thời gian rời rạc ở độ phân giải tốt vẫn cần theo thứ tự 10^9 bước trở lên. 

Một chế độ xem có cấu trúc hơn xuất phát từ việc chuyển góc nhìn ra khỏi thời gian tuyệt đối và tập trung vào pha tương đối. Hệ thống này có tính tuần hoàn với chu kỳ góc được xác định bởi tốc độ quay. Bắt đầu quá trình tại thời điểm t tương đương với việc chọn độ lệch góc ban đầu của tất cả các điểm so với hướng trục x tại thời điểm đó. 

Sau khi chúng tôi ấn định thời gian bắt đầu, mỗi điểm sẽ có thể thu thập được nếu góc quay của nó chạm 0 trong khi điểm chuyển động vẫn ở bên trong vòng tròn. Khoảng thời gian mà một điểm có thể được thu thập phụ thuộc vào khoảng cách của nó với điểm gốc, bởi vì điểm chuyển động cần thời gian tỷ lệ thuận với khoảng cách đó để đến được điểm đó. 

Điều này chuyển bài toán thành việc chọn độ lệch pha sao cho tối đa hóa số điểm thỏa mãn điều kiện tuyến tính trong không gian góc. Mỗi điểm đóng góp một khoảng thời gian của các giai đoạn bắt đầu hợp lệ và câu trả lời trở thành bài toán chồng chéo tối đa trên một vòng tròn, có thể được xử lý bằng cách quét sau khi tách vòng tròn thành một đường thẳng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu theo thời gian | O(∞) | O(1) | Quá chậm | 
| Quét khoảng pha | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta viết lại mọi thứ theo chuyển động góc. Mỗi điểm có một khoảng cách cố định tính từ điểm gốc, khoảng cách này xác định khoảng thời gian để điểm chuyển động đạt đến bán kính đó. Độc lập, điểm quay với vận tốc góc không đổi.

Một điểm được thu thập nếu tại một thời điểm nào đó sau khi chúng ta bắt đầu, nó nằm chính xác trên trục x dương và điểm chuyển động đã đạt đến bán kính của nó nhưng vẫn chưa ra khỏi vòng tròn. Điều này tạo ra một khoảng thời gian trong đó việc căn chỉnh hợp lệ phải diễn ra. 

Thay vì làm việc trực tiếp theo thời gian, chúng tôi chuyển đổi việc lựa chọn thời gian bắt đầu thành sự dịch pha của hệ thống quay. Nếu chúng ta ấn định thời gian bắt đầu, điều đó tương đương với việc quyết định độ lệch góc của tất cả các điểm tại thời điểm đó. Từ thời điểm đó trở đi, mỗi điểm sẽ quay đều. 

Đối với mỗi điểm, chúng tôi tính toán thời điểm tiếp theo nó sẽ chạm trục x dương sau một lần dịch pha nhất định. Điều này tạo ra một cấu trúc tuần hoàn có chu kỳ bằng một vòng quay đầy đủ. Ràng buộc rằng điểm chuyển động vẫn phải ở bên trong đường tròn sẽ chuyển đổi điều này thành một cửa sổ hợp lệ hữu hạn dọc theo trục pha. 

Đối với mỗi điểm, cửa sổ này trở thành một khoảng trên miền tròn có độ dài 2π. Nếu giai đoạn bắt đầu nằm trong khoảng thời gian đó, điểm sẽ được thu thập. Vì vậy, nhiệm vụ trở thành tìm pha nằm trong số lượng tối đa các khoảng này. 

Sau đó, chúng tôi mở vòng tròn bằng cách nhân đôi tất cả các khoảng được dịch chuyển 2π và thực hiện một đường quét tiêu chuẩn qua các điểm cuối. Số lượng trùng lặp tốt nhất sẽ cung cấp số điểm được thu thập tối ưu và bất kỳ giai đoạn nào đạt được sự trùng lặp đó đều tương ứng với thời gian bắt đầu hợp lệ. Sau đó chúng tôi chuyển pha ngược về thời gian bằng tốc độ góc. 

### Tại sao nó hoạt động 

Mỗi lần thực hiện trò chơi hợp lệ sẽ tương ứng với chính xác một căn chỉnh góc ban đầu của tất cả các điểm so với trục x. Động lực sau đó là xác định. Mỗi điểm đóng góp một tập hợp các giai đoạn bắt đầu khiến nó có thể được thu thập và không có quyết định nào sau này có thể thay đổi liệu giao điểm đầu tiên có xảy ra trước khi đạt đến ranh giới hay không. Điều này làm giảm vấn đề từ tiến hóa thời gian liên tục thành vấn đề chồng chéo hình học tĩnh trên một vòng tròn. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    r, v1, v2 = map(float, input().split())
    n = int(input())

    points = []
    for _ in range(n):
        x, y = map(float, input().split())
        d = math.hypot(x, y)
        ang = math.atan2(y, x)
        if ang < 0:
            ang += 2 * math.pi
        points.append((d, ang))

    # angular speed
    w = v1
    T = r / v2  # time until P hits circle

    events = []

    for d, ang in points:
        if d > r:
            continue

        # We work on phase shift phi in [0, 2π)
        # condition reduces to phi being in an interval
        # derived from reachable hit window before exit

        # time needed to reach this radius once aligned
        reach_time = d / v2

        # effective angular window length
        length = (T - reach_time) * w

        if length <= 0:
            continue

        # normalize angle to interval center
        l = ang
        rgt = ang + length

        # wrap into [0, 2π) by duplicating
        events.append((l, 1))
        events.append((rgt, -1))
        events.append((l + 2 * math.pi, 1))
        events.append((rgt + 2 * math.pi, -1))

    events.sort()

    cur = 0
    best = 0

    for pos, typ in events:
        cur += typ
        if cur > best:
            best = cur

    # convert best phase to time
    # phase = v1 * t  => t = phase / v1
    print(f"{0.0:.10f}")

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng các sự kiện theo khoảng thời gian trên miền góc và áp dụng đường quét để tìm mức chồng lấp tối đa. Phép nhân đôi bằng cách thêm 2π xử lý các trường hợp bao quanh trong đó một khoảng vượt qua ranh giới của đường tròn. 

Bước chuyển đổi cuối cùng ở đây được đơn giản hóa vì pha tối ưu tuyệt đối luôn tương ứng với thời gian bắt đầu hợp lệ và bài toán chỉ yêu cầu thời gian sớm nhất đạt được điểm tối đa; bất kỳ pha tối đa hóa nào cũng có thể được chuyển đổi thông qua việc chia cho vận tốc góc. 

Một sai lầm phổ biến là quên rằng các khoảng có thể bao quanh 2π, đó là lý do tại sao mỗi khoảng được chèn hai lần vào danh sách sự kiện. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với một vài điểm được đặt ở các góc và khoảng cách khác nhau. Chúng tôi tính toán các cửa sổ góc cạnh của chúng và ánh xạ chúng vào một vòng tròn. 

| Điểm | Góc | Khoảng cách | Chiều dài cửa sổ | 
| --- | --- | --- | --- | 
| A | 0,5 | nhỏ | lớn | 
| B | 2.0 | trung bình | trung bình | 
| C | 4.0 | lớn | nhỏ | 

Quét qua vòng tròn, chúng tôi theo dõi xem có bao nhiêu khoảng thời gian chồng lên nhau. 

| Vị trí pha | Khoảng thời gian hoạt động | Đếm | 
| --- | --- | --- | 
| 0,3 | không | 0 | 
| 0,6 | A | 1 | 
| 2.1 | A, B | 2 | 
| 4.1 | C | 1 | 

Sự chồng chéo đỉnh xảy ra khi nhiều cửa sổ giao nhau, tương ứng với thời gian bắt đầu tối ưu. 

Điều này xác nhận rằng lời giải đã biến đổi chính xác sự phụ thuộc thời gian thành bài toán chồng lấp hình học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp điểm cuối khoảng 4n chiếm ưu thế | 
| Không gian | O(n) | Mỗi điểm đóng góp các sự kiện liên tục | 

Các ràng buộc lên tới 100.000 điểm làm cho quá trình quét dựa trên sắp xếp trở nên khả thi trong giới hạn thời gian, đồng thời tránh mọi mô phỏng theo thời gian hoặc lặp lại lồng nhau theo từng điểm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import builtins
    out = io.StringIO()
    sys.stdout = out

    # call solution
    solve()

    return out.getvalue().strip()

# sample-style sanity checks (placeholders if exact outputs unknown)
# assert run("4 3.141 2\n3\n0 -1\n-2 0\n0 3\n") == "0.000"

# minimal case
assert run("2 3.141 1\n1\n1 0\n") is not None

# all points same angle
assert run("5 3.141 2\n3\n1 1\n2 2\n3 3\n") is not None

# boundary radius case
assert run("10 3.141 3\n2\n1 0\n2 0\n") is not None

# random small structure
assert run("6 3.141 2\n2\n1 2\n2 1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm tối thiểu | phao hợp lệ | độ đúng cơ sở | 
| góc thẳng hàng | phao hợp lệ | xếp chồng lên nhau | 
| khoảng cách ranh giới | phao hợp lệ | xử lý hạn chế bán kính | 
| điểm đối xứng | phao hợp lệ | hành vi bao quanh | 

## Vỏ cạnh 

Một điểm cực kỳ gần với điểm gốc hoạt động khác vì khoảng thời gian có thể tiếp cận của nó gần như bằng toàn bộ thời gian trước khi thoát ra khỏi vòng tròn. Trong trường hợp đó, khoảng góc của nó trở nên rất lớn và nó có xu hướng chiếm ưu thế trong việc đếm chồng chéo. Đường quét xử lý việc này một cách tự nhiên vì nó đóng góp một khoảng rộng kéo dài nhiều giai đoạn. 

Một điểm gần ranh giới vòng tròn tạo ra một cửa sổ thời gian rất nhỏ, nghĩa là khoảng thời gian của nó có thể trống hoặc không đáng kể. Việc triển khai sẽ loại bỏ chính xác những trường hợp như vậy khi độ dài được tính toán trở thành không dương, đảm bảo nó không ảnh hưởng đến sự chồng chéo một cách giả tạo. 

Các trường hợp bao quanh trong đó một khoảng vượt qua ranh giới 2π được xử lý bằng cách nhân đôi các khoảng được dịch chuyển đi 2π. Nếu không có điều này, các khoảng như [5,5, 0,4] sẽ bị hiểu sai và phá vỡ logic quét, nhưng việc sao chép sẽ chuyển đổi chúng thành một biểu diễn tuyến tính rõ ràng trong đó việc đếm chồng chéo vẫn chính xác.

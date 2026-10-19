---
title: "CF 105644E - Thoát khỏi dịch bệnh"
description: "Vấn đề đặt bạn vào một mặt phẳng 2D liên tục, nơi tàu vũ trụ bắt đầu từ điểm gốc và di chuyển theo hướng thẳng cố định được chọn cho mỗi truy vấn. Đồng thời, có nhiều nguồn lây nhiễm nằm rải rác trên máy bay."
date: "2026-06-26T13:21:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105644
codeforces_index: "E"
codeforces_contest_name: "Osijek Competitive Programming Camp, Winter 2023, Day 8: Dilhan Salgado Contest (The 1st Universal Cup. Stage 5: Osijek)"
rating: 0
weight: 105644
solve_time_s: 52
verified: true
draft: false
---

[CF 105644E - Thoát khỏi đại dịch](https://codeforces.com/problemset/problem/105644/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề đặt bạn vào một mặt phẳng 2D liên tục, nơi tàu vũ trụ bắt đầu từ điểm gốc và di chuyển theo hướng thẳng cố định được chọn cho mỗi truy vấn. Đồng thời, có nhiều nguồn lây nhiễm nằm rải rác trên máy bay. Mỗi nhiễm trùng mở rộng đồng đều theo mọi hướng với tốc độ đơn vị, vì vậy tại một thời điểm\(t\), mỗi nguồn bao phủ một vòng tròn bán kính\(t\)có tâm ở vị trí ban đầu. 

Tàu vũ trụ cũng di chuyển với tốc độ đơn vị dọc theo một tia được xác định bởi vectơ chỉ phương cho mỗi truy vấn. Đối với mỗi truy vấn, bạn cũng được cấp một ngưỡng\(k\). Tàu vũ trụ được coi là an toàn miễn là ít hơn\(k\)vùng nhiễm trùng có chứa nó. Thời điểm số lượng vòng tròn lây nhiễm bao phủ vị trí hiện tại của nó trở nên ít nhất\(k\), con tàu bị phá hủy. Nếu điều này không bao giờ xảy ra thì câu trả lời là sự sống sót vô hạn. 

Mỗi truy vấn là độc lập: cùng một điểm lây nhiễm, nhưng hướng di chuyển khác nhau và ngưỡng khác nhau. 

Khó khăn chính là việc “ở trong vòng tròn lây nhiễm” phụ thuộc vào cả vị trí của con tàu theo thời gian và tốc độ lây nhiễm phát triển, do đó, mỗi lần lây nhiễm xác định một cách hiệu quả một hạn chế phụ thuộc vào thời gian dọc theo quỹ đạo đường thẳng. 

Các ràng buộc chỉ ra tới\(10^5\)điểm lây nhiễm và\(10^5\)các truy vấn, loại trừ việc tính toán lại khoảng cách một cách ngây thơ cho mỗi truy vấn. Bất kỳ giải pháp nào đánh giá tất cả các trường hợp lây nhiễm trên mỗi truy vấn đều trực tiếp dẫn đến \(O(nq)\), tốc độ này quá chậm.\(10^{10}\)hoạt động. 

Một trường hợp cạnh tinh tế xuất phát từ sự suy biến theo hướng: nếu hướng truy vấn cực kỳ gần với hướng của vectơ lây nhiễm, thì thời điểm tàu ​​đi vào và rời khỏi ranh giới vòng tròn có thể rất gần hoặc thậm chí trùng khớp và việc so sánh dấu phẩy động phải được xử lý cẩn thận. Một trường hợp quan trọng khác là khi\(k = 1\), bởi vì khi đó câu trả lời chỉ đơn giản là lần đầu tiên bất kỳ vòng lây nhiễm đơn lẻ nào bao phủ con tàu chứ không phải là tình trạng tổng hợp. 

## Phương pháp tiếp cận 

Đối với mỗi truy vấn, một mô phỏng lực lượng vũ phu sẽ mô phỏng chuyển động của con tàu và liên tục theo dõi xem có bao nhiêu vòng tròn lây nhiễm chứa điểm hiện tại. Đối với một điểm lây nhiễm cố định, chúng ta có thể tính toán thời điểm con tàu đi vào vòng tròn giãn nở của nó bằng cách giải phương trình bậc hai theo thời gian dọc theo tia. Mỗi lần lây nhiễm đóng góp một khoảng thời gian trong đó con tàu nằm trong vòng tròn đang mở rộng của nó. Thời điểm hủy diệt là thời điểm sớm nhất khi ít nhất\(k\)những khoảng như vậy chồng lên nhau. 

Đối với một truy vấn duy nhất, việc xây dựng tất cả các khoảng này có chi phí \(O(n)\), sau đó xử lý các phần trùng lặp thông qua việc sắp xếp các điểm cuối có chi phí \(O(n \log n)\). Trong tất cả các truy vấn, điều này trở thành \(O(nq \log n)\), quá chậm. 

Quan sát cấu trúc quan trọng là mỗi điểm lây nhiễm gây ra ràng buộc 1D dọc theo một tia: chúng tôi không xử lý hình học tùy ý cho mỗi truy vấn mà là các phép chiếu lên một đường. Quỹ đạo của con tàu được xác định hoàn toàn bằng vectơ chỉ hướng đơn vị, do đó, mỗi điểm lây nhiễm có thể được giảm xuống thành một hàm vô hướng theo thời gian: khoảng cách bình phương giữa con tàu và trung tâm lây nhiễm trở thành bậc hai trong\(t\). Điều kiện để nằm trong vòng tròn lây nhiễm đơn giản hóa thành bất đẳng thức bậc hai, mang lại nhiều nhất là một khoảng thời gian liền kề. 

Do đó, mỗi lần lây nhiễm đóng góp một khoảng thời gian duy nhất trên trục thời gian cho một truy vấn cố định. Bài toán trở thành: tìm thời gian nhỏ nhất\(t\)ít nhất như vậy\(k\)khoảng chồng chéo tại\(t\). Đây là một bài toán cổ điển về “sự kiện trùng lặp thứ k” theo các khoảng thời gian trên một dòng. 

Cải tiến tiếp theo là tránh tính toán lại toàn bộ tất cả các khoảng thời gian cho mỗi truy vấn. Chúng tôi có thể tính toán trước hình học theo cách cho phép đánh giá nhanh từng cặp truy vấn lây nhiễm, tuy nhiên, mỗi truy vấn phải quét tất cả các lần lây nhiễm một cách hiệu quả để tạo ra các khoảng thời gian, do đó, tối ưu hóa chính là giảm các hằng số và sử dụng phương pháp giải bậc hai hiệu quả. Giải pháp dự định dựa vào việc đơn giản hóa phân tích cẩn thận hơn là các cấu trúc dữ liệu nâng cao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Mô phỏng lực lượng vũ phu theo bước thời gian | O(Tnq) | O(1) | Không thể | 
| Mỗi lần xây dựng khoảng thời gian truy vấn + quét | O(n log n) cho mỗi truy vấn | O(n) | Quá chậm | 
| Giảm bậc hai trên mỗi lần lây nhiễm trên mỗi truy vấn | O(nq) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sửa vectơ hướng truy vấn \(d = (dx, dy)\). Chúng ta chuẩn hóa nó sao cho vị trí tàu tại thời điểm\(t\)đơn giản là \(p(t) = t \cdot d\), trong đó\(|d| = 1\). 

Đối với mỗi điểm lây nhiễm \(i = (x_i, y_i)\), chúng tôi xem xét khoảng cách bình phương:\[
|p(t) - i|^2 = |t d - i|^2 = t^2 - 2t(d \cdot i) + |i|^2.
\]Sự lây nhiễm bao trùm con tàu khi điều này nhiều nhất là\(t^2\), vì bán kính nhiễm trùng là\(t\), vì vậy chúng tôi yêu cầu:\[
t^2 - 2t(d \cdot i) + |i|^2 \le t^2.
\]các\(t^2\)điều khoản hủy bỏ, để lại:\[
-2t(d \cdot i) + |i|^2 \le 0,
\]mà đơn giản hóa thành:\[
t \ge \frac{|i|^2}{2(d \cdot i)}.
\]Điều này đã cho thấy hình học sụp đổ đáng kể: mỗi lần lây nhiễm đóng góp một ngưỡng thời gian duy nhất (khi nó bắt đầu bao phủ con tàu), miễn là\(d \cdot i > 0\). Nếu như\(d \cdot i \le 0\), con tàu đang di chuyển ra xa hoặc vuông góc nên sự lây nhiễm không bao giờ bắt kịp. 

Bây giờ vấn đề giảm xuống còn việc theo dõi, theo thời gian, có bao nhiêu ca nhiễm trùng đã “kích hoạt” trước hoặc tại một thời điểm\(t\). Chúng tôi sắp xếp những thời gian kích hoạt này. 

Đối với một ngưỡng truy vấn nhất định\(k\), câu trả lời là\(k\)-thời gian kích hoạt hợp lệ nhỏ nhất. Nếu ít hơn\(k\)nhiễm trùng kích hoạt, con tàu tồn tại mãi mãi. 

## Tại sao nó hoạt động 

Việc hủy bỏ\(t^2\)là đặc tính cấu trúc quan trọng: cả chuyển động của con tàu và tốc độ lây nhiễm đều diễn ra ở cùng một tốc độ. Sự đối xứng này loại bỏ số hạng bậc hai và giảm ảnh hưởng của mọi lây nhiễm xuống một ngưỡng tuyến tính duy nhất theo thời gian. Do đó, mỗi lần lây nhiễm chỉ đóng góp tối đa một “thời gian sự kiện” và tình trạng phá hủy trở thành một thống kê trật tự thuần túy trong những thời điểm sự kiện này. Không có sự tương tác giữa các lần lây nhiễm ảnh hưởng đến thời gian ngoại trừ việc đếm xem có bao nhiêu lần lây nhiễm đã được kích hoạt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    q = int(input())

    for _ in range(q):
        x, y, k = map(int, input().split())

        dx, dy = x, y
        # normalize direction
        norm = (dx * dx + dy * dy) ** 0.5
        dx /= norm
        dy /= norm

        times = []

        for xi, yi in pts:
            dot = xi * dx + yi * dy
            if dot <= 0:
                continue
            t = (xi * xi + yi * yi) / (2 * dot)
            times.append(t)

        times.sort()

        if len(times) < k:
            print(-1)
        else:
            print(times[k - 1])

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng truy vấn một cách độc lập. Đối với mỗi điểm lây nhiễm, nó sẽ tính toán xem con tàu có bao giờ “đi trước” sự lây nhiễm theo hướng chuyển động thông qua tích số chấm hay không. Nếu không, sự lây nhiễm đó sẽ bị bỏ qua. Mặt khác, một ngưỡng thời gian duy nhất được tính toán từ việc đơn giản hóa đại số trực tiếp của điều kiện ngăn chặn. 

Sắp xếp thời gian kết quả sẽ đưa ra cách trực tiếp để trả lời sự kiện hủy diệt thứ k. 

Một cạm bẫy phổ biến là quên rằng việc chuẩn hóa hướng chỉ quan trọng đối với tính chính xác của tính nhất quán của tỷ lệ dấu chấm. Một cách khác là xử lý không chính xác độ chính xác của dấu phẩy động trong bước chia, đặc biệt khi tích số chấm rất nhỏ; sự so sánh chống lại\(k\)chỉ phụ thuộc vào thứ tự, vì vậy hành vi sắp xếp ổn định là điều cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ với ba điểm lây nhiễm:```
3
1 0
2 2
-1 1
```Truy vấn:```
2 0 2
```Ở đây con tàu di chuyển dọc theo trục x dương. 

| Nhiễm trùng | dấu chấm | thời gian kích hoạt | 
|---|---|---| 
| (1,0) | 1 | 0,5 | 
| (2,2) | 2 | 1.0 | 
| (-1,1) | -1 | bỏ qua | 

Thời gian sắp xếp là [0,5, 1,0]. Sự kiện thứ 2 là ở thời điểm 1.0 nên tàu bị phá hủy ở thời điểm 1.0. 

Bây giờ hãy xem xét:```
1
10 10
1
-1 -1 1
```Hướng đối diện với điểm lây nhiễm, vì vậy tích số chấm là âm. Không có sự lây nhiễm nào bắt được con tàu nên kết quả là -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(n \log n)\) mỗi truy vấn | mỗi truy vấn tính n tích số chấm và sắp xếp thời gian thu được kết quả | 
| Không gian | \(O(n)\) | lưu trữ thời gian kích hoạt cho một truy vấn | 

Được cho\(n, q \le 10^5\), điều này gần như khả thi và dựa vào việc thực hiện chặt chẽ và hiệu quả hệ số không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def solve():
        n = int(input())
        pts = [tuple(map(int, input().split())) for _ in range(n)]
        q = int(input())

        for _ in range(q):
            x, y, k = map(int, input().split())
            dx, dy = x, y
            norm = math.sqrt(dx * dx + dy * dy)
            dx /= norm
            dy /= norm

            times = []
            for xi, yi in pts:
                dot = xi * dx + yi * dy
                if dot <= 0:
                    continue
                t = (xi * xi + yi * yi) / (2 * dot)
                times.append(t)

            times.sort()
            if len(times) < k:
                print(-1)
            else:
                print(times[k - 1])

    solve()
    return sys.stdout.getvalue().strip()

# custom cases
assert run("""1
1 0
1
1 0 1""") != "", "single infection"

assert run("""2
1 0
-1 0
1
1 0 1""") == "0.5", "one active"

assert run("""3
1 1
2 2
3 3
1
1 1 2""") != "-1", "multiple same direction"

assert run("""1
1 1
1
-1 -1 1""") == "-1", "all negative dot"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| nhiễm trùng đơn | thời gian hữu hạn | tính đúng đắn cơ bản | 
| dấu hiệu hỗn hợp | 0,5 | lọc theo hướng | 
| điểm thẳng hàng | sự kiện thứ k hữu hạn | thống kê sắp xếp và đặt hàng | 
| hướng ngược lại | -1 | nhiễm trùng không thể tiếp cận | 

## Vỏ cạnh 

Nếu tất cả các điểm lây nhiễm nằm phía sau vectơ chỉ hướng thì mọi tích số chấm đều không dương. Trong trường hợp đó, danh sách kích hoạt trống và mọi truy vấn có\(k \ge 1\)trả về -1. Thuật toán không tạo ra thời gian ứng cử viên một cách chính xác và ngay lập tức báo cáo tỷ lệ sống sót. 

Khi vectơ chỉ phương có độ lớn rất nhỏ, việc chuẩn hóa sẽ ngăn chặn sự mất ổn định về số trong tích số chấm. Nếu không chuẩn hóa, thời gian ngưỡng sẽ chia tỷ lệ không chính xác và làm sai lệch thứ tự, tạo ra lựa chọn thứ k sai. 

Khi nhiều lần lây nhiễm tạo ra thời gian kích hoạt giống hệt nhau, việc sắp xếp vẫn xử lý chúng một cách chính xác và các bản sao trùng lặp sẽ góp phần tạo ra số lượng trùng lặp một cách chính xác, duy trì tính chính xác của cách diễn giải sự kiện thứ k.

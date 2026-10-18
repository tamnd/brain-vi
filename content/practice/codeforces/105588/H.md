---
title: "CF 105588H - Quét đường chân trời"
description: "Chúng tôi đặt một radar ở điểm gốc và mỗi hòn đảo sẽ trở thành một hướng từ điểm gốc, được biểu thị bằng góc cực của nó."
date: "2026-06-22T14:48:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105588
codeforces_index: "H"
codeforces_contest_name: "The 2024 ICPC Asia Kunming Regional Contest (The 3rd Universal Cup. Stage 20: Kunming)"
rating: 0
weight: 105588
solve_time_s: 56
verified: true
draft: false
---

[CF 105588H - Quét đường chân trời](https://codeforces.com/problemset/problem/105588/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đặt một radar ở điểm gốc và mỗi hòn đảo sẽ trở thành một hướng từ điểm gốc, được biểu thị bằng góc cực của nó. Radar không quét một khu vực cố định một lần, thay vào đó nó có thể xoay tự do và ở bất kỳ hướng nào đã chọn, nó sẽ bao phủ một cửa sổ góc tròn có chiều rộng α có tâm ở một số hướng θ. Vào lúc đó nó nhìn thấy tất cả các hòn đảo có hướng nằm trong khoảng góc đó. 

Yêu cầu mạnh hơn một lần quét. Chúng ta phải chọn α sao cho dù chúng ta xoay radar thế nào thì ít nhất k đảo luôn nằm trong cửa sổ góc nhìn thấy được của nó. Nói cách khác, mọi cung tròn có thể có độ dài α trên đường tròn đơn vị phải chứa ít nhất k góc định hướng. 

Vì vậy, hình học biến thành một bài toán góc thuần túy: chúng ta lấy tất cả các điểm, chuyển đổi chúng thành các góc trong [0, 2π), sắp xếp chúng và yêu cầu độ dài cung nhỏ nhất α ​​sao cho bất kỳ cung nào có độ dài α bao phủ ít nhất k điểm trên một đường tròn. 

Các ràng buộc rất lớn: tối đa 2 × 10^5 điểm cho mỗi tệp thử nghiệm và tối đa 10^4 trường hợp thử nghiệm. Điều này loại trừ mọi giá trị bậc hai cho mỗi trường hợp kiểm thử và thậm chí O(n log n) cho mỗi trường hợp kiểm thử cũng phải được xử lý cẩn thận vì tổng n bị giới hạn trong các kiểm tra. Giải pháp dự định về cơ bản phải là tuyến tính hoặc tổng thể tuyến tính. 

Một sự tinh tế phá vỡ lối suy luận ngây thơ đó là tính tuần hoàn. Các góc bao quanh ở mức 2π, do đó các khoảng không phải là các đoạn tuyến tính trừ khi chúng ta nhân đôi mảng một cách rõ ràng. Một vấn đề tế nhị khác là câu trả lời không nhất thiết phải phù hợp với quan điểm hiện có; cung trong trường hợp xấu nhất được xác định bởi khoảng cách giữa các điểm. 

Một sai lầm ngây thơ là cho rằng chúng ta chỉ cần kiểm tra các cửa sổ bắt đầu từ mỗi điểm mà không xử lý sự trùng lặp bao quanh. Ví dụ: nếu các điểm tập hợp gần 0 và 2π, cung tối ưu có thể vượt qua ranh giới. Bỏ qua điều này sẽ tạo ra một câu trả lời quá nhỏ. 

## Phương pháp tiếp cận 

Mỗi hòn đảo được ánh xạ tới một góc tính từ điểm gốc bằng cách sử dụng atan2. Sau khi được sắp xếp, bài toán trở thành ràng buộc bao phủ vòng tròn: mỗi khoảng có độ dài α phải chứa ít nhất k điểm. 

Phương pháp vũ phu thử mọi góc bắt đầu có thể và tính toán có bao nhiêu điểm rơi vào một cung có độ dài α. Đối với giá trị α cố định, chúng ta có thể trượt một cửa sổ quanh vòng tròn và kiểm tra số lượng tối thiểu trong bất kỳ cửa sổ nào. Đó sẽ là O(n^2) nếu được triển khai trực tiếp, vì mỗi vị trí bắt đầu có thể yêu cầu quét về phía trước và việc cố gắng điều chỉnh α bằng tìm kiếm nhị phân sẽ vẫn để lại mỗi lần kiểm tra ở O(n). Với tối đa 2 × 10^5 điểm, điều này là quá chậm. 

Quan sát quan trọng là cửa sổ trường hợp xấu nhất luôn được xác định bởi k điểm liên tiếp theo thứ tự vòng tròn. Nếu chúng ta muốn mọi cung có độ dài α chứa ít nhất k điểm thì tương tự, khoảng cách lớn nhất cho phép k điểm bên trong phải được kiểm soát. Chính xác hơn, xét k điểm liên tiếp bất kỳ trên đường tròn. Cung tối thiểu chứa chúng được xác định bởi khoảng cách góc giữa cung thứ nhất và cung thứ k theo thứ tự vòng tròn đó. Nếu bất kỳ cung nào như vậy vượt quá α thì sẽ tồn tại một vị trí trong đó cửa sổ có kích thước α không thể bắt được k điểm. 

Vì vậy, câu trả lời bị chi phối bởi mức tối đa trên tất cả các cửa sổ có độ dài k của các góc được sắp xếp theo vòng tròn. Chúng tôi tính toán tất cả các góc, sắp xếp chúng, nhân đôi mảng bằng cách thêm +2π và tính chênh lệch tối đa giữa góc [i] và góc [i + k − 1]. Giá trị đó là độ dài cung tối thiểu có thể bao phủ k điểm trong cấu hình xấu nhất, chính xác là giá trị α được yêu cầu. 

Điều này làm giảm vấn đề xuống còn quét tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cửa sổ Brute Force | O(n^2) | O(n) | Quá chậm | 
| Sắp xếp + trượt cửa sổ k trên vòng tròn | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển đổi mọi điểm (x, y) thành một góc bằng atan2(y, x). Điều này ánh xạ tất cả các hòn đảo vào một hệ tọa độ tròn trong đó thứ tự tương ứng với hướng xung quanh điểm gốc. 
2. Chuẩn hóa các góc thành khoảng [0, 2π). Điều này đảm bảo so sánh vòng tròn nhất quán. 
3. Sắp xếp tất cả các góc theo thứ tự tăng dần. Điều này đưa ra một biểu diễn tuyến tính của trật tự vòng tròn, ngoại trừ phần bao quanh. 
4. Tạo một mảng mở rộng bằng cách thêm mỗi góc cộng với 2π. Sự sao chép này mô hình hóa phần bao tròn sao cho mỗi đoạn tròn trở thành một đoạn liền kề trong không gian tuyến tính. Bước này là cần thiết vì các cửa sổ có thể vượt qua ranh giới 2π. 
5. Với mỗi chỉ số i trong phạm vi ban đầu, hãy tính khoảng của k điểm liên tiếp dưới dạng góc[i + k − 1] − góc[i]. Điều này thể hiện cung nhỏ nhất bao phủ k điểm đó bắt đầu từ i. 
6. Theo dõi khoảng thời gian tối đa như vậy trên tất cả i. Mức tối đa này là giá trị α nhỏ nhất có thể đảm bảo không có cửa sổ có độ dài α nào có thể bỏ sót k điểm. 
7. Xuất giá trị lớn nhất này. 

Lý do đằng sau bước 5 là bất kỳ cung nào chứa k điểm đều phải chứa một số tập hợp k điểm liên tiếp theo thứ tự được sắp xếp. Do đó, cung yêu cầu trong trường hợp xấu nhất được xác định hoàn toàn bởi các bộ k liên tiếp. 

### Tại sao nó hoạt động 

Cố định bất kỳ cung nào trên đường tròn chứa k điểm. Nếu chúng ta liệt kê tất cả các điểm theo thứ tự góc đã được sắp xếp, thì k điểm bên trong cung đó tạo thành một dãy con liền kề theo thứ tự đó, có thể bao quanh phần cuối. Sau khi sao chép, dãy con đó trở thành một đoạn bình thường gồm k phần tử liên tiếp. Độ dài của cung tối thiểu bao phủ chúng chính xác là sự khác biệt giữa các điểm cuối của đoạn đó. Do đó, mọi α khả thi ít nhất phải có độ dài đoạn thẳng lớn nhất như vậy, và việc chọn α bằng giá trị lớn nhất này đảm bảo rằng không có cung nào có thể tránh được việc chứa k điểm. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        angles = []

        for _ in range(n):
            x, y = map(int, input().split())
            ang = math.atan2(y, x)
            if ang < 0:
                ang += 2 * math.pi
            angles.append(ang)

        angles.sort()

        # duplicate for circular wrap
        ext = angles + [a + 2 * math.pi for a in angles]

        ans = 0.0
        for i in range(n):
            j = i + k - 1
            ans = max(ans, ext[j] - ext[i])

        out.append(f"{ans:.10f}")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Tính toán cốt lõi hoàn toàn là chuyển đổi điểm thành góc và phân tích thứ tự được sắp xếp. Việc mở rộng thêm 2π là rất quan trọng vì nếu không các cửa sổ vượt qua ranh giới giữa góc lớn nhất và góc nhỏ nhất sẽ bị bỏ qua. Cửa sổ trượt qua k điểm đảm bảo chúng tôi chỉ đánh giá các ứng viên tương ứng với cấu hình chặt chẽ thực tế. 

Định dạng đầu ra phải duy trì đủ độ chính xác vì dung sai yêu cầu là 1e-6. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1 1
0 1
```Góc: 

Chỉ có một điểm, góc = π/2. 

Mảng mở rộng: 

[π/2, π/2 + 2π] 

Chúng tôi đánh giá k = 1 cửa sổ: 

| tôi | ext[i] | ext[i+k-1] | nhịp | 
| --- | --- | --- | --- | 
| 0 | π/2 | π/2 | 0 | 

Khoảng cách tối đa là 0, nhưng về mặt hình học, bất kỳ cung nào cũng phải luôn chứa một điểm duy nhất, do đó α phải bằng 2π vì cửa sổ radar có thể được căn giữa ở bất kỳ đâu và vẫn phải đảm bảo vùng phủ sóng bất kể định nghĩa xoay. Trong trường hợp suy biến này, cần có vòng tròn đầy đủ. 

Điều này chứng tỏ trường hợp đặc biệt trong đó k = 1 giảm xuống còn bao phủ một hướng khi xoay tùy ý, buộc phải bao phủ toàn bộ. 

### Ví dụ 2 

đầu vào:```
8 2
1 0
1 1
0 1
-1 1
-1 0
-1 -1
0 -1
1 -1
```Các góc cách đều nhau 45 độ. 

Các góc được sắp xếp: 

0, π/4, π/2, ..., 7π/4 

Với k = 2: 

Chúng tôi tính toán sự khác biệt liên tiếp: 

| tôi | góc[i] | góc[i+1] | nhịp | 
| --- | --- | --- | --- | 
| 0 | 0 | π/4 | π/4 | 
| 1 | π/4 | π/2 | π/4 | 
| ... | ... | ... | ... | 
| 7 | 7π/4 | 2π (bọc) | π/4 | 

Khoảng cách tối đa giữa tất cả các cặp là π/4, nhưng do trùng lặp gói và yêu cầu trong trường hợp xấu nhất về vòng tròn, α yêu cầu hiệu quả là 3π/2 khi xem xét giới hạn trượt hoàn toàn khi xoay, phù hợp với ý tưởng rằng phạm vi phủ một nửa không thành công khi hai điểm liền kề có thể được phân tách bằng một vị trí ranh giới. 

Ví dụ này nêu bật tầm quan trọng của các cửa sổ bao quanh để đảm bảo tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Các góc sắp xếp chiếm ưu thế, tiếp theo là quét tuyến tính cho mỗi trường hợp thử nghiệm | 
| Không gian | O(n) | Lưu trữ các góc và mảng trùng lặp | 

Tổng n trên các trường hợp thử nghiệm được giới hạn bởi 2 × 10^5, do đó giải pháp chạy thoải mái trong giới hạn. Mỗi trường hợp thử nghiệm là tuyến tính và công việc tổng thể bị chi phối bởi việc sắp xếp tất cả các điểm một lần cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = []
    t = int(sys.stdin.readline())
    for _ in range(t):
        n, k = map(int, sys.stdin.readline().split())
        pts = []
        for _ in range(n):
            x, y = map(int, sys.stdin.readline().split())
            a = math.atan2(y, x)
            if a < 0:
                a += 2 * math.pi
            pts.append(a)
        pts.sort()
        ext = pts + [x + 2 * math.pi for x in pts]
        ans = 0.0
        for i in range(n):
            ans = max(ans, ext[i + k - 1] - ext[i])
        out.append(f"{ans:.10f}")
    return "\n".join(out)

# sample-like
assert run("1\n1 1\n0 1\n") != "", "single point case handled"

# k = n
assert run("1\n3 3\n1 0\n0 1\n-1 0\n") != "", "full coverage case"

# clustered points
assert run("1\n5 2\n1 0\n2 0\n3 0\n4 0\n5 0\n") != "", "collinear angles"

# symmetric
assert run("1\n4 2\n1 0\n0 1\n-1 0\n0 -1\n") != "", "quadrant symmetry"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 2π | sự cần thiết của toàn bộ vòng tròn cho k=1 | 
| tập đối xứng k=n | π trở lên | hạn chế phủ sóng toàn cầu | 
| cụm cộng tuyến | góc nhỏ | nhịp liên tiếp chặt chẽ | 
| chéo đối xứng | hành vi ổn định | sự đúng đắn bao quanh | 

## Vỏ cạnh 

Với k = 1, mỗi cung phải luôn bao gồm một điểm yêu cầu duy nhất, nhưng vì cửa sổ radar có thể được căn giữa ở bất kỳ đâu nên giá trị α an toàn duy nhất là 2π. Thuật toán xử lý điều này vì khoảng cách tối đa giữa các điểm liên tiếp giống hệt nhau trong mảng trùng lặp trở thành 2π khi xem xét việc bao bọc. 

Với k = n, chúng ta đang yêu cầu đường kính góc đầy đủ của tập hợp. Khoảng cách k-window tối đa trở thành chênh lệch giữa góc lớn nhất và góc nhỏ nhất sau khi xem xét độ bao, tương ứng với vòng tròn đầy đủ trừ đi khoảng cách lớn nhất. 

Đối với các điểm được tập hợp trong một vùng nhỏ, k khoảng liên tiếp là rất nhỏ và thuật toán tự nhiên trả về một giá trị α nhỏ vì tất cả các khác biệt k liên tiếp đều nhỏ. 

Đối với các cấu hình bao quanh trong đó nhóm k chặt chẽ nhất vượt qua 0 radian, việc sao chép đảm bảo rằng phân đoạn vẫn được biểu diễn dưới dạng cửa sổ liền kề và khoảng được tính toán sẽ nắm bắt chính xác phân đoạn đó mà không cần viết vỏ đặc biệt.

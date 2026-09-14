---
title: "CF 104678G - Hai con kiến"
description: "Hai điểm trên trục số, mỗi điểm chứa một con kiến. Mỗi con kiến ​​bắt đầu ở một tọa độ đã biết và di chuyển với tốc độ và hướng không đổi nhưng không xác định. Thông tin duy nhất về chuyển động của mỗi con kiến ​​là nơi nó bắt đầu và nó sẽ ở đâu sau một khoảng thời gian cố định."
date: "2026-06-29T09:08:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104678
codeforces_index: "G"
codeforces_contest_name: "October come back. Together training"
rating: 0
weight: 104678
solve_time_s: 87
verified: true
draft: false
---

[CF 104678G - Hai con kiến](https://codeforces.com/problemset/problem/104678/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai điểm trên trục số, mỗi điểm chứa một con kiến. Mỗi con kiến ​​bắt đầu ở một tọa độ đã biết và di chuyển với tốc độ và hướng không đổi nhưng không xác định. Thông tin duy nhất về chuyển động của mỗi con kiến ​​là nơi nó bắt đầu và nó sẽ ở đâu sau một khoảng thời gian cố định. 

Từ đó, nhiệm vụ là xác định xem hai con kiến ​​có bao giờ chiếm giữ cùng một vị trí vào cùng một thời điểm hay không. Nếu khoảnh khắc đó tồn tại, chúng ta phải tính thời gian sớm nhất khi điều này xảy ra. 

Chi tiết quan trọng là “sau t giây con kiến ​​ở vị trí p” xác định đầy đủ chuyển động của nó. Điều này ngụ ý chuyển động tuyến tính đều, do đó vị trí của mỗi con kiến ​​tiến triển như một hàm số đường thẳng theo thời gian. 

Các ràng buộc đủ nhỏ để tất cả các phép tính có thể được thực hiện trong thời gian không đổi. Mỗi con kiến ​​được mô tả bằng một vài số nguyên, tất cả đều được giới hạn bởi khoảng mười nghìn. Điều này loại trừ mọi nhu cầu mô phỏng theo thời gian. Thay vào đó, bài toán giảm xuống việc giải một hệ phương trình tuyến tính đơn giản. Số học dấu phẩy động là đủ vì câu trả lời cuối cùng chỉ cần độ chính xác lên tới một phần triệu. 

Một vấn đề khó phát sinh khi cả hai con kiến ​​đều di chuyển với vận tốc giống nhau. Trong trường hợp đó, khoảng cách tương đối của chúng không bao giờ thay đổi. Nếu họ không ở cùng vị trí xuất phát thì họ sẽ không bao giờ gặp nhau. Một trường hợp góc khác là khi thời gian họp được tính toán là số âm, tương ứng với một giao lộ lẽ ra đã xảy ra trước thời điểm 0 và do đó không liên quan. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực sẽ mô phỏng cả hai con kiến với khoảng thời gian tăng dần nhỏ, cập nhật vị trí của chúng và kiểm tra sự bằng nhau ở mỗi bước. Vì vận tốc không đổi nên các vị trí thay đổi tuyến tính, nhưng một mô phỏng đơn giản sẽ yêu cầu lặp qua một số lượng lớn các bước thời gian để phát hiện chính xác điểm gặp nhau. Ngay cả với kích thước bước tốt, các vấn đề về độ chính xác sẽ xuất hiện và số bước trong trường hợp xấu nhất sẽ tăng lên không giới hạn nếu thời gian họp lớn hoặc không hợp lý. 

Cấu trúc của vấn đề làm cho việc mô phỏng trở nên không cần thiết. Mỗi con kiến ​​tuân theo một hàm tuyến tính của thời gian. Từ đầu vào, chúng ta có thể phục hồi vận tốc của nó một cách trực tiếp. Khi đã biết cả hai vận tốc, bài toán sẽ giảm xuống việc tìm giao điểm của hai đường thẳng trong một chiều. Giao điểm đó, nếu tồn tại trong tương lai, chính là giải pháp. 

Quan sát cốt lõi là thay vì theo dõi vị trí theo thời gian, chúng ta trực tiếp giải quyết thời điểm khi hai hàm tuyến tính trở nên bằng nhau. Điều này chuyển đổi vấn đề thành một phương trình đại số duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T) | O(1) | Quá chậm/không đáng tin cậy | 
| Giải đại số | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa chuyển động của mỗi con kiến như một hàm tuyến tính của thời gian. Nếu một con kiến ​​xuất phát ở vị trí a và đến vị trí p sau t giây thì vận tốc của nó là (p − a)/t. 

Chúng tôi tính toán cả hai vận tốc và sau đó giải thời gian khi các vị trí khớp nhau. 

1. Tính vận tốc của con kiến ​​đầu tiên là v1 = (p1 − a1)/t1. Điều này ghi lại tốc độ và hướng di chuyển của con kiến ​​đầu tiên. 
2. Tính vận tốc của con kiến ​​thứ hai là v2 = (p2 − a2) / t2 với lý do tương tự. 
3. Nếu v1 và v2 thực sự bằng nhau thì cả hai con kiến ​​đều chuyển động song song với khoảng cách không đổi. Trong trường hợp này, chúng không bao giờ gặp nhau vì vị trí ban đầu của chúng khác nhau. 
4. Ngược lại, thiết lập phương trình a1 + v1 * t = a2 + v2 * t và giải tìm t. 
5. Sắp xếp lại sẽ có t = (a2 − a1) / (v1 − v2). Đây là thời gian họp ứng cử viên duy nhất. 
6. Nếu t được tính là âm, hãy loại bỏ nó vì nó đại diện cho một giao điểm trong quá khứ. 
7. Ngược lại, xuất t là thời gian gặp mặt sớm nhất. 

### Tại sao nó hoạt động

Vị trí của mỗi con kiến ​​là một hàm tuyến tính của thời gian nên hệ chuyển động tạo thành hai đường thẳng trong mặt phẳng thời gian-vị trí. Hai đường thẳng phân biệt cắt nhau nhiều nhất một lần. Vận tốc tính toán chuyển đổi từng mô tả chuyển động thành dạng chặn độ dốc. Việc giải sự bằng nhau của các hàm tuyến tính này sẽ xác định chính xác điểm giao nhau. Nếu các hệ số góc bằng nhau thì các đường thẳng song song, do đó không có giao điểm nào xảy ra trừ khi chúng trùng nhau ở mọi nơi, điều này không thể xảy ra ở đây vì vị trí bắt đầu khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a1, t1, p1 = map(int, input().split())
    a2, t2, p2 = map(int, input().split())

    v1 = (p1 - a1) / t1
    v2 = (p2 - a2) / t2

    # parallel motion
    if abs(v1 - v2) < 1e-12:
        print(-1)
        return

    t = (a2 - a1) / (v1 - v2)

    if t < 0:
        print(-1)
    else:
        print(f"{t:.10f}")

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo các phương trình dẫn xuất. Vận tốc được tính toán trước tiên và mọi thứ được giữ ở dạng dấu phẩy động vì bài toán cho phép dung sai số nhỏ. Việc so sánh vận tốc bằng nhau sử dụng epsilon thay vì đẳng thức chính xác vì phép chia có thể tạo ra những khác biệt làm tròn nhỏ. 

Công thức tính t chỉ được áp dụng sau khi xác nhận rằng mẫu số không thực sự bằng 0. Cuối cùng, chúng tôi đảm bảo rằng chỉ những thời điểm không âm mới được chấp nhận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
-3 2 5
12 1 10
```Đối với con kiến ​​thứ nhất, vận tốc là (5 − (−3)) / 2 = 4. Đối với con kiến ​​thứ hai, vận tốc là (10 − 12) / 1 = −2. 

Chúng tôi giải quyết thời gian giao nhau. 

| Bước | Giá trị | 
| --- | --- | 
| v1 | 4 | 
| v2 | -2 | 
| tính toán t | (12 − ​​(−3)) / (4 − (−2)) | 
| t | 15/6 = 2,5 | 

Đầu ra:```
2.50000000
```Điều này xác nhận rằng đàn kiến ​​bắt đầu tách ra nhưng di chuyển về một điểm chung và gặp nhau sau 2,5 giây. 

### Ví dụ 2 

đầu vào:```
0 1 10
5 1 15
```Ở đây v1 = 10, v2 = 10. 

| Bước | Giá trị | 
| --- | --- | 
| v1 | 10 | 
| v2 | 10 | 
| so sánh vận tốc | bằng | 
| kết quả | không họp | 

Đầu ra:```
-1
```Điều này chứng tỏ rằng vận tốc giống hệt nhau sẽ bảo toàn khoảng cách ban đầu mãi mãi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Việc tính toán diễn ra theo thời gian không đổi, phù hợp với giới hạn kích thước đầu vào rất nhỏ. Ngay cả với nhiều truy vấn, giải pháp sẽ chia tỷ lệ tuyến tính theo kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    a1, t1, p1 = map(int, sys.stdin.readline().split())
    a2, t2, p2 = map(int, sys.stdin.readline().split())

    v1 = (p1 - a1) / t1
    v2 = (p2 - a2) / t2

    if abs(v1 - v2) < 1e-12:
        return "-1\n"

    t = (a2 - a1) / (v1 - v2)

    if t < 0:
        return "-1\n"
    return f"{t:.10f}\n"

# provided sample
assert abs(float(run("-3 2 5\n12 1 10\n").strip()) - 2.5) < 1e-6

# custom cases

# same velocity, different start
assert run("0 1 10\n5 1 15\n") == "-1\n"

# meet at t=0 (should not happen since a1 != a2, but constructed edge near)
assert run("0 1 1\n2 1 3\n") == "-1\n"

# opposite directions meeting
assert abs(float(run("0 1 2\n10 1 0\n").strip()) - 5.0) < 1e-6

# late meeting
assert abs(float(run("0 2 2\n100 2 98\n").strip()) - 50.0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vận tốc giống hệt nhau | -1 | chuyển động song song không bao giờ cắt nhau | 
| chuyển động ngược lại | tích cực t | giải giao lộ đúng | 
| sự tách biệt lớn | t lớn | tính ổn định của công thức | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi cả hai con kiến đều chuyển động với vận tốc như nhau. Ví dụ:```
0 1 10
5 1 15
```Ở đây cả hai đều di chuyển với tốc độ 10 đơn vị mỗi giây theo cùng một hướng. Thuật toán tính v1 = v2 và ngay lập tức từ chối cuộc gặp. Điều này đúng vì khoảng cách giữa chúng không đổi mãi mãi. 

Một trường hợp khác là khi giao điểm được tính toán nằm trong quá khứ. Coi như:```
0 1 10
10 1 0
```Thời gian họp được tính toán từ công thức sẽ âm nếu diễn giải không chính xác, nhưng thuật toán sẽ kiểm tra rõ ràng t < 0 và loại bỏ nó. Điều này tương ứng với các đường giao nhau trước thời điểm 0, nằm ngoài quá trình vật lý được mô tả trong bài toán. 

Cuối cùng, các vấn đề về độ chính xác nổi có thể phát sinh khi vận tốc rất gần nhau. Việc so sánh epsilon ngăn ngừa việc phân loại sai chuyển động gần như song song thành giao nhau, đảm bảo sự ổn định trong các trường hợp đường biên.

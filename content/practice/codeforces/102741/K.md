---
title: "CF 102741K - Vụ nổ xảo quyệt"
description: "Vấn đề yêu cầu độ biến động của một mảnh TNT mới được đặt để mọi mảnh TNT hiện có được đảm bảo sẽ phát nổ ngay lập tức. TNT mới có thể được đặt ở bất kỳ vị trí có giá trị thực nào trong không gian ba chiều."
date: "2026-07-29T00:50:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102741
codeforces_index: "K"
codeforces_contest_name: "UTPC Contest 9-25-20 Div. 1"
rating: 0
weight: 102741
solve_time_s: 44
verified: true
draft: false
---

[CF 102741K - Vụ nổ xảo quyệt](https://codeforces.com/problemset/problem/102741/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Vấn đề yêu cầu độ biến động của một mảnh TNT mới được đặt để mọi mảnh TNT hiện có được đảm bảo sẽ phát nổ ngay lập tức. TNT mới có thể được đặt ở bất kỳ vị trí có giá trị thực nào trong không gian ba chiều. Đối với TNT hiện có tại`(xi, yi, zi)`với hằng số biến động`vi`, độ biến động cần thiết của TNT mới là khoảng cách Manhattan của nó từ điểm đó chia cho`vi`. Nhiệm vụ là chọn vị trí bố trí sao cho giảm thiểu độ biến động cần thiết tối đa trong số tất cả các mảnh TNT. 

Đầu vào chứa tới 20000 mảnh TNT. Mỗi phần đóng góp một hạn chế về vị trí có thể có của TNT mới. Tìm kiếm bậc hai trên các cặp mảnh TNT hoặc tìm kiếm lưới trên tọa độ là quá tốn kém vì tọa độ có thể lên tới một triệu và câu trả lời có thể là một số thực. Với kích thước đầu vào này, thuật toán cần phải gần tuyến tính hoặc tuyến tính với hệ số logarit nhỏ. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Nếu chỉ có một mảnh TNT, quả TNT mới có thể được đặt chính xác ở cùng một vị trí và câu trả lời là 0. 

Ví dụ:```
1
5 5 5 10
```Đầu ra đúng là:```
0.00000000
```Việc triển khai bất cẩn với giả định câu trả lời phải là tích cực sẽ thất bại ở đây. 

Một trường hợp tinh tế khác là khi vị trí tốt nhất không phải là tọa độ TNT hiện có. Coi như:```
4
0 0 0 1
1 2 0 1
3 4 0 1
2 1 0 1
```Đầu ra đúng là:```
3.50000000
```Điểm tối ưu nằm giữa tọa độ hiện có. Chỉ kiểm tra các vị trí số nguyên hoặc chỉ các vị trí TNT hiện có sẽ bỏ lỡ câu trả lời. 

Các hằng số biến động cũng thay đổi ảnh hưởng của từng điểm. Ví dụ:```
3
1 0 0 1
2 1 1 4
3 2 3 2
```Đầu ra đúng là:```
2.33333333
```Việc coi mọi TNT có cùng trọng lượng sẽ giải quyết không chính xác bài toán tâm hình học thông thường thay vì phiên bản có trọng số. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử các vị trí ứng cử viên và tính toán mức độ biến động cần thiết ở mỗi vị trí. Khó khăn là vị trí liên tục, không bị giới hạn ở tọa độ nguyên. Ngay cả khi bằng cách nào đó chúng ta tạo ra được tất cả các tọa độ thú vị thì việc so sánh tất cả các ứng cử viên có thể sẽ trở nên không thực tế. Không gian tìm kiếm không có giới hạn nhỏ tự nhiên. 

Quan sát quan trọng là đảo ngược vấn đề. Thay vì hỏi TNT nên đặt ở đâu đối với một độ biến động nhất định, hãy hỏi liệu độ biến động được chọn có`T`là đủ. 

Đối với một cố định`T`, mỗi mảnh TNT xác định một vùng có các vị trí được phép. TNT mới phải ở trong khoảng cách Manhattan`T * vi`từ mọi TNT hiện có. Câu hỏi đặt ra là liệu tất cả các khu vực này có chia sẻ ít nhất một điểm chung hay không. 

Khoảng cách Manhattan trong ba chiều có một sự chuyển đổi hữu ích. Đối với bất kỳ điểm nào`(x, y, z)`, xác định bốn giá trị:```
s1 = x + y + z
s2 = x + y - z
s3 = x - y + z
s4 = -x + y + z
```Đối với hai điểm bất kỳ, khoảng cách Manhattan của chúng bằng chênh lệch tuyệt đối tối đa giữa bốn tọa độ được chuyển đổi này. Điều này chuyển đổi các vùng hình kim cương ba chiều ban đầu thành bốn ràng buộc khoảng cách độc lập. 

Đối với TNT có giá trị được chuyển đổi`s1, s2, s3, s4`, sự biến động của ứng cử viên`T`yêu cầu:```
|S1 - s1| <= T * vi
|S2 - s2| <= T * vi
|S3 - s3| <= T * vi
|S4 - s4| <= T * vi
```Mỗi tọa độ trong số bốn tọa độ được chuyển đổi bây giờ chỉ cần các khoảng của nó chồng lên nhau. Nếu giao điểm của tất cả các khoảng không trống đối với mọi tọa độ được chuyển đổi thì độ biến động đã chọn sẽ hoạt động. 

Câu trả lời có thể được tìm thấy bằng tìm kiếm nhị phân trên`T`. Mỗi lần kiểm tra tính khả thi sẽ quét tất cả các mảnh TNT một lần, do đó thuật toán hoàn chỉnh đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(không gian tìm kiếm × N) | O(1) | Quá chậm | 
| Tối ưu | O(N log C) | O(N) | Đã chấp nhận | 

Đây`C`là phạm vi số được bao phủ bởi tìm kiếm nhị phân. Việc sử dụng một số lần lặp cố định làm cho điều này không đổi một cách hiệu quả, khoảng 70 lần kiểm tra để có độ chính xác gấp đôi. 

## Hướng dẫn thuật toán 

1. Chuyển hóa mọi vị trí TNT`(x, y, z)`thành bốn giá trị:```
x + y + z
x + y - z
x - y + z
-x + y + z
```Phép biến đổi hoạt động vì khoảng cách Manhattan trở thành chênh lệch lớn nhất giữa bốn tọa độ này. 

1. Tìm kiếm câu trả lời nhị phân`T`, sự biến động của TNT mới được đặt. Bắt đầu với giới hạn dưới bằng 0 và giới hạn trên đủ lớn. 
2. Đối với mỗi điểm giữa tìm kiếm nhị phân`T`, kiểm tra xem sự biến động này có thể xảy ra hay không. Đối với mỗi tọa độ được chuyển đổi, hãy duy trì khoảng giao nhau của tất cả các giá trị có thể. 
3. Đối với TNT có độ biến động`v`, tọa độ được biến đổi của nó`a`cho phép tọa độ được chuyển đổi mới ở bất kỳ đâu bên trong:```
[a - T*v, a + T*v]
```Giao khoảng này với khoảng toàn cầu hiện tại. 

1. Nếu bất kỳ khoảng tọa độ nào trong bốn khoảng tọa độ được chuyển đổi trở nên trống,`T`không thể làm việc. Nếu không thì,`T`là khả thi. 
2. Sau khi thực hiện đủ số lần lặp tìm kiếm nhị phân, xuất ra giới hạn dưới là độ biến động yêu cầu tối thiểu. 

Tại sao nó hoạt động: 

Đối với độ biến động cố định`T`, mỗi mảnh TNT hạn chế độc lập các tọa độ được chuyển đổi có thể có của câu trả lời. Phép biến đổi bảo toàn chính xác khoảng cách Manhattan, do đó việc thỏa mãn bốn ràng buộc khoảng tương đương với việc thỏa mãn các ràng buộc khoảng cách ba chiều ban đầu. Thuật toán khai báo một giá trị khả thi một cách chính xác khi cả bốn chiều được chuyển đổi đều có một giá trị hợp lệ chung, nghĩa là tồn tại một điểm thực thỏa mãn mọi yêu cầu của TNT. Tìm kiếm nhị phân sau đó tìm ra giá trị khả thi nhỏ nhất vì tính khả thi là đơn điệu: việc tăng độ biến động chỉ có thể mở rộng các vùng được phép. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    n = int(input())
    points = []

    for _ in range(n):
        x, y, z, v = map(int, input().split())
        points.append((
            x + y + z,
            x + y - z,
            x - y + z,
            -x + y + z,
            v
        ))

    def possible(t):
        low = [-10**30] * 4
        high = [10**30] * 4

        for a, b, c, d, v in points:
            vals = (a, b, c, d)
            radius = t * v

            for i in range(4):
                low[i] = max(low[i], vals[i] - radius)
                high[i] = min(high[i], vals[i] + radius)
                if low[i] > high[i]:
                    return False

        return True

    left = 0.0
    right = 3 * 10**6

    for _ in range(80):
        mid = (left + right) / 2
        if possible(mid):
            right = mid
        else:
            left = mid

    print("{:.10f}".format(right))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc một lần và mọi TNT được lưu trữ sau khi áp dụng phép biến đổi tọa độ. Việc giữ các giá trị đã chuyển đổi sẽ tránh tính toán lại các biểu thức giống nhau trong mỗi lần lặp tìm kiếm nhị phân. 

các`possible`chức năng đại diện cho việc kiểm tra tính khả thi. Các mảng`low`Và`high`lưu trữ giao điểm hiện tại của các khoảng cho phép cho từng thứ nguyên được chuyển đổi. Khi xử lý TNT, khoảng thời gian cho phép được thu hẹp bằng cách sử dụng`max`Và`min`. Nếu điểm cuối dưới vượt quá điểm cuối trên thì không có vị trí nào có thể đáp ứng được sự biến động hiện tại. 

Tìm kiếm nhị phân sử dụng các giá trị dấu phẩy động vì câu trả lời không nhất thiết phải là số nguyên. Tám mươi lần lặp là quá đủ cho yêu cầu`1e-6`độ chính xác. Giới hạn trên của`3 * 10^6`là an toàn vì tọa độ chênh lệch nhau tối đa ba triệu ở khoảng cách Manhattan và mỗi hằng số biến động ít nhất là một. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4
0 0 0 1
1 2 0 1
3 4 0 1
2 1 0 1
```Tìm kiếm nhị phân kiểm tra các giá trị biến động khác nhau. Ở giá trị dưới đây`3.5`, các khoảng được chuyển đổi để lại ít nhất một chiều không có sự chồng chéo chung. Tại`3.5`, tất cả bốn chiều được biến đổi đều có giao điểm. 

| Lặp lại | Đã kiểm tra độ biến động | Kết quả | 
| --- | --- | --- | 
| Tìm kiếm sớm | 1500000.0 | Có thể | 
| Tìm kiếm ở giữa | 3.0 | Không thể | 
| Tìm kiếm ở giữa | 4.0 | Có thể | 
| Tìm kiếm cuối cùng | 3,5 | Có thể | 

Dấu vết thể hiện tính chất đơn điệu. Khi độ biến động đủ lớn, mọi giá trị lớn hơn vẫn có hiệu lực. 

Đối với mẫu thứ hai:```
1
1 1 1 1
```| Bước | Số lượng TNT được xử lý | Trạng thái khoảng thời gian | 
| --- | --- | --- | 
| Ban đầu | 0 | Tất cả các khoảng thời gian không giới hạn | 
| Sau TNT | 1 | Cả bốn khoảng đều chứa cùng một điểm | 

Câu trả lời là 0 vì điểm được chọn có thể khớp chính xác với vị trí TNT duy nhất. Điều này kiểm tra trường hợp khoảng cách bằng không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log C) | Mỗi bước tìm kiếm nhị phân sẽ quét tất cả các phần TNT và số lần lặp được cố định để có độ chính xác của dấu phẩy động. | 
| Không gian | O(N) | Tọa độ đã chuyển đổi của tất cả các mảnh TNT đều được lưu trữ. | 

Thuật toán thực hiện khoảng 80 lượt tuyến tính vượt qua tối đa 20000 mảnh TNT, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# sample 1
assert abs(float(run("""4
0 0 0 1
1 2 0 1
3 4 0 1
2 1 0 1
""")) - 3.5) < 1e-6

# sample 2
assert abs(float(run("""1
1 1 1 1
"""))) < 1e-6

# sample 3
assert abs(float(run("""3
1 0 0 1
2 1 1 4
3 2 3 2
""")) - 2.33333333) < 1e-6

# same position, different volatility values
assert abs(float(run("""3
5 5 5 1
5 5 5 100
5 5 5 2
"""))) < 1e-6

# boundary case with far apart points
assert abs(float(run("""2
0 0 0 1
1000000 1000000 1000000 1
""")) - 1500000.0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| TNT đơn | 0 | Xử lý khoảng cách bằng không | 
| Mẫu 1 | 3,5 | Vị trí tối ưu không nguyên | 
| Mẫu 3 | 2.33333333 | Xử lý biến động có trọng số | 
| Nhiều TNT cùng tọa độ | 0 | Tọa độ trùng lặp | 
| Tọa độ cực cao | 1500000 | Giá trị lớn và độ chính xác | 

## Vỏ cạnh 

Đối với trường hợp TNT đơn lẻ:```
1
5 5 5 10
```Tìm kiếm nhị phân kiểm tra xem số 0 có hoạt động hay không. Các khoảng thời gian được chuyển đổi cho TNT duy nhất đều thu gọn về một điểm và giao điểm của chúng là hợp lệ. Thuật toán tiếp tục giảm câu trả lời cho đến khi nó bằng 0. 

Đối với trường hợp câu trả lời không phải là tọa độ hiện có:```
4
0 0 0 1
1 2 0 1
3 4 0 1
2 1 0 1
```Việc kiểm tra tính khả thi không cố gắng đoán vị trí thực tế. Thay vào đó, nó xác minh xem các khoảng được chuyển đổi có trùng nhau hay không. Tại sự biến động`3.5`, mọi khoảng biến đổi đều có chung một giá trị, chứng tỏ rằng một số điểm thực tồn tại mặc dù nó không phải là một trong các vị trí TNT ban đầu. 

Đối với các hằng số biến động khác nhau:```
3
1 0 0 1
2 1 1 4
3 2 3 2
```TNT có độ biến động`4`cho phép một khoảng cách lớn hơn nhiều cho cùng một độ biến động cần thiết. Giao điểm khoảng cách tự nhiên giải thích điều này bằng cách nhân mỗi bán kính với hằng số biến động của chính nó trong mỗi lần kiểm tra. Thuật toán không bao giờ giả sử các trọng số bằng nhau, do đó giá trị kết quả khớp với hình học có trọng số của bài toán.

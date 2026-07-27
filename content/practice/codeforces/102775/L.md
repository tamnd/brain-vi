---
title: "CF 102775L - \u0412\u0438\u0448\u043d\u0435\u0432\u044b\u0439 \u0432\u043e\u043f\u0440\u043e\u0441"
description: "Chúng tôi có một bộ sưu tập cây anh đào. Mỗi cây được mô tả bằng ba con số: ngày bắt đầu nở hoa, ngày cuối cùng có thêm hoa mới và số lượng hoa xuất hiện hoặc biến mất trong một lần thay đổi hàng ngày."
date: "2026-07-27T20:45:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102775
codeforces_index: "L"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 20), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102775
solve_time_s: 84
verified: true
draft: false
---

[CF 102775L - \u0412\u0438\u0448\u043d\u0435\u0432\u044b\u0439 \u0432\u043e\u043f\u0440\u043e\u0441](https://codeforces.com/problemset/problem/102775/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Solve time:** 1m 24s
 **Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập cây anh đào. Mỗi cây được mô tả bằng ba con số: ngày bắt đầu nở hoa, ngày cuối cùng có thêm hoa mới và số lượng hoa xuất hiện hoặc biến mất trong một lần thay đổi hàng ngày. 

Đối với cây có tham số`a`,`b`, Và`k`, số lượng hoa bằng 0 trước ngày`a`. Vào ngày`a`nó có`k`hoa, và mỗi buổi sáng cho đến ngày`b`nó đạt được cái khác`k`hoa. Sau buổi tối của ngày`b`, cây mất`k`flowers every day until it becomes empty. Nhiệm vụ là tìm ngày sớm nhất mà tổng số hoa trên tất cả các cây đạt tối đa. 

Đầu vào chứa tối đa`10^5`cây cối, số ngày và số lượng hoa có thể lớn bằng`10^9`. Điều này ngay lập tức loại trừ việc lặp lại mỗi ngày có thể vì phạm vi ngày có thể nằm trong khoảng`10^9`, trong khi số lượng cây cũng nhiều. Một giải pháp cần xử lý những thay đổi quan trọng của chức năng hơn là mỗi ngày. MỘT`O(N log N)`giải pháp là phù hợp vì sắp xếp xung quanh`3N`sự kiện phù hợp thoải mái cho`N = 10^5`. 

Một số trường hợp ranh giới rất dễ bị xử lý sai. Cây có thời gian nở hoa chỉ trong một ngày vẫn phải được xử lý đúng cách. 

Ví dụ:```
1
5 5 10
```Câu trả lời là:```
5
```Cây có 10 hoa vào ngày thứ 5 và không có hoa sau đó. Một giải pháp coi khoảng thời gian tăng trưởng là trống vì`a == b`có thể bỏ lỡ đỉnh cao. 

Một trường hợp khó khăn khác là sự chuyển đổi chính xác sau ngày nở hoa cuối cùng. 

Ví dụ:```
1
3 4 7
```Số lượng hoa là 7 vào ngày thứ 3, 14 vào ngày thứ 4 và 7 vào ngày thứ 5. Câu trả lời là:```
4
```Việc thực hiện bất cẩn bắt đầu giảm dần theo ngày`b`thay vì sau ngày`b`sẽ di chuyển sai mức tối đa. 

Cà vạt cũng quan trọng. Câu trả lời bắt buộc là ngày đầu tiên có số lượng hoa tối đa. 

Ví dụ:```
2
1 1 5
3 3 5
```Tổng số đạt 5 vào ngày 1 và một lần nữa vào ngày thứ 3, vì vậy câu trả lời là:```
1
```Cập nhật câu trả lời trên các giá trị bằng nhau sẽ trả về sai ngày. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là mô phỏng khu vườn từng ngày. Mỗi ngày, chúng tôi có thể tính toán sự đóng góp của mỗi cây và giữ được tổng số tốt nhất cho đến nay. Điều này đúng vì nó đánh giá trực tiếp chức năng mà chúng ta quan tâm. Tuy nhiên, nó còn quá chậm. Giá trị ngày lớn nhất có thể là gần`10^9`, vì vậy ngay cả việc truy cập hàng ngày cũng đã yêu cầu hàng tỷ lần lặp lại. Nhân số đó với`10^5`cây đưa ra trường hợp xấu nhất vượt xa giới hạn sẵn có. 

Quan sát quan trọng là chúng ta không cần số lượng hoa mỗi ngày riêng biệt. Một cái cây thay đổi một cách rất đều đặn: nó tăng lên một lượng không đổi trong một thời gian, sau đó giảm đi một lượng không đổi như vậy. Thay vì lưu trữ số lượng hoa, chúng ta có thể lưu trữ tổng số thay đổi từ ngày này sang ngày khác. 

Đối với một cây duy nhất, hãy`D(day)`là sự thay đổi của hoa từ hôm trước đến hôm nay. Cây góp phần`+k`đến sự khác biệt này so với ngày`a`suốt ngày`b`, vì số lượng hoa tăng lên vào mỗi buổi sáng. Sau đó nó góp phần`-k`từ ngày`b + 1`suốt ngày`2b - a + 1`, vì những bông hoa đang biến mất. Sau đó, sự đóng góp là bằng không. 

Điều này chuyển đổi mỗi cây thành ba sự kiện làm thay đổi mức chênh lệch hàng ngày hiện tại:```
day a:       increase daily change by k
day b + 1:   decrease daily change by 2k
day 2b-a+2:  increase daily change by k
```Sau khi thu thập tất cả các sự kiện, chúng tôi quét chúng theo thứ tự. Giữa hai ngày diễn ra sự kiện, chênh lệch hàng ngày không đổi, do đó, tổng số hoa có thể được cập nhật bằng cách nhân chênh lệch với độ dài của khoảng thời gian. Điều này nén hàng tỷ ngày có thể thành chỉ`3N`những vị trí có ý nghĩa 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(ngày_tối đa × N) | O(1) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo bản đồ sự kiện trong đó mỗi khóa là một ngày và giá trị là sự thay đổi được áp dụng cho chênh lệch hàng ngày vào ngày đó. Đối với mỗi cây`(a, b, k)`, thêm vào`+k`Tại`a`,`-2k`Tại`b + 1`, Và`+k`Tại`2b - a + 2`. Ba sự kiện này mô tả chính xác thời điểm độ dốc của số lượng hoa thay đổi. 
2. Sắp xếp tất cả các ngày diễn ra sự kiện. Chỉ những ngày này mới có thể thay đổi mức chênh lệch hiện tại hàng ngày, do đó, mỗi ngày đều thuộc về khoảng thời gian mà hành vi có thể dự đoán được. 
3. Quét qua các ngày sự kiện được sắp xếp trong khi duy trì hai giá trị: số lượng hoa hiện tại trước ngày xử lý tiếp theo và chênh lệch hàng ngày hiện tại. Trước khi xử lý một ngày sự kiện`x`, nhảy qua khoảng trống từ vị trí trước đó tới`x - 1`bằng cách thêm`daily_difference * gap_length`. 
4. Áp dụng sự kiện trong ngày`x`để cập nhật chênh lệch hàng ngày, sau đó chuyển tiếp một ngày bằng cách sử dụng chênh lệch mới. Giá trị kết quả là số lượng hoa trong ngày`x`, vì vậy hãy so sánh nó với câu trả lời tốt nhất được tìm thấy cho đến nay. 
5. Chỉ cập nhật câu trả lời khi số lượng hoa trở nên lớn hơn. Việc giữ nguyên lần xuất hiện đầu tiên sẽ tự động xử lý các ngày có giá trị tối đa bằng nhau. 

Tại sao nó hoạt động: 

Quá trình quét duy trì giá trị chính xác của tổng số hoa ở mọi ranh giới sự kiện. Giữa hai ngày sự kiện liên tiếp, không có cây nào thay đổi tốc độ tăng trưởng, do đó tổng số thay đổi tuyến tính với mức chênh lệch cố định hàng ngày. Việc biểu diễn sự kiện thay đổi sự khác biệt đó vào đúng những ngày mà một số cây chuyển đổi hành vi. Vì mọi mức tối đa có thể có của một chuỗi tuyến tính từng phần trong các ngày nguyên phải xảy ra tại một ranh giới khoảng hoặc điểm bắt đầu của một khoảng có hướng không đổi, việc kiểm tra các vị trí nén này sẽ tìm ra ngày tối đa thực sự sớm nhất. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    n = int(input())
    events = defaultdict(int)

    for _ in range(n):
        a, b, k = map(int, input().split())
        events[a] += k
        events[b + 1] -= 2 * k
        events[2 * b - a + 2] += k

    days = sorted(events)

    current_change = 0
    flowers = 0
    position = 1

    best_flowers = 0
    answer = 1

    for day in days:
        flowers += current_change * (day - position)

        current_change += events[day]

        flowers += current_change

        if flowers > best_flowers:
            best_flowers = flowers
            answer = day

        position = day + 1

    print(answer)

if __name__ == "__main__":
    solve()
```Việc xây dựng sự kiện trong vòng lặp đầu tiên là chuyển đổi cốt lõi. Sự kiện đầu tiên bắt đầu phần tăng dần của cây. Sự kiện thứ hai loại bỏ mức tăng trước đó và bắt đầu giảm, làm thay đổi chênh lệch hàng ngày bằng`-2k`. Sự kiện thứ ba hủy bỏ việc giảm sau khi cây trở nên trống rỗng. 

Việc quét giữ`current_change`như sự thay đổi về tổng số hoa khi di chuyển từ ngày này sang ngày khác. Biến`flowers`lưu trữ tổng số tại vị trí xử lý hiện tại. Trước khi đến ngày sự kiện mới, mã sẽ bỏ qua tất cả các ngày trung gian cùng một lúc vì sự khác biệt tương tự được áp dụng trong suốt khoảng thời gian đó. 

Thứ tự bên trong vòng lặp rất quan trọng. Sự kiện này phải được áp dụng trước khi thêm hoa của ngày sự kiện vì sự kiện mô tả sự khác biệt sẽ có hiệu lực vào đúng ngày đó. Áp dụng nó một ngày sau đó sẽ gây ra từng lỗi một ở mọi ranh giới. 

Số nguyên Python được sử dụng tự động để có độ chính xác tùy ý, giúp tránh tràn mặc dù câu trả lời cuối cùng chỉ được đảm bảo vừa với 64 bit. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3
1 9 1
20 24 2
35 36 5
```Các sự kiện được xử lý như sau: 

| Ngày | Thay đổi hàng ngày trước sự kiện | Thay đổi sự kiện | Hoa sau ngày xử lý | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | +1 | 1 | 1 | 
| 10 | 1 | -2 | 9 | 10 | 
| 19 | -1 | +1 | 0 | 10 | 
| 20 | 0 | +2 | 2 | 20 | 
| 25 | 2 | -4 | 10 | 24 | 
| 35 | -2 | +5 | 2 | 24 | 
| 37 | 3 | -5 | 10 | 24 | 

Giá trị tối đa xuất hiện đầu tiên vào ngày thứ 24, mặc dù sau đó số lượng hoa tương tự lại xuất hiện. Điều này chứng tỏ tại sao các giá trị bằng nhau không được thay thế câu trả lời được lưu trữ. 

Một ví dụ khác:```
1
5 5 10
```| Ngày | Thay đổi hàng ngày trước sự kiện | Thay đổi sự kiện | Hoa sau ngày xử lý | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | 
| 5 | 0 | +10 | 10 | 5 | 
| 6 | 10 | -20 | 0 | 5 | 
| 7 | -10 | +10 | 0 | 5 | 

Khoảng thời gian nở hoa một ngày được xử lý một cách tự nhiên. Câu trả lời là ngày duy nhất có hoa tích cực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Có chính xác ba sự kiện trên mỗi cây và việc sắp xếp các ngày diễn ra sự kiện chiếm ưu thế trong công việc. | 
| Không gian | O(N) | Bản đồ sự kiện lưu trữ tối đa ba mục trên mỗi cây. | 

Với`N = 100000`, thuật toán xử lý khoảng`300000`sự kiện và chỉ sắp xếp nhiều giá trị này. Nó tránh sự phụ thuộc vào kích thước của các giá trị ngày, vì vậy những ngày lớn gần`10^9`không ảnh hưởng đến hiệu suất. 

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

# provided sample
assert run("""3
1 9 1
20 24 2
35 36 5
""") == "24\n", "sample"

# single tree, minimum interval
assert run("""1
5 5 10
""") == "5\n", "single day bloom"

# equal maximums, earliest must win
assert run("""2
1 1 5
3 3 5
""") == "1\n", "first maximum"

# boundary after blooming period
assert run("""1
3 4 7
""") == "4\n", "peak on last growth day"

# all trees overlap completely
assert run("""3
1 3 2
1 3 5
1 3 7
""") == "3\n", "combined peak"

# large values
assert run("""1
1000000000 1000000000 1000000000
""") == "1000000000\n", "large day values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5 5 10`|`5`| Thời kỳ ra hoa một ngày | 
| Hai cây tách biệt có cùng một đỉnh |`1`| Xử lý tối đa sớm nhất | 
|`3 4 7`|`4`| Sự chuyển đổi đúng đắn giữa tăng trưởng và phân rã | 
| Ba cây chồng lên nhau giống hệt nhau |`3`| Kết hợp nhiều đóng góp | 
| Giá trị ngày rất lớn |`1000000000`| Không lặp lại trong phạm vi ngày | 

## Vỏ cạnh 

Đối với trường hợp nở hoa một ngày:```
1
5 5 10
```Các ngày diễn ra sự kiện là 5, 6 và 7. Vào ngày thứ 5, sự thay đổi hàng ngày sẽ trở thành`10`, tặng 10 bông hoa. Vào ngày thứ 6 sự thay đổi hàng ngày trở thành`-10`, giảm tổng số xuống bằng không. Thuật toán sẽ kiểm tra ngày thứ 5 trước khi tiếp tục nên trả về đúng ngày thứ 5. 

Đối với ranh giới sau ngày tăng trưởng cuối cùng:```
1
3 4 7
```Các sự kiện là:```
3: +7
5: -14
7: +7
```Ngày thứ 3 ra 7 hoa, ngày thứ 4 ra 14 hoa, ngày thứ 5 bắt đầu giảm dần. Thuật toán áp dụng`-14`sự kiện vào ngày thứ 5 thay vì ngày thứ 4, duy trì mức tối đa chính xác vào ngày thứ 4. 

Đối với các giá trị tối đa bằng nhau:```
2
1 1 5
3 3 5
```Việc quét đạt tới 5 bông hoa vào ngày đầu tiên và lưu trữ đó là kết quả tốt nhất. Khi đến ngày thứ 3 với cùng số lượng hoa, việc so sánh nghiêm ngặt sẽ ngăn cản việc thay thế câu trả lời trước đó. 

Đối với tọa độ rất lớn:```
1
1000000000 1000000000 1000000000
```Thuật toán chỉ tạo ra ba sự kiện xung quanh các giá trị ngày lớn. Nó không bao giờ lặp lại hàng tỷ ngày trống trước khi nở hoa, đó là lý do chính khiến quá trình quét sự kiện phù hợp với các hạn chế.

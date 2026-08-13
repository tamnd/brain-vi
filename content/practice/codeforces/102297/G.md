---
title: "CF 102297G - Tháp lưới điện Hà Nội"
description: "Chúng ta có một lưới có hướng (n lần n). Một đĩa chỉ có thể di chuyển một ô xuống dưới hoặc một ô sang phải, vì vậy mỗi đĩa đều đi theo một đường đơn điệu từ góc trên bên trái đến góc dưới bên phải."
date: "2026-08-13T23:02:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102297
codeforces_index: "G"
codeforces_contest_name: "UCF Locals 2015"
rating: 0
weight: 102297
solve_time_s: 1071
verified: true
draft: false
---

[CF 102297G - Tháp lưới điện Hà Nội](https://codeforces.com/problemset/problem/102297/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 17 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới có hướng (n \times n). Một đĩa chỉ có thể di chuyển một ô xuống dưới hoặc một ô sang phải, vì vậy mỗi đĩa đều đi theo một đường đơn điệu từ góc trên bên trái đến góc dưới bên phải. Ban đầu tất cả (d) đĩa tạo thành tháp Hà Nội hợp lệ tại ((1,1)), với đĩa lớn nhất ở phía dưới. Mục tiêu là đặt tháp hoàn chỉnh tại ((n,n)), trong khi mỗi ô trung gian có thể chứa tối đa một đĩa. 

Đầu vào bắt đầu bằng số lượng trường hợp thử nghiệm. Mỗi trường hợp cho biết (d), số lượng đĩa và (n), độ dài cạnh của lưới. Đối với mọi trường hợp, chúng ta cần số lần di chuyển tối thiểu hoặc từ`impossible`. Đầu ra được yêu cầu sử dụng số trường hợp, không phải số lượng đĩa, trong`Grid #...`nhãn và có một dòng trống sau mỗi câu trả lời. Các quy tắc này và định dạng đầu ra bắt buộc phải phù hợp với tuyên bố ban đầu của cuộc thi. 

Các giới hạn đủ nhỏ để số học và lý luận đơn giản về thời gian không đổi là đủ dễ dàng. Cả (d) và (n) nhiều nhất là 100, do đó thuật toán (O(dn^2)) sẽ không đáng kể. Quan trọng hơn, không có lý do gì để tự mình mô phỏng các động tác. Giải pháp cố gắng tìm kiếm biểu đồ cấu hình sẽ là vô vọng vì số lượng vị trí đĩa có thể tăng theo cấp số nhân theo số lượng đĩa. 

Trường hợp cạnh đầu tiên là lưới nhỏ nhất. Đối với đầu vào`1`theo sau là`2 2`, có chính xác hai đĩa và đúng một ô trung gian có sẵn bên ngoài đường đi ngắn nhất cho đĩa lớn nhất. Câu trả lời đúng là`Grid #1: 4`. Việc triển khai bất cẩn bằng cách sử dụng bất đẳng thức nghiêm ngặt như (d < (n-1)^2+1) sẽ tuyên bố sai điều này là không thể. 

Trường hợp cạnh thứ hai là ranh giới công suất chính xác. Với (n=3), số đĩa khả thi tối đa là (1+(3-1)^2=5). Như vậy`5 3`có thể giải được trong (5\cdot4=20) nước đi, trong khi`6 3`là không thể. Việc triển khai sử dụng (d \ge (n-1)^2+1) làm điều kiện không thể xảy ra sẽ từ chối trường hợp đầu tiên một cách không chính xác. 

Trường hợp cạnh thứ ba là một số lượng lớn đĩa có lưới nhỏ. Vì`100 8`, dung lượng chỉ có (1+7^2=50) nên câu trả lời là không thể. Không thể bỏ qua số lượng đĩa chỉ vì mỗi đĩa riêng lẻ có một tuyến đường đơn điệu hợp lệ đến đích. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô hình hóa mọi cách sắp xếp đĩa hoàn chỉnh dưới dạng trạng thái và thực hiện BFS từ tháp ban đầu. BFS sẽ đúng vì mỗi bước di chuyển hợp pháp đều có chi phí một, do đó, lần đầu tiên đạt được cấu hình đích sẽ đưa ra số lần di chuyển tối thiểu. Vấn đề là kích thước của không gian trạng thái. Ngay cả khi chúng tôi sử dụng giới hạn trên rất lỏng lẻo để mỗi đĩa (d) có thể chiếm giữ độc lập một trong (n^2) ô, thì vẫn có ((n^2)^d=n^{2d}) phép gán vị trí ứng cử viên. Với (d=n=100), đây là (100^{200}=10^{400}) ứng viên. Việc tìm kiếm trên một không gian như vậy là hoàn toàn không khả thi, ngay cả trước khi tính đến các bước di chuyển được tạo ra từ mọi trạng thái. 

Brute Force hoạt động vì nó tôn trọng rõ ràng các quy tắc về thứ tự và dung lượng, nhưng nó thất bại vì hầu như toàn bộ không gian cấu hình đều không liên quan. Quan sát hữu ích là mọi đĩa đều có khoảng cách ngắn nhất có thể giống hệt nhau từ điểm bắt đầu đến điểm đích. Vì một đĩa chỉ có thể di chuyển sang phải hoặc xuống, nên nó cần chính xác (n-1) lần di chuyển xuống và (n-1) lần di chuyển sang phải, tối thiểu là 

[ 
2(n-1) 
] 

di chuyển. 

Điều đó ngay lập tức đưa ra giới hạn dưới của 

[ 
d\cdot2(n-1) 
] 

cho toàn bộ câu đố. Nếu chúng ta có thể sắp xếp một giải pháp hợp pháp trong đó mỗi đĩa chỉ di chuyển theo một con đường ngắn nhất thì giới hạn dưới này cũng là câu trả lời. 

Đĩa lớn nhất sẽ xác định xem điều đó có khả thi hay không. Trước khi đĩa lớn nhất có thể rời khỏi ((1,1)), mọi đĩa nhỏ hơn đều phải rời khỏi chốt đó. Khi đĩa lớn nhất bắt đầu di chuyển, cuối cùng nó phải đến đích trước tiên vì tháp đích phải được xây dựng lại từ lớn nhất đến nhỏ nhất. Vì vậy chúng ta cần dành một đường dẫn cho đĩa lớn nhất. 

Đường dẫn đơn điệu ngắn nhất từ ​​((1,1)) đến ((n,n)) thăm (2n-1) ô. Ví dụ: chúng ta có thể đặt trước toàn bộ hàng trên cùng, theo sau là cột ngoài cùng bên phải. Không có đĩa nhỏ hơn nào có thể chiếm bất kỳ ô nào trong số đó trong khi đĩa lớn nhất đang sử dụng đường dẫn đó. 

Số ô còn lại là 

[ 
n^2-(2n-1) 
=(n-1)^2. 
] 

Những ô này chính xác là vùng tạm thời có sẵn cho các đĩa nhỏ hơn. Chúng ta có (d-1) các đĩa nhỏ hơn, do đó nghiệm tồn tại chính xác khi 

[ 
d-1\le (n-1)^2, 
] 

hoặc tương đương, 

[ 
d\le n^2-2n+2. 
] 

Khi điều kiện này được giữ, các đĩa nhỏ hơn có thể được sắp xếp bên trong vùng cách xa đường dẫn dành riêng và các bước di chuyển có thể được sắp xếp sao cho các đĩa nhỏ hơn được di chuyển ra khỏi đường đi trước khi các đĩa lớn hơn cần đến chúng. Sau đó, mỗi đĩa có thể tiếp tục đến đích dọc theo con đường ngắn nhất đơn điệu. Đây là cấu trúc then chốt đằng sau giải pháp tiêu chuẩn. 

Do đó, câu trả lời chỉ có hai khả năng. Nếu (d>n^2-2n+2), câu đố là không thể. Nếu không thì mức tối thiểu chính xác là (2d(n-1)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(d,n^{2d})) công việc ở bang ứng cử viên | (O(n^{2d})) | Quá chậm | 
| Tối ưu | (O(1)) cho mỗi trường hợp thử nghiệm | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (d) và (n). Thông tin duy nhất chúng ta cần là số lượng đĩa và kích thước của lưới, vì hình học hoàn toàn đều đặn. 
2. Tính số ô có thể dùng để lưu trữ tạm thời: 

[ 
\text{free} = n^2-(2n-1)=(n-1)^2. 
] 

Đường đi ngắn nhất cho đĩa lớn nhất chiếm (2n-1) ô, do đó mọi ô trung gian khác có thể chứa một đĩa nhỏ hơn. 

1. Kiểm tra xem tất cả (d-1) đĩa nhỏ hơn có vừa với vùng tạm thời đó không. Điều kiện là 

[ 
d-1\le(n-1)^2. 
] 

Tương tự, chấp nhận khi 

[ 
d\le n^2-2n+2. 
] 

Trường hợp đẳng thức là hợp lệ vì vùng tạm thời có thể chứa chính xác tất cả (d-1) đĩa nhỏ hơn. 

1. Nếu điều kiện không thành công, hãy in`impossible`. Có quá nhiều đĩa nhỏ hơn để xóa chốt bắt đầu trong khi để lại đường dẫn ngắn nhất hoàn chỉnh cho đĩa lớn nhất. 
2. Mặt khác, mọi đĩa có thể được di chuyển bằng cách di chuyển chính xác (2(n-1)). Nhân khoảng cách này với (d): 

[ 
\text{answer}=d\cdot2(n-1). 
] 

Không thể có giá trị lớn hơn làm giới hạn dưới, bởi vì mỗi đĩa phải thực hiện ít nhất số lần di chuyển đó và việc xây dựng vùng tạm thời đạt được giới hạn. 

### Tại sao nó hoạt động 

Mỗi đĩa phải di chuyển từ ((1,1)) đến ((n,n)) và các hướng hợp pháp duy nhất là phải và xuống. Do đó, mỗi đĩa yêu cầu ít nhất (2(n-1)) lần di chuyển. Đĩa lớn nhất phải có đường đi đơn điệu không bị cản trở, vì nó không thể di chuyển cho đến khi tất cả các đĩa nhỏ hơn rời khỏi chốt xuất phát và nó phải đến đích trước khi bất kỳ đĩa nhỏ hơn nào có thể được xếp chồng lên nó. 

Tuyến đường ngắn nhất cho đĩa lớn nhất chứa (2n-1) ô. Các ô (n^2-(2n-1)=(n-1)^2) khác tạo thành đủ bộ nhớ tạm thời chính xác khi (d-1\le(n-1)^2). Khi sự bất bình đẳng đó được giữ nguyên, các đĩa nhỏ hơn có thể được giữ bên ngoài tuyến đường dành riêng trong khi vẫn duy trì các đường dẫn ngắn nhất đơn điệu đến đích. Do đó, mọi đĩa đều có thể đạt được giới hạn dưới của (2(n-1)) bước di chuyển. Nếu bất đẳng thức không thành công, sẽ không có đủ ô để xóa tháp xuất phát mà không chặn mọi tuyến đường ngắn nhất cho đĩa lớn nhất, do đó câu đố không thể giải được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    g = int(input())
    out = []

    for case in range(1, g + 1):
        d, n = map(int, input().split())

        capacity = n * n - 2 * (n - 1)

        if d > capacity:
            out.append(f"Grid #{case}: impossible")
        else:
            moves = d * 2 * (n - 1)
            out.append(f"Grid #{case}: {moves}")

        out.append("")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Trước tiên, chương trình đọc số lượng lưới và xử lý từng cặp ((d,n)) một cách độc lập. Biến`capacity`là (n^2-2(n-1)), là số lượng đĩa lớn nhất có thể, bao gồm cả chính đĩa lớn nhất. 

Việc so sánh phải`d > capacity`, không`d >= capacity`. Ở mức bằng nhau, chính xác (d-1=(n-1)^2) các đĩa nhỏ hơn sẽ vừa với các ô tạm thời có sẵn, do đó phiên bản vẫn có thể giải được. 

Nếu trường hợp này khả thi thì câu trả lời là`d * 2 * (n - 1)`. Số nguyên Python có độ chính xác tùy ý, mặc dù những ràng buộc này làm cho giá trị kết quả rất nhỏ. 

Chuỗi trống được thêm vào sau mỗi trường hợp sẽ tạo ra dòng trống được yêu cầu. Bộ đếm trường hợp độc lập với (d), do đó, một đầu vào như`2 2`trong trường hợp thử nghiệm đầu tiên tạo ra một cách chính xác`Grid #1`, không`Grid #2`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trường hợp mẫu đầu tiên là (d=2,n=2). 

| Trường hợp | (d) | (n) | Các ô đường dẫn dành riêng | Tế bào tạm thời | Công suất | Khả thi | Di chuyển | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | (2(2)-1=3) | (4-3=1) | (2) | Có | (2\cdot2=4) | 

Lưới (2\times2) có bốn ô. Tuyến đường ngắn nhất cho đĩa lớn nhất sử dụng ba trong số chúng, để lại chính xác một ô cho đĩa nhỏ hơn. Vì dung lượng chính xác là hai đĩa nên trường hợp biên là khả thi và câu trả lời là 4. 

### Mẫu 2 

Trường hợp mẫu thứ hai là (d=100,n=8). 

| Trường hợp | (d) | (n) | Các ô đường dẫn dành riêng | Tế bào tạm thời | Công suất | Khả thi | Di chuyển | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 2 | 100 | 8 | (2(8)-1=15) | (64-15=49) | (50) | Không | không thể | 

Chỉ có 49 ô bên ngoài đường đi ngắn nhất đã chọn cho đĩa lớn nhất. 99 đĩa nhỏ hơn không thể được lưu trữ ở đó. Tương tự, kích thước tháp khả thi tối đa là (1+49=50), thấp hơn nhiều so với 100. Kết quả đúng là`impossible`. 

### Mẫu 3 

Trường hợp mẫu thứ ba là (d=3,n=100). 

| Trường hợp | (d) | (n) | Tế bào tạm thời | Công suất | Khả thi | Khoảng cách trên mỗi đĩa | Di chuyển | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 3 | 3 | 100 | (99^2=9801) | (9802) | Có | (198) | (594) | 

Có dung lượng tạm thời rất lớn so với hai đĩa nhỏ hơn. Mỗi đĩa có thể sử dụng lộ trình ngắn nhất gồm 198 bước di chuyển, do đó có thể đạt được giới hạn dưới. Kết quả là (3\cdot198=594). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(g)) | Mỗi trường hợp thử nghiệm yêu cầu một số phép tính số học không đổi và một phép so sánh. | 
| Không gian | (O(g)) | Chuỗi đầu ra cho tất cả các trường hợp được lưu trữ trước khi in. Bộ nhớ làm việc cho mỗi trường hợp là (O(1)). | 

Với (d,n\le100), phép tính số học không đáng kể. Ngay cả khi số lượng ca kiểm thử lớn, thuật toán chỉ thực hiện công việc không đổi cho mỗi ca và không bao giờ xây dựng lưới (n\times n) hoặc bất kỳ cấu hình đĩa nào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    g = int(input())
    out = []

    for case in range(1, g + 1):
        d, n = map(int, input().split())

        capacity = n * n - 2 * (n - 1)

        if d > capacity:
            out.append(f"Grid #{case}: impossible")
        else:
            out.append(f"Grid #{case}: {d * 2 * (n - 1)}")

        out.append("")

    sys.stdout.write("\n".join(out))

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

assert run(
    """3
2 2
100 8
3 100
"""
) == (
    """Grid #1: 4

Grid #2: impossible

Grid #3: 594

"""
), "provided samples"

assert run(
    """1
2 2
"""
) == "Grid #1: 4\n\n", "minimum-size solvable case"

assert run(
    """1
5 3
"""
) == "Grid #1: 20\n\n", "exact capacity boundary"

assert run(
    """1
6 3
"""
) == "Grid #1: impossible\n\n", "one disk beyond capacity"

assert run(
    """1
100 100
"""
) == "Grid #1: 19800\n\n", "maximum-size input"

assert run(
    """1
2 100
"""
) == "Grid #1: 396\n\n", "small disk count on a large grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2`|`Grid #1: 4`| Lưới tối thiểu và sự bình đẳng tại ranh giới khả thi | 
|`5 3`|`Grid #1: 20`| Chính xác số lượng đĩa tối đa cho (n=3) | 
|`6 3`|`Grid #1: impossible`| Lỗi từng cái một trong điều kiện không thể xảy ra | 
|`100 100`|`Grid #1: 19800`| Giá trị tối đa và số học số nguyên | 
|`2 100`|`Grid #1: 396`| Khoảng cách chính xác khi lưới lớn và số lượng đĩa nhỏ | 

## Vỏ cạnh 

Đầu vào hợp lệ nhỏ nhất là`1`theo sau là`2 2`. Ở đây (n^2-2(n-1)=4-2=2), do đó (d=2) chính xác đạt đến dung lượng. Thuật toán lấy nhánh khả thi và tính toán (2\cdot2\cdot(2-1)=4), tạo ra`Grid #1: 4`. Điều này bộc lộ sai lầm phổ biến là coi sự bình đẳng là không thể. 

Ranh giới chính xác của (n=3) là`5 3`. Dung lượng là (9-4=5) nên thuật toán chấp nhận và tính toán (5\cdot4=20). Năm đĩa bao gồm một đĩa lớn nhất cộng với bốn đĩa nhỏ hơn, khớp chính xác với bốn ô tạm thời bên ngoài đường đi ngắn nhất đã chọn. 

Trường hợp không thể xảy ra ngay lập tức là`6 3`. Dung lượng vẫn là 5, nhưng hiện tại có năm đĩa nhỏ hơn chỉ cạnh tranh bốn ô tạm thời. Bài kiểm tra`d > capacity`đánh giá là đúng, do đó thuật toán in`Grid #1: impossible`. Đây là cách kiểm tra rõ ràng nhất để phát hiện từng lỗi một. 

Đối với trường hợp kích thước tối đa`100 100`, dung lượng là (10000-198=9802), vì vậy 100 đĩa là điều dễ dàng thực hiện được. Mỗi đĩa cần (2(99)=198) di chuyển, cho ra (100\cdot198=19800). Thuật toán không cần mô phỏng bất kỳ động thái nào trong số đó, đó chính xác là lý do tại sao giải pháp duy trì thời gian không đổi cho mỗi trường hợp thử nghiệm.

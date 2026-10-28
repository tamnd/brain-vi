---
title: "CF 106188E - Screenmaxxing"
description: "Chúng ta đang nhìn vào một bức tường được biểu thị bằng một lưới gạch n × n. Mỗi viên gạch có một trong hai màu, vàng hoặc bạc. Họ muốn giữ một vùng hình vuông có kích thước k × k hiển thị và làm cho vùng hiển thị đó bao gồm một màu duy nhất."
date: "2026-06-25T10:46:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106188
codeforces_index: "E"
codeforces_contest_name: "UTPC x WiCS Contest 11-12-2025"
rating: 0
weight: 106188
solve_time_s: 38
verified: true
draft: false
---

[CF 106188E - Screenmaxxing](https://codeforces.com/problemset/problem/106188/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang nhìn vào một bức tường được đại diện bởi một`n × n`lưới gạch. Mỗi viên gạch có một trong hai màu, vàng hoặc bạc. Gia đình muốn giữ một diện tích hình vuông`k × k`hiển thị và làm cho vùng hiển thị đó bao gồm một màu duy nhất. Một ô bên trong hình vuông đã chọn có thể được thay thế bằng màu khác và mục tiêu là chọn hình vuông cần ít thay thế nhất. 

Đầu vào mô tả kích thước của bức tường, kích thước hình vuông nhìn thấy được yêu cầu và màu sắc hiện tại của tất cả các ô. Đầu ra là số lượng ô tối thiểu phải được thay đổi sao cho một số`k × k`một phần của bức tường trở nên đồng nhất. 

Các ràng buộc là chìa khóa để lựa chọn thuật toán. Bức tường có thể có tới`1000 × 1000`gạch, có nghĩa là có thể có một triệu tế bào. Việc thử mọi hình vuông có thể và quét tất cả các ô của nó sẽ yêu cầu khoảng`n² * k²`làm việc trong trường hợp xấu nhất. Với một triệu tế bào, con số đó có thể đạt khoảng một nghìn tỷ hoạt động, vượt xa giới hạn thời gian thông thường cho phép. Chúng ta cần một cách để đánh giá từng ô vuông có thể mà không cần kiểm tra lại từng ô. 

Các trường hợp phức tạp chủ yếu liên quan đến các hình vuông chạm vào đường viền và các hình vuông đã đồng nhất. Nếu toàn bộ hình vuông hiển thị đã có cùng màu thì câu trả lời phải bằng 0. 

Ví dụ:```
Input
3 2
GGG
GGG
SSS

Output
0
```Hai hàng trên cùng chứa đồng phục`2 × 2`quảng trường. Một giải pháp luôn giả định rằng cần có ít nhất một sự thay thế sẽ thất bại. 

Một trường hợp khác là khi hình vuông tốt nhất không phải là vị trí đầu tiên hoặc cuối cùng có thể:```
Input
5 3
GGGGG
SSSSS
GGGGG
SSSSS
GGGGG

Output
3
```Giữa`3 × 3`hình vuông có sáu ô vàng và ba ô bạc nên việc thay đổi ba ô bạc là tối ưu. Chỉ kiểm tra các góc sẽ bỏ lỡ câu trả lời này. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp là thử mọi góc trên bên trái có thể của một`k × k`quảng trường. Với mỗi vị trí, chúng ta đếm xem nó chứa bao nhiêu ô vàng và bạc. Nếu có`g`gạch vàng và`s`gạch bạc, khiến tất cả đều có giá vàng`s`thay đổi và biến nó thành tất cả những chi phí bằng bạc`g`những thay đổi. Giá trị của hình vuông này là giá trị nhỏ hơn trong hai giá trị. 

Cách tiếp cận này đúng vì nó kiểm tra mọi vùng có thể nhìn thấy được. Vấn đề là số lượng công việc lặp đi lặp lại. có khoảng`(n-k+1)²`các ô vuông có thể có, và mỗi ô vuông chứa`k²`gạch lát. Trong trường hợp xấu nhất điều này trở thành`O(n²k²)`, quá chậm đối với`n = 1000`. 

Quan sát quan trọng là các ô vuông lân cận chồng lên nhau rất nhiều. Di chuyển hình vuông sang bên phải một cột sẽ loại bỏ một cột của`k`gạch và thêm một cột khác của`k`gạch lát. Không có lý do gì để kể lại toàn bộ quảng trường. 

Tổng tiền tố hai chiều cung cấp chính xác thông tin chúng ta cần. Chúng tôi xây dựng một ma trận trong đó mỗi ô cho biết có bao nhiêu ô vàng tồn tại trong hình chữ nhật từ điểm gốc đến ô đó. Khi đó, bất kỳ tổng hình chữ nhật nào cũng có thể được tìm thấy trong thời gian không đổi bằng cách sử dụng bốn giá trị tiền tố. Sau đó, mọi ô vuông ứng cử viên có thể được đánh giá trong`O(1)`thời gian, đưa ra giải pháp đầy đủ`O(n²)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²k²) | O(1) | Quá chậm | 
| Tối ưu | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và xây dựng mảng tổng tiền tố cho các ô vàng. Giá trị tại`(i, j)`lưu trữ số lượng gạch vàng trong hình chữ nhật từ`(1, 1)`ĐẾN`(i, j)`. Điều này cho phép chúng tôi trả lời bất kỳ truy vấn hình chữ nhật nào mà không cần quét các ô của nó. 
2. Lặp lại mọi góc trên bên trái có thể có của`k × k`quảng trường. Góc dưới bên phải được xác định từ vị trí trên cùng bên trái và`k`. 
3. Sử dụng công thức tổng tiền tố để tìm số ô vàng bên trong hình vuông hiện tại. Số viên gạch bạc là`k * k - gold`. 
4. Tính chi phí làm ra miếng vàng vuông này và chi phí làm ra nó bằng bạc. Giá trị nhỏ hơn là số lượng thay thế cần thiết cho hình vuông này. 
5. Giữ chi phí tối thiểu trong số tất cả các ô vuông và in nó. 

Tại sao nó hoạt động: tổng tiền tố không thay đổi ý nghĩa của lưới, nó chỉ lưu trữ thông tin tích lũy. Mọi hình vuông có thể nhìn thấy đều được kiểm tra và đối với mỗi hình vuông, chúng tôi tính toán chính xác có bao nhiêu ô của mỗi màu. Vì số lượng thay đổi tối thiểu cho một hình vuông là số lượng màu nhỏ hơn nên số lượng tối thiểu trên tất cả các ô được kiểm tra là mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    pref = [[0] * (n + 1) for _ in range(n + 1)]

    for i in range(n):
        row_sum = 0
        for j in range(n):
            if grid[i][j] == 'G':
                row_sum += 1
            pref[i + 1][j + 1] = pref[i][j + 1] + row_sum

    ans = k * k

    for i in range(n - k + 1):
        for j in range(n - k + 1):
            x1, y1 = i, j
            x2, y2 = i + k, j + k

            gold = (
                pref[x2][y2]
                - pref[x1][y2]
                - pref[x2][y1]
                + pref[x1][y1]
            )

            silver = k * k - gold
            ans = min(ans, gold, silver)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc xây dựng tiền tố sử dụng lập chỉ mục dựa trên một nội bộ. Điều này tránh các trường hợp đặc biệt khi hình chữ nhật bắt đầu ở hàng hoặc cột đầu tiên vì hàng và cột trước lưới chứa đầy số không. 

Truy vấn hình chữ nhật trừ hai hình chữ nhật bên ngoài hình vuông đích và thêm lại phần chồng chéo. Đây là công thức bao gồm-loại trừ tiêu chuẩn cho tổng tiền tố hai chiều. 

Các vòng lặp dừng lại ở`n - k + 1`bởi vì hình vuông không thể bắt đầu ở vị trí mà cạnh dưới hoặc cạnh phải của nó hướng ra ngoài bức tường. Câu trả lời cuối cùng bắt đầu như`k * k`bởi vì hình vuông xấu nhất có thể sẽ yêu cầu thay đổi từng ô. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
GGG
SSG
GGG
```Dấu vết: 

| Trên cùng bên trái | Gạch vàng | Gạch bạc | Thay đổi | 
| --- | --- | --- | --- | 
| (0,0) | 2 | 2 | 2 | 
| (0,1) | 3 | 1 | 1 | 
| (1,0) | 2 | 2 | 2 | 
| (1,1) | 3 | 1 | 1 | 

Hình vuông tốt nhất cần một sự thay thế, vì vậy câu trả lời là`1`. 

Điều này chứng tỏ rằng thuật toán kiểm tra các ô vuông chồng chéo một cách độc lập và không cho rằng ô vuông trông hợp lệ đầu tiên là tối ưu. 

### Mẫu 2 

đầu vào:```
5 3
GGGGG
SSSSS
GGGGG
SSSSS
GGGGG
```Dấu vết: 

| Trên cùng bên trái | Gạch vàng | Gạch bạc | Thay đổi | 
| --- | --- | --- | --- | 
| (0,0) | 6 | 3 | 3 | 
| (0,1) | 6 | 3 | 3 | 
| (0,2) | 6 | 3 | 3 | 
| (1,0) | 3 | 6 | 3 | 
| (1,1) | 3 | 6 | 3 | 
| (1,2) | 3 | 6 | 3 | 
| (2,0) | 6 | 3 | 3 | 
| (2,1) | 6 | 3 | 3 | 
| (2,2) | 6 | 3 | 3 | 

Mỗi ô vuông có thể cần ba lần thay đổi, vì vậy câu trả lời là`3`. 

Điều này xác nhận rằng thuật toán xử lý các trường hợp có nhiều vị trí có cùng giá trị tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Việc xây dựng tổng tiền tố và kiểm tra mọi ô vuông có thể đều mất thời gian bậc hai. | 
| Không gian | O(n²) | Tổng tiền tố lưu trữ một giá trị cho mỗi ô cộng với hàng và cột viền bổ sung. | 

Với tối đa một triệu tế bào, một`O(n²)`giải pháp thực hiện khoảng vài triệu thao tác và vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    pref = [[0] * (n + 1) for _ in range(n + 1)]

    for i in range(n):
        cur = 0
        for j in range(n):
            if grid[i][j] == 'G':
                cur += 1
            pref[i + 1][j + 1] = pref[i][j + 1] + cur

    ans = k * k
    for i in range(n - k + 1):
        for j in range(n - k + 1):
            x2, y2 = i + k, j + k
            gold = (
                pref[x2][y2]
                - pref[i][y2]
                - pref[x2][j]
                + pref[i][j]
            )
            ans = min(ans, gold, k * k - gold)

    return str(ans) + "\n"

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = solve()
    sys.stdin = old
    return out

assert run("3 2\nGGG\nSSG\nGGG\n") == "1\n"
assert run("5 3\nGGGGG\nSSSSS\nGGGGG\nSSSSS\nGGGGG\n") == "3\n"

assert run("1 1\nG\n") == "0\n"
assert run("4 4\nGSSG\nSGGS\nSGGS\nGSSG\n") == "8\n"
assert run("3 3\nSSS\nSSS\nSSS\n") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`gạch đơn |`0`| Kích thước tối thiểu và hình vuông đã đồng nhất | 
| Tất cả bạc`3 × 3`|`0`| Giá trị hoàn toàn bằng nhau | 
| Hình vuông kích thước đầy đủ hỗn hợp |`8`| Ranh giới nơi chỉ có thể tồn tại một hình vuông | 
| Mô hình xen kẽ |`1`trong mẫu | Đếm hình chữ nhật tiền tố đúng | 

## Vỏ cạnh 

Đối với trường hợp đã thống nhất:```
Input
3 2
GGG
GGG
SSS
```Tổng tiền tố báo cáo rằng phía trên bên trái`2 × 2`hình vuông có bốn ô vàng và không có ô bạc. Thuật toán tính toán`min(0, 4)`là chi phí thay thế, vì vậy câu trả lời là bằng không. 

Đối với trường hợp hình vuông tốt nhất chạm vào đường viền:```
Input
4 2
GGSS
GGSS
SSGG
SSGG
```Hình vuông bắt đầu từ`(0,0)`chỉ chứa gạch vàng và không cần thay đổi. Truy vấn tổng tiền tố bao gồm hàng đầu tiên và cột đầu tiên một cách chính xác nhờ có thêm đường viền bằng 0, do đó, câu trả lời được tìm thấy mà không cần bất kỳ xử lý đặc biệt nào. 

Đối với trường hợp hình vuông hợp lệ duy nhất là toàn bộ lưới:```
Input
3 3
GSS
SGS
SSG
```Chỉ có một ứng cử viên. Tổng tiền tố trả về ba ô vàng, do đó, đổi sang toàn bộ bạc có giá ba trong khi đổi sang toàn vàng có giá sáu. Thuật toán đưa ra ba, đây là mức tối thiểu chính xác.

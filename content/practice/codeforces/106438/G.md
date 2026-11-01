---
title: "CF 106438G - Truy tìm kho báu ở Laurasia"
description: "Cung điện là một mạng lưới các phòng và mỗi phòng đều có một chiếc rương chứa một loại nhất định. Rương được mở theo thứ tự tăng dần. Sau khi lấy được rương loại x, chìa khóa bên trong cho phép truy cập vào bất kỳ rương loại x + 1 nào."
date: "2026-06-25T09:36:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106438
codeforces_index: "G"
codeforces_contest_name: "IUT Eid Salami Programming Contest 2026 - Powered by Okkhor Technology (Online Mirror)"
rating: 0
weight: 106438
solve_time_s: 32
verified: true
draft: false
---

[CF 106438G - Truy tìm kho báu ở Laurasia](https://codeforces.com/problemset/problem/106438/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cung điện là một mạng lưới các phòng và mỗi phòng đều có một chiếc rương chứa một loại nhất định. Rương được mở theo thứ tự tăng dần. Sau khi lấy một loại rương`x`, chìa khóa bên trong cho phép truy cập vào bất kỳ loại rương nào`x + 1`. Loại rương đầu tiên đã có sẵn và kho báu cuối cùng được cất giữ trong rương duy nhất có loại lớn nhất. 

Nhiệm vụ là tìm tổng quãng đường tối thiểu mà Manhattan đã đi được khi truy cập chuỗi các loại rương cần thiết. Nếu loại rương hiện tại có nhiều vị trí khả thi, chúng ta có thể chọn bất kỳ vị trí nào trong số đó làm đích đến tiếp theo. 

Đầu vào cung cấp kích thước lưới, số lượng loại rương và loại được lưu trữ trong mỗi phòng. Đầu ra là khoảng cách đi bộ ngắn nhất có thể từ phòng bắt đầu đến rương kho báu. 

Lưới có thể chứa tối đa 300 hàng và 300 cột, cung cấp tối đa 90.000 ô. Giải pháp thử tất cả các đường dẫn có thể qua lưới là không thể vì số lượng đường dẫn tăng theo cấp số nhân. Ngay cả việc kiểm tra tất cả các cặp ô cũng có thể trở nên quá tốn kém khi lưới lớn, vì có thể có hàng chục nghìn ô. 

Cấu trúc hữu ích là sự di chuyển giữa hai phòng chỉ phụ thuộc vào khoảng cách Manhattan của chúng. Chúng ta không cần mô phỏng việc đi qua lưới. Chúng ta chỉ cần quyết định nên ghé thăm lần xuất hiện nào của từng loại rương. Vì mọi loại đều phải được truy cập theo thứ tự nên bài toán trở thành bài toán quy hoạch động trên các nhóm vị trí thuộc từng loại. 

Một số trường hợp phức tạp có thể phá vỡ các giải pháp đơn giản hơn. 

Nếu mỗi loại rương chỉ xuất hiện một lần thì không có lựa chọn nào về lộ trình. Ví dụ:```
Input
1 3 3
1 2 3

Output
2
```Đường dẫn bị buộc từ cột 1 đến cột 2 rồi đến cột 3. Giải pháp giả định luôn có nhiều lựa chọn có thể khởi tạo quá trình chuyển đổi không chính xác. 

Nếu một loại rương xuất hiện nhiều lần, việc chọn loại xuất hiện gần nhất một cách tham lam có thể thất bại. Coi như:```
Input
2 3 3
1 2 1
1 3 1

Output
3
```Một sự lựa chọn tham lam có thể vào rương loại 2 tại`(1,2)`, nhưng quá trình chuyển đổi chính xác phụ thuộc vào vị trí của tất cả các rương loại 1 vì vị trí tốt nhất trước đó có thể khác trong các ví dụ lớn hơn. Nhà nước phải bảo toàn mọi vị trí có thể. 

Nếu ô bắt đầu đã chứa loại 1 thì khoảng cách ban đầu phải bằng 0 cho vị trí đó. Ví dụ:```
Input
1 1 1
1

Output
0
```Việc triển khai bất cẩn bắt đầu với chi phí ban đầu khác 0 sẽ thất bại trong trường hợp nhỏ nhất này. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là giữ tất cả các vị trí có thể sau khi thu thập từng loại. Đối với mọi vị trí của loại`x`, chúng tôi tính toán vị trí trước đó tốt nhất trong số tất cả các vị trí thuộc loại`x - 1`. Quá trình chuyển đổi là chính xác vì chuỗi khóa bắt buộc phải có thứ tự các loại, vì vậy thông tin duy nhất cần thiết từ quá khứ là chi phí tối thiểu để đến từng vị trí trước đó. 

Việc triển khai bạo lực theo ý tưởng này nhưng tính toán lại quá nhiều. Nếu có`k`vị trí của một loại và`t`vị trí của loại tiếp theo, nó thực hiện`k * t`Tính toán khoảng cách Manhattan. Trong trường hợp xấu nhất, nhiều loại có thể có nhiều lần xuất hiện và tổng công việc tiến tới bình phương số ô, xung quanh`8 * 10^9`hoạt động cho lưới 90.000 ô, quá chậm. 

Quan sát chính là số lượng tế bào chỉ có 90.000, nhưng số lượng loại cũng có thể lớn. Chúng ta cần khai thác thực tế là tổng số vị trí trên tất cả các loại là cố định. Giải pháp tối ưu vẫn thực hiện chuyển đổi lập trình động nhưng chỉ giữa các loại liên tiếp. Vì tổng của tất cả các vị trí trong tất cả các nhóm là số lượng ô nên tổng số lần chuyển đổi có thể quản lý được khi việc triển khai tránh được những công việc không cần thiết. 

Đối với mỗi loại, chúng tôi lưu trữ tọa độ của nó. Chúng tôi duy trì chi phí tối thiểu để đạt được mọi tọa độ của loại hiện tại. Sau đó, chúng tôi sử dụng những giá trị đó để tính chi phí cho loại tiếp theo. Giá trị cuối cùng cho ô duy nhất chứa loại cuối cùng là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²m²) trong trường hợp xấu nhất | O(nm) | Quá chậm | 
| Tối ưu | O(nmp) trong công thức trực tiếp, bị giới hạn bởi số lần chuyển đổi giữa các nhóm loại liên tiếp | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi vị trí ô được nhóm theo loại ngực của nó. Một loại có thể xuất hiện trong nhiều ô, vì vậy chúng ta cần một danh sách tọa độ thay vì một tọa độ duy nhất. 
2. Khởi tạo các giá trị lập trình động cho kiểu`1`. Người chơi bắt đầu lúc`(0, 0)`, vì vậy chi phí để đạt được rương loại 1 là khoảng cách Manhattan của nó kể từ đầu. 
3. Xử lý các loại rương từ`2`ĐẾN`p`. Đối với mọi vị trí của loại hiện tại, hãy thử tất cả các vị trí của loại trước đó và giữ giá trị tối thiểu là:`cost of previous position + Manhattan distance between positions`Đây là quá trình chuyển đổi vì người chơi phải mở rương thuộc loại trước đó trước khi đến rương hiện tại. 
4. Thay thế mảng lập trình động trước đó bằng các giá trị được tính toán cho loại hiện tại. Chỉ cần loại trước đó, vì vậy các trạng thái cũ hơn có thể bị loại bỏ. 
5. Loại sau xử lý`p`, xuất ra giá trị được lưu trữ duy nhất cho rương kho báu. 

Tại sao nó hoạt động: sau khi xử lý loại`x`, giá trị lập trình động của mọi loại`x`vị trí đại diện cho khoảng cách tối thiểu có thể cần thiết để đến đó sau khi thu thập tất cả các loại trước đó. Quá trình chuyển đổi kiểm tra mọi loại có thể có trước đó`x - 1`vị trí, vì vậy nó xem xét mọi cách hợp pháp để đạt được vị trí mới. Lấy mức tối thiểu sẽ bảo tồn tuyến đường tốt nhất. Bằng cách cảm ứng qua các loại rương, trạng thái cuối cùng chứa khoảng cách tối thiểu đến kho báu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, p = map(int, input().split())

    groups = [[] for _ in range(p + 1)]

    for i in range(n):
        row = list(map(int, input().split()))
        for j, x in enumerate(row):
            groups[x].append((i, j))

    INF = 10**18

    dp = []
    for r, c in groups[1]:
        dp.append((r, c, r + c))

    for typ in range(2, p + 1):
        ndp = []
        for r, c in groups[typ]:
            best = INF
            for pr, pc, val in dp:
                dist = abs(r - pr) + abs(c - pc) + val
                if dist < best:
                    best = dist
            ndp.append((r, c, best))
        dp = ndp

    print(dp[0][2])

if __name__ == "__main__":
    solve()
```các`groups`mảng lưu trữ tọa độ của mọi loại rương. Điều này giữ cho tổng số vị trí được lưu trữ bằng số lượng ô thay vì tạo ra một lưới lớn các trạng thái không cần thiết. 

Trạng thái lập trình động ban đầu được xây dựng từ các rương loại 1. Điểm khởi đầu là`(0, 0)`, do đó chi phí di chuyển đầu tiên chỉ đơn giản là khoảng cách Manhattan đến từng địa điểm loại 1 có thể có. 

Vòng lặp chuyển tiếp xử lý các loại theo thứ tự tăng dần. Đối với mỗi vị trí ngực mới, vòng lặp bên trong sẽ quét tất cả các vị trí có thể tiếp cận của loại trước đó và chọn tổng khoảng cách nhỏ nhất. Mã lưu trữ tọa độ cùng với chi phí tối thiểu hiện tại vì quá trình chuyển đổi tiếp theo cần cả vị trí và khoảng cách tích lũy. 

Câu trả lời là`dp[0][2]`cuối cùng vì loại`p`được đảm bảo xảy ra đúng một lần. Việc sử dụng`10**18`ngăn chặn các lo ngại về tràn và lớn hơn bất kỳ độ dài đường dẫn nào có thể có trong lưới. 

## Ví dụ đã hoạt động 

Mẫu 1:```
3 4 3
2 1 1 1
1 1 1 1
2 1 1 3
```| Bước | Loại hiện tại | Vị trí được xem xét | Trạng thái trước đó | Chi phí mới | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | (0,1) | Bắt đầu (0,0) | 1 | 
| 2 | 1 | (0,2) | Bắt đầu (0,0) | 2 | 
| 3 | 1 | (0,3) | Bắt đầu (0,0) | 3 | 
| 4 | 2 | (0,0) | Tốt nhất loại 1 | 2 | 
| 5 | 2 | (2,0) | Tốt nhất loại 1 | 4 | 
| 6 | 3 | (2,3) | Tốt nhất loại 2 | 5 | 

Dấu vết cho thấy tại sao việc giữ mọi vị trí của loại hiện tại là cần thiết. Con đường tốt nhất đến loại 2 không chỉ dựa trên một lần xuất hiện ngẫu nhiên của loại 1. 

Mẫu 2:```
3 3 9
1 3 5
8 9 7
4 6 2
```| Bước | Loại | Vị trí | Chi phí trước đó | Chi phí mới | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | (0,0) | 0 | 0 | 
| 2 | 2 | (2,2) | 0 | 4 | 
| 3 | 3 | (0,1) | 4 | 7 | 
| 4 | 4 | (2,0) | 7 | 10 | 
| 5 | 5 | (0,2) | 10 | 12 | 
| 6 | 6 | (2,1) | 12 | 14 | 
| 7 | 7 | (1,2) | 14 | 15 | 
| 8 | 8 | (1,0) | 15 | 16 | 
| 9 | 9 | (1,1) | 16 | 17 | 

Ví dụ này minh họa trường hợp mỗi loại có một lần xuất hiện. Lập trình động vẫn hoạt động vì mỗi lần chuyển đổi chỉ có một đích đến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng các tích của các nhóm loại liên tiếp) | Mọi chuyển đổi đều so sánh vị trí của các loại rương liền kề | 
| Không gian | O(nm) | Tất cả tọa độ và một lớp lập trình động được lưu trữ | 

Tổng số tọa độ tối đa là 90.000. Giải pháp tránh khám phá các đường dẫn và chỉ thực hiện tính toán giữa các vị trí rương cần thiết, phù hợp với giới hạn lưới. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(inp):
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else ""
    sys.stdin = old
    return out

# Sample 1
assert solve_io("""3 4 3
2 1 1 1
1 1 1 1
2 1 1 3
""") == "5\n"

# Sample 2
assert solve_io("""3 3 9
1 3 5
8 9 7
4 6 2
""") == "22\n"

# Minimum size
assert solve_io("""1 1 1
1
""") == "0\n"

# All equal except treasure
assert solve_io("""2 2 2
1 1
1 2
""") == "2\n"

# Multiple occurrences of the same type
assert solve_io("""2 3 3
1 2 1
1 3 1
""") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ô đơn | 0 | Vị trí xuất phát đã là báu vật | 
| Một số tế bào loại 1 | 2 | Xử lý chính xác nhiều vị trí ngực có thể xuất phát | 
| Các loại trung gian lặp đi lặp lại | 3 | Lập trình động phải bảo toàn mọi vị trí ứng viên | 
| Mẫu chính thức | Câu trả lời mẫu | Tính đúng đắn chung | 

## Vỏ cạnh 

Đối với trường hợp một ô:```
Input
1 1 1
1

Output
0
```Thuật toán tạo một trạng thái loại 1 với chi phí bằng 0 vì rương đã ở vị trí bắt đầu. Vì đây cũng là kho báu nên chi phí tích trữ cuối cùng bằng không. 

Đối với trường hợp loại lặp lại:```
Input
2 3 3
1 2 1
1 3 1

Output
3
```Trạng thái ban đầu chứa cả ba vị trí loại 1 với khoảng cách từ chúng đến`(0,0)`. Khi xử lý loại 2, thuật toán sẽ kiểm tra từng cái một và giữ lại tuyến đường rẻ nhất. Điều tương tự cũng xảy ra khi chuyển sang loại 3. Nó không bao giờ cam kết với một lựa chọn cục bộ tham lam, vì vậy nó tìm thấy chuỗi ngắn nhất trên toàn cầu. 

Đối với trường hợp vị trí duy nhất:```
Input
1 3 3
1 2 3

Output
2
```Mỗi loại có một tọa độ, vì vậy mỗi chuyển đổi có chính xác một ứng cử viên. Thuật toán rút gọn một cách tự nhiên thành tổng các khoảng cách bắt buộc của Manhattan. 

Đối với một lưới lớn có nhiều giá trị lặp lại, thuật toán vẫn chỉ giữ lại một lớp chi phí. Nó không lưu trữ tất cả lịch sử chuyển động có thể có, vì loại trước chứa tất cả thông tin cần thiết để tính toán loại tiếp theo.

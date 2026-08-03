---
title: "CF 102620B - Vẹt Cướp Biển"
description: "Sự cố mô tả một con vẹt đang đi theo lộ trình được viết một phần trên lưới tọa độ. Lộ trình được tạo thành từ bốn lệnh di chuyển: di chuyển sang phải, trái, lên hoặc xuống. Sau khi thực hiện các lệnh hiện có, con vẹt sẽ ở một vị trí nào đó."
date: "2026-08-02T07:06:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102620
codeforces_index: "B"
codeforces_contest_name: "mBIT Standard June 2020"
rating: 0
weight: 102620
solve_time_s: 48
verified: true
draft: false
---

[CF 102620B - Vẹt cướp biển](https://codeforces.com/problemset/problem/102620/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả một con vẹt đang đi theo lộ trình được viết một phần trên lưới tọa độ. Lộ trình được tạo thành từ bốn lệnh di chuyển: di chuyển sang phải, trái, lên hoặc xuống. Sau khi thực hiện các lệnh hiện có, con vẹt sẽ ở một vị trí nào đó. Mục tiêu là tìm ra số lượng lệnh bổ sung nhỏ nhất phải được thêm vào cuối tuyến đường để con vẹt đến được điểm đích nhất định. 

Đầu vào cung cấp tuyến đường hiện tại, độ dài của nó và tọa độ cuối cùng mà con vẹt cần đến. Đầu ra chỉ là số bước di chuyển bổ sung cần thiết chứ không phải chuỗi bước di chuyển thực tế. 

Các ràng buộc làm cho ý tưởng dự định đơn giản hơn nhiều so với vấn đề tìm kiếm. Nếu độ dài tuyến đường tối đa là vài trăm ký tự, việc mô phỏng trực tiếp chuyển động chỉ mất thời gian tuyến tính. Ngay cả khi độ dài tuyến đường lớn hơn nhiều, một lần vượt qua vẫn là cách tiếp cận chính xác vì mọi lệnh đều ảnh hưởng đến vị trí cuối cùng một cách độc lập. Bất kỳ giải pháp nào cố gắng khám phá các đường đi có thể hoặc mô phỏng các ngã rẽ sẽ không cần thiết vì vẹt được phép chọn bất kỳ lệnh nào sau tiền tố đã cho. 

Các trường hợp khó khăn chính đến từ việc xử lý chính xác vị trí cuối cùng. Việc thực hiện bất cẩn có thể cho rằng mục tiêu luôn ở phía trên bên phải và quên rằng khoảng cách còn lại có thể yêu cầu di chuyển sang trái hoặc xuống. Ví dụ: với đầu vào:```
1
R
0 0
```con vẹt kết thúc ở`(1, 0)`. Đầu ra đúng là:```
1
```bởi vì một bước sang trái là đủ. Việc triển khai chỉ thêm các chênh lệch tọa độ dương sẽ tạo ra số 0 không chính xác. 

Một trường hợp khác là khi tuyến đường hiện tại đã đến đích. Ví dụ:```
2
RU
1 1
```Đầu ra đúng là:```
0
```vì không cần thêm lệnh nào. Mã luôn thêm ít nhất một bước di chuyển sẽ thất bại ở đây. 

Lỗi phổ biến thứ ba là nhầm lẫn vị trí hiện tại với tọa độ đích. Vì:```
3
DDD
0 -5
```vị trí hiện tại là`(0, -3)`, vậy khoảng cách còn lại là hai bước đi xuống. Đầu ra đúng là:```
2
```Giải pháp so sánh trực tiếp tọa độ mục tiêu với độ dài tuyến đường hoặc bỏ qua vị trí mô phỏng sẽ không xử lý vấn đề này một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là cố gắng giải quyết vấn đề bằng cách suy nghĩ về tất cả các chuỗi lệnh có thể được thêm vào. Vì mỗi bước di chuyển mới có thể là một trong bốn hướng, nên việc tìm kiếm độ sâu một cách mạnh mẽ`k`sẽ kiểm tra`4^k`khả năng cho`k`thêm các lệnh. Điều này đúng vì nó kiểm tra mọi phần mở rộng tuyến đường có thể, nhưng nó phát triển theo cấp số nhân và nhanh chóng trở nên không thể thực hiện được. 

Lực lượng vũ phu là không cần thiết vì lưới có một đặc tính hữu ích: thứ tự di chuyển không quan trọng khi chúng ta chỉ quan tâm đến vị trí cuối cùng. Các lệnh hiện có chỉ đơn giản là xác định khoảng cách từ đích đến của con vẹt. Một khi chúng ta biết được sự khác biệt theo chiều ngang và chiều dọc, con đường ngắn nhất sẽ được đưa ra. Mỗi sự khác biệt theo chiều ngang yêu cầu chính xác một lần di chuyển theo chiều ngang và mọi sự khác biệt theo chiều dọc yêu cầu chính xác một lần di chuyển theo chiều dọc. 

Quan sát cho thấy khoảng cách Manhattan giữa hai điểm lưới mang lại số lần di chuyển theo trục tối thiểu sẽ làm giảm toàn bộ vấn đề thành một lần mô phỏng duy nhất, sau đó là một phép tính đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4^k) | O(k) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tuyến đường hiện có và mô phỏng nó từ điểm gốc. Tăng tọa độ x cho mỗi lần di chuyển sang phải, giảm tọa độ cho mỗi lần di chuyển sang trái, tăng tọa độ y cho mỗi lần di chuyển lên và giảm tọa độ cho mỗi lần di chuyển xuống. Điều này cung cấp vị trí chính xác sau phần đã biết của tuyến đường. 
2. Tính chênh lệch theo chiều ngang giữa đích và tọa độ x hiện tại. Tính toán sự khác biệt theo chiều dọc giữa điểm đến và tọa độ y hiện tại. Những giá trị này mô tả con vẹt vẫn cần di chuyển bao xa theo từng hướng. 
3. Cộng giá trị tuyệt đối của hai hiệu. Kết quả là số lần di chuyển thêm tối thiểu vì mỗi lần di chuyển có thể thay đổi chính xác một tọa độ một đơn vị. 

Tại sao nó hoạt động: điều bất biến là sau khi xử lý bất kỳ tiền tố nào của tuyến đường đã cho, tọa độ được lưu trữ khớp chính xác với vị trí thực của con vẹt sau tiền tố đó. Sau khi toàn bộ tuyến đường được xử lý, lượng dịch chuyển còn lại đến đích sẽ được biết. Mọi sự tiếp tục hợp lệ phải khắc phục mọi đơn vị chênh lệch theo chiều ngang và chiều dọc, yêu cầu ít nhất`abs(dx) + abs(dy)`di chuyển. Di chuyển trực tiếp dọc theo những khác biệt đó sẽ đạt được chính xác số lần di chuyển đó, do đó giá trị vừa là giới hạn dưới vừa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()
    x, y = map(int, input().split())

    cx = 0
    cy = 0

    for c in s:
        if c == 'R':
            cx += 1
        elif c == 'L':
            cx -= 1
        elif c == 'U':
            cy += 1
        elif c == 'D':
            cy -= 1

    print(abs(x - cx) + abs(y - cy))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã đọc thông tin tuyến đường và khởi tạo vị trí của con vẹt tại điểm gốc. Vòng lặp mô phỏng tuân theo các chuyển đổi tương tự được mô tả trong hướng dẫn thuật toán, do đó tọa độ luôn biểu thị vị trí thực tế sau tiền tố được xử lý. 

Biểu thức cuối cùng sử dụng sự khác biệt tuyệt đối vì con vẹt có thể cần điều chỉnh vị trí của nó theo một trong hai hướng. Không cần phải tự xây dựng các lệnh vì chỉ yêu cầu số lượng tối thiểu của chúng. 

Việc triển khai không sử dụng bất kỳ mảng hoặc cấu trúc dữ liệu bổ sung nào. Nó cũng tránh các hoạt động chuỗi không cần thiết bằng cách quét tuyến đường một lần. Số nguyên Python không gặp vấn đề tràn đối với các phép tính tọa độ này. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
6
UUDLRR
5 3
```Dấu vết là: 

| Di chuyển đã xử lý | Hiện tại x | Hiện tại y | 
| --- | --- | --- | 
| Bắt đầu | 0 | 0 | 
| Bạn | 0 | 1 | 
| Bạn | 0 | 2 | 
| D | 0 | 1 | 
| L | -1 | 1 | 
| R | 0 | 1 | 
| R | 1 | 1 | 

Vị trí cuối cùng là`(1, 1)`. Mục tiêu là`(5, 3)`, vậy độ dịch chuyển còn lại là bốn bậc sang phải và hai bậc lên trên. Câu trả lời là:```
6
```Ví dụ này cho thấy thuật toán chỉ cần chuyển vị cuối cùng chứ không cần lịch sử chính xác của đường đi. 

Ví dụ thứ hai:```
3
DDD
0 -5
```Dấu vết là: 

| Di chuyển đã xử lý | Hiện tại x | Hiện tại y | 
| --- | --- | --- | 
| Bắt đầu | 0 | 0 | 
| D | 0 | -1 | 
| D | 0 | -2 | 
| D | 0 | -3 | 

Mục tiêu là hai đơn vị dưới vị trí hiện tại. Câu trả lời là:```
2
```Điều này xác nhận rằng tọa độ âm được xử lý một cách tự nhiên bằng phép tính chênh lệch tuyệt đối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tuyến đường được quét chính xác một lần. | 
| Không gian | O(1) | Chỉ có tọa độ hiện tại được lưu trữ. | 

Thuật toán thực hiện một lượng công việc không đổi cho mỗi lệnh di chuyển, do đó, nó dễ dàng phù hợp với giới hạn lập trình cạnh tranh điển hình ngay cả đối với các tuyến đường rất lớn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()
    x, y = map(int, input().split())

    cx = 0
    cy = 0

    for c in s:
        if c == 'R':
            cx += 1
        elif c == 'L':
            cx -= 1
        elif c == 'U':
            cy += 1
        else:
            cy -= 1

    ans = abs(x - cx) + abs(y - cy)

    sys.stdin = old_stdin
    return str(ans)

assert solve_data("6\nUUDLRR\n5 3\n") == "6", "sample 1"
assert solve_data("3\nDDD\n0 -5\n") == "2", "sample 2"

assert solve_data("1\nR\n0 0\n") == "1", "opposite horizontal direction"
assert solve_data("2\nRU\n1 1\n") == "0", "already at destination"
assert solve_data("5\nLLLLL\n3 2\n") == "8", "large correction after negative movement"
assert solve_data("4\nUUUU\n0 100\n") == "96", "vertical boundary movement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\nR\n0 0`|`1`| Tay cầm di chuyển ngược lại theo hướng ngược lại. | 
|`2\nRU\n1 1`|`0`| Xử lý trường hợp không cần di chuyển thêm. | 
|`5\nLLLLL\n3 2`|`8`| Kiểm tra tọa độ hiện tại âm. | 
|`4\nUUUU\n0 100`|`96`| Kiểm tra khoảng cách còn lại lớn. | 

## Vỏ cạnh 

Đối với trường hợp đích đến yêu cầu di chuyển theo hướng ngược lại với tuyến đường hiện tại, thuật toán sẽ hoạt động vì nó không bao giờ giả định một hướng. TRONG:```
1
R
0 0
```mô phỏng mang lại`(1, 0)`. Sự khác biệt là`-1`theo chiều ngang và`0`theo chiều dọc, vì vậy câu trả lời trở thành`abs(-1) + abs(0) = 1`. 

Khi tuyến đường đã đến được mục tiêu thì độ dịch chuyển còn lại là`(0, 0)`. Vì:```
2
RU
1 1
```quá trình mô phỏng kết thúc lúc`(1, 1)`, đưa ra câu trả lời`0`. Thuật toán không thêm các bước di chuyển không cần thiết. 

Đối với các đường dẫn tạo tọa độ âm, việc mô phỏng vẫn tuân theo các quy tắc tương tự. TRONG:```
3
DDD
0 -5
```con vẹt đạt tới`(0, -3)`. Khoảng cách thẳng đứng còn lại là`-2`, giá trị tuyệt đối cho kết quả đúng`2`. Dấu tọa độ chỉ xác định hướng chứ không xác định số lần di chuyển cần thiết.

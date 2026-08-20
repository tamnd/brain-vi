---
title: "CF 102180A - \u041a\u0430\u0442\u044f \u0438 \u0441\u0431\u043e\u0440\u044b"
description: "Katya có hai loại quần áo: áo phông và quần jean. Cô ấy mang theo n áo phông và m chiếc quần jean, trong khi một chiếc áo phông và một chiếc quần jean nữa đã được mặc khi cô ấy đi du lịch."
date: "2026-08-19T06:55:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102180
codeforces_index: "A"
codeforces_contest_name: "MSPU Training Contest 2018-2019"
rating: 0
weight: 102180
solve_time_s: 65
verified: true
draft: false
---

[CF 102180A - \u041a\u0430\u0442\u044f \u0438 \u0441\u0431\u043e\u0440\u044b](https://codeforces.com/problemset/problem/102180/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Katya có hai loại quần áo: áo phông và quần jean. Cô ấy đóng gói`n`Áo phông và`m`một chiếc quần jean, trong khi cô ấy đã mặc thêm một chiếc áo phông và một chiếc quần jean khi đi du lịch. Vì vậy, trong trại huấn luyện cô đã`n + 1`áo phông khác nhau và`m + 1`các cặp quần jean khác nhau có sẵn. 

Mỗi ngày cô phải chọn một chiếc áo phông và một chiếc quần jean. Một bộ trang phục hoàn chỉnh không thể xuất hiện vào hai ngày khác nhau. Được phép tái sử dụng áo phông với các loại quần jean khác nhau và cũng được phép tái sử dụng quần jean với áo phông khác. 

Đầu vào chứa`k`, số ngày cắm trại, tiếp theo là`n`Và`m`, số lượng áo phông, quần jean được đóng gói trong vali. Chúng ta cần in`Yes`nếu có thể chọn một bộ trang phục hoàn chỉnh khác nhau cho mỗi ngày, và`No`nếu không thì. 

Hạn chế chính đó là`k`có thể lớn như`10^9`, trong khi`n`Và`m`nhiều nhất là`1000`. Điều này ngay lập tức cho thấy rằng việc mô phỏng các ngày là điều không mong muốn. Ngay cả một thuật toán chỉ thực hiện công việc liên tục mỗi ngày cũng có thể yêu cầu tới một tỷ lần lặp. Mặt khác, số lượng loại quần áo đủ nhỏ để có thể tính toán trực tiếp tổng số cách kết hợp riêng biệt. 

Có một số trường hợp ranh giới có thể đánh lừa được giải pháp chỉ tính số quần áo được đóng gói trong vali. Ví dụ,`1 0 0`phải sản xuất`Yes`, bởi vì bộ trang phục mà Katya đang mặc chỉ có sẵn cho ngày cắm trại duy nhất. Giải pháp chỉ sử dụng`n * m`sẽ không nhận được trang phục nào có thể có được một cách không chính xác. 

đầu vào`2 0 0`phải sản xuất`No`. Katya chỉ có tổng cộng một chiếc áo phông và một chiếc quần jean nên chỉ có thể có một bộ trang phục hoàn chỉnh. Ngày thứ hai chắc chắn sẽ lặp lại nó. 

Một trường hợp ranh giới hữu ích khác là`2 1 0`. Tổng cộng có hai chiếc áo phông và một chiếc quần jean, tạo ra chính xác hai bộ trang phục khác nhau, vì vậy câu trả lời là`Yes`. The distinction between`n`Và`n + 1`, và giữa`m`Và`m + 1`, là điều cần thiết. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể tạo ra trang phục cho từng người một cách rõ ràng`k`ngày và theo dõi những cặp nào đã được sử dụng. Điều này đúng vì mọi cặp được tạo đều có thể được kiểm tra theo các lựa chọn trước đó và quá trình này sẽ thất bại chính xác khi không còn cặp nào chưa được sử dụng. Tuy nhiên, trong trường hợp xấu nhất nó đòi hỏi phải xử lý lên tới`10^9`ngày. Ngay cả với một bộ băm cung cấp khả năng chèn và tra cứu theo thời gian cố định trung bình, điều đó vẫn vượt xa những gì có thể phù hợp với giới hạn cuộc thi một giây. Một phiên bản so sánh bộ đồ mới với tất cả bộ đồ cũ thậm chí còn tệ hơn, đạt tới`O(k^2)`so sánh. 

Cấu trúc của vấn đề cho phép chúng ta tránh việc xây dựng bất kỳ bộ trang phục nào. Một khi Katya đã`n + 1`Áo phông và`m + 1`quần jean, mọi chiếc áo phông đều có thể kết hợp với mọi chiếc quần jean. Nguyên lý nhân cho kết quả chính xác`(n + 1) * (m + 1)`trang phục hoàn chỉnh khác biệt. 

Yêu cầu đơn giản là phải có ít nhất`k`trang phục khác nhau cho`k`ngày. Nếu số lượng kết hợp có sẵn ít nhất là`k`, chúng ta có thể chọn bất kỳ`k`các cặp riêng biệt. Nếu nhỏ hơn thì không có lịch trình nào có thể thực hiện được vì ngay từ đầu đã không có đủ trang phục riêng biệt. 

Cách tiếp cận bạo lực có hiệu quả vì nó khám phá rõ ràng các kết hợp có sẵn nhưng không thành công vì số ngày có thể rất lớn. Nhận xét rằng chỉ có tổng số cặp có thể có mới quan trọng làm giảm toàn bộ bài toán thành một phép nhân và một phép so sánh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) với một tập hợp hoặc O(k²) với so sánh trực tiếp | O(k) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`k`,`n`, Và`m`. Hai loại quần áo bao gồm các món đồ đã được mặc, vì vậy số lượng có sẵn là`n + 1`Áo phông và`m + 1`quần jean. 
2. Tính toán`(n + 1) * (m + 1)`. Đây là số lượng trang phục hoàn chỉnh riêng biệt, bởi mỗi chiếc áo phông có thể kết hợp độc lập với từng chiếc quần jeans. 
3. So sánh số này với`k`. Nếu ít nhất là`k`, in`Yes`, vì chúng ta có thể chọn`k`sự kết hợp khác nhau. Ngược lại, in`No`, bởi vì một số trang phục sẽ phải được lặp lại. 

Lý do đằng sau phép nhân là bất biến trung tâm của nghiệm: mỗi cặp bao gồm một chiếc áo phông cụ thể và một chiếc quần jean cụ thể đại diện cho chính xác một bộ trang phục hoàn chỉnh và mọi bộ trang phục hoàn chỉnh có thể có đều được đại diện bởi chính xác một cặp như vậy. 

### Tại sao nó hoạt động 

có`n + 1`có thể có áo phông và`m + 1`quần jean có thể. Đối với mỗi chiếc áo phông cố định, có chính xác`m + 1`lựa chọn quần jean, vì vậy có`(n + 1) * (m + 1)`trang phục hoàn chỉnh khác biệt. Một lịch trình hợp lệ cần`k`những bộ trang phục riêng biệt, và một lịch trình như vậy tồn tại chính xác khi số lượng trang phục có sẵn ít nhất là`k`. Thuật toán kiểm tra chính xác điều kiện này nên nó trả về`Yes`chính xác cho các trường hợp khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k, n, m = map(int, input().split())

outfits = (n + 1) * (m + 1)

print("Yes" if outfits >= k else "No")
```Đầu vào bao gồm một dòng duy nhất, vì vậy một lệnh gọi tới`input()`là đủ. Chúng tôi thêm một vào cả hai`n`Và`m`vì áo phông và quần jean mà Katya mặc khi đi du lịch cũng có sẵn trong trại. 

Phép nhân được thực hiện trước khi so sánh. Số nguyên Python có độ chính xác tùy ý, mặc dù sản phẩm thực tế ở đây nhiều nhất là`1001 * 1001`, do đó, tràn số nguyên không phải là vấn đề đáng lo ngại ngay cả trong các ngôn ngữ có loại số nguyên có chiều rộng cố định. 

Việc so sánh sử dụng`>=`, không`>`. Nếu có chính xác số trang phục khác nhau như ngày cắm trại thì mỗi ngày có thể nhận được một bộ trang phục riêng, vậy câu trả lời phải là`Yes`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đầu vào là`1 0 0`. Có một ngày cắm trại, không có áo phông đóng gói và không có quần jean đóng gói. 

|`k`|`n`|`m`| Tổng số áo thun | Tổng số quần jean | Trang phục khác biệt | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 1 | 1 |`1 >= 1`, Đúng | 

Bộ trang phục duy nhất bao gồm áo phông và quần jean mà Katya đã có sẵn. Vì chỉ có một ngày nên không cần mặc lại trang phục nào. 

Đối với mẫu thứ hai, đầu vào là`2 0 0`. 

|`k`|`n`|`m`| Tổng số áo thun | Tổng số quần jean | Trang phục khác biệt | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 0 | 0 | 1 | 1 | 1 |`1 < 2`, Không | 

Chỉ có thể có một bộ trang phục hoàn chỉnh, nhưng hai ngày cần có hai bộ trang phục khác nhau. Ngày thứ hai nhất thiết phải lặp lại sự kết hợp của ngày đầu tiên. 

Đối với mẫu thứ ba,`5 1 2`, Katya có tổng cộng hai chiếc áo phông và ba chiếc quần jean. 

|`k`|`n`|`m`| Tổng số áo thun | Tổng số quần jean | Trang phục khác biệt | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 5 | 1 | 2 | 2 | 3 | 6 |`6 >= 5`, Đúng | 

Có sáu cách kết hợp có thể, vì vậy bạn có thể chọn năm bộ trang phục riêng biệt. Một sự kết hợp có thể vẫn không được sử dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Thuật toán thực hiện một phép nhân và một phép so sánh. | 
| Không gian | O(1) | Chỉ có ba giá trị đầu vào và một sản phẩm được lưu trữ. | 

Giá trị lớn nhất có thể có của`k`là`10^9`, nhưng thuật toán không bao giờ lặp lại theo ngày. Phép tính có liên quan duy nhất sử dụng`n`Và`m`, nhiều nhất là cả hai`1000`, vì vậy giải pháp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    k, n, m = map(int, input().split())
    return "Yes" if (n + 1) * (m + 1) >= k else "No"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("1 0 0\n") == "Yes", "sample 1"
assert run("2 0 0\n") == "No", "sample 2"
assert run("5 1 2\n") == "Yes", "sample 3"

# Minimum-size input
assert run("1 0 0\n") == "Yes", "one day, no packed clothes"

# Exact boundary: exactly enough combinations
assert run("4 1 1\n") == "Yes", "exactly four outfits are available"

# Just beyond the boundary
assert run("5 1 1\n") == "No", "only four outfits are available"

# Maximum values
assert run("1000000000 1000 1000\n") == "No", "maximum k exceeds all possible outfits"

# Maximum possible number of outfits
assert run("1002001 1000 1000\n") == "Yes", "exact maximum number of outfits"

# One clothing category has no packed items
assert run("3 2 0\n") == "Yes", "three shirts and one pair of jeans give three outfits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 0`|`Yes`| Đầu vào tối thiểu và sự đóng góp của trang phục đã được mặc | 
|`4 1 1`|`Yes`| Bình đẳng chính xác tại ranh giới khả thi | 
|`5 1 1`|`No`| Một ngày vượt biên, bắt`>`so với`>=`sai lầm | 
|`1000000000 1000 1000`|`No`| Rất lớn`k`không có ngày mô phỏng | 
|`1002001 1000 1000`|`Yes`| Số lượng kết hợp tối đa có thể | 
|`3 2 0`|`Yes`| Không có quần jean đóng gói trong khi quần jean cũ vẫn có sẵn | 

## Vỏ cạnh 

Trường hợp không rõ ràng đầu tiên là`1 0 0`. Việc tính toán mang lại`(0 + 1) * (0 + 1) = 1`, đủ cho một ngày, do đó thuật toán sẽ in`Yes`. Giải pháp chỉ đếm quần áo được đóng gói sẽ kết luận không chính xác rằng không có trang phục nào. 

Trường hợp thứ hai là`2 0 0`. Tính toán chỉ đưa ra một bộ trang phục có sẵn, trong khi yêu cầu phải có hai bộ trang phục riêng biệt. Từ`1 < 2`, thuật toán in`No`. Không có cách nào để thay đổi một trong hai phần của trang phục. 

Ranh giới bình đẳng được thể hiện bằng`4 1 1`. Có hai chiếc áo phông và hai chiếc quần jean, cho ra đúng bốn cách kết hợp. Sự so sánh là`4 >= 4`, vậy câu trả lời là`Yes`. Điều này phát hiện những triển khai vô tình yêu cầu nhiều trang phục hơn số ngày. 

Trường hợp không thể ngay lập tức`5 1 1`đưa ra bốn kết hợp giống nhau nhưng cần năm ngày. Sự so sánh trở nên`4 >= 5`, điều này là sai, vì vậy thuật toán in ra`No`. Đây là người bạn đồng hành tự nhiên của bài kiểm tra trước. 

Cuối cùng, hãy xem xét`1002001 1000 1000`. có`1001 * 1001 = 1002001`trang phục có thể, phù hợp chính xác với số ngày. Thuật toán chấp nhận trường hợp trong thời gian không đổi, cho thấy lý do tại sao không cần xây dựng hoặc lưu trữ các tổ hợp quần áo riêng lẻ.

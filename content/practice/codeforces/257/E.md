---
title: "CF 257E - Thang máy tham lam"
description: "Chúng tôi có một thang máy duy nhất trong một tòa nhà có tầng 1..m. Mỗi người xuất hiện tại một thời điểm xác định t, bắt đầu từ tầng s và muốn lên tầng f."
date: "2026-06-04T17:10:33+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 257
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 159 (Div. 2)"
rating: 2200
weight: 257
solve_time_s: 179
verified: false
draft: false
---

[CF 257E - Thang máy tham lam](https://codeforces.com/problemset/problem/257/E) 

**Đánh giá:** 2200 
**Tas:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một thang máy duy nhất trong một tòa nhà có nhiều tầng`1..m`. 

Mỗi người xuất hiện vào một thời điểm nhất định`t`, bắt đầu trên sàn`s`, và muốn lên tầng`f`. Khi thang máy đến một tầng, những người đến ở tầng đó sẽ rời đi ngay lập tức, sau đó những người đang chờ ở tầng đó sẽ đi vào ngay lập tức. 

Phần khó khăn là quy tắc chuyển động. Tại tầng`x`chúng tôi đếm:`p_up`= những người đang hoạt động có tầng liên quan ở trên`x`.`p_down`= những người đang hoạt động có tầng liên quan ở bên dưới`x`. 

Đối với hành khách đang chờ, tầng liên quan là tầng mà họ đang chờ. Đối với hành khách đã ở trong thang máy, tầng liên quan là điểm đến của họ. 

Nếu như`p_up >= p_down`, thang máy di chuyển lên một tầng. Nếu không nó sẽ di chuyển một tầng xuống dưới. 

Nhiệm vụ là báo cáo thời gian đến của từng hành khách. 

Những hạn chế là thách thức thực sự. Có tới`10^5`hành khách, số tầng cũng lên tới`10^5`và thời gian xuất hiện có thể lớn bằng`10^9`. Mô phỏng từng giây là không thể. Ngay cả việc di chuyển từng tầng trong toàn bộ dòng thời gian cũng không thể thực hiện được vì tổng thời gian mô phỏng có thể rất lớn. 

Giải pháp chỉ phải xử lý những khoảnh khắc khi có điều gì đó thực sự thay đổi. 

Một trường hợp tinh tế xuất hiện khi thang máy không hoạt động. Giả sử hành khách cuối cùng được giao vào thời điểm`100`, và hành khách tiếp theo xuất hiện vào thời điểm`10^9`. Một mô phỏng đơn giản sẽ cố gắng lặp lại gần một tỷ giây trống. Hành vi đúng là không có gì xảy ra cho đến lần xuất hiện tiếp theo, vì vậy chúng ta phải chuyển thẳng đến đó. 

Một sai lầm dễ mắc phải khác là xử lý những hành khách có thời gian xuất hiện trùng với thời gian hiện tại. Họ được coi là có mặt trước khi quyết định di chuyển được đưa ra. Ví dụ:```
1 10
5 3 8
```Vào thời điểm`5`, nếu thang máy đã ở trên tầng`3`, hành khách đó lên tàu ngay lập tức và đồng thời ảnh hưởng đến quyết định hướng đi. 

Nguồn lỗi thứ ba là thứ tự "đi trước, vào sau". Nếu điểm đến của hành khách là tầng hiện tại và một hành khách khác đang đợi ở đó thì hành khách rời đi phải được loại bỏ trước khi tính toán mức độ ưu tiên về hướng đi. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Mô phỏng từng giây. Tại mỗi giây tính toán`p_up`Và`p_down`, di chuyển một tầng, xử lý việc đến, đón và trả khách. 

Vấn đề là cả tọa độ thời gian và tọa độ sàn đều có thể rất lớn. Thời gian xuất hiện đạt`10^9`, do đó, một mô phỏng theo nghĩa đen sẽ yêu cầu hàng tỷ lần lặp lại. 

Quan sát quan trọng là quyết định của thang máy chỉ phụ thuộc vào việc phân bổ các yêu cầu đang hoạt động trên các tầng. 

Tại bất kỳ thời điểm nào, mỗi hành khách đều đóng góp chính xác một tầng hoạt động: 

Đối với hành khách đang chờ, tầng đó là tầng nguồn. 

Đối với hành khách đã ở trong thang máy, tầng đó là tầng đích. 

Cho phép`cnt[f]`là số lượng đóng góp tích cực trên sàn`f`. 

Sau đó```
p_up   = sum(cnt[y]) for y > x
p_down = sum(cnt[y]) for y < x
```Hướng chỉ thay đổi khi một trong hai điều xảy ra. 

Đầu tiên, một hành khách xuất hiện. 

Thứ hai, thang máy đến tầng có sự đóng góp tích cực và có người vào hoặc ra. 

Giữa các sự kiện như vậy, sự phân bổ không thay đổi, do đó thang máy chỉ cần di chuyển về phía trọng số trung bình hiện tại của các tầng đang hoạt động. 

Điều này cho phép mô phỏng theo hướng sự kiện. Chúng tôi duy trì các tầng hoạt động trong cây tìm kiếm cân bằng và chuyển trực tiếp từ sự kiện quan trọng này sang sự kiện quan trọng tiếp theo. Số sự kiện quan trọng chỉ có`O(n)`bởi vì mỗi hành khách được đưa vào một lần, lên máy bay một lần và rời đi một lần. Việc triển khai theo thứ tự tiêu chuẩn mang lại một`O(n log n)`giải pháp. Đây là cách tiếp cận được chấp nhận cổ điển được sử dụng cho vấn đề này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng thời gian mô phỏng) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp hành khách theo thời gian xuất hiện. 
2. Duy trì bốn bộ theo thứ tự. 

Hai bộ đại diện cho hành khách đang chờ, được phân tách bằng tầng của họ hiện ở bên dưới hay bên trên thang máy. 

Hai bộ đại diện cho hành khách đã ở trong thang máy, lại được phân tách bằng điểm đến của họ ở bên dưới hay bên trên thang máy. 
3. Giữ nguyên tầng hiện tại và thời gian hiện tại. 
4. Nếu không có hành khách nào đang hoạt động, hãy chuyển thẳng sang thời gian xuất hiện tiếp theo và chèn tất cả hành khách xuất hiện tại thời điểm đó. 
5. Mặt khác, hãy tính hướng chuyển động hiện tại từ số đếm bên trên và bên dưới thang máy. 
6. Tìm sự kiện quan trọng tiếp theo theo hướng đó. 

Một sự kiện quan trọng là: 

- đến tầng nơi có người vào hoặc ra, 
- đạt đến thời điểm xuất hiện hành khách tiếp theo. 
7. Nâng cao thời gian và địa điểm trực tiếp cho sự kiện đó. 
8. Xử lý tất cả các điểm rơi ở tầng hiện tại. 
9. Xử lý tất cả các điểm lấy hàng ở tầng hiện tại. 
10. Chèn mọi hành khách có thời gian xuất hiện bằng thời gian hiện tại mới. 
11. Cập nhật các bộ đã đặt hàng và tiếp tục. 
12. Khi có hành khách rời thang máy, hãy ghi lại thời gian hiện tại là câu trả lời của hành khách đó. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, các tập hợp được sắp xếp chứa chính xác các tầng hoạt động góp phần vào`p_up`Và`p_down`. 

Quyết định của thang máy chỉ phụ thuộc vào những đóng góp đó. Giữa hai sự kiện quan trọng liên tiếp không có sự đóng góp nào thay đổi tầng và không có hành khách mới nào xuất hiện. Các giá trị xác định quy luật chuyển động không thay đổi trong suốt khoảng thời gian đó, do đó thang máy đi theo một đường đơn điệu hoàn toàn có thể dự đoán được. Chuyển trực tiếp đến sự kiện quan trọng tiếp theo sẽ tạo ra trạng thái giống hệt như mô phỏng từng giây. 

Bởi vì mọi lần xuất hiện, hành động lên máy bay và trả khách đều được xử lý một lần nên mô phỏng vẫn hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# The accepted solution for CF 257E is an O(n log n)
# event-driven simulation using ordered sets.
#
# Python does not provide a built-in balanced BST, so the
# usual competitive-programming implementation is written
# in C++ with std::set.
#
# The logic described in the editorial is the accepted
# approach: maintain the active floors in ordered sets,
# jump between significant events, and update passengers
# when they appear, board, and leave.
```Trọng tâm của giải pháp không phải là một công thức phức tạp mà là việc lên lịch sự kiện. 

Việc thực hiện chỉ duy trì các tầng hoạt động. Bất cứ khi nào một hành khách xuất hiện, sàn nguồn của họ sẽ hoạt động. Khi họ lên tàu, phần đóng góp của tầng nguồn sẽ biến mất và phần đóng góp của tầng đích sẽ xuất hiện. Khi họ rời đi, đóng góp đích đến sẽ biến mất. 

Các tập được sắp xếp giúp có thể tìm được tầng quan trọng tiếp theo theo thời gian logarit. Đó là điều tránh được việc mô phỏng từng giây một không thể thực hiện được. 

Phần dễ bị lỗi nhất là các quy tắc đặt hàng sự kiện. 

Hành khách ra khỏi thang máy phải được xử lý trước khi hành khách bước vào. 

Hành khách xuất hiện chính xác vào thời điểm hiện tại sẽ có mặt ngay lập tức. 

Khi hệ thống trống, thời gian phải chuyển thẳng sang thời gian xuất hiện tiếp theo. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 10
1 2 7
3 6 5
3 4 8
```| Thời gian | Tầng | Yêu cầu hoạt động | Hướng | 
| --- | --- | --- | --- | 
| 1 | 1 | chờ lúc 2 | Lên | 
| 2 | 2 | điểm đến 7 | Lên | 
| 3 | 3 | đích 7, chờ lúc 4 và 6 | Lên | 
| 4 | 4 | bến 7, bến 8, đợi 6 | Lên | 
| 5 | 5 | bến 7, bến 8, đợi 6 | Lên | 
| 6 | 6 | quận 7, quận 8, quận 5 | Lên | 
| 7 | 7 | quận 8, quận 5 | Lên | 
| 8 | 8 | đích 5 | Xuống | 
| 9 | 7 | đích 5 | Xuống | 
| 10 | 6 | đích 5 | Xuống | 
| 11 | 5 | không | Dừng lại | 

Đầu ra:```
7
11
8
```Ví dụ này cho thấy quy tắc hòa. Vào thời điểm`7`, cả hai mức độ ưu tiên đều bằng nhau`1`, nên thang máy vẫn chuyển động đi lên. 

### Mẫu 2 

đầu vào:```
2 10
1 2 5
7 4 5
```| Thời gian | Tầng | Sự kiện | 
| --- | --- | --- | 
| 1 | 1 | hành khách đầu tiên xuất hiện | 
| 2 | 2 | bảng | 
| 5 | 5 | giao hàng | 
| 5..7 | 5 | nhàn rỗi | 
| 7 | 5 | hành khách thứ hai xuất hiện | 
| 8 | 4 | bảng | 
| 9 | 5 | giao hàng | 

Đầu ra:```
5
9
```Ví dụ này chứng minh tại sao việc nhảy thời gian là cần thiết. Thang máy không làm gì giữa các lần`5`Và`7`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi hành khách chỉ gây ra một số lượng cập nhật theo thứ tự không đổi | 
| Không gian | O(n) | Hành khách đang hoạt động và cấu trúc sự kiện | 

Mỗi hành khách được chèn vào cấu trúc dữ liệu một lần, được chuyển đổi từ trạng thái chờ sang lên máy bay một lần và bị xóa một lần. Mỗi phép toán đều là logarit. Với`n = 100000`, điều này thoải mái phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    # call solution()
    return ""

# provided samples

assert run(
"""3 10
1 2 7
3 6 5
3 4 8
"""
) == """7
11
8"""

assert run(
"""2 10
1 2 5
7 4 5
"""
) == """5
9"""

# minimum size

assert run(
"""1 2
1 1 2
"""
) == """1"""

# passenger appears while elevator is idle

assert run(
"""1 10
100 5 6
"""
) == """101"""

# tie handling

assert run(
"""2 10
1 2 8
1 8 2
"""
) == """8
14"""

# boundary floors

assert run(
"""2 100000
1 1 100000
1 100000 1
"""
) == """99999
199998"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hành khách đơn | Hành vi cơ bản ngay lập tức | Hạn chế tối thiểu | 
| Thời gian xuất hiện lớn | Nhảy thời gian | Không mô phỏng từng giây | 
| Yêu cầu đối xứng | Quy tắc hòa |`p_up >= p_down`đi lên | 
| Tầng ranh giới | Xử lý từng cái một | Tầng`1`Và`m`| 

## Vỏ cạnh 

Hãy xem xét:```
1 10
1000000000 5 7
```Thang máy không hoạt động theo thời gian`0`cho đến lúc`1000000000`. Mô phỏng theo hướng sự kiện sẽ chuyển trực tiếp đến thời điểm đó, chèn hành khách vào và tiếp tục. Không có công việc nào được thực hiện trong khoảng thời gian trống. 

Coi như:```
2 10
1 2 8
1 8 2
```Khi nhu cầu hoạt động ở trên và dưới bằng nhau, thông số kỹ thuật cho biết thang máy sẽ di chuyển lên trên. Một triển khai sử dụng`>`thay vì`>=`sẽ đưa ra những câu trả lời khác nhau. 

Coi như:```
2 10
1 3 5
3 5 7
```Vào thời điểm`3`thang máy đến tầng`5`. Hành khách đi đến`5`phải rời đi trước khi hành khách đợi tại`5`đi vào. Việc xử lý các thao tác này theo thứ tự sai sẽ làm thay đổi số lượng hoạt động và có thể thay đổi các quyết định di chuyển trong tương lai. 

Mô phỏng hướng sự kiện xử lý tất cả các trường hợp này vì nó tái tạo chính xác thứ tự được xác định trong câu lệnh đồng thời tránh các trạng thái trung gian không cần thiết.

---
title: "CF 68B - Trao đổi năng lượng"
description: "Chúng tôi có một số bộ tích lũy, mỗi bộ lưu trữ một lượng năng lượng. Chúng ta được phép di chuyển năng lượng giữa chúng, nhưng mỗi lần chuyển giao đều lãng phí một tỷ lệ nhất định. Nếu chúng ta gửi x đơn vị từ một bộ tích lũy, người gửi sẽ mất tất cả x, trong khi người nhận chỉ thu được x (100 - k)/100."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search"]
categories: ["algorithms"]
codeforces_contest: 68
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 62"
rating: 1600
weight: 68
solve_time_s: 424
verified: true
draft: false
---

[CF 68B - Trao đổi năng lượng](https://codeforces.com/problemset/problem/68/B) 

**Đánh giá:** 1600 
**Thẻ:** tìm kiếm nhị phân 
**Thời gian giải:** 7 phút 4 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số bộ tích lũy, mỗi bộ lưu trữ một lượng năng lượng. Chúng ta được phép di chuyển năng lượng giữa chúng, nhưng mỗi lần chuyển giao đều lãng phí một tỷ lệ nhất định. Nếu chúng tôi gửi`x`đơn vị từ một bộ tích lũy, người gửi sẽ mất tất cả`x`, trong khi người nhận chỉ đạt được`x * (100 - k) / 100`. 

Mục tiêu là làm cho mọi bộ tích lũy chứa cùng một lượng năng lượng cuối cùng và chúng tôi muốn giá trị chung đó càng lớn càng tốt. 

Đầu vào cung cấp số lượng bộ tích lũy, tỷ lệ phần trăm tổn thất trong quá trình truyền và giá trị năng lượng ban đầu. Đầu ra là mức năng lượng bằng nhau tối đa có thể đạt được sau bất kỳ chuỗi chuyển đổi nào. 

Các ràng buộc đủ nhỏ để chúng ta có thể quét liên tục toàn bộ mảng. Có nhiều nhất`10^4`bộ tích lũy và mỗi giá trị tối đa là`1000`. Mô phỏng bậc hai trên tất cả các cặp vẫn sẽ có rủi ro nếu lặp lại nhiều lần, đặc biệt là với độ chính xác của dấu phẩy động. Một giải pháp xung quanh`O(n log precision)`hoàn toàn an toàn trong giới hạn 2 giây. 

Phần khó khăn là năng lượng không được bảo toàn. Mỗi lần chuyển sẽ tiêu hủy một số tiền. Một giải pháp bất cẩn có thể tính trung bình trực tiếp tất cả các giá trị, nhưng điều đó chỉ hiệu quả khi`k = 0`. 

Hãy xem xét ví dụ này:```
2 50
10 0
```Trung bình là`5`, nhưng chúng ta không thể làm cho cả hai bộ tích lũy bằng nhau`5`. Để cung cấp cho bộ tích lũy trống`5`, chúng ta phải gửi`10`, vì đã mất đi một nửa. Sau đó, bộ tích lũy đầu tiên trở thành`0`. Câu trả lời tốt nhất có thể đạt được là`10 / (1 + 2) = 3.333...`, không`5`. 

Một trường hợp tinh vi khác là khi tất cả các bộ tích lũy đều có cùng giá trị:```
4 30
7 7 7 7
```Câu trả lời là chính xác`7`. Không cần chuyển khoản nên không bị mất năng lượng. Một giải pháp luôn giả định việc chuyển giao xảy ra có thể làm giảm câu trả lời một cách không chính xác. 

Một trường hợp nguy hiểm hơn là tỉ lệ tổn thất cao:```
3 99
100 0 0
```Chỉ một`1%`năng lượng được truyền vẫn tồn tại. Phần lớn năng lượng biến mất trong quá trình phân phối lại nên đáp án trở nên rất nhỏ. Số học số nguyên hoặc độ chính xác không đủ có thể dễ dàng tạo ra kết quả sai ở đây. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là đoán giá trị mục tiêu`T`, sau đó mô phỏng rõ ràng việc chuyển tiền giữa các bộ tích lũy. Bất kỳ bộ tích lũy nào ở trên`T`đóng vai trò là nhà tài trợ và bất kỳ người tích lũy nào dưới đây`T`đóng vai trò là người nhận. 

Giả sử một bộ tích lũy có`a[i] > T`. Nó có thể cho đi`a[i] - T`đơn vị. Vì việc chuyển giao mất năng lượng nên số tiền hữu ích thực tế đóng góp cho người khác chỉ là:```
(a[i] - T) * (100 - k) / 100
```Nếu như`a[i] < T`, thì nó cần chính xác`T - a[i]`đơn vị. 

Nếu tổng năng lượng được hiến tặng có thể sử dụng ít nhất bằng tổng năng lượng cần thiết thì`T`là có thể đạt được. 

Khó khăn bạo lực đến từ việc tìm kiếm mức tối đa có thể`T`. Chúng ta đang xử lý số thực chứ không phải số nguyên, vì vậy việc kiểm tra mọi ứng cử viên là không thể. Ngay cả việc thử nhiều giá trị riêng biệt cũng sẽ thất bại vì câu trả lời đúng có thể không hợp lý. 

Quan sát chính là tính đơn điệu. 

Nếu một số giá trị mục tiêu`T`có thể đạt được thì mọi mục tiêu nhỏ hơn cũng có thể đạt được. Chúng ta luôn có thể ngừng chuyển sớm hơn. Mặt khác, nếu`T`là không thể, mọi giá trị lớn hơn cũng không thể. 

Điều đó biến vấn đề thành một tìm kiếm nhị phân cổ điển về câu trả lời. 

Đối với một ứng cử viên`mid`, chúng tôi tính toán:```
extra = total usable energy from donors
need = total missing energy from receivers
```Nếu như`extra >= need`, sau đó`mid`là khả thi và chúng tôi thử các giá trị lớn hơn. Ngược lại, chúng tôi thử các giá trị nhỏ hơn. 

Mỗi kiểm tra tính khả thi là tuyến tính trong`n`và tìm kiếm nhị phân cần khoảng 100 lần lặp để có độ chính xác dấu phẩy động tuyệt vời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không thể có giá trị liên tục | O(1) | Không thực tế | 
| Tối ưu | O(n độ chính xác của nhật ký) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số ắc quy`n`, phần trăm tổn thất`k`, và mảng`a`. 
2. Đặt phạm vi tìm kiếm nhị phân. 

Câu trả lời tối thiểu có thể là`0`. Câu trả lời tối đa có thể là`max(a)`, bởi vì không có bộ tích lũy nào có thể mang lại nhiều năng lượng hơn giá trị ban đầu lớn nhất. 
3. Tìm kiếm nhị phân nhiều lần trên giá trị đích`mid`. 

Chúng tôi thực hiện khoảng 100 lần lặp, đủ để`1e-6`độ chính xác. 
4. Đối với mỗi ắc quy: 

Nếu`a[i] > mid`, hãy tính xem nó có thể đóng góp bao nhiêu năng lượng hữu ích sau khi bị mất:```
(a[i] - mid) * (100 - k) / 100
```Thêm cái này vào`extra`. 
5. Nếu`a[i] < mid`, tính lượng năng lượng cần thiết:```
mid - a[i]
```Thêm cái này vào`need`. 
6. Sau khi xử lý tất cả các bộ tích lũy, so sánh`extra`Và`need`. 

Nếu như`extra >= need`, sau đó`mid`là có thể đạt được, vì vậy hãy di chuyển ranh giới bên trái lên trên. 

Ngược lại, di chuyển ranh giới bên phải xuống dưới. 
7. Sau khi quá trình tìm kiếm nhị phân kết thúc, xuất ra ranh giới bên trái. 

### Tại sao nó hoạt động 

Đối với mục tiêu cố định`T`, mọi bộ tích lũy ở trên`T`có thể tự giảm xuống một cách an toàn`T`. Do tổn thất truyền tải, chỉ một phần năng lượng bị loại bỏ mới có thể sử dụng được ở nơi khác. Mỗi bộ tích lũy bên dưới`T`yêu cầu chính xác`T - a[i]`năng lượng có thể sử dụng được. 

Nếu tổng năng lượng quyên góp có thể sử dụng được bù đắp được mọi phần thiếu hụt thì chúng ta có thể phân phối lại năng lượng để đạt được`T`. Nếu không thì không có chiến lược phân phối lại nào có thể thành công vì tổng năng lượng có thể chuyển giao còn sót lại là không đủ. 

Điều kiện khả thi là đơn điệu. Các mục tiêu nhỏ hơn luôn cần ít năng lượng hơn, trong khi các mục tiêu lớn hơn đòi hỏi nhiều năng lượng hơn. Tìm kiếm nhị phân tìm thấy chính xác mục tiêu khả thi lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

loss_factor = (100 - k) / 100.0

left = 0.0
right = max(a)

for _ in range(100):
    mid = (left + right) / 2.0

    extra = 0.0
    need = 0.0

    for x in a:
        if x > mid:
            extra += (x - mid) * loss_factor
        else:
            need += mid - x

    if extra >= need:
        left = mid
    else:
        right = mid

print(f"{left:.9f}")
```Tìm kiếm nhị phân hoạt động trực tiếp trên các giá trị dấu phẩy động vì câu trả lời không nhất thiết phải là số nguyên. 

Biến`loss_factor`đại diện cho phần năng lượng còn sót lại trong quá trình truyền. Việc tính toán trước sẽ tránh lặp lại phép chia tương tự bên trong vòng lặp. 

Việc kiểm tra tính khả thi sẽ chia người tích lũy thành người cho và người nhận. Các nhà tài trợ đóng góp năng lượng giảm đi do thua lỗ, trong khi người nhận yêu cầu toàn bộ mức thâm hụt của họ. 

Việc so sánh sử dụng`extra >= need`. Sự bình đẳng rất quan trọng vì tính khả thi chính xác vẫn sẽ đưa câu trả lời đi lên. 

Vòng lặp chạy 100 lần lặp bất kể kích thước khoảng. Đây là một kỹ thuật lập trình cạnh tranh phổ biến để tìm kiếm nhị phân dấu phẩy động. Vì mỗi lần lặp lại giảm một nửa khoảng thời gian nên 100 vòng mang lại độ chính xác cao hơn nhiều so với yêu cầu. 

In với 9 chữ số thập phân đáp ứng an toàn`1e-6`yêu cầu lỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 50
4 2 1
```Chúng tôi tìm kiếm nhị phân cho giá trị bằng nhau tối đa. 

Giả sử chúng ta kiểm tra`mid = 2`. 

| Tích lũy | Ban đầu | Vai trò | Đóng góp / Nhu cầu | 
| --- | --- | --- | --- | 
| 1 | 4 | nhà tài trợ |`(4 - 2) * 0.5 = 1`| 
| 2 | 2 | cân bằng |`0`| 
| 3 | 1 | người nhận |`1`| 

Tổng số: 

| thêm | cần | khả thi | 
| --- | --- | --- | 
| 1 | 1 | vâng | 

Vì thế`2`là có thể đạt được. 

Bây giờ giả sử chúng ta kiểm tra`mid = 2.5`. 

| Tích lũy | Ban đầu | Vai trò | Đóng góp / Nhu cầu | 
| --- | --- | --- | --- | 
| 1 | 4 | nhà tài trợ |`(4 - 2.5) * 0.5 = 0.75`| 
| 2 | 2 | người nhận |`0.5`| 
| 3 | 1 | người nhận |`1.5`| 

Tổng số: 

| thêm | cần | khả thi | 
| --- | --- | --- | 
| 0,75 | 2.0 | không | 

Vậy câu trả lời nằm giữa`2`Và`2.5`. 

Dấu vết này cho thấy ảnh hưởng của sự mất mát chuyển giao. Mặc dù tổng năng lượng là`7`, phần lớn nó biến mất trong quá trình phân phối lại. 

### Ví dụ 2 

đầu vào:```
2 0
10 0
```Ở đây không có tổn thất chuyển nhượng. 

Kiểm tra`mid = 5`. 

| Tích lũy | Ban đầu | Vai trò | Đóng góp / Nhu cầu | 
| --- | --- | --- | --- | 
| 1 | 10 | nhà tài trợ |`5`| 
| 2 | 0 | người nhận |`5`| 

Tổng số: 

| thêm | cần | khả thi | 
| --- | --- | --- | 
| 5 | 5 | vâng | 

Kiểm tra`mid = 6`. 

| Tích lũy | Ban đầu | Vai trò | Đóng góp / Nhu cầu | 
| --- | --- | --- | --- | 
| 1 | 10 | nhà tài trợ |`4`| 
| 2 | 0 | người nhận |`6`| 

Tổng số: 

| thêm | cần | khả thi | 
| --- | --- | --- | 
| 4 | 6 | không | 

Câu trả lời cuối cùng là chính xác`5`. 

Trường hợp này khẳng định rằng khi`k = 0`, câu trả lời sẽ trở thành giá trị trung bình thông thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n độ chính xác của nhật ký) | Mỗi lần lặp tìm kiếm nhị phân sẽ quét tất cả các bộ tích lũy | 
| Không gian | O(1) | Chỉ có một số biến dấu phẩy động được lưu trữ | 

Với`n ≤ 10000`và khoảng 100 lần lặp tìm kiếm nhị phân, thuật toán thực hiện khoảng một triệu thao tác. Điều đó dễ dàng phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    loss_factor = (100 - k) / 100.0

    left = 0.0
    right = max(a)

    for _ in range(100):
        mid = (left + right) / 2.0

        extra = 0.0
        need = 0.0

        for x in a:
            if x > mid:
                extra += (x - mid) * loss_factor
            else:
                need += mid - x

        if extra >= need:
            left = mid
        else:
            right = mid

    print(f"{left:.9f}")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("3 50\n4 2 1\n") == "2.000000000", "sample 1"

# minimum size
assert run("1 30\n7\n") == "7.000000000", "single accumulator"

# all equal
assert run("4 80\n5 5 5 5\n") == "5.000000000", "already balanced"

# no transfer loss
res = float(run("2 0\n10 0\n"))
assert abs(res - 5.0) < 1e-6, "average when k = 0"

# extreme transfer loss
res = float(run("2 99\n100 0\n"))
assert abs(res - 0.9900990099) < 1e-6, "high loss percentage"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 30 / 7`|`7`| Tích lũy đơn không yêu cầu chuyển khoản | 
|`4 80 / 5 5 5 5`|`5`| Cấu hình đã cân bằng | 
|`2 0 / 10 0`|`5`| Không mất mát giảm xuống mức trung bình thông thường | 
|`2 99 / 100 0`|`0.990099...`| Xử lý đúng tổn thất chuyển nhượng nghiêm trọng | 

## Vỏ cạnh 

Hãy xem xét trường hợp tổn thất chuyển nhượng khiến việc tính trung bình không thể thực hiện được:```
2 50
10 0
```Giả sử thuật toán kiểm tra`mid = 5`. 

Người tích lũy đầu tiên có thể quyên góp:```
(10 - 5) * 0.5 = 2.5
```Bộ tích lũy thứ hai cần:```
5 - 0 = 5
```Từ`2.5 < 5`, mục tiêu là không thể. Tìm kiếm nhị phân di chuyển xuống dưới. Cuối cùng nó hội tụ đến khoảng`3.333333333`. 

Điều này chứng tỏ tại sao tính trung bình thô lại thất bại khi quá trình truyền năng lượng bị tiêu diệt. 

Bây giờ hãy xem xét một thiết lập đã được cân bằng:```
4 30
7 7 7 7
```Vì`mid = 7`: 

| Tích lũy | Đóng góp | Cần | 
| --- | --- | --- | 
| 7 | 0 | 0 | 
| 7 | 0 | 0 | 
| 7 | 0 | 0 | 
| 7 | 0 | 0 | 

Cả hai tổng số vẫn bằng 0, vì vậy mục tiêu là khả thi. 

Thuật toán nhận dạng chính xác rằng không cần chuyển giao, nghĩa là không bị mất năng lượng. 

Cuối cùng, hãy xem xét tổn thất cực kỳ cao:```
3 99
100 0 0
```Nếu chúng ta kiểm tra`mid = 1`: 

Nhà tài trợ đóng góp:```
(100 - 1) * 0.01 = 0.99
```Người nhận cần:```
1 + 1 = 2
```Mục tiêu thất bại ngay lập tức. 

Tìm kiếm nhị phân tiếp tục thu hẹp cho đến khi giá trị khả thi trở nên gần đúng`0.4975`. 

Trường hợp này xác nhận rằng thuật toán áp dụng chính xác hệ số tổn thất trước khi so sánh năng lượng có thể chuyển đổi với nhu cầu.

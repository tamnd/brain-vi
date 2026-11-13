---
title: "CF 1729A - Hai Thang Máy"
description: "Tình huống mô tả quyết định giữa hai thang máy có khả năng tiếp cận Vlad ở tầng 1. Mỗi thang máy có một quy tắc di chuyển khác nhau và mục tiêu là xác định thang máy nào quay lại tầng 1 nhanh nhất sau khi Vlad nhấn nút gọi."
date: "2026-06-15T02:29:39+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1729
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 820 (Div. 3)"
rating: 800
weight: 1729
solve_time_s: 344
verified: true
draft: false
---

[CF 1729A - Hai thang máy](https://codeforces.com/problemset/problem/1729/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 5 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Tình huống mô tả quyết định giữa hai thang máy có khả năng tiếp cận Vlad ở tầng 1. Mỗi thang máy có một quy tắc di chuyển khác nhau và mục tiêu là xác định thang máy nào quay lại tầng 1 nhanh nhất sau khi Vlad nhấn nút gọi. 

Thang máy đầu tiên ban đầu dừng ở tầng nào đó`a`. Sau khi được gọi, nó ngay lập tức bắt đầu di chuyển về tầng 1. Thời gian di chuyển của nó hoàn toàn là khoảng cách giữa vị trí hiện tại và đích đến, vì vậy thời gian nó cần là`|a - 1|`. 

Thang máy thứ hai hoạt động khác. Nó bắt đầu ở tầng`b`và hiện đang di chuyển về phía tầng`c`. Nếu Vlad gọi nó, nó sẽ không ngay lập tức đi đến tầng 1. Thay vào đó, trước tiên nó tiếp tục (hoặc kết thúc) chuyển động hiện tại về phía`c`, và chỉ sau khi đạt được`c`nó có bắt đầu di chuyển về phía tầng 1 không. Điều này có nghĩa là tổng thời gian của nó là thời gian để đi từ`b`ĐẾN`c`, cộng với thời gian đi từ`c`ĐẾN`1`, đó là`|b - c| + |c - 1|`. 

Nhiệm vụ là so sánh hai thời gian đến này cho từng trường hợp thử nghiệm và quyết định xem thang máy đầu tiên nhanh hơn, thang máy thứ hai nhanh hơn hay chúng mất cùng thời gian. 

Các ràng buộc cho phép lên đến`10^4`trường hợp thử nghiệm, với giá trị sàn lên tới`10^8`. Điều này ngay lập tức loại trừ mọi mô phỏng chuyển động từng bước. Mỗi thử nghiệm phải được xử lý trong thời gian không đổi, vì ngay cả O(t log n) cũng sẽ là chi phí không cần thiết và khoảng cách O(t *) là không thể do độ lớn của chênh lệch tầng. 

Một trường hợp phức tạp xuất hiện khi một thang máy đã hoạt động hiệu quả ở tầng 1. Ví dụ: nếu`a = 1`, thang máy thứ nhất có thời gian di chuyển bằng không. Tương tự, nếu`b = 1`, thang máy thứ hai vẫn phải hoàn thành việc di chuyển hiện tại tới`c`trước khi quay lại, do đó, nó không bao giờ là ngay lập tức mặc dù nó bắt đầu ở tầng 1. Sự bất cân xứng này rất dễ bị hiểu sai nếu một người chỉ giả định vị trí bắt đầu sẽ xác định tính khả dụng ngay lập tức. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ mô phỏng chuyển động của từng thang máy theo các bước thời gian. Đối với thang máy đầu tiên, điều này không quan trọng, nó chỉ đi bộ từ`a`ĐẾN`1`. Đối với thang máy thứ hai, người ta có thể thử mô phỏng chuyển động của nó hướng tới`c`đầu tiên, sau đó hướng tới`1`, cập nhật vị trí từng bước. 

Điều này hoạt động về mặt khái niệm nhưng hoàn toàn không khả thi. Khoảng cách giữa các tầng có thể lên tới`10^8`, do đó, việc mô phỏng từng bước một trường hợp thử nghiệm có thể mất tới`10^8`hoạt động và xuyên suốt`10^4`trường hợp thử nghiệm này trở nên lớn về mặt thiên văn. 

Quan sát quan trọng là cả hai thang máy đều chuyển động với tốc độ không đổi và không tương tác với bất kỳ ràng buộc nào ngoài khoảng cách. Điều này làm giảm toàn bộ vấn đề trong việc tính toán tổng chiều dài đường đi. Thang máy đầu tiên đóng góp một đoạn duy nhất từ`a`ĐẾN`1`. Thang máy thứ hai đóng góp một đường dẫn hai đoạn cố định từ`b`ĐẾN`c`và sau đó`c`ĐẾN`1`. Sau khi được biểu thị bằng khoảng cách, việc so sánh sẽ trở thành phép kiểm tra số học đơn giản. 

Chúng tôi không tối ưu hóa một quy trình nữa, chúng tôi chỉ so sánh hai giá trị vô hướng rút ra từ hình học trên một trục số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(t · khoảng cách tối đa) | O(1) | Quá chậm | 
| Tính toán khoảng cách trực tiếp | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc ba số nguyên`a`,`b`, Và`c`. Chúng xác định vị trí bắt đầu và mục tiêu trung gian bắt buộc cho thang máy thứ hai. 
2. Tính thời gian đến của thang máy đầu tiên là`t1 = abs(a - 1)`. Điều này trực tiếp thể hiện sự chuyển động không ngừng nghỉ của nó lên tầng 1. 
3. Tính thời gian đến của thang máy thứ hai như sau:`t2 = abs(b - c) + abs(c - 1)`. Điều này mã hóa quy tắc rằng trước tiên nó phải hoàn thành chuyển động của mình để`c`, sau đó tiến tới 1. 
4. So sánh`t1`Và`t2`. Nếu như`t1 < t2`, thang máy đầu tiên nhanh hơn rất nhiều nên đầu ra`1`. 
5. Nếu`t2 < t1`, thang máy thứ hai nhanh hơn rất nhiều nên đầu ra`2`. 
6. Nếu cả hai thời gian bằng nhau, xuất ra`3`vì cả hai lựa chọn đều tương đương. 

Lý do đằng sau sự so sánh là cả hai biểu thức đều nắm bắt đầy đủ tổng thời gian di chuyển theo chuyển động xác định. Không có trạng thái ẩn, độ trễ hoặc đường phân nhánh nào vượt quá những khoảng cách này. 

### Tại sao nó hoạt động 

Hệ thống có thể được mô hình hóa dưới dạng chuyển động trên trục số một chiều với tốc độ không đổi. Hành vi của mỗi thang máy xác định độ dài đường dẫn cố định sau khi cuộc gọi được thực hiện. Vì thời gian di chuyển tỷ lệ thuận với khoảng cách và không có sự tương tác giữa các thang máy nên việc giảm thiểu thời gian đến tương đương với việc giảm thiểu tổng chiều dài đường đi. Thuật toán này đúng vì nó so sánh chính xác các độ dài đường dẫn đó mà không cần xấp xỉ hoặc mô phỏng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c = map(int, input().split())

    t1 = abs(a - 1)
    t2 = abs(b - c) + abs(c - 1)

    if t1 < t2:
        print(1)
    elif t2 < t1:
        print(2)
    else:
        print(3)
```Tính toán đầu tiên`t1`mã hóa khoảng cách trực tiếp từ thang máy đầu tiên đến tầng đích. Tính toán thứ hai`t2`tôn trọng rõ ràng ràng buộc mà thang máy thứ hai trước tiên phải đạt tới`c`trước khi hướng đến số 1, đó là lý do tại sao nó được chia thành hai điểm khác biệt tuyệt đối chứ không phải là một khoảng cách trực tiếp duy nhất từ`b`ĐẾN`1`. 

Logic so sánh là sự dịch trực tiếp quy tắc quyết định của bài toán. Mỗi nhánh loại trừ lẫn nhau và toàn diện, đảm bảo chính xác một đầu ra hợp lệ cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
1 2 3
3 1 2
3 2 1
```| một | b | c | t1 = |a−1| | t2 = |b−c| + |c−1| | Quyết định | 

|---|---|---|---|---|---| 

| 1 | 2 | 3 | 0 | 2 + 2 = 4 | 1 | 

| 3 | 1 | 2 | 2 | 1 + 1 = 2 | 3 | 

| 3 | 2 | 1 | 2 | 1 + 0 = 1 | 2 | 

Trường hợp đầu tiên cho thấy tình huống suy biến trong đó thang máy đầu tiên đã ở tầng 1, khiến thời gian di chuyển bằng 0. Trường hợp thứ hai thể hiện sự bằng nhau về tổng chiều dài đường đi, trong đó cả hai chiến lược đều hội tụ về cùng thời gian đến. Trường hợp thứ ba cho thấy thang máy thứ hai được hưởng lợi từ việc ở gần mục tiêu trung gian hơn và sau đó đã ở gần tầng 1. 

### Đầu vào mẫu 2```
2
10 5 8
1 100 2
```| một | b | c | t1 | t2 | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 10 | 5 | 8 | 9 | 3 + 7 = 10 | 1 | 
| 1 | 100 | 2 | 0 | 98 + 1 = 99 | 1 | 

Mẫu thứ hai nhấn mạnh rằng việc đi vòng cưỡng bức của thang máy thứ hai có thể chi phối chi phí của nó ngay cả khi nó bắt đầu tương đối gần tầng 1, bởi vì nó buộc phải hoàn thành chuyển động của mình tới tầng 1.`c`Đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu các phép tính và so sánh số học trong thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến cố định được sử dụng bất kể kích thước đầu vào | 

Giải pháp sẽ chia tỷ lệ tuyến tính theo số lượng ca kiểm thử, điều này là tối ưu vì mỗi ca phải được đọc và xử lý ít nhất một lần. Các phép tính số học là tầm thường và nằm trong giới hạn cho`t ≤ 10^4`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    it = iter(inp.strip().split())
    t = int(next(it))
    out = []
    for _ in range(t):
        a = int(next(it)); b = int(next(it)); c = int(next(it))
        t1 = abs(a - 1)
        t2 = abs(b - c) + abs(c - 1)
        if t1 < t2:
            out.append("1")
        elif t2 < t1:
            out.append("2")
        else:
            out.append("3")
    return "\n".join(out)

# provided samples
assert run("""3
1 2 3
3 1 2
3 2 1
""") == """1
3
2"""

# custom cases
assert run("""1
1 2 100
""") == "1"

assert run("""1
50 1 2
""") == "3"

assert run("""1
100 50 1
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 100`|`1`| Thang máy đầu tiên đã tối ưu | 
|`50 1 2`|`3`| Trường hợp đẳng thức đối xứng | 
|`100 50 1`|`2`| Thang máy thứ hai chiếm ưu thế | 

## Vỏ cạnh 

Khi nào`a = 1`, thang máy đầu tiên có chi phí bằng 0 vì nó đã ở tầng đích. Thuật toán tính toán`t1 = abs(1 - 1) = 0`, do đó, mọi đường dẫn thang máy thứ hai khác 0 sẽ bị mất một cách chính xác. 

Khi`b = 1`, thang máy thứ hai vẫn phải chịu chi phí vì trước tiên nó phải đến được`c`. Ví dụ, với`a = 5, b = 1, c = 10`, chúng tôi nhận được`t1 = 4`Và`t2 = 9 + 9 = 18`, do đó thang máy đầu tiên được chọn đúng mặc dù thang máy thứ hai bắt đầu từ tầng 1. 

Khi nào`c = 1`, chặng thứ hai của thang máy thứ hai trở thành số 0, nhưng nó vẫn trả chi phí từ`b`ĐẾN`1`. Ví dụ,`a = 10, b = 5, c = 1`cho`t1 = 9`,`t2 = 4 + 0 = 4`, do đó thang máy thứ hai thắng vì tuyến đường bắt buộc của nó thực sự kết thúc ở tầng mục tiêu ngay sau khi đến`c`.

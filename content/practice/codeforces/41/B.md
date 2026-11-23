---
title: "CF 41B - Đô la sao Hỏa"
description: "Chúng ta được yêu cầu tối đa hóa số tiền mà Vasya có thể có vào cuối _n_ ngày nếu anh ta bắt đầu với một số tiền nhất định và có thể mua và sau đó bán đô la sao Hỏa."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 41
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 40 (Div. 2)"
rating: 1400
weight: 41
solve_time_s: 68
verified: true
draft: false
---
[CF 41B - Đô la sao Hỏa](https://codeforces.com/problemset/problem/41/B) 

**Đánh giá:** 1400 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tối đa hóa số tiền mà Vasya có thể có vào cuối _n_ ngày nếu anh ta bắt đầu với một số tiền nhất định và có thể mua và sau đó bán đô la sao Hỏa. Mỗi ngày có một giá đô la và Vasya chỉ có thể thực hiện một hoạt động mua-bán, mua một số nguyên đô la và bán chúng sau đó với giá nguyên đầy đủ vào ngày tiếp theo. 

Đầu vào cung cấp số ngày, số tiền ban đầu tính bằng bourles và danh sách giá đô la hàng ngày. Sản lượng là số lượng rượu tối đa mà Vasya có thể có sau khi mua và bán một lần. 

Các ràng buộc rất nhỏ: cả số ngày và số tiền ban đầu nhiều nhất là 2000, điều này cho phép các thuật toán mở rộng theo bậc hai, bởi vì các hoạt động 20002 chỉ khoảng bốn triệu, vừa vặn thoải mái trong giới hạn thời gian 2 giây. Thách thức chính không phải là hiệu quả mà là mô hình hóa chính xác hành vi mua/bán số nguyên. 

Một số trường hợp đặc biệt không rõ ràng. Ví dụ: nếu giá đô la không bao giờ tăng, chiến lược tối ưu là không bao giờ mua, vì vậy số tiền tối đa là số tiền ban đầu. Nếu số tiền ban đầu nhỏ hơn tất cả giá đô la, Vasya không thể mua bất kỳ đô la nào và một lần nữa số tiền tối đa không thay đổi. Một giải pháp bất cẩn cho rằng anh ta luôn mua hàng vào ngày đầu tiên sẽ đưa ra câu trả lời sai ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là vũ phu. Mỗi ngày, hãy cân nhắc việc mua đô la vào ngày đó. Đối với mỗi ngày mua tiềm năng, hãy mô phỏng việc bán vào mỗi ngày sau đó. Tính số đô la mà Vasya có thể mua (chia số nguyên của số tiền hiện tại của anh ấy cho giá), nhân với giá bán và cộng số tiền còn lại để tính tổng cuối cùng. Theo dõi mức tối đa trên tất cả các cặp mua/bán. 

Phương pháp này đúng vì nó liệt kê tất cả các quyết định mua/bán có thể xảy ra. Số lượng hoạt động là khoảng$O(n^2)$bởi vì với mỗi n ngày mà chúng tôi có thể cân nhắc mua, chúng tôi sẽ kiểm tra tất cả những ngày sau đó là những ngày có khả năng bán. Với n 2000, nhiều nhất là bốn triệu phép tính, có thể chấp nhận được. 

Thông tin chi tiết quan trọng giúp chúng ta đơn giản hóa một chút là giá bán tối đa sau một ngày nhất định mới là điều quan trọng. Thay vì kiểm tra tất cả các ngày sau đó cho mỗi ngày mua, chúng ta có thể tính toán trước mức giá tối đa từ ngày _i_ trở đi. Khi đó lợi nhuận khi mua vào ngày _i_ chỉ đơn giản là`(max_price_after_i - price[i]) * dollars_can_buy`. Điều này làm giảm các phép so sánh dư thừa và đơn giản hóa lý luận, mặc dù về mặt tiệm cận, nó vẫn là O(n²) vì việc tính toán đô la và số tiền còn lại vẫn liên quan đến số học số nguyên mỗi ngày. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Theo dõi tối đa được tối ưu hóa | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số ngày`n`và số tiền ban đầu`b`, sau đó đọc mảng giá hàng ngày`a`. 
2. Khởi tạo một biến`max_money`với số tiền ban đầu, đại diện cho kịch bản Vasya không mua bất kỳ đô la nào. 
3. Lặp lại mỗi ngày`i`như một ngày mua tiềm năng. Tính số đô la mà Vasya có thể mua`b // a[i]`. Nếu anh ta không thể mua được đồng đô la nào, hãy chuyển sang ngày hôm sau. 
4. Cho mỗi ngày mua`i`, lặp lại mỗi ngày sau đó`j > i`như một ngày bán hàng tiềm năng. Tính số tiền cuối cùng nếu Vasya bán trong ngày`j`BẰNG`dollars_bought * a[j] + leftover_bourles`. Cập nhật`max_money`nếu tổng số này lớn hơn. 
5. Sau khi kiểm tra tất cả các cặp mua/bán có thể, hãy in`max_money`. 

Tại sao nó hoạt động: Tại mỗi lần lặp lại, chúng tôi xem xét tất cả các ngày mua và bán khả thi và tính toán tôn trọng các giao dịch mua bằng đô la nguyên và số tiền còn lại. Vì mọi kịch bản hợp lệ đều được liệt kê nên mức tối đa trên tất cả các lần lặp được đảm bảo là mức tối đa thực sự. Việc bỏ qua những ngày không thể mua được đô la sẽ đảm bảo tính chính xác cho các trường hợp khó khăn khi số tiền ban đầu quá nhỏ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, b = map(int, input().split())
a = list(map(int, input().split()))

max_money = b

for i in range(n):
    dollars = b // a[i]
    leftover = b % a[i]
    if dollars == 0:
        continue
    for j in range(i + 1, n):
        total = dollars * a[j] + leftover
        if total > max_money:
            max_money = total

print(max_money)
```Giải pháp này trước tiên đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Chúng tôi duy trì`max_money`để tính đến khả năng không mua gì cả. Đối với mỗi ngày mua tiềm năng, phép chia số nguyên sẽ cho biết số đô la mà Vasya có thể mua và phép tính modulo sẽ cho ra số tiền còn lại. Chúng tôi bỏ qua những lần lặp lại mà không thể mua được đô la. Các vòng lặp lồng nhau xem xét tất cả các ngày bán có thể, cập nhật số tiền tối đa được thấy. Sử dụng số học số nguyên một cách cẩn thận đảm bảo không có lỗi dấu phẩy động. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2 4
3 7
```| Mua ngày tôi | Đô la | Còn sót lại | Bán Ngày j | Tổng số tiền | max_money | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1*7 + 1 = 8 | 8 | 

Giải thích: Vasya có thể mua 1 đô la ở 3 bourle, để lại 1 bourle. Bán vào ngày 1 lúc 7 mang lại 8, đây là mức tối ưu. 

**Ví dụ tùy chỉnh** 

đầu vào:```
3 5
5 4 3
```| Mua ngày tôi | Đô la | Còn sót lại | Bán Ngày j | Tổng số tiền | max_money | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1*4 + 0 = 4 | 5 | 
| 0 | 1 | 0 | 2 | 1*3 + 0 = 3 | 5 | 
| 1 | 1 | 1 | 2 | 1*3 + 1 = 4 | 5 | 

Giải thích: Giá giảm hàng ngày. Mua bất kỳ ngày nào dẫn đến ít hơn 5 giao dịch ban đầu. Tốt nhất là không làm gì cả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi ngày mua sẽ kiểm tra tất cả các ngày bán sau đó; n 2000 nên ~4e6 phép toán | 
| Không gian | O(n) | Mảng giá, các biến phụ không đổi | 

Với những hạn chế, giải pháp này đủ nhanh. Việc sử dụng bộ nhớ nhỏ, dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, b = map(int, input().split())
    a = list(map(int, input().split()))
    max_money = b
    for i in range(n):
        dollars = b // a[i]
        leftover = b % a[i]
        if dollars == 0:
            continue
        for j in range(i + 1, n):
            total = dollars * a[j] + leftover
            if total > max_money:
                max_money = total
    return str(max_money)

# provided sample
assert run("2 4\n3 7\n") == "8", "sample 1"

# minimum-size input
assert run("1 1\n1\n") == "1", "min input"

# prices decreasing
assert run("3 5\n5 4 3\n") == "5", "no profit"

# all-equal prices
assert run("4 10\n2 2 2 2\n") == "10", "no profit all same"

# buy on first day, sell on last
assert run("3 10\n2 5 6\n") == "30", "first to last"

# initial money too small
assert run("3 1\n2 3 4\n") == "1", "cannot buy"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 \n 1 | 1 | Đầu vào tối thiểu | 
| 3 5 \n 5 4 3 | 5 | Giá giảm, không mua | 
| 4 10 \n 2 2 2 2 | 10 | Tất cả giá đều bằng nhau, mua không cần thiết | 
| 3 10 \n 2 5 6 | 30 | Mua ngày đầu, bán ngày cuối tối đa hóa lợi nhuận | 
| 3 1 \n 2 3 4 | 1 | Số tiền ban đầu quá nhỏ để mua | 

## Vỏ cạnh 

Nếu số tiền ban đầu nhỏ hơn tất cả các mức giá, Vasya không thể mua được đồng đô la nào. Ví dụ,`b = 1`và giá cả`[2,3,4]`. Thuật toán tính toán`dollars = b // a[i]`bằng 0 cho mỗi ngày, bỏ qua tất cả các lần lặp và rời khỏi`max_money = b = 1`. 

Nếu giá giảm một cách đơn điệu, việc mua sớm sẽ mang lại số tiền thấp hơn ban đầu, vì vậy việc bỏ qua việc mua mang lại lợi ích tối đa. Dấu vết xác nhận rằng thuật toán so sánh chính xác tổng số tiền với`max_money`ở mỗi bước và không bao giờ chọn một phương án tệ hơn. 

Nếu giá không đổi thì bất kỳ chu kỳ mua-bán nào cũng tương đương với việc giữ số tiền ban đầu và

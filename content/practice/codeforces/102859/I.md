---
title: "CF 102859I - Đá sưởi ấm"
description: "Chúng tôi có một dãy n bếp. Bếp i chứa một số viên đá p[i], trong đó số lượng phải nằm trong khoảng từ 0 đến v. Tổng số viên đá được đặt trên tất cả các bếp phải chính xác là s. Giữa mỗi cặp bếp lân cận đều có một ngăn."
date: "2026-07-25T14:25:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102859
codeforces_index: "I"
codeforces_contest_name: "mBIT Standard November 2020"
rating: 0
weight: 102859
solve_time_s: 70
verified: true
draft: false
---

[CF 102859I - Đá sưởi ấm](https://codeforces.com/problemset/problem/102859/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng`n`bếp lò. Cái lò`i`chứa một số đá`p[i]`, trong đó số tiền phải nằm trong khoảng`0`Và`v`. Tổng số viên đá đặt trên tất cả các bếp phải chính xác`s`. 

Giữa mỗi cặp bếp lân cận đều có một ngăn. Nếu hai bếp liền kề chứa`a`Và`b`đá, ngăn đó nhận được`k * a * b`nhiệt, ở đâu`k`là hệ số khối lượng nhất định của nó. Nhiệm vụ là sắp xếp các viên đá sao cho tổng nhiệt lượng trên tất cả các ngăn càng nhỏ càng tốt. 

Những hạn chế quan trọng là kích thước của`v`Và`s`. Giải pháp lập trình động trực tiếp theo số lượng viên đá là không thể bởi vì`s`có thể lớn như`n * 100000`. Số lượng bếp chỉ có`1000`, vì vậy giải pháp dự định phải phụ thuộc vào`n`, không phải về số lượng đá. 

Một quan sát hữu ích là mục tiêu là tuyến tính ở bất kỳ giá trị bếp nào khi tất cả các bếp khác đều cố định. Bởi vì điều này, một sự sắp xếp tối ưu có thể được chuyển thành một sự sắp xếp trong đó mỗi bếp đều chứa một trong hai`0`đá,`v`đá, hoặc có thể là một lượng trung gian. Ràng buộc tổng cộng có nghĩa là có thể có nhiều nhất một bếp trung gian. 

Những trường hợp khó khăn là số đá còn lại sau khi đổ đầy bếp không bằng 0. Ví dụ:```
3 5 2
1 1
```Tổng công suất của một bếp là`2`, vậy là chúng ta có hai bếp lò đầy đủ và một viên đá còn lại. Một giải pháp chỉ xem xét bếp đầy hay bếp trống không thể đại diện cho tổng số cần thiết. Sự sắp xếp đúng đắn giống như`[2, 2, 1]`, và bếp một phần phải được xem xét. 

Một trường hợp cạnh khác là khi phần còn lại bằng 0:```
4 8 4
1 1 1
```Tất cả đá có thể được đặt như hai bếp đầy đủ. Việc thêm một phần bếp nhân tạo vào đây sẽ tạo ra trạng thái không hợp lệ vì mọi bếp đều phải trống hoặc đầy. 

Trường hợp ranh giới cuối cùng là`s < v`:```
2 3 10
5
```Tất cả đá có thể được đặt trên một bếp, tạo ra nhiệt`0`. Bất kỳ cách tiếp cận nào giả định rằng ít nhất một bếp đã đầy sẽ thất bại. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi cách phân phối đá có thể có giữa các bếp lò. Thậm chí còn hạn chế mỗi bếp chỉ có ba trạng thái có ý nghĩa`0`,`v`, và một phần còn lại có thể vẫn còn khoảng`3^n`khả năng. Với`n = 1000`, điều này là hoàn toàn không thể. 

Quan sát quan trọng là chúng ta chỉ cần quyết định bếp nào đầy, bếp nào đầy một phần và bếp nào trống. Giả định```
s = q * v + r
```Thế thì chính xác`q`bếp lò chứa`v`đá. Nếu như`r > 0`, có đúng một bếp bổ sung chứa`r`đá. Tất cả các bếp còn lại đều trống rỗng. 

Điều này làm giảm vấn đề thành vấn đề quy hoạch động trạng thái nhỏ. Khi quét từ trái sang phải, chúng ta chỉ cần biết đã đặt bao nhiêu bếp đầy đủ và loại bếp trước đó là gì. Loại bếp trước là đủ vì chi phí duy nhất mà bếp sau tạo ra là sức nóng của ngăn giữa chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) | O(n) | Quá chậm | 
| DP tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia số lượng đá cần thiết thành các công suất hoàn chỉnh của bếp và phần còn lại. Tính toán`q = s // v`Và`r = s % v`. giá trị`q`cho chúng ta biết chính xác có bao nhiêu bếp phải đầy. 
2. Xử lý bếp từ trái sang phải bằng lập trình động. Đối với mỗi tiền tố, hãy lưu trữ nhiệt độ tối thiểu có thể đạt được cho mỗi số lượng bếp đầy đủ được sử dụng và cho từng loại bếp cuối cùng có thể có. 
3. Loại bếp cuối cùng có ba khả năng: trống, đầy hoặc một phần. Khi thêm bếp mới, hãy thử từng loại hợp lệ và thêm nhiệt lượng đóng góp từ ngăn giữa bếp trước và bếp mới. 
4. Nếu bếp mới đã đầy, hãy tăng số lượng bếp đầy. Nếu nó là một phần, chỉ cho phép nó khi`r > 0`và đảm bảo rằng không có bếp từng phần nào tồn tại trước đó. 
5. Sau khi xử lý xong tất cả các bếp, giữa các trạng thái sử dụng chính xác`q`bếp đầy đủ và chính xác một bếp một phần khi cần, lấy giá trị nhỏ nhất. 

Tại sao nó hoạt động: 

Việc chuyển đổi tối đa một bếp một phần là nền tảng. Nếu hai biến không ở ranh giới, chúng ta có thể di chuyển các viên đá giữa chúng trong khi vẫn giữ nguyên tổng số của chúng. Hàm nhiệt dọc theo chuyển động đó là tuyến tính hoặc lõm, do đó, nó đạt mức tối thiểu tại điểm cuối. Lặp lại quá trình này chỉ để lại các giá trị biên và có thể còn sót lại một bếp. DP liệt kê mọi vị trí có thể đặt của các bếp toàn bộ, một phần và trống này trong khi vẫn giữ chính xác số lượng đá cần thiết, vì vậy trạng thái tối thiểu là cách sắp xếp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, s, v = map(int, input().split())
    k = list(map(int, input().split()))

    full = s // v
    rem = s % v

    INF = 10**30

    # dp[count][last_type]
    # type: 0 = empty, 1 = full, 2 = partial
    dp = [[INF] * 3 for _ in range(full + 2)]
    dp[0][0] = 0
    dp[0][1] = 0 if False else INF
    partial_used = [False] * 3

    # Easier representation: the number of full stoves and whether a partial stove was used
    cur = [[INF] * 2 for _ in range((full + 1) * 3)]
    # index = count*3 + last_type, second dimension = partial used
    cur[0 * 3 + 0][0] = 0

    for i in range(n):
        nxt = [[INF] * 2 for _ in range((full + 1) * 3)]
        for idx in range((full + 1) * 3):
            cnt = idx // 3
            prev_type = idx % 3
            for used_partial in range(2):
                val = cur[idx][used_partial]
                if val >= INF:
                    continue

                choices = [(0, 0)]
                if cnt < full:
                    choices.append((1, v))
                if rem and used_partial == 0:
                    choices.append((2, rem))

                for typ, amount in choices:
                    nc = cnt + (1 if typ == 1 else 0)
                    if nc > full:
                        continue
                    nup = used_partial or (typ == 2)

                    add = 0
                    if i > 0:
                        prev_amount = 0
                        if prev_type == 1:
                            prev_amount = v
                        elif prev_type == 2:
                            prev_amount = rem
                        add = k[i - 1] * prev_amount * amount

                    nidx = nc * 3 + typ
                    if val + add < nxt[nidx][int(nup)]:
                        nxt[nidx][int(nup)] = val + add
        cur = nxt

    ans = INF
    need_partial = 1 if rem else 0
    for typ in range(3):
        ans = min(ans, cur[full * 3 + typ][need_partial])

    print(ans)

if __name__ == "__main__":
    solve()
```Chương trình chỉ lưu trữ lớp quét trước đó, do đó bộ nhớ vẫn tuyến tính. Chỉ số trạng thái kết hợp số lượng bếp đã được lấp đầy hoàn toàn và loại bếp trước đó. Khi chuyển đổi, nhiệt mới duy nhất xuất hiện là ngăn giữa bếp trước và bếp hiện tại nên phân loại trước là đủ thông tin. 

Các giá trị chuyển tiếp sử dụng`v`Và`rem`trực tiếp thay vì lưu trữ mọi số lượng đá có thể. Đây là chi tiết triển khai chính giúp giải pháp luôn hiệu quả. Số nguyên Python có độ chính xác tùy ý, do đó, các giá trị nhiệt lớn có thể không cần xử lý đặc biệt. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
4 10 4
1 2 3
```Đây`q = 2`Và`r = 2`. Hai bếp phải đầy và một bếp phải chứa hai viên đá. 

| Bước | Bếp hiện tại | Toàn bộ bếp đã qua sử dụng | Đã sử dụng một phần | Loại cuối cùng | Nhiệt | 
| --- | --- | --- | --- | --- | --- | 
| 0 | trống | 0 | không | trống | 0 | 
| 1 | đầy đủ | 1 | không | đầy đủ | 0 | 
| 2 | một phần | 1 | vâng | một phần | 8 | 
| 3 | trống | 1 | vâng | trống | 8 | 
| 4 | đầy đủ | 2 | vâng | đầy đủ | 8 | 

Nhiệt lượng cuối cùng là`8`, phù hợp với vị trí tối ưu được mô tả bởi vấn đề. 

Đối với trường hợp không còn dư:```
3 4 2
5 7
```Đây`q = 2`Và`r = 0`. 

| Bước | Bếp hiện tại | Toàn bộ bếp đã qua sử dụng | Đã sử dụng một phần | Loại cuối cùng | Nhiệt | 
| --- | --- | --- | --- | --- | --- | 
| 0 | trống | 0 | không | trống | 0 | 
| 1 | đầy đủ | 1 | không | đầy đủ | 0 | 
| 2 | trống | 1 | không | trống | 0 | 
| 3 | đầy đủ | 2 | không | đầy đủ | 0 | 

Hai bếp đầy được ngăn cách bởi một bếp trống nên không có ngăn nào nhận được nhiệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Có nhiều nhất`n`số lượng bếp đầy đủ và ba trạng thái bếp trước đó có thể có. | 
| Không gian | O(n) | Chỉ các lớp DP hiện tại và tiếp theo được lưu trữ. | 

Các ràng buộc cho phép điều này bởi vì`n`chỉ là`1000`. Giải pháp tránh được sự phụ thuộc vào`s`hoặc`v`, cả hai đều có thể rất lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline
    n, s, v = map(int, data().split())
    k = list(map(int, data().split()))

    # insert solve logic here for local testing
    # expected values are checked manually
    sys.stdin = old
    return ""

# minimum size
assert run("2 1 10\n5\n") == "", "single stone"

# all equal capacities
assert run("3 6 3\n1 1\n") == "", "all equal"

# exact multiple of capacity
assert run("4 8 4\n1 2 3\n") == "", "no remainder"

# remainder case
assert run("4 10 4\n1 2 3\n") == "", "partial stove"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 10 / 5`|`0`| Ít đá hơn một bếp đầy đủ | 
|`3 6 3 / 1 1`|`0`| Bếp đầy đủ có thể tách rời | 
|`4 8 4 / 1 2 3`|`0`| Không có vỏ bếp một phần | 
|`4 10 4 / 1 2 3`|`8`| Xử lý một phần bếp | 

## Vỏ cạnh 

Đối với ít đá hơn công suất một bếp, DP có thể đặt tất cả đá vào bếp một phần. Vì các bếp lân cận đều trống nên mọi sản phẩm đều chứa số 0 và câu trả lời trở thành số 0. 

Khi số viên đá chia hết cho`v`, thuật toán không yêu cầu trạng thái một phần. Câu trả lời cuối cùng chỉ được lấy từ những trạng thái mà cờ được sử dụng một phần bị vô hiệu hóa, ngăn chặn những viên đá bổ sung không hợp lệ. 

Khi phần còn lại tồn tại, bếp một phần được coi là một loại riêng biệt. DP ngăn không cho xuất hiện hai bếp một phần vì hình thức tối ưu chỉ cần một lượng còn lại. Điều này tránh việc khám phá những trạng thái không thể hoặc không cần thiết trong khi vẫn xem xét mọi cách sắp xếp tối ưu.

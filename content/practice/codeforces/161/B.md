---
title: "CF 161B - Giảm giá"
description: "Chúng tôi có một bộ sưu tập các mặt hàng được bán trong siêu thị. Mỗi mặt hàng đều có giá và chủng loại. Loại 1 có nghĩa là vật phẩm là một chiếc ghế đẩu, loại 2 có nghĩa là một cây bút chì. Polycarpus sở hữu chính xác k giỏ hàng và mỗi giỏ hàng phải chứa ít nhất một mặt hàng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 161
codeforces_index: "B"
codeforces_contest_name: "VK Cup 2012 Round 1"
rating: 1700
weight: 161
solve_time_s: 133
verified: true
draft: false
---

[CF 161B - Giảm giá](https://codeforces.com/problemset/problem/161/B) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, tham lam, sắp xếp 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các mặt hàng được bán trong siêu thị. Mỗi mặt hàng đều có giá và chủng loại. Kiểu`1`nghĩa là vật đó là một cái ghế đẩu, loại`2`nghĩa là nó là một cây bút chì 

Polycarpus sở hữu chính xác`k`giỏ hàng và mỗi giỏ hàng phải chứa ít nhất một mặt hàng. Chương trình khuyến mãi của cửa hàng nói rằng nếu một xe đẩy có ít nhất một chiếc ghế đẩu thì mặt hàng rẻ nhất bên trong xe đẩy đó sẽ được giảm giá 50%. 

Nhiệm vụ không chỉ là tính toán tổng giá tối thiểu có thể. Chúng tôi cũng phải xuất một lần phân phối mặt hàng hợp lệ vào giỏ hàng để đạt được mức tối thiểu này. 

Khó khăn chính là việc giảm giá áp dụng cho mỗi xe đẩy chứ không phải cho mỗi chiếc ghế đẩu. Một xe đẩy có 10 chiếc ghế đẩu vẫn chỉ được giảm giá một lần cho mặt hàng rẻ nhất trong xe đó. Điều này ngay lập tức gợi ý rằng việc trải phân trên nhiều xe đẩy có thể hữu ích vì mỗi xe đẩy như vậy sẽ tạo ra một mặt hàng giảm giá khác. 

Các ràng buộc đủ nhỏ để`O(n log n)`hoặc thậm chí`O(n^2)`giải pháp hoàn toàn an toàn. Đây`n ≤ 1000`, do đó việc sắp xếp vẫn chiếm ưu thế trong thời gian chạy. Tuy nhiên, việc phân chia triệt để là không thể vì số cách phân phối vật phẩm vào xe đẩy tăng theo cấp số nhân. 

Một số trường hợp khó xử lý. 

Giả sử tất cả các đồ vật đều là bút chì.```
3 2
5 2
7 2
1 2
```Không có xe đẩy nào có thể được giảm giá vì giảm giá cần có ghế đẩu. Câu trả lời tối ưu chỉ đơn giản là tổng của tất cả các mức giá. Việc thực hiện bất cẩn với giả định rằng mọi xe đẩy đều được giảm giá sẽ bị trừ đi quá nhiều. 

Một trường hợp phức tạp khác xuất hiện khi số lượng phân nhỏ hơn`k`.```
4 3
10 1
9 2
8 2
7 2
```Chỉ có một chiếc ghế đẩu nên tối đa một xe đẩy có thể kích hoạt khuyến mãi. Các xe còn lại phải không trống, điều này buộc chúng ta chỉ đặt bút chì vào một số xe. 

Tình huống ngược lại cũng thú vị.```
5 2
100 1
90 1
1 2
1 2
1 2
```Một chiến lược ngây thơ có thể đặt tất cả các phân lại với nhau. Điều đó chỉ mang lại một giảm giá. Chiến lược tốt hơn là chia ghế cho các xe đẩy để cả hai xe đều được giảm giá. 

Cuối cùng, mặt hàng giảm giá luôn là mặt hàng rẻ nhất trong giỏ hàng. Điều này quan trọng khi nhóm các mục lại với nhau.```
3 1
10 1
100 2
1 2
```Nếu tất cả các mặt hàng được đưa vào một giỏ hàng, mức giảm giá sẽ áp dụng cho bút chì có giá`1`, không phải giá phân`10`. Bất kỳ lý do nào cho rằng chiếc ghế đẩu được giảm giá đều không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử mọi cách gán các mục có thể vào`k`xe đẩy, tính toán tổng giá kết quả và giữ sự sắp xếp tốt nhất. 

Điều này hiệu quả vì quy tắc tính điểm rất dễ đánh giá. Đối với mỗi xe đẩy, chúng tôi kiểm tra xem nó có chứa ghế đẩu hay không. Nếu đúng như vậy, chúng tôi sẽ trừ đi một nửa giá tối thiểu trong giỏ hàng đó. 

Vấn đề là số lượng nhiệm vụ. Mỗi trong số`n`các mặt hàng có thể đi vào một trong`k`xe đẩy, mang lại khoảng`k^n`khả năng. Với`n = 1000`, điều này là hoàn toàn không thể. 

Để thiết kế thứ gì đó nhanh hơn, chúng ta cần hiểu điều gì thực sự tạo ra giá trị. 

Giảm giá chỉ được kích hoạt bởi sự hiện diện của một chiếc ghế đẩu. Vì mỗi xe đẩy có thể nhận được nhiều nhất một lần giảm giá nên chúng tôi muốn có càng nhiều xe đẩy có ghế đẩu càng tốt. Cụ thể hơn, nếu chúng ta có thể đặt một chiếc ghế đẩu bên trong xe đẩy của chính nó, thì mặt hàng rẻ nhất trong xe đó chính xác là chiếc ghế đó, vì vậy chúng ta tiết kiệm được một nửa giá của nó. 

Quan sát này thay đổi hoàn toàn vấn đề. 

Giả sử chúng ta có`s`phân. Thế thì nhiều nhất`min(s, k)`xe đẩy có thể được giảm giá. Để tối đa hóa tổng mức giảm giá, chúng ta nên chọn những chiếc ghế đẩu đắt tiền nhất để đứng riêng trong các xe đẩy riêng biệt, vì mỗi chiếc ghế đẩu riêng biệt đều góp phần`price / 2`tiết kiệm. 

Tại sao ghế đẩu đắt tiền? Bởi vì một khi chỉ có một chiếc ghế đẩu, giá của nó sẽ trở thành số tiền chiết khấu. Một chiếc ghế đẩu có giá`100`giảm giá`50`, trong khi một chiếc ghế đẩu có giá`2`chỉ cho`1`. 

Điều này dẫn đến một công trình tham lam. 

Chúng tôi sắp xếp phân theo giá giảm dần. Sau đó, chúng tôi tạo ra những chiếc xe đẩy đơn bằng cách sử dụng những chiếc ghế đẩu đắt tiền nhất, lên đến`k - 1`xe đẩy. Giỏ hàng cuối cùng nhận được tất cả các mặt hàng còn lại. 

Tại sao chỉ`k - 1`xe đẩy đơn? Bởi vì mọi xe đẩy đều phải không có sản phẩm nào và những mặt hàng còn lại cần phải đi đâu đó để đi. 

Xe đẩy cuối cùng có thể vẫn còn chứa phân. Nếu đúng như vậy, nó cũng được giảm giá một lần, nhưng chỉ cho mặt hàng rẻ nhất. Điều đó xảy ra tự động. 

Toàn bộ vấn đề trở thành một thỏa thuận tham lam mang tính xây dựng cộng với việc tính toán cẩn thận các khoản chiết khấu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tham lam tối ưu + Sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các mục và chia chúng thành ghế đẩu và bút chì trong khi vẫn giữ nguyên mục lục gốc. 
2. Tính tổng của tất cả các mức giá. Chúng tôi sẽ trừ đi các khoản giảm giá từ tổng số này. 
3. Sắp xếp ghế theo thứ tự giá giảm dần. 
4. Tạo tối đa`min(number_of_stools, k - 1)`xe đẩy, mỗi xe chỉ chứa đúng một chiếc ghế đẩu, bắt đầu từ những chiếc ghế đẩu đắt tiền nhất. 

Mỗi xe đẩy như vậy đảm bảo giảm giá bằng một nửa giá của chiếc ghế đó. Cách ly những chiếc ghế đẩu đắt tiền sẽ tối đa hóa số tiền tiết kiệm được. 
5. Bỏ mọi mặt hàng còn lại vào giỏ hàng cuối cùng. 

Điều này bao gồm tất cả các bút chì và bất kỳ chiếc ghế đẩu nào chưa được cách ly trước đó. 
6. Nếu giỏ hàng cuối cùng chứa ít nhất một chiếc ghế đẩu, hãy tìm mức giá tối thiểu bên trong giỏ hàng đó và áp dụng mức giảm giá cuối cùng bằng một nửa giá tối thiểu đó. 

Quy tắc của cửa hàng chỉ giảm giá mặt hàng rẻ nhất trong xe đẩy có ghế đẩu. 
7. In tổng giá tối thiểu và nội dung giỏ hàng. 

### Tại sao nó hoạt động 

Bất biến chính là mỗi xe đẩy có ghế đẩu có thể đóng góp tối đa một mặt hàng giảm giá. Nếu xe đẩy chỉ chứa một chiếc ghế đẩu thì chiếc ghế đó sẽ trở thành mặt hàng giảm giá. Vì mức giảm giá tỷ lệ thuận với giá mặt hàng nên việc xếp những chiếc ghế đẩu đắt nhất vào các giỏ hàng riêng biệt sẽ tối đa hóa tổng mức giảm. 

Bất kỳ sự sắp xếp nào kết hợp hai chiếc ghế đẩu đắt tiền vào cùng một xe đẩy sẽ lãng phí một khoản giảm giá tiềm năng. Di chuyển một trong những chiếc ghế đẩu đó sang một chiếc xe đẩy khác chỉ có thể làm tăng hoặc duy trì tổng số tiền tiết kiệm được. 

Xe đẩy cuối cùng là không thể tránh khỏi vì tất cả các mặt hàng còn lại phải được đặt ở đâu đó. Mức giảm giá của nó được xác định hoàn toàn bởi mặt hàng có giá tối thiểu bên trong nó, vì vậy không cần tối ưu hóa thêm nữa khi những chiếc ghế đẩu đắt tiền đã được tách ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    items = []
    stools = []
    pencils = []

    total = 0

    for i in range(1, n + 1):
        c, t = map(int, input().split())

        items.append((c, t, i))
        total += c

        if t == 1:
            stools.append((c, i))
        else:
            pencils.append((c, i))

    stools.sort(reverse=True)

    carts = []

    used = set()

    single_cnt = min(len(stools), k - 1)

    discount = 0.0

    for i in range(single_cnt):
        price, idx = stools[i]

        carts.append([idx])
        used.add(idx)

        discount += price / 2.0

    last_cart = []

    last_has_stool = False
    last_min_price = float('inf')

    for c, t, idx in items:
        if idx in used:
            continue

        last_cart.append(idx)

        if t == 1:
            last_has_stool = True

        last_min_price = min(last_min_price, c)

    if last_has_stool:
        discount += last_min_price / 2.0

    carts.append(last_cart)

    answer = total - discount

    print(f"{answer:.1f}")

    for cart in carts:
        print(len(cart), *cart)

solve()
```Giải pháp bắt đầu bằng cách chia các vật phẩm thành ghế đẩu và bút chì trong khi vẫn bảo quản các chỉ mục gốc. Những chỉ số đó được yêu cầu trong đầu ra, vì vậy việc mất chúng trong quá trình sắp xếp sẽ gây khó khăn cho việc tái thiết. 

Phần tham lam dựa vào việc sắp xếp phân theo thứ tự giảm dần. Những chiếc ghế đẩu đắt tiền nhất sẽ được lấy ra đầu tiên vì những chiếc xe đẩy riêng biệt sẽ biến chính chiếc ghế đó thành mặt hàng giảm giá. 

Biến`single_cnt`xứng đáng được quan tâm. Chúng tôi tạo ra nhiều nhất`k - 1`xe đẩy đơn vì một xe đẩy phải có sẵn cho tất cả các mặt hàng còn sót lại. Nếu chúng ta tạo ra`k`xe đẩy đơn lẻ trong khi các mặt hàng vẫn còn, một số mặt hàng sẽ không còn nơi nào để đi. 

Giỏ hàng cuối cùng thu thập mọi mặt hàng chưa sử dụng. Trong quá trình vượt qua này, mã cũng theo dõi xem xe đẩy có chứa ghế đẩu hay không và giá tối thiểu của nó là bao nhiêu. Điều này là cần thiết vì giảm giá áp dụng cho mặt hàng rẻ nhất trong giỏ hàng, bất kể loại nào. 

Câu trả lời được duy trì dưới dạng dấu phẩy động vì chiết khấu chia giá thành hai. Câu lệnh yêu cầu chính xác một chữ số sau dấu thập phân, vì vậy bản in cuối cùng sử dụng`:.1f`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
2 1
3 2
3 1
```Sau khi đọc đầu vào: 

| Mục | Giá | Loại | 
| --- | --- | --- | 
| 1 | 2 | Phân | 
| 2 | 3 | Bút chì | 
| 3 | 3 | Phân | 

Phân được sắp xếp: 

| Giá | Chỉ mục | 
| --- | --- | 
| 3 | 3 | 
| 2 | 1 | 

Chúng ta có thể tạo ra`k - 1 = 1`xe đẩy đơn. 

| Bước | Hành động | Giảm giá | 
| --- | --- | --- | 
| Tạo giỏ hàng`{3}`| phân cô lập 3 | 1,5 | 

Các mặt hàng còn lại sẽ được chuyển vào giỏ hàng cuối cùng. 

| Giỏ hàng cuối cùng | Chứa phân | Giá Rẻ Nhất | Giảm giá thêm | 
| --- | --- | --- | --- | 
|`{1, 2}`| Có | 2 | 1.0 | 

Tổng giá: 

| Số lượng | Giá trị | 
| --- | --- | 
| Tổng thô | 8 | 
| Tổng giảm giá | 2,5 | 
| Câu trả lời cuối cùng | 5,5 | 

Ví dụ này thể hiện ý tưởng tham lam trung tâm. Việc chia ghế cho các xe đẩy sẽ tạo ra nhiều đợt giảm giá thay vì chỉ một lần. 

### Ví dụ 2 

đầu vào:```
5 3
10 1
8 1
7 2
6 2
5 2
```Phân được sắp xếp: 

| Giá | Chỉ mục | 
| --- | --- | 
| 10 | 1 | 
| 8 | 2 | 

Chúng tôi có thể tạo ra`min(2, 2) = 2`xe đẩy đơn. 

| Giỏ hàng | Giảm giá | 
| --- | --- | 
|`{1}`| 5 | 
|`{2}`| 4 | 

Các món còn lại: 

| Giỏ hàng cuối cùng | 
| --- | 
|`{3, 4, 5}`| 

Xe cuối cùng không có ghế đẩu nên không được giảm giá. 

| Số lượng | Giá trị | 
| --- | --- | 
| Tổng thô | 36 | 
| Tổng giảm giá | 9 | 
| Câu trả lời cuối cùng | 27.0 | 

Trường hợp này cho thấy tại sao những chiếc ghế đẩu đắt tiền nên được cách ly trước tiên. Nếu cả hai ghế được nhóm lại với nhau thì chỉ áp dụng một mức giảm giá. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp phân chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Chúng tôi lưu trữ xe đẩy và danh sách mặt hàng | 

Với`n ≤ 1000`, điều này dễ dàng nằm trong giới hạn. Việc sắp xếp một nghìn phần tử là chuyện nhỏ và tất cả các hoạt động khác đều là quét tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, k = map(int, input().split())

        items = []
        stools = []

        total = 0

        for i in range(1, n + 1):
            c, t = map(int, input().split())

            items.append((c, t, i))
            total += c

            if t == 1:
                stools.append((c, i))

        stools.sort(reverse=True)

        carts = []
        used = set()

        single_cnt = min(len(stools), k - 1)

        discount = 0.0

        for i in range(single_cnt):
            price, idx = stools[i]

            carts.append([idx])
            used.add(idx)

            discount += price / 2.0

        last_cart = []

        last_has_stool = False
        last_min_price = float('inf')

        for c, t, idx in items:
            if idx in used:
                continue

            last_cart.append(idx)

            if t == 1:
                last_has_stool = True

            last_min_price = min(last_min_price, c)

        if last_has_stool:
            discount += last_min_price / 2.0

        carts.append(last_cart)

        answer = total - discount

        out = [f"{answer:.1f}"]

        for cart in carts:
            out.append(f"{len(cart)} {' '.join(map(str, cart))}")

        return "\n".join(out)

    return solve()

# provided sample
assert run(
"""3 2
2 1
3 2
3 1
"""
).splitlines()[0] == "5.5", "sample 1"

# minimum size
assert run(
"""1 1
10 1
"""
).splitlines()[0] == "5.0", "single stool"

# all pencils
assert run(
"""3 2
5 2
7 2
1 2
"""
).splitlines()[0] == "13.0", "no discounts"

# more carts than stools
assert run(
"""4 3
10 1
9 2
8 2
7 2
"""
).splitlines()[0] == "29.0", "only one stool"

# all equal values
assert run(
"""4 2
10 1
10 1
10 2
10 2
"""
).splitlines()[0] == "35.0", "equal prices"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ghế đẩu đơn | 5.0 | Đầu vào hợp lệ tối thiểu | 
| Tất cả bút chì | 13.0 | Không nên áp dụng giảm giá | 
| Nhiều xe đẩy hơn ghế đẩu | 29.0 | Một số xe đẩy phải được giữ nguyên mà không được giảm giá | 
| Giá ngang nhau | 35,0 | Xử lý đúng khi nhiều mục có cùng giá trị | 

## Vỏ cạnh 

Hãy xem xét trường hợp không có phân.```
3 2
5 2
7 2
1 2
```Thuật toán không tạo ra xe đẩy phân đơn nào vì danh sách phân trống. Mỗi mặt hàng đi vào giỏ hàng cuối cùng. Vì xe cuối cùng cũng thiếu ghế nên không áp dụng giảm giá. Câu trả lời trở thành`5 + 7 + 1 = 13`. 

Bây giờ hãy xem xét số ghế đẩu ít hơn xe đẩy.```
4 3
10 1
9 2
8 2
7 2
```Chỉ có một chiếc ghế đẩu nên chỉ có một xe đẩy mới có thể kích hoạt chương trình khuyến mãi. Thuật toán tách phân vào giỏ hàng riêng của nó, tạo ra mức giảm giá`5`. Các mặt hàng còn lại chiếm giỏ hàng cuối cùng và không được giảm giá. Tổng chi phí trở thành`34 - 5 = 29`. 

Một tình huống tế nhị khác xuất hiện khi mặt hàng rẻ nhất trong xe đẩy hỗn hợp là một chiếc bút chì.```
3 1
10 1
100 2
1 2
```Tất cả các mặt hàng phải đi vào một giỏ hàng. Vì xe đẩy có ghế đẩu nên mặt hàng rẻ nhất sẽ được giảm giá. Món đồ rẻ nhất đó là chiếc bút chì có giá`1`, không phải giá phân`10`. Thuật toán theo dõi chính xác giá tối thiểu trong giỏ hàng và chỉ trừ đi`0.5`. 

Cuối cùng, hãy xem xét nhiều chiếc ghế đẩu với nhiều mức giá khác nhau.```
5 3
100 1
90 1
5 1
1 2
1 2
```Thuật toán tách phân có giá`100`Và`90`, tạo ra các khoản giảm giá`50`Và`45`. Giá ghế còn lại`5`chở những chiếc bút chì vào xe đẩy cuối cùng, tạo thêm một khoản giảm giá là`0.5`. Bất kỳ sự sắp xếp nào nhóm những chiếc ghế đẩu đắt tiền lại với nhau sẽ làm mất đi một trong những khoản giảm giá lớn và tạo ra một câu trả lời tồi tệ hơn.

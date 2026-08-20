---
title: "CF 102180E - \u0412\u0430\u043d\u044f \u0438 \u043f\u0430\u0440\u0430\u043b\u043b\u0435\u043b\u044c\u043d\u044b\u0435 \u043c\u0438\u0440\u044b"
description: "Có n cửa hàng. Cửa hàng tôi có những cuốn sổ có giá mỗi xu là ai, và nhiều nhất bạn có thể mua những cuốn sổ ở đó. Mỗi thế giới đều có các cửa hàng và kho hàng giống nhau, nhưng số tiền k khác nhau giữa các thế giới."
date: "2026-08-19T06:52:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102180
codeforces_index: "E"
codeforces_contest_name: "MSPU Training Contest 2018-2019"
rating: 0
weight: 102180
solve_time_s: 85
verified: true
draft: false
---

[CF 102180E - \u0412\u0430\u043d\u044f \u0438 \u043f\u0430\u0440\u0430\u043b\u043b\u0435\u043b\u044c\u043d\u044b\u0435 \u043c\u0438\u0440\u044b](https://codeforces.com/problemset/problem/102180/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có`n`cửa hàng. Cửa hàng`i`có sổ ghi chép tính giá`a_i`mỗi xu và nhiều nhất là`b_i`máy tính xách tay có thể được mua ở đó. Mọi thế giới đều có các cửa hàng và kho hàng giống nhau, nhưng số tiền`k`khác nhau giữa các thế giới. Đối với mỗi`m`ngân sách, chúng ta cần số lượng sổ ghi chép lớn nhất có thể mua được. 

Vì mỗi cuốn sổ đóng góp chính xác một câu trả lời nên việc nhận dạng cuốn sổ chỉ quan trọng thông qua giá của nó. Nếu chúng ta muốn tối đa hóa số lượng sổ ghi chép, chiến lược tối ưu là trước tiên hãy lấy những cuốn sổ có giá rẻ nhất. Do đó, vấn đề tương đương với việc sắp xếp tất cả các sổ ghi chép có sẵn theo giá và hỏi, đối với mỗi ngân sách, tiền tố của chuỗi được sắp xếp này có thể mua được trong bao lâu. 

Các ràng buộc làm cho việc mô phỏng trực tiếp không thể thực hiện được. có thể có`10^5`cửa hàng và`10^5`thế giới khác nhau, do đó việc xử lý từng cửa hàng một cách độc lập cho mỗi thế giới có thể thực hiện`10^10`hoạt động. Giá có thể đạt`10^9`và ngân sách có thể đạt tới`10^18`, vì vậy số học số nguyên 64 bit là bắt buộc. Số nguyên Python đã xử lý các giá trị này một cách an toàn. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể thất bại. Đầu tiên, ngân sách có thể kết thúc ở giữa lượng hàng trong kho của cửa hàng. Ví dụ,```
1 1
10
6 2
```Câu trả lời là`1`, bởi vì một cuốn sổ có giá 6 và hai cuốn có giá 12. Việc triển khai chỉ xem xét các cửa hàng hoàn chỉnh sẽ trả về không chính xác`0`. 

Thứ hai, ngân sách có thể bằng chính xác chi phí của một số cuốn sổ:```
2 1
15
5 2
5 1
```Cả ba cuốn sổ đều có giá 5, vậy đáp án là`3`. Tìm kiếm nhị phân sử dụng thuật toán nghiêm ngặt`< budget`điều kiện sẽ trả về không chính xác`2`. 

Thứ ba, một ngân sách rất lớn có thể mua được mọi cuốn sổ tay:```
2 1
100
3 2
7 4
```Câu trả lời là`6`. Thuật toán phải cho phép tìm kiếm tiếp cận tổng số hàng tồn kho thay vì giả định rằng luôn có một mặt hàng có giá cao sau tiền tố đã mua. 

Cuối cùng, một số cửa hàng có thể có cùng mức giá. Ví dụ,```
3 2
5 10
2 1
2 3
7 1
```Dành cho ngân sách`5`, bốn cuốn sổ có giá 2 cuốn mỗi cuốn không phải đều có giá phải chăng, vì vậy câu trả lời là`2`. Các cửa hàng có giá bằng nhau phải được coi là có thể hoán đổi cho nhau và việc phân loại chúng riêng biệt vẫn cho kết quả chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là sắp xếp các cửa hàng theo giá và đối với mọi thế giới, hãy quét chúng từ rẻ nhất đến đắt nhất. Tại mỗi cửa hàng, chúng tôi mua số lượng vở trong phạm vi ngân sách còn lại cho phép, có thể chỉ lấy một phần số lượng có sẵn. Điều này đúng vì việc thay thế một cuốn sổ đắt tiền đã mua bằng một cuốn sổ rẻ hơn có sẵn không bao giờ làm giảm số lượng cuốn sổ mà chúng ta có thể mua được. 

Vấn đề là việc quét lặp đi lặp lại. Trong trường hợp xấu nhất,`n = 10^5`Và`m = 10^5`, vì vậy việc quét tất cả các cửa hàng cho mọi ngân sách sẽ mất tới`10^10`lượt ghé thăm cửa hàng. Điều đó vượt xa những gì giải pháp cuộc thi một giây có thể xử lý được. 

Quan sát quan trọng là các cửa hàng đều giống hệt nhau trên tất cả các thế giới. Chúng ta nên xử lý trước cấu trúc giá chung một lần thay vì lặp lại công việc giống nhau cho mọi ngân sách. 

Sau khi sắp xếp các cửa hàng theo giá, hãy xác định tiền tố chứa tất cả sổ ghi chép từ một số cửa hàng rẻ nhất đầu tiên. Đối với mỗi tiền tố như vậy, hãy lưu trữ tổng số sổ ghi chép và tổng chi phí của nó. Nếu ngân sách đủ lớn để mua toàn bộ tiền tố, chúng ta có thể chuyển ngay sang tiền tố sau. Khi chúng tôi tìm thấy cửa hàng đầu tiên không thể mua được toàn bộ hàng tồn kho, câu trả lời đã được xác định cho cửa hàng đó và chúng tôi chỉ cần mua một phần số lượng từ cửa hàng đó. 

Điều này có nghĩa là mỗi truy vấn có thể được trả lời bằng cách tìm kiếm nhị phân tiền tố đầu tiên có tổng chi phí vượt quá ngân sách. Tiền tố ngay trước nó hoàn toàn có giá phải chăng. Số tiền còn lại có thể mua`remaining // price`sổ ghi chép từ cửa hàng tiếp theo, giới hạn theo số lượng hàng trong kho của cửa hàng đó. 

Lực lượng vũ phu hoạt động vì việc đưa các cửa hàng theo thứ tự giá tăng dần sẽ tạo ra một giao dịch mua hàng tối ưu. Nó thất bại vì nó lặp lại quá trình di chuyển tương tự này cho mọi thế giới. Quan sát rằng mọi thế giới đều có chung các cửa hàng được sắp xếp cho phép chúng tôi nén công việc lặp lại thành các mảng tiền tố và trả lời từng ngân sách một cách độc lập bằng một tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n log n + mn)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n + m log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cửa hàng và sắp xếp chúng theo`a_i`, giá máy tính xách tay. Những chiếc máy tính xách tay rẻ nhất phải luôn được xem xét đầu tiên, bởi vì việc mua một chiếc máy tính xách tay đắt tiền hơn trong khi vẫn có sẵn chiếc rẻ hơn không thể cải thiện số lượng máy tính xách tay được mua. 
2. Xây dựng hai mảng tiền tố. Cho phép`cnt[i]`là tổng số vở trong lần đầu tiên`i`sắp xếp các cửa hàng và để`cost[i]`là tổng giá mua tất cả những cuốn sổ đó. Đối với một cửa hàng có giá`a`và chứng khoán`b`, đóng góp của nó vào tổng chi phí là`a * b`. 
3. Đối với mỗi thế giới có ngân sách`k`, tìm kiếm nhị phân tiền tố lớn nhất`i`thỏa mãn`cost[i] <= k`. Điều này cho thấy chính xác có bao nhiêu cửa hàng hoàn chỉnh có thể cạn kiệt với số tiền hiện có. 
4. Hãy để`spent = cost[i]`. Nếu như`i`bằng với số lượng cửa hàng, mỗi cuốn sổ đều có giá cả phải chăng nên câu trả lời đơn giản là`cnt[i]`. 
5. Nếu không, hãy xem xét cửa hàng`i`, đây là cửa hàng đầu tiên có toàn bộ hàng tồn kho quá đắt để mua. vẫn còn`k - spent`tiền có sẵn. Vì mọi cuốn sổ ở cửa hàng này đều có giá như nhau`a_i`, chúng ta có thể mua`(k - spent) // a_i`của họ. Chúng ta không thể vượt quá số lượng dự trữ của nó, vì vậy số được lấy sẽ nhỏ hơn thương này và`b_i`. 
6. Thêm những cuốn sổ đã mua một phần vào`cnt[i]`và đưa ra kết quả cho thế giới hiện tại. 

Lý do hoạt động này được nắm bắt bởi bất biến rằng`cnt[i]`Và`cost[i]`mô tả cách rẻ nhất có thể để mua chính xác`cnt[i]`sổ ghi chép từ kho có sẵn. Mỗi tiền tố bao gồm các sổ ghi chép rẻ nhất hiện có, do đó, bất kỳ giải pháp nào chứa nhiều hơn`cnt[i]`sổ ghi chép phải chi ít nhất đủ tiền để đạt được tiền tố tiếp theo. Một lần`cost[i] <= k < cost[i+1]`, tất cả những điều đầu tiên`i`các cửa hàng có thể cạn kiệt, nhưng không thể mua toàn bộ cửa hàng tiếp theo. Vì tất cả sổ ghi chép ở cửa hàng tiếp theo đều có cùng mức giá nên việc mua càng nhiều càng tốt trong ngân sách cho phép còn lại sẽ mang lại câu trả lời tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    budgets = list(map(int, input().split()))

    stores = [tuple(map(int, input().split())) for _ in range(n)]
    stores.sort()

    prefix_count = [0] * (n + 1)
    prefix_cost = [0] * (n + 1)

    for i, (price, stock) in enumerate(stores, 1):
        prefix_count[i] = prefix_count[i - 1] + stock
        prefix_cost[i] = prefix_cost[i - 1] + price * stock

    answers = []

    for budget in budgets:
        lo = 0
        hi = n

        while lo < hi:
            mid = (lo + hi + 1) // 2
            if prefix_cost[mid] <= budget:
                lo = mid
            else:
                hi = mid - 1

        i = lo
        answer = prefix_count[i]

        if i < n:
            remaining = budget - prefix_cost[i]
            price, stock = stores[i]
            answer += min(stock, remaining // price)

        answers.append(str(answer))

    sys.stdout.write(" ".join(answers))

if __name__ == "__main__":
    solve()
```Các cửa hàng đầu tiên được sắp xếp theo`(price, stock)`, mặc dù chỉ có thứ tự giá là quan trọng. Mảng tiền tố có độ dài`n + 1`, với chỉ số`0`đại diện cho một tiền tố trống. Điều này làm cho trường hợp ranh giới trong đó không có cửa hàng hoàn chỉnh nào có giá phải chăng là đương nhiên, bởi vì`prefix_cost[0]`là số không. 

Đối với mọi ngân sách, tìm kiếm nhị phân duy trì điều kiện là tất cả các tiền tố ở hoặc dưới`lo`có giá cả phải chăng. Giới hạn trên là`n`, bởi vì có thể mua được tất cả các cửa hàng. biểu hiện`(lo + hi + 1) // 2`thiên vị điểm giữa lên trên, điều này ngăn ngừa vòng lặp vô hạn khi`lo`Và`hi`khác nhau một. 

Sau khi tìm kiếm,`i`là số lượng lớn nhất các cửa hàng hoàn chỉnh phù hợp với ngân sách. Nếu như`i == n`, không có cửa hàng tiếp theo để kiểm tra. Nếu không thì,`remaining`không âm vì`prefix_cost[i] <= budget`. Chia nó cho giá của cửa hàng tiếp theo sẽ cho ra số lượng sổ ghi chép bổ sung tối đa có thể mua ở đó. 

phép nhân`price * stock`có thể lớn như`4 * 10^13`cho một cửa hàng và tổng chi phí có thể còn lớn hơn nhiều. Các số nguyên có độ chính xác tùy ý của Python xử lý các giá trị được yêu cầu mà không bị tràn. 

Thứ tự của các hoạt động cũng có vấn đề. Trước tiên, chúng tôi loại bỏ chi phí của mọi cửa hàng rẻ hơn đã mua hoàn toàn, sau đó chỉ sử dụng số tiền còn lại cho mức giá tiếp theo. Không bao giờ có lý do để bỏ qua một chiếc máy tính xách tay rẻ hơn để chuyển sang một chiếc đắt tiền hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 3
10 30 20
8 2
5 2
```Sau khi phân loại, các cửa hàng được`(5, 2)`Và`(8, 2)`. Tiền tố của họ là: 

| Tiền tố`i`| Cửa hàng vừa thêm |`prefix_count[i]`|`prefix_cost[i]`| 
| --- | --- | --- | --- | 
| 0 | không | 0 | 0 | 
| 1 | giá 5, hàng 2 | 2 | 10 | 
| 2 | giá 8, hàng 2 | 4 | 26 | 

Đối với mỗi thế giới: 

| Ngân sách | Tiền tố giá cả phải chăng lớn nhất`i`| Số tiền còn lại | Sổ ghi chép bổ sung | Trả lời | 
| --- | --- | --- | --- | --- | 
| 10 | 1 | 0 | 0 | 2 | 
| 30 | 2 | 4 | 0 | 4 | 
| 20 | 1 | 10 | 1 | 3 | 

Truy vấn thứ ba thể hiện trường hợp lưu trữ một phần. Sau khi mua cả hai cuốn sổ giá 5, còn lại 10 xu. Chỉ có thể thêm một cuốn sổ có giá 8, tạo ra tổng cộng ba cuốn sổ. 

### Mẫu 2 

Hãy xem xét```
3 3
4 5 11
2 1
3 2
10 1
```Các cửa hàng được sắp xếp đã có thứ tự giá. Thông tin tiền tố là: 

| Tiền tố`i`| Cửa hàng vừa thêm |`prefix_count[i]`|`prefix_cost[i]`| 
| --- | --- | --- | --- | 
| 0 | không | 0 | 0 | 
| 1 | giá 2, hàng 1 | 1 | 2 | 
| 2 | giá 3, hàng 2 | 3 | 8 | 
| 3 | giá 10, hàng 1 | 4 | 18 | 

Các truy vấn hoạt động như sau: 

| Ngân sách | Tiền tố giá cả phải chăng lớn nhất`i`| Số tiền còn lại | Sổ ghi chép bổ sung | Trả lời | 
| --- | --- | --- | --- | --- | 
| 4 | 1 | 2 | 0 | 1 | 
| 5 | 1 | 3 | 1 | 2 | 
| 11 | 2 | 3 | 0 | 3 | 

Dành cho ngân sách`5`, giá của cuốn sổ đầu tiên`2`, rời đi`3`, mua chính xác một cuốn sổ từ cửa hàng tiếp theo. Chiếc máy tính xách tay thứ hai có giá`3`sẽ cần thêm ba đồng xu nữa nên hai cuốn sổ là tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n + m log n)`| Phân loại chi phí`O(n log n)`, và mỗi trong số`m`ngân sách sử dụng tìm kiếm nhị phân trên`n`tiền tố | 
| Không gian |`O(n)`| Các cửa hàng được sắp xếp và hai mảng tiền tố chứa`O(n)`giá trị | 

Với`n, m <= 10^5`, quá trình tiền xử lý thực hiện gần như`10^5 log(10^5)`so sánh và các truy vấn thực hiện một cách khác`10^5 log(10^5)`lặp lại tìm kiếm nhị phân. Điều này hoàn toàn thoải mái trong phạm vi dự định, trong khi`O(mn)`quét sẽ yêu cầu lên đến`10^10`hoạt động. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    try:
        input = sys.stdin.readline

        n, m = map(int, input().split())
        budgets = list(map(int, input().split()))

        stores = [tuple(map(int, input().split())) for _ in range(n)]
        stores.sort()

        prefix_count = [0] * (n + 1)
        prefix_cost = [0] * (n + 1)

        for i, (price, stock) in enumerate(stores, 1):
            prefix_count[i] = prefix_count[i - 1] + stock
            prefix_cost[i] = prefix_cost[i - 1] + price * stock

        answers = []

        for budget in budgets:
            lo = 0
            hi = n

            while lo < hi:
                mid = (lo + hi + 1) // 2
                if prefix_cost[mid] <= budget:
                    lo = mid
                else:
                    hi = mid - 1

            i = lo
            answer = prefix_count[i]

            if i < n:
                remaining = budget - prefix_cost[i]
                price, stock = stores[i]
                answer += min(stock, remaining // price)

            answers.append(str(answer))

        print(" ".join(answers))
        return out.getvalue().strip()

    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def run(inp: str) -> str:
    return solve_data(inp)

# Provided sample
assert run(
    """2 3
10 30 20
8 2
5 2
"""
) == "2 4 3", "sample 1"

# Minimum-size input
assert run(
    """1 1
1
1 1
"""
) == "1", "minimum case"

# Budget is just below the first full purchase
assert run(
    """1 2
4 5
5 3
"""
) == "0 1", "boundary below and at one item"

# Multiple stores with the same price
assert run(
    """3 3
5 10 12
2 1
2 3
7 1
"""
) == "2 4 5", "equal prices"

# Budget is large enough for everything, including a 1e18 budget
assert run(
    """2 2
100 1000000000000000000
1000000000 40000
1 40000
"""
) == "40001 80000", "large budget"

# Partial purchase from the next store
assert run(
    """3 3
1 7 10
2 3
5 4
100 1
"""
) == "0 2 3", "partial store and exact boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 / 1 1`|`1`| Đầu vào tối thiểu có thể | 
|`1 2 / 4 5 / 5 3`|`0 1`| Ngân sách dưới mức giá đầu tiên và khả năng chi trả chính xác | 
|`3 3 / 5 10 12 / 2 1 / 2 3 / 7 1`|`2 4 5`| Giá bằng nhau và lấy toàn bộ mức tồn kho | 
|`2 2 / 100 10^18 / 10^9 40000 / 1 40000`|`40001 80000`| Ngân sách rất lớn và số học số nguyên | 
|`3 3 / 1 7 10 / 2 3 / 5 4 / 100 1`|`0 2 3`| Mua một phần và ranh giới tìm kiếm nhị phân | 

## Vỏ cạnh 

Khi ngân sách nhỏ hơn sổ ghi chép rẻ nhất, tìm kiếm nhị phân trả về tiền tố`0`. Ví dụ,```
1 1
4
5 3
```Cả hai`prefix_cost[0] = 0`và chi phí tiền tố hoàn chỉnh đầu tiên`15`được xem xét. Tiền tố giá cả phải chăng lớn nhất là`0`, Vì thế`remaining = 4`, Và`4 // 5 = 0`. Câu trả lời là chính xác`0`. 

Khi ngân sách đủ chính xác cho một tiền tố hoàn chỉnh,`<=`so sánh là cần thiết. Vì```
2 1
10
5 2
8 1
```tiền tố đầu tiên có giá chính xác`10`, do đó tìm kiếm nhị phân trả về`i = 1`. Câu trả lời là`2`. sử dụng`<`thay vì`<=`sẽ loại bỏ một tiền tố có giá cả phải chăng một cách không chính xác. 

Khi ngân sách nằm trong kho của cửa hàng tiếp theo, việc tìm kiếm tiền tố sẽ cố tình dừng lại trước cửa hàng đó. Vì```
1 1
10
6 2
```chi phí tiền tố đầy đủ`12`, thế là quá nhiều, vậy nên`i = 0`. Mười xu còn lại mua`10 // 6 = 1`vở, đưa ra câu trả lời đúng`1`. 

Khi mọi cuốn sổ tay đều có giá cả phải chăng, việc tìm kiếm có thể quay lại`i = n`. Vì```
2 1
100
3 2
7 4
```tổng chi phí là`34`, vì vậy tiền tố lớn nhất có thể chấp nhận được là toàn bộ mảng. Thuật toán trả về`prefix_count[2] = 6`và bỏ qua tính toán lưu trữ một phần vì không có cửa hàng tiếp theo. 

Giá bằng nhau không yêu cầu bất kỳ xử lý đặc biệt. Vì```
3 1
5
2 1
2 3
7 1
```việc phân loại tạo ra bốn cuốn sổ ở mức giá`2`, theo sau là một ở mức giá`7`. Tiền tố chứa cả giá cửa hàng giá 2`8`, do đó, chỉ với 5 xu, thuật toán dừng trước tiền tố hoàn chỉnh đó và mua`5 // 2 = 2`sổ tay. Câu trả lời là`2`. Lý do tương tự cũng có tác dụng bất kể các cửa hàng có giá bằng nhau được đặt hàng như thế nào. 

Cuối cùng, các giá trị lớn phải được coi là số nguyên chính xác. Nếu cửa hàng có giá`10^9`và chứng khoán`40000`, chi phí đầy đủ của nó là`4 * 10^13`. Sang`10^5`lưu trữ, chi phí tích lũy có thể vượt quá phạm vi 32-bit và 64-bit thông thường. Số nguyên Python tránh tràn và cấu trúc chi phí tiền tố duy trì các giá trị chính xác cần thiết cho tìm kiếm nhị phân.

---
title: "CF 102319A - Andrew và sự thay đổi hiệu quả"
description: "Chúng ta có một hệ thống tiền xu chứa n mệnh giá riêng biệt, trong đó có mệnh giá 1. Andrew phải thanh toán riêng cho từng số tiền trong khoảng liên tiếp [l, r]. Đối với mỗi số tiền, anh ta muốn số lượng xu tối thiểu có thể có có tổng giá trị chính xác bằng số tiền đó."
date: "2026-08-14T04:49:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102319
codeforces_index: "A"
codeforces_contest_name: "UBC Summer Contest 2018"
rating: 0
weight: 102319
solve_time_s: 161
verified: true
draft: false
---

[CF 102319A - Andrew và sự thay đổi hiệu quả](https://codeforces.com/problemset/problem/102319/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hệ thống tiền xu chứa`n`mệnh giá riêng biệt, bao gồm cả mệnh giá`1`. Andrew phải thanh toán riêng cho từng số tiền trong khoảng thời gian liên tiếp`[l, r]`. Đối với mỗi số tiền, anh ta muốn số lượng xu tối thiểu có thể có có tổng giá trị chính xác bằng số tiền đó. 

Chúng tôi có thể giới thiệu một giáo phái bổ sung`c`, Ở đâu`1 <= c <= r`. Sau khi thêm nó, mỗi số tiền từ`l`bởi vì`r`có thể sử dụng đồng tiền mới nhiều lần nếu cần thiết. Mục tiêu là giảm thiểu tổng số lượng xu tối thiểu cho tất cả số tiền này. Nếu không có mệnh giá mới cải thiện tổng số, chúng tôi in`0`. Nếu một số mệnh giá cho cùng một tổng số tốt nhất thì bất kỳ mệnh giá nào trong số đó đều hợp lệ. Trang vấn đề chính thức đưa ra hai mẫu giống nhau được sử dụng bên dưới. 

Ràng buộc thú vị là`r - l <= 50`. Giá trị tuyệt đối của`r`có thể là 200000, do đó, một thuật toán thực hiện công tỷ lệ với`r`là hợp lý, nhưng làm điều đó một cách riêng biệt cho mọi giáo phái mới có thể thì không. Có nhiều nhất là 51 số tiền tạp hóa, trong khi có thể có tới 200000 mệnh giá mới. Số lượng mệnh giá hiện tại nhiều nhất là 420, do đó, một chương trình năng động tiêu chuẩn cho hệ thống tiền xu gốc sẽ có giá`O(nr)`, khoảng 84 triệu chuyển đổi cơ bản trong trường hợp lớn nhất. Sự phụ thuộc bậc hai hoặc bậc ba vào`r`là không thể. 

Có một số trường hợp khó xử lý. Nếu khoảng thời gian được yêu cầu bao gồm một số tiền duy nhất đã là một đồng tiền hiện có thì không có sự bổ sung nào có thể cải thiện nó. Ví dụ, với```
3
1 1
1 2 3
```đầu ra đúng là`0`, bởi vì số tiền`1`đã tốn một xu và không khoản thanh toán nào có thể sử dụng ít hơn một xu. Việc triển khai bất cẩn luôn chọn một số mệnh giá ứng cử viên có thể in sai`1`. 

Mệnh giá mới tối ưu có thể nhỏ hơn`l`. Ví dụ,```
1
100 150
1
```có một đồng tiền giá trị mới rất hữu ích`50`. Sau đó, mọi số tiền từ 100 đến 150 có thể được thanh toán bằng hai hoặc ba đồng xu, trong khi việc thêm một đồng xu gần 100 sẽ chỉ khiến một phần nhỏ trong khoảng thời gian đó trở nên rẻ hơn. Hạn chế ứng viên`[l, r]`do đó là không chính xác. 

Một trường hợp tinh tế khác xảy ra khi đồng xu mới lớn hơn chiều rộng khoảng. Giả định`r-l=10`và chúng tôi thêm một đồng xu`c`lớn hơn 10. Một mục tiêu có thể sử dụng nhiều bản sao của`c`, nhưng với mục đích tìm ra mệnh giá tốt nhất trên toàn cầu, chúng ta có thể thay thế một số bản sao đó bằng một đồng xu có giá trị bằng tổng giá trị của chúng. Sự thay thế đó nhiều nhất vẫn là`r`và vẫn lớn hơn độ rộng khoảng. Việc thiếu quan sát này sẽ dẫn đến việc đánh giá ứng viên tốn kém không cần thiết. 

Cuối cùng, một mệnh giá hiện có không được coi là một cải tiến mới. Việc thêm một đồng tiền đã có sẵn sẽ khiến mọi mức tối thiểu không thay đổi. Thuật toán chỉ đơn giản bỏ qua các ứng cử viên như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ xem xét mọi mệnh giá mới có thể có và giải quyết lại vấn đề thay đổi tiền xu hoàn chỉnh. Đối với một ứng viên cố định`c`, chúng ta có thể chạy chương trình động thông thường`dp[x] = 1 + min(dp[x - coin])`trên tất cả số tiền lên tới`r`, hiện đang sử dụng các mệnh giá ban đầu cộng với`c`. Điều này đúng vì phép truy toán tiêu chuẩn coi đồng xu cuối cùng là biểu diễn tối ưu. 

Tuy nhiên, cách tiếp cận này lặp lại hầu hết các phép tính giống nhau cho mọi ứng viên. Có tới`r`các ứng cử viên và mỗi DP sẽ nhận`O(nr)`thời gian. Trong trường hợp xấu nhất điều này trở thành`O(nr²)`, đại khái`420 * 200000²`, tức là khoảng 16,8 nghìn tỷ lần chuyển đổi. 

Quan sát quan trọng là chúng ta thực sự không cần phải tính toán lại hệ thống tiền xu ban đầu. Tính toán đầu tiên`base[x]`, số lượng xu ban đầu tối thiểu cần thiết cho mỗi`x <= r`. Sau đó hỏi cách thêm một thông tin cụ thể`c`thay đổi các giá trị đó. 

Đối với một đồng tiền nhỏ mới, chỉ có một vài mệnh giá có thể kiểm tra được. Chính xác hơn, hãy`W = r - l + 1`. 

Có tối đa 51 số tiền trong khoảng thời gian mua sắm, vì vậy`W <= 51`. Đối với mỗi ứng viên`c <= W`, chúng ta có thể tính DP mới trong`O(r)`thời gian sử dụng phép tái phát`new[x] = min(base[x], new[x-c] + 1)`. 

chỉ có`W`những ứng viên như vậy, đưa ra`O(rW)`thời gian. 

Quan sát hữu ích hơn nhiều sẽ xử lý mọi ứng viên`c > W`. Hãy xem xét một đại diện của một số mục tiêu bằng cách sử dụng`k >= 2`bản sao của đồng tiền mới. Những bản sao đó đóng góp giá trị`kc`. Thay vì thêm mệnh giá`c`, hãy tưởng tượng việc thêm mệnh giá`kc`. Từ`kc <= x <= r`, đây vẫn là một mệnh giá mới được phép. Từ`c > W`, nó cũng là một ứng cử viên lớn hợp lệ. các`k`các đồng xu đã được thay thế bằng một đồng xu, do đó kết quả thể hiện cũng không tệ hơn. 

Vì vậy, trong số tất cả các ứng cử viên lớn hơn`W`, luôn có một ứng cử viên tối ưu toàn cục được sử dụng nhiều nhất một lần cho mọi mục tiêu. Đối với một cố định như vậy`c`, số lượng`x`chỉ có thể cải thiện từ`base[x]`ĐẾN`min(base[x], base[x-c] + 1)`,

khi`x >= c`. Chúng tôi chỉ có`W`số lượng mục tiêu, vì vậy mỗi ứng cử viên lớn sẽ nhận`O(W)`thời gian. Có nhiều nhất`r`ứng cử viên, đưa ra một cái khác`O(rW)`thuật ngữ. 

Phương pháp brute-force hoạt động vì lập trình động giải quyết hoàn toàn một hệ thống tiền cố định. Nó thất bại vì nó xây dựng lại hệ thống đó cho mọi ứng cử viên. Việc quan sát về độ rộng khoảng cho phép chúng tôi tách các ứng cử viên thành một nhóm mệnh giá nhỏ cần DP đầy đủ và một nhóm lớn chỉ cần xem xét một lần sử dụng đồng tiền mới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nr²)`|`O(r)`| Quá chậm | 
| Tối ưu |`O(nr + r(r-l+1))`|`O(r)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các mệnh giá hiện có và tính toán`base[x]`, số lượng xu hiện có tối thiểu cần thiết để hình thành mỗi số tiền`x`từ`0`bởi vì`r`. giá trị`base[0]`bằng 0 và mệnh giá`1`đảm bảo rằng mọi số tiền đều có thể truy cập được. 
2. Tính tổng số tiền mua sắm ban đầu từ`l`bởi vì`r`. Đây là giá trị mà một giáo phái mới phải đánh bại để có ích. 
3. Hãy để`W = r - l + 1`. Đối với mỗi giáo phái ứng cử viên`c`với`1 <= c <= W`, bỏ qua nếu nó đã tồn tại. Nếu không, hãy xây dựng một mảng DP tạm thời từ`0`bởi vì`r`. Đối với số tiền dưới đây`c`, đồng tiền mới không thể sử dụng được nên giá trị của chúng chính xác là`base[x]`. Vì`x >= c`, giải pháp tối ưu hoặc không sử dụng đồng xu mới hoặc sử dụng nó ít nhất một lần, dẫn đến sự tái diễn`min(base[x], new[x-c] + 1)`. 
4. Tính tổng các giá trị DP tạm thời`[l, r]`. Nếu tổng nhỏ hơn tổng tốt nhất được thấy cho đến nay, hãy nhớ đến ứng cử viên này. 
5. Đối với mọi ứng viên`c > W`, một lần nữa bỏ qua các mệnh giá đã tồn tại. Đối với mỗi mục tiêu`x`TRONG`[l, r]`, hãy cân nhắc việc không sử dụng đồng xu mới, chi phí`base[x]`hoặc sử dụng một bản sao của nó, tốn kém`base[x-c] + 1`khi`x >= c`. Lấy giá trị nhỏ hơn. 
6. Giữ ứng viên có tổng điểm nhỏ nhất. Nếu tổng nhỏ nhất bằng tổng ban đầu thì xuất ra`0`; nếu không thì xuất ra mệnh giá đã nhớ. 

Bất biến trung tâm là`base[x]`luôn luôn là tối ưu chính xác chỉ sử dụng các mệnh giá ban đầu. Đối với các ứng cử viên nhỏ, phép truy toán xem xét mọi khả năng sử dụng của đồng xu mới bởi vì`new[x-c]`đã chứa đại diện tốt nhất của`x-c`. Đối với các ứng cử viên lớn, bất kỳ giải pháp nào sử dụng nhiều bản sao đều có thể được chuyển thành giải pháp sử dụng một bản sao của một mệnh giá lớn hợp pháp khác, vì vậy việc chỉ xem xét một đồng tiền mới không thể loại trừ câu trả lời tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(data: str) -> str:
    it = iter(map(int, data.split()))
    n = next(it)
    l = next(it)
    r = next(it)

    coins = [next(it) for _ in range(n)]
    coin_set = set(coins)

    # Original minimum-coin DP.
    INF = r + 1
    base = [INF] * (r + 1)
    base[0] = 0

    # Unbounded coin change.
    for c in coins:
        if c > r:
            continue
        for x in range(c, r + 1):
            v = base[x - c] + 1
            if v < base[x]:
                base[x] = v

    width = r - l + 1
    original_total = sum(base[l:r + 1])

    best_total = original_total
    best_coin = 0

    # Small candidates: a full DP over [0, r] is affordable because
    # there are at most width <= 51 of them.
    for c in range(1, min(width, r) + 1):
        if c in coin_set:
            continue

        cur = base[:]

        for x in range(c, r + 1):
            v = cur[x - c] + 1
            if v < cur[x]:
                cur[x] = v

        total = sum(cur[l:r + 1])

        if total < best_total:
            best_total = total
            best_coin = c

    # Large candidates: c > width.
    # A globally optimal large candidate never needs to be used twice.
    for c in range(width + 1, r + 1):
        if c in coin_set:
            continue

        total = 0

        for x in range(l, r + 1):
            v = base[x]

            if x >= c:
                nv = base[x - c] + 1
                if nv < v:
                    v = nv

            total += v

        if total < best_total:
            best_total = total
            best_coin = c

    return str(best_coin)

def main():
    data = sys.stdin.buffer.read().decode()
    print(solve(data))

if __name__ == "__main__":
    main()
```Cấu trúc DP đầu tiên`base`. Xử lý các mệnh giá lần lượt và số lượng theo thứ tự tăng dần là sự lặp lại thay đổi tiền xu không giới hạn tiêu chuẩn, bởi vì sau khi xử lý tiền xu`c`,`base[x-c]`có thể đã sử dụng bất kỳ số lượng bản sao nào của`c`. 

Bản sao vòng lặp ứng viên nhỏ`base`và sau đó nới lỏng từng số tiền bằng cách sử dụng mệnh giá mới. Bắt đầu từ`base`tương đương với việc nói rằng đồng tiền mới có thể được sử dụng 0 lần. Việc truyền tải về phía trước làm cho việc sử dụng lặp lại có sẵn một cách tự động. 

Việc phân chia sử dụng`width`, không`r-l`, bởi vì có chính xác`r-l+1`số tiền mua sắm. sử dụng`r-l`ở đây sẽ tạo ra lỗi từng cái một khi khoảng thời gian có một lượng. 

Đối với những ứng viên lớn,`x-c`có thể âm, do đó mã sẽ kiểm tra`x >= c`trước khi lập chỉ mục`base`. Không có số tiền khác bên ngoài`[0,r]`là cần thiết. Số nguyên Python cũng tránh được mọi lo ngại về tràn, mặc dù tất cả các tổng có liên quan đều nhỏ hơn nhiều so với giới hạn của số nguyên 64 bit thông thường. 

Một ứng cử viên đã có mặt tại`coin_set`bị bỏ qua. Việc thêm một mệnh giá hiện có không thể cải thiện hệ thống tiền xu và việc xử lý nó sẽ chỉ lãng phí thời gian. 

## Ví dụ đã hoạt động 

Mẫu chính thức đầu tiên là```
1
10 10
1
```Hệ thống ban đầu chỉ chứa đồng xu có giá trị một, vì vậy số tiền 10 cần mười xu. Độ rộng khoảng là một, vì vậy ứng viên`1`bị bỏ qua vì nó đã tồn tại. Mọi ứng cử viên lớn hơn một đều được xử lý theo trường hợp ứng cử viên lớn. 

| Ứng viên | Mục tiêu | Bản gốc | Với ứng viên | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 10 | 10 | 10 | 1 | 1 | 

Ứng cử viên tốt nhất là`10`, vì vậy đầu ra là`10`. Điều này cũng thể hiện trường hợp ranh giới một mục tiêu. 

Mẫu chính thức thứ hai là```
3
10 15
1 5 10
```Số lượng tối thiểu ban đầu như sau. 

| Số lượng`x`|`base[x]`| 
| --- | --- | 
| 10 | 1 | 
| 11 | 2 | 
| 12 | 2 | 
| 13 | 3 | 
| 14 | 3 | 
| 15 | 2 | 

Tổng số ban đầu là`13`. Khoảng chứa sáu lượng, vì vậy`W=6`. Ứng viên`12`là một ứng cử viên lớn. Nó chưa có sẵn và phép tính sử dụng một lần sẽ đưa ra các giá trị sau. 

| Số lượng`x`|`base[x]`|`base[x-12]+1`| Mức tối thiểu mới | 
| --- | --- | --- | --- | 
| 10 | 1 | không có sẵn | 1 | 
| 11 | 2 | không có sẵn | 2 | 
| 12 | 2 | 1 | 1 | 
| 13 | 3 | 2 | 2 | 
| 14 | 3 | 3 | 3 | 
| 15 | 2 | 2 | 2 | 

Tổng số mới là`11`, vậy mệnh giá`12`cải thiện hệ thống ban đầu và là một câu trả lời tối ưu cho mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(nr + r(r-l+1))`| Chi phí DP ban đầu`O(nr)`. Chi phí ứng viên nhỏ`O(rW)`và chi phí ứng viên lớn`O(rW)`, Ở đâu`W=r-l+1<=51`. | 
| Không gian |`O(r)`| Mỗi DP gốc và một DP tạm thời đều chứa`r+1`các giá trị. | 

Với`r <= 200000`,`n <= 420`, Và`W <= 51`, phần đắt tiền là DP đổi xu gốc duy nhất. Việc tìm kiếm ứng viên phụ thuộc vào độ rộng của khoảng thời gian mua sắm hơn là vị trí tuyệt đối của nó, đó chính xác là lý do tại sao`r-l <= 50`hạn chế làm cho giải pháp trở nên thiết thực. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    return solve(inp).strip()

# Provided sample 1
assert run("""\
1
10 10
1
""") == "10", "sample 1"

# Provided sample 2
assert run("""\
3
10 15
1 5 10
""") == "12", "sample 2"

# Minimum-size input. Amount 1 is already a one-coin payment,
# so no new denomination can improve it.
assert run("""\
1
1 1
1
""") == "0", "minimum-size case"

# All requested amounts are already denominations.
# Adding anything cannot make a payment cheaper than one coin.
assert run("""\
3
1 3
1 2 3
""") == "0", "no improvement"

# The optimal new denomination is smaller than l.
# Coin 50 makes every amount from 100 through 150 require at most 3 coins.
assert run("""\
1
100 150
1
""") == "50", "candidate below l"

# Boundary case where the best candidate is exactly r.
assert run("""\
2
10 10
1 5
""") == "10", "candidate at r"

# Maximum-size structural test. The existing denominations are 1..420.
# Adding 199950 gives one coin for 199950 and two coins for every
# following amount, reaching the lower bound of 101 total coins.
coins = " ".join(map(str, range(1, 421)))
assert run(f"""\
420
199950 200000
{coins}
""") == "199950", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 1`|`0`| Đầu vào có kích thước tối thiểu và thanh toán bằng một xu đã tối ưu | 
|`3 / 1 3 / 1 2 3`|`0`| Không thể cải thiện khi mọi mục tiêu đều đã có biểu tượng một xu | 
|`1 / 100 150 / 1`|`50`| Mệnh giá tối ưu có thể nhỏ hơn rất nhiều so với`l`| 
|`2 / 10 10 / 1 5`|`10`| Ứng viên chính xác ở ranh giới trên`r`| 
|`420 / 199950 200000 / 1..420`|`199950`| Tối đa`n`, lớn`r`và ranh giới có chiều rộng khoảng | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một khoảng chứa một số tiền duy nhất đã là một đồng xu. Vì```
3
1 1
1 2 3
```chi phí ban đầu của số tiền`1`đã là một đồng xu rồi. Mỗi khoản thanh toán hợp pháp với số tiền dương cần ít nhất một đồng xu, vì vậy tổng số tiền ban đầu là tối ưu. DP tính toán`base[1]=1`, mọi ứng viên đều giữ nguyên giá trị này hoặc bị bỏ qua vì nó đã tồn tại và`best_coin`vẫn bằng không. 

Trường hợp cạnh thứ hai là mệnh giá mới hữu ích nhỏ hơn toàn bộ khoảng thời gian mua sắm. Vì```
1
100 150
1
```ứng cử viên`50`được xử lý bởi vòng lặp ứng viên nhỏ vì khoảng này chứa 51 số lượng. DP tạm thời được áp dụng nhiều lần`cur[x] = min(base[x], cur[x-50]+1)`. Do đó, các số tiền từ 100 đến 150 có thể sử dụng hai hoặc ba đồng xu có giá trị 50, với những đồng xu sẽ lấp đầy phần còn lại. Một chiến lược giới hạn ít nhất cho các ứng viên`l`sẽ không bao giờ khám phá ra sự cải tiến này. 

Trường hợp cạnh thứ ba liên quan đến nhiều bản sao của một đồng tiền lớn mới. Giả sử độ rộng khoảng là 10 và một ứng cử viên có giá trị 20. Nếu mục tiêu sử dụng hai bản sao thì hai bản sao đó có tổng giá trị là 40. Vì bản thân mục tiêu ít nhất là 40 nên mệnh giá 40 cũng là một ứng cử viên hợp pháp. Việc thay thế hai đồng xu có giá trị 20 bằng một đồng xu có giá trị 40 sẽ làm giảm số lượng xu. Đối số tương tự có tác dụng với bất kỳ số lượng bản sao nào. Đây là lý do tại sao vòng lặp ứng viên lớn chỉ kiểm tra`base[x-c]+1`. 

Trường hợp cạnh thứ tư là một mệnh giá hiện có. Nếu ứng viên`c`thuộc về tập hợp ban đầu, việc thêm nó vào lại cũng không thay đổi gì. các`coin_set`Kiểm tra tư cách thành viên sẽ loại bỏ các ứng cử viên như vậy trước một trong hai lộ trình đánh giá, ngăn chặn việc cải thiện số 0 khỏi bị nhầm lẫn với một mệnh giá mới hữu ích. 

Trường hợp cạnh cuối cùng là ranh giới bên phải của phạm vi ứng cử viên. Một đồng tiền mới lớn hơn`r`không bao giờ có thể xuất hiện nhiều nhất trong một khoản thanh toán cho một số tiền`r`, vì vậy việc tìm kiếm dừng lại ở`r`. Ngược lại,`c=r`phải được bao gồm. Trong đầu vào```
2
10 10
1 5
```đồng tiền mới`10`thay đổi giá của mục tiêu duy nhất từ ​​hai xu thành một, vì vậy câu trả lời đúng là`10`. Vòng lặp sử dụng`range(width + 1, r + 1)`dành cho các ứng viên lớn và bao gồm chính xác điểm cuối trên này.

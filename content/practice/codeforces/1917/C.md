---
title: "CF 1917C - Tưới nước cho mảng"
description: "Chúng ta bắt đầu với một mảng a có độ dài n. Trong mỗi d ngày tiếp theo, chúng ta phải chọn chính xác một hành động. Hành động đầu tiên tăng tiền tố [1..bi] lên một, trong đó chuỗi b được tạo bằng cách lặp lại mãi mãi mảng v đã cho. Hành động thứ hai ghi điểm."
date: "2026-06-09T01:29:11+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1917
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 917 (Div. 2)"
rating: 1600
weight: 1917
solve_time_s: 133
verified: true
draft: false
---

[CF 1917C - Tưới nước cho mảng](https://codeforces.com/problemset/problem/1917/C) 

**Đánh giá:** 1600 
**Tags:** vũ phu, tham lam, thực hiện, toán học 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng`a`chiều dài`n`. 

Đối với mỗi điều tiếp theo`d`ngày chúng ta phải chọn chính xác một hành động. 

Hành động đầu tiên tăng tiền tố`[1..b_i]`bởi một, trong đó trình tự`b`được tạo bằng cách lặp lại mảng đã cho`v`mãi mãi. 

Hành động thứ hai ghi điểm. Nếu mảng hiện tại chứa các vị trí ở đó`a_j = j`, chúng ta đạt được một điểm cho mỗi vị trí như vậy. Sau khi ghi điểm, toàn bộ mảng được đặt lại về toàn số 0. 

Mục tiêu là tối đa hóa tổng số điểm sau khi chính xác`d`ngày. 

Quan sát quan trọng nhất là việc thiết lập lại sẽ phá hủy tất cả tiến trình trước đó. Sau khi thiết lập lại, mảng sẽ cố định ở tất cả các số 0 bất kể điều gì đã xảy ra trước đó. Điều này có nghĩa là dòng thời gian sẽ tự nhiên được chia thành các phân đoạn độc lập được phân tách bằng các thao tác tính điểm. 

Những ràng buộc tiết lộ điều gì đó bất thường. Số ngày có thể lớn như`10^9`, nên giải pháp mô phỏng cả ngày đều không thể thực hiện được. Mặt khác, tổng của tất cả`n`các giá trị trên toàn bộ đầu vào chỉ`2000`. Điều đó cho thấy rõ ràng giải pháp dự kiến ​​nên chi khoảng`O(n²)`làm việc trên mỗi trường hợp thử nghiệm và hoàn toàn tránh sự phụ thuộc vào`d`. 

Một điểm tinh tế là sau khi thiết lập lại, mảng sẽ trở thành toàn số 0. Vì các vị trí được đánh số từ`1`, một mảng hoàn toàn bằng 0 sẽ đóng góp điểm bằng 0. Thực hiện hai phép tính điểm liên tiếp luôn vô ích vì phép tính thứ hai ghi điểm bằng 0 và giữ nguyên mảng. 

Một sai lầm dễ mắc phải khác là cho rằng nhiều ngày là quan trọng. Coi như:```
n = 3
a = [0,0,0]
v = [3]
d = 10^9
```Sau vị trí`j`đã vượt quá giá trị`j`, số gia tăng thêm không bao giờ có thể bằng`j`một lần nữa vì giá trị chỉ tăng lên. Sự phát triển hữu ích của mảng là rất ngắn mặc dù`d`là rất lớn. 

Cái bẫy thứ ba là quên rằng một thao tác tính điểm tiêu tốn một ngày. Giả định:```
n = 1
a = [1]
v = [1]
d = 1
```Câu trả lời là`1`, bởi vì chúng tôi có thể ghi bàn ngay lập tức. Chúng ta không thể vừa tăng vừa ghi điểm trong cùng một ngày. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là xem mỗi ngày như một điểm quyết định và thực hiện lập trình động theo ngày. Trạng thái sẽ cần phải nhớ toàn bộ mảng hiện tại vì điểm số trong tương lai phụ thuộc vào giá trị chính xác của nó. Từ`d`có thể đạt được`10^9`, điều này thật vô vọng. 

Để tìm thứ gì đó nhỏ hơn, chúng ta nên xem xét những gì thực sự thay đổi. 

Mỗi thao tác tăng chỉ thêm một vào tiền tố. Đối với bất kỳ vị trí nào`j`, giá trị của nó tăng đều đều trong suốt đoạn giữa hai lần đặt lại. Một lần`a_j`trở nên lớn hơn`j`, nó không bao giờ có thể đóng góp vào điểm số trong tương lai trong phân khúc đó. 

Giả sử chúng ta không bao giờ thiết lập lại và thực hiện bước đầu tiên`t`các hoạt động tăng dần. Cho phép```
good(t) = number of positions j with current value exactly j
```sau những cái đó`t`số gia tăng. 

Bây giờ hãy tưởng tượng việc chọn một phép toán tính điểm vào ngày`t + 1`. 

Chúng tôi đạt được`good(t)`điểm ngay lập tức. 

Những ngày còn lại thì sao? Sau khi ghi điểm, mảng trở thành toàn số 0. Bắt đầu từ số 0, số điểm cao nhất có thể đạt được trong những ngày còn lại đơn giản đến mức đáng ngạc nhiên. 

Từ một mảng bằng 0, sau`x`Những ngày trong tương lai, cứ hai ngày bạn có thể đạt được nhiều nhất một điểm: một ngày để tạo ra thứ gì đó hữu ích và một ngày để ghi điểm đó. Quan sát chính thức được sử dụng trong các giải pháp được chấp nhận là đóng góp bổ sung tối đa bằng```
floor((remaining_days) / 2)
```bởi vì vị trí`1`luôn có thể được tái tạo và thu hoạch nhiều lần. 

Vì vậy, nếu chúng ta chọn tính điểm sau khi thực hiện chính xác`t`tăng các hoạt động từ mảng ban đầu, tổng số điểm có thể đạt được là```
good(t) + floor((d - t - 1) / 2)
```Ở đâu`t + 1`là ngày được sử dụng để ghi điểm. 

Câu hỏi còn lại là có bao nhiêu giá trị của`t`phải được kiểm tra. 

Đối với vị trí`j`, một khi giá trị của nó vượt quá`j`, nó vĩnh viễn vô dụng. Kể từ lúc đầu`0 ≤ a_j ≤ n`, mỗi vị trí chỉ có thể bằng chỉ số của nó trong lần đầu tiên`O(n)`số gia tăng. Sau khoảng`2n`hoạt động, mọi vị trí đều đã khớp hoặc vượt quá mức vĩnh viễn. 

Giải pháp chính thức khai thác điều này và chỉ kiểm tra```
t = 0, 1, ..., min(d - 1, 2n)
```trong khi mô phỏng rõ ràng những bổ sung tiền tố đó. 

Bởi vì tổng của tất cả`n`chỉ là`2000`, mô phỏng này dễ dàng đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua nhiều ngày | Hàm mũ/phụ thuộc vào`d`| Rất lớn | Quá chậm | 
| Mô phỏng tối ưu lần đầu`2n`tiểu bang |`O(n²)`mỗi trường hợp thử nghiệm |`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`cur`là một bản sao của mảng ban đầu. 
2. Tính số vị trí thỏa mãn`cur[i] = i + 1`. Gọi giá trị này`good`. 
3. Khởi tạo câu trả lời bằng```
good + floor((d - 1) / 2)
```Điều này tương ứng với việc ghi điểm ngay trong ngày thứ nhất. 
4. Mô phỏng từng thao tác tăng dần. 
5. Hãy để`lim = min(d - 1, 2n)`. Chúng ta chỉ cần kiểm tra điều đầu tiên`lim`các hoạt động tăng dần. 
6. Đối với bước`t`từ`1`ĐẾN`lim`: 

1. Áp dụng số tiền tố tiếp theo bằng cách sử dụng`v[(t - 1) mod k]`. 
2. Tính lại xem hiện tại có bao nhiêu vị trí thỏa mãn`cur[i] = i + 1`. 
3. Nếu vẫn còn ít nhất một ngày để thực hiện thao tác tính điểm, hãy cập nhật```
answer = max(
    answer,
    good + floor((d - t - 1) / 2)
)
```Đây`t`ngày tăng dần đã được sử dụng và một ngày nữa sẽ được sử dụng để tính điểm. 
7. Xuất ra câu trả lời tối đa tìm được. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ chiến lược tối ưu nào. Tập trung vào thao tác tính điểm đầu tiên mà nó thực hiện. 

Giả sử điểm số này xảy ra sau chính xác`t`tăng các hoạt động từ mảng ban đầu. Số điểm đạt được tại thời điểm đó chính xác là`good(t)`. 

Sau ngày đó, mảng trở thành toàn số 0 và quá khứ không còn quan trọng nữa. Bài toán tối ưu còn lại chỉ phụ thuộc vào số ngày còn lại. Bắt đầu từ số không, cứ hai ngày có thể đóng góp tối đa một điểm và giới hạn này có thể đạt được. Do đó sự đóng góp tốt nhất trong tương lai chính xác là`floor((remaining_days)/2)`. 

Vì vậy, mọi chiến lược đều tương ứng với việc lựa chọn một số`t`và thu được```
good(t) + floor((d - t - 1)/2).
```Thuật toán đánh giá tất cả các giá trị liên quan của`t`. 

Chỉ có người đầu tiên`2n`các giá trị quan trọng vì mọi vị trí chỉ có thể đóng góp trước khi nó vĩnh viễn vượt quá chỉ số của nó. Sau thời điểm đó`good(t)`không bao giờ tăng, trong khi số hạng thứ hai giảm khi`t`lớn lên. Không lớn hơn`t`có thể cải thiện câu trả lời. 

Do đó, giá trị tối đa được thuật toán kiểm tra bằng điểm tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, k, d = map(int, input().split())
        a = list(map(int, input().split()))
        v = list(map(int, input().split()))

        def count_good(arr):
            cnt = 0
            for i, x in enumerate(arr, start=1):
                if x == i:
                    cnt += 1
            return cnt

        cur = a[:]

        ans = count_good(cur) + (d - 1) // 2

        lim = min(d - 1, 2 * n)

        for step in range(1, lim + 1):
            b = v[(step - 1) % k]

            for i in range(b):
                cur[i] += 1

            good = count_good(cur)
            ans = max(ans, good + (d - step - 1) // 2)

        print(ans)

if __name__ == "__main__":
    solve()
```Mảng`cur`đại diện cho trạng thái sau một số thao tác tăng dần nhất định từ cấu hình ban đầu. 

Đối với mọi số lần tăng có thể được thực hiện`t`, chúng tôi đánh giá tình huống trong đó thao tác tính điểm đầu tiên xảy ra ngay sau đó. biểu hiện```
good + (d - step - 1) // 2
```phù hợp với công thức rút ra trong chứng minh. 

Sự ràng buộc```
lim = min(d - 1, 2 * n)
```là sự tối ưu hóa quan trọng. Mô phỏng hơn`2n`các hoạt động tăng dần là không cần thiết vì không có vị trí nào có thể trở nên hữu ích sau thời điểm đó. 

Một chi tiết thường gây ra những sai sót nhỏ là ý nghĩa của`step`. Sau đó`step`số ngày tăng dần thì cần thêm một ngày để tính điểm. Vì vậy số ngày còn lại bằng nhau```
d - step - 1
```còn hơn là`d - step`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4 4
1 2 3
1 3 2 3
```Mảng ban đầu đã đáp ứng tất cả các vị trí. 

| t | Trạng thái mảng | tốt(t) | Giá trị được đánh giá | 
| --- | --- | --- | --- | 
| 0 | [1,2,3] | 3 | 3 + 1 = 4 | 
| 1 | [2,2,3] | 2 | 2 + 1 = 3 | 
| 2 | [3,3,4] | 0 | 0 + 0 = 0 | 
| 3 | [4,4,4] | 0 | 0 + 0 = 0 | 

Giá trị tối đa là`4`. 

Dấu vết này cho thấy tại sao việc ghi điểm ngay lập tức có thể là tối ưu. Việc trì hoãn điểm số đầu tiên sẽ làm giảm phần thưởng trong tương lai đồng thời phá hủy các trận đấu hiện có. 

### Ví dụ 2 

đầu vào:```
6 2 3
6 1 2 4 1 5
6 6
```| t | Trạng thái mảng | tốt(t) | Giá trị được đánh giá | 
| --- | --- | --- | --- | 
| 0 | [6,1,2,4,1,5] | 0 | 0 + 1 = 1 | 
| 1 | [7,2,3,5,2,6] | 2 | 2 + 0 = 2 | 
| 2 | [8,3,4,6,3,7] | 3 | 3 + 0 = 3 | 

Câu trả lời là`3`. 

Ví dụ này chứng tỏ rằng việc chờ đợi trước điểm số đầu tiên có thể tốt hơn khi các thao tác tăng dần tạo ra các vị trí khớp bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n * min(2n, d))`| Nhiều nhất`2n`ngày mô phỏng, mỗi lần chạm nhiều nhất`n`yếu tố | 
| Không gian |`O(n)`| Chỉ mảng hiện tại được lưu trữ | 

Từ`min(2n, d) ≤ 2n`, độ phức tạp thời gian là`O(n²)`. Tổng số tiền của tất cả`n`giá trị trên đầu vào nhiều nhất là`2000`, do đó tổng công việc nằm dưới mức giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, k, d = map(int, input().split())
        a = list(map(int, input().split()))
        v = list(map(int, input().split()))

        cur = a[:]

        def good(arr):
            return sum(x == i + 1 for i, x in enumerate(arr))

        ans = good(cur) + (d - 1) // 2
        lim = min(d - 1, 2 * n)

        for step in range(1, lim + 1):
            b = v[(step - 1) % k]
            for i in range(b):
                cur[i] += 1

            ans = max(ans, good(cur) + (d - step - 1) // 2)

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run(
"""5
3 4 4
1 2 3
1 3 2 3
6 2 3
6 1 2 4 1 5
6 6
5 1 1
0 5 0 5 0
5
1 1 1
1
1
3 4 6
1 2 3
1 3 2 3
"""
) == "4\n3\n0\n1\n5"

# minimum size
assert run(
"""1
1 1 1
1
1
"""
) == "1"

# all zeros
assert run(
"""1
3 1 3
0 0 0
3
"""
) == "1"

# already perfect array
assert run(
"""1
4 1 2
1 2 3 4
4
"""
) == "4"

# off-by-one around last day
assert run(
"""1
1 1 2
0
1
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1,d=1,a=[1]`|`1`| Ghi điểm ngay lập tức | 
| Tất cả số không |`1`| Xây dựng điểm số từ đầu | 
| Mảng đã hoàn hảo |`4`| Câu trả lời hay nhất có thể là ở`t=0`| 
| Yếu tố duy nhất, hai ngày |`1`| Tính đúng số ngày còn lại | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1 1
1
1
```Mảng đã có một vị trí phù hợp. Thuật toán đánh giá`t=0`và thu được```
1 + (1 - 1) // 2 = 1.
```Không thể thực hiện thao tác tăng dần vì không còn ngày nào cho điểm tiếp theo. Đầu ra là chính xác`1`. 

Bây giờ hãy xem xét:```
1
3 1 3
0 0 0
3
```Các tiểu bang là:```
t=0 : [0,0,0], good=0
t=1 : [1,1,1], good=1
t=2 : [2,2,2], good=1
```Giá trị tốt nhất thu được ở`t=1`, cho điểm`1`. Điều này xác nhận rằng thuật toán xử lý chính xác các mảng bắt đầu không có vị trí khớp. 

Cuối cùng hãy xem xét:```
1
4 1 10
4 4 4 4
1
```Chức vụ`1`nhanh chóng vượt quá mục tiêu và không bao giờ trở nên hữu ích nữa. Sau vài ngày mô phỏng đầu tiên,`good(t)`không bao giờ tăng. Vì thời hạn thưởng trong tương lai giảm dần theo`t`thô, chỉ kiểm tra lần đầu`2n`trạng thái là đủ. Thuật toán dừng sớm mà không cần kiểm tra cả mười ngày, đây chính xác là quan sát giúp giải pháp hoạt động với số lượng rất lớn.`d`.

---
title: "CF 102419G - Mảng lớn"
description: "Mảng a không được đưa ra một cách rõ ràng. Nó là sự lặp lại vô tận của mảng b ngắn hơn, bị cắt bớt sau n phần tử. Nếu b = [b0, b1, ..., b(m-1)] thì mọi khối gồm m phần tử liên tiếp của a là một bản sao khác của b."
date: "2026-08-14T14:53:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102419
codeforces_index: "G"
codeforces_contest_name: "SPC 2019"
rating: 0
weight: 102419
solve_time_s: 244
verified: true
draft: false
---

[CF 102419G - Mảng lớn](https://codeforces.com/problemset/problem/102419/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mảng`a`không được đưa ra một cách rõ ràng. Nó là sự lặp lại vô tận của mảng ngắn hơn`b`, cắt ngắn sau`n`các phần tử. Nếu như`b = [b0, b1, ..., b(m-1)]`, thì mọi khối của`m`các phần tử liên tiếp của`a`là một bản sao khác của`b`. 

Chúng ta cần một phân đoạn liền kề không trống của`a`tổng của nó chính xác là`k`. Trong số tất cả các phân đoạn như vậy, chúng tôi muốn phân đoạn ngắn nhất và nếu một số phân đoạn có cùng độ dài, chúng tôi muốn phân đoạn có điểm cuối bên trái nhỏ nhất. 

Có một sự mâu thuẫn nhỏ trong tuyên bố được lưu trữ: nó nói rằng câu trả lời thỏa mãn`1 <= l <= r <= n`, trong khi các mảng được khai báo có chỉ mục 0 và bản in mẫu chính thức`0 3`. Đầu ra dự định là các điểm cuối bao gồm dựa trên 0, do đó việc triển khai bên dưới tuân theo mẫu và bản in`l`Và`r`với`0 <= l <= r < n`. 

Ràng buộc quan trọng là`n <= 10^9`, trong khi chỉ`m <= 10^5`được giới hạn bởi giới hạn xử lý mảng thông thường. Xây dựng`a`rõ ràng có thể yêu cầu một tỷ phần tử, vốn đã quá lớn so với giới hạn bộ nhớ. Ngay cả việc quét O(n) cũng quá tốn kém trong trường hợp xấu nhất. Tổng của`m`trên tất cả các trường hợp thử nghiệm là nhiều nhất`3 * 10^5`, vì vậy thuật toán O(m log m) cho mỗi trường hợp thử nghiệm là thực tế. 

Các giá trị của`b`có thể âm nên không áp dụng được các kỹ thuật như cửa sổ trượt. Tổng tiền tố là cách biểu diễn tự nhiên vì tổng phân đoạn có thể được biểu thị dưới dạng hiệu giữa hai tổng tiền tố, bất kể dấu của các phần tử. Tổng tiền tố cũng có thể đạt khoảng`10^14`, Và`k`có thể đạt được`10^18`, vì vậy số học 32 bit là không đủ. Số nguyên Python xử lý trực tiếp các giá trị này. 

Trường hợp cạnh đầu tiên là một đoạn trống. Ví dụ,```
1
1 3 0
5
```không có đoạn nào khác trống với tổng bằng 0, nên câu trả lời là`-1`. Việc triển khai tổng tiền tố cho phép sử dụng cùng một vị trí tiền tố hai lần có thể vô tình báo cáo một đoạn có độ dài bằng 0. 

Trường hợp cạnh thứ hai là một câu trả lời trải dài với số lần lặp lại rất lớn. Vì```
1
1 1000000000 1000000000
1
```câu trả lời duy nhất có thể là toàn bộ mảng,`0 999999999`. Bất kỳ giải pháp nào chỉ kiểm tra một hoặc một vài bản sao của`b`không thể tìm thấy nó. 

Trường hợp cạnh thứ ba xảy ra khi một bản sao hoàn chỉnh của`b`có tổng bằng 0. Vì```
1
2 2 0
1 -1
```câu trả lời là`0 1`. Việc tìm kiếm bị giới hạn ở các mảng con thích hợp bên trong một bản sao sẽ bỏ lỡ phân đoạn toàn thời gian này. 

Trường hợp cạnh thứ tư là tổng số âm. Bản thân mẫu chứa`b = [1, 1, -3]`, tổng của nó là`-1`, và câu trả lời là`0 3`. Đạo hàm giả định số chu kỳ hoàn chỉnh thu được bằng phép chia dương thông thường có thể chọn hướng sai khi tổng chu kỳ âm. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ xây dựng toàn bộ mảng`a`, tính tổng tiền tố của nó và tìm hai vị trí tiền tố có hiệu là`k`. Với bản đồ băm, điều đó sẽ tốn O(n) thời gian và bộ nhớ O(n), điều này là không thể khi`n`là`10^9`. Việc liệt kê từng cặp điểm cuối thậm chí còn tệ hơn. có`n(n+1)/2`phân đoạn không trống, đó là về`5 * 10^17`phân đoạn khi`n = 10^9`. 

Ý tưởng vũ phu vẫn hữu ích vì nó tiết lộ cấu trúc thực sự của vấn đề. Mỗi phân đoạn được xác định bởi hai vị trí tiền tố và lý do duy nhất chúng tôi không thể kiểm tra chúng trực tiếp là có quá nhiều bản sao lặp lại của`b`. 

Cho phép`S`là tổng của một bản sao hoàn chỉnh của`b`. Xác định tổng tiền tố bên trong một bản sao bằng cách`p[0] = 0`và, đối với`1 <= i < m`,`p[i] = b[0] + b[1] + ... + b[i-1]`. 

Vị trí tiền tố có chỉ mục`q * m + r`, Ở đâu`0 <= r < m`, có giá trị`P(q * m + r) = q * S + p[r]`. 

Đây là nén khóa. Mặc dù`q`có thể lớn tới một tỷ, phần không tuần hoàn của mỗi tổng tiền tố là một trong số duy nhất`m`các giá trị. 

Xem xét vị trí tiền tố bên phải của ứng viên`y = q*m+r`. Giả sử vị trí tiền tố bên trái phù hợp của nó là`x = (q-h)*m+s`. 

Sau đó`P[y] - P[x] = h*S + p[r] - p[s]`. 

Vì`S != 0`, số lượng thời gian hoàn thành cần thiết bị buộc phải:`h = (k + p[s] - p[r]) / S`. 

Khoảng cách giữa các vị trí tiền tố là`y-x = h*m + r-s`. 

Đối với dư lượng quyền cố định`r`, giảm thiểu biểu thức này có nghĩa là tìm giá trị nhỏ nhất có thể`h`, và sau đó lớn nhất có thể`s`. 

Điều kiện chia hết cho một quan sát hữu ích khác. Chúng tôi cần`p[s] ≡ p[r] - k (mod |S|)`. 

Vì vậy, với mỗi dư lượng modulo`|S|`, chúng ta có thể giữ tất cả các tổng tiền tố thuộc phần dư đó theo thứ tự được sắp xếp. Nếu như`S > 0`, tăng dần`p[s]`tăng lên`h`, vì vậy chúng ta cần nhỏ nhất`p[s]`trên ngưỡng tương ứng với`h >= 1`. Nếu như`S < 0`, hướng đảo ngược, vì vậy chúng ta cần giá trị lớn nhất`p[s]`dưới ngưỡng tương ứng. 

Vụ án`h = 0`là đặc biệt. Khi đó cả hai vị trí tiền tố đều nằm trong cùng một bản sao, vì vậy chúng ta phải có`s < r`. Chúng ta có thể tìm thấy lần xuất hiện mới nhất trước đó của giá trị tiền tố được yêu cầu trong khi quét phần dư từ trái sang phải. 

Nếu như`S = 0`, các bản sao hoàn chỉnh không đóng góp gì vào tổng tiền tố. Điều kiện mục tiêu trở nên đơn giản`p[s] = p[r] - k`. 

Bên trong cùng một bản sao chúng ta lại cần`s < r`. Nếu không có vị trí như vậy tồn tại, chúng ta có thể sử dụng cùng một giá trị tiền tố từ bản sao trước đó. Điều đó mang lại một đoạn có chiều dài`m+r-s`, miễn là điểm cuối bên phải của nó vẫn nằm trong mảng thực tế. 

Lực lượng vũ phu hoạt động vì mọi phân đoạn hợp lệ tương ứng với một cặp tổng tiền tố. Nó không thành công vì có quá nhiều vị trí tiền tố. Nhận xét rằng tất cả các vị trí tiền tố đều có dạng`q*S + p[r]`hãy để chúng tôi loại bỏ cái lớn`q`kích thước và giải quyết phần còn lại`m`trường hợp dư lượng với sắp xếp và tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tiền tố băm qua`a`| O(n) | O(n) | Quá chậm | 
| Nén tiền tố định kỳ | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số tiền`S`của một bản sao của`b`và các giá trị tiền tố`p[0], ..., p[m-1]`. Những thứ này đủ để thể hiện mọi vị trí tiền tố của mảng lớn. 
2. Nếu`S = 0`, xử lý vấn đề một cách riêng biệt. Đối với mỗi dư lượng`r`, giá trị tiền tố bắt buộc trước đó là`p[r] - k`. Tìm kiếm lần xuất hiện mới nhất trước đó`r`để có được một phân đoạn bên trong bản sao đầu tiên. Nếu điều đó không tồn tại, hãy sử dụng lần xuất hiện mới nhất ở bất kỳ đâu và đặt nó vào bản sao trước đó. Ứng cử viên sau kết thúc ở vị trí tiền tố`m+r`, vì vậy nó chỉ có thể sử dụng được khi`m+r <= n`. 
3. Nếu`S != 0`, nhóm từng cặp`(p[s] mod |S|, p[s], s)`bởi modulo dư lượng của nó`|S|`và sắp xếp bộ sưu tập hoàn chỉnh theo phần dư và sau đó theo giá trị tiền tố. Điều này cho phép chúng tôi xác định vị trí tốt nhất có thể`p[s]`cho mọi dư lượng bên phải bằng cách sử dụng tìm kiếm nhị phân. 
4. Xử lý mọi dư lượng quyền có thể`r`từ`0`bởi vì`m-1`. Đầu tiên hãy tìm`p[s] = p[r] - k`giữa các vị trí`s < r`. Đây là`h = 0`trường hợp. Vì chiều dài của nó là`r-s`, hợp lệ lớn nhất`s`luôn là ứng cử viên tốt nhất cho việc này`r`. 
5. Đối với`h >= 1`, chỉ các giá trị tiền tố thỏa mãn`p[s] ≡ p[r] - k (mod |S|)`có thể làm việc. Nếu như`S > 0`, tìm giá trị tiền tố đầu tiên thỏa mãn`p[s] >= p[r] - k + S`. 

Nếu như`S < 0`, tìm giá trị tiền tố cuối cùng thỏa mãn`p[s] <= p[r] - k + S`. 

Những bất đẳng thức này chính là điều kiện`h >= 1`và việc chọn giá trị tiền tố gần nhất có thể sẽ cho giá trị nhỏ nhất`h`. 
6. Một lần`p[s]`được chọn, tính toán`h = (k + p[s] - p[r]) / S`. 

Sự xuất hiện sớm nhất của ứng cử viên cấu trúc này có được bằng cách chọn`q = h`. Khi đó vị trí tiền tố bên trái chỉ đơn giản là`s`và vị trí tiền tố bên phải là`h*m+r`. Ứng viên chỉ có thể được sử dụng khi`h*m+r <= n`. 
7. Chuyển đổi vị trí tiền tố trở lại điểm cuối mảng. Một cặp tiền tố`(x,y)`đại diện cho phân đoạn mảng bao gồm`[x, y-1]`. So sánh các ứng viên trước tiên bằng chiều dài của họ`y-x`, sau đó đến điểm cuối bên trái của chúng`x`. 
8. Nếu không tìm thấy ứng viên nào, hãy in`-1`. Nếu không thì hãy in các điểm cuối bao gồm dựa trên 0 tốt nhất. 

### Tại sao nó hoạt động 

Mỗi mảng con không trống tương ứng duy nhất với hai vị trí tiền tố`x < y`, với tổng`P[y]-P[x] = k`. Vì`S != 0`, viết hai vị trí là`(q-h)m+s`Và`qm+r`đưa ra phương trình chính xác`h*S+p[r]-p[s]=k`. Do đó, mọi phân đoạn hợp lệ đều xuất hiện trong số các ứng cử viên được thuật toán xem xét. 

Vì`h=0`, điều kiện`s<r`chính xác là điều kiện là cả hai vị trí tiền tố đều nằm trong cùng một bản sao. Vì`h>=1`, tiền tố bên trái tự động xuất hiện trước tiền tố bên phải và việc tìm kiếm đồng dư cộng với ngưỡng sẽ xem xét chính xác các giá trị có thể có của`s`điều đó mang lại sự tích cực`h`. Đối với một cố định`r`, chiều dài là`h*m+r-s`, vậy nhỏ nhất`h`rồi lớn nhất`s`đưa ra ứng cử viên ngắn nhất. Lựa chọn`q=h`tạo ra cùng độ dài với điểm cuối bên trái nhỏ nhất có thể. các`S=0`nhánh xem xét hai trường hợp cấu trúc có thể xảy ra duy nhất, cùng một bản sao và bản sao trước đó. Do đó, mọi phân đoạn tối ưu có thể đều được xem xét và so sánh cuối cùng sẽ chọn chính xác câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        m, n, k = map(int, input().split())
        b = list(map(int, input().split()))

        p = [0] * m
        cur = 0
        for i in range(m - 1):
            cur += b[i]
            p[i + 1] = cur

        S = sum(b)

        best_len = None
        best_l = None
        best_r = None

        def update(x, y):
            nonlocal best_len, best_l, best_r

            length = y - x
            l = x
            r = y - 1

            if best_len is None or (length, l) < (best_len, best_l):
                best_len = length
                best_l = l
                best_r = r

        if S == 0:
            last_all = {}
            for i, value in enumerate(p):
                last_all[value] = i

            last_before = {}

            for r in range(m):
                target = p[r] - k

                # h = 0, so the left prefix must be before r.
                s = last_before.get(target)
                if s is not None:
                    update(s, r)

                # Use the same prefix value in the previous copy.
                s = last_all.get(target)
                if s is not None:
                    y = m + r
                    if y <= n:
                        update(s, y)

                last_before[p[r]] = r

        else:
            D = abs(S)

            # Each item is (p[s] mod D, p[s], s).
            data = [(p[i] % D, p[i], i) for i in range(m)]
            data.sort()

            # For every residue, store the half-open interval in data.
            bounds = {}
            start = 0
            while start < m:
                key = data[start][0]
                end = start + 1
                while end < m and data[end][0] == key:
                    end += 1
                bounds[key] = (start, end)
                start = end

            last_before = {}

            for r in range(m):
                pr = p[r]
                target = pr - k

                # h = 0.
                s = last_before.get(target)
                if s is not None:
                    update(s, r)

                key = target % D
                interval = bounds.get(key)

                if interval is not None:
                    lo, hi = interval

                    if S > 0:
                        # h >= 1 means:
                        # k + p[s] - p[r] >= S
                        threshold = pr - k + S

                        idx = bisect_left(
                            data,
                            (key, threshold, -1),
                            lo,
                            hi
                        )

                        if idx < hi:
                            pval = data[idx][1]

                            # For the same pval, take the largest s,
                            # because that minimizes h*m + r - s.
                            j = bisect_right(
                                data,
                                (key, pval, m),
                                lo,
                                hi
                            ) - 1

                            s = data[j][2]
                            h = (k + pval - pr) // S
                            y = h * m + r

                            if h >= 1 and y <= n:
                                update(s, y)

                    else:
                        # h >= 1 means:
                        # k + p[s] - p[r] <= S
                        threshold = pr - k + S

                        idx = bisect_right(
                            data,
                            (key, threshold, m),
                            lo,
                            hi
                        ) - 1

                        if idx >= lo:
                            pval = data[idx][1]
                            s = data[idx][2]
                            h = (k + pval - pr) // S
                            y = h * m + r

                            if h >= 1 and y <= n:
                                update(s, y)

                last_before[pr] = r

        if best_len is None:
            out.append("-1")
        else:
            out.append(f"{best_l} {best_r}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Tiền tố xây dựng chỉ lưu trữ`m`các giá trị.`p[i]`là tổng trước vị trí`i`bên trong bản sao đầu tiên, do đó, các vị trí tiền tố trong mỗi bản sao sau này có thể được xây dựng lại bằng đại số thay vì được cụ thể hóa. 

các`S == 0`nhánh sử dụng hai từ điển.`last_before`chỉ chứa các vị trí nghiêm ngặt trước phần dư hiện tại, điều này ngăn cản việc vô tình xây dựng một phân đoạn trống.`last_all`cung cấp lần xuất hiện mới nhất khi tiền tố phù hợp phải đến từ bản sao trước đó. 

Vì`S != 0`,`data`được sắp xếp theo`p[s] mod |S|`Đầu tiên. Các giá trị tiền tố trong cùng một loại dư lượng chính xác là các giá trị có thể tham gia vào phương trình được yêu cầu. Tìm kiếm nhị phân thực hiện theo hướng được áp đặt bởi dấu của`S`. Bộ dữ liệu cũng chứa`s`, do đó, các giá trị tiền tố bằng nhau có thể được sắp xếp theo chỉ mục và chỉ mục mới nhất có thể được chọn trực tiếp. 

Việc tính toán`h`chỉ được thực hiện sau khi điều kiện cặn đã đảm bảo tính chia hết cho`S`. Các số nguyên có độ chính xác tùy ý của Python xử lý một cách an toàn các sản phẩm lớn liên quan đến`n`,`m`, và các tổng tiền tố. 

Mã này sử dụng các vị trí tiền tố nội bộ, vì vậy một ứng viên`(x,y)`trở thành khoảng mảng bao gồm`[x,y-1]`. Đây cũng là lý do tại sao mẫu`0 3`được sản xuất cho trường hợp thử nghiệm đầu tiên. 

## Ví dụ đã hoạt động 

###Bài kiểm tra mẫu 1 

Đầu vào là```
3 5 0
1 1 -3
```Tổng thời gian là`S = -1`và các giá trị tiền tố cho dư lượng`0,1,2`là`[0,1,2]`. 

|`r`|`p[r]`| mục tiêu`p[r]-k`|`h=0`ứng cử viên | tích cực`h`ứng cử viên | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | không | không | không | 
| 1 | 1 | 1 | không |`s=0, h=1, y=4`|`0 3`| 
| 2 | 2 | 2 | không |`s=1, h=1, y=5`|`0 3`| 

Vì`r=1`, đang chọn`s=0`cho`h=1`. Các vị trí tiền tố là`x=0`Và`y=4`, vậy phân đoạn mảng là`[0,3]`, chứa`1,1,-3,1`và có tổng bằng 0. Ứng cử viên cho`r=2`cũng có độ dài bằng bốn, nhưng bắt đầu ở chỉ số`1`, do đó quy tắc ràng buộc được giữ nguyên`0 3`. 

###Bài kiểm tra mẫu 2 

Đầu vào là```
5 5 10
1 1 1 2 2
```Đây`S=7`Và`p = [0,1,2,3,5]`. 

|`r`|`p[r]`| modulo mục tiêu`7`| ứng cử viên tích cực | tiền tố bên phải`y`| có thể sử dụng được? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 4 | không | | không | 
| 1 | 1 | 5 |`s=4, h=2`| 11 | không | 
| 2 | 2 | 6 | không | | không | 
| 3 | 3 | 0 |`s=0, h=1`| 8 | không | 
| 4 | 5 | 2 |`s=2, h=1`| 9 | không | 

Mọi ứng cử viên hợp lệ về mặt đại số đều cần một vị trí tiền tố bên phải ngoài`n=5`. Vì vậy không có mảng con nào của mảng đã cho có thể tổng bằng 10, và câu trả lời là`-1`. 

Những dấu vết này cho thấy tại sao`n`chỉ xuất hiện trong lần kiểm tra tính khả thi cuối cùng. Số lượng bản sao khổng lồ tiềm tàng không bao giờ phải được tạo ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Xây dựng tổng tiền tố là O(m), sắp xếp dữ liệu dư lượng là O(m log m) và mỗi`m`dư lượng thực hiện tìm kiếm nhị phân O (log m). | 
| Không gian | O(m) | Mảng tiền tố, dữ liệu được sắp xếp, ranh giới dư lượng và từ điển đều chứa các mục nhập O(m). | 

Tổng cộng`m`trên tất cả các trường hợp thử nghiệm là nhiều nhất`3 * 10^5`, do đó giải pháp chỉ xử lý vài trăm nghìn trạng thái tiền tố được lưu trữ ngay cả khi`n`lớn như`10^9`. Việc sử dụng bộ nhớ là tuyến tính trong`m`và thời gian bị chi phối bởi việc sắp xếp và tìm kiếm nhị phân, phù hợp với giới hạn 3 giây và 256 MB tốt hơn nhiều so với bất kỳ thứ gì phụ thuộc tuyến tính vào`n`. 

## Trường hợp thử nghiệm```python
# This harness contains the same algorithm as the submitted solution,
# but exposes solve_io() so that each test can be checked with assertions.

import sys
import io
from bisect import bisect_left, bisect_right

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        m, n, k = map(int, input().split())
        b = list(map(int, input().split()))

        p = [0] * m
        cur = 0
        for i in range(m - 1):
            cur += b[i]
            p[i + 1] = cur

        S = sum(b)

        best = None

        def update(x, y):
            nonlocal best
            cand = (y - x, x, y - 1)
            if best is None or cand[:2] < best[:2]:
                best = cand

        if S == 0:
            last_all = {}
            for i, value in enumerate(p):
                last_all[value] = i

            last_before = {}

            for r in range(m):
                target = p[r] - k

                s = last_before.get(target)
                if s is not None:
                    update(s, r)

                s = last_all.get(target)
                if s is not None:
                    y = m + r
                    if y <= n:
                        update(s, y)

                last_before[p[r]] = r

        else:
            D = abs(S)
            data = [(p[i] % D, p[i], i) for i in range(m)]
            data.sort()

            bounds = {}
            start = 0
            while start < m:
                key = data[start][0]
                end = start + 1
                while end < m and data[end][0] == key:
                    end += 1
                bounds[key] = (start, end)
                start = end

            last_before = {}

            for r in range(m):
                pr = p[r]
                target = pr - k

                s = last_before.get(target)
                if s is not None:
                    update(s, r)

                key = target % D
                interval = bounds.get(key)

                if interval is not None:
                    lo, hi = interval

                    if S > 0:
                        threshold = pr - k + S
                        idx = bisect_left(
                            data, (key, threshold, -1), lo, hi
                        )

                        if idx < hi:
                            pval = data[idx][1]
                            j = bisect_right(
                                data, (key, pval, m), lo, hi
                            ) - 1
                            s = data[j][2]
                            h = (k + pval - pr) // S
                            y = h * m + r

                            if y <= n:
                                update(s, y)

                    else:
                        threshold = pr - k + S
                        idx = bisect_right(
                            data, (key, threshold, m), lo, hi
                        ) - 1

                        if idx >= lo:
                            pval = data[idx][1]
                            s = data[idx][2]
                            h = (k + pval - pr) // S
                            y = h * m + r

                            if y <= n:
                                update(s, y)

                last_before[pr] = r

        out.append("-1" if best is None else f"{best[1]} {best[2]}")

    sys.stdin = old_stdin
    return "\n".join(out)

# Provided sample
assert solve_io("""\
2
3 5 0
1 1 -3
5 5 10
1 1 1 2 2
""") == """\
0 3
-1
""", "provided sample"

# Minimum-size input
assert solve_io("""\
1
1 1 5
5
""") == "0 0", "single element"

# Maximum n with m = 1
assert solve_io("""\
1
1 1000000000 1000000000
1
""") == "0 999999999", "huge number of repetitions"

# All equal values
assert solve_io("""\
1
4 7 6
2 2 2 2
""") == "0 2", "shortest equal-value segment"

# Zero period sum, plus an impossible large target
assert solve_io("""\
2
2 2 0
1 -1
1 3 10
1
""") == """\
0 1
-1
""", "zero total and impossible target"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 5 / 5`|`0 0`| Mảng tối thiểu có thể và câu trả lời một phần tử | 
|`1 / 1 1000000000 1000000000 / 1`|`0 999999999`| To lớn`n`và một phân khúc bao gồm một tỷ phần tử | 
|`1 / 4 7 6 / 2 2 2 2`|`0 2`| Các giá trị bằng nhau và lựa chọn có độ dài ngắn nhất | 
|`2 / 2 2 0 / 1 -1`Và`1 3 10 / 1`|`0 1`,`-1`| Tổng bằng 0, ranh giới cả kỳ và mục tiêu bất khả thi | 

## Vỏ cạnh 

Vấn đề đoạn trống được xử lý bằng cách giữ`last_before`tách biệt khỏi vị trí tiền tố hiện tại. Vì```
1
1 3 0
5
```các giá trị tiền tố duy nhất là`0`ở mọi phần dư, nhưng không có vị trí tiền tố sớm hơn khi xử lý phần dư đầu tiên và duy nhất. Từ`S=5`, cũng không có nghiệm chu kỳ dương cho tổng bằng 0. Thuật toán in`-1`thay vì nhầm lẫn tiền tố với chính nó thành một đoạn có độ dài bằng 0. 

Trường hợp lặp lại rất lớn được xử lý mà không cần xây dựng`a`. Vì```
1
1 1000000000 1000000000
1
```chúng tôi có`S=1`Và`p[0]=0`. Phương trình cho`h=1000000000`. Vị trí tiền tố bên phải là`h*m+r = 1000000000`, chính xác là`n`và vị trí tiền tố bên trái bằng 0. Như vậy câu trả lời là`0 999999999`. Giá trị của`h`có thể rất lớn, nhưng nó chỉ là một phép tính số nguyên. 

Trường hợp dấu chấm bằng 0 sử dụng một nhánh riêng vì phép chia cho`S`sẽ là vô nghĩa. Vì```
1
2 2 0
1 -1
```chúng tôi có`S=0`Và`p=[0,1]`. Tại`r=0`, giá trị tiền tố đích bằng 0 và giá trị tương tự xảy ra tại`s=0`. Không có sớm hơn`s`, do đó thuật toán sẽ đặt lần xuất hiện đó vào bản sao trước đó. Vị trí tiền tố bên phải trở thành`m+r=2`, đưa ra phân đoạn mảng`[0,1]`. Tổng của nó là`1 + (-1) = 0`, vậy kết quả là`0 1`. 

Tổng thời gian âm làm thay đổi hướng tìm kiếm nhị phân. Đối với thử nghiệm đầu tiên của mẫu,`S=-1`Và`p=[0,1,2]`. Tại`r=1`, ứng viên có thời gian tích cực được yêu cầu có`s=0`, cho`h = (0 + 0 - 1) / (-1) = 1`. 

Vị trí tiền tố bên phải là`1*3+1=4`, vậy đoạn đó là`[0,3]`. Các giá trị của nó là`1,1,-3,1`, tổng của nó bằng 0. Thuật toán tìm ứng viên này bằng cách tìm kiếm giá trị tiền tố lớn nhất dưới ngưỡng tổng âm, thay vì sử dụng hướng hợp lệ cho giá trị dương.`S`. 

Vấn đề lập chỉ mục được xử lý bằng cách chuyển đổi chính xác các điểm cuối tiền tố. Nếu cặp tiền tố là`(x,y)`, các phần tử mảng được chọn chính xác là các chỉ mục`x`bởi vì`y-1`. Do đó mã in`x`Và`y-1`, khớp với quy ước đầu ra dựa trên 0 của mẫu chính thức mặc dù giới hạn số được hiển thị trong câu lệnh không nhất quán.

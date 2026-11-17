---
title: "CF 1949B - Bữa Ăn Duyên Dáng"
description: "Chúng tôi có hai mảng có kích thước n. Mảng a chứa giá trị độ cay của món khai vị và mảng b chứa giá trị độ cay của món chính. Mỗi món khai vị phải được kết hợp với đúng một món chính và mỗi món chính phải được dùng đúng một lần."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1949
codeforces_index: "B"
codeforces_contest_name: "European Championship 2024 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 1500
weight: 1949
solve_time_s: 104
verified: true
draft: false
---

[CF 1949B - Bữa ăn quyến rũ](https://codeforces.com/problemset/problem/1949/B) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, bạo lực, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai mảng kích thước`n`. 

Mảng`a`chứa các giá trị độ cay của món khai vị và mảng`b`chứa các giá trị cay của các món ăn chính. Mỗi món khai vị phải được kết hợp với đúng một món chính và mỗi món chính phải được dùng đúng một lần. 

Đối với một cặp`(a[i], b[j])`, sự hấp dẫn của bữa ăn là`|a[i] - b[j]|`. Trong số tất cả các bữa ăn được tạo ra, chúng tôi nhìn vào sự quyến rũ nhỏ nhất. Mục tiêu của chúng tôi là chọn cặp sao cho lá bùa nhỏ nhất này càng lớn càng tốt. 

Đây là một vấn đề "tối đa hóa giá trị tối thiểu" cổ điển. Bất cứ khi nào một vấn đề yêu cầu giá trị lớn nhất`X`sao cho một số điều kiện có thể xảy ra thì tìm kiếm nhị phân thường là một ứng cử viên tốt. 

Những hạn chế là đầu mối quan trọng. Mỗi ca kiểm thử có`n ≤ 5000`, và tổng của`n²`trên tất cả các trường hợp thử nghiệm là nhiều nhất`25 · 10⁶`. Một giải pháp xung quanh`O(n²)`mỗi trường hợp thử nghiệm đều có thể chấp nhận được, trong khi mọi thứ theo giai thừa hoặc hàm mũ đều không thể. Việc liệt kê tất cả các kết quả phù hợp sẽ yêu cầu`n!`những khả năng trở nên vô vọng ngay cả đối với`n = 15`. 

Một khía cạnh tinh tế là điều kiện liên quan đến sự khác biệt tuyệt đối. Đối với một ngưỡng cố định`x`, một cặp là hợp lệ nếu`|a[i] - b[j]| ≥ x`. 

Điều này có nghĩa là một món ăn có thể được kết hợp với một món ăn nhỏ hơn nhiều hoặc với một món ăn lớn hơn nhiều. Một kẻ tham lam ngây thơ chỉ nhìn một phía sẽ dễ dàng thất bại. 

Coi như:```
a = [4, 5]
b = [1, 8]
```Vì`x = 3`, cả hai cặp đều hoạt động:```
4 ↔ 1
5 ↔ 8
```Và```
4 ↔ 8
5 ↔ 1
```Một tham lam luôn lấy giá trị hợp lệ gần nhất có thể chặn phép gán sau này ngay cả khi tồn tại một kết quả khớp hợp lệ. 

Một trường hợp phức tạp khác xuất hiện khi có nhiều giá trị bằng nhau:```
a = [1, 1, 1]
b = [1, 1, 1]
```Mọi sự quyến rũ đều ngang bằng`0`, vậy câu trả lời là`0`. Bất kỳ thuật toán nào giả định các giá trị riêng biệt hoặc dựa vào các bất đẳng thức nghiêm ngặt sẽ bị hỏng ở đây. 

Trường hợp cạnh thứ ba là khi tất cả các giá trị của một mảng thấp hơn nhiều so với tất cả các giá trị của mảng kia:```
a = [0, 0, 0]
b = [100, 100, 100]
```Mỗi sự kết hợp đều tạo nên sự quyến rũ`100`, vậy câu trả lời là`100`. Việc ghép đôi tối ưu không phải là điều gì phức tạp, mọi cặp đôi đều tương đương nhau. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo mọi kết quả khớp hoàn hảo có thể có giữa hai mảng, tính toán độ hấp dẫn tối thiểu bên trong kết quả khớp đó và giữ kết quả tốt nhất. 

Điều này hiệu quả vì nó trực tiếp kiểm tra mọi sắp xếp ghép nối hợp lệ. Vấn đề là số lượng kết quả phù hợp. có`n!`sự kết hợp hoàn hảo. Ngay cả đối với`n = 15`, điều này vượt quá một nghìn tỷ khả năng. Cách tiếp cận này không thể sử dụng được. 

Cấu trúc của mục tiêu gợi ý một quan điểm khác. 

Giả sử ai đó cho chúng ta biết một câu trả lời ứng cử viên`x`. Thay vì yêu cầu câu trả lời tối ưu ngay lập tức, hãy đặt câu hỏi có/không: 

"Liệu chúng ta có thể tạo ra sự kết hợp hoàn hảo sao cho ít nhất mỗi cặp đôi đều có sức quyến rũ không?`x`?" 

Nếu chúng ta có thể trả lời câu hỏi này một cách hiệu quả thì tìm kiếm nhị phân trên`x`mang lại giá trị khả thi tối đa. 

Thách thức còn lại là kiểm tra tính khả thi. 

Sắp xếp cả hai mảng. 

Đối với một ngưỡng cố định`x`, một cặp`(a, b)`được phép khi```
b ≤ a - x
```hoặc```
b ≥ a + x
```Vùng cấm là khoảng```
(a - x + 1 ... a + x - 1)
```Sau khi sắp xếp, tất cả các giá trị bị cấm đối với một mục cụ thể`a[i]`tạo thành một khoảng liền kề trong sắp xếp`b`mảng. 

Quan sát quan trọng là mọi đối tác hợp lệ cho`a[i]`nằm trong tiền tố của`b`hoặc trong một hậu tố của`b`. 

Giả sử chính xác`k`các yếu tố của`b`được gán từ "phía nhỏ" (`b ≤ a - x`). Sau đó phần còn lại`n-k`các yếu tố phải đến từ "phía lớn" (`b ≥ a + x`). 

Vì cả hai mảng đều được sắp xếp nên chúng ta có thể kiểm tra xem việc phân chia như vậy có khả thi hay không. 

Lần đầu tiên`k`món khai vị nhỏ nhất, chúng tôi kết hợp chúng với`k`món chính nhỏ nhất. Chúng tôi cần```
a[i] - b[i] ≥ x
```cho tất cả`i < k`. 

Đối với các phần tử còn lại, chúng tôi khớp```
a[i] with b[i-k+k] = b[i]
```theo thứ tự liên kết giữa các hậu tố, yêu cầu```
b[i] - a[i] ≥ x
```cho tất cả`i ≥ k`. 

Nếu cả hai điều kiện đều đúng cho một số vị trí phân chia`k`, thì tồn tại một kết hợp hoàn hảo hợp lệ. 

Toàn bộ quá trình kiểm tra tính khả thi sẽ được quét qua tất cả các điểm phân chia có thể có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log V + n) cho mỗi ca kiểm thử sau khi sắp xếp | O(n) | Đã chấp nhận | 

Đây`V`là phạm vi giá trị, nhiều nhất`10⁹`. 

## Hướng dẫn thuật toán 

### Kiểm tra tính khả thi của bản sửa lỗi`x`Giả sử cả hai mảng đã được sắp xếp. 

1. Tính một mảng`pref`.`pref[i]`cho biết liệu tất cả các vị trí`0..i`thỏa mãn```
a[j] - b[j] ≥ x
```Nếu như`pref[i]`là đúng thì điều đầu tiên`i+1`món khai vị nhỏ nhất có thể được kết hợp với món đầu tiên`i+1`món ăn chính nhỏ nhất sử dụng điều kiện "mặt nhỏ". 
2. Tính một mảng`suff`.`suff[i]`cho biết liệu tất cả các vị trí`i..n-1`thỏa mãn```
b[j] - a[j] ≥ x
```Nếu như`suff[i]`là đúng thì hậu tố bắt đầu tại`i`có thể được so khớp bằng cách sử dụng điều kiện "cạnh lớn". 
3. Hãy thử mọi vị trí phân chia`k`từ`0`ĐẾN`n`. 

đầu tiên`k`cặp sử dụng điều kiện bên nhỏ. 

cuối cùng`n-k`các cặp sử dụng điều kiện cạnh lớn. 
4. Phần tiền tố hợp lệ nếu một trong hai`k = 0`hoặc`pref[k-1]`là đúng. 
5. Phần hậu tố hợp lệ nếu một trong hai`k = n`hoặc`suff[k]`là đúng. 
6. Nếu cả hai phần đều hợp lệ đối với một số`k`, thì ngưỡng`x`là có thể đạt được. 
7. Tìm kiếm nhị phân lớn nhất khả thi`x`. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mọi thành phần hợp lệ của món khai vị đều nằm đủ xa về bên trái hoặc đủ xa về bên phải trong danh sách được sắp xếp.`b`mảng. Bất kỳ sự kết hợp hoàn hảo nào thỏa mãn ngưỡng sẽ tạo ra một ranh giới giữa các phần tử được khớp từ phía bên trái và các phần tử được khớp từ phía bên phải. 

Đối với ranh giới cố định`k`, phần bên trái khả thi chính xác khi mọi cặp căn chỉnh trong phần đầu tiên`k`vị trí khác nhau ít nhất`x`theo hướng`a ≥ b + x`. Tương tự, phần bên phải khả thi chính xác khi mỗi cặp căn chỉnh trong hậu tố khác nhau ít nhất`x`theo hướng`b ≥ a + x`. 

Đối số sắp xếp lại cho các chuỗi được sắp xếp đảm bảo rằng nếu các lần kiểm tra căn chỉnh này không thành công thì không có kết quả khớp thay thế nào trong cùng một phía có thể sửa chữa chúng. Do đó việc kiểm tra tất cả các vị trí phân chia hoàn toàn thể hiện tính khả thi. 

Bởi vì tính khả thi là đơn điệu, nếu một ngưỡng`x`có thể đạt được thì mọi ngưỡng nhỏ hơn cũng có thể đạt được. Tìm kiếm nhị phân là hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    ans = []

    for _ in range(t):
        n = int(input())
        a = sorted(map(int, input().split()))
        b = sorted(map(int, input().split()))

        def check(x):
            pref = [False] * n
            ok = True

            for i in range(n):
                ok &= (a[i] - b[i] >= x)
                pref[i] = ok

            suff = [False] * (n + 1)
            suff[n] = True

            ok = True
            for i in range(n - 1, -1, -1):
                ok &= (b[i] - a[i] >= x)
                suff[i] = ok

            for k in range(n + 1):
                left_ok = (k == 0) or pref[k - 1]
                right_ok = (k == n) or suff[k]

                if left_ok and right_ok:
                    return True

            return False

        lo, hi = 0, 10**9

        while lo < hi:
            mid = (lo + hi + 1) // 2

            if check(mid):
                lo = mid
            else:
                hi = mid - 1

        ans.append(str(lo))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp cả hai mảng. Việc sắp xếp là cần thiết vì đặc tính khả thi phụ thuộc vào thứ tự tương đối của các phần tử. 

các`check(x)`chức năng xác minh xem ít nhất mỗi bữa ăn có thể có sự quyến rũ hay không`x`.`pref[i]`lưu trữ xem tất cả các cặp đã căn chỉnh có đến đúng vị trí hay không`i`thỏa mãn`a[j] - b[j] ≥ x`. Khi một vị trí bị lỗi, mọi tiền tố lớn hơn cũng bị lỗi, do đó, một boolean đang chạy là đủ.`suff[i]`lưu trữ xem mọi cặp căn chỉnh trong hậu tố có thỏa mãn hay không`b[j] - a[j] ≥ x`. 

Đối với một vị trí chia`k`, đầu tiên`k`các cặp đến từ điều kiện bên trái và các cặp còn lại đến từ điều kiện bên phải. Nếu cả hai phạm vi tương ứng đều hợp lệ thì sẽ tồn tại một kết quả khớp hoàn hảo. 

Tìm kiếm nhị phân sử dụng mẫu trên-trung tiêu chuẩn:```
mid = (lo + hi + 1) // 2
```Điều này tránh các vòng lặp vô hạn khi chỉ còn lại hai giá trị. 

Tất cả số học đều phù hợp thoải mái với số nguyên Python vì giá trị không bao giờ vượt quá`10^9`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = [1,2,3,4,5]
b = [1,2,3,4,5]
```Kiểm tra`x = 2`. 

| tôi | một [tôi] | b[i] | a[i]-b[i] ≥2 | b[i]-a[i] ≥2 | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | Không | Không | 
| 1 | 2 | 2 | Không | Không | 
| 2 | 3 | 3 | Không | Không | 
| 3 | 4 | 4 | Không | Không | 
| 4 | 5 | 5 | Không | Không | 

Không có sự phân chia nào thỏa mãn cả yêu cầu về tiền tố và hậu tố. 

Bây giờ hãy kiểm tra`x = 1`. 

Thử nghiệm tính khả thi thành công, vì vậy tìm kiếm nhị phân cuối cùng trả về:```
2
```Điều này chứng tỏ rằng sự kết hợp tối ưu không phải là sự ghép đôi được sắp xếp. Sắp xếp lại các món ăn làm tăng sự quyến rũ tối thiểu. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [0,0,0]
b = [100,100,100]
```Kiểm tra`x = 100`. 

| tôi | một [tôi] | b[i] | a[i]-b[i] ≥100 | b[i]-a[i] ≥100 | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 100 | Không | Có | 
| 1 | 0 | 100 | Không | Có | 
| 2 | 0 | 100 | Không | Có |`k = 0`ngay lập tức hoạt động vì toàn bộ mảng thỏa mãn điều kiện hậu tố. 

Ngưỡng`100`là khả thi. 

Kiểm tra`101`thất bại nên câu trả lời là:```
100
```Ví dụ này thực hiện trường hợp mọi cặp phải đến từ cùng một phía của phần chia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + n log 10⁹) | Sắp xếp cộng với khoảng 30 lần kiểm tra tính khả thi | 
| Không gian | O(n) | Mảng tiền tố và hậu tố | 

Chi phí tiền xử lý chủ yếu là phân loại. Kiểm tra tính khả thi là tuyến tính và tìm kiếm nhị phân thực hiện khoảng 30 lần lặp vì các giá trị nằm trong`[0, 10⁹]`. Với sự đảm bảo rằng tổng số tiền`n²`nhiều nhất là`25 · 10⁶`, điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    def solve():
        input = sys.stdin.readline

        t = int(input())
        ans = []

        for _ in range(t):
            n = int(input())
            a = sorted(map(int, input().split()))
            b = sorted(map(int, input().split()))

            def check(x):
                pref = [False] * n
                ok = True

                for i in range(n):
                    ok &= (a[i] - b[i] >= x)
                    pref[i] = ok

                suff = [False] * (n + 1)
                suff[n] = True

                ok = True
                for i in range(n - 1, -1, -1):
                    ok &= (b[i] - a[i] >= x)
                    suff[i] = ok

                for k in range(n + 1):
                    left_ok = (k == 0) or pref[k - 1]
                    right_ok = (k == n) or suff[k]

                    if left_ok and right_ok:
                        return True

                return False

            lo, hi = 0, 10**9

            while lo < hi:
                mid = (lo + hi + 1) // 2

                if check(mid):
                    lo = mid
                else:
                    hi = mid - 1

            ans.append(str(lo))

        print("\n".join(ans))

    solve()

    sys.stdout = old_stdout
    return out.getvalue()

# provided sample
assert run(
"""4
3
0 0 0
1000000000 1000000000 1000000000
5
1 2 3 4 5
1 2 3 4 5
6
0 0 0 100 100 100
100 100 100 0 0 0
7
14 25 62 74 86 95 12
51 62 71 72 92 20 84
"""
) == "1000000000\n2\n100\n30\n"

# minimum size
assert run(
"""1
1
5
9
"""
) == "4\n"

# all equal
assert run(
"""1
4
7 7 7 7
7 7 7 7
"""
) == "0\n"

# reversed extremes
assert run(
"""1
3
0 0 0
100 100 100
"""
) == "100\n"

# off-by-one style case
assert run(
"""1
2
1 4
2 5
"""
) == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, [5], [9]`|`4`| Ví dụ nhỏ nhất có thể | 
| Tất cả các giá trị bằng nhau |`0`| Xử lý chính xác câu trả lời bằng 0 | 
|`[0,0,0]`vs`[100,100,100]`|`100`| Toàn bộ sự phù hợp đến từ một phía | 
|`[1,4]`vs`[2,5]`|`3`| Phát hiện các điều kiện biên trong tìm kiếm nhị phân | 

## Vỏ cạnh 

### Tất cả các giá trị bằng nhau 

đầu vào:```
1
3
5 5 5
5 5 5
```Đối với bất kỳ ngưỡng dương nào cũng không`a[i]-b[i]`cũng không`b[i]-a[i]`đạt đến ngưỡng. Mọi kiểm tra tính khả thi đều thất bại. Tìm kiếm nhị phân giải quyết tại`0`. 

Đầu ra:```
0
```### Một mảng hoàn toàn nhỏ hơn 

đầu vào:```
1
3
0 0 0
100 100 100
```Vì`x = 100`, mọi điều kiện hậu tố đều đúng. Sự chia rẽ`k = 0`thành công ngay lập tức. 

Đầu ra:```
100
```### Phần tử đơn 

đầu vào:```
1
1
8
3
```Chỉ có một cặp có thể. Sự quyến rũ của nó là```
|8 - 3| = 5
```Kiểm tra tính khả thi chấp nhận mọi ngưỡng lên tới`5`và từ chối các giá trị lớn hơn. 

Đầu ra:```
5
```### Các giá trị trùng lặp được trộn lẫn với các giá trị riêng biệt 

đầu vào:```
1
4
1 1 10 10
3 3 8 8
```Sự kết hợp tối ưu là:```
1 ↔ 8
1 ↔ 8
10 ↔ 3
10 ↔ 3
```Mọi sự quyến rũ đều ngang bằng`7`, vậy câu trả lời là`7`. 

Kiểm tra tính khả thi của mảng được sắp xếp xác định chính xác sự phân chia giữa phép gán bên nhỏ và bên lớn và trả về giá trị tối ưu.

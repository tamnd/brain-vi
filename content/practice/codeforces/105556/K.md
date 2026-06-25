---
title: "CF 105556K - GCD của Bộ"
description: "Chúng ta được cho một tập hợp các số nguyên dương phân biệt. Tập hợp phải được phân chia thành đúng k tập con không rỗng. Mỗi tập hợp con đóng góp gcd của tất cả các số bên trong nó và chúng tôi muốn tổng tối đa có thể có của các giá trị gcd này."
date: "2026-06-25T06:09:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105556
codeforces_index: "K"
codeforces_contest_name: "The 6th FanRuan Cup Southeast University Programming Contest (Winter)"
rating: 0
weight: 105556
solve_time_s: 95
verified: true
draft: false
---

[CF 105556K - GCD của Bộ](https://codeforces.com/problemset/problem/105556/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên dương phân biệt. Bộ này phải được phân chia thành chính xác`k`tập hợp con không trống. Mỗi tập hợp con đóng góp gcd của tất cả các số bên trong nó và chúng tôi muốn tổng tối đa có thể có của các giá trị gcd này. 

Các con số là khác nhau, mỗi giá trị nhiều nhất là`10^6`và trên tất cả các ca kiểm thử, tổng giá trị lớn nhất xuất hiện trong mỗi ca kiểm thử nhiều nhất là`10^6`. Điều kiện cuối cùng đó là gợi ý thực sự. Nó gợi ý mạnh mẽ một giải pháp có hiệu quả trong khoảng`O(M log M)`mỗi trường hợp thử nghiệm, trong đó`M`là giá trị lớn nhất trong tập hợp. Một thuật toán bậc hai trong`n`sẽ là vô vọng khi`n`bản thân nó có thể đạt tới`10^6`. 

Phần khó khăn là phân vùng hoàn toàn không bị hạn chế. Không thể tìm kiếm trực tiếp trên các phân vùng, ngay cả đối với các đầu vào rất nhỏ. 

Một sai lầm phổ biến là tập trung vào chính các giá trị gcd và cố gắng xây dựng các tập hợp con một cách tham lam. Cấu trúc phân vùng thực sự đơn giản hơn nhiều so với lần đầu tiên nó xuất hiện. 

Hãy xem xét tập hợp`{3, 5, 6}`với`k = 2`. 

Nếu chúng ta chọn tập hợp con`{5}`Và`{3, 6}`, tổng là:`5 + gcd(3, 6) = 5 + 3 = 8`Điều này là tối ưu. 

Trường hợp dễ bỏ sót thứ hai là khi một số số chia hết cho số khác. 

Vì`{5, 9, 10, 11, 13}`với`k = 4`, đặt`10`cùng với`5`có lợi vì tập hợp con gcd trở thành`5`. Phân vùng tối ưu là:`{13}, {11}, {9}, {5, 10}`cho đi`13 + 11 + 9 + 5 = 38`Một chiến lược ngây thơ luôn cô lập những số lớn nhất không giải thích được tại sao`10`nên được hợp nhất thay vì`5`. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Liệt kê tất cả các cách phân chia tập hợp thành`k`các tập hợp con không trống, tính gcd của mỗi tập hợp con và giữ lại câu trả lời đúng nhất. 

Điều này đúng vì nó kiểm tra mọi phân vùng hợp lệ. Vấn đề là số lượng phân vùng tăng theo cấp số nhân. Ngay cả đối với vài chục phần tử thì điều đó cũng là không thể. 

Quan sát quan trọng xuất phát từ việc hỏi điều gì xảy ra bên trong một phân vùng tối ưu. 

Giả sử một tập hợp con chứa ít nhất hai phần tử và gcd của nó là`g`. Chọn bất kỳ phần tử nào`x`trong tập con đó không phải là tập nhỏ nhất. Từ`g`chia mọi phần tử trong tập con, ta có`g ≤ x`. 

Nếu chúng ta loại bỏ`x`từ tập hợp con và biến nó thành một tập hợp con đơn lẻ, phần đóng góp sẽ thay đổi từ`g`ĐẾN`g + x`. 

Tổng số tiền tăng ít nhất`x - g ≥ 0`. 

Điều này có nghĩa là, bất cứ khi nào chúng ta được phép tạo thêm tập hợp con, việc tách các phần tử không bao giờ có hại. 

Vì chúng ta phải kết thúc bằng chính xác`k`các tập hợp con, một phân vùng tối ưu luôn có:`k - 1`các tập con đơn lẻ và một tập con còn lại chứa tập con kia`n - k + 1`các phần tử. 

Cho phép`m = n - k + 1`. 

Nếu tập con còn lại là`R`, thì câu trả lời sẽ trở thành`sum(all elements) - sum(R) + gcd(R)`. 

Tổng của tất cả các số là cố định nên ta chỉ cần tối đa hóa`gcd(R) - sum(R)`,

Ở đâu`|R| = m`. 

Bây giờ hãy sửa một giá trị gcd có thể`d`. 

Nếu như`gcd(R) = d`, mọi phần tử của`R`phải chia hết cho`d`. 

Trong số tất cả các yếu tố chia hết cho`d`, sự lựa chọn tốt nhất rõ ràng là`m`những cái nhỏ nhất, bởi vì thuật ngữ`-sum(R)`nên càng lớn càng tốt. 

Vậy với mỗi ước số`d`, nếu ít nhất`m`các số trong tập hợp đó có thể chia hết cho`d`, chúng tôi tính toán`d - (sum of the m smallest present multiples of d)`. 

Giá trị tốt nhất như vậy trên tất cả`d`đưa ra thuật ngữ hiệu chỉnh tối ưu. 

Cấu trúc của các ràng buộc làm cho giải pháp sàng chia trở nên tự nhiên. Đối với mỗi`d`, chúng tôi quét bội số của nó theo thứ tự tăng dần. Vì bội số được truy cập theo thứ tự số tăng dần, nên số đầu tiên`m`bội số hiện tại chính xác là`m`số chia nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(M log M) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tập hợp và tính tổng`S`của tất cả các phần tử. 
2. Hãy để`m = n - k + 1`. 

Đây là kích thước của tập hợp con không đơn lẻ trong một phân vùng tối ưu. 
3. Xây dựng một mảng hiện diện cho tất cả các giá trị lên đến giá trị tối đa trong tập hợp. 
4. Với mọi ước số có thể`d`từ`1`ĐẾN`max_value`: 

Quét tất cả bội số của`d`theo thứ tự tăng dần. 
5. Bất cứ khi nào có bội số trong tập hợp, hãy cộng nó vào tổng hiện có và tăng bộ đếm. 

Vì bội số được xử lý theo thứ tự tăng dần nên số đầu tiên`m`bội số hiện tại chính xác là`m`phần tử chia nhỏ nhất. 
6. Khi quầy đạt đến`m`, tính toán`candidate = d - current_sum`. 

Cập nhật giá trị tốt nhất. 
7. Câu trả lời cuối cùng là`S + best`. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là mọi phân vùng tối ưu đều có thể được chuyển đổi thành một phân vùng có chính xác`k - 1`tập hợp con đơn lẻ và một tập hợp con lớn hơn mà không làm giảm câu trả lời. 

Sau phép biến đổi đó, mọi nghiệm được mô tả duy nhất bởi tập`R`kích thước`m = n - k + 1`. 

Đóng góp của nó là`S - sum(R) + gcd(R)`. 

Đối với giá trị gcd cố định`d`, mọi phần tử của`R`phải chia hết cho`d`. Để tối đa hóa`d - sum(R)`, 

chúng ta phải giảm thiểu`sum(R)`, có nghĩa là chọn`m`số nhỏ nhất có thể chia hết cho`d`. 

Kiểm tra mọi ước số`d`bao gồm mọi gcd có thể. Giá trị tốt nhất được tìm thấy chính xác là giá trị tối đa có thể đạt được của`gcd(R) - sum(R)`. 

Do đó thuật toán luôn trả về câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        S = sum(a)
        m = n - k + 1
        mx = max(a)

        present = [0] * (mx + 1)
        for x in a:
            present[x] = 1

        best = -10**18

        for d in range(1, mx + 1):
            cnt = 0
            cur_sum = 0

            for multiple in range(d, mx + 1, d):
                if present[multiple]:
                    cnt += 1
                    cur_sum += multiple

                    if cnt == m:
                        value = d - cur_sum
                        if value > best:
                            best = value
                        break

        ans.append(str(S + best))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính số lượng cố định`S`, tổng của tất cả các phần tử. 

giá trị`m = n - k + 1`xuất phát trực tiếp từ bằng chứng cấu trúc rằng một phân vùng tối ưu bao gồm một tập hợp con lớn hơn và`k - 1`tập hợp con đơn lẻ. 

Mảng hiện diện cho phép kiểm tra liên tục theo thời gian để xem liệu một giá trị có thuộc tập hợp hay không. Bởi vì tất cả các giá trị đều khác biệt nên một mảng boolean là đủ. 

Với mỗi số chia`d`, chúng ta đi qua bội số của nó. Thứ tự duyệt ngày càng tăng nên điều đầu tiên`m`bội số hiện tại tự động là`m`số chia nhỏ nhất. Không cần sắp xếp. 

Khoảnh khắc chúng tôi đạt được`m`các phần tử, chúng ta đã biết tổng tập hợp con tối ưu cho ước số này, vì vậy chúng ta có thể ngừng quét các bội số lớn hơn của`d`. 

Tất cả số học đều phù hợp thoải mái với số nguyên Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
k = 2
set = {3, 5, 6}
```Đây:`S = 14`

`m = 2`| d | 2 bội số hiện tại đầu tiên | Tổng hợp | d - Tổng | 
| --- | --- | --- | --- | 
| 1 | 3, 5 | 8 | -7 | 
| 2 | 6 | không đủ | - | 
| 3 | 3, 6 | 9 | -6 | 
| 4 | không | không đủ | - | 
| 5 | 5 | không đủ | - | 
| 6 | 6 | không đủ | - | 

Giá trị tốt nhất là`-6`. 

Trả lời:`14 + (-6) = 8`Điều này tương ứng với việc chọn`R = {3, 6}`. Phân vùng là`{5}`Và`{3, 6}`. 

### Ví dụ 2 

đầu vào:```
n = 5
k = 4
set = {5, 9, 10, 11, 13}
```Đây:`S = 48`

`m = 2`| d | 2 bội số hiện tại đầu tiên | Tổng hợp | d - Tổng | 
| --- | --- | --- | --- | 
| 1 | 5, 9 | 14 | -13 | 
| 2 | 10 | không đủ | - | 
| 5 | 5, 10 | 15 | -10 | 
| 9 | 9 | không đủ | - | 

Giá trị tốt nhất là`-10`. 

Trả lời:`48 + (-10) = 38`Tập được chọn là`R = {5, 10}`. Các số còn lại trở thành tập con đơn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log M) | Bộ chia số chuỗi hài trên các giá trị lên tới`M`| 
| Không gian | O(M) | Mảng hiện diện có kích thước`M + 1`| 

Đây`M`là giá trị tối đa trong trường hợp thử nghiệm hiện tại. 

Bởi vì tổng của tất cả các cực đại của ca kiểm thử nhiều nhất là`10^6`, tổng công việc trên toàn bộ đầu vào vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    def solve():
        input = sys.stdin.readline
        t = int(input())

        res = []

        for _ in range(t):
            n, k = map(int, input().split())
            a = list(map(int, input().split()))

            S = sum(a)
            m = n - k + 1
            mx = max(a)

            present = [0] * (mx + 1)
            for x in a:
                present[x] = 1

            best = -10**18

            for d in range(1, mx + 1):
                cnt = 0
                cur = 0

                for x in range(d, mx + 1, d):
                    if present[x]:
                        cnt += 1
                        cur += x

                        if cnt == m:
                            best = max(best, d - cur)
                            break

            res.append(str(S + best))

        print("\n".join(res))

    solve()

    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided samples
assert run(
"""4
3 2
3 6 5
5 4
13 11 10 9 5
4 2
4 2 5 3
4 3
4 2 5 3
"""
) == """8
38
6
10"""

# custom cases
assert run(
"""1
1 1
7
"""
) == "7"

assert run(
"""1
2 1
4 8
"""
) == "4"

assert run(
"""1
2 2
4 8
"""
) == "12"

assert run(
"""1
3 2
2 4 8
"""
) == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7`|`7`| Kích thước tối thiểu | 
|`2 1 / 4 8`|`4`| Tập hợp con đơn, gcd của toàn bộ | 
|`2 2 / 4 8`|`12`| Tất cả các tập con đơn lẻ | 
|`3 2 / 2 4 8`|`10`| Tập hợp con còn lại tốt nhất sử dụng tính chia hết | 

## Vỏ cạnh 

Hãy xem xét:```
1
3 2
3 5 6
```Một cách tiếp cận hấp dẫn là cô lập phần tử lớn nhất và giả sử phần còn lại đóng góp phần tử tối thiểu của nó. Tập hợp con còn lại`{3, 6}`đóng góp`3`, không phải là mức tối thiểu một cách ngẫu nhiên mà vì gcd của nó chính xác là`3`. Thuật toán kiểm tra số chia`3`, tìm hai phần tử chia hết đầu tiên`{3, 6}`, và nhận được câu trả lời đúng`8`. 

Bây giờ hãy xem xét:```
1
5 4
13 11 10 9 5
```Giải pháp tốt nhất không thể thu được bằng cách nhóm hai số nhỏ nhất. Tập con tối ưu còn lại là`{5, 10}`bởi vì gcd của nó là`5`. Khi thuật toán xử lý số chia`5`, nó ngay lập tức tìm thấy chính xác hai bội số hiện tại, cho giá trị`5 - (5 + 10) = -10`, tốt hơn mọi ước số khác. 

Cuối cùng:```
1
4 2
2 3 4 5
```Câu trả lời tối ưu là`6`. Thuật toán chọn`R = {2, 3, 4}`bởi vì trong số tất cả các tập hợp con cỡ 3, nó tối đa hóa`gcd(R) - sum(R)`. Phân vùng kết quả là`{5}`Và`{2, 3, 4}`, cho`5 + 1 = 6`. Điều này xác nhận rằng tập hợp con còn lại không bắt buộc phải có gcd lớn hơn`1`.

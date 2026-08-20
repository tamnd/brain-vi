---
title: "CF 102220B - Chế độ ăn uống cân bằng"
description: "Chúng tôi có (m) loại đồ ngọt và (n) loại đồ ngọt riêng lẻ. Mỗi loại kẹo có giá trị dương (ai) và thuộc về một loại (bi). Với mỗi loại (j), đều có một ngưỡng (lj). Nếu chúng ta mua bất kỳ loại kẹo nào thuộc loại đó, chúng ta phải mua ít nhất (lj) loại đó. Mua số không cũng được cho phép."
date: "2026-08-19T00:14:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102220
codeforces_index: "B"
codeforces_contest_name: "The 13th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102220
solve_time_s: 284
verified: true
draft: false
---

[CF 102220B - Chế độ ăn uống cân bằng](https://codeforces.com/problemset/problem/102220/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 44 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (m) loại đồ ngọt và (n) loại đồ ngọt riêng lẻ. Mỗi loại kẹo có giá trị dương (a_i) và thuộc một loại (b_i). Với mỗi loại (j), đều có một ngưỡng (l_j). Nếu chúng ta mua bất kỳ loại kẹo nào thuộc loại đó thì chúng ta phải mua ít nhất (l_j) trong số đó. Mua số không cũng được cho phép. 

Đối với tập hợp đã chọn, gọi (S) là tổng của tất cả các giá trị đã mua và gọi (C) là số lượng kẹo được mua lớn nhất thuộc bất kỳ loại nào. Điểm số là (S/C). Chúng tôi cần số điểm tối đa có thể, được biểu thị dưới dạng phân số rút gọn. 

Các ràng buộc làm cho cấu trúc dự định khá rõ ràng. Một ca kiểm thử có thể chứa (10^5) viên kẹo, trong khi tổng (n) và (m) trên tất cả các ca kiểm thử nhiều nhất là (10^6). Một thuật toán (O(n^2)) hoặc hàm mũ ngay lập tức là không thể. Ngay cả phương thức (O(nm)) cũng có thể đạt tới (10^{10}) hoạt động trong một trường hợp lớn. Chúng ta cần khoảng (O(n\log n+m)), với hệ số logarit đến từ việc sắp xếp. 

Có một số trường hợp đặc biệt có thể khiến một giải pháp có vẻ hợp lý trở nên sai lầm. Đầu tiên, một loại có ngưỡng lớn hơn số lượng kẹo có sẵn sẽ không thể đóng góp chút nào. Ví dụ,```
1
2 1
2
5 1
1 1
```Lựa chọn hợp lệ duy nhất là cả hai loại kẹo, vì vậy câu trả lời là (6/2=3/1). Chỉ lấy đồ ngọt có giá trị 5 sẽ cho điểm số lớn hơn, nhưng lựa chọn đó vi phạm ngưỡng. 

Trường hợp cạnh thứ hai là (l_j=1). Trong tình huống đó, chúng ta có thể lấy bất kỳ số lượng kẹo dương nào từ loại đó, bao gồm tất cả chúng. Ví dụ,```
1
3 2
1 3
10 1
9 1
1 2
```Chỉ lấy giá trị 10 ngọt sẽ cho (10/1), là mức tối ưu. Một giải pháp giả định mọi loại được chọn đều phải đóng góp chính xác ngưỡng của nó sẽ bỏ lỡ khả năng này. 

Điểm tinh tế thứ ba là tham số (k) được sử dụng trong quá trình tối ưu hóa không nhất thiết phải là số lượng tối đa thực tế của tập hợp kết quả. Ví dụ,```
1
2 2
1 1
10 1
1 2
```Với (k=2), chúng ta có thể lấy cả hai chiếc kẹo và thu được (11/2) khi chia cho giới hạn áp đặt (k), mặc dù số lượng tối đa thực tế chỉ là (1). Bộ tương tự có điểm (11/1), được tính khi (k=1). Vì vậy, chúng tôi được phép tối ưu hóa với điều kiện mỗi loại có tối đa (k) số kẹo được chọn, thay vì nhấn mạnh rằng một số loại có chính xác (k). 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng ta có thể liệt kê mọi tập hợp con của (n) kẹo, đếm xem nó chứa bao nhiêu kẹo, từ chối các tập hợp con vi phạm ngưỡng, tính toán (S), tính toán (C) và tối đa hóa (S/C). Điều này đúng vì mọi giao dịch mua có thể được biểu thị bằng chính xác một tập hợp con. Thật không may, có (2^n) tập hợp con, tức là khoảng (10^{30103}) khi (n=100000). Ngay cả trước khi xem xét công việc cần thiết để đánh giá từng tập hợp con, điều này hoàn toàn không khả thi. 

Cách hữu ích để sắp xếp lại vấn đề là ngừng chọn các tập hợp con riêng lẻ và thay vào đó sửa mẫu số. Giả sử chúng ta quyết định rằng không loại nào có thể đóng góp nhiều hơn (k) kẹo. Hãy xem xét một loại chứa (các) loại kẹo, được sắp xếp theo giá trị từ lớn nhất đến nhỏ nhất là (v_1,v_2,\ldots,v_s). 

Nếu (s<l), loại này không bao giờ có thể được chọn, bởi vì việc chọn dù chỉ một món ngọt cũng sẽ vi phạm mức tối thiểu. Nếu (s\ge l) nhưng (k<l), loại cũng không thể được chọn theo giới hạn hiện tại (k). Nếu (l\le k), lựa chọn tốt nhất là lấy giá trị (\min(k,s)) lớn nhất. Tất cả các giá trị đều dương, do đó không bao giờ có lý do để loại bỏ đồ ngọt có giá trị cao được phép. 

Gọi (F(k)) là tổng giá trị lớn nhất có thể đạt được khi mỗi loại đóng góp tối đa (k) kẹo. Đối với loại có các giá trị được sắp xếp (v_1,\ldots,v_s), đóng góp của nó là 

[ 
0 \quad\text{for } k<l, 
] 

và 

[ 
v_1+\cdots+v_k 
] 

for (l\le k\le s, trong khi đó là tổng của loại cho (k>s). 

Vấn đề còn lại là tính (F(k)) cho mọi (k) mà không cần tính tổng các tiền tố nhiều lần. Quan sát quan trọng là phần đóng góp thay đổi rất đơn giản khi (k) tăng lên. Tại (k=l), loại đột nhiên đóng góp giá trị lớn nhất (l) đầu tiên. Khi (k) tăng từ (k-1) lên (k), phần đóng góp sẽ tăng chính xác (v_k), cho đến khi bao gồm tất cả các loại kẹo. 

Vì vậy, đối với mọi loại, chúng ta có thể thêm các thay đổi của nó vào một mảng. Nếu ngưỡng của nó là (l), chúng ta cộng tổng các giá trị (l) lớn nhất của nó vào`gain[l]`. Đối với mọi vị trí sau (k), tùy theo kích thước của loại, chúng tôi thêm (v_k) vào`gain[k]`. Tổng tiền tố`gain`sau đó cho (F(k)) với mọi (k). 

Chúng ta chỉ cần sắp xếp kẹo theo loại và sau đó theo giá trị giảm dần. Tổng số thao tác trên mỗi vị trí là tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n+m+n)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ngưỡng (l_j) cho mọi loại. Lưu trữ các ngưỡng để có thể nhận được ngưỡng của một loại ngay lập tức trong khi xử lý đồ ngọt của nó. 
2. Sắp xếp tất cả đồ ngọt theo loại và sắp xếp bên trong mỗi loại theo giá trị giảm dần. Điều này đặt mọi loại vào một phân khúc liền kề, trước tiên là những loại kẹo có giá trị nhất. Chúng ta không cần phải xây dựng các danh sách Python riêng biệt cho tất cả các loại (m), điều này rất hữu ích vì (m) cũng có thể là (10^5). 
3. Tạo một mảng`gain`, Ở đâu`gain[k]`biểu thị mức tăng của (F(k)) khi số lượng tối đa được phép thay đổi từ (k-1) thành (k). 
4. Xử lý từng loại một. Giả sử các giá trị được sắp xếp của nó là (v_1,\ldots,v_s) và ngưỡng của nó là (l). Duy trì tổng tiền tố đang chạy. 
5. Khi số lượng kẹo đã chế biến đạt (l), hãy cộng tổng tiền tố hiện tại vào`gain[l]`. Đây là điểm đầu tiên mà loại này có thể được chọn, do đó toàn bộ đóng góp hợp lệ tốt nhất của nó sẽ xuất hiện cùng một lúc. 
6. Với mỗi vị ngọt tiếp theo (v_k), hãy thêm (v_k) vào`gain[k]`trong khi (k\le s). Việc tăng số lượng được phép từ (k-1) lên (k) cho phép chúng tôi lấy chính xác loại kẹo lớn nhất tiếp theo này từ loại. 
7. Bỏ qua loại có số lượng kẹo có sẵn nhỏ hơn ngưỡng của nó. Không có lựa chọn tích cực hợp lệ nào từ loại như vậy, vì vậy nó không đóng góp gì cho mọi (k). 
8. Chuyển đổi`gain`vào (F) bằng cách lấy tổng tiền tố. Sau hoạt động này,`gain[k]`thực tế là (F(k)), tổng giá trị tốt nhất theo số lượng tối đa trên mỗi loại là (k). 
9. Xét mọi (k) từ (1) đến (n). So sánh (F(k)/k) với phân số tốt nhất được tìm thấy cho đến nay bằng cách sử dụng phép nhân chéo, do đó không cần số học dấu phẩy động. 
10. Rút gọn tử số và mẫu số tốt nhất bằng ước số chung lớn nhất của chúng và in phân số thu được. 

### Tại sao nó hoạt động 

Sửa mọi số nguyên dương (k). Một lựa chọn hợp lệ có mỗi loại xuất hiện nhiều nhất (k) lần có thể được tối ưu hóa độc lập theo loại vì không có sự tương tác giữa các giá trị của các loại khác nhau ngoại trừ giới hạn trên chung (k). Đối với loại có sẵn ít nhất (l) đồ ngọt, việc chọn ít hơn (l) bị cấm và cấm chọn nhiều hơn (k). Vì mọi giá trị đều dương nên số lượng được phép tốt nhất là (\min(k,s)) và kẹo tốt nhất chính xác là kẹo lớn nhất. 

các`gain`việc xây dựng thể hiện chính xác sự đóng góp tối ưu này thay đổi như thế nào khi (k) phát triển. Trước (l), đóng góp bằng không. Tại (l), các giá trị (l) đầu tiên sẽ có sẵn cùng nhau. Mỗi lần tăng sau đó của (k) sẽ thêm giá trị lớn nhất tiếp theo. Do đó tổng tiền tố của`gain`chính xác là (F(k)). 

Bây giờ hãy xem xét việc mua hàng tối ưu với số lượng tối đa thực tế (C). Nó khả thi dưới giới hạn (k=C), vì vậy (F(C)) ít nhất là tổng giá trị của nó. Do đó (F(C)/C) ít nhất là điểm tối ưu. Ngược lại, một tập hợp được xây dựng dưới giới hạn (k) có số lượng tối đa thực tế nhiều nhất là (k), do đó điểm thực của nó ít nhất là (F(k)/k). Do đó, việc kiểm tra tất cả (k) không thể bỏ sót giá trị tối ưu và không thể tạo ra giá trị lớn hơn giá trị tối ưu thực sự. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

MAX_A = 100_000_000
SHIFT = 27

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        need = [0] + list(map(int, input().split()))

        # Encode (type, value) into one integer.
        # Higher bits contain the type.
        # Lower bits contain MAX_A - value, so sorting ascending
        # gives the values of each type in decreasing order.
        sweets = [0] * n
        for i in range(n):
            a, b = map(int, input().split())
            sweets[i] = (b << SHIFT) | (MAX_A - a)

        sweets.sort()

        gain = [0] * (n + 1)

        i = 0
        while i < n:
            type_id = sweets[i] >> SHIFT
            limit = need[type_id]

            j = i + 1
            while j < n and (sweets[j] >> SHIFT) == type_id:
                j += 1

            prefix = 0
            count = 0

            for p in range(i, j):
                value = MAX_A - (sweets[p] & ((1 << SHIFT) - 1))
                count += 1
                prefix += value

                if count == limit:
                    gain[count] += prefix
                elif count > limit:
                    gain[count] += value

            i = j

        # gain[k] is the increment when changing the bound
        # from k-1 to k. Convert it to F(k).
        for k in range(1, n + 1):
            gain[k] += gain[k - 1]

        best_num = gain[1]
        best_den = 1

        for k in range(2, n + 1):
            if gain[k] * best_den > best_num * k:
                best_num = gain[k]
                best_den = k

        d = gcd(best_num, best_den)
        out.append(f"{best_num // d}/{best_den // d}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mảng ngưỡng được lập chỉ mục theo loại, vì vậy`need[type_id]`đưa ra số lượng kẹo tối thiểu phải lấy nếu sử dụng loại đó. Loại có quá ít đồ ngọt đương nhiên sẽ không bao giờ đạt đến ngưỡng trong vòng lặp bên trong nên không đóng góp được gì. 

Mã hóa số nguyên được sử dụng để tránh lưu trữ (n) bộ dữ liệu Python. Loại chiếm các bit cao, trong khi`MAX_A - value`chiếm 27 bit thấp. Vì (10^8<2^{27}), giá trị phù hợp một cách an toàn ở các bit thấp đó. Việc sắp xếp các số nguyên được mã hóa sẽ sắp xếp trước theo loại và sau đó theo giá trị giảm dần. 

Đối với mỗi phân đoạn loại liền kề,`prefix`là tổng số kẹo lớn nhất hiện nay. Chính xác`limit`, loại sẽ có thể được chọn, do đó toàn bộ tiền tố sẽ được thêm vào`gain[limit]`. Mỗi vị ngọt sau này chỉ đóng góp giá trị riêng của nó vì tiền tố trước đó đã được tính đến. 

Bước thứ hai biến số gia tăng thành giá trị thực tế của (F(k)). Công dụng so sánh phân số`gain[k] * best_den > best_num * k`, đó là chính xác. Tổng giá trị lớn nhất có thể lớn nhất là (10^6\cdot10^8=10^{14}), do đó số nguyên Python dễ dàng xử lý mọi số học. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, có một loại có ngưỡng (2), chứa các giá trị (7) và (2). Loại này có thể sử dụng được tại (k=2), trong đó đóng góp của nó là (9). 

| k | Giá trị đã xử lý | Gain[k] trước tiền tố | F(k) | F(k)/k | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 0 | 0 | 0 | 
| 2 | 7, 2 | 9 | 9 | 2/9 | 

Tối đa là (9/2). Sự chuyển đổi tại (k=2) chứng tỏ tại sao ngưỡng đóng góp được thêm vào dưới dạng toàn bộ tiền tố thay vì chỉ giá trị cuối cùng. 

Đối với Mẫu 2, loại 1 có ngưỡng (1) và giá trị (2). Loại 2 có ngưỡng (2) và giá trị (5,3,1). Sau khi sắp xếp, loại 2 được xử lý là (5,3,1). 

| k | Tăng loại 1 | tăng loại 2 | F(k) | F(k)/k | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 | 2 | 
| 2 | 0 | 8 | 10 | 5 | 
| 3 | 0 | 1 | 11 | 3/11 | 

Giá trị tốt nhất là (5). Giao dịch mua tương ứng bao gồm giá trị kẹo loại 1 (2) và hai giá trị kẹo loại 2 tốt nhất (5) và (3), cho tổng giá trị (10) và số loại tối đa (2). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n+m+n)) | Việc sắp xếp chiếm ưu thế, trong khi việc nhóm, xây dựng độ lợi, tổng tiền tố và so sánh phân số là tuyến tính | 
| Không gian | (O(n+m)) | Mỗi mảng kẹo, mảng ngưỡng và mảng khuếch đại được mã hóa đều sử dụng không gian tuyến tính | 

Các trường hợp thử nghiệm lớn nhất chứa (10^5) kẹo và tổng số kẹo trong tất cả các trường hợp thử nghiệm nhiều nhất là (10^6). Thuật toán thực hiện một lần sắp xếp toàn cục cho mỗi trường hợp thử nghiệm và sau đó chỉ quét tuyến tính. Mức sử dụng bộ nhớ nằm trong giới hạn 512 MB đã nêu, trong khi các số nguyên có độ chính xác tùy ý của Python xử lý tổng số khoảng (10^{14} một cách an toàn). 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

MAX_A = 100_000_000
SHIFT = 27

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        need = [0] + list(map(int, input().split()))

        sweets = [0] * n
        for i in range(n):
            a, b = map(int, input().split())
            sweets[i] = (b << SHIFT) | (MAX_A - a)

        sweets.sort()

        gain = [0] * (n + 1)
        mask = (1 << SHIFT) - 1

        i = 0
        while i < n:
            type_id = sweets[i] >> SHIFT
            limit = need[type_id]

            j = i + 1
            while j < n and (sweets[j] >> SHIFT) == type_id:
                j += 1

            prefix = 0
            count = 0

            for p in range(i, j):
                value = MAX_A - (sweets[p] & mask)
                count += 1
                prefix += value

                if count == limit:
                    gain[count] += prefix
                elif count > limit:
                    gain[count] += value

            i = j

        for k in range(1, n + 1):
            gain[k] += gain[k - 1]

        best_num = gain[1]
        best_den = 1

        for k in range(2, n + 1):
            if gain[k] * best_den > best_num * k:
                best_num = gain[k]
                best_den = k

        d = gcd(best_num, best_den)
        out.append(f"{best_num // d}/{best_den // d}")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
sample = """\
2
2 1
2
7 1
2 1
3 2
1 2
2 1
5 2
3 2
"""
assert run(sample) == "9/2\n5/1", "provided samples"

# Minimum-size input
assert run("""\
1
1 1
1
7 1
""") == "7/1", "minimum size"

# A type cannot be partially selected when its threshold is 2
assert run("""\
1
2 1
2
5 1
1 1
""") == "3/1", "threshold boundary"

# All values are equal and every type can be selected from one sweet upward
assert run("""\
1
4 2
1 1
7 1
7 1
7 2
7 2
""") == "14/1", "all equal values"

# The first valid k is exactly the threshold.
# Values are 10, 9, 1 and l = 2, so F(2) = 19.
assert run("""\
1
3 1
2
10 1
9 1
1 1
""") == "19/2", "off by one at threshold"

# Maximum n and m. Every type has exactly one sweet and l = 1.
# All sweets can be selected, while C = 1.
n = 100000
m = 100000
thresholds = " ".join(["1"] * m)
items = "\n".join(f"1 {i}" for i in range(1, m + 1))
maximum_case = f"1\n{n} {m}\n{thresholds}\n{items}\n"

assert run(maximum_case) == "100000/1", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 1 / 7 1`|`7/1`| Đầu vào và mẫu số kích thước tối thiểu (1) | 
|`2 1 / l=2 / values 5,1`|`3/1`| Một loại không thể được chọn một phần | 
| Bốn giá trị bằng nhau được chia thành hai loại với (l=1) |`14/1`| Các giá trị hoàn toàn bằng nhau và lấy mọi đồ ngọt có sẵn | 
| Một loại, giá trị (10,9,1), (l=2) |`19/2`| Chuyển đổi ngưỡng chính xác và xử lý từng cái một | 
| (n=m=100000), một giá trị cho mỗi loại, tất cả (l=1) |`100000/1`| Đầu vào có kích thước tối đa và xử lý hậu kỳ tuyến tính | 

## Vỏ cạnh 

Khi một loại có ít đồ ngọt hơn ngưỡng của nó, thuật toán sẽ không bao giờ đạt được`count == limit`, vậy nó`gain`đóng góp vẫn bằng không. Vì```
1
2 1
2
5 1
1 1
```loại duy nhất có hai viên kẹo và ngưỡng hai. Tại`count=1`, không có gì được thêm vào. Tại`count=2`, tiền tố là (5+1=6), vì vậy`gain[2]=6`. Kết quả (F(2)=6), cho ra (6/2=3/1). Một chiếc kẹo duy nhất không bao giờ được coi là hợp lệ. 

Khi (l=1), chiếc kẹo đầu tiên ngay lập tức đóng góp ở mức`gain[1]`và mỗi vị ngọt sau đó sẽ thêm giá trị của chính nó vào (k) lớn hơn tương ứng. Vì```
1
3 2
1 3
10 1
9 1
1 2
```loại 1 đóng góp (10) tại (k=1), trong khi loại 2 không thể đóng góp cho đến khi (k=3). Do đó (F(1)=10), và đáp án là (10/1). Thuật toán cho phép chọn loại đầu tiên một cách chính xác mà không cần thêm bất kỳ đồ ngọt nào. 

Khi tập hợp đã chọn có số lượng tối đa thực tế nhỏ hơn giới hạn hiện tại (k), phép tính vẫn an toàn. Coi như```
1
2 2
1 1
10 1
1 2
```Đối với (k=2), cả hai loại đều có thể đóng góp một vị ngọt, do đó (F(2)=11), tạo ra ứng cử viên (11/2). Tập được chọn thực tế có số lượng tối đa (1), do đó điểm thực của nó là (11/1). Thuật toán cũng đánh giá (k=1), trong đó (F(1)=11) và do đó tìm ra câu trả lời đúng (11/1). Giới hạn (k) chỉ là một công cụ để liệt kê các khả năng, không phải là khẳng định rằng một số loại phải chứa chính xác (k) kẹo. 

Cuối cùng, quá trình chuyển đổi ngưỡng phải được xử lý mà không xảy ra lỗi nào. Vì```
1
3 1
2
10 1
9 1
1 1
```các giá trị được sắp xếp là (10,9,1). Tại (k=1), loại này không khả dụng vì ngưỡng của nó là (2), do đó (F(1)=0). Tại (k=2), tiền tố (10+9=19) được thêm vào cùng một lúc, cho ra (F(2)=19). Tại (k=3), chỉ có giá trị phụ (1) được thêm vào, cho ra (F(3)=20). Các tỷ số là (0), (19/2) và (20/3), nên đáp án là (19/2). Đây chính xác là hành vi được mã hóa bằng cách thêm toàn bộ tiền tố ở ngưỡng và chỉ giá trị riêng lẻ tiếp theo sau đó.

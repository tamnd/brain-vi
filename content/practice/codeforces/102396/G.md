---
title: "CF 102396G - Tràn trọng lượng"
description: "Chúng ta cần đặt một số vật nặng đã cho lên hai tấm. Trọng lượng có thể đặt lên tấm đầu tiên, tấm thứ hai hoặc không được sử dụng. Thang đo không so sánh số tiền thực tế."
date: "2026-08-10T18:48:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102396
codeforces_index: "G"
codeforces_contest_name: "2019-2020 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 19)"
rating: 0
weight: 102396
solve_time_s: 803
verified: false
draft: false
---

[CF 102396G - Tràn trọng lượng](https://codeforces.com/problemset/problem/102396/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đặt một số vật nặng đã cho lên hai tấm. Trọng lượng có thể đặt lên tấm đầu tiên, tấm thứ hai hoặc không được sử dụng. Thang đo không so sánh số tiền thực tế. Thay vào đó, nó làm giảm modulo tổng khối lượng của mỗi tấm`m`và báo cáo cân bằng khi hai phần dư đó bằng nhau. 

Giả sử tấm đầu tiên nhận được trọng lượng với tổng`S1`và thứ hai nhận được trọng lượng với tổng số`S2`. Chúng tôi cần`S1 ≡ S2 (mod m)`với ít nhất một trọng lượng thực sự được đặt. Tương tự, với mỗi trọng số chúng ta có thể chọn một hệ số từ`{-1, 0, 1}`và cần`c1*a1 + c2*a2 + ... + cn*an ≡ 0 (mod m)`,

Ở đâu`1`có nghĩa là trọng lượng ở tấm đầu tiên,`-1`có nghĩa là tấm thứ hai, và`0`có nghĩa là chưa sử dụng. 

Ràng buộc`n <= 25`là đầu mối trung tâm. Có ba lựa chọn cho mỗi trọng lượng, vì vậy việc tìm kiếm trực tiếp có`3^25 = 847,288,609`bài tập. Điều đó vượt xa giới hạn một giây cho phép. Mô đun có thể gần như`4 * 10^7`, do đó một thuật toán tỷ lệ thuận với`m`cũng lớn một cách không cần thiết và việc tìm kiếm bậc hai trên tất cả các phép gán là không thể. Giá trị nhỏ của`n`thay vào đó đề xuất chia trọng số thành hai nhóm và liệt kê các khả năng của chúng một cách độc lập. 

Quần chúng có thể lớn như`10^9`, vì vậy số học 32-bit thông thường sẽ không an toàn đối với các tổng trung gian trong các ngôn ngữ như C++. Số nguyên Python không bị tràn, nhưng việc triển khai vẫn sẽ giảm dư lượng theo modulo`m`tại những điểm mà chúng được sử dụng làm khóa băm. 

Có một số trường hợp khó xử lý. Với`n = 1`,`m = 5`, Và`a = [3]`, không có nghiệm nào cả, vì vị trí duy nhất không trống sẽ cho cặn`3`, không`0`. Một chương trình bất cẩn luôn giả định đối số chuồng chim tạo ra một giải pháp sẽ in sai một trọng số. 

Vì`n = 1`,`m = 1`, Và`a = [3]`, câu trả lời đúng là đặt một vật nặng lên một trong hai tấm, vì mọi số nguyên đều bằng 0 modulo`1`. Một chương trình chỉ tìm kiếm hai tấm không trống có thể từ chối trường hợp này một cách không chính xác. 

Một trường hợp tinh tế khác là khi một đĩa trống. Ví dụ,```

```có một giải pháp hợp lệ bởi vì`1 + 2 + 4 = 7`, do đó cả ba quả nặng có thể được đặt lên đĩa thứ nhất và đĩa thứ hai có thể để trống. Việc yêu cầu cả hai tấm chứa một vật nặng sẽ từ chối nó một cách không chính xác. 

Cuối cùng, hai bên phải rời rạc. Ví dụ, với`m = 7`và trọng lượng`3, 3, 3, 3`, đặt hai quả cân mỗi bên sẽ tính được tổng`6`Và`6`. Một công thức chỉ tìm kiếm hai tổng tập hợp con bằng nhau mà không thể hiện rõ ràng ba trạng thái có thể có của mỗi trọng số có thể vô tình sử dụng lại cùng một trọng số ở cả hai vế. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp gán cho mỗi trọng lượng một trong ba trạng thái: chưa sử dụng, tấm thứ nhất hoặc tấm thứ hai. Đối với mỗi`3^n`bài tập, chúng tôi tính toán sự khác biệt giữa hai khoản tiền theo modulo`m`. Nếu nó bằng 0 và ít nhất một trọng số đã được sử dụng thì chúng ta có câu trả lời. Điều này hoàn toàn chính xác vì mọi vị trí có thể đều được thể hiện chính xác một lần. Tại`n = 25`, tuy nhiên, tìm kiếm chứa`3^25 = 847,288,609`trạng thái, vì vậy nó quá chậm. 

Quan sát hữu ích là phương trình có tính cộng. Chia các quả cân thành hai nhóm, nhóm thứ nhất có nhiều nhất 12 quả nặng và nhóm thứ hai nhiều nhất là 13 quả. Đối với mỗi nhóm, liệt kê tất cả các bài tập ternary một cách độc lập. Một bài tập có một số tiền đã ký`x = Σ c_i*a_i`. 

Nếu một bài tập từ nửa đầu có dư lượng`r`, sau đó là một bài tập từ hiệp hai có dư`-r mod m`kết hợp với nó thành một bài tập hoàn chỉnh có tổng số tiền đã ký chia hết cho`m`. 

Nửa đầu có nhiều nhất`3^12 = 531,441`bài tập, trong khi bài thứ hai có nhiều nhất`3^13 = 1,594,323`. Chúng tôi lưu trữ một phép gán cho mỗi phần dư được tạo ra bởi nửa đầu trong bảng băm, sau đó quét nửa sau và tra cứu phần dư bổ sung cần thiết. 

Sự đại diện của ba bang là điều làm cho cách tiếp cận gặp gỡ ở giữa này trở nên đặc biệt rõ ràng. Chúng ta không phải lo lắng về sự chồng chéo vì mỗi trọng lượng thuộc về chính xác một nửa và trong mỗi nửa, trạng thái của nó đã là trạng thái chưa sử dụng, tấm thứ nhất hoặc tấm thứ hai. 

Nhiệm vụ hoàn toàn bằng 0 xứng đáng được xử lý rõ ràng. Dư lượng`0`đương nhiên tương ứng với việc gán mọi trọng số cho cả hai tấm, nhưng bài toán yêu cầu ít nhất một trọng lượng. Chúng tôi chỉ đơn giản bỏ qua kết quả khớp trong đó cả mã ternary được lưu trữ và mã ternary hiện tại đều bằng 0. Nếu một trong hai bên có phép gán khác 0 thì vị trí kết quả là hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(3^n)`|`O(n)`| Quá chậm | 
| Gặp nhau ở giữa |`O(3^(n/2))`dự kiến ​​|`O(3^(n/2))`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia`n`trọng số vào nhóm đầu tiên`n // 2`trọng lượng và nhóm thứ hai chứa phần còn lại. Giữ kích thước nhóm đầu tiên tối đa là 12 để giữ cho bảng băm nhỏ. 
2. Liệt kê mọi nhiệm vụ thứ ba của nhóm đầu tiên. Với mỗi trọng lượng, chữ số`0`có nghĩa là chưa sử dụng, chữ số`1`có nghĩa là tấm đầu tiên và chữ số`2`có nghĩa là tấm thứ hai. Tính tổng có dấu của nó theo modulo`m`và lưu trữ mã ternary cho phần dư đó nếu phần dư chưa được nhìn thấy trước đó. 

Chỉ giữ lại một mã cho mỗi dư lượng là đủ. Bất kỳ nhiệm vụ nào tạo ra cùng một dư lượng đều có thể thay thế cho nhau nhằm mục đích khớp nó với nửa còn lại. 
3. Liệt kê mọi phép gán bậc ba của nhóm thứ hai và tính tổng theo modulo có dấu của nó`m`. 
4. Đối với dư lượng nửa sau`r`, tìm dư lượng`(-r) mod m`ở hiệp một. Nếu nó tồn tại thì hai tổng có dấu sẽ cộng bằng 0 theo modulo`m`, do đó vị trí kết hợp sẽ cân bằng tỷ lệ. 
5. Chỉ từ chối kết quả khớp khi cả hai mã bậc ba đều bằng 0. Sự kết hợp đó không sử dụng trọng lượng và bị cấm. Mỗi trận đấu khác đều đưa ra một câu trả lời hợp lệ. 
6. Giải mã hai mã bậc ba. Với mỗi chữ số bằng`1`, ghi trọng lượng đó lên tấm đầu tiên. Với mỗi chữ số bằng`2`, xuất nó trên tấm thứ hai. Hai nhóm được thiết kế rời rạc nên không có trọng lượng nào có thể xuất hiện trên cả hai tấm. 

### Tại sao nó hoạt động 

Đối với mọi vị trí có thể của các trọng số, mỗi trọng số có chính xác một hệ số từ`{-1, 0, 1}`. Việc chia các trọng số thành hai nhóm sẽ chia tổng số tiền đã ký thành`x + y`, Ở đâu`x`chỉ phụ thuộc vào nhóm đầu tiên và`y`chỉ vào ngày thứ hai. 

Nửa liệt kê đầu tiên chứa mọi giá trị có thể có của`x mod m`. Khi chúng tôi xử lý giá trị nửa sau`y`, đang tìm kiếm`(-y) mod m`tìm thấy chính xác các giá trị nửa đầu thỏa mãn`x + y ≡ 0 (mod m)`. 

Do đó, mọi kết quả trùng khớp do thuật toán tạo ra đều biểu thị hai tổng tấm có số dư bằng nhau. Ngược lại, bất kỳ vị trí hợp lệ nào cũng có một số dư lượng nửa đầu`x`và dư lượng nửa sau`y`thỏa mãn phương trình tương tự, do đó thuật toán sẽ tìm ra cặp bài tập phù hợp. Nhiệm vụ không hợp lệ duy nhất là nhiệm vụ hoàn toàn không được sử dụng mà chúng tôi loại trừ một cách rõ ràng. 

## Giải pháp Python```
Python
```ban đầu`m == 1`kiểm tra là một tối ưu hóa ranh giới nhỏ nhưng hữu ích. Vì mọi dư lượng modulo`1`bằng 0, bất kỳ trọng lượng đơn lẻ nào cũng đủ. 

Phím tắt tiếp theo sẽ kiểm tra xem trọng số riêng lẻ đã chia hết cho chưa`m`. Trọng lượng như vậy có thể được đặt riêng trên một tấm, vì vậy không có lý do gì để thực hiện tìm kiếm ở giữa. 

Đệ quy`build`hàm liệt kê ba lựa chọn của nửa đầu.`place`là lũy thừa hiện tại của 3, do đó phép gán bậc ba được lưu gọn dưới dạng số nguyên. Tổng đã ký được tích lũy trực tiếp và chỉ cần phần dư cuối cùng của nó làm khóa băm. 

các`search`hàm thực hiện phép liệt kê tương tự cho nửa sau. Nếu dư lượng của nó là`r`, dư lượng nửa đầu cần thiết là`(-r) % m`. Hoạt động modulo của Python tạo ra một giá trị trong`[0, m - 1]`, vì vậy điều này hoạt động chính xác ngay cả khi`r`là khác không. 

Mã ternary được giải mã bằng cách lặp lại`% 3`Và`// 3`. Chữ số bậc ba có ý nghĩa nhỏ nhất tương ứng với trọng số đầu tiên trong mỗi nửa vì phép đệ quy gán giá trị hiện tại`place`trước khi nhân nó với ba. 

Thuật toán không bao giờ sử dụng cùng một trọng lượng trên cả hai tấm vì hai nửa rời nhau. Trong vòng một nửa, mỗi chữ số bậc ba chỉ có một trạng thái, do đó kết quả được giải mã sẽ tự động thể hiện một vị trí hợp pháp. 

Độ sâu đệ quy nhiều nhất là 13, thấp hơn nhiều so với giới hạn đệ quy của Python. Số lượng các cuộc gọi đệ quy là theo cấp số nhân, nhưng nó vẫn theo thứ tự`3^13`, đó là quy mô dự định của giải pháp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho```

```sự chia tách là`1, 3`trong nửa đầu và`7, 10`trong nửa sau. 

Một trận đấu hữu ích là bài tập ở hiệp một không sử dụng cả hai trọng số, kết hợp với việc đặt bài tập ở hiệp hai.`7`trên tấm thứ hai và`10`trên tấm đầu tiên. Tổng số tiền họ ký là`10 - 7 = 3`, không bằng 0, vì vậy sự kết hợp cụ thể này không khớp. Sự kết hợp thành công được thuật toán tìm thấy tương ứng với việc đặt trọng số`4`trên đĩa đầu tiên và quả cân`2, 3`trên tấm thứ hai. 

| Trạng thái nửa đầu | Tổng nửa đầu | Trạng thái nửa sau | Tổng hiệp hai | Tổng mô-đun 14 | 
| --- | --- | --- | --- | --- | 
| tạ 1, 2 chưa sử dụng |`0`| trọng lượng 3 chưa sử dụng, trọng lượng 4 đầu tiên |`10`|`10`| 
| trọng lượng 1 chưa sử dụng, trọng lượng 2 giây |`-3`| trọng lượng 3 giây, trọng lượng 4 đầu tiên |`3`|`0`| 

Vị trí cuối cùng là trọng lượng`4`trên đĩa đầu tiên và quả cân`2, 3`trên tấm thứ hai. Tổng thực tế của chúng là`10`Và`3 + 7 = 10`, do đó, thang đo báo cáo sự bằng nhau ngay cả khi không dựa vào mô-đun bao quanh thực tế. 

### Mẫu 2 

cho```

```nửa đầu chứa trọng lượng`1`, trong khi nửa sau chứa trọng số`2`Và`3`. 

| Trạng thái nửa đầu | Tổng nửa đầu | Trạng thái nửa sau | Tổng hiệp hai | Tổng mô-đun 7 | 
| --- | --- | --- | --- | --- | 
| cân nặng 1 đầu tiên |`1`| tạ 2 và 3 trước |`6`|`0`| 

Vị trí kết quả đặt cả ba quả nặng lên tấm đầu tiên. Tổng số của nó là`1 + 2 + 4 = 7`, có dư lượng bằng 0 modulo`7`, trong khi tấm thứ hai trống cũng có dư lượng bằng không. 

Ví dụ này chứng minh tại sao phải cho phép một chiếc đĩa trống. Yêu cầu chỉ là ít nhất một quả cân được sử dụng ở đâu đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(3^(n/2))`dự kiến ​​| Chúng tôi liệt kê nhiều nhất`3^12 + 3^13`bài tập và thực hiện tra cứu băm trung bình theo thời gian liên tục. | 
| Không gian |`O(3^(n/2))`| Bảng băm lưu trữ tối đa một mã ba ngôi cho mỗi phần dư được tạo ra bởi nửa nhỏ hơn. | 

Với`n = 25`, nửa lớn nhất chứa 13 quả cân, cho kết quả là`1.59 * 10^6`nhiệm vụ tạm thời. Điều này nhỏ hơn đáng kể so với`8.47 * 10^8`nhiệm vụ của vũ lực. Yêu cầu bộ nhớ bị chi phối bởi bảng băm và nằm trong giới hạn rộng rãi 512 MB, mặc dù chi phí đối tượng của Python khiến việc triển khai này nặng hơn đáng kể so với triển khai C++ tương đương. 

## Trường hợp thử nghiệm 

Đầu ra của vấn đề này không phải là duy nhất, vì vậy các thử nghiệm nên xác thực vị trí được tạo ra thay vì so sánh chuỗi đầu ra theo nghĩa đen. Trình trợ giúp bên dưới phân tích cú pháp đầu ra của chương trình và kiểm tra xem các chỉ số đã chọn có rời rạc hay không, có sử dụng ít nhất một trọng số và tổng của hai tấm có bằng modulo không`m`.```python
import sys
import io

def solve(data=None):
    if data is None:
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
    else:
        it = iter(map(int, data.split()))
        n = next(it)
        m = next(it)
        a = [next(it) for _ in range(n)]

    if m == 1:
        return "1\n1\n0\n\n"

    for i, x in enumerate(a):
        if x % m == 0:
            return f"1\n{i + 1}\n0\n\n"

    left_n = n // 2
    left_n = min(left_n, n)
    left = a[:left_n]
    right = a[left_n:]
    right_n = len(right)

    left_map = {}

    def build(pos, total, code, place):
        if pos == left_n:
            r = total % m
            if r not in left_map:
                left_map[r] = code
            return

        build(pos + 1, total, code, place * 3)
        build(pos + 1, total + left[pos], code + place, place * 3)
        build(pos + 1, total - left[pos], code + 2 * place, place * 3)

    build(0, 0, 0, 1)

    answer = None

    def search(pos, total, code, place):
        nonlocal answer

        if answer is not None:
            return

        if pos == right_n:
            target = (-total) % m
            if target in left_map:
                lc = left_map[target]
                if lc != 0 or code != 0:
                    answer = (lc, code)
            return

        search(pos + 1, total, code, place * 3)
        search(pos + 1, total + right[pos], code + place, place * 3)
        search(pos + 1, total - right[pos], code + 2 * place, place * 3)

    search(0, 0, 0, 1)

    if answer is None:
        return "-1\n"

    lc, rc = answer
    first = []
    second = []

    for i in range(left_n):
        d = lc % 3
        lc //= 3
        if d == 1:
            first.append(i + 1)
        elif d == 2:
            second.append(i + 1)

    for i in range(right_n):
        d = rc % 3
        rc //= 3
        if d == 1:
            first.append(left_n + i + 1)
        elif d == 2:
            second.append(left_n + i + 1)

    return (
        f"{len(first)}\n"
        f"{' '.join(map(str, first))}\n"
        f"{len(second)}\n"
        f"{' '.join(map(str, second))}\n"
    )

def check(inp: str):
    data = list(map(int, inp.split()))
    n, m = data[0], data[1]
    a = data[2:2 + n]

    out = solve(inp).split()
    assert out, "empty output"

    if out[0] == "-1":
        # For tests below we only use cases with known solutions,
        # except the explicit impossible case checked separately.
        return False

    p = 0
    k = int(out[p])
    p += 1
    first = list(map(int, out[p:p + k]))
    p += k

    q = int(out[p])
    p += 1
    second = list(map(int, out[p:p + q]))

    assert len(first) == k
    assert len(second) == q
    assert k + q > 0
    assert len(set(first)) == k
    assert len(set(second)) == q
    assert set(first).isdisjoint(second)
    assert all(1 <= x <= n for x in first + second)

    s1 = sum(a[i - 1] for i in first) % m
    s2 = sum(a[i - 1] for i in second) % m
    assert s1 == s2

    return True

# Provided sample 1.
assert check("4 14\n1 3 7 10\n"), "sample 1"

# Provided sample 2.
assert check("3 7\n1 2 4\n"), "sample 2"

# Minimum-size input, m = 1 means any nonempty placement works.
assert check("1 1\n999999999\n"), "minimum size"

# All values equal. Two weights on each side give equal sums.
assert check("5 7\n3 3 3 3 3\n"), "all equal values"

# Maximum n and a modulus close to the upper boundary.
# A pair of equal weights already balances the scale.
assert check(
    "25 39999999\n"
    "1 1 1 1 1 1 1 1 1 1 1 1 1 "
    "1 1 1 1 1 1 1 1 1 1 1 1 1\n"
), "maximum n"

# A value divisible by m exercises the direct single-weight boundary.
assert check("4 10\n20 3 7 11\n"), "divisible weight"

# Explicit impossible case.
assert solve("1 5\n3\n").strip() == "-1", "impossible single weight"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 999999999`| Bất kỳ vị trí nào không trống | tối thiểu`n`Và`m = 1`| 
|`5 7 / 3 3 3 3 3`| Hai quả cân ở mỗi bên hoặc vị trí hợp lệ khác | Giá trị bằng nhau và tấm rời | 
|`25 39999999 / 25 ones`| Bất kỳ vị trí nào có dư lượng bằng nhau, ví dụ: một quả cân trên mỗi đĩa | Tối đa`n`và mô đun gần giới hạn trên của nó | 
|`4 10 / 20 3 7 11`| Cân nặng`1`một mình là hợp lệ | Phím tắt chia hết trực tiếp | 
|`1 5 / 3`|`-1`| Không có bài tập nào trống tồn tại | 

## Vỏ cạnh 

Đối với trường hợp không thể có trọng lượng đơn```
1 5
3
```đầu tiên thuật toán sẽ kiểm tra xem liệu`3 % 5`là số không, điều đó không phải vậy. Hai nửa chứa một quả nặng và không có quả nặng. Bài tập ternary duy nhất không trống có tổng có dấu`3`hoặc`-3`, không chia hết cho`5`, do đó việc tìm kiếm kết thúc mà không có kết quả trùng khớp và in ra`-1`. 

Đối với trường hợp mô-đun một```
1 1
3
```thuật toán dừng ngay lập tức. Vì mọi số nguyên đều đồng dạng với 0 modulo`1`, đặt trọng lượng`1`trên tấm đầu tiên là hợp lệ. Đầu ra tương đương với```
1
1
0
```Dòng trống tượng trưng cho tấm thứ hai trống. 

Đối với giải pháp một chiều```
3 7
1 2 4
```nhiệm vụ đã ký`(+1, +1, +1)`có tổng`7`, do đó dư lượng của nó bằng không. Tấm còn lại có thể sử dụng phép gán số 0. Thuật toán chấp nhận điều này vì mã ba ngôi kết hợp khác 0 mặc dù mã nửa sau có thể đại diện cho một tấm trống. 

Đối với trọng lượng bằng nhau,```
5 7
3 3 3 3 3
```đặt hai quả cân lên mỗi đĩa sẽ tính được tổng`6`Và`6`. Biểu diễn bậc ba gán hai trọng số cho hệ số`+1`, hai trọng số hệ số`-1`, và để lại cái thứ năm không được sử dụng. Tổng đã ký bằng 0 nên phần dư của hai tấm khớp chính xác. 

Lỗi triển khai nguy hiểm nhất là chấp nhận cặp trong đó cả hai mã ternary đều bằng 0. Cặp đó luôn tồn tại vì phép gán trống có số dư bằng 0 ở cả hai nửa. Sự rõ ràng`left_code != 0 or code != 0`kiểm tra loại bỏ chính xác giải pháp không hợp lệ đó trong khi vẫn bảo toàn các trường hợp hợp lệ khi một đĩa trống.

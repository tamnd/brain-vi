---
title: "CF 102396G - Tràn trọng lượng"
description: "Chúng tôi có tới 25 quả cân và mỗi quả cân có thể được đặt trên đĩa thứ nhất, đĩa thứ hai hoặc không sử dụng. Thang đo không so sánh các khoản tiền thông thường. Thay vào đó, nó làm giảm cả hai tổng số tấm modulo (m) và báo cáo số dư khi hai phần dư đó bằng nhau."
date: "2026-08-14T14:17:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102396
codeforces_index: "G"
codeforces_contest_name: "2019-2020 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 19)"
rating: 0
weight: 102396
solve_time_s: 196
verified: false
draft: false
---

[CF 102396G - Tràn trọng lượng](https://codeforces.com/problemset/problem/102396/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có tới 25 quả cân và mỗi quả cân có thể được đặt trên đĩa thứ nhất, đĩa thứ hai hoặc không sử dụng. Thang đo không so sánh các khoản tiền thông thường. Thay vào đó, nó làm giảm cả hai tổng số tấm modulo (m) và báo cáo số dư khi hai phần dư đó bằng nhau. 

Nếu tấm thứ nhất chứa tập hợp (L) và tấm thứ hai chứa tập hợp rời rạc (R) thì điều kiện cần là 

[ 
\sum_{i\in L} a_i \equiv \sum_{i\in R} a_i \pmod m. 
] 

Tương đương, 

[ 
\sum_{i\in L} a_i-\sum_{i\in R}a_i\equiv0\pmod m. 
] 

Vì vậy, mọi trọng lượng đều có chính xác ba trạng thái có thể xảy ra. Chúng ta có thể mã hóa các trạng thái này bằng (0), (+1) và (-1), trong đó (0) có nghĩa là không sử dụng, (+1) có nghĩa là tấm đầu tiên và (-1) có nghĩa là tấm thứ hai. Chúng ta cần một phép gán khác 0 có tổng có dấu chia hết cho (m). 

Giới hạn (n\le25) là đầu mối trung tâm. Bảng liệt kê trực tiếp có (3^n) trạng thái và (3^{25}=847288609443), vượt xa những gì giải pháp một giây có thể kiểm tra. Modulo (m) có thể gần bằng (4\cdot10^7), do đó, một mảng lập trình động được lập chỉ mục theo mọi phần dư cũng sẽ quá đắt nếu chúng ta xử lý tất cả các trọng số. Số lượng trọng số tương đối nhỏ gợi ý nên chia chúng thành hai nửa và liệt kê ba lựa chọn một cách độc lập trong mỗi nửa. 

Các giá trị (a_i) có thể lớn bằng (10^9), nhưng chỉ phần dư của chúng theo modulo (m) ảnh hưởng đến thang đo. Số nguyên Python cũng tránh tràn, do đó không có vấn đề tràn số học trong quá trình triển khai. 

Trường hợp cạnh đầu tiên là (n=1). Ví dụ,```
1 5
3
```không có câu trả lời, bởi vì vị trí duy nhất không trống đặt trọng số 3 lên một tấm và để trống cái kia, tạo ra các thặng dư 3 và 0. Một giải pháp bất cẩn chỉ kiểm tra xem một số tổng tập hợp con có chia hết cho (m) xảy ra trong trường hợp này hay không, nhưng giải pháp dựa trên việc so sánh hai tập hợp con được tạo độc lập phải cấm rõ ràng việc sử dụng cùng một trọng số ở cả hai bên. 

Trường hợp cạnh thứ hai xảy ra khi một trọng số đã chia hết cho (m). Ví dụ,```
1 5
10
```có đầu ra hợp lệ```
1
1
0
```vì tấm thứ nhất có cặn (10\bmod5=0), trong khi tấm thứ hai trống rỗng. Việc thực hiện bất cẩn yêu cầu cả hai tấm phải chứa vật nặng sẽ từ chối một câu trả lời hợp lệ. 

Trường hợp cạnh thứ ba là (m=1). Ví dụ,```
1 1
7
```luôn có thể giải được vì mọi số nguyên đều đồng dư với 0 theo modulo 1. Thuật toán phải cho phép tấm trống và không được vô tình coi phép gán toàn bộ chưa được sử dụng là một câu trả lời hợp lệ. 

Trường hợp cạnh thứ tư là các tổng thông thường không cần phải bằng nhau. Ví dụ,```
4 14
1 3 7 10
```có thể đặt quả cân (1,3,10) lên một đĩa. Tổng thông thường của chúng là 14, trở thành số dư 0, trong khi đĩa kia trống. Một giải pháp chỉ tìm kiếm các tổng thông thường bằng nhau sẽ bỏ lỡ sự đẳng thức mô-đun hợp lệ này. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu được rút ra trực tiếp từ ba trạng thái có thể có của mọi trọng số. Đối với mỗi trọng lượng, hãy chọn tấm thứ nhất hoặc tấm thứ hai chưa sử dụng, tính tổng có dấu theo modulo (m) và chấp nhận phép gán khác 0 có dư lượng bằng 0. Điều này đúng vì mọi vị trí hợp pháp đều tương ứng với chính xác một trong các nhiệm vụ ba trạng thái này. 

Vấn đề là số lượng nhiệm vụ. Với (n=25), có 

[ 
3^{25}=847288609443 
] 

khả năng. Ngay cả khi việc kiểm tra một nhiệm vụ chỉ thực hiện một thao tác duy nhất trong thời gian không đổi, thì thao tác này vẫn quá lớn so với giới hạn một giây. 

Quan sát hữu ích là số tiền đã ký có tính cộng. Nếu chúng ta chia các trọng số thành hai nhóm thì mỗi phép gán hoàn chỉnh có thể được viết dưới dạng tổng có dấu của nửa đầu cộng với tổng có dấu của nửa sau. Đối với phần dư (x) được tạo ra ở nửa đầu, chúng ta chỉ cần phần dư ở nửa phần sau bằng (-x\bmod m). 

Đây là ý tưởng gặp nhau ở giữa. Thay vì khám phá tất cả (3^n) bài tập, chúng tôi khám phá các bài tập (3^{\lfloor n/2\rfloor}) trong một nửa và (3^{\lceil n/2\rceil}) trong nửa còn lại. Đối với (n=25), những con số này là (3^{12}=531441) và (3^{13}=1594323), là những con số thực tế. 

Có một chi tiết làm cho công thức này trở nên đặc biệt thuận tiện. Chúng tôi không chỉ lưu trữ liệu phần dư có tồn tại trong nửa đầu hay không mà còn lưu trữ một nhiệm vụ tạo ra phần dư đó. Khi nửa thứ hai tìm thấy phần dư bổ sung, hai phép gán được lưu trữ cùng nhau mô tả trực tiếp hai tấm. 

Nhiệm vụ hoàn toàn bằng 0 phải được xử lý đặc biệt. Nó luôn cho dư lượng bằng 0, nhưng không được phép đặt trọng lượng ở bất cứ đâu. Nếu một trong hai nửa đóng góp một lựa chọn khác 0 thì phép gán kết hợp sẽ hợp lệ. Chỉ trường hợp cả hai mã được lưu trữ bằng 0 mới bị từ chối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(3^n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(3^{\lceil n/2\rceil})) | (O(3^{\lfloor n/2\rfloor})) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia các trọng lượng thành nửa kích thước đầu tiên (\lfloor n/2\rfloor) và nửa sau chứa các trọng lượng còn lại. Việc phân chia được chọn sao cho bảng liệt kê lớn hơn chỉ chứa tối đa 13 trọng số. 
2. Liệt kê từng nhiệm vụ của nửa đầu. Mỗi trọng số có ba lựa chọn: đóng góp (0), đóng góp (+a_i) hoặc đóng góp (-a_i). Lưu trữ dư lượng modulo (m) kết quả trong một từ điển, cùng với một phép gán được mã hóa tạo ra nó. Nếu một số nhiệm vụ tạo ra cùng một dư lượng thì chỉ cần một nhiệm vụ vì mỗi nhiệm vụ đều hữu ích như nhau trong việc hoàn thành một giải pháp. 
3. Liệt kê mọi nhiệm vụ của nửa sau bằng cách sử dụng ba lựa chọn giống nhau. Giả sử tổng đã ký của nó có (các) số dư. Để tổng có dấu đầy đủ chia hết cho (m) thì nửa đầu phải có dư 

[ 
(-s)\bmod m. 
] 

Hãy tra phần cặn này trong từ điển. 

1. Khi tìm thấy dư lượng phù hợp, hãy giải mã cả hai phép gán. Lựa chọn (+1) dành cho tấm đầu tiên, lựa chọn (-1) dành cho tấm thứ hai và lựa chọn (0) bị bỏ qua. 
2. Chỉ từ chối kết quả khớp khi cả hai mã gán đều bằng 0. Trong mọi trường hợp khác, ít nhất một trọng số được sử dụng và hai nửa tổng cộng lại bằng 0 modulo (m), do đó thang đo được cân bằng. 
3. Nếu bảng liệt kê hoàn chỉnh kết thúc mà không khớp, hãy in (-1). Mọi vị trí có thể đều được thể hiện bằng một cặp nửa bài tập, vì vậy không thể bỏ sót giải pháp nào.

Tại sao nó hoạt động: mọi vị trí hợp pháp đều tương ứng với một vectơ hệ số (c_i\in{-1,0,+1}), trong đó hệ số biểu thị tấm hoặc trạng thái không sử dụng. Việc chia vectơ thành hai nửa sẽ cho các dư lượng (x) và (y) thỏa mãn (x+y\equiv0\pmod m) chính xác khi vị trí cân bằng tỷ lệ. Từ điển nửa đầu chứa một đại diện cho mỗi dư lượng mà nó có thể tạo ra và tìm kiếm ở nửa sau sẽ kiểm tra chính xác dư lượng bổ sung cho mọi phép gán có thể có ở nửa sau. Do đó, mọi vị trí không trống hợp lệ đều được tìm thấy, trong khi mọi vị trí được báo cáo đều có tổng có dấu chia hết cho (m). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [x % m for x in map(int, input().split())]

    mid = n // 2
    left = a[:mid]
    right = a[mid:]

    # choice encoding:
    # 0 = unused
    # 1 = first plate
    # 2 = second plate
    #
    # Two bits are enough for every choice.
    first = {}

    def build_first(pos, end, residue, code):
        if pos == end:
            first.setdefault(residue, code)
            return

        x = a[pos]

        build_first(pos + 1, end, residue, code)

        nr = residue + x
        if nr >= m:
            nr -= m
        build_first(pos + 1, end, nr, code | (1 << (2 * (pos - 0))))

        nr = residue - x
        if nr < 0:
            nr += m
        build_first(pos + 1, end, nr, code | (2 << (2 * (pos - 0))))

    build_first(0, mid, 0, 0)

    answer = None

    def search_second(pos, end, residue, code):
        nonlocal answer

        if answer is not None:
            return

        if pos == end:
            need = (-residue) % m
            if need in first:
                left_code = first[need]
                if left_code != 0 or code != 0:
                    answer = (left_code, code)
            return

        x = a[pos]

        # Leave the weight unused.
        search_second(pos + 1, end, residue, code)

        if answer is not None:
            return

        # Put it on the first plate.
        nr = residue + x
        if nr >= m:
            nr -= m
        search_second(
            pos + 1,
            end,
            nr,
            code | (1 << (2 * (pos - mid)))
        )

        if answer is not None:
            return

        # Put it on the second plate.
        nr = residue - x
        if nr < 0:
            nr += m
        search_second(
            pos + 1,
            end,
            nr,
            code | (2 << (2 * (pos - mid)))
        )

    search_second(mid, n, 0, 0)

    if answer is None:
        return "-1\n"

    left_code, right_code = answer
    first_plate = []
    second_plate = []

    for i in range(mid):
        choice = (left_code >> (2 * i)) & 3
        if choice == 1:
            first_plate.append(i + 1)
        elif choice == 2:
            second_plate.append(i + 1)

    for i in range(n - mid):
        choice = (right_code >> (2 * i)) & 3
        idx = mid + i + 1
        if choice == 1:
            first_plate.append(idx)
        elif choice == 2:
            second_plate.append(idx)

    out = [
        str(len(first_plate)),
        " ".join(map(str, first_plate)),
        str(len(second_plate)),
        " ".join(map(str, second_plate)),
    ]
    return "\n".join(out) + "\n"

if __name__ == "__main__":
    sys.stdout.write(solve())
```Nửa đầu được lưu trữ trong một từ điển được khóa bằng dư lượng.`setdefault`giữ lại nhiệm vụ đầu tiên gặp phải đối với phần dư và tránh sự thay thế không cần thiết. Vì mỗi dư lượng chỉ cần một nhân chứng nên thế là đủ. 

Phép gán được mã hóa với hai bit cho mỗi trọng số. Các giá trị 0, 1 và 2 đại diện cho tấm thứ nhất và tấm thứ hai chưa được sử dụng. Việc giữ mã ở dạng số nguyên làm cho từ điển nhỏ hơn đáng kể so với việc lưu trữ danh sách hoặc bộ dữ liệu Python cho mọi trạng thái. 

Đệ quy tính toán dư lượng modulo (m) tại mỗi lần chuyển đổi. Vì phần dư hiện tại đã nằm trong khoảng từ (0) đến (m-1), nên việc thêm một phần dư cần tối đa một phép trừ (m) và phép trừ một phần dư cần nhiều nhất một phép cộng (m). Điều này tránh được cái chung đắt tiền hơn`%`hoạt động bên trong phần nóng nhất của bảng liệt kê. 

Hai nửa sử dụng các vị trí bit cục bộ riêng biệt. Khi giải mã nửa sau, các chỉ số được dịch chuyển bởi`mid`để phục hồi việc đánh số trọng lượng ban đầu. Do đó, mã hóa cục bộ giống nhau có thể được sử dụng ở cả hai nửa. 

Python không bị tràn số nguyên có chiều rộng cố định thường xảy ra ở các ngôn ngữ sử dụng số nguyên 32 bit. Trọng lượng ban đầu được giảm theo modulo (m) trước khi liệt kê, vì vậy mọi dư lượng được lưu trữ dù sao cũng vẫn ở mức nhỏ. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
4 14
1 3 7 10
```sự phân chia là ( [1,3] \mid [7,10] ). Nửa đầu có thể tạo ra các dư lượng có dấu như (0,1,3,4,10,12,\ldots). Việc tìm kiếm ở nửa sau cuối cùng xem xét việc đặt trọng lượng 10 lên tấm đầu tiên, đóng góp phần dư 10. Phần bù của nó là (14-10=4), và nửa đầu có thể tạo ra phần dư 4 bằng cách đặt trọng số 1 và 3 lên tấm đầu tiên. 

| Sân khấu | Dư lượng nửa đầu | Dư lượng nửa sau | Dư lượng cần thiết | Đã tìm thấy bài tập | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 0 | tất cả không được sử dụng, bị từ chối | 
| Điều tra nửa đầu | 4 | 0 | 0 | trọng số 1 và 2 tạo ra 4 | 
| Tìm kiếm nửa sau | 4 | 10 | 4 | tìm thấy trận đấu | 

Vị trí kết quả là các vật nặng 1, 2 và 4 trên tấm đầu tiên và không có vật nặng nào trên tấm thứ hai. Tổng thông thường của chúng là (1+3+10=14), do đó thang đo nhìn thấy (0) trên cả hai mặt theo modulo 14. Điều này chứng tỏ rằng thuật toán đang tìm kiếm đẳng thức mô-đun thay vì đẳng thức của tổng thông thường. 

Đối với mẫu 2,```
3 7
1 2 4
```sự phân chia là ( [1]\mid[2,4] ). Nửa đầu tạo ra dư lượng (0,1,6). Nửa thứ hai có thể tạo ra cặn 6 bằng cách đặt cả hai quả nặng 2 và 4 lên đĩa thứ nhất. Phần bù bắt buộc của nó là 1, tồn tại trong từ điển nửa đầu. 

| Sân khấu | Dư lượng nửa đầu | Dư lượng nửa sau | Dư lượng cần thiết | Đã tìm thấy bài tập | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 0 | tất cả không được sử dụng, bị từ chối | 
| Điều tra nửa đầu | 1 | 0 | 0 | trọng lượng 1 tạo ra 1 | 
| Điều tra nửa sau | 1 | 6 | 1 | trọng số 2 và 3 tạo ra 6 | 

Vị trí kết quả đặt cả ba quả nặng lên tấm đầu tiên. Tổng của chúng là (1+2+4=7), bằng 0 modulo 7, trong khi tấm thứ hai trống. Đây cũng là một ví dụ hữu ích về lý do tại sao tấm thứ hai trống là hợp pháp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(3^{\lceil n/2\rceil})) | Mỗi nửa liệt kê mọi phép gán bậc ba và nửa lớn hơn có tối đa 13 trọng số. | 
| Không gian | (O(3^{\lfloor n/2\rfloor})) | Từ điển lưu trữ một phép gán cho mỗi phần dư riêng biệt được tạo ra bởi một nửa nhỏ hơn. | 

Tại (n=25), nửa lớn hơn có (3^{13}=1,594,323) phép gán và nửa được lưu trữ có nhiều nhất (3^{12}=531,441) mục nhập. Nó nhỏ hơn theo cấp số nhân so với tìm kiếm brute-force (3^{25}) và vừa vặn thoải mái trong giới hạn bộ nhớ 512 MB với biểu diễn số nguyên nhỏ gọn được triển khai sử dụng. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới kiểm tra tính hợp lệ về cấu trúc của vị trí được trả về thay vì yêu cầu nhân chứng hợp lệ cụ thể. Đó là cách đúng đắn để kiểm tra vấn đề này vì nhiều đầu ra khác nhau có thể thỏa mãn cùng một đầu vào.```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

def check_output(inp: str, out: str) -> bool:
    data = list(map(int, inp.split()))
    n, m = data[0], data[1]
    a = data[2:2 + n]

    tokens = out.split()
    if not tokens:
        return False

    if tokens[0] == "-1":
        return len(tokens) == 1

    p = 0

    k = int(tokens[p])
    p += 1
    left = [int(tokens[p + i]) for i in range(k)]
    p += k

    q = int(tokens[p])
    p += 1
    right = [int(tokens[p + i]) for i in range(q)]
    p += q

    if p != len(tokens):
        return False

    if k + q == 0:
        return False

    if any(x < 1 or x > n for x in left + right):
        return False

    if len(set(left)) != len(left):
        return False
    if len(set(right)) != len(right):
        return False
    if set(left) & set(right):
        return False

    s_left = sum(a[i - 1] for i in left) % m
    s_right = sum(a[i - 1] for i in right) % m

    return s_left == s_right

# Provided sample 1
sample1 = """\
4 14
1 3 7 10
"""
out = run(sample1)
assert check_output(sample1, out), "sample 1"

# Provided sample 2
sample2 = """\
3 7
1 2 4
"""
out = run(sample2)
assert check_output(sample2, out), "sample 2"

# Minimum size, and m = 1.
case1 = """\
1 1
7
"""
out = run(case1)
assert check_output(case1, out), "minimum size"

# Minimum size with no possible answer.
case2 = """\
1 5
3
"""
assert run(case2).strip() == "-1", "single weight with nonzero residue"

# All equal values. One copy can balance another.
case3 = """\
4 10
7 7 7 7
"""
out = run(case3)
assert check_output(case3, out), "all equal values"

# Boundary-style pair: 2 + 3 = 5 modulo 5.
case4 = """\
2 5
2 3
"""
out = run(case4)
assert check_output(case4, out), "modulo boundary"

# Maximum n and a guaranteed no-answer instance.
# The sum of all powers of two is 2^25 - 1 = 33554431,
# which is smaller than m, so every subset has a distinct ordinary sum.
powers = [1 << i for i in range(25)]
case5 = "25 39999999\n" + " ".join(map(str, powers)) + "\n"
assert run(case5).strip() == "-1", "maximum-size no-answer case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7`| Bất kỳ vị trí trống hợp lệ nào | Tối thiểu (n), modulo 1 và tấm thứ hai trống | 
|`1 5 / 3`|`-1`| Trường hợp nhỏ nhất không thể | 
|`4 10 / 7 7 7 7`| Bất kỳ vị trí nào có phần dư không trống hoặc bên trống bằng nhau | Giá trị hoàn toàn bằng nhau và dư lượng trùng lặp | 
|`2 5 / 2 3`| Một trọng lượng trên mỗi tấm | Ranh giới modulo chính xác | 
|`25 39999999 / 1 2 4 ... 2^24`|`-1`| Tối đa (n), lớn (m) và trường hợp không có va chạm mô-đun | 

## Vỏ cạnh 

cho```
1 5
3
```nửa đầu trống và nửa sau chứa trọng số 3. Từ điển nửa đầu chỉ chứa phần dư 0 với phép gán trống. Nửa thứ hai có thể tạo ra các số dư 0, 3 và 2, nhưng chỉ số dư 0 mới có phần bù phù hợp. Trận đấu đó sẽ không sử dụng trọng số ở cả hai nửa, vì vậy thuật toán sẽ loại bỏ nó và in ra`-1`. Đây chính xác là hành vi cần thiết. 

Vì```
1 5
10
```trọng số duy nhất có phần dư 0. Từ điển nửa đầu lại chứa phần dư 0, trong khi phép gán nửa sau đặt trọng số 1 trên tấm đầu tiên cũng có phần dư 0. Hai mã gán không đều bằng 0, do đó sự trùng khớp được chấp nhận. Đầu ra đặt trọng số 1 lên tấm đầu tiên và để trống tấm thứ hai. 

Vì```
1 1
7
```mọi phần dư đều bằng 0 vì mô đun là 1. Phép liệt kê ở nửa sau ngay lập tức tìm thấy một phép gán khác rỗng với phần dư bằng 0, và phép gán trống ở nửa đầu sẽ cung cấp phần bù cần thiết. Vị trí kết quả là hợp lệ vì mỗi tổng của tấm đều bằng 0 modulo 1. 

cho```
4 14
1 3 7 10
```nửa đầu có thể tạo ra cặn 4 có dấu từ các quả cân 1 và 2 đặt trên đĩa thứ nhất. Nhiệm vụ ở nửa sau đặt vật nặng 4 lên đĩa đầu tiên sẽ đóng góp phần dư 10. Vì (4+10=14), phần dư có dấu tổng hợp của chúng bằng 0. Do đó, đầu ra có thể đặt các trọng số 1, 2 và 4 lên tấm đầu tiên, tạo ra tổng mô-đun bằng 0, khi tấm thứ hai trống. 

Đối với trường hợp không có câu trả lời kích thước tối đa```
25 39999999
1 2 4 8 16 32 64 128 256 512 1024 2048 4096
8192 16384 32768 65536 131072 262144 524288 1048576
2097152 4194304 8388608 16777216
```tổng của tất cả các trọng số là (2^{25}-1=33554431), nhỏ hơn (m). Do đó, mỗi tập hợp con có tổng thông thường khác nhau, vì vậy hai tập hợp con rời nhau không thể có tổng bằng nhau trừ khi cả hai đều rỗng. Vì tổng không bao giờ đạt đến (m) nên cũng không có tập con nào khác rỗng mà tổng chia hết cho (m). Tìm kiếm gặp ở giữa làm cạn kiệt tất cả các phép gán ba trạng thái và trả về chính xác`-1`.

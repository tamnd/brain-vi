---
title: "CF 102190L - đầu vào/đầu ra tiêu chuẩn"
description: "Chúng ta có những nhóm bạn có số lượng từ 1 đến 6. Một chiếc ghế dài có đúng sáu chỗ ngồi và mỗi nhóm phải chiếm một chiếc ghế dài. Các nhóm khác nhau có thể ngồi chung một băng ghế miễn là tổng số người của họ không vượt quá sáu."
date: "2026-08-19T06:08:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102190
codeforces_index: "L"
codeforces_contest_name: "2019 ECNU Campus Invitational Contest"
rating: 0
weight: 102190
solve_time_s: 149
verified: true
draft: false
---

[CF 102190L - đầu vào/đầu ra tiêu chuẩn](https://codeforces.com/problemset/problem/102190/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có những nhóm bạn có số lượng từ 1 đến 6. Một chiếc ghế dài có đúng sáu chỗ ngồi và mỗi nhóm phải chiếm một chiếc ghế dài. Các nhóm khác nhau có thể ngồi chung một băng ghế miễn là tổng số người của họ không vượt quá sáu. 

Đối với mỗi trường hợp thử nghiệm, sáu số đầu vào cho biết số nhóm có kích thước 1, 2, 3, 4, 5 và 6. Nhiệm vụ là giảm thiểu số lượng băng ghế. 

Tổng số người là dương tính và nhiều nhất là 10.000 cho một trường hợp thử nghiệm. Có tối đa 1.000 trường hợp thử nghiệm. Vì mỗi nhóm có ít nhất một người nên cũng có tối đa 10.000 nhóm trong một trường hợp thử nghiệm. Giá trị này đủ nhỏ để quét tuyến tính trên tất cả các nhóm, nhưng cấu trúc của bài toán cho phép chúng ta làm tốt hơn nhiều: giải pháp tối ưu có thể được tính toán chỉ bằng cách sử dụng một số phép tính số học không đổi. Việc tìm kiếm bậc hai hoặc hàm mũ trên các nhóm có thể là hoàn toàn không cần thiết. 

Các trường hợp nguy hiểm chính đến từ các nhóm gần như lấp đầy một băng ghế. Ví dụ, với```
1
1 0 0 0 0 0
```câu trả lời là 1, vì mỗi nhóm cần một chiếc ghế dài. 

Với```
1
0 0 0 1 1 0
```câu trả lời cũng là 1, vì một nhóm bốn người và một nhóm hai người ngồi đúng vào một băng ghế. Một giải pháp bất cẩn chỉ dựa trên tổng số người vẫn sẽ giải quyết đúng trường hợp này, nhưng một giải pháp xử lý từng quy mô nhóm một cách độc lập có thể sử dụng hai băng ghế. 

Một trường hợp ranh giới khác là```
1
0 0 1 0 0 1
```cần 2 băng ghế. Một nhóm sáu người không thể ngồi chung một chiếc ghế với bất kỳ ai, trong khi nhóm ba người lại cần một chiếc ghế khác. 

Sai lầm ngược lại xảy ra với các nhóm nhỏ. Vì```
1
8 0 0 0 0 0
```có tám nhóm một người nên đáp án là 2 chứ không phải 8. Sáu nhóm có thể ngồi chung một băng ghế, hai nhóm còn lại ngồi chung một băng ghế khác. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ coi mỗi nhóm như một món đồ và thử đệ quy mọi nhiệm vụ có thể có trên băng ghế dự bị. Khi xử lý một nhóm, chúng tôi có thể đặt nhóm đó vào bất kỳ nhóm nào tương thích hiện có hoặc mở một nhóm mới. Điều này đúng vì mọi cách sắp xếp chỗ ngồi có thể đều được đại diện bởi một nhánh nào đó của tìm kiếm. 

Vấn đề là số lượng sắp xếp. Nếu có (n) nhóm, việc gán chúng vào các nhóm băng ghế tùy ý có liên quan đến việc liệt kê các phân vùng tập hợp, có số đếm là số Bell (B_n). Với tối đa 10.000 nhóm, ngay cả giới hạn trên nhỏ hơn nhiều như (n^n = 10000^{10000}) cũng đã là vô vọng. Cách tiếp cận bạo lực không chỉ đơn thuần là quá chậm so với các giới hạn nhất định mà còn không khả thi ngay cả đối với các đầu vào tương đối nhỏ. 

Cấu trúc giúp giải quyết vấn đề trở nên dễ dàng là sức chứa cố định của băng ghế là sáu. Các nhóm lớn có rất ít đối tác tiềm năng. Một nhóm sáu người không có bạn đồng hành nào cả. Một nhóm năm người chỉ có thể được ghép nối với một nhóm một người. Nhóm bốn người có thể sử dụng nhóm hai hoặc hai nhóm một. Một nhóm ba người có thể ghép với ba người khác hoặc sử dụng các nhóm nhỏ hơn để lấp đầy những chỗ còn lại. Cuối cùng, nhóm hai người có thể xếp tối đa ba người trên một băng ghế và nhóm một người sẽ lấp đầy những chỗ còn lại. 

Điều này đưa ra một chiến lược tham lam. Trước tiên hãy xử lý các nhóm lớn hơn, luôn sử dụng các nhóm bổ sung hữu ích lớn nhất. Khi các nhóm lớn hơn đã được đặt, vấn đề còn lại chỉ bao gồm các nhóm có kích thước hai và một, trong đó việc đóng gói tối ưu là ngay lập tức. 

Chìa khóa không chỉ đơn giản là "đặt các nhóm lớn lên hàng đầu". Mỗi lựa chọn tham lam có thể được biện minh bởi thực tế là một nhóm lớn hơn có ít vị trí khả thi hơn một nhóm nhỏ hơn. Một nhóm năm người không thể sử dụng một nhóm hai hoặc ba người, vì vậy việc dành một nhóm một người cho nhóm đó không bao giờ có thể tệ hơn việc để lại nhóm đó cho một nhóm có những lựa chọn khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(B_n)) | (O(n)) | Quá chậm | 
| Tham lam tối ưu | (O(1)) cho mỗi trường hợp thử nghiệm | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng các nhóm có kích thước từ 1 đến 6. Đặt chúng là (c_1,c_2,\ldots,c_6). 
2. Đặt mỗi nhóm cỡ 6 vào ghế riêng của mình. Một nhóm như vậy không còn năng lực để nhóm khác có thể sử dụng nên những chiếc ghế này bị ép buộc. 
3. Xử lý các nhóm có số lượng 5. Mỗi nhóm như vậy cần năm chỗ ngồi và chỉ có thể chia sẻ với một nhóm có số lượng 1. Ghép càng nhiều nhóm 5 thành viên với 1 nhóm càng tốt và đặt bất kỳ nhóm 1 nào còn lại sang một bên. 
4. Xử lý các nhóm có kích thước 4. Đầu tiên hãy ghép chúng với các nhóm có kích thước 2. Sự kết hợp (4 + 2) sẽ lấp đầy chính xác một băng ghế. Nếu có nhiều hơn 4 nhóm sau khi đã sử dụng hết 2 nhóm hữu ích, hãy đặt tối đa hai nhóm 1 với mỗi nhóm 4 còn lại. 

Ghép nối số 4 với số 2 là an toàn ngay cả khi cũng có sẵn hai nhóm 1. Trong cả hai trường hợp, nhóm 4 đều đã yêu cầu ghế dài và việc sắp xếp thay thế không làm giảm số lượng ghế cần thiết cho các nhóm khác. 
5. Ghép các nhóm cỡ 3 với nhau. Mỗi cặp 3 nhóm lấp đầy chính xác một băng ghế, vì vậy các băng ghế (\lfloor c_3/2\rfloor) bị buộc phải có. 
6. Nếu còn lại một nhóm 3 người, hãy mở thêm một băng ghế cho nhóm đó. Nếu có sẵn 2 nhóm thì xếp vào nhóm 3. Nếu cũng có sẵn 1 nhóm, nhóm đó có thể tham gia cùng một băng ghế, tạo ra (3+2+1=6). Nếu không có nhóm 2, hãy lấp đầy các ghế còn lại bằng tối đa ba nhóm 1.

Sử dụng một nhóm 2 còn lại với 3 nhóm chưa từng có sẽ không bao giờ làm tăng câu trả lời. Việc loại bỏ một nhóm 2 sẽ thay đổi số lượng ghế cần thiết cho tất cả 2 nhóm còn lại từ (\lceil b/3\rceil) thành (\lceil(b-1)/3\rceil), số lượng này chỉ có thể giữ nguyên hoặc giảm đi. 
7. Các nhóm duy nhất còn lại hiện có kích thước 1 và 2. Xếp 2 nhóm 3 người vào mỗi băng ghế. Nếu (b) vẫn còn 2 nhóm, họ yêu cầu (\lceil b/3\rceil) ghế dài. Những băng ghế này có (6\lceil b/3\rceil-2b) chỗ ngồi chưa sử dụng, vì vậy hãy lấp đầy những chỗ ngồi đó với càng nhiều nhóm 1 càng tốt. 
8. Bất kỳ nhóm 1 nào còn lại yêu cầu mỗi sáu nhóm có một ghế dài, làm tròn lên. Thêm số này vào câu trả lời và in nó. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý kích thước nhóm từ lớn nhất đến nhỏ nhất, mọi nhóm đã xử lý sẽ được sắp xếp theo cách không thể tăng số lượng băng ghế tối thiểu cần thiết cho các nhóm chưa được xử lý. 

Một nhóm cỡ 6 không có sự lựa chọn. Nhóm có kích thước-5 chỉ có thể sử dụng nhóm có kích thước-1, do đó, việc ghép nối sẽ bắt buộc bất cứ khi nào có một nhóm tồn tại. Nhóm cỡ 4 có thể sử dụng nhóm 2 hoặc hai nhóm 1 và việc sử dụng nhóm 2 trước sẽ không bao giờ tốn thêm băng ghế dự bị sau này. Các nhóm Size-3 được ghép nối tối ưu với nhau, chỉ còn lại tối đa một nhóm 3 chưa từng có. Sau đó, chỉ còn lại kích thước 2 và 1, và tối đa có thể có ba nhóm 2 trong một băng ghế, vì vậy cần có băng ghế (\lceil c_2/3\rceil). Việc lấp đầy những chỗ chưa sử dụng của họ bằng nhóm 1 không có hại gì, vì nếu không những chỗ đó sẽ trống. 1 nhóm còn lại cần chính xác (\lceil c_1/6\rceil) băng ghế. 

Do đó, mọi quyết định đều bị ép buộc hoặc thay thế một bao bì có thể có bằng một bao bì khác mà không cần thêm băng ghế nữa. Do đó, số lượng cuối cùng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(c):
    c1, c2, c3, c4, c5, c6 = c

    ans = c6

    # Groups of 5 can only share with groups of 1.
    use = min(c1, c5)
    c1 -= use
    ans += c5

    # Groups of 4 should use groups of 2 first.
    use = min(c2, c4)
    c2 -= use
    remaining4 = c4 - use
    ans += c4

    # Remaining groups of 4 can take two groups of 1 each.
    use = min(c1, 2 * remaining4)
    c1 -= use

    # Pair groups of 3.
    ans += c3 // 2

    # One group of 3 may remain.
    if c3 % 2:
        ans += 1

        if c2 > 0:
            c2 -= 1
            if c1 > 0:
                c1 -= 1
        else:
            c1 -= min(c1, 3)

    # Pack groups of 2, three per bench.
    benches2 = (c2 + 2) // 3
    ans += benches2

    # Use the unused seats on those benches for groups of 1.
    free = 6 * benches2 - 2 * c2
    c1 = max(0, c1 - free)

    # Remaining groups of 1, six per bench.
    ans += (c1 + 5) // 6

    return ans

def main():
    t = int(input())
    out = []

    for _ in range(t):
        c = list(map(int, input().split()))
        out.append(str(solve_case(c)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Ba dòng đầu tiên bên trong`solve_case`giải nén sáu nhóm. Câu trả lời bắt đầu với tất cả các nhóm cỡ 6 vì mỗi nhóm trong số đó sử dụng toàn bộ băng ghế. 

Đối với kích thước 5,`min(c1, c5)`đưa ra chính xác có bao nhiêu nhóm 5 có thể nhận được 1 nhóm. 1 nhóm còn lại được lưu cho các nhóm nhỏ hơn. 

Đối với kích thước 4, trước tiên mã sẽ loại bỏ càng nhiều nhóm 2 càng tốt.`remaining4`là số 4 nhóm không lấy được 2 nhóm nên mỗi nhóm còn chỗ cho 2 nhóm 1. biểu hiện`2 * remaining4`là tổng số ghế còn trống. 

Việc xử lý kích thước 3 sử dụng phép chia số nguyên để ghép 3 nhóm. các`% 2`nhánh xử lý một nhóm duy nhất có thể chưa từng có. Mã ưu tiên 2 nhóm còn lại vì 3 nhóm cộng với 2 nhóm sử dụng dung lượng mà nếu không sẽ khó đặt hơn. Nếu cũng có sẵn 1 nhóm, băng ghế dự bị sẽ đầy đủ. 

Đối với 2 nhóm còn lại`(c2 + 2) // 3`là dạng số nguyên của (\lceil c_2/3\rceil). Số ghế trống trên các băng ghế đó được tính trực tiếp như sau:`6 * benches2 - 2 * c2`. Bất kỳ nhóm 1 nào còn lại sau đó sẽ được nhóm sáu người trên mỗi băng ghế. 

Tất cả các giá trị đều không âm và số nguyên Python không bị tràn. Việc sử dụng`min`Và`max`cũng ngăn chặn số lượng còn lại trở thành số âm. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên bao gồm sáu trường hợp thử nghiệm độc lập, mỗi trường hợp chứa chính xác một nhóm. 

| Nhóm đầu vào (c_1,c_2,c_3,c_4,c_5,c_6) | Hành động chính | Băng ghế | 
| --- | --- | --- | 
| 1 0 0 0 0 0 | Một nhóm 1 | 1 | 
| 0 1 0 0 0 0 | Một nhóm 2 người | 1 | 
| 0 0 1 0 0 0 | Một nhóm 3 người | 1 | 
| 0 0 0 1 0 0 | Một nhóm 4 người | 1 | 
| 0 0 0 0 1 0 | Một nhóm 5 người | 1 | 
| 0 0 0 0 0 1 | Một nhóm 6 người | 1 | 

Điều này xác nhận rằng mọi quy mô nhóm riêng lẻ đều được xử lý chính xác. Đặc biệt, trường hợp cỡ 6 không bao giờ cố gắng kết hợp nhóm này với nhóm khác. 

Đối với trường hợp thử nghiệm đầu tiên của mẫu thứ hai, số lượng là`6 9 5 8 3 2`. 

| Sân khấu | (c_1) | (c_2) | (c_3) | Đã thêm băng ghế | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 6 | 9 | 5 | 0 | 0 | 
| Cỡ 6 | 6 | 9 | 5 | 2 | 2 | 
| Kích thước 5 | 3 | 9 | 5 | 3 | 5 | 
| Kích thước 4 | 3 | 1 | 5 | 8 | 13 | 
| Cặp cỡ 3 | 3 | 1 | 1 | 2 | 15 | 
| Còn lại cỡ 3 | 2 | 0 | 0 | 1 | 16 | 
| Còn lại size 1 | 2 | 0 | 0 | 1 | 17 | 

Câu trả lời cuối cùng là 17. Tổng cộng có 98 người, vì vậy cần có ít nhất (\lceil98/6\rceil=17) ghế dài. Việc xây dựng tham lam đạt đến giới hạn dưới đó một cách chính xác. 

Đối với trường hợp thử nghiệm thứ hai của mẫu thứ hai, số lượng là`2 2 5 1 3 8`. 

| Sân khấu | (c_1) | (c_2) | (c_3) | Đã thêm băng ghế | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 2 | 2 | 5 | 0 | 0 | 
| Cỡ 6 | 2 | 2 | 5 | 8 | 8 | 
| Kích thước 5 | 0 | 2 | 5 | 2 | 10 | 
| Kích thước 4 | 0 | 1 | 5 | 1 | 11 | 
| Cặp cỡ 3 | 0 | 1 | 1 | 2 | 13 | 
| Còn lại cỡ 3 | 0 | 0 | 0 | 1 | 14 | 
| Còn lại cỡ 2 | 0 | 0 | 0 | 1 | 15 | 

Câu trả lời là 15. 3 nhóm chưa có đối thủ sẽ tiêu diệt 2 nhóm cuối cùng còn lại, không để lại nhóm nhỏ nào sau đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) cho mỗi trường hợp thử nghiệm | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | (O(1)) | Chỉ có sáu bộ đếm và một vài biến tạm thời được lưu trữ | 

Các ràng buộc cho phép tối đa 1.000 trường hợp kiểm thử, do đó toàn bộ chương trình chỉ thực hiện một lượng công việc không đổi cho mỗi trường hợp kiểm thử. Số lượng người tối đa, 10.000 người mỗi trường hợp, không ảnh hưởng đến thời gian thực hiện việc triển khai này. 

## Trường hợp thử nghiệm```python
# helper: run the solution logic on an input string
import sys
import io

def solve_case(c):
    c1, c2, c3, c4, c5, c6 = c

    ans = c6

    use = min(c1, c5)
    c1 -= use
    ans += c5

    use = min(c2, c4)
    c2 -= use
    remaining4 = c4 - use
    ans += c4

    use = min(c1, 2 * remaining4)
    c1 -= use

    ans += c3 // 2

    if c3 % 2:
        ans += 1
        if c2 > 0:
            c2 -= 1
            if c1 > 0:
                c1 -= 1
        else:
            c1 -= min(c1, 3)

    benches2 = (c2 + 2) // 3
    ans += benches2

    free = 6 * benches2 - 2 * c2
    c1 = max(0, c1 - free)

    ans += (c1 + 5) // 6

    return ans

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    t = int(sys.stdin.readline())
    out = []

    for _ in range(t):
        c = list(map(int, sys.stdin.readline().split()))
        out.append(str(solve_case(c)))

    sys.stdin = old_stdin
    return "\n".join(out)

# Provided sample 1
sample1 = """\
6
1 0 0 0 0 0
0 1 0 0 0 0
0 0 1 0 0 0
0 0 0 1 0 0
0 0 0 0 1 0
0 0 0 0 0 1
"""

assert run(sample1) == """\
1
1
1
1
1
1
""", "sample 1"

# Provided sample 2
sample2 = """\
19
6 9 5 8 3 2
2 2 5 1 3 8
1 2 0 4 0 0
4 1 5 5 2 8
8 6 6 0 0 0
4 8 8 6 0 3
2 2 2 9 5 2
8 1 7 6 3 10
8 9 7 10 6 6
7 3 8 5 2 1
3 5 5 8 6 10
9 2 6 3 9 5
10 8 5 2 0 5
3 7 1 1 4 4
0 9 2 0 5 8
5 5 1 10 6 2
1 5 5 10 3 5
2 5 7 0 1 9
5 3 9 1 4 5
"""

assert run(sample2) == """\
17
15
4
18
5
14
17
24
26
13
27
20
14
12
17
19
21
14
15
""", "sample 2"

# Minimum-size input
assert run("""\
1
1 0 0 0 0 0
""") == "1", "single group"

# Maximum number of people, all in groups of one
assert run("""\
1
10000 0 0 0 0 0
""") == "1667", "10000 groups of one"

# All groups have size three, with 9999 people
assert run("""\
1
0 0 3333 0 0 0
""") == "1667", "3333 groups of three"

# Exact-fit and near-exact-fit cases
assert run("""\
4
2 2 0 1 0 0
1 0 1 1 0 0
0 1 1 0 0 0
5 0 0 0 5 0
""") == """\
2
2
1
5
""", "boundary cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 0 0 0 0`|`1`| Đầu vào không trống tối thiểu | 
|`10000 0 0 0 0 0`|`1667`| Số người tối đa và phân chia trần | 
|`0 0 3333 0 0 0`|`1667`| Tất cả các nhóm đều có số lượng nhóm bằng nhau và lẻ | 
|`2 2 0 1 0 0`|`2`| Đóng gói chính xác (4+2) và các nhóm nhỏ còn lại | 
|`1 0 1 1 0 0`|`2`| Nhóm cỡ 4 cạnh tranh với nhóm cỡ 3 | 
|`0 1 1 0 0 0`|`1`| Đóng gói kiểu chính xác (3+2+1) sau 3 | 
|`5 0 0 0 5 0`|`5`| Nhóm cỡ 5 tiêu thụ nhóm cỡ 1 | 

## Vỏ cạnh 

Đối với một nhóm duy nhất, đầu vào là`1 0 0 0 0 0`. Thuật toán bỏ qua tất cả các nhóm lớn hơn, đạt đến phép tính cỡ 1 cuối cùng và tính toán`(1 + 5) // 6 = 1`. Kết quả là chính xác một băng ghế. 

Đối với một nhóm sáu người, đầu vào là`0 0 0 0 0 1`. ban đầu`ans = c6`ngay lập tức đóng góp một băng ghế. Không có hoạt động nào khác có thể kết hợp bất cứ điều gì với nhóm này vì năng lực của nó đã đầy. 

Đối với trường hợp phù hợp chính xác`0 1 0 1 0 0`, nhóm size-4 tiêu thụ nhóm size-2. Thuật toán thêm một băng ghế cho nhóm 4 và loại bỏ nhóm 2, tạo ra câu trả lời đúng là 1. 

Đối với nhóm ba người chưa từng có với nhóm hai người và nhóm một người, đầu vào`1 1 1 0 0 0`được xử lý bởi nhánh size-3. Nhóm 3 còn lại sử dụng nhóm 2 và nhóm 1, tạo ra một băng ghế đầy đủ sáu chỗ ngồi. 

Đối với nhiều nhóm một, đầu vào`10000 0 0 0 0 0`chỉ đạt đến giai đoạn cuối cùng. Sáu nhóm nằm trên mỗi băng ghế, vì vậy câu trả lời là (\lceil10000/6\rceil=1667). Trần nhà được thực hiện như`(c1 + 5) // 6`, tránh số học dấu phẩy động. 

Trường hợp tế nhị nhất là khi còn lại 3 nhóm nhưng không tồn tại 1 nhóm. Ví dụ,`0 1 1 0 0 0`cho một nhóm 3 và một nhóm 2. Thuật toán ghép chúng lại với nhau, để lại một chỗ chưa sử dụng. Như vậy vẫn là tối ưu vì việc tách chúng ra sẽ cần đến hai băng ghế, trong khi việc sử dụng nhóm 2 trên nhóm 3 hiện tại không thể làm tăng số lượng ghế cần thiết cho 2 nhóm còn lại.

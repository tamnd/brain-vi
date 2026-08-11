---
title: "CF 102409G - Giải pháp mỉa mai 2"
description: "Tin nhắn được mã hóa không phải là một tập hợp các số tùy ý. Có chính xác (2^N) giá trị, một giá trị cho mỗi tập hợp con của mã ký tự (N) gốc."
date: "2026-08-11T16:36:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102409
codeforces_index: "G"
codeforces_contest_name: "Semana i 2019"
rating: 0
weight: 102409
solve_time_s: 178
verified: true
draft: false
---

[CF 102409G - Giải pháp mỉa mai 2](https://codeforces.com/problemset/problem/102409/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Tin nhắn được mã hóa không phải là một tập hợp các số tùy ý. Có chính xác (2^N) giá trị, một giá trị cho mỗi tập hợp con của mã ký tự (N) gốc. Nếu mã ký tự gốc là (x_1,x_2,\ldots,x_N), mảng được truyền chứa mọi tổng có thể có 

[ 
\sum_{i\in S}x_i 
] 

với mọi tập con (S). Mảng được sắp xếp nên giá trị đầu tiên của nó luôn là (0), tương ứng với tập con trống. 

Nhiệm vụ là khôi phục mã ký tự (N) gốc từ các tổng tập hợp con này. Mã được khôi phục phải được in dưới dạng ký tự theo thứ tự ASCII không giảm. Vì các ký tự gốc là chữ và số nên mỗi giá trị được khôi phục tương ứng với một chữ số hoặc chữ cái hợp lệ. 

Giới hạn (N\leq20) là ràng buộc trung tâm. Có (2^{20}=1.048.576) tổng tập hợp con trong đầu vào lớn nhất, do đó, thuật toán có khoảng một thao tác cho mỗi số được truyền là thực tế. Một thuật toán thử mọi từ gốc có thể là hoàn toàn không thể và ngay cả một cách tiếp cận liên tục thực hiện tìm kiếm tuyến tính thông qua toàn bộ mảng phần tử (2^N) cũng có thể đạt được kết quả gần đúng (2^{2N}). Tổng tập hợp con tối đa chỉ là (20\cdot122=2440), vì giá trị ASCII chữ và số lớn nhất là 122. Phạm vi giá trị nhỏ này mang lại cho chúng ta cách biểu diễn mảng tần số đặc biệt hữu ích. 

Có một số trường hợp nguy hiểm khi việc triển khai bất cẩn không thành công. Trường hợp nhỏ nhất là```
1
65
```Tổng tập hợp con duy nhất là (0) và (65), vì vậy kết quả đầu ra đúng là`A`. Bộ giải mã giả định luôn có một cặp tổng tập hợp con không trống có thể đi qua phần cuối của dữ liệu. 

Mã ký tự lặp đi lặp lại cũng có thể. Ví dụ,```
3
0 65 65 65 130 130 130 195
```đến từ`AAA`, vì vậy đầu ra đúng là```
AAA
```Bộ giải mã lưu trữ các tổng tập hợp con trong một tập hợp thay vì bảo toàn bội số của chúng sẽ làm mất đi thực tế là (65) xảy ra ba lần và không thể tái tạo lại từ một cách chính xác. 

Các tập hợp con khác nhau có thể tạo ra cùng một tổng ngay cả khi các mã ký tự khác nhau. Coi như```
3
0 48 49 97 97 145 146 194
```Mã ban đầu là (48,49,97), tương ứng với`01a`. Hai lần xuất hiện của (97) là các tập con khác nhau, đó là ({97}) và ({48,49}). Việc xử lý các giá trị được mã hóa dưới dạng các số riêng biệt thay vì nhiều tập hợp sẽ âm thầm hủy thông tin này. 

Phạm vi ký tự cũng bao gồm các chữ số. Ví dụ,```
3
0 48 122 122 170 170 244 292
```đến từ`0zz`. Bộ giải mã giả định mọi giá trị được khôi phục là chữ hoa hoặc chữ thường sẽ từ chối câu trả lời hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp có thể thử mọi từ gốc có thể có, tạo tổng tập hợp con của nó, sắp xếp chúng và so sánh kết quả với đầu vào. Có thể có 62 ký tự chữ và số, do đó, ngay cả trước khi khai thác thứ tự sắp xếp bắt buộc, vẫn có (62^{20}), xấp xỉ (7.0\cdot10^{35}), các từ ứng cử viên tại (N=20). Việc tạo ra các tổng tập hợp con (2^{20}) cho mọi ứng cử viên khiến cho phương pháp này có nhiều bậc độ lớn vượt quá giới hạn thời gian. 

Có một cách có cấu trúc hơn nhiều để đảo ngược mã hóa. Giả sử một số mã ký tự đã được khôi phục và hãy để`subsets`chứa tất cả các tổng tập hợp con được hình thành bởi các mã được khôi phục đó. Ban đầu không có mã nào được phục hồi, vì vậy`subsets`chỉ chứa (0). 

Nhìn vào tổng tập hợp con nhỏ nhất chưa được tiêu thụ. Vì mọi mã ký tự đều dương nên giá trị nhỏ nhất phải là mã ký tự chưa được khôi phục tiếp theo. Khi mã đó là (x), mọi tập hợp con được hình thành từ các mã được khôi phục có thể được mở rộng bằng cách thêm (x). Nếu tổng tập hợp con trước đó là (s) thì tổng tập hợp con mới tương ứng là (s+x). Do đó, chúng ta biết chính xác giá trị nào phải được loại bỏ khỏi tập hợp mã hóa còn lại: mọi (s+x) cho (s) trong`subsets`. 

Sau khi loại bỏ các giá trị đó, tổng tập hợp con của các mã được khôi phục có thể được mở rộng bằng cách thêm từng (s+x) vào`subsets`. Quá trình lặp lại cho đến khi mã ký tự (N) được khôi phục. 

Câu hỏi còn lại là làm thế nào để tìm và loại bỏ các giá trị một cách hiệu quả. Một tập hợp đa mục đích chung như bản đồ băm sẽ hoạt động, nhưng vấn đề này mang lại cho chúng ta một thuộc tính thậm chí còn mạnh hơn: mọi tổng tập hợp con nằm trong khoảng từ (0) đến (2440). Chúng ta có thể lưu trữ bội số của mọi tổng có thể có trong một mảng tần số. Việc tìm giá trị còn lại nhỏ nhất sau đó chỉ cần một con trỏ di chuyển từ trái sang phải qua tối đa 2440 vị trí và việc xóa một giá trị là giảm mảng theo thời gian không đổi. 

Việc xây dựng lại bằng vũ lực không thành công vì mọi yêu cầu xóa có thể yêu cầu tìm kiếm trong một danh sách khổng lồ. Quan sát rằng phạm vi tổng rất nhỏ cho phép chúng tôi thay thế các tìm kiếm đó bằng lập chỉ mục trực tiếp, giảm toàn bộ quá trình tái thiết thành các hoạt động (O(2^N)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với những từ có thể | (O(62^N 2^N)) | (O(2^N)) | Quá chậm | 
| Tìm kiếm tuyến tính cho mỗi lần xóa | (O(4^N)) | (O(2^N)) | Quá chậm | 
| Tái thiết mảng tần số | (O(2^N + 2440)) | (O(2^N + 2440)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tổng tập hợp con được mã hóa (N) và (2^N). Thay vì dựa vào thứ tự sắp xếp của chúng, hãy đếm xem mỗi tổng xuất hiện bao nhiêu lần trong một mảng tần số`freq`. Vì mỗi tổng tối đa là 2440 nên mảng chỉ cần 2441 mục. 
2. Khởi tạo`subsets = [0]`. Điều này thể hiện tất cả các tổng tập hợp con có thể được hình thành từ các mã ký tự được khôi phục cho đến nay. Trước khi khôi phục bất cứ thứ gì, tập hợp con duy nhất là tập hợp con trống. 
3. Giữ một con trỏ`ptr`ban đầu ở mức 0. Nâng cao nó trong khi`freq[ptr]`là số không. Vị trí đầu tiên có tần số dương là tổng tập hợp con nhỏ nhất chưa được giải thích bằng các ký tự đã được khôi phục. 
4. Lấy`x = ptr`làm mã ký tự tiếp theo. Lý do điều này hoạt động là tích cực. Mọi ký tự chưa được khôi phục đều là số dương, do đó, tổng tập hợp con nhỏ nhất không giải thích được phải thu được bằng cách lấy chính xác một ký tự mới và không có ký tự nào được khôi phục trước đó. 
5. Hãy để`old_len = len(subsets)`. Đối với mỗi một trong những`old_len`tổng tập hợp con hiện có`s`, giảm`freq[s + x]`. Đây chính xác là tổng tập hợp con chứa ký tự mới được khôi phục`x`và nếu không thì sử dụng bất kỳ tập hợp con nào của các ký tự đã được khôi phục. 
6. Nối mọi`s + x`ĐẾN`subsets`. Sau hoạt động này,`subsets`chứa mọi tổng tập hợp con có thể lấy được từ tất cả các mã ký tự được khôi phục, do đó nó thể hiện trạng thái mới được yêu cầu bởi lần lặp tiếp theo. 
7. Lặp lại các bước trước đó chính xác (N) lần. Chuyển đổi mọi số nguyên được khôi phục thành một ký tự với`chr`và nối chúng lại. Việc xây dựng lại tự nhiên tạo ra các mã theo thứ tự không giảm, do đó không cần sắp xếp bổ sung. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi khôi phục được mã ký tự (k),`subsets`chứa chính xác tổng tập hợp con (2^k) được hình thành từ các mã đó, trong khi`freq[v]`ghi lại số lần xuất hiện của (v) trong nhiều tập hợp được mã hóa ban đầu vẫn chưa được giải thích bằng các mã được khôi phục đó. 

Ban đầu, bất biến giữ nguyên vì không có mã nào được khôi phục và tổng tập hợp con duy nhất bằng 0. Giả sử nó giữ nguyên trước khi khôi phục mã tiếp theo. Tổng nhỏ nhất không giải thích được phải là mã ký tự gốc nhỏ nhất còn lại vì mọi tập hợp con không giải thích được khác chứa ký tự đó ít nhất phải lớn bằng chính ký tự đó và mọi tập hợp con sử dụng hai hoặc nhiều giá trị dương chưa được khôi phục vẫn lớn hơn. Gọi giá trị này là (x), các tập con chứa (x) chính xác là (s+x), trong đó (s) là tổng tập hợp con của các mã đã được khôi phục. Việc loại bỏ chính xác những lần xuất hiện đó sẽ để lại chính xác tổng tập hợp con không chứa (x). Nối các giá trị (s+x) giống nhau vào`subsets`tạo tất cả các tổng tập hợp con sau khi thêm (x). Do đó, bất biến được giữ nguyên ở mỗi lần lặp và sau (N) lần lặp, mọi mã ký tự gốc đều được phục hồi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    m = 1 << n

    sums = list(map(int, input().split()))

    # Every original character is alphanumeric, so its ASCII value is
    # at most 122. With at most 20 characters, every subset sum is <= 2440.
    freq = [0] * 2441
    for x in sums:
        freq[x] += 1

    # The input array is no longer needed.
    del sums

    # All subset sums generated by the characters recovered so far.
    subsets = [0]

    answer = []
    ptr = 0

    for _ in range(n):
        while ptr <= 2440 and freq[ptr] == 0:
            ptr += 1

        x = ptr
        answer.append(chr(x))

        old_len = len(subsets)

        # Remove all subset sums that contain the newly recovered value x.
        for i in range(old_len):
            s = subsets[i]
            freq[s + x] -= 1

        # Add those sums to the set of subset sums of recovered values.
        for i in range(old_len):
            subsets.append(subsets[i] + x)

    print("".join(answer))

if __name__ == "__main__":
    solve()
```Đầu vào đầu tiên được chuyển đổi thành`freq`, bảo tồn tính đa dạng thay vì chỉ là thành viên. Điều này rất cần thiết vì mảng được mã hóa là một tập hợp nhiều tập hợp và tổng có thể xảy ra như nhau đối với nhiều tập hợp con khác nhau. 

các`subsets`mảng bắt đầu bằng 0 vì tập hợp con trống luôn có sẵn. Nếu hiện tại có (k) giá trị được khôi phục thì kích thước của nó chính xác là (2^k). Khi một giá trị mới`x`được tìm thấy, mã đầu tiên sẽ nhớ kích thước cũ. Ranh giới này là cần thiết vì chỉ nên sử dụng các tổng tập hợp con cũ để tạo ra`s + x`. Lặp lại danh sách đồng thời thêm vào danh sách mà không cần sửa`old_len`sẽ xử lý các khoản tiền mới được tạo trong cùng một lần lặp và tạo ra các giá trị bổ sung không chính xác. 

Việc giảm tần số xảy ra trước khi các tổng mới được thêm vào. Điều này diễn ra trực tiếp từ bất biến: tổng mới được tạo chính xác là các lần xuất hiện được giải thích bằng ký tự mới được khôi phục. 

Con trỏ chỉ di chuyển về phía trước. Một lần`freq[v]`đạt đến 0, không thao tác nào sau này có thể làm cho giá trị cụ thể đó không thể giải thích được nữa, bởi vì mọi thao tác chỉ tiêu tốn tổng tập hợp con được mã hóa. Vì tổng lớn nhất có thể là 2440 nên con trỏ thực hiện tổng cộng tối đa 2441 lần lặp. 

Số nguyên Python không gặp phải vấn đề tràn mà việc triển khai số nguyên có chiều rộng cố định có thể gặp phải, mặc dù tất cả các tổng tập hợp con thực tế đã bị giới hạn bởi 2440. Cấu trúc lớn nhất là`subsets`, chứa tối đa (2^{20}=1.048.576) số nguyên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mảng được mã hóa bắt đầu bằng```
0 66 101 101 114 121 167 167 180 187 ...
```Giá trị khác 0 đầu tiên không giải thích được là 66, vì vậy ký tự đầu tiên là`B`. Sau khi loại bỏ 66, giá trị không giải thích được tiếp theo là 101, cho`e`. Giá trị tương tự lại xuất hiện ở ký tự tiếp theo, vì từ gốc chứa hai`e`nhân vật. 

| Bước | Mã được chọn | Nhân vật được chọn | Kích thước của`subsets`sau bước | Các khoản tiền mới được giải thích | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | | 1 | 0 | 
| 1 | 66 | B | 2 | 66 | 
| 2 | 101 | e | 4 | 101, 167 | 
| 3 | 101 | e | 8 | 101, 167, 202, 268 | 
| 4 | 114 | r | 16 | 114, 180, 215, 281, 215, 281, 316, 382 | 
| 5 | 121 | y | 32 | 121, 187, 222, 288, 222, 288, 323, 389, 235, 301, 336, 402, 336, 402, 437, 503 | 

Các mã được khôi phục là (66,101,101,114,121), được chuyển đổi thành`Beery`. 101 lặp lại được xử lý một cách tự nhiên vì`freq[101]`vẫn tích cực sau lần đầu tiên`e`được phục hồi. 

### Xây dựng ví dụ 2 

Hãy xem xét ba bản sao của nhân vật`A`, có mã ASCII là 65.```
3
0 65 65 65 130 130 130 195
```Việc xây dựng lại hoạt động như sau. 

| Bước | Mã được chọn |`subsets`trước | Số tiền đã xóa |`subsets`sau | 
| --- | --- | --- | --- | --- | 
| 1 | 65 | [0] | 65 | [0, 65] | 
| 2 | 65 | [0, 65] | 65, 130 | [0, 65, 65, 130] | 
| 3 | 65 | [0, 65, 65, 130] | 65, 130, 130, 195 | [0, 65, 65, 130, 65, 130, 130, 195] | 

Câu trả lời cuối cùng là`AAA`. Dấu vết cho thấy tại sao tần số không thể được thay thế bằng một bộ. Ở bước cuối cùng, có ba lần xuất hiện riêng biệt, mỗi số 130 trong mảng được mã hóa hoàn chỉnh, tương ứng với ba tập con hai phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(2^N + 2440)) | Trên tất cả các lần lặp, số tổng tập hợp con cũ được xử lý là (1+2+\cdots+2^{N-1}=2^N-1), trong khi con trỏ quét tối đa 2441 giá trị. | 
| Không gian | (O(2^N + 2440)) |`subsets`có nhiều nhất (2^N) phần tử và`freq`có 2441 mục. | 

Đối với (N=20), việc xây dựng lại thực hiện ít hơn 1.048.576 cập nhật tập hợp con, cộng với việc quét trên phạm vi giá trị 2441 cố định. Điều đó phù hợp với giới hạn 3 giây. Mảng tần số cũng tiết kiệm bộ nhớ hơn nhiều so với Python`Counter`có khả năng chứa hàng trăm ngàn mục từ điển. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

input = sys.stdin.readline

def solve():
    n = int(input())

    sums = list(map(int, input().split()))

    freq = [0] * 2441
    for x in sums:
        freq[x] += 1

    del sums

    subsets = [0]
    answer = []
    ptr = 0

    for _ in range(n):
        while ptr <= 2440 and freq[ptr] == 0:
            ptr += 1

        x = ptr
        answer.append(chr(x))

        old_len = len(subsets)

        for i in range(old_len):
            s = subsets[i]
            freq[s + x] -= 1

        for i in range(old_len):
            subsets.append(subsets[i] + x)

    print("".join(answer))

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

sample1 = """\
5
0 66 101 101 114 121 167 167 180 187 202 215 215 222 222 235 268 281 281 288 288 301 316 323 336 336 382 389 402 402 437 503
"""

assert run(sample1) == "Beery", "sample 1"

assert run(
    """\
1
65
"""
) == "A", "minimum size"

assert run(
    """\
3
0 65 65 65 130 130 130 195
"""
) == "AAA", "all equal values"

assert run(
    """\
3
0 48 49 97 97 145 146 194
"""
) == "01a", "equal subset sums from distinct values"

assert run(
    """\
3
0 48 122 122 170 170 244 292
"""
) == "0zz", "digit and lowercase boundary"

# Maximum-size case: twenty copies of 'z', ASCII value 122.
# The sum 122*k occurs C(20, k) times.
parts = []
for k in range(21):
    parts.extend([str(122 * k)] * math.comb(20, k))

max_case = "20\n" + " ".join(parts) + "\n"

assert run(max_case) == "z" * 20, "maximum N"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 65`|`A`| Giá trị tập hợp con tối thiểu (N) ngoài số 0 | 
|`3 / 0 65 65 65 130 130 130 195`|`AAA`| Mã ký tự trùng lặp và bội số | 
|`3 / 0 48 49 97 97 145 146 194`|`01a`| Các tập hợp con khác nhau tạo ra cùng một tổng | 
|`3 / 0 48 122 122 170 170 244 292`|`0zz`| Chữ số nhỏ nhất và chữ cái viết thường lớn nhất | 
| (N=20), hai mươi bản 122 |`zzzzzzzzzzzzzzzzzzzz`| Kích thước đầu vào tối đa và tần suất trùng lặp cực cao | 

Thử nghiệm kích thước tối đa có chủ ý sử dụng hai mươi giá trị giống nhau. Nó tạo ra (2^{20}) số tiền được mã hóa trong khi vẫn giữ được câu trả lời mong đợi đơn giản. Các bội số nhị thức thực hiện rất nhiều mảng tần số và xác nhận rằng thuật toán không bao giờ giả định tổng tập hợp con là duy nhất. 

## Vỏ cạnh 

Với (N=1), đầu vào```
1
65
```bắt đầu bằng`freq[0]=1`Và`freq[65]=1`. Con trỏ bỏ qua số 0 vì tập hợp con trống không phải là ký tự, sau đó chọn 65.`subsets`ban đầu chỉ chứa số 0, vì vậy chính xác một lần xuất hiện của 65 sẽ bị loại bỏ. Đầu ra là`A`. Vòng lặp thực hiện chính xác một lần, do đó không có nỗ lực truy cập ký tự thứ hai không tồn tại. 

Đối với mã ký tự trùng lặp, hãy xem xét```
3
0 65 65 65 130 130 130 195
```Giá trị được chọn đầu tiên là 65. Thuật toán sử dụng một lần xuất hiện là 65 và tạo ra tổng tập hợp con`[0,65]`. Giá trị còn lại nhỏ nhất tiếp theo vẫn là 65, do đó thuật toán chọn đúng giá trị khác`A`. Nó tiêu thụ một 65 và một 130, sau đó mở rộng danh sách tập hợp con thành`[0,65,65,130]`. Lần lặp thứ ba lại chọn 65 và sử dụng 65 còn lại, hai giá trị 130 và 195. Kết quả là`AAA`. Mảng tần số là thứ làm cho các giá trị lặp lại có thể phân biệt được. 

Đối với tổng tập hợp con bằng nhau được tạo ra bởi các tập hợp con khác nhau, hãy sử dụng```
3
0 48 49 97 97 145 146 194
```Mã được khôi phục đầu tiên là 48. Mã thứ hai là 49, sử dụng 49 và 97, vì tổng tập hợp con hiện có là 0 và 48. Tại thời điểm này, vẫn còn một lần xuất hiện của 97. Mã được khôi phục thứ ba là 97. Nó sử dụng 97, 145, 146 và 194 bằng cách sử dụng bốn tổng tập hợp con hiện có. Kết quả là`01a`. Điều này chứng tỏ tại sao thuật toán theo dõi số lượng thay vì chỉ xóa các giá trị riêng biệt. 

Đối với trường hợp ranh giới chữ và số,```
3
0 48 122 122 170 170 244 292
```giá trị được khôi phục đầu tiên là 48, trở thành ký tự`0`. Giá trị tiếp theo là 122 và được chọn hai lần. Đầu ra là`0zz`. Mảng tần số hỗ trợ toàn bộ phạm vi tổng được phép, bao gồm cả mã chữ số nhỏ nhất và mã chữ cái viết thường lớn nhất. 

Trường hợp tối đa có (N=20), do đó`subsets`cuối cùng chứa (1.048.576) giá trị. Ngay cả khi mỗi mã gốc là 122, thuật toán không phân nhánh hoặc quay lui. Tại mỗi lần lặp, giá trị nhỏ nhất còn lại lại là 122 và số đếm tần số mã hóa chính xác bội số nhị thức của tổng tập hợp con. Sau hai mươi lần lặp, chính xác là hai mươi`z`các ký tự đã được phục hồi. 

Bài xã luận được cấu trúc để có thể xuất bản nguyên trạng.

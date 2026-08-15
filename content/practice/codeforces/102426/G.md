---
title: "CF 102426G - \u4f19\u4f34\u7cfb\u7edf"
description: "Hệ thống chỉ duy trì bộ nhớ trống thông qua 11 bộ đếm. Bộ đếm i lưu trữ bao nhiêu khối trống có kích thước 2^i, trong đó i nằm trong khoảng từ 0 đến 10."
date: "2026-08-14T15:18:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102426
codeforces_index: "G"
codeforces_contest_name: "The 7-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 102426
solve_time_s: 125
verified: true
draft: false
---

[CF 102426G - \u4f19\u4f34\u7cfb\u7edf](https://codeforces.com/problemset/problem/102426/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hệ thống chỉ duy trì bộ nhớ trống thông qua 11 bộ đếm. Quầy tính tiền`i`lưu trữ có bao nhiêu khối miễn phí có kích thước`2^i`, Ở đâu`i`nằm trong khoảng từ 0 đến 10. Chúng ta không bao giờ cần phải nhớ vị trí vật lý của các khối, bởi vì mô hình đơn giản hóa nói rõ ràng rằng các khối tự do không thể được hợp nhất. 

MỘT`free n`hoạt động giới thiệu một khối kích thước miễn phí mới`n`. Vì hệ thống chỉ lưu lũy thừa của 2 nên chúng ta biểu diễn`n`bằng sự phân rã nhị phân của nó. Ví dụ,`13 = 8 + 4 + 1`, do đó việc giải phóng một khối có kích thước 13 sẽ làm tăng bộ đếm cho các kích thước 8, 4 và 1. 

Một`allocate m`hoạt động hoạt động khác nhau. Chúng ta phải tìm khối lũy thừa hai nhỏ nhất có kích thước ít nhất là`m`và bộ đếm của nó dương. Nếu khối đó có kích thước`s`, nó được tiêu thụ và để lại phần còn lại`s - m`. Phần còn lại được xử lý giống hệt như một khối mới được giải phóng, do đó việc phân tách nhị phân của nó được chèn vào các bộ đếm. Nếu không tồn tại khối lũy thừa 2 phù hợp thì hoạt động sẽ thất bại và trạng thái không thay đổi. 

Đầu vào bắt đầu bằng một số nguyên`k`, theo sau là`k`hoạt động. Sau mỗi thao tác thành công, chúng tôi in tất cả 11 bộ đếm từ cỡ 1 đến cỡ 1024. Sau khi phân bổ không thành công, chúng tôi in`ERROR!`và không sửa đổi bộ đếm. 

Trình tự có thể chứa tới`10^5`hoạt động. Số lượng kích thước khối có thể có chỉ là 11, đây là hạn chế chính về cấu trúc. Một thuật toán thực hiện một lượng công việc không đổi trên 11 kích thước này cho mỗi thao tác là đủ nhanh. Ngược lại, việc triển khai quét mọi kích thước số nguyên có thể có từ 1 đến 1024 cho mỗi lần phân bổ sẽ thực hiện khoảng`1024 * 10^5 = 10^8`kiểm tra trong trường hợp xấu nhất, điều này gây tốn kém không cần thiết trong Python và không còn chỗ cho chi phí đầu vào và đầu ra. 

Có một số trường hợp đặc biệt trong đó việc triển khai trực tiếp có thể âm thầm gặp trục trặc. Đầu tiên là phân bổ có kích thước được yêu cầu đã là lũy thừa của hai. Ví dụ:```
3
free 8
allocate 8
allocate 1
```Đầu ra đúng là:```
0 0 0 0 0 0 0 0 0 0 0
ERROR!
```Khối có kích thước 8 được sử dụng chính xác nên phần còn lại của nó bằng 0. Việc thực hiện bất cẩn luôn chèn vào`s - m`không kiểm tra số 0 có thể vô tình coi số 0 là một khối. 

Trường hợp cạnh thứ hai là phân bổ cần khối lớn hơn và để lại phần còn lại chứa một số thành phần nhị phân:```
2
free 16
allocate 13
```Đầu ra đúng là:```
1 1 0 0 0 0 0 0 0 0 0
```Khối 16 được sử dụng, để lại 3, phân rã thành 2 và 1. Việc triển khai chỉ ghi phần còn lại dưới dạng một số nguyên sẽ vi phạm cách biểu diễn của hệ thống. 

Trường hợp cạnh thứ ba là lỗi mặc dù các khối trống nhỏ hơn có tổng bộ nhớ đủ:```
2
free 1
allocate 2
```Đầu ra đúng là:```
1 0 0 0 0 0 0 0 0 0 0
ERROR!
```Hệ thống không thể kết hợp khối cỡ 1 hiện có thành khối cỡ 2. Bộ đếm mô tả các khối độc lập, không phải là một nhóm bộ nhớ tổng hợp. 

Trường hợp cạnh thứ tư xảy ra khi một khối phù hợp tồn tại ở lũy thừa lớn hơn bằng 2 trong khi tồn tại lũy thừa nhỏ hơn nhưng không đủ riêng lẻ:```
3
free 1
free 8
allocate 2
```Đầu ra đúng là:```
1 0 1 0 0 0 0 0 0 0 0
```Khối size-1 không thể đáp ứng yêu cầu nên khối size-8 được chọn. Phần còn lại của nó là 6, trở thành 4 và 2. Cùng với khối cỡ 1 ban đầu, các bộ đếm trở thành`1 0 1`, tương ứng với kích thước 1, 2 và 4. 

## Phương pháp tiếp cận 

Việc triển khai đơn giản có thể lưu trữ 11 bộ đếm và, đối với`free n`, liên tục tìm lũy thừa lớn nhất của hai không vượt quá giá trị còn lại. Từ`n < 2048`, điều này tạo ra tối đa 11 mảnh. Để phân bổ, phiên bản brute-force đơn giản nhất có thể quét mọi kích thước số nguyên từ`m`đến 1024 và hỏi xem có tồn tại khối trống có kích thước chính xác như vậy không. Vì chỉ có lũy thừa của hai được biểu diễn nên hầu hết các lần kiểm tra đó đều vô dụng ngay lập tức, nhưng việc triển khai vẫn thực hiện tới 1024 lần kiểm tra cho một lần phân bổ. Với`10^5`hoạt động, trường hợp xấu nhất đạt khoảng`1024 * 10^5 = 10^8`kiểm tra, quá nhiều so với giới hạn cuộc thi 1 giây, đặc biệt là với Python. 

Ý tưởng vũ phu vẫn hữu ích vì nó tiết lộ sự chuyển đổi trạng thái thực. Khi một khối có kích thước phù hợp`s`được tìm thấy, chỉ có hai thay đổi: giảm bộ đếm cho`s`, sau đó chèn phân tách nhị phân của`s - m`. Không có gì về lịch sử hoặc vị trí vật lý của một khối. 

Quan sát quan trọng là hệ thống chỉ có 11 kích thước khối có thể. Chúng ta nên tìm kiếm trực tiếp 11 lớp này thay vì quét tất cả các kích thước nguyên. Bắt đầu từ lũy thừa nhỏ nhất của hai ít nhất là`m`, chúng tôi kiểm tra kích thước`2^j`theo thứ tự tăng dần cho đến khi tìm được bộ đếm khác rỗng. Có nhiều nhất 11 lần kiểm tra cho mỗi lần phân bổ, vì vậy toàn bộ mô phỏng có hiệu quả tuyến tính theo`k`. 

Cách biểu diễn tương tự cũng làm cho`free n`đơn giản. Biểu diễn nhị phân của một số nguyên cho chúng ta biết chính xác lũy thừa nào của hai xuất hiện trong quá trình phân tách cần thiết của nó. Chúng ta có thể kiểm tra 11 bit của`n`và tăng các bộ đếm tương ứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k × 1024) | O(11) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | O(k × 11) | O(11) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`cnt`có độ dài 11, ban đầu được điền bằng 0.`cnt[i]`đại diện cho số lượng khối kích thước miễn phí`2^i`. 
2. Đối với một`free n`hoạt động, kiểm tra 11 bit của`n`. Bất cứ khi nào bit`i`được thiết lập, tăng`cnt[i]`. Đây chính xác là sự phân rã cần thiết vì mỗi số nguyên dương có một biểu diễn nhị phân duy nhất và mỗi bit tương ứng với một khối lũy thừa hai được phép. 
3. Đối với một`allocate m`thao tác, tìm chỉ số nhỏ nhất`j`như vậy`2^j >= m`. Đây là kích thước khối nhỏ nhất có thể đáp ứng được yêu cầu. 
4. Bắt đầu từ chỉ mục`j`, tìm kiếm lên trên chỉ mục 10 cho đến khi tìm thấy chỉ mục`p`với`cnt[p] > 0`. Việc tìm kiếm hướng lên trên là cần thiết vì bộ cấp phát phải sử dụng khối nhỏ nhất có sẵn có kích thước ít nhất là`m`, chứ không phải là một khối tùy ý phù hợp. 
5. Nếu không có chỉ mục nào tồn tại, hãy in`ERROR!`và rời đi`cnt`không thay đổi. Yêu cầu thất bại không tiêu tốn hoặc biến đổi bất kỳ bộ nhớ nào. 
6. Nếu chỉ số`p`được tìm thấy, hãy`s = 2^p`. Giảm bớt`cnt[p]`bởi một vì khối này hiện đang được phân bổ. 
7. Tính toán`r = s - m`. Nếu như`r > 0`, phân hủy`r`bằng các bit nhị phân của nó và tăng các bộ đếm tương ứng. Nếu như`r = 0`, không có gì được trả về bảng bộ nhớ trống. 
8. In toàn bộ`cnt`mảng sau thao tác. Mảng tương tự được sử dụng lại cho thao tác tiếp theo, do đó, quá trình mô phỏng sẽ chuyển trạng thái bộ nhớ hiện tại về phía trước một cách tự nhiên. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi thao tác được xử lý thành công,`cnt[i]`chính xác là số khối có kích thước trống`2^i`hiện được đại diện bởi hệ thống. 

MỘT`free n`hoạt động bảo toàn bất biến này vì các phân vùng phân rã nhị phân`n`thành các lũy thừa được phép riêng biệt của hai, khớp chính xác với biểu diễn được yêu cầu của hệ thống. Để phân bổ, bộ đếm khả dụng đầu tiên bằng hoặc cao hơn kích thước yêu cầu chính xác là khối pháp lý nhỏ nhất mà đặc tả cho phép chúng ta chọn. Loại bỏ khối đó chiếm bộ nhớ được phân bổ, đồng thời phân tách`s - m`thêm chính xác phần còn lại miễn phí vào biểu diễn. Việc phân bổ không thành công không thay đổi gì cả, vì vậy tính bất biến cũng được giữ nguyên trong trường hợp đó. Bằng cách quy nạp theo trình tự hoạt động, mọi trạng thái được in đều chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def add_block(n, cnt):
    bit = 0
    while n:
        if n & 1:
            cnt[bit] += 1
        n >>= 1
        bit += 1

def solve():
    k = int(input())
    cnt = [0] * 11
    out = []

    for _ in range(k):
        op, x = input().split()
        x = int(x)

        if op == "free":
            add_block(x, cnt)
            out.append(" ".join(map(str, cnt)))
            continue

        # Find the smallest power of two >= x.
        size = 1
        idx = 0
        while size < x:
            size <<= 1
            idx += 1

        # Find the smallest available block that can satisfy x.
        while idx < 11 and cnt[idx] == 0:
            idx += 1

        if idx == 11:
            out.append("ERROR!")
            continue

        block_size = 1 << idx
        cnt[idx] -= 1

        remainder = block_size - x
        if remainder:
            add_block(remainder, cnt)

        out.append(" ".join(map(str, cnt)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`cnt`mảng là toàn bộ trạng thái có thể thay đổi của mô phỏng. Không cần phải lưu trữ các khối riêng lẻ vì các khối có cùng kích thước lũy thừa hai là không thể phân biệt được theo quy tắc. 

các`add_block`hàm sử dụng biểu diễn nhị phân trực tiếp. biểu hiện`n & 1`kiểm tra bit có ý nghĩa nhỏ nhất hiện tại và dịch chuyển`n`phải chuyển sang bit tiếp theo. Vì mọi kích thước đầu vào đều dưới 2048 nên cần tối đa 11 lần lặp. 

Để phân bổ,`size`bắt đầu từ 1 và tăng gấp đôi cho đến khi đạt hoặc vượt quá`x`. Kết quả`idx`là chỉ số kích thước khối hợp pháp nhỏ nhất. Vòng lặp tiếp theo chỉ kiểm tra các chỉ số từ đó trở lên, do đó nó thực hiện thứ tự ưu tiên chính xác của bộ cấp phát. 

ranh giới`idx < 11`là điều cần thiết. Chỉ số 10 đại diện cho kích thước 1024, khối lớn nhất được phép. Nếu tìm kiếm đạt tới 11, không khối nào có thể đáp ứng yêu cầu. Các bộ đếm không được thay đổi trước khi kiểm tra lỗi này, việc này tự động thực hiện yêu cầu bỏ qua việc phân bổ không thành công. 

Khi phân bổ thành công, khối ban đầu sẽ giảm đi trước khi phần còn lại của nó được chèn vào. Nếu như`block_size == x`, phần còn lại bằng 0 và`if remainder`bảo vệ ngăn chặn mọi phân tách kích thước 0 không hợp lệ. 

Số nguyên Python không tràn ở đây và tối đa tất cả các bộ đếm đều`10^5`cộng với số phần được tạo ra bởi các phép toán, do đó số học số nguyên thông thường là đủ. Đầu ra được tích lũy thành một danh sách và được ghi một lần ở cuối, giúp tránh phải trả chi phí cho nhiều lệnh gọi đầu ra riêng biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hoạt động đầu tiên cố gắng phân bổ trong khi hệ thống trống. Hoạt động thứ hai tạo khối có kích thước 1024. Phân bổ cuối cùng sử dụng khối đó để đáp ứng yêu cầu về kích thước 1. 

| Hoạt động | Trạng thái bắt đầu | Khối đã chọn | Phần còn lại | Trạng thái cuối cùng | 
| --- | --- | --- | --- | --- | 
|`allocate 1`| tất cả đều bằng không | không | không |`ERROR!`| 
|`free 1024`| tất cả đều bằng không | không | không |`0 0 0 0 0 0 0 0 0 0 1`| 
|`allocate 1`| cỡ 1024 có sẵn | 1024 | 1023 |`1 1 1 1 1 1 1 1 1 1 0`| 

1023 còn lại là`512 + 256 + 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1`, do đó mọi bộ đếm từ chỉ số 0 đến chỉ số 9 đều trở thành một. Điều này chứng tỏ tại sao phần còn lại phải được phân hủy thay vì được lưu trữ dưới dạng một khối duy nhất. 

### Mẫu 2 

Mỗi`free`hoạt động chỉ đơn giản là thêm các thành phần nhị phân của khối được phát hành. Vì các khối được phát hành là riêng biệt nên các khối có kích thước bằng nhau sẽ tích lũy trong bộ đếm của chúng và không bao giờ hợp nhất. 

| Hoạt động | Đã thêm phân hủy | Trạng thái truy cập | 
| --- | --- | --- | 
|`free 1`|`1`|`1 0 0 0 0 0 0 0 0 0 0`| 
|`free 1`|`1`|`2 0 0 0 0 0 0 0 0 0 0`| 
|`free 1`|`1`|`3 0 0 0 0 0 0 0 0 0 0`| 
|`free 2`|`2`|`3 1 0 0 0 0 0 0 0 0 0`| 
|`free 2`|`2`|`3 2 0 0 0 0 0 0 0 0 0`| 

Trạng thái cuối cùng chứa ba khối kích thước-1 độc lập và hai khối kích thước-2 độc lập. Mặc dù hai khối có kích thước 1 có kích thước kết hợp là 2 nhưng chúng không thể được hợp nhất vì mô hình không theo dõi vị trí vật lý của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k × 11) | Mọi thao tác kiểm tra tối đa 11 lũy thừa được hỗ trợ của hai | 
| Không gian | O(k + 11) | Mảng bộ đếm có 11 mục và bộ đệm triển khai tối đa một chuỗi đầu ra cho mỗi thao tác | 

Với`k <= 10^5`, thuật toán chỉ thực hiện vài triệu phép tính số nguyên nhỏ. Bản thân trạng thái bộ nhớ có kích thước không đổi và bộ đệm đầu ra tỷ lệ thuận với kích thước đầu ra được yêu cầu. Điều này hoàn toàn thoải mái trong phạm vi độ phức tạp dự định đối với các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def add_block(n, cnt):
    bit = 0
    while n:
        if n & 1:
            cnt[bit] += 1
        n >>= 1
        bit += 1

def solve_text(data):
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(data)

    k = int(sys.stdin.readline())
    cnt = [0] * 11
    out = []

    for _ in range(k):
        op, x = sys.stdin.readline().split()
        x = int(x)

        if op == "free":
            add_block(x, cnt)
            out.append(" ".join(map(str, cnt)))
            continue

        size = 1
        idx = 0
        while size < x:
            size <<= 1
            idx += 1

        while idx < 11 and cnt[idx] == 0:
            idx += 1

        if idx == 11:
            out.append("ERROR!")
            continue

        block_size = 1 << idx
        cnt[idx] -= 1

        remainder = block_size - x
        if remainder:
            add_block(remainder, cnt)

        out.append(" ".join(map(str, cnt)))

    sys.stdin = old_stdin
    return "\n".join(out)

# Sample 1
sample1 = """3
allocate 1
free 1024
allocate 1
"""

expected1 = """ERROR!
0 0 0 0 0 0 0 0 0 0 1
1 1 1 1 1 1 1 1 1 1 0"""

assert solve_text(sample1) == expected1, "sample 1"

# Sample 2
sample2 = """5
free 1
free 1
free 1
free 2
free 2
"""

expected2 = """1 0 0 0 0 0 0 0 0 0 0
2 0 0 0 0 0 0 0 0 0 0
3 0 0 0 0 0 0 0 0 0 0
3 1 0 0 0 0 0 0 0 0 0
3 2 0 0 0 0 0 0 0 0"""

assert solve_text(sample2) == expected2, "sample 2"

# Minimum-size input
case_min = """1
free 1
"""
assert solve_text(case_min) == "1 0 0 0 0 0 0 0 0 0 0", "minimum size"

# Exact power of two, followed by an impossible allocation
case_power = """3
free 8
allocate 8
allocate 1
"""
expected_power = """0 0 0 0 0 0 0 0 0 0 0
ERROR!"""
assert solve_text(case_power) == expected_power, "exact power of two"

# Remainder decomposition and smallest-fitting-block rule
case_remainder = """3
free 16
free 1
allocate 13
"""
expected_remainder = """0 0 0 0 1 0 0 0 0 0 0
1 0 0 0 1 0 0 0 0 0 0
1 1 0 0 0 0 0 0 0 0 0"""
assert solve_text(case_remainder) == expected_remainder, "remainder decomposition"

# Maximum block size and allocation of a non-power-of-two value
case_max = """2
free 1024
allocate 1023
"""
expected_max = """0 0 0 0 0 0 0 0 0 0 1
1 0 0 0 0 0 0 0 0 0 0"""
assert solve_text(case_max) == expected_max, "maximum block size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / free 1`|`1 0 0 0 0 0 0 0 0 0 0`| Kích thước khối tối thiểu được phép | 
|`free 8 / allocate 8 / allocate 1`| Khi đó trạng thái không`ERROR!`| Phù hợp chính xác và không còn lại | 
|`free 16 / free 1 / allocate 13`| Còn size 1 và size 2 | Phân tách chính xác và lựa chọn khối phù hợp nhỏ nhất | 
|`free 1024 / allocate 1023`| Vẫn còn một khối cỡ 1 | Kích thước được hỗ trợ tối đa và phần còn lại của ranh giới | 

## Vỏ cạnh 

###Vừa vặn chính xác 

cho```
3
free 8
allocate 8
allocate 1
```

`free 8`bộ`cnt[3]`đến một. Yêu cầu 8 bắt đầu ở chỉ số 3 và ngay lập tức tìm thấy khối đó. Nó trừ đi một và nhận được số dư bằng 0, do đó trạng thái hoàn toàn bằng 0. Yêu cầu tiếp theo cho 1 tìm kiếm các chỉ số từ 0 đến 10 và không tìm thấy gì, tạo ra`ERROR!`. Đầu ra cuối cùng chính xác là:```
0 0 0 0 0 0 0 0 0 0 0
ERROR!
```Chi tiết quan trọng là việc phân bổ chính xác không tạo ra khối trống có kích thước bằng 0. 

### Số dư có bội lũy thừa của hai 

cho```
2
free 16
allocate 13
```thao tác đầu tiên tạo ra một khối có kích thước 16. Việc phân bổ bắt đầu ở kích thước 16 vì đây là lũy thừa nhỏ nhất của hai không nhỏ hơn 13. Sau khi tiêu thụ nó, phần còn lại là`16 - 13 = 3`. Phân rã nhị phân cho`3 = 2 + 1`, Vì thế`cnt[0]`Và`cnt[1]`cả hai trở thành một. Đầu ra là:```
1 1 0 0 0 0 0 0 0 0 0
```Điều này bắt các triển khai quên phân hủy phần còn lại. 

### Các khối nhỏ riêng biệt không thể hợp nhất 

cho```
2
free 1
allocate 2
```trạng thái sau`free 1`là:```
1 0 0 0 0 0 0 0 0 0 0
```Việc phân bổ bắt đầu ở chỉ số 1 vì kích thước 2 là bắt buộc. Không có khối cỡ 2 và cũng không có khối lớn hơn. Việc phân bổ không thành công nếu không sửa đổi bộ đếm cỡ 1:```
1 0 0 0 0 0 0 0 0 0 0
ERROR!
```Việc triển khai toàn bộ bộ nhớ có thể coi khối có kích thước 1 không chính xác là một phần của nhóm đủ, nhưng bộ cấp phát thực tế hoạt động với các khối riêng lẻ. 

### Khối lớn hơn được chọn sau khi khối nhỏ hơn bị lỗi 

cho```
3
free 1
free 8
allocate 2
```các bộ đếm trước khi phân bổ là:```
1 0 0 1 0 0 0 0 0 0 0
```Yêu cầu cho 2 bắt đầu ở chỉ mục 1. Chỉ mục 1 trống và chỉ mục 2 cũng trống, do đó tìm kiếm đạt đến chỉ mục 3 và chọn khối có kích thước 8. Phần còn lại là`8 - 2 = 6`, phân hủy thành 4 và 2. Việc thêm chúng vào khối size-1 hiện có sẽ mang lại:```
1 1 1 0 0 0 0 0 0 0 0
```Ví dụ này xác nhận cả hai phần của quy tắc phân bổ: khối được chọn phải là khối có sẵn nhỏ nhất có thể phù hợp với yêu cầu và phần còn lại của nó phải được phân tách độc lập. 

### Ranh giới khối tối đa 

cho```
2
free 1024
allocate 1023
```khối trống duy nhất ở chỉ số 10. Yêu cầu phân bổ cho 1023 có kích thước khối nhỏ nhất có thể là 1024, vì vậy chỉ mục 10 được chọn. Số còn lại là 1, số này tăng dần`cnt[0]`. Đầu ra là:```
0 0 0 0 0 0 0 0 0 0 1
1 0 0 0 0 0 0 0 0 0 0
```Tìm kiếm phải bao gồm chỉ mục 10. Việc sử dụng vòng lặp dừng trước chỉ mục lớn nhất sẽ báo cáo không chính xác việc phân bổ hợp lệ này là thất bại.

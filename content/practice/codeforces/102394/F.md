---
title: "CF 102394F - Sửa Banner"
description: "Chúng tôi có đúng sáu biểu ngữ cũ. Từ mỗi biểu ngữ, chúng ta phải chọn chính xác một ký tự, cung cấp cho chúng ta tổng cộng chính xác sáu ký tự. Chúng ta có thể sắp xếp lại sáu ký tự đó một cách tùy ý và từ cuối cùng phải là Cáp Nhĩ Tân."
date: "2026-08-10T19:07:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102394
codeforces_index: "F"
codeforces_contest_name: "The 2019 China Collegiate Programming Contest Harbin Site"
rating: 0
weight: 102394
solve_time_s: 81
verified: true
draft: false
---

[CF 102394F - Sửa biểu ngữ](https://codeforces.com/problemset/problem/102394/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có đúng sáu biểu ngữ cũ. Từ mỗi biểu ngữ, chúng ta phải chọn chính xác một ký tự, cung cấp cho chúng ta tổng cộng chính xác sáu ký tự. Chúng ta có thể sắp xếp lại sáu ký tự đó một cách tự do và từ cuối cùng phải là`harbin`. 

Hạn chế chính là hai ký tự không thể đến từ cùng một biểu ngữ, vì mỗi biểu ngữ đóng góp chính xác một ký tự. Kể từ khi các chữ cái của`harbin`đều khác nhau, nhiệm vụ tương đương với việc hỏi liệu sáu biểu ngữ có thể được so khớp một-một với sáu chữ cái bắt buộc để mỗi biểu ngữ đều chứa chữ cái được chỉ định hay không. 

Đối với mỗi trường hợp thử nghiệm, đầu vào bao gồm sáu chuỗi chữ thường không trống. Chúng ta cần in`Yes`nếu sự phân công một-một như vậy tồn tại và`No`nếu không thì. Dữ liệu đầu vào chứa tối đa 50.000 trường hợp thử nghiệm nhưng tổng số ký tự trên mỗi biểu ngữ nhiều nhất là (2\cdot10^6). Giới hạn tổng chiều dài đó gợi ý rõ ràng rằng việc quét mọi ký tự đầu vào với số lần không đổi là an toàn. Một thuật toán phụ thuộc đa thức vào độ dài biểu ngữ, chẳng hạn như thử nhiều kết hợp vị trí, sẽ quá tốn kém. 

Có hai trường hợp tinh vi có thể đánh lừa giải pháp dựa trên tần số. Đầu tiên, việc có đủ bản sao của mọi chữ cái bắt buộc trong các biểu ngữ kết hợp là không đủ vì một biểu ngữ không thể cung cấp hai chữ cái. Ví dụ,```
1
harbin
x
x
x
x
x
```có mọi chữ cái được yêu cầu ở đâu đó, nhưng câu trả lời là`No`. Biểu ngữ đầu tiên chứa tất cả sáu chữ cái hữu ích, trong khi năm biểu ngữ còn lại không chứa chúng, vì vậy sáu biểu ngữ khác nhau không thể cung cấp sáu chữ cái. 

Vấn đề thứ hai là một chữ cái bắt buộc có thể xuất hiện nhiều lần trong một banner, nhưng những bản sao đó vẫn chỉ đại diện cho một ký tự có thể sử dụng được vì chúng ta phải cắt chính xác một ký tự khỏi banner đó. Ví dụ,```
1
hhhhhh
aaaaaa
rrrrrr
bbbbbb
iiiiii
nnnnnn
```có nhiều bản sao của mọi chữ cái được yêu cầu, nhưng câu trả lời là`Yes`bởi vì mỗi chữ cái được yêu cầu đều có sẵn từ biểu ngữ riêng của nó. Ngược lại,```
1
hhhhhh
hhhhhh
aaaaaa
rrrrrr
bbbbbb
iiiiii
```không có biểu ngữ chứa`n`, vậy câu trả lời là`No`. 

Vấn đề xuất phát từ thực tế là tính khả dụng và sự phân công là những vấn đề khác nhau. Trước tiên, chúng tôi cần biết mỗi biểu ngữ có thể cung cấp những chữ cái bắt buộc nào, sau đó chúng tôi cần kiểm tra xem liệu những khả năng đó có thể được kết hợp mà không cần sử dụng biểu ngữ hai lần hay không. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp có thể chọn một vị trí từ mỗi biểu ngữ trong số sáu biểu ngữ. Nếu độ dài của chúng là (L_1,L_2,\ldots,L_6), thì có 

[ 
L_1L_2L_3L_4L_5L_6 
] 

sự lựa chọn khác nhau. Với mỗi lựa chọn, chúng ta có thể kiểm tra xem sáu ký tự đã chọn có thể được sắp xếp lại thành`harbin`. Điều này đúng vì mọi cách có thể để lấy một ký tự từ mỗi biểu ngữ đều được xem xét. 

Vấn đề là số lượng lựa chọn. Dưới giới hạn tổng chiều dài là (2\cdot10^6), tích sẽ lớn nhất khi sáu độ dài cân bằng nhất có thể, cụ thể là hai độ dài 333.334 và bốn độ dài 333.333. Điều đó đã mang lại 

[ 
333334^2\cdot333333^4 
] 

lựa chọn ứng viên, khoảng (1,37\cdot10^{33}). Ngay cả việc kiểm tra một ứng viên trong thời gian liên tục cũng là điều không thể. 

Lực lượng vũ phu hoạt động vì nó tôn trọng rõ ràng giới hạn một ký tự cho mỗi biểu ngữ, nhưng nó khám phá các vị trí ký tự thực tế mặc dù vị trí của chúng bên trong biểu ngữ không quan trọng. Đối với từ mục tiêu, biểu ngữ chỉ có liên quan thông qua chữ cái nào trong số sáu chữ cái`h`,`a`,`r`,`b`,`i`, Và`n`nó chứa đựng. 

Quan sát này làm giảm mỗi biểu ngữ thành mặt nạ sáu bit. Bit 0 có thể đại diện`h`, bit một`a`, vân vân. Nếu một biểu ngữ chứa`h`Và`r`, mặt nạ của nó ghi lại chính xác hai khả năng đó. Sau đó, chúng ta cần chọn một chữ cái mục tiêu riêng biệt từ mỗi trong số sáu mặt nạ. 

Vì chỉ có sáu chữ cái đích nên chúng ta có thể biểu diễn tập hợp các chữ cái đã được chọn bằng một mặt nạ sáu bit khác. Một quy trình lập trình động nhỏ xử lý từng biểu ngữ một. Đối với mỗi biểu ngữ, chúng tôi không sử dụng một chữ cái mục tiêu cụ thể từ biểu ngữ đó hoặc chọn một chữ cái hiện chưa được sử dụng có trong biểu ngữ. Sau khi tất cả sáu biểu ngữ đã được xử lý, mặt nạ đầy đủ có nghĩa là mọi chữ cái bắt buộc đã được gán cho một biểu ngữ khác. 

Không gian trạng thái chỉ chứa (2^6=64) mặt nạ, do đó phần gán rất nhỏ. Công việc duy nhất tùy thuộc vào kích thước đầu vào thực tế là quét các ký tự để tạo ra sáu mặt nạ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(L_1L_2L_3L_4L_5L_6)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(S+6^2 2^6)) | (O(2^6)) | Đã chấp nhận | 

Ở đây (S) là tổng chiều dài của sáu chuỗi trong một trường hợp thử nghiệm. Vì sáu và (2^6) là hằng số nên giải pháp tối ưu là tuyến tính một cách hiệu quả theo kích thước đầu vào. 

## Hướng dẫn thuật toán 

1. Xác định mục tiêu là`harbin`và gán một bit cho mỗi chữ cái trong số sáu chữ cái của nó. Ví dụ,`h`sử dụng bit 0,`a`sử dụng bit 1 và`n`sử dụng bit 5. Vì tất cả sáu chữ cái mục tiêu đều khác biệt nên mọi cấu trúc hợp lệ tương ứng với việc chọn từng bit chính xác một lần. 
2. Đọc sáu chuỗi biểu ngữ và tạo một mặt nạ cho mỗi biểu ngữ. Trong khi quét một chuỗi, nếu ký tự hiện tại của nó là một trong sáu chữ cái đích, hãy đặt bit tương ứng trong mặt nạ của biểu ngữ đó. Việc lặp đi lặp lại của cùng một ký tự không thành vấn đề vì biểu ngữ chỉ có thể đóng góp một ký tự. 
3. Tạo mảng lập trình động`dp`với 64 tiểu bang.`dp[mask]`có nghĩa là, sau khi xử lý một số tiền tố của biểu ngữ, có thể đã chọn chính xác các chữ cái đích được biểu thị bằng`mask`, sử dụng tối đa một ký tự từ mỗi biểu ngữ được xử lý. 
4. Thiết lập ban đầu`dp[0]`thành true vì trước khi xử lý bất kỳ biểu ngữ nào, không có chữ cái đích nào được chọn. 
5. Xử lý từng biểu ngữ một. Đối với mọi trạng thái có thể truy cập`mask`, kiểm tra từng chữ cái mục tiêu được biểu thị trong mặt nạ của biểu ngữ hiện tại. Nếu bit của chữ cái đó chưa có trong`mask`, tạo trạng thái mới`mask | bit`. Điều này thể hiện việc lấy chữ cái đó từ biểu ngữ hiện tại. 
6. Giữ khả năng không sử dụng chữ cái hữu ích của biểu ngữ cụ thể khi hình thành trạng thái trung gian. Điều này được thể hiện bằng cách đưa trạng thái hiện tại tiến về phía trước. Mặc dù giải pháp cuối cùng phải sử dụng chính xác một ký tự từ mỗi biểu ngữ nhưng biểu ngữ có thể đóng góp một ký tự không phải là một phần của`harbin`, và sự lựa chọn như vậy không ảnh hưởng gì đến việc gán chữ cái đích. Vì có chính xác sáu biểu ngữ và chính xác sáu chữ cái bắt buộc, nên bất kỳ trạng thái nào chứa tất cả sáu chữ cái mục tiêu sau khi xử lý tất cả các biểu ngữ nhất thiết phải sử dụng một chữ cái hữu ích riêng biệt từ mỗi biểu ngữ. 
7. Sau khi tất cả sáu biểu ngữ đã được xử lý, hãy kiểm tra xem trạng thái`(1 << 6) - 1`có thể truy cập được. Nếu có thì in`Yes`; nếu không thì in`No`. 

### Tại sao nó hoạt động 

Tính bất biến đó là`dp[mask]`đúng chính xác khi các biểu ngữ được xử lý có thể cung cấp các chữ cái mục tiêu riêng biệt được biểu thị bằng`mask`, không có biểu ngữ nào đóng góp nhiều hơn một trong số chúng. Khi quá trình chuyển đổi thêm một bit, bit đó không được chọn trước đó, do đó không có chữ cái bắt buộc nào được sử dụng hai lần và quá trình chuyển đổi sẽ lấy chữ cái mới từ biểu ngữ hiện tại, do đó không có biểu ngữ nào đóng góp hai lần. Ngược lại, bất kỳ phép gán hợp lệ nào cũng có thể được theo sau bởi từng biểu ngữ: bất cứ khi nào chữ cái được gán của nó thuộc về`harbin`, quá trình chuyển đổi tương ứng có sẵn. Do đó, mặt nạ sáu bit đầy đủ có thể truy cập được chính xác khi sáu biểu ngữ có thể cung cấp tất cả sáu chữ cái bắt buộc một-một. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

TARGET = "harbin"
BIT = {ch: 1 << i for i, ch in enumerate(TARGET)}
FULL = (1 << 6) - 1

def solve_case():
    masks = []

    for _ in range(6):
        s = input().strip()
        mask = 0

        for ch in s:
            bit = BIT.get(ch)
            if bit is not None:
                mask |= bit

        masks.append(mask)

    dp = [False] * (1 << 6)
    dp[0] = True

    for available in masks:
        ndp = dp[:]

        for mask in range(1 << 6):
            if not dp[mask]:
                continue

            choices = available & ~mask

            while choices:
                bit = choices & -choices
                choices -= bit
                ndp[mask | bit] = True

        dp = ndp

    return "Yes" if dp[FULL] else "No"

def main():
    t = int(input())
    answer = []

    for _ in range(t):
        answer.append(solve_case())

    sys.stdout.write("\n".join(answer))

if __name__ == "__main__":
    main()
```các`TARGET`chuỗi sửa sự tương ứng giữa mỗi ký tự được yêu cầu và một bit. Việc tra cứu từ điển biến một ký tự đích thành bit của nó trong thời gian không đổi, trong khi các ký tự bên ngoài`harbin`chỉ đơn giản là có thể được bỏ qua. 

Đối với mỗi biểu ngữ,`mask`chỉ ghi lại liệu ký tự bắt buộc có xuất hiện ít nhất một lần hay không. Điều này là đủ vì lấy cái đầu tiên`h`hoặc cuối cùng`h`từ cùng một biểu ngữ không có gì khác biệt. các`get`call tránh được bài kiểm tra tư cách thành viên riêng biệt và cũng cung cấp cho chúng tôi một cách thuận tiện để bỏ qua các chữ cái viết thường không liên quan. 

Mảng lập trình động có 64 mục nhập vì có sáu lựa chọn có hoặc không độc lập về việc mỗi chữ cái đích đã được cung cấp hay chưa.`ndp = dp[:]`duy trì các trạng thái hiện có, tương ứng với việc xử lý biểu ngữ hiện tại mà không gán một trong các chữ cái mục tiêu của nó cho cấu trúc được theo dõi. 

biểu thức`available & ~mask`trích xuất các chữ cái mục tiêu có sẵn trong biểu ngữ hiện tại chưa được chọn. Hoạt động bit thấp`choices & -choices`chọn một chữ cái như vậy tại một thời điểm. Đây là kỹ thuật mặt nạ bit tiêu chuẩn giúp tránh lặp lại tất cả 64 mặt nạ cho mọi ký tự có thể. 

Không có chỉ mục nào trên các vị trí biểu ngữ sau khi mặt nạ đã được tạo nên không có vấn đề về ranh giới vị trí ký tự. Số nguyên Python có độ chính xác tùy ý, mặc dù giá trị lớn nhất được sử dụng ở đây chỉ là 63. Đầu vào được xử lý từng dòng và đầu ra được tích lũy và ghi một lần, phù hợp với tối đa 50.000 trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Đối với trường hợp mẫu đầu tiên, sáu biểu ngữ là`welcome`,`toparticipate`,`inthe`,`ccpccontest`,`inharbin`, Và`inoctober`. Mặt nạ chữ cái mục tiêu có liên quan của họ được hiển thị bên dưới. 

| Biểu ngữ | Các chữ cái mục tiêu có sẵn | Trạng thái mặt nạ | 
| --- | --- | --- | 
|`welcome`|`e`| 0 | 
|`toparticipate`|`a`,`i`| 10 | 
|`inthe`|`i`,`n`| 40 | 
|`ccpccontest`|`n`| 32 | 
|`inharbin`|`h`,`a`,`r`,`b`,`i`,`n`| 63 | 
|`inoctober`|`i`,`n`,`b`,`r`| 44 | 

Biểu ngữ đầu tiên không thể cung cấp bất kỳ ký tự nào từ`harbin`, vì vậy không có trạng thái hữu ích nào được tạo ở đó. Biểu ngữ thứ hai và thứ ba có thể cung cấp một số chữ cái được yêu cầu và các biểu ngữ sau cung cấp các lựa chọn bổ sung. Tuy nhiên,`h`chỉ xảy ra ở`inharbin`, do đó biểu ngữ đó phải cung cấp`h`. Khi đã được chọn, năm biểu ngữ còn lại vẫn không thể cung cấp tất cả`a`,`r`,`b`,`i`, Và`n`mà không cần sử dụng lại biểu ngữ hoặc thiếu chữ cái bắt buộc. Mặt nạ đầy đủ là không thể truy cập được. 

Do đó, trạng thái cuối cùng là sai, đưa ra`No`. 

Đối với trường hợp mẫu thứ hai, sáu biểu ngữ là`harvest`,`belong`,`ninja`,`reset`,`amazing`, Và`intriguing`. 

| Biểu ngữ | Những lá thư hữu ích | Mặt nạ | 
| --- | --- | --- | 
|`harvest`|`h`,`a`,`r`| 7 | 
|`belong`|`b`,`n`| 33 | 
|`ninja`|`i`,`n`,`a`| 42 | 
|`reset`|`r`| 4 | 
|`amazing`|`a`,`i`,`n`| 50 | 
|`intriguing`|`i`,`n`,`r`| 44 | 

Một nhiệm vụ hợp lệ là thực hiện`h`từ`harvest`,`b`từ`belong`,`i`từ`ninja`,`r`từ`reset`,`a`từ`amazing`, Và`n`từ`intriguing`. Mỗi chữ cái xuất phát từ một biểu ngữ khác nhau, do đó, mặt nạ sáu bit đầy đủ sẽ có thể truy cập được. 

Thuật toán cuối cùng đạt đến mặt nạ 63, vì vậy trường hợp này tạo ra`Yes`. Dấu vết cũng chứng minh tại sao một biểu ngữ chứa nhiều chữ cái hữu ích lại không gây ra vấn đề gì. DP chọn nhiều nhất một trong số chúng cho biểu ngữ đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(S+6^2 2^6)) | (S) ký tự được quét để tạo mặt nạ, theo sau là DP | có kích thước không đổi 
| Không gian | (O(2^6)) | DP chỉ lưu trữ 64 trạng thái boolean và sáu mặt nạ biểu ngữ | 

Trong tất cả các trường hợp thử nghiệm, tổng độ dài đầu vào tối đa là (2\cdot10^6), do đó phần quét ký tự chỉ thực hiện công việc tuyến tính trong giới hạn đó. DP còn lại có chi phí cố định tối đa là vài nghìn hoạt động cho mỗi trường hợp, có thể dễ dàng quản lý ngay cả đối với 50.000 trường hợp. Do đó, giải pháp phù hợp thoải mái trong giới hạn thời gian 1 giây đã nêu và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

TARGET = "harbin"
BIT = {ch: 1 << i for i, ch in enumerate(TARGET)}
FULL = (1 << 6) - 1

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    try:
        input = sys.stdin.readline
        t = int(input())
        ans = []

        for _ in range(t):
            masks = []

            for _ in range(6):
                s = input().strip()
                mask = 0

                for ch in s:
                    bit = BIT.get(ch)
                    if bit is not None:
                        mask |= bit

                masks.append(mask)

            dp = [False] * 64
            dp[0] = True

            for available in masks:
                ndp = dp[:]

                for mask in range(64):
                    if not dp[mask]:
                        continue

                    choices = available & ~mask

                    while choices:
                        bit = choices & -choices
                        choices -= bit
                        ndp[mask | bit] = True

                dp = ndp

            ans.append("Yes" if dp[FULL] else "No")

        return "\n".join(ans) + "\n"

    finally:
        sys.stdin = old_stdin

# Provided sample
sample = """\
2
welcome
toparticipate
inthe
ccpccontest
inharbin
inoctober
harvest
belong
ninja
reset
amazing
intriguing
"""
assert solve_data(sample) == "No\nYes\n", "provided sample"

# Minimum-size case: every banner contains exactly one required letter.
minimum = """\
1
h
a
r
b
i
n
"""
assert solve_data(minimum) == "Yes\n", "minimum-size valid case"

# All-equal values: no banner can provide six distinct target letters.
all_equal = """\
1
aaaa
aaaa
aaaa
aaaa
aaaa
aaaa
"""
assert solve_data(all_equal) == "No\n", "all-equal case"

# Several required letters are concentrated in one banner.
# Aggregate frequency is sufficient to fool a careless solution,
# but one banner can contribute only one character.
concentrated = """\
1
harbin
x
x
x
x
x
"""
assert solve_data(concentrated) == "No\n", "one-banner concentration"

# Every required letter exists, but two required letters are forced
# into the same banner, while another banner has no useful letter.
forced_conflict = """\
1
har
b
i
n
x
x
"""
assert solve_data(forced_conflict) == "No\n", "forced assignment conflict"

# Maximum total input length: exactly 2,000,000 characters.
# Each of the six banners contains its required letter once,
# so the answer is Yes.
lengths = [333334, 333334, 333333, 333333, 333333, 333333]
letters = "harbin"
large_lines = [
    letters[i] + "x" * (lengths[i] - 1)
    for i in range(6)
]
maximum = "1\n" + "\n".join(large_lines) + "\n"
assert sum(map(len, large_lines)) == 2_000_000
assert solve_data(maximum) == "Yes\n", "maximum-size case"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`h`,`a`,`r`,`b`,`i`,`n`|`Yes`| Đầu vào hợp lệ có kích thước tối thiểu và phân công chính xác từng người một | 
| Sáu bản sao của`aaaa`|`No`| Các giá trị hoàn toàn bằng nhau và thiếu các chữ cái bắt buộc | 
|`harbin`theo sau là năm`x`dây |`No`| Ngăn chặn việc xử lý tần số ký tự kết hợp là đủ | 
|`har`,`b`,`i`,`n`,`x`,`x`|`No`| Phát hiện xung đột gán bắt buộc giữa các biểu ngữ | 
| Sáu chuỗi có tổng cộng chính xác 2.000.000 ký tự |`Yes`| Tổng kích thước đầu vào tối đa và quét tuyến tính | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là sự tập trung của tất cả các chữ cái hữu ích trong một biểu ngữ. Vì```
1
harbin
x
x
x
x
x
```biểu ngữ đầu tiên có mặt nạ 63 và mọi biểu ngữ khác có mặt nạ 0. DP có thể chuyển từ mặt nạ 0 sang bất kỳ trạng thái một bit nào trong khi xử lý biểu ngữ đầu tiên, nhưng không biểu ngữ nào sau này có thể thêm một bit khác. Mask 63 không thể truy cập được nên kết quả là`No`. Đây chính xác là tình huống mà chỉ cần đếm số lần xuất hiện trên tất cả sáu chuỗi sẽ đưa ra câu trả lời sai. 

Trường hợp cạnh thứ hai là các bản sao lặp lại của một ký tự bên trong một biểu ngữ. Coi như```
1
hhhh
aaaa
rrrr
bbbb
iiii
nnnn
```Mặt nạ của mỗi biểu ngữ chỉ chứa một bit, bất kể số lượng bản sao. DP chọn một bit khác nhau từ mỗi biểu ngữ và chạm tới mặt nạ đầy đủ. Kết quả là`Yes`. Các ký tự lặp lại không thể được tính là nhiều tài nguyên có sẵn vì chỉ có thể cắt một ký tự khỏi mỗi biểu ngữ. 

Trường hợp cạnh thứ ba là ký tự bắt buộc bị thiếu. Vì```
1
har
b
i
n
x
x
```những chiếc mặt nạ là`har`,`b`,`i`,`n`, trống rỗng và trống rỗng. DP có thể đạt tối đa sự kết hợp bốn bit`h`,`a`,`r`,`b`,`i`,`n`chỉ khi những chữ cái đó đều có thể được cung cấp bởi các biểu ngữ riêng biệt, nhưng`har`chứa ba chữ cái bắt buộc trong khi nó chỉ có thể đóng góp một. Hai biểu ngữ trống không thể đóng góp bất cứ điều gì. Không thể truy cập được mặt nạ đầy đủ nên câu trả lời là`No`. 

Trường hợp kích thước tối đa kiểm tra một ranh giới khác. Sáu chuỗi có thể chứa tổng cộng chính xác 2.000.000 ký tự, ví dụ: với độ dài 333.334, 333.334, 333.333, 333.333, 333.333 và 333.333. Nếu mỗi chuỗi chứa một lần chữ cái bắt buộc tương ứng và phần còn lại bao gồm các ký tự không liên quan thì sáu mặt nạ đều là các mặt nạ một bit khác nhau và câu trả lời là`Yes`. Thuật toán quét tất cả 2.000.000 ký tự một lần và sau đó chỉ thực hiện DP có kích thước cố định, do đó kích thước đầu vào lớn không làm thay đổi hành vi tiệm cận.

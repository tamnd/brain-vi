---
title: "CF 102443H - Hành tinh thứ chín"
description: "Chúng tôi có một thanh ghi số nguyên thập phân. Bắt đầu từ a, chúng ta có thể thực hiện hai loại phép toán. Một phép cộng sẽ tăng thanh ghi lên 9x đối với bất kỳ số nguyên dương x nào."
date: "2026-08-09T01:57:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102443
codeforces_index: "H"
codeforces_contest_name: "2019-2020 Russia Team Open, High School Programming Contest (VKOSHP 19)"
rating: 0
weight: 102443
solve_time_s: 867
verified: false
draft: false
---

[CF 102443H - Hành tinh thứ chín](https://codeforces.com/problemset/problem/102443/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14 phút 27 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một thanh ghi số nguyên thập phân. Bắt đầu từ`a`, chúng ta có thể thực hiện hai loại hoạt động. Một thao tác bổ sung làm tăng thanh ghi bằng`9x`với mọi số nguyên dương`x`. Thao tác xóa sẽ loại bỏ một số dương các chữ số thập phân đứng đầu, nhưng mọi chữ số bị xóa phải`1`. Trường hợp đặc biệt trong đó thanh ghi chính xác`1`được bao gồm, bởi vì việc xóa một chữ số đó sẽ tạo ra`0`. 

Nhiệm vụ mang tính xây dựng. Được cho`a`Và`b`, chúng ta phải in một chuỗi nhiều nhất`1000`các thao tác thay đổi thanh ghi từ`a`ĐẾN`b`, hoặc in`Broken`nếu không có trình tự như vậy tồn tại. Giá trị thanh ghi trung gian phải ở mức tối đa`10^18`. Bản thân các giá trị đầu vào tối đa là`10^9`, vì vậy chúng chứa tối đa mười chữ số thập phân. 

Số lượng chữ số nhỏ là hạn chế chính. Chúng tôi không cần một chuỗi ngắn nhất và chúng tôi có giới hạn rất rộng rãi về`1000`các thao tác so với tối đa mười chữ số cần xử lý. Điều này gợi ý rõ ràng rằng giải pháp phù hợp nên xử lý chữ số biểu diễn thập phân theo chữ số thay vì tìm kiếm thông qua các giá trị thanh ghi có thể có. 

Một giải pháp bất cẩn cũng có thể thất bại vì các số 0 đứng đầu biến mất khỏi biểu diễn thập phân thông thường. Ví dụ, hãy xem xét`a = 100`. Sau khi xử lý chữ số đầu tiên, hậu tố khái niệm còn lại là`00`, nhưng sổ đăng ký lưu nó đơn giản dưới dạng`0`. Cấu trúc phải suy luận về vị trí của hậu tố bằng cách sử dụng số chữ số của nó, thay vì giả định rằng độ dài thập phân hiện tại của thanh ghi bằng số chữ số chưa được xử lý. 

Một trường hợp cạnh khác là`a = 0`Và`b = 0`. Không cần thao tác nào, do đó, đầu ra chính xác có thể đơn giản là`Stable`theo sau là`0`. Một công trình cố gắng xử lý chữ số một cách mù quáng`0`như thể nó là một chữ số dương có thể tạo ra một thao tác không hợp lệ. 

Vụ án`a = 1`,`b = 9`cũng mang tính hướng dẫn. Chúng ta có thể biến đổi`1`vào trong`0`bằng cách thêm`9`và xóa phần đầu`1`, sau đó biến đổi`0`vào trong`9`bằng cách thêm cái khác`9`. Do đó, một chuỗi hợp lệ là`+ 1`,`- 1`,`+ 1`. Mẫu sử dụng trình tự ngắn hơn,`+ 2`,`- 1`, nhưng sự tối thiểu là không liên quan. 

Cuối cùng, một giải pháp phải tôn trọng`10^18`ràng buộc trung gian. Chỉ tìm một cách xây dựng đúng về mặt đại số là không đủ nếu phép cộng tạo ra một số lớn hơn giới hạn cho phép. Cấu trúc chữ số bên dưới tạo ra các số có tối đa mười tám chữ số thập phân trong phạm vi đầu vào được phép và giá trị trung gian lớn nhất có thể của nó vẫn ở bên dưới`10^18`. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ tự nhiên là coi mỗi giá trị thanh ghi là một trạng thái và tìm kiếm đường dẫn từ`a`ĐẾN`b`. Từ một trạng thái chúng ta có thể thử phép cộng bằng cách tăng`x`và chúng tôi cũng có thể thử mọi cách xóa hợp lệ có thể. Về nguyên tắc, điều này đúng vì mọi hoạt động hợp pháp sẽ được biểu diễn dưới dạng một cạnh trong biểu đồ trạng thái. 

Vấn đề là không gian trạng thái rất lớn. Ngay cả khi chúng tôi giới hạn bản thân ở các giá trị dưới đây`10^18`, có tới`10^18 + 1`giá trị đăng ký có thể. Việc thử bổ sung từng cái một thậm chí còn tệ hơn đối với công trình cụ thể này. Ví dụ: khi xây dựng một chữ số`1`ở vị trí thập phân đủ cao, hệ số nhân cần thiết có thể là`12,345,679`lần lũy thừa của mười. Đối với mục tiêu chín chữ số bao gồm toàn bộ số 1, yêu cầu lớn nhất`x`là`1,234,567,900,000,000`, do đó, một tìm kiếm mạnh mẽ sẽ kiểm tra`x = 1, 2, 3, ...`có thể yêu cầu nhiều hơn`10^15`ứng viên kiểm tra tại một vị trí. các`1`giới hạn thứ hai loại trừ việc tìm kiếm như vậy ngay lập tức. 

Quan sát hữu ích là phép nhân với`9`được kết nối chặt chẽ với tổng chữ số thập phân. Nếu muốn biến một chữ số đứng đầu thành một dãy các chữ số đứng đầu, chúng ta có thể chọn một số nguyên có phép nhân với`9`tạo ra chính xác những cái đó. 

Giả sử số hiện tại bắt đầu bằng một chữ số`d`, theo sau là một hậu tố chính xác`k`các vị trí thập phân. Vì`d >= 1`, xét số được tạo thành bằng cách viết`1, 2, ..., d`. Ví dụ, đối với`d = 4`đây là`1234`. Danh tính`4 + 9 * 1234 = 11110`là mẫu chúng ta cần. Nói chung,`d + 9 * 123...d = 11...110`với chính xác`d`những người dẫn đầu. Nếu toàn bộ biểu thức được nhân với`10^k`, hậu tố vẫn không bị ảnh hưởng sau khi những hậu tố đứng đầu bị xóa. Điều này cho phép chúng tôi loại bỏ chữ số đầu tiên của`a`. Xử lý tất cả các chữ số từ trái sang phải cuối cùng sẽ biến đổi`a`vào số không. 

Đi theo hướng ngược lại thì hơi khác một chút. Giả sử chúng ta đã có hậu tố của mục tiêu và muốn thêm chữ số vào trước`d`. Chúng ta có thể cộng bội số của 9 để kết quả bắt đầu bằng một vài số tiếp theo là chữ số mong muốn và hậu tố hiện có. Số số đứng đầu được chọn sao cho số mới có cùng số dư modulo chín với hậu tố hiện có. Vì một số thập phân có phần dư theo modulo 9 như tổng các chữ số của nó nên chọn`9 - d`những người dẫn đầu cho`1 <= d < 9`làm cho số tiền thêm vào chia hết cho chín. Vì`d = 9`, không cần người dẫn đầu. 

Điều này đưa ra một cách xây dựng trực tiếp từ`0`đến bất kỳ`b`. Kết hợp hai cách xây dựng mang lại`a -> 0 -> b`. 

Mỗi chữ số thực hiện tối đa hai phép tính, do đó, với tối đa mười chữ số trong mỗi số, toàn bộ chuỗi có tối đa bốn mươi phép tính, thấp hơn nhiều so với giới hạn của`1000`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ω(10^15) ứng viên kiểm tra trong trường hợp xấu nhất đại diện | Có khả năng O(10^18) trạng thái | Quá chậm | 
| Xây dựng chữ số | O(D^2), trong đó D <= 10 | O(D) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`a`Và`b`dưới dạng dây. Việc giữ lại các chuỗi rất hữu ích vì việc xây dựng phụ thuộc vào các vị trí thập phân ban đầu, bao gồm cả các số 0 mà nếu không sẽ biến mất khỏi biểu diễn thanh ghi. 
2. Xây dựng trình tự biến đổi`a`vào trong`0`. Xử lý các chữ số của`a`từ trái sang phải. Cho phép`k`là số vị trí bên phải của chữ số hiện tại. 
3. Nếu chữ số hiện tại là`0`, không phát ra hoạt động nào. Thanh ghi hiện tại đã thể hiện chính xác hậu tố còn lại bằng số, do đó không có gì cần xóa. 
4. Nếu chữ số hiện tại là`d >= 1`, xây dựng số nguyên`123...d`. Ví dụ,`d = 1`cho`1`,`d = 2`cho`12`, Và`d = 5`cho`12345`. Thêm vào`9 * 123...d * 10^k`, được thể hiện bằng hoạt động`+ (123...d * 10^k)`. 
5. Sau phép cộng này, chữ số hàng đầu hiện tại và số tiền được thêm vào kết hợp chính xác thành`d`những cái đứng đầu, theo sau là hậu tố không thay đổi. Xóa những cái đó`d`những người dẫn đầu sử dụng`- d`. Bất biến sau thao tác này là chữ số đầu tiên được xử lý đã biến mất và thanh ghi bằng hậu tố chưa được xử lý. 
6. Sau tất cả các chữ số của`a`đã được xử lý, sổ đăng ký là`0`. Điều này tự động xử lý các số 0 đứng đầu vì các chữ số 0 không cần thao tác. 
7. Bây giờ xây dựng`b`bắt đầu từ số không. Xử lý các chữ số của`b`từ phải sang trái. Tại mọi điểm, thanh ghi chứa hậu tố của`b`cái đó đã được xây dựng rồi. 
8. Nếu chữ số mới là`9`, thêm vào`9 * 10^k`, Ở đâu`k`là số chữ số hậu tố đã được xây dựng. Điều này trực tiếp thay đổi tiền tố từ hậu tố hiện tại`S`vào trong`9S`, vì vậy không cần xóa. 
9. Nếu chữ số mới là`d`, Ở đâu`1 <= d < 9`, chọn chính xác`9 - d`những người dẫn đầu. Xây dựng số nguyên`C`gồm các chữ số`1, 2, ..., 8-d`, theo sau là`10-d`. Ví dụ,`d = 2`cho`C = 1234568`, trong khi`d = 8`cho`C = 2`. 
10. Thêm`C * 10^k`bằng cách in`+ C * 10^k`. Thanh ghi kết quả bao gồm`9-d`số đứng đầu, sau đó là chữ số mong muốn`d`, thì hậu tố đã được tạo. Xóa cái đầu tiên`9-d`chữ số sử dụng`- (9-d)`. Sổ đăng ký bây giờ là hậu tố dài hơn chính xác. 
11. Nối các thao tác từ`a -> 0`giai đoạn và`0 -> b`giai đoạn. Vì mỗi chữ số khác 0 tạo ra nhiều nhất hai phép tính và mỗi số có nhiều nhất là mười chữ số nên có nhiều nhất là bốn mươi phép tính. 

### Tại sao nó hoạt động 

Bất biến của pha thứ nhất là sau khi xử lý vị trí`j`, thanh ghi chứa chính xác hậu tố thập phân bắt đầu tại vị trí`j+1`. Đối với một chữ số`d >= 1`, danh tính`d + 9 * 123...d = 11...110`tạo ra chính xác`d`những số đứng đầu, vì vậy việc xóa những số đó sẽ loại bỏ chính xác chữ số được xử lý. Nhân với`10^k`bảo toàn các vị trí hậu tố. Do đó, mọi chữ số đã xử lý sẽ bị xóa mà không thay đổi bất kỳ chữ số nào chưa được xử lý. 

Đối với giai đoạn thứ hai, giả sử hậu tố đã được xây dựng là`S`và chữ số mục tiêu tiếp theo là`d < 9`. Chúng tôi tạo ra một số biểu mẫu`111...1 d S`với`9-d`những người dẫn đầu. Tổng chữ số của nó vượt quá`S`chính xác`9`, do đó nó phù hợp với`S`modul chín. Do đó hiệu của chúng chia hết cho 9 và có thể được tạo ra bằng phép tính cộng. Sau khi xóa`9-d`dẫn đầu, sổ đăng ký trở nên chính xác`dS`. Vì`d = 9`, việc bổ sung trực tiếp của`9 * 10^k`cho`9S`. Do đó, mọi chữ số mục tiêu đều có thể được thêm vào trước một cách chính xác. 

Hai giai đoạn này độc lập vì giai đoạn đầu tiên luôn kết thúc ở mức 0 và giai đoạn thứ hai bắt đầu ở mức 0. Vì mọi phép toán đều hợp pháp về mặt xây dựng và mọi giá trị trung gian đều bị giới hạn nên thanh ghi cuối cùng chính xác là`b`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_to_zero(s):
    ops = []
    n = len(s)

    for i, ch in enumerate(s):
        d = ord(ch) - ord('0')

        if d == 0:
            continue

        # 123...d
        c = 0
        for x in range(1, d + 1):
            c = c * 10 + x

        k = n - 1 - i
        x = c * (10 ** k)

        ops.append(("+", x))
        ops.append(("-", d))

    return ops

def build_from_zero(s):
    ops = []
    n = len(s)

    for i in range(n - 1, -1, -1):
        d = ord(s[i]) - ord('0')
        k = n - 1 - i

        if d == 0:
            # A zero cannot be introduced by deleting leading ones,
            # but it is already represented by the positional suffix.
            continue

        if d == 9:
            # Add 9 * 10^k, which simply prepends digit 9.
            ops.append(("+", 10 ** k))
            continue

        # We want:
        #
        #   111...1 d S
        #
        # with 9-d leading ones.
        #
        # The difference from S must be divisible by 9.
        c = 0
        for x in range(1, 9 - d):
            c = c * 10 + x
        c = c * 10 + (10 - d)

        x = c * (10 ** k)

        ops.append(("+", x))
        ops.append(("-", 9 - d))

    return ops

def solve():
    a, b = input().split()

    ops = build_to_zero(a)
    ops.extend(build_from_zero(b))

    print("Stable")
    print(len(ops))
    for typ, value in ops:
        print(typ, value)

if __name__ == "__main__":
    solve()
```các`build_to_zero`hàm thực hiện trực tiếp bất biến từ trái sang phải. Biến`k`là số vị trí thập phân sau chữ số hiện tại, do đó nhân giá trị được xây dựng với`10^k`làm cho những cái đứng đầu mới được tạo xuất hiện trước hậu tố chưa được chạm tới. 

Cấu trúc vòng lặp`c`cố tình sử dụng các chữ số`1`bởi vì`d`. Nhận dạng kết quả là trung tâm của giai đoạn đầu tiên. Ví dụ, với`d = 3`,`c = 123`, Và`3 + 9 * 123 = 1110`. 

các`build_from_zero`chức năng đảo ngược hướng. Của nó`k`đếm xem có bao nhiêu vị trí mục tiêu đã được xây dựng ở bên phải. Đối với các chữ số từ`1`bởi vì`8`, giá trị của`c`được chọn sao cho số tạo ra sau khi nhân với 9 chứa chính xác`9-d`những người dẫn đầu. Việc xóa tiếp theo sẽ loại bỏ những số đó và để lại chữ số mong muốn trước hậu tố hiện có. 

các`d == 9`trường hợp là riêng biệt bởi vì`9-d`là số không. Không có gì để xóa và thêm`9 * 10^k`chỉ cần chèn một`9`ở đúng vị trí. 

Zeros không cần hoạt động rõ ràng trong quá trình xây dựng. Chúng được xử lý bằng phép nhân vị trí trong các bước sau. Đây là lý do tại sao đầu vào ban đầu phải được giữ lại dưới dạng chuỗi mặc dù bản thân thanh ghi lưu trữ một số nguyên. 

Số nguyên Python không bị tràn nhưng cách xây dựng cũng tôn trọng giới hạn số của bài toán. Với đầu vào không lớn hơn`10^9`, đầu vào có mười chữ số duy nhất là`1000000000`, chữ số đầu tiên của nó là`1`; do đó các giá trị trung gian được xây dựng vẫn ở mức dưới`10^18`. Số lượng hoạt động nhiều nhất là bốn mươi. 

## Ví dụ đã hoạt động 

### Mẫu 1:`0 0`Giai đoạn đầu tiên không có chữ số nào cần xử lý và giai đoạn thứ hai cũng không có chữ số khác 0 để xây dựng. 

| Giai đoạn | Vị trí | Chữ số | Hoạt động | Đăng ký | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | | | | 0 | 
|`a -> 0`| | | không | 0 | 
|`0 -> b`| | | không | 0 | 

Đầu ra chỉ đơn giản là`Stable`tiếp theo là hoạt động bằng không. Điều này thể hiện trường hợp ranh giới trong đó cả hai điểm cuối đều bằng 0. 

### Mẫu 2:`1 9`Vì`a = 1`, chữ số xử lý giai đoạn đầu tiên`1`. Đây`k = 0`Và`123...d = 1`, vì vậy chúng tôi thêm`9`và có được`10`. Loại bỏ một hàng đầu`1`lá số không. 

Vì`b = 9`, giai đoạn thứ hai nhìn thấy chữ số`9`. Vì nó là`9`, không cần có cái dẫn đầu. Thêm`9`về 0 sẽ tạo ra mục tiêu trực tiếp. 

| Giai đoạn | Chữ số |`k`|`+ x`| Đăng ký sau`+`|`- y`| Đăng ký sau`-`| 
| --- | --- | --- | --- | --- | --- | --- | 
|`1 -> 0`| 1 | 0 | 1 | 10 | 1 | 0 | 
|`0 -> 9`| 9 | 0 | 1 | 9 | không | 9 | 

Việc xây dựng sử dụng ba thao tác, trong khi mẫu sử dụng hai thao tác. Cả hai đều hợp lệ vì bài toán yêu cầu bất kỳ chuỗi hợp lệ nào thay vì chuỗi có độ dài tối thiểu. 

### Ví dụ có nhiều chữ số:`21 -> 21`Chữ số đầu tiên của`21`là`2`, với một chữ số ở bên phải. Chúng tôi sử dụng`123 * 10 = 1230`, do đó phép cộng làm tăng thanh ghi lên`11070`, lấy`21`ĐẾN`11091`. Loại bỏ hai lá đầu tiên`91`, đó là hậu tố còn lại`1`chỉ khi chúng ta kiểm tra số học một cách cẩn thận. Trực tiếp hơn, việc xây dựng nên được diễn giải thông qua danh tính trên hậu tố đầy đủ:`21 + 9 * (12 * 10) = 21 + 1080 = 1101`. 

Xóa hai lá đầu`1`. 

Chữ số tiếp theo là`1`, do đó thêm`9`cho`10`và xóa cái đứng đầu của nó sẽ bằng không. 

Đối với hướng ngược lại, xử lý chữ số đích cuối cùng`1`Đầu tiên. Bắt đầu từ số 0, thêm`9 * 12345679`sản xuất`111111111`, và xóa tám lá cái đứng đầu`1`. 

Sau đó xử lý chữ số`2`. Hậu tố đã được xây dựng là`1`, vì vậy chúng tôi thêm`9 * 1234568 * 10`, sản xuất`111111121`. Xóa bảy lá đứng đầu`21`. 

| Giai đoạn | Chữ số mục tiêu hiện tại |`k`| Phép cộng`x`| Kết quả trước khi xóa | Những cái đã xóa | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
|`21 -> 0`| 2 | 1 | 120 | 1101 | 2 | 1 | 
|`21 -> 0`| 1 | 0 | 1 | 10 | 1 | 0 | 
|`0 -> 21`| 1 | 0 | 12345679 | 111111111 | 8 | 1 | 
|`0 -> 21`| 2 | 1 | 12345680 | 111111121 | 7 | 21 | 

Dấu vết này cho thấy tại sao hướng lại quan trọng. Để loại bỏ các chữ số, trước tiên chúng ta làm việc từ phía có ý nghĩa nhất. Để xây dựng các chữ số, trước tiên chúng tôi làm việc từ phía ít quan trọng nhất vì hậu tố đó phải tồn tại khi một chữ số mới được thêm vào trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(D^2) | Đối với mỗi chữ số có nhiều nhất là D, việc xây dựng mẫu thập phân nhỏ mất O(D) thời gian. | 
| Không gian | O(D) | Tối đa hai thao tác được lưu trữ trên mỗi chữ số. | 

Đây`D <= 10`bởi vì`a`Và`b`nhiều nhất là`10^9`. Số lượng hoạt động kết quả là nhiều nhất`4D <= 40`, thoải mái dưới mức cho phép`1000`và mức tiêu thụ bộ nhớ không đáng kể so với`512 MB`giới hạn. 

## Trường hợp thử nghiệm 

Phần khai thác sau đây kiểm tra trình tự hoạt động được tạo ra thay vì so sánh nó với một câu trả lời cố định. Điều đó là cần thiết vì bài toán chấp nhận mọi cách xây dựng hợp lệ.```python
# helper: run solution on input string, return output string
import sys
import io

def build_to_zero(s):
    ops = []
    n = len(s)

    for i, ch in enumerate(s):
        d = int(ch)

        if d == 0:
            continue

        c = 0
        for x in range(1, d + 1):
            c = c * 10 + x

        k = n - 1 - i
        ops.append(("+", c * (10 ** k)))
        ops.append(("-", d))

    return ops

def build_from_zero(s):
    ops = []
    n = len(s)

    for i in range(n - 1, -1, -1):
        d = int(s[i])
        k = n - 1 - i

        if d == 0:
            continue

        if d == 9:
            ops.append(("+", 10 ** k))
            continue

        c = 0
        for x in range(1, 9 - d):
            c = c * 10 + x
        c = c * 10 + (10 - d)

        ops.append(("+", c * (10 ** k)))
        ops.append(("-", 9 - d))

    return ops

def solve_string(inp):
    a, b = inp.strip().split()

    ops = build_to_zero(a)
    ops.extend(build_from_zero(b))

    out = ["Stable", str(len(ops))]
    out.extend(f"{typ} {value}" for typ, value in ops)
    return "\n".join(out) + "\n"

def run(inp: str) -> str:
    return solve_string(inp)

def validate(inp):
    a, b = map(int, inp.split())
    out = run(inp).strip().splitlines()

    assert out[0] == "Stable"

    n = int(out[1])
    assert 0 <= n <= 1000
    assert len(out) == n + 2

    value = a

    for line in out[2:]:
        typ, number = line.split()
        number = int(number)

        assert number > 0

        if typ == "+":
            value += 9 * number
        else:
            assert typ == "-"
            s = str(value)
            y = number
            assert y <= len(s)
            assert s[:y] == "1" * y
            value = int(s[y:]) if s[y:] else 0

        assert 0 <= value <= 10**18

    assert value == b

# Provided samples.
assert run("0 0") == "Stable\n0\n", "sample 1"

# Any valid construction is accepted, so validate the sample instead
# of requiring the sample's particular two-operation sequence.
validate("1 9")

# Minimum-size values.
validate("0 1")

# Both endpoints equal.
validate("123456789 123456789")

# Maximum allowed input value.
validate("1000000000 1000000000")

# Boundary digits and many zeroes.
validate("100000000 900000009")

# Every digit is nonzero, exercising both directions heavily.
validate("987654321 123456789")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`Stable`,`0`hoạt động | Kích thước tối thiểu và ranh giới từ 0 đến 0 | 
|`0 1`| Bất kỳ hợp lệ`Stable`xây dựng | Xây dựng một chữ số từ số 0 | 
|`123456789 123456789`| Bất kỳ hợp lệ`Stable`xây dựng | Nhiều chữ số khác 0 liên tiếp | 
|`1000000000 1000000000`| Bất kỳ hợp lệ`Stable`xây dựng | Giá trị đầu vào tối đa và số 0 vị trí | 
|`100000000 900000009`| Bất kỳ hợp lệ`Stable`xây dựng | Chữ số hàng đầu, số 0 bên trong và chữ số`9`| 
|`987654321 123456789`| Bất kỳ hợp lệ`Stable`xây dựng | Cả hai hướng xây dựng với tất cả các chữ số hoạt động | 

## Vỏ cạnh 

cho`a = 0`Và`b = 0`, cả hai hàm xây dựng đều trả về một danh sách thao tác trống. Thuật toán in`Stable`Và`0`, đây chính xác là phép biến đổi hợp lệ đơn giản nhất. Không nhân tạo`+`hoặc`-`hoạt động là cần thiết. 

Đối với một số chứa số 0, hãy xem xét`a = 100`. Chữ số đầu tiên là`1`, do đó thuật toán thêm`10^2`đối tượng, nghĩa là thanh ghi tăng thêm`900`, thay đổi`100`vào trong`1000`. Loại bỏ phần dẫn đầu`1`lá`0`. Hai chữ số 0 còn lại không tạo ra hoạt động nào. Điều này có hiệu quả vì vị trí của chúng đã được tính bằng lũy ​​thừa mười được sử dụng khi xử lý chữ số đầu tiên. 

Đối với mục tiêu chứa số 0, hãy xem xét`b = 900000009`. Thuật toán xây dựng kết quả cuối cùng`9`Đầu tiên. Các số 0 ở giữa không yêu cầu thao tác rõ ràng, bởi vì khi số 0 đứng đầu sau đó`9`được chèn vào, sức mạnh mười của nó đã đặt nó chín vị trí so với hậu tố hiện có. Do đó, việc xây dựng tạo ra các số 0 dưới dạng khoảng trống vị trí thay vì cố gắng tạo ra chúng một cách riêng lẻ. 

Đối với chữ số`9`, cấu trúc xóa chung sẽ yêu cầu số 0 đứng đầu, đây không phải là thao tác xóa hợp lệ. Thuật toán xử lý trường hợp này một cách riêng biệt bằng cách thêm`10^k`các vật thể, tạo ra sự gia tăng`9 * 10^k`. Điều này trực tiếp thêm vào trước chữ số`9`, vì vậy không cần xóa. 

Đối với chữ số`1`, điều cực đoan ngược lại xảy ra. Chúng ta cần tám cái dẫn đầu khi xây dựng nó từ con số không. Ví dụ,`0 + 9 * 12345679 = 111111111`; xóa 8 lá cái đứng đầu`1`. số`12345679`không phải là tùy tiện. Tổng chữ số của nó là`37`và sản phẩm thu được có chính xác cấu trúc thập phân cần thiết. 

Để có đầu vào tối đa`1000000000`, giai đoạn đầu tiên chỉ thực hiện việc xây dựng cho giai đoạn đầu của nó`1`. Hoạt động sử dụng`10^9`, sản xuất`10^10`và xóa cái đứng đầu sẽ bằng 0. Các số 0 còn lại không cần thực hiện thao tác nào. Trường hợp này cũng giải thích vì sao`10^18`ràng buộc là an toàn: đầu vào mười chữ số duy nhất được ràng buộc cho phép có dạng đặc biệt này, vì vậy chúng ta không bao giờ gặp phải số có mười chữ số bắt đầu bằng một chữ số lớn khác 0. 

Điều tinh tế cuối cùng là trình tự đầu ra không cần phải tối thiểu. Vì`1 -> 9`, mẫu có cấu trúc hai thao tác, trong khi mẫu đơn giản`a -> 0 -> b`xây dựng sử dụng ba. Từ chối cái sau vì nó dài hơn sẽ hiểu sai yêu cầu đầu ra. Giới hạn duy nhất quan trọng là tính hợp pháp,`1000`ràng buộc hoạt động,`10^18`giới hạn trung gian và đạt được mục tiêu chính xác.

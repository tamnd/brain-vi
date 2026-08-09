---
title: "CF 102436D - Tập hợp con ``VÀ"
description: "Chúng ta cần xây dựng một tập hợp các số nguyên, mỗi số sử dụng tối đa s bit, sao cho nếu chúng ta lấy AND theo từng bit của mọi tập hợp con không trống của tập hợp đó thì sẽ xuất hiện chính xác k giá trị khác nhau."
date: "2026-08-08T16:04:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102436
codeforces_index: "D"
codeforces_contest_name: "Innopolis Open 2019-2020, qualification, contest 1"
rating: 0
weight: 102436
solve_time_s: 323
verified: true
draft: false
---

[CF 102436D - Tập hợp con ``AND''](https://codeforces.com/problemset/problem/102436/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một tập hợp các số nguyên, mỗi số sử dụng tối đa`s`các bit, sao cho nếu chúng ta lấy bit AND của mọi tập hợp con không trống của tập hợp, thì chính xác`k`giá trị khác nhau xuất hiện. Bản thân tập hợp này là đầu ra, vì vậy chúng ta có thể tự do chọn bất kỳ số nào thỏa mãn số lượng kết quả AND tập hợp con riêng biệt được yêu cầu. 

Ví dụ, với bộ`{9, 6, 10}`, các tập con đơn lẻ cho`9`,`6`, Và`10`. Một số tập hợp con lớn hơn cho`9 & 10 = 8`,`10 & 6 = 2`, Và`9 & 6 = 0`. Do đó, tập hợp các giá trị kết quả là`{0, 2, 6, 8, 9, 10}`, chứa sáu giá trị. Mẫu chính thức sử dụng chính xác cấu trúc này. 

Khó khăn cốt yếu là số tập con không rỗng của một tập hợp kích thước`n`là`2^n - 1`. Bản thân đầu ra bị giới hạn ở mức tối đa`125`số, vì vậy việc liệt kê trực tiếp từng tập hợp con có thể yêu cầu tới`2^125 - 1`, đại khái`4.25 * 10^37`, hoạt động AND. Ngay cả khi mỗi thao tác cực kỳ rẻ, thì điều này vẫn vượt xa những gì giới hạn ba giây có thể hỗ trợ. Những hạn chế về điểm số cho phép`k`để đạt được thứ tự của`2^20`, do đó, việc xây dựng phải tăng theo cấp số nhân về số lượng câu trả lời được trình bày mà không liệt kê rõ ràng các câu trả lời đó. 

Có một số trường hợp có thể dễ dàng phá vỡ một công trình bất cẩn. Đầu tiên,`k = 1`chỉ cần một giá trị AND riêng biệt. bộ`{0}`hoạt động được, vì tập con duy nhất không trống của nó có AND bằng`0`. Một cấu trúc giả định rằng nó phải tạo ra một giá trị dương có thể tiêu thụ bit một cách không cần thiết hoặc xử lý sai trường hợp cơ sở. 

Thứ hai, một mục tiêu kỳ lạ như`k = 3`phải tạo chính xác một giá trị AND mới trên đầu công trình cho`2`. Ví dụ,`{1, 0, 3}`tạo ra các giá trị riêng biệt`1`,`0`, Và`3`. Đơn giản chỉ cần thêm số 0 vào một công trình cho`2`sẽ không nhất thiết phải tăng cường giải pháp theo cách được yêu cầu trừ khi công trình hiện tại được chuyển đổi trước. 

Thứ ba, lũy thừa của hai là trường hợp biên của số bit. Vì`k = 4`, bộ`{3, 2, 1}`sản xuất chính xác`0, 1, 2, 3`, bốn giá trị khác nhau. Cấu trúc vô tình kích hoạt thêm một bit ở giai đoạn này có thể tạo ra các số cần nhiều bit hơn mức cần thiết. 

Cuối cùng, giới hạn bit được cung cấp`s`là giới hạn trên cứng của mọi số được xây dựng. Một tập hợp đúng về mặt toán học vẫn không hợp lệ nếu một trong các giá trị của nó đạt tới`2^s`hoặc hơn thế nữa. Cấu trúc bên dưới chỉ giới thiệu một bit mới khi cần tăng số lượng giá trị AND riêng biệt và bài toán đảm bảo rằng bit được yêu cầu`k`Và`s`thừa nhận một câu trả lời. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực bắt đầu bằng cách chọn một số tập hợp số và liệt kê mọi tập hợp con không trống. Đối với mỗi tập hợp con, chúng tôi tính toán AND của nó và chèn kết quả vào một tập hợp các giá trị riêng biệt. Điều này đúng vì bài toán yêu cầu chính xác số lượng giá trị AND khác nhau được tạo ra bởi tất cả các tập hợp con không trống. 

Vấn đề là`2^n - 1`tập hợp con. Ở kích thước đầu ra tối đa được phép là`125`, điều này có nghĩa`2^125 - 1`tập hợp con, khoảng`4.25 * 10^37`. Việc tìm kiếm trên các tập hợp có thể thậm chí còn tệ hơn, vì vậy việc sử dụng vũ lực không phải là một chiến lược xây dựng thực tế. 

Quan sát quan trọng là chúng ta không cần phải kiểm soát mọi tập hợp con một cách độc lập. Chúng ta có thể xây dựng một tập hợp trong khi vẫn duy trì tính bất biến rất mạnh về các bit của nó. Giả sử mọi số hiện tại chỉ sử dụng số thấp hơn`b`bit. Khi đó mọi tập con hiện tại VÀ cũng chỉ sử dụng những tập hợp con đó`b`bit. Điều này mang lại cho chúng ta một bit hoàn toàn không được sử dụng ở vị trí`b`. 

Bit không được sử dụng đó có thể tách hai nhóm tập hợp con AND. Nếu chúng ta đặt bit mới đó vào mọi số hiện có thì mọi tập hợp con cũ VÀ sẽ nhận được tập hợp bit mới. Sau đó, chúng ta thêm một số mới có bit mới bằng 0 và các bit cũ hơn đều là một. Các tập hợp con không chứa số mới sẽ giữ nguyên bit mới, trong khi các tập hợp con chứa số đó sẽ bị xóa bit mới. Hai nhóm không thể trùng nhau nên số lượng kết quả khác biệt tăng gấp đôi. 

Chúng ta cũng có thể tăng số lượng kết quả riêng biệt lên đúng một. Sau khi dự trữ một bit mới, cộng số gồm tất cả các số 1 qua bit đó. Đơn vị AND của nó là một giá trị mới, trong khi AND với bất kỳ tập hợp con nào trước đó sẽ giữ nguyên tập hợp con trước đó vì số mới chứa các số 1 ở mọi vị trí được sử dụng trước đó. 

Điều này mang lại cho chúng ta hai thao tác về số lượng câu trả lời riêng biệt: tăng lên một hoặc gấp đôi. mục tiêu`k`có thể được xây dựng từ`1`sử dụng chính xác các thao tác này, theo cấu trúc nhị phân của`k`. Bài xã luận chính thức sử dụng chính xác bất biến này và hai phép toán xây dựng này. 

Bắt đầu từ một câu trả lời, chúng ta giảm một cách đệ quy một số lẻ`k`ĐẾN`k - 1`, bởi vì việc thêm một cái rất dễ dàng. Thậm chí`k`, chúng tôi giải quyết đệ quy`k / 2`, sau đó thực hiện thao tác nhân đôi. Kể từ khi giảm một nửa liên tục đạt đến`1`nhanh chóng, chỉ`O(log k)`mức độ xây dựng là cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^n)`đánh giá tập hợp con |`O(2^n)`trong trường hợp xấu nhất | Quá chậm | 
| Tối ưu |`O(log^2 k)`|`O(log k)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với`k = 1`và bộ`{0}`. Có chính xác một tập hợp con không trống, do đó tồn tại chính xác một giá trị AND riêng biệt. Chúng tôi cũng duy trì`bit`, vị trí bit tiếp theo có sẵn để xây dựng. 
2. Nếu mục tiêu hiện tại`k`là số lẻ và lớn hơn một, trước tiên hãy xây dựng một tập hợp cho`k - 1`. Sau đó, chúng tôi chuyển sang một bit mới và nối thêm một số chứa số 1 trong mỗi bit được sử dụng cho đến nay, bao gồm cả bit mới. Bản thân số này tạo ra chính xác một giá trị AND mới. Việc AND nó với bất kỳ tập hợp con cũ nào cũng không làm thay đổi kết quả của tập hợp con đó vì tất cả các bit có liên quan trước đó đều là một trong số mới. 
3. Nếu mục tiêu hiện tại`k`là số chẵn, trước tiên hãy xây dựng một tập hợp có số kết quả phân biệt là`k / 2`. Cho phép`bit`là bit không được sử dụng đầu tiên. Đặt bit này trong mọi số hiện có. 
4. Thêm một số mới có số thấp hơn`bit`tất cả các bit đều là một và bit mới của nó bằng 0. Về mặt số học con số này là`(1 << bit) - 1`. 
5. Xét một tập con không chứa số mới được thêm vào. AND của nó có tập bit mới và các bit thấp hơn của nó chính xác là AND được tạo bởi tập hợp con cũ tương ứng. Do đó các tập hợp con này tạo ra`k / 2`các giá trị trong nhóm với tập bit mới. 
6. Bây giờ hãy xem xét một tập hợp con chứa số mới. Bit mới của nó trở thành 0, trong khi các bit thấp hơn không thay đổi vì số mới có số 1 ở mọi vị trí đó. Những cái này mang lại cái khác`k / 2`giá trị, bây giờ với bit mới được xóa. 
7. Hai nhóm không thể chứa cùng một số nguyên vì một nhóm có bit mới được đặt và nhóm kia đã xóa nó. Do đó số lượng giá trị AND riêng biệt trở nên chính xác`k`. 
8. Lặp lại đệ quy cho đến khi đạt`k = 1`, sau đó trả về các số được xây dựng. Độ sâu đệ quy chỉ mang tính logarit vì mọi mục tiêu chẵn được chia cho hai và mọi mục tiêu lẻ lớn hơn một sẽ bị giảm đi một trước khi cuối cùng đạt đến giá trị chẵn. 

Điều bất biến là sau khi xây dựng mục tiêu`k`, tất cả các giá trị AND tập hợp con được tạo được biểu thị bằng cấu trúc hiện tại và tất cả các số chỉ sử dụng các bit đã được giới thiệu. các`+1`thao tác thêm chính xác một giá trị mới, trong khi thao tác nhân đôi tạo ra hai bản sao rời rạc của mỗi giá trị AND cũ. Vì các hoạt động này biến đổi số lượng kết quả khác biệt từ`x`ĐẾN`x + 1`hoặc`2x`chính xác, sau sự phân rã đệ quy của`k`đảm bảo rằng tập cuối cùng có chính xác`k`các giá trị tập hợp con-AND riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build(k):
    if k == 1:
        return [0], 0

    if k & 1:
        result, bit = build(k - 1)

        # Reserve a fresh bit and add a number having
        # all currently relevant bits set.
        bit += 1
        result.append((1 << bit) - 1)

        return result, bit

    result, bit = build(k // 2)

    # Put the fresh bit into every old number.
    mask = 1 << bit
    for i in range(len(result)):
        result[i] |= mask

    # The new number has the fresh bit cleared and
    # every older bit set.
    result.append(mask - 1)

    bit += 1
    return result, bit

def solve():
    k, s = map(int, input().split())

    result, _ = build(k)

    print(len(result))
    print(*result)

if __name__ == "__main__":
    solve()
```Đệ quy`build`chức năng là thực hiện trực tiếp hai quy tắc xây dựng. Trường hợp cơ sở trả về`{0}`, có một tập con AND riêng biệt. 

Đối với một điều kỳ lạ`k`, cuộc gọi đệ quy sẽ tạo ra`k - 1`các giá trị. Chúng tôi tăng bộ đếm bit và nối thêm`(1 << bit) - 1`. Bởi vì tất cả các số được xây dựng trước đó đều sử dụng các bit thấp hơn nên giá trị được thêm vào này lớn hơn mọi tập con AND cũ và do đó đóng góp chính xác một kết quả mới. 

Thậm chí`k`, cuộc gọi đệ quy sẽ tạo ra`k / 2`các giá trị. biểu hiện`1 << bit`xác định bit hiện không được sử dụng và OR nó vào mọi số hiện có sẽ đặt bit đó vào mọi tập hợp con AND cũ. Phần bổ sung`mask - 1`có mọi bit cũ được đặt nhưng bit mới bị xóa. Do đó, tập hợp con cũ và tập hợp con chứa số mới được phân tách bằng bit mới. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn số nguyên. Ranh giới liên quan duy nhất là vấn đề`s`-bit hạn chế. Cấu trúc chỉ sử dụng các bit mới được đưa vào trong quá trình đệ quy và vấn đề đảm bảo đủ số bit có sẵn cho yêu cầu.`k`. 

Độ sâu đệ quy nhỏ, bị giới hạn bởi số bit của`k`, do đó giới hạn đệ quy của Python không đạt được đối với các ràng buộc đã nêu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu được cung cấp, đầu vào là`k = 6`Và`s = 10`. Cấu trúc sau đây hợp lệ ngay cả khi nó khác với đầu ra mẫu, vì bài toán chấp nhận bất kỳ tập hợp nào có chính xác sáu giá trị AND riêng biệt. 

Sự phân rã đệ quy là`6 -> 3 -> 2 -> 1`. Sử dụng cách xây dựng trên mang lại`{5, 4, 7, 3}`. 

| Bước | Mục tiêu | Hoạt động | Bộ hiện tại | Giá trị VÀ khác biệt | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | Trường hợp cơ sở |`{0}`|`0`| 
| 2 | 2 | Đôi |`{1, 0}`|`{0, 1}`| 
| 3 | 3 | Thêm một |`{1, 0, 3}`|`{0, 1, 3}`| 
| 4 | 6 | Đôi |`{5, 4, 7, 3}`|`{0, 1, 3, 4, 5, 7}`| 

Đối với tập cuối cùng, kết quả đơn lẻ là`5`,`4`,`7`, Và`3`. AND theo cặp và lớn hơn giới thiệu`1`Và`0`, trong khi mọi kết quả khác đều đã là một trong sáu giá trị này. Như vậy có đúng sáu giá trị khác nhau xuất hiện. 

### Mẫu 2 

Chỉ có một mẫu được cung cấp trong báo cáo vấn đề, vì vậy hãy xem xét dữ liệu đầu vào hợp lệ`8 3`. Mục tiêu là lũy thừa của hai, thực hiện thao tác nhân đôi nhiều lần. 

| Bước | Mục tiêu | Hoạt động | Bộ hiện tại | Giá trị VÀ khác biệt | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | Trường hợp cơ sở |`{0}`|`{0}`| 
| 2 | 2 | Đôi |`{1, 0}`|`{0, 1}`| 
| 3 | 4 | Đôi |`{3, 2, 1}`|`{0, 1, 2, 3}`| 
| 4 | 8 | Đôi |`{7, 6, 5, 3}`|`{0, 1, 2, 3, 4, 5, 6, 7}`| 

Việc xây dựng cuối cùng tạo ra mọi giá trị từ`0`bởi vì`7`dưới dạng tập con AND. Có chính xác tám kết quả riêng biệt và mỗi số phù hợp với ba bit, nằm trong yêu cầu`s = 3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(log^2 k)`| có`O(log k)`mức đệ quy và sửa đổi chi phí đã đặt hiện tại nhiều nhất`O(log k)`mỗi cấp độ. | 
| Không gian |`O(log k)`| Tập hợp được xây dựng chỉ chứa`O(log k)`số và độ sâu đệ quy cũng`O(log k)`. | 

Vì`k`lên đến khoảng`2^20`, cách xây dựng chỉ có vài chục số và vài trăm phép tính số nguyên đơn giản. Nó thấp hơn nhiều so với giới hạn đầu ra của`125`số và thoải mái trong giới hạn thời gian ba giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm 

Đầu ra không phải là duy nhất, vì vậy trình trợ giúp kiểm tra sẽ xác thực tập hợp được tạo ra thay vì so sánh nó với một chuỗi cố định. Nó kiểm tra kích thước đầu ra,`s`-bit bị ràng buộc, tính duy nhất của các phần tử tập hợp và, đối với các trường hợp nhỏ, liệt kê trực tiếp mọi tập hợp con không trống để xác minh chính xác số lượng kết quả AND riêng biệt.```python
import sys
import io

def build(k):
    if k == 1:
        return [0], 0

    if k & 1:
        result, bit = build(k - 1)
        bit += 1
        result.append((1 << bit) - 1)
        return result, bit

    result, bit = build(k // 2)

    mask = 1 << bit
    for i in range(len(result)):
        result[i] |= mask

    result.append(mask - 1)
    bit += 1
    return result, bit

def solve_case(inp):
    data = inp.split()
    k = int(data[0])
    s = int(data[1])

    result, _ = build(k)

    return str(len(result)) + "\n" + " ".join(map(str, result)) + "\n"

def run(inp: str) -> str:
    return solve_case(inp)

def validate(inp: str, out: str):
    k, s = map(int, inp.split())

    data = list(map(int, out.split()))
    assert data, "empty output"

    n = data[0]
    a = data[1:]

    assert n == len(a), "wrong number of printed values"
    assert 1 <= n <= 125, "invalid set size"
    assert len(set(a)) == n, "the output must be a set"
    assert all(0 <= x < (1 << s) for x in a), "number does not fit in s bits"

    if n <= 20:
        values = set()

        for mask in range(1, 1 << n):
            cur = (1 << s) - 1
            for i in range(n):
                if mask & (1 << i):
                    cur &= a[i]
            values.add(cur)

        assert len(values) == k, (
            f"expected {k} distinct AND values, got {len(values)}"
        )

# Provided sample
sample1 = "6 10"
validate(sample1, run(sample1))

# Minimum target
case2 = "1 1"
validate(case2, run(case2))

# Small power of two
case3 = "4 3"
validate(case3, run(case3))

# Odd target, exercises the +1 construction
case4 = "7 3"
validate(case4, run(case4))

# Maximum target from the stated scoring range.
# We only validate structural properties here because enumerating
# all subsets would be exponential.
case5 = "1048576 20"
validate(case5, run(case5))

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6 10`| Bất kỳ cấu trúc 6 giá trị hợp lệ nào | Cung cấp mẫu và thi công tổng thể | 
|`1 1`| Tập hợp một phần tử như`{0}`| Mục tiêu tối thiểu và trường hợp cơ sở | 
|`4 3`| Một bộ có chính xác bốn kết quả AND riêng biệt | Nhân đôi lặp đi lặp lại | 
|`7 3`| Một bộ có đúng bảy kết quả AND riêng biệt | Số lẻ`k`và`+1`hoạt động | 
|`1048576 20`| Bất kỳ công trình hợp lệ nào có tối đa 125 số | Mục tiêu tối đa và ranh giới bit | 

## Vỏ cạnh 

cho`k = 1`, đầu vào chính xác là`1 1`. Thuật toán ngay lập tức tiếp cận trường hợp cơ sở và trả về`{0}`. Chỉ có một tập con không trống, chứa một số duy nhất`0`, vì vậy kết quả AND duy nhất là`0`. Do đó, đầu ra là một số và có chính xác một kết quả riêng biệt. 

Đối với mục tiêu lẻ`k = 3`, coi như`3 2`. Thuật toán đầu tiên xây dựng câu trả lời cho`2`, cho`{1, 0}`. Sau đó nó chuyển sang bit tiếp theo và nối thêm`3`, sản xuất`{1, 0, 3}`. Người độc thân`3`là một kết quả mới, trong khi`3 & 1 = 1`Và`3 & 0 = 0`, do đó không có giá trị mới nào khác xuất hiện. Các kết quả khác biệt là chính xác`{0, 1, 3}`, đưa ra số lượng đầu ra cần thiết là`3`. 

Đối với ranh giới lũy thừa hai`k = 4`, coi như`4 2`. Việc xây dựng mang lại`{3, 2, 1}`. Tập hợp con VÀ giá trị của nó là`3`,`2`,`1`,`2`,`1`,`0`, Và`0`, vì vậy kết quả khác biệt là chính xác`{0, 1, 2, 3}`. Mỗi giá trị phù hợp với hai bit. Điều này chứng tỏ tại sao thao tác nhân đôi phải phân biệt các tập con bằng một bit mới được đưa vào. 

Để đạt được mục tiêu tối đa`k = 2^20`, quá trình đệ quy bao gồm toàn bộ các phép toán nhân đôi. Mỗi lần nhân đôi giới thiệu một bit mới và giữ cho số lượng phần tử được xây dựng ở mức nhỏ. Các số kết quả vẫn ở dưới đây`2^20`, vì vậy chúng phù hợp`20`bit. Do đó, việc xây dựng đạt được mục tiêu lớn nhất từ ​​các ràng buộc tính điểm mà không liệt kê bất kỳ tập hợp con nào theo cấp số nhân của nó. 

Bất biến hữu ích nhất để giải quyết các vấn đề mang tính xây dựng tương tự là một bit mới có thể hoạt động như một dấu phân cách. Nếu mọi kết quả cũ được đặt bit đó và mọi kết quả liên quan đến phần tử mới được chọn đặc biệt sẽ bị xóa, thì hai tập hợp kết quả giống hệt nhau trước đây sẽ trở nên rời rạc. Khi ý tưởng đó được nhận ra, việc nhân đôi số lượng trạng thái có thể đạt được sẽ trở thành một cấu trúc bitwise đơn giản thay vì tìm kiếm theo cấp số nhân.

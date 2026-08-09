---
title: "CF 102443H - Hành tinh thứ chín"
description: "Thanh ghi bắt đầu ở a và phải kết thúc ở b. Chỉ có hai loại sự kiện. Phép cộng làm tăng thanh ghi lên bội số dương của 9, trong khi phép xóa sẽ loại bỏ một số chữ số thập phân đứng đầu và mọi chữ số bị loại bỏ phải là 1."
date: "2026-08-09T18:09:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102443
codeforces_index: "H"
codeforces_contest_name: "2019-2020 Russia Team Open, High School Programming Contest (VKOSHP 19)"
rating: 0
weight: 102443
solve_time_s: 479
verified: true
draft: false
---

[CF 102443H - Hành tinh thứ chín](https://codeforces.com/problemset/problem/102443/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 59 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Việc đăng ký bắt đầu lúc`a`và phải kết thúc tại`b`. Chỉ có hai loại sự kiện. Một phép cộng sẽ làm tăng thanh ghi lên bội số dương của 9, trong khi việc xóa sẽ loại bỏ một số chữ số thập phân đứng đầu và mọi chữ số bị loại bỏ phải là`1`. Đầu ra là bất kỳ chuỗi hợp lệ nào gồm tối đa 1000 sự kiện như vậy. Mỗi giá trị thanh ghi trung gian phải ở mức tối đa là 10 18. 

Đại lượng hữu ích là phần dư modulo 9. Việc cộng 9x không bao giờ làm thay đổi nó. Nếu chúng ta xóa`y`những số đứng đầu từ một số thập phân thì tiền tố bị loại bỏ sẽ phù hợp với`y`modulo 9 và mọi lũy thừa của 10 cũng bằng 1 modulo 9. Do đó xóa`y`các chữ số giảm phần còn lại một cách chính xác`y`modulo 9. Đây là thuộc tính số học trung tâm đằng sau việc xây dựng. 

Giới hạn trên`a`Và`b`đủ nhỏ để biểu diễn thập phân của chúng có tối đa 10 chữ số. Giới hạn giá trị trung gian lớn hơn nhiều, 10 18, vì vậy chúng ta có chỗ để tạm thời tạo ra các số có nhiều chữ số hơn đáng kể. Giới hạn hoạt động là 1000, loại trừ bất kỳ tìm kiếm nào thông qua chuỗi hoạt động dài, nhưng việc xây dựng bên dưới chỉ cần vài chục hoạt động. 

Có một số trường hợp đặc biệt có thể khiến một công trình có vẻ hợp lý bị thất bại. Vì`0 0`, câu trả lời đúng chỉ đơn giản là`Stable`theo sau là`0`, bởi vì không có sự kiện nào là cần thiết. Một chương trình luôn đưa ra phép cộng tích cực sẽ thay đổi thanh ghi một cách không cần thiết. 

Vì`1 9`, hai giá trị có số dư khác nhau theo modulo 9. Chỉ cộng các số chín không thể thay đổi`1`vào trong`9`. Một cách xây dựng hợp lệ là mẫu`+ 2`, cái nào thay đổi`1`ĐẾN`19`, theo sau là`- 1`, loại bỏ phần đầu`1`và lá`9`. Một chương trình chỉ kiểm tra xem`a <= b`và sau đó thêm`(b-a)/9`sẽ từ chối trường hợp này một cách không chính xác. 

Vụ án`0 1`là một ranh giới hữu ích khác. Không có phép cộng trực tiếp vì 1 không phải là bội số của 9. Chúng ta có thể liên tục tạo một số dẫn đầu`1`và loại bỏ nó, thay đổi phần còn lại từng bước một, cuối cùng đạt tới 1. Một công trình là`0 -> 18 -> 8 -> 17 -> 7 -> ... -> 11 -> 1`. Các giá trị tạm thời lớn hơn giá trị cuối cùng nhưng chúng vẫn thấp hơn nhiều so với 10 18. 

Cuối cùng, các giá trị như`1000000000`cần được chăm sóc vì chúng có 10 chữ số. Công trình tạo ra một số có quá nhiều số đứng đầu có thể vượt quá giới hạn 10 18. Cấu trúc tối ưu có chủ ý sử dụng đơn vị tối đa 18 chữ số, có giá trị dưới 1,12⋅10 17. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực có thể mô hình hóa mọi giá trị thanh ghi có thể có dưới dạng trạng thái và thử cả hai loại hoạt động từ mọi trạng thái. Điều này thất bại ngay lập tức vì phép cộng có rất nhiều đối số có thể xảy ra. Ngay từ số 0, vẫn có 

⌊ 9 10 18 ​ ⌋=111111111111111111 

những bổ sung tích cực có thể có tôn trọng giới hạn giá trị trung gian. Chuỗi thao tác tìm kiếm thậm chí còn tệ hơn: nếu chúng ta hạn chế một cách giả tạo chỉ có hai lựa chọn cố định ở mỗi bước, thì tìm kiếm độ sâu 1000 sẽ chứa 2 1000 chuỗi. Tập hoạt động thực tế lớn hơn nhiều, vì vậy vũ lực không phải là một lựa chọn có ý nghĩa. 

Lực lượng vũ phu hoạt động về mặt khái niệm vì cuối cùng nó sẽ phát hiện ra một chuỗi hợp lệ nếu có. Quan sát hữu ích là chúng ta không cần phải tìm kiếm các phần bổ sung nào cả. Chúng ta có thể chọn chúng theo đại số. 

Giả sử giá trị hiện tại là`v`, và để nó có`d`chữ số thập phân. Xác định 

t=(v−1)mod9. 

Bây giờ hãy xem xét 

T=10 d +t. 

số`T`bắt đầu bằng chữ số`1`và 

T≡1+t≡v(mod9). 

Do đó`T-v`là bội số dương của 9. Chúng ta có thể đạt được`T`bằng một phép cộng, xóa chữ số đầu tiên của nó và thu được chính xác`t`. Do đó, hai thao tác biến đổi bất kỳ`v`thành một số có số dư theo modulo 9 nhỏ hơn số dư cũ một đơn vị. 

Điều này cho chúng ta một cách xác định để điều chỉnh phần dư modulo 9 cho đến khi nó khớp`b`. Khi phần còn lại khớp và giá trị hiện tại không vượt quá`b`, một phép cộng duy nhất đạt tới`b`. 

Có một điều phức tạp. Nếu như`a`ít nhất là lớn bằng`b`, quá trình điều chỉnh phần dư tạm thời có thể chuyển qua các giá trị lớn hơn`b`và phép cộng cuối cùng có thể trở thành số âm. Chúng tôi tránh điều đó hoàn toàn bằng cách gửi trước tiên`a`về không. Để thực hiện việc này, hãy chọn đơn vị thập phân 

RL ​ =11…1 

chiều dài của ai`L`phù hợp với`a`modulo 9 và có giá trị ít nhất là`a`. Như vậy`L`luôn tồn tại với L<18. Vì RL ​ ≡L(mod9), hiệu`R_L-a`chia hết cho 9. Ta cộng số chênh lệch đó rồi xóa tất cả`L`những người dẫn đầu, đạt đến số không. Đây chính xác là cách xây dựng hai giai đoạn được mô tả trong bài xã luận chính thức. 

Từ số 0 chúng ta có thể sử dụng quá trình điều chỉnh số dư một cách an toàn để đạt được số dư của`b`, sau đó cộng bội số còn lại của 9. Tổng số sự kiện vẫn rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong giới hạn hoạt động, với >10 17 lần bổ sung đầu tiên có thể có | Hàm mũ | Quá chậm | 
| Tối ưu | O(log 10 ​ max(a,b)) | O(log 10 ​ max(a,b)) cho đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`a == b`, đầu ra`Stable`và không hoạt động. Không cần xây dựng. 
2. Nếu`a < b`, giữ giá trị hiện tại bằng`a`và liên tục điều chỉnh phần dư của nó theo modulo 9. Đối với giá trị hiện tại`v`, tính số chữ số của nó`d`, sau đó đặt`t = (v - 1) % 9`Và`T = 10^d + t`. 
3. Thêm`(T-v)/9`số chín. Kết quả là chính xác`T`, Và`T`bắt đầu bằng`1`. Thương số dương vì`T > v`, trong khi nó tích phân vì`T`Và`v`có cùng số dư modulo 9. 
4. Xóa một chữ số đứng đầu. Vì chữ số đó là`1`, hoạt động là hợp pháp và sổ đăng ký trở thành`t`. Số dư của nó nhỏ hơn số dư trước modulo 9 một đơn vị. 
5. Lặp lại hai bước trước cho đến khi giá trị hiện tại có cùng số dư modulo 9 như`b`. Cần nhiều nhất chín phép biến đổi như vậy vì mỗi phép biến đổi sẽ giảm phần dư đi một modulo 9. 
6. Thêm`(b-v)/9`. Phần dư bây giờ khớp nhau nên thương là số nguyên. Bởi vì`a < b`, sau giai đoạn điều chỉnh còn lại, giá trị hiện tại tối đa là 8, do đó nó không lớn hơn`b`. Do đó, phép cộng cuối cùng là dương trừ khi các giá trị đã bằng nhau. 
7. Nếu`a > b`, đầu tiên xây dựng một đơn vị lại`R`có nhiều nhất 18 chữ số sao cho`R >= a`Và`R % 9 == a % 9`. Độ dài như vậy có thể được tìm thấy bằng cách kiểm tra độ dài từ 1 đến 18. 
8. Thêm`(R-a)/9`số chín, trừ khi`R == a`, trong trường hợp đó không cần bổ sung. Việc đăng ký bây giờ là chính xác`R`. 
9. Xóa tất cả`len(R)`chữ số hàng đầu. Mỗi chữ số là`1`, do đó việc xóa là hợp lệ và thay đổi thanh ghi thành 0. 
10. Bắt đầu từ số 0, áp dụng quy trình điều chỉnh số dư tương tự đối với`b`, sau đó thực hiện phép cộng cuối cùng từ phần dư phù hợp vào`b`. 

Sự bất biến rất đơn giản. Mỗi phép cộng được xây dựng sẽ thay đổi thanh ghi theo bội số của 9, do đó nó giữ nguyên phần còn lại hiện tại. Mỗi thao tác xóa sẽ loại bỏ chính xác một phần đầu`1`trong cách xây dựng của chúng ta, do đó nó giảm phần còn lại đi một modulo 9. Số được xây dựng trước mỗi lần xóa luôn là`10^d+t`, nghĩa là chữ số đầu tiên của nó được đảm bảo là`1`và việc xóa là hợp pháp. Khi số dư bằng`b % 9`, sự khác biệt từ giá trị hiện tại đến`b`là bội số của 9, do đó phép cộng cuối cùng đạt`b`chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 10**18

def add_to_repunit(a, ops):
    """Transform a to zero using one repunit."""
    if a == 0:
        return 0

    # Find a repunit R >= a with R % 9 == a % 9.
    rep = 0
    chosen = None

    for length in range(1, 19):
        rep = rep * 10 + 1
        if rep >= a and rep % 9 == a % 9:
            chosen = rep
            chosen_len = length
            break

    # Such a repunit always exists for a <= 1e9.
    if chosen is None:
        raise RuntimeError("No suitable repunit")

    if chosen > a:
        x = (chosen - a) // 9
        ops.append(("+", x))

    ops.append(("-", chosen_len))
    return 0

def adjust_residue(cur, target_residue, ops):
    """
    Repeatedly decrease cur's residue modulo 9 until it equals
    target_residue. Each iteration uses +x, -1.
    """
    while cur % 9 != target_residue:
        d = len(str(cur))
        t = (cur - 1) % 9
        target = 10**d + t

        x = (target - cur) // 9
        if x <= 0:
            raise RuntimeError("Invalid positive addition")

        ops.append(("+", x))
        ops.append(("-", 1))

        cur = t

    return cur

def solve():
    a, b = map(int, input().split())

    if a == b:
        print("Stable")
        print(0)
        return

    ops = []

    if a >= b:
        # First reach zero.
        cur = add_to_repunit(a, ops)

        # Then construct b from zero.
        cur = adjust_residue(cur, b % 9, ops)

        if cur < b:
            x = (b - cur) // 9
            ops.append(("+", x))
            cur = b
    else:
        # a < b, so we can directly adjust the residue and then add.
        cur = adjust_residue(a, b % 9, ops)

        if cur < b:
            x = (b - cur) // 9
            ops.append(("+", x))
            cur = b

    assert cur == b
    assert len(ops) <= 1000

    print("Stable")
    print(len(ops))
    for typ, x in ops:
        print(typ, x)

if __name__ == "__main__":
    solve()
```các`add_to_repunit`chức năng thực hiện nửa đầu của công trình cho`a >= b`. Nó chỉ tìm kiếm 18 độ dài đơn vị có thể có. Đối với mỗi ứng cử viên, tính chia hết cho 9 được tính từ`rep % 9 == a % 9`, vì vậy số lượng phép cộng cần thiết là một số nguyên. 

các`adjust_residue`chức năng thực hiện chuyển đổi hai thao tác chính. Vì`cur`với`d`chữ số,`10**d + t`có chính xác`d+1`chữ số và bắt đầu bằng`1`. Số lượng bổ sung được tính toán trước khi thực hiện thao tác, điều này tránh mọi sự phụ thuộc vào hành vi mang số thập phân. 

Sau đó`- 1`, giá trị mới chính xác là`t`, vì việc xóa chữ số đầu tiên khỏi`10^d+t`để lại biểu diễn thập phân của`t`, bao gồm cả khả năng`t`là số không. 

Đơn vị trả lại 18 chữ số ở mức an toàn dưới 10 18 và tất cả các giá trị tạm thời khác được tạo từ đầu vào có tối đa 10 chữ số nhiều nhất là 10 10 +8. Số nguyên Python cũng có độ chính xác tùy ý, do đó không có hiện tượng tràn số học trong quá trình xây dựng. 

Thứ tự hoạt động quan trọng. Phép cộng phải diễn ra trước khi xóa vì việc xóa chỉ hợp lệ khi chữ số đứng đầu là`1`. Phép cộng cuối cùng chỉ được thực hiện sau khi phần dư modulo 9 đồng ý, đảm bảo rằng đối số của nó là số nguyên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào`0 0`, các giá trị đã trùng khớp. 

| Hiện tại | Mục tiêu | Hành động | Hiện Tại Mới | 
| --- | --- | --- | --- | 
| 0 | 0 | không | 0 | 

Thuật toán ngay lập tức in các hoạt động bằng 0. Đây là cách xây dựng nhỏ nhất có thể và tránh tạo ra một hoạt động tích cực không cần thiết. 

### Mẫu 2 

Đối với đầu vào`1 9`, chúng tôi có`1 < 9`, và phần còn lại mục tiêu là`0`. 

| Hiện tại |`current % 9`|`t = (current-1) % 9`| Giá trị xây dựng | Hành động | Hiện Tại Mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 10 |`+ 1`,`- 1`| 0 | 
| 0 | 0 | 0 | 0 |`+ 1`| 9 | 

Cặp đầu tiên thay đổi phần còn lại từ 1 thành 0. Phép cộng cuối cùng thay đổi 0 thành 9. Mẫu sử dụng cấu trúc ngắn hơn,`+ 2`,`- 1`, nhưng vấn đề chấp nhận bất kỳ chuỗi hợp lệ nào. 

Dấu vết cho thấy tại sao việc giảm thiểu số lượng thao tác là không cần thiết. Việc xây dựng có thể sử dụng một vài sự kiện bổ sung, nhưng nó vẫn thấp hơn nhiều so với giới hạn 1000. 

### Ví dụ về ranh giới 

Hãy xem xét`0 1`. 

| Hiện tại |`current % 9`|`t`| Giá trị xây dựng | Hiện Tại Mới | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 8 | 18 | 8 | 
| 8 | 8 | 7 | 17 | 7 | 
| 7 | 7 | 6 | 16 | 6 | 
| 6 | 6 | 5 | 15 | 5 | 
| 5 | 5 | 4 | 14 | 4 | 
| 4 | 4 | 3 | 13 | 3 | 
| 3 | 3 | 2 | 12 | 2 | 
| 2 | 2 | 1 | 11 | 1 | 

Số dư mong muốn là 1 nên quá trình dừng ở 1. Không cần thêm phép cộng cuối cùng. 

Ví dụ này thực hiện trường hợp mục tiêu cực kỳ nhỏ mặc dù việc xây dựng tạm thời tạo ra các số có hai chữ số. Giá trị tạm thời lớn nhất chỉ là 18. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(logmax(a,b)) | Tối đa 18 vòng kiểm tra đơn vị lại và tối đa 9 vòng điều chỉnh dư lượng, mỗi vòng sử dụng số nguyên có kích thước không đổi | 
| Không gian | O(logmax(a,b)) | Danh sách phép toán chỉ chứa một số phép toán không đổi liên quan đến số thập phân | 

Số lượng hoạt động thực tế tối đa là 21 với việc triển khai này. Việc chuyển đổi đơn vị lại ban đầu sử dụng tối đa hai sự kiện và việc điều chỉnh dư lượng sử dụng tối đa 18 sự kiện, sau đó là một lần bổ sung cuối cùng. Như vậy giới hạn yêu cầu là 1000 thao tác là vô cùng lỏng lẻo. Tất cả các giá trị tạm thời đều dưới 10 18, do đó việc xây dựng cũng thỏa mãn giới hạn đăng ký. 

## Trường hợp thử nghiệm 

Vì đầu ra không phải là duy nhất nên các thử nghiệm nên xác nhận trình tự vận hành được tạo ra thay vì so sánh nó với một đầu ra cố định.```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

def verify(inp: str, out: str) -> bool:
    a, b = map(int, inp.split())
    lines = out.strip().splitlines()

    assert lines[0] == "Stable"
    n = int(lines[1])
    assert 0 <= n <= 1000
    assert len(lines) == n + 2

    cur = a

    for line in lines[2:]:
        parts = line.split()
        assert len(parts) == 2

        typ, x = parts
        x = int(x)
        assert x > 0

        if typ == "+":
            cur += 9 * x
            assert cur <= 10**18

        elif typ == "-":
            s = str(cur)
            y = x

            assert 1 <= y <= len(s)
            assert all(ch == "1" for ch in s[:y])

            s = s[y:]
            cur = int(s) if s else 0
            assert cur <= 10**18

        else:
            assert False, "unknown operation"

    assert cur == b
    return True

# Provided sample 1 has an exact canonical output.
assert run("0 0") == "Stable\n0\n", "sample 1"

# Provided sample 2 has many valid outputs, so verify its semantics.
assert verify("1 9", run("1 9")), "sample 2"

# Minimum-size input.
assert verify("0 0", run("0 0")), "minimum values"

# All-equal nonzero values.
assert verify("123456789 123456789", run("123456789 123456789")), "equal values"

# Maximum input values.
assert verify("1000000000 1000000000", run("1000000000 1000000000")), "maximum equal values"

# Large value going down to zero, exercising the repunit construction.
assert verify("1000000000 0", run("1000000000 0")), "repunit boundary"

# Different residues with a very small target.
assert verify("0 1", run("0 1")), "small target"

# Adjacent boundary near 1e9.
assert verify("999999999 1000000000", run("999999999 1000000000")), "large adjacent values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`Stable`, không hoạt động | Bình đẳng ngay lập tức và giá trị tối thiểu | 
|`1 9`| Bất kỳ hợp lệ`Stable`xây dựng | Cung cấp mẫu và thay đổi dư lượng | 
|`123456789 123456789`|`Stable`, không hoạt động | Giá trị khác 0 bằng nhau | 
|`1000000000 1000000000`|`Stable`, không hoạt động | Ranh giới đầu vào tối đa | 
|`1000000000 0`| Bất kỳ hợp lệ`Stable`xây dựng | Chuyển đổi đơn vị và xóa nhiều cái hàng đầu | 
|`0 1`| Bất kỳ hợp lệ`Stable`xây dựng | Mục tiêu nhỏ có dư lượng modulo 9 khác | 
|`999999999 1000000000`| Bất kỳ hợp lệ`Stable`xây dựng | Giá trị liền kề lớn và điều chỉnh dư lượng lặp đi lặp lại | 

## Vỏ cạnh 

cho`0 0`, việc kiểm tra đẳng thức kết thúc trước bất kỳ phép xây dựng số học nào. Thanh ghi vẫn bằng 0, vì vậy đầu ra chính xác là`Stable`theo sau là`0`. 

Vì`1 9`, phép cộng trực tiếp là không thể vì hiệu là 8 chứ không phải bội số của 9. Trước tiên, thuật toán thay đổi phần dư từ 1 thành 0 bằng cách xây dựng 10 và xóa phần đầu của nó`1`, thu được bằng không. Sau đó, nó cộng một bội số của 9 và đạt 9. Giá trị tạm thời 10 là hợp lệ. 

Vì`0 1`, thuật toán bắt đầu với số dư 0 trong khi mục tiêu có số dư 1. Mỗi`+x, -1`cặp giảm phần còn lại đi một modulo 9. Chuỗi các giá trị sau khi xóa là`8, 7, 6, 5, 4, 3, 2, 1`, do đó đạt được dư lượng mục tiêu sau tám vòng. Mọi cấu trúc phép cộng`18`,`17`, ...,`11`, tương ứng, theo sau là xóa phần đầu`1`. 

Vì`1000000000 0`, thuật toán chọn đơn vị 10 chữ số`1111111111`. Nó có phần dư giống như`1000000000`, và nó lớn hơn nên hiệu sẽ chia hết cho 9. Cộng số chênh đó sẽ đạt đơn vị, sau đó xóa cả mười`1`chữ số tạo ra số không. Giá trị tạm thời lớn nhất chỉ là`1111111111`. 

Vì`999999999 1000000000`, giá trị ban đầu có số dư 0 trong khi mục tiêu có số dư 1. Quy trình điều chỉnh số dư trước tiên tạo ra 8, sau đó là 7, tiếp tục cho đến 1. Từ đó, hiệu`1000000000`chia hết cho 9 nên phép cộng cuối cùng sẽ đạt được mục tiêu một cách chính xác. Điều này chứng tỏ rằng giá trị thanh ghi trung gian không cần phải ở dưới mục tiêu cuối cùng, chỉ dưới giới hạn 10 18 được yêu cầu.

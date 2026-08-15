---
title: "CF 102348G - Hoán đổi chữ cái"
description: "Chúng ta có hai chuỗi s và t có cùng độ dài. Mỗi vị trí đều chứa a hoặc b. Một thao tác chọn bất kỳ vị trí nào trong s và bất kỳ vị trí nào trong t, sau đó hoán đổi hai ký tự."
date: "2026-08-15T17:31:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "G"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 224
verified: false
draft: false
---

[CF 102348G - Hoán đổi chữ cái](https://codeforces.com/problemset/problem/102348/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai chuỗi`s`Và`t`có cùng độ dài. Mỗi vị trí đều chứa một trong hai`a`hoặc`b`. Một thao tác chọn bất kỳ vị trí nào trong`s`và bất kỳ vị trí nào trong`t`, sau đó hoán đổi hai ký tự. Mục tiêu là làm cho toàn bộ hai chuỗi giống hệt nhau bằng cách sử dụng ít thao tác nhất có thể, đồng thời in ra một chuỗi hoán đổi tối ưu. 

Cách hữu ích để xem xét một vị trí là so sánh hai ký tự chiếm vị trí đó. Nếu họ đã đồng ý rồi thì không cần quan tâm đến quan điểm đó nữa. Nếu họ không đồng ý, vị trí đó thuộc loại`ab`, nghĩa`s[i] = a`Và`t[i] = b`, hoặc gõ`ba`, nghĩa`s[i] = b`Và`t[i] = a`. 

Chiều dài có thể đạt tới`2 * 10^5`, do đó, một thuật toán có công bậc hai có thể thực hiện xung quanh`4 * 10^10`lặp lại trong trường hợp xấu nhất, vượt xa giới hạn cuộc thi 2 giây cho phép. Chúng ta cần một giải pháp mà công việc của nó về cơ bản là tuyến tính theo`n`, chỉ với một lượng nhỏ sổ sách kế toán cho mỗi vị trí. Vì bảng chữ cái chỉ chứa hai ký tự nên các kiểu không khớp sẽ cung cấp cho chúng ta chính xác cấu trúc cần thiết để đạt được điều đó. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai có vẻ hợp lý không thành công. Đầu tiên, số lẻ không khớp là không thể xảy ra. Ví dụ,```
1
a
b
```có một`ab`không khớp. Chỉ có một`a`giữa hai ký tự và mọi cặp bằng nhau cuối cùng đều chứa 0 hoặc 2 ký tự`a`nhân vật. Không có chuỗi hoán đổi nào có thể thay đổi tổng số`a`ký tự, vì vậy đầu ra chính xác là`-1`. Việc triển khai bất cẩn chỉ đơn giản ghép các cặp không khớp mà không kiểm tra tính chẵn lẻ có thể khiến một vị trí không được giải quyết. 

Trường hợp cạnh thứ hai xảy ra khi cả hai loại không khớp đều có số lẻ. Ví dụ,```
2
ab
ba
```có một`ab`vị trí và một`ba`chức vụ. Việc ghép nối một lần hoán đổi trực tiếp không thể khắc phục được chúng vì hướng của chúng trái ngược nhau. Tuy nhiên, hai lần hoán đổi là đủ. Tráo đổi`s[1]`với`t[1]`, biến sự không khớp đầu tiên từ`ab`vào trong`ba`, sau đó trao đổi`s[1]`với`t[2]`. Cả hai vị trí đều trở nên bình đẳng. Giải pháp chỉ ghép các loại không khớp bằng nhau sẽ kết luận sai rằng trường hợp này là không thể. 

Trường hợp cạnh thứ ba là khi không có sự không khớp nào cả:```
3
aba
aba
```Câu trả lời đúng là`0`, không có đường dây hoạt động. Việc triển khai giả sử có ít nhất một điểm không khớp có thể vô tình truy cập vào danh sách trống hoặc in một thao tác không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liên tục tìm ra sự không phù hợp và tìm kiếm một vị trí khác có thể sửa được. Ví dụ, sau khi tìm thấy một`ab`vị trí, chúng ta có thể quét các vị trí còn lại để tìm vị trí khác`ab`vị trí và sử dụng một trao đổi chuỗi chéo để giải quyết cả hai. Nếu không có vị trí như vậy tồn tại, chúng ta có thể xử lý trường hợp đặc biệt liên quan đến một`ba`vị trí với hai lần hoán đổi. 

Chiến lược này hợp lý về mặt logic, bởi vì mọi tìm kiếm đều tìm kiếm một đối tác hợp lệ và mỗi hoạt động thành công sẽ giảm số lượng kết quả không khớp. Vấn đề là việc quét lặp đi lặp lại. Trong trường hợp xấu nhất có thể có`Θ(n)`sự không phù hợp và việc tìm kiếm từng đối tác có thể kiểm tra`Θ(n)`các vị trí. Tổng số so sánh ký tự có thể đạt tới`Θ(n²)`, đó là về`4 * 10^10`vì`n = 2 * 10^5`. Đó là quá nhiều cho thời hạn. 

Quan sát chính là chúng ta không cần phải tìm kiếm đối tác một cách linh hoạt. Thông tin duy nhất liên quan đến một vị trí sai là liệu nó có`ab`hoặc`ba`. Chúng tôi có thể thu thập tất cả các vị trí của từng loại trong một lần. 

Hai`ab`các vị trí luôn có thể được cố định cùng với một thao tác. Giả sử vị trí`x`Và`y`cả hai đều`ab`. Hoán đổi`s[x]`với`t[y]`trao đổi`a`Và`b`. Tại`x`,`s[x]`thay đổi từ`a`ĐẾN`b`, khớp`t[x]`. Tại`y`,`t[y]`thay đổi từ`b`ĐẾN`a`, khớp`s[y]`. Lập luận tương tự có tác dụng đối với hai`ba`các vị trí. 

Điều này ngay lập tức xử lý mọi cặp loại không khớp bằng nhau. Tình huống duy nhất còn lại là khi cả hai danh sách không khớp đều chứa một vị trí không ghép đôi. Vì tổng số điểm không khớp phải là số chẵn nên hai danh sách này đều có kích thước chẵn hoặc cả hai đều có kích thước lẻ. Trong trường hợp lẻ, hãy`x`là người còn lại`ab`vị trí và`y`phần còn lại`ba`chức vụ. Trao đổi đầu tiên`s[x]`với`t[x]`. Điều này thay đổi vị trí`x`từ`ab`vào trong`ba`. Bây giờ cả hai`x`Và`y`là`ba`, do đó, trao đổi thứ hai giữa`s[x]`Và`t[y]`sửa cả hai. 

Giới hạn dưới của số lượng thao tác cũng đơn giản. Một thao tác có thể khắc phục tối đa hai vị trí hiện không khớp, do đó, việc ghép hai vị trí không khớp bằng nhau cần ít nhất một thao tác và là tối ưu. Khi một`ab`và một`ba`còn lại, một thao tác không thể giải quyết được cả hai, vì hướng của chúng trái ngược nhau. Do đó, việc xây dựng hai thao tác ở trên là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm đối tác nhiều lần |`O(n²)`|`O(n)`| Quá chậm | 
| Lưu trữ các vị trí không khớp và ghép chúng |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mọi vị trí từ trái sang phải. Nếu như`s[i] == t[i]`, bỏ qua nó. Nếu như`s[i] == 'a'`Và`t[i] == 'b'`, nối thêm`i`đến`ab`danh sách. Nếu không thì nối thêm`i`đến`ba`danh sách. Điều này tách biệt mọi vị trí có vấn đề theo hai hướng duy nhất có thể có. 
2. Kiểm tra tính chẵn lẻ của hai danh sách. Tổng số`a`các ký tự trong cả hai chuỗi được giữ nguyên sau mỗi lần hoán đổi và một cặp bằng nhau cuối cùng chứa 0 hoặc hai`a`nhân vật. Như vậy tổng số`a`các ký tự phải chẵn. Tương tự, tổng số điểm không khớp phải là số chẵn. Vì hai số đếm không khớp có cùng tính chẵn lẻ bất cứ khi nào tổng của chúng là số chẵn, nên có thể loại bỏ khi`len(ab) + len(ba)`thật kỳ quặc. 
3. Ghép các vị trí liên tiếp trong`ab`. Đối với mỗi cặp`ab[2j]`Và`ab[2j + 1]`, xuất ra một trao đổi giữa`s[ab[2j]]`Và`t[ab[2j + 1]]`. Một thao tác sẽ cố định cả hai vị trí nên điều này là tối ưu cho cặp đó. 
4. Ghép các vị trí liên tiếp trong`ba`theo cách hoàn toàn giống nhau. Đối với các vị trí`x = ba[2j]`Và`y = ba[2j + 1]`, tráo đổi`s[x]`với`t[y]`. Cả hai điểm không khớp đều trở thành vị trí bằng nhau sau khi thực hiện thao tác. 
5. Nếu cả hai danh sách không khớp nhau đều có độ dài lẻ thì mỗi danh sách vẫn còn một vị trí. Hãy để những vị trí đó được`x = ab[-1]`Và`y = ba[-1]`. Đầu ra đầu tiên`(x, x)`, hoán đổi hai ký tự ở vị trí`x`và thay đổi loại của nó từ`ab`ĐẾN`ba`. Sau đó xuất ra`(x, y)`. Hai người còn lại`ba`sự không phù hợp bây giờ được ghép nối và trở thành bằng nhau. 
6. Chuyển đổi mọi chỉ mục dựa trên số 0 được lưu trữ thành chỉ mục dựa trên một khi in. Số lượng đầu ra chỉ đơn giản là độ dài của danh sách thao tác được tạo. 

### Tại sao nó hoạt động 

Sau lần quét đầu tiên, mọi thông tin không khớp đều thuộc về chính xác một trong hai danh sách. Việc hoán đổi giữa hai vị trí có cùng loại không khớp sẽ sửa cả hai vị trí đó mà không ảnh hưởng đến bất kỳ vị trí nào đã được cố định. Do đó, tất cả các phần có kích thước chẵn của hai danh sách có thể được loại bỏ một cách tối ưu theo cặp. 

Nếu cả hai danh sách đều là số lẻ thì mỗi danh sách còn lại chính xác một vị trí. Hoán đổi bổ sung đầu tiên sẽ thay đổi phần còn lại`ab`không khớp thành một`ba`không khớp, sau đó hai điểm không khớp còn lại có cùng loại và có thể được sửa bằng một lần hoán đổi nữa. Nếu tổng số không khớp là số lẻ thì không có giải pháp nào tồn tại vì mọi thao tác đều bảo toàn điều kiện chẵn lẻ cần thiết để hai chuỗi trở nên giống hệt nhau. 

Mọi thao tác được sử dụng trên hai loại không khớp bằng nhau sẽ sửa hai lỗi không khớp, đây là mức tối đa có thể. Ngoại lệ duy nhất không thể tránh khỏi là cặp đối diện cuối cùng, trong đó cần có hai thao tác. Do đó chuỗi được xây dựng có độ dài tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()
    t = input().strip()

    ab = []
    ba = []

    for i in range(n):
        if s[i] == t[i]:
            continue
        if s[i] == 'a':
            ab.append(i)
        else:
            ba.append(i)

    if (len(ab) + len(ba)) % 2:
        print(-1)
        return

    operations = []

    for i in range(0, len(ab) - 1, 2):
        operations.append((ab[i] + 1, ab[i + 1] + 1))

    for i in range(0, len(ba) - 1, 2):
        operations.append((ba[i] + 1, ba[i + 1] + 1))

    if len(ab) % 2 == 1:
        x = ab[-1] + 1
        y = ba[-1] + 1
        operations.append((x, x))
        operations.append((x, y))

    out = [str(len(operations))]
    out.extend(f"{x} {y}" for x, y in operations)
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên chỉ ghi lại những điểm không khớp, do đó các vị trí bằng nhau không bao giờ được nhập vào logic sau. Điều này rất hữu ích vì mọi thao tác được tạo sau đó có thể được lý giải hoàn toàn dựa trên danh sách không khớp. 

Việc kiểm tra tính chẵn lẻ được thực hiện trước khi xây dựng các hoạt động. Vì mỗi thao tác chỉ trao đổi các ký tự hiện có nên nó không thể thay đổi tổng số ký tự`a`các ký tự trong hai chuỗi. Trạng thái cuối cùng có các chuỗi bằng nhau nhất thiết phải có tổng số chẵn`a`các ký tự, do đó số lượng ký tự không khớp kỳ lạ chứng tỏ là không thể. 

Hai vòng ghép nối sử dụng`range(0, len(list) - 1, 2)`. Giới hạn trên có chủ ý dừng trước phần tử cuối cùng khi danh sách có độ dài lẻ. Yếu tố cuối cùng đó được dành riêng cho việc xây dựng hai hoạt động đặc biệt. 

Trường hợp đặc biệt sử dụng cùng một chỉ mục hai lần trong thao tác đầu tiên, chẳng hạn như`(x, x)`. Điều này là hợp pháp vì chỉ mục đầu tiên thuộc về`s`và thứ hai thuộc về`t`, do đó phép toán hoán đổi`s[x]`Và`t[x]`. Nó không phải là không hoạt động. Đối với một`ab`không khớp, nó thay đổi vị trí thành`ba`. 

Tất cả các chỉ mục bên trong đều dựa trên 0 vì các chuỗi Python sử dụng lập chỉ mục dựa trên 0. Chúng chỉ được tăng thêm một khi được lưu trữ ở đầu ra, khớp với các vị trí dựa trên một mà bài toán yêu cầu. 

Không có đột biến`s`hoặc`t`là cần thiết. Các hoạt động bắt nguồn từ phân loại không khớp ban đầu và mỗi hoạt động được tạo ra được biết về mặt toán học để cố định các vị trí dự định của nó. Điều này cũng tránh được những thay đổi ngẫu nhiên đối với các quyết định phân loại sau này. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
4
abab
aabb
```Phân loại không phù hợp là: 

| Chỉ mục |`s[i]`|`t[i]`| Loại |`ab`danh sách |`ba`danh sách | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một | một | bằng | [] | [] | 
| 1 | b | một |`ba`| [] | [1] | 
| 2 | một | b |`ab`| [2] | [1] | 
| 3 | b | b | bằng | [2] | [1] | 

Có một cái`ab`và một`ba`, vì vậy cả hai danh sách đều là số lẻ. Thuật toán sử dụng trường hợp đặc biệt: 

| Bước | Hoạt động, dựa trên số không | Mục đích | 
| --- | --- | --- | 
| 1 |`(2, 2)`| Thay đổi vị trí 2 từ`ab`ĐẾN`ba`| 
| 2 |`(2, 1)`| Ghép đôi cả hai`ba`không khớp | 

Được chuyển đổi sang lập chỉ mục một cơ sở, đầu ra là:```
2
3 3
3 2
```Thao tác đầu tiên thay đổi chuỗi từ`abab`Và`aabb`ĐẾN`abbb`Và`aaab`. Thao tác thứ hai làm cho cả hai chuỗi`abab`. Hai thao tác này là cần thiết vì sự không khớp ban đầu có hướng ngược nhau. 

### Mẫu 2 

Đầu vào là:```
1
a
b
```Dấu vết là: 

| Chỉ mục |`s[i]`|`t[i]`| Loại |`ab`|`ba`| Tổng số không khớp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | một | b |`ab`| [0] | [] | 1 | 

Số đếm không khớp là số lẻ nên thuật toán sẽ in ngay:```
-1
```Không thể có một chuỗi hợp lệ vì hai ký tự duy nhất chứa chính xác một`a`, trong khi hai chuỗi bằng nhau sẽ chứa số chẵn`a`các ký tự trên cả hai chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi vị trí được quét một lần và mỗi vị trí không khớp được xử lý một lần khi các hoạt động được tạo. | 
| Không gian |`O(n)`| Hai danh sách không khớp và danh sách thao tác kết quả chứa tối đa`O(n)`mục nhập. | 

Với`n <= 2 * 10^5`, thuật toán chỉ thực hiện một vài lần tuyến tính trên chuỗi. Công việc trong trường hợp xấu nhất của nó tỷ lệ thuận với khoảng`n`, thay vì hàng chục tỷ lượt kiểm tra được tạo ra bởi tìm kiếm bậc hai, do đó, nó vừa vặn thoải mái với giới hạn 2 giây. Việc sử dụng bộ nhớ cũng tuyến tính và nằm trong khoảng 256 MB. 

## Trường hợp thử nghiệm 

Bởi vì các trình tự vận hành tối ưu không phải là duy nhất, nên một bộ khai thác thử nghiệm mạnh mẽ không nên so sánh từng ký tự đầu ra thành công với đầu ra mẫu. Thay vào đó, cần xác minh rằng đầu ra có số lượng thao tác tối thiểu chính xác và việc áp dụng các thao tác đó thực sự tạo ra các chuỗi bằng nhau.```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()
    t = input().strip()

    ab = []
    ba = []

    for i in range(n):
        if s[i] == t[i]:
            continue
        if s[i] == 'a':
            ab.append(i)
        else:
            ba.append(i)

    if (len(ab) + len(ba)) % 2:
        print(-1)
        return

    operations = []

    for i in range(0, len(ab) - 1, 2):
        operations.append((ab[i] + 1, ab[i + 1] + 1))

    for i in range(0, len(ba) - 1, 2):
        operations.append((ba[i] + 1, ba[i + 1] + 1))

    if len(ab) % 2 == 1:
        x = ab[-1] + 1
        y = ba[-1] + 1
        operations.append((x, x))
        operations.append((x, y))

    print(len(operations))
    for x, y in operations:
        print(x, y)

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

def validate(inp: str, out: str):
    data = inp.strip().splitlines()
    n = int(data[0])
    original_s = data[1]
    original_t = data[2]

    lines = out.strip().splitlines()

    if not lines:
        raise AssertionError("empty output")

    if lines[0].strip() == "-1":
        mismatches = sum(a != b for a, b in zip(original_s, original_t))
        assert mismatches % 2 == 1, "reported impossible for a solvable case"
        return

    k = int(lines[0])
    assert len(lines) == k + 1, "wrong number of operation lines"

    ab = []
    ba = []

    for i in range(n):
        if original_s[i] == original_t[i]:
            continue
        if original_s[i] == 'a':
            ab.append(i)
        else:
            ba.append(i)

    expected = len(ab) // 2 + len(ba) // 2
    if len(ab) % 2:
        expected += 2

    assert k == expected, f"not minimum: got {k}, expected {expected}"

    s = list(original_s)
    t = list(original_t)

    for line in lines[1:]:
        x, y = map(int, line.split())
        assert 1 <= x <= n
        assert 1 <= y <= n
        x -= 1
        y -= 1
        s[x], t[y] = t[y], s[x]

    assert s == t, "operations did not make strings equal"

# Provided samples.
sample1 = """4
abab
aabb
"""
validate(sample1, run(sample1))

sample2 = """1
a
b
"""
validate(sample2, run(sample2))

sample3 = """8
babbaabb
abababaa
"""
validate(sample3, run(sample3))

# Minimum-size solvable case: already equal.
case1 = """1
a
a
"""
validate(case1, run(case1))
assert run(case1).strip() == "0"

# Minimum-size impossible case: one mismatch.
case2 = """1
a
b
"""
assert run(case2).strip() == "-1"

# Opposite mismatch types. This catches the special two-operation case.
case3 = """2
ab
ba
"""
validate(case3, run(case3))

# Maximum-size input, with all positions equal.
case4 = "200000\n" + "a" * 200000 + "\n" + "a" * 200000 + "\n"
assert run(case4).strip() == "0"

# Boundary case with two equal mismatch types.
case5 = """4
aabb
bbaa
"""
validate(case5, run(case5))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a / a`|`0`| Kích thước tối thiểu và các chuỗi đã bằng nhau | 
|`1 / a / b`|`-1`| Trường hợp không thể tối thiểu và số lượng không khớp lẻ | 
|`2 / ab / ba`|`2`hoạt động | Trường hợp đặc biệt khi cả hai loại không khớp xảy ra cùng một lúc | 
|`n = 200000`, cả hai chuỗi đều`a`|`0`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 
|`4 / aabb / bbaa`|`2`hoạt động | Ghép nối nhiều điểm không khớp cùng loại | 

Trình xác thực áp dụng mọi hoán đổi được in cho các bản sao có thể thay đổi của chuỗi, sau đó kiểm tra sự bằng nhau ở cuối. Nó cũng tính toán mức tối thiểu theo lý thuyết từ số lượng không khớp. Điều này phát hiện các giải pháp tình cờ làm cho các chuỗi bằng nhau nhưng sử dụng các thao tác không cần thiết, cũng như các giải pháp in các chỉ mục không hợp lệ hoặc một chuỗi trường hợp đặc biệt không chính xác. 

## Vỏ cạnh 

Khi hai chuỗi đã bằng nhau thì không có vị trí không khớp. Ví dụ,```
3
aba
aba
```sản xuất trống`ab`Và`ba`danh sách. Cả hai vòng lặp ghép nối đều thực hiện 0 lần, trường hợp đặc biệt bị bỏ qua và câu trả lời là`0`. Không có thao tác nào là cần thiết hoặc được phép trong một câu trả lời tối ưu. 

Khi có tổng số điểm không khớp là lẻ thì câu trả lời là không thể. Vì```
1
a
b
```cái`ab`danh sách là`[0]`và`ba`danh sách trống. Tổng số là một, do đó thuật toán in`-1`trước khi cố gắng truy cập một đối tác. Đây là điều kiện chẵn lẻ gây ra bởi việc bảo toàn tổng số`a`nhân vật. 

Khi cả hai loại không khớp đều có kích thước lẻ, tổng số không khớp là chẵn, do đó, trường hợp này có thể giải được nhưng yêu cầu cấu trúc đặc biệt. Vì```
2
ab
ba
```các danh sách là`ab = [0]`Và`ba = [1]`. hoạt động`(1, 1)`thay đổi vị trí đầu tiên từ`ab`ĐẾN`ba`. hoạt động`(1, 2)`sau đó ghép đôi hai`ba`các vị trí. Chính xác hai thao tác được sử dụng và một thao tác không thể giải quyết các hướng ngược lại ban đầu. 

Khi danh sách không khớp có kích thước chẵn lớn hơn 0, mọi vị trí trong danh sách đó có thể được xử lý độc lập theo cặp. Vì```
4
aabb
bbaa
```sự không phù hợp là`ab`tại các vị trí`1`Và`2`, Và`ba`tại các vị trí`3`Và`4`. Thuật toán hoán đổi`(1, 2)`cho cặp đầu tiên và`(3, 4)`cho cặp thứ hai. Mỗi thao tác sẽ sửa hai điểm không khớp, tối thiểu là hai thao tác. 

Ranh giới lập chỉ mục một cơ sở cũng được xử lý rõ ràng. Trong nội bộ, vị trí`0`đại diện cho ký tự đầu tiên, nhưng mọi thao tác được lưu trữ đều thêm một ký tự trước khi in. Do đó, sự không khớp ở ký tự đầu tiên được in bằng chỉ mục`1`, trong khi không khớp ở ký tự cuối cùng của độ dài-`n`chuỗi được in bằng chỉ mục`n`. Khai thác thử nghiệm sẽ kiểm tra cả hai giới hạn cho mọi hoạt động được tạo.

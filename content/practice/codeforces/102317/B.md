---
title: "CF 102317B - Âm vị Palindromes"
description: "Một bảng màu bình thường đọc giống hệt nhau từ cả hai hướng. Ở đây, hai chữ cái khác nhau cũng có thể biểu thị cùng một âm thanh."
date: "2026-08-16T18:44:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102317
codeforces_index: "B"
codeforces_contest_name: "UCF Locals 2016"
rating: 0
weight: 102317
solve_time_s: 179
verified: true
draft: false
---

[CF 102317B - Âm vị Palindromes](https://codeforces.com/problemset/problem/102317/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một bảng màu bình thường đọc giống hệt nhau từ cả hai hướng. Ở đây, hai chữ cái khác nhau cũng có thể biểu thị cùng một âm thanh. Ví dụ, nếu`c`Và`k`được khai báo tương đương thì`cak`là một âm vị nhạt vì các chữ cái bên ngoài của nó,`c`Và`k`, âm thanh giống nhau, trong khi phần giữa`a`khớp với chính nó. 

Đầu vào chứa một số trường hợp thử nghiệm độc lập. Đối với mỗi trường hợp thử nghiệm, trước tiên chúng tôi nhận được một tập hợp các cặp chữ cái viết thường rời nhau. Mỗi cặp nói rằng hai chữ cái của nó có âm thanh giống nhau. Một chữ cái thuộc về nhiều nhất một cặp như vậy, do đó quan hệ tương đương đặc biệt đơn giản. Sau các cặp, chúng tôi nhận được một số chuỗi. Đối với mỗi chuỗi, chúng ta phải quyết định xem mọi ký tự có cùng âm thanh với ký tự ở vị trí đối xứng từ đầu bên kia hay không. Đầu ra phải tái tạo chuỗi gốc theo sau là`YES`hoặc`NO`, với tiêu đề và một dòng trống phân cách các trường hợp thử nghiệm. 

Số cặp tương đương nhiều nhất là 13, bao gồm tối đa 26 chữ cái. Mỗi chuỗi thử nghiệm có độ dài tối đa là 50 và có tối đa 100 chuỗi trong một trường hợp thử nghiệm. Các giới hạn này đủ nhỏ để thậm chí việc kiểm tra tất cả 13 cặp cho mỗi cặp ký tự đối xứng cũng đủ nhanh. Việc triển khai tối ưu có thể hoạt động tốt hơn bằng cách gán cho mỗi chữ cái một đại diện cho lớp âm thanh của nó, giảm mọi so sánh về thời gian không đổi. 

Một chuỗi có độ dài bằng 1 luôn là một âm vị palindrome. Ví dụ: với bất kỳ cặp âm thanh nào, đầu vào```
1
1
c k
1
a
```sản xuất```
Test case #1:
a YES
```Không có đặc điểm trái ngược nào để không đồng ý, vì vậy việc triển khai bất cẩn đòi hỏi ít nhất một cặp vị trí có thể từ chối nó một cách không chính xác. 

Trường hợp cạnh thứ hai xảy ra khi các ký tự khác nhau nhưng tương đương. Với`c`Và`k`được khai báo tương đương, chuỗi`ck`là hợp lệ:```
1
1
c k
1
ck
```Kết quả đúng là```
Test case #1:
ck YES
```So sánh các ký tự thô thay vì âm thanh của chúng sẽ tạo ra kết quả không chính xác`NO`. 

Tình huống ngược lại cũng có vấn đề. Với`c`Và`k`tương đương,`cab`không phải là một âm vị đối xứng vì các ký tự bên ngoài là`c`Và`b`, có âm thanh khác nhau:```
1
1
c k
1
cab
```Kết quả đúng là```
Test case #1:
cab NO
```Việc triển khai bất cẩn chỉ kiểm tra xem chuỗi có chứa các chữ cái tương đương đã biết hay không, thay vì kiểm tra các vị trí tương ứng, có thể chấp nhận chuỗi đó một cách không chính xác. 

## Phương pháp tiếp cận 

Phương pháp brute-force trực tiếp lưu trữ các cặp âm thanh tương đương và đối với mỗi dây, kiểm tra các vị trí từ hai đầu về phía tâm. Khi hai ký tự bằng nhau, cặp này có hiệu lực ngay lập tức. Khi chúng khác nhau, chúng tôi quét tất cả các cặp âm thanh được khai báo để xem liệu hai ký tự đó có tạo thành một trong các cặp hay không. Nếu không có cặp nào trùng khớp thì chuỗi đó không phải là một âm vị đối xứng. Điều này đúng vì một chuỗi là một âm vị đối xứng chính xác khi mọi cặp vị trí đối xứng đều có cùng một âm thanh. 

Trong trường hợp xấu nhất, một trường hợp kiểm thử chứa 100 chuỗi có độ dài 50. Có tối đa 25 phép so sánh vị trí đối xứng trên mỗi chuỗi và mỗi phép so sánh không thành công có thể kiểm tra tất cả 13 cặp âm thanh. Điều đó mang lại nhiều nhất`100 * 25 * 13 = 32,500`kiểm tra cặp cho một trường hợp thử nghiệm. Giá trị này rất nhỏ trong giới hạn nhất định, vì vậy phương pháp vũ phu thực sự đủ nhanh. 

Giải pháp sạch hơn xuất phát từ việc lưu ý rằng mối quan hệ âm thanh được cố định cho toàn bộ trường hợp thử nghiệm. Thay vì tìm kiếm liên tục trong danh sách các cặp, hãy gán cho mỗi chữ cái một đại diện chuẩn. Nếu như`c`Và`k`nghe có vẻ giống nhau, cả hai đều có thể ánh xạ tới`c`. Những lá thư không có bản đồ đối tác cho chính họ. Khi đó hai chữ cái có âm giống nhau khi đại diện của chúng bằng nhau. 

Điều này biến mỗi so sánh đối xứng thành tra cứu mảng theo thời gian không đổi. Thuật toán vẫn quét từng chuỗi một lần nhưng việc tìm kiếm bên trong lên tới 13 cặp sẽ biến mất. Quan sát quan trọng là thông tin âm thanh là tĩnh, vì vậy chúng ta nên xử lý trước thông tin đó một lần thay vì khám phá lại thông tin đó cho mỗi lần so sánh ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · L · p) | O(p) | Được chấp nhận theo giới hạn nhất định | 
| Tối ưu | O(q · L + p) | O(26) | Đã chấp nhận | 

Đây`q`là số chuỗi,`L`là độ dài chuỗi tối đa và`p`là số cặp âm tương đương. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case và xử lý từng test case một cách độc lập. Các mối quan hệ đúng đắn từ một ca kiểm thử không được ảnh hưởng đến bất kỳ ca kiểm thử nào sau này. 
2. Tạo ánh xạ cho tất cả 26 chữ cái viết thường, ban đầu ánh xạ từng chữ cái vào chính nó. Điều này thể hiện quy tắc mặc định là một chữ cái luôn phát âm giống chính nó. 
3. Với mỗi cặp được khai báo`(a, b)`, chỉ định cùng một đại diện cho cả hai chữ cái. Vì không có chữ cái nào xuất hiện trong nhiều hơn một cặp nên chúng ta chỉ cần chọn`a`với tư cách là người đại diện và thiết lập cả hai`a`Và`b`ĐẾN`a`. 
4. Đọc từng chuỗi và so sánh các ký tự của nó một cách đối xứng, sử dụng các chỉ số`left`Và`right`. Bắt đầu với ký tự đầu tiên và cuối cùng và di chuyển cả hai chỉ số về phía giữa. 
5. Với mỗi cặp đối xứng, hãy so sánh`representative[s[left]]`với`representative[s[right]]`. Nếu khác nhau thì hai ký tự có âm thanh khác nhau nên toàn bộ chuỗi ngay lập tức bị lỗi. 
6. Nếu tất cả các cặp đối xứng có đại diện bằng nhau, hãy in chuỗi gốc theo sau bởi`YES`. Nếu tìm thấy sự không khớp, hãy in chuỗi gốc theo sau`NO`. 

### Tại sao nó hoạt động 

Điều bất biến là hai chữ cái có cùng một cách thể hiện chính xác khi chúng có cùng một âm thanh. Ban đầu, mỗi chữ cái đại diện cho chính nó và với mỗi cặp tương đương được khai báo, cả hai chữ cái đều được gán cùng một đại diện. Bởi vì mỗi chữ cái thuộc về nhiều nhất một cặp nên không thể xảy ra xung đột phép gán. 

Đối với mỗi cặp vị trí đối xứng trong một chuỗi, thuật toán sẽ kiểm tra tính bằng nhau của các đại diện này. Bình đẳng có nghĩa là hai ký tự phát âm giống nhau, trong khi bất bình đẳng có nghĩa là chúng không giống nhau. Một bảng màu âm vị được xác định chính xác bằng cách có các âm thanh bằng nhau ở mọi cặp đối xứng, do đó việc chấp nhận chính xác khi tất cả các so sánh như vậy thành công là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    output = []

    for case_no in range(1, t + 1):
        p = int(input())

        representative = list(range(26))

        for _ in range(p):
            a, b = input().split()
            x = ord(a) - ord('a')
            y = ord(b) - ord('a')

            representative[y] = x
            representative[x] = x

        q = int(input())

        output.append(f"Test case #{case_no}:")

        for _ in range(q):
            s = input().strip()

            left = 0
            right = len(s) - 1
            ok = True

            while left < right:
                x = representative[ord(s[left]) - ord('a')]
                y = representative[ord(s[right]) - ord('a')]

                if x != y:
                    ok = False
                    break

                left += 1
                right -= 1

            output.append(f"{s} {'YES' if ok else 'NO'}")

        output.append("")

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```các`representative`mảng có 26 mục, một mục cho mỗi chữ cái viết thường. Việc giữ các chỉ số nguyên thay vì chuỗi làm cho việc so sánh âm thanh trở thành một phép tra cứu mảng đơn giản. 

Khi một cặp như`c k`được đọc,`c`trở thành đại diện của cả hai chữ cái. Sự so sánh giữa`c`Và`k`do đó trở thành phép so sánh giữa cùng một số nguyên, do đó nó thành công ngay cả khi bản thân các ký tự khác nhau. 

Vòng lặp hai con trỏ chỉ cần kiểm tra nửa đầu của chuỗi. Một lần`left >= right`, mọi cặp đối xứng đã được kiểm tra. Đối với các chuỗi có độ dài lẻ, ký tự ở giữa không bao giờ được so sánh với bất kỳ thứ gì, điều này đúng vì một ký tự luôn khớp với chính nó. 

Mã dừng ở lần không khớp đầu tiên vì các vị trí sau không thể sửa được cặp đối xứng bị lỗi. Chuỗi gốc được lưu trữ không thay đổi, do đó, đầu ra được yêu cầu có thể tái tạo chính xác chuỗi đó. 

Đầu ra được tích lũy thành một danh sách và được ghi một lần vào cuối. Điều này tránh việc ghi lặp lại và cũng giúp dễ dàng đặt dòng trống cần thiết sau mỗi trường hợp kiểm thử. 

## Ví dụ đã hoạt động 

Đầu vào mẫu chính thức chứa hai trường hợp thử nghiệm. 

### Mẫu 1```
1
1
c k
6
a
cac
ck
cab
kaak
ckckkcck
```Việc lập bản đồ là`c -> c`,`k -> c`và mọi chữ cái khác ánh xạ tới chính nó. 

Vì`cac`, các ký tự bên ngoài đều được biểu thị bằng`c`và ký tự ở giữa không liên quan. 

| Chuỗi | Trái | Đúng | Âm thanh trái | Đúng âm thanh | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
|`a`| 0 | 0 | | | CÓ | 
|`cac`| 0 | 2 | c | c | tiếp tục | 
|`cac`| 1 | 1 | | | CÓ | 
|`ck`| 0 | 1 | c | c | CÓ | 
|`cab`| 0 | 2 | c | b | KHÔNG | 
|`kaak`| 0 | 3 | c | c | tiếp tục | 
|`kaak`| 1 | 2 | một | một | CÓ | 
|`ckckkcck`| 0 | 7 | c | c | tiếp tục | 
|`ckckkcck`| 1 | 6 | c | c | tiếp tục | 
|`ckckkcck`| 2 | 5 | c | c | tiếp tục | 
|`ckckkcck`| 3 | 4 | c | c | tiếp tục | 
|`ckckkcck`| 4 | 3 | | | CÓ | 

Kết quả đầu ra là:```
Test case #1:
a YES
cac YES
ck YES
cab NO
kaak YES
ckckkcck YES
```Dấu vết cho thấy tại sao sự bình đẳng của ký tự thô là không đủ. TRONG`ck`, các ký tự khác nhau, nhưng đại diện của chúng bằng nhau. 

### Mẫu 2```
1
2
a z
x s
5
abbbz
asxz
cx
sxxabzxss
ks
```Đây`a`Và`z`chia sẻ một đại diện, cũng như`x`Và`s`. 

| Chuỗi | Cặp đối xứng | Âm thanh trái | Đúng âm thanh | Kết quả | 
| --- | --- | --- | --- | --- | 
|`abbbz`|`a`,`z`| một | một | tiếp tục | 
|`abbbz`|`b`,`b`| b | b | CÓ | 
|`asxz`|`a`,`z`| một | một | tiếp tục | 
|`asxz`|`s`,`x`| x | x | CÓ | 
|`cx`|`c`,`x`| c | x | KHÔNG | 
|`sxxabzxss`|`s`,`s`| x | x | tiếp tục | 
|`sxxabzxss`|`x`,`s`| x | x | tiếp tục | 
|`sxxabzxss`|`x`,`x`| x | x | tiếp tục | 
|`sxxabzxss`|`a`,`z`| một | một | tiếp tục | 
|`sxxabzxss`|`b`,`b`| b | b | CÓ | 
|`ks`|`k`,`s`| k | x | KHÔNG | 

Kết quả đầu ra là:```
Test case #1:
abbbz YES
asxz YES
cx NO
sxxabzxss YES
ks NO
```Ví dụ này thực hiện cả hai hướng của các cặp tương đương. Ánh xạ đại diện làm cho`x`Và`s`có thể hoán đổi cho nhau mà không cần tìm kiếm danh sách cặp ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(p + q · L) | Việc xây dựng ánh xạ mất O(p) và mọi chuỗi được quét từ cả hai đầu trong thời gian O(L) | 
| Không gian | O(26) | Ánh xạ âm thanh chứa một mục nhập cho mỗi chữ cái viết thường, ngoài bộ đệm đầu ra | 

Với`p <= 13`,`q <= 100`, Và`L <= 50`, khối lượng công việc thực tế là rất nhỏ. Ngay cả giải pháp brute-force cũng chỉ thực hiện 32.500 lần kiểm tra cặp âm thanh trong trường hợp thử nghiệm đơn lẻ lớn nhất, trong khi ánh xạ đại diện làm giảm điều này hơn nữa xuống còn tối đa 2.500 so sánh ký tự đối xứng. Giải pháp phù hợp thoải mái với giới hạn 1 giây và 256 MB được báo cáo cho vấn đề cuộc thi. 

## Trường hợp thử nghiệm 

Các mẫu chính thức được đưa ra trong tài liệu cuộc thi.```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    output = []

    for case_no in range(1, t + 1):
        p = int(input())
        representative = list(range(26))

        for _ in range(p):
            a, b = input().split()
            x = ord(a) - ord('a')
            y = ord(b) - ord('a')
            representative[y] = x
            representative[x] = x

        q = int(input())
        output.append(f"Test case #{case_no}:")

        for _ in range(q):
            s = input().strip()
            left, right = 0, len(s) - 1
            ok = True

            while left < right:
                if representative[ord(s[left]) - 97] != representative[ord(s[right]) - 97]:
                    ok = False
                    break
                left += 1
                right -= 1

            output.append(f"{s} {'YES' if ok else 'NO'}")

        output.append("")

    return "\n".join(output)

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        return solve()
    finally:
        sys.stdin = old_stdin
        input = old_input

# provided sample 1
sample1 = """1
1
c k
6
a
cac
ck
cab
kaak
ckckkcck
"""

expected1 = """Test case #1:
a YES
cac YES
ck YES
cab NO
kaak YES
ckckkcck YES
"""

assert run(sample1) == expected1, "sample 1"

# provided sample 2
sample2 = """1
2
a z
x s
5
abbbz
asxz
cx
sxxabzxss
ks
"""

expected2 = """Test case #1:
abbbz YES
asxz YES
cx NO
sxxabzxss YES
ks NO
"""

assert run(sample2) == expected2, "sample 2"

# Minimum-size input, a single character.
assert run("""1
1
a b
1
z
""") == """Test case #1:
z YES
""", "single-character string"

# All characters are equivalent in the only declared pair.
assert run("""1
1
a z
4
az
za
aaaa
azaa
""") == """Test case #1:
az YES
za YES
aaaa YES
azaa YES
""", "equivalent outer characters"

# Boundary case where the first comparison fails immediately.
assert run("""1
1
c k
3
cab
babc
kc
""") == """Test case #1:
cab NO
babc NO
kc YES
""", "early mismatch and equivalent pair"

# Maximum-size string and all-equal values.
large = "a" * 50
assert run(f"""1
1
b c
1
{large}
""") == f"""Test case #1:
{large} YES
""", "length 50 all-equal string"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ký tự đơn`z`|`z YES`| Độ dài chuỗi tối thiểu và`left < right`ranh giới | 
|`az`,`za`,`aaaa`,`azaa`với`a z`tương đương | Tất cả`YES`| Các chữ cái khác nhau có cùng âm thanh và các ký tự hoàn toàn bằng nhau | 
|`cab`,`babc`,`kc`với`c k`tương đương |`NO`,`NO`,`YES`| Sự không khớp ngay lập tức và các ký tự ranh giới tương đương | 
| 50 bản sao của`a`|`YES`| Độ dài chuỗi tối đa và các ký tự giống hệt nhau lặp lại | 

## Vỏ cạnh 

Đối với chuỗi một ký tự, vòng lặp không thực thi vì`left == right`. Thuật toán chấp nhận chuỗi ngay lập tức. Ví dụ,```
1
1
c k
1
a
```sản xuất`a YES`. Đây chính xác là định nghĩa palindrome ở cấp độ âm vị vì chỉ có một âm thanh để so sánh. 

Đối với các ký tự khác nhau nhưng tương đương, ánh xạ đại diện sẽ xử lý trường hợp này mà không cần logic đặc biệt. Với```
1
1
c k
1
ck
```bản đồ chứa`representative[c] = c`Và`representative[k] = c`. Do đó, sự so sánh duy nhất là`c == c`, vì vậy đầu ra là`ck YES`. 

Đối với sự không khớp thực sự, thuật toán sẽ dừng ngay khi tìm thấy. Với```
1
1
c k
1
cab
```sự so sánh đầu tiên là giữa`c`Và`b`. Đại diện của họ là`c`Và`b`, do đó tập hợp thuật toán`ok`thành sai và in`cab NO`. Nó không cần phải kiểm tra ký tự ở giữa. 

Đối với chuỗi có độ dài chẵn, mỗi ký tự thuộc về một cặp đối xứng. Với```
1
1
c k
1
kaak
```sự so sánh đầu tiên là`k`chống lại`k`, đại diện bởi`c`chống lại`c`, và thứ hai là`a`chống lại`a`. Cả hai đều thành công, đưa ra`kaak YES`. 

Đối với một chuỗi có độ dài lẻ, ký tự trung tâm không có đối trọng nào có thể làm mất hiệu lực bảng màu. TRONG`cac`, bên ngoài`c`các ký tự khớp nhau và ở giữa`a`không bao giờ được so sánh. Thuật toán trả về chính xác`cac YES`. 

Cuối cùng, các cặp âm thanh độc lập. Với`a z`Và`x s`, so sánh giữa`a`Và`z`thành công trong khi so sánh giữa`a`Và`x`thất bại. Ánh xạ mã hóa chính xác các mối quan hệ độc lập này, do đó, một ký tự không thể vô tình kế thừa âm thanh của một cặp không liên quan.

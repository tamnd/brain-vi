---
title: "CF 102348G - Hoán đổi chữ cái"
description: "Chúng ta có hai chuỗi s và t có cùng độ dài và mọi ký tự đều là a hoặc b. Trong một thao tác, chúng ta có thể chọn bất kỳ vị trí nào trong s và bất kỳ vị trí nào trong t, sau đó hoán đổi hai ký tự."
date: "2026-08-17T10:41:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "G"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 405
verified: false
draft: false
---

[CF 102348G - Hoán đổi chữ cái](https://codeforces.com/problemset/problem/102348/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 45 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai chuỗi`s`Và`t`có cùng độ dài và mọi ký tự đều`a`hoặc`b`. Trong một thao tác, chúng ta có thể chọn bất kỳ vị trí nào trong`s`và bất kỳ vị trí nào trong`t`, sau đó hoán đổi hai ký tự. Mục tiêu là làm cho hai chuỗi giống nhau bằng cách sử dụng càng ít hoán đổi chuỗi chéo càng tốt. 

Đầu ra phải chứa số lần hoán đổi tối thiểu và một chuỗi các cặp chỉ mục đạt được nó. Nếu không có chuỗi nào có thể làm cho các chuỗi bằng nhau thì chúng ta in`-1`. 

Quan sát hữu ích đầu tiên là về số lượng ký tự. Một trao đổi không bao giờ thay đổi tổng số`a`các ký tự trên cả hai chuỗi. Nếu các chuỗi cuối cùng bằng nhau thì mỗi vị trí sẽ đóng góp cùng một ký tự cho cả hai chuỗi, do đó tổng số`a`các ký tự phải chẵn. Tương tự, số lượng vị trí`s[i] != t[i]`phải chẵn. Nếu nó là số lẻ thì không có chuỗi hoán đổi nào có thể hoạt động được. 

Với`n`lớn như`2 * 10^5`, một thuật toán có hành vi bậc hai hoặc hàm mũ là không phù hợp. Giới hạn hai giây có nghĩa là chúng ta nên hướng tới công việc tuyến tính hoặc gần tuyến tính, tỷ lệ gần đúng với kích thước đầu vào. Việc lưu trữ các vị trí không khớp cũng rẻ vì có thể có nhiều nhất`n`của họ. 

Có hai loại không phù hợp. Tại một vị trí mà`s[i] = a`Và`t[i] = b`, gọi nó là một`ab`không khớp. Tại một vị trí mà`s[i] = b`Và`t[i] = a`, gọi nó là`ba`không khớp. Những loại này hoạt động khác nhau khi được ghép nối và bỏ qua sự khác biệt đó là nguồn gốc chính của các giải pháp không chính xác. 

Coi như`n = 1`,`s = "a"`,`t = "b"`. Có một điểm không khớp nên tổng số điểm không khớp là số lẻ. Một giải pháp bất cẩn có thể thử hoán đổi hai ký tự ở chỉ mục`1`, nhưng thao tác đó trao đổi`a`Và`b`giữa các chuỗi và chỉ để lại các chuỗi như`"b"`Và`"a"`. Câu trả lời đúng là`-1`. 

Một trường hợp quan trọng khác là mỗi loại không khớp nhau. Ví dụ, với`s = "ab"`Và`t = "ba"`, sự không phù hợp là`ab`ở vị trí`1`Và`ba`ở vị trí`2`. Chúng không thể được sửa chữa bằng một lần hoán đổi giữa hai vị trí khác nhau. Hai thao tác cần thiết: trao đổi`s[1]`với`t[1]`, sau đó trao đổi`s[1]`với`t[2]`. Câu trả lời là`2`. Một giải pháp luôn kết hợp một`ab`với một`ba`và cho rằng một thao tác là đủ sẽ thất bại ở đây. 

Cuối cùng, khi có hai điểm không khớp cùng loại thì chỉ cần một thao tác là đủ. Vì`s = "aabb"`Và`t = "bbaa"`, vị trí`1`Và`2`cả hai đều`ab`sự không phù hợp. Hoán đổi`s[1]`với`t[2]`sửa chữa cả hai vị trí cùng một lúc. Một giải pháp xử lý độc lập mọi vị trí không khớp sẽ sử dụng quá nhiều thao tác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp có thể mô hình hóa mọi sự sắp xếp có thể có của tất cả`2n`các ký tự trong hai chuỗi dưới dạng trạng thái. Từ một tiểu bang có`n^2`có thể hoán đổi chuỗi chéo, do đó, tìm kiếm theo chiều rộng có thể khám phá các trạng thái với số lượng hoạt động ngày càng tăng và dừng khi hai chuỗi trở nên bằng nhau. Điều này đúng vì BFS tìm đường đi ngắn nhất trong biểu đồ trạng thái không có trọng số. 

Vấn đề là kích thước của biểu đồ đó. Tổng số`a`các ký tự được giữ nguyên, vì vậy nếu có`k`trong số đó có thể có`C(2n, k)`các trạng thái riêng biệt. Điều này được tối đa hóa xung quanh`k = n`, đưa ra một cách đại khái`C(2n, n)`, tăng trưởng như`4^n / sqrt(n)`. Khám phá tới`n^2`chuyển đổi từ mỗi trạng thái đưa ra thang đo trong trường hợp xấu nhất`Theta(n^2 C(2n, n))`, điều này hoàn toàn không khả thi ngay cả đối với vài chục vị trí. 

Một cách tiếp cận tham lam tập trung hơn có thể kiểm tra những điểm không khớp và cố gắng khắc phục từng điểm một. Cái nhìn sâu sắc quan trọng là những điểm không khớp có thể được ghép nối. Nếu hai vị trí có cùng loại không khớp, một hoán đổi chuỗi chéo sẽ sửa cả hai. Ví dụ, hai`ab`vị trí có thể được ghép nối bằng cách hoán đổi`a`từ vị trí đầu tiên với`b`từ thứ hai. Lập luận tương tự có tác dụng đối với hai`ba`các vị trí. 

Sau nhiều lần ghép các loại bằng nhau, có thể có nhiều nhất một`ab`không khớp và nhiều nhất là một`ba`trái không khớp. Nếu không còn lại, chúng tôi đã hoàn thành. Nếu chỉ còn lại một, thì tổng số trường hợp không khớp là số lẻ, do đó trường hợp này là không thể. Nếu cả hai vẫn còn, họ yêu cầu hai thao tác. Thao tác đầu tiên hoán đổi hai ký tự ở một trong các vị trí còn lại với chính nó trên các chuỗi, thay đổi loại không khớp ở đó. Hai điểm không khớp còn lại sau đó trở thành cùng loại và có thể được giải quyết bằng cách hoán đổi thứ hai. 

Do đó, toàn bộ vấn đề giảm xuống còn việc thu thập các vị trí không khớp, ghép các loại bằng nhau và xử lý riêng hai phần còn lại có thể có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`Theta(n^2 C(2n,n))`trong trường hợp xấu nhất | Hàm mũ | Quá chậm | 
| Tối ưu |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét tất cả các vị trí từ trái sang phải. Nếu như`s[i] == t[i]`, vị trí đó đã đồng ý rồi và không cần thao tác gì cả. Nếu không, hãy thêm chỉ mục dựa trên 1 của nó vào`ab`hoặc`ba`, tùy thuộc vào việc cặp đó có`(a, b)`hoặc`(b, a)`. 

Việc tách biệt hai loại không khớp này là điều giúp chúng tôi nhận biết khi nào một thao tác có thể giải quyết được hai vị trí. 
2. Nếu tổng số không khớp,`len(ab) + len(ba)`, lẻ, in`-1`. 

Mọi thao tác đều bảo toàn tổng số`a`các ký tự, trong khi các chuỗi cuối cùng bằng nhau chứa số chẵn`a`tổng số nhân vật. Do đó điều kiện chẵn lẻ là cần thiết. Đối với chuỗi nhị phân thì cũng đủ. 
3. Ghép nối liên tiếp`ab`sự không phù hợp. Đối với mỗi cặp`ab[i]`Và`ab[i + 1]`, thêm thao tác`(ab[i], ab[i + 1])`. 

Ở vị trí đầu tiên,`s`chứa`a`Và`t`chứa`b`. Ở vị trí thứ hai,`s`chứa`a`Và`t`chứa`b`. Hoán đổi`s[ab[i]]`với`t[ab[i + 1]]`thay đổi cả hai vị trí thành`(b, b)`hoặc, tùy thuộc vào hướng của cặp, sửa cả hai điểm không khớp cùng một lúc. Lý do tương tự áp dụng cho hai`ba`sự không phù hợp. 
4. Ghép nối liên tiếp`ba`không khớp theo cách hoàn toàn giống nhau. Đối với mỗi cặp`ba[i]`Và`ba[i + 1]`, thêm vào`(ba[i], ba[i + 1])`. 

Mỗi thao tác như vậy sẽ loại bỏ hai điểm không khớp, vì vậy sau bước này chỉ có thể có nhiều nhất một điểm không khớp đối với mỗi loại. 
5. Nếu cả hai`ab`Và`ba`có một vị trí chưa ghép đôi, nói`x`Và`y`, trình diễn`(x, x)`Đầu tiên. 

Tại vị trí`x`, các nhân vật là`a`TRONG`s`Và`b`TRONG`t`. Hoán đổi hai ký tự đó sẽ thay đổi sự không khớp từ`ab`ĐẾN`ba`. Bây giờ cả hai vị trí còn lại đều có cùng kiểu không khớp. 
6. Thực hiện`(x, y)`như hoạt động thứ hai. 

Hai lỗi không khớp kiểu bằng nhau còn lại hiện đã được sửa bằng cùng một đối số ghép nối được sử dụng trước đó. 
7. Xuất ra các thao tác đã thu thập ở các bước trước. 

Mọi thao tác sẽ loại bỏ hai phần không khớp, ngoại trừ thao tác đầu tiên trong trường hợp còn lại sẽ thay đổi cách sắp xếp của chúng để thao tác thứ hai có thể loại bỏ hai phần cuối cùng. Vì mỗi thao tác được sử dụng để giảm tối đa có thể nên số lượng kết quả là tối thiểu. 

### Tại sao nó hoạt động 

Điều bất biến là các vị trí không khớp được thu thập sẽ mô tả chính xác những vị trí mà hai chuỗi vẫn không đồng nhất. Hai điểm không khớp cùng loại luôn có thể được loại bỏ bằng một thao tác, do đó, việc ghép chúng là tối ưu vì không một thao tác nào có thể khắc phục được nhiều hơn hai vị trí không khớp. 

Sau khi loại bỏ tất cả các cặp như vậy, tối đa chỉ còn lại một loại không khớp. Nếu chỉ còn lại một, số đếm không khớp là số lẻ và không thể có sự bằng nhau. Nếu còn lại hai thì nhất thiết chúng phải có loại trái ngược nhau. Hoán đổi chỉ mục tự thay đổi loại này thành loại khác và hoán đổi chỉ mục chéo cuối cùng sẽ khắc phục cả hai. Do đó, mọi cấu hình có thể giải được đều được xử lý và mọi thao tác được sử dụng theo cách đạt được số lượng tối thiểu có thể. 

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
            ab.append(i + 1)
        else:
            ba.append(i + 1)

    if (len(ab) + len(ba)) % 2:
        print(-1)
        return

    ans = []

    for i in range(0, len(ab) - 1, 2):
        ans.append((ab[i], ab[i + 1]))

    for i in range(0, len(ba) - 1, 2):
        ans.append((ba[i], ba[i + 1]))

    if len(ab) % 2 == 1:
        x = ab[-1]
        y = ba[-1]

        ans.append((x, x))
        ans.append((x, y))

    print(len(ans))
    for x, y in ans:
        print(x, y)

if __name__ == "__main__":
    solve()
```Lần quét đầu tiên phân loại mọi sự bất đồng thành chính xác một trong hai mảng không khớp. các`i + 1`việc chuyển đổi là có chủ ý vì thuật toán sử dụng nội bộ tính năng lập chỉ mục dựa trên số 0 của Python trong khi vấn đề yêu cầu các vị trí bắt đầu từ`1`. 

Các vòng ghép nối tiến lên hai. Ví dụ, nếu`ab = [2, 5, 7, 9]`, các hoạt động là`(2, 5)`Và`(7, 9)`. Vòng lặp bị ràng buộc`len(ab) - 1`ngăn truy cập phần tử thứ hai không tồn tại khi mảng có độ dài lẻ. 

Trường hợp còn sót lại là phần tinh tế. Khi`ab`có một vị trí không ghép đôi,`ba`cũng phải có một vì tổng số không khớp là số chẵn. hoạt động`(x, x)`là hợp pháp vì hai vị trí được chọn có thể có cùng chỉ số số, miễn là một vị trí thuộc về`s`và cái kia để`t`. Nó lật sự không khớp ở`x`, sau đó`(x, y)`sửa chữa cả hai vị trí. 

Không cần mô phỏng chuỗi. Chúng ta chỉ cần phân loại không khớp từ các chuỗi ban đầu vì các phép toán được xây dựng đã được suy luận về mặt đại số. Số nguyên Python cũng không có vấn đề tràn và nhiều nhất là`n`các hoạt động được sản xuất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
4
abab
aabb
```Việc phân loại không phù hợp như sau. 

| Vị trí |`s[i]`|`t[i]`| Loại không khớp | Vị trí được lưu trữ | 
| --- | --- | --- | --- | --- | 
| 1 | một | một | bằng | không | 
| 2 | b | một | ba | 2 | 
| 3 | một | b | ab | 3 | 
| 4 | b | b | bằng | không | 

Có một cái`ab`không phù hợp và một`ba`không khớp, do đó không thể ghép nối với một điểm không khớp khác cùng loại. 

| Bước |`ab`|`ba`| Hoạt động | Lý do | 
| --- | --- | --- | --- | --- | 
| Ban đầu |`[3]`|`[2]`| không | Hai loại thức ăn thừa đối diện | 
| 1 |`[3]`|`[2]`|`(3, 3)`| Chuyển đổi`ab`không khớp ở mức 3 thành`ba`| 
| 2 |`[3]`|`[2]`|`(3, 2)`| Ghép nối và loại bỏ cả hai`ba`không khớp | 

Sau đó`(3, 3)`, các dây trở thành`abbb`Và`aaab`. Hoán đổi cuối cùng`(3, 2)`làm cho cả hai chuỗi bằng nhau`abab`. Thuật toán đưa ra hai thao tác, là tối ưu vì một thao tác không thể giải quyết trực tiếp hai lỗi không khớp kiểu đối lập. 

### Mẫu 2 

Đầu vào là:```
1
a
b
```Chỉ có một vị trí duy nhất và đó là`ab`không khớp. 

| Vị trí |`s[i]`|`t[i]`|`ab`|`ba`| Tổng số không khớp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | một | b |`[1]`|`[]`| 1 | 

Tổng số điểm không khớp là số lẻ nên thuật toán trả về ngay`-1`. 

Điều này chứng tỏ tại sao việc kiểm tra tính chẵn lẻ phải diễn ra trước khi cố gắng ghép các phần còn lại. Không có sự không khớp thứ hai nào có thể hấp thụ ký tự không khớp, do đó không có chuỗi hoán đổi nào có thể tạo ra các chuỗi bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Một lần quét sẽ phân loại những điểm không khớp và các vòng lặp ghép nối sẽ xử lý từng điểm không khớp một lần. | 
| Không gian |`O(n)`| Hai mảng không khớp và câu trả lời chứa nhiều nhất`O(n)`chỉ số và hoạt động. | 

Vì`n <= 2 * 10^5`, thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi ký tự và lưu trữ một số nguyên tuyến tính. Điều này thoải mái trong giới hạn thời gian hai giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm 

Trình tự đầu ra không phải là duy nhất, do đó, việc kiểm tra sẽ xác thực các hoạt động được trả về thay vì so sánh byte văn bản đầu ra với byte. Bộ khai thác sau đây chạy thuật toán, kiểm tra xem số lượng thao tác được báo cáo có tối thiểu hay không và mô phỏng mọi thao tác để xác minh rằng các chuỗi kết quả bằng nhau.```python
import sys
import io

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    n = int(sys.stdin.readline())
    s = sys.stdin.readline().strip()
    t = sys.stdin.readline().strip()

    ab = []
    ba = []

    for i in range(n):
        if s[i] == t[i]:
            continue
        if s[i] == 'a':
            ab.append(i + 1)
        else:
            ba.append(i + 1)

    if (len(ab) + len(ba)) % 2:
        print(-1)
    else:
        ans = []

        for i in range(0, len(ab) - 1, 2):
            ans.append((ab[i], ab[i + 1]))

        for i in range(0, len(ba) - 1, 2):
            ans.append((ba[i], ba[i + 1]))

        if len(ab) % 2:
            x = ab[-1]
            y = ba[-1]
            ans.append((x, x))
            ans.append((x, y))

        print(len(ans))
        for x, y in ans:
            print(x, y)

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

def run(inp: str) -> str:
    return solve_io(inp)

def validate(inp: str):
    data = inp.strip().splitlines()
    n = int(data[0])
    s = list(data[1])
    t = list(data[2])

    out = run(inp).strip().splitlines()

    mismatch_count = sum(a != b for a, b in zip(s, t))

    if mismatch_count % 2 == 1:
        assert out == ["-1"], "expected impossible"
        return

    assert out[0] != "-1"

    k = int(out[0])
    assert k == len(out) - 1

    for line in out[1:]:
        x, y = map(int, line.split())
        assert 1 <= x <= n
        assert 1 <= y <= n
        s[x - 1], t[y - 1] = t[y - 1], s[x - 1]

    assert s == t

    ab = sum(a == 'a' and b == 'b' for a, b in zip(data[1], data[2]))
    ba = sum(a == 'b' and b == 'a' for a, b in zip(data[1], data[2]))

    expected = ab // 2 + ba // 2
    if ab % 2:
        expected += 2

    assert k == expected, f"expected {expected}, got {k}"

# Provided samples
validate("""4
abab
aabb
""")

validate("""1
a
b
""")

validate("""8
babbaabb
abababaa
""")

# Minimum size, already equal
validate("""1
a
a
""")

# Minimum size, impossible
validate("""1
b
a
""")

# Two equal-type mismatches, requiring exactly one operation
validate("""2
aa
bb
""")

# Two opposite-type mismatches, requiring exactly two operations
validate("""2
ab
ba
""")

# Larger boundary-style case with many equal-type pairs
validate("""8
aaaaaaaa
bbbbbbbb
""")

# Maximum-size input, already equal
n = 200000
validate(f"""{n}
{'a' * n}
{'a' * n}
""")
```Các trường hợp tùy chỉnh thực hiện một số chế độ lỗi khác nhau. Trường hợp đã bằng nhau sẽ kiểm tra xem các phép toán bằng 0 có được chấp nhận hay không. các`n = 1`trường hợp ký tự đối diện kiểm tra điều kiện không thể xảy ra ở kích thước đầu vào nhỏ nhất có thể. các`aa`so với`bb`trường hợp kiểm tra việc ghép nối nhiều trường hợp không khớp loại bằng nhau. các`ab`so với`ba`trường hợp kiểm tra việc xây dựng hai hoạt động đặc biệt. Trường hợp hoàn toàn không khớp lớn sẽ kiểm tra xem việc triển khai có còn tuyến tính khi`n`đạt tới mức tối đa. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a / a`|`0`| Đã bằng nhau rồi, không còn thao tác nào | 
|`1 / b / a`|`-1`| Trường hợp nhỏ nhất không thể | 
|`2 / aa / bb`|`1`hoạt động | Ghép nối hai không khớp cùng loại | 
|`2 / ab / ba`|`2`hoạt động | Đối diện các loại còn sót lại | 
|`8 / aaaaaaaa / bbbbbbbb`|`4`hoạt động | Nhiều cặp cùng loại | 
|`n = 200000`, cả hai chuỗi đều`a`|`0`| Kích thước đầu vào tối đa và quét tuyến tính | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là số lẻ không khớp. Vì```
1
a
b
```các mảng không khớp là`ab = [1]`Và`ba = []`. Kích thước tổng hợp của chúng là`1`, do đó thuật toán in`-1`trước khi xây dựng bất kỳ hoạt động nào. Điều này tránh được ý tưởng sai lầm khi cố gắng khắc phục sự không khớp duy nhất bằng cách hoán đổi vị trí`1`với chính nó. Sự trao đổi đó chỉ đơn thuần là trao đổi`a`Và`b`và để lại hai chuỗi khác nhau. 

Trường hợp cạnh thứ hai là hai trường hợp không khớp cùng loại. Coi như```
2
aa
bb
```Cả hai vị trí đều`ab`không phù hợp, vì vậy`ab = [1, 2]`. Vòng lặp ghép nối tạo ra`(1, 2)`. Hoán đổi hoạt động`s[1] = a`với`t[2] = b`, sản xuất`s = "ba"`Và`t = "ba"`. Một thao tác là đủ và rõ ràng là tối thiểu vì ban đầu các chuỗi không bằng nhau. 

Trường hợp cạnh thứ ba là một trường hợp không khớp của từng loại:```
2
ab
ba
```Đây`ab = [1]`Và`ba = [2]`. Có hai trường hợp không khớp nên có thể thực hiện được, nhưng không loại nào có cặp. Thuật toán đầu tiên thực hiện`(1, 1)`, thay đổi chuỗi từ`ab`Và`ba`ĐẾN`bb`Và`aa`. Bây giờ khái niệm còn lại không phù hợp ở vị trí`1`đã thay đổi loại, cho phép`(1, 2)`trao đổi`s[1] = b`với`t[2] = a`. Các dây trở thành`ab`Và`ab`. Cần có hai thao tác vì thao tác đầu tiên chỉ có thể thay đổi một thao tác không khớp khi hai loại còn lại khác nhau. 

Trường hợp ranh giới cuối cùng là đầu vào có kích thước tối đa trong đó các chuỗi đã bằng nhau. Với`n = 200000`và cả hai chuỗi bao gồm toàn bộ`a`, quá trình quét không gặp sự không khớp nào, cả hai mảng vẫn trống và câu trả lời là`0`. Điều này xác nhận rằng thuật toán không vô tình tạo ra các hoạt động cho các vị trí khớp và việc sử dụng bộ nhớ của nó vẫn tuyến tính ngay cả ở kích thước đầu vào lớn nhất được phép.

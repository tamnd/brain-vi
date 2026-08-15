---
title: "CF 102348G - Hoán đổi chữ cái"
description: "Chúng ta có hai chuỗi nhị phân s và t có cùng độ dài. Tại một thao tác, chúng ta có thể chọn bất kỳ vị trí nào trong s và bất kỳ vị trí nào trong t, sau đó hoán đổi hai ký tự đó. Hai vị trí không nhất thiết phải bằng nhau, do đó một thao tác có thể khắc phục hai điểm không khớp khác nhau cùng một lúc."
date: "2026-08-14T02:20:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "G"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 332
verified: false
draft: false
---

[CF 102348G - Hoán đổi chữ cái](https://codeforces.com/problemset/problem/102348/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai chuỗi nhị phân`s`Và`t`có cùng độ dài. Tại một thao tác, chúng ta có thể chọn bất kỳ vị trí nào trong`s`và bất kỳ vị trí nào trong`t`, sau đó hoán đổi hai ký tự đó. Hai vị trí không nhất thiết phải bằng nhau, do đó một thao tác có thể khắc phục hai điểm không khớp khác nhau cùng một lúc. 

Mục tiêu là làm cho toàn bộ chuỗi giống hệt nhau bằng cách sử dụng càng ít hoán đổi chuỗi chéo càng tốt. Chúng ta phải in số lần hoán đổi tối thiểu cùng với một chuỗi tối ưu hoặc in`-1`khi không có trình tự nào có thể hoạt động. 

Cách hữu ích để xem xét một vị trí là so sánh hai ký tự của nó. Chỉ có bốn khả năng. Vị trí chứa`aa`hoặc`bb`đã đúng rồi. Một vị trí chứa`ab`có nghĩa`s[i] = a`Và`t[i] = b`, trong khi`ba`có nghĩa là ngược lại. Việc hoán đổi giữa các vị trí phù hợp có thể loại bỏ đồng thời hai điểm không khớp. 

Chiều dài có thể đạt tới`2 * 10^5`, do đó thuật toán quét tất cả các cặp vị trí đã quá chậm. Một thuật toán bậc hai có thể thực hiện gần đúng`4 * 10^10`kiểm tra cặp trong trường hợp xấu nhất, vượt xa giới hạn 2 giây cho phép. Chúng tôi cần một giải pháp có công việc tăng tuyến tính theo độ dài chuỗi, ngoài số lượng thao tác chúng tôi thực sự in. 

Ngoài ra còn có một điều kiện khả thi toàn cầu. Mọi thao tác chỉ trao đổi ký tự giữa hai chuỗi, do đó tổng số ký tự`a`ký tự trên cả hai chuỗi không bao giờ thay đổi. Để các chuỗi cuối cùng bằng nhau, mỗi ký tự phải xuất hiện số lần chẵn trên hai chuỗi. Theo đó, nếu tổng số`a`ký tự là lẻ, câu trả lời là không thể. Điều kiện tương tự có thể được biểu diễn trực tiếp hơn bằng cách sử dụng sự không khớp: số lượng`ab`vị trí cộng với số lượng`ba`các vị trí phải bằng nhau. 

Trường hợp cạnh đầu tiên là trường hợp không khớp duy nhất. Đối với đầu vào```
1
a
b
```vị trí duy nhất là`ab`, do đó tổng số điểm không khớp là số lẻ. Đầu ra đúng là`-1`. Việc triển khai bất cẩn chỉ ghi lại những điểm không khớp và ghép nối bất cứ thứ gì có sẵn có thể để lại một vị trí không khớp và in sai một chuỗi. 

Trường hợp cạnh thứ hai xảy ra khi có một`ab`không phù hợp và một`ba`không khớp. Ví dụ,```
2
ab
ba
```hai thao tác là cần thiết. Sự hoán đổi đầu tiên có thể biến đổi`ab`vị trí vào`ba`, nhưng nó không giải quyết được vấn đề. Cần có sự hoán đổi thứ hai để giải quyết cặp kết quả. Việc coi mọi cặp không khớp là có thể giải được trong một thao tác sẽ cho rằng một thao tác là đủ một cách không chính xác. 

Trường hợp cạnh thứ ba là tất cả các vị trí đều có thể khớp nhau. Ví dụ,```
3
aba
aba
```không yêu cầu thao tác nào. Việc triển khai giả định tồn tại ít nhất một điểm không khớp có thể vô tình truy cập vào danh sách không khớp trống hoặc in một thao tác không cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là kiểm tra các vị trí không khớp và liên tục tìm kiếm hai vị trí có ký tự có thể được trao đổi để đạt được tiến bộ. Vì có hai loại không khớp, người ta có thể thử mọi cặp chỉ số có thể có và kiểm tra xem việc hoán đổi các ký tự tương ứng có làm giảm số lượng không khớp hay không. Điều này đúng vì việc tìm kiếm toàn diện sẽ xem xét mọi giao dịch hoán đổi hợp pháp, nhưng nó quá đắt. Với`n`các vị trí, có`n²`các hoán đổi chuỗi chéo có thể xảy ra và việc tìm kiếm liên tục qua chúng có thể yêu cầu theo thứ tự`n²`séc. Tại`n = 2 * 10^5`, đó là về`4 * 10^10`trao đổi ứng viên. 

Quan sát quan trọng là các ký tự thực tế chỉ quan trọng thông qua loại không khớp. Giả sử vị trí`i`Và`j`cả hai đều thuộc loại`ab`. Ở cả hai vị trí ta có`s = a`Và`t = b`. Hoán đổi`s[i]`với`t[j]`trao đổi`a`Và`b`, vì vậy cả hai vị trí đều trở thành`bb`Và`aa`. Như vậy hai`ab`sự không phù hợp có thể được sửa chữa bằng một thao tác. Lập luận tương tự có tác dụng đối với hai`ba`sự không phù hợp. 

Điều này ngay lập tức gợi ý việc lưu trữ các chỉ mục của hai loại không khớp riêng biệt. Mỗi cặp trong cùng một loại tốn chính xác một thao tác và không có lý do gì để tìm kiếm cách sắp xếp tốt hơn vì một thao tác là mức tối thiểu về mặt lý thuyết để sửa hai điểm không khớp. 

Trường hợp thú vị duy nhất là khi cả hai danh sách không khớp đều có kích thước lẻ. Sau khi ghép nối càng nhiều vị trí càng tốt, một`ab`và một`ba`duy trì. Chúng không thể được cố định trong một thao tác vì hướng ký tự của chúng ngược nhau. Tuy nhiên, hai thao tác là đủ. Nếu như`i`là phần còn lại`ab`vị trí và`j`là phần còn lại`ba`vị trí, trao đổi đầu tiên`s[i]`với`t[i]`. Chức vụ`i`thay đổi từ`ab`ĐẾN`ba`. Vị trí hiện tại`i`Và`j`cả hai đều`ba`, thế là đổi chỗ`s[i]`với`t[j]`sửa cả hai. 

Mức tối thiểu tuân theo cùng một cấu trúc. Mỗi thao tác chỉ có thể khắc phục tối đa hai vị trí không khớp, do đó việc ghép hai loại không khớp bằng nhau trong một thao tác là tối ưu. Khi mỗi loại vẫn còn một điểm không khớp, một thao tác không thể khắc phục cả hai, trong khi cấu trúc ở trên sẽ sửa chúng thành chính xác hai. Do đó, số lượng hoạt động kết quả là tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n²)`hoặc tệ hơn với những tìm kiếm lặp đi lặp lại |`O(n)`| Quá chậm | 
| Tối ưu |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mọi vị trí`i`từ`0`ĐẾN`n - 1`. Nếu như`s[i] = t[i]`, không cần phải làm gì ở đó. Nếu như`s[i] = a`Và`t[i] = b`, cửa hàng`i`trong`ab`danh sách. Nếu như`s[i] = b`Và`t[i] = a`, cửa hàng`i`trong`ba`danh sách. Hai danh sách này chứa chính xác các vị trí vẫn cần được chú ý. 
2. Kiểm tra tính chẵn lẻ của hai danh sách không khớp. Nếu như`len(ab) + len(ba)`thật kỳ lạ, hãy in`-1`. Số lượng không khớp không thể thay đổi tính chẵn lẻ theo cách cho phép mọi vị trí trở nên bằng nhau, vì tổng số mỗi ký tự trên cả hai chuỗi được giữ nguyên. 
3. Ghép các vị trí liên tiếp trong`ab`. Đối với mỗi cặp`ab[i]`Và`ab[i + 1]`, thêm thao tác`(ab[i], ab[i + 1])`. Cả hai vị trí đều có dạng`ab`, do đó hoán đổi`a`từ vị trí đầu tiên trong`s`với`b`từ vị trí thứ hai trong`t`thay đổi cả hai vị trí thành các cặp bằng nhau. 
4. Ghép các vị trí liên tiếp trong`ba`theo cách tương tự. Đối với mỗi cặp`ba[i]`Và`ba[i + 1]`, thêm vào`(ba[i], ba[i + 1])`. Vì cả hai vị trí đều có dạng`ba`, lý do tương tự sẽ khắc phục cả hai bằng một thao tác. 
5. Nếu cả hai danh sách đều có độ dài lẻ, một vị trí`i`vẫn còn trong`ab`và một vị trí`j`vẫn còn trong`ba`. Thêm thao tác`(i, i)`. Kể từ vị trí`i`là`ab`, hoán đổi hai ký tự của chính nó sẽ thay đổi nó thành`ba`. 
6. Thêm thao tác thứ hai`(i, j)`. Chức vụ`i`bây giờ là`ba`, và vị trí`j`đã rồi`ba`, thế là đổi chỗ`s[i]`với`t[j]`thay đổi cả hai vị trí thành các cặp bằng nhau. 
7. Chuyển đổi mọi chỉ mục dựa trên số 0 được lưu trữ thành chỉ mục dựa trên một khi in. In ra số lượng thao tác theo sau là các cặp thao tác. 

### Tại sao nó hoạt động 

Điều bất biến là mọi vị trí không được lưu trữ trong`ab`hoặc`ba`đã bằng nhau, trong khi mọi vị trí được lưu trữ được biểu thị bằng chính xác một loại không khớp. Một cặp cùng loại luôn có thể tháo rời được trong một thao tác, vì vậy tất cả các cặp như vậy có thể được loại bỏ một cách độc lập. Nếu vẫn còn hai danh sách không khớp có kích thước lẻ, thao tác đầu tiên sẽ thay đổi một danh sách còn sót lại`ab`vào trong`ba`, sau đó hai phần còn lại có cùng loại và thao tác thứ hai sẽ loại bỏ chúng. Nếu tổng số không khớp là số lẻ thì tổng số ký tự được giữ nguyên sẽ khiến trạng thái mục tiêu không thể truy cập được. Mỗi thao tác sửa tối đa hai điểm không khớp và thuật toán sử dụng một thao tác bất cứ khi nào hai điểm không khớp có thể được sửa cùng nhau, với chính xác hai thao tác cho phần còn lại thuộc loại đối lập không thể tránh khỏi. Do đó chuỗi được tạo ra vừa hợp lệ vừa tối thiểu. 

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

    ans = []

    for i in range(0, len(ab) - 1, 2):
        ans.append((ab[i], ab[i + 1]))

    for i in range(0, len(ba) - 1, 2):
        ans.append((ba[i], ba[i + 1]))

    if len(ab) % 2 == 1:
        i = ab[-1]
        j = ba[-1]
        ans.append((i, i))
        ans.append((i, j))

    out = [str(len(ans))]
    for x, y in ans:
        out.append(f"{x + 1} {y + 1}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên phân loại mọi vị trí chính xác một lần. Các vị trí bằng nhau sẽ bị bỏ qua, trong khi hai hướng có thể không khớp sẽ được lưu trữ riêng biệt. Vì bảng chữ cái chỉ chứa`a`Và`b`, không có loại không khớp thứ ba để xử lý. 

Hai vòng ghép nối tiến lên hai. Đây là lý do tại sao giới hạn trên là`len(ab) - 1`còn hơn là`len(ab)`: phần tử chưa ghép cặp cuối cùng phải được giữ nguyên cho đến khi trường hợp hai thao tác đặc biệt được xử lý. Điều tương tự cũng áp dụng cho`ba`. 

Trường hợp đặc biệt chỉ kiểm tra`len(ab) % 2`. Khi tổng số điểm không khớp được biết là chẵn,`ab`Và`ba`nhất thiết phải có cùng độ chẵn lẻ. Như vậy nếu`ab`thật kỳ lạ,`ba`cũng lẻ và cả hai đều có đúng một phần tử còn sót lại sau khi ghép nối. 

Các chỉ mục hoạt động được lưu trữ nội bộ dưới dạng giá trị dựa trên 0 vì các chuỗi Python sử dụng chỉ mục dựa trên 0. Đầu ra yêu cầu các vị trí dựa trên một, vì vậy`x + 1`Và`y + 1`được in. Không cần mô phỏng các giao dịch hoán đổi. Việc phân loại một cặp chứng minh tác dụng của thao tác đó và việc tránh mô phỏng giúp việc triển khai trở nên đơn giản. 

Không có vấn đề tràn số nguyên trong Python. Số lượng hoạt động tối đa là nhiều nhất`n + 1`, đủ nhỏ để lưu trữ và in trực tiếp. Bản thân đầu ra có thể chứa`O(n)`nên việc xây dựng nó dưới dạng danh sách các chuỗi và viết nó một lần sẽ hiệu quả hơn việc thực hiện nhiều chuỗi riêng lẻ.`print`cuộc gọi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4
abab
aabb
```Việc phân loại vị trí là: 

| Vị trí |`s[i]`|`t[i]`| Loại | 
| --- | --- | --- | --- | 
| 1 |`a`|`a`| bằng | 
| 2 |`b`|`a`|`ba`| 
| 3 |`a`|`b`|`ab`| 
| 4 |`b`|`b`| bằng | 

Các biến kết quả là: 

|`ab`|`ba`| Đã thêm hoạt động | 
| --- | --- | --- | 
|`[3]`|`[2]`| chưa có | 
|`[3]`|`[2]`|`(3, 3)`| 
|`[3]`|`[2]`|`(3, 3)`,`(3, 2)`| 

Hoạt động đầu tiên sử dụng phần còn lại`ab`vị trí của chính nó. Vị trí 3 thay đổi từ`ab`ĐẾN`ba`. Bây giờ vị trí 3 và 2 đều là`ba`, do đó thao tác thứ hai sẽ sửa chúng lại với nhau. Câu trả lời cuối cùng có hai thao tác, khớp với mức tối thiểu của mẫu. 

### Mẫu 2 

Đầu vào là```
1
a
b
```Quá trình quét tạo ra: 

| Vị trí |`s[i]`|`t[i]`|`ab`|`ba`| 
| --- | --- | --- | --- | --- | 
| 1 |`a`|`b`|`[1]`|`[]`| 

Tổng số không khớp là một, số lẻ. 

| Tổng số không khớp | Khả thi? | Đầu ra | 
| --- | --- | --- | 
| 1 | Không |`-1`| 

Không có vị trí không khớp thứ hai nào có thể giải quyết được sự mất cân bằng ký tự đơn độc. Thuật toán từ chối trường hợp trước khi cố gắng truy cập đối tác không tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi ký tự được quét một lần, mỗi ký tự không khớp được xử lý một lần và đầu ra chứa`O(n)`hoạt động. | 
| Không gian |`O(n)`| Hai danh sách không khớp và danh sách thao tác đầu ra cùng nhau chứa`O(n)`các phần tử. | 

Với`n`lên đến`2 * 10^5`, quét tuyến tính dễ dàng phù hợp với giới hạn 2 giây. Việc sử dụng bộ nhớ cũng tuyến tính và thoải mái dưới 256 MB. Bản thân số lượng thao tác được in là tuyến tính, do đó thuật toán tối ưu tiệm cận vì chỉ cần đọc đầu vào và tạo ra câu trả lời đã yêu cầu`O(n)`làm việc trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm 

Bộ khai thác thử nghiệm phải xác thực các thuộc tính của đầu ra thay vì so sánh trình tự hoạt động chính xác vì bài toán cho phép bất kỳ trình tự tối ưu nào. Việc triển khai đúng khác nhau có thể tạo ra các cặp chỉ mục khác nhau, tối thiểu như nhau.```python
import sys
import io

def solve_io():
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

    ans = []

    for i in range(0, len(ab) - 1, 2):
        ans.append((ab[i], ab[i + 1]))

    for i in range(0, len(ba) - 1, 2):
        ans.append((ba[i], ba[i + 1]))

    if len(ab) % 2:
        i = ab[-1]
        j = ba[-1]
        ans.append((i, i))
        ans.append((i, j))

    print(len(ans))
    for x, y in ans:
        print(x + 1, y + 1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve_io()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def check(inp: str):
    data = inp.strip().splitlines()
    n = int(data[0])
    s = data[1]
    t = data[2]

    out = run(inp).strip().splitlines()

    possible = (sum(c == 'a' for c in s) +
                sum(c == 'a' for c in t)) % 2 == 0

    if not possible:
        assert out == ["-1"]
        return

    k = int(out[0])
    assert len(out) == k + 1

    # The theoretical minimum.
    ab = []
    ba = []
    for i in range(n):
        if s[i] == t[i]:
            continue
        if s[i] == 'a':
            ab.append(i)
        else:
            ba.append(i)

    expected = len(ab) // 2 + len(ba) // 2
    if len(ab) % 2:
        expected += 2

    assert k == expected

    ss = list(s)
    tt = list(t)

    for line in out[1:]:
        x, y = map(int, line.split())
        assert 1 <= x <= n
        assert 1 <= y <= n

        x -= 1
        y -= 1
        ss[x], tt[y] = tt[y], ss[x]

    assert ss == tt

# Provided samples.
check("""4
abab
aabb
""")

check("""1
a
b
""")

check("""8
babbaabb
abababaa
""")

# Minimum size, already equal.
check("""1
a
a
""")

# Minimum size, impossible with one mismatch.
check("""1
b
a
""")

# All equal values, with a longer input.
check("""6
aaaaaa
aaaaaa
""")

# Two same-type mismatches, requiring exactly one operation.
check("""2
aa
bb
""")

# Opposite mismatch types, requiring the special two-operation construction.
check("""2
ab
ba
""")

# Larger boundary-style case.
n = 200000
s = "a" * n
t = "b" * n
check(f"{n}\n{s}\n{t}\n")

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a / a`|`0`hoạt động | Trường hợp có kích thước tối thiểu đã bằng nhau | 
|`1 / b / a`|`-1`| Trường hợp không thể có kích thước tối thiểu và số lượng không khớp lẻ | 
|`6 / aaaaaa / aaaaaa`|`0`hoạt động | Tất cả các vị trí đã bằng nhau | 
|`2 / aa / bb`|`1`hoạt động | Hai sự không phù hợp cùng loại | 
|`2 / ab / ba`|`2`hoạt động | Công trình xây dựng kiểu đối lập đặc biệt | 
|`n = 200000`,`s = a...a`,`t = b...b`|`100000`hoạt động | Đầu vào có kích thước tối đa và hành vi thời gian tuyến tính | 

Bộ khai thác cũng kiểm tra xem mọi chỉ mục được báo cáo có nằm trong phạm vi dựa trên một hợp lệ hay không, áp dụng các thao tác cho các chuỗi, xác minh rằng các chuỗi cuối cùng bằng nhau và tính toán độc lập mức tối thiểu theo lý thuyết. Điều này phát hiện cả việc xây dựng hoạt động không chính xác và lỗi từng cái một. 

## Vỏ cạnh 

Trường hợp không khớp lẻ được xử lý trước khi ghép nối. Vì```
1
a
b
```cái`ab`danh sách có kích thước một và`ba`danh sách có kích thước bằng không. Tổng của chúng là số lẻ nên thuật toán in ngay`-1`. Không có đối tác không hợp lệ nào được truy cập. 

Trường hợp ngược lại là trường hợp tế nhị. Vì```
2
ab
ba
```chúng tôi nhận được`ab = [0]`Và`ba = [1]`. Cả hai danh sách đều kỳ quặc. Thuật toán đầu tiên thêm`(0, 0)`, thay đổi vị trí đầu tiên từ`ab`ĐẾN`ba`. Sau đó nó thêm`(0, 1)`. Tại thời điểm đó cả hai vị trí đều có loại`ba`, do đó trao đổi`s[0] = b`với`t[1] = a`làm cho vị trí 1 bằng`aa`và vị trí 2 bằng`bb`. Hai thao tác được sử dụng và một thao tác không thể đủ. 

Đối với một đầu vào đã bằng nhau như```
3
aba
aba
```cả hai danh sách không khớp vẫn trống. Tổng số không khớp bằng 0, các vòng lặp ghép nối không làm gì, trường hợp đặc biệt không làm gì và chương trình sẽ in`0`. Đây là mức tối thiểu chính xác vì không cần trao đổi. 

Đối với hai sự không khớp cùng loại, hãy xem xét```
2
aa
bb
```Cả hai vị trí đều`ab`, Vì thế`ab = [0, 1]`. Vòng ghép nối đầu tiên thêm`(0, 1)`. Hoán đổi`s[0] = a`với`t[1] = b`sản xuất`bb`ở vị trí đầu tiên và`aa`trong lần thứ hai, do đó các chuỗi trở nên bằng nhau sau đúng một thao tác. Thuật toán đạt đến giới hạn dưới của một thao tác. 

Cuối cùng, trường hợp kích thước tối đa với```
200000
aaaaaaaaaa...aaaaaaaaaa
bbbbbbbbbb...bbbbbbbbbb
```có`200000` `ab`sự không phù hợp. Chúng có thể được nhóm lại thành`100000`cặp và mỗi cặp yêu cầu một lần hoán đổi. Thuật toán thực hiện một lần quét tuyến tính và tạo ra chính xác`100000`hoạt động. Nó không bao giờ kiểm tra đại khái`4 * 10^10`có thể có các cặp chỉ mục chuỗi chéo, đó là lý do chính khiến nó vẫn đủ nhanh.

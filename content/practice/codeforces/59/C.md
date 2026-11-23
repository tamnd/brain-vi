---
title: "CF 59C - Tiêu đề"
description: "Chúng ta được cung cấp một mẫu chuỗi bao gồm các chữ cái viết thường và dấu chấm hỏi. Chuỗi cuối cùng phải thỏa mãn đồng thời ba điều kiện. Đầu tiên, nó phải là một bảng màu, do đó các ký tự được phản chiếu xung quanh trung tâm phải khớp với nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "expression-parsing"]
categories: ["algorithms"]
codeforces_contest: 59
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 55 (Div. 2)"
rating: 1600
weight: 59
solve_time_s: 113
verified: true
draft: false
---

[CF 59C - Tiêu đề](https://codeforces.com/problemset/problem/59/C) 

**Đánh giá:** 1600 
**Thẻ:** phân tích biểu thức 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mẫu chuỗi bao gồm các chữ cái viết thường và dấu chấm hỏi. Chuỗi cuối cùng phải thỏa mãn đồng thời ba điều kiện. 

Đầu tiên, nó phải là một bảng màu, do đó các ký tự được phản chiếu xung quanh trung tâm phải khớp với nhau. 

Thứ hai, nó chỉ có thể sử dụng cái đầu tiên`k`chữ cái viết thường, nghĩa là chỉ các ký tự từ`'a'`ĐẾN`'a' + k - 1`. 

Thứ ba, mỗi thứ đó`k`các chữ cái phải xuất hiện ít nhất một lần ở đâu đó trong chuỗi cuối cùng. 

Dấu chấm hỏi có thể được thay thế bằng các chữ cái, nhưng các chữ cái cố định thì không thể thay đổi được. 

Trong số tất cả các câu trả lời hợp lệ, chúng ta phải đưa ra câu trả lời nhỏ nhất về mặt từ điển. Nếu không có bảng màu hợp lệ tồn tại, chúng tôi in`"IMPOSSIBLE"`. 

Độ dài chuỗi tối đa là 100, rất nhỏ. Ngay cả lực lượng vũ phu theo cấp số nhân cũng không lớn về mặt thiên văn đối với các trường hợp nhỏ, nhưng số dấu hỏi cũng có thể lên tới 100, vì vậy việc thử tất cả các thay thế có nghĩa là lên tới`26^100`khả năng, hoàn toàn không thể. Thay vào đó, giới hạn nhỏ cho thấy rằng một thuật toán xây dựng cẩn thận là đủ. 

Phần tinh tế nhất là thứ tự từ điển. Một chiến lược tham lam luôn đặt chữ cái nhỏ nhất có thể có thể vô tình khiến một số ký tự bắt buộc không thể đặt sau này. 

Một điểm tinh tế khác là hạn chế palindrom. Mỗi phép gán đều ảnh hưởng đến hai vị trí cùng một lúc trừ khi chúng ta ở giữa một chuỗi có độ dài lẻ. 

Hãy xem xét ví dụ này:```
k = 2
s = "a?b"
```Ký tự đầu tiên và cuối cùng phải khớp nhau trong một bảng màu nhưng chúng đã được cố định thành`'a'`Và`'b'`. Không có sự thay thế nào có thể sửa chữa được điều này, vì vậy câu trả lời đúng là:```
IMPOSSIBLE
```Việc thực hiện bất cẩn mà chỉ điền vào dấu chấm hỏi sẽ bỏ sót mâu thuẫn này. 

Một trường hợp nguy hiểm khác là không đủ chỗ để bao gồm tất cả các chữ cái được yêu cầu.```
k = 3
s = "aaa"
```Xâu này đã là một chuỗi palindrome nhưng nó chỉ chứa`'a'`. Vì không còn vị trí nào để giới thiệu`'b'`Và`'c'`, câu trả lời là không thể. 

Vị trí trung tâm cũng cần được chú ý.```
k = 2
s = "?"
```Một palindrome một ký tự chỉ có thể chứa một chữ cái riêng biệt, vì vậy chúng ta không thể bao gồm cả hai`'a'`Và`'b'`. Câu trả lời là không thể mặc dù dây hoàn toàn mềm dẻo. 

Cuối cùng, trật tự từ điển có thể phá vỡ tính tham lam ngây thơ.```
k = 2
s = "??"
```Các palindrome có thể có là`"aa"`Và`"bb"`, nhưng chỉ`"bb"`chứa thư`'b'`. Câu trả lời đúng là:```
bb
```Lựa chọn`'a'`ngay lập tức vì nó nhỏ hơn sẽ tạo ra chuỗi cuối cùng không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Thay thế mọi dấu chấm hỏi bằng mọi chữ cái được phép có thể, tạo ra tất cả các chuỗi ứng cử viên và giữ lại bảng màu hợp lệ nhỏ nhất chứa tất cả`k`các chữ cái. 

Điều này hoạt động hợp lý vì tìm kiếm sẽ khám phá mọi khả năng hoàn thành có thể. Vấn đề là số lượng khả năng. Nếu chuỗi chứa`q`dấu hỏi, có`k^q`bài tập. Với`q = 100`Và`k = 26`, cái này lớn về mặt thiên văn. 

Quan sát quan trọng là cấu trúc palindrome kết hợp các vị trí với nhau. Một khi chúng ta quyết định nhân vật ở vị trí`i`, vị trí phản chiếu`n - 1 - i`bị ép buộc. Thay vì xử lý từng dấu chấm hỏi một cách độc lập, chúng ta có thể xử lý các cặp đối xứng. 

Một quan sát quan trọng khác là thứ tự từ điển phụ thuộc vào các vị trí trước đó. Khi quyết định một cặp, chúng tôi muốn chữ cái nhỏ nhất có thể mà vẫn có đủ tự do để đặt tất cả các chữ cái cần thiết còn thiếu sau này. 

Điều này dẫn đến một giải pháp tham lam mang tính xây dựng. 

Đầu tiên chúng tôi thực thi tính nhất quán của palindrome. Đối với mỗi cặp được nhân đôi: 

Nếu cả hai ký tự đều cố định và khác nhau thì câu trả lời là không thể. 

Nếu một bên cố định và bên kia`'?'`, dấu chấm hỏi phải trở thành ký tự cố định. 

Nếu cả hai đều`'?'`, chúng tôi trì hoãn sự lựa chọn. 

Sau giai đoạn này, chuỗi đã hoạt động giống như một phần palindrome. 

Tiếp theo, chúng tôi theo dõi những chữ cái cần thiết vẫn còn thiếu. Mỗi cặp đối xứng chưa được giải quyết có thể giới thiệu nhiều nhất một chữ cái mới, vì cả hai vị trí phải bằng nhau. Ký tự trung tâm của một chuỗi có độ dài lẻ cũng có thể giới thiệu một chữ cái. 

Chúng tôi xử lý các cặp từ trái sang phải. Để có được sự tối giản về mặt từ điển, chúng ta thử dùng chữ cái nhỏ nhất có thể ở mỗi bước. Nhưng trước khi cam kết, chúng tôi kiểm tra xem các vị trí chưa được giải quyết còn lại có đủ để đặt tất cả các chữ cái còn thiếu sau này hay không. 

Đây là mô hình tham lam cổ điển: chọn phương án hợp lệ nhỏ nhất mà không phá hủy tính khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^q * n) | O(n) | Quá chậm | 
| Tối ưu | O(n * k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi thành một mảng có thể thay đổi. 
2. Xử lý từng cặp được phản chiếu`(i, j)`Ở đâu`j = n - 1 - i`. 

Nếu cả hai ký tự đều cố định và khác nhau thì quay lại ngay`"IMPOSSIBLE"`bởi vì không có bảng màu nào có thể thỏa mãn cả hai. 

Nếu chính xác một bên là`'?'`, sao chép ký tự cố định sang phía bên kia. 

Bước này loại bỏ tất cả các quyết định bắt buộc trước khi bắt đầu bất kỳ lựa chọn tham lam nào. 
3. Đếm cái nào đầu tiên`k`các chữ cái đã xuất hiện trong chuỗi. 
4. Thu thập mọi vị trí chưa được giải quyết. 

Đối với một cặp`"??"`chúng tôi chỉ lưu trữ chỉ mục bên trái vì cả hai vị trí sẽ luôn nhận được cùng một chữ cái. 

Nếu chiều dài là số lẻ và tâm là`'?'`, hãy lưu trữ riêng. 
5. Tính tập hợp các chữ cái còn thiếu. 
6. Xử lý các vị trí chưa giải quyết từ trái qua phải. 

Với mỗi vị trí, hãy thử các chữ cái từ`'a'`trở lên. 

Đặt tạm thời chữ cái và kiểm tra xem các vị trí chưa được giải quyết còn lại có đủ để đặt tất cả các chữ cái còn thiếu sau đó hay không. 

Chữ cái khả thi đầu tiên được chọn vĩnh viễn. 

Các vị trí trước đó thống trị thứ tự từ điển, vì vậy việc chọn chữ cái khả thi nhỏ nhất sẽ đảm bảo câu trả lời nhỏ nhất trên toàn cầu. 
7. Sau khi điền đầy đủ tất cả các vị trí, hãy xác minh rằng mọi chữ cái được yêu cầu đều xuất hiện ít nhất một lần. 

Nếu vẫn thiếu một số chữ cái cần thiết, hãy in`"IMPOSSIBLE"`. 
8. Nếu không thì nối mảng vào chuỗi cuối cùng. 

### Tại sao nó hoạt động 

Thuật toán duy trì hai bất biến trong suốt quá trình xây dựng. 

Đầu tiên, chuỗi luôn tương thích với việc trở thành một bảng màu. Mỗi bài tập đều viết cùng một ký tự vào các vị trí phản chiếu. 

Thứ hai, sau mỗi lần lựa chọn tham lam, những vị trí chưa giải quyết còn lại vẫn đủ để đặt tất cả các chữ cái cần thiết còn thiếu. Vì việc kiểm tra tính khả thi này nên thuật toán không bao giờ tự dồn mình vào một góc. 

Sự tối thiểu về mặt từ điển học tuân theo thứ tự tham lam từ trái sang phải. Ở mọi vị trí, chúng tôi chọn ký tự nhỏ nhất mà vẫn cho phép giải pháp hoàn chỉnh hợp lệ sau này. Bất kỳ lựa chọn nhỏ hơn nào cũng sẽ khiến việc hoàn thành không thể thực hiện được và bất kỳ lựa chọn nào lớn hơn sẽ tạo ra một chuỗi lớn hơn về mặt từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input())
    s = list(input().strip())

    n = len(s)

    # Enforce palindrome constraints
    for i in range(n // 2):
        j = n - 1 - i

        if s[i] != '?' and s[j] != '?':
            if s[i] != s[j]:
                print("IMPOSSIBLE")
                return

        elif s[i] == '?' and s[j] != '?':
            s[i] = s[j]

        elif s[i] != '?' and s[j] == '?':
            s[j] = s[i]

    required = [chr(ord('a') + i) for i in range(k)]

    present = set(ch for ch in s if ch != '?')

    unresolved = []

    for i in range(n // 2):
        j = n - 1 - i
        if s[i] == '?' and s[j] == '?':
            unresolved.append((i, j))

    center = -1
    if n % 2 == 1 and s[n // 2] == '?':
        center = n // 2

    total_slots = len(unresolved) + (1 if center != -1 else 0)

    missing = set(required) - present

    if len(missing) > total_slots:
        print("IMPOSSIBLE")
        return

    positions = []

    for pair in unresolved:
        positions.append(pair)

    if center != -1:
        positions.append((center, center))

    for idx, (i, j) in enumerate(positions):
        remaining_after = len(positions) - idx - 1

        for c in required:
            new_missing = len(missing)

            if c in missing:
                new_missing -= 1

            if new_missing <= remaining_after:
                s[i] = c
                s[j] = c

                if c in missing:
                    missing.remove(c)

                break

    final_present = set(s)

    for c in required:
        if c not in final_present:
            print("IMPOSSIBLE")
            return

    print("".join(s))

solve()
```Giai đoạn đầu tiên giải quyết các phép gán palindrome bắt buộc. Đây là nơi duy nhất mà mâu thuẫn có thể xuất hiện. Nếu các ký tự cố định được nhân đôi không đồng ý thì không có lựa chọn nào sau này có thể sửa chữa chuỗi. 

Thuật toán sau đó xác định các vị trí đối xứng chưa được giải quyết. Việc coi mỗi cặp như một quyết định duy nhất là cách đơn giản hóa chính. Một cặp chỉ đóng góp một lựa chọn nhân vật độc lập vì cả hai bên phải phù hợp. 

Giai đoạn tham lam là nơi trật tự từ điển được xử lý cẩn thận. Tại mỗi vị trí chưa được giải quyết, mã sẽ kiểm tra các chữ cái từ nhỏ nhất đến lớn nhất. Trước khi cam kết, nó sẽ kiểm tra xem các vị trí còn lại có thể bao gồm tất cả các chữ cái bị thiếu hay không. 

Điều kiện:```
new_missing <= remaining_after
```là thử nghiệm khả thi cốt lõi. 

Nếu chúng ta vẫn cần ba chữ cái riêng biệt nhưng chỉ còn lại hai vị trí chưa được giải quyết thì lựa chọn hiện tại không thể hoạt động. 

Vị trí trung tâm được thể hiện dưới dạng`(center, center)`, do đó, logic gán giống nhau sẽ hoạt động thống nhất cho cả cặp và ký tự ở giữa. 

Bước xác minh cuối cùng sẽ dư thừa về mặt kỹ thuật nếu logic đúng, nhưng việc giữ nó sẽ giúp việc triển khai an toàn hơn và dễ dàng hơn để đưa ra lý do. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
a?c
```| Bước | Trạng thái chuỗi | Giải thích | 
| --- | --- | --- | 
| Ban đầu |`a?c`| Mẫu gốc | 
| Kiểm tra cặp |`a?c`|`'a'`Và`'c'`xung đột | 
| Kết quả |`IMPOSSIBLE`| Không thể hình thành palindrom | 

Ví dụ này cho thấy sự thất bại sớm nhất có thể xảy ra. Mặc dù có sẵn dấu chấm hỏi nhưng tính đối xứng palindrome đã buộc các ký tự không tương thích. 

### Ví dụ 2 

đầu vào:```
3
?????
```| Bước | Trạng thái chuỗi | Chữ Thiếu | Các slot còn lại | 
| --- | --- | --- | --- | 
| Ban đầu |`?????`|`{a,b,c}`| 3 | 
| Điền vào cặp (0,4) bằng`a`|`a???a`|`{b,c}`| 2 | 
| Điền vào cặp (1,3) bằng`b`|`ab?ba`|`{c}`| 1 | 
| Điền vào trung tâm với`c`|`abcba`|`{}`| 0 | 

Dấu vết cho thấy chiến lược tham lam duy trì tính khả thi như thế nào. Thuật toán luôn chọn chữ cái nhỏ nhất có thể trong khi vẫn đảm bảo vẫn còn đủ chỗ cho các chữ cái còn thiếu trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * k) | Mỗi vị trí chưa được giải quyết có thể kiểm tra tới`k`chữ cái | 
| Không gian | O(n) | Mảng ký tự có thể thay đổi và cấu trúc sổ sách kế toán | 

Độ dài chuỗi tối đa chỉ là 100, vì vậy ngay cả`100 * 26`các hoạt động rất nhỏ. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    k = int(input())
    s = list(input().strip())

    n = len(s)

    for i in range(n // 2):
        j = n - 1 - i

        if s[i] != '?' and s[j] != '?':
            if s[i] != s[j]:
                return "IMPOSSIBLE"

        elif s[i] == '?' and s[j] != '?':
            s[i] = s[j]

        elif s[i] != '?' and s[j] == '?':
            s[j] = s[i]

    required = [chr(ord('a') + i) for i in range(k)]

    present = set(ch for ch in s if ch != '?')

    unresolved = []

    for i in range(n // 2):
        j = n - 1 - i
        if s[i] == '?' and s[j] == '?':
            unresolved.append((i, j))

    center = -1
    if n % 2 == 1 and s[n // 2] == '?':
        center = n // 2

    total_slots = len(unresolved) + (1 if center != -1 else 0)

    missing = set(required) - present

    if len(missing) > total_slots:
        return "IMPOSSIBLE"

    positions = unresolved[:]

    if center != -1:
        positions.append((center, center))

    for idx, (i, j) in enumerate(positions):
        remaining_after = len(positions) - idx - 1

        for c in required:
            new_missing = len(missing)

            if c in missing:
                new_missing -= 1

            if new_missing <= remaining_after:
                s[i] = c
                s[j] = c

                if c in missing:
                    missing.remove(c)

                break

    if any(c not in s for c in required):
        return "IMPOSSIBLE"

    return "".join(s)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("3\na?c\n") == "IMPOSSIBLE", "sample 1"

# minimum size valid
assert run("1\n?\n") == "a", "single character"

# minimum size impossible
assert run("2\n?\n") == "IMPOSSIBLE", "cannot fit two letters"

# palindrome conflict
assert run("2\na?b\n") == "IMPOSSIBLE", "fixed mismatch"

# lexicographical choice
assert run("3\n?????\n") == "abcba", "smallest valid palindrome"

# already complete
assert run("2\nabba\n") == "abba", "already valid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n?\n`|`a`| Trường hợp hợp lệ nhỏ nhất | 
|`2\n?\n`|`IMPOSSIBLE`| Không đủ chỗ cho tất cả các chữ cái | 
|`2\na?b\n`|`IMPOSSIBLE`| Đã sửa lỗi mâu thuẫn palindrome | 
|`3\n?????\n`|`abcba`| Xây dựng từ điển tham lam | 
|`2\nabba\n`|`abba`| Bảng màu đã hợp lệ | 

## Vỏ cạnh 

Hãy xem xét trường hợp palindrome xung đột:```
2
a?b
```Thuật toán kiểm tra vị trí`0`Và`2`. Vì chúng chứa các chữ cái cố định khác nhau nên nó ngay lập tức trả về`"IMPOSSIBLE"`mà không thử bất kỳ sự thay thế nào. Điều này xử lý chính xác các mâu thuẫn trước khi bắt đầu tham lam. 

Bây giờ hãy xem xét khả năng không đủ:```
3
aaa
```Không có vị trí nào chưa được giải quyết nên thuật toán không thể đưa ra`'b'`hoặc`'c'`. Bộ còn thiếu có kích thước`2`, trong khi các vị trí có sẵn bằng nhau`0`. Việc kiểm tra tính khả thi thất bại ngay lập tức, tạo ra`"IMPOSSIBLE"`. 

Trường hợp ranh giới một ký tự cũng hoạt động chính xác:```
2
?
```Trung tâm cung cấp chính xác một khe, nhưng bắt buộc phải có hai chữ cái riêng biệt. Từ`len(missing) = 2`Và`total_slots = 1`, thuật toán sẽ từ chối thể hiện trước khi xây dựng. 

Cuối cùng, hãy xem xét áp lực từ điển:```
2
??
```Cặp chưa được giải quyết là`(0,1)`. 

Đang cố gắng`'a'`sẽ rời đi`'b'`vẫn còn thiếu và không còn chỗ trống nên lựa chọn bị từ chối. 

Đang cố gắng`'b'`không để lại chữ cái nào bị thiếu, vì vậy cặp này trở thành`"bb"`. 

Thuật toán đưa ra câu trả lời khả thi nhỏ nhất về mặt từ điển chứ không chỉ là ký tự nhỏ nhất cục bộ.

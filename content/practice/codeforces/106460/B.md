---
title: "CF 106460B - \u0421\u0442\u0440\u043e\u043a\u0430 \u0438\u0437 \u043f\u0430\u043b\u0438\u043d\u0434\u0440\u043e\u043c\u043e\u0432"
description: "Nhiệm vụ là lấy tất cả các ký tự của một chuỗi và chia chúng thành đúng m nhóm không trống. Mỗi nhóm phải được sắp xếp lại thành một bảng màu và mỗi ký tự gốc phải thuộc về chính xác một nhóm. Thứ tự các ký tự trong một nhóm không quan trọng."
date: "2026-06-25T08:59:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106460
codeforces_index: "B"
codeforces_contest_name: "\u041a\u043e\u0433\u043d\u0438\u0442\u0438\u0432\u043d\u044b\u0435 \u0442\u0435\u0445\u043d\u043e\u043b\u043e\u0433\u0438\u0438 2023-2024. \u0422\u0440\u0435\u0442\u0438\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 106460
solve_time_s: 43
verified: true
draft: false
---

[CF 106460B - \u0421\u0442\u0440\u043e\u043a\u0430 \u0438\u0437 \u043f\u0430\u043b\u0438\u043d\u0434\u0440\u043e\u043c\u043e\u0432](https://codeforces.com/problemset/problem/106460/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là lấy tất cả các ký tự của một chuỗi và chia chúng thành các phần chính xác.`m`các nhóm không trống. Mỗi nhóm phải được sắp xếp lại thành một bảng màu và mỗi ký tự gốc phải thuộc về chính xác một nhóm. 

Thứ tự các ký tự trong một nhóm không quan trọng. Chỉ có số lần xuất hiện của mỗi ký tự là quan trọng, bởi vì một bảng màu cần có các cặp khớp xung quanh tâm. Ví dụ: một nhóm chứa`aabb`có thể trở thành`abba`, trong khi một nhóm chứa`aabc`không thể trở thành một bảng màu vì hai ký tự khác nhau sẽ cần chiếm vị trí trung tâm. 

Thông tin chính trong đầu vào là số lượng ký tự xuất hiện với số lần lẻ. Một palindrome có thể chứa nhiều nhất một ký tự có tần số lẻ, vì tất cả các ký tự khác phải được đặt đối xứng. Nếu toàn bộ chuỗi có`k`các ký tự có tần số lẻ thì bất kỳ phân vùng nào thành các palindrome cần ít nhất`k`các nhóm, vì mỗi lần xuất hiện lẻ đó cần có một trung tâm palindrome riêng biệt. 

Các ràng buộc cho phép tổng chiều dài của tất cả các chuỗi đạt tới`2 * 10^5`. Điều này loại trừ bất cứ điều gì liên quan đến việc thử các phân vùng có thể có hoặc xây dựng các bảng màu một cách rõ ràng. Chúng ta cần một giải pháp quét từng ký tự với số lần không đổi, tạo ra độ phức tạp tuyến tính tổng thể. 

Một lỗi phổ biến là chỉ kiểm tra xem toàn bộ chuỗi có thể được tạo thành một bảng màu hay không. Đó là một câu hỏi khác. Ví dụ, đầu vào`ababcab`với`m = 3`có ba ký tự với tần số lẻ, do đó nó có thể được chia thành ba palindrome, mặc dù bản thân nó không phải là một palindrome. Đầu ra đúng là`YES`. 

Một trường hợp khác là khi mỗi ký tự có tần số lẻ. Đối với đầu vào`3 3`và chuỗi`abc`, câu trả lời là`YES`bởi vì mỗi ký tự đơn lẻ đã là một bảng màu. Một giải pháp yêu cầu một bảng màu lớn trước tiên sẽ từ chối nó một cách không chính xác. 

Ranh giới quan trọng khác là khi có quá nhiều tần số lẻ. Đối với đầu vào`5 2`và chuỗi`cabad`, số đếm là`a:2`,`b:1`,`c:1`,`d:1`, do đó có ba ký tự tần số lẻ. Hai palindromes không thể cung cấp ba trung tâm riêng biệt, vì vậy câu trả lời là`NO`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng xây dựng`m`từng nhóm một. Chúng ta có thể chọn các tập hợp con của các ký tự, kiểm tra xem mỗi tập hợp con có thể tạo thành một bảng màu hay không và tìm kiếm một phân vùng hợp lệ. Điều này đúng vì mọi sự phân chia có thể có của các ký tự cuối cùng sẽ được xem xét. Tuy nhiên, số lượng phân vùng có thể tăng lên một cách bùng nổ. Thậm chí chỉ với`n`các ký tự, số cách để chia chúng thành các nhóm vượt xa những gì có thể khám phá được.`n = 2 * 10^5`. 

Quan sát hữu ích là trở ngại duy nhất đến từ số ký tự lẻ. Mỗi palindrome cần tất cả các ký tự ngoại trừ một ký tự có thể được ghép nối. Nếu toàn bộ chuỗi có`odd`các ký tự có tần số lẻ, những ký tự đó`odd`các nhân vật phải trở thành trung tâm của các palindrome khác nhau. Điều này mang lại giới hạn dưới mà chúng ta cần ít nhất`odd`palindrome. 

Bây giờ hãy xem xét hướng ngược lại. Nếu chúng ta có ít nhất`odd`các nhóm có sẵn, chúng ta có thể tạo các bảng màu đó xung quanh các ký tự lẻ. Bất kỳ nhóm bổ sung nào cũng có thể được tạo bằng cách tách các bảng màu hiện có hoặc bằng cách sử dụng các ký tự đơn. Vì mỗi ký tự đều là một bảng màu và`m`không bao giờ lớn hơn`n`, luôn có đủ ký tự để tiếp cận chính xác`m`các nhóm. 

Vấn đề được rút gọn thành việc đếm xem có bao nhiêu tần số ký tự là số lẻ và so sánh số đó với`m`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`n`| số mũ trong`n`| Quá chậm | 
| Tối ưu | O(n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần mỗi chữ cái viết thường xuất hiện trong chuỗi. Mảng tần số lưu trữ thông tin duy nhất cần thiết vì việc sắp xếp chuỗi gốc không liên quan. 
2. Đếm xem có bao nhiêu chữ cái có tần số lẻ. Mỗi chữ cái như vậy yêu cầu một bảng màu riêng biệt chứa nó làm ký tự trung tâm. Nếu một palindrome có hai ký tự tần số lẻ khác nhau thì một trong số chúng sẽ không có vị trí trùng khớp. 
3. So sánh số tần số lẻ với`m`. Nếu số lượng tâm yêu cầu lớn hơn số lượng palindromes có sẵn, xuất ra`NO`. Nếu không thì xuất ra`YES`. 

Tại sao nó hoạt động: số lượng ký tự tần số lẻ chính xác là số lượng bảng màu tối thiểu được yêu cầu. Mỗi palindrome chỉ có thể đóng góp nhiều nhất một số lẻ, do đó không thể có nhiều nhóm ít hơn số đó. Nếu có đủ nhóm, chúng ta có thể gán cho mỗi ký tự có tần số lẻ ở trung tâm riêng và chia các ký tự còn lại thành các nhóm đối xứng bổ sung. Vì thế điều kiện`odd <= m`là vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())
        s = input().strip()

        freq = [0] * 26
        for ch in s:
            freq[ord(ch) - ord('a')] += 1

        odd = 0
        for value in freq:
            if value % 2:
                odd += 1

        if odd <= m:
            ans.append("YES")
        else:
            ans.append("NO")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm và xử lý từng chuỗi một cách độc lập. Mảng tần số có kích thước cố định là 26 vì bảng chữ cái được giới hạn ở các chữ cái tiếng Anh viết thường. 

Sau khi đếm tần số, vòng lặp trên mảng sẽ xác định các ký tự không thể ghép nối hoàn toàn. Biến`odd`đại diện cho số lượng palindromes tối thiểu cần thiết. Sự so sánh với`m`là toàn bộ bước quyết định. 

Không có rủi ro về lập chỉ mục vì mọi ký tự đều được chuyển đổi trực tiếp thành chỉ mục từ`0`ĐẾN`25`. Số nguyên Python cũng tránh được vấn đề tràn, mặc dù số lượng lớn nhất có thể chỉ là`200000`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 3
ababcab
```Các tần số là`a = 3`,`b = 3`,`c = 1`. 

| Bước | Số lượng ký tự | Số lẻ | Quyết định | 
| --- | --- | --- | --- | 
| Sau khi đếm | a:3, b:3, c:1 | 3 | Cần 3 palindromes | 
| So sánh với m | m = 3 | 3 <= 3 | CÓ | 

Ba ký tự tần số lẻ có thể trở thành trung tâm của một bảng màu. Một sự phân chia có thể là`aba`,`bcb`, Và`a`. 

### Ví dụ 2 

đầu vào:```
5 2
cabad
```Các tần số là`a = 2`,`b = 1`,`c = 1`,`d = 1`. 

| Bước | Số lượng ký tự | Số lẻ | Quyết định | 
| --- | --- | --- | --- | 
| Sau khi đếm | a:2, b:1, c:1, d:1 | 3 | Cần ít nhất 3 palindromes | 
| So sánh với m | m = 2 | 3 > 2 | KHÔNG | 

Hai palindromes không thể cung cấp đủ vị trí trung tâm cho`b`,`c`, Và`d`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được tính một lần và quá trình quét bảng chữ cái chỉ mất 26 thao tác. | 
| Không gian | O(1) | Mảng tần số luôn chứa chính xác 26 giá trị. | 

Tổng chiều dài của tất cả các chuỗi được giới hạn bởi`2 * 10^5`, do đó quá trình quét tuyến tính dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# provided samples
assert run("""3
7 3
ababcab
5 2
cabad
6 6
vkvkvk
""") == """YES
NO
YES
""", "samples"

# minimum size
assert run("""1
1 1
a
""") == """YES
""", "single character"

# all characters have odd frequency but enough palindromes exist
assert run("""1
3 3
abc
""") == """YES
""", "all single-character palindromes"

# too many odd frequencies
assert run("""1
4 2
abcd
""") == """NO
""", "not enough centers"

# all equal characters
assert run("""1
10 4
aaaaaaaaaa
""") == """YES
""", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7 3 / ababcab`|`YES`| Trường hợp bình thường khi tần số lẻ khớp chính xác`m`. | 
|`1 1 / a`|`YES`| Chuỗi nhỏ nhất có thể. | 
|`3 3 / abc`|`YES`| Mỗi nhân vật có thể tạo thành bảng màu riêng của mình. | 
|`4 2 / abcd`|`NO`| Nhiều tần số lẻ hơn các nhóm có sẵn. | 
|`10 4 / aaaaaaaaaa`|`YES`| Tần số chẵn và chia thành nhiều palindromes. | 

## Vỏ cạnh 

cho`abc`với`m = 3`, mỗi ký tự có tần số là một, nên số lẻ là ba. Thuật toán thấy rằng`3 <= 3`và trả về`YES`. Cấu trúc gồm ba bảng màu một chữ cái:`a`,`b`, Và`c`. 

Vì`cabad`với`m = 2`, thuật toán đếm ba tần số lẻ:`b`,`c`, Và`d`. Vì hai palindrome không thể có ba tâm nên nó trả về`NO`. 

Đối với một chuỗi trong đó tất cả các ký tự đều giống nhau, chẳng hạn như`aaaaaaaaaa`với`m = 4`, số lẻ bằng 0 vì tần số là số chẵn. Thuật toán trả về`YES`, vì các ký tự có thể được chia thành nhiều bảng màu như`aaa`,`aaa`,`aa`, Và`aa`. 

Đối với trường hợp số lẻ chính xác bằng`m`, chẳng hạn như`ababcab`với`m = 3`, thuật toán không cố gắng xây dựng các palindromes. Nó chỉ xác minh số lượng trung tâm cần thiết, đủ bằng chứng minh ở trên.

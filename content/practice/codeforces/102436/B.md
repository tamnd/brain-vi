---
title: "CF 102436B - Giảm thiểu Trie"
description: "Chúng ta được cấp một tập hợp các chuỗi chữ thường. Chúng tôi có thể thay thế các chữ cái riêng lẻ, mỗi lần thay thế sẽ tốn một thao tác. Sau tất cả các lần thay thế, chúng ta xây dựng một bộ ba thông thường từ các chuỗi kết quả. Mục tiêu không phải là trực tiếp giảm thiểu số lượng thay thế."
date: "2026-08-09T00:12:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102436
codeforces_index: "B"
codeforces_contest_name: "Innopolis Open 2019-2020, qualification, contest 1"
rating: 0
weight: 102436
solve_time_s: 81
verified: true
draft: false
---

[CF 102436B - Giảm thiểu Trie](https://codeforces.com/problemset/problem/102436/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các chuỗi chữ thường. Chúng tôi có thể thay thế các chữ cái riêng lẻ, mỗi lần thay thế sẽ tốn một thao tác. Sau tất cả các lần thay thế, chúng ta xây dựng một bộ ba thông thường từ các chuỗi kết quả. Mục tiêu không phải là trực tiếp giảm thiểu số lượng thay thế. Thay vào đó, trước tiên chúng tôi muốn trie kết quả có số lượng nút nhỏ nhất có thể và trong số tất cả các phép biến đổi đạt được kích thước trie tối thiểu đó, chúng tôi muốn biến đổi yêu cầu ít thay thế nhất. 

Thực tế cấu trúc quan trọng là việc thay thế các chữ cái không làm thay đổi độ dài chuỗi. Giả sử chuỗi dài nhất có độ dài L. Bất kỳ trie nào chứa chuỗi có độ dài L phải chứa ít nhất một đỉnh ở mọi độ sâu từ 0 đến L, trong đó độ sâu bằng 0 là gốc. Do đó không có trie nào có thể có ít hơn L+1 đỉnh. Chúng ta luôn có thể đạt được chính xác các đỉnh L+1 bằng cách làm cho mọi chuỗi sử dụng cùng một ký tự ở mọi vị trí mà chuỗi đó tồn tại. Trie kết quả chỉ là một chuỗi, với các chuỗi ngắn hơn kết thúc ở các đỉnh trung gian. 

Vì vậy, vấn đề tối ưu hóa thực tế trở nên đơn giản hơn nhiều. Tại vị trí j, chỉ xét các chuỗi có độ dài lớn hơn j. Chúng tôi chọn một ký tự mà tất cả các chuỗi đó sẽ sử dụng ở vị trí đó. Nếu một ký tự xuất hiện k lần trong số các chuỗi này, thì việc chọn nó đòi hỏi phải thay đổi mọi ký tự khác, do đó chi phí là số chuỗi hoạt động trừ k. Sự lựa chọn tốt nhất là nhân vật thường xuyên nhất. 

Các ràng buộc làm cho một giải pháp gần tuyến tính trở nên cần thiết. Có thể có tối đa 100000 chuỗi, các chuỗi riêng lẻ có thể có độ dài 100000 và tổng số ký tự tối đa là 1000000. Một giải pháp tỷ lệ thuận với tổng kích thước đầu vào là lý tưởng. Ngay cả hệ số không đổi thêm 26 cũng vô hại, vì bảng chữ cái chỉ chứa 26 chữ cái tiếng Anh viết thường. Việc quét bậc hai trên tất cả các cặp chuỗi hoặc bất kỳ phương pháp nào liệt kê các chuỗi biến đổi có thể xảy ra là hoàn toàn không thực tế. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến xử lý sai. Đầu tiên, một chuỗi đã có bộ ba tối thiểu có thể, do đó không cần thay thế.```
1
abc
```Đầu ra đúng là`0`. Không có gì để hợp nhất vì trie đã là một chuỗi. 

Trường hợp thứ hai liên quan đến các chuỗi có độ dài khác nhau. Tại vị trí không tồn tại trong chuỗi ngắn hơn, chuỗi đó không được tham gia tính toán tần số.```
3
a
ab
bb
```Ở vị trí 0, các ký tự được`a, a, b`, do đó việc thay đổi`b`ĐẾN`a`tốn một cái. Ở vị trí một, chỉ`ab`Và`bb`tồn tại và cả hai đều đã chứa`b`, do đó chi phí bằng không. Câu trả lời đúng là`1`. Việc đếm mọi chuỗi đầu vào ở mọi vị trí sẽ bao gồm chuỗi một ký tự ở vị trí một không chính xác. 

Trường hợp cạnh thứ ba là ràng buộc tần số.```
2
ab
ba
```Ở vị trí số 0,`a`Và`b`mỗi lần xảy ra một lần nên việc thay thế một lần là điều không thể tránh khỏi. Điều tương tự cũng xảy ra ở vị trí một. Câu trả lời là`2`. Ký tự cụ thể được chọn trong một trận hòa không ảnh hưởng đến số lần thay thế. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ trực tiếp để suy nghĩ về vấn đề là quyết định nhân vật cuối cùng một cách độc lập cho mọi vị trí. Đối với mỗi vị trí, chúng ta có thể thử tất cả 26 chữ cái có thể và đếm xem cần thay đổi bao nhiêu chuỗi hoạt động. Nếu tổng độ dài đầu vào là S, việc quét lại các ký tự liên quan cho mọi chữ cái có thể sẽ mất 26S kiểm tra ký tự trong trường hợp xấu nhất. Với S=1000000, tức là 26000000 lần kiểm tra trước khi tính đến cấu trúc dữ liệu và chi phí Python. Dưới giới hạn một giây, đây là khối lượng công việc không cần thiết, đặc biệt khi thông tin tần số có thể được thu thập trực tiếp. 

Ngoài ra còn có một cách giải thích bạo lực tồi tệ hơn nhiều, trong đó chúng tôi liệt kê mọi chuỗi cuối cùng có thể có độ dài tối đa. Điều đó đòi hỏi phải xem xét tới 26 lựa chọn L, điều này trở nên không thể thực hiện được ngay cả đối với L rất nhỏ. Lý do việc liệt kê này là không cần thiết là vì các lựa chọn ở các vị trí khác nhau không tương tác với nhau. Việc thay thế ký tự ở vị trí j không ảnh hưởng đến ký tự nào xuất hiện ở vị trí j+1, vì việc thay thế không bao giờ chèn hoặc xóa ký tự. 

Cái nhìn sâu sắc quan trọng là xem xét từng cấp độ. Ở độ sâu j, mỗi chuỗi dài hơn j đóng góp chính xác một cạnh từ tiền tố độ sâu j của nó cho ký tự tiếp theo. Nếu hai chuỗi như vậy sử dụng các ký tự khác nhau ở đó thì trie phải phân nhánh. Vì giới hạn dưới tuyệt đối là một đỉnh trên mỗi độ sâu nên trie nhỏ nhất thu được chính xác khi chỉ có một ký tự được sử dụng ở mỗi độ sâu. 

Khi điều đó được nhận ra, việc tối ưu hóa ở mọi độ sâu sẽ độc lập. Nếu có c a ​ ,c b ​ ,…,cz ​ xuất hiện của 26 chữ cái tại vị trí đó thì chọn chữ x có giá 

(số chuỗi hoạt động)−c x ​ . 

Mức tối thiểu đạt được bằng cách tối đa hóa c x ​. Vì vậy, sự đóng góp của một vị trí chỉ đơn giản là 

chuỗi hoạt động−tần số tối đa. 

Chúng ta có thể tích lũy các tần số này trong khi đọc chuỗi. Giải pháp tham chiếu chính thức sử dụng chính xác đối số tần số trên mỗi vị trí này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên 26 lựa chọn | O(26S) | O(S) | Chậm một cách không cần thiết | 
| Liệt kê các chuỗi cuối cùng | O(26 L ) | Hàm mũ | Không thể | 
| Đếm tần số tối ưu | O(S+26L) | O(26L) | Đã chấp nhận | 

Ở đây S là tổng chiều dài của tất cả các chuỗi đầu vào và L là độ dài chuỗi tối đa. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các chuỗi và duy trì một mảng tần số cho mọi vị trí chuỗi. Đối với vị trí j, mảng có 26 bộ đếm, một bộ đếm cho mỗi chữ cái viết thường. Khi một chuỗi chứa ký tự`c`tại vị trí j, tăng bộ đếm tương ứng. Chúng tôi chỉ tạo các vị trí thực sự xảy ra trong một số chuỗi, vì vậy các chuỗi ngắn hơn đương nhiên sẽ ngừng đóng góp. 
2. Sau khi tất cả các chuỗi đã được xử lý, hãy kiểm tra từng vị trí một cách độc lập. Tổng 26 bộ đếm của nó là số chuỗi đạt đến vị trí đó. Đây không nhất thiết phải là n, vì các chuỗi ngắn hơn vị trí hiện tại sẽ không xuất hiện trong quá trình thử ở độ sâu đó. 
3. Tìm tần số lớn nhất trong số 26 chữ cái ở vị trí đó. Việc chọn chữ cái đó cho lần thử cuối cùng sẽ tiết kiệm chính xác số lượng thay thế đó, bởi vì những chuỗi đó đã chứa ký tự mong muốn. 
4. Thêm`active - best`để trả lời. Mỗi chuỗi hoạt động khác có một ký tự khác nhau và do đó cần chính xác một chuỗi thay thế ở vị trí này. 
5. In câu trả lời tích lũy. Không cần thiết phải xây dựng chuỗi kết quả hoặc bộ ba vì chỉ có số lượng thay thế mới quan trọng. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ vị trí j. Vì việc thay thế các chữ cái không làm thay đổi độ dài nên mỗi chuỗi dài hơn j vẫn phải đóng góp một ký tự ở vị trí đó. Để bộ ba có số lượng nút tối thiểu có thể, chỉ có một đỉnh bộ ba ở độ sâu j+1, vì vậy tất cả các chuỗi hoạt động phải có cùng một ký tự ở đó. 

Giả sử ký tự đó là x. Mỗi chuỗi hoạt động có ký tự gốc tại j không phải là x yêu cầu một thay thế, trong khi mọi chuỗi đã chứa x không yêu cầu thay thế nào. Như vậy chi phí chính xác là`active - count[x]`. Sự lựa chọn rẻ nhất là nhân vật có tần số tối đa. 

Đối số này áp dụng độc lập ở mọi vị trí. Việc chọn ký tự ở một vị trí không thể thay đổi ký tự hoặc độ dài ở vị trí khác, do đó việc giảm thiểu từng vị trí riêng biệt sẽ giảm thiểu tổng số lần thay thế. Các chuỗi kết quả chia sẻ cùng một tiền tố ở mọi độ sâu, tạo ra một chuỗi duy nhất và do đó có kích thước trie tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    # counts[j][c] = number of strings having character c at position j
    counts = []

    for _ in range(n):
        s = input().strip()

        while len(counts) < len(s):
            counts.append([0] * 26)

        for j, ch in enumerate(s):
            counts[j][ord(ch) - ord('a')] += 1

    ans = 0

    for row in counts:
        active = sum(row)
        best = max(row)
        ans += active - best

    print(ans)

if __name__ == "__main__":
    solve()
```các`counts`mảng đại diện cho thông tin cần thiết cho toàn bộ quá trình tối ưu hóa.`counts[j]`chứa tần số của mỗi chữ cái ở vị trí`j`. các`while`vòng lặp chỉ phát triển cấu trúc khi chuỗi mới đọc dài hơn tất cả các chuỗi trước đó. 

Đối với mỗi ký tự trong mỗi chuỗi đầu vào, một bộ đếm sẽ tăng lên. Đây chính xác là việc thu thập dữ liệu được mô tả trong bước thuật toán đầu tiên và không cần phải xây dựng thử. 

Sau khi xử lý đầu vào,`active = sum(row)`chỉ đếm các chuỗi thực sự có ký tự ở vị trí đó. Điều này tự động xử lý các độ dài chuỗi khác nhau.`best = max(row)`chọn ký tự đã xuất hiện thường xuyên nhất. Sự khác biệt là số lượng thay thế tối thiểu cần thiết ở độ sâu đó. 

Không có vấn đề tràn số nguyên trong Python. Ngay cả trong ngôn ngữ có chiều rộng cố định, câu trả lời nhiều nhất là tổng chiều dài đầu vào, tối đa là 1000000. Việc triển khai cũng tránh sắp xếp, băm các chuỗi riêng lẻ hoặc xây dựng các nút trie, giữ cho vòng lặp nóng tỷ lệ thuận với kích thước đầu vào. 

Hàng 26 phần tử cho mỗi vị trí có kích thước nhỏ do bảng chữ cái được cố định. Dung lượng lưu trữ 26L thu được nhiều nhất là 2,6 triệu bộ đếm dưới độ dài tối đa đã nêu. 

## Ví dụ đã hoạt động 

Mẫu chính thức là:```
4
min
trie
task
mini
```Độ dài tối đa là bốn, do đó các vị trí được lập chỉ mục từ 0 đến 3. 

| Vị trí | Nhân vật hoạt động | Tần số | Tốt nhất | Đóng góp | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | m, t, t, m | m:2, t:2 | 2 | 2 | 2 | 
| 1 | tôi, r, một, tôi | i:2, r:1, a:1 | 2 | 2 | 4 | 
| 2 | n, tôi, s, n | n:2, i:1, s:1 | 2 | 2 | 6 | 
| 3 | e, k, tôi | e:1, k:1, i:1 | 1 | 2 | 8 | 

Ở ba vị trí đầu tiên, hai chuỗi đã phù hợp với ký tự tốt nhất và hai chuỗi cần thay đổi. Ở vị trí cuối cùng chỉ tồn tại ba chuỗi và cả ba ký tự đều khác nhau nên cần có hai chuỗi thay thế. 

Một kết quả tối ưu có thể là`min`,`mine`,`mine`,`mine`, như thể hiện trong lời giải thích chính thức. Trie kết quả là một chuỗi chứa năm nút bao gồm cả nút gốc, đây là mức tối thiểu có thể có đối với một chuỗi dài nhất có độ dài bốn. 

Ví dụ thứ hai minh họa các độ dài khác nhau.```
3
a
ab
bb
```| Vị trí | Nhân vật hoạt động | Tần số | Tốt nhất | Đóng góp | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | a, a, b | a:2, b:1 | 2 | 1 | 1 | 
| 1 | b, b | b:2 | 2 | 0 | 1 | 

Ở vị trí 0, thay đổi`bb`vào trong`ab`tốn một lần thay thế. Ở vị trí một, chuỗi một ký tự`a`không tồn tại ở độ sâu này và không được tính. Hai chuỗi còn lại đã thống nhất rồi`b`, do đó không cần thay thế bổ sung. 

Chuỗi cuối cùng có thể là`a`,`ab`,`ab`, tri của nó là một chuỗi. Câu trả lời là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S+26L) | Mỗi ký tự đầu vào được xử lý một lần, sau đó mỗi hàng 26 bộ đếm sẽ được quét | 
| Không gian | O(26L) | Một mảng 26 bộ đếm được lưu trữ cho mọi vị trí có thể | 

Ở đây S<1000000 và L<100000. Thuật toán chỉ thực hiện một số thao tác cho mỗi ký tự đầu vào cộng với tối đa 2,6 triệu lượt kiểm tra bộ đếm, do đó, thuật toán này phù hợp một cách thoải mái với cách tiếp cận quy mô tuyến tính dự định trong giới hạn thời gian một giây. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới sử dụng logic giống như giải pháp đã gửi nhưng hiển thị nó dưới dạng một hàm để có thể kiểm tra từng trường hợp bằng`assert`.```python
import io
import sys

def solve_data(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    counts = []

    for _ in range(n):
        s = next(it)

        while len(counts) < len(s):
            counts.append([0] * 26)

        for j, ch in enumerate(s):
            counts[j][ord(ch) - ord('a')] += 1

    ans = 0

    for row in counts:
        ans += sum(row) - max(row)

    return str(ans)

# Provided sample
assert solve_data(
    """4
min
trie
task
mini
"""
) == "8", "sample 1"

# Minimum-size input
assert solve_data(
    """1
a
"""
) == "0", "single string needs no replacement"

# All strings are already identical
assert solve_data(
    """4
abc
abc
abc
abc
"""
) == "0", "all strings already form a chain"

# Different lengths, shorter strings must not affect later positions
assert solve_data(
    """3
a
ab
bb
"""
) == "1", "short strings must be ignored at deeper positions"

# Tie at every position
assert solve_data(
    """2
ab
ba
"""
) == "2", "ties require one replacement at each position"

# Maximum-size shape: 100000 strings of length 1
# 50000 are 'a', 50000 are 'b', so exactly 50000 replacements are needed.
inp = "100000\n" + "a\n" * 50000 + "b\n" * 50000
assert solve_data(inp) == "50000", "maximum-size input"

| Test input | Expected output | What it validates |
|---|---:|---|
| `1 / a` | `0` | Minimum-size input and already optimal trie |
| Four copies of `abc` | `0` | All-equal strings |
| `a`, `ab`, `bb` | `1` | Different lengths and inactive positions |
| `ab`, `ba` | `2` | Frequency ties and per-position independence |
| 100000 one-character strings, half `a`, half `b` | `50000` | Maximum input size and linear processing |

## Edge Cases

For a single string such as

```văn bản 
1 
abc```

there is only one path in the trie. At position zero the only active character is `a`, so the contribution is zero. The same holds for positions one and two. The algorithm returns `0`, correctly recognizing that no branching exists.

For strings of different lengths,

```3 
một 
bụng 
bb```

the first position contains `a, a, b`, giving a contribution of `1`. At the second position, only `ab` and `bb` remain active. Both contain `b`, so the contribution is zero and the final answer is `1`. The implementation handles this because it increments a counter only when the current string actually has that position.

For tied frequencies,

```2 
bụng 
ba```

position zero has one `a` and one `b`, so whichever final character we choose, one replacement is necessary. Position one has the same situation. The answer is `2`. The algorithm only needs the maximum frequency, so ties require no special handling.

For strings whose lengths reach the maximum allowed value, such as many strings of length 100000, the algorithm does not create trie nodes or compare strings against each other. It records one counter update per character and later scans 26 counters per position. This keeps the work bounded by the total input size plus a small alphabet factor.

The most common conceptual mistake is to optimize the trie by thinking about complete strings rather than trie depths. The example

```3 
một 
bụng 
bb 
``` 

cho thấy tại sao điều đó thất bại. Chuỗi`a`quan trọng ở độ sâu 0 nhưng biến mất khỏi thử nghiệm ở độ sâu 1. Khi vấn đề được xem xét theo cấp độ, tính độc lập của các vị trí sẽ trở nên rõ ràng và giải pháp giảm xuống việc đếm tần số.

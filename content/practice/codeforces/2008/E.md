---
title: "CF 2008E - Chuỗi xen kẽ"
description: "Chúng ta được cấp một chuỗi các chữ cái viết thường. Một chuỗi xen kẽ có một hình thức rất cụ thể. Tất cả các ký tự ở chỉ số chẵn phải giống nhau, tất cả các ký tự ở chỉ số lẻ phải giống nhau và độ dài cuối cùng phải là số chẵn."
date: "2026-06-08T13:26:54+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 2008
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 970 (Div. 3)"
rating: 1500
weight: 2008
solve_time_s: 124
verified: true
draft: false
---

[CF 2008E - Chuỗi thay thế](https://codeforces.com/problemset/problem/2008/E) 

**Đánh giá:** 1500 
**Tags:** bạo lực, cấu trúc dữ liệu, dp, tham lam, triển khai, chuỗi 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ cái viết thường. Một chuỗi xen kẽ có một hình thức rất cụ thể. 

Tất cả các ký tự ở chỉ số chẵn phải giống nhau, tất cả các ký tự ở chỉ số lẻ phải giống nhau và độ dài cuối cùng phải là số chẵn. 

Hai nhóm chẵn lẻ có thể sử dụng cùng một chữ cái hoặc các chữ cái khác nhau. Ví dụ,`"aaaa"`xen kẽ vì mọi vị trí chẵn đều chứa`'a'`và mọi vị trí lẻ cũng chứa`'a'`. 

Chúng tôi có thể thực hiện bất kỳ số lần thay thế nào, trong đó một ký tự được thay đổi thành bất kỳ chữ cái nào khác. Ngoài ra, chúng ta có thể xóa nhiều nhất một ký tự khỏi chuỗi. Mục tiêu là giảm thiểu tổng số hoạt động. 

Đầu vào chứa tối đa$10^4$các trường hợp thử nghiệm và tổng độ dài của tất cả các chuỗi nhiều nhất là$2 \cdot 10^5$. Giới hạn tổng hợp này là hạn chế thực sự. Bất kỳ giải pháp bậc hai nào về độ dài của trường hợp thử nghiệm sẽ trở nên quá chậm trên một chuỗi có độ dài$2 \cdot 10^5$. Chúng ta cần thứ gì đó xung quanh$O(n \cdot 26)$hoặc$O(n \log n)$cho mỗi trường hợp thử nghiệm, vì kích thước bảng chữ cái chỉ là 26. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một chuỗi ký tự đơn:```
1
a
```Câu trả lời là 1. Chúng ta phải xóa ký tự duy nhất, tạo ra chuỗi trống, là một chuỗi xen kẽ có độ dài chẵn. Giải pháp giả sử chuỗi cuối cùng không được trống sẽ không thành công. 

Coi như:```
3
abc
```Chiều dài là số lẻ. Việc xóa một lần là bắt buộc vì độ dài cuối cùng phải chẵn. Giải pháp chỉ tính số lần thay thế mà không xem xét việc xóa sẽ trả về sai 2, trong khi câu trả lời đúng là 1. 

Một trường hợp tế nhị khác là:```
5
ababa
```Xóa ký tự đầu tiên mang lại`"baba"`, vốn đã xen kẽ. Câu trả lời là 1. Một chiến lược tham lam luôn xóa nhân vật gây ra nhiều sự không phù hợp nhất có thể dễ dàng bỏ lỡ điều này. 

Cạm bẫy lớn nhất đến từ sự thay đổi tính chẵn lẻ sau khi xóa. TRONG```
5
aabaa
```xóa ký tự ở giữa sẽ thay đổi tính chẵn lẻ của mọi ký tự ở bên phải của nó. Việc xử lý phần bên trái và bên phải một cách độc lập mà không tính đến sự dịch chuyển này sẽ tạo ra số đếm tần số không chính xác. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta hãy tưởng tượng giải pháp brute-force. 

Nếu độ dài là chẵn thì chuỗi đích phải có cùng độ dài. Chúng ta chọn một chữ cái cho vị trí chẵn và một chữ cái cho vị trí lẻ. chỉ có$26^2 = 676$khả năng. Đối với mỗi cặp, chúng tôi đếm xem có bao nhiêu vị trí đã khớp và cần bao nhiêu vị trí thay thế. 

Nếu độ dài là số lẻ thì phải thực hiện đúng một lần xóa. Chúng tôi có thể thử mọi vị trí xóa. Sau khi xóa một ký tự, chúng tôi lại thử tất cả 676 cặp ký tự chẵn lẻ và tính chi phí thay thế. 

Ý tưởng bạo lực là hoàn toàn chính xác. Khó khăn là tốc độ. Đối với một chuỗi có độ dài$n$, thử tất cả các vị trí xóa và quét toàn bộ chuỗi mỗi lần tốn$O(n^2)$. Với$n = 2 \cdot 10^5$, điều này thật vô vọng. 

Quan sát quan trọng là một chuỗi xen kẽ chỉ quan tâm đến các lớp chẵn lẻ. Đối với một chuỗi có độ dài chẵn, khi chúng ta chọn chữ cái được sử dụng ở vị trí chẵn và chữ cái được sử dụng ở vị trí lẻ, mọi vị trí sẽ trở nên độc lập. 

Giả sử độ dài chuỗi đã chẵn. Cho phép:

-`even[c]`là số lần xuất hiện của chữ cái`c`ở các vị trí chẵn. 
-`odd[c]`là số lần xuất hiện của chữ cái`c`ở các vị trí lẻ. 

Nếu chọn chữ`x`cho các vị trí và chữ cái chẵn`y`đối với các vị trí lẻ thì số vị trí đã đúng là:$$even[x] + odd[y]$$Để tối đa hóa các ký tự được giữ lại, chúng tôi chỉ cần chọn chữ cái thường xuyên nhất ở vị trí chẵn và chữ cái thường xuyên nhất ở vị trí lẻ. Số thay thế tối thiểu trở thành:$$n - \max(even) - \max(odd)$$Trường hợp có độ dài lẻ thú vị hơn. 

Sau khi xóa vị trí$i$, ký tự trước$i$giữ tính chẵn lẻ của chúng, trong khi các ký tự sau$i$chuyển đổi tính chẵn lẻ vì mọi thứ đều dịch chuyển sang trái một vị trí. 

Điều này gợi ý việc duy trì số lượng tần số ở cả hai phía của điểm xóa. 

Cho phép:

-`pref_even`,`pref_odd`đếm các chữ cái trước khi xóa. 
-`suf_even`,`suf_odd`đếm các chữ cái từ vị trí hiện tại trở đi. 

Khi chúng tôi xóa vị trí$i$: 

- ký tự trước$i$đóng góp bình thường; 
- ký tự sau$i$hoán đổi các lớp chẵn lẻ. 

Đối với mỗi chữ cái, chúng ta có thể tính toán số lần nó xuất hiện ở vị trí chẵn mới và vị trí lẻ mới sau khi xóa. 

Sau đó, mục tiêu xen kẽ tốt nhất lại giữ lại chữ cái phổ biến nhất trong mỗi lớp chẵn lẻ. 

Bởi vì kích thước bảng chữ cái chỉ có 26, việc đánh giá một vị trí xóa sẽ tốn kém$O(26)$. Quét tất cả các chi phí vị trí$O(26n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot 26^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \cdot 26)$|$O(26)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chuỗi có độ dài chẵn 

1. Đếm tần số của mỗi chữ cái ở vị trí chẵn. 
2. Đếm tần số của mỗi chữ cái ở vị trí lẻ. 
3. Chọn chữ cái ở vị trí chẵn thường gặp nhất và chữ cái ở vị trí lẻ thường gặp nhất. 
4. Số ký tự được giữ nguyên bằng tổng của hai giá trị lớn nhất đó. 
5. Câu trả lời là`n - preserved`. 

Chuỗi xen kẽ mục tiêu được xác định hoàn toàn bằng một chữ cái cho mỗi lớp chẵn lẻ, do đó, việc tối đa hóa các ký tự được bảo toàn độc lập cho mỗi lớp chẵn lẻ là tối ưu. 

### Chuỗi có độ dài lẻ 

1. Xây dựng bảng tần số`suf_even`Và`suf_odd`cho toàn bộ chuỗi. 
2. Khởi tạo bảng tiền tố trống`pref_even`Và`pref_odd`. 
3. Lặp lại mọi vị trí`i`với tư cách là ứng cử viên xóa. 
4. Xóa`s[i]`khỏi bảng hậu tố thích hợp vì ký tự này đang bị xóa. 
5. Đối với mỗi chữ cái`c`, tính tần số của nó tại các vị trí chẵn:$$pref\_even[c] + suf\_odd[c]$$Các ký tự sau khi xóa chuyển đổi tính chẵn lẻ. 

1. Đối với mỗi chữ cái`c`, tính tần số của nó ở các vị trí lẻ thu được:$$pref\_odd[c] + suf\_even[c]$$1. Hãy để`best_even`Và`best_odd`là giá trị lớn nhất từ ​​các mảng đó. 
2. Độ dài chuỗi kết quả là`n - 1`. Số người thay thế cần thiết là:$$(n-1) - best\_even - best\_odd$$1. Thêm một thao tác nữa để xóa chính nó. 
2. Giữ câu trả lời tối thiểu cho tất cả các vị trí xóa. 
3. Di chuyển`s[i]`vào bảng tiền tố thích hợp và tiếp tục. 

### Tại sao nó hoạt động 

Đối với bất kỳ vị trí xóa cố định nào, mọi ký tự còn lại đều thuộc về lớp chẵn lẻ chẵn hoặc lẻ của chuỗi kết quả. Một chuỗi xen kẽ chỉ yêu cầu một chữ cái được chọn cho mỗi lớp. Lựa chọn tối ưu luôn là chữ cái thường xuyên nhất trong lớp đó vì mọi lần xuất hiện khác đều phải được thay thế. 

Việc phân tách tiền tố/hậu tố tính toán các lớp chẵn lẻ chính xác sau khi xóa. Các ký tự trước khi xóa giữ tính chẵn lẻ, trong khi các ký tự sau khi xóa sẽ hoán đổi tính chẵn lẻ. Mỗi ký tự còn lại được tính chính xác một lần trong lớp mới của nó. Vì mọi vị trí xóa đều được kiểm tra nên mức tối thiểu trên tất cả các vị trí là mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()

        if n % 2 == 0:
            even = [0] * 26
            odd = [0] * 26

            for i, ch in enumerate(s):
                idx = ord(ch) - 97
                if i % 2 == 0:
                    even[idx] += 1
                else:
                    odd[idx] += 1

            ans = n - max(even) - max(odd)
            print(ans)
            continue

        suf_even = [0] * 26
        suf_odd = [0] * 26

        for i, ch in enumerate(s):
            idx = ord(ch) - 97
            if i % 2 == 0:
                suf_even[idx] += 1
            else:
                suf_odd[idx] += 1

        pref_even = [0] * 26
        pref_odd = [0] * 26

        ans = n

        for i, ch in enumerate(s):
            idx = ord(ch) - 97

            if i % 2 == 0:
                suf_even[idx] -= 1
            else:
                suf_odd[idx] -= 1

            best_even = 0
            best_odd = 0

            for c in range(26):
                best_even = max(
                    best_even,
                    pref_even[c] + suf_odd[c]
                )
                best_odd = max(
                    best_odd,
                    pref_odd[c] + suf_even[c]
                )

            replacements = (n - 1) - best_even - best_odd
            ans = min(ans, replacements + 1)

            if i % 2 == 0:
                pref_even[idx] += 1
            else:
                pref_odd[idx] += 1

        print(ans)

solve()
```Nhánh có chiều dài chẵn thì đơn giản. Chúng tôi đếm tần số riêng biệt trên cả hai lớp chẵn lẻ và giữ lại chữ cái phổ biến nhất trong mỗi lớp. 

Nhánh có độ dài lẻ duy trì bốn mảng tần số. Mảng hậu tố ban đầu chứa số lượng từ toàn bộ chuỗi. Khi chúng ta quét từ trái sang phải, ký tự hiện tại sẽ bị xóa khỏi phía hậu tố vì nó được coi là đã bị xóa. 

Các công thức```
pref_even[c] + suf_odd[c]
pref_odd[c] + suf_even[c]
```mã hóa trao đổi chẵn lẻ sau khi xóa. Đây là chi tiết cốt lõi của giải pháp. Quên trao đổi là sai lầm phổ biến nhất. 

Câu trả lời cho vị trí xóa bao gồm hai phần. Đầu tiên chúng ta đếm số lần thay thế cần thiết trong chuỗi có độ dài kết quả`n - 1`. Sau đó, chúng tôi thêm một thao tác để xóa chính nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
ababa
```| tôi | Đã xóa | tốt nhất_thậm chí | tốt nhất_odd | Thay thế | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một | 2 | 2 | 0 | 1 | 
| 1 | b | 2 | 1 | 1 | 2 | 
| 2 | một | 2 | 2 | 0 | 1 | 
| 3 | b | 2 | 1 | 1 | 2 | 
| 4 | một | 2 | 2 | 0 | 1 | 

Tổng số tối thiểu là 1. Xóa bất kỳ`'a'`để lại một chuỗi xen kẽ ngay lập tức. 

### Ví dụ 2 

đầu vào:```
6
acdada
```Các vị trí chẵn có chứa:```
a d d
```Các vị trí lẻ có chứa:```
c a a
```| Chẵn lẻ | Tần số | Tối đa | 
| --- | --- | --- | 
| Thậm chí | a:1, d:2 | 2 | 
| lẻ | a:2, c:1 | 2 | 

Số ký tự được giữ nguyên là:$$2 + 2 = 4$$Câu trả lời là:$$6 - 4 = 2$$Hai sự thay thế là đủ, phù hợp với mẫu. 

Ví dụ này chứng tỏ rằng khi độ dài đã là số chẵn thì việc xóa không bao giờ hữu ích vì việc xóa sẽ làm cho độ dài trở thành số lẻ và vi phạm định nghĩa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot 26)$| Mỗi vị trí được xử lý một lần và các trường hợp có độ dài lẻ đánh giá 26 chữ cái | 
| Không gian |$O(26)$| Chỉ các mảng tần số có kích thước không đổi mới được lưu trữ | 

Vì tổng độ dài của tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, thuật toán thực hiện gần đúng$26 \cdot 2 \cdot 10^5$hoạt động cơ bản, dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    out = []

    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()

        if n % 2 == 0:
            even = [0] * 26
            odd = [0] * 26

            for i, ch in enumerate(s):
                idx = ord(ch) - 97
                if i % 2 == 0:
                    even[idx] += 1
                else:
                    odd[idx] += 1

            out.append(str(n - max(even) - max(odd)))
            continue

        suf_even = [0] * 26
        suf_odd = [0] * 26

        for i, ch in enumerate(s):
            idx = ord(ch) - 97
            if i % 2 == 0:
                suf_even[idx] += 1
            else:
                suf_odd[idx] += 1

        pref_even = [0] * 26
        pref_odd = [0] * 26

        ans = n

        for i, ch in enumerate(s):
            idx = ord(ch) - 97

            if i % 2 == 0:
                suf_even[idx] -= 1
            else:
                suf_odd[idx] -= 1

            be = 0
            bo = 0

            for c in range(26):
                be = max(be, pref_even[c] + suf_odd[c])
                bo = max(bo, pref_odd[c] + suf_even[c])

            ans = min(ans, (n - 1) - be - bo + 1)

            if i % 2 == 0:
                pref_even[idx] += 1
            else:
                pref_odd[idx] += 1

        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run("""10
1
a
2
ca
3
aab
5
ababa
6
acdada
9
ejibmyyju
6
bbccbc
6
abacba
5
bcbca
5
dcbdb
""") == """1
0
1
1
2
6
2
3
1
1"""

# minimum size
assert run("""1
1
a
""") == "1"

# already alternating even length
assert run("""1
4
abab
""") == "0"

# all equal odd length
assert run("""1
5
aaaaa
""") == "1"

# parity shift after deletion
assert run("""1
5
ababa
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, "a"`|`1`| Chuỗi trống sau khi xóa | 
|`n=4, "abab"`|`0`| Đã xen kẽ | 
|`n=5, "aaaaa"`|`1`| Chỉ xóa, không thay thế | 
|`n=5, "ababa"`|`1`| Xử lý đúng các ca chẵn lẻ | 

## Vỏ cạnh 

Hãy xem xét:```
1
a
```Độ dài chuỗi là số lẻ. Việc xóa ký tự duy nhất để lại một chuỗi có độ dài bằng 0. Trong quá trình quét, ứng cử viên xóa duy nhất mang lại kết quả`best_even = best_odd = 0`. Chi phí thay thế là 0 và việc thêm thao tác xóa sẽ là 1. Thuật toán trả về chính xác 1. 

Hãy xem xét:```
3
abc
```Xóa bất kỳ ký tự nào sẽ tạo ra một chuỗi có độ dài 2. Thuật toán đánh giá cả ba vị trí xóa và tính toán tần số chẵn lẻ tốt nhất sau khi dịch chuyển chẵn lẻ. Tổng chi phí tối thiểu là 1, đạt được bằng cách xóa`'b'`để có được`"ac"`, vốn đã xen kẽ. 

Coi như:```
5
ababa
```Khi xóa chỉ số 0, tất cả các ký tự còn lại sẽ dịch chuyển tính chẵn lẻ. Công thức$$pref\_even + suf\_odd$$di chuyển chính xác phía bên phải vào lớp chẵn lẻ đối diện. Số lượng kết quả cho thấy tất cả bốn ký tự còn lại đã khớp với mẫu xen kẽ, đưa ra câu trả lời 1. 

Hãy xem xét:```
5
aabaa
```Việc xóa ký tự ở giữa sẽ khiến mọi ký tự bên phải hoán đổi tính chẵn lẻ. Một tính toán tần số đơn giản mà bỏ qua sự thay đổi này sẽ đếm các chữ cái trong lớp sai và đánh giá quá cao số lượng thay thế. Công thức tiền tố/hậu tố thực hiện việc hoán đổi một cách rõ ràng và tạo ra chi phí tối thiểu chính xác.

---
title: "CF 102163E - Trình điều khiển Adnan và Burned"
description: "Chúng tôi duy trì một chuỗi các chữ cái viết thường có thể thay đổi. Bản cập nhật thay đổi một vị trí thành một ký tự được chỉ định. Truy vấn đưa ra một phạm vi ([l,r]) và chúng ta phải quyết định xem chuỗi con bên trong phạm vi đó có đọc giống hệt nhau từ cả hai hướng hay không."
date: "2026-08-19T07:46:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "E"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 151
verified: true
draft: false
---

[CF 102163E - Trình điều khiển Adnan và những kẻ bị đốt cháy](https://codeforces.com/problemset/problem/102163/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một chuỗi các chữ cái viết thường có thể thay đổi. Bản cập nhật thay đổi một vị trí thành một ký tự được chỉ định. Truy vấn đưa ra một phạm vi ([l,r]) và chúng ta phải quyết định xem chuỗi con bên trong phạm vi đó có đọc giống hệt nhau từ cả hai hướng hay không. 

Ví dụ: nếu chuỗi hiện tại là`abacaba`, phạm vi ([2,6]) chứa`bacab`, đó là một palindrome. Một phạm vi độ dài một luôn là một palindrome, bởi vì các biểu diễn tiến và lùi của nó chứa cùng một ký tự đơn. 

Có thể có tối đa (10^5) vị trí và (10^5) sự kiện trong một trường hợp thử nghiệm. Với nhiều sự kiện như vậy, việc kiểm tra từng ký tự trong mỗi truy vấn là quá tốn kém. Trong trường hợp xấu nhất, mỗi truy vấn (10^5) có thể kiểm tra (10^5) ký tự, đưa ra so sánh ký tự gần đúng (10^{10}). Điều đó vượt xa những gì giới hạn thời gian của một cuộc thi thông thường cho phép. Chúng tôi cần cả cập nhật và truy vấn palindrome gần với thời gian logarit. 

Trường hợp ranh giới đầu tiên là truy vấn một ký tự. Ví dụ:```
1
1 1
a
2 1 1
```Câu trả lời là:```
Adnan Wins
```Một thói quen so sánh giả định ít nhất hai ký tự có thể vô tình từ chối trường hợp này. 

Một sai lầm dễ mắc phải khác là quên rằng bản cập nhật có thể thay đổi một ký tự thành ký tự giống như ký tự đã có. Ví dụ:```
1
3 2
aba
1 2 b
2 1 3
```Câu trả lời là`Adnan Wins`. Bản cập nhật không thay đổi chuỗi nên trạng thái palindrome phải không thay đổi. Việc triển khai coi mọi cập nhật là thay đổi cấu trúc vẫn có thể đúng nhưng phải ghi đè giá trị được lưu trữ thay vì áp dụng điều chỉnh tăng dần không chính xác. 

Lỗi ranh giới phổ biến nhất xuất hiện khi khoảng được truy vấn chạm vào một trong hai đầu của chuỗi. Ví dụ:```
1
5 2
abcba
2 1 5
2 2 4
```Cả hai câu trả lời đều`Adnan Wins`. Bất kỳ triển khai nào sử dụng lập chỉ mục dựa trên 0 đều phải dịch phạm vi đầu vào bao gồm một cách cẩn thận, vì biểu diễn bên trong thường sẽ sử dụng các khoảng thời gian nửa mở. 

Cuối cùng, một chuỗi con có thể có số lượng ký tự phù hợp nhưng vẫn không phải là một chuỗi màu nhạt. Ví dụ,`aabb`chứa hai`a`nhân vật và hai`b`ký tự, nhưng nó không phải là một bảng màu. Một giải pháp dựa trên tần số sẽ chấp nhận nó một cách không chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là kiểm tra chuỗi con được truy vấn từ cả hai đầu. Đối với truy vấn ([l,r]), so sánh các ký tự tại (l) và (r), sau đó (l+1) và (r-1), tiếp tục cho đến khi hai con trỏ gặp nhau. Điều này đúng vì một chuỗi là một bảng màu chính xác khi mọi cặp đối xứng đều chứa các ký tự bằng nhau. 

Vấn đề là khối lượng công việc. Một truy vấn trên chuỗi con có độ dài (k) mất (O(k)) thời gian. Nếu chuỗi có (10^5) ký tự và chúng tôi thực hiện (10^5) truy vấn trên phạm vi độ dài gần bằng (10^5), thì trường hợp xấu nhất là so sánh khoảng (5 \times 10^9) ký tự. Cập nhật điểm không cải thiện tình trạng này. 

Quan sát hữu ích là một palindrome có trình tự giống hệt nhau khi đọc tiến và lùi. Thay vì so sánh từng ký tự đó, chúng ta có thể biểu thị toàn bộ chuỗi con bằng hàm băm cuộn. Chúng tôi duy trì hàm băm chuyển tiếp cho mọi phân đoạn và hàm băm ngược cho cùng một phân đoạn. Nếu hai giá trị băm bằng nhau thì chuỗi con được coi là một bảng màu. 

Cây phân đoạn là sự phù hợp tự nhiên vì các nút của nó đại diện cho các phần liền kề của chuỗi. Mỗi nút lưu trữ hàm băm của phân đoạn của nó từ trái sang phải và từ phải sang trái. Khi hai phân đoạn liền kề được nối với nhau, giá trị băm của chúng có thể được kết hợp trong thời gian không đổi bằng cách sử dụng lũy ​​thừa của cơ sở băm. 

Một cập nhật điểm chỉ ảnh hưởng đến các nút cây phân đoạn (O(\log N)) trên đường đi từ lá đã thay đổi tới gốc. Một truy vấn phạm vi sẽ truy cập các nút có liên quan (O(\log N)) và kết hợp các giá trị băm của chúng theo thứ tự ban đầu. Kết quả băm tiến và lùi sau đó có thể được so sánh. 

Việc so sánh hàm băm mang tính xác suất theo nghĩa hàm băm cuộn tiêu chuẩn. Việc sử dụng hai mô đun nguyên tố lớn khác nhau khiến cho khả năng xảy ra va chạm ngẫu nhiên là cực kỳ khó xảy ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N)) cho mỗi truy vấn, trường hợp xấu nhất (O(NE)) | (O(N)) | Quá chậm | 
| Cây phân đoạn + Hash kép | (O(\log N)) mỗi sự kiện | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước lũy thừa của modulo cơ sở băm cả hai số nguyên tố được chọn. Chúng ta cần (B^k) khi kết hợp một đoạn có độ dài (k) với một đoạn khác, do đó, việc tính toán các lũy thừa này một lần sẽ tránh được việc lũy thừa lặp lại. 
2. Xây dựng cây phân đoạn trên chuỗi hiện tại. Một chiếc lá đại diện cho một nhân vật. Đối với mỗi nút, hãy lưu trữ hàm băm thuận, hàm băm ngược và độ dài phân đoạn. Đối với một chiếc lá, cả hai giá trị băm chỉ đơn giản là giá trị số được gán cho ký tự của nó. 
3. Khi ghép con trái (A) và con phải (C), giả sử độ dài của chúng là (x) và (y). Nếu một hàm băm được định nghĩa là 
[ 
H(s)=\sum_{i=0}^{|s|-1} giá trị(s_i)B^i, 
] 
thì hàm băm chuyển tiếp của (AC) là 
[ 
H(A)+B^xH(C). 
] 
Hàm băm ngược được hình thành theo cách tương tự, nhưng các phần bên trái và bên phải bị đảo ngược xuất hiện theo thứ tự khái niệm ngược lại: 
[ 
RH(C)+B^yRH(A). 
] 
Cả hai công thức đều mất thời gian không đổi. 
4. Để cập nhật`1 i c`, chuyển đổi (i) sang quy ước lập chỉ mục của cây phân đoạn và thay thế lá tương ứng bằng giá trị của`c`. Tính toán lại mọi tổ tiên bằng cách sử dụng các công thức hợp nhất. Chỉ các nút (O(\log N)) thay đổi vì một điểm thuộc về một đường dẫn từ gốc đến lá. 
5. Đối với một truy vấn`2 l r`, truy xuất thông tin nút tổng hợp cho chính xác khoảng thời gian đó. Khi một vài phần được trả về, hãy ghép chúng theo thứ tự từ trái sang phải bằng cách sử dụng thao tác hợp nhất tương tự. Do đó, truy vấn tạo ra một hàm băm tiến và một hàm băm ngược cho chuỗi con hoàn chỉnh. 
6. So sánh hai giá trị băm thu được theo cả hai mô đun. Nếu cả hai khớp nhau, hãy in`Adnan Wins`; nếu không thì in`ARCNCD!`. Một palindrome có các chuỗi tiến và lùi giống hệt nhau, vì vậy các giá trị băm của chúng phải giống nhau. Với phép băm kép, khả năng không có bảng màu vượt qua cả hai phép so sánh là không đáng kể. 

Tại sao nó hoạt động: tính bất biến của cây phân đoạn là mọi nút đều lưu trữ chính xác hàm băm cuộn của chuỗi con được biểu thị của nó và hàm băm cuộn của chuỗi con đó được đảo ngược chính xác. Các công thức hợp nhất bảo toàn tính bất biến này khi hai đoạn liền kề được kết hợp. Các bản cập nhật điểm sẽ bảo vệ nó bằng cách xây dựng lại đường dẫn bị ảnh hưởng và các truy vấn phạm vi sẽ bảo vệ nó bằng cách nối các phân đoạn đã chọn theo thứ tự ban đầu của chúng. Do đó, hàm băm chuyển tiếp cuối cùng biểu thị chuỗi con được truy vấn và hàm băm ngược cuối cùng biểu thị chuỗi con đó theo chiều ngược lại. Giá trị băm bằng nhau có nghĩa là hai biểu diễn khớp nhau, chính xác là điều kiện palindrome, cho đến xác suất không đáng kể của xung đột băm kép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD1 = 1_000_000_007
MOD2 = 1_000_000_009
BASE = 911382323

def solve_case(n, q, s, queries):
    size = 1
    while size < n:
        size <<= 1

    # Powers used when concatenating hashes.
    pow1 = [1] * (n + 1)
    pow2 = [1] * (n + 1)

    for i in range(1, n + 1):
        pow1[i] = pow1[i - 1] * BASE % MOD1
        pow2[i] = pow2[i - 1] * BASE % MOD2

    total = size << 1

    # Forward hashes.
    hf1 = [0] * total
    hf2 = [0] * total

    # Reverse hashes.
    hr1 = [0] * total
    hr2 = [0] * total

    # Segment lengths.
    length = [0] * total

    def value(ch):
        return ord(ch) - 96

    # Leaves.
    for i, ch in enumerate(s):
        p = size + i
        v = value(ch)

        hf1[p] = v
        hf2[p] = v
        hr1[p] = v
        hr2[p] = v
        length[p] = 1

    # Padding leaves have length zero.
    for p in range(size - 1, 0, -1):
        left = p << 1
        right = left | 1

        ll = length[left]
        lr = length[right]
        length[p] = ll + lr

        hf1[p] = (hf1[left] + pow1[ll] * hf1[right]) % MOD1
        hf2[p] = (hf2[left] + pow2[ll] * hf2[right]) % MOD2

        hr1[p] = (hr1[right] + pow1[lr] * hr1[left]) % MOD1
        hr2[p] = (hr2[right] + pow2[lr] * hr2[left]) % MOD2

    def pull(p):
        left = p << 1
        right = left | 1

        ll = length[left]
        lr = length[right]
        length[p] = ll + lr

        hf1[p] = (hf1[left] + pow1[ll] * hf1[right]) % MOD1
        hf2[p] = (hf2[left] + pow2[ll] * hf2[right]) % MOD2

        hr1[p] = (hr1[right] + pow1[lr] * hr1[left]) % MOD1
        hr2[p] = (hr2[right] + pow2[lr] * hr2[left]) % MOD2

    def update(pos, ch):
        p = size + pos
        v = value(ch)

        hf1[p] = v
        hf2[p] = v
        hr1[p] = v
        hr2[p] = v
        length[p] = 1

        p >>= 1
        while p:
            pull(p)
            p >>= 1

    def merge(a, b):
        # Each item is:
        # (forward_hash_1, forward_hash_2,
        #  reverse_hash_1, reverse_hash_2, length)
        if a[4] == 0:
            return b
        if b[4] == 0:
            return a

        a1, a2, ar1, ar2, la = a
        b1, b2, br1, br2, lb = b

        return (
            (a1 + pow1[la] * b1) % MOD1,
            (a2 + pow2[la] * b2) % MOD2,
            (br1 + pow1[lb] * ar1) % MOD1,
            (br2 + pow2[lb] * ar2) % MOD2,
            la + lb
        )

    def query(left, right):
        # Convert [left, right) into segment-tree coordinates.
        left += size
        right += size

        res_left = (0, 0, 0, 0, 0)
        res_right = (0, 0, 0, 0, 0)

        while left < right:
            if left & 1:
                node = (
                    hf1[left], hf2[left],
                    hr1[left], hr2[left],
                    length[left]
                )
                res_left = merge(res_left, node)
                left += 1

            if right & 1:
                right -= 1
                node = (
                    hf1[right], hf2[right],
                    hr1[right], hr2[right],
                    length[right]
                )
                res_right = merge(node, res_right)

            left >>= 1
            right >>= 1

        return merge(res_left, res_right)

    output = []

    for typ, x, y in queries:
        if typ == 1:
            update(x - 1, y)
        else:
            # Input uses inclusive [x, y].
            # query() uses half-open [x - 1, y).
            h1, h2, rh1, rh2, _ = query(x - 1, y)

            if h1 == rh1 and h2 == rh2:
                output.append("Adnan Wins")
            else:
                output.append("ARCNCD!")

    return "\n".join(output)

def main():
    t = int(input())
    answers = []

    for _ in range(t):
        n, q = map(int, input().split())
        s = input().strip()

        queries = []
        for _ in range(q):
            parts = input().split()
            typ = int(parts[0])

            if typ == 1:
                queries.append((1, int(parts[1]), parts[2]))
            else:
                queries.append((2, int(parts[1]), int(parts[2])))

        answers.append(solve_case(n, q, s, queries))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    main()
```Hai mảng nguồn được xây dựng trước tiên vì mọi nhu cầu hợp nhất (B^k), trong đó (k) là độ dài của một trong các phân đoạn con. Vì tất cả độ dài được truy vấn tối đa là (N), lũy thừa (N+1) là đủ. 

Cây phân đoạn sử dụng bố cục lặp lại với các lá bắt đầu từ`size`. Chuỗi thực tế chiếm các lá (N) đầu tiên, trong khi bất kỳ lá bổ sung nào được tạo ra bởi vì`size`là sức mạnh của hai vẫn trống rỗng. Độ dài của chúng bằng 0, vì vậy chúng không đóng góp gì cho việc hợp nhất. 

các`pull`hàm thực hiện bất biến phân đoạn. Đối với hướng thuận, đoạn bên trái vẫn giữ nguyên số mũ hiện tại và đoạn bên phải được dịch chuyển theo độ dài của đoạn bên trái. Đối với chiều ngược lại, đoạn bên phải xuất hiện trước vì chiều ngược lại của`left + right`là`reverse(right) + reverse(left)`. 

Thủ tục truy vấn sử dụng hai bộ tích lũy.`res_left`nhận các nút đã chọn gặp ở bên trái và nối chúng bình thường.`res_right`nhận các nút đã chọn gặp ở bên phải và thêm vào trước mỗi nút mới. Thứ tự này là cần thiết vì việc duyệt cây phân đoạn không nhất thiết phải gặp tất cả các nút được chọn từ trái sang phải. 

Khoảng thời gian đầu vào là bao gồm, trong khi hàm truy vấn nội bộ sử dụng khoảng thời gian nửa mở. Vì vậy, một truy vấn đầu vào`[l, r]`trở thành`query(l - 1, r)`. Việc chuyển đổi đơn lẻ này chịu trách nhiệm cho một số lỗi dễ xảy ra. 

Số nguyên Python không bị tràn nhưng giá trị băm phải nằm trong phạm vi mô-đun đã chọn. Mọi phép nhân và phép cộng trong công thức băm đều được theo sau bởi`% MOD1`hoặc`% MOD2`. Không có mối lo ngại về tràn số nguyên ngoài số học có độ chính xác tùy ý của Python thông thường. 

Việc triển khai lưu trữ cả hai hướng và cả hai mô-đun trực tiếp trong cây. Việc này sử dụng nhiều bộ nhớ hơn so với việc lưu trữ một hàm băm duy nhất, nhưng nó vẫn giữ nguyên (O(N)) và vừa vặn với giới hạn 256 MB cho (N \le 10^5). 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, chuỗi ban đầu là`adaersd`. Bản cập nhật ở vị trí 5 thay đổi`r`ĐẾN`a`, sản xuất`adaeasd`. Truy vấn ([3,5]) là`aea`, đó là một palindrome. 

| Sự kiện | Hoạt động | Chuỗi hiện tại | Chuỗi con được truy vấn | Tiến = Ngược? | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`1 5 a`|`adaeasd`| | | | 
| 2 |`2 3 5`|`adaeasd`|`aea`| Có |`Adnan Wins`| 
| 3 |`2 1 6`|`adaeasd`|`adaeas`| Không |`ARCNCD!`| 
| 4 |`1 1 d`|`ddaeasd`| | | | 
| 5 |`2 1 2`|`ddaeasd`|`dd`| Có |`Adnan Wins`| 

Dấu vết chứng minh rằng cây đại diện cho chuỗi hiện tại sau mỗi lần cập nhật chứ không chỉ là chuỗi gốc. Truy vấn cuối cùng cũng thực hiện một phạm vi bắt đầu từ vị trí đầu tiên. 

Ví dụ thứ hai cho thấy một palindrome trở thành không palindrome sau một lần cập nhật:```
1
5 3
abcba
2 1 5
1 3 d
2 1 5
```| Sự kiện | Hoạt động | Chuỗi hiện tại | Truy vấn | Chuyển tiếp băm | Băm ngược | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 |`2 1 5`|`abcba`|`[1,5]`| Bằng | Bằng |`Adnan Wins`| 
| 2 |`1 3 d`|`abdba`| | | | | 
| 3 |`2 1 5`|`abdba`|`[1,5]`| Bằng | Bằng |`Adnan Wins`| 

Bản cập nhật cụ thể này xảy ra để bảo toàn bảng màu vì`abdba`cũng đối xứng. Để chứng minh một truy vấn không thành công, hãy thay đổi bản cập nhật thành vị trí 2:```
1
3 2
aba
1 2 c
2 1 3
```| Sự kiện | Hoạt động | Chuỗi hiện tại | Truy vấn | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 |`1 2 c`|`aca`| | | 
| 2 |`2 1 3`|`aca`|`[1,3]`|`Adnan Wins`| 

Bất biến được hiển thị trong cả hai dấu vết: bất cứ khi nào chuỗi con được truy vấn là đối xứng, các biểu diễn tiến và lùi của nó giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N + E\log N)) | Việc xây dựng cây và lũy thừa mất (O(N)), trong khi mọi cập nhật và truy vấn mất (O(\log N)). | 
| Không gian | (O(N)) | Các lũy thừa và tất cả các mảng cây phân đoạn đều chứa các phần tử (O(N)). | 

Với (N,E \le 10^5), thuật toán chỉ thực hiện logarit nhiều thao tác cây cho mỗi sự kiện. Tổng số cấp độ của cây phân đoạn là khoảng 17 cấp cho (10^5) phần tử, do đó, cần phải thực hiện các thao tác nút đại khái (O(10^5 \log 10^5)) thay vì hàng tỷ so sánh ký tự. Việc sử dụng bộ nhớ là tuyến tính và duy trì trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# This test harness contains the same algorithm as the submission,
# exposed through run() so that the assertions can execute it.

import sys
import io

MOD1 = 1_000_000_007
MOD2 = 1_000_000_009
BASE = 911382323

def solve_case(n, q, s, queries):
    size = 1
    while size < n:
        size <<= 1

    pow1 = [1] * (n + 1)
    pow2 = [1] * (n + 1)

    for i in range(1, n + 1):
        pow1[i] = pow1[i - 1] * BASE % MOD1
        pow2[i] = pow2[i - 1] * BASE % MOD2

    total = size << 1
    hf1 = [0] * total
    hf2 = [0] * total
    hr1 = [0] * total
    hr2 = [0] * total
    length = [0] * total

    def value(ch):
        return ord(ch) - 96

    for i, ch in enumerate(s):
        p = size + i
        v = value(ch)
        hf1[p] = hf2[p] = hr1[p] = hr2[p] = v
        length[p] = 1

    def pull(p):
        left = p << 1
        right = left | 1

        ll = length[left]
        lr = length[right]
        length[p] = ll + lr

        hf1[p] = (hf1[left] + pow1[ll] * hf1[right]) % MOD1
        hf2[p] = (hf2[left] + pow2[ll] * hf2[right]) % MOD2
        hr1[p] = (hr1[right] + pow1[lr] * hr1[left]) % MOD1
        hr2[p] = (hr2[right] + pow2[lr] * hr2[left]) % MOD2

    for p in range(size - 1, 0, -1):
        pull(p)

    def update(pos, ch):
        p = size + pos
        v = value(ch)
        hf1[p] = hf2[p] = hr1[p] = hr2[p] = v
        length[p] = 1

        p >>= 1
        while p:
            pull(p)
            p >>= 1

    def merge(a, b):
        if a[4] == 0:
            return b
        if b[4] == 0:
            return a

        a1, a2, ar1, ar2, la = a
        b1, b2, br1, br2, lb = b

        return (
            (a1 + pow1[la] * b1) % MOD1,
            (a2 + pow2[la] * b2) % MOD2,
            (br1 + pow1[lb] * ar1) % MOD1,
            (br2 + pow2[lb] * ar2) % MOD2,
            la + lb
        )

    def query(left, right):
        left += size
        right += size

        a = (0, 0, 0, 0, 0)
        b = (0, 0, 0, 0, 0)

        while left < right:
            if left & 1:
                a = merge(a, (
                    hf1[left], hf2[left],
                    hr1[left], hr2[left],
                    length[left]
                ))
                left += 1

            if right & 1:
                right -= 1
                b = merge((
                    hf1[right], hf2[right],
                    hr1[right], hr2[right],
                    length[right]
                ), b)

            left >>= 1
            right >>= 1

        return merge(a, b)

    ans = []

    for typ, x, y in queries:
        if typ == 1:
            update(x - 1, y)
        else:
            h1, h2, rh1, rh2, _ = query(x - 1, y)
            if h1 == rh1 and h2 == rh2:
                ans.append("Adnan Wins")
            else:
                ans.append("ARCNCD!")

    return "\n".join(ans)

def run(inp: str) -> str:
    data = io.StringIO(inp)
    t = int(data.readline())
    all_answers = []

    for _ in range(t):
        n, q = map(int, data.readline().split())
        s = data.readline().strip()

        queries = []
        for _ in range(q):
            parts = data.readline().split()
            if parts[0] == "1":
                queries.append((1, int(parts[1]), parts[2]))
            else:
                queries.append((2, int(parts[1]), int(parts[2])))

        all_answers.append(solve_case(n, q, s, queries))

    return "\n".join(all_answers)

# Provided sample.
sample1 = """\
1
7 5
adaersd
1 5 a
2 3 5
2 1 6
1 1 d
2 1 2
"""

assert run(sample1) == """\
Adnan Wins
ARCNCD!
Adnan Wins
""".strip(), "sample 1"

# Minimum size and length-one palindrome.
case2 = """\
1
1 3
a
2 1 1
1 1 z
2 1 1
"""

assert run(case2) == """\
Adnan Wins
Adnan Wins
""".strip(), "minimum size"

# All equal characters remain palindromes after updates.
case3 = """\
1
5 4
aaaaa
2 1 5
1 3 a
2 2 4
2 1 4
"""

assert run(case3) == """\
Adnan Wins
Adnan Wins
Adnan Wins
""".strip(), "all equal"

# Boundary queries and a change that destroys the palindrome.
case4 = """\
1
5 4
abcba
2 1 5
2 2 4
1 1 z
2 1 5
"""

assert run(case4) == """\
Adnan Wins
Adnan Wins
ARCNCD!
""".strip(), "boundary and update"

# Maximum-size construction. The first query is a palindrome,
# then one endpoint changes and the full-range query must fail.
n = 100000
case5 = (
    "1\n"
    f"{n} 3\n"
    + "a" * n
    + "\n2 1 100000\n"
    + "1 1 b\n"
    + "2 1 100000\n"
)

assert run(case5) == """\
Adnan Wins
ARCNCD!
""".strip(), "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`Adnan Wins`,`ARCNCD!`,`Adnan Wins`| Cập nhật chính thức và chuỗi truy vấn phạm vi | 
|`N=1`, phạm vi một ký tự |`Adnan Wins`hai lần | Kích thước tối thiểu và khoảng thời gian đơn lẻ | 
|`aaaaa`| Ba`Adnan Wins`câu trả lời | Giá trị hoàn toàn bằng nhau và cập nhật không thay đổi | 
|`abcba`với bản cập nhật điểm cuối |`Adnan Wins`,`Adnan Wins`,`ARCNCD!`| Ranh giới toàn phạm vi và tuyên truyền cập nhật | 
|`100000`ký tự bằng nhau |`Adnan Wins`,`ARCNCD!`| Kích thước và hiệu suất tối đa | 

## Vỏ cạnh 

Khoảng đơn không cần trường hợp đặc biệt nào trong cây. Đối với đầu vào```
1
1 1
a
2 1 1
```khoảng nội bộ là`[0,1)`, do đó truy vấn trả về một đoạn có độ dài bằng một. Giá trị băm thuận và ngược của nó đều là giá trị của`a`, và chương trình in`Adnan Wins`. 

Bản cập nhật gán lại ký tự hiện tại được xử lý bằng cách thay thế lá bằng cùng một giá trị và xây dựng lại tổ tiên của nó. Vì```
1
3 2
aba
1 2 b
2 1 3
```chuỗi vẫn còn`aba`, do đó, giá trị băm toàn dải vẫn bằng nhau và đầu ra là`Adnan Wins`. Việc triển khai không cho rằng mọi cập nhật đều thay đổi giá trị. 

Một truy vấn chạm vào ranh giới bên phải sẽ thực hiện chuyển đổi từ lập chỉ mục bao hàm sang lập chỉ mục nửa mở. Vì```
1
5 2
abcba
2 1 5
2 2 4
```truy vấn đầu tiên trở thành`[0,5)`và thứ hai trở thành`[1,4)`. Chuỗi con của chúng là`abcba`Và`bcb`, tương ứng và cả hai đều có giá trị băm thuận và ngược phù hợp. Cả hai đầu ra đều`Adnan Wins`. 

Một non-palindrome với tần số ký tự đối xứng sẽ bắt các cách tiếp cận chỉ dựa trên số lượng. Ví dụ,```
1
4 1
aabb
2 1 4
```sản xuất`ARCNCD!`. Trình tự chuyển tiếp là`aabb`, trong khi ngược lại là`bbaa`. Cây phân đoạn lưu trữ hai giá trị băm khác nhau này, do đó nó loại bỏ phạm vi mặc dù tần số của`a`Và`b`giống hệt nhau. 

Trường hợp cập nhật toàn chuỗi cũng kiểm tra xem các thay đổi có lan truyền đến tận gốc hay không. Bắt đầu với`abcba`, thay đổi vị trí 1 thành`z`sản xuất`zbcba`. Một truy vấn kết thúc`[1,5]`sau đó so sánh`zbcba`chống lại`abcbz`, khác nhau nên câu trả lời là`ARCNCD!`. Điều này xác nhận rằng đường dẫn cập nhật sẽ xây dựng lại chính xác mọi tổ tiên có chuỗi con được lưu trữ chứa vị trí đã thay đổi.

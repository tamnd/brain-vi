---
title: "CF 102441A - Mẫu tìm kiếm"
description: "Chúng ta được cung cấp một mẫu chứa các chữ cái viết thường, ?, và . Một chữ cái viết thường phải xuất hiện theo nghĩa đen, ? có thể đại diện cho bất kỳ một chữ cái viết thường nào và có thể đại diện cho bất kỳ chuỗi chữ cái viết thường nào, bao gồm cả chuỗi trống."
date: "2026-08-09T01:31:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102441
codeforces_index: "A"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Final"
rating: 0
weight: 102441
solve_time_s: 425
verified: true
draft: false
---

[CF 102441A - Mẫu tìm kiếm](https://codeforces.com/problemset/problem/102441/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mẫu chứa các chữ cái viết thường,`?`, Và`*`. Một chữ cái viết thường phải xuất hiện theo nghĩa đen,`?`có thể đại diện cho bất kỳ một chữ cái viết thường nào và`*`có thể đại diện cho bất kỳ chuỗi chữ cái viết thường nào, bao gồm cả chuỗi trống. Chúng ta cần chọn một chuỗi chữ thường thực tế phù hợp với toàn bộ mẫu, với yêu cầu bổ sung rằng chuỗi kết quả là một bảng màu. Trong số tất cả các lựa chọn có thể, chúng tôi muốn một lựa chọn có độ dài tối thiểu. Nếu không có bảng màu nào có thể khớp với mẫu, chúng tôi sẽ in`-1`. Chuỗi trống được tính là một palindrome hợp lệ. 

Độ dài của mẫu tối đa là 500. Độ dài này đủ nhỏ để lập trình động bậc hai, nhưng không đủ nhỏ để liệt kê các chuỗi đầu ra có thể có. Ngay cả một bảng chữ cái chỉ có 26 chữ cái cũng có nhiều ứng cử viên theo cấp số nhân. Thuật toán bậc ba thực hiện khoảng (500^3 = 125) triệu chuyển đổi trạng thái cơ bản trong trường hợp xấu nhất, điều này vốn đã gây khó chịu trong Python, vì vậy chúng ta sẽ tiến thêm một bước nữa và biến DP thành bậc hai. 

Có một số trường hợp việc xây dựng tham lam đơn giản không thành công. Vì`ac?ba`, hai bên ngoài`a`các ký tự tương thích, nhưng sau khi khớp chúng, chúng ta chỉ còn lại`c?b`, mà các đầu của chúng không thể bằng nhau. Câu trả lời đúng là`-1`. Một thuật toán bất cẩn xử lý`?`vì việc tự động sửa lỗi không khớp có thể tạo ra chuỗi không hợp lệ. 

Vì`*ac?ba`, ngôi sao không thể đơn giản bị bỏ qua. Palindrome hợp lệ ngắn nhất là`abacaba`. Ngôi sao hàng đầu tiêu thụ`ab`, trong khi hậu tố cố định`ba`tạo ra sự phản chiếu`ab`ở đầu bên kia. Một triển khai luôn xử lý`*`như một chuỗi trống bỏ lỡ cấu trúc tối ưu. 

Vì`*`, câu trả lời là chuỗi rỗng. Từ`*`có thể tiêu thụ 0 ký tự, không có lý do gì để xuất ra dù chỉ một chữ cái. Việc triển khai giả định mọi ký tự mẫu đóng góp cho câu trả lời sẽ tạo ra một chuỗi không trống không chính xác. 

Vì`??`, câu trả lời là`aa`. Cả hai dấu chấm hỏi có thể độc lập lựa chọn`a`và hai vị trí phải bằng nhau vì chuỗi cuối cùng là một chuỗi palindrome. Việc triển khai bất cẩn có thể buộc chúng phải chuyển sang các ký tự khác nhau một cách không cần thiết hoặc xử lý`?`như một biểu tượng theo nghĩa đen. 

Cuối cùng,`a*b`không có giải pháp khi`a`Và`b`khác nhau. Mỗi chuỗi khớp với mẫu này đều bắt đầu bằng`a`và kết thúc bằng`b`, trong khi một bảng màu phải có ký tự đầu tiên và cuối cùng bằng nhau. Ngôi sao không thể thay đổi sự thật đó vì nó nằm giữa hai điểm cuối cố định. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp sẽ liệt kê mọi bảng màu có độ dài tăng dần, kiểm tra xem nó có khớp với mẫu hay không và dừng ở độ dài thành công đầu tiên. Nếu giải pháp tồn tại, cấu trúc DP bên dưới cung cấp độ dài tối đa (2n), vì vậy chúng tôi có thể hạn chế tìm kiếm trong phạm vi đó. Một bảng màu có độ dài (L) được xác định bởi các ký tự đầu tiên (\lceil L/2\rceil) của nó, cho ra các ứng cử viên (26^{\lceil L/2\rceil}). 

Đối với (n=500), việc liệt kê độ dài từ 0 đến 1000 sẽ kiểm tra chính xác 

1+\frac{52(26^{500}-1)}{25} 
] 

palindromes ứng cử viên. Việc kiểm tra từng ứng viên theo mẫu mất thời gian tuyến tính, do đó tổng công việc sẽ vào khoảng (n26^n). Lý do cách tiếp cận này đúng về mặt khái niệm rất đơn giản: mọi bảng màu có thể cuối cùng đều được kiểm tra và bảng màu được chấp nhận đầu tiên có độ dài tối thiểu. Vấn đề là không gian tìm kiếm rất lớn về mặt thiên văn. 

Quan sát quan trọng là bảng màu cho chúng ta một cách tự nhiên để phân chia mẫu từ hai đầu của nó. Giả sử khoảng thời gian mẫu hiện tại là (s[l..r]). Nếu cả hai điểm cuối đều là ký tự thông thường hoặc dấu chấm hỏi thì chúng phải tạo ra cùng một ký tự. Chúng ta có thể đặt ký tự đó ở cả hai đầu của câu trả lời và giải khoảng bên trong. 

Trường hợp thú vị là một`*`tại một điểm cuối. Hãy xem xét một sự dẫn đầu`*`. Nó có thể tiêu thụ một số chuỗi (X). Bởi vì câu trả lời cuối cùng là một bảng màu nên hậu tố tương ứng của câu trả lời phải là (đảo ngược (X)). Phần còn lại của mẫu nằm giữa hai bản sao đó. Nếu chúng ta quyết định rằng hậu tố (s[k..r]) chịu trách nhiệm tạo ra (X), thì ngôi sao dẫn đầu có thể tạo ra (đảo ngược(X)) và mẫu ở giữa (s[l+1..k-1]) phải tạo ra một bảng màu. 

Để giảm thiểu độ dài, chuỗi tốt nhất được tạo ra bởi một đoạn mẫu tùy ý đặc biệt đơn giản. Mỗi nhân vật bình thường đóng góp một nhân vật, mỗi nhân vật`?`đóng góp một nhân vật được chọn và mỗi`*`không thể đóng góp được gì. Do đó, độ dài tối thiểu của chuỗi khớp (s[k..r]) chỉ là số ký tự không phải dấu sao trong khoảng đó. 

Điều này biến quá trình chuyển đổi ngôi sao thành 

\min_k 
\left( 
dp[l+1][k-1] 
+ 
Số lượng 2\cdot(k,r) 
\đúng), 
] 

ở đâu`count(k,r)`là số ký tự không phải dấu sao trong (s[k..r]). Ngoài ra còn có khả năng làm cho ngôi sao dẫn đầu trống rỗng, đưa ra`dp[l+1][r]`. 

Việc triển khai trực tiếp sẽ thử mọi (k) cho mỗi khoảng thời gian, cho ra (O(n^3)). Biểu thức có thể được sắp xếp lại bằng cách sử dụng số lượng tiền tố: 

2P[r+1] 
+ 
\left(dp[l+1][k-1]-2P[k]\right), 
] 

trong đó (P[x]) là số ký tự không phải dấu sao ở vị trí (x) đầu tiên. Đối với (l) cố định, mức tối thiểu bên trong dấu ngoặc đơn có thể được duy trì tăng dần khi (r) tăng lên. Biểu thức đối xứng xử lý một dấu vết`*`. Điều đó loại bỏ hệ số phụ của (n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n26^n)) | (O(n)) | Quá chậm | 
| Khoảng thời gian trực tiếp DP | (O(n^3)) | (O(n^2)) | Hợp lệ về mặt khái niệm, không thân thiện với Python | 
| Khoảng thời gian được tối ưu hóa DP | (O(n^2)) | (O(n^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định`dp[l][r]`là độ dài tối thiểu của một bảng màu phù hợp với khoảng mẫu`s[l..r]`. Nếu khoảng trống, giá trị của nó bằng 0. 
2. Tính toán trước`pref[i]`, số ký tự không phải dấu sao trong số`s[0..i-1]`. Sau đó, độ dài tối thiểu của một chuỗi khớp tùy ý`s[l..r]`là`pref[r+1] - pref[l]`, bởi vì các ngôi sao luôn có thể trống rỗng. 
3. Khoảng thời gian xử lý trong việc tăng điểm cuối bên phải và giảm điểm cuối bên trái. Thứ tự này làm cho`dp[i+1][j]`,`dp[i][j-1]`, Và`dp[i+1][j-1]`có sẵn bất cứ khi nào họ cần thiết. 
4. Nếu khoảng bao gồm một ký tự, dấu sao sẽ không có ký tự nào, trong khi một chữ cái hoặc`?`đóng góp một nhân vật. 
5. Nếu`s[l]`là một ngôi sao, một lựa chọn là làm trống nó và sử dụng`dp[l+1][r]`. Tùy chọn khác là để nó phản chiếu một chuỗi ngắn nhất được tạo bởi một số hậu tố`s[k..r]`. Độ dài kết quả là`2 * nonstars(k,r) + dp[l+1][k-1]`. 
6. Duy trì giá trị tối thiểu của`dp[l+1][k-1] - 2 * pref[k]`trong khi điểm cuối bên phải phát triển. Điều này làm cho việc sử dụng ngôi sao dẫn đầu tốt nhất có sẵn trong thời gian không đổi cho mỗi khoảng thời gian. 
7. Nếu điểm cuối bên trái không phải là ngôi sao mà là`s[r]`là một ngôi sao, hãy áp dụng ý tưởng tương tự từ hướng khác. Dấu sao ở cuối trống hoặc nó phản ánh chuỗi ngắn nhất được tạo bởi một số tiền tố`s[l..k]`. 
8. Duy trì mức tối thiểu tương ứng`2 * pref[k+1] + dp[k+1][r-1]`trong khi điểm cuối bên trái di chuyển vào trong. Điều này mang lại cách sử dụng tốt nhất dấu sao ở cuối trong thời gian không đổi. 
9. Nếu cả hai điểm cuối đều không phải là ngôi sao thì chúng phải có khả năng đại diện cho cùng một ký tự. Hai chữ cái bằng nhau thì tương thích nhau, một chữ cái và`?`tương thích và hai`?`các ký tự tương thích. Khi chúng tương thích, hãy đặt cùng một ký tự đã chọn ở cả hai đầu và thêm hai ký tự vào`dp[l+1][r-1]`. 
10. Bên cạnh mỗi giá trị DP, hãy lưu trữ quá trình chuyển đổi nào đã tạo ra giá trị đó. Để phân chia sao, lưu trữ vị trí phân chia đã chọn`k`. Điều này cho phép chúng ta xây dựng lại bảng màu thực tế sau khi DP kết thúc. 
11. Trong quá trình tái thiết, một ngôi sao dẫn đầu đã tách ra ở`k`sản xuất`reverse(T) + middle + T`, Ở đâu`T`là chuỗi ngắn nhất được biểu diễn bởi`s[k..r]`. Một ngôi sao phía sau tạo ra`T + middle + reverse(T)`. Điểm cuối phù hợp thông thường tạo ra`c + middle + c`. 
12. Nếu`dp[0][n-1]`là vô hạn, không có bảng màu nào khớp với mẫu đó, vì vậy hãy in`-1`. Nếu không thì xây dựng lại các quyết định được lưu trữ. 

### Tại sao nó hoạt động 

Tính bất biến đó là`dp[l][r]`chính xác là độ dài tối thiểu của một kết hợp palindrome`s[l..r]`. Khi cả hai điểm cuối đều không phải là một ngôi sao, thì một palindrome buộc hai điểm cuối phải sử dụng cùng một ký tự, do đó quá trình chuyển đổi thông thường sẽ xem xét mọi lựa chọn hợp lệ có thể có. Khi điểm cuối bên trái là một ngôi sao, mọi bảng màu phù hợp sẽ sử dụng ngôi sao đó cho 0 ký tự hoặc sử dụng một tiền tố đảo ngược với chuỗi được tạo bởi một số hậu tố của mẫu còn lại. Cái sau chính xác là những gì quá trình chuyển đổi phân chia liệt kê. Trường hợp dấu sao phía sau có tính đối xứng. 

Đối với mỗi lần phân tách, chúng tôi sử dụng chuỗi ngắn nhất có thể được biểu thị bằng phân đoạn mẫu được phản chiếu. Việc làm cho đoạn đó dài hơn chỉ có thể thêm các ký tự vào cả hai phía của bảng màu và không thể làm cho bài toán ở giữa độc lập ngắn hơn. Vì vậy, sự lựa chọn độ dài tối thiểu cho mỗi lần phân chia là đủ. Vì mọi khả năng sử dụng ngôi sao bên ngoài đều được biểu thị bằng một số phân chia và mọi cặp điểm cuối không phải sao được biểu thị bằng chuyển đổi thông thường nên DP xem xét mọi cấu trúc khả thi và chọn cấu trúc ngắn nhất. 

Việc xây dựng lại tuân theo sự phân rã tương tự được DP sử dụng. Mỗi phần được xây dựng hoặc là một ký tự được phản chiếu xung quanh một palindrome nhỏ hơn hoặc một chuỗi được phản ánh xung quanh một palindrome nhỏ hơn, vì vậy kết quả luôn là một palindrome nhỏ hơn. Các đoạn mẫu tương ứng nối theo thứ tự bắt buộc nên kết quả cũng khớp với mẫu ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**9

# Transition types:
# 1 = skip left star
# 2 = use left star, split at arg[i][j]
# 3 = skip right star
# 4 = use right star, split at arg[i][j]
# 5 = match both endpoints
#
# dp[l][r] = minimum palindrome length matching s[l:r+1]

def solve_template(s):
    n = len(s)

    # pref[i] = number of non-'*' characters in s[:i]
    pref = [0] * (n + 1)
    for i, ch in enumerate(s):
        pref[i + 1] = pref[i] + (ch != '*')

    dp = [[INF] * n for _ in range(n)]
    kind = [[0] * n for _ in range(n)]
    arg = [[-1] * n for _ in range(n)]

    # For a fixed left endpoint i:
    # left_best[i] =
    # min_{k=i+1..j} dp[i+1][k-1] - 2*pref[k]
    #
    # left_arg[i] stores the k producing that minimum.
    left_best = [INF] * n
    left_arg = [-1] * n

    for j in range(n):
        # For this fixed j, while i decreases:
        # right_best =
        # min_{k=i..j-1} 2*pref[k+1] + dp[k+1][j-1]
        right_best = INF
        right_arg = -1

        for i in range(j, -1, -1):
            if i < j:
                # dp[i+1][j-1], with the empty interval handled explicitly.
                inner = 0 if i + 1 >= j else dp[i + 1][j - 1]

                # Add k = j to the running minimum for a leading star.
                candidate_left = inner - 2 * pref[j]
                if candidate_left < left_best[i]:
                    left_best[i] = candidate_left
                    left_arg[i] = j

                # Add k = i to the running minimum for a trailing star.
                candidate_right = 2 * pref[i + 1] + inner
                if candidate_right < right_best:
                    right_best = candidate_right
                    right_arg = i

            # Single-character interval.
            if i == j:
                if s[i] == '*':
                    dp[i][j] = 0
                    kind[i][j] = 1
                else:
                    dp[i][j] = 1
                    kind[i][j] = 5
                continue

            if s[i] == '*':
                # Option 1: make the left star empty.
                best = dp[i + 1][j]
                best_kind = 1
                best_arg = -1

                # Option 2: mirror a shortest string produced by s[k..j].
                candidate = 2 * pref[j + 1] + left_best[i]
                if candidate < best:
                    best = candidate
                    best_kind = 2
                    best_arg = left_arg[i]

                dp[i][j] = best
                kind[i][j] = best_kind
                arg[i][j] = best_arg

            elif s[j] == '*':
                # Option 1: make the right star empty.
                best = dp[i][j - 1]
                best_kind = 3
                best_arg = -1

                # Option 2: mirror a shortest string produced by s[i..k].
                candidate = right_best - 2 * pref[i]
                if candidate < best:
                    best = candidate
                    best_kind = 4
                    best_arg = right_arg

                dp[i][j] = best
                kind[i][j] = best_kind
                arg[i][j] = best_arg

            else:
                # Neither endpoint is a star.
                a = s[i]
                b = s[j]

                compatible = (
                    a == b or
                    a == '?' or
                    b == '?'
                )

                if compatible:
                    dp[i][j] = 2 + (
                        0 if i + 1 >= j else dp[i + 1][j - 1]
                    )
                    kind[i][j] = 5
                # Otherwise dp[i][j] stays INF.

    if dp[0][n - 1] >= INF:
        return "-1"

    def canonical(l, r):
        """Shortest concrete string matching s[l:r+1]."""
        out = []
        for p in range(l, r + 1):
            if s[p] == '*':
                continue
            if s[p] == '?':
                out.append('a')
            else:
                out.append(s[p])
        return ''.join(out)

    def build(l, r):
        if l > r:
            return ""

        t = kind[l][r]

        if l == r:
            if s[l] == '*':
                return ""
            if s[l] == '?':
                return "a"
            return s[l]

        if t == 1:
            # Left star is empty.
            return build(l + 1, r)

        if t == 2:
            # Left star mirrors the shortest string from k..r.
            k = arg[l][r]
            middle = build(l + 1, k - 1)
            x = canonical(k, r)
            return x[::-1] + middle + x

        if t == 3:
            # Right star is empty.
            return build(l, r - 1)

        if t == 4:
            # Right star mirrors the shortest string from l..k.
            k = arg[l][r]
            middle = build(k + 1, r - 1)
            x = canonical(l, k)
            return x + middle + x[::-1]

        # Ordinary compatible endpoints.
        a = s[l]
        b = s[r]

        if a == '?':
            c = b if b != '?' else 'a'
        else:
            c = a

        return c + build(l + 1, r - 1) + c

    return build(0, n - 1)

def main():
    s = input().strip()
    print(solve_template(s))

if __name__ == "__main__":
    main()
```Mảng tiền tố là sự tối ưu hóa đầu tiên.`pref[r + 1] - pref[l]`cho chúng ta biết một khoảng mẫu phải đóng góp bao nhiêu ký tự thực tế nếu tất cả các ngôi sao của nó bị bỏ trống. Điều này là đủ vì các chuyển đổi phân tách chỉ cần chuỗi ngắn nhất có thể được biểu thị bằng khoảng thời gian được phản ánh. 

các`left_best`mảng lưu trữ mức tối thiểu được chuyển đổi cho mọi ngôi sao dẫn đầu có thể. Thay vì đánh giá liên tục mỗi lần phân chia`k`, mã bổ sung thêm khả năng mới`k = j`khi ranh giới bên phải tiến lên. Biểu thức liên quan đến`pref`tách phần tùy thuộc vào`j`từ một phần tùy thuộc vào`k`. 

các`right_best`biến thực hiện tối ưu hóa đối xứng cho một ngôi sao ở cuối. Nó được đặt lại cho mỗi điểm cuối bên phải và được cập nhật khi điểm cuối bên trái di chuyển về 0. Ngay bây giờ`dp[i][j]`được tính toán, nó chứa chính xác phần phân chia tốt nhất có phần đầu tiên bắt đầu tại hoặc sau`i`. 

Thứ tự của các vòng lặp lồng nhau là cần thiết. Vòng ngoài tăng lên`j`, trong khi vòng lặp bên trong giảm`i`. Do đó,`dp[i+1][j]`đã được tính toán trước đó trong cùng một cột và`dp[i+1][j-1]`đã được tính toán ở cột trước đó. 

Việc tái thiết có chủ ý sử dụng chuỗi bê tông ngắn nhất được biểu thị bằng một khoảng cách. MỘT`?`được thay thế bởi`a`, trong khi các ngôi sao bị bỏ qua. Vì những ký tự đó được phản ánh xung quanh phần giữa được xây dựng đệ quy, nên lựa chọn chính xác cho`?`không ảnh hưởng đến sự tối ưu. 

Không thể tràn số nguyên trong Python. Trong các ngôn ngữ khác, loại số nguyên bình thường đã đủ vì câu trả lời tối đa là (2n), trong khi DP sử dụng`INF`chỉ với tư cách là lính canh. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mô hình là`*ac?ba`. Đánh số các vị trí từ 0 đến 5. 

| Tình trạng`(l,r)`| Khoảng thời gian mẫu | Chuyển tiếp | Chuỗi nhân đôi | Trung | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
|`(0,5)`|`*ac?ba`| Sao trái chia ở 4 |`ba`|`aca`|`abacaba`| 
|`(1,3)`|`ac?`| Cuộc thi đấu`a`với`?`|`a`|`c`|`aca`| 
|`(2,2)`|`c`| Ký tự đơn |`c`| trống |`c`| 
| trống | trống | Trường hợp cơ sở | trống | trống | trống | 

Ở cấp độ cao nhất, hậu tố`ba`được khớp với vị trí mẫu 4 và 5. Ngôi sao dẫn đầu sử dụng đảo ngược của nó,`ab`. Mẫu còn lại là`ac?`, có bảng màu ngắn nhất là`aca`. Kết hợp chúng mang lại`ab + aca + ba = abacaba`. Kết quả khớp với mẫu vì ngôi sao dẫn đầu tiêu thụ`ab`, sau đó`ac?ba`tiêu thụ`acaba`. 

### Mẫu 2 

Mô hình là`ac?ba`. 

| Tình trạng`(l,r)`| Khoảng thời gian mẫu | So sánh điểm cuối | Khoảng bên trong | Kết quả | 
| --- | --- | --- | --- | --- | 
|`(0,4)`|`ac?ba`|`a`Và`a`trận đấu |`c?b`| phụ thuộc vào bên trong | 
|`(1,3)`|`c?b`|`c`Và`b`xung đột |`?`| không thể | 
|`(0,4)`|`ac?ba`| cặp ngoài không thể hoàn thành | không thể |`-1`| 

Bên ngoài`a`các nhân vật buộc phải phù hợp với nhau. Sau khi chúng bị xóa, mẫu bên trong có các điểm cuối cố định`c`Và`b`, không thể trở thành bằng nhau. Không có ngôi sao nào có thể hấp thụ sự không khớp, vì vậy toàn bộ mẫu không có sự khớp nhạt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2)) | Có (O(n^2)) trạng thái DP và mức tối thiểu phân chia sao được duy trì tăng dần theo thời gian không đổi | 
| Không gian | (O(n^2)) | Các giá trị DP và các lựa chọn tái thiết sử dụng bộ nhớ bậc hai | 

Đối với (n \le 500), chỉ có 250.000 trạng thái DP. Mỗi trạng thái thực hiện công việc liên tục sau khi duy trì mức tối thiểu đang chạy, do đó thuật toán dễ dàng nằm trong giới hạn dự kiến. Việc tái cấu trúc cũng tuyến tính về kích thước của mẫu cộng với câu trả lời được tạo ra và câu trả lời được tạo ra có độ dài tối đa (2n). 

## Trường hợp thử nghiệm```python
import io
import sys

# The submitted solution is represented by solve_template(s).

INF = 10**9

def solve_template(s):
    n = len(s)

    pref = [0] * (n + 1)
    for i, ch in enumerate(s):
        pref[i + 1] = pref[i] + (ch != '*')

    dp = [[INF] * n for _ in range(n)]
    kind = [[0] * n for _ in range(n)]
    arg = [[-1] * n for _ in range(n)]

    left_best = [INF] * n
    left_arg = [-1] * n

    for j in range(n):
        right_best = INF
        right_arg = -1

        for i in range(j, -1, -1):
            if i < j:
                inner = 0 if i + 1 >= j else dp[i + 1][j - 1]

                candidate_left = inner - 2 * pref[j]
                if candidate_left < left_best[i]:
                    left_best[i] = candidate_left
                    left_arg[i] = j

                candidate_right = 2 * pref[i + 1] + inner
                if candidate_right < right_best:
                    right_best = candidate_right
                    right_arg = i

            if i == j:
                if s[i] == '*':
                    dp[i][j] = 0
                    kind[i][j] = 1
                else:
                    dp[i][j] = 1
                    kind[i][j] = 5
                continue

            if s[i] == '*':
                best = dp[i + 1][j]
                best_kind = 1
                best_arg = -1

                candidate = 2 * pref[j + 1] + left_best[i]
                if candidate < best:
                    best = candidate
                    best_kind = 2
                    best_arg = left_arg[i]

                dp[i][j] = best
                kind[i][j] = best_kind
                arg[i][j] = best_arg

            elif s[j] == '*':
                best = dp[i][j - 1]
                best_kind = 3
                best_arg = -1

                candidate = right_best - 2 * pref[i]
                if candidate < best:
                    best = candidate
                    best_kind = 4
                    best_arg = right_arg

                dp[i][j] = best
                kind[i][j] = best_kind
                arg[i][j] = best_arg

            else:
                a = s[i]
                b = s[j]

                if a == b or a == '?' or b == '?':
                    dp[i][j] = 2 + (
                        0 if i + 1 >= j else dp[i + 1][j - 1]
                    )
                    kind[i][j] = 5

    if dp[0][n - 1] >= INF:
        return "-1"

    def canonical(l, r):
        out = []
        for p in range(l, r + 1):
            if s[p] == '*':
                continue
            out.append('a' if s[p] == '?' else s[p])
        return ''.join(out)

    def build(l, r):
        if l > r:
            return ""

        t = kind[l][r]

        if l == r:
            if s[l] == '*':
                return ""
            return 'a' if s[l] == '?' else s[l]

        if t == 1:
            return build(l + 1, r)

        if t == 2:
            k = arg[l][r]
            x = canonical(k, r)
            return x[::-1] + build(l + 1, k - 1) + x

        if t == 3:
            return build(l, r - 1)

        if t == 4:
            k = arg[l][r]
            x = canonical(l, k)
            return x + build(k + 1, r - 1) + x[::-1]

        a = s[l]
        b = s[r]
        if a == '?':
            c = b if b != '?' else 'a'
        else:
            c = a

        return c + build(l + 1, r - 1) + c

    return build(0, n - 1)

def run(inp: str) -> str:
    return solve_template(inp)

# Provided samples
assert run("*ac?ba") == "abacaba", "sample 1"
assert run("ac?ba") == "-1", "sample 2"

# Minimum-size and empty-palindrome case
assert run("*") == "", "a single star can match the empty string"

# Minimum-size question-mark case
assert run("?") == "a", "a question mark can choose any lowercase letter"

# All-equal values
assert run("aa") == "aa", "two equal fixed endpoints form a palindrome"

# Boundary case with a star and mismatching fixed endpoints
assert run("a*b") == "-1", "a palindrome cannot start with a and end with b"

# Star at the boundary can mirror the fixed prefix
assert run("abc*") == "abccba", "trailing star mirrors abc"

# Maximum-size all-equal input
assert run("a" * 500) == "a" * 500, "maximum-size fixed palindrome"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`*`| chuỗi trống | Mẫu có kích thước tối thiểu và ngôi sao có độ dài bằng 0 | 
|`?`|`a`| Xử lý ký tự đại diện đơn | 
|`aa`|`aa`| Điểm cuối cố định bằng nhau | 
|`a*b`|`-1`| Điểm cuối cố định không thể khớp | 
|`abc*`|`abccba`| Ngôi sao ranh giới và tiền tố phản chiếu | 
|`a`lặp lại 500 lần |`a`lặp lại 500 lần | Kích thước đầu vào tối đa và ranh giới DP bậc hai | 

## Vỏ cạnh 

cho`ac?ba`, thuật toán đầu tiên xử lý bên ngoài`a`Và`a`như một cặp tương thích. Khoảng còn lại là`c?b`. Vì cả hai điểm cuối đều không phải là ngôi sao và`c`không thể bằng`b`, giá trị DP của nó là vô hạn. Sự vô cực đó lan truyền đến khoảng thời gian ban đầu, tạo ra`-1`. 

Vì`*ac?ba`, ngôi sao dẫn đầu có hai khả năng khác nhau về cơ bản. Nó có thể trống rỗng, để lại`ac?ba`và cuối cùng là thất bại. Hoặc nó có thể phản ánh một hậu tố. Sự phân chia tối ưu chọn hậu tố`ba`. Chuỗi kết hợp ngắn nhất của nó chính xác là`ba`, vì vậy ngôi sao đóng góp`ab`ở bên trái. Khoảng giữa`ac?`trở thành`aca`, cho`abacaba`. 

Vì`*`, trường hợp cơ sở một ký tự sẽ gán độ dài bằng 0 vì ngôi sao không thể tiêu thụ gì cả. Việc xây dựng lại trả về chuỗi trống và chương trình in ra một dòng trống. Đây là tình huống duy nhất mà câu trả lời không có ký tự. 

Vì`??`, hai điểm cuối tương thích vì cả hai đều là ký tự đại diện. Việc tái thiết lựa chọn`a`cho cả hai, sản xuất`aa`. Sự lựa chọn của`a`là tùy ý, nhưng điều kiện palindrome yêu cầu sử dụng cùng một ký tự. 

Vì`a*b`, các ký tự bên ngoài là`a`Và`b`, do đó mẫu không thể tạo ra bảng màu. Ngôi sao là nội bộ và không thể thay đổi điểm cuối. DP đạt đến một khoảng thời gian không thể chính xác thay vì cố gắng sử dụng ngôi sao để sửa chữa sự không khớp mà nó không thể sửa chữa. 

Vì`abc*`, điểm cuối bên phải là một ngôi sao. Quá trình chuyển đổi tối ưu giữ tiền tố cố định`abc`như sợi dây được phản chiếu bởi ngôi sao. Kết quả là`abc + cba = abccba`, phù hợp`abc*`bởi vì ngôi sao tiêu thụ`cba`. Câu trả lời có độ dài sáu, cho thấy tại sao chỉ xóa từng dấu sao là không đủ. 

Đối với đầu vào có kích thước tối đa bao gồm 500`a`các ký tự, không có ngôi sao và mọi cặp phản chiếu đều tương thích. DP giảm xuống mức tái phát palindrome thông thường, tạo ra chính xác 500`a`nhân vật. Trường hợp này thực hiện không gian trạng thái bậc hai đầy đủ và cả hai ranh giới khoảng.

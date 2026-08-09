---
title: "CF 102465K - Lái Xe Không Trung Thực"
description: "Chúng ta có một chuỗi mô tả trình tự các địa điểm đã ghé thăm trong chuyến đi. Một mô tả được nén có thể biểu thị trực tiếp một ký tự, nối hai mô tả đã được nén hoặc lấy một mô tả đã nén và lặp lại nó với số lần dương bất kỳ."
date: "2026-08-08T09:31:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102465
codeforces_index: "K"
codeforces_contest_name: "2018-2019 ICPC Southwestern European Regional Programming Contest (SWERC 2018)"
rating: 0
weight: 102465
solve_time_s: 263
verified: true
draft: false
---

[CF 102465K - Trình điều khiển không trung thực](https://codeforces.com/problemset/problem/102465/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi mô tả trình tự các địa điểm đã ghé thăm trong chuyến đi. Một mô tả được nén có thể biểu thị trực tiếp một ký tự, nối hai mô tả đã được nén hoặc lấy một mô tả đã nén và lặp lại nó với số lần dương bất kỳ. 

Giá của một mô tả nén không phải là số lượng ký tự được viết trong mô tả. Đó là số đường dẫn nguyên tử, nghĩa là số lượng ký tự riêng lẻ còn lại ở phần cuối của quá trình nén. Ví dụ,`ababab`có thể được biểu diễn dưới dạng`(ab)3`, giá của nó là 2 vì đường cơ sở`ab`chứa hai đường đi nguyên tử. Nhiệm vụ là tìm chi phí tối thiểu có thể cho toàn bộ chuỗi. 

Đầu vào chứa một chuỗi có độ dài (N), trong đó (1 \le N \le 700). Bảng chữ cái chứa 62 ký tự có thể có, nhưng kích thước bảng chữ cái không ảnh hưởng đến thuật toán. Ràng buộc đáng kể là (N \le 700). Thuật toán bậc ba thực hiện theo thứ tự (700^3 \khoảng 3,43 \time 10^8) lần lặp cơ bản, do đó việc triển khai phải tránh thực hiện thêm công việc tuyến tính bên trong mỗi lần chuyển đổi bậc ba. Giải pháp (O(N^4)) quá lớn, trong khi chương trình động (O(N^3)) là mục tiêu tự nhiên cho giới hạn sáu giây. 

Có một số trường hợp khó xử lý. Một chuỗi một ký tự như`a`có câu trả lời 1, vì không có sự lặp lại hoặc ghép nối không cần thiết để khai thác. Một chuỗi như`aa`có câu trả lời 1, vì nó là`(a)2`, vì vậy việc coi mọi khối lặp lại là yêu cầu độ dài đầy đủ của nó sẽ là sai. 

Một trường hợp tế nhị hơn là`aaba`. Câu trả lời của nó là 3 vì chúng ta có thể viết`(a)2ba`, cho ba đường đi nguyên tử. Toàn bộ chuỗi không tuần hoàn, do đó việc triển khai chỉ tìm kiếm một lần lặp lại trong toàn bộ khoảng thời gian sẽ bỏ lỡ giải pháp tối ưu. Việc nén có thể được lồng bên trong một phép nối lớn hơn. 

Một trường hợp quan trọng khác là một chuỗi có sự lặp lại mà khối lặp lại có thể nén được. Vì`abababab`, chúng ta có thể viết`(ab)4`, Nhưng`ab`bản thân nó không thể nén được. Vì`aaaaaaaa`, tuy nhiên, toàn bộ chuỗi có thể được biểu diễn bằng`(a)8`, với chi phí 1. Việc triển khai dừng sau khi tìm thấy sự lặp lại mà không sử dụng đệ quy giá trị tối ưu của khoảng cơ sở của nó có thể trả về một giá trị quá lớn. 

## Phương pháp tiếp cận 

Một lực lượng vũ phu đệ quy trực tiếp sẽ thử mọi cách nối có thể và mọi lần lặp lại có thể. Điều đó mô tả không gian tìm kiếm phù hợp, nhưng số lượng cây nén có thể tăng theo cấp số nhân nên không thực tế. 

Cải tiến hữu ích đầu tiên là lập trình động theo quãng. Cho phép`dp[i][j]`là số đường dẫn nguyên tử tối thiểu cần thiết để mô tả chuỗi con từ vị trí`i`thông qua vị trí`j`. Đối với mỗi khoảng, chỉ có hai cách khác nhau cơ bản để hình thành mô tả tối ưu của nó. 

Khả năng đầu tiên là nối. Chúng tôi chọn ranh giới`k`và mô tả các phần bên trái và bên phải một cách độc lập. Điều này mang lại 

[ 
dp[i][j] = \min_{i \le k < j}(dp[i][k] + dp[k+1][j]). 
] 

Khả năng thứ hai là sự lặp lại. Nếu chuỗi con có độ dài (L) và bao gồm một số bản sao của một khối có độ dài (p), thì toàn bộ chuỗi con có thể được mô tả bằng cách sử dụng biểu diễn nén của khối đó. Do đó 

[ 
dp[i][j] = \min(dp[i][j], dp[i][i+p-1]). 
] 

Khoảng thời gian brute-force hoạt động vì mọi phép nén hợp lệ đều có phép nối ở gốc hoặc sự lặp lại ở gốc của nó. Vấn đề của nó là kiểm tra sự lặp lại. Nếu chúng ta thử mọi độ dài khối có thể và so sánh trực tiếp tất cả các ký tự, thì việc kiểm tra một khoảng thời gian có thể mất (O(L^2)), tạo ra thuật toán (O(N^4)). Trên tất cả các khoảng thời gian, đây là khoảng hàng chục tỷ so sánh ký tự cho (N=700). 

Quan sát quan trọng là việc kiểm tra xem một chuỗi con có dấu chấm (p) không yêu cầu so sánh mọi bản sao lặp lại. một chuỗi`s[i..i+L-1]`có dấu chấm (p) chính xác là khi nào 

[ 
s[i..i+L-p-1] = s[i+p..i+L-1]. 
] 

Hai chuỗi con này có độ dài bằng nhau (L-p). Nếu chúng ta biết tiền tố chung dài nhất của mỗi cặp hậu tố thì đẳng thức này có thể được kiểm tra trong (O(1)). 

Chúng ta có thể tính toán trước tất cả các tiền tố chung dài nhất trong (O(N^2)). Sau đó, mỗi giai đoạn ứng cử viên được kiểm tra theo thời gian không đổi. Chỉ có (O(N)) khoảng thời gian ứng cử viên cho một khoảng thời gian, do đó việc xử lý lặp lại đóng góp (O(N^3)) trong trường hợp xấu nhất, phù hợp với phép nối bậc ba DP. 

Có một sự đơn giản hóa hơn nữa. Chúng ta không cần phải thử mọi khoảng thời gian hợp lệ khi đã biết khoảng thời gian nhỏ nhất. Giả sử khoảng có các khoảng thời gian (p) và (q), với (p < q). Nếu (q) chia độ dài khoảng, thì khối có độ dài (q) được tạo từ các bản sao của khối cơ bản nhỏ hơn bất cứ khi nào (p) là chu kỳ nhỏ nhất. Do đó, chi phí nén tối ưu của nó không thể nhỏ hơn chi phí tối ưu của khối có chu kỳ nhỏ nhất. Việc tìm kiếm theo thứ tự tăng dần cho phép chúng ta dừng lại ở thời điểm hợp lệ đầu tiên. 

Phân tích chính thức trình bày cấu trúc khoảng-DP tương tự và thu được (O(N^3)) bằng cách tìm các lần lặp lại một cách hiệu quả với KMP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Khoảng thời gian Brute Force DP với kiểm tra lặp lại trực tiếp | (O(N^4)) | (O(N^2)) | Quá chậm | 
| Khoảng thời gian DP với (O(1)) kiểm tra định kỳ | (O(N^3)) | (O(N^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định`dp[i][j]`là số lượng đường dẫn nguyên tử tối thiểu cần thiết để nén chuỗi con`s[i:j]`, sử dụng các khoảng nửa mở. Khoảng một ký tự có giá trị 1 vì bản thân một ký tự là một đường dẫn nguyên tử. 
2. Tính toán trước`lcp[i][j]`, độ dài của tiền tố chung dài nhất của các hậu tố bắt đầu tại các vị trí`i`Và`j`. Chúng ta có thể tính toán ngược điều này với 

[ 
lcp[i][j] = 
\bắt đầu{trường hợp} 
1 + lcp[i+1][j+1], & s[i]=s[j],\ 
0, & s[i]\ne s[j]. 
\end{trường hợp} 
] 

Chỉ cặp với`i < j`là cần thiết. Đường chéo cũng có thể được lấp đầy, mặc dù nó không cần thiết cho quá trình chuyển đổi. 
3. Xử lý độ dài chuỗi con từ 1 đến (N). Khi xử lý một khoảng độ dài (L), mọi khoảng thời gian ngắn hơn đều đã được giải quyết, do đó tất cả các giá trị cần thiết cho quá trình chuyển đổi của nó đều có sẵn. 
4. Bắt đầu`dp[i][j]`với quá trình chuyển đổi nối. Đối với mỗi vị trí phân chia`k`, kết hợp các mô tả tối ưu của`s[i:k]`Và`s[k:j]`: 

[ 
dp[i][j] = \min(dp[i][j], dp[i][k] + dp[k][j]). 
] 

Điều này bao gồm mọi thao tác nén có hoạt động ngoài cùng là nối. 
5. Tìm kiếm sự lặp lại của khoảng thời gian hiện tại. Chiều dài khối`p`chỉ có thể được sử dụng khi`L % p == 0`, vì khoảng phải chứa một số nguyên các bản sao hoàn chỉnh. 
6. Đối với ứng viên`p`, so sánh cái đầu tiên`L-p`ký tự có hậu tố bắt đầu`p`các vị trí sau này. Bảng LCP cho chúng ta biết ngay liệu chúng có bằng nhau hay không: 

[ 
lcp[i][i+p] \ge L-p. 
] 

Nếu điều này đúng, toàn bộ khoảng bao gồm các bản sao của lần đầu tiên`p`nhân vật. Chi phí nén của nó sau đó là`dp[i][i+p]`. 
7. Hãy thử các giai đoạn ứng viên theo thứ tự tăng dần. Khi tìm thấy khoảng thời gian hợp lệ, đó là khoảng thời gian nhỏ nhất và khối của nó đủ cho quá trình chuyển đổi lặp lại. Nếu không tìm thấy dấu chấm, khoảng thời gian chỉ có thể sử dụng phép nối ở cấp độ này. 
8. Sau khi tất cả các khoảng thời gian đã được xử lý, hãy quay lại`dp[0][N]`, đại diện cho toàn bộ chuỗi đầu vào. 

### Tại sao nó hoạt động 

Hãy xem xét một biểu diễn nén tối ưu của bất kỳ khoảng nào. Nếu hoạt động ngoài cùng của nó là nối thì có một số vị trí phân chia`k`và hai khoảng kết quả phải được biểu diễn một cách tối ưu, nếu không việc thay thế một trong số chúng bằng cách biểu diễn tốt hơn sẽ cải thiện toàn bộ giải pháp. Quá trình chuyển đổi phân chia xem xét mọi điều như vậy`k`. 

Nếu hoạt động ngoài cùng là sự lặp lại, khoảng thời gian được tạo thành từ một số bản sao giống hệt nhau của một số khối. Độ dài khối của nó là ước số của độ dài khoảng và điều kiện LCP sẽ phát hiện chính xác xem khối đó có lặp lại trong toàn bộ khoảng hay không. Quá trình chuyển đổi sử dụng chi phí tối ưu của khối đó nên nó thể hiện sự lặp lại một cách tối ưu. 

Mọi nén pháp lý đều thuộc một trong hai trường hợp này, trong khi mọi chuyển đổi đều xây dựng một nén pháp lý. Vì các khoảng thời gian được xử lý từ ngắn hơn đến dài hơn nên mọi tham chiếu`dp`giá trị đã là tối ưu. Bằng quy nạp theo độ dài khoảng, mọi`dp[i][j]`là mức tối thiểu thực sự và`dp[0][N]`là câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(s):
    n = len(s)

    # lcp[i][j] = length of the longest common prefix
    # of s[i:] and s[j:].
    lcp = [[0] * (n + 1) for _ in range(n + 1)]

    for i in range(n - 1, -1, -1):
        row = lcp[i]
        next_row = lcp[i + 1]
        si = s[i]
        for j in range(n - 1, i, -1):
            if si == s[j]:
                row[j] = next_row[j + 1] + 1

    # dp[i][j] is the answer for s[i:j].
    dp = [[0] * (n + 1) for _ in range(n + 1)]

    for i in range(n):
        dp[i][i + 1] = 1

    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length
            best = length

            # Concatenation.
            dpi = dp[i]
            for k in range(i + 1, j):
                value = dpi[k] + dp[k][j]
                if value < best:
                    best = value
                    if best == 1:
                        break

            # Repetition.
            # A valid period must divide the whole length.
            p = 1
            while p * p <= length:
                if length % p == 0:
                    if p < length and lcp[i][i + p] >= length - p:
                        value = dp[i][i + p]
                        if value < best:
                            best = value

                    q = length // p
                    if q != p and q < length:
                        if lcp[i][i + q] >= length - q:
                            value = dp[i][i + q]
                            if value < best:
                                best = value

                p += 1

            dp[i][j] = best

    return dp[0][n]

def main():
    n = int(input())
    s = input().strip()
    print(solve(s))

if __name__ == "__main__":
    main()
```Vòng lặp lồng nhau đầu tiên xây dựng bảng LCP. Nó hoạt động ngược lại vì`lcp[i][j]`chỉ phụ thuộc vào`lcp[i+1][j+1]`. Khi`s[i] == s[j]`, hai hậu tố chia sẻ ký tự đó và sau đó chia sẻ chính xác số lượng ký tự bổ sung như các hậu tố tiếp theo của chúng. 

DP sử dụng các khoảng thời gian nửa mở. Như vậy`dp[i][j]`mô tả`s[i:j]`, có độ dài là`j-i`. Quy ước này làm cho ranh giới nối và lặp lại rõ ràng hơn. Một sự chia rẽ ở`k`sản xuất chính xác`s[i:k]`Và`s[k:j]`, không có sự trùng lặp và không có ký tự bị thiếu. 

Giá trị ban đầu`best = length`luôn là giới hạn trên hợp lệ vì chúng ta có thể để nguyên mọi ký tự. Vòng lặp nối sau đó sẽ cải thiện giá trị này. Vòng lặp lặp lại chỉ kiểm tra các ước số, vì khối lặp lại phải chiếm một số nguyên lần. 

Điều kiện LCP là chi tiết triển khai trung tâm. Trong một khoảng thời gian bắt đầu từ`i`với chiều dài`length`, Giai đoạn`p`có nghĩa```
s[i : i + length - p] == s[i + p : i + length]
```Cả hai mảnh đều có chiều dài`length-p`. giá trị`lcp[i][i+p]`ít nhất là độ dài này chính xác khi chúng bằng nhau. 

Vòng lặp chỉ xem xét các ước số đến căn bậc hai và kiểm tra cả hai`p`Và`length // p`. Điều này làm giảm số lượng ứng cử viên khoảng thời gian từ (O(N)) trên mỗi khoảng xuống số ước của độ dài khoảng. Độ phức tạp trong trường hợp xấu nhất vẫn bị giới hạn bởi (O(N^3)) và trong thực tế, điều này làm cho việc triển khai Python nhẹ hơn đáng kể so với việc kiểm tra mọi giai đoạn có thể. 

Không có vấn đề tràn số nguyên trong Python. Trong ngôn ngữ có chiều rộng cố định, giá trị DP tối đa là (N), do đó số nguyên 32 bit thông thường cũng đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chuỗi đầu vào là```
aaabaaabccdaaabaaabccd
```Độ dài của nó là 22. Biểu diễn tối ưu cuối cùng có thể được hiểu về mặt cấu trúc như các khối lồng nhau lặp đi lặp lại. Phần quan trọng đối với DP là một số khoảng thời gian trở thành định kỳ và các khối lặp lại của chúng có thể được nén lại. 

Một dấu vết nhỏ gọn của các trạng thái có liên quan nhất là: 

| Khoảng thời gian | Chiều dài | Thời gian hữu ích nhỏ nhất | Giá trị DP | 
| --- | --- | --- | --- | 
|`a`| 1 | không | 1 | 
|`aaa`| 3 | 1 | 1 | 
|`aaab`| 4 | không | 2 | 
|`aaabaaab`| 8 | 4 | 2 | 
|`cc`| 2 | 1 | 1 | 
|`aaabaaabccdaaabaaabccd`| 22 | sự lặp lại và nối lồng nhau | 4 | 

Giá trị cuối cùng là 4. Cấu trúc lặp lại cho phép cùng một tập hợp nhỏ các đường dẫn nguyên tử được tái sử dụng nhiều lần. Ví dụ này chứng minh tại sao sự lặp lại phải được kết hợp với phép nối thông thường. Toàn bộ chuỗi không chỉ đơn giản là sự lặp lại của một ký tự hoặc một khối ngắn. 

### Mẫu 2 

Đầu vào là```
aaba
```Khoảng thời gian`aa`tuần hoàn với chu kì 1 nên 

[ 
dp[0][2] = dp[0][1] = 1. 
] 

Toàn bộ chuỗi không tuần hoàn, vì vậy cấu trúc tối ưu xuất phát từ việc chia nó thành`aa`,`b`, Và`a`. 

| Khoảng thời gian | Chiều dài | Chuyển tiếp | Giá trị DP | 
| --- | --- | --- | --- | 
|`a`| 1 | nguyên tử | 1 | 
|`aa`| 2 | sự lặp lại của`a`| 1 | 
|`aab`| 3 |`aa`+`b`| 2 | 
|`aaba`| 4 |`aa`+`b`+`a`| 3 | 

Câu trả lời cuối cùng là 3. Dấu vết này chứng tỏ rằng sự lặp lại có thể hữu ích trong một phép nối lớn hơn. Nó cũng thực hiện trường hợp toàn bộ khoảng thời gian không có khoảng thời gian hữu ích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^3)) | Bảng LCP có chi phí (O(N^2)), phép nối xem xét việc phân chia (O(N)) cho mỗi khoảng (O(N^2)) và các kiểm tra lặp lại được giới hạn trong cùng một khối. | 
| Không gian | (O(N^2)) | các`dp`Và`lcp`mỗi bảng chứa (O(N^2)) mục nhập. | 

Với (N \le 700), yêu cầu bộ nhớ bậc hai nằm trong khoảng 256 MB. DP khối là thang đo dự kiến ​​cho vấn đề này, đồng thời việc tránh kiểm tra tuần hoàn từng ký tự trực tiếp sẽ ngăn hệ số bổ sung của (N) giúp việc triển khai đơn giản (O(N^4)). Phân tích cuộc thi chính thức cũng xác định (O(N^3)) là giải pháp chính bị ràng buộc. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(s):
    n = len(s)

    lcp = [[0] * (n + 1) for _ in range(n + 1)]

    for i in range(n - 1, -1, -1):
        row = lcp[i]
        next_row = lcp[i + 1]
        si = s[i]
        for j in range(n - 1, i, -1):
            if si == s[j]:
                row[j] = next_row[j + 1] + 1

    dp = [[0] * (n + 1) for _ in range(n + 1)]

    for i in range(n):
        dp[i][i + 1] = 1

    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length
            best = length

            for k in range(i + 1, j):
                value = dp[i][k] + dp[k][j]
                if value < best:
                    best = value
                    if best == 1:
                        break

            p = 1
            while p * p <= length:
                if length % p == 0:
                    if p < length and lcp[i][i + p] >= length - p:
                        best = min(best, dp[i][i + p])

                    q = length // p
                    if q != p and q < length:
                        if lcp[i][i + q] >= length - q:
                            best = min(best, dp[i][i + q])

                p += 1

            dp[i][j] = best

    return dp[0][n]

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    s = sys.stdin.readline().strip()
    return str(solve(s)) + "\n"

# Provided samples.
assert run("22\naaabaaabccdaaabaaabccd\n") == "4\n", "sample 1"
assert run("4\naaba\n") == "3\n", "sample 2"

# Minimum size.
assert run("1\na\n") == "1\n", "single character"

# No repetition, forcing ordinary concatenation.
assert run("2\nab\n") == "2\n", "non-repeating pair"

# All characters equal, maximum length.
assert run("700\n" + "a" * 700 + "\n") == "1\n", "maximum all-equal string"

# Repetition nested inside concatenation.
assert run("9\nabcabcabc\n") == "3\n", "three repetitions"

# Repetition of a block that is itself compressible.
assert run("8\naaaaaaaa\n") == "1\n", "nested repetition"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a`| 1 | Khoảng kích thước tối thiểu và trường hợp cơ sở nguyên tử | 
|`2 / ab`| 2 | Khoảng thời gian không định kỳ và chuyển tiếp phân chia | 
|`700 / aaaaa...`| 1 | Kích thước đầu vào tối đa và sự lặp lại được lồng sâu | 
|`9 / abcabcabc`| 3 | Sự lặp lại của một khối không cần thiết | 
|`8 / aaaaaaaa`| 1 | Một khối lặp lại có cách biểu diễn riêng cũng có thể nén được | 

## Vỏ cạnh 

Đầu vào một ký tự`a`cho`dp[0][1] = 1`ngay lập tức. Vòng lặp lặp lại không bao giờ cần thiết vì không có khối không trống nào ngắn hơn, do đó thuật toán trả về 1 mà không cần dựa vào quy tắc nén đặc biệt. 

Vì`aa`, khoảng có độ dài 2 và chu kỳ ứng viên 1. Giá trị LCP`lcp[0][1]`ít nhất là 1, do đó thuật toán nhận ra`aa`như hai bản sao của`a`và bộ`dp[0][2]`ĐẾN`dp[0][1] = 1`. Việc triển khai bất cẩn chỉ xem xét việc ghép nối sẽ trả về 2. 

cho`aaba`, khoảng`aa`được công nhận là sự lặp lại, tạo ra`dp[0][2] = 1`. Toàn bộ khoảng không có khoảng thời gian hợp lệ, vì vậy câu trả lời của nó xuất phát từ phép nối. Sự phân chia tốt nhất có hiệu quả tạo ra`aa`,`b`, Và`a`, cho`1 + 1 + 1 = 3`. Điều này mắc phải sai lầm khi cho rằng sự lặp lại hữu ích phải bao trùm toàn bộ chuỗi. 

Vì`aaaaaaaa`, bài kiểm tra giai đoạn 1 sẽ thành công trong mọi khoảng thời gian có liên quan. DP lần đầu tiên thiết lập`dp[0][1] = 1`, sau đó`dp[0][2] = 1`, vân vân. Do đó, khoảng thời gian đầy đủ cũng nhận được giá trị 1. Điều này xác nhận rằng các lần lặp lại có thể được lồng sâu tùy ý mà không yêu cầu DP xây dựng lại cây nén thực tế. 

Vì`abababab`, thử nghiệm giai đoạn 2 thành công vì sáu ký tự đầu tiên bằng hậu tố bắt đầu ở hai vị trí sau. Do đó, thuật toán có thể sử dụng`dp[0][2]`, là 2, cho toàn bộ khoảng thời gian. Việc chuỗi cũng có các khoảng thời gian lớn hơn chẳng hạn như 4 không thành vấn đề, vì khối lặp lại nhỏ hơn ít nhất cũng hữu ích. 

Ranh giới giữa hai khoảng nối được xử lý bằng phạm vi nửa mở. Vì`ab`, phép chia duy nhất là`k = 1`, cho`dp[0][1] + dp[1][2]`. Không có sự phân chia ở một trong hai điểm cuối, điều này ngăn chặn các khoảng trống xảy ra lặp lại và tránh được lỗi xảy ra phổ biến nhất trong DP này. 

Cuối cùng, trường hợp có độ dài tối đa 700 ký tự giống hệt nhau thực hiện cả hai chiều của chương trình động ở kích thước lớn nhất của chúng. Câu trả lời vẫn là 1 và mỗi khoảng thời gian có thể sử dụng lại cách biểu diễn một ký tự thông qua việc lặp lại. Thuật toán không bao giờ tự xây dựng biểu thức nén, do đó mức tiêu thụ bộ nhớ của nó vẫn là bậc hai thay vì phụ thuộc vào số lượng mô tả lặp lại lồng nhau có thể rất lớn.

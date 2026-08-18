---
title: "CF 102254B - Xây dựng cầu"
description: "Chúng ta có hai bờ sông, được biểu thị bằng chuỗi a và b. Vị trí i ở ngân hàng thứ nhất được dành cho đội có tên bởi a[i] và vị trí j ở ngân hàng thứ hai được dành cho đội có tên bởi b[j]. Một cây cầu chỉ có thể nối vị trí i và j khi hai chữ cái bằng nhau."
date: "2026-08-17T21:02:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102254
codeforces_index: "B"
codeforces_contest_name: "IME++ Starters Try-outs 2019"
rating: 0
weight: 102254
solve_time_s: 259
verified: false
draft: false
---

[CF 102254B - Xây cầu](https://codeforces.com/problemset/problem/102254/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai bờ sông, được biểu thị bằng những sợi dây`a`Và`b`. Chức vụ`i`ở ngân hàng đầu tiên được dành riêng cho đội được đặt tên bởi`a[i]`, và vị trí`j`ở bờ thứ hai được dành riêng cho đội được đặt tên bởi`b[j]`. Một cây cầu có thể kết nối các vị trí`i`Và`j`chỉ khi hai chữ cái bằng nhau. 

Những cây cầu không được bắc qua. Nếu một cây cầu nối`(i1, j1)`và một cái khác kết nối`(i2, j2)`, sau đó, sau khi sắp xếp các vị thế ngân hàng đầu tiên sao cho`i1 < i2`, chúng ta cũng phải có`j1 < j2`. Hai cây cầu có thứ tự đảo ngược sẽ cắt nhau về mặt hình học. Nhiệm vụ là tối đa hóa số lượng cặp chữ cái bằng nhau có thể được chọn trong khi vẫn giữ nguyên thứ tự này. Các ràng buộc ban đầu cho phép cả hai chuỗi có độ dài tối đa là 1000. 

Điều kiện đặt hàng này là chìa khóa của toàn bộ vấn đề. Một chuỗi các vị trí được chọn trong`a`và trình tự tương ứng trong`b`tạo thành hai dãy con và mỗi cặp được chọn đều có cùng một chữ cái. Nói cách khác, các cây cầu tạo thành một dãy con chung của hai dây. Do đó, câu trả lời là độ dài của dãy con chung dài nhất của chúng, hay LCS. 

Giới hạn 1000 đủ lớn để loại trừ các thuật toán liệt kê các tập hợp con hoặc thử mọi tập hợp cầu có thể có, nhưng đủ nhỏ cho một chương trình động bậc hai. MỘT`O(nm)`Thuật toán thực hiện tối đa một triệu chuyển đổi trạng thái khi cả hai chuỗi có độ dài 1000, phù hợp với các giới hạn đã cho. Một thuật toán hàm mũ như`O(2^n)`đã là không thể đối với`n = 1000`. 

Có một số trường hợp việc triển khai trực quan có thể gặp trục trặc. Ví dụ: nếu các chuỗi có các ký tự hoàn toàn khác nhau`a = "a"`Và`b = "b"`, câu trả lời là`0`. Việc thực hiện bất cẩn khi đếm các chữ cái phổ biến mà không tôn trọng vị trí thực tế của chúng vẫn có thể báo cáo sự trùng khớp. 

Các ký tự lặp đi lặp lại cũng cần được chăm sóc. Vì`a = "aa"`Và`b = "aa"`, câu trả lời là`2`, bởi vì cả hai cặp có thể được kết nối mà không cần giao nhau. Xử lý các chữ cái bằng nhau như thể chỉ có thể sử dụng một lần xuất hiện sẽ trả về không chính xác`1`. 

Thứ tự vẫn quan trọng ngay cả khi có nhiều chữ cái xuất hiện trong cả hai chuỗi. Vì`a = "abca"`Và`b = "acba"`, câu trả lời là`3`, sử dụng dãy con chung`"aca"`. Một thuật toán tham lam phù hợp với thuật toán đầu tiên`a`, sau đó lấy ngay cái đầu tiên có sẵn`b`, chỉ nhận được`2`và bỏ lỡ sự lựa chọn tốt hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là xem xét mọi tập hợp con các vị trí trong`a`. Mỗi tập hợp con xác định một dãy con của`a`và chúng ta có thể kiểm tra xem chuỗi con đó có xảy ra trong`b`đồng thời giữ gìn trật tự. Trong số tất cả các dãy con chung, ta giữ dãy con lớn nhất. Điều này đúng vì mọi tập hợp pháp của các cây cầu không cắt nhau đều tương ứng chính xác với một dãy con chung. 

có`2^n`tập hợp con của một`n`-chuỗi ký tự. Nếu kiểm tra một dãy con với`b`mất`O(m)`thời gian, công việc trong trường hợp xấu nhất là`O(m * 2^n)`. Với`n = m = 1000`, điều đó tùy thuộc vào`1000 * 2^1000`hoạt động ở cấp độ ký tự, vượt xa giới hạn. 

Cách tiếp cận bạo lực có hiệu quả vì nó khám phá rõ ràng mọi lựa chọn có trật tự hợp pháp. Nó thất bại khi số lượng các lựa chọn có thể trở nên rất lớn. Quan sát giúp mở ra giải pháp nhanh hơn là nhiều lựa chọn từng phần khác nhau có những khả năng tương lai giống hệt nhau. Khi chúng ta biết có thể tạo bao nhiêu cầu nối bằng cách sử dụng tiền tố của hai chuỗi, chúng ta không cần phải nhớ chính xác các cầu nối đã tạo ra giá trị đó. 

Định nghĩa`dp[i][j]`là số lượng cầu không bắc qua tối đa có thể được xây dựng chỉ bằng cầu đầu tiên`i`vị trí của`a`và lần đầu tiên`j`vị trí của`b`. Hãy xem xét vị trí cuối cùng của các tiền tố này. 

Nếu như`a[i - 1] == b[j - 1]`, chúng ta có thể kết nối hai vị trí đó và thu được`dp[i - 1][j - 1] + 1`. Chúng ta cũng có thể chọn không sử dụng một trong những vị trí đó, đưa ra`dp[i - 1][j]`hoặc`dp[i][j - 1]`. Như vậy,`dp[i][j] = max(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1] + 1)`khi các ký tự khớp nhau. 

Khi chúng khác nhau, hai vị trí cuối cùng không thể được kết nối trực tiếp, do đó phải loại trừ một trong số chúng:`dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])`. 

Đây là phép lặp LCS tiêu chuẩn, nhưng ở đây ý nghĩa hình học của nó đặc biệt hữu ích: việc chọn các ký tự trùng khớp sẽ tạo ra một cây cầu mới, trong khi việc chuyển sang tiền tố nhỏ hơn có nghĩa là không sử dụng một vị trí bờ sông. 

Vì mọi trạng thái chỉ phụ thuộc vào hàng trước và hàng hiện tại nên chúng ta có thể giảm bộ nhớ từ`O(nm)`ĐẾN`O(m)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(m * 2^n)`|`O(n)`| Quá chậm | 
| LCS DP tối ưu |`O(nm)`|`O(m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai chuỗi và đặt độ dài của chúng là`n`Và`m`. Các vị trí trong mỗi chuỗi đã đưa ra thứ tự dọc theo bờ sông tương ứng. 
2. Tạo hàng DP có độ dài`m + 1`, ban đầu chứa đầy số không.`dp[j]`đại diện cho câu trả lời tốt nhất cho phần của chuỗi đầu tiên được xử lý cho đến nay và chuỗi đầu tiên`j`vị trí của chuỗi thứ hai. 
3. Xử lý ký tự của`a`từ trái sang phải. Đối với mỗi ký tự mới, hãy tạo một hàng mới có giá trị đầu tiên bằng 0, vì tiền tố trống của`b`không thể chứa bất kỳ cây cầu nào. 
4. Đối với mọi vị trí`j`TRONG`b`, so sánh ký tự hiện tại`a[i - 1]`với`b[j - 1]`. Nếu chúng bằng nhau thì việc kết nối hai vị trí này là hợp pháp và cầu nối có thể được thêm vào một giải pháp tối ưu cho hai tiền tố trước đó. Giá trị ứng cử viên là`dp[j - 1] + 1`. 
5. Cho dù các ký tự có khớp hay không, hãy xem xét bỏ qua vị trí hiện tại của một trong hai ngân hàng. Những lựa chọn này cung cấp giá trị hàng trước`dp[j]`và giá trị hàng bên trái hiện tại`new_dp[j - 1]`. 
6. Lưu trữ tối đa các ứng cử viên này trong`new_dp[j]`, sau đó thay hàng cũ bằng hàng mới sau khi kết thúc vị trí hiện tại của`a`. 
7. Sau khi tất cả các ký tự đã được xử lý, giá trị DP cuối cùng là số lượng cầu không giao nhau tối đa có thể được xây dựng. Trở lại`dp[m]`. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý lần đầu tiên`i`nhân vật của`a`,`dp[j]`chính xác là số lượng cầu không vượt qua tối đa sử dụng những cầu đó`i`nhân vật và nhân vật đầu tiên`j`nhân vật của`b`. Mọi giải pháp pháp lý đều để lại một trong hai vị trí cuối cùng không được sử dụng hoặc khi thư của họ đồng ý thì sử dụng chúng làm cầu nối cuối cùng. Phép truy toán xem xét cả ba khả năng nên không thể loại bỏ một giải pháp tối ưu. Vì mọi quá trình chuyển đổi đều bảo toàn các vị trí ngày càng tăng trên cả hai bờ, nên mọi giá trị do DP xây dựng đều tương ứng với một tập hợp cầu không giao nhau hợp pháp. Cuối cùng, trạng thái của hai chuỗi hoàn chỉnh chính xác là mức tối đa mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = input().split()

    # Keep the DP dimension small.
    if len(b) > len(a):
        a, b = b, a

    m = len(b)
    dp = [0] * (m + 1)

    for ca in a:
        new_dp = [0] * (m + 1)

        for j in range(1, m + 1):
            if ca == b[j - 1]:
                new_dp[j] = dp[j - 1] + 1
            else:
                new_dp[j] = max(dp[j], new_dp[j - 1])

            # Even when the characters match, skipping either position
            # may be better than using this particular matching pair.
            if dp[j] > new_dp[j]:
                new_dp[j] = dp[j]
            if new_dp[j - 1] > new_dp[j]:
                new_dp[j] = new_dp[j - 1]

        dp = new_dp

    print(dp[m])

if __name__ == "__main__":
    solve()
```Việc hoán đổi tùy chọn lúc đầu làm cho`b`chuỗi ngắn hơn. Sự truy hồi là đối xứng trong hai chuỗi, vì vậy điều này không làm thay đổi câu trả lời. Nó chỉ làm giảm kích thước của mỗi hàng DP và do đó giảm mức sử dụng bộ nhớ.`dp[j]`là giá trị từ hàng trước đó, trong khi`new_dp[j - 1]`là giá trị ngay bên trái của hàng hiện tại. Khi các ký tự khớp nhau,`dp[j - 1] + 1`đại diện cho việc lấy cây cầu mới. Mã vẫn so sánh nó với cả hai cách bỏ qua một vị trí, vì việc khớp hai ký tự hiện tại không nhất thiết là một phần của LCS tối ưu. 

Việc kiểm tra rõ ràng sau lần gán đầu tiên dài dòng hơn một chút so với việc viết một bài toán lồng nhau`max`, nhưng chúng hiển thị ba chuyển đổi có thể có. Họ cũng tránh được một lỗi phổ biến khi trường hợp so khớp sử dụng một cách mù quáng`dp[j - 1] + 1`mặc dù lựa chọn khác sẽ cho kết quả lâu hơn. 

Không có vấn đề tràn số nguyên trong Python. Câu trả lời không bao giờ có thể vượt quá độ dài của chuỗi ngắn hơn, tối đa là 1000.`m + 1`việc lập chỉ mục mang lại cho DP một cột số 0 rõ ràng, vì vậy các vị trí`1`bởi vì`m`tương ứng trực tiếp với các ký tự`b[0]`bởi vì`b[m - 1]`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`a = "abacaba"`Và`b = "aba"`, dãy con chung tối ưu là`"aba"`, nên có thể xây được ba cây cầu không bắc qua. 

Bảng hiển thị hàng DP hoàn chỉnh sau mỗi ký tự của`a`. Mỗi hàng chứa câu trả lời cho mọi tiền tố của`b`. 

|`i`|`a[i]`| Hàng DP sau khi xử lý`a[i]`| 
| --- | --- | --- | 
| 0 | trống |`0 0 0 0`| 
| 1 |`a`|`0 1 1 1`| 
| 2 |`b`|`0 1 2 2`| 
| 3 |`a`|`0 1 2 3`| 
| 4 |`c`|`0 1 2 3`| 
| 5 |`a`|`0 1 2 3`| 
| 6 |`b`|`0 1 2 3`| 
| 7 |`a`|`0 1 2 3`| 

Sau ký tự thứ ba, DP đã tìm thấy`"aba"`. Các ký tự sau không thể giảm độ dài LCS nên đáp án cuối cùng vẫn là`3`. Bảng này thể hiện tính bất biến rằng mọi giá trị DP là kết quả tốt nhất cho cặp tiền tố tương ứng. 

### Mẫu 2 

cho`a = "aadodbcecoeo"`Và`b = "dcceoafas"`, đáp án tối ưu là`5`. 

|`i`|`a[i]`| Hàng DP sau khi xử lý`a[i]`| 
| --- | --- | --- | 
| 0 | trống |`0 0 0 0 0 0 0 0 0 0`| 
| 1 |`a`|`0 0 0 0 0 1 1 1 1 1`| 
| 2 |`a`|`0 0 0 0 0 1 1 1 1 1`| 
| 3 |`d`|`0 1 1 1 1 1 1 1 1 1`| 
| 4 |`o`|`0 1 1 1 1 2 2 2 2 2`| 
| 5 |`d`|`0 1 1 1 1 2 2 2 2 2`| 
| 6 |`b`|`0 1 1 1 1 2 2 2 2 2`| 
| 7 |`c`|`0 1 2 2 2 2 2 2 2 2`| 
| 8 |`e`|`0 1 2 2 3 3 3 3 3 3`| 
| 9 |`c`|`0 1 2 3 3 3 3 3 3 3`| 
| 10 |`o`|`0 1 2 3 3 4 4 4 4 4`| 
| 11 |`e`|`0 1 2 3 4 4 4 4 4 4`| 
| 12 |`o`|`0 1 2 3 4 5 5 5 5 5`| 

Giá trị đạt`5`khi trận chung kết`o`được xử lý. Một dãy con chung tối ưu là`"dceoo"`, với vị trí khớp tăng dần trong cả hai chuỗi. Ví dụ này cũng luyện tập các chữ cái lặp lại, trong đó việc chọn đúng lần xuất hiện là điều cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(nm)`| Mỗi cặp ký tự được kiểm tra một lần. | 
| Không gian |`O(min(n, m))`| Chỉ các hàng DP trước đó và hiện tại được lưu trữ. | 

Với cả hai chuỗi được giới hạn bởi 1000 ký tự, DP sẽ kiểm tra tối đa một triệu cặp ký tự. Việc sử dụng bộ nhớ tỷ lệ thuận với chuỗi ngắn hơn, nhiều nhất là 1001 mục nhập số nguyên trên mỗi hàng, do đó, giải pháp nằm trong giới hạn 1 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        a, b = sys.stdin.readline().split()

        if len(b) > len(a):
            a, b = b, a

        m = len(b)
        dp = [0] * (m + 1)

        for ca in a:
            new_dp = [0] * (m + 1)

            for j in range(1, m + 1):
                if ca == b[j - 1]:
                    new_dp[j] = dp[j - 1] + 1
                else:
                    new_dp[j] = max(dp[j], new_dp[j - 1])

                if dp[j] > new_dp[j]:
                    new_dp[j] = dp[j]
                if new_dp[j - 1] > new_dp[j]:
                    new_dp[j] = new_dp[j - 1]

            dp = new_dp

        print(dp[m])
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert solution("abacaba aba\n") == "3", "sample 1"
assert solution("aadodbcecoeo dcceoafas\n") == "5", "sample 2"

# Minimum-size input
assert solution("a a\n") == "1", "single matching position"

# No matching characters
assert solution("a b\n") == "0", "no common character"

# Repeated characters
assert solution("aaa aa\n") == "2", "repeated characters"

# Order matters, and greedy matching can fail
assert solution("abca acba\n") == "3", "order-sensitive LCS"

# Maximum-size input
assert solution("a" * 1000 + " " + "a" * 1000 + "\n") == "1000", \
    "maximum-size all-equal strings"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a a`|`1`| Ranh giới kích thước tối thiểu | 
|`a b`|`0`| Không có cây cầu nào | 
|`aaa aa`|`2`| Vị trí khớp lặp đi lặp lại | 
|`abca acba`|`3`| Xử lý đúng trật tự và bẫy tham lam | 
| 1000`a`ký tự ở mỗi bên |`1000`| Kích thước đầu vào tối đa và giá trị lặp lại | 

## Vỏ cạnh 

Đối với các ký tự hoàn toàn khác nhau, đầu vào`a b`tạo ra một so sánh DP duy nhất. Từ`a != b`, không vị trí nào có thể được ghép nối và kết quả vẫn giữ nguyên`0`. Điều này nắm bắt các triển khai đếm số lượng vị trí thay vì các ký tự khớp thực tế. 

Đối với các ký tự lặp lại,`aaa aa`bắt đầu bằng một hàng số 0 và xử lý từng hàng`a`. đầu tiên`a`đưa ra câu trả lời cho`1`, và thứ hai`a`nâng nó lên`2`. Ký tự thứ ba không còn vị trí ở ngân hàng thứ hai nên đáp án giữ nguyên`2`. Thuật toán sử dụng lại một cách chính xác các lần xuất hiện khác nhau thay vì coi chính ký tự đó chỉ có thể sử dụng được một lần. 

Đối với trường hợp nhạy cảm với thứ tự,`abca acba`có ba cây cầu phù hợp có sẵn thông qua chuỗi con`"aca"`. Một phương pháp tham lam chiếm ưu thế đầu tiên`a`, thì điều đầu tiên có thể`b`, sử dụng thứ tự hữu ích và chỉ thu được hai cây cầu. DP duy trì cả hai khả năng thông qua các trạng thái riêng biệt, cuối cùng đạt đến`3`. 

Đối với ranh giới tối đa, hai chuỗi gồm 1000 bản sao`a`sản xuất`1000`. Mọi vị trí ở phía ngắn hơn đều có thể được so khớp và DP tiếp cận thêm một cầu nối cho mỗi vị trí được xử lý cho đến khi đạt đến 1000. Công thức bậc hai chính xác là khoảng một triệu chuyển đổi trạng thái, đây là thang đo dự kiến ​​cho các ràng buộc.

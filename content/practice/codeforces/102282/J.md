---
title: "CF 102282J - \u041f\u043e\u0441\u043b\u0435\u0434\u043d\u044f\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Chúng ta có hai chuỗi, s1 và s2. Chuỗi đầu tiên là văn bản đáng lẽ phải được sao chép, trong khi chuỗi thứ hai là văn bản thực sự được viết. Trong quá trình sao chép, có thể xảy ra hai loại lỗi: một ký tự có thể bị bỏ qua hoặc một ký tự có thể được thay thế bằng một ký tự khác."
date: "2026-08-13T09:16:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102282
codeforces_index: "J"
codeforces_contest_name: "2011, \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u043a\u043e\u043d\u0442\u0435\u0441\u0442 \u0421\u0413\u0410\u0423 \u043d\u0430 \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b ACM ICPC"
rating: 0
weight: 102282
solve_time_s: 59
verified: true
draft: false
---

[CF 102282J - \u041f\u043e\u0441\u043b\u0435\u0434\u043d\u044f\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102282/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai chuỗi,`s1`Và`s2`. Chuỗi đầu tiên là văn bản đáng lẽ phải được sao chép, trong khi chuỗi thứ hai là văn bản thực sự được viết. Trong quá trình sao chép, có thể xảy ra hai loại lỗi: một ký tự có thể bị bỏ qua hoặc một ký tự có thể được thay thế bằng một ký tự khác. Chúng ta cần số lượng sai lầm tối thiểu như vậy để biến đổi`s1`vào trong`s2`. 

Sự khác biệt giữa hai hoạt động quan trọng. Chúng ta không được phép chèn ký tự mới vào`s1`. Do đó,`s2`không thể dài hơn`s1`và bài toán đảm bảo rằng cặp đã cho là hợp lệ theo các quy tắc này. Việc thay thế sẽ gây ra một lỗi, bất kể có liên quan đến hai chữ cái nào và việc bỏ sót một ký tự cũng gây ra một lỗi. 

Cả hai chuỗi đều có độ dài tối đa là 1024. Do đó, thuật toán bậc hai thực hiện tối đa khoảng một triệu chuyển đổi trạng thái, có thể dễ dàng quản lý trong giới hạn một giây. Tuy nhiên, việc tìm kiếm theo cấp số nhân trên tất cả các cách có thể để căn chỉnh các chuỗi sẽ trở nên tốn kém không cần thiết ngay cả ở độ dài nhỏ này. 

Có một số trường hợp ranh giới có thể khiến việc triển khai bất cẩn không chính xác. Nếu các chuỗi đã bằng nhau, chẳng hạn như```
abc
abc
```câu trả lời là`0`. Việc triển khai đếm các vị trí trong đó các ký tự khác nhau mà không xem xét khả năng các ký tự bị bỏ qua ở đây là ổn, nhưng nó sẽ thất bại trong các trường hợp tổng quát hơn. 

Ký tự bị bỏ qua có thể xảy ra ở đầu hoặc cuối. Ví dụ,```
abc
bc
```có câu trả lời`1`, bởi vì đầu tiên`a`có thể được bỏ qua. Tương tự,```
abc
ab
```có câu trả lời`1`, bởi vì trận chung kết`c`có thể được bỏ qua. Một cách tiếp cận chỉ so sánh các vị trí có độ dài bằng nhau không thể biểu diễn chính xác một trong hai thao tác. 

Ký tự bị bỏ qua cũng có thể xảy ra giữa hai ký tự trùng khớp:```
abc
ac
```Câu trả lời là một lần nữa`1`, bởi vì`b`được bỏ qua. Một sự thực hiện tham lam sẽ xử lý ngay lập tức`b`Và`c`như một sự thay thế sẽ có được`1`ở đây là do trùng hợp ngẫu nhiên, nhưng các chuỗi phức tạp hơn có thể khiến cho quyết định cục bộ như vậy trở nên dưới mức tối ưu. 

Cuối cùng, việc thay thế và xóa có thể xảy ra trong cùng một tiền tố. Ví dụ,```
abcd
acd
```chỉ cần một lỗi, xóa`b`. Công thức DP phải phân biệt giữa việc khớp các ký tự hiện tại và xóa ký tự hiện tại của`s1`và thay thế ký tự hiện tại. Việc coi mọi sự không phù hợp như một sự thay thế sẽ làm mất đi khả năng điều chỉnh rẻ hơn sau này. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp nhất là liệt kê tất cả sự sắp xếp có thể có giữa hai chuỗi. Khi các ký tự hiện tại bằng nhau, chúng có thể được so khớp. Khi chúng khác nhau, có một số khả năng: thay thế ký tự hiện tại của`s1`hoặc xóa nó và cố gắng khớp cùng một ký tự của`s2`chống lại một vị trí sau này của`s1`. Việc khám phá các lựa chọn này một cách đệ quy là đúng vì mọi chuỗi lỗi hợp lệ đều tương ứng với một đường đi qua cây quyết định này. 

Vấn đề với cách tiếp cận này là các hậu tố giống nhau được sử dụng nhiều lần. Một chuỗi chứa nhiều điểm không khớp có thể phân nhánh nhiều lần giữa việc thay thế và xóa. Trong trường hợp xấu nhất, số đường dẫn đệ quy tăng theo cấp số nhân, gần bằng`O(2^n)`cho một chuỗi dài`n`. Với`n = 1024`, điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là kết quả của một bài toán con chỉ phụ thuộc vào hai vị trí chứ không phụ thuộc vào toàn bộ lịch sử dẫn đến đó. Giả sử chúng ta đã xử lý xong dữ liệu đầu tiên`i`nhân vật của`s1`và lần đầu tiên`j`nhân vật của`s2`. Mọi thứ trước khi các vị thế đó kết thúc, vì vậy thông tin liên quan duy nhất là cặp tiền`(i, j)`. 

Điều đó biến vấn đề thành lập trình động. Cho phép`dp[i][j]`là số lỗi tối thiểu cần thiết để chuyển đổi lần đầu tiên`i`nhân vật của`s1`vào đầu tiên`j`nhân vật của`s2`. 

Từ tiểu bang`(i, j)`, có ba cách có thể để hoàn thành các ký tự hiện tại. Chúng ta có thể sánh đôi`s1[i-1]`với`s2[j-1]`với chi phí bằng 0 nếu chúng bằng nhau hoặc thay thế`s1[i-1]`với`s2[j-1]`với giá một nếu chúng khác nhau. Chúng ta cũng có thể xóa`s1[i-1]`, chuyển động đến`(i-1, j)`và chi phí một. 

Không có sự chuyển đổi tương ứng với việc chèn vào`s1`, vì việc chèn không phải là một trong những sai lầm được phép. Đây là sự khác biệt đáng kể duy nhất so với vấn đề khoảng cách chỉnh sửa đầy đủ thông thường. 

Phép đệ quy brute-force hoạt động vì nó khám phá rõ ràng những lựa chọn tương tự này. Lập trình động chỉ đơn giản nhận ra rằng nhiều đường dẫn đệ quy khác nhau có cùng một`(i, j)`nêu và lưu trữ câu trả lời tốt nhất của nó một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^n)`trong trường hợp xấu nhất |`O(n)`độ sâu đệ quy | Quá chậm | 
| DP tối ưu |`O(nm)`|`O(nm)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`n = len(s1)`Và`m = len(s2)`. Tạo một bảng DP trong đó`dp[i][j]`đại diện cho số lỗi tối thiểu cần thiết để chuyển đổi`s1[:i]`vào trong`s2[:j]`. 
2. Khởi tạo`dp[0][0] = 0`. Việc chuyển đổi một tiền tố trống thành một tiền tố trống khác không có sai sót. 
3. Khởi tạo cột đầu tiên với`dp[i][0] = i`. Nếu tiền tố đích trống, mỗi tiền tố đầu tiên`i`nhân vật của`s1`chính xác là phải xóa đi`i`những sai sót là cần thiết. 
4. Khởi tạo hàng đầu tiên với`dp[0][j]`như không thể đối với mọi người`j > 0`. Chuỗi nguồn trống không thể tạo ra mục tiêu không trống vì thao tác chèn không được phép. Chúng ta có thể biểu diễn những trạng thái này với một giá trị rất lớn. 
5. Xử lý mọi trạng thái`(i, j)`với`i >= 1`Và`j >= 1`. Đầu tiên hãy xem xét việc tiêu thụ cả hai ký tự hiện tại. Nếu như`s1[i-1] == s2[j-1]`, điều này không gây thêm lỗi. Nếu không, việc thay thế một ký tự bằng ký tự khác sẽ tốn một ký tự. 
6. Cũng cân nhắc việc xóa`s1[i-1]`. Điều này thay đổi trạng thái từ`(i, j)`ĐẾN`(i-1, j)`và thêm một lỗi. 
7. Lấy chi phí nhỏ hơn giữa việc khớp hoặc thay thế ký tự và xóa. Giá trị thu được là câu trả lời tối ưu cho`dp[i][j]`. 
8. Sau khi điền vào bảng, xuất ra`dp[n][m]`, đại diện cho việc chuyển đổi toàn bộ chuỗi đầu tiên thành toàn bộ chuỗi thứ hai. 

### Tại sao nó hoạt động 

Tính bất biến đó là`dp[i][j]`luôn lưu trữ số lỗi tối thiểu có thể để chuyển đổi chính xác lần đầu tiên`i`nhân vật của`s1`vào chính xác cái đầu tiên`j`nhân vật của`s2`. 

Hãy xem xét một phép biến đổi tối ưu cho một trạng thái`(i, j)`. Hành động cuối cùng của nó phải sử dụng cả hai ký tự cuối cùng, có nghĩa là chúng khớp với nhau hoặc ký tự này được thay thế bằng ký tự kia hoặc xóa ký tự cuối cùng của tiền tố nguồn. Đây chính xác là những chuyển đổi được xem xét bởi sự tái phát. Vì mỗi quá trình chuyển đổi sẽ cộng thêm chính xác chi phí cho hoạt động cuối cùng của nó và trạng thái trước đó là tối ưu theo bất biến DP, nên việc lấy mức tối thiểu trong các quá trình chuyển đổi sẽ mang lại giá trị tối ưu cho`(i, j)`. Các trường hợp cơ sở bao gồm tất cả các phép biến đổi liên quan đến tiền tố trống, do đó, bất biến giữ cho toàn bộ bảng và trạng thái cuối cùng là mức tối thiểu bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s1 = input().strip()
    s2 = input().strip()

    n = len(s1)
    m = len(s2)

    INF = n + m + 1

    dp = [[INF] * (m + 1) for _ in range(n + 1)]
    dp[0][0] = 0

    for i in range(1, n + 1):
        dp[i][0] = i

    for i in range(1, n + 1):
        for j in range(1, m + 1):
            cost = 0 if s1[i - 1] == s2[j - 1] else 1

            # Match the two characters, or replace s1[i - 1].
            dp[i][j] = dp[i - 1][j - 1] + cost

            # Delete s1[i - 1].
            dp[i][j] = min(dp[i][j], dp[i - 1][j] + 1)

    print(dp[n][m])

if __name__ == "__main__":
    solve()
```Hai dòng đầu vào đầu tiên chứa hai chuỗi, vì vậy`strip()`là đủ để loại bỏ các dòng mới ở cuối chúng. Không có nhiều trường hợp thử nghiệm ở định dạng đầu vào. 

Bảng DP có`(n + 1) * (m + 1)`tiểu bang. Cột đầu tiên được khởi tạo thành`i`bởi vì chuyển đổi`s1[:i]`thành một chuỗi trống yêu cầu xóa tất cả`i`nhân vật. 

Hàng đầu tiên được để lại tại`INF`ngoại trừ`dp[0][0]`. Đây là cố ý. Không thể xây dựng mục tiêu có độ dài dương từ nguồn trống chỉ bằng cách xóa và thay thế. Việc sử dụng một giá trị không thể rõ ràng sẽ ngăn chặn việc chuyển đổi ngẫu nhiên khỏi việc phát minh ra thao tác chèn. 

Đối với mỗi trạng thái thông thường,`cost`là số không hoặc một. Nếu các nhân vật hiện tại đồng ý, việc tiêu thụ cả hai đều không tốn kém gì. Nếu họ không đồng ý, việc tiêu thụ cả hai sẽ tương ứng với một sự thay thế. Quá trình chuyển đổi thứ hai xóa ký tự nguồn hiện tại và giữ nguyên vị trí đích. 

Các chỉ số sử dụng`i - 1`Và`j - 1`bởi vì`dp[i][j]`mô tả tiền tố có độ dài`i`Và`j`. Đây là chi tiết chính từng bước một trong quá trình thực hiện. Số nguyên Python không có vấn đề tràn và câu trả lời có ý nghĩa lớn nhất nhiều nhất là`n`, vì đầu vào hợp lệ đảm bảo rằng`s2`có thể thu được bằng cách xóa các ký tự và thay thế các ký tự từ`s1`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, các chuỗi là`flash`Và`flesh`. Các trạng thái DP có liên quan có thể được theo dõi như sau. 

|`i`|`j`|`s1[i-1]`|`s2[j-1]`| Chi phí khớp/thay thế | Xóa chi phí |`dp[i][j]`| 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | f | f | 0 | 2 | 0 | 
| 2 | 2 | tôi | tôi | 0 | 2 | 0 | 
| 3 | 3 | một | e | 1 | 1 | 1 | 
| 4 | 4 | s | s | 0 | 2 | 1 | 
| 5 | 5 | h | h | 0 | 2 | 1 | 

Hai chuỗi đồng ý ở mọi nơi ngoại trừ`a`so với`e`. Thay thế`a`với`e`gây ra một lỗi nên trạng thái cuối cùng là`dp[5][5] = 1`. 

Đối với mẫu thứ hai, các chuỗi là`bread`Và`beer`. Độ dài khác nhau nên cần phải xóa ít nhất một lần. 

|`i`|`j`|`s1[i-1]`|`s2[j-1]`| Chi phí khớp/thay thế | Xóa chi phí |`dp[i][j]`| 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | b | b | 0 | 2 | 0 | 
| 2 | 2 | r | e | 1 | 1 | 1 | 
| 3 | 2 | e | e | 0 | 1 | 1 | 
| 4 | 3 | một | e | 1 | 2 | 1 | 
| 5 | 4 | d | r | 1 | 2 | 2 | 

Điểm chuyển tiếp quan trọng nằm ở giữa dây. Thay vì thay thế`r`với`e`và mất đi sự liên kết hữu ích, DP có thể xóa`r`và sau đó khớp với bản gốc`e`. Các ký tự còn lại có thể được căn chỉnh bằng một ký tự thay thế nữa, hạn chế tối thiểu hai lỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(nm)`| có`(n+1)(m+1)`trạng thái và mỗi trạng thái mất thời gian không đổi. | 
| Không gian |`O(nm)`| Bảng DP hoàn chỉnh được lưu trữ. | 

Với cả hai chuỗi được giới hạn ở 1024 ký tự, bảng chỉ chứa khoảng 1,05 triệu trạng thái. Mỗi trạng thái thực hiện một số lượng không đổi các phép toán số nguyên, do đó nghiệm nằm trong giới hạn đã nêu. Mức tiêu thụ bộ nhớ cũng nhỏ so với 128 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.splitlines()
    s1 = data[0].strip()
    s2 = data[1].strip()

    n = len(s1)
    m = len(s2)

    INF = n + m + 1
    dp = [[INF] * (m + 1) for _ in range(n + 1)]

    dp[0][0] = 0

    for i in range(1, n + 1):
        dp[i][0] = i

    for i in range(1, n + 1):
        for j in range(1, m + 1):
            cost = 0 if s1[i - 1] == s2[j - 1] else 1
            dp[i][j] = min(
                dp[i - 1][j - 1] + cost,
                dp[i - 1][j] + 1
            )

    return str(dp[n][m])

# Provided sample 1
assert solve_data("flash\nflesh\n") == "1", "sample 1"

# Provided sample 2
assert solve_data("bread\nbeer\n") == "2", "sample 2"

# Minimum-size strings, equal characters
assert solve_data("a\na\n") == "0", "equal one-character strings"

# Minimum-size strings, replacement
assert solve_data("a\nb\n") == "1", "one-character replacement"

# Deletion at the beginning
assert solve_data("abc\nbc\n") == "1", "deletion at beginning"

# Deletion at the end
assert solve_data("abc\nab\n") == "1", "deletion at end"

# Deletion in the middle
assert solve_data("abc\nac\n") == "1", "deletion in middle"

# All characters equal, maximum length
s = "a" * 1024
assert solve_data(s + "\n" + s + "\n") == "0", "maximum equal strings"

# Maximum number of deletions
s1 = "a" * 1024
s2 = "a"
assert solve_data(s1 + "\n" + s2 + "\n") == "1023", "maximum deletions"

# Several operations together
assert solve_data("abcdef\nacef\n") == "2", "multiple deletions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`/`a`|`0`| Đầu vào có kích thước tối thiểu và không có lỗi | 
|`a`/`b`|`1`| Chuyển tiếp thay thế | 
|`abc`/`bc`|`1`| Xóa ở đầu | 
|`abc`/`ab`|`1`| Xóa ở cuối và xử lý ranh giới | 
|`abc`/`ac`|`1`| Xóa ở giữa | 
|`a...a`có độ dài 1024 / cùng một chuỗi |`0`| Kích thước đầu vào tối đa và tất cả các ký tự bằng nhau | 
|`a...a`có chiều dài 1024 /`a`|`1023`| Số lượng xóa lớn | 
|`abcdef`/`acef`|`2`| Xóa nhiều lần mà không vô tình thay thế | 

## Vỏ cạnh 

Trường hợp ranh giới đầu tiên là xóa ngay từ đầu. Vì```
abc
bc
```DP đạt tới`dp[1][0] = 1`, đại diện cho việc xóa`a`. Từ đó,`b`trận đấu`b`Và`c`trận đấu`c`, Vì thế`dp[3][2] = 1`. So sánh theo từng vị trí sẽ thấy`a != b`và mô tả không chính xác thao tác đầu tiên là thao tác thay thế, mặc dù căn chỉnh tối ưu là thao tác xóa. 

Việc xóa ở cuối được xử lý bởi cột đầu tiên của DP. Vì```
abc
ab
```tiểu bang`dp[2][2]`trở thành số không vì`ab`đã khớp rồi. Di chuyển từ`(2, 2)`ĐẾN`(3, 2)`sử dụng quá trình chuyển đổi xóa, tạo ra`dp[3][2] = 1`. Thuật toán không cần trường hợp đặc biệt cho ký tự cuối cùng vì cùng một phép lặp xử lý nó một cách tự nhiên. 

Việc xóa ở giữa chứng minh lý do tại sao DP phải duy trì các sắp xếp thay thế. Vì```
abc
ac
```đường dẫn tối ưu phù hợp`a`, xóa`b`, và sau đó khớp`c`. Các trạng thái tương ứng là`dp[1][1] = 0`,`dp[2][1] = 1`, Và`dp[3][2] = 1`. Một trình xử lý không khớp tham lam có thể thay thế`b`với`c`, nhưng DP giữ cả hai khả năng và chọn cách tiếp tục rẻ hơn. 

Đối với các chuỗi một ký tự bằng nhau,```
a
a
```quá trình chuyển đổi theo đường chéo có chi phí bằng 0, mang lại`dp[1][1] = 0`. Điều này kiểm tra xem các ký tự trùng khớp không bao giờ được tính là lỗi. 

Ở kích thước đầu vào tối đa, hãy xem xét hai chuỗi giống hệt nhau bao gồm 1024`a`nhân vật. Mỗi lần chuyển đổi theo đường chéo đều có chi phí bằng 0, vì vậy câu trả lời cuối cùng vẫn là 0 sau khoảng một triệu lần cập nhật trạng thái liên tục. Thuật toán chia tỷ lệ trực tiếp với giới hạn độ dài đã nêu và không dựa vào độ sâu đệ quy hoặc phân nhánh theo cấp số nhân. 

Cực đoan ngược lại là nguồn 1024 ký tự và mục tiêu một ký tự:```
aaaaaaaa...
a
```Cột đầu tiên ghi lại chi phí xóa các tiền tố nguồn tùy ý và DP có thể giữ lại một tiền tố nguồn.`a`làm ký tự mục tiêu phù hợp trong khi xóa 1023 ký tự còn lại. Câu trả lời cuối cùng là`1023`, xác nhận rằng quá trình khởi tạo cho mục tiêu trống và quá trình chuyển đổi xóa hoạt động chính xác ở ranh giới lớn nhất.

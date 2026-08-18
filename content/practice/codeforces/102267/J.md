---
title: "CF 102267J - Sở thú"
description: "Sở thú là một vòng tròn gồm n địa điểm. Một công dân chọn vị trí xuất phát a, vị trí khác b và một trong hai hướng quay quanh vòng tròn kết nối chúng. Đường đi đơn được chọn có tối đa k cạnh."
date: "2026-08-17T19:31:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102267
codeforces_index: "J"
codeforces_contest_name: "The 2019 University of Jordan Collegiate Programming Contest"
rating: 0
weight: 102267
solve_time_s: 241
verified: false
draft: false
---

[CF 102267J - Sở thú](https://codeforces.com/problemset/problem/102267/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Sở thú là một vòng quay`n`địa điểm. Người dân chọn nơi xuất phát`a`, vị trí khác`b`và một trong hai hướng xung quanh chu trình kết nối chúng. Đường đi đơn được chọn có nhiều nhất`k`các cạnh. Sau đó người dân sẽ đi bộ dọc theo con đường đó, bắt đầu và kết thúc tại`a`, ghé thăm mọi vị trí của con đường đã chọn, với nhiều nhất`m`di chuyển. 

Nhiệm vụ là đếm tất cả các lần đi bộ như vậy theo modulo`10^9 + 7`. 

Một cách hữu ích để tạm thời quên đi chu trình là cố định vị trí bắt đầu và một hướng. Đánh số các vị trí dọc theo hướng đó là`0, 1, 2, ...`. Việc đi bộ khi đó chỉ đơn giản là một chuỗi các bước di chuyển bằng`+1`hoặc`-1`. Nó bắt đầu lúc`0`, không bao giờ được trở thành số âm, không bao giờ được đi xa hơn`k`, và cuối cùng phải quay lại`0`. Vì cuộc đi bộ phải đến điểm cuối đã chọn`b`, vị trí đạt tới tối đa của nó sẽ xác định điểm cuối của đường dẫn đã chọn. 

Ràng buộc`n <= 10^5`ngay lập tức loại trừ bất cứ điều gì tỷ lệ thuận với`n^2`, và thời gian chỉ có một giây. tham số`m <= 2000`nhỏ hơn nhiều, điều này gợi ý rõ ràng rằng quy hoạch động thực tế sẽ phụ thuộc vào`m`hơn là vào kích thước của chu kỳ. Từ`k`có thể lớn như`10^5`, chúng ta cũng không thể có được một không gian trạng thái bao gồm tất cả các cặp vị trí chu trình. 

Có một số cách dễ dàng để đếm sai. Vì`2 1 1`, câu trả lời là`0`, bởi vì một chuyến đi khác trống bắt đầu và kết thúc ở cùng một vị trí có độ dài chẵn. Một giải pháp xử lý số lượng địa điểm đã ghé thăm dưới dạng chiều dài đi bộ sẽ tính không chính xác nội dung nào đó ở đây. 

Vì`2 1 2`, câu trả lời là`8`. Có hai vị trí bắt đầu và hai lựa chọn hướng đi. Đối với mỗi lựa chọn, bước đi hợp lệ duy nhất là`a -> neighbor -> a`. Quên đi hai hướng cho`4`thay vì`8`. 

Vì`3 2 4`, câu trả lời là`18`. Với vị trí và hướng xuất phát cố định, các bước đi về hợp lệ có độ dài`2`Và`4`. Có một bước đi dài`2`, và chiều dài hai bước`4`, đưa ra ba lần đi bộ cho mỗi`2n = 6`lựa chọn hướng xuất phát. Một lỗi phổ biến là đếm từng độ dài đường đi một cách độc lập và vô tình đếm cùng một bước đi theo một số điểm cuối có thể xảy ra. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp có thể chọn vị trí bắt đầu, chọn một trong hai hướng, chọn khoảng cách điểm cuối và liệt kê mọi chuỗi di chuyển trái và phải theo chiều dài`m`. Đối với đường đi cố định, có`2^L`trình tự có thể có chính xác`L`di chuyển, do đó liệt kê tất cả các độ dài lên đến`m`yêu cầu`2 + 4 + ... + 2^m = 2^(m+1) - 2`trình tự. Trên tất cả các vị trí bắt đầu, chỉ đường và độ dài đường đi, số lượng thao tác là`2 * n * k * (2^(m+1) - 2)`. 

Ở các giá trị lớn nhất, giá trị này chứa hệ số xấp xỉ`2^2000`, do đó ngay cả phần đầu tiên của không gian tìm kiếm cũng vượt xa những gì có thể xử lý được. 

Lý do vũ lực hoạt động về mặt khái niệm là vì mỗi bước đi chỉ là một chuỗi gồm hai bước di chuyển có thể xảy ra. Vấn đề là hầu hết các trình tự đó đều không liên quan vì chúng rời khỏi đường dẫn đã chọn hoặc không thể quay lại từ đầu. Thay vì tạo ra chúng, chúng ta chỉ có thể đếm những chuỗi vẫn có thể xảy ra sau mỗi số nước đi. 

Sửa vị trí bắt đầu và hướng. Cho phép`j`là khoảng cách hiện tại từ vị trí bắt đầu dọc theo hướng đó. Việc đi bộ có thể di chuyển từ`j`ĐẾN`j-1`hoặc`j+1`. Nó có thể không bao giờ đạt đến vị trí âm vì điều đó sẽ khiến đường đi đã chọn ở sau điểm xuất phát. Nó có thể không bao giờ vượt quá`k`vì đường dẫn đã chọn có độ dài tối đa`k`. 

Điều này mang lại một trạng thái lập trình động nhỏ. Sau đó`i`di chuyển,`dp[i][j]`đếm tất cả các tiền tố hợp lệ hiện ở khoảng cách`j`. Cuộc đi bộ trở về chính xác là một trạng thái có`j = 0`. Tổng các trạng thái đó theo tất cả các độ dài dương cho đến`m`đếm mọi bước đi trừu tượng có thể có cho một vị trí xuất phát và một hướng. 

Có một quan sát đặc biệt hữu ích đằng sau phép nhân cuối cùng. Thực hiện bất kỳ chuyến đi bộ trở về hợp lệ nào, không trống để có vị trí và hướng xuất phát cố định. Cho phép`h`là khoảng cách tối đa mà nó đạt được. Vì bước đi di chuyển từng cạnh một nên nó ghé thăm mọi khoảng cách từ`0`bởi vì`h`. Chúng ta có thể chọn điểm cuối`b`là vị trí ở khoảng cách`h`, do đó, chuyến đi sẽ truy cập toàn bộ đường dẫn đã chọn. Ngược lại, mỗi bước đi hợp lệ từ bài toán ban đầu sẽ tạo ra chính xác một chuỗi như vậy cho vị trí và hướng bắt đầu của nó. Vì vậy chúng ta không cần phải liệt kê điểm cuối một cách riêng biệt. 

có`n`lựa chọn cho vị trí bắt đầu và hai hướng, vì vậy sau khi đếm số bước đi trừu tượng, chúng tôi nhân với`2n`. Đây là mức giảm trung tâm được sử dụng bởi giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nk2^m)`|`O(m)`cho đệ quy | Quá chậm | 
| Tối ưu |`O(m min(m,k))`|`O(min(m,k))`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cố định vị trí bắt đầu trừu tượng tại vị trí`0`và một hướng xung quanh chu kỳ. Chúng ta chỉ cần đếm số bước đi trong biểu diễn một chiều này vì mọi vị trí và hướng xuất phát thực sự đều hoạt động giống hệt nhau. 
2. Xác định`dp[j]`là số bước đi hợp lệ của độ dài hiện tại hiện đang ở vị trí`j`. Ban đầu bước đi có độ dài bằng 0 và đang ở điểm bắt đầu, vì vậy`dp[0] = 1`và mọi trạng thái khác đều bằng không. 
3. Với mỗi nước đi tiếp theo, hãy tính một mảng mới`next`. Một cuộc đi bộ kết thúc tại vị trí`j > 0`có thể đã đến từ`j-1`hoặc`j+1`, Vì thế`next[j] = dp[j-1] + dp[j+1]`. Cuộc đi bộ kết thúc lúc`0`chỉ có thể đến từ`1`, Vì thế`next[0] = dp[1]`. 
4. Chỉ các vị trí tối đa`min(i, k)`cần cân nhắc sau`i`di chuyển. Một cuộc đi bộ không thể di chuyển nhiều hơn`i`cạnh trong`i`di chuyển và bản thân đường dẫn đã chọn có độ dài tối đa`k`. Điều này giới hạn độ rộng DP tối đa`min(m, k)`. 
5. Sau khi tính toán các trạng thái về độ dài`i`, thêm vào`dp[0]`để trả lời. Mỗi bước đi có độ dài khác 0 quay trở lại`0`là một cuộc đi bộ hoàn toàn hợp lệ. Chúng tôi làm điều này cho mọi`i`từ`1`bởi vì`m`, bởi vì độ dài được giới hạn ở trên bởi`m`, không bắt buộc phải bằng`m`. 
6. Nhân số tích lũy với`2n`. có`n`vị trí xuất phát có thể có và hai hướng có thể có từ mỗi vị trí xuất phát. DP một chiều đã ngầm xác định điểm cuối là khoảng cách tối đa đạt được khi đi bộ. 
7. Lấy mọi phép cộng và modulo nhân cuối cùng`10^9 + 7`. Số lần đi bộ tăng theo cấp số nhân, do đó cần phải tính toán mô-đun trong suốt quá trình tính toán. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý chính xác`i`di chuyển,`dp[j]`đếm chính xác chiều dài-`i`di chuyển các chuỗi nằm trong khoảng cho phép và kết thúc ở khoảng cách`j`ngay từ đầu. Quá trình chuyển đổi xem xét chính xác hai vị trí có thể có trước đó, đồng thời bỏ qua các vị trí bên ngoài`[0,k]`, do đó không có bước đi không hợp lệ nào vào DP và không có bước đi hợp lệ nào bị mất. Một cuộc đi bộ hoàn chỉnh được đặc trưng bởi việc kết thúc tại`0`, và bởi vì mọi bước quay về khác rỗng đều đạt đến một giá trị cực đại dương nào đó`h`, mức tối đa đó mang lại điểm cuối duy nhất của đường dẫn đã chọn. Do đó, mỗi bước đi được tính DP tương ứng với chính xác một bước đi hợp lệ cho vị trí và hướng bắt đầu cố định. nhân với`2n`sau đó tính đến mọi vị trí và hướng xuất phát thực tế chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, k, m = map(int, input().split())

    width = min(k, m)

    # prev[j] = number of valid walks of the previous length
    # currently at distance j from the starting point.
    prev = [0] * (width + 2)
    prev[0] = 1

    ans = 0

    for length in range(1, m + 1):
        limit = min(length, k)
        cur = [0] * (width + 2)

        # Position 0 can only be reached from position 1.
        cur[0] = prev[1]

        for j in range(1, limit + 1):
            cur[j] = (prev[j - 1] + prev[j + 1]) % MOD

        ans += cur[0]
        ans %= MOD

        prev = cur

    ans = ans * (2 * n) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Mảng đầu tiên biểu thị trạng thái trước bất kỳ động thái nào. Cài đặt`prev[0] = 1`có nghĩa là có đúng một lối đi trống ở vị trí bắt đầu. Không có vị trí nào khác có thể truy cập được trước bước đi đầu tiên. 

Đối với mỗi độ dài, mã sẽ xây dựng một`cur`mảng. Sự chuyển đổi tích cực`j`đến trực tiếp từ hai vị trí lân cận. biểu hiện`prev[j + 1]`là an toàn vì mảng có thêm hai ô. Các ô đó vẫn bằng 0, giúp xử lý ranh giới trên một cách thuận tiện mà không cần trường hợp đặc biệt. 

Ranh giới dưới được xử lý riêng biệt thông qua`cur[0] = prev[1]`. Không có sự chuyển tiếp từ`-1`, vì di chuyển xuống dưới vị trí 0 sẽ rời khỏi đường đã chọn. 

Vòng lặp chỉ đạt`min(length, k)`. Thế là đủ vì sau`length`di chuyển khung tập đi không thể xa hơn`length`và đường dẫn đã chọn không thể vượt ra ngoài`k`. 

Chỉ một`cur[0]`được thêm vào câu trả lời. Điều này được cố tình thực hiện sau mỗi đoạn dài thay vì chỉ sau`m`, bởi vì mọi độ dài chẵn lên đến`m`có thể xác định một bước đi khác. Độ dài lẻ tự động đóng góp bằng 0. 

Mảng cuộn là đủ vì sự chuyển đổi theo chiều dài`i`chỉ phụ thuộc vào độ dài`i-1`. Điều này làm giảm bộ nhớ từ không gian hai chiều`O(mk)`bàn để`O(min(m,k))`. Số nguyên Python cũng có chi phí đối tượng, do đó mức giảm này rất hữu ích trong giới hạn bộ nhớ 256 MB. 

Phép nhân với`2 * n`chỉ xảy ra sau khi số lượng DP đã được tích lũy. Giá trị giảm modulo`10^9 + 7`trước và sau phép nhân, vì vậy Python không bao giờ cần đưa ra kết quả chính xác lớn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`n = 4`,`k = 3`, Và`m = 3`, DP cho một vị trí xuất phát và một hướng là: 

| Chiều dài | Vị trí có thể tiếp cận |`dp[length][0]`| 
| --- | --- | --- | 
| 0 |`{0: 1}`| 1 | 
| 1 |`{1: 1}`| 0 | 
| 2 |`{0: 1, 2: 1}`| 1 | 
| 3 |`{1: 2, 3: 1}`| 0 | 

Chuyến đi về không trống duy nhất có chiều dài`2`, cụ thể là`0 -> 1 -> 0`. Do đó, số hướng bắt đầu cố định là`1`. 

có`4`vị trí xuất phát và`2`hướng dẫn từ mỗi người, đưa ra`1 * 4 * 2 = 8`.```
8
```Ví dụ này cũng giải thích tại sao không thể thu được câu trả lời bằng cách chỉ đếm các cặp điểm cuối không có thứ tự. Hướng của con đường đã chọn là một phần của sự lựa chọn và hai hướng tạo ra những bước đi khác nhau. 

### Mẫu 2 

cho`n = 10`,`k = 5`, Và`m = 6`, số lần trả lại có liên quan là: 

| Chiều dài |`dp[length][0]`| Số tích lũy | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 1 | 1 | 
| 3 | 0 | 1 | 
| 4 | 2 | 3 | 
| 5 | 0 | 3 | 
| 6 | 5 | 8 | 

Giới hạn đường dẫn`k = 5`không ảnh hưởng nhiều nhất đến bất kỳ quãng đường đi bộ nào`6`, bởi vì quãng đường quay lại có độ dài`6`có thể đạt được khoảng cách tối đa`3`. Số hướng bắt đầu cố định là`8`. 

có`10 * 2 = 20`lựa chọn hướng bắt đầu, vì vậy câu trả lời là`8 * 20 = 160`.```
160
```Ví dụ này cho thấy lý do tại sao DP nên tính trực tiếp số lần quay lại thay vì chọn điểm cuối trước tiên. Khoảng cách điểm cuối có thể được xác định tự động theo vị trí tối đa đạt được sau mỗi lần đi bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(m min(m,k))`| Ở độ dài`i`, chỉ các vị trí`0`bởi vì`min(i,k)`được xử lý. | 
| Không gian |`O(min(m,k))`| Chỉ các lớp DP trước đó và hiện tại được lưu trữ. | 

Từ`m <= 2000`, DP thực hiện tối đa khoảng hai triệu chuyển đổi trạng thái. Điều này đủ nhỏ cho thời hạn, mặc dù`n`Có lẽ`100000`, bởi vì`n`chỉ xuất hiện trong phép nhân cuối cùng. Việc sử dụng bộ nhớ cũng nhỏ vì việc triển khai chỉ giữ lại hai mảng một chiều. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10**9 + 7

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    n, k, m = data

    width = min(k, m)
    prev = [0] * (width + 2)
    prev[0] = 1

    ans = 0

    for length in range(1, m + 1):
        limit = min(length, k)
        cur = [0] * (width + 2)

        cur[0] = prev[1]

        for j in range(1, limit + 1):
            cur[j] = (prev[j - 1] + prev[j + 1]) % MOD

        ans = (ans + cur[0]) % MOD
        prev = cur

    return str(ans * (2 * n) % MOD)

# Provided samples
assert solve_data("4 3 3") == "8", "sample 1"
assert solve_data("10 5 6") == "160", "sample 2"

# Minimum possible n and k, but too little length for a return walk.
assert solve_data("2 1 1") == "0", "minimum size and odd length"

# One edge is the entire allowed path. The only valid walk has length 2.
assert solve_data("2 1 2") == "8", "single-edge path"

# Boundary case where k = 2 and m = 4.
# One fixed direction has 1 walk of length 2 and 2 walks of length 4.
assert solve_data("3 2 4") == "18", "path boundary"

# k = 1 prevents every walk from reaching distance 2.
# Only length 2 contributes, while the odd length 3 contributes nothing.
assert solve_data("5 1 3") == "20", "k = 1 and odd m"

# Maximum n. With m = 1 no nonempty closed walk can exist.
assert solve_data("100000 99999 1") == "0", "maximum n boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1`|`0`| Biểu đồ hợp lệ tối thiểu và độ dài tối đa lẻ | 
|`2 1 2`|`8`| Chuyến đi khứ hồi không trống nhỏ nhất có thể và`2n`yếu tố | 
|`3 2 4`|`18`| Khoảng cách điểm cuối và ranh giới độ dài đường dẫn | 
|`5 1 3`|`20`| Con đường chặt chẽ ràng buộc`k = 1`và một độ dài lẻ chưa được sử dụng | 
|`100000 99999 1`|`0`| Tối đa`n`và thực tế là không có chuyến đi bộ chiều dài lẻ nào tồn tại | 

Ý tưởng được yêu cầu về phép thử "tất cả các giá trị bằng nhau" không thể xuất hiện theo nghĩa đen dưới các ràng buộc của bài toán, bởi vì`k < n`, Vì thế`n = k = m`không hợp lệ. Ranh giới có ý nghĩa gần nhất là làm cho các giới hạn hoạt động bằng nhau, chẳng hạn như`k = m`, được bao phủ bởi`3 2 4`phong cách kiểm tra ranh giới và theo mẫu. 

## Vỏ cạnh 

cho`2 1 1`, thuật toán bắt đầu bằng`prev[0] = 1`. Sau một lần di chuyển, trạng thái khác 0 duy nhất là vị trí`1`, Vì thế`cur[0] = 0`. Câu trả lời vẫn là 0, điều này đúng vì việc quay lại điểm xuất phát cần có số lần di chuyển chẵn. 

Vì`2 1 2`, sau lần di chuyển đầu tiên người đi bộ đã ở vị trí`1`. Ở lần di chuyển thứ hai nó có thể trở về vị trí`0`, Vì thế`cur[0] = 1`. Số lượng hướng bắt đầu cố định là một. nhân với`2n = 4`cho`8`. Điều này mắc phải sai lầm phổ biến là quên rằng các lựa chọn theo chiều kim đồng hồ và ngược chiều kim đồng hồ là khác nhau. 

Vì`3 2 4`, số lần trả về có độ dài-2 là`1`. Ở độ dài`4`, có hai chuỗi hợp lệ, tương ứng với các bước đi trừu tượng`0 -> 1 -> 0 -> 1 -> 0`Và`0 -> 1 -> 2 -> 1 -> 0`. Do đó, tổng số tích lũy cho một hướng xuất phát là`3`, và nhân với`6`cho`18`. Lần đi bộ thứ hai đạt đến khoảng cách ranh giới`k = 2`, vì vậy trường hợp này kiểm tra xem ranh giới trên có được phép hay không được coi là bị cấm. 

Vì`5 1 3`, người đi bộ chỉ có thể sử dụng các vị trí`0`Và`1`. Chuyến đi khứ hồi không trống duy nhất có độ dài tối đa là ba là`0 -> 1 -> 0`. DP đóng góp một lần đi bộ và`10`lựa chọn hướng bắt đầu tạo ra`20`. Điều này phát hiện lỗi từng cái một trong đó vị trí`k`vô tình bị loại trừ. 

Đối với một rất lớn`n`, chẳng hạn như`100000 99999 1`, DP không trở nên lớn hơn vì`n`. Nó chỉ xử lý các độ dài di chuyển có thể có, không tìm thấy bước đi quay lại nào có độ dài bằng 1, sau đó nhân 0 với`200000`. Kết quả là`0`, chứng minh tại sao kích thước chu kỳ không xuất hiện ở trạng thái DP. 

Trường hợp cạnh tinh tế nhất là khi`k`lớn hơn một nửa kích thước chu kỳ. Đường đi đơn giản được chọn sau đó có thể dài hơn đường đi ngắn hơn giữa các điểm cuối của nó, nhưng nó vẫn là đường đi đơn giản hợp lệ vì`k < n`. DP không cần phải so sánh hai tuyến đường. Trước tiên, nó xác định một hướng, tính số bước đi theo hướng đó và hệ số hai tính cho hai hướng có thể có từ mọi vị trí bắt đầu.

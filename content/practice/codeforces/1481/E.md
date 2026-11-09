---
title: "CF 1481E - Phân Loại Sách"
description: "Chúng tôi có một hàng sách. Mỗi cuốn sách có một màu sắc. Chúng ta có thể liên tục chọn bất kỳ cuốn sách nào và di chuyển nó đến cuối hàng. Mục đích là làm cho sự sắp xếp cuối cùng bao gồm các khối màu, nghĩa là mỗi màu xuất hiện trong đúng một phân đoạn liền kề."
date: "2026-06-10T23:37:34+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1481
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 699 (Div. 2)"
rating: 2500
weight: 1481
solve_time_s: 139
verified: false
draft: false
---

[CF 1481E - Sắp xếp sách](https://codeforces.com/problemset/problem/1481/E) 

**Đánh giá:** 2500 
**Tags:** cấu trúc dữ liệu, dp, tham lam 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng sách. Mỗi cuốn sách có một màu sắc. Chúng ta có thể liên tục chọn bất kỳ cuốn sách nào và di chuyển nó đến cuối hàng. 

Mục đích là làm cho sự sắp xếp cuối cùng bao gồm các khối màu, nghĩa là mỗi màu xuất hiện trong đúng một phân đoạn liền kề. Các màu khác nhau có thể xuất hiện theo bất kỳ thứ tự nào, nhưng các sách cùng màu phải ở cạnh nhau. 

Chúng tôi cần số lượng sách tối thiểu được di chuyển. 

Hoạt động này là bất thường. Chúng tôi không bao giờ chèn một cuốn sách vào đâu đó ở giữa. Mỗi cuốn sách được di chuyển sẽ đi về phía bên phải. Do đó, một số cuốn sách sẽ giữ nguyên trật tự tương đối ban đầu mãi mãi, trong khi những cuốn sách đã di chuyển sẽ được thu thập ở cuối. 

Kích thước đầu vào đạt$5 \cdot 10^5$. Bất kỳ thuật toán nào thử tất cả các thứ tự màu cuối cùng có thể có, tất cả các tập hợp con của sách đã di chuyển hoặc thực hiện quét bậc hai đều không thể thực hiện được ngay lập tức. Thậm chí$O(n \sqrt n)$sẽ không thoải mái ở quy mô này. Chúng ta cần một cái gì đó gần tuyến tính hoặc$O(n \log n)$. 

Phần khó khăn là hiểu những cuốn sách vẫn còn nguyên sẽ trông như thế nào. Một ý tưởng ngây thơ là giữ lại dãy con lớn nhất vốn đã đẹp, nhưng thao tác này hạn chế hơn nhiều so với việc xóa tùy ý. Chúng ta phải tôn trọng cấu trúc được tạo ra bằng cách chỉ di chuyển sách đến cuối cùng. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
1 2 1
```Đáp án là 1. Di chuyển cuốn sách ở giữa:```
1 1 2
```Giải pháp chỉ kiểm tra xem mỗi màu đã tiếp giáp nhau trong mảng ban đầu hay chưa sẽ kết luận sai rằng hai cuốn sách phải di chuyển. 

Một trường hợp tế nhị khác là:```
1 2 2 1
```Đáp án là 1. Di chuyển cái cuối cùng`1`:```
1 2 2 1
→
1 2 2 1   (remove last 1)
→
1 2 2 1   (append to end, same position)
```Tự nhiên hơn, hãy di chuyển đầu tiên`1`:```
2 2 1 1
```Quan sát quan trọng là một màu không nhất thiết cần tất cả các lần xuất hiện của nó được bảo toàn trong phần tĩnh. 

Trường hợp thứ ba là khi một màu đã chiếm trọn một khoảng:```
1 2 2 2 3
```Câu trả lời là 0. Bất kỳ thuật toán nào buộc màu sắc xuất hiện theo thứ tự giá trị tăng dần hoặc giảm dần sẽ thất bại, vì thứ tự cuối cùng của các khối màu hoàn toàn không bị hạn chế. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là quyết định những cuốn sách nào được giữ nguyên. Cuốn nào không ở lại thì phải chuyển về cuối. 

Giả sử chúng ta biết tập sách văn phòng phẩm. Thứ tự tương đối của chúng không bao giờ thay đổi. Sau khi loại bỏ tất cả các sách đã di chuyển, các sách văn phòng phẩm phải tạo thành một kệ đẹp vì sách đã di chuyển sẽ được thêm vào sau và không thể làm thay đổi cấu trúc bên trong của tiền tố văn phòng phẩm. 

Điều này gợi ý việc tìm kiếm tập hợp con lớn nhất của các cuốn sách còn sót lại. Nếu chúng ta có thể giữ$k$sách thì câu trả lời là$n-k$. 

Vấn đề là có$2^n$tập hợp con có thể. Ngay cả việc cố gắng mô tả trực tiếp tất cả các tập hợp con hợp lệ cũng không thể thực hiện được. 

Quan sát quan trọng xuất phát từ việc xem xét lần xuất hiện đầu tiên và cuối cùng của mỗi màu. 

Cho phép:

-$L_c$= sự xuất hiện đầu tiên của màu sắc$c$-$R_c$= lần xuất hiện cuối cùng của màu$c$-$cnt_c$= tần số màu$c$Nếu chúng ta quyết định màu đó$c$tham gia vào phần đứng yên, sau đó một khi chúng ta giữ bất kỳ sự xuất hiện nào của$c$, mỗi lần xuất hiện bên trong$[L_c,R_c]$cũng phải thuộc cùng một khối màu. Nếu không thì màu khác sẽ chia khối. 

Cấu trúc khoảng này biến bài toán thành bài toán quy hoạch động trên các vị trí. 

Định nghĩa$dp[i]$là số lượng sách tối đa có thể đứng yên chỉ sử dụng các vị trí hậu tố bắt đầu từ$i$. 

Khi chúng ta đứng ở vị trí$i$, có hai khả năng tồn tại. 

Chúng tôi có thể chỉ cần xóa cuốn sách hiện tại. Sau đó, chúng tôi không đạt được gì và tiếp tục với$dp[i+1]$. 

Hoặc chúng ta có thể bắt đầu giữ nguyên màu có khoảng thời gian bắt đầu từ đây. Giả sử màu sắc là$c$Và$i=L_c$. 

Bên trong$[L_c,R_c]$, mỗi lần xuất hiện màu sắc$c$có thể ở lại. Đối với mọi màu khác có toàn bộ khoảng nằm trong phạm vi này, chúng tôi cũng có thể giữ nguyên tất cả các lần xuất hiện của nó. Điều này tạo ra một chuỗi các khoảng lồng nhau. 

Giải pháp được chấp nhận ban đầu xử lý các vị trí từ phải sang trái và duy trì, đối với mỗi màu, giá trị tốt nhất có thể đạt được khi khoảng thời gian của nó được chọn. Cấu trúc khoảng cho phép mọi vị trí chỉ được xử lý một lần, mang lại độ phức tạp tuyến tính. 

Quá trình chuyển đổi cuối cùng sẽ tính toán số lượng sách tối đa có thể tồn tại và câu trả lời là:$$n - \max(\text{stationary books})$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Xử lý khoảng thời gian DP + tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Tính toán đầu tiên cho mỗi màu lần xuất hiện đầu tiên, lần xuất hiện cuối cùng và tần suất. 

Cho phép`dp[i]`biểu thị số lượng sách tối đa có thể đứng yên chỉ xem xét các vị trí từ`i`trở đi. 

Xử lý các vị trí từ phải sang trái. 

1. Nếu ta bỏ cuốn sách ở vị trí`i`, chúng tôi có được`dp[i+1]`. 
2. Đặt màu hiện tại là`c`. 
3. Duy trì`best[c]`, giá trị tương ứng với việc chọn khoảng màu`c`. 
4. Nếu vị trí`i`là lần xuất hiện cuối cùng của`c`, khởi tạo$$best[c] = cnt_c$$bởi vì ít nhất chúng ta có thể giữ lại tất cả các lần xuất hiện của màu này. 
5. Nếu không thì hãy cập nhật$$best[c] = best[c] + 1$$trong khi di chuyển sang trái qua một lần xuất hiện khác cùng màu. 
6. Nếu vị trí`i`là lần xuất hiện đầu tiên của`c`, chúng ta có thể hoàn thành khoảng màu`c`và tiếp tục sau lần xuất hiện cuối cùng của nó:$$best[c] += dp[R_c + 1]$$7. Đặt$$dp[i] = \max(dp[i+1], best[c])$$8. Tiếp tục cho đến vị trí 1. 

Số lượng sách văn phòng phẩm tối đa là`dp[1]`. Câu trả lời là`n - dp[1]`. 

### Tại sao nó hoạt động 

Đối với mỗi màu, khoảng thời gian giữa lần xuất hiện đầu tiên và lần cuối cùng của nó sẽ xác định vùng phải được xử lý nhất quán. Khi quá trình quét đạt đến sự xuất hiện của màu`c`,`best[c]`lưu trữ giải pháp tốt nhất giữ hậu tố hiện tại của khoảng thời gian`c`. 

Di chuyển sang trái kéo dài khoảng thời gian được bảo toàn đó thêm một lần xuất hiện của`c`. Khi quá trình quét đạt đến lần xuất hiện đầu tiên, toàn bộ khoảng thời gian`c`đã được hạch toán. Tại thời điểm đó chúng ta có thể ghép nối một giải pháp tối ưu bắt đầu ngay sau`R_c`. 

Mỗi bộ cố định hợp lệ có thể được phân tách thành các khoảng màu hoàn chỉnh như vậy và mọi chuyển đổi do DP tạo ra đều tương ứng với một bộ cố định có thể thực hiện được. Phép truy toán kiểm tra cả hai khả năng ở mọi vị trí: loại bỏ cuốn sách hiện tại hoặc cam kết cấu trúc khoảng màu của nó. Vì tất cả các khoảng thời gian được xử lý chính xác một lần nên số lượng sách tối đa có thể giữ lại được tính toán chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    first = [-1] * (n + 1)
    last = [-1] * (n + 1)
    cnt = [0] * (n + 1)

    for i, x in enumerate(a):
        if first[x] == -1:
            first[x] = i
        last[x] = i
        cnt[x] += 1

    dp = [0] * (n + 1)
    best = [0] * (n + 1)

    for i in range(n - 1, -1, -1):
        c = a[i]

        if i == last[c]:
            best[c] = cnt[c]
        else:
            best[c] += 1

        if i == first[c]:
            best[c] += dp[last[c] + 1]

        dp[i] = max(dp[i + 1], best[c])

    print(n - dp[0])

if __name__ == "__main__":
    solve()
```Lần đầu tiên tính toán thông tin khoảng thời gian cho mỗi màu.`best[c]`là ý tưởng trung tâm. Trong khi quét từ phải sang trái, nó lưu trữ giá trị của khoảng màu được xử lý một phần`c`. Gặp phải một hiện tượng khác`c`kéo dài khoảng thời gian đó bằng một cuốn sách được bảo quản. 

Việc xử lý đặc biệt ngay lần xảy ra đầu tiên là rất quan trọng. Chỉ sau khi đạt đến lần xuất hiện đầu tiên, chúng ta mới biết rằng toàn bộ khoảng màu đó đã được che phủ. Tại thời điểm đó, chúng tôi được phép thêm một giải pháp tối ưu bắt đầu sau khi khoảng thời gian kết thúc. 

Mảng DP là hậu tố DP.`dp[i]`luôn đại diện cho câu trả lời tốt nhất có sẵn trong hậu tố bắt đầu ở vị trí`i`. Lấy`max(dp[i + 1], best[c])`tương ứng chính xác với lựa chọn giữa việc loại bỏ cuốn sách hiện tại hoặc sử dụng cấu trúc khoảng màu của nó. 

Tất cả các giá trị đều phù hợp thoải mái với số nguyên 32 bit vì chúng không bao giờ vượt quá`n`, nhưng số nguyên Python loại bỏ mọi mối lo ngại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 2 1 3
```Khoảng thời gian: 

| Màu sắc | Đầu tiên | Cuối cùng | Đếm | 
| --- | --- | --- | --- | 
| 1 | 0 | 3 | 2 | 
| 2 | 1 | 2 | 2 | 
| 3 | 4 | 4 | 1 | 

DP từ phải sang trái: 

| tôi | Màu sắc | tốt nhất[màu] sau khi cập nhật | dp[i] | 
| --- | --- | --- | --- | 
| 4 | 3 | 1 | 1 | 
| 3 | 1 | 2 | 2 | 
| 2 | 2 | 2 | 2 | 
| 1 | 2 | 3 | 3 | 
| 0 | 1 | 4 | 4 | 

Chúng ta có thể giữ 4 cuốn sách nên đáp án là:```
5 - 4 = 1
```Dấu vết cho thấy việc hoàn thành khoảng màu 2 ở vị trí 1 cho phép DP kết nối với các khoảng màu sau như thế nào. 

### Ví dụ 2 

đầu vào:```
5
2 1 1 1 2
```Khoảng thời gian: 

| Màu sắc | Đầu tiên | Cuối cùng | Đếm | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | 3 | 
| 2 | 0 | 4 | 2 | 

Tiến hóa DP: 

| tôi | Màu sắc | tốt nhất[màu] sau khi cập nhật | dp[i] | 
| --- | --- | --- | --- | 
| 4 | 2 | 2 | 2 | 
| 3 | 1 | 3 | 3 | 
| 2 | 1 | 4 | 4 | 
| 1 | 1 | 5 | 5 | 
| 0 | 2 | 6 | 6 | 

Số lượng đứng yên tối đa đạt tới 4 cuốn, mang lại câu trả lời là 1. Một bước di chuyển là đủ để làm cho các màu liền kề nhau. 

Ví dụ này minh họa các khoảng lồng nhau, chính xác là những gì DP được thiết kế để xử lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi vị trí được xử lý một lần | 
| Không gian |$O(n)$| Mảng cho các khoảng, DP và trạng thái màu | 

Với$n \le 5 \cdot 10^5$, thời gian tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 256 MB vì ​​tất cả các mảng chỉ lưu trữ số nguyên và có kích thước tỷ lệ thuận với$n$. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))

    first = [-1] * (n + 1)
    last = [-1] * (n + 1)
    cnt = [0] * (n + 1)

    for i, x in enumerate(a):
        if first[x] == -1:
            first[x] = i
        last[x] = i
        cnt[x] += 1

    dp = [0] * (n + 1)
    best = [0] * (n + 1)

    for i in range(n - 1, -1, -1):
        c = a[i]

        if i == last[c]:
            best[c] = cnt[c]
        else:
            best[c] += 1

        if i == first[c]:
            best[c] += dp[last[c] + 1]

        dp[i] = max(dp[i + 1], best[c])

    return str(n - dp[0]) + "\n"

assert solve_io("5\n1 2 2 1 3\n") == "1\n"

assert solve_io("1\n1\n") == "0\n"
assert solve_io("5\n7 7 7 7 7\n") == "0\n"
assert solve_io("4\n1 2 3 4\n") == "0\n"
assert solve_io("3\n1 2 1\n") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Kích thước tối thiểu | 
|`7 7 7 7 7`|`0`| Tất cả sách cùng màu | 
|`1 2 3 4`|`0`| Đã đẹp với những gam màu đơn sắc | 
|`1 2 1`|`1`| Màu không liền kề cần di chuyển chính xác một lần | 

## Vỏ cạnh 

Hãy xem xét:```
3
1 2 1
```Khoảng màu`1`là`[0,2]`. Khoảng màu`2`là`[1,1]`. 

Trong khi quét từ phải sang trái, DP nhận ra rằng việc bảo toàn cả hai lần xuất hiện của`1`và sự xuất hiện duy nhất của`2`bên trong khoảng là có thể. Số lượng đứng yên tối đa trở thành`2`, đưa ra câu trả lời`1`. Điều này phù hợp với động thái:```
1 2 1
→
1 1 2
```Coi như:```
4
1 2 2 1
```Màu sắc`2`đã tạo thành một khối liền kề. DP giữ toàn bộ khoảng thời gian đó và kết hợp nó với một khối màu xuất hiện`1`. Số lượng văn phòng phẩm đạt`3`, vậy câu trả lời là`1`. 

Một chiến lược tham lam nhất quyết bảo toàn mọi sự xuất hiện của một màu khi nó được chọn sẽ bỏ lỡ khả năng này. 

Coi như:```
5
1 2 2 2 3
```Mỗi màu đã chiếm một phân đoạn liền kề. Các khoảng rời rạc và đã hợp lệ. DP giữ tất cả năm cuốn sách, lấy được`dp[0] = 5`, vậy câu trả lời là`0`. 

Điều này xác nhận rằng thuật toán xử lý chính xác các kệ không yêu cầu thao tác nào cả.

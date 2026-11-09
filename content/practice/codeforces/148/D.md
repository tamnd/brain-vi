---
title: "CF 148D - Túi đựng chuột"
description: "Chúng tôi có một cái túi đựng chuột trắng và chuột đen. Công chúa di chuyển trước, sau đó là rồng, rồi lại là công chúa, v.v. Người nào rút được chuột trắng sẽ thắng ngay. Nếu ai đó rút được con chuột đen, trò chơi tiếp tục. Có một quy tắc bổ sung trong lượt của rồng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "games", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 148
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 105 (Div. 2)"
rating: 1800
weight: 148
solve_time_s: 115
verified: true
draft: false
---

[CF 148D - Túi đựng chuột](https://codeforces.com/problemset/problem/148/D) 

**Đánh giá:** 1800 
**Tags:** dp, trò chơi, toán học, xác suất 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cái túi đựng chuột trắng và chuột đen. Công chúa di chuyển trước, sau đó là rồng, rồi lại là công chúa, v.v. Người nào rút được chuột trắng sẽ thắng ngay. Nếu ai đó rút được con chuột đen, trò chơi tiếp tục. 

Có một quy tắc bổ sung trong lượt của rồng. Sau khi rồng rút ra một con chuột, một con chuột ngẫu nhiên khác nhảy ra khỏi túi vì những con chuột còn lại hoảng sợ. Con chuột nhảy lên không được tính là do ai rút ra nên không thể trực tiếp quyết định người chiến thắng. 

Đầu vào đưa ra số lượng chuột bạch ban đầu`w`và chuột đen`b`. Chúng ta phải tính xác suất để công chúa chiến thắng nếu mọi lựa chọn ngẫu nhiên đều giống nhau. 

Các ràng buộc cho phép tối đa 1000 con chuột trắng và 1000 con chuột đen. Mô phỏng bạo lực trên tất cả các chuỗi trò chơi có thể xảy ra sẽ bùng nổ theo cấp số nhân vì mỗi bước di chuyển sẽ phân nhánh thành một số trạng thái trong tương lai. Ngay cả một tìm kiếm đệ quy không ghi nhớ cũng sẽ truy cập lại cùng một kết quả`(w, b)`tuyên bố nhiều lần. Chỉ với khoảng một triệu trạng thái riêng biệt, lập trình động là khả thi, nhưng bất cứ điều gì theo cấp số nhân thì không. 

Phần nguy hiểm nhất của vấn đề này là mô hình hóa chính xác thứ tự các sự kiện trong lượt của rồng. Việc triển khai bất cẩn thường hợp nhất nhiều bước xác suất thành một và vô tình đếm những bước chuyển đổi không thể thực hiện được. 

Hãy xem xét đầu vào:```
1 1
```Công chúa vẽ trước. Cô ấy thắng ngay lập tức với xác suất`1/2`. Nếu cô ấy rút con chuột đen thay vào đó, con rồng sẽ lấy được con chuột trắng duy nhất còn lại và thắng. Câu trả lời đúng là:```
0.5
```Sự lặp lại sai mà bỏ qua thứ tự lần lượt có thể tạo ra sai`1`. 

Một trường hợp tế nhị khác là:```
0 5
```Không có con chuột bạch nào cả nên công chúa không bao giờ có thể thắng được. Câu trả lời phải là:```
0
```Một số giải pháp đệ quy vô tình chia cho 0 ở đây vì chúng cố tính xác suất liên quan đến chuột trắng không tồn tại. 

Trường hợp phức tạp thứ ba là:```
2 0
```Công chúa ngay lập tức rút được một con chuột trắng với xác suất`1`, vậy đáp án là:```
1
```Không bao giờ nên đánh giá các chuyển đổi liên quan đến rồng ở trạng thái này vì trò chơi đã kết thúc ở nước đi đầu tiên của công chúa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng đệ quy mọi chuỗi rút và nhảy có thể có. Từ một tiểu bang`(w, b)`, chúng ta phân nhánh theo việc công chúa vẽ màu trắng hay đen, sau đó lại phân nhánh theo hình vẽ của con rồng, rồi lại phân nhánh theo con chuột nhảy đi. 

Quá trình đệ quy này đúng về mặt toán học vì mọi kết quả của trò chơi đều tương ứng với chính xác một đường dẫn trong cây đệ quy. Vấn đề là số lượng đường dẫn. Trong trường hợp xấu nhất, mỗi lượt sẽ tạo ra nhiều nhánh và thời lượng trò chơi có thể lên tới 2000 sự kiện. Số lượng trạng thái được truy cập mà không cần ghi nhớ trở nên khổng lồ, vượt xa con số vừa vặn trong 2 giây. 

Quan sát quan trọng là tương lai chỉ phụ thuộc vào việc còn lại bao nhiêu con chuột trắng và đen. Lịch sử chính xác không thành vấn đề. Nếu hai trình tự khác nhau dẫn đến cùng một`(w, b)`Ở lượt của công chúa, xác suất chiến thắng cuối cùng kể từ thời điểm đó trở đi là như nhau. 

Điều đó làm cho đây trở thành một vấn đề lập trình động cổ điển trên các trạng thái. 

Định nghĩa`dp[w][b]`là xác suất để công chúa thắng khi đến lượt mình và chiếc túi hiện chứa`w`chuột trắng và`b`chuột đen. 

Từ trạng thái này, chỉ có một số khả năng. 

Nếu công chúa rút được ngay con chuột trắng thì cô ấy thắng. 

Nếu cô ấy vẽ một con chuột đen, con rồng sẽ di chuyển tiếp theo. Để công chúa sống sót đến lượt rồng, rồng cũng phải vẽ một con chuột đen. Sau đó, thêm một con chuột nữa nhảy đi. Tùy theo con chuột nhảy lên có màu trắng hay đen mà chúng ta chuyển sang trạng thái khác nhỏ hơn. 

Mỗi lần chuyển đổi đều làm giảm tổng số chuột, vì vậy chúng ta có thể tính toán các trạng thái từ dưới lên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Cây đệ quy hàm mũ | Quá chậm | 
| Lập trình động tối ưu | O(w × b) | O(w × b) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định`dp[i][j]`là xác suất để công chúa thắng khi đến lượt mình với`i`chuột trắng và`j`chuột đen còn lại. 
2. Khởi tạo các trường hợp cơ sở. 

Nếu không có chuột bạch thì công chúa không bao giờ thắng được:```
dp[0][j] = 0
```Nếu không có chuột đen thì công chúa chắc chắn sẽ vẽ ra màu trắng ngay:```
dp[i][0] = 1
```3. Lặp lại tất cả các trạng thái`(i, j)`với`i >= 1`Và`j >= 1`. 
4. Tính xác suất để công chúa thắng ngay trong lần rút thăm của mình. 

Cô ấy chọn đồng đều trong số tất cả các con chuột, vì vậy:```
win_now = i / (i + j)
```5. Tính xác suất để công chúa rút được con chuột đen đầu tiên.```
princess_black = j / (i + j)
```6. Sau khi công chúa bắt được một con chuột đen, con rồng sẽ tác động lên một chiếc túi chứa`i`trắng và`j - 1`chuột đen. 

Để công chúa còn cơ hội, rồng phải vẽ một con chuột đen. Nếu rồng rút được quân trắng thì rồng thắng ngay. 

Xác suất để rồng lấy được màu đen là:```
dragon_black = (j - 1) / (i + j - 1)
```7. Sau khi rồng chuyển sang màu đen, một con chuột khác nhảy ra. 

Lúc đó trong túi có`i`trắng và`j - 2`chuột đen. 

Có hai khả năng. 

Nếu một con chuột bạch nhảy ra:```
probability = i / (i + j - 2)
next state = dp[i - 1][j - 2]
```Nếu một con chuột đen nhảy ra:```
probability = (j - 2) / (i + j - 2)
next state = dp[i][j - 3]
```8. Thêm tất cả các chuyển đổi hợp lệ lại với nhau. 

Sự tái phát đầy đủ trở thành:```
dp[i][j] =
    i / (i + j)
    +
    (j / (i + j))
    × ((j - 1) / (i + j - 1))
    × (
        (i / (i + j - 2)) × dp[i - 1][j - 2]
        +
        ((j - 2) / (i + j - 2)) × dp[i][j - 3]
      )
```1. Kiểm tra cẩn thận giới hạn trước khi truy cập các trạng thái như`j - 2`hoặc`j - 3`. 

### Tại sao nó hoạt động 

Sự truy hồi phân chia triệt để tất cả các kết quả có thể xảy ra từ một trạng thái`(i, j)`. 

Hoặc công chúa thắng ngay khi vẽ màu trắng, hoặc cô ấy vẽ màu đen. Trong trường hợp thứ hai, con đường duy nhất còn sống sót là những con đường mà con rồng cũng vẽ màu đen. Sau đó chuột nhảy sẽ xác định trạng thái giảm tiếp theo. 

Mọi xác suất chuyển đổi đều được nhân với xác suất đạt được chuyển đổi đó và tất cả các khả năng rời rạc sẽ được tính tổng. Vì mỗi bước di chuyển đều làm giảm đáng kể số lượng chuột nên cuối cùng DP sẽ đạt đến trạng thái cơ bản nơi câu trả lời đã được biết trước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

w, b = map(int, input().split())

dp = [[0.0] * (b + 1) for _ in range(w + 1)]

for i in range(1, w + 1):
    dp[i][0] = 1.0

for i in range(1, w + 1):
    for j in range(1, b + 1):
        total = i + j

        # Princess draws white immediately
        ans = i / total

        # Princess draws black
        if j >= 1:
            princess_black = j / total

            # Dragon draws black
            if j >= 2:
                dragon_black = (j - 1) / (total - 1)

                survive_prob = princess_black * dragon_black

                remaining = total - 2

                # A white mouse jumps out
                if i >= 1:
                    ans += (
                        survive_prob
                        * (i / remaining)
                        * dp[i - 1][j - 2]
                    )

                # A black mouse jumps out
                if j >= 3:
                    ans += (
                        survive_prob
                        * ((j - 2) / remaining)
                        * dp[i][j - 3]
                    )

        dp[i][j] = ans

print("{:.9f}".format(dp[w][b]))
```Bảng DP lưu trữ xác suất cho mọi trạng thái có thể truy cập. Vì mỗi lần chuyển đổi đều làm giảm tổng số chuột nên các trạng thái nhỏ hơn đã được tính toán trước khi các trạng thái lớn hơn cần đến chúng. 

Bộ trường hợp cơ sở đầu tiên`dp[i][0] = 1`bởi vì nếu chỉ còn lại chuột trắng thì đến lượt công chúa sẽ thắng ngay lập tức. 

Sự tái phát phản ánh trực tiếp quá trình trò chơi. Phần tế nhị nhất là đến lượt rồng. Sau khi công chúa loại bỏ một con chuột đen, mẫu số sẽ thay đổi vì kích thước túi giảm đi. Sau khi con rồng loại bỏ một con chuột đen khác, mẫu số lại thay đổi trước khi cơn hoảng loạn xảy ra. 

Nhiều giải pháp sai vô tình sử dụng lại cùng một mẫu số cho tất cả các giai đoạn. Điều đó tạo ra xác suất không chính xác vì kích thước túi thay đổi sau mỗi sự kiện. 

Một chi tiết triển khai tinh vi khác là kiểm tra giới hạn trước khi truy cập`dp[i][j - 3]`hoặc`dp[i - 1][j - 2]`. Những trạng thái này chỉ tồn tại khi còn đủ chuột đen. 

Độ chính xác của dấu phẩy động là đủ vì bài toán chỉ yêu cầu lỗi bên dưới`1e-9`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3
```Dấu vết chuyển tiếp DP: 

| Bước | Sự kiện | Xác suất | Bang tiếp theo | 
| --- | --- | --- | --- | 
| 1 | Công chúa vẽ màu trắng | 1/4 | Thắng | 
| 2 | Công chúa vẽ màu đen | 3/4 | Tiếp tục | 
| 3 | Rồng vẽ màu đen | 2/3 | Tiếp tục | 
| 4 | Chuột bạch nhảy ra | 1/2 | dp[0][1] = 0 | 
| 5 | Chuột đen nhảy ra | 1/2 | dp[1][0] = 1 | 

Bây giờ hãy tính:```
dp[1][3]
= 1/4 + (3/4)*(2/3)*((1/2)*0 + (1/2)*1)
= 1/4 + 1/4
= 1/2
```Ví dụ này cho thấy tại sao việc nhảy chuột lại quan trọng. Nếu con chuột trắng nhảy đi, công chúa sẽ mất mọi cơ hội trong tương lai. 

### Ví dụ 2 

đầu vào:```
2 1
```Dấu vết chuyển tiếp DP: 

| Bước | Sự kiện | Xác suất | Bang tiếp theo | 
| --- | --- | --- | --- | 
| 1 | Công chúa vẽ màu trắng | 2/3 | Thắng | 
| 2 | Công chúa vẽ màu đen | 1/3 | Tiếp tục | 
| 3 | Rồng vẽ màu trắng | 1 | Rồng thắng | 

Tính toán:```
dp[2][1] = 2/3
```Trường hợp này chứng tỏ đôi khi quá trình đệ quy dừng lại ngay sau khi rồng rút vì rồng có thể trực tiếp thắng bằng cách rút trắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(w × b) | Mỗi trạng thái DP được tính toán một lần với công việc không đổi | 
| Không gian | O(w × b) | Bảng DP lưu trữ một giá trị cho mỗi trạng thái | 

Kích thước bảng tối đa là khoảng`1001 × 1001`, tức là khoảng một triệu tiểu bang. Mỗi trạng thái chỉ thực hiện một vài phép tính số học, do đó lời giải dễ dàng phù hợp với giới hạn thời gian và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    w, b = map(int, input().split())

    dp = [[0.0] * (b + 1) for _ in range(w + 1)]

    for i in range(1, w + 1):
        dp[i][0] = 1.0

    for i in range(1, w + 1):
        for j in range(1, b + 1):
            total = i + j

            ans = i / total

            if j >= 2:
                survive = (j / total) * ((j - 1) / (total - 1))
                remaining = total - 2

                ans += survive * (i / remaining) * dp[i - 1][j - 2]

                if j >= 3:
                    ans += survive * ((j - 2) / remaining) * dp[i][j - 3]

            dp[i][j] = ans

    print("{:.9f}".format(dp[w][b]))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("1 3\n") == "0.500000000", "sample 1"

# minimum size, no white mice
assert run("0 5\n") == "0.000000000", "no possible win"

# only white mice
assert run("5 0\n") == "1.000000000", "immediate win"

# small mixed case
assert abs(float(run("1 1\n")) - 0.5) < 1e-9, "balanced small case"

# another small case
assert abs(float(run("2 1\n")) - (2/3)) < 1e-9, "dragon immediately wins after black"

# larger boundary-style case
res = float(run("1000 1000\n"))
assert 0.0 <= res <= 1.0, "probability range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 5`|`0.000000000`| Không có con chuột bạch nào tồn tại | 
|`5 0`|`1.000000000`| Công chúa chiến thắng ngay lập tức | 
|`1 1`|`0.500000000`| Trạng thái hỗn hợp có ý nghĩa nhỏ nhất | 
|`2 1`|`0.666666667`| Rồng có thể thắng ngay | 
|`1000 1000`| Xác suất hợp lệ | Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
0 5
```DP ngay lập tức sử dụng trường hợp cơ bản:```
dp[0][5] = 0
```Không có sự chuyển đổi nào để đánh giá vì không có con chuột trắng nào tồn tại trong túi. Công chúa không thể thắng được. 

Bây giờ hãy xem xét:```
2 0
```Bộ thuật toán:```
dp[2][0] = 1
```Đến lượt rồng không bao giờ được tính đến vì công chúa luôn rút quân trắng ở nước đi đầu tiên. 

Cuối cùng, hãy xem xét:```
1 2
```Công chúa thắng ngay với xác suất`1/3`. 

Nếu cô ấy vẽ màu đen trước, con rồng sẽ rút từ một con chuột trắng và một con chuột đen. Con rồng chỉ sống sót nếu nó cũng rút được màu đen, xác suất`1/2`. Sau đó con chuột trắng duy nhất còn lại nhảy đi, không để lại con chuột trắng nào cho công chúa. 

Sự tái phát tính toán:```
dp[1][2]
= 1/3 + (2/3)*(1/2)*dp[0][0]
= 1/3
```Trường hợp này kiểm tra tình huống tế nhị khi cú nhảy hoảng loạn loại bỏ con chuột trắng cuối cùng.

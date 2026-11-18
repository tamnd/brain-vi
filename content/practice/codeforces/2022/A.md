---
title: "CF 2022A - Xe buýt tới P\u00e9njamo"
description: "Mỗi hàng xe buýt có đúng hai chỗ ngồi. Chúng tôi có một số gia đình và gia đình tôi có những người ai. Một người hạnh phúc ở một trong hai tình huống. Họ ngồi cạnh một thành viên trong cùng một gia đình, hoặc họ ngồi một mình ở hàng ghế còn lại trống."
date: "2026-06-08T12:36:42+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2022
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 978 (Div. 2)"
rating: 800
weight: 2022
solve_time_s: 138
verified: true
draft: false
---

[CF 2022A - Xe buýt đến P\u00e9njamo](https://codeforces.com/problemset/problem/2022/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, thực hiện, toán học 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi hàng xe buýt có đúng hai chỗ ngồi. Chúng tôi có nhiều gia đình và gia đình`i`chứa`a_i`mọi người. 

Một người hạnh phúc ở một trong hai tình huống. Họ ngồi cạnh một thành viên trong cùng một gia đình, hoặc họ ngồi một mình ở hàng ghế còn lại trống. 

Chúng ta phải xếp từng người lên xe buýt và tối đa hóa tổng số người hạnh phúc. 

Những hạn chế là rất nhỏ. Mỗi gia đình có nhiều nhất là 10 người`n`nhiều nhất là 100 và có nhiều nhất 1000 ca kiểm thử. Thậm chí một`O(n)`hoặc`O(n log n)`giải pháp là quá đủ. Thách thức không phải là hiệu suất, mà là tìm ra đối số đếm phù hợp. 

Một sai lầm phổ biến là tập trung vào việc sắp xếp chỗ ngồi một cách rõ ràng. Câu trả lời có thể được tính toán trực tiếp từ một vài lần đếm. 

Xem xét đầu vào```
1
2 2
3 1
```Gia đình cỡ 3 đóng góp một cặp cùng gia đình và một người còn sót lại. Chúng ta có thể xếp cặp ngồi cạnh nhau và sau đó hai người còn lại phải ngồi chung một hàng. Câu trả lời đúng là 2, không phải 3. Một chiến lược tham lam coi mọi người còn sót lại là hạnh phúc sẽ đánh giá quá cao. 

Một trường hợp phức tạp khác là```
1
4 5
1 1 1 1
```Không có cặp gia đình nào cả. Chúng tôi có bốn người và năm hàng nên mỗi người có thể ngồi một mình. Câu trả lời là 4. Bất kỳ cách tiếp cận nào chỉ tính các cặp cùng gia đình sẽ trả về 0 không chính xác. 

Trường hợp cạnh thứ ba là```
1
2 3
3 3
```Mỗi gia đình đóng góp một cặp và một người còn dư. Ta được hai cặp hạnh phúc, tặng ngay 4 người hạnh phúc. Hai người thừa phải chia nhau hàng còn lại nên chẳng ai vui cả. Câu trả lời là 4. Những người còn sót lại không tự động vui vẻ chỉ vì có một hàng trống. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ cố gắng liệt kê cách sắp xếp chỗ ngồi và đánh giá xem có bao nhiêu người cuối cùng hài lòng. Ngay cả đối với một số lượng nhỏ các hàng, số lượng chỗ ngồi có thể được bố trí sẽ tăng lên một cách bùng nổ. Vì mọi người có thể được phân biệt theo thành viên gia đình và các hàng có thể được sắp xếp theo nhiều cách nên điều này nhanh chóng trở nên không khả thi. 

Quan sát quan trọng là hạnh phúc chỉ đến từ hai mẫu hàng. 

Mẫu đầu tiên là một hàng chứa hai thành viên của cùng một họ. Một hàng như vậy tạo ra hai người hạnh phúc và sử dụng hoàn toàn một hàng. Vì hai thành viên trong gia đình ngồi cùng nhau luôn tốt hơn là tách họ ra, nên nên sử dụng mọi cặp cùng một gia đình có thể. 

Đối với một gia đình có quy mô`a_i`, chúng ta có thể hình thành`a_i // 2`những cặp như vậy. Tổng hợp tất cả các gia đình cho```
pairs = Σ(a_i // 2)
```Mỗi cặp đóng góp đúng hai người hạnh phúc. 

Sau khi đặt tất cả các cặp này, mỗi gia đình còn lại tối đa một thành viên chưa ghép đôi. Cho phép```
odd = Σ(a_i % 2)
```là số người còn lại 

Số hàng không sử dụng là```
free_rows = r - pairs
```vì mỗi cặp họ chiếm trọn một hàng. 

Bây giờ chỉ tập trung vào những người còn sót lại. Nếu một người còn sót lại được ngồi một hàng một mình, họ sẽ rất vui. Nếu hai người còn sót lại ngồi chung một hàng thì cũng không vui vì họ đến từ hai gia đình khác nhau. 

Ban đầu, chúng ta có thể đặt một người còn sót lại vào mỗi hàng trống, tạo`free_rows`những người hạnh phúc. Nếu có nhiều đồ ăn thừa hơn số hàng trống, thì mỗi người còn thừa phải ngồi bên cạnh một trong những người độc thân vui vẻ đó. Khi điều đó xảy ra, người độc thân hạnh phúc trước đây sẽ mất đi hạnh phúc. Mỗi người thêm vào sẽ làm giảm số lượng hạnh phúc trong số những người còn sót lại đúng một. 

Điều này dẫn trực tiếp đến công thức tính cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`pairs = Σ(a_i // 2)`. 

Mỗi cặp như vậy có thể cùng nhau chiếm một hàng và góp phần làm hai người hạnh phúc. 
2. Bắt đầu câu trả lời bằng`ans = 2 * pairs`. 

Những người hạnh phúc này được đảm bảo. 
3. Tính toán`odd = Σ(a_i % 2)`. 

Đây là những người còn lại sau khi tất cả các cặp cùng một gia đình có thể được hình thành. 
4. Tính toán`free_rows = r - pairs`. 

Từ`Σ a_i ≤ 2r`, chúng tôi luôn có`pairs ≤ r`. 
5. Nếu`odd ≤ free_rows`, mỗi người còn sót lại có thể ngồi một mình. 

Thêm vào`odd`để trả lời. 
6. Nếu không, xếp một người còn sót lại vào mỗi hàng trống. 

Điều này mang lại`free_rows`những người hạnh phúc ban đầu. 

có`odd - free_rows`thêm nhiều người vẫn chưa có chỗ ngồi. Mỗi người trong số họ phải ngồi cạnh một trong những kẻ độc thân đó, tiêu diệt một con người hạnh phúc trước đây. 

Số lượng thức ăn thừa hạnh phúc trở thành```
free_rows - (odd - free_rows)
= 2 * free_rows - odd
```Thêm giá trị đó vào câu trả lời. 

### Tại sao nó hoạt động 

Mỗi hàng vui vẻ chứa hai người phải chứa các thành viên trong cùng một gia đình. Việc chia tách một cặp gia đình có thể thành các hàng khác nhau không bao giờ có thể làm tăng hạnh phúc, bởi vì giải pháp thay thế tốt nhất là làm cho nhiều nhất một trong số họ hạnh phúc khi còn độc thân. Vì vậy việc sử dụng tất cả các cặp gia đình có sẵn luôn là tối ưu. 

Sau khi sử dụng hết các cặp, mỗi gia đình còn lại đóng góp tối đa một người. Không thể tạo thêm cặp cùng họ. Cách duy nhất để những người này trở nên hạnh phúc là ngồi một mình. Mỗi hàng miễn phí có thể hỗ trợ tối đa một singleton hạnh phúc. Bất cứ khi nào một người còn sót lại thứ hai được chèn vào một hàng như vậy, chính xác một người hạnh phúc sẽ bị mất. Công thức trên tính hiệu ứng đó một cách chính xác, điều này chứng tỏ tính tối ưu của thuật toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, r = map(int, input().split())
    a = list(map(int, input().split()))

    pairs = sum(x // 2 for x in a)
    odd = sum(x % 2 for x in a)

    ans = 2 * pairs
    free_rows = r - pairs

    if odd <= free_rows:
        ans += odd
    else:
        ans += 2 * free_rows - odd

    print(ans)
```Vòng lặp đầu tiên đếm xem có thể hình thành bao nhiêu cặp cùng họ. Mỗi cặp như vậy ngay lập tức đóng góp hai người hạnh phúc. 

Số thứ hai theo dõi số lượng người chưa ghép đôi còn lại. Đây là những người duy nhất mà hạnh phúc vẫn chưa được quyết định.`free_rows`là số hàng chưa có cặp họ nào chiếm giữ. Nhánh cuối cùng xử lý hai tình huống có thể xảy ra. Mỗi người còn sót lại sẽ có hàng riêng của mình hoặc có nhiều hàng thừa hơn các hàng có sẵn và một số đơn lẻ phải được ghép nối với nhau. 

Không cần xử lý đặc biệt đối với các giá trị lớn vì tất cả các số đều rất nhỏ. biểu hiện`2 * free_rows - odd`không bao giờ âm trong các trạng thái hợp lệ vì tổng số người vừa đủ trên xe buýt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
2 3 1
```| Quy mô gia đình | Các cặp đã thêm | Đã thêm lẻ | 
| --- | --- | --- | 
| 2 | 1 | 0 | 
| 3 | 1 | 1 | 
| 1 | 0 | 1 | 

Sau khi xử lý tất cả các họ: 

| Biến | Giá trị | 
| --- | --- | 
| cặp | 2 | 
| lẻ | 2 | 
| free_rows | 1 | 
| câu trả lời ban đầu | 4 | 

Từ`odd > free_rows`: 

| Tính toán | Giá trị | 
| --- | --- | 
| thức ăn thừa hạnh phúc | 2 * 1 - 2 = 0 | 
| câu trả lời cuối cùng | 4 | 

Hai cặp gia đình đã chiếm tất cả những người hạnh phúc. Hai phần còn lại phải chia nhau một hàng. 

### Ví dụ 2 

đầu vào:```
3 3
2 2 2
```| Quy mô gia đình | Các cặp đã thêm | Đã thêm lẻ | 
| --- | --- | --- | 
| 2 | 1 | 0 | 
| 2 | 1 | 0 | 
| 2 | 1 | 0 | 

Sau khi xử lý tất cả các họ: 

| Biến | Giá trị | 
| --- | --- | 
| cặp | 3 | 
| lẻ | 0 | 
| free_rows | 0 | 
| câu trả lời ban đầu | 6 | 

Vì không còn người nào nên đáp án vẫn là 6. 

Ví dụ này chứng tỏ rằng khi mọi người có thể được xếp vào các cặp cùng một gia đình thì mọi người đều trở nên hạnh phúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần vượt qua quy mô gia đình | 
| Không gian | O(1) | Chỉ có một vài quầy được lưu trữ | 

Trường hợp thử nghiệm lớn nhất chỉ chứa 100 kích cỡ họ, do đó việc quét tuyến tính là không đáng kể. Ngay cả với 1000 trường hợp thử nghiệm, tổng công việc vẫn thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, r = map(int, input().split())
        a = list(map(int, input().split()))

        pairs = sum(x // 2 for x in a)
        odd = sum(x % 2 for x in a)

        ans = 2 * pairs
        free_rows = r - pairs

        if odd <= free_rows:
            ans += odd
        else:
            ans += 2 * free_rows - odd

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run(
"""4
3 3
2 3 1
3 3
2 2 2
4 5
1 1 2 2
4 5
3 1 1 3
"""
) == "4\n6\n6\n6", "samples"

# minimum size
assert run(
"""1
1 1
1
"""
) == "1", "single person sits alone"

# all equal values
assert run(
"""1
4 4
2 2 2 2
"""
) == "8", "everyone paired with family"

# leftover people exceed free rows
assert run(
"""1
2 3
3 3
"""
) == "4", "two leftovers must share a row"

# boundary case, bus exactly full
assert run(
"""1
2 2
1 3
"""
) == "2", "only one family pair contributes happiness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| Ví dụ tối thiểu có thể | 
|`2 2 2 2`|`8`| Tất cả mọi người đều có thể ghép đôi với gia đình | 
|`3 3`Và`3 3`|`4`| Thức ăn thừa buộc phải chia sẻ hàng | 
|`1 3`với`r=2`|`2`| Xe buýt chật kín, hàng hỗn hợp không thể tránh khỏi | 

## Vỏ cạnh 

Hãy xem xét```
1
2 2
3 1
```Chúng tôi có`pairs = 1`,`odd = 2`, Và`free_rows = 1`. 

Thuật toán bắt đầu với`ans = 2`. Từ`odd > free_rows`, nó thêm vào`2 * 1 - 2 = 0`. Đáp án cuối cùng là 2. Một người còn sót lại ban đầu có thể ngồi một mình, nhưng người còn lại thứ hai phải ngồi vào hàng đó và loại bỏ niềm vui độc thân. Kết quả là đúng. 

Coi như```
1
4 5
1 1 1 1
```Chúng tôi có`pairs = 0`,`odd = 4`, Và`free_rows = 5`. 

Từ`odd <= free_rows`, mỗi người có thể chiếm một hàng một mình. Thuật toán trả về 4. Điều này xử lý chính xác các trường hợp không có cặp họ nào cả. 

Coi như```
1
2 3
3 3
```Chúng tôi có`pairs = 2`,`odd = 2`, Và`free_rows = 1`. 

Thuật toán tính toán```
ans = 2 * 2 = 4
happy leftovers = 2 * 1 - 2 = 0
```cho đáp án cuối cùng là 4. Hai người thừa không thể cùng vui vì chỉ còn một hàng nên phải ngồi cạnh nhau. Việc đếm khớp chính xác với sự sắp xếp chỗ ngồi tối ưu.

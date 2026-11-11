---
title: "CF 1552B - Chạy tìm vàng"
description: "Mỗi vận động viên được mô tả bằng năm thứ hạng, một thứ hạng từ mỗi cuộc chạy marathon trước đây. Giá trị nhỏ hơn sẽ tốt hơn vì thứ hạng 1 có nghĩa là vị trí đầu tiên. Đối với hai vận động viên x và y, chúng ta nói rằng x cao hơn y nếu x có thứ hạng cao hơn ở ít nhất ba trong số năm cuộc đua marathon."
date: "2026-06-10T13:09:32+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "graphs", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1552
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 15"
rating: 1500
weight: 1552
solve_time_s: 103
verified: true
draft: false
---

[CF 1552B - Chạy đua giành vàng](https://codeforces.com/problemset/problem/1552/B) 

**Đánh giá:** 1500 
**Tags:** tổ hợp, đồ thị, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi vận động viên được mô tả bằng năm thứ hạng, một thứ hạng từ mỗi cuộc chạy marathon trước đây. Giá trị nhỏ hơn sẽ tốt hơn vì thứ hạng 1 có nghĩa là vị trí đầu tiên. 

Đối với hai vận động viên`x`Và`y`, chúng tôi nói rằng`x`vượt trội hơn`y`nếu như`x`có thứ hạng tốt hơn ở ít nhất ba trong số năm cuộc chạy marathon. Vì có đúng năm lần so sánh, một vận động viên sẽ thắng trong cuộc so sánh đối đầu nếu họ giỏi hơn ở hầu hết các khía cạnh. 

Nhiệm vụ của chúng ta là tìm ra một vận động viên đánh bại mọi vận động viên khác trong cuộc so sánh theo cặp này. Nếu có một vận động viên như vậy, hãy xuất ra chỉ số của họ. Nếu không thì xuất ra`-1`. 

Điều đầu tiên cần lưu ý là mối quan hệ ưu việt không có tính bắc cầu. Nếu vận động viên A đánh bại B và B đánh bại C, thì A không nhất thiết phải đánh bại C. Điều này ngay lập tức loại trừ nhiều ý tưởng dựa trên sự sắp xếp tiêu chuẩn. Trên thực tế, các chu kỳ có thể tồn tại. 

Những hạn chế là đầu mối thực sự. Tổng số vận động viên trong tất cả các trường hợp thử nghiệm tối đa là 50.000. Một thuật toán bậc hai sẽ thực hiện khoảng$$50{,}000^2 = 2.5 \times 10^9$$so sánh cặp trong trường hợp xấu nhất, vượt xa những gì phù hợp trong một giây. Chúng ta cần một cái gì đó gần tuyến tính trong`n`. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một mối quan hệ tuần hoàn:```
3
10 10 20 30 30
20 20 30 10 10
30 30 10 20 20
```Vận động viên 1 nhịp 2, vận động viên 2 nhịp 3 và vận động viên 3 nhịp 1. Không vận động viên nào đánh bại người khác, vì vậy câu trả lời là`-1`. Bất kỳ cách tiếp cận nào cố gắng chọn vận động viên có "thứ hạng trung bình tốt nhất" sẽ trả về một người nào đó một cách không chính xác. 

Một trường hợp tinh vi khác là khi một thí sinh đánh bại nhiều vận động viên nhưng không phải tất cả:```
3
1 1 1 100 100
2 2 2 1 1
3 3 3 2 2
```Vận động viên 1 đánh bại vận động viên 2, nhưng vận động viên 2 đánh bại vận động viên 3. Chúng tôi vẫn phải xác minh rõ ràng rằng ứng cử viên cuối cùng đánh bại mọi vận động viên. Chỉ loại bỏ đối thủ thôi là chưa đủ. 

Trường hợp thử nghiệm nhỏ nhất có thể cũng đáng để kiểm tra:```
1
50000 1 50000 50000 50000
```Chỉ với một vận động viên, vận động viên đó đã đáp ứng được yêu cầu một cách tầm thường. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi vận động viên`i`, so sánh họ với mọi vận động viên khác`j`. Nếu như`i`đánh bại tất cả những người khác, trở lại`i`. 

Việc so sánh giữa hai vận động viên mất nhiều thời gian vì chỉ có năm thứ hạng. Vấn đề là số lượng so sánh. Chúng tôi biểu diễn`O(n²)`kiểm tra cặp. Với`n = 50,000`, tức là khoảng 2,5 tỷ so sánh, quá chậm. 

Để có được thứ gì đó nhanh hơn, chúng ta cần khai thác thực tế là chỉ có năm chiều. 

Giả sử có một nhà vô địch hợp lệ. Hãy để vận động viên đó được`C`. 

Hãy tưởng tượng việc quét các vận động viên từ trái sang phải trong khi vẫn duy trì một ứng cử viên hiện tại. Bất cứ khi nào một vận động viên khác đánh bại ứng cử viên hiện tại, chúng tôi sẽ thay thế ứng cử viên đó. 

Tại sao điều này lại thú vị? Nếu ứng cử viên hiện tại thua ai đó, thì ứng cử viên hiện tại không bao giờ có thể trở thành nhà vô địch toàn cầu. Một nhà vô địch phải đánh bại tất cả mọi người. Thua dù chỉ một lần sẽ bị loại ngay lập tức. 

Đây chính xác là ý tưởng loại bỏ tương tự được sử dụng để tìm ra yếu tố thống trị hoặc người chiến thắng giải đấu. Mọi so sánh đều loại bỏ một vận động viên khỏi sự cân nhắc. 

Sau một lần vượt qua tuyến tính, chỉ có một ứng cử viên sống sót. 

Câu hỏi còn lại là liệu người sống sót có thực sự hợp lệ hay không. Vì mối quan hệ ưu việt không mang tính bắc cầu nên kẻ sống sót chỉ là nhà vô địch tiềm năng. Chúng ta phải xác minh điều đó đối với tất cả các vận động viên. 

Quan sát chính là: 

Nếu một nhà vô địch tồn tại, thẻ loại trừ không thể loại bỏ nó. Bất kỳ vận động viên nào đối mặt với nhà vô địch đều thua cuộc, vì vậy nhà vô địch cuối cùng trở thành hoặc vẫn là ứng cử viên. Vì vậy, giai đoạn loại trừ luôn kết thúc với nhà vô địch thực sự nếu có. 

Sau khi có được ứng cử viên, lần quét tuyến tính thứ hai sẽ kiểm tra xem nó có đánh bại mọi vận động viên hay không. Nếu bất kỳ vận động viên nào đánh bại ứng cử viên thì không có nhà vô địch nào tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các vận động viên và lưu trữ năm thứ hạng của họ. 
2. Xác định hàm so sánh`better(a, b)`điều đó trả về đúng nếu vận động viên`a`có thứ hạng nhỏ hơn vận động viên`b`trong ít nhất ba trong số năm cuộc chạy marathon. 
3. Bắt đầu với vận động viên`0`với tư cách là ứng cử viên hiện tại. 
4. Quét vận động viên từ trái sang phải. 
5. Đối với mỗi vận động viên`i`, so sánh`i`với ứng viên hiện tại. 
6. Nếu vận động viên`i`đánh bại ứng cử viên hiện tại, thay thế ứng cử viên bằng`i`. 

Ứng viên cũ bây giờ đã thua khi so sánh đối đầu nên không thể nào vượt trội hơn mọi người được. 
7. Sau khi quá trình quét kết thúc, chúng ta có một ứng cử viên duy nhất còn sống sót. 
8. Xác minh ứng viên bằng cách so sánh với mọi vận động viên. 
9. Nếu có vận động viên nào đánh bại ứng cử viên, hãy ghi`-1`. 
10. Nếu không thì xuất chỉ mục dựa trên 1 của ứng viên. 

### Tại sao nó hoạt động 

Trong quá trình vượt qua vòng loại, mỗi người thay thế sẽ loại bỏ một vận động viên không thể là nhà vô địch toàn cầu. Nếu vận động viên`A`thua vận động viên`B`, sau đó`A`chắc chắn là không vượt trội so với tất cả các vận động viên. 

Giả sử một nhà vô địch hợp lệ`C`tồn tại. Bất cứ khi nào`C`được so sánh với vận động viên khác,`C`thắng. Do đó một lần`C`trở thành ứng cử viên, nó không bao giờ có thể bị loại bỏ. Ngay cả trước khi điều đó xảy ra, mỗi vận động viên loại bỏ một vận động viên khác chỉ đơn thuần là loại bỏ một người không vô địch. Cuối cùng quá trình quét kết thúc với`C`với tư cách là ứng cử viên. 

Nếu không có nhà vô địch nào tồn tại, đường loại trừ vẫn tạo ra một số người sống sót. Thẻ xác minh phát hiện điều này bằng cách tìm một vận động viên đánh bại người sống sót. Do đó, thuật toán trả về một vận động viên hợp lệ chính xác khi vận động viên đó tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def better(a, b):
    cnt = 0
    for k in range(5):
        if a[k] < b[k]:
            cnt += 1
    return cnt >= 3

t = int(input())

for _ in range(t):
    n = int(input())
    athletes = [list(map(int, input().split())) for _ in range(n)]

    cand = 0

    for i in range(1, n):
        if better(athletes[i], athletes[cand]):
            cand = i

    ok = True
    for i in range(n):
        if i != cand and not better(athletes[cand], athletes[i]):
            ok = False
            break

    print(cand + 1 if ok else -1)
```các`better`hàm thực hiện trực tiếp định nghĩa của vấn đề. Nó đếm có bao nhiêu trong số năm vận động viên marathon`a`thi đấu tốt hơn vận động viên`b`. Chiến thắng ít nhất ba chiều có nghĩa là chiến thắng trong sự so sánh đa số. 

Vòng lặp đầu tiên thực hiện giai đoạn loại bỏ. Bất cứ khi nào một vận động viên khác đánh bại ứng cử viên hiện tại, ứng cử viên hiện tại sẽ bị loại vĩnh viễn. 

Vòng thứ hai là giai đoạn xác minh quan trọng. Mối quan hệ ưu việt không mang tính bắc cầu, do đó người sống sót sau lần vượt qua đầu tiên không tự động có hiệu lực. Chúng ta phải kiểm tra rõ ràng rằng nó có thể đánh bại mọi vận động viên. 

Các chỉ số được lưu trữ nội bộ dưới dạng giá trị dựa trên 0 vì danh sách Python sử dụng lập chỉ mục dựa trên 0. Câu trả lời cuối cùng thêm một số trước khi in vì bài toán sử dụng cách đánh số vận động viên dựa trên một. 

Không cần xử lý đặc biệt đối với`n = 1`. Vòng loại trừ không làm gì, vòng xác minh thành công và vận động viên 1 được in ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1 1 1 1
2 2 2 2 2
3 3 3 3 3
```Giai đoạn loại bỏ: 

| Bước | Ứng viên hiện tại | Người thách đấu | Kẻ thách thức đánh bại ứng cử viên? | Ứng viên mới | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | - | - | 1 | 
| 1 | 1 | 2 | Không | 1 | 
| 2 | 1 | 3 | Không | 1 | 

Giai đoạn xác minh: 

| Vận động viên đã kiểm tra | Ứng viên đánh bại vận động viên? | 
| --- | --- | 
| 2 | Có | 
| 3 | Có | 

Câu trả lời là vận động viên 1. Ví dụ này cho thấy trường hợp thành công đơn giản nhất khi một vận động viên chiếm ưu thế trong mọi cuộc so sánh. 

### Ví dụ 2 

đầu vào:```
3
10 10 20 30 30
20 20 30 10 10
30 30 10 20 20
```Giai đoạn loại bỏ: 

| Bước | Ứng viên hiện tại | Người thách đấu | Kẻ thách thức đánh bại ứng cử viên? | Ứng viên mới | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | - | - | 1 | 
| 1 | 1 | 2 | Không | 1 | 
| 2 | 1 | 3 | Có | 3 | 

Giai đoạn xác minh: 

| Vận động viên đã kiểm tra | Ứng viên đánh bại vận động viên? | 
| --- | --- | 
| 1 | Có | 
| 2 | Không | 

Vì vận động viên 3 không đánh bại vận động viên 2 nên việc xác minh không thành công và câu trả lời là`-1`. 

Ví dụ này chứng minh tại sao việc vượt qua thứ hai là cần thiết. Chỉ riêng giai đoạn loại trừ sẽ đưa vận động viên 3 trở lại không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét loại trừ và một lần quét xác minh | 
| Không gian | O(n) | Lưu trữ thứ hạng của vận động viên | 
| Thêm không gian | O(1) | Ngoài bộ lưu trữ đầu vào | 

Mỗi vận động viên tham gia vào một số lượng so sánh không đổi và mỗi so sánh chỉ kiểm tra năm thứ hạng. Vì tổng số vận động viên trong tất cả các trường hợp thử nghiệm tối đa là 50.000 nên giải pháp này phù hợp một cách thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    def better(a, b):
        cnt = 0
        for k in range(5):
            if a[k] < b[k]:
                cnt += 1
        return cnt >= 3

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        athletes = [list(map(int, input().split())) for _ in range(n)]

        cand = 0

        for i in range(1, n):
            if better(athletes[i], athletes[cand]):
                cand = i

        ok = True
        for i in range(n):
            if i != cand and not better(athletes[cand], athletes[i]):
                ok = False
                break

        ans.append(str(cand + 1 if ok else -1))

    print("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue()

# provided sample
assert run(
"""4
1
50000 1 50000 50000 50000
3
10 10 20 30 30
20 20 30 10 10
30 30 10 20 20
3
1 1 1 1 1
2 2 2 2 2
3 3 3 3 3
6
9 5 3 7 1
7 4 1 6 8
5 6 7 3 2
6 7 8 8 6
4 2 2 4 5
8 3 6 9 4
"""
) == "1\n-1\n1\n5\n"

# n = 1
assert run(
"""1
1
1 2 3 4 5
"""
) == "1\n"

# clear champion
assert run(
"""1
3
1 1 1 1 1
2 2 2 2 2
3 3 3 3 3
"""
) == "1\n"

# cyclic relation
assert run(
"""1
3
10 10 20 30 30
20 20 30 10 10
30 30 10 20 20
"""
) == "-1\n"

# candidate must be verified
assert run(
"""1
3
1 1 1 100 100
2 2 2 1 1
3 3 3 2 2
"""
) == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vận động viên đơn | 1 | Trường hợp cơ bản tầm thường | 
| Đặt hàng thống trị nghiêm ngặt | 1 | Nhà vô địch tồn tại | 
| Quan hệ ưu việt tuần hoàn | -1 | Giai đoạn xác minh là bắt buộc | 
| Thứ hạng hỗn hợp với người chiến thắng hợp lệ | 1 | Ứng viên sống sót sau khi bị loại và xác minh | 

## Vỏ cạnh 

Hãy xem xét ví dụ tuần hoàn:```
3
10 10 20 30 30
20 20 30 10 10
30 30 10 20 20
```Vòng loại kết thúc với vận động viên 3. Một giải pháp bất cẩn có thể ngay lập tức đưa ra kết quả 3. Trong quá trình xác minh, vận động viên 2 đánh bại vận động viên 3, do đó thuật toán trả về chính xác`-1`. 

Xét trường hợp một vận động viên:```
1
50000 1 50000 50000 50000
```Thí sinh bắt đầu với tư cách vận động viên 1. Không xảy ra so sánh loại trừ. Việc xác minh cũng không thực hiện những kiểm tra có ý nghĩa vì không có đối thủ. Đầu ra của thuật toán`1`, phù hợp với định nghĩa. 

Hãy xem xét trường hợp nhà vô địch ban đầu không phải là ứng cử viên:```
3
2 2 2 2 2
1 1 1 1 1
3 3 3 3 3
```Ứng viên bắt đầu với tư cách là vận động viên 1. Vận động viên 2 đánh bại vận động viên 1 và trở thành ứng cử viên mới. Vận động viên 3 không thể đánh bại vận động viên 2 nên vận động viên 2 sống sót. Xác minh xác nhận vận động viên 2 đánh bại tất cả mọi người. Câu trả lời là`2`. 

Hãy xem xét trường hợp một vận động viên có vẻ mạnh ở địa phương nhưng không phải là người chiến thắng toàn cầu:```
3
1 1 1 100 100
2 2 2 1 1
3 3 3 2 2
```Vận động viên 1 đánh bại vận động viên 2 ở ba chiều và vận động viên 3 ở ba chiều. Thẻ loại sẽ giữ lại vận động viên 1. Xác minh xác nhận vận động viên 1 đánh bại mọi vận động viên khác, vì vậy câu trả lời là`1`. Điều này cho thấy thuật toán dựa vào tính ưu việt theo cặp thực tế hơn là các số liệu thống kê tổng hợp như tổng hoặc trung bình.

---
title: "CF 140B - Thiệp Năm Mới"
description: "Alexander lần lượt nhận được thiệp từ bạn bè. Bạn tôi gửi thẻ i nên số thẻ và số bạn bè giống nhau. Bất cứ lúc nào Alexander cũng có thể quyết định gửi thiệp cho một số bạn bè. Anh ấy không bao giờ tạo thẻ mới, anh ấy chỉ sử dụng lại những thẻ đã nhận được."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 140
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 100"
rating: 1800
weight: 140
solve_time_s: 161
verified: false
draft: false
---

[CF 140B - Thiệp Năm Mới](https://codeforces.com/problemset/problem/140/B) 

**Đánh giá:** 1800 
**Tags:** vũ phu, tham lam, thực hiện 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Alexander lần lượt nhận được thiệp từ bạn bè. Bạn bè`i`gửi thẻ`i`, vậy số thẻ và số bạn bè là như nhau. Bất cứ lúc nào Alexander cũng có thể quyết định gửi thiệp cho một số bạn bè. Anh ấy không bao giờ tạo thẻ mới, anh ấy chỉ sử dụng lại những thẻ đã nhận được. 

Khi Alexander gửi thiệp cho bạn bè`i`, áp dụng hai quy tắc. Anh ấy không thể gửi thẻ`i`quay lại với người bạn đó, và trong số tất cả những tấm thẻ còn lại mà anh ấy hiện đang sở hữu, anh ấy luôn chọn tấm thẻ mà bản thân anh ấy thích nhất. 

Mỗi người bạn cũng có thứ hạng thẻ riêng của mình. Chúng tôi muốn mọi người bạn nhận được tấm thẻ tốt nhất mà họ có thể nhận được dưới hành vi của Alexander. Nhiệm vụ của chúng ta không phải là trực tiếp chọn quân bài, vì sự lựa chọn của Alexander là bắt buộc. Thay vào đó, chúng tôi chọn thời điểm mỗi người bạn nhận được thẻ của mình. Đầu ra yêu cầu những khoảnh khắc này ở dạng nén: cho mọi người bạn`i`, in một số người bạn`k`đến nỗi Alexander gửi tấm thiệp tới`i`ngay sau khi nhận được thẻ`k`. 

Các ràng buộc đủ nhỏ để cho phép mô phỏng khá trực tiếp.`n`nhiều nhất là`300`, vì vậy ngay cả các thuật toán bậc ba cũng được. MỘT`O(n^3)`giải pháp thực hiện xung quanh`27 million`trong trường hợp xấu nhất, có thể thoải mái trong Python dưới giới hạn 2 giây. Điều này có nghĩa là chúng ta không cần các thuật toán đồ thị hoặc kỹ thuật luồng nâng cao. Thách thức ở đây là hiểu được cấu trúc của quy trình và chứng minh rằng việc xây dựng tham lam là hợp lệ. 

Có một số trường hợp tế nhị dễ dàng phá vỡ lối suy luận ngây thơ. 

Một sai lầm phổ biến là cho rằng mỗi người bạn sẽ nhận được thẻ yêu thích của họ trong số tất cả các thẻ ngoại trừ thẻ của họ. Điều đó bỏ qua các tùy chọn và tính khả dụng của thẻ của Alexander. 

Coi như:```
n = 3

Friend 1: 2 3 1
Friend 2: 1 3 2
Friend 3: 1 2 3

Alexander: 3 2 1
```Bạn 1 rất thích thiệp`2`, nhưng Alexander thích bài hơn`3`hơn. Thẻ một lần`3`sẵn sàng, Alexander phải gửi nó thay vì`2`. Vì vậy, thời gian quan trọng hơn bộ thẻ cuối cùng có sẵn. 

Một lỗi dễ mắc khác là quên rằng các thẻ xuất hiện theo thời gian.```
n = 3

Friend 1: 3 2 1
Friend 2: 3 1 2
Friend 3: 1 2 3

Alexander: 3 1 2
```Bạn`1`Và`2`cả hai đều muốn thẻ`3`, nhưng không ai có thể nhận được trước khi Alexander nhận được thẻ`3`từ bạn bè`3`. Một giải pháp chỉ quan tâm đến thứ hạng mà không tôn trọng thời gian đến sẽ tạo ra những lịch trình không thể thực hiện được. 

Cái bẫy thứ ba là cho rằng mọi thao tác gửi phải xảy ra vào một thời điểm riêng biệt. Tuyên bố rõ ràng cho phép Alexander gửi thiệp cho nhiều người bạn ngay sau khi nhận được cùng một thẻ. Điều này quan trọng vì nhiều bạn bè có thể nhận được cùng một thẻ vào cùng một thời điểm một cách tối ưu. 

Ví dụ:```
n = 4

Alexander: 4 3 2 1
```Nếu thẻ`4`có sẵn và hợp lệ cho một số bạn bè, tất cả họ có thể nhận được nó ngay sau khi kết bạn`4`gửi thẻ của họ. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là mô phỏng mọi lịch trình nhận và gửi thẻ có thể. Sau mỗi thẻ nhận được, Alexander có thể gửi thẻ cho bất kỳ nhóm nhỏ bạn bè nào không hài lòng. Đối với mỗi người bạn, chúng tôi có thể kiểm tra xem cuối cùng họ sẽ nhận được thẻ nào và giữ lịch trình tốt nhất. 

Điều này nhanh chóng trở thành không thể. Thậm chí sau mỗi lần`n`khi nhận sự kiện, có rất nhiều lựa chọn về việc bạn bè sẽ xử lý theo cấp số nhân. Tổng số lịch trình bùng nổ vượt xa mọi thứ có thể quản lý được. 

Quan sát quan trọng là hành vi của Alexander hoàn toàn mang tính quyết định một khi chúng ta biết những quân bài nào hiện có sẵn. 

Giả sử Alexander đã nhận được một số bộ thẻ. Dành cho bạn bè`i`, Alexander quét thứ tự ưu tiên của mình từ tốt nhất đến tệ nhất và chọn thẻ có sẵn đầu tiên không phù hợp`i`. 

Vì vậy đối với mỗi người bạn`i`, thẻ nhận được của họ chỉ phụ thuộc vào bộ thẻ mà Alexander hiện đang sở hữu. 

Bây giờ hãy nghĩ xem những bộ nào thực sự có thể thực hiện được. Sau khi nhận được thẻ`1...k`, Alexander sở hữu chính xác những lá bài đó. Không có trạng thái nào khác có thể xảy ra. Điều này có nghĩa là quá trình này chỉ có`n`những khoảnh khắc ý nghĩa:```
after receiving 1
after receiving 2
...
after receiving n
```Đối với mỗi người bạn và mỗi tiền tố`1...k`, chúng ta có thể tính toán xem Alexander sẽ gửi lá bài nào vào thời điểm đó. 

Trong số tất cả những khoảnh khắc này, chúng tôi chọn khoảnh khắc mang lại cho người bạn thẻ xếp hạng cao nhất theo danh sách ưu tiên của người bạn đó. 

Điều này biến vấn đề thành vấn đề thuần túy đối với tiền tố. 

Dành cho bạn bè`i`và thời gian`k`:```
available cards = {1,2,...,k}
chosen card = highest-ranked available card in Alexander's order, excluding i
```Sau đó, chúng tôi so sánh tất cả những gì có thể`k`và chọn kết quả tốt nhất cho bạn bè`i`. 

Sức mạnh vũ phu trở nên khả thi vì chỉ có`n^2`trạng thái, và việc đánh giá từng trạng thái mất nhiều nhất`O(n)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm lịch trình đầy đủ | Hàm mũ | Hàm mũ | Quá chậm | 
| Mô phỏng tiền tố | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị sở thích của mỗi người bạn. 
2. Đọc hoán vị ưu tiên của Alexander. 
3. Xây dựng bảng vị trí cho từng người bạn.`rank[i][x]`dự trữ bao nhiêu bạn ơi`i`thẻ thích`x`. Giá trị nhỏ hơn có nghĩa là thẻ tốt hơn. 
4. Đối với mỗi độ dài tiền tố`k`từ`1`ĐẾN`n`, xác định xem Alexander sẽ gửi thẻ nào cho mỗi người bạn nếu anh ấy gửi thẻ ngay sau khi nhận được thẻ`k`. 

Lúc này Alexander đang sở hữu quân bài`1...k`. 
5. Để xác định thẻ đã gửi cho bạn bè`i`, hãy quét thứ tự ưu tiên của Alexander từ tốt nhất đến tệ nhất. 

Lá bài đầu tiên thỏa mãn cả hai điều kiện sẽ được chọn:`card <= k`, nghĩa là thẻ đã có sẵn.`card != i`, vì Alexander không thể trả lại thẻ của chính một người bạn. 
6. So sánh thẻ ứng viên này với thẻ tốt nhất đã được tìm thấy cho bạn bè`i`. 

Sử dụng bảng xếp hạng của bạn bè để quyết định xem họ thích thẻ nào hơn. 
7. Nếu thẻ mới tốt hơn, hãy lưu trữ: 

bản thân thẻ, 

và khoảnh khắc`k`khi nó trở nên có thể đạt được. 
8. Sau khi xử lý tất cả các tiền tố, xuất ra những khoảnh khắc đã chọn cho tất cả bạn bè. 

### Tại sao nó hoạt động 

Bất cứ lúc nào sau khi nhận được thẻ`k`, các thẻ có sẵn chính xác là`{1,2,...,k}`. Sự lựa chọn của Alexander trong số những lá bài đó mang tính quyết định vì anh ấy luôn chọn lá bài hợp lệ yêu thích của mình. 

Vì vậy, mọi kết quả khả thi đối với một người bạn đều tương ứng với chính xác một độ dài tiền tố`k`. Thuật toán của chúng tôi kiểm tra rõ ràng tất cả các tiền tố như vậy. 

Đối với mỗi người bạn, chúng tôi giữ thẻ tốt nhất có thể đạt được theo sở thích của người bạn đó. Vì mọi thời điểm gửi có thể đều được kiểm tra nên không có kết quả nào tốt hơn ngoài tìm kiếm của chúng tôi. Do đó, tiền tố được lưu trữ là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    prefs = [list(map(int, input().split())) for _ in range(n)]
    alex = list(map(int, input().split()))

    rank = [[0] * (n + 1) for _ in range(n)]

    for i in range(n):
        for pos, card in enumerate(prefs[i]):
            rank[i][card] = pos

    best_card = [-1] * n
    answer = [1] * n

    for k in range(1, n + 1):

        for i in range(n):

            chosen = -1

            for card in alex:
                if card <= k and card != i + 1:
                    chosen = card
                    break

            if best_card[i] == -1 or rank[i][chosen] < rank[i][best_card[i]]:
                best_card[i] = chosen
                answer[i] = k

    print(*answer)

solve()
```Phần đầu tiên đọc các hoán vị và xây dựng`rank`. Thay vì tìm kiếm liên tục trong danh sách ưu tiên, chúng tôi tính toán trước các vị trí. Điều này biến những so sánh như “bạn có`i`thích thẻ hơn`a`hoặc thẻ`b`?” vào các hoạt động thời gian không đổi. 

Vòng lặp bên ngoài lặp qua tất cả các tiền tố`1...k`. Điều đó tượng trưng trực tiếp cho khoảnh khắc sau khi Alexander nhận được tấm thiệp`k`. 

Đối với mỗi người bạn, chúng tôi mô phỏng sự lựa chọn bắt buộc của Alexander. Chúng tôi quét hoán vị ưu tiên của Alexander từ trái sang phải vì các thẻ trước đó được Alexander ưa thích hơn. Thẻ có sẵn đầu tiên không phải là thẻ của người bạn đó chính xác là thẻ mà Alexander sẽ gửi. 

Sự so sánh```
rank[i][chosen] < rank[i][best_card[i]]
```kiểm tra xem người bạn đó có thích ứng viên mới hơn không. 

Một chi tiết tinh tế là việc lập chỉ mục. Bạn bè được lưu trữ với các chỉ số dựa trên số 0 trong mảng, nhưng số thẻ là dựa trên một. điều kiện```
card != i + 1
```xử lý việc chuyển đổi này một cách cẩn thận. 

Một điểm tinh tế khác là chúng ta lưu giữ khoảnh khắc`k`, không phải bản thân thẻ. Định dạng đầu ra yêu cầu người bạn vừa nhận được thẻ trước khi gửi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
1 2 3 4
4 1 3 2
4 3 1 2
3 4 2 1
3 1 2 4
```Thứ tự ưu tiên của Alexander là:```
3 > 1 > 2 > 4
```Chúng tôi xử lý từng tiền tố một. 

| k | Thẻ có sẵn | Bạn 1 được | Bạn 2 được | Bạn 3 được | Bạn 4 được | 
| --- | --- | --- | --- | --- | --- | 
| 1 | {1} | không | 1 | 1 | 1 | 
| 2 | {1,2} | 2 | 1 | 1 | 1 | 
| 3 | {1,2,3} | 3 | 3 | 1 | 3 | 
| 4 | {1,2,3,4} | 3 | 3 | 1 | 3 | 

Bây giờ hãy so sánh bằng cách sử dụng sở thích của từng người bạn. 

Bạn bè`1`thích hơn`2`qua`3`, vì vậy khoảnh khắc tuyệt vời nhất là`k=2`. 

Bạn bè`2`thích hơn`1`qua`3`, vì vậy khoảnh khắc tuyệt vời nhất là`k=1`. 

Bạn bè`3`không thể nhận thẻ`3`, và thích`1`qua`4`Và`2`, Vì thế`k=1`. 

Bạn bè`4`thích hơn`3`, có thể đạt được từ`k=3`. 

Đầu ra cuối cùng:```
2 1 1 3
```Đầu ra mẫu sử dụng`4`thay vì`3`cho người bạn cuối cùng, điều này cũng hợp lệ vì thẻ`3`vẫn được gửi sau khi nhận được thẻ`4`. 

Dấu vết này chứng tỏ rằng việc trì hoãn thao tác gửi có thể làm cho kết quả tệ hơn. Bạn bè`1`nên nhận thẻ`2`trước thẻ`3`trở nên có sẵn. 

### Ví dụ tùy chỉnh 

đầu vào:```
3
2 3 1
1 3 2
1 2 3
3 2 1
```Alexander thích:```
3 > 2 > 1
```| k | Thẻ có sẵn | Bạn 1 được | Bạn 2 được | Bạn 3 được | 
| --- | --- | --- | --- | --- | 
| 1 | {1} | 1 | 1 | không | 
| 2 | {1,2} | 2 | 1 | 2 | 
| 3 | {1,2,3} | 3 | 3 | 2 | 

Bây giờ hãy đánh giá từ quan điểm của mỗi người bạn. 

| Bạn | Thẻ tốt nhất | Đạt được ở k | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 2 | 1 | 1 | 
| 3 | 2 | 2 | 

Đầu ra:```
2 1 2
```Ví dụ này cho thấy rằng thẻ tối ưu của một người bạn có thể biến mất khi có thẻ ưu tiên hơn cho Alexander. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | có`n`tiền tố,`n`bạn bè, và lên đến`n`quét qua thứ tự ưu tiên của Alexander | 
| Không gian | O(n^2) | Bảng xếp hạng ưu tiên cho tất cả bạn bè | 

Với`n ≤ 300`, thời gian chạy khối dễ dàng đủ nhanh. Xung quanh`27 million`các thao tác đơn giản nằm trong giới hạn của Python đối với cài đặt cuộc thi 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())

    prefs = [list(map(int, input().split())) for _ in range(n)]
    alex = list(map(int, input().split()))

    rank = [[0] * (n + 1) for _ in range(n)]

    for i in range(n):
        for pos, card in enumerate(prefs[i]):
            rank[i][card] = pos

    best_card = [-1] * n
    answer = [1] * n

    for k in range(1, n + 1):
        for i in range(n):

            chosen = -1

            for card in alex:
                if card <= k and card != i + 1:
                    chosen = card
                    break

            if best_card[i] == -1 or rank[i][chosen] < rank[i][best_card[i]]:
                best_card[i] = chosen
                answer[i] = k

    return " ".join(map(str, answer))

# provided sample
assert run(
"""4
1 2 3 4
4 1 3 2
4 3 1 2
3 4 2 1
3 1 2 4
"""
) in ["2 1 1 3", "2 1 1 4"]

# minimum size
assert run(
"""2
1 2
2 1
1 2
"""
) == "2 1"

# everyone likes highest available card
assert run(
"""3
3 2 1
3 2 1
3 2 1
3 2 1
"""
) == "2 1 2"

# delayed sending is worse
assert run(
"""3
2 3 1
1 3 2
1 2 3
3 2 1
"""
) == "2 1 2"

# boundary ordering
assert run(
"""4
2 1 3 4
1 2 3 4
4 3 2 1
3 4 1 2
4 3 2 1
"""
) == "2 1 1 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu`n=2`trường hợp |`2 1`| Đầu vào hợp lệ nhỏ nhất | 
| Mọi người đều thích thẻ mạnh nhất |`2 1 2`| Nhiều bạn bè cùng chia sẻ thời điểm tối ưu | 
| Gửi chậm trễ gây tổn hại |`2 1 2`| Vấn đề thời gian quan trọng hơn tính khả dụng cuối cùng | 
| Trường hợp đặt hàng ranh giới |`2 1 1 3`| Xử lý đúng việc tự loại trừ thẻ | 

## Vỏ cạnh 

Hãy xem xét trường hợp lá bài yêu thích của Alexander cuối cùng cản trở một kết quả tốt hơn cho một người bạn nào đó.```
3
2 3 1
1 3 2
1 2 3
3 2 1
```Tại`k=2`, bạn ơi`1`nhận thẻ`2`. Tại`k=3`, Alexander phải chuyển sang chơi bài`3`bởi vì nó trở nên có sẵn và cao hơn trong thứ hạng của chính anh ấy. Bạn bè`1`thực sự thích`2`, vì vậy thời điểm sớm hơn là tối ưu. Thuật toán xử lý việc này vì nó kiểm tra mọi tiền tố một cách độc lập thay vì cho rằng sau này luôn tốt hơn. 

Bây giờ hãy xem xét trường hợp có nhiều người bạn cần được xử lý cùng một lúc.```
4
4 1 2 3
4 1 2 3
4 1 2 3
1 2 3 4
4 3 2 1
```Ngay khi có thẻ`4`xuất hiện, Alexander gửi nó cho mọi người bạn ngoại trừ người bạn`4`. Thuật toán cho phép điều này một cách tự nhiên vì mỗi người bạn sẽ ghi lại tiền tố tối ưu giống nhau một cách độc lập`k=4`. 

Cuối cùng, hãy xem xét hạn chế tự thẻ.```
2
1 2
2 1
2 1
```Sau khi nhận được thẻ`1`, bạn ơi`1`không thể nhận được nó, vì vậy chưa có lệnh gửi hợp lệ nào tồn tại. Sau khi nhận được thẻ`2`, bạn ơi`1`nhận thẻ`2`. Bạn bè`2`có thể nhận được thẻ`1`ngay sau đó`k=1`. Thuật toán kiểm tra rõ ràng`card != i + 1`, vì vậy nó không bao giờ trả lại thẻ của chính bạn bè.

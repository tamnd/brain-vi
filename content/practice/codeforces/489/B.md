---
title: "CF 489B - Bóng BerSU"
description: "Chúng tôi có hai nhóm vũ công. Mỗi chàng trai có một cấp độ kỹ năng và mỗi cô gái có một cấp độ kỹ năng. Một bé trai và một bé gái chỉ có thể được ghép đôi nếu trình độ kỹ năng của họ khác nhau tối đa là 1. Mỗi vũ công chỉ có thể thuộc về nhiều nhất một cặp. Nhiệm vụ là tạo thành càng nhiều cặp trai gái hợp lệ càng tốt."
date: "2026-06-07T17:35:19+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graph-matchings", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 489
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 277.5 (Div. 2)"
rating: 1200
weight: 489
solve_time_s: 123
verified: true
draft: false
---

[CF 489B - Bóng BerSU](https://codeforces.com/problemset/problem/489/B) 

**Đánh giá:** 1200 
**Thẻ:** dfs và tương tự, dp, so khớp đồ thị, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai nhóm vũ công. Mỗi chàng trai có một cấp độ kỹ năng và mỗi cô gái có một cấp độ kỹ năng. Một bé trai và một bé gái chỉ có thể được ghép đôi nếu trình độ kỹ năng của họ khác nhau tối đa là 1. Mỗi vũ công chỉ có thể thuộc về nhiều nhất một cặp. 

Nhiệm vụ là tạo thành càng nhiều cặp trai gái hợp lệ càng tốt. 

Đầu vào bao gồm hai mảng. Mảng đầu tiên chứa cấp độ kỹ năng của nam sinh và mảng thứ hai chứa cấp độ kỹ năng của nữ sinh. Đầu ra là số lượng cặp rời rạc tối đa có chênh lệch kỹ năng tối đa là 1. 

Những hạn chế là rất nhỏ. Cả hai nhóm đều chứa tối đa 100 vũ công, do đó, ngay cả các thuật toán bậc hai cũng đủ nhanh. Điều này có nghĩa là chúng ta không cần các thuật toán so khớp phức tạp như Hopcroft-Karp. Thách thức là nhận ra cấu trúc cho phép thực hiện một giải pháp tham lam đơn giản. 

Một lỗi phổ biến là ghép các vũ công một cách tham lam theo thứ tự đầu vào ban đầu. Thứ tự của các vũ công trong đầu vào không có ý nghĩa. Ví dụ:```
boys  = [4, 1]
girls = [2, 3]
```Câu trả lời đúng là 2, bằng cách ghép nối`(1,2)`Và`(4,3)`. Quét tham lam từ trái sang phải trên các mảng chưa được sắp xếp chỉ có thể tìm thấy một cặp. 

Một trường hợp tinh tế khác xảy ra khi một số vũ công có cùng kỹ năng:```
boys  = [2, 2, 2]
girls = [1, 2, 3]
```Câu trả lời đúng là 3. Bất kỳ thuật toán nào luôn chọn đối tác tương thích có sẵn đầu tiên mà không cân nhắc việc sắp xếp thứ tự có thể vô tình chặn các kết quả trùng khớp trong tương lai. 

Trường hợp cạnh thứ ba là khi một kết quả khớp có vẻ hợp lệ nên bị bỏ qua vì sau đó có một kết quả phù hợp tốt hơn:```
boys  = [1, 2]
girls = [2, 3]
```Đáp án tối ưu là 2. Ghép đôi chàng trai`2`với cô gái`2`cậu bé lá đầu tiên`1`chưa từng có. Ghép nối`(1,2)`Và`(2,3)`đưa ra hai cặp. Giải pháp phải tránh những xung đột như vậy một cách có hệ thống. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để xem vấn đề là vấn đề đối sánh hai bên. Tạo một biểu đồ trong đó mọi chàng trai được kết nối với mọi cô gái có kỹ năng khác nhau tối đa 1. Sau đó tìm kết quả phù hợp tối đa. 

Cách tiếp cận này đúng vì mọi cặp hợp lệ đều tương ứng với một kết quả khớp trong biểu đồ. Với tối đa 100 bé trai và 100 bé gái, thuật toán đường tăng cường tiêu chuẩn sẽ chạy thoải mái trong giới hạn. 

Tuy nhiên, phối cảnh biểu đồ ẩn chứa một thuộc tính quan trọng. Khả năng tương thích chỉ phụ thuộc vào giá trị số của các kỹ năng. Nếu cả hai mảng kỹ năng được sắp xếp, những vũ công có kỹ năng tương tự sẽ trở thành hàng xóm theo thứ tự được sắp xếp. 

Giả sử cậu bé nhỏ nhất còn lại có kỹ năng`b`, và cô gái nhỏ nhất còn lại có kỹ năng`g`. 

Nếu như`|b - g| ≤ 1`, việc ghép nối chúng luôn an toàn. Không vũ công nào có thể giúp tạo ra câu trả lời lớn hơn bằng cách chờ đợi một người có kỹ năng thậm chí còn nhỏ hơn, bởi vì không tồn tại. 

Nếu như`b < g - 1`, rồi chàng trai`b`không thể sánh được với cô gái hiện tại hay bất kỳ cô gái nào sau này. Tất cả các cô gái sau này đều có kỹ năng ít nhất`g`, vì vậy sự khác biệt của họ từ`b`thậm chí còn lớn hơn. Cậu bé không bao giờ có thể sánh được, vì vậy chúng ta nên loại bỏ cậu ấy. 

Tương tự, nếu`g < b - 1`, cô gái hiện tại không bao giờ có thể sánh bằng và nên bị loại bỏ. 

Quan sát này dẫn trực tiếp đến thuật toán tham lam hai con trỏ sau khi sắp xếp cả hai mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp lưỡng cực Brute Force | O((n+m)·n·m) | O(n·m) | Đã chấp nhận | 
| Sắp xếp + Hai con trỏ | O(n log n + m log m) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng kỹ năng của nam và mảng kỹ năng của nữ. 
2. Sắp xếp cả hai mảng theo thứ tự không giảm. 

Sau khi sắp xếp, các vũ công có trình độ kỹ năng tương tự sẽ xuất hiện gần nhau, điều này cho phép các quyết định địa phương trở nên chính xác trên toàn cầu. 
3. Khởi tạo hai con trỏ,`i`dành cho bé trai và`j`đối với con gái, cả hai đều bắt đầu từ 0. 
4. Khởi tạo`answer = 0`. 
5. Trong khi cả hai con trỏ đều nằm trong mảng của chúng, hãy so sánh các kỹ năng hiện tại. 
6. Nếu`abs(boys[i] - girls[j]) <= 1`, tạo thành một cặp, tăng dần`answer`và nâng cao cả hai con trỏ. 

Cặp này hợp lệ và việc giữ một trong hai vũ công cho đối tác sau không thể cải thiện kết quả. 
7. Ngược lại, nếu`boys[i] < girls[j]`, nâng cao`i`. 

Chàng trai hiện tại quá yếu đuối để có thể sánh được với cô gái hiện tại. Vì tất cả các cô gái sau này ít nhất cũng mạnh mẽ như vậy nên chàng trai này sẽ không bao giờ có thể tham gia vào bất kỳ cặp đôi nào trong tương lai. 
8. Nếu không, hãy tiến lên`j`. 

Đối xứng, cô gái hiện tại quá yếu để có thể sánh ngang với chàng trai hiện tại và không bao giờ có thể tham gia vào bất kỳ cặp đôi nào trong tương lai. 
9. Khi một con trỏ đến cuối mảng, dừng và in`answer`. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán sẽ kiểm tra cậu bé nhỏ nhất chưa từng có và cô gái nhỏ nhất chưa từng có. 

Khi kỹ năng của họ khác nhau tối đa 1, việc ghép đôi họ không thể làm giảm câu trả lời tối ưu. Bất kỳ đối tác tương lai nào của một trong hai vũ công đều có kỹ năng ít nhất bằng ứng viên hiện tại, vì vậy việc hoãn trận đấu không thể tạo thêm cơ hội. 

Khi một vũ công quá yếu để đấu với người kia, vũ công đó không thể đấu với bất kỳ vũ công nào sau đó trong mảng được sắp xếp ngược lại. Việc loại bỏ chúng chẳng mất gì vì chúng đã không thể ghép đôi được nữa. 

Thuật toán không bao giờ loại bỏ một vũ công vẫn có thể đóng góp vào giải pháp tối ưu. Mỗi quyết định bảo toàn số lượng tối đa các cặp tương lai có thể đạt được. Bằng cách quy nạp theo các vị trí con trỏ, số đếm cuối cùng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    boys = list(map(int, input().split()))

    m = int(input())
    girls = list(map(int, input().split()))

    boys.sort()
    girls.sort()

    i = 0
    j = 0
    ans = 0

    while i < n and j < m:
        if abs(boys[i] - girls[j]) <= 1:
            ans += 1
            i += 1
            j += 1
        elif boys[i] < girls[j]:
            i += 1
        else:
            j += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc đầu vào và sắp xếp cả hai mảng kỹ năng. Sắp xếp là bước tiền xử lý quan trọng giúp cho lý luận tham lam trở nên hợp lệ. 

Các biến`i`Và`j`luôn luôn chỉ vào chàng trai và cô gái nhỏ nhất chưa từng có. Vòng lặp tiếp tục trong khi cả hai nhóm vẫn chứa các vũ công chưa được xử lý. 

Khi các vũ công hiện tại tương thích, một cặp được hình thành và cả hai con trỏ đều di chuyển về phía trước. Vì mỗi vũ công chỉ có thể thuộc về một cặp nên cả hai đều không được xem xét lại. 

Khi kỹ năng của chàng trai hiện tại nhỏ hơn kỹ năng của cô gái hiện tại hơn 1, thì chỉ nâng con trỏ chàng trai là đúng. Bất kỳ cô gái tương lai nào cũng sẽ có kỹ năng thậm chí còn lớn hơn, vì vậy chàng trai hiện tại không còn cơ hội sánh ngang. 

Trường hợp đối xứng tiến tới con trỏ cô gái. 

Không có lo ngại về tràn vì tất cả các giá trị đều rất nhỏ. Chi tiết triển khai duy nhất thường gây ra lỗi là quên sắp xếp trước khi chạy quét hai con trỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 4 6 2
5
5 1 5 7 9
```Sau khi sắp xếp:```
boys  = [1, 2, 4, 6]
girls = [1, 5, 5, 7, 9]
```| tôi | j | cậu bé | cô gái | Hành động | Cặp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | Trận đấu | 1 | 
| 1 | 1 | 2 | 5 | Cậu bé tiến bộ | 1 | 
| 2 | 1 | 4 | 5 | Trận đấu | 2 | 
| 3 | 2 | 6 | 5 | Trận đấu | 3 | 

Thuật toán tìm thấy ba cặp và kết thúc vì tất cả các bé trai đều đã được xử lý. 

### Ví dụ 2 

đầu vào:```
3
2 2 2
3
1 2 3
```Sau khi sắp xếp:```
boys  = [2, 2, 2]
girls = [1, 2, 3]
```| tôi | j | cậu bé | cô gái | Hành động | Cặp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 1 | Trận đấu | 1 | 
| 1 | 1 | 2 | 2 | Trận đấu | 2 | 
| 2 | 2 | 2 | 3 | Trận đấu | 3 | 

Mọi vũ công đều được ghép đôi. Ví dụ này cho thấy các giá trị lặp lại được xử lý một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m log m) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(1) thêm | Chỉ có một vài biến ngoài mảng đầu vào | 

Với tối đa 100 nam và 100 nữ, thời gian chạy rất nhỏ. Ngay cả công thức đối sánh cũng có thể phù hợp một cách thoải mái, nhưng giải pháp hai con trỏ tham lam thì đơn giản hơn và nhanh hơn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    boys = list(map(int, input().split()))
    m = int(input())
    girls = list(map(int, input().split()))

    boys.sort()
    girls.sort()

    i = j = ans = 0

    while i < n and j < m:
        if abs(boys[i] - girls[j]) <= 1:
            ans += 1
            i += 1
            j += 1
        elif boys[i] < girls[j]:
            i += 1
        else:
            j += 1

    return str(ans)

# provided sample
assert run(
    "4\n1 4 6 2\n5\n5 1 5 7 9\n"
) == "3", "sample 1"

# minimum size
assert run(
    "1\n1\n1\n1\n"
) == "1", "single compatible pair"

# no possible matches
assert run(
    "2\n1 1\n2\n5 5\n"
) == "0", "all differences too large"

# all equal values
assert run(
    "4\n3 3 3 3\n3\n3 3 3\n"
) == "3", "every girl matched"

# boundary difference exactly one
assert run(
    "3\n1 2 3\n3\n2 3 4\n"
) == "3", "difference of one is allowed"

# unsorted input
assert run(
    "2\n4 1\n2\n2 3\n"
) == "2", "sorting is necessary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một trai kỹ năng 1, một cô gái kỹ năng 1 | 1 | Phiên bản hợp lệ tối thiểu | 
| Trai [1,1], gái [5,5] | 0 | Không có cặp tương thích | 
| Con trai [3,3,3,3], con gái [3,3,3] | 3 | Giá trị lặp lại | 
| Con trai [1,2,3], con gái [2,3,4] | 3 | Cho phép chênh lệch chính xác 1 | 
| Trai [4,1], gái [2,3] | 2 | Xử lý đúng sau khi sắp xếp | 

## Vỏ cạnh 

Hãy xem xét:```
1
1
1
2
```Sự khác biệt về kỹ năng chính xác là 1. Thuật toán so sánh`1`Và`2`, thấy thế`abs(1 - 2) = 1`, tạo thành một cặp và xuất ra`1`. Bất kỳ việc triển khai nào bằng cách sử dụng`< 1`thay vì`<= 1`sẽ xuất ra không chính xác`0`. 

Coi như:```
2
4 1
2
2 3
```Sau khi sắp xếp:```
boys  = [1, 4]
girls = [2, 3]
```Thuật toán khớp đầu tiên`(1,2)`, sau đó khớp`(4,3)`, đưa ra câu trả lời`2`. Nếu không sắp xếp, quá trình quét tham lam có thể bắt đầu từ`4`và thua một trận. 

Coi như:```
3
2 2 2
3
1 2 3
```Thuật toán lần lượt khớp cả ba cặp. Các giá trị lặp lại không gây khó khăn gì đặc biệt vì các con trỏ luôn di chuyển về phía trước sau trận đấu, ngăn không cho bất kỳ vũ công nào được sử dụng hai lần. 

Coi như:```
2
1 1
2
5 5
```Sự so sánh đầu tiên là`1`so với`5`. Vì cậu bé quá yếu nên thuật toán sẽ nâng cao con trỏ cậu bé. Điều tương tự lại xảy ra với cậu bé thứ hai. Không có cặp nào được hình thành và đầu ra là`0`. Điều này chứng tỏ tại sao việc loại bỏ một vũ công có kỹ năng kém xa là an toàn.

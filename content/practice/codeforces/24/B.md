---
title: "CF 24B - Nhà vô địch F1"
description: "Chúng tôi nhận được kết quả của toàn bộ mùa giải Công thức Một. Mỗi cuộc đua liệt kê các tay đua từ vị trí đầu tiên đến vị trí cuối cùng. Người chiến thắng chức vô địch phụ thuộc vào một trong hai hệ thống xếp hạng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 24
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 24"
rating: 1500
weight: 24
solve_time_s: 139
verified: true
draft: false
---
[CF 24B - Nhà vô địch F1](https://codeforces.com/problemset/problem/24/B) 

**Đánh giá:** 1500 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được kết quả của toàn bộ mùa giải Công thức Một. Mỗi cuộc đua liệt kê các tay đua từ vị trí đầu tiên đến vị trí cuối cùng. Người chiến thắng chức vô địch phụ thuộc vào một trong hai hệ thống xếp hạng. 

Trong hệ thống ban đầu, người lái xe được xếp hạng chủ yếu theo tổng điểm. 10 vị trí dẫn đầu trong mỗi cuộc đua sẽ trao số điểm cố định: 25, 18, 15, 12, 10, 8, 6, 4, 2, 1. Nếu hai tay đua có cùng số điểm, chúng tôi so sánh xem họ có bao nhiêu lần về đích ở vị trí đầu tiên. Nếu vẫn bằng nhau, chúng ta so sánh vị trí thứ hai, rồi đến vị trí thứ ba, v.v. 

Hệ thống thay thế đảo ngược hai ưu tiên đầu tiên. Các tay đua được xếp hạng đầu tiên theo số trận thắng. Nếu số trận thắng bằng nhau thì chúng ta so sánh tổng số điểm. Nếu vẫn hòa, chúng ta lại so sánh số vị trí thứ hai, thứ ba, v.v. 

Đầu vào đưa ra thứ tự cho mọi cuộc đua. Với mỗi chặng đua, chúng tôi nhận được danh sách các tay đua tham gia được sắp xếp theo vị trí về đích. Một tay đua có thể bỏ qua một số cuộc đua và chúng ta vẫn phải xem xét họ trong bảng xếp hạng cuối cùng nếu họ xuất hiện ít nhất một lần. 

Những hạn chế là rất nhỏ. Có tối đa 20 cuộc đua và tổng cộng có tối đa 50 tay đua. Ngay cả khi mỗi cuộc đua có tất cả 50 tay đua thì tổng số kết quả cuộc đua chỉ là 1000. Điều đó có nghĩa là chúng ta không cần những kỹ thuật tối ưu hóa nâng cao hay cấu trúc dữ liệu phức tạp. Việc thực hiện trực tiếp các quy tắc xếp hạng là đủ nhanh. 

Khó khăn thực sự là việc thực hiện việc bẻ dây một cách chính xác. Việc thực hiện bất cẩn thường thất bại vì các quy tắc so sánh mang tính từ điển chứ không chỉ dựa trên điểm và chiến thắng. 

Hãy xem xét ví dụ này:```
2
3
A
B
C
3
B
A
C
```Cả A và B đều ghi được 43 điểm và cùng có một trận thắng. Nhà vô địch cần được xác định bằng cách so sánh vị trí thứ hai. Mỗi người có một vị trí thứ hai nên vẫn bằng nhau. Tuyên bố đảm bảo điều này sẽ không xảy ra trong các thử nghiệm chính thức, nhưng logic so sánh của chúng ta vẫn phải tuân theo đúng thứ tự. 

Một sai lầm dễ mắc phải khác là quên các vị trí ngoài top 10. Những vị trí này không cho điểm nào, nhưng chúng vẫn quan trọng đối với việc phân hạng vì số lượng vị trí được so sánh từ đầu đến cuối. 

Ví dụ:```
2
11
A
B
C
D
E
F
G
H
I
J
K
11
B
A
C
D
E
F
G
H
I
J
K
```A và B đều có một giải nhất và một giải nhì. Điểm của họ cũng ngang nhau. Các vị trí còn lại giống hệt nhau. Một giải pháp có lỗi chỉ lưu trữ 10 kết thúc hàng đầu có thể kết luận sớm việc so sánh một cách không chính xác. 

Một vấn đề tế nhị khác là các tay đua đã bỏ lỡ cuộc đua. Nếu một tay đua không bao giờ xuất hiện trong cuộc đua, chúng ta không được vô tình tạo các vị trí giả cho họ. Bỏ lỡ một cuộc đua đơn giản có nghĩa là họ không giành được điểm và không được tính vị trí. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng toàn bộ mùa giải chính xác như đã viết. Đối với mỗi tay đua, chúng tôi lưu trữ tổng số điểm và số lần họ đã hoàn thành ở mỗi vị trí. Sau khi xử lý tất cả các cuộc đua, chúng tôi so sánh từng cặp tay đua theo quy tắc xếp hạng bắt buộc. 

Phiên bản brute-force có thể tính toán lại tất cả số liệu thống kê từ đầu bất cứ khi nào so sánh hai trình điều khiển. Giả sử chúng ta so sánh tay đua A và B. Chúng ta có thể quét mọi cuộc đua, tính điểm và vị trí cho cả hai, sau đó quyết định xem ai xếp hạng cao hơn. Với tối đa 50 tay đua, chúng tôi có thể thực hiện khoảng 2500 so sánh và mỗi so sánh có thể quét tới 1000 kết quả cuộc đua. Thậm chí điều này vẫn nằm trong giới hạn, khoảng vài triệu hoạt động. 

Tuy nhiên, việc tính toán lại cùng một thông tin nhiều lần là không cần thiết. Mọi so sánh chỉ phụ thuộc vào số liệu thống kê tích lũy của mùa giải. Một khi chúng ta nhận ra điều này, vấn đề sẽ trở nên rõ ràng hơn nhiều. 

Quan sát quan trọng là mùa của mỗi tay đua có thể được biểu thị bằng một bản ghi nhỏ gọn: 

1. Tổng số điểm. 
2. Số vị trí đầu tiên. 
3. Số vị trí thứ hai. 
4. Số vị trí thứ ba. 
5. Và vân vân. 

Sau đó, các yếu tố xếp hạng sẽ trở thành một sự so sánh từ điển thuần túy giữa các bản ghi này, với thứ tự ưu tiên hơi khác nhau cho hai hệ thống tính điểm. 

Đối với hệ thống ban đầu, khóa so sánh là:```
(points, first_places, second_places, ...)
```Đối với hệ thống thay thế, khóa so sánh là:```
(first_places, points, second_places, ...)
```Bởi vì số lượng tay đua và cuộc đua rất ít nên chúng ta có thể chỉ cần lặp lại tất cả các tay đua và giữ lại tay đua tốt nhất theo bộ so sánh đã chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(D2 × R × N) | O(1) | Đã chấp nhận | 
| Tối ưu | O(R × N + D² × N) | O(D × N) | Đã chấp nhận | 

Đây,`D`là số lượng người lái xe,`R`là số chủng tộc, và`N`là số lượng tay đua được phân loại tối đa trong một cuộc đua. 

## Hướng dẫn thuật toán 

1. Tạo từ điển cho tất cả các trình điều khiển. 

Đối với mỗi trình điều khiển, lưu trữ: 

- Tổng số điểm. 
- Một mảng trong đó`places[i]`đếm xem người lái xe đã về đích bao nhiêu lần tại vị trí`i`. 

Chúng tôi cần tính đầy đủ vị trí vì việc hòa có thể tiếp tục diễn ra sau khi thắng. 
2. Xử lý mọi cuộc đua từ trên xuống dưới. 

Tay đua đầu tiên là người chiến thắng, tay đua thứ hai về nhì, v.v. Cập nhật: 

- Bộ đếm vị trí cho vị trí đó. 
- Điểm của tay đua nếu vị trí nằm trong top 10. 
3. Xác định hàm so sánh cho hệ ban đầu. 

So sánh hai trình điều khiển theo thứ tự sau: 

- Tổng số điểm. 
- Số vị trí đầu tiên. 
- Số vị trí thứ hai. 
- Số vị trí thứ ba. 
- Tiếp tục cho đến khi có sự khác biệt. 

Điều này hoàn toàn phù hợp với quy định của giải vô địch. 
4. Xác định một hàm so sánh khác cho hệ thống thay thế. 

So sánh theo thứ tự sau: 

- Số lần thắng. 
- Tổng số điểm. 
- Số vị trí thứ hai. 
- Số vị trí thứ ba. 
- Tiếp tục nếu cần. 

Chiến thắng được kiểm tra trước tiên vì các quy tắc thay thế ưu tiên chiến thắng hơn tính nhất quán. 
5. Lặp lại tất cả các trình điều khiển để tìm trình điều khiển tốt nhất cho mỗi hệ thống. 

Bắt đầu với bất kỳ tay đua nào là nhà vô địch hiện tại. Đối với mọi trình điều khiển khác, hãy áp dụng chức năng so sánh. Thay tướng nếu tay đua mới có thứ hạng cao hơn. 
6. Xuất ra cả hai nhà vô địch. 

### Tại sao nó hoạt động 

Mỗi cuộc đua đóng góp độc lập vào số liệu thống kê mùa giải của mỗi tay đua. Tổng số điểm và số vị trí xác định đầy đủ các quy tắc xếp hạng chính thức. Không có thông tin bổ sung nào về thứ tự chủng tộc hoặc vấn đề nhận dạng chủng tộc. 

Các hàm so sánh phản ánh chính xác báo cáo vấn đề. Nếu một tay đua có nhiều điểm hơn, họ sẽ thắng ngay lập tức trong hệ thống ban đầu. Nếu điểm bằng nhau, số vị trí khác nhau đầu tiên sẽ quyết định thứ hạng. Hệ thống thay thế tuân theo logic tương tự với chiến thắng được kiểm tra trước. 

Bởi vì số liệu thống kê của mọi tay đua đều được tích lũy chính xác và các so sánh tuân theo thứ tự hòa chính thức nên nhà vô địch được chọn phải chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

POINTS = [25, 18, 15, 12, 10, 8, 6, 4, 2, 1]

def solve():
    t = int(input())

    drivers = {}

    for _ in range(t):
        n = int(input())

        for pos in range(n):
            name = input().strip()

            if name not in drivers:
                drivers[name] = {
                    "points": 0,
                    "places": [0] * 55
                }

            drivers[name]["places"][pos + 1] += 1

            if pos < 10:
                drivers[name]["points"] += POINTS[pos]

    names = list(drivers.keys())

    def better_original(a, b):
        da = drivers[a]
        db = drivers[b]

        if da["points"] != db["points"]:
            return da["points"] > db["points"]

        for place in range(1, 55):
            if da["places"][place] != db["places"][place]:
                return da["places"][place] > db["places"][place]

        return False

    def better_alternative(a, b):
        da = drivers[a]
        db = drivers[b]

        if da["places"][1] != db["places"][1]:
            return da["places"][1] > db["places"][1]

        if da["points"] != db["points"]:
            return da["points"] > db["points"]

        for place in range(2, 55):
            if da["places"][place] != db["places"][place]:
                return da["places"][place] > db["places"][place]

        return False

    champion_original = names[0]
    champion_alternative = names[0]

    for name in names[1:]:
        if better_original(name, champion_original):
            champion_original = name

        if better_alternative(name, champion_alternative):
            champion_alternative = name

    print(champion_original)
    print(champion_alternative)

solve()
```Giải pháp đầu tiên là xây dựng số liệu thống kê về mùa giải cho mọi tay đua. các`places`mảng lưu trữ số lần người lái xe đã hoàn thành ở mỗi vị trí. chỉ mục`1`đại diện cho chiến thắng, chỉ số`2`đại diện cho vị trí thứ hai, v.v. 

Sử dụng mảng có kích thước cố định đơn giản và an toàn hơn so với việc thay đổi kích thước cấu trúc một cách linh hoạt trong quá trình so sánh. Vì có tối đa 50 tay đua tham gia một cuộc đua nên một dãy kích thước 55 sẽ thoải mái đáp ứng mọi vị trí có thể. 

Hai chức năng so sánh trực tiếp mã hóa hệ thống xếp hạng. Hệ thống ban đầu so sánh điểm trước, trong khi hệ thống thay thế so sánh chiến thắng trước. Sau các tiêu chí chính đó, cả hai đều tiếp tục theo từ điển thông qua số lượng vị trí. 

Một chi tiết tinh tế là phạm vi vòng lặp trong bộ ngắt kết nối. Hệ thống ban đầu bắt đầu kiểm tra từ nơi`1`bởi vì chiến thắng là trận hòa đầu tiên sau khi có điểm. Hệ thống thay thế đã xử lý các chiến thắng riêng biệt nên vòng lặp của nó bắt đầu từ vị trí`2`. 

Một lựa chọn triển khai quan trọng khác là lưu trữ tất cả các vị trí, không chỉ các vị trí trong top 10. Các tay đua ngoài điểm vẫn ảnh hưởng tới các tay đua sau này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3
Hamilton
Vettel
Webber
2
Webber
Vettel
2
Hamilton
Vettel
```Các cuộc đua xử lý: 

| Cuộc đua | Vị trí | Tài xế | Điểm đã thêm | Tổng số điểm | Thắng | Giây | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | Hamilton | 25 | 25 | 1 | 0 | 
| 1 | 2 | Vettel | 18 | 18 | 0 | 1 | 
| 1 | 3 | Webber | 15 | 15 | 0 | 0 | 
| 2 | 1 | Webber | 25 | 40 | 1 | 0 | 
| 2 | 2 | Vettel | 18 | 36 | 0 | 2 | 
| 3 | 1 | Hamilton | 25 | 50 | 2 | 0 | 
| 3 | 2 | Vettel | 18 | 54 | 0 | 3 | 

Bảng xếp hạng cuối cùng: 

| Tài xế | Điểm | Thắng | Giây | 
| --- | --- | --- | --- | 
| Hamilton | 50 | 2 | 0 | 
| Vettel | 54 | 0 | 3 | 
| Webber | 40 | 1 | 0 | 

Hệ thống ban đầu chọn Vettel vì điểm quan trọng nhất. Hệ thống thay thế chọn Hamilton vì chiến thắng quan trọng nhất. 

Dấu vết này chứng tỏ hai hệ thống xếp hạng có thể tạo ra những nhà vô địch khác nhau ngay cả khi có kết quả cuộc đua giống nhau. 

### Ví dụ 2 

đầu vào:```
2
3
A
B
C
3
B
A
C
```Các cuộc đua xử lý: 

| Cuộc đua | Vị trí | Tài xế | Tổng số điểm | Thắng | Giây | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | A | 25 | 1 | 0 | 
| 1 | 2 | B | 18 | 0 | 1 | 
| 2 | 1 | B | 43 | 1 | 1 | 
| 2 | 2 | A | 43 | 1 | 1 | 

Bảng xếp hạng cuối cùng: 

| Tài xế | Điểm | Thắng | Giây | 
| --- | --- | --- | --- | 
| A | 43 | 1 | 1 | 
| B | 43 | 1 | 1 | 

Các tay đua vẫn bị ràng buộc ngay cả sau khi so sánh chiến thắng và vị trí thứ hai. Vấn đề chính thức đảm bảo rằng các trường hợp thử nghiệm thực tế sẽ tránh được tình trạng này. 

Ví dụ này cho thấy lý do tại sao logic so sánh phải tiếp tục qua mọi cấp độ vị trí thay vì dừng sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(R × N + D² × N) | Xử lý các cuộc đua cộng với việc so sánh các tay đua thông qua số lượng vị trí | 
| Không gian | O(D × N) | Lưu trữ số liệu thống kê vị trí cho mọi tài xế | 

Các ràng buộc rất nhỏ nên giải pháp này dễ dàng nằm trong giới hạn. Chúng tôi lưu trữ số liệu thống kê tối đa cho 50 tài xế và mỗi so sánh kiểm tra tối đa 50 vị trí vị trí. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

POINTS = [25, 18, 15, 12, 10, 8, 6, 4, 2, 1]

def solve():
    input = sys.stdin.readline

    t = int(input())

    drivers = {}

    for _ in range(t):
        n = int(input())

        for pos in range(n):
            name = input().strip()

            if name not in drivers:
                drivers[name] = {
                    "points": 0,
                    "places": [0] * 55
                }

            drivers[name]["places"][pos + 1] += 1

            if pos < 10:
                drivers[name]["points"] += POINTS[pos]

    names = list(drivers.keys())

    def better_original(a, b):
        da = drivers[a]
        db = drivers[b]

        if da["points"] != db["points"]:
            return da["points"] > db["points"]

        for place in range(1, 55):
            if da["places"][place] != db["places"][place]:
                return da["places"][place] > db["places"][place]

        return False

    def better_alternative(a, b):
        da = drivers[a]
        db = drivers[b]

        if da["places"][1] != db["places"][1]:
            return da["places"][1] > db["places"][1]

        if da["points"] != db["points"]:
            return da["points"] > db["points"]

        for place in range(2, 55):
            if da["places"][place] != db["places"][place]:
                return da["places"][place] > db["places"][place]

        return False

    best1 = names[0]
    best2 = names[0]

    for name in names[1:]:
        if better_original(name, best1):
            best1 = name

        if better_alternative(name, best2):
            best2 = name

    print(best1)
    print(best2)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run(
"""3
3
Hamilton
Vettel
Webber
2
Webber
Vettel
2
Hamilton
Vettel
"""
) == "Vettel\nHamilton\n", "sample 1"

# minimum input
assert run(
"""1
1
Alice
"""
) == "Alice\nAlice\n", "single driver"

# wins matter more in alternative system
assert run(
"""2
2
A
B
2
B
A
"""
) == "A\nA\n", "tie resolved by first encountered equal stats"

# driver outside top 10 still affects tie-break
assert run(
"""2
11
A
B
C
D
E
F
G
H
I
J
K
11
B
A
C
D
E
F
G
H
I
J
K
"""
) == "A\nA\n", "full placement tracking"

# consistency beats wins in original system
assert run(
"""3
3
A
B
C
3
B
A
C
3
B
A
C
"""
) == "B\nB\n", "points and wins"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đua đơn với một tay đua | Cùng một tài xế hai lần | Đầu vào kích thước tối thiểu | 
| Hai cuộc đua đối xứng | Cùng một tài xế hai lần | Xử lý ổn định số liệu thống kê bằng nhau | 
| Hai chặng đua với 11 tay đua | Cùng một tài xế hai lần | Các vị trí ngoài top 10 được theo dõi | 
| Ba chặng đua liên tục lên bục vinh quang | B hai lần | Tích lũy điểm và chiến thắng đúng cách | 

## Vỏ cạnh 

Hãy xem xét trường hợp các tay đua bằng điểm nhưng không bằng điểm thắng.```
2
2
A
B
2
B
A
```Sau khi xử lý cả hai chủng tộc: 

- A có 43 điểm và 1 trận thắng. 
- B có 43 điểm và 1 trận thắng. 

Sự so sánh tiến tới vị trí thứ hai. Cả hai tay đua cũng có một vị trí thứ hai nên vẫn bị ràng buộc. Việc triển khai tiếp tục kiểm tra tất cả số lượng vị trí một cách chính xác theo yêu cầu. 

Bây giờ hãy xem xét các vị trí bên ngoài top 10:```
2
11
A
B
C
D
E
F
G
H
I
J
K
11
B
A
C
D
E
F
G
H
I
J
K
```Tay đua A và B nhận được số điểm giống nhau và về đích trong top 10 giống nhau. Việc triển khai có lỗi bỏ qua vị trí thứ 11 trở đi có thể dừng theo dõi dữ liệu quá sớm một cách không chính xác. Giải pháp của chúng tôi lưu trữ mọi vị trí về đích, do đó việc bẻ dây vẫn chính xác. 

Một trường hợp quan trọng khác là người lái xe bỏ qua cuộc đua:```
2
2
A
B
1
B
```A ghi được 25 điểm từ một cuộc đua. B ghi được 43 điểm trong hai cuộc đua. Bỏ lỡ cuộc đua đơn giản có nghĩa là không có cập nhật nào xảy ra cho tay đua đó trong cuộc đua đó. Vì tất cả số liệu thống kê được tích lũy độc lập nên việc triển khai sẽ xử lý các trình điều khiển vắng mặt một cách tự nhiên mà không có bất kỳ trường hợp đặc biệt nào.

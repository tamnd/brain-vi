---
title: "CF 19A - Cúp bóng đá thế giới"
description: "Chúng ta được cung cấp kết quả đầy đủ của một giải đấu bóng đá trong đó mỗi cặp đội thi đấu đúng một trận. Với mỗi trận đấu, chúng ta biết mỗi bên ghi được bao nhiêu bàn thắng. Sử dụng thể lệ của giải đấu, chúng ta phải xác định đội nào lọt vào vòng loại trực tiếp."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 19
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 19"
rating: 1400
weight: 19
solve_time_s: 351
verified: true
draft: false
---
[CF 19A - Cúp bóng đá thế giới](https://codeforces.com/problemset/problem/19/A) 

**Đánh giá:** 1400 
**Thẻ:** triển khai 
**Thời gian giải:** 5 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp kết quả đầy đủ của một giải đấu bóng đá trong đó mỗi cặp đội thi đấu đúng một trận. Với mỗi trận đấu, chúng ta biết mỗi bên ghi được bao nhiêu bàn thắng. Sử dụng thể lệ của giải đấu, chúng ta phải xác định đội nào lọt vào vòng loại trực tiếp. 

Mỗi đội kiếm được điểm từ các trận đấu. Trận thắng được 3 điểm, trận hòa được 1 điểm, trận thua được 0 điểm. Các đội được xếp hạng theo ba tiêu chí, được kiểm tra theo thứ tự: 

1. Tổng số điểm. 
2. Hiệu số bàn thắng bại, số bàn thắng ghi được trừ số bàn thua. 
3. Tổng số bàn thắng ghi được. 

Tuyên bố đảm bảo rằng các quy tắc này luôn tạo ra một trật tự nghiêm ngặt, do đó không có hai đội nào hoàn toàn hòa nhau sau khi áp dụng cả ba cách so sánh. 

Sau khi tính toán thứ hạng, top`n / 2`các đội đủ điều kiện. Đầu ra phải chứa các tên đội đủ điều kiện được sắp xếp theo từ điển chứ không phải theo thứ tự xếp hạng. 

Những hạn chế là rất nhỏ. Tối đa 50 đội tham gia, tức là số trận đấu nhiều nhất`50 * 49 / 2 = 1225`. Thậm chí một`O(n²)`hoặc`O(n² log n)`giải pháp dễ dàng đủ nhanh trong giới hạn 2 giây. Thách thức chính không phải là tối ưu hóa, mà là thực hiện chính xác các quy tắc xếp hạng và phân tích định dạng trận đấu một cách an toàn. 

Một lỗi phổ biến là xuất các từ hạn định theo thứ tự xếp hạng thay vì thứ tự bảng chữ cái. Hãy xem xét ví dụ này:```
4
Brazil
Argentina
Spain
Italy
Brazil-Argentina 1:0
Brazil-Spain 0:0
Brazil-Italy 2:0
Argentina-Spain 1:0
Argentina-Italy 1:0
Spain-Italy 3:0
```Giả sử thứ tự xếp hạng trở thành:```
Brazil
Spain
Argentina
Italy
```Các đội đủ điều kiện là Brazil và Tây Ban Nha, nhưng kết quả đầu ra yêu cầu là:```
Brazil
Spain
```Nếu vòng loại là Tây Ban Nha và Argentina, kết quả đầu ra chính xác vẫn cần sắp xếp theo thứ tự bảng chữ cái:```
Argentina
Spain
```Một lỗi dễ dàng khác xuất hiện khi cập nhật trực tiếp hiệu số bàn thắng bại thay vì theo dõi số bàn thắng ghi và thủng lưới riêng. Tưởng tượng:```
2
A
B
A-B 2:1
```Đội A có: 

- điểm = 3 
- ghi bàn = 2 
- thừa nhận = 1 
- chênh lệch = 1 

Nếu chỉ lưu hiệu số mà quên tổng số bàn thắng ghi được thì sau này chúng ta không thể áp dụng tỷ số hòa thứ ba. 

Phân tích cú pháp cũng đáng được chú ý vì tên chứa chữ hoa và chữ thường. Dòng trận đấu:```
RealMadrid-PSG 3:2
```phải được chia chính xác thành: 

- đội1 =`RealMadrid`- đội2 =`PSG`- điểm1 =`3`- điểm2 =`2`Chỉ sự phân chia không gian một cách bất cẩn sẽ làm mất đi cấu trúc của tên và điểm số. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng toàn bộ giải đấu đúng như mô tả. Đối với mỗi đội, hãy duy trì số liệu thống kê tích lũy của mình: 

- điểm 
- bàn thắng được ghi 
- bàn thua 

Sau đó xử lý từng trận đấu một. Cập nhật cả hai đội theo kết quả, sau đó sắp xếp tất cả các đội theo quy tắc xếp hạng. 

Phương pháp vũ phu này đã đủ nhanh vì có tối đa 1225 kết quả phù hợp. Việc xử lý mỗi trận đấu mất nhiều thời gian và việc sắp xếp 50 đội hầu như không tốn kém gì. Tổng công việc thoải mái dưới một triệu hoạt động. 

Một số người mới bắt đầu phức tạp hóa vấn đề này bằng cách cố gắng tính toán lại thứ hạng nhiều lần sau mỗi trận đấu hoặc so sánh các đội theo cặp nhiều lần. Điều đó vẫn hoạt động dưới những ràng buộc này, nhưng nó che khuất cấu trúc thực sự của vấn đề. 

Quan sát quan trọng là tiêu chí xếp hạng chỉ phụ thuộc vào số liệu thống kê tổng hợp. Sau khi tất cả các trận đấu được xử lý, mỗi đội sẽ được mô tả đầy đủ bởi: 

- tổng số điểm 
- hiệu số bàn thắng bại 
- bàn thắng được ghi 

Không có gì khác quan trọng. Chúng tôi không bao giờ cần lịch sử trận đấu cá nhân nữa. 

Điều đó cho phép chúng tôi giảm toàn bộ vấn đề xuống: 

1. Xây dựng số liệu thống kê từ tất cả các trò chơi. 
2. Sắp xếp các đội bằng bộ so sánh chính thức. 
3. Lấy cái đầu tiên`n / 2`. 
4. Xuất chúng theo thứ tự bảng chữ cái. 

Giải pháp chủ yếu là thực hiện cẩn thận hơn là khó khăn về mặt thuật toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với những so sánh lặp đi lặp lại | O(n³) | O(n) | Đã chấp nhận | 
| Tổng hợp tối ưu + sắp xếp đơn | O(n2 + n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đội và lưu trữ tên tất cả các đội. 

Chúng tôi cần tên cho cả số liệu thống kê lập chỉ mục và đầu ra cuối cùng. 
2. Tạo bảng thống kê cho từng đội. 

Đối với mỗi cửa hàng của nhóm: 

- điểm 
- bàn thắng được ghi 
- bàn thua 

Hiệu số bàn thắng bại luôn có thể được tính như sau:```
scored - conceded
```3. Xử lý từng dòng khớp. 

Mỗi dòng có dạng:```
team1-team2 x:y
```Tách dòng thành phần đội và phần tính điểm, sau đó trích xuất: 

-`team1`-`team2`-`x`-`y`4. Cập nhật bàn thắng cho cả hai đội. 

Nếu đội 1 ghi bàn`x`và thừa nhận`y`:

- thêm vào`x`ghi bàn 
- thêm`y`thừa nhận 

Thực hiện cập nhật đối xứng cho team2. 
5. Cập nhật điểm theo kết quả. 

Nếu như`x > y`, đội1 được 3 điểm. 

Nếu như`x < y`, đội2 được 3 điểm. 

Ngược lại cả hai đội sẽ được 1 điểm. 
6. Sau khi tất cả các trận đấu được xử lý, hãy sắp xếp các đội theo quy tắc xếp hạng. 

Bộ so sánh là: 

- điểm cao hơn trước 
- hiệu số bàn thắng bại cao hơn trước 
- bàn thắng cao hơn ghi trước 

Vì Python sắp xếp tăng dần theo mặc định nên chúng tôi sắp xếp bằng các giá trị âm. 
7. Chọn cái đầu tiên`n / 2`các đội từ bảng xếp hạng được sắp xếp. 

Đây là những đội đủ điều kiện. 
8. Sắp xếp tên các đội đủ điều kiện theo từ điển. 

Định dạng đầu ra yêu cầu thứ tự bảng chữ cái chứ không phải thứ tự xếp hạng. 
9. In tên mỗi dòng một. 

### Tại sao nó hoạt động 

Mọi tiêu chí xếp hạng chỉ phụ thuộc vào số liệu thống kê tích lũy của giải đấu. Xử lý từng trận đấu cập nhật những thống kê đó chính xác theo quy định tính điểm bóng đá. Sau khi tất cả các trận đấu được xử lý, giá trị được lưu trữ của mỗi đội sẽ khớp với thành tích giải đấu thực sự của đội đó. 

Sắp xếp theo: 

1. điểm, 
2. Hiệu số bàn thắng bại, 
3. bàn thắng được ghi, 

tái tạo chính xác định nghĩa bảng xếp hạng chính thức. Vì câu lệnh đảm bảo không có mối liên kết hoàn chỉnh nào nên thứ tự sắp xếp là duy nhất. Lựa chọn đầu tiên`n / 2`các đội đưa ra chính xác các vòng loại. Sắp xếp các tên đó theo thứ tự bảng chữ cái phù hợp với định dạng đầu ra được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    stats = {}

    for _ in range(n):
        name = input().strip()
        stats[name] = {
            "points": 0,
            "scored": 0,
            "conceded": 0
        }

    matches = n * (n - 1) // 2

    for _ in range(matches):
        line = input().strip()

        teams_part, score_part = line.split()
        team1, team2 = teams_part.split('-')

        g1, g2 = map(int, score_part.split(':'))

        stats[team1]["scored"] += g1
        stats[team1]["conceded"] += g2

        stats[team2]["scored"] += g2
        stats[team2]["conceded"] += g1

        if g1 > g2:
            stats[team1]["points"] += 3
        elif g1 < g2:
            stats[team2]["points"] += 3
        else:
            stats[team1]["points"] += 1
            stats[team2]["points"] += 1

    teams = list(stats.keys())

    teams.sort(
        key=lambda name: (
            -stats[name]["points"],
            -(stats[name]["scored"] - stats[name]["conceded"]),
            -stats[name]["scored"]
        )
    )

    qualified = teams[: n // 2]
    qualified.sort()

    print('\n'.join(qualified))

solve()
```Phần đầu tiên đọc tất cả tên các đội và khởi tạo một từ điển thống kê. Việc sử dụng từ điển được khóa theo tên nhóm sẽ tránh mọi vấn đề về lập chỉ mục và giúp việc cập nhật trở nên đơn giản. 

Mỗi dòng trận đấu được chia thành hai phần: 

-`"A-B"`-`"1:0"`Sau đó, mỗi phần được phân tích cú pháp độc lập. Điều này an toàn hơn việc cố gắng chia nhỏ mọi thứ cùng một lúc vì tên nhóm có thể chứa cả chữ hoa và chữ thường. 

Các bàn thắng ghi được và thủng lưới được cập nhật trước khi tính điểm. Việc ghi bàn và thủng lưới riêng biệt sẽ tránh được những sai sót trong tính toán tie-break. Sự khác biệt về mục tiêu chỉ có được trong quá trình sắp xếp. 

Phím sắp xếp trực tiếp mã hóa thể lệ giải đấu. Các giá trị âm đảo ngược thứ tự để số liệu thống kê lớn hơn được ưu tiên trước. 

Một chi tiết tinh tế là thứ tự bảng chữ cái cuối cùng của các đội đủ điều kiện. Thay vào đó, nhiều bài nộp sai sẽ in các nhóm theo thứ tự xếp hạng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
A
B
C
D
A-B 1:1
A-C 2:2
A-D 1:0
B-C 1:0
B-D 0:3
C-D 0:3
```### Dấu vết xử lý đối sánh 

| Trận đấu | Đội | Điểm | Ghi bàn | Bị thủng lưới | Sự khác biệt | 
| --- | --- | --- | --- | --- | --- | 
| A-B 1:1 | A | 1 | 1 | 1 | 0 | 
| A-B 1:1 | B | 1 | 1 | 1 | 0 | 
| A-C 2:2 | A | 2 | 3 | 3 | 0 | 
| A-C 2:2 | C | 1 | 2 | 2 | 0 | 
| Sau Công Nguyên 1:0 | A | 5 | 4 | 3 | 1 | 
| Sau Công Nguyên 1:0 | D | 0 | 0 | 1 | -1 | 
| B-C 1:0 | B | 4 | 2 | 1 | 1 | 
| B-C 1:0 | C | 1 | 2 | 3 | -1 | 
| B-D 0:3 | B | 4 | 2 | 4 | -2 | 
| B-D 0:3 | D | 3 | 3 | 1 | 2 | 
| C-D 0:3 | C | 1 | 2 | 6 | -4 | 
| C-D 0:3 | D | 6 | 6 | 1 | 5 | 

### Xếp hạng cuối cùng 

| Đội | Điểm | Sự khác biệt | Bàn thắng được ghi | 
| --- | --- | --- | --- | 
| D | 6 | 5 | 6 | 
| A | 5 | 1 | 4 | 
| B | 4 | -2 | 2 | 
| C | 1 | -4 | 2 | 

Hai đội dẫn đầu là`D`Và`A`. Sau khi sắp xếp theo thứ tự bảng chữ cái:```
A
D
```Ví dụ này xác nhận rằng thứ tự chất lượng và thứ tự đầu ra là các khái niệm khác nhau. 

### Ví dụ 2 

đầu vào:```
4
Alpha
Beta
Gamma
Delta
Alpha-Beta 1:0
Alpha-Gamma 0:2
Alpha-Delta 3:1
Beta-Gamma 1:1
Beta-Delta 2:0
Gamma-Delta 0:1
```### Dấu vết xử lý đối sánh 

| Đội | Điểm | Ghi bàn | Bị thủng lưới | Sự khác biệt | 
| --- | --- | --- | --- | --- | 
| Alpha | 6 | 4 | 3 | 1 | 
| Beta | 4 | 3 | 2 | 1 | 
| Gamma | 4 | 3 | 2 | 1 | 
| Đồng bằng | 3 | 2 | 5 | -3 | 

Beta và Gamma bằng điểm và hiệu số bàn thắng bại. Người hòa thứ ba, số bàn thắng được ghi, cũng bị ràng buộc ở đây, điều này sẽ vi phạm đảm bảo tuyên bố. Những đầu vào như vậy không bao giờ xuất hiện chính thức. 

Nếu chúng ta sửa đổi một chút:```
Gamma-Delta 1:1
```thì Gamma trở thành: 

| Đội | Điểm | Sự khác biệt | Bàn thắng được ghi | 
| --- | --- | --- | --- | 
| Gamma | 5 | 1 | 4 | 

Bây giờ thứ tự là duy nhất. 

Dấu vết này chứng tỏ tại sao cả ba số liệu thống kê phải được lưu trữ chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2 + n log n) | Việc xử lý tất cả các trận đấu mất O(n²), các đội sắp xếp mất O(n log n) | 
| Không gian | O(n) | Chỉ số liệu thống kê của đội được lưu trữ | 

Với tối đa 50 đội, số trận đấu nhiều nhất là 1225. Giải pháp chỉ thực hiện vài nghìn thao tác, thấp hơn nhiều so với thời hạn. Việc sử dụng bộ nhớ rất nhỏ vì chỉ cần một bản ghi thống kê cho mỗi nhóm. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n = int(input())

        stats = {}

        for _ in range(n):
            name = input().strip()
            stats[name] = {
                "points": 0,
                "scored": 0,
                "conceded": 0
            }

        matches = n * (n - 1) // 2

        for _ in range(matches):
            line = input().strip()

            teams_part, score_part = line.split()
            team1, team2 = teams_part.split('-')

            g1, g2 = map(int, score_part.split(':'))

            stats[team1]["scored"] += g1
            stats[team1]["conceded"] += g2

            stats[team2]["scored"] += g2
            stats[team2]["conceded"] += g1

            if g1 > g2:
                stats[team1]["points"] += 3
            elif g1 < g2:
                stats[team2]["points"] += 3
            else:
                stats[team1]["points"] += 1
                stats[team2]["points"] += 1

        teams = list(stats.keys())

        teams.sort(
            key=lambda name: (
                -stats[name]["points"],
                -(stats[name]["scored"] - stats[name]["conceded"]),
                -stats[name]["scored"]
            )
        )

        qualified = teams[: n // 2]
        qualified.sort()

        return '\n'.join(qualified)

    return solve()

# provided sample
assert run(
"""4
A
B
C
D
A-B 1:1
A-C 2:2
A-D 1:0
B-C 1:0
B-D 0:3
C-D 0:3
"""
) == "A\nD", "sample 1"

# minimum size
assert run(
"""2
X
Y
X-Y 5:0
"""
) == "X", "minimum teams"

# draw with goal difference tie
assert run(
"""4
A
B
C
D
A-B 1:0
A-C 0:1
A-D 2:0
B-C 0:0
B-D 3:0
C-D 2:0
"""
) == "A\nC", "tie-break by goals scored"

# alphabetical output check
assert run(
"""4
Zebra
Apple
Monkey
Bear
Zebra-Apple 0:1
Zebra-Monkey 0:1
Zebra-Bear 1:0
Apple-Monkey 1:0
Apple-Bear 1:0
Monkey-Bear 2:0
"""
) == "Apple\nMonkey", "alphabetical output"

# larger balanced case
assert run(
"""6
A
B
C
D
E
F
A-B 1:0
A-C 1:0
A-D 1:0
A-E 1:0
A-F 1:0
B-C 1:0
B-D 1:0
B-E 1:0
B-F 1:0
C-D 1:0
C-E 1:0
C-F 1:0
D-E 1:0
D-F 1:0
E-F 1:0
"""
) == "A\nB\nC", "strict ranking chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giải đấu 2 đội | X | Đầu vào hợp lệ tối thiểu | 
| Kịch bản hòa | Một C | Xếp hạng trung học và đại học đúng | 
| Đầu ra vòng loại theo bảng chữ cái | Khỉ táo | Thứ tự đầu ra khác với thứ tự xếp hạng | 
| Chuỗi nghiêm ngặt 6 đội | A B C | Tính toán thứ hạng lớn hơn | 

## Vỏ cạnh 

Hãy xem xét trường hợp thứ tự đầu ra khác với thứ tự xếp hạng:```
4
Z
A
M
B
Z-A 0:1
Z-M 0:1
Z-B 1:0
A-M 1:0
A-B 1:0
M-B 2:0
```Thứ hạng trở thành:```
A
M
Z
B
```Các vòng loại là`A`Và`M`. Thuật toán trước tiên trích xuất nửa trên theo thứ tự xếp hạng, sau đó chỉ sắp xếp những tên đó theo thứ tự bảng chữ cái trước khi in:```
A
M
```Điều này xử lý chính xác định dạng cần thiết. 

Bây giờ hãy xem xét tỷ số hòa được giải quyết bằng hiệu số bàn thắng bại:```
4
A
B
C
D
A-B 1:0
A-C 0:2
A-D 1:0
B-C 1:0
B-D 2:0
C-D 0:1
```Thống kê cuối cùng: 

| Đội | Điểm | Sự khác biệt | Bàn thắng | 
| --- | --- | --- | --- | 
| A | 6 | 0 | 2 | 
| B | 6 | 2 | 3 | 
| C | 3 | 1 | 2 | 
| D | 3 | -3 | 1 | 

Dù A và B bằng điểm nhau nhưng B vượt lên trên A nhờ hiệu số bàn thắng bại vượt trội. Vì thuật toán sắp xếp bằng cách sử dụng cả ba khóa xếp hạng theo thứ tự nên nó sẽ tái tạo chính xác thứ hạng chính thức. 

Cuối cùng, hãy xem xét giải đấu nhỏ nhất có thể:```
2
A
B
A-B 0:0
```Cả hai đội hòa và kiếm được một điểm. Hiệu số bàn thắng bại và số bàn thắng ghi được của họ cũng bằng nhau, điều này sẽ vi phạm sự đảm bảo về thứ tự duy nhất. Dữ liệu thử nghiệm chính thức tránh những trường hợp như vậy. Thuật toán vẫn xử lý đầu vào một cách nhất quán, nhưng vấn đề đảm bảo rằng chúng ta không bao giờ cần phải giải quyết các mối quan hệ không thể giải quyết được.

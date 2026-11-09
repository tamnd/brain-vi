---
title: "CF 149A - Đi công tác"
description: "Petya có thể quyết định anh ấy tưới hoa vào tháng nào. Mỗi tháng đóng góp một mức tăng trưởng cố định và việc bỏ qua một tháng chẳng đóng góp gì cả. Mục tiêu là đạt được tổng mức tăng trưởng ít nhất là k cm trong khi sử dụng càng ít tháng càng tốt."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 149
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 106 (Div. 2)"
rating: 900
weight: 149
solve_time_s: 93
verified: true
draft: false
---

[CF 149A - Chuyến công tác](https://codeforces.com/problemset/problem/149/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Petya có thể quyết định anh ấy tưới hoa vào tháng nào. Mỗi tháng đóng góp một mức tăng trưởng cố định và việc bỏ qua một tháng chẳng đóng góp gì cả. Mục tiêu là đạt được ít nhất`k`centimet của tổng mức tăng trưởng trong khi sử dụng càng ít tháng càng tốt. 

Đầu vào mang lại mức tăng trưởng cần thiết`k`, theo sau là 12 số nguyên. các`i`-số nguyên thứ cho chúng ta biết bông hoa sẽ phát triển bao nhiêu nếu Petya tưới nước trong tháng`i`. 

Sản lượng là số tháng tối thiểu cần thiết để đạt được ít nhất`k`tăng trưởng tổng thể. Nếu tưới nước suốt 12 tháng vẫn không đủ thì câu trả lời là`-1`. 

Những hạn chế là rất nhỏ. Luôn luôn có chính xác 12 tháng, vì vậy về mặt kỹ thuật, ngay cả những phương pháp tiếp cận theo cấp số nhân cũng có thể thực hiện được. Một giải pháp mạnh mẽ trên tất cả các tập hợp con sẽ kiểm tra`2^12 = 4096`kết hợp, vẫn hoàn toàn ổn trong vòng 2 giây. Tuy nhiên, vấn đề được thiết kế để dạy cách quan sát tham lam và giải pháp rõ ràng nhất sẽ diễn ra trong thời gian cơ bản không đổi. 

Những trường hợp phức tạp không phải về hiệu suất mà là về việc xử lý các tình huống đặc biệt một cách chính xác. 

Một trường hợp quan trọng là khi`k = 0`. 

đầu vào:```
0
0 0 0 0 0 0 0 0 0 0 0 0
```Đầu ra đúng:```
0
```Petya đã đáp ứng được yêu cầu mà không cần tưới nước chút nào. Việc thực hiện bất cẩn vẫn có thể kéo dài nhiều tháng một cách không cần thiết. 

Một trường hợp khó khăn khác là khi tổng mức tăng trưởng hàng năm không đủ. 

đầu vào:```
15
1 1 1 1 1 1 1 1 1 1 1 1
```Đầu ra đúng:```
-1
```Ngay cả việc sử dụng hàng tháng cũng chỉ mang lại mức tăng trưởng tổng thể`12`. Việc quên kiểm tra này có thể tạo ra một câu trả lời tích cực không chính xác. 

Một trường hợp tinh tế hơn xảy ra khi một vài tháng không có mức tăng trưởng nào. 

đầu vào:```
5
5 0 0 0 0 0 0 0 0 0 0 0
```Đầu ra đúng:```
1
```Nếu quá trình triển khai xử lý các tháng theo thứ tự tùy ý thay vì ưu tiên những lợi ích lớn nhất trước tiên, thì nó có thể tính những tháng tăng trưởng bằng 0 vô ích trước khi đạt được mục tiêu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử từng nhóm nhỏ trong 12 tháng. Đối với mỗi tập hợp con, chúng tôi tính toán tổng mức tăng trưởng và đếm số tháng đã chọn mà nó sử dụng. Trong số tất cả các tập hợp con có tổng mức tăng trưởng ít nhất là`k`, chúng tôi chọn số lượng tối thiểu. 

Điều này hiệu quả vì chỉ có 12 tháng. Tổng số tập hợp con là`2^12 = 4096`và đối với mỗi tập hợp con, chúng tôi có thể kiểm tra cả 12 tháng, đưa ra khoảng 50.000 phép tính. 

Điểm yếu của vũ lực là nó bỏ qua cấu trúc của vấn đề. Chúng tôi thực sự không quan tâm đến tháng cụ thể nào được chọn. Chúng tôi chỉ quan tâm đến giá trị tăng trưởng mà những tháng đó đóng góp. 

Quan sát quan trọng rất đơn giản: nếu muốn giảm thiểu số tháng đã chọn, trước tiên chúng ta phải luôn lấy giá trị tăng trưởng sẵn có lớn nhất. 

Giả sử chúng ta chọn một tháng nhỏ hơn trong khi không sử dụng tháng lớn hơn. Việc thay thế cái nhỏ hơn bằng cái lớn hơn chỉ có thể làm tăng tổng mức tăng trưởng mà không làm tăng số tháng. Do đó, bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi thành giải pháp sử dụng số tháng sẵn có lớn nhất trước tiên. 

Điều đó ngay lập tức gợi ý một chiến lược tham lam: 

1. Sắp xếp các giá trị tăng trưởng hàng tháng theo thứ tự giảm dần. 
2. Tiếp tục dùng tháng lớn nhất tiếp theo cho đến khi mức tăng trưởng tích lũy đạt ít nhất`k`. 
3. Nếu đã sử dụng hết các tháng mà vẫn không đạt được mục tiêu, hãy in`-1`. 

Chỉ với 12 con số, việc sắp xếp là chuyện nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^12 × 12) | O(1) | Đã chấp nhận | 
| Tối ưu | O(12 log 12) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mức tăng trưởng mục tiêu`k`và giá trị tăng trưởng hàng tháng của 12. 
2. Xử lý trường hợp đặc biệt`k = 0`. 

Nếu không cần tăng trưởng thì Petya hoàn toàn không cần tưới nước cho hoa nên câu trả lời là`0`. 
3. Sắp xếp 12 giá trị tăng trưởng theo thứ tự giảm dần. 

Những tháng tăng trưởng lớn nhất nên được xem xét đầu tiên vì chúng giúp chúng ta đạt được mục tiêu với ít lựa chọn nhất. 
4. Khởi tạo hai biến:`total = 0`cho sự tăng trưởng tích lũy.`months = 0`cho số tháng đã chọn. 
5. Lặp lại danh sách đã sắp xếp. 

Cộng mức tăng trưởng của tháng hiện tại vào`total`và tăng dần`months`. 
6. Sau mỗi lần thêm, kiểm tra xem`total >= k`. 

Ngay khi điều này trở thành sự thật, hãy in`months`và dừng lại. Vì chúng tôi đã xử lý các tháng từ lớn nhất đến nhỏ nhất nên đây là số tháng tối thiểu có thể có. 
7. Nếu vòng lặp kết thúc và không đạt được mục tiêu, hãy in`-1`. 

Ngay cả việc sử dụng hàng tháng cũng không đủ. 

### Tại sao nó hoạt động 

Sự lựa chọn tham lam luôn an toàn vì giá trị tăng trưởng hàng tháng lớn hơn sẽ lấn át giá trị tăng trưởng nhỏ hơn. Nếu một giải pháp sử dụng giá trị nhỏ hơn trong khi loại trừ giá trị lớn hơn, việc hoán đổi chúng sẽ không gây hại gì và có thể cải thiện mức tăng trưởng tổng thể. Việc lặp lại lập luận này sẽ biến bất kỳ giải pháp tối ưu nào thành giải pháp bao gồm các tháng có sẵn lớn nhất theo thứ tự giảm dần. 

Do thuộc tính đó, khi chúng ta sắp xếp các giá trị giảm dần, tiền tố đầu tiên có tổng đạt`k`được đảm bảo sử dụng số tháng tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input())
    months = list(map(int, input().split()))

    if k == 0:
        print(0)
        return

    months.sort(reverse=True)

    total = 0

    for i in range(12):
        total += months[i]

        if total >= k:
            print(i + 1)
            return

    print(-1)

solve()
```Trường hợp đặc biệt đầu tiên kiểm tra xem`k`đã bằng không rồi. Điều này tránh việc vô tình chọn những tháng không cần tăng trưởng. 

Danh sách được sắp xếp theo thứ tự giảm dần vì chiến lược tham lam phụ thuộc vào việc luôn lấy mức tăng trưởng lớn nhất còn lại trước. 

Vòng lặp tích lũy tăng trưởng mỗi tháng một lần. Câu trả lời là`i + 1`bởi vì`i`được lập chỉ mục bằng 0 trong khi số tháng được chọn bắt đầu từ một. 

trận chung kết`-1`chỉ đạt được nếu sử dụng hết 12 tháng và mức tăng trưởng tích lũy vẫn còn quá nhỏ. 

Không cần thêm cấu trúc dữ liệu. Quá trình triển khai diễn ra trong thời gian ngắn vì logic tham lam phù hợp trực tiếp với bằng chứng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 1 1 1 2 2 3 2 2 1 1 1
```Sau khi sắp xếp:```
3 2 2 2 2 1 1 1 1 1 1 1
```| Bước | Tăng trưởng được chọn | Tăng trưởng tổng thể | Tháng sử dụng | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 1 | 
| 2 | 2 | 5 | 2 | 

Ở bước 2, tổng mức tăng trưởng đạt`5`, vậy câu trả lời là`2`. 

Dấu vết này thể hiện sự bất biến tham lam. Sau khi chọn hai tháng có sẵn lớn nhất, không có cặp tháng nào khác có thể tạo ra tổng mức tăng trưởng lớn hơn. 

### Mẫu 2 

đầu vào:```
0
1 2 3 4 5 6 7 8 9 10 11 12
```| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Kiểm tra`k == 0`| Đúng | 
| 2 | In câu trả lời | 0 | 

Thuật toán thoát ngay lập tức mà không cần sắp xếp hoặc chọn tháng. 

Ví dụ này xác nhận rằng trường hợp đặc biệt là cần thiết. Tưới nước bất kỳ tháng nào sẽ là công việc không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(12 log 12) | Sắp xếp 12 số nguyên thống trị thời gian chạy | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Kích thước đầu vào được cố định ở mức 12 tháng, do đó giải pháp chạy hiệu quả trong thời gian không đổi. Cả thời gian chạy và mức sử dụng bộ nhớ đều thấp hơn nhiều so với giới hạn của vấn đề. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        k = int(input())
        months = list(map(int, input().split()))

        if k == 0:
            return "0"

        months.sort(reverse=True)

        total = 0

        for i in range(12):
            total += months[i]

            if total >= k:
                return str(i + 1)

        return "-1"

    return solve()

# provided sample
assert run(
    "5\n1 1 1 1 2 2 3 2 2 1 1 1\n"
) == "2", "sample 1"

# k = 0
assert run(
    "0\n0 0 0 0 0 0 0 0 0 0 0 0\n"
) == "0", "no growth needed"

# impossible case
assert run(
    "15\n1 1 1 1 1 1 1 1 1 1 1 1\n"
) == "-1", "total yearly growth insufficient"

# single month enough
assert run(
    "5\n5 0 0 0 0 0 0 0 0 0 0 0\n"
) == "1", "largest month alone works"

# all equal values
assert run(
    "6\n1 1 1 1 1 1 1 1 1 1 1 1\n"
) == "6", "needs exact number of equal months"

# maximum useful boundary
assert run(
    "100\n100 0 0 0 0 0 0 0 0 0 0 0\n"
) == "1", "largest possible single contribution"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`k = 0`|`0`| Không cần tháng | 
| Tổng số tiền nhỏ hơn`k`|`-1`| Kịch bản không thể xảy ra | 
| Một tháng lớn và nhiều số không |`1`| Tham lam chọn lớn nhất trước | 
| Tất cả các giá trị bằng nhau | Số lượng chính xác | Logic tích lũy đúng | 
| Một tháng có giá trị 100 |`1`| Giá trị tăng trưởng biên | 

## Vỏ cạnh 

Hãy xem xét trường hợp không cần tăng trưởng. 

đầu vào:```
0
3 4 5 6 7 8 9 1 2 3 4 5
```Thuật toán kiểm tra ngay lập tức`k == 0`và in`0`. Không có sự phân loại hoặc tích lũy nào xảy ra. Điều này đúng vì Petya đã đáp ứng được yêu cầu không cần tưới nước chút nào. 

Bây giờ hãy xem xét một trường hợp không thể xảy ra. 

đầu vào:```
20
1 1 1 1 1 1 1 1 1 1 1 1
```Sau khi sắp xếp, mảng không thay đổi. Thuật toán tích lũy các giá trị:```
1, 2, 3, ..., 12
```Thậm chí sau tất cả 12 tháng, tổng số chỉ là`12`, vẫn ở bên dưới`20`. Vòng lặp kết thúc và thuật toán được in ra`-1`. 

Cuối cùng, hãy xem xét nhiều tháng tăng trưởng bằng 0. 

đầu vào:```
5
5 0 0 0 0 0 0 0 0 0 0 0
```Sau khi sắp xếp:```
5 0 0 0 0 0 0 0 0 0 0 0
```Tháng được chọn đầu tiên đã đạt được mục tiêu:```
total = 5
```Thuật toán in`1`. 

Điều này xác nhận tại sao việc sắp xếp giảm dần lại quan trọng. Việc chọn các tháng theo thứ tự tùy ý có thể lãng phí các lựa chọn vào những tháng tăng trưởng bằng 0 trước khi đạt được mục tiêu.

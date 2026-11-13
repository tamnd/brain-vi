---
title: "CF 174A - Bài toán về phương trình"
description: "Chúng tôi có một số cốc đã chứa lượng Ber-Cola khác nhau. Ngoài ra còn có một ít đồ uống còn sót lại trong chai. Chúng ta phải đổ toàn bộ số tiền còn lại vào các cốc sao cho mỗi cốc đều có thể tích bằng nhau. Nếu số tiền ban đầu là a1, a2, ..."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 174
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2012 Round 3 (Unofficial Div. 2 Edition)"
rating: 1100
weight: 174
solve_time_s: 93
verified: true
draft: false
---

[CF 174A - Vấn đề về phương trình](https://codeforces.com/problemset/problem/174/A) 

**Đánh giá:** 1100 
**Thẻ:** toán 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số cốc đã chứa lượng Ber-Cola khác nhau. Ngoài ra còn có một ít đồ uống còn sót lại trong chai. Chúng ta phải đổ toàn bộ số tiền còn lại vào các cốc sao cho mỗi cốc đều có thể tích bằng nhau. 

Nếu số tiền ban đầu là`a1, a2, ..., an`và chai chứa`b`, chúng ta cần xác định lượng đồ uống bổ sung vào mỗi cốc là bao nhiêu. Số tiền được thêm vào phải thỏa mãn đồng thời hai điều kiện. Đầu tiên, tổng số tiền thêm vào phải bằng`b`. Thứ hai, sau khi rót, tất cả các cốc phải chứa cùng một thể tích cuối cùng. 

Những hạn chế là cực kỳ nhỏ. Có nhiều nhất là 100 cốc và mỗi giá trị nhiều nhất là 100. Ngay cả thuật toán bậc hai hoặc bậc ba cũng dễ dàng nằm trong giới hạn. Thách thức không phải là hiệu suất, mà là việc nhận ra điều kiện toán học quyết định liệu giải pháp có tồn tại hay không. 

Quan sát quan trọng là mọi cốc phải kết thúc ở một giá trị chung nào đó`x`. cho cốc`i`, số tiền thêm vào là:$$c_i = x - a_i$$Tổng số tiền thêm vào phải bằng`b`, Vì thế:$$\sum (x - a_i) = b$$Việc mở rộng điều này mang lại:$$n \cdot x - \sum a_i = b$$xác định ngay giá trị cuối cùng duy nhất có thể có:$$x = \frac{\sum a_i + b}{n}$$Một lần`x`được biết, mọi`c_i`đã được sửa. 

Trường hợp nguy hiểm là khi một số cốc đã chứa nhiều hơn giá trị mục tiêu. Trong tình huống đó, tương ứng`c_i`trở thành âm, điều này là không thể vì chúng ta chỉ được phép rót đồ uống vào cốc chứ không được lấy ra. 

Hãy xem xét ví dụ này:```
2 1
5 1
```Tổng số tiền sau khi đổ sẽ là`7`, do đó mục tiêu trở thành`3.5`. Cốc đầu tiên đã chứa`5`, lớn hơn`3.5`. Để đạt được khối lượng bằng nhau sẽ yêu cầu loại bỏ`1.5`từ chiếc cốc đó. Đầu ra đúng là:```
-1
```Việc thực hiện bất cẩn có thể tính toán công thức một cách máy móc và in ra các giá trị âm. 

Một trường hợp tinh vi khác xảy ra khi tất cả các cốc đều bằng nhau.```
3 6
2 2 2
```Mục tiêu trở thành`4`, vì vậy mỗi cốc chỉ đơn giản nhận được`2`. Đầu ra là:```
2.000000
2.000000
2.000000
```Việc triển khai giả định không chính xác các giá trị bắt đầu không bằng nhau có thể xử lý sai tình huống này. 

Trường hợp cạnh thứ ba là khi mục tiêu trở thành phân số.```
2 1
1 1
```Số tiền cuối cùng trong mỗi cốc là`1.5`, vậy mỗi cốc nhận được`0.5`. Đầu ra dấu phẩy động được yêu cầu ở đây. 

## Phương pháp tiếp cận 

Tư duy vũ phu sẽ bắt đầu bằng việc đoán số tiền chung cuối cùng`x`. Đối với mỗi ứng viên`x`, chúng ta có thể tính xem mỗi cốc cần bao nhiêu và kiểm tra xem tổng số tiền thêm vào có bằng không`b`. 

Vì tất cả các giá trị đều nhỏ nên chúng ta thậm chí có thể thử nhiều giá trị thực có thể có với độ chính xác nhất định. Cách tiếp cận này đúng về mặt logic bởi vì bất kỳ giải pháp hợp lệ nào cũng phải tương ứng với một số tập chung cuối cùng. Vấn đề là việc tìm kiếm trên số thực là khó khăn và không cần thiết. Ngay cả việc rời rạc hóa việc tìm kiếm cũng gây ra những lo ngại về độ chính xác và công việc làm thêm vô nghĩa. 

Cấu trúc của các phương trình mang lại điều gì đó mạnh mẽ hơn nhiều. Mỗi cốc phải có cùng một giá trị, có nghĩa là tất cả số tiền được thêm vào đều được liên kết trực tiếp với một biến`x`. Tổng hợp các phương trình thu gọn toàn bộ hệ thống thành một công thức duy nhất:$$x = \frac{\sum a_i + b}{n}$$Không cần phải tìm kiếm. Giá trị mục tiêu được xác định duy nhất bằng cách bảo toàn tổng chất lỏng. 

Một khi chúng ta biết`x`, mỗi câu trả lời chỉ đơn giản là:$$c_i = x - a_i$$Kiểm tra tính hợp lệ duy nhất còn lại là liệu có`c_i`là tiêu cực. Nếu thậm chí một giá trị âm thì không thể cấu hình được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không xác định cho tìm kiếm có giá trị thực | O(1) | Không thực tế | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`b`và mảng khối lượng cốc. 
2. Tính tổng lượng chất lỏng còn lại sau khi rót:$$total = \sum a_i + b$$3. Tính thể tích bằng nhau cuối cùng của mỗi cốc:$$x = \frac{total}{n}$$Giá trị này bị ép buộc bởi sự bảo toàn chất lỏng. Không có mục tiêu nào khác có thể đáp ứng các điều kiện. 
4. Với mỗi cốc, hãy tính:$$c_i = x - a_i$$Đây chính xác là số tiền phải được thêm vào để đạt được giá trị chung cuối cùng. 
5. Nếu có`c_i`là âm, in`-1`và dừng lại. 

Giá trị âm có nghĩa là cốc đã chứa nhiều hơn lượng mục tiêu, do đó việc cân bằng sẽ yêu cầu loại bỏ chất lỏng. 
6. Nếu không, hãy in mọi`c_i`có ít nhất sáu chữ số sau dấu thập phân. 

### Tại sao nó hoạt động 

Thuật toán dựa trên định luật bảo toàn. Tổng lượng chất lỏng sau khi rót là cố định, vì vậy nếu tất cả các cốc đều có cùng một lượng chất lỏng`x`, thì tổng chất lỏng phải bằng`n * x`. Điều đó xác định duy nhất`x`. 

Đối với mỗi cốc, số tiền bổ sung duy nhất có thể là`x - a_i`. Không có phép gán thay thế nào có thể tồn tại vì việc thay đổi số tiền thêm vào của một cốc sẽ làm thay đổi tập cuối cùng của nó khỏi`x`. 

Nếu mọi giá trị được tính toán đều không âm thì tất cả các cốc thực sự có thể được tăng lên chính xác`x`và tổng số tiền được thêm tự động bằng`b`bởi vì làm thế nào`x`đã được bắt nguồn. Nếu bất kỳ giá trị nào là âm thì không tồn tại nghiệm hợp lệ vì việc rót không thể làm giảm chất lỏng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, b = map(int, input().split())
    a = list(map(int, input().split()))

    target = (sum(a) + b) / n

    ans = []

    for value in a:
        add = target - value

        if add < 0:
            print(-1)
            return

        ans.append(add)

    for x in ans:
        print(f"{x:.6f}")

solve()
```Phần đầu tiên đọc đầu vào và tính toán khối lượng mục tiêu cuối cùng. Giá trị này được lấy trực tiếp từ tổng lượng chất lỏng có sẵn sau khi cạn chai. 

Vòng lặp tính toán lượng phải thêm vào mỗi cốc. điều kiện`add < 0`là toàn bộ bài kiểm tra tính khả thi. Nếu cốc đã vượt quá mức mục tiêu, chúng tôi không thể khắc phục tình trạng này vì thao tác chỉ cho phép thêm chất lỏng. 

Các câu trả lời được lưu trữ và in sau đó. In bằng`:.6f`đảm bảo ít nhất sáu chữ số sau dấu thập phân, đáp ứng yêu cầu đầu ra. 

Một chi tiết tinh tế là sử dụng phép chia dấu phẩy động khi tính toán mục tiêu. Việc sử dụng phép chia số nguyên sẽ cắt bớt các câu trả lời phân số và tạo ra kết quả không chính xác trong các trường hợp như`2 1`với cốc`1 1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 50
1 2 3 4 5
```Tổng lượng chất lỏng sau khi đổ là:$$1 + 2 + 3 + 4 + 5 + 50 = 65$$Số tiền mục tiêu cho mỗi cốc là:$$65 / 5 = 13$$| Cốc | Ban đầu | Mục tiêu | Đã thêm | 
| --- | --- | --- | --- | 
| 1 | 1 | 13 | 12 | 
| 2 | 2 | 13 | 11 | 
| 3 | 3 | 13 | 10 | 
| 4 | 4 | 13 | 9 | 
| 5 | 5 | 13 | 8 | 

Đầu ra:```
12.000000
11.000000
10.000000
9.000000
8.000000
```Dấu vết này chứng tỏ rằng số tiền được thêm vào sẽ tự động tổng hợp chính xác`50`, phù hợp với thể tích chai. 

### Ví dụ 2 

đầu vào:```
2 1
5 1
```Tổng lượng chất lỏng sau khi đổ là:$$5 + 1 + 1 = 7$$Số tiền mục tiêu cho mỗi cốc là:$$7 / 2 = 3.5$$| Cốc | Ban đầu | Mục tiêu | Đã thêm | 
| --- | --- | --- | --- | 
| 1 | 5 | 3,5 | -1,5 | 
| 2 | 1 | 3,5 | 2,5 | 

Cốc đầu tiên yêu cầu số tiền thêm âm, điều này là không thể. 

Đầu ra:```
-1
```Ví dụ này xác nhận rằng tính khả thi phụ thuộc hoàn toàn vào việc liệu tất cả các phép cộng được tính có phải là không âm hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét mảng một lần để tính tổng và một lần để xây dựng câu trả lời | 
| Không gian | O(1) | Ngoài danh sách đầu ra, chỉ có một số biến được sử dụng | 

Với tối đa 100 cốc, giải pháp sẽ diễn ra ngay lập tức. Việc sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, b = map(int, input().split())
    a = list(map(int, input().split()))

    target = (sum(a) + b) / n

    ans = []

    for value in a:
        add = target - value

        if add < 0:
            print(-1)
            return

        ans.append(add)

    for x in ans:
        print(f"{x:.6f}")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue()

# provided sample
assert run(
    "5 50\n1 2 3 4 5\n"
) == (
    "12.000000\n"
    "11.000000\n"
    "10.000000\n"
    "9.000000\n"
    "8.000000\n"
), "sample 1"

# minimum size with fractional answer
assert run(
    "2 1\n1 1\n"
) == (
    "0.500000\n"
    "0.500000\n"
), "fractional target"

# impossible case
assert run(
    "2 1\n5 1\n"
) == "-1\n", "negative addition required"

# all equal initially
assert run(
    "3 6\n2 2 2\n"
) == (
    "2.000000\n"
    "2.000000\n"
    "2.000000\n"
), "all equal"

# boundary-style large values
assert run(
    "2 100\n0 100\n"
) == (
    "100.000000\n"
    "0.000000\n"
), "one mug already at target"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 1`|`0.500000 0.500000`| Mục tiêu phân số | 
|`2 1 / 5 1`|`-1`| Cấu hình không thể | 
|`3 6 / 2 2 2`| Bổ sung bằng nhau | Cốc đã cân bằng | 
|`2 100 / 0 100`|`100`Và`0`| Ranh giới trong đó một phép cộng chính xác bằng 0 | 

## Vỏ cạnh 

Hãy xem xét cấu hình không thể:```
2 1
5 1
```Thuật toán tính toán:$$target = (5 + 1 + 1) / 2 = 3.5$$Đối với cốc đầu tiên:$$3.5 - 5 = -1.5$$Kết quả âm tính ngay lập tức gây ra`-1`. Điều này đúng vì việc cân bằng sẽ yêu cầu loại bỏ chất lỏng khỏi cốc đầu tiên. 

Bây giờ hãy xem xét một mục tiêu phân số:```
2 1
1 1
```Mục tiêu trở thành:$$(1 + 1 + 1) / 2 = 1.5$$Cả hai cốc đều nhận được`0.5`. Thuật toán sử dụng phép chia dấu phẩy động, do đó độ chính xác không bị mất khi cắt bớt số nguyên. 

Cuối cùng, hãy xem xét trường hợp một cốc đã phù hợp với mục tiêu:```
2 100
0 100
```Mục tiêu trở thành:$$(0 + 100 + 100) / 2 = 100$$Các bổ sung là:$$100 - 0 = 100$$Và$$100 - 100 = 0$$Được phép sử dụng 0 vì chúng tôi không bắt buộc phải đổ vào từng cốc mà chỉ phân phát toàn bộ chai với thể tích bằng nhau.

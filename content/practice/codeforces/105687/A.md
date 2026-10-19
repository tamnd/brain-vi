---
title: "CF 105687A - Đá Phạt"
description: "Chúng ta có hai đội bóng đá, mỗi đội có n cầu thủ. Đội A sút vào thủ môn của Đội B, người có chiều cao k. Đội B sút vào thủ môn của Đội A, có chiều cao m. Một cầu thủ ghi bàn nếu lực sút của họ cao ít nhất bằng một nửa chiều cao của thủ môn đối phương."
date: "2026-06-25T06:12:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105687
codeforces_index: "A"
codeforces_contest_name: "AlgoChief Sprint Round 2"
rating: 0
weight: 105687
solve_time_s: 43
verified: true
draft: false
---

[CF 105687A - Quả đá phạt đền](https://codeforces.com/problemset/problem/105687/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai đội bóng đá, mỗi đội có`n`người chơi. Đội A sút vào thủ môn của Đội B, người có chiều cao bằng`k`. Đội B sút vào thủ môn của Đội A, người có chiều cao bằng`m`. 

Một cầu thủ ghi bàn nếu lực sút của họ cao ít nhất bằng một nửa chiều cao của thủ môn đối phương. Việc so sánh được thực hiện trực tiếp với`height / 2`, vậy nên một người chơi có quyền lực`x`ghi điểm khi`2 * x >= height`. 

Người chiến thắng được quyết định trong hai giai đoạn. Đầu tiên hãy so sánh tổng số bàn thắng mà mỗi đội ghi được. Nếu một đội ghi được nhiều bàn thắng hơn thì đội đó sẽ thắng. Nếu cả hai đội ghi được số bàn thắng bằng nhau, hãy so sánh sức mạnh sút tối đa của tất cả các cầu thủ mỗi đội. Đội nào có sức mạnh tối đa lớn hơn sẽ chiến thắng. 

Alice dự đoán Đội A sẽ thắng. Bob dự đoán Đội B sẽ thắng. Chúng ta phải xuất`"Alice"`nếu Đội A thắng theo quy tắc trên, nếu không thì xuất ra`"Bob"`. 

Các giới hạn là rất nhỏ. Có tối đa 500 trường hợp thử nghiệm và mỗi đội có tối đa 100 người chơi. Ngay cả việc quét đơn giản tất cả người chơi cũng chỉ là vài chục nghìn thao tác. Không cần thuật toán nâng cao. 

Một điểm tinh tế là điều kiện tính điểm. Tuyên bố cho biết một cầu thủ ghi bàn khi sức mạnh của họ lớn hơn hoặc bằng một nửa chiều cao của thủ môn. Nếu chiều cao của thủ môn là số lẻ thì một nửa không phải là số nguyên. 

Coi như:```
1
1 5 3
2
1
```Đội A sút ngược chiều cao`3`. Từ`2 >= 1.5`, Đội A ghi điểm. 

Việc thực hiện bất cẩn bằng cách sử dụng phép chia số nguyên sẽ kiểm tra`2 >= 3 // 2 = 1`, điều này xảy ra ở đây nhưng không thành công trong các trường hợp khác. 

Ví dụ:```
1
1 5 5
2
1
```Ngưỡng đúng là`2.5`, vậy sức mạnh`2`nên bỏ lỡ. sử dụng`5 // 2 = 2`sẽ tính sai đó là một bàn thắng. 

Một trường hợp khác xảy ra khi số bàn thắng bằng nhau.```
1
2 4 4
2 10
3 9
```Hai đội ghi được số bàn thắng như nhau. Sức mạnh tối đa của đội A là`10`, Đội B là`9`, vậy là đội A thắng. Một giải pháp chỉ so sánh số lượng mục tiêu sẽ tạo ra câu trả lời sai. 

Tuyên bố đảm bảo rằng tất cả các sức mạnh sút là khác nhau, vì vậy nếu số bàn thắng bằng nhau thì sức mạnh tối đa không thể bằng nhau. Trận hòa luôn tạo ra một người chiến thắng duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng chính xác những gì các quy tắc mô tả. 

Đối với Đội A, đếm xem có bao nhiêu người chơi hài lòng`2 * a[i] >= k`, bởi vì họ đang bắn vào thủ môn có chiều cao của đội B`k`. 

Đối với Đội B, đếm xem có bao nhiêu người chơi hài lòng`2 * b[i] >= m`, bởi vì họ đang bắn vào thủ môn có chiều cao của đội A`m`. 

Sau khi có được cả hai mục tiêu, hãy so sánh chúng. Nếu Đội A có nhiều bàn thắng hơn thì Alice đúng. Nếu Đội B có nhiều bàn thắng hơn thì Bob đúng. Khi số điểm bằng nhau, so sánh sức mạnh bắn tối đa của hai đội và tuyên bố đội có sức bắn tối đa lớn hơn là đội chiến thắng. 

Vì mỗi người chơi được kiểm tra một lần nên thời gian chạy là tuyến tính theo`n`. Với những hạn chế, điều này là quá đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Trong vấn đề này, mô phỏng lực lượng vũ phu đã là tối ưu vì bản thân các quy tắc yêu cầu kiểm tra mọi người chơi ít nhất một lần. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc`n`,`m`, Và`k`. 
3. Đọc sức mạnh bắn súng của đội A và sức mạnh bắn súng của đội B. 
4. Đếm số bàn thắng của Đội A bằng cách kiểm tra từng giá trị`a[i]`. Bàn thắng được ghi khi`2 * a[i] >= k`. 
5. Đếm mục tiêu của Đội B bằng cách kiểm tra từng giá trị`b[i]`. Bàn thắng được ghi khi`2 * b[i] >= m`. 
6. Nếu số bàn thắng của Đội A lớn hơn, hãy in`"Alice"`. 
7. Nếu số bàn thắng của Đội B lớn hơn, hãy in`"Bob"`. 
8. Nếu số bàn thắng bằng nhau, hãy tính`max(a)`Và`max(b)`. Nếu số điểm tối đa của Đội A lớn hơn, hãy in`"Alice"`, nếu không thì in`"Bob"`. 

### Tại sao nó hoạt động 

Thuật toán tuân theo các quy tắc khớp chính xác. 

Số lượng được tính ở bước 4 và 5 chính xác là số quả đá phạt đền thành công của mỗi đội vì mọi cầu thủ đều được kiểm tra điều kiện ghi điểm bắt buộc. Nếu số bàn thắng khác nhau, đội ghi nhiều bàn thắng hơn phải giành chiến thắng theo định nghĩa. 

Khi số lượng bằng nhau, luật quy định rằng sức mạnh bắn của cá nhân cao nhất sẽ quyết định người chiến thắng. So sánh`max(a)`Và`max(b)`tái tạo chính xác tie-break này. Vì mọi quyết định đều phù hợp với đặc điểm kỹ thuật nên thuật toán luôn trả về kết quả chiến thắng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, m, k = map(int, input().split())

    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    goals_a = sum(1 for x in a if 2 * x >= k)
    goals_b = sum(1 for x in b if 2 * x >= m)

    if goals_a > goals_b:
        print("Alice")
    elif goals_b > goals_a:
        print("Bob")
    else:
        if max(a) > max(b):
            print("Alice")
        else:
            print("Bob")
```Vòng lặp đầu tiên xử lý từng trường hợp thử nghiệm một cách độc lập. 

Việc tính toán số bàn thắng sử dụng`2 * x >= height`thay vì so sánh với`height / 2`. Điều này tránh số học dấu phẩy động và xử lý chính xác chiều cao thủ môn lẻ. 

Trận hòa chỉ được tính khi số bàn thắng bằng nhau. Bởi vì tất cả các lực bắn đều khác nhau nên mức tối đa không thể bằng nhau nên luôn có một đội giành chiến thắng trong cuộc so tài. 

Không cần thêm mảng hoặc cấu trúc dữ liệu nào ngoài việc lưu trữ các giá trị đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5 4 6
1 6 4 8 7
2 3 9 5 10
```Đội A sút ngược chiều cao`6`, Đội B sút ngược chiều cao`4`. 

| Sức mạnh người chơi | Đội | Tình trạng | Mục tiêu? | 
| --- | --- | --- | --- | 
| 1 | A | 2 >= 6 | Không | 
| 6 | A | 12 >= 6 | Có | 
| 4 | A | 8 >= 6 | Có | 
| 8 | A | 16 >= 6 | Có | 
| 7 | A | 14 >= 6 | Có | 

Đội A ghi được 4 bàn thắng. 

| Sức mạnh người chơi | Đội | Tình trạng | Mục tiêu? | 
| --- | --- | --- | --- | 
| 2 | B | 4 >= 4 | Có | 
| 3 | B | 6 >= 4 | Có | 
| 9 | B | 18 >= 4 | Có | 
| 5 | B | 10 >= 4 | Có | 
| 10 | B | 20 >= 4 | Có | 

Đội B ghi được 5 bàn thắng. 

Từ`5 > 4`, Đội B thắng và kết quả là:```
Bob
```Ví dụ này chứng minh rằng việc so sánh số lượng mục tiêu là đủ khi số lượng mục tiêu khác nhau. 

### Ví dụ 2 

đầu vào:```
1
2 4 4
2 10
3 9
```| Số lượng | Đội A | Đội B | 
| --- | --- | --- | 
| Bàn thắng | 2 | 2 | 
| Công suất tối đa | 10 | 9 | 

Số bàn thắng bằng nhau nên áp dụng loạt tie-break. Từ`10 > 9`, Đội A thắng. 

Đầu ra:```
Alice
```Ví dụ này thực hiện so sánh thứ cấp và xác nhận rằng sức mạnh sút tối đa sẽ quyết định người chiến thắng khi số bàn thắng hòa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi người chơi được kiểm tra một lần | 
| Không gian | O(1) phụ trợ | Chỉ có một số bộ đếm và cực đại được sử dụng | 

Với tối đa 100 người chơi mỗi đội và 500 trường hợp thử nghiệm, tổng công việc là rất nhỏ. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

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
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        ga = sum(1 for x in a if 2 * x >= k)
        gb = sum(1 for x in b if 2 * x >= m)

        if ga > gb:
            out.append("Alice")
        elif gb > ga:
            out.append("Bob")
        else:
            out.append("Alice" if max(a) > max(b) else "Bob")

    return "\n".join(out)

# provided samples
assert run(
"""2
5 4 6
1 6 4 8 7
2 3 9 5 10
5 4 6
1 2 3 4 5
6 7 8 9 10
"""
) == "Bob\nBob"

# minimum size
assert run(
"""1
1 1 1
1
2
"""
) == "Bob"

# tie on goals, Team A wins by maximum power
assert run(
"""1
2 4 4
2 10
3 9
"""
) == "Alice"

# odd goalkeeper height, checks half-height handling
assert run(
"""1
1 5 5
2
1
"""
) == "Bob"

# all players score
assert run(
"""1
3 2 2
5 6 7
1 2 3
"""
) == "Alice"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp chơi đơn | Bob | Kích thước hợp lệ tối thiểu | 
| Hòa về mục tiêu | Alice | Logic tie-break đúng đắn | 
| Thủ môn cao 5 | Bob | Xử lý đúng nửa không nguyên | 
| Mọi người đều ghi điểm | Alice | So sánh số bàn thắng thuần túy | 
| Mẫu chính thức | Bob / Bob | Ví dụ về câu lệnh khớp | 

## Vỏ cạnh 

Hãy xem xét trường hợp ngưỡng chiều cao lẻ:```
1
1 5 5
2
1
```Đội A sút trúng chiều cao của thủ môn`5`. Ngưỡng tính điểm là`2.5`. Quyền lực`2`không ghi điểm. 

Thuật toán đánh giá:```
2 * 2 >= 5
4 >= 5
false
```Vậy đội A ghi 0 bàn. 

Đội B sút ngược chiều cao`5`cũng như:```
2 * 1 >= 5
2 >= 5
false
```Đội B cũng ghi được 0 bàn thắng. 

Tie-break so sánh cực đại,`2`so với`1`, vậy là đội A thắng. Thuật toán tránh được lỗi chia số nguyên dẫn đến tính lũy thừa không chính xác.`2`như một mục tiêu. 

Bây giờ hãy xem xét một tỉ số hòa:```
1
2 4 4
2 10
3 9
```Cả hai đội ghi hai bàn vì sức mạnh của mỗi cầu thủ ít nhất là`2`. 

Thuật toán đạt đến giai đoạn tie-break:```
max(A) = 10
max(B) = 9
```Từ`10 > 9`, nó in`"Alice"`. 

Một giải pháp chỉ so sánh số lượng mục tiêu sẽ coi tình huống này là chưa được giải quyết một cách không chính xác. 

Cuối cùng, hãy xem xét đầu vào nhỏ nhất có thể:```
1
1 1 1
1
2
```Cả hai cầu thủ đều ghi bàn vì một nửa`1`là`0.5`. 

Số bàn thắng được gắn ở mức 1. Mức tối đa là`1`Và`2`, do đó Đội B thắng và thuật toán in ra`"Bob"`. 

Điều này xác nhận rằng việc triển khai xử lý chính xác ranh giới dưới của tất cả các ràng buộc.

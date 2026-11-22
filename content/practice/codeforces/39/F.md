---
title: "CF 39F - Ếch hòa bình"
description: "Thumbelina muốn vượt qua đầm lầy bằng cách cưỡi một con ếch. Đầm lầy có các ngọn đồi được đánh số từ 1 đến n, trong đó ngọn đồi i cách hòn đảo đúng 1 mét. Một con ếch có chiều dài bước nhảy d đáp xuống ngọn đồi d, 2d, 3d, ..."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 39
codeforces_index: "F"
codeforces_contest_name: "School Team Contest 1 (Winter Computer School 2010/11)"
rating: 1300
weight: 39
solve_time_s: 98
verified: true
draft: false
---
[CF 39F - Những chú ếch theo chủ nghĩa hòa bình](https://codeforces.com/problemset/problem/39/F) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Thumbelina muốn vượt qua đầm lầy bằng cách cưỡi một con ếch. Đầm lầy có các ngọn đồi được đánh số từ`1`ĐẾN`n`, đồi ở đâu`i`chính xác là`i`cách hòn đảo vài mét. Một con ếch có bước nhảy dài`d`đất trên đồi`d, 2d, 3d, ...`cho đến khi bước nhảy tiếp theo của nó vượt xa`n`, tại thời điểm đó nó đến bờ. 

Một số ngọn đồi có muỗi đang ngủ. Mỗi lần ếch đáp xuống ngọn đồi như vậy là nó sẽ giết chết con muỗi ở đó. Vì ếch là những người theo chủ nghĩa hòa bình nên chúng tôi muốn chọn những con ếch giết được càng ít muỗi càng tốt. 

Đầu vào cung cấp độ dài bước nhảy của tất cả các con ếch và các vị trí ngọn đồi có muỗi. Đầu ra yêu cầu mọi con ếch có đường đi chạm tới số lượng vị trí muỗi tối thiểu. 

Quan sát quan trọng từ các ràng buộc là`m`Và`k`nhiều nhất là cả hai`100`, mặc dù`n`có thể lớn như`10^9`. Một giá trị khổng lồ của`n`ban đầu có thể gợi ý rằng việc mô phỏng mọi ngọn đồi là không thể, và điều đó đúng. Bất kỳ thuật toán nào tỷ lệ thuận với`n`sẽ thất bại ngay lập tức. 

Đồng thời, số lượng ếch và muỗi rất ít. Điều đó thay đổi quan điểm hoàn toàn. Thay vì lặp đi lặp lại qua tất cả các ngọn đồi, chúng ta chỉ cần suy luận về vị trí của muỗi. 

Một mô phỏng đơn giản cho một con ếch sẽ liên tục tạo ra các vị trí`d, 2d, 3d, ...`cho đến khi đi qua`n`. Điều đó trở nên tai hại khi`d = 1`, bởi vì con ếch ghé thăm mọi ngọn đồi từ`1`ĐẾN`n`. Với`n = 10^9`, điều này là không thể. 

Một số trường hợp khó khăn dễ bị xử lý sai nếu việc triển khai được viết quá máy móc. 

Hãy xem xét đầu vào này:```
10 2 2
3 5
6 10
```Con ếch có bước nhảy dài`3`đáp xuống`3, 6, 9`, vậy là nó giết chết đúng một con muỗi trên đồi`6`. Con ếch có bước nhảy dài`5`đáp xuống`5, 10`, thế là nó cũng giết chết một con muỗi. Cả hai con ếch đều phải được in. 

Việc thực hiện bất cẩn dừng lại trước khi kiểm tra ngọn đồi có thể tiếp cận cuối cùng có thể bỏ sót con muỗi ở`10`. 

Một trường hợp tế nhị khác là khi muỗi tồn tại trên những ngọn đồi mà không con ếch nào có thể đậu được.```
12 2 2
4 6
5 7
```Không`5`cũng không`7`chia hết cho`4`hoặc`6`, vậy là cả hai con ếch đều không giết được con muỗi nào. Câu trả lời đúng có cả hai con ếch. 

Việc triển khai cố gắng "di chuyển" từng ngọn đồi ếch thay vì kiểm tra khả năng chia hết có thể dễ dàng làm phức tạp thêm trường hợp này. 

Một tình huống khó khăn nữa là độ dài bước nhảy trùng lặp.```
15 3 1
5 5 7
10
```Cả hai con ếch đều có bước nhảy dài`5`giết muỗi tại`10`, trong khi con ếch có chiều dài bước nhảy`7`không giết chết ai. Đáp án đúng chỉ có ếch thôi`3`. 

Những con ếch được xác định theo thứ tự đầu vào, không phải bởi các giá trị độ dài bước nhảy duy nhất. Sắp xếp ếch mà không bảo toàn chỉ số sẽ tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng hành trình của từng chú ếch. Đối với một con ếch có chiều dài bước nhảy`d`, chúng tôi tạo ra mọi ngọn đồi đã ghé thăm:```
d, 2d, 3d, ...
```Đối với mỗi vị trí hạ cánh, chúng tôi kiểm tra xem có con muỗi nào ở đó không. 

Cách làm này đúng vì nó tuân theo chính xác quy luật chuyển động của ếch. Vấn đề xuất hiện khi`d`là nhỏ. Nếu như`d = 1`, chú ếch ghé thăm mọi ngọn đồi từ`1`ĐẾN`n`. Từ`n`có thể`10^9`, thậm chí một con ếch cũng cần hàng tỷ lần lặp lại. 

Quan sát quan trọng là muỗi rất thưa thớt. Có nhiều nhất`100`tổng số muỗi. Một con ếch chỉ giết những con muỗi nằm trên những ngọn đồi có thể chia được cho chiều dài bước nhảy của nó. 

Thay vì hỏi:```
Which hills does the frog visit?
```chúng ta có thể hỏi:```
Which mosquitoes lie on multiples of the frog's jump length?
```Vì một con muỗi trên đồi`x`, một con ếch có bước nhảy dài`d`giết nó chính xác khi:```
x % d == 0
```Bây giờ công việc trở nên nhỏ bé. Đối với mỗi con ếch, chúng tôi chỉ cần kiểm tra tất cả các vị trí của muỗi bằng cách sử dụng khả năng chia hết. Vì có nhiều nhất`100`ếch và`100`muỗi, tổng số lần kiểm tra chỉ là`10,000`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m · n / d) trường hợp xấu nhất O(m · n) | O(k) | Quá chậm | 
| Tối ưu | O(m · k) | O(1) không bao gồm lưu trữ đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị của`n`,`m`, Và`k`. 

giá trị`n`thực sự không cần thiết trong giải pháp tối ưu hóa vì vị trí của muỗi đã xác định tất cả thông tin liên quan. 
2. Đọc độ dài bước nhảy của tất cả các con ếch vào một mảng. 

Các chỉ số ếch phải được giữ nguyên vì đầu ra yêu cầu đánh số gốc. 
3. Đọc vị trí đồi muỗi vào mảng khác. 
4. Với mỗi con ếch, hãy đếm xem có bao nhiêu vị trí muỗi chia hết cho chiều dài bước nhảy của nó. 

Nếu một con muỗi đậu trên đồi`x`và con ếch nhảy qua`d`, sau đó ếch đáp xuống`x`chính xác khi nào`x`là bội số của`d`. 
5. Lưu trữ số lượng muỗi cho từng con ếch. 
6. Tìm số lượng muỗi tối thiểu trong số tất cả các con ếch. 
7. Thu thập tất cả những con ếch có số lượng tối thiểu đó. 

Những con ếch được báo cáo bằng cách sử dụng chỉ mục dựa trên 1 từ thứ tự đầu vào. 
8. In số lượng ếch tối ưu, sau đó in chỉ số của chúng. 

### Tại sao nó hoạt động 

Một con ếch có bước nhảy dài`d`thăm chính xác những ngọn đồi:```
d, 2d, 3d, ...
```Một ngọn đồi`x`thuộc dãy này khi và chỉ khi`x`chia hết cho`d`. Thuật toán kiểm tra chính xác điều kiện này cho từng đồi muỗi. 

Vì mỗi con muỗi được đếm chính xác khi con ếch đậu trên ngọn đồi của nó nên số lượng tính toán khớp với số lượng muỗi bị con ếch đó giết. Việc chọn tất cả các con ếch có số lượng tối thiểu sẽ tạo ra câu trả lời chính xác theo yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# solution

def solve():
    n, m, k = map(int, input().split())

    frogs = list(map(int, input().split()))
    mosquitoes = list(map(int, input().split()))

    killed = []

    for d in frogs:
        cnt = 0

        for pos in mosquitoes:
            if pos % d == 0:
                cnt += 1

        killed.append(cnt)

    best = min(killed)

    answer = []

    for i in range(m):
        if killed[i] == best:
            answer.append(i + 1)

    print(len(answer))
    print(*answer)

solve()
```Giải pháp không bao giờ lặp lại trên các ngọn đồi. Đó là ý tưởng thực hiện trung tâm. 

Vòng ngoài xử lý từng con ếch một cách độc lập. Đối với một con ếch có chiều dài bước nhảy`d`, vòng lặp bên trong sẽ quét tất cả các vị trí của muỗi và kiểm tra khả năng chia hết bằng cách sử dụng`pos % d == 0`. 

Mảng`killed`lưu trữ số lượng muỗi mà mỗi con ếch tiêu diệt. Sau khi tất cả số đếm được tính toán, giá trị tối thiểu được trích xuất bằng`min(killed)`. 

Vòng lặp cuối cùng xây dựng lại câu trả lời bằng cách sử dụng các chỉ số ếch ban đầu. các`+1`là cần thiết vì các câu lệnh Codeforces sử dụng cách đánh số dựa trên 1. 

Một điểm tinh tế là`n`không tham gia tính toán. Vị trí của muỗi được đảm bảo nằm trong giới hạn hợp lệ và khả năng phân chia đã xác định xem ếch có hạ cánh ở đó trước khi đến bờ hay không. 

Một sai lầm dễ mắc phải khác là sắp xếp ếch theo chiều dài bước nhảy. Đầu ra yêu cầu thứ tự đầu vào ban đầu nên các chỉ số phải không thay đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3 5
2 3 4
1 2 3 4 5
```| Chỉ số ếch | Độ dài nhảy | Vị Trí Muỗi Đánh | Đếm | 
| --- | --- | --- | --- | 
| 1 | 2 | 2, 4 | 2 | 
| 2 | 3 | 3 | 1 | 
| 3 | 4 | 4 | 1 | 

Số lượng tối thiểu là`1`, đạt được bởi ếch`2`Và`3`. 

Đầu ra:```
2
2 3
```Ví dụ này thể hiện trực tiếp quy tắc chia hết. Đồi`4`bị trúng phải những con ếch có chiều dài bước nhảy chia đôi`4`. 

### Ví dụ 2 

đầu vào:```
12 3 3
2 5 7
4 10 11
```| Chỉ số ếch | Độ dài nhảy | Vị Trí Muỗi Đánh | Đếm | 
| --- | --- | --- | --- | 
| 1 | 2 | 4, 10 | 2 | 
| 2 | 5 | 10 | 1 | 
| 3 | 7 | không | 0 | 

Số lượng tối thiểu là`0`, chỉ đạt được bởi ếch`3`. 

Đầu ra:```
1
3
```Dấu vết này cho thấy một số loài ếch có thể không bao giờ tiếp cận được muỗi. Đồi`11`không chia hết cho bất kỳ chiều dài bước nhảy nào ở đây, nên không có con ếch nào giết được con muỗi đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · k) | Mỗi con ếch đều kiểm tra mỗi con muỗi một lần | 
| Không gian | O(m) | Cửa hàng đếm muỗi cho ếch | 

Với`m ≤ 100`Và`k ≤ 100`, thuật toán thực hiện tối đa`10,000`kiểm tra tính chia hết. Đó là rất nhỏ so với các giới hạn, vì vậy giải pháp dễ dàng phù hợp với cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, m, k = map(int, input().split())

    frogs = list(map(int, input().split()))
    mosquitoes = list(map(int, input().split()))

    killed = []

    for d in frogs:
        cnt = 0

        for pos in mosquitoes:
            if pos % d == 0:
                cnt += 1

        killed.append(cnt)

    best = min(killed)

    answer = []

    for i in range(m):
        if killed[i] == best:
            answer.append(i + 1)

    print(len(answer))
    print(*answer)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""5 3 5
2 3 4
1 2 3 4 5
"""
) == "2\n2 3\n", "sample 1"

# minimum size
assert run(
"""1 1 1
1
1
"""
) == "1\n1\n", "minimum case"

# no frog hits any mosquito
assert run(
"""20 2 2
6 8
5 7
"""
) == "2\n1 2\n", "all frogs kill zero"

# duplicate jump lengths
assert run(
"""15 3 1
5 5 7
10
"""
) == "1\n3\n", "preserve indices correctly"

# boundary divisibility
assert run(
"""10 2 2
2 5
10 9
"""
) == "1\n2\n", "must count mosquito at hill 10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vỏ kích thước tối thiểu | Một con ếch được chọn | Xử lý chính xác các giới hạn nhỏ nhất | 
| Không có ếch đánh muỗi | Tất cả ếch được chọn | Xử lý đúng số 0 | 
| Độ dài bước nhảy trùng lặp | Chỉ có ếch sau này được chọn | Bảo tồn các chỉ số gốc | 
| Muỗi ở ngọn đồi cuối cùng | Đếm số chia đúng | Tránh từng sai lầm | 

## Vỏ cạnh 

Hãy xem xét trường hợp ngọn đồi cuối cùng có thể đến được có một con muỗi.```
10 2 2
3 5
6 10
```Đối với chiều dài bước nhảy`3`, con ếch đáp xuống`3, 6, 9`, giết chết một con muỗi. Đối với chiều dài bước nhảy`5`, con ếch đáp xuống`5, 10`, cũng giết chết một con muỗi. 

Thuật toán kiểm tra:```
6 % 3 == 0
10 % 5 == 0
```Cả hai đều được tính chính xác vì khả năng chia cắt đương nhiên bao gồm cả vị trí hạ cánh cuối cùng trước khi vào bờ. 

Bây giờ hãy xem xét những con muỗi mà không con ếch nào có thể chạm tới.```
12 2 2
4 6
5 7
```Thuật toán kiểm tra:```
5 % 4 != 0
7 % 4 != 0
5 % 6 != 0
7 % 6 != 0
```Cả hai con ếch đều không diệt được muỗi nên cả hai đều được in. Không cần xử lý đặc biệt vì những con muỗi không thể tiếp cận chỉ đơn giản là thất bại trong bài kiểm tra khả năng phân chia. 

Cuối cùng, hãy xem xét độ dài bước nhảy lặp đi lặp lại.```
15 3 1
5 5 7
10
```Số đếm trở thành: 

| Chỉ số ếch | Độ dài nhảy | Đếm | 
| --- | --- | --- | 
| 1 | 5 | 1 | 
| 2 | 5 | 1 | 
| 3 | 7 | 0 | 

Thuật toán lưu trữ số đếm theo vị trí của ếch chứ không phải theo độ dài bước nhảy duy nhất. Điều đó duy trì việc đánh số cần thiết và chỉ xuất ra chính xác ếch`3`.

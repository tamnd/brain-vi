---
title: "CF 102346E - Triển lãm cá hề"
description: "Chúng tôi có (N) xe tăng. Bể (i) chứa cá hề đực (Mi) và cá hề cái (Fi). Điều kiện (Mi=0) hoặc (Fi0) có nghĩa là mọi bể hiện có con đực cũng có ít nhất một con cái. Trong một đêm, có đúng một con cá có thể được di chuyển từ bể này sang bể khác."
date: "2026-08-14T05:25:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102346
codeforces_index: "E"
codeforces_contest_name: "2019-2020 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102346
solve_time_s: 323
verified: true
draft: false
---

[CF 102346E - Triển lãm cá hề](https://codeforces.com/problemset/problem/102346/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (N) xe tăng. Bể (i) chứa (M_i) cá hề đực và (F_i) cá hề cái. Điều kiện (M_i=0) hoặc (F_i>0) có nghĩa là mọi bể hiện có con đực thì cũng có ít nhất một con cái. 

Trong một đêm, có đúng một con cá có thể được di chuyển từ bể này sang bể khác. Vào cuối đêm đó, mỗi bể chứa ít nhất một con đực và không có con cái nào khiến chính xác một con đực trong bể đó trở thành con cái vào ngày hôm sau. Mục tiêu là làm cho mọi con cá đực trở thành con cái, sử dụng càng ít chuyển động càng tốt. Câu trả lời là số lần di chuyển cá tối thiểu cần thiết. 

Số lượng bể nhiều nhất là 3000, trong khi số lượng cá có thể lên tới (10^5). Do đó, tổng số cá có thể vào khoảng (3\cdot10^8), do đó thuật toán không thể lặp lại từng con cá. Công việc liên quan phải phụ thuộc vào số lượng bể chứ không phụ thuộc vào tổng số cá. Với (N=3000), (O(N^2)) đã có khoảng chín triệu hoạt động và là hợp lý, trong khi việc liệt kê theo cấp số nhân các tập hợp con xe tăng là hoàn toàn không khả thi. 

Có một số trường hợp khó xử lý. Nếu không có con đực nào cả, câu trả lời là không. Ví dụ,```
2
0 3
0 5
```không cần di chuyển, mặc dù bể chứa cá. 

Một chiếc xe tăng có thể hoàn toàn trống rỗng. Ví dụ,```
2
1 1
0 0
```cần một chuyển động. Di chuyển con cái ra khỏi bể đầu tiên vào bể trống. Bể đầu tiên chỉ có nam giới, vì vậy nam giới sẽ trở thành nữ giới vào ngày hôm sau. Việc coi một chiếc xe tăng trống rỗng như một chiếc xe tăng bình thường chỉ dành cho nữ sẽ bỏ lỡ khả năng này. 

Bể chứa những con đực không nhất thiết phải là bể nơi những con đực đó được chuyển đổi. Ví dụ,```
2
2 5
1 3
```có ba con đực. Việc xử lý hai bể độc lập sẽ tốn (5+2-1=6) chuyển động cho bể đầu tiên và (3+1-1=3) cho bể thứ hai, cho chín chuyển động. Một giải pháp tốt hơn là di chuyển một con đực từ bể thứ hai sang bể thứ nhất và xử lý cả ba con đực ở đó, đòi hỏi bảy chuyển động. Đầu ra mẫu chính thức là 7. 

Mẫu thứ ba,```
4
2 3
0 0
3 1
0 0
```có năm con đực và hai bể trống. Câu trả lời của nó là 5. Những chiếc xe tăng trống rất hữu ích vì bản thân chúng có thể trở thành những chiếc xe tăng trong đó những con đực được cải đạo. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp là quyết định bể nào cuối cùng sẽ được sử dụng làm bể nơi chuyển đổi cá đực. Đối với mỗi tập hợp con xe tăng, chúng tôi có thể tính toán chi phí phân bổ tất cả con đực vào các xe tăng đã chọn đó và giữ được kết quả tốt nhất. Điều này đúng vì cuối cùng mọi con đực đều phải thuộc về một số xe tăng không có con cái khi quá trình chuyển đổi của nó được kích hoạt. 

Vấn đề là số lượng tập hợp con. Có (2^N) lựa chọn khả thi và việc đánh giá một tập hợp con bằng cách quét tất cả (N) bể sẽ đưa ra các phép toán (\Theta(N2^N)). Tại (N=3000), thậm chí (2^{3000}) vượt xa mọi tính toán thực tế. 

Quan sát quan trọng là sự đóng góp của việc lựa chọn bể có thể được thể hiện độc lập với các bể được chọn khác. Bắt đầu từ một chiến lược tham chiếu đơn giản, trong đó mỗi nam phải thực hiện hai động tác. Một chuyển động đưa con đực vào bể sẽ xử lý nó và một chuyển động khác giải thích việc loại bỏ con cái được tạo ra khi bể đó được sử dụng lại cho một con đực khác. Điều này đưa ra một chi phí tham khảo của 

[ 
2\sum_i M_i. 
] 

Bây giờ hãy cân nhắc việc chọn bể (i) là một trong những bể thực sự xử lý con đực. Nếu con đực (M_i) ban đầu của nó vẫn ở đó, chúng ta không cần phải trả hai lần di chuyển cho mỗi con. Thay vào đó, tất cả con đực (M_i) có thể được xử lý bằng chuyển động (F_i+M_i-1). Chênh lệch so với giá tham chiếu (2M_i) là 

F_i-M_i-1. 
] 

Vì vậy, việc chỉ định bể (i) làm bể xử lý sẽ thay đổi câu trả lời một cách chính xác. 

[ 
c_i=F_i-M_i-1. 
] 

Công thức tương tự cũng xử lý các xe tăng trống và chỉ dành cho nữ. Nếu (M_i=0), bể đó có thể tiếp nhận con đực từ bể khác. Giá trị của nó vẫn là (F_i-1). Cụ thể, một bể trống có (c_i=-1), biểu thị chính xác mức tiết kiệm một chuyển động thu được từ việc sử dụng nó làm bể xử lý. 

Mỗi bể xử lý được chọn phải tiếp nhận ít nhất một con đực, do đó có thể chọn tối đa (M=\sum_i M_i). Vì số âm (c_i) luôn cải thiện câu trả lời nên chúng tôi muốn có nhiều giá trị âm nhất, với nhiều nhất là (M). Nếu không có giá trị âm thì ta vẫn cần 1 bể xử lý vì có con đực nên ta chọn cái nhỏ nhất (c_i). 

Điều này biến tìm kiếm tập hợp con theo cấp số nhân thành các giá trị sắp xếp (N) và lấy các giá trị âm hữu ích. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta(N2^N)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng bể và tính tổng số con đực (M). Nếu (M=0), không có con cá nào phải chuyển đổi giới tính nên câu trả lời ngay lập tức là 0. 
2. Với mỗi chiếc xe tăng, hãy tính giá trị của nó 
[ 
c_i=F_i-M_i-1. 
] 
Đây là sự thay đổi về chi phí khi bể đó được chọn làm bể xử lý thay vì sử dụng chi phí tham chiếu của hai chuyển động cho mỗi con đực ban đầu. 
3. Sắp xếp tất cả (c_i) theo thứ tự tăng dần. Các giá trị âm nhất thể hiện mức tiết kiệm lớn nhất, vì vậy chúng cần được xem xét trước tiên. 
4. Bắt đầu với`answer = 2 * M`. Đây là chi phí tham khảo trước khi lựa chọn bất kỳ bể xử lý nào. 
5. Lấy tối đa (M) giá trị âm từ mảng đã sắp xếp và cộng chúng vào`answer`. Mỗi giá trị được chọn tương ứng với một xe tăng nhận được ít nhất một con đực và lưu lại các chuyển động (-c_i). 
6. Nếu không có giá trị âm và (M>0), thay vào đó hãy thêm giá trị nhỏ nhất. Cần có một bể xử lý và khi mọi lựa chọn có thể có sự thay đổi chi phí không âm thì lựa chọn ít tốn kém nhất là tối ưu. 
7. In giá trị kết quả. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ chiến lược cuối cùng nào và gọi một bể là bể xử lý nếu cuối cùng có ít nhất một con đực được chuyển đổi ở đó. Mỗi con đực thuộc về chính xác một chiếc xe tăng như vậy. Nếu bể không phải là bể xử lý thì tất cả cá đực ban đầu của nó phải được chuyển đi nơi khác. Nếu một bể là một bể xử lý, việc giữ các con đực ban đầu ở đó sẽ thay thế chi phí tham chiếu của hai chuyển động cho mỗi con đực bằng (F_i+M_i-1), đưa ra sự điều chỉnh độc lập (F_i-M_i-1). Hạn chế toàn cầu duy nhất là mỗi bể xử lý cần ít nhất một con đực, do đó có thể có nhiều nhất (M) bể xử lý. Do đó, trong số tất cả các chiến lược có thể, chiến lược tốt nhất sẽ chọn chính xác những điều chỉnh tiêu cực có lợi nhất, lên tới (M) xe tăng. Nếu không có lợi thì việc lựa chọn mức điều chỉnh nhỏ nhất là cần thiết vì phải tồn tại ít nhất một bể xử lý. Thuật toán xem xét chính xác những lựa chọn đó, do đó nó đạt được số lượng chuyển động tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    total_males = 0
    costs = []

    for _ in range(n):
        m, f = map(int, input().split())
        total_males += m
        costs.append(f - m - 1)

    if total_males == 0:
        print(0)
        return

    costs.sort()

    answer = 2 * total_males
    used = 0

    for c in costs:
        if c >= 0 or used == total_males:
            break
        answer += c
        used += 1

    if used == 0:
        answer += costs[0]

    print(answer)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên chỉ cần tổng số con đực và giá trị (F_i-M_i-1) cho mỗi bể. Sau đó, không cần lưu trữ các cặp (M_i,F_i) gốc. 

Chi phí tham khảo được lưu trữ trong`answer`BẰNG`2 * total_males`. Số nguyên Python có độ chính xác tùy ý, do đó tổng số cá tối đa có thể không gây ra sự cố tràn. 

Sau khi sắp xếp, các giá trị âm được xử lý từ nhỏ nhất đến lớn nhất. Chúng tôi dừng lại sau khi chọn`total_males`giá trị vì mỗi bể xử lý được chọn phải chứa ít nhất một con đực. Trong thực tế (N\le3000), giới hạn này chủ yếu liên quan đến tính đúng đắn của công thức hơn là hiệu suất. 

điều kiện`used == 0`xử lý trường hợp mọi (c_i) đều không âm. Chúng ta vẫn phải chọn một bể xử lý bất cứ khi nào có ít nhất một con đực, sao cho giá trị nhỏ nhất được cộng thêm. 

Một thùng rỗng tự nhiên tạo ra (c_i=-1). Không có trường hợp đặc biệt nào cần thiết cho nó, đây là một trong những tính năng hữu ích của công thức. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2
2 1
0 2
```Tổng cộng có hai nam nhân. Hai giá trị của bể là 

[ 
1-2-1=-2 
] 

và 

[ 
2-0-1=1. 
] 

Các giá trị được sắp xếp là (-2,1). Chúng ta có thể chọn tối đa hai bể xử lý, nhưng chỉ giá trị âm mới cải thiện được câu trả lời. 

| Bước | Tổng số nam | Sắp xếp chi phí | Chi phí đã chọn | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 2 | -2, 1 | không | 4 | 
| Chọn phủ định đầu tiên | 2 | -2, 1 | -2 | 2 | 
| Dừng lại | 2 | -2, 1 | -2 | 2 | 

Kết quả là 2. Thực hiện, di chuyển con cái duy nhất ra khỏi bể đầu tiên, để một con đực trở thành con cái, sau đó di chuyển con cái đó ra ngoài và để con đực còn lại trở thành con cái. 

### Mẫu 2 

Đầu vào là```
2
2 5
1 3
```Có ba con đực. Các giá trị của bể là 

[ 
5-2-1=2 
] 

và 

[ 
3-1-1=1. 
] 

Không có giá trị nào âm nên chúng ta phải chọn giá trị nhỏ hơn. 

| Bước | Tổng số nam | Sắp xếp chi phí | Chi phí đã chọn | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 1, 2 | không | 6 | 
| Không có giá trị âm | 3 | 1, 2 | không | 6 | 
| Chọn nhỏ nhất | 3 | 1, 2 | 1 | 7 | 

Bể thứ hai là bể xử lý tốt hơn. Di chuyển con đực của nó vào bể đầu tiên, sau đó loại bỏ những con cái cần thiết đồng thời chuyển đổi cả ba con đực. Kết quả tối thiểu là 7. 

### Mẫu 3 

Đầu vào là```
4
2 3
0 0
3 1
0 0
```Có năm nam nhân. Bốn giá trị của bể là 

[ 
3-2-1=0, 
] 

[ 
0-0-1=-1, 
] 

[ 
1-3-1=-3, 
] 

và 

[ 
0-0-1=-1. 
] 

Ba giá trị âm đều có thể được chọn vì có năm con đực. 

| Bước | Tổng số nam | Sắp xếp chi phí | Chi phí đã chọn | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 5 | -3, -1, -1, 0 | không | 10 | 
| Chọn -3 | 5 | -3, -1, -1, 0 | -3 | 7 | 
| Chọn -1 | 5 | -3, -1, -1, 0 | -3, -1 | 6 | 
| Chọn -1 | 5 | -3, -1, -1, 0 | -3, -1, -1 | 5 | 

Hai thùng rỗng đặc biệt hữu ích ở đây. Mỗi cái có giá trị (-1), do đó, mỗi cái có thể đóng vai trò là bể xử lý và lưu một chuyển động liên quan đến chiến lược tham chiếu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Chúng tôi tính toán một giá trị cho mỗi bể và sắp xếp các giá trị (N). | 
| Không gian | (O(N)) | Danh sách các giá trị bể chứa (N) số nguyên. | 

Với (N\le3000), việc sắp xếp chỉ vài nghìn số nguyên là dễ dàng trong phạm vi tài nguyên sẵn có. Thuật toán không bao giờ phụ thuộc vào tổng số lượng cá có thể khổng lồ, đây là yêu cầu chính được áp đặt bởi giới hạn (10^5) trên mỗi số lượng bể. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    total_males = 0
    costs = []

    for _ in range(n):
        m = next(it)
        f = next(it)
        total_males += m
        costs.append(f - m - 1)

    if total_males == 0:
        return "0\n"

    costs.sort()

    answer = 2 * total_males
    used = 0

    for c in costs:
        if c >= 0 or used == total_males:
            break
        answer += c
        used += 1

    if used == 0:
        answer += costs[0]

    return str(answer) + "\n"

# Provided sample 1
assert solve_data(
    """2
2 1
0 2
"""
) == "2\n", "sample 1"

# Provided sample 2
assert solve_data(
    """2
2 5
1 3
"""
) == "7\n", "sample 2"

# Provided sample 3
assert solve_data(
    """4
2 3
0 0
3 1
0 0
"""
) == "5\n", "sample 3"

# Minimum-size input, one male and one female
assert solve_data(
    """2
1 1
0 0
"""
) == "1\n", "one male with an empty tank"

# No males at all
assert solve_data(
    """2
0 0
0 100000
"""
) == "0\n", "no males"

# All tanks equal, maximum-size construction
n = 3000
maximum_case = str(n) + "\n" + ("100000 100000\n" * n)
assert solve_data(maximum_case) == "599997000\n", "maximum-size case"

# Boundary case with several empty tanks
assert solve_data(
    """5
1 1
0 0
0 0
0 2
0 0
"""
) == "1\n", "one male and multiple empty tanks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 1 / 0 0`| 1 | Cấu hình kích thước tối thiểu và sử dụng bình rỗng | 
|`2 / 0 0 / 0 100000`| 0 | Không có con đực nào cả | 
| 3000 bản`100000 100000`| 599997000 | Tối đa (N), số lượng lớn và phạm vi số nguyên | 
|`5 / 1 1 / 0 0 / 0 0 / 0 2 / 0 0`| 1 | Một số xe tăng trống và điều kiện mục tiêu tối đa (M) | 

## Vỏ cạnh 

Khi không có con đực, thuật toán trả về 0 trước khi sắp xếp. Vì```
2
0 0
0 100000
```tổng số nam là 0 nên không cần di chuyển. 

Khi bình rỗng, giá trị của nó là (-1). Vì```
2
1 1
0 0
```các giá trị là (-1,-1) và có một nam, do đó thuật toán chọn một trong số chúng. Chi phí tham khảo ban đầu là 2 và xe tăng được chọn đóng góp (-1), cho đáp án 1. 

Khi một bể có con cái nhưng không có con đực, nó vẫn có thể là bể xử lý hữu ích vì con đực từ bể khác có thể được chuyển vào đó. Ví dụ,```
2
2 1
0 5
```có các giá trị (-2) và (4). Bể đầu tiên là mục tiêu tốt hơn, vì vậy con cái của bể thứ hai có thể giữ nguyên vị trí của chúng trong khi con đực của nó được xử lý ở nơi khác nếu cần. Công thức coi bể thứ hai là mục tiêu khả thi mà không yêu cầu nó phải chứa một con đực ban đầu một cách không chính xác. 

Khi có nhiều xe tăng mục tiêu có lợi hơn con đực, chúng ta không thể chọn tất cả vì mỗi xe tăng được chọn đều cần ít nhất một con đực. Giả sử có một con đực và nhiều bể trống. Chỉ có một mục tiêu thực sự có thể tiếp nhận người đàn ông đó. Do đó, thuật toán mất nhiều nhất`total_males`các giá trị âm. 

Khi mỗi bể có giá trị không âm (F_i-M_i-1), việc chọn bể xử lý không bao giờ cải thiện được chi phí tham chiếu. Mục tiêu vẫn cần thiết nếu có ít nhất một nam, vì vậy thuật toán chọn giá trị nhỏ nhất. Đây chính xác là tình huống trong Mẫu 2, trong đó các giá trị là 2 và 1 và câu trả lời trở thành (2\cdot3+1=7). 

Khi một số bể có giá trị âm, tất cả những bể hữu ích nên được chọn miễn là có đủ cá đực để chiếm giữ chúng. Trong Mẫu 3, các giá trị là (0,-1,-3,-1) và có năm nam, do đó có thể chọn cả ba giá trị âm. Bắt đầu từ (2\cdot5=10), các điều chỉnh giảm câu trả lời thành (10-3-1-1=5).

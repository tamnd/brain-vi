---
title: "CF 1428B - Phòng thắt lưng"
description: "Các phòng tạo thành một chu kỳ. Giữa mỗi cặp phòng liên tiếp có một băng chuyền, mỗi băng có thể hướng theo chiều kim đồng hồ (), hướng ngược chiều kim đồng hồ (<) hoặc sử dụng được theo cả hai hướng (-)."
date: "2026-06-11T05:28:44+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1428
codeforces_index: "B"
codeforces_contest_name: "Codeforces Raif Round 1 (Div. 1 + Div. 2)"
rating: 1200
weight: 1428
solve_time_s: 103
verified: true
draft: false
---

[CF 1428B - Phòng có dây đai](https://codeforces.com/problemset/problem/1428/B) 

**Đánh giá:** 1200 
**Tags:** đồ thị, thực hiện 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các phòng tạo thành một chu kỳ. Giữa mỗi cặp phòng liên tiếp có một băng chuyền, mỗi băng có thể chuyển động theo chiều kim đồng hồ (`>`), hướng ngược chiều kim đồng hồ (`<`) hoặc có thể sử dụng theo cả hai hướng (`-`). 

Một căn phòng được gọi là có thể quay trở lại nếu con rắn bắt đầu từ căn phòng đó có thể rời khỏi phòng và sau đó quay trở lại căn phòng đó bằng cách đi theo các hướng vành đai cho phép. 

Một cách giải thích đồ thị hữu ích là coi mỗi phòng là một đỉnh của đồ thị có hướng. Mỗi đai đóng góp một cạnh định hướng hoặc hai cạnh định hướng. Chúng ta phải đếm xem có bao nhiêu đỉnh thuộc về một chu trình có hướng nào đó, bởi vì một con rắn có thể rời đi và sau đó quay trở lại chính xác khi tồn tại một chu trình có hướng đi qua căn phòng của nó. 

Tổng số phòng trong tất cả các trường hợp thử nghiệm tối đa là 300.000. Điều này ngay lập tức loại trừ bất kỳ thuật toán nào thực hiện tìm kiếm biểu đồ từ mọi phòng. Một cách tiếp cận ngây thơ sẽ đòi hỏi khoảng$O(n^2)$hoạt động trong trường hợp xấu nhất, điều này trở thành khoảng$9 \times 10^{10}$hoạt động khi$n = 300000$. Chúng ta cần một giải pháp tuyến tính. 

Một số tình huống rất dễ bị đánh giá sai. 

Coi như:```
1
5
>>>>>
```Câu trả lời đúng là:```
5
```Thoạt nhìn, đồ thị trông hoàn toàn có hướng, nhưng mọi phòng đều thuộc cùng một chu trình có hướng quanh vòng tròn. Bắt đầu từ bất kỳ căn phòng nào, con rắn có thể di chuyển khắp nơi và quay trở lại. 

Coi như:```
1
2
<>
```Câu trả lời đúng là:```
0
```Phòng 0 chỉ có thể di chuyển đến phòng 1 và phòng 1 chỉ có thể di chuyển đến phòng 0. Không phòng nào có đường đi ra ngoài để nó rời đi và sau đó quay trở lại. Đồ thị không chứa chu trình có hướng. 

Một trường hợp tế nhị khác là:```
1
3
<--
```Câu trả lời đúng là:```
3
```Một giải pháp bất cẩn có thể chỉ tập trung vào các hướng chu kỳ toàn cầu. Ở đây hai`-`vành đai tạo ra chuyển động hai chiều cục bộ và mọi phòng đều có thể tham gia vào một chu trình. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xây dựng đồ thị có hướng và kiểm tra từng phòng một cách độc lập. Đối với mỗi phòng, chúng tôi có thể chạy DFS hoặc BFS để xác định xem liệu chúng tôi có thể rời khỏi phòng và quay lại hay không. Điều này đúng vì nó kiểm tra rõ ràng định nghĩa về khả năng trả lại. 

Vấn đề là chi phí. Một tìm kiếm đồ thị mất$O(n)$thời gian và lặp lại nó cho tất cả$n$phòng yêu cầu$O(n^2)$. Với$n$đạt 300.000, con số này vượt xa giới hạn. 

Để tìm thứ gì đó nhanh hơn, chúng ta nên kiểm tra cấu trúc của đồ thị thay vì coi nó như một đồ thị có hướng tùy ý. 

Nhận xét quan trọng là chỉ có hai tình huống toàn cầu có thể xảy ra. 

Nếu tất cả các dây đai định hướng đều hướng về cùng một hướng tròn, nghĩa là sợi dây không chứa`<`hoặc không chứa`>`, thì toàn bộ đường tròn là một chu trình có hướng. Mỗi phòng đều có thể được trả lại. 

Ngược lại thì cả hai`<`Và`>`xuất hiện ở đâu đó. Trong trường hợp đó không có chu trình toàn cục xung quanh đường tròn. Các phòng duy nhất chắc chắn có thể thuộc về một chu kỳ là các phòng liền kề với một chu kỳ.`-`thắt lưng. 

Tại sao? MỘT`-`vành đai tạo ra chuyển động theo cả hai hướng giữa hai phòng lân cận. Hai phòng đó ngay lập tức tạo thành một chu kỳ có chiều dài 2. Bất kỳ phòng nào chạm vào vành đai như vậy đều có thể trả lại. 

Khi cả hai`<`Và`>`tồn tại, mọi cạnh có hướng không phải là một phần của`-`vành đai tham gia vào một định hướng không nhất quán trên toàn cầu. Biểu đồ không thể chứa bất kỳ chu kỳ lớn hơn. Các chu kỳ có hướng duy nhất đến từ các cạnh hai chiều này. 

Điều này làm giảm vấn đề đối với việc đếm các phòng là điểm cuối của ít nhất một`-`đai, trừ khi toàn bộ vòng tròn được định hướng nhất quán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi cấu hình dây đai. 
2. Kiểm tra xem chuỗi có chứa bất kỳ`>`nhân vật. 
3. Kiểm tra xem chuỗi có chứa bất kỳ`<`nhân vật. 
4. Nếu thiếu ít nhất một trong các hướng này thì toàn bộ vòng tròn có thể di chuyển theo một hướng duy nhất xung quanh vòng tròn. Mỗi phòng đều thuộc cùng một chu trình được định hướng, do đó đầu ra`n`. 
5. Nếu không, hãy tạo một mảng`good`chiều dài`n`, ban đầu tất cả đều sai. 
6. Đối với mọi vị trí đai`i`, nếu như`s[i] == '-'`, đánh dấu phòng`i`và phòng`(i + 1) mod n`tốt như vậy. 
7. Đếm xem có bao nhiêu phòng được đánh dấu tốt và số lượng đầu ra được tính. 

Lý do bước 6 có hiệu quả là vì`-`vành đai cho phép di chuyển theo cả hai hướng giữa hai phòng điểm cuối của nó. Những căn phòng đó có thể di chuyển đi và quay trở lại ngay lập tức, thỏa mãn định nghĩa. 

### Tại sao nó hoạt động 

Nếu vòng tròn chỉ có một hướng định hướng thì tất cả các dây đai đều theo chiều kim đồng hồ hoặc tất cả đều ngược chiều kim đồng hồ. Đi theo vòng tròn cuối cùng sẽ quay trở lại phòng bắt đầu, vì vậy mọi phòng đều có thể quay lại được. 

Bây giờ giả sử cả hai`<`Và`>`xuất hiện. Bất kỳ chu kỳ có chỉ đạo nào cũng sẽ phải tuân theo thứ tự vòng tròn của các phòng. Một chu kỳ dài hơn hai phòng sẽ yêu cầu một sự định hướng nhất quán trên khắp vòng, điều này là không thể vì cả hai hướng đều xuất hiện. Các chu kỳ còn lại duy nhất là độ dài hai chu kỳ được tạo bởi`-`thắt lưng. Mỗi vành đai như vậy kết nối hai phòng lân cận theo cả hai hướng, khiến cả hai điểm cuối đều có thể quay lại được. Không có phòng không chạm vào một`-`vành đai có thể thuộc về bất kỳ chu kỳ. 

Như vậy thuật toán sẽ đếm chính xác số phòng có thể trả lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()

        has_left = '<' in s
        has_right = '>' in s

        if not has_left or not has_right:
            print(n)
            continue

        good = [False] * n

        for i, ch in enumerate(s):
            if ch == '-':
                good[i] = True
                good[(i + 1) % n] = True

        print(sum(good))

solve()
```Phần đầu tiên kiểm tra xem cả hai hướng định hướng có tồn tại hay không. Nếu thiếu một hướng, mọi phòng sẽ được tính ngay lập tức. 

Phần thứ hai xử lý trường hợp hướng hỗn hợp. Mảng`good`ghi lại xem một căn phòng có tiếp giáp với một vành đai hai chiều nào đó hay không. 

biểu thức`(i + 1) % n`là cần thiết vì các phòng tạo thành một vòng tròn. Khi`i = n - 1`, phòng bên cạnh là phòng`0`. 

Sử dụng mảng boolean sẽ tránh được việc tính hai lần. Một căn phòng có thể chạm vào hai cái khác nhau`-`thắt lưng, nhưng nó chỉ nên đóng góp một lần cho câu trả lời cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
-><-
```Chuỗi chứa cả hai`<`Và`>`, vì vậy chúng tôi đếm các phòng liền kề`-`. 

| Chỉ số đai | Nhân vật | Phòng được đánh dấu | tốt sau bước | 
| --- | --- | --- | --- | 
| 0 | - | 0, 1 | [T, T, F, F] | 
| 1 | > | không | [T, T, F, F] | 
| 2 | < | không | [T, T, F, F] | 
| 3 | - | 3, 0 | [T, T, F, T] | 

Câu trả lời là 3. 

Phòng 2 không chạm vào đai 2 chiều nên không trả lại được. 

### Ví dụ 2 

đầu vào:```
5
>>>>>
```| Kiểm tra | Kết quả | 
| --- | --- | 
| Chứa`>`| Có | 
| Chứa`<`| Không | 

Vì không có một hướng nào nên mọi phòng đều thuộc về chu trình được định hướng theo vòng tròn. 

Đầu ra:```
5
```Ví dụ này minh họa trường hợp chu kỳ toàn cầu. KHÔNG`-`thắt lưng là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đai được kiểm tra tối đa một lần | 
| Không gian | O(n) | Mảng Boolean được sử dụng trong trường hợp hướng hỗn hợp | 

Tổng số lượng phòng tối đa là 300.000, do đó tổng công việc trên tất cả các trường hợp thử nghiệm là tuyến tính ở kích thước đầu vào. Điều này dễ dàng phù hợp với giới hạn thời gian và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = []

    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()

        has_left = '<' in s
        has_right = '>' in s

        if not has_left or not has_right:
            out.append(str(n))
            continue

        good = [False] * n

        for i, ch in enumerate(s):
            if ch == '-':
                good[i] = True
                good[(i + 1) % n] = True

        out.append(str(sum(good)))

    return "\n".join(out)

# provided samples
assert run(
"""4
4
-><-
5
>>>>>
3
<--
2
<>
"""
) == """3
5
3
0"""

# minimum size, no cycle
assert run(
"""1
2
<>
"""
) == "0"

# all bidirectional
assert run(
"""1
4
----
"""
) == "4"

# mixed directions, one bidirectional belt
assert run(
"""1
5
><-><
"""
) == "2"

# all clockwise
assert run(
"""1
6
>>>>>>
"""
) == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / <>`|`0`| Cấu hình không thể hoàn trả nhỏ nhất | 
|`4 / ----`|`4`| Mỗi phòng liền kề vành đai hai chiều | 
|`5 / ><-><`|`2`| Chỉ các điểm cuối của một`-`đếm | 
|`6 / >>>>>>`|`6`| Toàn bộ vòng tròn tạo thành một chu trình có hướng | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
<>
```Cả hai hướng đều xuất hiện nên thuật toán đi vào nhánh thứ hai. không có`-`thắt lưng nên không có chỗ nào được đánh dấu. Câu trả lời là 0. Điều này phù hợp với thực tế là không phòng nào có thể rời đi và sau đó quay lại. 

Coi như:```
1
5
>>>>>
```Chuỗi không chứa`<`. Bước 4 ngay lập tức trở lại`n = 5`. Toàn bộ vòng là một chu trình được định hướng, vì vậy mọi phòng đều có thể được trả lại. 

Coi như:```
1
3
<--
```Cả hai hướng đều không hiện diện vì không có`>`. Thuật toán xuất ra 3 ngay lập tức. Thật vậy, một con rắn có thể di chuyển quanh vòng tròn và quay trở lại từ mọi phòng. 

Coi như:```
1
5
><-><
```Cả hai`<`Và`>`xuất hiện nên chỉ có phòng chạm vào`-`được tính. các`-`vành đai ở vị trí 2, nối phòng 2 và 3. Hai phòng đó được đánh dấu, tạo ra câu trả lời 2. Không thể tồn tại chu kỳ lớn hơn vì vòng tròn chứa các hướng xung đột.

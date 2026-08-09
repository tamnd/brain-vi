---
title: "CF 102470I - Điện thoại hạnh phúc"
description: "Mỗi cuộc điện thoại chiếm một khoảng thời gian liên tục. Một cuộc gọi được mô tả bởi hai điểm cuối của nó, thời gian bắt đầu S và thời gian kết thúc S + D, trong đó D là thời lượng của cuộc gọi. Bản thân số điện thoại không ảnh hưởng đến câu trả lời."
date: "2026-08-09T15:30:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102470
codeforces_index: "I"
codeforces_contest_name: "2009-2010 ACM ICPC Southwestern European Regional Programming Contest (SWERC 2009)"
rating: 0
weight: 102470
solve_time_s: 206
verified: true
draft: false
---

[CF 102470I - Điện thoại vui vẻ](https://codeforces.com/problemset/problem/102470/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi cuộc điện thoại chiếm một khoảng thời gian liên tục. Một cuộc gọi được mô tả bởi hai điểm cuối, thời gian bắt đầu`S`và thời điểm kết thúc của nó`S + D`, Ở đâu`D`là thời hạn của nó. Bản thân số điện thoại không ảnh hưởng đến câu trả lời. Chúng chỉ xác định ai đang nói nên sau khi đọc cuộc gọi, chúng ta chỉ cần khoảng thời gian của nó. 

Đối với mỗi truy vấn của cảnh sát, chúng tôi được cung cấp một khoảng thời gian khác. Chúng ta phải đếm có bao nhiêu cuộc gọi trùng lặp với truy vấn đó trong một khoảng thời gian nhất định. Cuộc gọi kết thúc chính xác khi truy vấn bắt đầu không được tính và cuộc gọi bắt đầu chính xác khi truy vấn kết thúc không được tính. Nói cách khác, nếu một cuộc gọi chiếm`[a, b]`và một truy vấn chiếm`[c, d]`, chúng giao nhau trong ít nhất một giây chính xác khi`a < d`Và`b > c`. 

Có thể có ít hơn 10.000 cuộc gọi và ít hơn 100 truy vấn trong một trường hợp thử nghiệm. Do đó, việc so sánh trực tiếp mọi cuộc gọi với mọi truy vấn sẽ thực hiện tối đa`9999 * 99 = 989901`kiểm tra chồng chéo trong một trường hợp thử nghiệm. Đó không phải là một con số lớn về mặt thiên văn, vì vậy sức mạnh vũ phu có thể tồn tại đối với những giới hạn đã nêu này. Tuy nhiên, cấu trúc của truy vấn cho phép chúng tôi thực hiện tốt hơn đáng kể. Việc sắp xếp thời gian bắt đầu và kết thúc cuộc gọi cho phép mỗi truy vấn được trả lời bằng cách sử dụng tìm kiếm nhị phân, giảm công việc trên mỗi truy vấn từ tuyến tính về số lượng cuộc gọi sang logarit. 

Giá trị thời gian có thể lớn nhưng`Start + Duration`phù hợp với số nguyên 32 bit có dấu. Dù sao thì số nguyên Python không có vấn đề tràn ở đây và chúng tôi không bao giờ cần mô phỏng từng giây riêng lẻ. 

Một lỗi ranh giới phổ biến là đếm những khoảng thời gian chỉ chạm nhau. Coi như:```
1 1
10 20 0 10
10 1
0 0
```Cuộc gọi chiếm`[0, 10]`, trong khi truy vấn chiếm`[10, 11]`. Giao điểm của chúng có thời gian bằng 0, vì vậy câu trả lời là`0`. Việc thực hiện bất cẩn bằng cách sử dụng`call_start <= query_end`Và`call_end >= query_start`sẽ đếm sai. 

Một trường hợp ranh giới khác xảy ra khi truy vấn nằm hoàn toàn bên trong một cuộc gọi:```
1 1
1 2 0 10
3 2
0 0
```Cuộc gọi được kích hoạt từ`0`ĐẾN`10`và truy vấn bao gồm`3`ĐẾN`5`, vậy câu trả lời là`1`. Bất kỳ phương thức nào chỉ kiểm tra xem cuộc gọi bắt đầu hay kết thúc bên trong truy vấn đều có thể bỏ sót trường hợp này, vì cả điểm cuối của cuộc gọi đều không nằm bên trong`[3, 5]`. 

Trường hợp thứ ba là một cuộc gọi hoàn toàn bên trong truy vấn:```
1 1
1 2 4 2
0 10
0 0
```Cuộc gọi chiếm`[4, 6]`và truy vấn chiếm`[0, 10]`, vậy câu trả lời lại là`1`. Điều này cho thấy tại sao sự chồng chéo phải được thể hiện thông qua các điểm cuối trong khoảng thời gian thay vì chỉ kiểm tra một hướng ngăn chặn. 

## Phương pháp tiếp cận 

Giải pháp đơn giản lưu trữ mọi cuộc gọi dưới dạng khoảng thời gian. Đối với mỗi truy vấn của cảnh sát`[L, R]`, nó sẽ quét tất cả các cuộc gọi và kiểm tra xem mỗi cuộc gọi có`[S, E]`thỏa mãn`S < R`Và`E > L`. Hai bất đẳng thức đó mô tả chính xác giao điểm có độ dài dương. Vì có nhiều nhất 9.999 cuộc gọi và 99 truy vấn, trường hợp xấu nhất là 989.901 lần kiểm tra khoảng thời gian cho mỗi trường hợp thử nghiệm. Nó đã đủ nhỏ đối với các ràng buộc nhất định và đó là một cách triển khai hoàn toàn hợp lý nếu sự đơn giản là mối quan tâm duy nhất. 

Cách tiếp cận nhanh hơn đến từ việc viết lại điều kiện chồng chéo. Cuộc gọi không bị chồng chéo`[L, R]`chính xác khi nó hoàn toàn ở bên trái hoặc hoàn toàn ở bên phải của truy vấn. Cuộc gọi hoàn toàn ở bên trái khi thời gian kết thúc của nó lớn nhất`L`. Cuộc gọi hoàn toàn ở bên phải khi thời gian bắt đầu ít nhất là`R`. 

Vì vậy số chúng ta muốn có thể được viết là`N - (# calls with end <= L) - (# calls with start >= R)`. 

Hai đại lượng này độc lập. Nếu tất cả thời gian kết thúc cuộc gọi được sắp xếp, tìm kiếm nhị phân sẽ cho số kết thúc nhỏ hơn hoặc bằng`L`. Nếu tất cả thời gian bắt đầu cuộc gọi được sắp xếp, một tìm kiếm nhị phân khác sẽ cho số lần bắt đầu lớn hơn hoặc bằng`R`. 

của Python`bisect_right`đưa ra chính xác số lượng giá trị`<= L`, trong khi`bisect_left`đưa ra số lượng giá trị`>= R`bằng cách trừ kết quả của nó khỏi`N`. Sự bất bình đẳng nghiêm ngặt là nguyên nhân khiến các cuộc gọi chạm đến điểm cuối biến mất khỏi câu trả lời. 

Chi phí tiền xử lý`O(N log N)`và mỗi truy vấn đều có giá`O(log N)`. Với ít hơn 100 truy vấn, việc này nhanh chóng và cũng có quy mô tốt hơn nhiều nếu số lượng truy vấn tăng lên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(NM)`|`O(N)`| Được chấp nhận cho các giới hạn đã nêu | 
| Tối ưu |`O(N log N + M log N)`|`O(N)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cuộc gọi và chuyển đổi từng cuộc gọi thành thời gian bắt đầu và thời gian kết thúc. Đối với cuộc gọi có bắt đầu`S`và thời lượng`D`, thời điểm kết thúc của nó là`S + D`. Chúng ta không cần số nguồn và số đích vì truy vấn chỉ hỏi về sự chồng chéo thời gian. 
2. Lưu trữ mọi cuộc gọi bắt đầu trong một mảng và mọi cuộc gọi kết thúc trong một mảng khác. Việc sắp xếp các mảng này chuyển câu hỏi về số lượng lệnh gọi nằm hoàn toàn trước hoặc sau một truy vấn thành các bài toán tìm kiếm nhị phân. 
3. Sắp xếp cả hai mảng. Sau khi sắp xếp, tất cả thời gian kết thúc nhỏ hơn hoặc bằng một giá trị cụ thể tạo thành một tiền tố và tất cả thời gian bắt đầu lớn hơn hoặc bằng một giá trị cụ thể tạo thành một hậu tố. 
4. Đối với truy vấn bắt đầu tại`L`với thời lượng`D`, tính thời điểm kết thúc của nó`R = L + D`. 
5. Sử dụng`bisect_right(ends, L)`đếm các cuộc gọi có thời gian kết thúc nhiều nhất`L`. Các cuộc gọi như vậy hoàn toàn kết thúc trước khi truy vấn bắt đầu, vì vậy chúng không có thời lượng chung với truy vấn. 
6. Sử dụng`bisect_left(starts, R)`để tìm cuộc gọi đầu tiên có thời gian bắt đầu ít nhất là`R`. có`N - bisect_left(starts, R)`những cuộc gọi như vậy. Các cuộc gọi này bắt đầu khi truy vấn đã kết thúc, vì vậy chúng cũng không có thời lượng chung. 
7. Trừ cả hai nhóm không chồng chéo khỏi`N`. Các cuộc gọi còn lại thỏa mãn`start < R`Và`end > L`, có nghĩa là chúng trùng lặp truy vấn trong một khoảng thời gian dương. In số này. 

### Tại sao nó hoạt động 

Đối với bất kỳ cuộc gọi nào`[S, E]`và truy vấn`[L, R]`, đúng một trong ba tình huống được áp dụng. Cuộc gọi hoàn toàn trước truy vấn khi`E <= L`, hoàn toàn sau truy vấn khi`S >= R`hoặc nó chồng chéo truy vấn khi không có điều kiện nào được đáp ứng. Hai nhóm đầu tiên không thể chồng lên nhau vì truy vấn có`L < R`. Do đó loại bỏ cả hai nhóm khỏi tất cả`N`cuộc gọi để lại chính xác những cuộc gọi thỏa mãn`S < R`Và`E > L`, chính xác là các cuộc gọi đang hoạt động trong ít nhất một giây của khoảng thời gian truy vấn. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

def solve():
    out = []

    while True:
        n, m = map(int, input().split())

        if n == 0 and m == 0:
            break

        starts = []
        ends = []

        for _ in range(n):
            source, destination, start, duration = map(int, input().split())
            starts.append(start)
            ends.append(start + duration)

        starts.sort()
        ends.sort()

        for _ in range(m):
            start, duration = map(int, input().split())
            end = start + duration

            finished_before = bisect_right(ends, start)
            started_after = n - bisect_left(starts, end)

            answer = n - finished_before - started_after
            out.append(str(answer))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào xử lý các trường hợp kiểm thử cho đến khi`0 0`. Đối với mỗi cuộc gọi, chỉ`start`Và`start + duration`được giữ lại vì số điện thoại không bao giờ tham gia vào truy vấn chồng chéo. 

Hai mảng được sắp xếp thể hiện hai cách mà một cuộc gọi có thể không trùng lặp một truy vấn.`bisect_right(ends, start)`bao gồm các kết thúc chính xác bằng với phần bắt đầu truy vấn. Đó là cố ý, bởi vì cuộc gọi kết thúc vào cùng thời điểm truy vấn bắt đầu không có giao điểm. Tương tự,`bisect_left(starts, end)`bao gồm các phần bắt đầu chính xác bằng phần cuối của truy vấn, phần này cũng phải được loại trừ khỏi câu trả lời. 

Phép trừ chỉ được thực hiện sau khi cả hai nhóm không chồng chéo đã được đếm. Không cần phải kiểm tra các cuộc gọi riêng lẻ cho từng truy vấn. 

Cũng không có vấn đề tràn số nguyên trong Python. Mặc dù các giới hạn ban đầu đảm bảo rằng điểm cuối phù hợp với số nguyên có dấu 32 bit, loại số nguyên của Python có thể biểu thị trực tiếp nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trường hợp thử nghiệm đầu tiên chứa ba cuộc gọi: 

| Gọi | Bắt đầu | Kết thúc | 
| --- | --- | --- | 
| 1 | 2 | 7 | 
| 2 | 0 | 10 | 
| 3 | 5 | 13 | 

Sau khi sắp xếp, mảng bắt đầu là`[0, 2, 5]`và mảng cuối cùng là`[7, 10, 13]`. 

Đối với truy vấn đầu tiên, khoảng thời gian là`[0, 6]`. 

| Truy vấn |`bisect_right(ends, L)`|`bisect_left(starts, R)`| Trả lời | 
| --- | --- | --- | --- | 
|`[0, 6]`| 0 | 3 | 3 | 

Không có cuộc gọi nào kết thúc đúng thời gian`0`và không có cuộc gọi nào bắt đầu vào hoặc sau thời gian`6`. Tất cả ba cuộc gọi chồng chéo truy vấn. 

Đối với truy vấn thứ hai, khoảng thời gian là`[8, 10]`. 

| Truy vấn |`bisect_right(ends, L)`|`bisect_left(starts, R)`| Trả lời | 
| --- | --- | --- | --- | 
|`[8, 10]`| 1 | 0 | 2 | 

Cuộc gọi đầu tiên đã kết thúc vào lúc`7`, nên nó bị loại trừ. Hai cuộc gọi còn lại trùng nhau`[8, 10]`, cho`2`. 

Kết quả đầu ra cho trường hợp thử nghiệm này là:```
3
2
```Dấu vết cho thấy lý do tại sao phần kết thúc chính xác ở ranh giới bắt đầu của truy vấn lại thuộc về tiền tố không chồng chéo. 

### Mẫu 2 

Trường hợp thử nghiệm thứ hai có một cuộc gọi bắt đầu lúc`0`với thời lượng`10`, vậy khoảng của nó là`[0, 10]`. 

Truy vấn đầu tiên là`[9, 10]`. 

| Truy vấn |`L`|`R`| Hoàn thành bởi`L`| Bắt đầu vào hoặc sau`R`| Trả lời | 
| --- | --- | --- | --- | --- | --- | 
|`[9, 10]`| 9 | 10 | 0 | 0 | 1 | 

Cuộc gọi chồng chéo truy vấn trong khoảng thời gian từ`9`ĐẾN`10`, vì vậy nó được tính. 

Truy vấn thứ hai là`[10, 11]`. 

| Truy vấn |`L`|`R`| Hoàn thành bởi`L`| Bắt đầu vào hoặc sau`R`| Trả lời | 
| --- | --- | --- | --- | --- | --- | 
|`[10, 11]`| 10 | 11 | 1 | 0 | 0 | 

Cuộc gọi kết thúc chính xác vào thời điểm bắt đầu của truy vấn. Không có giao điểm có độ dài dương nên không được tính. 

Kết quả đầu ra là:```
1
0
```Ví dụ này trực tiếp thực hiện các điều kiện biên nghiêm ngặt và nắm bắt được lỗi phổ biến khi coi các khoảng tiếp xúc là chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N log N + M log N)`| Sắp xếp hai mảng điểm cuối chi phí`O(N log N)`, và mỗi trong số`M`truy vấn sử dụng hai tìm kiếm nhị phân | 
| Không gian |`O(N)`| Mảng bắt đầu và kết thúc, mỗi mảng chứa một giá trị cho mỗi lệnh gọi | 

Trường hợp thử nghiệm lớn nhất chứa ít hơn 10.000 cuộc gọi, do đó việc sắp xếp không tốn kém. Mỗi truy vấn chỉ yêu cầu hai tìm kiếm nhị phân thay vì quét tất cả các cuộc gọi. Phương pháp này cũng tránh sự phụ thuộc vào độ lớn thực tế của dấu thời gian, do đó, một cuộc gọi kéo dài hàng nghìn giây không yêu cầu lặp lại trong những giây đó. 

## Trường hợp thử nghiệm```python
import sys
import io
from bisect import bisect_left, bisect_right

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    input = sys.stdin.readline
    out = []

    while True:
        n, m = map(int, input().split())

        if n == 0 and m == 0:
            break

        starts = []
        ends = []

        for _ in range(n):
            source, destination, start, duration = map(int, input().split())
            starts.append(start)
            ends.append(start + duration)

        starts.sort()
        ends.sort()

        for _ in range(m):
            start, duration = map(int, input().split())
            end = start + duration

            finished_before = bisect_right(ends, start)
            started_after = n - bisect_left(starts, end)

            out.append(str(n - finished_before - started_after))

    sys.stdout = old_stdout
    sys.stdin = old_stdin

    return "\n".join(out)

sample = """\
3 2
3 4 2 5
1 2 0 10
6 5 5 8
0 6
8 2
1 2
8 9 0 10
9 1
10 1
0 0
"""

assert solve_data(sample) == "3\n2\n1\n0", "provided sample"

minimum = """\
1 1
0 0 0 1
0 1
0 0
"""

assert solve_data(minimum) == "1", "minimum-size overlapping case"

touching = """\
1 3
1 2 10 5
0 10
15 1
16 1
0 0
"""

assert solve_data(touching) == "1\n0\n0", "endpoint touching"

equal_values = """\
4 3
1 1 5 5
2 2 5 5
3 3 5 5
4 4 5 5
5 5
0 5
10 1
0 0
"""

assert solve_data(equal_values) == "4\n4\n0", "equal starts and ends"

containment = """\
3 3
1 2 0 20
3 4 5 2
5 6 10 5
6 3
0 1
20 1
0 0
"""

assert solve_data(containment) == "3\n1\n0", "containment and boundaries"

large_endpoint = """\
2 2
1 2 0 10000
3 4 2147480000 10000
0 10000
2147480000 10000
0 0
"""

assert solve_data(large_endpoint) == "1\n1", "large timestamps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vỏ kích thước tối thiểu |`1`| Một cuộc gọi và một truy vấn | 
| Trường hợp cảm động |`1`,`0`,`0`| Xử lý điểm cuối chính xác | 
| Giá trị bằng nhau |`4`,`4`,`0`| Nhiều cuộc gọi có khoảng thời gian giống hệt nhau | 
| Trường hợp ngăn chặn |`3`,`1`,`0`| Cuộc gọi chứa truy vấn và cuộc gọi chứa truy vấn | 
| Trường hợp điểm cuối lớn |`1`,`1`| Dấu thời gian hợp lệ lớn và số học điểm cuối | 

## Vỏ cạnh 

Trường hợp ranh giới đầu tiên là cuộc gọi kết thúc chính xác khi truy vấn bắt đầu. Ví dụ:```
1 1
10 20 0 10
10 1
0 0
```Cuộc gọi kết thúc lúc`10`, trong khi truy vấn bắt đầu tại`10`.`bisect_right(ends, 10)`trả lại`1`, do đó cuộc gọi được phân loại là đã kết thúc trước hoặc chính xác ở ranh giới truy vấn. Câu trả lời là`1 - 1 - 0 = 0`. 

Trường hợp đối xứng là cuộc gọi bắt đầu chính xác khi truy vấn kết thúc:```
1 1
10 20 10 5
0 15
0 0
```Cuộc gọi là`[10, 15]`, và truy vấn là`[0, 15]`, vì vậy những khoảng thời gian này thực sự chồng lên nhau trước thời gian`15`và câu trả lời là`1`. Tìm kiếm nhị phân có liên quan là`bisect_left(starts, 15)`, trả về`1`, nhưng thao tác đó không xóa cuộc gọi vì cuộc gọi bắt đầu lúc`10`, đó là đúng trước`15`. 

Nếu thay vào đó truy vấn là`[0, 10]`:```
1 1
10 20 10 5
0 10
0 0
```sau đó`bisect_left(starts, 10)`trả lại`0`, cho`started_after = 1`. Cuộc gọi bắt đầu chính xác ở ranh giới kết thúc của truy vấn, vì vậy câu trả lời là`0`. 

Một truy vấn có thể hoàn toàn nằm trong một cuộc gọi:```
1 1
1 2 0 10
3 2
0 0
```Đây là cuộc gọi`[0, 10]`và truy vấn là`[3, 5]`. Không có cuộc gọi nào kết thúc trước`3`và không có cuộc gọi nào bắt đầu vào hoặc sau`5`, do đó công thức cho`1 - 0 - 0 = 1`. Phương thức này không yêu cầu điểm cuối của cuộc gọi phải nằm trong truy vấn. 

Việc ngăn chặn ngược lại được xử lý giống hệt nhau:```
1 1
1 2 3 2
0 10
0 0
```cuộc gọi`[3, 5]`nằm hoàn toàn bên trong`[0, 10]`. Một lần nữa, không có cuộc gọi nào thuộc nhóm không chồng chéo, vì vậy câu trả lời là`1`. 

Cuối cùng, một truy vấn có thể hoàn toàn nằm ngoài mọi cuộc gọi:```
2 1
1 2 0 2
3 4 5 2
2 1
0 0
```Các cuộc gọi là`[0, 2]`Và`[5, 7]`, trong khi truy vấn là`[2, 3]`. Cuộc gọi đầu tiên kết thúc chính xác vào lúc`2`, Vì thế`bisect_right`loại bỏ nó. Cuộc gọi thứ hai bắt đầu sau`3`, Vì thế`bisect_left`loại bỏ nó. Cả hai cuộc gọi đều bị loại trừ và câu trả lời là`0`. Đây chính xác là tình huống trong đó việc coi liên hệ điểm cuối là chồng chéo sẽ tạo ra kết quả không chính xác.

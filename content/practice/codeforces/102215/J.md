---
title: "CF 102215J - Sức mạnh của mặt tối - 2"
description: "Chúng ta có (n) Jedi và Jedi (i) có ba tham số nguyên không âm ((ai,bi,ci)). Trong một trận chiến thông thường, một Jedi thắng nếu ít nhất hai trong số ba tọa độ của anh ta lớn hơn tọa độ tương ứng của đối thủ."
date: "2026-08-23T18:50:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "J"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 2380
verified: false
draft: false
---

[CF 102215J - Sức mạnh của mặt tối - 2](https://codeforces.com/problemset/problem/102215/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có (n) Jedi và Jedi (i) có ba tham số nguyên không âm ((a_i,b_i,c_i)). Trong một trận chiến thông thường, một Jedi thắng nếu ít nhất hai trong số ba tọa độ của anh ta lớn hơn tọa độ tương ứng của đối thủ. 

Khả năng của mặt tối làm thay đổi vấn đề một cách đáng kể. Khi Jedi (i) được biến đổi, trước mỗi trận đấu, chúng ta có thể thay thế ba tham số của anh ta bằng ba số nguyên không âm bất kỳ mà tổng vẫn giữ nguyên (a_i+b_i+c_i). Các giá trị mới có thể được chọn độc lập cho mọi đối thủ. Đối với mỗi Jedi ban đầu, chúng ta cần đếm xem có bao nhiêu Jedi khác có thể bị đánh bại theo cách này. Vấn đề ban đầu và các ràng buộc của nó có sẵn trong kho lưu trữ của Codeforces. 

Do đó, câu hỏi trọng tâm không phải là ba giá trị cụ thể mà Jedi đã biến đổi nên sử dụng. Đối với một đối thủ cố định, chúng ta chỉ cần biết tổng công suất tối thiểu cần thiết để làm cho hai tọa độ lớn hơn tọa độ tương ứng của đối thủ đó. 

Với (n\le 500000), việc so sánh mỗi cặp sẽ cần khoảng (n(n-1)/2), tức là khoảng (1,25\times10^{11}) trận đấu ở kích thước tối đa. Giới hạn 2 giây loại trừ bất kỳ phương pháp bậc hai nào. Các tham số riêng lẻ có thể đạt tới (10^9), do đó tổng có thể đạt tới (3\times10^9), vừa vặn thoải mái với số nguyên 64 bit và cũng phù hợp với số nguyên có độ chính xác tùy ý của Python mà không cần xử lý đặc biệt. 

Có một số trường hợp khó xử lý. Vấn đề bất bình đẳng nghiêm ngặt. Ví dụ: với một Jedi có ((1,1,1)), câu trả lời là`0`: anh ta không thể tạo ra hai tọa độ lớn hơn một Jedi giống hệt khác bằng cách sử dụng tổng công suất (3), bởi vì làm như vậy sẽ cần tổng công suất ít nhất là (2+2=4). Giải pháp sử dụng (\ge) thay vì (>) sẽ tính sai trận đấu này. 

Cái bẫy phổ biến khác là tính Jedi đã biến hình chống lại chính anh ta. Coi như```
2
1 1 2
1 1 1
```Jedi đầu tiên có tổng sức mạnh (4). Chống lại ((1,1,1)), anh ta có thể chọn ((2,2,0)), vì vậy anh ta đánh bại Jedi thứ hai. Hai tọa độ nhỏ nhất của anh ấy là (1,1), nên ngưỡng cho bản ghi của anh ấy cũng là (4). Số ngưỡng thô sẽ bao gồm chính anh ta, tạo ra`2`thay vì câu trả lời đúng đầu tiên`1`. Đầu ra đúng là```
1 0
```Jedi thứ hai có tổng sức mạnh (3), không đủ để vượt quá hai tọa độ bằng (1). 

Mối quan hệ giữa các Jedi khác nhau không được loại bỏ. Nếu một số đối thủ có tổng tọa độ hai nhỏ nhất giống nhau thì mỗi đối thủ là một đối thủ riêng biệt. Ví dụ,```
3
2 2 2
1 1 1
1 1 1
```Jedi đầu tiên có tổng số (6) và có thể đánh bại cả Jedi khác. Mỗi bản sao của ((1,1,1)) có tổng (3) và không đánh bại ai. Đầu ra là```
2 0 0
```Giải pháp dựa trên tần số phải tính các ngưỡng trùng lặp thay vì xử lý các bộ ba tham số bằng nhau như một đối tượng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lấy từng cặp Jedi riêng biệt theo thứ tự. Đối với Jedi đang tấn công, hãy cố gắng tìm cách phân phối lại hợp pháp tổng sức mạnh của anh ta sao cho ít nhất hai tọa độ lớn hơn tọa độ tương ứng của đối thủ. Điều này đúng vì mọi đối thủ có thể có đều được xem xét độc lập và sự phân bổ lại mặt tối có thể thay đổi tùy theo từng trận đấu. 

Tuy nhiên, ngay cả khi việc kiểm tra một cặp được giảm xuống thời gian không đổi thì vẫn có (n(n-1)) cặp có thứ tự. Tại (n=500000), gần như là (2,5\time10^{11}) lượt kiểm tra cặp, vượt xa thời gian có sẵn. 

Quan sát hữu ích đến từ việc sắp xếp ba tham số của đối thủ. Giả sử đối thủ có giá trị 

[ 
x\le y\le z. 
] 

Để giành chiến thắng, Jedi đã biến đổi cần vượt quá hai giá trị bất kỳ trong số này ở vị trí tọa độ ban đầu của chúng. Vì kẻ tấn công có thể phân phối tổng sức mạnh của mình một cách tùy ý giữa ba tọa độ, nên lựa chọn rẻ nhất có thể là đánh bại hai giá trị nhỏ nhất. Anh ta có thể phân công 

[ 
x+1,\qquad y+1 
] 

tới hai tọa độ đó. Tọa độ còn lại nhận được toàn bộ năng lượng còn sót lại. 

Như vậy đối thủ có thể bị đánh bại chính xác khi tổng sức mạnh (S) của kẻ tấn công thỏa mãn 

[ 
S\ge (x+1)+(y+1)=x+y+2. 
] 

Tọa độ thứ ba không gây ra hạn chế bổ sung nào vì nó chỉ cần không âm. Nếu tổng ít nhất là (x+y+2), tất cả công suất còn lại có thể được đặt ở đó. 

Điều này làm giảm mọi đối thủ xuống một con số duy nhất: 

[ 
T=x+y+2. 
] 

Với mỗi Jedi (i), hãy tính tổng sức mạnh của anh ta 

[ 
S_i=a_i+b_i+c_i. 
] 

Anh ta có thể đánh bại chính xác những Jedi có ngưỡng (T) nhiều nhất là (S_i). Sau khi tính toán tất cả các ngưỡng, hãy sắp xếp chúng. Sau đó, số lượng ngưỡng nhiều nhất (S_i) thu được bằng một`upper_bound`hoặc của Python`bisect_right`. 

Có một sự điều chỉnh cuối cùng. Nếu ngưỡng riêng của Jedi (i) lớn nhất là (S_i), tìm kiếm nhị phân sẽ bao gồm chính Jedi (i). Vì câu trả lời yêu cầu Jedi khác, hãy trừ đi một trong trường hợp đó. Đây là điều chỉnh duy nhất cần thiết cho danh tính cụ thể. 

Do đó, toàn bộ vấn đề đã trở thành một vấn đề đếm ngoại tuyến tiêu chuẩn: biến mỗi đối thủ thành một yêu cầu vô hướng, sắp xếp tất cả các yêu cầu, sau đó trả lời một truy vấn giới hạn trên cho mỗi Jedi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi Jedi, hãy tính tổng sức mạnh của anh ta (S_i=a_i+b_i+c_i). Lưu trữ tổng số này vì đây là thuộc tính duy nhất của kẻ tấn công quan trọng sau khi đối thủ được chuyển thành ngưỡng. 
2. Sắp xếp ba tham số của mỗi Jedi và gọi hai giá trị nhỏ nhất (x_i) và (y_i). Xác định 

[ 
T_i=x_i+y_i+2. 
] 

Đây là tổng sức mạnh tối thiểu mà một Jedi khác cần để đánh bại Jedi (i). các`+2`là bắt buộc vì cả hai tọa độ phải lớn hơn rất nhiều, do đó việc đánh bại (x_i) tốn ít nhất (x_i+1) và việc đánh bại (y_i) tốn ít nhất (y_i+1). 
3. Đặt mọi (T_i) vào một mảng và sắp xếp mảng đó. Sau khi sắp xếp, tất cả các đối thủ có thể có được thể hiện bằng sức mạnh yêu cầu tối thiểu của họ. 
4. Với mỗi Jedi (i), hãy sử dụng`bisect_right`để tìm có bao nhiêu ngưỡng thỏa mãn 

[ 
T_j\le S_i. 
] 

Ranh giới chia đôi bên phải là cần thiết vì sự bình đẳng được cho phép đối với tổng yêu cầu. Nếu (S_i=T_j), kẻ tấn công có thể chi tiêu chính xác số tiền cần thiết. 
5. Kiểm tra xem (T_i\le S_i) có hay không. Nếu vậy, tìm kiếm nhị phân sẽ tính chính Jedi (i), vì vậy hãy trừ đi một. Nếu (T_i>S_i), anh ta không được tính và không cần phải xóa gì. 
6. In kết quả đếm theo thứ tự Jedi ban đầu. Việc chỉ sắp xếp mảng ngưỡng riêng biệt không thay đổi thứ tự của các tổng được lưu trữ hoặc ngưỡng theo Jedi, do đó mỗi câu trả lời vẫn được liên kết với Jedi ban đầu của nó. 

Bất biến chính là (T_j) chính xác là tổng sức mạnh tối thiểu cần thiết để đánh bại Jedi (j). Do đó, sau khi sắp xếp tất cả (T_j), số đối thủ mà Jedi (i) có thể đánh bại chính xác là số ngưỡng không lớn hơn (S_i). Ngưỡng duy nhất không đại diện cho đối thủ hợp lệ là chính (T_i) và nó bị xóa chính xác khi được đưa vào. Do đó, mọi Jedi được tính đều có thể bị đánh bại, mọi Jedi bị bỏ qua đều không thể bị đánh bại và việc tự đếm sẽ bị loại trừ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from bisect import bisect_right

def solve():
    n = int(input())

    totals = [0] * n
    thresholds = [0] * n

    for i in range(n):
        a, b, c = map(int, input().split())

        totals[i] = a + b + c

        if a > b:
            a, b = b, a
        if b > c:
            b, c = c, b
        if a > b:
            a, b = b, a

        thresholds[i] = a + b + 2

    sorted_thresholds = sorted(thresholds)

    answer = [0] * n

    for i in range(n):
        count = bisect_right(sorted_thresholds, totals[i])

        if thresholds[i] <= totals[i]:
            count -= 1

        answer[i] = count

    sys.stdout.write(" ".join(map(str, answer)))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào tính toán hai mẩu thông tin cho mỗi Jedi.`totals[i]`duy trì tổng công suất ban đầu, trong khi`thresholds[i]`ghi lại sức mạnh tối thiểu cần thiết để đánh bại Jedi đó. 

Sắp xếp ba giá trị được thực hiện bằng ba phép so sánh thay vì gọi`sorted`trong danh sách tạm thời. Vì chỉ có ba tọa độ nên đây là công việc liên tục của Jedi và tránh các đối tượng tạm thời không cần thiết trong Python. Sau ba lần so sánh,`a`Và`b`là hai giá trị nhỏ nhất nên`a + b + 2`là ngưỡng cần thiết. 

Sự riêng biệt`sorted_thresholds`mảng là có chủ ý. Chúng tôi cần các ngưỡng theo thứ tự được sắp xếp để tìm kiếm nhị phân, nhưng chúng tôi cũng cần ngưỡng ban đầu của mỗi Jedi để quyết định xem tìm kiếm nhị phân có tính Jedi đó hay không. của Python`sorted`tạo một danh sách mới, rời khỏi`thresholds[i]`liên kết với Jedi (i).`bisect_right`trả về vị trí đầu tiên lớn hơn`totals[i]`. Mọi ngưỡng trước vị trí đó nhiều nhất là tổng của kẻ tấn công, kể cả các ngưỡng bằng tổng. Điều đó phù hợp chính xác với điều kiện khả thi. 

Công dụng tự sửa lỗi`thresholds[i] <= totals[i]`, không`<`. Bình đẳng có nghĩa là Jedi có tổng sức mạnh chính xác để đáp ứng ngưỡng của chính mình, vì vậy mục nhập của chính anh ta đã được đưa vào và phải bị loại bỏ. 

Không cần xử lý tràn trong Python. Tổng số lớn nhất là (3\times10^9) và số nguyên Python biểu thị trực tiếp số đó. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, bốn Jedi có tổng và ngưỡng như sau. 

| Jedi | Sắp xếp thông số | Tổng (S_i) | Ngưỡng (T_i) | 
| --- | --- | --- | --- | 
| 1 | (1,3,4) | 8 | 6 | 
| 2 | (2,5,9) | 16 | 9 | 
| 3 | (3,6,10) | 19 | 11 | 
| 4 | (2,3,5) | 10 | 7 | 

Sau khi sắp xếp, mảng ngưỡng là`[6, 7, 9, 11]`. 

| Jedi | Tổng cộng |`bisect_right([6,7,9,11], total)`| Tự bao gồm? | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 8 | 2 | Có, ngưỡng 6 | 1 | 
| 2 | 16 | 4 | Có, ngưỡng 9 | 3 | 
| 3 | 19 | 4 | Có, ngưỡng 11 | 3 | 
| 4 | 10 | 3 | Có, ngưỡng 7 | 2 | 

Ví dụ: Jedi 1 có tổng số (8). Anh ta có thể đánh bại ngưỡng (6) và (7), tương ứng với Jedi 1 và Jedi 4. Việc loại bỏ bản thân sẽ để lại một đối thủ hợp lệ. Đầu ra cuối cùng là`1 3 3 2`, phù hợp với mẫu 

Đối với ví dụ thứ hai, hãy xem xét```
3
0 0 0
1 1 1
1 2 2
```Quá trình tiền xử lý tạo ra trạng thái sau. 

| Jedi | Sắp xếp thông số | Tổng cộng | Ngưỡng | 
| --- | --- | --- | --- | 
| 1 | (0,0,0) | 0 | 2 | 
| 2 | (1,1,1) | 3 | 4 | 
| 3 | (1,2,2) | 5 | 4 | 

Các ngưỡng được sắp xếp là`[2, 4, 4]`. 

| Jedi | Tổng cộng | Tổng số ngưỡng (\le) | Tự bao gồm? | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | Không | 0 | 
| 2 | 3 | 1 | Không | 1 | 
| 3 | 5 | 3 | Có | 2 | 

Jedi 2 có thể đánh bại Jedi 1 vì chi tiêu (1) cho một tọa độ và (1) cho tọa độ khác là đủ để vượt quá hai số không. Anh ta không thể đánh bại Jedi 3 vì điều đó đòi hỏi phải vượt quá (1) và (2), tốn (2+3=5). Jedi 3 có tổng sức mạnh chính xác đủ để đánh bại cả hai Jedi khác, bao gồm cả ngưỡng thuộc về chính anh ta nên việc tự tính bị loại bỏ. Kết quả đầu ra là`0 1 2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Việc tính toán tất cả các giá trị lấy (O(n)), ngưỡng sắp xếp (n) lấy (O(n\log n)) và tất cả các tìm kiếm nhị phân cùng nhau lấy (O(n\log n)). | 
| Không gian | (O(n)) | Tổng số ban đầu, ngưỡng, mảng ngưỡng được sắp xếp và mảng câu trả lời đều chứa (n) giá trị. | 

Với (n=500000), thuật toán thực hiện một (O(n\log n)) sắp xếp và (n) tìm kiếm nhị phân logarit thay vì hàng trăm tỷ so sánh cặp. Dữ liệu được lưu trữ là tuyến tính theo (n), phù hợp thoải mái trong giới hạn bộ nhớ 256 MB khi được biểu thị bằng các đối tượng danh sách và số nguyên Python. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng tương tự`solve`hoạt động như giải pháp được gửi. Người trợ giúp chuyển hướng`sys.stdin`và chụp`sys.stdout`, do đó các xác nhận thực hiện việc triển khai thực tế thay vì triển khai lại riêng biệt.```python
import sys
import io
from bisect import bisect_right

def solve(inp):
    n = int(inp.readline())

    totals = [0] * n
    thresholds = [0] * n

    for i in range(n):
        a, b, c = map(int, inp.readline().split())
        totals[i] = a + b + c

        if a > b:
            a, b = b, a
        if b > c:
            b, c = c, b
        if a > b:
            a, b = b, a

        thresholds[i] = a + b + 2

    sorted_thresholds = sorted(thresholds)

    answer = []
    for i in range(n):
        count = bisect_right(sorted_thresholds, totals[i])
        if thresholds[i] <= totals[i]:
            count -= 1
        answer.append(count)

    return " ".join(map(str, answer))

def run(inp: str) -> str:
    return solve(io.StringIO(inp))

# Provided sample
assert run("""\
4
1 3 4
2 5 9
6 10 3
5 2 3
""") == "1 3 3 2", "sample 1"

# Minimum-size input
assert run("""\
1
0 0 0
""") == "0", "single Jedi"

# All equal values
assert run("""\
3
1 1 1
1 1 1
1 1 1
""") == "0 0 0", "equal Jedi cannot beat each other"

# Exact threshold boundary and self exclusion
assert run("""\
2
1 1 2
1 1 1
""") == "1 0", "exact required total"

# Duplicated opponents and zero boundary
assert run("""\
3
2 2 2
1 1 1
1 1 1
""") == "2 0 0", "duplicate thresholds"

# Maximum-size stress case
n = 500000
inp = str(n) + "\n" + ("1 1 1\n" * n)
expected = " ".join(["0"] * n)
assert run(inp) == expected, "maximum n"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 0 0`|`0`| Tối thiểu (n) và không tự tính. | 
| Ba bản sao của`1 1 1`|`0 0 0`| Giá trị bình đẳng và bất bình đẳng nghiêm ngặt. | 
|`1 1 2`so với`1 1 1`|`1 0`| Ngưỡng bình đẳng chính xác và loại bỏ chính Jedi đang tấn công. | 
|`2 2 2`so với hai bản sao của`1 1 1`|`2 0 0`| Tất cả các ngưỡng trùng lặp phải được tính riêng. | 
| 500000 bản sao`1 1 1`| 500000 số không | Kích thước đầu vào tối đa và tiền xử lý bộ nhớ tuyến tính. | 

## Vỏ cạnh 

Ranh giới bất đẳng thức chặt chẽ được xử lý bằng cách thêm một vào cả hai tọa độ đối thủ nhỏ nhất. Vì```
2
1 1 2
1 1 1
```Jedi thứ hai có ngưỡng (1+1+2=4), trong khi Jedi thứ nhất có tổng (4). Vì (4\ge4), Jedi đầu tiên có thể chọn hai tọa độ bằng (2) và đánh bại Jedi thứ hai. Tìm kiếm nhị phân bao gồm ngưỡng vì nó sử dụng`bisect_right`. Nó cũng bao gồm ngưỡng riêng của Jedi đầu tiên, được loại bỏ bằng cách tự kiểm tra, đưa ra`1 0`. 

Đối thủ có số 0 thực hiện ranh giới dưới. Vì```
2
0 0 0
1 1 1
```ngưỡng đầu tiên là (2), trong khi Jedi thứ hai có tổng (3). Do đó, Jedi thứ hai có thể đánh bại Jedi thứ nhất bằng cách phân bổ ít nhất (1) cho hai tọa độ. Jedi đầu tiên có tổng (0) nên anh ta không thể đạt ngưỡng (4) cho Jedi thứ hai. Đầu ra là`0 1`. 

Đối thủ trùng lặp phải giữ hồ sơ riêng biệt. Vì```
3
2 2 2
1 1 1
1 1 1
```các ngưỡng là (6,4,4) và Jedi đầu tiên có tổng số (6).`bisect_right`trả về cả ba ngưỡng, bao gồm cả bản sao của (4) và bản sao của Jedi đầu tiên (6). Trừ một để lại chính xác hai đối thủ, do đó kết quả là`2 0 0`. 

Cuối cùng, một bộ tham số giống hệt nhau không có nghĩa là hai Jedi có thể đánh bại lẫn nhau. Với```
3
1 1 1
1 1 1
1 1 1
```mọi tổng số là (3), trong khi mọi ngưỡng là (1+1+2=4). Không có tìm kiếm nhị phân nào trả về số dương cho bất kỳ Jedi nào, vì vậy đầu ra là`0 0 0`. Đây chính xác là lý do tại sao ngưỡng này phải thể hiện tính ưu việt tuyệt đối hơn là sự so sánh không nghiêm ngặt.

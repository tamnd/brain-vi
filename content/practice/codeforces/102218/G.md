---
title: "CF 102218G - Tạo sự cố"
description: "Chúng tôi có hai lịch trình cuộc thi được đề xuất. Lịch trình của Filiberto là một mảng (d) có độ dài (n) và lịch trình của Abraham là một mảng (x) có độ dài (m). Mỗi mục là độ khó của một bài toán và vị trí của mục là cố định vì không thể sắp xếp lại lịch trình."
date: "2026-08-20T03:24:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "G"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 474
verified: false
draft: false
---

[CF 102218G - Tạo sự cố](https://codeforces.com/problemset/problem/102218/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 54 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai lịch trình cuộc thi được đề xuất. Lịch trình của Filiberto là một mảng (d) có độ dài (n) và lịch trình của Abraham là một mảng (x) có độ dài (m). Mỗi mục là độ khó của một bài toán và vị trí của mục là cố định vì không thể sắp xếp lại lịch trình. 

Nếu chúng ta chọn độ khó (v), chúng ta phải chọn một bài toán có độ khó (v) từ mỗi mảng. Hai bài toán được chọn tạo thành một cặp trong tập thi cuối cùng. Các giá trị độ khó đã chọn phải tăng nghiêm ngặt từ cặp này sang cặp tiếp theo, trong khi các vị trí đã chọn phải tiếp tục tăng bên trong cả hai mảng ban đầu. 

Vì vậy, nếu những khó khăn được lựa chọn là 

[ 
v_1 < v_2 < \dots < v_k, 
] 

thì phải có vị trí 

[ 
i_1 < i_2 < \dots < i_k 
] 

trong mảng đầu tiên và 

[ 
j_1 < j_2 < \dots < j_k 
] 

trong mảng thứ hai sao cho 

[ 
d_{i_t}=x_{j_t}=v_t. 
] 

Mỗi khó khăn được lựa chọn đóng góp hai vấn đề thực tế, một vấn đề từ mỗi đề xuất. Nếu chuỗi dài nhất có thể chứa (k) độ khó thì đầu ra yêu cầu là (2k). 

Đây chính xác là bài toán về Dãy số tăng chung dài nhất, ngoại trừ việc đáp án cuối cùng phải được nhân với hai. 

Độ dài mảng đều có thể đạt tới (10^4), do đó có thể có (10^8) cặp vị trí. Một thuật toán liệt kê rõ ràng tất cả các cặp đã có kích thước lớn trong C++ và việc tìm kiếm theo cấp số nhân là hoàn toàn không thể. Giải pháp quy hoạch động tiêu chuẩn sử dụng thời gian (O(nm)) và bộ nhớ (O(\min(n,m))), đây là giải pháp bậc hai thực tế cho bài toán này. Các giá trị độ khó có thể lớn bằng (10^9), nhưng độ lớn của chúng không ảnh hưởng đến DP vì chỉ cần sự bình đẳng và so sánh. 

Một số trường hợp đặc biệt có thể dễ dàng gây ra câu trả lời sai. Đầu tiên, những khó khăn lặp đi lặp lại như nhau không thể đếm hết được. Ví dụ,```
1 1
5
5
```có câu trả lời`2`, vì một bài toán có độ khó 5 được chọn từ mỗi mảng. Coi cặp lặp lại như một chuỗi tăng dần của hai độ khó sẽ tạo ra kết quả không chính xác`4`. 

Thứ hai, gặp nhiều khó khăn chung vẫn chưa đủ. Vị trí của chúng phải đồng nhất trong cả hai mảng. Ví dụ,```
2 2
1 2
2 1
```có câu trả lời`2`. Độ khó 1 và độ khó 2 đều có ở cả hai mảng, nhưng việc chọn chúng theo độ khó tăng dần đòi hỏi phải có 1 trước 2 ở cả hai mảng, điều này là không thể. 

Thứ ba, hai mảng có thể chứa cùng một độ khó nhiều lần và lần xuất hiện tốt nhất phụ thuộc vào nội dung đã được chọn trước đó. Ví dụ,```
4 4
2 3 1 4
1 2 3 4
```có câu trả lời`6`, sử dụng khó khăn (2,3,4). Một chiến lược tham lam luôn chọn độ khó nhỏ nhất hiện có sẽ chọn 1 trước và chỉ đạt được hai độ khó là 1 và 4. 

Cuối cùng, nếu các mảng không có khó khăn chung nào cả thì câu trả lời là 0. Vì```
2 2
1 2
3 4
```không có gì có thể được ghép nối, vì vậy đầu ra chính xác là`0`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ tạo ra mọi chuỗi con của mảng đầu tiên và mọi chuỗi con của mảng thứ hai, sau đó kiểm tra xem cặp nào tạo thành chuỗi tăng nghiêm ngặt giống nhau. Có (2^n) dãy con trong một mảng và (2^m) trong mảng kia, do đó, ngay cả khi chỉ xem xét các cặp dãy con ứng cử viên cũng có thể có (\Theta(2^{n+m})). Ở kích thước tối đa, giá trị này trở thành (2^{20000}), vượt xa mọi thứ có thể thực thi được. 

Quan sát hữu ích là chúng ta không cần phải nhớ toàn bộ chuỗi đã chọn. Giả sử chúng ta xử lý mảng đầu tiên từ trái sang phải. Đối với mọi vị trí (j) trong mảng thứ hai, chúng ta có thể lưu trữ độ dài tốt nhất của chuỗi tăng dần chung kết thúc chính xác tại (x_j). Khi giá trị hiện tại (d_i) được xem xét, mọi (x_j) trước đó với (x_j<d_i) đều là giá trị tiền thân hợp lệ. Trong số tất cả các vị trí như vậy, chúng ta chỉ cần giá trị DP tối đa. Nếu (d_i=x_j), chúng ta có thể thêm (d_i) vào tiền thân tốt nhất đó. 

Phần tinh tế là xử lý mảng thứ hai từ trái sang phải. Chúng tôi duy trì một biến`best`, biểu thị giá trị DP tối đa trong số các vị trí đã truy cập có giá trị nhỏ hơn giá trị hiện tại (d_i). Khi (d_i=x_j), ứng viên mới là`best + 1`. Khi (x_j<d_i),`dp[j]`trở thành đủ điều kiện với tư cách là người tiền nhiệm và có thể cập nhật`best`. Khi (x_j>d_i), nó không thể là phần trước vì chuỗi độ khó thu được sẽ không tăng lên một cách nghiêm ngặt. 

DP được cập nhật trong khi quét mảng thứ hai, do đó các vị trí trong mảng đó sẽ tự động được tôn trọng. Vòng lặp bên ngoài quét mảng đầu tiên nên các vị trí ở đó cũng được tôn trọng. Các giá trị bằng nhau không bao giờ được đưa vào`best`, đưa ra thứ tự tăng chặt chẽ thay vì thứ tự không giảm. 

Thuật toán kết quả là kỹ thuật lập trình động LCIS một chiều cổ điển. Chúng ta chỉ cần độ dài chứ không cần chuỗi độ khó thực tế, vì vậy toàn bộ bảng (n\times m) có thể được nén thành một mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^{n+m})) hoặc tệ hơn | Hàm mũ | Quá chậm | 
| Tối ưu | (O(nm)) | (O(\min(n,m))) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ hai mảng độ khó. Chúng ta có thể tùy ý đặt mảng đầu tiên thành mảng ngắn hơn vì mảng DP có một mục nhập cho mỗi phần tử của mảng thứ hai. Điều này không thay đổi độ chính xác và giảm bộ nhớ khi kích thước đầu vào khác nhau. 
2. Tạo`dp[j]`, Ở đâu`dp[j]`là độ dài tối đa của một dãy tăng nghiêm ngặt chung có độ khó được chọn cuối cùng là`b[j]`. Số 0 có nghĩa là hiện tại không có chuỗi hợp lệ nào kết thúc ở đó. 
3. Xử lý mọi giá trị`a[i]`từ trái sang phải. Trước khi quét mảng thứ hai, hãy đặt`best = 0`. Biến này sẽ chứa độ dài chuỗi tốt nhất kết thúc ở vị trí đã được xử lý của mảng thứ hai có độ khó nhỏ hơn`a[i]`. 
4. Quét`b`từ trái sang phải. Nếu như`b[j] < a[i]`, sau đó`b[j]`có thể đi trước`a[i]`, vậy hãy cập nhật`best`với`dp[j]`. 
5. Nếu`b[j] == a[i]`, thì hai phần tử hiện tại có thể được so khớp. Chuỗi tốt nhất kết thúc ở giá trị này có độ dài`best + 1`, vậy hãy cập nhật`dp[j]`với giá trị đó. 
6. Nếu`b[j] > a[i]`, không có gì thay đổi. Giá trị như vậy không thể đặt trước`a[i]`theo một trình tự tăng dần chặt chẽ. 
7. Sau khi tất cả các phần tử của mảng đầu tiên đã được xử lý, giá trị lớn nhất trong`dp`là chiều dài (k) của LCIS. Mỗi độ khó được chọn tương ứng với một bài toán được chọn từ mỗi mảng, do đó hãy in`2 * k`. 

Bất biến quan trọng là, tại mọi điểm trong vòng lặp bên ngoài,`dp[j]`biểu thị chuỗi hợp lệ tốt nhất có thể thu được từ tiền tố đã xử lý của mảng đầu tiên và các tiền tố đã xử lý có liên quan đến vị trí (j) của mảng thứ hai. Trong khi quét một hàng,`best`chứa chính xác giá trị tối đa`dp[j]`giữa các vị trí có độ khó nhỏ hơn giá trị mảng đầu tiên hiện tại. Do đó, mọi chuyển đổi đều xem xét chính xác các phần trước hợp lệ và không có phần trước không hợp lệ. Vì mọi LCIS có thể đều có thể được xem bằng cặp khớp cuối cùng của nó, nên cuối cùng DP sẽ xem xét cặp cuối cùng tối ưu, do đó độ dài được lưu trữ tối đa chính xác là số độ khó tối ưu đã chọn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    # Keep the DP array on the shorter sequence.
    if len(b) > len(a):
        a, b = b, a

    dp = [0] * len(b)
    answer = 0

    for x in a:
        best = 0

        for j, y in enumerate(b):
            if y < x:
                if dp[j] > best:
                    best = dp[j]
            elif y == x:
                candidate = best + 1
                if candidate > dp[j]:
                    dp[j] = candidate
                    if candidate > answer:
                        answer = candidate

    print(answer * 2)

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc hai lịch trình được đề xuất. Hoán đổi các mảng khi cần thiết là an toàn vì vấn đề có tính đối xứng: một dãy con tăng chung phải tôn trọng cả hai mảng bất kể mảng nào được gọi là mảng đầu tiên. 

các`dp`mảng lưu trữ một giá trị cho mọi vị trí của mảng ngắn hơn. Vòng lặp bên ngoài khắc phục sự cố từ mảng khác, trong khi vòng lặp bên trong kiểm tra các vị trí có thể khớp trong mảng thứ hai. 

Thứ tự của hai điều kiện bên trong vòng lặp bên trong là rất quan trọng. Vì`y < x`,`dp[j]`trở thành ứng cử viên cho`best`. Vì`y == x`, giá trị hiện tại được khớp bằng cách sử dụng`best`được tích lũy từ các giá trị nhỏ hơn. Bởi vì`best`chỉ được cập nhật cho các giá trị nhỏ hơn, một giá trị bằng nhau không bao giờ có thể mở rộng một giá trị bằng nhau khác. Đó chính xác là những gì điều kiện tăng nghiêm ngặt yêu cầu. 

Chúng tôi không cập nhật`best`sau khi xử lý một giá trị bằng nhau. Làm như vậy sẽ cho phép sử dụng giá trị hiện tại làm giá trị tiền thân cho một lần xuất hiện khác có cùng giá trị trong cùng một lần quét, điều này sẽ biến bất đẳng thức nghiêm ngặt thành bất đẳng thức không nghiêm ngặt một cách không chính xác. 

bản cập nhật`dp[j] = max(dp[j], best + 1)`cũng cần thiết vì có thể đạt được cùng một vị trí trong mảng thứ hai từ các tiền tố khác nhau của mảng thứ nhất. Chúng tôi giữ lại khả năng mạnh nhất. 

Câu trả lời cuối cùng được nhân với hai vì`dp`đếm các giá trị độ khó đã chọn, trong khi bài toán yêu cầu tổng số bài toán đã chọn. Mỗi độ khó trong dãy tăng dần chung đều đóng góp chính xác một vấn đề cho mỗi đề xuất. 

Số nguyên Python không tràn cho các giá trị này và giá trị DP tối đa là (10^4). 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, các mảng là```
A = [1, 2, 1, 2, 1, 3]
B = [2, 1, 3, 2, 1]
```Trình tự theo dõi DP kết thúc ở vị trí của`B`. 

| Một giá trị | Vị trí B | Giá trị B | tốt nhất trước trận đấu | dp sau khi xử lý | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 |`[0,0,0,0,0]`| 
| 1 | 2 | 1 | 0 |`[0,1,0,0,0]`| 
| 1 | 3 | 3 | 0 |`[0,1,0,0,0]`| 
| 1 | 4 | 2 | 0 |`[0,1,0,0,0]`| 
| 1 | 5 | 1 | 0 |`[0,1,0,0,0]`| 
| 2 | 1 | 2 | 0 |`[1,1,0,0,0]`| 
| 2 | 2 | 1 | 0 |`[1,1,0,0,0]`| 
| 2 | 3 | 3 | 1 |`[1,1,0,0,0]`| 
| 2 | 4 | 2 | 1 |`[1,1,0,2,0]`| 
| 2 | 5 | 1 | 1 |`[1,1,0,2,0]`| 
| 1 | 1..5 | hỗn hợp | 0 |`[1,1,0,2,0]`| 
| 3 | 1..5 | hỗn hợp | 2 |`[1,1,3,2,0]`| 

Giá trị DP tối đa là 2, tương ứng với dãy độ khó (1,3). Mỗi khó khăn trong số hai khó khăn đó đều đóng góp vào hai vấn đề được chọn, do đó kết quả đầu ra là (2\cdot2=4). 

Đối với ví dụ thứ hai, hãy xem xét```
5 4
1 2 3 4 5
1 3 2 4
```Dãy tăng chung tốt nhất là (1,3,4). 

| Một giá trị | Kết quả quét B | tốt nhất | Cập nhật dp có liên quan | Tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | 1 trận đấu, giá trị sau lớn hơn | 0 |`dp[0] = 1`| 1 | 
| 2 | 1 nhỏ hơn, 2 vắng mặt | 1 | không | 1 | 
| 3 | 1 nhỏ hơn, 3 trận đấu | 1 |`dp[1] = 2`| 2 | 
| 4 | 1, 3, 2 được quét, 3 cho kết quả tốt nhất 2 | 2 |`dp[3] = 3`| 3 | 
| 5 | tất cả các giá trị liên quan đều nhỏ hơn | 3 | không phù hợp 5 | 3 | 

Độ dài LCIS là 3, do đó có thể chọn sáu bài toán. Trình tự này khả thi trong cả hai lịch trình ban đầu vì các vị trí được chọn là (1,2,4) trong mảng đầu tiên và (1,2,4) trong mảng thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm)) | Mọi phần tử của một mảng được so sánh với mọi phần tử của mảng kia một lần | 
| Không gian | (O(\min(n,m))) | DP chứa một giá trị cho mỗi phần tử của mảng ngắn hơn | 

Với (n,m\le10^4), DP trong trường hợp xấu nhất thực hiện (10^8) lần lặp vòng lặp bên trong. Thuật toán chỉ sử dụng bộ nhớ tuyến tính nên mức tiêu thụ bộ nhớ dễ dàng trong khoảng 256 MB. Thời gian bậc hai là ràng buộc lập trình động LCIS tiêu chuẩn và là cấu trúc dự định của giải pháp. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(data: str) -> str:
    it = iter(data.split())
    n = int(next(it))
    m = int(next(it))

    a = [int(next(it)) for _ in range(n)]
    b = [int(next(it)) for _ in range(m)]

    if len(b) > len(a):
        a, b = b, a

    dp = [0] * len(b)
    answer = 0

    for x in a:
        best = 0

        for j, y in enumerate(b):
            if y < x:
                if dp[j] > best:
                    best = dp[j]
            elif y == x:
                candidate = best + 1
                if candidate > dp[j]:
                    dp[j] = candidate
                    if candidate > answer:
                        answer = candidate

    return str(answer * 2)

def run(inp: str) -> str:
    return solution(inp).strip()

# Provided sample
assert run("""\
6 5
1 2 1 2 1 3
2 1 3 2 1
""") == "4", "sample 1"

# Second worked example
assert run("""\
5 4
1 2 3 4 5
1 3 2 4
""") == "6", "increasing common sequence 1,3,4"

# Minimum-size input
assert run("""\
1 1
7
7
""") == "2", "one matched difficulty gives two problems"

# No common difficulty
assert run("""\
2 2
1 2
3 4
""") == "0", "no common values"

# All values equal
assert run("""\
3 4
7 7 7
7 7 7 7
""") == "2", "strict increase forbids using the same difficulty twice"

# Greedy counterexample and boundary value 1e9
assert run("""\
4 4
2 3 1 1000000000
1 2 3 1000000000
""") == "6", "best sequence is 2,3,1000000000"

# Maximum-size input
n = 10000
a = "5 " * n
b = "5 " * n
max_case = f"{n} {n}\n{a}\n{b}\n"
assert run(max_case) == "2", "maximum-size all-equal arrays"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7 / 7`|`2`| Ví dụ kích thước tối thiểu và hệ số hai | 
|`2 2 / 1 2 / 3 4`|`0`| Không có khó khăn chung | 
|`3 4 / 7 7 7 / 7 7 7 7`|`2`| Tăng cường xử lý nghiêm ngặt và trùng lặp | 
|`4 4 / 2 3 1 1000000000 / 1 2 3 1000000000`|`6`| Thứ tự vị trí, bẫy tham lam và ranh giới (10^9) | 
|`10000 10000 / all 5 / all 5`|`2`| Kích thước đầu vào tối đa và giá trị lặp lại | 

## Vỏ cạnh 

Khi cả hai mảng chỉ chứa một vấn đề giống hệt nhau, chẳng hạn như```
1 1
7
7
```DP bắt đầu bằng tất cả số không. Khi giá trị đầu tiên và duy nhất khớp nhau,`best`bằng 0, do đó vị trí phù hợp sẽ nhận được`dp = 1`. LCIS ​​chứa một độ khó và thuật toán sẽ in ra (1\cdot2=2). 

Khi không có khó khăn nào xảy ra ở cả hai mảng,```
2 2
1 2
3 4
```không có nhánh bình đẳng nào đạt được. Mọi`dp`mục nhập vẫn bằng 0, do đó độ dài LCIS tối đa bằng 0 và câu trả lời cuối cùng là`0`. 

Đối với các giá trị bằng nhau lặp lại,```
3 4
7 7 7
7 7 7 7
```cái đầu tiên`7`tạo ra một chuỗi có độ dài một. Mỗi lần sau`7`không thấy giá trị nào nhỏ hơn, vì điều kiện`y < x`là sai đối với người khác`7`. Do đó, không có quá trình chuyển đổi nào có thể tạo ra độ dài hai. Độ dài LCIS cuối cùng là một và câu trả lời là`2`. 

Vấn đề đặt hàng xuất hiện trong```
4 4
2 3 1 1000000000
1 2 3 1000000000
```Chuỗi tăng chung (2,3,10^9) là hợp lệ. Mảng đầu tiên sử dụng các vị trí (1,2,4), trong khi mảng thứ hai sử dụng các vị trí (2,3,4). giá trị`1`cũng phổ biến, nhưng việc chọn nó đầu tiên sẽ đặt sự xuất hiện của nó trong mảng đầu tiên ở vị trí 3, khiến mảng sau`2`Và`3`không có sẵn. DP không tham lam cam kết`1`; nó giữ tất cả các trạng thái liên quan và tìm độ dài thứ ba, tạo ra`6`. 

Cơ chế tương tự xử lý trường hợp giá trị lặp lại kích thước tối đa. Với 10.000 bản`5`trong mỗi mảng, lần xuất hiện trùng khớp đầu tiên mang lại độ dài bằng một, trong khi mọi lần xuất hiện bằng nhau tiếp theo đều bị ngăn không cho mở rộng nó vì`best`chỉ kết hợp những khó khăn nhỏ hơn. Kết quả vẫn còn`2`, bất kể có bao nhiêu bản sao của cùng một độ khó.

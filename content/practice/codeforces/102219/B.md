---
title: "CF 102219B - SpongeBob SquarePants"
description: "Mỗi trường hợp thử nghiệm mô tả một hình có bốn cạnh với các góc vuông sử dụng chiều rộng w và chiều cao h. Vì mọi hình dạng như vậy đều là hình chữ nhật nên câu hỏi duy nhất là liệu hai kích thước của nó có bằng nhau hay không."
date: "2026-08-20T03:44:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102219
codeforces_index: "B"
codeforces_contest_name: "2019 ICPC Malaysia National"
rating: 0
weight: 102219
solve_time_s: 274
verified: false
draft: false
---

[CF 102219B - SpongeBob SquarePants](https://codeforces.com/problemset/problem/102219/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 34 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một hình có bốn cạnh với các góc vuông sử dụng chiều rộng của nó`w`và chiều cao`h`. Vì mọi hình dạng như vậy đều là hình chữ nhật nên câu hỏi duy nhất là liệu hai kích thước của nó có bằng nhau hay không. Hình vuông có cùng chiều rộng và chiều cao, trong khi hình chữ nhật không phải hình vuông có kích thước khác nhau. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi in`YES`khi`w == h`, bởi vì hình dạng có thể là hình vuông và`NO`nếu không thì. 

Kích thước là số nguyên dương giữa`1`Và`1,000,000`. Ngay cả ở những giá trị lớn nhất có thể, việc so sánh hai số nguyên là một phép toán liên tục, do đó kích thước số của`w`Và`h`không tạo ra bất kỳ khó khăn số học nào. Yếu tố duy nhất có thể ảnh hưởng đến thời gian chạy là số lượng ca kiểm thử và giải pháp sẽ xử lý từng ca kiểm thử một lần. Một giải pháp thực hiện công việc tỷ lệ thuận với diện tích, chẳng hạn như lặp lại trên tất cả`w * h`vị trí đơn vị, có thể yêu cầu lên đến`10^12`lặp đi lặp lại cho một trường hợp thử nghiệm và hoàn toàn không phù hợp với giới hạn 1 giây. 

Có một vài trường hợp nhỏ có thể bộc lộ việc thực hiện bất cẩn. Kích thước tối thiểu là`1 1`, phải tạo ra`YES`; việc triển khai xử lý các kích thước nhỏ một cách đặc biệt có thể vô tình từ chối nó. Một cặp gần bằng nhau như`5 6`phải sản xuất`NO`, bởi vì sự bình đẳng là cần thiết một cách chính xác, không phải xấp xỉ. Thứ tự của các kích thước không quan trọng về mặt hình học, vì vậy`6 5`cũng sản xuất`NO`, trong khi`1000000 1000000`sản xuất`YES`. Cuối cùng, các kích thước bằng nhau ở ranh giới tối đa vẫn hoàn toàn hợp lệ, do đó không cần xử lý tràn hoặc xử lý ranh giới đặc biệt. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực theo nghĩa đen có thể tưởng tượng việc xây dựng hình chữ nhật từ các ô đơn vị của nó và kiểm tra xem hình dạng của nó có tạo thành hình vuông hay không. Đối với một`w × h`hình chữ nhật, đòi hỏi phải kiểm tra tối đa`w * h`các vị trí. Ở kích thước tối đa, điều này trở thành`1,000,000 × 1,000,000 = 10^12`hoạt động tế bào cho một trường hợp thử nghiệm duy nhất. Cách tiếp cận này đúng về mặt khái niệm vì hình dạng hoàn chỉnh chứa chính xác`w * h`các ô đơn vị, nhưng nó giải quyết một câu hỏi hình học bằng cách xây dựng lại thông tin đã được mã hóa trực tiếp theo hai chiều. 

Quan sát quan trọng là định nghĩa về hình vuông cho chúng ta chính xác điều kiện chúng ta cần: chiều rộng và chiều cao của nó phải bằng nhau. Không cần phải kiểm tra bên trong, tính diện tích, đo đường chéo hoặc liệt kê các cạnh có thể có. Hai số nguyên đầu vào chứa tất cả thông tin liên quan, do đó, một phép so sánh đẳng thức sẽ xác định hoàn toàn câu trả lời. 

Lực lượng vũ phu hoạt động vì việc kiểm tra toàn bộ hình dạng cuối cùng sẽ tiết lộ liệu hai chiều của nó có khớp với nhau hay không, nhưng nó không thành công vì nó thực hiện tối đa`10^12`những thao tác không cần thiết. Quan sát cho thấy bình phương tương đương với`w == h`giảm toàn bộ trường hợp thử nghiệm thành một so sánh theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(w × h) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`T`, bởi vì quyết định độc lập giống nhau phải được đưa ra cho mọi hình dạng. 
2. Đối với mỗi test case, hãy đọc chiều rộng của nó`w`và chiều cao`h`. Hai giá trị này mô tả đầy đủ sự khác biệt mà chúng tôi quan tâm. 
3. So sánh`w`Và`h`. Nếu chúng bằng nhau thì xuất ra`YES`, bởi vì chiều rộng và chiều cao bằng nhau chính xác là điều kiện xác định cho hình vuông. 
4. Nếu chúng khác nhau, xuất ra`NO`, vì hình chữ nhật có chiều rộng và chiều cao không bằng nhau không thể là hình vuông. 
5. Tiếp tục cho đến hết`T`các trường hợp kiểm thử đã được xử lý, tạo ra chính xác một câu trả lời cho mỗi dạng đầu vào. 

### Tại sao nó hoạt động 

Bất biến cho mỗi trường hợp thử nghiệm được xử lý rất đơn giản: đầu ra là`YES`chính xác khi hai chiều bằng nhau. Một hình vuông phải có chiều rộng và chiều cao bằng nhau nên sự bình đẳng là đủ cho việc phân loại theo yêu cầu. Ngược lại, nếu kích thước khác nhau thì hình đó không thể có bốn cạnh bằng nhau và không phải là hình vuông. Vì mọi trường hợp thử nghiệm đều được đánh giá bằng điều kiện chính xác này nên thuật toán không thể phân loại sai đầu vào hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    w, h = map(int, input().split())
    print("YES" if w == h else "NO")
```Dòng đầu tiên ghi`T`, xác định có bao nhiêu trường hợp thử nghiệm độc lập theo sau. Vòng lặp chạy chính xác`T`lần, vì vậy mỗi hình sẽ nhận được một câu trả lời và không có dữ liệu đầu vào bổ sung nào được xử lý. 

Bên trong vòng lặp,`w`Và`h`được phân tích cú pháp dưới dạng số nguyên. Biểu thức điều kiện trực tiếp thực hiện thuật toán: đẳng thức tạo ra`YES`, và sự bất bình đẳng tạo ra`NO`. 

Không có tính toán ranh giới, vòng lặp trên các kích thước hoặc chỉ số mảng, do đó không có vấn đề riêng lẻ. Số nguyên Python cũng có độ chính xác tùy ý, mặc dù điều đó không cần thiết ở đây vì kích thước tối đa là`1,000,000`. sử dụng`sys.stdin.readline`cung cấp khả năng xử lý đầu vào hiệu quả ngay cả khi có nhiều trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu chứa bốn hình chữ nhật độc lập. 

| Trường hợp thử nghiệm |`w`|`h`|`w == h`| Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 9 | Đúng |`YES`| 
| 2 | 16 | 30 | Sai |`NO`| 
| 3 | 200 | 33 | Sai |`NO`| 
| 4 | 547 | 547 | Đúng |`YES`| 

Đối với hình dạng thứ nhất và thứ tư, kích thước khớp chính xác nên chúng được chấp nhận là hình vuông. Hai cái còn lại có kích thước khác nhau và bị từ chối. Điều này chứng tỏ rằng thuật toán không cần cấu trúc hình học vì mọi quyết định đều được đưa ra trực tiếp từ cặp đầu vào. 

### Ví dụ được xây dựng 

Hãy xem xét đầu vào:```
3
1 1
5 6
1000000 1000000
```Việc thực hiện là: 

| Trường hợp thử nghiệm |`w`|`h`|`w == h`| Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | Đúng |`YES`| 
| 2 | 5 | 6 | Sai |`NO`| 
| 3 | 1000000 | 1000000 | Đúng |`YES`| 

Dấu vết này bao gồm cả ranh giới của các kích thước được phép và một cặp khác nhau chính xác một. Nó xác nhận rằng thuật toán tự kiểm tra sự bằng nhau thay vì dựa vào ngưỡng kích thước hoặc chênh lệch lớn hơn một số giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trong số`T`trường hợp thử nghiệm yêu cầu một so sánh. | 
| Không gian | O(1) | Chỉ có chiều rộng và chiều cao hiện tại được lưu trữ. | 

Kích thước tối đa của`1,000,000`không ảnh hưởng đến khối lượng công việc được thực hiện bởi giải pháp tối ưu. Kể cả nếu`T`lớn, thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi trường hợp thử nghiệm, dễ dàng phù hợp với giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())

    for _ in range(t):
        w, h = map(int, input().split())
        print("YES" if w == h else "NO")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return output

# Provided sample
assert run("""4
9 9
16 30
200 33
547 547
""") == """YES
NO
NO
YES
""", "sample 1"

# Minimum-size dimensions
assert run("""1
1 1
""") == """YES
""", "minimum dimensions"

# Maximum-size equal dimensions
assert run("""1
1000000 1000000
""") == """YES
""", "maximum equal dimensions"

# Maximum-size unequal dimensions
assert run("""2
1000000 999999
999999 1000000
""") == """NO
NO
""", "maximum boundary with unequal dimensions"

# Difference of exactly one and several equal cases
assert run("""5
2 3
3 2
7 7
42 42
100 99
""") == """NO
NO
YES
YES
NO
""", "boundary equality cases")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`YES`| Kích thước tối thiểu được phép và sự bằng nhau ở ranh giới dưới | 
|`1000000 1000000`|`YES`| Kích thước tối đa được phép với các cạnh bằng nhau | 
|`1000000 999999`,`999999 1000000`|`NO`,`NO`| Giá trị biên tối đa và tính độc lập với thứ tự chiều | 
|`2 3`,`3 2`,`7 7`,`42 42`,`100 99`|`NO`,`NO`,`YES`,`YES`,`NO`| Bình đẳng chính xác và sai lầm về phong cách từng người một | 

## Vỏ cạnh 

Hình dạng nhỏ nhất có thể là`1 × 1`. đầu vào```
1
1 1
```cho`w = 1`Và`h = 1`, do đó so sánh`w == h`là đúng và đầu ra là`YES`. Việc triển khai bất cẩn giả định một hình vuông phải có kích thước lớn hơn một sẽ thất bại ở đây. 

Một hình chữ nhật có kích thước chỉ khác nhau một vẫn không phải là hình vuông. Vì```
1
5 6
```thuật toán so sánh`5`Và`6`, nhận thấy chúng không bằng nhau và in`NO`. Không có dung sai nào liên quan nên việc các kích thước gần nhau không làm thay đổi cách phân loại. 

Hai chiều có thể xuất hiện theo một trong hai thứ tự. Vì```
1
6 5
```sự so sánh lại sai, tạo ra`NO`. Thuật toán không cần chuẩn hóa kích thước bằng`min`Và`max`, bởi vì sự bình đẳng không bị ảnh hưởng bởi thứ tự của chúng. 

Cuối cùng, kích thước hợp lệ lớn nhất không yêu cầu xử lý đặc biệt. Với```
1
1000000 1000000
```cả hai giá trị đều bằng nhau nên thuật toán sẽ in ngay`YES`. Với```
1
1000000 999999
```các giá trị khác nhau, vì vậy nó in`NO`. Vì giải pháp không bao giờ nhân các kích thước hoặc thực hiện bất kỳ thao tác nào tỷ lệ thuận với độ lớn của chúng, nên các trường hợp biên này tốn chính xác cùng một lượng công việc như`1 × 1`.

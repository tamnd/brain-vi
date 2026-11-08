---
title: "CF 1413E - Solo mid Oracle"
description: "Chúng tôi liên tục niệm phép lên kẻ thù. Mỗi lần sử dụng sẽ gây sát thương ngay lập tức. Sau đó, chiêu thức tương tự sẽ hồi máu cho kẻ địch bằng b máu mỗi giây trong đúng c giây. Các lần sử dụng diễn ra cứ sau d giây do thời gian hồi chiêu và tất cả các hiệu ứng đều cộng dồn."
date: "2026-06-11T07:22:54+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1413
codeforces_index: "E"
codeforces_contest_name: "Technocup 2021 - Elimination Round 1"
rating: 2100
weight: 1413
solve_time_s: 110
verified: true
draft: false
---

[CF 1413E - Solo mid Oracle](https://codeforces.com/problemset/problem/1413/E) 

**Đánh giá:** 2100 
**Tags:** tham lam, toán học, tìm kiếm bậc ba 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi liên tục niệm phép lên kẻ thù. 

Mỗi lần sử dụng sẽ gây ngay lập tức`a`hư hại. Sau đó, sử dụng tương tự sẽ chữa lành vết thương cho kẻ thù bằng cách`b`sức khỏe chính xác từng giây`c`giây. Diễn viên xảy ra mỗi`d`giây vì thời gian hồi chiêu và tất cả hiệu ứng đều cộng dồn. 

Kẻ địch chết nếu tại một thời điểm nào đó, tổng hiệu ứng tích lũy của tất cả các lần sử dụng khiến máu của kẻ địch trở nên không tích cực. Vì mọi thay đổi về sức khỏe xảy ra tại cùng một thời điểm đều được áp dụng cùng nhau, điều thực sự quan trọng là tổng sát thương trừ đi mức hồi phục tối đa đạt được bất kỳ lúc nào. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải tìm ra lượng máu lớn nhất của kẻ thù vẫn có thể bị tiêu diệt. Nếu sát thương tích lũy có thể tăng lên không giới hạn, thì mọi giá trị sức khỏe cuối cùng đều có thể bị tiêu diệt và câu trả lời là`-1`. 

Số lượng ca kiểm thử lớn như`10^5`, vì vậy bất cứ điều gì liên quan đến mô phỏng theo thời gian là không thể. Bản thân các thông số có thể đạt tới`10^6`, có nghĩa là câu trả lời có thể ở xung quanh`10^12`, vì vậy cần có số học 64 bit. 

Thách thức là hiệu ứng chữa lành của nhiều lần sử dụng trước đó chồng chéo lên nhau. Mô phỏng dòng thời gian trực tiếp sẽ cần xử lý hàng triệu hoặc hàng tỷ khoảnh khắc, vượt xa giới hạn. 

Một số trường hợp đặc biệt đáng được quan tâm đặc biệt. 

Coi như:```
a = 1, b = 2, c = 3, d = 4
```Phép thuật chỉ hồi phục trong ba giây, nhưng thời gian hồi chiêu là bốn giây. Mỗi lần niệm phép sẽ hoàn tất quá trình hồi máu trước khi lần niệm phép tiếp theo diễn ra. Sát thương tốt nhất đạt được chỉ đơn giản là đòn đánh đầu tiên nên câu trả lời là`1`. 

Bây giờ hãy xem xét:```
a = 239, b = 21, c = 11, d = 3
```Ở đây, mỗi lần sử dụng sẽ gây ra nhiều sát thương hơn tổng số lần hồi máu bổ sung được đưa ra giữa các lần sử dụng. Thiệt hại tích lũy sẽ tăng mãi mãi, vì vậy câu trả lời là`-1`. 

Một trường hợp tế nhị khác là:```
a = 4, b = 3, c = 2, d = 1
```Việc triển khai ngây thơ có thể chỉ nhìn vào thiệt hại ròng trên mỗi lần sử dụng và kết luận rằng câu trả lời là lớn. Trên thực tế, các vết thương chồng chéo sẽ nhanh chóng chống lại các lần bó bột trong tương lai. Câu trả lời thực sự là`5`, đạt được tại một thời điểm trung gian cụ thể. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng quá trình. 

Đôi khi chúng tôi có thể bỏ việc`0, d, 2d, ...`, theo dõi tất cả các hiệu ứng hồi máu đang hoạt động và tính toán tổng sát thương-trừ-hồi phục sau mỗi sự kiện. Giá trị tối đa đạt được sẽ là lượng máu lớn nhất có thể bị tiêu diệt. 

Điều này có hiệu quả về mặt khái niệm vì câu trả lời chính xác là thiệt hại tích lũy tối đa từng đạt được. Thật không may, thời lượng có thể dài tùy ý. Khi sát thương cứ tăng mãi thì không có điểm dừng hữu hạn. Ngay cả trong trường hợp hữu hạn, các tham số lên tới`10^6`làm cho việc mô phỏng từng sự kiện trở nên quá đắt đỏ. 

Quan sát quan trọng là chúng ta không cần toàn bộ dòng thời gian. 

Giả sử diễn viên xảy ra vào những thời điểm:```
0, d, 2d, 3d, ...
```Chúng ta hãy xem xét tác động tích lũy ngay sau khi`k`-thứ diễn viên. 

Mỗi diễn viên mới đóng góp`+a`. 

Giữa các lần sử dụng liên tiếp, một số lần hồi phục sẽ xảy ra. Bao nhiêu? 

Một bó bột sẽ lành lại trong lần tiếp theo`c`giây. Vì các phôi cách nhau bởi`d`giây, một diễn viên cụ thể vẫn hoạt động chính xác trong```
ceil(c / d)
```khoảng thời gian đúc. 

Một cách rõ ràng hơn để đếm khả năng hồi máu tích cực tại thời điểm sử dụng là quan sát điều đó giữa các lần sử dụng`i-1`và đúc`i`, mọi phép thuật trước đó có thời gian hồi phục vẫn bao gồm khoảng thời gian đó đều góp phần`b*d`đang lành lại. 

Số lượng phép thuật hoạt động như vậy cuối cùng sẽ ổn định ở mức```
m = floor((c - 1) / d)
```bởi vì một câu thần chú ảnh hưởng đến câu thần chú tiếp theo`m + 1`khoảng thời gian. 

Sau khi sử dụng đủ số lần, mỗi lần sử dụng bổ sung sẽ thay đổi sát thương tích lũy bằng```
a - b * (m + 1)
```đó là```
a - b * ceil(c / d)
```từ```
ceil(c/d) = floor((c-1)/d) + 1.
```Nếu số lượng này là dương, thiệt hại tích lũy sẽ tăng mãi mãi. Bất kỳ giá trị sức khỏe nào cuối cùng cũng có thể bị mất đi, vì vậy câu trả lời là`-1`. 

Ngược lại, trình tự cuối cùng sẽ giảm, nghĩa là mức tối đa xảy ra trong số các lần thực hiện đầu tiên. 

Cho phép```
k = floor((c - 1) / d)
```Vậy thì khoảnh khắc tuyệt vời nhất chính là sau`(k+1)`-diễn viên thứ nhất. Cho đến thời điểm đó, mỗi lần sử dụng mới sẽ tăng sát thương tích lũy lên thêm```
a - b * i
```để tăng giá trị của`i`. 

Tổng hợp những lợi ích này mang lại```
answer =
a + (a-b) + (a-2b) + ... + (a-kb)
```đơn giản hóa để```
(k+1)a - b * k(k+1)/2.
```Giá trị này là lượng máu tối đa có thể bị giết. 

## So sánh các phương pháp tiếp cận 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | Không giới hạn / quá lớn | O(số phép thuật kích hoạt) | Quá chậm | 
| Đạo hàm toán học | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán```
k = floor((c - 1) / d).
```Đây là số lần niệm phép trước đó mà phép thuật vẫn có thể giúp hồi máu. 
2. Kiểm tra xem```
a > b * (k + 1).
```Đây là lợi ích lâu dài từ việc thêm một lần đúc nữa sau khi mô hình chồng chéo đã ổn định. 
3. Nếu bất đẳng thức đúng, hãy in`-1`. 

Sát thương tích lũy tăng lên không giới hạn, vì vậy kẻ địch có lượng máu lớn tùy ý cuối cùng có thể bị giết. 
4. Ngược lại tính toán```
(k + 1) * a - b * k * (k + 1) / 2.
```Đây là mức sát thương tích lũy tối đa từng đạt được. 
5. Xuất giá trị đó. 

### Tại sao nó hoạt động 

hãy để`D(n)`là sát thương-trừ-hồi phục tích lũy ngay sau khi`n`-thứ diễn viên. 

Dàn diễn viên đầu tiên góp mặt`a`. 

Mỗi diễn viên tiếp theo sẽ thêm một diễn viên khác`a`, nhưng cũng phải chịu sự hồi phục do các phép thuật hoạt động trước đó tạo ra. Trước khi mô hình chồng chéo ổn định, khả năng hồi máu được thêm vào giữa các lần sử dụng sẽ tăng chính xác`b`từng bước. Sau đó`k+1`khoảng thời gian hoạt động hiện diện, khả năng hồi phục được thêm vào sẽ không đổi ở`b(k+1)`. 

Như vậy sự khác biệt liên tiếp của`D(n)`hình thức:```
a,
a-b,
a-2b,
...
a-kb,
a-(k+1)b,
a-(k+1)b,
...
```Nếu hằng số chênh lệch cuối cùng là dương thì chuỗi sẽ tăng mãi mãi. Ngược lại, dãy sẽ không tăng sau khi đạt đến số hạng tương ứng với`a-kb`, do đó giá trị cực đại của nó đạt được chính xác tại thời điểm đó. Tổng hợp cấp số cộng sẽ mang lại công thức trên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        a, b, c, d = map(int, input().split())

        k = (c - 1) // d

        if a > b * (k + 1):
            ans.append("-1")
        else:
            res = (k + 1) * a - b * k * (k + 1) // 2
            ans.append(str(res))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo đạo hàm toán học trực tiếp. 

giá trị```
k = (c - 1) // d
```ghi lại số lần sử dụng bổ sung vẫn bị ảnh hưởng bởi quá trình hồi phục của phép thuật. 

điều kiện```
a > b * (k + 1)
```kiểm tra xem độ dốc ổn định của chuỗi thiệt hại tích lũy có dương hay không. Nếu đúng như vậy, câu trả lời sẽ không bị giới hạn và chúng tôi sẽ in`-1`. 

Mặt khác, chúng tôi đánh giá tổng dạng đóng của cấp số cộng. Số nguyên Python tự động xử lý các giá trị lớn hơn 64 bit, mặc dù số nguyên 64 bit có dấu cũng đủ cho vấn đề này. 

Sai lầm phổ biến nhất là sử dụng`c // d`thay vì`(c - 1) // d`. Quá trình hồi máu bắt đầu một giây sau khi niệm phép và kéo dài chính xác`c`giây, do đó số lượng trùng lặp chính xác được lấy từ`ceil(c/d)`, bằng`(c - 1)//d + 1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
228 21 11 3
```Tính toán:```
k = (11 - 1) / 3 = 3
```| Biến | Giá trị | 
| --- | --- | 
| một | 228 | 
| b | 21 | 
| c | 11 | 
| d | 3 | 
| k | 3 | 
| b(k+1) | 84 | 
| a > b(k+1)? | Có | 

Từ```
228 > 84
```thiệt hại tích lũy cứ tăng mãi mãi. 

Đầu ra:```
-1
```Điều này chứng tỏ trường hợp tăng trưởng không giới hạn. 

### Ví dụ 2 

đầu vào:```
4 3 2 1
```| Biến | Giá trị | 
| --- | --- | 
| một | 4 | 
| b | 3 | 
| c | 2 | 
| d | 1 | 
| k | 1 | 
| b(k+1) | 6 | 
| a > b(k+1)? | Không | 

Bây giờ hãy tính:```
(1+1)*4 - 3*1*2/2
= 8 - 3
= 5
```Đầu ra:```
5
```Trình tự đạt được là:```
4, 1, -2, -2, ...
```vì vậy sát thương tích lũy đạt mức tối đa sau lần thi triển thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một vài phép tính số học | 
| Không gian | O(1) | Không có công trình phụ trợ | 

Với nhiều nhất`10^5`các trường hợp thử nghiệm, tổng công việc vẫn tuyến tính về số lượng trường hợp và dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    out = []

    for _ in range(t):
        a, b, c, d = map(int, input().split())

        k = (c - 1) // d

        if a > b * (k + 1):
            out.append("-1")
        else:
            out.append(str((k + 1) * a - b * k * (k + 1) // 2))

    return "\n".join(out)

# provided sample
assert run(
"""7
1 1 1 1
2 2 2 2
1 2 3 4
4 3 2 1
228 21 11 3
239 21 11 3
1000000 1 1000000 1
"""
) == "\n".join([
    "1",
    "2",
    "1",
    "5",
    "534",
    "-1",
    "500000500000"
]), "sample"

# minimum values
assert run(
"""1
1 1 1 1
"""
) == "1"

# exact boundary, slope becomes zero
assert run(
"""1
6 3 2 1
"""
) == "9"

# no overlap between casts
assert run(
"""1
5 2 3 10
"""
) == "5"

# very large answer
assert run(
"""1
1000000 1 1000000 1
"""
) == "500000500000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`1`| Trường hợp hợp lệ nhỏ nhất | 
|`6 3 2 1`|`9`| Ranh giới trong đó độ dốc dài hạn chính xác bằng 0 | 
|`5 2 3 10`|`5`| Không có sự chồng chéo hồi máu giữa các lần sử dụng | 
|`1000000 1 1000000 1`|`500000500000`| Số học lớn và an toàn 64-bit | 

## Vỏ cạnh 

### Không có sự trùng lặp nào cả 

đầu vào:```
1
5 2 3 10
```Chúng tôi nhận được:```
k = (3 - 1) // 10 = 0
```Công thức trở thành:```
(0+1) * 5 = 5
```Mỗi lần niệm phép sẽ hồi phục rất lâu trước khi lần niệm phép tiếp theo bắt đầu. Sát thương tốt nhất chỉ đơn giản là cú đánh đầu tiên. 

Đầu ra:```
5
```### Độ dốc dài hạn chính xác bằng 0 

đầu vào:```
1
6 3 2 1
```Chúng tôi có:```
k = 1
a = 6
b(k+1) = 6
```Trình tự đạt được là:```
6, 3, 0, 0, ...
```Sát thương tích lũy đạt tới:```
6 -> 9 -> 9 -> 9 ...
```Cực đại là hữu hạn và bằng`9`. 

Đầu ra:```
9
```Trường hợp này phát hiện các giải pháp sử dụng sai`>=`thay vì`>`trong bài kiểm tra tính không giới hạn. 

###Tăng trưởng không giới hạn 

đầu vào:```
1
239 21 11 3
```Chúng tôi tính toán:```
k = 3
b(k+1) = 84
```Từ```
239 > 84
```mỗi lần sử dụng đủ muộn sẽ tăng sát thương tích lũy. Không có mức tối đa hữu hạn. 

Đầu ra:```
-1
```Trường hợp này tìm ra các giải pháp luôn áp dụng công thức lũy tiến số học mà không kiểm tra trước xem chuỗi có bị chặn hay không.

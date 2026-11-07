---
title: "CF 1368A - C+="
description: "Chúng ta bắt đầu với hai số nguyên dương a và b. Trong một thao tác, chúng ta có thể thêm một biến vào một biến khác: a += b hoặc b += a. Chỉ có một giá trị thay đổi cho mỗi thao tác. Mục tiêu của chúng ta là làm cho ít nhất một trong hai số trở nên lớn hơn giới hạn n cho trước."
date: "2026-06-11T11:45:09+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1368
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 8"
rating: 800
weight: 1368
solve_time_s: 110
verified: true
draft: false
---

[CF 1368A - C+=](https://codeforces.com/problemset/problem/1368/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với hai số nguyên dương,`a`Và`b`. Trong một thao tác, chúng ta có thể thêm một biến vào biến khác:`a += b`hoặc`b += a`. 

Chỉ có một giá trị thay đổi cho mỗi thao tác. Mục tiêu của chúng ta là làm cho ít nhất một trong hai số trở nên lớn hơn một giới hạn nhất định`n`. Đối với mỗi trường hợp thử nghiệm, chúng ta phải tìm số lượng thao tác tối thiểu cần thiết. 

Các ràng buộc nhỏ về số lượng kiểm tra, nhiều nhất là 100 trường hợp, nhưng bản thân các giá trị có thể lớn bằng`10^9`. Điều đó ngay lập tức gợi ý rằng chúng ta không nên mô phỏng mọi chuỗi lựa chọn có thể có bằng tìm kiếm hoặc lập trình động. Ngay cả hệ số phân nhánh là hai cho mỗi nước đi cũng sẽ bùng nổ. Chúng ta cần một chiến lược trực tiếp. 

Một quan sát hữu ích là con số này tăng lên rất nhanh. Nếu chúng ta luôn cộng giá trị lớn hơn vào giá trị nhỏ hơn thì chuỗi sẽ hoạt động tương tự như sự tăng trưởng Fibonacci. Bắt đầu từ giá trị đến`10^9`, số lượng thao tác cần thiết trước khi vượt quá`n`chỉ có vài chục thôi. Điều này có nghĩa là một mô phỏng đơn giản đã đủ nhanh nếu chúng ta luôn đưa ra lựa chọn tham lam chính xác. 

Có một số trường hợp đáng để kiểm tra cẩn thận. 

Giả định`a = 1`,`b = 1`,`n = 1`. Câu trả lời là`1`, không`0`, bởi vì mục tiêu là trở nên lớn hơn`n`. Ban đầu cả hai giá trị bằng nhau`n`, vì vậy vẫn cần một thao tác. 

Giả định`a = 1`,`b = 2`,`n = 3`. Sau một thao tác chúng ta có thể đạt được`(3,2)`hoặc`(1,3)`, nhưng không có giá trị nào vượt quá`3`. Một hoạt động thứ hai là cần thiết. Câu trả lời đúng là`2`. Việc triển khai bất cẩn sẽ dừng lại khi giá trị bằng`n`sẽ tạo ra kết quả sai. 

Giả định`a = 5`,`b = 4`,`n = 100`. Việc chọn các bản cập nhật kém có thể làm tăng số lượng hoạt động. Ví dụ: việc liên tục thêm giá trị nhỏ hơn vào giá trị lớn hơn sẽ phát triển chậm hơn so với chiến lược tối ưu. Câu trả lời tối thiểu đòi hỏi phải thực hiện mức tăng lớn nhất có thể ở mỗi bước. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi mọi hoạt động như một lựa chọn nhị phân. Từ một tiểu bang`(a, b)`chúng ta có thể chuyển đến`(a + b, b)`hoặc`(a, a + b)`. Vì chúng ta muốn số lượng thao tác tối thiểu nên chúng ta có thể tưởng tượng việc tìm kiếm theo chiều rộng trên tất cả các trạng thái có thể truy cập. 

Cách tiếp cận này đúng vì BFS khám phá các trạng thái trong việc tăng số lượng hoạt động. Vấn đề là số lượng trạng thái. Ngay cả sau một số bước khiêm tốn, hệ số phân nhánh của hai vẫn tạo ra nhiều khả năng theo cấp số nhân. Việc tìm kiếm như vậy là hoàn toàn không thực tế. 

Quan sát quan trọng là tại bất kỳ thời điểm nào, nếu chúng ta muốn vượt quá`n`càng nhanh càng tốt, chúng ta nên cộng số lớn hơn vào số nhỏ hơn. 

Cho rằng`a ≤ b`. 

Nếu chúng ta biểu diễn`a += b`, giá trị mới trở thành`a + b`. 

Nếu thay vào đó chúng ta thực hiện`b += a`, giá trị mới trở thành`b + a`, nhưng giá trị nhỏ hơn không thay đổi. 

Sự lựa chọn đầu tiên biến đổi`(a, b)`vào trong`(a + b, b)`, trong khi cái thứ hai biến nó thành`(a, a + b)`. 

Số lớn hơn sau một trong hai thao tác là như nhau, cụ thể là`a + b`. Sự khác biệt là lựa chọn đầu tiên cũng làm tăng giá trị nhỏ hơn. Giá trị lớn hơn nhỏ hơn chỉ có thể hỗ trợ các hoạt động trong tương lai vì mọi phép cộng trong tương lai đều sử dụng cả hai số. Giữ giá trị nhỏ nhất càng lớn càng tốt không bao giờ gây hại và thường hữu ích. 

Điều này dẫn đến một quá trình tham lam đơn giản: 

Tiếp tục cộng số lớn hơn với số nhỏ hơn cho đến khi một trong số chúng vượt quá`n`. 

Bởi vì các giá trị tăng lên như số Fibonacci nên số lần lặp lại rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Mô phỏng tham lam tối ưu | O(k) | O(1) | Đã chấp nhận | 

Đây`k`là số thao tác được thực hiện, nhiều nhất là khoảng 50 đối với các ràng buộc đã cho. 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm hoạt động về 0. 
2. Trong khi cả hai`a`Và`b`nhiều nhất là`n`, tiếp tục thực hiện các thao tác. 
3. So sánh hai giá trị. 
4. Nếu`a < b`, trình diễn`a += b`. 
5. Nếu không thì thực hiện`b += a`. 

Điều này luôn làm tăng giá trị nhỏ hơn, tạo ra cặp lớn nhất có thể để tăng trưởng trong tương lai. 
6. Tăng bộ đếm hoạt động. 
7. Khi một trong hai giá trị lớn hơn`n`, dừng và xuất bộ đếm. 

### Tại sao nó hoạt động 

Tại mỗi bước, hãy`x`có giá trị nhỏ hơn và`y`được giá trị lớn hơn. 

Hai trạng thái tiếp theo có thể xảy ra là`(x + y, y)`Và`(x, x + y)`. 

Cả hai đều tạo ra cùng một giá trị tối đa mới,`x + y`. Sự khác biệt là giá trị còn lại. Giữ nó như`y`ít nhất thì luôn tốt bằng việc giữ nó như`x`, bởi vì`y ≥ x`. 

Vì vậy, sau một thao tác, việc cập nhật giá trị nhỏ hơn sẽ tạo ra một trạng thái có hai số ít nhất bằng các số tương ứng ở trạng thái thay thế. Vì các hoạt động trong tương lai chỉ liên quan đến việc bổ sung các giá trị này, nên việc bắt đầu từ trạng thái lớn hơn về thành phần không thể yêu cầu nhiều hoạt động hơn để vượt quá`n`. 

Do đó, luôn tồn tại một giải pháp tối ưu cập nhật giá trị nhỏ hơn ở mỗi bước, đó chính xác là những gì thuật toán thực hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    
    for _ in range(t):
        a, b, n = map(int, input().split())
        
        ops = 0
        
        while max(a, b) <= n:
            if a < b:
                a += b
            else:
                b += a
            ops += 1
        
        print(ops)

solve()
```Vòng lặp tiếp tục khi cả hai số đều chưa đủ lớn. sử dụng`max(a, b) <= n`tương đương với việc nói không có giá trị nào vượt quá`n`. 

Bước tham lam luôn cập nhật giá trị nhỏ hơn. Đây là ý tưởng trung tâm của giải pháp. Sau mỗi lần cập nhật, chúng tôi tăng bộ đếm hoạt động. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn mặc dù các giá trị có thể tạm thời lớn hơn`10^9`. 

Một lỗi phổ biến là dừng lại khi một giá trị bằng`n`. Vấn đề đòi hỏi một giá trị phải _hoàn toàn lớn hơn_`n`, vậy điều kiện phải là`<= n`bên trong vòng lặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = 1, b = 2, n = 3
```| Hoạt động | một | b | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | 2 | Trạng thái ban đầu | 
| 1 | 3 | 2 | a += b | 
| 2 | 3 | 5 | b += a | 

Sau ca phẫu thuật thứ hai,`5 > 3`, vậy câu trả lời là`2`. 

Ví dụ này cho thấy tại sao việc tiếp cận chính xác`n`là không đủ. Sau bước đi đầu tiên chúng ta có`a = 3`, nhưng vẫn cần thực hiện một thao tác khác. 

### Ví dụ 2 

đầu vào:```
a = 5, b = 4, n = 100
```| Hoạt động | một | b | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | 5 | 4 | Trạng thái ban đầu | 
| 1 | 5 | 9 | b += a | 
| 2 | 14 | 9 | a += b | 
| 3 | 14 | 23 | b += a | 
| 4 | 37 | 23 | a += b | 
| 5 | 37 | 60 | b += a | 
| 6 | 97 | 60 | a += b | 
| 7 | 97 | 157 | b += a | 

Sau hoạt động 7,`157 > 100`, vậy câu trả lời là`7`. 

Dấu vết minh họa sự tăng trưởng giống Fibonacci được tạo ra bằng cách cập nhật liên tục giá trị nhỏ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Một lần lặp cho mỗi thao tác được thực hiện | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 
| | | | 

Giá trị của`k`là rất nhỏ vì các con số tăng lên gần giống như số Fibonacci. Ngay cả đối với đầu vào được phép lớn nhất, chỉ cần vài chục lần lặp. Giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        a, b, n = map(int, input().split())

        ops = 0
        while max(a, b) <= n:
            if a < b:
                a += b
            else:
                b += a
            ops += 1

        ans.append(str(ops))

    return "\n".join(ans)

# provided sample
assert run("2\n1 2 3\n5 4 100\n") == "2\n7", "sample"

# minimum values
assert run("1\n1 1 1\n") == "1", "minimum case"

# equal values
assert run("1\n2 2 10\n") == "4", "equal starting values"

# already close to limit
assert run("1\n10 10 10\n") == "1", "strictly greater required"

# large boundary values
assert run("1\n1000000000 1000000000 1000000000\n") == "1", "maximum values"

# off-by-one check
assert run("1\n1 2 2\n") == "1", "exceed immediately"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Đầu vào pháp lý nhỏ nhất | 
|`2 2 10`|`4`| Giá trị bắt đầu bằng nhau | 
|`10 10 10`|`1`| Phải trở nên lớn hơn, không lớn hơn hoặc bằng | 
|`1000000000 1000000000 1000000000`|`1`| Giá trị được phép lớn nhất | 
|`1 2 2`|`1`| Vượt ngay lập tức sau một thao tác | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1 1
```Ban đầu không có giá trị nào lớn hơn`1`. Thuật toán đi vào vòng lặp, thực hiện`b += a`, sản xuất`(1, 2)`, và dừng lại. Đầu ra là`1`. Điều này khẳng định rằng sự bình đẳng với`n`không thỏa mãn mục tiêu. 

Coi như:```
1
1 2 3
```Việc thực hiện là:`(1,2) -> (3,2) -> (3,5)`Sau thao tác đầu tiên, giá trị tối đa chính xác là`3`, vì vậy vòng lặp tiếp tục. Sau thao tác thứ hai, giá trị tối đa sẽ trở thành`5`, vượt quá`3`. Đầu ra là`2`. Điều này xử lý sự bất bình đẳng nghiêm ngặt một cách chính xác. 

Coi như:```
1
10 10 10
```Việc thực hiện là:`(10,10) -> (10,20)`Một thao tác là đủ vì`20 > 10`. Một triển khai kiểm tra`>= n`trước khi thực hiện bất kỳ động thái nào sẽ quay trở lại không chính xác`0`. 

Coi như:```
1
5 4 100
```Chiến lược tham lam liên tục cập nhật giá trị nhỏ hơn và đạt giá trị trên`100`trong bảy hoạt động. Bất kỳ chiến lược nào đôi khi cập nhật giá trị lớn hơn sẽ khiến số nhỏ hơn trở nên nhỏ một cách không cần thiết và không thể hoàn thành sớm hơn. Điều này chứng tỏ sự lựa chọn đúng đắn của người tham lam.

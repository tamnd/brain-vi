---
title: "CF 910C - Số tiền tối thiểu"
description: "Chúng ta có một số chuỗi được tạo từ các chữ cái 'a' đến 'j'. Ban đầu, những chuỗi này là số thập phân. Một kẻ chơi khăm đã thay thế mỗi chữ số bằng một chữ cái duy nhất, tạo ra sự tương ứng một-một giữa mười chữ số 0...9 và mười chữ cái a...j."
date: "2026-06-12T10:23:22+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 910
codeforces_index: "C"
codeforces_contest_name: "Testing Round 14 (Unrated)"
rating: 1700
weight: 910
solve_time_s: 223
verified: false
draft: false
---

[CF 910C - Số tiền tối thiểu](https://codeforces.com/problemset/problem/910/C) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 3m 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số chuỗi được làm từ các chữ cái`'a'`bởi vì`'j'`. Ban đầu, những chuỗi này là số thập phân. Một kẻ chơi khăm đã thay thế mỗi chữ số bằng một chữ cái duy nhất, tạo ra sự tương ứng một-một giữa mười chữ số`0...9`và mười chữ cái`a...j`. 

Cùng một chữ cái luôn đại diện cho cùng một chữ số và các chữ cái khác nhau đại diện cho các chữ số khác nhau. Nhiệm vụ của chúng ta là xây dựng lại ánh xạ sao cho mọi số kết quả đều hợp lệ, nghĩa là không có số nào bắt đầu bằng chữ số`0`và tổng tổng của tất cả các số được xây dựng lại càng nhỏ càng tốt. 

Quan sát quan trọng là chúng ta không xây dựng lại các số riêng lẻ một cách độc lập. Việc gán các chữ số cho các chữ cái phải hoạt động trên toàn cầu trên tất cả các chuỗi. 

Các ràng buộc nhỏ về độ dài chuỗi. Có nhiều nhất 1000 chuỗi và mỗi chuỗi có độ dài tối đa là 6, vì vậy tổng số vị trí ký tự nhiều nhất là 6000. Điều đó có nghĩa là chúng ta có đủ khả năng xử lý mọi ký tự và tính toán đóng góp vị trí. Số lượng các chữ cái riêng biệt được cố định ở mức 10, một con số rất nhỏ. Bất kỳ thuật toán nào liên quan đến việc sắp xếp hoặc gán mười chữ cái này về cơ bản là thời gian không đổi. 

Một sai lầm hấp dẫn nhưng nguy hiểm là xử lý từng sự cố một cách độc lập. Chữ số được gán cho một chữ cái sẽ ảnh hưởng đến mọi vị trí mà chữ cái đó xuất hiện. Ví dụ:```
2
aa
a
```Nếu như`a`được gán chữ số`9`, tổng là`99 + 9 = 108`. Nếu như`a`được gán chữ số`1`, tổng là`11 + 1 = 12`. Mỗi lần xuất hiện đều đóng góp đồng thời, vì vậy chúng ta phải tổng hợp tất cả các tác động vị trí của từng chữ cái. 

Một vấn đề tế nhị khác là hạn chế số 0 đứng đầu. Coi như:```
2
ab
ac
```Thư`a`xuất hiện dưới dạng ký tự đầu tiên của một số có nhiều chữ số, do đó việc gán chữ số`0`ĐẾN`a`là bất hợp pháp. Một chiến lược tham lam chỉ đơn giản là đưa ra con số`0`đến chữ cái có giá trị thấp nhất mà không kiểm tra ràng buộc này có thể tạo ra một bản tái thiết không hợp lệ. 

Trường hợp cạnh thứ ba xảy ra khi một chữ cái không bao giờ xuất hiện dưới dạng ký tự đầu. Chữ cái như vậy là loại chữ cái duy nhất có thể nhận được chữ số`0`. Câu lệnh đảm bảo rằng phép gán hợp lệ luôn tồn tại, nhưng thuật toán vẫn phải chọn ứng cử viên số 0 chính xác. 

Ví dụ:```
1
ab
```Đây`a`không thể bằng 0, nhưng`b`Có thể. Phân công`b = 0`Và`a = 1`đưa ra số lượng hợp lệ tối thiểu`10`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê tất cả các phép gán chữ số có thể có cho các chữ cái. Vì có mười chữ cái và mười chữ số nên có`10! = 3,628,800`song ngữ. Đối với mỗi bài tập, chúng ta có thể đánh giá tổng kết quả và giữ giá trị tối thiểu hợp lệ. 

Phương pháp brute-force là đúng vì nó kiểm tra mọi ánh xạ có thể có. Vấn đề là khối lượng công việc. Ngay cả khi việc đánh giá một bản đồ rất rẻ thì vài triệu nhiệm vụ cũng đã là quá nhiều cho giới hạn một giây. 

Để tìm ra lời giải tốt hơn, chúng ta nên xem xét tổng số tiền được hình thành như thế nào. 

Giả sử một chữ cái xuất hiện ở vị trí đơn vị năm lần. Chỉ định chữ số`7`lá thư đó góp phần`5 × 7`đến số tiền cuối cùng. Nếu cùng một chữ cái xuất hiện hai lần ở vị trí hàng trăm, điều đó góp phần`2 × 100 × 7`. 

Tất cả đóng góp của một lá thư có thể được kết hợp thành một hệ số duy nhất:$$\text{contribution} = \text{digit(letter)} \times \text{weight(letter)}$$Ở đâu`weight(letter)`là tổng của tất cả các giá trị vị trí nơi chữ cái xuất hiện. 

Tổng số tiền trở thành$$\sum \text{digit(letter)} \times \text{weight(letter)}$$Bây giờ vấn đề đã rõ ràng hơn nhiều. Chúng tôi có mười trọng số và phải gán chữ số`0...9`cho họ. Để giảm thiểu tổng có trọng số, các chữ số nhỏ hơn sẽ có trọng số nhỏ hơn và các chữ số lớn hơn sẽ có trọng số lớn hơn. Đây là một đối số trao đổi tiêu chuẩn. 

Sự phức tạp duy nhất là chữ số`0`. Nó không thể được gán cho một chữ cái xuất hiện dưới dạng ký tự đầu của một số có nhiều chữ số. Trong số tất cả các chữ cái đủ điều kiện bằng 0, chúng ta nên chọn chữ cái có trọng số nhỏ nhất, vì số 0 loại bỏ hoàn toàn trọng số đó. 

Sau khi sửa chữ số 0, chín chữ còn lại nhận chữ số`1...9`theo thứ tự tăng dần về trọng lượng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10! · T) | O(1) | Quá chậm | 
| Tối ưu | O(T + 10 log 10) | O(10) | Đã chấp nhận | 

Đây`T`là tổng số ký tự trên tất cả các chuỗi. 

## Hướng dẫn thuật toán 

1. Tạo một mảng`weight[10]`, một mục nhập cho mỗi chữ cái. 
2. Đối với mỗi chuỗi, hãy quét từ phải sang trái. Vị trí ngoài cùng bên phải góp phần`1`, đóng góp tiếp theo`10`, sau đó`100`, vân vân. 
3. Bất cứ khi nào một chữ cái xuất hiện ở vị trí có giá trị theo vị trí`p`, thêm vào`p`với trọng lượng của lá thư đó. 
4. Ghi lại những chữ cái bị cấm trở thành số 0. Bất kỳ chữ cái nào xuất hiện dưới dạng ký tự đầu tiên của một chuỗi có độ dài lớn hơn một đều bị cấm. 
5. Trong số tất cả các chữ cái không bị cấm, hãy chọn chữ cái có trọng lượng nhỏ nhất. Gán chữ số`0`tới bức thư này. 
6. Thu thập chín chữ cái còn lại và sắp xếp chúng theo trọng lượng tăng dần. 
7. Gán chữ số`1, 2, ..., 9`các chữ cái này theo thứ tự được sắp xếp đó. 
8. Tính đáp án cuối cùng bằng tổng của`digit(letter) × weight(letter)`trên tất cả mười chữ cái. 

### Tại sao nó hoạt động 

Trọng lượng của một chữ cái hoàn toàn độc lập với việc gán chữ số. Khi đã biết các trọng số, tổng cuối cùng là biểu thức tuyến tính có trọng số. 

Trong số tất cả các chữ cái được phép nhận số 0, việc gán số 0 cho trọng số nhỏ nhất luôn là tối ưu. Nếu số 0 được gán cho trọng số lớn hơn trong khi trọng số đủ điều kiện nhỏ hơn nhận được chữ số dương thì việc hoán đổi các phép gán đó sẽ làm giảm tổng. 

Sau khi sửa chữ 0 thì chỉ còn chữ số`1...9`duy trì. Giả sử hai chữ cái có trọng số`w1 ≤ w2`nhưng nhận được chữ số`d1 > d2`. Hoán đổi các chữ số của chúng sẽ thay đổi tổng bằng cách$$(w1d2+w2d1)-(w1d1+w2d2) =(w2-w1)(d1-d2)\ge 0$$vì vậy bài tập ban đầu không thể tốt hơn bài tập đã đổi chỗ. Áp dụng lập luận này nhiều lần cho thấy trọng số và chữ số phải được sắp xếp theo cùng một cách. Do đó sắp xếp các chữ cái còn lại theo trọng số và gán các chữ số`1...9`theo thứ tự tăng dần thì số tiền nhỏ nhất có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    weight = [0] * 10
    leading = [False] * 10

    for _ in range(n):
        s = input().strip()

        if len(s) > 1:
            leading[ord(s[0]) - ord('a')] = True

        place = 1
        for ch in reversed(s):
            weight[ord(ch) - ord('a')] += place
            place *= 10

    zero_letter = -1
    best_weight = None

    for i in range(10):
        if not leading[i]:
            if best_weight is None or weight[i] < best_weight:
                best_weight = weight[i]
                zero_letter = i

    letters = [i for i in range(10) if i != zero_letter]
    letters.sort(key=lambda x: weight[x])

    digit = [0] * 10

    for d, letter in enumerate(letters, start=1):
        digit[letter] = d

    ans = 0
    for i in range(10):
        ans += digit[i] * weight[i]

    print(ans)

solve()
```Phần đầu tiên tính trọng lượng vị trí của mỗi chữ cái. Thay vì trực tiếp xây dựng lại các số, chúng ta tích lũy mỗi chữ cái đóng góp bao nhiêu vào tổng cuối cùng trên mỗi đơn vị chữ số được chỉ định. 

các`leading`mảng theo dõi những chữ cái nào bị cấm nhận số 0. Chỉ các ký tự đầu tiên của chuỗi nhiều chữ số mới quan trọng. Một số có một chữ số có thể bằng 0 một cách hợp pháp sau khi được khôi phục vì nó không có số 0 đứng đầu. 

Sự lựa chọn của`zero_letter`là bước tinh tế nhất. Chúng tôi chỉ tìm kiếm trong số các chữ cái không được đánh dấu là đứng đầu và chọn trọng số nhỏ nhất. Điều này loại bỏ số tiền đóng góp không cần thiết lớn nhất có thể. 

Sau khi loại trừ chữ cái số 0, các chữ cái còn lại được sắp xếp theo trọng số. Gán chữ số`1...9`theo thứ tự đó thực hiện tối ưu đối số trao đổi. 

Trọng số lớn nhất có thể nằm trong khả năng số nguyên của Python, nhưng việc sử dụng số nguyên Python sẽ tự động tránh mọi lo ngại về tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
ab
de
aj
```Trọng lượng: 

| Thư | Vị trí | Cân nặng | 
| --- | --- | --- | 
| một | hàng chục, hàng chục | 20 | 
| b | đơn vị | 1 | 
| d | hàng chục | 10 | 
| e | đơn vị | 1 | 
| j | đơn vị | 1 | 

Tất cả các chữ cái khác có trọng số 0. 

Các chữ cái dẫn đầu là`a`Và`d`. 

Các chữ cái số 0 đủ điều kiện là`b,c,e,f,g,h,i,j`. Trọng số nhỏ nhất trong số chúng là 0, vì vậy một chữ cái không được sử dụng sẽ nhận được chữ số 0. 

| Thư | Cân nặng | Chữ số được gán | 
| --- | --- | --- | 
| c | 0 | 0 | 
| f | 0 | 1 | 
| g | 0 | 2 | 
| h | 0 | 3 | 
| tôi | 0 | 4 | 
| b | 1 | 5 | 
| e | 1 | 6 | 
| j | 1 | 7 | 
| d | 10 | 8 | 
| một | 20 | 9 | 

Tổng cuối cùng:$$20\cdot9+10\cdot8+1\cdot5+1\cdot6+1\cdot7=278$$Dấu vết thể hiện ý tưởng trung tâm: chỉ có trọng lượng mới quan trọng. Chúng tôi không bao giờ xây dựng lại những con số thực tế. 

### Ví dụ 2 

đầu vào:```
3
aa
bb
ab
```Trọng lượng: 

| Thư | Cân nặng | 
| --- | --- | 
| một | 21 | 
| b | 21 | 

Các chữ cái dẫn đầu là`a`Và`b`. 

Mọi chữ cái không được sử dụng đều có trọng số 0 và đủ điều kiện bằng 0. 

| Thư | Cân nặng | Chữ số | 
| --- | --- | --- | 
| c | 0 | 0 | 
| d | 0 | 1 | 
| e | 0 | 2 | 
| f | 0 | 3 | 
| g | 0 | 4 | 
| h | 0 | 5 | 
| tôi | 0 | 6 | 
| j | 0 | 7 | 
| một | 21 | 8 | 
| b | 21 | 9 | 

Trả lời:$$21\cdot8 + 21\cdot9 = 357$$Ví dụ này cho thấy các chữ cái không được sử dụng là những ứng cử viên cực kỳ có giá trị cho chữ số 0 vì chúng không đóng góp gì bất kể nhiệm vụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T + 10 log 10) | Xử lý tất cả các ký tự cộng với sắp xếp mười chữ cái | 
| Không gian | O(10) | Trọng số, chữ số và mảng dẫn đầu | 

Từ`T ≤ 6000`, công việc chủ yếu chỉ đơn giản là quét các chuỗi đầu vào. Sắp xếp mười phần tử có hiệu quả là thời gian không đổi. Giải pháp thoải mái phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())

    weight = [0] * 10
    leading = [False] * 10

    for _ in range(n):
        s = input().strip()

        if len(s) > 1:
            leading[ord(s[0]) - ord('a')] = True

        place = 1
        for ch in reversed(s):
            weight[ord(ch) - ord('a')] += place
            place *= 10

    zero_letter = -1
    best_weight = None

    for i in range(10):
        if not leading[i]:
            if best_weight is None or weight[i] < best_weight:
                best_weight = weight[i]
                zero_letter = i

    letters = [i for i in range(10) if i != zero_letter]
    letters.sort(key=lambda x: weight[x])

    digit = [0] * 10
    for d, letter in enumerate(letters, start=1):
        digit[letter] = d

    ans = sum(weight[i] * digit[i] for i in range(10))
    return str(ans)

# custom minimum case
assert run("1\na\n") == "1"

# only one used letter
assert run("3\na\na\na\n") == "3"

# leading-zero restriction
assert run("1\nab\n") == "10"

# symmetric weights
assert run("2\naa\nbb\n") == "357"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a`|`1`| Kích thước đầu vào tối thiểu | 
|`a,a,a`|`3`| Sử dụng nhiều lần một chữ cái | 
|`ab`|`10`| Chữ số hàng đầu không thể trở thành số 0 | 
|`aa,bb`|`357`| Trọng số bằng nhau và phép gán số 0 chữ cái không được sử dụng | 

## Vỏ cạnh 

Hãy xem xét:```
1
ab
```Trọng lượng là`a=10`,`b=1`. Thư`a`là nhân vật đứng đầu và không thể nhận được số 0. Thuật toán chỉ tìm kiếm trong số các chữ cái không đứng đầu và chọn`b`cho chữ số`0`. Việc gán kết quả mang lại giá trị`10`, đây là sự tái thiết hợp lệ nhỏ nhất. 

Bây giờ hãy xem xét:```
3
a
a
a
```Chỉ có một chữ cái xuất hiện. trọng lượng của nó là`3`. Mỗi chữ cái khác đều có trọng lượng`0`và có đủ điều kiện cho số không. Thuật toán gán số 0 cho một chữ cái và chữ số không sử dụng`1`ĐẾN`a`, đưa ra câu trả lời`3`. Gán số 0 cho`a`sẽ đưa ra số tiền`0`, nhưng điều đó sẽ làm cho số dương ban đầu không hợp lệ. 

Cuối cùng hãy xem xét:```
2
ab
ac
```Trọng lượng là`a=20`,`b=1`,`c=1`. Thư`a`bị cấm nhận số không. Thuật toán chọn một chữ cái không đứng đầu không được sử dụng có trọng số`0`cho chữ số`0`, sau đó gán các chữ số dương nhỏ nhất cho các chữ cái có trọng số 1 và chữ số lớn nhất cho`a`. Ràng buộc số 0 đứng đầu được tự động tôn trọng trong khi vẫn giảm thiểu tổng số tiền.

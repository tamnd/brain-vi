---
title: "CF 1863A - Kênh"
description: "Một kênh có n người đăng ký. Khi một bài đăng mới được xuất bản, chính xác là người đăng ký đang trực tuyến, vì vậy những người đó sẽ đọc bài đăng ngay lập tức. Sau đó, chúng tôi nhận được một chuỗi thông báo. '+' có nghĩa là một số người đăng ký đã trực tuyến và '-' có nghĩa là một số người đăng ký đã ngoại tuyến."
date: "2026-06-09T00:01:18+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1863
codeforces_index: "A"
codeforces_contest_name: "Pinely Round 2 (Div. 1 + Div. 2)"
rating: 800
weight: 1863
solve_time_s: 114
verified: true
draft: false
---

[CF 1863A - Kênh](https://codeforces.com/problemset/problem/1863/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một kênh có`n`người đăng ký. Khi một bài viết mới được xuất bản, chính xác`a`người đăng ký đang trực tuyến, vì vậy những người đăng ký đó`a`mọi người đọc ngay bài viết. 

Sau đó, chúng tôi nhận được một chuỗi thông báo. MỘT`'+'`có nghĩa là một số người đăng ký đã trực tuyến và`'-'`có nghĩa là một số thuê bao đã ngoại tuyến. Chúng tôi biết trình tự này hợp lệ nhưng chúng tôi không biết mỗi thông báo đề cập đến người đăng ký cụ thể nào. 

Câu hỏi đặt ra là liệu mọi người đăng ký đã đọc bài viết này vào một thời điểm nào đó hay chưa. 

Có ba câu trả lời có thể.`YES`có nghĩa là mọi người đăng ký đều phải đọc bài đăng, bất kể thông báo đề cập đến người nào.`NO`có nghĩa là không thể chỉ định người đăng ký nhận thông báo cho phép mọi người đọc bài đăng.`MAYBE`có nghĩa là một số bài tập cho phép mọi người đọc bài viết còn những bài khác thì không. 

Những hạn chế là cực kỳ nhỏ. Cả hai`n`Và`q`nhiều nhất là 100 và có nhiều nhất 500 ca kiểm thử. Ngay cả một mô phỏng đắt tiền cũng sẽ vừa vặn thoải mái trong giới hạn. Thách thức không phải là hiệu suất mà là lý do chính xác về thông tin mà thông báo tiết lộ. 

Khó khăn chính là chúng ta không bao giờ tìm hiểu được danh tính. Khi ai đó trực tuyến, người đó có thể là người đăng ký chưa từng đọc bài đăng trước đó hoặc có thể là người đã trực tuyến trước đó và quay lại sau khi ngoại tuyến. 

Hãy xem xét ví dụ sau:```text
n = 5
a = 4
s = "-+"
```Ban đầu có 4 người đã đọc bài viết. 

Sau khi`-`, một người rời đi. 

Sau khi`+`, cùng một người quay lại hoặc người đăng ký thứ năm trực tuyến lần đầu tiên. 

Cả hai cách giải thích đều có giá trị. Trong một trường hợp, tất cả năm người đăng ký đều đọc bài đăng, trong trường hợp còn lại chỉ có bốn người đọc. Câu trả lời đúng là`MAYBE`. 

Một sai lầm dễ mắc phải khác là chỉ nhìn vào số lượng người đăng ký trực tuyến cuối cùng.```text
n = 5
a = 2
s = "++-"
```Số lượng trực tuyến trở thành 2 → 3 → 4 → 3. 

Số lượng người đăng ký trực tuyến tối đa từng thấy chỉ là 4, vì vậy ít nhất một người đăng ký chưa bao giờ có cơ hội trực tuyến và đọc bài đăng. Câu trả lời là`NO`. 

Một trường hợp khác xảy ra khi mọi người đều trực tuyến ngay từ đầu.```text
n = 5
a = 5
s = "--+"
```Tất cả người đăng ký đọc bài viết ngay lập tức khi nó được xuất bản. Thông báo trong tương lai không thành vấn đề. Câu trả lời là`YES`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng theo dõi chính xác những người đăng ký đã đọc bài đăng. Bất cứ khi nào một`'+'`xảy ra, chúng ta cần phải quyết định thuê bao ngoại tuyến nào đã trực tuyến. Vì danh tính không xác định nên mọi thông báo có thể phân nhánh thành nhiều khả năng. 

Số lượng trạng thái có thể tăng theo cấp số nhân với số lượng thông báo. Ngay cả khi chỉ có 100 thông báo, việc tìm kiếm như vậy sẽ hoàn toàn không thực tế. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến danh tính. Chúng tôi chỉ cần biết liệu có thể, không thể hoặc đảm bảo rằng tất cả người đăng ký đã đọc bài đăng. 

Hãy nghĩ về số lượng người đã từng có cơ hội đọc bài đăng. 

Ban đầu,`a`người đăng ký đã đọc nó. 

Mọi`'+'`thông báo có khả năng có thể giới thiệu một người đăng ký mới chưa bao giờ trực tuyến kể từ khi bài đăng được xuất bản. Nếu chúng ta giả định một cách lạc quan mọi`'+'`mang lại một người đăng ký chưa đọc mới thì số lượng người đọc tối đa có thể là:```text
a + number_of_pluses
```giới hạn ở`n`. 

Nếu thậm chí con số lạc quan này nhỏ hơn`n`, thì một số người đăng ký không bao giờ có thể đọc được bài viết. Câu trả lời là`NO`. 

Bây giờ hãy xem xét thời điểm tất cả người đăng ký được đảm bảo đã đọc bài đăng. 

Nếu tại bất kỳ thời điểm nào số lượng người đăng ký trực tuyến đạt`n`, thì mọi thuê bao phải trực tuyến đồng thời vào thời điểm đó. Vì bất kỳ ai trực tuyến đều đọc bài đăng nên tất cả người đăng ký chắc chắn đã đọc nó. Một khi điều này xảy ra, những thông báo sau này không thể thay đổi được sự thật. 

Vì vậy, chúng tôi mô phỏng số lượng trực tuyến. Nếu nó đạt tới`n`, câu trả lời là`YES`. 

Nếu cả hai điều kiện đều không được áp dụng thì việc tiếp cận tất cả người đăng ký là có thể nhưng không bắt buộc. Câu trả lời là`MAYBE`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | số mũ trong`q`| Hàm mũ | Quá chậm | 
| Tối ưu | O(q) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với số lượng trực tuyến hiện tại bằng`a`. 

2. Đếm xem có bao nhiêu`'+'`thông báo xuất hiện trong chuỗi. 

3. Nếu`a + plus_count < n`, đầu ra`NO`. 

Đây là số lượng người đăng ký riêng biệt lớn nhất có thể đã đọc bài đăng. Nếu nó vẫn ở dưới`n`, việc tiếp cận tất cả người đăng ký là điều không thể. 

4. Nếu không, hãy mô phỏng số lượng trực tuyến thông qua chuỗi thông báo. 

5. Đối với mỗi`'+'`, tăng số lượng trực tuyến hiện tại lên một. 

6. Đối với mỗi`'-'`, giảm số lượng trực tuyến hiện tại xuống một. 

7. Sau mỗi lần cập nhật, hãy kiểm tra xem số lượng trực tuyến hiện tại có bằng không`n`. 

Nếu có, xuất ra`YES`. 

8. Nếu quá trình mô phỏng kết thúc mà không đạt được`n`, đầu ra`MAYBE`. 

### Tại sao nó hoạt động 

Lần kiểm tra đầu tiên xác định khả năng. Ban đầu`a`mọi người đã đọc bài viết. MỘT`'+'`thông báo là sự kiện duy nhất có thể tạo người đọc mới và mỗi sự kiện như vậy có thể thêm tối đa một người đăng ký chưa đọc trước đó. Như vậy`a + plus_count`là giới hạn trên của số lượng người đăng ký có thể đọc bài đăng. Nếu giới hạn đó ở dưới`n`, thành công là điều không thể. 

Để đảm bảo thành công, hãy lưu ý rằng khi số lượng trực tuyến trở nên chính xác`n`, tất cả người đăng ký đều trực tuyến cùng một lúc. Mọi người đăng ký chắc chắn đã đọc bài viết tại thời điểm đó. Vì việc đọc là vĩnh viễn nên câu trả lời là ngay lập tức`YES`. 

Nếu thành công là có thể nhưng chúng tôi không bao giờ đạt được số lượng trực tuyến`n`, tồn tại những cách giải thích trong đó mọi`'+'`giới thiệu người đăng ký mới và cách diễn giải trong đó một số`'+'`các sự kiện tương ứng với những người đăng ký quay trở lại. Cả hai kết quả đều khả thi nên câu trả lời đúng là`MAYBE`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, a, q = map(int, input().split())
    s = input().strip()

    if a + s.count('+') < n:
        print("NO")
        continue

    cur = a
    guaranteed = (cur == n)

    for ch in s:
        if ch == '+':
            cur += 1
        else:
            cur -= 1

        if cur == n:
            guaranteed = True

    if guaranteed:
        print("YES")
    else:
        print("MAYBE")
```Phần đầu tiên tính toán số lượng người đăng ký riêng biệt tối đa có thể đọc bài đăng. Nếu giá trị đó thấp hơn`n`, chúng ta biết ngay câu trả lời là`NO`. 

Nếu không, chúng tôi sẽ mô phỏng số lượng trực tuyến. Chúng tôi không theo dõi danh tính vì điều kiện đảm bảo chỉ phụ thuộc vào việc số lượng có đạt đến hay không`n`. 

Biến`guaranteed`lưu trữ xem chúng tôi đã thấy tất cả người đăng ký trực tuyến cùng một lúc hay chưa. Nó cũng phải được khởi tạo với`cur == n`vì mọi người có thể đã trực tuyến trước khi có bất kỳ thông báo nào đến. 

Mô phỏng chỉ áp dụng từng thông báo và kiểm tra xem số lượng hiện tại có trở thành`n`. 

Không cần xử lý đặc biệt đối với các trạng thái không hợp lệ vì sự cố đảm bảo rằng trình tự thông báo nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```text
n = 5
a = 0
s = "++++-++"
```Đầu tiên, tính khả năng:```text
a + pluses = 0 + 6 = 6 ≥ 5
```Vì thế`NO`là không thể. 

| Bước | Sự kiện | Đếm trực tuyến | 
|---|---|---| 
| Bắt đầu | - | 0 | 
| 1 | + | 1 | 
| 2 | + | 2 | 
| 3 | + | 3 | 
| 4 | + | 4 | 
| 5 | - | 3 | 
| 6 | + | 4 | 
| 7 | + | 5 | 

Số lượng đạt tới 5, vì vậy tất cả người đăng ký đều trực tuyến cùng một lúc. 

Đầu ra:```text
YES
```Dấu vết này thể hiện điều kiện bảo hành. Sau khi đếm xong`n`, mọi người đăng ký chắc chắn đã đọc bài viết. 

### Ví dụ 2 

đầu vào:```text
n = 5
a = 4
s = "-+"
```Kiểm tra khả năng:```text
a + pluses = 4 + 1 = 5
```Vì vậy, việc tiếp cận tất cả người đăng ký là có thể. 

| Bước | Sự kiện | Đếm trực tuyến | 
|---|---|---| 
| Bắt đầu | - | 4 | 
| 1 | - | 3 | 
| 2 | + | 4 | 

Số lượng không bao giờ đạt đến 5. 

Đầu ra:```text
MAYBE
```Dấu vết cho thấy tại sao sự chắc chắn là không thể. trận chung kết`+`có thể là người đăng ký thứ năm bị thiếu hoặc có thể là người đã đọc bài đăng trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(q) | Một lượt để đếm`'+'`và một lượt để mô phỏng | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Từ`q ≤ 100`, thời gian chạy rất nhỏ. Ngay cả với 500 trường hợp thử nghiệm, tổng công việc chỉ có vài chục nghìn thao tác, thấp hơn nhiều so với giới hạn. 

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
        n, a, q = map(int, input().split())
        s = input().strip()

        if a + s.count('+') < n:
            ans.append("NO")
            continue

        cur = a
        guaranteed = (cur == n)

        for ch in s:
            if ch == '+':
                cur += 1
            else:
                cur -= 1

            if cur == n:
                guaranteed = True

        ans.append("YES" if guaranteed else "MAYBE")

    return "\n".join(ans) + "\n"

# provided sample
assert run(
"""4
5 5 3
--+
5 2 3
++-
5 4 2
-+
5 0 7
++++-++
"""
) == """YES
NO
MAYBE
YES
"""

# minimum values
assert run(
"""1
1 1 1
-
"""
) == """YES
"""

# impossible to reach all subscribers
assert run(
"""1
5 1 2
++
"""
) == """NO
"""

# possible but not guaranteed
assert run(
"""1
5 4 1
+
"""
) == """YES
"""

# never reaches n although possible
assert run(
"""1
5 4 2
-+
"""
) == """MAYBE
"""

# everyone online initially
assert run(
"""1
100 100 100
----------------------------------------------------------------------------------------------------
"""
) == """YES
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`n=1, a=1`| CÓ | Trạng thái ban đầu đã chứa tất cả người đăng ký | 
|`n=5, a=1, "++"`| KHÔNG | Ngay cả số lượng độc giả lạc quan cũng không đủ | 
|`n=5, a=4, "+"`| CÓ | Đếm lượt tiếp cận`n`trong quá trình mô phỏng | 
|`n=5, a=4, "-+"`| CÓ THỂ | Có thể nhưng không đảm bảo | 
|`n=100, a=100`| CÓ | Ranh giới kích thước tối đa với tất cả người đăng ký trực tuyến ban đầu | 

## Vỏ cạnh 

### Mọi người đều trực tuyến vào thời điểm xuất bản 

đầu vào:```text
1
5 5 3
--+
```Thuật toán bắt đầu với`cur = 5`, vốn đã bằng`n`. Biến`guaranteed`trở thành`True`trước khi xử lý bất kỳ thông báo nào. 

Mặc dù sau đó người đăng ký ngoại tuyến nhưng họ đã đọc bài đăng. Đầu ra là:```text
YES
```Giải pháp chỉ kiểm tra số lượng sau khi xử lý thông báo có thể bỏ sót trường hợp này. 

### Không đủ thông báo '+' 

đầu vào:```text
1
5 2 3
++-
```Chúng tôi tính toán:```text
2 + 2 = 4
```Số lượng độc giả tối đa có thể chỉ là bốn. Không có sự phân công danh tính nào có thể khiến cả năm người đăng ký đọc bài đăng. 

Thuật toán ngay lập tức xuất ra:```text
NO
```mà không cần bất kỳ mô phỏng nào. 

### Có thể nhưng không chắc chắn 

đầu vào:```text
1
5 4 2
-+
```Việc kiểm tra khả năng mang lại:```text
4 + 1 = 5
```nên thành công là có thể. 

Số lượng trực tuyến là:```text
4 -> 3 -> 4
```Số lượng không bao giờ đạt tới 5 nên chúng tôi không thể đảm bảo tất cả người đăng ký đã đọc bài đăng. 

Một cách giải thích hợp lệ là cùng một người đăng ký rời đi và quay lại, chỉ đưa ra bốn độc giả. Một điều nữa là người đăng ký còn thiếu sẽ trực tuyến vào cuối, cung cấp cho năm độc giả. 

Thuật toán xuất ra chính xác:```text
MAYBE
```

---
title: "CF 105689C - Múa Rồng"
description: "Dòng múa rồng đã có thứ tự cố định rồi. James có thể loại bỏ một số vũ công, nhưng những vũ công ở lại vẫn phải tôn trọng mối quan hệ “ai đứng sau ai” ban đầu."
date: "2026-06-26T09:45:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105689
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 1-29-25 Div. 2 (Beginner)"
rating: 0
weight: 105689
solve_time_s: 43
verified: true
draft: false
---

[CF 105689C - Múa Rồng](https://codeforces.com/problemset/problem/105689/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Dòng múa rồng đã có thứ tự cố định rồi. James có thể loại bỏ một số vũ công, nhưng những vũ công ở lại vẫn phải tôn trọng mối quan hệ “ai đứng sau ai” ban đầu. Một vũ công chỉ có thể giữ người ngay trước mặt họ so với dòng ban đầu, trừ khi vũ công đó trở thành vũ công đầu tiên ở dòng mới. 

Điều này có nghĩa là các vũ công được chọn phải tạo thành một đoạn liên tục của dòng ban đầu. Nếu chúng ta bỏ qua một vũ công ở giữa, vũ công được chọn tiếp theo sẽ buộc phải đứng sau một người mới, điều này là không được phép. 

Yêu cầu còn lại là các vũ công lân cận trong phân khúc này phải có chiều cao đủ gần. Đối với mỗi cặp vũ công liên tiếp trong phần đã chọn, chênh lệch tuyệt đối về chiều cao của họ không được vượt quá`k`. Nhiệm vụ là tìm độ dài tối đa của đoạn liền kề đó. 

Đầu vào chứa số lượng vũ công, chênh lệch chiều cao tối đa được phép và chiều cao của các vũ công theo thứ tự ban đầu của họ. Đầu ra là số lượng vũ công lớn nhất có thể tham gia một điệu múa rồng hợp lệ. 

Số lượng vũ công có thể đạt tới`2 * 10^5`, vì vậy thuật toán kiểm tra mọi phân đoạn có thể là không thực tế. có khoảng`n^2 / 2`các phân khúc có thể, xung quanh`2 * 10^10`kiểm tra trong trường hợp xấu nhất. Một giải pháp phải xử lý dòng trong thời gian gần như tuyến tính. 

Các trường hợp biên chính xuất phát từ việc coi bài toán như một bài toán dãy con tổng quát thay vì bài toán phân đoạn liền kề. Ví dụ, hãy xem xét:```
5 1
1 10 2 3 4
```Đầu ra đúng là:```
3
```Phân đoạn hợp lệ là`2 3 4`. Một cách tiếp cận bất cẩn có thể chọn`1 2 3 4`bằng cách bỏ qua`10`, nhưng điều đó sẽ vi phạm mối quan hệ thứ tự ban đầu vì vũ công`2`bây giờ sẽ đứng đằng sau một người khác. 

Một trường hợp khác là khi mọi cặp liền kề đều hợp lệ:```
4 100
5 6 7 8
```Câu trả lời là:```
4
```Thuật toán không được dừng sớm hoặc yêu cầu phân đoạn phải có chính xác kích thước tối thiểu nào đó một cách không chính xác. 

Trường hợp ranh giới cuối cùng là:```
1 0
42
```Câu trả lời là:```
1
```Một vũ công duy nhất luôn là một màn múa rồng hợp lệ vì không có cặp lân cận nào có thể vi phạm điều kiện về chiều cao. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi vũ công bắt đầu có thể và mở rộng đoạn sang bên phải cho đến khi chênh lệch chiều cao trở nên quá lớn. Đối với mỗi vị trí xuất phát, chúng tôi có thể quét hầu hết các vũ công còn lại. Điều này đúng vì nó kiểm tra mọi phân đoạn liền kề có thể có, nhưng trong trường hợp xấu nhất, nó thực hiện khoảng`n * (n + 1) / 2`so sánh. Với`n = 200000`, đại khái là vậy`2 * 10^10`hoạt động vượt xa thời hạn. 

Cấu trúc của bài toán cho chúng ta cái nhìn đơn giản hơn. Một phân đoạn hợp lệ chính xác khi mọi cặp liền kề bên trong nó hợp lệ. Trong khi quét từ trái sang phải, nếu vũ công hiện tại có thể đứng phía sau vũ công trước đó thì đoạn hợp lệ hiện tại có thể được mở rộng. Nếu chênh lệch chiều cao quá lớn thì không có phân đoạn nào kết thúc ở vũ công hiện tại có thể bao gồm vũ công trước đó, vì vậy vũ công hiện tại phải bắt đầu một phân đoạn mới. 

Điều này biến vấn đề thành việc duy trì độ dài của hậu tố hợp lệ hiện tại trong khi vẫn theo dõi câu trả lời hay nhất được thấy cho đến nay. Mỗi vũ công được xử lý một lần, do đó giải pháp trở thành tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng vũ công, chênh lệch chiều cao tối đa cho phép và khoảng chiều cao. 

2. Bắt đầu với độ dài đoạn hiện tại là`1`và câu trả lời hay nhất`1`. Một vũ công duy nhất luôn tạo thành một dòng hợp lệ. 

3. Quét các vũ công từ vị trí thứ hai trở đi. So sánh chiều cao của vũ công hiện tại với chiều cao của vũ công trước đó. 

4. Nếu chênh lệch tuyệt đối lớn nhất`k`, mở rộng đoạn hiện tại bằng cách tăng độ dài của nó. Điều này hợp lệ vì vũ công mới có thể đứng ngay sau vũ công trước đó. 

5. Nếu chênh lệch lớn hơn`k`, vũ công hiện tại không thể ở cùng phân khúc. Bắt đầu một phân đoạn mới chỉ chứa vũ công này. 

6. Sau mỗi lần cập nhật, hãy so sánh độ dài đoạn hiện tại với câu trả lời đúng nhất và giữ mức tối đa. 

Lý do điều này có hiệu quả là thông tin duy nhất cần có về một phân đoạn là liệu cặp liền kề cuối cùng của nó có hợp lệ hay không. Nếu một cặp vi phạm quy tắc thì mọi phân đoạn bao gồm cả hai vũ công đều không hợp lệ, do đó, phân đoạn hợp lệ duy nhất có thể kết thúc ở vũ công hiện tại phải bắt đầu ở vũ công hiện tại hoặc muộn hơn. Phân đoạn được duy trì luôn là phân đoạn hợp lệ dài nhất kết thúc ở vị trí hiện tại, có nghĩa là mức tối đa được tìm thấy trong quá trình quét là câu trả lời chung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    h = list(map(int, input().split()))

    ans = 1
    cur = 1

    for i in range(1, n):
        if abs(h[i] - h[i - 1]) <= k:
            cur += 1
        else:
            cur = 1
        if cur > ans:
            ans = cur

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp chỉ giữ lại hai quầy.`cur`đại diện cho độ dài của đoạn hợp lệ kết thúc ở vũ công hiện tại.`ans`lưu trữ độ dài phân đoạn lớn nhất gặp phải. 

Vòng lặp bắt đầu tại chỉ mục`1`bởi vì người nhảy đầu tiên không có người nhảy trước đó để so sánh. Khi chênh lệch chiều cao liền kề được chấp nhận, phân khúc hiện tại sẽ phát triển. Nếu không, những người nhảy trước đó không còn có thể là một phần của phân đoạn hợp lệ kết thúc ở đây nữa, do đó bộ đếm sẽ đặt lại về một. 

Không lập chỉ mục vượt quá ranh giới mảng được sử dụng vì quá trình quét chỉ truy cập`i`Và`i - 1`sau đó`i`đã đạt tới`1`. Số nguyên Python cũng xử lý các giá trị chiều cao lớn một cách an toàn mà không lo tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 4
1 2 1 2 6 7 1
```| Vị trí | Chiều cao | Sự khác biệt trước đó | Độ dài hiện tại | Câu trả lời hay nhất | 
|---|---|---|---|---| 
| 1 | 1 | - | 1 | 1 | 
| 2 | 2 | 1 | 2 | 2 | 
| 3 | 1 | 1 | 3 | 3 | 
| 4 | 2 | 1 | 4 | 4 | 
| 5 | 6 | 4 | 5 | 5 | 
| 6 | 7 | 1 | 6 | 6 | 
| 7 | 1 | 6 | 1 | 6 | 

Sáu vũ công đầu tiên tạo thành một phân đoạn hợp lệ. Người nhảy cuối cùng không thể tham gia vì chênh lệch chiều cao so với`7`ĐẾN`1`là quá lớn. 

### Ví dụ 2 

đầu vào:```
6 3
7 4 7 4 8 9
```| Vị trí | Chiều cao | Sự khác biệt trước đó | Độ dài hiện tại | Câu trả lời hay nhất | 
|---|---|---|---|---| 
| 1 | 7 | - | 1 | 1 | 
| 2 | 4 | 3 | 2 | 2 | 
| 3 | 7 | 3 | 3 | 3 | 
| 4 | 4 | 3 | 4 | 4 | 
| 5 | 8 | 4 | 1 | 4 | 
| 6 | 9 | 1 | 2 | 4 | 

Bốn vũ công đầu tiên là nhóm liên tiếp hợp lệ lâu nhất. Cặp đôi`4, 8`phá vỡ phân khúc vì sự khác biệt của chúng lớn hơn`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(n) | Mỗi vũ công được viếng thăm đúng một lần. | 
| Không gian | O(1) | Chỉ các bộ đếm được lưu trữ sau khi đọc chiều cao. | 

Thời gian chạy tuyến tính phù hợp cho`n = 200000`bởi vì thuật toán chỉ thực hiện một lượng công việc nhỏ không đổi cho mỗi vũ công. Việc sử dụng bộ nhớ cũng ở mức tối thiểu ngoài việc lưu trữ mảng đầu vào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n, k = map(int, sys.stdin.readline().split())
    h = list(map(int, sys.stdin.readline().split()))

    ans = 1
    cur = 1

    for i in range(1, n):
        if abs(h[i] - h[i - 1]) <= k:
            cur += 1
        else:
            cur = 1
        ans = max(ans, cur)

    sys.stdin = old_stdin
    return str(ans)

assert solve_data("7 4\n1 2 1 2 6 7 1\n") == "6"
assert solve_data("6 3\n7 4 7 4 8 9\n") == "4"

assert solve_data("1 0\n42\n") == "1"
assert solve_data("5 1\n1 10 2 3 4\n") == "3"
assert solve_data("4 100\n5 6 7 8\n") == "4"
assert solve_data("5 0\n9 9 8 8 8\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`1 0 / 42`|`1`| Đầu vào kích thước tối thiểu với một vũ công | 
|`5 1 / 1 10 2 3 4`|`3`| Ngăn chặn việc coi câu trả lời là một dãy con tùy ý | 
|`4 100 / 5 6 7 8`|`4`| Tất cả các cặp liền kề đều hợp lệ | 
|`5 0 / 9 9 8 8 8`|`3`| Kiểm tra ranh giới bình đẳng chính xác và đặt lại phân đoạn | 

## Vỏ cạnh 

Đối với trường hợp vũ công đơn:```
1 0
42
```Thuật toán khởi tạo cả hai`cur`Và`ans`ĐẾN`1`. Vòng lặp không bao giờ chạy nên câu trả lời vẫn còn đó`1`, phù hợp với thực tế là một vũ công luôn tạo thành một đường hợp lệ. 

Đối với bẫy nhảy nhảy:```
5 1
1 10 2 3 4
```Quá trình quét bắt đầu với độ dài`1`. Sự khác biệt giữa`1`Và`10`quá lớn nên đoạn này sẽ đặt lại ở`10`. Sự khác biệt giữa`10`Và`2`cũng đặt lại nó. Ba vũ công cuối cùng có sự khác biệt`1`Và`1`, đưa ra một đoạn có độ dài hợp lệ`3`. Thuật toán không bao giờ kết hợp sai các vũ công bị tách biệt. 

Đối với trường hợp tất cả hợp lệ:```
4 100
5 6 7 8
```Mọi hiệu liền kề đều nhỏ hơn`100`, do đó chiều dài hiện tại tăng từ`1`ĐẾN`4`. Câu trả lời trở thành`4`, cho thấy thuật toán có thể tiếp tục kéo dài một đoạn cho đến hết. 

Đối với các giá trị lặp lại có giới hạn nghiêm ngặt:```
5 0
9 9 8 8 8
```Cặp đầu tiên của`9`s là hợp lệ, cho biết độ dài`2`. Cặp đôi`9`Và`8`ngắt đoạn, do đó quá trình quét bắt đầu lại ở lần đầu tiên`8`. Ba cuối cùng`8`s đưa ra câu trả lời, đưa ra`3`. Điều này kiểm tra xem việc thiết lập lại có xảy ra chính xác ở cặp liền kề không hợp lệ hay không.

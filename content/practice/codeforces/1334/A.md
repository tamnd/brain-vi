---
title: "CF 1334A - Thống kê cấp độ"
description: "Chúng tôi quan sát số liệu thống kê của một cấp độ trò chơi nhiều lần. Ở mỗi lần quan sát, chúng tôi biết hai giá trị: cấp độ đã được chơi bao nhiêu lần và cấp độ đó đã được hoàn thành bao nhiêu lần. Một nỗ lực thành công sẽ tăng cả hai con số lên một con số cùng một lúc."
date: "2026-06-11T15:59:05+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1334
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 85 (Rated for Div. 2)"
rating: 1200
weight: 1334
solve_time_s: 120
verified: true
draft: false
---

[CF 1334A - Thống kê cấp độ](https://codeforces.com/problemset/problem/1334/A) 

**Đánh giá:** 1200 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi quan sát số liệu thống kê của một cấp độ trò chơi nhiều lần. Ở mỗi lần quan sát, chúng tôi biết hai giá trị: cấp độ đã được chơi bao nhiêu lần và cấp độ đó đã được hoàn thành bao nhiêu lần. 

Một nỗ lực thành công sẽ tăng cả hai con số lên một con số cùng một lúc. Một lần thử thất bại chỉ làm tăng số lần chơi. Giữa hai lần quan sát, bất kỳ số lượng người chơi nào cũng có thể đã thử cấp độ này, kể cả không có người chơi nào. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải xác định xem trình tự được ghi có thực sự xảy ra hay không. Nếu một chuỗi các nỗ lực thành công và không thành công nào đó tạo ra chính xác những quan sát đó theo thứ tự thời gian, chúng ta sẽ in`"YES"`. Nếu không chúng tôi in`"NO"`. 

Những hạn chế là rất nhỏ. Có tối đa 500 ca kiểm thử và mỗi ca kiểm thử chứa tối đa 100 quan sát. Ngay cả một thuật toán so sánh từng cặp quan sát cũng có thể chạy thoải mái. Không cần cấu trúc dữ liệu phức tạp hoặc tối ưu hóa. Quét tuyến tính qua các quan sát là quá đủ. 

Phần tinh tế nhất là hiểu được mối quan hệ giữa lượt chơi và lượt xóa. Một số tình huống tưởng chừng như vô hại thực ra lại không thể thực hiện được. 

Giả sử số lượt chơi không đổi trong khi số lần xóa tăng lên. 

đầu vào:```
1
3
0 0
1 1
1 2
```Câu trả lời đúng là:```
NO
```Một lần xóa mới luôn đến từ một lần chơi mới, vì vậy mức tăng số lần xóa không bao giờ có thể vượt quá mức tăng số lần chơi. Một giải pháp bất cẩn chỉ kiểm tra xem cả hai chuỗi có phải là không giảm hay không sẽ chấp nhận trường hợp này một cách sai lầm. 

Một tình huống khó khăn khác là khi tổng số lần xóa vượt quá tổng số lượt chơi. 

đầu vào:```
1
1
3 5
```Câu trả lời là:```
NO
```Năm lần thử thành công cần ít nhất năm lượt chơi. 

Một cạm bẫy nữa là giá trị giảm. 

đầu vào:```
1
3
10 2
15 3
14 3
```Câu trả lời là:```
NO
```Thống kê chỉ tăng theo thời gian. Bất kỳ sự sụt giảm nào ngay lập tức sẽ khiến kỷ lục không thể thực hiện được. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng tái tạo lại mọi nỗ lực riêng lẻ giữa các lần quan sát. Nếu số lượt chơi tăng thêm 1000, chúng ta có thể tưởng tượng tất cả các cách có thể chia 1000 lần thử đó thành các lần chạy thành công và không thành công. Phương pháp như vậy là đúng vì nó mô phỏng rõ ràng quá trình, nhưng số lượng lịch sử có thể tăng theo cấp số nhân với số lần thử. Ngay cả một khoảng thời gian với 1000 lượt chơi mới cũng đã có rất nhiều cơ hội. 

Quan sát quan trọng là chúng ta không bao giờ cần biết chính xác trình tự thành công và thất bại. Chỉ có ba điều kiện quan trọng. 

Đầu tiên, lượt chơi không thể giảm. 

Thứ hai, số lần thanh toán không thể giảm. 

Thứ ba, bất cứ khi nào chúng ta chuyển từ quan sát này sang quan sát tiếp theo, số lần xóa mới không thể vượt quá số lần phát mới. Mỗi lần thử thành công đều đóng góp một lần chơi và một lần xóa, do đó, để có thêm năm lần xóa cần có ít nhất năm lần chơi bổ sung. 

Ý tưởng tương tự cũng áp dụng cho quan sát đầu tiên. Tại bất kỳ thời điểm nào, tổng số lần xóa không thể vượt quá tổng số lượt chơi. 

Khi các điều kiện này được kiểm tra cho mỗi cặp liên tiếp, câu trả lời sẽ được xác định. Một cặp không hợp lệ sẽ khiến toàn bộ bản ghi không thể thực hiện được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các quan sát cho trường hợp thử nghiệm hiện tại. 
2. Kiểm tra quan sát đầu tiên. Nếu số lần xóa vượt quá số lần chơi, câu trả lời là`"NO"`bởi vì mọi sự rõ ràng đều phải đến từ một vở kịch. 
3. Duyệt các quan sát từ trái sang phải. 
4. Đối với mỗi cặp quan sát liên tiếp, hãy tính toán những thay đổi trong lượt chơi và lượt xóa. 
5. Nếu số lượt chơi giảm, việc ghi lại là không thể vì số liệu thống kê không bao giờ bị lùi lại. 
6. Nếu số lần xóa giảm, việc ghi lại không thể thực hiện được vì lý do tương tự. 
7. Nếu mức tăng số lượt chơi lớn hơn mức tăng số lượt chơi thì không thể lập kỷ lục. Số lần thử thành công không thể xảy ra nhiều hơn tổng số lần thử. 
8. Nếu không có bước kiểm tra nào thất bại, hãy in`"YES"`. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ cặp quan sát liên tiếp nào, thuật toán duy trì tính bất biến là tồn tại ít nhất một chuỗi các lần thử tạo ra tất cả các quan sát được thấy cho đến nay. 

Khi chuyển từ quan sát này sang quan sát tiếp theo, thông tin duy nhất quan trọng là có bao nhiêu lượt chơi mới và số lần xóa mới xuất hiện. Miễn là cả hai số lượng đều không âm và số lần xóa mới không vượt quá số lần chơi mới, chúng tôi có thể hiểu những lần xóa mới đó là những lần thử thành công và số lần xóa mới còn lại là những lần thử thất bại. Do đó, phần mở rộng hợp lệ luôn tồn tại. Nếu bất kỳ điều kiện nào trong số này bị vi phạm thì không có chuỗi nỗ lực nào có thể giải thích được quá trình chuyển đổi đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    records = [tuple(map(int, input().split())) for _ in range(n)]

    ok = True

    if records[0][1] > records[0][0]:
        ok = False

    for i in range(1, n):
        prev_p, prev_c = records[i - 1]
        cur_p, cur_c = records[i]

        dp = cur_p - prev_p
        dc = cur_c - prev_c

        if dp < 0 or dc < 0 or dc > dp:
            ok = False

    print("YES" if ok else "NO")
```Giải pháp bắt đầu bằng cách đọc tất cả các quan sát cho một trường hợp thử nghiệm. Giữ chúng trong một danh sách làm cho việc so sánh theo cặp trở nên đơn giản. 

Quan sát đầu tiên xứng đáng được đối xử đặc biệt. Nếu cấp độ đã có nhiều lần vượt qua hơn số lượt chơi, thì không thể ghi lại trước khi xem xét bất kỳ chuyển đổi nào. 

Đối với mỗi cặp liên tiếp, chúng tôi tính toán mức tăng lượt chơi và lượt xóa. Sự khác biệt tiêu cực có nghĩa là một số thống kê đã giảm đi, điều này không thể xảy ra. điều kiện`dc > dp`nắm bắt được trường hợp tinh tế hơn trong đó số lần chạy mới hoàn thành vượt quá số lần chạy mới bắt đầu. 

Thứ tự kiểm tra không quan trọng vì vi phạm sẽ đưa ra câu trả lời`"NO"`. Không cần phải dừng ngay sau khi phát hiện lỗi, mặc dù làm như vậy cũng có tác dụng. 

Vì tất cả các giá trị tối đa là 1000 nên việc tràn số nguyên không phải là vấn đề đáng lo ngại trong Python. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp mẫu đầu tiên:```
3
0 0
1 1
1 2
```| Bước | Trước (p,c) | Hiện tại (p,c) | dp | dc | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | (0,0) | - | - | Có | 
| 1 | (0,0) | (1,1) | 1 | 1 | Có | 
| 2 | (1,1) | (1,2) | 0 | 1 | Không | 

Ở lần chuyển đổi thứ hai, chúng tôi có thêm một lần rõ ràng mà không cần chơi thêm. Từ`dc > dp`, trình tự trở nên không thể và câu trả lời là`"NO"`. 

Hãy xem xét trường hợp mẫu```
2
1 0
1000 3
```| Bước | Trước (p,c) | Hiện tại (p,c) | dp | dc | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | (1,0) | - | - | Có | 
| 1 | (1,0) | (1000,3) | 999 | 3 | Có | 

Ba lần thử thành công trong số 999 lượt chơi mới là hoàn toàn có thể xảy ra. Mọi điều kiện đều đúng nên câu trả lời là`"YES"`. 

Những dấu vết này minh họa sự bất biến. Mỗi quá trình chuyển đổi được kiểm tra một cách độc lập và tính hợp lệ của tất cả các quá trình chuyển đổi hàm ý tính hợp lệ của toàn bộ lịch sử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi quan sát được xử lý một lần | 
| Không gian | O(1) | Chỉ cần một vài biến ngoài bộ lưu trữ đầu vào | 

Vì mỗi trường hợp kiểm thử chứa tối đa 100 quan sát và có tối đa 500 trường hợp kiểm thử nên tổng công việc là rất nhỏ. Giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        records = [tuple(map(int, input().split())) for _ in range(n)]

        ok = True

        if records[0][1] > records[0][0]:
            ok = False

        for i in range(1, n):
            prev_p, prev_c = records[i - 1]
            cur_p, cur_c = records[i]

            dp = cur_p - prev_p
            dc = cur_c - prev_c

            if dp < 0 or dc < 0 or dc > dp:
                ok = False

        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided samples
assert run(
"""6
3
0 0
1 1
1 2
2
1 0
1000 3
4
10 1
15 2
10 2
15 2
1
765 432
2
4 4
4 3
5
0 0
1 0
1 0
1 0
1 0
"""
) == """NO
YES
NO
YES
NO
YES"""

# minimum size
assert run(
"""1
1
0 0
"""
) == "YES"

# first observation invalid
assert run(
"""1
1
3 5
"""
) == "NO"

# all values equal
assert run(
"""1
4
2 1
2 1
2 1
2 1
"""
) == "YES"

# clears increase faster than plays
assert run(
"""1
2
5 2
7 5
"""
) == "NO"

# decreasing plays
assert run(
"""1
2
10 3
9 3
"""
) == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quan sát đơn lẻ (0,0) | CÓ | Kích thước tối thiểu | 
| Quan sát đơn lẻ (3,5) | KHÔNG | Xóa không thể vượt quá lượt chơi | 
| Những quan sát giống hệt nhau lặp đi lặp lại | CÓ | Không được phép thay đổi | 
| Chuyển từ (5,2) sang (7,5) | KHÔNG | Lượt xóa mới không thể vượt quá lượt chơi mới | 
| Chuyển từ (10,3) sang (9,3) | KHÔNG | Thống kê không thể giảm | 

## Vỏ cạnh 

Hãy xem xét trường hợp số lượt chơi tăng lên nhưng lượt chơi thì không. 

đầu vào:```
1
3
0 0
1 1
1 2
```Thuật toán tính toán sự khác biệt giữa hai quan sát cuối cùng:`dp = 0`,`dc = 1`. 

Từ`dc > dp`, quá trình chuyển đổi là không thể và đầu ra trở thành:```
NO
```Bây giờ hãy xem xét trường hợp quan sát đầu tiên đã vi phạm yêu cầu cơ bản. 

đầu vào:```
1
1
3 5
```Trước khi vào vòng lặp, thuật toán sẽ kiểm tra xem số lần xóa có vượt quá số lần phát hay không. Đây`5 > 3`, vì vậy nó ngay lập tức đánh dấu trường hợp thử nghiệm không hợp lệ và in:```
NO
```Cuối cùng, hãy xem xét giảm số liệu thống kê. 

đầu vào:```
1
3
10 2
15 3
14 3
```Đối với lần chuyển đổi cuối cùng:`dp = 14 - 15 = -1`Sự khác biệt âm có nghĩa là số lần phát bị lùi lại. Thuật toán từ chối chuỗi và in:```
NO
```Những ví dụ này bao gồm các tình huống thường gây ra việc triển khai không chính xác. Kiểm tra sự khác biệt theo cặp xử lý tất cả chúng một cách tự nhiên.

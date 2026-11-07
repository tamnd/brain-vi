---
title: "CF 131C - Thế giới là một nhà hát"
description: "Chúng tôi có một câu lạc bộ sân khấu với n nam và m nữ. Một nhóm biểu diễn phải có chính xác t người, với hai hạn chế bổ sung: ít nhất 4 người trong số họ phải là nam và ít nhất 1 người phải là nữ. Nhiệm vụ là đếm xem có thể tạo được bao nhiêu nhóm hợp lệ khác nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 131
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 95 (Div. 2)"
rating: 1400
weight: 131
solve_time_s: 95
verified: true
draft: false
---

[CF 131C - Thế giới là một nhà hát](https://codeforces.com/problemset/problem/131/C) 

**Đánh giá:** 1400 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một câu lạc bộ kịch với`n`chàng trai và`m`các cô gái. Một nhóm hiệu suất phải chứa chính xác`t`người, với hai hạn chế bổ sung: ít nhất 4 người trong số họ phải là con trai và ít nhất 1 người phải là con gái. 

Nhiệm vụ là đếm xem có thể tạo được bao nhiêu nhóm hợp lệ khác nhau. Hai nhóm được coi là khác nhau nếu có ít nhất một người được chọn khác nhau. 

Những hạn chế là rất nhỏ. Cả hai`n`Và`m`nhiều nhất là 30, do đó ngay cả những phép tính tổ hợp khá đắt tiền cũng an toàn. Câu trả lời cũng phù hợp với số nguyên 64 bit, có nghĩa là chúng ta có thể tính toán trực tiếp các kết hợp mà không cần số học mô-đun. 

Nhận xét quan trọng từ các ràng buộc là chúng ta không cần các kỹ thuật tối ưu hóa nâng cao. Số lượng bé trai có thể đếm được là rất nhỏ vì nhóm được chọn có quy mô`t`và nam phải đóng góp ít nhất 4 thành viên. Chúng ta có thể đơn giản thử mọi sự phân chia khả thi giữa con trai và con gái. 

Có một số trường hợp có thể dễ dàng đưa ra câu trả lời sai nếu giới hạn lặp lại không cẩn thận. 

Hãy xem xét đầu vào này:```
4 10 5
```Thành phần hợp lệ duy nhất là 4 nam và 1 nữ. Câu trả lời đúng là:```
40
```Một vòng lặp lỗi bắt đầu từ số 0 thay vì 4 sẽ tính nhiều đội không hợp lệ. 

Một trường hợp khó khăn khác là khi không có đủ cô gái cho một đợt phân chia cụ thể.```
10 1 8
```Thành phần hợp lệ duy nhất là 7 nam và 1 nữ. Chọn 6 chàng trai và 2 cô gái là điều không thể vì chỉ có một cô gái tồn tại. Việc triển khai bất cẩn tính toán một cách mù quáng các kết hợp cho tất cả các phần tách có thể thử các giá trị không hợp lệ, chẳng hạn như`C(1, 2)`. 

Một trường hợp tế nhị hơn xuất hiện khi`t`gần với`n + m`.```
4 1 5
```Có chính xác một nhóm hợp lệ: mọi người phải được chọn. 

Một vòng lặp có giới hạn trên không chính xác có thể vô tình bỏ qua trường hợp tất cả các bé trai đều được chọn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng ta có thể liệt kê từng tập hợp con của mọi người và kiểm tra xem kích thước của nó có chính xác không`t`, có chứa ít nhất 4 bé trai và có chứa ít nhất 1 bé gái hay không. 

Điều này hiệu quả vì mỗi nhóm hợp lệ tương ứng với chính xác một tập hợp con. Vấn đề là số lượng tập hợp con. Trong trường hợp xấu nhất có tổng cộng 60 người, có nghĩa là`2^60`tập hợp con. Đó là mức độ lớn về mặt thiên văn, vượt xa những gì có thể chạy trong 2 giây. 

Cấu trúc của vấn đề đưa ra hướng đi tốt hơn nhiều. Chúng tôi thực sự không quan tâm đến danh tính trong quá trình điều tra mà chỉ quan tâm đến việc chọn bao nhiêu bé trai và bé gái. 

Giả sử chúng ta quyết định rằng nhóm chứa chính xác`b`các chàng trai. Khi đó số bạn gái buộc phải là`t - b`. 

Đối với sự phân chia cố định này:```
ways = C(n, b) * C(m, t - b)
```bởi vì chúng tôi độc lập lựa chọn`b`chàng trai từ`n`Và`t - b`cô gái từ`m`. 

Nhiệm vụ duy nhất còn lại là lặp lại mọi giá trị hợp lệ của`b`. 

Cách tiếp cận bạo lực không thành công vì nó xử lý từng tập hợp con một cách riêng biệt. Phương pháp tổ hợp nhóm một số lượng lớn các tập hợp con lại với nhau theo thành phần. Thay vì kiểm tra các nhóm riêng lẻ, chúng tôi đếm tất cả các nhóm của một cấu trúc cùng một lúc. 

Vì có tối đa 30 giá trị có thể thử nên giải pháp này đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n+m)) | O(1) | Quá chậm | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, Và`t`. 
2. Lặp lại số lượng chàng trai được chọn, gọi nó là`boys`. 
3. Số lượng nam sinh hợp lệ tối thiểu là 4, vì câu lệnh yêu cầu ít nhất 4 nam. 
4. Số lượng nam sinh tối đa được phép là`min(n, t - 1)`. 

Chúng ta không thể chọn nhiều hơn`n`các chàng trai, và chúng ta phải chừa chỗ cho ít nhất một cô gái. 
5. Đối với mỗi hợp lệ`boys`giá trị, tính toán:```
girls = t - boys
```6. Đếm số cách chọn những người này:```
C(n, boys) * C(m, girls)
```7. Thêm giá trị này vào câu trả lời. 
8. In câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Mỗi đoàn hợp lệ có một số lượng chàng trai được chọn duy nhất. Khi chúng tôi lặp qua tất cả số lượng cậu bé khả thi, mọi nhóm hợp lệ sẽ xuất hiện trong đúng một lần lặp. 

Đối với sự phân chia cố định`(boys, girls)`, các lựa chọn là độc lập. Đầu tiên chúng tôi chọn những chàng trai tham gia, sau đó chọn những cô gái tham gia. Nhân hai số kết hợp sẽ cho ra số lượng đoàn chính xác có thành phần đó. 

Vì vòng lặp bao gồm tất cả các thành phần hợp lệ và không có thành phần nào trùng lặp với thành phần khác nên tổng cuối cùng sẽ tính mỗi nhóm hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
from math import comb

input = sys.stdin.readline

n, m, t = map(int, input().split())

answer = 0

for boys in range(4, min(n, t - 1) + 1):
    girls = t - boys
    
    if 1 <= girls <= m:
        answer += comb(n, boys) * comb(m, girls)

print(answer)
```Giải pháp trực tiếp tuân theo lý luận tổ hợp. 

Vòng lặp bắt đầu từ số 4 vì sẽ có ít nam sinh vi phạm quy tắc hơn. Giới hạn trên là`min(n, t - 1)`bởi vì chúng tôi không thể chọn nhiều bé trai hơn số lượng hiện có và chọn tất cả`t`những người là con trai sẽ không để lại con gái. 

Việc kiểm tra:```
if 1 <= girls <= m:
```bảo vệ chống lại số lượng cô gái không hợp lệ. Nếu không có điều kiện này, chúng ta có thể thực hiện những lựa chọn bất khả thi, chẳng hạn như chọn 5 cô gái khi chỉ có 2 cô gái. 

của Python`math.comb`tính toán các hệ số nhị thức chính xác xử lý phạm vi yêu cầu một cách hiệu quả và an toàn. Vì câu trả lời vừa với phạm vi số nguyên 64 bit nên tràn không phải là vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2 5
```Các phân chia hợp lệ có thể là: 

| Chàng Trai | Cô gái | C(5, Nam) | C(2, Cô gái) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 4 | 1 | 5 | 2 | 10 | 

Tổng số câu trả lời:```
10
```Ví dụ này cho thấy trường hợp không tầm thường đơn giản nhất. Chỉ được tổ chức một đội vì chọn 5 người có ít nhất 4 nam và ít nhất 1 nữ thì có đúng 4 nam và 1 nữ. 

### Ví dụ 2 

đầu vào:```
6 3 7
```Phân chia hợp lệ: 

| Chàng Trai | Cô gái | C(6, Nam) | C(3, Cô gái) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 4 | 3 | 15 | 1 | 15 | 
| 5 | 2 | 6 | 3 | 18 | 
| 6 | 1 | 1 | 3 | 3 | 

Câu trả lời cuối cùng:```
36
```Dấu vết này cho thấy nhiều tác phẩm hợp lệ đóng góp độc lập như thế nào. Mỗi hàng đếm các nhóm có phân bố nam-nữ cố định và tổng số là tổng của tất cả các phân phối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Chúng tôi lặp lại tất cả số lượng cậu bé khả thi một lần | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Giá trị tối đa của`t`nhiều nhất là 60, do đó vòng lặp chỉ thực hiện một số lần lặp rất nhỏ. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from math import comb

def solve():
    input = sys.stdin.readline
    
    n, m, t = map(int, input().split())
    
    ans = 0
    
    for boys in range(4, min(n, t - 1) + 1):
        girls = t - boys
        
        if 1 <= girls <= m:
            ans += comb(n, boys) * comb(m, girls)
    
    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    
    solve()
    
    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run("5 2 5\n") == "10", "sample 1"

# minimum valid configuration
assert run("4 1 5\n") == "1", "minimum sizes"

# only one possible girl count
assert run("10 1 8\n") == str(comb(10, 7)), "single girl available"

# multiple valid splits
assert run("6 3 7\n") == "36", "multiple compositions"

# larger boundary-style case
assert run("30 30 30\n") == str(
    sum(comb(30, b) * comb(30, 30 - b)
        for b in range(4, 30))
), "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 1 5`|`1`| Ranh giới chính xác nơi mọi người phải được chọn | 
|`10 1 8`|`120`| Những trường hợp chỉ có một cô gái có thể đếm được | 
|`6 3 7`|`36`| Nhiều tác phẩm hợp lệ | 
|`30 30 30`| Giá trị tính toán | Giá trị tổ hợp lớn gần giới hạn | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
4 10 5
```Thuật toán lặp lại số lượng cậu bé bắt đầu từ 4. Phép phân chia hợp lệ duy nhất là:```
boys = 4
girls = 1
```Việc tính toán trở thành:```
C(4,4) * C(10,1) = 1 * 10 = 10
```Không xem xét trường hợp không hợp lệ có ít hơn 4 bé trai. 

Bây giờ hãy kiểm tra:```
10 1 8
```Vòng lặp kiểm tra: 

| Chàng Trai | Cô gái | Có hiệu lực? | 
| --- | --- | --- | 
| 4 | 4 | Không | 
| 5 | 3 | Không | 
| 6 | 2 | Không | 
| 7 | 1 | Có | 

Chỉ có hàng cuối cùng thỏa mãn`girls <= m`. Câu trả lời trở thành:```
C(10,7) * C(1,1) = 120
```Điều này khẳng định tầm quan trọng của việc xác nhận số lượng cô gái trước khi thêm các kết hợp. 

Cuối cùng, hãy xem xét:```
4 1 5
```Phạm vi vòng lặp là:```
range(4, min(4, 4) + 1)
```trong đó bao gồm chính xác một giá trị:```
boys = 4
girls = 1
```Thuật toán đếm chính xác một nhóm có chứa tất cả mọi người.

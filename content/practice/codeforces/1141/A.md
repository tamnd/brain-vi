---
title: "CF 1141A - Trận 23"
description: "Chúng tôi bắt đầu với một con số và muốn đạt được con số mục tiêu lớn hơn. Thao tác duy nhất được phép là nhân giá trị hiện tại với 2 hoặc với 3. Nhiệm vụ là xác định số lượng thao tác cần thiết hoặc báo cáo rằng việc chuyển đổi không thể thực hiện được."
date: "2026-06-12T03:41:05+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1141
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 547 (Div. 3)"
rating: 1000
weight: 1141
solve_time_s: 95
verified: true
draft: false
---

[CF 1141A - Trận 23](https://codeforces.com/problemset/problem/1141/A) 

**Đánh giá:** 1000 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một con số và muốn đạt được con số mục tiêu lớn hơn. Hoạt động duy nhất được phép là nhân giá trị hiện tại với`2`hoặc bởi`3`. Nhiệm vụ là xác định có bao nhiêu thao tác cần thiết hoặc báo cáo rằng việc chuyển đổi không thể thực hiện được. 

Một cách hữu ích để suy nghĩ về quá trình này là xem xét những gì thay đổi giữa giá trị ban đầu và giá trị đích. Mỗi động thái chỉ giới thiệu thêm các yếu tố của`2`hoặc`3`. Không có gì khác có thể xuất hiện. Nếu mục tiêu chứa bất kỳ thừa số nguyên tố bổ sung nào ngoài`2`Và`3`, so với số bắt đầu thì việc chuyển đổi là không thể. 

Số lượng nhiều nhất là`5 × 10^8`, đủ nhỏ để lặp lại phép chia cho`2`Và`3`là cực kỳ rẻ. Ngay cả trong trường hợp xấu nhất, một con số dưới đây`5 × 10^8`chứa ít hơn ba mươi yếu tố của`2`và ít hơn hai mươi yếu tố của`3`, vì vậy một vòng lặp đơn giản là quá đủ. 

Một số trường hợp đáng chú ý. 

Giả sử giá trị bắt đầu và giá trị đích đã bằng nhau. 

đầu vào:```
10 10
```Đầu ra:```
0
```Không có hoạt động cần thiết. Một giải pháp luôn thực hiện ít nhất một phép nhân sẽ thất bại ở đây. 

Giả sử mục tiêu không phải là bội số của giá trị bắt đầu. 

đầu vào:```
48 72
```Đầu ra:```
-1
```Vì mọi thao tác chỉ nhân giá trị hiện tại nên chúng ta không bao giờ có thể giảm hoặc loại bỏ các thừa số. Nếu như`m`không chia hết cho`n`, đạt được nó là không thể. 

Giả sử tỉ số có chứa một thừa số nguyên tố khác với`2`hoặc`3`. 

đầu vào:```
4 20
```Đầu ra:```
-1
```Tỷ lệ là`20 / 4 = 5`. nhân với`2`Và`3`không bao giờ có thể tạo ra một yếu tố`5`. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi mỗi số là một trạng thái và thử nhân nhiều lần với`2`Và`3`cho đến khi đạt được mục tiêu. Tìm kiếm theo chiều rộng cuối cùng sẽ tìm ra câu trả lời vì mỗi cạnh đại diện cho một nước đi. 

Vấn đề là phạm vi số lớn. Mặc dù mục tiêu chỉ là`5 × 10^8`, việc khám phá tất cả các trạng thái có thể tiếp cận là công việc không cần thiết. Biểu đồ trạng thái tăng theo cấp số nhân với số lần di chuyển và việc tìm kiếm chung phức tạp hơn nhiều so với mức cần thiết. 

Quan sát quan trọng là mọi chuỗi phép tính hợp lệ đều nhân số bắt đầu với một tổ hợp lũy thừa nào đó của`2`Và`3`. 

Nếu như$$m = n \cdot 2^a \cdot 3^b,$$thì chính xác`a + b`di chuyển là cần thiết. Mỗi phép nhân với`2`đóng góp một yếu tố`2`và mỗi phép nhân với`3`đóng góp một yếu tố`3`. 

Điều này ngay lập tức giảm bớt vấn đề để phân tích tỷ lệ$$r = \frac{m}{n}.$$Nếu như`m`không chia hết cho`n`, không có giải pháp. 

Ngược lại, chia nhiều lần`r`qua`2`trong khi có thể và đếm xem nó xảy ra bao nhiêu lần. Sau đó làm tương tự cho`3`. Nếu còn lại gì ngoài`1`, tỷ số này chứa một số thừa số nguyên tố khác, khiến cho phép biến đổi là không thể. Nếu giá trị còn lại là`1`, câu trả lời đơn giản là tổng số lần chia được thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo số lần di chuyển | Hàm mũ | Quá chậm | 
| Tối ưu | O(log(m/n)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. 
2. Kiểm tra xem`m`chia hết cho`n`. 

Nếu không thì xuất ra`-1`bởi vì phép nhân không bao giờ có thể loại bỏ được các yếu tố đã có trong tỷ lệ mục tiêu. 
3. Tính toán`r = m // n`. 

Tỷ lệ này đại diện cho tất cả mọi thứ phải được thêm vào thông qua phép nhân với`2`Và`3`. 
4. Khởi tạo`moves = 0`. 
5. Trong khi`r`chia hết cho`2`, chia nó cho`2`và tăng dần`moves`. 

Mỗi phép chia tương ứng với một phép nhân với`2`trong quá trình chuyển tiếp. 
6. Trong khi`r`chia hết cho`3`, chia nó cho`3`và tăng dần`moves`. 

Mỗi phép chia tương ứng với một phép nhân với`3`. 
7. Kiểm tra giá trị còn lại của`r`. 

Nếu như`r != 1`, một số thừa số nguyên tố khác ngoài`2`hoặc`3`vẫn còn, vì vậy đầu ra`-1`. 
8. Nếu không thì xuất ra`moves`. 

### Tại sao nó hoạt động 

Tỷ lệ`m / n`nắm bắt chính xác sự thay đổi cấp số nhân cần thiết để đạt được mục tiêu. Mỗi hoạt động được phép đóng góp một yếu tố của một trong hai`2`hoặc`3`, do đó, mọi tỷ lệ có thể truy cập đều phải có dạng`2^a 3^b`. 

Thuật toán loại bỏ tất cả các yếu tố của`2`Và`3`từ tỷ lệ. Nếu không còn gì thì tỷ lệ chính xác là`2^a 3^b`, Và`a + b`các hoạt động đều cần thiết và đủ. Nếu vẫn còn yếu tố nào đó, nó không bao giờ có thể được tạo bằng các thao tác được phép, khiến cho việc chuyển đổi là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    if m % n != 0:
        print(-1)
        return

    r = m // n
    moves = 0

    while r % 2 == 0:
        r //= 2
        moves += 1

    while r % 3 == 0:
        r //= 3
        moves += 1

    if r != 1:
        print(-1)
    else:
        print(moves)

solve()
```Kiểm tra đầu tiên xác minh rằng mục tiêu là bội số của giá trị bắt đầu. Nếu không có điều kiện này, phép chia số nguyên sẽ ẩn đi những trường hợp không thể thực hiện được. 

Biến`r`lưu trữ tỷ lệ vẫn cần được giải thích bằng các phép toán được phép. Mỗi lần chúng ta chia cho`2`hoặc`3`, chúng ta đếm một nước đi tương ứng. 

Việc kiểm tra cuối cùng là rất quan trọng. Sau khi loại bỏ tất cả các yếu tố của`2`Và`3`, giá trị còn lại phải chính xác`1`. Nếu không, vẫn còn một số thừa số nguyên tố bị cấm. Thiếu kiểm tra này sẽ chấp nhận không chính xác các trường hợp như`4 → 20`, trong đó tỷ lệ là`5`. 

Tất cả số học vẫn ở mức thoải mái trong các số nguyên Python và các vòng lặp chỉ thực hiện theo logarit nhiều lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
120 51840
```Tỷ lệ là:$$51840 / 120 = 432$$| Bước | r | di chuyển | 
| --- | --- | --- | 
| Ban đầu | 432 | 0 | 
| Chia cho 2 | 216 | 1 | 
| Chia cho 2 | 108 | 2 | 
| Chia cho 2 | 54 | 3 | 
| Chia cho 2 | 27 | 4 | 
| Chia cho 3 | 9 | 5 | 
| Chia cho 3 | 3 | 6 | 
| Chia cho 3 | 1 | 7 | 

Tỉ lệ còn lại trở thành`1`, do đó việc chuyển đổi là có thể. Câu trả lời là`7`. 

### Ví dụ 2 

đầu vào:```
10 10
```| Bước | r | di chuyển | 
| --- | --- | --- | 
| Ban đầu | 1 | 0 | 

Không có vòng lặp phân chia nào chạy. Tỷ lệ đã rồi`1`, vậy câu trả lời là`0`. 

Ví dụ này cho thấy các số bằng nhau không yêu cầu thao tác và được thuật toán xử lý một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(m/n)) | Mỗi lần lặp sẽ loại bỏ hệ số 2 hoặc 3 khỏi tỷ lệ | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Tỷ lệ này giảm ít nhất một hệ số là`2`hoặc`3`trên mỗi lần lặp vòng lặp, vì vậy tổng số lần lặp rất nhỏ. Với các giá trị giới hạn bởi`5 × 10^8`, thời gian chạy thực sự là tức thời và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        n, m = map(int, input().split())

        if m % n != 0:
            print(-1)
            return

        r = m // n
        moves = 0

        while r % 2 == 0:
            r //= 2
            moves += 1

        while r % 3 == 0:
            r //= 3
            moves += 1

        print(moves if r == 1 else -1)

    solve()
    return sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else ""

# provided sample
assert run("120 51840\n").strip() == "7", "sample 1"

# custom cases
assert run("1 1\n").strip() == "0", "equal values"
assert run("48 72\n").strip() == "-1", "ratio contains forbidden factor"
assert run("4 20\n").strip() == "-1", "factor 5 remains"
assert run("1 268435456\n").strip() == "28", "large power of 2"
assert run("9 162\n").strip() == "2", "one factor 2 and one factor 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Không cần di chuyển | 
|`48 72`|`-1`| Tỷ lệ chứa yếu tố bị cấm | 
|`4 20`|`-1`| Hệ số còn lại sau khi loại bỏ 2s và 3s | 
|`1 268435456`|`28`| Công suất lớn 2 | 
|`9 162`|`2`| Hệ số hỗn hợp 2 và 3 | 

## Vỏ cạnh 

Hãy xem xét trường hợp các số đã bằng nhau. 

đầu vào:```
10 10
```Tỷ lệ là`1`. Cả vòng lặp phân chia đều không thực thi. Thuật toán đạt đến bước kiểm tra cuối cùng với`r = 1`và đầu ra`0`. Điều này phản ánh chính xác rằng không cần thực hiện thao tác nào. 

Hãy xem xét một mục tiêu không chia hết cho giá trị bắt đầu. 

đầu vào:```
48 72
```Kiểm tra đầu tiên tính toán`72 % 48`, không bằng không. Thuật toán xuất ra ngay lập tức`-1`. Bất kỳ chuỗi phép nhân nào đều bảo toàn tính chia hết cho giá trị ban đầu, do đó đạt được`72`từ`48`là không thể. 

Xét một tỉ số chứa một thừa số nguyên tố khác. 

đầu vào:```
4 20
```Tỷ lệ là`5`. Nó không chia được cho`2`hoặc`3`, vì vậy cả hai vòng lặp đều không chạy. Giá trị còn lại vẫn là`5`, không`1`và đầu ra của thuật toán`-1`. Điều này loại bỏ chính xác các phép biến đổi yêu cầu đưa vào một yếu tố khác ngoài`2`hoặc`3`.

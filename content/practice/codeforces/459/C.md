---
title: "CF 459C - Pashmak và xe buýt"
description: "Chúng ta cần phân công mỗi học sinh vào một trong k xe buýt vào mỗi d ngày. Đối với mỗi học sinh, chúng ta có thể coi bài tập của họ là một chuỗi có độ dài d. Ví dụ: nếu d = 3, một học sinh có thể đi xe buýt (2, 1, 2) trong ba ngày."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 459
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 261 (Div. 2)"
rating: 1900
weight: 459
solve_time_s: 119
verified: false
draft: false
---

[CF 459C - Pashmak và xe buýt](https://codeforces.com/problemset/problem/459/C) 

**Xếp hạng:** 1900 
**Tags:** tổ hợp, thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần chỉ định mỗi học sinh vào một trong`k`xe buýt trên mỗi`d`ngày. 

Đối với mỗi học sinh, chúng ta có thể coi bài tập của họ như một chuỗi độ dài`d`. Ví dụ, nếu`d = 3`, một học sinh có thể đi xe buýt`(2, 1, 2)`trong ba ngày. 

Hai học sinh trở thành bạn thân nếu toàn bộ trình tự của họ giống hệt nhau. Họ được phép đi chung xe buýt vào một số ngày, nhưng nếu họ dùng chung một xe buýt vào mỗi ngày thì trình tự của họ khớp nhau và sự sắp xếp không hợp lệ. 

Nhiệm vụ là xây dựng các bài tập cho`n`học sinh để mỗi học sinh có một trình tự duy nhất. Nếu điều đó là không thể, chúng ta phải in`-1`. 

Các ràng buộc ngay lập tức gợi ý rằng chúng ta không giải quyết được vấn đề tìm kiếm. Cả hai`n`Và`d`có thể đạt tới 1000, trong khi`k`có thể lớn như`10^9`. Việc tìm kiếm một cách thô bạo thông qua các bài tập sẽ là vô vọng vì số lượng lịch trình có thể tăng theo cấp số nhân. Vì bản thân đầu ra chứa`n × d`số, bất kỳ giải pháp được chấp nhận nào cũng có thể sẽ dành phần lớn thời gian để tạo ra câu trả lời trực tiếp. 

Câu hỏi trọng tâm là liệu chúng ta có thể cống hiến tất cả`n`học sinh có trình tự độ dài khác nhau`d`, trong đó mỗi vị trí có thể đảm nhận một trong`k`các giá trị. 

Có chính xác`k^d`trình tự có độ dài khác nhau`d`trên một bảng chữ cái có kích thước`k`. Nếu như`n > k^d`, thì ngay cả trong sự sắp xếp tốt nhất có thể cũng không có đủ các dãy khác nhau, vì vậy một số cặp học sinh phải nhận được dãy giống nhau. Theo nguyên lý chuồng bồ câu, câu trả lời là không thể. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
3 1 2
```Chỉ có một chiếc xe buýt. Mỗi học sinh phải đi xe buýt số 1 mỗi ngày. Tất cả học sinh đều nhận được trình tự giống nhau`(1,1)`, vậy đáp án đúng là:```
-1
```Việc xây dựng bất cẩn mà bỏ qua yêu cầu về tính duy nhất sẽ đưa ra các phép gán không chính xác. 

Coi như:```
1 1 1
```Chỉ có một học sinh. Tính duy nhất là tự động vì không có cặp học sinh nào. Đầu ra đúng là:```
1
```Một số triển khai từ chối không chính xác tất cả các trường hợp có`k = 1`. 

Một trường hợp tế nhị khác là:```
8 2 3
```Từ`2^3 = 8`, có chính xác đủ các dãy phân biệt. Một giải pháp đúng phải sử dụng tất cả các trình tự có thể. Việc triển khai có một lỗi nhỏ trong hệ thống đánh số của nó có thể chỉ tạo ra bảy nhiệm vụ riêng biệt và báo cáo lỗi không chính xác. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề này là gán một dãy cho mỗi học sinh và sau đó kiểm tra xem tất cả các dãy có khác biệt hay không. 

Cách tiếp cận bạo lực sẽ tạo ra các bài tập hàng ngày, so sánh từng cặp học sinh và tiếp tục sửa đổi lịch trình cho đến khi mọi xung đột biến mất. Chiến lược như vậy nhanh chóng trở nên không khả thi vì không gian tìm kiếm chứa`k^(n·d)`lịch trình có thể. Ngay cả đối với những đầu vào rất nhỏ, việc thăm dò toàn diện là không thể. 

Quan sát quan trọng là học sinh chỉ cần trình tự riêng biệt. 

Lịch trình của mỗi học sinh là một thời gian dài`d`chuỗi trên một bảng chữ cái có kích thước`k`. Số lượng lịch trình có thể chính xác là`k^d`. Điều này biến bài toán thành bài toán đánh số: gán một từ mã khác nhau cho mỗi học sinh. 

Cách tự nhiên để tạo ra các từ mã riêng biệt là viết các chỉ số sinh viên theo dạng cơ số`k`. 

Giả sử chúng ta đánh số học sinh từ`0`ĐẾN`n-1`. 

Học sinh`0`tương ứng với:```
000...
```Học sinh`1`tương ứng với:```
001...
```Học sinh`2`tương ứng với:```
002...
```v.v., sử dụng chính xác`d`căn cứ-`k`chữ số. 

Nếu như`n ≤ k^d`, thì tất cả các chỉ số sinh viên phù hợp với`d`căn cứ-`k`chữ số. Các số riêng biệt có cách biểu diễn chữ số riêng biệt, vì vậy mỗi học sinh sẽ tự động nhận được một lịch trình duy nhất. 

Số xe buýt trong báo cáo được đánh số từ`1`ĐẾN`k`, trong khi cơ sở-`k`chữ số nằm trong khoảng từ`0`ĐẾN`k-1`. Chúng tôi chỉ cần thêm một khi in. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n·d) | O(n·d) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`k`, Và`d`. 
2. Xác định xem ít nhất`n`lịch trình riêng biệt tồn tại. 

Tính công suất của`k`lên đến chiều dài`d`, dừng sớm khi giá trị đạt hoặc vượt quá`n`. 
3. Nếu sau`d`phép nhân giá trị vẫn nhỏ hơn`n`, in`-1`. 

Điều này có nghĩa`k^d < n`, do đó không có đủ trình tự riêng biệt cho tất cả học sinh. 
4. Tạo một`d × n`ma trận trả lời. 

Hàng ngang`i`sẽ đại diện cho các nhiệm vụ trong ngày`i`. 
5. Đối với mỗi chỉ số sinh viên`s`từ`0`ĐẾN`n-1`, viết`s`trong căn cứ`k`. 

Trích xuất chính xác`d`chữ số. Thiếu các chữ số cao hơn được coi là bằng 0. 
6. Lưu trữ các chữ số được trích xuất vào ma trận đáp án. 

các`i`-chữ số thứ xác định xe buýt được sử dụng trong ngày`i`. 
7. Sau khi xử lý tất cả học sinh, hãy thêm một vào mỗi chữ số được lưu trữ khi in. 

Điều này chuyển đổi chữ số`0..k-1`vào số xe buýt`1..k`. 

### Tại sao nó hoạt động 

Mỗi học sinh được giao cơ sở`k`biểu diễn một số nguyên phân biệt giữa`0`Và`n-1`. 

Bởi vì căn cứ-`k`cách biểu diễn là duy nhất, không có hai học sinh nào nhận được dãy chữ số giống nhau. Do đó, đối với bất kỳ cặp học sinh nào, sẽ có ít nhất một ngày mà các chuyến xe được chỉ định của họ khác nhau. 

Việc xây dựng sử dụng chính xác`d`chữ số. Những biểu diễn như vậy tồn tại cho tất cả các chỉ số sinh viên một cách chính xác khi`n ≤ k^d`. Nếu như`n > k^d`, có ít dãy khả dĩ hơn sinh viên, vì thế không thể tồn tại một sự sắp xếp hợp lệ. Như vậy thuật toán vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, d = map(int, input().split())

    cnt = 1
    for _ in range(d):
        if cnt >= n:
            break
        cnt *= k

    if cnt < n:
        print(-1)
        return

    ans = [[0] * n for _ in range(d)]

    for student in range(n):
        x = student
        for day in range(d):
            ans[day][student] = x % k
            x //= k

    out = []
    for day in range(d):
        out.append(" ".join(str(ans[day][student] + 1) for student in range(n)))

    sys.stdout.write("\n".join(out))

solve()
```Phần đầu tiên kiểm tra xem có tồn tại đủ lịch trình riêng biệt hay không. Chúng ta không bao giờ cần giá trị chính xác của`k^d`một khi nó vượt quá`n`, do đó phép nhân có thể dừng sớm. 

Ma trận được sắp xếp theo ngày thay vì theo học sinh vì định dạng đầu ra bắt buộc in một ngày trên mỗi dòng. Điều này tránh được thao tác chuyển vị sau này. 

Đối với mỗi học sinh, chúng tôi liên tục lấy`x % k`và chia cho`k`. Điều này chiết xuất cơ sở-`k`các chữ số từ ít quan trọng nhất đến quan trọng nhất. Vì chỉ số của mỗi học sinh là khác nhau nên vectơ chữ số của chúng cũng khác nhau. 

Một lỗi phổ biến là quên số xe buýt bắt đầu lúc`1`. Biểu diễn bên trong sử dụng chữ số`0..k-1`, vì vậy chúng tôi chỉ thêm một khi in. 

Một lỗi thường gặp khác là cố gắng tính toán`k^d`trực tiếp. Mặc dù Python có thể xử lý các số nguyên lớn nhưng điều đó là không cần thiết. Dừng sớm giúp việc kiểm tra đơn giản và hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 2
```Học sinh được đánh số`0,1,2`. 

| Sinh viên | Biểu diễn cơ số 2 (2 chữ số) | 
| --- | --- | 
| 0 | 00 | 
| 1 | 01 | 
| 2 | 10 | 

Các chữ số được lưu trữ theo ngày trở thành: 

| Ngày | Sinh viên 0 | Sinh viên 1 | Sinh viên 2 | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | 
| 2 | 0 | 0 | 1 | 

Sau khi thêm một: 

| Ngày | Sinh viên 0 | Sinh viên 1 | Sinh viên 2 | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 
| 2 | 1 | 1 | 2 | 

Đầu ra:```
1 2 1
1 1 2
```Điều này khác với kết quả đầu ra mẫu nhưng có giá trị như nhau vì tất cả học sinh đều nhận được lịch trình riêng biệt. 

### Ví dụ 2 

đầu vào:```
8 2 3
```Từ`2^3 = 8`, mọi chuỗi nhị phân có thể có độ dài bằng ba đều cần thiết. 

| Sinh viên | Chữ số | 
| --- | --- | 
| 0 | 000 | 
| 1 | 001 | 
| 2 | 010 | 
| 3 | 011 | 
| 4 | 100 | 
| 5 | 101 | 
| 6 | 110 | 
| 7 | 111 | 

Mỗi chuỗi xuất hiện đúng một lần. Không có hai học sinh nào chia sẻ cả ba bài tập trên xe buýt. 

Ví dụ này minh họa trường hợp ranh giới trong đó`n = k^d`và mọi từ mã có sẵn đều phải được sử dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·d) | Mỗi học sinh đóng góp chính xác`d`căn cứ-`k`chữ số | 
| Không gian | O(n·d) | Lưu trữ ma trận đầu ra`d × n`giá trị | 

Đầu ra lớn nhất có thể đã chứa một triệu số vì cả hai`n`Và`d`có thể là 1000. Một`O(n·d)`xây dựng phù hợp với kích thước đầu ra và thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, k, d = map(int, input().split())

    cnt = 1
    for _ in range(d):
        if cnt >= n:
            break
        cnt *= k

    if cnt < n:
        return "-1"

    ans = [[0] * n for _ in range(d)]

    for student in range(n):
        x = student
        for day in range(d):
            ans[day][student] = x % k
            x //= k

    return "\n".join(
        " ".join(str(ans[day][student] + 1) for student in range(n))
        for day in range(d)
    )

# minimum case
assert run("1 1 1\n") == "1"

# impossible case
assert run("3 1 2\n") == "-1"

# boundary n = k^d
out = run("8 2 3\n")
assert out != "-1"

# sample-sized case
out = run("3 2 2\n")
assert out != "-1"

# another impossible boundary
assert run("9 2 3\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Trường hợp hợp lệ nhỏ nhất | 
|`3 1 2`|`-1`| Chỉ có một chuỗi có sẵn | 
|`8 2 3`| Bất kỳ lịch trình hợp lệ nào | Giới hạn công suất chính xác`n = k^d`| 
|`3 2 2`| Bất kỳ lịch trình hợp lệ nào | Xây dựng cơ bản | 
|`9 2 3`|`-1`| Giá trị không thể đầu tiên trên công suất | 

## Vỏ cạnh 

Hãy xem xét:```
3 1 2
```Thuật toán tính toán:```
1^2 = 1
```Từ`1 < 3`, nó in ngay:```
-1
```Chỉ có một trình tự có thể`(1,1)`, vì vậy ba học sinh khác nhau không thể được xếp lịch học duy nhất. 

Coi như:```
1 1 1
```Việc kiểm tra dung lượng thành công vì`1^1 = 1`. Học sinh`0`nhận được chữ số`0`, trở thành xe buýt`1`sau khi thêm một:```
1
```Trường hợp một học sinh là hợp lệ vì không cần có tính duy nhất giữa nhiều học sinh. 

Coi như:```
8 2 3
```Việc kiểm tra năng lực đạt chính xác`8`. Chỉ số sinh viên`0`bởi vì`7`tạo ra tất cả các chuỗi nhị phân có độ dài ba. Vì các biểu diễn cơ sở 2 là duy nhất nên mỗi học sinh sẽ có một lịch trình khác nhau và việc xây dựng thành công ở ranh giới chặt chẽ. 

Coi như:```
9 2 3
```Thuật toán tính toán:```
2^3 = 8
```Từ`8 < 9`, nó xuất ra:```
-1
```Nguyên tắc chuồng bồ câu đảm bảo không thể thực hiện được vì chỉ có tám lịch trình riêng biệt tồn tại trong khi chín học sinh yêu cầu những lịch trình duy nhất.

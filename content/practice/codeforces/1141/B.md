---
title: "CF 1141B - Nghỉ ngơi liên tục tối đa"
description: "Chúng ta được cung cấp một chuỗi nhị phân biểu thị một ngày, trong đó mỗi vị trí tương ứng với một giờ. Giá trị 1 có nghĩa là Polycarp đang nghỉ ngơi trong giờ đó, trong khi giá trị 0 có nghĩa là anh ấy đang làm việc."
date: "2026-06-12T03:40:43+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1141
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 547 (Div. 3)"
rating: 900
weight: 1141
solve_time_s: 72
verified: true
draft: false
---

[CF 1141B - Nghỉ ngơi liên tục tối đa](https://codeforces.com/problemset/problem/1141/B) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân biểu thị một ngày, trong đó mỗi vị trí tương ứng với một giờ. Một giá trị của`1`có nghĩa là Polycarp đang nghỉ ngơi trong giờ đó, trong khi`0`nghĩa là anh ấy đang làm việc. Trình tự tương tự lặp đi lặp lại mãi mãi, vì vậy sau giờ cuối cùng của một ngày, giờ tiếp theo lại là giờ đầu tiên của cùng một lịch trình. 

Nhiệm vụ là xác định khoảng thời gian nghỉ ngơi không bị gián đoạn dài nhất nếu chúng ta được phép di chuyển qua ranh giới giữa cuối ngày và đầu ngày tiếp theo. 

Một điểm quan trọng là trình tự là vòng tròn. Bất kỳ phân đoạn nào bắt đầu ở gần cuối mảng đều có thể tiếp tục ở đầu mảng, như thể mảng đó được kết nối từ đầu đến cuối trong một vòng lặp. 

Kích thước đầu vào có thể lớn tới 200.000 giờ. Điều đó loại trừ bất kỳ giải pháp nào cố gắng kiểm tra mọi vị trí bắt đầu có thể và mở rộng ra bên ngoài, vì điều đó sẽ dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Quét tuyến tính là mục tiêu thực tế duy nhất. 

Một sai lầm ngây thơ xảy ra khi người ta coi mảng là tuyến tính và bỏ qua phần bao quanh. Ví dụ, trong`1 1 0 1 1 1`, câu trả lời đúng là 4, vì việc chạy`1 1`cuối cùng kết nối với`1 1 1`lúc đầu. Quét tuyến tính sẽ báo cáo sai 3. 

Một trường hợp cạnh phổ biến khác là khi mảng hoàn toàn bằng 0 ngoại trừ một số duy nhất. Câu trả lời đúng là 0 vì không có giờ nghỉ ngơi và chiến lược hợp nhất vòng tròn ngây thơ có thể coi các khoảng trống là thời gian nghỉ ngơi một cách không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử mỗi giờ bắt đầu và kéo dài về phía trước trong khi gặp phải`1`s, bao quanh bằng cách sử dụng số học modulo. Với mỗi vị trí xuất phát, chúng ta tiến tới`n`các bước. Điều này tạo ra một`O(n^2)`thuật toán trong trường hợp xấu nhất, vì mỗi điểm bắt đầu có thể đi qua gần như toàn bộ mảng. Với`n = 2 * 10^5`, tốc độ này quá chậm. 

Cấu trúc của bài toán gợi ý một mảng hình tròn và mục tiêu là tìm khối liền kề dài nhất của`1`s trên một vòng tròn. Một thủ thuật tiêu chuẩn cho các chuỗi hình tròn là sao chép mảng, biến các phân đoạn bao quanh thành các phân đoạn liền kề thông thường. 

Nếu chúng ta xây dựng`a + a`, bất kỳ phân đoạn nào vượt qua ranh giới trong mảng ban đầu sẽ trở thành phân đoạn bình thường trong mảng nhân đôi. Lưu ý duy nhất là các phân đoạn hợp lệ không thể vượt quá độ dài`n`, bởi vì chúng ta vẫn đang biểu diễn một vòng quay đầy đủ trong ngày. 

Vì vậy bài toán quy về việc tìm số lớn nhất của các số liên tiếp`1`s trong mảng nhân đôi, đồng thời đảm bảo chúng tôi không đếm nhiều hơn`n`các phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu (quét nhân đôi/quét tuyến tính) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng độ dài`n`. Chúng ta sẽ coi nó là hình tròn, nghĩa là chỉ số`n - 1`kết nối trở lại chỉ mục`0`. 
2. Tạo một mảng mới`b`đó là`a`nối với chính nó. Điều này biến đổi vùng lân cận hình tròn thành vùng kề thông thường, do đó các chuỗi bao quanh trở thành các đoạn thẳng. 
3. Khởi tạo hai biến:`best = 0`để lưu trữ chuỗi tối đa của những cái đó, và`current = 0`để theo dõi độ dài vệt hiện tại trong khi quét. 
4. Lặp lại từng phần tử trong`b`từ trái sang phải. 

Nếu phần tử hiện tại là`1`, tăng`current`bởi một vì vệt vẫn tiếp tục. Nếu nó là`0`, cài lại`current`về 0 vì vệt bị đứt. 
5. Sau khi cập nhật`current`, cập nhật`best`BẰNG`max(best, current)`. 
6. Nếu`current`vượt quá`n`, kẹp nó lại`n`. Điều này ngăn cản việc đếm một phân đoạn kéo dài nhiều hơn một chu kỳ đầy đủ của ngày ban đầu. 
7. Tiếp tục cho đến hết`b`. Câu trả lời cuối cùng là`best`. 

### Tại sao nó hoạt động 

Mỗi đoạn tròn hợp lệ của các đoạn liên tiếp trong mảng ban đầu tương ứng chính xác với một số đoạn liền kề trong mảng nhân đôi. Bằng cách quét mảng nhân đôi, chúng tôi liệt kê tất cả các phân đoạn bao quanh có thể có. Hạn chế duy nhất là chúng tôi không thể sử dụng nhiều hơn`n`các phần tử liên tiếp nhau, vì điều đó có nghĩa là phải trải qua nhiều hơn một chu kỳ trọn vẹn trong ngày. Việc duy trì số lần chạy đơn giản đảm bảo mọi khối tối đa đều được ghi lại và việc đặt lại về số 0 đảm bảo tính chính xác của phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
a = list(map(int, input().split()))

b = a + a

best = 0
current = 0

for i in range(len(b)):
    if b[i] == 1:
        current += 1
        if current > n:
            current = n
    else:
        current = 0
    best = max(best, current)

print(best)
```Việc triển khai trực tiếp theo ý tưởng chuyển đổi mảng tròn thành mảng tuyến tính. Bước nhân đôi đảm bảo các phân đoạn bao quanh được xử lý một cách tự nhiên. 

Sự tinh tế duy nhất là nắp ở`n`, điều này ngăn chặn việc phóng đại quá trình chạy khi mảng nhân đôi cho phép các vệt dài giả tạo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 0 1 0 1
```Chúng tôi xây dựng`b = [1,0,1,0,1,1,0,1,0,1]`. 

| tôi | b[i] | hiện tại | tốt nhất | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 
| 1 | 0 | 0 | 1 | 
| 2 | 1 | 1 | 1 | 
| 3 | 0 | 0 | 1 | 
| 4 | 1 | 1 | 1 | 
| 5 | 1 | 2 | 2 | 
| 6 | 0 | 0 | 2 | 
| 7 | 1 | 1 | 2 | 
| 8 | 0 | 0 | 2 | 
| 9 | 1 | 1 | 2 | 

Phân đoạn tốt nhất là`2`, đến từ ranh giới giữa cuối và đầu ngày. 

### Ví dụ 2 

đầu vào:```
6
1 1 0 1 1 1
```Chúng tôi xây dựng`b = [1,1,0,1,1,1,1,1,0,1,1,1]`. 

| tôi | b[i] | hiện tại | tốt nhất | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 
| 1 | 1 | 2 | 2 | 
| 2 | 0 | 0 | 2 | 
| 3 | 1 | 1 | 2 | 
| 4 | 1 | 2 | 2 | 
| 5 | 1 | 3 | 3 | 
| 6 | 1 | 4 | 4 | 
| 7 | 1 | 5 | 5 | 
| 8 | 0 | 0 | 5 | 
| 9 | 1 | 1 | 5 | 
| 10 | 1 | 2 | 5 | 
| 11 | 1 | 3 | 5 | 

Phân đoạn bao quanh tối đa là`5`, được hình thành bằng cách lấy phần cuối của ngày và tiếp tục bắt đầu. 

Những dấu vết này cho thấy cách sao chép biến cấu trúc hình tròn thành một bản quét tuyến tính đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi duyệt qua một danh sách kích thước`2n`một lần, với O(1) hoạt động trên mỗi phần tử | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng trùng lặp | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì thậm chí`4 * 10^5`các hoạt động trong Python là không đáng kể và việc sử dụng bộ nhớ là tuyến tính theo kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    n = int(input().strip())
    a = list(map(int, input().split()))
    b = a + a

    best = 0
    cur = 0

    for x in b:
        if x == 1:
            cur += 1
            if cur > n:
                cur = n
        else:
            cur = 0
        best = max(best, cur)

    print(best)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from io import StringIO
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_stdout
    sys.stdin = old_stdin
    return out.getvalue().strip()

# provided samples
assert run("5\n1 0 1 0 1\n") == "2", "sample 1"
assert run("4\n1 1 1 1\n") == "4", "sample 2 (all rest)".__class__

# custom cases
assert run("1\n0\n") == "0", "single working hour"
assert run("1\n1\n") == "1", "single rest hour"
assert run("5\n0 0 0 0 0\n") == "0", "no rest at all"
assert run("6\n1 1 0 1 1 1\n") == "5", "wrap-around maximum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 0 | 0 | không có giờ nghỉ ngơi | 
| đơn 1 | 1 | nghỉ ngơi hợp lệ tối thiểu | 
| tất cả số không | 0 | không có vệt nào tồn tại | 
| trường hợp bọc hỗn hợp | 5 | sự đúng đắn vượt qua ranh giới | 

## Vỏ cạnh 

Xem xét đầu vào`6 1 1 0 1 1 1`. Đoạn tối ưu bao bọc từ đầu đến cuối, tạo thành một chuỗi năm đoạn. 

Trong mảng nhân đôi, chúng ta thấy`1 1 0 1 1 1 1 1 0 1 1 1`. Quá trình quét bắt đầu một chuỗi ở chỉ số 0 và đạt độ dài 2, ngắt ở 2, khởi động lại ở 3 và tăng dần qua ranh giới để đạt 5 ở chỉ số 7. Tại thời điểm đó,`best`trở thành 5 và không thay đổi. 

Điều này xác nhận rằng các trường hợp bao quanh được nắm bắt một cách tự nhiên mà không cần số học mô-đun rõ ràng.

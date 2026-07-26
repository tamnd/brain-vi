---
title: "CF 102811A - \u0410\u0432\u0442\u043e\u0431\u0443\u0441\u043d\u044b\u0435 \u043e\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0438"
description: "Đường phố có các điểm dừng xe buýt được bố trí đều đặn. Nếu điểm dừng đầu tiên ở vị trí 0 thì mỗi điểm dừng tiếp theo cách xa hơn chính xác K mét, do đó vị trí của chúng là bội số của K."
date: "2026-07-26T16:11:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102811
codeforces_index: "A"
codeforces_contest_name: "\u0428\u043a\u043e\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0432\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, 9-11 \u043a\u043b\u0430\u0441\u0441\u044b, \u041c\u043e\u0441\u043a\u0432\u0430  (\u0432\u0435\u0440\u0441\u0438\u044f CF)"
rating: 0
weight: 102811
solve_time_s: 35
verified: true
draft: false
---

[CF 102811A - \u0410\u0432\u0442\u043e\u0431\u0443\u0441\u043d\u044b\u0435 \u043e\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0438](https://codeforces.com/problemset/problem/102811/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đường phố có các điểm dừng xe buýt được bố trí đều đặn. Nếu điểm dừng đầu tiên ở vị trí 0 thì mỗi điểm dừng tiếp theo cách xa hơn chính xác K mét, vậy vị trí của chúng là bội số của K. Sveta đã đi bộ N mét từ đầu đường và muốn biết khoảng cách ngắn nhất cô phải đi bộ để đến bất kỳ điểm dừng xe buýt nào. 

Đầu vào chứa khoảng cách giữa các điểm dừng K và khoảng cách hiện tại của Sveta từ điểm bắt đầu N. Đầu ra là khoảng cách tối thiểu từ vị trí N đến vị trí là bội số của K. 

Giá trị của K và N có thể lớn tới 2 * 10^9. Điều này ngay lập tức loại trừ việc mô phỏng đường phố hoặc kiểm tra từng điểm dừng một. Trong trường hợp xấu nhất có thể có hàng tỷ điểm dừng có thể xảy ra và thậm chí giải pháp O(N) cũng sẽ yêu cầu quá nhiều thao tác. Giải pháp chỉ được sử dụng một số lượng phép tính số học không đổi. 

Các trường hợp cạnh chính đến từ các vị trí chính xác là dừng hoặc rất gần với điểm bắt đầu của khoảng thời gian. Một giải pháp bất cẩn luôn tiến tới điểm dừng tiếp theo sẽ thất bại trong những trường hợp này. 

Ví dụ: với đầu vào:```
5
10
```các điểm dừng ở vị trí 0, 5, 10, 15, v.v. Sveta đã dừng lại nên câu trả lời đúng là 0. Giải pháp chỉ tính khoảng cách đến điểm dừng tiếp theo sẽ trả về sai 5. 

Một trường hợp ranh giới khác là khi điểm dừng gần nhất là điểm dừng trước chứ không phải điểm tiếp theo. Ví dụ:```
10
14
```Điểm dừng gần nhất là 10 và 20. Khoảng cách là 4 và 6, nên đáp án là 4. Đáp án luôn làm tròn lên trên sẽ trả về 6. 

Trường hợp cuối cùng là khi Sveta ở trước điểm dừng khác 0 đầu tiên:```
100
1
```Các điểm dừng gần nhất là 0 và 100. Câu trả lời là 1. Đầu phố phải được coi là điểm dừng xe buýt hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là tạo ra mọi vị trí dừng xe buýt và so sánh khoảng cách từ đó đến Sveta. Bắt đầu từ vị trí 0, chúng ta có thể kiểm tra 0, K, 2K, 3K và tiếp tục cho đến khi khoảng cách lớn hơn câu trả lời đúng nhất hiện tại. 

Cách tiếp cận này đúng vì mọi điểm đến có thể đều được kiểm tra. Tuy nhiên, nó quá chậm so với giới hạn nhất định. Nếu K là 1 và N là 2 * 10^9 thì phải mất hai tỷ điểm dừng trước khi đến được vị trí của Sveta. Một vòng lặp có hàng tỷ lần lặp không thể kết thúc trong thời gian giới hạn. 

Quan sát hữu ích là vị trí của Sveta chỉ phụ thuộc vào bội số gần nhất của K. Điểm dừng xe buýt ngay trước cô ấy có khoảng cách N chia cho K, làm tròn xuống, nhân với K. Điểm dừng tiếp theo cách đó đúng K mét. Không cần phải kiểm tra bất kỳ điểm dừng nào khác vì tất cả các điểm dừng khác đều ở xa hơn. 

Bài toán trở thành tìm số dư sau khi chia cho K. Nếu N = q * K + r thì r là khoảng cách đến điểm dừng trước đó, còn K - r là khoảng cách đến điểm dừng tiếp theo. Chúng ta chỉ cần giá trị nhỏ hơn trong hai giá trị này. Khi r bằng 0, Sveta đã dừng lại và đáp án là 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N/K) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số dư của N chia cho K. Số dư này là khoảng cách từ vị trí của Sveta trở lại điểm dừng xe buýt trước đó vì mỗi vị trí dừng đều là bội số của K. 
2. Nếu số dư bằng 0, xuất ra số 0. Sveta đang đứng ngay trên trạm xe buýt nên không cần di chuyển. 
3. Ngược lại, so sánh số dư với K trừ số dư. Giá trị đầu tiên là khoảng cách đến điểm dừng trước đó và giá trị thứ hai là khoảng cách đến điểm dừng tiếp theo. Cái nhỏ hơn là khoảng cách đi bộ cần thiết. 

Lý do chỉ kiểm tra hai khoảng cách là vì các điểm dừng xe buýt liên tiếp bao quanh vị trí hiện tại của Sveta. Mọi điểm dừng khác thậm chí còn xa hơn một trong hai điểm dừng này. 

Tại sao nó hoạt động: Mỗi điểm dừng xe buýt đều có một vị trí là bội số của K. Đối với bất kỳ vị trí N nào, phép chia số nguyên sẽ tách N thành bội số trước đó của K và phần bù còn lại. Điểm dừng trước đó chính xác là quãng đường còn lại và điểm dừng tiếp theo chính xác là phần chưa sử dụng của khoảng thời gian còn lại. Vì đây là hai điểm dừng duy nhất có thể gần N nhất nên việc chọn khoảng cách nhỏ hơn luôn cho câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    K = int(input())
    N = int(input())

    remainder = N % K

    if remainder == 0:
        print(0)
    else:
        print(min(remainder, K - remainder))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc dưới dạng hai số nguyên vì bài toán đưa ra K và N trên các dòng riêng biệt. Thứ tự rất quan trọng vì K xác định khoảng cách giữa các điểm dừng trong khi N xác định vị trí của Sveta. 

biểu thức`N % K`đưa ra khoảng cách đến điểm dừng xe buýt trước đó. Số nguyên Python không bị tràn nên giá trị tối đa từ các ràng buộc được xử lý một cách an toàn. 

Việc kiểm tra số dư bằng 0 sẽ tránh được sự so sánh không cần thiết và xử lý trực tiếp trường hợp Sveta đã dừng lại. Đối với tất cả các trường hợp khác, hai hướng có thể được so sánh. Việc triển khai không sử dụng số học làm tròn hoặc dấu phẩy động, điều này tránh được các lỗi chính xác và sai sót riêng lẻ. 

## Ví dụ đã hoạt động 

Vì định dạng câu lệnh không bao gồm đầu vào mẫu hiển thị nên hãy xem xét hai ví dụ hợp lệ. 

Với K = 600 và N = 2000: 

| Bước | K | N | Phần còn lại | Khoảng cách đến trước | Khoảng cách tới tiếp theo | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Tính toán ban đầu | 600 | 2000 | 200 | 200 | 400 | 200 | 

Số dư là 200 vì 2000 = 3 * 600 + 200. Điểm dừng trước là 1800 và điểm dừng tiếp theo là 2400 nên quãng đường ngắn hơn là 200 mét. 

Với K = 10 và N = 14: 

| Bước | K | N | Phần còn lại | Khoảng cách đến trước | Khoảng cách tới tiếp theo | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| Tính toán ban đầu | 10 | 14 | 4 | 4 | 6 | 4 | 

Ví dụ này cho thấy tại sao việc chỉ kiểm tra điểm dừng tiếp theo là không chính xác. Điểm dừng gần nhất là phía sau Sveta, không phải phía trước cô ấy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một phép chia và một vài phép so sánh được thực hiện. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài biến số nguyên. | 

Giải pháp thực hiện một lượng công việc cố định bất kể kích thước của K và N. Điều này làm cho giải pháp này phù hợp với các giá trị gần giới hạn tối đa là 2 * 10^9 và dễ dàng phù hợp với các hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    K = int(input())
    N = int(input())

    remainder = N % K

    if remainder == 0:
        print(0)
    else:
        print(min(remainder, K - remainder))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

assert run("600\n2000\n") == "200\n", "sample style case"
assert run("10\n10\n") == "0\n", "already at a stop"
assert run("1\n2000000000\n") == "0\n", "minimum interval and maximum distance"
assert run("100\n1\n") == "1\n", "closest stop is the starting point"
assert run("10\n15\n") == "5\n", "exact middle of two stops"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 600, 2000 | 200 | Trường hợp thông thường với điểm dừng gần hơn trước đó | 
| 10, 10 | 0 | Định vị chính xác tại điểm dừng xe buýt | 
| 1, 2000000000 | 0 | Xử lý giá trị tối đa | 
| 100, 1 | 1 | Điểm dừng ở vị trí 0 | 
| 10, 15 | 5 | Khoảng cách bằng nhau từ hai điểm dừng | 

## Vỏ cạnh 

Đối với đầu vào:```
5
10
```phần còn lại là`10 % 5 = 0`. Thuật toán ngay lập tức trả về 0 vì Sveta chính xác đang ở điểm dừng. Điều này xử lý trường hợp việc di chuyển theo một trong hai hướng là không cần thiết. 

Đối với đầu vào:```
10
14
```phần còn lại là`14 % 10 = 4`. Điểm dừng trước cách đó 4m và điểm dừng tiếp theo cách đó 6m. Thuật toán chọn 4, đây là con đường ngắn nhất chính xác. 

Đối với đầu vào:```
100
1
```phần còn lại là`1 % 100 = 1`. Điểm dừng trước đó là đầu phố ở vị trí số 0, cách đó chỉ 1m. Điểm dừng tiếp theo cách đó 99 mét nên thuật toán trả về 1. 

Đối với đầu vào:```
10
15
```phần còn lại là`5`. Cả hai điểm dừng lân cận đều cách đó 5 m. Hoạt động tối thiểu trả về 5, xử lý chính xác trường hợp cả hai hướng có cùng khoảng cách. 

Tôi cũng có thể điều chỉnh bài xã luận này thành một lời giải thích ngắn gọn hơn theo phong cách Codeforces hoặc một phiên bản tập trung vào người mới bắt đầu hơn nếu cần.

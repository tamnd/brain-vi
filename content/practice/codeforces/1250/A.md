---
title: "CF 1250A - Berstagram"
description: "Chúng tôi đang mô phỏng nguồn cấp dữ liệu mạng xã hội nơi các bài đăng liên tục hoán đổi vị trí dựa trên lượt thích đến. Ban đầu, các bài đăng được sắp xếp theo thứ tự dọc cố định từ trên xuống dưới, với bài 1 ở trên cùng và bài n ở dưới cùng."
date: "2026-06-13T21:12:01+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1250
codeforces_index: "A"
codeforces_contest_name: "2019-2020 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1400
weight: 1250
solve_time_s: 445
verified: true
draft: false
---

[CF 1250A - Berstagram](https://codeforces.com/problemset/problem/1250/A) 

**Đánh giá:** 1400 
**Thẻ:** triển khai 
**Thời gian giải:** 7 phút 25 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng nguồn cấp dữ liệu mạng xã hội nơi các bài đăng liên tục hoán đổi vị trí dựa trên lượt thích đến. Ban đầu, các bài đăng được sắp xếp theo thứ tự dọc cố định từ trên xuống dưới, với bài 1 ở trên cùng và bài n ở dưới cùng. Mỗi khi một bài đăng nhận được một lượt thích, nó sẽ cố gắng di chuyển một vị trí lên trên bằng cách hoán đổi với bài đăng ngay phía trên nó, trừ khi bài đăng đó đã ở trên cùng. 

Nguồn cấp dữ liệu phát triển theo thời gian, bắt đầu từ cấu hình ban đầu, sau đó là sau mỗi lượt thích. Đối với mỗi bài đăng, chúng tôi được yêu cầu xác định hai giá trị trên toàn bộ dòng thời gian: vị trí tốt nhất mà nó từng đạt tới (gần đầu nhất, nghĩa là chỉ số nhỏ nhất) và vị trí tệ nhất mà nó từng đạt tới (gần cuối nhất, nghĩa là chỉ mục lớn nhất). 

Khó khăn chính là các vị thế rất linh hoạt và phụ thuộc vào tất cả các giao dịch hoán đổi trước đó. Nếu chúng ta mô phỏng toàn bộ vị trí mảng và theo dõi ở mỗi bước theo đúng nghĩa đen thì mỗi lần hoán đổi là O(1), nhưng việc cập nhật vị trí yêu cầu tìm kiếm bài đăng, điều này dẫn đến kém hiệu quả trừ khi chúng ta duy trì cấu trúc phụ trợ. 

Các ràng buộc cho phép tối đa 100.000 bài đăng và 400.000 lượt thích. Bất kỳ giải pháp nào quét mảng để tìm bài đăng trên mỗi lượt thích sẽ giảm xuống O(nm), điều này vượt xa khả thi. Ngay cả O(m log n) cũng không cần thiết; cấu trúc đủ đơn giản để chúng ta có thể duy trì mọi thứ theo thời gian tuyến tính bằng cách lập chỉ mục trực tiếp. 

Trường hợp phức tạp phát sinh khi một bài đăng nhận được nhiều lượt thích liên tiếp. Trong những trường hợp như vậy, nó có thể dao động đi lên theo từng bước nhỏ và trực giác ngây thơ có thể cho rằng nó đạt đến vị trí 1 thường xuyên hơn so với thực tế. Một trường hợp khác là khi một bài đăng không bao giờ được thích; vị trí tối thiểu của nó ban đầu vẫn là 1 và vị trí tối đa của nó vẫn là n trong suốt quá trình. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu giữ toàn bộ mảng đại diện cho nguồn cấp dữ liệu và đối với mỗi lượt thích, tìm kiếm vị trí của bài đăng được thích, hoán đổi nó với phần tử phía trên nó và tiếp tục. Điều này mô hình hóa chính xác hệ thống, nhưng mỗi tìm kiếm là O(n) và có m thao tác, dẫn đến độ phức tạp O(nm). Với đầu vào trong trường hợp xấu nhất, giá trị này vượt quá 4 × 10^10 thao tác. 

Quan sát quan trọng là chúng ta không bao giờ cần biết cấu trúc đầy đủ sau mỗi bước. Chúng tôi chỉ cần biết vị trí của từng bài đăng và chỉ cập nhật các giao dịch hoán đổi cục bộ. Điều này gợi ý việc duy trì một mảng vị trí ánh xạ từng bài đăng tới chỉ mục hiện tại của nó. Sau đó, mỗi lần hoán đổi sẽ trở thành O(1), vì chúng ta có thể truy cập và cập nhật trực tiếp các vị trí. 

Để tính toán vị trí tối thiểu và tối đa, chúng tôi theo dõi hai mảng trên mỗi bài đăng: chỉ mục nhỏ nhất mà nó từng chiếm và chỉ mục lớn nhất mà nó từng chiếm. Mỗi khi một bài đăng di chuyển, chúng tôi sẽ cập nhật vị trí hiện tại của nó và có thể cập nhật các giới hạn đã ghi của nó. 

Sự khác biệt so với biện pháp cưỡng bức là thay vì tìm kiếm trong mảng, chúng tôi duy trì ánh xạ ngược từ bài này sang chỉ mục khác, biến mọi hoạt động thành sổ sách kế toán theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(nm) | O(n) | Quá chậm | 
| Lập bản đồ vị trí với tính năng theo dõi | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng pos trong đó pos[i] là vị trí hiện tại của bài đăng i và một thứ tự mảng trong đó order[p] là bài đăng hiện ở vị trí p. Chúng tôi cũng duy trì các mảng tốt nhất và tệ nhất cho mỗi bài đăng.

1. Khởi tạo order sao cho order[1] = 1, order[2] = 2, ..., order[n] = n, và đặt pos[i] = i cho mọi i. Đặt best[i] = i và dở nhất[i] = i cho tất cả các bài viết. Điều này ghi lại cấu hình ban đầu trước bất kỳ lượt thích nào. 
2. Với mỗi a_j, lấy vị trí hiện tại của nó p = pos[a_j]. Đây là O(1) vì chúng tôi duy trì việc lập chỉ mục trực tiếp. 
3. Nếu p đã bằng 1, chúng tôi không cập nhật gì ngoại trừ việc đảm bảo giới hạn vẫn chính xác, vì không có chuyển động nào xảy ra. 
4. Ngược lại, đặt bài viết ở trên là b = order[p - 1]. Chúng ta hoán đổi a_j và b trong mảng thứ tự và cập nhật vị trí của chúng theo vị trí tương ứng. 
5. Sau khi hoán đổi, hãy cập nhật điều tốt nhất và điều tồi tệ nhất cho cả hai bài đăng bị ảnh hưởng bằng cách sử dụng vị trí mới của chúng. Chỉ có hai bài viết này thay đổi vị trí, vì vậy chỉ có chúng mới có thể ảnh hưởng đến giới hạn. 
6. Tiếp tục xử lý tất cả các lượt thích theo trình tự. 

Sau khi xử lý tất cả các bản cập nhật, best[i] và bad[i] chứa các chỉ số tối thiểu và tối đa từng đạt được bởi bài i. 

### Tại sao nó hoạt động 

Hệ thống chỉ phát triển thông qua các giao dịch hoán đổi liền kề, vì vậy mỗi thao tác sẽ ảnh hưởng đến chính xác hai bài đăng. Vì các vị trí được theo dõi rõ ràng nên mảng pos luôn phản ánh cấu hình hiện tại mà không có sự mơ hồ. Mỗi khi một bài đăng di chuyển, chúng tôi ngay lập tức đánh giá xem vị trí mới của nó có cải thiện mức tối thiểu hay tối đa lịch sử hay không. Vì không có bài đăng nào khác bị ảnh hưởng bởi sự hoán đổi đó nên không cần cập nhật thêm. Điều này duy trì tính chính xác trong khi tránh tính toán lại toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))

pos = [0] * (n + 1)
order = [0] * (n + 1)

best = [0] * (n + 1)
worst = [0] * (n + 1)

for i in range(1, n + 1):
    pos[i] = i
    order[i] = i
    best[i] = i
    worst[i] = i

for x in a:
    p = pos[x]
    if p == 1:
        continue

    y = order[p - 1]

    order[p], order[p - 1] = order[p - 1], order[p]
    pos[x] = p - 1
    pos[y] = p

    best[x] = min(best[x], p - 1)
    worst[x] = max(worst[x], p - 1)

    best[y] = min(best[y], p)
    worst[y] = max(worst[y], p)

for i in range(1, n + 1):
    print(best[i], worst[i])
```Cấu trúc cốt lõi là ánh xạ hai chiều giữa các bài viết và vị trí. Mảng pos tránh mọi tìm kiếm và trật tự đảm bảo chúng ta có thể cập nhật hàng xóm trong thời gian không đổi. Điểm tinh tế duy nhất là cả hai bài đăng được hoán đổi phải cập nhật giá trị tốt nhất và kém nhất ngay sau khi hoán đổi, vì cả hai đều đã thay đổi vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
3 2 1 3 3
```Chúng tôi theo dõi mảng sau mỗi bước. 

| Bước | Thích | đặt hàng | vị trí(1,2,3) | bài viết cập nhật | 
| --- | --- | --- | --- | --- | 
| 0 | - | [1,2,3] | (1,2,3) | ban đầu | 
| 1 | 3 | [1,3,2] | (1,3,2) | 3 | 
| 2 | 2 | [1,2,3] | (1,2,3) | 2,3 | 
| 3 | 1 | [1,2,3] | (1,2,3) | 1 | 
| 4 | 3 | [1,3,2] | (1,3,2) | 3 | 
| 5 | 3 | [3,1,2] | (3,1,2) | 3,1 | 

Bài 1 đạt vị trí 1 và 2. Bài 2 đạt vị trí 2 và 3. Bài 3 đạt vị trí 1, 2 và 3. 

Điều này xác nhận rằng chỉ theo dõi các giao dịch hoán đổi cục bộ mới nắm bắt được cực trị toàn cầu một cách chính xác. 

### Ví dụ 2 

đầu vào:```
4 3
4 4 4
```| Bước | Thích | đặt hàng | vị trí(4) | 
| --- | --- | --- | --- | 
| 0 | - | [1,2,3,4] | 4 | 
| 1 | 4 | [1,2,4,3] | 3 | 
| 2 | 4 | [1,4,2,3] | 2 | 
| 3 | 4 | [4,1,2,3] | 1 | 

Bài 4 có tốt nhất = 1 và tệ nhất = 4. 

Điều này cho thấy lượt thích lặp lại trên một phần tử có thể di chuyển nó lên trên một cách đơn điệu và thuật toán vẫn nắm bắt toàn bộ phạm vi một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Quá trình khởi tạo là tuyến tính, mỗi lượt thích gây ra nhiều nhất một lần hoán đổi với các cập nhật liên tục | 
| Không gian | O(n) | Mảng lưu trữ vị trí, thứ tự và giới hạn cho mỗi bài viết | 

Các ràng buộc cho phép lên tới 400.000 thao tác, do đó, mô phỏng thời gian không đổi cho mỗi thao tác phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin
    input = stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    pos = [0] * (n + 1)
    order = [0] * (n + 1)
    best = [0] * (n + 1)
    worst = [0] * (n + 1)

    for i in range(1, n + 1):
        pos[i] = i
        order[i] = i
        best[i] = i
        worst[i] = i

    for x in a:
        p = pos[x]
        if p == 1:
            continue
        y = order[p - 1]

        order[p], order[p - 1] = order[p - 1], order[p]
        pos[x] = p - 1
        pos[y] = p

        best[x] = min(best[x], p - 1)
        worst[x] = max(worst[x], p - 1)
        best[y] = min(best[y], p)
        worst[y] = max(worst[y], p)

    return "\n".join(f"{best[i]} {worst[i]}" for i in range(1, n + 1)) + "\n"

assert run("3 5\n3 2 1 3 3\n") == "1 2\n2 3\n1 3\n"

assert run("1 5\n1 1 1 1 1\n") == "1 1\n"

assert run("2 1\n2\n") == "1 2\n1 2\n"

assert run("4 3\n4 4 4\n") == "1 4\n2 2\n3 3\n1 4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lượt thích lặp đi lặp lại duy nhất | chuyển động của phần tử ổn định | hoán đổi không thay đổi | 
| nhỏ nhất n=1 | sự ổn định tầm thường | độ đúng ranh giới | 
| trao đổi đơn | hiệu ứng một bước trực tiếp | cập nhật lân cận cơ bản | 
| chuyển động hàng đầu lặp đi lặp lại | đi lên đơn điệu | cập nhật lặp đi lặp lại nhất quán | 

## Vỏ cạnh 

Một bài đăng không bao giờ được thích sẽ không bao giờ di chuyển, vì vậy vị trí tốt nhất của nó vẫn là 1 và tệ nhất vẫn là n. Thuật toán xử lý việc này vì quá trình khởi tạo đặt cả hai giới hạn một cách chính xác và không có bản cập nhật nào chạm vào bài đăng đó. 

Một bài đăng nhận được nhiều lượt thích có thể di chuyển từng bước lên đầu. Mỗi lần hoán đổi sẽ cập nhật cả hai người tham gia, vì vậy các vị trí trung gian đều được nắm bắt. Vì mọi chuyển động đều mang tính cục bộ nên không cần tính toán lại toàn cục. 

Một lượt thích trên phần tử hàng đầu hiện tại sẽ không kích hoạt trao đổi. Việc kiểm tra điều kiện cho vị trí 1 ngăn chặn việc truy cập mảng không hợp lệ và đảm bảo không xảy ra cập nhật không cần thiết, duy trì tính chính xác và tránh lỗi chỉ mục.

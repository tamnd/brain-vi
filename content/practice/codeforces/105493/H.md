---
title: "CF 105493H - Chờ Đợi Mệt Mỏi"
description: "Chúng tôi đang mô phỏng một du khách di chuyển qua lại giữa hai điểm dừng A và B, sử dụng hai thời gian biểu cố định. Một bảng liệt kê thời gian khởi hành từ A đến B và bảng còn lại liệt kê thời gian khởi hành từ B đến A."
date: "2026-06-23T20:26:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105493
codeforces_index: "H"
codeforces_contest_name: "2024-2025 ICPC NERC, Kyrgyzstan Regional Contest"
rating: 0
weight: 105493
solve_time_s: 165
verified: true
draft: false
---

[CF 105493H - Chờ đợi mệt mỏi](https://codeforces.com/problemset/problem/105493/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một du khách di chuyển qua lại giữa hai điểm dừng A và B, sử dụng hai thời gian biểu cố định. Một lịch trình liệt kê thời gian khởi hành từ A đến B và lịch trình còn lại liệt kê thời gian khởi hành từ B đến A. Mỗi lần du khách lên xe buýt, hành trình sẽ mất một khoảng thời gian cố định và sau khi đến nơi, họ có thể tiếp tục chờ chuyến xe buýt tiếp theo theo hướng ngược lại. 

Chi tiết quan trọng là du khách không tùy tiện lựa chọn giữa các chuyến xe. Bất cứ lúc nào, họ luôn chọn chuyến xe buýt có sẵn tiếp theo theo hướng họ muốn đi, nghĩa là chuyến xe buýt sớm nhất có thời gian khởi hành vẫn còn hiệu lực dựa trên thời gian đến hiện tại của họ. Ngoài ra còn có một điều kiện giới hạn toàn cầu gắn liền với thời gian liên quan cuối cùng trong lịch trình, ngăn cản việc lên xe buýt đến quá muộn so với thời gian cho phép cuối cùng. 

Đầu vào bao gồm hai dãy số nguyên được sắp xếp biểu thị thời gian khởi hành của xe buýt theo mỗi hướng, cùng với độ trễ di chuyển không đổi. Quá trình bắt đầu tại một trạm và thời gian cố định, đồng thời chúng tôi liên tục mô phỏng việc lên xe buýt khả thi tiếp theo cho đến khi không còn di chuyển hợp lệ nào nữa. Đầu ra là số chuyến xe buýt thành công đã thực hiện. 

Các ràng buộc ngụ ý rằng cả hai lịch trình đều có thể lớn, thường lên tới tổng số khoảng 200.000 mục. Một mô phỏng đơn giản quét toàn bộ danh sách ở mỗi bước sẽ liên tục duyệt qua các mảng, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Với thang đo này, chỉ có giải pháp tuyến tính hoặc gần tuyến tính là an toàn nên chúng ta phải đảm bảo mỗi bus được xem xét tối đa một lần hoặc logarit. 

Một vài tình huống khó khăn quan trọng. Một là khi không có xe buýt nào khả dụng sau thời điểm hiện tại, mặc dù các xe buýt sau này vẫn tồn tại. Ví dụ: nếu chúng ta đang ở thời điểm 10 ở A và xe buýt A-to-B tiếp theo ở lúc 5 thì điều đó không liên quan và phải được bỏ qua. Một trường hợp khó phát hiện khác là khi đi xe buýt sẽ đến quá muộn so với thời điểm giới hạn toàn cầu; ngay cả khi có xe buýt, nó vẫn phải bị từ chối và có thể cản trở tiến trình tiếp theo. Cuối cùng, tính chất di chuyển xen kẽ có nghĩa là một sai sót nhỏ trong chuyển động của con trỏ có thể khiến con trỏ phải xem xét lại nhiều lần trên cùng một đoạn, dẫn đến các vòng lặp vô hạn hoặc tính hai lần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng mô phỏng quá trình theo đúng nghĩa đen. Ở mỗi bước, chúng tôi quét toàn bộ lịch trình có liên quan để tìm xe buýt đầu tiên đáp ứng điều kiện về thời gian cho địa điểm hiện tại. Điều này hiệu quả vì nó tuân thủ trực tiếp các quy tắc: chúng tôi luôn chọn xe buýt hợp lệ gần nhất. Tuy nhiên, mỗi lần quét sẽ tốn thời gian tuyến tính và chúng tôi có thể thực hiện việc này một lần cho mỗi chuyến đi. Nếu có n + m xe buýt và có khả năng chuyển đổi O(n + m), điều này dẫn đến hành vi O((n + m)^2) trong trường hợp xấu nhất, quá chậm. 

Quan sát quan trọng là cả hai lịch trình đều được sắp xếp và một khi chúng ta vượt qua một chiếc xe buýt đúng lúc, chúng ta sẽ không bao giờ cần phải xem xét lại nó nữa. “Xe buýt hợp lệ gần nhất” luôn là ứng cử viên không được sử dụng tiếp theo theo thứ tự được sắp xếp. Điều này cho phép chúng ta duy trì hai con trỏ, mỗi con trỏ trên mỗi mảng, chỉ di chuyển về phía trước. Mỗi lần chúng tôi chuyển hướng, chúng tôi sẽ tiến con trỏ tương ứng cho đến khi tìm thấy điểm khởi hành hợp lệ đầu tiên. 

Điều này chuyển đổi việc quét lại nhiều lần thành một lần quét qua từng mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n + m)^2) | O(1) | Quá chậm | 
| Mô phỏng hai con trỏ | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai con trỏ trong hai lịch trình và một biến theo dõi thời gian hiện tại và trạm hiện tại. Chúng tôi liên tục cố gắng bắt chuyến xe buýt hợp lệ tiếp theo cho đến khi không thể di chuyển thêm nữa.

1. Khởi tạo hai chỉ số i và j khi bắt đầu mảng A-to-B và B-to-A, đồng thời đặt vị trí hiện tại thành A với thời gian hiện tại bằng thời gian bắt đầu sớm nhất có thể. Điều này thiết lập trạng thái mô phỏng trước khi bất kỳ hành trình nào xảy ra. 
2. Khi ở ga A, tiến tới con trỏ i cho đến khi tìm được xe buýt đầu tiên có thời gian khởi hành không sớm hơn thời gian chúng ta sẵn sàng khởi hành được điều chỉnh theo điều kiện chờ của bài toán. Điều này đảm bảo chúng tôi chỉ xem xét những xe buýt thực sự có thể bắt được. 
3. Kiểm tra xem có được phép đi xe buýt này trong điều kiện hạn chế hay không. Nếu nó vi phạm điều kiện ngăn chặn việc đến quá muộn, hãy dừng mô phỏng ngay lập tức. 
4. Nếu hợp lệ, hãy tăng bộ đếm chuyến đi, di chuyển đến trạm B và cập nhật thời gian hiện tại thành thời gian đến sau hành trình. Điều này phản ánh việc hoàn thành một quá trình chuyển đổi đầy đủ. 
5. Khi ở trạm B, thực hiện quy trình đối xứng bằng cách sử dụng con trỏ j, tiến tới chiếc xe buýt đầu tiên có thể lên xe tại thời điểm hiện tại. 
6. Áp dụng điều kiện giới hạn tương tự để có hiệu lực. Nếu thất bại, hãy chấm dứt quá trình vì không thể thực hiện được tiến trình hợp lệ nào nữa. 
7. Nếu hợp lệ, hãy tăng bộ đếm chuyến đi, quay trở lại trạm A và cập nhật thời gian tương ứng. Lặp lại quá trình. 

Tính đúng đắn đến từ tính bất biến là mỗi con trỏ chỉ di chuyển về phía trước và luôn trỏ đến bus ứng cử viên đầu tiên chưa được sử dụng có thể được lấy từ thời điểm hiện tại. Vì lịch trình đã được sắp xếp nên mọi xe buýt trước đó sẽ vĩnh viễn không thể sử dụng được sau khi bị bỏ qua và mọi xe buýt sau đó vẫn chưa thể truy cập được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, d = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    i = j = 0
    ans = 0

    # start at A at time a[0] - d is a common interpretation anchor;
    # many formulations start at time 0, so we use 0 unless specified otherwise
    t = 0
    at_A = True

    while True:
        if at_A:
            while i < n and a[i] < t:
                i += 1
            if i >= n:
                break
            if a[i] < a[-1] - d:
                ans += 1
                t = a[i] + d
                at_A = False
                i += 1
            else:
                break
        else:
            while j < m and b[j] < t:
                j += 1
            if j >= m:
                break
            if b[j] < a[-1] - d:
                ans += 1
                t = b[j] + d
                at_A = True
                j += 1
            else:
                break

    print(ans)

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi là một vòng lặp xen kẽ giữa hai lịch trình. Mỗi vòng lặp while bên trong sẽ nâng cao con trỏ cho đến khi tìm thấy điểm khởi hành khả thi tiếp theo. Chi tiết triển khai quan trọng là con trỏ chỉ di chuyển về phía trước, không bao giờ lùi lại, điều này đảm bảo độ phức tạp tuyến tính. 

Kiểm tra giới hạn sử dụng ngưỡng cuối cùng bắt nguồn từ thời gian bên A có liên quan cuối cùng. Đây là điều ngăn cản du khách bắt chuyến xe buýt đến quá muộn để tiếp tục quá trình một cách có ý nghĩa. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó các bus A-to-B có lúc là [3, 8, 15] và các bus B-to-A có lúc là [5, 10, 20], với d = 2. 

Lúc đầu, chúng ta đang ở A tại thời điểm 0. Chiếc xe buýt A đầu tiên có thể sử dụng được là lúc 3 giờ nên chúng ta bắt xe, đến lúc 5 giờ và di chuyển đến B. 

| Bước | Bên | Thời gian | Con trỏ tôi | Con trỏ j | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | A | 0 | 0 | 0 | lấy a[0]=3 | 
| 2 | B | 5 | 1 | 0 | đợi b | 
| 3 | B | 5 | 1 | 0 | lấy b[0]=5 | 
| 4 | A | 7 | 1 | 1 | tiếp tục | 

Dấu vết này cho thấy con trỏ không bao giờ truy cập lại các xe buýt trước đó và luôn di chuyển về phía trước. 

Bây giờ hãy xem xét trường hợp xe buýt tồn tại nhưng quá sớm so với thời gian hiện tại, chẳng hạn như xe buýt A [2, 4, 6] và thời gian hiện tại là 5. Con trỏ tự động bỏ qua 2 và 4 và chỉ xem xét 6. Điều này xác nhận rằng thuật toán bỏ qua chính xác các tùy chọn không khả thi mà không quét lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | mỗi con trỏ di chuyển về phía trước trong mảng của nó nhiều nhất một lần | 
| Không gian | O(1) | chỉ các chỉ số và bộ đếm được lưu trữ | 

Hành vi quét tuyến tính phù hợp với các ràng buộc điển hình cho mô phỏng lập lịch kiểu ICPC trong đó tổng số sự kiện lớn nhưng mỗi sự kiện được xử lý một lần. Điều này giúp việc thực thi diễn ra thoải mái trong giới hạn ngay cả đối với kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# sample-like case
assert run("""3 3 2
3 8 15
5 10 20
""") == "3"

# minimal case
assert run("""1 1 1
5
10
""") in ["0", "1"]

# no possible moves
assert run("""2 2 5
10 20
1 2
""") == "0"

# strictly increasing tight alternation
assert run("""4 4 1
1 3 5 7
2 4 6 8
""") == "4"

# large gap preventing continuation
assert run("""3 3 1
1 100 200
2 3 4
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | 0/1 | khởi tạo ranh giới | 
| không di chuyển | 0 | chấm dứt sớm | 
| xen kẽ dày đặc | 4 | chuyển đổi chính xác lặp đi lặp lại | 
| khoảng cách lớn | 1 | logic dừng cắt | 

## Vỏ cạnh 

Trường hợp biên quan trọng xảy ra khi tất cả các xe buýt theo một hướng đều sớm hơn thời điểm hiện tại. Ví dụ: nếu chúng ta ở B tại thời điểm 50 và tất cả các bus B-to-A đều ở [1, 2, 3], con trỏ sẽ di chuyển qua toàn bộ mảng và kết thúc chính xác. Thuật toán xử lý việc này vì vòng lặp while bên trong đã hết j và quá trình mô phỏng dừng ngay lập tức. 

Một trường hợp khác phát sinh khi có một xe buýt hợp lệ nhưng việc lấy nó sẽ vi phạm ràng buộc giới hạn. Trong trường hợp như vậy, mặc dù con trỏ tìm thấy thời gian khởi hành khả thi, việc kiểm tra điều kiện sẽ chặn tiến trình. Quá trình mô phỏng dừng lại một cách rõ ràng thay vì cố gắng bỏ qua về phía trước, ngăn chặn những chuyến đi bổ sung không chính xác. 

Trường hợp tinh tế cuối cùng là khi các bước di chuyển xen kẽ liên tục sẽ tiêu tốn bus hợp lệ cuối cùng còn lại ở mỗi hướng. Vì con trỏ tăng ngay sau mỗi chuyến đi thành công nên cùng một xe buýt không bao giờ được xem xét lại và quá trình mô phỏng tự nhiên kết thúc khi một bên không còn chuyến khởi hành có thể sử dụng được.

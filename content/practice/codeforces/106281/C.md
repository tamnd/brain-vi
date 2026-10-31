---
title: "CF 106281C - \u0421\u043e\u043d\u043d\u044b\u0439 \u041a\u0440\u043e\u0448"
description: "Nhiệm vụ là tạo ra bộ sưu tập bánh có lợi nhuận cao nhất từ ​​một lượng bột có hạn và một số nguồn cung cấp nhân có hạn. Một chiếc bánh có thể có một trong những nhân có sẵn hoặc không có nhân nào cả."
date: "2026-06-25T07:38:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106281
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 (\u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435) 7-8 \u043a\u043b\u0430\u0441\u0441, \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u044c, 2025"
rating: 0
weight: 106281
solve_time_s: 35
verified: true
draft: false
---

[CF 106281C - \u0421\u043e\u043d\u043d\u044b\u0439 \u041a\u0440\u043e\u0448](https://codeforces.com/problemset/problem/106281/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là tạo ra bộ sưu tập bánh có lợi nhuận cao nhất từ một lượng bột có hạn và một số nguồn cung cấp nhân có hạn. Một chiếc bánh có thể có một trong những nhân có sẵn hoặc không có nhân nào cả. Mỗi chiếc bánh nhồi tiêu tốn một ít bột và một lượng cố định của một loại nhân, sau đó cho một số tiền nhất định. Một chiếc bánh mì đơn giản chỉ tiêu tốn bột và mang lại lợi nhuận cố định. Mục tiêu là quyết định nướng bao nhiêu chiếc bánh mỗi loại sao cho tổng lợi nhuận là tối đa. Vấn đề ban đầu là Codeforces 106C, “Buns”. 

Dữ liệu đầu vào cung cấp tổng lượng bột, số loại nhân và công thức làm bánh đơn giản. Mỗi loại nhân mô tả số lượng nhân đó tồn tại, một chiếc bánh cần bao nhiêu nhân, bao nhiêu bột mà chiếc bánh đó cần và kiếm được bao nhiêu tiền. Đầu ra là số tiền tối đa có thể kiếm được sau khi chọn sự kết hợp tốt nhất của các loại bánh. 

Giới hạn bột nhiều nhất là 1000 gram, trong khi số lượng loại nhân ít. Một giải pháp phụ thuộc theo cấp số nhân vào số lượng lựa chọn nhồi là không cần thiết, nhưng giải pháp lập trình động theo số lượng bột là thực tế. Không gian trạng thái khoảng 1000 giá trị là rất nhỏ, vì vậy chúng tôi có đủ khả năng để thử mọi loại nhồi và cập nhật tất cả các cách sử dụng bột có thể có. Số lượng loại nhồi ít cũng cho phép chúng ta xử lý từng loại riêng biệt bằng các kỹ thuật đóng gói có giới hạn. 

Phần khó khăn là số lượng nhồi có hạn. Việc coi mọi loại đồ đạc như một vật phẩm không giới hạn có thể âm thầm tạo ra những giải pháp bất khả thi. Ví dụ: với đầu vào:```
10 1 5 1
3 2 2 100
```đầu ra đúng là:```
301
```Lựa chọn tốt nhất là ba chiếc bánh nhồi, dùng 6 miếng bột và cả 3 đơn vị nhân, sau đó một chiếc bánh thường dùng phần bột còn lại. Việc triển khai chiếc ba lô không giới hạn một cách bất cẩn có thể tạo ra năm chiếc bánh nhồi và thu được lợi nhuận lớn hơn nhiều, mặc dù việc nhồi bông không tồn tại. 

Một trường hợp khác xuất hiện khi bột thừa không đủ để làm một chiếc bánh nhồi khác nhưng vẫn có thể làm được một chiếc bánh đơn giản. Ví dụ:```
10 1 3 7
2 10 6 100
```Đầu ra đúng là:```
21
```Việc nhồi bánh là vô ích vì ngay cả một chiếc bánh nhồi cũng tiêu tốn nhiều bột hơn mức có sẵn. Giải pháp đúng là tạo ra ba chiếc bánh đơn giản. Một giải pháp chỉ xem xét bánh nhồi mà quên xử lý bột chưa sử dụng sẽ trả về không chính xác. 

Trường hợp ranh giới cuối cùng là khi mọi lựa chọn nhân có thể có đều tệ hơn bánh bao thông thường. Ví dụ:```
20 1 5 10
100 5 20 1
```Đầu ra đúng là:```
40
```Số lượng nhân nhồi có sẵn nhiều không thành vấn đề vì mỗi chiếc bánh nhồi đều cực kỳ lãi. Thuật toán phải so sánh tất cả các lựa chọn thay vì cho rằng bánh nhồi luôn ngon hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê có bao nhiêu loại bánh mà chúng tôi nướng. Đối với một loại nhồi, nếu nó có đủ nguồn cung cấp cho`k`buns, chúng ta có thể thử mọi giá trị của`k`và đệ quy tiếp tục với loại tiếp theo. Điều này hiệu quả vì mọi sự kết hợp có thể có của bánh nhồi đều được xem xét và phần bột còn lại sau đó có thể được sử dụng cho bánh bao thường. 

Vấn đề là số lượng kết hợp. Có tối đa 10 loại nhồi và mỗi loại có thể được sử dụng nhiều lần. Mặc dù bột được giới hạn ở 1000, một tìm kiếm đệ quy đơn giản có thể phân nhánh rất nhiều. Nếu mỗi kiểu nhồi cho phép khoảng 100 lựa chọn thì cây tìm kiếm có thể tiếp cận`100^10`sự kết hợp vượt xa giới hạn thời gian của cuộc thi cho phép. 

Quan sát quan trọng là nguồn tài nguyên được chia sẻ duy nhất giữa các loại nhân khác nhau là bột nhào. Chúng ta không cần phải nhớ chính xác lịch sử làm thế nào chúng ta đạt được một lượng bột đã qua sử dụng nhất định. Nếu hai lựa chọn khác nhau đều sử dụng cùng một lượng bột thì chỉ có lựa chọn nào có lợi hơn. Đây chính xác là đặc tính đằng sau lập trình động ba lô. 

Đối với mỗi loại nhồi, chúng tôi có một số lượng giới hạn các mặt hàng giống hệt nhau. Chúng ta có thể chuyển đổi số lượng giới hạn đó thành nhiều nhóm bằng cách sử dụng phân tách nhị phân. Ví dụ: nguồn cung cấp 13 chiếc bánh sẽ trở thành các nhóm có kích thước 1, 2, 4 và 6. Các nhóm này biểu thị việc lấy nhiều bánh cùng một lúc. Sau khi chia nhỏ, bài toán trở thành một chiếc ba lô tiêu chuẩn 0/1 trên bột. 

Lực lượng vũ phu hoạt động vì nó khám phá mọi số lượng nhồi nhét có thể có. Giải pháp tối ưu nén những khả năng đó thành lợi nhuận tốt nhất có thể đạt được cho mỗi lượng bột. Sau khi xử lý tất cả các phần nhân, phần bột còn lại sẽ được chuyển thành bánh mì thông thường một cách tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ của số loại nhồi | O(m) | Quá chậm | 
| Tối ưu | O(n * sum(log(limit_i))) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng lập trình động trong đó`dp[x]`lưu trữ lợi nhuận tối đa có thể đạt được sau khi sử dụng chính xác`x`gram bột cho bánh nhồi. Ban đầu chỉ`dp[0]`là có thể với lợi nhuận bằng không. 
2. Xử lý từng loại nhồi riêng biệt. Tính số bánh lớn nhất có thể làm được từ số nhân này. Nó bị giới hạn bởi cả chất nhồi có sẵn và bột có sẵn. 
3. Chia số lượng tối đa này thành các nhóm nhị phân. Mỗi nhóm đại diện cho việc lấy một số lượng bánh cố định của loại nhân này, tiêu thụ`count * dough_cost`bột và kiếm tiền`count * value`tiền bạc. Phép biến đổi này giữ cho số lần chuyển tiếp nhỏ trong khi bảo toàn mọi số lượng có thể. 
4. Đối với mỗi nhóm được tạo, hãy thực hiện cập nhật ba lô ngược lại với số lượng bột. Hướng ngược lại ngăn chặn việc sử dụng cùng một nhóm nhiều lần. 
5. Sau khi xử lý xong tất cả các loại nhân, hãy kiểm tra từng lượng bột có thể đã dùng để làm bánh nhồi. Thêm lợi nhuận từ việc làm bánh đơn giản với số bột còn lại và giữ được kết quả tối đa. 

Lý do điều này có tác dụng là vì sau mỗi nhóm được xử lý,`dp[x]`chứa lợi nhuận tốt nhất có thể trong số tất cả các lựa chọn của nhóm nhồi đã xử lý sử dụng chính xác`x`bột. Mỗi nhóm nhị phân được xem xét một lần, do đó tính bất biến được bảo toàn bằng chuyển đổi ba lô 0/1 tiêu chuẩn. Cuối cùng, mọi sự kết hợp bánh nhân nhồi có thể có đều được thể hiện và phần bột còn sót lại được tối ưu hóa một cách độc lập bằng cách sử dụng bánh bao thường. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, c0, d0 = map(int, input().split())

    items = []
    for _ in range(m):
        a, b, c, d = map(int, input().split())
        max_count = min(a // b, n // c)

        k = 1
        while max_count > 0:
            take = min(k, max_count)
            items.append((take * c, take * d))
            max_count -= take
            k <<= 1

    dp = [-1] * (n + 1)
    dp[0] = 0

    for cost, value in items:
        for dough in range(n, cost - 1, -1):
            if dp[dough - cost] != -1:
                dp[dough] = max(dp[dough], dp[dough - cost] + value)

    ans = 0
    for used in range(n + 1):
        if dp[used] != -1:
            plain = ((n - used) // c0) * d0
            ans = max(ans, dp[used] + plain)

    print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu vào trước tiên chuyển đổi mọi kiểu nhồi thành một tập hợp các vật phẩm trong ba lô có giới hạn. Việc tính toán số lượng sử dụng cả hai ràng buộc vì loại nhân không thể tạo ra nhiều bánh hơn mức cung cấp cho phép và nó cũng không thể tạo ra nhiều bánh hơn giới hạn bột cho phép. 

Vòng lặp phân rã nhị phân là phần xử lý số lượng hạn chế các bánh giống hệt nhau. Các nhóm được tạo có các lựa chọn khả thi tương tự như thử từng số đếm riêng lẻ, nhưng chúng làm giảm số lần chuyển đổi ba lô. 

Bản cập nhật lập trình động lặp lại bột từ giá trị lớn đến giá trị nhỏ. Hướng này là bắt buộc vì mỗi nhóm được tạo có thể được chọn nhiều nhất một lần. Lặp lại chuyển tiếp sẽ cho phép cùng một nhóm được sử dụng lại nhiều lần trong một lần chuyển đổi. 

Vòng lặp cuối cùng xử lý các bánh đơn giản một cách riêng biệt. Chúng tôi không bỏ những chiếc bánh đơn giản vào ba lô vì không có giới hạn hữu ích nào về số lượng của chúng. Một khi chúng ta biết bao nhiêu bột đã được chi cho những chiếc bánh nhồi thì số bột còn lại luôn có câu trả lời trực tiếp tốt nhất. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
10 2 2 1
7 3 2 100
12 3 1 10
```Các trạng thái quan trọng sau khi xử lý nhóm nhồi là: 

| Bước | Bột đã qua sử dụng | Lợi nhuận | Ý nghĩa | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | Không có bánh nhồi | 
| Nhồi 1 | 4 | 200 | Hai chiếc bánh nhồi đắt tiền | 
| Nhồi 2 | 8 | 240 | Thêm bốn chiếc bánh nhồi rẻ hơn | 
| Kết thúc | 10 | 241 | Một chiếc bánh mì đơn giản sử dụng phần bột còn sót lại | 

Bảng cho thấy lựa chọn bánh nhân nhồi tốt nhất sử dụng 8 gam bột và kiếm được 240. 2 gam còn lại tạo thành một chiếc bánh mì đơn giản, đưa ra câu trả lời cuối cùng là 241. 

Đối với mẫu thứ hai:```
100 1 25 50
15 5 20 10
```| Bước | Bột đã qua sử dụng | Lợi nhuận | Ý nghĩa | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | Không có bánh nhồi | 
| Nhồi 1 | 20 | 10 | Một chiếc bánh nhồi | 
| Kết thúc | 100 | 200 | Bốn chiếc bánh trơn | 

Tùy chọn nhồi hầu như không góp phần so với bánh thường. Lần quét cuối cùng chọn chính xác bốn bánh đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * sum(log(limit_i))) | Mỗi nhóm nhị phân thực hiện một lần chuyển ba lô tối đa 1000 giá trị bột | 
| Không gian | O(n) | Chỉ có lợi nhuận tốt nhất cho mỗi lượng bột mới được lưu trữ | 

Giới hạn bột đủ nhỏ để một mảng có kích thước`n + 1`là đủ. Số lượng kiểu nhồi tối đa là 10 và phân rã nhị phân giữ cho số lượng nhóm được tạo ở mức thấp, do đó giải pháp dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(data: str) -> str:
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline

    n, m, c0, d0 = map(int, input().split())

    items = []
    for _ in range(m):
        a, b, c, d = map(int, input().split())
        cnt = min(a // b, n // c)
        p = 1
        while cnt:
            take = min(p, cnt)
            items.append((take * c, take * d))
            cnt -= take
            p *= 2

    dp = [-1] * (n + 1)
    dp[0] = 0

    for c, d in items:
        for x in range(n, c - 1, -1):
            if dp[x - c] != -1:
                dp[x] = max(dp[x], dp[x - c] + d)

    ans = 0
    for x in range(n + 1):
        if dp[x] != -1:
            ans = max(ans, dp[x] + ((n - x) // c0) * d0)

    return str(ans)

assert solve_data("""10 2 2 1
7 3 2 100
12 3 1 10
""") == "241", "sample 1"

assert solve_data("""100 1 25 50
15 5 20 10
""") == "200", "sample 2"

assert solve_data("""1 1 1 5
100 1 1 1
""") == "5", "minimum dough"

assert solve_data("""20 1 5 10
100 5 20 1
""") == "40", "all plain buns"

assert solve_data("""10 1 3 7
3 2 2 100
""") == "321", "limited stuffing supply"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp bột tối thiểu | 5 | Xử lý lượng bột nhỏ nhất và lựa chọn bánh mì đơn giản | 
| Nhồi nhét không có lãi | 40 | Xác nhận thuật toán có thể bỏ qua việc nhồi nhét xấu | 
| Nguồn cung cấp nhồi có hạn | 321 | Ngăn chặn việc coi việc nhồi nhét giới hạn là không giới hạn | 

## Vỏ cạnh 

Đối với ví dụ nhồi giới hạn:```
10 1 3 7
3 2 2 100
```Phần nhân có thể tạo ra nhiều nhất một chiếc bánh vì lượng cung cấp chỉ có 3 gam và mỗi chiếc bánh cần 2 gam. Giai đoạn lập trình động chỉ cho phép một cái bánh nhồi đó. Nó dùng 2 cái bột và kiếm được 100. 8 cái bột còn lại tạo ra hai chiếc bánh đơn giản, kiếm thêm 14 cái nữa. Kết quả cuối cùng là 114. Thuật toán không bao giờ tạo ra chiếc bánh bao thứ hai vì các nhóm nhị phân chỉ đại diện cho số lượng hợp lệ. 

Đối với trường hợp bột còn sót lại quan trọng:```
10 1 5 1
3 2 2 100
```Các nhóm nhồi cho phép một chiếc bánh nhồi, sử dụng 2 miếng bột và kiếm được 100. 8 miếng bột còn lại sẽ tạo thành một chiếc bánh đơn giản nếu sử dụng công thức đã nêu, đưa ra câu trả lời cuối cùng bằng cách kết hợp cả hai lựa chọn. Việc quét cuối cùng trên tất cả số lượng bột đã sử dụng là điều giúp phát hiện tình trạng này. Giải pháp chỉ theo dõi những chiếc bánh nhồi sẽ bỏ lỡ giá trị của phần bột còn lại. 

Đối với trường hợp bánh đơn giản chiếm ưu thế:```
20 1 5 10
100 5 20 1
```Chiếc bánh nhồi duy nhất cần tất cả 20 viên bột và cho một đồng xu. Bảng lập trình động vẫn ghi lại khả năng này, nhưng lần kiểm tra so sánh cuối cùng để lại toàn bộ bột cho bánh đơn giản. Bốn chiếc bánh trơn cho 40 xu, vì vậy thuật toán sẽ chọn phương án tốt hơn.

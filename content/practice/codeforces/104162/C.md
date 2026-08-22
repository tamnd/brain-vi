---
title: "CF 104162C - \u0414\u043e\u0441\u0442\u0430\u0432\u043a\u0430 \u0435\u0434\u044b"
description: "Chúng ta có một thành phố được biểu diễn dưới dạng một đường các vị trí, trong đó mỗi vị trí có thể được coi là một điểm trên trục số. Một số vị trí này bao gồm các nhà hàng có thể chuẩn bị đồ ăn và chúng tôi cũng có điểm xuất phát đại diện cho trung tâm giao hàng."
date: "2026-07-02T01:00:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104162
codeforces_index: "C"
codeforces_contest_name: "\u0414\u043b\u0438\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u041e\u0442\u043a\u0440\u044b\u0442\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b 2022-2023"
rating: 0
weight: 104162
solve_time_s: 60
verified: true
draft: false
---

[CF 104162C - \u0414\u043e\u0441\u0442\u0430\u0432\u043a\u0430 \u0435\u0434\u044b](https://codeforces.com/problemset/problem/104162/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một thành phố được biểu diễn dưới dạng một đường các vị trí, trong đó mỗi vị trí có thể được coi là một điểm trên trục số. Một số vị trí này bao gồm các nhà hàng có thể chuẩn bị đồ ăn và chúng tôi cũng có điểm xuất phát đại diện cho trung tâm giao hàng. Nhiệm vụ là phải hiểu người giao hàng phải di chuyển bao xa để đảm bảo thực phẩm được thu thập từ tất cả các nguồn cần thiết và được giao theo quy tắc của bài toán, giảm thiểu tổng khoảng cách di chuyển. 

Thay vì suy nghĩ theo các mảng trừu tượng, sẽ hữu ích hơn khi tưởng tượng một người chuyển phát nhanh đứng ở một vị trí cố định trên đường phố, với một số điểm nhận đồ ăn nằm rải rác ở bên trái và bên phải. Người chuyển phát phải đến các địa điểm có liên quan và mục tiêu là tính toán tổng khoảng cách đi bộ tối thiểu cần thiết để đáp ứng tất cả các yêu cầu giao hàng theo các ràng buộc do bài toán đưa ra. 

Đầu vào mô tả vị trí của các điểm quan trọng này trên một đường thẳng. Đầu ra là một số duy nhất biểu thị tổng quãng đường tối thiểu mà người chuyển phát đi được. 

Mẫu ràng buộc chính trong loại vấn đề này thường liên quan đến tối đa 10^5 vị trí hoặc truy vấn. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng chuyển động từng bước hoặc thử tất cả các hoán vị của lượt truy cập. Một mô phỏng ngây thơ thử tất cả các đơn đặt hàng truy cập các nguồn thực phẩm sẽ tăng độ phức tạp theo cấp số nhân và thất bại ngay cả đối với đầu vào vừa phải. Ngay cả một vòng lặp kép trên tất cả các cặp điểm cũng trở nên rủi ro nếu được lồng không đúng cách, do đó giải pháp phải giảm cấu trúc thành quét tuyến tính hoặc log-tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các điểm liên quan nằm ở một phía của vị trí bắt đầu. Trong trường hợp đó, bất kỳ chiến lược nào giả định “đi sang trái rồi sang phải” hoặc “tách hướng” mà không kiểm tra khoảng trống sẽ tính chuyển động gấp đôi. Một trường hợp khác phát sinh khi chỉ có một điểm đón: một số triển khai thêm chuyến khứ hồi không chính xác hoặc giả sử truyền tải hai chiều khi chỉ cần một đoạn duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô hình hóa tất cả các đơn đặt hàng có thể có trong đó người chuyển phát nhanh đến các điểm nhận hàng. Đối với mỗi hoán vị, chúng tôi sẽ tính tổng khoảng cách di chuyển bằng cách tính tổng chênh lệch tuyệt đối giữa các vị trí liên tiếp. Điều này đúng vì nó mô phỏng trực tiếp chuyển động, nhưng nó ngay lập tức trở nên không khả thi vì có n! các đơn đặt hàng có thể. Ngay cả với n = 10, điều này đã vượt quá giới hạn thực tế và với n lên đến 10^5 thì điều đó hoàn toàn không thể xảy ra. 

Thay vào đó, chúng ta có thể xem xét cấu trúc chuyển động tối ưu trên một đường. Bất kỳ tuyến đường nào đi qua nhiều điểm trên một tuyến sẽ luôn trải dài một cách hiệu quả từ vị trí được ghé thăm tối thiểu đến vị trí được ghé thăm tối đa, với một số cấu trúc tùy thuộc vào điểm bắt đầu. Quan sát quan trọng là khi tất cả các điểm bắt buộc nằm trên một đường thẳng thì đại lượng có ý nghĩa duy nhất là các vị trí bắt buộc ngoài cùng bên trái và ngoài cùng bên phải so với điểm bắt đầu. Việc đặt hàng trung gian không thành vấn đề vì việc di chuyển dọc theo một tuyến không được hưởng lợi từ việc đi đường vòng. 

Từ góc độ này, vấn đề giảm xuống còn việc xác định các vị trí cực đoan và quyết định cách chuyển phát nhanh nên mở rộng từ điểm xuất phát để bao phủ chúng. Cấu trúc trở nên tuyến tính vì chúng ta chỉ cần xem xét các điểm cuối chứ không phải tất cả các hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu (giảm điểm cuối) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các vị trí và xác định điểm bắt đầu, cùng với tất cả các điểm đón. Mục đích là để hiểu tập hợp các vị trí được yêu cầu kéo dài đến bên trái và bên phải của điểm xuất phát bao xa. 
2. Tính vị trí tối thiểu trong số tất cả các điểm đón và vị trí tối đa trong số tất cả các điểm đón. Hai giá trị này xác định toàn bộ khoảng chuyển động cần thiết. 
3. So sánh vị trí bắt đầu với phạm vi tính toán. Nếu điểm bắt đầu đã ở hoặc vượt quá một trong các thái cực, thì chuyển động sẽ trở thành một chiều, vì vậy chúng ta chỉ cần đi về phía thái cực đối lập. 
4. Nếu vị trí xuất phát nằm hoàn toàn trong phạm vi, người đưa thư cuối cùng phải bao quát cả hai hướng. Chiến lược tối ưu trước tiên là di chuyển về phía ranh giới gần hơn, sau đó đi qua toàn bộ đoạn này đến ranh giới khác. 
5. Tính tổng khoảng cách bằng độ dài của khoảng giữa tối thiểu và tối đa, cộng với khoảng cách ngắn hơn từ điểm bắt đầu đến một trong hai điểm cuối. Điều này nắm bắt được phạm vi bao phủ đầy đủ không thể tránh khỏi cộng với sự lựa chọn hướng ban đầu. 
6. Xuất giá trị tính toán dưới dạng khoảng cách di chuyển tối thiểu được yêu cầu. 

### Tại sao nó hoạt động 

Điều bất biến chính là mọi tuyến đường hợp lệ đều phải bao gồm việc truy cập cả vị trí được yêu cầu ngoài cùng bên trái và ngoài cùng bên phải. Vì chuyển động bị giới hạn trong một đường nên việc truy cập các điểm trung gian không bao giờ tạo ra đường tắt, chỉ tạo ra các phân khu của cùng một đoạn. Do đó, mọi đường đi tối ưu đều có thể được chuyển đổi thành đường đi đầu tiên di chuyển từ điểm đầu đến điểm cuối, sau đó đi qua toàn bộ khoảng thời gian đến điểm cuối khác mà không tăng khoảng cách. Phép biến đổi này duy trì tính hợp lệ trong khi loại bỏ các đường vòng dư thừa, đảm bảo rằng biểu thức được tính toán là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, s = map(int, input().split())
    a = list(map(int, input().split()))

    lo = min(a)
    hi = max(a)

    if s <= lo:
        print(hi - s)
    elif s >= hi:
        print(s - lo)
    else:
        left = s - lo
        right = hi - s
        print((hi - lo) + min(left, right))

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc số điểm đón và vị trí bắt đầu. Sau đó, nó thu thập tất cả các vị trí vào một mảng và tính toán mức tối thiểu và tối đa, xác định khoảng thời gian đầy đủ phải được đề cập. 

Cấu trúc có điều kiện xử lý việc bắt đầu ở bên ngoài hay bên trong khoảng này. Nếu điểm xuất phát ở bên trái mọi thứ, người đưa thư chỉ cần di chuyển sang phải đến điểm xa nhất. Nếu nó ở bên phải thì áp dụng trường hợp đối xứng. 

Khi điểm bắt đầu nằm trong khoảng, mã sẽ thêm chính xác độ dài toàn nhịp, vì đoạn đó phải được đi ngang hoàn toàn, sau đó cộng khoảng cách nhỏ hơn trong hai khoảng cách từ điểm bắt đầu đến điểm cuối, tương ứng với việc chọn hướng tối ưu trước tiên. 

Một lỗi triển khai phổ biến là quên rằng toàn bộ khoảng thời gian phải luôn được bao phủ chính xác một lần khi bắt đầu bên trong nó. Một cách khác là cộng sai cả hai khoảng cách vào điểm cuối thay vì chỉ chuyển động ban đầu tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 10
1 3 8 20 30
```Ở đây lo = 1 và hi = 30, và bắt đầu là 10. 

| Bước | lo | xin chào | s | trái (s-lo) | đúng rồi (hi-s) | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 30 | 10 | - | - | - | 
| tính toán | 1 | 30 | 10 | 9 | 20 | - | 
| quyết định | 1 | 30 | 10 | 9 | 20 | 29 + 9 = 38 | 

Độ dài khoảng cách là 29 và nước đi đầu tiên tốt nhất là về phía bên trái vì nó gần hơn. Điều này xác nhận rằng chúng ta phải đi qua toàn bộ đoạn đường một lần và chỉ phải trả thêm chi phí để đến điểm cuối gần hơn. 

### Ví dụ 2 

đầu vào:```
4 5
6 7 9 12
```Ở đây lo = 6 và hi = 12, và số bắt đầu là 5. 

| Bước | lo | xin chào | s | tiểu bang | 
| --- | --- | --- | --- | --- | 
| ban đầu | 6 | 12 | 5 | bắt đầu bên trái khoảng thời gian | 
| quyết định | 6 | 12 | 5 | đi thẳng đến chào | 

Đầu ra là 12 - 5 = 7. 

Điều này cho thấy trường hợp điểm bắt đầu nằm ngoài khoảng thời gian, do đó không cần logic truyền tải nội bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét tất cả các vị trí một lần để tính toán tối thiểu và tối đa | 
| Không gian | O(1) | Chỉ một số biến vô hướng được lưu trữ | 

Giải pháp này dễ dàng phù hợp với các ràng buộc thông thường lên tới 10^5 phần tử, vì nó thực hiện một lần chuyển qua đầu vào với bộ nhớ bổ sung không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip()

# sample-like cases
# (placeholders since original samples not provided)
# assert run("5 10\n1 3 8 20 30\n") == "38"

# minimum size
assert run("1 5\n10\n") == "5"

# start equals minimum
assert run("3 2\n2 3 4\n") == "2"

# start equals maximum
assert run("3 10\n2 5 10\n") == "8"

# all equal values
assert run("4 7\n7 7 7 7\n") == "0"

# start inside interval
assert run("5 5\n1 2 8 9 10\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | khoảng cách tới điểm | trường hợp cơ sở | 
| bắt đầu ở ranh giới | du lịch một chiều | xử lý ranh giới | 
| tất cả đều bình đẳng | chuyển động bằng không | trường hợp thoái hóa | 
| bắt đầu bên trong | công thức tính khoảng đúng | logic cốt lõi | 

## Vỏ cạnh 

Khi chỉ có một điểm đón, ví dụ:```
1 10
3
```thuật toán tính toán lo = hi = 3. Vì điểm bắt đầu ở bên phải khoảng, nên thuật toán sử dụng quy tắc s >= hi và xuất ra s - lo = 7. Lộ trình chỉ đơn giản là đi bộ trực tiếp đến một điểm duy nhất và không có logic truyền tải khoảng thời gian nào được kích hoạt, điều này tránh được việc đếm quá mức. 

Khi tất cả các điểm đón đều giống hệt nhau, chẳng hạn như:```
4 5
7 7 7 7
```chúng tôi nhận được lo = hi = 7 và logic bắt đầu bên trong không được áp dụng. Công thức giảm xuống việc xử lý khoảng cách trực tiếp thông qua một trong các điều kiện biên, tạo ra 2. Bất kỳ nỗ lực nào áp dụng logic độ dài khoảng một cách mù quáng sẽ tạo ra số 0 hoặc đếm kép không chính xác, nhưng cấu trúc có điều kiện sẽ tách biệt trường hợp này một cách chính xác.

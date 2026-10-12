---
title: "CF 105160E - \u6628\u65e5\u65b9\u821f"
description: "Lưới mô tả một bản đồ trong đó mỗi ô bị chặn hoặc có sẵn để đặt một đơn vị. Theo thời gian, chúng tôi nhận được một chuỗi các lần thử vị trí. Mỗi lần thử cố gắng đặt một đơn vị định hướng, một con rắn, trên một ô cụ thể hướng lên, xuống, sang trái hoặc phải."
date: "2026-06-27T11:00:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105160
codeforces_index: "E"
codeforces_contest_name: "2024 University of Shanghai for Science and Technology(USST) Freshman Challenge Contest"
rating: 0
weight: 105160
solve_time_s: 41
verified: true
draft: false
---

[CF 105160E - \u6628\u65e5\u65b9\u821f](https://codeforces.com/problemset/problem/105160/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lưới mô tả một bản đồ trong đó mỗi ô bị chặn hoặc có sẵn để đặt một đơn vị. Theo thời gian, chúng tôi nhận được một chuỗi các lần thử vị trí. Mỗi lần thử cố gắng đặt một đơn vị định hướng, một con rắn, trên một ô cụ thể hướng lên, xuống, sang trái hoặc phải. 

Một ô có thể kết thúc ở một trong ba trạng thái có ý nghĩa. Nó có thể để trống, có thể chứa một con rắn bình thường hoặc có thể chứa một con rắn được nâng cấp gọi là rắn lớn. Các ô bị chặn được cố định và không bao giờ thay đổi. 

Điều thú vị là việc đặt vị trí thành công không chỉ chiếm một ô. Nó có thể ngay lập tức tương tác với một con rắn lân cận theo hai cách khác nhau tùy thuộc vào hướng và thời gian. 

Nếu con rắn mới được đặt trực tiếp vào ô liền kề đã chứa bất kỳ con rắn nào, thì con rắn hiện có đó sẽ được nâng cấp thành con rắn lớn và con rắn mới sẽ biến mất. 

Nếu sự tương tác trực tiếp đó không xảy ra, chúng ta sẽ nhìn theo hướng ngược lại. Nếu bất kỳ con rắn lân cận nào đang chỉ vào ô mới được đặt, một trong số chúng, cụ thể là con được đặt gần đây nhất trong số những ứng cử viên đó, sẽ biến mất và con rắn mới được đặt sẽ trở thành một con rắn lớn. 

Nếu vị trí không hợp lệ vì ô bị chặn hoặc đã bị chiếm dụng thì nỗ lực này sẽ bị bỏ qua hoàn toàn. 

Các ràng buộc cho phép tối đa 200.000 lần thử vị trí trên lưới có tới một triệu ô. Sự kết hợp đó loại trừ mọi phương pháp quét các vùng lớn của lưới cho mỗi thao tác. Bất kỳ giải pháp nào cũng phải xử lý từng lần thử trong thời gian không đổi hoặc rất gần với thời gian đó. 

Chế độ lỗi tinh vi xuất hiện khi nhiều hàng xóm có thể phản ứng với một vị trí. Ví dụ, một ô có thể có hai con rắn liền kề hướng về phía nó. Việc chọn sai trong quy tắc hai sẽ phá vỡ tính đúng đắn vì điều kiện “được đặt mới nhất” là cần thiết và không thể xấp xỉ bằng thứ tự tùy ý. 

Một trường hợp thất bại khác xảy ra khi một con rắn được nâng cấp thành một con rắn lớn. Nó vẫn tham gia vào các tương tác trong tương lai, do đó, việc coi nó như bị loại bỏ hoặc bỏ qua hướng của nó sẽ dẫn đến hành vi xuôi dòng không chính xác. 

## Phương pháp tiếp cận 

Giải thích đơn giản mô phỏng từng vị trí bằng cách quét lưới hoặc tìm kiếm động các con rắn tương tác. Đối với mỗi vị trí, người ta có thể kiểm tra các ô xung quanh hoặc thậm chí tìm kiếm tất cả các con rắn có thể trỏ vào ô mục tiêu. Điều này hoạt động về mặt khái niệm vì các tương tác mang tính cục bộ, nhưng cách giải thích ngây thơ có xu hướng chuyển sang việc duy trì danh sách toàn cầu về các loài rắn và quét chúng cho mỗi truy vấn. 

Nút thắt cổ chai xuất hiện ngay lập tức trong quy tắc thứ hai. Nếu chúng ta duy trì một danh sách toàn cục gồm tất cả các loài rắn và lọc những con trỏ trỏ vào một ô thì mỗi truy vấn sẽ trở thành tuyến tính theo số lượng các loài rắn đang hoạt động. Với tối đa 200.000 vị trí, điều này biến thành hàng chục tỷ séc trong trường hợp xấu nhất, điều này là không khả thi. 

Quan sát quan trọng là cả hai quy tắc chỉ phụ thuộc vào bốn ô lân cận. Một con rắn chỉ có thể tương tác thông qua sự lân cận. Không có sự phụ thuộc lâu dài, không có phản ứng dây chuyền đòi hỏi phải tìm kiếm bên ngoài những nước láng giềng ngay lập tức vào thời điểm quyết định. Điều này thu gọn vấn đề thành các truy vấn cục bộ theo thời gian không đổi cho mỗi bản cập nhật. 

Mỗi ô chỉ cần biết liệu hiện tại nó có chứa một con rắn hay không và nếu có thì hướng của nó, dấu thời gian và liệu nó đã được nâng cấp hay chưa. Quy tắc một kiểm tra chính xác một hàng xóm. Quy tắc hai kiểm tra tối đa ba hàng xóm sau khi loại trừ hướng chuyển tiếp. Vì tính liền kề là cố định nên chúng ta không bao giờ cần các cấu trúc phụ trợ ngoài lưới. 

Điều này làm giảm vấn đề từ lựa chọn động toàn cầu đến kiểm tra nhà nước địa phương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét toàn cầu Brute Force | O(k × n × m) | O(n×m) | Quá chậm | 
| Mô phỏng hàng xóm địa phương | O(k) | O(n×m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì một lưới trong đó mỗi ô lưu trữ xem nó hiện có chứa một con rắn hay không, hướng của nó, thời gian chèn và liệu nó có được nâng cấp hay không. 

Mỗi lần thử vị trí được xử lý theo thứ tự thời gian. 

1. Nếu ô mục tiêu bị chặn hoặc đã chứa con rắn thì thao tác sẽ bị bỏ qua vì không có quy tắc tương tác nào áp dụng cho các vị trí không thành công. 
2. Nếu không, hãy tạm thời xem xét việc đặt một con rắn mới cùng với hướng và dấu thời gian của nó. 
3. Kiểm tra ô lân cận theo hướng con rắn mới đối mặt. Nếu ô đó tồn tại và chứa một con rắn thì sự tương tác sẽ tuân theo quy tắc một. Con rắn hiện tại được nâng cấp thành con rắn lớn và giữ nguyên phương hướng cũng như dấu thời gian. Con rắn mới đặt sẽ biến mất và không có cuộc kiểm tra nào được thực hiện nữa. 
4. Nếu quy tắc một không kích hoạt, hãy kiểm tra ba ô liền kề còn lại. Đối với mỗi hàng xóm, nếu nó chứa một con rắn có hướng quay ngược lại ô hiện tại thì đó là ứng cử viên cho quy tắc thứ hai. 
5. Trong số tất cả các ứng cử viên, hãy chọn con rắn có dấu thời gian lớn nhất, nghĩa là con rắn được đặt gần đây nhất. 
6. Nếu có ứng cử viên, hãy loại bỏ con rắn đó và biến con rắn mới được đặt thành một con rắn lớn. 
7. Nếu không có ứng cử viên nào tồn tại, con rắn mới vẫn là con rắn bình thường. 

Trạng thái lưới được cập nhật tương ứng sau khi giải quyết tương tác. 

Tính đúng đắn dựa trên tính bất biến rằng ảnh hưởng duy nhất của mỗi con rắn là thông qua bốn ô liền kề của nó. Không có sự phụ thuộc tiềm ẩn nào tồn tại ngoài các nước láng giềng trực tiếp, vì vậy việc duy trì toàn bộ lịch sử toàn cầu là không cần thiết. 

Mỗi ô luôn lưu trữ trạng thái hiện tại thực sự của con rắn đang chiếm giữ nó và dấu thời gian đảm bảo lựa chọn xác định cho quy tắc hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())
g = [input().split() for _ in range(n)]

# state: 0 empty, 1 small, 2 big
# dir stored as char, time stored as int
state = [[0] * m for _ in range(n)]
direc = [[''] * m for _ in range(n)]
tstamp = [[-1] * m for _ in range(n)]

dx = {'u': -1, 'd': 1, 'l': 0, 'r': 0}
dy = {'u': 0, 'd': 0, 'l': -1, 'r': 1}

rev = {'u': 'd', 'd': 'u', 'l': 'r', 'r': 'l'}

for t in range(k):
    x, y, c = input().split()
    x = int(x) - 1
    y = int(y) - 1

    if g[x][y] == '0' or state[x][y] != 0:
        continue

    nx = x + dx[c]
    ny = y + dy[c]

    removed = False

    # rule 1
    if 0 <= nx < n and 0 <= ny < m and state[nx][ny] != 0:
        state[nx][ny] = 2
        direc[nx][ny]() direc[nx][ny] or c
        # current snake disappears
        continue

    # rule 2 candidates
    best = (-1, -1, -1)  # time, px, py
    for d in "udlr":
        if d == c:
            continue
        px = x + dx[d]
        py = y + dy[d]
        if 0 <= px < n and 0 <=
```

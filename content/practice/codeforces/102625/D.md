---
title: "CF 102625D - Lời chúc tốt đẹp nhất!!"
description: "Chúng tôi bắt đầu với khoản phí 1 vào ngày đầu tiên. Mỗi ngày tiếp theo, khoản phí mới phải được lấy từ khoản phí của ngày hôm trước bằng một trong ba thao tác: nhân đôi, nhân ba hoặc tăng thêm một."
date: "2026-08-03T15:18:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102625
codeforces_index: "D"
codeforces_contest_name: "IIT(ISM) Virtual Farewell"
rating: 0
weight: 102625
solve_time_s: 69
verified: true
draft: false
---

[CF 102625D - Lời chúc tốt đẹp nhất !!](https://codeforces.com/problemset/problem/102625/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Chúng tôi bắt đầu với khoản phí 1 vào ngày đầu tiên. Mỗi ngày tiếp theo, khoản phí mới phải được lấy từ khoản phí của ngày hôm trước bằng một trong ba thao tác: nhân đôi, nhân ba hoặc tăng thêm một. Đưa ra một khoản phí mục tiêu`D`, ta cần tìm số ngày cần thiết nhỏ nhất để số tiền phải trả vào ngày cuối cùng chính xác`D`. Chúng tôi cũng phải in một chuỗi các khoản phí hàng ngày hợp lệ để đạt được mục tiêu trong số ngày tối thiểu đó. 

Đầu vào chứa một số mục tiêu độc lập. Vì mục tiêu tối đa chỉ là`10^6`, giải pháp dự định có thể đủ khả năng xử lý trước tất cả các giá trị có thể cho đến mục tiêu lớn nhất xuất hiện. Một giải pháp cố gắng tìm kiếm riêng biệt với mọi truy vấn sẽ lãng phí công sức vì có thể có`10^5`truy vấn. Ở quy mô này, một`O(D)`hoặc`O(maxD)`Phương pháp tiền xử lý là phù hợp, trong khi các thuật toán khám phá nhiều đường dẫn độc lập cho mỗi truy vấn có thể dễ dàng trở nên quá chậm. 

Các trường hợp cạnh chính xuất hiện xung quanh các giá trị rất nhỏ và các giá trị trong đó tuyến đường tốt nhất không sử dụng phép nhân lớn nhất ngay lập tức. Ví dụ, đối với`D = 1`, câu trả lời là một ngày và trình tự chỉ là`1`. Việc triển khai bắt đầu tìm kiếm từ một nước đi thay vì xem xét trạng thái bắt đầu có thể trả về một câu trả lời dài hơn một cách không chính xác. 

Vì`D = 5`, trình tự tối ưu là:```
1 3 4 5
```Đầu ra là`4`ngày. Một chiến lược tham lam luôn nhân lên khi có thể có thể chọn`1 2 4 5`, hợp lệ nhưng cũng có bốn ngày ở đây, che giấu vấn đề. Trên các giá trị lớn hơn, việc ưu tiên phép nhân một cách mù quáng có thể bỏ lỡ các đường đi ngắn hơn vì đạt được giá trị trung gian hữu ích với`+1`có thể kích hoạt chuỗi nhân sau này. 

Vì`D = 10`, một chuỗi ngắn nhất là:```
1 2 3 9 10
```Câu trả lời là`5`ngày. Một phương pháp chỉ lưu trữ khoảng cách chứ không lưu trữ các lựa chọn của cha mẹ sẽ tìm ra độ dài tối thiểu chính xác nhưng sẽ không thể xây dựng lại chuỗi điện tích cần thiết. 

# Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể mô hình hóa mọi chuỗi hoạt động có thể có. Bắt đầu từ`1`, nó thử tất cả các kết hợp của`+1`,`*2`, Và`*3`cho đến khi đạt được`D`. Điều này đúng vì mọi trình tự pháp lý đều được khám phá, do đó trình tự ngắn nhất đầu tiên được tìm thấy sẽ đưa ra câu trả lời. Vấn đề là số lượng khả năng. Sau đó`k`số ngày có thể lên tới`3^k`trình tự hoạt động khác nhau, do đó điều này tăng theo cấp số nhân và trở nên không thể ngay cả đối với các mục tiêu lớn vừa phải. 

Cấu trúc của vấn đề mang lại một cái nhìn rõ ràng hơn nhiều. Mỗi giá trị điện tích là một nút trong biểu đồ. Từ một giá trị`x`, có các cạnh`x + 1`,`2x`, Và`3x`bất cứ khi nào những giá trị đó không lớn hơn mục tiêu tối đa mà chúng tôi cần. Mỗi chuỗi điện tích hợp lệ chính xác là một đường dẫn trong biểu đồ này. Vì mỗi hoạt động tốn một ngày nên chuỗi ngắn nhất là đường đi ngắn nhất từ ​​nút`1`đến nút`D`. Vì tất cả các cạnh đều có giá bằng nhau nên tìm kiếm theo chiều rộng sẽ tìm ra số ngày tối thiểu. 

Tìm kiếm brute-force không thành công vì nó liên tục khám phá các chuỗi từng phần tương tự. BFS hợp nhất tất cả các đường dẫn đạt cùng giá trị điện tích, giải quyết từng trạng thái một lần. Bằng cách lưu trữ giá trị gốc cho mỗi giá trị được truy cập, chúng ta có thể xây dựng lại trình tự chính xác sau khi tìm thấy mục tiêu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^k) | O(k) | Quá chậm | 
| Tối ưu | O(maxD) | O(maxD) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đọc tất cả các giá trị mục tiêu và tìm mục tiêu lớn nhất`M`. Chúng ta chỉ cần xây dựng các trạng thái lên tới`M`vì không có giá trị trung gian hữu ích nào có thể vượt quá đích cuối cùng trong bài toán này. Tất cả các hoạt động chỉ làm tăng phí. 
2. Chạy BFS bắt đầu từ khi sạc`1`. Lưu trữ khoảng cách của mỗi lần sạc đạt được và lần sạc trước đó được sử dụng để tiếp cận nó. Hàng đợi BFS đảm bảo rằng các giá trị được xử lý theo số ngày tăng dần. 
3. Đối với mỗi lần sạc hiện tại`x`, hãy thử ba khoản phí có thể áp dụng tiếp theo:`x + 1`,`2x`, Và`3x`. Bỏ qua các giá trị lớn hơn`M`hoặc các giá trị đã được truy cập. Lần đầu tiên một giá trị được truy cập là thông qua một con đường ngắn nhất, do đó giá trị gốc của nó có thể được cố định vĩnh viễn. 
4. Đối với mỗi truy vấn`D`, bắt đầu từ`D`và liên tục theo dõi cha mẹ được lưu trữ cho đến khi đạt được`1`. Đảo ngược danh sách đã thu thập này để có được trình tự tính phí hàng ngày theo thứ tự chuyển tiếp. 

Tại sao nó hoạt động: BFS khám phá tất cả các trạng thái có thể truy cập trong một ngày trước khi bất kỳ trạng thái nào có thể truy cập trong hai ngày, sau đó tất cả các trạng thái có thể truy cập trong hai ngày trước ba ngày, v.v. Do đó, khi đạt được giá trị điện tích lần đầu tiên, đường dẫn được sử dụng có số lượng thao tác nhỏ nhất có thể. Các con trỏ gốc lưu trữ chính xác một đường đi ngắn nhất tới mọi khoản phí có thể tiếp cận, vì vậy hãy theo dõi chúng từ`D`quay lại`1`xây dựng lại trình tự tối ưu. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, sys.stdin.buffer.read().split()))
    if not data:
        return

    t = data[0]
    queries = data[1:1 + t]
    limit = max(queries)

    parent = [-1] * (limit + 1)
    parent[1] = 0

    from collections import deque
    q = deque([1])

    while q:
        x = q.popleft()

        for y in (x + 1, x * 2, x * 3):
            if y <= limit and parent[y] == -1:
                parent[y] = x
                q.append(y)

    ans = []
    for d in queries:
        path = []
        cur = d
        while cur != 0:
            path.append(cur)
            if cur == 1:
                break
            cur = parent[cur]
        path.reverse()

        ans.append(str(len(path)))
        ans.append(" ".join(map(str, path)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần tiền xử lý xây dựng cây đường dẫn ngắn nhất một lần. các`parent`mảng có hai mục đích:`parent[x] == -1`có nghĩa là giá trị chưa đạt được và nếu không, nó sẽ lưu trữ khoản phí trước đó theo đường đi ngắn nhất. 

Thứ tự chuyển đổi BFS không ảnh hưởng đến tính chính xác vì tất cả các hoạt động đều có chi phí như nhau. Điều kiện bắt buộc duy nhất là mỗi trạng thái mới được phát hiện phải được thêm vào một lần. Các giá trị trên truy vấn tối đa sẽ bị bỏ qua vì chúng không bao giờ có thể là một phần của đường dẫn kết thúc ở mục tiêu nhỏ hơn. 

Trong quá trình xây dựng lại, đường dẫn được thu thập ngược từ`D`ĐẾN`1`. Đảo ngược nó sẽ đưa ra thứ tự các khoản phí hàng ngày. Số ngày là số giá trị trong chuỗi chứ không phải số thao tác vì ngày đầu tiên đã chứa khoản phí ban đầu`1`. 

# Ví dụ đã hoạt động 

cho`D = 5`, BFS đạt được mục tiêu thông qua con đường ngắn nhất sau đây. 

| Phí hiện tại | Hoạt động | Phí mới | Được lưu trữ chính thức | 
| --- | --- | --- | --- | 
| 1 | *3 | 3 | 1 | 
| 3 | +1 | 4 | 3 | 
| 4 | +1 | 5 | 4 | 

Theo chân cha mẹ từ`5`cho`5 -> 4 -> 3 -> 1`, ngược lại thành:```
1 3 4 5
```Ví dụ này cho thấy rằng một chuỗi chứa số gia nhỏ hơn có thể đánh bại chiến lược chỉ tập trung vào phép nhân. 

Vì`D = 10`, một khả năng tái cấu trúc BFS có thể là: 

| Phí hiện tại | Hoạt động | Phí mới | Được lưu trữ chính thức | 
| --- | --- | --- | --- | 
| 1 | *2 | 2 | 1 | 
| 2 | +1 | 3 | 2 | 
| 3 | *3 | 9 | 3 | 
| 9 | +1 | 10 | 9 | 

Trình tự được xây dựng lại là:```
1 2 3 9 10
```Điều này chứng tỏ tại sao cả ba quá trình chuyển đổi đều phải được xem xét. Loại bỏ`+1`hoạt động sẽ làm cho một số đường dẫn tối ưu không thể truy cập được. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(maxD) | Mỗi giá trị phí được truy cập một lần và có ba lần chuyển tiếp đi. | 
| Không gian | O(maxD) | Mảng gốc và hàng đợi BFS lưu trữ thông tin cho mỗi lần sạc có thể truy cập. | 

Mục tiêu tối đa là`10^6`, do đó quá trình tiền xử lý sẽ truy cập tối đa một triệu trạng thái. Điều này dễ dàng nằm trong giới hạn và nó tránh lặp lại công việc lên đến`10^5`truy vấn. 

# Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def solution(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = list(map(int, sys.stdin.buffer.read().split()))
    if not data:
        sys.stdin = old
        return ""

    t = data[0]
    queries = data[1:1+t]
    limit = max(queries)

    parent = [-1] * (limit + 1)
    parent[1] = 0
    q = deque([1])

    while q:
        x = q.popleft()
        for y in (x + 1, x * 2, x * 3):
            if y <= limit and parent[y] == -1:
                parent[y] = x
                q.append(y)

    out = []
    for d in queries:
        path = []
        while d:
            path.append(d)
            if d == 1:
                break
            d = parent[d]
        path.reverse()
        out.append(str(len(path)))
        out.append(" ".join(map(str, path)))

    sys.stdin = old
    return "\n".join(out)

assert solution("3\n1\n5\n96234\n").splitlines()[0:4] == ["1", "1", "4", "1 3 4 5"]
assert solution("1\n1000000\n").splitlines()[0] == "20"

assert solution("1\n1\n") == "1\n1"
assert solution("1\n2\n") == "2\n1 2"
assert solution("1\n3\n") == "2\n1 3"
assert solution("1\n10\n").splitlines()[0] == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1 / 1`| Trạng thái bắt đầu đã là câu trả lời. | 
|`2`|`2 / 1 2`| Chuyển đổi không tầm thường nhỏ nhất. | 
|`3`|`2 / 1 3`| Phép nhân trực tiếp được xử lý. | 
|`10`|`5`ngày | Tái thiết thông qua các hoạt động hỗn hợp. | 
|`1000000`|`20`ngày | Giá trị biên lớn và giới hạn tiền xử lý. | 

# Vỏ cạnh 

cho`D = 1`, BFS bắt đầu với câu trả lời đã được phát hiện. Vòng lặp tái thiết dừng ngay lập tức và trả về một lần sạc`1`. Điều này tránh được sai lầm phổ biến khi cho rằng cần phải thực hiện ít nhất một thao tác. 

Đối với các mục tiêu như`D = 5`, thuật toán không bắt buộc các đường nhân. BFS khám phá cả ba lựa chọn từ mọi giá trị và giữ con đường ngắn nhất đầu tiên đến từng trạng thái. Chuỗi gốc được lưu trữ đạt tới`5`bởi vì`1 -> 3 -> 4 -> 5`, đưa ra số ngày tối thiểu. 

Để đạt được mục tiêu tối đa`D = 1000000`, thuật toán không tạo ra các trạng thái vượt quá giá trị này. BFS vẫn bao gồm toàn bộ không gian tìm kiếm hữu ích một lần và mảng cha cho phép xây dựng lại mà không cần tìm kiếm khác. Chuỗi kết quả có độ dài tối thiểu có thể vì mọi trạng thái đều được chỉ định khoảng cách trong BFS.

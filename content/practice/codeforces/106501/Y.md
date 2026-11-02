---
title: "CF 106501Y - Mua sắm tạp hóa"
description: "Bài toán yêu cầu chúng ta lên lịch ghé thăm từng cửa hàng càng nhanh càng tốt. Chỉ có một số ít cửa hàng nhưng mỗi cửa hàng có thể có nhiều khách hàng khác đến theo thời gian."
date: "2026-06-25T08:34:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106501
codeforces_index: "Y"
codeforces_contest_name: "IPL 2026"
rating: 0
weight: 106501
solve_time_s: 37
verified: true
draft: false
---

[CF 106501Y - Mua sắm hàng tạp hóa](https://codeforces.com/problemset/problem/106501/Y) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta lên lịch ghé thăm từng cửa hàng càng nhanh càng tốt. Chỉ có một số ít cửa hàng nhưng mỗi cửa hàng có thể có nhiều khách hàng khác đến theo thời gian. Auchenai chọn thứ tự các cửa hàng và khi đến một cửa hàng, anh ta có thể phải đợi những khách hàng đã xếp hàng sẵn. Mỗi cửa hàng cần một khoảng thời gian cố định để phục vụ bất kỳ khách hàng nào, kể cả Auchenai. 

Dữ liệu đầu vào cung cấp số lượng cửa hàng, số lượng khách hàng bên ngoài, thời gian mua sắm của mỗi cửa hàng và thời gian đến của mọi khách hàng bên ngoài cùng với cửa hàng họ ghé thăm. Đầu ra là thời điểm sớm nhất có thể khi Auchenai hoàn thành tất cả các cửa hàng. 

Số lượng cửa hàng nhiều nhất là 18. Giá trị nhỏ này là đầu mối chính. Việc thử mọi thứ tự có thể yêu cầu`18!`những khả năng vốn đã quá lớn. Giải pháp quy hoạch động tập hợp con là thực tế vì chỉ có`2^18 = 262144`tập hợp con. Số lượng khách hàng bên ngoài có thể tiếp cận`200000`, do đó việc mô phỏng hàng đợi cho mỗi lần chuyển đổi sẽ quá chậm. Chúng ta cần xử lý trước hành vi xếp hàng của từng cửa hàng. 

Các giá trị lớn của thời gian, lên đến`10^9`, có nghĩa là việc triển khai phải sử dụng số nguyên 64 bit. Số nguyên Python đã xử lý việc này một cách an toàn. 

Một số trường hợp rất dễ xử lý sai. Nếu Auchenai đến đúng lúc một khách hàng khác đến thì khách hàng đó phải đi trước anh ta. Ví dụ:```
Input
1 1
5
1 0
```Đầu ra đúng là`10`. Việc thực hiện bất cẩn chỉ tính những khách hàng có thời gian đến nhỏ hơn thời gian đến của Auchenai sẽ khiến Auchenai bắt đầu ngay lập tức và sản xuất`5`. 

Một tình huống khó khăn khác là khi không có khách hàng chờ đợi. Ví dụ:```
Input
1 0
5
```Câu trả lời là`5`. Giải pháp luôn tra cứu thời gian hoàn thành hàng đợi trước đó có thể vô tình tạo thêm độ trễ không cần thiết. 

Trường hợp cuối cùng là khi khách hàng đã đến từ lâu nhưng hàng đợi của họ đã trống. Ví dụ: nếu một cửa hàng lấy 3 đơn vị và một khách hàng đến lúc 1, thì đến lúc 10 sẽ kết thúc vào lúc 13 chứ không phải 14. Lịch sử hàng đợi quan trọng, nhưng chỉ cho đến thời điểm Auchenai đến. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi đơn đặt hàng có thể có của các cửa hàng ghé thăm. Đối với một đơn hàng cố định, chúng ta có thể mô phỏng hàng đợi và tính toán thời gian hoàn thành cuối cùng. Điều này đúng vì mọi lịch trình có thể đều được xem xét. Vấn đề là số lượng lịch trình. Với 18 cửa hàng có`18!`mệnh lệnh, đó là về`6.4 * 10^15`, làm cho vũ lực không thể thực hiện được. 

Cải tiến đầu tiên là nhận ra rằng tương lai không phụ thuộc vào thứ tự chính xác được sử dụng để đạt đến tình huống hiện tại. Sau khi ghé thăm một số cửa hàng, thông tin duy nhất quan trọng là thời gian hiện tại và những cửa hàng nào còn lại. Điều này mang lại trạng thái lập trình động tập hợp con tự nhiên. 

Đối với một tập hợp con các cửa hàng đã ghé thăm, chúng tôi lưu trữ thời gian sớm nhất Auchenai có thể hoàn thành tất cả các cửa hàng trong tập hợp con đó. Từ trạng thái đó, chúng tôi thử thêm mọi cửa hàng chưa ghé thăm. 

Thử thách còn lại là trả lời câu hỏi này một cách nhanh chóng: nếu Auchenai đến cửa hàng`i`vào thời điểm đó`x`, khi nào anh ấy kết thúc? Khách hàng bên ngoài cửa hàng luôn đến theo trình tự thời gian và họ không bao giờ phụ thuộc vào quyết định của Auchenai. Đối với mỗi cửa hàng, chúng tôi xử lý trước thời gian kết thúc của hàng đợi bên ngoài sau lần đầu tiên`k`khách hàng. Sau đó tìm kiếm nhị phân cho chúng ta biết có bao nhiêu khách hàng đã đến theo thời gian`x`, và chúng ta biết ngay thời điểm hàng đợi của họ được dọn sạch. 

Brute-force hoạt động vì mọi đơn hàng có thể đều được kiểm tra, nhưng thất bại vì số lượng đơn hàng bùng nổ. Nhận xét rằng chỉ có tập truy cập và thời gian kết thúc sớm nhất mới là quan trọng làm giảm vấn đề đối với tập con DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n! * n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(2^n * n * log m)`|`O(2^n + m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp thời gian đến của khách hàng đối với từng cửa hàng. Đối với mỗi cửa hàng, hãy xây dựng một mảng tiền tố trong đó`k`-giá trị thứ đại diện cho thời điểm đầu tiên`k`khách hàng bên ngoài đã hoàn toàn mua sắm xong. 

Các giá trị tiền tố cho phép chúng ta bỏ qua mô phỏng hàng đợi riêng lẻ sau này. Mỗi lần chuyển đổi trong DP chỉ cần biết có bao nhiêu khách hàng đã đến. 
2. Tạo mảng lập trình động`dp[mask]`. Một chút trong`mask`đại diện cho một cửa hàng mà Auchenai đã ghé thăm. Giá trị được lưu trữ là thời điểm hoàn thành sớm nhất cho chính xác những cửa hàng đó. 

Trạng thái ban đầu là`dp[0] = 0`bởi vì trước khi ghé thăm bất kỳ cửa hàng nào, thời gian hiện tại bằng không. 
3. Lặp lại qua từng tập hợp con`mask`. Nếu có thể truy cập trạng thái, hãy thử mọi cửa hàng không có mặt nạ. 

Việc thêm một cửa hàng tương ứng với việc chọn điểm đến tiếp theo trong lịch trình. 
4. Đối với cửa hàng tiếp theo được chọn`i`, đặt thời gian hiện tại là`cur = dp[mask]`. Sử dụng tìm kiếm nhị phân trên cửa hàng`i`danh sách khách đến để tìm số lượng khách hàng bên ngoài có thời gian đến nhiều nhất`cur`. 

Khách hàng đến đúng lúc`cur`phải được đưa vào vì họ vào hàng trước Auchenai. 
5. Sử dụng thời gian hoàn thành hàng đợi được tính toán trước cho những khách hàng đó. Auchenai bắt đầu sau khi cả thời gian đến và hàng đợi hiện tại đã kết thúc, sau đó dành`t[i]`đơn vị mua sắm. 
6. Cập nhật trạng thái chứa cửa hàng mới truy cập. Sau khi tất cả các tập hợp con được xử lý, câu trả lời là`dp[(1 << n) - 1]`. 

Tại sao nó hoạt động: 

Tính bất biến đó là`dp[mask]`luôn lưu trữ thời gian hoàn thành tốt nhất có thể trong số tất cả các đơn hàng ghé thăm chính xác các cửa hàng ở`mask`. Khi chúng tôi mở rộng một trạng thái bằng cách ghé thăm một cửa hàng khác, chúng tôi tính toán thời gian hoàn thành chính xác cho lựa chọn đó vì hàng đợi của cửa hàng chỉ phụ thuộc vào thời gian hiện tại và số lượng khách đến bên ngoài cố định. Vì mọi cửa hàng tiếp theo có thể đều được xem xét nên mọi đơn hàng hoàn chỉnh có thể xuất hiện dưới dạng một chuỗi chuyển đổi DP. Do đó, giá trị tối thiểu cho mặt nạ đầy đủ là lịch trình tối ưu. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    t = list(map(int, input().split()))

    arrivals = [[] for _ in range(n)]
    for _ in range(m):
        s, u = map(int, input().split())
        arrivals[s - 1].append(u)

    prefix = []
    for i in range(n):
        arrivals[i].sort()
        cur = 0
        pref = [0]
        for x in arrivals[i]:
            cur = max(cur, x) + t[i]
            pref.append(cur)
        prefix.append(pref)

    size = 1 << n
    inf = 10**30
    dp = [inf] * size
    dp[0] = 0

    for mask in range(size):
        cur = dp[mask]
        if cur == inf:
            continue

        for i in range(n):
            if (mask >> i) & 1:
                continue

            cnt = bisect_right(arrivals[i], cur)
            finish_queue = prefix[i][cnt]
            nxt_time = max(cur, finish_queue) + t[i]

            nxt = mask | (1 << i)
            if nxt_time < dp[nxt]:
                dp[nxt] = nxt_time

    print(dp[-1])

if __name__ == "__main__":
    solve()
```Phần tiền xử lý chuyển đổi hàng đợi bên ngoài của mỗi cửa hàng thành thời gian hoàn thành tiền tố. Giá trị tại chỉ mục`k`có nghĩa là sau khi phục vụ lần đầu tiên`k`khách hàng, lúc đó quán sẽ rảnh. 

Phần DP tuân theo quá trình chuyển đổi tập hợp con được mô tả ở trên. Mặt nạ được lưu trữ dưới dạng số nguyên, giúp giữ cho biểu diễn trạng thái nhỏ gọn. Vì chỉ có`2^18`nêu rõ, việc lặp lại trên tất cả các mặt nạ là khả thi. 

Việc tìm kiếm nhị phân sử dụng`bisect_right`còn hơn là`bisect_left`vì khách đến đúng giờ Auchenai vẫn xếp hàng trước. biểu hiện`max(cur, finish_queue)`xử lý cả hai tình huống: hàng đợi đã trống hoặc Auchenai phải đợi. 

Không cần xử lý tràn vì số nguyên Python tự động tăng lên. Giá trị trọng điểm lớn chỉ được sử dụng để biểu thị các trạng thái không thể truy cập được. 

## Ví dụ đã hoạt động 

Mẫu 1:```
2 2
2 3
1 1
2 2
```Các trạng thái quan trọng là: 

| Mặt nạ | Đã ghé thăm cửa hàng | Thời gian sớm nhất hiện tại | 
| --- | --- | --- | 
| 00 | không | 0 | 
| 01 | cửa hàng 1 | 3 | 
| 10 | cửa hàng 2 | 5 | 
| 11 | cả hai | 5 | 

Đến cửa hàng 1 trước tiên lấy 3 chiếc vì khách hàng bên ngoài lúc 1 kết thúc lúc 3. Sau đó cửa hàng 2 đã có khách ra về nên Auchenai kết thúc lúc 5. 

Mẫu 2:```
2 2
2 3
1 3
2 2
```| Mặt nạ | Đã ghé thăm cửa hàng | Thời gian sớm nhất hiện tại | 
| --- | --- | --- | 
| 00 | không | 0 | 
| 01 | cửa hàng 1 | 5 | 
| 10 | cửa hàng 2 | 5 | 
| 11 | cả hai | 7 | 

Khách hàng đến lúc 3 khối Auchenai nếu anh ta đến cửa hàng 1 vào lúc 3. Trường hợp bình đẳng buộc phải có thêm thời gian chờ đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(2^n * n * log m)`| Mỗi tập hợp con thử mọi cửa hàng tiếp theo và mỗi lần chuyển đổi sẽ thực hiện một tìm kiếm nhị phân trong danh sách đến của cửa hàng. | 
| Không gian |`O(2^n + m)`| Mảng DP lưu trữ tất cả các trạng thái tập hợp con và tiền xử lý lưu trữ tất cả các lượt khách hàng đến. | 

Số lượng trạng thái lớn nhất là`262144`, và số lần chuyển đổi chỉ có vài triệu. Quá trình tiền xử lý xử lý số lượng khách hàng lớn một lần, do đó giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else ""
    sys.stdin = old_stdin
    return out

# This section is illustrative because solve() prints directly.
# Use a StringIO replacement for stdout in a local judge harness.

tests = [
    (
        "1 0\n5\n",
        "5\n",
        "single shop without customers"
    ),
    (
        "1 1\n5\n1 0\n",
        "10\n",
        "customer arriving exactly at start"
    ),
    (
        "2 2\n2 3\n1 1\n2 2\n",
        "5\n",
        "provided sample 1"
    ),
    (
        "2 2\n2 3\n1 3\n2 2\n",
        "7\n",
        "provided sample 2"
    ),
]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một cửa hàng, không có khách hàng |`5`| Xử lý hàng đợi trống | 
| Một cửa hàng đến vào thời điểm 0 |`10`| Đặt hàng đến đồng thời | 
| Mẫu 1 |`5`| Chuyển đổi tập hợp con cơ bản | 
| Mẫu 2 |`7`| Chờ đợi do đến đồng thời | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên, hãy xem xét:```
1 1
5
1 0
```DP bắt đầu tại thời điểm 0. Tìm kiếm nhị phân tìm thấy một khách hàng đã có trong hàng đợi vì thời gian đến bằng thời gian hiện tại. Việc hoàn thành hàng đợi tiền tố là 5, vì vậy Auchenai bắt đầu ở thời điểm 5 và kết thúc ở thời điểm 10. 

Đối với hàng đợi trống:```
1 0
5
```Tìm kiếm nhị phân trả về không có khách hàng nào. Việc hoàn thành hàng đợi được lưu trữ bằng 0, do đó quá trình chuyển đổi trở thành`max(0, 0) + 5`, cho câu trả lời đúng là 5. 

Đối với một khách hàng cũ đã rời đi:```
1 1
3
1 0
```Nếu Auchenai có thể đến cửa hàng vào thời điểm 10, tìm kiếm nhị phân sẽ bao gồm khách hàng, nhưng việc hoàn thành tiền tố sẽ là 3. Việc chuyển đổi sử dụng`max(10, 3) + 3`, cho 13. Lịch sử hàng đợi cũ không tạo ra độ trễ sau khi nó bị xóa.

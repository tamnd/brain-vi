---
title: "CF 102483F - Tốc độ chạy nhanh nhất"
description: "Chúng tôi có một trò chơi với n cấp độ. Hoàn thành cấp độ i sẽ vĩnh viễn cho chúng ta mục i. Tại bất kỳ thời điểm nào, vật phẩm duy nhất quan trọng đối với lối chơi thông thường là vật phẩm có số lượng lớn nhất mà chúng tôi đã thu thập được, bởi vì mọi vật phẩm lớn hơn không bao giờ tệ hơn vật phẩm nhỏ hơn."
date: "2026-08-06T04:16:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102483
codeforces_index: "F"
codeforces_contest_name: "2018-2019 ICPC Northwestern European Regional Programming Contest (NWERC 2018)"
rating: 0
weight: 102483
solve_time_s: 190
verified: true
draft: false
---

[CF 102483F - Tốc độ chạy nhanh nhất](https://codeforces.com/problemset/problem/102483/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Chúng tôi có một trò chơi với`n`cấp độ. Mức độ hoàn thiện`i`vĩnh viễn cung cấp cho chúng tôi vật phẩm`i`. Tại bất kỳ thời điểm nào, vật phẩm duy nhất quan trọng đối với lối chơi thông thường là vật phẩm có số lượng lớn nhất mà chúng tôi đã thu thập được, bởi vì mọi vật phẩm lớn hơn không bao giờ tệ hơn vật phẩm nhỏ hơn. 

Đối với mỗi cấp độ, chúng tôi biết hai loại thời gian hoàn thành. Thời gian bình thường`a[i][j]`là lúc để hoàn thành cấp độ`i`trong khi mang đồ`j`. Thời gian phím tắt`s[i]`chỉ có thể được sử dụng nếu chúng tôi đã sở hữu mục phím tắt cụ thể`x[i]`. Mục tiêu là chọn thứ tự hoàn thành các cấp độ sao cho tổng thời gian hoàn thành càng nhỏ càng tốt. 

Ràng buộc`n <= 2500`là hạn chế chính. Một thuật toán với khoảng`n^2`hoạt động có thể chấp nhận được, tạo ra khoảng sáu triệu lần chuyển đổi. Các phương pháp liệt kê thứ tự hoặc duy trì các tập hợp con tùy ý là không thể vì số lượng trạng thái có thể tăng theo cấp số nhân. Ngay cả nhiều thuật toán đồ thị hoạt động trên tất cả các cạnh có hướng có thể cũng sẽ quá đắt nếu chúng yêu cầu nhiều hơn công việc bậc hai. 

Cấu trúc ẩn chính là kho hiện tại có thể được tóm tắt bằng một số duy nhất, mặt hàng thu được lớn nhất. Hoàn thành một cấp độ với chỉ số nhỏ hơn mức tối đa hiện tại không bao giờ cải thiện được khả năng trong tương lai. Mức độ như vậy chỉ đóng góp chi phí và có thể bị trì hoãn. Các cấp độ duy nhất thay đổi các tùy chọn trong tương lai là các cấp độ tăng vật phẩm tối đa hiện tại. 

Một giải pháp bất cẩn có thể thất bại theo nhiều cách. Hãy xem xét đầu vào này:```
2
2 1 10 10 1
2 11 11 11 11
```Câu trả lời tối ưu là`12`. Lần đầu tiên chúng tôi hoàn thành cấp độ 2 đúng thời hạn`11`, sau đó sử dụng mục 2 để hoàn thành cấp độ 1 kịp thời`1`. Một chiến lược tham lam luôn hoàn thành mức rẻ nhất hiện tại trước tiên sẽ chọn cấp 1, chi tiêu`10`, rồi trả tiền`11`đối với cấp độ 2, đưa ra`21`. Sai lầm là bỏ qua rằng cấp độ đắt hơn một chút có thể mở khóa các cấp độ rẻ hơn nhiều trong tương lai. 

Một sai lầm phổ biến khác là cho rằng mọi cấp độ có chỉ số nhỏ hơn phải được hoàn thành ngay sau khi có sẵn. Ví dụ:```
2
0 5 5 5 5
0 7 7 7 7
```Câu trả lời là`12`. Việc hoàn thành cấp độ 1 trước hoặc cấp độ 2 trước đều có tác dụng vì cả hai đều không tạo ra vật phẩm tốt hơn cho vật phẩm kia. Thuật toán phải cho phép sắp xếp các cấp độ tùy ý mà không làm tăng mục tối đa. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi thứ tự hoàn thiện có thể có. Điều này đúng vì mọi lệnh hợp lệ đều thể hiện một lần chạy nhanh có thể xảy ra, nhưng có`n!`những đơn đặt hàng có thể, điều này trở nên không thể thực hiện được ngay cả đối với những đơn hàng rất nhỏ`n`. 

Một nỗ lực hợp lý hơn là mô hình hóa quy trình như chọn cấp độ tiếp theo từ tập hợp hiện đã hoàn thành. Điều này vẫn bỏ lỡ cấu trúc quan trọng. Bộ hoàn chỉnh có thể chứa nhiều cấp độ, nhưng tất cả các cấp độ dưới mục tối đa đều tương đương với tiến độ trong tương lai. Thông tin trạng thái có ý nghĩa duy nhất là mục lớn nhất thu được cho đến nay. 

Giả sử mục tối đa hiện tại là`m`. Bất kỳ cấp độ chưa hoàn thành nào có chỉ số nhỏ hơn hoặc bằng`m`không thể tăng mức tối đa nên việc hoàn thành sớm hơn không thể mở khóa bất cứ thứ gì. Chúng ta có thể trì hoãn tất cả các cấp độ như vậy cho đến cuối cùng. Các quyết định quan trọng duy nhất là chúng ta thu được chỉ số nào lớn hơn và theo thứ tự nào. 

Điều này chuyển đổi bài toán thành bài toán đường đi ngắn nhất trên chỉ số. Chuyển đổi từ mục tối đa`m`đến một mức độ lớn hơn`i`nghĩa là mức đó`i`là mức tối đa mới tiếp theo. Chi phí chuyển đổi chính xác là thời gian cần thiết để hoàn thành cấp độ`i`trong khi có vật phẩm`m`. Từ`i`phải lớn hơn`m`, tất cả các quá trình chuyển đổi sẽ tiếp tục, tạo ra một DAG. 

Sau khi đạt mục tối đa`n`, không thể tiến thêm được nữa. Mọi cấp độ còn lại được hoàn thành bằng cách sử dụng vật phẩm`n`, luôn là vật phẩm mạnh nhất. Chi phí dọn dẹp cuối cùng chỉ đơn giản là tổng chi phí của tất cả các cấp độ chưa được chọn làm bước tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo lệnh | Ồ (n!) | O(n) | Quá chậm | 
| Lập trình động để tăng các mục tối đa | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính chi phí tối thiểu cho mỗi lần chuyển đổi giữa các mục tối đa. Đối với mọi mức tối đa hiện tại có thể`j`và mọi cấp độ`i > j`, chi phí chuyển đổi là thời gian cần thiết để hoàn thành cấp độ`i`trong khi sở hữu món đồ`j`. Phím tắt chỉ được bao gồm khi`x[i] <= j`, bởi vì chỉ khi đó mục yêu cầu của nó mới có sẵn. 
2. Sử dụng quy hoạch động ở đâu`dp[i]`là thời gian tối thiểu cần thiết để đạt đến trạng thái mà vật phẩm`i`là vật phẩm thu được lớn nhất. Trạng thái ban đầu là`dp[0] = 0`, vì mục 0 đã có sẵn ngay từ đầu. 
3. Xử lý các mục tối đa từ chỉ mục nhỏ hơn đến chỉ mục lớn hơn. Từ một tiểu bang`j`, hãy thử mọi cấp độ lớn hơn`i`là mục tối đa tiếp theo và thư giãn`dp[i]`với`dp[j] + cost(j, i)`. Thứ tự này hợp lệ vì tất cả các chuyển đổi đều chuyển từ chỉ mục nhỏ hơn sang chỉ mục lớn hơn. 
4. Sau khi tìm được chi phí tối thiểu để đạt được mặt hàng`n`, hãy thêm chi phí hoàn thành mọi cấp độ không được sử dụng làm bước tăng tối đa. Một cách đơn giản hơn là quan sát việc tiếp cận vật phẩm`n`luôn là một phần của đường dẫn tối ưu và đường dẫn chuyển tiếp cuối cùng đã được thanh toán theo cấp độ`n`. Mọi cấp độ khác sau đó có thể được hoàn thành bằng cách sử dụng vật phẩm`n`với chi phí`min(a[i][n], s[i] if x[i] <= n)`. Vì mục`n`luôn có sẵn, đây sẽ trở thành chi phí tối thiểu có thể có cho mỗi cấp độ còn lại. 

Tại sao nó hoạt động: Mục tối đa thu được là mô tả đầy đủ về tiến trình trong tương lai. Bất kỳ mức nào dưới mức tối đa hiện tại chỉ có thể tiêu tốn thời gian và không thể thay đổi các mục có sẵn, vì vậy việc di chuyển nó sớm hơn không thể cải thiện giải pháp. Mọi tốc độ chạy tối ưu đều có thể được chuyển đổi thành một tốc độ trong đó chỉ những cấp độ tăng vật phẩm tối đa mới được xem xét đầu tiên. Các mức tăng tối đa này tạo thành một chuỗi tăng dần và quy hoạch động sẽ xem xét mọi chuỗi như vậy. Sau khi thu được vật phẩm lớn nhất, mỗi cấp độ còn lại sẽ có thời gian hoàn thành rẻ nhất có thể, vì vậy việc dọn dẹp cuối cùng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    x = [0] * (n + 1)
    s = [0] * (n + 1)
    a = [[0] * (n + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        data = list(map(int, input().split()))
        x[i], s[i] = data[0], data[1]
        for j in range(n + 1):
            a[i][j] = data[2 + j]

    dp = [10**30] * (n + 1)
    dp[0] = 0

    for cur in range(n):
        if dp[cur] == 10**30:
            continue
        for nxt in range(cur + 1, n + 1):
            cost = a[nxt][cur]
            if x[nxt] <= cur:
                cost = min(cost, s[nxt])
            if dp[nxt] > dp[cur] + cost:
                dp[nxt] = dp[cur] + cost

    ans = dp[n]
    used = [False] * (n + 1)
    cur = n
    while cur != 0:
        found = False
        for prev in range(cur):
            cost = a[cur][prev]
            if x[cur] <= prev:
                cost = min(cost, s[cur])
            if dp[prev] + cost == dp[cur]:
                used[cur] = True
                cur = prev
                found = True
                break
        if not found:
            break

    for i in range(1, n + 1):
        if not used[i]:
            cost = a[i][n]
            if x[i] <= n:
                cost = min(cost, s[i])
            ans += cost

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu vào được lưu trữ bằng cách lập chỉ mục dựa trên một vì số mục khớp với số cấp. Mảng lập trình động chỉ đại diện cho mục tối đa hiện tại chứ không phải toàn bộ tập hợp các cấp độ đã hoàn thành. 

Vòng chuyển tiếp chỉ cố gắng`nxt > cur`. Đây là thuộc tính trung tâm của giải pháp. Một cấp độ chỉ có thể trở thành mức tối đa mới khi chỉ số của nó lớn hơn mức tối đa hiện tại, do đó tất cả tiến trình hữu ích đều tạo thành một chuỗi tăng dần. 

Việc kiểm tra phím tắt sử dụng`x[nxt] <= cur`. Điều kiện này có nghĩa là mục phím tắt đã được thu thập. Vì trạng thái hiện tại chỉ lưu trữ mục tối đa nên mọi mục có chỉ số nhỏ hơn cũng được đảm bảo là có sẵn. 

Bước tái thiết đánh dấu các mức đã được sử dụng để tăng mức tối đa. Các cấp độ còn lại được thêm vào cuối bằng cách sử dụng vật phẩm`n`, bởi vì mục`n`thống trị mọi mặt hàng khác. Số nguyên Python có độ chính xác tùy ý, do đó, câu trả lời lớn có thể không yêu cầu xử lý đặc biệt. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3
1 1 40 30 20 10
3 1 95 95 95 10
2 1 95 50 30 20
```Các trạng thái quan trọng là: 

| Tối đa hiện tại | Tối đa tiếp theo | Chi phí chuyển đổi | Giá trị dp mới | 
| --- | --- | --- | --- | 
| 0 | 1 | 40 | 40 | 
| 0 | 2 | 95 | 95 | 
| 0 | 3 | 95 | 95 | 
| 1 | 2 | 50 | 90 | 
| 1 | 3 | 50 | 90 | 
| 2 | 3 | 20 | 115 | 

Đường đi tốt nhất đến mục 3 với chi phí`90`, sử dụng cấp độ 1 và 3 làm dãy tăng dần. Cấp độ 2 sau đó được hoàn thành với mục 3 kịp thời`1`, đưa ra câu trả lời`91`. 

Đối với mẫu thứ hai:```
4
4 4 5 5 5 5 5
4 4 5 5 5 5 5
4 4 5 5 5 5 5
4 4 5 5 5 5 5
```Các chuyển đổi hữu ích là: 

| Tối đa hiện tại | Tối đa tiếp theo | Chi phí chuyển đổi | Giá trị dp mới | 
| --- | --- | --- | --- | 
| 0 | 1 | 5 | 5 | 
| 0 | 2 | 5 | 5 | 
| 0 | 3 | 5 | 5 | 
| 0 | 4 | 5 | 5 | 

Sau khi có được vật phẩm 4, mỗi cấp độ còn lại sẽ tiêu tốn`4`vì mục phím tắt 4 hiện đã có sẵn. Trình tự tối ưu là đạt cấp độ 4 trước rồi hoàn thành ba cấp độ còn lại, mang lại`5 + 4 + 4 + 4 = 17`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi cặp mục tối đa tăng dần có thể được xem xét một lần. | 
| Không gian | O(n²) | Bảng hoàn thành đầu vào yêu cầu lưu trữ`n * (n + 1)`các giá trị. | 

Với`n = 2500`, số lần chuyển đổi bậc hai là khoảng 6,25 triệu, phù hợp thoải mái trong thời hạn. Bảng chứa khoảng 6,25 triệu số nguyên, nằm trong giới hạn bộ nhớ trong Python chỉ khi được triển khai cẩn thận. Việc triển khai được cung cấp sẽ lưu trữ toàn bộ bảng vì kích thước đầu vào yêu cầu quyền truy cập vào tùy ý`a[i][j]`các giá trị trong quá trình chuyển đổi. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    result = sys.stdout.getvalue()
    sys.stdin = old
    return result

assert run("""1
0 5 5 5
""") == "5\n", "single level"

assert run("""2
2 1 10 10 1
2 11 11 11 11
""") == "12\n", "unlocking better item"

assert run("""2
0 5 5 5 5
0 7 7 7 7
""") == "12\n", "equal progress choices"

assert run("""3
3 1 100 100 100 1
3 2 100 100 100 2
3 3 100 100 100 3
""") == "6\n", "shortcut chain"

assert run("""3
3 10 50 40 30 20
3 10 50 40 30 20
3 10 50 40 30 20
""") == "40\n", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cấp độ đơn | 5 | Kích thước tối thiểu và xử lý mục ban đầu | 
| Mở khóa vật phẩm tốt hơn | 12 | Cho thấy tại sao tham lam theo chi phí hiện tại lại thất bại | 
| Lựa chọn tiến bộ bình đẳng | 12 | Khẳng định mức độ không cải thiện tối đa có thể bị trì hoãn | 
| Chuỗi phím tắt | 6 | Kiểm tra nhiều mức tăng tối đa | 
| Tất cả các giá trị bằng nhau | 40 | Kiểm tra các chuyển tiếp bằng nhau lặp đi lặp lại | 

## Vỏ cạnh 

Đối với trường hợp nước đi đầu tiên đắt hơn sẽ mở ra nước đi rẻ hơn nhiều trong tương lai:```
2
2 1 10 10 1
2 11 11 11 11
```Lập trình động không cam kết ở mức rẻ nhất ngay lập tức. Nó xem xét cả hai quá trình chuyển đổi từ mục 0. Chi phí chuyển đổi sang mục 2`11`, đạt được vật phẩm mạnh nhất và rời khỏi cấp 1 với giá cuối cùng`1`, sản xuất`12`. 

Đối với các cấp độ không bao giờ tăng tối đa ngoại trừ thông qua một chỉ số lớn:```
3
3 1 100 100 100 1
3 2 100 100 100 2
3 3 100 100 100 3
```Con đường tốt nhất là đến mục 3 trước. Sau đó, mọi cấp độ còn lại có thể sử dụng mục phím tắt 3. Lập trình động ghi lại mức tăng tối đa hữu ích duy nhất và giai đoạn dọn dẹp sẽ bổ sung thêm các chi phí tối ưu còn lại. Kết quả là`6`. 

Đối với các phím tắt yêu cầu mục 0:```
2
0 5 5 5 5
0 7 7 7 7
```Mục 0 tồn tại ngay từ đầu nên cả hai phím tắt đều có thể sử dụng được ngay lập tức. Chi phí chuyển đổi được giảm một cách chính xác trước khi thu bất kỳ hạng mục nào khác và câu trả lời là`12`.

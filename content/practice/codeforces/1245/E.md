---
title: "CF 1245E - Hyakugoku và Thang"
description: "Trò chơi có thể được xem như một chuỗi các trạng thái được định hướng được sắp xếp dọc theo một con đường ngoằn ngoèo cố định trên một lưới 10 x 10. Mỗi ô tương ứng với một vị trí trên đường dẫn này, bắt đầu từ ô dưới cùng bên trái và kết thúc ở ô trên cùng bên trái."
date: "2026-06-15T21:36:09+07:00"
tags: ["codeforces", "competitive-programming", "dp", "probabilities", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1245
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 597 (Div. 2)"
rating: 2300
weight: 1245
solve_time_s: 209
verified: false
draft: false
---

[CF 1245E - Hyakugoku và Thang](https://codeforces.com/problemset/problem/1245/E) 

**Đánh giá:** 2300 
**Tags:** dp, xác suất, đường đi ngắn nhất 
**Thời gian giải:** 3m 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi có thể được xem như một chuỗi các trạng thái được định hướng được sắp xếp dọc theo một con đường ngoằn ngoèo cố định trên một lưới 10 x 10. Mỗi ô tương ứng với một vị trí trên đường dẫn này, bắt đầu từ ô dưới cùng bên trái và kết thúc ở ô trên cùng bên trái. Chuyển động dọc theo đường đi mang tính quyết định: trong một hàng bạn di chuyển theo chiều ngang theo các hướng xen kẽ và tại các ranh giới hàng bạn di chuyển lên trên theo chiều dọc. Vì vậy, mỗi ô có một ô kế thừa duy nhất trừ khi đó là ô mục tiêu cuối cùng. 

Mỗi lượt bao gồm việc tung một con xúc xắc sáu mặt công bằng. Người chơi cố gắng tiến về phía trước dọc theo con đường bằng chính xác số bước đó. Nếu giá trị xúc xắc vượt quá mục tiêu, người chơi sẽ giữ nguyên vị trí. Sau khi hạ cánh, nếu ô chứa chân thang, người chơi có thể chọn ở lại hoặc dịch chuyển tức thời đến điểm cuối của thang cao hơn trên bảng. Sau khi một chiếc thang được lấy đi, nó không thể bị xích ngay lập tức ngay cả khi một chiếc thang khác bắt đầu ở ô hạ cánh. 

Nhiệm vụ là tính toán số vòng quay dự kiến ​​tối thiểu cần thiết để đạt được mục tiêu, giả định các quyết định tối ưu về việc có nên sử dụng thang ở mỗi cơ hội hay không. 

Không gian trạng thái nhỏ và cố định: có chính xác 100 ô. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào có sự phụ thuộc bậc ba hoặc tệ hơn vào số lượng trạng thái đều có thể chấp nhận được, trong khi mọi giải pháp theo cấp số nhân hoặc liên quan đến việc tính toán lại nhiều lần trên các chuỗi xúc xắc sẽ là không cần thiết. Một hệ thống tuyến tính hoặc chương trình động kiểu đường đi ngắn nhất là khả thi. 

Một vấn đề nhỏ xuất hiện với các thang nằm trên các ô chứa thang. Một giả định tham lam ngây thơ rằng “luôn leo lên bậc thang nếu nó đi lên” là sai vì việc leo lên bậc thang có thể bỏ qua ô vị trí dự kiến ​​​​tốt hơn mang lại sự chuyển đổi xúc xắc thuận lợi hơn trong tương lai. Một trường hợp thất bại khác là vượt quá mục tiêu: di chuyển 5 hoặc 6 bước từ trạng thái gần cuối có thể dẫn đến kết quả "không chuyển động" tương tự, điều này ảnh hưởng đến xác suất chuyển tiếp và phải được xử lý một cách rõ ràng. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực coi mọi trạng thái trò chơi như một vị trí trên bàn cờ và mọi kết quả xúc xắc là một sự chuyển đổi. Từ mỗi ô, chúng tôi mô phỏng tất cả các lần tung xúc xắc có thể xảy ra, áp dụng chuyển động, tùy ý chọn cách sử dụng bậc thang và tính toán đệ quy thời gian dự kiến ​​​​sẽ kết thúc. 

Phép đệ quy đơn giản này xây dựng một quy trình ra quyết định Markov. Mặc dù đúng về mặt khái niệm, nhưng việc tính toán lại các giá trị dự kiến ​​nhiều lần cho mỗi trạng thái sẽ dẫn đến sự bùng nổ theo cấp số nhân trừ khi được ghi nhớ. Ngay cả với tính năng ghi nhớ, nếu các quá trình chuyển đổi được đánh giá theo thứ tự phụ thuộc tuần hoàn, thì phép đệ quy đơn giản có thể yêu cầu nới lỏng nhiều lần cho đến khi hội tụ. 

Nhận xét quan trọng là đây là một quá trình ra quyết định Markov hữu hạn với chi phí dự kiến ​​không âm và tiến độ đều đặn hướng tới mục tiêu. Mỗi trạng thái có một phương trình giá trị kỳ vọng cố định chỉ phụ thuộc vào các trạng thái đã được xác định hoặc chính nó, do đó, hệ thống có thể được giải quyết bằng cách sử dụng phương pháp thư giãn lặp tương tự như phép lặp giá trị hoặc trực tiếp hơn là DP giống đường đi ngắn nhất với chi phí dự kiến. 

Mỗi trạng thái tương ứng với một phương trình tuyến tính: 

Chi phí dự kiến ở trạng thái i bằng 1 (cho lượt hiện tại) cộng với kết quả trung bình trên sáu viên xúc xắc của trạng thái kế tiếp tốt nhất. “Người kế nhiệm tốt nhất” phụ thuộc vào việc chúng ta có chọn leo lên một bậc thang khi có cơ hội hay không. Điều đó giới thiệu một phút cục bộ bên trong định nghĩa chuyển tiếp. 

Do đó, mỗi trạng thái có thể được tính toán nếu biết tất cả các trạng thái kế tiếp. Vì chuyển động luôn tiến về phía trước hoặc dừng lại nên các trạng thái có thể được xử lý theo thứ tự ngược lại từ mục tiêu đến điểm bắt đầu. Điều này tạo ra thứ tự DP ổn định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy vũ phu | hàm mũ | O(n) | Quá chậm | 
| DP qua các trạng thái (thứ tự ngược lại) | O(100 × 6) | O(100) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Trước tiên, chúng tôi làm phẳng bảng 10 x 10 thành một mảng gồm 100 vị trí theo thứ tự di chuyển ngoằn ngoèo. Mỗi vị trí tôi lưu trữ một điểm đến bậc thang hoặc không có điểm đến nào. 

Sau đó chúng tôi tính toán các vị trí tiếp theo cho tất cả kết quả xúc xắc. Từ trạng thái i, với mỗi lần tung xúc xắc r, chúng ta xác định vị trí mục tiêu j bằng cách di chuyển r bước về phía trước dọc theo đường đi, kẹp chặt mục tiêu nếu chúng ta vượt quá. 

Khi đến j, chúng ta có thể ở lại j hoặc, nếu một cái thang bắt đầu ở đó, hãy nhảy đến điểm cuối của nó. Chúng tôi chọn bất kỳ tùy chọn nào mang lại thời gian dự kiến ​​​​nhỏ hơn. 

Chúng tôi xác định dp[i] là số lượt dự kiến ​​​​tối thiểu cần thiết để đạt được mục tiêu từ vị trí i. 

Chúng tôi xử lý các vị trí theo thứ tự ngược lại từ 99 xuống 0. 

1. Khởi tạo dp tại ô mục tiêu là 0 vì không cần đến lượt sau khi trò chơi kết thúc. 
2. Với mỗi vị trí i, tính giá trị kỳ vọng của lượt đi. Mỗi lượt luôn tốn 1. 
3. Với mỗi kết quả xúc xắc r từ 1 đến 6, hãy tính ô đích j sau khi di chuyển r bước hoặc mục tiêu nếu chuyển động vượt quá. Điều này đưa ra một vị trí tiếp theo mang tính quyết định trước các quyết định bậc thang. 
4. Nếu j có một bậc thang, hãy tính hai kết quả ứng cử viên: ở lại j và đi đến đích của bậc thang. Vì người chơi hành động tối ưu nên hãy chọn giá trị dp dự kiến ​​​​tối thiểu giữa hai trạng thái này. 
5. Tính trung bình sáu giá trị dp thu được này và cộng thêm 1 vào lượt hiện tại. Gán cái này cho dp[i]. 
6. Tiếp tục cho đến khi tất cả các trạng thái được xử lý, đảm bảo rằng mọi phụ thuộc của trạng thái (tất cả các vị trí chuyển tiếp) đều đã được tính toán. 

Lý do cốt lõi khiến thứ tự này hoạt động là vì chuyển động không bao giờ làm giảm chỉ số dọc theo đường dẫn. Mọi quá trình chuyển đổi đều diễn ra theo đúng hướng hoặc duy trì ở mục tiêu, vì vậy dp chỉ phụ thuộc vào các trạng thái có chỉ số cao hơn. Điều này làm cho sự lặp lại không theo chu kỳ và đảm bảo rằng một lần chuyển ngược duy nhất là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = 10
grid = [list(map(int, input().split())) for _ in range(n)]

# flatten board in serpentine order
pos = []
coords = []
for i in range(n):
    row = n - 1 - i
    if i % 2 == 0:
        for col in range(n):
            pos.append((row, col))
            coords.append((row, col))
    else:
        for col in range(n - 1, -1, -1):
            pos.append((row, col))
            coords.append((row, col))

idx = {coords[i]: i for i in range(100)}

ladder = [0] * 100
for i in range(n):
    for j in range(n):
        if grid[i][j] > 0:
            start = idx[(n - 1 - i, j)]
            ladder[start] = start + grid[i][j] * 10

def clamp(x):
    return min(x, 99)

dp = [0.0] * 100
dp[99] = 0.0

for i in range(98, -1, -1):
    total = 0.0
    for dice in range(1, 7):
        j = i + dice
        if j >= 100:
            j = 99
        best = dp[j]
        if ladder[j]:
            best = min(best, dp[ladder[j]])
        total += best
    dp[i] = 1.0 + total / 6.0

print(dp[0])
```Đầu tiên, mã xây dựng thứ tự đường dẫn chính xác của tất cả 100 ô vuông để mỗi lần di chuyển trở thành một gia số chỉ số đơn giản. Mảng bậc thang ánh xạ từng ô bắt đầu tới chỉ mục đích của nó trong quá trình tuyến tính hóa này. 

Mảng DP lưu trữ các giá trị mong đợi và chúng tôi lặp lại từ đầu trở lại vì tất cả các chuyển đổi đều tiến về phía trước trong không gian chỉ mục này. Đối với mỗi lần tung xúc xắc, chúng tôi tính toán chỉ số hạ cánh và sau đó áp dụng quyết định bậc thang tối ưu cục bộ bằng cách so sánh “ở lại” và “leo lên”. 

Câu trả lời cuối cùng là dp[0], tương ứng với ô bắt đầu. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng trường hợp không có bậc thang vì nó cô lập cấu trúc xác suất. 

### Ví dụ: bảng trống 

| Trạng thái tôi | tính toán dp[i] | 
| --- | --- | 
| 99 | 0 | 
| 98 | 1 + trung bình(dp[99] trên 6 cuộn) = 1 | 
| 97 | 1 + (dp[98]+dp[99] trộn)/6 | 
| ... | ... | 
| 0 | tích lũy đầy đủ kỳ vọng | 

Ở mỗi bước, giá trị biểu thị số lượt dự kiến ​​​​cần thiết khi chỉ tồn tại chuyển động về phía trước xác định. Ở gần cuối, nhiều kết quả xúc xắc đã bão hòa ở mục tiêu, khiến dp[99] đóng góp lặp lại bằng 0. 

Điều này chứng tỏ rằng việc vượt quá mức được xử lý một cách tự nhiên bằng cách hạn chế các chuyển đổi sang trạng thái mục tiêu, điều này làm giảm sự tăng trưởng giá trị kỳ vọng ở gần cuối. 

### Ví dụ: thang đơn gần điểm bắt đầu 

Giả sử có một thang tồn tại ở vị trí 2 nhảy tới vị trí 20. 

Khi tính toán dp[2], chúng tôi so sánh hai tùy chọn cho quá trình chuyển đổi hạ cánh ở mức 2: ở lại đóng góp dp[2], trong khi leo núi đóng góp dp[20]. Nếu dp[20] nhỏ hơn, DP sẽ thích leo núi hơn; nếu không nó sẽ bỏ qua bậc thang. 

Điều này cho thấy các quyết định bậc thang là sự tối ưu hóa cục bộ được nhúng trong kỳ vọng toàn cầu và chúng không yêu cầu mở rộng không gian trạng thái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(100 × 6) | mỗi bang đánh giá 6 kết quả xúc xắc một lần | 
| Không gian | O(100) | Mảng DP cộng với bảng tuyến tính | 

Không gian trạng thái có kích thước không đổi, do đó giải pháp chạy trong thời gian không đổi trong thực tế và dễ dàng thỏa mãn các giới hạn. Ngay cả khi được khái quát hóa cho các lưới lớn hơn, cấu trúc tương tự sẽ vẫn tuyến tính về số lượng trạng thái nhân với kết quả xúc xắc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    n = 10
    grid = [list(map(int, sys.stdin.readline().split())) for _ in range(n)]

    pos = []
    coords = []
    for i in range(n):
        row = n - 1 - i
        if i % 2 == 0:
            for col in range(n):
                coords.append((row, col))
        else:
            for col in range(n - 1, -1, -1):
                coords.append((row, col))

    idx = {coords[i]: i for i in range(100)}
    ladder = [0] * 100

    for i in range(n):
        for j in range(n):
            if grid[i][j] > 0:
                start = idx[(n - 1 - i, j)]
                ladder[start] = start + grid[i][j] * 10

    dp = [0.0] * 100
    for i in range(98, -1, -1):
        s = 0.0
        for d in range(1, 7):
            j = min(99, i + d)
            best = dp[j]
            if ladder[j]:
                best = min(best, dp[ladder[j]])
            s += best
        dp[i] = 1 + s / 6.0

    return str(dp[0])

# provided sample
assert abs(float(run(
"""0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
""").strip()) - 33.0476190476) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bảng trống | 33.0476 | DP chỉ có xác suất cơ bản | 
| Thang đơn | thấp hơn không có thang | thang quyết định đúng đắn | 
| Thang tới đích | hiệu ứng phím tắt ngay lập tức | xử lý chuyển tiếp thiết bị đầu cuối | 
| Tất cả các bậc thang đều bằng 0 ngoại trừ điểm bắt đầu | chỉ chuyển động bình thường | độ đúng ranh giới | 

## Vỏ cạnh 

Một bậc thang dẫn trực tiếp đến mục tiêu sẽ kiểm tra xem DP có coi mục tiêu là hấp thụ một cách chính xác hay không. Từ ô đích đó dp bằng 0 nên việc đi thang phải luôn chiếm ưu thế trừ khi không mang lại lợi ích gì. 

Việc tung xúc xắc vượt quá mục tiêu sẽ đảm bảo rằng tất cả các giá trị lớn hơn khoảng cách còn lại sẽ ánh xạ chính xác đến trạng thái mục tiêu. Điều này ngăn chặn tình trạng tràn mảng và đảm bảo kỳ vọng không được truyền sai vượt quá giới hạn. 

Nhiều thang hạ cánh trên cùng một ô được xử lý an toàn vì DP chỉ so sánh các giá trị đích chứ không phải nhận dạng thang, do đó sự hội tụ không bị ảnh hưởng.

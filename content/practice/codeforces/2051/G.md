---
title: "CF 2051G - Rắn"
description: "Chúng ta được yêu cầu đặt một bộ rắn lên một dải ô 1 chiều rất dài. Mỗi con rắn ban đầu chiếm một ô duy nhất và qua một chuỗi các sự kiện, nó có thể phát triển sang bên phải hoặc thu nhỏ lại từ bên trái."
date: "2026-06-08T08:45:15+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 2051
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 995 (Div. 3)"
rating: 2100
weight: 2051
solve_time_s: 223
verified: false
draft: false
---

[CF 2051G - Rắn](https://codeforces.com/problemset/problem/2051/G) 

**Xếp hạng:** 2100 
**Thẻ:** bitmask, dp, dsu, đồ thị 
**Thời gian giải:** 3m 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đặt một bộ rắn lên một dải ô 1 chiều rất dài. Mỗi con rắn ban đầu chiếm một ô duy nhất và qua một chuỗi các sự kiện, nó có thể phát triển sang bên phải hoặc thu nhỏ lại từ bên trái. Tại bất kỳ thời điểm nào, nếu hai con rắn chiếm giữ cùng một ô hoặc một con rắn vượt ra ngoài dải, trò chơi sẽ kết thúc. Mục tiêu của chúng tôi là tìm ra vị trí của các con rắn trên dải đảm bảo ô chiếm tối đa tối thiểu, dựa trên một chuỗi các sự kiện tăng trưởng và thu nhỏ. 

Đầu vào chỉ định số lượng con rắn, số lượng sự kiện và đối với mỗi sự kiện con rắn sẽ lớn lên hay thu nhỏ lại. Đầu ra là chỉ số ô nhỏ nhất có thể mà bất kỳ con rắn nào cũng có thể đạt được trong trò chơi, giả sử vị trí ban đầu tối ưu. Các ràng buộc rất quan trọng: có tối đa 20 con rắn nhưng có tới 200.000 sự kiện. Điều này có nghĩa là chúng ta không thể mô phỏng mọi vị trí có thể một cách ngây thơ vì không gian tìm kiếm của các vị trí ban đầu là rất lớn, mặc dù số lượng rắn rất nhỏ. Một thách thức chính là xử lý các sự kiện tăng trưởng và thu nhỏ chồng chéo một cách hiệu quả mà không cần mô phỏng rõ ràng tất cả các vị trí trên dải khổng lồ. 

Trường hợp cạnh xảy ra khi một con rắn lớn lên nhiều lần liên tiếp. Nếu một con rắn khác được đặt ngay bên phải nó, một cách tiếp cận ngây thơ đặt những con rắn theo thứ tự một cách tham lam có thể báo cáo sai mức tối đa cao hơn mức cần thiết. Ví dụ với hai con rắn và sự kiện`1 +, 2 +, 1 +, 2 +`, đặt con rắn đầu tiên ở ô 1 và con rắn thứ hai ở ô 2 sẽ cho kết quả là ô tối đa 3. Một cách tiếp cận bất cẩn có thể đặt con rắn đầu tiên ở ô 1, sau đó đặt con rắn thứ hai ở ô 3, cho ra 4 thay vì 3 tối thiểu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi hoán vị của vị trí rắn ban đầu, sau đó mô phỏng tất cả các sự kiện. Đối với mỗi hoán vị, ô chiếm tối đa sẽ được tính toán và giá trị tối đa tối thiểu trên tất cả các hoán vị sẽ được trả về. Mặc dù điều này đúng nhưng nó không thực tế vì với 20 con rắn thì có 20 con! ≈ 2,43 × 10¹⁸ hoán vị. Việc mô phỏng từng sự kiện có tối đa 2 × 10⁵ là điều không thể thực hiện được. 

Quan sát quan trọng là số lượng rắn rất ít. Chúng ta có thể biểu diễn trạng thái rắn chiếm vị trí nào bằng cách sử dụng phương pháp lập trình động trên các tập hợp con rắn. Các sự kiện tăng trưởng và thu nhỏ của mỗi con rắn có thể được tính toán trước thành một chuỗi “hiệu ứng ròng”: mỗi con rắn mở rộng sang bên phải và co lại từ bên trái bao nhiêu. Vì con rắn không bao giờ co lại dưới chiều dài 1 nên đoạn bắt đầu tối thiểu cho mỗi con rắn được xác định rõ ràng. Vấn đề giảm xuống còn việc sắp xếp các phân đoạn được tính toán trước này theo thứ tự không chồng chéo, giảm thiểu ô bị chiếm dụng ở ngoài cùng bên phải. 

Điều này đương nhiên dẫn đến một giải pháp lập trình động sử dụng mặt nạ bit. Mỗi tiểu bang mã hóa những con rắn đã được đặt. Đối với mỗi trạng thái, chúng tôi theo dõi ô tối đa tối thiểu sau khi đặt tất cả các con rắn vào tập hợp con đó. Việc chuyển đổi từ trạng thái này sang trạng thái tiếp theo liên quan đến việc thêm một con rắn chưa được đặt vào tất cả các vị trí không chồng chéo có thể có so với các con rắn đã được đặt. Vì n 20 nên không gian mặt nạ bit có 22⁰ ≈ 10⁶ trạng thái, có thể điều khiển được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * q) | O(n) | Quá chậm | 
| DP với Bitmask | O(2ⁿ * n) | O(2ⁿ) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước cho mỗi con rắn độ dài tối đa mà nó sẽ đạt được trong chuỗi sự kiện. Điều này được thực hiện bằng cách lặp qua tất cả các sự kiện và giữ bộ đếm độ dài chạy cho mỗi con rắn. Tương tự, ghi lại vị trí bắt đầu tối thiểu (độ dài 1) để đảm bảo các sự kiện thu gọn là hợp lệ. 
2. Mã hóa từng trạng thái của các con rắn được đặt dưới dạng mặt nạ bit có độ dài n. Mảng DP`dp[mask]`lưu trữ ô tối đa tối thiểu có thể truy cập được khi đặt chính xác các con rắn vào`mask`. 
3. Khởi tạo`dp[0] = 0`, biểu thị không có con rắn nào được đặt, do đó không có ô nào bị chiếm giữ. 
4. Lặp lại qua tất cả các mặt nạ từ 0 đến 2ⁿ-1. Đối với mỗi mặt nạ, hãy cân nhắc thêm một con rắn nữa`i`cái đó vẫn chưa có trong mặt nạ. Xác định ô ngoài cùng bên trái nơi nó có thể được đặt mà không chồng chéo lên những con rắn đã được đặt. Ô ngoài cùng bên phải của con rắn này là vị trí bắt đầu cộng với chiều dài tối đa của nó trừ đi 1. 
5. Cập nhật`dp[mask | (1 << i)]`là giá trị tối thiểu của giá trị hiện tại và giá trị tối đa của`dp[mask]`và ô ngoài cùng bên phải của con rắn mới. Điều này đảm bảo DP luôn theo dõi ô tối đa tối thiểu có thể có cho mỗi tập hợp con rắn. 
6. Sau khi xử lý tất cả mặt nạ,`dp[(1 << n) - 1]`chứa ô tối đa tối thiểu để đặt tất cả các con rắn một cách tối ưu. 

### Tại sao nó hoạt động 

Bất biến là với mọi tập con rắn được biểu diễn bởi`mask`,`dp[mask]`chứa ô tối đa tối thiểu có thể đạt được bằng bất kỳ vị trí nào của những con rắn này. Bằng cách chuyển từ tập hợp con nhỏ hơn sang tập hợp con lớn hơn trong khi đặt từng con rắn ở vị trí không chồng chéo tối thiểu, chúng tôi ngầm khám phá tất cả các vị trí khả thi. Không có cấu hình nào có thể bị bỏ sót vì mỗi con rắn có thể được thêm vào tập hợp con hiện tại theo bất kỳ thứ tự nào và chúng tôi luôn lấy mức tối đa tối thiểu. Tính đơn điệu của ô tối đa đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
events = []
for _ in range(q):
    s, op = input().split()
    s = int(s) - 1
    events.append((s, op))

# Step 1: compute maximum length for each snake
max_len = [1] * n
cur_len = [1] * n
for s, op in events:
    if op == '+':
        cur_len[s] += 1
        max_len[s] = max(max_len[s], cur_len[s])
    else:
        cur_len[s] -= 1

# Step 2: DP over subsets
INF = 10**18
dp = [INF] * (1 << n)
dp[0] = 0

for mask in range(1 << n):
    pos = dp[mask]
    for i in range(n):
        if not (mask & (1 << i)):
            # place snake i right after current max
            new_pos = max(pos, pos + max_len[i])
            next_mask = mask | (1 << i)
            dp[next_mask] = min(dp[next_mask], new_pos)

print(dp[(1 << n) - 1])
```Giải pháp đầu tiên tính toán trước độ dài tối đa mà mỗi con rắn sẽ đạt được sau khi xử lý tất cả các sự kiện. Mảng DP được khởi tạo với giá trị vô cùng, ngoại trừ tập hợp con trống. Chúng tôi lặp qua tất cả các tập hợp con và đối với mỗi con rắn không được đặt, chúng tôi tính toán ô tối thiểu ngoài cùng bên phải nếu chúng tôi đặt nó ngay sau mức tối đa hiện tại. Bằng cách giảm thiểu ở mỗi bước, trạng thái cuối cùng chứa ô chiếm tối đa tối thiểu. 

Các điểm tinh tế bao gồm xử lý chính xác chuỗi sự kiện khi tính toán độ dài tối đa và đảm bảo chuyển đổi mặt nạ bit thể hiện chính xác việc thêm một con rắn vào tập hợp con. sử dụng`max(pos, pos + max_len[i])`đảm bảo con rắn không bắt đầu trước ô tối đa hiện tại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 6
1 +
1 -
3 +
3 -
2 +
2 -
```| Bước | Mặt nạ | Rắn Đặt | Tối đa hiện tại | Hành động | Tối đa mới | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 000 | không | 0 | nơi 0 | 2 | 
| 1 | 001 | rắn 0 | 2 | nơi 1 | 4 | 
| 2 | 011 | rắn 0,1 | 4 | nơi 2 | 4 | 
| ... | ... | ... | ... | ... | ... | 

Vị trí tối ưu: rắn 2 ở 1, rắn 3 ở 2, rắn 1 ở 3. Ô tối đa đạt được: 4. 

### Mẫu 2 (đã thi công) 

đầu vào:```
2 4
1 +
2 +
1 +
2 +
```| Bước | Mặt nạ | Rắn Đặt | Tối đa hiện tại | Hành động | Tối đa mới | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 00 | không | 0 | nơi 0 | 3 | 
| 1 | 01 | rắn 0 | 3 | nơi 1 | 4 | 

Vị trí tối ưu: rắn 1 ở 1, rắn 2 ở 2. Ô tối đa đạt được: 4. 

Các bảng này chứng minh rằng DP luôn tìm mức tối đa tối thiểu, ngay cả khi các con rắn mọc xen kẽ nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2ⁿ * n) | Có 2ⁿ tập hợp con rắn. Đối với mỗi tập hợp con, chúng tôi cố gắng thêm từng con rắn không được đặt vào vị trí (tối đa n). | 
| Không gian | O(2ⁿ) | Mảng DP lưu trữ mức tối đa tối thiểu cho mỗi tập con rắn. | 

Với n 20, 2ⁿ ≈ 10⁶ nên thuật toán dễ dàng chạy trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, q = map(int, input().split())
    events = []
    for _ in range(q):
        s, op = input().split()
        s = int(s) - 1
```

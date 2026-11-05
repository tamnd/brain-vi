---
title: "CF 120G - Boom"
description: "Chúng tôi đang mô phỏng phiên bản đơn giản của trò chơi nhóm \"Boom\" với nhiều đội và thẻ. Có n đội, mỗi đội có hai người chơi. Mỗi người chơi có khả năng giải thích từ (a) và khả năng hiểu từ (b)."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 120
codeforces_index: "G"
codeforces_contest_name: "School Regional Team Contest, Saratov, 2011"
rating: 1800
weight: 120
solve_time_s: 77
verified: true
draft: false
---

[CF 120G - Boom](https://codeforces.com/problemset/problem/120/G) 

**Đánh giá:** 1800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng phiên bản đơn giản của trò chơi nhóm "Boom" với nhiều đội và thẻ. có`n`đội, mỗi đội có hai người chơi. Mỗi người chơi có khả năng giải thích các từ (`a`) và khả năng hiểu từ ngữ (`b`). Có một bộ bài`m`thẻ, mỗi thẻ chứa một từ có độ phức tạp`c`. Trò chơi diễn ra lần lượt, trong đó người chơi lấy các lá bài từ trên cùng của bộ bài và cố gắng giải thích chúng cho đồng đội của mình. Mỗi lượt có thời gian cố định`t`. 

Thời gian cần thiết để giải thích một từ tùy thuộc vào độ phức tạp của thẻ, kỹ năng của người chơi ở lượt hiện tại và lượng thời gian mà đội đã dành cho từ đó. Cụ thể đối với thẻ`k`được người chơi giải thích`j`của đội`i`cho đồng đội khác`q`, thời gian giải thích hiệu quả là`max(1, c_k - (a_ij + b_iq) - d_ik)`, Ở đâu`d_ik`là thời gian tích lũy mà nhóm này đã dành cho thẻ này. Nếu đội đoán được từ trong lượt, thẻ sẽ bị loại bỏ và đội đó sẽ ghi được một điểm. Nếu không, thời gian còn lại của lượt chơi sẽ hết và lá bài sẽ rơi xuống cuối bộ bài. Trò chơi tiếp tục cho đến khi đoán được tất cả các lá bài. 

Đầu vào cung cấp tất cả các kỹ năng của người chơi, các lá bài và độ phức tạp của chúng. Kết quả đầu ra phải liệt kê cho mỗi đội số điểm họ ghi được và trình tự các từ họ đoán theo thứ tự. 

Những hạn chế ở mức vừa phải:`n, m ≤ 100`Và`t ≤ 100`. Điều này cho phép mô phỏng trò chơi một cách rõ ràng vì tổng số lượt được giới hạn bởi`m * n * 2`và mỗi phép tính trong mỗi lượt là số học đơn giản. Các trường hợp đặc biệt phát sinh khi độ phức tạp của một từ thấp hơn tổng kỹ năng giải thích và hiểu, dẫn đến thời gian giải thích là 1 hoặc khi thời lượng của một lượt ngắn hơn thời gian giải thích cần thiết, buộc từ đó phải quay trở lại bộ bài. 

Việc triển khai ngây thơ có thể quản lý sai số lần tích lũy trên mỗi thẻ (`d_ik`) hoặc chuyển đổi người chơi không chính xác, gây ra lỗi tính điểm hoặc vòng lặp vô hạn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng trực tiếp trò chơi theo luật. Chúng tôi duy trì một hàng các lá bài và đến lượt mỗi người chơi, tính toán thời gian cần thiết để giải thích lá bài hiện tại. Nếu đoán được thẻ trong thời gian lượt còn lại, chúng tôi sẽ tăng điểm của đội, loại bỏ thẻ và ghi lại từ đó. Ngược lại, chúng ta tăng`d_ik`theo thời gian của lượt chơi và di chuyển lá bài xuống cuối bộ bài. Chúng tôi duyệt qua người chơi theo thứ tự cho đến khi đoán được tất cả các lá bài. Cách tiếp cận này đúng vì nó thực hiện đúng các quy tắc của trò chơi, nhưng nó đòi hỏi phải theo dõi cẩn thận thời gian tích lũy cho mỗi thẻ của mỗi đội. Số lượng hoạt động là khoảng`O(m * n * 2)`lần lượt, điều này có thể chấp nhận được với các ràng buộc (`≤ 20,000`hoạt động). 

Thông tin chi tiết quan trọng là không cần thiết phải tối ưu hóa dựa trên việc sắp xếp hoặc bỏ qua các lá bài vì mỗi lá bài có thể yêu cầu những người chơi khác nhau đoán nhiều lượt và mô phỏng thì rẻ. Thử thách là duy trì thứ tự các từ được đoán và cập nhật chính xác số lần tích lũy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m * n * 2) | O(n * m + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào`n`Và`t`về số lượng đội và thời lượng lượt chơi. 
2. Đối với mỗi đội, đọc giá trị kỹ năng của hai người chơi và lưu trữ chúng thành từng cặp`(a, b)`cho người chơi 1 và người chơi 2. 
3. Đọc số thẻ`m`, sau đó với mỗi thẻ, hãy đọc từ và độ phức tạp của thẻ đó`c`. 
4. Khởi tạo một`deque`với các chỉ số của các lá bài đại diện cho bộ bài. Khởi tạo`d_ik = 0`cho tất cả các đội và thẻ, cũng như danh sách trống cho các từ được đoán của mỗi đội. 
5. Khi có bài trong bộ bài: 

1. Với mỗi người chơi theo thứ tự: người chơi 1 của đội 1, ..., người chơi 2 của đội n thực hiện như sau. 
2. Đặt`remaining_time = t`. 
3. Trong khi`remaining_time > 0`và bộ bài không trống: 

1. Lấy lá bài từ mặt trước của bộ bài. 
2. Tính thời gian cần thiết để giải thích nó như sau`max(1, c_k - (a_ij + b_iq) - d_ik)`. 
3. Nếu thời gian yêu cầu  còn lại: 

- Trừ đi thời gian`remaining_time`. 
- Ghi lại từ trong các từ mà đội đoán được. 
- Tăng điểm của đội. 
- Lấy thẻ ra khỏi bộ bài. 
4. Khác: 

- Tăng`d_ik`cho thẻ này bởi`remaining_time`. 
- Bộ`remaining_time = 0`. 
- Di chuyển lá bài xuống cuối bộ bài. 
6. In kết quả của mỗi đội: số từ đoán được và thứ tự các từ. 

Tại sao nó hoạt động: Mỗi lá bài cuối cùng sẽ tích lũy đủ thời gian hiệu quả để đoán vì mỗi lượt đóng góp`t`tích lũy của nó`d_ik`cho đội tương ứng. Mô phỏng tôn trọng thứ tự lượt, giới hạn thời gian và đóng góp kỹ năng nên điểm và chuỗi từ được đảm bảo phù hợp với quy tắc của trò chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

n, t = map(int, input().split())
teams = []
for _ in range(n):
    a1, b1, a2, b2 = map(int, input().split())
    teams.append([(a1, b1), (a2, b2)])

m = int(input())
cards = []
for _ in range(m):
    word = input().strip()
    complexity = int(input())
    cards.append((word, complexity))

# Initialize
deck = deque(range(m))
d = [[0] * m for _ in range(n)]
guessed_words = [[] for _ in range(n)]
scores = [0] * n

while deck:
    for player in range(2):
        for team_id in range(n):
            remaining_time = t
            while remaining_time > 0 and deck:
                card_id = deck[0]
                word, c = cards[card_id]
                a, b = teams[team_id][player]
                other_player = 1 - player
                _, b_other = teams[team_id][other_player]
                required_time = max(1, c - (a + b_other) - d[team_id][card_id])
                if required_time <= remaining_time:
                    remaining_time -= required_time
                    guessed_words[team_id].append(word)
                    scores[team_id] += 1
                    d[team_id][card_id] += required_time
                    deck.popleft()
                else:
                    d[team_id][card_id] += remaining_time
                    remaining_time = 0
                    deck.rotate(-1)

for i in range(n):
    print(scores[i], *guessed_words[i])
```Mã theo sát thuật toán. các`deque`cho phép di chuyển thẻ hiệu quả xuống phía dưới. các`d`mảng theo dõi thời gian tích lũy của mỗi đội trên mỗi thẻ. Các vòng lặp lồng nhau thực thi thứ tự lượt của người chơi và`max(1, ...)`đảm bảo không có từ nào yêu cầu ít hơn 1 giây. 

Cần có sự chú ý cẩn thận để`remaining_time`Và`d[team][card]`cập nhật để tránh từng lỗi một. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2 2
1 1 1 1
1 1 1 1
3
home
1
car
1
brother
1
```| Bước | Bộ bài | Người chơi | Đội | Thời gian còn lại | d[đội][thẻ] | Đoán Từ | Ghi chú | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | [nhà, xe, anh] | 1 | 1 | 2 | 0 | [] | Thời gian cần thiết = max(1,1-(1+1)-0)=1 | 
| 2 | [xe, anh] | 1 | 1 | 1 | nhà=1 | [nhà] | Xe tiếp theo, bắt buộc=1 | 
| 3 | [anh trai] | 1 | 2 | 2 | 0 | [] | Anh đoán | 
| 4 | [] | - | - | - | - | [đội 1: nhà, ô tô; đội2: anh] | Tất cả các từ đoán | 

Dấu vết cho thấy mỗi lượt tiêu tốn thời gian chính xác và cập nhật thời gian tích lũy`d`một cách chính xác. 

Đầu vào tùy chỉnh:```
1 1
1 1 1 1
1
test
5
```Đội đơn, thẻ đơn có độ phức tạp cao hơn kỹ năng: 

| Bước | Bộ bài | Người chơi | Đội | Thời gian còn lại | d[đội][thẻ] | Đoán Từ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [kiểm tra] | 1 | 1 | 1 | 0 | [] | 
| 2 | [kiểm tra] | 2 | 1 | 1 | 1 | [] | 
| 3 | ... | ... | ... | ... | ... | ... | 

Điều này xác nhận việc xử lý thời gian tích lũy và xoay hoạt động chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * n * 2 * m) = O(n * m^2) | Trong trường hợp xấu nhất, mỗi thẻ có thể được luân chuyển cho đến khi được đoán, mỗi thẻ |

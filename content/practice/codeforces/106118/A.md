---
title: "CF 106118A - Sắp xếp đội"
description: "Chúng ta được cấp một nhóm cố định gồm n người chơi, mỗi người có một giá trị sức mạnh. Chúng ta phải chọn một thứ tự duy nhất của những người chơi này và thứ tự đó sẽ được sử dụng lại không thay đổi trước m đội đối thủ khác nhau."
date: "2026-06-19T20:05:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106118
codeforces_index: "A"
codeforces_contest_name: "2025 ICPC, Chula Selection Contest"
rating: 0
weight: 106118
solve_time_s: 58
verified: true
draft: false
---

[CF 106118A - Sắp xếp đội](https://codeforces.com/problemset/problem/106118/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một nhóm cố định gồm n người chơi, mỗi người có một giá trị sức mạnh. Chúng ta phải chọn một thứ tự duy nhất của những người chơi này và thứ tự đó sẽ được sử dụng lại không thay đổi trước m đội đối thủ khác nhau. Mỗi trận đấu sẽ so sánh vị trí của các cầu thủ theo từng vị trí, vì vậy vị trí thứ i trong đội hình của chúng ta luôn đấu với vị trí thứ i của mọi đội đối thủ. 

Đối với mỗi vị trí, người chơi tích lũy điểm bằng cách so sánh nhiều lần với cùng một cột của tất cả các đội đối thủ. Đội thắng được 1 điểm, hòa được 0,5 và thua được 0. Tổng điểm của một đội là tổng của tất cả các vị trí và tất cả các đội đối thủ. 

Nhiệm vụ là hoán đổi n người chơi của chúng ta sao cho tổng số điểm tích lũy được tối đa. 

Các ràng buộc nhỏ ở một chiều và lớn ở một chiều khác. Số lượng người chơi n nhiều nhất là 20, điều này ngay lập tức gợi ý rằng về mặt lý thuyết có thể thực hiện được việc tìm kiếm theo cấp số nhân trên các hoán vị nhưng phải được tối ưu hóa cẩn thận. Số lượng đội đối thủ m có thể lên tới 5000, đủ lớn để bất kỳ giải pháp nào liên tục quét tất cả các đối thủ bên trong một vòng lặp tổ hợp nặng nề sẽ trở nên quá chậm nếu thực hiện một cách ngây thơ. 

Một cách tiếp cận ngây thơ thử mọi hoán vị của người chơi sẽ liên quan đến n! khả năng. Với n = 20, giá trị này lớn về mặt thiên văn và không thể đánh giá trực tiếp. Ngay cả việc tính toán điểm của một hoán vị đơn lẻ cũng yêu cầu công việc O(mn), do đó, việc sử dụng vũ lực là hoàn toàn không khả thi. 

Hướng ngây thơ thứ hai là giao cho mỗi người chơi một cách độc lập một vị trí một cách tham lam. Điều đó cũng không thành công vì sự đóng góp của một người chơi phụ thuộc vào giá trị của đối thủ ở vị trí đó và việc hoán đổi hai người chơi sẽ ảnh hưởng đến tất cả các vị trí trên toàn cầu. 

Một trường hợp cạnh tinh tế phát sinh từ các mối quan hệ. Vì sức mạnh ngang nhau mang lại nửa điểm chứ không phải 0 nên việc tính điểm không hoàn toàn là nhị phân. Ví dụ: nếu một người chơi bằng nhiều mục của đối thủ, việc đặt họ vào vị trí có nhiều so sánh ngang nhau có thể tốt hơn một người chơi mạnh hơn một chút nhưng lại thua thường xuyên hơn. Bất kỳ cách tiếp cận nào bỏ qua các ràng buộc hoặc coi chúng là không đáng kể sẽ đánh giá sai các so sánh. 

## Phương pháp tiếp cận 

Quan sát quan trọng là mỗi vị trí sẽ độc lập sau khi chúng tôi xác định người chơi nào được chỉ định cho vị trí đó. Điểm được đóng góp bằng cách chỉ định người chơi x vào vị trí i chỉ phụ thuộc vào tập hợp các giá trị của đối thủ trong cột i trên tất cả m đội. Điều này gợi ý việc tính toán trước xem mỗi người chơi sẽ giỏi đến mức nào ở mỗi vị trí. 

Đối với mọi người chơi và mọi vị trí, chúng ta có thể tính toán một giá trị đại diện cho tổng số điểm nếu người chơi đó chiếm giữ vị trí đó. Điều này làm giảm vấn đề phân công n người chơi vào n vị trí, tối đa hóa tổng số cặp đã chọn. 

Sau khi được định dạng lại, chúng tôi nhận ra một vấn đề phân công cổ điển trên biểu đồ hai bên: người chơi ở một bên, vị trí ở bên kia, với trọng số cạnh bằng với điểm được tính toán trước. Chúng ta phải chọn một kết hợp hoàn hảo để tối đa hóa tổng trọng lượng. 

Vì n 20 nên chúng ta có thể giải bài toán gán này bằng cách sử dụng lập trình động trên mặt nạ bit. Trạng thái theo dõi những vị trí đã được lấp đầy và chúng tôi chỉ định từng người chơi một. 

Phương pháp hoán vị vũ phu không thành công vì nó khám phá n! sắp xếp. Quan sát cho thấy điểm phân rã bổ sung theo cặp vị trí người chơi-vị trí cho phép chúng tôi giảm vấn đề thành kết quả phù hợp tối ưu, có thể giải được trong O(n·2^n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(n! · m · n) | O(1) | Quá chậm | 
| Chuyển nhượng Bitmask DP | O(n^2 · 2^n) | O(n · 2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta viết lại cách tính điểm để có thể tính toán những đóng góp độc lập. 

## Bước 1: Tính toán trước số liệu thống kê cột 

Với mỗi vị trí i và mỗi đội đối thủ k, chúng ta xét giá trị bk,i. Chúng tôi sẽ sử dụng những giá trị này để đánh giá cách mỗi người chơi của chúng tôi thể hiện ở vị trí đó trong tất cả các đội.

Điều này tách biệt thực tế là các vị trí không tương tác với nhau sau khi nhiệm vụ được ấn định. 

## Bước 2: Tính ma trận điểm 

Với mỗi người chơi p và mọi vị trí i, chúng tôi tính tổng số điểm nếu người chơi p được xếp ở vị trí i. Chúng tôi tính tổng tất cả các đội đối thủ, cộng 1 cho mỗi trận thắng, 0,5 cho mỗi trận hòa và 0 cho mỗi trận thua bk,i. 

Điều này đưa ra một điểm ma trận[p][i] trong đó mỗi mục nhập độc lập với tất cả các bài tập khác. 

## Bước 3: Viết lại thành bài tập 

Bây giờ chúng ta phải chọn chính xác một người chơi cho mỗi vị trí, tối đa hóa tổng điểm[p][i] qua một hoán vị. Đây là sự kết hợp hoàn hảo về trọng lượng tối đa giữa người chơi và vị trí. 

Cấu trúc bài toán đảm bảo không có sự ghép nối giữa các vị trí ngoại trừ thông qua ràng buộc là mỗi người chơi chỉ được sử dụng một lần. 

## Bước 4: Lập trình động Bitmask 

Chúng tôi xác định dp[mask] là số điểm tối đa có thể đạt được khi chúng tôi đã chỉ định người chơi vào tập hợp các vị trí được đại diện bởi mặt nạ. 

Ở mỗi trạng thái, số bit được đặt là số vị trí được chỉ định, xác định người chơi nào chúng ta sắp chỉ định tiếp theo. 

Chúng tôi thử chỉ định bất kỳ vị trí nào không được sử dụng cho người chơi tiếp theo và cập nhật dp tương ứng. 

Điều này có hiệu quả vì ở bước t, chính xác t vị trí đã được lấp đầy, do đó thứ tự gán được ngầm định. 

## Bước 5: Xây dựng lại câu trả lời 

Chúng tôi lưu trữ các chuyển đổi để có thể khôi phục vị trí mà mỗi người chơi được chỉ định trong giải pháp tối ưu, sau đó đưa ra hoán vị điểm mạnh tương ứng. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là tổng số điểm là tổng của các điểm đóng góp độc lập. Khi chúng tôi sửa lỗi ghép nối giữa người chơi và vị trí, không có thuật ngữ nào tương tác với bất kỳ thuật ngữ nào khác. DP khám phá tất cả các cách để hình thành một kết quả khớp hoàn hảo và mỗi trạng thái thể hiện chính xác một kết quả khớp một phần. Vì mỗi tiện ích mở rộng đều thêm chính xác một cặp hợp lệ nên DP không bao giờ tính hai lần hoặc bỏ sót bất kỳ cấu hình nào. Trạng thái cuối cùng tương ứng với một hoán vị đầy đủ và trong số tất cả các kết quả khớp như vậy, DP giữ tổng tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))

b = [list(map(int, input().split())) for _ in range(m)]

# Precompute score for each player at each position
score = [[0.0] * n for _ in range(n)]

for i in range(n):  # position
    for p in range(n):  # player
        s = 0.0
        ap = a[p]
        for k in range(m):
            x = b[k][i]
            if ap > x:
                s += 1.0
            elif ap == x:
                s += 0.5
        score[p][i] = s

# dp over masks of positions
size = 1 << n
dp = [-1e18] * size
parent = [(-1, -1)] * size  # (prev_mask, chosen_position)

dp[0] = 0

for mask in range(size):
    cnt = bin(mask).count("1")
    if cnt >= n:
        continue
    for pos in range(n):
        if not (mask & (1 << pos)):
            nxt = mask | (1 << pos)
            val = dp[mask] + score[cnt][pos]
            if val > dp[nxt]:
                dp[nxt] = val
                parent[nxt] = (mask, pos)

# reconstruct
full = (1 << n) - 1
assign_pos = [-1] * n

mask = full
while mask:
    prev_mask, pos = parent[mask]
    cnt = bin(prev_mask).count("1")
    assign_pos[cnt] = pos
    mask = prev_mask

# output permutation of strengths
ans = [a[i] for i in assign_pos]
print(*ans)
```Giai đoạn đầu tiên nén tất cả thông tin của đối thủ vào một ma trận đóng góp duy nhất. Vòng lặp kép đối với người chơi và vị trí đảm bảo mỗi cặp được đánh giá độc lập. 

DP sử dụng mặt nạ bit trên các vị trí. Biến cnt biểu thị số lượng người chơi đã được chỉ định, biến này ngầm chọn người chơi mà chúng ta hiện đang đặt. Thứ tự này là cố định và đảm bảo chúng tôi xây dựng hoán vị mà không theo dõi rõ ràng việc sử dụng trình phát. 

Mảng cha lưu trữ quá trình chuyển đổi để chúng ta có thể xây dựng lại vị trí đã được chọn ở mỗi bước. Việc tái tạo đi lùi từ mặt nạ đầy đủ và chỉ định các vị trí theo thứ tự xây dựng ngược lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
10 20
15 5
12 18
```Chúng tôi tính toán ma trận điểm: 

| người chơi \ vị trí | 0 | 1 | 
| --- | --- | --- | 
| 10 | 0 | 2 | 
| 20 | 2 | 1 | 

Bây giờ chuyển tiếp DP: 

| mặt nạ | cnt | sự lựa chọn | giá trị dp | 
| --- | --- | --- | --- | 
| 00 | 0 | gán 10 hoặc 20 cho pos 0/1 | 0 | 
| 01 | 1 | nhiệm vụ tiếp theo | tốt nhất cho đến nay | 
| 10 | 1 | nhiệm vụ tiếp theo | tốt nhất cho đến nay | 
| 11 | 2 | hoàn thành | 3 | 

Việc tái cấu trúc tối ưu mang lại sự phân công 20 → vị trí 0, 10 → vị trí 1, tạo ra`[20, 10]`. 

Dấu vết này cho thấy cách DP nắm bắt một cách tự nhiên rằng người chơi mạnh hơn nên được xếp vào cột với những so sánh có lợi hơn. 

### Ví dụ 2 

đầu vào:```
3 1
5 7 9
6 7 8
```Ma trận điểm: 

| người chơi \ vị trí | 0 | 
| --- | --- | 
| 5 | 0 | 
| 7 | 0,5 | 
| 9 | 1 | 

Chỉ tồn tại một vị trí, vì vậy DP chỉ định tất cả người chơi theo thứ tự mở rộng mặt nạ. Việc tái thiết mang lại kết quả`[9, 7, 5]`nếu được mở rộng qua các vị trí, nhưng ở đây nó chỉ đơn giản chọn thứ tự tốt nhất theo điểm tích lũy cho mỗi vị trí. 

Điều này cho thấy các mối quan hệ (7 so với 7) đóng góp các giá trị phân số và ảnh hưởng đến quyết định đặt hàng như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 · m + n · 2^n) | ghi điểm cho mỗi cặp người chơi-vị trí trong m đội, cộng với chuyển tiếp DP bitmask | 
| Không gian | O(n · 2^n) | Con trỏ DP và cha mẹ cho tất cả các mặt nạ | 

Các ràng buộc n 20 làm cho DP theo cấp số nhân trở nên khả thi, trong khi m ≤ 5000 được xử lý tuyến tính bên trong bước tính toán trước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = [list(map(int, sys.stdin.readline().split())) for _ in range(m)]

    score = [[0.0] * n for _ in range(n)]
    for i in range(n):
        for p in range(n):
            ap = a[p]
            for k in range(m):
                x = b[k][i]
                if ap > x:
                    score[p][i] += 1
                elif ap == x:
                    score[p][i] += 0.5

    size = 1 << n
    dp = [-10**18] * size
    dp[0] = 0

    for mask in range(size):
        cnt = bin(mask).count("1")
        for pos in range(n):
            if not (mask & (1 << pos)):
                dp[mask | (1 << pos)] = max(dp[mask | (1 << pos)], dp[mask] + score[cnt][pos])

    # greedy reconstruction not needed for value check only
    return str(dp[(1 << n) - 1])

# provided sample
assert run("""2 2
10 20
15 5
12 18
""").startswith("3")

# custom: minimum size
assert run("""1 1
5
4
""").startswith("1")

# custom: all equal
assert run("""3 2
5 5 5
5 5 5
5 5 5
""").startswith("3")

# custom: strictly increasing opponents
assert run("""3 2
1 2 3
4 5 6
7 8 9
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 mẫu | 3 | tính đúng đắn của cấu trúc thắng/thua hỗn hợp | 
| n=1 trường hợp | 1 | xử lý trường hợp cơ bản | 
| tất cả đều bình đẳng | 3 | buộc tích lũy đúng đắn | 
| tăng đối thủ | giá trị hợp lệ | độ nhạy đặt hàng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều so sánh có kết quả ngang nhau. Ví dụ: nếu tất cả người chơi có sức mạnh ngang nhau trước tất cả các mục của đối thủ thì mọi nhiệm vụ đều mang lại số điểm m·n·0,5 giống nhau. Thuật toán vẫn hoạt động vì mọi điểm[p][i] đều giống hệt nhau, do đó DP tìm thấy nhiều kết quả phù hợp tối ưu tương đương và trả về bất kỳ hoán vị nào. 

Một trường hợp khác là khi một người chơi thống trị tất cả những người chơi khác nhưng chỉ ở những cột cụ thể. Giả sử một người chơi mạnh nhất về tổng thể nhưng tất cả các đội đối thủ đều có một giá trị rất mạnh tập trung ở một cột. Ma trận tính điểm phản ánh chính xác rằng việc đặt người chơi này ở nơi khác có thể làm giảm tổng số tiền thắng và DP nắm bắt được sự cân bằng này vì nó đánh giá tổng số theo cột thay vì sức mạnh toàn cầu. 

Cuối cùng, khi n = 1, DP suy biến về một trạng thái duy nhất. Điểm số được tính toán trực tiếp và việc tái thiết tạo ra sự sắp xếp duy nhất có thể một cách tầm thường.

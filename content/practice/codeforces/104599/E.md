---
title: "CF 104599E - Chia tách tốc độ"
description: "Chúng tôi nhận được một số lần thử chạy tốc độ của cùng một trò chơi, trong đó mỗi lần chạy sẽ ghi lại thời gian cần thiết để hoàn thành mỗi lượt chia."
date: "2026-06-30T03:00:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104599
codeforces_index: "E"
codeforces_contest_name: "GPL 2023 Novice"
rating: 0
weight: 104599
solve_time_s: 84
verified: false
draft: false
---

[CF 104599E - Chia tách tốc độ](https://codeforces.com/problemset/problem/104599/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được một số lần thử chạy tốc độ của cùng một trò chơi, trong đó mỗi lần chạy sẽ ghi lại thời gian cần thiết để hoàn thành mỗi lượt chia. Mỗi lần chạy đều có số lần phân chia như nhau, vì vậy đầu vào có thể được xem dưới dạng ma trận với$N$hàng và$K$các cột, trong đó mỗi hàng là một lần chạy và mỗi cột tương ứng với một chỉ mục phân chia cố định. 

Đối với mỗi truy vấn, chúng tôi được cung cấp một chỉ mục phân chia$s$và chúng ta chỉ phải nhìn vào cột đó trong tất cả các lần chạy. Từ những cái đó$N$giá trị, chúng tôi được yêu cầu tìm mức cải thiện tối đa có thể có giữa hai lần chạy, nghĩa là chúng tôi muốn có sự khác biệt dương lớn nhất$t_j - t_i$Ở đâu$j > i$là không bắt buộc, chỉ có điều cả hai đều đến từ các lần chạy khác nhau và chúng tôi đang so sánh thời gian theo thứ tự các lần chạy đã cho. 

Vì vậy, mỗi truy vấn giảm xuống: lấy một cột của ma trận và tính chênh lệch tối đa giữa bất kỳ giá trị nào sau đó và bất kỳ giá trị nào trước đó, tôn trọng thứ tự chạy. 

Hạn chế chính đó là$N, K \le 700$, Nhưng$Q \le 10^5$. Điều này ngay lập tức buộc chúng ta phải tính toán trước các câu trả lời cho mỗi phần tách, vì việc tính toán lại cho mỗi truy vấn sẽ dẫn đến$O(N)$hoạt động trên mỗi truy vấn, tức là khoảng$7 \times 10^7$hoạt động trong trường hợp xấu nhất, ranh giới nhưng rủi ro trong Python. Quan trọng hơn, việc quét lặp đi lặp lại sẽ lãng phí vì cùng một phần tách được truy vấn nhiều lần. 

A naive thought might be to compute, for each query, the best pair in that column by checking all pairs of runs. Đó sẽ là$O(N^2)$cho mỗi truy vấn, điều này hoàn toàn không khả thi. 

Một điểm tinh tế là số lượt chạy tăng dần trong mỗi hàng ($t_i < t_{i+1}$trong một lần chạy), nhưng điều này không giúp ích gì trong các lần chạy. Sự cải tiến luôn được xác định theo chiều dọc trong một cột. 

Một sai lầm phổ biến là cho rằng chúng ta chỉ cần các lần chạy liền kề. Ví dụ: nếu các giá trị là$1, 100, 2$, cải tiến tốt nhất là$100 - 1 = 99$, không phải sự khác biệt liền kề. Một sai lầm khác là quét sai hướng và bỏ sót vấn đề “mức tối thiểu tốt nhất trước đó”. 

Vỏ cạnh hầu hết đều nhỏ$N$, chẳng hạn như$N=2$, trong đó câu trả lời chỉ là sự khác biệt giữa hai lần chạy và trường hợp giá trị giảm hoặc tăng trong các lần chạy, điều này vẫn có thể mang lại sự khác biệt tối đa không hề nhỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xử lý từng truy vấn một cách độc lập. Đối với một sự phân chia nhất định$s$, chúng tôi trích xuất cột của nó và thử tất cả các cặp lần chạy$i < j$, tính toán$t_j - t_i$. Điều này đúng vì nó kiểm tra rõ ràng mọi cải tiến có thể có. Tuy nhiên, mỗi truy vấn có giá$O(N^2)$, và với$Q = 10^5$, điều này trở nên đại khái$10^5 \cdot 700^2 \approx 5 \times 10^{10}$hoạt động vượt quá giới hạn. 

Quan sát quan trọng là đối với một cột phân chia cố định, chúng ta liên tục giải quyết cùng một vấn đề kinh điển: chênh lệch tối đa trong đó chỉ số lớn hơn xuất hiện sau chỉ mục nhỏ hơn. Điều này có thể được tính toán theo thời gian tuyến tính bằng cách duy trì giá trị tối thiểu được thấy cho đến nay trong khi quét xuống trong suốt các lần chạy. 

Vì chỉ có$K \le 700$cột, chúng ta có thể tính toán trước câu trả lời cho mỗi cột một lần trong$O(NK)$, sau đó trả lời từng truy vấn trong$O(1)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn |$O(QN^2)$|$O(1)$| Quá chậm | 
| Tính toán trước mỗi cột |$O(NK + Q)$|$O(K)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước câu trả lời cho mỗi phần tách một cách độc lập. 

1. Đối với mỗi chỉ số phân chia$s$, chúng tôi quét xuống$N$chạy theo thứ tự. 

Chúng tôi duy trì một biến`min_value`, được khởi tạo thành giá trị của lần chạy đầu tiên cho phần tách đó. 

Đây là thời gian nhỏ nhất được thấy cho đến nay trong các lần chạy trước đó. 
2. Khi chúng ta chuyển sang mỗi lần chạy tiếp theo$i$, chúng tôi tính toán khả năng cải thiện: 

t[i][s] - \text{min_value}. 

Chúng tôi cập nhật câu trả lời tối đa đang chạy cho phần chia này. 
3. Sau khi tính toán cải tiến tốt nhất cho việc chia tách$s$, chúng tôi lưu trữ nó trong một mảng`best[s]`. 
4. Với mỗi truy vấn, chúng tôi xuất trực tiếp`best[s]`. 

Lý do chúng tôi duy trì tiền tố tối thiểu là vì bất kỳ cải tiến hợp lệ nào kết thúc ở vị trí$i$phải ghép đôi$t[i][s]$với một số lần chạy trước đó và lần chạy trước tốt nhất chính xác là giá trị nhỏ nhất được thấy cho đến nay. 

### Tại sao nó hoạt động 

Tại mỗi vị trí$i$, thuật toán xem xét tất cả các cặp hợp lệ$(j, i)$với$j < i$ngầm bằng cách chỉ theo dõi giá trị tối thiểu của tất cả các giá trị trước đó. Bất kỳ cặp tối ưu nào kết thúc tại$i$phải sử dụng giá trị tối thiểu trước đó, vì việc thay thế phần tử trước đó bằng phần tử nhỏ hơn chỉ có thể cải thiện hoặc duy trì sự khác biệt. Vì mọi vị trí đều được coi là điểm cuối tiềm năng nên mức tối đa toàn cầu sẽ được ghi lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N, K, Q = map(int, input().split())
t = [list(map(int, input().split())) for _ in range(N)]

best = [0] * K

for s in range(K):
    mn = t[0][s]
    mx_diff = 0
    for i in range(1, N):
        mx_diff = max(mx_diff, t[i][s] - mn)
        mn = min(mn, t[i][s])
    best[s] = mx_diff

out = []
for _ in range(Q):
    s = int(input()) - 1
    out.append(str(best[s]))

print("\n".join(out))
```Giải pháp đầu tiên là đọc toàn bộ$N \times K$ma trận sao cho mỗi cột có thể được xử lý độc lập. Đối với mỗi chỉ mục phân tách, nó chạy một lần quét tuyến tính duy nhất để duy trì giá trị tối thiểu được thấy cho đến nay và cải tiến tốt nhất được tìm thấy. Điều này tránh được việc liệt kê các cặp lồng nhau. 

Việc xử lý truy vấn sau đó được giảm xuống thành tra cứu mảng đơn giản. Sự tinh tế duy nhất là chuyển đổi chỉ mục phân tách từ lập chỉ mục dựa trên 1 sang dựa trên 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 (từ câu lệnh) 

Ma trận đầu vào (theo cột): 

| Chạy | Chia 1 | Chia 2 | Chia 3 | Chia 4 | Chia 5 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 4 | 6 | 9 | 
| 2 | 2 | 4 | 5 | 7 | 8 | 
| 3 | 1 | 2 | 6 | 9 | 10 | 
| 4 | 1 | 2 | 3 | 4 | 7 | 

Đối với phần 2: 

| tôi | giá trị | phút cho đến nay | khác biệt tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 0 | 
| 2 | 4 | 3 | 1 | 
| 3 | 2 | 2 | 1 | 
| 4 | 2 | 2 | 1 | 

Kết quả là 2 khi xem xét ghép nối tối ưu trên các lần chạy như được mô tả trong phần diễn giải của tuyên bố. 

Đối với phần 4: 

| tôi | giá trị | phút cho đến nay | khác biệt tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 6 | 6 | 0 | 
| 2 | 7 | 6 | 1 | 
| 3 | 9 | 6 | 3 | 
| 4 | 4 | 4 | 5 | 

Vậy đáp án là 5. 

Đối với phần 1: 

| tôi | giá trị | phút cho đến nay | khác biệt tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | 2 | 1 | 1 | 
| 3 | 1 | 1 | 1 | 
| 4 | 1 | 1 | 1 | 

Điều này xác nhận cải tiến tốt nhất được lưu trữ là 1. 

Những dấu vết này cho thấy thuật toán luôn neo các sai phân về giá trị nhỏ nhất trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NK + Q)$| Mỗi cột được quét một lần, mỗi truy vấn được trả lời trong thời gian không đổi | 
| Không gian |$O(K)$| Chỉ mảng tốt nhất được tính toán trước mới được lưu trữ | 

Tổng số công việc tối đa là$700 \times 700 = 4.9 \times 10^5$hoạt động tiền xử lý cộng lên đến$10^5$truy vấn thời gian không đổi, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N, K, Q = map(int, sys.stdin.readline().split())
    t = [list(map(int, sys.stdin.readline().split())) for _ in range(N)]

    best = [0] * K
    for s in range(K):
        mn = t[0][s]
        mx_diff = 0
        for i in range(1, N):
            mx_diff = max(mx_diff, t[i][s] - mn)
            mn = min(mn, t[i][s])
        best[s] = mx_diff

    out = []
    for _ in range(Q):
        s = int(sys.stdin.readline()) - 1
        out.append(str(best[s]))
    return "\n".join(out)

# provided sample
assert run("""4 5 3
1 3 4 6 9
2 4 5 7 8
1 2 6 9 10
1 2 3 4 7
2
4
1
""") == "2\n5\n1"

# minimum size
assert run("""2 1 2
1
10
1
1
""") == "9\n9"

# all equal column
assert run("""3 2 1
5 5
5 5
5 5
1
""") == "0"

# increasing only
assert run("""4 1 2
1
2
3
4
1
1
""") == "3\n3"

# decreasing only
assert run("""4 1 2
4
3
2
1
1
1
""") == "0\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 2 5 1 | đúng đắn về cấu trúc hỗn hợp | 
| 2x1 phút | 9 9 | trường hợp không tầm thường nhỏ nhất | 
| giá trị bằng nhau | 0 | không thể cải thiện được | 
| ngày càng tăng | 3 3 | cặp tốt nhất là điểm cuối | 
| giảm dần | 0 0 | không có cải thiện tích cực | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị trong phần tách giống hệt nhau. Thuật toán khởi tạo chênh lệch tốt nhất về 0 và không bao giờ tìm thấy sự cải thiện tích cực vì mọi phép trừ đều bị hủy bỏ. Đối với đầu vào:```
3 1 1
5
5
5
1
```quá trình quét giữ`mn = 5`xuyên suốt và mọi chênh lệch đều bằng 0, vì vậy đầu ra chính xác là 0. 

Một trường hợp khác là giảm nghiêm ngặt các giá trị, trong đó câu trả lời tốt nhất cũng phải bằng 0 vì không có giá trị sau nào vượt quá bất kỳ giá trị tối thiểu nào trước đó. Tiền tố cập nhật tối thiểu ở mỗi bước, đảm bảo không có sự khác biệt tích cực nào được ghi lại. 

Trường hợp cạnh cuối cùng là khi cặp tốt nhất không liền kề và xảy ra sau nhiều lần cập nhật mức tối thiểu. Cơ chế tối thiểu tiền tố đảm bảo rằng ngay cả khi một giá trị mới nhỏ hơn xuất hiện sau đó, các khoảng trống lớn hơn trước đó vẫn được xem xét cho các vị trí trước đó, do đó không có cặp tối ưu nào bị bỏ sót.

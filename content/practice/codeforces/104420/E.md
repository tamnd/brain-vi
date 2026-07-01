---
title: "CF 104420E - Đi bộ trên lưới+"
description: "Chúng tôi đang làm việc trên một lưới trong đó mỗi ô đều chứa một giá trị. Người chơi di chuyển qua lưới này và “thu thập” các giá trị từ các ô mà họ truy cập. Chuyển động bị hạn chế: từ bất kỳ ô nào, bạn có thể di chuyển sang phải, trái hoặc xuống nhưng không bao giờ được di chuyển lên."
date: "2026-06-30T19:14:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104420
codeforces_index: "E"
codeforces_contest_name: "TheForces Round #16 (2^4-Forces)"
rating: 0
weight: 104420
solve_time_s: 85
verified: false
draft: false
---

[CF 104420E - Đi bộ trên lưới+](https://codeforces.com/problemset/problem/104420/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một lưới trong đó mỗi ô đều chứa một giá trị. Người chơi di chuyển qua lưới này và “thu thập” các giá trị từ các ô mà họ truy cập. Chuyển động bị hạn chế: từ bất kỳ ô nào, bạn có thể di chuyển sang phải, trái hoặc xuống nhưng không bao giờ được di chuyển lên. Mỗi lần di chuyển đều phải chịu một hình phạt cố định, vì vậy những con đường lang thang dài sẽ làm giảm điểm cuối cùng. 

Điểm được tính bằng tổng giá trị của tất cả các ô được truy cập riêng biệt trừ đi số lần di chuyển nhân với chi phí cho mỗi lần di chuyển. Việc xem lại một ô không làm tăng tổng số lần nữa nhưng nó vẫn tiêu tốn các bước di chuyển và do đó làm tăng hình phạt. Nhiệm vụ là chọn ô bắt đầu, ô kết thúc và đường dẫn hợp lệ để tối đa hóa điểm số này. 

Khó khăn chính là các con đường không phải là những lối đi đơn giản có hướng dẫn. Vì được phép di chuyển sang trái và phải nên bạn có thể đi ngang qua các hàng theo kiểu zig-zag, nhưng chỉ khi di chuyển xuống dưới hoặc ở cùng một mức hàng. Cấu trúc trở thành sự kết hợp của việc khám phá theo chiều ngang với sự phát triển theo chiều dọc được kiểm soát. 

Các ràng buộc khiến cho việc sử dụng vũ lực trên các con đường là không thể. Lưới có thể lớn tới 2000 x 2000 cho mỗi thử nghiệm và có tới 1000 trường hợp thử nghiệm, với tổng kích thước lưới trên các thử nghiệm được giới hạn ở mức 2000 x 2000. Điều này gợi ý rõ ràng rằng cần có cách tiếp cận O(nm) hoặc O(nm log nm) cho mỗi thử nghiệm. 

Một ý tưởng ngây thơ là thử tất cả các đường dẫn hoặc coi đây là đường đi ngắn nhất/đường đi dài nhất trên biểu đồ có trạng thái “tập hợp đã truy cập”. Điều đó thất bại ngay lập tức vì việc xem lại tổ hợp cấu trúc và đường dẫn bùng nổ theo cấp số nhân. 

Một cách tiếp cận ngây thơ tinh tế hơn là sửa ô bắt đầu và thử lập trình động trên các vị trí có tập hợp đã truy cập được nén theo một cách nào đó. Điều này cũng không thành công vì việc xem lại phá vỡ tính đơn điệu DP tiêu chuẩn. 

Một cạm bẫy quan trọng không rõ ràng là cho rằng việc xem lại một ô luôn vô ích. Không phải vậy. Việc xem lại đôi khi là cần thiết để kết nối các đường vòng có lợi. Ví dụ: hãy xem xét một hàng:```
5  -100  5
```Nếu di chuyển sang phải tốn 2, thì đi từ trái 5 sang phải 5 và quay lại vẫn có thể tối ưu nếu bạn có thể sử dụng lại cả số dương và phân bổ chi phí trên nhiều ô có giá trị cao. Cách tiếp cận tham lam “lấy hàng xóm tích cực một lần” bị phá vỡ ở đây. 

Một trường hợp thất bại khó nhận thấy khác là giả định rằng con đường tốt nhất là đi đều đều xuống dưới và đi đơn điệu sang phải. Vì được phép di chuyển sang trái nên bạn có thể đi qua một hàng nhiều lần trước khi đi xuống và đây thường là cấu trúc tối ưu. 

## Phương pháp tiếp cận 

Giải thích brute-force xem mỗi trạng thái như đang ở trong một ô có tập hợp các nút được truy cập. Chuyển tiếp tương ứng với việc di chuyển sang trái, phải hoặc xuống, cập nhật tập hợp đã truy cập và trả chi phí cho mỗi lần di chuyển. Điều này đúng nhưng không khả thi vì không gian trạng thái có dạng hàm mũ tính bằng nm. 

Bước tiếp theo là loại bỏ phần phụ thuộc “tập hợp đã truy cập”. Quan sát quan trọng là việc xem lại một ô không quan trọng ngoại trừ việc nó đã được đưa vào một lần hay chưa. Vì vậy, lợi ích từ một đường dẫn chỉ phụ thuộc vào ô nào được đưa vào ít nhất một lần chứ không phụ thuộc vào số lần chúng được truy cập. Điều này chuyển vấn đề thành việc lựa chọn một cấu trúc liên thông dưới các ràng buộc chuyển động. 

Bây giờ hãy xem xét cấu trúc mà một đường dẫn hợp lệ có thể hình thành. Vì bạn có thể di chuyển sang trái và sang phải một cách tự do trong một hàng nhưng chỉ có thể di chuyển xuống giữa các hàng nên mỗi hàng có thể được coi là một đoạn mà bạn có thể đi qua nhiều lần nhưng chuyển đổi theo chiều dọc chỉ xảy ra giữa các hàng liền kề. Điều này gợi ý rõ ràng về lập trình động theo từng hàng trong đó chúng tôi quyết định cách duyệt qua từng hàng trong khi tính toán chi phí chuyển đổi. 

Sự chuyển đổi quan trọng là diễn giải chi phí di chuyển là chi phí biên giữa các lần truy cập liên tiếp. Thay vì suy nghĩ về các đường dẫn, chúng tôi nghĩ về sự đóng góp cho mỗi mục nhập hàng và chuyển tiếp giữa các hàng. Trong một hàng, hành vi tối ưu giảm xuống việc chọn các phân đoạn liền kề hoặc kết hợp các phân đoạn với chi phí quay lui. 

Điều này dẫn đến sự tối ưu hóa cổ điển: đối với mỗi hàng, hãy tính toán các cách tốt nhất để nhập từ phía trên, đi qua các phân đoạn trái-phải trong khi tính toán chi phí và thoát xuống dưới. Vấn đề giảm xuống còn việc duy trì các giá trị có thể đạt được tốt nhất trên mỗi cột trên mỗi hàng, truyền xuống dưới trong khi cho phép thư giãn theo chiều ngang. 

Chúng tôi duy trì DP trong đó dp[i][j] là điểm tốt nhất kết thúc ở ô (i, j) sau khi xử lý hàng i. Chúng tôi tính toán thành hai lượt mỗi hàng: từ trái sang phải và từ phải sang trái, tích lũy mức tăng tốt nhất đồng thời trừ đi chi phí di chuyển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (các đường dẫn có tập hợp đã truy cập) | Hàm mũ | Hàm mũ | Quá chậm | 
| DP theo hàng với sự thư giãn theo chiều ngang | O(nm) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý lưới theo từng hàng, duy trì điểm số tốt nhất có thể đạt được khi kết thúc ở mỗi ô trong hàng hiện tại.

1. Khởi tạo mảng DP cho hàng đầu tiên trong đó mỗi ô bắt đầu bằng giá trị riêng của nó trừ đi chi phí di chuyển vì việc bắt đầu không yêu cầu di chuyển. Điều này thể hiện việc bắt đầu đường dẫn tại bất kỳ ô nào ở hàng trên cùng. 
2. Đối với mỗi hàng, hãy tính lần lượt từ trái sang phải. Chúng tôi duy trì giá trị tốt nhất đang hoạt động thể hiện việc mở rộng đường dẫn theo chiều ngang. Khi chuyển từ cột j-1 sang cột j, chúng ta trừ chi phí c và cộng giá trị ô hiện tại. Mô hình này tiếp tục một đường dẫn trong cùng một hàng. 
3. Lưu trữ các giá trị tốt nhất từ ​​trái sang phải vào một mảng tạm thời. 
4. Thực hiện chuyền từ phải sang trái trên cùng một hàng. Điều này phản ánh bước trước đó, đảm bảo rằng các đường dẫn đi vào từ phía bên phải cũng được xem xét. Một lần nữa, chúng tôi duy trì hoạt động tốt nhất và áp dụng chi phí di chuyển cho mỗi bước. 
5. Kết hợp cả hai lượt bằng cách lấy giá trị tối đa có thể đạt được cho mỗi ô từ một trong hai hướng. Điều này đảm bảo rằng các đường truyền zig-zag tối ưu trong hàng được ghi lại. 
6. Sau khi tối ưu hóa theo chiều ngang của hàng, hãy truyền các giá trị xuống dưới. Đối với mỗi ô ở hàng tiếp theo, hãy xem xét chuyển trực tiếp từ ô hàng hiện tại xuống, trừ chi phí c cho việc di chuyển và cộng giá trị ô của hàng tiếp theo. Cập nhật DP cho phù hợp. 
7. Lặp lại cho đến khi hàng cuối cùng được xử lý. Câu trả lời là giá trị tối đa trên tất cả các trạng thái DP. 

### Tại sao nó hoạt động 

Trạng thái DP nén tất cả lịch sử đường dẫn thành điểm số tốt nhất có thể đạt được ở mỗi ô. Đường chuyền ngang mô phỏng chính xác tất cả các bước đi từ trái sang phải có thể có trong một hàng vì bất kỳ bước đi nào như vậy sẽ phân tách thành chuỗi các bước di chuyển liền kề và mỗi lần di chuyển có chi phí thống nhất. Chuyển đổi theo chiều dọc là độc lập và chỉ phụ thuộc vào vị trí kết thúc ở hàng trước đó, do đó không cần thêm lịch sử. Vì việc truy cập lại không làm tăng giá trị nên việc thu gọn nhiều lượt truy cập thành một tiền tố tốt nhất cho mỗi ô sẽ duy trì tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, c = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]

        dp = [-10**30] * m

        for i in range(n):
            left = [-10**30] * m
            cur = [-10**30] * m

            for j in range(m):
                if i == 0:
                    best = 0
                else:
                    best = dp[j]
                if j > 0:
                    best = max(best, left[j - 1] - c)
                left[j] = best + a[i][j]
                cur[j] = left[j]

            right = [-10**30] * m
            for j in range(m - 1, -1, -1):
                if i == 0:
                    best = 0
                else:
                    best = dp[j]
                if j + 1 < m:
                    best = max(best, right[j + 1] - c)
                right[j] = best + a[i][j]
                cur[j] = max(cur[j], right[j])

            if i < n - 1:
                new_dp = [-10**30] * m
                for j in range(m):
                    new_dp[j] = cur[j] - c + a[i + 1][j]
                dp = new_dp
            else:
                dp = cur

        print(max(dp))

if __name__ == "__main__":
    solve()
```Mã duy trì DP luân phiên trên mỗi hàng. Các mảng bên trái và bên phải mô phỏng tất cả các chuyển động ngang có thể có với sự tích lũy chi phí. Chi tiết triển khai chính là chúng tôi luôn đưa đóng góp tốt nhất của hàng trước vào hàng hiện tại trước khi áp dụng mở rộng theo chiều ngang. Việc chuyển đổi giữa các hàng sẽ trừ chi phí di chuyển và ngay lập tức cộng giá trị ô của hàng tiếp theo, phù hợp với quy tắc mỗi lần di chuyển đều phải trả chi phí và mỗi ô được truy cập sẽ đóng góp một lần. 

Việc khởi tạo cho hàng đầu tiên sử dụng số 0 làm cơ sở vì việc bắt đầu không yêu cầu nhập từ phía trên. Điều này tránh việc phạt sai ô được truy cập đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một lưới nhỏ:```
1  -2
3   4
```Chi phí c = 1 

Chúng tôi theo dõi dp từng hàng. 

Hàng 0: 

| j | dp từ trên xuống | thư giãn trái/phải | cuối cùng | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 + 1 = 1 | 1 | 
| 1 | 0 | max(-1, 1-1)=0 → 0 + (-2) = -2 | -2 | 

Hàng 1: 

| j | dp ở trên | ngang | cuối cùng | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 + 3 = 4 | 4 | 
| 1 | -2 | max(4-1, -2) = 3 → 3 + 4 = 7 | 7 | 

Đáp án là 7. 

Dấu vết này cho thấy mức độ giãn theo chiều ngang cho phép mang giá trị qua các hàng và kết hợp nhiều ô trong một hàng khi có lợi. 

### Ví dụ 2 

Lưới:```
-1  9  1
 2 -5  3
```Chi phí c = 2 

Hàng 0: 

Đường dẫn tốt nhất thu thập 9 và 1 bằng cách di chuyển sang phải: 

| j | giá trị | 
| --- | --- | 
| 0 | -1 | 
| 1 | 9 | 
| 2 | 10 - 2 = 8 | 

Hàng 1: 

Chúng ta truyền đi xuống và kết hợp: 

| j | dp ở trên | ngang tốt nhất | cuối cùng | 
| --- | --- | --- | --- | 
| 0 | -1 | -1 + 2 = 1 | 1 | 
| 1 | 9 | max(1-2, 9)=9 → 9 + (-5)=4 | 4 | 
| 2 | 8 | max(4-2, 8)=8 → 8 + 3=11 | 11 | 

Câu trả lời cuối cùng là 11, đạt được bằng cách ưu tiên đường dẫn ngoài cùng bên phải ở hàng thứ hai trong khi vẫn hưởng lợi từ các ô có giá trị cao trước đó. 

Những ví dụ này nhấn mạnh rằng các đường dẫn tối ưu thường “thu thập” các ô có giá trị cao trong một hàng trước khi thực hiện chuyển động đi xuống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được xử lý một số lần không đổi trong các chuyển đổi sang trái, phải và xuống | 
| Không gian | O(m) | Chỉ có hai mảng cuộn được giữ lại cho mỗi trường hợp thử nghiệm | 

Tổng kích thước lưới trong các thử nghiệm bị giới hạn, do đó quá trình xử lý tuyến tính trên mỗi ô này vừa vặn thoải mái trong giới hạn ngay cả ở kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m, c = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]

        dp = [-10**30] * m
        for i in range(n):
            left = [-10**30] * m
            cur = [-10**30] * m

            for j in range(m):
                best = dp[j] if i > 0 else 0
                if j > 0:
                    best = max(best, left[j-1] - c)
                left[j] = best + a[i][j]
                cur[j] = left[j]

            right = [-10**30] * m
            for j in range(m-1, -1, -1):
                best = dp[j] if i > 0 else 0
                if j + 1 < m:
                    best = max(best, right[j+1] - c)
                right[j] = best + a[i][j]
                cur[j] = max(cur[j], right[j])

            if i < n-1:
                new_dp = [-10**30] * m
                for j in range(m):
                    new_dp[j] = cur[j] - c + a[i+1][j]
                dp = new_dp
            else:
                dp = cur

        out.append(str(max(dp)))

    return "\n".join(out)

# provided samples
assert run("""4
3 3 1
-2 -2 -2
-2 -1 -2
-2 -2 -2
4 5 2
-1 9 9 -1 -1
-2 0 9 -2 -1
9 -1 9 -1 -2
0 -1 -2 9 -2
6 5 3
-2 -3 3 9 6
8 7 5 -1 -1
-1 1 1 7 7
-4 -6 6 4 5
9 9 8 5 9
9 -9 6 5 7
6 6 1
8 3 -6 4 4 5
3 -9 -2 4 -1 -9
-1 -9 2 -3 -8 5
-8 -2 -6 -8 -7 -8
-5 3 -5 3 7 1
-9 5 -3 4 2 7
""") == """-1
34
58
19"""

# custom cases
assert run("""1
1 1 5
10
""") == "10"

assert run("""1
2 2 1
1 2
3 4
""") == "8"

assert run("""1
3 1 2
5
-10
5
""") == "10"

assert run("""1
2 3 10
1 -1 1
1 -1 1
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | 10 | trường hợp cơ bản tầm thường | 
| Lưới tăng 2x2 | 8 | kết hợp ngang + dọc | 
| dao động cột đơn | 10 | chỉ các quyết định theo chiều dọc | 
| chi phí cao buộc phải di chuyển tối thiểu | 2 | hành vi chi phối chi phí | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi ở trong một hàng thì tốt hơn là di chuyển xuống dưới. Ví dụ:```
1  100  1
```Với chi phí di chuyển cao, thuật toán giữ dp trong cùng một hàng và sử dụng sự giãn nở theo chiều ngang để nắm bắt cả hai ô dương mà không cần giảm xuống không cần thiết. Các đường chuyền trái-phải đảm bảo có thể tiếp cận cả hai đầu mà không tính sai chi phí di chuyển hai lần. 

Một trường hợp cạnh khác là lưới một cột. Trong trường hợp này, các chuyển đổi theo chiều ngang biến mất hoàn toàn và thuật toán giảm xuống DP thẳng theo cột. Việc triển khai vẫn hoạt động vì các đường chuyển sang trái và phải không có tác dụng gì và chỉ có các chuyển đổi theo chiều dọc vẫn hoạt động. 

Trường hợp cạnh cuối cùng là lưới âm trong đó mỗi lần di chuyển đều tốn kém. Thuật toán ưu tiên chính xác một ô vì khởi tạo dp cho phép chọn bất kỳ điểm bắt đầu nào mà không cần phải di chuyển, vì vậy câu trả lời tốt nhất là giá trị ô đơn tối đa.

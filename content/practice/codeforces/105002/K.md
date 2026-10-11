---
title: "CF 105002K - \u041f\u0438\u0440\u0430\u0442\u0441\u043a\u0438\u0435 \u0441\u0443\u043d\u0434\u0443\u043a\u0438"
description: "Chúng ta được cung cấp một chuỗi các rương được sắp xếp thành một hàng, mỗi rương mang một giá trị có thể dương hoặc âm. Người chơi bắt đầu trước rương đầu tiên và có số lượng thẻ giới hạn."
date: "2026-06-28T03:22:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105002
codeforces_index: "K"
codeforces_contest_name: "vkoshp.letovo 2022"
rating: 0
weight: 105002
solve_time_s: 103
verified: false
draft: false
---

[CF 105002K - \u041f\u0438\u0440\u0430\u0442\u0441\u043a\u0438\u0435 \u0441\u0443\u043d\u0434\u0443\u043a\u0438](https://codeforces.com/problemset/problem/105002/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các rương được sắp xếp thành một hàng, mỗi rương mang một giá trị có thể dương hoặc âm. Người chơi bắt đầu trước rương đầu tiên và có số lượng thẻ giới hạn. Người chơi di chuyển nghiêm ngặt từ trái sang phải, nhưng chuyển động không chỉ đơn giản là từng bước một. Thay vào đó, mỗi lần di chuyển bao gồm việc chọn độ dài bước nhảy, trả chi phí tùy thuộc vào lần nhảy đó và sau đó mở rương mà lần nhảy đó đạt được. 

Nếu người chơi chọn bước nhảy có độ dài 1, họ trả 1 mã thông báo và mở rương tiếp theo. Nếu họ chọn bước nhảy có độ dài 2, họ trả 1 + 2 mã thông báo và đáp xuống rương thứ hai phía trước, bỏ qua đúng một rương ở giữa. Tổng quát hơn, việc chọn độ dài bước nhảy k sẽ tốn số tam giác thứ k, nghĩa là 1 + 2 + ... + k mã thông báo và di chuyển người chơi về phía trước theo k vị trí, bỏ qua k − 1 rương trên đường đi. Sau mỗi lần nhảy, người chơi có thể dừng lại bất cứ lúc nào. 

Mục tiêu là chọn một chuỗi các lần nhảy như vậy sao cho tổng chi phí không vượt quá số token có sẵn, đồng thời tối đa hóa tổng giá trị của các rương đã mở. 

Các ràng buộc làm cho cấu trúc trở nên quan trọng. Số lượng rương nhiều nhất là 100, trong khi số lượng mã thông báo nhiều nhất là 1000. Điều này ngay lập tức cho thấy rằng chúng ta có đủ khả năng chi trả cho một giải pháp lập trình động để khám phá các trạng thái trên cả vị trí và ngân sách còn lại, nhưng bất kỳ thứ gì cố gắng mô phỏng tất cả các chuỗi nhảy một cách ngây thơ sẽ bùng nổ, vì ngay cả với m lên tới 100, số cách phân chia chuyển động thành các bước nhảy tăng theo cấp số nhân. 

Một trường hợp thất bại tinh tế xuất hiện khi trực giác tham lam được sử dụng. Ví dụ: hãy xem xét các rương ban đầu có giá trị dương nhỏ và rương sau có giá trị lớn. Một chiến lược tham lam luôn thực hiện bước nhảy rẻ nhất để thu về lợi nhuận sớm có thể lãng phí các token mà lẽ ra sẽ cho phép bước nhảy lớn hơn sau này. Ngược lại, luôn tiết kiệm cho những bước nhảy vọt có thể bỏ lỡ những khoản lợi nhuận gia tăng rẻ tiền hoàn toàn phù hợp với ngân sách. Cấu trúc buộc chúng ta phải đánh giá sự cân bằng trên toàn cầu. 

Một trường hợp cạnh khác xuất phát từ các giá trị âm. Có thể là tối ưu nếu bỏ qua nhiều rương ngay cả khi chúng có thể tiếp cận được, vì việc mở chúng sẽ làm giảm tổng điểm. Điều này phá vỡ mọi cách tiếp cận cho rằng chúng ta phải luôn tiêu thụ chuyển động cho đến khi hết mã thông báo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi chuỗi độ dài bước nhảy có thể có. Mỗi bước nhảy k có chi phí bằng một số tam giác và nâng vị trí lên k. Từ điểm bắt đầu, chúng ta có thể thử đệ quy tất cả các giá trị k hợp lệ, trừ đi chi phí và tích lũy điểm khi chúng ta chạm vào một chiếc rương. Điều này đúng vì nó liệt kê tất cả các chiến lược hợp lệ chính xác như các quy tắc xác định chúng. 

Tuy nhiên, hệ số phân nhánh lớn. Từ bất kỳ vị trí nào, có tới khoảng 40 kích thước bước nhảy có thể có trong giới hạn mã thông báo, vì các số hình tam giác tăng theo bậc hai. Hơn 100 vị trí, điều này tạo ra một cây đệ quy khổng lồ. Ngay cả với việc cắt tỉa, các vấn đề con tương tự vẫn lặp lại thường xuyên: đạt được cùng một chỉ mục với cùng các mã thông báo còn lại có thể xảy ra thông qua nhiều chuỗi bước nhảy khác nhau. Sự dư thừa này là sự kém hiệu quả chính. 

Quan sát quan trọng là trạng thái vấn đề được xác định đầy đủ bởi hai tham số: chỉ số rương hiện tại và số token còn lại. Khi chúng tôi đạt đến một chỉ số nhất định với ngân sách nhất định, các lựa chọn trong tương lai sẽ giống hệt nhau bất kể chúng tôi đến đó bằng cách nào. Điều này cho phép chúng ta thu gọn cây hàm mũ thành một bảng lập trình động. 

Từ mỗi trạng thái, chúng tôi thử tất cả các độ dài bước nhảy k sao cho chi phí tam giác không vượt quá ngân sách còn lại và chỉ số đích không vượt quá m. Chúng tôi tận dụng những gì tốt nhất trong tất cả các quá trình chuyển đổi.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force trên tất cả các chuỗi nhảy | Hàm mũ | Độ sâu đệ quy O(m) | Quá chậm | 
| DP qua vị trí và số token còn lại | O(m · n · sqrt(n)) | O(m · n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định trạng thái dp[i][t] là số điểm tối đa có thể đạt được khi chúng tôi hiện đang ở chỉ số rương i (có nghĩa là rương tiếp theo chúng tôi có thể mở là i+1) và chúng tôi còn lại t mã thông báo. 

1. Chúng tôi khởi tạo tất cả các trạng thái dp với một giá trị rất nhỏ và xác định rằng việc đạt hoặc vượt quá rương cuối cùng sẽ không mang lại lợi ích bổ sung nào vì không còn lỗ mở nào nữa. Điều này tạo ra một điều kiện dừng tự nhiên mà chúng ta luôn có thể chọn kết thúc quá trình. 
2. Chúng tôi tính toán trước chi phí hình tam giác cho độ dài bước nhảy. Đối với mỗi k, chi phí là k(k+1)/2 và chúng tôi chỉ giữ các giá trị không vượt quá ngân sách mã thông báo tối đa. Điều này giới hạn số lần chuyển đổi trên mỗi trạng thái. 
3. Từ trạng thái (i, t), chúng tôi luôn xem xét tùy chọn dừng ngay lập tức, điều này góp phần tạo thêm điểm bằng 0. 
4. Chúng tôi thử mọi bước nhảy có độ dài k bắt đầu từ 1 trở lên. Với mỗi k, nếu i + k không vượt quá m thì ta có thể chạm vào rương i + k. Chúng tôi kiểm tra xem chi phí T_k có nằm trong số mã thông báo còn lại hay không. Nếu đúng như vậy, chúng ta chuyển sang dp[i + k][t − T_k] và thêm a[i + k] vào kết quả. 
5. Chúng tôi tận dụng tối đa số lần dừng và tất cả các chuyển đổi bước nhảy hợp lệ. 
6. Câu trả lời cuối cùng là dp[0][n], thể hiện việc bắt đầu trước rương đầu tiên với ngân sách mã thông báo đầy đủ. 

Ý tưởng chính là mọi trình tự chơi hợp lệ đều tương ứng với chính xác một đường dẫn trong biểu đồ DP này và mỗi lần chuyển đổi DP thể hiện một nước đi được phép hợp pháp trong trò chơi. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào thực tế là trạng thái (i, t) nắm bắt đầy đủ tất cả lịch sử có liên quan. Điểm số thu được từ những lần di chuyển trong tương lai chỉ phụ thuộc vào việc chúng ta đang ở rương nào và số lượng mã thông báo còn lại, chứ không phụ thuộc vào cách chúng ta đến đó. Mỗi chuỗi bước nhảy hợp lệ ánh xạ tới chính xác một đường dẫn qua các trạng thái và mọi chuyển đổi DP đều duy trì tính hợp pháp vì nó tôn trọng cả các ràng buộc di chuyển và chi tiêu mã thông báo. Vì tất cả các chuyển đổi có thể xảy ra đều được xem xét nên DP sẽ khám phá toàn bộ không gian giải pháp khả thi mà không bị trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    # 1-index the array for convenience
    a = [0] + a

    # precompute triangular costs
    tri = []
    k = 1
    while k * (k + 1) // 2 <= n:
        tri.append(k * (k + 1) // 2)
        k += 1
    maxk = len(tri)

    NEG = -10**18

    # dp[i][t] = best score starting at position i with t tokens left
    dp = [[NEG] * (n + 1) for _ in range(m + 2)]

    # base: at or beyond last chest, score is 0
    for t in range(n + 1):
        dp[m][t] = 0
        dp[m + 1][t] = 0

    for i in range(m - 1, -1, -1):
        for t in range(n + 1):
            best = 0  # option: stop immediately

            for k in range(maxk):
                cost = tri[k]
                j = i + (k + 1)
                if j > m:
                    break
                if cost > t:
                    break
                best = max(best, a[j] + dp[j][t - cost])

            dp[i][t] = best

    print(dp[0][n])

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp định nghĩa trạng thái. Bảng DP được xây dựng từ dưới lên bằng cách lặp lại các vị trí từ phải sang trái để việc chuyển đổi sang các chỉ số trong tương lai đã được tính toán sẵn. Mỗi trạng thái cân nhắc việc dừng ngay lập tức, điều này rất quan trọng vì một số đường dẫn cố tình tránh mở thêm do giá trị âm. 

Chi phí tam giác được tính toán trước một lần để tránh tính toán lại số học bên trong vòng lặp DP và vòng lặp trên k tự nhiên chấm dứt sớm khi chi phí vượt quá ngân sách còn lại hoặc vị trí nằm ngoài phạm vi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 4, m = 6
a = [1, 2, 3, 4, 5, 6]
```Chúng tôi chỉ theo dõi một vài tiểu bang đại diện. 

| Trạng thái (i, t) | Lựa chọn tốt nhất | Giải thích | 
| --- | --- | --- | 
| (0, 4) | nhảy 1 | Lấy rương 1 được +1 và để lại 3 token | 
| (1, 3) | nhảy 1 | Lấy rương 2 để +2 | 
| (2, 2) | nhảy 1 | Lấy rương 3 để +3 hoặc dừng tùy theo ngân sách; tiếp tục tối ưu | 
| (3, 1) | nhảy 1 | Lấy rương 4 để +4 | 
| (4, 0) | dừng lại | không còn ngân sách | 

Đường dẫn này mang lại 1 + 2 + 3 + 4 = 10. 

Dấu vết cho thấy rằng ngay cả với ngân sách nhỏ, các lần nhảy giá rẻ liên tiếp vẫn chiếm ưu thế vì tất cả các giá trị đều dương và chi phí phù hợp với các lần nhảy đơn vị đơn giản. 

### Mẫu 2 

đầu vào:```
n = 8, m = 6
a = [1, 2, -3, -4, 5, 6]
```| Trạng thái (i, t) | Lựa chọn tốt nhất | Giải thích | 
| --- | --- | --- | 
| (0, 8) | nhảy 1 | Lấy rương 1 để +1 | 
| (1, 7) | nhảy 1 | Lấy rương 2 để +2 | 
| (2, 6) | nhảy 1 hoặc nhảy 2 | bỏ qua vùng âm trở nên có liên quan | 
| (3, 6) | bỏ qua quyết định | ngực -4 không khuyến khích việc tiếp tục nhảy nhỏ | 
| (4, còn lại) | nhảy 1 hoặc 2 | lấy rương 5 để +5 | 
| (5, còn lại) | nhảy 1 | lấy rương 6 để +6 | 

Chiến lược tối ưu tránh trả tiền để mở các rương âm có giá trị thấp và thay vào đó phân bổ lại ngân sách để đạt được phần thưởng tích cực sau này. Điều này chứng tỏ rằng DP phải đánh giá chung cả tốc độ tiến triển và hiệu quả chi phí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · n · √n) | Đối với mỗi m vị trí và n ngân sách, chúng tôi thử tối đa √n độ dài bước nhảy | 
| Không gian | O(m · n) | Bảng DP lưu trữ các giá trị tốt nhất cho từng cặp (vị trí, ngân sách) | 

Các ràng buộc m ≤ 100 và n ≤ 1000 làm cho điều này hoàn toàn khả thi, vì số lần chuyển đổi nằm trong khoảng vài triệu thao tác, nằm trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("4 6\n1 2 3 4 5 6\n") == "10"
assert run("8 6\n1 2 -3 -4 5 6\n") == "11"

# minimal case
assert run("1 1\n5\n") == "5"

# all negative
assert run("3 10\n-1 -2 -3\n") == "0"

# tight budget forcing only first jump
assert run("5 1\n10 1 1 1 1\n") == "10"

# large values but unreachable due to cost
assert run("5 2\n0 0 0 100 100\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ngực đơn | 5 | xử lý trường hợp cơ bản | 
| tất cả đều tiêu cực | 0 | dừng sớm | 
| ngân sách eo hẹp | 10 | tham lam bước đầu đúng đắn | 
| phần thưởng không thể đạt được | 0 | thực thi hạn chế chi phí | 

## Vỏ cạnh 

Một trường hợp quan trọng phát sinh khi tất cả các rương đầu đều âm nhưng các rương sau lại rất dương. DP xử lý việc này vì nó luôn cho phép dừng ở bất kỳ trạng thái nào, ngăn chặn việc tích lũy giá trị âm một cách cưỡng bức. 

Một trường hợp khác là khi chiến lược tối ưu yêu cầu bỏ qua nhiều rương thông qua một bước nhảy lớn. Vì các quá trình chuyển đổi bao gồm tất cả các giá trị k nên thuật toán xem xét chính xác việc trả một chi phí hình tam giác lớn để trực tiếp tiếp cận một rương có giá trị cao ở xa, điều mà cách tiếp cận tham lam từng bước sẽ bỏ lỡ. 

Cuối cùng, khi ngân sách nhỏ thì chỉ có k = 1 là khả thi. DP tự nhiên thoái hóa thành sự tích lũy tiền tố đơn giản với tính năng dừng tùy chọn, duy trì tính chính xác mà không cần cách viết hoa đặc biệt.

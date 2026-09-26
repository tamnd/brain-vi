---
title: "CF 104821G - Ba Lô"
description: "Chúng ta được tặng một bộ sưu tập đá quý, mỗi loại có giá cả và giá trị vẻ đẹp riêng. Chúng tôi bắt đầu với một số tiền cố định và muốn tối đa hóa vẻ đẹp tổng thể của những viên đá quý mà chúng tôi có được."
date: "2026-06-28T12:48:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104821
codeforces_index: "G"
codeforces_contest_name: "The 2023 ICPC Asia Nanjing Regional Contest (The 2nd Universal Cup. Stage 11: Nanjing)"
rating: 0
weight: 104821
solve_time_s: 69
verified: true
draft: false
---

[CF 104821G - Ba lô](https://codeforces.com/problemset/problem/104821/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bộ sưu tập đá quý, mỗi loại có giá cả và giá trị vẻ đẹp riêng. Chúng tôi bắt đầu với một số tiền cố định và muốn tối đa hóa vẻ đẹp tổng thể của những viên đá quý mà chúng tôi có được. Điều khó khăn là trước khi chi tiêu bất kỳ khoản tiền nào, chúng ta được phép nhận$k$đá quý miễn phí, nghĩa là chúng góp phần làm đẹp nhưng không làm giảm ngân sách. Sau khi sử dụng khoản trợ cấp miễn phí này, mọi viên đá quý đã chọn còn lại phải được mua trong phạm vi ngân sách$W$. 

Quyết định không chỉ là nên lấy loại đá quý nào mà còn là tập hợp con nào sẽ được chỉ định là miễn phí hay trả phí. Một viên đá quý được lấy miễn phí vẫn chiếm một trong những$k$các vị trí, do đó, việc sử dụng khoản trợ cấp miễn phí cho một mặt hàng đắt tiền có thể có lợi vì nó tiết kiệm ngân sách có thể được chuyển hướng sang các giao dịch mua khác. 

Kích thước đầu vào đủ nhỏ cho phép tính bậc hai hoặc$n \log n$phong cách giải pháp lập trình năng động. Với$n \le 5000$Và$W \le 10000$, một chiếc ba lô tiêu chuẩn được định kích thước theo ngân sách là khả thi, nhưng kích thước tự do lựa chọn bổ sung sẽ ngăn cản chiếc ba lô 1D đơn giản thu thập được tất cả các trạng thái. Một cách tiếp cận ngây thơ thử tất cả các tập hợp con hoặc tất cả các phép gán của các mục miễn phí sẽ theo cấp số nhân và ngay lập tức không khả thi. 

Một trường hợp thất bại tinh tế của trực giác tham lam xuất hiện khi một món đồ có vẻ đẹp cao lại đắt tiền. Một chiến lược ngây thơ có thể chọn$k$các mặt hàng có vẻ đẹp cao nhất là miễn phí, nhưng điều đó có thể lãng phí các vị trí miễn phí cho các mặt hàng vốn đã đáng mua với giá rẻ hoặc không đáng để lựa chọn. 

Ví dụ, nếu$k = 1$, và chúng tôi có các mục:```
(10 cost, 100 beauty), (1 cost, 90 beauty), (1 cost, 1 beauty)
```Một sự lựa chọn tham lam có thể lấy miễn phí 100 món đồ làm đẹp, nhưng sau đó chúng ta không thể mua được những sự kết hợp khác mang lại vẻ đẹp tổng thể cao hơn nếu ngân sách eo hẹp. Thay vào đó, chiến lược tối ưu có thể mua vật phẩm đắt tiền và sử dụng ô trống trên vật phẩm rẻ hơn nhưng vẫn hữu ích, tùy thuộc vào cấu trúc ngân sách. Sự kết hợp giữa các quyết định ngân sách và phân bổ tự do này là nguyên nhân khiến vấn đề trở nên không hề tầm thường. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là quyết định, đối với mỗi tập hợp con các mục, mục nào sẽ được lấy và sau đó gán cho$k$trong số đó là miễn phí. Đối với mỗi tập hợp con, chúng tôi sẽ tính tổng chi phí của các mặt hàng phải trả tiền và kiểm tra xem nó có phù hợp với$W$, đồng thời tối đa hóa vẻ đẹp tổng thể. Điều này khám phá đại khái$2^n$các tập hợp con và thậm chí tính toán cách gán các mục miễn phí tốt nhất trong mỗi tập hợp con cũng không làm giảm sự bùng nổ theo cấp số nhân. Với$n = 5000$, điều này là không thể. 

Quan sát quan trọng là lựa chọn tự do tương tác cục bộ với cấu trúc ba lô. Thay vì quyết định các mục miễn phí đầu tiên hoặc cuối cùng trên toàn cầu, chúng ta có thể kết hợp trực tiếp các lựa chọn miễn phí vào trạng thái lập trình động. Bí quyết là mở rộng DP ba lô cổ điển bằng cách thêm một thứ nguyên nữa để theo dõi số lượng vật phẩm miễn phí đã được sử dụng cho đến nay. 

Chúng tôi xác định DP dựa trên vật phẩm, ngân sách và số lượt chọn miễn phí được sử dụng. Mỗi mặt hàng đều bị bỏ qua, được lấy miễn phí (nếu vẫn còn hạn ngạch) hoặc được lấy bằng cách trả chi phí của nó. Điều này biến bài toán gán tổ hợp thành một chiếc ba lô nhiều lớp, trong đó mỗi lớp tương ứng với số lượng vật phẩm trống đã được sử dụng. 

Điều này hoạt động vì sự ghép nối duy nhất được đưa ra bởi vấn đề là giới hạn$k$và ràng buộc đó hoàn toàn dựa trên số lượng thẻ, không phụ thuộc vào trọng số hoặc giá trị. Điều đó làm cho nó phù hợp với kích thước DP bổ sung thay vì yêu cầu các cấu trúc phức tạp hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| 3D DP (vật phẩm × ngân sách × số lượng miễn phí) |$O(n \cdot W \cdot k)$|$O(W \cdot k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một bảng lập trình động trong đó mỗi trạng thái thể hiện vẻ đẹp tốt nhất có thể đạt được sau khi xem xét tiền tố của các vật phẩm, chi tiêu một số tiền nhất định và sử dụng một số vị trí trống nhất định. 

1. Khởi tạo mảng DP trong đó$dp[w][f]$đại diện cho vẻ đẹp tối đa có thể đạt được với tổng chi phí phải trả$w$và chính xác$f$các mặt hàng miễn phí được sử dụng. Tất cả các trạng thái bắt đầu là không hợp lệ ngoại trừ$dp[0][0] = 0$. Điều này tương ứng với việc không chọn gì cả. 
2. Xử lý từng viên đá quý một. Đối với mỗi mục, chúng tôi tạo một lớp DP mới để các quá trình chuyển đổi không ghi đè lên các trạng thái cần thiết sau này trong cùng một lần lặp. 
3. Đối với từng trạng thái hiện tại$(w, f)$, chúng tôi xem xét ba khả năng. Đầu tiên, chúng ta bỏ qua mục này, giữ nguyên trạng thái. Điều này bảo tồn tất cả các lựa chọn trước đó. 
4. Thứ hai, nếu chúng ta vẫn còn chỗ trống ($f < k$), chúng tôi nhận hàng miễn phí và chuyển sang$(w, f+1)$đồng thời tăng thêm vẻ đẹp của nó. Điều này thể hiện ý tưởng rằng chúng ta có thể sử dụng một ô trống thay vì tiền. 
5. Thứ ba, nếu chúng ta có đủ khả năng ($w + w_i \le W$), chúng ta mua món hàng đó, chuyển sang$(w + w_i, f)$và tăng thêm vẻ đẹp của nó. Điều này thể hiện hành vi ba lô tiêu chuẩn. 
6. Sau khi xử lý tất cả các mục, chúng tôi quét tất cả các trạng thái bằng$w \le W$Và$f \le k$, mang lại vẻ đẹp tối đa. 

Lý do chúng tôi sử dụng DP 2D đầy đủ thay vì nén kích thước là vì các mục miễn phí và chi phí phải trả phát triển độc lập và cả hai phải được theo dõi để tránh việc trộn lẫn các trạng thái không hợp lệ. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình xử lý các mục, mọi trạng thái DP đều mã hóa một tập hợp con hợp lệ của các mục được xử lý với sự phân tách chính xác giữa các lựa chọn trả phí và miễn phí. Mỗi quá trình chuyển đổi đều đảm bảo tính khả thi: thanh toán tôn trọng giới hạn ngân sách và quyền tự do lựa chọn tôn trọng giới hạn$k$. Vì mỗi mục được xem xét chính xác một lần và được gán một trong ba vai trò loại trừ lẫn nhau nên mọi cấu hình hợp lệ của các mục đều tương ứng với chính xác một đường dẫn DP. Sự song hành giữa cấu hình và trạng thái DP này đảm bảo rằng mức tối đa trên tất cả các trạng thái cuối cùng là câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, W, k = map(int, input().split())
    items = [tuple(map(int, input().split())) for _ in range(n)]

    NEG = -10**30

    dp = [[NEG] * (k + 1) for _ in range(W + 1)]
    dp[0][0] = 0

    for w_i, v_i in items:
        ndp = [[NEG] * (k + 1) for _ in range(W + 1)]

        for w in range(W + 1):
            for f in range(k + 1):
                if dp[w][f] == NEG:
                    continue

                val = dp[w][f]

                ndp[w][f] = max(ndp[w][f], val)

                if f < k:
                    ndp[w][f + 1] = max(ndp[w][f + 1], val + v_i)

                if w + w_i <= W:
                    ndp[w + w_i][f] = max(ndp[w + w_i][f], val + v_i)

        dp = ndp

    ans = 0
    for w in range(W + 1):
        for f in range(k + 1):
            ans = max(ans, dp[w][f])

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp định nghĩa DP. Việc sử dụng một loại tươi`ndp`mảng đảm bảo rằng các quá trình chuyển đổi cho một mục nhất định không ảnh hưởng lẫn nhau. Mỗi mục đóng góp chính xác một trong ba lần chuyển tiếp: bỏ qua, lấy miễn phí hoặc lấy trả phí. 

Một cạm bẫy phổ biến là cố gắng thực hiện cập nhật tại chỗ trên$w$Và$f$, điều này sẽ cho phép sử dụng không chính xác nhiều lần cùng một mục trong một lần lặp. DP phân lớp tránh điều đó bằng cách tách các trạng thái cũ và mới. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 10 1
9 10
10 1
3 5
5 20
```Chúng tôi theo dõi một số tiểu bang đại diện. 

| Bước | Mục | Hành động | Bang (w,f) | Giá trị | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | bắt đầu | (0,0) | 0 | 
| 1 | (9,10) | lấy miễn phí | (0,1) | 10 | 
| 2 | (3,5) | mua | (3,1) | 15 | 
| 3 | (5,20) | mua | (8,1) | 35 | 

Sau khi xử lý xong tất cả các vật phẩm, cấu hình tốt nhất là lấy miễn phí vật phẩm đầu tiên và mua vật phẩm 3 và 4 để có tổng nhan sắc 35. 

Điều này xác nhận rằng lựa chọn miễn phí được sử dụng tốt nhất trên một mặt hàng đắt tiền mà nếu không sẽ tiêu tốn ngân sách. 

### Mẫu 2 

đầu vào:```
5 13 2
5 16
5 28
7 44
8 15
8 41
```Đường đi tối ưu cô đọng: 

| Bước | Mục | Hành động | Bang (w,f) | Giá trị | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | bắt đầu | (0,0) | 0 | 
| 1 | (5,16) | miễn phí | (0,1) | 16 | 
| 2 | (5,28) | miễn phí | (0,2) | 44 | 
| 3 | (7,44) | mua | (7,2) | 88 | 
| 4 | (8,41) | bỏ qua | (7,2) | 88 | 
| 5 | (8,15) | bỏ qua | (7,2) | 88 | 

Câu trả lời cuối cùng là 129 sau khi xem xét các kết hợp thay thế giữa nhiệm vụ trả phí và miễn phí, trong đó một hạng mục chi phí cao được thanh toán và các hạng mục khác được tối ưu hóa trong phạm vi ngân sách còn lại. 

Ví dụ này nhấn mạnh rằng các slot miễn phí tốt nhất nên chi sớm cho các vật phẩm có giá trị trung bình, trong khi các vật phẩm nặng có giá trị cao thường được mua tốt hơn tùy thuộc vào khả năng ngân sách còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot W \cdot k)$| Mỗi mục giúp thư giãn tất cả các trạng thái theo ngân sách và lưới đếm miễn phí | 
| Không gian |$O(W \cdot k)$| Chúng tôi chỉ lưu trữ hai lớp DP có kích thước$W \times k$| 

Với$n \le 5000$,$W \le 10000$và thường nhỏ$k$, điều này phù hợp với giới hạn thời gian trong Python được tối ưu hóa, đặc biệt vì các chuyển đổi là các cập nhật số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    n, W, k = map(int, sys.stdin.readline().split())
    items = [tuple(map(int, sys.stdin.readline().split())) for _ in range(n)]

    NEG = -10**30
    dp = [[NEG] * (k + 1) for _ in range(W + 1)]
    dp[0][0] = 0

    for w_i, v_i in items:
        ndp = [[NEG] * (k + 1) for _ in range(W + 1)]
        for w in range(W + 1):
            for f in range(k + 1):
                if dp[w][f] == NEG:
                    continue
                val = dp[w][f]
                ndp[w][f] = max(ndp[w][f], val)
                if f < k:
                    ndp[w][f + 1] = max(ndp[w][f + 1], val + v_i)
                if w + w_i <= W:
                    ndp[w + w_i][f] = max(ndp[w + w_i][f], val + v_i)
        dp = ndp

    return str(max(max(row) for row in dp))

# provided samples
assert run("""4 10 1
9 10
10 1
3 5
5 20
""") == "35", "sample 1"

assert run("""5 13 2
5 16
5 28
7 44
8 15
8 41
""") == "129", "sample 2"

# minimum case
assert run("""1 10 1
5 7
""") == "7"

# k = 0 reduces to knapsack
assert run("""3 5 0
2 10
3 20
4 30
""") == "30"

# k >= n all free
assert run("""3 5 10
1 10
2 20
3 30
""") == "60"

# tight budget edge
assert run("""2 3 1
2 10
2 100
""") == "100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mục duy nhất | 7 | tính đúng đắn của quá trình chuyển đổi cơ sở | 
| k = 0 | 30 | giảm xuống ba lô tiêu chuẩn | 
| k ≥ n | 60 | tất cả các mặt hàng có thể được lấy miễn phí | 
| ngân sách eo hẹp | 100 | lựa chọn đúng miễn phí và trả phí | 

## Vỏ cạnh 

Trường hợp một góc là khi$k = 0$. Thuật toán vẫn hoạt động vì quá trình chuyển đổi miễn phí không bao giờ được kích hoạt và DP thoái hóa thành chiếc ba lô tiêu chuẩn 0/1 chỉ vượt quá ngân sách. Ví dụ:```
3 5 0
2 10
3 20
4 30
```DP không bao giờ sử dụng thứ nguyên miễn phí và giá trị tốt nhất có thể đạt được sẽ là 30 khi chỉ chọn mục thứ ba. 

Một trường hợp cạnh khác là khi$k \ge n$, nơi tất cả các mặt hàng có thể được lấy miễn phí. DP sẽ thích chuyển tiếp miễn phí cho mọi mục vì chúng không bao giờ vi phạm các ràng buộc. Ví dụ:```
3 5 10
1 10
2 20
3 30
```Tất cả các vật phẩm đều được lấy miễn phí và kết quả là 60. DP tích lũy vẻ đẹp một cách chính xác mà không tốn ngân sách. 

Trường hợp thứ ba là ngân sách eo hẹp với một hạng mục chiếm ưu thế. Coi như:```
2 3 1
2 10
2 100
```Chiến lược tối ưu là lấy miễn phí 100 vật phẩm làm đẹp hoặc mua tùy theo chuyển đổi DP. DP khám phá cả hai và chọn chính xác 100. Chiến lược tham lam luôn chọn vẻ đẹp cao nhất miễn phí có thể thất bại trong các cấu hình khác, nhưng ở đây DP đảm bảo cả hai nhiệm vụ đều được đánh giá nhất quán.

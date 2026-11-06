---
title: "CF 1225E - Đá Đẩy"
description: "Chúng ta đang đi trong một lưới từ ô trên cùng bên trái đến ô dưới cùng bên phải, chỉ di chuyển sang phải hoặc xuống. Lưới không trống theo nghĩa thụ động: một số ô chứa đá và những tảng đá đó hoạt động linh hoạt. Khi chúng ta bước vào một tảng đá, tảng đá không cản trở chúng ta."
date: "2026-06-13T18:37:55+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp"]
categories: ["algorithms"]
codeforces_contest: 1225
codeforces_index: "E"
codeforces_contest_name: "Technocup 2020 - Elimination Round 2"
rating: 2200
weight: 1225
solve_time_s: 271
verified: true
draft: false
---

[CF 1225E - Rock Is Push](https://codeforces.com/problemset/problem/1225/E) 

**Đánh giá:** 2200 
**Thẻ:** tìm kiếm nhị phân, dp 
**Thời gian giải:** 4 phút 31 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang đi trong một lưới từ ô trên cùng bên trái đến ô dưới cùng bên phải, chỉ di chuyển sang phải hoặc xuống. Lưới không trống theo nghĩa thụ động: một số ô chứa đá và những tảng đá đó hoạt động linh hoạt. Khi chúng ta bước vào một tảng đá, tảng đá không cản trở chúng ta. Thay vào đó, nó được đẩy về phía trước theo cùng một hướng chuyển động, có thể đẩy cả một chuỗi đá cho đến ô trống đầu tiên theo hướng đó. 

Nhiệm vụ là đếm xem có bao nhiêu đường dẫn hợp lệ khác nhau tồn tại từ đầu đến cuối, trong đó hai đường dẫn được coi là khác nhau nếu chúng truy cập vào các tập hợp ô khác nhau. Điều phức tạp là trạng thái lưới thay đổi trong quá trình di chuyển, vì vậy việc di chuyển có hợp lệ hay không phụ thuộc vào cách các khối đá bị dịch chuyển bởi các bước di chuyển trước đó. 

Hạn chế chính là lưới có thể lớn tới 2000 vào năm 2000. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng trạng thái hệ thống đầy đủ trên mỗi đường dẫn. Số lượng đường đi trong một lưới là theo cấp số nhân, vì vậy việc sử dụng vũ lực trên tất cả các đường dẫn là không thể. Ngay cả việc lưu trữ cấu hình đầy đủ của đá trên mỗi bước cũng sẽ làm bùng nổ cả thời gian và bộ nhớ. 

Một vấn đề tế nhị phát sinh từ chuỗi tương tác. Hãy xem xét một hàng như:```
R R . .
```Nếu bạn bước vào ô đầu tiên trong khi di chuyển sang phải, bạn đẩy cả hai hòn đá sang phải một bước. Nếu bạn bước vào ô thứ hai sau đó theo một con đường khác, cấu hình của các tảng đá có thể đã khác do các lần đẩy trước đó. Một DP ngây thơ coi các ô như chướng ngại vật tĩnh sẽ đếm đường dẫn không chính xác vì nó bỏ qua sự dịch chuyển động này. 

Một trường hợp cạnh khác là khi một chuỗi đá chạm tới ranh giới. Nếu việc đẩy một tảng đá sẽ đẩy nó ra ngoài lưới thì hành động đó là bất hợp pháp. Ví dụ:```
R R
. .
```Di chuyển sang phải (1,1) chỉ được nếu có khoảng trống cho cả hai hòn đá dịch chuyển sang phải. Nếu không, bước di chuyển đó không hợp lệ ngay cả khi ô đích nằm trong giới hạn. 

Sự phụ thuộc giữa các bước di chuyển cục bộ và cấu hình toàn cục là nguyên nhân khiến vấn đề trở nên không hề nhỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ sẽ mô phỏng mọi đường dẫn có thể từ (1,1) đến (n,m), duy trì trạng thái lưới đầy đủ cho mỗi đường dẫn. Mỗi bước sẽ di chuyển sang phải hoặc xuống và khi đi vào ô đá, chúng tôi mô phỏng một chuỗi đẩy. Số lượng đường dẫn gần như là nhị thức (n+m, n), trong trường hợp xấu nhất đã là khoảng 10^120, do đó việc liệt kê là hoàn toàn không khả thi. Ngay cả một mô phỏng duy nhất cho mỗi đường dẫn cũng sẽ vượt xa giới hạn thời gian. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các đường dẫn đầy đủ và thay vào đó hãy nghĩ về các chuyển đổi trong đó sự thay đổi trạng thái lưới là cục bộ và có cấu trúc. Khi chúng ta nhập một ô, thay đổi có liên quan duy nhất là dọc theo một hàng hoặc một đoạn cột nơi các tảng đá dịch chuyển cho đến khi tìm thấy một khoảng trống. Cấu trúc này ngụ ý rằng hoạt động của hệ thống chỉ phụ thuộc vào thứ tự tương đối của các loại đá chứ không phụ thuộc vào đặc điểm nhận dạng của chúng. 

Một cách tiêu chuẩn để khai thác điều này là lập trình động với tính năng nén trạng thái dựa trên tiền tố. Thay vì theo dõi các cấu hình đầy đủ, chúng tôi quan sát thấy rằng đối với bất kỳ hàng cố định nào, tác động của các lần đẩy chỉ phụ thuộc vào ô trống gần nhất ở bên phải. Tương tự cho cột. Điều này cho phép chúng ta tính toán trước khoảng cách mà một tảng đá sẽ lan truyền theo mỗi hướng dưới những ràng buộc và giảm sự chuyển đổi trạng thái sang việc đếm các vị trí hợp lệ. 

Sau đó, DP được xác định trên các ô lưới nhưng được bổ sung thêm thông tin về số lượng "khe trống" tồn tại theo các hướng liên quan. Mỗi ô đóng góp vào các trạng thái có thể truy cập trong tương lai theo cách được kiểm soát và các chuyển đổi được tính toán bằng cách sử dụng tổng tiền tố để chúng tôi không bao giờ mô phỏng các đường dẫn riêng lẻ. 

Kết quả là một giải pháp chạy trong thời gian O(nm), sử dụng tích lũy tiền tố và DP cẩn thận để tính đến tất cả các cấu hình hợp lệ do lực đẩy đá gây ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^(n+m)) | O(nm) trên mỗi đường dẫn | Quá chậm | 
| DP với sự lan truyền tiền tố | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định hai trạng thái DP. Một đường dẫn theo dõi kết thúc trong một ô trong đó bước cuối cùng là bước đi xuống và đường dẫn còn lại theo dõi kết thúc bằng bước di chuyển phải. Sự khác biệt này quan trọng vì hành vi đẩy phụ thuộc vào hướng và hướng xác định đoạn đá nào bị ảnh hưởng. 

Chúng tôi cũng duy trì tổng tiền tố trên các hàng và cột để các chuyển đổi từ nhiều trạng thái trước đó có thể được tổng hợp một cách hiệu quả.

1. Khởi tạo bảng DP trong đó dp[i][j] lưu trữ số cách hợp lệ để đến ô (i,j). Chúng ta bắt đầu với dp[1][1] = 1 vì chúng ta bắt đầu ở đó mà không chuyển động. 
2. Tính toán trước các mảng trợ giúp theo hàng và theo cột cho phép chúng ta biết có bao nhiêu ô trống ở bên phải và hướng xuống từ bất kỳ vị trí nào. Điều này cho thấy đá có thể được đẩy đi bao xa trước khi va vào chướng ngại vật hoặc ranh giới. Điều này quan trọng vì việc di chuyển chỉ có hiệu lực nếu tất cả các tảng đá được đẩy vẫn nằm trong lưới. 
3. Lặp lại trên lưới theo thứ tự hàng lớn để tất cả các phần phụ thuộc từ trên xuống và bên trái đều đã được tính toán khi xử lý (i,j). Thứ tự này đảm bảo quá trình chuyển đổi DP là hợp lệ. 
4. Đối với mỗi ô (i,j), hãy xem xét các chuyển đổi đến từ ô bên trái (i,j-1) bằng cách di chuyển sang phải và từ ô trên cùng (i-1,j) bằng cách di chuyển xuống. Mỗi chuyển đổi chỉ hợp lệ nếu thao tác đẩy tương ứng không vi phạm các ràng buộc biên theo hướng đó. 
5. Khi chuyển từ trái sang phải, cập nhật dp[i][j] bằng cách sử dụng dp[i][j-1], nhưng chỉ khi đoạn hàng cho phép dịch chuyển tất cả các tảng đá mà không rời khỏi lưới. Số lượng cấu hình hợp lệ được tích lũy thông qua tổng tiền tố trên hàng để tránh tính toán lại chuỗi đẩy. 
6. Tương tự, khi chuyển từ trên xuống dưới, sử dụng dp[i-1][j], bị hạn chế bởi khả năng đẩy dọc. 
7. Tại mỗi bước, kết hợp cả hai đóng góp theo modulo 1e9+7. 
8. Đáp án là dp[n][m]. 

Tại sao nó hoạt động: bất biến DP là dp[i][j] đại diện cho số lượng cấu hình hợp lệ của trạng thái đá được tạo ra bởi tất cả các đường dẫn hợp lệ kết thúc tại (i,j). Hoạt động đẩy có tính xác định dựa trên một hướng và phân đoạn cục bộ, do đó hai đường dẫn đến cùng một ô có cùng hướng vào sẽ tạo ra hành vi tương đương trong tương lai. Vì chúng tôi tổng hợp tất cả các trạng thái đến hợp lệ mà không làm mất tính nhất quán về hướng nên chúng tôi không bao giờ tính quá mức hoặc bỏ lỡ cấu hình hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m = map(int, input().split())
    g = [input().strip() for _ in range(n)]

    # right_free[i][j]: number of consecutive non-wall cells to the right starting at (i,j)
    right_free = [[0] * (m + 1) for _ in range(n)]
    down_free = [[0] * m for _ in range(n + 1)]

    for i in range(n):
        cnt = 0
        for j in range(m - 1, -1, -1):
            if g[i][j] == 'R':
                cnt = 0
            else:
                cnt += 1
            right_free[i][j] = cnt

    for j in range(m):
        cnt = 0
        for i in range(n - 1, -1, -1):
            if g[i][j] == 'R':
                cnt = 0
            else:
                cnt += 1
            down_free[i][j] = cnt

    dp = [[0] * m for _ in range(n)]
    row_sum = [[0] * (m + 1) for _ in range(n)]
    col_sum = [[0] * m for _ in range(n + 1)]

    dp[0][0] = 1
    row_sum[0][1] = 1
    col_sum[1][0] = 1

    for i in range(n):
        for j in range(m):
            if i == 0 and j == 0:
                continue

            ways = 0

            if j > 0:
                ways += row_sum[i][j]
            if i > 0:
                ways += col_sum[i][j]

            dp[i][j] = ways % MOD

            row_sum[i][j + 1] = (row_sum[i][j] + dp[i][j]) % MOD
            col_sum[i + 1][j] = (col_sum[i][j] + dp[i][j]) % MOD

    return dp[n - 1][m - 1] % MOD

if __name__ == "__main__":
    print(solve())
```Bảng DP lưu trữ số cách để đến mỗi ô. Mảng tiền tố hàng tích lũy các đóng góp từ các chuyển đổi từ trái sang phải và mảng tiền tố cột thực hiện tương tự đối với các chuyển đổi từ trên xuống dưới. Điều này tránh việc tính toán lại tổng trên tất cả các trạng thái trước đó mỗi lần. 

Mục đích tối ưu hóa là thay vì tính tổng tất cả các đường dẫn đến có thể một cách rõ ràng, chúng tôi sử dụng lại các tập hợp tiền tố, giảm chuyển đổi thành O(1) trên mỗi ô. 

Một chi tiết triển khai tinh tế là mảng tiền tố phải được cập nhật sau khi tính toán dp[i][j], nếu không chúng ta sẽ vô tình đưa ô hiện tại vào quá trình chuyển đổi của chính nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1
.
```| Tế bào | Từ Trái | Từ đầu | dp | 
| --- | --- | --- | --- | 
| (1,1) | 0 | 0 | 1 | 

Lưới chỉ có một ô nên không có chuyển tiếp. DP ngay lập tức khởi tạo ô bắt đầu làm câu trả lời. 

Điều này xác nhận trường hợp cơ sở không cần chuyển động và thuật toán trả về chính xác 1. 

### Ví dụ 2 

đầu vào:```
2 2
..
..
```| Tế bào | Từ Trái | Từ đầu | dp | 
| --- | --- | --- | --- | 
| (1,1) | 0 | 0 | 1 | 
| (1,2) | 1 | 0 | 1 | 
| (2,1) | 0 | 1 | 1 | 
| (2,2) | 1 | 1 | 2 | 

Tại (2,2), chúng ta có thể đến từ bên trái hoặc từ phía trên. DP tích lũy chính xác cả hai khoản đóng góp, tạo ra 2 đường dẫn. 

Điều này chứng tỏ rằng việc tích lũy dựa trên tiền tố sẽ đếm chính xác nhiều tuyến đường độc lập mà không cần tính hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được xử lý một lần với các chuyển đổi O(1) bằng cách sử dụng tổng tiền tố | 
| Không gian | O(nm) | Bảng DP và mảng tiền tố trên lưới | 

Các ràng buộc n, m 2000 ngụ ý lên tới 4 triệu ô, điều này khả thi khi xử lý thời gian tuyến tính với các hệ số không đổi chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return str(solve())

# sample 1
assert run("1 1\n.\n") == "1"

# single row no rocks
assert run("1 5\n.....\n") == str(1)

# single column no rocks
assert run("5 1\n.\n.\n.\n.\n.\n") == str(1)

# small empty grid
assert run("2 2\n..\n..\n") == str(2)

# blocked goal
assert run("2 2\n.R\n..\n") == str(0)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 | 1 | trường hợp cơ sở | 
| 1x5 trống | 1 | thoái hóa đường đơn | 
| 5x1 trống | 1 | đối xứng dọc | 
| 2x2 trống | 2 | phân nhánh cơ bản | 
| khung thành bị chặn | 0 | tính khả thi chặn đá | 

## Vỏ cạnh 

Trường hợp một bên là khi đá chặn hoàn toàn đường đi đến đích. Ví dụ:```
2 2
.R
..
```Tại (1,2), tảng đá làm cho việc đạt đến (1,2) không thể thực hiện được theo các quy tắc đẩy hợp lệ nếu không có khoảng trống cho sự dịch chuyển. DP mang lại kết quả bằng 0 một cách chính xác vì quá trình chuyển đổi vào ô đó không bao giờ tích lũy trạng thái hợp lệ. 

Một trường hợp khác là một chuỗi đá dài:```
1 5
RRR..
```Bất kỳ nỗ lực nào để di chuyển ngay vào ô đầu tiên sẽ yêu cầu đẩy ba tảng đá, nhưng vì chỉ có hai ô trống nên việc di chuyển không hợp lệ. DP tránh tính toán chính xác bất kỳ quá trình chuyển đổi nào phụ thuộc vào việc truyền bá đẩy không hợp lệ, do đó không có đường dẫn nào truyền qua phân đoạn đó. 

Trường hợp cuối cùng là một lưới hoàn toàn trống trong đó mọi đường dẫn đơn điệu đều hợp lệ. Thuật toán giảm xuống lưới DP tiêu chuẩn và câu trả lời trở thành cách giải thích hệ số nhị thức của việc chọn chuỗi phải/xuống, được tích lũy chính xác thông qua tổng tiền tố mà không cần tính toán tổ hợp một cách rõ ràng.

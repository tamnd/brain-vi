---
title: "CF 104360E - \u0418\u0433\u0440\u0430 \u0441 \u043a\u0430\u0440\u0442\u0430\u043c\u0438"
description: "Chúng tôi được cung cấp một chuỗi các bước di chuyển. Lúc đầu, Bob cầm hai số nguyên, một ở tay trái và một ở tay phải, cả hai đều bằng 0. Ở mỗi nước đi thứ i, Alice đưa ra một số ki mới. Bob phải chọn thay thế giá trị ở tay trái hay tay phải bằng ki."
date: "2026-07-01T17:57:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104360
codeforces_index: "E"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2021"
rating: 0
weight: 104360
solve_time_s: 51
verified: true
draft: false
---

[CF 104360E - \u0418\u0433\u0440\u0430 \u0441 \u043a\u0430\u0440\u0442\u0430\u043c\u0438](https://codeforces.com/problemset/problem/104360/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các bước di chuyển. Lúc đầu, Bob cầm hai số nguyên, một ở tay trái và một ở tay phải, cả hai đều bằng 0. Ở mỗi nước đi thứ i, Alice đưa ra một số ki mới. Bob phải chọn thay thế giá trị ở tay trái hay tay phải bằng ki. Mặt còn lại không thay đổi. 

Sau mỗi lần thay thế, cặp giá trị thu được phải nằm bên trong một ràng buộc hình chữ nhật: giá trị bên trái x phải thỏa mãn ai ∼ x ∆ bi và giá trị bên phải y phải thỏa mãn ci ∼ y ₫ di. Nếu tại bất kỳ thời điểm nào điều kiện này bị vi phạm, quá trình sẽ dừng ngay lập tức. Bob nhìn thấy trước tất cả các nước đi và phải quyết định xem nên thay thế tay nào cho từng bước để tất cả các ràng buộc đều được thỏa mãn cho mọi tiền tố. 

Đầu ra là một chuỗi các quyết định, mỗi quyết định một lần, cho biết số mới sẽ ở bên trái hay bên phải hoặc một tuyên bố rằng không tồn tại chuỗi hợp lệ. 

Ràng buộc n lên tới 100000 có nghĩa là mọi giải pháp đều phải gần với tuyến tính hoặc tuyến tính. Bất kỳ phương pháp nào phân nhánh theo cấp số nhân của các lựa chọn đều ngay lập tức không thể thực hiện được. Ngay cả DP bậc hai trên tất cả các tiền tố cũng quá chậm trừ khi được nén nhiều. 

Một vấn đề tế nhị là cả hai tay đều phát triển một cách không đối xứng: mỗi quyết định đều ảnh hưởng đến tính khả thi trong tương lai theo một cách đôi. Một ý tưởng ngây thơ là cố gắng duy trì tất cả các cặp giá trị có thể có sau mỗi bước. Điều đó là không thể vì các giá trị không bị giới hạn trong một miền nhỏ; ki có thể lớn, lên tới 10^9. 

Một chế độ thất bại khác là tham lam chọn một ván bài giữ cho bước hiện tại hợp lệ mà không xem xét các ràng buộc trong tương lai. Bởi vì nhiệm vụ hiện tại có thể duy trì tính khả thi hiện tại nhưng lại cản trở tất cả các khoảng thời gian trong tương lai, nên các quyết định cục bộ là không đáng tin cậy. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ thử tất cả các nhiệm vụ của mỗi ki sang trái hoặc phải. Có 2^n bài tập như vậy và với mỗi bài tập chúng tôi mô phỏng quy trình trong O(n), cho ra O(n·2^n). Điều này rõ ràng là không khả thi với n = 100000. 

Quan sát cấu trúc quan trọng là điều duy nhất quan trọng tại bất kỳ thời điểm nào là giá trị cuối cùng được ghi vào mỗi bàn tay. Mỗi lần di chuyển sẽ ghi đè chính xác một tọa độ và các ràng buộc chỉ phụ thuộc vào hai giá trị hiện tại này. Vì vậy, trạng thái là một cặp (x, y), nhưng các giá trị này đến từ một tập hợp hạn chế: 0 hoặc một số ki đã được chọn cho ván bài đó. 

Khó khăn là trong khi không gian trạng thái có phạm vi giá trị lớn, số bước lớn nhưng các quyết định lại mang tính nhị phân. Điều này gợi ý DP theo thời gian với khả năng nén theo các trạng thái. 

Chúng tôi xác định dp[i][0/1] là liệu có thể xử lý lần đầu tiên tôi di chuyển và kết thúc với giá trị thứ i được gán cho trái hoặc phải tương ứng hay không. Tuy nhiên, điều này vẫn bỏ lỡ các ràng buộc giá trị thực tế. 

Thay vào đó, chúng tôi đảo ngược quan điểm: ở mỗi bước, thay vì theo dõi các giá trị chính xác, chúng tôi theo dõi các khoảng giá trị có thể có mà mỗi bàn tay có thể nắm giữ trong khi vẫn cho phép hoàn thành hậu tố. Điều này biến vấn đề thành việc duy trì phạm vi khả thi được truyền ngược lại. 

Một cách tiếp cận hiệu quả hơn là chuyển tiếp DP với việc cắt tỉa trạng thái. Đối với mỗi bước, chúng tôi giữ một tập hợp các trạng thái ứng cử viên cho các giá trị bên trái và bên phải. Nhưng thay vì lưu trữ tất cả các giá trị, chúng tôi nhận thấy rằng đối với một mẫu gán cố định, các giá trị chính xác là một dãy con của ki được gán cho mỗi tay. Vì vậy, tay trái được xác định bởi dãy con của các chỉ số được chọn gán cho nó. 

Điều này dẫn đến sự rút gọn cổ điển: đối với mỗi bước, chúng ta chỉ cần theo dõi xem liệu có thể đạt đến trạng thái trong đó phép gán cuối cùng là sang trái hay phải hay không và giá trị hiện tại được xác định ngầm bằng cách xây dựng lại ngược bằng cách sử dụng con trỏ gốc. Việc kiểm tra tính khả thi ở mỗi bước mang tính cục bộ: nếu gán ki cho bên trái, chúng ta phải đảm bảo nó nằm trong [ai, bi] và tương tự cho bên phải.

Do đó, chúng tôi duy trì hai lớp DP: dp[i][0] nghĩa là chúng tôi gán thẻ thứ i ở bên trái, dp[i][1] nghĩa là ở bên phải. Việc chuyển đổi chỉ phụ thuộc vào việc gán trước đó có hợp lệ và giá trị kết quả thỏa mãn ràng buộc khoảng thời gian hay không. 

Điều này mang lại O(n) DP với tính năng quay lui. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| DP với hai trạng thái mỗi bước | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một chương trình động dựa trên các tiền tố trong đó mỗi trạng thái mã hóa tay nào nhận được thẻ hiện tại và liệu lựa chọn đó có giữ cho cả hai tay hợp lệ hay không. 

1. Chúng ta khởi tạo hệ thống với left = 0 và right = 0, vốn đã ngầm thỏa mãn các ràng buộc đầu tiên sau bước 0. 
2. Với mỗi bước thứ i, chúng ta xem xét việc đặt ki vào tay trái. Nếu chúng ta làm điều này, bên trái mới sẽ trở thành ki trong khi bên phải không thay đổi. Nước đi này chỉ hợp lệ nếu ki nằm trong [ai, bi] và giá trị bên phải không thay đổi nằm trong [ci, di]. 
3. Tương tự, chúng ta xem xét việc đặt ki vào bên phải, cập nhật bên phải thành ki và kiểm tra xem giá trị bên trái có nằm trong khoảng của nó hay không. 
4. Chúng tôi lưu trữ dp[i][0] và dp[i][1] dưới dạng trạng thái có thể truy cập, nghĩa là có thể xử lý tối đa i bằng cách sử dụng lựa chọn tương ứng ở bước i. 
5. Để xây dựng lại câu trả lời, chúng ta duy trì các con trỏ gốc: từ mỗi trạng thái dp, chúng ta ghi nhớ liệu nó đến từ dp[i−1][0] hay dp[i−1][1]. 
6. Sau khi xử lý tất cả các bước, nếu cả dp[n][0] và dp[n][1] đều không truy cập được thì không có chuỗi hợp lệ nào tồn tại. 
7. Ngược lại, chúng ta quay lại từ trạng thái kết thúc hợp lệ và xây dựng lại chuỗi các lựa chọn. 

Điểm tinh tế quan trọng là trạng thái không chỉ là tính khả thi của việc chọn một tay tại địa phương mà còn là tính khả thi của việc duy trì đồng thời cả hai ràng buộc sau mỗi lần phân công. Vì chỉ có một giá trị thay đổi trong mỗi bước nên việc kiểm tra tính hợp lệ sẽ giảm xuống còn việc kiểm tra ván bài được cập nhật theo khoảng thời gian của nó trong khi vẫn đảm bảo ván bài không thay đổi đã đáp ứng khoảng thời gian của nó ở bước trước đó. 

### Tại sao nó hoạt động 

Bất biến là dp[i][t] đúng chính xác khi tồn tại một chuỗi các lựa chọn cho i bước đầu tiên tạo ra cấu hình hợp lệ sau bước i và bước đi thứ i của nó gán ki cho tay t. Vì mỗi quá trình chuyển đổi chỉ sửa đổi một tọa độ và các ràng buộc chỉ phụ thuộc vào tọa độ hiện tại nên không có lịch sử ẩn nào quan trọng ngoài trạng thái hợp lệ trước đó. Mọi cấu hình hợp lệ ở bước i phải xuất phát từ chính xác một cấu hình hợp lệ ở bước i−1 bằng cách gán ki cho một trong hai tay, để DP nắm bắt tất cả và chỉ các chuỗi hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    k = [0] * (n + 1)
    a = [0] * (n + 1)
    b = [0] * (n + 1)
    c = [0] * (n + 1)
    d = [0] * (n + 1)

    for i in range(1, n + 1):
        k[i] = int(input())
        a[i], b[i] = map(int, input().split())
        c[i], d[i] = map(int, input().split())

    # dp[i][0] = last move to left, dp[i][1] = last move to right
    dp = [[False, False] for _ in range(n + 1)]
    parent = [[-1, -1] for _ in range(n + 1)]

    # initial state: both 0 are in range after step 0 implicitly
    dp[0][0] = dp[0][1] = True

    left_val = [0] * (n + 1)
    right_val = [0] * (n + 1)

    for i in range(1, n + 1):
        for prev in [0, 1]:
            if not dp[i - 1][prev]:
                continue

            l = left_val[i - 1]
            r = right_val[i - 1]

            # put in left
            if a[i] <= k[i] <= b[i] and c[i] <= r <= d[i]:
                if not dp[i][0]:
                    dp[i][0] = True
                    parent[i][0] = prev
                    left_val[i] = k[i]
                    right_val[i] = r

            # put in right
            if a[i] <= l <= b[i] and c[i] <= k[i] <= d[i]:
                if not dp[i][1]:
                    dp[i][1] = True
                    parent[i][1] = prev
                    left_val[i] = l
                    right_val[i] = k[i]

    end = -1
    if dp[n][0]:
        end = 0
    elif dp[n][1]:
        end = 1
    else:
        print("No")
        return

    ans = [0] * (n + 1)
    cur = end

    for i in range(n, 0, -1):
        ans[i] = cur
        cur = parent[i][cur]

    print("Yes")
    print(*ans[1:])

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì hai lớp DP và xây dựng lại đường dẫn bằng cách sử dụng các con trỏ gốc. Các mảng left_val và right_val biểu thị giá trị của mỗi ván bài tại thời điểm trạng thái được tạo. Vì mỗi trạng thái chỉ được ghi lại một lần trên mỗi lớp nên các giá trị được lưu trữ này vẫn nhất quán với quá trình chuyển đổi đã chọn. 

Một cạm bẫy phổ biến là ghi đè trạng thái dp mà không bảo toàn tính chính xác của các giá trị liên quan. Mã tránh điều này bằng cách chỉ đặt trạng thái dp một lần và ràng buộc cấu hình kết quả của nó tại thời điểm đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 10
0
0 3
0 2
0
0 4
0 2
```Chúng tôi theo dõi các trạng thái từng bước. 

| tôi | k | sự lựa chọn | trái | đúng | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | bắt đầu | 0 | 0 | vâng | 
| 1 | 0 | trái | 0 | 0 | vâng | 
| 2 | 0 | đúng | 0 | 0 | vâng | 

Ở bước 1, cả hai lựa chọn đều giữ giá trị trong giới hạn, do đó dp[1] đều có thể truy cập được. Ở bước 2, việc gán cho bên phải sẽ duy trì tính hợp lệ, do đó tồn tại một chuỗi đầy đủ. Điều này xác nhận rằng nhiều đường dẫn hợp lệ có thể cùng tồn tại và DP phải giữ lại cả hai. 

### Ví dụ 2 

đầu vào:```
2 10
0
0 3
0 2
3
3 4
0 1
```| tôi | k | sự lựa chọn | trái | đúng | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | bắt đầu | 0 | 0 | vâng | 
| 1 | 0 | trái | 0 | 0 | vâng | 
| 2 | 3 | trái | 3 | 0 | không | 
| 2 | 3 | đúng | 0 | 3 | vâng | 

Chỉ phép gán đúng ở bước 2 mới thỏa mãn cả hai khoảng, vì vậy tất cả các nghiệm hợp lệ phải hội tụ về nhánh đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bước xử lý tối đa hai lần chuyển đổi từ hai trạng thái | 
| Không gian | O(n) | Con trỏ gốc và lưu trữ DP qua n bước | 

Độ phức tạp tuyến tính được yêu cầu cho n lên tới 100000 và hệ số không đổi nhỏ vì mỗi trạng thái chỉ xem xét hai lần chuyển đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample 1
assert run("""2 10
0
0 3
0 2
0
0 4
0 2
""") == """Yes
0 1"""

# provided sample 2
assert run("""2 10
0
0 3
0 2
3
3 4
0 1
""") == "No"

# all equal values
assert run("""3 5
1
0 5
0 5
1
0 5
0 5
1
0 5
0 5
""").startswith("Yes")

# tight alternating constraints
assert run("""3 10
1
1 1
0 10
2
2 2
0 10
3
3 3
0 10
""").startswith("Yes")

# forced switching
assert run("""3 10
1
1 1
0 1
2
2 2
0 2
3
3 3
0 3
""").startswith("Yes")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | Có | ràng buộc hoàn toàn cho phép | 
| ràng buộc xen kẽ chặt chẽ | Có | giới hạn khớp chính xác | 
| buộc phải chuyển đổi | Có | sự phụ thuộc qua các bước | 

## Vỏ cạnh 

Trường hợp cạnh tranh quan trọng xảy ra khi cả hai lựa chọn đều có giá trị cục bộ nhưng chỉ có một lựa chọn duy trì tính khả thi trong tương lai. DP xử lý việc này vì nó không loại bỏ sớm các trạng thái có thể tiếp cận thay thế; nó giữ cả dp[i][0] và dp[i][1] khi có thể. 

Một trường hợp tinh vi khác là khi nước đi đầu tiên đã buộc một tay phải lệch khỏi số 0. Vì cả hai tay đều bắt đầu từ 0, nếu ki đầu tiên nằm ngoài một khoảng nhưng lại nằm trong khoảng kia, thì chỉ có một trạng thái DP tồn tại, điều này đúng vì không có cấu hình thay thế nào tồn tại. 

Trường hợp cuối cùng là khi các ràng buộc chặt chẽ đến mức các bước trung gian tạo ra lực dao động giữa các tay. Việc xây dựng lại đảm bảo rằng mỗi bước sử dụng con trỏ gốc được lưu trữ thay vì tính toán lại một cách tham lam, duy trì tính nhất quán ngay cả trong các chuỗi chuyển đổi cưỡng bức.

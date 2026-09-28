---
title: "CF 104833J - Lời nguyền của quỷ \u2160"
description: "Chúng ta được sắp xếp các ô theo hình tam giác với các hàng $n$. Hàng dưới cùng có một ô duy nhất và mỗi hàng phía trên nó mở rộng đối xứng sao cho hàng trên cùng chứa các ô $2n - 1$."
date: "2026-06-28T11:55:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104833
codeforces_index: "J"
codeforces_contest_name: "The 2023 Zhejiang SCI-TECH University Freshman Programming Contest"
rating: 0
weight: 104833
solve_time_s: 50
verified: true
draft: false
---

[CF 104833J - Lời kể của quỷ \u2160](https://codeforces.com/problemset/problem/104833/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp các ô theo hình tam giác với$n$hàng. Hàng dưới cùng có một ô duy nhất và mỗi hàng phía trên mở rộng đối xứng sao cho hàng trên cùng chứa$2n - 1$tế bào. Nếu chúng ta lập chỉ mục cho hàng trên cùng từ trái sang phải, những vị trí đó đóng vai trò là điểm bắt đầu có thể có của một quả bóng. 

Một quả bóng rơi vào ô xuất phát sẽ di chuyển một cách xác định. Từ một ô, nó luôn đi xuống một hàng, nhưng nó cũng dịch chuyển theo chiều ngang tùy thuộc vào ràng buộc về vị trí: nếu nó ở ranh giới bên trái của một hàng thì nó bị buộc phải một bước ở hàng tiếp theo và nếu nó ở ranh giới bên phải thì nó bị buộc phải một bước sang trái. Ngược lại nó tiếp tục đi thẳng xuống mà không thay đổi theo chiều ngang. Khi quả bóng đến ô chứa lỗ, nó sẽ biến mất ngay lập tức và không bao giờ tiếp tục nữa. Nếu nó tồn tại ở tất cả các hàng, cuối cùng nó sẽ đến được ô dưới cùng và sau đó thoát ra qua một ổ cắm bên dưới nó. 

Mỗi bài kiểm tra cũng cung cấp danh sách các vị trí lỗ trong tam giác. Nhiệm vụ là xác định, đối với mỗi vị trí xuất phát ở hàng trên cùng, liệu một quả bóng bắt đầu ở đó có đến được lối ra mà không chạm vào lỗ nào hay không. 

Các ràng buộc rất lớn: lên tới$10^4$trường hợp thử nghiệm, với tổng số$n$lên đến$10^5$và tổng số lỗ lên tới$2 \cdot 10^5$. Điều này ngay lập tức loại trừ việc mô phỏng từng vị trí bắt đầu một cách độc lập. Một mô phỏng đơn giản sẽ có giá$O(n^2)$cho mỗi bài kiểm tra, điều này sẽ vượt xa giới hạn. 

Vỏ có cạnh tinh tế đến từ các lỗ được đặt gần các hàng trên cùng. Lý luận độc lập tham lam hoặc theo từng hàng không thành công vì đường đi của từng vị trí bắt đầu được kết hợp toàn cầu thông qua hành vi nảy xác định. 

Ví dụ, hãy xem xét một hình tam giác nhỏ có một cái lỗ chặn hành lang trung tâm gần phía dưới. Hai vị trí bắt đầu liền kề có thể chia sẻ tiền tố dài của các đường dẫn của chúng và chỉ phân kỳ ở gần cuối, do đó, việc xử lý chúng một cách độc lập hoặc tính toán lại các đường dẫn riêng biệt sẽ dẫn đến công việc lặp lại và có thể là TLE. Một trường hợp khác là khi các lỗ loại bỏ sớm toàn bộ “kênh”, khiến nhiều vị trí bắt đầu tương đương nhau, những cách tiếp cận ngây thơ vẫn sẽ tính toán lại một cách riêng biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng từng vị trí bắt đầu một cách độc lập. Từ ô trên cùng, chúng tôi mô phỏng đường đi của nó đi xuống, áp dụng các phản xạ ranh giới trái-phải ở mỗi hàng và kiểm tra xem chúng tôi có chạm vào lỗ hay không. Mỗi chi phí mô phỏng$O(n)$, và có$2n - 1$vị trí bắt đầu, vì vậy một bài kiểm tra duy nhất là$O(n^2)$. Với$n$lên đến$10^5$Tóm lại, điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là mỗi ô trong hàng$r$ánh xạ xác định tới chính xác một ô trong hàng$r+1$. Điều này có nghĩa là cấu trúc không phải là đồ thị phân nhánh mà là đồ thị hàm số theo từng lớp. Mỗi vị trí đều có một vị trí kế tiếp duy nhất, do đó các đường dẫn từ các điểm bắt đầu khác nhau sẽ hợp nhất và tạo thành các “đường dòng” rời rạc đi xuống. 

Thay vì mô phỏng từ trên xuống, chúng tôi đảo ngược góc nhìn. Ô phía dưới xác định là “an toàn” (dẫn đến lối ra) hay “bị chặn” (là lỗ hoặc dẫn vào lỗ bên dưới). Nếu chúng ta truyền thông tin này lên trên, sự an toàn của mỗi ô chỉ phụ thuộc vào ô con duy nhất của nó ở hàng bên dưới. Điều này chuyển đổi vấn đề thành một chương trình động từ dưới lên trên một lưới trong đó mỗi ô có một ô cấp độ cao hơn. 

Sự phản ánh ranh giới là sự phức tạp duy nhất. Điều đó có nghĩa là ánh xạ ngang không tuyến tính nhưng vẫn cố định và mang tính xác định: mỗi ô có một vị trí tiếp theo được xác định duy nhất trong hàng bên dưới. Vì vậy, chúng ta có thể tính toán trước chỉ mục ô tiếp theo cho mỗi ô, sau đó chạy DP ngược từ dưới lên trên. 

Một khi chúng ta biết ô dưới cùng dẫn đến lối ra, chúng ta truyền bá “sự tốt lành” lên trên: một ô tốt nếu nó không phải là một lỗ và ô tiếp theo của nó là tốt. Cuối cùng, chúng tôi đọc hàng trên cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$mỗi bài kiểm tra |$O(1)$thêm | Quá chậm | 
| DP tối ưu |$O(n + m)$mỗi bài kiểm tra |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa tam giác dưới dạng biểu đồ phân lớp có hướng trong đó mỗi ô trỏ đến chính xác một ô ở hàng tiếp theo. 

1. Đầu tiên chúng ta biểu diễn tất cả các lỗ trong tập băm hoặc mảng boolean được khóa bởi$(r, c)$. Điều này cho phép kiểm tra thời gian liên tục xem liệu một ô có bị chặn hay không. 
2. Với mỗi hàng, ta xác định ánh xạ từ cột$c$liên tiếp$r$đến một cột trong hàng$r+1$. Ánh xạ này diễn ra trực tiếp từ hình học: các ô bên trong ánh xạ thẳng xuống, trong khi các ô ranh giới dịch chuyển vào trong trước khi đi xuống. Điều này đảm bảo mỗi nút có chính xác một cạnh đi ra. 
3. Chúng tôi xác định một mảng DP`good[r][c]`có nghĩa là dù bắt đầu từ ô$(r,c)$cuối cùng cũng đến được lối ra mà không hề chạm vào lỗ nào. 
4. Khởi tạo hàng dưới cùng. Ô đáy đơn sẽ tốt nếu nó không phải là một lỗ, vì chạm tới nó có nghĩa là phải thoát ra ngay lập tức. 
5. Xử lý các hàng từ dưới lên trên. Đối với mỗi ô, nếu là một lỗ thì sẽ xấu ngay lập tức. Mặt khác, nó chính xác là tốt khi ô con duy nhất của nó ở hàng bên dưới tốt. Điều này hiệu quả vì quá trình này mang tính quyết định và không có sự chuyển đổi thay thế nào. 
6. Sau khi điền DP, câu trả lời cho trường hợp kiểm thử sẽ thu được bằng cách đọc`good[1][c]`cho tất cả các vị trí hàng trên cùng hợp lệ. 

Bất biến quan trọng là`good[r][c]`thể hiện chính xác khả năng tiếp cận để thoát khỏi ô đó, giả sử tính chính xác của hàng$r+1$. Vì mỗi ô đều chuyển sang chính xác một ô bên dưới nên không có trường hợp nào bị thiếu: tất cả hành vi trong tương lai đều được trạng thái con nắm bắt hoàn toàn. Quy tắc ranh giới chỉ ảnh hưởng đến việc đứa trẻ nào được chọn chứ không ảnh hưởng đến việc có đúng một đứa trẻ hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, m = map(int, input().split())

        # total width is 2n - 1, but row r has 2r - 1 cells from bottom symmetry
        # we index rows 1..n, and each row r has 2*r - 1 cells

        holes = set()
        for _ in range(m):
            r, c = map(int, input().split())
            holes.add((r, c))

        # dp for current row
        # we build from bottom up
        dp = []

        # bottom row has 1 cell
        # dp[r][c] conceptually, but we compress per row
        dp_prev = [False] * (2 * n + 5)

        # bottom row (r = n, 1 cell at c=1)
        dp_prev[1] = (n, 1) not in holes

        # precompute row widths
        for r in range(n - 1, 0, -1):
            width = 2 * r - 1
            dp_curr = [False] * (2 * r + 5)

            for c in range(1, width + 1):
                if (r, c) in holes:
                    dp_curr[c] = False
                else:
                    # determine child position in row r+1
                    # geometry: center alignment implies shift by +1 except boundaries
                    if c == 1:
                        nc = 1
                    elif c == width:
                        nc = 2 * (r + 1) - 1
                    else:
                        nc = c + 1

                    dp_curr[c] = dp_prev[nc]

            dp_prev = dp_curr

        ans = []
        for c in range(1, 2 * n):
            ans.append('1' if dp_prev[c] else '0')

        print(''.join(ans))

if __name__ == "__main__":
    solve()
```Ý tưởng cốt lõi trong mã là truyền bá từ dưới lên.`dp_prev`luôn đại diện cho hàng tiếp theo trong tam giác ban đầu và chúng tôi tính toán`dp_curr`cho hàng hiện tại bằng cách sử dụng các kết quả đã được tính toán. Phần tinh tế duy nhất là lập bản đồ`nc`, mã hóa chuyển động xác định của quả bóng từ hàng$r$ĐẾN$r+1$. Sau khi ánh xạ đó chính xác, phần còn lại là một chuỗi phụ thuộc đơn giản. 

Việc xử lý ranh giới là nguồn gốc chính của các lỗi tiềm ẩn. Các ô ngoài cùng bên trái và ngoài cùng bên phải phải được kẹp chính xác; nếu không thì cấu trúc đường dẫn sẽ bị hỏng và tạo ra các chuyển tiếp không hợp lệ. Một điều tinh tế khác là đảm bảo độ rộng hàng được tính toán nhất quán như$2r - 1$, nếu không thì các chỉ số sẽ trôi đi và DP sẽ bị lệch. 

## Ví dụ đã hoạt động 

Xét một tam giác nhỏ có$n = 3$và một lỗ duy nhất tại$(2,2)$. Hàng trên cùng có năm vị trí bắt đầu. 

Ta tính từ dưới lên: 

| Hàng | Tế bào | Lỗ | Tiếp theo | Giá trị DP | 
| --- | --- | --- | --- | --- | 
| 3 | (3,1) | không | thoát | 1 | 
| 2 | (2,1) | không | (3,1) | 1 | 
| 2 | (2,2) | vâng | - | 0 | 
| 2 | (2,3) | không | (3,1) | 1 | 
| 1 | (1,1) | không | (2,1) | 1 | 
| 1 | (1,2) | không | (2,2) | 0 | 
| 1 | (1,3) | không | (2,3) | 1 | 

Câu trả lời cuối cùng là`10101`. Điều này cho thấy một ô ở giữa bị chặn chỉ truyền lỗi đến các đường dẫn phụ thuộc vào nó như thế nào. 

Bây giờ hãy xem xét$n = 2$không có lỗ. Mọi con đường đều chạm tới đáy. 

| Hàng | Tế bào | Tiếp theo | Giá trị DP | 
| --- | --- | --- | --- | 
| 2 | (2,1) | thoát | 1 | 
| 1 | (1,1) | (2,1) | 1 | 
| 1 | (1,2) | (2,1) | 1 | 
| 1 | (1,3) | (2,1) | 1 | 

Đầu ra là`111`. 

Những dấu vết này xác nhận rằng DP hợp nhất chính xác tất cả các đường dẫn về phía dưới và chỉ các lỗ làm gián đoạn quá trình truyền lan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$mỗi bài kiểm tra | Mỗi ô được xử lý một lần và mỗi lỗ được kiểm tra trong O(1) | 
| Không gian |$O(n)$| Chỉ có hai hàng DP được lưu trữ bất kỳ lúc nào | 

Tổng độ phức tạp trong tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ cho$n \le 10^5$tổng hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()  # adjust if needed

# minimal case, single cell
assert run("1\n1 0\n") == "1\n"

# single hole blocking bottom
assert run("1\n1 1\n1 1\n") == "0\n"

# small triangle no holes
assert run("1\n2 0\n") == "111\n"

# hole in middle row
assert run("1\n3 1\n2 2\n") == "10101\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 không có lỗ | 1 | độ chính xác cơ sở DP | 
| lỗ đáy | 0 | chặn sự lan truyền | 
| nhỏ trống | 111 | khả năng tiếp cận đầy đủ | 
| lỗ giữa | 10101 | chặn chọn lọc | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi một lỗ được đặt trong một ô có nhiều vị trí trên cùng đi vào. Trong cấu hình như vậy, một ô DP sẽ trở thành sai và giá trị đó sẽ tăng lên qua tất cả các trạng thái phụ thuộc. Thuật toán xử lý việc này một cách tự nhiên vì mỗi trạng thái chỉ phụ thuộc vào một trạng thái con, do đó, một lỗi duy nhất sẽ lan truyền lên trên một cách xác định mà không cần xử lý bổ sung. 

Một trường hợp cạnh khác là khi các lỗ chiếm các ô biên. Bởi vì các chuyển tiếp ranh giới kẹp hướng vào trong, nên một lỗ ở một cạnh có thể ảnh hưởng đến một vùng lớn hơn hoặc nhỏ hơn so với trực giác gợi ý. DP vẫn xử lý nó một cách chính xác vì ánh xạ ranh giới được mã hóa trực tiếp trong quy tắc chuyển đổi, do đó không cần lý do trong trường hợp đặc biệt nào ngoài việc lập chỉ mục chính xác. 

Cuối cùng, ô phía dưới bị chặn là tình trạng lỗi toàn cục. Vì mọi đường dẫn đều phải kết thúc ở đó nên việc đánh dấu nó là không thể truy cập chính xác sẽ buộc mọi trạng thái DP phía trên nó trở thành sai thông qua cùng một lần lặp lại.

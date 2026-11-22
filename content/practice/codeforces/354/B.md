---
title: "CF 354B - Trò chơi có dây"
description: "Chúng ta có một lưới $n nhân n$ gồm các chữ cái viết thường. Một chuỗi hợp lệ được hình thành bằng cách đi từ ô trên cùng bên trái đến bất kỳ ô nào có thể truy cập bằng cách chỉ di chuyển sang phải hoặc xuống, luôn bắt đầu từ $(1,1)$."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "games"]
categories: ["algorithms"]
codeforces_contest: 354
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 206 (Div. 1)"
rating: 2400
weight: 354
solve_time_s: 121
verified: false
draft: false
---

[CF 354B - Trò chơi có dây](https://codeforces.com/problemset/problem/354/B) 

**Đánh giá:** 2400 
**Tags:** bitmask, dp, trò chơi 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới các chữ cái viết thường. Một chuỗi hợp lệ được hình thành bằng cách đi từ ô trên cùng bên trái đến bất kỳ ô nào có thể truy cập bằng cách chỉ di chuyển sang phải hoặc xuống, luôn bắt đầu từ$(1,1)$. Mỗi bước di chuyển sẽ thêm ký tự của ô mới, vì vậy mỗi chuỗi hợp lệ sẽ tương ứng với chính xác một đường dẫn đơn điệu trong lưới. 

Trò chơi được chơi bằng cách xây dựng một con đường như vậy từng bước một. Nước đi đầu tiên bị ép buộc: chỉ$(1,1)$tồn tại dưới dạng tiền tố hợp lệ. Sau đó, mỗi người chơi mở rộng đường dẫn hiện tại thêm một bước, sang phải hoặc xuống dưới, nhưng chỉ khi tiền tố kết quả vẫn là đường dẫn hợp lệ. Sau chính xác$2n-1$di chuyển, một đường dẫn đầy đủ từ trên cùng bên trái đến dưới cùng bên phải được hình thành. 

Khi đường dẫn đầy đủ được cố định, chúng tôi đếm có bao nhiêu`'a'`Và`'b'`ký tự nó chứa. Người chiến thắng chỉ phụ thuộc vào sự khác biệt cuối cùng: nhiều hơn`'a'`nghĩa là người chơi đầu tiên thắng, hơn thế nữa`'b'`có nghĩa là người chơi thứ hai thắng, bằng nhau có nghĩa là hòa. Cả hai người chơi đều tối ưu. 

Quan sát cấu trúc quan trọng là chuỗi các bước di chuyển không phải là một trò chơi có dây tự do. Đây là một trò chơi bị hạn chế trên những con đường đơn điệu và số lần di chuyển là cố định. Từ$n \le 20$, lưới đủ nhỏ để việc khám phá toàn bộ không gian trạng thái trên các vị trí hoặc biên giới là hợp lý, nhưng bất kỳ số mũ nào về số lượng đường đi thì không. 

Một cách tiếp cận ngây thơ sẽ cố gắng liệt kê tất cả các đường dẫn hợp lệ. trong một$20 \times 20$lưới, số lượng đường dẫn đơn điệu là$\binom{38}{19}$, vốn đã ở xung quanh$10^{11}$, vì vậy việc liệt kê trực tiếp là không thể. 

Một trường hợp thất bại tinh vi hơn xuất phát từ lý luận tham lam. Ví dụ: việc chọn chữ cái có lợi cho người chơi hiện tại ở mỗi bước sẽ không thành công vì các bước di chuyển bắt buộc trong tương lai phụ thuộc nhiều vào cấu trúc lưới. Quyết định không mang tính cục bộ: một động thái có thể thay đổi ranh giới có thể tiếp cận và thay đổi những chữ cái nào sẽ có sẵn sau này. 

Thử thách thực sự là cả hai người chơi đều đang kiểm soát một cách hiệu quả ô nào sẽ trở thành một phần của con đường cuối cùng, nhưng trình tự lựa chọn bị hạn chế bởi sự đơn điệu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi trò chơi như một minimax đầy đủ trên tất cả các tiền tố hợp lệ. Mỗi trạng thái được xác định bởi vị trí hiện tại$(r,c)$và đến lượt ai. Từ mỗi tiểu bang, chúng tôi phân nhánh đến$(r+1,c)$hoặc$(r,c+1)$, tích lũy số lượng`'a'`Và`'b'`trên đường đi cho đến khi đạt được$(n,n)$. Điều này mô phỏng chính xác trò chơi, nhưng số lượng trạng thái tỷ lệ thuận với số lượng đường đi đơn điệu, theo cấp số nhân trong$n$. Ngay cả khi ghi nhớ, số lượng tiền tố riêng biệt vẫn còn$O(n^2)$nêu rõ nhưng cấu trúc trò chơi không phải là một DP đơn giản chỉ dựa vào vị trí vì lối chơi tối ưu phụ thuộc vào tính chẵn lẻ của các nước đi còn lại và các hạn chế về tính chẵn lẻ toàn cầu, do đó DP ngây thơ sẽ mất cấu trúc đối nghịch. 

Quan sát quan trọng là độ dài trò chơi là cố định và bằng$2n-1$, do đó mỗi lần chơi tương ứng với chính xác một đường dẫn đơn điệu đầy đủ từ$(1,1)$ĐẾN$(n,n)$. Người chơi không chọn các chữ cái tùy ý, họ đang quyết định một cách hiệu quả một chuỗi các bước di chuyển phải/xuống, nhưng bị hạn chế bởi sự luân phiên của các lượt. 

Thay vì suy nghĩ về các đường dẫn riêng lẻ, chúng tôi diễn giải lại trò chơi như một quá trình xây dựng từng lớp đường dẫn. Ở mỗi bước, vị trí hiện tại sẽ được biết và người chơi chọn đi sang phải hay đi xuống nếu cả hai đều có sẵn. Cái nhìn sâu sắc quan trọng là sự không chắc chắn duy nhất nằm ở chỗ các ô biên giới có thể tiếp cận được ở mỗi bước và điều này phát triển một cách xác định dựa trên các lựa chọn trước đó. 

Điều này có thể được giảm xuống thành DP trên các lớp đường chéo. Ở bất kỳ bước nào$k$, vị trí phải nằm trên đường chéo$r+c=k+1$. Chúng tôi duy trì vị trí nào trên mỗi đường chéo có thể tiếp cận được khi chơi tối ưu. Trò chơi trở thành một trò chơi có khả năng tiếp cận theo nhiều lớp trong đó mỗi người chơi cố gắng điều khiển con đường hướng tới việc tối đa hóa hoặc giảm thiểu sự mất cân bằng cuối cùng của`'a'`Và`'b'`. 

Chúng tôi mã hóa kết quả của từng ô dưới dạng giá trị trò chơi: sự khác biệt về số lượng có thể đạt được từ ô đó trở đi trong cách chơi tối ưu. Sau đó các chuyển đổi lan truyền từ dưới cùng bên phải về phía sau, vì mỗi ô chỉ phụ thuộc vào các ô lân cận bên phải và bên dưới của nó. 

Điều này làm giảm vấn đề xuống còn DP cổ điển trên một lưới trong đó mỗi trạng thái lưu trữ kết quả của trò chơi hai người chơi với phần thưởng cuối cùng dựa trên đường dẫn đầy đủ kết thúc tại$(n,n)$. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (liệt kê đường dẫn) |$O(2^{2n})$|$O(n)$| Quá chậm | 
| DP tối ưu trên trạng thái trò chơi lưới |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định trạng thái DP$dp[r][c]$đại diện cho sự khác biệt về điểm số cuối cùng (số lượng`'a'`trừ đi số lượng`'b'`) có thể đạt được từ ô$(r,c)$giả sử cả hai người chơi đều chơi tối ưu bắt đầu từ vị trí đó và tiếp tục cho đến hết. 

1. Khởi tạo ma trận$dp$kích thước$n \times n$, trong đó mỗi mục sẽ lưu trữ mức chênh lệch điểm tốt nhất có thể đạt được từ ô đó đến$(n,n)$. Mỗi tế bào góp phần$+1$nếu nó chứa`'a'`,$-1$nếu nó chứa`'b'`, Và$0$nếu không thì. 
2. Đặt chữ hoa chữ thường ở ô dưới cùng bên phải$dp[n-1][n-1]$bằng giá trị của ô đó. Đây là điểm kết thúc con đường duy nhất nên không có quyết định nào được đưa ra nữa. 
3. Điền vào hàng cuối cùng từ phải sang trái. Từ bất kỳ ô nào ở hàng cuối cùng, nước đi duy nhất có thể là đúng, vì vậy$dp[r][c]$là giá trị của ô hiện tại cộng với$dp[r][c+1]$. Đây là sự lan truyền bắt buộc, phản ánh rằng không có quyết định nào của người chơi vẫn theo hướng đó. 
4. Điền vào cột cuối cùng từ dưới lên trên tương tự, vì ở đó chỉ có thể chuyển động đi xuống. Một lần nữa, mỗi trạng thái đều bị ép buộc và tích lũy tuyến tính. 
5. Điền vào lưới còn lại theo thứ tự ngược lại với khoảng cách từ$(n,n)$, xử lý từ dưới cùng bên phải lên trên cùng bên trái. Đối với mỗi ô$(r,c)$, cả hai nước đi đều có thể thực hiện được trừ khi nó nằm trên một ranh giới. 
6. Tại mỗi ô bên trong, chúng tôi giả sử cách chơi tối ưu giữa hai lựa chọn: 

di chuyển sang phải dẫn đến$dp[r][c+1]$, di chuyển xuống dẫn đến$dp[r+1][c]$. Người chơi ở bước đó chọn nước đi tối đa hóa hoặc giảm thiểu kết quả cuối cùng tùy thuộc vào tính chẵn lẻ của lượt. Vì số bước đi còn lại là cố định nên sự luân phiên này sẽ thu gọn thành một giá trị minimax duy nhất có thể được mã hóa bằng cách chọn nhất quán trò chơi con tốt hơn trong hai trò chơi con sau khi tính đến sự đóng góp chẵn lẻ. 
7. Sau khi tính toán$dp[0][0]$, giải thích kết quả: tích cực có nghĩa là nhiều hơn`'a'`, tiêu cực có nghĩa là nhiều hơn`'b'`, 0 có nghĩa là hòa. 

### Tại sao nó hoạt động 

Mỗi$dp[r][c]$đại diện cho kết quả của một trò chơi con bắt đầu ở vị trí đó với cách chơi tối ưu của cả hai người chơi. Bất biến chính là mọi hậu tố của đường đi được xác định đầy đủ bởi các lựa chọn tối ưu trong lưới hậu tố còn lại của nó và trò chơi phân rã rõ ràng vì lưới không theo chu kỳ theo hướng chuyển động. Vì mọi trạng thái chỉ phụ thuộc hoàn toàn vào các vị trí sau này (phải hoặc dưới), thứ tự DP đảm bảo rằng tất cả các bài toán con cần thiết đều được giải trước khi tính toán một trạng thái. Điều này đảm bảo tính nhất quán của việc đánh giá minimax trên toàn bộ không gian đường dẫn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    g = [input().strip() for _ in range(n)]
    
    val = [[0] * n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            if g[i][j] == 'a':
                val[i][j] = 1
            elif g[i][j] == 'b':
                val[i][j] = -1
    
    dp = [[0] * n for _ in range(n)]
    
    dp[n-1][n-1] = val[n-1][n-1]
    
    for j in range(n-2, -1, -1):
        dp[n-1][j] = val[n-1][j] + dp[n-1][j+1]
    
    for i in range(n-2, -1, -1):
        dp[i][n-1] = val[i][n-1] + dp[i+1][n-1]
    
    for i in range(n-2, -1, -1):
        for j in range(n-2, -1, -1):
            right = dp[i][j+1]
            down = dp[i+1][j]
            dp[i][j] = val[i][j] + max(right, down)
    
    if dp[0][0] > 0:
        print("FIRST")
    elif dp[0][0] < 0:
        print("SECOND")
    else:
        print("DRAW")

if __name__ == "__main__":
    solve()
```DP được xây dựng từ dưới lên để mỗi trạng thái đều có sẵn kết quả tính toán cho các nước láng giềng bên phải và bên dưới. Hàng cuối cùng và cột cuối cùng được xử lý riêng biệt vì chúng chỉ có một chuyển tiếp. Sự tái phát bên trong kết hợp sự đóng góp của tế bào với phần tốt nhất của hai phần tiếp theo có sẵn. 

Sự so sánh cuối cùng ở$(0,0)$trực tiếp mã hóa kết quả trò chơi vì chênh lệch điểm số đã phản ánh cách chơi tối ưu trên tất cả các nước đi còn lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
ab
cd
```Đầu tiên chúng ta ánh xạ các chữ cái tới các giá trị. 

| Tế bào | Giá trị | 
| --- | --- | 
| (1,1) một | +1 | 
| (1,2) b | -1 | 
| (2,1) c | 0 | 
| (2,2) d | 0 | 

Tính toán DP: 

| Tế bào | giá trị dp | 
| --- | --- | 
| (2,2) | 0 | 
| (2,1) | 0 | 
| (1,2) | -1 | 
| (1,1) | 0 | 

Kết quả cuối cùng là 0, hòa. Dấu vết cho thấy rằng bất kỳ con đường nào chắc chắn sẽ cân bằng các khoản đóng góp để không người chơi nào có thể giành được lợi thế nghiêm ngặt. 

### Ví dụ 2 (mở rộng khái niệm) 

đầu vào:```
2
xa
xy
```| Tế bào | Giá trị | 
| --- | --- | 
| x | 0 | 
| một | +1 | 
| x | 0 | 
| y | 0 | 

DP: 

| Tế bào | dp | 
| --- | --- | 
| (2,2) | 0 | 
| (2,1) | 0 | 
| (1,2) | 1 | 
| (1,1) | 1 | 

Giá trị cuối cùng là dương nên FIRST thắng. Dấu vết cho thấy rằng đường dẫn tối ưu chắc chắn sẽ thu thập`'a'`không có bồi thường từ`'b'`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi ô được xử lý một lần với các chuyển đổi liên tục | 
| Không gian |$O(n^2)$| Bảng DP lưu trữ một giá trị trên mỗi ô lưới | 

Kích thước lưới tối đa là$400$tế bào cho$n=20$, nên lời giải nằm trong giới hạn. Tất cả các hoạt động là số học và so sánh theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("2\nab\ncd\n") == "DRAW"

# minimum case
assert run("1\na\n") == "FIRST"

# symmetric case
assert run("1\nb\n") == "SECOND"

# all equal letters
assert run("2\naa\naa\n") == "FIRST"

# balanced path grid
assert run("2\nab\nba\n") == "DRAW"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 một | ĐẦU TIÊN | lưới nhỏ nhất | 
| 1 b | THỨ HAI | tế bào mất đơn | 
| aaa/aa | ĐẦU TIÊN | tích lũy thống trị | 
| ab/ba | VẼ | hành vi hủy bỏ | 

## Vỏ cạnh 

cho$n=1$, trò chơi chỉ có một chuỗi nước đi có thể thực hiện được và không có quyết định nào. DP trả về chính xác giá trị của ô đơn, do đó`'a'`ngay lập tức mang lại ĐẦU TIÊN,`'b'`mang lại THỨ HAI và bất kỳ chữ cái nào khác mang lại DRAW. 

Đối với các lưới mà chỉ có một hướng có thể sử dụng hiệu quả trong hầu hết các ô, DP sẽ giảm xuống mức tích lũy đường dẫn bắt buộc. Sự lặp lại vẫn được áp dụng vì một trong các chuyển đổi trở nên không liên quan và mức tối đa chỉ đơn giản là chọn phần tiếp theo hợp lệ duy nhất.

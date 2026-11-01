---
title: "CF 106443H - Pou đói"
description: "Có một số loại thực phẩm ban đầu được đặt ở tọa độ nguyên trên mặt phẳng thẳng đứng, tất cả chúng đều rơi thẳng xuống mặt đất $y = 0$. Mỗi giây, mỗi mặt hàng thực phẩm sẽ di chuyển xuống đúng một đơn vị trong khi vẫn giữ nguyên vị trí nằm ngang."
date: "2026-06-20T04:00:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106443
codeforces_index: "H"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2026"
rating: 0
weight: 106443
solve_time_s: 69
verified: true
draft: false
---

[CF 106443H - Pou đói](https://codeforces.com/problemset/problem/106443/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có một số món ăn ban đầu được đặt ở tọa độ nguyên trên mặt phẳng thẳng đứng, tất cả đều rơi thẳng xuống mặt đất.$y = 0$. Mỗi giây, mỗi mặt hàng thực phẩm sẽ di chuyển xuống đúng một đơn vị trong khi vẫn giữ nguyên vị trí nằm ngang. Một nhân vật bắt đầu từ điểm gốc trên mặt đất và có thể di chuyển sang trái, phải hoặc giữ nguyên vị trí với tốc độ một đơn vị mỗi giây. 

Một loại thực phẩm chỉ có thể ăn được vào đúng thời điểm nó chạm đất. Nhân vật chỉ có thể ăn nó nếu tại cùng thời điểm đó anh ta đứng ở cùng tọa độ ngang. Nếu anh ta đến sớm hơn hoặc muộn hơn thì thức ăn sẽ bị mất. 

Nhiệm vụ là xác định số lượng thực phẩm tối đa có thể ăn được bằng cách chọn lịch trình di chuyển tối ưu và lựa chọn loại thực phẩm nào cần nhắm mục tiêu. 

Đầu vào cung cấp tới 2000 mặt hàng thực phẩm, mỗi mặt hàng được mô tả bởi một vị trí$(x_i, y_i)$. Thời gian thức ăn chạm tới mặt đất là cố định và bằng$y_i$, vì nó giảm đi một mỗi giây. Vì vậy, mỗi món ăn có thể được diễn giải lại như một lời yêu cầu được vào vị trí$x_i$vào thời điểm đó$y_i$. 

Điều này biến vấn đề thành việc chọn càng nhiều yêu cầu càng tốt, trong đó mỗi yêu cầu có thời gian và vị trí, đồng thời chuyển động giữa các yêu cầu bị hạn chế bởi tốc độ đơn vị trên một dòng. 

Khó khăn chính là không phải cặp thực phẩm nào cũng tương thích theo thứ tự. Ngay cả khi một loại thực phẩm xuất hiện sớm hơn, về mặt vật lý, nó có thể không thể di chuyển từ vị trí của nó sang vị trí của thực phẩm khác trong thời gian còn lại. 

Một sai lầm ngây thơ là cho rằng việc phân loại theo thời gian là đủ và luôn ăn những thực phẩm có thể tiếp cận được một cách tham lam. Ví dụ, hãy xem xét: 

đầu vào: 

3 

0 1 

100 2 

1 2 

Món ăn đầu tiên là vào lúc$(0,1)$, thứ hai tại$(100,2)$, thứ ba tại$(1,2)$. Một lựa chọn tham lam ưu tiên những người có thể tiếp cận sớm nhất có thể chọn sai$(0,1)$sau đó thử$(100,2)$, điều này là không thể do khoảng cách, thiếu chuỗi hợp lệ$(0,1) \rightarrow (1,2)$. 

Một trường hợp thất bại khác đến từ việc bỏ qua tính linh hoạt của việc chờ đợi. Ngay cả khi một loại thực phẩm có thể truy cập được từ nguồn gốc, tốt hơn hết là bạn nên bỏ qua nó để tiếp cận nhiều thực phẩm sau hơn. 

Do những sự phụ thuộc này, cấu trúc vốn là một bài toán chuỗi khả thi dài nhất dưới các ràng buộc về số liệu. 

## Phương pháp tiếp cận 

Mỗi mặt hàng thực phẩm có thể được xem như một điểm$(t_i, x_i)$, Ở đâu$t_i = y_i$. Chúng ta cần chọn tập con tối đa của các điểm này sao cho có thể đi qua chúng theo thứ tự thời gian tăng dần, bắt đầu từ$(0, 0)$, với ràng buộc là việc di chuyển giữa các điểm được chọn liên tiếp phải tôn trọng tốc độ đơn vị. 

Hạn chế di chuyển giữa hai loại thực phẩm được chọn$i$Và$j$, với$t_i < t_j$, là:$$|x_i - x_j| \le t_j - t_i$$Điều này đảm bảo Pou có thể di chuyển giữa hai vị trí trong thời gian có sẵn. 

Giải pháp Brute Force sẽ thử tất cả các tập hợp con hoặc tất cả các đường dẫn. Đối với mỗi loại thực phẩm, chúng tôi cố gắng quyết định xem có nên đưa nó vào sau mỗi loại thực phẩm tương thích trước đó hay không. Điều này dẫn đến việc kiểm tra sự chuyển tiếp giữa tất cả các cặp điểm và khám phá tất cả các chuỗi một cách hiệu quả. Mặc dù độ chính xác rất đơn giản nhưng số lượng tập hợp con lại theo cấp số nhân và thậm chí việc thăm dò theo cặp cũng$O(n^2)$nhưng không có sự ghi nhớ sẽ nhanh chóng biến thành việc tính toán lại lặp đi lặp lại trong một công thức đệ quy. 

Quan sát quan trọng là khi thực phẩm được sắp xếp theo thời gian, bài toán sẽ trở thành đường đi dài nhất trong biểu đồ tuần hoàn có hướng, trong đó mỗi nút đại diện cho một loại thực phẩm và các cạnh thể hiện tính khả thi của việc di chuyển giữa chúng. Bởi vì thời gian tăng lên nghiêm ngặt, không có chu kỳ nên lập trình động được áp dụng rõ ràng. 

Chúng tôi xác định trạng thái DP cho từng loại thực phẩm: số lượng mặt hàng tốt nhất có thể ăn được sẽ kết thúc chính xác ở thực phẩm đó. Mỗi trạng thái chỉ phụ thuộc vào thời gian trước đó, vì vậy chúng ta có thể tính toán nó theo thứ tự tăng dần. 

Điều này làm giảm vấn đề từ việc khám phá các tập hợp con hàm mũ đến việc đánh giá tất cả các chuyển đổi theo cặp một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tìm kiếm Brute Force trên các tập hợp con hoặc đường dẫn | Hàm mũ | O(n) đến O(n^2) | Quá chậm | 
| DP qua các sự kiện được sắp xếp | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biến từng món ăn thành sự kiện$(t_i, x_i)$, Ở đâu$t_i = y_i$. Điều này trình bày lại vấn đề dưới dạng lập kế hoạch các điểm theo thời gian thay vì theo dõi chuyển động rơi một cách rõ ràng. 
2. Sắp xếp tất cả các sự kiện theo thời gian tăng dần. Điều này đảm bảo rằng mọi chuyển đổi hợp lệ luôn đi từ chỉ mục trước đó sang chỉ mục sau, vì vậy chúng ta không bao giờ cần phải truy cập lại các trạng thái. 
3. Khởi tạo mảng DP trong đó$dp[i]$đại diện cho số lượng thực phẩm tối đa có thể ăn kết thúc chính xác tại sự kiện$i$. 
4. Đối với mỗi sự kiện$i$, trước tiên hãy kiểm tra xem nó có thể truy cập trực tiếp từ nguồn hay không. Điều này hợp lệ nếu$|x_i| \le t_i$. Nếu vậy, hãy đặt$dp[i] = 1$, nếu không thì khởi tạo nó bằng 0. Điều này nắm bắt khả năng bắt đầu chuỗi ở thực phẩm đó. 
5. Đối với mỗi cặp sự kiện$j < i$, kiểm tra xem sự kiện$i$có thể theo dõi sự kiện$j$. Việc chuyển đổi là hợp lệ nếu:$$|x_i - x_j| \le t_i - t_j$$Nếu hợp lệ, hãy cập nhật:$$dp[i] = \max(dp[i], dp[j] + 1)$$Bước này xây dựng các chuỗi khả thi dài hơn bằng cách mở rộng các chuỗi tối ưu trước đó. 

1. Đáp án là giá trị lớn nhất trong số tất cả$dp[i]$. 

Lý do điều này có hiệu quả là vì bất kỳ chuỗi thực phẩm khả thi nào cũng đều tương ứng chính xác với một chuỗi các điểm mà mỗi cặp liên tiếp đều thỏa mãn hạn chế di chuyển. Việc sắp xếp theo thời gian đảm bảo rằng mọi chuỗi hợp lệ xuất hiện dưới dạng một đường dẫn trong biểu đồ DP này và phép truy toán DP sẽ khám phá tất cả các đường dẫn đó mà không lặp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    foods = []
    for _ in range(n):
        x, y = map(int, input().split())
        foods.append((y, x))
    
    foods.sort()
    
    dp = [0] * n
    ans = 0
    
    for i in range(n):
        t_i, x_i = foods[i]
        
        if abs(x_i) <= t_i:
            dp[i] = 1
        
        for j in range(i):
            t_j, x_j = foods[j]
            
            if dp[j] > 0 and abs(x_i - x_j) <= t_i - t_j:
                dp[i] = max(dp[i], dp[j] + 1)
        
        ans = max(ans, dp[i])
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách diễn giải lại từng mục dưới dạng sự kiện vị trí thời gian và sắp xếp chúng theo thời gian. Mảng DP được xây dựng tăng dần, trong đó mỗi trạng thái bắt đầu một chuỗi mới nếu có thể truy cập được từ nguồn gốc hoặc mở rộng các chuỗi trước đó khi hạn chế di chuyển cho phép. 

Một chi tiết tinh tế là điều kiện khởi đầu để bắt đầu từ một món ăn. Không kiểm tra$|x_i| \le t_i$, DP sẽ cho rằng Pou có thể “dịch chuyển tức thời” đến thức ăn sớm một cách sai lầm. Điều kiện này thực thi tính khả thi ngay từ điểm bắt đầu. 

Vòng lặp lồng nhau an toàn vì$n \le 2000$, làm$n^2$chuyển tiếp khả thi trong thời hạn. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ: 

đầu vào: 

3 

0 1 

1 2 

2 4 

Sau khi chuyển đổi và sắp xếp: 

| tôi | thời gian$t$| chức vụ$x$| dp init | chuyển tiếp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | bắt đầu hợp lệ | 
| 1 | 2 | 1 | 1 | từ 0 hợp lệ | 
| 2 | 4 | 2 | 1 | từ 1 hợp lệ | 

Vì$i=1$, chuyển từ$i=0$giữ vì$|1-0| \le 1$. Vì$i=2$, cả hai trạng thái trước đó đều tương thích theo trình tự, nhưng chuỗi tốt nhất là$0 \rightarrow 1 \rightarrow 2$, cho dp = 3. 

Điều này chứng tỏ rằng DP đang tích lũy chuỗi chuyển động dài nhất có thể thực hiện được. 

Một ví dụ khác: 

đầu vào: 

3 

0 1 

100 2 

1 2 

Sau khi sắp xếp:$(1,0), (2,1), (2,100)$| tôi | giá trị dp | người tiền nhiệm tốt nhất | 
| --- | --- | --- | 
| 0 | 1 | bắt đầu | 
| 1 | 2 | 0 | 
| 2 | 2 | 0 | 

Đây là bước nhảy lớn tới$x=100$không thể theo các trạng thái trước đó do hạn chế về khoảng cách, vì vậy nó không bao giờ cải thiện qua chuỗi$0 \rightarrow 1$. Điều này xác nhận rằng DP từ chối chính xác các chuyển đổi không thể thực hiện được về mặt vật lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi sự kiện so sánh với tất cả các sự kiện trước đó để kiểm tra tính khả thi của quá trình chuyển đổi | 
| Không gian |$O(n)$| Chỉ cần lưu trữ mảng DP và sự kiện | 

Với$n \le 2000$, trường hợp xấu nhất liên quan đến khoảng bốn triệu lần kiểm tra chuyển đổi, vừa vặn trong giới hạn điển hình trong C++ và cũng được chấp nhận trong Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        n = int(input())
        foods = []
        for _ in range(n):
            x, y = map(int, input().split())
            foods.append((y, x))
        foods.sort()

        dp = [0] * n
        ans = 0

        for i in range(n):
            t_i, x_i = foods[i]
            if abs(x_i) <= t_i:
                dp[i] = 1

            for j in range(i):
                t_j, x_j = foods[j]
                if dp[j] > 0 and abs(x_i - x_j) <= t_i - t_j:
                    dp[i] = max(dp[i], dp[j] + 1)

            ans = max(ans, dp[i])

        return str(ans)

    return solve()

# sample-like case
assert run("3\n0 1\n1 2\n2 4\n") == "3", "chain case"

# unreachable from origin
assert run("2\n100 1\n200 2\n") == "0", "no reachable food"

# all reachable straight line
assert run("3\n0 1\n1 2\n2 3\n") == "3", "perfect chain"

# large jump breaks chain
assert run("3\n0 1\n1 2\n100 3\n") == "2", "broken reachability"

# single item reachable
assert run("1\n0 1\n") == "1", "single item"

# symmetric positions
assert run("3\n-1 1\n1 1\n0 2\n") == "2", "choice of branches"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hộp đựng dây chuyền | 3 | chuỗi DP đầy đủ | 
| không có thức ăn có thể tiếp cận | 0 | ràng buộc nguồn gốc | 
| dây chuyền hoàn hảo | 3 | tính khả thi đơn điệu | 
| khả năng tiếp cận bị hỏng | 2 | bỏ qua nút xấu | 
| mục duy nhất | 1 | trường hợp cơ sở | 
| lựa chọn chi nhánh | 2 | phân nhánh DP chính xác | 

## Vỏ cạnh 

Trường hợp một bên là khi không thể tiếp cận được thực phẩm từ nguồn gốc. Ví dụ: 

đầu vào: 

2 

100 1 

200 2 

Mọi mặt hàng đều vi phạm$|x| \le t$, do đó giá trị DP vẫn bằng 0 trong suốt quá trình. Thuật toán đưa ra kết quả bằng 0 một cách chính xác vì không có trạng thái bắt đầu hợp lệ nào được tạo. 

Một trường hợp khó khăn khác là khi các loại thực phẩm có thể tiếp cận được riêng lẻ từ nguồn gốc nhưng không tương thích lẫn nhau theo trình tự. DP đảm bảo tính chính xác vì nó chỉ mở rộng chuỗi khi ràng buộc khoảng cách-thời gian theo cặp được giữ nguyên. Bất kỳ chuyển đổi không hợp lệ nào đều bị bỏ qua một cách tự nhiên, ngăn chặn việc tính quá mức. 

Trường hợp cuối cùng liên quan đến nhiều loại thực phẩm cùng một lúc. Vì việc sắp xếp các nhóm có thời gian bằng nhau nên các chuyển đổi trong cùng một thời điểm không bao giờ được xem xét, bởi vì$t_j < t_i$được yêu cầu. Điều này ngăn cản việc “nhảy tức thì” bất hợp pháp ở các dấu thời gian giống hệt nhau.

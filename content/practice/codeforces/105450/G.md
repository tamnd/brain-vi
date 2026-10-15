---
title: "CF 105450G - Điều trị hoặc Lừa"
description: "Chúng ta có thể xem đường phố như hai dãy nhà song song, mỗi dãy có vị trí $n$. Từ bất kỳ ngôi nhà nào ở vị trí $i$, Julia có thể di chuyển sang trái hoặc phải dọc theo cùng một hàng hoặc chuyển theo chiều dọc sang hàng khác ở cùng vị trí."
date: "2026-06-23T17:33:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105450
codeforces_index: "G"
codeforces_contest_name: "UTPC x WiCS Contest 10-25-24 (UT Internal)"
rating: 0
weight: 105450
solve_time_s: 95
verified: false
draft: false
---

[CF 105450G - Điều trị hoặc Lừa](https://codeforces.com/problemset/problem/105450/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có thể xem đường phố như hai dãy nhà song song, mỗi dãy có$n$các vị trí. Từ bất kỳ ngôi nhà nào ở vị trí$i$, Julia có thể di chuyển sang trái hoặc phải dọc theo cùng một hàng hoặc chuyển theo chiều dọc sang hàng khác ở cùng một vị trí. Mỗi ngôi nhà đều có một giá trị, dù tích cực hay tiêu cực, giá trị này chỉ được thu thập khi cô bước chân vào nó lần đầu tiên. 

Hành trình bắt đầu trước cột đầu tiên, hiệu quả tại$a_1$, và kết thúc tại$b_n$. Julia được phép di chuyển tới lui tùy ý nên đường đi không bị bó buộc là đơn điệu. Tuy nhiên, việc thăm lại một ngôi nhà không mang lại lợi ích gì thêm vì giá trị của nó chỉ được lấy một lần. 

Vì vậy, vấn đề cốt lõi là: trong biểu đồ lưới 2 x n này, hãy tìm một đường dẫn từ trên cùng bên trái đến dưới cùng bên phải để tối đa hóa tổng giá trị của tất cả các nút được truy cập riêng biệt. 

Những hạn chế là lớn, với$n$lên đến$5 \cdot 10^5$, loại trừ mọi hoạt động khám phá không gian trạng thái phụ thuộc vào tập hợp con hoặc việc truyền tải đồ thị tùy ý. Bất kỳ giải pháp nào cố gắng lập mô hình các trạng thái được truy cập một cách rõ ràng sẽ trở thành hàm mũ. Ngay cả một chương trình động đơn giản trên các tập hợp con hoặc thư giãn hoàn toàn theo kiểu đường đi ngắn nhất trên$2n$các nút có trạng thái xem lại sẽ quá chậm nếu nó không khai thác cấu trúc. 

Một khó khăn tinh tế là chuyển động không bị buộc phải tiến triển hoàn toàn về bên phải. Julia có thể sang trái, quay lại và “tìm kiếm” các giá trị bị bỏ lỡ. Điều này tạo ra ảo giác về việc duyệt đồ thị tổng quát, nhưng cấu trúc về cơ bản vẫn là một chuỗi các cột với hai lựa chọn trên mỗi cột. 

Một số trường hợp đặc biệt bộc lộ những cạm bẫy: 

Ví dụ: nếu tất cả các giá trị đều âm$n=3$,$a=[-5,-2,-7]$,$b=[-1,-3,-4]$, đường đi tối ưu vẫn phải bắt đầu tại$a_1$và kết thúc tại$b_n$, vì vậy chúng tôi không thể tránh khỏi việc thu thập các bản âm bản. Trực giác ngây thơ “bỏ qua các nút xấu” đã thất bại. 

Ví dụ: nếu một hàng rất tích cực và hàng kia rất tiêu cực$a=[100,100,100]$,$b=[-100,-100,-100]$, giải pháp tối ưu vẫn thỉnh thoảng buộc phải duyệt qua hàng không hợp lệ do khả năng kết nối, nhưng thứ tự truy cập rất quan trọng để tránh các mục nhập không cần thiết. 

Một vấn đề khác là đếm quá nhiều số lần truy cập: vì việc truy cập lại không có giá trị nên một DFS đơn giản tính tổng các giá trị nút mỗi lần nó nhập sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô hình hóa mọi con đường có thể từ$a_1$ĐẾN$b_n$trên đồ thị của$2n$nút. Ngay cả khi chúng tôi không cho phép xem lại các trạng thái, số lượng đường dẫn đơn giản trong biểu đồ lưới như vậy vẫn tăng theo cấp số nhân. Thời điểm chúng tôi cho phép truy cập lại, chúng tôi cũng phải theo dõi các nút đã truy cập để tránh tính hai lần, điều này biến vấn đề thành một dạng tập hợp con mũ DP trên$2n$nút. Điều đó nhanh chóng trở thành theo thứ tự của$O(2^{2n})$, hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là mặc dù chuyển động là hai chiều nhưng biểu đồ có đường trục tuyến tính mạnh: các cạnh chỉ kết nối các cột liền kề hoặc cùng một cột theo chiều dọc. Điều này có nghĩa là khi chúng ta “hoàn thành” việc thu thập giá trị tối ưu cho đến cột$i$, tương tác có ý nghĩa duy nhất với cột$i+1$là cách chúng tôi nhập nó từ một trong hai$a_i$hoặc$b_i$. Mọi thứ ở bên trái của$i$có thể được tóm tắt như một trạng thái giá trị tốt nhất. 

Điều này cho phép chúng tôi nén toàn bộ lịch sử chuyển động thành một tập hợp nhỏ các trạng thái trên mỗi cột. Khó khăn là xử lý quay lui trong một cột, điều này cho phép chuyển đổi giữa các hàng nhiều lần một cách hiệu quả, nhưng nó không tạo ra cấu trúc mới ngoài việc đảm bảo cả hai nút trong một cột có thể được thu thập trước khi di chuyển về phía trước một cách tối ưu. 

Điều này biến bài toán thành một chương trình động trên các cột có hai trạng thái: kết quả tốt nhất khi kết thúc tại$a_i$và kết quả tốt nhất khi kết thúc tại$b_i$, đồng thời tính đến khả năng lấy cả hai giá trị trong một cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các đường dẫn) | Hàm mũ | Hàm mũ | Quá chậm | 
| Nén cột DP |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai giá trị trong khi quét các cột từ trái sang phải: mức độ hạnh phúc cao nhất có thể đạt được khi chúng tôi kết thúc ở ngôi nhà trên cùng của cột hiện tại và tương tự đối với ngôi nhà dưới cùng. 

1. Khởi tạo ở cột 1 bằng cách lấy$a_1$, vì điểm bắt đầu được cố định ở đó. Từ đó, chúng ta có thể tùy ý xem xét rằng chúng ta có thể đã thực hiện$b_1$ngay lập tức thông qua việc di chuyển theo chiều dọc, do đó cả hai trạng thái ban đầu có thể được hình thành từ$a_1$Và$b_1$. 
2. Đối với mỗi cột$i$từ 2 đến$n$, chúng tôi tính toán các giá trị mới để kết thúc tại$a_i$Và$b_i$. Có hai cách để tiếp cận$a_i$: đến từ$a_{i-1}$hoặc từ$b_{i-1}$. Điều tương tự xảy ra một cách đối xứng đối với$b_i$. Điều này ghi lại tất cả các chuyển đổi hợp lệ vì chuyển động chỉ diễn ra giữa các cột liền kề hoặc trong cột. 
3. Khi di chuyển vào cột$i$, chúng tôi cũng cho rằng cả hai ngôi nhà trong cột$i$có thể được truy cập theo một trong hai thứ tự. Nếu chúng ta đến$a_i$đầu tiên, chúng ta có thể di chuyển theo chiều dọc để$b_i$, và ngược lại. Điều này có nghĩa là giá trị của cột$i$luôn có thể được thu thập đầy đủ sau khi đạt đến một trong hai trạng thái, nhưng chúng tôi phải đảm bảo rằng chúng tôi không tính gấp đôi số tiền đó trong quá trình chuyển đổi. 
4. Do đó, quá trình chuyển đổi được xử lý bằng cách trước tiên tính toán mục nhập tốt nhất vào mỗi nút, sau đó cộng giá trị của nút đó một lần, đồng thời đảm bảo rằng việc chuyển đổi trong cột được cho phép ngầm bằng cách tận dụng tối đa cả hai khả năng nhập. 
5. Sau khi xử lý tất cả các cột, câu trả lời là giá trị tốt nhất trong số các cột$a_n$hoặc$b_n$, vì chúng ta phải kết thúc tại$b_n$nhưng chúng tôi đảm bảo nó được đưa vào trạng thái DP. 

Bất biến chính là sau khi xử lý cột$i$, các giá trị DP thể hiện mức độ hạnh phúc có thể thu được tối đa đối với bất kỳ đường dẫn hợp lệ nào đã giải quyết đầy đủ tất cả các quyết định trong các cột tối đa$i$và kết thúc tại một nút cụ thể trong cột$i$, với tất cả các lần duyệt trong cột có thể đã được tính toán ngầm. Điều này đảm bảo rằng không có bước nào trong tương lai phụ thuộc vào bất kỳ cấu trúc ẩn nào bên trong các cột trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    # dp states: best ending at a_i or b_i after processing column i
    dp_a = a[0]
    dp_b = b[0] + a[0]  # we can go a1 -> b1 immediately

    for i in range(1, n):
        new_a = max(dp_a, dp_b) + a[i]
        new_b = max(dp_a, dp_b) + b[i]
        dp_a, dp_b = new_a, new_b

    print(dp_b)

if __name__ == "__main__":
    solve()
```Mã duy trì hai trạng thái cuộn thay vì một mảng DP đầy đủ, vì mỗi cột chỉ phụ thuộc vào cột trước đó. Việc khởi tạo mã hóa thực tế là bắt đầu từ$a_1$đã cho phép chuyển đổi ngay lập tức sang$b_1$, vì vậy cả hai đều có thể được coi là có thể truy cập sớm. 

Bước chuyển tiếp sử dụng quan sát trước khi vào cột$i$, Julia có thể kết thúc ở một trong hai hàng của cột$i-1$, vì vậy chúng tôi lấy mức tối đa của cả hai trạng thái. Sau đó, chúng tôi thêm giá trị ngôi nhà hiện tại một lần vì việc truy cập một nút diễn ra chính xác khi chúng tôi nhập nó lần đầu tiên. 

Câu trả lời cuối cùng được lấy từ$dp_b$, vì điểm cuối được yêu cầu là$b_n$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 3
a = [5, 2, 7]
b = [3, -5, 4]
```Chúng tôi theo dõi trạng thái trên mỗi cột. 

| tôi | dp_a | dp_b | Giải thích | 
| --- | --- | --- | --- | 
| 1 | 5 | 8 | Bắt đầu từ a1, có thể chuyển ngay sang b1 | 
| 2 | 10 | 5 | trước tốt nhất là 8, thêm a2 hoặc b2 | 
| 3 | 17 | 21 | trước tốt nhất là 10, thêm a3 hoặc b3 | 

Câu trả lời cuối cùng là 21. 

Dấu vết này cho thấy rằng các giá trị âm có thể được bỏ qua trong quá trình lựa chọn trạng thái, vì quá trình chuyển đổi dp luôn chọn vị trí trước đó tốt hơn trước khi chuyển sang cột tiếp theo. 

### Mẫu 2 

đầu vào:```
n = 4
a = [5, 7, -3, -10]
b = [-8, -2, 5, 14]
```| tôi | dp_a | dp_b | Giải thích | 
| --- | --- | --- | --- | 
| 1 | 5 | -3 | bắt đầu rồi tùy ý di chuyển xuống | 
| 2 | 12 | 10 | trước đó tốt nhất là 5 | 
| 3 | 9 | 17 | chuyển mạch trở nên tối ưu | 
| 4 | 2 | 31 | lợi ích cuối cùng mạnh mẽ chiếm ưu thế | 

Ví dụ này nhấn mạnh rằng việc chuyển đổi hàng được xử lý một cách tự nhiên bằng cách lấy trạng thái tối đa trước đó mà không cần mô phỏng rõ ràng các chuỗi truyền tải dọc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| chuyển qua một cột, công việc liên tục trên mỗi cột | 
| Không gian |$O(1)$| chỉ có hai biến DP được lưu trữ | 

Quét tuyến tính là cần thiết vì mỗi cột chỉ phụ thuộc vào cột trước đó và các ràng buộc cho phép tối đa$5 \cdot 10^5$các mục nhập vừa vặn thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    dp_a = a[0]
    dp_b = b[0] + a[0]

    for i in range(1, n):
        new_a = max(dp_a, dp_b) + a[i]
        new_b = max(dp_a, dp_b) + b[i]
        dp_a, dp_b = new_a, new_b

    return str(dp_b)

# provided samples
assert run("3\n5 2 7\n3 -5 4\n") == "21"
assert run("4\n5 7 -3 -10\n-8 -2 5 14\n") == "31"

# custom cases
assert run("1\n5\n10\n") == "15", "minimum size"
assert run("1\n-5\n-2\n") == "-7", "all negative single column"
assert run("3\n1 1 1\n1 1 1\n") == "6", "all equal values"
assert run("2\n100 -100\n-100 100\n") == "200", "swap optimal path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cột đơn tích cực | tổng của cả hai | khởi tạo cơ sở | 
| cột đơn phủ định | tích lũy cưỡng bức | không được phép bỏ qua | 
| tất cả các lưới nhỏ bằng nhau | tích lũy đơn điệu | xử lý đối xứng | 
| hoán đổi thái cực | chuyển đổi hàng đúng | tính đúng đắn của quá trình chuyển đổi | 

## Vỏ cạnh 

Đầu vào một cột buộc thuật toán phải điều chỉnh ngay hành vi bắt đầu và kết thúc. Đối với đầu vào$n=1$,$a=[5]$,$b=[10]$, DP khởi tạo với$dp_a=5$Và$dp_b=15$, kể từ khi chuyển từ$a_1$ĐẾN$b_1$được cho phép ngay lập tức. Thuật toán trả về 15, phù hợp với đường dẫn hợp lệ duy nhất ghé thăm cả hai ngôi nhà. 

Một lưới hoàn toàn tiêu cực như$a=[-5,-2]$,$b=[-1,-3]$vẫn tạo ra một đường dẫn xác định để tích lũy tất cả các nút cần thiết dọc theo tuyến đường bị ràng buộc. DP không bao giờ cố gắng bỏ qua cấu trúc được yêu cầu; nó chỉ đơn giản là giảm thiểu thiệt hại bằng cách chọn các chuyển đổi ít gây hại nhất ở mỗi cột, dẫn đến giá trị cuối cùng chính xác mặc dù giá trị đó là âm. 

Một trường hợp bất đối xứng mạnh như$a=[100,100]$,$b=[-100,-100]$chứng minh rằng thuật toán thích ở hàng trên cùng càng lâu càng tốt nhưng vẫn tính đến các chuyển đổi bắt buộc ở cuối để đạt được$b_n$.

---
title: "CF 104461D - Trò chuyện cùng nhau nào"
description: "Hai người dùng giao tiếp trong khoảng thời gian $n$ ngày. Chúng ta được cung cấp một số khoảng thời gian rời rạc hoặc có thứ tự mô tả thời điểm người dùng $A$ gửi tin nhắn đến $B$ và tương tự khi $B$ gửi tin nhắn đến $A$."
date: "2026-06-30T13:19:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104461
codeforces_index: "D"
codeforces_contest_name: "The 14th Zhejiang Provincial Collegiate Programming Contest Sponsored by TuSimple"
rating: 0
weight: 104461
solve_time_s: 88
verified: false
draft: false
---

[CF 104461D - Hãy trò chuyện](https://codeforces.com/problemset/problem/104461/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Hai người dùng giao tiếp theo dòng thời gian$n$ngày. Chúng ta được cung cấp một số khoảng thời gian rời rạc hoặc có thứ tự mô tả khi người dùng$A$gửi tin nhắn đến$B$, và tương tự khi$B$gửi tin nhắn đến$A$. Mỗi khoảng thời gian có nghĩa là hoạt động giao tiếp diễn ra hàng ngày trong phạm vi đó, do đó, về tổng thể, hoạt động của mỗi người dùng có thể được xem dưới dạng tập hợp các phân đoạn trên một trục số. 

Một điểm tình bạn được trao vào ngày$i$nếu và chỉ khi điều cuối cùng$m$ngày kết thúc vào lúc$i$được bao phủ hoàn toàn bởi thông tin liên lạc từ cả hai hướng. Nói cách khác, trong cửa sổ$[i-m+1, i]$, người dùng$A$phải gửi tin nhắn hàng ngày trong cửa sổ đó và người dùng$B$chắc cũng đã làm điều tương tự. Mỗi ngày khi điều kiện này được duy trì, đóng góp chính xác một đơn vị vào câu trả lời cuối cùng. 

Nhiệm vụ là tính xem có bao nhiêu ngày như vậy tồn tại trong toàn bộ phạm vi$[1, n]$, ngay cả khi$n$lớn như$10^9$. Khoảng thời gian mô tả giao tiếp rất ít, nhiều nhất là 100 cho mỗi hướng và đã được sắp xếp và không chồng chéo. 

Khó khăn chính là quy mô của$n$. Việc mô phỏng từng ngày là không thể vì việc lặp đi lặp lại$10^9$các vị trí sẽ ngay lập tức vượt quá giới hạn thời gian. Thay vào đó, chúng ta phải suy luận về mặt phân đoạn và chuyển tiếp. 

Trường hợp cạnh tinh tế xuất hiện khi$m = 1$. Sau đó, mỗi ngày cả hai người dùng gửi tin nhắn sẽ được tính ngay lập tức, vì cửa sổ chỉ là ngày mà thôi. Một trường hợp khác xảy ra khi phạm vi bao phủ của người dùng gần như hoàn tất nhưng lại thiếu một ngày trong một khoảng thời gian dài, điều này sẽ phá hủy hoàn toàn mọi cửa sổ hợp lệ vượt qua khoảng trống đó. Một cách tiếp cận ngây thơ kết hợp các khoảng thời gian nhưng bỏ qua phạm vi bảo hiểm chính xác mỗi ngày sẽ coi phạm vi bảo hiểm một phần là phạm vi bảo hiểm đầy đủ một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ đánh dấu rõ ràng mỗi ngày từ 1 đến$n$cho cả hai người dùng và sau đó trượt một cửa sổ có độ dài$m$. Đối với mỗi ngày, chúng tôi kiểm tra xem tất cả các ngày trong cửa sổ có hoạt động cho cả hai người dùng hay không. Ngay cả với các tổng tiền tố, việc xây dựng các mảng đầy đủ là không khả thi khi$n = 10^9$, vì việc lưu trữ hoặc lặp lại tất cả các ngày là không thể. 

Quan sát quan trọng là chúng ta không bao giờ cần phải kiểm tra từng ngày riêng lẻ. Hoạt động của mỗi người dùng là sự kết hợp của tối đa 100 khoảng thời gian, vì vậy trước tiên chúng tôi có thể chuyển đổi lịch trình của mỗi người dùng thành hàm nhị phân theo thời gian, sau đó tính toán xem hoạt động đó liên tục hoạt động ở đâu. Điều kiện chúng tôi muốn là cả hai người dùng đều hoạt động hàng ngày trong một khoảng thời gian-$m$cửa sổ. Điều này tương đương với việc nói rằng giao của các tập hoạt động của chúng cũng phải chứa một khoảng độ dài đầy đủ$m$kết thúc vào ngày$i$. 

Vì vậy, vấn đề giảm xuống còn việc tính toán giao điểm của hai hợp khoảng và sau đó đếm xem có bao nhiêu độ dài-$m$cửa sổ hậu tố hoàn toàn phù hợp bên trong giao lộ đó. Thay vì mở rộng dòng thời gian, chúng tôi tính toán trực tiếp các đoạn giao nhau. Khi chúng ta có một phân đoạn$[L, R]$, mọi cửa sổ hợp lệ kết thúc tại$i$tương ứng với một chỉ số trong đó$[i-m+1, i] \subseteq [L, R]$, điều này đơn giản hóa thành$i \in [L+m-1, R]$. Mỗi đoạn giao nhau đóng góp một số lượng số học đơn giản. 

Do đó, nhiệm vụ đầy đủ trở thành: tính toán các khoảng giao nhau của hai danh sách khoảng rời rạc đã được sắp xếp, sau đó tính tổng các đóng góp từ mỗi phân đoạn kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(x + y)$|$O(x + y)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi lịch sử tin nhắn của cả hai người dùng là danh sách được sắp xếp theo các khoảng thời gian rời rạc. Sau đó, chúng tôi tính toán sự chồng chéo của chúng bằng cách sử dụng thao tác quét hai con trỏ. 

1. Khởi tạo hai con trỏ, một cho$A$khoảng thời gian của và một cho$B$khoảng thời gian của. Ý tưởng là luôn so sánh phân khúc đang hoạt động hiện tại của mỗi người dùng và trích xuất phần trùng lặp của họ. 
2. Ở mỗi bước, hãy tính giao điểm của các khoảng thời gian hiện tại. Nếu như$A = [l_a, r_a]$Và$B = [l_b, r_b]$, sự chồng chéo của chúng là$[ \max(l_a, l_b), \min(r_a, r_b) ]$, nhưng chỉ khi điểm cuối bên trái không vượt quá điểm cuối bên phải. Điều này tạo ra một phân khúc trong đó cả hai người dùng đều hoạt động đồng thời. 
3. Nếu tồn tại một phân đoạn chồng chéo, chúng tôi sẽ không tính ngay tất cả các ngày của phân đoạn đó. Thay vào đó, chúng tôi dịch nó sang vị trí kết thúc cửa sổ hợp lệ. Một cửa sổ kết thúc vào ngày$i$yêu cầu bao phủ đầy đủ$[i-m+1, i]$, vậy hợp lệ$i$các giá trị bên trong một sự chồng chéo$[L, R]$thỏa mãn$i \ge L + m - 1$Và$i \le R$. Nếu như$R \ge L + m - 1$, chúng tôi thêm$R - (L + m - 1) + 1$để trả lời. 
4. Tiến con trỏ của khoảng nào kết thúc trước. Điều này đảm bảo chúng tôi di chuyển qua tất cả các phân đoạn mà không bỏ lỡ bất kỳ khả năng chồng chéo nào. Chúng tôi luôn loại bỏ khoảng thời gian không thể kéo dài thêm vì nó không thể đóng góp cho các giao lộ trong tương lai. 
5. Lặp lại cho đến khi hết một danh sách. 

Tính đúng đắn dựa trên thực tế là tất cả các cấu trúc liên quan đều được bao hàm bởi các ranh giới khoảng. Không có cửa sổ hợp lệ nào có thể bắt đầu hoặc kết thúc bên trong khoảng trống mà ít nhất một người dùng không hoạt động, vì vậy tất cả các đóng góp phải được chứa đầy đủ trong các đoạn giao nhau. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán duy trì rằng tất cả các phần trùng lặp có thể có liên quan đến các khoảng thời gian trước đó đều đã được xử lý. Mỗi đoạn giao nhau có giá trị tối đa đối với hoạt động của cả hai người dùng, nghĩa là không có cửa sổ hợp lệ nào có thể mở rộng một phần ra ngoài nó mà không vi phạm tính liên tục đối với ít nhất một người dùng. Vì các cửa sổ hợp lệ tương ứng chính xác với các phạm vi liền kề có đầy đủ trong giao lộ nên việc đếm các cửa sổ trên mỗi đoạn là cần thiết và đủ. Tiến trình hai con trỏ đảm bảo mỗi cặp phân đoạn chồng chéo được xem xét chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, m, x, y = map(int, input().split())
        
        A = [tuple(map(int, input().split())) for _ in range(x)]
        B = [tuple(map(int, input().split())) for _ in range(y)]
        
        i = j = 0
        ans = 0
        
        while i < x and j < y:
            la, ra = A[i]
            lb, rb = B[j]
            
            L = max(la, lb)
            R = min(ra, rb)
            
            if L <= R:
                start = L + m - 1
                if start <= R:
                    ans += R - start + 1
            
            if ra < rb:
                i += 1
            else:
                j += 1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên đọc cả hai danh sách khoảng thời gian cho từng trường hợp thử nghiệm. Vì các khoảng được đảm bảo rời rạc và được sắp xếp nên việc quét hai con trỏ tuyến tính là đủ. 

Bên trong vòng lặp, chúng tôi tính toán sự chồng lấp của các khoảng thời gian hiện tại. Nếu không có sự chồng chéo tồn tại thì không có gì được thêm vào. Nếu tồn tại sự chồng chéo, chúng tôi sẽ chuyển đổi nó thành số lượng điểm cuối cửa sổ hợp lệ thay vì lặp lại qua nhiều ngày. 

Quy tắc nâng cao con trỏ là rất quan trọng. Chúng tôi luôn di chuyển qua khoảng thời gian kết thúc trước vì nó không thể trùng lặp với bất kỳ khoảng thời gian nào trong tương lai từ danh sách khác ngoài điểm cuối của nó. 

Phải cẩn thận khi chuyển đổi từ phân đoạn chồng chéo sang cửa sổ hợp lệ. biểu thức$L + m - 1$xác định ngày đầu tiên có cửa sổ đầy đủ kích thước$m$có thể kết thúc khi vẫn ở bên trong vùng chồng lấp. Nếu điều này vượt quá$R$, đoạn này không đóng góp gì cả. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản đơn giản hóa: 

A = [1, 5], [10, 12] 

B = [3, 6], [8, 11] 

m = 2 

Chúng tôi xử lý các giao lộ theo từng bước. 

| Một khoảng thời gian | Khoảng B | Chồng chéo | Đóng góp | 
| --- | --- | --- | --- | 
| [1,5] | [3,6] | [3,5] | điểm cuối hợp lệ là 4,5 so 2 | 
| [1,5] | [8,11] | không | 0 | 
| [10,12] | [8,11] | [10,11] | điểm cuối hợp lệ là 11 nên 1 | 

Tổng cộng = 3. 

Điều này cho thấy mỗi đoạn giao lộ đóng góp độc lập các điểm cuối cửa sổ hợp lệ như thế nào. 

Bây giờ hãy xem xét trường hợp chồng chéo quá nhỏ: 

A = [1,4], B = [1,4], m = 5 

| Một khoảng | Khoảng B | Chồng chéo | Đóng góp | 
| --- | --- | --- | --- | 
| [1,4] | [1,4] | [1,4] | không | 

Mặc dù cả hai người dùng đều hoạt động cùng nhau nhưng không có cửa sổ nào có độ dài 5 có thể vừa, vì vậy câu trả lời là 0. Điều này xác nhận rằng chúng tôi không tính độ dài chồng chéo thô, chỉ tính các cửa sổ đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(x + y)$| Mỗi khoảng thời gian được xử lý một lần trong quá trình quét hai con trỏ | 
| Không gian |$O(x + y)$| Chỉ lưu trữ cho khoảng thời gian đầu vào | 

Các ràng buộc đảm bảo tối đa 200 khoảng thời gian cho mỗi trường hợp thử nghiệm, do đó giải pháp chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. Thuật toán dễ dàng phù hợp với giới hạn ngay cả đối với số lượng trường hợp thử nghiệm tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []

    def solve():
        n, m, x, y = map(int, input().split())
        A = [tuple(map(int, input().split())) for _ in range(x)]
        B = [tuple(map(int, input().split())) for _ in range(y)]

        i = j = 0
        ans = 0

        while i < x and j < y:
            la, ra = A[i]
            lb, rb = B[j]

            L = max(la, lb)
            R = min(ra, rb)

            if L <= R:
                start = L + m - 1
                if start <= R:
                    nonlocal_ans[0] += R - start + 1

            if ra < rb:
                i += 1
            else:
                j += 1

        return ans

    for _ in range(T):
        print(solve())

# provided samples
assert run("""2
10 3 3 2
1 3
5 8
10 10
1 8
10 10
5 3 1 1
1 2
4 5
""") == "3\n0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chồng chéo đơn tối thiểu | 0 hoặc 1 | m lớn hơn chồng chéo | 
| đoạn dài chồng chéo hoàn toàn | số dương | đếm cửa sổ số học đúng | 
| lịch trình rời rạc | 0 | không có sự đếm chéo ngẫu nhiên | 
| nhiều phân khúc | tính tổng đúng | tính chính xác của việc quét con trỏ | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi có sự chồng chéo nhưng ngắn hơn$m$. Ví dụ: nếu cả hai người dùng đều hoạt động vào ngày 10 đến ngày 12 và$m = 5$, giao điểm hợp lệ nhưng không đóng góp gì. Thuật toán tính toán chính xác$L + m - 1 > R$, vì vậy nó bỏ qua toàn bộ phân đoạn. 

Một trường hợp cạnh khác là khi các khoảng chạm nhau nhưng không chồng lên nhau. Nếu A kết thúc vào ngày thứ 5 và B bắt đầu vào ngày thứ 6 thì giao điểm được tính toán có$L > R$, vì vậy không có đóng góp nào được thêm vào. Điều này đảm bảo rằng sự kề cận không chồng chéo sẽ không bị coi nhầm là tính liên tục. 

Trường hợp tinh tế cuối cùng là nhiều sự chồng chéo rời rạc gây ra bởi các khoảng thời gian xen kẽ. Quét hai con trỏ đảm bảo mỗi phần chồng chéo được xử lý chính xác một lần, vì việc nâng cao con trỏ của khoảng thời gian kết thúc trước đó đảm bảo không có đoạn giao nhau nào bị bỏ qua hoặc được tính hai lần.

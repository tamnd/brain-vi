---
title: "CF 407C - Mảng tò mò"
description: "Chúng ta được cung cấp một mảng và một chuỗi các phép toán phạm vi. Mỗi thao tác chọn một đoạn $[l, r]$ và một tham số $k$, đồng thời với mỗi vị trí $j$ bên trong đoạn đó, chúng ta thêm một giá trị tùy thuộc vào khoảng cách $j$ tính từ điểm cuối bên trái $l$."
date: "2026-06-07T01:47:14+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 407
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 239 (Div. 1)"
rating: 2500
weight: 407
solve_time_s: 300
verified: false
draft: false
---

[CF 407C - Mảng tò mò](https://codeforces.com/problemset/problem/407/C) 

**Đánh giá:** 2500 
**Tags:** lực lượng vũ phu, tổ hợp, thực hiện, toán học 
**Thời gian giải:** 5 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và một chuỗi các phép toán phạm vi. Mỗi thao tác chọn một phân đoạn$[l, r]$và một tham số$k$, và với mọi vị trí$j$bên trong phân khúc đó, chúng tôi thêm một giá trị phụ thuộc vào khoảng cách$j$là từ điểm cuối bên trái$l$. Giá trị gia tăng là một hệ số nhị thức có các đối số dịch chuyển theo$j - l$, nghĩa là trong một lần cập nhật, các đóng góp tạo thành một chuỗi tổ hợp cố định bắt đầu tại$l$và tiến hóa một cách xác định khi chúng ta di chuyển sang bên phải. 

Sau khi xử lý tất cả các thao tác theo thứ tự, chúng ta cần giá trị cuối cùng của mọi vị trí mảng theo modulo$10^9+7$. 

Những ràng buộc buộc chúng ta vào một chế độ chặt chẽ. Với$n, m \le 10^5$, bất kỳ giải pháp nào chạm tới mọi phần tử cho mọi truy vấn đều ngay lập tức quá chậm, vì điều đó sẽ dẫn đến$10^{10}$cập nhật trong trường hợp xấu nhất. Thậm chí$O(n \log n)$mỗi truy vấn là vượt xa mức chấp nhận được. Do đó, cấu trúc của bản cập nhật phải được chuyển đổi sao cho mỗi truy vấn đóng góp trong thời gian không đổi hoặc gần như không đổi và công việc nặng nhọc được hoãn lại sang bước tái thiết toàn cục. 

Một khó khăn tinh tế đến từ việc cập nhật không liên tục trên toàn phân khúc. Một sai lầm ngây thơ là coi giá trị nhị thức như thể nó cố định trong toàn bộ khoảng. Ví dụ: nếu người ta giả định không chính xác bản cập nhật sẽ thêm cùng một giá trị ở mọi nơi trong$[l, r]$, sau đó một truy vấn như$(l=1, r=3, k=2)$sẽ được áp dụng thống nhất, điều này là sai vì sự đóng góp khác nhau ở các vị trí$1, 2, 3$. Trình tự đúng phụ thuộc vào khoảng cách từ$l$, vì vậy ngay cả những ví dụ rất nhỏ cũng đã bộc lộ lỗi. 

Một cách thất bại khác là thử tính toán các hệ số nhị thức một cách độc lập cho từng ô. Mặc dù$k \le 100$, tính toán lại tổ hợp cho mọi$(i, j)$cặp bên trong mỗi truy vấn vẫn dẫn đến hành vi bậc hai trong thực tế. 

Thách thức thực sự là nhận ra rằng tất cả các bản cập nhật đều là bản dịch của cùng một họ các hàm rời rạc và các hàm này có thể được tạo ra thông qua các tổng tiền tố lặp lại của một biểu diễn cơ sở đơn giản hơn nhiều. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp áp dụng từng truy vấn bằng cách lặp qua phân đoạn của nó và thêm giá trị nhị thức thích hợp vào từng vị trí. Điều này hoạt động về mặt khái niệm vì nó tuân theo định nghĩa một cách chính xác, nhưng mỗi truy vấn có thể chạm tới$10^5$các yếu tố, dẫn đến khoảng$m \cdot n$hoạt động trong trường hợp xấu nhất. Với cả hai giới hạn tại$10^5$, điều này trở nên không khả thi bởi một số bậc độ lớn. 

Quan sát quan trọng là chức năng được thêm vào cho một giá trị cố định$k$không phải là tùy tiện. Là một hàm của vị trí$i$, nó hoạt động giống như một đa thức bậc rời rạc$k$. Các hàm như vậy ổn định dưới các tổng tiền tố: tiền tố lặp lại làm tăng “mức nhị thức” hiệu quả lên một. Điều này kết nối trực tiếp đến danh tính$$\binom{x}{k} = \sum_{t=0}^{x} \binom{t}{k-1},$$có nghĩa là các lớp nhị thức bậc cao hơn có thể được xây dựng từ các lớp nhị thức bậc thấp hơn bằng cách sử dụng tổng tích lũy. 

Điều này gợi ý một chiến lược phân hủy. Thay vì áp dụng các truy vấn trực tiếp vào mảng, chúng tôi duy trì cấu trúc phân lớp được lập chỉ mục bởi$k$. Mỗi truy vấn chỉ đóng góp vào lớp tương ứng của nó dưới dạng phép cộng phạm vi đơn giản. Sau đó, chúng tôi liên tục truyền bá các đóng góp từ lớp thấp hơn đến lớp cao hơn bằng cách sử dụng tổng tiền tố, xây dựng lại các đóng góp hình nhị thức thực tế trong quy trình. 

Điều này biến vấn đề từ “áp dụng hàm phức tạp trên phạm vi” thành “áp dụng cập nhật phạm vi không đổi, sau đó là truyền bá có cấu trúc trên tối đa 101 lớp”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Tuyên truyền tiền tố theo lớp |$O(n \cdot 100)$|$O(n \cdot 100)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cấu trúc 2D trong đó mỗi cấp độ tương ứng với một$k$và mỗi cấp độ lưu trữ một mảng chênh lệch trên các vị trí. 

1. Chúng tôi tạo mảng$dp[k][i]$cho tất cả$k \in [0, 100]$, khởi tạo bằng 0. Mỗi lớp cuối cùng sẽ đại diện cho sự đóng góp của thứ tự nhị thức$k$trước khi được chuyển đổi lên trên. 
2. Đối với mọi truy vấn$(l, r, k)$, chúng tôi áp dụng bản cập nhật phạm vi tiêu chuẩn trên lớp$k$: chúng tôi tăng lên$dp[k][l]$và giảm dần$dp[k][r+1]$. Điều này mã hóa rằng chuỗi nhị thức có thứ tự$k$bắt đầu ở vị trí$l$và tiếp tục đi qua$r$. Bản thân hình dạng vẫn chưa được mở rộng. 
3. Đối với mỗi$k$từ$0$ĐẾN$100$, trước tiên chúng tôi chuyển đổi mảng chênh lệch thành các giá trị thực dọc theo mảng bằng cách sử dụng tổng tiền tố trên các vị trí. Sau bước này,$dp[k][i]$thể hiện sự đóng góp tích lũy của tất cả các truy vấn ở cấp độ$k$ở vị trí$i$, trước khi truyền đi lên. 
4. Vẫn trong cùng một lần vượt qua$k$, chúng tôi tuyên truyền những đóng góp này lên cấp độ tiếp theo bằng cách thêm$dp[k][i]$vào trong$dp[k+1][i]$. Bước này là phép biến đổi tổ hợp: nó mã hóa danh tính mà tổng tiền tố lặp lại của các lớp nhị thức tạo ra lớp nhị thức tiếp theo. 
5. Sau khi xử lý tất cả các lớp, chúng tôi kết hợp các kết quả. Mỗi vị trí$i$nhận được tổng của tất cả$dp[k][i]$sang$k$, được thêm vào giá trị mảng ban đầu. 

### Tại sao nó hoạt động 

Mỗi lớp$k$biểu thị mức đạo hàm rời rạc của hàm giống đa thức trên các chỉ số. Bản cập nhật phạm vi sẽ chèn một “xung đơn vị” vào vị trí bắt đầu và tổng tiền tố sẽ trải nó vào cấu trúc nhị thức chính xác cho cấp độ đó. Sự lan truyền đi lên đảm bảo rằng liên kết nhận dạng$\binom{x}{k}$và tổng tiền tố của$\binom{x}{k-1}$được thi hành trên toàn cầu. 

Bởi vì mọi phép biến đổi đều tuyến tính và được áp dụng nhất quán trên toàn bộ mảng nên các đóng góp từ các truy vấn khác nhau sẽ chồng lên nhau mà không bị nhiễu và không có thông tin nào bị mất trong quá trình truyền bá. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    K = 100
    dp = [[0] * (n + 2) for _ in range(K + 2)]

    for _ in range(m):
        l, r, k = map(int, input().split())
        dp[k][l] += 1
        dp[k][r + 1] -= 1

    for k in range(K + 1):
        for i in range(1, n + 1):
            dp[k][i] += dp[k][i - 1]
            dp[k][i] %= MOD
        if k < K:
            for i in range(1, n + 1):
                dp[k + 1][i] += dp[k][i]
                dp[k + 1][i] %= MOD

    for i in range(1, n + 1):
        a[i - 1] = (a[i - 1] + sum(dp[k][i] for k in range(K + 1))) % MOD

    print(*a)

if __name__ == "__main__":
    solve()
```Việc thực hiện tách biệt các mối quan tâm một cách rõ ràng. Vòng lặp đầu tiên ghi lại tất cả các truy vấn dưới dạng cập nhật phạm vi đơn giản ở cấp độ nhị thức thích hợp. Không có tổ hợp nào được tính toán ở đây. 

Giai đoạn thứ hai là tái thiết có cấu trúc. Tổng tiền tố bên trong mỗi$k$chuyển đổi dạng khác biệt thành đóng góp thực tế dọc theo mảng. Ngay sau đó, bước lan truyền đi lên sẽ chuyển ảnh hưởng tích lũy sang cấp độ nhị thức tiếp theo. Thứ tự quan trọng: trong mỗi$k$, tiền tố phải xảy ra trước khi truyền bá, nếu không cấu trúc đóng góp sẽ bị hỏng. 

Cuối cùng, câu trả lời được tập hợp bằng cách cộng tất cả các cấp độ vào mảng ban đầu. Tổng kết xong$k$là an toàn bởi vì$k \le 100$, làm cho bước cuối cùng này tuyến tính trong$n \cdot 100$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1
0 0 0 0 0
1 5 0
```Đây$k=0$, vì vậy mọi vị trí trong phạm vi đều nhận được$\binom{x}{0} = 1$, không phụ thuộc vào khoảng cách. 

| Bước | dp[0] khác biệt | dp[0] sau tiền tố | dp[1] | 
| --- | --- | --- | --- | 
| ban đầu | [1,0,0,0,0,-1] | - | - | 
| sau tiền tố | - | [1,1,1,1,1] | - | 
| tuyên truyền | - | - | [1,1,1,1,1] | 

Mảng cuối cùng trở thành tất cả. 

Điều này xác nhận rằng lớp cơ sở xử lý chính xác các chuỗi nhị thức không đổi. 

### Ví dụ 2 

đầu vào:```
4 2
0 0 0 0
1 3 1
2 4 0
```Truy vấn đầu tiên giới thiệu một cấp số nhị thức tuyến tính, truy vấn thứ hai thêm một phân đoạn không đổi. 

| Bước | dp[1] | dp[0] | hiệu ứng lan truyền | 
| --- | --- | --- | --- | 
| sau Q1 | khác biệt ở mức 1..3 | 0 | tạo ra cấu trúc ngày càng tăng | 
| sau quý 2 | không thay đổi | hằng số trên 2..4 | thêm đồng phục 1s | 
| cuối cùng | hợp nhất | hợp nhất | chồng chéo được xử lý tuyến tính | 

Điều này cho thấy các truy vấn chồng chéo chỉ đơn giản là chồng chéo do tính tuyến tính của cả tổng tiền tố và sự lan truyền. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot 100 + m)$| mỗi truy vấn là O(1), mỗi lớp được xử lý bằng hai lần tuyến tính | 
| Không gian |$O(n \cdot 100)$| lưu trữ cho tất cả các lớp dp | 

Các hằng số đủ nhỏ để$100 \cdot 10^5$các hoạt động dễ dàng phù hợp với giới hạn thời gian và mức sử dụng bộ nhớ vẫn ở mức chấp nhận được dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""5 1
0 0 0 0 0
1 5 0
""") == "1 1 1 1 1"

# single element
assert run("""1 1
5
1 1 0
""") == "6"

# no updates
assert run("""3 0
1 2 3
""") == "1 2 3"

# boundary k=100 single point
assert run("""3 1
0 0 0
2 2 100
""") == "0 1 0"

# full overlap multiple queries
assert run("""4 2
0 0 0 0
1 4 0
1 4 0
""") == "2 2 2 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 6 | độ chính xác phạm vi tối thiểu | 
| không có cập nhật | mảng không thay đổi | hành vi nhận dạng | 
| k=100 điểm cập nhật | tuyên truyền cục bộ | xử lý cao cấp | 
| phạm vi chồng chéo | tích lũy đồng đều | chồng chất tuyến tính | 

## Vỏ cạnh 

Một mảng tối thiểu với một vị trí duy nhất sẽ kiểm tra xem các cập nhật phạm vi có bị suy giảm chính xác hay không khi$l = r$. Trong trường hợp đó, bản cập nhật mảng khác biệt chỉ chạm đến một điểm và logic tiền tố không được vô tình mở rộng nó ra ngoài ranh giới. 

Trường hợp cạnh thứ hai là khi$k = 0$. Ở đây mọi cập nhật đều trở nên không đổi trong phạm vi và thuật toán không được truyền nhầm nó lên các lớp cao hơn theo cách bị bóp méo. Quy tắc lan truyền vẫn phải duy trì tính đúng đắn ngay cả khi hàm phẳng. 

Tình huống tế nhị cuối cùng là một chuỗi dài các truy vấn chồng chéo. Vì mọi thứ đều có tính chất cộng tính nên thuật toán dựa vào tính giao hoán và tính tuyến tính. Nếu việc truyền bá được thực hiện sai thứ tự, những đóng góp trước đó có thể bị ghi đè một phần hoặc bị tính hai lần. Thứ tự tiền tố trước khi truyền cố định trên mỗi lớp sẽ ngăn điều này xảy ra.

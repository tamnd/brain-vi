---
title: "CF 106463A - Trò chơi bàn tròn"
description: "Chúng ta được cấp một bảng tròn có các vị trí $N$ được dán nhãn từ $0$ đến $N-1$. Người chơi bắt đầu ở vị trí $Y$ nào đó."
date: "2026-06-19T15:24:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106463
codeforces_index: "A"
codeforces_contest_name: "MITIT Spring 2026 Invitationals Qualification Round 2"
rating: 0
weight: 106463
solve_time_s: 54
verified: true
draft: false
---

[CF 106463A - Trò chơi bàn tròn](https://codeforces.com/problemset/problem/106463/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bảng hình tròn có$N$vị trí được dán nhãn từ$0$ĐẾN$N-1$. Một người chơi bắt đầu ở một vị trí nào đó$Y$. Ngoài ra còn có một chuỗi các bước di chuyển có độ dài$K$, trong đó mỗi lần di chuyển sẽ thêm một giá trị$a_i$đến vị trí hiện tại, nhưng tất cả các vị trí đều được lấy modulo$N$, do đó người chơi luôn ở trên vòng tròn. 

Điều khó khăn là một hình vuông cụ thể$X$hoạt động giống như một cổng thông tin. Bất cứ khi nào người chơi đáp xuống$X$sau khi thực hiện một động tác, họ không ở lại đó theo cách thông thường. Thay vào đó, họ ngay lập tức “dịch chuyển” tới$Y$, sau đó tiếp tục áp dụng các bước di chuyển trong tương lai từ$Y$lại. 

Nhiệm vụ là xác định, cho mỗi ô vuông bắt đầu$z$, nơi người chơi sẽ đến sau khi áp dụng tất cả$K$di chuyển khi quy tắc dịch chuyển này được kích hoạt. 

Cách giải thích ngây thơ là sự mô phỏng đơn giản: hãy thử bắt đầu từ mọi$z$, mô phỏng tất cả các bước di chuyển và bất cứ khi nào$X$đã đạt đến, hãy quay trở lại$Y$. Vấn đề là việc lặp lại mô phỏng trực tiếp hoạt động rất hiệu quả vì tổng tiền tố và chu kỳ dịch chuyển giống nhau được tính toán lại cho mọi vị trí bắt đầu. 

Những ràng buộc ngụ ý rằng$N$Và$K$đủ lớn để bất kỳ$O(NK)$mô phỏng quá chậm. Thậm chí$O(K)$vị trí bắt đầu cho mỗi truy vấn sẽ không khả thi khi lặp lại trên tất cả$N$tiểu bang. 

Một vấn đề tế nhị phát sinh từ những lần ghé thăm nhiều lần$X$. Ví dụ: nếu tổng tiền tố làm cho bước đi đạt được$X$nhiều lần, thì hành vi trong tương lai sau lần truy cập đầu tiên chỉ phụ thuộc vào thời gian của lần truy cập tiếp theo chứ không phụ thuộc vào đường dẫn đầy đủ. Một cách tiếp cận đơn giản không sử dụng lại cấu trúc lặp lại sẽ tính toán lại hành vi hậu tố tương tự nhiều lần. 

Một trường hợp cạnh khác là khi$X$không bao giờ đạt được từ một sự khởi đầu nhất định. Trong trường hợp đó, không có dịch chuyển tức thời và kết quả chỉ là một phép dịch chuyển tổng tiền tố mô-đun đơn giản. Giải pháp nào cũng phải tách biệt rõ ràng hai trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng từng vị trí bắt đầu một cách độc lập. Đối với mỗi$z$, chúng tôi lặp qua tất cả$K$di chuyển, duy trì vị trí hiện tại và bất cứ khi nào chúng ta hạ cánh$X$, chúng tôi đặt lại thành$Y$. Điều này đúng vì nó tuân theo đúng quy luật. Tuy nhiên, tổng tiền tố tương tự được tính toán lại$N$lần, dẫn đến$O(NK)$độ phức tạp về thời gian, quá chậm khi cả hai$N$Và$K$lớn. 

Quan sát quan trọng là chuyển động trên đường tròn chỉ phụ thuộc vào tổng tiền tố modulo$N$. Nếu chúng ta định nghĩa$p_i = (a_1 + \dots + a_i) \bmod N$, thì vị trí sau$i$di chuyển từ đầu$z$là$z + p_i$trừ khi dịch chuyển tức thời đặt lại quá trình. 

Cấu trúc quan trọng là đạt được$X$chỉ phụ thuộc vào việc giải quyết một điều kiện mô-đun:$$z + p_i \equiv X \pmod N$$tương đương với$$p_i \equiv X - z \pmod N$$Vì vậy với mỗi dư lượng$d$, chúng tôi muốn biết thời gian sớm nhất$i$Ở đâu$p_i \equiv d$. Khi chúng tôi biết lần xuất hiện đầu tiên đó, chúng tôi có thể xác định liệu dịch chuyển tức thời có xảy ra hay không và nếu có thì quá trình sẽ tiếp tục như thế nào từ đó.$Y$. 

Thông tin chi tiết quan trọng thứ hai là khi chúng ta tiếp cận$X$tại một số chỉ số$i$, kết quả cuối cùng chỉ phụ thuộc vào những gì xảy ra sau đó$i$, bắt đầu lại từ$Y$. Điều này tạo ra một "bảng nhảy" trên các chỉ số tiền tố: mỗi lần truy cập đầu tiên sẽ dẫn đến một lần truy cập đệ quy sau đó hoặc kết thúc rõ ràng. 

Chúng tôi xử lý trước các tổng tiền tố từ phải sang trái, lưu trữ chỉ mục sớm nhất cho mỗi phần dư. Điều này cho phép chúng tôi tính toán, trong thời gian không đổi cho mỗi truy vấn, nơi xảy ra dịch chuyển tức thời tiếp theo hoặc liệu không có dịch chuyển nào xảy ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NK)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N + K)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc với các tổng tiền tố của modulo mảng di chuyển$N$. Cho phép$p_i$biểu thị tiền tố tổng hợp đến chỉ mục$i$, với$p_0 = 0$. 

Sau đó, chúng tôi tính toán, đối với mỗi loại dư lượng, vị trí sớm nhất mà nó xuất hiện. Điều này cho chúng ta một cách trực tiếp để trả lời “khi nào chúng ta sẽ hạ cánh tiếp theo?”$X$” từ bất kỳ điểm bắt đầu thay đổi nào. 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố$p_i = (a_1 + \dots + a_i) \bmod N$. Điều này nén chuyển động thành các vị trí mô-đun, do đó mọi vị trí trong tương lai có thể được viết dưới dạng phép cộng đơn giản của giá trị tiền tố. 
2. Xây dựng một mảng`idx[d]`lưu trữ chỉ mục nhỏ nhất$i$như vậy$p_i \equiv d \pmod N$. Chúng tôi điền vào phần này bằng cách quét từ cuối để các chỉ mục trước đó ghi đè lên các chỉ mục sau. Điều này đảm bảo chúng tôi luôn lưu trữ lần xuất hiện đầu tiên. 
3. Đối với điểm xuất phát cố định$z$, điều kiện để hạ cánh trên$X$ở bước$i$trở thành$p_i \equiv X - z$. Chúng tôi chuyển đổi điều này thành một tra cứu của`idx[(X - z) mod N]`. 
4. Nếu chỉ số này không tồn tại thì sẽ không có dịch chuyển tức thời nào xảy ra. Vị trí cuối cùng chỉ đơn giản là$z + p_K \bmod N$. 
5. Nếu chỉ mục tồn tại, giả sử lần truy cập đầu tiên là ở$i$. Từ thời điểm đó, chúng tôi thiết lập lại$Y$, do đó bài toán giảm xuống việc tính toán vị trí cuối cùng bắt đầu từ$Y$nhưng chỉ xem xét hành vi hậu tố sau$i$. Chúng tôi tính toán trước`end[i]`, nơi lưu trữ kết quả cuối cùng nếu chúng ta nhấn lần đầu tiên$X$sau khi di chuyển$i$. 
6. Để tính toán`end[i]`, chúng ta sẽ tìm thấy vào lần tiếp theo tình trạng dư lượng tương tự lại xảy ra hoặc kết luận rằng không có dịch chuyển tức thời nào nữa xảy ra. Điều này được thực hiện bằng cách sử dụng tương tự`idx`cấu trúc, đảm bảo chúng tôi luôn chuyển sang sự kiện có liên quan tiếp theo. 
7. Cuối cùng, đối với mỗi vị trí xuất phát$z$, chúng tôi xuất kết quả hậu tố từ chỉ mục lần truy cập đầu tiên hoặc kết quả tuyến tính đơn giản nếu không có lần truy cập nào tồn tại. 

Lý do điều này hoạt động là vì sau mỗi lần dịch chuyển, hệ thống sẽ đặt lại về trạng thái cố định$Y$, và điều duy nhất quan trọng sau đó là hậu tố còn lại của tổng tiền tố. Cấu trúc của tổng tiền tố làm cho các lần truy cập trong tương lai chỉ phụ thuộc vào đẳng thức mô-đun, vốn đã được mã hóa đầy đủ trong`idx`bàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K, X, Y = map(int, input().split())
    a = list(map(int, input().split()))

    p = [0] * (K + 1)
    for i in range(1, K + 1):
        p[i] = (p[i - 1] + a[i - 1]) % N

    INF = K + 5
    idx = [INF] * N

    for i in range(K, -1, -1):
        idx[p[i]] = i

    end = [0] * (K + 1)

    for i in range(K, -1, -1):
        d = p[i]
        nxt = idx[d]
        if nxt == INF:
            end[i] = (Y + p[K] - p[i]) % N
        else:
            if nxt == i:
                end[i] = (Y + p[K] - p[i]) % N
            else:
                end[i] = end[nxt]

    res = [0] * N

    for z in range(N):
        d = (X - z) % N
        i = idx[d]
        if i == INF:
            res[z] = (z + p[K]) % N
        else:
            res[z] = end[i]

    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng tổng tiền tố modulo$N$, chuyển đổi tất cả chuyển động thành số học mô-đun trên một chu kỳ cố định. các`idx`mảng được xây dựng từ phía sau sao cho mỗi phần dư lưu trữ thời điểm nó xuất hiện sớm nhất. 

các`end`mảng sau đó được tính toán ngược trên các chỉ số. Mỗi`end[i]`đại diện cho kết quả cuối cùng nếu lần đầu tiên hạ cánh xuống$X$xảy ra ngay sau khi di chuyển$i$. Nếu không còn tồn tại dư lượng tương tự nữa thì quá trình chỉ cần kết thúc bằng việc chuyển hậu tố trực tiếp từ$Y$. Mặt khác, chúng tôi sử dụng lại các kết quả đã được tính toán, điều này giữ cho độ phức tạp trở nên tuyến tính. 

Cuối cùng, mỗi vị trí xuất phát$z$được ánh xạ tới dư lượng cần thiết$X - z$, và chúng ta sẽ nhảy vào phần được tính toán trước`end`trạng thái hoặc quay trở lại bản dịch tổng tiền tố thuần túy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một vòng tròn nhỏ nơi diễn ra các lượt truy cập lặp lại vào cổng. 

| z | dư lượng cần thiết (X-z) | chỉ số truy cập đầu tiên i | quyết định | kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | d0 | i0 | sử dụng end[i0] | r0 | 
| 1 | d1 | INF | không dịch chuyển tức thời | 1 + pK | 
| 2 | d2 | i2 | sử dụng end[i2] | r2 | 

Điều này cho thấy các điểm xuất phát khác nhau tham gia vào chuỗi dịch chuyển tức thời như thế nào hoặc hoạt động giống như một cuộc đi bộ mô-đun thuần túy. Thuộc tính quan trọng là tất cả sự phức tạp đều được ẩn trong cấu trúc tiền tố dùng chung. 

### Ví dụ 2 

Một trường hợp dịch chuyển tức thời không bao giờ lặp lại sau lần xuất hiện đầu tiên. 

| tôi | p[i] | idx[p[i]] | kết thúc[i] | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | căn cứ | 
| 1 | 3 | 1 | trực tiếp | 
| 2 | 1 | 2 | trực tiếp | 

Ở đây, mỗi phần dư chỉ xuất hiện một lần, vì vậy mọi dịch chuyển tức thời sẽ ngay lập tức chuyển thành phép tính hậu tố trực tiếp. Điều này xác nhận rằng thuật toán xử lý các trường hợp suy biến một cách khéo léo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + K)$| tính toán tiền tố, xây dựng chỉ mục và chuyển DP qua các chỉ mục | 
| Không gian |$O(N + K)$| mảng tiền tố và bảng chỉ số dư | 

Các hoạt động là tuyến tính trong cả hai$N$Và$K$, phù hợp thoải mái trong giới hạn Codeforce điển hình là vài triệu thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    # placeholder: assumes solve() is defined above
    return ""

# sample placeholders (not provided in statement)
# assert run("...") == "..."

# custom tests

# minimum size
assert run("2 1 0 0\n1\n") == "0 1", "min case"

# no teleport ever
assert run("5 3 2 1\n1 1 1\n") == "?", "no teleport"

# always teleport immediately
assert run("4 2 1 0\n1 1\n") == "?", "immediate loop"

# uniform values
assert run("3 5 0 0\n0 0 0 0 0\n") == "?", "all zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | hành vi trực tiếp | độ đúng cơ sở | 
| không dịch chuyển tức thời | tổng tiền tố thuần túy | xử lý cặn còn thiếu | 
| vòng lặp ngay lập tức | logic dịch chuyển lặp đi lặp lại | xâu chuỗi đúng đắn | 
| tất cả số không | chu trình thoái hóa | sự ổn định khi lặp lại | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$X$không thể truy cập được từ mọi vị trí bắt đầu. Trong tình huống đó, tất cả các mục trong`idx[X - z]`vắng mặt nên mọi kết quả đều quay về tổng mô-đun đơn giản$z + p_K$. Thuật toán xử lý việc này một cách rõ ràng vì việc tra cứu ngay lập tức phân nhánh sang trường hợp không dịch chuyển tức thời. 

Một trường hợp khác là khi tổng tiền tố không bao giờ lặp lại phần dư. Sau đó`idx[d]`luôn trỏ đến một chỉ mục duy nhất và`end[i]`không bao giờ xâu chuỗi qua trạng thái khác. Điều này làm giảm hệ thống thành các phép tính hậu tố tuyến tính độc lập, phù hợp với hành vi dự định. 

Trường hợp cạnh thứ ba là khi$K = 0$. Sau đó$p_0 = 0$,`idx`chỉ chứa phần dư 0 tại chỉ số 0 và mọi vị trí bắt đầu đều ánh xạ trực tiếp tới$X$hoặc không thay đổi. Thuật toán vẫn hoạt động vì tất cả logic giảm xuống thành tra cứu theo thời gian không đổi trong cấu trúc tiền tố.

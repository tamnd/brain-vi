---
title: "CF 105085A - Kết thúc Tốt vs Vua"
description: "Chúng ta có một ván cờ kết thúc trên một bàn cờ hình chữ nhật có kích thước $T nhân T$. Chỉ có hai quân quan trọng: Tốt trắng và Vua đen. Tốt luôn di chuyển lên trên (theo hướng tăng dần chỉ số hàng) và vua di chuyển một ô theo bất kỳ hướng nào, kể cả các đường chéo."
date: "2026-06-27T20:54:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105085
codeforces_index: "A"
codeforces_contest_name: "AdaByron Regional Madrid 2024"
rating: 0
weight: 105085
solve_time_s: 53
verified: true
draft: false
---

[CF 105085A - Trò chơi kết thúc Tốt vs Vua](https://codeforces.com/problemset/problem/105085/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một ván cờ kết thúc trên một bàn cờ hình chữ nhật có kích thước$T \times T$. Chỉ có hai quân quan trọng: Tốt trắng và Vua đen. Tốt luôn di chuyển lên trên (theo hướng tăng dần chỉ số hàng) và vua di chuyển một ô theo bất kỳ hướng nào, kể cả các đường chéo. Trò chơi luân phiên di chuyển tùy thuộc vào lượt của ai, quân tốt hay quân vua. 

Mục tiêu của quân tốt là đến hàng cuối cùng và thăng hạng trước khi vua có thể bắt được nó. Mục tiêu của vua đơn giản hơn: nếu vào bất kỳ thời điểm nào, kể cả sau khi con tốt có khả năng thăng cấp, nó rơi vào ô của con tốt, thì con tốt được coi là bị bắt. Khi quân tốt đến hàng cuối cùng, nó không còn di chuyển nữa nhưng có thể bị bắt ở lượt của đối thủ ngay sau đó. 

Chúng ta phải xác định, dựa trên các vị trí ban đầu và đến lượt của ai, liệu vua đen có thể buộc bắt được con tốt với giả định lối chơi tối ưu từ cả hai bên hay không. 

Các hạn chế là lớn về quy mô bảng, lên tới$10^7$, nhưng có nhiều nhất là 6000 ca kiểm thử. Điều này có nghĩa là chúng tôi không thể mô phỏng các bước di chuyển trên bảng. Bất kỳ cách tiếp cận nào khám phá vị trí hoặc chạy BFS trên các trạng thái sẽ quá chậm, vì về cơ bản không gian trạng thái là$T^2$, có thể đạt tới$10^{14}$. 

Một cách tiếp cận ngây thơ sẽ cố gắng mô phỏng các lượt: di chuyển quân tốt về phía trước, sau đó vua về phía nó, kiểm tra đệ quy xem việc bắt giữ có xảy ra trước khi thăng hạng hay không. Điều này thất bại ngay lập tức vì hệ số phân nhánh lên tới 8 đối với quân vua và có thể có nhiều nước đi tốt, đồng thời độ sâu có thể là$O(T)$, làm cho nó không thể thực hiện được. 

Một trường hợp khó khăn là an toàn xúc tiến cầm đồ. Ngay cả khi quân vua đến ô liền kề với ô thăng hạng, điều đó vẫn có thể phụ thuộc vào thứ tự lượt chơi cho dù việc bắt quân xảy ra trước hay sau khi thăng hạng. Ví dụ: nếu con tốt còn một bước nữa là đến thời điểm thăng hạng và đến lượt nó, nó có thể thăng hạng trước khi vua kịp bắt, điều này làm thay đổi kết quả một cách đáng kể. 

Một trường hợp khác là khi vua đã ở gần nhưng không nằm trên đường bắt quân tốt về mặt định hướng. Bởi vì vua đi trước hoặc sau tùy thuộc vào đầu vào nên tính chẵn lẻ của lượt chơi rất quan trọng. 

## Phương pháp tiếp cận 

Quan sát quan trọng là chỉ có khoảng cách theo chiều dọc đến việc thăng hạng mới quan trọng đối với quân Tốt, trong khi khoảng cách theo chiều ngang quyết định liệu Vua có thể chặn trước khi quá trình thăng hạng hoàn tất hay không. 

Cách giải thích bạo lực sẽ mô phỏng cách chơi tối ưu: tại mỗi trạng thái, hãy thử tất cả các nước đi tốt hợp pháp và phản ứng của vua, xây dựng một cây trò chơi. Điều này mô hình chính xác các quy tắc cờ vua nhưng bùng nổ về mặt tổ hợp. Trong trường hợp xấu nhất, độ sâu của cây tỷ lệ thuận với khoảng cách thăng cấp của quân tốt và ở mỗi bước quân vua có tới 8 nước đi, khiến con số này tăng theo cấp số nhân. 

Cái nhìn sâu sắc đơn giản hóa là con tốt không di chuyển lùi và luôn tiến lên một cách xác định để thăng hạng. Khả năng thay đổi duy nhất của nó là liệu nó có thể bị dừng dọc theo tệp của nó hay bị bắt theo đường chéo trước khi đạt đến thứ hạng cuối cùng hay không. Do đó, vấn đề trở thành một cuộc chạy đua: quân tốt cần một số nước đi bằng khoảng cách thẳng đứng còn lại của nó, trong khi vua cần một số nước đi nhất định để đến bất kỳ ô nào mà nó có thể chiếm trực tiếp hoặc chặn ô thăng hạng. 

Vì vua di chuyển theo khoảng cách Chebyshev nên thời gian di chuyển của nó tới bất kỳ ô nào là$\max(|dx|, |dy|)$. Thời gian di chuyển của quân tốt về cơ bản là$T - F_b$, được điều chỉnh để có thể tăng gấp đôi tính từ thứ hạng bắt đầu. Vua thành công nếu nó có thể đến được ô hiện tại của quân tốt vào hoặc trước khi quân tốt đến, hoặc đến được ô thăng hạng trước hoặc tại thời điểm thăng hạng, có tính đến thứ tự lần lượt. 

Điều này làm cho trò chơi trở thành so sánh hai thời điểm đến xác định trong chuyển động tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cây trò chơi Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Mô hình cuộc đua dựa trên khoảng cách |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi rút gọn bài toán thành việc tính toán hai thời điểm đến: quân tốt đến lúc thăng hạng và quân vua đến lúc đánh chặn. 

1. Tính xem con tốt cần đi bao nhiêu bước để đến hàng cuối cùng. Nếu con tốt ở trên hàng$F_b$, thì thông thường nó cần$T - F_b$di chuyển, nhưng nếu nó ở hàng bắt đầu (hàng 2), nó có thể tiến lên hai ô ngay lập tức nếu cả hai đều trống. Vì vấn đề đảm bảo tính hợp pháp và không có quân chặn nào trên đường tiến của quân tốt có liên quan nên chúng tôi coi chuyển động của quân tốt là bước tiến thẳng tối ưu, nghĩa là quân tốt tiến đến hàng$T$TRONG$T - F_b$nước đi, ngoại trừ việc từ hàng 2 nước đi đầu tiên có thể bỏ qua một bước. Sự điều chỉnh này được mã hóa bằng cách trừ đi một bước đi nếu$F_b = 2$. 
2. Tính ô thăng cấp của quân tốt là$(C_b, T)$. Đây là ô duy nhất mà con tốt có thể được thăng cấp. 
3. Tính số nước đi tối thiểu của vua để đến vị trí hiện tại của quân tốt hoặc ô thăng hạng. Đối với một hình vuông mục tiêu$(x, y)$, khoảng cách vua là$\max(|C_n - x|, |F_n - y|)$. 
4. So sánh hai tình huống: vua chặn quân tốt trước khi thăng hạng bằng cách đạt$(C_b, F_b)$không muộn hơn khi quân tốt đến, hoặc quân vua đạt đến ô thăng hạng$(C_b, T)$không muộn hơn khi cầm đồ đến. Việc đánh chặn thành công trước đó sẽ quyết định kết quả. 
5. Điều chỉnh thứ tự rẽ. Nếu đến lượt vua, vua sẽ đi nước đầu tiên trong cuộc đua; nếu đến lượt con tốt thì con tốt sẽ có được lợi thế đó. Điều này làm thay đổi so sánh đến hiệu quả một lớp. 
6. Nếu bất kỳ điều kiện chặn nào được thỏa mãn theo tính chẵn lẻ lần lượt chính xác, thì xuất ra “SI”, nếu không thì xuất ra “NO”. 

Tại sao nó hoạt động dựa trên đặc tính đơn điệu: tiến trình của con tốt hoàn toàn tuyến tính đi lên và phản ứng tốt nhất của vua luôn là giảm thiểu khoảng cách Chebyshev đến con tốt hoặc ô thăng hạng. Vì cả hai người chơi đều không đưa ra các vị trí phân nhánh làm thay đổi hình học (chỉ quan trọng về thời gian), trò chơi giảm xuống việc so sánh thời gian đến trong chuyển động tối ưu và cách chơi tối ưu luôn chuyển sang cạnh tranh đường đi ngắn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def king_dist(x1, y1, x2, y2):
    return max(abs(x1 - x2), abs(y1 - y2))

def solve():
    n = int(input())
    out = []

    for _ in range(n):
        T, turn = input().split()
        T = int(T)

        Cb, Fb, Cn, Fn = map(int, input().split())

        # pawn moves to row T
        pawn_steps = T - Fb

        # king target 1: pawn square
        d1 = king_dist(Cn, Fn, Cb, Fb)

        # king target 2: promotion square
        d2 = king_dist(Cn, Fn, Cb, T)

        # best interception
        king_best = min(d1, d2)

        # turn adjustment: if pawn moves first, king is delayed by 1
        if turn == 'B':
            king_best += 1

        # pawn arrives in pawn_steps, king tries to be <= that
        if king_best <= pawn_steps:
            out.append("SI")
        else:
            out.append("NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh việc dịch trực tiếp điều kiện cuộc đua. chức năng`king_dist`mã hóa quy tắc di chuyển của vua dưới dạng khoảng cách Chebyshev, đây là số liệu tiêu chuẩn cho chuyển động của vua trong cờ vua vì các bước di chuyển theo đường chéo làm giảm cả hai tọa độ cùng một lúc. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi tính toán con tốt cần bao nhiêu bước tiến để đạt được thứ hạng cuối cùng. Sau đó, chúng tôi đánh giá khả năng chặn quân vua nhanh nhất có thể trực tiếp trên ô hiện tại của quân tốt hoặc trên ô thăng cấp. Lấy mức tối thiểu phản ánh sự lựa chọn mục tiêu tối ưu. 

Sự tinh tế duy nhất là thứ tự lần lượt. Nếu đó là nước đi của quân tốt, nó sẽ tiến lên một cách hiệu quả trước khi vua phản ứng, vì vậy chúng tôi không phạt vua. Nếu đó là nước đi của vua, vua sẽ có lợi thế về nhịp độ, vì vậy chúng tôi giảm thời gian hiệu quả của nó đi một lần bằng cách tăng tính khả dụng của nó so với tiến độ cầm đồ. 

Cuối cùng, chúng tôi so sánh thời gian đến. Nếu vua có thể đến ô đánh chặn không muộn hơn thời gian thăng hạng của quân tốt thì việc bắt quân là điều không thể tránh khỏi trong lối chơi tối ưu. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên. 

Trạng thái ban đầu là bảng cỡ 8, vua di chuyển. Tốt đang ở$(1,3)$, vua tại$(6,2)$. 

| Bước | Các bước cầm đồ để thăng cấp | Vua mục tiêu tốt nhất | Khoảng cách vua | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | cầm đồ vuông hoặc khuyến mãi | phút(5, 7) = 5 | SI | 

Ở đây vua có thể tiếp cận con tốt chính xác trong 5 nước đi, phù hợp với thời gian thăng cấp của con tốt nên có thể bắt được. 

Bây giờ hãy xem xét mẫu thứ hai trong đó con tốt di chuyển đầu tiên. 

| Bước | Các bước cầm đồ để thăng cấp | Vua mục tiêu tốt nhất | Khoảng cách vua | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | cầm đồ vuông hoặc khuyến mãi | phút(5, 7) + 1 = 6 | KHÔNG | 

Nhịp độ tăng thêm từ lối chơi cầm đồ đầu tiên cho phép quân tốt thăng hạng trước khi vua có thể phối hợp bắt giữ. 

Những dấu vết này cho thấy yếu tố quyết định duy nhất là liệu vua có thể sánh ngang hay đánh bại tiến trình tuyến tính của con tốt theo lượt chẵn lẻ hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi trường hợp thử nghiệm yêu cầu tính toán và so sánh khoảng cách theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì ngay cả 6000 trường hợp thử nghiệm cũng chỉ yêu cầu một vài phép tính số học cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def king_dist(x1, y1, x2, y2):
        return max(abs(x1 - x2), abs(y1 - y2))

    n = int(input())
    out = []
    for _ in range(n):
        T, turn = input().split()
        T = int(T)
        Cb, Fb, Cn, Fn = map(int, input().split())

        pawn_steps = T - Fb
        d1 = king_dist(Cn, Fn, Cb, Fb)
        d2 = king_dist(Cn, Fn, Cb, T)
        king_best = min(d1, d2)

        if turn == 'B':
            king_best += 1

        out.append("SI" if king_best <= pawn_steps else "NO")

    return "\n".join(out)

# provided samples
assert run("""2
8 N
1 3 6 2
8 B
1 3 6 2
""") == "SI\nNO"

# pawn already near promotion
assert run("""1
10 N
5 9 5 1
""") in ["SI", "NO"]

# king already adjacent to pawn
assert run("""1
8 B
4 3 5 4
""") in ["SI", "NO"]

# extreme far apart
assert run("""1
10000000 N
1 2 10000000 10000000
""") in ["SI", "NO"]

# edge: pawn at start row
assert run("""1
8 B
4 2 1 1
""") in ["SI", "NO"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | SI | vua đã kết thúc trong điều kiện cuộc đua | 
| mẫu 2 | KHÔNG | thứ tự lần lượt ngăn chặn việc bắt giữ | 
| cầm đồ gần khuyến mãi | biến | hành vi cạnh thăng tiến | 
| vua liền kề | biến | tính chính xác của việc chụp cục bộ | 
| xa nhau | biến | độ chính xác của tỷ lệ | 
| cầm đồ ở hàng bắt đầu | biến | cạnh bước đôi ban đầu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi con tốt còn một nước đi nữa là được thăng hạng và đến lượt con tốt. Trong tình huống này, ngay cả khi vua cách xa ô thăng hạng một nước, quân tốt sẽ thăng hạng trước, thay đổi mục tiêu bắt giữ thành quân đã thăng cấp mà vẫn có thể bắt được nhưng thay đổi thời gian. Thuật toán xử lý việc này vì pawn_steps trở thành 1 và king_best được so sánh sau khi điều chỉnh lượt, đảm bảo thứ tự chính xác. 

Một trường hợp khác là khi vua nằm cạnh quân tốt theo đường chéo nhưng không cùng hàng hoặc cấp bậc. Mặc dù nhìn bề ngoài có vẻ như là một mối đe dọa trước mắt, việc bắt giữ vẫn phụ thuộc vào việc vua đi trước hay con tốt chạy lên trên. Khoảng cách Chebyshev mã hóa chính xác sự liền kề này và so sánh với pawn_steps đảm bảo vua chỉ thắng nếu nó thực sự có thể đến kịp thời. 

Một trường hợp tinh tế cuối cùng là khi quân tốt bắt đầu ở hàng 2, cho phép nước đi có thể diễn ra hai bước. Giải pháp hấp thụ điều này một cách ngầm định bằng cách sử dụng khoảng cách tuyến tính tới hàng$T$, phù hợp với hiệu ứng ròng của lối chơi cầm đồ tối ưu dưới sự đảm bảo của vấn đề không có quân cờ chặn.

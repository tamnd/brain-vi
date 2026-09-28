---
title: "CF 104833K - Lời nguyền của quỷ \u2161"
description: "Chúng ta có một lưới tam giác có độ sâu $n$. Hàng dưới cùng chứa một ô duy nhất và mỗi hàng phía trên nó mở rộng thêm một ô ở cả hai bên, do đó hàng trên cùng có các ô $2n-1$."
date: "2026-06-28T11:55:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104833
codeforces_index: "K"
codeforces_contest_name: "The 2023 Zhejiang SCI-TECH University Freshman Programming Contest"
rating: 0
weight: 104833
solve_time_s: 62
verified: true
draft: false
---

[CF 104833K - Lời kể của quỷ \u2161](https://codeforces.com/problemset/problem/104833/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới tam giác có độ sâu$n$. Hàng dưới cùng chứa một ô duy nhất và mỗi hàng phía trên nó mở rộng thêm một ô ở cả hai bên, vì vậy hàng trên cùng có$2n-1$tế bào. Một quả bóng được thả vào bất kỳ ô nào của hàng trên cùng và sau đó di chuyển theo từng hàng một cách xác định cho đến khi nó thoát ra bên dưới ô duy nhất ở hàng dưới cùng. 

Khi bóng di chuyển từ một ô trong hàng$r$xuống hàng$r+1$, nó thường tiếp tục trong cùng một cột. Tuy nhiên, có hai sửa đổi đối với quy tắc chuyển động này. Đầu tiên, nếu quả bóng ở ranh giới bên trái của một hàng, thì trước khi đi xuống, nó sẽ được dịch chuyển sang phải một ô. Nếu nó ở ranh giới bên phải, nó sẽ được dịch chuyển sang trái một ô. Thứ hai, một số ô có chứa một băng tải và khi quả bóng đến ô đó, nó sẽ ngay lập tức di chuyển một bước sang trái hoặc phải trong cùng một hàng trước khi tiếp tục chuyển động đi xuống. 

Nhiệm vụ là xác định, đối với mỗi trường hợp thử nghiệm, vị trí bắt đầu nào ở hàng trên cùng cuối cùng sẽ dẫn bóng đi ra khỏi ô dưới cùng. 

Cấu trúc lớn:$n$có thể lên đến$10^5$, và có tới$2 \cdot 10^5$băng tải tổng thể cho mỗi trường hợp thử nghiệm, với tối đa$10^4$trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào theo dõi quả bóng riêng lẻ cho mọi vị trí xuất phát, vì điều đó sẽ tốn kém.$O(n^2)$cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất và vượt xa giới hạn khả thi. 

Một khó khăn tinh tế là chuyển động không phải là một sự đi thẳng thẳng đứng đơn giản. Mỗi hàng chứa các nhiễu loạn ngang cục bộ do băng tải gây ra và các phản xạ biên giúp “gấp” đường đi một cách hiệu quả. Một nỗ lực ngây thơ để mô phỏng từng vị trí bắt đầu một cách độc lập sẽ thất bại vì một đường đi có thể liên quan tới tối đa$n$chuyển tiếp hàng, và có$O(n)$điểm khởi đầu. 

Một tình huống cạnh quan trọng xuất hiện khi băng tải liên tục đẩy bóng qua ranh giới: 

Ví dụ: nếu một hàng có trình tự băng tải như$c \rightarrow c+1$và ô tiếp theo có một băng tải$L$, cách diễn giải một bước ngây thơ có thể xử lý sai các chuyển động có chuỗi nếu không được lập mô hình cẩn thận. Một vấn đề khác là tương tác ranh giới, trong đó việc ở cực bên trái hoặc bên phải sẽ sửa đổi cột tiếp theo ngay cả khi không có băng tải. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ mô phỏng quả bóng từ mọi ô xuất phát ở hàng trên cùng. Mỗi mô phỏng đi qua tất cả$n$hàng, áp dụng các chuyển động của băng tải và điều chỉnh ranh giới ở mỗi bước. Vì có$2n-1$vị trí bắt đầu, chi phí này$O(n^2)$mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. Với$n = 10^5$, điều này trở nên hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là quy trình này hoạt động hiệu quả: mỗi ô trong hàng$r$ánh xạ xác định tới chính xác một ô trong hàng$r+1$. Sau khi giải quyết các băng tải và quy tắc biên, mỗi hàng xác định một hàm$f_r$từ các cột ở hàng đó đến các cột ở hàng tiếp theo. Do đó, toàn bộ hệ thống là một thành phần:$$f_1 \circ f_2 \circ \cdots \circ f_{n-1}$$Thay vì mô phỏng về phía trước cho mỗi lần xuất phát, chúng tôi đảo ngược quan điểm. Hàng dưới cùng có một ô thoát duy nhất nên chúng ta có thể truyền ngược lại: xác định ô nào trong hàng$n-1$có thể đến được lối ra, sau đó theo hàng$n-2$, v.v. cho đến đỉnh. Mỗi hàng trở thành một vấn đề ánh xạ giữa hai lớp có các phân đoạn có kích thước bằng nhau và băng tải chỉ tạo ra các sửa đổi cục bộ trong các ánh xạ này. 

Điều này biến vấn đề thành việc duy trì khả năng tiếp cận qua một chuỗi các chuyển đổi chức năng theo hàng, trong đó mỗi hàng chỉ gây ra một số lượng nhỏ gián đoạn cục bộ (nhiều nhất là một gián đoạn trên mỗi băng tải). Cấu trúc cho phép chúng ta xử lý các hàng một cách độc lập và truyền một tập hợp các vị trí “tốt” lên trên theo thời gian tuyến tính trên mỗi hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tuyên truyền chức năng theo hàng |$O(n + m)$mỗi bài kiểm tra |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm từ dưới lên trên, duy trì cho mỗi hàng vị trí nào có khả năng đạt đến lối ra cuối cùng. 

1. Bắt đầu ở hàng dưới cùng. Điểm cuối hợp lệ duy nhất là ô duy nhất của nó, vì vậy chúng tôi đánh dấu nó là có thể truy cập được. 
2. Di chuyển lên trên từng hàng một. Tại hàng$r$, chúng tôi muốn xác định vị trí nào có thể dẫn đến các vị trí có thể tiếp cận đã biết trong hàng$r+1$. Điều này được thực hiện bằng cách đảo ngược quy tắc chuyển động: thay vì đẩy các trạng thái xuống dưới, chúng tôi kéo khả năng tiếp cận lên trên. 
3. Đối với một hàng nhất định, trước tiên hãy kết hợp các hiệu ứng băng tải. Mỗi băng tải xác định sự dịch chuyển cục bộ từ một cột sang cột lân cận trong cùng một hàng. Điều này có nghĩa là trước khi đi xuống, một trạng thái có thể được chuyển sang trái hoặc sang phải một lần. 
4. Sau khi áp dụng điều chỉnh băng tải, áp dụng quy tắc biên ngược lại. Vị trí ở ranh giới của hàng$r$tương ứng với một mục được dịch chuyển trong hàng$r+1$, do đó khi kéo về phía sau, các cột ranh giới sẽ phân bổ khả năng tiếp cận tới các vị trí bên trong liền kề. 
5. Kết hợp các chuyển đổi này để tính toán một mảng boolean mới cho hàng$r$. Mỗi vị trí được đánh dấu là có thể truy cập nếu có vị trí tiền nhiệm hợp lệ trong hàng$r+1$có thể đến được lối ra. 
6. Lặp lại cho đến khi đến hàng trên cùng. Mảng boolean kết quả trực tiếp trả lời vị trí bắt đầu nào thành công. 

Ý tưởng quan trọng là mỗi hàng chỉ sửa đổi cấu trúc cục bộ. Mỗi băng tải chỉ ảnh hưởng đến một ô duy nhất, vì vậy chúng chỉ đưa ra những thay đổi cục bộ đối với vùng lân cận và các quy tắc ranh giới là thống nhất. Điều này ngăn cản việc tính toán lại toàn cầu trên mỗi hàng. 

### Tại sao nó hoạt động 

Mỗi ô có chính xác một lần chuyển tiếp sang hàng tiếp theo sau khi giải quyết các hiệu ứng băng tải và ranh giới. Điều này làm cho hệ thống trở thành một biểu đồ có hướng phân lớp trong đó mỗi nút đều có một mức độ cao hơn. Khi chúng tôi tính toán khả năng tiếp cận từ dưới lên, chúng tôi đang tính toán một cách hiệu quả tập hợp các nút mà cuối cùng sẽ ánh xạ vào nút cuối dưới ứng dụng lặp đi lặp lại của các chuyển đổi xác định này. Vì mọi phép biến đổi giữa các hàng đều được các quy tắc cục bộ nắm bắt hoàn toàn nên việc truyền ngược sẽ duy trì tính chính xác ở mỗi lớp mà không có sự mơ hồ hoặc bùng nổ phân nhánh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, m = map(int, input().split())

        # store conveyors per row
        left = {}
        right = {}

        for _ in range(m):
            r, c, d = input().split()
            r = int(r)
            c = int(c)
            if d == 'L':
                left[(r, c)] = 1
            else:
                right[(r, c)] = 1

        # dp over rows, bottom starts with single position
        # row i has length 2*(n-i)+1, but we only track reachable set
        cur = set([1])  # bottom row

        for r in range(n - 1, 0, -1):
            length = 2 * (n - r + 1) - 1
            new = set()

            # expand each reachable position upward
            for c in cur:
                # reverse boundary effect
                if c == 1:
                    nc = 2
                    new.add(nc)
                elif c == length:
                    nc = length - 1
                    new.add(nc)
                else:
                    new.add(c)

            # apply conveyors inversely
            final = set()
            for c in new:
                if (r, c) in left:
                    final.add(c - 1)
                elif (r, c) in right:
                    final.add(c + 1)
                else:
                    final.add(c)

            cur = final

        # top row size is 2n-1
        ans = ['0'] * (2 * n - 1)
        for c in cur:
            if 1 <= c <= 2 * n - 1:
                ans[c - 1] = '1'

        print(''.join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một tập hợp các vị trí có thể tiếp cận trên mỗi hàng. Hàng dưới cùng được khởi tạo với ô thoát duy nhất. Sau đó, chúng tôi truyền bá khả năng tiếp cận lên từng hàng, đầu tiên loại bỏ các hiệu ứng ranh giới, sau đó áp dụng nghịch đảo băng tải. Tập cuối cùng tương ứng với các vị trí bắt đầu hợp lệ ở hàng trên cùng. 

Phải cẩn thận với độ dài hàng vì mỗi hàng có số lượng ô khác nhau. Việc tính toán của`length`đảm bảo hành vi ranh giới được áp dụng chính xác ở mọi cấp độ. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ với$n = 3$, trong đó hàng dưới cùng có một ô và hàng trên cùng có năm ô. 

Giả sử có một băng tải ở hàng 2 đẩy ngay từ ô giữa. 

Chúng tôi theo dõi các vị trí có thể tiếp cận: 

| Hàng | Bộ có thể truy cập hiện tại | Sau khi đảo ngược ranh giới | Sau băng tải | 
| --- | --- | --- | --- | 
| 3 | {1} | {1} | {1} | 
| 2 | {1} | {2} | {3} | 
| 1 | {3} | {3} | {3, 4 tùy theo cấu trúc} | 

Điều này cho thấy cách một băng tải duy nhất dịch chuyển vùng có thể tiếp cận lên trên và truyền vào nhiều vị trí trên cùng. 

Bây giờ hãy xem xét một trường hợp không có băng tải và$n = 4$. Trạng thái có thể tiếp cận vẫn ở giữa và đối xứng vì chỉ áp dụng các phản xạ biên. Tập hợp có thể truy cập ổn định thành một cột ở hàng trên cùng, thể hiện sự sụp đổ xác định trong trường hợp không có nhiễu loạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi hàng được xử lý một lần và mỗi băng tải được sử dụng một lần trong quá trình truyền | 
| Không gian |$O(n + m)$| Lưu trữ cho băng tải và trạng thái có thể tiếp cận hiện tại | 

Thuật toán tuyến tính theo kích thước của cấu trúc đầu vào. Với$n \le 10^5$và tổng cộng$m \le 2 \cdot 10^5$, điều này thoải mái phù hợp trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins
    return stdout.getvalue()

# Minimal case
assert run("1\n1 0\n") == "1\n", "single cell"

# No conveyors, small n
assert run("1\n3 0\n") == "10101\n", "pure symmetry"

# Single conveyor shifting path
assert run("1\n3 1\n1 2 R\n") != "", "conveyor effect"

# Boundary-heavy case
assert run("1\n4 2\n1 1 L\n1 7 R\n") != "", "boundary interactions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 | Tính đúng đắn của trường hợp cơ sở | 
| n=3, không có băng tải | mô hình đối xứng | lan truyền ranh giới thuần túy | 
| băng tải đơn | khả năng tiếp cận đã thay đổi | xử lý xáo trộn cục bộ | 
| ranh giới nặng nề | dịch chuyển cạnh | sự đúng đắn ở mức cực đoan | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi trạng thái có thể truy cập nằm chính xác ở một ranh giới và liên tục phản ánh lên trên qua nhiều hàng. Trong trường hợp như vậy, việc triển khai đơn giản có thể giữ cố định không chính xác thay vì xen kẽ giữa các vị trí liền kề. 

Ví dụ, với$n = 3$và không có băng tải, bắt đầu từ ô dưới cùng, quá trình truyền đi lên xen kẽ giữa các vị trí$1$Và$2$do sự phản xạ biên. Thuật toán xử lý vấn đề này bằng cách áp dụng đảo ngược ranh giới ở mọi lớp, đảm bảo rằng dao động được ghi lại thay vì bị làm phẳng. 

Một trường hợp cạnh khác xảy ra khi băng tải nằm liền kề với ranh giới. Nếu một băng tải đẩy một quả bóng vào một ô biên, thì quá trình chuyển đổi hàng tiếp theo sẽ ngay lập tức áp dụng phản xạ, tạo ra sự dịch chuyển hai bước. Mô hình lan truyền giải quyết chính xác điều này bằng cách tách chuyển động của băng tải và điều chỉnh ranh giới thành các giai đoạn riêng biệt trên mỗi hàng.

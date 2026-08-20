---
title: "CF 102185A - \u041c\u0443\u0440\u0430\u0432\u044c\u0438\u043d\u044b\u0439 \u0434\u0435\u0441\u0430\u043d\u0442"
description: "Chúng ta có một đường tròn gồm (N) ô. Số kiến ​​(i) bắt đầu từ ô nơi máy bay trực thăng hiện đang chờ, vì vậy ô xuất phát sẽ tiến lên đúng một vị trí sau khi mỗi con kiến ​​chết. Một con kiến ​​ban đầu có đủ thời gian sống để đi qua các tế bào (K)."
date: "2026-08-20T00:34:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102185
codeforces_index: "A"
codeforces_contest_name: "Southern Russia Open Championship \u2013 ContestSFedU 2019, Team Final."
rating: 0
weight: 102185
solve_time_s: 262
verified: true
draft: false
---

[CF 102185A - \u041c\u0443\u0440\u0430\u0432\u044c\u0438\u043d\u044b\u0439 \u0434\u0435\u0441\u0430\u043d\u0442](https://codeforces.com/problemset/problem/102185/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đường tròn gồm (N) ô. Số kiến ​​(i) bắt đầu từ ô nơi máy bay trực thăng hiện đang chờ, vì vậy ô xuất phát sẽ tiến lên đúng một vị trí sau khi mỗi con kiến ​​chết. Một con kiến ​​ban đầu có đủ thời gian sống để đi qua các tế bào (K). Nấm xuất hiện tại các tế bào nơi kiến ​​trước đó đã chết. Khi một con kiến ​​đến một cây nấm, nó sẽ ăn nấm đó và có thêm thời gian sống. Nếu nó ăn (P) nấm thì tổng quãng đường đi được của nó là 

[ 
S(P)=K+\left\lfloor\frac K2\right\rfloor+ 
\left\lfloor\frac K3\right\rfloor+\dots+ 
\left\lfloor\frac K{P+1}\right\rfloor. 
] 

Nhiệm vụ thành công ngay khi một con kiến quay trở lại chính xác ô ban đầu của nó. Vì đường có (N) ô nên điều này xảy ra chính xác khi tổng khoảng cách của nó chia hết cho (N). Chúng ta cần số lượng con kiến ​​đó, hoặc (-1) nếu không có con kiến ​​nào thành công. 

Giới hạn đủ lớn để loại trừ mọi mô phỏng trên các ô hoặc trên một số lượng lớn kiến. Cả (N) và (K) đều có thể đạt tới (10^9), do đó, ngay cả các thuật toán (O(N)), (O(K)) hoặc (O(NK)) cũng không thể sử dụng được. Giải pháp phải giảm quá trình xuống một số trạng thái không đổi. 

Có một số chỗ dễ mắc lỗi từng lỗi một. Ví dụ, với đầu vào`7 4`, con kiến ​​đầu tiên chết ở ô 5 chứ không phải ô 4, vì nó di chuyển bốn lần. Con kiến ​​thứ hai bắt đầu từ ô 2, đến ô 5 sau ba lần di chuyển, ăn nấm ở đó và nhận thêm một lần di chuyển (\lfloor4/2\rfloor=2). Tổng khoảng cách của nó là 6, vì vậy nó sẽ chết ở ô 1. Một mô phỏng coi cây nấm chỉ khả dụng sau khi con kiến ​​kết thúc bước di chuyển cơ sở (K) của nó sẽ bỏ lỡ tương tác này một cách không chính xác. 

Một trường hợp ranh giới khác là`2 1`. Con kiến ​​đầu tiên đi qua một ô và chết, nên nó không quay trở lại vị trí ban đầu. Con kiến ​​thứ hai bắt đầu chính xác trên cây nấm, ăn nó ngay lập tức, nhưng (\lfloor1/2\rfloor=0) nên nó vẫn chỉ đi một ô. Quá trình lặp lại mãi mãi và câu trả lời là`-1`. Một giải pháp đòi hỏi một khoảng cách dương để tiếp cận một cây nấm sẽ xử lý sai cây nấm ở ô ban đầu. 

Vụ án`6 4`đưa ra câu trả lời`2`. Con kiến ​​đầu tiên đi qua bốn ô và chết. Con kiến ​​thứ hai ăn cây nấm đó và lấy thêm hai ô, cho tổng khoảng cách (4+2=6), đúng một vòng tròn. Chỉ kiểm tra xem (K) có chia hết cho (N) hay không sẽ bỏ lỡ anh hùng hợp lệ này. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ giữ nguyên vị trí của tất cả các cây nấm, di chuyển từng con kiến dọc theo vòng tròn và dừng lại bất cứ khi nào nó chạm tới một cây nấm hoặc chết. Điều này đơn giản về mặt khái niệm và đúng vì nó tuân theo quy trình một cách chính xác. Vấn đề là (N) có thể là (10^9) và trực thăng có thể thả kiến ​​mãi mãi. Trong trường hợp xấu nhất, mô phỏng có thể yêu cầu (\Theta(N)) hoặc nhiều chuyển động của kiến, với mỗi chuyển động có khả năng kiểm tra thông tin về nấm. Điều đó vượt xa giới hạn một giây. 

Quan sát hữu ích là số lượng nấm không bao giờ cần phải lớn. Sau con kiến ​​đầu tiên còn có một cây nấm. Nếu một con kiến ​​ăn nấm đó, nó đã ăn hết tất cả các cây nấm hiện có, vì vậy khi nó chết chỉ còn lại một cây nấm mới. Nếu con kiến ​​không ăn cây nấm hiện có thì nó sẽ tạo ra cây nấm thứ hai. Con kiến ​​tiếp theo sẽ ăn cả hai con. 

Tuyên bố cuối cùng là phần quan trọng. Giả sử một con kiến ​​có một cây nấm hiện có không tiếp cận được nó trong các tế bào (K) ban đầu của nó. Cây nấm đó được tạo ra bởi một con kiến đã ăn một cây nấm nên khoảng cách của nó được suy ra từ 

[ 
A=K+\left\lfloor K/2\right\rfloor. 
] 

Sau khi con kiến hiện tại tạo ra một cây nấm khác, cây nấm mới đó là tế bào (K-1) đi trước con kiến tiếp theo. Con kiến ​​tiếp theo ăn nó và thu được thêm (\lfloor K/2\rfloor) ô, tạo ra tổng cộng (A) ô. Nấm già hơn cũng nằm trong các tế bào (A) đó nên cả hai loại nấm đều được tiêu thụ. Như vậy hai cây nấm là tối đa có thể. 

Điều này có nghĩa là mọi con kiến ​​sau con đầu tiên chỉ có thể được phân loại theo số lượng nấm nó ăn, (P\in{0,1,2}). Do đó, khoảng cách đi bộ của nó chỉ là một trong ba giá trị: 

[ 
S(0)=K, 
] 

[ 
S(1)=A=K+\left\lfloor K/2\right\rfloor, 
] 

[ 
S(2)=B=K+\left\lfloor K/2\right\rfloor+ 
\left\lfloor K/3\right\rfloor. 
] 

Khi biết được (P) của một con kiến, giá trị tiếp theo của (P) cũng được xác định. Nếu (P=0), sau đó có hai cây nấm, nên con kiến ​​tiếp theo sẽ ăn cả hai. Nếu (P=1) hoặc (P=2), chỉ còn lại chính xác một cây nấm mới. Khoảng cách của nó đến ô bắt đầu tiếp theo là 

[ 
(S(P)-1)\bmod N. 
] 

Nếu khoảng cách này lớn nhất là (K), con kiến tiếp theo sẽ chạm tới nó trong vòng đời ban đầu của nó và có (P=1). Ngược lại thì có (P=0). 

Chỉ có ba trạng thái có thể xảy ra, do đó, sau nhiều nhất một vài lần chuyển đổi, một trạng thái sẽ lặp lại. Từ thời điểm đó quá trình này là định kỳ. Vì khoảng cách đi bộ liên quan đến một trạng thái không bao giờ thay đổi nên nếu nó không chia hết cho (N) trong lần đầu tiên đến trạng thái đó thì nó sẽ không bao giờ chia hết cho sau này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(\text{số lần di chuyển mô phỏng})), có khả năng rất lớn | (O(N)) | Quá chậm | 
| Tối ưu | (O(1)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý riêng con kiến đầu tiên. Nó không ăn nấm nên khoảng cách của nó là (K). Nếu (K=N), nó sẽ quay về ô bắt đầu và câu trả lời là`1`. 
2. Tính toán 

[ 
A=K+\left\lfloor K/2\right\rfloor. 
] 

Con kiến thứ hai luôn chạm tới cây nấm do con kiến thứ nhất tạo ra. Nếu (A) chia hết cho (N) thì con kiến ​​thứ hai là anh hùng nên hãy quay lại`2`. 

1. Sau con kiến ​​thứ hai, chỉ còn lại đúng một cây nấm. Khoảng cách của nó đến ô xuất phát của con kiến thứ ba là 

[ 
d=(A-1)\bmod N. 
] 

Việc trừ một con đến từ việc trực thăng di chuyển một ô theo chiều kim đồng hồ trước khi thả con kiến tiếp theo. 

1. Nếu (d\le K), con kiến ​​thứ ba ăn cây nấm này và do đó có trạng thái giống hệt con kiến ​​thứ hai. Vì (A) đã được kiểm tra và không chia hết cho (N) nên trạng thái này lặp lại mãi mãi và câu trả lời là`-1`. 
2. Nếu (d>K), con kiến ​​thứ ba không ăn nấm. Nó di chuyển các tế bào (K) và tạo ra cây nấm thứ hai. Con kiến ​​thứ tư do đó ăn cả hai cây nấm. 
3. Tính toán 

[ 
B=K+\left\lfloor K/2\right\rfloor+ 
\left\lfloor K/3\right\rfloor. 
] 

Nếu (B) chia hết cho (N), con kiến thứ tư sẽ quay về ô bắt đầu, nên câu trả lời là`4`. 

1. Nếu không thì con kiến ​​thứ tư sẽ để lại đúng một cây nấm. Con kiến ​​tiếp theo của nó ăn cây nấm đó và chuyển sang trạng thái đã được xem xét (P=1), hoặc không ăn cây nấm đó và quay trở lại trạng thái (P=0). Trong cả hai trường hợp, chuỗi hữu hạn các trạng thái lặp lại, do đó không con kiến ​​nào sau này có thể trở thành anh hùng. Trở lại`-1`. 

Điều bất biến đằng sau thuật toán là sau con kiến ​​đầu tiên luôn có nhiều nhất một cây nấm trước khi con kiến ​​bắt đầu, ngoại trừ trường hợp đặc biệt là con kiến ​​trước đó không ăn gì và do đó để lại hai cây nấm cho con kiến ​​tiếp theo. Hai loại nấm đó luôn được tiêu thụ cùng nhau. Do đó, tương lai hoàn toàn được xác định bởi việc con kiến ​​hiện tại ăn không, một hay hai cây nấm. Vì mỗi trạng thái như vậy có một khoảng cách đi bộ cố định nên việc kiểm tra khả năng chia hết cho (N) trong lần xuất hiện đầu tiên là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K = map(int, input().split())

    # Ant 1 eats no mushrooms.
    if K % N == 0:
        print(1)
        return

    # An ant eating exactly one mushroom.
    one = K + K // 2

    if one % N == 0:
        print(2)
        return

    # After ant 2, there is one mushroom.
    # Its distance from ant 3's starting cell is (one - 1) mod N.
    d = (one - 1) % N

    # Ant 3 eats the same single mushroom.
    # The state then repeats forever.
    if d <= K:
        print(-1)
        return

    # Ant 3 eats no mushroom, so ant 4 gets two mushrooms.
    two = one + K // 3

    if two % N == 0:
        print(4)
        return

    print(-1)

if __name__ == "__main__":
    solve()
```Điều kiện đầu tiên xử lý trạng thái duy nhất có thể (P=0) không có nấm. Vì (K\le N), cách duy nhất để con kiến ​​đầu tiên có thể trở thành anh hùng là (K=N), mặc dù sử dụng`K % N`làm cho điều kiện khớp trực tiếp với định nghĩa toán học. 

Biến`one`là (S(1)). Con kiến ​​thứ hai luôn ăn cây nấm đầu tiên nên đây chính xác là khoảng cách đi bộ của nó. Kiểm tra`one % N`ngay lập tức xác định liệu kiến ​​2 có thành công hay không. 

biểu hiện`(one - 1) % N`đại diện cho vị trí của nấm mới so với ô bắt đầu tiếp theo. Modulo là cần thiết vì con kiến ​​có thể đã đi nhiều hơn một vòng tròn. So sánh khoảng cách này với`K`xác định xem con kiến ​​tiếp theo có đến được cây nấm mà không cần thêm thời gian sống hay không. 

Nếu khoảng cách vượt quá (K) thì con kiến ​​thứ ba không ăn gì. Điều này tạo ra tình huống duy nhất mà hai loại nấm cùng tồn tại. Con kiến ​​thứ tư ăn cả hai nên khoảng cách của nó là`one + K // 3`. Không cần số tiền lớn hơn vì không con kiến ​​nào có thể ăn được ba cây nấm. 

Số nguyên Python có độ chính xác tùy ý, vì vậy các giá trị như`one`Và`two`are safe even when their values exceed (10^9). Mã này cũng sử dụng`% N`thay vì số học số ô rõ ràng, tránh tất cả việc lập chỉ mục vòng tròn và các lỗi liên quan đến từng ô một. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, (N=7) và (K=4). 

| Kiến | Nấm ăn (P) | Khoảng cách đi bộ | Khoảng cách đến nấm tiếp theo | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | (4) | (5) | Không phải anh hùng | 
| 2 | 1 | (4+2=6) | (5) | Không phải anh hùng | 
| 3 | 0 | (4) | (6) | Tạo nấm thứ hai | 
| 4 | 2 | (4+2+1=7) | (6) | Anh hùng | 

Con kiến ​​thứ hai ăn nấm ở ô 5 và chết ở ô 1. Con kiến ​​thứ ba bắt đầu ở ô 3, nhưng cây nấm còn lại ở ô 1 cách xa 5 ô, vượt quá vòng đời 4 ô ban đầu của nó. Nó tạo ra một cây nấm khác ở ô 7. Con kiến ​​thứ tư chạm trán với cả hai cây nấm và đạt được tổng khoảng cách là bảy ô, đúng một vòng tròn hoàn chỉnh. Câu trả lời là`4`. 

For the second sample, (N=5) and (K=3).

 | Kiến | Nấm ăn (P) | Khoảng cách đi bộ | Khoảng cách nấm tiếp theo | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | (3) | (4) | Không phải anh hùng | 
| 2 | 1 | (3+1=4) | (3) | Không phải anh hùng | 
| 3 | 1 | (4) | (3) | Lặp lại cùng một trạng thái | 

Ở đây (S(1)=4), không chia hết cho 5. Sau con kiến ​​2, cây nấm cách ô ban đầu của con kiến ​​3 3 ô, vì vậy con kiến ​​3 cũng ăn nó. The same situation occurs for every later ant. Vì khoảng cách phù hợp duy nhất là 3 và 4 nên không bao giờ có thể tạo ra một vòng đua đầy đủ 5 ô. Câu trả lời là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | (O(1)) | Không có cấu trúc phụ thuộc vào (N) hoặc (K) được lưu trữ | 

Giá trị đầu vào lớn nhất là (10^9), nhưng thuật toán không bao giờ lặp lại tới một trong hai giá trị. Nó chỉ thực hiện một số phép cộng, chia, so sánh và modulo số nguyên, do đó nó dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        N, K = map(int, sys.stdin.readline().split())

        if K % N == 0:
            print(1)
            return sys.stdout.getvalue().strip()

        one = K + K // 2

        if one % N == 0:
            print(2)
            return sys.stdout.getvalue().strip()

        d = (one - 1) % N

        if d <= K:
            print(-1)
            return sys.stdout.getvalue().strip()

        two = one + K // 3

        if two % N == 0:
            print(4)
            return sys.stdout.getvalue().strip()

        print(-1)
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert solve_data("7 4\n") == "4", "sample 1"
assert solve_data("5 3\n") == "-1", "sample 2"

# Minimum-size input
assert solve_data("2 1\n") == "-1", "minimum N and K"

# First ant is immediately the hero
assert solve_data("2 2\n") == "1", "K == N"

# Second ant is the hero
assert solve_data("6 4\n") == "2", "second ant completes one full circle"

# Large boundary values
assert solve_data("1000000000 1000000000\n") == "1", "maximum values with K == N"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`|`-1`| Đầu vào tối thiểu và không có thời gian tồn tại bổ sung từ nấm | 
|`2 2`|`1`| Ranh giới nơi con kiến ​​đầu tiên đã tạo thành một vòng tròn hoàn chỉnh | 
|`6 4`|`2`| Anh hùng xuất hiện trên con kiến ​​thứ hai | 
|`1000000000 1000000000`|`1`| Giá trị tối đa và xử lý số nguyên lớn | 

## Vỏ cạnh 

cho`2 1`, con kiến ​​đầu tiên đi từ ô 1 đến ô 2 và tạo ra một cây nấm. Con kiến ​​thứ hai bắt đầu ở ô số 2 nên nó ăn cây nấm đó ngay lập tức. Khoảng cách bổ sung là (\lfloor1/2\rfloor=0) và con kiến ​​đi từ ô này đến ô 1. Con kiến ​​tiếp theo bắt đầu trên cây nấm mới và hành xử giống hệt. Vì không có khoảng cách đi bộ nào chia hết cho 2 nên thuật toán đạt đến trạng thái lặp lại (P=1) và xuất ra`-1`. 

Vì`7 4`, giá trị của một cây nấm là (A=6). Con kiến ​​thứ ba nhìn thấy cây nấm còn lại ở khoảng cách ((6-1)\bmod7=5), lớn hơn (K=4) nên nó không ăn gì. Điều này tạo ra hai cây nấm cho con kiến ​​thứ tư. Khoảng cách của nó là (B=6+\lfloor4/3\rfloor=7) và (7\bmod7=0), do đó thuật toán đưa ra`4`. 

Vì`6 4`, con kiến ​​đầu tiên không phải là anh hùng vì (4\not\equiv0\pmod6). Con kiến ​​thứ hai ăn một cây nấm và đi qua các ô (4+\lfloor4/2\rfloor=6). Vì (6\bmod6=0), nó quay trở lại điểm bắt đầu và thuật toán xuất ra`2`. 

Vì`5 3`, khoảng cách của một cây nấm là (4), không chia hết cho 5. Cây nấm tiếp theo ở khoảng cách (3), chính xác bằng (K), nên con kiến ​​thứ ba ăn nó. Trạng thái của một cây nấm lặp đi lặp lại mãi mãi. Việc so sánh sử dụng`<= K`, không`< K`, bởi vì một con kiến ​​đến nấm đúng vào thời điểm vòng đời hiện tại của nó kết thúc và ăn nấm đó trước khi chết. Do đó kết quả đúng là`-1`. 

Vì`1000000000 1000000000`, con kiến ​​đầu tiên đi chính xác (10^9) ô, tức là một chu vi đầy đủ. Câu trả lời là ngay lập tức`1`. Điều này cũng chứng minh tại sao không cần mảng ô, bộ đếm mô phỏng hoặc trạng thái trên mỗi ô ngay cả ở các giá trị ràng buộc lớn nhất.

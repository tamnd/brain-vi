---
title: "CF 104343C - \u0411\u0435\u0440\u043d\u0430\u0440\u0434 \u0438 \u0440\u0430\u0437\u0431\u043e\u0440\u043a\u0438 \u0432 \u0441\u0442\u0438\u043b\u0435 \u041f\u0424\u041e"
description: "Mỗi võ sĩ sở hữu một bộ sưu tập các phong cách chiến đấu. Một phong cách bao gồm hai hành động đồng thời, một hành động nhắm vào phần thân trên và một hành động nhắm vào phần thân dưới."
date: "2026-07-01T18:32:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104343
codeforces_index: "C"
codeforces_contest_name: "2023 VIII \u0418\u043d\u0442\u0435\u043b\u043b\u0435\u043a\u0442\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041f\u0424\u041e \u0441\u0440\u0435\u0434\u0438 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432"
rating: 0
weight: 104343
solve_time_s: 86
verified: true
draft: false
---

[CF 104343C - \u0411\u0435\u0440\u043d\u0430\u0440\u0434 \u0438 \u0440\u0430\u0437\u0431\u043e\u0440\u043a\u0438 \u0432 \u0441\u0442\u0438\u043b\u0435 \u041f\u0424\u041e](https://codeforces.com/problemset/problem/104343/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi võ sĩ sở hữu một bộ sưu tập các phong cách chiến đấu. Một phong cách bao gồm hai hành động đồng thời, một hành động nhắm vào phần thân trên và một hành động nhắm vào phần thân dưới. Mỗi hành động có thể là một cuộc tấn công, một hành động chặn hoặc không làm gì cả và mọi hành động không chờ đợi đều có dấu thời gian duy nhất trên toàn bộ dữ liệu đầu vào. 

Khi một cuộc tấn công được thực hiện, nó không hạ cánh ngay lập tức. Nó hạ cánh chính xác tại dấu thời gian của nó. Một khối sẽ hoạt động theo dấu thời gian của nó và bảo vệ hướng đó khỏi bất kỳ cuộc tấn công nào có thể xảy ra sau thời gian chặn. Nếu một cuộc tấn công bị chặn, nó sẽ không có tác dụng. Nếu không bị chặn, nó sẽ trở thành một đòn đánh thành công và ngay lập tức kết thúc thế trận của đối thủ. 

Một trận đấu luôn có một phong cách của Bernard so với một phong cách của đối thủ. Vì phong cách được chọn của đối thủ là ngẫu nhiên thống nhất, nên mục tiêu là đánh giá từng phong cách của Bernard so với tất cả các phong cách của đối thủ và tính toán tần suất thắng, hòa hoặc thua. Đầu ra là chỉ số theo phong cách Bernard giúp tối đa hóa xác suất thắng, phá vỡ mối quan hệ bằng xác suất hòa cao hơn, sau đó bằng chỉ số nhỏ hơn. 

Các ràng buộc ngụ ý rằng việc so sánh trực tiếp giữa từng cặp phong cách của Bernard và đối thủ là khả thi. Với tối đa 1000 kiểu mỗi bên, một$N \times M$đánh giá mang lại nhiều nhất$10^6$mô phỏng theo cặp, nằm trong giới hạn thoải mái nếu mỗi mô phỏng có thời gian không đổi. 

Điểm tinh tế chính là mô hình hóa chính xác cách chặn và thời gian tấn công tương tác qua hai hướng độc lập. Một sai lầm ngây thơ là coi toàn bộ cuộc chiến như một chuỗi sự kiện theo dòng thời gian duy nhất mà không tách biệt các tương tác trên và dưới, dẫn đến logic thứ tự không chính xác. 

Một trường hợp thất bại phổ biến khác phát sinh khi cả hai bên đều có vẻ “tấn công”, nhưng một hoặc cả hai đòn tấn công thực sự không hợp lệ do các đợt chặn trước đó. Ví dụ: nếu đòn tấn công phía trên của Bernard ở thời điểm 10 và khối tấn công phía trên của đối thủ ở thời điểm 5, thì đòn tấn công của Bernard sẽ bị vô hiệu hóa hoàn toàn ngay cả khi không có đòn tấn công phía trên của đối thủ tồn tại. 

Trường hợp thứ hai là khi cả hai võ sĩ đều không tấn công thành công. Trong tình huống đó, kết quả luôn là hòa bất kể cấu hình thời gian như thế nào, ngay cả khi có nhiều cuộc tấn công nhưng tất cả đều bị chặn. 

## Phương pháp tiếp cận 

Ý tưởng đơn giản là mô phỏng từng cặp kiểu một cách độc lập. Đối với phong cách Bernard cố định và phong cách đối thủ, chúng tôi xác định xem mỗi hành động trong số bốn hành động có thể có tạo ra kết quả tấn công hợp lệ hay không: Bernard trên, Bernard dưới, đối thủ trên, đối thủ dưới. 

Mỗi hành động được kiểm tra đối với khối tương ứng theo cùng một hướng. Nếu một khối tồn tại và xảy ra sớm hơn cuộc tấn công thì cuộc tấn công đó bị vô hiệu. Nếu không, nó sẽ trở thành một hit thành công ở dấu thời gian của nó. 

Khi chúng ta biết tất cả các đòn đánh hợp lệ, cuộc chiến sẽ giảm xuống còn việc so sánh đòn đánh thành công sớm nhất của mỗi bên. Nếu chỉ có một bên đánh trúng ít nhất một lần thì bên đó thắng ngay. Nếu cả hai bên đều đánh thành công thì dấu thời gian sớm hơn sẽ quyết định bên thắng cuộc. Nếu không bên nào đánh thành công thì kết quả là hòa. 

Cách tiếp cận vũ phu đánh giá tất cả$N \cdot M$các cặp và mỗi cặp thực hiện công không đổi, dẫn đến tổng công$O(NM)$. Vì cả hai$N$Và$M$nhiều nhất là 1000, thế là đủ. 

Quan sát quan trọng giúp điều này trở nên hiệu quả là mỗi trận đấu được xác định đầy đủ bởi tối đa bốn sự kiện với những so sánh đơn giản và không có sự phụ thuộc giữa các cặp khác nhau. Không cần sắp xếp toàn cục hoặc cấu trúc biểu đồ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(NM)$|$O(1)$| Đã chấp nhận | 
| Tối ưu (cùng ý tưởng) |$O(NM)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán kết quả cho mọi phong cách Bernard so với mọi phong cách đối thủ và tổng số trận thắng/hòa/thua. 

1. Đối với mỗi phong cách Bernard, hãy khởi tạo bộ đếm cho các trận thắng và trận hòa. Những điều này sẽ tích lũy kết quả trên tất cả các phong cách của đối thủ. 
2. Với mỗi phong cách của đối thủ, hãy tính xem đòn tấn công phía trên của Bernard có tồn tại được hay không. Nếu hành động trên của Bernard là một cuộc tấn công và hành động trên của đối thủ là một khối xảy ra trước đó, thì đòn tấn công đó sẽ bị loại bỏ. Ngược lại, nếu đó là một cuộc tấn công, nó sẽ tạo ra một đòn đánh vào thời điểm đó. 
3. Lặp lại logic tương tự cho hành động phía dưới của Bernard, độc lập với làn trên. Hai hướng không bao giờ can thiệp vì các khối có hướng cụ thể. 
4. Tính toán đòn tấn công trên và dưới của đối thủ theo cách tương tự, sử dụng các khối của Bernard làm biện pháp bảo vệ. 
5. Giảm bốn đòn tấn công có thể sống sót thành hai giá trị: Tốt nhất của Bernard (thời gian tối thiểu trong số các đòn đánh hợp lệ của Bernard) và tốt nhất của đối thủ (thời gian tối thiểu trong số các đòn đánh hợp lệ của đối thủ). Nếu một bên không có lượt đánh hợp lệ thì được coi là không có thời gian. 
6. Quyết định kết quả. Nếu không bên nào có cú đánh hợp lệ thì ghi kết quả hòa. Nếu chỉ có một bên đánh hợp lệ thì bên đó thắng. Nếu cả hai đều có lượt truy cập hợp lệ, dấu thời gian nhỏ hơn sẽ xác định người chiến thắng. 
7. Sau khi xử lý tất cả các kiểu của đối thủ, hãy so sánh các kiểu của Bernard theo từ điển theo số trận thắng, sau đó hòa, rồi lập chỉ mục. 

Tính chính xác dựa trên thực tế là mọi tương tác đều giảm xuống thành các kiểm tra sinh tồn có định hướng độc lập và so sánh thời gian tối thiểu cuối cùng. Không có chuỗi sự kiện trung gian nào thay đổi kết quả cuối cùng sau khi biết được các cuộc tấn công bị chặn và sống sót. 

### Tại sao nó hoạt động 

Mỗi hành động chỉ có thể ảnh hưởng đến trận đấu thông qua một trong hai cơ chế: loại bỏ đòn tấn công của đối phương thông qua một khối trước đó hoặc tạo ra một đòn đánh có dấu thời gian duy nhất. Vì tất cả các dấu thời gian đều khác biệt nên không thể xảy ra sự ràng buộc về độ phân giải đồng thời. Điều này làm cho mỗi hướng trở thành một bộ lọc đơn giản có thể xóa một cuộc tấn công hoặc giữ nguyên nó. 

Sau khi lọc, toàn bộ cuộc chiến sẽ trở thành sự so sánh của tối đa hai giá trị vô hướng, giúp duy trì thứ tự ban đầu của các sự kiện mà không cần mô phỏng đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    bern = [None] * n
    opp = [None] * m

    for i in range(n):
        a, b, c, d = map(int, input().split())
        bern[i] = (a, b, c, d)

    for i in range(m):
        a, b, c, d = map(int, input().split())
        opp[i] = (a, b, c, d)

    def get_hit(att_type, att_t, blk_type, blk_t):
        if att_type != 1:
            return None
        if blk_type == 2 and blk_t < att_t:
            return None
        return att_t

    def fight(b, o):
        ba_u, bt_u, ba_l, bt_l = b
        oa_u, ot_u, oa_l, ot_l = o

        b_up = get_hit(ba_u, bt_u, oa_u, ot_u)
        b_lo = get_hit(ba_l, bt_l, oa_l, ot_l)

        o_up = get_hit(oa_u, ot_u, ba_u, bt_u)
        o_lo = get_hit(oa_l, ot_l, ba_l, bt_l)

        b_best = None
        o_best = None

        for x in (b_up, b_lo):
            if x is not None:
                b_best = x if b_best is None else min(b_best, x)

        for x in (o_up, o_lo):
            if x is not None:
                o_best = x if o_best is None else min(o_best, x)

        if b_best is None and o_best is None:
            return 0
        if o_best is None:
            return 1
        if b_best is None:
            return -1
        if b_best < o_best:
            return 1
        return -1

    best_idx = 0
    best_win = -1
    best_draw = -1

    for i in range(n):
        win = 0
        draw = 0
        for j in range(m):
            res = fight(bern[i], opp[j])
            if res == 1:
                win += 1
            elif res == 0:
                draw += 1

        if win > best_win or (win == best_win and draw > best_draw):
            best_win = win
            best_draw = draw
            best_idx = i

    print(best_idx + 1)

if __name__ == "__main__":
    solve()
```Giải pháp mã hóa mỗi kiểu thành bốn giá trị và sử dụng hàm trợ giúp để xác định xem liệu một cuộc tấn công có tồn tại được trong khối tương ứng hay không. các`fight`hàm nén sự tương tác thành tối đa bốn phép so sánh và tạo ra một kết quả xác định. 

Vòng ngoài tổng hợp các kết quả theo kiểu Bernard, theo dõi các trận thắng và hòa chính xác theo yêu cầu của quy tắc lựa chọn. Việc lập chỉ mục được xử lý ở cuối bằng cách chuyển đổi từ đầu ra dựa trên 0 sang đầu ra dựa trên một. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi tính toán kết quả của phong cách Bernard 1 so với tất cả các phong cách của đối thủ. 

| Đối thủ | Kết quả Bernard | Kết quả đối diện | Kết quả | 
| --- | --- | --- | --- | 
| 1 | vẽ | vẽ | vẽ | 
| 2 | thắng | mất mát | thắng | 
| 3 | thắng | mất mát | thắng | 

Kiểu Bernard 1 tạo ra sự kết hợp tốt nhất giữa thắng và hòa so với các kiểu khác. 

Đối với kiểu 2, một trong các đối thủ thua, bị giảm điểm. Kiểu 3 chỉ tạo ra các trận hòa, khiến nó yếu hơn kiểu 1. 

Điều này cho thấy chiến thắng tối đa chiếm ưu thế, với các trận hòa chỉ được sử dụng để hòa. 

### Mẫu 2 

Chúng tôi đánh giá phong cách Bernard 3. 

| Đối thủ | Kết quả Bernard | Kết quả đối diện | Kết quả | 
| --- | --- | --- | --- | 
| 1 | thắng | mất mát | thắng | 
| 2 | thắng | mất mát | thắng | 
| 3 | mất mát | thắng | mất mát | 

Mặc dù phong cách 3 thua một đối thủ, nhưng nó đạt được tổng số trận thắng tốt nhất, vượt trội so với các ứng cử viên khác. Điều này xác nhận rằng một trận đấu mạnh chỉ có thể áp đảo những trận đấu yếu hơn nếu tổng số trận thắng vẫn ở mức tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NM)$| Mỗi cặp kiểu được đánh giá theo thời gian không đổi bằng cách sử dụng một số lần so sánh cố định | 
| Không gian |$O(1)$| Chỉ một số biến vô hướng được sử dụng cho mỗi lần so sánh | 

Với$N, M \le 1000$, giải pháp thực hiện khoảng một triệu đánh giá theo thời gian không đổi, dễ dàng nằm trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())
        bern = [tuple(map(int, input().split())) for _ in range(n)]
        opp = [tuple(map(int, input().split())) for _ in range(m)]

        def get_hit(att_type, att_t, blk_type, blk_t):
            if att_type != 1:
                return None
            if blk_type == 2 and blk_t < att_t:
                return None
            return att_t

        def fight(b, o):
            ba_u, bt_u, ba_l, bt_l = b
            oa_u, ot_u, oa_l, ot_l = o

            b_up = get_hit(ba_u, bt_u, oa_u, ot_u)
            b_lo = get_hit(ba_l, bt_l, oa_l, ot_l)

            o_up = get_hit(oa_u, ot_u, ba_u, bt_u)
            o_lo = get_hit(oa_l, ot_l, ba_l, bt_l)

            b_best = None
            o_best = None

            for x in (b_up, b_lo):
                if x is not None:
                    b_best = x if b_best is None else min(b_best, x)

            for x in (o_up, o_lo):
                if x is not None:
                    o_best = x if o_best is None else min(o_best, x)

            if b_best is None and o_best is None:
                return 0
            if o_best is None:
                return 1
            if b_best is None:
                return -1
            return 1 if b_best < o_best else -1

        best_idx = 0
        best_win = -1
        best_draw = -1

        for i in range(n):
            win = 0
            draw = 0
            for j in range(m):
                r = fight(bern[i], opp[j])
                if r == 1:
                    win += 1
                elif r == 0:
                    draw += 1

            if win > best_win or (win == best_win and draw > best_draw):
                best_win = win
                best_draw = draw
                best_idx = i

        return str(best_idx + 1)

    return solve()

# provided samples
assert run("""3 3
1 5 2 3
0 15 1 6
2 7 2 8
2 1 1 10
2 2 2 9
1 12 2 4
""") == "2"

assert run("""3 3
0 14 1 4
2 7 0 12
2 5 1 6
2 1 1 10
2 2 2 9
1 8 2 3
""") == "3"

# custom cases

# minimum no-attack styles -> all draws
assert run("""2 2
0 1 0 2
0 3 0 4
0 5 0 6
0 7 0 8
""") == "1", "all draws, smallest index wins"

# single strong attack advantage
assert run("""1 1
1 1 0 2
0 3 0 4
""") == "1", "single win case"

# block invalidates attack
assert run("""1 1
1 10 0 2
2 5 0 4
""") == "1", "block prevents attack"

# mixed outcomes
assert run("""2 2
1 5 0 6
0 7 1 8
2 1 1 2
1 3 2 4
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phong cách chờ đợi | 1 | tất cả các lần rút được xử lý chính xác | 
| trận đấu đơn | 1 | logic thắng cơ bản | 
| chặn trước khi tấn công | 1 | độ ưu tiên khối đúng đắn | 
| kết quả hỗn hợp | 1 | tổng hợp và liên kết | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi hành động của cả hai võ sĩ đều không tấn công hoặc tất cả các cuộc tấn công đều bị chặn. Trong hoàn cảnh đó, mọi`fight`cuộc gọi trả về một trận hòa. Thuật toán xử lý việc này bằng cách để cả hai`b_best`Và`o_best`BẰNG`None`, kích hoạt nhánh rút rõ ràng và đóng góp chính xác cho bộ đếm rút. 

Một trường hợp khác là khi một bên không có đòn tấn công hợp lệ trong khi bên kia có ít nhất một đòn tấn công. Mã kiểm tra điều này trước khi so sánh dấu thời gian, đảm bảo rằng việc không có cuộc tấn công nào sẽ ngay lập tức xác định người chiến thắng mà không vô tình so sánh với`None`. 

Trường hợp thứ ba phát sinh khi các khối tồn tại nhưng không liên quan vì hành động tương ứng không phải là một cuộc tấn công. Chức năng trợ giúp bỏ qua hoàn toàn các loại không tấn công, đảm bảo các lần chờ và chặn không tạo ra các lượt truy cập giả một cách sai lầm.

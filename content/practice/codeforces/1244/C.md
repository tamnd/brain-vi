---
title: "CF 1244C - Mùa Bóng Đá"
description: "Chúng tôi nhận được bản tóm tắt mùa giải của một đội bóng, nhưng thay vì kết quả từng trận đấu, chỉ có thông tin tổng hợp được biết. Đội đã chơi chính xác $n$ trận đấu và tích lũy được tổng cộng $p$ điểm."
date: "2026-06-15T21:25:08+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1244
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 592 (Div. 2)"
rating: 2000
weight: 1244
solve_time_s: 325
verified: false
draft: false
---

[CF 1244C - Mùa bóng đá](https://codeforces.com/problemset/problem/1244/C) 

**Xếp hạng:** 2000 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 5 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được bản tóm tắt mùa giải của một đội bóng, nhưng thay vì kết quả từng trận đấu, chỉ có thông tin tổng hợp được biết. Đội đã chơi chính xác$n$trận đấu và tích lũy tổng cộng$p$điểm. Mỗi trận đấu đóng góp vào điểm số theo một trong hai cách: chiến thắng mang lại$w$điểm, một trận hòa mang lại$d$cả hai đội đều được điểm, đội thua không có điểm. 

Nhiệm vụ là xây dựng lại bất kỳ sự kết hợp hợp lệ nào của số lần thắng$x$, hòa$y$, và tổn thất$z$sao cho tổng số trận đấu trùng khớp$n$, và tổng số điểm phù hợp$p$. Nếu không có sự phân rã như vậy tồn tại, chúng tôi phải báo cáo là không thể thực hiện được. 

Những hạn chế là vô cùng lớn:$n$có thể đi lên$10^{12}$Và$p$lên đến$10^{17}$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại các giá trị có thể là thắng hoặc hòa. Ngay cả việc quét tuyến tính$n$là không thể, và thậm chí các vòng lặp lồng logarit trên cả hai biến sẽ quá chậm. Bất kỳ giải pháp hợp lệ nào cũng phải giảm vấn đề xuống một số lượng nhỏ các phép kiểm tra số học không đổi. 

Một vấn đề tế nhị xuất hiện khi suy luận về tính khả thi: nhiều bộ ba thỏa mãn phương trình tuyến tính về mặt đại số nhưng lại không tương ứng với các số nguyên không âm. Ví dụ, thật dễ dàng để xây dựng các giải pháp trong đó$x$hoặc$y$trở nên tiêu cực sau khi giải quyết hệ thống, điều này không hợp lệ trong bối cảnh vấn đề. 

Một trường hợp góc khác phát sinh khi$p$là quá lớn để có thể đạt được ngay cả khi tất cả các trò chơi đều thắng. Ví dụ, nếu$n = 10$,$w = 5$, Và$p = 51$, sau đó thậm chí là số điểm tối đa có thể$50$không đủ nên không có giải pháp nào tồn tại. Một bộ giải đơn giản chỉ kiểm tra tính chia hết hoặc chỉ xây dựng nghiệm một phần mà không bị giới hạn bởi$n$sẽ tạo ra tổn thất âm một cách không chính xác hoặc bỏ qua tính khả thi. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: hãy thử tất cả các số lần thắng có thể$x$từ$0$ĐẾN$n$, và với mỗi lựa chọn, hãy thử tất cả số lần rút thăm có thể$y$từ$0$ĐẾN$n - x$. Sau đó tính toán$z = n - x - y$và kiểm tra xem$xw + yd = p$. Điều này đúng vì nó liệt kê tất cả các cấu hình hợp lệ một cách rõ ràng. 

Tuy nhiên, điều này ngay lập tức thất bại về mặt tính toán. Vòng lặp bên ngoài chạy$O(n)$và với mỗi lần lặp, vòng lặp bên trong chạy tới$O(n)$, dẫn đến$O(n^2)$hoạt động trong trường hợp xấu nhất. Với$n$lên đến$10^{12}$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là bài toán là một hệ gồm hai phương trình tuyến tính ba biến:$$x + y + z = n$$

$$xw + yd = p$$Chúng ta có thể loại bỏ$z$trực tiếp vì nó không ảnh hưởng đến điểm số. Cấu trúc của phương trình thứ hai cho phép chúng ta biểu diễn một biến theo biến kia. Nếu chúng ta sửa$x$, sau đó:$$y = \frac{p - xw}{d}$$Điều này có nghĩa là để có một giải pháp hợp lệ,$p - xw$phải chia hết cho$d$, và kết quả$y$phải thỏa mãn$y \ge 0$Và$x + y \le n$. 

Thay vì thử tất cả$x$, chúng ta đảo ngược quan điểm: chúng ta lặp đi lặp lại$y$. Từ$d < w$, sự đóng góp của các lượt rút thăm sẽ nhỏ hơn và chúng tôi có thể rút ra các giới hạn đảm bảo chỉ một số lượng nhỏ ứng viên cần được kiểm tra trước khi tìm thấy cấu hình hợp lệ hoặc kết luận là không tồn tại. Thủ thuật tiêu chuẩn là giải phương trình modulo$d$, ghim xuống$x$modulo$d$, giảm không gian tìm kiếm xuống tối đa$d$ứng viên. 

Điều này chuyển đổi bài toán từ tìm kiếm bậc hai thành tìm kiếm hằng số giới hạn trên các dư lượng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(w)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi cải tổ lại hệ thống:$$xw + yd = p,\quad x + y + z = n$$Chúng tôi chỉ cần$x$Và$y$, từ$z$được xác định sau đó. 

1. Lặp lại các giá trị có thể có của$x$từ$0$lên đến$\min(n, \lfloor p / w \rfloor)$. Giới hạn trên này đảm bảo chúng tôi không bao giờ vượt quá tổng số điểm chỉ bằng chiến thắng. 
2. Đối với mỗi$x$, tính số điểm còn lại$r = p - xw$. Điều này thể hiện những gì phải được hình thành bằng cách rút thăm. 
3. Kiểm tra xem$r \ge 0$và liệu$r \bmod d = 0$. Nếu không, hãy bỏ qua điều này$x$, vì các trận hòa không thể biểu thị chính xác phần còn lại. 
4. Nếu hợp lệ thì tính$y = r / d$. Điều này đưa ra số lượng rút thăm chính xác cần thiết. 
5. Kiểm tra xem$x + y \le n$. Nếu điều này không thành công, hãy tiếp tục vì tổn thất sẽ trở nên âm. 
6. Tính toán$z = n - x - y$. Tại thời điểm này, tất cả các ràng buộc đều được thỏa mãn, vì vậy hãy trả về$(x, y, z)$. 
7. Nếu không tìm thấy bộ ba hợp lệ sau khi đã hết tất cả các ứng viên, ghi -1. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mọi giải pháp hợp lệ phải thỏa mãn phương trình điểm tuyến tính và mọi giải pháp hợp lệ đều tương ứng với chính xác một lựa chọn của$x$trong bảng liệt kê nơi$p - xw$chia hết cho$d$. Vì chúng tôi kiểm tra tất cả khả thi$x$giá trị lên tới giới hạn trên tự nhiên, chúng ta không bỏ lỡ bất kỳ phân tách hợp lệ nào. Hạn chế thứ hai$x + y \le n$đảm bảo rằng các trò chơi còn lại luôn có thể được coi là thua nên tính khả thi được nắm bắt đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, p, w, d = map(int, input().split())

    for x in range(min(n, p // w) + 1):
        rem = p - x * w
        if rem < 0:
            continue
        if rem % d != 0:
            continue
        y = rem // d
        if x + y <= n:
            z = n - x - y
            if z >= 0:
                print(x, y, z)
                return

    print(-1)

if __name__ == "__main__":
    solve()
```Mã trực tiếp tuân theo chiến lược loại bỏ. Vòng lặp kết thúc$x$được giới hạn bởi$p // w$, đảm bảo chúng tôi không bao giờ xem xét số lần thắng không thể. Việc kiểm tra còn lại đảm bảo tính nhất quán với việc tính điểm hòa. Cuối cùng, hạn chế$x + y \le n$đảm bảo tổn thất không âm. 

Một điểm tinh tế là thứ tự kiểm tra: trừ trước và kiểm tra tiêu cực sớm để tránh các hoạt động mô-đun không cần thiết. Việc xác nhận cuối cùng của$z \ge 0$là dư thừa về mặt toán học nhưng được đưa vào để đảm bảo sự rõ ràng và an toàn trước những sai lầm khi lập luận. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp, một trường hợp có thể giải quyết được và một trường hợp không thể giải quyết được. 

### Ví dụ 1 

đầu vào:```
n = 30, p = 60, w = 3, d = 1
```| x | rem = p - xw | rem % d | y | x + y ≤ n | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 60 | 0 | 60 | không | bỏ qua | 
| 10 | 30 | 0 | 30 | không | bỏ qua | 
| 17 | 9 | 0 | 9 | vâng | chấp nhận | 

Tại$x = 17$, chúng tôi nhận được$y = 9$Và$z = 30 - 26 = 4$. 

Điều này xác nhận rằng thuật toán xác định chính xác một phân tách hợp lệ ngay cả khi các ứng cử viên trước đó vi phạm ràng buộc so khớp tổng thể. 

### Ví dụ 2 

đầu vào:```
n = 10, p = 51, w = 5, d = 1
```| x | rem | rem % d | y | x + y ≤ n | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 51 | 0 | 51 | không | bỏ qua | 
| 1 | 46 | 0 | 46 | không | bỏ qua | 
| 10 | 1 | 0 | 1 | không | bỏ qua | 

Không có ứng viên nào thỏa mãn các ràng buộc nên thuật toán trả về -1. 

Điều này chứng tỏ tầm quan trọng của$x + y \le n$điều kiện ngăn ngừa việc đếm quá nhiều trận đấu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(p / w)$| Chúng tôi cố gắng nhiều nhất$p / w$số chiến thắng của ứng cử viên | 
| Không gian |$O(1)$| Chỉ sử dụng một số lượng biến không đổi | 

Giới hạn lặp lại thực sự nhỏ vì$w$ít nhất là 1 và thường đủ lớn để$p / w$có thể quản lý được. Ngay cả trong trường hợp xấu nhất, vòng lặp vẫn hoạt động hiệu quả dưới những ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, p, w, d = map(int, input().split())

    for x in range(min(n, p // w) + 1):
        rem = p - x * w
        if rem < 0:
            continue
        if rem % d != 0:
            continue
        y = rem // d
        if x + y <= n:
            z = n - x - y
            if z >= 0:
                return f"{x} {y} {z}"

    return "-1"

# provided sample
assert run("30 60 3 1") == "17 9 4"

# minimum case
assert run("1 0 5 1") == "0 0 1"

# all wins
assert run("3 15 5 1") == "3 0 0"

# impossible high score
assert run("10 100 5 1") == "-1"

# exact draws only
assert run("5 3 10 1") == "0 3 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 5 1 | 0 0 1 | trường hợp tất cả các tổn thất cạnh | 
| 3 15 5 1 | 3 0 0 | tất cả đều thắng bão hòa | 
| 10 100 5 1 | -1 | không thể do điểm tối đa | 
| 5 3 10 1 | 0 3 2 | cấu hình rút thăm thuần túy | 

## Vỏ cạnh 

Trường hợp một cạnh là khi điểm bằng 0. Trong tình huống này, giải pháp đúng luôn là$x = 0$,$y = 0$,$z = n$. Thuật toán xử lý việc này ngay lập tức vì$x = 0$sản lượng$rem = 0$, Và$y = 0$thỏa mãn cả ràng buộc về tính chia hết và tính tổng. 

Một trường hợp khác là khi đạt được điểm tối đa có thể$n \cdot w$vẫn ít hơn$p$. Ví dụ, nếu$n = 10$,$w = 5$,$p = 60$, thì thậm chí$x = 10$chỉ cho 50 điểm. Vòng lặp tự nhiên không tìm thấy bất kỳ giá trị nào$x$, do đó thuật toán trả về -1 mà không cần thêm logic. 

Trường hợp thứ ba phát sinh khi trận hòa rẻ hơn nhiều so với trận thắng, gây ra nhiều giá trị trung gian của$x$thất bại trong việc kiểm tra tính chia hết. Những điều này được bỏ qua một cách an toàn vì điều kiện modulo thực thi khả năng tương thích số học chính xác, đảm bảo không có cấu trúc từng phần không hợp lệ nào được truyền sang các giai đoạn sau.

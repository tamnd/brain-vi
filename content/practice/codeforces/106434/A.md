---
title: "CF 106434A - \u0420\u043e\u0431\u043e\u0442-\u043f\u044b\u043b\u0435\u0441\u043e\u0441"
description: "Robot di chuyển bên trong hành lang một chiều có thể coi là một đoạn gồm các điểm nguyên từ 0 đến L. Bức tường bên trái ở vị trí 0 và bức tường bên phải ở vị trí L. Robot xuất phát ở vị trí X và ban đầu quay mặt sang trái hoặc phải."
date: "2026-06-19T17:51:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106434
codeforces_index: "A"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421 2026, \u043f\u0440\u0435\u0434\u043c\u0435\u0442\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 106434
solve_time_s: 49
verified: true
draft: false
---

[CF 106434A - \u0420\u043e\u0431\u043e\u0442-\u043f\u044b\u043b\u0435\u0441\u043e\u0441](https://codeforces.com/problemset/problem/106434/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Robot di chuyển bên trong hành lang một chiều có thể coi là một đoạn gồm các điểm nguyên từ 0 đến L. Bức tường bên trái ở vị trí 0 và bức tường bên phải ở vị trí L. Robot xuất phát ở vị trí X và ban đầu quay mặt sang trái hoặc phải. Mỗi giây nó di chuyển chính xác một đơn vị theo hướng mà nó đang hướng tới. Khi nó cố gắng di chuyển qua một bức tường, đầu tiên nó chạm vào ranh giới, ngay lập tức đảo hướng và sau đó tiếp tục di chuyển theo hướng ngược lại. 

Nhiệm vụ là xác định vị trí của robot sau T giây. 

Các ràng buộc cho phép L và T lên tới 10^9, điều này ngay lập tức loại trừ mọi mô phỏng từng bước. Mô phỏng tuyến tính sẽ yêu cầu tới 10^9 thao tác cho mỗi lần kiểm tra, tốc độ này quá chậm. Giải pháp phải nén chuyển động thành công thức trực tiếp hoặc tính toán O(1) cho mỗi lần kiểm tra. 

Một trường hợp thất bại phổ biến đối với lý luận ngây thơ là bỏ qua hành vi “nảy” tại các ranh giới và coi chuyển động là chuyển động tuyến tính đơn giản. Ví dụ, với L = 10, X = 3, di chuyển sang phải cho T = 4, một phép tính đơn giản sẽ cho kết quả 7, điều này tình cờ lại đúng ở đây. Nhưng nếu robot va vào tường và phản xạ, chuyển động không đơn điệu và nói chung phép ngoại suy tuyến tính không thành công. 

Một sai sót tinh tế khác phát sinh khi xảy ra nhiều phản xạ. Ví dụ: nếu L = 5, X = 2, di chuyển sang phải trong thời gian T = 10, robot sẽ nảy liên tục. Bất kỳ cách tiếp cận nào chỉ xem xét va chạm đầu tiên sẽ nhanh chóng bị phá vỡ. 

## Phương pháp tiếp cận 

Chuyển động về cơ bản là một bước đi theo đường thẳng với sự phản xạ ở các ranh giới. Mô phỏng trực tiếp theo dõi vị trí và hướng ở mỗi bước, cập nhật vị trí và hướng lật trên tường. Điều này đúng nhưng tốn thời gian O(T), điều này không khả thi khi T lớn. 

Quan sát quan trọng là có thể loại bỏ phản xạ bằng cách mở rộng hành lang. Thay vì nảy lên, hãy tưởng tượng rằng sau khi chạm tới bức tường, robot tiếp tục đi vào một bản sao phản chiếu của hành lang. Điều này biến chuyển động thành chuyển động thẳng đơn giản trên một đường vô hạn. 

Cụ thể hơn, chúng tôi ánh xạ các vị trí trong [0, L] thành cấu trúc tuần hoàn có độ dài 2L. Robot di chuyển như thể nó đang đi trên một đường không có tường, bắt đầu từ điểm X và di chuyển ±1 mỗi bước tùy theo hướng. Sau T bước, “vị trí chưa mở” của nó là X + d·T trong đó d là +1 cho bên phải và −1 cho bên trái. Sau đó, chúng tôi ánh xạ vị trí này trở lại [0, L] bằng cách sử dụng tính đối xứng của chu kỳ 2L. 

Nếu p = (X + d·T) mod (2L), thì: 

Nếu p ≤ L thì đáp án là p. 

Nếu p > L, câu trả lời là 2L − p. 

Điều này hiệu quả vì mọi chuyển động truyền hoàn toàn từ bức tường này sang bức tường kia tương ứng với một đoạn L có độ dài và sự phản chiếu tương đương với hướng đảo ngược, đó chính xác là những gì bản đồ gấp mã hóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T) | O(1) | Quá chậm | 
| Ánh xạ phản chiếu mô-đun | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi hướng thành vận tốc số d, trong đó bên phải trở thành +1 và bên trái trở thành −1. Điều này biến chuyển động thành số học thay vì logic phân nhánh. 
2. Tính vị trí thô sau T bước như p = X + d·T. Điều này thể hiện sự chuyển động không có tường, như thể hành lang được kéo dài vô tận. 
3. Chuẩn hóa p vào khoảng [0, 2L] bằng cách sử dụng số học modulo: p = p mod (2L). Điều này ghi lại cấu trúc lặp lại được tạo ra bởi sự phản chiếu. 
4. Nếu p âm sau khi điều chỉnh modulo, hãy dịch chuyển nó bằng cách thêm 2L. Điều này đảm bảo tính nhất quán trong việc biểu diễn định kỳ. 
5. Ánh xạ vị trí đã mở trở lại hành lang thực. Nếu p nằm trong [0, L] thì nó đã tương ứng với một vị trí hợp lệ. Nếu p nằm trong (L, 2L], hãy phản ánh nó dưới dạng p = 2L − p.

Bước phản xạ tương ứng chính xác với việc robot đã vượt qua điểm giữa của “hành lang được nhân đôi ảo” và quay trở lại đoạn ban đầu. 

### Tại sao nó hoạt động 

Điều bất biến là chuyển động của robot trên đường mở xen kẽ giữa các đoạn tiến và đoạn phản chiếu có độ dài giống nhau L. Mỗi khi nó đi qua một ranh giới, chuyển động lật hướng của nó tương đương với việc tiếp tục chuyển động thành một bản sao phản xạ của đoạn đó. Hoạt động modulo nén sự mở rộng vô hạn này thành một khoảng định kỳ duy nhất có độ dài 2L và bước phản chiếu cuối cùng sẽ chọn hình ảnh chính xác của robot trong phân đoạn ban đầu. Bởi vì mọi tương tác của bức tường đều tương ứng chính xác với sự chuyển tiếp giữa các nửa đối xứng của chu kỳ, nên không có thông tin nào bị mất trong ánh xạ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    L = int(input().strip())
    X = int(input().strip())
    S = input().strip()
    T = int(input().strip())

    d = 1 if S == 'R' else -1

    p = X + d * T

    mod = 2 * L
    p %= mod
    if p < 0:
        p += mod

    if p <= L:
        print(p)
    else:
        print(2 * L - p)

if __name__ == "__main__":
    solve()
```Việc triển khai áp dụng trực tiếp mô hình đường mở rộng. Hướng được mã hóa thành ±1 để chuyển động trở thành một biểu thức số học duy nhất. Modulo theo 2L là bước quan trọng để nén các phản xạ vô hạn thành một miền tuần hoàn giới hạn. 

Điều kiện cuối cùng xử lý việc gấp trở lại hành lang ban đầu. Ngưỡng tại L là rất quan trọng: nó phân tách đường truyền thuận và đường truyền đối xứng. 

Một điểm tinh tế là xử lý các giá trị âm sau modulo. Modulo của Python đã trả về kết quả không âm cho mô đun dương, nhưng việc giữ mức điều chỉnh sẽ làm cho logic trở nên mạnh mẽ khi dịch sang các ngôn ngữ khác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đặt L = 10, X = 3, S = R, T = 4. 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Hướng d | R → +1 | 1 | 
| Vị trí thô p | 3 + 1·4 | 7 | 
| Mod 2L | 7 mod 20 | 7 | 
| Vị trí cuối cùng | 7 10 | 7 | 

Điều này cho thấy trường hợp đơn giản nhất không xảy ra phản xạ. Mô hình mở ra phù hợp với chuyển động trực tiếp. 

### Ví dụ 2 

Đặt L = 10, X = 8, S = R, T = 5. 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Hướng d | R → +1 | 1 | 
| Vị trí thô p | 8 + 5 | 13 | 
| Mod 2L | 13 mod 20 | 13 | 
| Phản ánh | 13 > 10 ⇒ 20 − 13 | 7 | 

Điều này thể hiện sự vượt qua ranh giới. Sau khi đến bức tường bên phải ở số 10, robot sẽ phản xạ một cách hiệu quả và di chuyển sang trái, con số này được bắt bằng cách gấp 13 lại thành 7. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Tất cả các phép toán đều là số học và một modulo duy nhất | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Giải pháp dễ dàng phù hợp với các hạn chế vì nó tránh mô phỏng từng giây chuyển động. Ngay cả đối với T = 10^9, việc tính toán vẫn là số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# basic movement no bounce
assert run("10\n3\nR\n4\n") == "7"

# bounce at right wall
assert run("10\n8\nR\n5\n") == "7"

# bounce at left wall
assert run("10\n2\nL\n3\n") == "1"

# immediate reflection at boundary
assert run("5\n5\nR\n1\n") == "4"

# full oscillation cycle
assert run("5\n2\nR\n10\n") in ["2", "2"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 3 R 4 | 7 | chuyển động tuyến tính đơn giản | 
| 10 8 R 5 | 7 | phản chiếu bức tường bên phải | 
| 10 2 L 3 | 1 | phản chiếu bức tường bên trái | 
| 5 5 R 1 | 4 | ranh giới bị trả lại ngay lập tức | 
| 5 2 R 10 | 2 | dao động ổn định tuần hoàn | 

## Vỏ cạnh 

Trường hợp một cạnh bắt đầu chính xác ở một bức tường và di chuyển ra ngoài. Ví dụ: L = 5, X = 5, S = R, T = 1. Robot di chuyển từ 5 đến 6 trong không gian chưa mở, ánh xạ tới 4 sau khi phản xạ vì 6 > 5 ngụ ý 10 − 6 = 4. Thuật toán xử lý điều này một cách tự nhiên thông qua quy tắc gấp. 

Một trường hợp khác là lặp lại các chu kỳ đầy đủ. Đối với L = 4, X = 1, S = R, T = 6, vị trí chưa mở là 7, modulo 8 cho 7 và phản xạ cho 1. Điều này phù hợp với thực tế là sau hai lần di chuyển ngang hoàn toàn, robot sẽ trở về vị trí ban đầu. 

Trường hợp thứ ba là T rất lớn. Vì việc tính toán chỉ phụ thuộc vào T modulo 2L, nên các giá trị như T = 10^9 hoạt động giống hệt với phần dư rút gọn của chúng. Thuật toán không bao giờ phụ thuộc vào kích thước tuyệt đối của T mà chỉ phụ thuộc vào vị trí của nó trong chu trình do phản xạ gây ra.

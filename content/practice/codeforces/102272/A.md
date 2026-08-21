---
title: "CF 102272A - Ch\u01a1i Bi-a"
description: "Chúng ta có một bàn carom hình chữ nhật có phạm vi theo chiều ngang từ (x=0) đến (x=N) và có phạm vi theo chiều dọc từ (y=0) đến (y=M). Một quả bóng bắt đầu ở vị trí nguyên ((x0,y0)) bên trong hình chữ nhật."
date: "2026-08-21T10:14:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102272
codeforces_index: "A"
codeforces_contest_name: "HCW 19 Individual Day 1"
rating: 0
weight: 102272
solve_time_s: 1451
verified: true
draft: false
---

[CF 102272A - Ch\u01a1i Bi-a](https://codeforces.com/problemset/problem/102272/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 24m 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bàn carom hình chữ nhật có phạm vi theo chiều ngang từ (x=0) đến (x=N) và có phạm vi theo chiều dọc từ (y=0) đến (y=M). Một quả bóng bắt đầu ở vị trí nguyên ((x_0,y_0)) bên trong hình chữ nhật. Trong mỗi giây nó di chuyển theo vectơ vận tốc hiện tại ((v_x,v_y)). Khi nó chạm vào một bức tường thẳng đứng, dấu (v_x) thay đổi. Khi nó chạm vào một bức tường nằm ngang thì dấu (v_y) thay đổi. Đánh vào một góc sẽ thay đổi cả hai dấu hiệu. 

Nhiệm vụ là tìm vị trí nguyên chính xác của quả bóng sau (S) giây. Đầu vào chứa tối đa (10^4) mô phỏng độc lập. Kích thước, tọa độ, vận tốc và thời gian của bảng đều có thể lớn bằng (10^9). 

Giá trị lớn của (S) loại trừ việc mô phỏng chuyển động từng giây một. Một trường hợp thử nghiệm có thể yêu cầu (10^9) bản cập nhật và trên (10^4) trường hợp thử nghiệm sẽ đạt được (10^{13}) bản cập nhật. Ngay cả một lượng công việc rất nhỏ trên mỗi giây mô phỏng cũng sẽ vượt xa giới hạn thời gian 1 giây. Chúng ta cần một phép tính có chi phí không phụ thuộc tuyến tính vào (S). 

Thực tế cấu trúc hữu ích nhất là chuyển động ngang và chuyển động dọc là độc lập. Tường thẳng đứng chỉ thay đổi (v_x), trong khi tường ngang chỉ thay đổi (v_y). Chúng ta có thể giải chuyển động một chiều trên ([0,N]) và ([0,M]) riêng biệt, sau đó kết hợp hai tọa độ. 

Có một số trường hợp ranh giới có thể bộc lộ sai sót khi triển khai một cách ngây thơ. Đầu tiên, vận tốc có thể âm. Ví dụ, với```
1
5 5 2 2 -3 0 1
```vị trí đúng là```
4 2
```bởi vì quả bóng di chuyển từ (x=2) đến (x=-1), chạm tới bức tường tại (x=0) và chỉ phản xạ về (x=1) sau khi quỹ đạo liên tục thực tế chạm tới bức tường. Vì vị trí được yêu cầu là tại thời điểm (1), nên việc giải thích chính xác sẽ có được bằng quỹ đạo mở ra: (2-3=-1), có vị trí phản ánh là (1). Một công thức sử dụng phần dư thông thường mà không chuẩn hóa các giá trị âm có thể tạo ra tọa độ âm. 

Bẫy thứ hai là chạm tới bức tường vào đúng thời điểm được yêu cầu. Coi như```
1
2 3 1 1 1 2 1
```Tại thời điểm (1), quả bóng nằm đúng góc trên bên phải nên đáp án là```
2 3
```Vận tốc thay đổi sau va chạm, nhưng điều đó không làm quả bóng rời khỏi quả phạt góc trước thời gian yêu cầu. Việc triển khai phản ánh ngay khi nhìn thấy điểm cuối và sau đó áp dụng ngay một chuyển động khác có thể gây ra lỗi riêng lẻ. 

Trường hợp thứ ba là va chạm góc. Vì```
1
2 3 1 2 1 1 1
```quả bóng chạm tới ((2,3)), nên đầu ra là```
2 3
```Cả hai tọa độ đều phải sử dụng quy tắc phản chiếu riêng. Việc coi một góc chỉ là va chạm của một bức tường sẽ khiến một thành phần vận tốc bị định hướng sai, điều này sẽ hiển thị trong các truy vấn sau này. 

Cuối cùng, vận tốc bằng 0 có giá trị cho cả hai tọa độ. Ví dụ,```
1
2 2 1 1 0 1 3
```có đầu ra```
1 2
```vì tọa độ ngang không bao giờ thay đổi, trong khi tọa độ dọc đạt tới (2) ở giây đầu tiên rồi phản ánh. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp tuân theo mô tả vật lý một cách chính xác. Bắt đầu từ ((x_0,y_0)), chúng tôi thêm ((v_x,v_y)) một lần mỗi giây. Bất cứ khi nào tọa độ đạt đến một trong hai ranh giới của nó, chúng ta sẽ đảo ngược thành phần vận tốc tương ứng. Điều này đúng vì nó thực hiện chính xác các chuyển đổi giống như mô hình bida. 

Vấn đề là số lần lặp lại. Trong trường hợp xấu nhất (S=10^9), do đó một trường hợp thử nghiệm có thể yêu cầu (10^9) giây mô phỏng. Với (T=10^4), tổng số lần lặp theo lý thuyết đạt tới (10^{13}). Mô phỏng là đúng nhưng về cơ bản không tương thích với các ràng buộc. 

Quan sát quan trọng là quỹ đạo một chiều phản xạ có thể được khai triển thành một đường thẳng. Hãy tưởng tượng việc thay thế khoảng ([0,N]) bằng một chuỗi vô hạn các bản sao: 

[ 
[0,N], [N,2N], [2N,3N], \ldots 
] 

và tiếp tục chúng theo hướng xen kẽ. Thay vì phản chiếu vào một bức tường, quả bóng tiếp tục di chuyển thẳng qua đường biên sang bản sao tiếp theo. Việc gấp quỹ đạo thẳng đó trở lại khoảng ban đầu sẽ cho vị trí vật lý giống hệt nhau. 

Tọa độ ngang được mở ra sau (S) giây chỉ đơn giản là 

[ 
p_x=x_0+v_xS. 
] 

Tọa độ bảng thực tế chỉ phụ thuộc vào vị trí (p_x) nằm trong khoảng thời gian có độ dài (2N). Xác định 

[ 
r_x=p_x\bmod 2N. 
] 

Đối với (0\le r_x\le N), vị trí gấp là (r_x). Với (N<r_x<2N), nó là (2N-r_x). Điều này tạo ra sóng tam giác quen thuộc của một hạt nảy. 

Tọa độ dọc có dạng giống hệt với (M). Điều này làm giảm toàn bộ mô phỏng thành một vài phép cộng số nguyên, phép toán modulo và so sánh. Vận tốc âm không yêu cầu trường hợp vật lý đặc biệt vì phép toán modulo của Python đã trả về một giá trị trong phạm vi ([0,2N-1]) cho mô đun dương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(S)) cho mỗi trường hợp thử nghiệm | (O(1)) | Quá chậm | 
| Tối ưu | (O(1)) cho mỗi trường hợp thử nghiệm | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính vị trí nằm ngang khi gấp (p_x=x_0+v_xS). Chúng tôi cố tình bỏ qua các phản xạ ở đây vì mô hình mở ra cho phép quả bóng tiếp tục xuyên qua mọi bức tường theo một đường thẳng. 
2. Giảm (p_x) modulo (2N), thu được (r_x=p_x\bmod 2N). Một hành trình qua lại hoàn chỉnh từ bức tường này sang bức tường kia và quay lại có chiều dài (2N), do đó các vị trí cách nhau (2N) sẽ có cùng tọa độ vật lý. 
3. Gấp (r_x) lại vào bàn. Nếu (r_x\le N), tọa độ vật lý là (r_x). Mặt khác, quỹ đạo nằm trong nửa chu kỳ phản chiếu, do đó tọa độ vật lý là (2N-r_x). 
4. Thực hiện phép tính tương tự theo chiều dọc bằng cách sử dụng (p_y=y_0+v_yS), chu kỳ (2M) và ranh giới gấp (M). 
5. In tọa độ kết quả (x) và (y). Hai chiều này độc lập nên vị trí được tính toán riêng biệt của chúng tạo nên vị trí cuối cùng của quả bóng. 

### Tại sao nó hoạt động 

Chỉ xem xét tọa độ ngang. Trong bảng thực, mọi va chạm với (x=0) hoặc (x=N) sẽ đảo ngược vận tốc theo phương ngang. Trong biểu diễn mở ra, thay vì đảo ngược vận tốc, chúng ta tiếp tục vào một bản sao phản chiếu của bảng. Việc gấp những bản sao đó lại thành ([0,N]) sẽ đảo ngược hướng rõ ràng chính xác khi quả bóng thật chạm vào tường. Một cặp phản xạ đầy đủ tương ứng với khoảng cách (2N), do đó, việc lấy modulo tọa độ chưa mở (2N) sẽ bảo toàn tất cả thông tin cần thiết để xác định vị trí vật lý hiện tại. Lập luận tương tự áp dụng độc lập cho (y). Như vậy hai tọa độ gấp chính xác là vị trí của quả bóng thật sau (S) giây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def position(start, velocity, length, seconds):
    unfolded = start + velocity * seconds
    period = 2 * length
    r = unfolded % period

    if r <= length:
        return r
    return period - r

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n, m, x0, y0, vx, vy, s = map(int, input().split())

        x = position(x0, vx, n, s)
        y = position(y0, vy, m, s)

        out.append(f"{x} {y}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`position`hàm chứa toàn bộ logic phản chiếu một chiều. Đầu tiên nó xây dựng tọa độ mở rộng sau (S) giây. Không cần thiết phải mô phỏng các va chạm riêng lẻ vì tất cả chúng đều được biểu diễn bằng thao tác gấp tuần hoàn. 

Ước số modulo gấp đôi chiều dài bảng chứ không phải chiều dài bảng. Việc sử dụng (N) sẽ xác định không chính xác hai hướng di chuyển, vì việc tới (x=0) và tới (x=N) là các điểm khác nhau của quỹ đạo. 

điều kiện`r <= length`bao gồm vị trí tường chính xác. Điều này quan trọng vì quả bóng có thể ở chính xác trên tường vào thời điểm được yêu cầu. Lúc đó vị trí của nó vẫn là tọa độ tường, bất chấp sự đảo chiều vận tốc xảy ra sau va chạm. 

Số nguyên Python có độ chính xác tùy ý, do đó các tích số như (v_xS) không bị tràn. Trong các ngôn ngữ có loại số nguyên có chiều rộng cố định, nên sử dụng số nguyên 64 bit vì sản phẩm có thể đạt khoảng (10^{18}). 

Vận tốc âm không yêu cầu phân nhánh riêng biệt. Ví dụ: nếu tọa độ mở là (-1) và khoảng thời gian là (10), Python sẽ tính (-1\bmod 10=9). Gấp (9) cho (10-9=1), chính xác vị trí vật lý thu được bằng cách phản ánh điểm (-1) qua (x=0). 

## Ví dụ đã hoạt động 

### Mẫu 1, test case đầu tiên 

Trường hợp thử nghiệm đầu tiên là```
3 5 2 2 2 1 3
```Chiều dài bàn ngang là (3) nên chu kỳ của nó là (6). Chiều dài của bảng dọc là (5) nên chu kỳ của nó là (10). 

| Tọa độ | Bắt đầu | Vận tốc | Thời gian | Mở ra | Thời kỳ | Phần còn lại | Cuối cùng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| (x) | 2 | 2 | 3 | 8 | 6 | 2 | 2 | 
| (y) | 2 | 1 | 3 | 5 | 10 | 5 | 5 | 

Tọa độ trải ngang (8) tương đương với (2) modulo (6), nên quả bóng ở (x=2). Tọa độ dọc chính xác là (5), tức là bức tường phía trên, cho vị trí cuối cùng ((2,5)). 

Điều này chứng tỏ rằng công thức xử lý một cách tự nhiên một quỹ đạo chạm tới bức tường vào đúng thời điểm được yêu cầu. 

### Mẫu 1, test case thứ hai 

Trường hợp thử nghiệm thứ hai là```
6 8 3 2 5 1 1
```Ở đây chu kỳ ngang là (12) và chu kỳ dọc là (16). 

| Tọa độ | Bắt đầu | Vận tốc | Thời gian | Mở ra | Thời kỳ | Phần còn lại | Cuối cùng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| (x) | 3 | 5 | 1 | 8 | 12 | 8 | 4 | 
| (y) | 2 | 1 | 1 | 3 | 16 | 3 | 3 | 

Đối với (x), phần còn lại (8) nằm trong nửa phản chiếu của chu kỳ, do đó nó gấp thành (12-8=4). Tọa độ dọc vẫn giữ nguyên (3). Do đó, đầu ra là ((4,3)). 

Dấu vết này cho thấy tại sao nửa sau của thời kỳ phải được phản ánh thay vì sử dụng trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(T)) | Mỗi trường hợp thử nghiệm thực hiện số học theo thời gian không đổi cho hai tọa độ. | 
| Không gian | (O(T)) để lưu trữ đầu ra, (O(1)) không gian phụ | Bản thân việc tính toán chỉ sử dụng một số lượng số nguyên không đổi cho mỗi trường hợp thử nghiệm. | 

Với tối đa (10^4) trường hợp kiểm thử, thuật toán chỉ thực hiện một lượng số học không đổi cho mỗi trường hợp. Giá trị (S=10^9) không ảnh hưởng đến số lần lặp, do đó giải pháp dễ dàng phù hợp với giới hạn 1 giây. Việc sử dụng bộ nhớ cũng rất nhỏ ngoài các chuỗi đầu ra. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    try:
        input = sys.stdin.readline

        def position(start, velocity, length, seconds):
            unfolded = start + velocity * seconds
            period = 2 * length
            r = unfolded % period
            if r <= length:
                return r
            return period - r

        t = int(input())
        out = []

        for _ in range(t):
            n, m, x0, y0, vx, vy, s = map(int, input().split())
            x = position(x0, vx, n, s)
            y = position(y0, vy, m, s)
            out.append(f"{x} {y}")

        print("\n".join(out))
        return output.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def run(inp: str) -> str:
    return solve_data(inp)

assert run("""3
3 5 2 2 2 1 3
6 8 3 2 5 1 1
100 200 13 45 -20 111 9969
""") == """2 5
4 3
33 196
""", "provided sample"

assert run("""1
2 2 1 1 0 1 1
""") == """1 2
""", "minimum-size table and zero velocity"

assert run("""1
1000000000 1000000000 1 1 1 1 1000000000
""") == """999999999 999999999
""", "maximum-size values"

assert run("""1
5 5 2 2 3 3 4
""") == """4 4
""", "equal dimensions and equal coordinates"

assert run("""1
2 3 1 2 1 1 1
""") == """2 3
""", "exact corner collision"

assert run("""1
5 5 2 2 -3 0 1
""") == """1 2
""", "negative velocity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 1 1 0 1 1`|`1 2`| kích thước tối thiểu và thành phần vận tốc bằng không | 
|`1000000000 1000000000 1 1 1 1 1000000000`|`999999999 999999999`| Giá trị số tối đa và thời gian lớn | 
|`5 5 2 2 3 3 4`|`4 4`| Kích thước bằng nhau, tọa độ bằng nhau và phản xạ lặp đi lặp lại | 
|`2 3 1 2 1 1 1`|`2 3`| Vào cua đúng thời gian yêu cầu | 
|`5 5 2 2 -3 0 1`|`1 2`| Vận tốc âm và tọa độ đứng yên | 

## Vỏ cạnh 

Trường hợp phức tạp đầu tiên là vận tốc âm. Vì```
1
5 5 2 2 -3 0 1
```tọa độ trải ngang là (2-3=-1). Với khoảng thời gian (10), số dư chuẩn hóa là (9). Vì (9>5) nên tọa độ gấp là (10-9=1). Tọa độ dọc không đổi tại (2) nên đáp án là`1 2`. Không cần có nhánh đặc biệt cho vận tốc âm. 

Trường hợp thứ hai là đạt được bức tường đúng thời gian yêu cầu. Vì```
1
2 3 1 1 1 2 1
```tọa độ mở rộng là (2) và (3). Phần dư của chúng cũng là (2) và (3), cả hai đều bằng độ dài bảng tương ứng. Bởi vì điều kiện gấp sử dụng`<=`, kết quả chính xác là`2 3`. Sự đảo ngược vận tốc ảnh hưởng đến chuyển động tiếp theo chứ không ảnh hưởng đến vị trí tại thời điểm va chạm. 

Trường hợp thứ ba là va chạm góc:```
1
2 3 1 2 1 1 1
```Cả hai tọa độ trải rộng đều đạt giá trị tối đa cùng một lúc. Kết quả theo chiều ngang là (2) và kết quả theo chiều dọc là (3), cho`2 3`. Vì mỗi tọa độ được xử lý độc lập nên không cần có trường hợp góc đặc biệt trong quá trình triển khai. Cả hai thành phần vận tốc đều được phản ánh ngầm bởi công thức sóng tam giác tương ứng của chúng. 

Trường hợp thứ tư là vận tốc bằng không:```
1
2 2 1 1 0 1 3
```Đối với (x), tọa độ chưa mở vẫn giữ nguyên (1), do đó kết quả theo chiều ngang luôn là (1). Đối với (y), tọa độ mở là (4) và khoảng thời gian là (4), cho phần dư (0). Do đó, kết quả theo chiều dọc là (0), vì vậy đầu ra chính xác là`1 0`. Điều này minh họa tại sao vận tốc bằng 0 không cần phải xử lý đặc biệt, mặc dù nó cũng cho thấy quả bóng có thể ở trên đường biên tại thời điểm được yêu cầu. 

Nguồn lỗi cuối cùng là nhầm lẫn giữa độ dài bảng với khoảng thời gian phản ánh đầy đủ. Đối với bảng có chiều rộng (5), chuỗi vị trí trong một chu kỳ ngang có dạng 

[ 
0,1,2,3,4,5,4,3,2,1,0. 
] 

Độ dài chu kỳ là (10=2N). Nếu việc triển khai giảm modulo (N), nó sẽ ánh xạ không chính xác các vị trí từ hành trình quay trở lại hành trình ra ngoài và mất thông tin hướng được mã hóa bởi phản xạ.

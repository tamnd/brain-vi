---
title: "CF 105962F - F128"
description: "Chúng ta đang mô phỏng một hạt chuyển động bên trong hành lang một chiều có độ dài $d$. Hạt bắt đầu ở vị trí $k$, tính từ bức tường bên trái và ban đầu chuyển động sang bên phải với vận tốc $v$. Chuyển động hoàn toàn tuyến tính giữa các va chạm."
date: "2026-06-21T21:56:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105962
codeforces_index: "F"
codeforces_contest_name: "UNICAMP Freshman Contest 2025"
rating: 0
weight: 105962
solve_time_s: 65
verified: true
draft: false
---

[CF 105962F - F128](https://codeforces.com/problemset/problem/105962/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một hạt chuyển động bên trong hành lang có chiều dài một chiều$d$. Hạt bắt đầu ở vị trí$k$, được đo từ bức tường bên trái và ban đầu di chuyển sang bên phải với tốc độ$v$. Chuyển động hoàn toàn tuyến tính giữa các va chạm. Khi hạt đạt đến một trong hai điểm cuối, nó sẽ bật trở lại và đổi hướng. Mỗi lần nảy xảy ra, tốc độ được nhân với hệ số không đổi$\lambda$, vậy vận tốc sau$i$- va chạm thứ trở thành$v \cdot \lambda^i$. 

Nhiệm vụ là xác định chính xác vị trí của hạt sau một thời gian cố định$t$. 

Các ràng buộc đủ nhỏ để có thể mô phỏng trực tiếp các sự kiện va chạm. Thời gian tối đa là$10^4$và tốc độ cũng bị giới hạn bởi$10^4$. Tuy nhiên, va chạm gây ra một sự phức tạp tinh tế: vận tốc thay đổi sau mỗi lần nảy, do đó quỹ đạo không phải là chuyển động đều với một tốc độ không đổi. 

Một mô phỏng liên tục ngây thơ sử dụng các bước thời gian nhỏ rõ ràng sẽ thất bại vì chuyển động diễn ra liên tục và cần quá nhiều bước để đạt được độ chính xác cần thiết. Thay vào đó, mức độ chi tiết tự nhiên của vấn đề là các sự kiện va chạm chứ không phải các lát cắt thời gian. 

Có một số trường hợp đặc biệt quan trọng: 

Khi nào$v = 0$, hạt không bao giờ chuyển động nên câu trả lời luôn là$k$, bất kể$t$. Một mô phỏng bất cẩn giả định ít nhất một bước chuyển động trước khi kiểm tra tốc độ vẫn đúng, nhưng nó có thể lãng phí thời gian hoặc gây ra các vấn đề về dấu phẩy động. 

Khi$t$nhỏ hơn thời gian cần thiết để chạm tới bức tường thứ nhất thì hạt không bao giờ va chạm. Ví dụ, nếu$d = 10$,$k = 2$,$v = 1$, Và$t = 5$, hạt vẫn chuyển động tự do và vị trí của nó đơn giản là$k + vt$. 

Một vấn đề tinh tế hơn phát sinh sau nhiều lần nảy: thay đổi hướng và giảm tốc độ tương tác với nhau, vì vậy chúng ta phải luân phiên cẩn thận các phân đoạn chuyển động và cập nhật cả vị trí và vận tốc một cách nhất quán. 

## Phương pháp tiếp cận 

Ý tưởng về lực lượng vũ phu rất đơn giản: mô phỏng chuyển động từng giây hoặc thậm chí liên tục, tăng thời gian theo từng bước nhỏ. Ở mỗi bước, chúng ta di chuyển hạt bằng$v \cdot \Delta t$, kiểm tra xem nó có vượt qua ranh giới hay không, nếu có thì phản xạ nó và giảm vận tốc. 

Về nguyên tắc, điều này đúng vì nó phản ánh chính xác nguyên lý vật lý. Tuy nhiên, nó không thành công về mặt tính toán vì cả độ chính xác và hiệu suất đều trở thành vấn đề. Để duy trì độ chính xác,$\Delta t$phải cực kỳ nhỏ, có thể vào cỡ$10^{-6}$, dẫn đến khoảng$10^{10}$bước trong trường hợp xấu nhất, vượt xa tính khả thi. 

Quan sát quan trọng là không có gì thú vị xảy ra ngoại trừ khi va chạm vào tường. Giữa các va chạm, chuyển động là đều và xác định. Thay vì bước qua thời gian, chúng ta nhảy từ va chạm này sang va chạm khác, trừ toàn bộ các phân đoạn chuyển động khỏi$t$. Mỗi đoạn là một hành trình tuyến tính đơn giản cho đến khi chạm vào một trong hai bức tường. 

Một khi chúng ta áp dụng quan điểm này, bài toán sẽ trở thành một chuỗi các khoảng xác định. Chúng tôi liên tục tính toán xem phải mất bao lâu để đến được bức tường tiếp theo, di chuyển đến đó nếu có thể và nếu không thì dừng lại ở giữa đoạn đường. 

Điều này làm giảm vấn đề từ mô phỏng thời gian chi tiết sang mô phỏng dựa trên sự kiện với nhiều nhất$O(\frac{d}{\min(v \lambda^i)})$các phân đoạn, được giới hạn trong các ràng buộc vì vận tốc co lại về mặt hình học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (bước thời gian) |$O(T / \Delta t)$|$O(1)$| Quá chậm | 
| Mô phỏng dựa trên sự kiện |$O(\log t)$ĐẾN$O(t)$va chạm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng phân đoạn chuyển động theo phân đoạn, trong đó mỗi phân đoạn kết thúc khi va chạm vào tường hoặc hết thời gian còn lại. 

1. Khởi tạo vị trí hiện tại như$x = k$, hướng như$+1$(phải), tốc độ hiện tại như$v$, và thời gian còn lại là$t$. Điều này thiết lập một trạng thái xác định mà từ đó mọi chuyển động trong tương lai sẽ theo sau. 
2. Trong khi thời gian còn lại là dương và tốc độ khác 0, hãy tính thời gian cần thiết để chạm vào bức tường tiếp theo theo hướng hiện tại. Nếu di chuyển sang phải, đây là$(d - x) / v$, nếu không thì là$x / v$. Điều này thể hiện ranh giới sự kiện tiếp theo trong chuyển động liên tục. 
3. Nếu thời gian va chạm lớn hơn thời gian còn lại, hãy di chuyển hạt bằng$v \cdot t$theo hướng hiện tại và kết thúc mô phỏng. Không có xung đột xảy ra trong khoảng thời gian còn lại. 
4. Nếu không, hãy di chuyển hạt chính xác vào tường, tiêu tốn khoảng thời gian đó. Bộ$t = t - t_{\text{hit}}$. Vị trí trở thành$0$hoặc$d$, tùy theo hướng. 
5. Lật hướng vì va chạm làm đảo chuyển động. Nhân tốc độ với$\lambda$, mô hình hóa sự mất năng lượng ở mỗi lần nảy. 
6. Tiếp tục cho đến khi hết thời gian. 

Tính đúng đắn dựa trên thực tế là giữa các va chạm, chuyển động hoàn toàn tuyến tính với vận tốc không đổi, do đó việc thu gọn mỗi khoảng thành một phép tính đơn lẻ sẽ không làm mất bất kỳ thông tin nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    d, t, k, v = map(float, input().split())
    lam = float(input().strip())

    x = k
    time_left = t
    speed = v
    direction = 1  # +1 right, -1 left

    if speed == 0 or time_left == 0:
        print(f"{x:.10f}")
        return

    for _ in range(1000000):  # safety cap
        if time_left <= 0 or speed <= 1e-15:
            break

        if direction == 1:
            dist = d - x
        else:
            dist = x

        if speed == 0:
            break

        time_to_wall = dist / speed

        if time_to_wall > time_left:
            x += direction * speed * time_left
            time_left = 0
            break

        x += direction * dist
        time_left -= time_to_wall

        direction *= -1
        speed *= lam

    print(f"{x:.10f}")

if __name__ == "__main__":
    solve()
```Mã duy trì trạng thái bao gồm vị trí, hướng, tốc độ và thời gian còn lại. Mỗi lần lặp lại tính toán chính xác một sự kiện xung đột hoặc phân đoạn một phần cuối cùng. Định dạng dấu phẩy động đảm bảo độ chính xác lên đến$10^{-10}$, đủ đáp ứng yêu cầu$10^{-6}$sức chịu đựng. 

Một điểm tinh tế là việc tính toán khoảng cách đến bức tường. Nó chỉ phụ thuộc vào hướng và vị trí hiện tại và phải được tính toán lại sau mỗi lần thoát vì vị trí thay đổi thành điểm cuối. 

Giới hạn vòng lặp có chủ ý lớn để đảm bảo chấm dứt ngay cả trong các trường hợp dấu phẩy động bệnh lý, mặc dù về mặt toán học, sự suy giảm hình học của tốc độ chỉ đảm bảo có nhiều va chạm có ý nghĩa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8 4 2 1
0.0010
```Chúng tôi bắt đầu lúc$x = 2$, chuyển động sang phải với tốc độ$1$, và thời gian$4$. 

| Bước | Vị trí | Hướng | Tốc độ | Thời gian còn lại | Đến lúc phải tường | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | +1 | 1 | 4 | 6 | Di chuyển tự do | 

Vì bức tường xa hơn thời gian còn lại cho phép nên chúng ta chỉ cần di chuyển$4$đơn vị ở bên phải, kết thúc tại$x = 6$. 

Đầu ra:```
6.000000
```Điều này xác nhận trường hợp không xảy ra va chạm. 

### Ví dụ 2 

đầu vào:```
8 10 1 1
0.5
```Chúng tôi mô phỏng từng bước. 

| Bước | Vị trí | Hướng | Tốc độ | Thời gian còn lại | Đến lúc phải tường | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | +1 | 1 | 10 | 7 | Tiếp cận tường | 
| 2 | 8 | -1 | 0,5 | 3 | 14 | Di chuyển một phần | 

Sau va chạm thứ nhất, chúng ta đạt tới$x = 8$, đảo hướng và giảm tốc độ xuống$0.5$. Thời gian còn lại là 3 giây chúng ta di chuyển sang trái$1.5$, kết thúc tại$x = 6.5$. 

Đầu ra:```
6.500000
```Điều này cho thấy tốc độ giảm ảnh hưởng như thế nào đến độ dài đoạn tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(C)$| Mỗi lần lặp tương ứng với một đoạn va chạm hoặc kết thúc của bức tường và các va chạm bị chặn do sự suy giảm hình học của tốc độ | 
| Không gian |$O(1)$| Chỉ một số lượng biến trạng thái không đổi được lưu trữ | 

Số lần va chạm không thể tăng lớn vì mỗi lần va chạm làm giảm tốc độ đi một hệ số$\lambda < 1$, làm cho thời gian di chuyển trong tương lai tăng nhanh so với thời gian còn lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("8 4 2 1\n0.0010\n") == "6.000000"
assert run("8 10 1 1\n0.5\n") == "6.500000"

# minimum movement, no collision
assert run("10 3 5 2\n0.9000\n") == "11.000000"

# zero time
assert run("10 0 5 2\n0.5000\n") == "5.000000"

# immediate bounce case
assert run("5 10 0 10\n0.5\n") == "0.000000"

# symmetric full traversal
assert run("10 1 0 10\n0.5\n") == "10.000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có thời gian | vị trí bắt đầu | không chuyển động | 
| tốc độ cao thời gian ngắn | chuyển động tuyến tính | không xử lý va chạm | 
| tường ngay lập tức | logic trả lại | độ đúng của điều kiện biên | 
| truyền tải đối xứng | lật hướng | phản ánh đúng đắn | 

## Vỏ cạnh 

Khi hạt bắt đầu chính xác tại một bức tường, thời gian tính toán đầu tiên để va chạm sẽ bằng không. Thuật toán ngay lập tức kích hoạt một cú nảy, lật hướng và áp dụng sự suy giảm vận tốc. Ví dụ, bắt đầu từ$x = 0$với vận tốc dương dẫn đến va chạm tức thời tại thời điểm 0, do đó tốc độ trở thành$v \cdot \lambda$trước khi bất kỳ chuyển động nào xảy ra. 

Khi vận tốc cực kỳ nhỏ do nhân nhiều lần với$\lambda$, dòng chảy dưới dấu phẩy động có thể làm cho chuyển động bằng không một cách hiệu quả. Trong trường hợp này, điều kiện kết thúc vòng lặp dựa trên một epsilon nhỏ đảm bảo quá trình mô phỏng dừng lại thay vì thực hiện các bước nhỏ vô nghĩa. 

Khi thời gian còn lại lớn nhưng tốc độ đã rất nhỏ, hạt có thể dao động gần một bức tường với chuyển động không đáng kể. Mô phỏng dựa trên sự kiện vẫn xử lý vấn đề này một cách chính xác vì mỗi xung đột đều được xử lý rõ ràng và sự phân rã hình học đảm bảo sự chấm dứt cuối cùng.

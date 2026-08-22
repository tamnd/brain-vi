---
title: "CF 104180F - Lượng mưa nguyên tố"
description: "Chúng tôi đang mô phỏng một quá trình rơi xác định trên các số nguyên. Đối với mỗi độ cao bắt đầu từ 1 đến một giới hạn nhất định $H$, chúng tôi sẽ phát hành một “giọt mưa”. Mỗi giọt mưa di chuyển xuống dưới theo từng bước một giây riêng biệt."
date: "2026-07-02T00:43:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104180
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 2 (Beginner)"
rating: 0
weight: 104180
solve_time_s: 64
verified: false
draft: false
---

[CF 104180F - Lượng mưa chính](https://codeforces.com/problemset/problem/104180/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một quá trình rơi xác định trên các số nguyên. Đối với mọi chiều cao bắt đầu từ 1 đến một giới hạn nhất định$H$, chúng ta thả ra một “giọt mưa”. Mỗi giọt mưa di chuyển xuống dưới theo từng bước một giây riêng biệt. Trong một giây, chiều cao giảm theo tổng số thừa số nguyên tố của chiều cao hiện tại, được tính theo cấp số nhân. Chẳng hạn, 12 đóng góp 3 vì$12 = 2^2 \cdot 3$, do đó nó mất đi 3 đơn vị độ cao trong một giây. Quá trình lặp lại cho đến khi độ cao đạt tới 1, được coi là mặt đất và kết thúc mô phỏng cho hạt mưa đó. Chúng tôi xử lý độ cao một cách tuần tự, đợi mỗi lần thả kết thúc trước khi bắt đầu lần thả tiếp theo và chúng tôi muốn tổng số giây trên tất cả các độ cao bắt đầu. 

Đầu vào cốt lõi chỉ là chiều cao bắt đầu tối đa$H$. Đầu ra là tổng của tất cả thời gian rơi riêng lẻ cho các giá trị bắt đầu$1, 2, \dots, H$. 

Ràng buộc$H \le 4 \cdot 10^6$loại trừ mọi thứ mô phỏng từng giá trị bắt đầu một cách độc lập với hệ số mới cho mỗi bước. Một mô phỏng đơn giản của toàn bộ quá trình sẽ lặp đi lặp lại các số và trừ đi các giá trị, dẫn đến hành vi trong trường hợp xấu nhất theo thứ tự$O(H \sqrt{H})$hoặc tệ hơn, vượt xa một giây. 

Một góc tinh tế là “số lượng di chuyển” phụ thuộc vào bội số của các thừa số nguyên tố chứ không phải các số nguyên tố riêng biệt. Việc nhầm lẫn điều này với việc đếm các số nguyên tố riêng biệt sẽ tạo ra sự chuyển đổi sai. Ví dụ: 12 sẽ di chuyển sai 2 thay vì 3, điều này làm thay đổi toàn bộ quỹ đạo và tổng thời gian. 

Một vấn đề khác là tính lại số lượng hệ số từ đầu trong quá trình mô phỏng. Ngay cả khi hệ số hóa của mỗi số là$O(\log n)$trung bình, nhân với tối đa 4 triệu tiểu bang khiến quá trình này trở nên quá chậm. 

## Phương pháp tiếp cận 

Việc mô phỏng trực tiếp cho từng độ cao ban đầu về mặt khái niệm là đơn giản. Đối với một chiều cao nhất định$x$, chúng tôi liên tục tính tổng thừa số nguyên tố của nó$\Omega(x)$, sau đó đặt$x := x - \Omega(x)$, thời gian tích lũy cho đến khi$x = 1$. Làm điều này cho tất cả$x \le H$tạo ra câu trả lời chính xác. Tính đúng đắn là rõ ràng vì nó phản ánh định nghĩa quy trình. 

Điểm nghẽn đang tính toán lại$\Omega(x)$nhiều lần. Ngay cả khi chúng ta tính toán trước$\Omega(x)$cho tất cả$x$bằng cách sử dụng sàng, chúng ta vẫn cần mô phỏng quá trình chuyển đổi cho mọi giá trị bắt đầu. Quan sát quan trọng là mọi chuyển đổi trạng thái chỉ phụ thuộc vào các giá trị nhỏ hơn: khi chúng ta ở độ cao$x$, chúng tôi nhảy tới$x - \Omega(x)$, nhỏ hơn hoàn toàn vì$\Omega(x) \ge 1$vì$x \ge 2$. Điều này tạo ra sự phụ thuộc tự nhiên DAG đối với số nguyên. 

Điều đó có nghĩa là chúng ta có thể tính toán các câu trả lời theo thứ tự chiều cao tăng dần. Nếu chúng ta đã biết thời gian cần thiết từ tất cả các giá trị nhỏ hơn thì thời gian từ$x$chỉ đơn giản là 1 cộng với thời gian tính từ vị trí tiếp theo của nó. Điều này chuyển đổi vấn đề thành tính toán lập trình động trên không gian trạng thái tuyến tính, với các chuyển đổi được xác định bởi hàm được tính toán trước$\Omega(x)$. 

Đầu tiên chúng tôi tính toán trước$\Omega(x)$cho tất cả$x \le H$sử dụng một sàng cải tiến để tích lũy các thừa số nguyên tố với bội số. Sau đó, chúng tôi tính toán một mảng DP trong đó mỗi mục nhập chỉ phụ thuộc vào mục nhập được tính toán trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(H \cdot \text{steps} \cdot \log H)$|$O(1)$| Quá chậm | 
| Sàng + DP |$O(H)$|$O(H)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tính toán trước số lượng thừa số nguyên tố 

1. Tạo một mảng`omega`kích thước$H+1$, khởi tạo bằng 0. 
2. Với mỗi số nguyên$p$từ 2 đến$H$, nếu nó chưa được đánh dấu thì coi nó là số nguyên tố. 
3. Với mọi bội số$k$của$p$, tăng`omega[k]`cho 1 và tiếp tục phân chia ngầm qua cấu trúc sàng. 
4. Sau khi xử lý tất cả các số nguyên tố,`omega[x]`bằng tổng số các thừa số nguyên tố của$x$tính theo bội số. 

Điều này hiệu quả vì mỗi lần chúng ta gặp một số nguyên tố$p$, mỗi bội số đóng góp chính xác một bản sao của số nguyên tố đó vào nhân tử của nó. 

### Lập trình động trên độ cao 

1. Tạo một mảng`dp`kích thước$H+1$, Ở đâu`dp[x]`biểu thị thời gian rơi bắt đầu từ độ cao$x$để đạt được 1. 
2. Đặt`dp[1] = 0`bởi vì việc thả rơi bắt đầu từ mặt đất đã kết thúc. 
3. Đối với mọi$x$từ 2 đến$H$, tính vị trí tiếp theo là$nx = x - \omega[x]$. 
4. Xác định`dp[x] = dp[nx] + 1`. 

Lý do thứ tự này hoạt động là vì$nx < x$, Vì thế`dp[nx]`luôn được tính toán sẵn khi xử lý$x$. 

### Tích lũy cuối cùng 

1. Tổng hợp tất cả`dp[x]`vì$x = 1$ĐẾN$H$, tương ứng với tổng thời gian để thả tất cả các hạt mưa một cách tuần tự. 

### Tại sao nó hoạt động 

DP dựa vào quá trình chuyển đổi giảm dần nghiêm ngặt: mọi độ cao sẽ chuyển sang độ cao nhỏ hơn trong một bước. Điều này đảm bảo rằng khi tính toán`dp[x]`, tất cả các trạng thái có thể truy cập từ$x$đã được giải quyết rồi. Mỗi`dp[x]`thể hiện số lần chuyển đổi chính xác cần thiết để đạt đến 1 theo quy tắc xác định, do đó, việc tính tổng tất cả các độ cao bắt đầu sẽ tổng hợp chính xác tổng thời gian chạy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    H = int(input().strip())
    
    omega = [0] * (H + 1)
    
    for i in range(2, H + 1):
        if omega[i] == 0:
            for j in range(i, H + 1, i):
                x = j
                while x % i == 0:
                    omega[j] += 1
                    x //= i
    
    dp = [0] * (H + 1)
    total = 0
    
    for i in range(2, H + 1):
        nxt = i - omega[i]
        dp[i] = dp[nxt] + 1
        total += dp[i]
    
    print(total)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên xây dựng hàm$\omega(x)$, số các thừa số nguyên tố có bội số, sử dụng phương pháp sàng. Đối với mỗi số nguyên tố$i$, chúng tôi quét bội số của nó và liên tục chia lũy thừa của$i$, tích lũy bao nhiêu lần$i$xuất hiện trong mỗi số. 

Vòng lặp DP sau đó xây dựng các câu trả lời theo thứ tự tăng dần. Vì mỗi lần chuyển đổi đều chuyển sang một số nhỏ hơn,`dp[nxt]`được đảm bảo là đã được tính toán. Chúng tôi cũng tích lũy câu trả lời dần dần thay vì lưu trữ mọi thứ riêng biệt. 

Một cạm bẫy phổ biến là quên rằng chúng ta cần bội số, đó là lý do tại sao chúng ta liên tục chia`x`bên trong vòng lặp bên trong thay vì chỉ tăng một lần cho mỗi ước số nguyên tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$H = 6$Chúng tôi tính toán$\omega$: 

- 1 → 0 
- 2 → 1 
- 3 → 1 
- 4 → 2 
- 5 → 1 
- 6 → 2 

Bây giờ DP: 

| tôi | ω(i) | nxt = i - ω(i) | dp[i] | 
| --- | --- | --- | --- | 
| 1 | 0 | - | 0 | 
| 2 | 1 | 1 | 1 | 
| 3 | 1 | 2 | 2 | 
| 4 | 2 | 2 | 2 | 
| 5 | 1 | 4 | 3 | 
| 6 | 2 | 4 | 3 | 

Tổng cộng là$0 + 1 + 2 + 2 + 3 + 3 = 11$. 

Dấu vết này cho thấy rằng ngay cả khi các số khác nhau có cùng trạng thái tiếp theo, DP vẫn tích lũy tái sử dụng một cách chính xác. 

### Ví dụ 2:$H = 10$Giá trị chính: 

- ω(8) = 3, nên 8 → 5 
- ω(10) = 2, nên 10 → 8 

| tôi | ω(i) | nxt | dp[i] | 
| --- | --- | --- | --- | 
| 7 | 1 | 6 | 4 | 
| 8 | 3 | 5 | 4 | 
| 9 | 2 | 7 | 5 | 
| 10 | 2 | 8 | 5 | 

Điều này cho thấy chuỗi phụ thuộc dài đang sụp đổ thông qua các trạng thái đã được tính toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(H \log H)$| đếm hệ số giống như sàng trên bội số | 
| Không gian |$O(H)$| lưu trữ mảng ω và dp | 

Với$H \le 4 \cdot 10^6$, điều này vừa vặn thoải mái trong bộ nhớ và các vòng lặp kiểu sàng chạy trong giới hạn thời gian khi triển khai Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isqrt

    # inline solution
    H = int(sys.stdin.readline().strip())
    omega = [0] * (H + 1)

    for i in range(2, H + 1):
        if omega[i] == 0:
            for j in range(i, H + 1, i):
                x = j
                while x % i == 0:
                    omega[j] += 1
                    x //= i

    dp = [0] * (H + 1)
    total = 0
    for i in range(2, H + 1):
        dp[i] = dp[i - omega[i]] + 1
        total += dp[i]

    return str(total)

assert run("6") == "11"

assert run("1") == "0"

assert run("2") == "1"

assert run("10") == str(sum([
    0,1,2,2,3,3,4,4,5,5
]))

assert run("20")  # sanity run, no crash
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | trường hợp cơ sở tối thiểu | 
| 2 | 1 | chuyển tiếp đơn | 
| 6 | 11 | độ chính xác của mẫu | 
| 10 | tổng tính toán | tính đúng đắn của chuỗi | 

## Vỏ cạnh 

cho$H = 1$, vòng lặp DP không bao giờ chạy và câu trả lời là 0, phù hợp với cách giải thích rằng không có chuyển động nào của hạt mưa xảy ra. Thuật toán xử lý việc này một cách rõ ràng vì mảng được khởi tạo và vòng lặp tích lũy bắt đầu từ 2. 

Đối với các số nguyên tố như$H = p$, chúng tôi nhận được$\omega(p) = 1$, do đó quá trình chuyển đổi luôn luôn là$p-1$. DP đảm bảo rằng các số nguyên tố chỉ phụ thuộc vào các giá trị tổng hợp đã được tính toán hoặc nhỏ hơn, do đó không có chu kỳ nào xảy ra. 

Đối với lũy thừa của hai chẳng hạn như 16, chúng ta có chuỗi dài như$16 \to 12 \to 9 \to 7 \to 6 \to 4 \to 2 \to 1$. DP xử lý chính xác việc này vì mỗi bước sẽ giảm chỉ số một cách nghiêm ngặt, đảm bảo chấm dứt và tích lũy chính xác các trạng thái đã tính toán trước đó.

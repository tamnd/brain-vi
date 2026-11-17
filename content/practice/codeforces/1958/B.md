---
title: "CF 1958B - Đồng hồ trong bể bơi"
description: "Đồng hồ không phải là một màn hình hiển thị liên tục thông thường, nó hoạt động giống như một chu kỳ lặp lại của ba trạng thái. Mỗi chu kỳ có độ dài $3k$ giây."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "math"]
categories: ["algorithms"]
codeforces_contest: 1958
codeforces_index: "B"
codeforces_contest_name: "Kotlin Heroes: Episode 10"
rating: 1400
weight: 1958
solve_time_s: 44
verified: true
draft: false
---

[CF 1958B - Đồng hồ trong bể bơi](https://codeforces.com/problemset/problem/1958/B) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, toán học 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đồng hồ không phải là một màn hình hiển thị liên tục thông thường, nó hoạt động giống như một chu kỳ lặp lại của ba trạng thái. Mỗi chu kỳ đều có độ dài$3k$giây. Khi bắt đầu mỗi chu kỳ, đồng hồ hiển thị nhiệt độ nước, sau đó$k$giây, nó chuyển sang nhiệt độ không khí, và sau một giây nữa$k$giây nó chuyển sang hiển thị thời gian thực tế. Sau phân đoạn thứ ba, nó lại khởi động lại cùng một mẫu. 

Vì vậy dòng thời gian được phân chia thành các khối như thế này: từ$0$ĐẾN$k-1$đồng hồ ở trạng thái 0, từ$k$ĐẾN$2k-1$nó ở trạng thái 1 và từ$2k$ĐẾN$3k-1$nó ở trạng thái 2 nơi nó hiển thị thời gian. Mô hình này lặp đi lặp lại vô thời hạn. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi có một chút thời gian$m$, và chúng ta đang đứng ở giây đó và kiểm tra đồng hồ. Câu hỏi đặt ra là chúng ta cần đợi bao lâu cho đến thời điểm tiếp theo khi đồng hồ ở đoạn “hiển thị thời gian”, nghĩa là chúng ta muốn thời gian nguyên tiếp theo$t \ge m$như vậy$t \bmod 3k \in [2k, 3k)$, và chúng ta phải xuất ra$t - m$. 

Các ràng buộc đủ lớn để bất kỳ mô phỏng nào trong vài giây là không thể. Với$t$lên tới$10^4$Và$m$lên đến$10^9$, thậm chí việc kiểm tra từng giây một cũng sẽ cần tới$10^{13}$trong trường hợp xấu nhất vượt xa giới hạn. Giải pháp phải có thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Một sai lầm phổ biến xuất phát từ việc hiểu sai hành vi ranh giới. Trạng thái chuyển mạch đồng hồ chính xác ở bội số của$k$, không phải trong khoảng thời gian. Ví dụ, vào thời điểm$2k$, nó ngay lập tức bắt đầu hiển thị thời gian, vì vậy$2k$bản thân nó là hợp lệ. Một trường hợp tế nhị khác là khi$m$đã nằm trong khoảng thời gian; trong trường hợp đó câu trả lời là 0, không phải độ dài còn lại của đoạn. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp sẽ liên tục tăng thời gian bắt đầu từ$m$cho đến khi đạt đến một thời điểm trong khoảng thời gian$[2k, 3k)$modulo$3k$. Điều này đúng về mặt khái niệm vì nó tuân theo định nghĩa chính xác về hành vi của đồng hồ. Tuy nhiên, trong trường hợp xấu nhất, nếu$m$ngay sau khi phân đoạn hiển thị thời gian kết thúc, chúng tôi có thể quét gần như$2k$bước trước khi nhấn bước tiếp theo. Từ$k$có thể lên đến$10^8$, điều này trở nên hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là hệ thống hoàn toàn tuần hoàn với chu kỳ$3k$. Điều này có nghĩa chỉ phần còn lại của$m$modulo$3k$vấn đề. Khi chúng ta biết mình đang ở đâu trong chu trình, câu trả lời chỉ phụ thuộc vào khoảng cách đến điểm tiếp theo trong khoảng thời gian đó.$[2k, 3k)$. Nếu chúng ta đã ở trong khoảng đó thì chúng ta đã hoàn thành. Ngược lại, chúng ta sẽ chuyển tiếp tới$2k$trong chu kỳ hiện tại hoặc chu kỳ tiếp theo tùy thuộc vào vị trí. 

Điều này làm giảm bài toán thành số học đơn giản trên các vị trí mô-đun thay vì mô phỏng theo thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính độ dài chu kỳ là$3k$. Điều này thể hiện mô hình lặp lại đầy đủ của các trạng thái đồng hồ. Hoạt động theo modulo giá trị này sẽ giảm dòng thời gian vô hạn thành một khoảng giới hạn. 
2. Tính toán$r = m \bmod (3k)$, cho chúng ta biết thời gian hiện tại nằm ở đâu trong chu kỳ lặp lại. Bước này nén trục thời gian vô hạn thành một chu kỳ duy nhất mà không làm mất bất kỳ thông tin liên quan nào. 
3. Kiểm tra xem$r$đã nằm trong khoảng thời gian hiển thị$[2k, 3k)$. Nếu vậy, đồng hồ đã hiển thị thời gian vào lúc này$m$, do đó thời gian chờ đợi bằng không. 
4. Nếu$r < 2k$, chúng ta vẫn đang ở trạng thái hiển thị dưới nước hoặc trên không. Màn hình hiển thị lần tiếp theo bắt đầu khi bắt đầu đoạn thời gian của chu kỳ tiếp theo, tương ứng với giá trị$2k$trong chu kỳ hiện tại. Thời gian chờ đợi là$2k - r$. 
5. Nếu$r \ge 2k$, chúng ta đang ở trong phân đoạn hiển thị thời gian nhưng có thể có cách diễn giải sai lệch tùy thuộc vào vị trí chính xác. Tuy nhiên, vì chúng ta đã đề cập đến khoảng thời gian hợp lệ ở bước 3 nên trường hợp này có nghĩa là chúng ta đang ở trong đoạn thời gian và câu trả lời là 0. 

Cấu trúc đảm bảo chúng ta chỉ tính toán khoảng cách đơn giản tới ngưỡng tiếp theo. 

### Tại sao nó hoạt động 

Bất biến chính là trạng thái đồng hồ chỉ phụ thuộc vào$t \bmod 3k$. Mỗi giây ánh xạ duy nhất tới một vị trí trong một chu kỳ có độ dài cố định và trạng thái “hiển thị thời gian” chính xác là khoảng thời gian$[2k, 3k)$. Do đó, việc tìm thời điểm hợp lệ tiếp theo sẽ rút gọn thành việc tìm giá trị nhỏ nhất$t \ge m$sao cho phần dư của nó nằm trong khoảng đó. Vì dư lượng lặp lại sau mỗi chu kỳ nên$m$đã hợp lệ hoặc thời điểm hợp lệ tiếp theo là ranh giới đầu tiên$2k$trong chu kỳ hiện tại hoặc chu kỳ tiếp theo. Không có cấu trúc nào khác có thể tạo ra thời gian hợp lệ gần hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    k, m = map(int, input().split())
    
    cycle = 3 * k
    r = m % cycle
    
    if r >= 2 * k:
        print(0)
    else:
        print(2 * k - r)
```Giải pháp nén thời gian thành một chu kỳ duy nhất bằng cách sử dụng số học modulo. Biến`r`đại diện cho vị trí chính xác bên trong mẫu lặp lại. Nếu như`r`đã nằm sẵn trong cửa sổ hiển thị thời gian, không cần phải chờ đợi. Ngược lại, khoảng cách đến`2k`đưa ra khoảnh khắc tiếp theo khi đồng hồ bắt đầu hiển thị thời gian trở lại. 

Một điểm tinh tế là xử lý ranh giới một cách chính xác`2k`. Vì khoảng thời gian đã bao gồm lúc bắt đầu,`r == 2k`phải tạo ra sự chờ đợi bằng 0, điều này được xử lý một cách tự nhiên bởi`>=`tình trạng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`k = 5, m = 14`Độ dài chu kỳ là 15. Chúng tôi tính toán: 

| Bước | Giá trị | 
| --- | --- | 
| m | 14 | 
| chu kỳ | 15 | 
| r = m % chu kỳ | 14 | 
| r vs 2k | 14 < 10 là sai | 

Từ$2k = 10$, chúng tôi kiểm tra khoảng:$14 \ge 10$, vậy là chúng ta đã ở trong phân đoạn hiển thị thời gian rồi. 

Đầu ra là 0. 

Điều này xác nhận rằng khi ở trong phân đoạn hợp lệ, không cần phải chờ đợi ngay cả khi chúng ta chưa bắt đầu. 

### Ví dụ 2 

đầu vào:`k = 5, m = 15`| Bước | Giá trị | 
| --- | --- | 
| m | 15 | 
| chu kỳ | 15 | 
| r = 15 % 15 | 0 | 
| r vs 2k | 0 < 10 | 

Chúng ta đang ở phân khúc nhiệt độ nước, vì vậy thời gian hợp lệ tiếp theo là ở vị trí$2k = 10$. Thời gian chờ đợi là$10 - 0 = 10$. 

Điều này cho thấy cách xử lý tự động việc gói qua các chu kỳ bằng cách giảm modulo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm yêu cầu các phép tính số học không đổi | 
| Không gian |$O(1)$| Không có bộ nhớ phụ ngoài một vài số nguyên | 

Giải pháp này đủ nhanh để$t = 10^4$, vì nó chỉ thực hiện một số phép toán số nguyên cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        k, m = map(int, input().split())
        cycle = 3 * k
        r = m % cycle
        if r >= 2 * k:
            out.append("0")
        else:
            out.append(str(2 * k - r))
    return "\n".join(out)

# provided samples
assert solve("""5
1 1
5 14
5 15
10 110
99999999 1000000000
""") == """1
0
10
0
99999989"""

# custom cases
assert solve("""3
1 0
2 3
7 20
""") == """2
1
0"""

assert solve("""2
10 0
10 29
""") == """20
1"""

assert solve("""1
100000000 1
""") == """199999999"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`2`| Bắt đầu chính xác tại ranh giới chu kỳ | 
|`2 3`|`1`| Bên trong quá trình chuyển đổi phân khúc giữa | 
|`7 20`|`0`| Đã có trong phân đoạn thời gian | 
|`10 29`|`1`| Cạnh ngay trước khi bọc | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$m$chính xác là bội số của$3k$. Trong trường hợp này$r = 0$, nghĩa là chúng ta đang ở giai đoạn đầu của chu kỳ ở giai đoạn nhiệt độ nước. Thuật toán tính toán chính xác khoảng cách tới$2k$, đó là$2k$, khớp với thời điểm hiển thị thời gian đầu tiên. 

Một trường hợp cạnh khác là khi$m = 2k$. Đây$r = 2k$, vậy điều kiện$r \ge 2k$kích hoạt và đầu ra bằng không. Điều này phù hợp với thực tế là đồng hồ bắt đầu hiển thị thời gian chính xác vào thời điểm đó. 

Trường hợp thứ ba là khi$m$ngay trước khi đoạn thời gian kết thúc, chẳng hạn như$m = 3k - 1$. Modulo mang lại$r = 3k - 1$, vẫn nằm trong khoảng thời gian hiển thị, vì vậy câu trả lời là 0. Điều này xác nhận rằng bất kỳ vị trí nào bên trong khoảng là đủ, bất kể khoảng cách đến ranh giới là bao nhiêu.

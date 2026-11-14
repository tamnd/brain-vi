---
title: "CF 1796A - Bài toán phỏng vấn điển hình"
description: "Chúng ta được yêu cầu lý giải về một chuỗi vô hạn không được xây dựng trực tiếp theo từng ký tự mà được tạo từ các số nguyên dương theo thứ tự."
date: "2026-06-09T10:01:51+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1796
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 144 (Rated for Div. 2)"
rating: 800
weight: 1796
solve_time_s: 204
verified: true
draft: false
---

[CF 1796A - Typical Interview Problem](https://codeforces.com/problemset/problem/1796/A)

 **Đánh giá:** 800 
**Tags:** vũ lực, thực hiện, dây 
**Thời gian giải:** 3 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu lý giải về một chuỗi vô hạn không được xây dựng trực tiếp theo từng ký tự mà được tạo từ các số nguyên dương theo thứ tự. Với mỗi số nguyên$i = 1, 2, 3, \dots$, chúng tôi nối thêm các ký tự tùy thuộc vào khả năng chia hết: chúng tôi nối thêm`F`nếu như$i$chia hết cho 3 và chúng tôi nối thêm`B`nếu như$i$chia hết cho 5. Nếu một số chia hết cho cả 3 và 5 thì cả hai ký tự được thêm vào theo thứ tự đó, nghĩa là`F`đầu tiên và sau đó`B`. 

Điều này tạo ra một chuỗi vô hạn duy nhất trên bảng chữ cái`{F, B}`. Mỗi số nguyên đóng góp 0, một hoặc hai ký tự, do đó chuỗi phát triển không đồng đều nhưng mang tính xác định. 

Mỗi truy vấn đưa ra một chuỗi ngắn$s$có độ dài tối đa là 10 và chúng ta phải xác định xem nó có xuất hiện dưới dạng một đoạn liền kề ở đâu đó trong chuỗi được tạo vô hạn này hay không. 

Sự hạn chế đó$k \le 10$là gợi ý cấu trúc quan trọng. Một chuỗi con có độ dài nhỏ như vậy không thể yêu cầu chúng ta phải tìm hiểu sâu về công trình. Bất kỳ lời giải đúng nào cũng chỉ cần hiểu phần giới hạn của dãy vô hạn, bởi vì các mẫu cục bộ lặp lại thường xuyên. 

Một nỗ lực ngây thơ có thể cố gắng mô phỏng quy trình cho một phạm vi số nguyên lớn, nhưng điều này nhanh chóng trở nên không cần thiết khi chúng ta nhận thấy cấu trúc lặp lại sau mỗi 15 số nguyên, vì khả năng chia hết cho 3 và 5 chỉ phụ thuộc vào$i \bmod 15$. 

Một lỗi nhỏ sẽ phát sinh nếu chúng ta chỉ kiểm tra trong một tiền tố nhỏ của các số nguyên mà không xử lý sự chồng chéo chuỗi con qua các ranh giới. Ví dụ: một mẫu có thể bắt đầu ở gần cuối một khối gồm 15 số nguyên và tiếp tục sang khối tiếp theo. Một chế độ lỗi khác là xử lý quy trình dưới dạng một chuỗi trên mỗi số nguyên thay vì trên mỗi ký tự được phát ra, dẫn đến căn chỉnh chuỗi con không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng việc xây dựng chuỗi FB bằng cách lặp qua các số nguyên và nối thêm các ký tự cho đến khi chuỗi trở nên “đủ dài”, thường là cho đến khi nó vượt quá bội số của độ dài truy vấn. Vì mỗi số nguyên tạo ra tối đa 2 ký tự và$k \le 10$, chúng tôi có thể mô phỏng một cách an toàn khoảng vài trăm số nguyên cho mỗi trường hợp thử nghiệm và sau đó chạy kiểm tra chuỗi con. 

Cách tiếp cận này đúng vì nó tái tạo lại định nghĩa một cách trung thực. Tuy nhiên, nó sẽ không hiệu quả nếu được mở rộng quy mô, vì mỗi trường hợp thử nghiệm sẽ mô phỏng lặp lại nhiều số nguyên mặc dù cấu trúc lặp lại một cách xác định. 

Quan sát quan trọng là tính tuần hoàn. Phần đóng góp của mỗi số nguyên chỉ phụ thuộc vào việc nó có chia hết cho 3 và 5 hay không, điều này hoàn toàn được xác định bởi phần dư của nó theo modulo 15. Mỗi khối gồm 15 số nguyên liên tiếp tạo ra chính xác cùng một chuỗi các ký tự được nối thêm. Điều này có nghĩa là toàn bộ chuỗi FB vô hạn là sự lặp lại của một chuỗi hữu hạn cố định tương ứng với các số nguyên từ 1 đến 15. 

Khi chúng ta có khối cơ sở đó, có thể tìm thấy bất kỳ chuỗi con nào của chuỗi vô hạn bằng cách tìm kiếm trong phiên bản lặp lại đủ của nó. Vì độ dài truy vấn tối đa là 10 nên việc sao chép khối cơ sở một số lần nhỏ đảm bảo chúng ta không bao giờ bỏ lỡ các chuỗi con vượt qua ranh giới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(t · N) | O(N) | Về mặt khái niệm quá chậm | 
| Giai đoạn 15 Xây dựng | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một khoảng thời gian đầy đủ của chuỗi FB bằng cách mô phỏng các số nguyên từ 1 đến 15. Sau đó, chúng tôi lặp lại khối này một vài lần để bao phủ các chuỗi con vượt qua ranh giới một cách an toàn. Cuối cùng, chúng tôi kiểm tra xem chuỗi truy vấn có xuất hiện bên trong chuỗi mở rộng này hay không. 

1. Lặp lại các số nguyên từ 1 đến 15 và xây dựng chuỗi cơ sở. 

Với mỗi số nguyên, nối thêm`F`nếu chia hết cho 3 thì nối thêm`B`nếu chia hết cho 5 thì theo thứ tự đó. Điều này tạo ra khối lặp lại cơ bản. 
2. Lặp lại khối này nhiều lần, ví dụ 3 lần. 

Điều này đảm bảo mọi chuỗi con có độ dài tối đa 10 đều được chứa đầy đủ ngay cả khi nó vượt qua ranh giới giữa hai khối. 
3. Đối với mỗi chuỗi truy vấn, hãy kiểm tra xem đó có phải là chuỗi con của chuỗi hữu hạn được xây dựng này hay không. 
4. Đầu ra`YES`nếu tìm thấy, nếu không`NO`. 

### Tại sao nó hoạt động 

Việc xây dựng chỉ phụ thuộc vào khả năng chia hết cho 3 và 5, lặp lại với chu kỳ 15 trên các số nguyên. Vì mỗi số nguyên đóng góp độc lập chỉ dựa trên thuộc tính này nên chuỗi ký tự được phát ra sẽ lặp lại chính xác cứ sau 15 số nguyên. Do đó, bất kỳ chuỗi con nào của chuỗi vô hạn đều được chứa trong các bản sao lặp lại của một khối hữu hạn cố định. Việc mở rộng khối vượt quá một khoảng thời gian một chút sẽ đảm bảo rằng các chuỗi con vượt qua ranh giới cũng được ghi lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# build one period (1..15)
base = []
for i in range(1, 16):
    if i % 3 == 0:
        base.append("F")
    if i % 5 == 0:
        base.append("B")

base = "".join(base)

# repeat enough to cover substrings up to length 10 crossing boundaries
text = base * 3

t = int(input())
for _ in range(t):
    k = int(input())
    s = input().strip()
    if s in text:
        print("YES")
    else:
        print("NO")
```Trước tiên, giải pháp này xây dựng đơn vị lặp lại tối thiểu của chuỗi FB trên các chỉ số nguyên từ 1 đến 15. Sau đó, giải pháp này lặp lại đơn vị này ba lần để đảm bảo rằng bất kỳ chuỗi con nào có độ dài lên tới 10 đều được thể hiện đầy đủ ngay cả khi nó kéo dài đến cuối một khoảng thời gian và bắt đầu một khoảng thời gian khác. Mỗi truy vấn được trả lời bằng cách sử dụng kiểm tra chuỗi con trực tiếp, cách này hiệu quả vì cả văn bản và mẫu đều nhỏ và có kích thước cố định. 

Điểm tinh tế duy nhất là đảm bảo thứ tự chính xác của các ký tự được nối thêm khi một số nguyên chia hết cho cả 3 và 5. Mã duy trì điều này bằng cách nối thêm`F`đầu tiên và`B`thứ hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào`FFB`được kiểm tra dựa trên văn bản định kỳ được xây dựng. 

| Bước | Hoạt động | Hiện trạng | 
| --- | --- | --- | 
| 1 | Xây dựng căn cứ từ 1..15 | base = "F B F F B F F B F F B F F B F" (về mặt khái niệm) | 
| 2 | Lặp lại 3 lần | văn bản = cơ sở + cơ sở + cơ sở | 
| 3 | Kiểm tra chuỗi con | Đã tìm thấy "FFB" | 

Điều này xác nhận rằng các chuỗi ngắn bắt đầu ở các ranh giới số nguyên khác nhau được ghi lại chính xác bên trong cấu trúc lặp lại. 

### Ví dụ 2 

Chuỗi đầu vào`BBB`được thử nghiệm. 

| Bước | Hoạt động | Hiện trạng | 
| --- | --- | --- | 
| 1 | Xây dựng căn cứ | khối tuần hoàn cố định | 
| 2 | Lặp lại | văn bản mở rộng | 
| 3 | Kiểm tra chuỗi con | không tìm thấy | 

Điều này cho thấy rằng mặc dù`B`xuất hiện thường xuyên, ba lần liên tiếp`B`s không bao giờ căn chỉnh trong một phân đoạn xây dựng hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi thử nghiệm thực hiện kiểm tra chuỗi con trên chuỗi có kích thước không đổi | 
| Không gian | O(1) | Mẫu được xây dựng là cố định và không phụ thuộc vào kích thước đầu vào | 

Các ràng buộc cho phép tối đa 2046 truy vấn, nhưng mỗi truy vấn được trả lời trong thời gian không đổi dựa trên một chuỗi nhỏ được tính toán trước, do đó giải pháp dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    base = []
    for i in range(1, 16):
        if i % 3 == 0:
            base.append("F")
        if i % 5 == 0:
            base.append("B")
    base = "".join(base)
    text = base * 3

    t = int(input())
    out = []
    for _ in range(t):
        k = int(input())
        s = input().strip()
        out.append("YES" if s in text else "NO")
    return "\n".join(out)

# provided samples
assert run("""3
3
FFB
8
BFFBFFBF
3
BBB
""") == """YES
YES
NO"""

# minimum size
assert run("""1
1
F
""") == "YES"

# simple B case
assert run("""1
1
B
""") == "YES"

# boundary-crossing style case
assert run("""1
2
FB
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`F`| CÓ | khớp một ký tự | 
|`B`| CÓ | đơn B luôn xuất hiện | 
|`FB`| CÓ | sắp xếp thứ tự đúng trên cùng một số nguyên | 
|`BBB`| KHÔNG | mô hình ba không thể | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi một chuỗi con bắt đầu ở gần cuối một chu kỳ 15 số nguyên và tiếp tục vào chuỗi tiếp theo. Thuật toán xử lý việc này vì việc xây dựng lặp đi lặp lại`base * 3`đảm bảo rằng mọi vùng biên xuất hiện đầy đủ bên trong biểu diễn hữu hạn. 

Ví dụ: nếu một mẫu bắt đầu ở ký tự cuối cùng của một khối và tiếp tục sang khối tiếp theo, thì chuỗi được xây dựng vẫn chứa phần chồng chéo đó ở bản sao ở giữa của sự lặp lại. Việc kiểm tra chuỗi con sau đó sẽ tìm thấy nó trực tiếp mà không cần xử lý đặc biệt các chỉ mục hoặc căn chỉnh số nguyên.

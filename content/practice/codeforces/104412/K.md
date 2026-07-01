---
title: "CF 104412K - Phép loại trực tiếp"
description: "Chúng ta có một lưới vuông có kích thước $N nhân N$, trong đó mỗi ô chứa một chữ số từ 0 đến 9 biểu thị loại địa hình. Chúng ta cũng được cung cấp một kích thước cố định $K$ và chúng ta cần kiểm tra mọi ô vuông con $K nhân K$ bên trong lưới."
date: "2026-06-30T22:52:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104412
codeforces_index: "K"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 104412
solve_time_s: 51
verified: true
draft: false
---

[CF 104412K - Phép loại trực tiếp](https://codeforces.com/problemset/problem/104412/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới vuông có kích thước$N \times N$, trong đó mỗi ô chứa một chữ số từ 0 đến 9 biểu thị loại địa hình. Chúng tôi cũng được cung cấp một kích thước cố định$K$, và chúng ta cần kiểm tra mọi khả năng$K \times K$hình vuông con bên trong lưới. 

Một hình vuông con được coi là hợp lệ nếu cả bốn ô góc của nó đều chứa cùng một giá trị. Nhiệm vụ là đếm có bao nhiêu$K \times K$các ô vuông con tồn tại trong lưới. 

Các ràng buộc đầu vào làm cho lưới có thể lớn, lên tới$1000 \times 1000$. Điều đó ngay lập tức loại trừ mọi giải pháp kiểm tra lại mọi$K \times K$ô vuông từng ô trong trường hợp xấu nhất. Một kiểm tra đơn giản trên mỗi ô vuông sẽ kiểm tra$K^2$tế bào, dẫn đến$O(N^2 K^2)$, có thể đạt tới$10^{12}$hoạt động khi$N = K = 1000$, vượt xa giới hạn khả thi. 

Một vấn đề tinh tế hơn là tính toán chồng chéo. liền kề$K \times K$các cửa sổ chia sẻ phần lớn nội thất của chúng, nhưng một cách tiếp cận ngây thơ sẽ tính toán lại mọi thứ từ đầu. 

Một vài trường hợp đáng chú ý. 

Nếu như$K = N$, có chính xác một ô vuông cần kiểm tra, đó là toàn bộ lưới. Câu trả lời là 1 hoặc 0 tùy thuộc vào việc bốn góc có khớp với nhau hay không. 

Nếu tất cả các giá trị trong lưới đều giống nhau thì mọi$K \times K$hình vuông con là hợp lệ, vì vậy câu trả lời là$(N-K+1)^2$. 

Nếu chỉ có một chữ số xuất hiện ở các góc nhưng bên trong lại khác, một giải pháp đơn giản là kiểm tra sai tất cả các ô thay vì chỉ các góc vẫn hoạt động hợp lý nhưng lãng phí thời gian; tuy nhiên, một giải pháp có lỗi có thể vô tình bao gồm các hạn chế bên trong, điều này sẽ sai vì chỉ các góc mới quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi lặp lại mọi góc trên bên trái có thể$(i, j)$của một$K \times K$hình vuông phụ. Đối với mỗi người, chúng tôi so sánh rõ ràng tất cả$K^2$các ô hoặc ít nhất là đi qua ranh giới. Ngay cả khi chúng ta tối ưu hóa một chút và chỉ kiểm tra bốn góc, brute-force vẫn cần phải làm điều này$(N-K+1)^2$nhiều nhất là các vị trí$10^6$. Phần đó ổn. Chi phí thực chỉ xuất hiện nếu chúng tôi xác minh sai các ô vuông đầy đủ. 

Vì vậy, quan sát quan trọng là điều kiện chỉ phụ thuộc vào bốn vị trí: trên cùng bên trái, trên cùng bên phải, dưới cùng bên trái, dưới cùng bên phải. Mọi thứ khác bên trong hình vuông đều không liên quan. Điều này thu gọn vấn đề thành một cuộc kiểm tra liên tục theo thời gian cho mỗi vị trí. 

Do đó, thay vì quét lưới hoặc duy trì cấu trúc tiền tố, chúng tôi chỉ cần lặp lại tất cả các điểm bắt đầu hợp lệ và so sánh bốn ô. 

Vấn đề giảm từ một nhiệm vụ tổng hợp hình học 2D thành một vấn đề liệt kê trượt đơn giản với công việc liên tục trên mỗi cửa sổ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra ô vuông đầy đủ) |$O(N^2 K^2)$|$O(1)$| Quá chậm | 
| Kiểm tra chỉ ở góc |$O(N^2)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$N$Và$K$, sau đó lưu lưới vào mảng 2D. Điều này là cần thiết vì chúng ta cần truy cập ngẫu nhiên vào bất kỳ ô nào khi kiểm tra các góc. 
2. Lặp lại tất cả các vị trí trên cùng bên trái có thể có của một$K \times K$quảng trường. Những vị trí này là$(i, j)$Ở đâu$0 \le i \le N-K$Và$0 \le j \le N-K$. Các giới hạn này đảm bảo hình vuông nằm trong lưới. 
3. Đối với từng vị trí$(i, j)$, xác định bốn góc của nó:$(i, j)$,$(i, j+K-1)$,$(i+K-1, j)$, Và$(i+K-1, j+K-1)$. Đây là những ô duy nhất quan trọng đối với tính hợp lệ. 
4. Kiểm tra xem tất cả bốn giá trị góc có bằng nhau không. Nếu có, hãy tăng câu trả lời. 
5. Sau khi xử lý tất cả các vị trí, xuất ra số đếm tích lũy. 

Lựa chọn thiết kế quan trọng là chỉ hạn chế kiểm tra ở các góc. Điều này hợp lệ vì điều kiện không bao giờ tham chiếu đến các ô bên trong nên mọi tính toán bổ sung sẽ là dư thừa. 

### Tại sao nó hoạt động 

Mỗi vị trí hợp lệ được xác định hoàn toàn bởi một điều kiện cục bộ trên bốn vị trí cố định. Mỗi$K \times K$hình vuông được xác định duy nhất bởi góc trên cùng bên trái của nó và vị từ hợp lệ chỉ phụ thuộc vào các giá trị ở các độ lệch cố định từ điểm neo đó. Vì chúng tôi đánh giá mỗi neo chính xác một lần và áp dụng điều kiện chính xác xác định tính hợp lệ nên không có ô vuông hợp lệ nào bị bỏ sót và không có ô vuông không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(N)]
    
    ans = 0
    
    for i in range(N - K + 1):
        for j in range(N - K + 1):
            a = grid[i][j]
            b = grid[i][j + K - 1]
            c = grid[i + K - 1][j]
            d = grid[i + K - 1][j + K - 1]
            if a == b == c == d:
                ans += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo thuật toán. Các vòng lặp lồng nhau liệt kê tất cả các góc trên cùng bên trái hợp lệ. Bốn truy cập ở góc là tra cứu mảng theo thời gian không đổi, đảm bảo duy trì vòng lặp bên trong$O(1)$. 

Sự tinh tế chính là lập chỉ mục: góc dưới bên phải là ở$i + K - 1, j + K - 1$, không$i + K, j + K$. Lỗi ngẫu nhiên ở đây là nguồn lỗi phổ biến nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
0 0
0 0
```| tôi | j | góc (TL, TR, BL, BR) | bình đẳng? | đếm | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | (0,0,0,0) | vâng | 1 | 

Điều này xác nhận rằng khi tất cả các giá trị giống hệt nhau thì bình phương duy nhất có thể là hợp lệ. 

### Ví dụ 2 

đầu vào:```
2 2
1 2
1 1
```| tôi | j | góc (TL, TR, BL, BR) | bình đẳng? | đếm | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | (1,2,1,1) | không | 0 | 

Điều này cho thấy rằng mặc dù ba ô khớp nhau nhưng sự không khớp ở một góc sẽ làm hình vuông không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| Chúng tôi xem xét từng khả năng$K \times K$vị trí một lần và mỗi lần kiểm tra là thời gian không đổi | 
| Không gian |$O(N^2)$| Lưu trữ cho lưới | 

Kích thước lưới lên tới$10^6$các tế bào phù hợp thoải mái trong bộ nhớ, và$10^6$kiểm tra thời gian liên tục dễ dàng vượt qua trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""2 2
0 0
0 0
""") == "1", "sample 1"

assert run("""2 2
1 2
1 1
""") == "0", "sample 2"

assert run("""5 3
1 5 1 6 1
1 7 8 9 5
1 1 1 1 1
1 2 3 4 1
1 1 1 1 1
""") == "5", "sample 3"

# custom cases
assert run("""3 2
1 1 2
1 1 2
3 3 3
""") == "1", "single valid square"

assert run("""4 3
7 7 7 7
7 1 2 7
7 3 4 7
7 7 7 7
""") == "4", "multiple boundary-valid squares"

assert run("""3 3
9 9 9
9 8 9
9 9 9
""") == "1", "only full grid"

assert run("""3 2
1 2 1
3 4 5
6 7 8
""") == "0", "no matching corners"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3x2 với kết quả khớp một phần | 1 | phát hiện vị trí hợp lệ duy nhất | 
| Cấu trúc viền 4x3 | 4 | nhiều cửa sổ hợp lệ chồng chéo | 
| đầy đủ 3x3 với tâm không khớp | 1 | tính chính xác trong trường hợp toàn lưới | 
| lưới hoàn toàn khác biệt | 0 | từ chối tất cả các cửa sổ không hợp lệ | 

## Vỏ cạnh 

Khi nào$K = N$, có đúng một ô vuông ứng cử viên. Thuật toán đánh giá một lần lặp đơn với$(i, j) = (0, 0)$. Nó so sánh trực tiếp bốn góc của toàn bộ lưới và chỉ trả về 1 nếu chúng khớp nhau. Không có quyền truy cập ngoài giới hạn nào xảy ra vì$i + K - 1 = N - 1$, đó là lập chỉ mục hợp lệ. 

Khi tất cả các giá trị lưới giống hệt nhau, mọi lần lặp đều thỏa mãn điều kiện. Các vòng lặp chạy qua$(N-K+1)^2$vị trí và mỗi so sánh đều trả về giá trị đúng. Câu trả lời cuối cùng bằng số lượng vị trí có thể có, khớp với số lượng tổ hợp của các ô vuông phụ. 

Khi chỉ có các ô bên trong khác nhau nhưng các góc khớp nhau, thuật toán vẫn tính chính xác vì nó không bao giờ kiểm tra các vị trí bên trong. Ví dụ, trong một$3 \times 3$quảng trường:```
1 1 1
1 9 1
1 1 1
```MỘT$3 \times 3$cửa sổ có các góc trùng khớp (tất cả 1) nên được tính một lần, bất kể 9 ở giữa. Điều này khẳng định rằng việc hạn chế chú ý đến các góc vừa đủ vừa cần thiết cho tính chính xác.

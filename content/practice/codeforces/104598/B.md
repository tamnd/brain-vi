---
title: "CF 104598B - Chia tách tốc độ"
description: "Mỗi lần chạy trò chơi ghi lại một chuỗi thời gian phân chia và mỗi lần chạy đều có số lần phân chia như nhau. Bạn có thể coi đầu vào là một ma trận với các hàng $N$ và các cột $K$, trong đó hàng $i$ lưu trữ thời gian dành cho mỗi lần phân chia trong lần chạy $i$ và các cột tương ứng với cùng một…"
date: "2026-06-30T03:03:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104598
codeforces_index: "B"
codeforces_contest_name: "GPL 2023 Advanced"
rating: 0
weight: 104598
solve_time_s: 80
verified: true
draft: false
---

[CF 104598B - Chia tách tốc độ](https://codeforces.com/problemset/problem/104598/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi lần chạy trò chơi ghi lại một chuỗi thời gian phân chia và mỗi lần chạy đều có số lần phân chia như nhau. Bạn có thể coi đầu vào là một ma trận với$N$hàng và$K$cột, nơi hàng$i$lưu trữ thời gian thực hiện cho mỗi lần phân chia trong lần chạy$i$và các cột tương ứng với cùng một mục tiêu trong trò chơi qua các lần chạy khác nhau. 

Đối với bất kỳ truy vấn nào, chúng tôi được cung cấp một chỉ mục phân chia$s$. Nhiệm vụ là chỉ nhìn vào cột$s$, trích xuất$N$giá trị từ tất cả các lần chạy cho phần phân chia đó và tính toán mức cải thiện tối đa giữa hai lần chạy bất kỳ. Cải thiện có nghĩa là lần chạy sau có sự khác biệt nhanh hơn hoặc chậm hơn, nhưng vì thời gian tăng dần trong mỗi lần chạy nên cách giải thích có ý nghĩa chỉ đơn giản là sự khác biệt tối đa giữa hai giá trị bất kỳ trong cột đó, tức là.$\max(t_i[s]) - \min(t_i[s])$. 

Do đó, đầu ra cốt lõi của mỗi truy vấn không phụ thuộc vào thứ tự giữa các lần chạy và chỉ phụ thuộc vào phạm vi giá trị trong một cột. 

Các ràng buộc cho phép lên đến$N, K \le 700$, làm cho tổng số giá trị được lưu trữ tối đa là 490.000. Số lượng truy vấn$Q$có thể lớn như$10^5$, đủ lớn để việc tính toán lại phạm vi cột cho mỗi truy vấn sẽ quá chậm nếu được thực hiện một cách ngây thơ. 

Một giải pháp đơn giản có thể quét tất cả$N$chạy cho mỗi truy vấn sẽ tốn kém$O(NQ)$, trở thành về$7 \times 10^7$hoạt động trong trường hợp xấu nhất. Đây là ranh giới nhưng không cần thiết vì có thể xử lý trước. 

Trường hợp cạnh khóa xuất phát từ các truy vấn lặp đi lặp lại. Nếu cùng một chỉ mục phân tách xuất hiện nhiều lần, việc tính toán lại mức tối thiểu và tối đa của nó liên tục sẽ lãng phí thời gian. 

Một điểm tinh tế khác là giá trị của mỗi lần chạy đang tăng lên đáng kể qua các phần tách, nhưng thuộc tính này không giúp ích gì trong một cột duy nhất; nó chỉ đảm bảo cấu trúc trong các hàng chứ không phải trên các lần chạy. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi lặp lại tất cả các lần chạy, thu thập các giá trị cho phần tách được yêu cầu và tính toán mức tối thiểu và tối đa. Câu trả lời là sự khác biệt của họ. Điều này đúng vì định nghĩa cải tiến làm giảm mức chênh lệch tối đa trong cột đó. 

Vấn đề là hiệu quả. Mỗi truy vấn có giá$O(N)$, Vì thế$Q$chi phí truy vấn$O(NQ)$. Với$N = 700$Và$Q = 100000$, con số này đạt tới 70 triệu thao tác và trong Python, đây là chi phí không cần thiết, đặc biệt là khi quét và lập chỉ mục lặp đi lặp lại. 

Điều quan trọng là mỗi truy vấn chỉ phụ thuộc vào một cột duy nhất của ma trận cố định. Vì ma trận không bao giờ thay đổi nên chúng ta có thể tính toán trước giá trị tối thiểu và tối đa cho mỗi cột một lần. Sau đó, mỗi truy vấn được trả lời theo thời gian không đổi bằng cách trừ đi các giá trị được tính toán trước. 

Chúng tôi giao dịch quét lặp đi lặp lại một lần$O(NK)$bước tiền xử lý. Điều này dễ dàng phù hợp trong giới hạn vì nó có nhiều nhất là 490.000 thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NQ)$|$O(1)$thêm | Quá chậm | 
| Tính toán trước tối thiểu/tối đa trên mỗi cột |$O(NK + Q)$|$O(K)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hai mảng có kích thước$K$: một cho giá trị tối thiểu trên mỗi lần chia và một cho giá trị tối đa trên mỗi lần chia. 

1. Khởi tạo hai mảng`mn`Và`mx`kích thước$K$. Đặt tất cả`mn`giá trị đến một số lượng rất lớn và tất cả`mx`giá trị về một số rất nhỏ. Điều này chuẩn bị cho chúng ta cập nhật điểm cực trị một cách an toàn cho mỗi cột. 
2. Đọc từng phần một. Đối với mỗi lần chạy, hãy lặp lại$K$chia tách các giá trị. Đối với vị trí$j$, cập nhật`mn[j] = min(mn[j], value)`Và`mx[j] = max(mx[j], value)`. Điều này đảm bảo rằng sau khi xử lý tất cả các lần chạy, mỗi cột sẽ lưu trữ mức tối thiểu và tối đa toàn cầu của nó. 
3. Đối với mỗi truy vấn, hãy đọc chỉ mục phân tách$s$, chuyển đổi nó thành lập chỉ mục dựa trên 0 và xuất ra`mx[s] - mn[s]`. 

Tính chính xác xuất phát từ thực tế là mọi truy vấn đều yêu cầu sự khác biệt tối đa giữa hai phần tử bất kỳ trong một cột cố định. Các ứng cử viên duy nhất quan trọng là mức tối thiểu tổng thể và mức tối đa tổng thể của cột đó, vì bất kỳ cặp nào khác đều tạo ra sự khác biệt nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K, Q = map(int, input().split())

    INF = 10**18
    mn = [INF] * K
    mx = [-INF] * K

    for _ in range(N):
        row = list(map(int, input().split()))
        for j in range(K):
            v = row[j]
            if v < mn[j]:
                mn[j] = v
            if v > mx[j]:
                mx[j] = v

    for _ in range(Q):
        s = int(input()) - 1
        print(mx[s] - mn[s])

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì hai mảng được lập chỉ mục theo vị trí phân chia. Vòng lặp tiền xử lý đảm bảo rằng các giá trị cực trị của mỗi cột được tính toán trong một lần truyền qua ma trận đầu vào. Việc xử lý truy vấn được giảm xuống thành tra cứu và trừ mảng trực tiếp. 

Một lỗi phổ biến ở đây là tính toán lại giá trị tối thiểu và tối đa cho mỗi truy vấn hoặc đặt lại sai mảng trên mỗi hàng. Một vấn đề tinh tế khác là quên chuyển đổi chỉ mục truy vấn từ dựa trên 1 sang dựa trên 0, điều này sẽ âm thầm chuyển các câu trả lời sang phần phân tách sai. 

## Ví dụ đã hoạt động 

### Mẫu 1 Trace 

Ma trận đầu vào: 

| Chạy | Chia 1 | Chia 2 | Chia 3 | Chia 4 | Chia 5 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 4 | 6 | 9 | 
| 2 | 2 | 4 | 5 | 7 | 8 | 
| 3 | 1 | 2 | 6 | 9 | 10 | 
| 4 | 1 | 2 | 3 | 4 | 7 | 

Sau khi tiền xử lý: 

| Chia | Tối thiểu | Tối đa | Tối đa - Tối thiểu | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 
| 2 | 2 | 4 | 2 | 
| 3 | 3 | 6 | 3 | 
| 4 | 4 | 9 | 5 | 
| 5 | 7 | 10 | 3 | 

Các truy vấn yêu cầu chia 2, 4, 1, tạo ra kết quả 2, 5, 1. 

Dấu vết này xác nhận rằng mỗi truy vấn là độc lập và chỉ dựa vào thống kê cột chứ không dựa vào thứ tự chạy. 

### Dấu vết tùy chỉnh 

đầu vào:```
3 3 3
5 10 20
2 15 30
8 12 25
1
2
3
```| Chia | Giá trị | Tối thiểu | Tối đa | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 5,2,8 | 2 | 8 | 6 | 
| 2 | 10,15,12 | 10 | 15 | 5 | 
| 3 | 20,30,25 | 20 | 30 | 10 | 

Điều này cho thấy quá trình tiền xử lý nắm bắt rõ ràng toàn bộ phạm vi trên mỗi cột ngay cả khi các giá trị không đơn điệu trong các lần chạy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NK + Q)$| Một lần vượt qua tất cả các mục nhập ma trận, sau đó truy vấn thời gian không đổi | 
| Không gian |$O(K)$| Chỉ có hai mảng lưu trữ tối thiểu và tối đa trên mỗi cột | 

Quá trình tiền xử lý chiếm ưu thế với tối đa 490.000 bản cập nhật và mỗi truy vấn có thời gian không đổi. Điều này là thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline

    N, K, Q = map(int, input().split())

    INF = 10**18
    mn = [INF] * K
    mx = [-INF] * K

    for _ in range(N):
        row = list(map(int, input().split()))
        for j in range(K):
            mn[j] = min(mn[j], row[j])
            mx[j] = max(mx[j], row[j])

    out = []
    for _ in range(Q):
        s = int(input()) - 1
        out.append(str(mx[s] - mn[s]))

    return "\n".join(out)

# provided sample
assert run("""4 5 3
1 3 4 6 9
2 4 5 7 8
1 2 6 9 10
1 2 3 4 7
2
4
1
""") == "2\n5\n1"

# minimum size
assert run("""1 1 1
5
1
""") == "0"

# all equal column behavior
assert run("""3 2 2
5 5
5 5
5 5
1
2
""") == "0\n0"

# increasing spread
assert run("""3 3 3
1 2 3
2 4 6
3 6 9
1
2
3
""") == "2\n4\n6"

# mixed values
assert run("""2 3 3
10 1 7
3 9 2
1
2
3
""") == "7\n8\n5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vỏ 1×1 | 0 | yếu tố đơn lẻ mang lại sự cải thiện bằng không | 
| tất cả đều bình đẳng | 0 giây | không có chênh lệch sai khi các giá trị giống hệt nhau | 
| tăng sức lan tỏa | 2,4,6 | tính chính xác của tổng hợp tối thiểu/tối đa | 
| giá trị hỗn hợp | 7,8,5 | các giá trị không có thứ tự được xử lý chính xác | 

## Vỏ cạnh 

Đầu vào tối thiểu với một lần chạy cho thấy rằng mọi cột đều không có cải thiện gì vì mức tối thiểu bằng mức tối đa. Quá trình xử lý trước khởi tạo chính xác vì mọi giá trị đều cập nhật cả hai giới hạn thành cùng một số và các truy vấn trả về 0 một cách chính xác. 

Trường hợp tất cả các lần chạy đều có chung giá trị trên mỗi cột sẽ kiểm tra xem thuật toán có vô tình tích lũy sự khác biệt giữa các hàng hay không. Vì giá trị tối thiểu và tối đa vẫn bằng nhau cho mỗi cột nên phép trừ mang lại kết quả bằng 0, phù hợp với hành vi mong đợi. 

Trường hợp các giá trị rất khác nhau nhưng không được sắp xếp theo các lần chạy xác nhận rằng thứ tự các lần chạy không thành vấn đề. Thuật toán chỉ dựa vào cực trị, do đó, bất kỳ hoán vị hàng nào cũng tạo ra kết quả như nhau và quá trình tính toán vẫn ổn định.

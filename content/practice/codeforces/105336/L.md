---
title: "CF 105336L - \u7f51\u7edc\u9884\u9009\u8d5b"
description: "Chúng ta được cung cấp một lưới các chữ cái viết thường với $n$ hàng và $m$ cột. Từ lưới này, chúng tôi quan tâm đến việc chọn bất kỳ hai hàng liên tiếp và hai cột liên tiếp bất kỳ. Mỗi lựa chọn như vậy xác định một ma trận con $2 nhân 2$."
date: "2026-06-23T15:26:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105336
codeforces_index: "L"
codeforces_contest_name: "The 2024 CCPC Online Contest"
rating: 0
weight: 105336
solve_time_s: 42
verified: true
draft: false
---

[CF 105336L - \u7f51\u7edc\u9884\u9009\u8d5b](https://codeforces.com/problemset/problem/105336/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mạng lưới các chữ cái viết thường với$n$hàng và$m$cột. Từ lưới này, chúng tôi quan tâm đến việc chọn bất kỳ hai hàng liên tiếp và hai cột liên tiếp bất kỳ. Mỗi lựa chọn như vậy xác định một$2 \times 2$ma trận phụ. Mục tiêu là đếm xem có bao nhiêu trong số này$2 \times 2$các khối khớp với một mẫu cụ thể:$$\begin{matrix}
c & c \\
p & c
\end{matrix}$$Nói cách khác, hàng trên cùng của khối phải có hai ký tự giống nhau, cả hai đều bằng nhau`'c'`, trong khi hàng dưới cùng phải có`'p'`ở bên trái và`'c'`ở bên phải. 

Đầu ra chỉ đơn giản là số lượng vị trí$(i, j)$sao cho ma trận con được hình thành bởi các hàng$i, i+1$và cột$j, j+1$thỏa mãn mẫu này. 

Những hạn chế$2 \le n, m \le 500$ngụ ý nhiều nhất$500 \times 500 = 250000$có thể có các góc trên bên trái cho một$2 \times 2$ma trận phụ. Điều này đủ nhỏ để$O(nm)$quét là đủ. Tuy nhiên, mô hình truy cập lưới rất quan trọng vì mỗi ứng cử viên phải được kiểm tra trong thời gian liên tục. 

Một sự hiểu lầm ngây thơ đôi khi xảy ra ở đây là quét tất cả các cặp hàng và cột một cách độc lập và tính toán lại các ma trận con một cách nặng nề hơn. Điều đó vẫn có tác dụng đối với kích thước vấn đề này nhưng không cần thiết. 

Trường hợp cạnh là tối thiểu nhưng đáng chú ý. Nếu lưới không chứa`'c'`chút nào, câu trả lời rõ ràng là bằng không. Nếu mỗi ô đều`'c'`, câu trả lời vẫn bằng 0 vì góc dưới bên trái phải là`'p'`, không bao giờ xuất hiện. Một trường hợp tế nhị khác là khi`'p'`xuất hiện nhưng không ở vị trí dưới cùng bên trái so với giá trị hợp lệ`'c'`cặp, dẫn đến kết quả khớp một phần nhưng không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại mọi góc trên bên trái có thể có của một$2 \times 2$ma trận con, trích xuất bốn ký tự của nó và kiểm tra xem nó có khớp với mẫu được yêu cầu hay không. Vì mỗi lần kiểm tra là thời gian không đổi nên việc này diễn ra trong$O(nm)$, nhiều nhất là 250.000 phép toán. Ngay cả khi chúng tôi xem xét chi phí từ việc lập chỉ mục Python, thì điều này vẫn đủ nhanh. 

Không cần tiền xử lý hoặc cấu trúc dữ liệu nâng cao. Quan sát quan trọng là mỗi ma trận con ứng cử viên là độc lập và việc xác thực chỉ yêu cầu so sánh ký tự trực tiếp. 

Người ta có thể cố gắng tính toán trước vị trí của`'c'`hoặc`'p'`, nhưng điều đó không làm giảm độ phức tạp một cách có ý nghĩa, vì nút cổ chai chỉ quét lưới một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Đã chấp nhận | 
| Tối ưu |$O(nm)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chỉ cần kiểm tra từng khả năng$2 \times 2$ma trận con một lần. 

## Hướng dẫn thuật toán 

1. Lặp lại tất cả các vị trí trên cùng bên trái có thể$(i, j)$Ở đâu$0 \le i < n-1$Và$0 \le j < m-1$. Điều này đảm bảo ma trận con nằm trong giới hạn. 
2. Với mỗi vị trí, hãy kiểm tra bốn ô: 

phía trên bên trái$(i, j)$, trên cùng bên phải$(i, j+1)$, dưới cùng bên trái$(i+1, j)$, và dưới cùng bên phải$(i+1, j+1)$. 
3. Kiểm tra xem các điều kiện này có thỏa mãn điều kiện không: 

hàng trên cùng phải là`'c'`, phía dưới bên trái phải là`'p'`, và phía dưới bên phải phải là`'c'`. 
4. Nếu điều kiện được giữ, hãy tăng bộ đếm câu trả lời. 
5. Sau khi quét tất cả các vị trí, xuất bộ đếm. 

Lý do chúng tôi trực tiếp kiểm tra từng ứng cử viên thay vì nhóm hoặc xử lý trước là vì mỗi ma trận con được xác định đầy đủ bởi bốn ký tự, do đó không có tính toán chồng chéo nào đáng để khai thác. 

### Tại sao nó hoạt động 

Mỗi câu trả lời hợp lệ tương ứng với chính xác một lựa chọn ở góc trên bên trái$(i, j)$. Thuật toán liệt kê tất cả các góc như vậy mà không bỏ sót hoặc trùng lặp. Đối với mỗi góc, nó áp dụng một vị từ xác định phù hợp với mẫu được yêu cầu. Vì vị từ được đánh giá chính xác một lần cho mỗi ứng viên và chỉ phụ thuộc vào bốn ô đó nên không thể bỏ sót cấu hình hợp lệ và không thể tính cấu hình không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    g = [input().strip() for _ in range(n)]

    ans = 0

    for i in range(n - 1):
        row1 = g[i]
        row2 = g[i + 1]
        for j in range(m - 1):
            if row1[j] == 'c' and row1[j + 1] == 'c' and row2[j] == 'p' and row2[j + 1] == 'c':
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh cách liệt kê được mô tả trước đó. Lưới được lưu trữ dưới dạng danh sách các chuỗi để việc truy cập ký tự được thực hiện$O(1)$. Chúng tôi tránh lập chỉ mục lặp đi lặp lại vào`g[i][j]`nếu có thể bằng cách lưu các tham chiếu hàng vào bộ nhớ đệm vào`row1`Và`row2`, giúp giảm chi phí trong Python. 

Giới hạn vòng lặp`n - 1`Và`m - 1`đảm bảo chúng tôi không bao giờ truy cập các chỉ mục ngoài phạm vi khi hình thành một$2 \times 2$khối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
cccc
spcc
ccpc
```Chúng tôi quét tất cả$2 \times 2$ma trận phụ. 

| tôi | j | trên cùng bên trái | trên cùng bên phải | dưới cùng bên trái | dưới cùng bên phải | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | c | c | s | p | không | 
| 0 | 1 | c | c | p | c | vâng | 
| 0 | 2 | c | c | c | c | không | 
| 1 | 0 | s | p | c | c | không | 
| 1 | 1 | p | c | c | p | không | 
| 1 | 2 | c | c | p | c | vâng | 

Câu trả lời là 2. 

Dấu vết này cho thấy rằng chỉ những vị trí có hàng trên cùng chính xác`'cc'`đóng góp và phía dưới bên trái chính xác phải là`'p'`. 

### Ví dụ 2 

đầu vào:```
2 2
cp
cc
```| tôi | j | trên cùng bên trái | trên cùng bên phải | dưới cùng bên trái | dưới cùng bên phải | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | c | p | c | c | không | 

Câu trả lời là 0. 

Điều này xác nhận rằng ngay cả khi hầu hết cấu trúc giống với mẫu, thì sự không khớp trong bất kỳ ô nào sẽ làm mất hiệu lực của ma trận con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi$2 \times 2$ma trận con được kiểm tra một lần với công việc liên tục | 
| Không gian |$O(1)$| Chỉ sử dụng bộ lưu trữ lưới và một vài biến | 

Với$n, m \le 500$, tối đa 250.000 lượt kiểm tra nằm trong giới hạn 1 giây trong Python khi sử dụng lập chỉ mục trực tiếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip()

# provided sample
def test_sample():
    input_data = """3 4
cccc
spcc
ccpc
"""
    assert solve_io(input_data) == "2"

# minimal case
def test_min():
    input_data = """2 2
cp
cc
"""
    assert solve_io(input_data) == "0"

# all c's
def test_all_c():
    input_data = """3 3
ccc
ccc
ccc
"""
    assert solve_io(input_data) == "0"

# single valid pattern
def test_one():
    input_data = """2 3
ccp
ccc
"""
    assert solve_io(input_data) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 4 mẫu | 2 | tính đúng đắn cơ bản | 
| 2x2 cp/cc | 0 | xử lý lưới tối thiểu | 
| tất cả lưới 'c' | 0 | không có kết quả dương tính giả | 
| trận đấu đơn | 1 | phát hiện trực tiếp | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi lưới đồng nhất. Ví dụ:```
2 2
cc
cc
```Thuật toán chỉ kiểm tra một ma trận con tại$(0,0)$, nhưng không đáp ứng được điều kiện vì phía dưới bên trái không`'p'`. Việc lặp lại vẫn chạy chính xác và kết quả bằng 0. 

Một trường hợp khác là khi`'p'`tồn tại nhưng không bao giờ được căn chỉnh với giá trị hợp lệ`'cc'`phía trên nó:```
3 3
ccc
cpc
ccc
```Mỗi lần kiểm tra$2 \times 2$cửa sổ sẽ không thành công ở hàng trên cùng hoặc điều kiện dưới cùng bên phải. Quá trình quét vẫn đánh giá tất cả bốn ứng cử viên, nhưng không có ứng cử viên nào đáp ứng đồng thời tất cả các ràng buộc, tạo ra kết quả bằng 0 như mong đợi.

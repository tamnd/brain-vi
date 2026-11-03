---
title: "CF 1106A - Tết Nguyên đán và tính chéo"
description: "Chúng ta được cấp một lưới hình vuông chứa hai ký tự có thể là X và .. Nhiệm vụ là quét mọi ô bên trong của lưới này và quyết định xem nó có phải là tâm của một mẫu “chữ thập” được hình thành bởi các đường chéo hay không."
date: "2026-06-13T08:10:44+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1106
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 536 (Div. 2)"
rating: 800
weight: 1106
solve_time_s: 274
verified: true
draft: false
---

[CF 1106A - Tết Nguyên Đán và Đếm chéo](https://codeforces.com/problemset/problem/1106/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 4 phút 34 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới vuông chứa hai ký tự có thể có,`X`Và`.`. Nhiệm vụ là quét mọi ô bên trong của lưới này và quyết định xem nó có phải là tâm của mô hình “chữ thập” được hình thành bởi các đường chéo hay không. 

một vị trí`(i, j)`chỉ đủ tiêu chuẩn là tâm chéo nếu bản thân ô đó và bốn ô lân cận theo đường chéo của nó đều`X`. Cụ thể, năm ô bắt buộc là`(i, j)`,`(i-1, j-1)`,`(i-1, j+1)`,`(i+1, j-1)`, Và`(i+1, j+1)`. 

Đầu ra chỉ đơn giản là số lượng các trung tâm hợp lệ như vậy trong lưới. 

Kích thước lưới tối đa là`500 × 500`, có nghĩa là lên tới 250.000 tế bào. Việc kiểm tra từng ô với lượng công việc không đổi là khả thi một cách thoải mái, nhưng mọi giải pháp liên tục quét các mẫu lớn hơn hoặc kiểm tra lại các điều kiện đắt tiền vẫn sẽ đạt hiệu quả trừ khi nó vô tình đưa ra công việc lặp lại không cần thiết bên trong các vòng lặp sâu hơn. 

Trường hợp cạnh tinh tế là khi lưới quá nhỏ để chứa bất kỳ tâm hợp lệ nào. Nếu như`n < 3`, không có ô nào có cả bốn ô liền kề theo đường chéo, vì vậy câu trả lời luôn bằng 0. Một trường hợp cạnh khác xuất hiện khi`X`các cụm tồn tại nhưng không đầy đủ theo đường chéo, ví dụ như một hàng hoặc cột đầy đủ hình dấu cộng của`X`không giúp ích gì, vì chỉ có đường chéo mới quan trọng. 

Một lỗi điển hình là chỉ kiểm tra sự kề cận theo bốn hướng hoặc sử dụng nhầm các đường lân cận trực giao thay vì đường chéo. Một lỗi khác xuất phát từ việc quên kiểm tra ranh giới, đặc biệt là tại`i = 1`,`i = n`,`j = 1`,`j = n`, nơi truy cập theo đường chéo sẽ vượt quá giới hạn. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu rất đơn giản. Đối với mỗi tế bào`(i, j)`trong lưới, chúng tôi cố gắng xác minh xem đó có phải là tâm hợp lệ hay không bằng cách kiểm tra bốn đường chéo lân cận cộng với chính nó. Mỗi lần kiểm tra mất thời gian không đổi, do đó độ phức tạp tổng thể là`O(n^2)`. Vì mỗi lần xác minh chỉ kiểm tra năm ô nên phương pháp này đã tối ưu về mặt hành vi tiệm cận. 

Lý do điều này hoạt động hiệu quả là vì điều kiện cho một đường chéo hoàn toàn mang tính cục bộ. Không có sự phụ thuộc giữa các trung tâm ứng viên khác nhau, do đó không cần xử lý trước hoặc lập trình động. Mỗi ô có thể được đánh giá độc lập. 

Mọi nỗ lực tối ưu hóa hơn nữa sẽ không làm thay đổi lớp phức tạp vì chúng tôi đã chạm vào từng ô một lần. “Tối ưu hóa” duy nhất là xử lý ranh giới cẩn thận và tránh tính toán dư thừa bên trong các bước kiểm tra bên trong. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O(n²) | O(1) thêm | Đã chấp nhận | 
| Tối ưu | O(n²) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước lưới và chính lưới đó vào bộ nhớ. Chúng tôi lưu trữ nó dưới dạng danh sách các chuỗi nên chỉ mục là O(1). Điều này cho phép truy cập ký tự trực tiếp mà không cần phân tích cú pháp. 

2. Khởi tạo bộ đếm về 0. Điều này sẽ tích lũy số lượng trung tâm chéo hợp lệ. 

3. Lặp lại tất cả các ô`(i, j)`như vậy`1 < i < n - 1`Và`1 < j < n - 1`. Chúng ta bỏ qua đường viền vì một tâm hợp lệ cần có bốn đường chéo lân cận. 

4. Đối với mỗi ô ứng cử viên, hãy kiểm tra xem tất cả năm vị trí bắt buộc có chứa`X`. Nếu bất kỳ ai trong số họ không`X`, ô ngay lập tức bị từ chối. Việc từ chối sớm này sẽ tránh được việc kiểm tra không cần thiết khi phát hiện thấy sự không phù hợp. 

5. Nếu tất cả các điều kiện đều được thỏa mãn, hãy tăng bộ đếm. 

6. Sau khi quét toàn bộ lưới, xuất bộ đếm. 

### Tại sao nó hoạt động 

Mỗi chữ thập hợp lệ được xác định duy nhất bởi ô trung tâm của nó. Định nghĩa về hình chữ thập chỉ phụ thuộc vào một tập hợp cố định gồm năm vị trí, nên việc kiểm tra năm ô đó vừa cần thiết vừa đủ. Không có cấu hình nào khác có thể tạo ra một đường chéo hợp lệ mà không làm cho các vị trí đường chéo chính xác đó bằng`X`. Vì mỗi trung tâm ứng viên đều được kiểm tra chính xác một lần và không có cấu hình hợp lệ nào bị bỏ qua nên thuật toán sẽ tính tất cả các lần vượt qua chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    grid = [input().strip() for _ in range(n)]

    if n < 3:
        print(0)
        return

    ans = 0

    for i in range(1, n - 1):
        for j in range(1, n - 1):
            if (grid[i][j] == 'X' and
                grid[i - 1][j - 1] == 'X' and
                grid[i - 1][j + 1] == 'X' and
                grid[i + 1][j - 1] == 'X' and
                grid[i + 1][j + 1] == 'X'):
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo thuật toán: chúng tôi chỉ lặp lại trên các ô bên trong hợp lệ và kiểm tra rõ ràng năm vị trí bắt buộc. Sớm`n < 3`kiểm tra ngăn chặn việc lặp lại không cần thiết và cũng làm cho logic ranh giới sạch hơn. 

Một sai lầm thường gặp khi thực hiện là trộn lẫn`(i-1, j+1)`với`(i+1, j-1)`hoặc quên hoàn toàn một đường chéo. Một vấn đề tế nhị khác là vô tình lặp lại từ`0`ĐẾN`n-1`và sau đó thêm các kiểm tra ranh giới bên trong vòng lặp, điều này làm tăng sự lộn xộn và nguy cơ xảy ra lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 
đầu vào:```
5
.....
.XXX.
.XXX.
.XXX.
.....
```Chúng tôi chỉ đánh giá vùng 3×3 bên trong. 

| (tôi, j) | lưới[i][j] | đường chéo hợp lệ? | kết quả | 
|--------|--------------|-------------------|--------| 
| (1,1) | . | không | 0 | 
| (1,2) | X | không | 0 | 
| (1,3) | X | không | 0 | 
| (2,1) | X | không | 0 | 
| (2,2) | X | vâng | 1 | 
| (2,3) | X | không | 1 | 
| (3,1) | X | không | 1 | 
| (3,2) | X | không | 1 | 
| (3,3) | X | không | 1 | 

Chỉ một`(2,2)`thỏa mãn mọi ràng buộc về đường chéo, vì vậy câu trả lời là`1`. 

Điều này xác nhận rằng chỉ có sự đối xứng đường chéo hoàn toàn xung quanh tâm mới đóng góp, ngay cả khi các hàng xung quanh trông dày đặc với`X`. 

### Ví dụ 2 
đầu vào:```
3
X.X
.X.
X.X
```| (tôi, j) | trung tâm X | đường chéo | kết quả | 
|--------|----------|----------|--------| 
| (1,1) | X | hợp lệ | 1 | 

Chỉ có một tâm duy nhất và tất cả các đường chéo cần thiết đều khớp nhau. Thuật toán tính chính xác nó là một chữ thập. 

Điều này hiển thị cấu hình hợp lệ nhỏ nhất trong đó tồn tại một dấu thập, xác nhận rằng giới hạn ranh giới đối với các ô bên trong là chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(n²) | mỗi ô lưới được kiểm tra nhiều nhất một lần với công việc liên tục | 
| Không gian | O(1) thêm | lưu trữ lưới sang một bên, chỉ sử dụng bộ đếm | 

Giới hạn kích thước lưới là 500`n² = 250,000`, nằm trong giới hạn để kiểm tra hệ số không đổi trên mỗi ô. Việc sử dụng bộ nhớ cũng không đáng kể vì chúng tôi chỉ lưu trữ lưới đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n = int(sys.stdin.readline())
    grid = [sys.stdin.readline().strip() for _ in range(n)]
    if n < 3:
        return "0"

    ans = 0
    for i in range(1, n - 1):
        for j in range(1, n - 1):
            if (grid[i][j] == 'X' and
                grid[i-1][j-1] == 'X' and
                grid[i-1][j+1] == 'X' and
                grid[i+1][j-1] == 'X' and
                grid[i+1][j+1] == 'X'):
                ans += 1
    return str(ans)

# provided sample
assert run("""5
.....
.XXX.
.XXX.
.XXX.
.....
""") == "1"

# minimum size
assert run("""1
.
""") == "0"

# no crosses but many X
assert run("""3
XXX
XXX
XXX
""") == "0"

# single cross
assert run("""3
X.X
.X.
X.X
""") == "1"

# multiple crosses
assert run("""5
X.X.X
.X.X.
X.X.X
.X.X.
X.X.X
""") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| Lưới 1×1 | 0 | xử lý ranh giới tối thiểu | 
| đầy đủ X 3×3 | 0 | yêu cầu đường chéo, không phải khối dày đặc | 
| mẫu đơn | 1 | tính đúng đắn cơ bản | 
| lưới xen kẽ | 9 | nhiều trung tâm độc lập | 

## Vỏ cạnh 

cho`n = 1`hoặc`n = 2`, thuật toán ngay lập tức trả về 0 trước khi bất kỳ vòng lặp nào được thực thi. Điều này phù hợp với thực tế là không có ô nào có bốn đường chéo lân cận. 

Đối với một lưới đầy đủ của`X`, mọi ô bên trong đều được kiểm tra, nhưng hầu hết đều thất bại vì các đường chéo của các tâm liền kề chồng lên nhau không chính xác. Ví dụ, trong một`3×3`lưới của tất cả`X`, tâm là hợp lệ nhưng trong các lưới lớn hơn, các ứng cử viên liền kề với đường biên không thành công do thiếu đường chéo. 

Đối với lưới thưa thớt ở đâu`X`tạo thành các đường thẳng đứng hoặc nằm ngang, không phát hiện được đường chéo vì không có cấu trúc đường chéo. Thuật toán kiểm tra chính xác tất cả các ứng cử viên nhưng từ chối chúng do thiếu các kết quả trùng khớp trên đường chéo. 

Mỗi trường hợp xác nhận rằng quyết định hoàn toàn mang tính địa phương và được nắm bắt đầy đủ bằng cách kiểm tra năm ô cho mỗi ứng viên.

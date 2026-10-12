---
title: "CF 105017K - Đếm các ô vuông"
description: "Chúng ta có một lưới được hình thành bởi các ô vuông đơn vị, có chiều cao $N$ và chiều rộng $M$. Bên trong lưới này, chúng tôi muốn đếm xem có bao nhiêu hình vuông thẳng hàng với trục, xem xét tất cả các kích thước có thể có."
date: "2026-06-28T02:10:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105017
codeforces_index: "K"
codeforces_contest_name: "Winter Cup 4.0 Online Mirror Contest"
rating: 0
weight: 105017
solve_time_s: 41
verified: true
draft: false
---

[CF 105017K - Đếm các ô vuông](https://codeforces.com/problemset/problem/105017/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới được hình thành bởi các ô vuông đơn vị, với chiều cao$N$và chiều rộng$M$. Bên trong lưới này, chúng tôi muốn đếm xem có bao nhiêu hình vuông thẳng hàng với trục, xem xét tất cả các kích thước có thể có. Một hình vuông cạnh$k$hợp lệ nếu nó nằm hoàn toàn bên trong$N \times M$bảng, và chúng tôi đếm mọi vị trí riêng biệt nơi có thể đặt một hình vuông như vậy. 

Vì vậy, nhiệm vụ không chỉ là tìm hình vuông lớn nhất hoặc kiểm tra sự tồn tại mà là tổng hợp tất cả các kích thước hình vuông có thể có và tất cả các vị trí hợp lệ. 

Các ràng buộc về kích thước đầu vào cho phép$N, M \le 10^6$. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng liệt kê các ô vuông một cách rõ ràng. Ngay cả một lần lặp lại trên tất cả các ô lưới cũng được, nhưng bất cứ điều gì cố gắng xem xét từng ô vuông riêng lẻ sẽ bùng nổ$O(N^2 M^2)$trong trường hợp xấu nhất vượt xa giới hạn khả thi. 

Một dạng lỗi phổ biến ở đây là xử lý từng kích thước ô vuông một cách độc lập nhưng vẫn lặp lại tất cả các vị trí một cách đơn giản. Ví dụ, đối với mỗi$k$, lặp đi lặp lại tất cả$(i, j)$và kiểm tra tính hợp lệ dẫn đến$O(NM \cdot \min(N,M))$, vốn đã trở nên quá lớn khi cả hai chiều đều lớn. 

Một vấn đề tế nhị khác là tràn. Số lượng hình vuông tăng theo khối theo kích thước, gần như theo thứ tự$N^3$, do đó các phép tính trung gian phải sử dụng số nguyên 64 bit. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: cố định kích thước hình vuông$k$, sau đó trượt một$k \times k$cửa sổ trên lưới và đếm tất cả các vị trí hợp lệ. Đối với một cố định$k$, có$(N-k+1)(M-k+1)$các vị trí. Tổng hợp điều này trên tất cả$k$từ$1$ĐẾN$\min(N, M)$đưa ra câu trả lời đúng Đây là khái niệm đơn giản và đã gần với công thức cuối cùng. 

Tuy nhiên, tính toán trực tiếp điều này dưới dạng các vòng lặp lồng nhau$k$,$i$, Và$j$thoái hóa thành ba vòng lặp lồng nhau. Tổng công việc trở thành$$\sum_{k=1}^{\min(N,M)} (N-k+1)(M-k+1),$$điều này tốt về mặt toán học nhưng đắt tiền nếu được đánh giá lặp đi lặp lại một cách ngây thơ. 

Quan sát quan trọng là số lượng cho mỗi kích thước hình vuông là độc lập và tuân theo một dạng khép kín. Thay vì lặp lại tất cả các vị trí, chúng tôi tính toán trực tiếp số lượng vị trí tồn tại cho mỗi kích thước và tính tổng chúng. Điều này loại bỏ hoàn toàn các vòng lặp bên trong và giảm vấn đề xuống còn một đường tuyến tính duy nhất trên các độ dài cạnh có thể có. 

Sau đó chúng ta nhận ra rằng toàn bộ câu trả lời chỉ là sự tổng kết$k$, có thể được tính bằng$O(\min(N,M))$thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NM \cdot \min(N,M))$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\min(N,M))$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định kích thước giới hạn$L = \min(N, M)$. Đây là độ dài cạnh tối đa có thể có của bất kỳ hình vuông nào. Bất kỳ hình vuông lớn hơn nào cũng không thể vừa với lưới dọc theo ít nhất một chiều. 
2. Đối với mỗi kích thước hình vuông có thể$k$từ$1$ĐẾN$L$, tính số lượng vị trí hợp lệ. Một hình vuông có kích thước$k$có thể bắt đầu ở bất kỳ hàng nào từ$1$ĐẾN$N-k+1$và bất kỳ cột nào từ$1$ĐẾN$M-k+1$, vậy số vị trí là$(N-k+1)(M-k+1)$. 
3. Tích lũy giá trị này vào biến trả lời. Mỗi thuật ngữ đóng góp số lượng hình vuông riêng biệt có kích thước đó. 
4. Sau khi xử lý xong tất cả$k$, xuất ra tổng tích lũy. 

Lý do phép tính tổng trực tiếp này hoạt động hiệu quả là vì mỗi ô vuông được xác định duy nhất bởi góc trên bên trái và độ dài cạnh của nó, do đó không có sự trùng lặp hoặc đếm kép trong các lần lặp. 

### Tại sao nó hoạt động 

Mỗi ô vuông hợp lệ trong lưới được xác định duy nhất bởi hai lựa chọn: độ dài cạnh của nó$k$và tọa độ trên cùng bên trái của nó$(i, j)$. Đối với một cố định$k$, có hiệu lực$i$phạm vi độc lập với hợp lệ$j$và cả hai phạm vi đều liền kề nhau và được xác định đầy đủ bởi các ranh giới lưới. Thuật toán liệt kê mọi cặp hợp lệ$(k, i, j)$chính xác một lần thông qua số lượng được tính theo hệ số, do đó không có cấu hình nào bị bỏ sót và không có cấu hình nào được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N, M = map(int, input().split())

L = min(N, M)

ans = 0
for k in range(1, L + 1):
    ans += (N - k + 1) * (M - k + 1)

print(ans)
```Việc thực hiện trực tiếp theo công thức tính dẫn xuất. Biến vòng lặp`k`biểu thị độ dài cạnh hình vuông. Đối với mỗi`k`,`(N - k + 1)`đếm xem hình vuông có thể chiếm bao nhiêu vị trí thẳng đứng và`(M - k + 1)`đếm các vị trí nằm ngang. Sản phẩm của họ đưa ra số lượng vị trí. 

Một chi tiết tinh tế nhưng quan trọng là vòng lặp phải dừng ở`min(N, M)`. Tiếp tục vượt quá mức đó sẽ tạo ra những đóng góp tiêu cực, vô nghĩa và sẽ làm sai lệch số tiền. 

Tất cả số học được thực hiện bằng cách sử dụng số nguyên Python, xử lý các giá trị lớn một cách tự nhiên mà không lo tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
```Chúng tôi tính toán$L = 2$. 

| k | N-k+1 | M-k+1 | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 2 | 3 | 6 | 
| 2 | 1 | 2 | 2 | 

Tổng số tiền = 8 

Điều này xác nhận rằng ngay cả một lưới nhỏ cũng đã tạo ra nhiều kích thước hình vuông, với kích thước 1 chiếm ưu thế về số lượng nhưng kích thước 2 vẫn đóng góp các vị trí hợp lệ. 

### Ví dụ 2 

đầu vào:```
4 4
```Chúng tôi tính toán$L = 4$. 

| k | N-k+1 | M-k+1 | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 4 | 4 | 16 | 
| 2 | 3 | 3 | 9 | 
| 3 | 2 | 2 | 4 | 
| 4 | 1 | 1 | 1 | 

Tổng số tiền = 30 

Dấu vết này cho thấy các hình vuông lớn hơn đóng góp dần dần ít vị trí hơn, tạo thành mô hình bậc hai giảm dần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\min(N, M))$| một vòng lặp trên tất cả các kích thước hình vuông có thể có | 
| Không gian |$O(1)$| chỉ là tổng chạy và các hằng số | 

Ràng buộc$N, M \le 10^6$vượt qua tuyến tính$\min(N,M)$chấp nhận được. Tổng số$10^6$các lần lặp dễ dàng nằm trong giới hạn trong Python khi mỗi lần lặp là một phép toán số học có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N, M = map(int, input().split())
    L = min(N, M)
    ans = 0
    for k in range(1, L + 1):
        ans += (N - k + 1) * (M - k + 1)
    return str(ans)

# provided samples
assert run("2 3\n") == "8"
assert run("4 4\n") == "30"

# custom cases
assert run("1 1\n") == "1", "single cell grid"
assert run("1 5\n") == "5", "only 1x1 squares possible"
assert run("2 2\n") == "5", "small symmetric grid"
assert run("3 5\n") == str(sum((3-k+1)*(5-k+1) for k in range(1,4))), "formula consistency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | lưới tối thiểu | 
| 1 5 | 5 | hình chữ nhật suy biến | 
| 2 2 | 5 | tính đối xứng và tính đúng đắn của chữ thường | 
| 3 5 | dựa trên công thức | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một chiều là 1. Đối với đầu vào`1 5`, chỉ tồn tại các ô vuông có kích thước 1. Bộ thuật toán$L = 1$, vậy chỉ$k = 1$được xử lý, sản xuất$(1-1+1)(5-1+1) = 5$, phù hợp với thực tế là mọi ô đều là hình vuông 1×1 hợp lệ. 

Một trường hợp cạnh khác là lưới nhỏ nhất có thể`1 1`. Đây$L = 1$, và đóng góp duy nhất là$(1)(1) = 1$. Không có lần lặp lớn hơn xảy ra, do đó không có giá trị âm không hợp lệ nào được xem xét. 

Đối với lưới vuông như`4 4`, tất cả độ dài các cạnh từ 1 đến 4 đóng góp số lượng nhỏ dần. Thuật toán xử lý việc này một cách tự nhiên mà không cần cách viết hoa đặc biệt vì hệ số$(N-k+1)(M-k+1)$vẫn có hiệu lực trên toàn bộ phạm vi.

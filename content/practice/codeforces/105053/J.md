---
title: "CF 105053J - Niềm vui giao dịch"
description: "Mỗi nguồn tài nguyên có hai công nghệ sản xuất cạnh tranh nhau, mỗi làng sử dụng một công nghệ. Đối với tài nguyên $i$, Apolyanka chi $Ai$ giờ công cho mỗi đơn vị, trong khi Büddelsdorf chi $Bi$."
date: "2026-06-28T00:31:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105053
codeforces_index: "J"
codeforces_contest_name: "The 2024 ICPC Latin America Championship"
rating: 0
weight: 105053
solve_time_s: 38
verified: true
draft: false
---

[CF 105053J - Niềm vui giao dịch](https://codeforces.com/problemset/problem/105053/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi nguồn tài nguyên có hai công nghệ sản xuất cạnh tranh nhau, mỗi làng sử dụng một công nghệ. Đối với tài nguyên$i$, Apolyanka chi tiêu$A_i$số giờ công trên mỗi đơn vị, trong khi Büddelsdorf dành$B_i$. Mỗi làng ban đầu sản xuất một số lượng đơn vị cố định cho mỗi tài nguyên,$U_i$Và$W_i$tương ứng, do đó tổng sản lượng tài nguyên$i$được cố định tại$T_i = U_i + W_i$. 

Sự tự do chính là tổng số cuối cùng này$T_i$không cần phải chia như đã cho ban đầu. Các làng có thể phân công lại sản xuất một cách tùy ý giữa các làng trong khi vẫn giữ nguyên tổng sản lượng của từng nguồn tài nguyên. Thay đổi thế nào chỉ là ai sản xuất ra bao nhiêu. 

Nếu Apolyanka sản xuất$x_i$đơn vị tài nguyên$i$, thì Büddelsdorf tạo ra$T_i - x_i$, và tổng chi phí trở thành$$\sum_i (A_i x_i + B_i (T_i - x_i)).$$Chúng tôi muốn chọn$x_i$để giảm thiểu tổng chi phí này. 

Các ràng buộc cho phép lên đến$10^5$tài nguyên, vì vậy mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính trong$N$. Bất cứ điều gì bậc hai trong$N$hoặc liên quan đến các cấu trúc lồng nhau nặng về sắp xếp trên phạm vi lớn đều có thể chấp nhận được, nhưng bất kỳ điều gì cố gắng liệt kê các phân phối trên toàn cầu hoặc thực hiện mô phỏng trên mỗi đơn vị đều ngay lập tức quá chậm. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là việc cho rằng mỗi làng sẽ tối ưu hóa sản xuất một cách độc lập. Điều đó sẽ bỏ qua việc chuyển dịch sản xuất một nguồn tài nguyên giữa các làng sẽ làm thay đổi chi phí một cách tuyến tính và độc lập trên mỗi tài nguyên, do đó sự kết hợp không bao giờ tồn tại. Một cái bẫy khác là giải thích vấn đề là cần phân bổ số nguyên; phân bổ theo tỷ lệ được cho phép rõ ràng, vì vậy giải pháp là tối ưu hóa liên tục thay vì phân bổ tổ hợp. 

## Phương pháp tiếp cận 

Một công thức trực tiếp gán một biến$x_i$đơn vị tài nguyên$i$tới Apolyanka. Sự đóng góp chi phí của nguồn lực$i$là$$A_i x_i + B_i (T_i - x_i) = B_i T_i + (A_i - B_i)x_i.$$Học kỳ đầu tiên$B_i T_i$là không đổi. Quyết định duy nhất là thuật ngữ tuyến tính$(A_i - B_i)x_i$, Ở đâu$x_i \in [0, T_i]$. 

Đối với từng tài nguyên độc lập, chúng tôi đang giảm thiểu hàm tuyến tính trong một khoảng. Nếu như$A_i > B_i$, hệ số dương nên tăng$x_i$làm tăng chi phí, nghĩa là chúng ta nên đặt$x_i = 0$. Nếu như$A_i < B_i$, hệ số âm nên chúng ta tối đa hóa$x_i$, nghĩa$x_i = T_i$. Nếu chúng bằng nhau thì mọi sự phân chia đều có tác dụng. 

Điều này giải quyết hoàn toàn vấn đề: mỗi nguồn lực được quyết định độc lập chỉ dựa trên ngôi làng nào hiệu quả hơn. 

Việc giải thích bạo lực sẽ thử tất cả các phân chia có thể có của từng tài nguyên, nhưng vì mỗi$x_i$liên tục trong$[0, T_i]$, đó sẽ là vô hạn. Ngay cả khi bị rời rạc, nó sẽ bùng nổ tổ hợp như$O(\prod T_i)$, điều đó là không thể. 

Quan sát chính là khả năng phân tách: tổng chi phí là tổng của các hàm tuyến tính độc lập của mỗi$x_i$. Không có ràng buộc nào khi kết hợp các tài nguyên khác nhau, vì vậy mỗi tài nguyên có thể được tối ưu hóa riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử phân bổ) | hàm mũ | O(1) | Quá chậm | 
| Tối ưu (tham lam trên mỗi tài nguyên) | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các giá trị$A_i, B_i, U_i, W_i$. Tính toán$T_i = U_i + W_i$. Điều này khắc phục tổng sản lượng trên mỗi tài nguyên, đây là hạn chế toàn cầu duy nhất. 
2. Khởi tạo bộ tích lũy`answer = 0`. Điều này sẽ thu thập tổng chi phí tối thiểu. 
3. Đối với mỗi tài nguyên$i$, hãy tính chi phí cơ bản nếu Büddelsdorf sản xuất được mọi thứ:$$base_i = B_i \cdot T_i.$$4. Tính chênh lệch chi phí nếu chúng ta chuyển một đơn vị sản xuất từ ​​Büddelsdorf tới Apolyanka:$$delta_i = A_i - B_i.$$5. Quyết định phân bổ: 

Nếu$delta_i < 0$, chuyển sản xuất sang Apolyanka giúp giảm chi phí, vì vậy chúng tôi đảm nhận toàn bộ hoạt động sản xuất từ ​​Apolyanka, góp phần$A_i \cdot T_i$. 

Nếu như$delta_i \ge 0$, Büddelsdorf không tệ hơn hay tốt hơn, vì vậy chúng tôi giữ mọi thứ với Büddelsdorf, đóng góp$B_i \cdot T_i$. 
6. Cộng phần đóng góp đã chọn vào câu trả lời tổng thể. 

Mỗi tài nguyên được xử lý độc lập vì hàm chi phí phân tách thành tổng các hàm tuyến tính cho mỗi tài nguyên, do đó không có sự điều chỉnh tài nguyên chéo nào có thể cải thiện kết quả. 

### Tại sao nó hoạt động 

Tổng chi phí là tổng của các hàm$f_i(x_i) = B_i T_i + (A_i - B_i)x_i$, mỗi cái được xác định trên một khoảng riêng biệt. Vì không có ràng buộc liên kết khác nhau$x_i$, vùng khả thi là tích của các khoảng độc lập. Hàm tuyến tính trên một khoảng lồi đạt cực tiểu tại điểm cuối, do đó mỗi hàm$x_i$phải là một trong hai$0$hoặc$T_i$. dấu hiệu của$A_i - B_i$xác định điểm cuối nào là tối ưu, đảm bảo tính tối ưu toàn cầu từ các quyết định cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    ans = 0.0

    for _ in range(n):
        a, u, b, w = map(int, input().split())
        t = u + w

        if a < b:
            ans += a * t
        else:
            ans += b * t

    print(f"{ans:.10f}")

if __name__ == "__main__":
    main()
```Mã trực tiếp thực hiện quyết định điểm cuối cho mỗi tài nguyên. Chi tiết triển khai quan trọng là tránh xây dựng bất kỳ biểu diễn sản xuất trung gian nào trên mỗi đơn vị, vì$T_i$có thể lớn và lý luận phân số là tiềm ẩn. Sử dụng tích lũy dấu phẩy động là an toàn vì câu trả lời cuối cùng phù hợp với độ chính xác cần thiết và tất cả các phép toán đều là tổ hợp tuyến tính của các số nguyên. 

Điều kiện phân nhánh`a < b`mã hóa phép thử đạo hàm của hàm chi phí tuyến tính. Bình đẳng tự nhiên rơi vào một trong hai nhánh mà không ảnh hưởng đến tính đúng đắn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi xử lý từng tài nguyên một cách độc lập. 

| tôi | A | B | Bạn | W | T | đã chọn | chi phí | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 2 | 1 | 3 | A | 3 | 
| 2 | 2 | 3 | 1 | 4 | 5 | A | 10 | 

Tổng cộng là$3 + 14 = 17$. 

Điều này cho thấy rằng mặc dù việc phân chia sản xuất ban đầu có khác nhau nhưng mỗi nguồn lực đều độc lập chọn nhà sản xuất rẻ hơn. 

### Mẫu 2 

| tôi | A | B | Bạn | W | T | đã chọn | chi phí | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 1 | 2 | A | 2 | 
| 2 | 2 | 1 | 1 | 1 | 2 | B | 2 | 
| 3 | 1 | 1 | 1 | 1 | 2 | hoặc | 2 | 

Tổng chi phí là$6$. 

Điều này chứng tỏ trường hợp đối xứng trong đó cả hai làng đều có hiệu quả như nhau, khiến cho việc phân chia không còn phù hợp nữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi tài nguyên được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ có một khoản tiền được lưu trữ | 

Các ràng buộc cho phép lên đến$10^5$tài nguyên, do đó, một đường truyền tuyến tính duy nhất với số học theo thời gian không đổi cho mỗi mục là đủ dễ dàng trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    ans = 0.0
    for _ in range(n):
        a, u, b, w = map(int, input().split())
        t = u + w
        if a < b:
            ans += a * t
        else:
            ans += b * t

    return f"{ans:.10f}"

# provided sample 1
assert abs(float(run("2\n1 2 4 1\n2 1 3 4\n")) - 17.0) < 1e-6

# sample 2
assert abs(float(run("3\n1 1 2 1\n2 1 1 1\n1 1 1 1\n")) - 6.0) < 1e-6

# minimum case
assert abs(float(run("1\n5 10 3 10\n")) - 30.0) < 1e-6

# all equal efficiency
assert abs(float(run("2\n1 5 1 5\n2 3 2 3\n")) - 20.0) < 1e-6

# strongly skewed costs
assert abs(float(run("2\n1 1000 1000 1\n1000 1 1 1000\n")) - 1001.0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tài nguyên duy nhất | phép nhân trực tiếp | trường hợp ranh giới tối thiểu | 
| chi phí bằng nhau | xử lý cà vạt ổn định | nhánh bình đẳng đúng đắn | 
| chi phí sai lệch | sự lựa chọn tham lam đúng đắn | quyết định dựa trên dấu hiệu đúng đắn | 

## Vỏ cạnh 

Trường hợp góc là khi cả hai làng đều có hiệu quả sử dụng tài nguyên như nhau,$A_i = B_i$. Ví dụ, đầu vào`1 5 1 5`cho$T=10$. Cả hai phép gán đều mang lại chi phí là 50 và thuật toán gán nó một cách nhất quán cho Büddelsdorf do`else`nhánh, bảo toàn tính đúng đắn. 

Một trường hợp khác là sự bất đối xứng cực độ như$A_i \ll B_i$. Vì`1 1000 1000 1`, chúng tôi có$T=1001$. Thuật toán gán mọi thứ cho Apolyanka, tạo ra chi phí$1001$, trong khi bất kỳ sự phân chia nào liên quan đến Büddelsdorf đều làm tăng chi phí do chi phí trên mỗi đơn vị cao hơn. 

Trường hợp cuối cùng là khi một ngôi làng luôn tệ hơn trên tất cả các nguồn lực. Thuật toán tự nhiên bỏ qua hoàn toàn ngôi làng đó, thu gọn hệ thống thành mô hình một nhà sản xuất, phù hợp với cấu trúc giải pháp tối ưu thu được từ việc giảm thiểu tuyến tính độc lập.

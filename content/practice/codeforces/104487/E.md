---
title: "CF 104487E - Tỷ lệ thú vị"
description: "Mỗi trường hợp thử nghiệm mô tả một quá trình tải xuống hoàn thành theo đúng n bước bằng nhau, từ 0 đến n. Tại một số điểm trung gian x, trong đó 1 ≤ x ≤ n - 1, quá trình tải xuống đã hoàn thành x/n và còn lại (n - x)/n."
date: "2026-06-30T12:38:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104487
codeforces_index: "E"
codeforces_contest_name: "Tishreen + SVU CPC 2023"
rating: 0
weight: 104487
solve_time_s: 68
verified: true
draft: false
---

[CF 104487E - Tỷ lệ thú vị](https://codeforces.com/problemset/problem/104487/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một quá trình tải xuống hoàn thành chính xác`n`bước bằng nhau, từ`0`ĐẾN`n`. Tại một điểm trung gian nào đó`x`, Ở đâu`1 ≤ x ≤ n - 1`, quá trình tải xuống diễn ra tại`x/n`hoàn thành và`(n - x)/n`còn lại. Chúng ta được yêu cầu xét tỷ lệ giữa phần đã hoàn thành và phần còn lại, tức là`x : (n - x)`, giảm nó và chỉ giữ giá trị nguyên của tỷ lệ này khi nó trở thành số nguyên. 

Đối với một vị trí cố định`x`, tỷ lệ là số nguyên chính xác khi`x / (n - x)`là một số nguyên. Mỗi hợp lệ như vậy`x`đóng góp giá trị tỷ lệ giảm của nó vào một tổng. Nhiệm vụ là tính tổng này cho mọi`n`. 

Các ràng buộc cho phép lên đến`10^5`trường hợp thử nghiệm và mỗi`n`có thể lớn như`10^6`. Điều này ngay lập tức loại trừ việc kiểm tra mọi`x`từ`1`ĐẾN`n - 1`cho mỗi truy vấn, vì điều đó sẽ dẫn đến khoảng`10^11`hoạt động trong trường hợp xấu nhất. Ngay cả cách tiếp cận logarit theo từng bước trên tất cả các vị trí vẫn sẽ quá chậm trừ khi được xử lý trước nhiều. 

Trường hợp cạnh tinh tế xuất hiện khi`n`là nguyên tố. Khi đó chỉ tồn tại các ước số tầm thường và chúng ta phải đảm bảo logic không vô tình đếm các vị trí không hợp lệ. Ví dụ, khi`n = 5`, các vị trí hợp lệ phải được suy ra một cách cẩn thận thay vì giả sử tồn tại nhiều tỷ số nội tại. Một mô phỏng ngây thơ có thể thử tất cả`x`và bỏ lỡ cấu trúc mà chỉ những điều kiện đại số cụ thể mới quan trọng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực lặp đi lặp lại trên mọi vị trí`x`từ`1`ĐẾN`n - 1`, tính tỉ số`x / (n - x)`, kiểm tra xem đó có phải là số nguyên hay không và nếu có thì hãy thêm nó vào câu trả lời. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, nó đòi hỏi`O(n)`làm việc trên mỗi trường hợp thử nghiệm, dẫn đến`O(nT)`về tổng thể, điều này vượt xa giới hạn khả thi khi cả hai`n`Và`T`lớn. 

Sự đơn giản hóa chính xuất phát từ việc viết lại điều kiện tích phân. Tỷ lệ`x / (n - x)`là một số nguyên`k`nếu và chỉ khi`x = k(n - x)`. Sắp xếp lại mang lại`x(1 + k) = kn`, Vì thế`x = kn / (k + 1)`. Để điều này có hiệu lực,`(k + 1)`phải chia`kn`. Từ`k`Và`k + 1`là nguyên tố cùng nhau, lực này`(k + 1)`chia`n`. 

Điều này chuyển đổi vấn đề từ việc lặp lại các vị trí`x`để lặp lại các ước của`n`. Mỗi ước số trực tiếp tương ứng với chính xác một giá trị tỷ lệ hợp lệ, làm cho nghiệm chỉ phụ thuộc vào các hàm lý thuyết số của`n`, không phải phạm vi tuyến tính của nó. 

Từ đó, câu trả lời rút gọn thành một biểu thức đơn giản liên quan đến tổng và số chia, cho phép xử lý trước đầy đủ tất cả các giá trị lên đến`10^6`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nT) | O(1) | Quá chậm | 
| Tính toán trước dựa trên số chia | O(N log N + T) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành số học chia và tính toán trước mọi thứ một lần cho tất cả`n`đến giới hạn tối đa. 

1. Tính trước tổng các ước số`sigma(n)`cho mọi`n`lên đến`10^6`. Điều này được thực hiện bằng cách sử dụng cách tiếp cận kiểu sàng trong đó mỗi số nguyên đóng góp cho tất cả các bội số của nó. Điều này là cần thiết vì mọi tỷ lệ hợp lệ đều phụ thuộc vào cấu trúc ước số hơn là các vị trí riêng lẻ. 
2. Tính trước số ước`tau(n)`cho mọi`n`sử dụng cùng một sàng tích lũy. Mỗi lần chúng tôi thêm một đóng góp cho mỗi lần xuất hiện của số chia. 
3. Với mỗi test case, hãy đọc`n`và tính toán câu trả lời bằng cách sử dụng danh tính`answer = sigma(n) - tau(n)`. 
4. Xuất kết quả ngay lập tức cho mỗi truy vấn. 

Lý do bước 3 hợp lệ xuất phát từ việc ánh xạ từng tỷ lệ hợp lệ thành ước số`d`của`n`Ở đâu`d ≥ 2`và mỗi ước số như vậy đóng góp chính xác`(d - 1)`đến tổng số. 

### Tại sao nó hoạt động 

Mọi tỷ lệ hợp lệ đều tương ứng với một giá trị`k`như vậy`x / (n - x) = k`. Lực lượng này`x = kn / (k + 1)`, chỉ tích phân khi`(k + 1)`chia rẽ`n`. Viết`d = k + 1`, mỗi ước số`d ≥ 2`tạo ra chính xác một vị trí hợp lệ và đóng góp`d - 1`đến tổng số. Tổng tất cả các ước số như vậy tương đương với`sum_{d|n, d≥2}(d - 1)`, đơn giản hóa về mặt đại số thành`sigma(n) - tau(n)`. 

Thuật toán này đúng vì nó thay thế điều kiện dựa trên vị trí bằng phép đối chiếu chính xác giữa các tỷ lệ hợp lệ và ước số của`n`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 10**6

sigma = [0] * (MAXN + 1)
tau = [0] * (MAXN + 1)

for i in range(1, MAXN + 1):
    for j in range(i, MAXN + 1, i):
        sigma[j] += i
        tau[j] += 1

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str(sigma[n] - tau[n]))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Các vòng lặp tiền xử lý xây dựng tổng số chia và số đếm theo cách sàng cổ điển. Đối với mọi`i`, nó đóng góp vào mọi bội số`j`, tích lũy cả giá trị của nó vào`sigma[j]`và một người đếm vào`tau[j]`. 

Bước truy vấn là thời gian không đổi cho mỗi trường hợp thử nghiệm. Phép trừ`sigma[n] - tau[n]`trực tiếp triển khai công thức dẫn xuất, tránh mọi tính toán lại trong quá trình truy vấn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét`n = 6`. 

Tỷ lệ hợp lệ đến từ các ước có 6 lớn hơn 1, tức là 2, 3 và 6. 

| ước số d | tỷ lệ đóng góp (d - 1) | 
| --- | --- | 
| 2 | 1 | 
| 3 | 2 | 
| 6 | 5 | 

Tổng số tiền là`1 + 2 + 5 = 8`. 

Điều này phù hợp`sigma(6) = 1 + 2 + 3 + 6 = 12`Và`tau(6) = 4`, Vì thế`12 - 4 = 8`. 

### Ví dụ 2 

Hãy xem xét`n = 10`. 

Các ước lớn hơn 1 là 2, 5, 10. 

| ước số d | đóng góp | 
| --- | --- | 
| 2 | 1 | 
| 5 | 4 | 
| 10 | 9 | 

Tổng cộng là`14`. 

Lại,`sigma(10) = 18`,`tau(10) = 4`, Vì thế`18 - 4 = 14`. 

Những dấu vết này cho thấy rằng mỗi ước số đóng góp độc lập và không tồn tại sự tương tác giữa các vị trí sau khi áp dụng phép biến đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + T) | sự tích lũy giống như sàng trên tất cả các ước số cộng với các truy vấn có thời gian không đổi | 
| Không gian | O(N) | mảng cho tổng và số chia | 

Chi phí tiền xử lý có thể chấp nhận được đối với`N = 10^6`và mỗi truy vấn được trả lời theo thời gian không đổi, giúp giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

MAXN = 10**6
sigma = [0] * (MAXN + 1)
tau = [0] * (MAXN + 1)

for i in range(1, MAXN + 1):
    for j in range(i, MAXN + 1, i):
        sigma[j] += i
        tau[j] += 1

def solve_case(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        res.append(str(sigma[n] - tau[n]))
    return "\n".join(res)

# custom small cases
assert solve_case("1\n1\n") == "0"
assert solve_case("1\n2\n") == "1"
assert solve_case("1\n6\n") == "8"
assert solve_case("1\n10\n") == "14"

# multiple tests
assert solve_case("3\n6\n10\n12\n") == "\n".join([
    str(sigma[6]-tau[6]),
    str(sigma[10]-tau[10]),
    str(sigma[12]-tau[12])
])
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 1 | 0 | không có tiến trình trung gian hợp lệ | 
| n = 2 | 1 | trường hợp không tầm thường nhỏ nhất | 
| n = 6 | 8 | tổng hợp có nhiều ước số | 
| n = 10,12 | tính toán | tính nhất quán của nhiều truy vấn | 

## Vỏ cạnh 

cho`n = 1`, không có vị trí trung gian nào cả nên tổng phải bằng 0. Công thức cho`sigma(1) - tau(1) = 1 - 1 = 0`, khớp với định nghĩa mà không cần xử lý đặc biệt. 

Đối với nguyên tố`n`, chẳng hạn như`n = 5`, chỉ tồn tại những đóng góp dựa trên số chia. Các ước số là`1`Và`5`, vậy chỉ`5`đóng góp, trao`(5 - 1) = 4`. Công thức cho`sigma(5) - tau(5) = (1 + 5) - 2 = 4`, xác nhận tính đúng đắn ngay cả khi không có cấu trúc bên trong tồn tại trong khoảng. 

Đối với các số tổng hợp cao như`n = 12`, nhiều ước đóng góp độc lập. Thuật toán xử lý việc này một cách tự nhiên vì mỗi ước số đóng góp chính xác một lần trong quá trình tiền xử lý, đảm bảo không tính quá mức hoặc thiếu phần đóng góp.

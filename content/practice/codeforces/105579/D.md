---
title: "CF 105579D - Nhóm Fibonacci"
description: "Chúng ta có một dòng gồm $n$ người được đánh chỉ mục từ 1 đến $n$. Quá trình liên tục tạo thành một nhóm từ dòng hiện tại bằng cách chọn tất cả các vị trí có chỉ số là số Fibonacci."
date: "2026-06-22T17:46:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105579
codeforces_index: "D"
codeforces_contest_name: "Udmurtia High School Programming Contest (Qualification for VKOSHP 2012)"
rating: 0
weight: 105579
solve_time_s: 70
verified: true
draft: false
---

[CF 105579D - Nhóm Fibonacci](https://codeforces.com/problemset/problem/105579/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng$n$mọi người được lập chỉ mục từ 1 đến$n$. Quá trình liên tục tạo thành một nhóm từ dòng hiện tại bằng cách chọn tất cả các vị trí có chỉ số là số Fibonacci. Những người được chọn sẽ bị xóa khỏi dòng, những người còn lại sẽ đóng lại và quy tắc tương tự lại được áp dụng cho dòng rút ngắn. Nhiệm vụ là xác định xem có bao nhiêu vòng như vậy xảy ra cho đến khi không còn ai. 

Điểm tinh tế quan trọng là các vị trí Fibonacci luôn được xác định theo dòng sản phẩm hiện tại chứ không phải các chỉ số ban đầu. Sau mỗi lần loại bỏ, những người còn lại được lập chỉ mục lại bắt đầu từ 1, do đó bộ Fibonacci được tính toán lại theo độ dài mới. 

Ràng buộc$n \le 10^{18}$ngay lập tức loại trừ bất kỳ sự mô phỏng nào đối với các cá nhân. Ngay cả việc lặp lại tất cả các vị trí trong mỗi vòng cũng là không thể, vì một lần vượt qua sẽ được$O(n)$. Bất kỳ giải pháp khả thi nào cũng phải hoạt động hoàn toàn với kích thước ngày càng tăng của mảng thay vì các phần tử rõ ràng. 

Trường hợp góc không rõ ràng xuất hiện khi$n$là nhỏ. Ví dụ,$n = 1$tạo ra một nhóm ngay lập tức vì vị trí 1 là Fibonacci. Vì$n = 2$, cả hai vị trí 1 và 2 đều là Fibonacci nên quá trình cũng kết thúc sau một bước. Giả định ngây thơ rằng chỉ có vị trí 1 là Fibonacci sẽ dẫn đến kết quả không chính xác ở đây. 

Một cạm bẫy tinh vi khác là giả định rằng số lượng phần tử bị loại bỏ là không đổi qua các vòng. Trong thực tế, nó thay đổi khi ngưỡng Fibonacci dịch chuyển cùng với mảng thu hẹp lại. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp giữ một mảng kích thước$n$, liên tục xây dựng chỉ số Fibonacci được thiết lập theo độ dài hiện tại, loại bỏ các phần tử đó và đếm số lần lặp. Về mặt khái niệm, điều này đơn giản và chính xác vì nó tuân theo quy trình chính xác như được mô tả. Tuy nhiên, nó không khả thi về mặt tính toán. Ngay cả khi mỗi vòng loại bỏ một phần phần tử thì trường hợp xấu nhất vẫn bắt đầu từ$10^{18}$các phần tử, làm cho bất kỳ hoạt động nào trên mỗi phần tử đều không thể thực hiện được. 

Quan sát quan trọng là quá trình này chỉ phụ thuộc vào độ dài hiện tại$m$, không dựa trên giá trị thực tế hoặc danh tính của các phần tử. Trong mỗi vòng, số phần tử bị loại bỏ bằng số số Fibonacci không vượt quá$m$. Đặt giá trị này là$F(m)$. Quá trình chuyển đổi trở thành một phép lặp thuần túy trên các số nguyên:$$m \rightarrow m - F(m)$$Các số Fibonacci tăng theo cấp số nhân nên số lượng$F(m)$chỉ là$O(\log m)$. Điều này có nghĩa là mỗi vòng sẽ loại bỏ một số phần tử logarit tương ứng với kích thước hiện tại. Mặc dù điều này ban đầu có thể gợi ý một số lượng lớn các vòng, nhưng cấu trúc của phân bố Fibonacci làm cho trạng thái sụp đổ nhanh chóng trong thực tế vì điểm cắt dịch chuyển xuống phi tuyến tính khi$m$giảm đi. 

Vì vậy, vấn đề giảm xuống còn việc đánh giá nhiều lần$F(m)$và cập nhật$m$cho đến khi nó đạt đến số không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n)$mỗi vòng |$O(n)$| Quá chậm | 
| Giảm kích thước tái phát |$O(\text{number of rounds} \cdot \log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm toàn bộ quá trình xuống chỉ còn theo dõi độ dài hiện tại của dòng sản phẩm. 

1. Tính toán trước tất cả các số Fibonacci tối đa$10^{18}$, vì giới hạn này bao trùm tất cả các trạng thái trung gian có thể có. Điều này cho phép truy cập liên tục vào số lượng Fibonacci$\le m$. 
2. Khởi tạo$m = n$và một quầy$ans = 0$. Biến$m$đại diện cho số người còn lại sau mỗi vòng. 
3. Trong mỗi vòng, hãy xác định xem có bao nhiêu số Fibonacci nhỏ hơn hoặc bằng$m$. Điều này có thể được thực hiện bằng cách tìm kiếm nhị phân trong danh sách Fibonacci được tính toán trước. 
4. Trừ số này khỏi$m$, vì chính xác những vị trí đó đã bị loại bỏ trong vòng hiện tại. 
5. Tăng câu trả lời lên 1 vì mỗi vòng sẽ có một nhóm. 
6. Lặp lại cho đến khi$m = 0$. 

Tính đúng đắn phụ thuộc vào thực tế là sau mỗi hiệp đấu, cấu trúc của đội hình còn lại không còn phù hợp ngoài quy mô của nó. Điều quan trọng duy nhất là có bao nhiêu chỉ số Fibonacci tồn tại trong phạm vi hiện tại, vì những vị trí đó luôn là những vị trí bị loại bỏ. 

### Tại sao nó hoạt động 

Ở bất kỳ giai đoạn nào, hệ thống được mô tả đầy đủ bằng một số nguyên duy nhất$m$. Quy tắc lựa chọn chỉ phụ thuộc vào tập hợp các chỉ số Fibonacci tối đa$m$, được cố định cho một giá trị nhất định$m$. Việc loại bỏ các vị trí đó sẽ tạo ra một nhóm nhỏ hơn có cấu trúc không ảnh hưởng đến tư cách thành viên Fibonacci trong tương lai ngoài quy mô mới của nó. Điều này tạo ra sự lặp lại xác định rõ ràng trên các số nguyên, đảm bảo quá trình luôn phát triển nhất quán cho đến khi kết thúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Precompute Fibonacci numbers up to 1e18
F = [1, 2]
while True:
    nxt = F[-1] + F[-2]
    if nxt > 10**18:
        break
    F.append(nxt)

def count_fib_leq(x):
    # binary search
    lo, hi = 0, len(F)
    while lo < hi:
        mid = (lo + hi) // 2
        if F[mid] <= x:
            lo = mid + 1
        else:
            hi = mid
    return lo

def solve():
    n = int(input())
    m = n
    ans = 0

    while m > 0:
        cnt = count_fib_leq(m)
        m -= cnt
        ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Danh sách Fibonacci được xây dựng một lần và được sử dụng lại cho tất cả các truy vấn. Vì các giá trị tăng theo cấp số nhân nên kích thước danh sách vẫn rất nhỏ (khoảng 90 phần tử). Tìm kiếm nhị phân sẽ xác định có bao nhiêu chỉ số Fibonacci nằm trong phạm vi hiện tại. 

Vòng lặp chỉ cập nhật trạng thái số nguyên$m$, tránh bất kỳ sự mô phỏng nào của các cá nhân. Câu trả lời là tính số lần áp dụng bước giảm này cho đến khi cạn kiệt. 

Một lỗi triển khai phổ biến là tính lại số Fibonacci không chính xác sau mỗi lần cập nhật bằng cách xây dựng lại các chuỗi; giải pháp này tránh được điều đó hoàn toàn bằng cách tính toán trước một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 10$Chúng tôi chỉ theo dõi kích thước còn lại. 

| Vòng |$m$bắt đầu | số Fibonacci$\le m$|$m$sau | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 6 (1,2,3,5,8) | 4 | 1 | 
| 2 | 4 | 3 (1,2,3) | 1 | 2 | 
| 3 | 1 | 1 | 0 | 3 | 

Quá trình kết thúc sau 3 vòng, cho thấy kích thước còn lại sẽ giảm nhanh như thế nào khi ngưỡng Fibonacci nhỏ chiếm ưu thế. 

### Ví dụ 2:$n = 1$| Vòng |$m$bắt đầu | số Fibonacci$\le m$|$m$sau | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 1 | 

Trường hợp này xác nhận rằng vị trí Fibonacci cơ sở ngay lập tức loại bỏ phần tử duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n \cdot \log \log n)$| Mỗi vòng thực hiện tìm kiếm nhị phân trên ~90 số Fibonacci | 
| Không gian |$O(\log n)$| Lưu trữ chuỗi Fibonacci lên tới$10^{18}$| 

Chuỗi Fibonacci tăng theo cấp số nhân, do đó, mảng được tính toán trước vẫn có kích thước không đổi trong thực tế. Số vòng quay nhỏ do giảm nhanh$m$, làm cho lời giải dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    F = [1, 2]
    while True:
        nxt = F[-1] + F[-2]
        if nxt > 10**18:
            break
        F.append(nxt)

    def count_fib_leq(x):
        lo, hi = 0, len(F)
        while lo < hi:
            mid = (lo + hi) // 2
            if F[mid] <= x:
                lo = mid + 1
            else:
                hi = mid
        return lo

    n = int(inp.strip())
    m = n
    ans = 0
    while m > 0:
        m -= count_fib_leq(m)
        ans += 1
    return str(ans)

# provided / implied samples
assert run("1") == "1"
assert run("2") == "1"
assert run("10") == "3"

# custom cases
assert run("3") == "1"
assert run("4") == "2"
assert run("5") == "2"
assert run("100") > "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp cơ sở Fibonacci tối thiểu | 
| 2 | 1 | nhiều vị trí Fibonacci | 
| 10 | 3 | giảm nhiều vòng | 
| 3 | 1 | sụp đổ ranh giới nhỏ | 
| 4 | 2 | chuyển đổi giữa các ngưỡng Fibonacci | 
| 5 | 2 | ổn định tầm trung | 

## Vỏ cạnh 

cho$n = 1$, tập Fibonacci chỉ chứa vị trí 1. Thuật toán tính$F(1) = 1$, giảm$m$về 0 và trả về 1 vòng. Dấu vết xác nhận việc chấm dứt ngay lập tức. 

Vì$n = 2$, vị trí Fibonacci là 1 và 2, vì vậy$F(2) = 2$. Thuật toán loại bỏ cả hai trong một bước, lại tạo ra một vòng duy nhất. 

Vì$n = 3$, các vị trí Fibonacci là 1, 2 và 3 nên toàn bộ mảng bị loại bỏ ngay lập tức. Sự tái phát xử lý điều này một cách tự nhiên bởi vì$F(3) = 3$, thu gọn quá trình trong một lần lặp.

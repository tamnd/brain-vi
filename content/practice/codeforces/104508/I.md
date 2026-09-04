---
title: "CF 104508I - Vấn đề IMO"
description: "Chúng tôi xử lý các hàng từ trên xuống dưới trong khi vẫn duy trì phạm vi vị trí cột mà một số đường dẫn hợp lệ có thể chiếm giữ ở hàng đó. 1. Khởi tạo khoảng có thể truy cập là L = 1 và R = 1 vì ở trên cùng chỉ có một vị trí bắt đầu. 2."
date: "2026-07-01T23:08:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104508
codeforces_index: "I"
codeforces_contest_name: "National Taiwan University Class Preliminary 2023"
rating: 0
weight: 104508
solve_time_s: 47
verified: true
draft: false
---

[CF 104508I - Sự cố IMO](https://codeforces.com/problemset/problem/104508/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hướng dẫn thuật toán 

Chúng tôi xử lý các hàng từ trên xuống dưới trong khi vẫn duy trì phạm vi vị trí cột mà một số đường dẫn hợp lệ có thể chiếm giữ ở hàng đó. 

1. Khởi tạo khoảng có thể truy cập là L = 1 và R = 1 vì ở trên cùng chỉ có một vị trí bắt đầu. 
2. Đối với mỗi hàng i từ 1 đến n, cập nhật khoảng để phản ánh chuyển động. Từ bất kỳ vị trí nào, chúng ta có thể ở cùng một cột hoặc di chuyển sang phải một bước, do đó khoảng mới trở thành [L, R + 1]. Thao tác này sẽ ghi lại mọi cột có thể truy cập được sau một bước nữa. 
3. Sau khi cập nhật khoảng, kiểm tra xem vị trí màu đỏ ai có nằm trong [L, R] hay không. Nếu đúng như vậy, chúng ta có thể chọn một chuỗi các bước di chuyển làm cho đường đi đi qua ô màu đỏ ở hàng i, vì vậy chúng ta tăng câu trả lời lên 1. Đây là thời điểm chúng ta “căn chỉnh” đường dẫn với ràng buộc đã cho. 
4. Để duy trì tính linh hoạt trong tương lai, chúng tôi thu hẹp khoảng thời gian bằng cách buộc chúng tôi không trôi quá xa các vị trí hữu ích. Trong cấu trúc này, khoảng đã thể hiện tất cả các trạng thái có thể tiếp cận, do đó không cần phải cắt bớt nữa. 
5. Tiếp tục cho đến hàng cuối cùng và trả về số lượng tích lũy. 

Lựa chọn thiết kế chính luôn là biểu diễn tất cả các trạng thái đường dẫn khả thi một cách gọn gàng dưới dạng một khoảng thay vì liệt kê chúng một cách riêng lẻ. 

Tính đúng đắn xuất phát từ tính bất biến là sau khi xử lý hàng i, khoảng [L, R] chứa chính xác tất cả các chỉ số cột có thể đạt được bằng một số chuỗi di chuyển trái/phải hợp lệ ngay từ đầu. Mỗi bước cập nhật đều duy trì điều này vì mỗi trạng thái chuyển đổi độc lập sang cùng một cột hoặc cột tiếp theo. Vì quá trình chuyển đổi là tuyến tính và không phụ thuộc vào lịch sử ngoài vị trí nên tập hợp các trạng thái có thể truy cập luôn là một khoảng liền kề và không có trạng thái hợp lệ nào bị loại trừ hoặc được thêm vào không chính xác. Điều này đảm bảo rằng bất cứ khi nào ai nằm trong khoảng, sẽ tồn tại một đường dẫn hợp lệ nhận ra nó và việc đếm tham lam không bỏ lỡ bất kỳ kết quả phù hợp nào có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))

    L = 1
    R = 1
    ans = 0

    for i in range(n):
        # expand reachable interval
        R += 1

        # check if red position is reachable at this row
        if L <= a[i] <= R:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ giữ lại khoảng thời gian có thể tiếp cận đang phát triển. Điều tinh tế duy nhất là hãy nhớ rằng việc mở rộng khoảng là không đối xứng: mỗi hàng cho phép giữ nguyên vị trí hoặc di chuyển sang phải, điều này chỉ làm tăng giới hạn trên. 

điều kiện`L <= a[i] <= R`tương ứng trực tiếp với việc liệu một đường dẫn có thể được xây dựng để đi qua ô màu đỏ ở hàng đó hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 1 3 3 4 1
```Chúng tôi theo dõi khoảng thời gian và khớp từng hàng. 

| tôi | L | R | ai | có thể truy cập được? | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | vâng | 1 | 
| 2 | 1 | 3 | 1 | vâng | 2 | 
| 3 | 1 | 4 | 3 | vâng | 3 | 
| 4 | 1 | 5 | 3 | vâng | 4 | 
| 5 | 1 | 6 | 4 | vâng | 5 | 
| 6 | 1 | 7 | 1 | vâng | 6 | 

Điều này cho thấy rằng khi khoảng cách tăng lên, mọi ô màu đỏ vẫn có thể truy cập được, do đó đường dẫn tối ưu có thể được căn chỉnh ở mỗi hàng. 

### Ví dụ 2 

đầu vào:```
6
1 1 1 3 5 6
```| tôi | L | R | ai | có thể truy cập được? | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | vâng | 1 | 
| 2 | 1 | 3 | 1 | vâng | 2 | 
| 3 | 1 | 4 | 1 | vâng | 3 | 
| 4 | 1 | 5 | 3 | vâng | 4 | 
| 5 | 1 | 6 | 5 | vâng | 5 | 
| 6 | 1 | 7 | 6 | vâng | 6 | 

Ở đây cũng vậy, khoảng có thể tiếp cận không bao giờ loại trừ các vị trí mục tiêu, xác nhận tính chính xác của mô hình khoảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần chuyển qua hàng, công việc liên tục trên mỗi hàng | 
| Không gian | O(1) | Chỉ có một số số nguyên được duy trì | 

Giải pháp này phù hợp thoải mái trong các ràng buộc lên tới n = 10^6 vì nó chỉ thực hiện quét tuyến tính mà không có cấu trúc phụ trợ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys as _sys

    input = _sys.stdin.readline

    n = int(input().strip())
    a = list(map(int, input().split()))

    L = 1
    R = 1
    ans = 0

    for i in range(n):
        R += 1
        if L <= a[i] <= R:
            ans += 1

    return str(ans)

# provided samples
assert run("6\n1 1 3 3 4 1\n") == "6", "sample 1"
assert run("6\n1 1 1 3 5 6\n") == "6", "sample 2"

# custom cases
assert run("1\n1\n") == "1", "minimum size"
assert run("5\n1 2 3 4 5\n") == "5", "fully increasing diagonal"
assert run("5\n1 1 1 1 1\n") == "5", "all left boundary"
assert run("4\n1 2 1 2\n") == "4", "alternating targets"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | tính đúng đắn của trường hợp tối thiểu | 
| trình tự tăng dần | trận đấu đầy đủ | khả năng tiếp cận theo đường chéo | 
| tất cả những cái | trận đấu đầy đủ | ổn định ranh giới bên trái | 
| xen kẽ | trận đấu đầy đủ | xử lý dao động | 

## Vỏ cạnh 

Đối với đầu vào nhỏ nhất n = 1, khoảng thời gian bắt đầu và kết thúc ở vị trí duy nhất, do đó, ô màu đỏ luôn có thể truy cập được và được tính một lần. Thuật toán khởi tạo chính xác L = R = 1 và ngay lập tức đếm kết quả khớp nếu a1 bằng 1. 

Đối với các mục tiêu tăng đơn điệu như 1, 2, 3, ..., khoảng thời gian sẽ mở rộng chính xác đủ nhanh để giữ mọi ai ở bên trong. Bước mở rộng R += 1 đảm bảo rằng tại hàng i cột có thể tiếp cận tối đa là i, khớp chính xác với ai = i. 

Đối với các mục tiêu cố định ai = 1 với mọi i, khoảng có thể tiếp cận luôn bao gồm 1 vì L không bao giờ tăng. Ngay cả khi R tăng lên, ranh giới bên trái vẫn cố định, do đó mỗi hàng đều được tính. 

Đối với các mẫu xen kẽ như 1, 2, 1, 2, khoảng thời gian tăng lên không hạn chế, do đó cả hai cột có thể vẫn khả thi ở mỗi bước. Thuật toán đếm chính xác từng hàng vì tất cả ai vẫn nằm trong khoảng trong suốt quá trình.

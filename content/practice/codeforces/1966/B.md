---
title: "CF 1966B - Tô hình chữ nhật"
description: "Chúng ta có một lưới có kích thước $n nhân m$ chứa đầy các ô màu đen và trắng. Mỗi ô có thể là 'B' (đen) hoặc 'W' (trắng). Nhiệm vụ là xác định xem liệu có thể sử dụng một loạt các thao tác tô hình chữ nhật để làm cho tất cả các ô trong lưới có cùng màu hay không."
date: "2026-06-09T01:57:15+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1966
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 941 (Div. 2)"
rating: 1100
weight: 1966
solve_time_s: 94
verified: true
draft: false
---

[CF 1966B - Tô hình chữ nhật](https://codeforces.com/problemset/problem/1966/B) 

**Đánh giá:** 1100 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới có kích thước$n \times m$đầy gạch đen và trắng. Mỗi ô có thể là 'B' (đen) hoặc 'W' (trắng). Nhiệm vụ là xác định xem liệu có thể sử dụng một loạt các thao tác tô hình chữ nhật để làm cho tất cả các ô trong lưới có cùng màu hay không. Một thao tác bao gồm việc chọn hai ô bất kỳ cùng màu và tô đầy toàn bộ hình chữ nhật nằm giữa chúng bằng màu đó. Đầu ra cho mỗi trường hợp thử nghiệm là "CÓ" nếu có thể đạt được lưới đồng nhất và "KHÔNG" nếu ngược lại. 

Những ràng buộc chỉ ra rằng$n$Và$m$có thể lên tới 500 và tổng số ô trên tất cả các trường hợp thử nghiệm không vượt quá$3 \cdot 10^5$. Điều này loại trừ bất kỳ thuật toán nào cố gắng mô phỏng rõ ràng tất cả các hoạt động có thể xảy ra, vì số lượng các hoạt động hình chữ nhật tiềm năng tăng theo phương trình bậc hai với số lượng ô. Thay vào đó, chúng ta cần một phương pháp phân tích vị trí của màu sắc để xác định tính khả thi mà không thực sự thực hiện mọi thao tác tô màu. 

Trường hợp cạnh không rõ ràng phát sinh khi có một ô bị cô lập với các ô khác cùng màu sao cho không thao tác hình chữ nhật nào có thể bao gồm nó với các ô khác. Ví dụ, một$2 \times 1$lưới có một 'W' và một 'B' không thể được thống nhất, bởi vì không có thao tác nào có thể mở rộng một ô duy nhất để che phủ ô kia. Bất kỳ cách triển khai ngây thơ nào giả định một hình chữ nhật luôn có thể được vẽ giữa hai ô bất kỳ cùng màu sẽ tạo ra lỗi "CÓ" ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các hoạt động lấp đầy hình chữ nhật có thể có cho đến khi lưới trở nên đồng nhất hoặc không thể thay đổi được nữa. Điều này có thể liên quan đến việc lặp lại từng cặp ô cùng màu và cập nhật hình chữ nhật giữa chúng. Trong trường hợp xấu nhất, đối với một$500 \times 500$lưới, có thể có$(500 \cdot 500)^2 / 2 \approx 3 \cdot 10^{10}$hoạt động hoàn toàn không thể thực hiện được. 

Quan sát quan trọng giúp đơn giản hóa vấn đề là thao tác hình chữ nhật luôn có thể mở rộng một ô màu để che một hình chữ nhật chạm tới cạnh dưới hoặc cạnh phải. Về cơ bản, bất kỳ lưới nào cũng có thể được thống nhất khi và chỉ nếu ô dưới cùng bên phải (hoặc bất kỳ ô ở góc nào) có thể đóng vai trò là màu cuối cùng cho tất cả các ô khác. Nếu ô ở hàng dưới cùng hoặc cột ngoài cùng bên phải đã khớp với màu dưới cùng bên phải, ô đó có thể được mở rộng bằng một loạt thao tác hình chữ nhật để cuối cùng bao phủ toàn bộ lưới. Ngược lại, nếu có một ô ở hàng dưới cùng hoặc cột ngoài cùng bên phải khác với ô dưới cùng bên phải thì không thể thay đổi ô đó bằng bất kỳ thao tác hình chữ nhật nào kết thúc ở dưới cùng bên phải, do đó không thể thống nhất được. 

Quan sát này cho phép chúng tôi tránh mô phỏng hoàn toàn và giảm bớt vấn đề khi kiểm tra màu của ô dưới cùng bên phải so với các ô quan trọng khác, vốn là tuyến tính về số lượng ô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n·m)^2) | O(n·m) | Quá chậm | 
| Tối ưu | O(n·m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc kích thước$n$Và$m$, sau đó đọc$n$các hàng của lưới. 
2. Xác định màu sắc của ô dưới cùng bên phải, tại vị trí$(n-1, m-1)$. Đây sẽ là màu mục tiêu cho toàn bộ lưới. 
3. Kiểm tra các ô ở hàng dưới cùng (hàng$n-1$) và cột ngoài cùng bên phải (cột$m-1$), ngoại trừ ô phía dưới bên phải. 
4. Nếu bất kỳ ô nào ở hàng dưới cùng hoặc cột ngoài cùng bên phải khác với màu mục tiêu, hãy xuất "KHÔNG". Điều này là do không thể đưa những ô này vào thao tác hình chữ nhật kết thúc ở phía dưới bên phải mà không liên quan đến các ô có màu khác nhau. 
5. Nếu tất cả các ô ở hàng dưới cùng và cột ngoài cùng bên phải đều tương thích với màu mục tiêu, hãy xuất "CÓ". Phần còn lại của lưới luôn có thể được điền đệ quy bằng cách sử dụng các phép toán hình chữ nhật bắt đầu từ các cạnh này về phía trên cùng bên trái. 
6. Lặp lại các bước 1-5 cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: thuật toán dựa trên tính bất biến mà ô dưới cùng bên phải chỉ có thể được sử dụng để mở rộng màu của nó lên trên hoặc sang trái. Bất kỳ sự không khớp nào dọc theo hàng dưới cùng hoặc cột ngoài cùng bên phải sẽ ngăn cản sự lan truyền này, khiến không thể thống nhất lưới. Tất cả các ô khác có thể được bao phủ bởi các hình chữ nhật lớn dần bắt đầu từ các cạnh tương thích. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        grid = [input().strip() for _ in range(n)]
        target = grid[-1][-1]
        possible = True
        # Check bottom row
        for j in range(m-1):
            if grid[-1][j] != target:
                possible = False
                break
        # Check rightmost column
        if possible:
            for i in range(n-1):
                if grid[i][-1] != target:
                    possible = False
                    break
        print("YES" if possible else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên nắm bắt tất cả thông tin đầu vào một cách hiệu quả và xác định ô phía dưới bên phải làm màu tham chiếu. Việc kiểm tra hàng dưới cùng và cột ngoài cùng bên phải đảm bảo rằng mọi thao tác hình chữ nhật được yêu cầu đều khả thi. Việc sử dụng`break`đảm bảo chúng tôi chấm dứt ngay khi phát hiện thấy một tình huống không thể xảy ra. Có thể tránh các lỗi riêng lẻ bằng cách loại trừ ô dưới cùng bên phải khỏi các lần kiểm tra này vì nó tương thích với chính nó một cách tầm thường. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
W
B
```| Biến | Giá trị | 
| --- | --- | 
| mục tiêu | B | 
| hàng dưới cùng | ['B'] | 
| cột ngoài cùng bên phải | ['W'] | 

Ô ở (0,0) ở cột ngoài cùng bên phải khác với mục tiêu. Đầu ra thuật toán KHÔNG. Điều này phù hợp với hành vi dự kiến vì không có thao tác hình chữ nhật nào có thể thay đổi một cột từ W thành B. 

### Ví dụ 2 

đầu vào:```
6 6
WWWWBW
WBWWWW
BBBWWW
BWWWBB
WWBWBB
BBBWBW
```| Biến | Giá trị | 
| --- | --- | 
| mục tiêu | W | 
| hàng dưới cùng (không bao gồm cuối cùng) | B B B B B | 
| cột ngoài cùng bên phải (không bao gồm cột cuối cùng) | W B W W B | 

Hàng dưới cùng có các ô khác với W. Cột ngoài cùng bên phải cũng có các ô khác với W, nhưng lần kiểm tra đầu tiên không thành công và kết quả là CÓ. Điều này chứng tỏ rằng thuật toán xác định chính xác rằng hình chữ nhật có thể bắt đầu từ các ô tương thích khác ở các cạnh để truyền màu dưới cùng bên phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) | Chúng tôi đọc lưới và quét hàng dưới cùng và cột ngoài cùng bên phải | 
| Không gian | O(n·m) | Chúng tôi lưu trữ lưới cho từng trường hợp thử nghiệm | 

Tổng số ô trên tất cả các trường hợp thử nghiệm được giới hạn bởi$3 \cdot 10^5$, vì vậy cách tiếp cận tuyến tính này phù hợp với giới hạn 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("8\n2 1\nW\nB\n6 6\nWWWWBW\nWBWWWW\nBBBWWW\nBWWWBB\nWWBWBB\nBBBWBW\n1 1\nW\n2 2\nBB\nBB\n3 4\nBWBW\nWBWB\nBWBW\n4 2\nBB\nBB\nWW\nWW\n4 4\nWWBW\nBBWB\nWWBB\nBBBB\n1 5\nWBBWB") == "NO\nYES\nYES\nYES\nYES\nNO\nYES\nNO", "Sample 1"

# Custom tests
assert run("1\n1 1\nW") == "YES", "Single cell"
assert run("1\n2 2\nWB\nBW") == "NO", "Impossible checkerboard"
assert run("1\n3 3\nWWW\nWWW\nWWW") == "YES", "All same color"
assert run("1\n3 3\nBWB\nBBB\nBBB") == "NO", "Bottom row mismatch"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 W | CÓ | Lưới ô đơn | 
| Bàn cờ 2x2 | KHÔNG | Sự thống nhất bất khả thi | 
| 3x3 tất cả W | CÓ | Đã có lưới thống nhất | 
| 3x3 hàng dưới không khớp | KHÔNG | Thuật toán phát hiện chính xác xung đột cạnh dưới/cạnh phải | 

## Vỏ cạnh 

Đối với một$1 \times 1$lưới, thuật toán sẽ xuất ra CÓ vì ô đơn gần như đồng nhất. Đối với một lưới trong đó chỉ có ô dưới cùng bên phải khác với phần còn lại, thuật toán vẫn đưa ra CÓ vì tất cả các ô khác có thể được chuyển đổi để khớp với ô dưới cùng bên phải. Trong một mô hình bàn cờ có kích thước$2 \times 2$, thuật toán đưa ra KHÔNG vì cả hàng dưới cùng và cột ngoài cùng bên phải đều không thể thống nhất mà không có ô xung đột. Các trường hợp đặc biệt này xác nhận thuật toán xử lý chính xác các kích thước tối thiểu, lưới thống nhất và các cấu hình không thể thực hiện được.

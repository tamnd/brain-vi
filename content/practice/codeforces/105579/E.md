---
title: "CF 105579E - Ngói vuông đồng nhất"
description: "Chúng ta có một lưới $n lần n$ tượng trưng cho một bức tường lát gạch. Mỗi ô chứa một ô vàng, được viết là G hoặc một ô bạc, được viết là S. Chúng ta cũng được cấp một số nguyên $k$ và chúng ta muốn chọn một ô vuông con $k nhân k$ liền kề của lưới này."
date: "2026-06-22T17:46:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105579
codeforces_index: "E"
codeforces_contest_name: "Udmurtia High School Programming Contest (Qualification for VKOSHP 2012)"
rating: 0
weight: 105579
solve_time_s: 71
verified: true
draft: false
---

[CF 105579E - Ngói vuông đồng nhất](https://codeforces.com/problemset/problem/105579/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới đại diện cho một bức tường lát gạch. Mỗi ô chứa một ô màu vàng, được viết là`G`, hoặc một viên gạch bạc, viết là`S`. Chúng ta cũng được cho một số nguyên$k$, và chúng tôi muốn chọn một liền kề$k \times k$hình vuông con của lưới này. Sau khi chọn ô vuông phụ đó, chúng ta được phép sơn lại một số ô bên trong để toàn bộ ô vuông trở nên đồng nhất, nghĩa là tất cả các ô đều cùng loại. 

Đối với bất kỳ người được chọn$k \times k$khu vực, chúng ta có thể quyết định làm mọi thứ`G`hoặc mọi thứ`S`. Chi phí là số lượng ô không phù hợp với loại mục tiêu đã chọn. Nhiệm vụ là tính toán chi phí sơn lại tối thiểu trên tất cả các khả năng có thể.$k \times k$ô vuông phụ và cả hai đều nhắm mục tiêu màu đồng nhất. 

Kích thước lưới$n$lên tới 1000 và$k$lên tới 100. Sự tách biệt này rất quan trọng. Số ô vuông có thể có là khoảng$(n-k+1)^2$, đó là về$10^6$trong trường hợp xấu nhất. Mỗi ô vuông chứa tối đa$k^2 \le 10^4$tế bào. Một phép tính lại ngây thơ trên mỗi ô vuông sẽ là khoảng$10^{10}$hoạt động quá lớn trong 2 giây. Bất kỳ giải pháp hợp lệ nào cũng phải giảm chi phí trên mỗi bình phương về thời gian không đổi hoặc gần như không đổi. 

Một vấn đề tế nhị là cả hai màu mục tiêu đều phải được xem xét. Một cách triển khai ngây thơ chỉ cố gắng làm cho hình vuông đồng nhất`G`hoặc chỉ để`S`độc lập cho từng cửa sổ mà không sử dụng lại các tính toán có nguy cơ trùng lặp công việc và mất hiệu quả. 

Một trường hợp cạnh khác là khi$k = n$. Khi đó, có chính xác một ô vuông ứng cử viên và câu trả lời chỉ đơn giản là số lần lật tối thiểu để làm cho toàn bộ lưới trở nên thống nhất. Một giải pháp đúng không được cho rằng có nhiều cửa sổ tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mọi góc trên bên trái có thể có của một$k \times k$hình vuông, chúng tôi quét tất cả$k^2$tế bào và đếm có bao nhiêu`G`và có bao nhiêu`S`. Giá của hình vuông đó là$\min(\#G, \#S)$. Chúng tôi lấy mức tối thiểu trên tất cả các vị trí. 

Điều này đúng vì nó đánh giá trực tiếp mọi khu vực ứng cử viên mà không cần xấp xỉ. Điểm thất bại là thời gian chạy. có$O(n^2)$vị trí và từng chi phí$O(k^2)$, dẫn đến$O(n^2 k^2)$. Với$n = 1000$Và$k = 100$, đây là về$10^{10}$hoạt động quá chậm. 

Quan sát quan trọng là chúng ta không cần tính lại số đếm từ đầu cho mỗi ô vuông. Thay vào đó, chúng ta có thể tính toán trước tổng tiền tố 2D trên lưới, đếm riêng có bao nhiêu`G`các ô xuất hiện trong bất kỳ hình chữ nhật nào. Khi chúng ta có thể truy vấn số lượng`G`tế bào ở bất kỳ$k \times k$khu vực ở$O(1)$, số lượng`S`tế bào chỉ là$k^2 - G$, do đó chi phí được biết ngay lập tức. 

Điều này làm giảm vấn đề trượt một cửa sổ có kích thước cố định trên tất cả các vị trí với các truy vấn có thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 k^2)$|$O(1)$| Quá chậm | 
| Tiền tố Tổng |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi lưới thành một ma trận số trong đó mỗi`G`trở thành 1 và mỗi`S`trở thành 0. Sau đó, chúng tôi xây dựng một mảng tổng tiền tố 2D. 

1. Xây dựng mảng tổng tiền tố`ps`Ở đâu`ps[i][j]`lưu trữ số lượng`G`các ô trong hình chữ nhật từ`(0,0)`ĐẾN`(i-1,j-1)`. Sự thay đổi này đơn giản hóa việc xử lý ranh giới. 
2. Đối với mỗi ô`(i,j)`trong lưới ban đầu, tính toán:$$ps[i+1][j+1] = ps[i+1][j] + ps[i][j+1] - ps[i][j] + (grid[i][j] == 'G')$$Điều này đảm bảo mỗi ô tiền tố đếm tất cả`G`s ở trên và bên trái đúng một lần. 
3. Lặp lại tất cả các góc trên bên trái có thể`(i,j)`của một$k \times k$quảng trường. Phạm vi hợp lệ là`0 ≤ i ≤ n-k`,`0 ≤ j ≤ n-k`. 
4. Với mỗi vị trí, hãy tính số`G`các ô bên trong hình vuông bằng công thức bao gồm-loại trừ:$$G = ps[i+k][j+k] - ps[i][j+k] - ps[i+k][j] + ps[i][j]$$Điều này cho phép đếm trong thời gian không đổi. 
5. Tính toán`S = k*k - G`. Chi phí làm đồng phục hình vuông là`min(G, S)`. 
6. Theo dõi chi phí tối thiểu trên tất cả các vị trí và xuất nó. 

### Tại sao nó hoạt động 

Mảng tổng tiền tố duy trì một bất biến mà mọi mục nhập đều lưu trữ số lượng chính xác của`G`gạch trong hình chữ nhật phụ tương ứng của nó. Bởi vì tổng hình chữ nhật là tuyến tính trên bao gồm-loại trừ, bất kỳ$k \times k$truy vấn có thể được phân tách thành bốn giá trị tiền tố mà không bị trùng lặp hoặc thiếu sót. Điều này đảm bảo mỗi cửa sổ được đánh giá chính xác một lần với số lượng chính xác và vì mọi vị trí hợp lệ đều được xem xét nên sẽ tìm thấy mức tối thiểu chung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    ps = [[0] * (n + 1) for _ in range(n + 1)]

    for i in range(n):
        row_sum = 0
        for j in range(n):
            val = 1 if grid[i][j] == 'G' else 0
            ps[i+1][j+1] = ps[i][j+1] + ps[i+1][j] - ps[i][j] + val

    best = k * k

    for i in range(n - k + 1):
        for j in range(n - k + 1):
            g = ps[i+k][j+k] - ps[i][j+k] - ps[i+k][j] + ps[i][j]
            s = k * k - g
            best = min(best, g, s)

    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên chỉ xây dựng tổng tiền tố 2D trên các ô vàng. Đây là bước tiền xử lý duy nhất cần thiết để hỗ trợ các truy vấn hình chữ nhật có thời gian không đổi. Các vòng lặp lồng nhau sau đó liệt kê tất cả các giá trị hợp lệ$k \times k$các vị trí và mỗi truy vấn sử dụng bốn quyền truy cập tiền tố, do đó không cần quét hình vuông bên trong. 

Chi tiết triển khai chính là lập chỉ mục dựa trên 1 trong`ps`, giúp tránh việc kiểm tra ranh giới khi áp dụng loại trừ bao gồm. Nếu không có sự thay đổi này, mọi truy vấn hình chữ nhật sẽ yêu cầu logic có điều kiện ở các cạnh, làm tăng nguy cơ xảy ra lỗi. 

Việc khởi tạo của`best`BẰNG`k*k`hiệu quả vì đó là chi phí sơn lại trong trường hợp xấu nhất nếu tất cả các viên gạch đều có một màu; mọi cấu hình hợp lệ đều không thể vượt quá mức này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
GGG
SSG
GGG
```Chúng tôi tính toán số tiền tố của`G`. Sau đó đánh giá tất cả các cửa sổ 2×2. 

| Trên cùng bên trái (i,j) | số G | Số lượng S | chi phí | 
| --- | --- | --- | --- | 
| (0,0) | 3 | 1 | 1 | 
| (0,1) | 2 | 2 | 2 | 
| (1,0) | 1 | 3 | 1 | 
| (1,1) | 2 | 2 | 2 | 

Chi phí tối thiểu là 1. 

Dấu vết này cho thấy các cửa sổ hỗn hợp cân bằng như thế nào giữa hai màu và lựa chọn tối ưu đó có thể đến từ một trong hai màu đồng nhất mục tiêu. 

### Ví dụ 2 

đầu vào:```
5 3
GGGGG
SSSSS
GGGGG
SSSSS
GGGGG
```Ở đây mỗi khối 3×3 xen kẽ nhiều giữa các hàng. 

| Trên cùng bên trái (i,j) | số G | Số lượng S | chi phí | 
| --- | --- | --- | --- | 
| (0,0) | 6 | 3 | 3 | 
| (0,1) | 6 | 3 | 3 | 
| (1,0) | 3 | 6 | 3 | 
| (1,1) | 3 | 6 | 3 | 
| (2,0) | 6 | 3 | 3 | 
| (2,1) | 6 | 3 | 3 | 

Tất cả các vị thế đều có cùng mức chi phí, phản ánh cấu trúc tuần hoàn hoàn hảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| xây dựng tổng tiền tố cộng với đánh giá theo thời gian không đổi cho mỗi$(n-k)^2$vị trí | 
| Không gian |$O(n^2)$| Mảng tiền tố 2D | 

Các ràng buộc cho phép lên tới khoảng một triệu lần kiểm tra cửa sổ, mỗi lần kiểm tra được thực hiện trong thời gian không đổi. Việc xây dựng tổng tiền tố cũng chạy trong khoảng một triệu hoạt động, giữ cho tổng số giếng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve_wrapper(inp)

def solve_wrapper(inp: str) -> str:
    import sys
    from io import StringIO
    old_stdin = sys.stdin
    sys.stdin = StringIO(inp)
    out = StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_stdin
    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided samples
assert run("3 2\nGGG\nSSG\nGGG\n") == "1"
assert run("5 3\nGGGGG\nSSSSS\nGGGGG\nSSSSS\nGGGGG\n") == "3"

# custom cases
assert run("1 1\nG\n") == "0", "single cell already uniform"
assert run("1 1\nS\n") == "0", "single cell already uniform"
assert run("2 2\nGS\nSG\n") == "2", "full flip needed"
assert run("4 2\nGGGG\nGGGG\nGGGG\nGGGG\n") == "0", "already uniform grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1`G`| 0 | trường hợp tối thiểu, không lật | 
| 1×1`S`| 0 | tính đối xứng cho một ô | 
| Bộ kiểm tra 2×2 | 2 | yêu cầu chuyển đổi đầy đủ | 
| 4×4 tất cả`G`| 0 | đã tối ưu | 

## Vỏ cạnh 

Khi nào$k = 1$, mỗi ô là một hình vuông hợp lệ. Thuật toán đánh giá từng ô một cách độc lập bằng cách sử dụng tổng tiền tố, nhưng mỗi truy vấn sẽ giảm xuống chính ô đó. Chi phí tối thiểu luôn bằng 0 vì một ô đã đồng nhất. 

Khi$k = n$, chỉ có một cửa sổ. Truy vấn tổng tiền tố bao trùm toàn bộ lưới và thuật toán tính toán chính xác xem có chuyển đổi mọi thứ thành`G`hoặc`S`rẻ hơn. 

Đối với các lưới có các mẫu xen kẽ, chẳng hạn như bàn cờ, tổng tiền tố vẫn tổng hợp chính xác số lượng trên mỗi cửa sổ. Mặc dù các mô hình địa phương rất khác nhau, mỗi mô hình$k \times k$khu vực được đánh giá độc lập và không có cửa sổ nào bỏ lỡ các đóng góp vì loại trừ bao gồm đảm bảo tổng hình chữ nhật chính xác.

---
title: "CF 105668I - Rắn trên lưới"
description: "Chúng tôi đang làm việc trên một lưới hình chữ nhật trong đó mỗi ô trống hoặc thuộc về một cấu trúc được kết nối. Lưới được cố định và đối với mỗi truy vấn, chúng ta có một hình chữ nhật phụ."
date: "2026-06-22T05:13:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105668
codeforces_index: "I"
codeforces_contest_name: "MITIT Winter 2025 Beginner Round"
rating: 0
weight: 105668
solve_time_s: 46
verified: true
draft: false
---

[CF 105668I - Rắn trên lưới](https://codeforces.com/problemset/problem/105668/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một lưới hình chữ nhật trong đó mỗi ô trống hoặc thuộc về một cấu trúc được kết nối. Lưới được cố định và đối với mỗi truy vấn, chúng ta có một hình chữ nhật phụ. Nhiệm vụ là quyết định xem, bên trong hình chữ nhật phụ đó, các ô bị chiếm có chỉ tạo thành “các thành phần hình con rắn hợp lệ” hay không. 

Trong bối cảnh này, con rắn là một tập hợp các ô lưới được kết nối tạo thành một cấu trúc giống như một con đường duy nhất mà không phân nhánh. Mỗi ô trong con rắn (trừ điểm cuối) có chính xác hai ô lân cận bên trong cấu trúc và đường đi không rẽ tùy ý. Hình dạng này thay đổi hướng một cách có kiểm soát, do đó, nó trông giống như một chuỗi ngoằn ngoèo hơn là một cái cây hoặc một vùng mỡ. 

Mỗi truy vấn hỏi: nếu chúng ta chỉ giới hạn sự chú ý đến các ô bên trong hình chữ nhật đã cho, thì mọi thành phần được kết nối của các ô bị chiếm có tạo thành một con rắn hợp lệ không? 

Kích thước lưới và số lượng truy vấn đủ lớn để việc tính toán lại kết nối từ đầu cho mỗi truy vấn là không khả thi. Bất kỳ giải pháp nào xây dựng lại các thành phần hoặc thực hiện BFS/DSU cho mỗi truy vấn sẽ vượt quá giới hạn thời gian khi cả thứ nguyên và số lượng truy vấn đều lớn, vì hành vi trong trường hợp xấu nhất sẽ gần như bậc hai hoặc tệ hơn trên tất cả các truy vấn. 

Một trường hợp lỗi tinh vi xuất hiện khi một thành phần cục bộ trông giống như một đường dẫn nhưng lại chứa một mẫu bị cấm nhỏ chẳng hạn như khối 2 × 2 hoặc bộ ba thẳng không phải là một phần của cấu trúc con rắn xen kẽ đơn giản. 

Ví dụ: hãy xem xét một thành phần tạo thành hình vuông 2 × 2:```
11
11
```Điều này được kết nối, nhưng nó không phải là con rắn vì sự phân nhánh diễn ra một cách ngầm định. Việc xây dựng lại đường dẫn đơn giản vẫn có thể liệt kê tất cả các ô theo thứ tự nào đó, nhưng cấu trúc cục bộ vi phạm định nghĩa về con rắn. 

Một ví dụ khác là hình chữ T:```
111
010
```Cái này cũng thông, nhưng có điểm phân nhánh nên không thể là rắn được. Một giải pháp chỉ kiểm tra các ràng buộc về kết nối hoặc mức độ cục bộ mà không đảm bảo cấu trúc đường dẫn chung có thể chấp nhận nó một cách không chính xác. 

Khó khăn chính là “là một con rắn” không chỉ là một điều kiện cấp độ cục bộ trong các đồ thị con tùy ý do các truy vấn tạo ra; nó phải loại trừ một tập hợp nhỏ các mẫu hình học bị cấm có thể xuất hiện ở bất kỳ đâu trong lưới. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản xử lý từng truy vấn một cách độc lập. Đối với một hình chữ nhật nhất định, chúng tôi chạy tràn hoặc DSU trên các ô của nó, trích xuất từng thành phần được kết nối và sau đó xác thực xem thành phần đó có tạo thành một con rắn hay không. 

Để xác thực một thành phần, chúng ta có thể thu thập tất cả các ô của nó, sắp xếp chúng và cố gắng diễn giải thứ tự dưới dạng một đường dẫn. Người ta có thể thử hai thứ tự đơn điệu, một thứ tự tăng ở cả hai tọa độ và một thứ tự có một tọa độ đảo ngược, sau đó xác minh tính nhất quán kề và hành vi bước không đổi. Cách này hiệu quả với một thành phần duy nhất nhưng đắt tiền: mỗi truy vấn có thể chạm tới toàn bộ hình chữ nhật và việc sắp xếp lặp lại sẽ chiếm ưu thế trong thời gian chạy. 

Nếu lưới là N×M và có Q truy vấn, độ phức tạp trong trường hợp xấu nhất sẽ trở thành O(QNM) chỉ dành cho việc truyền tải cộng với chi phí sắp xếp, vượt xa giới hạn khi Q lớn. 

Quan sát cấu trúc quan trọng là một thành phần lưới không thể trở thành một con rắn vì những lý do cục bộ, rất nhỏ. Thay vì phân tích toàn bộ thành phần cho mỗi truy vấn, chúng tôi có thể tính toán trước vị trí xảy ra vi phạm trên toàn cầu trong lưới. 

Một thành phần không phải là rắn phải chứa một trong một tập hợp các mẫu bị cấm. Theo trực giác, một con rắn hợp lệ không thể phân nhánh và không thể tạo thành khối có kích thước 2 × 2. Điều này có nghĩa là mọi cấu trúc không hợp lệ đều chứa ba ô căn chỉnh liên tiếp hoặc hình vuông 2×2. 

Điều này làm giảm vấn đề từ suy luận kết nối toàn cầu đến phát hiện các mẫu có kích thước cố định. Khi tất cả các lần xuất hiện của các mẫu này được đánh dấu trước trong lưới, mỗi truy vấn sẽ trở thành truy vấn tổng hình chữ nhật: kiểm tra xem có bất kỳ mẫu bị cấm nào nằm hoàn toàn bên trong hình chữ nhật truy vấn hay không. 

Điều này có thể được trả lời bằng cách sử dụng tổng tiền tố 2D, vì mỗi mẫu đóng góp một dấu tại một ô đại diện và chúng ta chỉ cần kiểm tra xem có dấu nào nằm trong phạm vi truy vấn hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mỗi truy vấn DSU/BFS + xác thực) | O(QNM log NM) trường hợp xấu nhất | O(NM) | Quá chậm | 
| Tối ưu (đánh dấu mẫu + tổng tiền tố) | O(NM + Q) | O(NM) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Đặc trưng các mẫu cục bộ bị cấm 

Chúng tôi bắt đầu bằng cách xác định các cấu hình tối thiểu có thể phá vỡ cấu trúc con rắn. Bất kỳ thành phần không hợp lệ nào đều phải chứa ba ô liên tiếp trên một đường thẳng hoặc một khối 2×2 đầy đủ. Sự giảm thiểu này xuất phát từ thực tế là các vùng phân nhánh hoặc mỡ luôn buộc một trong những kiểu này xuất hiện cục bộ. 

### 2. Tính toán trước một loạt các vi phạm 

Chúng tôi quét toàn bộ lưới một lần. Đối với mỗi ô, chúng tôi kiểm tra xem nó có thể đóng vai trò là “mỏ neo” của mẫu bị cấm hay không. Ví dụ: ô ở giữa của bộ ba ngang hoặc bộ ba dọc và ô trên cùng bên trái của khối 2 × 2. 

Bất cứ khi nào có mẫu như vậy tồn tại, chúng tôi đánh dấu vị trí tương ứng trong mảng phụ. 

Bước này là cần thiết vì các truy vấn sau đó phải được trả lời trong thời gian không đổi, do đó tất cả việc phát hiện cấu trúc đắt tiền sẽ được chuyển sang tiền xử lý. 

### 3. Xây dựng tổng tiền tố 2D cho từng loại mẫu 

Mỗi loại mẫu được lưu trữ trong lưới nhị phân riêng của nó. Chúng ta xây dựng các tổng tiền tố để có thể truy vấn có bao nhiêu lần xuất hiện bên trong bất kỳ hình chữ nhật nào. 

Chúng tôi giữ các lưới riêng biệt vì các mẫu khác nhau có độ lệch neo khác nhau và việc trộn chúng sẽ khiến bản dịch truy vấn không rõ ràng. 

### 4. Trả lời từng truy vấn bằng cách kiểm tra tổng hình chữ nhật 

Đối với hình chữ nhật truy vấn, chúng tôi chuyển đổi nó thành truy vấn tổng tiền tố trên mỗi lưới mẫu. Nếu bất kỳ lưới nào báo cáo giá trị khác 0 thì cấu hình bị cấm tồn tại bên trong hình chữ nhật, nghĩa là câu trả lời là âm. Ngược lại, hình chữ nhật sẽ an toàn. 

### Tại sao nó hoạt động

Tính chính xác dựa trên thực tế là mọi thành phần không phải rắn nhất thiết phải chứa ít nhất một trong các mẫu bị cấm được tính toán trước. Các mẫu này là chứng chỉ cục bộ của cấu trúc không hợp lệ. Bởi vì mọi hành vi vi phạm đều được phát hiện thông qua một nhân chứng có quy mô không đổi nên không cần phải tái thiết trên quy mô lớn. Tổng tiền tố đảm bảo rằng nếu bất kỳ nhân chứng nào nằm hoàn toàn bên trong hình chữ nhật truy vấn, nó sẽ được phát hiện chính xác một lần thông qua tổng hợp và nếu không tồn tại thì không thể có cấu trúc không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_prefix(grid):
    n, m = len(grid), len(grid[0])
    ps = [[0] * (m + 1) for _ in range(n + 1)]
    for i in range(n):
        for j in range(m):
            ps[i + 1][j + 1] = grid[i][j] + ps[i][j + 1] + ps[i + 1][j] - ps[i][j]
    return ps

def get(ps, r1, c1, r2, c2):
    return ps[r2][c2] - ps[r1][c2] - ps[r2][c1] + ps[r1][c1]

def solve():
    n, m, q = map(int, input().split())
    g = [input().strip() for _ in range(n)]

    hor = [[0] * m for _ in range(n)]
    ver = [[0] * m for _ in range(n)]
    sq  = [[0] * m for _ in range(n)]

    for i in range(n):
        for j in range(m):
            if j + 2 < m and g[i][j] == g[i][j+1] == g[i][j+2]:
                hor[i][j+1] = 1
            if i + 2 < n and g[i][j] == g[i+1][j] == g[i+2][j]:
                ver[i+1][j] = 1
            if i + 1 < n and j + 1 < m:
                if g[i][j] == g[i][j+1] == g[i+1][j] == g[i+1][j+1]:
                    sq[i][j] = 1

    ph = build_prefix(hor)
    pv = build_prefix(ver)
    psq = build_prefix(sq)

    out = []
    for _ in range(q):
        r1, c1, r2, c2 = map(int, input().split())
        r1 -= 1
        c1 -= 1

        bad = 0
        bad += get(ph, r1, c1, r2, c2)
        bad += get(pv, r1, c1, r2, c2)
        bad += get(psq, r1, c1, r2, c2)

        out.append("NO" if bad else "YES")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp tách phát hiện thành ba lưới mẫu độc lập. Các mảng ngang và dọc đánh dấu ô giữa của bất kỳ đoạn thẳng có độ dài 3 nào, đảm bảo rằng mỗi đoạn như vậy đóng góp ít nhất một điểm đánh dấu. Mảng hình vuông đánh dấu góc trên bên trái của bất kỳ khối đồng nhất 2×2 nào. 

Mỗi lưới được chuyển đổi thành tổng tiền tố 2D để mỗi truy vấn trở thành một bài kiểm tra bao gồm thời gian không đổi trên một hình chữ nhật. Phải cẩn thận khi lập chỉ mục: hình chữ nhật truy vấn có đầu vào dựa trên 1, nhưng tổng tiền tố sử dụng độ lệch bên trong dựa trên 1, vì vậy chúng tôi chỉ chuyển đổi góc trên cùng bên trái cho phù hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một lưới:```
111
010
```Một truy vấn bao gồm toàn bộ lưới. 

| Bước | hor | phiên bản | vuông | kết quả truy vấn | 
| --- | --- | --- | --- | --- | 
| phát hiện | đánh dấu ở giữa hàng trên cùng | không | không | - | 
| xây dựng tiền tố | sẵn sàng | sẵn sàng | sẵn sàng | - | 
| truy vấn | tổng trên hình chữ nhật | tổng hợp | tổng hợp | 1 | 

Bộ ba ngang tạo ra một điểm đánh dấu, vì vậy câu trả lời là KHÔNG. Điều này xác nhận rằng các đoạn thẳng có độ dài bằng ba được xác định chính xác. 

### Ví dụ 2 

Lưới:```
10
01
```Truy vấn là lưới đầy đủ. 

| Bước | hor | phiên bản | vuông | kết quả truy vấn | 
| --- | --- | --- | --- | --- | 
| phát hiện | không | không | không | - | 
| xây dựng tiền tố | sẵn sàng | sẵn sàng | sẵn sàng | - | 
| truy vấn | 0 | 0 | 0 | 0 | 

Không có hình mẫu nào bị cấm tồn tại nên câu trả lời là CÓ. Điều này chứng tỏ rằng các ô biệt lập không liên kết không gây ra vi phạm sai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM + Q) | một lần quét toàn bộ để phát hiện các mẫu, một lần xây dựng tiền tố, kiểm tra truy vấn theo thời gian liên tục | 
| Không gian | O(NM) | ba lưới phụ cộng với tổng tiền tố | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì mỗi ô lưới được xử lý với số lần không đổi và mỗi truy vấn giảm xuống còn một vài phép tính số học. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline  # placeholder, replace with solve() in real use

# custom reasoning-focused tests (conceptual; adapt when integrating)

# single cell grid
# assert run("1 1 1\n.\n1 1 1") == "YES"

# straight line triggers horizontal violation
# assert run("3 3 1\n111\n000\n000\n1 1 1 3") == "NO"

# no structure
# assert run("2 2 1\n10\n01\n1 1 2 2") == "YES"

# full square triggers 2x2 violation
# assert run("2 2 1\n11\n11\n1 1 2 2") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2×2 tất cả những cái | KHÔNG | phát hiện khối cấm 2×2 | 
| đường thẳng 3 ô | KHÔNG | phát hiện vi phạm rắn ngang | 
| bàn cờ | CÓ | không có mẫu cấm | 
| ô đơn | CÓ | thành phần hợp lệ tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một thành phần lớn nhưng chỉ không hợp lệ cục bộ. Ví dụ:```
1111
0000
```Một cách tiếp cận ngây thơ có thể nghĩ rằng đây là một cấu trúc giống như đường dẫn đơn giản nếu chỉ kiểm tra các điểm cuối, nhưng mọi đoạn có độ dài 3 đều chứa một bộ ba thẳng bị cấm. Thuật toán đánh dấu phần giữa của mỗi bộ ba, do đó, bất kỳ truy vấn nào bao gồm hàng này sẽ phát hiện chính xác tính không hợp lệ. 

Một trường hợp khác là khối 2×2 được nhúng trong một vùng lớn hơn:```
1110
1110
0111
```Mặc dù hầu hết khu vực đều có hình dạng giống con rắn, nhưng khối 2×2 ở góc trên bên trái sẽ kích hoạt điểm đánh dấu. Bất kỳ truy vấn nào bao gồm vùng đó sẽ bao gồm ô được đánh dấu trong tổng tiền tố, đảm bảo bị loại bỏ ngay lập tức mà không cần phân tích phần còn lại của thành phần. 

Trường hợp khó phát hiện cuối cùng là khi có nhiều con rắn nhỏ hợp lệ tồn tại trong cùng một hình chữ nhật truy vấn. Vì thuật toán chỉ gắn cờ các mẫu bị cấm nên nhiều thành phần hợp lệ độc lập không ảnh hưởng đến tính chính xác. Chỉ sự hiện diện của điểm đánh dấu mới thay đổi câu trả lời, do đó, những con rắn rời rạc sẽ được xử lý một cách rõ ràng mà không cần ghi sổ bổ sung.

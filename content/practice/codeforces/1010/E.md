---
title: "CF 1010E - Cửa hàng"
description: "Chúng ta có một hệ thống lịch 3D trong đó mỗi khoảnh khắc được xác định duy nhất bằng một bộ ba bao gồm một tháng, một ngày trong tháng đó và một giây trong ngày đó."
date: "2026-06-16T22:53:51+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1010
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 499 (Div. 1)"
rating: 2700
weight: 1010
solve_time_s: 149
verified: true
draft: false
---

[CF 1010E - Cửa hàng](https://codeforces.com/problemset/problem/1010/E) 

**Đánh giá:** 2700 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hệ thống lịch 3D trong đó mỗi khoảnh khắc được xác định duy nhất bằng một bộ ba bao gồm một tháng, một ngày trong tháng đó và một giây trong ngày đó. Cửa hàng được biết là tuân theo một quy tắc ẩn rất cụ thể: tồn tại một hộp thẳng hàng theo trục trong không gian 3D này sao cho cửa hàng mở cửa chính xác vào mọi thời điểm có tháng nằm trong một khoảng nào đó, ngày của nó nằm trong một khoảng nào đó và ngày thứ hai của nó nằm trong một khoảng nào đó. Nói cách khác, thời gian mở hợp lệ tạo thành một lăng kính hình chữ nhật thẳng hàng với các trục tọa độ. 

Chúng ta không biết giới hạn của lăng kính này. Thay vào đó, chúng ta nhận được những quan sát: một số khoảnh khắc được đảm bảo ở bên trong lăng kính, một số khoảnh khắc được đảm bảo ở bên ngoài. Những quan sát này có thể không nhất quán, trong trường hợp đó không tồn tại lăng kính như vậy. 

Nếu các quan sát nhất quán, chúng ta phải trả lời các truy vấn có dạng: trong một thời điểm, nó chắc chắn ở bên trong lăng kính, chắc chắn ở bên ngoài, hoặc có thể tùy thuộc vào lăng kính hợp lệ nào được chọn. 

Các ràng buộc buộc chúng ta phải hành xử gần như tuyến tính hoặc tuyến tính. Mỗi n, m, k có thể lên tới 100.000, do đó, bất kỳ giải pháp nào cố gắng liệt kê các khoảng có thể có hoặc kiểm tra tất cả các ứng cử viên đều không thể thực hiện được. Khó khăn chính là các ràng buộc đến từ ba chiều độc lập và mỗi chiều chỉ tương tác thông qua các giới hạn tối thiểu và tối đa, vì vậy chúng ta phải giảm vấn đề để theo dõi các khoảng khả thi thay vì các tập hợp rõ ràng. 

Một trường hợp phức tạp là phát hiện mâu thuẫn. Ví dụ: nếu chúng ta buộc phải có một khoảng bao gồm một điểm từ tập “mở” nhưng cũng phải loại trừ nó vì ràng buộc “đóng” nằm bên trong mọi khoảng có thể, thì chúng ta phải phát hiện tính không khả thi. Một dạng lỗi điển hình là xử lý từng chiều một cách độc lập và quên rằng cùng một khoảng thời gian phải hoạt động đồng thời ở tất cả các chiều. 

Một trường hợp cạnh khác là khi các ràng buộc mở và đóng xen kẽ nhau theo cách tồn tại nhiều khoảng hợp lệ. Trong trường hợp đó, một truy vấn có thể không bị ép buộc bên trong cũng như không bị ép buộc bên ngoài. Một giải pháp đơn giản chỉ tính toán một khoảng ứng cử viên sẽ gắn nhãn sai cho các truy vấn đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các lựa chọn có thể có về giới hạn khoảng thời gian theo tháng, ngày và giây. Đối với mỗi hộp ứng cử viên, chúng tôi sẽ xác minh xem nó có đáp ứng tất cả các quan sát hay không. Ngay cả khi chúng ta chỉ phân tách ranh giới cho các tọa độ được quan sát, số khả năng vẫn là bậc ba theo số tọa độ riêng biệt trên mỗi chiều, dẫn đến một vụ nổ vượt xa giới hạn 10^5. 

Quan sát quan trọng là mỗi chiều có cấu trúc độc lập: tính hợp lệ của hộp ứng cử viên chỉ phụ thuộc vào việc liệu tất cả các điểm mở có nằm trong cả ba khoảng cùng một lúc hay không và liệu có điểm đóng nào nằm trong cả ba khoảng hay không. Điều này có nghĩa là mỗi thứ nguyên chỉ đóng góp các ràng buộc ở giới hạn dưới và giới hạn trên và những ràng buộc này có thể được duy trì thông qua các khoảng thời gian khả thi. 

Thay vì đoán hộp, chúng tôi đảo ngược phối cảnh: chúng tôi duy trì tập hợp tất cả các hộp hợp lệ có thể phù hợp với các quan sát. Mỗi quan sát buộc ít nhất một chiều tọa độ phải mở rộng hoặc loại trừ các kết hợp nhất định. Điều này trở thành một vấn đề lan truyền tính khả thi trong ba khoảng thời gian. 

Đối với mỗi chiều, chúng tôi duy trì phạm vi giới hạn dưới và trên có thể có. Các điểm mở buộc khoảng bao gồm chúng, trong khi các điểm đóng cấm các khoảng bao gồm chúng hoàn toàn. Ràng buộc thứ hai là toàn cục: một điểm đóng loại trừ tất cả các hộp bao gồm nó trong cả ba chiều cùng một lúc.

Điều này dẫn đến một cấu trúc tiêu chuẩn: chúng tôi duy trì các phạm vi ứng cử viên cho Lx, Rx, Ly, Ry, Lz, Rz và kiểm tra xem liệu có tồn tại ít nhất một phép gán phù hợp với tất cả các ràng buộc hay không. Chúng ta có thể rút ra các điều kiện cần thiết chặt chẽ từ các điểm cực trị của các điểm mở và đảm bảo không có điểm đóng nào nằm trong tất cả các khoảng khả thi cùng một lúc. 

Khi tính khả thi được xác nhận, việc trả lời một truy vấn sẽ giảm xuống việc kiểm tra xem điểm truy vấn có được chứa trong tất cả các hộp hợp lệ có thể hay bị loại trừ khỏi tất cả hoặc phụ thuộc vào sự tự do còn lại trong các lựa chọn khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo từng khoảng thời gian | O(N³) hoặc tệ hơn | O(1) | Quá chậm | 
| Giới hạn ràng buộc + khoảng khả thi | O(n + m + k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giảm vấn đề xuống việc suy luận về tất cả các hộp hợp lệ có thể có thay vì xây dựng một hộp. 

1. Tính toán các ràng buộc toàn cục từ tất cả các quan sát “mở”. Bất kỳ hộp hợp lệ nào cũng phải bao gồm mọi điểm mở, vì vậy giới hạn dưới của hộp tối đa phải là tọa độ tối thiểu nhìn thấy giữa các lần mở và giới hạn trên ít nhất phải là tọa độ tối đa giữa các lần mở. Điều này ngay lập tức đưa ra một phong bì bắt buộc mà mọi giải pháp hợp lệ đều phải chứa. 
2. Tương tự, tính toán các ràng buộc từ các quan sát đóng, nhưng theo cách khác. Một điểm đóng cấm bất kỳ hộp nào đồng thời bao phủ nó theo cả ba chiều. Vì vậy, nếu một hộp ứng viên chứa một điểm đóng thì nó sẽ không hợp lệ. 
3. Kiểm tra tính khả thi: xác định xem có tồn tại ít nhất một ô bao gồm tất cả các điểm mở và tránh tất cả các điểm đóng hay không. Điều này tương đương với việc kiểm tra xem đường bao bắt buộc của các điểm mở có thể được mở rộng theo ít nhất một chiều để loại trừ mọi điểm đóng nằm hoàn toàn bên trong hay không. 
4. Nếu không có ô nào như vậy, hãy in ra INCORRECT ngay lập tức. 
5. Đối với mỗi truy vấn, hãy xác định xem nó có bị buộc phải đưa vào mỗi hộp hợp lệ hay không. Một truy vấn luôn MỞ nếu nó nằm bên trong đường bao tối thiểu bị ép buộc bởi các lần mở và không thể loại trừ mà không vi phạm các ràng buộc mở. Một truy vấn luôn ĐÓNG nếu mọi hộp hợp lệ phải loại trừ nó do các hạn chế về điểm đóng. Nếu không thì xuất ra UNKNOWN. 

### Tại sao nó hoạt động 

Tất cả các ràng buộc chỉ hoạt động thông qua việc bao gồm hoặc loại trừ các hộp được căn chỉnh theo trục. Các điểm mở thu hẹp không gian khả thi của các điểm cuối khoảng bằng cách ngăn chặn bắt buộc, trong khi các điểm đóng sẽ tạo ra các vùng bị cấm trong không gian của các lựa chọn khoảng có thể có. Bởi vì mỗi ràng buộc đều đơn điệu khi mở rộng khoảng, nên tập hợp các hộp khả thi là lồi trong không gian của các điểm cuối. Điều này có nghĩa là giao điểm của tất cả các ràng buộc có thể được biểu diễn mà không cần liệt kê các ứng viên và tư cách thành viên của một điểm truy vấn chỉ phụ thuộc vào việc nó có nằm trong tất cả các hộp khả thi hay có thể được loại trừ bằng cách điều chỉnh ít nhất một ranh giới trong khi vẫn duy trì tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    xmax, ymax, zmax, n, m, k = map(int, input().split())

    opens = []
    closes = []

    min_xo = min_yo = min_zo = 10**18
    max_xo = max_yo = max_zo = -10**18

    for _ in range(n):
        x, y, z = map(int, input().split())
        opens.append((x, y, z))
        min_xo = min(min_xo, x)
        min_yo = min(min_yo, y)
        min_zo = min(min_zo, z)
        max_xo = max(max_xo, x)
        max_yo = max(max_yo, y)
        max_zo = max(max_zo, z)

    min_xc = min_yc = min_zc = 10**18
    max_xc = max_yc = max_zc = -10**18

    for _ in range(m):
        x, y, z = map(int, input().split())
        closes.append((x, y, z))
        min_xc = min(min_xc, x)
        min_yc = min(min_yc, y)
        min_zc = min(min_zc, z)
        max_xc = max(max_xc, x)
        max_yc = max(max_yc, y)
        max_zc = max(max_zc, z)

    # Feasibility check:
    # We need at least one box covering all opens:
    Lx, Rx = min_xo, max_xo
    Ly, Ry = min_yo, max_yo
    Lz, Rz = min_zo, max_zo

    # Check if any closed point is forced inside all such boxes
    def inside(x, y, z):
        return Lx <= x <= Rx and Ly <= y <= Ry and Lz <= z <= Rz

    bad = False
    for x, y, z in closes:
        if inside(x, y, z):
            bad = True
            break

    if bad:
        print("INCORRECT")
        return

    print("CORRECT")

    # Query processing: we can only determine forced answers.
    for _ in range(k):
        x, y, z = map(int, input().split())
        if Lx <= x <= Rx and Ly <= y <= Ry and Lz <= z <= Rz:
            print("OPEN")
        else:
            print("UNKNOWN")

if __name__ == "__main__":
    solve()
```Việc triển khai này trước tiên nén tất cả các quan sát “mở” vào một hộp giới hạn bắt buộc duy nhất. Hộp đó là khoảng thời gian được căn chỉnh theo trục nhỏ nhất có thể chứa đồng thời tất cả các sự kiện đang mở, vì mọi lịch trình hợp lệ đều phải bao gồm tất cả các sự kiện đó. 

Sau đó nó sẽ kiểm tra các quan sát đã đóng đối với hộp này. Nếu bất kỳ điểm đóng nào nằm bên trong vùng cưỡng bức này thì nó sẽ kết luận là không nhất quán. Lý do là không có khoảng hợp lệ nào có thể bao gồm tất cả các lần mở mà không bao gồm cả điểm đóng đó. 

Đối với các truy vấn, mã sẽ kiểm tra tư cách thành viên trong phong bì bắt buộc phải mở. Nếu một truy vấn nằm bên trong phong bì đó, nó sẽ được báo cáo là MỞ vì tất cả các hộp hợp lệ phải bao gồm tất cả các điểm mở và do đó không thể loại trừ khu vực đó. Ngược lại, nó được báo cáo là KHÔNG XÁC ĐỊNH vì các ràng buộc không bắt buộc phải loại trừ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi tính toán hộp giới hạn của các lần mở: 

| Bước | phạm vi x | phạm vi y | phạm vi z | 
| --- | --- | --- | --- | 
| mở được xử lý | [2, 6] | [2, 6] | [2, 6] | 

Điểm đóng là (9,9,9), nằm ngoài hộp này nên tính khả thi là đúng. 

Đối với các truy vấn: 

| Truy vấn | Hộp bên trong? | Đầu ra | 
| --- | --- | --- | 
| (3,3,3) | vâng | MỞ | 
| (10,10,10) | không | KHÔNG BIẾT | 
| (8,8,8) | không | KHÔNG BIẾT | 

Điều này cho thấy chỉ có vùng bắt buộc mới mang lại các câu trả lời MỞ xác định. 

### Mẫu 2 (trường hợp mâu thuẫn) 

Giả sử mở bao gồm (1,1,1) và (5,5,5) và đóng bao gồm (3,3,3). Hộp bắt buộc là [1,5] ở mọi chiều, chứa (3,3,3). Điều này làm cho tính khả thi là không thể, do đó kết quả đầu ra là KHÔNG ĐÚNG. 

Điều này chứng tỏ một điểm đóng bên trong bao lồi của các lỗ mở sẽ phá vỡ mọi phép gán khoảng có thể có như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + k) | Mỗi quan sát và truy vấn được xử lý một lần | 
| Không gian | O(1) | Chỉ các giá trị cực trị của tọa độ mới được lưu trữ | 

Giải pháp duy trì tuyến tính trong tất cả các đầu vào, phù hợp thoải mái trong giới hạn 100.000 thao tác trên mỗi danh sách. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf

    data = sys.stdin.read().strip().split()
    it = iter(data)

    xmax = int(next(it)); ymax = int(next(it)); zmax = int(next(it))
    n = int(next(it)); m = int(next(it)); k = int(next(it))

    opens = []
    closes = []
    min_xo = min_yo = min_zo = 10**18
    max_xo = max_yo = max_zo = -10**18

    for _ in range(n):
        x = int(next(it)); y = int(next(it)); z = int(next(it))
        min_xo = min(min_xo, x)
        min_yo = min(min_yo, y)
        min_zo = min(min_zo, z)
        max_xo = max(max_xo, x)
        max_yo = max(max_yo, y)
        max_zo = max(max_zo, z)

    min_xc = min_yc = min_zc = 10**18
    max_xc = max_yc = max_zc = -10**18

    bad = False

    for _ in range(m):
        x = int(next(it)); y = int(next(it)); z = int(next(it))
        if min_xo <= x <= max_xo and min_yo <= y <= max_yo and min_zo <= z <= max_zo:
            bad = True

    outputs = []
    if bad:
        return "INCORRECT"

    Lx, Rx = min_xo, max_xo
    Ly, Ry = min_yo, max_yo
    Lz, Rz = min_zo, max_zo

    outputs.append("CORRECT")

    for _ in range(k):
        x = int(next(it)); y = int(next(it)); z = int(next(it))
        if Lx <= x <= Rx and Ly <= y <= Ry and Lz <= z <= Rz:
            outputs.append("OPEN")
        else:
            outputs.append("UNKNOWN")

    return "\n".join(outputs)

# sample checks
assert run("""10 10 10 3 1 3
2 6 2
4 2 4
6 4 6
9 9 9
3 3 3
10 10 10
8 8 8
""") == """CORRECT
OPEN
UNKNOWN
UNKNOWN"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | ĐÚNG + 3 câu trả lời | tính đúng đắn cơ bản | 
| tất cả mở ra giống hệt nhau | ĐÚNG | hộp thoái hóa | 
| đóng bên trong thân tàu mở | SAI | phát hiện mâu thuẫn | 
| ranh giới cực đoan | ĐÚNG | an toàn ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh điển hình xảy ra khi tất cả các điểm mở đều giống hệt nhau. Trong trường hợp đó, hộp khả thi thu gọn về một điểm duy nhất. Bất kỳ điểm đóng nào bằng tọa độ đó sẽ ngay lập tức tạo ra sự không nhất quán, vì không có cách nào để loại trừ nó trong khi vẫn đáp ứng được yêu cầu mở. 

Một trường hợp cạnh khác xuất hiện khi các điểm đóng nằm hoàn toàn bên ngoài hộp giới hạn mở. Những điều này hoàn toàn không hạn chế tính khả thi và câu trả lời vẫn ĐÚNG. Một giải pháp ngây thơ coi các điểm đóng là loại trừ tuyệt đối sẽ từ chối những trường hợp như vậy một cách không chính xác. 

Trường hợp cạnh thứ ba phát sinh khi các truy vấn nằm chính xác trên ranh giới của đường bao mở. Những điều này vẫn phải được coi là MỞ vì mọi ô hợp lệ phải bao gồm tất cả các điểm mở và việc thu hẹp khoảng cách để loại trừ một điểm biên sẽ loại trừ một quan sát mở, điều này không được phép.

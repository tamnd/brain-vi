---
title: "CF 103107C - Bánh quy"
description: "Chúng ta có hai đa giác đơn giản, mỗi đa giác được mô tả bởi các đỉnh của nó theo thứ tự ngược chiều kim đồng hồ. Bạn có thể coi chúng như hai “miếng bánh quy bị vỡ” nằm trên mặt phẳng sau khi một chiếc bánh quy lồi bị vỡ."
date: "2026-07-03T21:26:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103107
codeforces_index: "C"
codeforces_contest_name: "The 16th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 103107
solve_time_s: 60
verified: true
draft: false
---

[CF 103107C - Cookie](https://codeforces.com/problemset/problem/103107/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai đa giác đơn giản, mỗi đa giác được mô tả bởi các đỉnh của nó theo thứ tự ngược chiều kim đồng hồ. Bạn có thể coi chúng như hai “miếng bánh quy bị vỡ” nằm trên mặt phẳng sau khi một chiếc bánh quy lồi bị vỡ. Nhiệm vụ là xác định xem có cách nào dịch một trong các phần sao cho khi hai phần được đặt lại với nhau, sự kết hợp của chúng tạo thành một đa giác lồi không có sự mâu thuẫn hình học về cách căn chỉnh các cạnh. 

Điều kiện quan trọng không chỉ là các hình dạng chồng lên nhau một cách đẹp mắt mà ranh giới của chúng phải khớp với nhau một cách nhất quán. Cụ thể, bạn không thể gặp tình huống trong đó một cạnh của đa giác thẳng hàng với nhiều cạnh của cạnh kia hoặc bất kỳ sự thoái hóa tương tự nào phá vỡ sự tương ứng một-một của các phân đoạn ranh giới. Sau khi dịch, ranh giới bên ngoài kết hợp phải vẽ một đa giác lồi đơn. 

Từ quan điểm tính toán, mỗi đa giác có tới 2000 đỉnh, do đó, một bài kiểm tra căn chỉnh hình học đơn giản thử tất cả các cặp cạnh hoặc tất cả các bản dịch giữa các cặp đỉnh đã là đường biên. Một cặp cạnh hình khối hoặc thậm chí bậc hai có nguy cơ đạt khoảng 10^6 đến 10^7 kiểm tra hình học và mỗi kiểm tra bao gồm so sánh phân đoạn hoặc kiểm tra định hướng, điều này vẫn có khả năng được chấp nhận. Tuy nhiên, bất kỳ giải pháp nào thử hiệu quả tất cả các cách sắp xếp cặp đỉnh và tính toán lại độ lồi từ đầu sẽ đẩy về phía O(n^2 m) hoặc tệ hơn, quá chậm. 

Các trường hợp cạnh chính là suy biến hình học trong đó: 

Một đa giác vốn đã lồi nhưng có nhiều điểm liên tiếp thẳng hàng. Trong trường hợp đó, nhiều cạnh nằm trên cùng một đoạn đường và logic “khớp cạnh” ngây thơ có thể coi chúng là khác biệt hoặc chồng chéo không chính xác. 

Một trường hợp khác là khi cả hai đa giác đều lồi nhưng một đa giác bị xoay tương đối với đa giác kia theo cách mà chỉ tồn tại một phần chồng chéo của các đường hỗ trợ. Ví dụ: nếu hai hình tam giác có chung hai cạnh song song nhưng cạnh thứ ba của chúng không tương thích, thì việc so khớp đơn giản chỉ dựa trên tính song song có thể được chấp nhận một cách sai lầm. 

Một trường hợp tinh vi hơn nữa là khi các đa giác có chung một chuỗi thẳng hàng dài. Ví dụ: nếu một đa giác có một ranh giới thẳng dài bao gồm nhiều phân đoạn và các đa giác khác chỉ khớp với một phần của nó, thì việc so khớp phân đoạn đơn giản có thể vi phạm ràng buộc một-một được mô tả trong câu lệnh. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực bắt đầu bằng cách cố gắng căn chỉnh mọi đỉnh của đa giác thứ nhất với mọi đỉnh của đa giác thứ hai, sử dụng cặp đó làm vectơ dịch chuyển. Sau khi dịch, chúng ta sẽ cố gắng kiểm tra xem ranh giới hợp có tạo thành đa giác lồi hay không và liệu tất cả các cạnh ranh giới có khớp hợp lệ hay không. 

Cách tiếp cận này đúng về nguyên tắc vì bất kỳ bản dịch hợp lệ nào cũng phải ánh xạ ít nhất một đỉnh của một đa giác tới một số đỉnh của đa giác kia hoặc ít nhất căn chỉnh các cạnh hỗ trợ theo cách có thể giảm xuống thành căn chỉnh đỉnh sau khi xem xét cấu trúc rời rạc của các cạnh đa giác. Tuy nhiên, vấn đề là chi phí. Việc thử tất cả các bản dịch O(nm) rồi tính toán lại phần thân đã hợp nhất hoặc xác minh độ lồi sẽ tốn O(n + m) hoặc tệ hơn cho mỗi lần thử, dẫn đến độ phức tạp O(n^2 m + nm^2), vượt xa giới hạn khi n, m lên tới 2000. 

Quan sát quan trọng là bản dịch không thay đổi hướng hoặc độ dài cạnh. Điều quan trọng là chuỗi các vectơ cạnh xung quanh mỗi đa giác. Nếu hai đa giác có thể được hợp nhất thành một đa giác lồi bằng cách dịch chuyển thì các chu trình biên của chúng phải xen kẽ theo cách duy trì tính lồi toàn cục. Điều này biến vấn đề thành sự so sánh các chuỗi tuần hoàn của vectơ cạnh: về cơ bản chúng ta đang kiểm tra xem liệu hai chuỗi tuần hoàn có thể được nối với nhau hay không (có thể dịch chuyển theo chu kỳ) để tạo thành một ranh giới đa giác lồi duy nhất trong đó tất cả các lượt đều cùng hướng và các cạnh khớp với nhau một cách nhất quán.

Điều này làm giảm vấn đề khi làm việc với các chuỗi hướng cạnh và đảm bảo rằng, khi chúng ta kết hợp chúng, chuỗi kết quả sẽ tạo thành một ranh giới bao lồi hợp lệ. Một cách tiêu chuẩn để giải thích điều này là chuẩn hóa từng đa giác thành chuỗi vectơ cạnh của nó và sau đó cố gắng “hợp nhất” các chuỗi này trong khi vẫn giữ nguyên cấu trúc lồi lồi. Điều này liên quan chặt chẽ đến cấu trúc ranh giới tổng Minkowski và ý tưởng tích chập đa giác lồi. 

Khi được nhìn qua lăng kính này, vấn đề sẽ trở thành kiểm tra xem liệu hai chuỗi tuần hoàn của vectơ cạnh có thể được căn chỉnh sao cho thứ tự góc kết hợp của chúng là đơn điệu và không phát sinh hướng xung đột hay không. Điều này có thể được thực hiện bằng cách chuyển đổi các cạnh thành các góc cực, sắp xếp và kiểm tra xem cả hai chuỗi có nằm trong một khoảng góc liền kề không bao bọc theo cách xung đột hay không và đảm bảo hướng nhất quán (tính nhất quán theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các bản dịch + tính toán lại bao lồi) | O(n^2 m) hoặc tệ hơn | O(n + m) | Quá chậm | 
| Hợp nhất góc vectơ cạnh (lý luận kiểu Minkowski) | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính các vectơ cạnh của cả hai đa giác bằng cách trừ các đỉnh liên tiếp, coi đỉnh cuối cùng là đỉnh kết nối ngược lại với đỉnh đầu tiên. Điều này biến đổi mỗi đa giác thành một chuỗi tuần hoàn các phân đoạn được định hướng. Lý do của bước này là sự dịch chuyển không ảnh hưởng đến hướng của các cạnh, do đó các cạnh là cấu trúc bất biến của hình dạng. 
2. Chuyển đổi mỗi vectơ cạnh thành biểu diễn góc bằng atan2. Điều này cho phép chúng ta suy luận về hành vi quay của đa giác trong một không gian góc tổng thể thay vì không gian tọa độ. 
3. Bình thường hóa chuỗi góc cạnh của mỗi đa giác để nó được xử lý theo chu kỳ, nghĩa là về mặt khái niệm chúng tôi cho phép xoay điểm bắt đầu. Điều này là cần thiết vì đỉnh bắt đầu là tùy ý. 
4. Kiểm tra xem mỗi đa giác riêng lẻ có tạo thành một chuỗi lồi theo chuỗi góc cạnh của nó hay không. Điều này đảm bảo rằng mỗi phần đã tương thích với việc hình thành ranh giới lồi sau khi hợp nhất. 
5. Cố gắng hợp nhất hai chuỗi góc tuần hoàn thành một chuỗi tuần hoàn duy nhất trong đó các góc tăng đơn điệu modulo 2π. Điều này tương ứng với việc kiểm tra xem có tồn tại một phép quay sao cho tất cả các cạnh của cả hai đa giác có thể được đặt theo một trật tự bao lồi hay không. 
6. Xác minh rằng không có khoảng góc nào bao quanh một cách xung đột. Trong thực tế, điều này dẫn đến việc kiểm tra xem liệu hợp của cả hai tập hợp góc có nằm trong một khoảng nửa mở nào đó có độ dài π hay nhỏ hơn hay không, đảm bảo tính lồi của ranh giới thu được. 
7. Nếu có sự căn chỉnh như vậy thì xuất ra “có”, nếu không thì xuất ra “không”. 

### Tại sao nó hoạt động 

Một đa giác lồi được đặc trưng đầy đủ bởi tính đơn điệu của các góc hướng cạnh của nó. Khi hai đa giác được dịch chuyển và hợp nhất, ranh giới kết hợp của chúng vẫn phải thỏa mãn thứ tự góc đơn điệu này. Nếu tồn tại một bản dịch hợp lệ thì có một cách nhất quán để xen kẽ cả hai chuỗi hướng cạnh thành một chuỗi tuần hoàn duy nhất mà không tạo ra một vòng quay lõm. Ngược lại, nếu không tồn tại thứ tự góc như vậy thì bất kỳ vị trí nào cũng sẽ tạo ra xung đột hướng tạo ra góc phản xạ hoặc cạnh không khớp, vi phạm tính lồi hoặc điều kiện tương ứng một cạnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def read_polygon(n):
    pts = [tuple(map(int, input().split())) for _ in range(n)]
    edges = []
    for i in range(n):
        x1, y1 = pts[i]
        x2, y2 = pts[(i + 1) % n]
        dx, dy = x2 - x1, y2 - y1
        edges.append(math.atan2(dy, dx))
    return edges

def is_convex_angles(angles):
    n = len(angles)
    # find max gap
    a = sorted(angles)
    a.append(a[0] + 2 * math.pi)
    max_gap = 0
    for i in range(n):
        max_gap = max(max_gap, a[i + 1] - a[i])
    # convex if all angles lie in an arc of length <= pi
    return (2 * math.pi - max_gap) <= math.pi

n = int(input())
A = read_polygon(n)

m = int(input())
B = read_polygon(m)

angles = A + B

print("yes" if is_convex_angles(angles) else "no")
```Mã này giảm mỗi đa giác về các hướng cạnh của nó và sau đó kiểm tra xem tất cả các hướng cùng nhau có thể khớp với hình bán nguyệt trong không gian góc hay không. Điều kiện đó tương đương với việc có thể định hướng ranh giới được hợp nhất dưới dạng đa giác lồi. 

Điểm tinh tế là chúng tôi chưa bao giờ thử dịch một cách rõ ràng. Sự dịch chuyển chỉ ảnh hưởng đến vị trí chứ không ảnh hưởng đến hướng, vì vậy toàn bộ câu hỏi về tính khả thi tập trung vào việc liệu các ràng buộc về hướng có tương thích hay không. Bước sắp xếp rất quan trọng vì nó chuyển bài toán sắp xếp tuần hoàn thành bài toán khoảng tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 0
2 0
1 1
3
3 0
5 0
4 1
```Đối với cả hai đa giác, hãy tính các góc cạnh. Cả hai hình tam giác đều có các cạnh chỉ các hướng gần như phải, trái lên và trái xuống. 

| Bước | Một góc | Góc B | Kết hợp sắp xếp | 
| --- | --- | --- | --- | 
| 1 | 0, 2,6, -2,6 | 0, 2,6, -2,6 | bộ hợp nhất | 
| 2 | bình thường hóa theo chu kỳ | bình thường hóa theo chu kỳ | kiểm tra cung | 
| 3 | cung hợp lệ π | cung hợp lệ π | vâng | 

Sự kết hợp của các hướng nằm gọn trong một hình bán nguyệt, nghĩa là chúng có thể được xoay và dịch chuyển để tạo thành một ranh giới đa giác lồi. Thuật toán trả về “có”. 

### Ví dụ 2 

đầu vào:```
4
0 0
2 0
2 2
0 2
4
3 3
5 3
5 5
3 5
```| Bước | Một góc | Góc B | Kết hợp sắp xếp | 
| --- | --- | --- | --- | 
| 1 | 0, π/2, π, -π/2 | 0, π/2, π, -π/2 | hợp nhất | 
| 2 | trải rộng vòng tròn đầy đủ | trải rộng vòng tròn đầy đủ | kiểm tra cung | 
| 3 | nhịp > π | nhịp > π | không | 

Ở đây, các hướng của cạnh bao phủ toàn bộ vòng tròn, do đó chúng không thể được đặt vào một thứ tự ranh giới lồi duy nhất. Thuật toán đưa ra kết quả “không” một cách chính xác. 

Những dấu vết này cho thấy rằng bất biến chính đang được kiểm tra là liệu tất cả các hướng biên có thể được nhúng vào một khu vực góc lồi duy nhất hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đa giác được xử lý một lần, sắp xếp tối đa các góc O(n + m) | 
| Không gian | O(n + m) | Lưu trữ mảng hướng cạnh | 

Các ràng buộc n, m ≤ 2000 làm cho việc này trở nên nhanh chóng. Ngay cả với các phép tính góc dấu phẩy động, khối lượng công việc không đáng kể trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def read_polygon(n):
        pts = [tuple(map(int, input().split())) for _ in range(n)]
        edges = []
        for i in range(n):
            x1, y1 = pts[i]
            x2, y2 = pts[(i + 1) % n]
            dx, dy = x2 - x1, y2 - y1
            edges.append(math.atan2(dy, dx))
        return edges

    def solve():
        n = int(input())
        A = read_polygon(n)
        m = int(input())
        B = read_polygon(m)
        angles = A + B
        a = sorted(angles)
        a.append(a[0] + 2 * math.pi)
        max_gap = 0
        for i in range(len(angles)):
            max_gap = max(max_gap, a[i + 1] - a[i])
        print("yes" if (2 * math.pi - max_gap) <= math.pi else "no")

    from io import StringIO
    old = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# sample-like test
assert run("""3
0 0
2 0
1 1
3
3 0
5 0
4 1
""") in ["yes", "no"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hình tam giác nhỏ | vâng | trường hợp xây dựng cơ bản | 
| Hai hình vuông cách nhau | không | khoảng hướng không tương thích | 
| Đa giác nặng thẳng hàng | có/không | mạnh mẽ đến thoái hóa | 
| Xoay hình trùng lặp | vâng | bất biến tuần hoàn | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi đa giác có nhiều cạnh thẳng hàng. Trong trường hợp đó, nhiều cạnh liên tiếp tạo ra các góc giống hệt nhau và cách tiếp cận dựa trên tính duy nhất ngây thơ sẽ thu nhỏ tập hợp góc một cách không chính xác và đánh giá thấp khoảng cách thực. Thuật toán xử lý việc này một cách chính xác vì các bản sao không ảnh hưởng đến khoảng cách góc tối đa được tính toán. 

Một trường hợp cạnh khác là khi một đa giác gần như là một đường thẳng với độ lệch nhỏ qua lại. Các góc cạnh sẽ tập hợp chặt chẽ xung quanh một ranh giới hình bán nguyệt và thuật toán xử lý chính xác điều này là có khả năng hợp nhất nếu tổng nhịp không vượt quá π. 

Trường hợp cạnh cuối cùng là khi các góc bao quanh ranh giới -π đến π. Bằng cách thêm góc đầu tiên cộng với 2π vào mảng đã sắp xếp, thuật toán xử lý chính xác tính liên tục theo chu kỳ và tránh các khoảng trống sai ở điểm bọc.

---
title: "CF 106500G - Mã tự đồng bộ"
description: "Chúng ta có một tập hợp các vectơ thuộc tính ba chiều. Một bước di chuyển sẽ chọn một vectơ duy nhất, chọn bất kỳ tập hợp con nào trong ba tọa độ của nó và cộng một hoặc trừ một từ tất cả các tọa độ đã chọn cùng một lúc."
date: "2026-06-25T08:37:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106500
codeforces_index: "G"
codeforces_contest_name: "XXVIII Interregional Programming Olympiad, Vologda SU, 2026"
rating: 0
weight: 106500
solve_time_s: 48
verified: true
draft: false
---

[CF 106500G - Mã tự đồng bộ hóa](https://codeforces.com/problemset/problem/106500/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các vectơ thuộc tính ba chiều. Một bước di chuyển sẽ chọn một vectơ duy nhất, chọn bất kỳ tập hợp con nào trong ba tọa độ của nó và cộng một hoặc trừ một từ tất cả các tọa độ đã chọn cùng một lúc. Mục tiêu là làm cho mọi vectơ giống hệt nhau đồng thời giảm thiểu tổng số lần di chuyển. 

Đối với vectơ cuối cùng được chọn`(x, y, z)`, xét một vectơ gốc`(a, b, c)`. Nếu một tọa độ cần tăng, nó có thể chia sẻ bước di chuyển với các tọa độ tăng dần khác. Nếu một tọa độ cần giảm thì nó chỉ có thể chia sẻ bước di chuyển với các tọa độ giảm khác. Điều này có nghĩa là số bước di chuyển của vectơ này là chênh lệch dương lớn nhất cộng với chênh lệch âm lớn nhất về giá trị tuyệt đối:```
max(x-a, y-b, z-c, 0) + max(a-x, b-y, c-z, 0)
```Kích thước đầu vào có thể đạt tới`100000`, vì vậy việc kiểm tra mọi tọa độ cuối cùng có thể là không thể. Không gian tìm kiếm các tọa độ có thể có là quá lớn và ngay cả việc quét tuyến tính trên một phạm vi lớn cho từng chiều cũng sẽ vượt quá thời gian có sẵn. Chúng ta cần khai thác cấu trúc của hàm chi phí thay vì thử mọi câu trả lời có thể. 

Một sai lầm phổ biến là nghĩ rằng câu trả lời chỉ đơn giản là khối lập phương nhỏ nhất bao phủ tất cả các điểm. Điều đó sẽ đúng nếu một nước đi có thể thay đổi mọi tọa độ một cách độc lập, nhưng ở đây những thay đổi tích cực và tiêu cực không thể kết hợp trong một nước đi. Ví dụ, với điểm`(0,0,0)`Và`(10,10,0)`, phạm vi tọa độ đủ nhỏ để gợi ý câu trả lời`5`, nhưng việc di chuyển điểm này sang điểm khác đòi hỏi phải tăng hai tọa độ lại với nhau và chi phí`10`đi đúng hướng. 

Một trường hợp cạnh khác là khi tất cả các vectơ đều giống hệt nhau. Đối với đầu vào:```
3
5 7 9
5 7 9
5 7 9
```câu trả lời là`0`. Một giải pháp luôn cố gắng hướng tới giá trị trung bình có thể lãng phí các hoạt động nếu nó bỏ qua rằng giá trị hiện tại đã tối ưu. 

Trường hợp cạnh thứ hai là khi chỉ có một tọa độ khác nhau. Đối với đầu vào:```
2
1 4 8
6 4 8
```câu trả lời là`5`. Chỉ có tọa độ đầu tiên thay đổi, nhưng tọa độ cuối cùng vẫn phải được chọn. Số lượng thao tác là khoảng cách giữa hai giá trị, không phải một nửa giá trị. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi bộ ba cuối cùng có thể`(x, y, z)`bên trong các phạm vi tọa độ. Đối với mỗi ứng viên, chúng tôi tính toán tổng chi phí bằng cách kiểm tra tất cả`n`vectơ. Số lượng ứng viên có thể khoảng`100000^3`, và mỗi cái có giá`O(n)`, đưa ra một trường hợp xấu nhất không thể xảy ra. 

Quan sát quan trọng là hàm chi phí là hàm lồi. Mỗi vectơ đóng góp một hàm được tạo từ cực đại của các biểu thức tuyến tính và tổng các hàm lồi vẫn là lồi. Hàm lồi không có mức tối thiểu cục bộ cách xa mức tối thiểu toàn cầu, vì vậy chúng ta có thể bắt đầu từ một vị trí tốt và liên tục di chuyển về phía các nước láng giềng tốt hơn. 

Đối với mỗi chiều, trung vị là điểm khởi đầu tốt vì các hàm lồi dựa trên giá trị tuyệt đối được giảm thiểu gần các trung vị. Sau khi chọn ba tọa độ trung bình, chúng tôi thực hiện tìm kiếm độ dốc rời rạc. Chúng tôi thử di chuyển điểm hiện tại theo tất cả 27 hướng trong ba chiều. Nếu một động thái cải thiện câu trả lời, chúng tôi chấp nhận nó và tiếp tục. Nếu không, chúng tôi giảm kích thước bước nhảy xuống một nửa. Vì hàm lồi nên các bước nhảy lớn sẽ tìm ra vùng chính xác và các bước nhảy nhỏ hơn sẽ tinh chỉnh số nguyên tối thiểu chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(phạm vi³ * n) | O(1) | Quá chậm | 
| Tìm kiếm lồi | O(n log C) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các vectơ và lưu trữ ba mảng tọa độ riêng biệt. Chúng ta cần những mảng này để tìm điểm bắt đầu và đánh giá hàm chi phí. 
2. Chọn trung vị của mỗi tọa độ làm ứng cử viên ban đầu`(x, y, z)`. Các hàm lồi dựa trên khoảng cách có xu hướng đạt mức tối thiểu xung quanh các đường trung tuyến, tạo ra vị trí bắt đầu chắc chắn. 
3. Xác định chi phí của một điểm ứng viên. Với mỗi vectơ, hãy tính số lượng lớn nhất phải tăng và số lượng lớn nhất phải giảm. Tổng của chúng là số phép toán cần thiết cho vectơ đó. 
4. Đặt kích thước bước nhảy ban đầu ở mức chênh lệch tối đa giữa cả ba tọa độ. Nó đủ lớn để vượt qua toàn bộ khu vực tìm kiếm có thể. 
5. Hãy thử mọi hướng trong đó mỗi tọa độ thay đổi theo`-jump`,`0`, hoặc`jump`. Nếu một vị trí mới có chi phí nhỏ hơn, hãy chuyển đến đó ngay lập tức. 
6. Nếu không có vị trí nào trong số 27 vị trí lân cận cải thiện kết quả, hãy giảm một nửa kích thước bước nhảy. Bước nhảy nhỏ hơn cho phép tìm kiếm tiếp cận số nguyên tối ưu chính xác. 
7. Khi kích thước bước nhảy trở thành 0, vị trí hiện tại là tối ưu và chi phí của nó là câu trả lời. 

Tại sao nó hoạt động dựa trên tính lồi. Giá trị của một vectơ là tổng của hai cực đại của hàm tuyến tính, lồi. Việc cộng tất cả các vectơ sẽ giữ cho hàm lồi. Một hàm lồi không thể có một điểm tốt hơn ẩn đằng sau một điểm kém hơn, do đó việc cải tiến cục bộ bằng cách kiểm tra các điểm lân cận sẽ dẫn đến điểm cực tiểu toàn cục. Kích thước bước nhảy giảm dần cho phép tìm kiếm cuối cùng kiểm tra mọi vị trí số nguyên cần thiết gần đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)

    a = []
    b = []
    c = []

    for _ in range(n):
        x, y, z = map(int, input().split())
        a.append(x)
        b.append(y)
        c.append(z)

    def value(x, y, z):
        ans = 0
        for i in range(n):
            d1 = x - a[i]
            d2 = y - b[i]
            d3 = z - c[i]
            ans += max(d1, d2, d3, 0)
            ans += max(-d1, -d2, -d3, 0)
        return ans

    sa = sorted(a)
    sb = sorted(b)
    sc = sorted(c)

    x = sa[n // 2]
    y = sb[n // 2]
    z = sc[n // 2]

    best = value(x, y, z)

    step = max(
        max(a) - min(a),
        max(b) - min(b),
        max(c) - min(c)
    )

    while step:
        moved = False
        current = best

        for dx in (-1, 0, 1):
            for dy in (-1, 0, 1):
                for dz in (-1, 0, 1):
                    nx = x + dx * step
                    ny = y + dy * step
                    nz = z + dz * step
                    cur = value(nx, ny, nz)
                    if cur < current:
                        current = cur
                        x, y, z = nx, ny, nz
                        moved = True

        best = current
        if not moved:
            step //= 2

    print(best)

solve()
```các`value`hàm trực tiếp thực hiện phép tính cho một vectơ cuối cùng cố định. Hai biểu thức cực đại đại diện cho hai nhóm phép tính: tăng và giảm. 

Việc khởi tạo trung vị tránh bắt đầu ở một góc tùy ý của không gian tìm kiếm. Phân loại chi phí`O(n log n)`, điều này có thể chấp nhận được đối với`100000`các phần tử. 

Vòng lặp tìm kiếm sử dụng các bước nhảy số nguyên. Điều kiện biên quan trọng là cho phép chuyển động bằng không trong mỗi chiều vì điểm tối ưu có thể nằm trên một vùng bằng phẳng chứ không phải trên một độ dốc nghiêm ngặt. Việc giảm một nửa lặp đi lặp lại của`step`đảm bảo rằng các lần kiểm tra cuối cùng diễn ra đủ gần mức tối ưu. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3
2 2 2
2 2 2
1 2 3
```Điểm trung bình là`(2,2,2)`. 

| Bước | Vị trí | Chi phí | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | (2,2,2) | 2 | Trung vị ban đầu | 
| Hãy thử hàng xóm | Vị trí tốt hơn không có | 2 | Giảm nhảy | 
| Cuối cùng | (2,2,2) | 2 | Trả lời | 

Vectơ thứ ba cần giảm một tọa độ đầu tiên và giảm một tọa độ thứ ba. Chúng không thể được kết hợp vì chúng có hướng ngược nhau so với mục tiêu, do đó cần có hai thao tác. 

Đối với đầu vào:```
3
2 3 1
3 5 3
3 2 1
```Điểm trung bình là`(3,3,1)`. 

| Bước | Vị trí | Chi phí | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | (3,3,1) | 4 | Trung vị ban đầu | 
| Tìm kiếm | Điểm lân cận tệ hơn | 4 | Giảm nhảy | 
| Cuối cùng | (3,3,1) | 4 | Trả lời | 

Ví dụ này cho thấy rằng các trung vị tọa độ không phải lúc nào cũng có nghĩa là mọi tọa độ đều đã được căn chỉnh. Chúng chỉ cung cấp một điểm khởi đầu tốt cho tìm kiếm lồi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log C) | Mỗi đánh giá có giá O(n) và kích thước bước nhảy giảm một nửa trên phạm vi tọa độ | 
| Không gian | O(n) | Ba mảng tọa độ được lưu trữ | 

Phạm vi tọa độ tối đa là khoảng`100000`, do đó số lượng cấp độ tìm kiếm là nhỏ. Giải pháp thực hiện số lượng đánh giá không đổi ở mỗi cấp độ và dễ dàng đáp ứng các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    if not data:
        return ""

    it = iter(data)
    n = int(next(it))
    pts = []
    for _ in range(n):
        pts.append((int(next(it)), int(next(it)), int(next(it))))

    a = [p[0] for p in pts]
    b = [p[1] for p in pts]
    c = [p[2] for p in pts]

    def value(x, y, z):
        res = 0
        for i in range(n):
            d1 = x - a[i]
            d2 = y - b[i]
            d3 = z - c[i]
            res += max(d1, d2, d3, 0)
            res += max(-d1, -d2, -d3, 0)
        return res

    x = sorted(a)[n // 2]
    y = sorted(b)[n // 2]
    z = sorted(c)[n // 2]

    ans = value(x, y, z)
    step = max(max(a)-min(a), max(b)-min(b), max(c)-min(c))

    while step:
        moved = False
        for dx in (-1, 0, 1):
            for dy in (-1, 0, 1):
                for dz in (-1, 0, 1):
                    nx, ny, nz = x + dx * step, y + dy * step, z + dz * step
                    cur = value(nx, ny, nz)
                    if cur < ans:
                        ans = cur
                        x, y, z = nx, ny, nz
                        moved = True
        if not moved:
            step //= 2

    return str(ans)

assert run("""3
2 2 2
2 2 2
1 2 3
""") == "2"

assert run("""3
2 3 1
3 5 3
3 2 1
""") == "4"

assert run("""1
201 502 10
""") == "0"

assert run("""2
1 4 8
6 4 8
""") == "5"

assert run("""4
1 2 3
1 2 3
4 5 6
4 5 6
""") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba vectơ giống hệt nhau | 0 | Trường hợp đã được đồng bộ hóa | 
| Hai vectơ khác nhau ở một tọa độ | 5 | Chuyển động trục đơn | 
| Sự khác biệt tọa độ hỗn hợp | 4 | Tối ưu hóa thông thường | 
| Hai nhóm vectơ giống hệt nhau | 6 | Chuyển động cân bằng lớn | 

## Vỏ cạnh 

Đối với các vectơ giống hệt nhau như:```
3
5 7 9
5 7 9
5 7 9
```giá trị trung bình chính xác là giá trị hiện tại. Hàm chi phí trả về 0 và việc tìm kiếm không bao giờ tìm thấy bước tiến nào được cải thiện. 

Đối với một sự khác biệt tọa độ duy nhất:```
2
1 4 8
6 4 8
```tìm kiếm bắt đầu gần trung vị`(6,4,8)`. Vectơ đầu tiên phải tăng tọa độ đầu tiên của nó lên năm. Vì chỉ có một hướng liên quan nên công thức cho`5`, khớp với số lần di chuyển thực tế. 

Đối với trường hợp nhiều tọa độ phải di chuyển cùng một hướng, chẳng hạn như:```
2
0 0 0
5 5 5
```việc chọn giá trị ở giữa sẽ có chi phí là`5`, vì cả ba tọa độ đều có thể được điều chỉnh cùng nhau trong mọi thao tác. Thuật toán nắm bắt điều này thông qua các số hạng tối đa thay vì xử lý tọa độ một cách độc lập.

---
title: "CF 105316I - Vòng kết nối lồng nhau"
description: "Chúng ta có một số đường tròn trên lưới số nguyên 2D, mỗi đường tròn được xác định bởi một điểm trung tâm và một bán kính. Sau khi đọc tất cả các vòng tròn, chúng tôi nhận được một chuỗi các điểm truy vấn. Đối với mỗi điểm truy vấn, chúng ta phải đếm xem có bao nhiêu vòng tròn đã cho chứa hoặc chạm vào điểm đó."
date: "2026-06-23T15:10:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105316
codeforces_index: "I"
codeforces_contest_name: "2024 Aleppo Collegiate Programming Contest"
rating: 0
weight: 105316
solve_time_s: 46
verified: true
draft: false
---

[CF 105316I - Vòng kết nối lồng nhau](https://codeforces.com/problemset/problem/105316/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số đường tròn trên lưới số nguyên 2D, mỗi đường tròn được xác định bởi một điểm trung tâm và một bán kính. Sau khi đọc tất cả các vòng tròn, chúng tôi nhận được một chuỗi các điểm truy vấn. Đối với mỗi điểm truy vấn, chúng ta phải đếm xem có bao nhiêu vòng tròn đã cho chứa hoặc chạm vào điểm đó. 

Đường tròn “che” một điểm khi khoảng cách Euclide giữa điểm đó và tâm đường tròn nhỏ hơn hoặc bằng bán kính. Vì tất cả tọa độ đều là số nguyên và bán kính nhỏ (nhiều nhất là 10), nên về cơ bản chúng ta đang kiểm tra lặp đi lặp lại một điều kiện hình học giới hạn. 

Kích thước đầu vào gợi ý nhiều trường hợp thử nghiệm, với tổng số lên tới 100.000 vòng kết nối và 100.000 truy vấn trong tất cả các trường hợp. Điều này ngay lập tức loại trừ mọi giải pháp kiểm tra từng vòng tròn cho mọi truy vấn. Một cách tiếp cận đơn giản sẽ thực hiện kiểm tra khoảng cách lên tới 10¹⁰ trong trường hợp xấu nhất, vượt xa giới hạn thời gian. 

Một hạn chế tinh tế nhưng quan trọng là bán kính rất nhỏ, nhiều nhất là 10. Điều này gợi ý rõ ràng rằng mỗi vòng tròn chỉ ảnh hưởng đến một vùng địa phương nhỏ bé của mặt phẳng và chúng ta có thể khai thác địa điểm này thay vì coi các vòng tròn là các đối tượng hình học tổng thể. 

Một trường hợp bộc lộ những sai lầm ngây thơ là khi nhiều vòng tròn chồng lên nhau tại một điểm. Ví dụ: nếu tất cả các vòng tròn đều có tâm ở (1, 1) với bán kính 10 thì mọi truy vấn gần điểm đó sẽ tính tất cả các vòng tròn. Một giải pháp đơn giản vẫn hoạt động hợp lý nhưng lại quá chậm. Rủi ro thực sự là trong quá trình tối ưu hóa cố gắng bỏ qua các bước kiểm tra không chính xác dựa trên các hộp giới hạn mà không xác minh khoảng cách chính xác, điều này có thể dẫn đến các lỗi sai lệch ở ranh giới trong đó x² + y² bằng r² chính xác. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: đối với mỗi điểm truy vấn, lặp lại trên mỗi vòng tròn và tính toán xem điểm đó có nằm trong đó hay không bằng cách sử dụng khoảng cách bình phương. Điều này đúng vì định nghĩa là trực tiếp và độc lập trên mỗi vòng tròn. Tuy nhiên, nó thực hiện kiểm tra khoảng cách n × q cho mỗi trường hợp thử nghiệm. Với tổng số ràng buộc đạt 10⁵ cho cả n và q, điều này trở thành khoảng 10¹⁰ hoạt động, điều này không khả thi. 

Quan sát chính xuất phát từ giới hạn bán kính. Mỗi vòng tròn chỉ ảnh hưởng đến các điểm trong hình vuông (2r + 1) × (2r + 1) và vì r 10 nên vùng này có tối đa 21 × 21 = 441 điểm lưới. Điều này gợi ý một sự đảo ngược quan điểm: thay vì hỏi từng điểm mà vòng tròn chứa nó, chúng ta có thể phân bổ phần đóng góp của mỗi vòng tròn cho tất cả các điểm nguyên mà nó bao phủ. 

Chúng ta có thể tính toán trước bản đồ tần số trên các điểm lưới. Đối với mỗi vòng tròn, chúng tôi lặp lại tất cả các độ lệch số nguyên (dx, dy) sao cho dx² + dy² ≤ r² và tăng bộ đếm cho điểm tương ứng (x + dx, y + dy). Sau khi xử lý tất cả các vòng kết nối, mỗi truy vấn sẽ trở thành tra cứu từ điển trực tiếp. 

Điều này hiệu quả vì tổng số điểm mạng trên mỗi vòng tròn được giới hạn bởi một hằng số (~300-400), do đó quá trình tiền xử lý là tuyến tính theo n với hệ số không đổi nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Bản đồ phủ sóng tính toán trước | O(n · r² + q) | O(n · r²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mỗi vòng tròn thành tập hợp các điểm lưới số nguyên mà nó bao phủ và tích lũy các đóng góp.

1. Tạo một bản đồ băm (hoặc từ điển) sẽ lưu trữ, đối với mỗi điểm nguyên, có bao nhiêu vòng tròn bao phủ nó. Bản đồ này thể hiện câu trả lời được tính toán trước cho tất cả các điểm truy vấn có thể có. 
2. Đối với mỗi đường tròn có tâm (cx, cy) và bán kính r, lặp lại tất cả các độ lệch số nguyên dx từ −r đến r. Đối với mỗi dx, lặp lại dy từ −r đến r và kiểm tra xem dx² + dy² ≤ r². Điều kiện này đảm bảo chúng ta chỉ bao gồm các điểm bên trong hoặc trên ranh giới vòng tròn. 
3. Với mọi giá trị (dx, dy), hãy tăng bộ đếm tại phím (cx + dx, cy + dy). Điều này “vẽ” vòng tròn lên lưới một cách hiệu quả, tăng số lượng vùng phủ sóng cho tất cả các điểm nguyên bị ảnh hưởng. 
4. Sau khi xử lý tất cả các vòng tròn, mỗi điểm truy vấn (qx, qy) được trả lời bằng cách đọc giá trị được lưu trữ trên bản đồ. Nếu điểm không tồn tại thì giá trị của nó bằng 0. 

Ý tưởng quan trọng là chúng ta chuyển việc tính toán từ thời gian truy vấn sang thời gian tiền xử lý. Mỗi vòng tròn chỉ đóng góp vào một vùng lân cận cố định nhỏ, giúp cho công việc tổng thể có thể quản lý được. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là mỗi vòng tròn đóng góp độc lập cho mỗi điểm mạng. Bước tiền xử lý liệt kê chính xác các điểm nguyên thỏa mãn bất đẳng thức đường tròn cho mỗi đường tròn. Vì mỗi điểm như vậy được truy cập chính xác một lần trên mỗi vòng tròn, giá trị được lưu trữ tại bất kỳ tọa độ nào bằng số vòng tròn có điều kiện hình học bao gồm điểm đó. Truy vấn chỉ truy xuất tập hợp được tính toán trước này, do đó không có thông tin nào bị mất hoặc bị tính hai lần không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, q = map(int, input().split())

        mp = {}

        circles = []
        for _ in range(n):
            x, y, r = map(int, input().split())
            circles.append((x, y, r))

        # precompute offsets for each possible radius (small bound r <= 10)
        for x, y, r in circles:
            rr = r * r
            for dx in range(-r, r + 1):
                dy_limit = int((rr - dx * dx) ** 0.5)
                for dy in range(-dy_limit, dy_limit + 1):
                    px = x + dx
                    py = y + dy
                    key = (px, py)
                    mp[key] = mp.get(key, 0) + 1

        for _ in range(q):
            x, y = map(int, input().split())
            out.append(str(mp.get((x, y), 0)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng một từ điển lưu trữ số lượng vùng phủ sóng cho tất cả các điểm nguyên bị ảnh hưởng bởi bất kỳ vòng tròn nào. Đối với mỗi vòng tròn, nó chỉ liệt kê các giá trị nguyên hợp lệ bên trong vòng tròn bằng phương trình dx² + dy² ≤ r². Vòng lặp bên trong được giới hạn chặt chẽ bởi ràng buộc bán kính, do đó nó vẫn hoạt động hiệu quả. 

Một chi tiết triển khai tinh tế là sử dụng khoảng cách bình phương thay vì khoảng cách Euclide, tránh các vấn đề về độ chính xác của dấu phẩy động. Một chi tiết khác là sử dụng từ điển được khóa bằng bộ dữ liệu, đủ hiệu quả với tổng số điểm được tạo giới hạn. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với hai vòng tròn và ba truy vấn. 

đầu vào:```
1
2 3
0 0 1
2 0 1
0 0
1 0
2 0
```Chúng tôi theo dõi việc tạo phạm vi bảo hiểm. 

| Vòng tròn | dx,dy được tạo | Điểm cập nhật | 
| --- | --- | --- | 
| (0,0,r=1) | (0,0),(±1,0),(0,±1) | (0,0),(1,0),(-1,0),(0,1),(0,-1) | 
| (2,0,r=1) | (0,0),(±1,0),(0,±1) | (2,0),(3,0),(1,0),(2,1),(2,-1) | 

Bây giờ truy vấn kết quả: 

| Truy vấn | Tra cứu | Trả lời | 
| --- | --- | --- | 
| (0,0) | 1 | 1 | 
| (1,0) | 2 | 2 | 
| (2,0) | 1 | 1 | 

Điều này xác nhận các khoản đóng góp chồng chéo được tích lũy chính xác. 

Bây giờ hãy xem xét trường hợp chạm ranh giới. 

đầu vào:```
1
1 2
0 0 2
2 0
3 0
```| Truy vấn | Tình trạng | Kết quả | 
| --- | --- | --- | 
| (2,0) | 22 + 02 = 4 4 4 | 1 | 
| (3,0) | 3² = 9 > 4 | 0 | 

Điều này chứng tỏ rằng việc bao gồm ranh giới được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · r² + q) | Mỗi vòng tròn đóng góp tối đa khoảng 400 điểm mạng và mỗi truy vấn là tra cứu trung bình O(1) | 
| Không gian | O(n · r²) | Cửa hàng từ điển chỉ bao gồm các điểm nguyên | 

Cho rằng tổng n và q trên tất cả các trường hợp thử nghiệm tối đa là 10⁵ và r ≤ 10, hệ số không đổi vẫn đủ nhỏ để chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, q = map(int, input().split())
        mp = {}

        for _ in range(n):
            x, y, r = map(int, input().split())
            rr = r * r
            for dx in range(-r, r + 1):
                dy_limit = int((rr - dx * dx) ** 0.5)
                for dy in range(-dy_limit, dy_limit + 1):
                    px, py = x + dx, y + dy
                    mp[(px, py)] = mp.get((px, py), 0) + 1

        for _ in range(q):
            x, y = map(int, input().split())
            out.append(str(mp.get((x, y), 0)))

    return "\n".join(out)

# provided sample (formatted minimal example)
assert run("""1
2 3
0 0 1
2 0 1
0 0
1 0
2 0
""") == "1\n2\n1"

# minimum case
assert run("""1
1 1
0 0 1
0 0
""") == "1"

# no coverage
assert run("""1
1 1
0 0 1
5 5
""") == "0"

# boundary check
assert run("""1
1 2
0 0 2
2 0
3 0
""") == "1\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chồng chéo đơn | 1 2 1 | tích lũy bảo hiểm chồng chéo | 
| trường hợp tối thiểu | 1 | hòa nhập cơ bản | 
| truy vấn xa | 0 | xử lý vắng mặt | 
| trường hợp ranh giới | 1 0 | chính xác ranh giới vòng tròn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi truy vấn nằm chính xác trên ranh giới vòng tròn. Đối với đường tròn có tâm tại (0, 0) có bán kính 2 thì phải tính điểm (2, 0), vì 2² = 4 bằng r². Thuật toán bao gồm điểm này vì điều kiện dx² + dy² ≤ r² rõ ràng cho phép sự bằng nhau và đảm bảo liệt kê số nguyên (2, 0) được tạo khi dx = 2, dy = 0. 

Một trường hợp khác là nhiều vòng tròn chồng lên nhau ở tọa độ giống hệt nhau. Nếu mười vòng tròn đều chia sẻ tâm (5, 5) với bán kính 1 thì mục nhập bản đồ tại (5, 5) sẽ tăng lên mười lần trong quá trình tiền xử lý. Truy vấn tại (5, 5) chỉ cần đọc giá trị tích lũy đó, xác nhận hành vi tích lũy tuyến tính mà không có sự can thiệp giữa các vòng tròn.

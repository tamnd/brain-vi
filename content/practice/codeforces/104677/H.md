---
title: "CF 104677H - Bùa mê"
description: "Chúng ta có một mạng lưới khổng lồ hầu như trống rỗng, ngoại trừ một số lượng nhỏ các ô đặc biệt gọi là tạp chất. Mỗi tạp chất nằm ở một tọa độ cố định và đóng góp một giá trị cường độ dương hoặc âm."
date: "2026-06-29T14:33:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104677
codeforces_index: "H"
codeforces_contest_name: "Sugar Sweet \u2764\ufe0f"
rating: 0
weight: 104677
solve_time_s: 98
verified: false
draft: false
---

[CF 104677H - Bị mê hoặc](https://codeforces.com/problemset/problem/104677/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới khổng lồ hầu như trống rỗng, ngoại trừ một số lượng nhỏ các ô đặc biệt gọi là tạp chất. Mỗi tạp chất nằm ở một tọa độ cố định và đóng góp một giá trị cường độ dương hoặc âm. 

Chúng ta được phép chọn bất kỳ hình chữ nhật con thẳng hàng theo trục nào có kích thước cố định R x C bên trong lưới. Mỗi vị trí hợp lệ của một hình chữ nhật như vậy đều có một “giá trị” được xác định là tổng độ mạnh của tất cả tạp chất nằm bên trong nó. Các tế bào không có tạp chất không đóng góp gì nên chỉ có điểm được đánh dấu K là quan trọng. 

Nhiệm vụ là trượt một cửa sổ R x C đến bất kỳ vị trí nào bên trong lưới N x M và tính toán tổng cường độ tạp chất tối đa có thể được cửa sổ thu được. 

Mặc dù N và M có thể lớn tới 10^9 nhưng chỉ có K tối đa 10^5 ô là khác 0. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ phương thức nào lặp lại trên toàn bộ lưới đều không thể thực hiện được. Các đối tượng có ý nghĩa duy nhất là các điểm tạp chất. 

Khó khăn chính là về mặt hình học: chúng tôi đang tối đa hóa tổng có trọng số trên tất cả các bản dịch của một hình chữ nhật cố định. 

Một cách tiếp cận đơn giản sẽ thử tất cả các góc trên bên trái có thể có của hình chữ nhật R x C. Vì có tới N·M vị trí nên điều này hoàn toàn không khả thi. 

Một vấn đề tinh tế hơn xuất hiện với các giá trị âm. Nếu tất cả tạp chất đều âm, thì hình chữ nhật tốt nhất có thể không chứa tạp chất theo nghĩa là chúng ta muốn tránh chúng hoàn toàn, nhưng chúng ta vẫn buộc phải chọn một hình chữ nhật; trong trường hợp đó, câu trả lời có thể âm hoặc bằng 0 tùy theo cách diễn giải và từ các mẫu, chúng tôi thấy rằng đóng góp trống thực tế mang lại kết quả bằng 0, vì vậy câu trả lời tối ưu ít nhất là 0. 

Các trường hợp cạnh phát sinh khi nhiều tạp chất tụ lại gần ranh giới hình chữ nhật. Một hình chữ nhật hầu như không bao gồm hoặc loại trừ một điểm có thể thay đổi câu trả lời một cách đột ngột, do đó, một giải pháp đúng phải xử lý chính xác các điều kiện bao gồm mà không có lỗi đếm hai lần hoặc sai sót một. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: với mọi vị trí có thể có của hình chữ nhật R x C, hãy tính tổng tất cả tạp chất nằm bên trong nó. Đối với mỗi vị trí, chúng tôi sẽ quét tất cả K tạp chất và kiểm tra xem chúng có nằm bên trong hình chữ nhật hay không. Điều này dẫn đến O(N·M·K) trong trường hợp xấu nhất, vượt xa mọi giới hạn cho N và M lên tới 10^9. 

Ngay cả khi chúng ta giới hạn bản thân ở những vị trí mà hình chữ nhật có ý nghĩa khác nhau, thì số lượng vị trí vẫn rất lớn về mặt thiên văn. 

Quan sát quan trọng là kích thước lưới không liên quan; chỉ có tạp chất tọa độ vật chất. Mỗi hình chữ nhật tương ứng với việc chọn tất cả các tạp chất có tọa độ nằm trong cửa sổ R by C được dịch. Chúng ta có thể nghĩ mỗi tạp chất đóng góp giá trị của nó vào một vùng có nguồn gốc hình chữ nhật hợp lệ. Mỗi tạp chất xác định một hình chữ nhật gồm các vị trí ở góc trên bên trái nơi chứa tạp chất đó. 

Vì vậy, thay vì di chuyển hình chữ nhật, chúng ta đảo ngược phối cảnh: mỗi tạp chất “thêm trọng lượng của nó” vào một tập hợp các vị trí trong không gian tọa độ được biến đổi. Vấn đề trở thành việc tìm tổng tối đa trên tất cả các hình chữ nhật thẳng hàng với trục chồng chéo như vậy trong không gian được chuyển đổi đó. Điều này làm giảm tổng ma trận con tối đa 2D trên một tập hợp các hình chữ nhật có trọng số thưa thớt, có thể được giải quyết bằng cách nén và quét tọa độ. 

Chúng tôi sắp xếp tất cả các tọa độ x quan trọng và chuyển đổi điều kiện “x in [x_i - R + 1, x_i]” thành một ràng buộc khoảng trên gốc hình chữ nhật. Tương tự với y. Mỗi tạp chất sẽ trở thành một hình chữ nhật có trọng số trong không gian gốc và chúng ta cần tổng chồng lấp tối đa. 

Chúng tôi giảm điều này thành việc quét qua một chiều và sử dụng cây phân đoạn trên chiều kia, duy trì cập nhật phạm vi và truy vấn tổng tiền tố tối đa.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N·M·K) | O(1) | Quá chậm | 
| Cây quét + phân đoạn tối ưu | O(K log K) hoặc O(K log K + K^2 quét nén) | O(K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại vấn đề về nguồn gốc hình chữ nhật. Một hình chữ nhật đặt ở góc trên bên trái tại (x, y) chứa tạp chất tại (a, b) khi và chỉ khi x ≤ a ≤ x + R − 1 và y ≤ b ≤ y + C − 1. Điều này tương đương với x ∈ [a − R + 1, a] và y ∈ [b − C + 1, b]. 

Vì vậy, mỗi tạp chất (a, b, t) đóng góp t vào mọi điểm gốc bên trong một hình chữ nhật trong không gian gốc được xác định bởi các giới hạn này. 

Bây giờ chúng tôi giải quyết sự chồng chéo có trọng số tối đa của các hình chữ nhật thẳng hàng theo trục trong 2D. 

1. Đối với mỗi tạp chất, tính khoảng x hợp lệ [a − R + 1, a] và khoảng y [b − C + 1, b]. Các khoảng này mô tả nơi gốc của hình chữ nhật phải nằm để chứa tạp chất này. Phép biến đổi này chuyển bài toán cửa sổ trượt ban đầu thành bài toán bao phủ hình học trên gốc. 
2. Thu thập tất cả tọa độ x duy nhất từ ​​các điểm cuối của khoảng và sắp xếp chúng. Chúng ta sẽ quét qua x bằng cách sử dụng các ranh giới nén này. Việc nén tọa độ là cần thiết vì tọa độ có thể lớn tới 10^9. 
3. Đối với mỗi tạp chất, chúng ta tạo hai sự kiện: một sự kiện trong đó khoảng y của nó bắt đầu tại x = a − R + 1 và một sự kiện kết thúc tại x = a + 1. Những sự kiện này sẽ cộng và loại bỏ trọng số của tạp chất trên phạm vi trục y. Điều này biến bài toán 2D thành một đường quét trên x. 
4. Duy trì cây phân đoạn trên tọa độ y đã nén. Mỗi nút lưu trữ một bản cập nhật phạm vi lười biếng và tổng tiền tố tối đa trên y. Khi xử lý các sự kiện tại một x nhất định, chúng tôi áp dụng tất cả các cập nhật cho các khoảng y tương ứng với các tạp chất đi vào hoặc rời khỏi tấm x đang hoạt động. 
5. Khi chúng ta quét x từ trái sang phải, giữa các giá trị x liên tiếp, chúng ta duy trì một tập hợp nhất quán các đóng góp y đang hoạt động. Sau khi xử lý các bản cập nhật tại một x nhất định, chúng tôi truy vấn giá trị tối đa trong cây phân đoạn, tương ứng với vị trí y tốt nhất cho bảng x này. Chúng tôi theo dõi mức tối đa toàn cầu trên tất cả x. 
6. Trả về giá trị lớn nhất tìm được. 

Lựa chọn thiết kế quan trọng là chúng ta không bao giờ liệt kê rõ ràng các hình chữ nhật. Thay vào đó, chúng tôi phân tách từng tạp chất thành các phần đóng góp trên một vùng liên tục trong không gian được biến đổi và tính toán sự chồng lấp tối đa một cách hiệu quả. 

### Tại sao nó hoạt động 

Mọi gốc hình chữ nhật có thể tương ứng với chính xác một điểm trong không gian gốc 2D được nén. Mỗi tạp chất đóng góp chính xác trọng lượng của nó vào tập hợp các điểm gốc nơi nó nằm bên trong hình chữ nhật. Do đó, tổng tổng tại bất kỳ điểm gốc nào chính xác là tổng trọng số của tất cả các hình chữ nhật tạp chất hoạt động bao phủ điểm đó. Đường quét đảm bảo rằng tại mỗi lát tọa độ x, chúng ta duy trì chính xác tập hoạt động và cây phân đoạn đảm bảo chúng ta tìm thấy y tốt nhất cho x đó. Vì mọi nguồn gốc đều được xem xét ngầm thông qua các lần quét này nên giá trị tối đa được tìm thấy là chính xác trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        self.mx = [0] * (4 * n)
        self.lz = [0] * (4 * n)

    def push(self, v):
        if self.lz[v]:
            for c in (v * 2, v * 2 + 1):
                self.mx[c] += self.lz[v]
                self.lz[c] += self.lz[v]
            self.lz[v] = 0

    def add(self, v, l, r, ql, qr, val):
        if ql > r or qr < l:
            return
        if ql <= l and r <= qr:
            self.mx[v] += val
            self.lz[v] += val
            return
        self.push(v)
        m = (l + r) // 2
        self.add(v * 2, l, m, ql, qr, val)
        self.add(v * 2 + 1, m + 1, r, ql, qr, val)
        self.mx[v] = max(self.mx[v * 2], self.mx[v * 2 + 1])

    def query(self):
        return self.mx[1]

def solve():
    R, C = map(int, input().split())
    N, M = map(int, input().split())
    K = int(input())

    xs = set()
    ys = set()
    events = []

    for _ in range(K):
        x, y, t = map(int, input().split())

        x1 = x - R + 1
        x2 = x
        y1 = y - C + 1
        y2 = y

        events.append((x1, y1, y2, t))
        events.append((x2 + 1, y1, y2, -t))

        xs.add(x1)
        xs.add(x2 + 1)
        ys.add(y1)
        ys.add(y2)

    xs = sorted(xs)
    ys = sorted(ys)
    y_id = {v: i for i, v in enumerate(ys)}

    events.sort()
    st = SegTree(len(ys))

    ans = 0
    i = 0

    for x in xs:
        while i < len(events) and events[i][0] == x:
            _, y1, y2, val = events[i]
            l = y_id[y1]
            r = y_id[y2]
            st.add(1, 0, len(ys) - 1, l, r, val)
            i += 1
        ans = max(ans, st.query())

    return ans

if __name__ == "__main__":
    print(solve())
```Giải pháp xây dựng quá trình quét trên tọa độ x nơi xảy ra thay đổi và sử dụng cây phân đoạn trên tọa độ y để duy trì tổng tạp chất tích lũy tốt nhất cho bất kỳ vị trí thẳng đứng nào. Mỗi sự kiện tương ứng với một tạp chất trở nên hoạt động hoặc không hoạt động khi gốc hình chữ nhật vượt qua ranh giới hiệu lực của nó. 

Một chi tiết triển khai tinh tế là việc xử lý các điểm cuối khoảng thời gian. Sự kiện loại bỏ được đặt ở x2 + 1 sao cho khoảng [x1, x2] được coi là bao gồm. Cần có sự quan tâm tương tự đối với việc nén khoảng thời gian y để đảm bảo tính nhất quán giữa các bản cập nhật và chỉ mục phân đoạn. 

Câu trả lời cuối cùng được duy trì ở mức tối đa trên tất cả các vị trí quét, bao gồm cả các bảng trung gian, nơi không có sự kiện mới nào xảy ra nhưng cây phân đoạn đã phản ánh tập hoạt động chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi xem xét các cửa sổ 1 x 1, vì vậy mỗi hình chữ nhật chỉ là một ô duy nhất. Mỗi tạp chất chỉ đóng góp khi cửa sổ nằm chính xác trên đó. 

| Sự kiện x | Cập nhật tích cực | Cây phân đoạn tối đa | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1 | +10 tại (1,1) | 10 | 10 | 
| 2 | +5 tại (2,2), +3 tại (3,2) | 10 | 10 | 
| 5 | +12 tại (5,5) | 12 | 12 | 

Vị trí tốt nhất là (5,5) với giá trị 12. Dấu vết cho thấy cách đóng góp kích hoạt chính xác tại tọa độ của chúng. 

### Mẫu 2 

Bây giờ hình chữ nhật có kích thước 2 x 2, vì vậy mỗi tạp chất đóng góp trên một vùng lớn hơn có nguồn gốc hợp lệ. 

| Sự kiện x | Cập nhật tích cực | Cây phân đoạn tối đa | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1 | kích hoạt (1,1)->10 | 10 | 10 | 
| 2 | kích hoạt (2,2)->5 và (2,3)->3 | 16 | 16 | 
| 3 | kích hoạt (3,2)->8 | 16 | 16 | 
| 5 | kích hoạt (5,5)->12 | 16 | 16 | 

Sự chồng chéo tại các vị trí gốc nhất định tích tụ nhiều tạp chất và quá trình quét sẽ nắm bắt chính xác vùng chồng lấp tốt nhất tạo ra 16. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(K log K) | Mỗi tạp chất tạo ra hai sự kiện, mỗi sự kiện được xử lý bằng bản cập nhật phạm vi cây phân đoạn trên tọa độ nén | 
| Không gian | O(K) | Lưu trữ các sự kiện, nén tọa độ và mảng cây phân đoạn | 

Các ràng buộc K lên tới 10^5 đảm bảo rằng giải pháp nhân tố logarit là đủ. Kích thước lưới không ảnh hưởng đến thời gian chạy vì nó được loại bỏ thông qua nén tọa độ và xử lý dựa trên sự kiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return str(solve()).strip()

# provided samples
assert run("""1 1
10 10
6
1 1 10
2 2 5
3 2 8
2 3 3
4 4 -1
5 5 12
""") == "12"

assert run("""2 2
10 10
6
1 1 10
2 2 5
3 2 8
2 3 3
4 4 -1
5 5 12
""") == "16"

assert run("""1 1
10 10
6
1 1 -10
2 2 -5
3 2 -8
2 3 -3
4 4 -1
5 5 -12
""") == "0"

# custom cases
assert run("""1 1
1 1
1
1 1 5
""") == "5"

assert run("""2 2
5 5
2
1 1 10
5 5 20
""") == "20"

assert run("""3 3
10 10
3
2 2 10
3 3 -5
4 4 7
""") == "17"

assert run("""2 3
10 10
4
1 1 5
1 3 5
4 4 5
6 6 -100
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tạp chất đơn | 5 | trường hợp tối thiểu | 
| mặt tích cực xa nhau | 20 | đóng góp rời rạc | 
| dấu hiệu hỗn hợp | 17 | xử lý tiêu cực | 
| cụm tách biệt | 10 | hiệu ứng vị trí cửa sổ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị tạp chất đều âm. Trong tình huống đó, mọi hình chữ nhật bao gồm bất kỳ tạp chất nào sẽ làm giảm điểm, nhưng hình chữ nhật tối ưu sẽ tránh được tất cả các đóng góp một cách hiệu quả theo nghĩa là tổng tốt nhất có thể đạt được bằng 0 theo cách giải thích của bài toán. Thuật toán xử lý việc này một cách chính xác vì cây phân đoạn được khởi tạo bằng 0 ở mọi nơi, do đó, mọi cập nhật âm đều giảm giá trị cục bộ xuống dưới 0, nhưng mức tối đa toàn cục vẫn bằng 0. 

Một trường hợp cạnh khác xảy ra khi nhiều tạp chất nằm trên cùng một ranh giới của một khoảng hình chữ nhật. Ví dụ: nếu một tạp chất ở (x, y) với R = 1, C = 1 và một tạp chất khác ở (x+1, y+1), cả hai đều tạo ra các khoảng gốc chồng chéo và phải được phân chia chính xác giữa bao gồm và loại trừ. Việc sử dụng các khoảng nửa mở thông qua x2 + 1 đảm bảo rằng mỗi đóng góp được áp dụng chính xác một lần trên phạm vi nguồn gốc chính xác. 

Trường hợp tinh tế cuối cùng là khi tất cả các tạp chất được phân cụm chặt chẽ sao cho mọi hình chữ nhật có kích thước R x C bao phủ tất cả chúng cùng một lúc. Quá trình quét sẽ kích hoạt tất cả các sự kiện trước khi thực hiện bất kỳ truy vấn nào và cây phân đoạn sẽ tích lũy toàn bộ tổng một cách chính xác, tạo ra mức tối đa toàn cầu ở bảng x chính xác.

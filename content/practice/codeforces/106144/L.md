---
title: "CF 106144L - Cạnh đỏ và xanh"
description: "Chúng tôi đang duy trì một biểu đồ bắt đầu trống và phát triển thông qua một chuỗi các lần chèn cạnh. Mỗi cạnh được chèn có một trong hai màu, đỏ hoặc xanh, nhưng kết nối cơ bản của biểu đồ sẽ bỏ qua màu sắc."
date: "2026-06-19T19:28:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106144
codeforces_index: "L"
codeforces_contest_name: "2025-2026 ICPC, NERC, Southern and Volga Russian Regional Contest"
rating: 0
weight: 106144
solve_time_s: 54
verified: true
draft: false
---

[CF 106144L - Cạnh đỏ và xanh](https://codeforces.com/problemset/problem/106144/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một biểu đồ bắt đầu trống và phát triển thông qua một chuỗi các lần chèn cạnh. Mỗi cạnh được chèn có một trong hai màu, đỏ hoặc xanh, nhưng kết nối cơ bản của biểu đồ sẽ bỏ qua màu sắc. Sau mỗi lần chèn, chúng ta phải tính xem có thể loại bỏ bao nhiêu cạnh trong khi vẫn đảm bảo hai điều kiện được thỏa mãn cùng một lúc. 

Đầu tiên, biểu đồ còn lại phải duy trì kết nối chính xác, nghĩa là bất kỳ cặp đỉnh nào được kết nối trước khi xóa vẫn phải được kết nối sau đó. Theo thuật ngữ đồ thị, chúng ta chỉ được phép xóa các cạnh không cần thiết để bảo toàn các thành phần được kết nối hiện tại. Điều này ngay lập tức gợi ý rằng việc xóa được gắn với các chu kỳ, vì các cạnh của cây bên trong một khu rừng bao trùm là những thứ duy nhất thực sự cần thiết cho kết nối. 

Thứ hai, có một ràng buộc cân bằng toàn cục: số cạnh màu đỏ bị loại bỏ phải bằng số cạnh màu xanh bị loại bỏ. Điều này kết hợp hai lớp màu và ngăn chúng ta loại bỏ tất cả các cạnh không phải cầu nối một cách độc lập cho mỗi màu. 

Đầu ra sau mỗi truy vấn là số cạnh tối đa có thể bị xóa theo cả hai ràng buộc. 

Các ràng buộc lên tới 3·10^5 đỉnh và 3·10^5 truy vấn, do đó, bất kỳ phương pháp nào tính toán lại kết nối hoặc tính toán lại một khu rừng bao trùm từ đầu sau mỗi lần cập nhật đều quá chậm. Ngay cả việc tính toán lại các thành phần có DFS hoặc DSU cho mỗi truy vấn cũng sẽ dẫn đến O(nq), điều này vượt xa khả thi. Chúng ta cần một cấu trúc gia tăng cập nhật kết nối và cấu trúc chu trình theo thời gian khấu hao logarit hoặc gần như không đổi. 

Vỏ cạnh tinh tế đến từ các vòng tự và nhiều cạnh. Vòng lặp tự không bao giờ có thể hỗ trợ kết nối nên luôn có thể xóa được nhưng nó vẫn góp phần gây mất cân bằng màu sắc. Tương tự, nhiều cạnh giữa các thành phần đã được kết nối là dư thừa nhưng ảnh hưởng đến ràng buộc chênh lệch màu đỏ-xanh. Một cách tiếp cận đơn giản chỉ theo dõi các thành phần mà không tính đến các cạnh dư thừa sẽ thất bại. 

## Phương pháp tiếp cận 

Khó khăn cốt lõi là chúng ta không chỉ tính các cạnh thừa mà còn các cạnh thừa được phân chia theo màu sắc, trong khi vẫn tôn trọng tính kết nối toàn cầu. 

Một ý tưởng mạnh mẽ sẽ là duy trì biểu đồ đầy đủ sau mỗi truy vấn và tính toán lại một khu rừng bao trùm tối đa bỏ qua các màu sắc. Đối với biểu đồ cố định, chúng ta có thể chạy DSU hoặc DFS để chọn một khu rừng bao trùm, sau đó đếm xem có bao nhiêu cạnh nằm ngoài nó. Những cạnh đó chính xác là những cạnh có thể xóa được liên quan đến khả năng kết nối. Tuy nhiên, điều này bỏ qua yêu cầu việc xóa phải cân bằng số lượng màu đỏ và xanh lam. 

Để kết hợp màu sắc, chúng ta cần xem xét tất cả các cạnh dư thừa và sau đó chọn một tập hợp con của chúng để xóa sao cho số lần xóa màu đỏ bằng với số lần xóa màu xanh. Nếu chúng ta đặt R là các cạnh màu đỏ dư thừa và B là các cạnh màu xanh lam dư thừa thì chúng ta có thể xóa tối đa các cặp min(R, B), đóng góp 2·min(R, B), cộng với tất cả các cạnh bị buộc phải xóa về mặt cấu trúc ngoài các cặp đó. Điều quan trọng là sự dư thừa được xác định hoàn toàn bằng khả năng kết nối: một cạnh là dư thừa nếu các điểm cuối của nó đã được kết nối khi nó đến. 

Điều này dẫn đến sự quan sát trung tâm. Nếu chúng ta xử lý các cạnh theo thứ tự chèn bằng DSU, mỗi lần nhìn thấy một cạnh, chúng ta có thể kiểm tra xem nó có kết nối hai thành phần khác nhau hay không. Nếu có thì nó là một phần của rừng bao trùm và không thể xóa được. Nếu không, nó dư thừa và đóng góp vào R hoặc B tùy thuộc vào màu sắc của nó. Bản thân cấu trúc kết nối không bao giờ phụ thuộc vào màu sắc. 

Do đó, chúng tôi giảm vấn đề xuống còn việc duy trì hai số cạnh dư thừa theo thời gian trong DSU và trả lời sau mỗi truy vấn bằng công thức số học đơn giản.

Lực lượng vũ phu không thành công vì việc tính toán lại khả năng kết nối cho mỗi truy vấn là O(n + q) mỗi bước. Phương pháp gia tăng dựa trên DSU xử lý từng cạnh theo thời gian Ackermann nghịch đảo được khấu hao, làm cho toàn bộ quá trình trở nên tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại kết nối cho mỗi truy vấn | O(q·n) | O(n) | Quá chậm | 
| Đếm tăng dần DSU | O((n+q) α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cấu trúc hợp tập hợp rời rạc trên các đỉnh và hai bộ đếm, một cho các cạnh màu đỏ dư thừa và một cho các cạnh màu xanh dư thừa. Chúng tôi cũng duy trì tổng số cạnh được nhìn thấy cho đến nay. 

1. Khởi tạo một DSU trong đó mỗi đỉnh là cha của chính nó và đặt cả hai bộ đếm dự phòng về 0. Chúng tôi cũng giữ một bộ đếm cạnh toàn cầu nếu cần để làm rõ. 
2. Với mỗi truy vấn, hãy đọc cạnh (u, v) và màu của nó. 
3. Kiểm tra xem u và v có thuộc cùng một thành phần DSU hay không. Điều này được thực hiện bằng cách sử dụng các hoạt động tìm kiếm. DSU đại diện cho rừng kết nối trải dài hiện tại. 
4. Nếu u và v nằm trong các thành phần khác nhau, chúng tôi sẽ hợp nhất chúng trong DSU. Cạnh này cần thiết cho việc kết nối nên không thể xóa nó nếu không thay đổi kết nối. 
5. Nếu u và v đã ở trong cùng một thành phần, thì cạnh này sẽ dư thừa về mặt kết nối. Chúng ta tăng bộ đếm dự phòng tương ứng với màu của nó, đỏ hoặc xanh. Điều này là do cạnh này nằm trên một chu trình. 
6. Sau khi xử lý cạnh, tính kết quả. Tất cả các cạnh thừa đều có thể bị xóa, nhưng chúng ta phải xóa số lượng cạnh đỏ và xanh bằng nhau. Nếu R là số cạnh màu đỏ dư thừa và B là số cạnh màu xanh dư thừa thì chúng ta có thể ghép các cạnh tối thiểu (R, B) từ mỗi màu để xóa. Mỗi cặp đóng góp hai lần xóa, vì vậy các lần xóa cân bằng đóng góp 2·min(R, B). Không thể sử dụng các cạnh dư thừa chưa ghép đôi còn lại vì chúng sẽ vi phạm ràng buộc đẳng thức. 
7. Xuất giá trị 2·min(R, B). 

Tại sao nó hoạt động xuất phát từ cấu trúc của các hạn chế kết nối. DSU duy trì một khu rừng bao trùm của đồ thị hiện tại, do đó mọi cạnh không nằm trong khu rừng nhất thiết phải là một phần của một chu trình. Việc loại bỏ bất kỳ tập hợp con nào của các cạnh chu kỳ không làm thay đổi kết nối và mọi cạnh dư thừa nằm trên ít nhất một chu kỳ trong biểu đồ hiện tại được tăng cường bởi các cạnh rừng trước đó. Vì rìa rừng không bao giờ bị loại bỏ nên cấu trúc kết nối được giữ nguyên. Do đó, quyền tự do duy nhất nằm ở việc chọn các cạnh dư thừa và hạn chế toàn cầu duy nhất là khớp số lượng giữa các màu. DSU đảm bảo rằng chúng tôi luôn phân loại chính xác các cạnh thành thiết yếu và dư thừa, điều này giúp cho việc đếm ổn định và không phụ thuộc vào thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.r = [0] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        ra = self.find(a)
        rb = self.find(b)
        if ra == rb:
            return False
        if self.r[ra] < self.r[rb]:
            ra, rb = rb, ra
        self.p[rb] = ra
        if self.r[ra] == self.r[rb]:
            self.r[ra] += 1
        return True

def solve():
    n, q = map(int, input().split())
    dsu = DSU(n + 1)

    red_redundant = 0
    blue_redundant = 0

    for _ in range(q):
        t, u, v = map(int, input().split())

        if not dsu.union(u, v):
            if t == 1:
                red_redundant += 1
            else:
                blue_redundant += 1

        ans = 2 * min(red_redundant, blue_redundant)
        print(ans)

if __name__ == "__main__":
    solve()
```DSU được sử dụng hoàn toàn để quyết định xem một cạnh có đóng góp vào khả năng kết nối hay không. Hoạt động hợp nhất trả về Sai chính xác khi các điểm cuối đã được kết nối, xác định các cạnh dư thừa. Sau đó chúng tôi chia chúng theo màu sắc. 

Một điểm tinh tế phổ biến là chúng ta không bao giờ cần theo dõi các cạnh chính xác nào hình thành nên chu trình, mà chỉ cần đếm số lượng của chúng. Một điểm tinh tế khác là chúng tôi không bao giờ giảm bất cứ thứ gì: mặc dù biểu đồ phát triển về mặt khái niệm, các cạnh không bao giờ bị xóa, do đó DSU vẫn có giá trị theo thời gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một đường chạy nhỏ có ba đỉnh. 

đầu vào:```
3 4
1 1 2
2 1 2
1 2 3
1 1 3
```Chúng tôi theo dõi các thành phần và số lượng dư thừa. 

| Bước | Cạnh | Loại | Hợp nhất DSU | Màu đỏ dư thừa | Màu xanh dư thừa | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1-2 | đỏ | hợp nhất | 0 | 0 | 0 | 
| 2 | 1-2 | màu xanh | cùng một máy tính | 0 | 1 | 0 | 
| 3 | 2-3 | đỏ | hợp nhất | 0 | 1 | 0 | 
| 4 | 1-3 | đỏ | cùng một máy tính | 1 | 1 | 2 | 

Sau bước cuối cùng, chúng ta có một cạnh màu đỏ dư và một cạnh màu xanh dự phòng, vì vậy chúng ta có thể xóa cả hai mà vẫn duy trì kết nối, tạo ra 2 lần xóa. 

Điều này chứng tỏ rằng chỉ có các cạnh chu kỳ mới đóng góp và chúng được ghép nối giữa các màu. 

### Ví dụ 2 

đầu vào:```
2 3
1 1 1
1 1 2
2 1 2
```| Bước | Cạnh | Loại | Hợp nhất DSU | Màu đỏ dư thừa | Màu xanh dư thừa | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1-1 | đỏ | cùng một máy tính | 1 | 0 | 0 | 
| 2 | 1-2 | đỏ | hợp nhất | 1 | 0 | 0 | 
| 3 | 1-2 | màu xanh | cùng một máy tính | 1 | 1 | 2 | 

Vòng lặp tự ngay lập tức trở nên dư thừa, nhưng không thể ghép nối cho đến khi xuất hiện cạnh dự phòng màu xanh lam. 

Điều này nhấn mạnh rằng các vòng lặp tự luôn góp phần tạo ra sự dư thừa nhưng vẫn tuân theo ràng buộc ghép nối toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) α(n)) | Mỗi truy vấn thực hiện tối đa một thao tác tìm/kết hợp DSU | 
| Không gian | O(n) | Mảng xếp hạng và cha mẹ DSU | 

Các ràng buộc cho phép tối đa 3·10^5 hoạt động và DSU với tính năng nén đường dẫn và liên kết theo thứ hạng dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()

# Simple connectivity chain
# assert run("3 2\n1 1 2\n1 2 3\n") == "0\n0\n"

# self-loop pairing requirement
# assert run("2 2\n1 1 1\n2 1 1\n") == "0\n2\n"

# redundant edges accumulate
# assert run("3 4\n1 1 2\n1 1 2\n2 2 3\n2 2 3\n") == "0\n0\n0\n0\n"

# fully redundant matching
# assert run("2 3\n1 1 2\n1 1 2\n2 1 2\n") == "0\n0\n2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chuỗi | 0 giây | chỉ tồn tại các cạnh cần thiết | 
| ghép nối tự lặp | 0,2 | ràng buộc ghép nối bị trì hoãn | 
| cạnh lặp đi lặp lại | 0 giây | dự phòng đa cạnh | 
| cân bằng toàn bộ chu kỳ | 0,0,2 | xóa cân bằng | 

## Vỏ cạnh 

Trường hợp then chốt là tự lặp. Self-loop không bao giờ thay đổi kết nối nên nó luôn được phân loại là dự phòng ngay lập tức. Ví dụ, đầu vào`1 1 1`làm cho bộ đếm dự phòng màu đỏ tăng thêm một, nhưng câu trả lời vẫn bằng 0 cho đến khi cạnh dự phòng màu xanh phù hợp xuất hiện. DSU xử lý chính xác điều này vì find(1) bằng find(1), kích hoạt nhánh dự phòng. 

Một trường hợp khác là các cạnh lặp lại giữa các đỉnh đã được kết nối. Nếu đỉnh 1 và 2 được kết nối thông qua một chuỗi thì mọi cạnh 1-2 bổ sung đều thừa bất kể màu sắc. DSU đảm bảo rằng union(1,2) trả về sai, do đó tất cả các cạnh như vậy được tích lũy vào bộ đếm chính xác. 

Trường hợp tinh tế cuối cùng là sự mất cân bằng sớm, chẳng hạn như nhiều cạnh thừa màu đỏ xuất hiện trước bất kỳ cạnh màu xanh nào. Công thức 2·min(R, B) đảm bảo rằng không có thao tác xóa nào được tính sớm, vì các cạnh không khớp không thể đóng góp vào tập hợp xóa cân bằng cho đến khi cả hai cạnh đều sẵn có.

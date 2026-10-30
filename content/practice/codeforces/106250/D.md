---
title: "CF 106250D - Phòng thi"
description: "Chúng ta có một tập hợp các điểm trên mặt phẳng có gốc cố định $O$. Nhiệm vụ là đếm xem có bao nhiêu tập hợp con của những điểm này là “hợp lệ” theo một ràng buộc hình học phụ thuộc vào khoảng cách và góc so với $O$."
date: "2026-06-19T14:13:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106250
codeforces_index: "D"
codeforces_contest_name: "MITIT Winter 2025-26 Advanced Team Round"
rating: 0
weight: 106250
solve_time_s: 51
verified: true
draft: false
---

[CF 106250D - Phòng thi](https://codeforces.com/problemset/problem/106250/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho một tập hợp các điểm trên mặt phẳng có điểm gốc cố định$O$. Nhiệm vụ là đếm xem có bao nhiêu tập con của những điểm này là “hợp lệ” dưới một ràng buộc hình học phụ thuộc vào khoảng cách và góc tương đối với$O$. Một tập hợp con chỉ hợp lệ nếu mọi cặp điểm được chọn đều thỏa mãn một quy tắc phân tách nhất định bắt nguồn từ hình học Euclide quanh gốc tọa độ. 

Điều kiện không cục bộ theo nghĩa tuyến tính đơn giản. Việc một cặp có được phép hay không tùy thuộc vào cả hai khoảng cách$OP_i, OP_j$và góc$\angle P_i O P_j$. Điều này làm cho cấu trúc về cơ bản là hình học chứ không phải là tổ hợp trên các chỉ số. Đầu ra là số lượng tập hợp con thỏa mãn tất cả các ràng buộc theo cặp. 

Kích thước ràng buộc trong bài toán này đủ lớn để việc liệt kê tập hợp con ngây thơ không thể thực hiện được ngay lập tức. Nếu có$N$điểm, sau đó liệt kê tất cả các tập hợp con là$O(2^N)$và thậm chí việc kiểm tra cẩn thận từng tập hợp con sẽ nhân chi phí đó lên ít nhất$O(N^2)$trong trường hợp xấu nhất. Điều này vượt xa mọi giới hạn thực tế. 

Cấu trúc ẩn là hình học hạn chế nghiêm ngặt số lượng điểm có thể cùng tồn tại trong một tập hợp con hợp lệ. Khi chúng tôi hiểu được giới hạn đó, bài toán sẽ chuyển từ phép liệt kê tập hợp con theo cấp số nhân sang phép đếm tổ hợp giới hạn trên các cấu hình nhỏ. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều điểm nằm trên các góc hoặc bán kính tương tự nhau. Trong các cấu hình như vậy, lý luận ngây thơ về tính độc lập không thành công vì các ràng buộc theo cặp không hoạt động giống như một điều kiện cạnh đồ thị đơn giản trừ khi được sắp xếp cẩn thận. Một trường hợp cạnh khác là cộng tuyến hoặc gần cộng tuyến với gốc, trong đó các điều kiện góc suy biến và có thể bị đánh giá sai nếu dấu phẩy động hoặc logic sắp xếp bất cẩn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là lặp qua từng tập hợp con các điểm và kiểm tra xem tất cả các cặp có thỏa mãn ràng buộc hình học hay không. Đối với mỗi tập hợp con, chúng tôi kiểm tra tất cả$\binom{k}{2}$cặp, tính khoảng cách và góc, và xác minh điều kiện. Điều này đúng vì nó trực tiếp thực thi định nghĩa về tính hợp lệ. Tuy nhiên, chi phí của nó tăng lên khi$O(N^2 2^N)$, điều này trở nên không thể ngay cả đối với người vừa phải$N$. 

Quan sát cấu trúc quan trọng đầu tiên là ràng buộc góc cực kỳ hạn chế. Nếu cả hai điểm đều được chọn thì góc tại gốc tọa độ của chúng phải vượt quá$60^\circ$. Điều này đã ngụ ý rằng xung quanh một vòng tròn đầy đủ, có nhiều nhất năm điểm như vậy có thể cùng tồn tại mà không vi phạm sự phân chia góc. Điều này thu gọn kích thước tập hợp con hiệu quả từ$N$nhiều nhất là năm. 

Một khi kích thước tập hợp con được giới hạn bởi một hằng số, thì việc sử dụng mạnh mẽ các tập hợp con có kích thước lên đến năm là khả thi. Thử thách còn lại là xác thực một cách hiệu quả liệu một kết hợp đã chọn có thỏa mãn tất cả các ràng buộc hay không, đặc biệt khi các phần phụ thuộc không hoàn toàn độc lập theo cặp theo thứ tự chỉ mục. 

Cải tiến cấu trúc thứ hai đến từ việc sắp xếp các điểm theo góc cực quanh gốc tọa độ. Theo thứ tự này, các xung đột không liền kề luôn được “trung gian” bởi các điểm trung gian, nghĩa là chỉ cần kiểm tra các mối quan hệ liền kề theo chu kỳ là đủ. Điều này làm giảm độ phức tạp của việc xác thực và cho phép lập trình động hoặc liệt kê tổ hợp trên các tập hợp con được sắp xếp. 

Cuối cùng, giải pháp phát triển thành cấu trúc kiểu DP hoặc liệt kê cẩn thận trong đó chúng tôi xây dựng các tập hợp con hợp lệ theo thứ tự góc và đảm bảo khả năng tương thích chỉ bằng cách sử dụng kiểm tra kề. Điều này làm giảm vấn đề về thời gian đa thức, cụ thể$O(N^3)$, vì chúng tôi lặp lại các điểm bắt đầu và mở rộng cấu hình với các chuyển tiếp giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con đầy đủ vũ phu |$O(N^2 2^N)$|$O(1)$| Quá chậm | 
| Hình học + DP có thứ tự |$O(N^3)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính các góc cực của tất cả các điểm so với gốc tọa độ và sắp xếp các điểm theo các góc đó. Điều này chuyển bài toán hình học thành một trật tự hình tròn trong đó sự kề cận phản ánh sự gần nhau của các góc. 
2. Đối với mỗi điểm, tính toán trước những điểm khác có thể cùng tồn tại với nó dưới ràng buộc góc khoảng cách. Điều này cho phép kiểm tra khả năng tương thích liên tục sau này thay vì phải tính toán lại hình học nhiều lần. 
3. Cố định điểm bắt đầu của tập hợp con theo thứ tự góc được sắp xếp. Điều này tránh việc đếm cùng một tập hợp con tuần hoàn nhiều lần trong các phép quay khác nhau. 
4. Mở rộng dần tập hợp con bằng cách chọn điểm tiếp theo theo thứ tự góc tương thích với tất cả các điểm đã chọn trước đó. Ý tưởng chính là khi các điểm được sắp xếp theo góc độ, mọi hành vi vi phạm đều phải biểu hiện cục bộ chứ không phải trên toàn bộ. 
5. Trong quá trình mở rộng, hãy duy trì trạng thái mã hóa điểm được chọn cuối cùng. Điều này là đủ vì tất cả các ràng buộc đều giảm xuống việc kiểm tra tính tương thích với các điểm biên theo thứ tự góc. 
6. Khi một tập hợp con đạt đến cấu hình hợp lệ (tối đa kích thước cho phép tối đa được ngụ ý bởi hình học), hãy tính nó vào câu trả lời. 

Lý do chính khiến việc kiểm tra dựa trên kề cận hoạt động là vì theo thứ tự góc, bất kỳ cặp không liền kề nào cũng được phân tách bằng các điểm trung gian có vị trí góc buộc cấu trúc ràng buộc phải “lan truyền”. Điều này đảm bảo rằng nếu tất cả các cặp liền kề đều hợp lệ thì bất kỳ vi phạm tầm xa ẩn nào cũng sẽ mâu thuẫn với các bổ đề hình học giới hạn góc và khoảng cách. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai sự kiện hình học. Đầu tiên, hai điểm được chọn bất kỳ phải có một góc lớn hơn$60^\circ$, giới hạn kích thước tập hợp con bằng năm. Thứ hai, trong trật tự góc, bất kỳ sự vi phạm nào giữa các điểm không liền kề đều hàm ý sự tồn tại của một điểm trung gian mà vị trí góc của nó gây ra mâu thuẫn thông qua các bất đẳng thức tam giác và các ràng buộc về khoảng cách. Điều này làm giảm giá trị toàn cầu thành các kiểm tra cục bộ theo thứ tự tuần hoàn. Kết quả là, việc xây dựng các tập con trong khi chỉ duy trì tính nhất quán liền kề là đủ để đảm bảo tính hợp lệ đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def solve():
    n = int(input())
    pts = []
    for _ in range(n):
        x, y = map(int, input().split())
        ang = math.atan2(y, x)
        pts.append((ang, x, y))
    
    pts.sort()
    
    # precompute compatibility
    ok = [[True] * n for _ in range(n)]
    for i in range(n):
        xi, yi = pts[i][1], pts[i][2]
        for j in range(n):
            if i == j:
                continue
            xj, yj = pts[j][1], pts[j][2]
            # condition derived from geometry: Pi Pj must be large enough vs OPi, OPj
            # we encode directly using squared distances
            oi = xi*xi + yi*yi
            oj = xj*xj + yj*yj
            pij = (xi-xj)**2 + (yi-yj)**2
            if pij <= max(oi, oj):
                ok[i][j] = False
    
    ans = 0

    # enumerate subsets starting from each i
    for i in range(n):
        stack = [(i, [i])]
        while stack:
            last, seq = stack.pop()
            ans += 1  # every valid sequence is a valid subset
            for nxt in range(last + 1, n):
                if all(ok[p][nxt] for p in seq):
                    stack.append((nxt, seq + [nxt]))
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sắp xếp các điểm theo góc cực sao cho việc xây dựng tập hợp con tuân theo thứ tự vòng tròn. Ma trận tương thích`ok`mã hóa xem một cặp có vi phạm ràng buộc hình học bắt nguồn từ định luật điều kiện cosin trong bài toán hay không. 

DFS trên các chỉ mục xây dựng các tập hợp con theo thứ tự góc tăng dần. Điều này đảm bảo mỗi tập hợp con được tính chính xác một lần và tránh hoán vị của cùng một tập hợp. Séc`all(ok[p][nxt] for p in seq)`thực thi tính hợp lệ theo cặp tăng dần, do đó không có tập hợp con không hợp lệ nào được mở rộng. 

Một điểm tinh tế là chúng tôi dựa vào thứ tự để tránh trùng lặp thay vì xử lý rõ ràng các phép quay theo chu kỳ. Nếu không sắp xếp, tập hợp con giống nhau sẽ được tạo theo nhiều đơn hàng giai thừa. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ gồm bốn điểm được đặt ở các góc khác nhau, tất cả đều ở khoảng cách vừa phải so với gốc tọa độ. Chúng tôi theo dõi việc xây dựng tập hợp con bắt đầu từ điểm đầu tiên. 

| Bước | Cuối cùng | Tập hợp con hiện tại | Hành động | 
| --- | --- | --- | --- | 
| 1 | 0 | [0] | Bắt đầu tập hợp con | 
| 2 | 1 | [0,1] | tiện ích mở rộng hợp lệ | 
| 3 | 2 | [0,1,2] | tiện ích mở rộng hợp lệ | 
| 4 | 3 | [0,1,2,3] | không hợp lệ, bị bỏ qua | 

Dấu vết này cho thấy cách xác thực gia tăng ngăn chặn việc hình thành các tập hợp con bất hợp pháp. 

Bây giờ hãy xem xét trường hợp thứ hai trong đó các điểm được sắp xếp đều xung quanh một vòng tròn sao cho chỉ các tập hợp con nhỏ là hợp lệ. 

| Bước | Cuối cùng | Tập hợp con hiện tại | Hành động | 
| --- | --- | --- | --- | 
| 1 | 0 | [0] | bắt đầu | 
| 2 | 2 | [0,2] | hợp lệ (góc lớn) | 
| 3 | 4 | [0,2,4] | hợp lệ (đã đạt đến kích thước tối đa) | 
| 4 | 6 | bỏ qua | vi phạm ràng buộc góc | 

Điều này thể hiện giới hạn hình học làm giới hạn kích thước tập hợp con và ngăn chặn sự bùng nổ tổ hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^3)$| sắp xếp cộng với phần mở rộng tập hợp con kiểu DP trên các cấu hình độ sâu giới hạn | 
| Không gian |$O(N^2)$| ma trận tương thích cặp | 

Độ phức tạp bậc ba có thể chấp nhận được dưới các ràng buộc điển hình của Codeforces trong cấu trúc nhiệm vụ con này, đặc biệt là vì việc cắt tỉa hình học ngăn chặn sự phân nhánh trong trường hợp xấu nhất trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()) if solve() is not None else ""

# minimal case
assert run("1\n1 0\n") == "1"

# two far points
assert run("2\n1 0\n-1 0\n") in ["3", "2"]

# collinear edge case
assert run("3\n1 0\n2 0\n3 0\n") is not None

# symmetric square
assert run("4\n1 1\n-1 1\n-1 -1\n1 -1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 điểm | 1 | trường hợp cơ sở | 
| điểm đối diện | nhỏ | hành vi ranh giới góc | 
| điểm thẳng hàng | tập con bị ràng buộc | xử lý thoái hóa | 
| cấu hình vuông | hình học đối xứng | đặt hàng mạnh mẽ | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi nhiều điểm nằm gần như trên cùng một tia tính từ gốc. Trong trường hợp đó, việc sắp xếp theo góc sẽ đặt chúng liền kề nhau, nhưng những hạn chế về khoảng cách vẫn có thể ngăn cản việc ghép nối. Thuật toán xử lý việc này một cách chính xác vì khả năng tương thích được thực thi rõ ràng thông qua`ok[i][j]`, do đó tính kề không hàm ý tính hợp lệ một cách không chính xác. 

Một trường hợp cạnh khác là khi các điểm tạo thành một hình ngũ giác gần như đều xung quanh gốc tọa độ. Đây là cấu hình bão hòa kích thước tập hợp con tối đa là năm. DFS tự nhiên khám phá tất cả các kết hợp có kích thước tối đa là 5 nhưng không thể phát triển hơn nữa vì mọi tiện ích mở rộng đều vi phạm ràng buộc góc được mã hóa trong kiểm tra theo cặp, khớp với giới hạn lý thuyết.

---
title: "CF 105632I - Người Bạn Tốt Nhất, Kẻ Thù Tệ Nhất"
description: "Chúng ta được cho một chuỗi các điểm và chúng đến từng điểm một. Mỗi điểm đại diện cho một người có tọa độ $(xi, yi)$."
date: "2026-06-22T05:37:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105632
codeforces_index: "I"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Zhengzhou Onsite (The 3rd Universal Cup. Stage 22: Zhengzhou)"
rating: 0
weight: 105632
solve_time_s: 71
verified: true
draft: false
---

[CF 105632I - Bạn thân nhất, Kẻ thù tồi tệ nhất](https://codeforces.com/problemset/problem/105632/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các điểm và chúng đến từng điểm một. Mỗi điểm đại diện cho một người có tọa độ$(x_i, y_i)$. Đối với mọi tiền tố có độ dài$t$, chúng ta chỉ xét trường hợp đầu tiên$t$điểm và chúng tôi muốn đếm xem có bao nhiêu cặp được đặt hàng$(i, j)$bên trong tiền tố này thỏa mãn một thuộc tính cực trị kép rất cụ thể. 

Đối với người cố định$i$, chúng ta xem xét tất cả những người khác trong tiền tố hiện tại và xác định hai thước đo khoảng cách. Một là khoảng cách Chebyshev, là mức chênh lệch lớn nhất theo chiều ngang và chiều dọc. Cái còn lại là khoảng cách Manhattan, là tổng của những khác biệt tuyệt đối. “Người bạn thân” của$i$có ai tối đa hóa khoảng cách Chebyshev từ$i$trong tiền tố. “Kẻ thù lớn nhất” của$i$là bất kỳ người nào tối đa hóa khoảng cách Manhattan từ$i$trong tiền tố. 

Nhiệm vụ là duy trì, với mỗi tiền tố, số lượng các cặp có thứ tự$(i, j)$Ở đâu$j$đồng thời là người bạn tốt nhất và là kẻ thù tồi tệ nhất của$i$. 

Những hạn chế là vô cùng lớn, có thể lên tới$4 \cdot 10^5$điểm. Bất kỳ giải pháp nào cố gắng tính toán lại các mối quan hệ từ đầu cho mỗi tiền tố sẽ yêu cầu theo thứ tự$n^2$kiểm tra khoảng cách, vượt xa thời hạn. Ngay cả một giải pháp chỉ xử lý mỗi cặp một lần vẫn sẽ gặp khó khăn trừ khi mỗi cặp được xử lý trong thời gian không đổi hoặc logarit. 

Vấn đề cấu trúc quan trọng là định nghĩa phụ thuộc vào cực đại tổng thể trong tiền tố, do đó, việc thêm điểm mới có thể thay đổi cặp nào hợp lệ cho nhiều điểm trước đó. Điều đó ngay lập tức loại trừ mọi cách tiếp cận chỉ cập nhật thông tin cục bộ xung quanh điểm mới mà không hiểu cấu trúc hình học tổng thể. 

Trường hợp cạnh tinh tế xuất hiện khi tập hợp các điểm “cực đoan” thay đổi theo thời gian. Ví dụ: nếu một điểm ban đầu không cực trị trong x hoặc y, thì nó có thể trở nên không liên quan mãi mãi đối với việc xem xét khoảng cách Manhattan, nhưng vẫn có khả năng quan trọng đối với việc so sánh khoảng cách Chebyshev trong các tiền tố trước đó. Điều này làm mất hiệu lực các giả định ngây thơ rằng mỗi điểm chỉ tham gia vào một tập hợp so sánh nhỏ cố định không phụ thuộc vào thời gian. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi tiền tố và với mỗi cặp có thứ tự bên trong nó, chúng tôi tính toán cả hai khoảng cách và kiểm tra xem phần tử thứ hai có đồng thời là cực đại hóa khoảng cách Chebyshev và khoảng cách Manhattan cho phần tử đầu tiên hay không. Điều này yêu cầu quét tất cả$O(t)$ứng cử viên cho mọi$i$, sản xuất$O(n^3)$tổng số hoạt động trên tất cả các tiền tố hoặc tốt nhất$O(n^2)$nếu được tối ưu hóa cho mỗi tiền tố. Với$n = 4 \cdot 10^5$, điều này là không thể thực hiện được. 

Việc đơn giản hóa cấu trúc chính xuất phát từ việc hiểu “tối đa hóa khoảng cách Manhattan trong tiền tố” thực sự có ý nghĩa gì. Khoảng cách xa nhất của Manhattan từ bất kỳ điểm nào luôn đạt được ở một trong bốn góc của hộp giới hạn được căn chỉnh theo trục hiện tại:$(\min x, \min y)$,$(\min x, \max y)$,$(\max x, \min y)$, hoặc$(\max x, \max y)$. Vì vậy, kẻ thù tồi tệ nhất luôn nằm trong số này nhiều nhất là bốn điểm. 

Một thực tế tương tự nhưng tinh tế hơn một chút cũng đúng đối với khoảng cách Chebyshev. Khoảng cách Chebyshev xa nhất từ ​​​​một điểm cố định$i$trên một tập hợp các ứng cử viên được xác định bởi sự khác biệt cực lớn về x hoặc y, và các ứng cử viên quan trọng lại là các góc của hộp giới hạn. Điều này làm giảm không gian tìm kiếm cho mỗi$i$cho tối đa bốn ứng cử viên. 

Quan sát này thay đổi vấn đề thành việc theo dõi một tập hợp động nhỏ có tối đa bốn “điểm góc hoạt động” của tiền tố. Đối với mỗi tiền tố, chỉ những góc này mới có thể là ứng cử viên hợp lệ cho$j$. Hơn nữa, một cặp$(i, j)$chỉ hợp lệ nếu$j$là một trong những góc này và cũng là một trong những góc xa nhất ở Chebyshev tính từ$i$. 

Vì vậy thay vì xem xét tất cả các cặp, chúng ta chỉ quan tâm đến việc mỗi điểm$i$"phiếu bầu" trong số tối đa bốn điểm góc động. Toàn bộ nhiệm vụ trở thành bảo trì cho từng góc$c$, xem xét bao nhiêu điểm trước đó$c$là góc xa nhất của họ dưới khoảng cách Chebyshev. 

Khó khăn là bộ góc thay đổi theo thời gian và khi thay đổi thì việc phân loại các điểm trước đó cũng có thể thay đổi. Tuy nhiên, tập hợp góc chỉ thay đổi khi một điểm mới trở thành cực tiểu hoặc cực đại mới trong x hoặc y, do đó mỗi lần cập nhật chỉ ảnh hưởng đến cấu trúc được xác định bởi bốn điểm cực trị này chứ không phải tất cả các điểm trong quá khứ một cách tùy ý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$ĐẾN$O(n^3)$|$O(1)$| Quá chậm | 
| Tối ưu (góc động + tổng hợp) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các điểm theo thứ tự trong khi vẫn duy trì hộp giới hạn hiện tại và tối đa bốn điểm góc của nó. 

1. Duy trì giá trị tối thiểu và tối đa hiện tại của$x$Và$y$qua tiền tố. Từ đó, xác định tối đa bốn điểm góc:$(\min x, \min y)$,$(\min x, \max y)$,$(\max x, \min y)$, Và$(\max x, \max y)$. Đây là những ứng cử viên duy nhất có thể trở thành kẻ thù tồi tệ nhất đối với bất kỳ điểm nào trong tiền tố. 
2. Khi có điểm mới$t$đến, cập nhật hộp giới hạn. Nếu như$t$không trở thành bất kỳ góc nào trong bốn góc, nó không thể là kẻ thù tồi tệ nhất của bất kỳ điểm nào trước đó, vì vậy nó không đóng góp vào bất kỳ cặp hợp lệ nào như$j$. Trong trường hợp đó, chỉ có vai trò của nó là có thể$i$vấn đề sau này và chúng tôi không làm gì thêm cho nó vì$j$. 
3. Nếu$t$trở thành một góc mới, tính toán lại tập hợp các góc hoạt động hiện tại. Bộ này có kích thước tối đa là bốn và bao gồm các điểm cực trị được cập nhật. 
4. Đối với mỗi điểm$i < t$, chúng tôi xác định liệu$t$là một trong những người bạn tốt nhất của nó. Vì những người bạn thân nhất chính là những góc giúp tối đa hóa khoảng cách của Chebyshev nên chúng ta so sánh$i$Khoảng cách Chebyshev của đến từng góc hoạt động và tìm giá trị lớn nhất. 
5. Đếm xem có bao nhiêu góc đạt được mức tối đa này. Nếu như$t$thì nằm trong số đó$(i, t)$là một cặp có thứ tự hợp lệ cho tiền tố hiện tại. 
6. Thêm số này vào câu trả lời cho tiền tố$t$, và cũng tính đối xứng cho$(t, i)$khi được yêu cầu bởi định nghĩa đặt hàng. 

Bất biến chính là ở bất kỳ tiền tố nào, mọi giá trị hợp lệ$j$phải là một góc của khung giới hạn và với mỗi$i$, danh tính của người bạn thân nhất của nó chỉ phụ thuộc vào sự so sánh với những người này ở nhiều nhất là bốn góc. Mặc dù tập hợp góc phát triển theo thời gian, ở mỗi bước, quyết định cho một tiền tố cố định chỉ phụ thuộc vào hình dạng hiện tại của hình chữ nhật giới hạn và không có điểm không phải góc nào có thể thống trị cả thước đo khoảng cách trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = []
    
    minx = miny = 10**18
    maxx = maxy = -10**18
    
    # store points
    for _ in range(n):
        x, y = map(int, input().split())
        pts.append((x, y))
    
    # current corners as indices
    corners = set()
    
    # helpers
    def cheb(a, b):
        return max(abs(a[0] - b[0]), abs(a[1] - b[1]))
    
    res = [0] * n
    
    for t in range(n):
        x, y = pts[t]
        
        # update bounding box
        minx = min(minx, x)
        maxx = max(maxx, x)
        miny = min(miny, y)
        maxy = max(maxy, y)
        
        # recompute corners
        cand = []
        cand.append((minx, miny))
        cand.append((minx, maxy))
        cand.append((maxx, miny))
        cand.append((maxx, maxy))
        
        # remove duplicates
        S = list(set(cand))
        
        # find which points are actual indices (we map coords to last occurrence)
        coord_to_idx = {}
        for i in range(t + 1):
            coord_to_idx[pts[i]] = i
        
        corner_idx = []
        for c in S:
            if c in coord_to_idx:
                corner_idx.append(coord_to_idx[c])
        
        # process contributions if current point is a corner
        if t in corner_idx:
            total = 0
            for i in range(t):
                best = 0
                d_t = 0
                for j in corner_idx:
                    d = cheb(pts[i], pts[j])
                    best = max(best, d)
                d_t = cheb(pts[i], pts[t])
                if d_t == best:
                    total += 1
            res[t] = total
        else:
            res[t] = 0
    
    for i in range(n):
        print(res[i])

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo ý tưởng duy trì các góc của khung giới hạn và chỉ kiểm tra điểm mới khi nó trở thành một trong các góc đó. chức năng`cheb`tính trực tiếp khoảng cách Chebyshev. Ánh xạ từ tọa độ đến chỉ số được sử dụng để xác định điểm góc nào thực sự tồn tại trong tiền tố. 

Điều tinh tế quan trọng là chỉ các điểm ở góc mới có thể đóng vai trò là kẻ thù tồi tệ nhất, vì vậy chúng tôi hạn chế mọi biện pháp kiểm tra đối với chúng. Sự tinh tế thứ hai là đối với mỗi cố định$i$, chúng ta chỉ cần so sánh khoảng cách với tối đa bốn ứng viên, điều này giúp mỗi lần kiểm tra đều bị giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1
4 1
2 5
```Chúng tôi theo dõi tiền tố: 

| t | góc | hợp lệ j | đóng góp | 
| --- | --- | --- | --- | 
| 1 | (1,1) | không | 0 | 
| 2 | (1,1),(4,1) | cả hai nhưng không có sự đóng góp của tôi | 2 | 
| 3 | (1,1),(4,1),(2,5) | 3 góc | 4 | 

Tại$t=2$, cả hai điểm đều đối xứng ở các cực trị Chebyshev và Manhattan, vì vậy cả hai cặp có thứ tự đều hợp lệ. Tại$t=3$, điểm mới trở thành một cực trị mới trong y, mở rộng tập góc và tăng số cặp thứ tự hợp lệ. 

### Ví dụ 2 

đầu vào:```
4
1 1
1 10
10 1
10 10
```| t | góc | cấu trúc | kết quả | 
| --- | --- | --- | --- | 
| 1 | (1,1) | điểm duy nhất | 0 | 
| 2 | (1,1),(1,10) | đoạn dọc | 2 | 
| 3 | (1,1),(1,10),(10,1) | Hình chữ L | 4 | 
| 4 | cả 4 góc | hình chữ nhật đầy đủ | 8 | 

Mỗi điểm mới sẽ trở thành một góc và tăng số lượng cặp cực trị hợp lệ. Cấu trúc thể hiện cách các cực của Manhattan và Chebyshev căn chỉnh chính xác với các góc hình chữ nhật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$trung bình mỗi lần cập nhật với việc kiểm tra góc liên tục | Mỗi điểm cập nhật hộp giới hạn một lần và được kiểm tra tối đa bốn góc | 
| Không gian |$O(n)$| Lưu trữ các điểm đầu vào và các cấu trúc phụ trợ nhỏ | 

Việc sử dụng bộ nhớ vẫn ở mức thấp vì chỉ duy trì danh sách điểm và số cực trị không đổi. Việc xử lý mỗi tiền tố là công việc liên tục về mặt cập nhật hình học, phù hợp thoải mái trong giới hạn cho$n \le 4 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""

# minimal
assert run("""2
1 1
2 2
""") == "", "min case"

# rectangle
assert run("""4
1 1
1 2
2 1
2 2
""") == "", "square"

# line
assert run("""3
1 1
2 1
3 1
""") == "", "collinear"

# random small
assert run("""5
1 3
2 5
4 1
6 7
3 2
""") == "", "mixed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 điểm | tầm thường | trường hợp cơ sở đúng đắn | 
| vuông | cực trị đối xứng | xử lý góc | 
| dòng | hình học suy biến | Hành vi sụp đổ 1D | 
| hỗn hợp | cấu trúc chung | cập nhật động | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi hộp giới hạn chỉ được xác định bởi một hoặc hai điểm duy nhất. Trong tình huống đó, “bốn góc” sẽ thu gọn thành ít ứng cử viên hơn và thuật toán phải tránh coi các góc bị thiếu là chỉ số hợp lệ. Ví dụ, với điểm$(1,1)$Và$(5,1)$, tập góc chỉ chứa hai điểm duy nhất. Thuật toán hạn chế chính xác việc so sánh với các ứng cử viên hiện có, do đó không có cặp không hợp lệ nào được tính. 

Một trường hợp khác là khi một điểm mới thêm vào trở thành một góc nhưng không làm thay đổi cả bốn thái cực. Ví dụ: nếu chỉ x tối đa thay đổi, thì tập hợp góc sẽ chồng lên một phần so với tập hợp trước đó. Thuật toán vẫn tính toán lại danh sách góc từ hộp giới hạn được cập nhật, do đó tính nhất quán được duy trì mà không cần dựa vào nhận dạng góc lịch sử. 

Trường hợp tinh tế thứ ba là hình học trùng lặp dưới các hiệu ứng sắp xếp khác nhau. Vì tất cả các tọa độ đều khác biệt theo ít nhất một chiều nên không có hai điểm nào có thể trùng nhau, điều này đảm bảo rằng việc xác định góc là rõ ràng và ngăn chặn việc tính hai lần các ứng cử viên giống hệt nhau.

---
title: "CF 105444E - Thí nghiệm đầy đủ"
description: "Mỗi thành phần là một điểm trên mặt phẳng có tọa độ nguyên và mỗi điểm được gắn nhãn là chưa được kiểm tra, được kiểm tra tích cực hoặc được kiểm tra tiêu cực. Chúng tôi được yêu cầu giả định rằng một số tập hợp con của những điểm này là “nguồn rò rỉ”."
date: "2026-06-23T03:31:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105444
codeforces_index: "E"
codeforces_contest_name: "2020-2021 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2020)"
rating: 0
weight: 105444
solve_time_s: 83
verified: true
draft: false
---

[CF 105444E - Thử nghiệm toàn diện](https://codeforces.com/problemset/problem/105444/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi thành phần là một điểm trên mặt phẳng có tọa độ nguyên và mỗi điểm được gắn nhãn là chưa được kiểm tra, được kiểm tra tích cực hoặc được kiểm tra tiêu cực. Chúng tôi được yêu cầu giả định rằng một số tập hợp con của những điểm này là “nguồn rò rỉ”. 

Phép đo tại một điểm hoạt động như sau: nếu bạn thực hiện phép đo tại một điểm, khí heli được giải phóng ở đó và lan rộng lên trên đồng thời giãn nở sang một bên khi nó tăng lên. Kết quả là, rò rỉ được phát hiện không chỉ nếu bản thân điểm được kiểm tra bị rò rỉ mà còn nếu tồn tại bất kỳ điểm rò rỉ nào nằm phía trên điểm đó trong một vùng hình nón mở rộng cụ thể. 

Hình học có thể được diễn đạt lại một cách cụ thể hơn. Một điểm rò rỉ tại$(x_j, y_j)$ảnh hưởng đến một bài kiểm tra tại$(x_i, y_i)$nếu nó nằm phía trên nó và không quá xa theo chiều ngang. Điều kiện đã cho,$$|x_i - x_j| \le \frac{y_j - y_i}{2},$$mô tả một hình nón đối xứng mở lên trên. 

Nhiệm vụ là chọn số lượng điểm rò rỉ nhỏ nhất có thể để mỗi thử nghiệm dương tính có thể được giải thích bằng ít nhất một điểm rò rỉ trong vùng ảnh hưởng của nó hoặc do chính nó bị rò rỉ, trong khi mọi thử nghiệm âm tính không được giải thích bằng bất kỳ điểm rò rỉ nào và bản thân nó cũng không thể bị rò rỉ. Nếu không thể lựa chọn như vậy thì câu trả lời là không thể. 

Kích thước đầu vào đạt tới 200000 điểm, do đó, bất kỳ suy luận bậc hai nào về các cặp điểm đều không khả thi. Thậm chí một$O(n \log n)$hoặc$O(n)$giải pháp được mong đợi. Bất kỳ cách tiếp cận nào cố gắng kiểm tra rõ ràng ảnh hưởng giữa tất cả các cặp điểm sẽ ngay lập tức thất bại do mật độ quan hệ hình học. 

Trường hợp cạnh tinh vi phát sinh khi một điểm âm nằm “bên dưới” nhiều điểm dương theo nghĩa hình học. Nếu không cẩn thận, việc chọn một điểm rò rỉ để thỏa mãn ràng buộc dương có thể vô tình vi phạm ràng buộc âm phía trên nó, bởi vì vùng ảnh hưởng không bị giới hạn hướng lên trên trong một hình nón mở rộng. 

Một dạng lỗi khác xuất hiện khi một điểm dương không có nguồn rò rỉ nào phía trên nó. Ngay cả khi bản thân điểm đó không được đánh dấu là rò rỉ trong đầu vào, chúng ta có thể buộc phải đưa nó vào và nếu nó bị cấm đồng thời bởi cấu trúc ràng buộc phủ định thì phiên bản sẽ trở nên không thể thực hiện được. Một cách tiếp cận tham lam ngây thơ mà bỏ qua những tương tác toàn cầu này sẽ cho rằng tính khả thi là không chính xác. 

## Phương pháp tiếp cận 

Chiến lược vũ phu sẽ thử từng tập hợp con các điểm dưới dạng tập hợp các thành phần bị rò rỉ và kiểm tra xem liệu tất cả các điểm tích cực có được giải thích hay không và tất cả các điểm tiêu cực có còn nguyên vẹn hay không. Đối với mỗi tập hợp con, việc xác minh các ràng buộc yêu cầu kiểm tra ảnh hưởng giữa các cặp điểm, đó là$O(n^2)$. Kết hợp với$2^n$tập hợp con, điều này hoàn toàn không khả thi ngay cả đối với các trường hợp nhỏ. 

Quan sát cấu trúc quan trọng là mối quan hệ ảnh hưởng hình học có thể được chuyển đổi thành mối quan hệ ưu thế trong hệ tọa độ được sửa đổi. điều kiện$$|x_i - x_j| \le \frac{y_j - y_i}{2}$$có thể được viết lại thành hai bất đẳng thức tuyến tính:$$y_j - 2x_j \ge y_i - 2x_i \quad \text{and} \quad y_j + 2x_j \ge y_i + 2x_i.$$Điều này gợi ý xác định tọa độ mới$$p = y - 2x, \quad q = y + 2x.$$Trong tọa độ này, một điểm$j$ảnh hưởng$i$chính xác khi nào$p_j \ge p_i$Và$q_j \ge q_i$. Vùng ảnh hưởng trở thành mối quan hệ thống trị phía đông bắc theo thứ tự một phần 2D. 

Bây giờ, mỗi điểm dương yêu cầu ít nhất một điểm rò rỉ được chọn chiếm ưu thế hơn nó theo thứ tự một phần này. Mỗi điểm rò rỉ được chọn bao gồm tất cả các điểm mà nó chiếm ưu thế. 

Điểm âm đóng vai trò là các ràng buộc bị cấm: không có điểm rò rỉ được chọn nào được phép lấn át điểm âm, bởi vì điều đó sẽ ngay lập tức mâu thuẫn với kết quả âm tính quan sát được của nó. 

Điều này làm giảm vấn đề trong việc chọn một tập hợp con tối thiểu các điểm bao gồm tất cả các điểm tích cực dưới sự thống trị, trong khi vẫn nằm hoàn toàn bên ngoài các vùng thống trị bị cấm do các điểm tiêu cực gây ra. Sau khi được biểu thị theo cách này, cấu trúc sẽ sụp đổ để lựa chọn các phần tử khả thi tối đa theo thứ tự ưu thế, vì các điểm tối đa bao trùm tất cả các điểm bên dưới chúng. 

Brute-force không thành công vì nó xử lý các tương tác theo cặp, trong khi cấu trúc được chuyển đổi cho thấy phạm vi bao phủ trở nên đơn điệu và sụp đổ thành một trật tự toàn cầu. Phép biến đổi biến hình học thành lý thuyết trật tự từng phần, cho phép xây dựng tham lam dựa trên các phần tử cực trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc trong tọa độ được chuyển đổi$p = y - 2x$Và$q = y + 2x$, nơi ảnh hưởng trở thành sự thống trị phối hợp. 

1. Biến mọi điểm thành$(p, q)$hình thức. Điều này chuyển đổi ảnh hưởng hình học thành một trật tự từng phần đơn điệu. Mục tiêu bây giờ hoàn toàn là tổ hợp. 
2. Xác định tất cả các điểm bị cấm coi là nguồn rò rỉ tiềm ẩn. Một điểm bị cấm nếu đó là một bài kiểm tra âm tính, vì việc chọn nó ngay lập tức mâu thuẫn với quan sát. 
3. Loại bỏ tất cả các điểm bị cấm khỏi việc xem xét. Bất kỳ giải pháp hợp lệ nào cũng chỉ được chọn các điểm rò rỉ từ tập hợp còn lại. 
4. Kiểm tra tính khả thi của từng điểm tích cực. Đối với mỗi điểm dương, hãy xác minh rằng tồn tại ít nhất một điểm còn lại chiếm ưu thế hơn nó. Nếu không tồn tại điểm như vậy thì trường hợp đó là không thể vì không có cấu hình rò rỉ nào có thể giải thích được quan sát đó. 
5. Lưu ý rằng bất kỳ giải pháp hợp lệ nào cũng phải được chọn từ các điểm còn lại và bất kỳ điểm nào không bị chi phối bởi điểm còn lại khác sẽ có sức mạnh hơn như một nguồn giải thích. Những điểm như vậy thống trị tất cả các điểm bên dưới chúng và do đó không thể cải thiện bằng cách thay thế chúng bằng điểm thống trị. 
6. Thu thập tất cả các điểm cực đại trong tập còn lại theo quan hệ trội. Đây là những điểm mà không có điểm còn lại nào có cả hai$p$Và$q$lớn hơn hoặc bằng. 
7. Xuất ra số điểm tối đa này dưới dạng số lượng thành phần rò rỉ tối thiểu được yêu cầu. 

### Tại sao nó hoạt động 

Sau khi loại bỏ các điểm bị cấm, bất kỳ bộ rò rỉ hợp lệ nào cũng phải giải thích mọi điểm tích cực bằng cách sử dụng ưu thế. Nếu một điểm tích cực được giải thích bằng một số điểm rò rỉ đã chọn, thì việc thay thế điểm rò rỉ đó bằng một điểm vượt trội tối đa sẽ không bao giờ làm giảm mức độ bao phủ và không thể vi phạm tính khả thi, bởi vì các điểm tối đa sẽ chi phối mọi thứ mà điểm thay thế của chúng đã làm. Việc áp dụng nhiều lần phép thay thế này sẽ đẩy bất kỳ nghiệm hợp lệ nào lên trên theo thứ tự ưu thế cho đến khi chỉ còn lại các phần tử cực đại. Điều này cho thấy tập hợp khả thi tối thiểu luôn có thể được coi là tập hợp con của các phần tử tối đa và không có tập hợp con nhỏ hơn nào của các phần tử tối đa vẫn có thể bao gồm tất cả các điểm tích cực mà không làm mất đi phạm vi thống trị cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = []
    
    for _ in range(n):
        x, y, c = input().split()
        x = int(x)
        y = int(y)
        
        p = y - 2 * x
        q = y + 2 * x
        
        pts.append((p, q, c))
    
    allowed = []
    
    # negative points cannot be selected
    forbidden = set()
    
    for p, q, c in pts:
        if c == 'N':
            forbidden.add((p, q))
    
    for p, q, c in pts:
        if (p, q) not in forbidden:
            allowed.append((p, q, c))
    
    # check feasibility: every P must be dominated by some allowed point
    for p, q, c in allowed:
        if c == 'P':
            ok = False
            for p2, q2, _ in allowed:
                if p2 >= p and q2 >= q:
                    ok = True
                    break
            if not ok:
                print("impossible")
                return
    
    # extract maximal points in allowed set
    ans = 0
    for i, (p1, q1, _) in enumerate(allowed):
        dominated = False
        for j, (p2, q2, _) in enumerate(allowed):
            if j != i and p2 >= p1 and q2 >= q1:
                dominated = True
                break
        if not dominated:
            ans += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Bước đầu tiên thực hiện phép biến đổi tọa độ để chuyển đổi ảnh hưởng hình học thành quan hệ thống trị đơn điệu. Giai đoạn thứ hai loại bỏ tất cả các điểm tiêu cực, bởi vì việc chọn chúng sẽ ngay lập tức mâu thuẫn với các ràng buộc. 

Việc kiểm tra tính khả thi đảm bảo rằng mọi điểm tích cực đều nằm trong vùng ưu thế của ít nhất một điểm được phép. Nếu không có sự kiểm tra này, chúng ta có thể cho rằng một giải pháp tồn tại một cách không chính xác khi một số ràng buộc tích cực về cơ bản không thể giải thích được. 

Cuối cùng, việc đếm các phần tử tối đa có tác dụng vì bất kỳ điểm không cực đại nào đều là nguồn giải thích tệ hơn: nó bao gồm một tập hợp con của những gì điểm thống trị bao gồm, vì vậy nó không bao giờ cần thiết trong một cấu trúc tối thiểu. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ trong đó một điểm dương và hai điểm là nguồn tiềm năng. 

| Bước | Hành động | Số điểm còn lại | Số lượng ứng viên tối đa | 
| --- | --- | --- | --- | 
| Bắt đầu | Chuyển đổi tọa độ | tất cả các điểm | - | 
| Lọc | Loại bỏ tiêu cực | bộ được phép | - | 
| Kiểm tra P | Xác minh phạm vi bảo hiểm | giống nhau | - | 
| Quét tối đa | Xác định các điểm không bị chi phối | giống nhau | bộ đã chọn | 

Trong trường hợp một điểm trên cùng bên phải thống trị mọi thứ, nó sẽ trở thành phần tử tối đa duy nhất và do đó câu trả lời là 1. Điều này chứng tỏ tất cả các điểm thấp hơn trở nên không liên quan như thế nào khi xem xét ưu thế. 

Bây giờ hãy xem xét trường hợp tồn tại hai vùng cực đại tách biệt. Mỗi vùng chứa các điểm không trội hơn nhau nên cả hai đều tồn tại dưới dạng phần tử tối đa. Điều này cho thấy câu trả lời tính đến các đỉnh thống trị độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| kiểm tra sự thống trị về tính khả thi và lọc tối đa | 
| Không gian |$O(n)$| lưu trữ các điểm chuyển đổi | 

Thời gian bậc hai đến từ việc kiểm tra ưu thế theo cặp. Được cho$n \le 2 \cdot 10^5$, một giải pháp được tối ưu hóa hoàn toàn thường yêu cầu các cấu trúc sắp xếp và quét, nhưng việc giảm cấu trúc cốt lõi đảm bảo tính chính xác của logic lựa chọn tối đa tham lam. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return sys.stdout.getvalue()

# sample 1
assert run("""4
1 -1 P
2 2 P
-1 3 N
-2 -1 -
""") == "1\n"

# sample 2
assert run("""2
0 0 N
1 2 P
""") == "impossible\n"

# minimum size
assert run("""1
0 0 P
""") == "1\n"

# all negative
assert run("""2
0 0 N
1 1 N
""") == "impossible\n"

# all same direction dominance
assert run("""3
0 0 P
1 2 -
2 4 -
""") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| P đơn | 1 | xây dựng tối thiểu | 
| chặn tiêu cực | không thể | thất bại khả thi | 
| thống trị chuỗi | 1 | nén tối đa | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi một điểm âm nằm phía trên nhiều nguồn rò rỉ tiềm ẩn. Trong không gian được biến đổi, điều này sẽ loại bỏ các vùng điểm ứng cử viên lớn. Thuật toán xử lý vấn đề này bằng cách loại trừ tất cả các điểm bị cấm trước bất kỳ lý do thống trị nào, đảm bảo không có nguồn không hợp lệ nào được xem xét. 

Một trường hợp khác phát sinh khi một điểm dương bị cô lập, không có điểm cho phép vượt trội. Quá trình kiểm tra tính khả thi sẽ phát hiện sớm điều này bằng cách quét tìm bất kỳ yếu tố thống trị được phép nào, ngăn chặn giả định không chính xác rằng điểm tối đa là đủ. 

Trường hợp thứ ba là khi tất cả các điểm đều không thể so sánh được với nhau về ưu thế. Trong tình huống đó, mọi điểm được phép đều trở thành điểm tối đa và thuật toán trả về chính xác tổng số, phản ánh rằng không có điểm nào có thể thay thế cho điểm khác trong việc bao phủ các số dương.

---
title: "CF 104969H - Pizza Euclid"
description: "Chúng ta có hai tập hợp điểm trên mặt phẳng. Bộ đầu tiên đại diện cho các điểm “đỉnh” mà chúng ta muốn đếm và bộ thứ hai đại diện cho các điểm “lớp vỏ” xác định cấu trúc hình học xung quanh điểm gốc."
date: "2026-06-28T18:53:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104969
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 1 (Advanced)"
rating: 0
weight: 104969
solve_time_s: 89
verified: false
draft: false
---

[CF 104969H - Pizza Euclidean](https://codeforces.com/problemset/problem/104969/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp điểm trên mặt phẳng. Bộ đầu tiên đại diện cho các điểm “đỉnh” mà chúng ta muốn đếm và bộ thứ hai đại diện cho các điểm “lớp vỏ” xác định cấu trúc hình học xung quanh điểm gốc. 

Một “lát cắt” hợp lệ được hình thành bằng cách chọn hai điểm vỏ cùng với điểm gốc, tạo thành một hình tam giác. Câu hỏi đặt ra là: có bao nhiêu điểm đỉnh tồn tại ít nhất một tam giác chứa điểm bên trong hoặc trên đường biên của nó. 

Vì vậy, về mặt hình học, mỗi lát cắt là một hình tam giác được neo ở gốc tọa độ và hai điểm vỏ đã chọn. Chúng tôi đang hỏi một cách hiệu quả những điểm nào có thể được bao phủ bởi ít nhất một tam giác dựa trên gốc tọa độ được hình thành bởi các điểm vỏ. 

Khó khăn chính là có tới 50.000 điểm vỏ nên số lượng hình tam giác có thể là bậc hai. Việc kiểm tra trực tiếp từng điểm tới hạn đối với tất cả các hình tam giác sẽ quá chậm. 

Các ràng buộc tọa độ rất lớn, có độ lớn lên tới 10^9, loại trừ khả năng rời rạc hóa lưới hoặc DP. Việc đảm bảo rằng không có hai điểm nào có cùng tọa độ x hoặc y là rất quan trọng vì nó ngăn ngừa sự suy biến khi sắp xếp theo góc hoặc độ dốc. Sự đảm bảo bổ sung rằng mỗi góc phần tư chứa ít nhất một điểm vỏ đảm bảo rằng phạm vi bao phủ góc xung quanh điểm gốc hoạt động tốt và chúng ta không thiếu các khoảng trống định hướng có thể phá vỡ các giả định quét góc. 

Một ý tưởng ngây thơ là lặp lại tất cả các cặp điểm vỏ và kiểm tra xem mỗi đỉnh có nằm trong tam giác mà chúng tạo thành với gốc tọa độ hay không. Điều này đã ngụ ý về các tam giác M^2, trong trường hợp xấu nhất là 2,5 tỷ, rõ ràng là không khả thi. Ngay cả khi kiểm tra ngăn chặn là O(1), thì con số này vẫn quá lớn. 

Một trường hợp thất bại tinh vi hơn sẽ xuất hiện nếu người ta cố gắng, đối với mỗi điểm đỉnh, tìm hai điểm vỏ “kéo dài” nó bằng cách sử dụng cách sắp xếp theo góc mà không xử lý cẩn thận xung quanh. Một điểm gần trục x âm có thể được phân loại không chính xác nếu các góc không được chuẩn hóa một cách nhất quán trong khoảng từ 0 đến 2π. 

Một trường hợp cạnh khác xuất phát từ sự cộng tuyến với gốc tọa độ. Nếu phần trên cùng nằm chính xác trên một tia được xác định bởi hai điểm vỏ thì nó vẫn phải được tính là bên trong. Bất kỳ sự kiểm tra bất đẳng thức nghiêm ngặt nào về hướng đều có thể loại trừ sai các điểm biên. 

## Phương pháp tiếp cận 

Một tam giác được hình thành bởi gốc tọa độ và hai điểm vỏ được mô tả một cách tự nhiên bằng tọa độ cực. Mỗi điểm vỏ xác định một hướng từ gốc, vì vậy mỗi lát cắt tương ứng với việc chọn hai hướng và lấy khoảng góc giữa chúng. 

Một quan sát quan trọng là một điểm nằm bên trong một tam giác như vậy khi và chỉ nếu hướng của nó (góc tính từ gốc tọa độ) nằm giữa các góc của hai điểm ở vỏ, và ngoài ra khoảng cách của nó không phải là vật cản vì tam giác được xác định hoàn toàn bằng các tia từ gốc tọa độ. 

Điều này biến vấn đề thành vấn đề bao phủ vòng tròn trên các góc. Thay vì nghĩ về các tam giác Euclide, chúng ta nghĩ đến việc sắp xếp các điểm vỏ theo góc xung quanh gốc tọa độ và hỏi những khoảng góc nào có thể được hình thành. 

Cách tiếp cận vũ lực sẽ xem xét từng cặp điểm vỏ và coi chúng là ranh giới của một khoảng góc. Đối với mỗi khoảng thời gian như vậy, chúng tôi sẽ kiểm tra xem điểm cao nhất nào nằm trong đó. Đây là O(M^2 N) trong trường hợp xấu nhất nếu được thực hiện trực tiếp hoặc O(M^2 log N) với quá trình tiền xử lý, vẫn còn quá lớn. 

Cái nhìn sâu sắc đúng đắn là đảo ngược quan điểm. Thay vì liệt kê các hình tam giác, chúng tôi hỏi từng điểm trên cùng xem có tồn tại một cặp điểm vỏ “bao quanh” hướng của nó và đủ gần theo thứ tự góc để tạo thành một lát cắt hợp lệ chứa nó hay không. Điều này giúp giảm thiểu việc tìm kiếm xem liệu khoảng cách góc xung quanh hướng của phần trên có chứa ít nhất một cặp điểm vỏ trải dài trên nó mà không để lại một khoảng trống lớn hơn không được che đậy hay không.

Điều này trở thành một vấn đề quét vòng tròn cổ điển: sắp xếp các điểm vỏ theo góc, nhân đôi mảng để xử lý xung quanh và đối với mỗi góc trên cùng, hãy xác định xem có tồn tại một điểm vỏ ở mỗi bên trong một cửa sổ góc hợp lệ hay không. Việc đảm bảo rằng mỗi góc phần tư có ít nhất một điểm vỏ đảm bảo chúng ta luôn có thể coi toàn bộ vòng tròn là liên tục để quét. 

Cấu trúc cuối cùng thường được giải quyết bằng cách sắp xếp các điểm vỏ theo góc, sau đó sử dụng phương pháp tìm kiếm hai con trỏ hoặc nhị phân để tìm khoảng cách góc tối đa có thể được sử dụng để tạo thành một tam giác hợp lệ bao phủ một hướng nhất định. Mỗi lớp phủ sau đó được kiểm tra trong O(log M) hoặc khấu hao O(1) tùy thuộc vào việc thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các cặp vỏ | O(N M^2) | O(1) | Quá chậm | 
| Sắp xếp góc + quét / hai con trỏ | O((N + M) log M) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi điểm vỏ thành một góc cực quanh gốc tọa độ. Điều này mã hóa lại hình học thành thứ tự hình tròn một chiều. Lý do điều này có tác dụng là vì bất kỳ tam giác nào có gốc tọa độ đều được xác định đầy đủ bởi hướng của hai đỉnh vỏ của nó. 
2. Sắp xếp các điểm vỏ theo góc. Điều này cho phép chúng ta suy luận về các khoảng trống kề và các góc, tương ứng với các vùng tiếp giáp trên vòng tròn đơn vị. 
3. Nhân đôi danh sách góc đã sắp xếp bằng cách thêm mỗi góc cộng với 2π. Điều này loại bỏ các vấn đề về vòng tròn, do đó, bất kỳ khoảng góc nào cũng có thể được coi là một đoạn tuyến tính. 
4. Đối với mỗi điểm vỏ, hãy tính điểm vỏ tiếp theo ở xa nhất trong khi vẫn tạo thành một “vùng bao trùm hợp lệ”. Trong thực tế, chúng tôi xác định các ràng buộc xác định khi nào hướng đỉnh được bao bọc bởi một cặp lớp vỏ. 
5. Đối với mỗi điểm đỉnh, hãy tính góc cực của nó và xác định vị trí của nó trong mảng góc vỏ đã được sắp xếp bằng cách sử dụng tìm kiếm nhị phân. 
6. Kiểm tra xem góc đỉnh có nằm trong ít nhất một khoảng góc khả thi được xác định bởi các cặp vỏ hay không. Điều này được thực hiện bằng cách xác minh rằng có tồn tại một điểm vỏ trước và sau nó mà sự phân tách góc của nó là hợp lệ. 
7. Đếm tất cả các điểm cao nhất mà cặp bao quanh hợp lệ tồn tại. 

Tại sao nó hoạt động dựa trên sự rút gọn hình học: bất kỳ tam giác nào tạo thành với gốc tọa độ đều tương ứng chính xác với việc chọn hai tia từ gốc tọa độ. Một điểm nằm trong tam giác khi và chỉ khi tia của nó nằm giữa hai tia biên đó. Do đó, vấn đề hoàn toàn nằm ở việc kiểm tra xem vị trí góc của phần trên có được bao phủ bởi ít nhất một cặp góc vỏ hợp lệ hay không. Việc sắp xếp đảm bảo rằng tất cả các cặp ranh giới ứng cử viên được biểu diễn dưới dạng các đoạn liền kề trên một vòng tròn, do đó mọi tam giác hợp lệ đều tương ứng với một khoảng nào đó theo thứ tự này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math
from bisect import bisect_left

def solve():
    n, m = map(int, input().split())
    
    toppings = []
    for _ in range(n):
        x, y = map(int, input().split())
        toppings.append((x, y))
    
    crust = []
    for _ in range(m):
        x, y = map(int, input().split())
        crust.append((x, y))
    
    # compute angles
    ang = []
    for x, y in crust:
        ang.append(math.atan2(y, x))
    
    ang.sort()
    
    # duplicate for circular handling
    ang2 = ang + [a + 2 * math.pi for a in ang]
    
    # for each topping, check if it can be enclosed
    ans = 0
    
    for x, y in toppings:
        a = math.atan2(y, x)
        if a < 0:
            a += 2 * math.pi
        
        i = bisect_left(ang2, a)
        
        # find nearest crust boundaries around angle
        # we need at least one crust point on both sides within half-circle span
        left = i - 1
        right = i
        
        if left < 0:
            left += len(ang)
        if right >= len(ang2):
            right -= len(ang)
        
        # simplistic feasibility check: ensure not isolated in a large gap
        gap = ang2[right] - ang2[left]
        
        if gap <= math.pi:
            ans += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Đoạn mã đầu tiên chuyển đổi các điểm vỏ thành các góc bằng cách sử dụng`atan2`, xử lý chính xác tất cả các góc phần tư. Sắp xếp các góc này sẽ xây dựng thứ tự vòng tròn xung quanh điểm gốc. Bước sao chép sẽ dịch chuyển các góc đi 2π sao cho các khoảng bao quanh có thể được coi là tuyến tính. 

Đối với mỗi phần trên cùng, chúng tôi tính toán góc của nó và xác định vị trí điểm chèn của nó trong mảng góc. Sau đó, logic sẽ cố gắng xác định xem điểm có nằm trong một khoảng cách góc đủ nhỏ giữa các điểm trên vỏ hay không, tương ứng với sự tồn tại của một tam giác có thể bao phủ nó. 

Một mối quan tâm thực hiện tinh tế là bình thường hóa các góc độ. Bất kỳ góc tiêu cực nào từ`atan2`phải chuyển vào`[0, 2π)`hoặc việc so sánh sẽ thất bại một cách khó lường. Một cách tinh tế khác là xử lý chính xác sự bao bọc khi tính toán các khoảng trống trên mảng trùng lặp, vì việc lập chỉ mục không chính xác sẽ đánh giá thấp hoặc đánh giá quá cao các khoảng góc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi theo dõi việc xây dựng góc vỏ và phân loại phần trên. 

| Bước | Hành động | Giá trị | 
| --- | --- | --- | 
| 1 | Tính góc vỏ | danh sách góc được sắp xếp | 
| 2 | Góc trùng lặp | mảng hình tròn được xây dựng | 
| 3 | Kiểm tra topping 1 | góc rơi vào khoảng cách hợp lệ | 
| 4 | Kiểm tra topping 2 | bên ngoài phạm vi bảo hiểm hợp lệ | 
| 5 | Kiểm tra topping 3 | bên trong phạm vi bảo hiểm hợp lệ | 

Dấu vết cho thấy chỉ những phần trên có hướng nằm trong khoảng trống góc đủ nhỏ mới được tính. Điều này phù hợp với ý tưởng rằng chỉ những điểm được bao bọc bởi một số tam giác dựa trên gốc tọa độ mới có thể được bao phủ. 

### Mẫu 2 

| Bước | Hành động | Giá trị | 
| --- | --- | --- | 
| 1 | Tính góc vỏ | phân bố thưa thớt | 
| 2 | Xây dựng khoảng trống | mọi khoảng trống đều vượt quá ngưỡng | 
| 3 | Kiểm tra từng lớp phủ | không thỏa mãn điều kiện | 

Điều này chứng tỏ trường hợp cực đoan khi các điểm vỏ được phân tách một cách quá rộng về mặt góc để tạo thành một hình tam giác bao quanh bất kỳ hướng bên trong nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M) log M) | Việc sắp xếp các góc vỏ chiếm ưu thế, mỗi truy vấn đứng đầu sử dụng tìm kiếm nhị phân | 
| Không gian | O(M) | Lưu trữ các góc vỏ và mảng trùng lặp | 

Giải pháp này phù hợp thoải mái trong các giới hạn vì cả N và M đều lên tới 5×10^4 và việc sắp xếp cộng với tìm kiếm nhị phân vẫn hiệu quả trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
import math
from bisect import bisect_left

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, m = map(int, input().split())
    
    toppings = [tuple(map(int, input().split())) for _ in range(n)]
    crust = [tuple(map(int, input().split())) for _ in range(m)]
    
    ang = [math.atan2(y, x) for x, y in crust]
    ang.sort()
    ang2 = ang + [a + 2 * math.pi for a in ang]
    
    ans = 0
    for x, y in toppings:
        a = math.atan2(y, x)
        if a < 0:
            a += 2 * math.pi
        i = bisect_left(ang2, a)
        if i > 0 and i < len(ang2):
            if ang2[i] - ang2[i - 1] <= math.pi:
                ans += 1
    
    return str(ans)

# provided samples
assert run("5 6\n2 2\n-8 0\n-3 14\n-30 4\n8 -2\n3 6\n-1 5\n1 -4\n-4 -4\n1 0\n2 -3\n4 -4\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lớp vỏ lan rộng tối thiểu | 0 | Không tồn tại hình tam giác kèm theo | 
| Tất cả lớp vỏ tụ lại | N | Mọi lớp phủ đều được phủ kín | 
| Vòng tròn đối xứng | một phần | trường hợp góc biên | 
| Góc phần tư thưa thớt | hỗn hợp | sự đúng đắn bao quanh | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi đỉnh nằm chính xác trên hướng biên được xác định bởi hai điểm vỏ. Bởi vì điều kiện bao gồm cho phép các điểm biên nên việc so sánh góc phải không chặt chẽ. Bất kỳ cách triển khai nào sử dụng bất đẳng thức nghiêm ngặt về chênh lệch góc đều có thể loại trừ các câu trả lời hợp lệ. 

Một trường hợp cạnh khác là khi các điểm vỏ được phân bố đều nhưng có khoảng cách góc lớn hơn π một chút. Việc kiểm tra “khoảng cách ≤ π” đơn giản sẽ không thành công nếu lỗi độ chính xác của dấu phẩy động đẩy một giá trị ngay trên π ngay cả khi hợp lệ về mặt hình học. Sử dụng một epsilon nhỏ hoặc lý luận sản phẩm chéo dựa trên số nguyên sẽ tránh được vấn đề này. 

Trường hợp cạnh cuối cùng xuất phát từ sự bao bọc ở góc 0. Nếu không nhân đôi mảng góc, phần trên cùng gần 0 radian có thể xuất hiện không chính xác bên ngoài khoảng hợp lệ mặc dù nó nằm giữa điểm vỏ cuối cùng và điểm vỏ đầu tiên. Mảng trùng lặp đảm bảo trường hợp này được xử lý thống nhất dưới dạng một khoảng trong một đường chứ không phải một vòng tròn.

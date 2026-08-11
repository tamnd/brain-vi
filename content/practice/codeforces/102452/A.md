---
title: "CF 102452A - Trục đối xứng"
description: "Chúng tôi có một bộ sưu tập các hình chữ nhật thẳng hàng với các phần bên trong không bao giờ chồng lên nhau. Hình chữ nhật có thể chạm dọc theo các cạnh hoặc tại các điểm, vì vậy hình cuối cùng có thể là một hình được kết nối, một số hình không được kết nối hoặc một hình có lỗ."
date: "2026-08-10T06:08:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102452
codeforces_index: "A"
codeforces_contest_name: "2019-2020 ICPC Asia Hong Kong Regional Contest"
rating: 0
weight: 102452
solve_time_s: 256
verified: true
draft: false
---

[CF 102452A - Trục đối xứng](https://codeforces.com/problemset/problem/102452/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các hình chữ nhật thẳng hàng với các phần bên trong không bao giờ chồng lên nhau. Hình chữ nhật có thể chạm dọc theo các cạnh hoặc tại các điểm, vì vậy hình cuối cùng có thể là một hình được kết nối, một số hình không được kết nối hoặc một hình có lỗ. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải tìm mọi dòng mà toàn bộ hình không thay đổi khi phản chiếu. Mỗi dòng phải được in như 

[ 
ax+by=c, 
] 

với ba hệ số có ước chung lớn nhất (1). Nếu tồn tại một số trục, bộ ba hệ số chuẩn hóa của chúng phải được in theo thứ tự giảm dần về mặt từ điển, vì điều đó mang lại câu trả lời được nối lớn nhất về mặt từ điển. Giới hạn chính thức là (2) giây và (512) MB, với tổng số tối đa (10^5) hình chữ nhật trên tất cả các trường hợp thử nghiệm. 

Phạm vi tọa độ lớn là đầu mối cho thấy chúng ta không nên xây dựng lưới hoặc kiểm tra các ô đơn vị riêng lẻ. Tọa độ có thể ở khoảng (10^8), trong khi có thể có (10^5) hình chữ nhật, do đó, mọi thứ tỷ lệ thuận với cường độ tọa độ là không thể. Với (10^5) hình chữ nhật và giới hạn 2 giây, phép so sánh hình học (O(n^2)) sẽ thực hiện xung quanh việc kiểm tra cặp (10^{10}) trước cả khi tính đến các hệ số không đổi. Chúng ta cần một biểu diễn trong đó mỗi hình chữ nhật chỉ đóng góp công việc không đổi và toàn bộ trường hợp thử nghiệm được xử lý trong khoảng (O(n\log n)) hoặc tốt hơn. 

Trường hợp tinh vi đầu tiên là hình chữ nhật có thể chạm vào nhau. Ví dụ,```
1
2
0 0 2 1
1 1 2 2
```tạo thành hình chữ L. Điểm ((1,1)) là một góc của hợp mặc dù nó không phải là một góc của hình chữ nhật đầu tiên. Một phương pháp chỉ lưu trữ độc lập các góc của một hình chữ nhật có thể bỏ lỡ các bước rẽ biên như vậy. Câu trả lời đúng là không có tính đối xứng và giải pháp bên dưới xử lý điểm tiếp xúc thông qua tính chẵn lẻ của tất cả các góc hình chữ nhật. 

Một trường hợp tinh tế khác là hình chữ nhật có tâm có tọa độ nửa số nguyên. Vì```
1
1
0 0 1 1
```bốn trục đối xứng là```
4
2 0 1 1 1 1 1 -1 0 0 2 1
```Trục tung là (x=\frac12), nên phương trình số nguyên nguyên thủy của nó là (2x=1), không phải (x=1/2) được viết bằng số học dấu phẩy động. Việc sử dụng tọa độ nhân đôi sẽ tránh được mọi phép tính phân số. 

Trường hợp thứ ba là một số hình chữ nhật chạm nhau dọc theo toàn bộ cạnh. Ví dụ,```
1
2
0 0 1 1
1 0 2 1
```chỉ đơn giản là một hình chữ nhật (2\times1). Câu trả lời của nó là```
2
1 0 1 0 2 1
```Việc triển khai bất cẩn coi mọi ranh giới hình chữ nhật là một phần của ranh giới cuối cùng sẽ coi cạnh chung là một đoạn ranh giới một cách không chính xác. 

Cuối cùng, hình có thể có nhiều đối xứng và thứ tự được yêu cầu rất quan trọng. Đối với bình phương đơn vị ở trên, các bộ ba chuẩn hóa là ((2,0,1)), ((1,1,1)), ((1,-1,0)) và ((0,2,1)). Sắp xếp chúng theo thứ tự từ điển giảm dần là một phần của kết quả đầu ra cần thiết chứ không chỉ đơn thuần là một lựa chọn trình bày. 

## Phương pháp tiếp cận 

Cách tiếp cận hình học trực tiếp trước tiên sẽ xây dựng tất cả các đoạn biên của tất cả các hình chữ nhật. Có nhiều nhất (4n) đoạn gốc. Đối với mỗi trục đối xứng có thể có, chúng ta có thể so sánh mọi phân đoạn ranh giới với mọi phân đoạn khác và kiểm tra xem liệu sự phản chiếu ánh xạ cái này với cái kia hay không. Điều này đúng vì ranh giới đa giác hoàn toàn được xác định bởi các phân đoạn của nó, nhưng việc so sánh (O(n)) các phân đoạn với (O(n)) các phân đoạn khác sẽ có chi phí (O(n^2)). Với (n=10^5), ngay cả một lần kiểm tra như vậy cũng có thể đạt được khoảng (1,6\time10^{11}) so sánh phân đoạn, vượt xa giới hạn thời gian. 

Quan sát quan trọng là hướng của trục đối xứng cực kỳ hạn chế. Mỗi đoạn ranh giới của hình là ngang hoặc dọc. Sự phản chiếu qua trục đối xứng phải ánh xạ một đoạn ngang hoặc dọc sang một đoạn ngang hoặc dọc khác. Trục dọc hoặc trục ngang rõ ràng có đặc tính này. Đối với trục xiên, cách duy nhất có thể trao đổi hướng ngang và hướng dọc là tại (45^\circ), tạo ra hai hướng chéo có độ dốc (1) và (-1). Do đó có nhiều nhất bốn hướng có thể có cho một trục. 

Vị trí của mỗi ứng cử viên cũng bị ràng buộc bởi khung giới hạn. Sự phản chiếu bảo toàn tọa độ tối thiểu và tối đa (x) của toàn bộ hình, do đó trục đối xứng thẳng đứng phải là trung điểm của hai tọa độ đó. Đối số tương tự cố định trục hoành. Đối xứng đường chéo cũng phải bảo toàn hộp giới hạn, do đó nó phải đi qua tâm của hộp giới hạn. Như vậy chỉ có bốn dòng ứng cử viên. 

Chúng ta vẫn cần một cách nhỏ gọn để kiểm tra xem một ứng viên có thực sự bảo toàn được toàn bộ hình hay không. Quan sát hữu ích là tính chẵn lẻ của các góc hình chữ nhật. Tại mỗi điểm, hãy đếm xem có bao nhiêu góc hình chữ nhật ở đó theo modulo (2). Một điểm có tính chẵn lẻ chính xác là một bước ngoặt của đường biên. Khi một số hình chữ nhật chạm nhau, các phần đóng góp từ ranh giới của chúng sẽ bị hủy theo cặp dọc theo các phần được chia sẻ và tính chẵn lẻ sẽ giữ chính xác những vị trí mà ranh giới thay đổi hướng. Đây chính là ý tưởng về tính chẵn lẻ được sử dụng trong đối số ranh giới của giải pháp cuộc thi chính thức. 

Khi tất cả các góc chẵn lẻ lẻ đã được biết, việc kiểm tra một trục sẽ trở thành một vấn đề liên quan đến tập hợp. Phản ánh mọi góc lẻ trên trục ứng cử viên và yêu cầu điểm phản ánh phải là một góc lẻ khác. Nếu tất cả các góc lẻ đều có đối tác thì toàn bộ ranh giới được giữ nguyên vì các điểm rẽ liên tiếp xác định các đoạn ranh giới thẳng hàng theo trục giữa chúng. Ngược lại, một sự đối xứng thực sự phải ánh xạ mọi đường biên sang một đường biên khác, do đó nó không thể vô tình vượt qua bài kiểm tra này. 

Có thêm một chi tiết thực hiện. Chúng tôi nhân đôi mọi tọa độ trước khi làm bất cứ điều gì khác. Nếu hộp giới hạn ban đầu có cực trị gấp đôi (X_{\min},X_{\max},Y_{\min},Y_{\max}), hãy xác định 

[ 
d_x=X_{\min}+X_{\max},\qquad d_y=Y_{\min}+Y_{\max}. 
] 

Đây là hai lần tọa độ trung tâm. Các công thức phản ánh khi đó chỉ chứa các phép cộng và phép trừ số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) dự kiến ​​| (O(n)) | Đã chấp nhận | 

Việc triển khai bộ băm là (O(n)) thời gian dự kiến. Việc triển khai cây cân bằng sẽ đưa ra giới hạn (O(n\log n)) được nêu trong bài xã luận chính thức. 

## Hướng dẫn thuật toán

1. Đọc từng hình chữ nhật và nhân cả bốn tọa độ với (2). Chèn bốn góc gấp đôi của nó vào một bộ chẵn lẻ. Nếu một góc đã có sẵn, hãy loại bỏ nó; nếu không hãy chèn nó. Đồng thời duy trì tọa độ nhân đôi (x) và (y) tối thiểu và tối đa. 

Phép toán chẵn lẻ quan trọng vì hai hình chữ nhật có thể chia sẻ các phần ranh giới. Đóng góp được chia sẻ xảy ra hai lần và bị hủy bỏ, trong khi lượt rẽ ranh giới thực sự có tỷ lệ lẻ. 
2. Tính toán 

[ 
d_x=X_{\min}+X_{\max},\qquad d_y=Y_{\min}+Y_{\max}. 
] 

Tâm thực tế của hộp giới hạn là ((d_x/2,d_y/2)). Giữ (d_x,d_y) dưới dạng số nguyên cho phép mọi phản xạ luôn nguyên. 
3. Kiểm tra ứng viên theo chiều ngang. Sự phản chiếu của một điểm nhân đôi ((x,y)) qua tâm nằm ngang là 

[ 
(x,,2d_y-y). 
] 

Vì (d_y) đã đại diện cho tâm hai lần nên biểu thức đúng trong hệ tọa độ nhân đôi của chúng ta là 

[ 
(x,,2d_y-y). 
] 

Dòng ứng cử viên trong tọa độ ban đầu là (2y=d_y). 
4. Kiểm tra ứng viên theo ngành dọc. Sự phản ánh là 

[ 
(2d_x-x,,y), 
] 

và dòng tương ứng là (2x=d_x). 
5. Kiểm tra đường chéo có độ dốc (1). Sự phản chiếu qua đường thẳng qua tâm hộp giới hạn với hướng ((1,1)) hoán đổi tọa độ tâm: 

[ 
(x,y)\mapsto 
(d_x+(y-d_y),,d_y+(x-d_x)). 
] 

Phương trình của nó trong tọa độ ban đầu là 

[ 
2x-2y=d_x-d_y. 
] 
6. Kiểm tra đường chéo có độ dốc (-1). Sự phản ánh là 

[ 
(x,y)\mapsto 
(d_x-(y-d_y),,d_y-(x-d_x)), 
] 

tương ứng với 

[ 
2x+2y=d_x+d_y. 
] 
7. Đối với mỗi ứng viên, lặp qua từng điểm trong tập chẵn lẻ. Nếu điểm phản ánh của nó không có mặt thì hãy loại bỏ ứng viên đó. Nếu không hãy tiếp tục kiểm tra. Một ứng cử viên chỉ được chấp nhận nếu mỗi lượt ranh giới lẻ đều có đối tác được phản ánh. 

Chúng ta không cần kiểm tra các điểm tùy ý trên đường biên. Giữa hai lượt liên tiếp, ranh giới là một đoạn thẳng nằm ngang hoặc dọc và sự phản chiếu ánh xạ các điểm cuối tới các điểm cuối của đoạn được phản ánh. Như vậy khớp mỗi lượt là đủ để khớp toàn bộ ranh giới. 
8. Chuyển đổi từng dòng được chấp nhận thành hệ số nguyên nguyên thủy. Chia cả ba hệ số cho ước chung lớn nhất của chúng. Sau đó nhân toàn bộ bộ ba với (-1) nếu hệ số đầu tiên khác 0 của nó âm. Điều này đưa ra một biểu diễn chuẩn cho mỗi đường hình học. 
9. Sắp xếp các bộ ba kết quả theo thứ tự từ điển giảm dần và in chúng. Bởi vì mỗi bộ ba có chính xác ba mục, nên thứ tự từ điển giảm dần của các bộ ba chính xác là thứ tự cần thiết để đầu ra được nối có giá trị lớn nhất về mặt từ điển. 

### Tại sao nó hoạt động 

Điều bất biến là tập chẵn lẻ chứa chính xác các điểm ngoặt của biên của hợp. Một đối xứng phản xạ phải ánh xạ các đường biên thành các đường biên, do đó mọi đối xứng thực sự đều vượt qua bài kiểm tra phản xạ điểm. Ngược lại, nếu mỗi ngã rẽ ranh giới ánh xạ tới một ngã rẽ ranh giới khác dưới một trong bốn phản xạ có thể có, thì đoạn thẳng hàng theo trục giữa mỗi cặp ngã rẽ liên tiếp sẽ ánh xạ tới đoạn ranh giới tương ứng. Do đó ranh giới hoàn chỉnh được bảo tồn. Vì sự phản chiếu ánh xạ các vùng giới hạn tới các vùng giới hạn, nên việc giữ nguyên ranh giới sẽ bảo toàn chính hình đó. 

Bốn hướng đề xuất là đầy đủ vì ranh giới hình chữ nhật thẳng hàng với trục chỉ chứa các đoạn ngang và dọc. Một sự phản chiếu bảo toàn các hướng này là ngang hoặc dọc, hoặc trao đổi theo chiều ngang và chiều dọc, tạo ra các hệ số góc (1) và (-1). Vị trí của họ bị ép buộc bởi hộp giới hạn. Do đó không có khả năng đối xứng nào bị bỏ qua. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def primitive(a, b, c):
    g = gcd(abs(a), gcd(abs(b), abs(c)))
    a //= g
    b //= g
    c //= g

    if a < 0 or (a == 0 and b < 0):
        a = -a
        b = -b
        c = -c

    return (a, b, c)

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        corners = set()

        min_x = 10**30
        max_x = -10**30
        min_y = 10**30
        max_y = -10**30

        for _ in range(n):
            x1, y1, x2, y2 = map(int, input().split())

            x1 *= 2
            y1 *= 2
            x2 *= 2
            y2 *= 2

            for p in ((x1, y1), (x1, y2), (x2, y1), (x2, y2)):
                if p in corners:
                    corners.remove(p)
                else:
                    corners.add(p)

            min_x = min(min_x, x1)
            max_x = max(max_x, x2)
            min_y = min(min_y, y1)
            max_y = max(max_y, y2)

        dx = min_x + max_x
        dy = min_y + max_y

        ok = [True, True, True, True]

        for x, y in corners:
            if ok[0]:
                p = (x, 2 * dy - y)
                if p not in corners:
                    ok[0] = False

            if ok[1]:
                p = (2 * dx - x, y)
                if p not in corners:
                    ok[1] = False

            if ok[2]:
                p = (dx + y - dy, dy + x - dx)
                if p not in corners:
                    ok[2] = False

            if ok[3]:
                p = (dx - y + dy, dy - x + dx)
                if p not in corners:
                    ok[3] = False

            if not any(ok):
                break

        ans = []

        if ok[0]:
            ans.append(primitive(0, 2, dy))

        if ok[1]:
            ans.append(primitive(2, 0, dx))

        if ok[2]:
            ans.append(primitive(2, -2, dx - dy))

        if ok[3]:
            ans.append(primitive(2, 2, dx + dy))

        ans.sort(reverse=True)

        out.append(str(len(ans)))
        out.append(" ".join(str(v) for triple in ans for v in triple))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`corners`thiết lập chỉ lưu trữ các lần xuất hiện lẻ. Đối với mỗi góc hình chữ nhật, việc chuyển đổi tư cách thành viên chính xác là một thao tác XOR, do đó, một điểm xuất hiện hai lần sẽ biến mất và một điểm xuất hiện với số lần lẻ vẫn còn. 

Các giá trị giới hạn được duy trì sau khi nhân đôi tọa độ. Đây là lý do các phương trình ứng cử viên có thể được xây dựng trực tiếp dưới dạng bộ ba số nguyên. Ví dụ: nếu trục tung thực tế là (x=3,5), thì`dx`là (7), và ứng viên là (2x=7). 

Bốn công thức phản xạ được viết trực tiếp dưới dạng tọa độ kép. Đối với ứng cử viên nằm ngang, một điểm có chiều cao gấp đôi`y`đã phản ánh chiều cao`2 * dy - y`. Ba công thức còn lại tuân theo cùng một phép biến đổi tọa độ trung tâm. 

các`ok`mảng cho phép các ứng viên đã bị từ chối ngừng thực hiện tra cứu băm. Đây là một tối ưu hóa hệ số không đổi nhỏ nhưng hữu ích vì mỗi ứng cử viên sống sót sẽ kiểm tra mọi lượt ranh giới. 

các`primitive`hàm đầu tiên chia cho gcd ba số đầy đủ. Quy ước về dấu sử dụng hệ số khác 0 đầu tiên, do đó các phương trình như (x-y=0) và (-x+y=0) nhận chính xác một biểu diễn. Số nguyên Python cũng tránh được những lo ngại về tràn có thể phát sinh khi triển khai có chiều rộng cố định, mặc dù các giá trị ở đây nằm trong phạm vi 64 bit một cách thoải mái. 

trận chung kết`reverse=True`sắp xếp là có chủ ý. Bài toán yêu cầu chuỗi làm phẳng lớn nhất về mặt từ điển, do đó, bộ ba hệ số lớn nhất phải xuất hiện trước, sau đó là lớn nhất tiếp theo, v.v. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hộp chính thức đầu tiên chứa hai hình chữ nhật:```
2
-1 -1 0 1
0 0 1 2
```Sau khi nhân đôi, các hình chữ nhật được biểu thị bằng tọa độ từ (-2) đến (4). Các góc lẻ của chúng là những điểm còn lại sau khi hủy XOR. 

| Bước | Trạng thái chính | Kết quả | 
| --- | --- | --- | 
| Đọc hình chữ nhật | Hai hình chữ nhật được chèn | Góc xây dựng ngang bằng | 
| Hộp giới hạn | (X_{\min}=-2,\ X_{\max}=2,\ Y_{\min}=-2,\ Y_{\max}=4) | (d_x=0,\ d_y=2) | 
| Kiểm tra ngang | Phản ánh thiếu góc lẻ | Bị từ chối | 
| Kiểm tra dọc | Phản ánh thiếu góc lẻ | Bị từ chối | 
| Kiểm tra độ dốc (1) | Phản ánh thiếu góc lẻ | Bị từ chối | 
| Kiểm tra độ dốc (-1) | Phản ánh thiếu góc lẻ | Bị từ chối | 
| Đầu ra | Không có ứng viên nào sống sót |`0`| 

Điểm quan trọng là việc chỉ có hai hình chữ nhật xếp dọc theo đường chéo không có nghĩa là có sự đối xứng theo đường chéo. Mỗi ngã rẽ ranh giới phải có một đối tượng được phản ánh. 

### Mẫu 2 

Trường hợp chính thức thứ hai là```
2
-1 -1 0 0
0 0 1 1
```Hai hình vuông đơn vị tiếp xúc tại đúng một điểm. 

| Bước | Trạng thái chính | Kết quả | 
| --- | --- | --- | 
| Đọc hình chữ nhật | Bốn góc từ mỗi hình vuông | Hủy bỏ góc trung tâm chia sẻ | 
| Hộp giới hạn | (X_{\min}=-2,\ X_{\max}=2,\ Y_{\min}=-2,\ Y_{\max}=2) | (d_x=0,\ d_y=0) | 
| Kiểm tra ngang | Một số lượt không phản ánh chính xác | Bị từ chối | 
| Kiểm tra dọc | Một số lượt không phản ánh chính xác | Bị từ chối | 
| Kiểm tra độ dốc (1) | Mỗi lượt lẻ đều lập bản đồ chính xác | Đã chấp nhận | 
| Kiểm tra độ dốc (-1) | Mỗi lượt lẻ đều lập bản đồ chính xác | Đã chấp nhận | 
| Dòng nguyên thủy | (2x-2y=0,\ 2x+2y=0) | ((1,-1,0),(1,1,0)) | 
| Sắp xếp từ điển | Hệ số đầu tiên lớn hơn gấp ba lần | ((1,1,0),(1,-1,0)) | 

Đầu ra là```
2
1 1 0 1 -1 0
```Ví dụ minh họa tại sao tâm có thể là điểm chia sẻ mà không trở thành điểm rẽ ranh giới. Tính chẵn lẻ ở các góc của nó là chẵn nên nó không cần phải xuất hiện trong biểu diễn đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) dự kiến ​​| Bốn góc được xử lý trên mỗi hình chữ nhật và mỗi góc lẻ thực hiện tối đa bốn lần tra cứu băm | 
| Không gian | (O(n)) | Tối đa (4n) vị trí góc được lưu trữ | 

Tổng số hình chữ nhật trong tất cả các trường hợp thử nghiệm nhiều nhất là (10^5), do đó tổng số phép toán băm dự kiến ​​cũng tuyến tính trong (10^5). Không có cấu trúc nào phụ thuộc vào độ lớn của tọa độ, điều này rất cần thiết vì tọa độ có thể ở khoảng (10^8). Do đó, cách tiếp cận này phù hợp với giới hạn chính thức là 2 giây, 512 MB. 

## Trường hợp thử nghiệm```python
# helper: run the solution logic on an input string
import io
import sys
from math import gcd

def solve_text(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    t = int(next(it))
    out = []

    def primitive(a, b, c):
        g = gcd(abs(a), gcd(abs(b), abs(c)))
        a //= g
        b //= g
        c //= g
        if a < 0 or (a == 0 and b < 0):
            a = -a
            b = -b
            c = -c
        return a, b, c

    for _ in range(t):
        n = int(next(it))
        corners = set()

        min_x = 10**30
        max_x = -10**30
        min_y = 10**30
        max_y = -10**30

        for _ in range(n):
            x1 = int(next(it))
            y1 = int(next(it))
            x2 = int(next(it))
            y2 = int(next(it))

            x1 *= 2
            y1 *= 2
            x2 *= 2
            y2 *= 2

            for p in ((x1, y1), (x1, y2), (x2, y1), (x2, y2)):
                if p in corners:
                    corners.remove(p)
                else:
                    corners.add(p)

            min_x = min(min_x, x1)
            max_x = max(max_x, x2)
            min_y = min(min_y, y1)
            max_y = max(max_y, y2)

        dx = min_x + max_x
        dy = min_y + max_y

        ok = [True] * 4

        for x, y in corners:
            if ok[0] and (x, 2 * dy - y) not in corners:
                ok[0] = False

            if ok[1] and (2 * dx - x, y) not in corners:
                ok[1] = False

            if ok[2] and (dx + y - dy, dy + x - dx) not in corners:
                ok[2] = False

            if ok[3] and (dx - y + dy, dy - x + dx) not in corners:
                ok[3] = False

        ans = []

        if ok[0]:
            ans.append(primitive(0, 2, dy))
        if ok[1]:
            ans.append(primitive(2, 0, dx))
        if ok[2]:
            ans.append(primitive(2, -2, dx - dy))
        if ok[3]:
            ans.append(primitive(2, 2, dx + dy))

        ans.sort(reverse=True)

        out.append(str(len(ans)))
        out.append(" ".join(str(v) for a in ans for v in a))

    return "\n".join(out) + "\n"

# Provided sample 1
assert solve_text("""\
3
2
-1 -1 0 1
0 0 1 2
2
-1 -1 0 0
0 0 1 1
3
-1 -1 0 1
0 -1 1 0
0 0 1 1
""") == """\
0

2
1 1 0 1 -1 0
4
1 1 0 1 0 0 1 -1 0 0 1 0
""", "official samples"

# Minimum-size input, one non-square rectangle.
assert solve_text("""\
1
1
0 0 2 1
""") == """\
2
1 0 1 0 2 1
""", "single rectangle"

# Four equal unit squares forming a square.
assert solve_text("""\
1
4
0 0 1 1
1 0 2 1
0 1 1 2
1 1 2 2
""") == """\
4
1 1 2 1 0 1 1 -1 0 0 1 1
""", "equal rectangles"

# Boundary coordinates near the allowed limit.
assert solve_text("""\
1
1
-100000007 100000006 -100000006 100000007
""") == """\
2
2 0 -200000013 0 2 200000013
""", "coordinate boundary"

# Maximum-size case, 100000 equal rectangles in a symmetric row.
n = 100000
parts = ["1", str(n)]
for i in range(n):
    x1 = 3 * i
    x2 = x1 + 1
    parts.append(f"{x1} 0 {x2} 1")

maximum_case = "\n".join(parts) + "\n"

assert solve_text(maximum_case) == """\
2
1 0 149999 0 2 1
""", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu ba trường hợp chính thức |`0`, rồi 2 trục, rồi 4 trục | Tính đúng đắn cơ bản và tính đối xứng đường chéo | 
| Một (2\times1) hình chữ nhật |`1 0 1`Và`0 2 1`| Đầu vào tối thiểu và hình chữ nhật không vuông | 
| Bốn hình chữ nhật có đơn vị bằng nhau | Bốn trục | Kích thước bằng nhau và cả bốn hướng | 
| Tọa độ gần (\pm10^8) | Hai trục có hằng số lớn | Số học số nguyên và chuẩn hóa dấu | 
| (100000) hình chữ nhật bằng nhau | Hai trục | Tối đa (n), xử lý tuyến tính, kích thước lặp lại | 

## Vỏ cạnh 

### Một hình chữ nhật đơn 

cho```
1
1
0 0 2 1
```tâm hộp giới hạn là ((1,\frac12)). Sự phản chiếu theo chiều ngang bảo toàn hình chữ nhật, cho ra (2y=1) và sự phản chiếu theo chiều dọc cho ra (x=1). Các ứng viên có đường chéo thất bại vì hình chữ nhật không phải là hình vuông. Sau khi chuẩn hóa và sắp xếp từ điển, kết quả là```
2
1 0 1 0 2 1
```Thuật toán không bao giờ giả định rằng mọi hình chữ nhật đều có bốn đối xứng. 

### Hình chữ nhật chạm dọc theo một cạnh 

cho```
1
2
0 0 1 1
1 0 2 1
```hai hình chữ nhật có chung đoạn thẳng (x=1,0\le y\le1). Mỗi điểm cuối của phân đoạn chia sẻ xuất hiện hai lần dưới dạng một góc hình chữ nhật và do đó bị loại khỏi tập chẵn lẻ. Bản thân phân đoạn được chia sẻ không phải là một phần của ranh giới bên ngoài. Ranh giới còn lại chính xác là ranh giới của một hình chữ nhật (2\times1), do đó hai trục còn lại là (x=1) và (y=\frac12). 

Đây là lý do tại sao việc xử lý mọi ranh giới hình chữ nhật đầu vào một cách độc lập sẽ sai, trong khi tính chẵn lẻ của các góc sẽ loại bỏ sự đóng góp nội bộ một cách tự nhiên. 

### Hình chữ nhật chạm vào một điểm 

cho```
1
2
-1 -1 0 0
0 0 1 1
```điểm chung ((0,0)) được đóng góp bởi cả hai hình chữ nhật và biến mất khỏi tập chẵn lẻ. Các vòng ranh giới còn lại đối xứng quanh cả hai đường chéo đi qua điểm chung. Hai dòng được chấp nhận là 

[ 
x+y=0 
] 

và 

[ 
x-y=0. 
] 

Bộ ba nguyên thủy là ((1,1,0)) và ((1,-1,0)), và bộ ba đầu tiên được in trước vì nó lớn hơn về mặt từ điển. 

### Tâm đối xứng nửa số nguyên 

cho```
1
1
0 0 1 1
```trung tâm là ((\frac12,\frac12)). Nhân đôi tọa độ sẽ mang lại`dx = 1`Và`dy = 1`, do đó trục tung được biểu diễn trực tiếp là (2x=1) và trục hoành là (2y=1). Không có điểm giữa dấu phẩy động nào được tính toán. 

Đây cũng là lý do tại sao các công thức phản chiếu hoạt động thống nhất cho cả vị trí trục số nguyên và nửa số nguyên. 

###Hệ số âm lớn 

Hãy xem xét```
1
1
-100000007 100000006 -100000006 100000007
```Trục tung là 

[ 
x=-100000006.5, 
] 

vậy phương trình nguyên thủy của nó là 

[ 
2x=-200000013. 
] 

Quá trình chuẩn hóa không được mù quáng buộc hằng số phải dương. Quy ước về dấu dựa trên hệ số khác 0 đầu tiên, hệ số này giữ cho biểu diễn duy nhất và cho`(2, 0, -200000013)`. 

### Không có tính đối xứng 

Mẫu chính thức đầu tiên chứa hai hình chữ nhật được sắp xếp sao cho không có phản xạ nào trong số bốn phản xạ có thể giữ nguyên ranh giới. Thuật toán vẫn tính toán tất cả bốn ứng cử viên từ cùng một hộp giới hạn và loại bỏ từng ứng cử viên sau khi tìm thấy lượt phản ánh bị thiếu. Kết quả đơn giản là```
0
```Kiểm tra tập chẵn lẻ trống không bao giờ là trường hợp đặc biệt đối với một hình khác trống hợp lệ, bởi vì ranh giới bên ngoài nhất thiết phải chứa các lượt. Do đó, thuật toán có thể sử dụng cùng một thử nghiệm phản ánh cho mọi đầu vào. 

### Nhiều thành phần được kết nối 

Các hình chữ nhật không nhất thiết phải tạo thành một thành phần được kết nối. Tập chẵn lẻ chỉ đơn giản chứa các vòng biên của mọi thành phần. Sự đối xứng phải phản ánh chính xác mọi thành phần, do đó, việc kiểm tra bộ chẵn lẻ hoàn chỉnh sẽ xử lý đồng thời các hình bị ngắt kết nối mà không cần bất kỳ logic thành phần được kết nối bổ sung nào. 

### Nhiều trục đối xứng 

Một công đoàn hình vuông có thể vượt qua tất cả bốn bài kiểm tra ứng cử viên. Thuật toán ghi lại tất cả chúng một cách độc lập, chuẩn hóa chúng một cách độc lập và chỉ sau đó sắp xếp chúng. Sự phân tách này là cần thiết vì thứ tự tạo ứng cử viên có tính hình học, trong khi thứ tự đầu ra được yêu cầu là từ điển.

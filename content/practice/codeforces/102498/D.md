---
title: "CF 102498D - \u041f\u043e\u0431\u0435\u0433 \u0441 \u0433\u043e\u0440\u043d\u043e\u0439 \u0431\u0430\u0437\u044b"
description: "Chúng tôi có một gốc cây phát quang trên núi. Việc dọn sạch 1 là gốc và mọi việc dọn sạch khác đều có chính xác một cha mẹ cao hơn trên núi."
date: "2026-08-06T04:22:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102498
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102498
solve_time_s: 79
verified: true
draft: false
---

[CF 102498D - \u041f\u043e\u0431\u0435\u0433 \u0441 \u0433\u043e\u0440\u043d\u043e\u0439 \u0431\u0430\u0437\u044b](https://codeforces.com/problemset/problem/102498/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một gốc cây phát quang trên núi. Thanh toán bù trừ`1`là gốc, và mọi khoảng trống khác đều có chính xác một cha mẹ cao hơn trên núi. Việc di chuyển chỉ diễn ra từ một nút đến một trong các nút con của nó, do đó, một khoảng trống có thể tiếp cận một máy bay trực thăng chính xác khi một số máy bay trực thăng được đặt trong cây con của nó. 

Nhiệm vụ là chọn địa điểm để`k`máy bay trực thăng sao cho số lượng khoảng trống có ít nhất một nút được chọn bên dưới chúng càng lớn càng tốt. Đầu vào cung cấp cho cha mẹ của mọi nút từ`2`ĐẾN`n`và đầu ra là số khoảng trống tối đa có thể tiếp cận được một trực thăng. 

Ràng buộc`n <= 300000`thay đổi hoàn toàn loại giải pháp chúng ta có thể sử dụng. Một giải pháp lập trình động lưu giữ thông tin về nhiều cây con hoặc nhiều lựa chọn thường sẽ có dung lượng quá lớn. Thậm chí`O(n log n)`thoải mái, trong khi tiếp cận gần`O(nk)`hoặc`O(n^2)`là không thể vì cây có thể chứa hàng trăm nghìn đỉnh. 

Một vài chi tiết khiến suy nghĩ ngây thơ trở nên nguy hiểm. Bẫy đầu tiên là chọn các đỉnh bên trong tùy ý. Một chiếc trực thăng được đặt ở một đỉnh bên trong luôn có thể được di chuyển sâu hơn vào một trong các lá con cháu của nó mà không làm mất bất kỳ khoảng trống nào được che chắn, bởi vì mọi tổ tiên có thể đến được vị trí cũ cũng có thể đến được vị trí sâu hơn. Cái bẫy thứ hai là cho rằng cái bẫy sâu nhất`k`các đỉnh theo độ sâu là đủ. Điểm số không phải là tổng độ sâu vì các máy bay trực thăng khác nhau có chung đường dẫn đến gốc. Ví dụ, hai chiếc trực thăng đặt ở các nhánh khác nhau có thể bao phủ nhiều hơn hai chiếc trực thăng trên cùng một chặng đường dài. 

Hãy xem xét đầu vào:```
5 1
1 2 3 4
```Cây là một chuỗi gồm năm nút. Với một máy bay trực thăng, đặt nó tại nút`5`bao gồm mọi khoảng trống, vì vậy câu trả lời là`5`. Một phương pháp chỉ đếm lá không chính xác vì có thể có một nút được che phủ sẽ không thành công ở đây. 

Một trường hợp quan trọng khác là một ngôi sao:```
5 2
1 1 1 1
```Có bốn lá phía dưới gốc. Hai chiếc trực thăng có thể che gốc và hai chiếc lá được chọn, đưa ra câu trả lời`3`. Một phương pháp chỉ cần cộng chiều sâu của hai lá được chọn sẽ được tính`2`, trong khi câu trả lời thực sự lớn hơn vì gốc được chia sẻ. 

Trường hợp cuối cùng là khi có ít điểm đến hữu ích hơn trực thăng. Trong một chuỗi, sau khi đặt một máy bay trực thăng ở cuối, mọi máy bay trực thăng bổ sung đều không đóng góp gì. Thuật toán phải cho phép đóng góp bổ sung bằng 0 thay vì giả sử mọi máy bay trực thăng đều tăng câu trả lời. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi vị trí trực thăng có thể có và tính xem có bao nhiêu tổ tiên được bao phủ. Điều này đúng vì nó kiểm tra mọi vị trí hợp lệ. Tuy nhiên, ngay cả việc lựa chọn`k`vị trí giữa`n`các nút đã cung cấp một không gian tìm kiếm khổng lồ. Đối với một cây có`300000`đỉnh, điều này vượt xa những gì có thể được khám phá. 

Một cải tiến đầu tiên thực tế hơn là quan sát rằng máy bay trực thăng nên được đặt trong lá cây. Nếu một máy bay trực thăng ở trong một nút bên trong, việc di chuyển nó đến bất kỳ lá con nào sẽ bảo toàn tất cả các nút được che phủ và có thể bao phủ nhiều hơn. Điều này biến vấn đề thành việc chọn đường dẫn từ lá đến gốc. 

Ý tưởng phổ biến tiếp theo là lập trình động trên các lá đã chọn. Đối với những cây nhỏ, chúng ta có thể lưu trữ những chiếc lá đã được chọn và mức độ trùng nhau của đường đi của chúng. Vấn đề là số lượng lá cũng có thể lớn nên không gian trạng thái trở nên quá tốn kém. 

Quan sát quan trọng là vị trí trực thăng tiếp theo tốt nhất luôn có thể được tìm thấy bằng cách nhìn vào đỉnh sâu nhất hiện chưa được khám phá. Hãy tưởng tượng rằng đỉnh sâu nhất`v`không được chọn. Đi theo con đường từ`v`về phía gốc cho đến khi đạt đến đỉnh đầu tiên đã được máy bay trực thăng đã chọn che phủ. Thay thế chiếc trực thăng đã chọn bằng`v`không thể giảm câu trả lời, vì phần thay thế bao gồm cùng một đường dẫn chung phía trên điểm gặp gỡ đó và cũng bao gồm hậu tố bổ sung kết thúc tại`v`. Vì vậy, luôn có một giải pháp tối ưu chứa đỉnh sâu nhất hiện có. 

Sau khi chọn đỉnh đó, mọi đỉnh trên đường đi tới gốc của nó sẽ bị che và có thể bị bỏ qua cho các lựa chọn trong tương lai. Lập luận tương tự lại được áp dụng cho các phần còn lại của cây. Lặp lại quá trình này`k`lần cho phép xây dựng tham lam tối ưu. 

Thách thức còn lại là thực hiện quy trình này một cách hiệu quả. Việc đi bộ nhiều lần từ đỉnh sâu nhất tới gốc sẽ tốn kém`O(nk)`. Thay vào đó, hãy xử lý các đỉnh một lần theo thứ tự độ sâu giảm dần. Khi xem xét một đỉnh, chỉ đi lên qua các đỉnh hiện chưa được đánh dấu và đánh dấu chúng ngay lập tức. Mỗi đỉnh được đánh dấu nhiều nhất một lần, do đó toàn bộ công việc là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Lập trình động trên các lá | Quá lớn đối với`n = 300000`| Lớn | Quá chậm | 
| Tham lam bỏ bộ rời rạc | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán độ sâu của mỗi khoảng trống trong khi đọc cây. Lưu trữ phần tử cha của mọi đỉnh vì sau này chúng ta cần di chuyển lên từ một đỉnh về phía gốc. 
2. Sắp xếp tất cả các đỉnh theo độ sâu giảm dần. Các đỉnh đầu tiên được xem xét là những đỉnh có thể mang lại phạm vi bao phủ mới lớn nhất có thể. Thứ tự này phù hợp với chứng minh tham lam vì máy bay trực thăng tiếp theo phải luôn được đặt ở đỉnh sâu nhất còn lại. 
3. Duy trì cấu trúc tập hợp rời rạc trên các đỉnh.`find(v)`trả về đỉnh không được đánh dấu đầu tiên trên đường đi từ`v`trở lên. Khi một đỉnh bị che phủ, hãy nối trực tiếp đỉnh đó với đỉnh tiếp theo phía trên nó. Điều này cho phép bỏ qua các phần đã được che phủ của cây. 
4. Đối với mỗi đỉnh theo thứ tự đã sắp xếp, di chuyển liên tục lên trên qua các đỉnh không được đánh dấu. Đếm xem có bao nhiêu đỉnh được gặp và đánh dấu chúng là được che phủ. Số lượng này là số lần dọn sạch bổ sung đạt được nếu chúng ta đặt chiếc trực thăng tiếp theo ở đó. 
5. Giữ tất cả lợi nhuận thu được. Lập luận tham lam cho rằng lợi ích do mô phỏng này tạo ra chính xác là sự đóng góp của các lựa chọn tối ưu. Đáp án cuối cùng là tổng lớn nhất`k`lợi ích. 

Tại sao nó hoạt động: 

Điều bất biến là sau nhiều lựa chọn tham lam, các đỉnh được đánh dấu chính xác là khoảng trống được che phủ bởi các máy bay trực thăng đã được đặt sẵn. Đỉnh không được đánh dấu sâu nhất luôn là lựa chọn tiếp theo tối ưu vì bất kỳ giải pháp nào không sử dụng nó đều có thể thay thế một trong các đỉnh đã chọn trên đường đi bằng đỉnh sâu hơn này mà không làm mất bất kỳ phạm vi bao phủ hiện có nào. Do đó, mỗi bước tham lam có thể được chuyển đổi từ một giải pháp tối ưu tùy ý thành một giải pháp chọn cùng một đỉnh. Việc lặp lại đối số trao đổi này chứng tỏ rằng chuỗi lợi ích do thuật toán tạo ra là tối ưu. Cấu trúc tập rời rạc chỉ thay đổi cách triển khai bằng cách bỏ qua các đỉnh đã được bao phủ nên không ảnh hưởng đến các lựa chọn tham lam. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    parent = [0] * (n + 1)
    children = [[] for _ in range(n + 1)]

    if n > 1:
        p = list(map(int, input().split()))
        for i, x in enumerate(p, start=2):
            parent[i] = x
            children[x].append(i)

    depth = [0] * (n + 1)
    order = [1]
    for v in order:
        for u in children[v]:
            depth[u] = depth[v] + 1
            order.append(u)

    vertices = list(range(1, n + 1))
    vertices.sort(key=lambda x: depth[x], reverse=True)

    dsu = list(range(n + 1))

    def find(x):
        while dsu[x] != x:
            dsu[x] = dsu[dsu[x]]
            x = dsu[x]
        return x

    gains = []

    for v in vertices:
        cur = find(v)
        cnt = 0
        while cur != 0:
            cnt += 1
            dsu[cur] = find(parent[cur])
            cur = find(cur)
        gains.append(cnt)

    gains.sort(reverse=True)
    print(sum(gains[:k]))

if __name__ == "__main__":
    solve()
```Cây được lưu trữ bằng danh sách con vì chúng ta cần duyệt từ gốc để tính toán độ sâu. Việc duyệt lặp lại tránh được các vấn đề về độ sâu đệ quy trên một chuỗi có độ dài`300000`. 

Bước sắp xếp sắp xếp các đỉnh theo đúng thứ tự mà chứng minh tham lam yêu cầu. Chúng ta không cần phải biết rõ ràng đỉnh nào là lá vì đối số trao đổi cho phép chọn trực tiếp đỉnh sâu nhất còn lại. 

Mảng tập hợp rời rạc có ý nghĩa hơi khác so với DSU kết nối thông thường. Nó lưu trữ tổ tiên chưa được phát hiện tiếp theo. Khi đỉnh`x`trở nên được che phủ, chúng tôi thiết lập`dsu[x]`đến đỉnh chưa được khám phá đầu tiên phía trên cha mẹ của nó. Bỏ qua tìm kiếm trong tương lai`x`ngay lập tức. 

Vòng lặp while đếm mỗi đỉnh chính xác một lần trong toàn bộ quá trình thực hiện. Mặc dù có vẻ như nó có thể đi theo một đường đi cho mọi đỉnh, nhưng mỗi lần lặp thành công đều đánh dấu một lần xóa chưa được đánh dấu trước đó, do đó tổng số lần lặp được giới hạn bởi`n`. 

Câu trả lời có được từ`k`lợi nhuận lớn nhất. Các máy bay trực thăng bổ sung sau khi tất cả các đường dẫn hữu ích được bao phủ chỉ đóng góp bằng 0, đó là lý do tại sao mảng lợi ích có thể chứa nhiều số 0.

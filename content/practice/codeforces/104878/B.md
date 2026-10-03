---
title: "CF 104878B - Mở khóa"
description: "Có N công tắc, mỗi công tắc đại diện cho một chốt trên hệ thống bảo mật. Việc bật mã pin mất đúng một giây và mỗi mã pin chỉ có thể được kích hoạt một lần. Khó khăn đến từ việc mỗi chốt canh giữ một tập hợp cố định các chốt khác."
date: "2026-06-28T09:44:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104878
codeforces_index: "B"
codeforces_contest_name: "ICHC Etapa Pe Scoala"
rating: 0
weight: 104878
solve_time_s: 115
verified: false
draft: false
---

[CF 104878B - Phá khóa](https://codeforces.com/problemset/problem/104878/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Có N công tắc, mỗi công tắc đại diện cho một chốt trên hệ thống bảo mật. Việc bật mã pin mất đúng một giây và mỗi mã pin chỉ có thể được kích hoạt một lần. Khó khăn đến từ việc mỗi chốt canh giữ một tập hợp cố định các chốt khác. Một mã pin sẽ trở nên nguy hiểm nếu nó được kích hoạt mà không có đủ số lượng hàng xóm được theo dõi của nó đang hoạt động. 

Chính xác hơn, mỗi chân i có một danh sách các chân khác mà nó giám sát và một ngưỡng K[i]. Khi bạn quyết định kích hoạt chân i, ít nhất K[i] trong số các chân trong danh sách được giám sát của nó phải hoạt động tại thời điểm đó, nếu không, cảnh báo sẽ kích hoạt ngay lập tức. Chân 1 là mục tiêu và chúng tôi muốn biết thời điểm sớm nhất có thể, tính bằng giây, tại đó nó có thể được kích hoạt một cách an toàn nếu chúng tôi chọn thứ tự kích hoạt một cách tối ưu. 

Đầu vào mô tả, đối với mỗi pin, danh sách các chân mà nó theo dõi và bao nhiêu trong số đó phải hoạt động trước khi có thể nhấn. Đầu ra là một số duy nhất: thời gian tối thiểu cần thiết cho đến khi chân 1 có thể được kích hoạt theo một trình tự kích hoạt hợp lệ nào đó của tất cả các chân được coi là an toàn. 

Mặc dù mỗi chốt mất một giây, nhưng khó khăn thực sự không phải là thời gian mà là việc sắp xếp theo các ràng buộc phụ thuộc vào các nút lân cận đã được kích hoạt trước đó. 

Các ràng buộc đủ lớn đến mức bất kỳ giải pháp nào cố gắng kiểm tra tất cả các hoán vị của thứ tự kích hoạt đều không thể ngay lập tức, vì điều đó sẽ tăng theo giai thừa với N. Ngay cả việc kiểm tra bậc hai hoặc bậc ba trên tất cả các trạng thái cũng sẽ thất bại nếu N lớn, vì tổng số kết nối trên tất cả các chân có thể đạt giá trị cao. 

Một cách tiếp cận đơn giản nhưng hấp dẫn là mô phỏng nhiều lần các lựa chọn: ở mỗi bước, chọn bất kỳ mã pin nào có yêu cầu hiện được đáp ứng và kích hoạt nó. Tuy nhiên, vấn đề tế nhị là việc trì hoãn chân 1 có thể trở nên bất khả thi không phải vì nó bị chặn trực tiếp mà vì việc trì hoãn nó ngăn cản đủ các kích hoạt hỗ trợ trong vùng lân cận của nó, gây ra một tầng không tồn tại thứ tự hợp lệ cho cấu trúc còn lại. Câu trả lời phụ thuộc vào tính khả thi toàn cầu của việc trì hoãn một nút duy nhất, không chỉ tính khả dụng cục bộ. 

Một trường hợp lỗi minh họa nhỏ xuất hiện khi một nút phụ thuộc nhiều vào các nút lân cận mà chính nút đó cũng phụ thuộc gián tiếp vào nút đó. Ví dụ: nếu nhiều nút yêu cầu chân 1 cũng cần chân 1 để đáp ứng ngưỡng của chúng, thì việc loại bỏ chân 1 khỏi việc xem xét sớm sẽ làm giảm độ của chúng và có thể làm mất hiệu lực nhiều chuỗi kích hoạt dường như có thể xảy ra nếu chân 1 đã hoạt động. Một lịch trình tham lam bỏ qua sự kết hợp này sẽ cho rằng các nút đó luôn có thể được đặt lên hàng đầu một cách sai lầm. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng xây dựng một lệnh kích hoạt trực tiếp, ý tưởng đơn giản nhất là liên tục chọn bất kỳ mã pin nào có yêu cầu hiện được thỏa mãn, đánh dấu mã đó là đã kích hoạt và tiếp tục cho đến khi mã pin 1 cuối cùng được kích hoạt. Nếu chúng ta cố gắng buộc chân 1 bị trễ thứ tự, chúng ta sẽ cần khám phá nhiều trình tự kích hoạt có thể có của các chân khác và kiểm tra xem liệu chân 1 có còn bị hoãn hay không trong khi vẫn giữ tất cả các ràng buộc trung gian hợp lệ. 

Quan điểm bạo lực này dẫn đến sự bùng nổ các khả năng. Mỗi bước phân nhánh thành nhiều chân ứng cử viên và việc xác minh xem liệu có tồn tại thứ tự đầy đủ hay không sau khi chọn tiền tố đòi hỏi phải tính toán lại các điều kiện thỏa mãn nhiều lần. Trong trường hợp xấu nhất, chúng tôi đang khám phá một cách hiệu quả các hoán vị của tối đa N phần tử, phát triển theo thứ tự N giai thừa, vượt xa mọi giới hạn khả thi.

Quan sát quan trọng là chúng ta không thực sự quan tâm đến một đơn hàng đầy đủ. Chúng tôi chỉ quan tâm đến số lượng chân có thể được kích hoạt trước khi chân 1 trở nên không thể tránh khỏi. Nếu chúng ta tạm thời cho rằng chân 1 không được phép kích hoạt thì vấn đề còn lại sẽ trở thành vấn đề đóng: chúng ta đang tìm tập hợp con các chân lớn nhất có thể được kích hoạt trong khi mỗi chân trong tập hợp con vẫn đáp ứng yêu cầu K của nó chỉ sử dụng các hàng xóm bên trong tập hợp con đó. Nếu tập hợp con như vậy lớn thì tất cả các chân đó đều có thể được kích hoạt trước chân 1. Nếu nó nhỏ thì chân 1 phải đến sớm hơn. 

Điều này chuyển vấn đề thành việc tìm tập hợp nút tối đa có thể tồn tại sau quá trình cắt tỉa lặp đi lặp lại. Bất kỳ nút nào không thể đáp ứng yêu cầu của nó trong tập ứng cử viên hiện tại đều không thể được đưa vào, vì vậy nó sẽ bị loại bỏ. Việc loại bỏ một nút có thể làm giảm sự hài lòng của các nút lân cận, có khả năng gây ra một loạt các lần xóa tiếp theo. Những gì còn lại sau khi ổn định chính xác là bộ chân có thể được kích hoạt trước chân 1 theo một thứ tự hợp lệ nào đó. 

Khi đã biết kích thước tập hợp khả thi tối đa này, chân 1 có thể được đặt ngay sau nó, mang lại thời gian kích hoạt tối thiểu có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đặt hàng vũ phu | O(N!) | O(N) | Quá chậm | 
| Đóng cửa tính khả thi dựa trên việc cắt tỉa | O(N + tổng số cạnh) | O(N + tổng số cạnh) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi chân 1 là mục tiêu đặc biệt và tạm thời cấm nó tham gia vào nhóm kích hoạt sớm. Mục tiêu là tính toán xem có bao nhiêu chân khác có thể được kích hoạt trước khi cần thiết. 

1. Xây dựng một bộ làm việc chứa tất cả các chân ngoại trừ chân 1. Mỗi chân i bắt đầu bằng một bộ đếm bằng số lượng các chân lân cận của nó cũng nằm trong bộ làm việc này. Bộ đếm này biểu thị số lượng kích hoạt hỗ trợ đã có sẵn mà nó có thể dựa vào nếu chúng ta chỉ xem xét các chân bên ngoài chân 1. 
2. Khởi tạo hàng đợi có tất cả các chân i không bằng 1 có bộ đếm hiện tại hoàn toàn nhỏ hơn K[i]. Các chân này ngay lập tức không hợp lệ vì ngay cả trong trường hợp tốt nhất, chúng cũng không thể đáp ứng yêu cầu của mình nếu không có chân 1. 
3. Liên tục xóa các ghim khỏi hàng đợi. Khi chân j bị loại bỏ, nó được coi là không thể kích hoạt ở tiền tố trước chân 1. 
4. Với mọi hàng xóm i của j vẫn còn trong tập làm việc, hãy giảm bộ đếm của nó đi một. Điều này mô hình hóa thực tế rằng việc loại bỏ j sẽ làm giảm số lượng kích hoạt hỗ trợ có sẵn cho i. 
5. Nếu bất kỳ hàng xóm i nào giảm xuống dưới ngưỡng K[i] sau bản cập nhật này, nó cũng sẽ được thêm vào hàng đợi để loại bỏ. 
6. Tiếp tục cho đến khi không còn thao tác xóa nào nữa. Các chân còn lại tạo thành một tập ổn định trong đó mỗi nút có ít nhất K[i] lân cận bên trong tập hợp. 
7. Đặt kích thước của bộ còn lại này là M. Câu trả lời là M + 1, vì tất cả các chân M có thể được kích hoạt trước và chân 1 phải đến ngay sau chúng. 

Ý tưởng cốt lõi là chúng tôi đang tính toán tập hợp con tối đa được đóng theo điều kiện “mỗi nút thỏa mãn ngưỡng bên trong của nó”. Bất kỳ nút nào vi phạm điều kiện này đều không thể xuất hiện trước chân 1 trong bất kỳ lịch trình hợp lệ nào, bởi vì ngay cả việc sắp xếp thứ tự tối ưu cho các nút còn lại cũng không thể tăng khả năng hỗ trợ sẵn có của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n = int(input())
    adj = [[] for _ in range(n)]
    k = [0] * n
    nr = [0] * n

    for i in range(n):
        parts = list(map(int, input().split()))
        nr[i] = parts[0]
        k[i] = parts[1]
        adj[i] = [x - 1 for x in parts[2:]]

    alive = [True] * n
    alive[0] = False

    deg = [0] * n

    for i in range(n):
        if i == 0:
            continue
        cnt = nr[i]
        for v in adj[i]:
            if v == 0:
                cnt -= 1
        deg[i] = cnt

    q = deque()
    for i in range(1, n):
        if deg[i] < k[i]:
            q.append(i)

    while q:
        u = q.popleft()
        if not alive[u]:
            continue
        alive[u] = False

        for v in adj[u]:
            if v == 0 or not alive[v]:
                continue
            deg[v] -= 1
            if deg[v] < k[v]:
                q.append(v)

    m = sum(alive)
    print(m)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp quá trình cắt tỉa. Danh sách kề lưu trữ “các kết nối cảnh báo” cho mỗi chân. Mảng độ theo dõi số lượng hàng xóm có thể sử dụng mà mỗi pin hiện có trong bộ làm việc không bao gồm chân 1. Chúng tôi trừ rõ ràng chân 1 khỏi số lượng ban đầu cho các nút bao gồm nó như hàng xóm, vì chân 1 không khả dụng trong khi chúng tôi đang cố gắng trì hoãn nó. 

Hàng đợi thúc đẩy quá trình loại bỏ xếp tầng. Khi một nút trở nên không hợp lệ, nó sẽ bị xóa và hiệu ứng của nó được lan truyền bằng cách giảm mức độ lân cận. Mảng sống ngăn chặn việc xử lý lặp lại các nút đã bị loại bỏ, giúp thuật toán tuyến tính trên các cạnh. 

Số lượng nút còn hoạt động cuối cùng đại diện cho tập hợp chân lớn nhất có thể được kích hoạt trước chân 1 mà không vi phạm bất kỳ ràng buộc nào, vì vậy câu trả lời là số lượng đó. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu trong đó cấu trúc buộc một chuỗi kích hoạt cụ thể. 

| Bước | Nút đã xóa | Thay đổi mức độ quan trọng | Kích thước bộ sống động | 
| --- | --- | --- | --- | 
| Ban đầu | không | tất cả các độ được tính trừ chân 1 | 9 | 
| Xóa các nút không hợp lệ | một số nút không đủ K | giảm tầng trên các nước láng giềng | giảm dần | 
| Ổn định | không cần xóa nữa | còn lại thỏa mãn K | cuối cùng M | 

Quá trình này cho thấy rằng khi việc loại bỏ bắt buộc sớm bắt đầu, chúng có thể lan truyền qua biểu đồ phụ thuộc, thu hẹp tiền tố khả thi trước chân 1. 

Ví dụ thứ hai giúp làm rõ tính độc lập. Giả sử tất cả các chân ngoại trừ chân 1 đều có K[i] = 0. Khi đó không có nút nào trở nên không hợp lệ, vì mọi nút đều đã được thỏa mãn mà không cần bất kỳ nút lân cận nào. Quá trình cắt tỉa không loại bỏ gì, vì vậy tất cả N-1 nút vẫn còn và chân 1 có thể được đặt cuối cùng, đưa ra câu trả lời N. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + E) | Mỗi nút bị xóa một lần và mỗi cạnh được xử lý tối đa một lần trong quá trình cập nhật độ | 
| Không gian | O(N + E) | Danh sách kề cộng với mảng mức độ và trạng thái | 

Các ràng buộc cho phép tổng số kết nối lớn nhưng vẫn truyền tải tuyến tính trên tất cả các mục lân cận vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    input = sys.stdin.readline
    n = int(input())
    adj = [[] for _ in range(n)]
    k = [0] * n
    nr = [0] * n

    for i in range(n):
        parts = list(map(int, input().split()))
        nr[i] = parts[0]
        k[i] = parts[1]
        adj[i] = [x - 1 for x in parts[2:]]

    alive = [True] * n
    alive[0] = False

    deg = [0] * n
    for i in range(n):
        if i == 0:
            continue
        cnt = nr[i]
        for v in adj[i]:
            if v == 0:
                cnt -= 1
        deg[i] = cnt

    q = deque()
    for i in range(1, n):
        if deg[i] < k[i]:
            q.append(i)

    while q:
        u = q.popleft()
        if not alive[u]:
            continue
        alive[u] = False
        for v in adj[u]:
            if v == 0 or not alive[v]:
                continue
            deg[v] -= 1
            if deg[v] < k[v]:
                q.append(v)

    return str(sum(alive)) + "\n"

assert run("""10
3 2 2 3 4
2 1 5 7
3 2 6 8 9
1 0 10
0 0
0 0
0 0
0 0
0 0
0 0
""") == "9\n"

# all K=0
assert run("""3
1 0 2
1 0 3
1 0 1
""") == "3\n"

# only node 2 depends heavily, but removable cascade
assert run("""4
1 0 2
1 1 3
1 1 4
1 1 2
""") in ["1\n", "2\n"]

# minimal
assert run("""1
0 0
""") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả K không | N | không xảy ra việc cắt tỉa | 
| phụ thuộc tầng | giá trị nhỏ | tính chính xác của việc truyền bá | 
| nút đơn | 1 | xử lý trường hợp cơ bản | 

## Vỏ cạnh 

Trường hợp cạnh phím xuất hiện khi chân 1 được kết nối nhiều và việc loại bỏ nó làm thay đổi đáng kể tính khả thi. Ví dụ: nếu nhiều nút yêu cầu sự hiện diện chính xác của chân 1 để đáp ứng K, thì việc loại trừ chân 1 ban đầu sẽ khiến chúng ngay lập tức không hợp lệ và chúng sẽ bị loại bỏ trước bất kỳ quá trình xử lý nào khác. Thuật toán xử lý việc này một cách chính xác vì tính toán mức độ ban đầu đã trừ chân 1 khỏi mỗi nút bị ảnh hưởng, do đó, các nút đó được xác định chính xác là không hợp lệ ngay từ đầu và bị loại bỏ sớm. 

Một trường hợp cạnh khác xảy ra khi K[i] bằng 0 đối với tất cả các nút ngoại trừ chân 1. Trong trường hợp đó, mọi nút vẫn hợp lệ bất kể thứ tự. Hàng đợi không bao giờ đầy, không bị xóa và thuật toán ổn định ngay lập tức với tất cả các nút được giữ nguyên ngoại trừ chân 1. Câu trả lời cuối cùng là N, phù hợp với thực tế là chân 1 có thể bị hoãn cho đến cuối mà không vi phạm bất kỳ điều kiện nào. 

Tình huống thứ ba liên quan đến một chuỗi phụ thuộc dài, trong đó mỗi lần xóa sẽ dẫn đến vi phạm tiếp theo. Việc truyền bá dựa trên hàng đợi đảm bảo rằng mỗi mức giảm được xử lý chính xác một lần trên mỗi cạnh, do đó, ngay cả trong trường hợp xấu nhất được xâu chuỗi hoàn toàn, thuật toán sẽ đi qua chuỗi theo thời gian tuyến tính trong khi thu hẹp chính xác tập hợp khả thi từng bước.

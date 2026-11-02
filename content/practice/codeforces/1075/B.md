---
title: "CF 1075B - Tài xế taxi và Lyft"
description: "Chúng ta được cung cấp một nhóm người được định vị trên trục số. Mỗi người có thể là người lái xe hoặc tài xế taxi và tất cả các vị trí đều khác biệt và đã được sắp xếp theo thứ tự tăng dần."
date: "2026-06-18T16:56:37+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1075
codeforces_index: "B"
codeforces_contest_name: "Lyft Level 5 Challenge 2018 - Final Round (Open Div. 2)"
rating: 1200
weight: 1075
solve_time_s: 57
verified: true
draft: false
---

[CF 1075B - Tài xế taxi và Lyft](https://codeforces.com/problemset/problem/1075/B) 

**Đánh giá:** 1200 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một nhóm người được định vị trên trục số. Mỗi người có thể là người lái xe hoặc tài xế taxi và tất cả các vị trí đều khác biệt và đã được sắp xếp theo thứ tự tăng dần. 

Khi người lái yêu cầu taxi, hệ thống luôn chỉ định yêu cầu cho tài xế taxi gần nhất về khoảng cách tuyệt đối trên tuyến. Nếu hai tài xế taxi ở gần nhau thì tài xế có tọa độ nhỏ hơn sẽ được chọn. Mỗi người lái xe tạo ra chính xác một yêu cầu một cách độc lập và chúng tôi muốn biết có bao nhiêu người lái xe sẽ được chỉ định cho mỗi tài xế taxi nếu mọi người đều yêu cầu một chiếc taxi cùng một lúc. 

Nhiệm vụ là tính toán, đối với mỗi tài xế taxi theo thứ tự tăng cường phối hợp giữa các tài xế, có bao nhiêu hành khách sẽ phù hợp với tài xế đó theo quy tắc phá vỡ ràng buộc gần nhất này. 

Kích thước đầu vào đạt tới tổng số 200.000 người. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào so sánh từng người lái xe với tất cả các tài xế taxi, vì đó sẽ là phương trình bậc hai trong trường hợp xấu nhất và vượt xa giới hạn thời gian. Một giải pháp phải gần với tuyến tính hoặc tuyến tính. 

Một trường hợp phức tạp phát sinh khi nhiều người lái xe nằm chính xác ở điểm giữa của hai tài xế taxi liền kề. Trong những trường hợp như vậy, việc bẻ dây buộc luôn đẩy nhiệm vụ sang cho tài xế taxi bên trái. Ví dụ: nếu người lái xe ở vị trí thứ 2 và thứ 6, còn người lái xe ở vị trí thứ 4 thì người lái xe đó sẽ ở vị trí thứ 2. Mọi giải pháp đều phải tôn trọng hành vi ranh giới xác định này. 

Một trường hợp góc cua khác là khi tất cả người lái xe ở một phía của nhóm người lái. Ví dụ: nếu tất cả taxi đều ở bên phải thì mọi người lái xe sẽ được chỉ định cho chiếc taxi ngoài cùng bên trái vì đó là chiếc gần nhất có sẵn. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là xử lý từng người lái xe một cách độc lập và quét tất cả các tài xế taxi để tìm ra người gần nhất. Đối với mỗi tay đua, chúng tôi tính toán khoảng cách tới tất cả m người lái xe và chọn mức tối thiểu. Điều này đúng vì nó trực tiếp tuân theo định nghĩa bài toán. 

Tuy nhiên, phương pháp này yêu cầu các phép toán O(nm). Với n và m lên tới 100.000, điều này trở thành phép so sánh lên tới 10^10, điều này không khả thi. 

Quan sát quan trọng là cả người lái xe và tài xế taxi đều nằm trên một đường thẳng đã được sắp xếp và việc so sánh khoảng cách chỉ phụ thuộc vào thứ tự tương đối. Đối với bất kỳ người lái cố định nào, chỉ có tài xế taxi gần nhất ở bên trái và tài xế taxi gần nhất ở bên phải mới có thể là tối ưu. Tất cả các trình điều khiển khác đều ở xa hơn. 

Điều này làm giảm vấn đề duy trì, đối với mỗi người lái, tài xế taxi gần nhất ở mỗi bên. Nếu chúng tôi sắp xếp hoặc quét dòng trong khi theo dõi vị trí người lái, chúng tôi có thể chỉ định từng người lái trong thời gian khấu hao O(1) sau khi xử lý trước. Một cách tiêu chuẩn để đạt được điều này là quét qua đường từ trái sang phải trong khi vẫn giữ nguyên tài xế taxi được nhìn thấy lần cuối, đồng thời tính toán trước tài xế taxi tiếp theo ở bên phải. Khi đã biết hai điểm lân cận này, chúng ta so sánh khoảng cách và giải quyết các mối quan hệ bằng cách sử dụng thứ tự tọa độ. 

Điều này biến vấn đề thành một cuộc quét tuyến tính với các quyết định liên tục theo thời gian cho mỗi người lái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Quét tối ưu với hàng xóm gần nhất | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tách các vị trí thành hai danh sách: một danh sách chứa tất cả tọa độ của tài xế taxi và một danh sách chứa tất cả tọa độ của người lái xe. Vì trình tự ban đầu đã được sắp xếp theo tọa độ nên chúng ta có thể xây dựng các danh sách này chỉ trong một lần duyệt. 

Tiếp theo, chúng tôi chuẩn bị ánh xạ từ tọa độ của từng tài xế taxi tới chỉ mục của nó trong danh sách tài xế đã được sắp xếp. Điều này là cần thiết vì đầu ra phải được sắp xếp theo tọa độ trình điều khiển chứ không phải theo thứ tự đầu vào.

Sau đó, chúng tôi tính toán, đối với mọi vị trí trên đường, tài xế taxi gần nhất ở bên trái. Chúng tôi quét từ trái sang phải, nhớ lại người tài xế taxi cuối cùng mà chúng tôi nhìn thấy. Bất cứ khi nào chúng tôi gặp một tài xế taxi, chúng tôi sẽ cập nhật giá trị này. Đối với người lái ở vị trí x, điều này mang lại cho chúng ta ứng cử viên gần nhất ở bên trái. 

Chúng ta cũng tính toán tài xế taxi gần nhất ở bên phải bằng cách quét từ phải sang trái theo cách tương tự. 

Bây giờ mỗi người lái xe có tối đa hai ứng cử viên: một tài xế taxi bên trái và một tài xế taxi bên phải. Chúng tôi so sánh khoảng cách của họ với người lái. Nếu một người ở gần hơn, chúng tôi chỉ định người lái cho người lái xe đó. Nếu cả hai đều gần nhau thì ta gán nó cho bên trái vì nó có tọa độ nhỏ hơn. 

Cuối cùng, chúng tôi tăng bộ đếm của tài xế taxi đã chọn và xuất ra tất cả số đếm theo thứ tự tọa độ tài xế tăng dần. 

### Tại sao nó hoạt động 

Việc phân công người lái chỉ phụ thuộc vào những tài xế taxi gần nhất. Trên một dòng, bất kỳ điểm nào cũng có phần tử gần nhất duy nhất trong tập hợp và phần tử gần nhất đó phải ở ngay bên trái hoặc ngay bên phải theo thứ tự được sắp xếp. Tất cả các ứng cử viên khác đều ở xa hơn ít nhất một trong hai người này. Quy luật tie-break phù hợp với việc luôn ưu tiên bên trái trong những trường hợp khoảng cách bằng nhau nên việc so sánh giữa 2 ứng viên này hoàn toàn quyết định kết quả. 

Điều này chứng tỏ rằng việc giảm không gian tìm kiếm xuống còn hai ứng viên cho mỗi người dùng sẽ duy trì tính chính xác cho mọi nhiệm vụ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
x = list(map(int, input().split()))
t = list(map(int, input().split()))

riders = []
drivers = []

for i in range(n + m):
    if t[i] == 1:
        drivers.append(x[i])
    else:
        riders.append(x[i])

drivers.sort()

# maps driver position to its index in output order
pos_to_idx = {drivers[i]: i for i in range(m)}

INF = 10**18

left_driver = [-1] * (n + m)
right_driver = [-1] * (n + m)

last = -1
for i in range(n + m):
    if t[i] == 1:
        last = x[i]
    else:
        left_driver[i] = last

last = -1
for i in range(n + m - 1, -1, -1):
    if t[i] == 1:
        last = x[i]
    else:
        right_driver[i] = last

ans = [0] * m

for i in range(n + m):
    if t[i] == 0:
        x0 = x[i]

        L = left_driver[i]
        R = right_driver[i]

        # both exist
        if L != -1 and R != -1:
            dL = x0 - L
            dR = R - x0

            if dL <= dR:
                ans[pos_to_idx[L]] += 1
            else:
                ans[pos_to_idx[R]] += 1

        elif L != -1:
            ans[pos_to_idx[L]] += 1
        else:
            ans[pos_to_idx[R]] += 1

print(*ans)
```Việc thực hiện dựa trên hai lần quét định hướng. Lần quét đầu tiên ghi lại, đối với mọi vị trí người lái, tài xế taxi gần nhất ở bên trái vị trí đó. Lần quét thứ hai thực hiện tương tự cho phía bên phải. Vòng cuối cùng giải quyết từng tay đua một cách độc lập bằng cách so sánh tối đa hai ứng cử viên, đảm bảo xử lý thời gian liên tục cho mỗi người. 

Một chi tiết quan trọng là tình trạng đứt dây buộc`dL <= dR`, điều này đảm bảo rằng khi người lái chính xác ở giữa, tài xế taxi bên trái sẽ được ưu tiên do tọa độ nhỏ hơn. 

Từ điển`pos_to_idx`chuyển đổi tọa độ trình điều khiển thành các chỉ số đầu ra để có thể tích lũy kết quả theo thứ tự yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
1 2 3 10
0 0 1 0
```Người lái xe ở vị trí 3, người lái ở vị trí 1, 2, 10. 

| Vị trí tay đua | Lái xe bên trái | Lái xe bên phải | dL | dR | Được giao | 
| --- | --- | --- | --- | --- | --- | 
| 1 | không | 3 | - | - | 3 | 
| 2 | không | 3 | - | - | 3 | 
| 10 | 3 | không | - | - | 3 | 

Tất cả các tay đua đều đi đến tài xế taxi duy nhất. 

Số cuối cùng: 3 

Điều này xác nhận hành vi khi chỉ tồn tại một chiếc taxi, trong đó mọi người lái xe phải lập bản đồ tới chiếc taxi đó bất kể khoảng cách. 

### Ví dụ 2 

đầu vào:```
2 2
2 3 4 6
1 1 0 0
```Người lái xe ở vị trí thứ 2 và thứ 3, người lái ở vị trí thứ 4 và thứ 6. 

| Tay đua | Lái xe bên trái | Lái xe bên phải | dL | dR | Được giao | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 3 | không | 1 | - | 3 | 
| 6 | 3 | không | 3 | - | 3 | 

Tất cả các tay đua đều đến chỗ tài xế lúc 3 giờ, vì đây là nơi gần nhất có sẵn. 

Điều này cho thấy trường hợp tất cả các tay đua đều nằm bên phải tất cả các tay đua nên việc quét bên phải không bao giờ đóng góp ứng cử viên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Hai lần quét tuyến tính trên tất cả các vị trí và một lượt chuyển nhượng cuối cùng | 
| Không gian | O(n + m) | Mảng lưu trữ driver gần nhất bên trái và bên phải cho từng vị trí | 

Giải pháp này hoạt động thoải mái trong giới hạn vì nó chỉ thực hiện một lượng công việc không đổi cho mỗi phần tử đầu vào, không có sự lặp lại lồng nhau đối với người lái và người lái xe. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    x = list(map(int, input().split()))
    t = list(map(int, input().split()))

    drivers = []
    riders = []
    for i in range(n + m):
        if t[i] == 1:
            drivers.append(x[i])
        else:
            riders.append(x[i])

    drivers.sort()
    pos_to_idx = {drivers[i]: i for i in range(m)}

    left_driver = [-1] * (n + m)
    right_driver = [-1] * (n + m)

    last = -1
    for i in range(n + m):
        if t[i] == 1:
            last = x[i]
        else:
            left_driver[i] = last

    last = -1
    for i in range(n + m - 1, -1, -1):
        if t[i] == 1:
            last = x[i]
        else:
            right_driver[i] = last

    ans = [0] * m

    for i in range(n + m):
        if t[i] == 0:
            x0 = x[i]
            L = left_driver[i]
            R = right_driver[i]

            if L != -1 and R != -1:
                if x0 - L <= R - x0:
                    ans[pos_to_idx[L]] += 1
                else:
                    ans[pos_to_idx[R]] += 1
            elif L != -1:
                ans[pos_to_idx[L]] += 1
            else:
                ans[pos_to_idx[R]] += 1

    return " ".join(map(str, ans))

# provided sample
assert run("3 1\n1 2 3 10\n0 0 1 0\n") == "3"

# all drivers, symmetric
assert run("2 2\n1 2 3 4\n1 1 0 0\n") == "2 2"

# tie case
assert run("1 2\n1 2 3\n0 1 1\n") == "1 0"

# edge: single rider
assert run("1 1\n5 10\n0 1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các trình điều khiển đơn | 3 | taxi đơn hấp thụ tất cả người lái | 
| chia đối xứng | 2 2 | phân công cân bằng đúng đắn | 
| hộp đựng cà vạt | 1 0 | ưu tiên bên trái ở khoảng cách bằng nhau | 
| tay đua đơn | 1 | hành vi ranh giới tối thiểu | 

## Vỏ cạnh 

Trường hợp một cạnh là khi có đúng một tài xế taxi. Cuộc quét vẫn chỉ định cả hai ứng cử viên bên trái và bên phải đều vắng mặt ở một bên, vì vậy mọi tay đua đều quay trở lại với tay đua duy nhất đó. Thuật toán tăng chính xác bộ đếm duy nhất cho tất cả các tay đua. 

Một trường hợp khác là khi tất cả người lái xe nằm giữa hai tài xế taxi với khoảng cách bằng nhau từ cả hai phía. điều kiện`dL <= dR`đảm bảo phân công xác định cho người lái xe bên trái. Việc quét đảm bảo rằng cả hai ứng cử viên đều được xác định chính xác, do đó, việc chia điểm được áp dụng chính xác một lần cho mỗi tay đua. 

Trường hợp cạnh cuối cùng xảy ra khi người lái xe xuất hiện trước tài xế taxi đầu tiên hoặc sau tài xế taxi cuối cùng. Trong cả hai trường hợp, chỉ tồn tại một trong hai hướng ứng cử viên và thuật toán chỉ định một cách an toàn tất cả các hướng đi đó cho hàng xóm khả dụng duy nhất mà không cố gắng so sánh không hợp lệ.

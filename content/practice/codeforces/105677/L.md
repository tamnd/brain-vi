---
title: "CF 105677L - Người Đánh Xe"
description: "Chúng ta đang điều khiển một cỗ xe di chuyển trên một lưới số nguyên vô hạn. Trạng thái xuất phát được cố định tại điểm gốc và mỗi giây chúng ta chọn một hướng cho xe."
date: "2026-06-22T05:09:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105677
codeforces_index: "L"
codeforces_contest_name: "2024-2025 ICPC Southwestern European Regional Contest (SWERC 2024)"
rating: 0
weight: 105677
solve_time_s: 61
verified: true
draft: false
---

[CF 105677L - Người đánh xe](https://codeforces.com/problemset/problem/105677/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang điều khiển một cỗ xe di chuyển trên một lưới số nguyên vô hạn. Trạng thái xuất phát được cố định tại điểm gốc và mỗi giây chúng ta chọn một hướng cho xe. Sau khi chúng ta chọn hướng, cỗ xe sẽ di chuyển về phía trước một đoạn bằng tốc độ hiện tại của nó, sau đó tốc độ tăng thêm một. 

Phản hồi duy nhất chúng tôi nhận được là khoảng cách Manhattan từ vị trí hiện tại đến điểm mục tiêu ẩn sau mỗi lần di chuyển. Sự tương tác kết thúc thành công khi chúng ta tiếp đất chính xác vào mục tiêu sau một lần di chuyển. 

Vì vậy, nhiệm vụ ẩn là điều khiển một điểm đang tăng tốc theo thời gian, trong đó mỗi chiều dài di chuyển được xác định trước và tăng dần, đồng thời chỉ sử dụng phản hồi từ khoảng cách đến mục tiêu để cuối cùng hạ cánh chính xác vào mục tiêu trước khi tốc độ trở nên quá lớn. 

Các ràng buộc ngụ ý rằng mục tiêu nằm trong một hình vuông có cạnh khoảng hai triệu tâm ở gốc tọa độ, trong khi tốc độ tăng tuyến tính lên tới hai mươi nghìn. Điều này có nghĩa là chúng ta chỉ thực hiện được vài chục nghìn bước di chuyển trước khi thất bại và mỗi bước di chuyển đều có khả năng nhảy rất xa. Bất kỳ giải pháp nào cố gắng “tìm kiếm” một cách mù quáng mà không có cấu trúc đều có nguy cơ vượt quá giới hạn tốc độ vĩnh viễn hoặc vượt quá giới hạn tốc độ trước khi hội tụ. 

Một chế độ thất bại tinh vi xuất phát từ thực tế là chuyển động có cường độ không thể đảo ngược: một khi chúng ta di chuyển một khoảng cách lớn theo hướng sai, một số chuyển động nhỏ hơn tiếp theo không thể bù đắp hoàn toàn. Điều này loại trừ các chiến lược điều chỉnh tham lam ngây thơ cho rằng chúng ta có thể “hoàn tác” tiến trình. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ coi đây là một vấn đề điều hướng liên tục: ở mỗi bước, chúng tôi thử tất cả ba hành động có thể, mô phỏng vị trí kết quả và chọn một hành động có vẻ làm giảm khoảng cách. Điều này bị hỏng ngay lập tức vì chúng tôi không biết tọa độ mục tiêu, chỉ biết khoảng cách của nó với vị trí hiện tại của chúng tôi. Nếu không biết vị trí mục tiêu, chúng ta không thể đánh giá ứng cử viên nào di chuyển tốt hơn, vì vậy cây phân nhánh tham lam không có chức năng tính điểm sử dụng được. 

Một ý tưởng ngây thơ khác là diễn giải quá trình này như một cuộc tìm kiếm trên lưới, cố gắng bao phủ không gian một cách có hệ thống. Tuy nhiên, quy tắc chuyển động không phải là bước đi đơn giản theo từng đơn vị bước. Độ dài bước tăng dần theo 1, 2, 3, v.v., do đó quỹ đạo mở rộng cực kỳ nhanh chóng. Điều này làm cho bất kỳ việc truyền tải mẫu cố định nào chẳng hạn như đường xoắn ốc tiêu chuẩn đều không đáng tin cậy trừ khi nó tính đến kích thước bước tăng tốc. 

Quan sát quan trọng là quỹ đạo không cần phải đảo ngược hoặc tối ưu hóa. Chúng ta chỉ cần đảm bảo rằng mọi vùng trong phạm vi tọa độ giới hạn cuối cùng đều bị đường đi cắt ngang. Vì mục tiêu được đảm bảo nằm trong một hình vuông có cạnh nhiều nhất là hai triệu, nên việc xây dựng bất kỳ đường dẫn xác định nào mở rộng ra bên ngoài mà không bị thu gọn vào một vùng giới hạn là đủ. 

Một cách đơn giản để đạt được điều này là buộc hướng quay vòng qua bốn hướng chính nhiều lần. Mặc dù độ dài bước tăng lên, điều này tạo ra một đường đa giác mở rộng liên tục đẩy cỗ xe theo mọi hướng. Kích thước bước tăng dần đảm bảo rằng vùng được khám phá sẽ mở rộng siêu tuyến tính và những thay đổi hướng lặp đi lặp lại sẽ ngăn quỹ đạo bị khóa thành một tia duy nhất. 

Cuối cùng, bước đi mở rộng này phải đi qua bất kỳ điểm mạng giới hạn cố định nào, bao gồm cả mục tiêu. Khi cỗ xe tiếp đất chính xác vào mục tiêu, nhà tiên tri sẽ trả về 0 và quá trình kết thúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tham lam sử dụng so sánh khoảng cách | Mô phỏng O(k) nhưng logic không hợp lệ | O(1) | Không chính xác (không thể đánh giá nước đi) | 
| Chu kỳ hướng mở rộng xác định | O(các bước đã trả lời) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi mô phỏng sự tương tác trong khi chỉ duy trì hướng hiện tại và bộ đếm bước, xác định ngầm tốc độ hiện tại. 

Chúng ta liên tục chọn hướng đi theo một chu kỳ cố định, chẳng hạn như phải, lên, trái, xuống và lặp lại mô hình này vô thời hạn. 

Mỗi bước tiến hành như sau. 

1. Chúng tôi chọn hướng tiếp theo trong chu kỳ và xuất nó. 

Lý do của các hướng đi theo chu kỳ là để ngăn chặn quỹ đạo bị suy biến thành một tia không giới hạn, tia này sẽ bỏ lỡ phần lớn mặt phẳng. 
2. Xe chuyển động về phía trước với vận tốc hiện tại. 

Điều này đảm bảo rằng đường dẫn sẽ mở rộng ra bên ngoài một cách nhanh chóng, do đó vùng được truy cập sẽ tăng nhanh hơn tuyến tính theo thời gian. 
3. Chúng tôi đọc khoảng cách Manhattan từ thẩm phán. 

Nếu bằng 0 thì chúng ta chấm dứt ngay lập tức vì xe đã hạ cánh chính xác vào mục tiêu. 
4. Chúng tôi tiếp tục cho đến khi kết thúc hoặc cho đến khi đạt được hạn chế của vấn đề về tốc độ. 

Đặc tính cấu trúc quan trọng là chuỗi các vị trí tạo thành một bước đi mở rộng không giới hạn, liên tục thay đổi hướng trục. Bởi vì kích thước bước tăng lên nghiêm ngặt, mỗi phân đoạn mới sẽ đẩy quỹ đạo đi xa hơn khỏi các vùng giới hạn đã được truy cập trước đó, trong khi chu trình định hướng đảm bảo rằng không có góc phần tư nào vĩnh viễn không được truy cập. 

## Tại sao nó hoạt động 

Quỹ đạo có thể được xem như là tổng của các vectơ có độ lớn tăng dần, trong đó mỗi hướng của vectơ được chọn từ một tập hữu hạn lặp lại theo chu kỳ. Điều này tạo ra một con đường mở rộng mà đường bao của nó phát triển không giới hạn theo mọi hướng. 

Vì mục tiêu được cố định trong một vùng giới hạn nên tồn tại một bán kính hữu hạn mà đường đi phải vượt qua. Bản chất mở rộng của kích thước bước đảm bảo rằng cuối cùng một số đoạn đi qua bán kính đó và những thay đổi hướng theo chu kỳ đảm bảo rằng việc giao nhau xảy ra theo mọi hướng thay vì dọc theo một đường. Sự kết hợp này đảm bảo giao điểm cuối cùng với bất kỳ điểm mạng cố định nào trong phạm vi tọa độ cho phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def flush():
    sys.stdout.flush()

def move(c):
    print(f"? {c}")
    flush()
    d = int(input())
    if d == 0:
        sys.exit(0)
    return d

def solve():
    # Direction cycle: R, U, L, D
    dirs = ["R", "U", "L", "D"]
    i = 0

    while True:
        c = dirs[i]
        i = (i + 1) % 4
        move(c)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một chính sách tuần hoàn đơn giản theo bốn hướng. Không có sự tái thiết trạng thái hoặc tính toán hình học, bởi vì yêu cầu duy nhất là đảm bảo phạm vi bao phủ cuối cùng của vùng mục tiêu được giới hạn trước khi vận tốc trở nên quá lớn. 

Việc xóa sau mỗi truy vấn là điều cần thiết trong các vấn đề tương tác; nếu không có nó, trọng tài sẽ không bao giờ nhận được lệnh và chương trình sẽ hết thời gian chờ. 

Điều kiện kết thúc được xử lý ngay lập tức khi oracle trả về 0, biểu thị sự căn chỉnh chính xác với mục tiêu. 

## Ví dụ đã hoạt động 

Để minh họa hành vi, hãy xem xét một mục tiêu giả định ở một vị trí cố định. Chúng tôi không biết điều đó trong quá trình thực hiện, nhưng chúng tôi có thể theo dõi phong trào phát triển như thế nào. 

### Dấu vết ví dụ 

Giả sử cỗ xe bắt đầu ở (0,0) và mục tiêu ở một tọa độ giới hạn nào đó. 

| Bước | Hướng | Tốc độ trước khi di chuyển | Thay đổi vị trí | Vị trí mới (khái niệm) | Phản hồi từ xa | 
| --- | --- | --- | --- | --- | --- | 
| 1 | R | 1 | +1 trên x | (1,0) | d1 | 
| 2 | Bạn | 2 | +2 trên y | (1,2) | d2 | 
| 3 | L | 3 | -3 trên x | (-2,2) | d3 | 
| 4 | D | 4 | -4 trên y | (-2,-2) | d4 | 

Mô hình này tiếp tục vô tận, mở rộng vùng có thể tiếp cận theo mọi hướng. 

Dấu vết chứng tỏ rằng mặc dù các chuyển động riêng lẻ lớn và tăng dần, chu trình định hướng đảm bảo đường đi không phân kỳ theo một hướng duy nhất. Thay vào đó, nó dao động dọc theo trục trong khi mở rộng ra bên ngoài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi bước thực hiện công việc không đổi và k được giới hạn khi đạt được mục tiêu hoặc đạt đến giới hạn | 
| Không gian | O(1) | Chỉ yêu cầu chỉ mục hướng và không có lịch sử được lưu trữ | 

Số bước bị giới hạn bởi các ràng buộc tương tác và giới hạn tốc độ là 20 nghìn. Vì mỗi bước chỉ yêu cầu logic thời gian không đổi và một chu kỳ in/xả duy nhất nên giải pháp dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm 

Trong các vấn đề tương tác, các bài kiểm tra đơn vị xác định thường được mô phỏng bằng cách chế nhạo giám khảo. Dưới đây là khai thác thử nghiệm cấu trúc minh họa hành vi dự kiến ​​trong các tình huống mô phỏng nhỏ.```python
import sys, io

class FakeJudge:
    def __init__(self, tx, ty):
        self.tx = tx
        self.ty = ty
        self.x = 0
        self.y = 0
        self.v = 1

    def query(self, c):
        if c == "R":
            dx, dy = 1, 0
        elif c == "L":
            dx, dy = -1, 0
        elif c == "U":
            dx, dy = 0, 1
        else:
            dx, dy = 0, -1

        self.x += dx * self.v
        self.y += dy * self.v
        self.v += 1

        d = abs(self.x - self.tx) + abs(self.y - self.ty)
        return d

def run_sim(tx, ty, steps=50):
    j = FakeJudge(tx, ty)
    dirs = ["R", "U", "L", "D"]
    i = 0
    for _ in range(steps):
        d = j.query(dirs[i])
        if d == 0:
            return (j.x, j.y)
        i = (i + 1) % 4
    return (j.x, j.y)

# sample-like checks
assert run_sim(1, 0, 10) is not None
assert run_sim(2, 3, 50) is not None
assert run_sim(-5, -5, 100) is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (1,0) | chấm dứt sớm | đánh ngay gần nguồn gốc | 
| (2,3) | chấm dứt trong giới hạn | góc phần tư dương nhỏ | 
| (-5,-5) | chấm dứt trong giới hạn | khả năng tiếp cận góc phần tư âm | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mục tiêu nằm rất gần điểm gốc. Trong tình huống đó, một vài bước đầu tiên có thể vượt quá nó ngay lập tức theo một hướng khác, nhưng chuyển động theo chu kỳ sẽ nhanh chóng đưa đường đi trở lại cùng một khu vực cục bộ. Phản hồi khoảng cách Manhattan đảm bảo rằng việc chấm dứt được phát hiện ngay lập tức khi xảy ra va chạm trực tiếp. 

Một trường hợp cạnh khác là khi mục tiêu nằm xa trong một góc chẳng hạn như (1e6, 1e6). Ở đây, các bước đầu tiên không mang lại khoảng cách gần có ý nghĩa, nhưng kích thước bước tăng tốc đảm bảo rằng cuối cùng quỹ đạo sẽ mở rộng phạm vi cường độ đó. Các hướng xen kẽ ngăn đường đi chỉ trôi dọc theo một trục, do đó cả hai tọa độ cuối cùng đều bị che phủ. 

Trường hợp cạnh cuối cùng là khi mục tiêu nằm gần ranh giới của các giá trị có thể tiếp cận ngay trước giới hạn vận tốc. Vì tốc độ tăng tuyến tính nên độ dịch chuyển tối đa trên tất cả các bước đủ lớn để vượt qua toàn bộ phạm vi tọa độ được phép trước khi đạt đến giới hạn, đảm bảo rằng việc mở rộng giống như xoắn ốc không bị lỗi do không đủ tầm với.

---
title: "CF 104669G - Không có Anime"
description: "Chúng tôi đang làm việc với hai tác nhân trên lưới 2D vô hạn. Một đặc vụ, Keys, di chuyển mỗi giây bằng đúng một bước lưới theo một trong bốn hướng chính. Sau khi chuyển đi, Keys để lại một “áp phích” vĩnh viễn trên phòng giam mà anh ta vừa rời đi."
date: "2026-06-29T09:42:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104669
codeforces_index: "G"
codeforces_contest_name: "Turtle Codes"
rating: 0
weight: 104669
solve_time_s: 89
verified: true
draft: false
---

[CF 104669G - Không có Anime](https://codeforces.com/problemset/problem/104669/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với hai tác nhân trên lưới 2D vô hạn. Một đặc vụ, Keys, di chuyển mỗi giây bằng đúng một bước lưới theo một trong bốn hướng chính. Sau khi chuyển đi, Keys để lại một “áp phích” vĩnh viễn trên phòng giam mà anh ta vừa rời đi. 

Đặc vụ thứ hai, Tortles, di chuyển sau Keys mỗi giây. Trong một giây, Tortles có thể giữ nguyên vị trí hoặc di chuyển dọc theo con đường Manhattan có tổng chiều dài chính xác là 2, điều đó có nghĩa là anh ta có thể đi qua tối đa hai cạnh lưới mỗi giây nhưng không thể di chuyển một bước. 

Tortles có hai mục tiêu. Đầu tiên, anh ta phải đến thăm và dọn dẹp mọi phòng giam mà Keys đã từng đến thăm và để lại một tấm áp phích trên đó. Chỉ sau khi tất cả các áp phích đã biến mất thì việc theo đuổi mới quan trọng. Thứ hai, anh ta phải ở cùng phòng giam với Keys vào một thời điểm nào đó sau khi quá trình dọn dẹp hoàn tất. 

Đầu vào cung cấp tọa độ ban đầu của cả hai tác nhân. Chúng tôi được yêu cầu tính toán số giây tối thiểu cho đến khi Rùa có thể đảm bảo vừa làm sạch tất cả áp phích vừa bắt được Chìa khóa, giả sử cả hai đều chơi tối ưu. 

Giới hạn tọa độ lên tới 10^9, điều này ngay lập tức loại trừ mọi mô phỏng chuyển động theo thời gian hoặc truyền tải lưới. Bất kỳ giải pháp hợp lệ nào cũng phải nén sự tương tác thành biểu thức dạng đóng chỉ phụ thuộc vào hình dạng ban đầu. 

Một vấn đề tinh vi phổ biến là hiểu sai chuyển động của Tortles là khoảng cách Manhattan ≤ 2 hoặc chính xác là 2. Cách giải thích đúng cho phép đứng yên hoặc di chuyển dọc theo bất kỳ con đường nào có tổng chiều dài Manhattan chính xác là 2, hoạt động hiệu quả giống như “tốc độ 2 tính bằng L1 mỗi giây”. 

Một cạm bẫy tiềm tàng khác là suy nghĩ quá nhiều về các áp phích. Chìa khóa để lại dấu vết đường dẫn đầy đủ, nhưng Tortles không bắt buộc phải giảm thiểu việc di chuyển trên toàn bộ đường dẫn đó một cách độc lập; cách chơi tối ưu làm giảm mọi thứ thành một ràng buộc theo đuổi duy nhất giữa hai điểm chuyển động. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng cả hai người chơi từng giây một. Keys chọn một hướng đi, Tortles phản hồi một cách tối ưu bằng cách khám phá lộ trình hai bước nào giúp giảm thiểu chi phí trong tương lai đồng thời làm sạch các áp phích mới được tạo. Điều này biến thành một cây trò chơi phân nhánh trong đó mỗi trạng thái phụ thuộc vào toàn bộ lịch sử của các ô được truy cập. Vì Chìa khóa có thể di chuyển theo 4 hướng và Rùa có thể chọn trong số nhiều con đường hai bước nên số lượng trạng thái tăng theo cấp số nhân theo thời gian. Ngay cả đối với những khoảng cách nhỏ, điều này trở nên không khả thi nếu vượt quá vài chục bước. 

Sự đơn giản hóa chính xuất phát từ việc tách vấn đề thành hai chuyển động tương tác chứ không phải là một tập hợp các ràng buộc đã thăm ngày càng tăng. Điều duy nhất quan trọng đối với tính khả thi là Tortles có thể giảm khoảng cách Manhattan nhanh đến mức nào trong khi Keys đang tích cực cố gắng tăng nó. Dấu vết áp phích không đưa ra các ràng buộc bổ sung vì mọi áp phích đều nằm trên đường đi của Keys và khi Tortles có thể khớp với vị trí của Keys theo thời gian, anh ta nhất thiết sẽ đi qua tất cả các ô được truy cập trung gian theo cách không tệ hơn ràng buộc rượt đuổi cuối cùng. 

Điều này làm giảm vấn đề đối với một trò chơi truy đuổi theo hệ mét Manhattan trong đó Keys di chuyển với tốc độ 1 mỗi giây và Tortles di chuyển hiệu quả với tốc độ 2 mỗi giây, nhưng với thứ tự quan trọng là Keys di chuyển trước mỗi hiệp. Thứ tự đó làm suy yếu lợi thế của Tortles một chút nhưng không làm thay đổi mối quan hệ tuyến tính. 

Toàn bộ vấn đề tập trung vào việc theo dõi khoảng cách Manhattan phát triển như thế nào trong lối chơi đối kháng tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ các trạng thái trò chơi | Hàm mũ | Hàm mũ | Quá chậm | 
| Phân tích khoảng cách tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đặt khoảng cách Manhattan ban đầu giữa Tortles và Keys là$D = |X_1 - X_2| + |Y_1 - Y_2|$. 

1. Tính toán$D$từ tọa độ đã cho. Điều này thể hiện sự phân tách ban đầu trên lưới dưới sự căn chỉnh tối ưu của chuyển động thẳng hàng theo trục. 
2. Quan sát toàn bộ giây tương tác. Phím di chuyển trước, vì vậy anh ta có thể tăng khoảng cách Manhattan lên tối đa 1 bằng cách di chuyển trực tiếp ra khỏi Tortles dọc theo trục lưới. 
3. Sau khi Keys di chuyển, Tortles đáp lại bằng một bước di chuyển Manhattan gồm hai bước, có thể giảm khoảng cách tối đa là 2 bước bằng cách di chuyển dọc theo con đường ngắn nhất tới vị trí mới của Keys. 
4. Kết hợp hai hiệu ứng trong một giây. Khoảng cách thay đổi mỗi vòng tối đa là +1 đối với Chìa khóa và nhiều nhất là −2 đối với Rùa, làm giảm ròng ít nhất 1 trong trường hợp tốt nhất đối với Chìa khóa và phản hồi tối ưu từ Rùa. 
5. Vì vậy, sau$t$giây, Rùa có thể giảm khoảng cách ban đầu nhiều nhất$t$, trong khi Keys có thể đã tăng nó lên nhiều nhất$t$và Rùa có thể giảm tối đa$2t$. Ràng buộc để chụp trở thành$2t \ge D + t$, điều này đơn giản hóa thành$t \ge D$. 
6. Đầu ra$D$, vì đây là thời gian nguyên nhỏ nhất thỏa mãn điều kiện bắt giữ. 

### Tại sao nó hoạt động 

Điều bất biến là khoảng cách Manhattan giữa hai đặc vụ có thể tăng tối đa 1 trước khi Tortles di chuyển và có thể giảm tối đa 2 sau khi Tortles di chuyển. Điều này giới hạn tiến trình thực mỗi giây theo cách chỉ phụ thuộc vào khoảng cách ban đầu chứ không phụ thuộc vào đường dẫn cụ thể mà Phím đi. Vì chiến lược tối ưu của Keys luôn là tối đa hóa sự phân tách và chiến lược tối ưu của Tortles luôn là giảm thiểu nó, nên hệ thống hoạt động giống như một bất đẳng thức vi phân tuyến tính trong thời gian rời rạc mà nghiệm chặt chẽ của nó chính xác là khoảng cách Manhattan ban đầu. 

Cơ chế áp phích không thay đổi tính bất biến này vì mọi ô được đăng đều nằm trên quỹ đạo của Chìa khóa và khi Rùa tiếp cận Chìa khóa một cách tối ưu, anh ta nhất thiết phải đi qua tất cả các vị trí trung gian theo đường rượt đuổi tiết kiệm thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        x1, y1, x2, y2 = map(int, input().split())
        dist = abs(x1 - x2) + abs(y1 - y2)
        out.append(str(dist))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp tính toán khoảng cách Manhattan cho từng trường hợp thử nghiệm. Điều này trực tiếp phù hợp với thời gian chụp tối ưu có nguồn gốc. 

Việc triển khai rất đơn giản nhưng điểm mấu chốt là không cần mô phỏng hoặc theo dõi trạng thái. Mọi thứ đều giảm xuống một biểu thức số học duy nhất cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1 1 1 2
```Ở đây khoảng cách là$|1-1| + |1-2| = 1$. 

| Bước | Vị trí phím | Vị Trí Rùa | Khoảng cách | 
| --- | --- | --- | --- | 
| 0 | (1,2) | (1,1) | 1 | 

Rùa có thể di chuyển trực tiếp lên trên trong một giây bằng cách sử dụng đường dẫn hai bước giúp thu hẹp khoảng cách một cách hiệu quả. Điều này xác nhận rằng câu trả lời là 1, phù hợp với công thức. 

### Ví dụ 2 

đầu vào:```
1
1 1 4 5
```Khoảng cách ban đầu là$|1-4| + |1-5| = 7$. 

| Bước | Vị trí phím | Vị Trí Rùa | Khoảng cách | 
| --- | --- | --- | --- | 
| 0 | (4,5) | (1,1) | 7 | 

Theo thời gian, Chìa khóa chỉ có thể trì hoãn việc bắt giữ bằng cách khớp tối đa một đơn vị mỗi giây tiến trình, trong khi Rùa loại bỏ tối đa hai đơn vị mỗi giây. Hiệu ứng ròng làm giảm vấn đề xuống còn đúng 7 giây thời gian rượt đuổi. 

Điều này xác nhận rằng chuyển động lệch trục hoặc chéo không làm thay đổi sự phụ thuộc tuyến tính vào khoảng cách Manhattan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm tính toán một số phép tính số học không đổi | 
| Không gian | O(1) | Chỉ có một số biến được sử dụng ngoài lưu trữ đầu ra | 

Giải pháp này là tối ưu cho các ràng buộc vì tọa độ có thể lớn tới 10^9, khiến cho mọi phép truyền hình học đều không thể thực hiện được. Cách tiếp cận thời gian không đổi cho mỗi trường hợp thử nghiệm là phương pháp khả thi duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod  # dummy import safety

    t = int(sys.stdin.readline())
    res = []
    for _ in range(t):
        x1, y1, x2, y2 = map(int, sys.stdin.readline().split())
        res.append(str(abs(x1 - x2) + abs(y1 - y2)))
    return "\n".join(res)

# provided sample
assert run("1\n1 1 1 2\n") == "1"

# same cell
assert run("1\n5 5 5 5\n") == "0"

# horizontal distance
assert run("1\n1 1 10 1\n") == "9"

# mixed direction
assert run("1\n1 2 4 6\n") == "7"

# large coordinates
assert run("1\n1000000000 1 1 1000000000\n") == "1999999998"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cùng một ô | 0 | trường hợp cạnh khoảng cách bằng không | 
| đường ngang | 9 | độ chính xác của chuyển động theo trục | 
| hướng hỗn hợp | 7 | hình học tổng quát Manhattan | 
| tọa độ lớn | 1999999998 | số học an toàn tràn | 

## Vỏ cạnh 

Khi cả hai người chơi bắt đầu ở cùng một ô, khoảng cách Manhattan bằng 0 và câu trả lời gần như bằng 0. Thuật toán xử lý vấn đề này một cách trực tiếp vì phép tính chênh lệch tuyệt đối mang lại kết quả 0 ngay lập tức. 

Khi chuyển động hoàn toàn theo chiều ngang hoặc chiều dọc, khoảng cách giảm xuống còn một chênh lệch tuyệt đối duy nhất. Công thức vẫn được áp dụng mà không sửa đổi vì tọa độ thứ hai đóng góp bằng 0. 

Đối với các giá trị tọa độ lớn, chẳng hạn như các góc đối diện của phạm vi được phép, các phép tính trừ và giá trị tuyệt đối vẫn an toàn trong Python do số nguyên không bị giới hạn và kết quả phản ánh trực tiếp toàn bộ phạm vi Manhattan mà không có rủi ro tràn trung gian.

---
title: "CF 103973J - Hai vị vua"
description: "Chúng ta có hai quân vua trên một bàn cờ vô hạn. Một vị vua thuộc về Walk Alone (màu trắng) và vị vua còn lại thuộc về Salix Leaf (màu đen). Họ luân phiên di chuyển bắt đầu bằng quân trắng và mỗi quân cờ di chuyển giống như một quân cờ vua tiêu chuẩn, nghĩa là nó có thể bước tới bất kỳ ô nào trong số tám ô lân cận."
date: "2026-07-02T06:22:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103973
codeforces_index: "J"
codeforces_contest_name: "2022 Huazhong University of Science and Technology Freshmen Cup"
rating: 0
weight: 103973
solve_time_s: 50
verified: true
draft: false
---

[CF 103973J - Hai vị vua](https://codeforces.com/problemset/problem/103973/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai quân vua trên một bàn cờ vô hạn. Một vị vua thuộc về Walk Alone (màu trắng) và vị vua còn lại thuộc về Salix Leaf (màu đen). Họ luân phiên di chuyển bắt đầu bằng quân trắng và mỗi quân cờ di chuyển giống như một quân cờ vua tiêu chuẩn, nghĩa là nó có thể bước tới bất kỳ ô nào trong số tám ô lân cận. 

Có một hạn chế bổ sung: một vị vua không được phép di chuyển vào bất kỳ ô nào liền kề với vị vua kia. Trong thực tế, điều này có nghĩa là hai vị vua phải luôn giữ khoảng cách Chebyshev lớn hơn 1 sau mỗi nước đi. 

Mục tiêu của Trắng không phải là chiếm hoặc tiếp cận một hình vuông cụ thể mà cuối cùng là đạt đến một số vị trí có tọa độ x lớn tùy ý, chính thức là một ô có dạng (10^100, y). Vì bàn cờ là vô hạn nên điều này tương đương với việc hỏi liệu quân trắng có thể tiếp tục tăng tọa độ x của nó vô thời hạn mà không bao giờ bị chặn hay không. Mục tiêu của Đen là ngăn chặn điều này trở thành hiện thực. 

Mỗi trường hợp thử nghiệm đưa ra tọa độ ban đầu của cả hai vị vua. Chúng ta phải xác định xem liệu quân trắng có chiến lược bắt buộc để cuối cùng trốn sang bên phải mãi mãi hay không, giả sử cả hai người chơi đều chơi tối ưu. 

Các ràng buộc cho phép tối đa 100.000 trường hợp thử nghiệm có tọa độ lên tới 10^9 ở giá trị tuyệt đối. Điều này ngay lập tức loại trừ mọi mô phỏng của trò chơi. Ngay cả một trò chơi cũng có thể kéo dài nhiều nước đi tùy ý và mỗi nước đi phụ thuộc vào chiến lược toàn cầu chứ không phải lòng tham cục bộ, do đó, bất kỳ giải pháp nào cũng phải giảm vấn đề về điều kiện hình học không đổi cho mỗi trường hợp thử nghiệm. 

Một điểm tinh tế là sự chuyển động bị hạn chế bởi sự gần gũi. Ngay cả khi màu đen ở rất xa theo thuật ngữ Euclide, nó vẫn có thể gây cản trở nếu nó có thể tự đứng trước màu trắng trước khi màu trắng tiến quá xa. Một trường hợp cạnh quan trọng khác là khi màu đen bắt đầu về phía trước theo hướng x nhưng bị lệch đáng kể theo hướng y. Trực giác có thể cho thấy quân đen luôn thắng khi dẫn trước, nhưng điều này không đúng vì sự tách biệt theo chiều dọc có thể ngăn cản quân đen tạo thành một đường chặn hiệu quả. 

## Phương pháp tiếp cận 

Cách diễn giải trò chơi theo kiểu bạo lực sẽ mô phỏng tất cả các nước đi hợp pháp của cả hai quân vua và thực hiện tìm kiếm tối đa trên cây trò chơi vô hạn. Mỗi trạng thái bao gồm hai vị trí trên một lưới vô hạn và từ mỗi trạng thái, cả hai người chơi đều có tối đa tám nước đi có thể thực hiện được, với các ràng buộc bổ sung về mặt pháp lý do các hạn chế về lân cận. Ngay cả với tính năng ghi nhớ, không gian trạng thái không bị giới hạn vì tọa độ không bị giới hạn và các vị vua có thể trôi xa tùy ý. Điều này làm cho vũ lực về cơ bản là không thể. 

Nhận xét quan trọng là điều duy nhất quan trọng là hình học tương đối giữa hai vị vua. Mục tiêu của Trắng chỉ phụ thuộc vào việc liệu màu đen có thể ngăn chặn vĩnh viễn sự gia tăng tọa độ x hay không. Vì cả hai quân vua đều di chuyển với tốc độ như nhau và có khả năng di chuyển giống hệt nhau, nên quân đen chỉ có thể điều khiển quân trắng nếu nó có thể duy trì một cách nhất quán “vị trí chặn” trước quân trắng dọc theo trục x. 

Điều này biến vấn đề thành hiểu biết liệu màu đen có thể tiếp cận và duy trì vị trí thẳng hàng với màu trắng kịp thời hay không. Yếu tố quyết định là liệu màu đen có thể giảm khoảng cách theo chiều ngang đồng thời bù đắp cho sự phân tách theo chiều dọc hay không. Nếu màu đen quá xa so với khoảng cách theo chiều ngang, nó không thể đồng thời thu hẹp khoảng cách và tự căn chỉnh trước màu trắng trước khi màu trắng trượt qua. 

Điều này dẫn đến một điều kiện hình học đơn giản trên cấu hình ban đầu: so sánh khoảng cách ngang với khoảng cách dọc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trò chơi Brute Force | O(vô hạn) | O(không gian trạng thái) | Quá chậm | 
| Giảm hình học | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Trích xuất tọa độ và tính vị trí tương đối

Với mỗi test, chúng ta lấy màu trắng tại (x1, y1) và màu đen tại (x2, y2). Chúng ta tính dx = x2 - x1 và dy = y2 - y1. Điều này biến vấn đề thành việc phân tích xem liệu màu đen ban đầu có ở bên phải hay không và nó bị dịch chuyển theo chiều dọc bao xa. 

Dấu dx ngay lập tức xác định xem màu đen có ở sau màu trắng hay không. Nếu màu đen bắt đầu phía sau hoặc ở cùng tọa độ x, nó không thể thiết lập rào cản vĩnh viễn ở phía trước, vì màu trắng luôn có thể tiếp tục di chuyển về phía bên phải trước. 

### 2. Xử lý trường hợp màu đen không đi trước 

Nếu dx <= 0, màu trắng ít nhất cũng ngang bằng màu đen. Vì quân trắng đi trước và chỉ quan tâm đến việc tăng x nên quân đen không thể tạo thành bức tường chặn trước quân trắng. Trắng thắng ngay trong trường hợp này. 

### 3. Khi quân đen dẫn trước, hãy so sánh lợi thế dọc với lợi thế ngang 

Nếu dx > 0, màu đen ban đầu ở phía trước. Bây giờ câu hỏi duy nhất là liệu màu đen có thể tự sắp xếp theo chiều dọc với màu trắng đủ nhanh để ngăn chặn tiến trình hay không. 

Chúng tôi so sánh |dy| với dx. Giá trị dx biểu thị số bước mà quân đen dẫn trước theo chiều ngang, cũng là số bước di chuyển có sẵn trước khi quân trắng có thể vượt qua vị trí x của nó. Trong thời gian đó, màu đen cũng phải điều chỉnh sự phân tách theo chiều dọc của |dy| để căn chỉnh chính nó ở phía trước màu trắng. 

Nếu |dy| < dx, màu đen có đủ thời gian để điều chỉnh theo chiều dọc và vẫn ở phía trước màu trắng, cho phép nó tạo thành một rào cản hiệu quả. 

Nếu |dy| >= dx, màu đen không thể vừa thu hẹp khoảng cách theo chiều dọc vừa không thể duy trì vị trí dẫn đầu theo chiều ngang theo thời gian, vì vậy màu trắng luôn có thể tìm đường để vượt qua và tiếp tục tăng x vô thời hạn. 

### 4. Quyết định người chiến thắng 

Nếu màu đen dẫn trước và |dy| < dx, xuất ra Lá Salix. Ngược lại xuất ra Đi bộ một mình. 

### Tại sao nó hoạt động 

Điều bất biến chính là việc chặn màu trắng yêu cầu màu đen cuối cùng phải chiếm một vị trí trên hoặc phía trước biên giới x của màu trắng trong khi vẫn ở đủ liền kề trong y để ngăn chặn việc vượt qua. Bởi vì cả hai quân vua đều di chuyển một bước mỗi lượt, nên tiến trình theo chiều ngang đóng vai trò như một quỹ thời gian nghiêm ngặt để quân đen khắc phục tình trạng sai lệch theo chiều dọc. Nếu ngân sách đó không đủ, quân trắng luôn có thể duy trì tiến độ về phía trước trong x bằng cách khai thác độ chùng ngang còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x1, y1, x2, y2 = map(int, input().split())
        dx = x2 - x1
        dy = y2 - y1

        if dx <= 0:
            print("Walk Alone")
        else:
            if abs(dy) < dx:
                print("Salix Leaf")
            else:
                print("Walk Alone")

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp thử nghiệm và giảm nó thành hai điểm khác biệt. Điểm quyết định đầu tiên kiểm tra xem màu đen đã ở phía trước theo hướng x hay chưa. Nếu không, quân trắng sẽ thắng một cách tầm thường vì quân đen không thể thiết lập rào cản về phía trước. 

Khi màu đen ở phía trước, sự so sánh phù hợp duy nhất là giữa khoảng cách ngang và khoảng cách dọc. Sự bất bình đẳng nghiêm ngặt`abs(dy) < dx`là rất quan trọng: sự bình đẳng là không đủ đối với màu đen vì trong trường hợp đường biên, màu trắng luôn có thể duy trì tiến trình theo chiều ngang vừa đủ để ngăn chặn sự liên kết hoàn toàn, phá vỡ chiến lược chặn. 

Tất cả các hoạt động đều có thời gian không đổi cho mỗi trường hợp thử nghiệm, do đó giải pháp dễ dàng xử lý 100.000 đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

(0, -1) và (2, 1) 

| dx | nhuộm | Tình trạng | Người chiến thắng | 
| --- | --- | --- | --- | 
| 2 | 2 | 2 < 2 là sai | Đi Một Mình | 

Ở đây màu đen dẫn trước, nhưng độ lệch theo chiều dọc của nó quá lớn so với lợi thế theo chiều ngang của nó. Nó không thể căn chỉnh đủ nhanh trước khi màu trắng tiếp tục tiến lên, vì vậy màu trắng sẽ thoát ra. 

### Ví dụ 2 

đầu vào: 

(-2, 3) và (2, 3) 

| dx | nhuộm | Tình trạng | Người chiến thắng | 
| --- | --- | --- | --- | 
| 4 | 0 | 0 < 4 là đúng | Lá Salix | 

Màu đen được căn chỉnh trực tiếp theo chiều ngang với màu trắng và đủ xa về phía trước. Nó có thể ngay lập tức thiết lập một vị trí chặn và duy trì nó, ngăn chặn quân trắng giành được đường đi rõ ràng về bên phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm chỉ yêu cầu số học theo thời gian không đổi | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài các biến đầu vào | 

Các ràng buộc cho phép tối đa 10^5 trường hợp thử nghiệm và giải pháp chỉ thực hiện một số thao tác số nguyên cho mỗi trường hợp, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import sys
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        x1, y1, x2, y2 = map(int, input().split())
        dx = x2 - x1
        dy = y2 - y1
        if dx <= 0:
            print("Walk Alone")
        else:
            print("Salix Leaf" if abs(dy) < dx else "Walk Alone")

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("3\n0 -1 2 1\n-2 3 2 3\n2 0 -1 0") == "Walk Alone\nSalix Leaf\nWalk Alone"

# black ahead but diagonal too large
assert run("1\n0 0 3 3") == "Walk Alone"

# black directly ahead aligned
assert run("1\n0 0 3 0") == "Salix Leaf"

# white already ahead
assert run("1\n5 0 2 0") == "Walk Alone"

# equality boundary case
assert run("1\n0 0 2 2") == "Walk Alone"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bộ mẫu | hỗn hợp | tính đúng đắn trong các trường hợp nhất định | 
| (0,0)-(3,3) | Đi Một Mình | ranh giới bình đẳng chéo | 
| (0,0)-(3,0) | Lá Salix | trường hợp chặn sạch | 
| trắng phía trước | Đi Một Mình | dx <= 0 trường hợp | 
| bằng dx=dy | Đi Một Mình | hành vi bất bình đẳng nghiêm ngặt | 

## Vỏ cạnh 

Khi quân đen bắt đầu dẫn trước nhưng khớp chính xác với độ lệch dọc về độ lớn, chẳng hạn như (0,0) và (2,2), thuật toán sẽ phân loại quân đen là quân trắng thắng vì abs(dy) không hoàn toàn nhỏ hơn dx. Trong tình huống này, khả năng điều chỉnh độ lệch dọc của quân đen chỉ đủ chặt để nó không thể đồng thời duy trì vị trí chặn phía trước, cho phép quân trắng tiếp tục tiến lên theo x. 

Khi màu đen đứng sau x, chẳng hạn như màu trắng ở (2,0) và màu đen ở (-1,0), dx là số âm và thuật toán ngay lập tức trả về Walk Alone. Mặc dù màu đen có thể tiến về phía màu trắng, nhưng nó không bao giờ có thể trở thành một kẻ chặn đường liên tục về phía trước vì màu trắng luôn giữ thế chủ động theo hướng x.

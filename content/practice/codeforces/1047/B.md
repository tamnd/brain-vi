---
title: "CF 1047B - Điểm bảo hiểm"
description: "Chúng ta được cho một tập hợp các điểm trên một mặt phẳng. Chúng ta phải đặt một hình tam giác thật cụ thể sao cho mọi điểm đều nằm bên trong nó hoặc trên ranh giới của nó."
date: "2026-06-15T11:08:46+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1047
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 511 (Div. 2)"
rating: 900
weight: 1047
solve_time_s: 399
verified: false
draft: false
---

[CF 1047B - Điểm bảo hiểm](https://codeforces.com/problemset/problem/1047/B) 

**Xếp hạng:** 900 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 6 phút 39 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm trên một mặt phẳng. Chúng ta phải đặt một hình tam giác thật cụ thể sao cho mọi điểm đều nằm bên trong nó hoặc trên ranh giới của nó. Tam giác không phải là tùy ý, nó bị ràng buộc là tam giác cân và thẳng hàng với các trục tọa độ theo nghĩa là hai cạnh bằng nhau của nó nằm dọc theo các trục. Nhiệm vụ là chọn một tam giác có “độ dài cạnh ngắn hơn” nhỏ nhất có thể trong khi vẫn bao trùm được tất cả các điểm. 

Về mặt hình học, điều này làm giảm sự tự do mà chúng ta có: một khi tam giác đã được định vị, hình dạng của nó về cơ bản được cố định bởi một tham số, độ dài của các cạnh bằng nhau của nó. Việc tăng độ dài này sẽ mở rộng vùng được che phủ, giảm độ dài này sẽ làm vùng được che phủ co lại. Mục tiêu là tìm giá trị tối thiểu của tham số này sao cho tất cả các điểm đều được bao phủ. 

Kích thước đầu vào lên tới 100.000 điểm, do đó, bất kỳ giải pháp nào cố gắng mô phỏng vị trí hình tam giác cho mọi điểm ứng viên hoặc kiểm tra phạm vi hình học theo cách lồng nhau đơn giản sẽ không hoạt động. Quét bậc hai sẽ yêu cầu khoảng$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 1 giây. Điều này ngay lập tức gợi ý rằng câu trả lời phải phụ thuộc vào một số thuộc tính tổng hợp của tập hợp điểm thay vì tương tác theo cặp. 

Các trường hợp cạnh chủ yếu là thoái hóa hình học. Nếu tất cả các điểm trùng nhau thì câu trả lời sẽ ở mức tối thiểu. Nếu các điểm nằm cách nhau rất xa theo một hướng thì câu trả lời sẽ bị chi phối bởi mức chênh lệch đó. Một lỗi phổ biến là cố gắng suy luận về cấu trúc thân lồi hoặc các lựa chọn hướng tam giác, điều này không cần thiết ở đây và dẫn đến sự phức tạp quá mức mà không cải thiện được giới hạn lõi. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là sửa kích thước tam giác ứng cử viên và sau đó kiểm tra xem tất cả các điểm có khớp với một số vị trí của tam giác đó hay không. Điều này đòi hỏi phải thử nhiều vị trí có thể hoặc ít nhất là xác minh mức độ phù hợp đối với các vị trí neo khác nhau. Mỗi lần xác minh sẽ có giá$O(n)$và nếu chúng ta tìm kiếm trên nhiều kích thước hoặc cấu hình ứng cử viên, thì độ phức tạp tổng cộng ít nhất sẽ trở thành bậc hai hoặc tệ hơn. Khó khăn là tam giác bị hạn chế nhưng vẫn có thể đặt được liên tục, do đó mô phỏng đơn giản không làm rời rạc không gian tìm kiếm một cách tự nhiên. 

Quan sát quan trọng là tam giác có cấu trúc thẳng hàng theo trục, nghĩa là các ràng buộc biên của nó giảm một cách hiệu quả về các điều kiện độc lập trên tọa độ x và y. Thay vì suy nghĩ về hình học một cách tổng thể, chúng ta có thể diễn giải lại điều kiện bao phủ như là giới hạn các điểm trong một hình được xác định bởi các giá trị tọa độ cực trị. 

Vì tam giác giãn nở đồng đều theo chiều dài cạnh của nó nên hệ số giới hạn luôn được xác định bằng khoảng cách mà điểm xa nhất nằm trong hệ tọa độ do hướng của tam giác gây ra. Sau khi chuyển hình học thành các ràng buộc trục, vấn đề giảm xuống còn việc tìm một giá trị đồng thời chi phối trải rộng theo cả hướng ngang và dọc trong một kết hợp tuyến tính cụ thể. Điều này thu gọn việc tối ưu hóa hình học thành một phép tính đơn giản trên cực trị của tọa độ được biến đổi. 

Do đó, thay vì thử tất cả các vị trí, chúng ta chỉ cần tính toán một số lượng nhỏ thống kê tổng thể từ tập hợp điểm, cụ thể là giá trị tối thiểu và tối đa của các biểu thức tuyến tính nhất định xuất phát từ tọa độ. Câu trả lời sẽ trở thành mức tối đa của các phạm vi dẫn xuất này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta viết lại ràng buộc hình học thành một thứ chỉ phụ thuộc vào các điểm cực trị tọa độ. Mỗi điểm đóng góp một ràng buộc về độ lớn của cạnh tam giác để nó được bao gồm trong vùng được bao phủ. 

1. Đọc tất cả các điểm và theo dõi tọa độ x nhỏ nhất và lớn nhất cũng như tọa độ y nhỏ nhất và lớn nhất. Các giá trị này nắm bắt toàn bộ chiều ngang và chiều dọc của tập hợp điểm. 
2. Tính nhịp ngang là$x_{\max} - x_{\min}$. Điều này thể hiện khoảng cách giữa các điểm từ trái sang phải. 
3. Tính nhịp dọc như$y_{\max} - y_{\min}$. Điều này thể hiện sự lây lan lên xuống. 
4. Kết hợp hai giá trị này theo hình dạng của tam giác cân thẳng hàng với trục. Yếu tố giới hạn không hoàn toàn nằm ngang hay dọc mà là hướng trong trường hợp xấu nhất, buộc tam giác phải đủ lớn để bao phủ cả hai cực cùng một lúc. 
5. Trả về khoảng thời gian yêu cầu tối đa, đảm bảo rằng cả hai ràng buộc về hướng đều được thỏa mãn. 

### Tại sao nó hoạt động 

Bất biến quan trọng là bất kỳ vị trí hợp lệ nào của tam giác đều phải đủ lớn để chứa đồng thời các điểm cực trị được phân cách bằng x và các điểm cực trị được phân tách bằng y. Bởi vì tam giác mở rộng đồng đều dọc theo các trục xác định của nó, nên không có sự dịch chuyển thông minh nào có thể làm giảm yêu cầu do các cực trị này đặt ra. Mỗi tập hợp điểm có một cặp điểm xác định kích thước tối thiểu cần thiết một cách độc lập theo các hướng trực giao và vị trí tối ưu chỉ căn chỉnh các ràng buộc này chứ không thể thu nhỏ chúng. Do đó, giải pháp giảm chính xác chỉ còn một hàm của phạm vi tọa độ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    xmin = ymin = 10**18
    xmax = ymax = -10**18

    for _ in range(n):
        x, y = map(int, input().split())
        xmin = min(xmin, x)
        xmax = max(xmax, x)
        ymin = min(ymin, y)
        ymax = max(ymax, y)

    print(max(xmax - xmin, ymax - ymin))

if __name__ == "__main__":
    solve()
```Giải pháp chỉ theo dõi cực trị toàn cục trong khi đọc dữ liệu đầu vào. Điều này tránh việc lưu trữ tất cả các điểm và đảm bảo thời gian tuyến tính. Câu trả lời cuối cùng được tính toán trực tiếp từ các cực trị này, vì bất kỳ tam giác khả thi nào cũng phải trải dài cả đường kính ngang và dọc của điểm được đặt trong hệ tọa độ cảm ứng. 

Một điểm tinh tế là việc khởi tạo: điểm cực trị phải bắt đầu từ các trọng điểm dương và âm đủ lớn để xử lý chính xác các đầu vào một điểm. Một điều nữa là tất cả số học đều nằm trong phạm vi số nguyên vì tọa độ tăng lên$10^9$, và sự khác biệt vẫn còn trong$10^9$tỉ lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1
1 2
2 1
```Chúng tôi theo dõi cực trị từng bước: 

| Bước | x | y | xmin | xmax | ymin | ymax | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 1 | 1 | 
| 2 | 1 | 2 | 1 | 1 | 1 | 2 | 
| 3 | 2 | 1 | 1 | 2 | 1 | 2 | 

Khoảng ngang là 1, khoảng dọc là 1, vì vậy câu trả lời là 1. 

Điều này cho thấy rằng mặc dù các điểm tạo thành một tam giác nhỏ nhưng yếu tố hạn chế chỉ là độ trải đều đơn vị theo cả hai hướng. 

### Ví dụ 2 

đầu vào:```
4
1 1
1 10
10 1
10 10
```| Bước | xmin | xmax | ymin | ymax | 
| --- | --- | --- | --- | --- | 
| sau tất cả các điểm | 1 | 10 | 1 | 10 | 

Khoảng ngang = 9, khoảng dọc = 9, vì vậy câu trả lời là 9. 

Điều này thể hiện một trường hợp cực đoan đối xứng trong đó cả hai trục đóng góp như nhau và câu trả lời hoàn toàn được điều khiển bởi kích thước hộp giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi điểm được xử lý một lần để cập nhật extrema | 
| Không gian |$O(1)$| Chỉ có bốn biến được lưu trữ | 

Các ràng buộc cho phép lên đến$10^5$điểm, do đó, một lần quét tuyến tính duy nhất vừa vặn thoải mái trong giới hạn thời gian và bộ nhớ không đổi đảm bảo không có chi phí lưu trữ tập dữ liệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    xmin = ymin = 10**18
    xmax = ymax = -10**18

    for _ in range(n):
        x, y = map(int, input().split())
        xmin = min(xmin, x)
        xmax = max(xmax, x)
        ymin = min(ymin, y)
        ymax = max(ymax, y)

    return str(max(xmax - xmin, ymax - ymin))

# provided sample
assert run("""3
1 1
1 2
2 1
""") == "1"

# single point
assert run("""1
5 5
""") == "0"

# horizontal line
assert run("""3
1 1
5 1
10 1
""") == "9"

# vertical line
assert run("""3
2 1
2 4
2 8
""") == "7"

# square corners
assert run("""4
1 1
1 100
100 1
100 100
""") == "99"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 0 | trường hợp thoái hóa | 
| đường ngang | 9 | sự thống trị của x-span | 
| đường thẳng đứng | 7 | sự thống trị của y-span | 
| góc vuông | 99 | mức chênh lệch cực kỳ đối xứng | 

## Vỏ cạnh 

Đối với một điểm duy nhất như`(5, 5)`, cực trị không bao giờ thay đổi và cả hai nhịp đều bằng 0, do đó thuật toán trả về đúng 0. Tam giác có thể suy biến thành một bìa có kích thước điểm. 

Đối với một đường ngang như`(1,1), (5,1), (10,1)`, the vertical span is zero while the horizontal span is 9. The algorithm correctly identifies that only horizontal spread matters, producing 9.

 Đối với một đường thẳng đứng như`(2,1), (2,4), (2,8)`, khoảng ngang bằng 0 và khoảng dọc là 7, do đó câu trả lời trở thành 7. Điều này xác nhận rằng phương pháp xử lý các trục một cách đối xứng. 

Đối với một ranh giới hình vuông đầy đủ, tất cả các điểm cực trị đều hoạt động và kết quả được điều khiển bởi độ dài cạnh tối đa của hộp giới hạn. Điều này xác nhận rằng không có cấu hình hình học ẩn nào có thể giảm kích thước yêu cầu xuống dưới mức cực trị thực thi.

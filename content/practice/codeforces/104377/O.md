---
title: "CF 104377O - \u6355\u9c7c\u8fbe\u4eba\uff01"
description: "Chúng ta có một tập hợp các điểm trên mặt phẳng, mỗi điểm mang một giá trị dương hoặc âm. Người câu cá đứng ở gốc và có thể giăng lưới một cách rất linh hoạt."
date: "2026-07-01T17:25:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104377
codeforces_index: "O"
codeforces_contest_name: "The 21st Sichuan University Programming Contest"
rating: 0
weight: 104377
solve_time_s: 61
verified: true
draft: false
---

[CF 104377O - \u6355\u9c7c\u8fbe\u4eba\uff01](https://codeforces.com/problemset/problem/104377/O) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên mặt phẳng, mỗi điểm mang một giá trị dương hoặc âm. Người câu cá đứng ở gốc và có thể giăng lưới một cách rất linh hoạt. Lưới luôn tương ứng với một vòng tròn có ranh giới đi qua điểm gốc và mọi thứ bên trong hoặc trên vòng tròn đó đều được thu thập. Tổng mức tăng là tổng giá trị của tất cả các điểm thu thập được. 

Sự tự do hình học quan trọng là chúng ta không cố định tâm hoặc bán kính hình tròn. Chúng ta được phép chọn bất kỳ đường tròn nào miễn là nó đi qua gốc tọa độ, và điều này bao gồm trường hợp suy biến trong đó đường tròn trở nên lớn tùy ý, biến ranh giới thành một đường thẳng đi qua gốc tọa độ, để lại một nửa mặt phẳng là vùng thu thập. 

Vì vậy, nhiệm vụ là chọn một vùng hình học được xác định bởi một đường tròn như vậy để tối đa hóa tổng giá trị của tất cả các điểm bên trong nó. 

Các ràng buộc đủ nhỏ để có thể thực hiện được việc quét hình học O(n^2). Với n lên tới 1000, ngay cả một giải pháp coi mỗi điểm là một hướng tham chiếu và xử lý tất cả những điểm khác có liên quan đến nó cũng sẽ trôi qua một cách thoải mái trong giới hạn thời gian. 

Một điểm tinh tế là vùng tối ưu không nhất thiết phải là “vòng tròn nhỏ bao quanh một cụm điểm tích cực”. Bởi vì chúng ta có thể mở rộng đường tròn trong khi vẫn buộc nó đi qua gốc tọa độ, nên chúng ta có thể xấp xỉ một cách hiệu quả bất kỳ nửa mặt phẳng nào đi qua gốc tọa độ. Điều này làm cho các cấu hình có nhiều điểm tích cực vừa phải nằm ở một phía của đường quan trọng hơn nhiều so với các điểm có giá trị cao được phân cụm chặt chẽ. 

Một sai lầm ngây thơ là chỉ nghĩ về khoảng cách, giả sử chúng ta nên chọn bán kính có tâm ở đâu đó để nắm bắt các điểm tích cực gần đó. Điều đó bỏ qua ràng buộc rằng đường tròn phải đi qua điểm gốc, về cơ bản ràng buộc mọi vùng hợp lệ với một hướng được neo tại điểm gốc. 

Một cạm bẫy phổ biến khác là xử lý bài toán như chọn bài toán bao lồi hoặc bài toán đường tròn giới hạn. Ví dụ: người ta có thể cố gắng chọn đường tròn bao quanh tối thiểu của các điểm dương, nhưng đường tròn đó không bắt buộc phải đi qua gốc tọa độ, vì vậy nó không phải là ứng cử viên hợp lệ. 

## Phương pháp tiếp cận 

Cách giải thích mạnh mẽ là liệt kê mọi đường tròn có thể đi qua gốc tọa độ, sau đó kiểm tra tất cả các điểm bên trong nó và tính tổng. Một đường tròn trong mặt phẳng có ba bậc tự do, nhưng ràng buộc mà nó đi qua gốc tọa độ làm giảm điều này xuống còn hai bậc tự do cho tâm của nó. Ngay cả khi chúng ta rời rạc hóa các vị trí trung tâm hoặc cố gắng liệt kê tổ hợp các vòng tròn ứng cử viên được xác định bằng các cặp điểm, chúng ta sẽ nhanh chóng phải đối mặt với sự bùng nổ về khả năng. Việc kiểm tra tư cách thành viên của tất cả n điểm cho mỗi ứng cử viên mang lại độ phức tạp bậc ba hoặc tệ hơn, điều này là không cần thiết đối với cấu trúc hình học. 

Quan sát quan trọng là mọi đường tròn đi qua gốc tọa độ tạo ra một phân vùng của mặt phẳng trong đó “hành vi biên xa” tương đương với một nửa mặt phẳng. Khi bán kính của đường tròn tăng lên, ranh giới của nó phẳng cục bộ xung quanh điểm gốc và vùng tiến đến một nửa mặt phẳng có đường biên đi qua điểm gốc. 

Cụ thể hơn, bất kỳ cấu hình tối ưu nào cũng có thể được biểu diễn dưới dạng lựa chọn một đường có hướng đi qua gốc tọa độ, giữ tất cả các điểm nằm về một phía của đường đó. Khi chúng ta cố định một hướng, chúng ta đang quyết định một nửa mặt phẳng một cách hiệu quả và mọi đường tròn hợp lệ có thể được kéo dài sao cho nó bao gồm chính xác những điểm nằm trên một phía của một đường thẳng nào đó đi qua gốc tọa độ. 

Điều này làm giảm bài toán xuống còn: tìm một đường thẳng đi qua gốc tọa độ sao cho tổng giá trị của các điểm ở một phía của đường thẳng đó là lớn nhất.

Đây là một vấn đề quét góc cổ điển. Mỗi điểm có một góc cực xung quanh gốc tọa độ. Bất kỳ nửa mặt phẳng nào cũng tương ứng với một khoảng các góc có độ dài nhỏ hơn π. Vì vậy, chúng tôi sắp xếp các điểm theo góc và sử dụng cửa sổ trượt có chiều rộng π trên không gian góc tròn, duy trì tổng các giá trị bên trong cửa sổ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Vòng tròn vũ phu | Hàm mũ hoặc tốt nhất là O(n^3) | O(n) | Quá chậm | 
| Nửa mặt phẳng quét góc | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi bài toán hình học thành tọa độ góc, sau đó tìm kiếm phạm vi bao phủ hình bán nguyệt tốt nhất của các điểm có trọng số. 

1. Tính góc cực của mọi điểm đối với gốc tọa độ. Điều này ánh xạ từng điểm tới một góc trong phạm vi [0, 2π). 
2. Nhân đôi danh sách các điểm bằng cách thêm 2π vào mỗi góc và thêm nó vào mảng. Điều này cho phép chúng ta mô phỏng sự bao quanh hình tròn bằng cách sử dụng quét tuyến tính. 
3. Sắp xếp tất cả các điểm theo góc của chúng. Cần phải sắp xếp sao cho các khoảng góc liền kề tương ứng với các đoạn liền kề trong mảng. 
4. Sử dụng cửa sổ trượt hai con trỏ. Đối với mỗi điểm cuối bên trái, chúng tôi nâng điểm cuối bên phải lên càng xa càng tốt trong khi vẫn duy trì khoảng góc nhỏ hơn π. 
5. Đối với mỗi cửa sổ hợp lệ, hãy tính tổng giá trị của các điểm bên trong nó và theo dõi mức tối đa trên tất cả các cửa sổ. 

Lý do kích thước cửa sổ là π là vì nửa mặt phẳng đi qua gốc tọa độ tương ứng chính xác với mọi hướng nằm trong bất kỳ khoảng độ dài π nào. Bất kỳ khoảng lớn hơn nào nhất thiết sẽ bao gồm các điểm từ cả hai phía của đường thẳng nào đó đi qua gốc tọa độ, vi phạm cấu trúc nửa mặt phẳng. 

### Tại sao nó hoạt động 

Mọi giải pháp khả thi đều tương ứng với việc chọn một đường thẳng có hướng đi qua gốc tọa độ, xác định một nửa mặt phẳng. Nửa mặt phẳng đó tương ứng với một khoảng góc liên tục có chiều rộng π trên đường tròn đơn vị. Ngược lại, bất kỳ khoảng nào như vậy đều xác định một nửa mặt phẳng hợp lệ. Cửa sổ trượt liệt kê tất cả các khoảng như vậy, do đó mọi cấu hình hình học hợp lệ được xem xét chính xác một lần. Do đó, tổng cửa sổ tốt nhất bằng với lựa chọn dựa trên vòng tròn tối ưu. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve():
    n = int(input())
    v = list(map(int, input().split()))
    
    pts = []
    for i in range(n):
        x, y = map(int, input().split())
        ang = math.atan2(y, x)
        pts.append((ang, v[i]))
    
    pts.sort()
    
    extended = pts + [(ang + 2 * math.pi, val) for ang, val in pts]
    
    m = len(extended)
    
    ans = -10**18
    current_sum = 0
    r = 0
    
    for l in range(n):
        if r < l:
            r = l
            current_sum = 0
        
        while r < m and extended[r][0] - extended[l][0] < math.pi - 1e-12:
            current_sum += extended[r][1]
            r += 1
        
        ans = max(ans, current_sum)
        current_sum -= extended[l][1]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện bắt đầu bằng cách chuyển đổi từng điểm thành dạng góc cực. Việc sử dụng`atan2`đảm bảo xử lý chính xác trên tất cả các góc phần tư, bao gồm cả tọa độ âm. 

Sau khi sắp xếp, chúng tôi nhân đôi mảng một cách rõ ràng với các góc được dịch chuyển 2π. Đây là điều cho phép cửa sổ trượt coi không gian góc tròn như một khoảng tuyến tính mà không cần số học mô-đun đặc biệt. 

Quét hai con trỏ duy trì tổng giá trị đang chạy bên trong cửa sổ góc hiện tại. điều kiện`< π`thực thi ràng buộc nửa mặt phẳng. Bước trừ khi di chuyển con trỏ sang trái đảm bảo tổng cửa sổ không đổi. 

Một lỗi triển khai phổ biến là quên các vấn đề về độ chính xác của dấu phẩy động khi so sánh các góc khác nhau. Epsilon nhỏ ngăn chặn sự mất ổn định biên khi các điểm nằm chính xác trên ngưỡng π. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ: 

đầu vào:```
3
1 -2 3
1 0
-1 0
0 1
```Chúng tôi tính toán các góc và giá trị: 

| Điểm | Góc | Giá trị | 
| --- | --- | --- | 
| (1,0) | 0 | 1 | 
| (-1,0) | π | -2 | 
| (0,1) | π/2 | 3 | 

Sau khi sắp xếp theo góc, chúng ta đánh giá các cửa sổ có chiều rộng π: 

| tôi | r | Điểm cửa sổ | Tổng hợp | 
| --- | --- | --- | --- | 
| 0 | 2 | (1,0), (0,1) | 4 | 
| 1 | 3 | (-1,0), (0,1) | 1 | 
| 2 | 3 | (0,1) | 3 | 

Câu trả lời tốt nhất là 4, đạt được bằng cách chọn một nửa mặt phẳng bao phủ trục x dương và vùng nửa mặt phẳng phía trên. 

Dấu vết này cho thấy thuật toán chọn phân chia có hướng một cách hiệu quả như thế nào thay vì vùng hình học bị chặn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp điểm theo góc chiếm ưu thế, cửa sổ trượt là tuyến tính | 
| Không gian | O(n) | Lưu trữ các cặp giá trị góc và mảng trùng lặp | 

Với n lên tới 1000, điều này dễ dàng phù hợp với giới hạn thời gian. Ngay cả trong những ràng buộc chặt chẽ hơn, giải pháp có thể mở rộng quy mô một cách thoải mái nhờ một bước sắp xếp duy nhất và quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n = int(input())
    v = list(map(int, input().split()))
    pts = []
    for i in range(n):
        x, y = map(int, input().split())
        pts.append((math.atan2(y, x), v[i]))
    pts.sort()
    extended = pts + [(a + 2*math.pi, b) for a,b in pts]

    m = len(extended)
    ans = -10**18
    cur = 0
    r = 0
    for l in range(n):
        if r < l:
            r = l
            cur = 0
        while r < m and extended[r][0] - extended[l][0] < math.pi:
            cur += extended[r][1]
            r += 1
        ans = max(ans, cur)
        cur -= extended[l][1]
    return str(ans)

# provided sample-like tests
assert run("""3
1 -2 3
1 0
-1 0
0 1
""").strip() == "4"

# all negative
assert run("""2
-5 -7
1 0
-1 0
""").strip() == "-5"

# all positive clustered
assert run("""3
1 2 3
1 0
2 0
3 0
""").strip() == "6"

# symmetric points
assert run("""4
1 1 1 1
1 0
-1 0
0 1
0 -1
""").strip() == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trộn nhỏ | 4 | lựa chọn nửa mặt phẳng đúng | 
| tất cả đều tiêu cực | -5 | tránh các vấn đề sai lệch tập hợp trống | 
| liên kết tích cực | 6 | sự ổn định bao gồm đầy đủ | 
| chéo đối xứng | 2 | độ chính xác của ranh giới góc | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là khi các điểm nằm chính xác trên đường biên của nửa mặt phẳng đã chọn. Về mặt góc cạnh, điều này tương ứng với các hiệu chính xác bằng π. Thuật toán xử lý cửa sổ hoàn toàn nhỏ hơn π, điều này đảm bảo mỗi ranh giới được gán nhất quán chỉ cho một bên. Ví dụ: các điểm ở góc 0 và π không bao giờ được đưa vào cùng một cửa sổ, phù hợp với cách giải thích hình học rằng một đường thẳng đi qua gốc tọa độ sẽ đặt các điểm ở các cạnh đối diện. 

Một trường hợp tinh tế khác xảy ra khi tất cả các giá trị đều âm. Chiến lược tối ưu vẫn là chọn nửa mặt phẳng âm nhỏ nhất chứ không phải chọn tập trống. Cửa sổ trượt xử lý việc này một cách tự nhiên vì mỗi tổng cửa sổ đều được tính toán, bao gồm cả những tổng có tổng âm có cường độ tối thiểu, đảm bảo giá trị tối đa vẫn được ghi lại chính xác.

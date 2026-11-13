---
title: "CF 1713A - Bài toán nhân viên bán hàng du lịch"
description: "Chúng tôi đang làm việc trên một mạng lưới cho phép di chuyển theo bốn hướng chính và mỗi lần di chuyển đều tốn một bước."
date: "2026-06-09T20:12:12+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1713
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 812 (Div. 2)"
rating: 800
weight: 1713
solve_time_s: 104
verified: true
draft: false
---

[CF 1713A - Vấn đề của nhân viên bán hàng du lịch](https://codeforces.com/problemset/problem/1713/A) 

**Đánh giá:** 800 
**Tags:** hình học, tham lam, thực hiện 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một mạng lưới cho phép di chuyển theo bốn hướng chính và mỗi lần di chuyển đều tốn một bước. Một số điểm được đặt trên lưới này, nhưng có một hạn chế: mọi điểm đều nằm hoàn toàn trên một trong các trục, nghĩa là tọa độ x của nó bằng 0 hoặc tọa độ y của nó bằng 0. Nhiệm vụ của chúng ta là bắt đầu từ điểm gốc, ghé thăm tất cả các điểm này theo bất kỳ thứ tự nào và quay trở lại điểm gốc trong khi giảm thiểu tổng chi phí di chuyển. 

Một cách hữu ích để điều chỉnh lại nhiệm vụ là mỗi điểm nằm trên một đường ngang hoặc dọc đi qua gốc tọa độ. Các điểm trên trục x nằm trên đường thẳng y = 0 và các điểm trên trục y nằm trên x = 0. Vì chuyển động là khoảng cách Manhattan nên việc di chuyển giữa các điểm có tính cộng dọc theo các đoạn thẳng hàng với trục. 

Kích thước đầu vào cho mỗi thử nghiệm nhỏ nhưng số lượng trường hợp thử nghiệm có thể lớn. Mỗi bài kiểm tra chứa tối đa 100 điểm, do đó, giải pháp bậc hai hoặc bậc ba cho mỗi bài kiểm tra đã an toàn nhưng chúng ta nên hướng tới giải pháp dựa trên quan sát tuyến tính hoặc gần tuyến tính. Tổng số điểm trong các bài kiểm tra không bị giới hạn, điều đó có nghĩa là O(n²) cho mỗi giải pháp kiểm tra vẫn ổn, nhưng bất cứ điều gì tệ hơn trên mỗi cấu trúc trường hợp kiểm thử sẽ là không cần thiết. 

Một cách giải thích ngây thơ có thể đề xuất thử tất cả các hoán vị của các điểm tham quan để tính toán quãng đường đi bộ khép kín ngắn nhất. Điều đó sẽ ngay lập tức bùng nổ về mặt tổ hợp vì có n! các đơn đặt hàng có thể. Ngay cả việc tính toán các đường đi ngắn nhất theo cặp và thực hiện giải pháp lập trình động dành cho nhân viên bán hàng du lịch cũng là quá mức cần thiết đối với n 100. 

Các trường hợp cạnh đáng suy nghĩ đến từ sự phân bố điểm suy biến: 

Nếu tất cả các điểm đều ở (0, 0) thì không cần di chuyển. 

Nếu tất cả các điểm chỉ nằm trên một phía trục, ví dụ chỉ có trục y dương, một chiến lược đơn giản vẫn có thể cố gắng len lỏi giữa các hướng không cần thiết, nhưng đường đi tối ưu chỉ đơn giản là đi đến điểm xa nhất và quay lại, thu thập mọi thứ trên đường đi. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mỗi hộp như một nút và xem xét mọi thứ tự có thể có để truy cập chúng, tính tổng khoảng cách Manhattan giữa các điểm liên tiếp và quay trở lại điểm gốc. Đây là một công thức cổ điển của người bán hàng du lịch. Tính chính xác rất đơn giản vì nó kiểm tra tất cả các chuyến tham quan có thể. 

Sự thất bại hoàn toàn là tính toán. Với n điểm, số lượng hoán vị là n!, và thậm chí với n = 10, điều này đã trở nên không khả thi, trong khi ở đây n có thể là 100. Ngay cả việc lập trình động trên các tập hợp con cũng sẽ yêu cầu O(n² · 2ⁿ), vượt xa giới hạn. 

Quan sát quan trọng là hình học bị hạn chế rất nhiều. Mỗi điểm nằm trên một trong hai đường thẳng cắt nhau tại gốc tọa độ. Điều này có nghĩa là chuyển động có thể được phân tách độc lập dọc theo hướng trục x và trục y. Trên mỗi trục, tất cả các điểm đều nằm trên một đường duy nhất, do đó việc truy cập chúng sẽ giảm thiểu việc đi bộ từ 0 đến điểm xa nhất và quay lại trong khi quét qua các điểm trung gian một cách tối ưu. 

Đối với trục x, chỉ x dương tối đa và x âm tối thiểu là quan trọng. Bất kỳ tuyến đường nào đi qua cả hai thái cực đều phải đi qua toàn bộ khoảng cách giữa chúng và các điểm trung gian sẽ tự động được che phủ nếu chúng ta di chuyển ra ngoài một cách đơn điệu. Logic tương tự áp dụng cho trục y. 

Do đó, chiến lược tối ưu chỉ đơn giản là tính toán xem chúng ta phải đi bao xa theo mỗi hướng: các điểm xa nhất phải, trái, lên và xuống và tính tổng gấp đôi những khoảng cách đó vì mỗi trục yêu cầu một chuyến đi khứ hồi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (TSP trên điểm) | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu (tọa độ cực cao) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tách các điểm thành các phần đóng góp trên trục x và trục y. Các điểm có y = 0 góp phần chuyển động theo chiều ngang và các điểm có x = 0 góp phần chuyển động theo chiều dọc. Sự tách biệt này có tác dụng vì chuyển động trên một trục không ảnh hưởng đến trục kia. 
2. Theo dõi tọa độ x dương lớn nhất trong số tất cả các điểm trên trục x. 
3. Theo dõi tọa độ x âm tối thiểu giữa tất cả các điểm trên trục x. 
4. Theo dõi tọa độ y dương tối đa trong số tất cả các điểm trên trục y. 
5. Theo dõi tọa độ y âm tối thiểu giữa tất cả các điểm trên trục y. 
6. Tính khoảng cách ngang cần thiết là max_x + abs(min_x). Điều này thể hiện tổng khoảng cách cần thiết để đạt được cả hai thái cực kể từ điểm gốc. 
7. Tính khoảng cách theo chiều dọc cần thiết là max_y + abs(min_y) vì lý do tương tự. 
8. Hoàn trả gấp đôi tổng của hai nhịp này, vì mỗi chuyến đi từ điểm gốc đến điểm cực trị đều phải quay trở lại. 

Ý tưởng chính đằng sau các bước này là việc truy cập tất cả các điểm trên một đường thẳng không yêu cầu xem lại cấu trúc trung gian; chỉ những thái cực mới xác định được phong bì du lịch cần thiết. 

### Tại sao nó hoạt động 

Tất cả các điểm trên trục x đều nằm trên một đường thẳng y = 0, do đó, bất kỳ tuyến đường nào đi qua cả điểm ngoài cùng bên trái và điểm ngoài cùng bên phải đều phải đi qua toàn bộ khoảng cách giữa chúng. Các điểm trung gian nằm trong khoảng này và được tự động truy cập trong quá trình di chuyển đó nếu chúng ta di chuyển đơn điệu. Điều tương tự cũng áp dụng cho trục y một cách độc lập. Vì các chuyển động trên trục x và y là trực giao và cộng theo khoảng cách Manhattan, nên tổng đường đi tối ưu là tổng các đường đi tối ưu độc lập cho mỗi trục, mỗi trục yêu cầu một hành trình khứ hồi từ điểm gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    
    max_x = 0
    min_x = 0
    max_y = 0
    min_y = 0
    
    for _ in range(n):
        x, y = map(int, input().split())
        if y == 0:
            max_x = max(max_x, x)
            min_x = min(min_x, x)
        else:
            max_y = max(max_y, y)
            min_y = min(min_y, y)
    
    ans = 2 * (max_x - min_x + max_y - min_y)
    print(ans)
```Việc triển khai giữ bốn giá trị đang chạy để nắm bắt đường bao hình học của tất cả các điểm. Việc khởi tạo ở mức 0 hoạt động vì gốc luôn là đường cơ sở hợp lệ và bất kỳ điểm nào chỉ có trục sẽ điều chỉnh cực dương hoặc cực âm tương ứng. 

Công thức cuối cùng nhân với hai vì chúng ta phải quay lại điểm gốc sau khi thu thập tất cả các điểm, làm cho mỗi trục đi qua một vòng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 4
(0, -2), (1, 0), (-1, 0), (0, 2)
```Chúng tôi theo dõi các thái cực: 

| Bước | max_x | phút_x | max_y | phút_y | 
| --- | --- | --- | --- | --- | 
| (0,-2) | 0 | 0 | 0 | -2 | 
| (1,0) | 1 | 0 | 0 | -2 | 
| (-1,0) | 1 | -1 | 0 | -2 | 
| (0,2) | 1 | -1 | 2 | -2 | 

Khoảng ngang = 1 - (-1) = 2 

Khoảng dọc = 2 - (-2) = 4 

Tổng = 2 × (2 + 4) = 12 

Dấu vết này cho thấy mức độ xác định đầy đủ chi phí, bất kể đặt hàng. 

### Mẫu 2 

đầu vào:```
n = 3
(0,2), (-3,0), (0,-1)
```| Bước | max_x | phút_x | max_y | phút_y | 
| --- | --- | --- | --- | --- | 
| (0,2) | 0 | 0 | 2 | 0 | 
| (-3,0) | 0 | -3 | 2 | 0 | 
| (0,-1) | 0 | -3 | 2 | -1 | 

Khoảng ngang = 0 - (-3) = 3 

Khoảng dọc = 2 - (-1) = 3 

Tổng = 2 × (3 + 3) = 12 

Điều này xác nhận rằng ngay cả các phân phối hỗn hợp cũng thu gọn thành các trục trục độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi điểm được xử lý một lần để cập nhật các điểm cực trị | 
| Không gian | O(1) | Chỉ có bốn số nguyên được duy trì | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì ngay cả tổng số điểm lớn nhất có thể cũng chỉ yêu cầu quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        max_x = 0
        min_x = 0
        max_y = 0
        min_y = 0
        for _ in range(n):
            x, y = map(int, input().split())
            if y == 0:
                max_x = max(max_x, x)
                min_x = min(min_x, x)
            else:
                max_y = max(max_y, y)
                min_y = min(min_y, y)
        out.append(str(2 * (max_x - min_x + max_y - min_y)))
    return "\n".join(out)

# provided samples
assert run("""3
4
0 -2
1 0
-1 0
0 2
3
0 2
-3 0
0 -1
1
0 0
""") == """12
12
0"""

# single point off-origin
assert run("""1
1
0 5
""") == "10"

# only x-axis points
assert run("""1
2
-2 0
3 0
""") == "10"

# only y-axis points
assert run("""1
2
0 -4
0 6
""") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm trục y đơn | 10 | hành vi chuyến đi khứ hồi cơ bản | 
| chỉ trải rộng trục x | 10 | xử lý các thái cực tiêu cực và tích cực | 
| chỉ trải rộng trục y | 20 | sự đối xứng trên trục tung | 

## Vỏ cạnh 

Khi chỉ có một điểm tại một vị trí không có điểm gốc, thuật toán sẽ đặt một khoảng trục về 0 và điểm còn lại theo khoảng cách từ điểm gốc đến điểm đó. Ví dụ, đầu vào`(0, 5)`mang lại max_y = 5 và min_y = 0, tạo ra khoảng dọc là 5 và câu trả lời cuối cùng là 10. Việc truyền tải tương ứng với việc đi lên (0,5) và quay trở lại. 

Khi tất cả các điểm nằm hoàn toàn trên trục x, giả sử`(-2,0)`Và`(3,0)`, y-span vẫn bằng 0 trong khi x-span trở thành 5. Thuật toán tạo ra 10, tương ứng với việc đi từ gốc đến -2, đi qua gốc đến 3 và quay trở lại. 

Khi điểm bao gồm chính gốc tọa độ, nó không ảnh hưởng đến bất kỳ cực trị nào. Một điểm tại (0,0) không đóng góp yêu cầu chuyển động nào và đương nhiên bị bỏ qua bởi logic nhịp, vì tất cả các cực trị vẫn không thay đổi.

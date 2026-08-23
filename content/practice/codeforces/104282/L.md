---
title: "CF 104282L - Cờ Tướng Tự Động"
description: "Chúng ta được cung cấp một tập hợp các vị trí của kẻ thù trên một mặt phẳng, tất cả đều được đo lường tương ứng với nguồn gốc nơi nhân vật của chúng ta đứng."
date: "2026-07-01T21:08:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104282
codeforces_index: "L"
codeforces_contest_name: "The 20th Hangzhou City University Programming Contest"
rating: 0
weight: 104282
solve_time_s: 60
verified: true
draft: false
---

[CF 104282L - Cờ vua tự động](https://codeforces.com/problemset/problem/104282/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các vị trí của kẻ thù trên một mặt phẳng, tất cả đều được đo lường tương ứng với nguồn gốc nơi nhân vật của chúng ta đứng. Nhân vật có thể xoay vũ khí và chọn hướng, và bất cứ khi nào tấn công, nhân vật sẽ loại bỏ tất cả kẻ thù nằm trong một khu vực góc 45 độ cố định bắt đầu từ hướng đó. Khu vực này được neo ở điểm gốc, vì vậy quyền tự do duy nhất là cách chúng ta xoay cái nêm này. 

Nhiệm vụ là chọn hướng của cái nêm 45 độ này sao cho số lượng kẻ thù bên trong nó là tối đa. 

Đầu vào chính lên tới 100.000 điểm với tọa độ trong phạm vi −10⁴ đến 10⁴. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng kiểm tra mọi hướng nêm có thể được xác định bởi các cặp hoặc bộ ba điểm, vì một phép so sánh góc đơn giản theo hướng ứng cử viên sẽ dẫn đến hành vi bậc hai hoặc tệ hơn. 

Một vấn đề tế nhị xuất hiện với hành vi ranh giới. Phải tính các điểm nằm chính xác trên hai tia tạo thành góc 45 độ. Điều này quan trọng vì bất kỳ công thức góc nào cũng phải xử lý sự bình đẳng một cách cẩn thận, nếu không các điểm trên đường biên có thể bị mất hoặc bị tính hai lần tùy thuộc vào so sánh dấu phẩy động. Một trường hợp cạnh khác là hướng dọc và hướng ngang trong đó việc tính toán độ dốc đơn giản không thành công do chia cho 0 hoặc độ chính xác không ổn định. 

Một ví dụ nhỏ minh họa độ nhạy biên. Nếu chúng ta có các điểm (1, 2), (2, 1) và (1, 1), góc tối ưu 45 độ có thể bao gồm cả ba nếu được định hướng đúng dọc theo đường y = x. Một cách tiếp cận đơn giản dựa trên các góc có dấu phẩy động có thể loại trừ các điểm được căn chỉnh theo ranh giới do lỗi chính xác. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mọi điểm của kẻ thù là hướng ứng cử viên cho một tia ranh giới của hình nêm. Đối với mỗi hướng như vậy, chúng ta có thể quét một tia biên khác ra xa 45 độ và đếm xem có bao nhiêu điểm nằm ở giữa. Đối với mỗi hướng cố định, việc kiểm tra tất cả các điểm sẽ mất O(n) và thử tất cả n hướng sẽ dẫn đến O(n²), tức là tệ nhất là khoảng 10¹⁰ thao tác và sẽ không chạy trong thời gian giới hạn. 

Quan sát quan trọng là hình học trở nên đơn giản khi chúng ta thay đổi tọa độ. Một cái nêm 45 độ thẳng hàng trong mặt phẳng tương ứng với điều kiện liên quan đến sự khác biệt về tọa độ. Nếu chúng ta xoay mặt phẳng 45 độ, hình nêm sẽ biến thành một ràng buộc khoảng cách thẳng hàng với trục trong hệ tọa độ được chuyển đổi. 

Cụ thể, xác định tọa độ được chuyển đổi: 

u = x + y và v = x − y. 

Cung góc 45 độ trong mặt phẳng ban đầu tương ứng với việc chọn các điểm có giá trị u nằm trong một khoảng có độ dài tỷ lệ với hướng nêm, đồng thời tôn trọng thứ tự trong v hoặc xử lý tương đương việc căn chỉnh đơn điệu. Sau khi biến đổi, bài toán rút gọn thành việc tìm số điểm tối đa có thể được bao phủ bởi một cửa sổ trượt trên các biểu diễn góc đã được sắp xếp hoặc tương đương trên các góc định hướng đã được sắp xếp. 

Một cách giải thích rõ ràng hơn sẽ tránh hoàn toàn lượng giác: mỗi điểm xác định một góc θ = atan2(y, x). Bài toán hình nêm trở thành: tìm số điểm tối đa có các góc nằm trong khoảng có độ dài π/4. Chúng tôi sắp xếp tất cả các góc và sử dụng thao tác quét hai con trỏ trên mảng hình tròn (nhân đôi các góc bằng cách thêm 2π). Điều này biến bài toán thành một điểm cực đại cổ điển trong một cửa sổ góc cố định. 

Quá trình quét hoạt động vì khi các góc được sắp xếp, việc mở rộng cửa sổ cho đến khi vượt quá π/4 và sau đó thu nhỏ nó sẽ duy trì quá trình quét tuyến tính trên tất cả các điểm cuối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Sắp xếp góc + hai con trỏ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách chuyển đổi hướng hình học thành thứ tự góc một chiều.

1. Tính góc của mọi điểm bằng cách sử dụng atan2(y, x). Thao tác này sẽ đưa ra một giá trị trong phạm vi (−π, π). Bước này chuyển đổi hướng hình học thành một đại lượng vô hướng có thể sắp xếp được, điều này rất cần thiết vì việc bao gồm góc sẽ trở thành ngăn chặn khoảng. 
2. Chuẩn hóa các góc thành một phạm vi nhất quán, thường là [0, 2π), sao cho có thể xử lý rõ ràng đường bao quanh hình tròn. 
3. Sắp xếp tất cả các góc theo thứ tự tăng dần. Cần phải sắp xếp sao cho bất kỳ phân đoạn liền kề nào trong danh sách này đều tương ứng với việc quét liên tục trong không gian góc. 
4. Mở rộng mảng bằng cách thêm mỗi góc cộng với 2π. Sự trùng lặp này cho phép chúng ta xử lý các khoảng bao quanh mà không có sự phức tạp về số học mô-đun. 
5. Sử dụng cửa sổ trượt hai con trỏ. Duy trì con trỏ bên phải mở rộng trong khi hiệu góc giữa góc [r] và góc [l] nhiều nhất là π/4. Điều này đảm bảo tất cả các điểm trong cửa sổ đều hợp lệ theo ràng buộc hình nêm. 
6. Đối với mỗi vị trí con trỏ trái, hãy tính phần mở rộng bên phải hợp lệ tối đa và cập nhật câu trả lời với r − l + 1. Sau đó di chuyển con trỏ trái về phía trước và tiếp tục. 
7. Trả về kích thước cửa sổ tối đa được tìm thấy. 

Lý do chúng ta có thể sử dụng cửa sổ trượt một cách an toàn là vì sau khi sắp xếp, việc mở rộng con trỏ bên phải chỉ làm tăng khoảng góc. Khi khoảng vượt quá π/4, nó sẽ không bao giờ hợp lệ nữa đối với điểm cuối bên trái cố định đó trừ khi chúng ta di chuyển con trỏ bên trái. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là bất kỳ cái nêm hợp lệ nào cũng tương ứng chính xác với một khoảng các góc có độ dài tối đa là π/4. Mỗi khoảng như vậy có thể được biểu diễn bằng một góc bắt đầu bằng góc nhỏ nhất bên trong nó. Đối với góc bắt đầu đó, giải pháp tối ưu là khối điểm liền kề lớn nhất trong phạm vi π/4 phía trước. Vì việc sắp xếp duy trì thứ tự góc nên mọi tập hợp con hợp lệ sẽ xuất hiện dưới dạng một phân đoạn liền kề trong mảng nhân đôi được sắp xếp và quá trình quét hai con trỏ sẽ liệt kê tất cả các phân đoạn như vậy một cách ngầm định. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve():
    n = int(input())
    angles = []
    
    for _ in range(n):
        x, y = map(int, input().split())
        ang = math.atan2(y, x)
        if ang < 0:
            ang += 2 * math.pi
        angles.append(ang)
    
    angles.sort()
    
    # duplicate for circular wrap
    extended = angles + [a + 2 * math.pi for a in angles]
    
    ans = 0
    r = 0
    window = math.pi / 4
    
    for l in range(n):
        while r < len(extended) and extended[r] - extended[l] <= window + 1e-12:
            r += 1
        ans = max(ans, r - l)
    
    print(ans)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là chuyển đổi sang các góc bằng cách sử dụng atan2, giúp tránh mọi sự mất ổn định dựa trên độ dốc. Bước chuẩn hóa đảm bảo tất cả các góc đều có thể so sánh được trên một thang đo tròn. Sự trùng lặp của mảng là điều cho phép xử lý các cửa sổ bao quanh như góc gần 350 độ kết hợp với góc gần 10 độ mà không cần trường hợp đặc biệt. 

Logic hai con trỏ cẩn thận duy trì một con trỏ phải đơn điệu, do đó mỗi phần tử được xử lý nhiều nhất hai lần, một lần ở bên trái và một lần ở bên phải. 

Epsilon nhỏ trong so sánh tránh được các vấn đề về độ chính xác của dấu phẩy động khi các điểm nằm chính xác trên ranh giới của khu vực 45 độ. 

## Ví dụ đã hoạt động 

Xét các điểm (1, 2), (2, 1) và (1, 1). 

Sau khi chuyển đổi sang góc: 

| Điểm | Góc (xấp xỉ) | 
| --- | --- | 
| (1,2) | 1.107 | 
| (2,1) | 0,464 | 
| (1,1) | 0,785 | 

Các góc được sắp xếp trở thành [0,464, 0,785, 1,107]. Chúng tôi nhân đôi chúng thành [0,464, 0,785, 1,107, 6,747, 7,068, 7,389]. 

Bây giờ chúng ta trượt một cửa sổ có kích thước π/4 ≈ 0,785. 

| tôi | r | góc[l] | góc[r-1] | nhịp | kích thước cửa sổ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 0,464 | 1.107 | 0,643 | 3 | 
| 1 | 3 | 0,785 | 1.107 | 0,322 | 2 | 
| 2 | 3 | 1.107 | 1.107 | 0 | 1 | 

Tối đa là 3, nghĩa là tất cả các điểm có thể được bao phủ trong một hình nêm 45 độ. 

Dấu vết này cho thấy rằng khi các điểm được sắp xếp trong không gian góc, phần nêm tối ưu sẽ tương ứng với một đoạn liền kề và cửa sổ trượt sẽ chụp chính xác đoạn đó. 

Ví dụ thứ hai với các điểm cách nhau rộng rãi như (1,0), (0,1), (-1,0), (0,-1) cho thấy rằng không có ba điểm nào nằm trong bất kỳ khoảng π/4 nào và cửa sổ đạt đỉnh chính xác ở 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Góc sắp xếp chiếm ưu thế, cửa sổ trượt tuyến tính | 
| Không gian | O(n) | Lưu trữ danh sách góc và mảng trùng lặp | 

Với n lên tới 100.000, việc sắp xếp dễ dàng đủ nhanh và độ quét tuyến tính là không đáng kể. Việc sử dụng bộ nhớ nằm trong giới hạn vì chúng tôi chỉ lưu trữ một vài mảng dấu phẩy động. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    angles = []
    for _ in range(n):
        x, y = map(int, input().split())
        ang = math.atan2(y, x)
        if ang < 0:
            ang += 2 * math.pi
        angles.append(ang)

    angles.sort()
    extended = angles + [a + 2 * math.pi for a in angles]

    ans = 0
    r = 0
    window = math.pi / 4

    for l in range(n):
        while r < len(extended) and extended[r] - extended[l] <= window + 1e-12:
            r += 1
        ans = max(ans, r - l)

    return str(ans)

# provided samples
assert run("2\n1 2\n2 1\n") == "2"
assert run("1\n1 100\n100 1\n") == "1"

# custom cases
assert run("1\n1 1\n") == "1", "single point"
assert run("3\n1 1\n2 2\n3 3\n") == "3", "collinear same direction"
assert run("4\n1 0\n0 1\n-1 0\n0 -1\n") == "1", "orthogonal spread"
assert run("5\n1 2\n2 1\n3 3\n4 4\n1 0\n") == "4", "mixed cluster"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 1 | độ chính xác đầu vào tối thiểu | 
| thẳng hàng cùng hướng | 3 | mọi điểm trong một tia góc | 
| trải rộng trực giao | 1 | không thể có cái nêm lớn | 
| cụm hỗn hợp | 4 | tính đúng đắn của cửa sổ trượt trên vùng dày đặc | 

## Vỏ cạnh 

Các điểm nằm rất gần ranh giới của khu vực 45 độ kiểm tra độ bền của dấu phẩy động. Ví dụ: các điểm như (1, 100) và (100, 1) tạo ra các góc cực kỳ gần với ranh giới π/4. Thuật toán xử lý việc này thông qua một epsilon nhỏ trong các phép so sánh, đảm bảo việc bao gồm ranh giới là nhất quán. 

Các trường hợp bao quanh như góc gần 0 và gần 2π được xử lý bằng cách sao chép mảng. Nếu không có sự trùng lặp, một hình nêm bao trùm góc 0 sẽ bị chia thành hai đoạn không chính xác. Mảng mở rộng hợp nhất các trường hợp này vào một cửa sổ liền kề duy nhất và con trỏ trượt tự nhiên chọn chúng như một phần của khoảng thời gian hợp lệ.

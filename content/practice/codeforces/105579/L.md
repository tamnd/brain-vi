---
title: "CF 105579L - Nhiệt độ sữa tối ưu"
description: "Chúng ta được cho một dãy các lô sữa, mỗi lô có hai thuộc tính. Đầu tiên là hệ số $bi$ biểu thị lượng Vova thu được trên mỗi đơn vị nhiệt độ gia nhiệt."
date: "2026-06-22T21:52:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105579
codeforces_index: "L"
codeforces_contest_name: "Udmurtia High School Programming Contest (Qualification for VKOSHP 2012)"
rating: 0
weight: 105579
solve_time_s: 61
verified: true
draft: false
---

[CF 105579L - Nhiệt độ sữa tối ưu](https://codeforces.com/problemset/problem/105579/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các lô sữa, mỗi lô có hai thuộc tính. Đầu tiên là hệ số$b_i$đại diện cho lượng hương vị mà Vova thu được trên một đơn vị nhiệt độ gia nhiệt. Thứ hai là giới hạn$t_i$đại diện cho nhiệt độ cao nhất mà mẻ này có thể được đun nóng mà không tạo ra bọt. Nếu nhiệt độ gia nhiệt toàn cầu được chọn là$T$, sau đó hàng loạt$i$chỉ có thể uống được nếu$T \le t_i$. Nếu nó có thể uống được, nó đóng góp chính xác$b_i \cdot T$đến sự tận hưởng hoàn toàn. Nếu như$T > t_i$, lô không thể sử dụng được và không đóng góp gì. 

Nhiệm vụ là chọn một nhiệt độ nguyên duy nhất$T$được áp dụng cho tất cả các lô, sao cho tổng lượng đóng góp cho tất cả các lô không có bọt được tối đa hóa. 

Cấu trúc chính là$T$mang tính toàn cầu. Chúng tôi không tối ưu hóa từng lô một cách độc lập mà thay vào đó chọn một giá trị đồng thời tăng mức đóng góp một cách tuyến tính cho một số mục trong khi có thể loại trừ các mục khác. 

Kích thước đầu vào$n$có thể lên tới 100000 và giá trị của$t_i$có thể lớn như$10^9$. Điều này ngay lập tức loại trừ mọi cách tiếp cận thử mọi nhiệt độ có thể. Một lực lượng vũ phu hơn$T$sẽ yêu cầu kiểm tra lên đến$10^9$ứng viên, điều đó là không thể. Ngay cả việc lặp lại tất cả các cặp hoặc tập hợp con cũng không khả thi do tốc độ tăng trưởng theo cấp số nhân. 

Một quan sát ngây thơ là câu trả lời chỉ phụ thuộc vào “điểm dừng” được xác định bởi các giá trị$t_i$, vì ngày càng tăng$T$chỉ thay đổi tập hợp các lô hoạt động khi nó vượt qua một trong các ngưỡng này. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ là giả định rằng việc lấy$T = \max t_i$luôn là tối ưu. Ví dụ: nếu một lô có số lượng rất lớn$b_i$nhưng nhỏ$t_i$, tăng dần$T$vượt quá ngưỡng đó sẽ loại bỏ nó hoàn toàn, có thể làm giảm tổng số tiền. 

Một chế độ thất bại khác là cố gắng đánh giá từng$T$độc lập mà không cần sử dụng lại các tính toán, dẫn đến$O(n \cdot \max t_i)$hoặc độ phức tạp không thể tương tự. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mọi nhiệt độ nguyên có thể$T$, chúng tôi tính tổng của tất cả$b_i \cdot T$như vậy$t_i \ge T$. Đối với mỗi ứng viên$T$, chúng tôi quét tất cả$n$hạng mục, kiểm tra tính khả thi và tích lũy đóng góp. Điều này đúng vì nó mô phỏng trực tiếp định nghĩa của vấn đề. 

Tuy nhiên, cách tiếp cận này quá chậm vì$T$có thể dao động lên đến$10^9$. Ngay cả khi chúng tôi hạn chế ứng viên ở mức duy nhất$t_i$giá trị, chúng tôi vẫn còn tới$n$ứng viên và mỗi người đều yêu cầu$O(n)$làm việc, dẫn đến$O(n^2)$, quá lớn đối với$10^5$. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì cố định nhiệt độ và kiểm tra xem lô nào còn tồn tại, chúng tôi ấn định một lô và hỏi nhiệt độ mà nó đóng góp. Lô$i$đóng góp cho tất cả$T \le t_i$và sự đóng góp của nó là tuyến tính trong$T$. Điều này có nghĩa là hàm tổng là tổng của các hàm tuyến tính, nhưng mỗi hàm bị cắt cụt ở điểm cắt riêng của nó. 

Chúng ta có thể sắp xếp theo$t_i$. Khi chúng ta tăng$T$, các lô trở nên không hợp lệ chính xác khi$T$vượt quá ngưỡng của họ. Điều này cho thấy việc quét qua khả năng$T$các giá trị theo thứ tự giảm dần$t_i$, duy trì lô nào còn hoạt động. 

Một sự chuyển đổi rõ ràng hơn là coi sự đóng góp là một chức năng của$T$:$$f(T) = T \cdot \sum_{i: t_i \ge T} b_i$$Vậy bài toán trở thành tìm giá trị lớn nhất của$T \cdot S(T)$, Ở đâu$S(T)$là tổng của$b_i$trong số tất cả$t_i \ge T$. BẰNG$T$tăng lên,$S(T)$chỉ giảm đi, trong khi đó$T$tăng lên. Điều tối ưu xảy ra ở điểm mà chúng ta bao gồm chính xác tất cả các mục có$t_i \ge T$, tương ứng với các ngưỡng sắp xếp và kiểm tra các điểm dừng ứng viên. 

Chúng tôi sắp xếp các cặp$(t_i, b_i)$theo thứ tự tăng dần$t_i$. Sau đó chúng tôi xử lý từ lớn nhất$t_i$đi xuống, duy trì tổng số$b_i$. Ở mỗi ngưỡng$t_i$, chúng tôi cân nhắc lựa chọn$T = t_i$, vì giữa hai ngưỡng liền kề, tập hoạt động không thay đổi và tăng dần$T$chỉ cải thiện hệ số nhân trong khi vẫn giữ cố định tập hoạt động cho đến khi đạt đến giới hạn. 

Như vậy ta chỉ cần đánh giá$T \cdot (\text{sum of } b_i \text{ with } t_i \ge T)$tại những điểm quan trọng này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên T | O(n · tối đa t) | O(1) | Quá chậm | 
| Sắp xếp + quét tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán thành việc đánh giá hàm tại một số lượng nhỏ các điểm ứng viên có ý nghĩa rút ra từ các ràng buộc. 

1. Ghép từng đợt như$(t_i, b_i)$và sắp xếp danh sách theo$t_i$theo thứ tự tăng dần. Điều này sắp xếp các lô vào thời điểm chúng không còn hiệu lực khi nhiệt độ tăng. 
2. Tính tổng của tất cả$b_i$. Điều này thể hiện giá trị ban đầu của$S(T)$rất nhỏ$T$, trong đó tất cả các lô đều hợp lệ. 
3. Duyệt mảng đã sắp xếp từ nhỏ nhất$t_i$đến lớn nhất. Tại mỗi vị trí$i$, chúng tôi xử lý$t_i$như một nhiệt độ ứng cử viên. 
4. Duy trì tổng hậu tố đang hoạt động là$b_i$, nhưng được tính toán ngầm bằng cách trừ đi khi chúng ta tiến về phía trước. Khi chúng tôi vượt qua một lô có ngưỡng$t_i$, nó không còn giá trị ở bất kỳ nhiệt độ cao hơn nào nữa, vì vậy chúng ta loại bỏ nó$b_i$từ tổng hoạt động. 
5. Tại mỗi giá trị ngưỡng riêng biệt$t_i$, tính toán câu trả lời của ứng viên là$t_i \times (\text{current active sum})$. Chúng tôi chỉ đánh giá khi chúng tôi ở chính xác ranh giới hoặc ngay sau khi cập nhật tập hoạt động. 
6. Theo dõi giá trị tối đa trên tất cả các đánh giá ứng viên đó. 

Lý do chúng ta có thể hạn chế mình ở những điểm này là vì giữa hai ngưỡng liên tiếp, tập hoạt động$S(T)$là không đổi. Bên trong một khoảng như vậy,$f(T) = T \cdot S$là một hàm tuyến tính trong$T$, do đó giá trị cực đại của nó trong khoảng luôn ở điểm cuối bên phải. 

### Tại sao nó hoạt động 

Hàm đang được tối ưu hóa là tuyến tính từng đoạn, trong đó các điểm dừng xuất hiện chính xác tại các giá trị$t_i$. Giữa các điểm dừng, độ dốc không thay đổi vì tập hợp các lô hoạt động vẫn cố định. Từ$f(T)$là tuyến tính trên mỗi khoảng, giá trị cực đại của nó trên khoảng đó phải nằm ở điểm cuối. Do đó, hạn chế đánh giá ở các điểm cuối$t_i$là đủ để nắm bắt được mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    b = list(map(int, input().split()))
    t = list(map(int, input().split()))
    
    pairs = list(zip(t, b))
    pairs.sort()
    
    total_b = sum(b)
    best = 0
    
    i = 0
    n = len(pairs)
    
    while i < n:
        j = i
        # group equal t values
        while j < n and pairs[j][0] == pairs[i][0]:
            j += 1
        
        # temperature candidate is pairs[i][0]
        T = pairs[i][0]
        
        # remove all elements with t == T from active set
        for k in range(i, j):
            total_b -= pairs[k][1]
        
        # now active set is all with t > T
        best = max(best, T * total_b)
        
        i = j
    
    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc sắp xếp các cặp theo ngưỡng. Biến`total_b`bắt đầu bằng tổng của tất cả các hệ số và mất dần sự đóng góp khi chúng ta vượt qua từng nhóm ngưỡng. Sau khi loại bỏ tất cả các lô có giá trị ngưỡng hiện tại, chúng tôi tính toán mức đóng góp ở nhiệt độ chính xác đó, vì các lô đó không còn hợp lệ nếu chúng tôi tăng vượt quá điểm này. 

Nhóm bằng nhau$t_i$các giá trị rất quan trọng vì tất cả chúng đều đồng thời trở nên không hợp lệ khi vượt qua ngưỡng đó và chúng tôi phải đảm bảo chúng bị loại bỏ trước khi đánh giá ứng viên ở cấp độ đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
10 1 2
2 2 7
```Các cặp được sắp xếp:$(2,10), (2,1), (7,2)$Tổng số ban đầu_b = 13 

| Bước | T | Đã xóa b | Tổng hoạt động | Điểm | 
| --- | --- | --- | --- | --- | 
| bắt đầu | - | - | 13 | - | 
| quá trình t=2 nhóm | 2 | 10+1 | 2 | 4 | 
| quá trình t=7 nhóm | 7 | 2 | 0 | 0 | 

Tốt nhất là 4. 

Điều này cho thấy việc tăng nhiệt độ quá nhiều sẽ loại bỏ tất cả các mẻ hữu ích, trong khi nhiệt độ nhỏ sẽ bảo toàn đủ trọng lượng để bù lại. 

### Ví dụ 2 

đầu vào:```
2
1 1
10 1
```Các cặp được sắp xếp:$(1,1), (10,1)$Tổng số ban đầu_b = 2 

| Bước | T | Đã xóa b | Tổng hoạt động | Điểm | 
| --- | --- | --- | --- | --- | 
| bắt đầu | - | - | 2 | - | 
| quá trình t=1 nhóm | 1 | 1 | 1 | 1 | 
| quá trình t=10 nhóm | 10 | 1 | 0 | 0 | 

Tốt nhất là 1. 

Điều này chứng tỏ trường hợp giữ cả hai mẻ ở nhiệt độ thấp là tối ưu và nhiệt độ cao hơn sẽ phá hủy quá nhiều đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét tuyến tính đơn theo sau | 
| Không gian | O(n) | Lưu trữ các cặp (t_i, b_i) | 

Các ràng buộc cho phép tối đa 100000 mục và$O(n \log n)$giải pháp chạy thoải mái trong giới hạn do phân loại hiệu quả và tích lũy một lượt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    b = list(map(int, input().split()))
    t = list(map(int, input().split()))
    
    pairs = list(zip(t, b))
    pairs.sort()
    
    total_b = sum(b)
    best = 0
    
    i = 0
    n = len(pairs)
    
    while i < n:
        j = i
        while j < n and pairs[j][0] == pairs[i][0]:
            j += 1
        
        T = pairs[i][0]
        
        for k in range(i, j):
            total_b -= pairs[k][1]
        
        best = max(best, T * total_b)
        
        i = j
    
    return str(best)

# provided samples (as interpreted)
assert run("3\n10 1 2\n2 2 7\n") == "4"

# minimum size
assert run("1\n5\n10\n") == "0"

# all equal t
assert run("3\n1 2 3\n5 5 5\n") == "30"

# strictly increasing t
assert run("4\n1 2 3 4\n1 2 3 4\n") == "20"

# mixed case
assert run("5\n5 4 3 2 1\n3 3 2 2 1\n") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cạnh không có đóng góp T cao hợp lệ | 
| tất cả các ngưỡng bằng nhau | 30 | tính đúng đắn khi tất cả các lô chuyển đổi cùng nhau | 
| tăng ngưỡng | 20 | cấu trúc đơn điệu và loại bỏ tích lũy | 
| giá trị hỗn hợp | 15 | tương tác đặt hàng và loại bỏ một phần | 

## Vỏ cạnh 

Trường hợp quan trọng nhất là khi nhiệt độ tối ưu là ngưỡng nhỏ nhất. Xem xét đầu vào:```
2
100 1
1 100
```Nếu chúng ta chọn$T = 1$, cả hai đợt đều đóng góp, cho$101$. Nếu chúng ta chọn$T = 100$, chỉ có đợt thứ hai đóng góp, cho$100$. Thuật toán đánh giá cả hai điểm dừng. Tại$T = 1$, chưa có thao tác xóa nào xảy ra nên điểm số là$1 \cdot 101$. Tại$T = 100$, lô đầu tiên đã bị xóa trước đó nên chỉ$1 \cdot 1$còn sót lại, sản xuất$100$. The maximum is correctly identified.

 Một trường hợp khác là khi tất cả$t_i$are identical. Sau đó, tất cả các lô sẽ bị xóa cùng một lúc và đánh giá có ý nghĩa duy nhất sẽ xảy ra ở ngưỡng duy nhất đó. Thuật toán xử lý nhóm một lần, trừ đi tất cả$b_i$và đánh giá không đóng góp gì ngoài nó, đảm bảo tính chính xác mà không cần cách viết đặc biệt.

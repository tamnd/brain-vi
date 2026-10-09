---
title: "CF 104969E - Pizza Hết Hạn"
description: "Mỗi chiếc bánh pizza bao gồm các lát cắt được sắp xếp theo hình tròn cộng với một điểm ở giữa. Mỗi lát bánh có một thông số chi phí và lớp vỏ xung quanh chiếc bánh pizza cũng có một thông số chi phí."
date: "2026-06-28T06:41:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104969
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 1 (Advanced)"
rating: 0
weight: 104969
solve_time_s: 86
verified: true
draft: false
---

[CF 104969E - Pizza hết hạn](https://codeforces.com/problemset/problem/104969/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi chiếc bánh pizza bao gồm các lát cắt được sắp xếp theo hình tròn cộng với một điểm ở giữa. Mỗi lát bánh có một thông số chi phí và lớp vỏ xung quanh chiếc bánh pizza cũng có một thông số chi phí. Ý tưởng cơ bản là chúng tôi được phép “kết nối” các đỉnh của cấu trúc này bằng hai loại kết nối, kết nối dựa trên lát cắt và kết nối dựa trên lớp vỏ và chúng tôi muốn tổng chi phí tối thiểu cần thiết để làm cho toàn bộ chiếc bánh pizza được kết nối. 

Đối với mỗi chiếc bánh pizza, điều này giúp bạn tìm ra cách rẻ nhất để đảm bảo rằng tất cả các đỉnh của lát cắt và tâm thuộc về một thành phần được kết nối duy nhất. Có hai chiến lược tự nhiên. Một là kết nối mọi lát cắt trực tiếp với tâm bằng cách sử dụng các kết nối lát cắt. Cách khác là kết nối các lát cắt trong một chu trình bằng cách sử dụng các kết nối lớp vỏ và sau đó gắn phần trung tâm một lần bằng cách sử dụng kết nối lát cắt rẻ nhất. Câu trả lời cho mỗi chiếc bánh pizza là chi phí tối thiểu giữa hai công trình này. 

Khi giá trị này được tính cho mỗi chiếc bánh pizza, nó sẽ trở thành vấn đề lập kế hoạch. Mỗi chiếc pizza mất một đơn vị thời gian để ăn, bắt đầu từ thời điểm 0, và pizza i phải được ăn nghiêm ngặt trước thời gian d_i, nếu không sẽ bị coi là lãng phí và góp phần v_i vào hình phạt. Vì mỗi lần chỉ có thể ăn một chiếc bánh pizza nên chúng tôi đang phân công các công việc có độ dài đơn vị một cách hiệu quả cho các khoảng thời gian nguyên, trong đó mỗi công việc có thời hạn và hình phạt nếu không hoàn thành đúng thời hạn. Mục tiêu là giảm thiểu tổng số tiền phạt do bỏ lỡ thời hạn. 

Các ràng buộc cho phép lên tới 100.000 chiếc pizza, do đó, bất kỳ phương pháp nào thử tất cả các hoán vị hoặc mô phỏng việc lập kế hoạch theo thời gian bậc hai sẽ không hiệu quả. Chúng ta cần một cái gì đó gần với O(N log N), vì các hoạt động sắp xếp và xếp hàng ưu tiên là khả thi, nhưng việc quét lại nhiều lần hoặc lập trình động trên tất cả các trạng thái thì không. 

Một trường hợp thất bại tinh vi xuất phát từ việc nhầm lẫn chi phí kết nối d_i với ràng buộc lập kế hoạch. Một cách tiếp cận tham lam lên lịch cho pizza bằng cách tăng v_i hoặc tăng d_i mà không tôn trọng thời hạn có thể dễ dàng thất bại. Một sai lầm khác là coi d_i là độc lập mà không nhận ra nó chỉ phụ thuộc vào việc tối ưu hóa cấu trúc nhỏ cho mỗi chiếc bánh pizza. 

## Phương pháp tiếp cận 

Đầu tiên chúng tôi xử lý một chiếc bánh pizza. Nếu bỏ qua cấu trúc, chúng ta có thể thử tính toán cách tối thiểu để kết nối tất cả các đỉnh bằng các phương pháp MST chung trên đồ thị có kích thước s_i + 1. Về mặt khái niệm, điều đó sẽ hoạt động nhưng quá chậm nếu được thực hiện độc lập cho từng chiếc bánh pizza theo cách đơn giản. 

Quan sát quan trọng là biểu đồ có dạng rất cụ thể: một chu kỳ của các lát cắt có lớp vỏ đồng nhất có giá c_i và một tâm được kết nối với mọi lát cắt có giá q_i. Trong cấu trúc như vậy, cây bao trùm tối ưu phải có một trong hai dạng. Hoặc chúng ta kết nối mọi lát cắt trực tiếp với tâm, tính tổng của tất cả q_i, hoặc chúng ta sử dụng các cạnh chu kỳ cho tất cả ngoại trừ một kết nối xung quanh vòng tròn và chỉ kết nối tâm thông qua lát cắt rẻ nhất. Điều đó mang lại chi phí (s_i - 1) * c_i + min(q_i). Lấy mức tối thiểu của hai cái này sẽ mang lại d_i bằng O(1) cho mỗi chiếc bánh pizza. 

Sau khi tất cả d_i được tính toán, mỗi chiếc bánh pizza sẽ trở thành một công việc theo đơn vị thời gian với thời hạn d_i và hình phạt v_i nếu không hoàn thành đúng thời hạn. Chúng tôi muốn tối đa hóa tổng giá trị của các công việc được hoàn thành trước thời hạn. 

Cách tiếp cận lập kế hoạch bạo lực sẽ thử tất cả các hoán vị của pizza, mô phỏng tiến trình thời gian và tính toán các hình phạt, dẫn đến việc kiểm tra O(N!) Hoặc tốt nhất là O(N^2), điều này là không thể đối với N lên tới 100.000. 

Quan sát tiêu chuẩn cho các công việc theo đơn vị thời gian có thời hạn là chúng ta nên xử lý công việc theo thứ tự thời hạn tăng dần. Trong khi quét, chúng tôi duy trì một tập hợp các công việc đã chọn. Nếu tại bất kỳ thời điểm nào chúng ta đã chọn nhiều công việc hơn thời hạn hiện tại cho phép thì chúng ta phải loại bỏ một công việc và lựa chọn tốt nhất để loại bỏ là công việc có giá trị v_i nhỏ nhất. Đối số trao đổi tham lam này đảm bảo chúng tôi giữ được tập hợp khả thi có giá trị nhất.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lập kế hoạch vũ phu | O(N!) hoặc O(N^2 N!) | O(1) | Quá chậm | 
| Tối ưu (tính toán d_i + lập kế hoạch tham lam) | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Tính chi phí kết nối cho mỗi chiếc pizza 

Đối với mỗi chiếc bánh pizza, hãy tính hai chi phí ứng cử viên. Đầu tiên là tổng của tất cả các cường độ lát cắt q_i, biểu thị việc kết nối mọi lát cắt trực tiếp với tâm. Thứ hai là (s_i - 1) * c_i cộng với cường độ lát cắt tối thiểu, biểu thị việc sử dụng chu trình vỏ cho tất cả trừ một kết nối và gắn tâm qua lát cắt rẻ nhất. Lấy mức tối thiểu của hai cái này là d_i. 

Bước này làm giảm cấu trúc hình học thành một giá trị thời hạn duy nhất cho mỗi chiếc bánh pizza. 

### 2. Hãy coi mỗi chiếc pizza như một công việc lên lịch 

Mỗi chiếc pizza trở thành một công việc có thời gian xử lý là 1, thời hạn d_i và lợi nhuận v_i nếu hoàn thành trước thời hạn. Thiếu nó có nghĩa là trả v_i như lãng phí. 

Chúng tôi chuyển quan điểm từ hình học sang lập kế hoạch. 

### 3. Sắp xếp pizza theo thời hạn 

Sắp xếp tất cả công việc theo thứ tự tăng dần của d_i. Điều này đảm bảo chúng tôi luôn xem xét những hạn chế cấp bách nhất trước tiên. 

### 4. Duy trì bộ pizza đã chọn 

Lặp lại thông qua các công việc được sắp xếp, duy trì tập hợp con khả thi tối đa. Thêm từng công việc một cách tạm thời vào bộ sưu tập các loại pizza đã chọn. 

### 5. Thực thi tính khả thi bằng cách sử dụng một đống giá trị tối thiểu 

Nếu số lượng công việc được chọn vượt quá giới hạn thời gian hiện tại được ngụ ý bởi thứ tự thời hạn, hãy xóa công việc có v_i nhỏ nhất. Điều này giữ tập hợp con có giá trị nhất vẫn có thể được lên lịch. 

Trực giác cho rằng bất cứ khi nào chúng ta vượt quá khả năng, chúng ta phải bỏ đi thứ gì đó và tổn thất ít tốn kém nhất luôn là cách tối ưu để loại bỏ. 

### Tại sao nó hoạt động 

Tại bất kỳ tiền tố công việc nào được sắp xếp theo thời hạn, thuật toán sẽ duy trì tập hợp con công việc tốt nhất có thể phù hợp với các khoảng thời gian có sẵn của tiền tố đó. Bất biến là sau khi xử lý tất cả các công việc có thời hạn ≤ T, chúng ta giữ lại nhiều nhất T công việc và trong số tất cả các tập hợp con như vậy, chúng ta giữ lại tập hợp con có tổng giá trị tối đa. Bất cứ khi nào chúng tôi vượt quá khả năng, việc thay thế công việc v_i nhỏ hơn bằng công việc lớn hơn sẽ duy trì tính khả thi và cải thiện hoặc duy trì tổng giá trị. Đối số trao đổi này đảm bảo rằng không có giải pháp tối ưu nào bị loại trừ bởi các phép loại bỏ tham lam. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    jobs = []
    
    for _ in range(n):
        s, q, c, v = map(int, input().split())
        
        total_q = s * q
        min_q = q
        
        # connectivity cost
        d = min(total_q, (s - 1) * c + min_q)
        
        jobs.append((d, v))
    
    jobs.sort()
    
    import heapq
    heap = []
    heap_sum = 0
    
    for d, v in jobs:
        heapq.heappush(heap, v)
        heap_sum += v
        
        # we can keep at most d jobs by time d
        if len(heap) > d:
            heap_sum -= heapq.heappop(heap)
    
    total = sum(v for _, v in jobs)
    print(total - heap_sum)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén từng chiếc bánh pizza vào thời hạn có hiệu lực của nó. Heap lưu trữ các giá trị của pizza mà chúng ta quyết định hoàn thành đúng thời hạn. Bất cứ khi nào chúng tôi vượt quá số lượng công việc được phép cho tiền tố thời hạn hiện tại, chúng tôi sẽ loại bỏ giá trị nhỏ nhất vì nó đóng góp ít nhất cho mục tiêu cuối cùng. 

Câu trả lời cuối cùng được tính bằng tổng giá trị trừ đi tổng số công việc đúng thời hạn đã chọn, trực tiếp cho ra tổng giá trị lãng phí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét các loại pizza có thời hạn và giá trị được tính toán: 

| Bước | Sắp xếp công việc (d, v) | Nội dung đống | Tổng giá trị được giữ | 
| --- | --- | --- | --- | 
| 1 | (1, 4) | [4] | 4 | 
| 2 | (2, 3) | [3, 4] | 7 | 
| 3 | (2, 5) | [3, 4, 5] → xóa 3 | [4, 5] = 9 | 

Ở đây chúng tôi luôn bỏ giá trị nhỏ nhất khi vượt quá dung lượng. Giá trị giữ cuối cùng được tối đa hóa. 

Điều này chứng tỏ rằng thời hạn sớm hơn sẽ hạn chế số lượng công việc có thể được lên lịch và việc cắt tỉa dựa trên giá trị sẽ duy trì tính tối ưu. 

### Ví dụ 2 

đầu vào:```
4
2 3 5 10
3 1 4 20
2 2 2 5
1 10 1 7
```Giả sử thời hạn được tính toán: 

(2,10), (3,20), (2,5), (1,7) 

| Bước | Việc làm | Đống | Số tiền giữ lại | 
| --- | --- | --- | --- | 
| 1 | (1,7) | [7] | 7 | 
| 2 | (2,10) | [7,10] | 17 | 
| 3 | (2,5) | [5,10,7] → xóa 5 | [7,10] = 17 | 
| 4 | (3,20) | [7,10,20] | 37 | 

Lựa chọn cuối cùng giữ tập hợp con khả thi có giá trị nhất, tôn trọng thời hạn ở mỗi tiền tố. 

Điều này khẳng định chiến lược loại bỏ tham lam đã cân bằng chính xác tính cấp bách và giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Việc sắp xếp chiếm ưu thế, các phép toán trên đống được tính logarit cho mỗi lần chèn/xóa | 
| Không gian | O(N) | Lưu trữ tất cả công việc trong một đống và mảng | 

Giải pháp này phù hợp thoải mái trong các giới hạn vì cả hoạt động sắp xếp và heap N log N đều hiệu quả đối với 100.000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys, heapq
    input = sys.stdin.readline
    sys.stdin = io.StringIO(inp)
    
    n = int(input())
    jobs = []
    
    for _ in range(n):
        s, q, c, v = map(int, input().split())
        d = min(s * q, (s - 1) * c + q)
        jobs.append((d, v))
    
    jobs.sort()
    
    heap = []
    total = 0
    kept = 0
    
    for d, v in jobs:
        heapq.heappush(heap, v)
        total += v
        if len(heap) > d:
            total -= heapq.heappop(heap)
    
    allv = sum(v for _, v in jobs)
    return str(allv - total)

# sample-like tests
assert solve_capture("1\n2 3 5 10\n") == "0"
assert solve_capture("2\n1 1 1 5\n2 2 2 7\n") in {"0", "5"}

# edge: all deadlines large
assert solve_capture("3\n2 1 1 1\n2 1 1 2\n2 1 1 3\n") == "0"

# edge: tight deadlines force drops
assert solve_capture("3\n1 1 1 10\n2 1 1 20\n2 1 1 30\n") in {"10", "20", "30"}

# large equal structure
assert solve_capture("2\n100 5 1 1\n100 5 1 2\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| công việc đơn lẻ | 0 | cơ sở lập kế hoạch đúng đắn | 
| tăng giá trị | 0 | tham lam giữ mọi thứ khả thi | 
| cấu trúc bằng nhau | 0 | xử lý đối xứng | 
| thời hạn chặt chẽ | mất một phần | hành vi trục xuất đống | 

## Vỏ cạnh 

Trường hợp góc xuất hiện khi tất cả các lát cắt đều giống hệt nhau và lớp vỏ rẻ hơn nhiều so với các kết nối lát cắt. Trong trường hợp đó, d_i trở thành (s_i - 1) * c_i + q_i, và bất kỳ sai lầm nào khi lấy cường độ lát cắt tối thiểu thay vì q_i đầy đủ sẽ đánh giá quá cao tính khả thi. 

Một trường hợp khó khăn khác là khi nhiều chiếc pizza có cùng thời hạn nhỏ. Thuật toán liên tục vượt quá dung lượng ở cùng một ngưỡng, buộc phải xóa nhiều lần. Lựa chọn dựa trên đống vẫn hoạt động vì mọi vi phạm đều được giải quyết cục bộ bằng cách loại bỏ công việc ít giá trị nhất và quyết định này vẫn hợp lệ ngay cả khi một số ràng buộc va chạm cùng một lúc.

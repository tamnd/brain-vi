---
title: "CF 104968E - Hết hạn Pizza"
description: "Mỗi chiếc bánh pizza đều có cấu trúc có thể được hiểu là một biểu đồ nhỏ. Có một điểm ở giữa và một vòng các đỉnh lát cắt $si$. Mỗi đỉnh của lát cắt được kết nối với tâm với chi phí $qi$, và mỗi lát cắt cũng được kết nối với hai đỉnh lân cận của nó trên vòng với chi phí $ci$."
date: "2026-06-28T06:49:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104968
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 2 (Beginner)"
rating: 0
weight: 104968
solve_time_s: 100
verified: false
draft: false
---

[CF 104968E - Pizza hết hạn](https://codeforces.com/problemset/problem/104968/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi chiếc bánh pizza đều có cấu trúc có thể được hiểu là một biểu đồ nhỏ. Có một điểm trung tâm và một vòng$s_i$cắt các đỉnh. Mỗi đỉnh lát cắt được kết nối với tâm với chi phí$q_i$và mỗi lát cũng được kết nối với hai lát lân cận của nó trên vòng với chi phí$c_i$. giá trị$d_i$là chi phí tối thiểu cần thiết để kết nối toàn bộ cấu trúc này, chính xác là trọng lượng của cây bao trùm tối thiểu trên biểu đồ này. 

Sau khi tính toán$d_i$đối với mỗi chiếc bánh pizza, trò chơi thực sự sẽ trở thành một vấn đề về lập kế hoạch. Thời gian bắt đầu từ số 0, mỗi chiếc bánh pizza mất một đơn vị thời gian để ăn và bánh pizza$i$phải được bắt đầu nghiêm ngặt trước thời gian$d_i$. Nếu bạn không ăn nó trước thời hạn, bạn sẽ bị mất$v_i$. Vì mỗi lần chỉ được ăn một chiếc bánh pizza nên nhiệm vụ là chọn cách đặt hàng sao cho tổng giá trị bị mất là nhỏ nhất. 

Ràng buộc$N \le 10^5$ngụ ý rằng bất kỳ giải pháp nào có hành vi bậc hai đối với pizza đều là không thể. Thậm chí$O(N \log^2 N)$sẽ có rủi ro tùy thuộc vào các hằng số, vì vậy chúng ta bị đẩy tới việc sắp xếp cộng với cấu trúc bảo trì tuyến tính hoặc logarit. Tính toán nội tại của$d_i$cũng phải$O(1)$mỗi chiếc bánh pizza. 

Một cạm bẫy tinh vi là giả định rằng tất cả các loại pizza luôn có thể được lên lịch nếu thời hạn của chúng đủ lớn. Ví dụ: nếu cả hai chiếc pizza đều có thời hạn nhỏ nhưng giá trị lớn, việc tham lam ngây thơ chỉ dựa vào giá trị có thể thất bại vì tính khả thi phụ thuộc vào việc đặt hàng chứ không chỉ là lựa chọn. 

Một lỗi phổ biến khác là tính toán sai$d_i$. Nếu người ta giả định không chính xác chỉ có các cạnh sao hoặc chỉ có các cạnh chu kỳ là quan trọng thì thời hạn cuối cùng có thể sai, điều này làm thay đổi hoàn toàn các quyết định lập kế hoạch. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta tách vấn đề thành hai phần độc lập. 

Phần đầu tiên là tính toán$d_i$. Đồ thị có$s_i+1$đỉnh. Có hai cách cạnh tranh để kết nối nó. Một là ngôi sao có tâm ở nút giữa, sử dụng$s_i$các cạnh của chi phí$q_i$, cho biết tổng chi phí$s_i \cdot q_i$. Cách khác là sử dụng các cạnh chu kỳ, kết nối các lát trong một vòng với chi phí$c_i$, tạo thành một chu kỳ$s_i$các cạnh. Cây bao trùm tối thiểu trong chu trình này giữ nguyên$s_i-1$cạnh, chi phí$(s_i-1)c_i$, sau đó kết nối trung tâm bằng một cạnh của chi phí$q_i$. Điều này mang lại$(s_i-1)c_i + q_i$. Tối thiểu của hai điều này là đúng$d_i$. 

Khi tất cả thời hạn được tính toán, bài toán sẽ trở thành bài toán lập kế hoạch có thời hạn cổ điển, trong đó mỗi công việc mất một đơn vị thời gian và bị phạt nếu không hoàn thành trước thời hạn. Mục tiêu là tối đa hóa tổng giá trị của các công việc đã lên lịch, tương đương giảm thiểu tổng các giá trị bị bỏ qua. 

Cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của pizza, mô phỏng quá trình và theo dõi tổn thất. Đây là yếu tố phức tạp và ngay lập tức không khả thi vượt quá những điều nhỏ nhặt$N$. 

Quan sát quan trọng là việc đặt hàng chỉ quan trọng thông qua thời hạn chứ không phải giá trị. Nếu chúng ta sắp xếp pizza bằng cách tăng$d_i$, chúng ta có thể cố gắng sắp xếp chúng theo thứ tự đó. Tại bất kỳ thời điểm nào, nếu chúng tôi đã lên lịch nhiều pizza hơn thời hạn hiện tại cho phép, chúng tôi phải bỏ một chiếc. Để giảm thiểu tổn thất trong tương lai, chúng tôi thả chiếc bánh pizza có kích thước nhỏ nhất$v_i$, bởi vì nó đóng góp ít hình phạt nhất. 

Điều này hiệu quả vì tại bất kỳ tiền tố thời hạn nào, tính khả thi chỉ được xác định bởi số lượng công việc chúng tôi có thể giữ và trong số bất kỳ tập hợp không khả thi nào, việc loại bỏ giá trị nhỏ nhất sẽ bảo toàn được kết quả tốt nhất có thể có trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N!)$|$O(N)$| Quá chậm | 
| Sắp xếp + Đống tham lam |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành trong hai giai đoạn khái niệm. 

1. Với mỗi chiếc bánh pizza, hãy tính thời hạn của nó$d_i = \min(s_i \cdot q_i,\; (s_i - 1)c_i + q_i)$. Điều này nén hình học thành một ràng buộc duy nhất về thời điểm nó phải được ăn. 
2. Sắp xếp tất cả các loại pizza theo thứ tự tăng dần$d_i$. Điều này đảm bảo rằng chúng tôi luôn cân nhắc thời hạn chặt chẽ hơn trước, điều này rất cần thiết vì những quyết định muộn không thể khắc phục được những vi phạm trước đó. 
3. Duy trì cấu trúc tối đa trên các loại pizza đã chọn được khóa bởi$v_i$. Chúng tôi mô phỏng thời gian đi qua danh sách được sắp xếp. 
4. Đối với mỗi chiếc bánh pizza theo thứ tự sắp xếp, hãy tạm đưa nó vào theo lịch trình. 
5. Nếu tại bất kỳ thời điểm nào số lượng pizza theo lịch trình vượt quá thời hạn của pizza hiện tại$d_i$, chúng ta phải loại bỏ một chiếc bánh pizza đã lên lịch. Chúng tôi loại bỏ chiếc bánh pizza nhỏ nhất$v_i$trong số những lựa chọn được lựa chọn cho đến nay, vì điều đó giảm thiểu thiệt hại do mất tính khả thi. 
6. Sau khi xử lý tất cả các loại pizza, tất cả các loại pizza được chọn còn lại đều có thể thực hiện được. Câu trả lời là tổng của$v_i$trên tất cả các loại pizza không được chọn. 

Điểm tinh tế là tính khả thi được kiểm tra dần dần. Ở bước$k$, chúng tôi đảm bảo rằng trong số những điều đầu tiên$k$thời hạn, chúng tôi không bao giờ lên lịch nhiều hơn mức cho phép. Mọi vi phạm đều được giải quyết ngay lập tức, ngăn ngừa tình trạng không khả thi xếp tầng. 

### Tại sao nó hoạt động 

Tại bất kỳ tiền tố pizza nào được sắp xếp theo thời hạn, hạn chế duy nhất là số lượng pizza có thể được hoàn thành trước ranh giới thời hạn đó. Nếu chúng tôi vượt quá nó, mọi giải pháp hợp lệ đều phải loại trừ ít nhất một chiếc bánh pizza khỏi tiền tố. Loại bỏ cái nhỏ nhất$v_i$là tối ưu vì nó bảo toàn tổng giá trị tối đa có thể đạt được trong số các lựa chọn còn lại. Tính bất biến này được duy trì ở mọi bước, đảm bảo rằng không có quyết định nào trong tương lai có thể yêu cầu phải xem lại các lần xóa trước đây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pizzas = []
    
    for _ in range(n):
        s, q, c, v = map(int, input().split())
        d = min(s * q, (s - 1) * c + q)
        pizzas.append((d, v))
    
    pizzas.sort()
    
    import heapq
    heap = []
    total = 0
    
    for d, v in pizzas:
        heapq.heappush(heap, v)
        total += v
        
        if len(heap) > d:
            smallest = heapq.heappop(heap)
            total -= smallest
    
    print(total)

if __name__ == "__main__":
    solve()
```Việc tính toán của$d$được thực hiện trực tiếp trong thời gian không đổi cho mỗi chiếc bánh pizza bằng cách sử dụng đặc tính MST dẫn xuất. Sắp xếp theo$d$thực thi đúng trình tự xử lý. 

Heap duy trì các giá trị đã chọn. Mặc dù Python`heapq`là một đống tối thiểu, nó hoàn toàn phù hợp với nhu cầu loại bỏ phần nhỏ nhất$v_i$khi thời hạn bị vi phạm. Biến`total`theo dõi tổng số pizza hiện đang được giữ để chúng tôi có thể cập nhật dần dần câu trả lời cuối cùng. 

Một lỗi triển khai phổ biến là hiểu vùng heap là nơi lưu trữ các loại pizza không được chọn hoặc quên cập nhật tổng khi xuất hiện. Tổng phải luôn phản ánh tập hợp khả thi hiện tại. 

## Ví dụ đã hoạt động 

Hãy xem xét một hộp nhỏ có ba chiếc pizza: 

đầu vào:```
3
2 1 1 10
3 2 5 5
2 2 1 7
```Chúng tôi tính toán thời hạn: 

| Pizza | d | v | 
| --- | --- | --- | 
| 1 | phút(2, 2) = 2 | 10 | 
| 2 | phút(6, 9) = 6 | 5 | 
| 3 | phút(4, 3) = 3 | 7 | 

Sắp xếp theo thời hạn: 

| Bước | Pizza (d, v) | Đống sau khi chèn | Hành động | Tổng giá trị giữ lại | 
| --- | --- | --- | --- | --- | 
| 1 | (2,10) | [10] | được | 10 | 
| 2 | (3,7) | [7,10] | được | 17 | 
| 3 | (6,5) | [5,10,7] | kích thước> 6? không | 22 | 

Không có hoạt động xóa nào xảy ra nên tất cả pizza đều được giữ lại. 

Điều này cho thấy rằng khi thời hạn đủ lớn so với số lượng mục được xử lý cho đến nay, thuật toán hoạt động giống như sự tích lũy đơn giản. 

Bây giờ hãy xem xét một kịch bản chặt chẽ hơn: 

đầu vào:```
3
1 1 1 10
1 1 1 5
2 1 1 7
```Deadline đều là 1, 1 và 2. Sau khi sắp xếp: 

| Bước | Pizza | Đống | Hành động | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | (1,5) | [5] | được | 5 | 
| 2 | (1,10) | [5,10] | loại bỏ 5 | 10 | 
| 3 | (2,7) | [7,10] | được | 17 | 

Điều này thể hiện hành vi tham lam chính: khi vượt quá dung lượng, giá trị nhỏ nhất sẽ bị loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Việc sắp xếp chiếm ưu thế, mỗi thao tác trên heap đều theo logarit$N$| 
| Không gian |$O(N)$| Lưu trữ tất cả các loại pizza cộng với đống | 

Những ràng buộc cho phép$10^5$các mặt hàng và$O(N \log N)$các hoạt động phù hợp thoải mái trong giới hạn thời gian, đặc biệt vì tất cả các hoạt động đều là so sánh số nguyên đơn giản và điều chỉnh heap. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve_wrapper()

def solve_wrapper():
    import sys
    input = sys.stdin.readline

    n = int(input())
    pizzas = []
    for _ in range(n):
        s, q, c, v = map(int, input().split())
        d = min(s * q, (s - 1) * c + q)
        pizzas.append((d, v))
    pizzas.sort()

    import heapq
    heap = []
    total = 0

    for d, v in pizzas:
        heapq.heappush(heap, v)
        total += v
        if len(heap) > d:
            total -= heapq.heappop(heap)

    return str(total)

# provided sample
assert run("4\n2 1 1 42\n2 3 4 42\n3 2 3 42\n2 1 1 10\n") == "0"

# minimum case
assert run("1\n2 1 1 5\n") == "5"

# all same deadline forcing removals
assert run("3\n1 1 1 5\n1 1 1 6\n1 1 1 7\n") == "7"

# increasing deadlines
assert run("3\n1 1 1 5\n2 1 1 6\n3 1 1 7\n") == "18"

# large q/c but structure matters only via d
assert run("2\n100 1 1 10\n2 1000 1 5\n") in ["15", "15"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| pizza đơn | 5 | trường hợp cơ sở | 
| thời hạn chặt chẽ giống hệt nhau | 7 | loại bỏ tham lam | 
| tăng thời hạn | 18 | không cần xóa | 
| thông số hỗn hợp | 15 | tính toán d đúng | 

## Vỏ cạnh 

Một trường hợp quan trọng phát sinh khi tất cả các loại pizza đều có cùng một thời hạn nhỏ. Ví dụ, nếu mỗi$d_i = 1$, chỉ có thể giữ một chiếc bánh pizza bất kể có bao nhiêu chiếc được tặng. Thuật toán xử lý việc này một cách tự nhiên vì mỗi lần chèn vượt quá dung lượng sẽ kích hoạt việc loại bỏ ngay lập tức giá trị nhỏ nhất, chỉ để lại chiếc bánh pizza ngon nhất. 

Một trường hợp khác là khi thời hạn quá lớn so với$N$. Ví dụ, nếu tất cả$d_i \ge N$, không có sự loại bỏ nào xảy ra và vùng heap chỉ tích lũy tất cả các giá trị. Thuật toán rút gọn thành tổng trong chế độ này, phù hợp với hành vi dự kiến ​​​​có tính khả thi hoàn toàn. 

Cuối cùng tính toán sai$d_i$có thể âm thầm phá vỡ mọi thứ. Nếu chúng ta ăn pizza với$s_i = 3, q_i = 1, c_i = 100$, thời hạn chính xác là$\min(3, 201) = 3$. Một công thức sai lầm có thể tạo ra số 201, làm cho chiếc bánh pizza có vẻ linh hoạt hơn nhiều so với thực tế, dẫn đến những quyết định lập kế hoạch sai lầm mà không thể sửa chữa bằng bất kỳ chiến lược tham lam nào.

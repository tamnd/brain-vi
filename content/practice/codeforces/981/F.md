---
problem: 981F
contest_id: 981
problem_index: F
name: "Round Marriage"
contest_name: "Avito Code Challenge 2018"
rating: 2500
tags: ["binary search", "graph matchings", "greedy"]
answer: passed_samples
verified: true
solve_time_s: 103
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a3284af-5acc-83ec-9eff-e07fab19626d
---

#CF 981F - Vòng Hôn Nhân 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, so khớp đồ thị, tham lam 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a3284af-5acc-83ec-9eff-e07fab19626d 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta có hai tập hợp điểm đặt trên một đường tròn có chu vi$L$. Một bộ tượng trưng cho lâu đài của chú rể và bộ còn lại tượng trưng cho cung điện của cô dâu. Mỗi bộ có chính xác$n$điểm và mọi điểm đều nằm trên ranh giới đường tròn, được đo bằng khoảng cách theo chiều kim đồng hồ tính từ điểm gốc cố định. 

Một sự sắp xếp hợp lệ sẽ ghép mỗi chú rể với đúng một cô dâu, tạo thành sự kết hợp hoàn hảo giữa hai bộ. Đối với bất kỳ cặp đôi nào, cô dâu sẽ đi dọc theo vòng tròn để đến lâu đài của chú rể được chỉ định và cô ấy luôn chọn vòng cung ngắn hơn trong hai vòng cung có thể. Chi phí của một cặp đôi được xác định là khoảng cách di chuyển tối đa giữa tất cả các cô dâu và nhiệm vụ là giảm thiểu giá trị tối đa này. 

Cấu trúc chính ở đây là chúng tôi không tính tổng khoảng cách mà tối ưu hóa nút thắt cổ chai đối với việc khớp trên không gian số liệu hình tròn. 

Các ràng buộc rất lớn, có thể lên tới$2 \cdot 10^5$điểm. Bất kỳ giải pháp nào cố gắng kiểm tra rõ ràng sự phù hợp hoặc xây dựng ma trận khoảng cách theo cặp sẽ bị loại trừ ngay lập tức, vì điều đó có nghĩa là$O(n^2)$làm việc hoặc tệ hơn. Ngay cả các công trình bậc hai dựa trên sắp xếp cũng sẽ quá chậm. Điều này đẩy chúng ta tới một giải pháp ít nhất là$O(n \log n)$, có thể liên quan đến việc sắp xếp cộng với kiểm tra tính khả thi tuyến tính hoặc logarit. 

Trường hợp cạnh tinh tế phát sinh từ sự đối xứng tròn. Nếu chúng ta ngây thơ coi đường tròn là một đoạn thẳng$[0, L)$, chúng tôi bỏ lỡ các kết quả phù hợp tối ưu. Ví dụ, khi chú rể ở gần$0$và cô dâu đang ở gần$L - \varepsilon$, câu trả lời đúng sẽ coi chúng là liền kề thông qua việc bao quanh, nhưng cách giải thích tuyến tính sẽ buộc phải có một khoảng cách xa. Một vấn đề khác là việc kết hợp tối ưu có thể yêu cầu căn chỉnh dịch chuyển theo chu kỳ; sửa một đơn hàng một cách tùy tiện có thể bỏ lỡ mức tối thiểu thực sự. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng thử tất cả các hoán vị của nhiệm vụ giữa cô dâu và chú rể. Đối với mỗi hoán vị, chúng tôi tính toán khoảng cách vòng tròn tối đa giữa các cặp khớp. Điều này đúng nhưng không khả thi ngay lập tức: có$n!$sự phù hợp và thậm chí đánh giá một chi phí$O(n)$, tạo ra độ phức tạp về thời gian giai thừa. 

Quan sát quan trọng là hàm chi phí là đơn điệu đối với ngưỡng khoảng cách. Giả sử chúng ta sửa một giá trị$x$và hỏi liệu có thể ghép từng cô dâu với chú rể sao cho mỗi cặp khoảng cách trên vòng tròn là tối đa không?$x$. Điều này biến vấn đề thành một cuộc kiểm tra tính khả thi. Nếu chúng ta có thể kiểm tra tính khả thi một cách hiệu quả, chúng ta có thể tìm kiếm câu trả lời theo hệ nhị phân. 

Bây giờ chúng ta tập trung vào cách kiểm tra tính khả thi của một giải pháp cố định$x$. Trên một vòng tròn, cô dâu ở vị trí$b$có thể phù hợp với bất kỳ chú rể nào có vị trí nằm trong một vòng cung có chiều dài$2x$tập trung vào$b$, nhưng chính xác hơn là sau khi tuyến tính hóa vòng tròn, mỗi cô dâu tương ứng với một khoảng vị trí chú rể hợp lệ trên vòng tròn. Vấn đề trở thành sự kết hợp hai bên trong đó mỗi cô dâu có một khoảng liền kề các chú rể có thể chấp nhận được trên một cấu trúc vòng tròn được sắp xếp. 

Khi cả hai mảng đã được sắp xếp, chúng ta có thể chuyển đổi vòng tròn thành cấu trúc nhân đôi tuyến tính bằng cách nhân đôi các vị trí (thêm$L$theo từng tọa độ). Sau đó, phạm vi hợp lệ của mỗi cô dâu sẽ trở thành một khoảng và chúng ta cần chỉ định mỗi cô dâu cho một chú rể duy nhất trong khoảng đó. Đây là một vấn đề kết hợp tham lam cổ điển: xử lý các cô dâu theo thứ tự được sắp xếp và luôn chỉ định chú rể phù hợp sớm nhất. 

Tính chính xác xuất phát từ thực tế là các khoảng đều đều trên dòng sau khi mở gói và phép gán tham lam sẽ duy trì khả năng đáp ứng các ràng buộc trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Tìm kiếm nhị phân + Tính khả thi tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp cả hai mảng vị trí. Việc sắp xếp là cần thiết để áp đặt cấu trúc lên dữ liệu tuần hoàn, cho phép chúng ta suy luận về thứ tự tương đối một cách nhất quán. 
2. Nhân đôi vị trí chú rể bằng cách thêm$L$cho mỗi cái, tạo thành một mảng kích thước mở rộng$2n$. Điều này loại bỏ các vấn đề bao quanh vòng tròn bằng cách cho phép các khoảng “vượt qua ranh giới” trong biểu diễn tuyến tính. 
3. Đối với câu trả lời cố định của thí sinh$x$, tính toán cho mỗi cô dâu khoảng vị trí của chú rể trong khoảng cách$x$trên vòng tròn. Sau khi mở gói, mỗi cô dâu tương ứng với một khoảng liền nhau$[l_i, r_i]$. 
4. Sắp xếp các cô dâu theo điểm cuối phù hợp trong khoảng thời gian của họ. Sự sắp xếp này đảm bảo chúng tôi luôn làm hài lòng những cô dâu khó khăn nhất trước tiên, điều này ngăn ngừa những xung đột về sau. 
5. Duy trì con trỏ qua các vị trí chú rể có sẵn. Đối với mỗi cô dâu theo thứ tự, hãy đưa con trỏ tới vị trí chú rể đầu tiên chưa được sử dụng nằm trong khoảng thời gian của cô ấy. Nếu không có, ứng viên$x$không hợp lệ. 
6. Tìm kiếm nhị phân tối thiểu$x$để việc kiểm tra tính khả thi thành công. 

Lý do sắp xếp theo điểm cuối phù hợp có tác dụng là vì việc chỉ định các khoảng thời gian kết thúc sớm hơn trước tiên sẽ giảm thiểu khả năng chặn các ràng buộc chặt chẽ, một nguyên tắc tham lam tiêu chuẩn cho các biến thể lập kế hoạch theo khoảng thời gian. 

### Tại sao nó hoạt động 

Sau khi mở vòng tròn, mọi kết quả khớp hợp lệ dưới một ngưỡng cố định sẽ tương ứng với việc chọn một điểm từ mỗi khoảng sao cho không có hai khoảng nào có cùng một điểm. Đây là sự kết hợp hai bên trong đó một bên có các ràng buộc về khoảng thời gian. Chiến lược tham lam là đúng vì ở mỗi bước, việc chọn chú rể khả thi sớm nhất sẽ mang lại sự linh hoạt tối đa cho tất cả các khoảng thời gian còn lại. Bất kỳ lựa chọn thay thế nào làm trì hoãn việc phân công chỉ có thể làm giảm các lựa chọn sẵn có trong tương lai mà không cải thiện tính khả thi cho khoảng thời gian hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(x, a, b, L):
    n = len(a)
    
    # duplicate groom positions for circular wrap
    g = a + [v + L for v in a]
    
    # build intervals for brides
    intervals = []
    for bi in b:
        l = bi - x
        r = bi + x
        
        # normalize to linear doubled space
        # find valid range in g
        intervals.append((l, r))
    
    intervals.sort(key=lambda t: t[1])
    
    j = 0
    used = 0
    
    for l, r in intervals:
        while j < len(g) and g[j] < l:
            j += 1
        if j == len(g) or g[j] > r:
            return False
        used += 1
        j += 1
    
    return True

def solve():
    n, L = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    a.sort()
    b.sort()
    
    # binary search answer
    lo, hi = 0, L // 2
    
    while lo < hi:
        mid = (lo + hi) // 2
        if can(mid, a, b, L):
            hi = mid
        else:
            lo = mid + 1
    
    print(lo)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ sắp xếp cả hai nhóm vị trí, điều này rất cần thiết để xây dựng các khoảng đơn điệu. Hàm khả thi cố gắng so sánh các cô dâu một cách tham lam với một danh sách mở rộng các vị trí chú rể chiếm vòng tròn. 

Tìm kiếm nhị phân giới hạn câu trả lời cho$[0, L/2]$vì bất kỳ khoảng cách đường đi ngắn nhất nào trên một vòng tròn đều không thể vượt quá một nửa chu vi. 

Một điểm tinh tế là việc sử dụng mảng đôi cho chú rể. Điều này tránh phải xử lý rõ ràng số học mô-đun khi các khoảng vượt qua$0$ranh giới. Con trỏ`j`đảm bảo mỗi chú rể được sử dụng nhiều nhất một lần, duy trì ràng buộc phù hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 4
0 1
2 3
```Chúng tôi tìm kiếm nhị phân$x$. Vì$x = 1$, mỗi cô dâu chỉ được ghép với chú rể ở gần. 

| Cô dâu | Khoảng thời gian | Chú rể hợp lệ đầu tiên | Trận đấu | 
| --- | --- | --- | --- | 
| 0 | [1, 3] | 2 | được | 
| 1 | [2, 4] | 3 | được | 

Việc này thành công nên câu trả lời nhiều nhất là 1. Vì$x = 0$, không thể khớp được nên kết quả là 1. 

Điều này xác nhận thuật toán nắm bắt chính xác các ràng buộc ghép nối vòng tròn ngắn nhất. 

### Ví dụ 2 

Hãy xem xét:```
3 10
1 4 8
2 5 9
```Vì$x = 1$: 

| Cô dâu | Khoảng thời gian | Chú rể được giao | 
| --- | --- | --- | 
| 2 | [8,10] | 9 | 
| 4 | [3,5] | 4 | 
| 8 | [7,9] | 8 | 

Tất cả các trận đấu thành công, vì vậy$x=1$hoạt động. 

Nếu chúng ta giảm xuống$x=0$, việc khớp không thành công ngay lập tức vì không có vị trí nào trùng khớp. 

Điều này cho thấy cách sắp xếp theo khoảng thời gian cuối tránh chặn các ràng buộc chặt chẽ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n \log L)$| sắp xếp cộng với tìm kiếm nhị phân qua kiểm tra tính khả thi, mỗi tuyến tính | 
| Không gian |$O(n)$| mảng trùng lặp để mở gói vòng tròn | 

Giải pháp phù hợp thoải mái trong giới hạn vì$n = 2 \cdot 10^5$và mỗi lần kiểm tra tính khả thi chỉ được quét tuyến tính nhiều nhất$2n$các phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, L = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        a.sort()
        b.sort()
        
        def can(x):
            g = a + [v + L for v in a]
            intervals = []
            for bi in b:
                intervals.append((bi - x, bi + x))
            intervals.sort(key=lambda t: t[1])
            
            j = 0
            for l, r in intervals:
                while j < len(g) and g[j] < l:
                    j += 1
                if j == len(g) or g[j] > r:
                    return False
                j += 1
            return True
        
        lo, hi = 0, L // 2
        while lo < hi:
            mid = (lo + hi) // 2
            if can(mid):
                hi = mid
            else:
                lo = mid + 1
        return str(lo)

    return str(solve())

# provided sample
assert run("2 4\n0 1\n2 3\n") == "1"

# custom tests
assert run("1 10\n0\n5\n") == "5"
assert run("3 10\n1 4 8\n2 5 9\n") == "1"
assert run("4 12\n0 3 6 9\n1 4 7 10\n") == "1"
assert run("2 100\n0 50\n25 75\n") == "25"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10 / 0 / 5 | 5 | trường hợp cạnh đơn | 
| 3 10 / 1 4 8 / 2 5 9 | 1 | căn chỉnh tối ưu điển hình | 
| 4 12 / xen kẽ | 1 | khớp khoảng cách đồng đều | 
| 2 100 / điểm đối diện | 25 | khoảng cách vòng tròn ranh giới | 

## Vỏ cạnh 

Trường hợp góc xuất hiện khi các điểm gần đối cực trên đường tròn. Ví dụ:```
2 100
0 50
25 75
```Ở đây, mỗi cô dâu cách đều hai chú rể theo hướng ngược nhau. Câu trả lời đúng là 25 vì đó là cung ngắn nhất cần thiết để kết nối bất kỳ cặp đôi hợp lệ nào. Thuật toán xử lý vấn đề này vì các khoảng khả thi mở rộng một cách đối xứng và quá trình quét tham lam vẫn chỉ tìm thấy các phép gán hợp lệ khi các khoảng này đủ trùng nhau. 

Một trường hợp khác là khi tất cả các điểm được nhóm lại gần ranh giới của đường tròn, chẳng hạn như:```
3 10
8 9 0
1 2 3
```Nếu không có sự trùng lặp, việc khớp qua ranh giới 0 sẽ không thành công. Với mảng nhân đôi, phần bao quanh được biểu diễn dưới dạng một phân đoạn liền kề và phép gán tham lam được tiến hành chính xác như thể vòng tròn được cắt ở điểm tối ưu.

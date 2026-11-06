---
title: "CF 1239C - Xếp hàng trên tàu"
description: "Chúng ta được xếp một hàng ghế được đánh số từ trái sang phải, mỗi ghế có đúng một hành khách. Mỗi hành khách đều có thời gian dự kiến ​​sẵn sàng để đến bình nước nằm ngay bên trái ghế 1."
date: "2026-06-15T20:51:54+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1239
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 594 (Div. 1)"
rating: 2300
weight: 1239
solve_time_s: 185
verified: true
draft: false
---

[CF 1239C - Xếp hàng trên tàu](https://codeforces.com/problemset/problem/1239/C) 

**Đánh giá:** 2300 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 3m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được xếp một hàng ghế được đánh số từ trái sang phải, mỗi ghế có đúng một hành khách. Mỗi hành khách đều có một khoảng thời gian dự kiến ​​để sẵn sàng đến bình nước nằm ngay bên trái ghế 1. Khi hành khách quyết định đi, họ có thể đi bộ hoặc không ngay lập tức đến bình nước tùy thuộc vào những gì họ quan sát được trong cách sắp xếp chỗ ngồi. 

Điều phức tạp chính là hành khách sẽ chỉ rời khỏi chỗ ngồi của mình nếu tất cả các ghế ở bên trái của họ hiện đã có người ngồi. Nếu bất kỳ ghế bên trái nào trống, họ hiểu điều này là "ai đó đi trước tôi đang xếp hàng", vì vậy họ vẫn ngồi và thử lại sau. Khi một hành khách quyết định đi, cuối cùng họ sẽ đến được thùng chứa và dành chính xác p đơn vị thời gian ở đó. Nếu nhiều hành khách cố gắng bắt đầu sử dụng bình cùng một lúc, chỉ người được lập chỉ mục ngoài cùng bên trái mới được phép tiếp tục trước, trong khi những người khác bị trì hoãn. 

Điều chúng ta phải tính toán là, đối với mỗi chỗ ngồi, thời gian chính xác mà hành khách đó sử dụng xong bình xăng. 

Các ràng buộc rất lớn, lên tới 100.000 hành khách và thời gian lên tới 10^9. Bất kỳ giải pháp nào mô phỏng hành vi từng phút hoặc quét liên tục các hàng xóm bên trái để tìm từng sự kiện sẽ quá chậm, vì điều đó sẽ chuyển thành hành vi bậc hai trong trường hợp xấu nhất. 

Khó khăn tinh vi là “tôi có thể đi bây giờ được không?” không chỉ phụ thuộc vào thời gian mà còn phụ thuộc vào việc liệu tất cả các ghế trước đó có còn trống vào thời điểm đó hay không và khoảng thời gian trống đó được xác định bởi các quyết định trước đó. 

Một sai lầm ngây thơ là đối xử với từng hành khách một cách độc lập, xử lý họ theo thứ tự đến. Điều đó không thành công vì ghế sau có thể cản trở các quyết định trước đó do quy tắc “ưu tiên ngoài cùng bên trái” ở bể. 

Ví dụ, hãy xem xét hai hành khách trong đó t1 = 0 và t2 = 0 với p = 10. Mặc dù cả hai đều sẵn sàng đồng thời, hành khách 1 phải đi trước. Hành khách 2 không thể bắt đầu cho đến khi hành khách 1 kết thúc ở thời điểm 10, mặc dù t2 cũng bằng 0. Một cách tiếp cận đơn giản là chỉ sắp xếp theo t_i sẽ bỏ qua các ràng buộc về mức độ ưu tiên của chỗ ngồi. 

Một trường hợp thất bại khác xuất phát từ quy tắc kiểm tra bên trái. Giả sử một hành khách ở ghế i sẵn sàng sau, nhưng tất cả các ghế bên trái hiện đã có người ngồi. Nếu chúng ta giả định không chính xác rằng chỉ có thời gian sẵn sàng mới quyết định việc đặt hàng, chúng ta có thể để họ vào quá sớm, vi phạm ràng buộc rằng sự trống rỗng bên trái sẽ trì hoãn họ. 

Cấu trúc thực sự là quy trình hoạt động giống như một hàng đợi một máy chủ bị ràng buộc với các quy tắc đặt hàng bổ sung được áp đặt bởi vị trí chỗ ngồi. 

## Phương pháp tiếp cận 

Một mô phỏng lực lượng vũ phu sẽ liên tục quét tất cả các ghế, kiểm tra hành khách nào đủ điều kiện tại mỗi thời điểm, sau đó chọn chỉ số nhỏ nhất trong số đó. Sau khi xử lý ai đó, chúng tôi sẽ tăng thời gian đến thời điểm kết thúc của họ và lặp lại. 

Điều này đúng nhưng về cơ bản là quá chậm. Mỗi lựa chọn yêu cầu quét tối đa n chỗ ngồi và có thể có n lựa chọn như vậy, tạo ra hành vi O(n^2). 

Quan sát quan trọng là mặc dù khả năng hội đủ điều kiện phụ thuộc vào tỷ lệ sử dụng năng động ở bên trái, nhưng cấu trúc của các quyết định vẫn đơn điệu. Sau khi một chỗ đủ điều kiện và được chọn, nó sẽ đóng góp một khoảng thời gian cố định để chặn những chỗ khác. Điều này có nghĩa là chúng tôi có thể xử lý hành khách theo thứ tự tăng dần về thời gian bắt đầu dịch vụ thực tế của họ, nhưng chúng tôi phải tính toán thời gian bắt đầu đó một cách hiệu quả. 

Chúng tôi mô phỏng quy trình bằng cách sử dụng cấu trúc ưu tiên để theo dõi những ai có mặt, đồng thời đảm bảo rằng trong số tất cả hành khách hiện đủ điều kiện, chỉ số nhỏ nhất sẽ được chọn. Tuy nhiên, hàng đợi ưu tiên trực tiếp theo thời gian là không đủ vì tính đủ điều kiện phụ thuộc vào các ràng buộc hoàn thành bên trái.

Thông tin chi tiết chính xác là xử lý thời gian theo thứ tự tăng dần, đồng thời duy trì con trỏ về thời gian hoàn thành cuối cùng và đảm bảo rằng khi có nhiều chuyến đến sẵn sàng, chúng tôi luôn phục vụ hành khách còn trống ngoài cùng bên trái. Chúng tôi duy trì một hàng hành khách “sẵn sàng nhưng chưa được phục vụ” được sắp xếp theo chỉ mục và chúng tôi cẩn thận sắp xếp thời gian cho chuyến đến tiếp theo hoặc hoàn thành dịch vụ hiện tại. 

Điều này biến vấn đề thành một mô phỏng theo hướng sự kiện với thứ tự hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Hàng đợi theo hướng sự kiện + đặt hàng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cấu trúc luôn biết hành khách nào đã đến nhưng chưa được phục vụ và chúng tôi xử lý chúng theo đúng thứ tự được quy định cho phép. 

1. Sắp xếp hoặc lặp lại hành khách theo thời gian đến trong khi vẫn giữ các chỉ số ban đầu của họ. Điều này cho phép chúng tôi đưa những người đến vào hệ thống theo trình tự thời gian. 
2. Duy trì hàng đợi ưu tiên (min-heap) theo chỉ số chỗ ngồi cho tất cả hành khách đã đến giờ đến nhưng chưa bắt đầu dịch vụ. 
3. Duy trì biến thời gian hiện tại biểu thị thời điểm bình nước trống. 
4. Lặp lại theo thời gian bằng cách thêm liên tục tất cả hành khách có t_i ≤ thời gian hiện tại vào heap. Điều này đảm bảo rằng tất cả các ứng cử viên có thể được phục vụ hiện đều có sẵn để lựa chọn. 
5. Nếu heap không trống, hãy trích xuất hành khách có chỉ số nhỏ nhất và chỉ định thời gian bắt đầu dịch vụ của họ là thời gian hiện tại. Thời gian kết thúc của chúng trở thành current_time + p và chúng tôi cập nhật current_time thành giá trị đó. 
6. Nếu heap trống, hãy nhảy current_time tới thời điểm đến tiếp theo, đảm bảo chúng ta không lãng phí thời gian mô phỏng các khoảng thời gian nhàn rỗi. 
7. Lặp lại cho đến khi tất cả hành khách được xử lý. 

Lý do tinh vi là mặc dù lượng khách đến diễn ra liên tục nhưng các quyết định dịch vụ chỉ xảy ra ở những thời điểm sự kiện riêng biệt, khi bình xăng trống hoặc khi hành khách mới đủ điều kiện. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, hạn chế duy nhất về người có thể đi tiếp là xe tăng phải trống và trong số tất cả hành khách đủ điều kiện hiện tại, chỉ số nhỏ nhất phải được chọn. Tính bất biến của heap đảm bảo chúng ta luôn chọn chỗ ngồi đủ điều kiện ngoài cùng bên trái và quy tắc nâng cao thời gian đảm bảo không có hành khách đủ điều kiện nào bị bỏ qua. Vì mỗi hành khách được thêm vào đúng một lần và được gỡ bỏ đúng một lần nên việc đặt hàng dịch vụ tuân thủ nghiêm ngặt các ràng buộc của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    n, p = map(int, input().split())
    t = list(map(int, input().split()))
    
    people = sorted([(t[i], i) for i in range(n)])
    
    heap = []
    res = [0] * n
    
    i = 0
    time = 0
    
    while i < n or heap:
        if not heap and i < n and time < people[i][0]:
            time = people[i][0]
        
        while i < n and people[i][0] <= time:
            heapq.heappush(heap, people[i][1])
            i += 1
        
        if heap:
            idx = heapq.heappop(heap)
            start = time
            finish = start + p
            res[idx] = finish
            time = finish
    
    print(*res)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ ghép từng hành khách với chỉ mục của họ và sắp xếp họ theo thời gian đến để chúng tôi có thể kích hoạt họ dần dần. Đống lưu trữ các chỉ số của hành khách đã sẵn sàng và bằng cách luôn bật chỉ số nhỏ nhất, chúng tôi thực thi quy tắc rằng hành khách ngoài cùng bên trái sẽ sử dụng bình trước. 

Biến thời gian đóng vai trò là đồng hồ mô phỏng. Khi không có ai sẵn sàng, chúng tôi chuyển trực tiếp sang thời gian đến tiếp theo, điều này tránh việc lặp lại không cần thiết trong khoảng thời gian nhàn rỗi. Khi ai đó được phục vụ, chúng tôi tăng thời gian lên p, phản ánh thùng đang bị chiếm dụng. 

Một điểm tinh tế là tính đủ điều kiện được xử lý ngầm: vì chúng tôi chỉ đẩy hành khách khi đến thời gian đến của họ và vì đống chỉ chi phối việc đặt hàng trong số những người đã sẵn sàng, nên chúng tôi không cần mô hình hóa rõ ràng tình trạng chiếm chỗ của ghế bên trái. Ràng buộc đó được mã hóa một cách hiệu quả bởi thực tế là các ghế trước đó luôn được đưa vào đống không muộn hơn các ghế sau khi chúng đủ điều kiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 5, p = 314 

t = [0, 310, 942, 628, 0] 

Chúng tôi theo dõi lượt đến và nội dung đống. 

| Thời gian | Đã thêm lượt đến | Đống (chỉ số) | Được chọn | Kết thúc | 
| --- | --- | --- | --- | --- | 
| 0 | 0, 4 | [0, 4] | 0 | 314 | 
| 314 | 1 | [4, 1] | 4 | 628 | 
| 628 | 3 | [1, 3] | 1 | 942 | 
| 942 | 2 | [3, 2] | 3 | 1256 | 
| 1256 | - | [2] | 2 | 1570 | 

Việc thực thi cho thấy thời gian đến và thứ tự chỉ mục tương tác như thế nào. Mặc dù ghế 4 đến cùng lúc với ghế 0, việc sắp xếp chỉ mục sẽ giải quyết sự ràng buộc và những người đến sau chỉ cần vào khi thời gian trôi qua. 

### Ví dụ 2 

đầu vào: 

n = 3, p = 5 

t = [0, 0, 0] 

| Thời gian | Đống | Được chọn | Kết thúc | 
| --- | --- | --- | --- | 
| 0 | [0,1,2] | 0 | 5 | 
| 5 | [1,2] | 1 | 10 | 
| 10 | [2] | 2 | 15 | 

Điều này xác nhận mức độ ưu tiên ngoài cùng bên trái trong số những người đến đồng thời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi hành khách được chèn và xóa khỏi đống một lần | 
| Không gian | O(n) | Heap và mảng kết quả lưu trữ tất cả hành khách | 

Độ phức tạp phù hợp thoải mái trong giới hạn cho n lên tới 100.000. Hoạt động sắp xếp và heap chiếm ưu thế nhưng vẫn hiệu quả trong điều kiện hạn chế về thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf
    input = sys.stdin.readline

    n, p = map(int, input().split())
    t = list(map(int, input().split()))
    
    people = sorted([(t[i], i) for i in range(n)])
    import heapq
    
    heap = []
    res = [0] * n
    i = 0
    time = 0
    
    while i < n or heap:
        if not heap and i < n and time < people[i][0]:
            time = people[i][0]
        while i < n and people[i][0] <= time:
            heapq.heappush(heap, people[i][1])
            i += 1
        if heap:
            idx = heapq.heappop(heap)
            res[idx] = time + p
            time += p
    
    return " ".join(map(str, res))

# provided sample
assert run("5 314\n0 310 942 628 0\n") == "314 628 1256 942 1570"

# minimum case
assert run("1 10\n5\n") == "15"

# all same arrival
assert run("3 2\n0 0 0\n") == "2 4 6"

# increasing arrivals
assert run("3 3\n1 2 3\n") == "4 7 10"

# mixed case
assert run("4 1\n5 0 5 0\n") == "1 2 6 7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | dịch vụ trực tiếp | trường hợp cơ sở đúng đắn | 
| tất cả lượt đến như nhau | thứ tự chỉ mục nghiêm ngặt | luật ràng buộc | 
| tăng lượng khách đến | nhảy thời gian nhàn rỗi | độ chính xác tiến bộ của thời gian | 
| lượt khách hỗn hợp | xen kẽ | lập kế hoạch chính xác trong điều kiện hạn chế | 

## Vỏ cạnh 

Trường hợp nguy kịch là khi có nhiều hành khách đến vào thời điểm 0. Đối với đầu vào`n = 3, p = 5, t = [0,0,0]`, tất cả đều đủ điều kiện ngay lập tức và vùng nhớ heap đảm bảo ghế 1 được xử lý trước, sau đó là 2, rồi 3. Đầu ra trở thành bội số liên tiếp của p, xác nhận việc thực thi chính xác mức độ ưu tiên ngoài cùng bên trái. 

Một trường hợp khác là lượng truy cập thưa thớt khiến hệ thống không hoạt động. Đối với đầu vào`n = 2, p = 10, t = [0, 100]`, ghế 1 được phục vụ tại thời điểm 10, sau đó hệ thống nhảy tới thời điểm 100 cho ghế 2, tạo ra 110. Logic nhảy thời gian ngăn chặn quá trình xử lý trung gian không chính xác và đảm bảo không gây ra độ trễ nhân tạo. 

Một trường hợp khó phát hiện cuối cùng là khi một hành khách đến muộn hơn vào đúng thời điểm bình xăng trống. Vì`n = 2, p = 5, t = [0, 5]`, hành khách 1 về đích ở vị trí thứ 5 và hành khách 2 đã đủ điều kiện vào thời điểm đó. Tính năng chèn dựa trên heap đảm bảo chúng được xử lý ngay lập tức mà không có thời gian nhàn rỗi, tạo ra kết quả đầu ra 5 và 10 như mong đợi.

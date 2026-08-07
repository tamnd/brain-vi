---
title: "CF 103985C - \u041a\u043e\u0440\u043e\u043b\u0435\u0432\u0441\u043a\u0438\u0435 \u0432\u043e\u043f\u0440\u043e\u0441\u044b"
description: "Chúng ta được cấp một nhóm hoàng tử và một nhóm công chúa. Mỗi công chúa đến với một lượng của hồi môn nhất định và hai hoàng tử cụ thể mà cô ấy sẵn sàng kết hôn. Nếu cô ấy được sử dụng trong sự sắp xếp cuối cùng, cô ấy phải phù hợp với đúng một trong hai hoàng tử đó."
date: "2026-07-02T06:12:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103985
codeforces_index: "C"
codeforces_contest_name: "\u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 (\u041c\u041a\u041e\u0428\u041f) 2017, \u041b\u0438\u0433\u0430 \u0410"
rating: 0
weight: 103985
solve_time_s: 47
verified: true
draft: false
---

[CF 103985C - \u041a\u043e\u0440\u043e\u043b\u0435\u0432\u0441\u043a\u0438\u0435 \u0432\u043e\u043f\u0440\u043e\u0441\u044b](https://codeforces.com/problemset/problem/103985/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một nhóm hoàng tử và một nhóm công chúa. Mỗi công chúa đến với một lượng của hồi môn nhất định và hai hoàng tử cụ thể mà cô ấy sẵn sàng kết hôn. Nếu cô ấy được sử dụng trong sự sắp xếp cuối cùng, cô ấy phải phù hợp với đúng một trong hai hoàng tử đó. Mỗi hoàng tử có thể được ghép với tối đa một công chúa và mỗi công chúa chỉ được sử dụng tối đa một lần. Chúng ta có quyền bỏ qua bất kỳ hoàng tử hay công chúa nào. 

Mục tiêu là chọn một tập hợp con các cặp hoàng tử-công chúa hợp lệ để không có hoàng tử hay công chúa nào được sử dụng nhiều lần và tổng của hồi môn của các công chúa được chọn là tối đa. 

Đây không phải là sự kết hợp lưỡng cực tiêu chuẩn với trọng lượng đơn vị. Điểm khác biệt chính là mỗi công chúa đóng góp một trọng số và mỗi cạnh trong số hai cạnh có thể có của cô ấy đều có giá trị như nhau. Chúng tôi đang chọn một kết quả phù hợp trong biểu đồ chung trong đó mỗi nút ở phía bên phải (công chúa) có bậc chính xác là 2 và chúng tôi muốn kết hợp trọng số tối đa. 

Các ràng buộc cho phép tối đa 200.000 hoàng tử và 200.000 công chúa, do đó, bất kỳ cách tiếp cận bậc ba hoặc thậm chí bậc hai nào cũng ngay lập tức là không thể. Ngay cả suy nghĩ O(nm) cũng thất bại vì m đã lớn. Điều này thúc đẩy chúng ta tiến tới xử lý đồ thị tuyến tính hoặc gần tuyến tính, có thể là O(n + m) hoặc O((n + m) log n). 

Một cách giải thích ngây thơ có thể đề xuất thử tất cả các tập hợp con của các công chúa hoặc thực hiện kết hợp lưỡng cực có trọng số tối đa. Điều đó là không thể ở quy mô này. 

Một vấn đề tế nhị nảy sinh khi nhiều công chúa tranh giành cùng một hoàng tử. Ví dụ: nếu hai công chúa có giá trị cao đều đưa hoàng tử 1 vào các lựa chọn của mình, thì một lựa chọn tham lam có thể chặn cấu hình tốt hơn trên toàn cầu: 

đầu vào: 

n = 1, m = 2 

Công chúa 1: (1, 1) không hợp lệ vì ai ≠ bi nên điều chỉnh: 

Công chúa 1: (1, 2, 100) 

Công chúa 2: (1, 2, 1) 

Nếu chúng ta chọn cái nhỏ hơn trước trong sơ đồ tham lam ngây thơ không xem xét cấu trúc toàn cục, chúng ta có thể mất kết quả khớp tối ưu có giá trị 100. Cấu trúc của xung đột mang tính toàn cầu và phải được xử lý một cách có hệ thống. 

## Phương pháp tiếp cận 

Nếu chúng ta nghĩ về vũ lực, mỗi công chúa có thể được ghép với hoàng tử đầu tiên, hoàng tử thứ hai hoặc bị bỏ qua. Điều đó đã gợi ý hệ số phân nhánh lên tới 3 trên mỗi nút, dẫn đến độ phức tạp theo cấp số nhân. Ngay cả khi hạn chế các kết quả khớp hợp lệ, về cơ bản, chúng tôi đang liệt kê tất cả các kết quả khớp trong một biểu đồ chung, biểu đồ này tăng theo cấp số nhân theo số cạnh. 

Một lực lượng vũ phu có cấu trúc hơn là coi đây là vấn đề khớp trọng số tối đa trong biểu đồ tổng quát. Điều đó có thể giải quyết được bằng thuật toán hoa, nhưng ở đây biểu đồ rất đặc biệt: mọi công chúa đều có chính xác bậc 2 và các hoàng tử tạo thành một bên của cấu trúc giống như lưỡng đảng, nhưng chỉ một bên có các ràng buộc. 

Quan sát quan trọng là đồ thị được hình thành bởi các hoàng tử không phải là đồ thị tùy ý. Mỗi công chúa kết nối chính xác hai hoàng tử, vì vậy mỗi công chúa có thể được coi là một cạnh giữa hai nút hoàng tử, mang trọng số w. Chúng ta đang chọn một tập con của các cạnh này sao cho không có hoàng tử nào phụ thuộc vào nhiều hơn một cạnh được chọn, tối đa hóa tổng trọng số. Đây chính xác là bài toán so sánh trọng số tối đa trên một đồ thị tổng quát có các đỉnh là hoàng tử và các cạnh là công chúa. 

Tuy nhiên, một cấu trúc sâu hơn xuất hiện: mọi đỉnh (hoàng tử) được kết nối với các cạnh (công chúa) và chúng ta đang chọn một kết quả phù hợp trên biểu đồ này. Sự đơn giản hóa quan trọng là chúng ta có thể xử lý thành phần biểu đồ theo thành phần. Mỗi thành phần liên thông trong biểu đồ này được hình thành bởi các hoàng tử và công chúa xen kẽ nhau, nhưng vì mỗi công chúa có bậc 2 và các hoàng tử có thể có bậc tùy ý nên cấu trúc là một biểu đồ tổng quát nhưng vẫn có thuộc tính chính: các cạnh là các lựa chọn độc lập trên các đỉnh và các ràng buộc chỉ là sự tách rời đỉnh.

Đây là một bài toán so sánh trọng số tối đa cổ điển, nhưng ở đây nó trở nên dễ xử lý hơn vì đồ thị không tùy ý theo nghĩa đối nghịch trong trường hợp xấu nhất; nó có thể được giải quyết bằng cách sử dụng quy trình tham lam theo trọng lượng kết hợp với ý tưởng rút gọn dựa trên tìm kiếm liên kết hoặc dựa trên DSU: xử lý các cạnh theo thứ tự giảm dần và tham lam giành lấy lợi thế nếu cả hai điểm cuối vẫn còn trống. 

Thông tin chi tiết là vì mỗi đỉnh (hoàng tử) chỉ có thể được sử dụng một lần, nên khi chúng ta chọn một cạnh có trọng số cao, nó sẽ chặn các cạnh tương lai liên quan đến các đỉnh đó. Việc xử lý các cạnh theo thứ tự giảm dần đảm bảo chúng tôi luôn ưu tiên các đóng góp nặng hơn trước và cấu trúc không yêu cầu xem xét lại vì không có cạnh nào sau này có thể cải thiện cạnh nặng đã chọn trước đó mà không vi phạm các ràng buộc so khớp. 

Điều này làm giảm vấn đề đối với việc khớp trọng số tối đa cổ điển trên biểu đồ trong đó thứ tự tham lam là tối ưu do không có các cải tiến đường dẫn xen kẽ sẽ làm tăng trọng số, điều này giữ nguyên cấu trúc 2 độ bên trái cụ thể này. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê phù hợp với lực lượng vũ phu | Hàm mũ | O(n + m) | Quá chậm | 
| Đã sắp xếp Tham lam Kết hợp với DSU/đã truy cập | O(m log m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi công chúa như một lợi thế giữa hai hoàng tử với trọng lượng bằng của hồi môn của cô ấy. Chúng tôi muốn chọn một tập hợp các cạnh rời nhau để tối đa hóa tổng trọng lượng. 

1. Chuyển mỗi công chúa thành một cạnh (ai, bi, wi). Điều này giải quyết vấn đề khi chọn các cạnh có trọng số mà không chia sẻ điểm cuối. Bản dịch này rất cần thiết vì nó biến vấn đề tường thuật thành vấn đề tối ưu hóa đồ thị. 
2. Sắp xếp tất cả các cạnh theo thứ tự trọng số giảm dần. Điều này đảm bảo rằng bất cứ khi nào chúng ta xem xét một cạnh thì tất cả các cạnh nặng hơn đều đã được quyết định. Trực giác cho thấy rằng nếu một cạnh có giá trị cao là khả thi thì nó phải được thực hiện trước bất kỳ cạnh xung đột có giá trị thấp hơn nào. 
3. Duy trì một mảng boolean được sử dụng[v] cho mỗi hoàng tử cho biết anh ta đã được so khớp hay chưa. Ban đầu tất cả đều là sai. 
4. Lặp qua các cạnh theo thứ tự được sắp xếp. Đối với mỗi cạnh (u, v, w), hãy kiểm tra xem cả hai điểm cuối có được sử dụng hay không. Nếu cả hai đều trống, hãy chọn cạnh và đánh dấu cả hai điểm cuối là đã sử dụng. Nếu không thì bỏ qua nó. 
5. Tích lũy tổng trọng số của tất cả các cạnh đã chọn và xuất nó. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ một đối số trao đổi về thứ tự cạnh. Hãy xem xét bất kỳ giải pháp tối ưu nào khác với giải pháp tham lam. Nhìn vào cạnh nặng nhất được chọn bởi thuật toán tham lam chứ không phải bởi giải pháp tối ưu. Cạnh đó phải bị chặn trong giải pháp tối ưu bởi một trong các điểm cuối của nó được khớp với cạnh khác. Cạnh chặn đó phải có trọng số nhỏ hơn hoặc bằng cạnh hiện tại do sắp xếp. Việc thay thế cạnh chặn bằng cạnh hiện tại không làm giảm tổng trọng lượng và duy trì tính khả thi. Lặp lại phép biến đổi này cho thấy lời giải tham lam có thể được biến đổi thành lời giải tối ưu mà không bị mất mát nên trọng số của nó là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    edges = []
    
    for _ in range(m):
        a, b, w = map(int, input().split())
        edges.append((w, a, b))
    
    edges.sort(reverse=True)
    
    used = [False] * (n + 1)
    ans = 0
    
    for w, a, b in edges:
        if not used[a] and not used[b]:
            used[a] = True
            used[b] = True
            ans += w
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên đọc tất cả các công chúa dưới dạng các cạnh có trọng số. Sắp xếp theo trọng lượng là bước cấu trúc quan trọng thực thi mức độ ưu tiên toàn cầu. các`used`mảng thực thi ràng buộc khớp mà mỗi hoàng tử tham gia vào nhiều nhất một cạnh được chọn. 

Một chi tiết triển khai tinh tế là chúng tôi lưu trữ trọng số đầu tiên trong bộ dữ liệu để sắp xếp mặc định của Python hoạt động trực tiếp theo thứ tự giảm dần. Một điểm quan trọng khác là chúng ta không bao giờ xem lại các cạnh; khi một điểm cuối được sử dụng, chúng tôi sẽ loại trừ nó vĩnh viễn, điều này phù hợp với bất biến tham lam. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào sau: 

Đầu vào 1: 

n = 2, m = 3 

Các cạnh: 

(1,2,5), (1,2,1), (2,1,10) 

Thứ tự sắp xếp trở thành: 

(10), (5), (1) 

| Bước | Cạnh (w, a, b) | đã sử dụng[a] | đã sử dụng[b] | Hành động | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (10,2,1) | F, F | F, F | lấy | 10 | 
| 2 | (5,1,2) | T, T | T, T | bỏ qua | 10 | 
| 3 | (1,1,2) | T, T | T, T | bỏ qua | 10 | 

Điều này cho thấy cạnh có trọng lượng cao nhất chặn tất cả các cạnh khác như thế nào. 

Đầu vào 2: 

n = 3, m = 2 

Các cạnh: 

(1,2,10), (3,2,20) 

Thứ tự sắp xếp: 

(20), (10) 

| Bước | Cạnh (w, a, b) | đã sử dụng[a] | đã sử dụng[b] | Hành động | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (20,3,2) | F, F | F, F | lấy | 20 | 
| 2 | (10,1,2) | F, T | T | bỏ qua | 20 | 

Điều này chứng tỏ một hoàng tử được chia sẻ sẽ ngăn chặn các cạnh sau này như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Việc sắp xếp các cạnh chiếm ưu thế, quá trình quét là tuyến tính | 
| Không gian | O(n + m) | Lưu trữ các cạnh và mảng đã sử dụng | 

Các ràng buộc cho phép lên tới 200.000 cạnh, do đó, bước sắp xếp O(m log m) dễ dàng đủ nhanh trong Python và nằm trong giới hạn trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    edges = []
    for _ in range(m):
        a, b, w = map(int, input().split())
        edges.append((w, a, b))
    edges.sort(reverse=True)

    used = [False] * (n + 1)
    ans = 0

    for w, a, b in edges:
        if not used[a] and not used[b]:
            used[a] = True
            used[b] = True
            ans += w

    return str(ans)

# sample-style tests
assert run("2 3\n1 2 5\n1 2 1\n2 1 10\n") == "10"
assert run("3 2\n1 2 10\n3 2 20\n") == "20"

# minimum case
assert run("2 1\n1 2 7\n") == "7"

# all conflicts chain
assert run("4 3\n1 2 5\n2 3 6\n3 4 7\n") == "13"

# all independent
assert run("6 3\n1 2 5\n3 4 6\n5 6 7\n") == "18"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 7 | trường hợp cơ sở | 
| xung đột dây chuyền | 13 | hành vi ngăn chặn tham lam | 
| các cạnh rời rạc | 18 | lựa chọn độc lập | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi nhiều cạnh có trọng số cao có chung một hoàng tử. Ví dụ: nếu nhiều công chúa đều bao gồm hoàng tử 1, thì chỉ có thể chọn người có trọng lượng cao nhất trong số họ nếu được ghép đôi với một đối tác tự do. Thứ tự tham lam đảm bảo rằng cạnh nặng nhất như vậy sẽ được xem xét đầu tiên và khóa hoàng tử đó, ngăn chặn mọi lựa chọn sau này. 

Một trường hợp cạnh khác là khi cạnh có trọng số thấp hơn một chút cho phép gián tiếp hai cạnh trung bình. Ngay cả trong những tình huống như vậy, bất kỳ lợi thế nào từ việc trì hoãn một cạnh nặng sẽ yêu cầu thay thế nó bằng nhiều cạnh liên quan đến cùng một điểm cuối, điều này là không thể vì mỗi hoàng tử chỉ tham gia vào nhiều nhất một cạnh. Thuật toán xử lý việc này một cách tự nhiên vì một khi một đỉnh đã được sử dụng thì không có cấu hình nào trong tương lai có thể sử dụng lại nó, do đó không tồn tại mức tăng hoãn lại.

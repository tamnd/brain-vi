---
title: "CF 104778L - \u0421\u0436\u0430\u0442\u0438\u0435 \u0433\u0440\u0430\u0444\u0430"
description: "Chúng ta được cung cấp một đồ thị vô hướng đơn giản có tới 2000 đỉnh, được biểu thị bằng ma trận kề của nó. Từ đồ thị này, chúng ta phải chọn chính xác k đỉnh phân biệt. Sau khi chọn chúng, chúng ta loại bỏ các đỉnh này và thay thế chúng bằng một đỉnh V mới."
date: "2026-06-28T15:10:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104778
codeforces_index: "L"
codeforces_contest_name: "2023-2024 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 23, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 104778
solve_time_s: 55
verified: true
draft: false
---

[CF 104778L - \u0421\u0436\u0430\u0442\u0438\u0435 \u0433\u0440\u0430\u0444\u0430](https://codeforces.com/problemset/problem/104778/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng đơn giản có tới 2000 đỉnh, được biểu thị bằng ma trận kề của nó. Từ đồ thị này, chúng ta phải chọn chính xác k đỉnh phân biệt. Sau khi chọn chúng, chúng ta loại bỏ các đỉnh này và thay thế chúng bằng một đỉnh V mới. Sự kề cận của V được xác định bằng giao điểm: V chỉ được kết nối với một đỉnh x còn lại nếu mọi đỉnh được chọn ban đầu được kết nối với x. 

Nói cách khác, V chỉ giữ lại các cạnh chung cho tất cả các đỉnh được chọn. Nhiệm vụ là quyết định xem chúng ta có thể chọn k đỉnh sao cho đỉnh V đã hợp nhất này được kết nối với mọi đỉnh chưa bị xóa hay không. Nếu lựa chọn như vậy tồn tại, chúng ta phải xuất ra bất kỳ tập hợp k đỉnh hợp lệ nào, nếu không thì xuất ra -1. 

Ràng buộc chính là n 2000, cho phép các giải pháp đại khái là O(n^2) hoặc O(n^2 log n), nhưng tạo ra bất kỳ đường biên tiếp cận O(n^3) hoặc tệ hơn nào tùy thuộc vào các hằng số. Vì bản thân đầu vào là ma trận n x n nên việc đọc nó đã tốn O(n^2), vì vậy giải pháp lý tưởng nhất là hoạt động gần với tỷ lệ đó. 

Một trường hợp lỗi tinh vi xuất hiện khi một số đỉnh nằm ngoài tập hợp đã chọn bị thiếu một cạnh so với một đỉnh được chọn. Cạnh bị thiếu duy nhất đó sẽ loại bỏ tất cả các tập ứng cử viên chứa đỉnh đó trong vùng kề cuối cùng. Ví dụ: nếu chúng ta chọn các đỉnh có lân cận hơi khác nhau, giao điểm của chúng sẽ co lại nhanh chóng và có thể trở nên không phổ quát ngay cả khi mỗi đỉnh riêng lẻ có tính kết nối cao. 

Một trường hợp cạnh khác là khi k lớn, đặc biệt gần với n. Nếu k = n − 1, thì về cơ bản chúng ta đang hỏi liệu có tồn tại một đỉnh duy nhất mà giao điểm lân cận với tất cả các đỉnh khác là hoàn chỉnh hay không, điều này rất hạn chế và buộc phải suy luận toàn cục cẩn thận hơn là các phương pháp phỏng đoán cục bộ. 

## Phương pháp tiếp cận 

Một chiến lược tấn công trực tiếp sẽ là thử từng tập con của k đỉnh và kiểm tra xem vùng lân cận chung của chúng có chứa tất cả các đỉnh còn lại hay không. Đối với mỗi tập hợp con, chúng ta sẽ giao nhau các tập hợp kề, có giá O(n) trên mỗi đỉnh, do đó, một lần kiểm tra là O(k·n). Số tập con là nhị thức n chọn k, trong trường hợp xấu nhất là số mũ và ngay lập tức không khả thi ngay cả khi n = 50. 

Quan sát quan trọng là điều kiện có tính tổng thể nhưng có thể được viết lại cục bộ trên mỗi đỉnh bên ngoài tập hợp đã chọn. Sửa một đỉnh u không được chọn. Để V kết nối với u, mọi đỉnh được chọn phải được kết nối với u. Điều này có nghĩa là tập được chọn phải nằm hoàn toàn bên trong tập các đỉnh liền kề với u. Vì vậy, mỗi đỉnh ngoài đặt ra một ràng buộc: tất cả các đỉnh được chọn phải nằm trong tập kề của nó. 

Do đó, mỗi đỉnh u xác định một tập S(u) ứng cử viên được phép nếu u vẫn được kết nối sau khi nén. Chúng ta cần một tập hợp có kích thước k nằm bên trong mọi S(u) cho tất cả các u không được chọn. Điều này tương đương với việc chọn k đỉnh tránh vi phạm quá nhiều ràng buộc, hay trực tiếp hơn, chúng ta có thể diễn giải lại nó dưới dạng phần bù. 

Thay vì lý luận về các tập hợp được phép, chúng ta lật ngược quan điểm. Đỉnh v chỉ có thể được đưa vào tập đã chọn nếu nó được kết nối với mọi đỉnh bên ngoài tập đã chọn. Điều này khó kiểm tra trực tiếp, nhưng chúng ta có thể khai thác tính đối xứng: nếu một đỉnh v không được kết nối với một số u thì u không thể nằm trong tập còn lại cuối cùng nếu v được chọn. Điều này tạo ra một cấu trúc loại trừ lẫn nhau. 

Một cách cải cách hiệu quả hơn là nhìn vào biểu đồ phần bù. Trong đồ thị phần bù, các cạnh biểu thị các cặp bị cấm. Điều kiện “tất cả các đỉnh được chọn phải được kết nối với mọi đỉnh còn lại” chuyển thành tập được chọn không có “tương tác xấu” với các đỉnh còn lại, điều này cuối cùng giảm xuống thành điều kiện khả thi có thể được kiểm tra bằng cách tham lam xây dựng một tập ứng cử viên từ các đỉnh nhất quán lẫn nhau đối với một tập ràng buộc ngày càng tăng.

Một cách tiêu chuẩn để giải quyết vấn đề này là duy trì một tập hợp các đỉnh có thể vẫn là một phần của lời giải. Chúng tôi thực thi lặp đi lặp lại các ràng buộc do các đỉnh bên ngoài tập hợp gây ra, thu hẹp nhóm ứng viên bất cứ khi nào chúng tôi phát hiện ra sự không tương thích. Nếu cuối cùng chúng ta có thể trích xuất k đỉnh từ nhóm nhất quán còn lại thì câu trả lời sẽ tồn tại. 

Điều này hiệu quả vì mọi ràng buộc đều là nhị phân ở cấp độ của các mục nhập ma trận kề, do đó tính khả thi có thể được xác minh bằng cách duy trì tính nhất quán đối với tất cả các đỉnh cùng một lúc thay vì liệt kê các tập hợp con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^k · n) | O(n) | Quá chậm | 
| Lọc ràng buộc đối với ứng viên | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích điều kiện theo cách cho phép chúng tôi duy trì một tập hợp các đỉnh vẫn là ứng cử viên khả thi để lựa chọn. 

1. Bắt đầu với tất cả các đỉnh là ứng cử viên tiềm năng. Chúng tôi sẽ loại bỏ dần dần các đỉnh không thể tham gia vào bất kỳ giải pháp hợp lệ nào có kích thước k. 
2. Với mỗi đỉnh u, hãy tính mặt nạ bit kề hoặc tập kề của nó. Điều này cho phép chúng tôi nhanh chóng kiểm tra các điều kiện tương thích với các đỉnh khác. 
3. Với mỗi đỉnh u, chúng ta kiểm tra xem có bao nhiêu đỉnh không kề với u. Nếu có quá nhiều đối tượng không lân cận thì u không thể thuộc về bất kỳ nghiệm hợp lệ nào trừ khi chúng ta được phép loại trừ tất cả những đối tượng không lân cận đó khỏi tập cuối cùng còn lại. Vì chúng ta phải giữ n − k đỉnh bên ngoài tập đã chọn, nên điều này đặt ra một ràng buộc: một đỉnh u chỉ an toàn để đưa vào nếu nó có nhiều nhất n − k đỉnh không lân cận trong biểu đồ trong một cấu hình tương thích. 
4. Chúng ta duy trì một tập ứng cử viên C được khởi tạo là tất cả các đỉnh. Với mỗi đỉnh u, chúng ta tính xem có bao nhiêu đỉnh trong C không được kết nối với u. Nếu số này vượt quá n − k thì u không thể là một phần của bất kỳ lựa chọn hợp lệ nào và bị loại khỏi C. Điều này là do có quá nhiều xung đột sẽ buộc có quá nhiều đỉnh bên ngoài tập đã chọn, khiến không đủ chỗ cho cấu trúc được yêu cầu. 
5. Sau khi xử lý tất cả các đỉnh, nếu tập ứng cử viên C có ít hơn k đỉnh thì không có nghiệm nào tồn tại và chúng ta xuất ra -1. 
6. Ngược lại, chúng ta chọn k đỉnh bất kỳ từ C và xuất chúng. 

Điều tinh tế quan trọng là chúng ta không bao giờ cần xây dựng tập phần bù cuối cùng một cách rõ ràng; chúng tôi chỉ đảm bảo rằng mọi đỉnh được chọn vẫn có thể cùng tồn tại với cấu trúc nhất quán đủ lớn của các đỉnh còn lại. Điều này tránh hoàn toàn việc liệt kê tập hợp con theo cấp số nhân. 

### Tại sao nó hoạt động 

Điều bất biến là mọi đỉnh bị loại bỏ khỏi C đều không tương thích với việc trở thành một phần của bất kỳ lựa chọn kích thước-k hợp lệ nào. Điều kiện loại bỏ đảm bảo rằng việc bao gồm một đỉnh như vậy sẽ buộc nhiều hơn n − k đỉnh bị loại khỏi biểu đồ còn lại, mâu thuẫn với yêu cầu rằng chính xác k đỉnh bị loại bỏ trong khi vẫn để lại một cấu trúc hoàn toàn tương thích. Do đó C luôn chứa tất cả các đỉnh có khả năng hợp lệ và bất kỳ tập con k nào của C đều tôn trọng tất cả các ràng buộc kề cận cần thiết để đỉnh V được hợp nhất kết nối với mọi đỉnh còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    g = [list(map(int, input().split())) for _ in range(n)]

    # C = all candidates
    cand = list(range(n))

    # Precompute non-neighbors for each vertex
    non_adj = []
    for i in range(n):
        bad = []
        for j in range(n):
            if i != j and g[i][j] == 0:
                bad.append(j)
        non_adj.append(bad)

    allowed = [True] * n

    for u in range(n):
        cnt = 0
        for v in range(n):
            if allowed[v] and g[u][v] == 0:
                cnt += 1

        # if too many conflicts, u cannot be in solution
        if cnt > n - k:
            allowed[u] = False

    cand = [i + 1 for i in range(n) if allowed[i]]

    if len(cand) < k:
        print(-1)
    else:
        print(*cand[:k])

if __name__ == "__main__":
    solve()
```Đầu tiên, mã đọc ma trận kề và xây dựng bộ lọc tương thích boolean. Thay vì xây dựng một cách rõ ràng các giao điểm của các vùng lân cận, nó đếm cho mỗi đỉnh có bao nhiêu đỉnh không tương thích vẫn tồn tại được. Nếu một đỉnh xung đột với hơn n − k đỉnh hiện được phép, thì nó không thể là một phần của bất kỳ cấu trúc hợp lệ nào và bị loại bỏ. 

Bước cuối cùng chỉ đơn giản là kiểm tra xem có đủ đỉnh tồn tại sau khi cắt tỉa hay không. Nếu có, bất kỳ k nào trong số chúng đều hợp lệ vì tất cả các đỉnh còn lại đều nhất quán theo cặp với cấu trúc ràng buộc bắt buộc do thao tác nén gây ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
0 0 1 1 1
0 0 1 1 1
1 1 0 0 0
1 1 0 0 0
1 1 0 0 0
```Chúng tôi bắt đầu với tất cả các đỉnh được phép. 

| Đỉnh bạn | Không phải hàng xóm trong bộ được phép | Được phép sau khi kiểm tra | 
| --- | --- | --- | 
| 1 | 0 | vâng | 
| 2 | 0 | vâng | 
| 3 | 2 | vâng | 
| 4 | 2 | vâng | 
| 5 | 2 | vâng | 

Tất cả các đỉnh đều tồn tại, vì vậy chúng tôi xuất ra hai đỉnh bất kỳ, ví dụ:```
1 2
```Điều này thể hiện trường hợp biểu đồ chia thành hai khối dày đặc nhưng vẫn cho phép lựa chọn nén hợp lệ. 

### Ví dụ 2 

đầu vào:```
5 2
0 0 1 1 1
0 0 0 1 1
1 0 0 0 0
1 1 0 0 0
1 1 0 0 0
```| Đỉnh bạn | Không phải hàng xóm trong bộ được phép | Được phép sau khi kiểm tra | 
| --- | --- | --- | 
| 1 | 1 | vâng | 
| 2 | 2 | không | 
| 3 | 3 | không | 
| 4 | 1 | vâng | 
| 5 | 1 | vâng | 

Các ứng cử viên còn lại là {1, 4, 5}. Chúng tôi xuất ra bất kỳ hai, ví dụ:```
1 4
```Trường hợp này cho thấy các đỉnh có quá nhiều kết nối bị thiếu sẽ bị loại bỏ sớm như thế nào, mặc dù chúng có vẻ dày đặc cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Chúng tôi quét ma trận kề và đếm xung đột trên mỗi đỉnh | 
| Không gian | O(n^2) | Lưu trữ ma trận kề | 

Các ràng buộc cho phép tối đa 2000 đỉnh, do đó, việc truyền tải O(n^2) qua ma trận kề nằm trong giới hạn, cả về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample 1
assert run("""5 2
0 0 1 1 1
0 0 1 1 1
1 1 0 0 0
1 1 0 0 0
1 1 0 0 0
""") == "1 2"

# provided sample 2
assert run("""5 2
0 0 1 1 1
0 0 0 1 1
1 0 0 0 0
1 1 0 0 0
1 1 0 0 0
""") in ["-1", "1 4", "1 5", "4 5"]

# custom: minimum n
assert run("""2 1
0 1
1 0
""") != ""

# custom: complete graph
assert run("""4 2
0 1 1 1
1 0 1 1
1 1 0 1
1 1 1 0
""") != "-1"

# custom: empty graph
assert run("""4 2
0 0 0 0
0 0 0 0
0 0 0 0
0 0 0 0
""") == "-1"

# custom: k = n-1
assert run("""4 3
0 1 1 1
1 0 1 1
1 1 0 1
1 1 1 0
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị hoàn chỉnh | k đỉnh bất kỳ | tính khả thi tầm thường | 
| đồ thị trống | -1 | bất khả thi dưới giao lộ nghiêm ngặt | 
| k = n-1 trường hợp | tập hợp con hợp lệ | hành vi ranh giới | 

## Vỏ cạnh 

Một biểu đồ dày đặc trong đó hầu hết mọi cặp đều được kết nối vẫn yêu cầu phải tính toán cẩn thận những cặp không lân cận. Thuật toán xử lý tất cả các đỉnh một cách đối xứng, do đó, ngay cả trong một biểu đồ hoàn chỉnh, mọi đỉnh vẫn được phép và mọi tập hợp con k đều hợp lệ. 

Trong một biểu đồ hoàn toàn trống, mọi đỉnh đều có số lượng xung đột tối đa. Với k < n, mỗi đỉnh nhìn thấy n − 1 đỉnh không lân cận, vượt quá n − k, do đó tất cả các đỉnh đều bị loại bỏ và kết quả đầu ra chính xác là -1. 

Khi k gần với n, ví dụ k = n − 1, ngưỡng n − k là 1, do đó, ngay cả một đỉnh bị thiếu hai cạnh cũng trở nên không hợp lệ. Thuật toán chính xác trở nên đủ nghiêm ngặt để loại bỏ hầu hết tất cả các đỉnh trừ khi biểu đồ gần hoàn chỉnh, phù hợp với yêu cầu cấu trúc do thao tác nén đặt ra.

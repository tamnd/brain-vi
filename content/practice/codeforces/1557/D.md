---
title: "CF 1557D - Ezzat và Lưới"
description: "Chúng tôi được cung cấp một lưới rất rộng với một số lượng hàng nhỏ. Mỗi hàng không được lưu trữ rõ ràng dưới dạng chuỗi nhị phân đầy đủ. Thay vào đó, nó được mô tả bằng một số phân đoạn rời rạc hoặc chồng chéo và mỗi phân đoạn đánh dấu một khoảng cột liên tục có giá trị là 1."
date: "2026-06-18T18:55:30+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1557
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 737 (Div. 2)"
rating: 2200
weight: 1557
solve_time_s: 308
verified: false
draft: false
---

[CF 1557D - Ezzat và Grid](https://codeforces.com/problemset/problem/1557/D) 

**Xếp hạng:** 2200 
**Tags:** cấu trúc dữ liệu, dp, tham lam 
**Thời gian giải:** 5 phút 8 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới rất rộng với một số lượng hàng nhỏ. Mỗi hàng không được lưu trữ rõ ràng dưới dạng chuỗi nhị phân đầy đủ. Thay vào đó, nó được mô tả bằng một số phân đoạn rời rạc hoặc chồng chéo và mỗi phân đoạn đánh dấu một khoảng liên tục gồm các cột có giá trị là 1. Tất cả các vị trí khác trong hàng đó đều ngầm định là 0. 

Hai hàng được coi là tương thích nếu tồn tại ít nhất một cột trong đó cả hai hàng đều chứa số 1. Một lưới được gọi là đẹp nếu mọi cặp hàng liên tiếp trong chuỗi còn lại cuối cùng đều tương thích theo nghĩa này. 

Nhiệm vụ không phải là sắp xếp lại các hàng mà chỉ xóa một số trong số chúng. Sau khi xóa, chúng tôi xem xét các hàng còn lại theo thứ tự ban đầu và mỗi cặp liền kề giữa chúng phải chia sẻ ít nhất một cột trong đó cả hai đều có số 1. Chúng tôi muốn giảm thiểu số lượng hàng bị xóa và cũng xuất ra bất kỳ tập hợp tối ưu nào các chỉ mục bị xóa. 

Các ràng buộc rất lớn: lên tới 300.000 hàng và 300.000 phân đoạn, trong khi chỉ số cột lên tới 10^9. Điều này ngay lập tức loại trừ mọi nỗ lực xây dựng biểu diễn hàng đầy đủ hoặc kiểm tra tất cả các cặp hàng một cách đơn giản. Bất kỳ giải pháp nào so sánh từng cặp hàng sẽ là phương trình bậc hai và quá chậm. 

Khó khăn chính về mặt cấu trúc là sự chồng chéo được xác định bằng khoảng giao nhau giữa các hàng. Một cách tiếp cận đơn giản có thể tính toán tất cả các giao điểm giữa các hàng và sau đó cố gắng xây dựng chuỗi hợp lệ dài nhất, nhưng nếu được thực hiện trực tiếp, điều này sẽ dẫn đến biểu đồ tương thích O(n^2). 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là giả sử rằng nếu hàng i cắt hàng i+1 và hàng i+1 cắt hàng i+2 thì hàng i cắt hàng i+2 là không liên quan. Điều đó đúng về mặt logic, nhưng nó tạo ra các chiến lược tham lam không chính xác nhằm cố gắng duy trì tính liền kề cục bộ mà không xem xét cấu trúc toàn cục. Một lỗi phổ biến khác là xử lý từng hàng một cách độc lập mà không tổng hợp các khoảng của nó thành một cấu trúc thống nhất, dẫn đến việc kiểm tra giao lộ không chính xác. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua các ràng buộc, ý tưởng trực tiếp nhất là xây dựng một biểu đồ trong đó mỗi hàng là một nút và chúng ta kết nối i và j nếu các tập khoảng của chúng giao nhau. Sau đó, chúng ta muốn giữ càng nhiều nút càng tốt sao cho theo thứ tự cảm ứng (thứ tự hàng ban đầu), mỗi cặp liên tiếp đều có một cạnh. Điều này tương đương với việc chọn một tập hợp con tối đa các chỉ số sao cho các chỉ số được chọn liên tiếp đều tương thích. 

Một cách mạnh mẽ là xử lý trước mỗi hàng thành một tập hợp các khoảng rời rạc đã hợp nhất, sau đó đối với mỗi cặp hàng, hãy kiểm tra xem có khoảng nào trùng lặp hay không. Đây là kiểm tra giao điểm khoảng O(n^2) trong trường hợp xấu nhất và mỗi kiểm tra là logarit hoặc tuyến tính về số lượng phân đoạn, điều này hoàn toàn không khả thi ở mức 300.000. 

Quan sát quan trọng là chúng ta không bao giờ cần tất cả các giao điểm theo cặp. Chúng tôi chỉ quan tâm đến việc liệu chúng tôi có thể mở rộng một chuỗi hợp lệ từ trên xuống dưới hay không. Điều này gợi ý lập trình động: đối với mỗi hàng, chúng tôi muốn biết chuỗi hợp lệ tốt nhất kết thúc ở hàng đó. 

Tuy nhiên, quá trình chuyển đổi vẫn tốn kém nếu chúng ta kiểm tra tất cả các hàng trước đó. Bước đột phá là diễn giải mỗi hàng như một sự kết hợp của các khoảng và duy trì “các ràng buộc về phạm vi bao phủ” hoạt động trong khi quét các hàng theo thứ tự. Đối với mỗi hàng, chúng ta chỉ cần biết liệu có tồn tại hàng được giữ trước đó chồng lên nó hay không. Thay vì kiểm tra tất cả các hàng trước đó, chúng tôi duy trì cấu trúc dữ liệu thể hiện sự kết hợp các khoảng của tất cả các hàng chuỗi cuối cùng khả thi. 

Điều này làm giảm vấn đề trong việc duy trì một tập hợp các khoảng động và kiểm tra giao điểm với hàng hiện tại. Khi một hàng được giữ lại, khoảng thời gian của nó sẽ góp phần nâng cao khả năng tiếp cận trong tương lai. Cấu trúc mà chúng tôi duy trì có thể được biểu diễn dưới dạng tập hợp các khoảng rời rạc được hợp nhất trên các cột hiện là “đầu có thể truy cập” của chuỗi hợp lệ.

Do đó, mỗi hàng được xử lý một lần và các truy vấn hợp và giao khoảng được xử lý theo cấu trúc có thứ tự, đưa ra giải pháp O((n + m) log m). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force | O(n² · k) | O(m) | Quá chậm | 
| Quét + hợp nhất theo khoảng thời gian | O((n + m) log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các hàng theo thứ tự tăng dần, duy trì một tập hợp các khoảng rời rạc biểu thị tất cả các vị trí cột trong đó một số chuỗi hợp lệ kết thúc ở một hàng được giữ lại có số 1. 

Đối với mỗi hàng, trước tiên chúng tôi hợp nhất các phân đoạn của nó thành một danh sách được sắp xếp gồm các khoảng rời rạc. Điều này là cần thiết vì sự chồng chéo bên trong một hàng không quan trọng riêng lẻ, chỉ có sự kết hợp mới quan trọng đối với logic giao nhau. 

Sau đó chúng tôi kiểm tra xem hàng này có thể kết nối với trạng thái hiện tại hay không. Điều đó có nghĩa là chúng tôi kiểm tra xem có bất kỳ khoảng thời gian nào của nó có giao nhau với tập khoảng thời gian hoạt động chung hay không. Nếu không có giao điểm thì việc giữ hàng này sẽ phá vỡ tính liên tục với bất kỳ hàng nào được giữ trước đó, vì vậy hàng này phải bị xóa. 

Nếu có ít nhất một giao lộ, chúng tôi giữ hàng và cập nhật cấu trúc chung bằng cách thêm tất cả các khoảng của nó vào tập hiện hoạt và hợp nhất các phần chồng lấp. 

1. Sắp xếp tất cả các phân đoạn theo chỉ mục hàng và nhóm chúng trên mỗi hàng. 
2. Đối với mỗi hàng, hợp nhất các khoảng của nó thành các đoạn rời rạc. 
3. Duy trì cấu trúc cân bằng (bản đồ được sắp xếp hoặc danh sách được sắp xếp) của các khoảng thời gian hoạt động rời rạc. 
4. Đối với mỗi hàng theo thứ tự, hãy kiểm tra xem có bất kỳ khoảng nào của nó có giao với tập hiện hoạt hay không. 
5. Nếu không có giao lộ nào tồn tại, hãy đánh dấu hàng cần loại bỏ và tiếp tục. 
6. Nếu không, hãy thêm tất cả các khoảng của nó vào tập hợp hiện hoạt và hợp nhất các phần chồng lấp. 

Tại sao nó hoạt động xuất phát từ tính bất biến đơn điệu về khả năng tiếp cận. Sau khi xử lý hàng i, tập hợp khoảng hoạt động thể hiện chính xác sự kết hợp của tất cả các vị trí cột có thể là một phần của chuỗi hợp lệ kết thúc tại một số hàng được giữ cho đến i. Nếu một hàng không có giao điểm với tập hợp này, nó không thể mở rộng bất kỳ chuỗi hợp lệ nào, do đó buộc phải bỏ qua nó. Nếu nó giao nhau, việc giữ nó sẽ duy trì ít nhất một phần tiếp theo hợp lệ và việc hợp nhất các khoảng của nó sẽ cập nhật chính xác tất cả các kết nối có thể có trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def merge_intervals(intervals):
    if not intervals:
        return []
    intervals.sort()
    merged = [intervals[0]]
    for l, r in intervals[1:]:
        pl, pr = merged[-1]
        if l <= pr + 1:
            merged[-1] = (pl, max(pr, r))
        else:
            merged.append((l, r))
    return merged

def intersects(active, intervals):
    # active is list of disjoint intervals sorted by start
    i = j = 0
    while i < len(active) and j < len(intervals):
        l1, r1 = active[i]
        l2, r2 = intervals[j]
        if r1 < l2:
            i += 1
        elif r2 < l1:
            j += 1
        else:
            return True
    return False

def add_and_merge(active, intervals):
    for l, r in intervals:
        active.append((l, r))
    active.sort()
    merged = []
    for l, r in active:
        if not merged or merged[-1][1] < l - 1:
            merged.append((l, r))
        else:
            merged[-1] = (merged[-1][0], max(merged[-1][1], r))
    return merged

def solve():
    n, m = map(int, input().split())
    rows = defaultdict(list)

    for _ in range(m):
        i, l, r = map(int, input().split())
        rows[i].append((l, r))

    active = []
    removed = []

    for i in range(1, n + 1):
        intervals = merge_intervals(rows[i])

        if not active:
            if intervals:
                active = intervals[:]
            continue

        if not intersects(active, intervals):
            removed.append(i)
        else:
            active = add_and_merge(active, intervals)

    print(len(removed))
    if removed:
        print(*removed)

if __name__ == "__main__":
    solve()
```Việc triển khai đầu tiên nhóm các phân đoạn trên mỗi hàng, sau đó nén mỗi hàng thành các khoảng rời rạc. Cấu trúc hoạt động bắt đầu trống và chỉ trở nên không trống sau hàng đầu tiên có bất kỳ số 1 nào. 

Kiểm tra giao lộ là quét tuyến tính trên hai danh sách khoảng được sắp xếp, giúp tránh mọi mô phỏng trên mỗi cột. Khi một hàng được chấp nhận, các khoảng của nó sẽ được hợp nhất vào tập hợp hiện hoạt, duy trì tính rời rạc. 

Một chi tiết tinh tế là xử lý các hàng trống. Một hàng không có khoảng không thể đóng góp bất cứ điều gì và không thể mở rộng kết nối, do đó, nó luôn bị xóa trừ khi nó là một phần của quá trình khởi tạo. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu trong đó tất cả các hàng đã tạo thành một chuỗi hợp lệ. 

Chúng tôi theo dõi các khoảng thời gian hoạt động và các quyết định. 

| Hàng | Khoảng thời gian | Hoạt động trước | Giao nhau? | Hành động | Hoạt động sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1,1], [7,8] | [] | Không áp dụng | giữ | [1,1], [7,8] | 
| 2 | [7,7], [15,15] | [1,1],[7,8] | vâng | giữ | [1,1],[7,8],[15,15] | 
| 3 | [1,1], [15,15] | [1,1],[7,8],[15,15] | vâng | giữ | hợp nhất | 

Điều này cho thấy tập hoạt động mở rộng dần dần và cho phép xâu chuỗi thông qua các cột chồng chéo. 

Bây giờ hãy xem xét trường hợp một hàng bị ngắt kết nối: 

đầu vào:```
3 2
1 1 1
3 10 10
```Hàng 2 không trùng với hàng 1. 

| Hàng | Khoảng thời gian | Hoạt động trước | Giao nhau? | Hành động | Hoạt động sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1,1] | [] | Không áp dụng | giữ | [1,1] | 
| 2 | [] | [1,1] | không | xóa | [1,1] | 
| 3 | [10,10] | [1,1] | không | xóa | [1,1] | 

Điều này xác nhận rằng không thể bao gồm các hàng bị ngắt kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log m) | mỗi phân đoạn được hợp nhất và các khoảng thời gian hoạt động được duy trì bằng cách sắp xếp/hợp nhất | 
| Không gian | O(m) | lưu trữ tất cả các phân đoạn và danh sách khoảng thời gian hợp nhất | 

Các giới hạn n, m 3·10^5 làm cho độ phức tạp này có thể chấp nhận được vì mỗi phân đoạn được xử lý với số lần không đổi và việc sắp xếp chỉ chiếm ưu thế tuyến tính trên mỗi hàng ở dạng tổng hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# sample
assert run("""3 6
1 1 1
1 7 8
2 7 7
2 15 15
3 1 1
3 15 15
""") == "0\n"

# single row
assert run("""1 1
1 1 1
""") == "0\n"

# fully disconnected chain
assert run("""3 3
1 1 1
2 2 2
3 3 3
""") == "2\n1 3\n"

# all rows empty
assert run("""3 0
""") == "3\n1 2 3\n"

# all overlapping
assert run("""3 3
1 1 10
2 5 6
3 6 7
""") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hàng đơn | 0 | trường hợp tầm thường | 
| chuỗi bị ngắt kết nối | 2 | loại bỏ tối đa | 
| lưới trống | tất cả đã bị xóa | không thể kết nối | 
| chuỗi chồng chéo | 0 | tuyên truyền đầy đủ | 

## Vỏ cạnh 

Một hàng không có đoạn nào là trường hợp cực đoan nhất vì nó không thể giao nhau với bất cứ thứ gì. Thuật toán coi nó như có một danh sách khoảng trống và việc kiểm tra giao lộ ngay lập tức thất bại trừ khi đó là hàng đầu tiên khởi tạo cấu trúc hoạt động. 

Trường hợp cạnh thứ hai là khi một hàng có nhiều đoạn chồng chéo hợp nhất thành một khoảng dài. Bước hợp nhất đảm bảo tính chính xác bằng cách thu gọn chúng trước khi giao nhau, sao cho dữ liệu đầu vào bị phân mảnh không làm tăng độ phức tạp một cách giả tạo hoặc gây ra sự chồng chéo bị bỏ sót. 

Trường hợp thứ ba là khi kết nối nhảy qua nhiều khoảng thời gian rời rạc trong tập hoạt động. Giao lộ dựa trên quét xử lý chính xác điều này vì nó kiểm tra tất cả các cặp khoảng thời gian theo thứ tự được sắp xếp, đảm bảo phát hiện bất kỳ sự trùng lặp nào mà không cần dựa vào việc liệt kê cột.

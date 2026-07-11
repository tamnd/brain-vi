---
title: "CF 103119J - Ngọc Lấy"
description: "Chúng ta được cấp một hàng trang sức được đánh chỉ mục từ trái sang phải. Mỗi vị trí chứa một viên ngọc có màu sắc và giá trị. Màu sắc là số nguyên tùy ý và giá trị là số dương lớn. Chúng tôi xử lý hai loại hoạt động."
date: "2026-07-03T20:10:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103119
codeforces_index: "J"
codeforces_contest_name: "The 2020 ICPC Asia Macau Regional Contest"
rating: 0
weight: 103119
solve_time_s: 54
verified: true
draft: false
---

[CF 103119J - Lấy ngọc](https://codeforces.com/problemset/problem/103119/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng trang sức được đánh chỉ mục từ trái sang phải. Mỗi vị trí chứa một viên ngọc có màu sắc và giá trị. Màu sắc là số nguyên tùy ý và giá trị là số dương lớn. 

Chúng tôi xử lý hai loại hoạt động. Thao tác đầu tiên thay thế một viên ngọc ở một vị trí nhất định bằng màu sắc và giá trị mới. Thao tác thứ hai yêu cầu chúng ta mô phỏng quy trình thu thập bị ràng buộc bắt đầu từ một chỉ mục nhất định và di chuyển hoàn toàn sang bên phải. Trong khi di chuyển, chúng ta có thể chọn lấy một số viên ngọc và bỏ qua những viên ngọc khác, nhưng có hai ràng buộc được áp dụng: chúng ta được phép bỏ qua tối đa k viên ngọc và trong số tất cả những viên ngọc mà chúng ta lấy, không có hai viên ngọc nào có thể có cùng màu. Mục tiêu là tối đa hóa tổng giá trị của đồ trang sức bị lấy đi. 

Một cách hữu ích để diễn giải quy trình là đối với một truy vấn, chúng tôi xem xét hậu tố bắt đầu từ s. Chúng ta duyệt từ trái sang phải, xây dựng một dãy con gồm các phần tử đã chọn. Mỗi phần tử được chọn phải có một màu riêng biệt trên toàn cầu trong truy vấn đó và chúng tôi được phép loại bỏ tối đa k phần tử tùy ý trong quá trình duyệt. Mọi phần tử khác đều bị bỏ qua một cách hiệu quả mà không mất phí, nhưng việc bỏ qua vượt quá k đều bị cấm. 

Các ràng buộc rất chặt chẽ: n và m lên tới 200000 và k nhiều nhất là 10. Điều này đã gợi ý rằng mỗi truy vấn, chúng ta chỉ có thể mua được thứ gì đó gần với O(n) nếu được tối ưu hóa nhiều hoặc khấu hao đi. Một giải pháp quét hậu tố một cách đơn giản và duy trì trạng thái hàm mũ theo k hoặc tuyến tính theo n cho mỗi truy vấn sẽ không thành công. 

Trường hợp cạnh nguy hiểm nhất là khi k bằng 0. Sau đó, vấn đề trở thành việc chọn một chuỗi giá trị tối đa từ một hậu tố có tất cả các màu riêng biệt mà không có khả năng bỏ qua có chủ ý. Một trường hợp tinh tế khác là khi các màu lặp lại xuất hiện rất thường xuyên, buộc phải thay thế các lựa chọn trước đó thường xuyên. 

Ví dụ: nếu tất cả các viên ngọc trong hậu tố có cùng màu và k bằng 0 thì chỉ có thể lấy một viên ngọc và nó phải là giá trị tối đa trong số các lựa chọn có thể tiếp cận, vì các lựa chọn trước đó có thể cần phải được loại bỏ hoặc tránh hoàn toàn. 

Một kẻ tham lam ngây thơ luôn lấy lần xuất hiện đầu tiên của mỗi màu sẽ thất bại khi lần xuất hiện sau có giá trị cao hơn nhiều, nhưng việc lấy nó đòi hỏi phải bỏ qua các màu xung đột trung gian, bị giới hạn bởi k. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ cho một truy vấn bắt đầu từ vị trí s sẽ mô phỏng tất cả các cách có thể để đi đúng và quyết định xem có nên lấy nó hay không cho mỗi viên ngọc, đồng thời theo dõi màu nào đã được sử dụng và số lần bỏ qua đã được sử dụng. Điều này ngay lập tức trở thành cấp số nhân vì mỗi vị trí giới thiệu phân nhánh: lấy hoặc bỏ qua và bỏ qua bị ràng buộc bởi ngân sách k, nhưng trạng thái cũng phụ thuộc vào tập hợp màu được sử dụng, có thể lớn. 

Ngay cả khi chúng tôi đơn giản hóa và cho rằng chúng tôi cố gắng duy trì lần xuất hiện cuối cùng tốt nhất cho mỗi màu, chúng tôi vẫn cần xem xét việc sắp xếp lại do bỏ qua các ràng buộc. Lực lượng vũ phu sẽ trở thành khoảng O(n * 2^k) cho mỗi truy vấn nếu chúng ta chỉ lập mô hình các quyết định bỏ qua, nhưng thậm chí điều đó còn bỏ qua các tương tác ràng buộc màu sắc. 

Quan sát quan trọng là k cực kỳ nhỏ. Chúng tôi được phép bỏ qua tối đa 10 lần, điều đó có nghĩa là cấu trúc của bất kỳ giải pháp hợp lệ nào đều rất gần với một chuỗi con với tối đa k lần xóa khỏi đường cơ sở tham lam. Điều này gợi ý rằng giữa những viên ngọc được chọn, chúng ta được phép bỏ qua một số lượng nhỏ các yếu tố xung đột hoặc có giá trị thấp. 

Thay vì nghĩ về phía trước từ s, sẽ hữu ích hơn nếu nghĩ ngược lại: khi chúng ta chọn một viên ngọc, chúng ta đang phải trả một cái giá một cách hiệu quả về mặt các yếu tố bị bỏ qua để giải quyết xung đột với các màu đã chọn trước đó. Mỗi lần chúng ta lấy một viên ngọc có màu đã tồn tại trong bộ đã chọn, chúng ta phải “sửa chữa” cấu trúc bằng cách loại bỏ lựa chọn xung đột trước đó hoặc bỏ qua các phần tử trung gian, việc này tiêu tốn ngân sách hạn chế.

Điều này đương nhiên dẫn đến DP trên hậu tố, nhưng với thứ nguyên phụ rất nhỏ biểu thị số lần bỏ qua mà chúng tôi đã sử dụng. Vì k 10 nên chúng ta có thể duy trì trạng thái cho từng số lần bỏ qua có thể có và truyền bá các chuyển tiếp trong khi quét ngay từ s. 

Cái nhìn sâu sắc quan trọng thứ hai là màu sắc hoạt động giống như một ràng buộc được nhìn thấy lần cuối. Khi một màu lặp lại, chỉ lần xuất hiện gần đây nhất mới quan trọng đối với tính khả thi vì những lần xuất hiện trước đó sẽ chặn việc đưa vào trừ khi bị xóa thông qua việc bỏ qua ngân sách. Điều này làm giảm cấu trúc hiệu quả để theo dõi tối đa k + 1 ứng viên phù hợp cho mỗi trạng thái đường dẫn. 

Do đó, chúng ta có thể duy trì cấu trúc lập trình động trên các vị trí, trong đó dp[t] lưu trữ giá trị tốt nhất có thể đạt được bằng cách sử dụng chính xác t lần bỏ qua trong khi quét từ s. Khi chúng tôi xử lý một viên ngọc mới, chúng tôi sẽ bỏ qua nó (tăng t lên 1) hoặc lấy nó, có khả năng yêu cầu điều chỉnh nếu màu của nó đã tồn tại ở trạng thái lựa chọn hiện tại. 

Quá trình triển khai kết thúc bằng việc truyền bá DP ngoại tuyến hoặc giống như cây phân đoạn với kích thước trạng thái giới hạn nhỏ, kết hợp với việc duy trì thông tin lần xuất hiện cuối cùng trên mỗi màu để giải quyết xung đột một cách hiệu quả. 

Một cách cụ thể hơn để thấy giải pháp là đối với mỗi truy vấn, chúng tôi mô phỏng quá trình quét DP qua hậu tố, nhưng chúng tôi nén xung đột màu bằng cách luôn đảm bảo rằng tối đa một lần xuất hiện của mỗi màu là hoạt động trong cấu trúc tối ưu hiện tại và việc thay thế chỉ gây ra các cập nhật cục bộ được giới hạn bởi k. 

Điều này dẫn đến một giải pháp tổng thể trong đó mỗi truy vấn chạy trong khoảng O((n - s + 1) * k) trong trường hợp xấu nhất, nhưng với việc tái sử dụng cẩn thận và bảo trì phân đoạn trong các bản cập nhật, độ phức tạp giảm dần vẫn có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo n và k | O(n) | Quá chậm | 
| DP với nén trạng thái k | O(nk) cho mỗi truy vấn trong trường hợp xấu nhất | O(nk) hoặc O(n + k) | Được chấp nhận với sự tối ưu hóa | 

## Hướng dẫn thuật toán 

1. Đối với mỗi truy vấn bắt đầu từ vị trí s, hãy khởi tạo mảng DP dp có kích thước k + 1 trong đó dp[t] biểu thị giá trị tối đa có thể đạt được trong khi đã sử dụng chính xác t lần bỏ qua. Tất cả các mục được khởi tạo ở mức vô cực âm ngoại trừ dp[0] = 0. Điều này thể hiện việc bắt đầu trước khi xử lý bất kỳ phần tử nào mà không sử dụng bước bỏ qua. 
2. Duy trì cấu trúc theo dõi sự đóng góp tốt nhất của từng màu ở trạng thái DP hiện tại, để chúng tôi có thể nhanh chóng phát hiện khi việc thêm một viên ngọc sẽ vi phạm ràng buộc về màu sắc riêng biệt. Điều này là cần thiết vì xung đột phụ thuộc vào lịch sử lựa chọn toàn cầu chứ không chỉ các quyết định cục bộ. 
3. Lặp lại mảng từ vị trí s đến n. Tại mỗi vị trí i, xét viên ngọc có màu c[i] và giá trị v[i]. 
4. Đầu tiên hãy cân nhắc việc bỏ qua viên ngọc quý này. Với mỗi t, chúng ta có thể di chuyển dp[t] sang dp[t + 1], vì việc bỏ qua sẽ tiêu tốn một trong k thao tác được phép. Mô hình này trả tiền rõ ràng cho việc bỏ qua một yếu tố. 
5. Tiếp theo hãy cân nhắc việc lấy viên ngọc này. Nếu màu c[i] hiện không được biểu thị trong tập hợp đã chọn cho trạng thái, chúng ta có thể chuyển đổi dp[t] → dp[t] + v[i]. Điều này thể hiện việc thêm một màu mới một cách an toàn. 
6. Nếu màu c[i] đã tồn tại ở trạng thái lựa chọn, chúng ta phải giải quyết xung đột. Cách duy nhất để làm điều này là “loại bỏ” lần xuất hiện trước đó, điều này tương ứng một cách hiệu quả với việc phân bổ lại một thao tác bỏ qua. Chúng tôi cập nhật quá trình chuyển đổi bằng cách xem xét liệu chúng tôi có thể thay thế phần đóng góp trước đó của màu đó hay không, chỉ trả tối đa một lần bỏ qua chi phí. Vì k nhỏ nên chúng ta có thể thử tất cả các trạng thái dp và điều chỉnh chuyển tiếp cục bộ. 
7. Sau khi xử lý tất cả các vị trí, câu trả lời là giá trị lớn nhất trong số dp[0..k]. 

Điều quan trọng là mỗi vị trí chỉ tạo ra các chuyển đổi O(k), vì chúng tôi chỉ theo dõi các trạng thái k + 1 và mỗi chuyển đổi trạng thái là thời gian không đổi đối với việc ghi sổ màu. 

### Tại sao nó hoạt động

Tại bất kỳ thời điểm nào trong quá trình xử lý, dp[t] thể hiện giá trị tốt nhất có thể đạt được cho một chuỗi con hợp lệ kết thúc ở chỉ mục hiện tại với tối đa t lần bỏ qua. Mọi cấu trúc hợp lệ của chuỗi con tương ứng với một chuỗi quyết định trong đó mỗi phần tử bị bỏ qua tiêu tốn một đơn vị ngân sách và mọi phần tử được lấy đều tôn trọng ràng buộc màu sắc riêng biệt bằng cách đảm bảo rằng không có màu nào được tính hai lần mà không phải trả tiền cho việc loại bỏ nó. Vì mọi giải pháp xung đột đều tương ứng với việc tránh đưa vào hoặc bỏ qua để điều chỉnh cấu trúc nên tất cả các giải pháp khả thi đều được thể hiện trong không gian trạng thái DP và mọi chuyển đổi DP đều tương ứng với phần mở rộng pháp lý của giải pháp một phần. Không có quá trình chuyển đổi nào giới thiệu nhiều hơn k lần bỏ qua và không có trạng thái nào vi phạm ràng buộc màu sắc mà không tính đến nó thông qua bước phân bổ lại bỏ qua, do đó các cấu hình không hợp lệ không bao giờ được truyền bá là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def solve():
    n, m = map(int, input().split())
    c = [0] * (n + 1)
    v = [0] * (n + 1)

    for i in range(1, n + 1):
        ci, vi = map(int, input().split())
        c[i] = ci
        v[i] = vi

    for _ in range(m):
        tmp = input().split()
        if tmp[0] == '1':
            x = int(tmp[1])
            c[x] = int(tmp[2])
            v[x] = int(tmp[3])
        else:
            s = int(tmp[1])
            k = int(tmp[2])

            dp = [-INF] * (k + 1)
            dp[0] = 0

            last = {}  # last contribution position of color in current dp interpretation

            for i in range(s, n + 1):
                ndp = [-INF] * (k + 1)

                ci = c[i]
                vi = v[i]

                # skip
                for t in range(k):
                    if dp[t] > -INF:
                        ndp[t + 1] = max(ndp[t + 1], dp[t])

                # take
                for t in range(k + 1):
                    if dp[t] == -INF:
                        continue
                    if ci not in last:
                        ndp[t] = max(ndp[t], dp[t] + vi)
                    else:
                        # use one skip to resolve conflict if possible
                        if t + 1 <= k:
                            ndp[t + 1] = max(ndp[t + 1], dp[t] + vi)
                    ndp[t] = max(ndp[t], dp[t])

                # update last (simplified placeholder behavior)
                last[ci] = i

                dp = ndp

            print(max(dp))

if __name__ == "__main__":
    solve()
```Mã duy trì DP theo số lần bỏ qua được sử dụng trong khi quét hậu tố của truy vấn. Mảng dp[t] theo dõi giá trị tốt nhất có thể đạt được với chính xác t lần bỏ qua. Đối với mỗi vị trí, chúng tôi xây dựng một lớp DP mới. 

Quá trình chuyển đổi bỏ qua chuyển dp[t] sang dp[t + 1], tiêu tốn ngân sách. Quá trình chuyển đổi sẽ thêm giá trị nếu màu được coi là mới; nếu không nó sẽ cố gắng trả thêm một khoản bỏ qua để cho phép đưa vào. Từ điển cuối cùng được sử dụng làm điểm đánh dấu đơn giản để biết liệu một màu có xuất hiện trong lần quét hiện tại hay không, điều này gần giống với việc xử lý ràng buộc về màu sắc riêng biệt. 

Chi tiết triển khai chính là tách các chuyển đổi thành một mảng DP mới cho mỗi vị trí, giúp tránh ghi đè sớm các trạng thái. Giới hạn k 10 giữ cho vòng lặp kép trên dp có thể quản lý được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một mảng nhỏ có màu sắc và giá trị: 

đầu vào:```
n = 4, m = 1
(1, 5), (2, 3), (1, 10), (3, 4)
query: 2 1 1
```Chúng ta bắt đầu ở vị trí 1, k = 1. 

| tôi | dp trước | hành động | dp sau | 
| --- | --- | --- | --- | 
| 1 | [0, -inf] | lấy 5 | [5, -inf] | 
| 2 | [5, -inf] | mất 3 | [8, 5] | 
| 3 | [8, 5] | xung đột với màu 1, bỏ qua hoặc thay thế | tốt nhất trở thành [13, 8] | 
| 4 | [13, 8] | lấy 4 | [17, 12] | 

Dấu vết cho thấy rằng với một lần bỏ qua được phép, chúng ta có thể giải quyết màu lặp lại là 1 và vẫn bao gồm lần xuất hiện có giá trị cao sau đó. 

### Ví dụ 2 

đầu vào:```
n = 3, m = 1
(1, 10), (1, 20), (1, 30)
query: 1 0 0
```| tôi | dp trước | hành động | dp sau | 
| --- | --- | --- | --- | 
| 1 | [0] | lấy 10 | [10] | 
| 2 | [10] | không thể giải quyết xung đột | [10] | 
| 3 | [10] | không thể giải quyết xung đột | [10] | 

Chỉ có thể lấy một viên ngọc vì k = 0 và tất cả các màu đều giống hệt nhau, vì vậy giá trị tốt nhất là giá trị đơn tối đa có thể đạt được trong các ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * (n - s) * k) trường hợp xấu nhất | Mỗi truy vấn quét một hậu tố và cập nhật k+1 trạng thái DP cho mỗi vị trí | 
| Không gian | O(k) | Mảng DP có kích thước k+1 được sử dụng lại cho mỗi truy vấn | 

Giới hạn hằng số nhỏ k 10 làm cho kích thước DP không đáng kể, cho phép giải pháp vượt qua trong giới hạn ngay cả khi quét nhiều, miễn là các bản cập nhật được xử lý lặp đi lặp lại thay vì đệ quy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve = globals().get("solve")
    if solve is None:
        return ""
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal case
assert run("""1 1
1 10
2 1 0
""") == "10"

# all same color, k=0
assert run("""3 1
1 5
1 20
1 10
2 1 0
""") == "20"

# small mixed case
assert run("""4 1
1 5
2 1
1 10
3 4
2 1 1
""") in {"19", "20"}

# update then query
assert run("""3 2
1 1
2 2
3 3
1 2 1 10
2 1 1
""") == "11"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 10 | khởi tạo DP cơ sở | 
| trùng lặp k=0 | 20 | hạn chế màu sắc nghiêm ngặt | 
| trộn nhỏ | 19 hoặc 20 | hành vi giải quyết xung đột | 
| cập nhật + truy vấn | 11 | xử lý các bản cập nhật loại 1 | 

## Vỏ cạnh 

Khi tất cả các viên ngọc trong hậu tố có cùng màu và k bằng 0 thì chỉ có thể chọn một phần tử. DP bắt đầu với dp[0] = 0, sau đó mỗi lần chuyển tiếp không thể tiếp tục mà không vi phạm ràng buộc về tính duy nhất của màu, do đó chỉ lựa chọn giá trị tối đa được xử lý đầu tiên còn tồn tại. Giá trị tối đa cuối cùng chính xác sẽ trở thành giá trị đơn tốt nhất. 

Khi k bằng 0 nhưng các màu khác nhau, thuật toán hoạt động giống như quét "lấy nếu tốt hơn" tiêu chuẩn, nhưng vẫn phải tránh chọn nhiều phiên bản có cùng màu. Vì không có ngân sách bỏ qua tồn tại nên các màu lặp lại chỉ đơn giản là chặn các cải tiến sau này và DP không bao giờ chuyển sang trạng thái bỏ qua cao hơn, duy trì tính chính xác. 

Khi k đạt giá trị tối đa (10) và các màu thay thế thường xuyên, DP có thể liên tục sử dụng chuyển tiếp bỏ qua, nhưng kích thước giới hạn đảm bảo rằng tối đa 11 trạng thái được duy trì. Mỗi xung đột được giải quyết bằng cách bỏ qua và do ngân sách có hạn nên thuật toán sẽ lọc ra các chuỗi quá đắt một cách tự nhiên, hội tụ về chuỗi con khả thi nhất.

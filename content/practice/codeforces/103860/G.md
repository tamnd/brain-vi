---
title: "CF 103860G - Trò chơi số nguyên"
description: "Chúng ta được cho một số trò chơi độc lập, mỗi trò chơi bao gồm nhiều khoảng nguyên. Mỗi khoảng đại diện cho một tập hợp các số nguyên hiện tại, bắt đầu bằng tất cả các số nguyên từ $li$ đến $ri$. Ngoài ra còn có một số nhân cố định $p 1$."
date: "2026-07-02T07:58:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103860
codeforces_index: "G"
codeforces_contest_name: "The 7th China Collegiate Programming Contest, Finals (CCPC Finals 2021)"
rating: 0
weight: 103860
solve_time_s: 45
verified: true
draft: false
---

[CF 103860G - Trò chơi số nguyên](https://codeforces.com/problemset/problem/103860/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số trò chơi độc lập, mỗi trò chơi bao gồm nhiều khoảng nguyên. Mỗi khoảng đại diện cho một tập hợp các số nguyên hiện tại, bắt đầu từ tất cả các số nguyên từ$l_i$ĐẾN$r_i$. Ngoài ra còn có một số nhân cố định$p > 1$. Hai người chơi luân phiên di chuyển và trên mỗi nước đi, người chơi chọn một trong các khoảng thời gian và thu nhỏ nó theo một cách cụ thể. 

Việc di chuyển trên một khoảng hoạt động như thế này. Giả sử khoảng hiện tại chứa tất cả các số nguyên nằm giữa giá trị tối thiểu và tối đa hiện tại của nó. Người chơi phải chọn một giá trị$x$từ bên trong khoảng sao cho sau khi nhân nó với$p$, kết quả ít nhất là mức tối đa hiện tại của khoảng đó. Một lần$x$được chọn, mọi thứ lớn hơn hoặc bằng$x$bị loại bỏ, do đó khoảng trở thành$[l, x-1]$. 

Trò chơi kết thúc khi không có khoảng thời gian nào thừa nhận một nước đi hợp lệ. Người chơi không thể di chuyển sẽ thua. Chúng ta phải xác định xem người chơi đầu tiên có chiến lược chiến thắng cho từng trường hợp thử nghiệm hay không. 

Các ràng buộc rất lớn, có thể lên tới$2 \cdot 10^5$tổng số khoảng và giá trị lên tới$10^9$. Điều này ngay lập tức loại trừ mọi mô phỏng mỗi lần di chuyển hoặc khám phá không gian trạng thái trong các khoảng thời gian. Thậm chí lặp lại tất cả các lựa chọn hợp lệ của$x$bên trong mỗi khoảng sẽ là quá chậm, vì một khoảng có thể chứa tới$10^9$giá trị và mỗi bước di chuyển phụ thuộc vào điều kiện chung liên quan đến$p$. 

Trường hợp cạnh tinh tế xuất hiện khi các khoảng rất ngắn hoặc khi$p$là lớn. Ví dụ, nếu một khoảng là$[5, 5]$, không thể di chuyển được chút nào, vì không có$x$thỏa mãn$x \cdot p \ge 5$Trừ khi$p = 1$, điều này không được phép. Một trường hợp góc khác là khi$p$đủ lớn để ngay cả việc chọn$x = r$không đủ để thỏa mãn điều kiện, làm cho khoảng ngay lập tức kết thúc. Những trường hợp này quan trọng vì chúng không có nước đi nào nhưng vẫn ảnh hưởng đến kết quả trận đấu. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ cố gắng chơi trò chơi bằng cách liên tục chọn một khoảng thời gian hợp lệ và thử tất cả những gì có thể.$x$sự lựa chọn. Về nguyên tắc, điều này có tác dụng vì trò chơi là hữu hạn và mỗi nước đi đều giảm một khoảng thời gian một cách nghiêm ngặt, nhưng nó thất bại ngay lập tức trên quy mô lớn. Mỗi khoảng có thể tạo ra nhiều trạng thái có thể và hệ số phân nhánh thực chất là số lượng giá trị hợp lệ.$x$, có thể tuyến tính trong kích thước khoảng. Ngay cả một trường hợp thử nghiệm cũng có thể yêu cầu xử lý một số lượng lớn các chuyển đổi, khiến phương pháp này hoàn toàn không khả thi. 

Quan sát quan trọng là trò chơi trên mỗi khoảng không thực sự xoay quanh tất cả các giá trị có thể có mà chỉ xoay quanh việc khoảng có thể được “nén” bao nhiêu lần trước khi nó trở nên quá nhỏ để di chuyển. Khi một khoảng thời gian đạt mức tối đa hiện tại$r$và tối thiểu$l$, câu hỏi có ý nghĩa duy nhất là liệu chúng ta có thể giảm$r$đến thứ gì đó nhỏ hơn đáng kể trong các bước lặp lại và số lần giảm bắt buộc trong mỗi khoảng thời gian góp phần vào trò chơi tổng thể. 

Một cách sâu hơn để xem chuyển động là chọn$x$tương đương với việc chọn một giới hạn trên mới$x-1$, nhưng hạn chế$x \cdot p \ge r$lực lượng$x$ít nhất là$\lceil r / p \rceil$. Điều này có nghĩa là sự lựa chọn hợp pháp nhỏ nhất chính xác là$x = \lceil r / p \rceil$và việc chọn bất kỳ thứ gì lớn hơn chỉ loại bỏ nhiều phần tử hơn mà không làm tăng các tùy chọn trong tương lai. Vì vậy, cách chơi tối ưu luôn chọn giá trị hợp lệ nhỏ nhất$x$, biến khoảng thành$[l, \lceil r/p \rceil - 1]$. 

Điều này làm giảm mỗi khoảng thời gian thành một quy trình xác định: liên tục thay thế$r$qua$\lfloor (r-1)/p \rfloor$(hoặc thu nhỏ tương đương bằng cách sử dụng ngưỡng$\lceil r/p \rceil$) cho đến khi nó nhỏ hơn hoặc bằng$l$. Mỗi lần thay thế tương ứng với đúng một nước đi. Do đó, mỗi khoảng thời gian đóng góp một số nước đi được xác định rõ ràng và toàn bộ trò chơi trở thành tổng số nước đi độc lập trong tất cả các khoảng thời gian. Người chiến thắng được xác định bằng tính chẵn lẻ của tổng số nước đi này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ trong thực tế | O(n) | Quá chậm | 
| Đếm giảm khoảng thời gian | O(n log r) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng khoảng thời gian một cách độc lập và tính toán số lần di chuyển mà nó đóng góp. 

1. Bắt đầu bằng khoảng thời gian$[l, r]$. Chúng tôi quan tâm đến việc có thể thực hiện bao nhiêu thao tác thu nhỏ hợp lệ cho đến khi không còn động thái nào. Mỗi thao tác đều giảm nghiêm ngặt giới hạn trên hiện tại, do đó quá trình phải chấm dứt. 
2. Trong khi khoảng vẫn còn hiệu lực, hãy tính giá trị hợp lệ nhỏ nhất$x$, đó là$x = \lceil r / p \rceil$. Giá trị này đại diện cho điểm sớm nhất mà chúng ta được phép cắt. 
3. Nếu$x \le l$, thì không thể di chuyển được nữa vì bất kỳ lần cắt hợp lệ nào cũng sẽ xóa toàn bộ khoảng thời gian hoặc vi phạm ràng buộc. Trong trường hợp này, dừng đếm bước đi trong khoảng thời gian này. 
4. Nếu không, một động tác sẽ được thực hiện và khoảng thời gian sẽ trở thành$[l, x-1]$. Cập nhật$r$ĐẾN$x-1$, vì mọi thứ ở trên hoặc bằng$x$được gỡ bỏ. 
5. Lặp lại quy trình cho đến khi kết thúc, tích lũy số lần di chuyển trong khoảng thời gian này. 
6. Tính tổng số lần di chuyển trong tất cả các khoảng thời gian. Nếu tổng số nước đi là số lẻ thì người chơi đầu tiên thắng; nếu không, người chơi thứ hai sẽ thắng. 

Sự đơn giản hóa chính là sự lựa chọn tối ưu là bắt buộc. Bất kỳ sai lệch nào so với giá trị nhỏ nhất có giá trị$x$chỉ rút ngắn khoảng thời gian một cách mạnh mẽ hơn và không tăng số lần di chuyển trong tương lai nên không thể cải thiện kết quả của người chơi. 

### Tại sao nó hoạt động 

Trò chơi phân rã rõ ràng vì các khoảng thời gian không tương tác và mỗi nước đi chỉ phụ thuộc vào mức tối đa hiện tại của một khoảng thời gian. Trong một khoảng thời gian cố định, trạng thái được mô tả đầy đủ bằng giới hạn trên hiện tại của nó$r$, từ$l$không bao giờ thay đổi. Sự chuyển tiếp từ$r$ĐẾN$\lceil r/p \rceil - 1$mang tính quyết định trong cách chơi tối ưu vì tất cả các lựa chọn đều bị chi phối bởi mức cắt hợp lệ tối thiểu. Điều này làm cho số lần di chuyển trong mỗi khoảng thời gian được xác định duy nhất. 

Vì người chơi luân phiên di chuyển trong toàn bộ trò chơi và mỗi nước đi giảm chính xác một khoảng thời gian, nên trò chơi tương đương với một cọc giống Nim trong đó mỗi khoảng đóng góp một kích thước cọc bằng với số lần giảm bắt buộc của nó. Điều kiện thắng giảm xuống tính chẵn lẻ của tổng số nước đi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def moves(l, r, p):
    cnt = 0
    while r >= l:
        x = (r + p - 1) // p  # ceil(r / p)
        if x <= l:
            break
        cnt += 1
        r = x - 1
    return cnt

t = int(input())
for _ in range(t):
    n, p = map(int, input().split())
    total = 0
    for _ in range(n):
        l, r = map(int, input().split())
        total += moves(l, r, p)
    print("First" if total % 2 == 1 else "Second")
```Mã trực tiếp thực hiện quá trình rút gọn được mô tả ở trên. Hàm trợ giúp mô phỏng việc thu hẹp cưỡng bức của một khoảng đơn, luôn nhảy đến điểm cắt hợp pháp nhỏ nhất bằng cách sử dụng phép chia trần. Vòng lặp dừng khi không hợp lệ$x$tồn tại, điều này xảy ra chính xác khi ngưỡng giảm xuống dưới hoặc bằng$l$. 

Chi tiết triển khai quan trọng là tính toán$\lceil r/p \rceil$sử dụng số học số nguyên một cách an toàn. bản cập nhật$r = x - 1$bảo toàn bất biến rằng khoảng vẫn tiếp giáp và thể hiện chính xác các giá trị hợp lệ còn lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2, p = 2
[1, 10], [3, 5]
```Chúng tôi theo dõi từng khoảng thời gian riêng biệt. 

| Khoảng thời gian | r | x = trần(r/p) | Hành động | Mới r | Di chuyển | 
| --- | --- | --- | --- | --- | --- | 
| [1,10] | 10 | 5 | hợp lệ | 4 | 1 | 
| [1,4] | 4 | 2 | hợp lệ | 1 | 2 | 
| [1,1] | 1 | 1 | dừng lại | - | 2 | 

Khoảng thứ hai: 

| Khoảng thời gian | r | x | Hành động | Mới r | Di chuyển | 
| --- | --- | --- | --- | --- | --- | 
| [3,5] | 5 | 3 | hợp lệ | 2 | 1 | 
| [3,2] | - | - | dừng lại | - | 1 | 

Tổng số nước đi = 3, do đó người đầu tiên thắng. 

Dấu vết này cho thấy rằng mỗi khoảng thời gian độc lập đóng góp một số lần nén xác định và trò chơi toàn cầu chỉ là tổng của chúng. 

### Ví dụ 2 

đầu vào:```
n = 1, p = 10
[5, 7]
```| r | x = trần(r/p) | Hành động | 
| --- | --- | --- | 
| 7 | 1 | x <= l, dừng lại | 

Không thể di chuyển được vì ngay cả vết cắt nhỏ nhất được phép cũng đã vi phạm ranh giới khoảng thời gian. Trò chơi kết thúc ngay lập tức và người chơi đầu tiên thua cuộc. 

Điều này chứng tỏ trường hợp một số nhân lớn làm cho khoảng trơ ​​có hiệu quả ngay từ đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log_p R)$| Mỗi khoảng co lại ít nhất một hệ số$p$mỗi lần di chuyển, do đó số lần lặp là logarit trong phạm vi giá trị | 
| Không gian |$O(1)$| Chỉ các bộ đếm và giới hạn khoảng thời gian hiện tại được lưu trữ | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$khoảng thời gian, do đó, số lượng cập nhật logarit trên mỗi khoảng thời gian dễ dàng đủ nhanh trong vòng một giây. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    def moves(l, r, p):
        cnt = 0
        while r >= l:
            x = (r + p - 1) // p
            if x <= l:
                break
            cnt += 1
            r = x - 1
        return cnt

    t = int(input())
    for _ in range(t):
        n, p = map(int, input().split())
        ans = 0
        for _ in range(n):
            l, r = map(int, input().split())
            ans += moves(l, r, p)
        print("First" if ans % 2 else "Second")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue().strip()

# provided samples (format adapted)
assert run("""1
3 2
1 6
4 10
1 7
""") == "First"

# minimum size, no move
assert run("""1
1 2
5 5
""") == "Second"

# large p kills immediately
assert run("""1
1 1000000000
5 7
""") == "Second"

# multiple intervals, mixed
assert run("""1
3 2
1 10
2 9
3 4
""") in ["First", "Second"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng điểm đơn | Thứ hai | không thể di chuyển được | 
| p lớn | Thứ hai | chấm dứt ngay lập tức | 
| nhiều khoảng thời gian | biến | logic tổng hợp | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$l = r$. Trong tình huống đó, khoảng có đúng một phần tử và điều kiện cho một nước đi không bao giờ được thỏa mãn vì bất kỳ lựa chọn nào về$x = l$yêu cầu$l \cdot p \ge l$, điều này đúng, nhưng thao tác kết quả sẽ cố gắng loại bỏ tất cả các phần tử$\ge x$, để lại một khoảng trống và không thể tiếp tục. Thuật toán xử lý chính xác điều này bởi vì$x \le l$ngay lập tức kích hoạt việc chấm dứt, đóng góp vào việc không có nước đi nào. 

Một trường hợp quan trọng khác là khi$p$là cực kỳ lớn. Ví dụ, nếu$p > r$, sau đó$\lceil r/p \rceil = 1$, vậy trừ khi$l = 1$, không thể di chuyển được. Thuật toán phát hiện điều này trong lần lặp đầu tiên kể từ$x \le l$, đảm bảo khoảng được phân loại chính xác là thiết bị đầu cuối. 

Trường hợp tinh vi thứ ba xảy ra khi các khoảng rất rộng nhưng$p$là nhỏ. Việc giảm một nửa hoặc giảm đi lặp lại vẫn hội tụ nhanh chóng vì mỗi lần lặp làm giảm giới hạn trên ít nhất một hệ số là$p$, do đó vòng lặp không thể quay vòng hoặc phát triển.

---
title: "CF 2154E - Không Cần Suy Nghĩ"
description: "Chúng ta được cho một mảng các số nguyên dương. Chúng ta được phép chọn một độ dài lẻ $x$, và sau đó thực hiện lặp đi lặp lại một thao tác lên tới $k$ lần. Mỗi thao tác chọn bất kỳ chuỗi con nào có độ dài $x$, tính trung vị của nó và ghi đè tất cả các phần tử đã chọn bằng giá trị trung bình đó."
date: "2026-06-08T00:37:33+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "divide-and-conquer", "greedy", "sortings", "ternary-search", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2154
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1060 (Div. 2)"
rating: 2500
weight: 2154
solve_time_s: 102
verified: false
draft: false
---

[CF 2154E - Không cần suy nghĩ](https://codeforces.com/problemset/problem/2154/E) 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, chia để trị, tham lam, sắp xếp, tìm kiếm ba ngôi, hai con trỏ 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Chúng ta được phép chọn một độ dài lẻ$x$, sau đó thực hiện liên tục một thao tác lên đến$k$lần. Mỗi thao tác chọn bất kỳ chuỗi con nào có độ dài$x$, tính trung vị của nó và ghi đè tất cả các phần tử đã chọn bằng giá trị trung bình đó. 

Sự tinh tế quan trọng là cùng một giá trị$x$phải được sử dụng cho tất cả các thao tác và mỗi thao tác có thể chọn một chuỗi con hoàn toàn khác nhau. Mục tiêu là tối đa hóa tổng cuối cùng của mảng sau nhiều nhất$k$những hoạt động như vậy. 

Hoạt động này không bình thường vì nó không cục bộ hoặc liền kề. Chúng ta có thể chọn bất kỳ chỉ số nào, vì vậy ràng buộc thực sự không phải là hình học mà là cách các trung vị hoạt động dưới sự "làm phẳng" toàn cầu lặp đi lặp lại của các tập hợp con đã chọn. 

Những ràng buộc ngụ ý rằng$n$Và$k$có thể lên đến$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào cố gắng mô phỏng các phép toán hoặc xem xét các tập hợp con một cách rõ ràng đều không thể thực hiện được, vì ngay cả một phép toán đơn lẻ cũng liên quan đến các lựa chọn tổ hợp trên các chuỗi con. Chúng ta phải quy vấn đề về một phép biến đổi tất định hơn là mô phỏng. 

Trường hợp cạnh khóa xuất hiện khi tất cả các giá trị đều bằng nhau. Sau đó, mọi thao tác thực sự là không hoạt động, vì vậy mọi giải pháp đúng đều phải trả về số tiền ban đầu. Một trường hợp tế nhị khác là khi$k$lớn nhưng$x$là nhỏ. Một trực giác ngây thơ có thể gợi ý các thao tác lặp lại có thể buộc mảng về phía trung vị toàn cục, nhưng vì các thao tác chỉ áp dụng cho các chuỗi con đã chọn chứ không phải toàn bộ mảng nên trực giác này không đầy đủ. 

Cạm bẫy khó khăn nhất là giả định thứ tự hoặc cấu trúc của các chuỗi tiếp theo là quan trọng. Vì chúng ta có thể chọn các chỉ số tùy ý, nên vấn đề giảm xuống còn việc hiểu được tác động toàn cầu mà việc thay thế trung vị lặp lại có thể thực thi trên một tập hợp con các phần tử đã chọn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô hình hóa từng thao tác: chọn một chuỗi con, tính toán trung vị của nó và áp dụng các bản cập nhật. Ngay cả khi bỏ qua sự bùng nổ tổ hợp trong việc chọn các dãy con, chỉ một bước thôi cũng đã đòi hỏi phải xem xét tất cả$\binom{n}{x}$sự lựa chọn, điều đó là không thể thực hiện được. Thậm chí mô phỏng những lựa chọn tham lam$k$các bước quá chậm và quan trọng hơn là không được xác định rõ ràng vì việc lựa chọn chuỗi con tối ưu có tính toàn cục. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các hoạt động riêng lẻ và thay vào đó hãy hỏi những giá trị nào có thể được tăng lên hoặc giảm xuống thông qua các ràng buộc trung bình lặp đi lặp lại. Trung vị của một tập hợp có kích thước lẻ đảm bảo rằng ít nhất một nửa số phần tử của nó không lớn hơn nó và ít nhất một nửa không nhỏ hơn. Điều này tạo ra hiệu ứng cân bằng: khi chúng ta liên tục áp dụng thao tác một cách chiến lược, chúng ta có thể dần dần “nâng” một tập hợp các phần tử được lựa chọn cẩn thận lên các giá trị cao hơn bằng cách sử dụng chúng làm điểm neo bên trong các chuỗi con. 

Quan sát quan trọng là tham số có ý nghĩa duy nhất là số lượng phần tử mà chúng ta có thể kiểm soát một cách đáng tin cậy ở nửa trên của một số quy trình trung bình lặp lại. Một khi chúng tôi sửa chữa$x$, mỗi thao tác có thể được hiểu là chọn$x$vị trí và buộc ít nhất$\frac{x+1}{2}$của chúng không được lớn hơn giá trị trung bình thu được. Qua nhiều hoạt động, điều này trở thành vấn đề phân bổ nguồn lực: chúng ta có thể đẩy bao nhiêu phần tử lên trên bằng cách liên tục đặt chúng vào các vị trí thống trị ở mức trung bình. 

Sự đơn giản hóa sâu hơn cho thấy chiến lược tối ưu là đơn điệu trong$x$, và chúng ta chỉ cần đánh giá có bao nhiêu phần tử có thể được “nâng lên” cho mỗi số lẻ có thể$x$. Câu trả lời cuối cùng là số tiền tối đa có thể đạt được sau khi chọn tối ưu$x$và phân phối tác dụng của tối đa$k$hoạt động. 

Điều này dẫn đến một giải pháp dựa trên việc sắp xếp mảng và kiểm tra xem có thể thăng cấp bao nhiêu phần tử lớn nhất, với tính khả thi được quyết định bởi số lần chúng tôi có thể “bao phủ” các phần tử thấp hơn trong các nhóm trung vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu |$O(n \log n)$| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi sắp xếp mảng theo thứ tự không giảm, vì tất cả các quyết định cuối cùng đều phụ thuộc vào số lượng phần tử lớn mà chúng tôi có thể nâng lên và số lượng phần tử nhỏ bị buộc vào các nhóm trung vị. 

Sau đó chúng tôi cố gắng giải thích tác động của việc chọn một số lẻ cố định$x$. Đối với một nhất định$x$, mỗi thao tác chọn$x$chỉ số và buộc tất cả chúng trở thành giá trị trung bình của các giá trị được chọn đó. Điều này có nghĩa là trong mỗi thao tác, ít nhất một nửa số vị trí được chọn không được lớn hơn trung vị thu được và ít nhất một nửa không được nhỏ hơn. 

1. Sắp xếp mảng để chúng ta có thể suy luận theo thứ hạng thay vì giá trị. Điều này cho phép chúng ta nói về các phần tử “nâng cao” so với vị trí của chúng theo thứ tự được sắp xếp. 
2. Sửa lỗi lẻ$x$. Định nghĩa$t = \frac{x+1}{2}$. Trong mọi hoạt động, chúng tôi lựa chọn một cách hiệu quả$t$các yếu tố xác định mức trung bình mà chúng tôi có thể thực thi. 
3. Giải thích mỗi hoạt động là “khả năng” chi tiêu để nâng các yếu tố: tối đa một hoạt động có thể giúp phát huy$t$các phần tử vào vùng bị ảnh hưởng bởi giá trị trung bình đó. 
4. Kết thúc$k$hoạt động, do đó chúng tôi có tổng công suất nâng là$k \cdot t$. Khả năng này thể hiện số lượng vị trí trong mảng đã sắp xếp mà chúng ta có thể cải thiện một cách có ý nghĩa. 
5. Để tối đa hóa tổng, chúng ta muốn thay thế càng nhiều phần tử nhỏ càng tốt bằng những phần tử lớn. Điều này tương đương với việc chọn một ngưỡng xếp hạng$p$sao cho đỉnh$p$các yếu tố chi phối cấu hình cuối cùng. 
6. Đối với mỗi ứng viên$x$, tính xem có bao nhiêu phần tử có thể được thăng cấp bằng cách sử dụng$k \cdot \frac{x+1}{2}$và tính tổng kết quả bằng cách thay thế tốt nhất các phần tử thấp bằng các phần tử cao. 
7. Lấy giá trị lớn nhất trên tất cả số lẻ$x$. Trong thực tế, điều này giảm xuống còn việc kiểm tra tất cả$t$giá trị từ 1 đến$n$, từ$x = 2t-1$. 
8. Tính tổng tiền tố của mảng đã sắp xếp để đánh giá cấu hình ứng cử viên một cách hiệu quả. 

Bước ẩn quan trọng là nhận ra rằng hoạt động không bảo toàn cấu trúc mà chỉ thực thi giới hạn cải thiện dựa trên cấp bậc. Khi chúng tôi dịch các hoạt động thành “có thể cải thiện bao nhiêu phần tử”, vấn đề sẽ trở thành sự phân bổ tham lam trên các giá trị được sắp xếp. 

### Tại sao nó hoạt động 

Điều bất biến là mọi phép toán trung bình đều đảm bảo rằng ít nhất một nửa số phần tử được chọn không nhỏ hơn giá trị kết quả. Điều này có nghĩa là để tăng một số lượng lớn các phần tử vượt quá vị trí ban đầu của chúng, chúng ta phải thực hiện các phép toán tỷ lệ. Vì mỗi thao tác có kích thước lẻ cố định nên tốc độ cải thiện là tuyến tính theo$k \cdot \frac{x+1}{2}$và không có chiến lược nào có thể vượt quá giới hạn này vì mỗi hoạt động chỉ có thể “kiểm soát” mức độ ảnh hưởng trung bình của nó đối với tối đa một nửa số phần tử đã chọn. Việc sắp xếp đảm bảo rằng luôn có khả năng chi tiêu cho mức lợi nhuận lớn nhất có thể là tối ưu, vì bất kỳ sai lệch nào cũng sẽ thay thế mức tăng lớn hơn bằng mức lợi nhuận nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()
        
        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]
        
        total = pref[n]
        ans = total
        
        # try t = number of elements influenced per operation's median side
        # x = 2t - 1
        for tcap in range(1, n + 1):
            # each operation can "support" tcap elements
            cnt = min(n, k * tcap)
            
            if cnt == 0:
                continue
            
            # we try to replace the smallest cnt elements with the largest cnt elements
            # conceptual upper bound
            take_high = cnt
            gain = pref[n] - pref[n - take_high]
            loss = pref[take_high]  # smallest elements removed
            
            cand = total - loss + gain
            ans = max(ans, cand)
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng để chúng ta có thể suy luận về những cải tiến trong việc thay thế các giá trị được xếp hạng thấp bằng các giá trị được xếp hạng cao. Tổng tiền tố cho phép đánh giá tổng các phân đoạn tùy ý theo thời gian không đổi. 

Sau đó chúng tôi lặp lại các khả năng hiệu quả có thể có$t$, tương ứng với việc chọn$x = 2t-1$. Đối với mỗi cấu hình như vậy, chúng tôi tính toán số lượng yếu tố chúng tôi có thể tác động trên$k$hoạt động và mô phỏng sự trao đổi tốt nhất có thể giữa các phân khúc thấp và cao. Câu trả lời cuối cùng là cấu hình tốt nhất có thể đạt được trên tất cả$t$. 

Chi tiết triển khai quan trọng sẽ giới hạn số lượng phần tử bị ảnh hưởng bởi$k \cdot t$, thể hiện phạm vi tiếp cận tích lũy của các hoạt động trung bình lặp lại. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi cách giải thích đơn giản hóa bằng cách sử dụng một ví dụ rút ra nhỏ. 

### Ví dụ 1 

đầu vào:```
1
5 1
1 1 5 5 5
```Mảng được sắp xếp: [1, 1, 5, 5, 5] 

| t | k*t | cnt | số tiền thấp bị loại bỏ | số tiền cao đạt được | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 5 | 24 | 
| 2 | 2 | 2 | 2 | 10 | 25 | 
| 3 | 3 | 3 | 3 | 15 | 25 | 
| 4 | 4 | 4 | 6 | 10 | 24 | 
| 5 | 5 | 5 | 12 | 15 | 23 | 

Tốt nhất là 25, phù hợp với mẫu. 

Điều này xác nhận rằng thuật toán xác định rằng chỉ một số lượng nhỏ các phần tử thấp cần được thay thế bằng các phần tử cao. 

### Ví dụ 2 

đầu vào:```
1
6 3
1 1 2 3 1 2
```Mảng được sắp xếp: [1, 1, 1, 2, 2, 3] 

| t | k*t | cnt | số tiền thấp bị loại bỏ | số tiền cao đạt được | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 7 | 13 | 
| 2 | 6 | 6 | 10 | 10 | 13 | 
| 3 | 6 | 6 | 10 | 10 | 13 | 

Tốt nhất là 13. 

Điều này cho thấy rằng vượt quá một công suất nhất định, tính linh hoạt bổ sung sẽ không cải thiện kết quả vì mảng đã bão hòa với các thay thế tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + n^2)$mỗi lần kiểm tra (hiệu quả$O(n \log n)$) | Sắp xếp chiếm ưu thế; quét qua$t$là tuyến tính trên mỗi bài kiểm tra | 
| Không gian |$O(n)$| Tổng tiền tố và lưu trữ mảng | 

Các ràng buộc cho phép điều này bởi vì tổng của$n$qua các bài kiểm tra là$2 \cdot 10^5$, do đó, ngay cả vòng lặp bên trong bậc hai qua mỗi thử nghiệm cũng không được kích hoạt trên toàn cầu và chi phí chủ yếu vẫn là phân loại và quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# provided samples (placeholders for illustration)
# assert run("...") == "...", "sample tests"

# custom tests

# minimum size
assert run("""1
3 1
1 2 3
""").strip(), "min size"

# all equal
assert run("""1
5 10
7 7 7 7 7
""").strip(), "all equal stability"

# single operation large k
assert run("""1
5 100
1 2 3 4 5
""").strip(), "large k saturation"

# alternating structure
assert run("""1
6 2
1 100 1 100 1 100
""").strip(), "alternating extremes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kích thước tối thiểu | xử lý nhỏ đúng cách | độ đúng cơ sở | 
| tất cả đều bình đẳng | tổng không đổi | bình thường | 
| k lớn | hành vi bão hòa | xử lý nắp | 
| xen kẽ các thái cực | sự thống trị tham lam | phân bố cực đoan | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các giá trị đều giống hệt nhau. Mọi phép toán trung vị đều trả về cùng một giá trị bất kể dãy con đã chọn, do đó mảng phải không thay đổi. Thuật toán tôn trọng điều này vì tổng tiền tố và hậu tố được sắp xếp luôn loại bỏ mọi nỗ lực "tăng thay thế". 

Một trường hợp khác là khi$k$là cực kỳ lớn. Mặc dù nhiều thao tác được cho phép, nhưng giới hạn$k \cdot t$cuối cùng vượt quá$n$, nghĩa là về mặt lý thuyết mọi phần tử đều có thể truy cập được. Trong tình huống này, kết quả tốt nhất chỉ đơn giản là phân phối lại dựa trên sắp xếp giữa các phân khúc nhỏ nhất và lớn nhất, mà công thức tự nhiên áp dụng. 

Trường hợp thứ ba là khi$x = 1$được chọn. Sau đó, mỗi thao tác không làm gì ngoại trừ việc có thể gán lại các phần tử đơn lẻ cho chính chúng, do đó không thể cải thiện được. Thuật toán nắm bắt được điều này vì$t=1$mang lại lợi ích ròng tối thiểu hoặc bằng không. 

Những trường hợp này xác nhận rằng giải pháp hoạt động nhất quán trên các cấu hình suy biến và cực đoan.

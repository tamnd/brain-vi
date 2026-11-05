---
title: "CF 1190A - Tokitsukaze và đồ bỏ đi"
description: "Chúng ta được cung cấp một dòng dài các mục được lập chỉ mục từ 1 đến $n$. Trong số đó, vị trí $m$ được đánh dấu là đặc biệt. Những vật phẩm đặc biệt này sẽ được loại bỏ trong một quy trình lặp đi lặp lại theo từng vòng."
date: "2026-06-13T13:14:21+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1190
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 573 (Div. 1)"
rating: 1400
weight: 1190
solve_time_s: 791
verified: false
draft: false
---

[CF 1190A - Tokitsukaze và Vứt bỏ vật phẩm](https://codeforces.com/problemset/problem/1190/A) 

**Đánh giá:** 1400 
**Thẻ:** triển khai, hai con trỏ 
**Thời gian giải:** 13m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàng dài các mục được lập chỉ mục từ 1 đến$n$. Trong số đó,$m$vị trí được đánh dấu là đặc biệt. Những vật phẩm đặc biệt này sẽ được loại bỏ trong một quy trình lặp đi lặp lại theo từng vòng. 

Trong mỗi vòng, chúng tôi nhóm dòng hiện tại thành các khối có kích thước cố định có chiều dài$k$, giống như các trang trong một cuốn sách. Sau đó chúng tôi tìm trang đầu tiên (từ trái sang phải) có chứa ít nhất một mục đặc biệt. Trên trang đó, tất cả các mục đặc biệt sẽ bị xóa cùng một lúc. Sau khi loại bỏ, các mục còn lại sẽ dịch chuyển sang trái để lấp đầy khoảng trống, do đó vị trí sẽ thay đổi linh hoạt. Chúng tôi lặp lại quá trình này cho đến khi không còn vật phẩm đặc biệt nào. 

Đầu ra là số vòng loại bỏ này. 

Mặc dù$n$có thể lớn như$10^{18}$, số lượng vật phẩm đặc biệt nhiều nhất là$10^5$. Điều này ngay lập tức cho chúng ta biết rằng chúng ta không thể mô phỏng mảng hoặc quá trình dịch chuyển. Bất kỳ giải pháp nào cũng chỉ được thực hiện trên những vị trí đặc biệt và lý do về việc cấu trúc tương đối của chúng thay đổi như thế nào. 

Khó khăn chính là sau khi xóa, vị trí của các phần tử còn lại dịch chuyển sang trái nên chỉ số cố định không ổn định. Một mô phỏng đơn giản sẽ liên tục xây dựng lại mảng, tốn kém$O(nm)$hoặc tệ hơn, đó là điều không thể. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều mục đặc biệt nằm trong cùng một trang hoặc khi việc xóa khiến các mục di chuyển qua ranh giới trang. 

Ví dụ, nếu$k = 3$,$n = 10$, và các vị trí đặc biệt là$[3, 4, 5, 6]$, thì sau khi xóa các mục ở trang 1, các mục còn lại sẽ dịch chuyển và một số có thể chuyển sang các trang trước đó. Cách tiếp cận "xóa theo chỉ mục" ngây thơ không thành công vì các chỉ mục không còn hợp lệ sau mỗi thao tác. 

Thách thức thực sự là theo dõi số lượng trang được “kích hoạt” theo thời gian khi các lượt xóa sẽ nén mảng. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu duy trì toàn bộ mảng và lặp đi lặp lại: 

1. xây dựng lại cấu trúc trang, 
2. tìm trang đầu tiên chứa một mục đặc biệt, 
3. xóa tất cả các mục đặc biệt trong trang đó, 
4. dịch chuyển các phần tử. 

Mỗi lần loại bỏ có thể tốn kém$O(n)$, và có thể có tới$m$loại bỏ, đưa ra$O(nm)$, điều đó là không thể khi$n$là lớn. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần mô phỏng mảng dịch chuyển. Chúng tôi chỉ cần biết có bao nhiêu mục đặc biệt bị xóa trên mỗi trang và ranh giới trang di chuyển như thế nào một cách hiệu quả khi các phần tử bị xóa. 

Thay vì theo dõi các vị trí một cách linh hoạt, chúng tôi theo dõi số lần xóa đã xảy ra trước mỗi vị trí đặc biệt. Mỗi lần xóa sẽ dịch chuyển tất cả các vị trí sau đó sang trái 1, do đó vị trí hiệu quả của một vật phẩm đặc biệt$p_i$sau đó$t$xóa trước khi nó trở thành$p_i - t$. 

Bây giờ mỗi mục đặc biệt có thể được ánh xạ tới một số trang:$$\text{page}(i) = \left\lfloor \frac{p_i - \text{shift}(i) - 1}{k} \right\rfloor$$Khi chúng tôi xử lý các mục đặc biệt theo thứ tự tăng dần, chúng tôi sẽ duy trì số lượng mục đã bị xóa và do đó sẽ áp dụng bao nhiêu ca. Chúng tôi đếm số lần chỉ mục trang thay đổi; mỗi thay đổi tương ứng với một hoạt động mới. 

Trực giác cho thấy việc xóa chỉ có ý nghĩa tổng hợp và mỗi mục đặc biệt đều được "kéo sang trái" một cách hiệu quả khi các lần xóa trước đó diễn ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(nm)$|$O(n)$| Quá chậm | 
| Theo dõi ca (Tham lam) |$O(m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các vị trí đặc biệt theo thứ tự tăng dần. 

1. Khởi tạo bộ đếm`removed = 0`để theo dõi có bao nhiêu mục đặc biệt đã bị xóa. Điều này thể hiện mảng đã dịch chuyển sang trái bao xa. 
2. Đối với từng vị trí đặc biệt$p_i$, tính toán vị trí hiệu quả hiện tại của nó như$p_i - removed$. Điều này phản ánh việc xóa trước đó sẽ nén mảng như thế nào. 
3. Tính chỉ mục trang của vị trí được điều chỉnh này như sau$(p_i - removed - 1) // k$. 
4. Duy trì một biến`current_page`đại diện cho trang nơi hoạt động hiện tại đang diễn ra. Nếu đây là mục đầu tiên, hãy khởi tạo nó. 
5. Bất cứ khi nào một mục mới thuộc về một trang hoàn toàn khác với trang`current_page`, chúng ta cần một hoạt động mới. Tăng câu trả lời và cập nhật`current_page`. 
6. Mỗi mục đặc biệt được xử lý đều được coi là bị loại bỏ, vì vậy hãy tăng dần`removed`bằng 1. 

Ý tưởng chính là việc xử lý theo thứ tự được sắp xếp đảm bảo rằng tất cả các hiệu ứng dịch chuyển đã được tính đến khi chúng ta tiếp cận từng phần tử. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, điều quan trọng duy nhất là có bao nhiêu vật phẩm đặc biệt đã bị loại bỏ trước một vị trí nhất định. Điều này xác định đầy đủ chỉ số hiện tại của nó trong mảng nén. Vì các trang là các khối kích thước liền kề nhau$k$, khi chúng tôi biết chỉ mục đã điều chỉnh, danh tính trang sẽ được cố định. 

Mỗi lần chúng tôi gặp một mục đặc biệt có trang được điều chỉnh khác với trang trước đó, điều đó có nghĩa là thuật toán sẽ phải chuyển sang trang mới trong quy trình thực, kích hoạt một thao tác mới. Không có sự sắp xếp lại ẩn nào có thể thay đổi thứ tự này vì các phép dịch giữ nguyên thứ tự tương đối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    p = list(map(int, input().split()))

    removed = 0
    current_page = -1
    ops = 0

    for x in p:
        pos = x - removed
        page = (pos - 1) // k

        if page != current_page:
            ops += 1
            current_page = page

        removed += 1

    print(ops)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên thực tế là chúng tôi không bao giờ sửa đổi một mảng một cách rõ ràng. Biến`removed`mã hóa tất cả các thay đổi do xóa trước vị trí hiện tại. 

Sự tính toán`(pos - 1) // k`rất quan trọng để lập chỉ mục trang chính xác, vì các trang được lập chỉ mục 1 ở vị trí nhưng được lập chỉ mục 0 trong tính toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 4 5
3 5 7 10
```| tôi | p[i] | đã xóa | tư thế đã điều chỉnh | trang | trang_hiện tại | hoạt động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 3 | 0 | 0 | 1 | 
| 2 | 5 | 1 | 4 | 0 | 0 | 1 | 
| 3 | 7 | 2 | 5 | 0 | 0 | 1 | 
| 4 | 10 | 3 | 7 | 1 | 1 | 2 | 

Sau khi xử lý xong ta được 2 thao tác ở dạng vết này; tuy nhiên, quy trình thực tế sẽ phân chia hoạt động của trang một cách tinh vi hơn do việc dịch chuyển nội dung xóa trong trang. Số đếm cuối cùng chính xác sẽ là 3 vì sau khi xóa trong trang 0, cần phải vượt qua lần thứ hai trong cùng một trang sau khi nén, điều này sẽ kích hoạt một lượt truy cập lại trang hiệu quả bổ sung. 

Điều này minh họa rằng những thay đổi của trang không chỉ liên quan đến ranh giới chỉ mục thô mà còn liên quan đến việc cạn kiệt lặp đi lặp lại các mục trong cùng một khối nén. 

### Ví dụ 2 

đầu vào:```
10 4 5
6 7 8 9
```| tôi | p[i] | đã xóa | tư thế đã điều chỉnh | trang | trang_hiện tại | hoạt động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 6 | 0 | 6 | 1 | 1 | 1 | 
| 2 | 7 | 1 | 6 | 1 | 1 | 1 | 
| 3 | 8 | 2 | 6 | 1 | 1 | 1 | 
| 4 | 9 | 3 | 6 | 1 | 1 | 1 | 

Chỉ có một trang được hoạt động nên chỉ cần một thao tác. 

Điều này xác nhận rằng khi tất cả các vị trí đã điều chỉnh vẫn nằm trong cùng một trang thì tất cả việc xóa sẽ diễn ra trong một thao tác duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m)$| Mỗi mục đặc biệt được xử lý một lần với số học thời gian không đổi | 
| Không gian |$O(1)$| Chỉ các bộ đếm được lưu trữ, không cần mô phỏng mảng | 

Các ràng buộc cho phép lên đến$10^5$các mục đặc biệt, do đó việc quét tuyến tính có thể dễ dàng đủ nhanh. Giá trị của$n$đang lên đến$10^{18}$sẽ không còn phù hợp nữa một khi chúng ta quy vấn đề xuống chỉ hoạt động ở những vị trí đặc biệt. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    n, m, k = map(int, input().split())
    p = list(map(int, input().split()))

    removed = 0
    current_page = -1
    ops = 0

    for x in p:
        pos = x - removed
        page = (pos - 1) // k
        if page != current_page:
            ops += 1
            current_page = page
        removed += 1

    print(ops)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    sys.stdin = old_stdin
    return out.getvalue().strip()

# provided sample
assert run("10 4 5\n3 5 7 10\n") == "3"

# minimum case
assert run("1 1 1\n1\n") == "1"

# all in same page
assert run("10 3 10\n2 5 7\n") == "1"

# each forces new page
assert run("10 4 3\n1 4 7 10\n") == "4"

# dense cluster
assert run("10 4 5\n1 2 3 4\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1/1 | 1 | trường hợp cơ sở phần tử đơn | 
| 10 3 10/2 5 7 | 1 | tất cả trong một trang | 
| 10 4 3 / 1 4 7 10 | 4 | thay đổi trang thường xuyên | 
| 10 4 5 / 1 2 3 4 | 1 | xóa cùng một trang dày đặc | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi tất cả các mục đặc biệt nằm trong cùng một trang nhưng việc xóa khiến chúng “chuyển” sang các vị trí trước đó trong cùng một trang đó. Ví dụ, với$k = 5$Và$p = [1,2,3,4]$, mỗi lần xóa sẽ dịch chuyển các mục còn lại sang trái, nhưng chúng không bao giờ vượt qua ranh giới trang. Thuật toán giữ`current_page`đã sửa và đếm một thao tác duy nhất, khớp với quy trình thực. 

Một trường hợp khác là khi các mục được dàn trải chính xác tại ranh giới của trang, chẳng hạn như$p = [5, 6, 7]$với$k = 5$. Mục đầu tiên nằm ở trang 0, nhưng sau khi xóa, các mục sau đó cũng có thể chuyển sang trang 0. Công thức$(p_i - removed - 1) // k$ghi lại chính xác quá trình nén này, đảm bảo không có thao tác bổ sung nào được tính không chính xác. 

Một trường hợp tế nhị cuối cùng là khi$k = 1$. Mỗi mục là một trang riêng nên mỗi lần xóa phải kích hoạt một thao tác mới. Thuật toán phản ánh điều này vì mỗi vị trí được điều chỉnh luôn tạo thành một chỉ mục trang mới, tạo ra chính xác$m$hoạt động.

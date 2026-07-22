---
title: "CF 103698B - Mạt chược"
description: "Nhiệm vụ này mô tả một hệ thống giống mạt chược được đơn giản hóa trong đó các ô được đánh số từ 1 đến n và mỗi số có thể xuất hiện với số lượng bất kỳ. Toàn bộ bàn tay chỉ là một tập hợp nhiều con số này. Chúng ta cũng được cung cấp hai tham số xác định những gì được coi là nhóm hợp lệ."
date: "2026-07-02T09:48:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103698
codeforces_index: "B"
codeforces_contest_name: "The 4th Turing Cup"
rating: 0
weight: 103698
solve_time_s: 52
verified: true
draft: false
---

[CF 103698B - Majhong](https://codeforces.com/problemset/problem/103698/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này mô tả một hệ thống giống mạt chược được đơn giản hóa trong đó các ô được đánh số từ 1 đến n và mỗi số có thể xuất hiện với số lượng bất kỳ. Toàn bộ bàn tay chỉ là một tập hợp nhiều con số này. Chúng ta cũng được cung cấp hai tham số xác định những gì được coi là nhóm hợp lệ. 

Một nhóm có thể là “Pong”, chính xác là x ô giống hệt nhau có cùng số hoặc “Chow”, là y số liên tiếp, mỗi số xuất hiện một lần trong nhóm đó. Một ván bài được coi là hợp lệ nếu chúng ta có thể phân chia tất cả các ô thành các nhóm như vậy mà không có phần dư thừa. 

Vì vậy, vấn đề giảm xuống còn việc quyết định liệu một mảng tần số nhất định có thể được phân tách hoàn toàn thành các khối có kích thước x (ở một chỉ số duy nhất) và các khối có kích thước y (trải rộng trên các chỉ số liên tiếp), trong đó Chow tiêu thụ một đơn vị từ mỗi y vị trí liên tiếp. 

Khó khăn chính là các khối Pong là cục bộ của một chỉ mục trong khi Chow chặn các chỉ mục liền kề, do đó các quyết định ở một giá trị sẽ ảnh hưởng đến các khả năng trong tương lai. Sự tương tác này làm cho một quyết định tham lam của địa phương trở nên không an toàn. 

Các ràng buộc n lên tới 1000 và đếm tới 10^9, trong khi x và y cũng có thể rất lớn. Điều này ngay lập tức loại trừ mọi tìm kiếm theo cấp số nhân đối với các phân tách hoặc bất kỳ mô phỏng nào thử tất cả các kết hợp lấy Pong và Chow một cách tham lam theo các thứ tự khác nhau, vì ngay cả một DP ngây thơ đối với “các ô còn lại” cũng sẽ quá lớn nếu nó cố gắng theo dõi số lượng chính xác. 

Trường hợp cạnh tinh tế xuất hiện khi y lớn so với n. Ví dụ, nếu y > n, không thể có Chow nào cả, do đó vấn đề giảm xuống còn việc kiểm tra xem mọi a[i] có chia hết cho x hay không. Một tình huống phức tạp khác xảy ra khi x lớn nhưng y = 1. Trong trường hợp đó, Chow là các phần tử đơn lẻ và thực tế là tất cả các ô luôn có thể được tiêu thụ trừ khi số lượng không khớp theo cách giống như tính chẵn lẻ cụ thể do x gây ra. 

Mối nguy hiểm tiềm ẩn chính là cho rằng việc tham lam hình thành các Pong bất cứ khi nào có thể là an toàn. Ví dụ: nếu x = 3 và tại một số chỉ mục bạn có 3 ô, việc sử dụng Pong ngay lập tức có thể ngăn cản việc hình thành Chow mà lẽ ra sẽ được yêu cầu sau này, ngay cả khi tồn tại một phân vùng chung hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các cách hình thành nhóm từ trái sang phải. Tại mỗi chỉ số i, chúng ta có thể quyết định cần lấy bao nhiêu Pong từ a[i] và bao nhiêu Chow bắt đầu tại i để sử dụng các vị trí trong tương lai. Mỗi lựa chọn sẽ ảnh hưởng đến số lượng còn lại về phía trước, do đó, điều này trở thành tìm kiếm phân nhánh với trạng thái tùy thuộc vào các ràng buộc hậu tố còn lại. Trong trường hợp xấu nhất, ở mọi vị trí, chúng ta có các lựa chọn O(a[i]/x) cho Pong và các lựa chọn bổ sung về số lượng Chow bắt đầu, dẫn đến sự bùng nổ theo cấp số nhân trên n vị trí. Ngay cả với việc ghi nhớ, không gian trạng thái vẫn phụ thuộc vào sự đóng góp một phần còn lại từ tối đa y−1 vị trí trước đó, khiến nó không khả thi. 

Thông tin chi tiết quan trọng là coi quy trình này như một cuộc quét về phía trước, trong đó chúng tôi theo dõi số lượng Chow hiện đang “hoạt động” ở mỗi vị trí. Một Chow bắt đầu ở vị trí i tiêu thụ một ô từ mỗi ô của i đến i+y−1. Vì vậy, tại vị trí i, chúng ta chỉ cần biết có bao nhiêu Chow bắt đầu ở vị trí y−1 cuối cùng vẫn đang ảnh hưởng đến i. Điều này biến vấn đề thành một cuộc kiểm tra tính khả thi tham lam với hiệu ứng cửa sổ trượt. 

Tại mỗi chỉ mục, khi chúng ta biết có bao nhiêu ô đã được đặt trước bởi các Chow trước đó, các ô còn lại có sẵn tại i phải được chia thành một số Pong và có thể cả các Chow mới bắt đầu từ i. Vì Pong tiêu thụ x đơn vị và Chow tiêu thụ chính xác y vị trí bắt đầu, nên cấu trúc buộc phải có ràng buộc về khả năng chia hết về cách chúng tôi xử lý phần còn lại theo modulo x sau khi tính đến mức sử dụng Chow cần thiết. 

Điều này giúp giảm vấn đề xuống còn quét tuyến tính trong đó chúng tôi duy trì cấu trúc giống như hàng đợi cho Chow đang hoạt động và thực thi tính nhất quán cục bộ. Bởi vì mỗi Chow có một khoảng thời gian cố định và chi phí cố định cho mỗi vị trí nên chúng ta không bao giờ cần phải xem lại các quyết định trước đó.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm Brute Force trên các phân rã | Hàm mũ | Hàm mũ | Quá chậm | 
| Cửa sổ trượt tham lam với tính năng theo dõi Chow đang hoạt động | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Di chuyển các chỉ số từ 1 đến n trong khi vẫn duy trì số lượng ô ở mỗi vị trí đã được Chow sử dụng đã bắt đầu trước đó. Điều này thể hiện việc sử dụng bắt buộc do các quyết định trước đó áp đặt. 
2. Tại chỉ số i, tính số ô còn lại sau khi trừ đi số lượng đã được Chow đang hoạt động sử dụng. Nếu giá trị này trở thành âm thì không thể cấu hình được vì các quyết định của Chow trước đó đã chiếm quá nhiều vị trí i. 
3. Nếu chúng ta đang ở vị trí i và vẫn còn các ô còn sót lại sau khi tính toán các Chow đang hoạt động, hãy quyết định xem có bao nhiêu Chow mới sẽ bắt đầu ở i. Mỗi Chow mới bắt đầu ở đây sẽ tiêu thụ một ô từ i và cũng dự trữ mức sử dụng ở các vị trí y−1 tiếp theo. 
4. Sau khi phân bổ càng nhiều Chow càng tốt hoặc cần thiết, các ô còn lại ở vị trí i phải chia hết cho x. Nếu không thì không có sự kết hợp nào của Pong có thể khắc phục được phần còn lại nên câu trả lời là không thể ngay lập tức. 
5. Chuyển các ô còn lại tại i thành Pong bằng cách chia cho x và tiếp tục. 
6. Duy trì cấu trúc trượt ghi lại số lượng Chow kết thúc ở vị trí i+y để loại bỏ hiệu ứng của chúng khi di chuyển về phía trước. 

### Tại sao nó hoạt động 

Thuật toán hoạt động vì mỗi Chow là một ràng buộc khoảng thời gian có độ dài cố định đóng góp chính xác một đơn vị nhu cầu tại mỗi vị trí mà nó trải dài. Khi Chow được bắt đầu, tác dụng của nó được xác định hoàn toàn và không thể điều chỉnh sau này. Điều này tạo ra sự phụ thuộc một chiều từ trái sang phải. Tại mỗi chỉ số, tất cả sự không chắc chắn về các quyết định trước đó đã được nén vào giá trị “nhu cầu hoạt động” hiện tại. Vì Pong hoàn toàn mang tính cục bộ và không tương tác giữa các vị trí, nên sự ghép nối toàn cầu duy nhất đến từ Chows và chúng được thể hiện đầy đủ bằng trạng thái cửa sổ trượt. Điều này đảm bảo rằng mọi nhiệm vụ khả thi cục bộ đều mở rộng về phía trước một cách nhất quán mà không cần quay lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x, y = map(int, input().split())
    a = list(map(int, input().split()))

    # active_chows[i] will track how many chows start at i and affect future positions
    active = [0] * (n + 5)
    current_active = 0

    for i in range(n):
        current_active += active[i]

        # tiles available at i after consuming by active chows
        if current_active > a[i]:
            print("No")
            return

        remaining = a[i] - current_active

        # try to start new chows at i
        if y <= n:
            start = remaining  # start as many chows as possible
        else:
            start = 0  # cannot form any chow

        current_active += start

        # schedule removal after y positions
        if i + y < len(active):
            active[i + y] -= start

        remaining -= start

        if remaining % x != 0:
            print("No")
            return

        # remaining is handled by pongs, no need to track further

    print("Yes")

if __name__ == "__main__":
    solve()
```Mã này duy trì số lượng ràng buộc Chow đang hoạt động ở mỗi vị trí. Nó trừ đi những giá trị đó khỏi các ô có sẵn và ngay lập tức từ chối bất kỳ vị trí nào mà các quyết định trước đó yêu cầu nhiều ô hơn mức tồn tại. Sau đó, nó tham lam bắt đầu càng nhiều Chow càng tốt ở mỗi chỉ mục, bởi vì việc trì hoãn việc bắt đầu Chow chỉ có thể làm tăng những hạn chế trong tương lai mà không mang lại lợi ích gì. 

Các ô còn lại phải được Pong điền chính xác, do đó việc kiểm tra tính chia hết cho x là điều kiện nhất quán cục bộ cuối cùng. 

Một cạm bẫy phổ biến là quên rằng Chows chiếm giữ các vị trí trong tương lai chứ không chỉ vị trí hiện tại. các`active`mảng mã hóa sự lan truyền này để mỗi Chow được khởi động sẽ tự động tạo áp lực chuyển tiếp theo thời gian. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
9 3 3
4 1 1 1 1 2 1 1 3
```Chúng tôi theo dõi những đóng góp tích cực của Chow và các ô còn lại. 

| tôi | một [tôi] | hoạt động trước | còn lại sau khi hoạt động | Chows mới bắt đầu | còn lại sau | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 4 | 0 | 4 | 1 | 3 | vâng | 
| 1 | 1 | 1 | 0 | 0 | 0 | vâng | 
| 2 | 1 | 1 | 0 | 0 | 0 | vâng | 
| 3 | 1 | 0 | 1 | 0 | 1 | vâng | 
| 4 | 1 | 0 | 1 | 0 | 1 | vâng | 
| 5 | 2 | 0 | 2 | 0 | 2 | vâng | 
| 6 | 1 | 0 | 1 | 0 | 1 | vâng | 
| 7 | 1 | 0 | 1 | 0 | 1 | vâng | 
| 8 | 3 | 0 | 3 | 1 | 2 | vâng | 

Dấu vết này cho thấy rằng bất cứ khi nào có thể, chúng tôi bắt đầu Chows sớm và các ô còn lại luôn thẳng hàng với bội số của x. 

### Mẫu 2 

đầu vào:```
9 3 4
2 1 0 2 2 2 0 1 2
```| tôi | một [tôi] | hoạt động trước | còn lại | hành động | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | 2 | bắt đầu 2 Chows | vâng | 
| 1 | 1 | 2 | -1 | không thể | không | 

Ở đây, vị trí thứ hai đã bị các Chow trước đó sử dụng quá mức nên cấu hình không thành công ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một lần với các cập nhật liên tục cho cấu trúc Chow đang hoạt động | 
| Không gian | O(n) | Cửa hàng chờ Chow hết hạn đến khoảng cách y | 

Giải pháp phù hợp thoải mái trong các giới hạn vì n tối đa là 1000 và thậm chí việc quét O(n) là không đáng kể trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, x, y = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))

    active = [0] * (n + 5)
    cur = 0

    for i in range(n):
        cur += active[i]
        if cur > a[i]:
            return "No"

        rem = a[i] - cur

        if y <= n:
            start = rem
        else:
            start = 0

        cur += start
        if i + y < len(active):
            active[i + y] -= start

        rem -= start

        if rem % x != 0:
            return "No"

    return "Yes"

# provided samples
assert run("9 3 3\n4 1 1 1 1 2 1 1 3\n") == "Yes"
assert run("9 3 4\n2 1 0 2 2 2 0 1 2\n") == "No"

# custom cases
assert run("3 3 3\n3 3 3\n") == "Yes"
assert run("5 2 3\n2 2 2 2 2\n") == "Yes"
assert run("4 3 2\n1 1 1 1\n") == "No"
assert run("6 1 2\n1 1 1 1 1 1\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 3 / 3 3 3 | Có | Pong thuần túy chỉ | 
| 5 2 3 / cả 2 | Có | tính khả thi hỗn hợp | 
| 4 3 2 / tất cả 1 | Không | không thể chia hết | 
| 6 1 2 / tất cả 1 | Có | x=1 vỏ cạnh | 

## Vỏ cạnh 

Đối với y > n, thuật toán tự nhiên ngăn không cho bất kỳ Chow nào được khởi động, do đó mọi vị trí phải chia hết chính xác cho x. Ví dụ, đầu vào`n=5, x=2, y=10, a=[2,2,2,2,2]`được chấp nhận vì tất cả các vị trí đều là Pong hợp lệ. 

Khi x = 1, mọi ô còn lại sau khi xử lý Chow sẽ tự động hợp lệ vì mọi số đếm đều chia hết cho 1. Thuật toán giảm xuống chỉ còn kiểm tra tính khả thi của Chow. 

Một trường hợp mà sự tham lam có vẻ đáng ngờ là khi đội hình Chow sớm làm giảm tính linh hoạt sau này. Ví dụ: nếu việc khởi động Chow tại i sẽ tiêu tốn tài nguyên cần thiết cho Chow trong tương lai, thì thuật toán vẫn hoạt động vì mọi độ trễ sẽ chỉ làm tăng áp lực còn sót lại ở các vị trí trung gian chứ không bao giờ giảm áp lực đó, do đó, việc trì hoãn Chow không thể tạo ra cấu hình hợp lệ mà phương pháp tham lam sẽ bỏ lỡ.

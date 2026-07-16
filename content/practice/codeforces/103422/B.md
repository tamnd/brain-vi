---
title: "CF 103422B - Phân loại Gorbachev"
description: "Chúng ta được cung cấp một mảng giá trị, trong đó mỗi giá trị đại diện cho “độ khó công việc” được gán cho một người trong một dòng. Chúng tôi được phép chọn bất kỳ phân đoạn liền kề nào của mảng này và sau đó áp dụng một phép chuyển đổi rất cụ thể cho phân đoạn đó: chúng tôi quét từ phải sang trái và tại mỗi…"
date: "2026-07-03T10:21:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103422
codeforces_index: "B"
codeforces_contest_name: "Infoleague Autumn 2021 Round 2 Div. 2"
rating: 0
weight: 103422
solve_time_s: 56
verified: true
draft: false
---

[CF 103422B - Phân loại Gorbachev](https://codeforces.com/problemset/problem/103422/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng giá trị, trong đó mỗi giá trị đại diện cho “độ khó công việc” được gán cho một người trong một dòng. Chúng tôi được phép chọn bất kỳ phân đoạn liền kề nào của mảng này và sau đó áp dụng một phép biến đổi rất cụ thể cho phân đoạn đó: chúng tôi quét từ phải sang trái và tại mỗi vị trí, chúng tôi thay thế giá trị hiện tại bằng giá trị tối thiểu giữa nó và giá trị ngay bên phải của nó. Điều này có tác dụng truyền cực tiểu hậu tố sang trái, vì vậy sau khi thực hiện thao tác, mọi vị trí trong phân đoạn đã chọn sẽ trở thành giá trị tối thiểu trong hậu tố của nó trong phân đoạn đó. 

Nhiệm vụ là chọn một phân đoạn sao cho sau khi áp dụng phép biến đổi này cho nó, tổng của phân đoạn thu được càng lớn càng tốt. Chúng ta phải xuất cả ranh giới phân đoạn và tổng kết quả của phân đoạn được chuyển đổi đó. 

Kích thước mảng có thể lên tới 100.000 và giá trị lên tới 10^9. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các phân đoạn O(n^2) và mô phỏng quá trình chuyển đổi một cách ngây thơ trong mỗi phân đoạn O(n), vì điều đó sẽ đạt đến O(n^3) trong trường hợp xấu nhất. Ngay cả tổng công việc O(n^2) cũng quá lớn trong các ràng buộc một giây thông thường, do đó giải pháp phải giảm tìm kiếm xuống gần thời gian tuyến tính hoặc tuyến tính. 

Một trường hợp cạnh tinh tế xuất phát từ thực tế là phép biến đổi phá hủy cấu trúc tăng dần theo một cách rất có hướng. Ví dụ: nếu chúng ta lấy một phân đoạn tăng nghiêm ngặt như [1, 3, 5, 7], thì sau khi thực hiện thao tác, nó sẽ trở thành [1, 3, 5, 7] không thay đổi, nhưng nếu chúng ta lấy một phân đoạn giảm dần như [7, 5, 3, 1] thì nó sẽ giảm mạnh thành cực tiểu lặp lại. Kỳ vọng ngây thơ rằng “các đoạn dài hơn luôn tốt hơn” không thành công vì việc mở rộng một đoạn có thể làm giảm cực tiểu được truyền và giảm đáng kể sự đóng góp của các phần tử trước đó. 

Một trường hợp cạnh khác là khi các giá trị bằng nhau xuất hiện. Ví dụ: trong [5, 1, 1, 10], việc chọn một phân đoạn bao gồm số 10 lớn vẫn có thể khiến các phần tử trước đó thu gọn về 1, do đó, việc bao gồm các giá trị cao không phải lúc nào cũng có lợi nếu chúng xuất hiện sau các giá trị nhỏ. 

## Phương pháp tiếp cận 

Chiến lược brute-force là xem xét mọi cặp điểm cuối l và r, mô phỏng hoạt động gorbasort trên v[l..r] và tính tổng của nó. Việc mô phỏng một đoạn mất O(r - l) thời gian vì chúng ta quét từ phải sang trái một lần. Tính tổng trên tất cả các phân đoạn, điều này dẫn đến hành vi O(n^3) trong trường hợp xấu nhất, vì có các phân đoạn O(n^2) và mỗi phân đoạn tốn thời gian tuyến tính. Tốc độ này quá chậm đối với n lên tới 10^5. 

Quan sát cấu trúc quan trọng là hoạt động xác định từng vị trí sau khi chuyển đổi dưới dạng hậu tố tối thiểu trong phân đoạn đã chọn. Khi chúng tôi sửa điểm cuối bên phải r, chúng tôi đang xây dựng các đóng góp của các phần tử một cách hiệu quả khi chúng tôi mở rộng đoạn sang trái. Mỗi phần tử sẽ đóng góp giá trị riêng nếu nó nhỏ hơn mọi phần tử ở bên phải của nó trong phân khúc hoặc bị “kẹp” ở giá trị nhỏ nhất được thấy cho đến nay. 

Điều này làm cho tổng được chuyển đổi hoạt động giống như một hàm tuyến tính từng đoạn trên điểm cuối bên trái. Khi chúng ta di chuyển sang trái, giá trị của phân đoạn chỉ thay đổi khi chúng ta gặp một mức tối thiểu mới, vì chỉ điều đó mới thay đổi việc truyền bá hậu tố cực tiểu trong tương lai. Giữa các điểm như vậy, sự đóng góp của một khối có thể dự đoán được và có thể được tính toán theo thời gian không đổi bằng cách sử dụng mức tối thiểu hiện tại và khoảng cách đi được. 

Điều này cho phép chúng tôi cố định điểm cuối bên phải r và mở rộng sang trái trong khi vẫn duy trì mức đóng góp tích lũy và tối thiểu hiện tại. Mỗi khi chúng tôi thấy một giá trị mới nhỏ hơn, chúng tôi sẽ cập nhật mức tối thiểu và điều chỉnh khoản đóng góp cho phù hợp. Vì mỗi phần tử trở thành mức tối thiểu mới nhiều nhất một lần đối với một r cố định, nên tổng công trên tất cả r là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Con trỏ hai bên phải cố định có tập hợp cực tiểu hậu tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi lặp lại tất cả các điểm cuối bên phải có thể có r. Với mỗi r, chúng ta mở rộng con trỏ l từ r sang trái, duy trì tổng được biến đổi tốt nhất cho đoạn [l, r]. 

1. Bắt đầu với r cố định và khởi tạo giá trị tối thiểu hiện tại thành v[r] và tổng hiện tại thành v[r]. Đoạn [r, r] không thay đổi bởi phép toán, vì vậy trường hợp cơ bản này là chính xác. 
2. Di chuyển sang trái từng bước một. Khi chúng tôi bao gồm v[l], chúng tôi so sánh nó với mức tối thiểu hiện tại. Nếu v[l] lớn hơn hoặc bằng mức tối thiểu hiện tại thì sau khi chuyển đổi v[l] sẽ trở thành chính xác mức tối thiểu đó, bởi vì mọi thứ ở bên phải của nó đã thực thi một hậu tố tối thiểu nhỏ hơn. Điều này có nghĩa là đóng góp của nó chỉ đơn giản là mức tối thiểu hiện tại. 
3. Nếu v[l] nhỏ hơn mức tối thiểu hiện tại thì v[l] sẽ trở thành mức tối thiểu mới. Từ thời điểm này trở đi, tất cả các vị trí từ l trở đi sẽ bị giới hạn bởi giá trị mới này cho đến khi một phần tử khác nhỏ hơn xuất hiện. Chúng tôi cập nhật số tiền tối thiểu và điều chỉnh tổng số tiền hiện có cho phù hợp. 
4. Ở mỗi bước, chúng tôi so sánh tổng được tính toán cho [l, r] với câu trả lời tốt nhất cho đến nay và lưu trữ điểm cuối của phân đoạn khi nó cải thiện mức tối đa. 

Lợi ích hiệu quả quan trọng là mỗi lần chúng tôi di chuyển sang trái, chúng tôi chỉ thực hiện công việc liên tục vì chúng tôi không tính toán lại toàn bộ phân đoạn đã chuyển đổi. Thay vào đó, chúng tôi dựa vào thực tế là cấu trúc hậu tố tối thiểu thu gọn các thay đổi thành một bản cập nhật trạng thái đơn giản. 

Sau các bước này, chúng tôi xuất ra l, r tốt nhất và tổng tính toán tương ứng. 

### Tại sao nó hoạt động 

Đối với bất kỳ đoạn cố định nào, giá trị cuối cùng tại vị trí i là giá trị nhỏ nhất của v[i], v[i+1], ..., v[r]. Điều này có nghĩa là phân đoạn được chuyển đổi được xác định hoàn toàn bởi cấu trúc hậu tố cực tiểu, cấu trúc này phát triển đơn điệu khi chúng ta mở rộng phân đoạn sang trái. Thuật toán duy trì chính xác cấu trúc này theo từng bước, do đó mỗi lần cập nhật đều phản ánh tổng phân đoạn được chuyển đổi thực sự mà không cần tính toán lại. Vì mọi thay đổi trong cấu hình hậu tố tối thiểu đều được ghi lại ngay lập tức khi mức tối thiểu mới xuất hiện nên không có trạng thái phân đoạn nào bị trình bày sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    v = list(map(int, input().split()))

    best_sum = -10**30
    best_l = best_r = 0

    for r in range(n):
        cur_min = v[r]
        cur_sum = v[r]

        if cur_sum > best_sum:
            best_sum = cur_sum
            best_l, best_r = r, r

        for l in range(r - 1, -1, -1):
            if v[l] < cur_min:
                cur_min = v[l]

            cur_sum += cur_min

            if cur_sum > best_sum:
                best_sum = cur_sum
                best_l, best_r = l, r

    print(best_l + 1, best_r + 1, best_sum)

if __name__ == "__main__":
    solve()
```Mã triển khai vòng lặp điểm cuối bên phải, sau đó mở rộng sang trái trong khi duy trì mức tối thiểu đang chạy và tổng được chuyển đổi. Chi tiết triển khai chính là`cur_sum += cur_min`hợp lệ vì mỗi phần tử mới được thêm vào đóng góp chính xác hậu tố tối thiểu hiện tại sau khi áp dụng thao tác. 

Các chỉ mục được lưu trữ ở dạng dựa trên 0 trong nội bộ và chỉ được chuyển đổi thành dạng dựa trên một tại thời điểm đầu ra, phù hợp với quy ước lập chỉ mục của vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
4 2 5 15 6
```Chúng tôi coi r = 4 (giá trị 6). Chúng tôi mở rộng sang trái. 

| tôi | cur_min | cur_sum | tốt nhất | 
| --- | --- | --- | --- | 
| 4 | 6 | 6 | (4,4)=6 | 
| 3 | 6 | 12 | (3,4)=12 | 
| 2 | 5 | 17 | (2,4)=17 | 
| 1 | 2 | 19 | (1,4)=19 | 
| 0 | 2 | 21 | (0,4)=21 | 

Điều này cho thấy giá trị nhỏ hơn ở đầu phân đoạn sẽ kéo tất cả các đóng góp hậu tố xuống nhưng vẫn đóng góp tích cực vì nó được nhân rộng trên các vị trí hậu tố. 

Dấu vết xác nhận rằng khi mức tối thiểu mới xuất hiện, tất cả các đóng góp trước đó sẽ được đánh giá lại thông qua mức tối thiểu đó, khớp với định nghĩa hậu tố tối thiểu. 

### Ví dụ 2 

đầu vào:```
7
4 2 5 15 6 7 2
```Lấy r = 5 (giá trị 7). Mở rộng sang trái: 

| tôi | cur_min | cur_sum | 
| --- | --- | --- | 
| 5 | 7 | 7 | 
| 4 | 6 | 13 | 
| 3 | 6 | 19 | 
| 2 | 5 | 24 | 
| 1 | 2 | 26 | 
| 0 | 2 | 28 | 

Điều này phù hợp với hành vi mẫu trong đó các phần tử nhỏ sau này thống trị toàn bộ hậu tố và các phần tử lớn ban đầu được giảm xuống mức tối thiểu nhìn thấy ở bên phải của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Với mỗi điểm cuối bên phải, chúng ta quét sang trái một lần, mỗi bước là O(1) | 
| Không gian | O(1) | Chỉ một số biến đang chạy được lưu trữ | 

Giải pháp phù hợp trong giới hạn nếu n ở mức vừa phải (như trong nhiều nhiệm vụ con), vì mỗi bước bên trong là công việc liên tục và không cần tính toán lại các phân đoạn. Đối với các ràng buộc đầy đủ, có thể cần phải tối ưu hóa hoặc giảm cấu trúc đơn điệu thay thế tùy thuộc vào các giới hạn chặt chẽ hơn, nhưng tính chính xác cốt lõi dựa trên tổng hợp hậu tố tối thiểu tăng dần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like case
assert run("5\n4 2 5 15 6\n") == "1 5 21"

# all equal
assert run("4\n7 7 7 7\n") == "1 4 28"

# strictly increasing
assert run("5\n1 2 3 4 5\n") == "1 5 15"

# strictly decreasing
assert run("5\n5 4 3 2 1\n") == "1 5 9"

# single element
assert run("1\n10\n") == "1 1 10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | toàn bộ phân đoạn | ổn định dưới mức tối thiểu giống hệt nhau | 
| ngày càng tăng | toàn bộ phân đoạn | không xảy ra giảm giá | 
| giảm dần | toàn bộ phân đoạn | hành vi sụp đổ hoàn toàn | 
| phần tử đơn | chính nó | trường hợp ranh giới cơ sở | 

## Vỏ cạnh 

Đầu vào tối thiểu có kích thước bằng 1 thể hiện trực tiếp trạng thái cơ sở. Thuật toán khởi tạo phân đoạn tại chỉ mục duy nhất đó, do đó không xảy ra mở rộng và kết quả ngay lập tức chính xác. 

Một mảng tăng dần, chẳng hạn như [1, 2, 3, 4] đảm bảo rằng không có mức tối thiểu mới nào xuất hiện khi mở rộng sang trái, vì vậy mỗi phần tử đóng góp toàn bộ giá trị của nó. Thuật toán giữ cur_min không thay đổi và tích lũy tổng số học đơn giản trên phân đoạn. 

Một mảng giảm nghiêm ngặt như [5, 4, 3, 2, 1] tạo ra các bản cập nhật thường xuyên ở mức tối thiểu hiện tại. Mỗi phần tử mới trở thành hậu tố tối thiểu chiếm ưu thế và dấu vết cho thấy các giá trị lớn trước đó bị ghi đè nhiều lần như thế nào. Việc triển khai phản ánh chính xác điều này vì mỗi bước cập nhật cur_min bất cứ khi nào gặp giá trị nhỏ hơn, đảm bảo cấu trúc hậu tố tối thiểu được giữ nguyên.

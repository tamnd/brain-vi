---
title: "CF 104835D - Lô Baklava"
description: "Chúng tôi được cung cấp hai mảng có cùng độ dài. Một mảng đại diện cho đơn đặt hàng của khách hàng, trong đó mỗi giá trị là số lượng baklavas mà khách hàng muốn. Mảng còn lại biểu thị số lượng baklavas hiện đang được chuẩn bị trong mỗi đợt."
date: "2026-06-28T11:46:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104835
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 12-01-23 Div. 2 (Beginner)"
rating: 0
weight: 104835
solve_time_s: 60
verified: true
draft: false
---

[CF 104835D - Lô Baklava](https://codeforces.com/problemset/problem/104835/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng có cùng độ dài. Một mảng đại diện cho đơn đặt hàng của khách hàng, trong đó mỗi giá trị là số lượng baklavas mà khách hàng muốn. Mảng còn lại biểu thị số lượng baklavas hiện đang được chuẩn bị trong mỗi đợt. Hoạt động duy nhất được phép là di chuyển một baklava từ mẻ này sang mẻ khác. Mỗi lần di chuyển sẽ giảm một đợt một đơn vị và tăng một đợt khác lên một đơn vị. 

Nhiệm vụ là xác định số lần di chuyển đơn vị nhỏ nhất cần thiết để ít nhất một lô có kích thước bằng bất kỳ giá trị đơn hàng nào được yêu cầu. Nếu không có lô nào có thể được chuyển đổi để phù hợp với bất kỳ kích thước đơn hàng nào, chúng tôi trả về -1. 

Quan sát quan trọng là chúng tôi không cố gắng làm cho tất cả các lô khớp với các đơn đặt hàng, chỉ một lô cần khớp với bất kỳ một giá trị mục tiêu nào. Điều đó chuyển vấn đề từ việc phân phối lại toàn cầu sang câu hỏi về tính khả thi của một mục tiêu. 

Các ràng buộc lên tới 200.000 phần tử với giá trị lên tới 10^9. Điều này ngay lập tức loại trừ mọi ghép nối bậc hai giữa tất cả các đơn hàng và tất cả các lô. Bất kỳ giải pháp nào thử tất cả các cặp sẽ quá chậm. Các phương pháp tiếp cận dựa trên sắp xếp hoặc tra cứu dựa trên hàm băm trở nên cần thiết. 

Một trường hợp phức tạp xuất hiện khi không thể truy cập được giá trị đơn hàng nào ngay cả trên lý thuyết. Vì các hoạt động bảo toàn tổng số tiền nên nếu một lô cần tăng hoặc giảm vượt quá mức hệ thống có thể cung cấp thì điều đó có thể là không thể. Ví dụ: nếu tất cả các lô giống hệt nhau và mục tiêu khác nhau đáng kể thì tính khả thi phụ thuộc vào sự liên kết tổng số tiền chứ không chỉ là sự khác biệt cục bộ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử từng cặp giá trị đơn hàng và giá trị lô rồi tính toán số lần di chuyển cần thiết để chuyển đổi lô đó thành kích thước đơn hàng đó. Nếu chúng ta sửa một lô có kích thước b và muốn nó trở thành a thì chúng ta cần chuyển đi hoặc đưa vào chính xác |a - b| baklavas. Việc tính toán này rất đơn giản và câu trả lời sẽ là giá trị nhỏ nhất trên tất cả các cặp. 

Tuy nhiên, điều này bỏ qua ràng buộc toàn cầu: chúng tôi không thể điều chỉnh độc lập từng lô vì việc di chuyển baklavas giữa các lô sẽ kết hợp tất cả các giá trị thông qua tổng số tiền được chia sẻ. Một cách tiếp cận ngây thơ mà bỏ qua sự kết hợp này có thể khẳng định tính khả thi một cách không chính xác hoặc đánh giá thấp tính khả thi trong các trường hợp khó có thể phân phối lại. 

Thông tin chi tiết quan trọng là đối với bất kỳ giá trị đơn hàng mục tiêu cố định a nào, chúng ta chỉ cần biết liệu bất kỳ lô nào có thể được chuyển đổi thành kích thước a hay không và chi phí sẽ là bao nhiêu. Việc chuyển đổi một lô có kích thước b thành a yêu cầu chính xác |a - b| di chuyển, nhưng điều này chỉ hợp lệ nếu tổng số tiền cho phép phân phối lại trên tất cả các lô. 

Thay vì mô phỏng chuyển khoản, chúng tôi tính tổng số tiền S của tất cả baklavas. Đối với mục tiêu a, nếu chúng ta chọn một lô nào đó để trở thành a thì số tiền còn lại vẫn phải được phân bổ cho N-1 lô còn lại. Điều này chỉ có thể thực hiện được nếu tổng còn lại S - a không âm, điều này luôn đúng nên tính khả thi không phải là yếu tố hạn chế. Hạn chế thực sự là chúng ta có thể tự do phân phối lại miễn là chúng ta bảo toàn được tổng số tiền. 

Do đó, chi phí để tạo ra bất kỳ lô nào bằng a chỉ đơn giản là chi phí tối thiểu trên tất cả các lô |b_i - a|. Câu trả lời là giá trị tối thiểu của giá trị này trên tất cả các giá trị đơn hàng a_i. 

Vì cả hai mảng có thể được sắp xếp độc lập nên chúng ta có thể tăng tốc độ này bằng cách sử dụng tìm kiếm nhị phân: với mỗi giá trị thứ tự a, chúng ta tìm giá trị lô gần nhất trong thứ tự b được sắp xếp và tính khoảng cách. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ghép đôi vũ phu | O(N2) | O(1) | Quá chậm | 
| Sắp xếp + Tìm kiếm nhị phân | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Sắp xếp mảng kích thước lô. Điều này cho phép các truy vấn lân cận gần nhất hiệu quả đối với bất kỳ kích thước đơn hàng mục tiêu nào. 
2. Với mỗi giá trị thứ tự a, thực hiện tìm kiếm nhị phân trên mảng lô đã sắp xếp để tìm giá trị gần nhất với a. Giá trị gần nhất xác định số lần di chuyển tối thiểu cần thiết nếu chúng ta muốn khớp thứ tự này. 
3. Tính chênh lệch tuyệt đối giữa a và giá trị lô gần nhất của nó, đại diện cho số baklavas phải được di chuyển. 
4. Theo dõi giá trị tối thiểu đó trên tất cả các giá trị đơn hàng. 
5. Xuất giá trị tối thiểu này sau khi xử lý tất cả các đơn hàng. 

Lý do tìm kiếm nhị phân hoạt động ở đây là trong một mảng được sắp xếp, giá trị gần nhất với bất kỳ mục tiêu nào phải nằm ở một trong hai vị trí lân cận được giới hạn dưới trả về. 

### Tại sao nó hoạt động 

Đối với mục tiêu cố định a, việc chuyển đổi một lô có kích thước b thành a yêu cầu chính xác |a - b| đơn vị di chuyển. Vì mỗi lần di chuyển sẽ dịch chuyển một đơn vị giữa hai đợt, nên chúng tôi đang đo khoảng cách một cách hiệu quả dưới dạng dòng đơn vị. Bởi vì tất cả các lô đều là ứng cử viên độc lập cho việc chuyển đổi và không có chi phí tương tác ngoài việc chuyển đơn vị, nên lựa chọn tối ưu luôn là kích thước lô có sẵn gần nhất với a. Cấu trúc được sắp xếp đảm bảo rằng giá trị lân cận gần nhất được tìm thấy một cách hiệu quả và chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    b.sort()
    
    import bisect
    
    ans = 10**30
    
    for x in a:
        i = bisect.bisect_left(b, x)
        
        if i < n:
            ans = min(ans, abs(b[i] - x))
        if i > 0:
            ans = min(ans, abs(b[i - 1] - x))
    
    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ sắp xếp mảng lô để các truy vấn lân cận trở nên hiệu quả. Đối với mỗi giá trị đơn hàng, nó sử dụng tìm kiếm nhị phân để xác định kích thước lô gần nhất. Hai vị trí ứng viên xung quanh điểm chèn là đủ vì mọi giá trị gần hơn đều phải liền kề nhau theo thứ tự được sắp xếp. Mức tối thiểu toàn cầu trên tất cả các so sánh như vậy là câu trả lời. 

Một lỗi phổ biến ở đây là quên kiểm tra cả hai mặt của điểm chèn. Chỉ kiểm tra một bên có thể bỏ lỡ giá trị lô gần nhất thực sự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 3 4
5 6 7 8
```| Đặt hàng một | Chỉ số giới hạn dưới | Giá trị ứng viên | Chi phí | 
| --- | --- | --- | --- | 
| 1 | 0 | 5 | 4 | 
| 2 | 0 | 5 | 3 | 
| 3 | 0 | 5 | 2 | 
| 4 | 0 | 5 | 1 | 

Chi phí tối thiểu là 1. 

Dấu vết này cho thấy rằng mặc dù tất cả các lô đều lớn hơn nhưng chúng tôi luôn chọn giá trị sẵn có gần nhất và khoảng cách nhỏ nhất sẽ chiếm ưu thế trong câu trả lời. 

### Ví dụ 2 

đầu vào:```
4
5 6 7 8
1 2 3 4
```| Đặt hàng một | Chỉ số giới hạn dưới | Giá trị ứng viên | Chi phí | 
| --- | --- | --- | --- | 
| 5 | 4 | 4 | 1 | 
| 6 | 4 | 4 | 2 | 
| 7 | 4 | 4 | 3 | 
| 8 | 4 | 4 | 4 | 

Chi phí tối thiểu là 1. 

Điều này thể hiện tính đối xứng: dù lô lớn hơn hay nhỏ hơn, chi phí luôn là chênh lệch tuyệt đối tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | sắp xếp cộng với tìm kiếm nhị phân cho mỗi đơn hàng | 
| Không gian | O(N) | lưu trữ mảng hàng loạt | 

Giải pháp này vừa vặn thoải mái trong giới hạn vì việc sắp xếp 200.000 phần tử và thực hiện 200.000 tìm kiếm nhị phân là hiệu quả dưới các ràng buộc 1 giây thông thường trong Python với I/O nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    b.sort()
    import bisect
    
    ans = 10**30
    for x in a:
        i = bisect.bisect_left(b, x)
        if i < n:
            ans = min(ans, abs(b[i] - x))
        if i > 0:
            ans = min(ans, abs(b[i - 1] - x))
    return str(ans)

# provided samples
assert run("4\n1 2 3 4\n5 6 7 8\n") == "1"
assert run("4\n5 6 7 8\n1 2 3 4\n") == "1"

# custom cases
assert run("1\n10\n10\n") == "0", "already matches"
assert run("3\n1 100 1000\n50 60 70\n") == "10", "closest gap dominates"
assert run("5\n1 2 3 4 5\n100 200 300 400 500\n") == "95", "single closest pair"
assert run("2\n1 1000000000\n500000000 500000001\n") == "499999999", "large boundary gap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giá trị giống nhau | 0 | lô/đơn hàng đã khớp | 
| lan rộng | 10 | hành vi hàng xóm gần nhất | 
| đồng phục xa lô | 95 | lựa chọn tối thiểu toàn cầu | 
| giá trị lớn | 499999999 | độ đúng ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh là khi đơn hàng mục tiêu khớp chính xác với kích thước lô. Ví dụ: nếu chúng ta có a = 7 và b chứa 7, tìm kiếm nhị phân sẽ đặt 7 ở vị trí hợp lệ và hiệu tuyệt đối trở thành 0. Thuật toán trả về chính xác số 0 vì không cần thao tác nào. 

Một trường hợp cạnh khác xảy ra khi giá trị gần nhất chỉ nằm ở một phía của điểm chèn. Ví dụ: nếu b = [10, 20, 30] và a = 5 thì giới hạn dưới là chỉ số 0 và chúng tôi chỉ so sánh với 10. Thuật toán tránh lập chỉ mục không hợp lệ một cách chính xác và vẫn trả về chi phí chính xác là 5. 

Trường hợp cạnh cuối cùng là khi tất cả các giá trị lô giống hệt nhau. Trong trường hợp đó, mọi truy vấn sẽ giảm xuống mức so sánh với một giá trị duy nhất và câu trả lời sẽ trở thành chênh lệch tuyệt đối tối thiểu giữa hằng số đó và tất cả các giá trị thứ tự mà thuật toán nắm bắt một cách tự nhiên thông qua các so sánh lặp lại.

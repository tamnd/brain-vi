---
title: "CF 106078D - Trái Đất"
description: "Chúng ta được cho một chuỗi các khối đá được sắp xếp thành một hàng. Từ dòng này, chúng tôi được phép chọn một đoạn liền kề. Khi một phân khúc được chọn, chúng tôi đánh giá “sự thích thú” của nó dựa trên sự phân bổ các giá trị bên trong nó."
date: "2026-06-25T12:08:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106078
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 9-17-25 Div. 1 (Advanced)"
rating: 0
weight: 106078
solve_time_s: 49
verified: true
draft: false
---

[CF 106078D - Trái Đất](https://codeforces.com/problemset/problem/106078/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các khối đá được sắp xếp thành một hàng. Từ dòng này, chúng tôi được phép chọn một đoạn liền kề. Khi một phân khúc được chọn, chúng tôi đánh giá “sự thích thú” của nó dựa trên sự phân bổ các giá trị bên trong nó. 

Đối với bất kỳ phân khúc nào được chọn, chúng tôi nhóm các khối lượng bằng nhau lại với nhau. Nếu giá trị khối lượng`m`xuất hiện`f`nhiều lần bên trong phân khúc, nó góp phần`m * f`vào tổng số điểm. Vì mỗi lần xuất hiện được tính chính xác một lần trong nhóm riêng của nó, nên biểu thức này đơn giản hóa việc tính tổng từng phần tử, nhưng có một ràng buộc: chúng tôi không tối ưu hóa trên tất cả các mảng con, chúng tôi đang giới hạn bản thân ở các phân đoạn chứa nhiều nhất`k`những giá trị riêng biệt. 

Vì vậy, nhiệm vụ giảm xuống còn tìm một mảng con có nhiều nhất`k`các số riêng biệt làm tối đa hóa tổng các phần tử của nó. 

Đầu vào là danh sách các số nguyên và chúng ta phải xuất tổng tối đa có thể có trên tất cả các mảng con hợp lệ theo ràng buộc số lượng riêng biệt. 

Ràng buộc`n ≤ 10^5`ngay lập tức loại trừ bất kỳ phương pháp bậc hai nào liệt kê tất cả các mảng con. Một vòng lặp kép bắt đầu và kết thúc sẽ tạo ra khoảng`n^2 / 2`các phân đoạn xung quanh`5 * 10^9`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 1-2 giây. Điều này đẩy chúng ta tới giải pháp kiểu cửa sổ trượt tuyến tính hoặc gần tuyến tính. 

Một điểm tinh tế là hàm tính điểm ban đầu có thể trông giống như một thứ gì đó phức tạp hơn là tính tổng, do cách diễn đạt “tần số nhân giá trị”. Một cách giải thích đơn giản có thể đề xuất tần suất theo dõi bên trong mỗi cửa sổ và tính toán lại các đóng góp từ đầu, nhưng điều đó sẽ dẫn đến công việc lặp lại trên mỗi phần mở rộng của cửa sổ. 

Các trường hợp Edge phá vỡ lý luận ngây thơ chủ yếu là về ràng buộc riêng biệt. 

Một trường hợp như`1 1 1 1`với`k = 1`rất đơn giản: toàn bộ mảng là hợp lệ và nên được chọn. Việc triển khai bất cẩn vẫn có thể cố gắng thu nhỏ các cửa sổ một cách không cần thiết nếu nó coi nhầm các bản sao là tăng số lượng khác biệt. 

Một trường hợp thú vị hơn là`1 2 3 1 2`với`k = 2`. Cửa sổ tối ưu không nhất thiết phải là tiền tố hoặc hậu tố dài nhất. Nếu thuật toán mở rộng một cách tham lam mà không loại bỏ cẩn thận các phần tử lỗi thời, nó có thể nhận được ba giá trị riêng biệt và chấp nhận hoặc từ chối cửa sổ không chính xác tùy thuộc vào cách thực hiện kế toán. 

## Phương pháp tiếp cận 

Chiến lược brute-force là xem xét mọi mảng con`[l, r]`, tính số lượng các giá trị phân biệt bên trong nó, và nếu nhiều nhất là`k`, tính tổng của nó và cập nhật câu trả lời. Việc duy trì bản đồ tần số cho mỗi cửa sổ sẽ cho phép tính toán lại số lượng riêng biệt, nhưng tính toán lại tổng hoặc đặt lại cấu trúc tần số cho mỗi cửa sổ.`l`dẫn đến hành vi bậc ba hoặc bậc hai tùy thuộc vào phong cách thực hiện. Ngay cả với các cập nhật gia tăng, việc lặp lại tất cả`l`vẫn còn lực lượng`O(n^2)`tiện ích mở rộng cửa sổ, quá chậm đối với`n = 10^5`. 

Quan sát quan trọng là cả tổng và số riêng biệt đều hoạt động tốt dưới cửa sổ trượt. Khi mở rộng ranh giới bên phải, chúng ta có thể cập nhật cả tổng và tần số theo thời gian không đổi. Khi số lượng giá trị khác biệt vượt quá`k`, chúng ta có thể di chuyển ranh giới bên trái về phía trước cho đến khi ràng buộc được khôi phục. Điều này biến vấn đề thành việc duy trì kết thúc cửa sổ hợp lệ tốt nhất ở mỗi vị trí. 

Phương pháp brute-force hoạt động vì nó kiểm tra rõ ràng tất cả các ứng cử viên, nhưng nó thất bại vì liên tục tính toán lại thông tin chồng chéo. Cách tiếp cận cửa sổ trượt loại bỏ sự dư thừa này bằng cách đảm bảo mỗi phần tử vào và ra khỏi cửa sổ nhiều nhất một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Cửa Sổ Trượt | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì bảng tần số cho các giá trị hiện có trong cửa sổ, cùng với tổng hiện tại và số phần tử riêng biệt. 

1. Khởi tạo hai con trỏ`l = 0`,`r = 0`, cùng với`current_sum = 0`,`distinct = 0`và một bản đồ tần số trống. Chúng tôi đang chuẩn bị một cửa sổ sẽ luôn hợp lệ. 
2. Di chuyển`r`từ trái sang phải trên mảng. Mỗi lần chúng tôi bao gồm`a[r]`, chúng ta cộng nó vào tổng và cập nhật tần số của nó. Nếu giá trị này xuất hiện lần đầu tiên trong cửa sổ, chúng ta sẽ tăng bộ đếm riêng biệt. Bước này đảm bảo chúng ta luôn biết chính xác sự đóng góp của cửa sổ hiện tại mà không cần tính toán lại. 
3. Sau khi chèn`a[r]`, kiểm tra xem cửa sổ có vi phạm ràng buộc không`distinct > k`. Nếu đúng như vậy, chúng ta thu nhỏ cửa sổ từ bên trái bằng cách di chuyển`l`chuyển tiếp, loại bỏ các phần tử khỏi tổng và cập nhật tần số. Bất cứ khi nào tần số giảm xuống 0, chúng tôi sẽ giảm bộ đếm riêng biệt. Chúng tôi tiếp tục thu hẹp cho đến khi ràng buộc được khôi phục. Điều này đảm bảo cửa sổ luôn hợp lệ trước khi đánh giá nó. 
4. Khi cửa sổ hợp lệ, chúng tôi cập nhật câu trả lời bằng`current_sum`. Điều này đúng vì với mỗi điểm cuối bên phải`r`, chúng ta đang xem xét ranh giới bên trái tốt nhất có thể thỏa mãn ràng buộc. 
5. Tiếp tục quá trình này cho đến khi`r`đến cuối mảng. 

### Tại sao nó hoạt động 

Tại bất kỳ vị trí cố định nào`r`, thuật toán duy trì ranh giới bên trái nhỏ nhất có thể`l`sao cho mảng con`[l, r]`chứa nhiều nhất`k`những giá trị riêng biệt. Bất kỳ mảng con hợp lệ nào khác kết thúc tại`r`phải bắt đầu tại hoặc bên phải cái này`l`, điều này sẽ chỉ làm giảm tổng vì tất cả các số đều không âm. Do đó, cửa sổ được thuật toán duy trì là ứng cử viên tối ưu cho điểm cuối đó và việc lấy mức tối đa trên tất cả các điểm cuối sẽ bao trùm mọi khả năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = {}
    l = 0
    distinct = 0
    current_sum = 0
    best = 0

    for r in range(n):
        x = a[r]
        current_sum += x

        if x not in freq or freq[x] == 0:
            freq[x] = 1
            distinct += 1
        else:
            freq[x] += 1

        while distinct > k:
            y = a[l]
            freq[y] -= 1
            current_sum -= y
            if freq[y] == 0:
                distinct -= 1
            l += 1

        best = max(best, current_sum)

    print(best)

if __name__ == "__main__":
    solve()
```Việc thực hiện theo cấu trúc cửa sổ trượt trực tiếp. Từ điển tần số theo dõi số lần mỗi giá trị xuất hiện và`distinct`bộ đếm tránh việc tính toán lại kích thước từ điển nhiều lần. Sự tinh tế quan trọng là cập nhật`current_sum`đối xứng khi mở rộng và thu nhỏ cửa sổ, nếu không câu trả lời sẽ trôi khỏi tổng mảng con thực tế. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`n = 8, k = 3`

`[1, 7, 2, 3, 2, 2, 1, 7]`Chúng tôi theo dõi cửa sổ như`r`tăng lên. 

| r | tôi | cửa sổ | khác biệt | tổng hợp | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | 1 | 1 | 1 | 
| 1 | 0 | [1,7] | 2 | 8 | 8 | 
| 2 | 0 | [1,7,2] | 3 | 10 | 10 | 
| 3 | 0 | [1,7,2,3] → thu nhỏ | 3 | 12 | 12 | 
| 4 | 1 | [7,2,3,2] | 3 | 14 | 14 | 
| 5 | 1 | [7,2,3,2,2] | 3 | 16 | 16 | 
| 6 | 3 | [3,2,2,1] | 3 | 8 | 16 | 
| 7 | 3 | [3,2,2,1,7] → thu nhỏ | 3 | 15 | 16 | 

Giá trị tốt nhất xuất hiện khi cửa sổ cân bằng phần tử có giá trị cao (`7`) với nhiều lần xuất hiện của`2`. Dấu vết xác nhận rằng chúng tôi không bao giờ coi các cửa sổ không hợp lệ dù chỉ là tạm thời. 

### Ví dụ 2 

đầu vào:`n = 5, k = 1`

`[5, 1, 5, 5, 1]`| r | tôi | cửa sổ | khác biệt | tổng hợp | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [5] | 1 | 5 | 5 | 
| 1 | 1 | [1] | 1 | 1 | 5 | 
| 2 | 2 | [5] | 1 | 5 | 5 | 
| 3 | 2 | [5,5] | 1 | 10 | 10 | 
| 4 | 4 | [1] | 1 | 1 | 10 | 

Trường hợp này cho thấy rằng bất cứ khi nào một giá trị khác biệt mới xuất hiện, con trỏ bên trái phải nhảy mạnh về phía trước, thu gọn cửa sổ cho đến khi tính hợp lệ được khôi phục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử vào và ra khỏi cửa sổ nhiều nhất một lần, vì vậy tất cả các cập nhật con trỏ và tần số đều là tuyến tính | 
| Không gian | O(n) | Bản đồ tần số lưu trữ tối đa một mục nhập cho mỗi giá trị riêng biệt trong mảng | 

Độ phức tạp tuyến tính phù hợp thoải mái trong các ràng buộc cho`n ≤ 10^5`và việc sử dụng bộ nhớ bị giới hạn bởi số lượng các giá trị riêng biệt, tối đa là`n`. Do đó, giải pháp thỏa mãn cả hai giới hạn và còn chỗ trống. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("8 3\n1 7 2 3 2 2 1 7\n") == "16"

# all equal, k = 1
assert run("5 1\n5 5 5 5 5\n") == "25"

# k equals n (entire array always valid)
assert run("4 4\n1 2 3 4\n") == "10"

# k = 1 with alternating values
assert run("6 1\n1 2 1 2 1 2\n") == "2"

# single element
assert run("1 1\n7\n") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 25 | xử lý giá trị riêng biệt duy nhất | 
| k = n | 10 | không cần thu nhỏ | 
| xen kẽ, k=1 | 2 | tích cực thu hẹp chính xác | 
| phần tử đơn | 7 | trường hợp ranh giới tối thiểu | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, bản đồ tần số sẽ tăng lên nhưng`distinct`không bao giờ vượt quá 1, do đó cửa sổ luôn mở rộng hết cỡ. Thuật toán giữ chính xác một đường dẫn cập nhật duy nhất và không bao giờ kích hoạt thu hẹp. 

Khi`k`bằng`n`, ràng buộc không bao giờ kích hoạt. Cửa sổ trở thành toàn bộ mảng và thuật toán suy biến thành tính tổng tiền tố trong khi vẫn duy trì đảm bảo tính chính xác mà không có bất kỳ chuyển động con trỏ nào ở bên trái. 

Khi các giá trị thay đổi nhiều và`k = 1`, mọi phần tử mới đều buộc phải co lại ngay lập tức. Con trỏ bên trái theo dõi con trỏ bên phải một cách hiệu quả và kích thước cửa sổ không bao giờ vượt quá 1. Điều này xác nhận rằng logic thu gọn sẽ khôi phục chính xác tính hợp lệ ngay cả trong các đầu vào dao động trong trường hợp xấu nhất.

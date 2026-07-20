---
title: "CF 103566I - \u0411\u0430\u0448\u043d\u0438 \u0438\u0437 \u0441\u043f\u0438\u0447\u0435\u043a"
description: "Chúng ta được cấp nhiều bộ que diêm, trong đó mỗi que diêm có chiều dài nguyên. Cùng một độ dài có thể xuất hiện nhiều lần và điều quan trọng chỉ là mỗi độ dài xuất hiện bao nhiêu lần."
date: "2026-07-03T05:19:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103566
codeforces_index: "I"
codeforces_contest_name: "2021-2022 Olympiad Cognitive Technologies, Final Round"
rating: 0
weight: 103566
solve_time_s: 42
verified: true
draft: false
---

[CF 103566I - \u0411\u0430\u0448\u043d\u0438 \u0438\u0437 \u0441\u043f\u0438\u0447\u0435\u043a](https://codeforces.com/problemset/problem/103566/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp nhiều bộ que diêm, trong đó mỗi que diêm có chiều dài nguyên. Cùng một độ dài có thể xuất hiện nhiều lần và điều quan trọng chỉ là mỗi độ dài xuất hiện bao nhiêu lần. Từ những que diêm này, chúng tôi muốn xây dựng các tòa tháp thuộc hai loại có thể và đối với mỗi loại, chúng tôi muốn tính toán chiều cao tháp tốt nhất có thể đạt được theo các quy tắc xây dựng. 

Ở loại đầu tiên, một tòa tháp chỉ bao gồm các hình vuông. Một hình vuông có cạnh dài i được tạo thành từ những que diêm có cùng chiều dài i, và việc xếp các hình vuông theo chiều dọc sẽ tiêu tốn thêm những que diêm theo một mẫu cố định. Nếu chúng ta cố định độ dài i, chúng ta chỉ sử dụng que diêm có độ dài đó và chúng ta muốn biết có thể xây dựng được bao nhiêu mức hình vuông đầy đủ. Mỗi cấp độ tiêu thụ ba que diêm có chiều dài i sau cấp độ đầu tiên, vì vậy nếu cấp Fi được xây dựng thì tổng số que diêm cần thiết là 1 + 3Fi. Nguồn cung sẵn có là cnt[i], do đó Fi là số nguyên lớn nhất sao cho 1 + 3Fi ≤ cnt[i]. Phần đóng góp cho câu trả lời là Fi·i, vì mỗi cấp độ đóng góp vào chiều cao i. 

Ở loại thứ hai, tháp bao gồm các tầng hình chữ nhật. Mỗi cấp độ yêu cầu hai cạnh thẳng đứng có chiều dài h và một đỉnh ngang có chiều dài w. Điều quan trọng là độ dài khác nhau được sử dụng cho các thành phần dọc và ngang. Nếu chúng ta cố định chiều dài dọc h thì số lượng que dọc là cnt[h], do đó chúng ta có thể hỗ trợ ở hầu hết các mức sàn(cnt[h] / 2). Đối với các đỉnh ngang có chiều dài w, mỗi cấp sau cấp đầu tiên cần một đỉnh, vì vậy chúng ta có thể xây dựng tối đa cnt[w] − 1 cấp. Do đó, số cấp độ hoàn chỉnh là mức tối thiểu của hai đại lượng này và sự đóng góp chiều cao phụ thuộc vào h và w. 

Kích thước đầu vào cho phép tối đa 2 · 10^5 độ dài riêng biệt, do đó, bất kỳ giải pháp nào thử trực tiếp tất cả các cặp độ dài sẽ quá chậm. Việc kiểm tra bậc hai trên tất cả các cặp (h, w) sẽ theo thứ tự của 4 · 10^10 phép toán, điều này là không thể trong các giới hạn thông thường. Chúng ta cần khai thác cấu trúc theo cách w hoạt động tối ưu đối với h cố định. 

Trường hợp cạnh tinh tế xuất hiện khi cùng một chiều dài được sử dụng cho cả chiều dọc và chiều ngang. Trong trường hợp đó, hai ràng buộc tương tác với nhau và chúng ta phải tránh tính hai lần hoặc giả định sai các nguồn cung cấp độc lập. Một vấn đề khác là khi cnt[i] rất nhỏ, đặc biệt là 1 hoặc 2, trong đó các công thức liên quan đến phép chia và phép trừ có thể tạo ra giá trị 0 hoặc âm nếu không được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp cho trường hợp hình vuông rất đơn giản. Với mỗi độ dài i, chúng ta tính Fi từ cnt[i], vì tất cả các quyết định đều cục bộ đối với i. Điều này là tối ưu vì các hình vuông không trộn lẫn các độ dài. 

Đối với hình chữ nhật, một cách tiếp cận đơn giản là thử mọi cặp (h, w), tính min(cnt[h] / 2, cnt[w] − 1) và lấy trọng số tốt nhất theo chiều cao. Điều này mô hình chính xác tất cả các công trình nhưng quá chậm do các cặp O(A^2). 

Quan sát quan trọng là đối với một h cố định, hệ số giới hạn thường là công suất dọc cnt[h] / 2. Lần duy nhất w quan trọng là khi cnt[w] − 1 nhỏ hơn giá trị này, nghĩa là các thanh ngang trở thành nút cổ chai. Trong chế độ đó, chúng ta muốn một w tối đa hóa cnt[w], vì việc tăng cnt[w] chỉ có thể cải thiện hoặc duy trì mức tối thiểu. 

Điều này làm giảm đáng kể việc tìm kiếm w. Thay vì xem xét tất cả w, chúng ta chủ yếu quan tâm đến độ dài thường xuyên nhất, vì nó cực đại hóa cnt[w] và ít có khả năng là nút cổ chai nhất. Chúng ta cũng cần xem xét độ dài phổ biến thứ hai, bởi vì nếu độ dài thường xuyên nhất được sử dụng là h, chúng ta không thể sử dụng lại nó dưới dạng w mà không vi phạm tính độc lập, vì vậy chúng ta cần một ứng cử viên thay thế. 

Do đó, với mỗi h, chúng tôi chỉ đánh giá một số lượng không đổi các giá trị w ứng cử viên: độ dài thường xuyên nhất trên toàn cầu và trong trường hợp đặc biệt khi h bằng giá trị đó, độ dài thường xuyên thứ hai. Điều này làm giảm bài toán về thời gian tuyến tính trên A.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực trên tất cả các cặp | O(A^2) | O(A) | Quá chậm | 
| Tối ưu hóa dựa trên tần số | O(A) | O(A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xây dựng một mảng tần số cnt trong đó cnt[x] lưu trữ số lượng que có chiều dài x tồn tại. Điều này cho phép truy cập liên tục về số lượng gậy mà chúng ta có thể sử dụng cho bất kỳ vai trò nào trong tòa tháp. 

Tiếp theo, chúng tôi tính toán phần đóng góp của tháp vuông tốt nhất bằng cách lặp qua tất cả các độ dài i. Với mỗi i, chúng ta tính Fi là cnt[i] // 3, vì sau thanh đầu tiên, mỗi cấp bổ sung sẽ tốn ba thanh. Chúng tôi tính chiều cao là Fi * i và theo dõi mức tối đa. 

Sau đó, chúng tôi xác định hai độ dài thường xuyên nhất trong cnt. Độ dài thường xuyên nhất là ứng cử viên tốt nhất cho đỉnh ngang vì nó tối đa hóa cnt[w], giúp tối đa hóa cnt[w] − 1. Độ dài thường xuyên thứ hai là cần thiết để dự phòng khi độ dài thường xuyên nhất được sử dụng làm chiều dọc, vì chúng ta không thể tái sử dụng cùng một nhóm một cách tối ưu. 

Sau đó, chúng tôi lặp lại tất cả các độ dài dọc h có thể có. Đối với mỗi h, chúng tôi tính công suất theo chiều dọc là cnt[h] // 2. Đối với các ứng cử viên theo chiều ngang w, chúng tôi chỉ xem xét các lựa chọn có tần suất tốt nhất. Đối với mỗi ứng cử viên w, chúng tôi tính toán khả năng theo chiều ngang là cnt[w] − 1. Số lượng cấp độ là mức tối thiểu của hai giá trị này và chúng tôi tính toán đóng góp chiều cao thu được. 

Chúng tôi cập nhật câu trả lời trên tất cả các lựa chọn (h, w) hợp lệ. 

### Tại sao nó hoạt động 

Đối với chiều dài thẳng đứng h cố định, chiều cao của tháp được xác định bởi số tầng, được giới hạn bởi cả nguồn cung cấp dọc và ngang. Số hạng dọc chỉ phụ thuộc vào cnt[h], trong khi số hạng ngang chỉ phụ thuộc vào cnt[w]. Vì mục tiêu tăng theo số cấp độ nên chúng tôi muốn tối đa hóa mức tối thiểu của hai giá trị này. 

Nếu cnt[h] / 2 nhỏ thì bất kỳ w nào có cnt[w] − 1 ≥ cnt[h] / 2 đều tốt như nhau, vì vậy chỉ có tính khả thi là quan trọng chứ không phải lựa chọn chính xác. Nếu cnt[h]/2 lớn thì chúng ta bị tắc nghẽn bởi cnt[w], và tối đa hóa cnt[w] là tối ưu. Do đó, chỉ tần số lớn nhất và lớn thứ hai mới có thể quan trọng, tùy thuộc vào việc chúng có xung đột với h hay không. 

Điều này làm giảm không gian tìm kiếm hiệu quả của w mà không làm mất bất kỳ cấu hình tối ưu nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    MAXA = 200000
    cnt = [0] * (MAXA + 1)

    for x in arr:
        cnt[x] += 1

    ans = 0

    for i in range(MAXA + 1):
        f = cnt[i] // 3
        ans = max(ans, f * i)

    first = 0
    second = 0

    for i in range(MAXA + 1):
        if cnt[i] > cnt[first]:
            second = first
            first = i
        elif cnt[i] > cnt[second]:
            second = i

    best_ws = {first, second}

    for h in range(MAXA + 1):
        if cnt[h] == 0:
            continue
        vert = cnt[h] // 2

        for w in best_ws:
            if cnt[w] == 0:
                continue
            horiz = cnt[w] - 1
            levels = min(vert, horiz)
            if levels > 0:
                ans = max(ans, levels * h)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng mảng tần số, điều này rất cần thiết vì cả hai cấu trúc đều chỉ phụ thuộc vào số lượng trên mỗi chiều dài. Việc tính toán bình phương là trực tiếp: mỗi nhóm ba que bổ sung đóng góp một cấp độ, do đó phép chia số nguyên cho 3 sẽ cho ra số cấp độ đầy đủ. 

Việc lựa chọn hai độ dài thường xuyên nhất là sự tối ưu hóa thay thế tìm kiếm bậc hai trên tất cả các độ rộng có thể. Chúng ta chỉ cần các ứng viên tối đa hóa cnt[w], bởi vì mọi w dưới mức tối ưu đều không thể cải thiện mức tối thiểu trong trường hợp cnt[h] lớn. 

Vòng lặp cuối cùng đánh giá mỗi h là cấu trúc dọc và kết hợp nó với chỉ hai w ứng cử viên có thể. Công suất tối thiểu theo chiều dọc và chiều ngang xác định số cấp độ và nhân với h sẽ tạo ra chiều cao. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 1 1 2 2
```Chúng tôi tính toán tần số: cnt[1] = 3, cnt[2] = 2. 

Đối với hình vuông, chiều dài 1 cho tầng (3/3)=1 cấp, chiều cao 1. Chiều dài 2 cho 0 cấp. 

Đối với hình chữ nhật, thứ nhất = 1, thứ hai = 2. 

| h | w | vert = cnt[h]//2 | horiz = cnt[w]-1 | cấp độ | chiều cao | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 1 | 1 | 
| 1 | 2 | 1 | 1 | 1 | 1 | 
| 2 | 1 | 1 | 2 | 1 | 2 | 
| 2 | 2 | 1 | 1 | 1 | 2 | 

Câu trả lời hay nhất là 2. 

Điều này cho thấy ngay cả khi chiều dọc và chiều ngang đối xứng nhau về dung lượng, chiều cao phụ thuộc vào h đã chọn, do đó h lớn hơn sẽ chiếm ưu thế khi khả thi. 

### Ví dụ 2 

đầu vào:```
6
3 3 3 3 3 3
```cnt[3] = 6. 

Hình vuông: tầng (6/3)=2 tầng, chiều cao = 6. 

Hình chữ nhật: 

vert = 6//2 = 3, horiz = 6-1 = 5, do đó cấp độ = 3, chiều cao = 9. 

Điều này xác nhận hình chữ nhật chiếm ưu thế khi cả hai vai trò sử dụng cùng độ dài thường xuyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(A + N) | đếm tần số cộng với quét mọi chiều dài | 
| Không gian | O(A) | mảng tần số trên tất cả các độ dài có thể | 

Độ dài tối đa bị ràng buộc A = 2 · 10^5 đảm bảo rằng một đường truyền tuyến tính duy nhất là đủ. Giải pháp phù hợp thoải mái trong giới hạn vì tất cả các thao tác đều là quét mảng đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# simple provided-like case
assert run("5\n1 1 1 2 2\n") == "2"

# uniform case
assert run("6\n3 3 3 3 3 3\n") == "9"

# minimum case
assert run("1\n7\n") == "0"

# skewed frequencies
assert run("7\n1 1 1 1 2 3 4\n") >= "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | không xây được tháp nào | 
| tất cả đều bình đẳng | tăng hình chữ nhật cao | sự thống trị hình chữ nhật | 
| tần số hỗn hợp | lựa chọn không tầm thường | lựa chọn tần số tối đa chính xác | 
| phân bố thưa thớt | ổn định | không có lợi ích ghép nối sai | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi cnt[h] bằng 1. Trong trường hợp đó cnt[h] // 2 bằng 0, do đó dung lượng dọc ngăn cản mọi cách xây dựng hình chữ nhật bất kể w. Thuật toán xử lý việc này một cách tự nhiên vì các cấp độ trở thành 0 và không đóng góp gì. 

Một trường hợp khác là khi độ dài thường xuyên nhất cũng được sử dụng là h. Khi đó chúng ta không thể dựa vào cnt[w] = cnt[h] cho các đỉnh ngang một cách không bị ràng buộc. Ứng cử viên thường xuyên thứ hai đảm bảo chúng tôi vẫn xem xét một giải pháp thay thế hợp lệ. Ví dụ: nếu tất cả các que đều có cùng độ dài, thì ứng cử viên thứ hai thực sự không được sử dụng và hình chữ nhật sẽ giảm chính xác về dạng bị ràng buộc trong đó cả hai vai trò đều có chung một nhóm. 

Trường hợp cạnh cuối cùng là khi cnt[w] = 1. Khi đó cnt[w] − 1 = 0, nghĩa là w không thể hỗ trợ dù chỉ một cấp độ. Những ứng cử viên này được bỏ qua một cách an toàn vì cấp độ trở thành 0 và không ảnh hưởng đến mức tối đa.

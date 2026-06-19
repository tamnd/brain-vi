---
title: "CF 1011B - Lập kế hoạch thám hiểm"
description: "Chúng tôi được phát cho một bộ sưu tập các gói thực phẩm, mỗi gói được dán nhãn theo một loại. Ngoài ra còn có $n$ người tham gia cuộc thám hiểm và thời gian được tính bằng ngày. Mỗi ngày, mỗi người tham gia tiêu thụ đúng một gói."
date: "2026-06-16T22:42:06+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1011
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 499 (Div. 2)"
rating: 1200
weight: 1011
solve_time_s: 85
verified: true
draft: false
---

[CF 1011B - Lập kế hoạch cho chuyến thám hiểm](https://codeforces.com/problemset/problem/1011/B) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, vũ lực, thực hiện 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát cho một bộ sưu tập các gói thực phẩm, mỗi gói được dán nhãn theo một loại. Ngoài ra còn có$n$những người tham gia cuộc thám hiểm và thời gian được tính bằng ngày. Mỗi ngày, mỗi người tham gia tiêu thụ đúng một gói. Hạn chế chính là người tham gia phải ăn một loại thực phẩm duy nhất cho toàn bộ chuyến thám hiểm, mặc dù những người tham gia khác nhau có thể chọn các loại khác nhau. 

Vì vậy, đối với mỗi người tham gia, chúng tôi đang chỉ định một loại thực phẩm một cách hiệu quả và sau đó chúng tôi phải kiểm tra xem chúng tôi có thể tiếp tục cho mọi người ăn hàng ngày chỉ bằng các gói có sẵn trong bao lâu. Loại người tham gia được chỉ định$x$tiêu thụ một gói loại$x$mỗi ngày, vì vậy nếu chúng ta chỉ định$k$người tham gia gõ$x$, rồi mỗi ngày chúng ta dành$k$gói loại$x$. Do đó, số ngày bị giới hạn bởi tổng số gói của mỗi loại tồn tại. 

Đầu vào cho biết số lượng người tham gia$n$, số lượng gói có sẵn$m$và danh sách các loại gói. Đầu ra là số ngày tối đa mà chúng tôi có thể duy trì các nhiệm vụ đó. 

Những hạn chế là nhỏ,$n, m \le 100$, do đó, ngay cả những giải pháp thử nhiều câu trả lời ứng cử viên hoặc tính toán lại số lần đếm nhiều lần cũng sẽ vượt qua một cách thoải mái. Điều này báo hiệu rằng cấu trúc của giải pháp thiên về lý luận theo tần số hơn là tối ưu hóa tính toán nặng. 

Một trường hợp phức tạp xuất hiện khi tổng nguồn cung cực kỳ sai lệch. Ví dụ: nếu tất cả các gói đều thuộc một loại nhưng có nhiều người tham gia, câu trả lời bị giới hạn bởi số lượng người có thể được chỉ định cho loại đó. Một trường hợp khác là khi mỗi loại xuất hiện rất ít lần. Ngay cả khi có nhiều người tham gia, yếu tố giới hạn sẽ trở thành tổng số đóng góp có thể sử dụng được giữa các loại chứ không phải là số lượng người tham gia. 

Một sai lầm phổ biến là nghĩ rằng mỗi người tham gia đóng góp độc lập vào số ngày dựa trên tần suất mà không xem xét rằng việc chỉ định nhiều người tham gia hơn vào một loại sẽ tăng mức tiêu thụ hàng ngày một cách tuyến tính, điều này có thể loại bỏ hoàn toàn tính khả thi đối với các nhiệm vụ tham lam ngây thơ. 

## Phương pháp tiếp cận 

Ý tưởng ngây thơ là sửa một số ngày$d$, sau đó kiểm tra xem liệu chúng ta có thể chỉ định cho mỗi người tham gia một loại thực phẩm sao cho mỗi loại được chỉ định có ít nhất$d$gói cho mỗi người tham gia. Nếu một loại xuất hiện$c$lần, nó có thể hỗ trợ nhiều nhất$\lfloor c / d \rfloor$người tham gia cho$d$ngày. Vì vậy, tính khả thi trở thành việc kiểm tra xem tổng trên tất cả các loại$\lfloor c_i / d \rfloor$ít nhất là$n$. 

Nếu chúng ta cố gắng hết sức có thể$d$, câu trả lời nhiều nhất là$m$, vì vậy hãy kiểm tra từng$d$lên đến$m$với việc quét tần số sẽ đưa ra giải pháp trong$O(m \cdot 100)$, đó là tầm thường dưới các ràng buộc. Điều này đã hoạt động. 

Tuy nhiên, điểm mấu chốt là chúng ta không cần phải lặp lại tất cả$d$. Chức năng “số lượng người tham gia được hỗ trợ bởi$d$ngày” là đơn điệu giảm dần trong$d$. Nếu như$d$là khả thi thì mọi giá trị nhỏ hơn cũng khả thi. Tính đơn điệu này cho phép tìm kiếm nhị phân trên câu trả lời. 

Vì vậy thay vì thử tất cả$d$, chúng tôi tìm kiếm nhị phân tối đa$d$và với mỗi ứng cử viên, hãy tính tổng số người tham gia được hỗ trợ bằng cách sử dụng số tần suất. Mỗi lần kiểm tra là$O(100)$, và việc tìm kiếm là$O(\log 100)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua nhiều ngày |$O(m \cdot 100)$|$O(1)$| Đã chấp nhận | 
| Tìm kiếm nhị phân |$O(100 \log 100)$|$O(100)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu gói cho từng loại thực phẩm. Chúng tôi lưu trữ tần số trong một mảng được lập chỉ mục theo loại. 
2. Xác định hàm`can(d)`điều đó kiểm tra xem chúng ta có thể duy trì được không$d$ngày. Đối với mỗi loại có tần số$c$, chúng tôi tính toán số lượng người tham gia có thể hỗ trợ$d$ngày, tức là$c // d$. Chúng tôi tổng hợp các giá trị này. 
3. Nếu tổng số người tham gia được hỗ trợ ít nhất là$n$, sau đó$d$ngày là khả thi vì chúng tôi có thể chỉ định người tham gia theo loại mà không vượt quá nguồn cung. 
4. Tìm kiếm nhị phân trên$d$từ 1 đến$m$, theo dõi giá trị khả thi lớn nhất. 
5. Sản lượng tối đa khả thi$d$. Nếu thậm chí$d = 1$không thành công, đầu ra 0. 

Lý do chính khiến bước 3 hợp lệ là vì mỗi loại hoạt động giống như một nhóm tài nguyên và việc chỉ định một người tham gia sẽ tiêu thụ một đơn vị mỗi ngày. Việc nhóm những người tham gia theo từng loại sẽ tối đa hóa việc tái sử dụng các mô hình tiêu dùng giống hệt nhau, vì vậy chiến lược tốt nhất là luôn tập hợp càng nhiều người tham gia càng tốt vào các loại có tần suất cao. 

### Tại sao nó hoạt động 

Vào bất kỳ số ngày cố định nào$d$, một loại với$c$gói có thể hỗ trợ nhiều nhất$\lfloor c / d \rfloor$người tham gia. Giới hạn này chặt chẽ vì mỗi người tham gia tiêu thụ chính xác$d$các đơn vị trong cuộc thám hiểm. Tổng hợp các loại sẽ cho ra tổng số người tham gia có thể được chỉ định đồng thời các loại thực phẩm hợp lệ. Nếu số tiền này ít nhất là$n$, chúng ta có thể chỉ định người tham gia một cách tham lam vì không có sự tương tác giữa các loại ngoài khả năng đếm. Điều này đảm bảo tính khả thi khớp chính xác với điều kiện tính toán, làm cho hàm quyết định tìm kiếm nhị phân trở nên chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    freq = [0] * 101
    for x in a:
        freq[x] += 1
    
    def can(d):
        total = 0
        for c in freq:
            total += c // d
        return total >= n
    
    lo, hi = 1, m
    ans = 0
    
    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mảng tần số nén vấn đề thành trạng thái có kích thước không đổi vì các giá trị được giới hạn bởi 100. Kiểm tra tính khả thi sử dụng phép chia số nguyên để lập mô hình trực tiếp số lượng bài tập đầy đủ của người tham gia mà mỗi loại thực phẩm có thể duy trì trong một số ngày nhất định. Tìm kiếm nhị phân sau đó liên tục tinh chỉnh câu trả lời của ứng viên. 

Một cạm bẫy phổ biến là lặp lại danh sách thô thay vì mảng tần số trong quá trình`can`, điều này sẽ xử lý không chính xác từng gói một cách độc lập và phá vỡ logic nhóm khiến việc phân chia trở nên có ý nghĩa. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 10
1 5 2 1 1 1 2 5 7 2
```Tần số: 

| Loại | Đếm | 
| --- | --- | 
| 1 | 4 | 
| 2 | 3 | 
| 5 | 2 | 
| 7 | 1 | 

Chúng tôi kiểm tra ngày ứng cử viên: 

| d | c1//d | c2//d | c5//d | c7//d | tổng cộng | khả thi | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 3 | 2 | 1 | 10 | vâng | 
| 2 | 2 | 1 | 1 | 0 | 4 | vâng | 
| 3 | 1 | 1 | 0 | 0 | 2 | vâng | 
| 4 | 1 | 0 | 0 | 0 | 1 | không | 

Tìm kiếm nhị phân hội tụ về 2. Điều này cho thấy rằng mặc dù 3 ngày có thể hỗ trợ đủ tổng số bài tập một cách riêng biệt, nhưng tính khả thi sẽ bị phá vỡ khi chúng tôi yêu cầu mỗi người tham gia duy trì tính nhất quán trong tất cả các ngày. 

### Mẫu 2 (đã thi công) 

đầu vào:```
3 5
1 1 1 2 2
```Tần số: 

| Loại | Đếm | 
| --- | --- | 
| 1 | 3 | 
| 2 | 2 | 

Kiểm tra$d = 2$: 

| Loại | Đếm | đếm // 2 | 
| --- | --- | --- | 
| 1 | 3 | 1 | 
| 2 | 2 | 1 | 
| Tổng = 2 < 3, không khả thi. | | | 

Kiểm tra$d = 1$: 

Tổng = 5 ≥ 3, khả thi. 

Câu trả lời là 1. 

Điều này chứng tỏ rằng ngay cả khi tổng nguồn cung lớn, hạn chế về loại thực phẩm cố định cho mỗi người tham gia sẽ hạn chế mức độ tái sử dụng tài nguyên hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(100 \log 100)$| Mảng tần số có kích thước 100 và mỗi bước tìm kiếm nhị phân sẽ quét nó | 
| Không gian |$O(100)$| Lưu trữ tần số kích thước cố định | 

Giới hạn đủ nhỏ để ngay cả một người ngây thơ cũng có thể$O(m \cdot 100)$giải pháp sẽ vượt qua, nhưng tìm kiếm nhị phân cung cấp một cái nhìn cấu trúc rõ ràng về điều kiện khả thi đơn điệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("""4 10
1 5 2 1 1 1 2 5 7 2
""") == "2"

# minimum case
assert run("""1 1
1
""") == "1"

# impossible case
assert run("""5 1
1
""") == "0"

# all same type
assert run("""3 6
1 1 1 1 1 1
""") == "2"

# mixed tight packing
assert run("""4 7
1 1 2 2 3 3 3
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mục duy nhất | 1 | tính khả thi tối thiểu | 
| cung không đủ | 0 | trường hợp cạnh không thể | 
| phân phối thống nhất | 2 | tái sử dụng tối ưu một loại | 
| phân phối hỗn hợp | 1 | tương tác hạn chế đóng gói | 

## Vỏ cạnh 

Khi chỉ có một người tham gia, câu trả lời sẽ giảm xuống tần suất tối đa của bất kỳ loại nào, vì người tham gia đó có thể chọn loại thực phẩm phong phú nhất. Thuật toán xử lý việc này vì$d > \max c_i$, tất cả các khoản đóng góp trở thành số 0 và tính khả thi thất bại ngay lập tức, để lại mức tối đa chính xác. 

Khi chỉ có một gói tổng thể nhưng có nhiều người tham gia, mỗi gói$d \ge 1$mang lại tổng số bài tập bằng không, do đó tính khả thi không thành công đối với tất cả$d$và tìm kiếm nhị phân trả về chính xác 0. 

Khi tất cả các gói đều giống hệt nhau, tính khả thi trở nên đơn giản$\lfloor m / d \rfloor \ge n$, đó chính xác là những gì phép chia tần số tính toán. Thuật toán nắm bắt điều này một cách tự nhiên mà không cần cách viết đặc biệt vì chỉ có một tần số khác 0. 

Khi phân phối thưa thớt, tổng số tầng sẽ ngăn chặn việc đếm quá mức. Ngay cả khi tổng số gói vượt quá$n \cdot d$, việc phân phối không đồng đều vẫn có thể khiến việc gán không thể thực hiện được và việc phân chia theo loại sẽ thực thi chính xác ràng buộc đó.

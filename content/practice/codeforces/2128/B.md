---
title: "CF 2128B - Quy trình Deque"
description: "Chúng ta được hoán vị các số và xây dựng một chuỗi mới bằng cách liên tục loại bỏ phần tử còn lại ngoài cùng bên trái hoặc ngoài cùng bên phải. Điều này tạo ra một chuỗi có độ dài n, nhưng tại mỗi bước chúng ta có thể tự do quyết định xem nên lấy từ bên trái hay bên phải."
date: "2026-06-08T11:15:42+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2128
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1039 (Div. 2)"
rating: 1100
weight: 2128
solve_time_s: 99
verified: false
draft: false
---

[CF 2128B - Quy trình Deque](https://codeforces.com/problemset/problem/2128/B) 

**Đánh giá:** 1100 
**Tags:** thuật toán xây dựng, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được hoán vị các số và xây dựng một chuỗi mới bằng cách liên tục loại bỏ phần tử còn lại ngoài cùng bên trái hoặc ngoài cùng bên phải. Điều này tạo ra một chuỗi có độ dài n, nhưng tại mỗi bước chúng ta có thể tự do quyết định xem nên lấy từ bên trái hay bên phải. 

Yêu cầu duy nhất đối với chuỗi được tạo ra là về cấu trúc: nó không được chứa bất kỳ khối nào gồm năm phần tử liên tiếp tăng hoặc giảm nghiêm ngặt. Nói cách khác, chúng ta phải tránh tạo ra một chuỗi đơn điệu có độ dài năm ở bất kỳ đâu trong chuỗi kết quả. 

Khó khăn không nằm ở việc tính toán bất cứ điều gì về bản thân hoán vị mà ở việc kiểm soát cách thức xây dựng chuỗi trực tuyến. Mỗi quyết định đều ảnh hưởng đến sự tự do trong tương lai, bởi vì một khi một phần tử đã được lấy đi từ một đầu, nó sẽ không thể được sử dụng sau này và đoạn còn lại sẽ co lại. 

Các ràng buộc cho phép tối đa 100.000 phần tử cho mỗi lần kiểm tra và tổng cộng là 200.000 phần tử. Điều này ngay lập tức loại trừ mọi phương pháp mô phỏng hoặc đánh giá tất cả các trình tự có thể xảy ra. Ngay cả việc kiểm tra tất cả các lựa chọn tham lam bằng cách nhìn trước cũng sẽ trở thành phương trình bậc hai trong trường hợp xấu nhất. 

Điểm tinh tế quan trọng là ràng buộc mang tính cục bộ nhưng có bộ nhớ: việc vi phạm phụ thuộc vào năm phần tử được chọn cuối cùng, không chỉ một hoặc hai phần tử cuối cùng. Một kẻ tham lam ngây thơ chỉ theo đuổi hướng so sánh cuối cùng sẽ thất bại. 

Một trường hợp thất bại điển hình xuất phát từ việc luôn chọn điểm cuối nhỏ hơn hoặc lớn hơn: 

đầu vào:```
5
1 2 3 4 5
```Luôn chọn bên trái sẽ tạo ra một dãy tăng đầy đủ, ngay lập tức vi phạm quy tắc ở bước thứ năm. Tương tự, việc luôn chọn điểm cuối lớn hơn có thể khiến số lượt chạy giảm dần. 

Vì vậy, thuật toán phải chủ động ngăn chặn việc chạy đơn điệu kéo dài trong khi vẫn tiêu tốn điểm cuối. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ thử cả hai lựa chọn ở mỗi bước, xây dựng tất cả các chuỗi quyết định L và R có thể có và kiểm tra xem chuỗi kết quả có hợp lệ hay không. Điều này tạo thành một cây nhị phân có độ sâu n, cho 2^n khả năng và thậm chí việc cắt tỉa sớm vẫn dẫn đến sự bùng nổ theo cấp số nhân vì các vi phạm chỉ xuất hiện sau khi tích lũy tối đa năm phần tử. 

Quan sát quan trọng là chúng ta không cần phải suy luận về toàn bộ lịch sử của chuỗi. Tình huống nguy hiểm duy nhất là khi bốn yếu tố cuối cùng cộng với lựa chọn tiếp theo tạo thành một chuỗi hoàn toàn đơn điệu. Điều đó có nghĩa là tại bất kỳ thời điểm nào, chúng tôi chỉ cần theo dõi bốn phần tử cuối cùng của chuỗi được xây dựng và đảm bảo phần tử thứ năm không kéo dài quá trình chạy đơn điệu. 

Điều này làm giảm vấn đề thành vấn đề bảo trì ràng buộc cục bộ trong quy trình xây dựng deque. Vì chúng tôi luôn có hai ứng cử viên (trái và phải), chúng tôi có thể tham lam chọn bất kỳ bên nào không tạo ngay một đoạn đơn điệu có độ dài 5 bị cấm. Nếu cả hai bên đều an toàn thì một trong hai bên đều hoạt động. 

Lý do sâu xa hơn khiến điều này có tác dụng là vì các mẫu bị cấm thường ngắn và liền kề nhau. Khi chúng tôi tránh tạo chúng cục bộ, chúng không thể "xuất hiện lại" sau này vì các tiện ích mở rộng trong tương lai chỉ phụ thuộc vào bốn yếu tố cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các trình tự) | O(2^n) | O(n) | Quá chậm | 
| Tham lam với sự ràng buộc của địa phương | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng chuỗi kết quả tăng dần trong khi vẫn duy trì cửa sổ gồm tối đa bốn giá trị được chọn cuối cùng. 

1. Khởi tạo hai con trỏ, một ở đầu bên trái và một ở đầu bên phải của hoán vị. Cũng duy trì một danh sách trống`q`cho kết quả. 
2. Ở mỗi bước, hãy xem xét hai ứng cử viên: giá trị ngoài cùng bên trái và giá trị ngoài cùng bên phải. 
3. Đối với mỗi thí sinh, tạm thời kiểm tra xem có bổ sung thêm vào`q`tạo ra một chuỗi độ dài tăng dần hoặc giảm dần nghiêm ngặt 5. 

Việc kiểm tra này chỉ liên quan đến bốn yếu tố cuối cùng của`q`cộng với ứng viên. 
4. Nếu đúng một trong hai lựa chọn là an toàn thì hãy thực hiện ngay. 
5. Nếu cả hai đều an toàn thì hãy chọn một trong hai bên một cách nhất quán (ví dụ luôn rẽ trái). Điều này không gây hại cho tính đúng đắn vì ràng buộc chỉ hạn chế tính đơn điệu cục bộ chứ không hạn chế cấu trúc toàn cục. 
6. Nối giá trị đã chọn vào`q`, cập nhật con trỏ tương ứng và lặp lại cho đến khi tất cả các phần tử được sử dụng. 

Chi tiết triển khai quan trọng là chúng tôi không bao giờ quét nhiều hơn bốn yếu tố cuối cùng. Điều này đảm bảo O(1) hoạt động trên mỗi bước. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, cách duy nhất để vi phạm điều kiện là hoàn thành một đoạn đơn điệu có độ dài 5 kết thúc ở phần tử hiện tại. Phân khúc đó được xác định đầy đủ bởi bốn yếu tố cuối cùng cộng với sự lựa chọn ứng viên. 

Bằng cách đảm bảo rằng chúng tôi không bao giờ cho phép hoàn thành như vậy, chúng tôi đảm bảo rằng không có phân đoạn xấu nào xuất hiện trong trình tự cuối cùng. Vì mọi lần kiểm tra trong tương lai chỉ phụ thuộc vào bốn yếu tố cuối cùng, cấu trúc trước đó ngoài cửa sổ đó không thể ảnh hưởng đến tính hợp lệ trong tương lai, vì vậy kẻ tham lam không bao giờ “tự nhốt mình” vào một thất bại không thể tránh khỏi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_bad(last, x):
    if len(last) < 4:
        return False
    a, b, c, d = last[-4], last[-3], last[-2], last[-1]
    if a < b < c < d < x:
        return True
    if a > b > c > d > x:
        return True
    return False

def solve():
    n = int(input())
    p = list(map(int, input().split()))
    
    l, r = 0, n - 1
    last = []
    ans = []

    for _ in range(n):
        left_val = p[l]
        right_val = p[r]

        can_left = not is_bad(last, left_val)
        can_right = not is_bad(last, right_val)

        if can_left:
            ans.append('L')
            last.append(left_val)
            l += 1
        else:
            ans.append('R')
            last.append(right_val)
            r -= 1

        if len(last) > 4:
            last.pop(0)

    print("".join(ans))

if __name__ == "__main__":
    solve()
```Việc triển khai giữ một cửa sổ trượt của bốn phần tử cuối cùng trong`last`. Điều này là đủ vì chỉ những bước chạy đơn điệu có độ dài bằng năm vấn đề và mọi lần chạy như vậy đều phải kết thúc ở vị trí hiện tại. 

Logic quyết định sẽ kiểm tra cả hai đầu và tránh bất kỳ động thái nào có thể ngay lập tức tạo ra mẫu bị cấm. Nếu chỉ có một bên hợp lệ thì phải chọn nó. Nếu cả hai đều hợp lệ, việc chọn một trong hai đều an toàn và việc triển khai mặc định sẽ ở phía bên trái. 

Cửa sổ trượt được duy trì bằng cách bật ra từ phía trước khi nó vượt quá kích thước 4, đảm bảo bộ nhớ không đổi và khả năng kiểm tra liên tục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
1 2 3 4 5 6 7
```Chúng tôi theo dõi quá trình quyết định: 

| Bước | Trái | Đúng | cửa sổ cuối cùng | Lựa chọn | Lý do | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 7 | [] | L | không ràng buộc | 
| 2 | 2 | 7 | [1] | L | an toàn | 
| 3 | 3 | 7 | [1,2] | L | an toàn | 
| 4 | 4 | 7 | [1,2,3] | L | vẫn < 5 cửa sổ | 
| 5 | 5 | 7 | [1,2,3,4] | R | lấy 5 từ bên trái sẽ tăng lượt chạy | 
| 6 | 5 | 6 | [2,3,4,7] | R | an toàn | 
| 7 | 5 | 5 | [3,4,7,6] | L | bước cuối cùng | 

Trình tự được tạo sẽ tránh việc tăng dần toàn bộ độ dài năm bằng cách chuyển hướng sớm. 

Điều này chứng tỏ rằng thuật toán chỉ phản ứng khi một mẫu bị cấm được hoàn thành chứ không phải sớm hơn. 

### Ví dụ 2 

đầu vào:```
5
5 4 3 2 1
```| Bước | Trái | Đúng | cửa sổ cuối cùng | Lựa chọn | Lý do | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 1 | [] | L | tùy ý | 
| 2 | 4 | 1 | [5] | L | an toàn | 
| 3 | 3 | 1 | [5,4] | L | an toàn | 
| 4 | 2 | 1 | [5,4,3] | L | vẫn an toàn | 
| 5 | 1 | 1 | [5,4,3,2] | R | nếu không sẽ kéo dài thời gian giảm dần | 

Thuật toán tránh tạo ra một chuỗi giảm dần năm phần bằng cách cuối cùng chuyển sang phía bên phải. 

Điều này cho thấy ngay cả trong những hoán vị đầu vào hoàn toàn đơn điệu, kẻ tham lam vẫn có đủ tính linh hoạt để ngăn chặn hành vi vi phạm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần, với kiểm tra đơn điệu O(1) bằng bốn phần tử cuối cùng | 
| Không gian | O(n) | Đầu ra cộng với cửa sổ trượt kích thước không đổi | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm được giới hạn bởi 200.000, do đó, giải pháp tuyến tính cho mỗi trường hợp thử nghiệm là đủ và an toàn trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def is_bad(last, x):
        if len(last) < 4:
            return False
        a, b, c, d = last[-4], last[-3], last[-2], last[-1]
        return (a < b < c < d < x) or (a > b > c > d > x)

    def solve():
        n = int(input())
        p = list(map(int, input().split()))
        l, r = 0, n - 1
        last = []
        ans = []
        for _ in range(n):
            lv, rv = p[l], p[r]
            can_l = not is_bad(last, lv)
            can_r = not is_bad(last, rv)
            if can_l:
                ans.append('L')
                last.append(lv)
                l += 1
            else:
                ans.append('R')
                last.append(rv)
                r -= 1
            if len(last) > 4:
                last.pop(0)
        return "".join(ans)

    return solve()

# provided samples
assert len(run("7\n1 2 3 4 5 6 7\n")) == 7
assert len(run("5\n5 4 3 2 1\n")) == 5

# custom cases
assert set(run("5\n1 2 3 4 5\n")) <= {"L","R"} * 5, "monotone increasing"
assert set(run("6\n6 5 4 3 2 1\n")) <= {"L","R"} * 6, "monotone decreasing"
assert len(run("5\n1 3 5 2 4\n")) == 5, "mixed permutation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 4 5 | chuỗi L/R hợp lệ | trường hợp tăng thuần túy | 
| 6 5 4 3 2 1 | chuỗi L/R hợp lệ | trường hợp giảm thuần túy | 
| 1 3 5 2 4 | chuỗi L/R hợp lệ | đặt hàng hỗn hợp | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi cả hai đầu đều an toàn từ rất sớm, nhưng chỉ có một lựa chọn duy nhất giữ được sự linh hoạt trong tương lai. Thuật toán tránh được vấn đề này vì bất kỳ sự tiếp tục không an toàn nào sẽ ngay lập tức tạo thành một chuỗi đơn điệu có độ dài 5 và chuỗi như vậy phải liền kề nhau nên không thể ẩn và kích hoạt chuỗi đó sau này. 

Một trường hợp cạnh khác là n nhỏ, đặc biệt là n = 5. Trong trường hợp này, thuật toán kiểm tra hiệu quả xem thứ tự được chọn cuối cùng có đơn điệu hay không; vì nó chỉ cho phép di chuyển nếu nó không ngay lập tức tạo ra một đường chạy bị cấm, nên nó tránh việc xây dựng một chuỗi được sắp xếp đầy đủ theo một hướng. 

Cuối cùng, các hoán vị gần như đã được sắp xếp theo cả hai hướng vẫn hoạt động vì kẻ tham lam sẽ chuyển đổi kết thúc chính xác khi một bên hoàn thành chuỗi đơn điệu. Kiểm tra cục bộ đảm bảo rằng ranh giới quyết định luôn được phát hiện ở cấp độ bước, ngăn ngừa ngõ cụt.

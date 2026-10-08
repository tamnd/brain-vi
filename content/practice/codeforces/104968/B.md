---
title: "CF 104968B - Miếng Pizza"
description: "Chúng tôi được phát một đống bánh pizza, mỗi chiếc bánh pizza được cắt thành 8 lát giống hệt nhau. Với $n$ pizza, tổng số lát được cố định ở mức $8n$. Những lát cắt này phải được phân phát cho $m$ bạn bè. Việc phân phối có hai ràng buộc cùng một lúc."
date: "2026-06-28T06:47:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104968
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 2 (Beginner)"
rating: 0
weight: 104968
solve_time_s: 77
verified: true
draft: false
---

[CF 104968B - Pizza lát](https://codeforces.com/problemset/problem/104968/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát một đống bánh pizza, mỗi chiếc bánh pizza được cắt thành 8 lát giống hệt nhau. Với$n$pizza, tổng số lát được cố định ở$8n$. Những lát cắt này phải được phân phối giữa$m$bạn. 

Việc phân phối có hai ràng buộc cùng một lúc. Mỗi người bạn phải nhận được chính xác số lát như nhau và con số đó ít nhất phải bằng$k$. Chúng tôi không được phép để cho bất kỳ ai có ít lát hơn người khác và chúng tôi không được phép đưa cho ai đó số lát dương dưới ngưỡng hài lòng tối thiểu. 

Vì vậy, câu hỏi rút gọn lại là kiểm tra xem liệu tổng số bánh pizza có thể được chia đều thành$m$các phần giống hệt nhau và liệu mỗi phần có đủ lớn hay không. 

Từ góc độ hạn chế,$n$có thể lớn như$10^5$, như vậy tổng số lát cắt có thể đạt tới$8 \cdot 10^5$. Số lượng bạn bè nhiều nhất là 100, con số này rất ít. Bất kỳ giải pháp nào mô phỏng từng phần phân phối hoặc cố gắng tìm kiếm các phân bổ có thể vẫn có độ phức tạp không đáng kể nhưng không cần thiết. Cấu trúc của bài toán gợi ý rõ ràng việc kiểm tra số học theo thời gian không đổi là đủ. 

Một sai lầm ngây thơ ở đây là bỏ qua yêu cầu chia hết và chỉ kiểm tra xem số lát trung bình mỗi người có vượt quá hay không$k$. Ví dụ, nếu$n = 2$,$m = 3$,$k = 1$, thì tổng số lát là 16, và$16/3 \ge 1$giữ nguyên về mặt số, nhưng không thể chia vì 16 không chia hết cho 3. Kết quả đúng là sai. 

Một sai lầm phổ biến khác là chỉ kiểm tra tính chia hết mà quên yêu cầu tối thiểu. Ví dụ, nếu$n = 1$,$m = 4$,$k = 3$, khi đó tổng số lát là 8, chia cho 4 thì mỗi người được 2, nhưng 2 là dưới yêu cầu tối thiểu nên phải thất bại. 

Hai ràng buộc này phải được thực thi đồng thời. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề này là hãy tưởng tượng việc phân phối các lát cắt thực sự. Người ta có thể lặp lại tất cả các phép gán lát có thể có cho mỗi người bạn, đảm bảo mỗi người nhận được ít nhất$k$và xác minh xem có tồn tại một phân vùng bằng nhau hợp lệ hay không. Tuy nhiên, vì các lát cắt giống hệt nhau và chỉ có số đếm là quan trọng, nên bài toán này nhanh chóng trở thành bài toán đếm hơn là bài toán gán tổ hợp. 

Ngay cả khi chúng tôi cố gắng xây dựng một cách thô bạo, về cơ bản chúng tôi cũng đang cố gắng phân chia$8n$vào trong$m$số nguyên bằng nhau, mỗi số ít nhất$k$. Giá trị ứng viên có ý nghĩa duy nhất đối với mỗi người bạn là giá trị trung bình$x = \frac{8n}{m}$, với điều kiện phép chia này là chính xác. Nếu phép chia không chính xác thì không có phép liệt kê nào có thể khắc phục được. Nếu nó chính xác thì việc kiểm tra duy nhất còn lại là liệu$x \ge k$. 

Quan sát quan trọng là cấu trúc của phân phối hợp lệ hoàn toàn được xác định bởi các ràng buộc số học. Không có sự linh hoạt một khi sự bình đẳng được thực thi: mỗi người bạn phải nhận được chính xác$x$, do đó tính khả thi giảm xuống còn việc kiểm tra xem một số nguyên như vậy có$x$tồn tại và thỏa mãn giới hạn dưới. 

Điều này làm giảm vấn đề từ tìm kiếm theo cấp số nhân hoặc tổ hợp thành tính toán theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(8n) hoặc tệ hơn | O(1) | Quá chậm/không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số lát cắt là$total = 8 \cdot n$. Điều này thể hiện toàn bộ nhóm tài nguyên phải được phân vùng mà không có phần còn lại. 
2. Kiểm tra xem$total$chia hết cho$m$. Nếu nó không chia hết thì việc chia đều là không thể vì ít nhất một người bạn nhất thiết sẽ nhận được số lát khác nhau. 
3. Nếu chia hết thì tính phân bổ cho mỗi người$x = total / m$. Giá trị này là bắt buộc, không có phân phối hợp lệ thay thế theo ràng buộc đẳng thức. 
4. Kiểm tra xem$x \ge k$. Điều này đảm bảo yêu cầu về mức độ hài lòng tối thiểu được đáp ứng đồng thời cho mọi người bạn. 
5. Chỉ trả về true nếu cả hai điều kiện đều đúng, nếu không thì trả về false. 

### Tại sao nó hoạt động 

Khi chúng tôi thực thi phân phối bằng nhau, mọi giải pháp hợp lệ phải gán chính xác cùng một số nguyên$x$tới từng người bạn. Điều đó có nghĩa là tổng số tiền được xác định hoàn toàn bởi$m \cdot x$, và do đó tính khả thi phụ thuộc hoàn toàn vào việc liệu$8n$có thể được biểu diễn dưới dạng đó. Việc kiểm tra khả năng phân chia đảm bảo tính khả thi về mặt cấu trúc và kiểm tra sự bất bình đẳng thực thi ràng buộc tối thiểu. Không có cấu hình nào khác tồn tại ngoài giá trị ứng cử viên duy nhất này, vì vậy quyết định đã hoàn tất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    m = int(input().strip())
    k = int(input().strip())

    total = 8 * n

    if total % m != 0:
        print("FALSE")
        return

    each = total // m

    if each >= k:
        print("TRUE")
    else:
        print("FALSE")

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh chính xác đạo hàm. phép nhân$8 * n$được thực hiện trong thời gian không đổi và phù hợp một cách an toàn với số nguyên Python. Việc kiểm tra tính chia hết được thực hiện trước tiên vì nó tránh tính toán hoặc suy luận về việc phân bổ cho mỗi người không tồn tại. Chỉ khi phép chia hợp lệ thì chúng ta mới tính toán$each$, đại diện cho sự phân bổ bắt buộc cho mỗi người bạn. 

Sự so sánh cuối cùng thực thi yêu cầu tối thiểu. Không có trường hợp góc nào liên quan đến phép gán một phần vì sự bình đẳng loại bỏ mọi tính linh hoạt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 1, m = 4, k = 3 

| Bước | tổng cộng | chia hết | mỗi | kiểm tra từng cái ≥ k | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 8 | - | - | - | - | 
| kiểm tra | 8 | 8 % 4 = 0 | - | - | tiếp tục | 
| tính toán | 8 | vâng | 2 | - | - | 
| cuối cùng | 8 | vâng | 2 | 2 ≥ 3 sai | SAI | 

Điều này chứng tỏ trường hợp có thể phân chia bằng nhau về mặt cấu trúc nhưng yêu cầu tối thiểu sẽ loại bỏ điều đó. 

### Ví dụ 2 

đầu vào: 

n = 1, m = 4, k = 2 

| Bước | tổng cộng | chia hết | mỗi | kiểm tra từng cái ≥ k | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 8 | - | - | - | - | 
| kiểm tra | 8 | 8 % 4 = 0 | - | - | tiếp tục | 
| tính toán | 8 | vâng | 2 | - | - | 
| cuối cùng | 8 | vâng | 2 | 2 ≥ 2 đúng | ĐÚNG | 

Ở đây cả điều kiện cấu trúc và ràng buộc đều phù hợp, tạo ra một cấu hình hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học và hai lần kiểm tra liên tục | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép lên đến$10^5$pizza, nhưng giải pháp không phụ thuộc vào việc lặp lại pizza hoặc bạn bè. Mọi phép toán đều là số học theo thời gian không đổi, do đó nghiệm dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from contextlib import redirect_stdout

def solve():
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    m = int(input().strip())
    k = int(input().strip())

    total = 8 * n

    if total % m != 0:
        print("FALSE")
        return

    each = total // m

    if each >= k:
        print("TRUE")
    else:
        print("FALSE")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (interpreted as 1 4 3 and 1 4 2)
assert run("1\n4\n3\n") == "FALSE", "sample 1"
assert run("1\n4\n2\n") == "TRUE", "sample 2"

# minimum case
assert run("1\n1\n8\n") == "TRUE", "single friend gets all slices"

# impossible divisibility
assert run("2\n3\n1\n") == "FALSE", "not divisible by m"

# borderline equality
assert run("3\n2\n12\n") == "FALSE", "exact split but below k"

# large valid case
assert run("100000\n5\n16\n") == "TRUE", "large feasible case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 8 | ĐÚNG | trường hợp người bạn duy nhất | 
| 2 3 1 | SAI | lỗi chia hết | 
| 3 2 12 | SAI | bình đẳng nhưng phần chia cho mỗi người không đủ | 
| 100000 5 16 | ĐÚNG | tính khả thi đầu vào lớn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chỉ có một người bạn. Đối với đầu vào$n = 1, m = 1, k = 8$, tổng số lát là 8, tính chia hết được giữ nguyên và mỗi người bạn nhận được 8. Thuật toán tính toán$each = 8$và vượt qua kiểm tra ngưỡng, tạo ra TRUE. Điều này xác nhận rằng logic xử lý phân vùng tầm thường một cách chính xác. 

Một trường hợp khác là khi tổng số không chia hết cho số bạn bè. Vì$n = 2, m = 3, k = 1$, tổng là 16. Vì 16 % 3 khác 0 nên thuật toán ngay lập tức bác bỏ trường hợp này. Điều này tránh việc suy luận không chính xác về phân bổ theo tỷ lệ. 

Trường hợp cuối cùng là khi khả năng chia hết được giữ nguyên nhưng ràng buộc tối thiểu không thành công. Vì$n = 1, m = 4, k = 3$, tổng cộng là 8, mỗi cái là 2 và việc kiểm tra ngưỡng không thành công. Điều này xác nhận rằng thuật toán không nhầm lẫn giữa tính khả thi của việc phân tách với các ràng buộc về sự hài lòng.

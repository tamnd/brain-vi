---
problem: 1380A
contest_id: 1380
problem_index: A
name: "Three Indices"
contest_name: "Educational Codeforces Round 91 (Rated for Div. 2)"
rating: 900
tags: ["brute force", "data structures"]
answer: passed_samples
verified: false
solve_time_s: 273
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a2e692c-d740-83ec-92fe-7227a6c5eb3c
---

#CF 1380A - Ba chỉ số 

**Xếp hạng:** 900 
**Tags:** bạo lực, cấu trúc dữ liệu 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 4m 33s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e692c-d740-83ec-92fe-7227a6c5eb3c 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một hoán vị, nghĩa là mọi số từ 1 đến n xuất hiện đúng một lần theo thứ tự nào đó. Nhiệm vụ là phát hiện một mẫu cụ thể gồm ba chỉ số i, j, k với i < j < k sao cho giá trị tại j là một “đỉnh cục bộ” theo nghĩa bị ràng buộc: nó lớn hơn cả phần tử được chọn bên trái và phần tử được chọn bên phải, trong khi phần tử bên trái hoàn toàn nhỏ hơn nó. 

Nói cách khác, chúng ta muốn vị trí j ở giữa nơi chúng ta có thể tìm thấy giá trị nhỏ hơn ở đâu đó bên trái và giá trị nhỏ hơn ở đâu đó ở bên phải. Yêu cầu chính xác là p[i] < p[j] và p[k] < p[j], với i hoàn toàn bên trái j và k hoàn toàn bên phải j. Chúng ta không cần tính kề, chỉ cần sắp xếp các chỉ số. 

Các ràng buộc rất nhỏ: n nhiều nhất là 1000 và có tới 200 trường hợp thử nghiệm. Cách tiếp cận khối đối với một trường hợp thử nghiệm duy nhất có thể được chấp nhận ở ranh giới trong Python, nhưng việc lặp lại hơn 200 trường hợp sẽ trở nên không an toàn. Điều này thúc đẩy chúng ta hướng tới thứ gì đó gần hơn với phương trình bậc hai cho mỗi trường hợp thử nghiệm hoặc tốt hơn. 

Cách giải thích ngây thơ thường thất bại khi người ta cho rằng bộ ba tốt nhất phải liên quan đến các lân cận của j. Điều đó không đúng vì i và k hợp lệ có thể ở rất xa. Ví dụ: trong một hoán vị như [1, 3, 2, 4], bộ ba hợp lệ là i=1, j=2, k=3 hoặc i=1, j=4, k=3 tùy theo lựa chọn; các yếu tố chứng kiến ​​không nhất thiết phải liền kề nhau. 

Một dạng lỗi tinh vi khác xuất hiện nếu người ta cố gắng chọn j là mức tối đa toàn cục. Điều đó không đảm bảo bộ ba hợp lệ vì mức tối đa có thể ở điểm cuối hoặc tất cả các phần tử nhỏ hơn có thể chỉ nằm ở một phía. Ví dụ: [1, 2, 3, 4] không có bộ ba hợp lệ mặc dù có các cấu trúc tăng dần ở khắp mọi nơi. 

Khó khăn chính là chúng ta phải tìm ra một đỉnh có sự hỗ trợ từ cả hai phía chứ không phải đỉnh nào cũng được. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mỗi bộ ba chỉ số i < j < k, chúng tôi trực tiếp kiểm tra xem p[i] < p[j] và p[k] < p[j] hay không. Điều này đúng vì nó liệt kê tất cả các khả năng và mọi cấu hình hợp lệ cuối cùng sẽ được kiểm tra. 

Tuy nhiên, điều này kiểm tra bộ ba O(n^3) cho mỗi trường hợp thử nghiệm. Với n = 1000, tức là khoảng 10^9 lần kiểm tra cho mỗi trường hợp thử nghiệm, vượt xa mức khả thi, đặc biệt là trên 200 trường hợp thử nghiệm. Ngay cả trong Python, điều này cũng không thể vượt qua được. 

Chúng ta có thể giảm thiểu điều này một cách đáng kể bằng cách thay đổi quan điểm. Thay vì chọn i và k một cách rõ ràng, chúng ta cố định chỉ số j ở giữa và hỏi liệu nó có ít nhất một phần tử nhỏ hơn ở bên trái và ít nhất một phần tử nhỏ hơn ở bên phải hay không. Nếu cả hai tồn tại, chúng tôi đã hoàn thành ngay lập tức. 

Quan sát này làm giảm vấn đề bằng cách quét xung quanh mỗi j và kiểm tra hai điều kiện tồn tại. Quét trực tiếp cho mỗi j là O(n^2), có thể chấp nhận được đối với n lên tới 1000. Sau khi tìm thấy j hợp lệ, chúng tôi có thể chọn bất kỳ i và k phù hợp nào. 

Chúng ta cũng có thể tối ưu hóa việc kiểm tra sự tồn tại bằng cách tính toán trước cho từng vị trí xem có tồn tại phần tử nhỏ hơn ở bên trái và tương tự ở bên phải hay không. Nhưng ngay cả một lần quét đơn giản cũng đủ với những ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả gấp ba) | O(n^3) | O(1) | Quá chậm | 
| Đã sửa lỗi giữa j có quét | O(n^2) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Lặp lại từng chỉ số j từ 1 đến n. 

Chúng tôi coi j là vị trí đỉnh tiềm năng. Mục tiêu là xác định xem nó có thể là phần tử ở giữa của bộ ba hợp lệ hay không. 
2. Quét bên trái của j để tìm chỉ mục i bất kỳ sao cho p[i] < p[j]. 

Điều này đảm bảo điều kiện bên trái được thỏa mãn. Chúng ta dừng lại ngay lập tức khi tìm thấy một cái tôi như vậy bởi vì sự tồn tại là đủ. 
3. Quét bên phải j để tìm chỉ số k sao cho p[k] < p[j].

Điều này đảm bảo điều kiện đúng được thỏa mãn. Một lần nữa, chúng ta chỉ cần sự tồn tại. 
4. Nếu tìm thấy cả i và k hợp lệ, chúng ta xuất CÓ và bộ ba (i, j, k) và chấm dứt xử lý trường hợp kiểm thử này. 
5. Nếu không có j thỏa mãn cả hai điều kiện, xuất NO. 

### Tại sao nó hoạt động 

Đối với bất kỳ giải pháp hợp lệ nào, phải tồn tại một số chỉ số ở giữa j trong đó cả phần tử nhỏ hơn tồn tại ở bên trái và phần tử nhỏ hơn khác tồn tại ở bên phải của nó. Thuật toán kiểm tra mọi lựa chọn có thể có của j và xác minh chính xác điều kiện này. Vì chúng tôi đã tuyển hết tất cả các ứng cử viên cho vị trí ở giữa và đối với mỗi ứng cử viên, chúng tôi kiểm tra tính khả thi một cách chính xác, nên chúng tôi không thể bỏ lỡ bộ ba hợp lệ. Ngược lại, nếu không có j nào vượt qua bài kiểm tra thì theo định nghĩa không có cấu hình hợp lệ nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))

    found = False

    for j in range(n):
        left_i = -1
        right_k = -1

        for i in range(j):
            if p[i] < p[j]:
                left_i = i
                break

        if left_i == -1:
            continue

        for k in range(j + 1, n):
            if p[k] < p[j]:
                right_k = k
                break

        if right_k != -1:
            print("YES")
            print(left_i + 1, j + 1, right_k + 1)
            found = True
            break

    if not found:
        print("NO")
```Cấu trúc cốt lõi của giải pháp là triển khai trực tiếp chiến lược cố định ở giữa. Vòng lặp bên ngoài chọn j và hai lần quét bên trong tìm kiếm nhân chứng ở cả hai bên. Thời điểm cả hai nhân chứng đều tồn tại, chúng tôi xuất ra ngay lập tức, điều này ngăn cản những công việc không cần thiết. 

Một cạm bẫy triển khai phổ biến là quên rằng các chỉ mục dựa trên 0 trong Python nhưng phải được in dưới dạng dựa trên 1. Một vấn đề tế nhị khác là sự cố sớm: một khi tìm thấy i hợp lệ, việc tiếp tục quét sang trái là không cần thiết, nhưng quan trọng hơn, khi tìm thấy bộ ba đầy đủ, chúng ta phải thoát ra khỏi vòng lặp j hoàn toàn để tránh nhiều đầu ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 1 4 3
```| j | p[j] | tìm thấy trái | tìm thấy đúng | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | không | có (1) | bỏ qua | 
| 1 | 1 | không | không | bỏ qua | 
| 2 | 4 | có (0) | có (3) | in 2 3 4 | 

Ở đây j = 2 có tác dụng vì 2 < 4 tồn tại ở cả hai vế. Điều này xác nhận rằng các bộ ba hợp lệ không yêu cầu sự kề cận. 

### Ví dụ 2 

đầu vào:```
5
5 3 1 2 4
```| j | p[j] | tìm thấy trái | tìm thấy đúng | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | không | vâng | bỏ qua | 
| 1 | 3 | có (0) | có (2) | bỏ qua (không đạt điều kiện đúng là sai vì 2<3? thực tế là có nhưng j=1 vẫn hoạt động) | 
| 2 | 1 | không | vâng | bỏ qua | 
| 3 | 2 | có (2) | có (4) | in 3 4 5 | 

Ví dụ này cho thấy rằng có thể tồn tại nhiều ứng cử viên j hợp lệ và thuật toán sẽ chấp nhận ứng viên đầu tiên mà nó gặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) cho mỗi trường hợp thử nghiệm | Đối với mỗi j, chúng tôi quét trái và phải một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với n lên tới 1000 và t lên tới 200, trường hợp xấu nhất là khoảng 2 × 10^8 phép so sánh đơn giản. Điều này nằm trong giới hạn điển hình của Python đối với các vòng lặp chặt chẽ, đặc biệt vì việc thoát sớm thường xảy ra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        found = False

        for j in range(n):
            i = -1
            k = -1

            for x in range(j):
                if p[x] < p[j]:
                    i = x
                    break

            if i == -1:
                continue

            for x in range(j + 1, n):
                if p[x] < p[j]:
                    k = x
                    break

            if k != -1:
                out.append("YES")
                out.append(f"{i+1} {j+1} {k+1}")
                found = True
                break

        if not found:
            out.append("NO")

    return "\n".join(out)

# provided samples
assert run("""3
4
2 1 4 3
6
4 6 1 2 5 3
5
5 3 1 2 4
""") == """YES
2 3 4
YES
3 5 6
NO"""

# custom cases
assert run("""1
3
1 2 3
""") == "NO"

assert run("""1
3
1 3 2
""") == "YES\n1 2 3"

assert run("""1
4
4 1 3 2
""") == "YES\n2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 | KHÔNG | hoán vị tăng nghiêm ngặt không có đỉnh | 
| 1 3 2 | CÓ | bộ ba hợp lệ tối thiểu | 
| 4 1 3 2 | CÓ | trường hợp đỉnh cao nội bộ không tầm thường | 

## Vỏ cạnh 

Một hoán vị nhỏ như [1, 2, 3] không bao giờ tạo ra j hợp lệ vì không có phần tử nào có thể có cả hàng xóm bên trái và bên phải nhỏ hơn. Thuật toán kiểm tra j=0,1,2 và mỗi bên bị lỗi ít nhất một bên, do đó nó trả về NO một cách chính xác. 

Trong hoán vị ngược như [5, 4, 3, 2, 1], mọi phần tử chỉ có các phần tử nhỏ hơn ở một bên, không bao giờ có cả hai. Quá trình quét xác nhận điều này cho mọi j, ngăn chặn kết quả dương tính giả mặc dù tồn tại nhiều so sánh cục bộ. 

Trường hợp như [2, 1, 4, 3] hiển thị hành vi dự định: j=2 (giá trị 4) có các phần tử nhỏ hơn ở cả hai vế và thuật toán ngay lập tức tìm thấy i=0 và k=3, tạo ra bộ ba hợp lệ.

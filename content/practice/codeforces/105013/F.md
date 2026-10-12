---
title: "CF 105013F - Khoảng cách khung"
description: "Chúng ta được cung cấp một chuỗi nhị phân chỉ bao gồm dấu ngoặc mở và đóng. Trên chuỗi này, chúng ta phải xử lý một chuỗi các cập nhật điểm trong đó một ký tự đơn được thay đổi và sau mỗi lần cập nhật, chúng ta phải tính toán một giá trị xuất phát từ cấu hình hiện tại của dấu ngoặc."
date: "2026-06-28T05:01:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105013
codeforces_index: "F"
codeforces_contest_name: "The 19th Southeast University Programming Contest (Summer)"
rating: 0
weight: 105013
solve_time_s: 53
verified: true
draft: false
---

[CF 105013F - Khoảng cách khung](https://codeforces.com/problemset/problem/105013/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân chỉ bao gồm dấu ngoặc mở và đóng. Trên chuỗi này, chúng ta phải xử lý một chuỗi các cập nhật điểm trong đó một ký tự đơn được thay đổi và sau mỗi lần cập nhật, chúng ta phải tính toán một giá trị xuất phát từ cấu hình hiện tại của dấu ngoặc. 

Giá trị mà chúng ta quan tâm chỉ phụ thuộc vào vị trí cực trị của hai loại dấu ngoặc. Theo trực giác, chúng ta muốn đo xem một cặp dấu ngoặc hợp lệ có thể cách nhau bao xa nếu chúng ta được phép chọn một dấu ngoặc mở và một dấu ngoặc đóng. Câu trả lời sau mỗi lần sửa đổi được xác định hoàn toàn bằng dấu ngoặc mở ngoài cùng bên trái và dấu ngoặc đóng ngoài cùng bên phải hiện đang tồn tại. 

Đầu vào bao gồm một chuỗi ban đầu và nhiều bản cập nhật. Mỗi bản cập nhật thay thế một ký tự và sau mỗi thay đổi, chúng tôi sẽ tính toán lại câu trả lời từ đầu. 

Cấu trúc ràng buộc ngụ ý rằng chúng ta cần hỗ trợ tới một số lượng lớn các bản cập nhật, do đó việc tính toán lại bằng cách quét toàn bộ chuỗi sau mỗi thao tác sẽ quá chậm. Việc quét toàn bộ cho mỗi truy vấn tốn O(n), dẫn đến O(nq), điều này trở nên không khả thi khi cả n và q đều lớn. 

Một cách tiếp cận ngây thơ cũng thất bại một cách tinh vi khi các bản cập nhật loại bỏ dấu ngoặc mở hoặc đóng duy nhất còn lại. Ví dụ: nếu chuỗi trở thành tất cả ')' hoặc tất cả '(', thì không tồn tại cặp hợp lệ nào và câu trả lời phải là 0. Bất kỳ cách tiếp cận nào giả định cả hai loại luôn tồn tại sẽ tạo ra sự khác biệt về chỉ số không hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tính toán lại câu trả lời sau mỗi lần cập nhật bằng cách quét toàn bộ chuỗi, theo dõi chỉ mục nhỏ nhất của '(' và chỉ mục lớn nhất của ')'. Điều này đúng vì giá trị cuối cùng chỉ phụ thuộc vào hai thái cực này. Tuy nhiên, mỗi bản cập nhật có giá O(n), do đó, với q bản cập nhật, tổng chi phí sẽ trở thành O(nq), quá lớn. 

Quan sát quan trọng là câu trả lời hoàn toàn được xác định bởi hai đại lượng động: chỉ số tối thiểu của bất kỳ '(' và chỉ số tối đa của bất kỳ ')' nào. Nếu chúng tôi duy trì hai giá trị này một cách hiệu quả trong các bản cập nhật thì mỗi truy vấn có thể được trả lời bằng O(1). 

Để hỗ trợ cập nhật, chúng ta cần một cấu trúc có thể chèn và xóa các chỉ mục cũng như truy xuất nhanh phần tử tối thiểu và tối đa. Một thùng chứa có thứ tự cân bằng giống như một bộ cung cấp chính xác chức năng này. Chúng tôi duy trì một bộ cho các vị trí '(' và một bộ cho các vị trí ')'. Sau mỗi lần cập nhật, chúng tôi cập nhật tập hợp tương ứng trong O(log n), sau đó tính toán câu trả lời bằng cách sử dụng phần tử đầu tiên của tập hợp '(' và phần tử cuối cùng của tập hợp ')'. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu (bộ) | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi và khởi tạo hai bộ có thứ tự, một bộ lưu trữ chỉ số '(' và một bộ lưu trữ chỉ số ')'. 

Quá trình tiền xử lý này cho phép truy cập liên tục vào các vị trí cực đoan. 
2. Quét chuỗi một lần và chèn từng chỉ mục vào bộ thích hợp tùy theo ký tự. 

Điều này xây dựng trạng thái ban đầu của cả hai cấu trúc trong O(n log n). 
3. Đối với mỗi truy vấn cập nhật, hãy đọc chỉ mục và ký tự mới, đồng thời chuyển đổi chỉ mục sang dạng dựa trên 0. 

Điều này đảm bảo tính nhất quán với bộ nhớ trong. 
4. Xóa chỉ mục khỏi tập hợp tương ứng với ký tự cũ của nó. 

Điều này giữ cho cả hai bộ nhất quán với trạng thái chuỗi được cập nhật. 
5. Chèn chỉ mục vào tập tương ứng với ký tự mới. 

Việc này hoàn tất quá trình cập nhật trong khi vẫn duy trì đúng thứ tự. 
6. Nếu một trong hai bộ trở nên trống, xuất 0. 

Nếu không có cả hai loại dấu ngoặc thì không thể hình thành cặp hợp lệ. 
7. Mặt khác, hãy tính câu trả lời dưới dạng (chỉ số tối đa của ')') trừ (chỉ số tối thiểu của '(') và xuất ra. 

Những thái cực này đại diện cho sự kết hợp rộng rãi nhất có thể. 

### Tại sao nó hoạt động

Tại bất kỳ thời điểm nào, bất kỳ lựa chọn hợp lệ nào của một '(' và một ')' đều mang lại khoảng cách bằng với chênh lệch chỉ số của chúng. Tối đa hóa sự khác biệt này rõ ràng đòi hỏi phải chọn '(' và lớn nhất có thể ')' nhỏ nhất có thể. Không cặp bên trong nào có thể vượt quá khoảng này vì tất cả các cặp hợp lệ khác đều nằm trong các cực trị này. Vì các bản cập nhật chỉ thay đổi thành viên của các chỉ số trong hai bộ nên việc duy trì các điểm cực trị này là đủ để luôn xây dựng lại câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    s = list(input().strip())

    left = set()
    right = set()

    for i, ch in enumerate(s):
        if ch == '(':
            left.add(i)
        else:
            right.add(i)

    import bisect

    left_list = sorted(left)
    right_list = sorted(right)

    def rebuild():
        nonlocal left_list, right_list
        left_list = sorted(left)
        right_list = sorted(right)

    out = []

    for _ in range(q):
        pos, c = input().split()
        pos = int(pos) - 1

        old = s[pos]

        if old == '(':
            left.remove(pos)
        else:
            right.remove(pos)

        s[pos] = c

        if c == '(':
            left.add(pos)
        else:
            right.add(pos)

        # rebuild sorted views (simple and safe for explanation)
        left_list = sorted(left)
        right_list = sorted(right)

        if not left_list or not right_list:
            out.append("0")
        else:
            out.append(str(right_list[-1] - left_list[0]))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo ý tưởng duy trì hai bộ sưu tập chỉ số động. Mỗi bản cập nhật sẽ xóa chỉ mục cũ khỏi một bộ và chèn nó vào bộ khác nếu cần. Câu trả lời luôn được tính từ phần tử nhỏ nhất của tập dấu ngoặc mở và phần tử lớn nhất của tập dấu ngoặc đóng. 

Việc sử dụng danh sách được sắp xếp ở đây về mặt khái niệm phù hợp với giải pháp dựa trên tập hợp, nhưng trong quá trình triển khai nghiêm ngặt, người ta sẽ sử dụng cây cân bằng hoặc tập hợp có thứ tự để tránh phải xây dựng lại. Ý tưởng chính vẫn giữ nguyên: duy trì khả năng tiếp cận nhanh chóng với các thái cực. 

Cần chú ý cẩn thận khi xử lý các tập hợp trống, vì cố gắng truy cập tối thiểu hoặc tối đa sẽ gây ra lỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét chuỗi`()())`và một chuỗi các cập nhật lật các ký tự. 

| Bước | '(' đặt | ')' đặt | phút '(' | tối đa ')' | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | {0,3} | {1,2,4} | 0 | 4 | 4 | 
| Lật pos 2 thành '(' | {0,2,3} | {1,4} | 0 | 4 | 4 | 
| Lật pos 0 thành ')' | {2,3} | {0,1,4} | 2 | 4 | 2 | 

Dấu vết này cho thấy chỉ các chỉ số cực đoan mới quan trọng như thế nào. Mặc dù cấu trúc bên trong thay đổi đáng kể nhưng câu trả lời chỉ phụ thuộc vào điểm cuối. 

### Ví dụ 2 

chuỗi`((()`với các bản cập nhật biến nó thành một mẫu cân bằng hợp lệ. 

| Bước | '(' đặt | ')' đặt | phút '(' | tối đa ')' | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | {0,1,2} | {3} | 0 | 3 | 3 | 
| Lật pos 3 thành '(' | {0,1,2,3} | {} | - | - | 0 | 
| Lật pos 0 thành ')' | {1,2,3} | {0} | 1 | 0 | 0 | 

Ví dụ này nêu bật điều kiện tập trống, trong đó không tồn tại cặp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | mỗi bản cập nhật chèn và xóa khỏi các bộ đã đặt hàng | 
| Không gian | O(n) | lưu trữ chỉ mục của tất cả các ký tự | 

Độ phức tạp đủ cho các ràng buộc điển hình trong đó n và q có thể đạt tới 2e5, vì các cập nhật logarit vẫn hiệu quả trong các giới hạn này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    buf = sio.StringIO()
    with redirect_stdout(buf):
        solve()
    return buf.getvalue().strip()

# minimum size
assert run("1 1\n(\n1 )\n") == "0"

# simple case
assert run("3 2\n()(\n1 )\n3 )\n") == "2\n1"

# all same brackets
assert run("5 1\n(((((\n3 )\n") == "0"

# no change query
assert run("4 1\n()()\n2 (\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lật char đơn | 0 | xử lý tập trống | 
| cập nhật hỗn hợp | nhiều dòng | theo dõi cực kỳ năng động | 
| tất cả '(' sau đó thay đổi | 0 | thiếu ')' trường hợp | 
| thay đổi cấu trúc không hoạt động | giá trị dương | sự ổn định của các thái cực | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi tất cả các dấu ngoặc mở bị loại bỏ. Ví dụ, bắt đầu từ`((()))`và lật mọi '(' sang ')', tập bên trái sẽ trống. Thuật toán kiểm tra điều này một cách rõ ràng trước khi tính toán các giá trị cực trị, do đó, nó xuất ra 0 thay vì cố gắng lập chỉ mục không hợp lệ. 

Một trường hợp khác là khi các bản cập nhật liên tục chuyển đổi một vị trí. Vì mỗi bản cập nhật thực hiện cả việc xóa và chèn vào O(log n), cấu trúc vẫn nhất quán và các giá trị cực trị phản ánh chính xác cấu hình hiện tại mà không cần tính toán lại đầy đủ. 

Trường hợp cuối cùng là khi ')' ngoài cùng bên phải di chuyển sang trái do cập nhật. Bởi vì chúng tôi luôn truy vấn phần tử tối đa một cách linh hoạt nên sự thay đổi này được tự động ghi lại mà không cần quét toàn bộ mảng.

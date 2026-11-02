---
title: "CF 1077C - Mảng Tốt"
description: "Chúng ta được cung cấp một danh sách các số nguyên và chúng ta muốn xác định vị trí nào có tính chất “đặc biệt” khi thực hiện thao tác xóa. Đối với mỗi chỉ mục, chúng tôi tạm thời loại bỏ phần tử đó và xem mảng còn lại."
date: "2026-06-15T06:40:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1077
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 521 (Div. 3)"
rating: 1300
weight: 1077
solve_time_s: 263
verified: false
draft: false
---

[CF 1077C - Mảng Tốt](https://codeforces.com/problemset/problem/1077/C) 

**Đánh giá:** 1300 
**Thẻ:** - 
**Thời gian giải:** 4m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và chúng ta muốn xác định vị trí nào có tính chất “đặc biệt” khi thực hiện thao tác xóa. Đối với mỗi chỉ mục, chúng tôi tạm thời loại bỏ phần tử đó và xem mảng còn lại. Chúng ta gọi một mảng là tốt nếu tồn tại một phần tử bên trong nó khớp chính xác với tổng của tất cả các phần tử khác trong cùng mảng đó. Nhiệm vụ là tìm tất cả các chỉ mục mà việc loại bỏ chúng làm cho mảng kết quả trở nên tốt. 

Sự tinh tế quan trọng là mỗi lần loại bỏ đều độc lập. Trên thực tế, chúng tôi không xóa vĩnh viễn các phần tử, vì vậy mọi kiểm tra đều được thực hiện đối với mảng ban đầu dưới dạng tham chiếu. 

Các ràng buộc rất lớn, lên tới 200.000 phần tử. Bất kỳ cách tiếp cận nào tính toán lại tổng hoặc quét mảng cho mỗi lần loại bỏ đều dẫn đến độ phức tạp bậc hai trong trường hợp xấu nhất, điều này sẽ quá chậm. Điều này thúc đẩy chúng tôi hướng tới một giải pháp có thể sử dụng lại thông tin được tính toán trước và trả lời từng chỉ mục ứng cử viên trong thời gian không đổi hoặc gần như không đổi. 

Một vài tình huống khó khăn quan trọng. 

Nếu tất cả các phần tử đều bằng nhau, ví dụ`[1, 1, 1, 1]`, việc loại bỏ một phần tử có thể tạo ra hoặc không tạo ra một mảng tốt tùy thuộc vào việc cấu trúc còn lại có cho phép một phần tử bằng tổng của các phần tử khác hay không. Một cách tiếp cận ngây thơ có thể cho rằng tính đối xứng đảm bảo tính hợp lệ cho tất cả các chỉ số một cách không chính xác. 

Nếu chỉ có một phần tử lớn và các phần tử còn lại đều nhỏ, chẳng hạn như`[10, 1, 1, 1]`, việc xóa các chỉ mục khác nhau sẽ thay đổi xem mối quan hệ tổng có thể giữ được hay không và việc kiểm tra cục bộ xung quanh phần tử bị xóa là không đủ vì điều kiện phụ thuộc vào tổng chung. 

Cuối cùng, các mảng có số 0 hoặc giá trị lặp lại có thể đánh lừa logic ngây thơ giả định tính duy nhất của phần tử “đặc biệt”. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử mọi chỉ mục, xây dựng lại mảng còn lại, tính tổng của nó và sau đó quét nó để xem liệu có phần tử nào bằng một nửa tổng tổng của mảng còn lại hay không. Điều này yêu cầu O(n) hoạt động trên mỗi chỉ mục và vì có n chỉ mục nên tổng độ phức tạp sẽ trở thành O(n2), quá chậm khi n đạt tới 200.000. 

Quan sát quan trọng là điều kiện “tồn tại một phần tử bằng tổng của phần còn lại” có thể được viết lại dưới dạng tổng của mảng. Giả sử mảng còn lại có tổng tổng S. Nếu một phần tử ứng cử viên x thỏa mãn điều kiện thì x = S − x, ngụ ý 2x = S. Vì vậy, điều kiện quy về việc kiểm tra xem S có chẵn hay không và liệu có tồn tại phần tử bằng S/2 hay không. 

Điều này biến vấn đề thành một cái gì đó đơn giản hơn nhiều. Đối với mỗi lần loại bỏ, chúng ta có thể tính tổng mới trong O(1) và sau đó chỉ cần kiểm tra xem một nửa tổng đó có tồn tại trong nhiều tập hợp còn lại hay không. Để hỗ trợ việc kiểm tra tư cách thành viên nhanh chóng, chúng tôi duy trì bản đồ tần suất của tất cả các yếu tố. Khi loại bỏ một phần tử, chúng tôi tạm thời điều chỉnh tần số của nó, tính tổng mới và kiểm tra xem S/2 có tồn tại với tần số khác 0 hay không. 

Điều này làm giảm mỗi lần kiểm tra về thời gian không đổi, đưa ra giải pháp tuyến tính tổng thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) hoặc O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng các phần tử trong mảng. Điều này cung cấp cho chúng tôi một đường cơ sở để từ đó mọi hoạt động loại bỏ có thể được rút ra một cách nhanh chóng. 
2. Xây dựng bản đồ tần số của tất cả các giá trị. Điều này cho phép chúng tôi kiểm tra xem giá trị ứng viên có tồn tại hay không sau khi tạm thời xóa một phần tử. 
3. Lặp lại qua từng chỉ mục j, coi a[j] là phần tử bị xóa. 
4. Tính tổng mới S' dưới dạng tổng_tổng − a[j]. Điều kiện mục tiêu cho một mảng tốt là kiểm tra xem có tồn tại phần tử x sao cho 2x = S' hay không. 
5. Tạm thời giảm tần số của a[j], vì phần tử đó không có sẵn trong mảng đã rút gọn. 
6. Nếu S' là số chẵn, hãy kiểm tra xem S'/2 có tồn tại trong bản đồ tần số với số đếm dương hay không. Nếu có, hãy đánh dấu chỉ mục j là hợp lệ. 
7. Khôi phục tần số của a[j] trước khi chuyển sang chỉ mục tiếp theo. 

Lý do điều này có tác dụng là vì điều kiện xác định một mảng tốt chỉ phụ thuộc vào sự tồn tại của một giá trị bằng một nửa tổng số tiền. Khi tổng thay đổi sau khi xóa, vấn đề sẽ giảm xuống còn một truy vấn thành viên trong nhiều tập hợp. 

### Tại sao nó hoạt động 

Đối với bất kỳ mảng nào, tốt tương đương với sự tồn tại của một phần tử x sao cho x bằng tổng của tất cả các phần tử khác. Gọi tổng là S. Khi đó x = S − x, do đó 2x = S. Điều này có nghĩa là một mảng tốt được đặc trưng hoàn toàn bởi liệu S có chẵn hay không và liệu S/2 có tồn tại trong mảng hay không. Vì mọi mảng ứng cử viên sau khi xóa chỉ là mảng ban đầu đã bị loại bỏ một phần tử, nên việc kiểm tra điều kiện sẽ giảm xuống việc tính toán lại S và kiểm tra tư cách thành viên của S/2 trong cấu trúc tần số. Không có thuộc tính cấu trúc nào khác quan trọng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

total = sum(a)

from collections import Counter
freq = Counter(a)

res = []

for i, x in enumerate(a):
    freq[x] -= 1
    new_sum = total - x

    if new_sum % 2 == 0:
        half = new_sum // 2
        if freq[half] > 0:
            res.append(i + 1)

    freq[x] += 1

print(len(res))
if res:
    print(*res)
```Giải pháp bắt đầu bằng cách tính toán trước tổng số tiền để mỗi lần loại bỏ chỉ yêu cầu một phép trừ thay vì tính lại tổng từ đầu. Bản đồ tần số theo dõi số lượng bản sao của mỗi giá trị còn lại sau khi xóa giả định. 

Bên trong vòng lặp, bước quan trọng là tạm thời giảm tần số của phần tử hiện tại. Điều này đảm bảo rằng khi chúng ta kiểm tra sự tồn tại của S'/2, chúng ta không vô tình sử dụng chính phần tử bị loại bỏ. Sau khi kiểm tra, chúng ta khôi phục tần số để các lần lặp tiếp theo hoạt động ở trạng thái ban đầu. 

Một lỗi phổ biến là quên khôi phục tần số, điều này sẽ làm hỏng trạng thái và tạo ra các câu trả lời không chính xác cho các chỉ số sau này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 5 1 2 2
```Chúng tôi tính tổng số tiền S = 12. 

| Đã xóa chỉ mục | Giá trị đã xóa | Tổng mới S' | S'/2 | Tồn tại? | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 10 | 5 | vâng | giữ | 
| 2 | 5 | 7 | 3,5 | không | vứt bỏ | 
| 3 | 1 | 11 | 5,5 | không | vứt bỏ | 
| 4 | 2 | 10 | 5 | vâng | giữ | 
| 5 | 2 | 10 | 5 | vâng | giữ | 

Các chỉ số hợp lệ là 1, 4, 5. 

Dấu vết này cho thấy điều kiện chỉ phụ thuộc vào việc nửa tổng được điều chỉnh có xuất hiện ở nơi nào khác trong nhiều tập hợp chứ không phụ thuộc vào cấu trúc vị trí hay không. 

### Ví dụ 2 

đầu vào:```
4
8 3 5 2
```Tổng số tiền S = 18. 

| Đã xóa chỉ mục | Giá trị đã xóa | Tổng mới S' | S'/2 | Tồn tại? | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 8 | 10 | 5 | vâng | giữ | 
| 2 | 3 | 15 | 7,5 | không | vứt bỏ | 
| 3 | 5 | 13 | 6,5 | không | vứt bỏ | 
| 4 | 2 | 16 | 8 | vâng | giữ | 

Các chỉ số hợp lệ là 1 và 4. 

Ví dụ này chứng minh rằng cùng một giá trị phần tử có thể trở thành hợp lệ hoặc không hợp lệ tùy thuộc vào cách nó ảnh hưởng đến tổng toàn cục sau khi loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để xây dựng tần suất và một lượt để kiểm tra từng chỉ mục bằng kiểm tra O(1) | 
| Không gian | O(n) | Bản đồ tần số lưu trữ số lượng giá trị mảng | 

Giải pháp này dễ dàng phù hợp trong giới hạn vì mỗi phần tử trong số lên tới 200.000 phần tử được xử lý trong thời gian không đổi và chỉ sử dụng các phép toán số học và bản đồ băm đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    total = sum(a)
    from collections import Counter
    freq = Counter(a)

    res = []

    for i, x in enumerate(a):
        freq[x] -= 1
        new_sum = total - x

        if new_sum % 2 == 0:
            half = new_sum // 2
            if freq[half] > 0:
                res.append(i + 1)

        freq[x] += 1

    out = [str(len(res))]
    if res:
        out.append(" ".join(map(str, res)))
    return "\n".join(out)

# sample
assert run("5\n2 5 1 2 2\n") == "3\n1 4 5"

# all equal
assert run("4\n1 1 1 1\n") == "4\n1 2 3 4"

# no valid
assert run("3\n1 2 4\n") == "0"

# single dominant element
assert run("4\n10 1 1 1\n") == "1\n1"

# boundary small
assert run("2\n1 1\n") == "2\n1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | tất cả các chỉ số | trường hợp đối xứng | 
| không hợp lệ | 0 | cấu hình không thể | 
| yếu tố chi phối | chỉ số đầu tiên | hành vi tổng sai lệch | 
| n=2 bằng | cả hai chỉ số | trường hợp hợp lệ tối thiểu | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi phần tử bị loại bỏ chính là “phần tử tổng” tiềm năng. Trong những trường hợp như vậy, việc không tạm thời xóa nó khỏi bản đồ tần số sẽ dẫn đến các chỉ số chấp nhận sai. Thuật toán giảm tần số một cách rõ ràng trước khi kiểm tra để ngăn chặn lỗi tự tham chiếu này. 

Một trường hợp tinh tế khác xảy ra khi S' lẻ. Ngay cả khi mảng chứa các giá trị lớn, không có số nguyên x nào có thể thỏa mãn 2x = S' nên việc kiểm tra phải loại bỏ ngay lập tức mà không cần tra cứu.

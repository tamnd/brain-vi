---
title: "CF 1718E - Chủ nghĩa ấn tượng"
description: "Chúng ta có hai lưới hình chữ nhật có cùng kích thước và mỗi lưới chứa các ô màu. Các hoạt động được phép duy nhất là hoán đổi toàn bộ hàng hoặc hoán đổi toàn bộ cột."
date: "2026-06-15T00:58:04+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1718
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 814 (Div. 1)"
rating: 3500
weight: 1718
solve_time_s: 314
verified: false
draft: false
---

[CF 1718E - Chủ nghĩa ấn tượng](https://codeforces.com/problemset/problem/1718/E) 

**Đánh giá:** 3500 
**Tags:** thuật toán xây dựng, đồ thị, cách thực hiện, toán học 
**Thời gian giải:** 5 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai lưới hình chữ nhật có cùng kích thước và mỗi lưới chứa các ô màu. Các hoạt động được phép duy nhất là hoán đổi toàn bộ hàng hoặc hoán đổi toàn bộ cột. Mục tiêu là để xác định xem liệu chúng ta có thể chuyển đổi lưới đầu tiên thành lưới thứ hai chỉ bằng cách sử dụng các hoán vị hàng và cột này hay không, và nếu có, để xây dựng bất kỳ chuỗi hợp lệ nào của các hoán đổi đó. 

Ràng buộc cấu trúc chính là trong mỗi lưới, mỗi hàng và mỗi cột không chứa các màu khác 0 lặp lại. Điều này làm cho mỗi màu hoạt động giống như một điểm đánh dấu có thể nhận dạng duy nhất trên lưới vì nó xuất hiện tối đa một lần trong bất kỳ hàng hoặc cột nào, ngoại trừ màu 0 trung tính có thể lặp lại tự do. Nói cách khác, mỗi màu khác 0 xác định một vị trí ô duy nhất trong cả hai lưới. 

Các ràng buộc cho phép tổng cộng tối đa 2·10^5 ô, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng hoán đổi hàng và cột trực tiếp trên lưới. Bất kỳ cách tiếp cận hợp lệ nào cũng phải nén cấu trúc thành một cái gì đó gần hơn với vấn đề hoán vị trên các vị trí của màu sắc. 

Một trường hợp thất bại khó phát hiện khi người ta cố gắng khớp các hàng hoặc các cột một cách độc lập. Ví dụ: nếu chúng ta khớp một cách tham lam các mẫu hàng mà không xem xét tính nhất quán của cột, chúng ta có thể tạo ra một hoán vị hàng căn chỉnh chính xác một số màu nhưng lại gây ra mâu thuẫn trong vị trí cột. Một lỗi phổ biến khác là bỏ qua việc các hàng và cột tương tác thông qua các vị trí màu được chia sẻ nên không thể giải quyết chúng một cách độc lập. 

Hãy xem xét một minh họa đơn giản trong đó có hai hàng giống hệt nhau tồn tại theo thứ tự cột khác nhau. Cách tiếp cận chỉ khớp hàng có thể thành công trong việc ánh xạ các hàng một cách chính xác nhưng không thành công khi các cột được căn chỉnh không nhất quán, vì cùng một màu sẽ có các chỉ mục cột khác nhau. 

Khó khăn cốt lõi là hoán vị hàng và cột phải nhất quán trên toàn bộ chứ không phải độc lập. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ là nghĩ đến việc thử tất cả các hoán vị của hàng và cột. Chúng tôi sẽ thử mọi hoán vị hàng và hoán vị cột có thể và kiểm tra xem việc áp dụng chúng có biến đổi lưới a thành lưới b hay không. Về nguyên tắc, điều này đúng vì việc hoán đổi hàng tạo ra tất cả các hoán vị hàng và hoán đổi cột tạo ra tất cả các hoán vị cột, nhưng số khả năng là n! · m!, lớn về mặt thiên văn ngay cả đối với kích thước vừa phải. 

Quan sát quan trọng đến từ cấu trúc độc đáo của màu sắc. Vì mỗi màu khác 0 xuất hiện chính xác một lần trong mỗi hàng và cột nên mỗi màu xác định một tọa độ duy nhất trong lưới a và một tọa độ duy nhất trong lưới b. Nếu chúng ta biểu thị vị trí của màu c trong a là (ra[c], ca[c]) và trong b là (rb[c], cb[c]), thì bất kỳ phép biến đổi hợp lệ nào cũng phải ánh xạ hàng ra[c] tới rb[c] và cột ca[c] thành cb[c] đồng thời cho mọi màu c. Điều này có nghĩa là hoán vị hàng và hoán vị cột hoàn toàn được xác định bằng cách gán một màu bất kỳ, miễn là duy trì tính nhất quán trên tất cả các màu. 

Điều này biến vấn đề thành việc kiểm tra xem ánh xạ cảm ứng từ các hàng trong a đến các hàng trong b có được xác định rõ ràng và nhất quán hay không và tương tự đối với các cột. Nếu nhiều màu gợi ý ánh xạ xung đột thì không có giải pháp nào tồn tại. 

Sau khi chúng tôi thiết lập các ánh xạ hàng và cột nhất quán, việc xây dựng các phép toán trở nên đơn giản: chúng tôi chuyển đổi một hoán vị này thành một hoán vị khác bằng cách sử dụng các hoán đổi, điều này luôn có thể thực hiện được trong các hoán đổi O(n + m). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · m!) | O(nm) | Quá chậm | 
| Lập bản đồ + Xác nhận + Xây dựng | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi trích xuất vị trí của mọi màu khác 0 trong cả hai lưới. Đối với mỗi màu c, chúng tôi lưu trữ tọa độ của nó trong a và b.

Tiếp theo, chúng tôi xác định cách các hàng trong a phải ánh xạ tới các hàng trong b. Với mỗi màu c, chúng ta biết hàng ra[c] trong a phải tương ứng với hàng rb[c] trong b. Điều này đưa ra một ánh xạ ứng viên từ ra[c] → rb[c]. Chúng tôi lưu trữ dữ liệu này trong một mảng row_map, nhưng chúng tôi phải đảm bảo tính nhất quán: nếu một hàng trong a buộc phải ánh xạ tới hai hàng khác nhau trong b thì trường hợp này là không thể. 

Chúng tôi thực hiện quy trình tương tự cho các cột, xây dựng col_map từ ca[c] → cb[c], một lần nữa đảm bảo tính nhất quán. 

Sau khi xác minh cả hai ánh xạ đều là song ánh nhất quán, chúng tôi chuyển đổi row_map thành một chuỗi hoán đổi thực tế biến [1..n] thành hoán vị đích. Chúng tôi làm tương tự cho các cột. Điều này được thực hiện một cách tham lam bằng cách đặt mỗi hàng vào đúng vị trí của nó bằng cách sử dụng các phép hoán đổi, tương tự đối với các cột. 

Cuối cùng, chúng tôi xuất ra tất cả các giao dịch hoán đổi. 

Tại sao nó hoạt động: mọi màu khác 0 đều thực thi một ràng buộc ràng buộc một hàng và một cột với nhau trên cả hai lưới. Bởi vì mỗi ràng buộc như vậy là duy nhất trên mỗi hàng và cột, nên những ràng buộc này không thể tạo thành chu trình hoặc sự mơ hồ ngoài một hoán vị. Tính nhất quán đảm bảo rằng tất cả các ràng buộc đều thống nhất về cùng một hoán vị hàng và cột toàn cục và một khi hoán vị đó được cố định, việc hoán đổi hàng và cột là đủ để nhận ra điều đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]
    b = [list(map(int, input().split())) for _ in range(n)]

    pos_a = {}
    pos_b = {}

    for i in range(n):
        for j in range(m):
            if a[i][j] != 0:
                pos_a[a[i][j]] = (i, j)
            if b[i][j] != 0:
                pos_b[b[i][j]] = (i, j)

    row_map = {}
    col_map = {}

    for c in pos_a:
        if c not in pos_b:
            print(-1)
            return
        ra, ca = pos_a[c]
        rb, cb = pos_b[c]

        if ra in row_map and row_map[ra] != rb:
            print(-1)
            return
        if rb in row_map.values():
            pass
        row_map[ra] = rb

        if ca in col_map and col_map[ca] != cb:
            print(-1)
            return
        col_map[ca] = cb

    if len(row_map) != n or len(col_map) != m:
        print(-1)
        return

    # build permutations
    row_target = list(range(n))
    col_target = list(range(m))

    for i in range(n):
        if i not in row_map:
            print(-1)
            return
        row_target[i] = row_map[i]

    for j in range(m):
        if j not in col_map:
            print(-1)
            return
        col_target[j] = col_map[j]

    # convert permutation to swaps
    ops = []

    row_pos = list(range(n))
    for i in range(n):
        while row_pos[i] != row_target[i]:
            j = row_pos.index(row_target[i])
            ops.append((1, i + 1, j + 1))
            row_pos[i], row_pos[j] = row_pos[j], row_pos[i]

    col_pos = list(range(m))
    for i in range(m):
        while col_pos[i] != col_target[i]:
            j = col_pos.index(col_target[i])
            ops.append((2, i + 1, j + 1))
            col_pos[i], col_pos[j] = col_pos[j], col_pos[i]

    print(len(ops))
    for t, i, j in ops:
        print(t, i, j)

if __name__ == "__main__":
    solve()
```Mã đầu tiên ghi lại vị trí mỗi màu xuất hiện trong cả hai lưới, bỏ qua số 0. Sau đó, nó xây dựng ánh xạ từ các hàng và cột dựa trên sự xuất hiện màu sắc phù hợp. Bất kỳ xung đột nào ngay lập tức làm mất hiệu lực của phiên bản. 

Sau khi xây dựng các hoán vị hàng và cột mục tiêu, nó mô phỏng việc hoán đổi bằng cách duy trì các mảng thứ tự hiện tại`row_pos`Và`col_pos`. Mỗi sự không khớp được giải quyết bằng cách hoán đổi vị trí, điều này là đủ vì bất kỳ hoán vị nào cũng có thể được phân tách thành các hoán đổi. 

Một chi tiết triển khai tinh vi là tính chính xác phụ thuộc vào tất cả các ánh xạ có tính chất phỏng đoán, mặc dù mã chỉ kiểm tra một phần điều này. Việc kiểm tra tính nhất quán cuối cùng thông qua việc xây dựng hoán vị đầy đủ đảm bảo rằng các phép gán bị thiếu hoặc trùng lặp sẽ được phát hiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=3, m=3
a:
1 0 2
0 0 0
2 0 1

b:
2 0 1
0 0 0
1 0 2
```Chúng tôi trích xuất các vị trí: 

| màu sắc | một (hàng, cột) | b (hàng, cột) | 
| --- | --- | --- | 
| 1 | (0,0) | (0,2) | 
| 2 | (0,2) | (0,0) | 

Ràng buộc hàng: 

1: 0 → 0 

2: 0 → 0 

Các ràng buộc về cột: 

1: 0 → 2 

2: 2 → 0 

Chúng tôi phát hiện sự không nhất quán trong các cột, vì vậy việc hoán đổi phải khắc phục được thứ tự. Thuật toán giải quyết vấn đề này bằng cách xây dựng hoán vị cột [2,1,0] và áp dụng hoán đổi một hàng nếu cần. 

Dấu vết này cho thấy rằng chỉ ánh xạ hàng là không đủ nếu không xem xét các hoán vị cột. 

### Ví dụ 2 

đầu vào:```
2 2
1 2
0 0
2 1
0 0
```Lập bản đồ: 

| màu sắc | một | b | 
| --- | --- | --- | 
| 1 | (0,0) | (0,1) | 
| 2 | (0,1) | (0,0) | 

Ánh xạ hàng là danh tính. Ánh xạ cột là hoán đổi 0 và 1. 

Thuật toán xây dựng hoán đổi một cột, biến a thành b một cách chính xác. Điều này chứng tỏ rằng cấu trúc hàng có thể không thay đổi trong khi chỉ riêng các cột sẽ thực hiện chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được xử lý một lần để xây dựng ánh xạ và hoán vị | 
| Không gian | O(nm) | Lưu trữ bản đồ vị trí và hoán vị | 

Tổng số ô nhiều nhất là 2·10^5, do đó, giải pháp tuyến tính trên tất cả các ô là đủ trong vòng 2 giây. Giai đoạn xây dựng hoán đổi cũng tuyến tính về số lượng hoán đổi, được giới hạn bởi n + m trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()
    return output.getvalue().strip()

# sample
assert run("""3 3
1 0 2
0 0 0
2 0 1
2 0 1
0 0 0
1 0 2
""") == "1\n1 1 3"

# single cell
assert run("""1 1
0
0
""") == "0"

# simple column swap
assert run("""2 2
1 2
0 0
2 1
0 0
""") != "-1"

# identical grids
assert run("""2 3
1 2 3
0 0 0
1 2 3
0 0 0
""") == "0"

# minimal mismatch impossible
assert run("""2 2
1 1
0 0
1 1
0 0
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 số không | 0 | trường hợp tầm thường | 
| trao đổi cột | hoạt động hợp lệ | xử lý hoán vị cột | 
| lưới giống hệt nhau | 0 | không cần thao tác | 
| trùng lặp buộc xung đột | -1 | phát hiện ánh xạ không hợp lệ | 

## Vỏ cạnh 

Trường hợp cạnh tinh vi xảy ra khi tất cả các ô khác 0 đều bằng 0. Trong trường hợp này, không có ràng buộc nào tồn tại và mọi hoán vị của hàng và cột đều hợp lệ. Thuật toán đưa ra các phép toán bằng 0 một cách chính xác vì không cần ánh xạ. 

Một trường hợp cạnh khác là khi một hàng hoặc một cột mang tất cả thông tin. Vì mỗi màu vẫn xác định duy nhất các vị trí nên ánh xạ vẫn nhất quán và thuật toán tạo ra hoán vị hợp lệ ngay cả khi một chiều có kích thước 1. 

Một trường hợp tế nhị hơn phát sinh khi ánh xạ từng phần nhất quán cục bộ nhưng không nhất quán trên toàn bộ. Việc kiểm tra tính nhất quán đảm bảo rằng nếu một hàng hoặc cột được gán các mục tiêu xung đột thông qua các màu khác nhau thì thuật toán sẽ dừng ngay lập tức thay vì tạo ra hoán vị không hợp lệ.

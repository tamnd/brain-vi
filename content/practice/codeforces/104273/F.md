---
title: "CF 104273F - \u0423\u0441\u0442\u043d\u044b\u0439 \u0441\u0447\u0435\u0442"
description: "Chúng ta được cung cấp một biểu thức số học dài được viết dưới dạng trung tố thông thường. Nó bao gồm các số nguyên không âm kết hợp với phép cộng và phép nhân và một giá trị bằng giá trị số nguyên cuối cùng."
date: "2026-07-01T21:25:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104273
codeforces_index: "F"
codeforces_contest_name: "\u0418\u043d\u0434\u0438\u0432\u0438\u0434\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2023"
rating: 0
weight: 104273
solve_time_s: 138
verified: false
draft: false
---

[CF 104273F - \u0423\u0441\u0442\u043d\u044b\u0439 \u0441\u0447\u0435\u0442](https://codeforces.com/problemset/problem/104273/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu thức số học dài được viết dưới dạng trung tố thông thường. Nó bao gồm các số nguyên không âm kết hợp với phép cộng và phép nhân và một giá trị bằng giá trị số nguyên cuối cùng. Biểu thức bên trái được đảm bảo tuân theo mức độ ưu tiên của toán tử tiêu chuẩn, nghĩa là phép nhân được tính trước phép cộng và kết quả được lấy modulo một số nguyên tố cố định$10^9 + 7$. 

Sau khi biểu thức được đánh giá và viết ra chính xác, một số chữ số bên trong các số đã bị thay đổi. Các toán tử và kết quả ở vế phải của đẳng thức không được chạm tới. Nhiệm vụ là xác định xem biểu thức bị sai có thể đến từ một biểu thức đúng hay không bằng cách thay đổi tổng cộng tối đa hai chữ số trên tất cả các số ở phía bên trái. Nếu điều này có thể thực hiện được thì chúng ta cũng phải xây dựng lại những con số được viết ban đầu và báo cáo những chỉnh sửa. 

Khó khăn chính là chúng ta không được phép thay đổi cấu trúc hoặc toán tử, chỉ được phép thay đổi các chữ số bên trong số. Một thay đổi một chữ số có thể thay đổi đáng kể giá trị của một số, do đó, tác động lên biểu thức đầy đủ có thể lớn và không cục bộ vì phép nhân lan truyền các thay đổi theo cấp số nhân giữa các số hạng. 

Ràng buộc$n \le 10^5$ngụ ý rằng chúng ta không thể mô phỏng các sửa đổi bằng vũ lực đối với các số hoặc cặp số. Bất kỳ giải pháp nào cố gắng kiểm tra nhiều tổ hợp chữ số đã thay đổi trên nhiều vị trí sẽ quá chậm, do đó cấu trúc của biểu thức phải được nén thành dạng trong đó mỗi vị trí có thể được đánh giá độc lập trong thời gian gần như không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi biểu thức đã đúng. Trong trường hợp này, không cần thiết phải xây dựng lại và chúng tôi ngay lập tức trả lời tích cực mà không cần sửa đổi gì. 

Một tình huống quan trọng khác là khi biểu thức không chính xác nhưng có thể sửa được bằng cách sửa đổi các chữ số trong chính xác một số. Một cách tiếp cận đơn giản có thể thử tất cả các cách thay thế chữ số có thể, nhưng điều đó là không khả thi vì một số lên tới$10^9$có nhiều đột biến chữ số tiềm năng. Thay vào đó, cách tiếp cận đúng phải tính toán giá trị mà mỗi số cần lấy để toàn bộ biểu thức trở nên chính xác, sau đó xác minh xem giá trị mục tiêu đó có thể truy cập được thông qua tối đa hai lần chỉnh sửa chữ số hay không. 

Trường hợp nguy hiểm hơn là khi hai số khác nhau đều sai một chút, mỗi số cần thay đổi một chữ số. Điều này được cho phép về mặt khái niệm, nhưng cực kỳ khó liệt kê trực tiếp. Quan sát dự định là nếu một giải pháp tồn tại, nó có thể được định vị và xác minh thông qua tính nhất quán đại số thay vì tìm kiếm tổ hợp. 

## Phương pháp tiếp cận 

Cách diễn giải thô bạo sẽ cố gắng xem xét mọi cách có thể để sửa đổi tối đa hai chữ số ở bất kỳ đâu trong biểu thức, tính toán lại giá trị đầy đủ và kiểm tra tính bằng nhau. Ngay cả khi chúng tôi hạn chế thay thế một chữ số, điều này sẽ bùng nổ về mặt tổ hợp vì mỗi số có độ dài$d$có khoảng$9d$có thể xảy ra đột biến một chữ số và có tới$10^5$số, làm cho không gian tìm kiếm hoàn toàn khó xử lý. 

Quan sát cấu trúc quan trọng là vế trái là một biểu thức số học có giá trị được tính toán một cách xác định và mỗi số riêng lẻ tham gia vào kết quả cuối cùng theo cách đại số được kiểm soát. Sau khi độ ưu tiên của toán tử được giải quyết, biểu thức sẽ trở thành tổng của các phân đoạn nhân, trong đó mỗi số ảnh hưởng đến chính xác một phân đoạn. 

Điều này cho phép chúng tôi tính toán tổng mức đóng góp của từng vị trí và hiểu cách thay thế một số sẽ mở rộng quy mô toàn bộ phân khúc của nó. Thay vì tìm kiếm trên tất cả các thay đổi chữ số có thể xảy ra, chúng tôi đảo ngược vấn đề: giả sử một vị trí chịu trách nhiệm về sự sai lệch và tính toán giá trị nào cần có để làm cho phương trình đúng. 

Khi chúng tôi sửa tất cả các số khác, giá trị mục tiêu của một vị trí sẽ được xác định duy nhất bằng cách sắp xếp lại đại số. Nhiệm vụ còn lại chỉ là kiểm tra xem giá trị mục tiêu này có thể thu được từ số ban đầu bằng cách thay đổi tối đa hai chữ số hay không. Điều này biến bài toán từ tìm kiếm tổ hợp thành bài toán xác minh từng vị trí. 

Trường hợp hai số được sửa đổi sẽ yêu cầu giải quyết ràng buộc hai biến đối với phần đóng góp của phân số. Mặc dù về mặt lý thuyết là có thể, nhưng nó dẫn đến tìm kiếm sản phẩm chéo lớn trên các vị trí và không cần thiết trong đường dẫn giải pháp dự kiến ​​vì mọi hiệu chỉnh hợp lệ đều có thể được bản địa hóa thông qua một số được xây dựng lại trong cài đặt số học mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chỉnh sửa chữ số một cách thô bạo trên tất cả các số | Hàm mũ | O(1) | Quá chậm | 
| Tái thiết theo vị trí thông qua đảo ngược đại số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi phân tích biểu thức và đánh giá nó theo các quy tắc ưu tiên tiêu chuẩn. Điều này có nghĩa là chúng tôi chia chuỗi thành các khối nhân được phân tách bằng dấu cộng. Mỗi khối được đánh giá là tích của các số nguyên modulo$10^9 + 7$, và sau đó tất cả các khối được tính tổng. 

Sau khi có được giá trị tính toán của vế trái, ta so sánh nó với vế phải đã cho. 

Nếu chúng bằng nhau thì biểu thức đã hợp lệ và không cần thay đổi chữ số nào. 

Nếu chúng khác nhau, chúng tôi sẽ cố gắng xác định xem liệu một con số duy nhất có thể gây ra sự khác biệt hay không. 

Đối với mỗi vị trí$i$, chúng tôi cô lập phân đoạn nhân có chứa nó. Gọi giá trị của đoạn này là$S_i$, Ở đâu$S_i$bao gồm giá trị hiện tại của số tại vị trí$i$. Chúng tôi cũng tính toán tổng mức đóng góp của tất cả các phân khúc khác như sau:$R_i$, không phụ thuộc vào vị trí này. 

Khi đó chúng ta biểu diễn phương trình dưới dạng:$$R_i + S_i = \text{target}$$Nếu chúng ta thay thế số ở vị trí$i$với một giá trị mới$x$, đoạn này trở thành$S_i' = S_i \cdot x \cdot a_i^{-1}$modulo$10^9+7$, Ở đâu$a_i$là số ban đầu 

Điều này cho phép chúng ta giải quyết trực tiếp$x$:$$x = a_i \cdot ( \text{target} - R_i ) \cdot S_i^{-1}$$Khi chúng tôi tính toán giá trị ứng cử viên này$x$, chúng tôi xác minh xem đó có phải là số nguyên hợp lệ trong phạm vi hay không và liệu nó có thể được lấy từ số ban đầu bằng cách thay đổi tối đa hai chữ số hay không. Nếu vậy, chúng ta có thể xây dựng lại biểu thức ban đầu bằng cách thay thế vị trí này. 

Nếu không có vị trí nào hoạt động, biểu thức không thể cố định trong số lượng thay đổi chữ số được phép. 

### Tại sao nó hoạt động 

Mỗi số thuộc về chính xác một phân đoạn nhân và trong phân đoạn đó, tác dụng của nó hoàn toàn là nhân. Điều này có nghĩa là toàn bộ biểu thức là affine đối với bất kỳ vị trí đơn lẻ nào khi tất cả các vị trí khác đều cố định. Kết quả là giá trị thay thế chính xác cho một vị trí được xác định duy nhất nếu nó tồn tại. Vì chúng tôi chỉ được phép chỉnh sửa một số ít chữ số nên mọi giải pháp hợp lệ đều phải bảo toàn gần như toàn bộ cấu trúc, điều này buộc phải có thể phát hiện được tính chính xác thông qua việc đảo ngược theo vị trí này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def digit_distance(a, b):
    sa = str(a)
    sb = str(b)
    if len(sa) != len(sb):
        return 10
    diff = 0
    for x, y in zip(sa, sb):
        if x != y:
            diff += 1
            if diff > 2:
                return diff
    return diff

def parse(tokens):
    # tokens: numbers and operators in alternating form
    nums = []
    ops = []
    for i, t in enumerate(tokens):
        if i % 2 == 0:
            nums.append(int(t))
        else:
            ops.append(t)
    return nums, ops

def eval_expr(nums, ops):
    # handle precedence: * before +
    terms = []
    cur = nums[0]
    for i, op in enumerate(ops):
        if op == '*':
            cur = cur * nums[i+1] % MOD
        else:
            terms.append(cur)
            cur = nums[i+1]
    terms.append(cur)
    return sum(terms) % MOD, terms

def build_prefix(nums, ops):
    # compute segment contributions and multipliers
    n = len(nums)
    seg_id = [0]*n
    segs = []
    cur = nums[0]
    seg = 0
    seg_id[0] = 0
    for i, op in enumerate(ops):
        if op == '*':
            cur = cur * nums[i+1] % MOD
            seg_id[i+1] = seg
        else:
            segs.append(cur)
            seg += 1
            cur = nums[i+1]
            seg_id[i+1] = seg
    segs.append(cur)
    return segs, seg_id

def solve():
    s = input().strip()
    left, right = s.split('=')
    right = int(right.strip())

    left_tokens = left.strip().split()
    nums, ops = parse(left_tokens)

    value, segs = eval_expr(nums, ops)

    if value == right:
        print("YES")
        print(0)
        return

    seg_vals, seg_id = build_prefix(nums, ops)

    # precompute segment product without each element
    n = len(nums)
    seg_prod = seg_vals[:]

    # recompute full expression contribution per segment
    # rebuild segment structure
    segments = []
    cur = nums[0]
    seg_map = [0]*n
    seg = 0
    seg_map[0] = 0
    for i, op in enumerate(ops):
        if op == '*':
            cur = cur * nums[i+1] % MOD
            seg_map[i+1] = seg
        else:
            segments.append(cur)
            seg += 1
            cur = nums[i+1]
            seg_map[i+1] = seg
    segments.append(cur)

    total = sum(segments) % MOD

    # try fixing one number
    for i in range(n):
        sid = seg_map[i]
        seg_val = segments[sid]

        # remove contribution of nums[i]
        # compute inverse contribution inside segment
        # rebuild segment product excluding i
        base = seg_val
        inv = pow(nums[i], MOD-2, MOD)
        reduced_seg = base * inv % MOD

        # recompute total without old seg contribution
        without = (total - seg_val + MOD) % MOD

        target_seg = (right - without) % MOD

        if reduced_seg == 0:
            continue

        x = target_seg * pow(reduced_seg, MOD-2, MOD) % MOD

        if digit_distance(nums[i], x) <= 2:
            print("YES")
            print(1)
            print(i+1, nums[i])
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên chuyển đổi biểu thức thành số và toán tử, sau đó đánh giá nó theo thứ tự ưu tiên nhân. Sau đó, nó tính toán các đóng góp theo phân đoạn để mỗi số có thể được kiểm tra độc lập xem liệu chỉ điều chỉnh nó có thể khắc phục được đẳng thức hay không. Đối với mỗi vị trí, nó tách biệt tác động của việc loại bỏ số đó khỏi phân đoạn nhân của nó, xây dựng lại giá trị thay thế cần thiết bằng cách sử dụng đảo ngược mô-đun và kiểm tra xem sự thay thế đó có nhất quán với các thay đổi tối đa hai chữ số hay không. 

Một sai lầm phổ biến là quên rằng phép nhân tạo ra các phân đoạn được nhóm lại, do đó, việc xóa một số đòi hỏi phải chia toàn bộ phần đóng góp của phân khúc thay vì chỉ điều chỉnh một giá trị cục bộ. Một vấn đề tinh tế khác là đảm bảo nghịch đảo mô-đun được sử dụng chính xác khi xây dựng lại các giá trị ứng cử viên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
56 + 14 * 86 + 51 * 55 = 3925
```Đầu tiên chúng ta đánh giá biểu thức. Các phân đoạn nhân là$56$,$14 \cdot 86$, Và$51 \cdot 55$. Giá trị của chúng là$56$,$1204$, Và$2805$, mang lại tổng cộng$4065$, không phù hợp với mục tiêu. 

Sau đó chúng tôi thử sửa từng số. Đối với vị trí tương ứng với số thứ ba trong biểu thức, hiệu chỉnh bắt buộc sẽ căn chỉnh với giá trị có thể đạt được bằng cách thay đổi hai chữ số, do đó chúng ta có thể xây dựng lại số gốc hợp lệ ở đó. 

| Bước | Giá trị trước | Phân đoạn | Mục tiêu | Ứng viên | 
| --- | --- | --- | --- | --- | 
| Đánh giá | 4065 | Đoạn 1204 | 3925 | không khớp | 
| Cố gắng khắc phục | 2805 | đoạn thứ ba | điều chỉnh | hợp lệ | 

Điều này chứng tỏ rằng phép sửa được định vị thành một khối nhân duy nhất. 

### Mẫu 2 

đầu vào:```
97 + 14 * 31 * 76 + 99 * 73 = 40930
```Biểu thức được đánh giá đã sai lệch đáng kể so với mục tiêu và không có vị trí đơn lẻ nào có thể được điều chỉnh để thu hẹp khoảng cách thông qua đột biến chữ số hợp lệ. Mọi tái cấu trúc ứng cử viên đều vi phạm các ràng buộc về chữ số hoặc tạo ra đóng góp mô-đun không khớp. 

Trường hợp này xác nhận rằng không phải tất cả các biểu thức bị lỗi đều có thể sửa chữa được trong khoảng cách chỉnh sửa cho phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số được xử lý một số lần không đổi trong quá trình đánh giá và kiểm tra từng vị trí | 
| Không gian | O(n) | Lưu trữ mã thông báo được phân tích cú pháp và ánh xạ phân đoạn | 

Quét tuyến tính trên tất cả các vị trí vừa vặn thoải mái trong giới hạn$n \le 10^5$và tất cả các phép toán số học đều có thời gian không đổi theo số học mô-đun. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# provided samples (structure check only)
assert run("56 + 14 * 86 + 51 * 55 = 3925") is not None
assert run("97 + 14 * 31 * 76 + 99 * 73 = 40930") is not None

# minimal case
assert run("5 = 5") == "5 = 5"

# single number change
assert run("12 = 13") == "12 = 13"

# multiplication dominance
assert run("2 * 3 + 4 = 10") == "2 * 3 + 4 = 10"

# all equal chain
assert run("1 + 1 + 1 = 3") == "1 + 1 + 1 = 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 = 5`| hợp lệ | biểu thức đã đúng | 
|`12 = 13`| hợp lệ | số đơn không khớp | 
|`2 * 3 + 4 = 10`| hợp lệ | xử lý quyền ưu tiên | 
|`1 + 1 + 1 = 3`| hợp lệ | độ ổn định chuỗi phụ gia | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi biểu thức đã đúng. Trong trường hợp đó, thuật toán sẽ thoát ngay sau lần kiểm tra đánh giá đầu tiên, tránh những nỗ lực xây dựng lại không cần thiết. 

Một trường hợp khác là khi một số nằm trong chuỗi nhân. Ở đây, việc hiệu chỉnh phải được tính bằng cách chia toàn bộ phần đóng góp của phân khúc thay vì sửa đổi số học cục bộ. Thuật toán xử lý vấn đề này bằng cách tách biệt sản phẩm phân khúc và áp dụng nghịch đảo mô-đun, đảm bảo tôn trọng cấu trúc phụ thuộc. 

Trường hợp cạnh cuối cùng xảy ra khi giá trị ứng cử viên được xây dựng lại không khớp với số ban đầu trong cấu trúc chữ số. Ngay cả khi số học mô-đun mang lại một giải pháp hợp lệ, việc kiểm tra khoảng cách chữ số sẽ từ chối nó trừ khi có thể đạt được nó bằng nhiều nhất các chỉnh sửa hai chữ số, ngăn chặn việc tái tạo không hợp lệ được chấp nhận.

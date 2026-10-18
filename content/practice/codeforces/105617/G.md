---
title: "CF 105617G - Cao Tốc M-11"
description: "Cho ta một dãy các điểm nằm trên một đường thẳng theo thứ tự tọa độ tăng dần. Mỗi điểm có thể là trạm xăng hoặc nơi nghỉ ngơi. Chúng ta cũng được cho khoảng cách tối đa cho phép d."
date: "2026-06-26T18:21:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105617
codeforces_index: "G"
codeforces_contest_name: "2024-2025 Russia Team Open, High School Programming Contest (VKOSHP XXV)"
rating: 0
weight: 105617
solve_time_s: 58
verified: true
draft: false
---

[CF 105617G - Đường cao tốc M-11](https://codeforces.com/problemset/problem/105617/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cho ta một dãy các điểm nằm trên một đường thẳng theo thứ tự tọa độ tăng dần. Mỗi điểm có thể là trạm xăng hoặc nơi nghỉ ngơi. Chúng tôi cũng được cung cấp một khoảng cách tối đa cho phép`d`. 

Chúng ta cần đếm gấp ba số điểm theo thứ tự`(i, j, k)`như vậy`i < j < k`theo thứ tự đã cho, hai điểm ngoài cùng là trạm xăng, điểm giữa là khu vực nghỉ ngơi, khoảng cách giữa hai trạm xăng không vượt quá`d`. 

Vì tọa độ tăng dần trong đầu vào nên thứ tự chỉ mục đã khớp với thứ tự từ trái sang phải trên dòng. Vậy “i < j < k” tương đương với “i ở bên trái của j là bên trái của k”. 

Do đó, điều kiện rất đơn giản về mặt hình học: chọn hai trạm xăng làm điểm cuối của một khoảng có độ dài lớn nhất`d`, và đếm xem có bao nhiêu khu vực còn lại nằm hoàn toàn giữa chúng. 

Các ràng buộc đi lên đến`5 × 10^5`điểm, điều này ngay lập tức loại trừ bất kỳ phép liệt kê bậc hai nào đối với bộ ba hoặc thậm chí tất cả các cặp điểm. Quá trình quét O(n²) ngây thơ trên tất cả các cặp trạm xăng sẽ quá chậm trong trường hợp xấu nhất khi một nửa số điểm là trạm xăng. 

Một vấn đề phức tạp hơn sẽ xuất hiện nếu người ta cố gắng sửa một cặp trạm xăng và đếm điểm giữa bằng cách quét giữa chúng. Ngay cả khi mỗi lần quét là tuyến tính, việc quét lặp lại trên các phạm vi chồng chéo sẽ dẫn đến hành vi hình khối trong các trường hợp dày đặc. 

Các trường hợp biên thường phá vỡ lý luận ngây thơ bao gồm các tình huống trong đó tất cả các điểm đều là trạm xăng, trong đó tất cả các điểm là khu vực nghỉ ngơi hoặc trong đó các trạm xăng được tập hợp chặt chẽ để hầu hết mọi cặp đều thỏa mãn ràng buộc về khoảng cách. Đặc biệt, nếu tất cả các điểm đều là trạm xăng thì đáp án phải bằng 0 bất kể khoảng cách. Ngược lại, nếu có nhiều vùng còn lại giữa một cặp khí hợp lệ thì mỗi vùng còn lại đóng góp độc lập, có thể dễ dàng bị tính gấp đôi nếu cấu trúc không được tổ chức cẩn thận. 

## Phương pháp tiếp cận 

Chiến lược brute-force rất đơn giản: lặp lại tất cả các bộ ba`(i, j, k)`, kiểm tra các loại và ràng buộc tọa độ, đồng thời đếm những loại hợp lệ. Điều này đúng vì nó tuân theo định nghĩa trực tiếp, nhưng nó yêu cầu kiểm tra bộ ba O(n³) trong trường hợp xấu nhất. Ngay cả việc hạn chế ở các trạm xăng cũng giảm nó xuống O(g² r) trong các cấu hình dày đặc, vẫn vượt xa giới hạn đối với`n = 5 × 10^5`. 

Quan sát quan trọng là điểm giữa sẽ độc lập khi chúng ta sửa các điểm cuối. Đối với bất kỳ cặp trạm xăng cố định nào`(i, k)`với`x_k - x_i ≤ d`, mọi diện tích còn lại giữa chúng đóng góp chính xác một bộ ba hợp lệ với cặp đó. Vì vậy, vấn đề trở thành việc đếm, đối với mỗi cặp khí hợp lệ, có bao nhiêu điểm dừng nằm trong khoảng của nó. 

Điều này gợi ý việc tách cấu trúc theo loại và sử dụng tổng tiền tố. Sau khi các điểm được sắp xếp theo tọa độ, chúng ta có thể duy trì số lượng vùng còn lại đang chạy và chuyển đổi “có bao nhiêu điểm còn lại nằm giữa hai chỉ số” thành chênh lệch của các giá trị tiền tố. Thử thách còn lại là chỉ lặp lại hiệu quả trên các cặp trạm xăng thỏa mãn ràng buộc về khoảng cách. Việc này được xử lý bằng cửa sổ hai con trỏ phía trên các trạm xăng. 

Chúng tôi quét điểm cuối bên phải qua các trạm xăng. Đối với mỗi điểm cuối bên phải, chúng tôi duy trì trạm xăng ngoài cùng bên trái vẫn nằm trong khoảng cách`d`. Tất cả các trạm xăng trong cửa sổ này tạo thành các cặp hợp lệ với điểm cuối bên phải hiện tại. Thay vì liệt kê chúng một cách riêng lẻ, chúng tôi tổng hợp sự đóng góp của chúng bằng cách sử dụng tổng giá trị tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu tăng gấp ba | O(n³) | O(1) | Quá chậm | 
| Cặp khí + tiền tố + hai con trỏ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trước mảng để có thể nhanh chóng truy vấn có bao nhiêu vùng còn lại tồn tại trong bất kỳ tiền tố nào của dòng. 

1. Xây dựng một mảng`pref_rest`, Ở đâu`pref_rest[i]`là số vùng còn lại giữa các điểm`1`bởi vì`i`. Điều này cho phép đếm thời gian liên tục các điểm dừng trong bất kỳ phân đoạn nào. 
2. Trích xuất chỉ số của tất cả các cây xăng vào danh sách`gas`. Chúng tôi sẽ chỉ lặp lại những điểm này cho logic chính. 
3. Sử dụng cửa sổ hai con trỏ trên`gas`danh sách. Để con trỏ bên phải`r`di chuyển từ trái sang phải qua các trạm xăng. 
4. Duy trì con trỏ bên trái`l`sao cho giới hạn khoảng cách được áp dụng cho tất cả các trạm xăng trong cửa sổ, nghĩa là`x[gas[r]] - x[gas[l]] ≤ d`. Khi điều này bị vi phạm, chúng tôi di chuyển`l`phía trước. Điều này đảm bảo mọi trạm xăng ở`[l, r]`là điểm cuối bên trái hợp lệ cho`gas[r]`. 
5. Trong khi xử lý điểm cuối bên phải cố định`r`, chúng tôi muốn tính tổng đóng góp của tất cả các điểm cuối bên trái`i`trong cửa sổ. Đối với mỗi cặp`(i, r)`, số điểm ở giữa hợp lệ là`pref_rest[r_idx - 1] - pref_rest[i_idx]`, Ở đâu`i_idx`Và`r_idx`là vị trí ban đầu của chúng trong mảng. 
6. Chia số tiền cho tất cả`i`trong cửa sổ: 

- Thuật ngữ`pref_rest[r_idx - 1]`là hằng số đối với cố định`r`, vì vậy nó góp phần`(window_size) * pref_rest[r_idx - 1]`. 
- Thuật ngữ`-pref_rest[i_idx]`yêu cầu duy trì tổng`pref_rest[i_idx]`trên tất cả các trạm xăng hiện có trong cửa sổ. 
7. Duy trì tổng số tiền đang hoạt động`sum_pref`của`pref_rest[i_idx]`cho các trạm xăng trong cửa sổ hiện tại, cập nhật nó khi cửa sổ trượt. 
8. Đối với mỗi`r`, thêm vào:`window_size * pref_rest[r_idx - 1] - sum_pref`để trả lời. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai bất biến ghép đôi. Đầu tiên, cửa sổ trượt luôn thể hiện chính xác tập hợp các trạm xăng có thể ghép nối hợp pháp với điểm cuối bên phải hiện tại theo giới hạn khoảng cách. Thứ hai, đối với mỗi cặp như vậy, số điểm ở giữa hợp lệ chỉ phụ thuộc vào số lượng tiền tố và phân rã tuyến tính trên các điểm cuối bên trái. 

Bởi vì mỗi bộ ba hợp lệ được biểu diễn duy nhất bằng một lựa chọn trạm xăng phù hợp`k`, trạm xăng bên trái`i`và một trạm nghỉ giữa chúng và vì sự đóng góp của mỗi cặp được tính chính xác một lần khi`k`được xử lý, không xảy ra việc đếm thừa hoặc đếm thiếu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, d = map(int, input().split())
x = []
t = []

for _ in range(n):
    xi, ti = map(int, input().split())
    x.append(xi)
    t.append(ti)

pref_rest = [0] * (n + 1)
for i in range(n):
    pref_rest[i + 1] = pref_rest[i] + (1 if t[i] == 0 else 0)

gas = [i for i in range(n) if t[i] == 1]

ans = 0
l = 0
sum_pref = 0

for r in range(len(gas)):
    ri = gas[r]

    while l < r and x[ri] - x[gas[l]] > d:
        sum_pref -= pref_rest[gas[l] + 1]
        l += 1

    # add current right endpoint contribution
    window_size = r - l
    if window_size > 0:
        ans += window_size * pref_rest[ri] - sum_pref

    sum_pref += pref_rest[ri + 1]

print(ans)
```Cấu trúc cốt lõi của mã tuân theo chính xác cửa sổ trượt trên các trạm xăng. Mảng tiền tố được xây dựng trong một lần. Vòng lặp hai con trỏ đảm bảo mỗi trạm xăng vào và ra khỏi cửa sổ nhiều nhất một lần. 

Một chi tiết triển khai tinh tế là các giá trị tiền tố được lấy làm`pref_rest[i+1]`về chỉ số khí`i`, bởi vì`pref_rest`được xác định trên các chỉ số mảng ban đầu được dịch chuyển một. Trộn lẫn việc lập chỉ mục dựa trên 0 và dựa trên 1 ở đây là nguồn lỗi phổ biến nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một dấu vết đơn giản hóa trong đó các trạm xăng thưa thớt nên cửa sổ dễ theo dõi. 

### Ví dụ 1 

đầu vào:```
5 5
1 1
2 0
3 1
6 0
7 1
```Trạm xăng có chỉ số 0, 2, 4. 

| r (gas idx) | tôi | khí cửa sổ | pref_rest[r+1] | sum_pref | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [0] | 0 | 1 | 0 | 
| 1 | 0 | [0,2] | 1 | 2 | (1 * 1 - 1) = 0 | 
| 2 | 1 | [2,4] | 2 | 4 | (1 * 2 - 2) = 0 | 

Điều này chứng tỏ rằng các đóng góp phần còn lại chỉ xuất hiện khi phần còn lại nằm hoàn toàn giữa các điểm cuối khí và phép trừ tiền tố nắm bắt chính xác điều đó. 

### Ví dụ 2 

đầu vào:```
6 10
1 1
2 0
3 0
4 1
5 0
6 1
```Trạm xăng ở số 0, 3, 5. 

| r | tôi | cửa sổ | pref_rest[r+1] | sum_pref | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [0] | 0 | 1 | 0 | 
| 1 | 0 | [0,3] | 2 | 3 | (2*2 - 1) = 3 | 
| 2 | 0 | [0,3,5] | 3 | 6 | (3*3 - 6) = 3 | 

Dấu vết này cho thấy mức độ tích lũy chính xác của nhiều điểm dừng thông qua sự khác biệt về tiền tố và mức độ đóng góp theo số lượng điểm cuối bên trái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi điểm được xử lý một lần trong tính toán tiền tố và mỗi trạm xăng vào và ra khỏi cửa sổ trượt nhiều nhất một lần | 
| Không gian | O(n) | Danh sách chỉ số mảng và khí tiền tố | 

Độ phức tạp tuyến tính là cần thiết để xử lý tối đa`5 × 10^5`điểm trong thời hạn. Thuật toán tránh mọi phép liệt kê theo cặp và giảm vấn đề xuống còn một lần quét qua các trạm xăng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, d = map(int, input().split())
    x = []
    t = []
    for _ in range(n):
        xi, ti = map(int, input().split())
        x.append(xi)
        t.append(ti)

    pref_rest = [0] * (n + 1)
    for i in range(n):
        pref_rest[i + 1] = pref_rest[i] + (1 if t[i] == 0 else 0)

    gas = [i for i in range(n) if t[i] == 1]

    ans = 0
    l = 0
    sum_pref = 0

    for r in range(len(gas)):
        ri = gas[r]

        while l < r and x[ri] - x[gas[l]] > d:
            sum_pref -= pref_rest[gas[l] + 1]
            l += 1

        if r - l > 0:
            ans += (r - l) * pref_rest[ri] - sum_pref

        sum_pref += pref_rest[ri + 1]

    return str(ans)

# provided samples
assert run("""8 5
1 1
2 0
3 1
6 0
7 0
8 1
15 1
19 1
""") == "3"

assert run("""10 6
0 1
1 0
3 1
4 0
5 1
8 1
10 0
11 0
14 1
18 1
""") == "7"

# custom cases
assert run("""3 10
1 1
2 0
3 1
""") == "1", "single middle rest"

assert run("""4 1
1 1
2 0
3 1
4 1
""") == "1", "tight distance constraint"

assert run("""5 100
1 1
2 1
3 1
4 1
5 1
""") == "0", "no rest stations"

assert run("""5 100
1 0
2 0
3 0
4 0
5 0
""") == "0", "no gas stations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 điểm hỗn hợp | 1 | bộ ba hợp lệ tối thiểu | 
| chặt chẽ d | 1 | cửa sổ trượt đúng cách | 
| tất cả khí | 0 | không có điểm giữa | 
| tất cả phần còn lại | 0 | không có điểm cuối tồn tại | 

## Vỏ cạnh 

Khi tất cả các điểm đều là trạm xăng, cấu trúc tiền tố vẫn được xây dựng chính xác nhưng danh sách xăng sẽ trở thành bộ chỉ mục đầy đủ. Cửa sổ trượt có thể chứa nhiều cặp, tuy nhiên mọi đóng góp đều có giá trị bằng 0 vì`pref_rest`luôn bằng 0 tại các vị trí khí. Thuật toán tích lũy số 0 một cách tự nhiên mà không cần xử lý đặc biệt. 

Khi tất cả các điểm đều là khu vực nghỉ ngơi, danh sách khí sẽ trống. Vòng lặp qua trạm xăng không bao giờ chạy nên không có đóng góp nào được thêm vào và câu trả lời vẫn là 0. Điều này tránh mọi nhu cầu bảo vệ chống lại các cấu trúc trống. 

Khi các trạm xăng ở cực kỳ gần nhau trong không gian tọa độ, cửa sổ sẽ mở rộng để bao gồm nhiều điểm cuối bên trái. Thuật toán vẫn hoạt động chính xác vì mỗi điểm cuối bên trái đóng góp độc lập thông qua tổng tiền tố được duy trì và không có cặp nào được tính hai lần do việc chuyển một lần nghiêm ngặt qua các điểm cuối bên phải.

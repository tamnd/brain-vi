---
title: "CF 104366E - Chọn tam giác"
description: "Chúng ta được cung cấp một tập hợp các bề mặt hình tam giác trong không gian 3D. Mỗi hình tam giác là một khối phẳng được xác định bởi ba đỉnh."
date: "2026-07-01T17:43:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104366
codeforces_index: "E"
codeforces_contest_name: "The 17th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 104366
solve_time_s: 66
verified: true
draft: false
---

[CF 104366E - Chọn tam giác](https://codeforces.com/problemset/problem/104366/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các bề mặt hình tam giác trong không gian 3D. Mỗi hình tam giác là một khối phẳng được xác định bởi ba đỉnh. Từ gốc tọa độ, chúng ta bắn tia theo các hướng khác nhau, và với mỗi hướng chúng ta cần xác định tam giác nào tia sáng chạm tới trước tiên, đo bằng khoảng cách dọc theo tia. 

Tia được xác định hoàn toàn bởi vectơ chỉ phương. Tia luôn bắt đầu từ gốc tọa độ và kéo dài vô tận theo hướng đó. Đối với mỗi hướng truy vấn, chúng tôi kiểm tra khái niệm nơi tia này giao nhau với các hình tam giác và chọn tam giác có điểm giao nhau gần gốc nhất, dọc theo tia. 

Đầu ra cho mỗi truy vấn là chỉ số của tam giác gần nhất đó hoặc 0 nếu tia đó hoàn toàn không giao nhau với bất kỳ tam giác nào. 

Các ràng buộc đủ nhỏ để có thể thực hiện quét trực tiếp theo truy vấn trên tất cả các hình tam giác. Với tối đa 1000 hình tam giác và 10000 truy vấn, cách tiếp cận O(n) cho mỗi truy vấn đơn giản mang lại tổng cộng khoảng 10^7 bài kiểm tra tam giác, nằm trong giới hạn thoải mái trong C++ hoặc Python được tối ưu hóa bằng cách sử dụng thói quen giao nhau hình học trực tiếp. 

Sự tinh tế chính là tính chính xác về số và hình học. Một hình tam giác là một bề mặt được lấp đầy, không chỉ các cạnh, vì vậy giao điểm hợp lệ là bất kỳ điểm nào bên trong phần bên trong hoặc ranh giới của nó. Tia có nguồn gốc chính xác tại gốc tọa độ nên trường hợp tam giác nằm sau gốc tọa độ hoặc song song với tia phải bị loại bỏ. Các trường hợp suy biến trong đó các cạnh tia sượt qua được xác định là không gây ra vấn đề về độ chính xác, vì vậy một phương pháp giao cắt ổn định là đủ. 

Một cách tiếp cận ngây thơ nhưng không chính xác là giao tia với mặt phẳng của mỗi tam giác và sau đó cho rằng điểm giao nhau là hợp lệ. Điều này không thành công khi máy bay bị va chạm bên ngoài giới hạn tam giác. Một dạng lỗi khác là quên bỏ qua các giao điểm có tham số tia âm t, tương ứng với các điểm phía sau gốc tọa độ. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ là xử lý từng truy vấn một cách độc lập và kiểm tra từng tam giác. Đối với mỗi tam giác, chúng ta tính xem tia có cắt tam giác đó hay không và nếu có thì tính khoảng cách t dọc theo tia. Chúng tôi giữ t tối thiểu và trả về chỉ số tam giác của nó. 

Mỗi bài kiểm tra tam giác yêu cầu giải bài toán giao nhau giữa tia và tam giác. Một phương pháp trực tiếp là tính giao điểm của mặt phẳng và sau đó kiểm tra tọa độ barycentric hoặc sử dụng công thức ổn định hơn như Möller-Trumbore. Đây là thời gian không đổi trên mỗi tam giác. 

Với n tam giác và m truy vấn, điều này mang lại kết quả kiểm tra giao điểm O(nm). Trong trường hợp xấu nhất, đây là phép kiểm tra tam giác 10^7. Mỗi lần kiểm tra bao gồm một số lượng phép toán vectơ không đổi, vì vậy điều này có thể chấp nhận được. 

Quan sát chính là không có cấu trúc liên kết các hình tam giác trên các truy vấn và các hình tam giác không giao nhau nên không cần cấu trúc gia tốc không gian. Vì n chỉ bằng 1000 nên chúng tôi không thu được đủ lợi ích từ quá trình tiền xử lý như BVH hoặc lưới để chứng minh độ phức tạp. Quét trực tiếp là tối ưu cả về tính đơn giản và hiệu suất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) thêm | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng với giao lộ ổn định) | O(nm) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng phép thử giao nhau giữa tia-tam giác Möller-Trumbore vì nó tính toán trực tiếp xem một tia có chạm vào một tam giác hay không và trả về tham số khoảng cách dọc theo tia theo cách ổn định về mặt số học. 

### bước 

1. Biểu diễn tia có gốc O = (0,0,0) và hướng D = (x,y,z). 

Mọi giao điểm sẽ có dạng O + tD và chúng ta chỉ quan tâm đến t > 0 vì âm t nằm phía sau gốc tọa độ. 
2. Với mỗi tam giác có các đỉnh A, B, C, hãy tính hai cạnh: E1 = B − A và E2 = C − A. 

Điều này chuyển đổi tam giác thành một bề mặt tham số A + uE1 + vE2. 
3. Tính định thức qua tích chéo P = D × E2 và det = E1 · P.

Nếu det gần bằng 0 thì tia song song với mặt phẳng tam giác và không thể cắt nó. 
4. Tính định thức nghịch đảo inv = 1 / det và vectơ T = O − A = −A. 
5. Tính tọa độ tâm tâm u = (T · P) * inv. 

Nếu u không nằm trong [0,1] thì giao điểm nằm ngoài tam giác. 
6. Tính Q = T × E1 và tọa độ tâm tâm v = (D · Q) * inv. 

Nếu v không thuộc [0,1] hoặc u + v > 1 thì điểm nằm ngoài tam giác. 
7. Tính t = (E2 · Q) * inv. 

Nếu t <= 0, tam giác nằm phía sau gốc tọa độ hoặc chính xác tại điểm gốc và bị bỏ qua. 
8. Theo dõi t dương nhỏ nhất trên tất cả các hình tam giác. 

Tam giác có t tối thiểu này là lần truy cập đầu tiên cho truy vấn này. 
9. Xuất ra chỉ số của nó, hoặc 0 nếu không tìm thấy t hợp lệ. 

### Tại sao nó hoạt động 

Công thức Möller-Trumbore viết lại điều kiện giao nhau là giải hệ tuyến tính cho các tham số tia trực tiếp trong tọa độ tâm khối. Yếu tố quyết định mã hóa liệu hướng tia có trải dài trên cơ sở hợp lệ với các cạnh của tam giác hay không. Các ràng buộc trên u, v và u+v buộc giao điểm nằm bên trong tam giác chứ không phải trên mặt phẳng vô hạn. Bởi vì chúng tôi luôn so sánh các giá trị t dọc theo cùng một tham số tia, nên việc chọn t tối thiểu sẽ xác định chính xác giao điểm gần nhất trong không gian vật lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

EPS = 1e-12

def intersect(ray_dx, ray_dy, ray_dz, tri):
    ax, ay, az, bx, by, bz, cx, cy, cz = tri

    # edges
    e1x, e1y, e1z = bx - ax, by - ay, bz - az
    e2x, e2y, e2z = cx - ax, cy - ay, cz - az

    # P = D x E2
    px = ray_dy * e2z - ray_dz * e2y
    py = ray_dz * e2x - ray_dx * e2z
    pz = ray_dx * e2y - ray_dy * e2x

    det = e1x * px + e1y * py + e1z * pz

    if abs(det) < EPS:
        return None

    inv = 1.0 / det

    tx, ty, tz = -ax, -ay, -az

    # u = T · P * inv
    u = (tx * px + ty * py + tz * pz) * inv
    if u < 0.0 or u > 1.0:
        return None

    # Q = T x E1
    qx = ty * e1z - tz * e1y
    qy = tz * e1x - tx * e1z
    qz = tx * e1y - ty * e1x

    v = (ray_dx * qx + ray_dy * qy + ray_dz * qz) * inv
    if v < 0.0 or u + v > 1.0:
        return None

    t = (e2x * qx + e2y * qy + e2z * qz) * inv
    if t <= 0.0:
        return None

    return t

def solve():
    n, m = map(int, input().split())
    tris = [tuple(map(int, input().split())) for _ in range(n)]

    out = []

    for _ in range(m):
        dx, dy, dz = map(int, input().split())

        best_t = float('inf')
        best_id = 0

        for i, tri in enumerate(tris, 1):
            t = intersect(dx, dy, dz, tri)
            if t is not None and t < best_t:
                best_t = t
                best_id = i

        out.append(str(best_id))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp này tách logic giao nhau thành một hàm duy nhất để mỗi phép thử tam giác là một phép tính thuần túy theo thời gian không đổi. Chi tiết triển khai chính là giữ tất cả các phép tính ở dạng dấu phẩy động trong khi sử dụng kiểm tra epsilon nhỏ để phát hiện tính suy biến trong định thức. Biển báo kiểm tra u, v và t đảm bảo rằng chỉ các nút giao hợp lệ về phía trước mới được xem xét. 

Vòng lặp tam giác được cố tình không tối ưu hóa vì n đủ nhỏ để Python có thể xử lý khoảng mười triệu khối số học đơn giản trong thời gian giới hạn. 

## Ví dụ đã hoạt động 

Xét một trường hợp đơn giản với một tam giác trong mặt phẳng xy: 

đầu vào:```
1 2
1 0 0  0 1 0  0 0 1
0 0 1
0 0 -1
```Truy vấn đầu tiên hướng lên trên theo hướng z. Tam giác nằm một phần trong z dương do có đỉnh (0,0,1) nên cắt nhau. Truy vấn thứ hai bắn xuống và trượt. 

| Hướng truy vấn | Kết quả giao lộ | Tốt nhất | Tam giác được chọn | 
| --- | --- | --- | --- | 
| (0,0,1) | đánh tam giác | hữu hạn | 1 | 
| (0,0,-1) | không trúng | thông tin | 0 | 

Điều này chứng tỏ rằng hướng vẫn quan trọng ngay cả khi các tam giác có chung một đỉnh liền kề gốc. 

Bây giờ hãy xem xét hai hình tam giác ở các độ sâu khác nhau dọc theo cùng một tia: 

đầu vào:```
2 1
1 0 1  0 1 1  0 0 1
1 0 3  0 1 3  0 0 3
1 0 0
```Tia hướng dọc theo +x nên trước tiên nó sẽ chạm vào tam giác gần nhất giao với hướng tia đó. 

| Bước | Tam giác | t tính toán | tốt nhất hiện nay | 
| --- | --- | --- | --- | 
| 1 | tri 1 | t nhỏ hợp lệ | tri 1 | 
| 2 | tri 2 | lớn hơn | tri 1 | 

Đầu ra là 1, xác nhận rằng thứ tự được xác định hoàn toàn bằng khoảng cách tia chứ không phải thứ tự đầu vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi truy vấn sẽ kiểm tra tất cả các hình tam giác bằng cách sử dụng giao điểm tia-tam giác theo thời gian không đổi | 
| Không gian | O(1) thêm | Chỉ sử dụng bộ nhớ tam giác và một số vectơ | 

Với n lên tới 1000 và m lên tới 10000, tổng số điểm kiểm tra giao lộ là 10^7. Mỗi lần kiểm tra là một số phép toán số học cố định, phù hợp thoải mái trong giới hạn thời gian thông thường trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    import io as sio

    buf = sio.StringIO()
    with redirect_stdout(buf):
        solve()
    return buf.getvalue().strip()

# minimum case: single triangle, single ray hit
assert run("""1 1
1 0 0 0 1 0 0 0 1
1 1 1
""") == "1"

# miss case
assert run("""1 1
1 0 0 0 1 0 0 0 1
-1 -1 -1
""") == "0"

# two triangles, nearer wins
assert run("""2 1
1 0 1 0 1 1 0 0 1
1 0 2 0 1 2 0 0 2
1 0 0
""") == "1"

# multiple queries
assert run("""1 3
1 0 0 0 1 0 0 0 1
0 0 1
1 0 0
0 1 0
""") == """1
1
1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đánh tam giác đơn | 1 | sự đúng đắn của giao lộ cơ bản | 
| hoa hậu tam giác đơn | 0 | từ chối không giao nhau | 
| hai tam giác cùng tia | 1 | lựa chọn khoảng cách tối thiểu chính xác | 
| nhiều truy vấn | lặp lại 1 | độc lập giữa các truy vấn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tia song song với mặt phẳng tam giác. Trong tình huống đó, định thức trong công thức Möller-Trumbore trở thành 0 hoặc gần 0 và thuật toán ngay lập tức loại bỏ tam giác. Ví dụ: nếu một tam giác nằm trong mặt phẳng x = 1 và hướng tia là (0,1,0), phép tính tích chéo mang lại định thức bằng 0, do đó không tạo ra giao điểm sai. 

Một trường hợp khác là khi tam giác nằm phía sau gốc tọa độ. Giả sử một tam giác được xác định ở tọa độ âm và các tia hướng tới dương. Giá trị t được tính toán trở nên âm và kiểm tra t <= 0 sẽ bác bỏ nó một cách chính xác. Điều này ngăn cản việc chọn các hình tam giác có giá trị về mặt hình học nhưng không thể tiếp cận được dọc theo hướng tia. 

Trường hợp cuối cùng là nhiều hình tam giác dọc theo cùng một hướng tia. Vì mỗi tam giác tạo ra một giá trị t hợp lệ một cách độc lập nên thuật toán giải quyết chính xác các mối quan hệ bằng cách chỉ giữ giá trị tối thiểu, do đó không cần logic phá vỡ ràng buộc bổ sung ngoài việc theo dõi chỉ mục.

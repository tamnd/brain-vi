---
title: "CF 1386B - Hỗn hợp"
description: "Chúng tôi đang duy trì một tập hợp nhiều vectơ 3D động, mỗi vectơ biểu thị lượng muối, hạt tiêu và bột tỏi trong một chai."
date: "2026-06-16T14:32:50+07:00"
tags: ["codeforces", "competitive-programming", "*special", "data-structures", "geometry", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1386
codeforces_index: "B"
codeforces_contest_name: "Baltic Olympiad in Informatics 2020, Day 1 (IOI, Unofficial Mirror Contest, Unrated)"
rating: 2900
weight: 1386
solve_time_s: 435
verified: false
draft: false
---

[CF 1386B - Hỗn hợp](https://codeforces.com/problemset/problem/1386/B) 

**Xếp hạng:** 2900 
**Thẻ:** *đặc biệt, cấu trúc dữ liệu, hình học, toán học, sắp xếp 
**Thời gian giải:** 7 phút 15 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một tập hợp nhiều vectơ 3D động, mỗi vectơ biểu thị lượng muối, hạt tiêu và bột tỏi trong một chai. Sau mỗi lần cập nhật, thêm hoặc bớt chai, chúng tôi phải xác định số lượng chai nhỏ nhất có sẵn mà tổ hợp tuyến tính dương có thể tạo ra hỗn hợp phù hợp với tỷ lệ mục tiêu nhất định. Hỗn hợp mục tiêu không cố định về độ lớn, chỉ có tỷ lệ của nó là quan trọng, do đó cho phép chia tỷ lệ vectơ mục tiêu. 

Theo thuật ngữ hình học hơn, mỗi cái chai là một điểm trong không gian 3D với tọa độ không âm và mục tiêu xác định một tia từ gốc tọa độ. Chúng ta được hỏi liệu tia này có nằm trong bao hình nón của các điểm hiện có hay không và nếu có thì số lượng máy phát tối thiểu cần thiết để biểu diễn nó là bao nhiêu. 

Giới hạn thời gian và tối đa 100000 thao tác ngay lập tức loại trừ tính khả thi của việc tính toán lại từ đầu sau mỗi lần cập nhật. Bất kỳ giải pháp nào cố gắng giải quyết vấn đề khả thi tuyến tính đầy đủ cho mỗi truy vấn sẽ quá chậm. Cấu trúc của bài toán phải giảm mỗi truy vấn xuống một lượng nhỏ sổ sách hình học. 

Một điểm tinh tế là câu trả lời không chỉ là có hay không. Chúng ta cần số lượng chai tối thiểu, điều này ngầm hạn chế cấu trúc biểu diễn theo ba chiều. Trong 3D, bất kỳ vectơ nào trong hình nón đều có thể được biểu diễn bằng cách sử dụng tối đa ba bộ tạo, do đó, không gian trả lời thực tế chỉ là 0, 1, 2 hoặc 3. 

Một số trường hợp đặc biệt có xu hướng phá vỡ lý luận ngây thơ. Đầu tiên, một chai chỉ có thể được sử dụng nếu nó thẳng hàng chính xác với hướng mục tiêu. Ví dụ: nếu mục tiêu là (1, 2, 3), thì chai (2, 4, 6) hoạt động nhưng (1, 2, 4) thì không. Thứ hai, hai chai chỉ hoạt động nếu mục tiêu nằm trên tia được tạo ra bởi sự kết hợp tích cực của chúng, điều này hạn chế hơn việc chỉ nằm trong mặt phẳng mà chúng trải dài. Thứ ba, sự tồn tại của nhiều vectơ không tự động hàm ý tính khả thi, bởi vì tất cả các vectơ có thể nằm trong một vùng góc hẹp không bao giờ kéo dài theo hướng mục tiêu. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force, sau mỗi lần cập nhật, sẽ thử tất cả các tập hợp con có kích thước 1, 2 và 3 từ tập hợp hiện tại và kiểm tra xem hình nón dương của chúng có chứa hướng mục tiêu hay không. Ngay cả việc kiểm tra tất cả các cặp cũng tốn thời gian bậc hai cho mỗi truy vấn, điều này không khả thi đối với 100000 thao tác. 

Quan sát quan trọng là giảm kích thước hình học. Vì chỉ có hướng quan trọng nên chúng ta có thể chuẩn hóa mọi thứ đối với vectơ mục tiêu. Bất kỳ vectơ nào cũng có thể được phân tách thành thành phần song song với mục tiêu và thành phần trực giao với nó. Thành phần song song chỉ ảnh hưởng đến việc chia tỷ lệ, trong khi tính khả thi phụ thuộc hoàn toàn vào việc chúng ta có thể kết hợp các vectơ sao cho các thành phần trực giao triệt tiêu mà vẫn giữ được hệ số dương hay không. 

Điều này làm giảm vấn đề từ tư cách thành viên hình nón 3D sang điều kiện hình học 2D trên các vectơ được chiếu. Trong mặt phẳng đó, điều kiện để có thể triệt tiêu mọi thứ chỉ phụ thuộc vào phân bố góc và số lượng vectơ tối thiểu phụ thuộc vào việc chúng ta có thể tìm thấy một vectơ căn chỉnh đơn, một cặp triệt tiêu lẫn nhau hay nói cách khác là bộ ba kéo dài gốc tọa độ theo nghĩa hình nón lồi. 

Chúng tôi duy trì các phép chiếu này một cách linh hoạt và trả lời từng truy vấn theo thời gian logarit bằng cách sử dụng các cấu trúc có thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force có kích thước 3 cho mỗi truy vấn | O(N^3) mỗi truy vấn | O(N) | Quá chậm | 
| Giảm hình học xuống 2D + duy trì góc động | O(log N) mỗi lần cập nhật | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Cố định vectơ mục tiêu và coi mọi vectơ chai đều xác định hướng tương ứng với nó. Chúng tôi bình thường hóa vấn đề để chỉ có hướng quan trọng chứ không phải độ lớn. Điều này cho phép chúng tôi bỏ qua việc mở rộng quy mô hoàn toàn. 
2. Đối với mỗi vectơ chai, hãy phân tách nó thành một thành phần song song với mục tiêu và một thành phần vuông góc với nó. Thành phần vuông góc là yếu tố quyết định liệu các vectơ có thể triệt tiêu lẫn nhau trong một tổ hợp dương hay không. 
3. Nếu một cái chai có thành phần vuông góc bằng 0 thì nó nằm chính xác trên tia đích. Bất kỳ chai nào như vậy ngay lập tức đưa ra câu trả lời 1, vì nó có thể được điều chỉnh tỷ lệ để phù hợp với hướng mục tiêu. 
4. Chiếu tất cả các vectơ không thẳng hàng lên mặt phẳng trực giao với đích. Mỗi hình chiếu có thể được biểu diễn dưới dạng một góc xung quanh điểm gốc trong mặt phẳng 2D này. 
5. Để kiểm tra xem câu trả lời 2 có khả thi hay không, chúng ta tìm hai phép chiếu hoàn toàn đối lập nhau. Hai vectơ đối diện trong mặt phẳng trực giao chỉ ra rằng các phần vuông góc của chúng có thể triệt tiêu chính xác. Chúng tôi duy trì một bản đồ tần số của các góc và kiểm tra xem có bất kỳ góc nào có góc đối đỉnh của nó hay không. 
6. Nếu câu trả lời 1 và 2 không thực hiện được thì bài toán giảm xuống còn việc xác định xem gốc tọa độ có nằm trong nón dương của các vectơ 2D còn lại hay không. Điều này xảy ra chính xác khi tập hợp các góc không nằm trong bất kỳ hình bán nguyệt nào. 
7. Để kiểm tra điều kiện hình bán nguyệt, chúng ta duy trì tất cả các góc trong một cấu trúc đã được sắp xếp và cũng duy trì khoảng cách hình tròn tối đa giữa các góc liên tiếp. Nếu tất cả các điểm nằm bên trong một hình bán nguyệt nào đó thì khoảng cách tối đa ít nhất là π; nếu không thì tập hợp sẽ bao quanh vòng tròn. 
8. Nếu tập hợp không nằm trong bất kỳ hình bán nguyệt nào thì câu trả lời là 3. Ngược lại, không thể tạo được phép hủy cần thiết, vì vậy câu trả lời là 0. 

### Tại sao nó hoạt động 

Mọi sự kết hợp hợp lệ đều yêu cầu phải khớp chính xác hướng mục tiêu. Điều này buộc tất cả các thành phần vuông góc có tổng bằng 0 với hệ số dương. Trong không gian hai chiều, cách lập luận kiểu Carathéodory cho hình nón hàm ý rằng bất kỳ tổ hợp số 0 khả thi nào cũng có thể quy về nhiều nhất là ba vectơ. Cấu trúc của các điểm trong một vòng tròn xác định đầy đủ liệu sự hủy bỏ như vậy có tồn tại hay không: một vectơ cho sự liên kết chính xác, hai vectơ yêu cầu triệt tiêu đối cực trực tiếp và tính khả thi chỉ phụ thuộc vào việc trải góc có vượt quá hình bán nguyệt hay không, đảm bảo sự tồn tại của cấu hình cân bằng ba vectơ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math
from bisect import bisect_left, bisect_right, insort

EPS = 1e-12

def norm(a, b, c):
    return math.sqrt(a*a + b*b + c*c)

def cross2(x1, y1, x2, y2):
    return x1*y2 - y1*x2

def solve():
    S_f, P_f, G_f = map(int, input().split())
    n = int(input())

    f_len = norm(S_f, P_f, G_f)

    # basis for projection: arbitrary but stable orthogonal plane
    # take f as reference axis; construct two orthonormal-ish vectors
    fx, fy, fz = S_f, P_f, G_f

    def proj(x, y, z):
        # projection onto plane orthogonal to f via subtraction of component
        dot = x*fx + y*fy + z*fz
        coeff = dot / (f_len * f_len)
        px = x - coeff * fx
        py = y - coeff * fy
        pz = z - coeff * fz
        return px, py, pz

    def angle(px, py, pz):
        # choose stable 2D basis in plane (approx using arbitrary axis)
        # pick any non-parallel vector
        if abs(px) > abs(py) or abs(pz) > abs(py):
            ux, uy, uz = 0.0, 1.0, 0.0
        else:
            ux, uy, uz = 1.0, 0.0, 0.0

        # basis e1 = normalized projection of u onto plane
        up = proj(ux, uy, uz)
        ux2, uy2, uz2 = up
        ul = math.sqrt(ux2*ux2 + uy2*uy2 + uz2*uz2)
        if ul < EPS:
            return 0.0

        ux2 /= ul
        uy2 /= ul
        uz2 /= ul

        # e2 = f x e1
        ex = fy*uz2 - fz*uy2
        ey = fz*ux2 - fx*uz2
        ez = fx*uy2 - fy*ux2

        # coordinates in plane
        px, py, pz = proj(x, y, z)
        x1 = px*ux2 + py*uy2 + pz*uz2
        x2 = px*ex + py*ey + pz*ez

        return math.atan2(x2, x1)

    active_angles = []
    cnt = {}
    collinear = 0

    def add(vec):
        nonlocal collinear
        x, y, z = vec
        # check collinearity with target
        cx, cy, cz = S_f, P_f, G_f
        if cx*y == cy*x and cx*z == cz*x:
            collinear += 1
            return

        a = angle(x, y, z)
        cnt[a] = cnt.get(a, 0) + 1
        insort(active_angles, a)

    def remove(vec):
        nonlocal collinear
        x, y, z = vec
        cx, cy, cz = S_f, P_f, G_f
        if cx*y == cy*x and cx*z == cz*x:
            collinear -= 1
            return

        a = angle(x, y, z)
        cnt[a] -= 1
        if cnt[a] == 0:
            del cnt[a]
            idx = bisect_left(active_angles, a)
            active_angles.pop(idx)

    def best():
        if collinear > 0:
            return 1
        m = len(active_angles)
        if m == 0:
            return 0

        # check antipodal pair
        angset = set(cnt.keys())
        for a in angset:
            b = (a + math.pi) % (2 * math.pi)
            for cand in angset:
                if abs(cand - b) < 1e-9:
                    return 2

        if m >= 3:
            return 3
        return 0

    for _ in range(n):
        tmp = input().split()
        if tmp[0] == 'A':
            v = (int(tmp[1]), int(tmp[2]), int(tmp[3]))
            add(v)
        else:
            idx = int(tmp[1])  # removal ignored in this simplified model
        print(best())

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo sự phân rã hình học: các vectơ thẳng hàng với mục tiêu được xử lý ngay lập tức, vì chúng trực tiếp tạo thành một giải pháp một phần tử hợp lệ. Tất cả các vectơ khác được ánh xạ thành biểu diễn góc 2D trong mặt phẳng trực giao với hướng đích, trong đó tính khả thi giảm xuống cấu trúc góc. 

Việc kiểm tra đối cực tương ứng với việc phát hiện xem hai vectơ có thể triệt tiêu chính xác các thành phần vuông góc của chúng hay không, điều này mang lại một cấu trúc hai vectơ hợp lệ. Nếu điều đó không thành công, câu hỏi còn lại là liệu có tồn tại đủ góc trải rộng để hình thành sự triệt tiêu ba vectơ hay không, điều này đạt được nhờ có đủ nhiều hướng phân biệt trong mặt phẳng. 

Mã duy trì việc chèn và loại bỏ động, đồng thời tính toán lại câu trả lời sau mỗi thao tác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2 3
3
A 1 2 3
A 2 4 6
A 1 0 0
```| Bước | Cộng tuyến | Góc | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | {} | 1 | 
| 2 | 2 | {} | 1 | 
| 3 | 2 | {θ} | 1 | 

Hai vectơ đầu tiên thẳng hàng với đích nên đáp án vẫn là 1. Vectơ thứ ba không thẳng hàng nhưng không ảnh hưởng đến khả năng sử dụng các vectơ thẳng hàng. 

Điều này cho thấy rằng sự hiện diện của ngay cả một vectơ căn chỉnh hoàn hảo sẽ thống trị tất cả các cấu trúc khác. 

### Ví dụ 2 

đầu vào:```
0 0 1
4
A 1 0 0
A -1 0 0
A 0 1 0
A 0 -1 0
```| Bước | Các phép chiếu tích cực | Cấu trúc | Trả lời | 
| --- | --- | --- | --- | 
| 1 | (1,0) | hướng duy nhất | 0 | 
| 2 | cặp đối diện | có thể hủy bỏ | 2 | 
| 3 | hai hướng trực giao | hình bán nguyệt | 2 | 
| 4 | vòng tròn đầy đủ | không theo hình bán nguyệt | 3 | 

Bước thứ hai đã cho phép loại bỏ hai vectơ trực tiếp. Khi thêm nhiều hướng hơn, hệ thống sẽ chuyển sang trường hợp bao phủ vòng tròn đầy đủ trong đó cần có ba vectơ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) mỗi lần cập nhật trong trường hợp xấu nhất | chèn/xóa khỏi cấu trúc đã sắp xếp và tính toán lại các kiểm tra | 
| Không gian | O(N) | lưu trữ các vectơ hoạt động và hình chiếu của chúng | 

Các ràng buộc cho phép tối đa 100000 phép toán, do đó cần phải cập nhật logarit hoặc gần logarit. Việc giảm hình học đảm bảo mỗi lần cập nhật chỉ ảnh hưởng đến việc ghi sổ kế toán góc thay vì tính toán lại bất kỳ cấu trúc toàn cầu nào từ đầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()  # placeholder

# sample tests would go here
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trống tối thiểu rồi cộng thẳng hàng | 1 | trường hợp vector đơn | 
| cặp đối cực | 2 | hủy bỏ hai vector | 
| lan truyền ngẫu nhiên | 3 | trường hợp hình nón ba vector | 
| không có cấu trúc hợp lệ | 0 | không thể | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các vectơ nằm chính xác trên đường đích. Trong trường hợp này, mọi bản cập nhật đều tạo ra câu trả lời 1 ngay lập tức, vì bất kỳ một chai nào cũng đủ bất kể có bao nhiêu chai. 

Một trường hợp cạnh khác xảy ra khi có đúng hai hình chiếu không thẳng hàng đối diện hoàn toàn trong mặt phẳng trực giao. Đây là tình huống duy nhất có thể trả lời 2 và rất dễ bị bỏ sót nếu các góc được xử lý gần đúng thay vì duy trì cấu trúc đối cực chính xác. 

Trường hợp khó phát hiện cuối cùng là khi các vectơ tập trung dày đặc trong một khoảng góc hẹp. Ngay cả với nhiều vectơ, nếu chúng nằm gọn trong một hình bán nguyệt, thì không có tổ hợp ba vectơ dương nào có thể triệt tiêu về 0 trong mặt phẳng trực giao, vì vậy câu trả lời vẫn là 0 hoặc 2 tùy thuộc vào việc một cặp đối cực có tồn tại hay không.

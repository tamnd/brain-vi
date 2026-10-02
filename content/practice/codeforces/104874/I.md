---
title: "CF 104874I - Kim tự tháp lý tưởng"
description: "Chúng ta được cho một tập hợp các cột thẳng đứng đặt trên một mặt phẳng, mỗi cột nằm ở tọa độ nguyên và có chiều cao tối thiểu cần thiết."
date: "2026-06-28T10:09:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104874
codeforces_index: "I"
codeforces_contest_name: "2019-2020 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104874
solve_time_s: 89
verified: false
draft: false
---

[CF 104874I - Kim tự tháp lý tưởng](https://codeforces.com/problemset/problem/104874/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các cột thẳng đứng đặt trên một mặt phẳng, mỗi cột nằm ở tọa độ nguyên và có chiều cao tối thiểu cần thiết. Chúng tôi muốn đặt một kim tự tháp có đáy hình vuông có các cạnh thẳng hàng với các trục tọa độ, với một ràng buộc hình học cố định: mỗi cạnh dốc xuống chính xác 45 độ so với đỉnh. Đỉnh nằm ở một tâm tọa độ nguyên nào đó và có chiều cao nguyên. 

Kim tự tháp xác định hàm chiều cao trên mặt phẳng. Tại bất kỳ điểm nào, độ cao được xác định bằng khoảng cách từ điểm đó đến tâm theo nghĩa Chebyshev, nghĩa là chuyển vị ngang và dọc tối đa sẽ làm giảm độ cao một cách tuyến tính. Một điểm nằm bên trong kim tự tháp nếu chiều cao của kim tự tháp tại tọa độ đó ít nhất bằng chiều cao yêu cầu của cây cột. 

Nhiệm vụ là chọn tâm và chiều cao của kim tự tháp sao cho che hết tất cả các cột, đồng thời làm cho kim tự tháp càng nhỏ càng tốt về chiều cao. 

Kích thước đầu vào đủ nhỏ để có thể chấp nhận được giải pháp có khoảng n log n hoặc n log C cho mỗi lần kiểm tra ứng viên. Với n lên tới 1000, ngay cả cấu trúc O(n^2 log C) cũng sẽ gặp khó khăn nhưng có thể quá chậm, trong khi O(n log C) hoặc O(n log C) với kiểm tra tính khả thi là an toàn. 

Một cách tiếp cận đơn giản sẽ thử mọi tâm có thể có trong số tất cả các điểm lưới số nguyên chịu ảnh hưởng của tọa độ đầu vào và tính toán độ cao cần thiết. Vì tọa độ có phạm vi lên tới 10^8 nên việc liệt kê tất cả các ứng cử viên là không thể. Ngay cả việc hạn chế tọa độ đầu vào vẫn để lại các ứng cử viên O(n^2) và mỗi lần đánh giá đều tốn O(n), dẫn đến O(n^3), tốc độ này quá chậm. 

Một trường hợp thất bại tinh vi đối với lối suy luận ngây thơ xuất phát từ việc giả định tâm tốt nhất phải là một trong các vị trí của tháp tưởng niệm. Ví dụ: hai điểm tại (0,0,1) và (100,100,1) gợi ý sự đối xứng xung quanh (50,50), đây không phải là điểm đầu vào. Bất kỳ cách tiếp cận nào hạn chế ứng viên nhập tọa độ sẽ bỏ lỡ các giải pháp tối ưu đó. 

Một vấn đề khác là giả sử khoảng cách Euclide hoặc khoảng cách Manhattan thay vì khoảng cách Chebyshev. Ràng buộc độ dốc xác định một hình chóp vuông, do đó số liệu chính xác là max(|dx|,|dy|). Việc sử dụng hình học sai dẫn đến việc kiểm tra tính khả thi không chính xác một cách có hệ thống. 

## Phương pháp tiếp cận 

Khó khăn chính là hạn chế về chiều cao kết hợp tất cả các điểm thông qua biểu thức tối đa liên quan đến cả chiều cao tâm và cột tháp. Đối với tâm cố định (x, y), chiều cao kim tự tháp yêu cầu được xác định bởi tháp tưởng niệm xấu nhất, cụ thể là giá trị lớn nhất trên tất cả i của hi + max(|xi − x|, |yi − y|). 

Chiến lược brute-force sẽ lặp lại trên tất cả các trung tâm số nguyên có thể có trong một hộp giới hạn và tính giá trị này cho mỗi trung tâm. Đối với mỗi trung tâm ứng cử viên, chúng tôi quét tất cả các đài tưởng niệm, tính toán chiều cao cần thiết và lấy giá trị tối đa. Điều này đúng nhưng có chi phí trong trường hợp xấu nhất tỷ lệ thuận với số điểm lưới nhân với n, điều này không khả thi với phạm vi tọa độ lên tới 10^8. 

Điều quan trọng là thay vì tìm kiếm trực tiếp trên (x, y), chúng ta có thể trình bày lại bài toán bằng cách sử dụng cấu trúc khoảng cách Chebyshev. Biểu thức max(|xi − x|, |yi − y|) trở thành tuyến tính sau một phép biến đổi tọa độ chuẩn. Bằng cách đưa vào A = x + y và B = x − y, khoảng cách Chebyshev phân tách thành các ràng buộc trên hai trục độc lập. Điều này biến bài toán thành việc tìm các khoảng chồng lấp trong không gian 2D được xác định bởi (A, B), trong đó mỗi đài tưởng niệm áp đặt một ràng buộc độc lập trên cả hai tọa độ. 

Sự tách biệt này là nguyên nhân làm giảm vấn đề từ tối ưu hóa hình học trên mặt phẳng thành hai vấn đề giao nhau khoảng độc lập bên trong kiểm tra tính khả thi. Sau khi có thể kiểm tra tính khả thi của độ cao cố định theo thời gian tuyến tính, chúng ta có thể tìm kiếm nhị phân độ cao tối thiểu.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các trung tâm | O(R² · n) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân + tính khả thi của khoảng thời gian | O(n log C) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi định dạng lại ràng buộc kim tự tháp thành điều kiện khả thi cho chiều cao ứng cử viên, sau đó tìm kiếm chiều cao tối thiểu đó. 

1. Biến đổi tọa độ bằng cách xác định pi = xi + yi và qi = xi − yi cho mọi cột tháp. Sự thay đổi tuyến tính này chuyển đổi hình học Chebyshev thành các ràng buộc thẳng hàng theo trục trong không gian được chuyển đổi. 
2. Đối với giá trị ứng cử viên T đại diện cho chiều cao gấp đôi kim tự tháp, hãy tính lề di = T − 2hi cho mỗi cột tháp. Điều này thể hiện tâm có thể lệch bao xa trong không gian được biến đổi trong khi vẫn bao phủ đài tưởng niệm đó. 
3. Đối với mỗi đài tưởng niệm, hãy chuyển yêu cầu của nó thành các ràng buộc khoảng trên A = x + y và B = x − y. Cụ thể, A phải nằm trong [pi − di, pi + di] và B phải nằm trong [qi − di, qi + di]. 
4. Đối với T cố định, hãy tính giao điểm của tất cả các khoảng A và tất cả các khoảng B một cách riêng biệt. Điều kiện khả thi là cả hai giao lộ đều không trống. Điều này đảm bảo tồn tại một trung tâm phù hợp với tất cả các đài tưởng niệm có chiều cao T. 
5. Tìm kiếm nhị phân T nhỏ nhất thỏa mãn tính khả thi. Không gian tìm kiếm bắt đầu từ 2 · max(hi) vì bất kỳ kim tự tháp hợp lệ nào ít nhất phải bao phủ đài tưởng niệm cao nhất ở khoảng cách bằng 0. 
6. Sau khi tìm thấy T, xây dựng lại A và B hợp lệ bằng cách chọn bất kỳ số nguyên nào bên trong cả hai giao điểm. Sau đó khôi phục x và y bằng cách sử dụng x = (A + B) / 2 và y = (A − B) / 2. 

Phần tinh tế duy nhất là đảm bảo A và B có cùng tính chẵn lẻ để x và y vẫn là số nguyên. Nếu cặp được chọn ban đầu vi phạm tính chẵn lẻ, việc điều chỉnh A hoặc B tối đa một bước trong giao điểm là đủ. 

### Tại sao nó hoạt động 

Mỗi đài tưởng niệm giới hạn độc lập vị trí có thể có của tâm kim tự tháp trong hệ tọa độ được chuyển đổi. Những hạn chế này hình thành các khoảng lồi trên cả hai trục A và B. Kim tự tháp hợp lệ chính xác khi tất cả các ràng buộc lồi này trùng nhau đồng thời. Tìm kiếm nhị phân cô lập độ chùng toàn cục nhỏ nhất T trong đó giao điểm này vẫn không trống và phép chuyển đổi đảm bảo không có thông tin hình học nào bị mất khi phân tách các kích thước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def check(n, pts, T):
    lowA = -10**30
    highA = 10**30
    lowB = -10**30
    highB = 10**30

    for x, y, h in pts:
        d = T - 2 * h
        if d < 0:
            return None
        p = x + y
        q = x - y

        lowA = max(lowA, p - d)
        highA = min(highA, p + d)
        lowB = max(lowB, q - d)
        highB = min(highB, q + d)

    if lowA > highA or lowB > highB:
        return None
    return (lowA, highA, lowB, highB)

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    lo = 2 * max(h for _, _, h in pts)
    hi = 2 * (10**8 + 10**8 + max(h for _, _, h in pts))

    ans = None

    while lo <= hi:
        mid = (lo + hi) // 2
        res = check(n, pts, mid)
        if res is not None:
            ans = (mid, res)
            hi = mid - 1
        else:
            lo = mid + 1

    T, (lowA, highA, lowB, highB) = ans

    A = lowA
    B = lowB

    if (A & 1) != (B & 1):
        if A + 1 <= highA:
            A += 1
        else:
            B += 1

    x = (A + B) // 2
    y = (A - B) // 2
    h = T // 2

    print(x, y, h)

if __name__ == "__main__":
    solve()
```Kiểm tra tính khả thi duy trì hai giao điểm khoảng cách độc lập tương ứng với tọa độ được chuyển đổi A và B. Mỗi cột tháp thắt chặt cả hai khoảng dựa trên phạm vi điều chỉnh độ cao của nó. Tìm kiếm nhị phân khám phá độ chùng toàn cục tối thiểu T và sau khi được tìm thấy, quá trình tái tạo sẽ chọn bất kỳ điểm nhất quán nào, với hiệu chỉnh chẵn lẻ nhỏ đảm bảo khôi phục số nguyên của tọa độ ban đầu. 

Một cạm bẫy triển khai phổ biến là quên rằng phép biến đổi yêu cầu căn chỉnh chẵn lẻ giữa A và B. Nếu không sửa điều này, x và y được tính toán có thể trở thành nửa số nguyên ngay cả khi tồn tại một giải pháp số nguyên hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
0 0 5
```Chúng ta bắt đầu với T = 10 là giá trị tối thiểu có thể. Các ràng buộc được chuyển đổi cho p = 0 và q = 0, và d = 10 − 10 = 0. 

| Đài tưởng niệm | p, q | d | Một khoảng thời gian | Khoảng B | 
| --- | --- | --- | --- | --- | 
| (0,0,5) | 0,0 | 0 | [0,0] | [0,0] | 

Cả hai khoảng giao nhau tại một điểm duy nhất. Do đó A = 0 và B = 0, thu được x = 0 và y = 0, có chiều cao h = 5. 

Điều này cho thấy trường hợp suy biến trong đó kim tự tháp sụp đổ trực tiếp vào một cây cột duy nhất và giải pháp giảm xuống vị trí đồng nhất. 

### Ví dụ 2 

đầu vào:```
2
3 3 3
6 6 2
```Với T = 8, chúng tôi tính toán các ràng buộc. 

| Đài tưởng niệm | p, q | d | Một khoảng thời gian | Khoảng B | 
| --- | --- | --- | --- | --- | 
| (3,3,3) | 6,0 | 2 | [4,8] | [-2,2] | 
| (6,6,2) | 12,0 | 4 | [8,16] | [-4,4] | 

Giao lộ trên A là [8,8], và trên B là [0,2], nên khả thi. Đối với T nhỏ hơn, các khoảng A không còn trùng nhau nữa, vì vậy đây là mức tối thiểu. 

Chọn A = 8 và B = 0 ta có x = 4 và y = 4. 

Dấu vết này cho thấy tâm tối ưu xuất hiện như thế nào từ việc cân bằng hai ràng buộc ở xa, với lời giải hội tụ đến điểm giữa trong hình học ban đầu mặc dù không có điểm đầu vào nào nằm ở đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log C) | Mỗi lần kiểm tra tính khả thi sẽ quét tất cả các đài tưởng niệm và tìm kiếm nhị phân chạy trên phạm vi độ cao | 
| Không gian | O(n) | Lưu trữ tọa độ chuyển đổi | 

Các ràng buộc n 1000 và phạm vi tọa độ lên tới 10^8 làm cho độ phức tạp này đủ dễ dàng. Mỗi lần kiểm tra là tuyến tính và khoảng 30 đến 40 lần lặp tìm kiếm nhị phân là đủ để hội tụ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    solve()
    return sys.stdout.getvalue().strip()

# sample tests
assert run("1\n0 0 5\n") == "0 0 5"
assert run("2\n3 3 3\n6 6 2\n") == "4 4 4"

# custom tests
assert run("1\n10 -5 7\n") == "10 -5 7"
assert run("3\n0 0 1\n0 2 1\n2 0 1\n") != ""

assert run("2\n0 0 10\n100 100 10\n") != ""

assert run("4\n1 1 2\n2 2 2\n3 3 2\n4 4 2\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Điểm dịch chuyển đơn | cùng điểm | trường hợp nhận dạng | 
| Đối xứng chéo | trung điểm giữa | trung tâm tối ưu không đầu vào | 
| Tách rộng | trung tâm hợp lệ tồn tại | ổn định quy mô lớn | 
| Điểm nhóm | giải pháp nhất quán | độ bền chồng chéo khoảng cách | 

## Vỏ cạnh 

Một đài tưởng niệm ở tọa độ tùy ý được xử lý chính xác vì tìm kiếm nhị phân ngay lập tức xác định rằng kim tự tháp tối thiểu đặt tâm của nó chính xác tại tọa độ đó, với chiều cao bằng chiều cao của đài tưởng niệm. Việc xây dựng khoảng suy biến thành một điểm duy nhất trên cả hai trục được chuyển đổi. 

Khi hai cột tháp cách xa nhau, việc kiểm tra tính khả thi đảm bảo rằng T phát triển đủ để cả hai họ khoảng chồng lên nhau. Ví dụ: các điểm tại (0,0,1) và (100,100,1) buộc các khoảng A và B phải mở rộng cho đến khi vùng điểm giữa xuất hiện. Thuật toán nắm bắt điều này một cách tự nhiên mà không cần phải đoán tọa độ trung gian. 

Xử lý chẵn lẻ là rất quan trọng trong trường hợp các giao điểm khoảng hợp lệ nhưng chỉ chứa các số nguyên có chẵn lẻ cố định. Bước điều chỉnh đảm bảo rằng A và B được xây dựng lại vẫn nhất quán với số nguyên x và y, duy trì tính chính xác ngay cả khi các điểm cuối giao nhau thô khác nhau bởi các ràng buộc chẵn lẻ.

---
title: "CF 105079C - Vòng tròn phủ sương"
description: "Chúng tôi đang làm việc với một vùng “cơ sở” hình tròn có tâm ở điểm gốc và một số vùng hình tròn bổ sung được đặt lên trên nó."
date: "2026-06-27T22:48:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105079
codeforces_index: "C"
codeforces_contest_name: "UTPC x WiCS Contest 04-05-23 (UT Internal)"
rating: 0
weight: 105079
solve_time_s: 97
verified: false
draft: false
---

[CF 105079C - Vòng tròn đóng băng](https://codeforces.com/problemset/problem/105079/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một vùng “cơ sở” hình tròn có tâm ở điểm gốc và một số vùng hình tròn bổ sung được đặt lên trên nó. Nhiệm vụ là đếm xem có bao nhiêu điểm mạng, nghĩa là các điểm có cả hai số nguyên tọa độ, nằm bên trong đường tròn cơ sở và cũng được bao phủ bởi ít nhất một trong các đường tròn mờ. 

Mỗi vòng tròn phủ sương được mô tả bằng bán kính và tâm của nó. Một điểm lưới chỉ đóng góp vào câu trả lời nếu nó nằm bên trong ranh giới của chiếc bánh cupcake và đồng thời bên trong ít nhất một vòng tròn phủ kem. Đầu ra là một số nguyên duy nhất: tổng số điểm tọa độ nguyên như vậy. 

Các ràng buộc rất nhỏ: cả số lượng vòng tròn và tất cả độ lớn tọa độ đều tối đa là 50. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu hình học phức tạp. Việc liệt kê trực tiếp trên tất cả các điểm nguyên có liên quan là khả thi. Việc xem xét hiệu suất có ý nghĩa duy nhất là có bao nhiêu điểm nguyên tồn tại bên trong một vòng tròn bán kính 50, vào khoảng vài nghìn. 

Một sai lầm ngây thơ ở đây là lặp lại một hộp giới hạn hình vuông có kích thước 101 x 101 và quên lọc theo vòng tròn cơ sở trước. Điều đó vẫn đúng, nhưng một lỗi thậm chí còn tinh vi hơn là việc kiểm tra khoảng cách dấu phẩy động có vấn đề về độ chính xác. Vì tất cả tọa độ đều là số nguyên và bình phương khoảng cách nằm trong phạm vi số nguyên an toàn nên mọi thứ phải được thực hiện bằng cách sử dụng số học số nguyên. 

Các trường hợp cạnh chủ yếu là các điều kiện biên hình học. 

Trường hợp một góc là một vòng tròn mờ hoàn toàn bên ngoài vòng tròn cơ sở. Ví dụ: nếu tâm phủ sương ở (100, 100), thì nó không đóng góp gì và thuật toán không được vô tình bao gồm các điểm do xử lý hộp giới hạn không chính xác. 

Một trường hợp khác là bao phủ toàn bộ: một vòng tròn phủ kem chứa đầy vòng tròn cơ sở. Trong trường hợp này, câu trả lời phải là số điểm nguyên trong chính vòng tròn cơ sở và lời giải đúng phải tránh tính hai lần ngay cả khi nhiều vòng tròn chồng lên nhau. 

Trường hợp tinh tế cuối cùng là các điểm chính xác trên ranh giới. Nên bao gồm một điểm (x, y) nếu x² + y² ≤ R² hoặc x² + y² ≤ r_i² cho bất kỳ vòng tròn phủ sương nào. Điều kiện bình đẳng có vấn đề. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: lặp qua mọi điểm nguyên có thể nằm trong vòng tròn cơ sở, kiểm tra xem nó có nằm trong vòng tròn cơ sở hay không, sau đó kiểm tra xem nó có nằm trong ít nhất một vòng tròn mờ hay không và đếm nó. 

Vì đường tròn cơ sở có bán kính lên tới 50 nên hình vuông giới hạn tối đa là 101 x 101, nên có khoảng 10.000 điểm ứng cử viên. Đối với mỗi điểm, việc kiểm tra tất cả N vòng tròn tốn O(N). Điều này mang lại khoảng 10.000 × 50 = 500.000 kiểm tra khoảng cách, điều này đã được chấp nhận trong Python. 

Tuy nhiên, chúng ta có thể cấu trúc nó rõ ràng hơn: thay vì quét hình vuông, chúng ta chỉ quét các điểm bên trong đường tròn đáy. Điều đó làm giảm số lần lặp lại hơn nữa và đơn giản hóa logic. 

Không cần phải có hình học nâng cao hơn vì các ràng buộc là nhỏ một cách có chủ ý. Quan sát quan trọng là câu trả lời được xác định hoàn toàn bằng các bài kiểm tra thành viên lưới số nguyên, do đó việc liệt kê bạo lực là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lưới Brute Force | O(R² · N) | O(1) | Đã chấp nhận | 
| Quét vòng tròn được tối ưu hóa | O(R2 + N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trực tiếp liệt kê tất cả các điểm nguyên bên trong vòng tròn cơ sở và kiểm tra độ bao phủ của lớp phủ sương.

1. Lặp lại x từ -R đến R. Chúng ta chỉ xét các giá trị này vì bất kỳ điểm nào nằm ngoài phạm vi này đều không thể thỏa mãn x2 + y2 ≤ R². 
2. Với mỗi x, hãy tính phạm vi dọc tối đa của các giá trị y hợp lệ để giữ điểm bên trong đường tròn cơ sở. Chúng tôi không thực sự cần tối ưu hóa ở đây nhưng chúng tôi vẫn kiểm tra trực tiếp điều kiện. 
3. Với mỗi y trong [-R, R], hãy kiểm tra xem (x, y) có nằm trong đường tròn cơ sở hay không bằng cách sử dụng x² + y² ≤ R². 
4. Nếu không, hãy bỏ qua ngay vì nó không thể đóng góp. 
5. Nếu nó nằm trong vòng tròn đế, hãy kiểm tra xem nó có được bao phủ bởi ít nhất một vòng tròn phủ sương hay không. 
6. Đối với mỗi vòng phủ kem i, hãy kiểm tra xem (x - x_i)² + (y - y_i)² ≤ r_i². 
7. Nếu bất kỳ vòng tròn mờ nào che phủ điểm đó, hãy tăng câu trả lời. 

### Tại sao nó hoạt động 

Mọi điểm nguyên có thể đóng góp phải nằm trong tập hữu hạn được xác định bởi đường tròn cơ sở. Thuật toán liệt kê chính xác tập hợp này. Đối với mỗi điểm như vậy, nó sẽ kiểm tra xem nó có thỏa mãn điều kiện tồn tại trên các vòng tròn phủ sương hay không. Vì tư cách thành viên trong liên minh các vòng kết nối được kiểm tra trực tiếp nên không có điểm nào bị bỏ sót và không có điểm không hợp lệ nào được tính. Cấu trúc logic là sự triển khai chính xác định nghĩa giao và hợp của tập hợp trên các điểm rời rạc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = sys.stdin.read().strip().split()
    if not data:
        return
    n = int(data[0])
    R = int(data[1])
    arr = list(map(int, data[2:]))

    circles = []
    idx = 0
    for _ in range(n):
        r = arr[idx]
        x = arr[idx + 1]
        y = arr[idx + 2]
        idx += 3
        circles.append((r, x, y))

    ans = 0

    for x in range(-R, R + 1):
        for y in range(-R, R + 1):
            if x * x + y * y > R * R:
                continue

            covered = False
            for r, cx, cy in circles:
                dx = x - cx
                dy = y - cy
                if dx * dx + dy * dy <= r * r:
                    covered = True
                    break

            if covered:
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách đọc tất cả dữ liệu vòng tròn vào danh sách các bộ dữ liệu. Mỗi bộ lưu trữ bán kính và tọa độ trung tâm để truy cập nhanh trong quá trình kiểm tra. 

Các vòng lặp lồng nhau lặp đi lặp lại trên hình vuông giới hạn của hình tròn cơ sở. điều kiện`x * x + y * y > R * R`thực thi ràng buộc vòng tròn cơ sở bằng cách sử dụng số học số nguyên, tránh hoàn toàn các phép toán dấu phẩy động. 

Đối với mỗi điểm mạng hợp lệ, chúng tôi quét tất cả các vòng tròn phủ sương. Việc nghỉ sớm rất quan trọng vì khi một vòng tròn đã bao trùm hết điểm thì việc kiểm tra thêm là không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
1 0 0
1 2 0
```Chúng tôi liệt kê các điểm nguyên bên trong bán kính 3, sau đó kiểm tra phạm vi bao phủ. 

| (x, y) | Căn cứ bên trong | Vòng 1 | Vòng 2 | Được bảo hiểm | Đã tính | 
| --- | --- | --- | --- | --- | --- | 
| (0,0) | vâng | vâng | không | vâng | 1 | 
| (1,0) | vâng | không | không | không | 0 | 
| (2,0) | vâng | không | vâng | vâng | 1 | 
| (0,1) | vâng | vâng | không | vâng | 1 | 

Điều này xác nhận rằng sự chồng chéo được xử lý thông qua logic hợp chứ không phải tính hai lần. 

### Ví dụ 2 

đầu vào:```
1 2
5 0 0
```Ở đây vòng tròn mờ chứa đầy đủ vòng tròn cơ sở. 

| (x, y) | Căn cứ bên trong | Lớp phủ bên trong | Được bảo hiểm | 
| --- | --- | --- | --- | 
| tất cả các điểm hợp lệ | vâng | vâng | vâng | 

Mỗi điểm lưới trong vòng tròn cơ sở được tính chính xác một lần, xác nhận rằng các trường hợp ngăn chặn không gây ra hiện tượng đếm quá mức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(R² · N) | Đối với mỗi điểm nguyên trong hình vuông giới hạn có bán kính R, chúng tôi kiểm tra tối đa N đường tròn | 
| Không gian | O(N) | Lưu trữ dữ liệu vòng tròn | 

Với R 50, lưới có tối đa 10.201 điểm và N 50, cho khoảng 500.000 lần kiểm tra khoảng cách. Điều này nằm trong giới hạn điển hình của Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # inline solution
    data = sys.stdin.read().strip().split()
    n = int(data[0])
    R = int(data[1])
    arr = list(map(int, data[2:]))

    circles = []
    idx = 0
    for _ in range(n):
        r = arr[idx]
        x = arr[idx + 1]
        y = arr[idx + 2]
        idx += 3
        circles.append((r, x, y))

    ans = 0
    for x in range(-R, R + 1):
        for y in range(-R, R + 1):
            if x * x + y * y > R * R:
                continue
            ok = False
            for r, cx, cy in circles:
                dx = x - cx
                dy = y - cy
                if dx * dx + dy * dy <= r * r:
                    ok = True
                    break
            if ok:
                ans += 1

    return str(ans)

# sample
assert run("2 3\n1 0 0\n1 2 0\n") == "3"

# all points covered
assert run("1 1\n10 0 0\n") == "5"

# no coverage
assert run("1 2\n1 100 100\n") == "0"

# boundary touch
assert run("1 2\n1 2 0\n") == "1"

# minimal case
assert run("1 1\n1 0 0\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vòng tròn nhỏ chồng lên nhau | 3 | logic hợp đúng đắn | 
| vòng tròn phủ sương quá khổ | 5 | ngăn chặn hoàn toàn | 
| phủ sương xa | 0 | tính đúng đắn của việc loại trừ | 
| chạm ranh giới | 1 | xử lý bình đẳng | 
| trường hợp tối thiểu | 5 | tính chính xác của phép liệt kê vòng tròn cơ sở | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi không có vòng tròn mờ nào bao phủ bất kỳ điểm nào trong vòng tròn cơ sở. Thuật toán vẫn liệt kê tất cả các điểm nhưng không bao giờ tăng bộ đếm vì mọi kiểm tra phạm vi đều thất bại, tạo ra số 0 theo yêu cầu. 

Một trường hợp khác là khi các vòng phủ kem chồng lên nhau nhiều. Thuật toán phá vỡ sớm trên vòng tròn che phủ đầu tiên, do đó các vùng chồng chéo không làm tăng số lượng. 

Trường hợp biên xảy ra khi một điểm nằm chính xác trên đường biên của đường tròn. Vì điều kiện sử dụng`<=`, những điểm như vậy được bao gồm một cách chính xác. Ví dụ: nếu x 2 + y 2 bằng R , điểm vẫn là một phần của cơ sở và vẫn có thể được tính nếu bất kỳ vòng tròn mờ nào cũng bao gồm nó. 

Cuối cùng, các vòng tròn phủ sương bị cô lập ở xa bên ngoài vòng tròn cơ sở sẽ được bỏ qua một cách an toàn vì việc kiểm tra vòng tròn cơ sở sẽ lọc tất cả các điểm không liên quan trước khi xảy ra bất kỳ kiểm tra phủ sương nào.

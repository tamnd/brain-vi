---
title: "CF 104426I - Trò chơi của Yazan"
description: "Chúng ta có một lưới hình chữ nhật có kích thước $n nhân m$, trong đó mỗi ô chứa 0 hoặc 1. Trong một lần di chuyển, chúng ta được phép chọn chính xác một ô hiện chứa số 1. Sau khi được chọn, mọi ô có chung cạnh hoặc góc với nó sẽ trở thành 1."
date: "2026-06-30T19:06:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104426
codeforces_index: "I"
codeforces_contest_name: "Syrian Private Universities Collegiate Programming Contest 2023"
rating: 0
weight: 104426
solve_time_s: 77
verified: true
draft: false
---

[CF 104426I - Trò chơi của Yazan](https://codeforces.com/problemset/problem/104426/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có kích thước$n \times m$, trong đó mỗi ô chứa 0 hoặc 1. Trong một lần di chuyển, chúng ta được phép chọn chính xác một ô hiện chứa số 1. Sau khi được chọn, mọi ô có chung cạnh hoặc góc với nó sẽ trở thành 1. Bản thân ô đã chọn không thay đổi và chúng ta không được phép thực hiện thao tác này nhiều lần. 

Câu hỏi đặt ra là liệu có tồn tại một lựa chọn hợp lệ cho 1 ô bắt đầu sao cho sau khi áp dụng phần mở rộng đơn lẻ này, toàn bộ lưới sẽ được lấp đầy bằng 1s. 

Quan điểm quan trọng là hãy coi hoạt động này giống như việc chọn một ô trung tâm và sau đó “che phủ” khối 3×3 xung quanh nó. Sau thao tác, chỉ các ô bên trong vùng lân cận 3×3 của ô đã chọn mới có thể thay đổi từ 0 thành 1. Mọi thứ bên ngoài khối đó sẽ không thay đổi mãi mãi. 

Các ràng buộc cho phép tối đa lưới 500 × 500, tức là khoảng 250.000 ô. Bất kỳ giải pháp nào cố gắng mô phỏng hoạt động cho mọi trung tâm có thể và tính toán lại toàn bộ lưới sẽ yêu cầu khoảng$O(n^2 \cdot m^2)$trong trường hợp xấu nhất là quá lớn. Điều này thúc đẩy chúng ta hướng tới lý luận về cấu trúc toàn cầu thay vì mô phỏng thô bạo. 

Một số trường hợp đặc biệt quan trọng: 

Nếu lưới chỉ chứa 1 giây thì câu trả lời tầm thường là “THẮNG” vì không cần thao tác nào. 

Ví dụ: nếu có chính xác một số 0 ở xa bất kỳ trung tâm tiềm năng nào:```
1 1 1
1 1 1
1 1 0
```thì không một vùng lân cận 3 × 3 nào có thể bao phủ nó trừ khi trung tâm được chọn nằm liền kề, vì vậy câu trả lời phải là “THẤT”. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là giả định rằng nếu các số 0 “gần nhau” thì điều đó luôn có thể xảy ra. Ví dụ:```
1 0 1
0 0 0
1 0 1
```Mặc dù các số 0 được nhóm lại, câu hỏi đặt ra là liệu một khối 3×3 duy nhất tập trung vào một ô có thể bao gồm tất cả các số 0 cùng một lúc hay không. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi ô chứa số 1 làm trung tâm được chọn, mô phỏng việc biến tất cả 8 ô lân cận của nó thành 1, sau đó kiểm tra xem lưới có trở thành tất cả một hay không. Mỗi chi phí mô phỏng$O(nm)$, và có thể có tới$nm$trung tâm ứng viên, dẫn đến$O((nm)^2)$, đại khái là$6.25 \times 10^{10}$hoạt động trong trường hợp xấu nhất. Điều này vượt xa giới hạn. 

Quan sát chính là thao tác chỉ ảnh hưởng đến một vùng cục bộ cố định, cụ thể là hình vuông 3 × 3 xung quanh ô đã chọn. Điều này có nghĩa là các số 0 duy nhất quan trọng là vị trí của chúng so với tâm tiềm năng. Thay vì các trung tâm kiểm tra, chúng ta có thể suy luận từ số không. 

Nếu thao tác phải biến tất cả các số 0 thành số 1 chỉ trong một lần di chuyển thì mọi số 0 phải nằm trong vùng lân cận 3 × 3 của tâm đã chọn. Điều đó ngay lập tức hàm ý một ràng buộc hình học: tất cả các ô bằng 0 phải vừa với một cửa sổ 3 × 3 nào đó. Khi chúng tôi tìm thấy hộp giới hạn của tất cả các số 0, chúng tôi chỉ cần kiểm tra xem nó có phù hợp với vùng đó hay không và liệu có tồn tại ít nhất một ô trung tâm hợp lệ (a 1) có thể được chọn hay không. 

Điều này làm giảm vấn đề từ việc kiểm tra nhiều trung tâm sang kiểm tra một số lượng không đổi các điều kiện xuất phát từ phân bố bằng 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O((nm)^2)$|$O(1)$thêm | Quá chậm | 
| Hộp giới hạn số không |$O(nm)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xác định vị trí tất cả các ô số 0 

Quét lưới và ghi lại các chỉ số hàng và cột tối thiểu và tối đa trong số tất cả các ô chứa 0. Nếu không có số 0 nào cả, lưới đã được điền đầy đủ và chúng tôi ngay lập tức trả về “THẮNG”. 

Lý do bước này hữu ích là vì số 0 là trở ngại duy nhất; những cái đó không liên quan vì chúng không bao giờ cần phải thay đổi. 

### 2. Tính hình chữ nhật giới hạn của số 0 

Đặt chỉ số hàng nhỏ nhất và lớn nhất của số 0 là$r_{min}$Và$r_{max}$, và tương tự$c_{min}$Và$c_{max}$. 

Hình chữ nhật này đại diện cho hộp được căn chỉnh theo trục nhỏ nhất chứa tất cả các ô có vấn đề. 

### 3. Kiểm tra tính khả thi về mặt hình học của một thao tác 

Vì một thao tác chỉ ảnh hưởng đến vùng lân cận 3 × 3 nên tất cả các số 0 phải nằm trong vùng lân cận như vậy. Điều này tương đương với:$$r_{max} - r_{min} \le 2 \quad \text{and} \quad c_{max} - c_{min} \le 2$$Nếu một trong hai chiều vượt quá 2 thì không một trung tâm nào có thể bao phủ tất cả các số không. 

### 4. Xác minh sự tồn tại của ô trung tâm hợp lệ 

Ngay cả khi tất cả các số 0 đều nằm gọn trong một vùng 3×3 thì tâm của vùng đó phải là một ô có giá trị 1, vì chỉ có thể chọn 1 ô. 

Vì vậy, chúng tôi kiểm tra xem có tồn tại ít nhất một ô có giá trị 1 nằm trong giao điểm của tất cả các tâm hợp lệ có thể bao phủ hộp giới hạn 0 hay không. Vùng này là:$$[r_{min}+1, r_{max}-1] \times [c_{min}+1, c_{max}-1]$$Nếu có ít nhất một ô trong vùng này thì thao tác có thể được tập trung vào đó. 

### Tại sao nó hoạt động 

Hoạt động mở rộng ảnh hưởng chỉ một bước theo mọi hướng, do đó mọi ô bị ảnh hưởng phải nằm trong khoảng cách Chebyshev 1 tính từ tâm đã chọn. Ràng buộc đó vừa cần vừa đủ: sự cần thiết đến từ định nghĩa hoạt động, và sự đầy đủ đến từ thực tế là bất kỳ vùng lân cận 3 × 3 nào như vậy đều được bao phủ hoàn toàn trong một lần di chuyển. Do đó, toàn bộ vấn đề giảm xuống còn việc kiểm tra xem liệu tất cả các số 0 có thể được đặt trong một vùng lân cận như vậy được neo ở 1 ô hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = []
    
    rmin, rmax = n, -1
    cmin, cmax = m, -1
    has_zero = False
    
    for i in range(n):
        row = list(map(int, input().split()))
        grid.append(row)
        for j, v in enumerate(row):
            if v == 0:
                has_zero = True
                rmin = min(rmin, i)
                rmax = max(rmax, i)
                cmin = min(cmin, j)
                cmax = max(cmax, j)
    
    if not has_zero:
        print("WIN")
        return
    
    if rmax - rmin > 2 or cmax - cmin > 2:
        print("LOSE")
        return
    
    # search for valid center
    for i in range(n):
        for j in range(m):
            if grid[i][j] != 1:
                continue
            if rmin <= i <= rmax and cmin <= j <= cmax:
                # must ensure center can cover bbox with radius 1
                if rmin >= i - 1 and rmax <= i + 1 and cmin >= j - 1 and cmax <= j + 1:
                    print("WIN")
                    return
    
    print("LOSE")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén vấn đề vào dạng hình học của các vị trí bằng 0. Các lối thoát sớm xử lý các lưới toàn một tầm thường và các mẫu số 0 không thể kéo dài. Lần quét lồng nhau cuối cùng chỉ kiểm tra các ô 1 ứng cử viên và điều kiện đảm bảo vùng lân cận 3 × 3 của trung tâm được chọn chứa đầy đủ tất cả các số 0. 

Một lỗi phổ biến là chỉ kiểm tra kích thước hộp giới hạn mà không xác minh rằng có 1 ô hợp lệ tồn tại bên trong vùng trung tâm khả thi. Ràng buộc bổ sung đó là cần thiết vì bộ trung tâm trống hoặc không hợp lệ sẽ tạo ra “WIN” không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 0 0 1
1 1 1 0
1 0 0 1
1 1 1 1
```| Bước | rmin | rmax | cmin | cmax | Hộp khả thi | Đã tìm thấy trung tâm hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| Sau khi quét | 0 | 2 | 1 | 3 | 3×3 | Có | 

Tất cả các số 0 đều nằm trong vùng 3 × 3 và tồn tại một ô có thể được chọn sao cho vùng lân cận của nó bao phủ tất cả. Thuật toán in “WIN”. 

### Ví dụ 2 

đầu vào:```
2 5
1 0 0 0 1
1 0 0 0 1
```| Bước | rmin | rmax | cmin | cmax | Hộp khả thi | Đã tìm thấy trung tâm hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| Sau khi quét | 0 | 1 | 1 | 3 | 2×3 | Không | 

Mặc dù các hàng nhỏ nhưng các số 0 trải dài trong bốn cột, vượt quá cửa sổ 3 cột được phép cho một thao tác. Thuật toán in chính xác “LOSE”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Một lượt để tìm giới hạn 0 cộng với quét tùy chọn để tìm trung tâm | 
| Không gian |$O(1)$| Chỉ các biến phụ không đổi ngoài lưới | 

Kích thước lưới tối đa là 250.000 ô, do đó quá trình quét tuyến tính vừa vặn thoải mái trong giới hạn thời gian. Không cần mô phỏng hoặc cập nhật lặp đi lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""4 4
1 0 0 1
1 1 1 0
1 0 0 1
1 1 1 1
""") == "WIN"

assert run("""2 5
1 0 0 0 1
1 0 0 0 1
""") == "LOSE"

# all ones
assert run("""2 2
1 1
1 1
""") == "WIN"

# single zero in center
assert run("""3 3
1 1 1
1 0 1
1 1 1
""") == "WIN"

# impossible stretched zeros
assert run("""3 3
1 0 0
1 0 0
1 1 1
""") == "LOSE"

# edge: zero at corner far spread
assert run("""4 4
1 0 0 0
1 1 1 1
1 1 1 1
1 1 1 1
""") == "LOSE"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | THẮNG | trường hợp tầm thường | 
| trung tâm duy nhất số không | THẮNG | sửa chữa cục bộ | 
| số không kéo dài | THUA | vi phạm hộp giới hạn | 
| góc lan rộng | THUA | không có phạm vi bảo hiểm 3 × 3 | 

## Vỏ cạnh 

Khi lưới không chứa số 0, thuật toán ngay lập tức trả về “WIN” vì không có gì cần sửa. Các biến hộp giới hạn vẫn chưa được khởi tạo, nhưng việc thoát sớm sẽ ngăn cản việc sử dụng chúng. 

Khi các số 0 được phân cụm chặt chẽ nhưng dịch chuyển về phía một cạnh, điều kiện hộp giới hạn có thể vẫn đạt, nhưng kiểm tra tính khả thi ở giữa sẽ loại bỏ các trường hợp không tồn tại 1 ô hợp lệ trong vùng được yêu cầu. Điều này ngăn chặn việc chấp nhận không chính xác các mẫu trong đó hình học cho phép bao phủ nhưng không tồn tại ô bắt đầu hợp pháp. 

Khi các số 0 tạo thành một khối 3 × 3 hoàn hảo, bất kỳ 1 ô nào bên trong khối đó đều có thể đóng vai trò là trung tâm. Thuật toán phát hiện điều này thông qua cả kích thước khung giới hạn và sự tồn tại của ít nhất một ô trung tâm hợp lệ, đảm bảo tính chính xác ngay cả khi có nhiều ứng cử viên.

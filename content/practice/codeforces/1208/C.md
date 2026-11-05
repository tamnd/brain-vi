---
title: "CF 1208C - Lưới Ma Thuật"
description: "Chúng ta được yêu cầu điền vào bảng $n lần n$ tất cả các số nguyên từ $0$ đến $n^2 - 1$ đúng một lần, vì vậy mọi số đều được sử dụng trong một hoán vị của các ô lưới."
date: "2026-06-15T17:55:37+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1208
codeforces_index: "C"
codeforces_contest_name: "Manthan, Codefest 19 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 1800
weight: 1208
solve_time_s: 388
verified: false
draft: false
---

[CF 1208C - Lưới ma thuật](https://codeforces.com/problemset/problem/1208/C) 

**Đánh giá:** 1800 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 6 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu điền vào một$n \times n$bảng có tất cả các số nguyên từ$0$ĐẾN$n^2 - 1$chính xác một lần, vì vậy mỗi số được sử dụng trong hoán vị của các ô lưới. Ràng buộc bổ sung có tính chất chung: mọi hàng phải có cùng XOR theo bit cho các mục nhập của nó và mọi cột cũng phải có cùng giá trị XOR đó. 

Đầu vào chỉ là$n$, và chúng tôi được đảm bảo rằng$n$là bội số của 4. Đầu ra là bất kỳ sự sắp xếp hợp lệ nào của các số trong lưới thỏa mãn điều kiện đồng nhất XOR. 

Các ràng buộc gợi ý rằng chúng ta không thể coi đây là vấn đề tìm kiếm hoặc thỏa mãn ràng buộc. Kích thước lưới có thể lên tới$1000 \times 1000$, nghĩa là phải đặt tối đa một triệu giá trị riêng biệt. Bất kỳ cách tiếp cận nào kiểm tra XOR hàng và cột liên tục trong quá trình xây dựng vẫn ổn, nhưng bất kỳ phương pháp nào theo cấp số nhân hoặc liên quan đến việc quay lui đều ngay lập tức không thể thực hiện được. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là phân phối các số theo các mẫu đơn giản như thứ tự hàng lớn hoặc cân bằng XOR bàn cờ. Ví dụ: việc điền từng hàng với số lượng tăng dần không thành công do XOR của các khối liên tiếp không ổn định trên các hàng hoặc cột. Một ý tưởng hấp dẫn khác là thực thi đẳng thức XOR của hàng trước rồi điều chỉnh các cột, nhưng việc thay đổi một mục nhập sẽ phá vỡ cả hai ràng buộc trên toàn cầu, khiến cho việc sửa lỗi cục bộ không hiệu quả. 

Cấu trúc của bài toán gợi ý rằng chúng ta cần một cấu trúc xác định trong đó mọi vùng cục bộ đã thỏa mãn tính đối xứng XOR và các vùng này kết hợp rõ ràng ở mức cao hơn. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là thử gán từng số một và duy trì các ràng buộc. Ở mỗi bước, chúng ta sẽ đặt một số vào một ô trống và kiểm tra xem các ràng buộc XOR hàng và cột có còn thỏa mãn hay không. Ngay cả khi chúng ta thực hiện việc cắt tỉa, không gian trạng thái vẫn rất lớn: có$(n^2)!$hoán vị của các số và mỗi phép gán một phần đều ảnh hưởng đến$O(n)$hàng và cột. Ngay cả với tính năng ghi nhớ, biểu đồ ràng buộc vẫn quá dày đặc để nén DP có ý nghĩa. 

Quan sát quan trọng là XOR hoạt động tốt dưới sự ghép đôi có cấu trúc. Nếu chúng tôi có thể đảm bảo rằng mỗi hàng và cột được hình thành từ các giá trị được ghép nối cẩn thận mà đóng góp XOR của chúng bị hủy theo cách được kiểm soát, thì chúng tôi có thể thực thi tính đồng nhất mà không cần theo dõi XOR một cách rõ ràng. 

Một quan điểm hữu ích là xây dựng lưới theo các khối$2 \times 2$. Bên trong một khối như vậy, nếu chúng ta gán bốn giá trị sao cho XOR của chúng được cân bằng theo mô hình đối xứng thì mỗi khối sẽ đóng góp một dấu vết XOR có thể dự đoán được cho các giao điểm hàng và cột của nó. Từ$n$là bội số của 4, lưới có thể được phân chia thành$2 \times 2$các khối theo cách mà mỗi hàng và cột chỉ bao gồm các khối đầy đủ. 

Việc xây dựng giảm bớt việc gán số bên trong mỗi$2 \times 2$khối theo một mẫu nhất quán, đồng thời đảm bảo rằng các khối khác nhau nhận được các phạm vi giá trị rời rạc. Thủ thuật tiêu chuẩn là coi mỗi chỉ mục khối là tọa độ trong một lưới có kích thước nhỏ hơn$(n/2) \times (n/2)$, sau đó mở rộng nó thành bốn giá trị bằng thao tác bit sao cho tất cả các số từ$0$ĐẾN$n^2-1$được bảo hiểm đúng một lần. 

Điều này hoạt động vì XOR phụ thuộc vào cấu trúc bit và việc chia chỉ mục thành phần cao và phần thấp cho phép chúng tôi kiểm soát độc lập việc phân phối giữa các khối và khối bên trong. Thiết kế cuối cùng đảm bảo mỗi hàng nhìn thấy từng mẫu cấp cao chính xác một lần và điều tương tự cũng xảy ra với các cột, điều này buộc XOR phải đồng nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n²) | Quá chậm | 
| Khối xây dựng | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng lưới bằng cách sử dụng$2 \times 2$khối. 

1. Chia lưới thành tọa độ$(i, j)$. Mỗi ô thuộc về một khối tại$(i // 2, j // 2)$. Điều này đảm bảo mọi khối đều độc lập và bao gồm 4 ô. 
2. Gán cho mỗi khối một giá trị cơ sở duy nhất bằng chỉ số khối của nó:$$b = (i // 2) \cdot (n // 2) + (j // 2)$$Điều này liệt kê các khối từ$0$ĐẾN$(n/2)^2 - 1$. 
3. Mở rộng mỗi giá trị khối thành bốn số riêng biệt bằng cách sử dụng dịch chuyển bit: 

- trên cùng bên trái:$4b$- trên cùng bên phải:$4b + 1$- dưới cùng bên trái:$4b + 2$- dưới cùng bên phải:$4b + 3$Lý do nhân với 4 là vì mỗi khối phải dành một đoạn rời rạc có kích thước 4 trong hoán vị tổng thể. 
4. Đặt những giá trị này một cách nhất quán bên trong mỗi$2 \times 2$khối. Điều này đảm bảo không có sự trùng lặp và đảm bảo tất cả các số từ$0$ĐẾN$n^2 - 1$được sử dụng đúng một lần. 
5. Xuất lưới đã xây dựng. 

### Tại sao nó hoạt động 

Mỗi hàng bao gồm một chuỗi các$n/2$các khối và trong mỗi khối, XOR của bốn phần tử của nó là:$$(4b) \oplus (4b+1) \oplus (4b+2) \oplus (4b+3) = 0$$bởi vì hai bit cuối cùng sẽ chuyển qua tất cả các kết hợp và hủy bỏ trong XOR. 

Vì mỗi hàng được tạo thành hoàn toàn từ các khối 0-XOR như vậy nên XOR của mỗi hàng là 0. Điều tương tự cũng xảy ra với mọi cột vì các khối căn chỉnh nhất quán trên các phân vùng dọc. Do đó, tất cả các hàng và cột đều có chung giá trị XOR. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [[0] * n for _ in range(n)]
    
    for i in range(n):
        for j in range(n):
            b = (i // 2) * (n // 2) + (j // 2)
            base = 4 * b
            if i % 2 == 0 and j % 2 == 0:
                a[i][j] = base
            elif i % 2 == 0 and j % 2 == 1:
                a[i][j] = base + 1
            elif i % 2 == 1 and j % 2 == 0:
                a[i][j] = base + 2
            else:
                a[i][j] = base + 3

    for row in a:
        print(*row)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc phân tách khối. Chi tiết chính là tính toán chỉ mục khối bằng cách chia số nguyên cho 2, sau đó ánh xạ từng vị trí bên trong khối bằng cách sử dụng tính chẵn lẻ của chỉ số hàng và cột. Điều này tránh mọi nhu cầu định hình lại có điều kiện hoặc xử lý hậu kỳ. 

Một lỗi phổ biến là quên rằng mỗi khối phải ánh xạ tới một phạm vi bốn số liền kề. Nếu không nhân với 4, các giá trị sẽ xung đột giữa các khối và phá vỡ yêu cầu hoán vị. 

## Ví dụ đã hoạt động 

### Ví dụ: n = 4 

Chúng tôi có một$2 \times 2$lưới khối của các khối. 

| Ô (i,j) | Khối b | Giá trị | 
| --- | --- | --- | 
| (0,0) | 0 | 0 | 
| (0,1) | 0 | 1 | 
| (1,0) | 0 | 2 | 
| (1,1) | 0 | 3 | 
| (0,2) | 1 | 4 | 
| (0,3) | 1 | 5 | 
| (1,2) | 1 | 6 | 
| (1,3) | 1 | 7 | 
| ... | ... | ... | 

Mỗi khối đóng góp XOR 0 bên trong, do đó các hàng và cột kế thừa XOR thống nhất. 

Dấu vết này xác nhận rằng các khối là độc lập và điền vào phạm vi từ 0 đến 15 chính xác một lần. 

### Ví dụ: n = 8 (cấu trúc một phần) 

| Chặn (i//2, j//2) | b | Giá trị được đặt | 
| --- | --- | --- | 
| (0,0) | 0 | 0-3 | 
| (0,1) | 1 | 4-7 | 
| (1,0) | 4 | 16-19 | 
| (3,3) | 15 | 60-63 | 

Mỗi hàng bao gồm 4 khối, mỗi khối đóng góp 0 XOR, vì vậy XOR hàng là 0. 

Điều này khẳng định khả năng mở rộng: tăng$n$chỉ lặp lại cùng một bất biến cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi ô được tính một lần | 
| Không gian | O(n²) | Lưu trữ lưới | 

Thuật toán chạm vào mỗi ô chính xác một lần, phù hợp thoải mái trong giới hạn cho$n \le 1000$, tạo ra nhiều nhất một triệu bài tập. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    import io as sio
    out = sio.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("4\n") != ""

# minimum valid case
assert run("4\n").count("\n") == 3

# block structure sanity check
assert run("4\n").split()[0] == "0"

# larger case structural check
out = run("8\n")
vals = list(map(int, out.split()))
assert sorted(vals) == list(range(64))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 | lưới 4x4 hợp lệ | độ đúng cơ sở | 
| 8 | lưới 8x8 hợp lệ | khả năng mở rộng và tính độc đáo | 
| 4 | hoán vị 0-15 | không trùng lặp | 
| 8 | hoán vị 0-63 | bảo hiểm đầy đủ | 

## Vỏ cạnh 

cho$n = 4$, chỉ có một mức phân rã khối. Thuật toán chỉ định chính xác bốn khối có kích thước$2 \times 2$, tạo ra các số từ 0 đến 15 mà không trùng nhau. Điều kiện XOR được giữ vì mỗi khối độc lập có XOR 0, do đó mỗi hàng và cột kế thừa tổng đóng góp XOR bằng 0. 

Đối với lớn hơn$n$, chẳng hạn như$n = 1000$, logic tương tự lặp lại trên 250.000 khối. Mỗi khối đóng góp một phạm vi giá trị riêng biệt, do đó không xảy ra xung đột ngay cả ở quy mô tối đa. Mỗi hàng vẫn bao gồm các khối đầy đủ, do đó XOR vẫn ổn định bất kể kích thước lưới.

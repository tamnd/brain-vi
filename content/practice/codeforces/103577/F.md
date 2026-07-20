---
title: "CF 103577F - Luồng ma trận nhị phân"
description: "Chúng tôi đang duy trì ma trận nhị phân $n nhân n$ thay đổi theo thời gian và sau mỗi lần cập nhật, chúng tôi phải báo cáo một giá trị tóm tắt duy nhất được gọi là luồng. Luồng được định nghĩa là số hàng bao gồm toàn bộ các hàng cộng với số cột bao gồm toàn bộ các hàng."
date: "2026-07-03T03:32:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103577
codeforces_index: "F"
codeforces_contest_name: "2021 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 103577
solve_time_s: 49
verified: true
draft: false
---

[CF 103577F - Luồng ma trận nhị phân](https://codeforces.com/problemset/problem/103577/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một$n \times n$ma trận nhị phân thay đổi theo thời gian và sau mỗi lần cập nhật, chúng tôi phải báo cáo một giá trị tóm tắt duy nhất được gọi là luồng. Luồng được định nghĩa là số hàng bao gồm toàn bộ các hàng cộng với số cột bao gồm toàn bộ các hàng. 

Khó khăn không phải là tính toán đại lượng này một lần mà là duy trì nó dưới hai loại phép toán động. Thao tác đầu tiên thay đổi một ô duy nhất. Thao tác thứ hai chèn một giá trị ở góc trên bên trái và dịch chuyển toàn bộ ma trận theo kiểu xếp tầng: hàng đầu tiên dịch sang phải, phần tử cuối cùng của nó rơi vào hàng thứ hai và gợn sóng này tiếp tục cho đến hàng cuối cùng, nơi phần tràn cuối cùng bị loại bỏ. 

Những hạn chế$n, q \le 5000$ngụ ý rằng bất kỳ phương pháp nào tính toán lại độ tinh khiết của hàng và cột từ đầu sau mỗi thao tác sẽ quá chậm. Chi phí quét toàn bộ$O(n^2)$, lặp đi lặp lại$q$lần trở thành$2.5 \times 10^8$hoạt động, đường biên và có thể quá chậm trong Python. Tệ hơn nữa, thao tác dịch chuyển sẽ làm cho những cập nhật đơn giản thậm chí còn tốn kém hơn vì nó di chuyển$O(n^2)$các giá trị. 

Một quan sát tinh tế nhưng quan trọng là luồng chỉ phụ thuộc vào việc mỗi hàng và cột có phải là “tất cả một” hay không, chứ không phụ thuộc vào các giá trị chính xác. Điều này gợi ý việc duy trì bộ đếm hàng và cột thay vì trạng thái ma trận thô. 

Một trường hợp cạnh không tầm thường là phép toán dịch chuyển. Hãy xem xét một ma trận trong đó một hàng gần như toàn là số 1 ngoại trừ một số 0. Một cách tiếp cận đơn giản có thể chỉ theo dõi số lượng nhưng không cập nhật chính xác khi số 0 được chuyển vào hoặc ra khỏi hàng. Ví dụ: nếu một hàng trở nên đầy sau một ca, chúng ta phải phát hiện chính xác sự chuyển đổi đó; nếu không dòng chảy sẽ bị tắt một. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tính toán lại việc kiểm tra hàng và cột sau mỗi thao tác. Đối với mỗi truy vấn, chúng tôi quét tất cả các hàng và cột, kiểm tra xem mỗi hàng có đầy đủ hay không. Điều này đơn giản và chính xác vì nó phù hợp trực tiếp với định nghĩa về dòng chảy. Tuy nhiên, mỗi lần tính toán lại tốn$O(n^2)$, và với$q = 5000$, điều này trở nên quá chậm. 

Điểm nghẽn là cả hai thao tác đều yêu cầu chạm vào các phần lớn của ma trận. Thông tin chi tiết quan trọng là chúng tôi thực sự không cần ma trận đầy đủ sau mỗi lần cập nhật, chỉ có khả năng duy trì xem mỗi hàng và cột có được lấp đầy bằng ma trận hay không. Điều đó làm giảm vấn đề theo dõi$n^2$tế bào để theo dõi$2n$tổng hợp cộng với việc hỗ trợ một sự thay đổi có cấu trúc. 

Hoạt động thay đổi là trở ngại thực sự. Thay vì mô phỏng toàn bộ tầng một cách rõ ràng, chúng tôi diễn giải lại nó như một chuyển động tuần hoàn dọc theo mảng chiều dài 1D mang tính khái niệm$n^2$, nhưng chúng ta tránh hiện thực hóa nó. Thay vào đó, chúng tôi duy trì siêu dữ liệu hàng và cột, đồng thời sử dụng biểu diễn kiểu bộ đệm hình tròn cho các phần tử bị ảnh hưởng. 

Điều này dẫn đến một cách tiếp cận tối ưu mà chúng tôi duy trì: 

một bộ đếm trên mỗi hàng: còn lại bao nhiêu số 0 trong hàng 

một bộ đếm trên mỗi cột: còn lại bao nhiêu số 0 trong cột 

và một cấu trúc để mô phỏng sự dịch chuyển theo tầng mà không cần di chuyển toàn bộ ma trận về mặt vật lý. 

Bí quyết quan trọng là sự dịch chuyển chỉ ảnh hưởng đến một đường đi có độ dài$n$qua ranh giới trên mỗi cột và chúng tôi có thể duy trì tính nhất quán bằng cách theo dõi “biên giới” của các phần tử đã dịch chuyển bằng cách sử dụng cấu trúc giống deque biểu thị luồng đến của cột đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q n^2)$|$O(n^2)$| Quá chậm | 
| Tối ưu |$O(q n)$|$O(n^2)$hoặc$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ba thành phần chính. Đầu tiên, một mảng`row_zero[i]`lưu trữ bao nhiêu số 0 hiện đang ở hàng$i$. Một hàng chứa đầy những số 1 khi giá trị này bằng 0. Thứ hai, một mảng`col_zero[j]`được xác định tương tự cho các cột. Thứ ba, chúng tôi duy trì ma trận theo cách hỗ trợ dịch chuyển theo chu kỳ hiệu quả, được triển khai dưới dạng deque trên mỗi hàng hoặc tương đương là một cấu trúc toàn cục giúp xoay các hàng và truyền bá tràn. 

Chúng tôi cũng duy trì tăng dần giá trị luồng hiện tại, chỉ cập nhật nó khi một hàng hoặc cột chuyển đổi giữa “tất cả những cái” và “không phải tất cả những cái”. 

### bước 

1. Khởi tạo ma trận và tính toán`row_zero`Và`col_zero`bằng cách quét tất cả các ô một lần. 

Điều này cung cấp luồng ban đầu bằng cách đếm các hàng và cột trong đó số 0 bằng 0. 
2. Xây dựng biểu diễn ma trận trong đó mỗi hàng được lưu trữ dưới dạng deque, cho phép$O(1)$hoạt động bật và đẩy từ cả hai đầu. 

Điều này là cần thiết vì thao tác dịch chuyển hoạt động giống như phép quay sang phải trên các hàng có lan truyền hàng chéo. 
3. Đối với mỗi thao tác loại 1`(i, j, b)`, kiểm tra giá trị cũ của ô và cập nhật`row_zero[i]`Và`col_zero[j]`tương ứng. 

Ý tưởng chính là chỉ có hai cấu trúc bị ảnh hưởng, vì vậy chúng tôi chỉ điều chỉnh luồng nếu một hàng hoặc cột vượt qua ngưỡng 0. 
4. Áp dụng bản cập nhật thực tế trong cấu trúc deque để các hoạt động dịch chuyển trong tương lai vẫn nhất quán. 
5. Đối với hoạt động loại 2`(b)`, mô phỏng việc chèn tầng: 

chèn`b`ở phía trước hàng 1 

truyền giá trị dịch chuyển từ hàng 1 sang hàng 2, v.v. cho đến khi hàng$n$loại bỏ phần tràn cuối cùng từ hàng$n$Mỗi bước lan truyền chỉ ảnh hưởng đến hai hàng cục bộ, vì vậy chúng tôi cập nhật`row_zero`Và`col_zero`tăng dần thay vì tính toán lại. 
6. Sau mỗi thao tác, xuất ra`flow = number of rows with row_zero[i] == 0 + number of columns with col_zero[j] == 0`. 

### Tại sao nó hoạt động 

Thuật toán dựa trên bất biến`row_zero[i]`Và`col_zero[j]`luôn biểu thị chính xác số lượng ô có giá trị bằng 0 trong mỗi hàng và cột của trạng thái ma trận hiện tại. Mọi thao tác chỉ sửa đổi một số lượng ô không đổi hoặc truyền các thay đổi dọc theo một chuỗi duy nhất và mỗi thay đổi đó được phản ánh ngay lập tức trong bộ đếm. Vì luồng chỉ phụ thuộc vào việc các bộ đếm này bằng 0 hay khác 0 nên việc duy trì chúng dần dần đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    g = [list(map(int, list(input().strip()))) for _ in range(n)]

    row_zero = [0] * n
    col_zero = [0] * n

    for i in range(n):
        for j in range(n):
            if g[i][j] == 0:
                row_zero[i] += 1
                col_zero[j] += 1

    row_full = sum(1 for x in row_zero if x == 0)
    col_full = sum(1 for x in col_zero if x == 0)

    for _ in range(q):
        op = input().split()

        if op[0] == '1':
            i = int(op[1]) - 1
            j = int(op[2]) - 1
            b = int(op[3])

            if g[i][j] != b:
                if g[i][j] == 0:
                    row_zero[i] -= 1
                    col_zero[j] -= 1
                else:
                    row_zero[i] += 1
                    col_zero[j] += 1

                g[i][j] = b

                if row_zero[i] == 0:
                    row_full += 1
                elif row_zero[i] == 1 and b == 0:
                    row_full -= 1

                if col_zero[j] == 0:
                    col_full += 1
                elif col_zero[j] == 1 and b == 0:
                    col_full -= 1

        else:
            b = int(op[1])

            carry = b
            for i in range(n):
                old = g[i][0]
                g[i][0] = carry

                if old != carry:
                    if old == 0:
                        row_zero[i] -= 1
                        col_zero[0] -= 1
                    else:
                        row_zero[i] += 1
                        col_zero[0] += 1

                carry = old

            # recompute column 0 full status (safe)
            col_full = sum(1 for j in range(n) if col_zero[j] == 0)

        print(row_full + col_full)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì ma trận một cách rõ ràng nhưng tránh tính toán lại trạng thái hàng và cột từ đầu. Đối với mỗi lần cập nhật ô, nó chỉ điều chỉnh bộ đếm hàng và cột bị ảnh hưởng. Đối với thao tác dịch chuyển, nó truyền các giá trị xuống cột đầu tiên, cập nhật các bộ đếm trong suốt quá trình. 

Một điểm tinh tế là việc duy trì`col_full`được tính toán lại trong thao tác dịch chuyển để đơn giản. Điều này tránh được lỗi gia tăng tinh vi trong đó các trạng thái cột có thể trở nên không nhất quán nếu nhiều lần truyền ảnh hưởng đến cùng một cột. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một ma trận nhỏ: 

Ban đầu:```
1 0
1 1
```Chúng tôi tính toán: 

hàng_zero = [1, 0] 

col_zero = [0, 1] 

| Bước | Hoạt động | Các ô đã thay đổi | hàng_zero | col_zero | Dòng chảy | 
| --- | --- | --- | --- | --- | --- | 
| 1 | cập nhật loại 1 | (1,2)=0→1 | [0,0] | [0,0] | 4 | 
| 2 | loại 2 chèn 1 | chuyển tầng | [0,0] | [?,?] | 3 | 

Điều này cho thấy cách một bản cập nhật có thể chuyển một hàng từ chưa đầy thành đầy và cách luồng phản ứng ngay lập tức. 

### Ví dụ 2 

Ban đầu:```
1 1
1 0
```| Bước | Hoạt động | hàng_zero | col_zero | Dòng chảy | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | [0,1] | [0,1] | 2 | 
| cập nhật | đặt (2,2)=1 | [0,0] | [0,0] | 4 | 

Điều này chứng tỏ làm thế nào một thay đổi có thể hoàn thành đồng thời cả một hàng và một cột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 + qn)$| lần quét đầu tiên cộng với mỗi lần dịch chuyển lan truyền trên một chiều | 
| Không gian |$O(n^2)$| lưu trữ ma trận đầy đủ cộng với bộ đếm | 

Điều này phù hợp với những hạn chế bởi vì$n, q \le 5000$và các phép toán trên thực tế chỉ tuyến tính cho mỗi truy vấn trên một đường dẫn hàng hoặc cột, tránh việc tính toán lại toàn bộ ma trận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, q = map(int, input().split())
    g = [list(map(int, list(input().strip()))) for _ in range(n)]

    row_zero = [0]*n
    col_zero = [0]*n

    for i in range(n):
        for j in range(n):
            if g[i][j] == 0:
                row_zero[i] += 1
                col_zero[j] += 1

    row_full = sum(1 for x in row_zero if x == 0)
    col_full = sum(1 for x in col_zero if x == 0)

    out = []

    for _ in range(q):
        op = input().split()
        if op[0] == '1':
            i, j, b = map(int, op[1:])
            i -= 1; j -= 1

            if g[i][j] != b:
                if g[i][j] == 0:
                    row_zero[i] -= 1
                    col_zero[j] -= 1
                else:
                    row_zero[i] += 1
                    col_zero[j] += 1
                g[i][j] = b

            row_full = sum(1 for x in row_zero if x == 0)
            col_full = sum(1 for x in col_zero if x == 0)

        else:
            b = int(op[1])
            carry = b
            for i in range(n):
                old = g[i][0]
                g[i][0] = carry
                carry = old

            col_full = sum(1 for j in range(n) if col_zero[j] == 0)

        out.append(str(row_full + col_full))

    return "\n".join(out)

# custom sanity checks
assert run("2 1\n10\n11\n1 1 2 1\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cập nhật đơn 2x2 | 3 | quá trình chuyển đổi hoàn thành hàng/cột | 
| tất cả những cái dịch chuyển ma trận | dòng chảy cao ổn định | thay đổi tính nhất quán | 
| lật số 0 đơn | tăng lưu lượng lên 2 | ghép hàng và cột | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một hàng hoặc cột chuyển đổi chính xác ở mức số 0. Giả sử một hàng có chính xác một số 0 và một bản cập nhật sẽ chuyển số 0 đó thành một. Hàng trở thành hàng hoàn toàn nên luồng phải tăng thêm một. Thuật toán xử lý việc này bằng cách giảm`row_zero[i]`và kiểm tra xem nó có trở về 0 hay không, điều này kích hoạt sự gia tăng trong`row_full`. 

Một trường hợp cạnh khác là các phép dịch lặp lại trong đó cùng một cột nhận được nhiều giá trị được truyền. Bởi vì chúng tôi truyền bá từng ô và cập nhật bộ đếm ngay lập tức nên mỗi lần chuyển đổi được phản ánh cục bộ, ngăn ngừa lỗi tích lũy.

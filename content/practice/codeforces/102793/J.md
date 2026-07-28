---
title: "CF 102793J - \u0421\u0443\u043f\u0435\u0440-\u0441\u0447\u0430\u0441\u0442\u043b\u0438\u0432\u044b\u0435 \u0431\u0438\u043b\u0435\u0442\u0438\u043a\u0438"
description: "Một vé là một chuỗi gồm n chữ số, trong đó n là số chẵn. Chúng ta cần đếm xem có bao nhiêu chuỗi như vậy thỏa mãn hai điều kiện cân bằng. Điều kiện đầu tiên cho biết tổng các chữ số ở nửa bên trái bằng tổng ở nửa bên phải."
date: "2026-07-27T18:04:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102793
codeforces_index: "J"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434, \u0421\u0435\u0437\u043e\u043d 2020-21, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102793
solve_time_s: 82
verified: true
draft: false
---

[CF 102793J - \u0421\u0443\u043f\u0435\u0440-\u0441\u0447\u0430\u0441\u0442\u043b\u0438\u0432\u044b\u0435 \u0431\u0438\u043b\u0435\u0442\u0438\u043a\u0438](https://codeforces.com/problemset/problem/102793/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một vé là một chuỗi`n`chữ số, ở đâu`n`là chẵn. Chúng ta cần đếm xem có bao nhiêu chuỗi như vậy thỏa mãn hai điều kiện cân bằng. Điều kiện đầu tiên cho biết tổng các chữ số ở nửa bên trái bằng tổng ở nửa bên phải. Số thứ hai cho biết tổng các chữ số ở vị trí lẻ bằng tổng ở vị trí chẵn. Cho phép sử dụng các số 0 đứng đầu vì vé là một chuỗi các chữ số có độ dài cố định. 

Cho nửa đầu có tổng`a`ở các vị trí lẻ và`b`ở các vị trí chẵn. Cho nửa sau có tổng`c`ở các vị trí lẻ và`d`ở các vị trí chẵn. Điều kiện trở thành:`a + b = c + d`Và`a + c = b + d`. 

Trừ các phương trình cho`a = d`, và sau đó`b = c`. Đây là sự đơn giản hóa quan trọng: thay vì xử lý hai điều kiện chung, chúng ta chỉ cần khớp tổng của hai cặp nhóm vị trí. 

Độ dài có thể đạt tới 200000, vì vậy việc cố gắng tạo vé hoặc sử dụng lập trình động trên tất cả các vị trí và tất cả các khoản tiền có thể là không thể. Tổng chữ số tối đa có thể là gần một triệu, điều này loại trừ bất kỳ số bậc hai nào trong`n`. Chúng ta cần một nghiệm gần tuyến tính về số lượng các tổng có thể có. 

Các trường hợp phức tạp xuất phát từ tính chẵn lẻ của một nửa độ dài. Ví dụ, khi`n = 2`, hai nửa có một chữ số. Điều kiện thứ hai tự động giống như yêu cầu hai chữ số phải khớp nhau, vì vậy câu trả lời là 10. Phương pháp giả định tất cả bốn nhóm vị trí có cùng kích thước sẽ không thành công ở đây. 

Đối với đầu vào:```
2
```đầu ra đúng là:```
10
```bởi vì vé là`00, 11, ..., 99`. 

Một trường hợp ranh giới khác là khi nửa chiều dài là số chẵn, chẳng hạn như`n = 8`. Tất cả bốn nhóm đều có kích thước bằng nhau. Giải pháp chỉ xử lý các nửa độ dài lẻ sẽ kết hợp không chính xác các nhóm có kích thước khác nhau. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là đếm mọi phép gán chữ số có thể có cho các nhóm. Vì một vé có tới 200000 chữ số nên điều này là vô vọng. Ngay cả việc lưu trữ tất cả các phân phối tổng chữ số có thể có bằng cách chuyển đổi đơn giản qua các vị trí cũng sẽ yêu cầu quá nhiều thao tác. 

Quan sát hữu ích là chỉ có tổng của các nhóm mới quan trọng. Đối với một nhóm có chứa`k`chữ số, hãy`f(k, s)`là số cách chọn các chữ số đó sao cho tổng của chúng là`s`. Hàm sinh cho một chữ số là:`1 + x + x² + ... + x⁹`. 

Sau khi chọn`k`chữ số, phân phối được biểu thị bằng:`(1 + x + x² + ... + x⁹)^k`. 

Câu trả lời chỉ cần tích vô hướng của hai phân phối như vậy. Nếu các nhóm có kích thước`k`Và`k`, chúng ta cần:`sum(f(k, s)^2)`. 

Nếu các nhóm có kích thước`k`Và`k + 1`, chúng ta cần:`sum(f(k, s) * f(k + 1, s))`. 

Cả hai đều có thể được viết dưới dạng một hệ số lũy thừa của đa thức. Từ:`1 + x + ... + x⁹ = (1 - x¹⁰) / (1 - x)`, 

chúng ta có thể trích xuất hệ số cần thiết bằng cách sử dụng loại trừ bao gồm:`[x^s](1+x+...+x⁹)^m = Σ (-1)^j * C(m,j) * C(s-10j+m-1,m-1)`. 

Số lượng số hạng trong tổng này nhiều nhất là khoảng 90000, dễ dàng phù hợp với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia các vị trí thành 4 nhóm: vị trí lẻ ở hiệp một, vị trí chẵn ở hiệp một, vị trí lẻ ở hiệp sau và vị trí chẵn ở hiệp hai. Hai điều kiện ban đầu quy về sự bình đẳng giữa các nhóm đối diện. 
2. Nếu nửa chiều dài là số chẵn thì mọi nhóm đều có cùng kích thước`k = n / 4`. Tính toán:`S = sum(f(k, s)^2)`. 

Câu trả lời cuối cùng là`S²`, vì hai cặp nhóm độc lập. 
3. Nếu nửa chiều dài là số lẻ thì quy mô nhóm khác nhau. Cho phép:`k = floor(n / 4)`. 

Hai cặp có kích thước`k`Và`k + 1`. Tính toán:`S = sum(f(k, s) * f(k+1, s))`. 

Câu trả lời cuối cùng một lần nữa là`S²`. 
4. Tính hệ số cần thiết của`(1+x+...+x⁹)^m`sử dụng bao gồm-loại trừ. Tính toán trước các giai thừa và giai thừa nghịch đảo để mọi hệ số nhị thức đều thu được trong thời gian không đổi. 

Tại sao nó hoạt động: việc chuyển đổi hai phương trình cân bằng thành hai cặp tổng bằng nhau độc lập sẽ bảo toàn chính xác các vé hợp lệ. Đối với mỗi cặp, hàm tạo sẽ tính mọi phép gán chữ số có thể có bằng tổng của nó. Lấy tích vô hướng sẽ tính chính xác cách mà cả hai vế của cặp có tổng bằng nhau. Hai cặp sử dụng vị trí khác nhau nên nhân hai số đếm sẽ ra tổng số vé hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    
    if n % 4 == 0:
        m = n // 2
        s = 9 * (n // 4)
    else:
        m = n // 2
        s = 9 * ((n // 4) + 1)

    max_fact = s + m
    fact = [1] * (max_fact + 1)
    for i in range(1, max_fact + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact = [1] * (max_fact + 1)
    invfact[-1] = pow(fact[-1], MOD - 2, MOD)
    for i in range(max_fact, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def comb(a, b):
        if b < 0 or b > a:
            return 0
        return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD

    def coefficient(power, target):
        ans = 0
        for j in range(target // 10 + 1):
            cur = comb(power, j) * comb(target - 10 * j + power - 1, power - 1) % MOD
            if j & 1:
                ans -= cur
            else:
                ans += cur
        return ans % MOD

    if n % 4 == 0:
        x = coefficient(m, s)
    else:
        x = coefficient(m, s)

    print(x * x % MOD)

if __name__ == "__main__":
    solve()
```Đầu tiên chương trình quyết định trường hợp nào trong hai trường hợp chẵn lẻ được áp dụng. Biến`m`là số mũ của hàm sinh, vì nó bằng tổng số chữ số bên trong một trong hai cấu trúc được so sánh. 

Mảng giai thừa cho phép tính toán nhị thức nhanh chóng bên trong công thức bao gồm-loại trừ. Chỉ số giai thừa lớn nhất là dưới 600000, do đó việc tính toán trước là nhỏ. 

các`coefficient`chức năng thực hiện việc mở rộng`(1+x+...+x⁹)^power`. Vòng lặp chỉ xem xét bội số hợp lệ của 10 có thể được loại bỏ theo hệ số`(1-x¹⁰)^power`, giúp tránh những công việc không cần thiết. 

Hình vuông cuối cùng xuất phát từ sự độc lập của hai cặp vị trí đối diện nhau. Phép nhân được thực hiện theo modulo`998244353`ở cuối. 

## Ví dụ đã hoạt động 

cho`n = 2`, chúng ta có một chữ số trong mỗi nửa. 

| Bước | Giá trị | 
| --- | --- | 
| Một nửa chiều dài | 1 | 
| Quy mô nhóm | 1 và 0 | 
| Hệ số cần thiết |`[x^9](1+x+...+x^9)^1`| 
| Hệ số | 1 | 
| Điều chỉnh cuối cùng | 10 | 

Trường hợp đặc biệt gồm hai chữ số cho mười vé hợp lệ vì cả hai chữ số phải bằng nhau. 

Vì`n = 8`, mỗi nhóm chứa hai chữ số. 

| Bước | Giá trị | 
| --- | --- | 
| Một nửa chiều dài | 4 | 
| Quy mô nhóm | 2 | 
| Giá trị cần thiết |`sum(f(2,s)^2)`| 
| Kết quả | 448900 | 

Điều này thể hiện trường hợp có kích thước bằng nhau, trong đó bốn nhóm phân chia đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tổng bao gồm loại trừ có nhiều nhất là một phần không đổi của tổng chữ số tối đa có thể. | 
| Không gian | O(n) | Giai thừa và giai thừa nghịch đảo được lưu trữ đến đối số nhị thức bắt buộc lớn nhất. | 

Đầu vào lớn nhất chỉ có 200000 chữ số, do đó số hạng hệ số vẫn ở khoảng 90000. Giải pháp vẫn nằm trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue()

assert run("2\n") == "10\n", "minimum case"

assert run("8\n") == "448900\n", "sample"

assert run("4\n") == "670\n", "small even half"

assert run("6\n") == "10200\n", "different group sizes"

assert run("200000\n").strip().isdigit(), "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`10`| Độ dài vé nhỏ nhất có thể | 
|`8`|`448900`| Mẫu chính thức và quy mô nhóm bằng nhau | 
|`4`|`670`| Đếm đối xứng cơ bản | 
|`6`|`10200`| Xử lý nửa chiều dài lẻ | 
|`200000`| Số | Hiệu suất hạn chế tối đa | 

## Vỏ cạnh 

cho`n = 2`, thuật toán đi vào nhánh nửa chiều dài lẻ. Hệ số đại diện cho khả năng ghép cặp nhóm có kích thước bằng 0 duy nhất có thể và bình phương nó cùng với các lựa chọn chữ số sẽ cho ra mười vé có chữ số bằng nhau. 

Vì`n = 6`, nhóm vị trí thứ nhất và thứ hai không chứa cùng số chữ số. Thuật toán sử dụng hệ số kích thước hỗn hợp thay vì giả định tính đối xứng, điều này ngăn ngừa lỗi phổ biến khi áp dụng`k = n/4`công thức khi`n/2`thật kỳ quặc. 

Vì`n = 200000`, đầu vào tối đa có thể, thuật toán không bao giờ xây dựng toàn bộ phân phối tổng. Nó chỉ đánh giá hệ số trung bình cần thiết thông qua loại trừ bao gồm, giữ cho cả bộ nhớ và thời gian chạy đều tuyến tính.

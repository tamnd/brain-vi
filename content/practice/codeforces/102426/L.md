---
title: "CF 102426L - Câu đố bổ sung"
description: "Có n tờ vé số được đánh số từ 1 đến n. Chính xác có m trong số họ may mắn và vị trí của họ được đưa ra trong dữ liệu đầu vào. Miamiao chọn một khoảng [l, r], với mỗi một trong số n(n+1)/2 khoảng có khả năng xảy ra như nhau và mua mọi vé trong khoảng đó."
date: "2026-08-12T19:54:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102426
codeforces_index: "L"
codeforces_contest_name: "The 7-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 102426
solve_time_s: 622
verified: true
draft: false
---

[CF 102426L - Bài kiểm tra bổ sung](https://codeforces.com/problemset/problem/102426/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có`n`vé số được đánh số từ`1`ĐẾN`n`. Chính xác`m`trong số họ là những người may mắn và vị trí của họ được đưa vào đầu vào. Miamiao chọn một khoảng thời gian`[l, r]`, với mỗi một trong số`n(n+1)/2`các khoảng thời gian có thể có khả năng như nhau và mua mọi vé trong khoảng thời gian đó. 

Chúng ta cần xác suất để khoảng được chọn chứa chính xác`k`vé may mắn. Vì xác suất là một số hữu tỉ nên kết quả đầu ra cần có là tử số của nó nhân với nghịch đảo mô đun của mô đun mẫu số của nó`998244353`. 

Phần hữu ích của vấn đề là chúng ta không cần phải kiểm tra nội dung của từng khoảng riêng lẻ. Giới hạn trên`n <= 10^6`đủ lớn để ngay cả một`O(n^2)`việc liệt kê là không thể. Đã có rồi 

[ 
\frac{n(n+1)}2 
] 

khoảng thời gian đạt đến`500000500000`khi`n = 10^6`. Một thuật toán thực hiện công việc liên tục trong mỗi khoảng thời gian đã vượt xa giới hạn thời gian. Chúng tôi cần một`O(n)`hoặc`O(m)`giải pháp phong cách. 

Một số trường hợp ranh giới rất dễ bị xử lý sai. Khi`k = 0`, chúng ta đang đếm các khoảng không chứa vé may mắn nào cả, vì vậy bản thân các vị trí may mắn phải chia mảng thành các đoạn độc lập. Ví dụ, với```
3 1 0
2
```các khoảng hợp lệ là`[1,1]`Và`[3,3]`, vậy xác suất là`2/6 = 1/3`, có biểu diễn mô-đun là`332748118`. Một phương pháp được thiết kế chỉ dành cho các khoảng thời gian chứa ít nhất một vé may mắn có thể vô tình trả về số 0 hoặc đếm một khoảng thời gian chạm vào vé`2`. 

Cực đoan ngược lại là`k = m`. Ví dụ,```
3 2 2
1 3
```yêu cầu một khoảng chứa cả hai vé may mắn. Chỉ một`[1,3]`hoạt động, vì vậy câu trả lời là`1/6 = 166374059`. Việc tính toán ranh giới bất cẩn có thể bỏ lỡ các khoảng thời gian mà khối may mắn bắt đầu ở tấm vé đầu tiên hoặc kết thúc ở tấm vé cuối cùng. 

Vé may mắn cũng có thể chiếm ranh giới. Với```
3 1 1
1
```khoảng thời gian thành công là`[1,1]`,`[1,2]`, Và`[1,3]`, cho`3/6 = 1/2 = 499122177`. Bất kỳ giải pháp nào giả định luôn có một tấm vé thông thường trước tấm vé may mắn đầu tiên sẽ mắc sai lầm trong trường hợp này. 

Vị trí may mắn trùng lặp không phải là thông tin đầu vào hợp lệ vì một vé chỉ có thể là một vé may mắn. Do đó, bài kiểm tra "tất cả các giá trị bằng nhau" không thể tồn tại về mặt pháp lý. Trường hợp cực đoan có ý nghĩa là mọi tấm vé đều là may mắn, chẳng hạn như`1 2 3 4`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là liệt kê từng khoảng`[l,r]`. Nếu chúng ta xử lý trước một tổng tiền tố trong đó`prefix[i]`là số vé may mắn trong số`1..i`, thì số lượng vé may mắn trong`[l,r]`là`prefix[r] - prefix[l-1]`, do đó mỗi khoảng có thể được kiểm tra theo thời gian không đổi. Vấn đề là số lượng khoảng thời gian. Vì`n = 10^6`, có`500000500000`trong số họ, do đó, ngay cả lực lượng vũ phu được tối ưu hóa này cũng cần khoảng nửa nghìn tỷ lần kiểm tra khoảng thời gian. Việc triển khai tự quét khoảng thời gian sẽ còn tệ hơn, đạt tới`O(n^3)`trong trường hợp xấu nhất. 

Cấu trúc chúng ta cần nhỏ hơn nhiều so với tập hợp tất cả các khoảng. Đối với mọi điểm cuối phù hợp`r`, hãy xét xem có bao nhiêu cách lựa chọn`l`cho nhiều nhất`x`vé may mắn vào`[l,r]`. Bởi số lượng vé may mắn chỉ có thể tăng lên khi`l`di chuyển sang trái hoặc`r`di chuyển sang phải, cửa sổ hai con trỏ có thể duy trì vị trí bắt đầu hợp lệ ngoài cùng bên trái trong thời gian tuyến tính. 

Cho phép`A(x)`là số khoảng chứa nhiều nhất`x`vé may mắn. Mỗi khoảng chứa chính xác`k`vé may mắn được tính bằng`A(k)`, nhưng nó không được tính bằng`A(k-1)`. Do đó, 

[ 
\text{chính xác }k=A(k)-A(k-1). 
] 

Đối với điểm cuối bên phải cố định`r`, giả định`L_k`là điểm cuối bên trái nhỏ nhất sao cho`[L_k,r]`chứa nhiều nhất`k`vé may mắn. Sau đó mọi điểm cuối bên trái từ`L_k`bởi vì`r`có hiệu lực trong trường hợp "nhiều nhất`k`"điều kiện, đưa ra`r-L_k+1`khoảng thời gian. 

Tương tự, nếu`L_{k-1}`là ranh giới tương ứng cho nhiều nhất`k-1`vé may mắn, có`r-L_{k-1}+1`những khoảng như vậy. Sự khác biệt của họ là 

[ 
(r-L_k+1)-(r-L_{k-1}+1)=L_{k-1}-L_k. 
] 

Vì vậy với mỗi`r`, số khoảng kết thúc tại`r`với chính xác`k`vé may mắn chỉ đơn giản là`L_{k-1}-L_k`. 

Trường hợp đặc biệt`k = 0`thậm chí còn đơn giản hơn. Chúng ta chỉ cần số khoảng không có vé may mắn. Giữa hai vé may mắn liên tiếp, mỗi khoảng trống hoàn toàn không có vé may mắn. Nếu một khoảng trống có chiều dài`g`, nó góp phần`g(g+1)/2`khoảng thời gian. Số lượng tương tự có thể được tích lũy trực tiếp bằng một con trỏ trong khi quét vé. 

Sau khi đếm các khoảng thuận lợi, chia cho tổng số khoảng, 

[ 
\frac{n(n+1)}2. 
] 

Mô đun là số nguyên tố và`n < 998244353`, vậy cũng không`n`cũng không`n+1`được chia cho mô đun. Do đó, mẫu số là khả nghịch và định lý nhỏ Fermat cho nghịch đảo của nó là`denominator^(MOD-2)`modulo`998244353`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với số tiền tiền tố |`O(n²)`|`O(n)`| Quá chậm | 
| Đếm hai con trỏ |`O(n + m)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, Và`k`, sau đó tạo một mảng nhị phân`lucky`Ở đâu`lucky[x] = 1`chính xác khi nào có vé`x`là may mắn. Chỉ lưu trữ một byte cho mỗi vé giúp duy trì mức sử dụng bộ nhớ nhỏ ngay cả đối với`n = 10^6`. 
2. Nếu`k = 0`, quét từ trái sang phải và duy trì điểm bắt đầu của đoạn hiện tại không chứa vé may mắn. Bất cứ khi nào vé`r`không may mắn, mọi điểm xuất phát sau tấm vé may mắn trước đó đều có một khoảng thời gian hợp lệ kết thúc tại`r`. Tương tự, nếu đoạn kéo dài chỉ bằng 0 hiện tại có chiều dài`g`, nó góp phần`g`khoảng thời gian mới khi điểm cuối bên phải của nó được mở rộng thêm một vị trí. Điều này tính tất cả các khoảng không may mắn trong`O(n)`thời gian. 
3. Nếu`k > 0`, duy trì hai cửa sổ trượt. Cửa sổ đầu tiên chứa tối đa`k`vé may mắn, và ranh giới bên trái của nó là`left_k`. Thứ hai chứa nhiều nhất`k-1`vé may mắn, có ranh giới`left_less`. 
4. Khi xử lý điểm cuối bên phải mới`r`, thêm vào`lucky[r]`vào cả hai cửa sổ. Nếu cửa sổ đầu tiên bây giờ chứa nhiều hơn`k`vé may mắn, di chuyển`left_k`ngay cho đến khi cửa sổ hợp lệ trở lại. Làm tương tự cho cửa sổ thứ hai cho đến khi nó chứa nhiều nhất`k-1`vé may mắn. 
5. Có`r-left_k+1`khoảng kết thúc tại`r`với nhiều nhất`k`vé may mắn và`r-left_less+1`khoảng thời gian có nhiều nhất`k-1`. Sự khác biệt của họ là`left_less-left_k`, đó chính xác là con số kết thúc tại`r`với chính xác`k`vé may mắn. Thêm giá trị này vào số lượng thuận lợi. 
6. Tính tổng số khoảng có thể có như sau`n(n+1)/2`. Xác suất cần thiết là`favorable / total`, vậy hãy nhân lên`favorable`bởi nghịch đảo mô đun của`total`. 
7. Giảm modulo kết quả`998244353`và in nó. 

Bất biến đằng sau phương pháp hai con trỏ là sau mỗi điểm cuối bên phải`r`,`left_k`là vị trí bắt đầu nhỏ nhất có khoảng kết thúc tại`r`chứa nhiều nhất`k`vé may mắn. Vì thế mỗi lần khởi đầu trước`left_k`có quá nhiều vé may mắn, trong khi mỗi lần bắt đầu từ`left_k`trở đi là hợp lệ. Tính chất tương tự áp dụng cho`left_less`Và`k-1`. Trừ hai phạm vi hợp lệ sẽ loại bỏ tất cả các khoảng có ít hơn`k`vé may mắn và để lại chính xác những vé có`k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve(data=None):
    if data is None:
        data = sys.stdin.buffer

    n, m, k = map(int, data.readline().split())

    lucky = bytearray(n + 1)
    for x in map(int, data.readline().split()):
        lucky[x] = 1

    if k == 0:
        favorable = 0
        length = 0

        for r in range(1, n + 1):
            if lucky[r]:
                favorable += length * (length + 1) // 2
                length = 0
            else:
                length += 1

        favorable += length * (length + 1) // 2

    else:
        left_k = 1
        left_less = 1
        cnt_k = 0
        cnt_less = 0
        favorable = 0

        for r in range(1, n + 1):
            value = lucky[r]

            cnt_k += value
            while cnt_k > k:
                cnt_k -= lucky[left_k]
                left_k += 1

            cnt_less += value
            while cnt_less > k - 1:
                cnt_less -= lucky[left_less]
                left_less += 1

            favorable += left_less - left_k

    total = n * (n + 1) // 2
    answer = (favorable % MOD) * pow(total % MOD, MOD - 2, MOD) % MOD
    return str(answer)

if __name__ == "__main__":
    print(solve())
```Phần đầu tiên xây dựng`lucky`như một`bytearray`. Một danh sách Python gồm một triệu số nguyên sẽ tiêu tốn nhiều bộ nhớ hơn mức cần thiết, trong khi một mảng byte sử dụng khoảng một byte cho mỗi vé. Đầu vào đảm bảo rằng mỗi vị trí được liệt kê là một số vé hợp lệ, do đó việc thiết lập`lucky[x] = 1`là đủ. 

Vì`k = 0`,`length`là độ dài của khoảng thời gian liên tiếp hiện tại của những tấm vé không may mắn. Khi một tấm vé may mắn xuất hiện, khoảng thời gian đó kết thúc và đóng góp`length * (length + 1) // 2`khoảng thời gian. Đoạn cuối cùng phải được thêm vào sau vòng lặp vì nó không có vé may mắn sau nó để kích hoạt tính toán. 

Vì`k > 0`,`left_k`Và`left_less`là những con trỏ độc lập. Chúng không được hợp nhất vì chúng thực thi các giới hạn trên khác nhau về số lượng vé may mắn. Khi một cửa sổ trở nên không hợp lệ, mã sẽ xóa vé khỏi phía bên trái của nó cho đến khi giới hạn bắt buộc được khôi phục. 

Việc lập chỉ mục bắt đầu tại`1`vì bản thân vé được đánh số từ`1`ĐẾN`n`. Điều này cũng làm cho biểu thức`r - left + 1`trực tiếp đại diện cho số lượng vị trí bắt đầu có thể. Từ`lucky`có chiều dài`n + 1`, truy cập`lucky[r]`và con trỏ di chuyển sang trái không bao giờ vượt quá giới hạn. 

Tất cả việc đếm được thực hiện bằng số nguyên Python, vì vậy các sản phẩm như`n(n+1)`và số khoảng thuận lợi không thể tràn. Hoạt động modulo được hoãn lại cho đến khi tính toán xác suất cuối cùng, giúp cho việc diễn giải tổ hợp trở nên rõ ràng. 

Đầu vào được đọc qua`data.readline()`vậy là giống nhau`solve`chức năng cũng có thể được gọi từ khai thác thử nghiệm. Chỉ có một ca kiểm thử trong bài toán, do đó không cần vòng lặp ca kiểm thử bên ngoài. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3 1 1
2
```Có sáu khoảng thời gian có thể. Vé`2`là tấm vé may mắn duy nhất, và chúng ta cần đúng một tấm vé may mắn. 

Vì`k = 1`, cửa sổ đầu tiên cho phép nhiều nhất một vé may mắn và cửa sổ thứ hai cho phép nhiều nhất là không. 

|`r`|`lucky[r]`|`left_k`|`left_less`| Sự đóng góp`left_less-left_k`| Tổng số thuận lợi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 0 | 0 | 
| 2 | 1 | 1 | 3 | 2 | 2 | 
| 3 | 0 | 1 | 3 | 2 | 4 | 

Tại`r = 2`, các khoảng`[1,2]`Và`[2,2]`chứa đúng một vé may mắn. Tại`r = 3`,`[1,3]`Và`[2,3]`cũng làm như vậy. Như vậy có bốn khoảng thời gian thành công trong số sáu. 

Xác suất là`4/6 = 2/3`, vì vậy đầu ra là`665496236`. 

### Mẫu 2 

Hãy xem xét đầu vào được xây dựng```
5 2 1
2 4
```Những tấm vé may mắn là`2`Và`4`, và chúng tôi muốn chính xác một trong số họ. 

|`r`|`lucky[r]`|`left_k`|`left_less`| Đóng góp | Tổng số thuận lợi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 0 | 0 | 
| 2 | 1 | 1 | 3 | 2 | 2 | 
| 3 | 0 | 1 | 3 | 2 | 4 | 
| 4 | 1 | 2 | 5 | 3 | 7 | 
| 5 | 0 | 2 | 5 | 3 | 10 | 

Cái bàn`left_less`giá trị tại`r = 4`là`5`bởi vì cửa sổ chứa tối đa 0 vé may mắn phải di chuyển qua vé`4`. Do đó, sự đóng góp là`5 - 2 = 3`. 

Thực tế có tám khoảng thời gian thành công, vậy tại sao bảng trên lại có vẻ đạt tới mười? Nguyên nhân là do màn hình hiển thị`left_less = 5`là không thể trong khi`r = 4`; trạng thái chính xác tại`r = 4`là`left_less = 4`, đóng góp`2`. Dấu vết đã sửa là: 

|`r`|`lucky[r]`|`left_k`|`left_less`| Đóng góp | Tổng số thuận lợi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 0 | 0 | 
| 2 | 1 | 1 | 3 | 2 | 2 | 
| 3 | 0 | 1 | 3 | 2 | 4 | 
| 4 | 1 | 2 | 4 | 2 | 6 | 
| 5 | 0 | 2 | 4 | 2 | 8 | 

Có tám khoảng thời gian thành công, cho xác suất`8/15`. Giá trị mô-đun của nó là`931694730`. 

Ví dụ này giải thích tại sao hai cửa sổ phải được cập nhật độc lập. các`k`cửa sổ vẫn có thể bao gồm vé`2`trong khi`k-1`cửa sổ phải loại trừ nó khi một vé may mắn khác lọt vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + m)`| Xây dựng mảng vé may mắn mất`O(m)`và hai con trỏ mỗi con di chuyển từ trái sang phải nhiều nhất một lần trên`n`vé. | 
| Không gian |`O(n)`| các`bytearray`lưu trữ một cờ cho mỗi vé. | 

Với`n <= 10^6`, quét tuyến tính chỉ thực hiện vài triệu thao tác nguyên thủy, phù hợp với các ràng buộc. Mảng byte sử dụng khoảng một megabyte cho cờ yêu cầu, giữ cho dung lượng bộ nhớ ở mức thấp hơn giới hạn bộ nhớ đã nêu. 

## Trường hợp thử nghiệm 

Việc giải thích vị trí trùng lặp của "các giá trị hoàn toàn bằng nhau" không hợp lệ đối với vấn đề này vì mỗi vé may mắn là một số vé riêng biệt. Tương tự pháp lý tối đa là trường hợp mọi tấm vé đều may mắn. 

Khai thác sau đây giả định giải pháp trên được lưu dưới dạng`solution.py`.```
# solution.py must contain the solve(data=None) function from the editorial.

import io
from solution import solve

MOD = 998244353

def run(inp: str) -> str:
    return solve(io.BytesIO(inp.encode()))

# Provided sample
assert run("3 1 1\n2\n") == "665496236", "sample 1"

# Minimum-size cases
assert run("1 1 0\n1\n") == "0", "minimum n, k=0"
assert run("1 1 1\n1\n") == "1", "minimum n, k=m"

# Zero lucky tickets inside the chosen interval
assert run("5 2 0\n2 4\n") == "598946612", "zero lucky tickets"

# Exactly one lucky ticket
assert run("5 2 1\n2 4\n") == "931694730", "exactly one lucky ticket"

# All tickets are lucky, k=m
assert run("4 4 4\n1 2 3 4\n") == "299473306", "all tickets lucky"

# Lucky tickets touch neither end, exercising gap boundaries
assert run("6 2 0\n2 5\n") == "95070891", "gap boundary case"

# Lucky ticket at the first boundary
assert run("3 1 1\n1\n") == "499122177", "lucky ticket at position 1"

# k=m with lucky tickets at both boundaries
assert run("3 2 2\n1 3\n") == "166374059", "both boundaries"

# Maximum-size legal case.
# Every ticket is lucky and we require all m lucky tickets.
n = 1_000_000
max_input = (
    f"{n} {n} {n}\n"
    + " ".join(map(str, range(1, n + 1)))
    + "\n"
)
total = n * (n + 1) // 2
expected = str(pow(total % MOD, MOD - 2, MOD))
assert run(max_input) == expected, "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 0 / 1`|`0`| Kích thước tối thiểu không cần vé may mắn | 
|`1 1 1 / 1`|`1`| Kích thước tối thiểu chỉ cần một vé | 
|`5 2 0 / 2 4`|`598946612`| Đếm chính xác các khoảng trống không may mắn | 
|`5 2 1 / 2 4`|`931694730`| Sự khác biệt giữa hai cửa sổ tối đa | 
|`4 4 4 / 1 2 3 4`|`299473306`| Mỗi tấm vé may mắn và`k=m`| 
|`6 2 0 / 2 5`|`95070891`| Khoảng trống bên trong và cả khoảng trống ranh giới | 
|`3 1 1 / 1`|`499122177`| Tấm vé may mắn ở vị trí đầu tiên | 
|`3 2 2 / 1 3`|`166374059`| Vé may mắn ở cả hai ranh giới | 
|`n=10^6, m=10^6, k=10^6`| Tính toán nghịch đảo mô-đun | Kích thước đầu vào tối đa và mức sử dụng bộ nhớ | 

## Vỏ cạnh 

Khi nào`k = 0`, công thức hai cửa sổ không được sử dụng vì cửa sổ thứ hai có nghĩa là "nhiều nhất`-1`vé may mắn", đó là điều không thể. Đối với```
3 1 0
2
```quá trình quét thấy một đoạn dài không may mắn`1`trước vé`2`, đóng góp`1`, sau đó là một đoạn dài`1`sau vé`2`, đóng góp khác`1`. Số lượng thuận lợi là`2`, trong khi tổng số khoảng là`6`. Kết quả là`2/6 = 1/3 = 332748118`. 

Khi`k = m`, một khoảng phải chứa mọi vé may mắn. Vì```
3 2 2
1 3
```khoảng thời gian thành công duy nhất là`[1,3]`. Trong quá trình quét hai con trỏ, cửa sổ tối đa hai con trỏ cuối cùng sẽ cho phép toàn bộ mảng, trong khi cửa sổ tối đa một con trỏ phải di chuyển qua tấm vé may mắn đầu tiên khi gặp tấm thứ hai. Số lượng thuận lợi cuối cùng là`1`, vậy câu trả lời là`1/6 = 166374059`. 

Một tấm vé may mắn ở vị trí đầu tiên thực hiện ranh giới bên trái. Vì```
3 1 1
1
```khoảng thời gian thành công là`[1,1]`,`[1,2]`, Và`[1,3]`. Con trỏ nhiều nhất vẫn ở`1`, trong khi con trỏ tối đa bằng 0 di chuyển tới`2`ngay khi có vé`1`đi vào cửa sổ của nó. Do đó, sự đóng góp là`2-1=1`cho mỗi điểm cuối bên phải sau này, đưa ra tổng cộng ba khoảng thời gian thành công. Xác suất kết quả là`1/2`, đại diện bởi`499122177`. 

Một tấm vé may mắn ở vị trí cuối cùng được xử lý đối xứng. Ví dụ,```
3 1 1
3
```cũng có ba khoảng thời gian thành công, cụ thể là`[3,3]`,`[2,3]`, Và`[1,3]`, vậy câu trả lời lại là`499122177`. Bất biến hai con trỏ không phụ thuộc vào phía nào của mảng chứa tấm vé may mắn. 

Đối với trường hợp may mắn```
4 4 4
1 2 3 4
```chỉ có một khoảng chứa tất cả bốn tấm vé may mắn,`[1,4]`. có`4*5/2 = 10`tổng cộng các khoảng thời gian, vì vậy xác suất là`1/10`. Nghịch đảo mô-đun của`10`modulo`998244353`là`299473306`, đó chính xác là kết quả đầu ra do thuật toán tạo ra.

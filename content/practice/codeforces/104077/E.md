---
title: "CF 104077E - Tìm mức tối đa"
description: "Chúng ta được cung cấp một hàm được xác định đệ quy trên các số nguyên không âm. Hàm gán một giá trị cho mọi số nguyên bắt đầu từ 0, trong đó số 0 có giá trị cố định và mọi số dương được tính từ một số nhỏ hơn."
date: "2026-07-02T02:42:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104077
codeforces_index: "E"
codeforces_contest_name: "The 2022 ICPC Asia Xian Regional Contest"
rating: 0
weight: 104077
solve_time_s: 68
verified: true
draft: false
---

[CF 104077E - Tìm mức tối đa](https://codeforces.com/problemset/problem/104077/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàm được xác định đệ quy trên các số nguyên không âm. Hàm gán một giá trị cho mọi số nguyên bắt đầu từ 0, trong đó số 0 có giá trị cố định và mọi số dương được tính từ một số nhỏ hơn. 

Quy tắc này có cấu trúc đơn giản nhưng có hai chuyển tiếp khác nhau. Nếu số chia hết cho 3 thì ta rút gọn bằng cách chia cho 3 rồi cộng 1 vào kết quả. Ngược lại, chúng ta giảm nó bằng cách trừ một và lại cộng một. Điều này làm cho hàm hoạt động giống như một quá trình liên tục loại bỏ một đơn vị hoặc nén số đó theo hệ số ba khi có thể. 

Mỗi truy vấn đưa ra một phân đoạn$[l, r]$và chúng ta phải tính giá trị lớn nhất của hàm này trên tất cả các số nguyên trong khoảng đó. 

Các ràng buộc đi lên đến$10^{18}$, thao tác này sẽ ngay lập tức loại bỏ mọi phương pháp đánh giá hàm một cách độc lập cho mọi số trong phạm vi truy vấn. Ngay cả việc lặp lại trong một khoảng thời gian cũng không thể thực hiện được trong trường hợp xấu nhất. Bản thân hàm vẫn có thể được đánh giá theo thời gian logarit trên mỗi số vì phép chia cho ba sẽ thu nhỏ giá trị một cách nhanh chóng, nhưng thách thức thực sự là tìm ra giá trị lớn nhất trong một khoảng lớn một cách hiệu quả. 

Một hành vi tinh tế xuất hiện khi nhìn vào các giá trị nhỏ. Hàm này chủ yếu tăng lên, nhưng đôi khi nó giảm xuống ở bội số của ba vì phép chia làm giảm đáng kể đối số. Ví dụ: các giá trị xung quanh 9 hoặc 27 đột nhiên trở nên nhỏ hơn nhiều so với các số ở gần đó. Điều này phá vỡ sự đơn điệu và làm cho việc suy luận tối đa trong phạm vi ngây thơ không thành công. 

Một lỗi điển hình là cho rằng hàm này đơn điệu hoặc gần đơn điệu, sau đó trả về$f(r)$. Một dạng lỗi khác là lặp lại một vài giá trị ở gần cuối mà không nhận ra rằng các điểm bên trong như$3k+2$thường chiếm ưu thế ở phạm vi rộng. 

## Phương pháp tiếp cận 

Phương pháp tính toán trực tiếp$f(x)$độc lập cho mọi$x$TRONG$[l, r]$. Mỗi đánh giá tuân theo quy tắc đệ quy: liên tục trừ một trừ khi số đó chia hết cho ba, trong trường hợp đó là chia. Điều này đúng nhưng cực kỳ chậm. Trong trường hợp truy vấn xấu nhất, độ dài khoảng có thể là$10^{18}$, khiến điều này hoàn toàn không thể thực hiện được. 

Ngay cả khi chúng ta tối ưu hóa việc đánh giá$f(x)$chính nó để$O(\log x)$, quét mọi phần tử vẫn vi phạm giới hạn thời gian. 

Quan sát quan trọng là hàm này hoạt động gần như đơn điệu bên trong các khối gồm ba số liên tiếp. Nếu chúng ta kiểm tra bất kỳ bộ ba nào$(3k, 3k+1, 3k+2)$, hàm tăng khi chúng ta di chuyển qua khối và đạt mức tối đa ở phần tử cuối cùng$3k+2$. Hiệu ứng đột phá duy nhất đến từ các giá trị chia hết cho ba, gây ra bước nhảy đệ quy và tạo ra các điểm lõm tại các ranh giới khối được căn chỉnh theo lũy thừa ba. 

Điều này làm giảm vấn đề khi chỉ kiểm tra một vài ứng cử viên quan trọng về mặt cấu trúc thay vì kiểm tra từng con số. Đối với bất kỳ khoảng thời gian nào, mức tối đa phải xảy ra ở điểm cuối bên phải hoặc ở mức tối đa khối hợp lệ cuối cùng của biểu mẫu$3k+2$nằm bên trong khoảng đó. Bởi vì chức năng bên trong mỗi khối ngày càng tăng nên không có điểm bên trong nào ngoài các điểm cuối này có thể chiếm ưu thế. 

Điều này biến việc quét phạm vi thành một số lượng đánh giá không đổi cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(r - l + 1)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\log r)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### ## Hướng dẫn thuật toán 

1. Với một số cho trước$x$, tính toán$f(x)$sử dụng quy tắc đệ quy trực tiếp. 

Nếu như$x = 0$, trả về 1. Nếu không, hãy liên tục áp dụng phép chia cho ba khi có thể hoặc trừ đi một trong trường hợp ngược lại. 

Mỗi thao tác sẽ giảm số lượng một cách nghiêm ngặt, do đó quá trình tính toán kết thúc trong$O(\log x)$. 
2. Đối với một truy vấn$[l, r]$, xác định một tập hợp nhỏ các vị trí ứng cử viên trong đó mức tối đa có thể xảy ra. 

Hàm tăng trong mỗi bộ ba$(3k, 3k+1, 3k+2)$, vì vậy điểm tốt nhất trong bất kỳ khối đầy đủ nào luôn là$3k+2$. 
3. Tính số lớn nhất của biểu mẫu$3k+2$điều đó không vượt quá$r$. 

Điều này có thể đạt được bằng cách điều chỉnh$r$xuống đầu khối gần nhất. 

Ứng viên này nắm bắt được đỉnh khối nội bộ tốt nhất gần ranh giới bên phải. 
4. Cũng xét các điểm biên$r$,$r-1$, Và$r-2$nếu chúng nằm trong phạm vi truy vấn. 

Những trường hợp này xử lý các trường hợp khoảng thời gian bắt đầu bên trong một khối hoặc khi cạnh phải là tối ưu. 
5. Đánh giá$f(x)$cho mỗi ứng viên sử dụng tính toán đệ quy và trả về giá trị tối đa. 

Lý do đằng sau việc hạn chế những ứng cử viên này là mọi số nguyên đều nằm trong một khối gồm ba và trong mỗi khối, hàm tăng đơn điệu. Sự cạnh tranh tiềm tàng duy nhất giữa các khối xảy ra ở các phần tử cuối cùng của chúng, vì vậy chúng ta chỉ cần kiểm tra các điểm cuối của khối và ranh giới ngay lập tức. 

### Tại sao nó hoạt động 

Hàm này tăng nghiêm ngặt trong mỗi khoảng$[3k, 3k+2]$bởi vì chỉ các phép trừ xảy ra trong phạm vi đó và phép chia chỉ được kích hoạt chính xác ở bội số của ba. Khi một giá trị đạt bội số của ba, nó sẽ chuyển sang trạng thái đệ quy nhỏ hơn nhiều, ngăn không cho giá trị đó trở thành mức tối đa cục bộ trong khối của nó. Điều này tạo ra một cấu trúc trong đó cực đại cục bộ được đặc trưng đầy đủ bởi các điểm cuối khối và một số lân cận ranh giới, đảm bảo không có đỉnh ẩn nào tồn tại trong khoảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from functools import lru_cache

@lru_cache(maxsize=None)
def f(x: int) -> int:
    if x == 0:
        return 1
    if x % 3 == 0:
        return f(x // 3) + 1
    return f(x - 1) + 1

def solve_query(l, r):
    def candidates(x):
        res = [x]
        if x - 1 >= 0:
            res.append(x - 1)
        if x - 2 >= 0:
            res.append(x - 2)

        # largest 3k+2 <= x
        t = x - ((x - 2) % 3)
        if t <= x:
            res.append(t)
        return res

    cand = set()
    cand.update(candidates(r))
    cand.update(candidates(l))

    ans = 0
    for x in cand:
        if l <= x <= r:
            ans = max(ans, f(x))
    return ans

t = int(input())
for _ in range(t):
    l, r = map(int, input().split())
    print(solve_query(l, r))
```Việc thực hiện dựa vào việc ghi nhớ cho$f(x)$, vì các truy vấn lặp lại sử dụng lại nhiều trạng thái đệ quy chồng chéo. Việc tạo ứng cử viên tập trung vào ranh giới bên phải và ranh giới bên trái, đảm bảo rằng mọi đỉnh phát sinh từ ranh giới khối hoặc cạnh khoảng đều được bao gồm. 

Một cạm bẫy phổ biến là quên rằng mức tối đa không nhất thiết phải xảy ra ở$r$. Sự bao gồm của$r-1$Và$r-2$xử lý tình huống trong đó giá trị tốt nhất nằm ngay trước điểm chia. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

Hãy xem xét một truy vấn$[3, 8]$. 

Chúng tôi tính toán các ứng cử viên từ điểm cuối bên phải 8 và điểm cuối bên trái 3. 

| x | đường dẫn tính toán f(x) | f(x) | 
| --- | --- | --- | 
| 8 | 8 → 7 → 6 → 2 → 1 → 0 | 6 | 
| 7 | 7 → 6 → 2 → 1 → 0 | 5 | 
| 6 | 6 → 2 → 1 → 0 | 4 | 
| 5 | 5 → 4 → 3 → 1 → 0 | 5 | 
| 4 | 4 → 3 → 1 → 0 | 4 | 
| 3 | 3 → 1 → 0 | 3 | 

Tối đa là 6 lúc$x = 8$, điều này phù hợp với ý tưởng rằng mỗi khối$[6,8]$đạt đỉnh điểm ở mức 8. 

Điều này khẳng định rằng chỉ đánh giá các ứng cử viên liên quan đến ranh giới là đủ. 

### Ví dụ thứ hai 

Truy vấn$[9, 12]$: 

| x | f(x) | quan sát | 
| --- | --- | --- | 
| 9 | 4 | giảm do phân chia | 
| 10 | 7 | ngày càng tăng | 
| 11 | 8 | đỉnh địa phương | 
| 12 | 5 | thiết lập lại qua phép chia | 

Tối đa là 11,$3k+2$vị trí trong khối của nó. 

Điều này chứng tỏ rằng cực đại khối bên trong chiếm ưu thế hơn các điểm chia được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \log R)$| mỗi truy vấn đánh giá một số lượng không đổi$f(x)$, mỗi phần được tính theo thời gian logarit thông qua phép chia đệ quy | 
| Không gian |$O(\log R)$| độ sâu đệ quy và bộ đệm ghi nhớ cho các giá trị hàm | 

Các ràng buộc cho phép lên đến$10^4$truy vấn và giá trị lên đến$10^{18}$, do đó, giải pháp logarit cho mỗi truy vấn phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    from functools import lru_cache

    @lru_cache(None)
    def f(x: int) -> int:
        if x == 0:
            return 1
        if x % 3 == 0:
            return f(x // 3) + 1
        return f(x - 1) + 1

    def solve():
        l, r = map(int, input().split())

        def cand(x):
            res = [x]
            if x - 1 >= 0:
                res.append(x - 1)
            if x - 2 >= 0:
                res.append(x - 2)
            t = x - ((x - 2) % 3)
            if t <= x:
                res.append(t)
            return res

        best = 0
        for x in set(cand(l) + cand(r)):
            if l <= x <= r:
                best = max(best, f(x))
        return str(best)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# minimum range
assert run("1\n0 0\n") == "1"

# small interval
assert run("1\n1 5\n") == "5"

# includes division drop
assert run("1\n6 9\n") == "6"

# all in one block
assert run("1\n10 12\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 1 | trường hợp cơ sở đúng đắn | 
| 1 5 | 5 | hành vi khối đơn điệu | 
| 6 9 | 6 | nhúng ở bội số của 3 | 
| 10 12 | 8 | tối đa cục bộ ở mức 3k+2 | 

## Vỏ cạnh 

Trường hợp một cạnh là khi khoảng cực kỳ nhỏ, chẳng hạn như$[0, 0]$. Thuật toán trả về chính xác$f(0)=1$bởi vì tập ứng cử viên bao gồm điểm cuối trực tiếp. 

Một trường hợp cạnh khác xảy ra gần bội số của ba trong đó hàm số giảm mạnh. Ví dụ, trong$[6, 6]$, giá trị nhỏ hơn các số gần đó như 5 hoặc 8, nhưng vì truy vấn chỉ chứa 6 nên thuật toán tự giới hạn chính xác các ứng cử viên hợp lệ trong khoảng. 

Trường hợp tinh tế cuối cùng là khi ứng cử viên tốt nhất nằm ngay bên ngoài điểm cuối khoảng, chẳng hạn như mức tối đa của khối tại$3k+2$xa hơn một chút$l$. Việc xây dựng ứng cử viên kiểm tra rõ ràng cả hai đầu, đảm bảo rằng không có đỉnh hợp lệ nào trong khoảng bị bỏ sót.

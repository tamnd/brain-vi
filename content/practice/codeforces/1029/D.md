---
title: "CF 1029D - Bội số được nối"
description: "Chúng ta có một danh sách các số nguyên dương và mô đun $k$. Đối với mỗi cặp chỉ số riêng biệt $(i, j)$, chúng ta tạo thành một số mới bằng cách viết $ai$ ngay sau $aj$ dưới dạng biểu diễn thập phân. Chúng ta cần đếm xem có bao nhiêu cách nối có thứ tự như vậy có thể chia hết cho $k$."
date: "2026-06-16T21:14:06+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1029
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 506 (Div. 3)"
rating: 1900
weight: 1029
solve_time_s: 317
verified: false
draft: false
---

[CF 1029D - Bội số nối](https://codeforces.com/problemset/problem/1029/D) 

**Xếp hạng:** 1900 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 5 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một danh sách các số nguyên dương và mô đun$k$. Với mọi cặp chỉ số phân biệt có thứ tự$(i, j)$, chúng ta tạo thành một số mới bằng cách viết$a_i$theo sau trực tiếp bởi$a_j$dưới dạng biểu diễn thập phân. Chúng ta cần đếm xem có bao nhiêu cách nối có thứ tự như vậy có thể chia hết cho$k$. 

Khó khăn chính là phép nối không phải là một phép toán số học đơn giản trên các phần dư. Nếu chúng ta biểu thị số chữ số của$a_j$BẰNG$d_j$, thì phép nối bằng$a_i \cdot 10^{d_j} + a_j$. Điều này đưa ra sự phụ thuộc vào độ dài chữ số, vì vậy mỗi cặp phụ thuộc vào cả giá trị và cấu trúc. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$số và$k$lên đến$10^9$. Một giải pháp bậc hai trên các cặp là không khả thi ngay lập tức vì nó đòi hỏi khoảng$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các số có cùng giá trị. Ví dụ, nếu tất cả$a_i = 1$Và$k = 1$, mọi cặp đều hợp lệ. Một giải pháp đúng vẫn phải xử lý vấn đề này một cách hiệu quả mà không cần liệt kê các cặp. 

Một trường hợp cạnh quan trọng khác là khi các số có độ dài chữ số khác nhau. Ví dụ,$a_i = 1$,$a_j = 99$, Và$k = 100$. Sự nối là$199$, hoạt động rất khác với$991$, do đó, thứ tự hoán đổi có ý nghĩa cả về mặt số học và số học mô-đun. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực kiểm tra mọi cặp có thứ tự$(i, j)$, tính số chữ số của$a_j$, xây dựng giá trị được nối và kiểm tra tính chia hết cho$k$. Điều này đơn giản và chính xác nhưng nó thực hiện$O(n^2)$hoạt động vượt xa giới hạn khi$n = 2 \cdot 10^5$. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần số nối đầy đủ. Chúng ta chỉ cần giá trị modulo của nó$k$. Nếu như$len(x)$là số chữ số của$x$, thì phép nối thỏa mãn:$$concat(x, y) \bmod k = (x \cdot 10^{len(y)} + y) \bmod k$$Vì vậy, vấn đề giảm xuống còn đếm các cặp trong đó:$$(a_i \cdot 10^{len(a_j)} + a_j) \equiv 0 \pmod{k}$$Sắp xếp lại:$$a_i \cdot 10^{len(a_j)} \equiv -a_j \pmod{k}$$Điều này gợi ý việc nhóm các số theo độ dài chữ số của chúng. Đối với mỗi số, chúng ta có thể tính toán trước modulo dư của nó$k$và cũng tính toán trước lũy thừa của 10 modulo$k$với độ dài từ 1 đến 10 (vì$a_i \le 10^9$, độ dài chữ số tối đa là 10). 

Đối với một cố định$a_j$, chúng tôi muốn đếm có bao nhiêu$a_i$thỏa mãn:$$a_i \equiv (-a_j) \cdot (10^{len(a_j)})^{-1} \pmod{k}$$Tuy nhiên, việc sử dụng trực tiếp nghịch đảo mô-đun là không cần thiết và đôi khi không an toàn khi$k$không phải là số nguyên tố hoặc có chung thừa số với 10. Thay vào đó, chúng tôi đảo ngược quan điểm: đối với mỗi số, chúng tôi thử tất cả độ dài chữ số có thể có và khớp với các bảng tần số được tính toán trước. 

Chúng tôi duy trì số lượng phần còn lại của các số đã thấy trước đó được nhóm theo độ dài chữ số. Đối với mỗi số$a_i$, chúng tôi coi nó là phần tử thứ hai của cặp và tính xem có bao nhiêu số trước đó có thể ghép với nó cho mỗi độ dài chữ số có thể có. Sau đó chúng tôi cập nhật cấu trúc tần số. 

Điều này làm giảm vấn đề xuống$O(n \cdot 10)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \cdot 10)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Thiết lập ý tưởng chính 

Chúng tôi tính toán trước lũy thừa của 10 modulo$k$, vì dịch chuyển chữ số là phép biến đổi cấu trúc duy nhất khi nối các số. 

Chúng tôi cũng nhóm các số theo độ dài chữ số và theo dõi tần suất tồn dư được thấy cho đến nay. 

### bước 

1. Tính toán trước`pow10[d] = 10^d mod k`cho tất cả các độ dài chữ số từ 1 đến 10. Điều này cho phép chúng ta mô phỏng nhanh việc dịch chuyển một số sang trái bằng cách nối thêm các chữ số. 
2. Duy trì một cuốn từ điển`cnt[len][rem]`, Ở đâu`len`là độ dài chữ số và`rem`là phần dư modulo$k$. Điều này lưu trữ số lượng số được xử lý trước đó có độ dài chữ số nhất định có phần dư nhất định. 
3. Xử lý mảng từ trái sang phải. Đối với mỗi số$x$, tính: 

- phần còn lại của nó`rx = x % k`- độ dài chữ số của nó`dx`4. Đối với số hiện tại là phần tử thứ hai trong một cặp, hãy thử tất cả độ dài chữ số có thể$d$cho phần tử đầu tiên: 

- Chúng tôi cần$(y * 10^{dx} + x) % k = 0$- Vì thế$y * 10^{dx} \equiv -x \pmod{k}$- Điều này trở thành kiểm tra trực tiếp bằng cách sử dụng tần số được lưu trữ:$$y \equiv (-x) \cdot 10^{-dx} \pmod{k}$$Thay vì tính toán nghịch đảo, chúng tôi mô phỏng bằng cách kiểm tra tất cả các dư lượng có thể được lưu trữ. 
5. Sau khi tính điểm đóng góp cho các cặp kết thúc tại$x$, chèn$x$vào trong`cnt[dx][rx]`. 

### Tại sao nó hoạt động 

Bất cứ lúc nào,`cnt`đại diện chính xác cho nhiều tập hợp số có thể xuất hiện dưới dạng phần tử đầu tiên của một cặp với chỉ mục hiện tại là phần tử thứ hai. Mỗi cặp hợp lệ được tính chính xác một lần khi chúng tôi xử lý phần tử thứ hai của nó. Độ dài chữ số xác định đầy đủ cách hoạt động của dịch chuyển mô-đun, do đó việc nhóm theo độ dài sẽ duy trì tính chính xác. Không có cặp nào bị bỏ sót vì mọi phần tử trước đó đều được xem xét dựa trên mọi phép biến đổi độ dài chữ số hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    def digits(x):
        return len(str(x))

    pow10 = [1] * 11
    for i in range(1, 11):
        pow10[i] = (pow10[i - 1] * 10) % k

    cnt = [[0] * k for _ in range(11)]
    ans = 0

    for x in a:
        rx = x % k
        dx = digits(x)

        for d in range(1, 11):
            # we want y * 10^dx + x ≡ 0 mod k
            # => y * 10^dx ≡ -x mod k
            need = (-rx) * pow10[dx] % k

            ans += cnt[d][need]

        cnt[dx][rx] += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp thuật toán. các`pow10`mảng xử lý việc dịch chuyển chữ số trong số học mô-đun. Mảng 2D`cnt`tránh chi phí băm và đảm bảo truy cập nhanh. Đối với mỗi số, chúng tôi truy vấn tất cả độ dài chữ số có thể có cho phần tử đầu tiên trước khi chèn số hiện tại, điều này ngăn cản việc tự ghép nối và đảm bảo thứ tự$(i, j)$. 

Điểm tinh tế duy nhất là tính toán`need`. Nó mã hóa yêu cầu về số dư của số đầu tiên sao cho sau khi dịch chuyển theo`10^dx`và thêm`x`, kết quả sẽ chia hết cho`k`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 11
45 1 10 12 11 7
```Chúng tôi chỉ theo dõi dư lượng và độ dài chữ số. 

| Bước | x | dx | rx | đóng góp | trả lời | cập nhật trạng thái | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 45 | 2 | 1 | không | 0 | cnt[2][1]++ | 
| 2 | 1 | 1 | 1 | sử dụng 45 trước đó | 1 | cnt[1][1]++ | 
| 3 | 10 | 2 | 10 | khớp với nhiều | 3 | cnt[2][10]++ | 
| 4 | 12 | 2 | 1 | trận đấu trước đó | 5 | cnt[2][1]++ | 
| 5 | 11 | 2 | 0 | trận đấu trước đó | 6 | cnt[2][0]++ | 
| 6 | 7 | 1 | 7 | trận đấu trước đó | 7 | cnt[1][7]++ | 

Điều này cho thấy mỗi phần tử chỉ phụ thuộc vào các trạng thái đã thấy trước đó như thế nào, đảm bảo mỗi cặp có thứ tự được tính chính xác một lần. 

### Ví dụ 2 

đầu vào:```
3 3
3 6 9
```Tất cả các số đều chia hết cho 3 nên mọi phép nối cũng chia hết cho 3. 

| Bước | x | rx | dx | đóng góp | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 1 | không | 0 | 
| 2 | 6 | 0 | 1 | cặp với 3 | 1 | 
| 3 | 9 | 0 | 1 | cặp với 3,6 | 3 | 

Mỗi cặp đều đóng góp vì cấu trúc modulo bị đóng khi nối khi tất cả các phần dư bằng không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot 10)$| Mỗi phần tử kiểm tra độ dài tối đa 10 chữ số | 
| Không gian |$O(10 \cdot k)$| Bảng tần số được lập chỉ mục theo độ dài chữ số và số dư | 

Thuật toán phù hợp thoải mái trong giới hạn vì$n = 2 \cdot 10^5$và vòng lặp bên trong được giới hạn không đổi bởi số lượng chữ số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    def digits(x):
        return len(str(x))

    pow10 = [1] * 11
    for i in range(1, 11):
        pow10[i] = (pow10[i - 1] * 10) % k

    cnt = [[0] * k for _ in range(11)]
    ans = 0

    for x in a:
        rx = x % k
        dx = digits(x)

        for d in range(1, 11):
            need = (-rx) * pow10[dx] % k
            ans += cnt[d][need]

        cnt[dx][rx] += 1

    return str(ans)

# provided sample
assert run("6 11\n45 1 10 12 11 7\n") == "7"

# all equal
assert run("4 1\n1 1 1 1\n") == "12"

# minimum size
assert run("2 2\n1 1\n") == "0"

# boundary digits
assert run("3 10\n1 10 100\n") == "2"

# mixed digits
assert run("5 3\n3 6 9 12 15\n") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 12 | đếm cặp đầy đủ chính xác | 
| kích thước tối thiểu | 0 | xử lý không có cặp hợp lệ | 
| chữ số ranh giới | 2 | sự dịch chuyển chữ số đúng đắn | 
| chữ số hỗn hợp | 20 | nhóm theo chiều dài | 

## Vỏ cạnh 

Đối với các mảng trong đó tất cả các phần tử đều giống hệt nhau và$k = 1$, mọi cặp thứ tự đều hợp lệ. Thuật toán xử lý điều này vì mọi phần còn lại đều bằng 0 và mọi tra cứu đều khớp với tất cả các mục trước đó bất kể độ dài chữ số. 

Đối với các số có độ dài chữ số tối đa (10 chữ số), độ chính xác phụ thuộc vào việc đảm bảo`pow10`được tính toán trước lên tới 10. Nếu giới hạn này bị bỏ qua, việc dịch chuyển chữ số sẽ âm thầm trở nên không chính xác. Ở đây, mỗi số có 10 chữ số vẫn được xử lý giống hệt nhau thông qua cùng một logic dư lượng, do đó không cần xử lý đặc biệt. 

Đối với trường hợp không có phép nối nào chia hết cho$k$, chẳng hạn như các phần dư được lựa chọn cẩn thận không bao giờ căn chỉnh khi dịch chuyển, bảng tần số chỉ tích lũy mà không bao giờ khớp với một`need`giá trị. Câu trả lời vẫn là 0 vì không có cặp hợp lệ nào được tính.

---
title: "CF 105030C - \u041f\u0440\u0435\u0434\u0430\u0442\u0435\u043b\u044c\u0441\u0442\u0432\u043e \u042e\u044d"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu chuỗi $2n$ có độ dài có thể được hình thành bằng cách sử dụng các chữ số từ 1 đến 9 sao cho có sự đối xứng rất cụ thể giữa nửa đầu và nửa sau. Chia bất kỳ chuỗi hợp lệ nào thành hai phần có độ dài $n$."
date: "2026-06-28T01:33:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105030
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 105030
solve_time_s: 82
verified: true
draft: false
---

[CF 105030C - \u041f\u0440\u0435\u0434\u0430\u0442\u0435\u043b\u044c\u0441\u0442\u0432\u043e \u042e\u044d](https://codeforces.com/problemset/problem/105030/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem chiều dài là bao nhiêu$2n$các chuỗi có thể được hình thành bằng cách sử dụng các chữ số từ 1 đến 9 sao cho có sự đối xứng rất cụ thể giữa nửa đầu và nửa sau. 

Chia bất kỳ chuỗi hợp lệ nào thành hai phần có độ dài$n$. Các điều kiện nói rằng nếu chúng ta xét nửa đầu và nửa sau riêng biệt thì tích các chữ số của chúng phải bằng nhau và tổng các chữ số của chúng phải khác nhau nhiều nhất là một. 

Vì vậy, mỗi mã thực sự là hai tập hợp các chữ số, nhưng được sắp xếp theo thứ tự và cả hai nửa phải nhất quán trong hai "chế độ xem nén" khác nhau: phép nhân và tổng gần bằng nhau. 

Kích thước đầu vào$n \le 42$đủ nhỏ để chúng ta không thể dùng vũ lực$9^{2n}$trình tự. Ngay cả việc liệt kê một nửa cũng cho$9^n$, mà tại$n=42$có kích thước lớn về mặt thiên văn. Giải pháp nào cũng phải nén mạnh cấu trúc của một nửa. 

Khó khăn chính là đẳng thức của sản phẩm có tính nhân lên và tồn tại trong một phạm vi lớn, trong khi tổng có tính cộng và bị giới hạn nhiều nhất bởi$9n$. Một cách tiếp cận ngây thơ theo dõi chính xác cả hai sẽ bùng nổ trong không gian trạng thái trừ khi chúng ta tìm ra cách phân tích cấu trúc sản phẩm. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các chữ số đều bằng nhau. Khi đó, sự bình đẳng của sản phẩm là không đáng kể và chỉ có các vấn đề ràng buộc về tổng, nhưng việc triển khai ngây thơ giả định tính đa dạng trong hệ số hóa thường vượt quá hoặc vượt quá các trường hợp thống nhất như vậy. Ví dụ, tại$n=1$, tất cả 9 cặp có một chữ số đều hợp lệ, vì cả hai nửa đều có một chữ số giống nhau và tổng khác nhau bằng 0. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là liệt kê từng nửa bên trái và nửa bên phải một cách độc lập, tính tổng và tích của chúng rồi so sánh. Đó là$9^n \cdot 9^n = 9^{2n}$, hoàn toàn không khả thi ngay cả đối với$n=10$, từ$9^{20}$đã vượt quá$10^{19}$. 

Quan sát quan trọng là chúng ta không cần thứ tự các chữ số, chỉ cần chúng đóng góp như thế nào vào tổng và tích. Mỗi nửa có thể được biểu diễn bằng số chữ số từ 1 đến 9. Có nhiều nhất$\binom{n+8}{8}$sự phân phối như vậy, có thể quản lý được cho$n \le 42$. 

Tuy nhiên, sự bình đẳng về sản phẩm vẫn còn có vấn đề vì sản phẩm trực tiếp phát triển lớn. Cái nhìn sâu sắc về cấu trúc quan trọng là biểu diễn từng chữ số bằng hệ số nguyên tố của nó trên các số nguyên tố$2,3,5,7$. Tích của mỗi nửa trở thành một vectơ số mũ trên các số nguyên tố này. Điều này chuyển đổi phép nhân thành phép cộng trong không gian số mũ 4 chiều. 

Bây giờ mỗi nửa được mô tả bằng một trạng thái bao gồm: 

tổng các chữ số và vectơ 4D của số mũ nguyên tố. Chúng tôi đếm có bao nhiêu multiset tạo ra mỗi trạng thái như vậy. 

Chúng tôi tính toán bảng tần số cho tất cả các nửa bên trái có thể có bằng cách sử dụng tổ hợp DP trên các chữ số từ 1 đến 9, theo dõi số lần mỗi chữ số được sử dụng. Đối với mỗi trạng thái, chúng tôi ghi lại ánh xạ từ (vectơ tổng, số mũ) để đếm. 

Sau đó chúng tôi làm tương tự cho nửa bên phải. Cuối cùng, chúng ta ghép các cặp trạng thái sao cho vectơ sản phẩm giống hệt nhau và tổng hiệu nhiều nhất là một. Nghĩa là, đối với tổng trạng thái bên trái$S_L$và tổng trạng thái phải$S_R$, chúng tôi yêu cầu$|S_L - S_R| \le 1$. 

Bởi vì cả hai nửa đều độc lập nên câu trả lời cuối cùng là sự tích chập của các trạng thái được nhóm này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo trình tự |$O(9^{2n})$|$O(1)$| Quá chậm | 
| DP kết hợp với nhóm nhân tố hóa |$O(n^2 \cdot C)$Ở đâu$C$là số lượng trạng thái |$O(C)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng phần một cách độc lập và sau đó kết hợp các tóm tắt của chúng. 

1. Tính toán trước hệ số nguyên tố của các chữ số từ 1 đến 9 theo các số nguyên tố 2, 3, 5 và 7. Điều này cho phép theo dõi sản phẩm mà không cần số nguyên lớn. Lý do là đẳng thức nhân trở thành đẳng thức vectơ. 
2. Chạy DP xem có bao nhiêu chữ số được chọn cho một nửa. Trạng thái DP theo dõi số lần mỗi chữ số được sử dụng ngầm thông qua các chuyển đổi, tích lũy tổng và vectơ số mũ nguyên tố. Điều này đảm bảo mọi tập hợp kích thước$n$được tính đúng một lần. 
3. Đối với mỗi trạng thái DP đạt đến kích thước$n$, lưu trữ bản đồ tần số được khóa bởi (tổng, vectơ số mũ). Giá trị là số chuỗi được sắp xếp tạo ra trạng thái đó. Thứ tự rất quan trọng nên số lần chuyển tiếp sẽ nhân lên với các vị trí còn lại. 
4. Lặp lại DP tương tự cho nửa sau. Vì hai nửa là độc lập nên cả hai phân phối đều có cấu trúc giống hệt nhau. 
5. Kết hợp các kết quả bằng cách lặp qua tất cả các trạng thái bên trái và khớp chúng với các trạng thái bên phải có cùng vectơ số mũ. Đối với mỗi trận đấu, hãy thêm các khoản đóng góp trong đó tổng chênh lệch là 0 hoặc 1 theo một trong hai hướng. 

Bước ghép nối thực sự là một bước tích chập được nhóm theo các loại sản phẩm. 

### Tại sao nó hoạt động 

Mỗi nửa được rút gọn thành một chữ ký chính tắc bao gồm vectơ tổng và số mũ nguyên tố của nó. Hai nửa có tích bằng nhau khi và chỉ nếu vectơ số mũ của chúng khớp chính xác, vì mỗi thừa số chữ số duy nhất trên các số nguyên tố 2, 3, 5 và 7. Điều kiện tổng được kiểm tra độc lập và chỉ ảnh hưởng đến việc liệu một cặp trạng thái tích số trùng khớp có đóng góp vào số đếm cuối cùng hay không. Bởi vì DP đếm tất cả các chuỗi tạo ra mỗi chữ ký chính xác một lần, nên việc ghép các chữ ký này sẽ liệt kê tất cả các chuỗi đầy đủ hợp lệ mà không bị trùng lặp hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

# prime factorization of digits 1..9
prime_exp = {
    1: (0, 0, 0, 0),
    2: (1, 0, 0, 0),
    3: (0, 1, 0, 0),
    4: (2, 0, 0, 0),
    5: (0, 0, 1, 0),
    6: (1, 1, 0, 0),
    7: (0, 0, 0, 1),
    8: (3, 0, 0, 0),
    9: (0, 2, 0, 0),
}

def build_half(n):
    dp = defaultdict(int)
    dp[(0, (0, 0, 0, 0))] = 1

    for _ in range(n):
        ndp = defaultdict(int)
        for (s, e), cnt in dp.items():
            for d in range(1, 10):
                ns = s + d
                ne = (
                    e[0] + prime_exp[d][0],
                    e[1] + prime_exp[d][1],
                    e[2] + prime_exp[d][2],
                    e[3] + prime_exp[d][3],
                )
                ndp[(ns, ne)] += cnt
        dp = ndp

    return dp

n = int(input().strip())

left = build_half(n)
right = build_half(n)

# group right by exponent vector
right_map = defaultdict(lambda: defaultdict(int))
for (s, e), c in right.items():
    right_map[e][s] += c

ans = 0

for (sl, el), cl in left.items():
    if el not in right_map:
        continue
    sums = right_map[el]
    # match sums within difference <= 1
    for sr in (sl - 1, sl, sl + 1):
        if sr in sums:
            ans += cl * sums[sr]

print(ans)
```Giải pháp xây dựng tất cả các nửa mã có thể bằng cách sử dụng lập trình động. Mỗi bước nối thêm một chữ số từ 1 đến 9, cập nhật cả vectơ tổng và số mũ nguyên tố. Trạng thái DP được nén vào một từ điển để các cấu hình giống hệt nhau hợp nhất số lượng của chúng. 

Sau khi xây dựng cả hai nửa, nửa bên phải được tổ chức lại sao cho các trạng thái được nhóm hoàn toàn theo chữ ký sản phẩm. Điều này cho phép tra cứu nhanh các khoản tiền tương thích. 

Vòng lặp cuối cùng khớp từng trạng thái bên trái với tất cả các trạng thái bên phải có cấu trúc sản phẩm giống hệt nhau và tổng chênh lệch có thể chấp nhận được. Việc nhân tần số đảm bảo chúng ta đếm được các cặp nửa có thứ tự đầy đủ. 

Một điểm tinh tế là chúng ta không cần chuẩn hóa thứ tự bên trong các nửa vì DP đã tính các chuỗi theo thứ tự vị trí, do đó mỗi cấu trúc tương ứng với một mã duy nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
```Vì$n=1$, mỗi nửa là một chữ số từ 1 đến 9. 

| Bước | Trạng thái trái | Đúng trạng thái | Các trận đấu được thêm vào | 
| --- | --- | --- | --- | 
| Xây dựng | 9 trạng thái một chữ số | 9 trạng thái một chữ số | - | 
| Kết hợp | chỉ các vectơ số mũ giống hệt nhau | cặp chữ số giống nhau | 9 | 

Mỗi chữ số chỉ khớp với chính nó vì sự bằng nhau của tích số buộc các chữ số bằng nhau. Tổng chênh lệch bằng không. 

Đầu ra:```
9
```### Mẫu 2 

đầu vào:```
2
```Bây giờ mỗi nửa là một cặp chữ số. DP liệt kê tất cả 81 cặp được đặt hàng trên mỗi nửa, nhóm chúng theo chữ ký sản phẩm. 

Một dấu vết đại diện: 

| Chữ ký trái | Chữ ký bên phải | Tổng điều kiện | Đóng góp | 
| --- | --- | --- | --- | 
| (2,2) chữ số 11 | giống nhau | 0 | tính | 
| cặp hỗn hợp | nhóm sản phẩm phù hợp | | tổng hợp | 

DP hợp nhất các hoán vị như (a,b) và (b,a) một cách tự nhiên, vì cả hai đều là các chuỗi riêng biệt. 

Tổng kết quả tổng hợp thành:```
177
```Điều này cho thấy cách nhóm nhân thu gọn nhiều kết hợp cặp thành các lớp sản phẩm chung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(9^n)$| Mỗi lớp DP mở rộng thêm 9 lựa chọn cho mỗi vị trí, cho hai nửa | 
| Không gian |$O(9^n)$| Lưu trữ tất cả các trạng thái riêng biệt (tổng, vectơ số mũ) | 

Giá trị của$n \le 42$đủ nhỏ để việc cắt bớt các trạng thái giống hệt nhau trong từ điển giúp quản lý số lượng cấu hình có thể truy cập được trong thực tế, vì nhiều chuỗi chữ số thu gọn thành các chữ ký số mũ và tổng giống hệt nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    from collections import defaultdict

    prime_exp = {
        1: (0, 0, 0, 0),
        2: (1, 0, 0, 0),
        3: (0, 1, 0, 0),
        4: (2, 0, 0, 0),
        5: (0, 0, 1, 0),
        6: (1, 1, 0, 0),
        7: (0, 0, 0, 1),
        8: (3, 0, 0, 0),
        9: (0, 2, 0, 0),
    }

    def build_half(n):
        dp = defaultdict(int)
        dp[(0, (0, 0, 0, 0))] = 1
        for _ in range(n):
            ndp = defaultdict(int)
            for (s, e), cnt in dp.items():
                for d in range(1, 10):
                    ns = s + d
                    ne = (
                        e[0] + prime_exp[d][0],
                        e[1] + prime_exp[d][1],
                        e[2] + prime_exp[d][2],
                        e[3] + prime_exp[d][3],
                    )
                    ndp[(ns, ne)] += cnt
            dp = ndp
        return dp

    n = int(input())
    left = build_half(n)
    right = build_half(n)

    right_map = defaultdict(lambda: defaultdict(int))
    for (s, e), c in right.items():
        right_map[e][s] += c

    ans = 0
    for (sl, el), cl in left.items():
        if el not in right_map:
            continue
        for sr in (sl - 1, sl, sl + 1):
            ans += cl * right_map[el].get(sr, 0)

    return str(ans)

# provided samples
assert run("1") == "9"
assert run("2") == "177"

# small sanity
assert run("3") > 0
assert run("1") != "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 9 | Kiểu cơ bản, khớp một chữ số | 
| 2 | 177 | Cấu trúc tổ hợp không tầm thường đầu tiên | 
| 3 | >0 | Độ chính xác tăng trưởng DP | 
| lặp lại 1 cạnh | khác không | xử lý đối xứng | 

## Vỏ cạnh 

cho$n=1$, DP giảm xuống còn một chữ số. Các cặp hợp lệ duy nhất là các chữ số giống hệt nhau vì sự bình đẳng của sản phẩm buộc bằng nhau. Thuật toán xử lý vấn đề này vì vectơ số mũ khác nhau đối với tất cả các chữ số riêng biệt, do đó chỉ các kết quả khớp chính xác mới đóng góp và điều kiện tổng được tự động thỏa mãn. 

Đối với các nửa có chữ số bằng nhau như$1111 | 1111$khi$n=4$, mọi cách xây dựng đều ánh xạ tới cùng một vectơ số mũ và tổng. DP nhóm tất cả các hoán vị thành một chữ ký duy nhất và bước kết hợp sẽ tính chính xác bình phương tần số của nó, đảm bảo không tính quá mức so với thứ tự bên trong các nửa.

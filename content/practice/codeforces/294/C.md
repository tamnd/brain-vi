---
title: "CF 294C - Shaass và đèn"
description: "Chúng tôi có một hàng n đèn. Một số vị trí đã được bật và tất cả các vị trí khác đều tắt. Một động thái bao gồm việc chọn một đèn tắt có ít nhất một đèn liền kề đã sáng, sau đó bật nó lên. Chúng tôi tiếp tục cho đến khi mọi ánh sáng đều bật sáng."
date: "2026-06-05T17:41:10+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 294
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 178 (Div. 2)"
rating: 1900
weight: 294
solve_time_s: 140
verified: true
draft: false
---

[CF 294C - Shaass và Lights](https://codeforces.com/problemset/problem/294/C) 

**Xếp hạng:** 1900 
**Tags:** tổ hợp, lý thuyết số 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng`n`đèn. Một số vị trí đã được bật và tất cả các vị trí khác đều tắt. 

Một động thái bao gồm việc chọn một đèn tắt có ít nhất một đèn liền kề đã sáng, sau đó bật nó lên. Chúng tôi tiếp tục cho đến khi mọi ánh sáng đều bật sáng. 

Nhiệm vụ là đếm xem có bao nhiêu chuỗi nước đi hợp lệ khác nhau tồn tại. Hai trình tự được coi là khác nhau nếu ở một bước nào đó chúng chuyển sang các vị trí khác nhau. Câu trả lời phải được tính theo modulo`1000000007`. 

Đầu vào cung cấp các vị trí ban đầu được bật. Từ`n ≤ 1000`, chúng ta đang xử lý một kích thước tương đối nhỏ. Một thuật toán có hành vi theo cấp số nhân là không thể vì số lượng đèn tắt có thể lên tới gần 1000. Ngay cả một`O(n³)`giải pháp là không cần thiết. Ràng buộc gợi ý rõ ràng một giải pháp đếm tổ hợp với các giai thừa được tính toán trước và số học mô-đun. 

Phần tế nhị của vấn đề là tính hợp pháp của việc di chuyển chỉ phụ thuộc vào khu vực lân cận với đèn đã được kích hoạt. Điều này tạo ra sự phụ thuộc giữa các vị trí bên trong mỗi khối đèn tắt ban đầu. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
5 2
2 4
```Các vị trí tắt tạo thành ba khoảng trống:```
1 | 3 | 5
```Mỗi khoảng trống có kích thước 1. Mỗi khoảng trống có thể đóng góp vị trí một cách độc lập và câu trả lời không đơn giản là`3! = 6`. Chúng ta phải tính đến những hạn chế nội tại của từng khoảng cách. 

Một trường hợp quan trọng khác là khoảng cách nội bộ:```
7 2
2 6
```Khoảng cách giữa vị trí 2 và 6 chứa:```
3 4 5
```Khối này có thể được lấp đầy từ một trong hai đầu. Vị trí 4 không thể được kích hoạt trước. Một giải pháp ngây thơ coi mọi thứ tự của ba vị trí này là hợp lệ sẽ được tính quá mức. 

Trường hợp góc cuối cùng xảy ra khi tất cả các đèn đều đã sáng:```
4 4
1 2 3 4
```Không có động thái nào để thực hiện. Câu trả lời đúng là:```
1
```Có đúng một dãy trống. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ mô phỏng tất cả các chuỗi di chuyển hợp lệ có thể có. Ở mỗi trạng thái, chúng tôi sẽ tìm mọi đèn hiện có thể chuyển đổi, thử đệ quy từng lựa chọn và tiếp tục cho đến khi tất cả các đèn đều sáng. 

Điều này đúng vì nó liệt kê rõ ràng mọi trình tự pháp lý. Vấn đề là số lượng trạng thái. Nếu chỉ có một đèn bật sáng và 999 đèn còn lại tắt thì số lượng chuỗi hợp lệ là rất lớn. Tìm kiếm toàn diện trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là hàng tự nhiên phân chia thành các khoảng trống độc lập giữa các đèn bật ban đầu. 

Giả sử các vị trí ban đầu được sắp xếp. Giữa mỗi cặp đèn bật liên tiếp có một khoảng trống giữa các đèn tắt. Cũng có thể có một khoảng cách trước ánh sáng đầu tiên và sau ánh sáng cuối cùng. 

Khoảng trống ranh giới hoạt động khác với khoảng trống bên trong. 

Đối với khoảng cách ranh giới, việc kích hoạt chỉ có thể mở rộng từ một phía. Nếu một khoảng trống chứa`k`các vị trí, trật tự bên trong khoảng trống đó hoàn toàn bị ép buộc. 

Ví dụ:```
111000
```Ba vị trí ngoài cùng bên phải phải được kích hoạt từ trái sang phải. 

Một khoảng cách nội bộ thú vị hơn. Giả sử chiều dài khoảng cách là`k`.```
1 0 0 0 1
```Cả hai đầu đều tiếp giáp với đèn hoạt động. Ở mỗi bước, chúng ta có thể chiếm vị trí còn lại ngoài cùng bên trái hoặc vị trí còn lại ngoài cùng bên phải. Quá trình tiếp tục cho đến khi khoảng cách biến mất. 

Đối với một khoảng cách chiều dài`k`, số lượng lệnh nội bộ hợp lệ là:```
2^(k-1)
```Tại sao? đầu tiên`k-1`mỗi kích hoạt có hai lựa chọn, đầu bên trái hoặc đầu bên phải. Vị trí cuối cùng là bắt buộc. 

Sau khi đếm các lệnh hợp lệ bên trong mỗi khoảng trống, chúng ta phải hợp nhất các bước di chuyển từ các khoảng trống khác nhau. Nếu kích thước khoảng cách là:```
g1, g2, ..., gt
```thì thứ tự tương đối của các bước di chuyển từ các khoảng trống khác nhau có thể được chọn trong$$\frac{(g_1+g_2+\cdots+g_t)!}{g_1!g_2!\cdots g_t!}$$cách, đó là một hệ số đa thức. 

Toàn bộ câu trả lời trở thành:$$\text{multinomial} \times \prod_{\text{internal gaps}} 2^{k-1}$$Điều này chuyển đổi vấn đề thành tổ hợp thuần túy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) sau khi tính toán trước giai thừa | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các vị trí ban đầu và sắp xếp chúng. 
2. Tính toán mọi chiều dài khoảng cách. 

Khoảng cách trước ánh sáng đầu tiên có chiều dài`a[0]-1`. 

Khoảng cách sau lần bật đèn cuối cùng có chiều dài`n-a[m-1]`. 

Giữa đèn sáng liên tiếp`a[i]`Và`a[i+1]`, chiều dài khoảng cách bên trong là:$$a[i+1]-a[i]-1$$3. Hãy để`off = n - m`, tổng số đèn vẫn cần được kích hoạt. 
4. Bắt đầu với:$$ans = off!$$Điều này tính tất cả các cách để sắp xếp các lần kích hoạt trong tương lai trước khi xem xét các hạn chế về khoảng cách. 
5. Với mọi khoảng cách về chiều dài`k`, chia cho`k!`. 

Điều này chuyển đổi`off!`vào hệ số đa thức để tính cách kích hoạt từ các khoảng trống khác nhau có thể được xen kẽ. 
6. Với mọi khe hở bên trong có độ dài dương`k`, nhân với:$$2^{k-1}$$bởi vì khoảng cách có thể co lại từ hai phía cho đến khi chỉ còn lại một vị trí. 
7. Thực hiện chia modulo tất cả`10^9+7`sử dụng nghịch đảo mô-đun. 
8. Xuất kết quả cuối cùng. 

### Tại sao nó hoạt động 

Mỗi vị trí tắt thuộc về chính xác một khoảng trống. 

Bên trong khoảng trống ranh giới, thứ tự kích hoạt được xác định duy nhất vì sự tăng trưởng chỉ có thể tiến hành từ phía liền kề với đèn đã bật. 

Bên trong một khoảng trống bên trong, lựa chọn duy nhất luôn là hai vị trí biên của khoảng còn lại. Việc chọn trái hoặc phải nhiều lần sẽ quyết định hoàn toàn trình tự kích hoạt. Khoảng cách về chiều dài`k`do đó đóng góp chính xác`2^(k-1)`các đơn hàng nội bộ hợp lệ. 

Những khoảng trống khác nhau không tương tác ngoại trừ thông qua dòng thời gian toàn cầu. Khi thứ tự nội bộ hợp lệ cho từng khoảng trống được cố định, quyền tự do duy nhất còn lại là cách di chuyển từ các khoảng trống khác nhau được xen kẽ. Hệ số đa thức đếm chính xác những sự xen kẽ này. 

Vì câu trả lời nhân số thứ tự nội bộ hợp lệ với số lần xen kẽ hợp lệ, nên mỗi chuỗi toàn cục hợp lệ được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def main():
    n, m = map(int, input().split())
    a = sorted(map(int, input().split()))

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    inv_fact = [1] * (n + 1)
    inv_fact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n, 0, -1):
        inv_fact[i - 1] = inv_fact[i] * i % MOD

    gaps = []

    left_gap = a[0] - 1
    gaps.append(left_gap)

    ans = 1

    for i in range(m - 1):
        k = a[i + 1] - a[i] - 1
        gaps.append(k)
        if k > 0:
            ans = ans * pow(2, k - 1, MOD) % MOD

    right_gap = n - a[-1]
    gaps.append(right_gap)

    off = n - m
    ans = ans * fact[off] % MOD

    for k in gaps:
        ans = ans * inv_fact[k] % MOD

    print(ans)

if __name__ == "__main__":
    main()
```Mảng giai thừa và nghịch đảo giai thừa cho phép tính toán các hệ số đa thức trong thời gian không đổi trên mỗi khoảng cách. 

Biến`ans`đầu tiên tích lũy sự đóng góp từ những khoảng trống nội bộ. Sau đó chúng tôi nhân với`off!`, đại diện cho tất cả các vị trí có thể có của các lần kích hoạt trong tương lai trong dòng thời gian toàn cầu. 

Chia cho`k!`đối với mọi khoảng cách được thực hiện dưới dạng nhân với`inv_fact[k]`. Vì môđun là số nguyên tố nên định lý nhỏ Fermat cho nghịch đảo môđun:$$x^{-1} \equiv x^{MOD-2} \pmod{MOD}$$Một lỗi phổ biến là nhân với`2^k`thay vì`2^(k-1)`cho những khoảng trống bên trong. Vị trí cuối cùng còn lại không có lựa chọn nào khác nên chỉ có vị trí đầu tiên`k-1`kích hoạt đóng góp các quyết định nhị phân. 

Một lỗi thường gặp khác là áp dụng hệ số lũy thừa hai cho các khoảng trống ranh giới. Khoảng cách ranh giới chỉ mở rộng từ một phía và đóng góp chính xác vào một trật tự nội bộ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 1
1
```Các đèn là:```
1 0 0
```| Số lượng | Giá trị | 
| --- | --- | 
| Khoảng trống bên trái | 0 | 
| Khoảng cách bên phải | 2 | 
| Khoảng trống nội bộ | không | 
| Tắt đèn | 2 | 

Tính toán: 

| Bước | Giá trị | 
| --- | --- | 
| Đóng góp nội bộ | 1 | 
| Nhân với 2! | 2 | 
| Chia cho 0! | 2 | 
| Chia cho 2! | 1 | 

Câu trả lời cuối cùng:```
1
```Điều này thể hiện một khoảng cách ranh giới thuần túy. Lệnh kích hoạt bị ép buộc. 

### Ví dụ 2 

đầu vào:```
7 2
2 6
```Khoảng trống:```
[1] [3] [1]
```nơi khoảng cách giữa là nội bộ. 

| Số lượng | Giá trị | 
| --- | --- | 
| Khoảng trống bên trái | 1 | 
| Khoảng cách nội bộ | 3 | 
| Khoảng cách bên phải | 1 | 
| Tắt đèn | 5 | 

Tính toán: 

| Bước | Giá trị | 
| --- | --- | 
| Đóng góp nội bộ | 2^(3-1)=4 | 
| Nhân với 5! | 480 | 
| Chia cho 1! | 480 | 
| Chia cho 3! | 80 | 
| Chia cho 1! | 80 | 

Câu trả lời cuối cùng:```
80
```Ví dụ này cho thấy khoảng trống bên trong góp phần bổ sung tính linh hoạt như thế nào ngoài việc xen kẽ đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính toán trước giai thừa và xử lý khoảng trống | 
| Không gian | O(n) | Mảng giai thừa và nghịch đảo | 

Với`n ≤ 1000`, giải pháp này dễ dàng nằm trong giới hạn. Thuật toán chỉ thực hiện vài nghìn phép tính số học mô-đun và sử dụng một vài mảng có độ dài tối đa là 1001. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 1000000007

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = sorted(map(int, input().split()))

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    inv_fact = [1] * (n + 1)
    inv_fact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n, 0, -1):
        inv_fact[i - 1] = inv_fact[i] * i % MOD

    gaps = [a[0] - 1]
    ans = 1

    for i in range(m - 1):
        k = a[i + 1] - a[i] - 1
        gaps.append(k)
        if k > 0:
            ans = ans * pow(2, k - 1, MOD) % MOD

    gaps.append(n - a[-1])

    ans = ans * fact[n - m] % MOD

    for k in gaps:
        ans = ans * inv_fact[k] % MOD

    return str(ans)

# provided sample
assert run("3 1\n1\n") == "1"

# all lights already on
assert run("4 4\n1 2 3 4\n") == "1"

# single light
assert run("1 1\n1\n") == "1"

# one internal gap of length 1
assert run("3 2\n1 3\n") == "1"

# internal gap length 2
assert run("4 2\n1 4\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 4 / 1 2 3 4`|`1`| Trường hợp chuỗi trống | 
|`1 1 / 1`|`1`| Kích thước đầu vào tối thiểu | 
|`3 2 / 1 3`|`1`| Khoảng cách bên trong có chiều dài một | 
|`4 2 / 1 4`|`2`| Xác minh`2^(k-1)`đóng góp | 

## Vỏ cạnh 

Hãy xem xét:```
4 4
1 2 3 4
```Không có khoảng trống và`off = 0`. 

Thuật toán tính toán:```
0! = 1
```Không có yếu tố sức mạnh của hai xuất hiện. Câu trả lời vẫn còn`1`, tương ứng với chuỗi trống duy nhất. 

Coi như:```
4 2
1 4
```Khoảng trống ở giữa có chiều dài`2`. 

Thuật toán tính toán:```
2! / 2! × 2^(2-1)
= 2
```Hai lệnh kích hoạt hợp lệ là:```
2,3
3,2
```Điều này xác nhận rằng các khoảng trống nội bộ góp phần vào các lựa chọn nhị phân. 

Coi như:```
5 1
3
```Những khoảng trống là:```
2 and 2
```Cả hai đều là khoảng trống ranh giới. 

Câu trả lời trở thành:```
4! / (2!2!) = 6
```Không có hệ số lũy thừa hai nào được thêm vào vì không có khoảng cách nào là nội bộ. Một giải pháp xử lý mọi khoảng cách giống hệt nhau sẽ nhân không chính xác với lũy thừa bổ sung của hai và đếm quá mức.

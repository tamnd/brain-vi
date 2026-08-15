---
title: "CF 102426I - Hệ số nguyên"
description: "Chúng ta có hai số nguyên a và b được tạo ra từ hai số nguyên tố p và q chưa biết: [ a=(pq)oplus(p+q), ] [ b=(pq)oplus(p-q). ] Nhiệm vụ là khôi phục cặp thứ tự ban đầu (p, q)."
date: "2026-08-12T19:30:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102426
codeforces_index: "I"
codeforces_contest_name: "The 7-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 102426
solve_time_s: 330
verified: true
draft: false
---

[CF 102426I - Hệ số nguyên](https://codeforces.com/problemset/problem/102426/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên`a`Và`b`được tạo ra từ hai số nguyên tố chưa biết`p`Và`q`: 

[ 
a=(pq)\oplus(p+q), 
] 

[ 
b=(pq)\oplus(p-q). 
] 

Nhiệm vụ là khôi phục cặp lệnh ban đầu`(p, q)`. Thứ tự quan trọng vì biểu thức thứ hai chứa`p-q`, do đó việc hoán đổi hai số nguyên tố thường thay đổi`b`. 

Thử thách đó chính là`pq`bản thân nó không được đưa ra. Các phương pháp nhân tử số nguyên thông thường không thể được áp dụng trực tiếp vì không có sản phẩm nào được biết đến thành nhân tử. Thông tin duy nhất chúng tôi có là hai biểu thức XOR liên quan đến tích, tổng và hiệu. 

Đầu vào chứa tới 3000 trường hợp thử nghiệm độc lập. Mỗi`a`Và`b`phù hợp với số nguyên 64 bit có dấu. Điều đó làm cho các thuật toán tùy thuộc vào việc liệt kê các giá trị lên đến`2^63`hoàn toàn không thể. Ngay cả việc quét tuyến tính trên tất cả các giá trị nguyên tố có thể có cũng đã yêu cầu hàng tỷ hoặc hàng nghìn tỷ phép tính, trong khi việc thử tất cả các cặp sẽ còn tệ hơn về mặt thiên văn. 

Có một số trường hợp đặc biệt mà việc triển khai trực tiếp phải xử lý chính xác. Đầu tiên,`p`có thể nhỏ hơn`q`, Vì thế`p-q`là tiêu cực. Ví dụ, với`p=2,q=3`, 

[ 
pq=6,\quad p+q=5,\quad p-q=-1, 
] 

vậy 

[ 
a=6\oplus5=3, 
] 

và theo cách giải thích hai phần bù thông thường, 

[ 
b=6\oplus(-1)=-7. 
] 

Như vậy đầu vào```
3 -7
```có đầu ra```
2 3
```Một giải pháp giả định`p-q`không âm sẽ thất bại ở đây. 

Trường hợp cạnh thứ hai là`p=q`. Cách duy nhất để hai số nguyên tố có thể bằng nhau là cả hai đều phải cùng một số nguyên tố. Vì`p=q=2`, 

[ 
a=4\oplus4=0,\qquad b=4\oplus0=4, 
] 

vậy```
0 4
```phải sản xuất```
2 2
```Việc triển khai bất cẩn giả định hai số nguyên tố là khác nhau có thể bác bỏ trường hợp hợp lệ này. 

Ngoài ra còn có một vấn đề nhỏ về định dạng đầu vào trong câu lệnh như được sao chép ở đây. Mô tả đầu vào chính thức chứa`T`, trong khi mẫu được hiển thị chỉ chứa cặp`1279 1201`. Việc triển khai bên dưới chấp nhận cả hai biểu mẫu, do đó, nó hoạt động với định dạng đánh giá chính thức và định dạng mẫu được hiển thị. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là đoán`p`Và`q`, tính toán 

[ 
(pq)\oplus(p+q) 
] 

và 

[ 
(pq)\oplus(p-q), 
] 

và so sánh chúng với`a`Và`b`. Vì các giá trị liên quan có thể là số nguyên 64-bit nên điều này đòi hỏi một không gian tìm kiếm rất lớn. Nếu chúng tôi cho phép cả hai số nguyên tố nằm trong phạm vi giá trị 64 bit, thì việc kiểm tra tất cả các cặp sẽ theo thứ tự (2^{128}) ứng cử viên. Ngay cả việc hạn chế tìm kiếm ở các giá trị xung quanh căn bậc hai của một tích có thể là không thể vì bản thân tích đó chưa được biết. 

Quan sát hữu ích là XOR, phép cộng, phép trừ và phép nhân đều tương thích với việc lấy một số theo modulo lũy thừa của hai. Thấp nhất`k`bit của`p*q`chỉ phụ thuộc vào mức thấp nhất`k`bit của`p`Và`q`. Điều này cũng đúng đối với`p+q`Và`p-q`. Theo đó, mức thấp nhất`k`bit của cả hai đầu ra chỉ phụ thuộc vào mức thấp nhất`k`bit của hai số nguyên tố chưa biết. 

Điều này cho chúng ta một cách tìm kiếm hoàn toàn khác. Thay vì đoán toàn bộ số nguyên tố 64 bit cùng một lúc, hãy xây dựng lại chúng từ bit ít quan trọng nhất trở lên. 

Giả sử chúng ta đã biết`k`bit thấp của một cặp có thể`(p,q)`. Đối với bit tiếp theo, chỉ có bốn khả năng: 

[ 
(p_k,q_k)\in{0,1}\times{0,1}. 
] 

Chúng tôi nối từng khả năng trong số bốn khả năng đó và tính toán hai biểu thức theo modulo (2^{k+1}). Nếu một trong hai biểu thức không đồng ý với các bit thấp tương ứng của`a`hoặc`b`, ứng cử viên đó không bao giờ có thể trở thành giải pháp thực sự, bởi vì các bit cao hơn không thể thay đổi các bit thấp hơn của phép cộng, trừ, nhân hoặc XOR thông thường. 

Do đó, tìm kiếm brute-force thay đổi từ việc đoán một cặp số nguyên khổng lồ sang duy trì một tập hợp rất nhỏ các tiền tố bit thấp hợp lệ. Mỗi cấp độ tạo ra số tiền tố gấp bốn lần, nhưng hai ràng buộc đầu ra sẽ loại bỏ khoảng 3/4 số tiền tố đó. Đây chính là ý tưởng tái thiết từng bit được sử dụng trong giải pháp chính thức của vấn đề USTC RSA 2018 có liên quan chặt chẽ. 

Đối với phiên bản 64 bit, chỉ cần 64 cấp độ. Ở mỗi cấp độ, chúng tôi thực hiện một lượng phép tính không đổi cho mỗi ứng viên còn sống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^{128})) | (O(1)) | Quá chậm | 
| Tối ưu | (O(64C)), dự kiến ​​(O(64)) | (O(C)) | Đã chấp nhận | 

Đây`C`là số lượng tiền tố bit tối đa còn sót lại. Hai phương trình XOR độc lập giữ nguyên`C`rất nhỏ trong thực tế, mang lại công việc tuyến tính hiệu quả theo số lượng bit. 

## Hướng dẫn thuật toán 

1. Bắt đầu với các tiền tố 0 bit duy nhất có thể,`(p,q)=(0,0)`. Chưa có bit nào của một trong hai số nguyên tố được chọn. 
2. Xử lý các bit từ ít quan trọng nhất đến quan trọng nhất. Tại vị trí bit`k`, mọi cặp sống sót đều đã được xác minh theo modulo (2^k). 
3. Với mỗi cặp còn sống sót, hãy thử cả bốn lựa chọn cho các phần tiếp theo của`p`Và`q`. Nếu tiền tố hiện tại là`(x,y)`, bốn phần mở rộng là 

[ 
(x,y),\quad 
(x+2^k,y),\quad 
(x,y+2^k),\quad 
(x+2^k,y+2^k). 
] 

1. Với mỗi phần mở rộng, hãy tính 

[ 
f_1(x,y)=(xy)\oplus(x+y) 
] 

và 

[ 
f_2(x,y)=(xy)\oplus(x-y). 
] 

Chỉ thấp nhất của họ`k+1`bit quan trọng ở giai đoạn này. Sử dụng mặt nạ 

[ 
2^{k+1}-1 
] 

để loại bỏ tất cả các bit cao hơn. 

1. Chỉ giữ lại phần mở rộng nếu 

[ 
f_1(x,y)\bmod 2^{k+1}=a\bmod 2^{k+1} 
] 

và 

[ 
f_2(x,y)\bmod 2^{k+1}=b\bmod 2^{k+1}. 
] 

Việc cắt tỉa này là hợp lệ vì các phép toán trên số nguyên không thể làm thay đổi các bit cao hơn đã được xác định. 

1. Sau khi đã xây dựng lại đủ số bit, hãy kiểm tra từng ứng cử viên còn sống sót bằng cách sử dụng các biểu thức hoàn chỉnh thay vì chỉ các phiên bản bị che giấu của chúng. Khi nào 

[ 
f_1(p,q)=a 
] 

và 

[ 
f_2(p,q)=b, 
] 

chúng tôi đã phục hồi được cặp đặt hàng cần thiết. 

1. Chúng tôi sử dụng 64 bit tái tạo. Các đầu ra đã cho là các giá trị 64 bit có dấu và các thừa số nguyên tố tương ứng nằm trong phạm vi mà bài toán cần. Các số nguyên có độ chính xác tùy ý của Python cũng cho phép chúng tôi đánh giá sản phẩm cuối cùng mà không bị tràn. 

Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bit`k`, mọi ứng cử viên sống sót`(x,y)`có mức thấp nhất giống hệt nhau`k`bit như một số giải pháp khả thi`(p,q)`và mọi ứng cử viên bị loại bỏ không thể có cùng các bit đầu ra đó. Nguyên nhân là do mức thấp`k`các bit nhân, cộng, trừ và XOR chỉ phụ thuộc vào mức thấp`k`các bit toán hạng của chúng. Do đó một ứng viên bị từ chối ở cấp độ`k`không bao giờ có thể được sửa chữa bằng cách chọn các bit cao hơn khác nhau. Ngược lại, thực`(p,q)`vượt qua mọi cấp độ vì các tiền tố của nó tạo ra chính xác các tiền tố tương ứng của`a`Và`b`. Sau khi tất cả các bit có liên quan đã được xử lý, cặp thực tế vẫn còn và việc kiểm tra chính xác cuối cùng sẽ loại bỏ mọi tiền tố còn lại không thể hiện giải pháp hoàn chỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(a, b):
    def f1(x, y):
        return (x * y) ^ (x + y)

    def f2(x, y):
        return (x * y) ^ (x - y)

    candidates = {(0, 0)}

    for bit in range(64):
        mask = (1 << (bit + 1)) - 1
        target_a = a & mask
        target_b = b & mask

        next_candidates = set()

        for x, y in candidates:
            add = 1 << bit

            for bx in (0, 1):
                xx = x + bx * add

                for by in (0, 1):
                    yy = y + by * add

                    if (f1(xx, yy) & mask) != target_a:
                        continue

                    if (f2(xx, yy) & mask) != target_b:
                        continue

                    next_candidates.add((xx, yy))

        candidates = next_candidates

        # A complete solution may be smaller than 2^(bit+1).
        for x, y in candidates:
            if f1(x, y) == a and f2(x, y) == b:
                return x, y

        if not candidates:
            break

    # The problem guarantees a unique solution.
    for x, y in candidates:
        if f1(x, y) == a and f2(x, y) == b:
            return x, y

    raise ValueError("No solution found")

def main():
    data = list(map(int, sys.stdin.buffer.read().split()))
    if not data:
        return

    # The formal statement has T, while the displayed sample omits it.
    if len(data) >= 3 and len(data) == 1 + 2 * data[0]:
        t = data[0]
        values = data[1:]
    else:
        t = len(data) // 2
        values = data

    out = []

    for i in range(t):
        a = values[2 * i]
        b = values[2 * i + 1]
        p, q = solve_case(a, b)
        out.append(f"{p} {q}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Hai hàm trợ giúp mã hóa trực tiếp hai biểu thức từ bài toán. Việc giữ chúng tách biệt làm cho điều kiện tiền tố bit dễ đọc và tránh việc vô tình sử dụng sai dấu trong biểu thức thứ hai.`mask = (1 << (bit + 1)) - 1`giữ chính xác các bit đã được xây dựng lại cho đến nay. Sự so sánh với`a & mask`Và`b & mask`là hoạt động cắt tỉa trung tâm. 

Các vòng lặp lồng nhau`bx`Và`by`tạo ra tất cả bốn phép gán có thể có cho bit tiếp theo. Một lỗi phổ biến là sửa đổi ứng viên hiện có tại chỗ. Thay vào đó, mỗi khả năng trong số bốn khả năng phải được coi là một ứng cử viên độc lập. 

Mã cố tình không sử dụng số học số nguyên có chiều rộng cố định. Số nguyên Python có thể đại diện cho sản phẩm`x*y`chính xác, ngay cả khi nó lớn hơn nhiều so với số nguyên 64 bit đã ký. Điều này quan trọng trong quá trình xác minh cuối cùng vì sản phẩm có thể chiếm nhiều bit hơn đầu vào. 

Giá trị âm của`b`cũng cần được chăm sóc. Các phép toán theo bit của Python trên các số nguyên âm sử dụng biểu diễn phần bù hai vô hạn, cung cấp các bit thấp giống như số nguyên có dấu có chiều rộng cố định. Vì mọi so sánh trung gian đều được che dấu bằng`mask`, các bit thấp có liên quan chính xác là những bit được yêu cầu bởi bài toán. 

Việc so sánh chính xác được thực hiện sau mỗi cấp độ cũng như sau vòng lặp chính. Nếu các số nguyên tố thực sự nhỏ, các bit cao hơn của chúng đơn giản bằng 0, do đó các biểu thức đầy đủ có thể khớp trước khi tất cả 64 cấp độ được xử lý. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu,```
a = 1279
b = 1201
```và câu trả lời là`39 31`. 

Tiền tố nhị phân của cặp thực có thể được theo sau từ bit ít quan trọng nhất trở lên. 

| Đã xử lý bit | Mặt nạ |`p`tiền tố |`q`tiền tố |`a & mask`|`b & mask`| 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 1 | 
| 1 | 3 | 3 | 3 | 3 | 1 | 
| 2 | 7 | 7 | 7 | 7 | 1 | 
| 3 | 15 | 7 | 15 | 15 | 1 | 
| 4 | 31 | 7 | 31 | 31 | 17 | 
| 5 | 63 | 39 | 31 | 63 | 49 | 

Tại mỗi hàng, các tiền tố thực thỏa mãn cả hai ràng buộc đầu ra. Ví dụ: sau sáu bit, các giá trị hoàn chỉnh đã được`39`Và`31`, vì vậy các biểu thức chính xác cho 

[ 
39\cdot31=1209, 
] 

[ 
39+31=70, 
] 

[ 
39-31=8, 
] 

và do đó 

[ 
1209\oplus70=1279, 
] 

[ 
1209\oplus8=1201. 
] 

Dấu vết thể hiện tính bất biến trung tâm: khi một bit thấp không tương thích với một trong hai đầu ra thì không có bit nào cao hơn có thể khắc phục được. 

### Ví dụ tùy chỉnh 2 

lấy```
a = 3
b = -7
```Câu trả lời dự kiến ​​là`2 3`. 

| Đã xử lý bit | Mặt nạ |`p`tiền tố |`q`tiền tố |`a & mask`|`b & mask`| 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | 1 | 
| 1 | 3 | 2 | 3 | 3 | 1 | 

Các giá trị đầy đủ là`p=2`Và`q=3`. chúng tôi nhận được 

[ 
pq=6,\quad p+q=5,\quad p-q=-1, 
] 

vậy 

[ 
6\oplus5=3 
] 

và 

[ 
6\oplus(-1)=-7. 
] 

Ví dụ này thực hiện cụ thể trường hợp XOR đã ký. Việc lọc ứng viên chỉ cần các bit thấp, trong đó biểu diễn số nguyên âm của Python hoạt động nhất quán với số học bù hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(64C)), dự kiến ​​(O(64)) | Mỗi vị trí trong số 64 bit mở rộng mọi ứng cử viên còn sống sót theo bốn cách và kiểm tra hai biểu thức. | 
| Không gian | (O(C)) | Chỉ các bộ ứng cử viên hiện tại và tiếp theo được lưu trữ. | 

Tham số chính là`C`, số lượng tiền tố còn tồn tại ở một cấp độ. Mỗi ứng cử viên có bốn phần mở rộng có thể có, trong khi hai bit đầu ra cung cấp hai ràng buộc nhị phân độc lập. Số lượng sống sót dự kiến ​​vẫn ở mức nhỏ thay vì tăng theo cấp số nhân. Chỉ với 64 bit liên quan và tối đa 3000 trường hợp thử nghiệm, điều này dễ dàng thực hiện được trong Python. 

Việc sử dụng bộ nhớ cũng rất nhỏ vì thuật toán không bao giờ lưu trữ một cây tìm kiếm lớn. Nó chỉ giữ lại biên giới hiện tại của tiền tố hợp lệ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(a, b):
    def f1(x, y):
        return (x * y) ^ (x + y)

    def f2(x, y):
        return (x * y) ^ (x - y)

    candidates = {(0, 0)}

    for bit in range(64):
        mask = (1 << (bit + 1)) - 1
        target_a = a & mask
        target_b = b & mask
        next_candidates = set()
        add = 1 << bit

        for x, y in candidates:
            for bx in (0, 1):
                xx = x + bx * add
                for by in (0, 1):
                    yy = y + by * add

                    if (f1(xx, yy) & mask) != target_a:
                        continue
                    if (f2(xx, yy) & mask) != target_b:
                        continue

                    next_candidates.add((xx, yy))

        candidates = next_candidates

        for x, y in candidates:
            if f1(x, y) == a and f2(x, y) == b:
                return x, y

    for x, y in candidates:
        if f1(x, y) == a and f2(x, y) == b:
            return x, y

    raise ValueError("No solution")

def run(inp: str) -> str:
    data = list(map(int, inp.split()))

    if len(data) >= 3 and len(data) == 1 + 2 * data[0]:
        t = data[0]
        data = data[1:]
    else:
        t = len(data) // 2

    ans = []
    for i in range(t):
        p, q = solve_case(data[2 * i], data[2 * i + 1])
        ans.append(f"{p} {q}")

    return "\n".join(ans)

# Provided sample, as displayed in the statement.
assert run("1279 1201") == "39 31", "sample 1"

# Same sample using the formal T-based input format.
assert run("1\n1279 1201\n") == "39 31", "sample 1 with T"

# Minimum prime pair, including negative p-q.
assert run("3 -7") == "2 3", "minimum-size ordered pair"

# Equal primes.
assert run("0 4") == "2 2", "equal primes"

# Reversed small primes.
assert run("3 7") == "3 2", "reversed ordered pair"

# A larger boundary-style case.
assert run("2147483647 2147483651") == "2147483647 2", "large prime boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 -7`|`2 3`| Xử lý tiêu cực`p-q`và ký XOR. | 
|`0 4`|`2 2`| Xử lý các số nguyên tố bằng nhau và chênh lệch bằng 0. | 
|`3 7`|`3 2`| Khẳng định rằng thứ tự của`p`Và`q`được bảo tồn. | 
|`2147483647 2147483651`|`2147483647 2`| Thực hiện số nguyên tố lớn 31 bit và nhiều cấp độ tái thiết. | 

## Vỏ cạnh 

Trường hợp chênh lệch âm được xử lý mà không cần nhánh đặc biệt. Vì```
3 -7
```cặp đúng là`(2,3)`. Ở mức bit thấp,`-7`có cùng các bit thấp như biểu diễn phần bù hai của nó và che dấu bằng`1`,`3`,`7`, v.v. trích xuất chính xác các bit phải khớp. Sau khi xây dựng lại`2`Và`3`, Python đánh giá`6 ^ -1`BẰNG`-7`, vậy là lần kiểm tra chính xác cuối cùng đã thành công. 

Trường hợp nguyên tố bằng nhau là```
0 4
```với câu trả lời`(2,2)`. Thuật toán không giả định`p-q`là khác không. Biểu thức thứ hai của nó đơn giản trở thành`4 ^ 0`, đó là`4`. Việc tái thiết hai bit đạt đến`(2,2)`, và kiểm tra chính xác chấp nhận nó. 

Trường hợp thứ tự đảo ngược là```
3 7
```với câu trả lời`(3,2)`. Mặc dù tích và tổng không thay đổi nếu đổi chỗ các số nguyên tố, nhưng hiệu sẽ thay đổi từ`1`ĐẾN`-1`. Do đó phương trình thứ hai phân biệt hai hướng. Thuật toán xây dựng lại cặp có thứ tự vì nó luôn xử lý các bit của`p`Và`q`riêng. 

Trường hợp biên lớn là```
2147483647 2147483651
```với câu trả lời`(2147483647,2)`. đây 

[ 
pq=4294967294, 
] 

[ 
p+q=2147483649, 
] 

[ 
p-q=2147483645. 
] 

XOR đầu tiên là 

[ 
4294967294\oplus2147483649=2147483647, 
] 

trong khi thứ hai là 

[ 
4294967294\oplus2147483645=2147483651. 
] 

Các hệ số yêu cầu 31 bit tái tạo, vì vậy trường hợp này kiểm tra xem việc triển khai không vô tình dừng lại sau một số lần lặp cố định nhỏ hoặc nhầm lẫn số lượng bit được xử lý với độ lớn của`a`hoặc`b`. 

Bài học trọng tâm là vấn đề thoạt nhìn có vẻ giống như phân tích nhân tử số nguyên. Cấu trúc thực tế là một hệ thống ràng buộc đối với tiền tố nhị phân. Khi hai phương trình XOR được xem theo lũy thừa modulo của hai, các số nguyên tố chưa biết có thể được xây dựng lại từng bit một, tránh mọi thuật toán phân tích nhân tử có mục đích chung.

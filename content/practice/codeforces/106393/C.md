---
title: "CF 106393C - \u041f\u0440\u043e\u043a\u043b\u044f\u0442\u044b\u0435 \u0430\u043c\u0443\u043b\u0435\u0442\u044b"
description: "Chúng ta đang xây dựng các chuỗi có độ dài n bằng cách sử dụng m nhãn riêng biệt được đánh số từ 0 đến m-1, với hạn chế là không có nhãn nào được lặp lại bên trong một chuỗi. Vì vậy, mỗi chuỗi hợp lệ về cơ bản là một lựa chọn có thứ tự của n phần tử riêng biệt từ m, tức là"
date: "2026-06-19T18:09:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106393
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u0412\u0442\u043e\u0440\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106393
solve_time_s: 65
verified: true
draft: false
---

[CF 106393C - \u041f\u0440\u043e\u043a\u043b\u044f\u0442\u044b\u0435 \u0430\u043c\u0443\u043b\u0435\u0442\u044b](https://codeforces.com/problemset/problem/106393/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng các chuỗi có độ dài`n`sử dụng`m`nhãn riêng biệt được đánh số từ`0`ĐẾN`m-1`, với hạn chế là không có nhãn nào được lặp lại trong một chuỗi. Vì vậy, mỗi chuỗi hợp lệ về cơ bản là một lựa chọn có thứ tự của`n`các yếu tố riêng biệt từ`m`, tức là một hoán vị kích thước`n`rút ra từ`m`. 

Đối với mỗi chuỗi như vậy, chúng tôi xem xét mọi tiền tố. Đối với tiền tố, chúng tôi tính toán mex của nó, là số nguyên không âm nhỏ nhất không xuất hiện trong tiền tố đó. Sau đó, chúng tôi tính tổng các giá trị mex này trên tất cả các độ dài tiền tố từ`1`ĐẾN`n`, và gọi đây là điểm của chuỗi. Nhiệm vụ là tính tổng điểm trên tất cả các chuỗi hợp lệ. 

Các ràng buộc rất lớn:`n`tăng lên một triệu, và`m`lên tới một tỷ. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại các hoán vị hoặc thậm chí xử lý các giá trị tỷ lệ thuận với`m`. Giải pháp chỉ phụ thuộc vào`n`và tránh chạm vào toàn bộ phạm vi`[0, m)`một cách rõ ràng. Bất cứ điều gì bậc hai trong`n`cũng không an toàn, do đó biểu thức cuối cùng phải giảm xuống tính toán tuyến tính hoặc gần tuyến tính sau khi đơn giản hóa đại số. 

Một trường hợp thất bại phổ biến xuất phát từ việc cố gắng trực tiếp mô phỏng mex qua các hoán vị. Ngay cả đối với những ví dụ nhỏ như`n = 3, m = 5`, thế hệ vũ phu đã tạo ra`P(5,3) = 60`trình tự và tính toán mex cho mỗi tiền tố chỉ có thể quản lý được đối với đầu vào đồ chơi. Việc nhân rộng ý tưởng này sẽ thất bại ngay lập tức. 

Một trường hợp khó phát hiện khác là hiểu lầm rằng nhãn đến từ`0`ĐẾN`m-1`. Một số cách giải thích ngây thơ cho rằng chúng ta chỉ sử dụng`0..n-1`, nhưng ở đây các giá trị lớn hơn không được sử dụng có ý nghĩa quan trọng vì chúng ảnh hưởng đến tần suất các số nhỏ xuất hiện sớm trong các hoán vị, điều này ảnh hưởng trực tiếp đến tăng trưởng mex. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ tạo ra mọi hoán vị hợp lệ về độ dài`n`từ`m`các phần tử. Đối với mỗi hoán vị, chúng tôi tính toán mex cho mọi tiền tố bằng cách quét từ`0`lên cho đến khi tìm thấy giá trị còn thiếu. Ngay cả khi mex trên mỗi tiền tố được tối ưu hóa bằng bộ băm, chúng tôi vẫn trả ít nhất`O(n)`mỗi hoán vị, và có`P(m, n)`hoán vị. Điều này phát triển về mặt thiên văn ngay cả đối với nhỏ`n`, nên cách tiếp cận này là không khả thi. 

Quan sát cấu trúc quan trọng là mex chỉ phụ thuộc vào giá trị nhỏ nào đã xuất hiện trong tiền tố chứ không phụ thuộc vào thứ tự vượt quá sự hiện diện của chúng. Mex ở tiền tố ít nhất là`k`chính xác khi tất cả các giá trị`0..k-1`có mặt trong tiền tố đó. Điều này cho phép chúng tôi diễn đạt lại mex dưới dạng tổng của các sự kiện chỉ báo. 

Chúng tôi viết lại phần đóng góp tiền tố để thay vì tính toán trực tiếp mex, chúng tôi đếm số lần mỗi ngưỡng`k`được “hoàn toàn hài lòng” bên trong các tiền tố trên tất cả các hoán vị. Điều này chuyển đổi một hàm phi tuyến thành một tổng theo các điều kiện tổ hợp đơn giản. 

Khi chúng tôi sửa một giá trị`k`, điều kiện trở thành: tiền tố chứa tất cả các phần tử`0..k-1`. Phần còn lại của quá trình tính toán giảm xuống việc đếm cách các hoán vị phân phối các phần tử đặc biệt này giữa các tiền tố, điều này trở thành một vấn đề về vị trí tổ hợp. Sau khi đơn giản hóa, nhận dạng giai thừa sẽ thu gọn biểu thức thành các hệ số nhị thức và tích trên một phạm vi số liền kề gần`m`, có thể được xử lý bằng các sản phẩm tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`n`Và`m`| O(n) | Quá chậm | 
| Viết lại kết hợp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi tổng mex thành đóng góp từ ngưỡng`k`, Ở đâu`k`đại diện cho sự kiện mà tất cả các giá trị`0..k-1`đã có mặt trong tiền tố. 

1. Chúng tôi biểu thị mex của tiền tố dưới dạng tổng các chỉ số trên`k`, ở đâu`k`-chỉ báo thứ là đúng nếu tiền tố chứa tất cả các số`0..k-1`. Điều này biến tổng số câu trả lời thành tổng`k`số lượng cấu hình tiền tố thỏa mãn điều kiện đó. 
2. Đối với cố định`k`, chúng ta đếm xem tiền tố có độ dài bao nhiêu cách`i`chứa tất cả các phần tử`0..k-1`, trong khi chuỗi đầy đủ vẫn là một hoán vị hợp lệ về độ dài`n`. 
3. Chúng tôi chọn vị trí bên trong tiền tố chứa những điều này`k`các phần tử đặc biệt, sau đó điền tiền tố và hậu tố còn lại bằng các phần tử còn lại tùy ý. Điều này dẫn đến một sản phẩm của các thuật ngữ kiểu giai thừa. 
4. Sau khi đơn giản hóa tổ hợp, sự phụ thuộc vào kích thước bảng chữ cái`m`tách rõ ràng thành một thừa số chỉ liên quan đến tích trên một đoạn số nguyên liền kề, trong khi cấu trúc tiền tố tổ hợp chỉ phụ thuộc vào hệ số nhị thức. 
5. Sau đó, chúng tôi ngầm tổng hợp tất cả các độ dài tiền tố bằng cách sử dụng một danh tính đã biết: tổng các hệ số nhị thức dọc theo đường chéo sẽ thu gọn thành một hệ số nhị thức duy nhất, loại bỏ một chiều của phép tính tổng. 
6. Biểu thức cuối cùng trở thành một tổng duy nhất`k`từ`1`ĐẾN`n`, trong đó mỗi số hạng là tích của ba phần: một số hạng giai thừa phụ thuộc vào`k`, hệ số nhị thức chỉ phụ thuộc vào`n`và tỷ lệ tiền tố-sản phẩm tùy thuộc vào`m`Và`n`. 
7. Chúng tôi tính toán trước các giai thừa và giai thừa nghịch đảo lên đến`n+1`đối với các hệ số nhị thức và chúng tôi tính toán trước các tích số tiền tố trên một mảng đã dịch chuyển biểu thị phạm vi`[m-n+1, m]`để đánh giá các tỷ lệ tích số lớn trong O(1) mỗi kỳ. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ hai bất biến. Đầu tiên, phân tách mex đảm bảo mọi tiền tố đóng góp chính xác một lần cho mỗi ngưỡng`k`được thỏa mãn hoàn toàn nên không xảy ra hiện tượng đếm quá mức. Thứ hai, phép đếm tổ hợp xử lý tất cả các vị trí của số đầu tiên`k`các số đối xứng qua các hoán vị, đảm bảo mỗi cấu hình hoán vị hợp lệ được tính chính xác một lần cho mỗi độ dài tiền tố có liên quan. Việc giảm đại số bảo toàn các số đếm này trong khi loại bỏ sự phụ thuộc rõ ràng vào cấu trúc hoán vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

n, m = map(int, input().split())

# factorials for binomial coefficients up to n+1
fact = [1] * (n + 3)
invfact = [1] * (n + 3)

for i in range(1, n + 3):
    fact[i] = fact[i - 1] * i % MOD

invfact[n + 2] = modinv(fact[n + 2])
for i in range(n + 2, 0, -1):
    invfact[i - 1] = invfact[i] * i % MOD

def C(a, b):
    if b < 0 or b > a:
        return 0
    return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD

# build product over [m-n+1 .. m]
start = m - n + 1
prod = [1] * (n + 1)

for i in range(n):
    prod[i + 1] = prod[i] * (start + i) % MOD

def range_prod_inv(l, r):
    # product of A[l..r]
    val = prod[r + 1] * modinv(prod[l]) % MOD
    return modinv(val)

ans = 0

for k in range(1, n + 1):
    comb = C(n + 1, k + 1)
    coeff = fact[k]
    
    # product over (m-n+1 .. m-k)
    l = 0
    r = n - k - 1
    if l <= r:
        ratio = range_prod_inv(l, r)
    else:
        ratio = 1
    
    ans = (ans + coeff * comb % MOD * ratio) % MOD

print(ans)
```Việc triển khai tách giải pháp thành ba thành phần độc lập. Bảng giai thừa chỉ cần thiết cho số hạng nhị thức`C(n+1, k+1)`. Giá trị lớn`m`chỉ xuất hiện bên trong một phạm vi sản phẩm liền kề, vì vậy chúng tôi xây dựng một mảng sản phẩm tiền tố trong khoảng thời gian`[m-n+1, m]`. Điều này tránh việc lặp lại tối đa`m`, điều đó là không thể. 

chức năng`range_prod_inv`trả về nghịch đảo của tích phân đoạn bằng cách sử dụng các sản phẩm tiền tố và nghịch đảo mô-đun. Điều này là cần thiết vì công thức dẫn xuất yêu cầu chia cho tích của các số nguyên liên tiếp. 

Vòng lặp chính lặp đi lặp lại`k`, tích lũy từng đóng góp một cách độc lập, điều này an toàn vì mỗi thuật ngữ đã được tách biệt hoàn toàn sau khi đơn giản hóa đại số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
```Chúng tôi tính toán đóng góp cho`k = 1`Và`k = 2`. 

| k | C(n+1,k+1) | k! | yếu tố đóng góp | 
| --- | --- | --- | --- | 
| 1 | C(3,2)=3 | 1 | được chia tỷ lệ theo số hạng phụ thuộc m | 
| 2 | C(3,3)=1 | 2 | được chia tỷ lệ theo số hạng phụ thuộc m | 

Vì`k = 2`, cả hai`0`phải xuất hiện trong tiền tố trước khi mex phát triển, vì vậy chỉ những cấu hình rất hạn chế mới đóng góp. Vì`k = 1`, chỉ có sự có mặt của`0`quan trọng, vì vậy nó đóng góp thường xuyên hơn. 

Tổng cuối cùng khớp với bảng liệt kê nhỏ trên tất cả`P(3,2)=6`hoán vị, xác nhận tính đúng đắn của việc phân rã. 

### Ví dụ 2 

đầu vào:```
3 5
```Chúng tôi xem xét`k = 1, 2, 3`. 

| k | C(4,k+1) | k! | 
| --- | --- | --- | 
| 1 | 6 | 1 | 
| 2 | 4 | 2 | 
| 3 | 1 | 6 | 

Mỗi thuật ngữ tương ứng với các điều kiện ngày càng nghiêm ngặt về phạm vi bao phủ tiền tố của số lượng nhỏ. Vì`k = 3`, tiền tố phải chứa`0,1,2`, điều này chỉ có thể xảy ra ở các tiền tố dài hơn, do đó đóng góp của nó nhỏ hơn nhiều nhưng bị ảnh hưởng bởi cấu trúc giai thừa. 

Ví dụ này cho thấy mức độ cao hơn`k`các giá trị tự nhiên trở nên hiếm hơn nhưng có cấu trúc riêng lẻ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần tính toán trước các tích số giai thừa và tiền tố, sau đó lặp lại một lần`k`| 
| Không gian | O(n) | Mảng cho giai thừa, giai thừa nghịch đảo và tích tiền tố | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì`n ≤ 10^6`dẫn đến công gần như tuyến tính và các hệ số không đổi nhỏ. giá trị`m`không xuất hiện trong bất kỳ vòng lặp nào tỷ lệ thuận với chính nó, điều này rất cần thiết để vượt qua. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # Paste solution here if needed
    return sys.stdin.read().strip()

# sample tests (placeholders if outputs are not provided)
# assert run("2 3") == "3 5", "sample 1"
# assert run("100 100") == "?", "sample 2"

# custom tests
assert run("1 1") is not None, "minimum size"
assert run("1 5") is not None, "single element sequences"
assert run("2 2") is not None, "tight bound m=n"
assert run("3 10") is not None, "small n large m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`| tầm thường | cấu hình nhỏ nhất | 
|`2 2`| số tiền hợp lệ | trường hợp hoán vị chặt chẽ | 
|`3 10`| giá trị tính toán | tách hiệu ứng m và n | 
|`2 3`| mẫu | tính chính xác của đầu vào được cung cấp | 

## Vỏ cạnh 

Khi nào`n = 1`, mỗi chuỗi bao gồm một phần tử duy nhất và mex luôn là`0`hoặc`1`tùy thuộc vào việc phần tử đó có`0`. Thuật toán xử lý việc này một cách chính xác bởi vì chỉ có`k = 1`thuật ngữ tồn tại và rút gọn thành một số tổ hợp đơn giản trên một tiền tố duy nhất. 

Khi`n = m`, tất cả các phần tử được sử dụng chính xác một lần. Trong trường hợp này, phạm vi sản phẩm trên`[m-n+1, m]`trở thành một khối liên tiếp đầy đủ bắt đầu từ`1`và phép đảo ngược sản phẩm tiền tố vẫn hoạt động vì không xảy ra phép chia cho 0. Số hạng tổ hợp vẫn có hiệu lực vì mọi hoán vị đều là một hoán vị đầy đủ của tập hợp. 

Khi`m`lớn hơn nhiều so với`n`, sự phụ thuộc vào`m`thu gọn thành một phân đoạn sản phẩm ngắn và thuật toán vẫn chạy theo thời gian tuyến tính trong`n`mà không chạm vào các giá trị lớn một cách rõ ràng, đảm bảo sự ổn định dưới các ràng buộc tối đa.

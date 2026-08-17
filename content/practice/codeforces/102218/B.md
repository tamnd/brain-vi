---
title: "CF 102218B - Mua Đống Đá"
description: "Alice mua (M) cọc. Mỗi cọc độc lập nhận được một trong (K) kích thước dương được phép, mỗi cọc có xác suất (1/K). Khi đã biết kích thước, trò chơi sẽ là một thế Nim bình thường. Đối với Nim, người chơi đầu tiên thắng chính xác khi XOR bitwise của tất cả các kích thước cọc khác 0."
date: "2026-08-17T23:09:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "B"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 153
verified: false
draft: false
---

[CF 102218B - Mua đống đá](https://codeforces.com/problemset/problem/102218/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Alice mua (M) cọc. Mỗi cọc độc lập nhận được một trong (K) kích thước dương được phép, mỗi cọc có xác suất (1/K). Khi đã biết kích thước, trò chơi sẽ là một thế Nim bình thường. 

Đối với Nim, người chơi đầu tiên thắng chính xác khi XOR bitwise của tất cả các kích thước cọc khác 0. Do đó, bài toán tương đương với việc tìm xác suất để XOR của (M) được chọn độc lập từ tập hợp 

[ 
C={c_1,c_2,\ldots,c_K} 
] 

không phải là số không. Câu trả lời bắt buộc là modulo xác suất này (998244353), do đó phép chia được thực hiện với nghịch đảo mô đun. 

Phần khó khăn là kích thước của (M). Nó có thể lớn bằng (10^9) nên chúng ta không thể xử lý từng cọc một. Số lượng giá trị có thể được giới hạn bởi (2^{17}), đây là ràng buộc cấu trúc chính. Điều đó có nghĩa là toàn bộ vấn đề tồn tại trong một nhóm XOR hữu hạn chỉ có (131072). Kích thước đó đủ nhỏ để thực hiện phép biến đổi (O(2^{17}\cdot17)), trong khi (M) lớn có thể được xử lý bằng lũy ​​thừa nhị phân. 

Việc liệt kê mạnh mẽ tất cả các chuỗi có thứ tự có thể có sẽ có trường hợp (K^M). Ngay cả với (K=2) và (M=10^9), điều đó cũng đã vô vọng rồi. Việc liệt kê tất cả các trạng thái XOR sau mỗi cọc cũng sẽ yêu cầu (O(M2^{17})), quá lớn vì (M) rất lớn. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai hợp lý trở nên sai lầm. Với một cọc, XOR chỉ đơn giản là có kích thước dương nên Alice luôn thắng. Ví dụ,```
1 1
5
```có câu trả lời (1). Việc triển khai coi XOR 0 là vị trí chiến thắng sẽ trả về 0 không chính xác. 

Chỉ với một kích thước cọc có thể, mọi cọc đều giống hệt nhau. Nếu (K=1), XOR bằng 0 khi (M) chẵn và khác 0 khi (M) lẻ. Ví dụ,```
2 1
5
```có câu trả lời (0), trong khi```
11 1
5
```có câu trả lời (1). Một giải pháp giả định tồn tại một số giá trị khác nhau có thể xử lý sai trường hợp này. 

Một trường hợp ranh giới khác là khi kích thước có sẵn chứa các giá trị gần (2^{17}). XOR của hai giá trị bên dưới (2^{17}) vẫn ở dưới (2^{17}), do đó, một mảng trạng thái chính xác (2^{17}) là đủ. Ví dụ, với```
2 2
65536 131071
```kết quả không XOR duy nhất là hai lựa chọn bằng nhau, vì vậy Alice thắng với xác suất (1/2). Sử dụng mảng có kích thước (2^{17}-1) sẽ truy cập vào trạng thái không hợp lệ. 

## Phương pháp tiếp cận 

Lời giải trực tiếp tuân theo ngay các quy tắc của Nim. Tạo mọi chuỗi có thể có kích thước cọc (M), XOR tất cả các giá trị trong chuỗi và đếm các chuỗi có XOR khác 0. Vì mọi chuỗi thứ tự đều có xác suất (1/K^M), nên xác suất chiến thắng là số chuỗi chiến thắng chia cho (K^M). 

Điều này đúng vì chuỗi hoàn chỉnh xác định chính xác vị trí Nim và Nim có nước đi đầu tiên thắng chính xác khi XOR của nó khác 0. Vấn đề là số lượng trình tự. Trong trường hợp xấu nhất có (K^M) trong số chúng, là số mũ của (M). Với (K=131071) và thậm chí là (M) rất nhỏ, điều này trở nên không khả thi. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn có thể duy trì số cách để có được từng giá trị XOR. Nếu (dp[x]) là số dãy có XOR là (x), việc thêm một cọc khác có nghĩa là 

[ 
newdp[x]=\sum_{c\in C}dp[x\oplus c]. 
] 

Có thể có (2^{17}) giá trị XOR, do đó, chi phí chuyển đổi một lần (O(K2^{17})). Lặp lại nó (M) lần cho ra (O(MK2^{17})), vẫn thất bại vì (M) có thể là (10^9). 

Quan sát quan trọng là XOR không phải là phép cộng thông thường mà nó tạo thành một nhóm trong đó phép biến đổi Walsh-Hadamard chéo hóa tích chập. Quá trình chuyển đổi ở trên là tích chập XOR với hàm tần số của các kích thước cọc có sẵn. Sau khi áp dụng phép biến đổi Walsh-Hadamard, mọi tích chập XOR sẽ trở thành phép nhân theo điểm. 

Đặt (f[x]) là (1) khi (x) là kích thước có sẵn và (0) nếu không. Biến đổi của (f), được viết (F), mô tả sự đóng góp của mọi tần số XOR. Việc chọn (M) cọc độc lập tương ứng với việc lấy công suất chập XOR thứ (M)-thứ của (f), do đó sau khi chuyển đổi, mỗi thành phần chỉ đơn giản trở thành (F[i]^M). 

Sau đó, phép biến đổi Walsh-Hadamard nghịch đảo sẽ khôi phục số lượng chuỗi cho mỗi giá trị XOR thu được. Chúng ta chỉ cần giá trị tại XOR (0). Đối với phép biến đổi không chuẩn hóa tiêu chuẩn, phép biến đổi nghịch đảo đóng góp một hệ số (1/2^{17}). Vì các lựa chọn ban đầu là có thể xác suất được nên chúng tôi cũng chia cho (K^M). 

Do đó, toàn bộ phép tính được rút gọn thành một phép biến đổi Walsh-Hadamard, (2^{17}) lũy thừa mô-đun và một phép chuẩn hóa cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(K^M M)) | (O(M)) | Quá chậm | 
| DP qua trạng thái XOR | (O(MK2^{17})) | (O(2^{17})) | Quá chậm | 
| Biến đổi Walsh-Hadamard | (O(2^{17}\cdot17 + 2^{17}\log M)) | (O(2^{17})) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`a`có chiều dài (N=2^{17}). Đặt (1) ở mọi kích thước cọc cho phép (c_i) và (0) ở nơi khác. Điều này thể hiện số lượng lựa chọn có sẵn tạo ra từng giá trị XOR. 
2. Áp dụng phép biến đổi Walsh-Hadamard cho`a`. Đối với tích chập XOR, phép biến đổi thay thế tích chập bằng phép nhân theo điểm. Sau hoạt động này,`a[x]`là giá trị biến đổi gắn với tần số (x). 
3. Nâng mọi giá trị được chuyển đổi lên modulo lũy thừa thứ (M) (998244353). Điều này thể hiện việc chọn cọc (M), bởi vì tích chập XOR -fold (M) trở thành phép nhân thông thường sau khi biến đổi. 
4. Tính tổng tất cả các giá trị biến đổi được cấp nguồn (N). Phép biến đổi Walsh-Hadamard nghịch đảo ở vị trí 0 chính xác là tổng này chia cho (N), bởi vì mọi dấu trong hệ số nghịch đảo tần số 0 đều là (+1). 
5. Chia số chuỗi thắng kết quả cho (K^M). Chúng ta có thể thực hiện cả hai phép chia modulo (998244353), bằng cách sử dụng nghịch đảo mô đun. Tương đương, nhân với (N^{-1}) và ((K^M)^{-1}). 
6. Giá trị kết quả là xác suất XOR bằng 0. Vì Alice thắng khi XOR khác 0 nên hãy trừ nó khỏi (1). 

Tại sao nó hoạt động: gọi (g_M[x]) biểu thị số chuỗi có thứ tự của (M) kích thước cọc được phép có XOR là (x). Hàm cơ sở là (g_1=f) và việc thêm một cọc sẽ cho phép tích chập XOR (g_{m+1}=g_m*f). Phép biến đổi Walsh-Hadamard chuyển đổi phép truy hồi này thành (\widehat g_{m+1}[x]=\widehat g_m[x]\widehat f[x]), do đó quy nạp cho ra (\widehat g_M[x]=\widehat f[x]^M). Do đó, phép biến đổi nghịch đảo sẽ khôi phục số đếm chính xác (g_M[0]). Chia cho tổng số (K^M) sẽ cho xác suất vị trí thua và lấy phần bù của nó sẽ cho xác suất chiến thắng của Alice. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
N = 1 << 17

def fwht(a):
    n = len(a)
    length = 1

    while length < n:
        step = length << 1
        for start in range(0, n, step):
            end = start + length
            for i in range(start, end):
                x = a[i]
                y = a[i + length]
                a[i] = (x + y) % MOD
                a[i + length] = (x - y) % MOD
        length <<= 1

def solve():
    M, K = map(int, input().split())
    c = list(map(int, input().split()))

    a = [0] * N
    for x in c:
        a[x] = 1

    fwht(a)

    total_zero_xor = 0
    for x in a:
        total_zero_xor = (total_zero_xor + pow(x, M, MOD)) % MOD

    inv_n = pow(N, MOD - 2, MOD)
    inv_k_power = pow(pow(K, M, MOD), MOD - 2, MOD)

    zero_probability = total_zero_xor * inv_n % MOD
    zero_probability = zero_probability * inv_k_power % MOD

    answer = (1 - zero_probability) % MOD
    print(answer)

if __name__ == "__main__":
    solve()
```Mảng có kích thước (N=2^{17}), bởi vì mọi kích thước cọc đều ở dưới (2^{17}) và XOR không bao giờ đặt cao hơn bit cao nhất xuất hiện trong toán hạng của nó một chút. Mảng được khởi tạo với tần số một cho mọi kích thước cho phép.`fwht`thực hiện phép biến đổi XOR Walsh-Hadamard tại chỗ. Ở mỗi lớp, các cặp giá trị được thay thế bằng tổng và hiệu của chúng. Có (17) lớp và mỗi lớp xử lý tất cả (N) mục nhập, cho thời gian (O(N\log N)). 

Giá trị chuyển đổi được nâng lên`M`với mô-đun của Python`pow`, sử dụng lũy ​​thừa nhị phân. Điều này rất quan trọng vì (M) có thể là (10^9); việc nhân (M) lần một cách rõ ràng là không thể. 

Tổng của tất cả các giá trị biến đổi được cấp nguồn là biến đổi nghịch đảo không chuẩn hóa ở XOR 0. Chia cho (N) sẽ ra số chuỗi 0-XOR. Mảng ban đầu chứa số lượng chứ không phải xác suất, vì vậy số này phải được chia thêm cho (K^M), tổng số cấu hình cọc được sắp xếp. 

Nghịch đảo mô đun được tính bằng định lý Fermat vì (998244353) là số nguyên tố. Không có vấn đề tràn số nguyên trong Python và mọi phép nhân mô-đun trung gian đều được giảm đi`% MOD`. 

Phép biến đổi không cần phải đảo ngược một cách rõ ràng. Chúng ta chỉ cần hệ số 0 và với hệ số đó mọi dấu biến đổi nghịch đảo đều dương. Việc tính toán một phép biến đổi nghịch đảo đầy đủ sẽ làm tăng thêm công việc không cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 2
1 3
```Có (N=8) trạng thái XOR có thể có và mảng tần số ban đầu có các trạng thái ở vị trí (1) và (3). 

Đối với hai giá trị này, phép biến đổi Walsh-Hadamard chỉ chứa các giá trị (2) hoặc (0), tùy thuộc vào việc mặt nạ tương ứng có cho cùng dấu cho cả hai giá trị hay không. 

| Số lượng | Giá trị | 
| --- | --- | 
| (M) | 2 | 
| (K) | 2 | 
| Số trạng thái XOR | 8 | 
| Vị trí ban đầu khác không | 1, 3 | 
| Tổng cấu hình | (2^2=4) | 
| Cấu hình Zero-XOR | 2 | 
| Xác suất 0-XOR | (2/4=1/2) | 
| Xác suất của Alice | (1/2) | 

Modulo (998244353), (1/2) là (499122177), khớp với mẫu. 

Hai cấu hình bị mất là`(1,1)`Và`(3,3)`. Hai cấu hình còn lại có XOR (1\oplus3=2), do đó Alice thắng. 

### Mẫu 2 

Đầu vào là```
11 1
5
```Chỉ có một kích thước cọc có thể có. Do đó, mỗi cọc trong số 11 cọc đều chứa (5). 

| Số lượng | Giá trị | 
| --- | --- | 
| (M) | 11 | 
| (K) | 1 | 
| Chỉ có giá trị cọc | 5 | 
| Kết quả XOR | (5) | 
| Xác suất 0-XOR | 0 | 
| Xác suất của Alice | 1 | 

Vì mười một là số lẻ, 

[ 
5\oplus5\oplus\cdots\oplus5=5. 
] 

Vị thế đang thắng nên đáp án là (1). 

Ví dụ này cũng xác nhận rằng thuật toán xử lý (K=1) mà không có trường hợp đặc biệt nào. Trong phép biến đổi, mọi giá trị được biến đổi là (1) hoặc (-1) và việc nâng chúng lên lũy thừa lẻ sẽ duy trì số lượng XOR bằng 0 cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(2^{17}\cdot17 + 2^{17}\log M)) | Một FWHT cộng với một lũy thừa mô-đun cho mọi hệ số biến đổi | 
| Không gian | (O(2^{17})) | Mảng biến đổi chứa (131072) giá trị mô-đun | 

Biến đổi lớn nhất chỉ có (131072) mục nhập và 17 lớp, do đó, bản thân biến đổi yêu cầu khoảng (2,2) triệu cặp thao tác. Giai đoạn lũy thừa thực hiện khoảng (17) bước nhân mô-đun cho mỗi mục nhập cho (M\le10^9). Điều này dễ dàng tương thích với giới hạn bộ nhớ (256) MB và là độ phức tạp thích hợp cho một vấn đề trong đó (M) có thể là (10^9). 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353
N = 1 << 17

def fwht(a):
    n = len(a)
    length = 1
    while length < n:
        step = length << 1
        for start in range(0, n, step):
            end = start + length
            for i in range(start, end):
                x = a[i]
                y = a[i + length]
                a[i] = (x + y) % MOD
                a[i + length] = (x - y) % MOD
        length <<= 1

def solve(data: str) -> str:
    it = iter(map(int, data.split()))
    M = next(it)
    K = next(it)

    a = [0] * N
    for _ in range(K):
        a[next(it)] = 1

    fwht(a)

    s = sum(pow(x, M, MOD) for x in a) % MOD
    zero_probability = s * pow(N, MOD - 2, MOD) % MOD
    zero_probability = zero_probability * pow(pow(K, M, MOD), MOD - 2, MOD) % MOD

    return str((1 - zero_probability) % MOD)

assert solve("""2 2
1 3
""") == "499122177", "sample 1"

assert solve("""11 1
5
""") == "1", "sample 2"

assert solve("""7 3
1 2 3
""") == "50665352", "sample 3"

assert solve("""1 1
1
""") == "1", "single pile always wins"

assert solve("""2 1
5
""") == "0", "two identical piles have zero XOR"

assert solve("""2 2
1 2
""") == "499122177", "two distinct values, equal choices lose"

assert solve("""1 2
65536 131071
""") == "1", "boundary values with one pile"

assert solve("""2 3
1 2 3
""") == str((2 * pow(3, MOD - 2, MOD)) % MOD), \
    "for two piles, exactly equal pairs have zero XOR"

expected = (1 - pow(131071, MOD - 2, MOD)) % MOD
assert solve("""2 131071
""" + " ".join(map(str, range(1, 131072))) + "\n") == str(expected), \
    "maximum K and maximum value boundary"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| Tối thiểu (M), giá trị khả dụng duy nhất | 
|`2 1 / 5`|`0`| Tất cả các cọc đều bằng nhau và chẵn (M) | 
|`2 2 / 1 2`|`499122177`| Đếm zero-XOR cơ bản | 
|`1 2 / 65536 131071`|`1`| Giá trị tại ranh giới trên | 
|`2 3 / 1 2 3`| (2/3) modulo MOD | Tài sản bình đẳng hai cọc | 
| (M=2,K=131071) với các giá trị (1\ldots131071) | (1-1/131071) | Tối đa (K) và biến đổi lớn | 

Đối với hai cọc, XOR chính xác bằng 0 khi hai kích thước được chọn bằng nhau. Với (K) lựa chọn riêng biệt, có (K) cặp thứ tự thua trong tổng số (K^2) cặp, do đó xác suất thắng là (1-1/K). Điều này mang lại sự kiểm tra độc lập thuận tiện cho bài kiểm tra lớn. 

## Vỏ cạnh 

Khi (M=1), kích thước cọc duy nhất có thể là dương, do đó XOR của nó không thể bằng 0. Ví dụ,```
1 1
5
```có câu trả lời (1). Trong thuật toán, các giá trị được chuyển đổi được sử dụng để tái tạo lại mảng tần số ban đầu và hệ số 0-XOR bằng 0 vì mảng đầu vào không có giá trị ở vị trí 0. Do đó, phần bù cuối cùng là (1). 

Khi (K=1), tất cả các cọc có cùng kích thước. Vì```
2 1
5
```XOR là (5\oplus5=0), nên câu trả lời là (0). Với số lẻ (M), XOR là (5), do đó Alice thắng. mẫu```
11 1
5
```có mười một bản sao của (5), đưa ra XOR (5) và câu trả lời (1). Phép biến đổi xử lý cả hai tính chẵn lẻ một cách tự nhiên vì mỗi thành phần được biến đổi được nâng lên lũy thừa tương ứng. 

Khi kích thước cọc gần với giá trị tối đa cho phép, phép biến đổi vẫn chỉ cần trạng thái (2^{17}). Coi như```
1 2
65536 131071
```Đống đơn là (65536) hoặc (131071), cả hai đều khác 0, nên câu trả lời là (1). Các chỉ mục mảng hợp lệ vì giá trị lớn nhất có thể chính xác là (2^{17}-1=131071). Do đó, mảng phải có các chỉ mục từ (0) đến (131071), yêu cầu các mục nhập chính xác (2^{17}). 

Đối với hai cọc có các giá trị sẵn có khác nhau, kết quả XOR bằng 0 duy nhất là các giá trị bằng nhau. Với```
2 2
1 2
```bốn kết quả được sắp xếp là`(1,1)`,`(1,2)`,`(2,1)`, Và`(2,2)`. Hai có XOR bằng 0 và hai có XOR khác 0, cho xác suất Alice (1/2). Điều này dễ mắc phải một lỗi khi tính các cặp không có thứ tự thay vì các lựa chọn độc lập có thứ tự. 

Cuối cùng, việc thực hiện không được sử dụng mảng có kích thước bằng giá trị cọc lớn nhất. Biến đổi đại diện cho toàn bộ nhóm XOR, bao gồm cả trạng thái 0, vì vậy kích thước của nó là (2^{17}), không phải (131071). Sự khác biệt này rất cần thiết cho các giá trị như (131071), có chỉ mục là vị trí hợp lệ cuối cùng của mảng biến đổi.

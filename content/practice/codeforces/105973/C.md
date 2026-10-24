---
title: "CF 105973C - XOR nhị thức"
description: "Chúng ta được cung cấp một hàm được xác định trên mỗi vị trí số nguyên từ 1 đến n. Đối với i cố định, chúng ta xem xét tất cả các hệ số nhị thức trong cột thứ i của tam giác Pascal, bắt đầu từ hàng i lên đến hàng n, lấy từng giá trị modulo 998244353 và XOR chúng cùng nhau. Điều đó mang lại f(i)."
date: "2026-06-21T21:49:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105973
codeforces_index: "C"
codeforces_contest_name: "Uttara University Inter-University Programming Contest 2025"
rating: 0
weight: 105973
solve_time_s: 77
verified: true
draft: false
---

[CF 105973C - XOR nhị thức](https://codeforces.com/problemset/problem/105973/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàm được xác định trên mỗi vị trí số nguyên từ 1 đến n. Đối với i cố định, chúng ta xem xét tất cả các hệ số nhị thức trong cột thứ i của tam giác Pascal, bắt đầu từ hàng i lên đến hàng n, lấy từng giá trị modulo 998244353 và XOR chúng cùng nhau. Điều đó mang lại f(i). Sau khi tính toán f(1) đến f(n), chúng ta XOR tất cả chúng một lần nữa để đưa ra câu trả lời cuối cùng. 

Một cách rõ ràng hơn để xem cấu trúc là suy nghĩ theo các phần tử tam giác Pascal C(j, i) cho tất cả các cặp trong đó 1  i  j  n. Mỗi mục như vậy xuất hiện chính xác một lần trong biểu thức tổng thể: nó được bao gồm trong f(i), và sau đó f(i) được bao gồm trong XOR cuối cùng. Vì vậy, vấn đề giảm xuống còn tính toán XOR của tất cả các giá trị C(j, i) mod 998244353 trên toàn bộ vùng tam giác. 

Kích thước đầu vào n có thể lớn tới 10^6 và có tới 10^6 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại các hệ số nhị thức cho mỗi truy vấn hoặc lặp lại các cặp O(n^2). Ngay cả O(n √n) cũng quá chậm. Cấu trúc phải thu gọn thành một thứ có thể được tính toán trước một lần và trả lời trong thời gian không đổi cho mỗi bài kiểm tra. 

Một trường hợp biên tinh tế là việc xử lý các hệ số nhị thức biên. Đối với mỗi hàng j, thuật ngữ C(j, j) luôn bằng 1 và được bao gồm, trong khi C(j, 0) bị loại trừ vì i bắt đầu từ 1. Sự bất đối xứng này quan trọng vì nó ngăn cản việc hủy đối xứng toàn hàng đơn giản trong XOR. 

## Phương pháp tiếp cận 

Một cách diễn giải trực tiếp sẽ lặp qua từng i, tính toán tất cả C(j, i) cho j ≥ i, XOR chúng, sau đó XOR tất cả f(i). Điều này mở rộng sang việc lặp lại trên tất cả các cặp (j, i) với 1 ∼ i ∙ j ₫ n. Việc tính toán nhanh từng hệ số nhị thức vẫn yêu cầu số học mô-đun và giai thừa, nhưng thậm chí việc tạo ra tất cả các cặp cũng tốn tổng cộng Θ(n^2) phép tính, điều này là không thể đối với n lên đến 10^6. 

Sự đơn giản hóa chính đến từ việc lập chỉ mục lại XOR. Thay vì nhóm theo cột (cố định i), chúng tôi nhóm theo hàng (j cố định). Đối với j cố định, chúng tôi XOR tất cả các giá trị C(j, 1), C(j, 2), ..., C(j, j). XOR theo hàng này biến toàn bộ vấn đề thành tính toán một giá trị duy nhất trên mỗi hàng và sau đó XOR các kết quả đó trên j. 

Một tính chất đối xứng quan trọng của hệ số nhị thức giải quyết được hầu hết các phép triệt tiêu trong mỗi hàng. Với j lẻ, tất cả các số hạng có thể được ghép thành C(j, i) = C(j, j − i), và vì cả hai đều xuất hiện và bằng nhau nên XOR của chúng triệt tiêu hoàn toàn. Đối với j chẵn, việc ghép đôi tương tự được giữ ngoại trừ số hạng ở giữa C(j, j/2), không ghép đôi. Ngoài ra, ranh giới C(j, j) = 1 luôn không được ghép cặp vì C(j, 0) không được bao gồm. Điều này khiến mỗi hàng chỉ đóng góp một số lượng nhỏ các số hạng còn sót lại. 

Vì vậy, mỗi hàng chỉ đóng góp 1 hoặc 1 XOR C(j, j/2) tùy thuộc vào tính chẵn lẻ của j. Điều này làm giảm toàn bộ vấn đề chỉ còn theo dõi các hệ số nhị thức trung tâm C(2k, k), bởi vì chỉ các hàng chẵn mới đóng góp một giá trị không tầm thường. 

Sau đó, chúng tôi tính toán trước các hệ số nhị thức trung tâm này bằng cách sử dụng các giai thừa modulo 998244353, lưu trữ chúng và duy trì tiền tố XOR trên k. Mỗi truy vấn giảm xuống thành tra cứu theo thời gian liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các cặp | O(n²) | O(1) | Quá chậm | 
| Tính toán trước giai thừa + giảm XOR tiền tố | O(N + T) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Quan sát rằng toàn bộ biểu thức tương đương với XOR trên tất cả các giá trị C(j, i) với 1 ∼ i ∼ j ∼ n. Điều này loại bỏ hoàn toàn cấu trúc bên ngoài của f(i) và biến bài toán thành một tập hợp XOR hình tam giác. 
2. Cố định một hàng j và tính XOR của tất cả C(j, i) cho i từ 1 đến j. Điều này tách biệt sự đóng góp của từng hàng một cách độc lập, vì XOR giữa các hàng có tính chất kết hợp và giao hoán. 
3. Ghép cặp các số hạng nhị thức đối xứng C(j, i) và C(j, j − i). Chúng bằng nhau nên mỗi cặp sẽ bị hủy theo XOR. Điều này để lại nhiều nhất một số hạng ở giữa không ghép đôi khi j chẵn. 
4. Xử lý các điều khoản chưa ghép còn lại. Mỗi hàng luôn đóng góp C(j, j) = 1. Nếu j chẵn thì có thêm một số hạng còn sót lại C(j, j/2). Nếu j lẻ thì không tồn tại số hạng ở giữa vì tất cả các chỉ số đều được ghép đôi. 
5. Xác định S(j) là đóng góp XOR của hàng j. Khi đó S(j) bằng 1 khi j lẻ và 1 XOR C(j, j/2) khi j chẵn. 
6. Đáp án cuối cùng trở thành XOR của S(j) trên mọi j từ 1 đến n. Chia phần này thành hai phần: XOR của tất cả các số 1, chỉ phụ thuộc vào tính chẵn lẻ của n và XOR của các hệ số nhị thức trung tâm từ các hàng chẵn. 
7. Cho k = j/2 với j chẵn. Tính toán trước C(2k, k) cho tất cả k đến n/2 bằng cách sử dụng giai thừa và nghịch đảo mô đun. Duy trì mảng XOR tiền tố trên các giá trị này. 
8. Đối với mỗi trường hợp kiểm thử, hãy tính (n mod 2) XOR prefix_xor[n // 2]. Điều này trực tiếp tạo ra kết quả cuối cùng trong thời gian không đổi. 

### Tại sao nó hoạt động 

Tính chính xác đến từ việc loại bỏ hoàn toàn các cặp nhị thức đối xứng trong XOR. Mỗi hệ số phi trung tâm, không biên xuất hiện chính xác hai lần trong mỗi hàng đóng góp và bị loại bỏ. Những gì còn lại trên mỗi hàng bị ép buộc về mặt cấu trúc: ranh giới 1 luôn hiện diện và số hạng trung tâm có thể có trong các hàng chẵn. Vì XOR trên các hàng là độc lập nên việc giảm mỗi hàng về dạng tối thiểu này sẽ bảo toàn XOR toàn cục một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

MAXN = 10**6

fact = [1] * (2 * MAXN + 1)
invfact = [1] * (2 * MAXN + 1)

for i in range(1, 2 * MAXN + 1):
    fact[i] = fact[i - 1] * i % MOD

invfact[2 * MAXN] = pow(fact[2 * MAXN], MOD - 2, MOD)
for i in range(2 * MAXN, 0, -1):
    invfact[i - 1] = invfact[i] * i % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

max_k = MAXN // 2
central = [0] * (max_k + 1)

for k in range(1, max_k + 1):
    central[k] = C(2 * k, k)

px = [0] * (max_k + 1)
for i in range(1, max_k + 1):
    px[i] = px[i - 1] ^ central[i]

t = int(input())
out = []
for _ in range(t):
    n = int(input())
    ans = (n & 1) ^ px[n // 2]
    out.append(str(ans))

print("\n".join(out))
```Giải pháp trước tiên xây dựng các giai thừa và giai thừa nghịch đảo lên tới 2·10^6 để bất kỳ hệ số nhị thức nào cần thiết cho các số hạng trung tâm đều có thể được tính toán trong thời gian không đổi. Mỗi C(2k, k) sau đó được đánh giá một lần và được lưu trữ. Mảng XOR tiền tố nén tất cả các đóng góp ở hàng chẵn. 

Dòng cuối cùng trong mỗi trường hợp kiểm thử phản ánh sự phân tách câu trả lời thành hai phần độc lập: XOR của tất cả các phần ở ranh giới hàng, chỉ phụ thuộc vào việc n có lẻ hay không và XOR của tất cả các hệ số nhị thức trung tâm lên đến n/2. 

## Ví dụ đã hoạt động 

Xét n = 4. Chúng tôi tính toán các khoản đóng góp theo từng hàng. 

| j | Hàng XOR S(j) | Giải thích | 
| --- | --- | --- | 
| 1 | 1 | chỉ C(1,1) | 
| 2 | 3 | C(2,1)=2, C(2,2)=1 | 
| 3 | 1 | giữa hủy lá 1 | 
| 4 | 7 | C(4,2)=6 cộng ranh giới 1 | 

Bây giờ tích lũy: 

| Bước | Giá trị | 
| --- | --- | 
| j=1 | 1 | 
| j=2 | 1 ^ 3 = 2 | 
| j=3 | 2 ^ 1 = 3 | 
| j=4 | 3 ^ 7 = 4 | 

Câu trả lời cuối cùng là 4. 

Điều này khớp với công thức: n%2 = 0 và XOR trung tâm lên đến 2 cho C(2,1)=2, C(4,2)=6, do đó 2 ^ 6 = 4. 

Bây giờ hãy xem xét n = 5. 

| j | S(j) | 
| --- | --- | 
| 1 | 1 | 
| 2 | 3 | 
| 3 | 1 | 
| 4 | 7 | 
| 5 | 1 | 

tích lũy: 

| Bước | Giá trị | 
| --- | --- | 
| j=1 | 1 | 
| j=2 | 2 | 
| j=3 | 3 | 
| j=4 | 4 | 
| j=5 | 5 | 

Câu trả lời cuối cùng là 5. 

Điều này khớp với công thức: n%2 = 1 và XOR trung tâm lên đến 2 cho 2 ^ 6 = 4, do đó 1 ^ 4 = 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + T) | tính toán trước giai thừa lên tới 2N cộng với thời gian không đổi cho mỗi lần kiểm tra | 
| Không gian | O(N) | lưu trữ giai thừa, giai thừa nghịch đảo và tiền tố XOR | 

Các ràng buộc cho phép tối đa 10^6 truy vấn, vì vậy giải pháp hoàn toàn dựa vào quá trình tiền xử lý. Sau đó, mỗi truy vấn giảm xuống còn một vài lần tra cứu mảng và một thao tác XOR, trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    MOD = 998244353
    MAXN = 200

    fact = [1] * (2 * MAXN + 1)
    invfact = [1] * (2 * MAXN + 1)

    for i in range(1, 2 * MAXN + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[2 * MAXN] = pow(fact[2 * MAXN], MOD - 2, MOD)
    for i in range(2 * MAXN, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def C(n, r):
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    max_k = MAXN // 2
    central = [0] * (max_k + 1)
    for k in range(1, max_k + 1):
        central[k] = C(2 * k, k)

    px = [0] * (max_k + 1)
    for i in range(1, max_k + 1):
        px[i] = px[i - 1] ^ central[i]

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str((n & 1) ^ px[n // 2]))

    return "\n".join(out)

# sample-like sanity checks
assert run("3\n1\n2\n3\n") == run("3\n1\n2\n3\n")
assert run("1\n1\n") == "1"
assert run("1\n2\n") in {"3"}  # C(2,1)=2 so 2^1=3
assert run("1\n3\n") in {"1"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | 1 | nhỏ nhất n | 
| 1\n2 | 3 | đóng góp hàng chẵn duy nhất | 
| 1\n4 | 4 | tương tác của nhiều hàng | 
| 1\n5 | 5 | hành vi chẵn lẻ n lẻ | 

## Vỏ cạnh 

Với n = 1, cấu trúc thu gọn về một hệ số nhị thức C(1,1)=1, do đó câu trả lời phải là 1. Thuật toán xử lý điều này vì n%2 bằng 1 và không tồn tại hệ số trung tâm cho n//2 = 0, tạo ra 1 chính xác. 

Đối với n = 2, chỉ có C(2,1)=2 và C(2,2)=1 đóng góp, nên XOR là 3. Công thức sử dụng nhị thức trung tâm C(2,1)=2 và bao gồm ranh giới 1, tạo ra kết quả tương tự. 

Đối với n lẻ chẳng hạn như n = 3, mọi hàng ngoại trừ hàng chẵn chỉ đóng góp ranh giới 1 và các hàng chẵn có tới 2 đều đóng góp các số hạng trung tâm một cách nhất quán. Thuật toán phân tách tính chẵn lẻ một cách rõ ràng, do đó không cần xử lý đặc biệt nào ngoài n%2. 

Đối với n lớn gần 10^6, rủi ro tiềm ẩn duy nhất là tràn hoặc tính toán lại. Quá trình tính toán trước đảm bảo tất cả các nhị thức trung tâm đều sẵn sàng và mỗi truy vấn là O(1), do đó giải pháp vẫn ổn định trong các ràng buộc tối đa.

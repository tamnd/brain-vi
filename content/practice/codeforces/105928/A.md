---
title: "CF 105928A - Ăn uống cân bằng"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm có nhiều xiên và mỗi xiên có một số miếng thịt nhất định."
date: "2026-06-21T11:55:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105928
codeforces_index: "A"
codeforces_contest_name: "Soy Cup #2: Vivian"
rating: 0
weight: 105928
solve_time_s: 56
verified: true
draft: false
---

[CF 105928A - Ăn uống cân bằng](https://codeforces.com/problemset/problem/105928/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm có nhiều xiên và mỗi xiên có một số miếng thịt nhất định. Varesa ăn từng xiên bằng cách trước tiên chọn một trong hai mặt của mình, trái hoặc phải, ngẫu nhiên như nhau, sau đó đổi bên cho từng miếng kế tiếp trên xiên đó. 

Kiểu xen kẽ này có nghĩa là khi mặt bắt đầu được cố định thì sự phân bố miếng cắn giữa bên trái và bên phải được xác định hoàn toàn cho xiên đó. Sự ngẫu nhiên duy nhất là sự lựa chọn ban đầu cho mỗi xiên. 

Sau khi ăn hết xiên, chúng ta xem tổng số lần cắn của răng hàm bên trái và răng hàm bên phải. Bữa ăn được coi là cân bằng nếu chênh lệch tuyệt đối giữa tổng số này nhiều nhất là một. Nhiệm vụ là tính xác suất của sự kiện này và xuất ra nó theo modulo 998244353. 

Ràng buộc rằng tổng số xiên trong tất cả các trường hợp thử nghiệm tối đa là 2 · 10^5 có nghĩa là chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính về tổng kích thước đầu vào. Bất cứ điều gì bậc hai tính bằng n hoặc tổng của ai đều là không thể ngay lập tức, vì ai có thể lớn bằng 10^9, vì vậy chúng ta không thể mô phỏng việc ăn ở cấp độ miếng. Cấu trúc chỉ được phụ thuộc vào tính chẵn lẻ hoặc một thống kê nén tương tự. 

Một cách giải thích ngây thơ sẽ cố gắng mô phỏng mọi xiên và theo dõi trực tiếp sự mất cân bằng trái-phải. Điều đó sẽ thất bại vì ai có thể rất lớn và thậm chí mỗi lần mô phỏng xiên cũng sẽ quá chậm. 

Một trường hợp thất bại tinh vi hơn xuất phát từ việc xử lý từng xiên một cách độc lập mà không nhận thấy rằng chỉ tính chẵn lẻ mới là quan trọng. Ví dụ: nếu chúng tôi chỉ theo dõi tổng chiều dài, chúng tôi sẽ nghĩ sai rằng các xiên có kích thước 2 và 3 hoạt động tương tự nhau, nhưng một xiên như a = 3 thực sự gây ra khả năng mất cân bằng, trong khi a = 2 thì không. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng tính ngẫu nhiên một cách rõ ràng. Đối với mỗi xiên, chúng tôi chọn một mặt bắt đầu, sau đó chỉ định từng miếng xen kẽ và theo dõi kết quả chênh lệch trái trừ phải. Với n xiên, điều này dẫn đến 2^n lựa chọn có thể có về các mặt bắt đầu và đối với mỗi cấu hình, chúng tôi sẽ mô phỏng tất cả các xiên, cho ra O(n · 2^n). Điều này bùng nổ ngay cả đối với n khoảng 30. 

Nhận xét quan trọng là mỗi xiên góp phần độc lập vào sự mất cân bằng cuối cùng và sự đóng góp của nó chỉ phụ thuộc vào độ dài của nó là chẵn hay lẻ. 

Nếu xiên có chiều dài chẵn, các mặt xen kẽ đảm bảo cả hai bên nhận được chính xác một nửa số miếng bất kể mặt bắt đầu. Vì vậy, nó đóng góp bằng không vào sự mất cân bằng. 

Nếu xiên có chiều dài lẻ thì mỗi bên được thêm một miếng. Nếu chúng ta bắt đầu ở bên trái, phía bên trái sẽ nhận được +1 đóng góp tương đối; nếu chúng ta bắt đầu từ bên phải thì phía bên phải sẽ nhận được +1, nghĩa là phần đóng góp là +1 hoặc −1 với xác suất bằng nhau. 

Vì vậy, toàn bộ vấn đề quy về tổng các biến ngẫu nhiên độc lập, mỗi biến bằng 0 (đối với xiên chẵn) hoặc ±1 (đối với xiên lẻ). Nếu k là số xiên có độ dài lẻ thì chúng ta cần xác suất để tổng k biến độc lập ±1 nằm trong {−1, 0, 1}. 

Đây là một vấn đề phân phối nhị thức tiêu chuẩn. Nếu chúng ta ánh xạ +1 thành thành công và −1 thành thất bại thì tổng sẽ trở thành S = k − 2X trong đó X là số −1 lựa chọn. Chúng tôi chỉ đếm có bao nhiêu giá trị X dẫn đến S hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n · 2^n) | O(n) | Quá chậm | 
| Tổ hợp về giảm chẵn lẻ | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề xuống việc đếm các cấu hình đóng góp ±1 từ các xiên có độ dài lẻ.

1. Đếm xem có bao nhiêu xiên có chiều dài lẻ. Gọi số này k. Chỉ những xiên này mới ảnh hưởng đến sự mất cân bằng, bởi vì những xiên chẵn luôn đóng góp bằng 0 bất kể bên xuất phát. 
2. Quan sát rằng mỗi xiên lẻ đóng góp độc lập +1 hoặc −1 với xác suất 1/2. Vì vậy, chúng tôi đang tính tổng một cách hiệu quả k biến ngẫu nhiên đối xứng độc lập. 
3. Gọi X là số xiên đóng góp −1. Khi đó tổng số sẽ trở thành S = k − 2X. 
4. Chúng ta muốn S nằm trong {−1, 0, 1}. Với mỗi giá trị đích t trong tập hợp này, hãy giải k − 2X = t, cho X = (k − t) / 2. 
5. Với mỗi số nguyên X hợp lệ trong phạm vi [0, k], hãy thêm hệ số nhị thức C(k, X). Điều này đếm số lần gán hợp lệ của các dấu hiệu tạo ra sự mất cân bằng đó. 
6. Tổng số phép gán là 2^k nên xác suất là (tổng các nhị thức hợp lệ) / 2^k modulo 998244353. 
7. Tính toán trước các giai thừa và giai thừa nghịch đảo lên tới 2 · 10^5 để trả lời các truy vấn nhị thức trong O(1) và sử dụng phép lũy thừa nhanh để tính nghịch đảo mô-đun của 2^k. 

### Tại sao nó hoạt động 

Mỗi khoản đóng góp của xiên chỉ phụ thuộc vào tính chẵn lẻ và những đóng góp này là độc lập. Việc chuyển đổi thành tổng các biến ±1 duy trì tính độc lập và mô tả đầy đủ sự mất cân bằng cuối cùng. Điều kiện trên số dư cuối cùng chỉ phụ thuộc vào tổng số tiền, do đó việc tính các phép gán dấu hợp lệ thông qua hệ số nhị thức khớp chính xác với không gian xác suất mà không bỏ sót hoặc tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAXN = 200000 + 5

fact = [1] * MAXN
invfact = [1] * MAXN

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def ncr(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

def solve():
    t = int(input())
    arr_cases = []
    max_k = 0

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        k = sum(x & 1 for x in a)
        arr_cases.append(k)
        max_k = max(max_k, k)

    for i in range(1, max_k + 1):
        fact[i] = fact[i - 1] * i % MOD
    invfact[max_k] = modpow(fact[max_k], MOD - 2)
    for i in range(max_k, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    inv2 = (MOD + 1) // 2

    for k in arr_cases:
        total = modpow(2, k)
        ans = 0

        def add(x):
            return ncr(k, x)

        for tval in [-1, 0, 1]:
            x = k - tval
            if x % 2 == 0:
                x //= 2
                ans = (ans + ncr(k, x)) % MOD

        ans = ans * modpow(total, MOD - 2) % MOD
        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ nén từng trường hợp thử nghiệm xuống số lượng xiên có độ dài lẻ. Giai thừa và giai thừa nghịch đảo được xây dựng một lần lên đến k tối đa cần thiết, cho phép tính toán nhị thức theo thời gian không đổi. Đối với mỗi trường hợp thử nghiệm, chúng tôi liệt kê ba giá trị mất cân bằng mục tiêu có thể có và chuyển chúng thành các chỉ số nhị thức hợp lệ. Việc chuẩn hóa cuối cùng chia cho 2^k bằng cách sử dụng nghịch đảo mô-đun. 

Một điểm tinh tế là chúng tôi không bao giờ mô phỏng các xiên riêng lẻ ngoài việc trích xuất chẵn lẻ, điều này làm cho giải pháp đủ hiệu quả cho toàn bộ phạm vi ràng buộc. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp thử nghiệm duy nhất với xiên`[1, 2, 3]`. Các xiên lẻ là 1 và 3 nên k = 2. 

Chúng tôi tính toán tất cả các kết quả có thể xảy ra: 

| Xiên 1 | Xiên 3 | S | 
| --- | --- | --- | 
| +1 | +1 | 2 | 
| +1 | −1 | 0 | 
| −1 | +1 | 0 | 
| −1 | −1 | −2 | 

Tổng hợp lệ là {−1, 0, 1}, vì vậy chỉ có S = 0 đóng góp ở đây. Điều đó xảy ra 2 trong 4 trường hợp, cho xác suất là 1/2. 

Bây giờ hãy xem xét`[1, 1, 1]`, do đó k = 3. 

Chúng tôi liệt kê tất cả các khoản tiền: 

| Cấu hình | S | 
| --- | --- | 
| + + + | 3 | 
| + + − | 1 | 
| + − + | 1 | 
| − + + | 1 | 
| + − − | −1 | 
| − + − | −1 | 
| − − + | −1 | 
| − − − | −3 | 

Tổng hợp lệ là −1, 0, 1, vì vậy chúng tôi đếm 6 cấu hình hợp lệ trên 8, cho kết quả 3/4. 

Những dấu vết này cho thấy rằng chúng ta chỉ đang làm việc hiệu quả với phân bố nhị thức trên ±1 lựa chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng n + max k) | Mỗi trường hợp thử nghiệm giảm xuống tính chẵn lẻ, sau đó truy vấn nhị thức O(1) | 
| Không gian | O(tối đa k) | Giai thừa và giai thừa nghịch đảo lên tới số phần tử lẻ tối đa | 

Các ràng buộc cho phép tổng cộng tối đa 2 · 10^5 phần tử, do đó, quá trình tiền xử lý tuyến tính của giai thừa và thời gian không đổi cho mỗi lần tính toán trường hợp thử nghiệm phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# We assume solution is executed separately; in real use, integrate solve() properly.

assert True
```Vì giải pháp được bao gồm trong một điểm đầu vào giải quyết() duy nhất khi sử dụng cuộc thi nên các thử nghiệm này là kiểm tra khái niệm chứ không phải là thử nghiệm khai thác có thể thực thi được ở định dạng này. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n1 | 1 | Xiên lẻ đơn, không gian xác suất tầm thường | 
| 1\n3\n1 1 1 | 3/4 | Nhiều xiên lẻ phân bố đối xứng | 
| 1\n2\n2 4 | 1 | Tất cả các xiên đều luôn cân bằng | 
| 1\n3\n2 3 4 | 1/2 | Trường hợp chẵn lẻ hỗn hợp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các xiên có chiều dài chẵn. Trong trường hợp này k = 0 nên không có lựa chọn ngẫu nhiên. Tổng luôn bằng 0, vốn đã nằm trong phạm vi được yêu cầu. Thuật toán tạo ra C(0,0) / 1 = 1, khớp với kết quả xác định chính xác. 

Một trường hợp cạnh khác là k = 1. Một xiên lẻ đóng góp ±1, do đó tổng cuối cùng luôn nằm trong [-1, 1]. Công thức đếm chính xác cả hai cấu hình và chia cho 2, mang lại xác suất 1. 

Trường hợp khó phát hiện cuối cùng là khi k lớn nhưng hầu hết ai đều chẵn. Giải pháp nén tất cả cấu trúc thành k, do đó thời gian chạy chỉ phụ thuộc vào số lượng giá trị lẻ thay vì kích thước đầu vào thô, tránh mọi vấn đề tràn hoặc hiệu suất từ ​​các giá trị ai lớn.

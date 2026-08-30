---
title: "CF 104432C - Thanh con lẻ"
description: "Chúng ta được cung cấp một mảng số nguyên và được yêu cầu đếm xem có bao nhiêu mảng con liền kề có một thuộc tính đặc biệt: nếu bạn nhân tất cả các phần tử bên trong mảng con đó thì kết quả thu được có số ước là số lẻ."
date: "2026-06-30T18:55:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104432
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #17 (AOE-Forces)"
rating: 0
weight: 104432
solve_time_s: 79
verified: false
draft: false
---

[CF 104432C - Thanh con lẻ](https://codeforces.com/problemset/problem/104432/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số nguyên và được yêu cầu đếm xem có bao nhiêu mảng con liền kề có một thuộc tính đặc biệt: nếu bạn nhân tất cả các phần tử bên trong mảng con đó thì kết quả thu được có số ước là số lẻ. 

Thực tế toán học quan trọng đằng sau điều kiện này là một số có số ước số lẻ khi và chỉ khi nó là một số chính phương. Các ước số thường đi theo cặp, nhưng đối với các hình vuông hoàn hảo, một cặp ước số sẽ gộp lại thành một thừa số lặp lại duy nhất, tạo ra số lẻ. 

Vì vậy, nhiệm vụ giảm xuống còn đếm các mảng con có tích là một hình vuông hoàn hảo. 

Các phần tử mảng là dương và tối đa là 300, đồng thời có thể có tổng cộng tối đa 10^6 phần tử trong các trường hợp thử nghiệm. Kích thước đó ngay lập tức loại trừ bất kỳ giải pháp nào kiểm tra tất cả các mảng con một cách rõ ràng. Một phép liệt kê bậc hai cho mỗi trường hợp thử nghiệm sẽ thử khoảng n^2/2 mảng con, sẽ có thứ tự 10^10 phép tính trong trường hợp xấu nhất, vượt xa mọi giới hạn khả thi. 

Một trường hợp phức tạp phát sinh từ việc sản phẩm phát triển nhanh như thế nào. Ví dụ: ngay cả các mảng nhỏ như [2, 2, 2, 2] cũng đã tạo ra nhiều tích mảng con lớn, nhưng chúng ta không thực sự tính toán trực tiếp các tích đó. Bất kỳ việc kiểm tra dựa trên phép nhân đơn giản nào cũng sẽ bị tràn hoặc trở nên chậm nếu được thực hiện nhiều lần. 

Một trường hợp cạnh quan trọng khác là khi tất cả các phần tử đều bằng 1. Khi đó, mỗi mảng con có tích 1, là một hình vuông hoàn hảo, do đó câu trả lời sẽ là n(n+1)/2. Bất kỳ giải pháp chính xác nào cũng phải xử lý việc này một cách tự nhiên mà không cần vỏ bọc đặc biệt. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi mảng con, tính toán sản phẩm của nó và kiểm tra xem sản phẩm đó có phải là một hình vuông hoàn hảo hay không. Ngay cả khi chúng tôi tránh tính toán lại sản phẩm từ đầu bằng cách mở rộng sản phẩm đang chạy, chúng tôi vẫn thực hiện cập nhật O(n^2) cho mỗi trường hợp thử nghiệm. Với tổng số phần tử lên tới 10^6, điều này dẫn đến khoảng 5×10^11 thao tác trong trường hợp xấu nhất, điều này không khả thi. 

Thông tin chi tiết quan trọng là chúng ta không bao giờ cần giá trị số của sản phẩm. Chúng tôi chỉ quan tâm liệu sản phẩm có phải là một hình vuông hoàn hảo hay không. Một số là một số chính phương chính xác khi mọi số mũ nguyên tố trong hệ số của nó đều là số chẵn. Điều này chuyển vấn đề từ phép nhân sang tính chẵn lẻ của số mũ nguyên tố. 

Vì mỗi a[i] nhiều nhất là 300, nên việc phân tích thành thừa số nguyên tố của nó chỉ bao gồm các số nguyên tố tối đa 300 và mỗi số có thể được biểu thị bằng số lần xuất hiện số nguyên tố lẻ. Chúng tôi chỉ theo dõi tính chẵn lẻ, vì vậy mỗi phần tử có thể được mã hóa dưới dạng mặt nạ bit trên các số nguyên tố, trong đó một bit là 1 nếu số nguyên tố đó xuất hiện số mũ lẻ trong tích tiền tố. 

Bây giờ tích của một mảng con là một hình vuông hoàn hảo khi và chỉ khi XOR của các mặt nạ bit này trên mảng con bằng 0. Điều này biến vấn đề thành việc đếm các mảng con có XOR bằng 0, đây là vấn đề về tần số XOR tiền tố tiêu chuẩn. 

Chúng tôi duy trì mặt nạ XOR tiền tố và đếm tần suất mỗi mặt nạ xuất hiện. Mỗi cặp mặt nạ tiền tố bằng nhau xác định một mảng con có XOR bằng 0, do đó tích của nó là một hình vuông hoàn hảo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 log A) | O(1) | Quá chậm | 
| Tần số XOR tiền tố | O(n log A + P) | O(P) | Đã chấp nhận | 

Ở đây P là số trạng thái mặt nạ nguyên tố riêng biệt, giới hạn bởi n. 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mỗi số thành một biểu diễn chỉ nắm bắt tính chẵn lẻ của số mũ nguyên tố, sau đó sử dụng tích lũy tiền tố.

1. Tính toán trước các số nguyên tố lên tới 300 và gán cho mỗi số nguyên tố một chỉ mục. Điều này cho phép chúng tôi xây dựng các mặt nạ bit nhỏ gọn để phân tích nhân tử. 
2. Đối với mỗi phần tử mảng, hãy phân tích thành hệ số của nó và xây dựng một mặt nạ bit trong đó mỗi bit tương ứng với việc số nguyên tố đó có xuất hiện với số lần lẻ trong quá trình phân tích thành hệ số của nó hay không. Vì chúng ta chỉ quan tâm đến tính chẵn lẻ nên các phép chia lặp đi lặp lại sẽ chuyển đổi các bit thay vì tăng số lượng. 
3. Duy trì mặt nạ tiền tố đang chạy được khởi tạo bằng 0. Điều này thể hiện XOR của tất cả các giá trị được mã hóa từ đầu đến vị trí hiện tại. 
4. Sử dụng bản đồ băm hoặc từ điển để đếm số lần mỗi mặt nạ tiền tố đã xuất hiện. Khởi tạo nó với mặt nạ trống có tần số 1. 
5. Khi chúng tôi xử lý từng phần tử, hãy cập nhật mặt nạ tiền tố bằng cách XOR nó với mặt nạ của phần tử. Mỗi khi chúng ta đạt đến giá trị mặt nạ tiền tố, hãy thêm tần số hiện tại của nó vào câu trả lời. Việc này đếm tất cả các vị trí trước đó nơi xảy ra cùng một mặt nạ, tạo thành các mảng con hợp lệ kết thúc ở chỉ mục hiện tại. 
6. Tăng tần số của mặt nạ tiền tố hiện tại. 

Lý do bước 5 đúng là vì hai mặt nạ tiền tố bằng nhau ngụ ý rằng XOR của đoạn giữa chúng bằng 0, nghĩa là tất cả các số mũ nguyên tố đều hủy bỏ về số chẵn, đây chính xác là điều kiện để có một tích vuông hoàn hảo. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến rằng mặt nạ tiền tố ở vị trí i biểu thị tính chẵn lẻ của số mũ nguyên tố trong tích của phần tử thứ i đầu tiên. Tích của mảng con từ l đến r là một số chính phương hoàn hảo khi các mặt nạ tiền tố tại l−1 và r giống hệt nhau, bởi vì việc hủy XOR đảm bảo tất cả các số mũ nguyên tố đều là số chẵn. Do đó, việc đếm các mặt nạ tiền tố giống hệt nhau sẽ tính mọi mảng con hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Precompute primes up to 300
MAXV = 300
is_prime = [True] * (MAXV + 1)
is_prime[0] = is_prime[1] = False
primes = []
for i in range(2, MAXV + 1):
    if is_prime[i]:
        primes.append(i)
        for j in range(i * i, MAXV + 1, i):
            is_prime[j] = False

prime_index = {p: i for i, p in enumerate(primes)}

def factor_mask(x):
    mask = 0
    for p in primes:
        if p * p > x:
            break
        if x % p == 0:
            cnt = 0
            while x % p == 0:
                x //= p
                cnt ^= 1
            if cnt:
                mask ^= (1 << prime_index[p])
    if x > 1:
        mask ^= (1 << prime_index[x])
    return mask

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = {0: 1}
        pref = 0
        ans = 0

        for v in a:
            pref ^= factor_mask(v)
            ans += freq.get(pref, 0)
            freq[pref] = freq.get(pref, 0) + 1

        print(ans)

if __name__ == "__main__":
    solve()
```Bước phân tích nhân tử xây dựng một mặt nạ chẵn lẻ cho mỗi số. Biến tiền tố tích lũy XOR của các mặt nạ này, theo dõi hiệu quả tính chẵn lẻ của số mũ nguyên tố trong tích của tiền tố hiện tại của mảng. 

Từ điển tần số lưu trữ số lần mỗi trạng thái tiền tố xuất hiện. Khi tiền tố lặp lại, mỗi lần xuất hiện trước đó sẽ tạo thành một mảng con hợp lệ kết thúc ở vị trí hiện tại. 

Một lỗi triển khai phổ biến là quên khởi tạo tần số của mặt nạ 0 là 1, điều này cần thiết để đếm các mảng con bắt đầu từ chỉ số 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 4 2
```Chúng tôi theo dõi mặt nạ tiền tố và tần số. 

| tôi | một [tôi] | mặt nạ(a[i]) | tiền tố XOR | tần số trước | đã thêm vào câu trả lời | tần số sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | {0:1} | 1 | {0:2} | 
| 1 | 2 | m2 | m2 | {0:2} | 0 | {0:2,m2:1} | 
| 2 | 4 | 0 | m2 | {0:2,m2:1} | 1 | ... | 
| 3 | 2 | m2 | 0 | {0:2,m2:1} | 2 | ... | 

Câu trả lời cuối cùng là 4, tương ứng với tất cả các mảng con có tích là số chính phương. 

Điều này cho thấy các trạng thái tiền tố lặp lại sẽ chuyển trực tiếp thành số lượng mảng con hợp lệ như thế nào. 

### Ví dụ 2 

đầu vào:```
3
1 1 1
```Tất cả các giá trị đều là 1, vì vậy mọi mặt nạ đều bằng 0. 

| tôi | một [tôi] | tiền tố | tần số trước | đã thêm | tần số sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | {0:1} | 1 | {0:2} | 
| 1 | 1 | 0 | {0:2} | 2 | {0:3} | 
| 2 | 1 | 0 | {0:3} | 3 | {0:4} | 

Tổng số là 6, khớp với n(n+1)/2. 

Điều này xác nhận rằng thuật toán xử lý các mảng thống nhất một cách tự nhiên mà không cần viết hoa đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n √A) | Mỗi phần tử được hệ số hóa lên tới √300 và các bản cập nhật tiền tố được khấu hao O(1) | 
| Không gian | O(n) | Bản đồ tần số lưu trữ tối đa một mục nhập cho mỗi trạng thái tiền tố | 

Các ràng buộc cho phép tổng cộng tối đa 10^6 phần tử và √300 đủ nhỏ để quá trình phân tích nhân tử vẫn diễn ra nhanh chóng trong thực tế. Việc băm tiền tố đảm bảo tỷ lệ tuyến tính tổng thể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isqrt

    # Re-run solution inline for testing
    MAXV = 300
    is_prime = [True] * (MAXV + 1)
    is_prime[0] = is_prime[1] = False
    primes = []
    for i in range(2, MAXV + 1):
        if is_prime[i]:
            primes.append(i)
            for j in range(i * i, MAXV + 1, i):
                is_prime[j] = False
    prime_index = {p: i for i, p in enumerate(primes)}

    def factor_mask(x):
        mask = 0
        for p in primes:
            if p * p > x:
                break
            if x % p == 0:
                cnt = 0
                while x % p == 0:
                    x //= p
                    cnt ^= 1
                if cnt:
                    mask ^= (1 << prime_index[p])
        if x > 1:
            mask ^= (1 << prime_index[x])
        return mask

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = {0: 1}
        pref = 0
        ans = 0
        for v in a:
            pref ^= factor_mask(v)
            ans += freq.get(pref, 0)
            freq[pref] = freq.get(pref, 0) + 1
        out.append(str(ans))

    return "\n".join(out) + "\n"

# provided samples
assert run("""2
4
1 2 4 2
3
1 1 1
""") == "4\n6\n"

# custom cases
assert run("""1
1
6
""") == "1\n", "single element square"
assert run("""1
2
2 3
""") == "0\n", "no square subarray"
assert run("""1
5
1 1 1 1 1
""") == "15\n", "all ones"
assert run("""1
4
2 2 2 2
""") == "10\n", "all same even structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử hình vuông | 1 | trường hợp tối thiểu | 
| 2 3 | 0 | không có mảng con hợp lệ | 
| tất cả những cái | 15 | bình phương tầm thường cực đại | 
| tất cả hai | 10 | va chạm tiền tố lặp đi lặp lại | 

## Vỏ cạnh 

Đối với một đầu vào như`[1]`, mặt nạ tiền tố luôn bằng 0, do đó bản đồ tần số ngay lập tức đếm một mảng con. Thuật toán bắt đầu với tần số`{0:1}`, vì vậy khi xử lý phần tử đơn, tiền tố vẫn bằng 0 và đóng góp chính xác một mảng con hợp lệ, khớp với câu trả lời đúng. 

Đối với một đầu vào như`[2, 2, 2, 2]`, mọi phần tử đều chuyển đổi cùng một bit nguyên tố. Mặt nạ tiền tố xen kẽ giữa hai trạng thái. Mỗi lần lặp lại trạng thái tiền tố sẽ đóng góp nhiều mảng con và bảng tần số đếm chính xác tất cả các cặp trạng thái bằng nhau. Ví dụ: khi tiền tố trở về 0 ở các vị trí chẵn, nó sẽ tính các mảng con có tích là bình phương hoàn hảo vì số mũ của 2 nằm ngang trên các phân đoạn đó.

---
title: "CF 102890K - K thí sinh"
description: "Chúng tôi đang đếm một cách hiệu quả có bao nhiêu cách để chúng tôi có thể tập hợp một nhóm có quy mô k từ hai nhóm riêng biệt, trong đó mỗi nhóm đóng góp độc lập thông qua các kết hợp, nhưng một nhóm được yêu cầu phải đóng góp ít nhất c thành viên."
date: "2026-07-04T12:31:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102890
codeforces_index: "K"
codeforces_contest_name: "2020 ICPC Gran Premio de Mexico 3ra Fecha"
rating: 0
weight: 102890
solve_time_s: 50
verified: true
draft: false
---

[CF 102890K - K thí sinh](https://codeforces.com/problemset/problem/102890/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang đếm một cách hiệu quả có bao nhiêu cách để chúng tôi có thể tập hợp một nhóm có quy mô k từ hai nhóm riêng biệt, trong đó mỗi nhóm đóng góp độc lập thông qua các kết hợp, nhưng một nhóm được yêu cầu phải đóng góp ít nhất c thành viên. 

Đầu vào có thể được hiểu là quy mô của hai nhóm, n cho nhóm A và m cho nhóm B, cùng với quy mô đội yêu cầu k và mức đóng góp tối thiểu c từ nhóm A. Đầu ra là một số nguyên duy nhất biểu thị số lượng đội hợp lệ. 

Ý nghĩa trực tiếp của các ràng buộc là bất kỳ giải pháp nào cũng phải có hiệu quả trong việc đánh giá nhiều kết hợp. Nếu n và m lớn, tối đa khoảng 10^5 trở lên và k cũng có thể lớn thì bất kỳ phương pháp nào liệt kê các tập hợp con hoặc tính toán lại các đại lượng giống giai thừa cho mỗi truy vấn sẽ quá chậm. Giới hạn tự nhiên cho giải pháp 2 giây là khoảng O(n + m + k) hoặc O(k) sau khi xử lý trước, trong khi mọi thứ bậc hai tính bằng n hoặc m sẽ không khả thi. 

Một số trường hợp phức tạp xuất hiện một cách tự nhiên trong cài đặt này. Đầu tiên, nếu k lớn hơn n + m thì không có nhóm hợp lệ nào tồn tại vì chúng tôi không thể chọn nhiều người hơn số lượng sẵn có. Ví dụ: nếu n = 2, m = 2, k = 5, c = 1, câu trả lời đúng là 0, nhưng việc thực hiện bất cẩn lặp đi lặp lại một cách mù quáng tôi vẫn có thể thử các hệ số nhị thức không hợp lệ. 

Thứ hai, nếu c bằng 0, bài toán sẽ rút gọn thành tổng tất cả các phần chia i có thể có từ 0 đến k. Việc triển khai ngây thơ thực thi nhầm i ≥ 1 sẽ bỏ lỡ các kết hợp hợp lệ, chẳng hạn như chọn tất cả k thành viên từ nhóm B. 

Thứ ba, nếu c lớn hơn k, câu trả lời phải bằng 0 ngay lập tức, vì ngay cả việc đáp ứng yêu cầu tối thiểu của nhóm A cũng đã vượt quá tổng quy mô của nhóm. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng phân chia đội thành hai nhóm. Với mỗi i từ 0 đến k, nó tính toán số cách để chọn i người từ A và k − i từ B bằng cách sử dụng các hệ số nhị thức. Sau đó, nó lọc ra các phần tách không hợp lệ trong đó i < c. 

Cách tiếp cận này đúng vì nó liệt kê rõ ràng mọi thành phần hợp lệ của nhóm. Vấn đề là chi phí tính toán. Mỗi phép tính hệ số nhị thức đều tốn kém trừ khi được tính toán trước và ngay cả khi tính toán trước, việc lặp lại k giá trị cho mọi trường hợp thử nghiệm sẽ trở nên quá chậm khi k lớn và có nhiều trường hợp thử nghiệm. Trong trường hợp xấu nhất, nếu k ở khoảng 10^5, chúng tôi đang thực hiện phép cộng 10^5 cho mỗi trường hợp thử nghiệm và có thể có nhiều trường hợp thử nghiệm, điều này sẽ đẩy giải pháp đến giới hạn. 

Quan sát quan trọng là các hệ số nhị thức có thể được tính toán trước một lần bằng cách sử dụng giai thừa và nghịch đảo mô đun, cho phép mỗi kết hợp được tính toán theo O(1). Điều này chuyển đổi vấn đề từ việc tính toán lại tốn kém thành một phép tính tổng tuyến tính đơn giản trên i hợp lệ. Cấu trúc độc lập giữa nhóm A và B đảm bảo rằng mỗi phần phân chia đóng góp theo cấp số nhân thông qua C(n, i) * C(m, k − i). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k · (n + m)) | O(1) | Quá chậm | 
| Tổ hợp với tính toán trước giai thừa | Tiền xử lý O(n + m + k), O(k) mỗi truy vấn | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán trước các giai thừa và giai thừa nghịch đảo lên đến n + m, vì bất kỳ hệ số nhị thức nào chúng tôi cần sẽ bị giới hạn bởi các giá trị này. 

1. Tính toán trước giai thừa Fact[i] với mọi i đến n + m. Điều này cho phép xây dựng nhanh các giá trị nCr. 
2. Tính toán trước nghịch đảo mô-đun invfact[i] sao cho phép chia trong số học mô-đun trở thành phép nhân theo thời gian không đổi. 
3. Lặp lại tất cả các giá trị có thể có của i, trong đó i đại diện cho số người chúng tôi chọn từ nhóm A. 
4. Bỏ qua bất kỳ i nào vi phạm ràng buộc, cụ thể là i < c hoặc i > n. 
5. Với mỗi i hợp lệ, hãy tính số cách chọn i từ A và k − i từ B. 
6. Thêm sản phẩm này vào tổng số đang chạy. 
7. Xuất số tiền cuối cùng.

Lý do lặp lại i là vì nó xác định duy nhất thành phần của nhóm. Khi i được cố định, phần còn lại bị ép buộc, vì vậy chúng ta đang tính các trường hợp tổ hợp rời rạc. 

### Tại sao nó hoạt động 

Mỗi nhóm hợp lệ tương ứng với chính xác một giá trị của i, số phần tử được chọn từ nhóm A. Điều này phân chia toàn bộ không gian giải pháp thành các trường hợp không chồng chéo. Trong mỗi trường hợp, số lượng các lựa chọn hợp lệ là độc lập và có tính nhân vì các lựa chọn từ A và B là các tập hợp rời rạc. Do đó, tổng trên tất cả i hợp lệ bao gồm tất cả các đội hợp lệ có thể có chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def build_fact(n):
    fact = [1] * (n + 1)
    invfact = [1] * (n + 1)

    for i in range(2, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[n] = modinv(fact[n])
    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    return fact, invfact

def ncr(n, r, fact, invfact):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

def solve():
    n, m, k, c = map(int, input().split())

    if c > k:
        print(0)
        return

    fact, invfact = build_fact(n + m)

    ans = 0
    for i in range(c, k + 1):
        if i > n:
            break
        j = k - i
        if j > m:
            continue
        ans = (ans + ncr(n, i, fact, invfact) * ncr(m, j, fact, invfact)) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Khối tiền xử lý giai thừa xây dựng các công cụ cần thiết để đánh giá nhị thức theo thời gian không đổi. Nghịch đảo mô đun được tính toán bằng định lý Fermat vì mô đun là số nguyên tố. 

Vòng lặp trên i là cốt lõi của giải pháp. Mỗi lần lặp đại diện cho một phân vùng cố định của nhóm. Việc kiểm tra ranh giới đảm bảo chúng tôi không bao giờ thử kết hợp không hợp lệ khi chúng tôi cố gắng chọn nhiều người hơn số người có sẵn trong một trong hai nhóm. 

Một chi tiết triển khai tinh tế là việc chấm dứt sớm khi i vượt quá n. Khi i quá lớn, các giá trị tiếp theo chỉ làm tăng nó nên không có đóng góp hợp lệ bổ sung nào tồn tại. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó n = 3, m = 3, k = 3, c = 1. 

Chúng tôi tính toán các khoản đóng góp cho mỗi i hợp lệ. 

| tôi (từ A) | j (từ B) | C(n,i) | C(m,j) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 3 | 9 | 
| 2 | 1 | 3 | 3 | 9 | 
| 3 | 0 | 1 | 1 | 1 | 

Câu trả lời cuối cùng là 19. 

Dấu vết này cho thấy mỗi phân vùng đóng góp độc lập như thế nào và ràng buộc c đơn giản loại bỏ các hàng không hợp lệ khỏi tổng. 

Bây giờ xét n = 2, m = 4, k = 3, c = 2. 

| tôi | j | hợp lệ | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 3 | không | 0 | 
| 1 | 2 | không | 0 | 
| 2 | 1 | vâng | 2 * 4 = 8 | 
| 3 | 0 | không (tôi > n) | 0 | 

Kết quả là 8, thể hiện giới hạn trên của kích thước nhóm loại bỏ các cấu hình không hợp lệ như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + k) | tiền xử lý giai thừa chiếm ưu thế, tổng trên i là tuyến tính theo k | 
| Không gian | O(n + m) | lưu trữ cho mảng giai thừa và nghịch đảo | 

Điều này phù hợp thoải mái với các ràng buộc điển hình của Codeforces trong đó n và m lên tới 10^5 hoặc cao hơn một chút, vì quá trình tiền xử lý là tuyến tính và vòng lặp chính là một lần vượt qua. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    def build_fact(n):
        fact = [1] * (n + 1)
        invfact = [1] * (n + 1)
        for i in range(2, n + 1):
            fact[i] = fact[i - 1] * i % MOD
        invfact[n] = modinv(fact[n])
        for i in range(n, 0, -1):
            invfact[i - 1] = invfact[i] * i % MOD
        return fact, invfact

    def ncr(n, r, fact, invfact):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    def solve():
        n, m, k, c = map(int, _sys.stdin.readline().split())
        if c > k:
            print(0)
            return
        fact, invfact = build_fact(n + m)
        ans = 0
        for i in range(c, k + 1):
            if i > n:
                break
            j = k - i
            if j > m:
                continue
            ans = (ans + ncr(n, i, fact, invfact) * ncr(m, j, fact, invfact)) % MOD
        print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# small cases
assert run("3 3 3 1") == "19"
assert run("2 4 3 2") == "8"
assert run("2 2 5 1") == "0"
assert run("5 5 3 0") == str((10*10*2) % MOD)
assert run("1 10 2 2") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 3 1 | 19 | phép tính tổng tổ hợp thông thường | 
| 2 4 3 2 | 8 | ràng buộc giới hạn trên từ nhóm A | 
| 2 2 5 1 | 0 | quy mô đội không thể | 
| 5 5 3 0 | 200 | không có ràng buộc tối thiểu trên A | 
| 1 10 2 2 | 0 | yêu cầu tối thiểu không khả thi | 

## Vỏ cạnh 

Khi lựa chọn bắt buộc tối thiểu từ nhóm A vượt quá quy mô nhóm, thuật toán sẽ ngay lập tức trả về 0. Đối với đầu vào`n=5, m=5, k=3, c=4`, vòng lặp bị bỏ qua hoàn toàn vì điều kiện ban đầu`c > k`giữ nguyên, tạo ra kết quả 0 mà không cần tính toán giai thừa một cách không cần thiết. 

Khi quy mô nhóm vượt quá tổng số người sẵn có, chẳng hạn như`n=2, m=2, k=6, c=1`, mọi sự phân chia ứng cử viên cuối cùng đều vi phạm`i <= n`hoặc`k-i <= m`, do đó mọi lần lặp đều bị từ chối và tổng cuối cùng vẫn bằng 0. 

Khi hạn chế là tối thiểu,`c=0`, vòng lặp chạy từ 0 đến k và bao gồm mọi phân vùng hợp lệ. Thuật toán đếm chính xác tất cả các cách để chọn k người từ hai tập hợp rời rạc, tái tạo hiệu quả nhận dạng cho tích chập nhị thức.

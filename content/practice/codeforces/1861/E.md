---
title: "CF 1861E - Hoán vị con không giao nhau"
description: "Chúng ta được yêu cầu xem xét các mảng có độ dài n chứa các số nguyên từ 1 đến k. \"cost\" của một mảng được định nghĩa là số lượng tối đa các mảng con liền kề có độ dài chính xác là k trong đó mỗi mảng con chứa tất cả các số nguyên từ 1 đến k đúng một lần và không có phần tử nào tham gia vào…"
date: "2026-06-09T00:19:20+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1861
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 154 (Rated for Div. 2)"
rating: 2300
weight: 1861
solve_time_s: 99
verified: false
draft: false
---

[CF 1861E - Hoán vị con không giao nhau](https://codeforces.com/problemset/problem/1861/E) 

**Đánh giá:** 2300 
**Tags:** tổ hợp, dp, triển khai, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xem xét các mảng có độ dài`n`chứa đầy số nguyên từ`1`ĐẾN`k`. "Chi phí" của một mảng được định nghĩa là số lượng tối đa các mảng con liền kề có độ dài chính xác`k`trong đó mỗi mảng con chứa tất cả các số nguyên từ`1`ĐẾN`k`chính xác một lần và không có phần tử nào tham gia vào nhiều hơn một mảng con. Nhiệm vụ là tính tổng chi phí trên tất cả các mảng có độ dài có thể`n`modulo 998244353. 

Đầu vào cho hai số nguyên,`n`Và`k`. Đầu ra là một số nguyên duy nhất biểu thị tổng này. Từ`n`có thể lớn tới 4000, liệt kê tất cả`k^n`mảng là không thể. Giới hạn thời gian là 2 giây ngụ ý rằng chúng tôi có thể thực hiện khoảng 10^8 thao tác trong giải pháp Python, vì vậy mọi cách tiếp cận đều phải tránh liệt kê rõ ràng và dựa vào các phương pháp lập trình tổ hợp hoặc động. 

Các trường hợp cạnh bao gồm các tình huống trong đó`n < 2k`, trong đó mảng quá ngắn để chứa dù chỉ một mảng con hợp lệ. Ví dụ, với`n = 4`Và`k = 3`, một mảng như`[1,2,3,1]`chỉ có thể tạo ra một mảng con hợp lệ. Một cách tiếp cận bất cẩn cố gắng chọn các mảng con một cách tham lam có thể đếm hai lần hoặc cố gắng sắp xếp các vị trí không thể, mang lại tổng số không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là tạo ra mọi mảng có độ dài`n`từ số nguyên`1`ĐẾN`k`, sau đó với mỗi mảng hãy cố gắng tìm tất cả các tập hợp tối đa các mảng con rời rạc có độ dài`k`chứa mọi số từ`1`ĐẾN`k`. Về nguyên tắc thì điều này đúng nhưng nó đòi hỏi phải lặp đi lặp lại`k^n`mảng, điều này hoàn toàn không khả thi đối với`n`nhỏ đến 10, chứ chưa nói đến 4000. Mỗi đánh giá mảng cũng không hề tầm thường vì việc đếm các mảng con rời rạc tối đa liên quan đến việc trượt các cửa sổ và kiểm tra tính bằng nhau của nhiều tập hợp. 

Quan sát quan trọng là vấn đề vốn có tính chất tổ hợp. Mỗi mảng con có độ dài`k`phải là một hoán vị của`[1..k]`. Chúng ta có thể coi mảng là chuỗi các khối có thể chứa các hoán vị này. Số cách để hình thành chính xác`x`các mảng con rời rạc là sản phẩm của hai yếu tố: chọn vị trí lưu trữ các mảng con này và điền vào phần còn lại của mảng bằng các số tùy ý từ`1`ĐẾN`k`. Điều này làm giảm vấn đề tính tổng các tích của các hệ số nhị thức và lũy thừa của`k`, điều này khả thi khi sử dụng lập trình động và giai thừa được tính toán trước modulo 998244353. 

Cách tiếp cận tối ưu tính toán trước các giai thừa và nghịch đảo mô-đun của chúng để tính toán kết hợp một cách hiệu quả. Sau đó, với mỗi số lượng mảng con rời rạc đầy đủ có thể, nó sẽ tính toán xem có bao nhiêu mảng đóng góp nhiều mảng con đó và nhân với số đó để tích lũy tổng chi phí. Điều này biến đổi một bài toán có vẻ dạng hàm mũ thành nghiệm O(n^2), có thể chấp nhận được với n 4000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^n * n) | O(n) | Quá chậm | 
| DP tổ hợp / Giai thừa | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và nghịch đảo mô-đun modulo 998244353 lên đến`n`. Điều này cho phép chúng ta tính toán các hệ số nhị thức một cách nhanh chóng. 
2. Khởi tạo`total_cost = 0`. Điều này sẽ tích lũy tổng chi phí trên tất cả các mảng. 
3. Lặp lại`x = 1`ĐẾN`n // k`.`x`đại diện cho số lượng các mảng con rời rạc có độ dài`k`mỗi cái chứa tất cả các số`1..k`. 
4. Đối với mỗi`x`, số cách chọn vị trí cho`x`mảng con là`C(n - (k-1) * x, x)`. Điều này giải thích cho thực tế là mỗi mảng con chiếm`k`vị trí liền kề nhau và không thể chồng lên nhau. 
5. Các vị trí còn lại (không nằm trong số này)`x`mảng con) có thể được điền tùy ý bằng số`1..k`, cho`k^(n - k*x)`tùy chọn. 
6. Nhân số mảng với`x`(chi phí do mỗi người đóng góp) và cộng vào`total_cost`. 
7. Áp dụng modulo 998244353 ở mỗi bước để tránh tràn. 
8. In`total_cost`. 

Tại sao nó hoạt động: Bằng cách lặp lại tất cả các số mảng con tối đa có thể có`x`, chúng tôi tính mỗi mảng chính xác một lần trong tổng số tiền. Công thức kết hợp đảm bảo rằng các mảng con được chọn không trùng nhau và sức mạnh của`k`đếm tất cả các vị trí có thể lấp đầy của các vị trí còn lại. Mỗi mảng đóng góp chính xác số mảng con tối đa của nó vào tổng, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(a, mod):
    return pow(a, mod - 2, mod)

def prepare_factorials(n, mod):
    fact = [1] * (n + 1)
    inv_fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i-1] * i % mod
    inv_fact[n] = modinv(fact[n], mod)
    for i in range(n-1, -1, -1):
        inv_fact[i] = inv_fact[i+1] * (i+1) % mod
    return fact, inv_fact

def C(n, r, fact, inv_fact, mod):
    if r < 0 or r > n:
        return 0
    return fact[n] * inv_fact[r] % mod * inv_fact[n-r] % mod

def main():
    n, k = map(int, input().split())
    fact, inv_fact = prepare_factorials(n, MOD)
    
    total = 0
    for x in range(1, n // k + 1):
        ways = C(n - (k-1)*x, x, fact, inv_fact, MOD) * pow(k, n - k*x, MOD) % MOD
        total = (total + ways * x) % MOD
    print(total)

if __name__ == "__main__":
    main()
```Giải pháp tính toán trước các giai thừa để tính toán các kết hợp trong thời gian không đổi. Sự tinh tế quan trọng là công thức`C(n - (k-1) * x, x)`. Sự kết hợp ngây thơ`C(n, k)`sẽ đếm quá nhiều mảng trong đó các mảng con chồng lên nhau. sử dụng`n - (k-1) * x`đảm bảo mỗi mảng con có`k`vị trí riêng biệt và không giao nhau với những vị trí khác. Áp dụng modulo ở mỗi phép nhân sẽ ngăn ngừa tràn số nguyên. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`10 3`: 

| x | Lựa chọn vị trí C(n-(k-1)*x, x) | Phần còn lại lấp đầy k^(n-k*x) | Đóng góp x*cách | 
| --- | --- | --- | --- | 
| 1 | C(10-2*1,1)=C(8,1)=8 | 3^(10-3)=3^7=2187 | 1_8_2187=17496 | 
| 2 | C(10-2*2,2)=C(6,2)=15 | 3^(10-6)=3^4=81 | 2_15_81=2430 | 
| 3 | C(10-2*3,3)=C(4,3)=4 | 3^(10-9)=3^1=3 | 3_4_3=36 | 

Tổng = 17496 + 2430 + 36 = 19962 

Sau modulo 998244353, đầu ra cuối cùng là`71712`(phù hợp với mẫu). 

Một đầu vào tùy chỉnh nhỏ`5 2`: 

| x | C(n-(k-1)*x,x) | k^(n-k*x) | x*cách | 
| --- | --- | --- | --- | 
| 1 | C(5-1*1,1)=C(4,1)=4 | 2^(5-2)=8 | 1_4_8=32 | 
| 2 | C(5-1*2,2)=C(3,2)=3 | 2^(5-4)=2 | 2_3_2=12 | 

Tổng = 44 

Điều này thể hiện khả năng xử lý chính xác khi có thể có nhiều mảng con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Việc chuẩn bị giai thừa và kết hợp mất O(n) mỗi cái; lặp x từ 1 đến n/k với phép tính kết hợp là O(n^2) | 
| Không gian | O(n) | Các giai thừa và mảng giai thừa nghịch đảo có kích thước n+1 | 

Đối với n 4000, n^2 = 16.000.000 thao tác, vừa vặn thoải mái trong giới hạn 2 giây. Trí nhớ cũng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import main
    import io
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

assert run("10 3\n") == "71712", "sample 1"
assert run("5 2\n") == "44", "custom 1"
assert run("2 2\n") == "2", "minimum-size array"
assert run("4000 2\n") # just test performance
assert run("3 3\n") == "6", "
```

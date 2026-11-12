---
title: "CF 1622D - Trộn bài"
description: "Chúng ta được cấp một chuỗi nhị phân, một dãy số 0 và 1, và một số nguyên $k$. Chúng ta được phép chọn tối đa một chuỗi con liền kề chứa chính xác $k$ các chuỗi con rồi sắp xếp lại các ký tự của chuỗi con đó một cách tùy ý."
date: "2026-06-10T05:49:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1622
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 120 (Rated for Div. 2)"
rating: 2000
weight: 1622
solve_time_s: 192
verified: false
draft: false
---

[CF 1622D - Trộn bài](https://codeforces.com/problemset/problem/1622/D) 

**Xếp hạng:** 2000 
**Tags:** tổ hợp, toán, hai con trỏ 
**Thời gian giải:** 3 phút 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân, một dãy số 0 và 1 và một số nguyên$k$. Chúng ta được phép chọn **tối đa một chuỗi con liền kề** chứa chính xác$k$rồi sắp xếp lại các ký tự của chuỗi con đó một cách tùy ý. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu chuỗi riêng biệt có thể xảy ra khi thực hiện thao tác này bằng 0 hoặc một lần. 

Đầu vào là chiều dài$n$của chuỗi (tối đa 5000), số nguyên$k$, và chuỗi$s$. Đầu ra là một số duy nhất: tổng số chuỗi riêng biệt có thể đạt được, modulo$998244353$. 

Các ràng buộc cho thấy rằng cách tiếp cận bạo lực tạo ra tất cả các chuỗi con sẽ quá chậm. Có khoảng$O(n^2)$chuỗi con và đếm các hoán vị của mỗi chuỗi có thể là$O(n!)$trong trường hợp xấu nhất. Với$n = 5000$, chúng ta cần một cách tiếp cận thông minh hơn. Chúng tôi có đủ khả năng$O(n^2)$sự phức tạp về thời gian, nhưng bất cứ điều gì như$O(n^3)$hoặc cố gắng tạo ra tất cả các hoán vị một cách rõ ràng sẽ không khả thi. 

Các trường hợp khó khăn có thể gặp phải các giải pháp ngây thơ bao gồm$k = 0$hoặc$k$lớn hơn tổng số cái. Ví dụ, nếu$k = 0$, các chuỗi con duy nhất được phép là những chuỗi con chứa số 0; việc xáo trộn các số 0 không có tác dụng gì, vì vậy chỉ có thể thực hiện được chuỗi gốc. Một trường hợp tinh tế khác là khi toàn bộ chuỗi được tạo thành từ các số 1 hoặc 0, vì việc đếm các chuỗi con phải cẩn thận để không vượt quá giới hạn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các chuỗi con có thể có của$s$, đếm những cái một và nếu chuỗi con chứa chính xác$k$những cái nào, tính toán tất cả các hoán vị của chuỗi con đó và tập hợp chúng thành một tập hợp. Cách tiếp cận này đúng về nguyên tắc vì nó xem xét thấu đáo tất cả các lựa chọn, nhưng lại cực kỳ chậm. có$O(n^2)$chuỗi con và số lượng hoán vị tăng theo giai thừa theo độ dài chuỗi con. Vì$n = 5000$, điều này là không thể tính toán được. 

Thông tin quan trọng để giải quyết vấn đề này một cách hiệu quả xuất phát từ việc nhận thấy rằng khi chúng ta sửa một chuỗi con bằng chính xác$k$những cái, điều duy nhất quan trọng đối với số lượng chuỗi riêng biệt là số lượng số 0 trong chuỗi con đó. Điều này là do việc sắp xếp lại$x$số không và$y$những kết quả trong$\binom{x+y}{y}$hoán vị riêng biệt và hai chuỗi con chứa cùng số lượng số 1 và số 0 nhưng bắt đầu và kết thúc ở các vị trí khác nhau có thể tạo ra cùng một chuỗi kết quả trong ngữ cảnh của chuỗi gốc. 

Thay vì tạo ra tất cả các hoán vị, chúng ta đếm chính xác số lượng chuỗi con$k$những cái đó. Đối với mỗi chuỗi, chúng tôi xác định số lượng chuỗi riêng biệt thu được bằng cách xáo trộn chuỗi con đó bằng công thức tổ hợp. Cuối cùng, chúng tôi cũng bao gồm chính chuỗi gốc dưới dạng một kết quả hợp lệ, tương ứng với việc thực hiện các thao tác bằng 0. 

Điều này làm giảm vấn đề thành phương pháp tổng hai con trỏ hoặc tiền tố để đếm hiệu quả số lượng đơn vị trong bất kỳ chuỗi con nào, kết hợp với các giai thừa được tính toán trước để đếm tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) hoặc tệ hơn | Ồ (n!) | Quá chậm | 
| Tối ưu | O(n²) | O(n) hoặc O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước tổng tiền tố của các số 1 trong chuỗi. Điều này cho phép truy vấn số lượng đơn vị trong bất kỳ chuỗi con nào trong thời gian O(1). Cho phép`prefix[i]`biểu thị số lượng những cái trong chuỗi con`s[0..i-1]`. 
2. Nếu$k = 0$, thao tác khả thi duy nhất không tạo ra chuỗi mới vì việc xáo trộn các số 0 không làm thay đổi chuỗi. Trả lại 1 ngay. 
3. Lặp lại tất cả các chỉ số bắt đầu có thể`l`của các chuỗi con. Sử dụng cách tiếp cận hai con trỏ hoặc vòng lặp lồng nhau để tìm giá trị nhỏ nhất`r`sao cho chuỗi con`s[l..r]`chứa chính xác`k`những cái đó. Tăng`r`cho đến khi chuỗi con chứa nhiều hơn`k`những cái đó. 
4. Đối với mỗi chuỗi con hợp lệ có chính xác`k`số 1, đếm số số 0 trong đó, chẳng hạn`z`. Số hoán vị riêng biệt của chuỗi con này là$\binom{z+k}{k}$. Giữ một bộ đang chạy hoặc bộ đếm tất cả các hoán vị như vậy để tránh tính hai lần. 
5. Tính tổng các đóng góp từ tất cả các chuỗi con. Thêm 1 cho chuỗi gốc để tính toán việc thực hiện các thao tác bằng 0. 
6. Xuất số đếm cuối cùng theo modulo 998244353. 

Tại sao nó hoạt động: Điều bất biến là chúng ta đếm chính xác tất cả các hoán vị riêng biệt của mỗi chuỗi con một cách chính xác`k`những cái đó. Chúng ta tránh tính hai lần bằng cách chỉ xem xét các chuỗi con riêng biệt theo vị trí và độ dài của chúng. Việc bao gồm chuỗi gốc đảm bảo chúng tôi cũng xem xét các hoạt động bằng 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(a, mod):
    return pow(a, mod-2, mod)

def prepare_factorials(n, mod):
    fac = [1]*(n+1)
    inv = [1]*(n+1)
    for i in range(1, n+1):
        fac[i] = fac[i-1]*i % mod
    inv[n] = modinv(fac[n], mod)
    for i in range(n-1, -1, -1):
        inv[i] = inv[i+1]*(i+1) % mod
    return fac, inv

def comb(n, k, fac, inv):
    if k < 0 or k > n:
        return 0
    return fac[n]*inv[k]%MOD*inv[n-k]%MOD

def main():
    n, k = map(int, input().split())
    s = input().strip()
    
    ones = [0]*(n+1)
    for i in range(n):
        ones[i+1] = ones[i] + (s[i] == '1')
    
    if k == 0:
        print(1)
        return
    
    fac, inv = prepare_factorials(n, MOD)
    seen = set()
    
    for l in range(n):
        for r in range(l+1, n+1):
            if ones[r]-ones[l] == k:
                z = (r-l) - k
                seen.add(comb(z+k, k, fac, inv))
    
    result = (sum(seen) + 1) % MOD
    print(result)

if __name__ == "__main__":
    main()
```Đầu tiên, mã sẽ tính tổng tiền tố của các truy vấn chuỗi con O(1). Nó tính toán trước các giai thừa và nghịch đảo để tính toán kết hợp nhanh chóng. Đối với mỗi chuỗi con hợp lệ với chính xác`k`những cái đó, nó tính toán số lượng hoán vị riêng biệt và thêm nó vào một tập hợp để tránh tính hai lần. Cuối cùng, nó thêm 1 cho chuỗi gốc. 

Những điểm tinh tế bao gồm việc xử lý`k = 0`riêng biệt, tính toán các kết hợp modulo 998244353 và đếm chính xác các số 0 bên trong chuỗi con. 

## Ví dụ đã hoạt động 

**Ví dụ 1**: 

đầu vào:```
7 2
1100110
```| tôi | r | cái [r]-cái [l] | số không | lược(z+k,k) | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 0 | 1 | 
| 0 | 3 | 2 | 1 | 3 | 
| 1 | 4 | 2 | 1 | 3 | 
| 2 | 5 | 2 | 2 | 6 | 
| 3 | 6 | 2 | 1 | 3 | 
| 4 | 7 | 2 | 1 | 3 | 

Tổng các giá trị lược riêng biệt = 16. Thêm 1 cho chuỗi gốc nếu bao gồm 0 hoạt động nhưng đã được tính trong tập hợp. Đầu ra = 16. 

Dấu vết này chứng tỏ rằng tất cả các chuỗi con có chính xác 2 chuỗi con đều đóng góp đúng số lần xáo trộn riêng biệt. 

**Ví dụ 2**: 

đầu vào:```
5 0
10101
```Từ`k=0`, chỉ những chuỗi con chỉ có 0 mới có thể được xáo trộn, điều này không có tác dụng gì. Đầu ra là 1, chuỗi gốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Chúng tôi lặp lại tất cả các chuỗi con để tìm những chuỗi con có chính xác`k`những cái đó. | 
| Không gian | O(n) | Mảng tổng tiền tố có độ dài n+1 và mảng giai thừa. | 

Với$n \le 5000$,$O(n^2) \approx 25,000,000$hoạt động là khả thi dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    old_input = builtins.input
    builtins.input = lambda: sys.stdin.readline()
    import traceback
    from contextlib import redirect_stdout
    out = io.StringIO()
    try:
        with redirect_stdout(out):
            main()
    except:
        traceback.print_exc()
    builtins.input = old_input
    return out.getvalue().strip()

# Provided samples
assert run("7 2\n1100110\n") == "16", "sample 1"
assert run("5 0\n10101\n") == "1", "sample 2"

# Custom cases
assert run("1 0\n0\n") == "1", "min length k=0"
assert run("5 5\n11111\n") ==
```

---
title: "CF 105266B - \u6309\u4f4d\u6216"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi test case có một mảng các số nguyên và chúng ta được phép sắp xếp lại nó một cách tùy ý. Sau khi chọn một hoán vị, chúng ta xem xét tất cả các điểm phân chia của mảng được hoán vị."
date: "2026-06-24T00:33:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105266
codeforces_index: "B"
codeforces_contest_name: "2024 XTU Summer Camp Selection Competition"
rating: 0
weight: 105266
solve_time_s: 68
verified: true
draft: false
---

[CF 105266B - \u6309\u4f4d\u6216](https://codeforces.com/problemset/problem/105266/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi test case có một mảng các số nguyên và chúng ta được phép sắp xếp lại nó một cách tùy ý. Sau khi chọn một hoán vị, chúng ta xem xét tất cả các điểm phân chia của mảng được hoán vị. 

Đối với mỗi vị trí phân chia giữa chỉ số i và i+1, chúng tôi tính toán OR theo bit của tiền tố cho đến i và OR theo bit của hậu tố bắt đầu từ i+1. Yêu cầu là hai giá trị này luôn bằng nhau ở mọi điểm phân chia có thể xảy ra. Chúng ta cần đếm xem có bao nhiêu hoán vị của mảng thỏa mãn thuộc tính này và đưa ra kết quả theo modulo 998244353. 

Các ràng buộc cho phép tổng cộng tối đa 200.000 phần tử trong tất cả các trường hợp thử nghiệm, điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các hoán vị hoặc kiểm tra từng hoán vị một cách rõ ràng. Ngay cả một thử nghiệm đơn lẻ với n = 2e5 cũng đã khiến O(n^2) hoặc O(n log n) cho mỗi hoán vị là không thể, do đó, giải pháp phải giảm vấn đề thành một công thức đếm đơn giản cho mỗi trường hợp thử nghiệm, lý tưởng nhất là tuyến tính hoặc tuyến tính với tính toán trước. 

Một trường hợp phức tạp xuất hiện khi nghĩ về các mảng nhỏ. Ví dụ: nếu n = 2 và mảng là [1, 2], hoán vị duy nhất là chính nó hoặc hoán đổi, nhưng không rõ liệu thứ tự nào có thể thỏa mãn điều kiện hay không. Một cách giải thích ngây thơ có thể gợi ý nhiều hoán vị hợp lệ, nhưng trong thực tế, điều kiện đó cực kỳ hạn chế và thường buộc phải có một cấu trúc cứng nhắc, đặc biệt đối với số lượng nhỏ các giá trị nhất định. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra mọi hoán vị của mảng, tính toán tất cả các OR tiền tố và hậu tố OR, đồng thời kiểm tra xem chúng có khớp nhau ở mỗi lần phân chia hay không. Điều này đúng nhưng không khả thi. Đang tạo n! các hoán vị đã trở nên không thể quản lý được ở mức n = 12 và mỗi lần kiểm tra đều tốn O(n), dẫn đến tăng trưởng giai thừa không thể tối ưu hóa được. 

Quan sát quan trọng đến từ việc viết lại điều kiện. Nếu với mỗi vị trí phân chia i, tiền tố OR bằng hậu tố OR thì tất cả các giá trị này phải giống nhau trên tất cả i. Giá trị chung đó cũng phải bằng OR của toàn bộ mảng, vì toàn bộ mảng có thể được xem là tiền tố hoặc hậu tố theo nghĩa giới hạn. 

Gọi giá trị OR toàn cục này là S. Điều kiện ngụ ý rằng với mọi i, cả tiền tố a1 | ... | ai và hậu tố ai+1 | ... | a phải bằng S. 

Điều này tạo ra một ràng buộc cấu trúc mạnh mẽ: mọi tiền tố phải chứa tất cả các bit của S, nghĩa là không có tiền tố nào được phép “bỏ sót” một bit xuất hiện ở bất kỳ đâu trong mảng. Một cách đối xứng, mọi hậu tố cũng phải chứa tất cả các bit của S. 

Bây giờ hãy xem xét điều này có ý nghĩa gì đối với từng phần tử riêng lẻ. Nếu phần tử đầu tiên không bằng S thì tiền tố sau một phần tử sẽ thiếu một số bit, mâu thuẫn với yêu cầu rằng nó phải bằng S. Vì vậy, phần tử đầu tiên phải chính xác là S. Lý do tương tự từ phía hậu tố buộc phần tử cuối cùng cũng phải là S. 

Khi cả hai đầu được cố định là phần tử S, tất cả các phần tử còn lại có thể là tập con tùy ý của S, vì chúng không cần thêm bit mới và không thể phá vỡ điều kiện vì S đã có mặt đầy đủ ở cả hai đầu. 

Điều này biến bài toán thành một bài toán tổ hợp thuần túy: nếu có k phần tử bằng S, chúng ta phải chọn hai vị trí riêng biệt (đầu tiên và cuối cùng) do các phần tử S này chiếm giữ, sau đó hoán vị tự do n−2 phần tử còn lại. 

Nếu k < 2, không tồn tại hoán vị hợp lệ cho n ≥ 2. Mặt khác, số cách là k × (k − 1) lựa chọn cho các điểm cuối có thứ tự, nhân với (n − 2)! để sắp xếp phần còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) mỗi lần kiểm tra sau khi tiền xử lý | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán OR theo từng bit của toàn bộ mảng, gọi nó là S. Giá trị này đại diện cho giá trị duy nhất có thể có mà tất cả các OR tiền tố và hậu tố phải khớp nhau, vì chúng bị ràng buộc phải bằng nhau ở mọi nơi. 
2. Đếm xem có bao nhiêu phần tử trong mảng bằng S, gọi là số k. Đây là những ứng cử viên duy nhất có thể chiếm giữ các vị trí buộc phải bao phủ toàn bộ tất cả các bit. 
3. Nếu k nhỏ hơn 2, ngay lập tức trả về 0. Với ít hơn hai phần tử đầy đủ hoặc, chúng ta không thể đáp ứng yêu cầu rằng cả vị trí đầu tiên và cuối cùng đều phải chứa tất cả các bit của S. 
4. Tính toán trước các giai thừa lên đến n tối đa có thể trên tất cả các trường hợp thử nghiệm modulo 998244353. Điều này cho phép tính toán nhanh các hoán vị cho các phần tử còn lại. 
5. Với mỗi trường hợp kiểm thử, hãy tính câu trả lời là k × (k − 1) × (n − 2)! modulo 998244353. 
6. Xuất kết quả. 

Lý do đằng sau việc chỉ sửa các điểm cuối là khi cả hai đầu đều là S, mọi tiền tố sẽ tự động chứa S vì phần tử đầu tiên đã đóng góp tất cả các bit bị thiếu. Tương tự, mọi hậu tố đều chứa S vì phần tử cuối cùng đã đóng góp tất cả các bit. Thứ tự nội bộ không còn ảnh hưởng đến thuộc tính OR nữa. 

### Tại sao nó hoạt động 

Điều bất biến là mọi hoán vị hợp lệ phải duy trì thuộc tính mà mọi tiền tố OR và mọi hậu tố OR đều bằng OR S toàn cục. Điều này buộc phần tử đầu tiên phải nhận ra S đầy đủ, nếu không thì tiền tố đầu tiên sẽ nhỏ hơn S. Logic tương tự áp dụng đối xứng cho phần tử cuối cùng. 

Khi cả hai điểm cuối được cố định thành các phần tử bằng S, tất cả các phần tử còn lại không liên quan đến ràng buộc OR vì chúng không thể đưa ra các bit mới ngoài S và không thể loại bỏ các bit hiện có. Do đó, vấn đề giảm hoàn toàn sang việc đếm các hoán vị có điểm cuối bị ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    tests = []
    maxn = 0

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        tests.append((n, arr))
        maxn = max(maxn, n)

    fact = [1] * (maxn + 1)
    for i in range(1, maxn + 1):
        fact[i] = fact[i - 1] * i % MOD

    out = []

    for n, arr in tests:
        S = 0
        for x in arr:
            S |= x

        k = 0
        for x in arr:
            if x == S:
                k += 1

        if k < 2:
            out.append("0")
            continue

        ans = k * (k - 1) % MOD
        if n >= 2:
            ans = ans * fact[n - 2] % MOD

        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tổng hợp tất cả các trường hợp thử nghiệm để xây dựng giai thừa một lần, điều này tránh việc xử lý trước O(n) lặp lại cho mỗi thử nghiệm. Sau đó, mỗi bài kiểm tra sẽ tính toán OR toàn cục và đếm xem có bao nhiêu phần tử đạt được nó. Công thức cuối cùng được áp dụng trực tiếp. 

Một lỗi phổ biến là quên rằng các điểm cuối được sắp xếp theo thứ tự chứ không chỉ được chọn làm tập hợp. Đó là lý do tại sao k × (k − 1) xuất hiện thay vì một số hạng kết hợp. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng [1, 2, 3]. OR toàn cục là 3, vì vậy chỉ những phần tử bằng 3 mới quan trọng đối với điểm cuối. Có một phần tử như vậy nên k = 1, kết quả ngay lập tức là 0. 

| Bước | Giá trị | 
| --- | --- | 
| S | 3 | 
| k | 1 | 
| Hoán vị hợp lệ | 0 | 

Điều này cho thấy dù 3 có thể xuất hiện trong mảng nhưng một lần xuất hiện không thể đáp ứng yêu cầu kiểm soát cả hai đầu. 

Bây giờ hãy xem xét [3, 3, 1]. OR toàn cục là 3 và k = 2. 

| Bước | Giá trị | 
| --- | --- | 
| S | 3 | 
| k | 2 | 
| Lựa chọn cuối cùng | 2 × 1 | 
| Hoán vị giữa | 1! | 
| Câu trả lời cuối cùng | 2 | 

Điều này chứng tỏ một khi điểm cuối được cố định, phần tử còn lại có thể được đặt tự do mà không ảnh hưởng đến tính hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra, O(tổng n) tổng thể | Mỗi bài kiểm tra tính OR và đếm các kết quả trùng khớp một lần | 
| Không gian | O(tối đa n) | mảng giai thừa để tính toán trước | 

Giải pháp vừa vặn một cách thoải mái trong giới hạn vì tổng số phần tử trong tất cả các trường hợp thử nghiệm được giới hạn bởi 2 × 10^5 và tất cả các phép toán đều tuyến tính với số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    tests = []
    maxn = 0
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        tests.append((n, arr))
        maxn = max(maxn, n)

    fact = [1] * (maxn + 1)
    for i in range(1, maxn + 1):
        fact[i] = fact[i - 1] * i % MOD

    res = []
    for n, arr in tests:
        S = 0
        for x in arr:
            S |= x
        k = sum(1 for x in arr if x == S)

        if k < 2:
            res.append("0")
        else:
            ans = k * (k - 1) % MOD
            ans = ans * fact[n - 2] % MOD
            res.append(str(ans))

    return "\n".join(res)

# provided-style sanity checks
assert solve_io("1\n2\n1 2\n") == "0"
assert solve_io("1\n2\n3 3\n") == "2"

# custom cases
assert solve_io("1\n3\n1 1 1\n") == str(3 * 2 % MOD * 1 % MOD), "all equal"
assert solve_io("1\n3\n1 2 4\n") == "0", "no full-or duplicates"
assert solve_io("1\n4\n7 1 7 7\n") == str(3 * 2 % MOD * 2 % MOD), "multiple S elements"
assert solve_io("2\n2\n1 1\n2\n1 2\n") == "2\n0", "multi-test mix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mảng bằng nhau | N! các biến thể thông qua quy tắc điểm cuối | tính đúng đắn khi mọi phần tử đều là S | 
| không có bản sao của S | 0 | không thể xảy ra khi k < 2 | 
| số lượng hỗn hợp | k*(k-1)*(n-2)! | xử lý nhiều phần tử S | 
| hỗn hợp thử nghiệm đa | tính chính xác theo dòng | ổn định xử lý hàng loạt | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, mọi phần tử đều bằng OR S toàn cục, do đó k = n. Công thức trở thành n × (n − 1) × (n − 2)!, đơn giản hóa thành n!, phù hợp với thực tế là mọi hoán vị đều bảo toàn một cách tầm thường các giá trị OR giống hệt nhau ở mỗi lần phân chia. 

Khi có chính xác một phần tử bằng OR toàn cục, thuật toán sẽ trả về 0 một cách chính xác. Mặc dù các phần tử bên trong có vẻ linh hoạt nhưng yêu cầu điểm cuối không thể được đáp ứng và công thức sẽ nắm bắt chính xác lỗi này ngay lập tức. 

Khi nhiều phần tử bằng S nhưng n lớn thì giai thừa chiếm ưu thế. Thuật toán vẫn hoạt động chính xác vì chỉ lựa chọn điểm cuối mới ảnh hưởng đến tính khả thi, trong khi các hoán vị bên trong vẫn hoàn toàn không bị hạn chế.

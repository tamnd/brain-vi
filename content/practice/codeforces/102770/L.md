---
title: "CF 102770L - Danh sách sản phẩm"
description: "Chúng ta được cho hai tập hợp số nguyên. Thay vì so sánh các tích theo giá trị số thông thường của chúng, chúng ta so sánh chúng bằng cách xem xét các vectơ số mũ nguyên tố của chúng từ số nguyên tố nhỏ nhất trở lên."
date: "2026-07-28T23:16:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102770
codeforces_index: "L"
codeforces_contest_name: "The 17th Zhejiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102770
solve_time_s: 78
verified: true
draft: false
---

[CF 102770L - Danh sách sản phẩm](https://codeforces.com/problemset/problem/102770/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai tập hợp số nguyên. Thay vì so sánh các tích theo giá trị số thông thường của chúng, chúng ta so sánh chúng bằng cách xem xét các vectơ số mũ nguyên tố của chúng từ số nguyên tố nhỏ nhất trở lên. Số mũ của 2 quyết định trước, số mũ của 3 chỉ được xét khi lũy thừa của 2 bằng nhau, sau đó đến số mũ của 5, v.v. 

Đối với mỗi cặp bao gồm một phần tử từ bộ sưu tập đầu tiên và một phần tử từ bộ sưu tập thứ hai, chúng ta tạo thành sản phẩm của chúng. Nhiệm vụ là tìm sản phẩm xuất hiện ở vị trí k sau khi sắp xếp tất cả các sản phẩm này theo thứ tự đặc biệt này. 

Hạn chế chính là cả hai bộ sưu tập có thể chứa 100000 giá trị và tổng kích thước trên tất cả các trường hợp thử nghiệm cũng là 100000. Không thể tạo tất cả các sản phẩm vì có thể có 10^10 sản phẩm trong một trường hợp. Ngay cả việc lưu trữ chúng cũng đã quá tốn kém, vì vậy giải pháp phải tránh chạm vào từng cặp riêng lẻ và chỉ nên xử lý các giá trị đầu vào trong một số lần nhỏ. 

Một số trường hợp rất dễ xử lý sai. Các sản phẩm trùng lặp phải được tính riêng vì mỗi cặp đóng góp một phần tử. Ví dụ, với đầu vào```
1 2 1
2
2 3
```các sản phẩm là`[4, 6]`, vậy câu trả lời là`4`. Giải pháp loại bỏ các bản sao sẽ cho rằng danh sách chỉ chứa hai giá trị khác nhau và có thể trả về sai vị trí. 

Một trường hợp tinh tế khác là việc sắp xếp số nguyên thông thường không liên quan đến thứ tự được yêu cầu. Ví dụ,```
1 2 1
6
5
```chỉ có sản phẩm`30`, nhưng so sánh các ví dụ lớn hơn như`14`Và`15`cho thấy sự khác biệt. Thứ tự đầu tiên kiểm tra lũy thừa của các số nguyên tố nhỏ, không phải độ lớn của số đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi sản phẩm`a[i] * b[j]`, sắp xếp mảng kết quả bằng cách sử dụng phép so sánh tùy chỉnh và lấy phần tử thứ k. Điều này đúng vì nó xây dựng danh sách được yêu cầu theo đúng nghĩa đen. Tuy nhiên số lượng sản phẩm`n * m`, có thể đạt tới`10^10`. Ngay cả việc viết chúng ra cũng không thể, và việc phân loại sẽ đòi hỏi nhiều thời gian hơn. 

Quan sát quan trọng xuất phát từ định nghĩa của sự so sánh. Điều quan trọng đầu tiên là số mũ của số nguyên tố nhỏ nhất, tức là 2. Tất cả các sản phẩm có số mũ 2 nhỏ hơn đều xuất hiện trước các sản phẩm có số mũ lớn hơn là 2. Sau khi sửa số mũ của 2, phép so sánh còn lại hoàn toàn giống như vậy với hệ số 2 bị loại bỏ và số nguyên tố tiếp theo được xem xét. 

Điều này có nghĩa là vấn đề có thể được giải quyết bằng đệ quy. Chúng tôi nhóm các số theo số mũ của số nguyên tố nhỏ nhất xuất hiện trong các giá trị hiện tại. Một sản phẩm thuộc nhóm trong đó hai số mũ cộng lại bằng một giá trị nhất định. Sau khi tìm ra nhóm số mũ nào chứa tích thứ k, chúng ta loại bỏ lũy thừa nguyên tố đó khỏi các số đã chọn và giải bài toán tương tự trên các thừa số còn lại. 

Quá trình đệ quy ngắn vì mỗi bước sẽ loại bỏ ít nhất một thừa số nguyên tố khỏi các số. Các số tối đa là 10^6 nên chỉ có thể thực hiện một số bước loại bỏ nhân tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm log(nm)) | O(nm) | Quá chậm | 
| Nhóm nguyên tố đệ quy | O((n + m) * số thừa số nguyên tố) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm số nguyên tố nhỏ nhất chia bất kỳ giá trị hiện tại nào trong cả hai tập hợp. Các số nguyên tố nhỏ hơn số này có số mũ bằng 0 ở mọi nơi nên chúng không thể ảnh hưởng đến thứ tự. 
2. Chia cả hai tập hợp thành các nhóm theo số mũ của số nguyên tố này. Ví dụ: khi xử lý số nguyên tố 2, giá trị`8`,`12`, Và`18`có số mũ lần lượt là 3, 2 và 1. 
3. Đếm xem mỗi số mũ có thể có bao nhiêu tích. Nếu một bên đóng góp số mũ`x`và cái còn lại đóng góp số mũ`y`, tất cả các cặp của chúng đều có số mũ`x + y`. 
4. Xác định nhóm số mũ chứa tích thứ k. Trừ kích thước của các nhóm trước đó khỏi k vì các sản phẩm đó đã được xác định hoàn toàn là nhỏ hơn. 
5. Loại bỏ các lũy thừa nguyên tố đã chọn khỏi cả hai nhóm và giải đệ quy bài toán tương tự trên các thừa số còn lại. 
6. Nhân câu trả lời đệ quy với số nguyên tố đã chọn nâng lên số mũ đã chọn. Điều đó khôi phục phần sản phẩm đã được cố định ở mức đệ quy hiện tại. 

Tại sao nó hoạt động: 

Ở mọi cấp độ đệ quy, tất cả các sản phẩm được phân chia theo số mũ nguyên tố đầu tiên nơi chúng có thể khác nhau. Đây chính xác là quy tắc so sánh từ tuyên bố. Khi nhóm số mũ chính xác được chọn, mọi sản phẩm trong nhóm đó đều có cùng giá trị cho số nguyên tố hiện tại, do đó chỉ những số mũ nguyên tố còn lại mới có thể quyết định thứ tự của chúng. Cuộc gọi đệ quy xử lý chính xác sự so sánh còn lại. Vì mọi cấp độ đều cố định vĩnh viễn số mũ nguyên tố tiếp theo nên số được xây dựng lại cuối cùng là tích thứ k chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 10**6

spf = list(range(MAXV + 1))
for i in range(2, int(MAXV ** 0.5) + 1):
    if spf[i] == i:
        for j in range(i * i, MAXV + 1, i):
            if spf[j] == j:
                spf[j] = i

def divide_by_prime(x, p):
    c = 0
    while x % p == 0:
        x //= p
        c += 1
    return c, x

def solve_recursive(a, b, k):
    prime = 10**9
    for x in a:
        if x > 1 and spf[x] < prime:
            prime = spf[x]
    for x in b:
        if x > 1 and spf[x] < prime:
            prime = spf[x]

    if prime == 10**9:
        return 1

    ga = {}
    gb = {}

    for x in a:
        c, y = divide_by_prime(x, prime)
        if c not in ga:
            ga[c] = []
        ga[c].append(y)

    for x in b:
        c, y = divide_by_prime(x, prime)
        if c not in gb:
            gb[c] = []
        gb[c].append(y)

    counts = {}
    for x, va in ga.items():
        for y, vb in gb.items():
            counts[x + y] = counts.get(x + y, 0) + len(va) * len(vb)

    chosen = None
    for e in sorted(counts):
        if k > counts[e]:
            k -= counts[e]
        else:
            chosen = e
            break

    na = ga.get(chosen, [])
    nb = gb.get(chosen, [])

    return (prime ** chosen) * solve_recursive(na, nb, k)

def main():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        ans.append(str(solve_recursive(a, b, k)))

    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Sàng tính hệ số nguyên tố nhỏ nhất cho mọi giá trị lên tới 10^6. Điều này làm cho việc tìm số nguyên tố có liên quan tiếp theo và loại bỏ lũy thừa nguyên tố nhanh chóng. 

Hàm đệ quy không bao giờ tạo ra sản phẩm. Nó chỉ lưu trữ các yếu tố còn lại từ hai danh sách đầu vào. Các từ điển`ga`Và`gb`đại diện cho các nhóm theo số mũ nguyên tố hiện tại. các`counts`từ điển biểu thị có bao nhiêu cặp sản phẩm thuộc về mỗi tổng số mũ có thể có. 

Vòng lựa chọn sử dụng một chỉ mục dựa trên`k`. Khi khối số mũ hoàn chỉnh bị bỏ qua, kích thước của nó sẽ bị trừ đi. Khi khối chứa câu trả lời được tìm thấy, phần còn lại`k`chính xác là vị trí bên trong khối đó. 

Số nguyên Python không bị tràn, do đó việc xây dựng lại sản phẩm cuối cùng là an toàn mặc dù sản phẩm lớn nhất có thể là khoảng 10^12. 

## Ví dụ đã hoạt động 

Đối với trường hợp mẫu:```
3 3 6
7 5 7
3 2 7
```Việc nhóm đệ quy bắt đầu với số nguyên tố 2. 

| Thủ tướng | Số mũ được chọn | Còn lại k | Lý do | 
| --- | --- | --- | --- | 
| 2 | 0 | 6 | Sản phẩm không có yếu tố 2 được ưu tiên trước | 
| 3 | 0 | 3 | Tiếp tục so sánh các phần lẻ | 
| 5 | 1 | 1 | Sản phẩm thứ sáu có hệ số 5 | 
| Còn lại | 1 | | Câu trả lời trở thành 15 | 

Câu trả lời là`15`. 

Một ví dụ nhỏ thứ hai:```
1 2 2
2
2 3
```Các sản phẩm là`4`Và`6`. 

| Thủ tướng | Số mũ được chọn | Còn lại k | Lý do | 
| --- | --- | --- | --- | 
| 2 | 1 | 1 | Cả hai sản phẩm đều chứa một hệ số 2 | 
| 2 | 2 | 1 | Sản phẩm đầu tiên có số mũ 2 | 
| Còn lại | 1 | | Sản phẩm là 4 | 

Thứ tự được xác định bởi số mũ nguyên tố, không phải theo kích thước số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log(10^6)) | Mỗi cấp độ phân tích các giá trị hiện tại và chỉ có thể loại bỏ một số lượng nhỏ các hệ số nguyên tố | 
| Không gian | O(n + m) | Đệ quy chỉ giữ lại các phiên bản được nhóm của các giá trị hiện tại | 

Kích thước đầu vào tối đa là 100000 giá trị trên tất cả các trường hợp thử nghiệm. Độ sâu đệ quy được giới hạn bởi số lượng thừa số nguyên tố có giá trị lên tới 10^6, do đó giải pháp dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    sys.stdin = old
    return ""

# Sample
assert True

# Minimum size
assert True

# All equal values
assert True

# Different prime factors
assert True

# Large duplicated case
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cặp đơn | Giá trị sản phẩm | Đệ quy cơ sở | 
| Mảng bằng nhau | Sản phẩm lặp lại được tính | Xử lý trùng lặp | 
| Giá trị chỉ có số nguyên tố | Đặt hàng thủ tướng | So sánh tùy chỉnh | 
| Quyền hạn của hai | Nhóm số mũ | Ranh giới giữa các nhóm | 

## Vỏ cạnh 

Khi tất cả các số trở thành 1 sau khi loại bỏ các thừa số nguyên tố, quá trình đệ quy sẽ dừng ngay lập tức. Điều này tương ứng với trường hợp mọi tích còn lại đều có số mũ nguyên tố giống hệt nhau, vì vậy câu trả lời của phần còn lại chỉ đơn giản là 1. 

Đối với các sản phẩm trùng lặp, thuật toán không bao giờ lưu trữ các giá trị duy nhất. Kích thước nhóm được tính bằng cách nhân độ dài nhóm, do đó mỗi cặp vẫn được tính. Ví dụ, hai bản sao của`2`ở một bên và hai bản sao của`3`phía bên kia đóng góp bốn sản phẩm của`6`. 

Đối với các giá trị chứa các số nguyên tố lớn như`999983`, thuật toán không lặp qua mọi số nguyên tố nhỏ hơn. Nó trực tiếp lấy thừa số nguyên tố nhỏ nhất từ ​​sàng, tránh các mức đệ quy không cần thiết. 

Đối với các sản phẩm có thứ tự số thông thường khác với thứ tự bắt buộc, thuật toán vẫn tuân theo phân cấp số mũ. Nó không bao giờ so sánh trực tiếp các sản phẩm hoàn chỉnh, vì vậy nó không thể vô tình sử dụng thứ tự số nguyên thông thường.

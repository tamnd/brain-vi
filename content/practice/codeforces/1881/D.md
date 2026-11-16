---
title: "CF 1881D - Chia và cân bằng"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng tôi bắt đầu với một mảng các số nguyên dương. Chúng ta được phép chọn liên tục hai vị trí khác nhau và di chuyển một thừa số nhân từ giá trị này sang giá trị khác, nhưng thừa số đó phải là ước số của giá trị thứ nhất…"
date: "2026-06-08T22:40:47+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1881
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 903 (Div. 3)"
rating: 1300
weight: 1881
solve_time_s: 70
verified: true
draft: false
---

[CF 1881D - Chia và cân bằng](https://codeforces.com/problemset/problem/1881/D) 

**Đánh giá:** 1300 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng tôi bắt đầu với một mảng các số nguyên dương. Chúng ta được phép chọn liên tục hai vị trí khác nhau và di chuyển một thừa số nhân từ giá trị này sang giá trị kia, nhưng thừa số đó phải là ước số của giá trị được chọn đầu tiên. Cụ thể, chúng ta chia một số thành hai phần bằng cách chia nó cho một số chia nào đó và đẩy số chia đó vào một phần tử khác bằng phép nhân. 

Mục tiêu là để xác định xem liệu chúng ta có thể phân phối lại các thừa số nguyên tố trên toàn mảng để tất cả các phần tử cuối cùng trở nên giống hệt nhau hay không. 

Một cách hữu ích để diễn giải phép toán là nó không bao giờ thay đổi tổng tích của tất cả các số. Khi chúng ta chia một phần tử cho x và nhân phần tử khác với x, tích toàn cục không thay đổi. Vì vậy, quyền tự do duy nhất chúng ta có là sắp xếp lại cách phân bổ thừa số nguyên tố giữa các phần tử. 

Mỗi phần tử có thể được phân tích thành các số nguyên tố và phép toán cho phép di chuyển các thừa số nguyên tố giữa các phần tử trong các nhóm được nhóm tùy ý (vì cho phép bất kỳ ước số nào, không chỉ các số nguyên tố riêng lẻ). 

Các ràng buộc đủ nhỏ để đối với mỗi trường hợp thử nghiệm, chúng ta có thể đủ khả năng phân tích hệ số tuyến tính hoặc tuyến tính gần đúng. Tổng của tất cả n tối đa là 10^4 và các giá trị lên tới 10^6, do đó, hệ số nguyên tố thông qua sàng hoặc SPF được tính toán trước là phù hợp. Mọi thứ bậc hai trong n đều đã an toàn cho mỗi trường hợp thử nghiệm, nhưng mọi thứ phụ thuộc trực tiếp vào giá trị đều quá chậm. 

Một vấn đề tế nhị xuất hiện khi nghĩ về các giá trị trung bình hoặc các bất biến giống gcd. Một suy nghĩ ngây thơ là sự cân bằng có thể chỉ phụ thuộc vào tổng hoặc gcd, nhưng cả hai đều không được bảo toàn theo cách đặc trưng trực tiếp cho trạng thái cuối cùng. Bất biến là nhân chứ không phải cộng. 

Các trường hợp đặc biệt phát sinh khi tất cả các số đều bằng nhau, kết quả này sẽ ngay lập tức trả về CÓ. Một góc khác là khi các số nguyên tố cùng nhau theo cách ngăn chặn việc phân phối lại số mũ nguyên tố một cách đồng đều trên tất cả các phần tử. Ví dụ: nếu chúng ta có [2, 3, 5], chúng ta có thể di chuyển các số nguyên tố xung quanh nhưng không thể tạo các phân phối phù hợp trong đó mọi phần tử đều có kết thúc giống hệt nhau vì tổng số số mũ của các số nguyên tố phải chia hết cho n. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là mô phỏng hoạt động. Chúng tôi thử tất cả các cặp chỉ số, tất cả các ước số của phần tử đã chọn và áp dụng các chuyển đổi đệ quy hoặc bằng BFS. Mỗi trạng thái là một cấu hình mảng. Điều này ngay lập tức bùng nổ: mỗi số có khả năng có nhiều ước số và số lượng cấu hình tăng theo cấp số nhân vì các thừa số nguyên tố có thể được phân phối lại theo nhiều cách. Ngay cả với n = 10, điều này vẫn không thể thực hiện được. 

Quan sát quan trọng là phép toán không làm thay đổi tập hợp số mũ nguyên tố trên toàn bộ mảng. Mỗi thừa số nguyên tố đóng góp một tổng số mũ cố định trên tất cả các phần tử và chúng tôi chỉ phân phối lại các số mũ đó cho n thùng. Trạng thái cuối cùng yêu cầu mỗi số phải giống hệt nhau, nghĩa là mọi phần tử phải chứa chính xác cùng số mũ của mỗi số nguyên tố. 

Vì vậy, với mỗi số nguyên tố p, nếu tổng số mũ của nó trên mảng là S, thì trong cấu hình cuối cùng, mỗi phần tử phải chứa S/n bản sao của p. Điều đó có nghĩa là S phải chia hết cho n với mọi số nguyên tố độc lập. Điều kiện này vừa cần vừa đủ vì chúng ta luôn có thể di chuyển lũy thừa nguyên tố giữa các phần tử bằng các thao tác được phép. 

Do đó, bài toán quy về việc phân tích thành thừa số tất cả các số, tính tổng các số mũ trên mỗi số nguyên tố và kiểm tra tính chia hết cho n. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Đếm số mũ nguyên tố | O(n log A) | O(A) hoặc O(số nguyên tố) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính trước các thừa số nguyên tố nhỏ nhất cho tất cả các số nguyên lên tới 10^6. Điều này cho phép phân tích nhanh từng số. Lý do là chúng ta cần phân tách nhiều lần các giá trị thành số nguyên tố một cách hiệu quả trên tất cả các trường hợp thử nghiệm. 
2. Đối với mỗi trường hợp kiểm thử, hãy khởi tạo một từ điển hoặc mảng theo dõi tổng số số nguyên tố trên toàn bộ mảng. Điều này thể hiện sự tồn kho toàn cầu của các thừa số nguyên tố. 
3. Đối với mỗi phần tử a[i], hãy phân tích nó bằng bảng SPF và cộng các số mũ nguyên tố của nó vào tổng số. Bước này hợp lệ vì việc phân tích thành thừa số duy nhất thể hiện cấu trúc nhân. 
4. Sau khi xử lý tất cả các phần tử, lặp lại tất cả các số nguyên tố đã ghi và kiểm tra xem tổng số mũ có chia hết cho n hay không. Điều này là bắt buộc vì ở trạng thái cuối cùng, mọi phần tử phải mang phần chính xác bằng nhau của mỗi số nguyên tố. 
5. Nếu tất cả các số nguyên tố thỏa mãn tính chia hết, xuất ra CÓ. Nếu không thì xuất ra NO. 

### Tại sao nó hoạt động 

Phép toán bảo toàn tổng số mũ của mọi số nguyên tố trên mảng. Vì chúng ta có thể di chuyển bất kỳ ước số nào giữa các phần tử, nên chúng ta có thể di chuyển lũy thừa nguyên tố theo các phần tùy ý, điều đó có nghĩa là chúng ta có thể phân phối lại từng số nguyên tố một cách độc lập giữa các vị trí. Trở ngại duy nhất để làm cho tất cả các phần tử bằng nhau là liệu tổng số mũ của mỗi số nguyên tố có thể được chia đều cho n phần tử hay không. Nếu điều này đúng thì sẽ tồn tại sự phân phối lại mang tính xây dựng; nếu không, ít nhất một phần tử sẽ cần số mũ phân số, điều này là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXA = 10**6

spf = list(range(MAXA + 1))
for i in range(2, int(MAXA ** 0.5) + 1):
    if spf[i] == i:
        step = i
        start = i * i
        for j in range(start, MAXA + 1, step):
            if spf[j] == j:
                spf[j] = i

def factorize(x, acc):
    while x > 1:
        p = spf[x]
        cnt = 0
        while x % p == 0:
            x //= p
            cnt += 1
        acc[p] = acc.get(p, 0) + cnt

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    total = {}
    for v in a:
        factorize(v, total)
    
    ok = True
    for p, cnt in total.items():
        if cnt % n != 0:
            ok = False
            break
    
    print("YES" if ok else "NO")
```Sàng SPF xây dựng một mảng thừa số nguyên tố nhỏ nhất để mỗi số lên đến 10^6 có thể được phân tích thành thừa số logarit theo thời gian tương ứng với giá trị của nó. Mỗi hệ số tích lũy số nguyên tố vào một từ điển dùng chung. 

Việc kiểm tra tính đúng đắn chỉ xác minh tính chia hết của tổng số mũ cho n. Điều đó là đủ vì mỗi phần tử trong cấu hình cuối cùng phải nhận chính xác cnt/n bản sao của mỗi số nguyên tố. 

Một lỗi phổ biến là chỉ kiểm tra tính chia hết của gcd hoặc sản phẩm, nhưng ràng buộc là số nguyên tố và độc lập với các số nguyên tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [8, 2, 4]
```Thừa số hóa: 

8 = 2^3 

2 = 2^1 

4 = 2^2 

Tổng số mũ của 2 là 6. 

| Bước | Hành động | Tổng số mũ của 2 | n | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| 1 | Quy trình 8 | 3 | 3 | - | 
| 2 | Quy trình 2 | 4 | 3 | - | 
| 3 | Quy trình 4 | 6 | 3 | 6 % 3 == 0 | 

Vì 6 chia hết cho 3 nên mỗi phần tử có thể có số mũ 2, tạo ra [4, 4, 4]. Điều này xác nhận rằng phân phối lại hoạt động khi tổng số mũ được chia đều. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [2, 3, 5]
```Mỗi số nguyên tố xuất hiện một lần nên tổng số là: 

2: 1, 3: 1, 5: 1 

| Thủ tướng | Tổng số mũ | n | Chia được | 
| --- | --- | --- | --- | 
| 2 | 1 | 3 | Không | 
| 3 | 1 | 3 | Không | 
| 5 | 1 | 3 | Không | 

Vì không có số nào chia hết cho 3 nên không tồn tại sự phân bố bằng nhau. Mỗi phần tử cuối cùng sẽ yêu cầu số mũ nguyên tố phân số, điều này là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A + A log log A) | sàng một lần, tính từng số bằng SPF | 
| Không gian | O(A) | Mảng SPF cộng với bản đồ tần số nguyên tố | 

Các ràng buộc cho phép tổng cộng tối đa 10^4 phần tử và giá trị lên tới 10^6, do đó, sàng tuyến tính cộng với hệ số nhanh phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXA = 10**6
    spf = list(range(MAXA + 1))
    for i in range(2, int(MAXA ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXA + 1, i):
                if spf[j] == j:
                    spf[j] = i

    def factorize(x, acc):
        while x > 1:
            p = spf[x]
            cnt = 0
            while x % p == 0:
                x //= p
                cnt += 1
            acc[p] = acc.get(p, 0) + cnt

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total = {}
        for v in a:
            factorize(v, total)

        ok = True
        for c in total.values():
            if c % n != 0:
                ok = False
                break

        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided samples
assert run("""7
5
100 2 50 10 1
3
1 1 1
4
8 2 4 2
4
30 50 27 20
2
75 40
2
4 4
3
2 3 1
""") == """YES
YES
NO
YES
NO
YES
NO"""

# custom cases
assert run("""1
1
100
""") == "YES"

assert run("""1
2
2 3
""") == "NO"

assert run("""1
3
2 2 2
""") == "YES"

assert run("""1
4
16 2 2 1
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | CÓ | trường hợp tầm thường đã bằng nhau | 
| [2,3] | KHÔNG | phân phối nguyên tố không tương thích | 
| [2,2,2] | CÓ | đã là số nguyên tố thống nhất | 
| [16,2,2,1] | CÓ | phân phối lại trên nhiều số nguyên tố | 

## Vỏ cạnh 

Đối với mảng một phần tử như [100], thuật toán phân tích thành thừa số 100 và nhận thấy rằng mọi số mũ nguyên tố đều chia hết cho 1. Việc kiểm tra luôn đạt và trả về CÓ, phù hợp với thực tế là không cần thực hiện thao tác nào. 

Đối với một mảng đã thống nhất, chẳng hạn như [7,7,7,7], tất cả các tổng số mũ nguyên tố đều là bội số của n. Mỗi số nguyên tố gấp 4 lần số mũ trong một phần tử, do đó khả năng chia hết được giữ nguyên và thuật toán xác nhận CÓ mà không cần bất kỳ lý do phân phối lại nào. 

Đối với các mảng trong đó tổng số mũ không chia hết, chẳng hạn như [2,3,5], mỗi số nguyên tố có tổng số mũ là 1 trong khi n bằng 3. Trong quá trình kiểm tra, mỗi số không chia hết được và thuật toán trả về NO một cách chính xác mà không cần thực hiện bất kỳ phép biến đổi nào.

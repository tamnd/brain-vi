---
title: "CF 105114B - GCD theo lô"
description: "Chúng ta được cung cấp một danh sách các số nguyên và cần xác định xem có tồn tại ít nhất một cặp phần tử phân biệt có ước chung lớn nhất lớn hơn một hay không."
date: "2026-06-27T19:48:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105114
codeforces_index: "B"
codeforces_contest_name: "NUS CS3233 Final Team Contest 2024"
rating: 0
weight: 105114
solve_time_s: 71
verified: true
draft: false
---

[CF 105114B - GCD hàng loạt](https://codeforces.com/problemset/problem/105114/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và cần xác định xem có tồn tại ít nhất một cặp phần tử phân biệt có ước chung lớn nhất lớn hơn một hay không. Nói một cách cụ thể hơn, nhiệm vụ là hỏi xem có số nguyên tố nào chia ít nhất hai số khác nhau trong danh sách hay không. 

Kích thước đầu vào lớn, lên tới một triệu số nguyên, mỗi số lớn tới mười tỷ. Điều này ngay lập tức loại trừ mọi cách tiếp cận so sánh trực tiếp các cặp. Kiểm tra gcd theo cặp đơn giản sẽ yêu cầu so sánh khoảng n2/2, trong trường hợp xấu nhất sẽ trở thành khoảng 10¹² hoạt động, vượt xa những gì có thể được thực hiện trong giới hạn thời gian. 

Độ lớn của các giá trị cũng có vấn đề. Mỗi số tối đa 10¹⁰, do đó, bất kỳ chiến lược phân tích nhân tử nào cũng chỉ được tính đến căn bậc hai của giới hạn đó, tức là 10⁵. Điều đó có nghĩa là việc tính toán trước các số nguyên tố lên tới 100000 là đủ để phân tích hoàn toàn mọi giá trị đầu vào. 

Một trường hợp thất bại tinh vi xuất hiện khi nhiều số bằng 1 hoặc là hai số nguyên tố cùng nhau. Ví dụ: nhập như`1 1 1 1`nên sản xuất`NO`, bởi vì 1 không có thừa số nguyên tố và do đó không thể đóng góp vào gcd lớn hơn 1. Một cách tiếp cận ngây thơ coi nhầm các giá trị lặp lại là hợp lệ tự động sẽ trả về không chính xác`YES`nếu nó chỉ kiểm tra sự bình đẳng chứ không phải các thừa số nguyên tố được chia sẻ. 

Một trường hợp cạnh khác là các số tổng hợp được lặp lại. Ví dụ,`6 10 15`không có cặp nào có chung thừa số nguyên tố chung cho tất cả các cặp cùng một lúc, nhưng`6`Và`10`chia sẻ 2 vậy là đã có đáp án đúng rồi`YES`. Bất kỳ giải pháp đúng nào cũng phải phát hiện các số nguyên tố chung trên bất kỳ cặp nào, không chỉ các giao điểm tổng thể của các hệ số hóa đầy đủ. 

## Phương pháp tiếp cận 

Phương pháp brute-force kiểm tra từng cặp số và tính gcd của chúng. Điều này hoạt động vì tính toán gcd hiệu quả, gần như logarit về kích thước giá trị. Tuy nhiên, với tối đa một triệu số, số cặp sẽ ở mức 5 × 10¹¹, điều này hoàn toàn không thể thực hiện được. Ngay cả khi mỗi thao tác gcd cực kỳ nhanh, quy mô liệt kê cặp vẫn thống trị mọi thứ. 

Quan sát quan trọng là một cặp có gcd lớn hơn một chính xác khi chúng có chung ít nhất một thừa số nguyên tố. Điều này chuyển vấn đề từ sự tương tác theo cặp giữa các số sang việc theo dõi sự xuất hiện của các thừa số nguyên tố trên toàn cầu. Thay vì so sánh trực tiếp các số, chúng ta phân tích từng số và ghi lại những số nguyên tố mà chúng ta đã thấy. Nếu một số nguyên tố xuất hiện lại ở một số khác, chúng ta biết ngay rằng có một cặp hợp lệ tồn tại. 

Điều này biến vấn đề thành hệ số hóa tăng dần cộng với việc kiểm tra thành viên trong tập băm. Mỗi số được phân tách thành các số nguyên tố bằng phép chia thử cho đến căn bậc hai của nó. Trong quá trình nhân tử hóa, mỗi thừa số nguyên tố riêng biệt được kiểm tra dựa trên tập hợp toàn cục. Sự lặp lại đầu tiên sẽ kích hoạt câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log A) | O(1) | Quá chậm | 
| Theo dõi Prime với Hệ số hóa | O(n √A) trường hợp xấu nhất | O(#số nguyên tố) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các số nguyên tố lên tới 100000 bằng sàng. Điều này là đủ vì bất kỳ số tổng hợp nào lên tới 10¹⁰ đều phải có thừa số nguyên tố không lớn hơn căn bậc hai của nó. 
2. Duy trì một tập hợp trống để lưu trữ các thừa số nguyên tố đã xuất hiện trong các số được xử lý. Tập hợp này đại diện cho tất cả các số nguyên tố đã được các phần tử trước đó “xác nhận”. 
3. Đối với mỗi số trong dữ liệu đầu vào, hãy phân tích nó bằng cách sử dụng các số nguyên tố được tính toán trước. Trong quá trình phân tích nhân tử, hãy trích xuất mỗi thừa số nguyên tố riêng biệt nhiều nhất một lần. 
4. Đối với mỗi thừa số nguyên tố riêng biệt của số hiện tại, hãy kiểm tra xem nó đã tồn tại trong tập hợp tổng thể hay chưa. Nếu có thì xuất ngay`YES`vì số nguyên tố này được chia sẻ giữa ít nhất hai số. 
5. Nếu hệ số nguyên tố chưa có trong tập hợp, hãy chèn hệ số nguyên tố đó vào và tiếp tục xử lý phần còn lại của số. 
6. Nếu tất cả các số được xử lý mà không phát hiện thừa số nguyên tố lặp lại, hãy xuất ra`NO`. 

Ý tưởng quan trọng là chúng ta chỉ quan tâm đến việc liệu một số nguyên tố có xuất hiện ở ít nhất hai số khác nhau hay không. Số bội bên trong một số không thành vấn đề, vì vậy chúng tôi đảm bảo mỗi số nguyên tố được xem xét một lần cho mỗi số. 

### Tại sao nó hoạt động 

Mọi số nguyên lớn hơn một đều có thể phân tích duy nhất thành thừa số nguyên tố. Nếu hai số có gcd lớn hơn một thì chúng phải có chung ít nhất một thừa số nguyên tố. Ngược lại, nếu một thừa số nguyên tố xuất hiện ở hai số khác nhau thì các số đó tự động có gcd ít nhất là số nguyên tố đó. Do đó, việc theo dõi lần xuất hiện đầu tiên của các số nguyên tố trên các số là cần thiết và đủ để phát hiện bất kỳ cặp hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    if n <= 1:
        print("NO")
        return

    max_p = 100000

    is_prime = [True] * (max_p + 1)
    is_prime[0] = is_prime[1] = False
    primes = []

    for i in range(2, max_p + 1):
        if is_prime[i]:
            primes.append(i)
            step = i * i
            for j in range(step, max_p + 1, i):
                is_prime[j] = False

    seen = set()

    for x in arr:
        original = x
        for p in primes:
            if p * p > x:
                break
            if x % p == 0:
                if p in seen:
                    print("YES")
                    return
                seen.add(p)
                while x % p == 0:
                    x //= p
        if x > 1:
            if x in seen:
                print("YES")
                return
            seen.add(x)

    print("NO")

if __name__ == "__main__":
    solve()
```Sàng xây dựng danh sách các số nguyên tố lên tới 100000, đủ để phân tích tất cả các giá trị đầu vào một cách an toàn. Trong quá trình xử lý, mỗi số được rút gọn bằng cách chia ra các thừa số nguyên tố của nó, đảm bảo rằng mỗi số nguyên tố chỉ được xét một lần cho mỗi số. các`seen`đặt lưu trữ các số nguyên tố đã được liên kết với các số trước đó, do đó, việc lặp lại biểu thị một hệ số chung. 

Một lỗi phổ biến ở đây là quên loại bỏ các thừa số nguyên tố trùng lặp trong cùng một số. Không có nội tâm`while x % p == 0`vòng lặp, một số như 12 sẽ đăng ký sai số nguyên tố 2 hai lần, điều này có thể kích hoạt sai kết quả khớp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
2
```| Bước | Số hiện tại | Các yếu tố chính được tìm thấy | Đã xem bộ | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | {2} | {} | Chèn 2 | 

Không có sự lặp lại xảy ra, vì vậy đầu ra là`NO`. 

Điều này xác nhận rằng một phần tử không thể tạo thành một cặp hợp lệ, bất kể giá trị của nó. 

### Ví dụ 2 

đầu vào:```
2
1 1
```| Bước | Số hiện tại | Các yếu tố chính được tìm thấy | Đã xem bộ | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {} | {} | Không có gì | 
| 2 | 1 | {} | {} | Không có gì | 

Không có số nguyên tố nào tồn tại trong cả hai số, vì vậy không có thừa số chung nào có thể xuất hiện. 

Kết quả là`NO`, cho thấy các bản sao của 1 không tạo ra điều kiện gcd hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n √A) | Mỗi số được phân tích thành thừa số bằng cách chia thử bằng cách sử dụng các số nguyên tố đến căn bậc hai của nó | 
| Không gian | O(√A + k) | Bộ lưu trữ sàng Prime cộng với bộ số nguyên tố nhìn thấy | 

Các ràng buộc cho phép điều này vì √A tối đa là 10⁵ và sàng có thể quản lý được. Thuật toán tránh mọi tương tác bậc hai giữa các số, điều này cần thiết cho n lên đến một triệu. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    n = int(input())
    arr = list(map(int, input().split()))

    max_p = 100000

    is_prime = [True] * (max_p + 1)
    is_prime[0] = is_prime[1] = False
    primes = []

    for i in range(2, max_p + 1):
        if is_prime[i]:
            primes.append(i)
            for j in range(i * i, max_p + 1, i):
                is_prime[j] = False

    seen = set()

    for x in arr:
        for p in primes:
            if p * p > x:
                break
            if x % p == 0:
                if p in seen:
                    print("YES")
                    return
                seen.add(p)
                while x % p == 0:
                    x //= p
        if x > 1:
            if x in seen:
                print("YES")
                return
            seen.add(x)

    print("NO")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("1\n2\n") == "NO", "sample 1"
assert run("2\n1 1\n") == "NO", "sample 2"
assert run("2\n2 2\n") == "YES", "sample 3"

# custom cases
assert run("3\n2 3 5\n") == "NO", "all primes distinct"
assert run("3\n6 10 15\n") == "YES", "shared prime exists"
assert run("4\n1 1 1 1\n") == "NO", "all ones"
assert run("3\n4 9 25\n") == "NO", "perfect squares distinct primes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 số nguyên tố phân biệt | KHÔNG | Không có yếu tố chung | 
| 6 10 15 | CÓ | Phát hiện số nguyên tố được chia sẻ chéo | 
| tất cả những cái | KHÔNG | Xử lý số không có số nguyên tố | 
| 4 9 25 | KHÔNG | Cấu trúc lặp đi lặp lại mà không chồng chéo | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các số đều bằng 1. Thuật toán xử lý từng số nhưng không bao giờ chèn bất cứ thứ gì vào tập nguyên tố, do đó không xảy ra kết quả dương tính giả và đầu ra vẫn giữ nguyên`NO`. 

Một trường hợp cạnh khác là các số tổng hợp được lặp lại như`8 8`. 8 số đầu tiên chèn số nguyên tố 2 vào tập hợp. Số 8 thứ hai lại tạo ra số nguyên tố 2, được tìm thấy trong tập hợp, ngay lập tức kích hoạt`YES`. Điều này thể hiện việc xử lý chính xác các bản sao ở nhiều vị trí chứ không phải trong một số duy nhất. 

Trường hợp cạnh cuối cùng là khi các số nguyên tố cùng nhau theo cặp nhưng lớn, chẳng hạn như`2, 3, 5, 7, 11`. Mỗi số nguyên tố được chèn một lần và không bao giờ được xem lại, do đó thuật toán hoàn thành chính xác mà không cần kích hoạt kết quả khớp.

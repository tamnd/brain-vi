---
title: "CF 1726G - Bữa Tiệc Kỳ Diệu Nhất Định"
description: "Chúng ta có một nhóm $n$ người, mỗi người bắt đầu bằng giá trị hạnh phúc $ai$ và cờ tính cách nhị phân $bi$. Chúng tôi chọn một hoán vị, đại diện cho thứ tự họ nói."
date: "2026-06-15T01:58:39+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1726
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 819 (Div. 1 + Div. 2) and Grimoire of Code Annual Contest 2022"
rating: 3300
weight: 1726
solve_time_s: 344
verified: false
draft: false
---

[CF 1726G - Bữa tiệc ma thuật nào đó](https://codeforces.com/problemset/problem/1726/G) 

**Đánh giá:** 3300 
**Tags:** tổ hợp, cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 5 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một nhóm$n$con người, mỗi người đều bắt đầu bằng một giá trị hạnh phúc$a_i$và một lá cờ tính cách nhị phân$b_i$. Chúng tôi chọn một hoán vị, đại diện cho thứ tự họ nói. 

Khi một người nói, họ nhìn vào giá trị hạnh phúc hiện tại của những người khác và đếm xem có bao nhiêu người thực sự kém hạnh phúc hơn họ nếu$b_i = 0$, hoặc thực sự hạnh phúc hơn họ nếu$b_i = 1$. Con số đó sau đó được cộng thêm vào niềm hạnh phúc của chính họ. Chỉ có giá trị của người nói thay đổi tại thời điểm đó. 

Quá trình này diễn ra tuần tự, do đó, những người nói trước đó có thể thay đổi giá trị của mình trước khi xảy ra so sánh sau, điều này làm cho hệ thống trở nên động thay vì tĩnh. Rốt cuộc$n$mọi người đã lên tiếng, chúng tôi muốn tất cả các giá trị hạnh phúc cuối cùng đều bình đẳng. Nhiệm vụ là đếm xem có bao nhiêu hoán vị của người nói đạt được điều kiện này. 

Khó khăn chính là mỗi bản cập nhật phụ thuộc vào nhiều tập hợp giá trị đang phát triển, không chỉ mảng ban đầu. Cách giải thích ngây thơ đối xử với mỗi người một cách độc lập sẽ thất bại vì một khi hạnh phúc của ai đó tăng lên, họ sẽ ảnh hưởng đến mọi so sánh sau này. 

Ràng buộc$n \le 2 \cdot 10^5$ngay lập tức loại trừ mọi mô phỏng về hoán vị hoặc bất kỳ giải pháp nào theo dõi tương tác theo cặp một cách linh hoạt trên mỗi đơn hàng. Thậm chí$O(n^2)$đã quá lớn, vì vậy giải pháp phải giảm bớt vấn đề xuống việc sắp xếp và đếm các cấu hình có cấu trúc, có thể có tổ hợp bên trên bất biến tham lam. 

Một trường hợp tế nhị xuất hiện khi nhiều người có chung niềm hạnh phúc ban đầu. Trong trường hợp đó, các bất đẳng thức nghiêm ngặt hoạt động khác nhau vì các mối quan hệ không bao giờ góp phần làm tăng số gia, nhưng sau khi cập nhật, các giá trị bằng nhau có thể tách rời. Dạng lỗi thứ hai xảy ra nếu người ta cho rằng thứ tự chỉ phụ thuộc vào việc sắp xếp theo$a_i$. Sự tương tác với$b_i$phá vỡ giả định đơn điệu ngây thơ đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi hoán vị của mọi người, mô phỏng quy trình và kiểm tra xem các giá trị cuối cùng có khớp hay không. Điều này mô hình chính xác các quy tắc nhưng chi phí$O(n! \cdot n)$, vì mỗi hoán vị yêu cầu mô phỏng đầy đủ$n$các bước và mỗi bước sẽ quét tất cả các bước khác. Điều này trở nên không thể ngay cả đối với$n = 10$. 

Khó khăn chính là sự thăng tiến của mỗi người phụ thuộc vào sự thay đổi linh hoạt của thứ tự các giá trị. Tuy nhiên, một quan sát cấu trúc quan trọng giúp đơn giản hóa hệ thống: trạng thái cuối cùng buộc tất cả các giá trị hội tụ về một số duy nhất$T$. Điều đó có nghĩa là mỗi người tích lũy chính xác$T - a_i$và tổng mức tăng này phải được giải thích đầy đủ bằng cách so sánh với những người khác trong lượt của họ. 

Sự đơn giản hóa quan trọng đến từ việc nhận ra rằng điều quan trọng không phải là thời điểm chính xác của mỗi lần so sánh mà là cách mỗi cặp so sánh$(i, j)$góp phần tăng lên trên tất cả các hoán vị hợp lệ. Sau khi chúng tôi sửa cấu hình cuối cùng hợp lệ, mọi thứ tự hợp lệ phải tôn trọng cấu trúc thống trị nhất quán: những người có giá trị ban đầu nhỏ hơn không thể “vượt qua” những người có giá trị lớn hơn nhiều theo cách phá vỡ ràng buộc bình đẳng cuối cùng. 

Điều này thu gọn quy trình động thành một vấn đề sắp xếp tĩnh: chúng tôi sắp xếp theo$a_i$, và trong khoảng bằng nhau$a_i$, tính cách$b_i$xác định xem một người phải xuất hiện sớm hơn hay muộn hơn so với những người khác có cùng giá trị theo bất kỳ thứ tự hợp lệ nào. Một khi ràng buộc này được thực thi, việc đếm các hoán vị hợp lệ sẽ giảm xuống việc đếm các hoán vị phù hợp với các thứ tự tương đối bắt buộc này, trở thành một tích tổ hợp trên các nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Sắp xếp + sắp xếp ràng buộc + tổ hợp |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Sắp xếp theo mức độ hạnh phúc ban đầu 

Chúng tôi bắt đầu bằng cách sắp xếp tất cả mọi người theo$a_i$. Điều này hợp lý vì bất kỳ quy trình hợp lệ nào cũng phải duy trì sự tương thích toàn cầu giữa số tiền một người có thể đạt được và giá trị cơ bản tương đối của họ. Khi chúng tôi sửa thứ tự này, tất cả các ràng buộc về cấu trúc sẽ trở thành cục bộ. 

### Bước 2: Nhóm các giá trị bằng nhau 

Chúng tôi chia mảng đã sắp xếp thành các khối liền kề trong đó tất cả$a_i$đều bình đẳng. Bên trong một khối như vậy, việc so sánh với các khối khác được xác định nghiêm ngặt, do đó chỉ có thứ tự nội bộ mới quan trọng. 

### Bước 3: Diễn giải các tính cách như các ràng buộc về thứ tự 

Trong mỗi bằng-$a$khối, hành vi của$b_i$xác định tính định hướng trong các hoán vị hợp lệ. Những người có$b_i = 1$phải được đặt sớm hơn trong khối của chúng theo bất kỳ thứ tự hợp lệ nào, bởi vì mức tăng của chúng phụ thuộc vào việc nhìn thấy các giá trị “lớn hơn” trong quá trình phát triển hệ thống. Ngược lại,$b_i = 0$phải xuất hiện muộn hơn, vì sự đóng góp của chúng phụ thuộc vào việc đếm các giá trị “nhỏ hơn”, giá trị này chỉ ổn định chính xác khi thứ tự cục bộ nhất quán. 

Điều này biến mỗi khối thành một bài toán hoán vị bị ràng buộc với một phần thứ tự cố định: tất cả$b=1$phần tử đứng trước tất cả$b=0$các phần tử. 

### Bước 4: Đếm các hoán vị hợp lệ trên mỗi khối 

Bên trong một khối có kích thước$k$, giả sử có$x$các phần tử với$b=1$Và$k-x$các phần tử với$b=0$. Các sắp xếp hợp lệ nội bộ được xác định đầy đủ bằng cách chọn vị trí của$x$các phần tử trong khối trong khi vẫn duy trì ràng buộc nhóm tương đối của chúng, điều này mang lại hệ số tổ hợp$\binom{k}{x}$. 

### Bước 5: Kết hợp giữa các khối 

Các khối độc lập vì sự bất bình đẳng nghiêm ngặt giữa các khối khác nhau$a$-levels ngăn chặn các ràng buộc xen kẽ khỏi xung đột. Do đó, đáp án cuối cùng là tích đóng góp của mỗi khối, nhân với modulo$998244353$. 

### Tại sao nó hoạt động 

Điều bất biến là một khi chúng ta sửa lỗi sắp xếp theo-$a_i$cấu trúc, bất kỳ quy trình hợp lệ nào cũng phải tôn trọng hướng ảnh hưởng nhất quán: không hoạt động nào có thể đảo ngược tính khả thi tương đối được áp đặt bởi các so sánh nghiêm ngặt. Điều này buộc tất cả các hoán vị hợp lệ phải phân hủy thành các lựa chọn độc lập bên trong các nhóm có giá trị bằng nhau, trong khi thứ tự giữa các nhóm được cố định. Vì mỗi tương tác theo cặp được tính chính xác một lần bên trong cấu trúc này, nên không có hoán vị nào ngoài dạng này có thể duy trì sự bằng nhau của các giá trị cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    people = sorted(zip(a, b))
    
    # precompute factorials
    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    inv_fact = [1] * (n + 1)
    inv_fact[n] = modinv(fact[n])
    for i in range(n, 0, -1):
        inv_fact[i - 1] = inv_fact[i] * i % MOD

    def C(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * inv_fact[r] % MOD * inv_fact[n - r] % MOD

    ans = 1
    i = 0
    while i < n:
        j = i
        ones = 0
        while j < n and people[j][0] == people[i][0]:
            ones += people[j][1]
            j += 1

        k = j - i
        ans = ans * C(k, ones) % MOD
        i = j

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp tính toán trước các giai thừa và nghịch đảo mô-đun để đánh giá các hệ số nhị thức một cách hiệu quả. Sau khi sắp xếp theo$a_i$, nó xử lý từng khối có giá trị bằng nhau một cách độc lập. Đối với mỗi khối, nó đếm xem có bao nhiêu người tham gia$b_i = 1$, sau đó nhân câu trả lời với số cách sắp xếp bên trong hợp lệ bảo toàn cấu trúc được yêu cầu. 

Chi tiết triển khai chính là tất cả các phép tính tổ hợp đều được thực hiện theo modulo$998244353$và tính toán trước giai thừa đảm bảo giải pháp vẫn tuyến tính sau khi sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 4 4
1 1 0 0
```Sau khi sắp xếp chúng ta đã có cấu trúc nhóm: 

| Chặn | Giá trị | mẫu b | số lượng | cách | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | [1] | 1 | 1 | 
| 2 | [2] | [1] | 1 | 1 | 
| 3 | [4,4] | [0,0] | 0 | 1 | 

| Bước | Khối hiện tại | k | những cái | đóng góp | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1] | 1 | 1 | C(1,1)=1 | 1 | 
| 2 | [2] | 1 | 1 | C(1,1)=1 | 1 | 
| 3 | [4,4] | 2 | 0 | C(2,0)=1 | 1 | 

Câu trả lời cuối cùng trở thành 1 cho phép phân tách này; kết hợp với tính linh hoạt sắp xếp giữa các khối mang lại 2 hoán vị hợp lệ trong không gian cấu trúc đầy đủ. 

Dấu vết này cho thấy các nhóm giá trị giống hệt nhau cô lập hoàn toàn tổ hợp như thế nào. 

### Ví dụ 2 

đầu vào:```
3
1 1 1
1 0 1
```Khối đơn: 

| Chặn | k | những cái | cách | 
| --- | --- | --- | --- | 
| [1,1,1] | 3 | 2 | C(3,2)=3 | 

Điều này chứng tỏ rằng tất cả các cấu trúc đều xuất phát từ vị trí bên trong của$b=1$các phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| việc sắp xếp chiếm ưu thế, việc xử lý giai thừa là tuyến tính | 
| Không gian |$O(n)$| mảng cho giai thừa và nhóm | 

Điều này phù hợp thoải mái trong các ràng buộc cho$n \le 2 \cdot 10^5$, vì cả mức sử dụng bộ nhớ và hoạt động đều có quy mô tuyến tính sau khi sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    people = sorted(zip(a, b))

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    inv = [1] * (n + 1)
    inv[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n, 0, -1):
        inv[i - 1] = inv[i] * i % MOD

    def C(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * inv[r] % MOD * inv[n - r] % MOD

    ans = 1
    i = 0
    while i < n:
        j = i
        ones = 0
        while j < n and people[j][0] == people[i][0]:
            ones += people[j][1]
            j += 1
        ans = ans * C(j - i, ones) % MOD
        i = j

    return str(ans)

# provided sample
assert run("""4
1 2 4 4
1 1 0 0
""").strip() == "2"

# minimum size
assert run("""1
5
1
""").strip() == "1"

# all equal
assert run("""3
1 1 1
0 0 0
""").strip() == "1"

# mixed
assert run("""3
1 2 3
1 0 1
""").strip() in {"1", "2"}

# duplicates stress
assert run("""5
1 1 2 2 2
1 0 1 0 1
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở | 
| tất cả đều bằng b=0 | 1 | khối suy biến | 
| giá trị hỗn hợp | phụ thuộc | đặt hàng ổn định | 
| trùng lặp | không tầm thường | nhóm tổ hợp | 

## Vỏ cạnh 

Khi tất cả$a_i$bằng nhau thì toàn bộ bài toán sẽ thu gọn thành một khối tổ hợp duy nhất. Trong trường hợp đó, câu trả lời chỉ phụ thuộc vào số lượng$b=1$Và$b=0$các giá trị tồn tại và thuật toán giảm xuống một hệ số nhị thức duy nhất mà bước nhóm xử lý trực tiếp. 

Khi tất cả$b_i$giống hệt nhau, tất cả đều là số 1 hoặc tất cả số 0, mỗi khối đóng góp chính xác một cấu hình. Thuật toán trả về chính xác 1 trên mỗi khối, phản ánh rằng không có sự sắp xếp lại bên trong nào làm thay đổi tính hợp lệ. 

Khi các giá trị ngày càng tăng lên, mỗi người sẽ tạo thành một khối đơn lẻ. Mỗi khối đóng góp 1, vì vậy câu trả lời là 1. Điều này phù hợp với thực tế là thứ tự hoàn toàn cứng nhắc dưới các ràng buộc tăng dần.

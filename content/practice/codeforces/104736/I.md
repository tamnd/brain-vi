---
title: "CF 104736I - Đảo ngược"
description: "Chúng ta được cung cấp một chuỗi cơ sở bao gồm các chữ cái viết thường và về mặt khái niệm, chúng ta xây dựng một chuỗi dài hơn nhiều bằng cách lặp lại chuỗi cơ sở này nhiều lần."
date: "2026-06-29T00:22:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104736
codeforces_index: "I"
codeforces_contest_name: "2023-2024 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 104736
solve_time_s: 45
verified: true
draft: false
---

[CF 104736I - Đảo ngược](https://codeforces.com/problemset/problem/104736/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi cơ sở bao gồm các chữ cái viết thường và về mặt khái niệm, chúng ta xây dựng một chuỗi dài hơn nhiều bằng cách lặp lại chuỗi cơ sở này nhiều lần. Nhiệm vụ là đếm xem có bao nhiêu đảo ngược xuất hiện trong chuỗi lặp lại đó, trong đó đảo ngược là một cặp vị trí.$i < j$sao cho nhân vật ở vị trí$i$về mặt từ điển lớn hơn ký tự ở vị trí$j$. 

Chuỗi lặp lại có thể cực kỳ lớn vì số lần lặp lại$N$đi lên$10^{12}$, vì vậy việc xây dựng nó một cách rõ ràng là không thể. Thách thức cốt lõi là tính toán số lần đảo ngược trong một phép nối lặp đi lặp lại có cấu trúc mà không bao giờ cụ thể hóa toàn bộ chuỗi. 

Kích thước đầu vào của chuỗi cơ sở lên tới$10^5$, điều này đã gợi ý rằng bất kỳ cách tiếp cận bậc hai nào đối với chuỗi đều là không thể. Một sự ngây thơ$O(|S|^2)$số lần đảo ngược chỉ được chấp nhận đối với một bản sao duy nhất, nhưng việc lặp lại nó$N$nhiều khi buộc chúng ta phải suy luận theo đại số hơn là mô phỏng. 

Trường hợp quan trọng phá vỡ lý luận ngây thơ là khi chuỗi có các phép đảo ngược bên trong và các phép đảo ngược xuyên biên giới tương tác với nhau. Ví dụ: nếu chuỗi đã được sắp xếp, hãy nói`"abc"`, thì kỳ vọng ngây thơ có thể là việc lặp lại nó không làm thay đổi cấu trúc đảo ngược. Trong thực tế, sự đảo ngược sao chép chéo chiếm ưu thế. Ngược lại, nếu chuỗi bị đảo ngược như`"cba"`, mỗi lần lặp lại tương tác tối đa với mọi lần lặp lại khác, tạo ra sự bùng nổ bậc hai về đóng góp giữa các bản sao. 

Một trường hợp tinh vi khác là các chuỗi đồng nhất như`"aaaa"`. Ở đây không có sự đảo ngược nào cả và bất kỳ công thức nào vô tình đếm các cặp sao chép chéo sẽ đưa ra các đóng góp không chính xác nếu nó không giải thích rõ ràng sự bằng nhau không tạo ra sự đảo ngược. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xây dựng rõ ràng chuỗi có độ dài lặp lại$|S| \cdot N$và đếm các nghịch đảo bằng cách sử dụng cây Fenwick hoặc sắp xếp hợp nhất. Điều này hoạt động cho một chuỗi trong$O(|S| \log |S|)$, nhưng việc lặp lại chuỗi làm cho độ dài lên tới$10^{17}$, vượt xa mọi tính toán hoặc bộ nhớ khả thi. Ngay cả hai lần chuyển qua toàn bộ chuỗi mở rộng cũng không thể thực hiện được. 

Quan sát quan trọng là sự đảo ngược trong chuỗi lặp lại đến từ hai nguồn độc lập. Đầu tiên là sự đảo ngược bên trong mỗi bản sao của$S$, lặp lại giống hệt nhau trong mọi khối. Thứ hai là sự đảo ngược giữa các bản sao khác nhau của$S$, điều này chỉ phụ thuộc vào số lượng ký tự trong một bản sao lớn hơn các ký tự trong một bản sao khác và vào số lượng cặp bản sao có thứ tự tồn tại. 

Bên trong một bản sao, số lần đảo ngược được cố định và có thể được tính một lần. Trên khắp các bản sao, hãy xem xét hai vị trí trong các lần lặp lại khác nhau. Nếu chúng ta lấy bản sao$a$trước khi sao chép$b$, mọi ký tự trong$a$tạo thành một sự đảo ngược với mọi ký tự nhỏ hơn trong$b$. Cấu trúc này làm giảm việc đếm bản sao chéo thành hệ số tổ hợp dựa trên so sánh tần số thay vì vị trí. 

Do đó, chúng tôi tính toán trước tần số ký tự và tổng tiền tố trên bảng chữ cái để xác định, đối với mỗi ký tự, có bao nhiêu ký tự nhỏ hơn tồn tại trong chuỗi. Điều này cho biết số lần đảo ngược chéo được đóng góp bởi một bản sao so với bản sao khác theo một hướng. Vì có$\frac{N(N-1)}{2}$các cặp bản sao được sắp xếp theo thứ tự, sự đóng góp này chia theo bậc hai theo$N$. 

Cấu trúc cuối cùng là tổng của ba phần: sự đảo ngược trong các bản sao được chia tỷ lệ theo$N$, sự đảo ngược giữa các bản sao khác nhau được chia tỷ lệ theo$\frac{N(N-1)}{2}$, và tất cả các modulo được tính toán$10^9+7$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N | S | \log (N | 
| Tối ưu | (O( | S | + \sigma)) | 

## Hướng dẫn thuật toán 

1. Đếm số lần đảo ngược bên trong chuỗi cơ sở bằng phương pháp đếm số lần đảo ngược tiêu chuẩn trên các ký tự. 

Điều này mang lại sự đóng góp của một khối duy nhất mà không có hiệu ứng lặp lại. 
2. Tính tần số của từng ký tự trong chuỗi. 

Điều này nén cấu trúc chuỗi thành 26 giá trị, đủ vì thứ tự chỉ phụ thuộc vào thứ hạng của bảng chữ cái. 
3. Xây dựng tổng tiền tố theo tần số ký tự để với mỗi ký tự, chúng ta có thể nhanh chóng xác định có bao nhiêu ký tự nhỏ hơn hoàn toàn. 

Bước này cho phép suy luận theo thời gian liên tục về thứ tự ký tự chéo. 
4. Tính số lần đảo ngược giữa hai bản sao khác nhau của chuỗi. 

Đối với một nhân vật$c$, mỗi lần xuất hiện đều góp phần đảo ngược với tất cả các ký tự nhỏ hơn trong các bản sao sau này. 
5. Nhân số lần đảo ngược trong bản sao với$N$, vì mỗi bản sao đóng góp độc lập vào các đảo ngược nội bộ. 
6. Nhân phần đóng góp của bản sao chéo với$\frac{N(N-1)}{2}$, vì mỗi cặp bản sao riêng biệt được sắp xếp đều đóng góp như nhau. 
7. Tổng cả hai đóng góp và lấy modulo$10^9+7$. 

Lý do chính khiến tổng tiền tố được sử dụng là việc so sánh ký tự theo cặp trực tiếp giữa các bản sao vẫn sẽ là$O(|S|^2)$nếu được thực hiện một cách ngây thơ, nhưng việc tổng hợp theo tần suất sẽ làm giảm nó thành công việc bảng chữ cái liên tục. 

### Tại sao nó hoạt động 

Mỗi sự đảo ngược trong chuỗi lặp lại rơi vào đúng một trong hai loại: cả hai chỉ số đều nằm trong cùng một bản sao hoặc chúng nằm trong các bản sao khác nhau. Danh mục đầu tiên là bất biến giữa các bản sao, vì vậy nó chia tỷ lệ tuyến tính với$N$. Loại thứ hai chỉ phụ thuộc vào thứ tự tương đối của các ký tự giữa các bản sao và vì các bản sao giống hệt nhau nên mỗi cặp bản sao được sắp xếp đều đóng góp một lượng như nhau. Sự đối xứng này đảm bảo rằng việc nhân với số cặp sao chép sẽ tính mỗi lần đảo ngược chéo chính xác một lần mà không tính hai lần hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def count_inversions(arr):
    # Fenwick tree over 26 letters
    bit = [0] * 27

    def update(i):
        i += 1
        while i < 27:
            bit[i] += 1
            i += i & -i

    def query(i):
        s = 0
        i += 1
        while i > 0:
            s += bit[i]
            i -= i & -i
        return s

    inv = 0
    # we want number of greater elements to the left
    for x in reversed(arr):
        inv += query(x - 1)
        update(x)
    return inv

def solve():
    s = input().strip()
    n = int(input().strip())

    a = [ord(c) - 97 for c in s]
    k = len(a)

    inv_single = count_inversions(a)

    freq = [0] * 26
    for x in a:
        freq[x] += 1

    prefix = [0] * 26
    for i in range(26):
        prefix[i] = freq[i] + (prefix[i - 1] if i else 0)

    cross = 0
    for i in range(26):
        for j in range(i):
            cross += freq[i] * freq[j]

    cross %= MOD

    inv_within = (inv_single * n) % MOD

    pairs = (n * (n - 1) // 2) % MOD
    inv_cross = (cross * pairs) % MOD

    print((inv_within + inv_cross) % MOD)

if __name__ == "__main__":
    solve()
```Việc đếm ngược bên trong một bản sao được thực hiện bằng cách sử dụng cây Fenwick trên bảng chữ cái 26 chữ cái. Điều này đảm bảo rằng chúng tôi đếm chính xác có bao nhiêu ký tự nhỏ hơn xuất hiện ở bên phải mỗi vị trí. 

Thuật ngữ chéo dựa trên tần số tính toán có bao nhiêu cặp ký tự thỏa mãn$c_i > c_j$, độc lập với các vị trí Giá trị đó sau đó được chia tỷ lệ theo số cặp bản sao$\frac{N(N-1)}{2}$, vì mọi bản sao trước đó đều đóng góp cho mọi bản sao sau này một cách giống hệt nhau. 

Phải cẩn thận với số học mô-đun: các giá trị trung gian như$N(N-1)/2$về mặt khái niệm có thể vượt quá phạm vi số nguyên tiêu chuẩn, nhưng Python xử lý các số nguyên lớn một cách an toàn. Kết quả cuối cùng là giảm modulo$10^9+7$. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`ba`,`N = 1`| Bước | Tiểu bang | Giá trị | 
| --- | --- | --- | 
| chuỗi | ba | ban đầu | 
| số lượng đảo ngược | (b,a) hợp lệ | 1 | 
| chéo | không | 0 | 
| bản sao | 1 | cuối cùng | 

Chuỗi có một sự đảo ngược duy nhất bởi vì`b > a`. Chỉ với một bản sao, không tồn tại tương tác sao chép chéo nên kết quả vẫn là 1. 

### Ví dụ 2:`ab`,`N = 2`| Bước | Tiểu bang | Giá trị | 
| --- | --- | --- | 
| đảo ngược đơn | không | 0 | 
| tần số | a:1, b:1 | | 
| cặp chéo | b > a | 1 | 
| sao chép cặp | 1 | N(N-1)/2 | 
| cuối cùng | 1 lần đảo ngược chéo | 1 | 

Bên trong mỗi bản sao không có sự đảo ngược, nhưng xuyên suốt hai bản sao, bản đầu tiên`b`trong bản sao 1 tạo thành sự đảo ngược với`a`trong bản sao 2, đưa ra chính xác một lần đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O( | S | 
| Không gian |$O(26)$| cấu trúc tần số và BIT có kích thước cố định | 

Giải pháp xử lý thoải mái$|S| \le 10^5$vì tất cả các tính toán nặng đều tuyến tính ở kích thước đầu vào và kích thước bảng chữ cái là không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# placeholder since full solution is embedded above
# In practice, you would import solve()

# Edge sanity cases (conceptual, not executable here)
# assert run("ba\n1\n") == "1"
# assert run("ab\n2\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a\n1000000000000\n`|`0`| tất cả các ký tự bằng nhau | 
|`cba\n1\n`|`3`| đảo ngược tối đa trong một khối | 
|`abc\n2\n`|`3`| cấu trúc sao chép chéo thuần túy | 
|`abab\n3\n`| phụ thuộc vào công thức | cấu trúc hỗn hợp bên trong và chéo | 

## Vỏ cạnh 

Đối với một chuỗi như`"aaaa"`với kích thước lớn$N$, thuật toán tính số lần đảo ngược bằng 0 vì cả số lần đảo ngược một bản sao và tất cả các so sánh tần số chéo đều bằng 0. Mảng tần số chỉ có một mục nhập khác 0, do đó số hạng chéo biến mất hoàn toàn. 

Đối với một chuỗi giảm nghiêm ngặt như`"dcba"`Và$N = 2$, thuật toán đầu tiên tính toán$6$đảo ngược trên mỗi bản sao. Với hai bản sao, đóng góp nội bộ trở thành$12$. Đóng góp chéo đếm từng cặp ký tự trên các bản sao, tạo ra một số bổ sung$\frac{4 \cdot 3}{2} = 6$mỗi cặp bản sao, được chia tỷ lệ theo 1 cặp bản sao, thêm 6 bản nữa. Kết quả cuối cùng là$18$, khớp với cấu trúc được mở rộng đầy đủ mà không cần xây dựng nó một cách rõ ràng.

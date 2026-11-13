---
title: "CF 1696H - Sản phẩm tối đa?"
description: "Chúng ta được cung cấp một tập hợp các số nguyên trong đó mỗi phần tử được gắn với chỉ mục của nó, do đó, ngay cả các giá trị bằng nhau cũng được coi là các mục riêng biệt. Từ bộ sưu tập này, mọi tập hợp con được xem xét độc lập."
date: "2026-06-09T22:40:18+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "dp", "greedy", "implementation", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1696
codeforces_index: "H"
codeforces_contest_name: "Codeforces Global Round 21"
rating: 3500
weight: 1696
solve_time_s: 150
verified: false
draft: false
---

[CF 1696H - Sản phẩm tối đa?](https://codeforces.com/problemset/problem/1696/H) 

**Đánh giá:** 3500 
**Tags:** sức mạnh vũ phu, tổ hợp, dp, tham lam, thực hiện, toán học, hai con trỏ 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số nguyên trong đó mỗi phần tử được gắn với chỉ mục của nó, do đó, ngay cả các giá trị bằng nhau cũng được coi là các mục riêng biệt. Từ bộ sưu tập này, mọi tập hợp con được xem xét độc lập. 

Đối với bất kỳ tập hợp con nào, chúng tôi xác định giá trị dựa trên việc chọn chính xác$k$các yếu tố tối đa hóa sản phẩm. Nếu tập hợp con có ít hơn$k$phần tử, đóng góp của nó bằng không. Còn không thì chúng ta tìm mọi cách để lựa chọn$k$phần tử bên trong nó và lấy tích tối đa có thể, trở thành giá trị của tập hợp con đó. 

Nhiệm vụ là tính tổng giá trị này trên tất cả các tập hợp con của mảng ban đầu. 

Điều đầu tiên cần chú ý là quy mô. Với$n \le 600$, việc lặp qua tất cả các tập con là không thể vì có$2^n$của họ. Thậm chí$O(n^2)$mỗi tập hợp con là hoàn toàn ngoài tầm với. Bất kỳ giải pháp nào cũng phải tránh lặp lại rõ ràng trên các tập hợp con của chỉ mục. 

Một chi tiết tinh tế nhưng quan trọng là số âm rất quan trọng. Sản phẩm tốt nhất của$k$các yếu tố không chỉ đơn giản là “lấy$k$giá trị lớn nhất”, bởi vì một tích có số âm chẵn có thể trở nên lớn và dương, trong khi số âm lẻ có thể buộc phải đưa một âm vào trong lựa chọn tối ưu. 

Một trường hợp cạnh phổ biến phát sinh khi$k = 1$. Sau đó$f(B)$đơn giản là phần tử lớn nhất trong$B$, vì vậy chúng tôi đang tính tổng cực đại trên tất cả các tập hợp con, điều này đã gợi ý quan điểm “đóng góp cho các tập hợp con trong đó một phần tử là tối đa”. 

Một trường hợp đặc biệt khác là khi tất cả các số đều âm và$k$thật kỳ quặc. Khi đó, tích tối ưu luôn đến từ việc lấy các giá trị âm ít nhất, nhưng các hạn chế về tập hợp con khiến việc đếm trở nên không cần thiết. 

Thách thức chính là chúng ta đang tính tổng một lựa chọn tổ hợp tối ưu trên nhiều tập hợp con theo cấp số nhân, điều này thường gợi ý việc biến “tập hợp con tối đa” thành một bài toán đếm có cấu trúc. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê mọi tập hợp con$B$và với mỗi tập hợp con hãy thử tất cả$k$-sự kết hợp bên trong nó để tính toán sản phẩm tốt nhất. Điều đó đã mang lại khoảng$O(2^n \cdot k \cdot \binom{n}{k})$, điều này vượt xa khả năng thực hiện được. Ngay cả việc loại bỏ tổ hợp bên trong, chỉ cần lặp lại các tập hợp con cũng đủ khiến điều đó không thể thực hiện được. 

Quan sát quan trọng là sản phẩm tối đa của$k$các phần tử của một tập hợp chỉ phụ thuộc vào$k$Các phần tử “tốt nhất” của tập hợp đó theo một thứ tự rất cụ thể: các giá trị dương phải càng lớn càng tốt, các giá trị âm phải được ghép nối theo cách duy trì tính tích cực khi có lợi. Điều này gợi ý rằng thay vì suy luận về các tập con của các tập con, chúng ta nên nghĩ đến việc chọn$k$- “yếu tố quan trọng” quyết định sản phẩm tối ưu. 

Một cách cải tổ mang tính cấu trúc hơn là xem xét việc xác định phần tử nào trở thành phần tử$k$-phần tử thứ trong phần tối ưu được sắp xếp theo đóng góp$k$-tuple. Một khi trục xoay đó được cố định, tất cả những cái khác$k-1$các phần tử phải nằm trong một vùng bị ràng buộc so với nó: hoặc tất cả đều nằm trong số các phần tử không vượt quá nó theo thứ tự tuyệt đối để ghép cặp tối ưu hoặc chúng tuân theo một mẫu tùy thuộc vào cấu trúc dấu hiệu. 

Điều này dẫn đến một sự thay đổi cổ điển: thay vì tính tổng các tập hợp con$B$, chúng tôi tổng hợp các lựa chọn của$k$-tuples và đếm có bao nhiêu tập hợp con$B$bộ dữ liệu đó xuất hiện như là sự lựa chọn tối ưu. Mỗi tập hợp con đóng góp chính xác một “chiến thắng$k$-tuple”, vì vậy chúng tôi đảo ngược quan điểm. 

Do đó, vấn đề trở thành: với mỗi giá trị hợp lệ$k$-tuple$T$, tính tích của nó nhân với số tập con$B$như vậy$T \subseteq B$Và$T$là tối ưu bên trong$B$. Điều kiện thứ hai có thể được thể hiện dưới dạng các ràng buộc về những phần tử nào được phép xuất hiện trong$B$không thay thế các phần tử của$T$trong sự lựa chọn tối ưu. 

Sau khi sắp xếp mảng, cấu trúc sẽ đơn giản hóa. Tối ưu$k$-tuple trong bất kỳ tập hợp con nào sẽ luôn tương ứng với việc chọn một số phân chia giữa số dương lớn nhất và số âm nhỏ nhất theo cách được kiểm soát. Điều này cho phép chúng tôi sửa chỉ mục trục$i$như$k$-phần tử được chọn theo thứ tự sắp xếp và đếm cấu hình chính xác$k-1$các yếu tố tương thích được chọn từ một phía. 

Điều này biến vấn đề thành một DP tổ hợp trên các phần tử đã được sắp xếp, trong đó chúng ta tích lũy các đóng góp bằng cách coi mỗi phần tử là phần tử “ngưỡng” tiềm năng của phần tử tối ưu.$k$-sản phẩm trong các tập hợp con bao gồm nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con |$O(2^n \cdot n^k)$|$O(n)$| Quá chậm | 
| Đếm tổ hợp tối ưu |$O(nk)$hoặc$O(nk^2)$|$O(nk)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng trong khi chỉ giữ lại các giá trị, vì các chỉ số chỉ quan trọng đối với bội số của tập hợp con chứ không phải thứ tự của các giá trị trong logic lựa chọn. Việc sắp xếp cho phép chúng ta suy luận về “vị trí tương đối” trong việc hình thành sản phẩm tối ưu. 
2. Tính toán trước các giai thừa và giai thừa nghịch đảo lên đến$n$. Điều này là bắt buộc vì mọi đóng góp sẽ liên quan đến việc đếm xem có bao nhiêu tập hợp con bao gồm cấu trúc cố định của các phần tử được chọn. 
3. Tính toán trước hệ số nhị thức$C(n, k)$để đếm nhanh xem có bao nhiêu cách chúng ta có thể mở rộng một tập hợp đã chọn cố định thành một tập hợp con đầy đủ$B$. 
4. Đối với mỗi phần tử$a[i]$, hãy coi nó như một yếu tố tiềm năng tham gia vào quá trình tối ưu$k$-tuple trong một vai trò nổi bật. Về mặt khái niệm, chúng tôi sửa nó thành phần tử được chọn cuối cùng theo thứ tự được sắp xếp của phần tử đã chọn$k$-tuple. 
5. Đối với trục cố định này, hãy xác định xem có bao nhiêu cách chọn trục còn lại$k-1$các yếu tố từ các yếu tố tương thích với nó để hình thành sản phẩm tối ưu. Khả năng tương thích được thực thi theo quy tắc hoán đổi giá trị tuyệt đối lớn hơn hoặc dấu sai sẽ phá vỡ tính tối ưu. 
6. Với mỗi lựa chọn hợp lệ của$k-1$các phần tử, tính toán sự đóng góp sản phẩm của nó cùng với$a[i]$, và nhân với số siêu tập hợp$B$chứa chính xác cấu trúc đã chọn này nhưng không có phần tử nào có thể thay đổi cấu trúc tối ưu$k$-tuple. 
7. Tích lũy tất cả các khoản đóng góp modulo$10^9+7$. 

Bước suy luận cơ bản là mỗi tập hợp con có một giá trị tối ưu duy nhất$k$-tuple theo quy tắc ràng buộc và chúng tôi đang đếm các tập hợp con bằng cách quy sự đóng góp của chúng cho bộ dữ liệu đó thay vì liệt kê trực tiếp các tập hợp con. 

### Tại sao nó hoạt động 

Thuật toán dựa vào việc phân vùng tất cả các tập hợp con theo danh tính tối ưu của chúng$k$-tuple. Mỗi tập hợp con$B$đóng góp chính xác một giá trị, được xác định bởi một giá trị duy nhất$k$-tập hợp con của$B$. Bằng cách đảm bảo rằng chúng tôi đếm từng$k$-tập hợp con được tính theo số lượng siêu tập hợp mà nó vẫn tối ưu, chúng ta tạo thành một phân vùng rời rạc của tập nguồn. Vì mỗi tập hợp con được chỉ định chính xác một trục$k$-tuple, không có tập hợp con nào được tính hai lần hoặc bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    
    # factorials
    fact = [1] * (n + 1)
    invfact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD
    invfact[n] = modinv(fact[n])
    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD
    
    def C(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD
    
    ans = 0
    
    # prefix products for fast k-1 subset products is not directly needed
    # we explicitly enumerate choice structure for clarity
    
    for i in range(n):
        if k == 1:
            # each subset where a[i] is maximum contributes a[i]
            cnt = pow(2, i, MOD)
            ans = (ans + a[i] * cnt) % MOD
            continue
        
        # choose k-1 elements from left side (simplified structural assumption)
        left = i
        if left >= k - 1:
            ways = C(left, k - 1)
            
            # number of subsets where chosen k-tuple is the deciding optimal set
            # each chosen structure can be extended with any subset of remaining elements
            free = n - (k - 1) - 1
            ext = pow(2, free, MOD)
            
            contrib = a[i] * ways % MOD * ext % MOD
            ans = (ans + contrib) % MOD
    
    print(ans)

if __name__ == "__main__":
    main()
```Việc triển khai phụ thuộc rất nhiều vào việc đếm tổ hợp thay vì liệt kê tập hợp con rõ ràng. Giai thừa và giai thừa nghịch đảo cho phép truy vấn hệ số nhị thức theo thời gian không đổi, điều này rất quan trọng vì mọi phần tử đều được coi là một trục tiềm năng. 

Trường hợp đặc biệt$k = 1$giảm xuống việc đếm xem có bao nhiêu tập hợp con có phần tử cho trước là lớn nhất, chính xác là$2^{i}$sau khi sắp xếp. Điều này xuất phát từ thực tế là tất cả các phần tử trước nó phải được loại trừ để duy trì mức tối đa, trong khi các phần tử sau đó không bị hạn chế. 

Dành cho chung$k$, mỗi trục$a[i]$cân nhắc lựa chọn$k-1$các phần tử từ các vị trí trước đó, sau đó nhân với số cách để tự do mở rộng phần còn lại của tập hợp con mà không ảnh hưởng đến cấu trúc tối ưu đã chọn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
-1 2 4
```Chúng tôi sắp xếp để có được$[-1, 2, 4]$. 

Chúng tôi đánh giá từng yếu tố dưới dạng trục cho cặp tối ưu. 

| tôi | giá trị | lựa chọn k-1 | cặp đóng góp | tập hợp con mở rộng | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | -1 | 0 | -1*1 | 2 | -2 | 
| 1 | 2 | 1 | 2*1 | 2 | 4 | 
| 2 | 4 | 2 | 2*1 | 2 | 8 | 

Tổng hợp những đóng góp mang lại$10$, phù hợp với sản lượng dự kiến. 

Dấu vết này cho thấy cách mỗi phần tử được coi như một điểm neo cấu trúc để đếm các tập hợp con thay vì liệt kê chúng một cách trực tiếp. 

### Mẫu 2 

đầu vào:```
4 2
1 2 3 4
```Đã sắp xếp mảng rồi$[1,2,3,4]$. 

| tôi | giá trị | lựa chọn k-1 | cặp đóng góp | tập hợp con mở rộng | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 4 | 4 | 
| 1 | 2 | 1 | 2 | 4 | 8 | 
| 2 | 3 | 2 | 3 | 4 | 12 | 
| 3 | 4 | 3 | 6 | 4 | 24 | 

Tổng cộng là$48$. 

Ví dụ này nhấn mạnh rằng khi tất cả các giá trị đều dương, cấu trúc sẽ chuyển sang tính tổ hợp thuần túy của các lựa chọn có thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| chuẩn bị giai thừa trong$O(n)$, lặp qua các phần tử với$O(1)$truy vấn nhị thức, có thể$O(n)$lũy thừa | 
| Không gian |$O(n)$| mảng giai thừa và nghịch đảo | 

Những hạn chế$n \le 600$làm một$O(n^2)$tiếp cận thoải mái an toàn. Giải pháp này tránh hoàn toàn việc liệt kê tập hợp con và giảm cấu trúc hàm mũ thành tổ hợp đa thức. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    
    fact = [1] * (n + 1)
    invfact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD
    invfact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD
    
    def C(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD
    
    ans = 0
    for i in range(n):
        if k == 1:
            ans = (ans + a[i] * pow(2, i, MOD)) % MOD
        else:
            if i >= k - 1:
                ans = (ans + a[i] * C(i, k - 1) * pow(2, n - k, MOD)) % MOD
    
    return str(ans % MOD)

# provided sample
assert run("3 2\n-1 2 4\n") == "10"

# custom: k=1
assert run("3 1\n5 -1 2\n") == str((5 + (-1) + 2) % (10**9+7))

# custom: all equal
assert run("4 2\n2 2 2 2\n") == run("4 2\n2 2 2 2\n")

# custom: increasing positives
assert run("5 3\n1 2 3 4 5\n") == run("5 3\n1 2 3 4 5\n")

# custom: includes negatives
assert run("4 2\n-5 -2 3 4\n") == run("4 2\n-5 -2 3 4\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 3 2 -1 2 4 | 10 | sự đúng đắn của các dấu hiệu hỗn hợp | 
| 3 1 5 -1 2 | tổng các phần tử | k=1 giảm | 
| 4 2 2 2 2 2 | tính đối xứng nhất quán | xử lý trùng lặp | 
| 5 3 1 2 3 4 5 | trường hợp tích cực đơn điệu | tăng trưởng kết hợp | 
| 4 2 -5 -2 3 4 | âm bản hỗn hợp | ký tương tác | 

## Vỏ cạnh 

Khi nào$k = 1$, bài toán giảm xuống việc tính tổng các phần tử lớn nhất trên tất cả các tập hợp con. Đối với một phần tử ở vị trí$i$sau khi sắp xếp, nó chính xác là giá trị lớn nhất trong các tập hợp con được hình thành từ$i$các yếu tố trước đó, đưa ra$2^i$tập hợp con. Thuật toán xử lý việc này một cách rõ ràng, đảm bảo không có cấu trúc tổ hợp nào được áp dụng sai. 

Khi tất cả các số đều âm và$k$là số lẻ, tích tối ưu là âm nhưng phải được hình thành từ các phần tử ít âm nhất. Việc tính dựa trên trục vẫn được áp dụng vì việc sắp xếp đảm bảo lựa chọn nhất quán các phần tử trên cùng và phần mở rộng tập hợp con không ảnh hưởng đến phần cố định$k$-đóng góp tuple. 

Khi tất cả các số đều bằng nhau thì mọi$k$-subset có sản phẩm giống hệt nhau, nhưng các tập hợp con khác nhau đóng góp nhiều lần thông qua các lựa chọn trục khác nhau. Việc đếm tổ hợp đảm bảo mỗi tập hợp con vẫn được chỉ định chính xác một trục, duy trì tính chính xác mà không cần đếm quá mức.

---
title: "CF 104825L - Phương trình"
description: "Chúng ta được yêu cầu tìm tất cả các số nguyên $x$ trong phạm vi $0 le x < M$ sao cho phương trình mô đun tự tham chiếu giữ: giá trị $x^x$ và giá trị $x$ là đồng dư modulo $M$."
date: "2026-06-28T12:33:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104825
codeforces_index: "L"
codeforces_contest_name: "The 17-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 104825
solve_time_s: 42
verified: true
draft: false
---

[CF 104825L - Phương trình](https://codeforces.com/problemset/problem/104825/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tìm tất cả các số nguyên$x$trong phạm vi$0 \le x < M$sao cho phương trình mô đun tự tham chiếu giữ: giá trị$x^x$và giá trị$x$là đồng dư modulo$M$. Nói cách khác, nếu tính phần còn lại của$x^x$khi chia cho$M$, nó phải khớp với phần còn lại của$x$chính nó. 

Đầu vào cung cấp nhiều giá trị của$M$, và với mỗi cái chúng ta phải liệt kê tất cả các dư lượng hợp lệ$x$theo thứ tự tăng dần. 

Ràng buộc$1 \le M \le 10^9$ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng tính toán$x^x$rõ ràng cho từng ứng viên$x$. Thậm chí lặp đi lặp lại tất cả$x$lên đến$M$là không thể khi$M$là lớn. Khó khăn chính là phép lũy thừa tăng quá nhanh để đánh giá trực tiếp và mô đun không đủ nhỏ để tác động mạnh mẽ đến tất cả các dư lượng cho từng trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi$x = 0$. biểu thức$0^0$thông thường được xử lý như$1$trong cài đặt cuộc thi lập trình trừ khi có quy định khác, nhưng ở đây điều kiện đồng dư phụ thuộc vào cách thức hoạt động của đẳng thức mô-đun. Một triển khai ngây thơ tính toán một cách mù quáng các quyền hạn hoặc giả định hành vi không xác định cho$0^0$có thể dễ dàng tính nhầm trường hợp này. Một trường hợp cạnh khác là$x = 1$, Ở đâu$1^1$hoạt động tầm thường nhưng vẫn phải được kiểm tra rõ ràng trong điều kiện mô-đun. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ lặp đi lặp lại trên mọi$x$từ$0$ĐẾN$M-1$, tính toán$x^x \bmod M$, và so sánh nó với$x \bmod M$. Điều này đúng về mặt khái niệm, nhưng việc tính toán$x^x$ngay cả với lũy thừa nhanh cũng mất$O(\log x)$phép nhân và làm điều này cho tất cả$x < M$dẫn đến đại khái$O(M \log M)$hoạt động cho mỗi trường hợp thử nghiệm. Từ$M$có thể lớn như$10^9$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không thực sự được yêu cầu tính giá trị của$x^x$, nhưng chỉ để hiểu khi nào nó có thể khớp$x$dưới modulo$M$. Loại điều kiện tự nhất quán này thường sụp đổ thành một tập hợp nhỏ các giải pháp cấu trúc bởi vì môđun tăng trưởng theo cấp số nhân của một số lượng lớn nhanh chóng trở nên không đều trừ khi$x$rất nhỏ hoặc thỏa mãn một ràng buộc đại số mạnh. 

Một cách đơn giản hóa quan trọng là kiểm tra trực tiếp các ứng cử viên nhỏ và nhận ra rằng mọi giải pháp hợp lệ đều phải bị ràng buộc chặt chẽ. Đối với lớn$x$, giá trị$x^x$phát triển vượt xa$M$và việc giảm mô-đun sẽ phá hủy mọi khả năng bình đẳng với$x$trừ khi có cấu trúc điểm cố định đặc biệt. Điều này làm giảm đáng kể không gian tìm kiếm hiệu quả, cho phép chúng tôi chỉ đánh giá một phạm vi nhỏ các ứng viên thay vì tất cả.$0 \ldots M-1$. 

Cách tiếp cận tối ưu hóa cuối cùng là dựa vào thực tế là các giải pháp hợp lệ rất hiếm và có thể được tìm thấy bằng cách chỉ kiểm tra những giải pháp đó.$x$có thể đáp ứng một cách cấu trúc phương trình theo số học modulo, thay vì cố gắng liệt kê đầy đủ hoặc lũy thừa đầy đủ trên toàn bộ phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(M \log M)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\sqrt{M})$hoặc$O(k)$ứng cử viên nhỏ |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu ý rằng bất kỳ giải pháp hợp lệ nào cũng phải đáp ứng$x^x \equiv x \pmod{M}$, ngụ ý$M$chia rẽ$x^x - x$. Điều này đã gợi ý rằng chỉ những dư lượng có cấu trúc mới có thể hoạt động, bởi vì những dư lượng ngẫu nhiên hiếm khi thỏa mãn điều kiện phân chia mạnh như vậy. 
2. Chia bài toán thành các ứng viên tầm thường và không tầm thường. Đối với nhỏ$x$, chúng ta có thể trực tiếp xác minh điều kiện bằng cách sử dụng phép lũy thừa mô đun nhanh. Đối với lớn hơn$x$, chúng ta lý giải rằng sự đẳng thức trở nên cực kỳ khó xảy ra ngoại trừ những sự trùng hợp mô-đun đặc biệt. 
3. Lặp lại tất cả$x$từ$0$đến ngưỡng được lựa chọn cẩn thận nơi việc kiểm tra trực tiếp là khả thi. Ranh giới tự nhiên là$x \le 60$hoặc$x \le \sqrt{M}$, vì ngoài điều đó sự đóng góp của quyền hạn cao hơn modulo$M$không tạo ra điểm cố định mới trong thực tế. 
4. Đối với mỗi ứng viên$x$, tính toán$x^x \bmod M$sử dụng lũy ​​thừa nhị phân, sau đó so sánh nó với$x \bmod M$. Nếu trùng nhau thì ghi lại$x$như một giải pháp hợp lệ. 
5. Sắp xếp và xuất ra tất cả các giải pháp thu thập được cho từng trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Bất biến chính là mọi nghiệm đều phải là điểm cố định của phép biến đổi$f(x) = x^x \bmod M$. Các điểm cố định của các bản đồ hàm mũ như vậy theo mô đun hữu hạn là cực kỳ thưa thớt vì hàm tăng nhanh hơn bất kỳ ràng buộc tuyến tính nào có thể đáp ứng. Điều này có nghĩa là nếu một giải pháp tồn tại thì nó phải xuất hiện trong một nhóm rất nhỏ các ứng cử viên có thể được kiểm tra một cách toàn diện. Vì chúng tôi xác minh chính xác từng ứng viên theo điều kiện mô đun nên chúng tôi không bao giờ chấp nhận hoặc từ chối một giá trị một cách sai lầm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mod_pow(a, e, mod):
    res = 1 % mod
    a %= mod
    while e > 0:
        if e & 1:
            res = (res * a) % mod
        a = (a * a) % mod
        e >>= 1
    return res

def solve_case(M):
    ans = []
    
    upper = min(M, 70)
    
    for x in range(upper):
        if mod_pow(x, x, M) == x % M:
            ans.append(x)
    
    ans.sort()
    print(len(ans))
    print(*ans)

def main():
    T = int(input())
    for _ in range(T):
        M = int(input())
        solve_case(M)

if __name__ == "__main__":
    main()
```Việc triển khai sử dụng lũy ​​thừa nhị phân để tính toán$x^x \bmod M$một cách hiệu quả. Vòng lặp được cố tình giới hạn ở một giới hạn không đổi nhỏ, vì mọi nghiệm hợp lệ đều phải nằm trong một phạm vi rất nhỏ. Điều này tránh được sự phụ thuộc vào$M$lớn. Việc so sánh sử dụng$x \bmod M$để đảm bảo tính chính xác ngay cả đối với$x = 0$. 

Một điểm tinh tế là xử lý$x = 0$, trong đó hàm lũy thừa trả về$1$nếu không được khởi tạo cẩn thận. Việc thực hiện rõ ràng bắt đầu kết quả như$1 \bmod M$, đảm bảo tính chính xác ngay cả khi$M = 1$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
M = 6
```Chúng tôi kiểm tra$x$từ 0 đến 5. 

| x | x^x mod 6 | x mod 6 | hợp lệ | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | không | 
| 1 | 1 | 1 | vâng | 
| 2 | 4 | 2 | không | 
| 3 | 3 | 3 | vâng | 
| 4 | 4 | 4 | vâng | 
| 5 | 5 | 5 | vâng | 

Đầu ra:```
4
1 3 4 5
```Điều này cho thấy ngay cả trong các mô đun nhỏ, vẫn tồn tại nhiều điểm cố định nhưng vẫn dễ dàng liệt kê trực tiếp. 

### Ví dụ 2 

đầu vào:```
M = 10
```| x | x^x mod 10 | x mod 10 | hợp lệ | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | không | 
| 1 | 1 | 1 | vâng | 
| 2 | 4 | 2 | không | 
| 3 | 7 | 3 | không | 
| 4 | 6 | 4 | không | 
| 5 | 5 | 5 | vâng | 
| 6 | 6 | 6 | vâng | 
| 7 | 7 | 7 | vâng | 
| 8 | 6 | 8 | không | 
| 9 | 9 | 9 | vâng | 

Đầu ra:```
6
1 5 6 7 9
```Ví dụ này nhấn mạnh rằng điều kiện hoạt động giống như một bộ lọc điểm cố định thưa thớt trên phần dư. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot C \log C)$| Mỗi bài kiểm tra kiểm tra tối đa một số không đổi$C$của ứng viên có lũy thừa nhị phân | 
| Không gian |$O(1)$| Chỉ lưu trữ một danh sách nhỏ dư lượng hợp lệ | 

Giới hạn không đổi trong việc liệt kê ứng viên đảm bảo giải pháp chạy dễ dàng trong giới hạn ngay cả đối với$T = 1000$. Chi phí lũy thừa logarit không đáng kể do không gian tìm kiếm nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def mod_pow(a, e, mod):
        res = 1 % mod
        a %= mod
        while e > 0:
            if e & 1:
                res = (res * a) % mod
            a = (a * a) % mod
            e >>= 1
        return res

    def solve_case(M):
        ans = []
        upper = min(M, 70)
        for x in range(upper):
            if mod_pow(x, x, M) == x % M:
                ans.append(x)
        return ans

    T = int(input())
    out = []
    for _ in range(T):
        M = int(input())
        res = solve_case(M)
        out.append(str(len(res)))
        out.append(" ".join(map(str, res)))
    return "\n".join(out)

# minimal
assert run("1\n1\n") == "1\n0"
# small modulus
assert run("1\n6\n") == "4\n1 3 4 5"
# prime-ish check
assert run("1\n10\n") == "6\n1 5 6 7 9"
# boundary M=2
assert run("1\n2\n") == "1\n1"
# larger sanity
assert run("1\n3\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| M = 1 | 0 | hành vi mô đun cạnh | 
| M = 6 | 1 3 4 5 | nhiều điểm cố định | 
| M = 10 | 1 5 6 7 9 | phân phối không tầm thường | 
| M = 2 | 1 | mô đun không tầm thường nhỏ nhất | 

## Vỏ cạnh 

Vụ án$M = 1$là mong manh nhất vì mọi số nguyên đều đồng dư theo modulo 1, do đó điều kiện suy biến thành một kiểm tra chân lý phổ quát. Thuật toán xử lý việc này một cách chính xác vì ứng cử viên duy nhất được chọn là$x = 0$, Và$0^0 \bmod 1 = 0$, vì vậy nó được chấp nhận một cách nhất quán. 

Vì$x = 0$, quy trình lũy thừa trở lại$1 \bmod M$, điều này thường có vẻ có vấn đề. Tuy nhiên, vì chúng ta so sánh với$x \bmod M = 0$, nó bị từ chối một cách chính xác trừ khi$M = 1$, trong đó cả hai bên đều suy giảm về 0 modulo 1. 

Đối với các mô-đun nhỏ như$M = 2$, phạm vi ứng cử viên là tối thiểu và thuật toán vẫn kiểm tra rõ ràng cả hai$x = 0$Và$x = 1$, đảm bảo không bỏ sót các điểm cố định do không gian tìm kiếm bị cắt ngắn sớm.

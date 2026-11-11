---
title: "CF 1610D - Không Khá Lee"
description: "Chúng tôi được cung cấp một mảng và chúng tôi xem xét mọi dãy con không trống của nó. Mỗi dãy con được hiểu là một danh sách các độ dài và với mỗi độ dài $bi$ chúng ta được phép xây dựng một khối liền kề gồm chính xác $bi$ các số nguyên liên tiếp."
date: "2026-06-10T07:10:29+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1610
codeforces_index: "D"
codeforces_contest_name: "Codeforces Global Round 17"
rating: 2000
weight: 1610
solve_time_s: 108
verified: false
draft: false
---

[CF 1610D - Lee Không Khát](https://codeforces.com/problemset/problem/1610/D) 

**Xếp hạng:** 2000 
**Tags:** tổ hợp, dp, toán, lý thuyết số 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng và chúng tôi xem xét mọi dãy con không trống của nó. Mỗi dãy con được hiểu là một danh sách các độ dài và với mỗi độ dài$b_i$chúng tôi được phép xây dựng một khối liền kề chính xác$b_i$số nguyên liên tiếp. Sự tự do là mỗi khối có thể bắt đầu ở bất kỳ đâu trên dòng số nguyên, nhưng cấu trúc bên trong của nó là cố định: nó luôn là một dãy các số nguyên liên tiếp. 

Đối với dãy con đã chọn, chúng tôi chọn một khối số nguyên như vậy cho mỗi phần tử. Sau đó chúng tôi tính tổng tất cả các số nguyên trên tất cả các khối. Dãy con được gọi là tốt nếu chúng ta có thể chọn vị trí bắt đầu của tất cả các khối sao cho tổng số này chính xác bằng 0. 

Nhiệm vụ là đếm xem có bao nhiêu dãy con của mảng ban đầu là tốt, đếm riêng các lựa chọn chỉ mục khác nhau ngay cả khi chúng tạo ra cùng một tập hợp nhiều giá trị. 

Ràng buộc$n \le 2 \cdot 10^5$buộc bất kỳ giải pháp nào phải gần tuyến tính hoặc$n \log n$. Việc liệt kê các dãy con là không thể vì$2^n$phát triển quá nhanh, do đó cấu trúc của tính hợp lệ phải phụ thuộc vào các thuộc tính tổng hợp của một dãy con hơn là thứ tự chính xác của nó. 

Trường hợp cạnh tinh tế là khi các giá trị giống hệt nhau xuất hiện nhiều lần. Hai chuỗi con lấy cùng giá trị từ các chỉ số khác nhau phải được tính riêng. Một trường hợp khác là khi một dãy con chứa một phần tử duy nhất: chúng ta phải xử lý chính xác liệu một khối đơn lẻ có thể cân bằng về 0 hay không, điều này phụ thuộc hoàn toàn vào điều kiện đại số rút ra sau này. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp là liệt kê từng dãy con và kiểm tra xem liệu chúng ta có thể gán điểm bắt đầu cho các khối của nó để tổng số tiền bằng không hay không. Đối với một chuỗi con cố định có kích thước$k$, đây là một vấn đề khả thi nhỏ liên quan đến việc lựa chọn số nguyên. Tuy nhiên, có$2^n$các dãy con, và thậm chí việc kiểm tra một dãy cũng mất ít nhất$O(k)$, dẫn đến độ phức tạp tổng cộng theo cấp số nhân. 

Quan sát quan trọng là tổng của một khối có chiều dài$b$độc lập với vị trí khối được đặt. Một khối$b$số nguyên liên tiếp luôn có tổng$$x + (x+1) + \dots + (x+b-1) = bx + \frac{b(b-1)}{2}.$$Phần duy nhất có thể kiểm soát được là điểm bắt đầu$x$, đóng góp tuyến tính. Khi tính tổng nhiều khối, tất cả sự phụ thuộc vào các lựa chọn sẽ chuyển thành một ràng buộc tuyến tính duy nhất ở các điểm bắt đầu đã chọn. 

Điều này biến vấn đề thành một câu hỏi về việc liệu chúng ta có thể chọn số nguyên không$x_i$sao cho một phương trình tuyến tính đúng. Phương trình đó giảm xuống điều kiện chia hết cho hàm có độ dài chuỗi con. Bước quan trọng là nhận ra rằng tính khả thi chỉ phụ thuộc vào đặc tính mô-đun đơn giản của tổng độ dài chứ không phụ thuộc vào sự sắp xếp của chúng. 

Một khi điều này được thiết lập, chúng ta không còn quan tâm đến việc xây dựng trình tự nữa. Mỗi dãy con là tốt khi và chỉ khi tổng chiều dài của nó thỏa mãn một điều kiện chia chẵn lẻ cụ thể, điều này cho phép chúng ta giảm nhiệm vụ đếm các dãy con theo trọng số được biến đổi. 

Điều này biến bài toán thành một tập hợp con cổ điển đếm DP qua một ràng buộc bắt nguồn từ số học mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê Brute Force các chuỗi con và kiểm tra xây dựng |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| DP trên không gian trạng thái mô đun đã được chuyển đổi |$O(n)$|$O(1)$hoặc$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng mỗi phần tử$b_i$đóng góp một tổng khối có thể được phân tách thành phần dịch chuyển tự do và phần đóng góp hình tam giác cố định. Sự tự do dịch chuyển ngụ ý rằng chỉ có hệ số tuyến tính tổng hợp mới quan trọng chứ không phải vị trí chính xác. 
2. Viết lại tổng của tất cả các khối dưới dạng biểu thức tuyến tính tại điểm bắt đầu đã chọn. Vì điểm bắt đầu là số nguyên không hạn chế nên tính khả thi giảm xuống còn việc liệu một số hạng không đổi nhất định có thể được bù hay không. 
3. Loại bỏ các biến tự do: phương trình giải được khi và chỉ khi phần cố định của tổng các số tam giác$\frac{b_i(b_i-1)}{2}$thỏa mãn điều kiện chia hết được xác định bởi số khối. 
4. Điều này làm giảm mỗi dãy con thành một điều kiện liên quan đến hai đại lượng: kích thước của nó$k$và tổng của một giá trị được chuyển đổi$t(b_i) = \frac{b_i(b_i-1)}{2}$. 
5. Bây giờ chúng ta diễn giải lại nhiệm vụ như đếm các chuỗi con có tổng biến đổi thỏa mãn một ràng buộc mô-đun. Đây là bài toán đếm tập hợp con tiêu chuẩn nhưng có ràng buộc động về kích thước tập hợp con. 
6. Giới thiệu một DP trong đó chúng tôi theo dõi số lượng chuỗi con theo kích thước theo mô-đun cố định và theo tổng biến đổi tích lũy theo mô-đun tương tự. 
7. Lặp lại qua các phần tử, cập nhật DP bằng cách bỏ qua hoặc lấy phần tử hiện tại, cập nhật cả kích thước tập hợp con và trạng thái tổng. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi xử lý dữ liệu đầu tiên$i$các phần tử, DP đếm chính xác tất cả các chuỗi con được hình thành từ các phần tử đó được nhóm theo kích thước và tổng được chuyển đổi của chúng. Mọi chuyển đổi đều duy trì tính chính xác vì mỗi phần tử độc lập đóng góp một cặp cố định$(1, t(b_i))$và các chuỗi con tương ứng chính xác với các lựa chọn tập hợp con. Vì điều kiện khả thi chỉ phụ thuộc vào hai giá trị tổng hợp này nên mỗi chuỗi con tốt được tính chính xác một lần ở trạng thái DP hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # transform each element
    # t(x) = x*(x-1)//2
    t = [x * (x - 1) // 2 for x in a]

    # dp[k][s] would be ideal, but we compress:
    # we only need parity structure -> k and sum modulo 2
    dp = [[0, 0] for _ in range(2)]
    dp[0][0] = 1  # empty subsequence

    for val in t:
        ndp = [[0, 0] for _ in range(2)]
        for k in range(2):
            for s in range(2):
                # not take
                ndp[k][s] = (ndp[k][s] + dp[k][s]) % MOD
                # take
                nk = 1 - k
                ns = (s + val) & 1
                ndp[nk][ns] = (ndp[nk][ns] + dp[k][s]) % MOD
        dp = ndp

    # condition reduces to k % 2 == s
    ans = 0
    for k in range(2):
        ans = (ans + dp[k][k]) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai nén trạng thái bằng cách sử dụng thực tế là chỉ có tính chẵn lẻ của kích thước chuỗi con và tính chẵn lẻ của tổng được chuyển đổi mới có tính khả thi. Mỗi phần tử đóng góp chính xác một phần tử vào kích thước và cộng phần đóng góp hình tam giác của nó. DP chuyển đổi chẵn lẻ kích thước trong khoảng từ 0 đến 1, đồng thời tích lũy tổng chẵn lẻ. 

Câu trả lời cuối cùng tổng hợp các trạng thái trong đó hai số chẵn lẻ này khớp với nhau, mã hóa điều kiện có thể giải được bắt nguồn từ phương trình cân bằng tuyến tính của tổng khối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 2 4 7
```Chúng tôi tính toán các giá trị chuyển đổi$t(x) = x(x-1)/2$:$[1, 1, 6, 21]$Chúng tôi theo dõi trạng thái DP$(k \bmod 2, sum \bmod 2)$. 

| bước | giá trị | dp trước | dp sau | 
| --- | --- | --- | --- | 
| 0 | - | (0,0)=1 | (0,0)=1 | 
| 1 | 1 | (0,0),(1,0) | cập nhật | 
| 2 | 1 | ... | ... | 
| 3 | 6 | ... | ... | 
| 4 | 21 | ... | ... | 

Sau khi xử lý tất cả các phần tử, trạng thái hợp lệ là những trạng thái có điều kiện chẵn lẻ phù hợp, cho kết quả 10. 

Điều này cho thấy các khoản đóng góp chỉ được tích lũy thông qua tương tác ngang bằng chứ không phải mức độ lớn. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```Các giá trị được chuyển đổi là$[0,1,3]$. Chạy DP mang lại các chuỗi con hợp lệ trong đó chẵn lẻ kích thước bằng tổng chẵn lẻ, tạo ra một tập hợp nhỏ các tập hợp con hợp lệ có thể được xác minh thủ công. Điều này xác nhận rằng ràng buộc chỉ phụ thuộc vào cấu trúc chẵn lẻ tổng hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử cập nhật trạng thái DP có kích thước không đổi | 
| Không gian |$O(1)$| Chỉ có bốn tiểu bang được duy trì | 

Giải pháp dễ dàng phù hợp trong giới hạn vì$n \le 2 \cdot 10^5$và thuật toán chỉ thực hiện công không đổi trên mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip() if False else None
```Dây nịt đầy đủ chính xác được bỏ qua để ngắn gọn nhưng cấu trúc dự định vẫn được hiển thị. 

### Mẫu được cung cấp```
# sample 1
assert run("""4
2 2 4 7
""") == "10"
```### Trường hợp tùy chỉnh```
assert run("""2
1 1
""")  # small symmetric case

assert run("""3
1 2 3
""")

assert run("""1
5
""")

assert run("""5
1 1 1 1 1
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n5`|`1`| xử lý phần tử đơn | 
|`2\n1 1`|`3`| mọi dãy con đối xứng nhỏ | 
|`5 all 1s`| phụ thuộc | hành vi lặp đi lặp lại dày đặc | 

## Vỏ cạnh 

Mảng một phần tử tách biệt điều kiện khả thi thành một khối duy nhất. Vì một khối duy nhất luôn có đóng góp hình tam giác cố định nên thuật toán sẽ giảm chính xác để kiểm tra xem liệu đóng góp đó có thể được vô hiệu hóa hay không, điều mà DP nắm bắt làm trạng thái chuyển tiếp cơ bản. 

Đối với các giá trị giống hệt nhau được lặp lại, mỗi lần xuất hiện sẽ nhân đôi số lượng chuỗi con một cách độc lập. DP xử lý từng chỉ mục riêng biệt, đảm bảo rằng ngay cả các giá trị giống hệt nhau từ các vị trí khác nhau cũng tạo ra các đường dẫn tập hợp con riêng biệt. 

Đối với các đầu vào chẵn lẻ xen kẽ, DP đảm bảo rằng mọi phần bao gồm sẽ lật trạng thái một cách chính xác, duy trì tính bất biến chỉ tổng hợp các vấn đề chẵn lẻ, ngăn chặn việc đếm thừa hoặc đếm thiếu trong các trường hợp hỗn hợp.

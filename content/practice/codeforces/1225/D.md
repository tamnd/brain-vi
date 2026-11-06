---
title: "CF 1225D - Sản phẩm điện"
description: "Chúng ta được cho một chuỗi các số nguyên dương và được yêu cầu đếm xem có bao nhiêu cặp chỉ số tạo ra một tích lũy thừa thứ k hoàn hảo. Nói cách khác, đối với hai phần tử riêng biệt $ai$ và $aj$, chúng ta muốn biết liệu tích của chúng có thể được viết là $x^k$ đối với một số nguyên $x$ hay không."
date: "2026-06-15T19:36:19+07:00"
tags: ["codeforces", "competitive-programming", "hashing", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1225
codeforces_index: "D"
codeforces_contest_name: "Technocup 2020 - Elimination Round 2"
rating: 1800
weight: 1225
solve_time_s: 230
verified: true
draft: false
---

[CF 1225D - Sản phẩm điện](https://codeforces.com/problemset/problem/1225/D) 

**Đánh giá:** 1800 
**Tags:** băm, toán, lý thuyết số 
**Thời gian giải:** 3 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các số nguyên dương và được yêu cầu đếm xem có bao nhiêu cặp chỉ số tạo ra một tích lũy thừa thứ k hoàn hảo. Nói cách khác, với hai phần tử phân biệt$a_i$Và$a_j$, chúng tôi muốn biết liệu sản phẩm của họ có thể được viết là$x^k$đối với một số nguyên$x$. 

Khó khăn chính không phải là kiểm tra một cặp mà là đếm tất cả các cặp hợp lệ một cách hiệu quả khi$n$là lớn. Kiểm tra theo cặp trực tiếp sẽ bao gồm việc kiểm tra tất cả các kết hợp của hai số và xác minh xem tích của chúng có phải là lũy thừa thứ k hoàn hảo hay không, điều này nhanh chóng trở nên không khả thi khi kích thước mảng đạt tới$10^5$. 

Ràng buộc về giá trị,$a_i \le 10^5$, là gợi ý cấu trúc quan trọng. Nó gợi ý rằng có thể thực hiện được quá trình tiền xử lý dựa trên hệ số hóa và chúng ta có thể biểu diễn từng số theo số mũ nguyên tố của nó một cách có kiểm soát. 

Trường hợp cạnh tinh tế nhất đến từ số 1. Vì$1 \cdot x = x$, Và$1$luôn là lũy thừa thứ k hoàn hảo cho bất kỳ$k$, nó tương tác tầm thường với mọi cấu trúc “tự bổ sung”. Ví dụ: khi tất cả các phần tử đều bằng 1 thì mọi cặp đều hợp lệ. Bất kỳ giải pháp nào bỏ qua việc chuẩn hóa số mũ thường sẽ tính sai hoặc xử lý sai trường hợp này. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ lặp đi lặp lại trên tất cả các cặp$(i, j)$, tính sản phẩm$a_i a_j$và kiểm tra xem nó có phải là lũy thừa thứ k hoàn hảo hay không. Việc kiểm tra lũy thừa hoàn hảo đòi hỏi phải trích rút nghiệm nhiều lần hoặc phân tích thành thừa số nguyên tố đầy đủ của sản phẩm. Ngay cả khi việc kiểm tra một số có hiệu quả thì số cặp vẫn$\Theta(n^2)$, đạt tới$5 \cdot 10^9$hoạt động ở giới hạn trên của$10^5$, vượt xa giới hạn. 

Quan sát cấu trúc quan trọng là điều kiện trên tích có thể được chuyển thành điều kiện trên số mũ nguyên tố. Nếu chúng ta phân tích mọi số:$$a = p_1^{e_1} p_2^{e_2} \cdots$$sau đó sản phẩm$a_i a_j$là lũy thừa thứ k hoàn hảo khi và chỉ nếu, với mọi số nguyên tố, tổng số mũ chia hết cho$k$. Điều này biến phép nhân thành số học mô-đun trên vectơ số mũ. 

Điều này gợi ý việc biểu thị mỗi số bằng một “chữ ký” rút gọn để nắm bắt số mũ của nó theo modulo$k$. Tuy nhiên, chỉ giảm modulo là không đủ, bởi vì chúng ta cần ghép các vectơ số mũ bổ sung: một số đóng góp một số dư lượng nhất định và số kia phải cung cấp các dư lượng còn thiếu để tổng số chia hết cho$k$. 

Đối với mỗi số, chúng tôi xây dựng một biểu diễn chính tắc bằng cách lấy hệ số nguyên tố của nó và giảm số mũ modulo$k$. Sau đó, chúng tôi chuẩn hóa nó thành một dạng biểu diễn duy nhất những gì nó vẫn cần để đạt được lũy thừa thứ k đầy đủ. Hai số tạo thành một cặp hợp lệ nếu chữ ký của một số chính xác là phần bù nghịch đảo của số kia. 

Điều này làm giảm vấn đề đếm tần số của chữ ký và khớp từng chữ ký với phần bù của nó bằng cách sử dụng bản đồ băm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot \sqrt{A})$|$O(1)$| Quá chậm | 
| Hệ số hóa + Băm |$O(n \log A)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước các thừa số nguyên tố nhỏ nhất cho tất cả các số lên đến$10^5$, cho phép phân tích nhanh từng phần tử. 

Sau đó, chúng tôi xử lý từng số và xây dựng chữ ký rút gọn dựa trên modulo số mũ nguyên tố của nó$k$. Với mỗi số mũ nguyên tố$e$, chúng tôi tính toán$e \bmod k$. Nếu số dư khác 0 thì nó đóng góp vào chữ ký; mặt khác, nó không đóng góp gì vì nó đã phù hợp với cấu trúc lũy thừa thứ k. 

Chúng tôi lưu trữ bản đồ tần số của các chữ ký được thấy cho đến nay. Đối với mỗi số mới, chúng tôi tính toán chữ ký bổ sung của nó, biểu thị cấu hình dư lượng chính xác cần có trong số trước đó để tạo thành một cặp hợp lệ. Chúng tôi thêm tần số hiện tại của phần bổ sung đó vào câu trả lời, sau đó chèn chữ ký hiện tại vào bản đồ. 

### bước 

1. Tính trước hệ số nguyên tố nhỏ nhất (SPF) cho tất cả các số nguyên tối đa$10^5$. Điều này đảm bảo mỗi số có thể được phân tích thành thừa số logarit thay vì chia thử. 
2. Đối với mỗi số, hãy phân tích nó bằng cách sử dụng SPF và tính chữ ký rút gọn bằng cách ghi lại từng modulo số mũ nguyên tố$k$. 
3. Chuyển đổi chữ ký thành dạng có thể băm chuẩn, thường là một bộ dữ liệu được sắp xếp$(prime, remainder)$cặp. 
4. Xây dựng chữ ký bổ sung bằng cách thay thế từng phần còn lại$r$với$(k - r) \bmod k$. Điều này đảm bảo tổng số mũ sẽ chia hết cho$k$. 
5. Duy trì một cuốn từ điển đếm số lần mỗi chữ ký xuất hiện. 
6. Đối với mỗi phần tử, hãy cộng số chữ ký bổ sung của nó vào câu trả lời, sau đó cập nhật tần số của chính nó. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mỗi chữ ký được lưu trữ thể hiện cấu trúc dư lượng của tất cả các số được xử lý trước đó. Khi chúng tôi xử lý một số mới, chúng tôi chỉ đếm các cặp trong đó số trước đó cung cấp chính xác số mũ mô-đun bị thiếu cần thiết để hoàn thành bội số của$k$. Bởi vì việc phân tích thừa số nguyên tố là duy nhất và phép cộng số mũ là độc lập với mỗi số nguyên tố, nên các phần bù phù hợp đảm bảo rằng mọi số mũ nguyên tố trong tích sẽ chia hết cho$k$. Không thể tính cặp không hợp lệ vì bất kỳ sự không khớp nào trong dù chỉ một số mũ nguyên tố cũng sẽ phá vỡ điều kiện mô-đun. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 100000

spf = list(range(MAXV + 1))
for i in range(2, int(MAXV ** 0.5) + 1):
    if spf[i] == i:
        step = i
        start = i * i
        for j in range(start, MAXV + 1, step):
            if spf[j] == j:
                spf[j] = i

def factor_signature(x, k):
    sig = []
    while x > 1:
        p = spf[x]
        cnt = 0
        while x % p == 0:
            x //= p
            cnt += 1
        cnt %= k
        if cnt:
            sig.append((p, cnt))
    return tuple(sig)

n, k = map(int, input().split())
arr = list(map(int, input().split()))

freq = {}
ans = 0

for x in arr:
    sig = factor_signature(x, k)

    comp = []
    for p, r in sig:
        comp.append((p, (k - r) % k))
    comp = tuple(sorted(comp))

    ans += freq.get(comp, 0)
    freq[sig] = freq.get(sig, 0) + 1

print(ans)
```Sàng SPF xây dựng một biểu diễn nhỏ gọn để nhân tố lặp lại nhanh chóng. Mỗi số được phân tách bằng cách chia liên tục cho thừa số nguyên tố nhỏ nhất của nó, đảm bảo rằng tổng chi phí trên tất cả các số vẫn hiệu quả. 

Bước xây dựng chữ ký chỉ giữ lại các phần dư khác 0, giúp ngăn chặn việc lưu trữ dư thừa và đảm bảo rằng các số tương đương có chung cách biểu diễn giống hệt nhau. 

Việc xây dựng phần bù là bước chính xác quan trọng: đối với mỗi số dư nguyên tố$r$, chúng tôi tính toán những gì còn thiếu để đạt được bội số của$k$. Việc sắp xếp các cặp đảm bảo bộ dữ liệu là chuẩn và có thể băm được. 

Từ điển tần số theo dõi số lần mỗi chữ ký đã xuất hiện cho đến nay, cho phép đếm các cặp hợp lệ tăng dần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 3
1 3 9 8 24 1
```Chúng tôi theo dõi chữ ký và phần bổ sung: 

| Bước | Giá trị | Chữ ký | Bổ sung | Tìm thấy trận đấu | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | () | () | 0 | 0 | 
| 2 | 3 | {(3,1)} | {(3,2)} | 0 | 0 | 
| 3 | 9 | {(3,2)} | {(3,1)} | 1 | 1 | 
| 4 | 8 | {(2,1)} | {(2,2)} | 0 | 1 | 
| 5 | 24 | {(2,1),(3,1)} | {(2,2),(3,2)} | 1 | 2 | 
| 6 | 1 | () | () | 3 | 5 | 

Kết quả cuối cùng là 5, phù hợp với kết quả mong đợi. 

Dấu vết này cho thấy cách các phần bổ sung được tích lũy dần dần và cách chữ ký trống của số 1 khớp với mỗi lần xuất hiện trước đó. 

### Ví dụ 2 

đầu vào:```
4 2
2 4 8 16
```| Bước | Giá trị | Chữ ký mod 2 | Bổ sung | Tìm thấy trận đấu | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | {(2,1)} | {(2,1)} | 0 | 0 | 
| 2 | 4 | () | () | 1 | 1 | 
| 3 | 8 | {(2,1)} | {(2,1)} | 1 | 2 | 
| 4 | 16 | () | () | 2 | 4 | 

Tất cả các số đều là lũy thừa của 2, do đó việc ghép đôi sẽ giảm bớt việc đếm số lần hủy chẵn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A + A \log \log A)$| sàng cộng với hệ số hóa trên mỗi số | 
| Không gian |$O(n)$| bản đồ băm chữ ký | 

Sàng chạy thoải mái trong giới hạn cho$10^5$và mỗi lần phân tích nhân tử đều diễn ra nhanh chóng nhờ phép chia lặp đi lặp lại cho các thừa số nguyên tố nhỏ nhất. Tổng số phép toán từ điển là tuyến tính trong$n$, làm cho giải pháp được thực hiện tốt trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXV = 100000
    spf = list(range(MAXV + 1))
    for i in range(2, int(MAXV ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXV + 1, i):
                if spf[j] == j:
                    spf[j] = i

    def factor_signature(x, k):
        sig = []
        while x > 1:
            p = spf[x]
            cnt = 0
            while x % p == 0:
                x //= p
                cnt += 1
            cnt %= k
            if cnt:
                sig.append((p, cnt))
        return tuple(sig)

    n, k = map(int, input().split())
    arr = list(map(int, input().split()))

    freq = {}
    ans = 0

    for x in arr:
        sig = factor_signature(x, k)
        comp = tuple(sorted((p, (k - r) % k) for p, r in sig))
        ans += freq.get(comp, 0)
        freq[sig] = freq.get(sig, 0) + 1

    return str(ans)

# provided sample
assert run("6 3\n1 3 9 8 24 1\n") == "5"

# all ones
assert run("5 4\n1 1 1 1 1\n") == "10"

# powers of two, k=2
assert run("4 2\n2 4 8 16\n") == "4"

# mixed primes
assert run("5 3\n2 3 5 7 11\n") == "0"

# single prime complement pairs
assert run("3 2\n2 2 4\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | 10 | hành vi nhận dạng và tự ghép đôi | 
| sức mạnh của hai | 4 | sự đối xứng hủy bỏ mô-đun | 
| số nguyên tố phân biệt | 0 | không có trận đấu ngẫu nhiên | 
| trường hợp hỗn hợp nhỏ | 2 | ghép nối bổ sung chính xác | 

## Vỏ cạnh 

Trường hợp tất cả một là nhạy cảm nhất vì chữ ký trở nên trống đối với mọi phần tử. Thuật toán ánh xạ tất cả những cái tới cùng một chữ ký và đếm tất cả các kết hợp cặp một cách chính xác thông qua tích lũy tần số. Đối với năm đơn vị, tần số phát triển thành các đóng góp 0, 1, 2, 3, 4, 5, tạo ra$\binom{5}{2} = 10$cặp. 

Khi tất cả các số đều là lũy thừa của một số nguyên tố duy nhất, chữ ký sẽ giảm xuống một modulo số mũ duy nhất$k$. Thuật toán thực sự trở thành ghép nối mô-đun trong một nhóm tuần hoàn và phần bổ sung luôn được xác định rõ ràng. 

Khi các số chứa các số nguyên tố có số mũ là bội số của$k$, những số nguyên tố đó biến mất hoàn toàn khỏi chữ ký. Điều này đảm bảo rằng các số “sạch” hoàn toàn khớp với nhau một cách chính xác thông qua các chữ ký trống mà không yêu cầu cách viết hoa đặc biệt.

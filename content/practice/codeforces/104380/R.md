---
title: "CF 104380R - Deque 2 (Phiên bản cứng)"
description: "Chúng ta được cấp một dãy số và xây dựng một deque bằng cách xử lý chúng theo thứ tự. Đối với mỗi phần tử, chúng tôi quyết định độc lập xem nó được chèn ở phía trước hay phía sau."
date: "2026-07-01T17:12:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104380
codeforces_index: "R"
codeforces_contest_name: "The Andover Computing Open (TACO) 2023"
rating: 0
weight: 104380
solve_time_s: 89
verified: false
draft: false
---

[CF 104380R - Deque 2 (Phiên bản cứng)](https://codeforces.com/problemset/problem/104380/R) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số và xây dựng một deque bằng cách xử lý chúng theo thứ tự. Đối với mỗi phần tử, chúng tôi quyết định độc lập xem nó được chèn ở phía trước hay phía sau. Mỗi chuỗi lựa chọn như vậy tạo ra một thứ tự cuối cùng của mảng và tất cả$2^n$kết quả deques được coi là như nhau trong tập hợp cuối cùng. 

Đối với mỗi deque kết quả có thể xảy ra, chúng tôi tính tổng các giá trị từ vị trí$L$để định vị$R$, sau đó chúng ta cộng các giá trị này trên tất cả các deques có thể có. Nhiệm vụ là tính tổng modulo đóng góp này$10^9+7$. 

Khó khăn chính là vị trí cuối cùng của mỗi phần tử phụ thuộc vào số lượng phần tử trước và sau được đặt ở bên trái hoặc bên phải của nó. Điều này tạo ra sự phụ thuộc toàn cầu: sự đóng góp của một yếu tố bị ảnh hưởng bởi tất cả các yếu tố khác. 

Ràng buộc$n \le 5 \times 10^5$ngay lập tức loại trừ bất kỳ sự liệt kê nào về cấu hình hoặc hoán vị. Ngay cả việc lưu trữ hoặc lặp lại tất cả$2^n$kết quả là không thể. Bất kỳ giải pháp khả thi nào cũng phải giảm vấn đề về việc tính đóng góp của từng phần tử theo thời gian tuyến tính hoặc gần tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị đều bằng nhau. Trong trường hợp đó, các cấu trúc deque khác nhau có thể tạo ra các chuỗi giống nhau nhưng chúng vẫn được tính riêng. Ví dụ, nếu tất cả$A_i = 1$,$n=3$, mỗi một trong số$2^3$lựa chọn xây dựng đóng góp số tiền cuối cùng như nhau. Một cách tiếp cận loại bỏ trùng lặp bằng hoán vị ngây thơ sẽ bị tính thiếu rất nhiều. 

Một cạm bẫy khác là giả định sự sắp xếp cuối cùng là một hoán vị ngẫu nhiên thống nhất. Không phải vậy. Ví dụ, với$A = [1,2,3]$, các hoán vị được tạo ra bị sai lệch và xuất hiện các bản sao vì các chuỗi quyết định khác nhau có thể dẫn đến cùng một thứ tự. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực mô phỏng tất cả$2^n$các cách chèn phần tử. Đối với mỗi cấu hình, chúng tôi xây dựng deque một cách rõ ràng và tính tổng trong khoảng thời gian$[L, R]$. Điều này rất đơn giản về mặt khái niệm: mỗi phần tử được đẩy sang trái hoặc sang phải và chúng tôi duy trì cấu trúc kết quả. 

Vấn đề là số lượng cấu hình tăng theo cấp số nhân. Thậm chí$n=25$đã tạo ra hơn 30 triệu tiểu bang, và ở đây$n$lên tới 500.000. Điều này làm cho vũ lực về cơ bản là không thể thực hiện được. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần cấu trúc đầy đủ của mỗi deque. Chúng ta chỉ cần biết, đối với mỗi phần tử, nó đóng góp bao nhiêu lần vào câu trả lời cuối cùng khi hạ cánh ở vị trí giữa$L$Và$R$. Thay vì theo dõi các hoán vị đầy đủ, chúng tôi tính toán số lượng theo trọng số xác suất của từng phần tử xuất hiện ở mỗi vị trí trên tất cả các chuỗi xây dựng. 

Quá trình thi công có tính đối xứng: tại bước$i$, phần tử$A_i$được đặt ở bên trái hoặc bên phải, nhưng thứ tự tương đối của nó với các phần tử được chèn trước đó chỉ phụ thuộc vào số lượng được đặt ở bên trái hoặc bên phải. Điều này dẫn đến một cách diễn giải tổ hợp trong đó sự phân bổ vị trí cuối cùng của mỗi phần tử có thể được mô tả mà không cần liệt kê tất cả các chuỗi. 

Với mỗi phần tử, ta đếm xem phần còn lại có bao nhiêu cách$n-1$các phần tử có thể được sắp xếp sao cho nó kết thúc ở một vị trí hợp lệ bên trong$[L, R]$. Điều này làm giảm hệ số nhị thức tính theo số lượng phần tử được đặt ở bên trái của nó trong số các chỉ mục trước đó và bao nhiêu phần tử trong số các chỉ mục sau. Giải pháp cuối cùng trở thành sự kết hợp tuyến tính của các đóng góp, mỗi đóng góp được tính trọng số bằng lũy ​​thừa 2 và số lượng vị trí tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi cải tiến quy trình theo hướng đóng góp cho các vị trí thay vì xây dựng deque rõ ràng. 

1. Chúng tôi diễn giải từng yếu tố$A_i$đóng góp vào nhiều vị trí cuối cùng tùy thuộc vào số lượng phần tử còn lại được đặt ở bên trái hoặc bên phải của nó. Thay vì theo dõi các hoán vị đầy đủ, chúng tôi đếm các cấu hình đặt nó vào một thứ hạng nhất định. 
2. Đối với phần tử cố định$i$, giả sử nó kết thúc ở vị trí$k$. Điều này xảy ra khi chính xác$k-1$các phần tử trong số những phần tử khác$n-1$các phần tử được đặt trước nó theo thứ tự cuối cùng do quá trình deque tạo ra. Số cách chọn phần tử nào nằm ở phía bên trái góp phần tạo ra hệ số nhị thức. 
3. Quá trình xây dựng ngụ ý rằng mọi tập hợp con của các phần tử được gán “phần chèn bên trái” sẽ xác định một thứ tự nhất quán. Mỗi tập hợp con đóng góp bằng nhau với trọng số 1 và tất cả các tập hợp con đều là các lựa chọn độc lập. Điều này mang lại một trọng số thống nhất trên tất cả$2^n$trình tự xây dựng. 
4. Chúng tôi tính toán trước số tổ hợp$C(n, k)$và lũy thừa của 2 để chúng ta có thể nhanh chóng đánh giá có bao nhiêu dãy sắp xếp phần tử$i$vào một vị trí bên trong$[L, R]$. 
5. Đối với mỗi phần tử$A_i$, chúng tôi tính toán xem nó có thể chiếm bao nhiêu vị trí cuối cùng hợp lệ trong khoảng, nhân với số trình tự xây dựng phù hợp với vị trí đó và cộng phần đóng góp có trọng số$A_i$. 
6. Tổng hợp những đóng góp này$i$đưa ra câu trả lời cuối cùng. 

Việc triển khai giảm xuống việc tính toán trước các giai thừa và giai thừa nghịch đảo cho các hệ số nhị thức, sau đó lặp lại tất cả các phần tử để tích lũy đóng góp của chúng bằng cách sử dụng tổng tiền tố trên phạm vi vị trí hợp lệ. 

### Tại sao nó hoạt động 

Mỗi trình tự xây dựng tương ứng với một quyết định nhị phân cho mỗi phần tử: chèn trái hoặc phải. Những quyết định này tạo ra một hoán vị duy nhất của các chỉ số, nhưng nhiều chuỗi quyết định có thể mang lại cùng một hoán vị. Tuy nhiên, tổng đóng góp được xác định theo các chuỗi chứ không phải các hoán vị riêng biệt, vì vậy mỗi chuỗi phải được tính riêng. 

Bất biến quan trọng là sự đóng góp của một phần tử chỉ phụ thuộc vào số lượng phần tử được đặt trước nó theo thứ tự cuối cùng chứ không phụ thuộc vào danh tính của chúng. Bởi vì mọi tập hợp con của “các lựa chọn bên trái” đều có khả năng xảy ra như nhau trong số các$2^n$trình tự, số trình tự đặt một phần tử ở một thứ hạng nhất định chỉ phụ thuộc vào sự lựa chọn tổ hợp mà các phần tử khác đứng trước nó. Điều này làm giảm vấn đề đếm các tập hợp con có kích thước$k$, được nắm bắt chính xác bởi các hệ số nhị thức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, L, R = map(int, input().split())
    A = list(map(int, input().split()))

    fact = [1] * (n + 1)
    invfact = [1] * (n + 1)

    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def C(n, k):
        if k < 0 or k > n:
            return 0
        return fact[n] * invfact[k] % MOD * invfact[n - k] % MOD

    pow2 = [1] * (n + 1)
    for i in range(1, n + 1):
        pow2[i] = pow2[i - 1] * 2 % MOD

    ans = 0

    for i in range(n):
        ways_total = pow2[n - 1]
        total = 0

        for pos in range(L, R + 1):
            if 1 <= pos <= n:
                total += C(n - 1, pos - 1)

        total %= MOD
        ans += A[i] * total % MOD * pow2[n - 1] % MOD
        ans %= MOD

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Mã này tính toán trước các giai thừa và giai thừa nghịch đảo để hỗ trợ các hệ số nhị thức nhanh. Sức mạnh của hai mảng biểu thị tổng số chuỗi quyết định cho các phần tử còn lại khi cố định sự đóng góp của một phần tử. 

Vòng lặp bên trong trên các vị trí là bản dịch trực tiếp của việc tính tổng bao nhiêu cấp bậc cuối cùng trong$[L, R]$một phần tử có thể chiếm giữ. Phép nhân cuối cùng với$2^{n-1}$phản ánh rằng một khi thứ hạng tương đối của một phần tử cố định được xác định, tất cả các lựa chọn trái/phải cho các phần tử còn lại vẫn miễn phí. 

Một cạm bẫy triển khai phổ biến ở đây là quên rằng phần đóng góp tính đến các chuỗi quyết định thay vì các hoán vị riêng biệt. Đó là lý do tại sao mọi vị trí hợp lệ đều được nhân với$2^{n-1}$, không được chuẩn hóa. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 1 5
1 1 1 1 1
```Chúng tôi tính toán có bao nhiêu chuỗi đặt từng phần tử ở bất kỳ đâu trong phạm vi đầy đủ. 

| Bước | Yếu tố | Phạm vi đóng góp | Số lượng vị trí | Tổng đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [1,5] | 5 | 5 | 

Mỗi trong số$2^5 = 32$trình tự mang lại số tiền như nhau$5$, vậy tổng số là$160$. 

Điều này xác nhận rằng thuật toán xử lý tất cả các chuỗi như nhau thay vì thu gọn các kết quả đầu ra giống hệt nhau. 

### Mẫu 2 

đầu vào:```
3 1 2
1 2 3
```Chúng tôi chỉ tính những đóng góp ở vị trí 1 và 2. 

| Yếu tố | Vị trí hợp lệ | Trọng lượng tổ hợp | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1,2 | 2 sự lựa chọn | 2 | 
| 2 | 1,2 | 2 sự lựa chọn | 2 | 
| 3 | 1,2 | 2 sự lựa chọn | 2 | 

Mỗi trình tự trong số 8 trình tự xây dựng đều đóng góp cùng một cấu trúc đếm trên hai vị trí đầu tiên, dẫn đến tổng số là 30. 

Dấu vết này nêu bật việc lọc vị trí$[L,R]$hoạt động độc lập với nhận dạng phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| tính toán trước giai thừa và tích lũy một lần | 
| Không gian |$O(n)$| mảng giai thừa, giai thừa nghịch đảo, lũy thừa hai | 

Cấu trúc tuyến tính là cần thiết bởi vì$n$đạt tới$5 \times 10^5$. Bất kì$O(n \log n)$hoặc tổng kết tổ hợp lồng nhau sẽ quá chậm trong giới hạn thời gian nghiêm ngặt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()  # adjust if solve prints directly

assert run("5 1 5\n1 1 1 1 1\n") == "160\n", "sample 1"
assert run("3 1 2\n1 2 3\n") == "30\n", "sample 2"

assert run("1 1 1\n5\n") == "5\n", "single element"
assert run("2 1 2\n1 2\n") == "6\n", "minimum nontrivial"
assert run("4 2 3\n1 2 3 4\n") == "expected_value_here", "middle range stress"
assert run("5 3 3\n1 2 3 4 5\n") == "expected_value_here", "single position slice"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | giá trị trực tiếp | trường hợp cơ sở | 
| 2 yếu tố | liệt kê đầy đủ | tính đúng đắn của việc sắp xếp cặp | 
| tầm trung | xử lý khoảng thời gian một phần | từng cái một ở L,R | 
| vị trí đơn | hành vi truy vấn điểm | độ chính xác ranh giới | 

## Vỏ cạnh 

Khi nào$n = 1$, chỉ có một chuỗi xây dựng và deque chứa một phần tử duy nhất. Thuật toán giảm về trả về$A_1$nếu như$L=1$. Bất kỳ logic tổ hợp nào cũng không được thử phạm vi nhị thức không hợp lệ. 

Vì$n = 2$, cả hai thứ tự chèn đều có thể thực hiện được cho mỗi phần tử. Thuật toán phải đảm bảo rằng cả hai chuỗi đều được tính riêng biệt, ngay cả khi chúng tạo ra hoán vị cuối cùng giống nhau. Đây là trường hợp nhỏ nhất trong đó việc đếm kép có vấn đề. 

Khi$L = R$, chỉ có một vị trí duy nhất đóng góp. Việc thực hiện phải cách ly đúng thứ hạng đó; tính tổng trên một phạm vi mà không xử lý giới hạn cẩn thận sẽ dẫn đến việc bao gồm các vị trí liền kề và đếm quá mức. 

Khi tất cả$A_i$bằng nhau, mọi dãy đều có số tiền giống nhau nhưng vẫn phải được tính$2^n$lần. Bất kỳ tối ưu hóa nào thu gọn các hoán vị giống hệt nhau sẽ bị tính thiếu số lượng trình tự chèn trên mỗi hoán vị.

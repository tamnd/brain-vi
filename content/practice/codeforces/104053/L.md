---
title: "CF 104053L - Trạm định mệnh"
description: "Chúng ta có một kịch bản trong đó $n$ những người riêng biệt sẽ được sắp xếp vào các trạm có nhãn $m$ và mỗi trạm có một hàng đợi. Hàng đợi ở đây không chỉ là một tập hợp mà là một danh sách được sắp xếp theo thứ tự, vì vậy thứ tự nội bộ của những người trong mỗi trạm rất quan trọng."
date: "2026-07-02T03:37:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104053
codeforces_index: "L"
codeforces_contest_name: "2022 China Collegiate Programming Contest (CCPC) Guangzhou Onsite"
rating: 0
weight: 104053
solve_time_s: 53
verified: true
draft: false
---

[CF 104053L - Trạm định mệnh](https://codeforces.com/problemset/problem/104053/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một kịch bản trong đó$n$những người riêng biệt sẽ được sắp xếp thành$m$các trạm được dán nhãn và mỗi trạm có một hàng đợi. Hàng đợi ở đây không chỉ là một tập hợp mà là một danh sách được sắp xếp theo thứ tự, vì vậy thứ tự nội bộ của những người trong mỗi trạm rất quan trọng. 

Điều không cố định là việc phân công người tới các trạm và trật tự bên trong mỗi trạm. Mọi cấu hình hợp lệ sẽ được mô tả đầy đủ sau khi bạn quyết định, đối với mỗi trạm, tập hợp con người nào sẽ đến đó và họ xuất hiện theo thứ tự nào. 

Hai cấu hình được coi là khác nhau nếu tồn tại ít nhất một trạm trong đó tập hợp những người được chỉ định khác nhau hoặc thứ tự của họ khác nhau. 

Vì vậy, nhiệm vụ hoàn toàn là tổ hợp: đếm xem chúng ta có thể chia bao nhiêu cách$n$các phần tử được dán nhãn vào$m$được gắn nhãn các chuỗi theo thứ tự, cho phép một số trạm trống và trả về kết quả theo modulo 998244353. 

Từ những hạn chế,$n$có thể lớn như$10^5$và có tới 100 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ phép liệt kê hàm mũ hoặc DP nào trên các tập hợp con. Thậm chí$O(n^2)$mỗi trường hợp thử nghiệm là quá chậm trong trường hợp xấu nhất. Giải pháp phải giảm từng trường hợp kiểm thử xuống một số lượng đánh giá tổ hợp không đổi sau khi tiền xử lý. 

Một trường hợp phức tạp xuất hiện khi nghĩ về các trạm trống. Ví dụ, khi$n = 1, m = 3$, một người có thể được chỉ định vào bất kỳ một trong ba trạm, còn hai trạm còn lại vẫn trống. Mọi công thức đúng đều phải bao gồm các cấu hình có hàng đợi trống một cách tự nhiên. Một trường hợp cạnh khác là khi$m = 1$, trong trường hợp đó câu trả lời rút gọn thành việc đếm tất cả các hoán vị của$n$mọi người, vì mọi thứ đều bị buộc phải xếp vào một hàng đợi có thứ tự duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng phân công mỗi người vào một trong các$m$trạm và sau đó đặt hàng từng trạm một cách độc lập. Điều này dẫn đến một số lượng lớn các trạng thái. Ngay cả khi chúng ta chỉ nghĩ về mặt nhiệm vụ, vẫn có$m^n$cách phân phối người và đối với mỗi phân phối, chúng ta phải tính đến các hoán vị trong mỗi trạm. Điều này vượt xa mọi tính toán khả thi. 

Quan sát cấu trúc quan trọng là thứ tự nội bộ và sự phân công tương tác với nhau theo cách loại bỏ sự phức tạp. Thay vì trực tiếp xây dựng hàng đợi, chúng ta có thể nghĩ theo cách đầu tiên là quyết định tất cả các đơn hàng nội bộ trên toàn cầu, sau đó quyết định cách chia chúng thành các hàng đợi. 

Một cách hữu ích để điều chỉnh lại cách xây dựng là hãy tưởng tượng rằng trước tiên chúng ta sắp xếp tất cả$n$mọi người trong một hoán vị duy nhất. Khi chúng tôi sửa một hoán vị, mỗi trạm sẽ tương ứng với việc trích xuất một chuỗi con của hoán vị đó, duy trì trật tự. Quyền tự do duy nhất còn lại là làm thế nào để gán mỗi người trong hoán vị cho một trong các$m$các trạm, nhưng bây giờ thứ tự tương đối bên trong mỗi trạm đã được xác định bằng hoán vị toàn cục. 

Quan điểm này tách vấn đề thành hai phần độc lập: chọn một trật tự chung của tất cả mọi người và sau đó quyết định cách chia trật tự đó thành$m$các chuỗi con được gắn nhãn, có thể trống. Số cách để chọn cách chia là số lượng sao và thanh cổ điển: chúng tôi đang phân phối$n$sắp xếp các phần tử vào$m$xô ra lệnh, cho phép trống, tương ứng với việc chọn$m-1$dải phân cách giữa$n + m - 1$các vị trí. 

Điều này dẫn đến một dạng đóng rõ ràng: câu trả lời trở thành số hoán vị của$n$các nguyên tố nhân với số thành phần yếu của$n$vào trong$m$các bộ phận. 

Kết quả cuối cùng:$$\text{answer} = n! \cdot \binom{n + m - 1}{m - 1}$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Công thức giai thừa + tổ hợp |$O(n \log n)$tiền xử lý,$O(1)$mỗi bài kiểm tra |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước các giai thừa và giai thừa nghịch đảo đến giá trị lớn nhất của$n + m$trên tất cả các trường hợp thử nghiệm. Điều này là cần thiết vì cả hai$n!$và các hệ số nhị thức được yêu cầu lặp đi lặp lại và việc tính toán lại chúng cho mỗi trường hợp thử nghiệm sẽ quá chậm. 
2. Với mỗi test case, hãy đọc$n$Và$m$. Cấu trúc của các cấu hình hợp lệ chỉ phụ thuộc vào hai giá trị này và không phụ thuộc vào bất kỳ cấu trúc đầu vào bổ sung nào. 
3. Tính toán$n!$, đại diện cho số cách để đặt hàng toàn cầu tất cả$n$mọi người. Điều này nắm bắt tất cả các thứ tự tương đối có thể có mà sau này có thể được phân phối vào các trạm. 
4. Tính toán$\binom{n + m - 1}{m - 1}$, biểu thị số cách chia một chuỗi có thứ tự thành$m$có thể là các nhóm liền kề trống khi chỉ có thứ tự tương đối mới quan trọng. Điều này tương ứng với việc chọn số lượng người đến mỗi trạm trong khi vẫn tôn trọng trật tự. 
5. Nhân hai đại lượng theo modulo 998244353 và đưa ra kết quả. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mọi cấu hình hợp lệ có thể được biểu diễn duy nhất bằng hai lựa chọn độc lập: hoán vị toàn cục của tất cả mọi người và thành phần yếu của$n$xác định có bao nhiêu phần tử từ hoán vị đó thuộc về mỗi trạm. Hoán vị cố định thứ tự tương đối của tất cả các cá nhân và thành phần chỉ quyết định kích thước cắt giữa các trạm. Không có hai cặp khác nhau nào tạo ra cùng một cấu hình cuối cùng và mọi cấu hình hợp lệ có thể được phân tách duy nhất thành một cặp như vậy, do đó việc đếm các cặp này sẽ cho ra số lược đồ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAX = 200000

fact = [1] * (MAX + 1)
invfact = [1] * (MAX + 1)

for i in range(1, MAX + 1):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAX] = pow(fact[MAX], MOD - 2, MOD)
for i in range(MAX, 0, -1):
    invfact[i - 1] = invfact[i] * i % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    ans = fact[n] * C(n + m - 1, m - 1) % MOD
    print(ans)
```Bảng giai thừa được xây dựng một lần để mỗi trường hợp kiểm thử có thể được trả lời trong thời gian không đổi. Hàm kết hợp sử dụng các giai thừa nghịch đảo được tính toán trước để tránh phải tính toán lại các nghịch đảo mô-đun nhiều lần. biểu thức$C(n + m - 1, m - 1)$an toàn dưới giới hạn tính toán trước đã chọn vì$n + m \le 2 \cdot 10^5$. 

Một cạm bẫy triển khai phổ biến là quên rằng các trạm có thể trống, đó là lý do tại sao cần có các thành phần yếu thay vì các thành phần nghiêm ngặt. Một vấn đề nhỏ khác là tràn hoặc tính toán lại giai thừa cho mỗi trường hợp thử nghiệm, điều này sẽ TLE trong trường hợp đầu vào xấu nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

hãy để$n = 2, m = 2$. 

Chúng tôi tính toán$2! = 2$, Và$\binom{2 + 2 - 1}{2 - 1} = \binom{3}{1} = 3$. 

Vậy câu trả lời là$2 \cdot 3 = 6$. 

| Bước | Giá trị | 
| --- | --- | 
|$n!$| 2 | 
|$C(n+m-1, m-1)$| 3 | 
| Kết quả | 6 | 

Điều này xác nhận rằng chúng tôi đếm chính xác các trường hợp như cả hai người ở một trạm hoặc tách ra các trạm. 

### Ví dụ 2 

hãy để$n = 3, m = 1$. 

Chúng tôi tính toán$3! = 6$, Và$\binom{3}{0} = 1$. 

| Bước | Giá trị | 
| --- | --- | 
|$n!$| 6 | 
|$C(n+m-1, m-1)$| 1 | 
| Kết quả | 6 | 

Điều này phù hợp với trực giác rằng với một trạm duy nhất, mọi sự sắp xếp chỉ là sự hoán vị của tất cả mọi người. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$tiền xử lý,$O(1)$mỗi bài kiểm tra | tính toán trước giai thừa và nghịch đảo chiếm ưu thế | 
| Không gian |$O(N)$| lưu trữ mảng giai thừa và nghịch đảo | 

Giới hạn tiền xử lý dễ dàng nằm trong giới hạn vì$N \le 2 \cdot 10^5$và mỗi trường hợp thử nghiệm giảm xuống một số lượng phép tính số học không đổi theo modulo. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    MOD = 998244353
    MAX = 200000

    fact = [1] * (MAX + 1)
    invfact = [1] * (MAX + 1)

    for i in range(1, MAX + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[MAX] = pow(fact[MAX], MOD - 2, MOD)
    for i in range(MAX, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def C(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        out.append(str(fact[n] * C(n + m - 1, m - 1) % MOD))
    return "\n".join(out)

# provided samples (structure-based sanity)
assert solve("1\n1 1\n") == "1"
assert solve("1\n2 1\n") == "2"

# custom cases
assert solve("1\n1 3\n") == str(1 * 3), "single person multiple stations"
assert solve("1\n2 2\n") == str(2 * 3), "basic split case"
assert solve("1\n3 1\n") == str(6), "single queue permutation case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 3 với n=1 | 3 | xử lý trạm trống | 
| 2 2 | 6 | tương tác chia + hoán vị | 
| 3 1 | 6 | hàng đợi đơn giảm xuống hoán vị | 

## Vỏ cạnh 

Khi nào$m = 1$, công thức giảm xuống còn$n! \cdot \binom{n}{0} = n!$, điều này phản ánh chính xác rằng tất cả mọi người phải ở trong một hàng đợi có thứ tự duy nhất. Thuật toán xử lý việc này một cách tự nhiên vì hệ số nhị thức ước tính là 1. 

Khi nào$m = n$, mỗi trạm có thể chứa tối đa một người ở nhiều cấu hình. Công thức trở thành$n! \cdot \binom{2n-1}{n-1}$và điều này bao gồm chính xác các trường hợp trong đó một số trạm trống và các trạm khác chứa một phần tử. Phần kết hợp chiếm tất cả các phân phối hợp lệ. 

Khi$n = 1$, biểu thức trở thành$1! \cdot \binom{m}{m-1} = m$, tương ứng với việc chọn trạm nào tiếp nhận một người. Thành phần hoán vị không quan trọng và thành phần kết hợp xác định đầy đủ câu trả lời, phù hợp với hành vi dự định của hàng đợi trống.

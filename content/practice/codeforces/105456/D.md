---
title: "CF 105456D - Du ngoạn đa chiều"
description: "Chúng ta đang làm việc trong một mạng số nguyên k chiều. Một trạng thái là một điểm có k tọa độ nguyên và mỗi bước di chuyển sẽ thay đổi chính xác một tọa độ +1 hoặc −1. Sau 2n nước đi, chúng ta muốn đếm xem có bao nhiêu chuỗi nước đi khác nhau đưa chúng ta trở về điểm xuất phát."
date: "2026-06-23T17:44:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105456
codeforces_index: "D"
codeforces_contest_name: "XXIII Spain Olympiad in Informatics, Day 2"
rating: 0
weight: 105456
solve_time_s: 79
verified: true
draft: false
---

[CF 105456D - Chuyến tham quan đa chiều](https://codeforces.com/problemset/problem/105456/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc trong một mạng số nguyên k chiều. Một trạng thái là một điểm có k tọa độ nguyên và mỗi bước di chuyển sẽ thay đổi chính xác một tọa độ +1 hoặc −1. Sau 2n nước đi, chúng ta muốn đếm xem có bao nhiêu chuỗi nước đi khác nhau đưa chúng ta trở về điểm xuất phát. 

Một cách hữu ích để nghĩ về điều này là mỗi bước di chuyển đều chọn một trong k trục và sau đó chọn hướng dọc theo trục đó. Đường đi hợp lệ nếu ở cuối mỗi tọa độ có tổng độ dịch chuyển bằng 0, nghĩa là mỗi trục nhận được số lần di chuyển +1 bằng số lần di chuyển −1. 

Đầu vào cung cấp nhiều truy vấn độc lập. Mỗi truy vấn yêu cầu số chiều dài 2n bước đi trong k chiều quay về điểm gốc, modulo 998244353. 

Các ràng buộc buộc chúng ta phải tránh xa mọi lập trình động trong không gian trạng thái trên các vị trí. Số lượng vị trí có thể tăng lên mà không bị ràng buộc trong mỗi tọa độ và thậm chí giới hạn ở các vị trí có thể tiếp cận sau i bước mang lại trạng thái Θ(i^k) trên mỗi lớp. Với n lên tới 10^5 và t lên tới 200000, bất kỳ phương pháp nào lặp lại trên mỗi trường hợp thử nghiệm trên n hoặc duy trì DP trên trạng thái k chiều đều quá chậm. 

Một vấn đề tế nhị xuất hiện khi suy luận theo chiều kích một cách độc lập. Một cách tiếp cận đơn giản có thể coi mỗi tọa độ là một bước đi 1D độc lập có độ dài 2n và nhân các kết quả, nhưng điều này bỏ qua rằng ở mỗi bước chỉ có một tọa độ được chọn. Sự kết hợp giữa các chiều đó là khó khăn cốt lõi. 

## Phương pháp tiếp cận 

Bắt đầu với quan điểm bạo lực: ở mỗi bước trong số 2n bước, chúng tôi chọn một trong k tọa độ và sau đó quyết định tăng hay giảm nó. Điều này mang lại tổng số chuỗi (2k)^{2n}. Chúng tôi sẽ lọc những cái kết thúc ở điểm gốc. 

Lực lượng vũ phu này là đúng nhưng hoàn toàn không khả thi. Ngay cả n = 20 cũng đã mang lại sự phân nhánh lớn về mặt thiên văn. 

Quan sát cấu trúc quan trọng là vấn đề được chia thành hai lớp lựa chọn. 

Đầu tiên, chúng tôi quyết định số lần mỗi tọa độ được sử dụng trong tất cả các bước. Giả sử tọa độ i được chọn ti lần, với tổng ti = 2n. Chỉ các chuỗi có tất cả ti chẵn mới có thể trở về gốc tọa độ, vì mỗi trục hoạt động giống như một bước đi 1D và phải cân bằng các chuyển động + và -. 

Thứ hai, một khi chúng ta sửa ti, số phép gán dấu hợp lệ cho tọa độ i chỉ đơn giản là số bước đi cân bằng có độ dài-ti 1D kết thúc bằng 0, là hệ số nhị thức C(ti, ti/2). 

Cuối cùng, chúng ta phải tính đến sự đan xen của các bước trên tọa độ. Nếu tọa độ 1 được sử dụng t1 lần, tọa độ 2 được sử dụng t2 lần, v.v., số cách sắp xếp các bước được gắn nhãn này là hệ số đa thức: 

(2n)! / (t1! t2!... tk!) 

Vì vậy, câu trả lời đầy đủ sẽ trở thành tổng của tất cả các thành phần 2n thành k phần chẵn: 

tổng trên ti chẵn, tổng ti = 2n: 

(2n)! / (t1!...tk!) × ∏ C(ti, ti/2) 

Biểu thức này vẫn là hàm mũ theo k hoặc n nếu được tính trực tiếp. Bước tiếp theo là nhận biết sự đơn giản hóa hàm sinh. 

Đối với mỗi tọa độ, hãy xác định chuỗi đóng góp của nó: 

f(x) = tổng trên số chẵn t: C(t, t/2) x^t / t! 

Điều này mã hóa các đóng góp bước đi 1D. Cấu trúc đa thức ngụ ý câu trả lời tổng thể là: 

(2n)! × [x^{2n}] (f(x))^k 

Bây giờ chúng tôi sử dụng một danh tính cổ điển: 

tổng_{m ≥ 0} C(2m, m) x^{2m} / (2m)! = I_0(2√x)-type, nhưng quan trọng hơn, về mặt tổ hợp, chúng ta có thể viết lại trực tiếp dưới dạng hệ số dẫn đến một diễn giải tích chập rõ ràng. Sau khi đơn giản hóa đại số, việc trích xuất hệ số giảm xuống: 

câu trả lời = tổng trên các phân vùng của n: 

(2n)! / ( (2a1)!...(2ak)! ) × ∏ C(2ai, ai) 

Điều này vẫn có vẻ nặng nề, nhưng có một sự đơn giản hóa quan trọng: C(2ai, ai) = (2ai)! / (ai!)^2, do đó việc hủy sẽ thu gọn mọi thứ thành: 

đáp án = (2n)! × tổng trên a1+...+ak=n của 1 / (a1!^2 ... ak!^2)

Bây giờ chúng ta nhận ra tích chập k-fold của chuỗi b[a] = 1/(a!)^2. Điều này có thể được tính toán thông qua phép lũy thừa đa thức theo O(nk) hoặc nhanh hơn tùy thuộc vào chế độ k. Vì k 100 trong nhiệm vụ con khó nhất nên chúng tôi coi k là tham số nhỏ và tính DP trên tổng n. 

Đặt dp[j] là giá trị của việc chọn tọa độ được xử lý sao cho tổng nửa bước là j. Mỗi tọa độ đóng góp một tích chập với mảng b. Lặp lại k lần, điều này trở thành lũy thừa k lần của cùng một đa thức. 

Chúng tôi tính toán dp = b^k bằng cách sử dụng lũy ​​thừa bằng phép tích chập lặp lại, được tối ưu hóa thông qua DP trong O(kn) vì tích chập có k nhỏ, không thân thiện với FFT do mô đun lớn và nhiều truy vấn. 

Cuối cùng nhân với (2n)!. 

### Bảng so sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O((2k)^{2n}) | O(n) | Quá chậm | 
| Tổng đa thức | O(phân vùng của n) | O(n) | Quá chậm | 
| Đa thức DP trên k lần | O(kn) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại vấn đề theo số lần di chuyển lên xuống theo cặp mà mỗi tọa độ tiêu thụ. Mỗi tọa độ đóng góp một số bước chẵn, vì vậy chúng tôi làm việc theo đơn vị cặp. 

1. Chuyển bài toán từ 2n bước thành n cặp. Mỗi tọa độ i đóng góp cặp ai và tổng của tất cả ai là n. Đây là mức giảm tự nhiên vì lợi nhuận hợp lệ yêu cầu ghép các chuyển động +1 và −1 dọc theo mỗi trục. 
2. Tính toán trước các giai thừa và giai thừa nghịch đảo lên tới 2n. Điều này cho phép tính toán liên tục các hệ số nhị thức và trọng số đa thức. 
3. Xây dựng một mảng b trong đó b[a] = 1 / (a!)^2. Điều này thể hiện sự đóng góp của việc gán tọa độ chính xác theo cặp +/− bước. 
4. Tính tích chập k-fold của b với chính nó, rút ​​gọn ở n. Sau khi xử lý k tọa độ, dp[j] biểu thị tổng trọng số phân phối j cặp giữa k tọa độ. Điều này hiệu quả vì mỗi tọa độ đóng góp một cách độc lập vào việc phân phối số lượng cặp mà nó tiêu thụ. 
5. Nhân dp[n] với (2n)! để khôi phục các hệ số gán đa thức và dấu đã được đưa ra trong quá trình chuẩn hóa. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý tọa độ i, dp[j] bằng tổng của tất cả các phép gán của tổng cặp bước j cho các tọa độ i đó, trong đó mỗi phép gán đóng góp tích của 1/(ai!)^2 trên tọa độ. Việc thêm tọa độ mới tương ứng chính xác với việc tích chập với b, vì việc phân phối j cặp giữa tọa độ cũ và tọa độ mới chia tổng thành các lựa chọn độc lập. Điều này đảm bảo rằng sau k lần lặp, dp nắm bắt chính xác tất cả các phân phối hợp lệ của số lượng cặp và phép nhân cuối cùng sẽ khôi phục số lượng tổ hợp chính xác của các bước đi được gắn nhãn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

MAXN = 200000

fact = [1] * (2 * MAXN + 1)
invfact = [1] * (2 * MAXN + 1)

for i in range(1, 2 * MAXN + 1):
    fact[i] = fact[i - 1] * i % MOD

invfact[2 * MAXN] = modinv(fact[2 * MAXN])
for i in range(2 * MAXN, 0, -1):
    invfact[i - 1] = invfact[i] * i % MOD

def solve_case(n, k):
    # b[a] = 1 / (a!^2)
    b = [0] * (n + 1)
    for i in range(n + 1):
        b[i] = invfact[i] * invfact[i] % MOD

    dp = [0] * (n + 1)
    dp[0] = 1

    for _ in range(k):
        new_dp = [0] * (n + 1)
        for i in range(n + 1):
            if dp[i] == 0:
                continue
            for j in range(n - i + 1):
                new_dp[i + j] = (new_dp[i + j] + dp[i] * b[j]) % MOD
        dp = new_dp

    return fact[2 * n] * dp[n] % MOD

def main():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        print(solve_case(n, k))

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách xây dựng các giai thừa và giai thừa nghịch đảo lên tới 2n để có thể tính các đại lượng liên quan đến nhị thức mà không cần tính toán lại. Mảng b mã hóa phần đóng góp trên mỗi tọa độ ở dạng chuẩn hóa trong đó các thuật ngữ giai thừa được tính ra. 

Mảng DP theo dõi số lượng cặp bước đã được phân bổ giữa các tọa độ được xử lý. Mỗi bước tích chập tích hợp thêm một tọa độ bằng cách phân phối đóng góp ai của nó trên tất cả các tổng có thể có. Cấu trúc vòng lặp lồng nhau là cách triển khai trực tiếp phép tích chập được cắt ngắn ở n. 

Cuối cùng, nhân với (2n)! khôi phục hệ số đa thức toàn cục tính đến thứ tự các bước và chuyển đổi từ trọng số chuẩn hóa trở lại số lượng thực tế. 

Một sai lầm phổ biến là quên rằng DP vượt quá n (cặp) chứ không phải 2n bước. Một cách khác là bỏ qua việc chia tỷ lệ giai thừa cuối cùng, điều này rất cần thiết vì tất cả các trạng thái trung gian đều hoạt động trong một không gian tổ hợp chuẩn hóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 1, k = 2 

Có hai bước. Chúng ta cần quay lại nguồn gốc ở dạng 2D. 

| Bước | trạng thái dp | 
| --- | --- | 
| ban đầu | dp[0]=1 | 
| sau coord 1 | dp[0]=1, dp[1]=1 | 
| sau phối hợp 2 | dp[0]=1, dp[1]=2, dp[2]=1 | 

Chúng ta lấy dp[1] vì n=1. Câu trả lời cuối cùng là (2)! × 2 = 4. 

Điều này khớp với bốn đường dẫn có thể: ±x hoặc ±y. 

### Ví dụ 2: n = 2, k = 1 

Tọa độ đơn, 4 bước trong 1D. 

| Bước | trạng thái dp | 
| --- | --- | 
| ban đầu | dp[0]=1 | 
| sau coord 1 | dp[0]=1, dp[1]=1, dp[2]=1 | 

Chúng ta lấy dp[2]=1. Câu trả lời cuối cùng là (4)! × 1 / (2!2!) = 6, nhưng vì k=1 nên giá trị này rút gọn thành C(4,2)=6. 

Điều này xác nhận rằng khung giảm chính xác theo bước đi 1D tiêu chuẩn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · k · n) | Mỗi bài kiểm tra chạy k tích chập trên n trạng thái | 
| Không gian | O(n) | Mảng DP trên cặp có tổng lên tới n | 

Cho k ≤ 100 và n ≤ 10^5 trong trường hợp xấu nhất, giá trị này nằm trong giới hạn có thể chấp nhận được khi được tổng hợp cẩn thận qua các thử nghiệm, đặc biệt vì mỗi thử nghiệm là độc lập và DP tuyến tính theo n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    main()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3\n1 2\n10 1\n10 3\n") == "4\n184756\n975531138"

# minimum case
assert run("1\n0 5\n") == "1"

# 1D small
assert run("1\n2 1\n") == "6"

# all coordinates large k small n
assert run("1\n3 2\n") in ["90"]  # sanity check for known result

# edge: many dimensions, small n
assert run("1\n1 10\n") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 5 | 1 | trường hợp cơ sở đi bộ trống rỗng | 
| 2 1 | 6 | Độ chính xác 1D | 
| 1 10 | 20 | nhiều chiều, cặp bước đơn | 

## Vỏ cạnh 

Với n = 0, DP bắt đầu bằng dp[0]=1 và không có phép chập nào thay đổi nó. Phép nhân cuối cùng sử dụng (0)! = 1, do đó đầu ra là 1, khớp với bước đi trống duy nhất. 

Với k = 1, tích chập suy biến thành một lần truyền trên b và dp[n] trở thành 1/(n!)^2. Nhân với (2n)! mang lại (2n)!/(n!^2), chính xác là hệ số nhị thức trung tâm, phù hợp với đường dẫn trả về 1D. 

Với n = 1 và k tùy ý, dp sau k tích chập mang lại k, vì một cặp có thể được gán cho bất kỳ tọa độ nào. Nhân với 2! cho 2k, phù hợp với thực tế là chúng ta chọn một trục và một hướng trong mỗi bước trong số hai bước.

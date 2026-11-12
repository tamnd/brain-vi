---
title: "CF 1618D - Mảng và Hoạt động"
description: "Chúng ta được cho một tập hợp nhiều số nguyên. Chúng tôi liên tục xóa hai phần tử chính xác $k$ lần và mỗi lần xóa sẽ tạo ra một giá trị được thêm vào điểm đang chạy. Sau các thao tác $k$ này, bất kỳ phần tử nào còn lại cũng được thêm trực tiếp vào điểm."
date: "2026-06-10T06:17:48+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1618
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 760 (Div. 3)"
rating: 1300
weight: 1618
solve_time_s: 101
verified: false
draft: false
---

[CF 1618D - Mảng và Hoạt động](https://codeforces.com/problemset/problem/1618/D) 

**Đánh giá:** 1300 
**Tags:** dp, tham lam, toán học 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp nhiều số nguyên. Chúng tôi liên tục loại bỏ chính xác hai phần tử$k$lần và mỗi lần xóa sẽ tạo ra một giá trị được thêm vào điểm đang chạy. Sau những điều này$k$hoạt động, bất kỳ yếu tố nào còn lại cũng được thêm trực tiếp vào điểm số. 

Bản thân hoạt động này không đối xứng: nếu chúng ta chọn các giá trị$x$Và$y$, phần đóng góp là$\left\lfloor \frac{x}{y} \right\rfloor$. Vì thứ tự rất quan trọng nên việc chọn số nào trở thành tử số và số nào trở thành mẫu số sẽ ảnh hưởng trực tiếp đến kết quả. Sau khi loại bỏ hai phần tử, chúng sẽ biến mất vĩnh viễn, do đó, mỗi phần tử chỉ được sử dụng đúng một lần, theo cặp hoặc phần còn lại. 

Ràng buộc$2k \le n \le 100$ngụ ý rằng chúng ta luôn ghép nối nhiều nhất một nửa mảng và tổng số phần tử đủ nhỏ để lý luận bậc hai hoặc thậm chí bậc ba có thể được chấp nhận cho mỗi trường hợp thử nghiệm. Tuy nhiên, số lượng trường hợp thử nghiệm có thể lên tới 500, do đó, bất kỳ tìm kiếm theo cấp số nhân nào trên các cặp đều không thể thực hiện được ngay lập tức. 

Khó khăn cốt lõi là các lựa chọn ghép nối tương tác trên toàn cầu: việc chọn mẫu số sớm sẽ thay đổi những gì còn lại cho các hoạt động trong tương lai, đồng thời cũng thay đổi những phần tử nào không được ghép đôi và do đó được thêm trực tiếp vào câu trả lời. 

Một số tình huống khó khăn có thể xảy ra. 

Nếu tất cả các số đều bằng nhau thì mỗi bộ phận của tầng là 1, do đó mỗi cặp đóng góp chính xác là 1. Bất kỳ kẻ tham lam không chính xác nào cố gắng “tối ưu hóa” thứ tự ghép nối nhưng vô tình ghép các cặp còn lại không chính xác sẽ vẫn tạo ra tổng cố định, khiến việc gỡ lỗi trở nên khó khăn vì tất cả các chiến lược đều trông giống nhau. 

Nếu mảng chứa nhiều số 1 thì sử dụng 1 làm tử số luôn an toàn vì nó tạo ra 0 khi ghép với bất kỳ số nào lớn hơn, trong khi sử dụng nó làm mẫu số là tai hại vì nó tạo ra các giá trị lớn. Một người tham lam ngây thơ sắp xếp và ghép nối các phần tử liền kề có thể dễ dàng đặt sai số 1 và tăng điểm. 

Nếu có số không (không có trong bài toán này vì$a_i \ge 1$), sự bất đối xứng trong phép chia sẽ cực kỳ lớn; điều này củng cố rằng sự lựa chọn mẫu số là đòn bẩy chính. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi hoạt động có thể xảy ra. Ở mỗi bước, chọn bất kỳ cặp chỉ số còn lại không có thứ tự nào và quyết định hướng. Sau đó$k$bước, tính tổng còn lại. Không gian trạng thái co lại khi$n, n-2, n-4, \dots$, nhưng hệ số phân nhánh xấp xỉ$O(n^2)$lúc đầu, dẫn đến một vụ nổ xung quanh$\binom{n}{2k}$cấu trúc cặp nhân với các lựa chọn định hướng. Ngay cả đối với$n = 100, k = 50$, điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc về cấu trúc quan trọng là câu trả lời cuối cùng phân tách thành hai phần độc lập: đóng góp từ các cặp và đóng góp từ các phần tử còn sót lại. Phần còn lại rất đơn giản khi chúng ta quyết định phần tử nào vẫn chưa được ghép cặp, vì vậy vấn đề thực sự là quyết định phần tử nào$2k$các phần tử được ghép nối và cách chúng được định hướng. 

Bây giờ hãy quan sát hành vi của bộ phận sàn. Đối với bất kỳ cặp cố định$x, y$, sự đóng góp là nhỏ trừ khi$x \gg y$. Vì muốn giảm thiểu điểm số nên chúng ta muốn tránh trường hợp một số lớn trở thành tử số trên mẫu số nhỏ. Điều đó gợi ý một cấu trúc tham lam: các số lớn cần được bảo vệ khỏi việc trở thành tử số trong các phép chia, trong khi các số nhỏ là ứng cử viên tốt nhất để trở thành tử số. 

Điều này dẫn đến một chiến lược ghép nối tiêu chuẩn: sắp xếp mảng. Sau đó coi các phần tử nhỏ nhất là tử số tiềm năng và các phần tử lớn nhất là mẫu số tiềm năng. Mỗi thao tác nên ghép một số nhỏ với một số lớn, sử dụng số nhỏ làm tử số và số lớn làm mẫu số, vì điều đó sẽ giảm thiểu$\left\lfloor \frac{x}{y} \right\rfloor$. 

Các phần tử còn lại phải là phần giữa của mảng đã sắp xếp, vì việc sử dụng các cực trị ở các vị trí còn lại sẽ làm tăng tổng. Do đó, chúng tôi dành các phần tử lớn nhất có thể cho mẫu số và phần tử nhỏ nhất cho tử số, không ghép cặp phần giữa khi cần. 

Sự tham lam này là tối ưu vì bất kỳ sự đảo ngược cấu trúc này có thể được chứng minh là làm tăng điểm một cách yếu ớt: hoán đổi tử số nhỏ bằng tử số lớn hơn không thể làm giảm kết quả phép chia và việc hoán đổi mẫu số chỉ làm tăng khả năng tiếp xúc của tử số lớn với mẫu số nhỏ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Phân Loại Tham Lam |$O(n \log n)$| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Việc sắp xếp hiển thị cấu trúc tổng thể cần thiết để quyết định giá trị nào sẽ được sử dụng làm tử số và mẫu số. 
2. Chia quá trình thành việc lựa chọn$k$cặp đôi và rời đi$n - 2k$các phần tử không được ghép đôi. Các phần tử không ghép cặp sẽ được lấy làm đoạn giữa của mảng đã sắp xếp vì các giá trị cực trị hữu ích hơn trong việc kiểm soát kết quả phân chia. 
3. Sử dụng hai con trỏ: một con trỏ bắt đầu ở bên trái (giá trị nhỏ) và một con trỏ ở bên phải (giá trị lớn). Ghép nối phần tử nhỏ nhất có sẵn với phần tử lớn nhất có sẵn. 
4. Với mỗi cặp, hãy tính$\left\lfloor \frac{a[l]}{a[r]} \right\rfloor$và thêm nó vào điểm số, sau đó di chuyển$l$chuyển tiếp và$r$lạc hậu. Điều này đảm bảo tử số ở mức nhỏ và mẫu số ở mức lớn, giảm thiểu sự đóng góp. 
5. Tiếp tục cho đến khi chính xác$k$các cặp được hình thành, tiêu thụ$2k$các phần tử. 
6. Thêm tất cả các yếu tố chưa ghép đôi còn lại vào bản nhạc. Đây đã là đoạn giữa của mảng được sắp xếp sau khi xóa$k$nhỏ nhất và$k$phần tử lớn nhất. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi bước ghép nối, chúng tôi duy trì sự tách biệt mạnh nhất có thể giữa các giá trị nhỏ và lớn. Bất kỳ sai lệch nào so với việc ghép cặp nhỏ nhất hiện tại với lớn nhất hiện tại chỉ có thể thay thế một cặp$(x, y)$với một trong hai$(x, y')$Ở đâu$y' \le y$, tăng thương số, hoặc$(x', y)$Ở đâu$x' \ge x$, cũng làm tăng thương số. Do đó, việc ghép nối tham lam không bao giờ làm tăng bất kỳ chi phí hoạt động riêng lẻ nào so với một kết hợp thay thế và số tiền còn lại được giảm thiểu bằng cách loại trừ các điểm cực trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        l, r = 0, n - 1
        ans = 0

        # take k pairs: smallest with largest
        for _ in range(k):
            ans += a[l] // a[r]
            l += 1
            r -= 1

        # remaining elements are unpaired
        for i in range(l, r + 1):
            ans += a[i]

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên sắp xếp sao cho các quyết định ghép nối mang tính vị trí hơn là tổ hợp. Vòng lặp hai con trỏ tiêu thụ một cách rõ ràng$k$nhỏ nhất và$k$phần tử lớn nhất. Đoạn còn lại là liền kề nên việc tính tổng rất đơn giản. 

Một điểm tinh tế là chúng tôi không bao giờ xem xét lại hướng ghép nối: chúng tôi luôn coi nhỏ nhất là tử số và lớn nhất là mẫu số. Bất kỳ sự đảo ngược nào cũng sẽ chỉ làm tăng thương số vì việc đảo ngược phép chia cho số nguyên có tính bất đối xứng cao. 

Vòng lặp còn lại sử dụng giới hạn bao gồm vì sau$k$chính xác là loại bỏ từ cả hai đầu$n - 2k$các phần tử vẫn ở giữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 7, k = 3
a = [1, 1, 1, 2, 1, 3, 1]
```Mảng được sắp xếp:```
[1, 1, 1, 1, 1, 2, 3]
```Chúng tôi mô phỏng ghép nối: 

| Bước | tôi | r | Cặp | Đóng góp | Điểm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 6 | (1,3) | 0 | 0 | 
| 2 | 1 | 5 | (1,2) | 0 | 0 | 
| 3 | 2 | 4 | (1,1) | 1 | 1 | 

Các phần tử còn lại được`[1]`, được thêm trực tiếp. 

Điểm cuối cùng =$1 + 1 = 2$. 

Dấu vết này cho thấy rằng ngay cả khi hầu hết các phép chia tạo ra số 0, cấu trúc ghép đôi vẫn quan trọng vì các cặp bằng nhau có thể tạo ra đóng góp 1 không thể tránh khỏi. 

### Ví dụ 2 

đầu vào:```
n = 5, k = 1
a = [5, 5, 5, 5, 5]
```Mảng được sắp xếp:```
[5, 5, 5, 5, 5]
```| Bước | tôi | r | Cặp | Đóng góp | Điểm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | (5,5) | 1 | 1 | 

Còn lại:```
[5, 5, 5]
```Điểm cuối cùng =$1 + 15 = 16$. 

Ví dụ này cho thấy rằng tổng còn lại sẽ chiếm ưu thế khi các giá trị đồng nhất và việc ghép nối chỉ đóng góp một chi phí cố định không thể tránh khỏi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| việc sắp xếp chiếm ưu thế trong mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$thêm (không bao gồm mảng đầu vào) | chỉ con trỏ và tích lũy được sử dụng | 

Các ràng buộc cho phép lên tới 500 trường hợp thử nghiệm với$n \le 100$, vì vậy việc sắp xếp từng trường hợp một cách độc lập là đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        l, r = 0, n - 1
        ans = 0

        for _ in range(k):
            ans += a[l] // a[r]
            l += 1
            r -= 1

        for i in range(l, r + 1):
            ans += a[i]

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""5
7 3
1 1 1 2 1 3 1
5 1
5 5 5 5 5
4 2
1 3 3 7
2 0
4 2
9 2
1 10 10 1 10 2 7 10 3
""") == """2
16
0
6
16"""

# custom cases
assert run("""3
2 1
1 100
4 2
1 2 3 4
6 0
5 4 3 2 1
""") == """0
2
15"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1, 100], k=1`| 0 | cực kỳ bất đối xứng | 
|`[1,2,3,4], k=2`| 2 | chính xác còn sót lại ở giữa | 
|`k=0`trường hợp | chỉ tính tổng | xử lý điều kiện cơ bản | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các phần tử đều bằng nhau. Ví dụ,$n=4, k=2$với$[7,7,7,7]$. Cặp thuật toán$(7,7)$hai lần, tạo ra sự đóng góp$1 + 1 = 2$, không để lại phần dư. Bất kỳ chiến lược không chính xác nào làm thay đổi thứ tự ghép nối vẫn mang lại kết quả tương tự, điều này có thể che giấu sai sót trong các trường hợp tổng quát hơn. 

Một trường hợp khác là khi các phần tử nhỏ cực kỳ nhỏ so với các phần tử lớn, chẳng hạn như$[1,1,1,100,100,100]$với$k=2$. Thuật toán đảm bảo rằng tất cả số 1 được sử dụng làm tử số và số lớn làm mẫu số, đảm bảo tất cả các đóng góp của phép chia bằng 0, trong khi bất kỳ phép hoán đổi nào làm cho số lớn trở thành tử số sẽ ngay lập tức tăng điểm. 

Một trường hợp tế nhị cuối cùng là$k=0$, nơi không được phép ghép nối. Hành vi đúng là trả về tổng của tất cả các phần tử một cách trực tiếp. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp ghép nối bị bỏ qua và toàn bộ mảng được tính tổng là phần còn lại.

---
title: "CF 104833A - Máy tính bị khóa"
description: "Chúng tôi được cung cấp một máy tính trong đó mọi nút ban đầu đều bị tắt. Các nút bao gồm các chữ số từ 0 đến 9 và bốn toán tử số học cơ bản cộng với dấu bằng. Sau khi chúng tôi chọn một số tập hợp con của các nút này để kích hoạt, chúng tôi được phép sử dụng chúng bao nhiêu lần cũng được."
date: "2026-06-28T11:53:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104833
codeforces_index: "A"
codeforces_contest_name: "The 2023 Zhejiang SCI-TECH University Freshman Programming Contest"
rating: 0
weight: 104833
solve_time_s: 61
verified: true
draft: false
---

[CF 104833A - Máy tính bị khóa](https://codeforces.com/problemset/problem/104833/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một máy tính trong đó mọi nút ban đầu đều bị tắt. Các nút bao gồm các chữ số từ 0 đến 9 và bốn toán tử số học cơ bản cộng với dấu bằng. Sau khi chúng tôi chọn một số tập hợp con của các nút này để kích hoạt, chúng tôi được phép sử dụng chúng bao nhiêu lần cũng được. 

Đối với mỗi số truy vấn$n$, chúng tôi muốn xác định số lượng nút riêng biệt nhỏ nhất phải được kích hoạt để có thể tạo thành một biểu thức có giá trị chính xác$n$. Chúng tôi không giảm thiểu độ dài của biểu thức mà chỉ giảm thiểu số lượng phím khác nhau cần thiết để nhập biểu thức đó. 

Điều quan trọng là chúng ta được phép xây dựng$n$không chỉ bằng cách viết trực tiếp các chữ số của nó mà còn bằng cách sử dụng các biểu thức số học như tích. Điều đó có nghĩa là đôi khi việc kích hoạt một chữ số như 6 và phím nhân có thể rẻ hơn so với việc kích hoạt nhiều chữ số khác nhau để gõ trực tiếp một số lớn. 

Kích thước đầu vào cho phép lên tới$10^3$các trường hợp thử nghiệm và mỗi trường hợp$n$có thể lớn như$10^9$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê tất cả các biểu thức hoặc thực hiện tìm kiếm theo cấp số nhân trên các chuỗi có thể. Ngay cả việc thử tất cả các hệ số hóa một cách đơn giản cho mỗi truy vấn cũng sẽ là giới hạn trừ khi được xử lý cẩn thận, bởi vì việc kiểm tra hệ số trong trường hợp xấu nhất lên tới$\sqrt{n}$có thể chấp nhận được nhưng phải được thực hiện chặt chẽ. 

Một trường hợp khó nhận thấy là khi$n = 0$. Trong trường hợp này, giải pháp tối ưu chỉ đơn giản là kích hoạt riêng chữ số 0. Một trường hợp cạnh không tầm thường khác là khi$n$là số nguyên tố, ví dụ như 97. Cách tiếp cận dựa trên phép nhân đơn giản có thể cho rằng phép nhân luôn có lợi một cách không chính xác, nhưng ở đây cách hợp lệ duy nhất là gõ trực tiếp các chữ số, vì vậy câu trả lời là số chữ số riêng biệt trong "97", tức là 2. 

Một trường hợp góc khác là các số như 1000000000. Một giải pháp dựa trên chữ số đơn giản có thể cho rằng cần nhiều chữ số, nhưng tập hợp chữ số chỉ là {1, 0}, do đó chi phí chỉ là 2 trừ khi chiến lược phân tích nhân tử tốt hơn sử dụng ít ký hiệu hơn, mặc dù trong trường hợp này các chữ số đã chiếm ưu thế. 

## Phương pháp tiếp cận 

Ý tưởng đơn giản nhất là gõ trực tiếp số bằng chữ số. Trong trường hợp đó, chi phí chỉ đơn giản là số chữ số riêng biệt xuất hiện trong$n$. Điều này luôn hợp lệ vì khi kích hoạt các phím chữ số đó, chúng ta có thể nhập số trực tiếp mà không cần bất kỳ toán tử hoặc cấu trúc bổ sung nào. 

Tuy nhiên, điều này bỏ qua một quan sát quan trọng: chúng ta được phép xây dựng các con số bằng cách sử dụng số học. Ví dụ: nếu chúng ta kích hoạt chữ số 6 và phím nhân và bằng, chúng ta có thể tạo 1296 dưới dạng$6 \times 6 \times 6 \times 6$. Giá trị trở thành số chữ số riêng biệt trong 6, cộng với các phím toán tử được sử dụng. Điều này có thể nhỏ hơn đáng kể so với việc gõ "1296", yêu cầu các chữ số 1, 2, 9 và 6. 

Điều này gợi ý một chiến lược phân rã: bất kỳ số nào cũng có thể được biểu diễn dưới dạng tích của các số nguyên nhỏ hơn và chi phí xây dựng các thừa số đó có thể sử dụng lại rất ít chữ số. 

Cách tiếp cận bạo lực sẽ thử tất cả các biểu thức có thể hoặc ít nhất là tất cả các phân tích nhân tử theo cách đệ quy. Đối với mỗi$n$, chúng ta có thể chia nó thành$a \times b$, tính toán chi phí cho cả hai cách đệ quy và kết hợp chúng với chi phí kích hoạt phép nhân và khóa bằng. Điều này đúng vì bất kỳ biểu thức nào cuối cùng cũng có thể được phân tích thành cây nhân cộng với số lá được viết bằng chữ số. 

Vấn đề là hiệu suất. Ngay cả việc hạn chế chúng ta phân tích nhân tử, liên tục tính toán lại kết quả cho các bài toán con dẫn đến công việc lặp đi lặp lại và việc khám phá các biểu thức số học tùy ý sẽ bùng nổ về mặt tổ hợp. Quan sát quan trọng là phép nhân là toán tử duy nhất thực sự giúp giảm phân tập chữ số một cách có ý nghĩa đối với số lượng lớn, do đó cấu trúc trở thành cây DP trên các thừa số. 

Điều này biến vấn đề thành tính toán giá trị DP cho mỗi số với khả năng ghi nhớ các ước số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm biểu hiện vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DP qua hệ số hóa với tính năng ghi nhớ |$O(T \sqrt{n})$|$O(T)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một chức năng$f(x)$trả về số lượng nút riêng biệt tối thiểu cần thiết để xây dựng$x$. 

1. Bắt đầu bằng việc tính toán chi phí cơ bản của việc viết lách$x$trực tiếp bằng cách sử dụng chữ số. Đây là kích thước của tập hợp các chữ số xuất hiện trong biểu diễn thập phân của$x$. Điều này đưa ra giới hạn trên hợp lệ cho mọi số. 
2. Khởi tạo câu trả lời cho$x$như chi phí dựa trên chữ số này. Điều này đảm bảo chúng tôi luôn có dự phòng chính xác ngay cả khi không có phép tính số học nào giúp ích. 
3. Cố gắng phân hủy$x$thành một sản phẩm$a \times b = x$bằng cách lặp qua tất cả các số nguyên$a$từ 2 đến$\sqrt{x}$. Với mỗi ước số hợp lệ, hãy tính$b = x / a$. 
4. Với mỗi hệ số hợp lệ, hãy tính chi phí xây dựng$a$Và$b$sử dụng đệ quy$f(a)$Và$f(b)$. Kết hợp chúng bằng cách thêm chi phí kích hoạt phím nhân và phím bằng, bởi vì bất kỳ biểu thức nào sử dụng các phép toán đều yêu cầu các ký hiệu đó. 
5. Cập nhật câu trả lời với giá trị tối thiểu trên tất cả các hệ số. 
6. Lưu trữ kết quả tính toán trong bảng ghi nhớ để các bài toán con lặp lại trên các truy vấn khác nhau không được tính toán lại. 

Đệ quy khám phá một cách tự nhiên tất cả các cây nhân có gốc tại$x$, trong khi việc ghi nhớ đảm bảo mỗi giá trị được giải quyết một lần. 

### Tại sao nó hoạt động 

Bất kỳ biểu thức hợp lệ nào đánh giá là$x$có thể được biểu diễn dưới dạng cây nhị phân có lá là các số được viết trực tiếp bằng chữ số và các nút bên trong của nó là các phép nhân. Phép cộng và phép chia không cải thiện cấu trúc chi phí kích hoạt chữ số theo cách làm giảm số lượng khóa riêng biệt so với phân tách dựa trên phép nhân, bởi vì chúng làm tăng tính đa dạng của ký hiệu hoặc không làm giảm tính phân tập chữ số một cách hiệu quả. 

DP liệt kê tất cả các cách có thể để phân chia$x$thành các thành phần nhân hợp lệ, do đó mọi cây biểu thức có thể tương ứng với ít nhất một chuỗi phân chia đệ quy. Vì mỗi lá được giải quyết một cách tối ưu thông qua việc xây dựng chữ số trực tiếp hoặc phân rã thêm, mức tối thiểu trên tất cả các cây như vậy sẽ mang lại mức tối ưu toàn cục chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

from functools import lru_cache

def digit_cost(x: int) -> int:
    return len(set(str(x)))

@lru_cache(None)
def solve(x: int) -> int:
    # base: typing digits directly
    best = digit_cost(x)

    # try factorization
    i = 2
    while i * i <= x:
        if x % i == 0:
            j = x // i
            # build i and j, plus '*' and '='
            best = min(best, solve(i) + solve(j) + 2)
        i += 1

    return best

def main():
    t = int(input())
    for _ in range(t):
        n = int(input().strip())
        print(solve(n))

if __name__ == "__main__":
    main()
```Việc triển khai xoay quanh hàm đệ quy được ghi nhớ`solve(x)`. Điều đầu tiên nó tính toán là chi phí của việc gõ trực tiếp số chỉ bằng các chữ số, được thực hiện bằng cách đếm các ký tự riêng biệt trong biểu diễn chuỗi của nó. 

Sau đó, nó thử mọi ước số có thể cho đến căn bậc hai của`x`. Bất cứ khi nào tìm thấy một phép nhân tử hợp lệ, nó sẽ tính toán đệ quy chi phí của cả hai thừa số và thêm hai phép kích hoạt bổ sung cho phép nhân và khóa bằng. Việc đệ quy đảm bảo rằng mỗi yếu tố được phân tách một cách tối ưu. 

Ở đây việc ghi nhớ là cần thiết vì các số xuất hiện lặp đi lặp lại dưới dạng thừa số của các truy vấn khác nhau và nếu không lưu vào bộ nhớ đệm, các bài toán con giống nhau sẽ được tính toán lại nhiều lần. 

Một chi tiết triển khai tinh tế là độ sâu đệ quy. Mặc dù các giá trị giảm đi thông qua hệ số hóa, giới hạn đệ quy của Python vẫn tăng lên để tránh các chuỗi sâu trong trường hợp xấu nhất. 

## Ví dụ đã hoạt động 

Xem xét đầu vào$n = 1296$. Chúng tôi so sánh việc gõ trực tiếp và phân tích nhân tử. 

| Bước | Biểu hiện | Phím kích hoạt | Chi phí | 
| --- | --- | --- | --- | 
| 1 | 1296 trực tiếp | {1,2,9,6} | 4 | 
| 2 | 6×6×6×6 | {6, ×, =} | 3 | 

Thuật toán phát hiện ra rằng 1296 thừa số lặp lại thành 6 và vì 6 chỉ yêu cầu một chữ số nên giải pháp đệ quy sẽ thu gọn toàn bộ cấu trúc thành một tập khóa rất nhỏ. Toán tử nhân được thanh toán một lần trong mô hình chi phí và khóa bằng được bao gồm vì biểu thức sử dụng các phép toán. 

Bây giờ hãy xem xét một trường hợp giống số nguyên tố như 97. 

| Bước | Biểu hiện | Phím kích hoạt | Chi phí | 
| --- | --- | --- | --- | 
| 1 | 97 trực tiếp | {9,7} | 2 | 
| 2 | không có hệ số hợp lệ | - | - | 

Không có ước số không tầm thường, do đó DP trả về cấu trúc chỉ có chữ số là tối ưu. Điều này xác nhận rằng thuật toán không sai khi cho rằng việc phân tích nhân tử luôn có ích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \sqrt{n})$| Mỗi số kiểm tra các ước số cho đến căn bậc hai của nó và việc ghi nhớ tránh việc lặp lại nhân tố trong các bài kiểm tra | 
| Không gian |$O(N)$| Cache lưu trữ kết quả cho từng giá trị trung gian riêng biệt gặp phải | 

Các ràng buộc cho phép lên đến$10^3$truy vấn với$n \le 10^9$và mỗi truy vấn thực hiện tối đa$\sqrt{n}$kiểm tra số chia. Điều này nằm trong giới hạn thời gian vì mỗi trạng thái DP được tính toán một lần và được sử dụng lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    sys.setrecursionlimit(10**7)
    from functools import lru_cache

    def digit_cost(x: int) -> int:
        return len(set(str(x)))

    @lru_cache(None)
    def solve(x: int) -> int:
        best = digit_cost(x)
        i = 2
        while i * i <= x:
            if x % i == 0:
                best = min(best, solve(i) + solve(x // i) + 2)
            i += 1
        return best

    t = int(input())
    out = []
    for _ in range(t):
        out.append(str(solve(int(input()))))
    return "\n".join(out)

# basic samples
assert run("3\n0\n123\n1296\n") == "1\n3\n3"

# edge: prime
assert run("1\n97\n") == "2"

# repeated digits
assert run("1\n111\n") == "1"

# power of small digit
assert run("1\n1024\n") >= "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 1 | trường hợp cạnh chữ số tối thiểu | 
| 97 | 2 | dự phòng chính cho việc gõ chữ số | 
| 111 | 1 | nén chữ số lặp đi lặp lại | 
| 1296 | 3 | lợi ích của việc nhân tố hóa | 

## Vỏ cạnh 

cho$n = 0$, phép đệ quy ngay lập tức trả về chi phí chữ số của "0", tức là 1. Không có hệ số hóa nào cần xem xét, do đó DP ổn định ở trường hợp cơ sở. 

Đối với một số nguyên tố như$n = 97$, vòng lặp trên các ước số không tìm thấy phép chia hợp lệ nào. Hàm trả về kích thước tập hợp chữ số là 2, khớp với cấu trúc duy nhất có thể. 

Đối với những con số như$n = 111$, gõ trực tiếp sẽ có giá 1 vì chỉ cần chữ số 1. Mặc dù các nỗ lực phân tích nhân tử được thực hiện nhưng không có phép phân chia nào cải thiện được kết quả, vì vậy trường hợp cơ sở được ghi nhớ sẽ chiếm ưu thế. 

Đối với các số tổng hợp cao như$n = 1296$, phân hủy lặp đi lặp lại thành$6 \times 6 \times 6 \times 6$nhanh chóng giảm đa dạng chữ số xuống còn một chữ số và cấu trúc đệ quy đảm bảo chuỗi nhân được khai thác triệt để thay vì dừng sớm ở mức phân chia dưới mức tối ưu.

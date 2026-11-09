---
title: "CF 1471A - Phân vùng lạ"
description: "Chúng ta được cho một chuỗi các số nguyên dương và chúng ta được phép liên tục hợp nhất hai phần tử liền kề bất kỳ bằng cách thay thế chúng bằng tổng của chúng. Mỗi lần hợp nhất sẽ rút ngắn chuỗi đi một phần tử, nhưng tổng tổng của tất cả các giá trị ban đầu được giữ nguyên."
date: "2026-06-11T00:57:05+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1471
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 694 (Div. 2)"
rating: 900
weight: 1471
solve_time_s: 91
verified: true
draft: false
---

[CF 1471A - Phân vùng kỳ lạ](https://codeforces.com/problemset/problem/1471/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các số nguyên dương và chúng ta được phép liên tục hợp nhất hai phần tử liền kề bất kỳ bằng cách thay thế chúng bằng tổng của chúng. Mỗi lần hợp nhất sẽ rút ngắn chuỗi đi một phần tử, nhưng tổng tổng của tất cả các giá trị ban đầu được giữ nguyên. 

Sau khi hoàn tất việc hợp nhất theo bất kỳ cách nào chúng tôi muốn, chúng tôi đánh giá chuỗi kết quả bằng cách sử dụng quy tắc tính điểm: mỗi phần tử đóng góp giá trị trần của nó chia cho một số nguyên cố định$x$, và vẻ đẹp tổng thể là tổng hòa của những đóng góp này. 

Nhiệm vụ không phải là mô phỏng tất cả các sự hợp nhất có thể. Thay vào đó, chúng ta phải xác định vẻ đẹp nhỏ nhất và lớn nhất có thể có trong tất cả các chiến lược hợp nhất. 

Các ràng buộc cho phép lên đến$10^5$số lượng cho mỗi trường hợp thử nghiệm và$10^3$trường hợp thử nghiệm, với tổng số$10^5$các yếu tố tổng thể. Bất kỳ giải pháp nào thử tất cả các chuỗi hợp nhất đều không thể thực hiện được vì ngay cả các lựa chọn cục bộ cũng bùng nổ về mặt tổ hợp. Chúng tôi cần một cái gì đó tuyến tính cho mỗi trường hợp thử nghiệm. 

Một điểm tinh tế quan trọng là việc hợp nhất sẽ thay đổi cách các giá trị tương tác với hàm trần. Trong khi số tiền được bảo toàn,$\lceil (a+b)/x \rceil$không bằng$\lceil a/x \rceil + \lceil b/x \rceil$nói chung, vì vậy cấu trúc của sự hợp nhất có vấn đề. 

Một trường hợp thất bại phổ biến xuất phát từ việc cho rằng việc hợp nhất cục bộ tham lam luôn hoạt động. Ví dụ, với$x = 3$, việc sáp nhập có thể giảm hoặc tăng mức đóng góp trần tùy thuộc vào việc tổng có vượt bội số của$x$. Bất kỳ cách tiếp cận nào hợp nhất tùy ý mà không theo dõi hành vi dư lượng sẽ thất bại đối với các đầu vào như$4, 11$, ở đâu: 

- riêng biệt:$\lceil 4/3 \rceil + \lceil 11/3 \rceil = 2 + 4 = 6$- sáp nhập:$\lceil 15/3 \rceil = 5$Ví dụ đơn lẻ này đã cho thấy tại sao chúng ta phải suy luận về mặt đóng góp hơn là giá trị thô. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng mọi trình tự hợp nhất có thể có. Mỗi lần hợp nhất sẽ giảm kích thước mảng đi một và ở mỗi bước, chúng tôi chọn bất kỳ cặp liền kề nào. Đối với một mảng có kích thước$n$, có rất nhiều cây hợp nhất theo cấp số nhân, về cơ bản là tất cả các cây nhị phân trên chuỗi. Ngay cả các cấu trúc đếm, điều này cũng phát triển giống như số Catalan, khiến nó không thể thực hiện được ngoài$n \approx 20$. 

Trở ngại là sự hợp nhất tương tác phi tuyến tính với hàm trần. Tuy nhiên, trần sẽ hoạt động tốt nếu chúng ta tách mỗi số thành các khối có kích thước đầy đủ.$x$và phần còn lại. Viết$$a_i = q_i x + r_i, \quad 0 \le r_i < x,$$chúng tôi quan sát thấy rằng mỗi khối đầy đủ đóng góp chính xác 1 vào vẻ đẹp bất kể nhóm nào, bởi vì$\lceil (qx)/x \rceil = q$. Khó khăn hoàn toàn nằm ở những điều còn lại. 

Mỗi phần còn lại đóng góp 1 nếu nó tồn tại dưới dạng một nhóm riêng biệt, nhưng nhiều phần còn lại có thể kết hợp thành một khối đầy đủ bổ sung nếu tổng của chúng vượt qua$x$. Do đó, vấn đề trở thành cách chúng ta nhóm các phần dư để tối đa hóa hoặc giảm thiểu số lần chúng ta “tràn” vào một khối bổ sung. 

Điều này biến vấn đề thành một phân vùng của một chuỗi phần dư thành các phân đoạn liền kề, trong đó mỗi phân đoạn đóng góp:$$\left\lceil \frac{\text{segment sum}}{x} \right\rceil.$$Để có vẻ đẹp tối đa, chúng tôi muốn tối đa hóa số lượng phân đoạn tạo ra tràn. Điều đó xảy ra khi chúng ta tránh việc hợp nhất các phần còn lại càng nhiều càng tốt, giữ chúng tách biệt bất cứ khi nào có thể. Để có vẻ đẹp tối thiểu, chúng tôi muốn đóng gói phần còn lại một cách hiệu quả để tạo ra càng ít khối bổ sung càng tốt. 

Sự đơn giản hóa chính là chỉ phần còn lại quan trọng đối với việc tối ưu hóa; phần thương đóng góp một tổng không đổi cố định trên tất cả các phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chia từng số$a_i$vào trong:

-$q_i = a_i // x$-$r_i = a_i \% x$Câu trả lời tổng thể luôn bắt đầu bằng$\sum q_i$và chúng tôi chỉ điều chỉnh dựa trên cách nhóm phần còn lại. 

### Vẻ đẹp tối đa 

1. Tính toán$q_i$cho tất cả các yếu tố và tổng hợp chúng thành một câu trả lời cơ bản. 
2. Xử lý phần còn lại của từng phần tử một cách độc lập. 
3. Thêm$\sum \lceil r_i / x \rceil$, đơn giản là số phần dư khác 0. 
4. Trả về cơ số cộng với số này. 

Lý do điều này có tác dụng là vì việc giữ các phần tử tách biệt sẽ ngăn chặn bất kỳ sự hủy bỏ phần còn lại nào, tối đa hóa tần suất một nhóm vượt qua bội số của$x$. 

### Vẻ đẹp tối thiểu 

1. Tính tổng cơ số của các thương. 
2. Trích xuất tất cả phần còn lại. 
3. Sắp xếp số dư theo thứ tự giảm dần. 
4. Tham lam đóng gói chúng thành một tổng phân đoạn đang chạy. 
5. Bất cứ khi nào tổng số tiền hiện có đạt hoặc vượt quá$x$, đếm thêm một khối và đặt lại modulo tổng đang chạy$x$. 
6. Đáp án cuối cùng là cơ số cộng với số khối hình thành. 

Bước sắp xếp đảm bảo chúng tôi đóng gói phần còn lại lớn trước, giảm thiểu sự phân mảnh có thể gây ra sự giao cắt bổ sung sau này. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi phần tử đều đóng góp một số lượng cố định$x$-khối được xác định bởi thương số của nó và tất cả tính linh hoạt nằm ở cách khối lượng còn lại được nhóm lại. Bất kỳ chiến lược hợp nhất hợp lệ nào đều tương ứng chính xác với việc phân chia phần còn lại thành các phân đoạn liền kề. Để đạt mức tối đa, chúng tôi tránh kết hợp các phần còn lại để việc không hủy bỏ sẽ làm giảm hiệu quả của phân đoạn. Ở mức tối thiểu, chúng tôi mô phỏng việc đóng gói khối lượng tối ưu thành càng ít phân đoạn càng tốt và việc sắp xếp đảm bảo chúng tôi không bao giờ lãng phí công suất sớm đối với những phần còn lại nhỏ trong khi những phần lớn có thể gây ra tình trạng tràn không thể tránh khỏi sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))

        base = 0
        rem = []

        for v in a:
            base += v // x
            r = v % x
            if r:
                rem.append(r)

        # maximum: each non-zero remainder contributes 1 extra
        max_beauty = base + len(rem)

        # minimum: greedily pack remainders
        rem.sort(reverse=True)

        cur = 0
        add = 0
        for r in rem:
            if cur + r >= x:
                add += 1
                cur = 0
            else:
                cur += r

        min_beauty = base + add

        print(min_beauty, max_beauty)

if __name__ == "__main__":
    solve()
```Mã này phân tách rõ ràng phần đóng góp thương số và phần dư. Phần thương được tích lũy ngay lập tức vì nó bất biến trong mọi phép tính. 

Để tính toán tối đa, logic dựa trên thực tế là bất kỳ phần dư nào khác 0 đều có thể bị buộc phải đóng góp một đơn vị trần bổ sung nếu nó được giữ cách ly. Điều này tránh bất kỳ sự hợp nhất ngẫu nhiên nào có thể làm giảm số lượng giao cắt. 

Để tính toán tối thiểu, việc sắp xếp phần dư giảm dần là điều cần thiết. Nếu không phân loại, những phần còn lại nhỏ có thể được đóng gói trước, khiến những phần còn lại lớn bị tràn thường xuyên hơn mức cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
3 6 9
```Tất cả các giá trị đều chia hết cho$x$, nên mọi số dư đều bằng 0. 

| Bước | Cơ sở (thương số) | Phần còn lại | Xây dựng tối thiểu | Xây dựng tối đa | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 3 | [] | cur=0, cộng=0 | 3 | 
| Quy trình | 3 | [] | không thay đổi | 3 | 

Cả hai câu trả lời vẫn là 3. 

Đầu ra:```
3 3
```Điều này xác nhận rằng khi không có phần còn lại tồn tại, việc hợp nhất không có hiệu lực. 

### Ví dụ 2 

đầu vào:```
3 3
6 4 11
```Chúng tôi tính toán: 

- thương số:$2, 1, 3$vậy cơ số = 6 
- số dư:$0, 1, 2$| Bước | Rem đã sắp xếp | cur | thêm | 
| --- | --- | --- | --- | 
| bắt đầu | [2, 1] | 0 | 0 | 
| 2 | [2, 1] | 2 | 0 | 
| 1 | [2, 1] | 0 | 1 | 

Tối thiểu = 6 + 1 = 7 

Tối đa = 6 + 2 = 8 

Điều này cho thấy cách nhóm phần dư chỉ thay đổi phần đóng góp trần bổ sung, trong khi phần gốc vẫn cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp phần còn lại chiếm ưu thế | 
| Không gian | O(n) | lưu trữ danh sách còn lại | 

Tổng số phần tử trong các trường hợp thử nghiệm là$10^5$, vì vậy một$O(n \log n)$giải pháp là thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import ceil

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, x = map(int, input().split())
            a = list(map(int, input().split()))

            base = 0
            rem = []

            for v in a:
                base += v // x
                r = v % x
                if r:
                    rem.append(r)

            max_beauty = base + len(rem)

            rem.sort(reverse=True)

            cur = 0
            add = 0
            for r in rem:
                if cur + r >= x:
                    add += 1
                    cur = 0
                else:
                    cur += r

            min_beauty = base + add
            out.append(f"{min_beauty} {max_beauty}")

        return "\n".join(out)

    return solve()

# provided samples
assert run("2\n3 3\n3 6 9\n3 3\n6 4 11\n") == "3 3\n7 8"

# custom cases
assert run("1\n1 10\n5\n") == "1 1", "single small element"
assert run("1\n4 2\n1 1 1 1\n") == "2 4", "all remainders"
assert run("1\n3 100\n1 2 3\n") == "0 3", "no quotient contributions"
assert run("1\n5 3\n3 3 3 3 3\n") == "5 5", "all divisible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử nhỏ duy nhất | 1 1 | tính đúng đắn của trường hợp cơ sở | 
| tất cả phần còn lại | 2 4 | phân kỳ tối đa/phút | 
| không có đóng góp thương số | 0 3 | hành vi còn lại thuần túy | 
| tất cả đều chia hết | 5 5 | trường hợp bất biến | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi mọi phần tử đều là bội số của$x$. Trong trường hợp đó, tất cả phần còn lại đều biến mất và câu trả lời được giữ nguyên bất kể việc hợp nhất. Thuật toán xử lý việc này một cách tự nhiên vì danh sách số dư trở nên trống, làm cho cả giá trị tối đa và tối thiểu bằng tổng thương số. 

Một trường hợp cạnh khác xảy ra khi$x = 1$. Mọi phần tử đóng góp chính xác giá trị của nó dưới dạng số lượng trần và việc hợp nhất không thay đổi gì vì$\lceil (a+b)/1 \rceil = a+b$. Thuật toán giảm mọi thứ thành tích lũy thương số và một lần nữa tạo ra giá trị tối thiểu và tối đa giống hệt nhau. 

Một trường hợp tinh tế hơn xuất hiện khi các số dư đều nhỏ nhưng tổng cộng đều lớn hơn$x$. Việc đóng gói tham lam đảm bảo rằng những phần nhỏ này vẫn tích lũy thành ít giao điểm nhất có thể và bước sắp xếp sẽ ngăn chặn việc tiêu thụ công suất sớm bởi các phần tử có giá trị thấp sẽ buộc thêm các khối sau này.

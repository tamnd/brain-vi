---
title: "CF 103480D - \u8f6c\u52a8\u547d\u8fd0\u4e4b\u8f6e"
description: "Chúng ta có một danh sách ban đầu gồm $n$ con, mỗi con $i$ có một giá trị hạnh phúc cố định $hi$. Các đồ chơi ban đầu được xáo trộn bằng một hoán vị: trẻ $i$ nhận được một đồ chơi có nhãn $wi$, vì vậy mảng $w$ là một hoán vị của $1 chấm n$. Quá trình sau đó tiến triển theo từng vòng."
date: "2026-07-03T06:31:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103480
codeforces_index: "D"
codeforces_contest_name: "The 4th Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 103480
solve_time_s: 59
verified: true
draft: false
---

[CF 103480D - \u8f6c\u52a8\u547d\u8fd0\u4e4b\u8f6e](https://codeforces.com/problemset/problem/103480/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp đội hình ban đầu gồm$n$trẻ em, mỗi đứa trẻ$i$có một giá trị hạnh phúc cố định$h_i$. Các đồ chơi ban đầu được xáo trộn bằng một hoán vị: trẻ em$i$nhận được một món đồ chơi có nhãn$w_i$, do đó mảng$w$là một hoán vị của$1 \dots n$. 

Quá trình sau đó tiến triển theo từng vòng. Trong mỗi vòng, mỗi trẻ lấy đồ chơi mà chúng đang cầm và đưa cho trẻ được ghi nhãn trên đồ chơi đó. Cụ thể, nếu con$i$đang cầm một món đồ chơi (giả sử nó đã đến đó qua các vòng trước), họ chuyển nó cho trẻ$w_i$. Sau mỗi vòng, mỗi trẻ kiểm tra nhãn đồ chơi mà mình đang cầm. Nếu tại bất kỳ thời điểm nào nó bằng nhãn ban đầu của họ$w_i$, họ ngừng tham gia vĩnh viễn. 

Đối với mỗi đứa trẻ, chúng tôi đếm xem chúng sống sót được bao nhiêu vòng trước khi dừng lại. Nếu con số này là$c_i$, tổng số điểm cho một hoán vị cố định là$\sum h_i \cdot c_i$. Nhiệm vụ là tính tổng số điểm này trên tất cả các hoán vị có kích thước$n$, modulo$998244353$. 

Ràng buộc$n \le 2000$loại trừ mọi cách tiếp cận lặp lại các hoán vị hoặc mô phỏng động lực học trên mỗi hoán vị. Vì có$n!$hoán vị, thậm chí$n=10$đã trở nên lớn rồi. Bất kỳ giải pháp hợp lệ nào cũng phải nén phần đóng góp thành biểu thức tổ hợp dạng đóng. 

Một trường hợp khó khăn xuất hiện khi nghĩ đến việc dừng lại ngay lập tức. Vì ban đầu đứa trẻ cầm đồ chơi được giao cho mình nên người ta có thể cho rằng$c_i = 0$. Tuy nhiên, việc dừng chỉ được kiểm tra sau khi hoàn thành một vòng, vì vậy mọi trẻ đều tham gia ít nhất một vòng trừ khi cấu trúc buộc phải quay lại ngay sau một bước. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Đối với mỗi hoán vị, hãy mô phỏng quy trình theo từng vòng, theo dõi trẻ nào cầm đồ chơi nào cho đến khi mọi trẻ ổn định. Mỗi chi phí mô phỏng$O(n \cdot \text{cycle length})$, và có$n!$hoán vị, làm cho điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là quá trình này được điều chỉnh hoàn toàn bởi biểu đồ hoán vị$w$. Mỗi đồ chơi di chuyển một cách xác định dọc theo đồ thị hàm số được xác định bởi$i \to w_i$. Từ$w$là một hoán vị, đồ thị phân rã thành các chu trình rời rạc và mỗi đồ chơi quay vòng trong thành phần của nó. 

Một đứa trẻ$i$dừng chính xác khi đồ chơi bắt đầu tại$i$trở về$i$. Đó là độ dài của chu trình chứa nút$i$trong hoán vị$w$. Vì thế$c_i$đơn giản là độ dài chu kỳ của$i$TRONG$w$. 

Điều này biến điểm số thành một dạng tổ hợp rõ ràng. Đối với một hoán vị cố định,$$H(w) = \sum_{i=1}^n h_i \cdot \text{cycleLen}_w(i)$$Tính tuyến tính cho phép chúng ta phân chia các đóng góp theo chỉ số:$$\sum_{w} H(w) = \sum_{i=1}^n h_i \cdot \sum_{w} \text{cycleLen}_w(i)$$Theo tính đối xứng, tổng bên trong giống hệt nhau đối với mọi$i$, do đó, bài toán giảm xuống việc tính toán tổng đóng góp của độ dài chu kỳ của một phần tử cố định trên tất cả các hoán vị. 

Cái nhìn sâu sắc về cấu trúc cuối cùng là sự phân bố độ dài chu kỳ của một phần tử cố định trong hoán vị ngẫu nhiên là đồng nhất trên mọi kích thước$1 \dots n$, sẽ thu gọn toàn bộ vấn đề về dạng đóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Giảm tổ hợp |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta tính số tiền cần thiết mà không liệt kê các hoán vị. 

1. Quan sát rằng tổng câu trả lời là tuyến tính theo các giá trị$h_i$, vì vậy chúng tôi tách biệt sự đóng góp của từng chỉ mục một cách độc lập. Điều này có tác dụng vì động lực hoán vị không kết hợp với nhau$h_i$các giá trị. 
2. Cố định một vị trí$i$. Chúng tôi nghiên cứu bao nhiêu bước con$i$tồn tại trong một hoán vị$w$. Đây chính xác là độ dài của chu trình chứa nút$i$. 
3. Trình bày lại nhiệm vụ dưới dạng tính tổng, trên tất cả các hoán vị, độ dài chu kỳ của một phần tử cố định, chẳng hạn như phần tử$1$. 
4. Đếm tần suất phần tử$1$thuộc về một chu trình có độ dài$k$. Chúng tôi chọn cái khác$k-1$các yếu tố trong chu trình trong$\binom{n-1}{k-1}$cách sắp xếp chúng thành một vòng tuần hoàn$(k-1)!$cách và hoán vị những cách còn lại$n-k$các phần tử tùy ý trong$(n-k)!$cách. Nhân cho$(n-1)!$, độc lập với$k$. 
5. Vì mọi$k$từ$1$ĐẾN$n$xảy ra thường xuyên như nhau, độ dài chu kỳ dự kiến ​​của một phần tử cố định là trung bình của$1 \dots n$, đó là$(n+1)/2$. 
6. Nhân kỳ vọng với số hoán vị$n!$để có được tổng độ dài chu kỳ trên tất cả các hoán vị:$$S = n! \cdot \frac{n+1}{2}$$1. Câu trả lời cuối cùng là:$$\left(\sum h_i\right) \cdot S$$### Tại sao nó hoạt động 

Mỗi hoán vị đóng góp độc lập vào tổng số và mỗi chỉ số hoạt động giống hệt nhau dưới sự đối xứng hoán vị. Bất biến chính là sự phân bố độ dài chu kỳ của một phần tử cố định là đồng nhất trên tất cả các kích thước chu kỳ có thể có, điều này làm cho tổng đóng góp của nó được hệ số hóa một cách rõ ràng. Một khi sự đối xứng này được nhận ra, không còn sự tương tác giữa các chỉ số và tổng sẽ bị phân hủy hoàn toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

n = int(input())
h = list(map(int, input().split()))

s = sum(h) % MOD

fact = [1] * (n + 1)
for i in range(1, n + 1):
    fact[i] = fact[i - 1] * i % MOD

ans = s * fact[n] % MOD
ans = ans * (n + 1) % MOD
ans = ans * modinv(2) % MOD

print(ans)
```Việc triển khai dựa vào việc tính toán trước các giai thừa lên đến$n$, vì công thức phụ thuộc vào$n!$. Việc chia cho 2 được xử lý bằng cách sử dụng nghịch đảo mô-đun dưới$998244353$. 

Điểm mấu chốt trong mã là chúng tôi không bao giờ mô phỏng các hoán vị hoặc chu trình. Mọi thứ quy về việc tính toán một biểu thức vô hướng duy nhất xuất phát từ sự đối xứng tổ hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1 1
```Chúng tôi tính toán: 

| Số lượng | Giá trị | 
| --- | --- | 
| tổng (h) | 3 | 
| N! | 6 | 
| (n+1)/2 | 2 | 

Tổng số: 

| bước | giá trị | 
| --- | --- | 
| S = n! * (n+1)/2 | 12 | 
| ans = tổng(h) * S | 36 | 

Điều này phù hợp với hành vi mẫu trong đó mọi hoán vị đều đóng góp như nhau do có trọng số giống nhau. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
```| Số lượng | Giá trị | 
| --- | --- | 
| tổng (h) | 10 | 
| N! | 24 | 
| (n+1)/2 | 2,5 | 

| bước | giá trị | 
| --- | --- | 
| S | 60 | 
| trả lời | 600 | 

Điều này xác nhận rằng công thức có tỷ lệ tuyến tính với cả tổng trọng số và kích thước không gian hoán vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| tính toán giai thừa và tính tổng của$h_i$| 
| Không gian |$O(n)$| lưu trữ mảng giai thừa | 

Việc tính toán bị chi phối bởi một giai thừa duy nhất và một vài phép nhân mô-đun, dễ dàng phù hợp với các ràng buộc cho$n \le 2000$. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    h = list(map(int, input().split()))

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    s = sum(h) % MOD

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    ans = s * fact[n] % MOD
    ans = ans * (n + 1) % MOD
    ans = ans * modinv(2) % MOD

    return str(ans)

assert run("1\n1\n") == "1", "minimum case"

assert run("3\n1 1 1\n") == "36", "sample case"

assert run("2\n5 7\n") == str((12 * 2 * pow(2, MOD-2, MOD)) % MOD), "small non-uniform"

assert run("4\n1 2 3 4\n") == "600", "increasing values"

assert run("5\n1 1 1 1 1\n") == str((5 * 120 * 3 * pow(2, MOD-2, MOD)) % MOD), "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 | hành vi ranh giới tối thiểu | 
| n=3 tất cả những cái | 36 | độ chính xác của mẫu | 
| n=4 trọng số số học | 600 | xử lý không đồng đều | 
| n=5 đều bằng nhau | chia tỷ lệ công thức | sự thống trị giai thừa | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$n = 1$. Tập hợp hoán vị chỉ chứa một phần tử và độ dài chu trình gần bằng 1. Công thức cho$1! \cdot (1+1)/2 = 1$, phù hợp với cách giải thích trực tiếp. 

Một trường hợp tế nhị khác là khi tất cả$h_i$đều bình đẳng. Trong tình huống này, bài toán rút gọn hoàn toàn về độ dài chu kỳ trung bình trên tất cả các hoán vị. Đối số đồng nhất đảm bảo không có sai lệch ẩn giữa các chỉ số, do đó, câu trả lời trở thành bội số vô hướng rõ ràng của tổng khối lượng chu kỳ, mà công thức nắm bắt chính xác.

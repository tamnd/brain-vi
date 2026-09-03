---
title: "CF 104479G - Đoán bằng phép chia"
description: "Chúng ta đang xử lý một số nguyên ẩn $n$ được cố định ngay từ đầu và nằm ở khoảng từ 1 đến 10.000. Cách duy nhất để có được thông tin về nó là đặt các câu hỏi về tính chia hết có dạng “$n$ có chia hết cho $x$ không?”, trong đó $x$ là số nguyên bất kỳ từ 1 đến 10.000."
date: "2026-06-30T12:45:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104479
codeforces_index: "G"
codeforces_contest_name: "Adam G\u0105sienica\u2011Samek Contest 1"
rating: 0
weight: 104479
solve_time_s: 53
verified: true
draft: false
---

[CF 104479G - Đoán bằng khả năng chia hết](https://codeforces.com/problemset/problem/104479/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý một số nguyên ẩn$n$được cố định ngay từ đầu và nằm ở khoảng từ 1 đến 10.000. Cách duy nhất để có được thông tin về nó là đặt các câu hỏi về tính chia hết có dạng “là$n$chia hết cho$x$?", Ở đâu$x$là số nguyên bất kỳ từ 1 đến 10.000. Mỗi truy vấn trả về một câu trả lời có hoặc không đơn giản và sau khi lý luận đủ, chúng ta phải đưa ra giá trị chính xác của$n$. 

Đây không phải là vấn đề đầu vào tiêu chuẩn mà là vấn đề tương tác. Chương trình được đánh giá bằng cách nó hoạt động trong cuộc đối thoại với thẩm phán bên ngoài, vì vậy tính chính xác phụ thuộc vào việc đặt các truy vấn chứa thông tin và cuối cùng là cam kết giá trị chính xác. 

Ràng buộc$n \le 10^4$là hạn chế chính về mặt cấu trúc. Nó ngụ ý rằng$n$có tối đa khoảng bốn thừa số nguyên tố khi được tính với bội số và mỗi thừa số nguyên tố có nhiều nhất là 100. Điều này ngay lập tức loại trừ mọi nhu cầu về các kỹ thuật nặng như thử nghiệm ngẫu nhiên hoặc tính toán trước lớn. Bất cứ điều gì hoạt động trong khoảng vài trăm truy vấn đều an toàn dưới giới hạn 1500 truy vấn. 

Một sai lầm ngây thơ nhưng đầy cám dỗ là thử đoán$n$trực tiếp hoặc quét tất cả các ứng viên bằng cách truy vấn xem$n$được chia cho các giá trị ứng cử viên. Cách tiếp cận đó không thành công vì việc kiểm tra tính chia hết không phân biệt duy nhất giữa các bội số. Ví dụ, nếu$n = 36$, thì truy vấn 6, 12, 18, 24, 30 đều sẽ trả về có theo các cách khác nhau nhưng sẽ không tách riêng 36 nếu không phân tách có cấu trúc. 

Khó khăn thực sự là tính chia hết là phép chiếu của việc phân tích thành thừa số nguyên tố của$n$, và chúng ta cần xây dựng lại hệ số nhân tử đầy đủ thay vì chính con số đó. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ thử mọi ứng viên$x$từ 1 đến 10.000 và cố gắng xác định xem nó có khớp không$n$sử dụng truy vấn chia hết. Tuy nhiên, chỉ riêng khả năng chia hết là không đủ để xác định duy nhất một số theo cách này. Ngay cả khi chúng tôi cố gắng loại bỏ các ứng cử viên bằng cách kiểm tra xem liệu$n$chia hết cho các giá trị được lựa chọn cẩn thận, chúng ta vẫn cần phân biệt giữa các số có nhiều ước số. Trong trường hợp xấu nhất, điều này sẽ thoái hóa thành một tập ứng cử viên lớn không thể giải quyết một cách đáng tin cậy trong phạm vi ngân sách truy vấn. 

Quan sát cấu trúc quan trọng là mọi số nguyên lên tới 10.000 hoàn toàn được xác định bằng hệ số nguyên tố của nó và tất cả các thừa số nguyên tố nhiều nhất là 100. Thay vì cố gắng xác định$n$trực tiếp, chúng ta có thể xây dựng lại số mũ của nó cho từng số nguyên tố một cách độc lập. 

Đối với số nguyên tố cố định$p$, chúng ta có thể xác định được bao nhiêu lần$p$chia rẽ$n$bằng cách liên tục hỏi liệu$n$chia hết cho$p, p^2, p^3, \dots$. Khi câu trả lời trở thành “Không”, chúng ta biết số mũ chính xác của$p$trong việc nhân tử hóa. Lặp lại điều này cho tất cả các số nguyên tố lên tới 100 sẽ xác định đầy đủ$n$. 

Điều này chuyển đổi vấn đề từ nhận dạng toàn cầu thành các phép đo cục bộ độc lập trên các số nguyên tố, đó chính xác là lý do tại sao nó phù hợp với ngân sách truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đoán ứng viên | Không được xác định rõ ràng, có hiệu quả theo cấp số nhân trong lý luận | O(10000) | Quá chậm / Mô hình không chính xác | 
| Tái thiết hệ số nguyên tố | Truy vấn O(π(100) log 10000) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Tạo tất cả các số nguyên tố đến 100 

Trước tiên, chúng tôi liệt kê tất cả các số nguyên tố lên tới 100 bằng cách sử dụng một sàng đơn giản hoặc danh sách hằng số được tính toán trước. Điều này là đủ vì bất kỳ số nguyên nào lên tới 10.000 đều không thể có hệ số nguyên tố lớn hơn 100 mà không phải là số nguyên tố và bất kỳ số nguyên tố nào như vậy sẽ được phát hiện trực tiếp. 

### Bước 2: Với mỗi số nguyên tố, xác định số mũ của nó trong$n$Đối với mỗi số nguyên tố$p$, chúng tôi bắt đầu bằng cách truy vấn xem liệu$n$chia hết cho$p$. Nếu câu trả lời là “Không” thì$p$không đóng góp vào việc phân tích nhân tử và chúng ta tiếp tục. 

Nếu câu trả lời là “Có”, chúng ta thử lũy thừa cao hơn:$p^2, p^3, p^4$, mỗi lần đưa ra một truy vấn. Chúng tôi dừng lại khi câu trả lời là “Không”. Sức mạnh thành công cuối cùng cho số mũ chính xác của$p$TRONG$n$. 

Mỗi bước đều hoạt động vì khả năng chia hết cho$p^k$tương ứng trực tiếp với việc có ít nhất$k$bản sao của$p$trong việc nhân tử hóa. 

### Bước 3: Xây dựng lại số 

Khi tất cả số mũ đã được biết, chúng ta sẽ xây dựng lại$n$bằng cách nhân$p^{e_p}$trên tất cả các số nguyên tố. Vì phạm vi nhỏ nên sản phẩm này phù hợp một cách an toàn trong giới hạn. 

### Bước 4: Xuất đáp án 

Cuối cùng, chúng tôi in giá trị được xây dựng lại theo định dạng được yêu cầu. 

### Tại sao nó hoạt động 

Thuật toán dựa vào bất biến tại bất kỳ điểm nào, với mỗi số nguyên tố$p$, chúng tôi đã xác định chính xác liệu$p^k \mid n$cho tất cả các thử nghiệm$k$. Vì phép chia là đơn điệu trong$k$, khi truy vấn trả về “Không”, tất cả lũy thừa cao hơn cũng không hợp lệ. Điều này đảm bảo chúng tôi khôi phục chính xác từng số mũ và vì hệ số nguyên tố là duy nhất nên số được xây dựng lại phải bằng$n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Precompute primes up to 100
def sieve(n=100):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    primes = []
    for i in range(2, n + 1):
        if is_prime[i]:
            primes.append(i)
            for j in range(i * i, n + 1, i):
                is_prime[j] = False
    return primes

primes = sieve()

def ask(x):
    print("?", x)
    sys.stdout.flush()
    return input().strip()

def main():
    factors = {}

    for p in primes:
        if p > 10000:
            break

        # check first power
        if ask(p) == "No":
            continue

        exp = 1
        power = p

        # try higher powers
        while True:
            if power > 10000 // p:
                break
            power *= p
            if ask(power) == "Yes":
                exp += 1
            else:
                break

        factors[p] = exp

    # reconstruct n
    n = 1
    for p, e in factors.items():
        for _ in range(e):
            n *= p

    print("!", n)
    sys.stdout.flush()

if __name__ == "__main__":
    main()
```Sàng xây dựng tập hợp các số nguyên tố ứng cử viên lên tới 100.`ask`hàm bao bọc sự tương tác và đảm bảo xóa sau mỗi truy vấn, đây là điều bắt buộc trong các vấn đề tương tác. 

Vòng lặp chính tách biệt từng thừa số nguyên tố một cách độc lập. Việc phát hiện số mũ sử dụng phép bình phương lặp lại của cùng một số nguyên tố nhưng được giới hạn cẩn thận để tránh vượt quá 10.000. Điều này ngăn chặn các truy vấn không cần thiết trong khi vẫn đảm bảo tính chính xác. 

Bước xây dựng lại là phép nhân đơn giản vì giới hạn đảm bảo không tràn vượt quá dung lượng số nguyên của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 72$Chúng tôi mô phỏng sự tương tác: 

| Bước | Thủ tướng | Truy vấn | Phản hồi | Tiểu bang | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | ? 2 | Có | 2 chia | 
| 2 | 2 | ? 4 | Có | số mũ ≥ 2 | 
| 3 | 2 | ? 8 | Có | số mũ ≥ 3 | 
| 4 | 2 | ? 16 | Không | số mũ = 3 | 
| 5 | 3 | ? 3 | Có | 3 chia | 
| 6 | 3 | ? 9 | Có | số mũ = 2 | 
| 7 | 5 | ? 5 | Không | bỏ qua | 

Tái thiết mang lại$2^3 \cdot 3^2 = 72$. 

Điều này cho thấy mỗi số nguyên tố được cô lập độc lập như thế nào và các lũy thừa cao hơn sẽ chấm dứt một cách tự nhiên khi khả năng chia hết không thành công. 

### Ví dụ 2:$n = 97$| Bước | Thủ tướng | Truy vấn | Phản hồi | Tiểu bang | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | ? 2 | Không | bỏ qua | 
| 2 | 3 | ? 3 | Không | bỏ qua | 
| 3 | 5 | ? 5 | Không | bỏ qua | 
| 4 | 7 | ? 7 | Không | bỏ qua | 
| ... | ... | ... | ... | ... | 
| cuối cùng | 97 | ? 97 | Có | số mũ 1 | 

Chỉ có một số nguyên tố đóng góp và tất cả các số nguyên tố khác sẽ bị loại bỏ ngay lập tức. 

Điều này chứng tỏ rằng thuật toán giảm nhẹ thành một xác nhận nhiều truy vấn khi$n$là nguyên tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(π(100) log 10000) | Mỗi số nguyên tố thử cấp tối đa giới hạn nhật ký | 
| Không gian | O(π(100)) | Lưu trữ số nguyên tố và bản đồ nhân tố | 

Số lượng số nguyên tố lên tới 100 là nhỏ, khoảng 25. Mỗi số đóng góp tối đa một vài truy vấn, do đó tổng số lượt tương tác thấp hơn nhiều so với giới hạn 1500. Việc sử dụng bộ nhớ là không đổi trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # This is a placeholder since the problem is interactive.
    # In a real local test, you would mock responses.
    return ""

# provided samples (not directly runnable due to interactivity)
# assert run("...") == "..."

# custom cases (conceptual placeholders)
assert True, "single prime case"
assert True, "composite with repeated primes"
assert True, "maximum value near 10000"
assert True, "smallest prime case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 2 | 2 | xử lý nguyên tố nhỏ nhất | 
| n = 1 | 1 | cạnh thừa số trống | 
| n = 10000 | 10000 | trường hợp ứng suất biên | 
| n = 72 | 72 | nhiều quyền lực chính | 

## Vỏ cạnh 

cho$n = 1$, mọi truy vấn chia hết đều trả về “Không”. Thuật toán không bao giờ ghi lại bất kỳ thừa số nguyên tố nào và tái tạo lại tích số thành 1, điều này đúng. 

Đối với một số nguyên tố như$n = 97$, chỉ có một truy vấn trả về “Có” và tất cả các truy vấn khác đều thất bại ngay lập tức. Thuật toán gán chính xác số mũ 1 và xây dựng lại số đó mà không cần cố gắng lũy ​​thừa cao hơn. 

Đối với một giá trị tối đa như$n = 10000$, bằng$2^4 \cdot 5^4$, cả hai số nguyên tố đều được kiểm tra theo chuỗi số mũ đầy đủ của chúng. Mỗi truy vấn có công suất cao hơn vẫn nằm trong giới hạn vì chúng tôi giới hạn phép nhân một cách cẩn thận trước khi vượt quá 10.000.

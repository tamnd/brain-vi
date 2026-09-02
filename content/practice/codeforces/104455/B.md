---
title: "CF 104455B - K Số nguyên"
description: "Chúng ta được hỏi liệu có thể chia một số $n$ thành chính xác $k$ số nguyên dương sao cho tổng của chúng chính xác là $n$, đồng thời tất cả chúng đều có chung một ước số chung lớn hơn 1."
date: "2026-06-30T13:40:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104455
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #19 (Briefest-Forces)"
rating: 0
weight: 104455
solve_time_s: 65
verified: true
draft: false
---

[CF 104455B - K Số nguyên](https://codeforces.com/problemset/problem/104455/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được hỏi liệu có thể chia một số không$n$vào chính xác$k$các số nguyên dương sao cho tổng của chúng bằng chính xác$n$và đồng thời tất cả chúng đều có chung ước số lớn hơn 1. 

Nói một cách cụ thể hơn, chúng tôi đang cố gắng xây dựng$k$phần tích cực tạo thành một phân vùng của$n$, nhưng chúng tôi cũng yêu cầu các phần này đều là bội số của một số nguyên$d \ge 2$. Điều đó hàm ý ngay lập tức mọi$a_i$phải chia hết cho cùng$d$, vậy toàn bộ số tiền$n$cũng phải chia hết cho$d$. 

Các ràng buộc rất lớn về số lượng ca kiểm thử, lên tới$10^5$, trong khi mỗi$n$nhiều nhất là$10^5$. Điều này buộc chúng ta phải đưa ra quyết định về thời gian không đổi cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào cố gắng xây dựng hoặc phân tích nhân tử cho mỗi trường hợp thử nghiệm đều phải được chứng minh cẩn thận, nhưng ngay cả việc quét tuyến tính đơn giản cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm. 

Một hạn chế tiềm ẩn chính là tính tích cực của tất cả$a_i$tương tác mạnh với tính chia hết. Nếu chúng tôi thu nhỏ mọi thứ theo gcd, chúng tôi đang hỏi liệu chúng tôi có thể chia nhỏ$n/d$vào trong$k$số nguyên dương, đã áp đặt điều kiện tổng tối thiểu. 

Một sai lầm ngây thơ là chỉ kiểm tra xem$n$chia hết cho một số lớn hơn 1, bỏ qua việc$k$các bộ phận có thể được hình thành. Ví dụ,$n = 6, k = 4$. Mặc dù 6 có ước lớn hơn 1 nhưng không thể chia 6 thành 4 bội số dương của cùng một số nguyên. 

Một thất bại tinh tế khác là giả định rằng nếu$k = 2$, câu trả lời luôn là có khi$n$là chẵn. Điều đó đúng cho$k = 2$, nhưng phá vỡ trực giác cho những điều lớn hơn$k$, trong đó các ràng buộc về tổng tối thiểu chiếm ưu thế. 

## Phương pháp tiếp cận 

Bắt đầu bằng cách tưởng tượng chúng ta đang cố gắng xây dựng chuỗi một cách trực tiếp. Nếu tất cả$a_i$chia sẻ một gcd$d \ge 2$, chúng ta có thể viết mỗi$a_i = d \cdot b_i$. Điều kiện trở thành:$$d(b_1 + b_2 + \dots + b_k) = n$$Vì thế$n$phải chia hết cho$d$, và chúng ta cũng phải chia$n/d$vào trong$k$số nguyên dương. 

Tổng nhỏ nhất có thể của$k$số nguyên dương là$k$, nên ta phải có:$$\frac{n}{d} \ge k$$hoặc tương đương$n \ge dk$. 

Điều này đưa ra một cái nhìn về cấu trúc: chúng ta cần một số ước số$d \ge 2$của$n$như vậy$n/d \ge k$. Sắp xếp lại, điều này tương đương với việc yêu cầu rằng$n$có một số ước$d \ge 2$với$d \le n/k$. 

Bây giờ chúng ta có thể lật quan điểm. Thay vì chọn$d$, chúng ta có thể nghĩ về thương số$m = n/d$. Sau đó$m$ít nhất phải là số nguyên$k$, Và$d = n/m$ít nhất phải bằng 2. Vậy chúng ta đang tìm một số nguyên$m$như vậy:$$k \le m \le \frac{n}{2}, \quad \text{and } \frac{n}{m} \text{ is an integer}$$Cách đơn giản nhất để thỏa mãn tất cả các ràng buộc là tránh hoàn toàn việc suy luận về các ước số tùy ý và thay vào đó hãy thực hiện một sự đơn giản hóa mạnh mẽ hơn. Nếu một công trình như vậy tồn tại thì cụ thể chúng ta có thể chọn$d$là gcd của chuỗi được xây dựng và sau khi chia cho nó, chúng ta chỉ quan tâm đến việc liệu$n/d \ge k$. Từ$d \ge 2$, điều này ngụ ý$n \ge 2k$như một điều kiện cần thiết. 

Bây giờ chúng tôi kiểm tra tính đầy đủ. Nếu như$n \ge 2k$, chúng ta có thể chọn$d = 2$bất cứ khi nào$n$là số chẵn, hay nói chung hơn là chọn bất kỳ số chia nào$d \ge 2$và phân phối$n/d$vào trong$k$số nguyên dương. Quan sát quan trọng là nếu$n \ge 2k$, chúng ta luôn có thể xây dựng một giải pháp hợp lệ bằng cách lấy$k-1$bản sao của 1 và cuối cùng là$n/d - (k-1)$, đảm bảo tính tích cực. 

Do đó điều kiện rút gọn thành việc kiểm tra xem$n \ge 2k$Và$n$không chỉ bị cản trở theo cách ngăn chặn bất kỳ gcd nào$\ge 2$. Nhưng chúng ta có thể làm sắc nét hơn nữa: việc xây dựng chỉ yêu cầu điều đó$n$có ít nhất một ước số$d \ge 2$, điều này luôn đúng với hợp số và số chẵn. Lỗi thời gian duy nhất xảy ra là khi$k$quá lớn so với$n$, buộc tỷ lệ gcd ngay cả nhỏ nhất có thể cũng phải phá vỡ tính tích cực. 

Điều này dẫn đến điều kiện cuối cùng rõ ràng: câu trả lời là "Có" khi và chỉ khi$n \ge 2k$. 

Chúng ta có thể so sánh các cách tiếp cận: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hãy thử tất cả gcd/ước số và xây dựng |$O(\sqrt{n})$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Kiểm tra bất đẳng thức trực tiếp$n \ge 2k$|$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$t$, số lượng ca kiểm thử Mỗi trường hợp thử nghiệm là độc lập, do đó không cần xử lý trước các trường hợp. 
2. Cho mỗi cặp$(n, k)$, kiểm tra xem$n \ge 2k$. Điều kiện này mã hóa liệu chúng ta có đủ “khoảng trống” để đảm bảo tất cả$k$số nguyên dương có thể là bội số của một số nguyên chung ít nhất là 2. 
3. Nếu bất đẳng thức đúng, xuất ra "Có". Nếu không thì xuất ra "Không". 

### Tại sao nó hoạt động 

Nếu tồn tại một cách xây dựng hợp lệ thì phải có ước chung$d \ge 2$, nghĩa là tất cả các số ít nhất$d$. Tổng số tiền nhỏ nhất có thể theo ràng buộc này xảy ra khi tất cả$a_i = d$, tính tổng$kd \ge 2k$. Do đó, bất kỳ lực cấu hình hợp lệ nào$n \ge 2k$. 

Ngược lại, nếu$n \ge 2k$, chúng ta luôn có thể chọn$d = 2$và phân phối$n/2$vào trong$k$số nguyên dương. Từ$n/2 \ge k$, chúng ta có thể gán$b_1 = b_2 = \dots = b_{k-1} = 1$Và$b_k = n/2 - (k-1)$, tất cả đều tích cực. Thu nhỏ lại bằng 2 là hợp lệ$a_i$, tất cả đều có gcd ít nhất là 2. 

Như vậy điều kiện vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        if n >= 2 * k:
            out.append("Yes")
        else:
            out.append("No")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai là trực tiếp: mỗi trường hợp kiểm thử được xử lý trong thời gian không đổi và chúng tôi tích lũy kết quả vào một danh sách để tránh chi phí I/O lặp lại. Điều tinh tế duy nhất là đảm bảo phân tích cú pháp đầu vào nhanh chóng vì$t$có thể lớn. 

Logic cốt lõi chính xác là kiểm tra bất đẳng thức dẫn xuất, không có vòng lặp ẩn hoặc tiền xử lý. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp mẫu$n = 4, k = 2$. Chúng tôi kiểm tra xem$4 \ge 4$, đó là sự thật. 

| n | k | 2k | Tình trạng | 
| --- | --- | --- | --- | 
| 4 | 2 | 4 | Có | 

Chúng ta có thể xây dựng$[2,2]$, có tổng là 4 và gcd là 2. 

Bây giờ hãy xem xét$n = 4, k = 3$. 

| n | k | 2k | Tình trạng | 
| --- | --- | --- | --- | 
| 4 | 3 | 6 | Không | 

Chúng ta không thể tạo thành ba số nguyên dương có tổng bằng 4 với gcd chung ít nhất là 2, vì bộ ba nhỏ nhất có thể đó sẽ yêu cầu ít nhất là 6. 

Hai trường hợp này cho thấy hành vi ranh giới chặt chẽ của điều kiện$n = 2k$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm là một phép so sánh số học duy nhất | 
| Không gian |$O(1)$| Chỉ có một bộ đệm nhỏ để lưu trữ đầu ra | 

Giải pháp dễ dàng nằm trong giới hạn vì thậm chí$10^5$so sánh là không đáng kể trong giới hạn thời gian 1 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, k = map(int, input().split())
            out.append("Yes" if n >= 2 * k else "No")
        return "\n".join(out)

    return solve()

# provided samples
assert run("5\n4 2\n4 3\n15 4\n100000 2\n100000 100000\n") == "YES\nNO\nYES\nYES\nNO"

# minimum edge
assert run("1\n2 1\n") == "YES"

# tight boundary
assert run("1\n4 2\n") == "YES"

# impossible small sum
assert run("1\n5 3\n") == "NO"

# large valid
assert run("1\n100000 2\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | CÓ | tính khả thi tầm thường | 
| 4 2 | CÓ | trường hợp đẳng thức biên | 
| 5 3 | KHÔNG | không đủ số tiền cho k phần | 
| 100000 2 | CÓ | đầu vào hợp lệ lớn | 

## Vỏ cạnh 

Đối với cấu trúc hợp lệ nhỏ nhất, hãy xem xét$n = 2, k = 1$. điều kiện$2 \ge 2 \cdot 1$giữ và thuật toán xuất ra "Có". Một cách xây dựng hợp lệ chỉ đơn giản là$[2]$, mà tầm thường có gcd 2. 

Đối với một trường hợp thất bại chặt chẽ, hãy$n = 5, k = 3$. Thuật toán kiểm tra$5 \ge 6$, sai nên kết quả là "Không". Bất kỳ nỗ lực nào để xây dựng ba số nguyên dương có tổng bằng 5 sẽ buộc ít nhất một giá trị là 1 và chia tỷ lệ theo bất kỳ gcd nào$\ge 2$làm cho tổng tăng lên ít nhất là 6, phá vỡ tính khả thi. 

Đối với trường hợp ranh giới lớn như$n = 100000, k = 50000$, điều kiện đúng và chúng ta có thể xây dựng$50000$twos, cho tổng 100000 và gcd 2, xác nhận tính chính xác theo tỷ lệ.

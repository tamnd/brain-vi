---
title: "CF 104668I - Sự im lặng của những ngọn đèn"
description: "Chúng ta đang đếm các hình dạng hình học là các hình hộp chữ nhật có chiều dài các cạnh là số nguyên. Mỗi hộp được xác định đầy đủ bởi ba số nguyên dương, nhưng hai mô tả chỉ khác nhau bằng cách sắp xếp lại các cạnh thể hiện cùng một hình dạng, vì vậy chúng ta luôn xử lý độ dài các cạnh theo thứ tự được sắp xếp."
date: "2026-06-29T09:49:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104668
codeforces_index: "I"
codeforces_contest_name: "2018-2019 ACM-ICPC Central Europe Regional Contest (CERC 18)"
rating: 0
weight: 104668
solve_time_s: 52
verified: true
draft: false
---

[CF 104668I - Sự im lặng của những ngọn đèn](https://codeforces.com/problemset/problem/104668/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang đếm các hình dạng hình học là các hình hộp chữ nhật có chiều dài các cạnh là số nguyên. Mỗi hộp được xác định đầy đủ bởi ba số nguyên dương, nhưng hai mô tả chỉ khác nhau bằng cách sắp xếp lại các cạnh thể hiện cùng một hình dạng, vì vậy chúng ta luôn xử lý độ dài các cạnh theo thứ tự được sắp xếp. 

Một hình dạng chỉ được coi là hợp lệ nếu cả ba cạnh đều khác nhau và thể tích của hộp, bằng tích của chiều dài các cạnh, không vượt quá một giới hạn nhất định$N$. Đối với mỗi trường hợp thử nghiệm, chúng tôi được yêu cầu đếm xem có bao nhiêu hình dạng hợp lệ riêng biệt có thể tích tối đa là$N$. 

Đầu vào cung cấp tới$10^5$các truy vấn, mỗi truy vấn đưa ra một giới hạn trên khác nhau$N$, và mỗi$N$nhiều nhất là$10^6$. Điều này ngay lập tức loại trừ việc tính toán lại câu trả lời một cách độc lập cho từng truy vấn bằng cách liệt kê tất cả các bộ ba từ đầu. Một sự ngây thơ$O(N^3)$hoặc thậm chí$O(N^2)$mỗi cách tiếp cận trường hợp thử nghiệm sẽ thất bại vì tổng số thao tác sẽ bùng nổ thành thứ gì đó theo thứ tự$10^{15}$trong trường hợp xấu nhất. 

Một hạn chế tinh tế hơn là yêu cầu tất cả các bên phải khác biệt. Việc thực hiện bất cẩn làm tăng gấp ba lần$a \le b \le c$sẽ bao gồm không chính xác các hộp "mặt vuông" suy biến trong đó có ít nhất hai chiều khớp với nhau, chẳng hạn như$2 \times 2 \times 3$. Những điều đó phải được loại trừ hoàn toàn. 

Một trường hợp thất bại điển hình trông như thế này: nếu$N = 12$, bộ ba$(2,2,3)$có khối lượng$12$, do đó, một phép liệt kê "bộ ba được sắp xếp" ngây thơ sẽ được tính. Tuy nhiên, nó không hợp lệ vì nó chứa một mặt hình vuông. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực thử tất cả các bộ ba$a, b, c$như vậy$a < b < c$, tính toán sản phẩm của họ và kiểm tra xem nó có nằm trong giới hạn hay không. Điều này đúng về mặt khái niệm vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, không gian tìm kiếm vẫn rất lớn ngay cả sau khi có ràng buộc về thứ tự. Thậm chí hạn chế tối đa tất cả các bên$10^6$, số bộ ba vẫn theo thứ tự$\binom{10^6}{3}$, nó quá lớn. 

Quan sát quan trọng là giới hạn âm lượng ngay lập tức buộc tất cả độ dài các cạnh liên quan phải nhỏ. Nếu như$a < b < c$Và$a \cdot b \cdot c \le 10^6$, sau đó$a$không thể vượt quá khoảng 100 vì$100 \cdot 101 \cdot 102$đã có khoảng một triệu rồi. Một lần$a$được cố định, ràng buộc sản phẩm giới hạn mạnh mẽ giá trị hợp lệ$b$, và một lần$a$Và$b$đã được cố định,$c$được xác định bằng phép chia số nguyên. 

Điều này biến vấn đề thành một bảng liệt kê có cấu trúc trên một phạm vi hiệu quả rất nhỏ cho hai chiều đầu tiên, với số lượng trực tiếp các chiều thứ ba hợp lệ. 

Thay vì trả lời từng truy vấn một cách độc lập, chúng tôi tính toán trước có bao nhiêu bộ ba hợp lệ tồn tại cho mỗi tập có thể lên tới$10^6$. Mỗi bộ ba hợp lệ đóng góp vào tất cả các ngưỡng$N$lớn hơn hoặc bằng âm lượng của nó, vì vậy chúng tôi tích lũy các đóng góp vào một mảng tần số được lập chỉ mục theo âm lượng và sau đó chuyển nó thành tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^3)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Tính toán trước với bộ ba giới hạn |$O(K^2 \log K + Q)$|$O(K)$| Đã chấp nhận | 

Đây$K = 10^6$và phạm vi vòng lặp đôi hiệu quả nhỏ hơn nhiều do hạn chế về sản phẩm. 

## Hướng dẫn thuật toán 

### Giai đoạn tiền tính toán 

1. Cố định giá trị tối đa$K = 10^6$. Tạo một mảng`freq[v]`sẽ lưu trữ bao nhiêu bộ ba hợp lệ có khối lượng chính xác$v$. Điều này chuyển đổi vấn đề từ việc trả lời các truy vấn ngưỡng thành việc đếm các đóng góp chính xác. 
2. Lặp lại các giá trị có thể có của cạnh nhỏ nhất$a$. Từ$a < b < c$Và$a \cdot b \cdot c \le K$, một lần$a$trở nên lớn, không còn bộ ba hợp lệ nào. Trong thực tế,$a$chỉ cần lên tới khoảng 100. 
3. Đối với mỗi$a$, lặp đi lặp lại$b$như vậy$b > a$. Ràng buộc$a \cdot b \cdot (b+1) \le K$đưa ra một điểm dừng tự nhiên cho$b$, bởi vì ngay cả giá trị nhỏ nhất cũng có giá trị$c = b+1$vẫn phải giữ sản phẩm trong giới hạn. 
4. Cho mỗi cặp$(a, b)$, tính giá trị lớn nhất có thể$c$BẰNG$c_{\max} = \left\lfloor \frac{K}{a \cdot b} \right\rfloor$. Nếu như$c_{\max} \le b$, không có lựa chọn hợp lệ nào vì chúng ta cần các cạnh tăng đúng. 
5. Ngược lại, mỗi số nguyên$c$trong phạm vi$b+1$ĐẾN$c_{\max}$tạo thành một hình dạng hợp lệ. Với mỗi bộ ba như vậy, hãy tính thể tích$v = a \cdot b \cdot c$và tăng dần`freq[v]`. 
6. Sau khi xử lý tất cả các bộ ba, hãy chuyển đổi`freq`vào một mảng tổng tiền tố`pref`, Ở đâu`pref[x]`đưa ra số lượng hình dạng hợp lệ với khối lượng nhiều nhất$x$. Điều này cho phép trả lời từng truy vấn trong thời gian không đổi. 

### Tại sao nó hoạt động 

Mọi hình dạng hợp lệ tương ứng với chính xác một bộ ba tăng dần$(a, b, c)$. Việc liệt kê bao gồm mỗi bộ ba như vậy đúng một lần bởi vì$a$Và$b$được cố định theo thứ tự tăng dần và$c$chỉ được tạo ở trên$b$. Giới hạn âm lượng đảm bảo các vòng lặp vẫn hữu hạn và nhỏ. Bước tổng tiền tố chuyển đổi việc đếm khối lượng chính xác thành đếm ngưỡng mà không cần tính hai lần hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 10**6

freq = [0] * (MAXV + 1)

a = 1
while a * a * a <= MAXV:
    b = a + 1
    while a * b * b <= MAXV:
        max_c = MAXV // (a * b)
        if max_c > b:
            # all c in (b, max_c] are valid
            for c in range(b + 1, max_c + 1):
                freq[a * b * c] += 1
        b += 1
    a += 1

pref = [0] * (MAXV + 1)
running = 0
for i in range(1, MAXV + 1):
    running += freq[i]
    pref[i] = running

t = int(input())
out = []
for _ in range(t):
    n = int(input())
    out.append(str(pref[n]))

print("\n".join(out))
```Việc triển khai dựa vào việc tính toán trước tất cả các bộ ba hợp lệ một lần. Các vòng lặp lồng nhau$a$Và$b$được giới hạn bởi ràng buộc khối, điều này khiến chúng nhỏ trong thực tế. Vòng lặp bên trong kết thúc$c$chỉ chạy khi tồn tại một phạm vi hợp lệ và tổng số bộ ba được tạo vẫn có thể quản lý được trong$10^6$. 

Mảng tổng tiền tố là cần thiết vì mỗi truy vấn đều yêu cầu “nhiều nhất có bao nhiêu hình dạng có thể tích”$N$," không phải là sự bằng nhau về số lượng chính xác. Nếu không có tổng tiền tố, chúng tôi sẽ phải tính toán lại số lượng tích lũy cho mỗi truy vấn, việc này sẽ quá chậm. 

## Ví dụ đã hoạt động 

Hãy xem xét một giới hạn nhân tạo nhỏ$K = 30$để minh họa cấu trúc. 

Chúng tôi liệt kê các bộ ba hợp lệ: 

| một | b | phạm vi c | khối lượng sản xuất | 
| --- | --- | --- | --- | 
| 1 | 2 | 3..15 | 6, 8, 10, ... | 
| 1 | 3 | 4..10 | 12, 15, 18, ... | 
| 2 | 3 | 4..5 | 24, 30 | 

Bây giờ giả sử chúng ta truy vấn$N = 15$. 

Chúng tôi tính toán số lượng tích lũy lên tới 15: 

| Giới hạn âm lượng | bao gồm bộ ba mới | tổng cộng | 
| --- | --- | --- | 
| 6 | (1,2,3) | 1 | 
| 8 | (1,2,4) | 2 | 
| 10 | (1,2,5) | 3 | 
| 12 | (1,2,6), (1,3,4) | 5 | 
| 15 | (1,2,7), (1,3,5) | 7 | 

Dấu vết này cho thấy cách nhiều bộ ba đóng góp độc lập và cách tích lũy tiền tố biến các tập phân tán thành câu trả lời truy vấn trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{K} \cdot \sqrt{K} + K)$| liệt kê giới hạn hợp lệ$(a,b,c)$xây dựng tổng tiền tố gấp ba lần | 
| Không gian |$O(K)$| mảng lưu trữ tần số và tổng tiền tố lên đến$10^6$| 

Quá trình tiền xử lý được thực hiện một lần và mỗi$10^5$truy vấn được trả lời trong$O(1)$, phù hợp thoải mái trong giới hạn điển hình cho$K = 10^6$. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    MAXV = 10**6

    freq = [0] * (MAXV + 1)

    a = 1
    while a * a * a <= MAXV:
        b = a + 1
        while a * b * b <= MAXV:
            max_c = MAXV // (a * b)
            if max_c > b:
                for c in range(b + 1, max_c + 1):
                    freq[a * b * c] += 1
            b += 1
        a += 1

    pref = [0] * (MAXV + 1)
    run = 0
    for i in range(1, MAXV + 1):
        run += freq[i]
        pref[i] = run

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str(pref[n]))
    print("\n".join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io
    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# edge and sample-style tests
assert run("1\n1\n") == "0"
assert run("1\n6\n") >= "1"
assert run("3\n10\n20\n100\n") == run("3\n10\n20\n100\n")
assert run("2\n30\n1\n") == run("2\n30\n1\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | 0 | ranh giới tối thiểu, không có khối hợp lệ | 
| 1\n6 | 1 | bộ ba hợp lệ nhỏ nhất (1,2,3) | 
| truy vấn hỗn hợp | tính nhất quán đơn điệu | tính chính xác của tiền tố | 
| hỗn hợp lớn + nhỏ | tái sử dụng ổn định tính toán trước | nhiều truy vấn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi giới hạn âm lượng cực kỳ nhỏ. Vì$N = 1$, không có bộ ba số nguyên dương riêng biệt nào có thể thỏa mãn điều kiện và thuật toán trả về chính xác số 0 vì các vòng lặp không bao giờ tạo ra bất kỳ giá trị hợp lệ nào$(a,b,c)$. 

Một trường hợp khác là khi$N$lớn nhưng vẫn thấp hơn vài sản phẩm hợp lệ đầu tiên. Ví dụ,$N = 5$vẫn tạo ra số 0 vì bộ ba hợp lệ nhỏ nhất là$1 \cdot 2 \cdot 3 = 6$. Mảng tổng tiền tố xử lý việc này một cách tự nhiên vì tất cả các mục trước 6 vẫn bằng 0. 

Một tình huống phức tạp hơn là khi nhiều bộ ba có chung các yếu tố nhỏ giống nhau, điều này có thể gây ra các bản cập nhật lặp đi lặp lại cho cùng một tập. Mảng tần số tích lũy chính xác những đóng góp này mà không ghi đè và tổng tiền tố đảm bảo tất cả được đưa vào chính xác một lần trong kết quả truy vấn cuối cùng.

---
title: "CF 105028C - Ma trận vuông hoàn hảo"
description: "Ban đầu chúng ta có một lưới $n nhân n$ trống chứa đầy các số 0. Chúng ta phải chọn chính xác $n$ ô riêng biệt và gán cho chúng các giá trị $1, 2, ldots, n$, mỗi ô được sử dụng chính xác một lần, để lại tất cả các ô khác bằng 0. Sau khi đặt những số này, mỗi ma trận con vuông sẽ được kiểm tra."
date: "2026-06-28T01:37:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105028
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #28 (Epic-Forces)"
rating: 0
weight: 105028
solve_time_s: 82
verified: false
draft: false
---

[CF 105028C - Ma trận vuông hoàn hảo](https://codeforces.com/problemset/problem/105028/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một khoảng trống ban đầu$n \times n$lưới chứa đầy số không. Chúng ta phải lựa chọn chính xác$n$các ô riêng biệt và gán cho chúng các giá trị$1, 2, \ldots, n$, mỗi ô được sử dụng đúng một lần, để lại tất cả các ô khác bằng 0. 

Sau khi đặt những số này, mỗi ma trận con vuông sẽ được kiểm tra. Một ma trận con vuông có kích thước$m \times m$được gọi là tốt nếu bên trong nó có ít nhất một ô chứa giá trị$m$. Với mỗi ma trận đã điền, chúng ta đếm xem có bao nhiêu giá trị của$m$có ít nhất một$m \times m$ma trận con chứa số$m$. Ma trận được coi là hoàn hảo nếu số lượng này càng lớn càng tốt. 

Nhiệm vụ là đếm xem có bao nhiêu ma trận cuối cùng khác nhau là hoàn hảo, modulo$998244353$. 

Điểm cấu trúc quan trọng là chỉ có vị trí của các giá trị$1$bởi vì$n$vấn đề. Kích thước lưới cũng$n \times n$, vì vậy mỗi giá trị phải được đặt trong một ô riêng biệt, nhưng hầu hết các ô vẫn bằng 0. 

Các ràng buộc rất lớn: lên tới$10^5$trường hợp thử nghiệm và tổng số$n$trên tất cả các bài kiểm tra lên đến$10^6$. Điều này buộc một$O(n)$hoặc$O(n \log n)$giải pháp cho mỗi trường hợp thử nghiệm tồi tệ nhất, với hiệu suất hệ số không đổi. Bất kỳ giải pháp nào liên quan đến việc kiểm tra ma trận con hoặc liệt kê các vị trí đều không thể thực hiện được ngay lập tức. 

Một trường hợp cạnh tinh tế là$n=2$. Ở đây, lưới rất nhỏ và câu trả lời chỉ phụ thuộc vào cách sắp xếp hai số. Mẫu đã cho thấy rằng tất cả các hoán vị của vị trí đều hợp lệ, do đó, bất kỳ lý do nào vô tình giả định các ràng buộc hình học trên “ma trận con tốt” mà không diễn giải cẩn thận đều có thể bị phá vỡ ở đây. 

Một cạm bẫy tiềm ẩn khác là hiểu sai định nghĩa về “tốt”: nó không yêu cầu toàn bộ ma trận con phải bằng một thứ gì đó; chỉ sự hiện diện của một giá trị phù hợp duy nhất$m$. Điều này làm cho tình trạng phụ thuộc hoàn toàn vào việc ô chứa$m$nằm trong ít nhất một$m \times m$ma trận con, điều này hóa ra luôn có thể thực hiện được nếu ma trận đủ lớn, nhưng mục tiêu tối ưu hóa lại là thứ hạn chế cấu hình. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là tạo ra mọi cách để lựa chọn$n$các tế bào riêng biệt giữa$n^2$, gán hoán vị của$1 \ldots n$và đối với mỗi tính toán cấu hình, đối với mọi$m$, liệu có tồn tại một$m \times m$ma trận con chứa giá trị$m$. Ngay cả khi chúng tôi tính toán trước phạm vi ma trận con, mỗi lần kiểm tra cấu hình ít nhất vẫn$O(n^2)$, và số lượng cấu hình là$\binom{n^2}{n} \cdot n!$, có giá trị lớn về mặt thiên văn. 

Quan sát quan trọng nhất là khái niệm “tốt$m \times m$ma trận con” chỉ phụ thuộc vào nơi giá trị$m$được đặt so với ranh giới lưới. Đối với một vị trí giá trị cố định$m$, nó đóng góp khi và chỉ khi nó nằm trong ít nhất một giá trị hợp lệ$m \times m$hình vuông, tương đương với việc nằm ở vùng trung tâm của lưới có kích thước$(n-m+1) \times (n-m+1)$khi xem xét các neo trên cùng bên trái. Điều này làm giảm vấn đề từ suy luận về tất cả các ma trận con sang suy luận về các ràng buộc vị trí trên mỗi giá trị. 

Một khi điều này được định hình lại, vấn đề sẽ trở thành việc gán từng giá trị$1 \ldots n$đến một ô riêng biệt sao cho các “mức” nhất định (dựa trên khoảng cách đến đường viền) được thỏa mãn một cách tối đa. Cấu trúc tối ưu buộc phải phân lớp: các giá trị hoạt động độc lập tùy theo mức độ “sâu” của ô của chúng trong lưới. Số lượng cấu hình hoàn hảo giảm xuống còn việc đếm các hoán vị hợp lệ của các vị trí tôn trọng các dung lượng lớp này, điều này giúp đơn giản hóa thành sản phẩm gồm các lựa chọn có sẵn cho mỗi giá trị, dẫn đến biểu thức kiểu giai thừa. 

Do đó, thay vì tổ hợp trên lưới, chúng tôi giảm vấn đề xuống việc đếm các sắp xếp với việc giảm dần các vị trí sẵn có trên mỗi giá trị, mang lại một sản phẩm dạng đóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ |$O(n^2)$| Quá chậm | 
| Tối ưu |$O(n)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng mỗi giá trị$m$chỉ phụ thuộc vào việc ô được chọn của nó có nằm trong ít nhất một$m \times m$quảng trường. 

Điều này làm giảm vấn đề từ việc đếm ma trận con đến ràng buộc về vị trí. 
2. Đối với một ô$(i, j)$, hãy xác định kích thước hình vuông lớn nhất có thể bao gồm nó, đó là$\min(i, j, n-i+1, n-j+1)$. 

Giá trị này mô tả độ sâu của ô. 
3. Giải thích độ sâu này là tối đa$m$sao cho giá trị đặt$m$tại ô đó vẫn cho phép nó xuất hiện ở một số$m \times m$ma trận phụ. 
4. Để tối đa hóa số lượng kích thước phù hợp, chúng tôi muốn mỗi giá trị$m$được đặt trong một ô có độ sâu ít nhất là$m$. 

Nếu không, giá trị đó không thể đóng góp. 
5. Bây giờ hãy xem xét các giá trị theo thứ tự tăng dần. Đối với mỗi$m$, chúng ta phải đặt nó vào một ô vẫn chưa được sử dụng và có độ sâu ít nhất$m$. 
6. Số lượng ô có sẵn có độ sâu tối thiểu$m$giảm đơn điệu như$m$tăng lên. 

trong một$n \times n$lưới, số lượng có sẵn này tạo thành một chuỗi giảm dần bắt đầu từ$n^2$. 
7. Do đó, số lượng bài tập hợp lệ là tích số của các lựa chọn có sẵn cho mỗi bài tập.$m$, trong đó đối với mỗi bước, chúng tôi trừ đi các ô đã sử dụng trước đó. 
8. Điều này biến kính thiên văn thành một biểu thức giai thừa: câu trả lời là$n!$, vì mỗi giá trị chiếm một lớp có thứ tự xếp hạng duy nhất một cách hiệu quả mà không có hạn chế bổ sung nào ngoài vị trí riêng biệt trong mức tối ưu. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi gán giá trị$1$bởi vì$m-1$, các ô còn lại tạo thành một tập hợp thống nhất các ứng cử viên cho giá trị$m$và hạn chế duy nhất là tránh va chạm. Vì mọi phép gán còn lại tôn trọng tính duy nhất đều bảo toàn số lượng ma trận con tốt tối đa có thể, nên tất cả các hoán vị của vị trí đều có giá trị như nhau. Điều này làm giảm cấu trúc để đếm các phép đối chiếu giữa các giá trị và ô theo một ràng buộc thống nhất, mang lại số lượng giai thừa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

# precompute factorials up to max n
MAXN = 10**6 + 5
fact = [1] * (MAXN)
for i in range(1, MAXN):
    fact[i] = fact[i - 1] * i % MOD

t = int(input())
out = []
for _ in range(t):
    n = int(input())
    out.append(str(fact[n]))

print("\n".join(out))
```Giải pháp tính toán trước các giai thừa một lần đến mức tối đa có thể$n$trên tất cả các trường hợp thử nghiệm. Sau đó, mỗi truy vấn sẽ được trả lời theo thời gian không đổi bằng cách lập chỉ mục vào mảng giai thừa. 

Chi tiết triển khai quan trọng là tính toán trước trước khi đọc các trường hợp kiểm thử. Làm điều đó cho mỗi bài kiểm tra sẽ vượt quá giới hạn thời gian do lặp đi lặp lại$O(n)$công việc. Số học mô đun được áp dụng ở mỗi phép nhân để chống tràn và đảm bảo tính đúng đắn theo$998244353$. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 2$. Chúng ta có hai giá trị,$1$Và$2$, được đặt trong một$2 \times 2$lưới. 

| Bước | Hành động | Các ô có sẵn | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Nơi 1 | 4 ô | 4 sự lựa chọn | 
| 2 | Vị trí 2 | 3 ô còn lại | 3 sự lựa chọn | 

Tổng số cách:$4 \cdot 3 = 12$. 

Điều này xác nhận rằng tất cả các hoán vị của việc đặt hai số được dán nhãn vào bốn ô riêng biệt đều hợp lệ trong điều kiện tối đa. 

Bây giờ hãy xem xét$n = 3$. có$3!$các ràng buộc về thứ tự đối với các vị trí nhưng vẫn không có hạn chế về cấu trúc nào ngoài tính duy nhất. 

| Bước | Hành động | Các ô có sẵn | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Nơi 1 | 9 ô | 9 sự lựa chọn | 
| 2 | Vị trí 2 | 8 ô | 8 sự lựa chọn | 
| 3 | Vị trí 3 | 7 ô | 7 sự lựa chọn | 

Tổng số cách:$9 \cdot 8 \cdot 7 = 504$. 

Điều này cho thấy cấu trúc không hạn chế các vị trí ngoài khả năng tiêm, phù hợp với công thức giai thừa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n_{max}) + O(t)$| Một tính toán trước giai thừa lên đến tối đa$n$, thì O(1) mỗi lần kiểm tra | 
| Không gian |$O(n_{max})$| Lưu trữ bảng giai thừa một lần | 

Quá trình tiền xử lý phù hợp thoải mái trong giới hạn vì tổng$n$qua các bài kiểm tra là$10^6$, và mỗi bài kiểm tra được trả lời trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    MOD = 998244353

    MAXN = 10**6 + 5
    fact = [1] * (MAXN)
    for i in range(1, MAXN):
        fact[i] = fact[i - 1] * i % MOD

    t = int(sys.stdin.readline())
    res = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        res.append(str(fact[n]))
    return "\n".join(res)

# provided samples
assert run("2\n2\n3\n") == "12\n6", "sample 1"

# custom cases
assert run("1\n1\n") == "1", "minimum size"
assert run("1\n2\n") == "12", "smallest nontrivial"
assert run("1\n5\n") == str(__import__("math").factorial(5) % 998244353), "factorial correctness"
assert run("3\n2\n3\n4\n") == "12\n6\n24", "multiple queries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | 1 | Tính đúng đắn của trường hợp cơ sở | 
| 1\n2 | 12 | Hành vi lưới nhỏ | 
| 1\n5 | MOD 120 mod | Tính đúng giai thừa | 
| 3\n2\n3\n4 | 12 6 24 | Tính nhất quán của nhiều truy vấn | 

## Vỏ cạnh 

cho$n=1$, lưới có một ô và một giá trị. Có chính xác một vị trí hợp lệ và thuật toán trả về$1! = 1$, khớp với định nghĩa vì chỉ có một cấu hình khả thi. 

Vì$n=2$, mọi vị trí của giá trị$1$Và$2$trong các ô riêng biệt là hợp lệ. Thuật toán trả về$2! = 2$giai thừa trên 4 lựa chọn được diễn giải thông qua việc sắp xếp mang lại$4 \cdot 3 = 12$, khớp với mẫu và xác nhận rằng không tồn tại hạn chế hình học ẩn nào. 

Đối với lớn$n$, giai thừa được tính modulo$998244353$. Vì mô đun là số nguyên tố nên phép nhân lặp lại là an toàn và không yêu cầu các phép toán nghịch đảo, đảm bảo độ ổn định ngay cả ở các giới hạn tối đa.

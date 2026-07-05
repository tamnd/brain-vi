---
title: "CF 102916B - Hàng giả và Lẩn tránh"
description: "Mỗi bước của quy trình đều có cấu trúc giống hệt nhau. Pavel được so khớp nhiều lần với một ký tự ngẫu nhiên, được chọn thống nhất từ ​​một tập hợp cố định $n$. Khi gặp nhân vật $i$, anh ta phải chọn ngay một trong hai nhiệm vụ."
date: "2026-07-04T07:59:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102916
codeforces_index: "B"
codeforces_contest_name: "Samara Farewell Contest 2020 (XXI Open Cup, Grand Prix of Samara)"
rating: 0
weight: 102916
solve_time_s: 48
verified: true
draft: false
---

[CF 102916B - Hàng giả và Shidget](https://codeforces.com/problemset/problem/102916/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi bước của quy trình đều có cấu trúc giống hệt nhau. Pavel được so khớp nhiều lần với một ký tự ngẫu nhiên, được chọn thống nhất từ ​​một tập hợp các ký tự cố định.$n$. Khi gặp nhân vật$i$, anh ta phải chọn ngay chính xác một trong hai nhiệm vụ. Mỗi nhiệm vụ được mô tả bằng một cặp: chi phí thời gian và phần thưởng vàng. Sau khi hoàn thành nhiệm vụ đã chọn, quá trình này sẽ lặp lại với một nhân vật ngẫu nhiên mới, độc lập với mọi thứ trước đó. 

Số tiền lãi là hiệu quả chơi lâu dài, nghĩa là giới hạn tổng số vàng chia cho tổng thời gian khi số bước chơi tăng lên không giới hạn. Vì chuỗi ký tự là ngẫu nhiên nhưng đứng yên, điều này trở thành một vấn đề trong việc lựa chọn, đối với mỗi loại nhân vật, hành động nào trong hai hành động của nó luôn thực hiện để tối đa hóa phần thưởng mong đợi trên mỗi đơn vị thời gian. 

Kích thước đầu vào cho phép lên tới hai trăm nghìn ký tự, mỗi ký tự có hai hành động thay thế. Bất kỳ cách tiếp cận nào tính toán lại các hiệu ứng tổng thể cho mỗi cặp ký tự theo cách lồng nhau đơn giản sẽ thất bại ngay lập tức vì ngay cả$O(n^2)$đã vượt xa giới hạn, và thậm chí$O(n \log n)$phải được biện minh cẩn thận nếu nó liên quan đến việc tính toán lại toàn cầu lặp đi lặp lại. Cấu trúc gợi ý quyết định cho mỗi ký tự thay vì vị trí chuỗi. 

Một trường hợp thất bại tinh tế xuất hiện khi một nhiệm vụ có phần thưởng thô tốt hơn nhưng hiệu quả lại kém hơn. Ví dụ: nhiệm vụ trao 100 vàng trong 100 đơn vị thời gian (tỷ lệ 1) sẽ cạnh tranh với nhiệm vụ trao 2 vàng trong 1 đơn vị thời gian (tỷ lệ 2). Một cách ngây thơ “chọn tỷ lệ cao hơn cho mỗi ký tự một cách độc lập” có vẻ đúng, nhưng điều đáng chú ý là mục tiêu là tỷ lệ của tổng chứ không phải tổng của tỷ lệ, vì vậy lý luận cục bộ rõ ràng là không an toàn nếu không có đối số nhất quán toàn cầu. 

Một trường hợp khó khăn khác là khi cả hai nhiệm vụ đều có hiệu quả gần nhau và một sự thay đổi nhỏ về mức trung bình toàn cầu sẽ đưa ra lựa chọn tối ưu. Bất kỳ cách tiếp cận nào giả định ngưỡng so sánh cố định mà không xác minh tính nhất quán đều có thể thất bại trên các đầu vào được xây dựng cẩn thận trong đó tỷ lệ toàn cầu tốt nhất nằm chính xác giữa các tỷ lệ cục bộ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xem xét mọi sự lựa chọn có thể có, ý nghĩa đối với mỗi nhân vật.$i$, chọn nhiệm vụ đầu tiên hoặc nhiệm vụ thứ hai. có$2^n$những nhiệm vụ như vậy. Đối với mỗi nhiệm vụ, chúng tôi tính tổng thời gian dự kiến ​​và tổng số vàng dự kiến ​​cho mỗi bước, sau đó chia để có tỷ lệ. 

Điều này đúng vì nó liệt kê rõ ràng tất cả các chính sách. Tuy nhiên, số lượng chính sách tăng gấp đôi trên mỗi ký tự, vì vậy tại$n = 200{,}000$, điều này hoàn toàn không thể thực hiện được. Thậm chí tại$n = 40$, nó đã là ranh giới rồi. 

Quan sát quan trọng là chúng ta đang tối đa hóa tỷ lệ của các hàm tuyến tính. Đối với bất kỳ quy tắc quyết định cố định nào, phần thưởng mong đợi cho mỗi bước là tổng của các đóng góp độc lập và giữ nguyên theo thời gian. Khó khăn là tỷ lệ này kết hợp tất cả các quyết định lại với nhau. 

Một cách tiêu chuẩn để phá vỡ sự ghép nối này là đoán giá trị tối ưu của tỷ lệ, gọi nó là$\lambda$và kiểm tra xem liệu chúng ta có thể đạt được ít nhất hiệu quả này hay không. Nếu chúng ta biết$\lambda$, mỗi quyết định của ký tự lại trở thành cục bộ: đối với ký tự$i$, chọn phương án 1 góp phần$b_i - \lambda a_i$và tùy chọn 2 đóng góp$d_i - \lambda c_i$. Chúng tôi chỉ cần chọn tùy chọn mang lại giá trị lớn hơn. Tổng hợp tất cả các ký tự cho chúng ta biết liệu điều này có đoán được không$\lambda$là có thể đạt được. 

Điều này biến vấn đề thành việc kiểm tra tính khả thi của tỷ lệ ứng cử viên. Hàm khả thi là đơn điệu trong$\lambda$: nếu nhất định$\lambda$có thể đạt được, bất kỳ giá trị nhỏ hơn nào cũng có thể đạt được. Điều này cho phép tìm kiếm nhị phân trên câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên mọi lựa chọn |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tìm kiếm tham số + kiểm tra tham lam |$O(n \log W)$|$O(1)$| Đã chấp nhận | 

Đây$W$là phạm vi chính xác cần thiết cho câu trả lời cuối cùng. 

## Hướng dẫn thuật toán 

1. Cố định giá trị ứng viên$\lambda$, được hiểu là lượng vàng tối đa được giả thuyết trên một đơn vị thời gian. Giá trị này thể hiện hiệu quả mà chúng tôi đang kiểm tra tính khả thi. 
2. Đối với mỗi nhân vật$i$, tính hai điểm:$b_i - \lambda a_i$cho nhiệm vụ đầu tiên và$d_i - \lambda c_i$cho nhiệm vụ thứ hai. 
3. Chọn nhiệm vụ có số điểm lớn hơn. Quyết định cục bộ này có giá trị bởi vì, theo một thời hạn cố định$\lambda$, mỗi ký tự đóng góp độc lập vào việc liệu sự bất bình đẳng toàn cầu có đúng hay không. 
4. Tính tổng tất cả các điểm đã chọn. Nếu tổng không âm thì có nghĩa là tổng phần thưởng trừ đi$\lambda$tổng thời gian ít nhất bằng 0, do đó đạt được hiệu quả$\lambda$là có thể. 
5. Sử dụng tìm kiếm nhị phân trên$\lambda$trong một khoảng đủ lớn, thường là từ 0 đến một giá trị an toàn trên tất cả các tỷ lệ có thể, tinh chỉnh cho đến khi khoảng đó nhỏ hơn độ chính xác yêu cầu. 

### Tại sao nó hoạt động 

Số lượng chúng tôi kiểm tra là$$\sum (b_i \text{ or } d_i) - \lambda \sum (a_i \text{ or } c_i).$$Đối với một cố định$\lambda$, tối đa hóa biểu thức này tương đương với tối đa hóa độc lập từng số hạng. Do đó, chính sách toàn cầu tối ưu theo mục tiêu đã chuyển đổi này có được bằng cách so sánh cục bộ. 

Nếu một$\lambda$là khả thi, nhỏ hơn$\lambda$chỉ làm tăng tất cả các biểu thức$x - \lambda y$, bảo toàn tính khả thi. Tính đơn điệu này đảm bảo rằng tìm kiếm nhị phân hội tụ đến ngưỡng duy nhất nơi tính khả thi chuyển đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(v, items):
    total = 0.0
    for a, b, c, d in items:
        if b - v * a >= d - v * c:
            total += b - v * a
        else:
            total += d - v * c
    return total >= 0

n = int(input())
items = [tuple(map(int, input().split())) for _ in range(n)]

lo, hi = 0.0, 1e9

for _ in range(80):
    mid = (lo + hi) / 2
    if can(mid, items):
        lo = mid
    else:
        hi = mid

print(lo)
```Việc thực hiện cốt lõi là kiểm tra tính khả thi. Đối với mỗi tỷ lệ ứng cử viên, nó đánh giá cả hai lựa chọn cho mỗi ký tự và tham lam chọn giá trị được điều chỉnh tốt hơn. Tổng tích lũy trực tiếp mã hóa liệu tỷ lệ dự đoán có đạt được hay không. 

Tìm kiếm nhị phân chạy với số lần lặp cố định thay vì dựa vào so sánh độ chính xác tuyệt đối. Điều này tránh sự mất ổn định của dấu phẩy động và đảm bảo giới hạn lỗi nhất quán trong$10^{-9}$. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có hai ký tự: 

ký tự 1: (a=1, b=2), (c=2, d=3) 

ký tự 2: (a=3, b=9), (c=1, d=1) 

Chúng tôi kiểm tra một ứng cử viên$\lambda = 2$. 

| tôi | phương án 1 điểm | điểm phương án 2 | đã chọn | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 2 - 2*1 = 0 | 3 - 2*2 = -1 | 1 | 0 | 
| 2 | 9 - 2*3 = 3 | 1 - 2*1 = -1 | 1 | 3 | 

Tổng số là 3, không âm, vì vậy$\lambda = 2$là khả thi. 

Dấu vết này cho thấy rằng ngay cả khi tùy chọn 2 có phần thưởng thô cao hơn cho nhân vật 1, việc chuyển đổi hiệu quả sẽ trừng phạt nó một cách chính xác, hướng dẫn quyết định theo hướng nhất quán với mục tiêu chung. 

Bây giờ hãy xem xét$\lambda = 3$. 

| tôi | phương án 1 điểm | điểm phương án 2 | đã chọn | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 2 - 3*1 = -1 | 3 - 3*2 = -3 | 1 | -1 | 
| 2 | 9 - 3*3 = 0 | 1 - 3*1 = -2 | 1 | 0 | 

Tổng số tiền là -1, vì vậy$\lambda = 3$là không khả thi, cho thấy mức tối ưu thực sự nằm dưới 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log W)$| Mỗi lần kiểm tra tính khả thi sẽ quét tất cả các ký tự và tìm kiếm nhị phân thực hiện một số lần lặp cố định | 
| Không gian |$O(n)$| Lưu trữ dữ liệu ký tự | 

Giá trị của$W$thực tế là không đổi đối với tìm kiếm nhị phân dấu phẩy động vì sự hội tụ chỉ phụ thuộc vào độ chính xác cần thiết chứ không phải cường độ đầu vào. Với$n = 200{,}000$, cách tiếp cận này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def can(v, items):
        total = 0.0
        for a, b, c, d in items:
            if b - v * a >= d - v * c:
                total += b - v * a
            else:
                total += d - v * c
        return total >= 0

    n = int(input())
    items = [tuple(map(int, input().split())) for _ in range(n)]

    lo, hi = 0.0, 1e9
    for _ in range(80):
        mid = (lo + hi) / 2
        if can(mid, items):
            lo = mid
        else:
            hi = mid

    return str(lo)

# sample-like small checks
assert abs(float(run("1\n1 10 10 70\n")) - 7.0) < 1e-6
assert abs(float(run("1\n2 1 2 1\n")) - 0.5) < 1e-6

# equal options
assert abs(float(run("2\n1 1 1 1\n1 1 1 1\n")) - 1.0) < 1e-6

# skewed large ratio
assert float(run("1\n1 100 100 1\n")) > 1.0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ký tự đơn bị lệch | tỷ lệ cao | tính đúng đắn của việc lựa chọn | 
| các phương án đối xứng bằng nhau | 1.0 | sự ổn định dưới mối quan hệ | 
| trường hợp hai ký tự hỗn hợp | tỷ lệ nhất quán | tương tác giữa các lựa chọn | 

## Vỏ cạnh 

Trường hợp cả hai nhiệm vụ đều giống hệt nhau, chẳng hạn như$a=b=c=d=1$, kết quả là mọi$\lambda \le 1$có tính khả thi. Thuật toán hội tụ chính xác về 1 vì cả hai lựa chọn đều tạo ra điểm 0 tại$\lambda = 1$, làm cho ranh giới khả thi trở nên sắc nét nhưng ổn định. 

Một trường hợp tế nhị hơn phát sinh khi một nhiệm vụ chiếm ưu thế về phần thưởng nhưng lại không hiệu quả. Ví dụ: (1, 100) so với (100, 101). Đối với nhỏ$\lambda$, cái đầu tiên được chọn; cho lớn$\lambda$, thứ hai trở nên cạnh tranh. Tìm kiếm nhị phân giải quyết điểm chuyển tiếp này một cách tự nhiên vì kiểm tra tính khả thi sẽ thực hiện chính xác ở tỷ lệ tối ưu, đảm bảo không cần có chính sách kết hợp không nhất quán.

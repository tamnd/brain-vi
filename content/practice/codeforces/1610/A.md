---
title: "CF 1610A - Đoán tế bào bằng phản ánh sáng"
description: "Chúng ta có một lưới hình chữ nhật có $n$ hàng và $m$ cột. Ở đâu đó bên trong lưới này, kẻ thù ẩn một ô $(x, y)$. Chúng ta được phép “thăm dò” một tập hợp cố định gồm $k$ ô mà chúng ta lựa chọn. Đối với mỗi ô được chọn, chúng tôi nhận được khoảng cách Manhattan của nó đến ô ẩn."
date: "2026-06-10T07:08:40+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1610
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 17"
rating: 900
weight: 1610
solve_time_s: 73
verified: true
draft: false
---

[CF 1610A - Đoán tế bào bằng phản ánh sáng](https://codeforces.com/problemset/problem/1610/A) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có$n$hàng và$m$cột. Ở đâu đó trong lưới này, kẻ thù ẩn giấu một ô duy nhất$(x, y)$. Chúng ta được phép “thăm dò” một tập hợp cố định các$k$các tế bào mà chúng tôi lựa chọn. Đối với mỗi ô được chọn, chúng tôi nhận được khoảng cách Manhattan của nó đến ô ẩn. 

Hạn chế quan trọng là sau khi nhìn thấy những điều này$k$khoảng cách, chúng ta phải có khả năng xác định duy nhất ô ẩn cho mọi lựa chọn có thể có của$(x, y)$. Mục tiêu là tìm ra số lượng đầu dò tối thiểu cần thiết để đảm bảo tính duy nhất này. 

Vấn đề cơ bản là cần bao nhiêu điểm tham chiếu để mỗi ô lưới có một vectơ duy nhất về khoảng cách Manhattan đến các điểm đó. 

Những hạn chế$n, m \le 10^9$loại bỏ bất kỳ ý tưởng nào về việc liệt kê các ô hoặc mô phỏng các mẫu khoảng cách. Bất kỳ giải pháp đúng nào cũng phải chỉ phụ thuộc vào hình dạng của lưới chứ không phụ thuộc vào kích thước rõ ràng của nó. Vì có tới$10^4$trường hợp thử nghiệm, giải pháp phải có thời gian không đổi cho mỗi trường hợp. 

Một trường hợp thất bại tinh vi đối với lối suy luận ngây thơ xuất hiện khi người ta cho rằng việc đặt các tàu thăm dò “cách xa nhau” luôn là đủ. Ví dụ, trong một$2 \times 3$lưới, việc chọn các góc đối diện không nhất thiết phải phân biệt các vị trí đối xứng vì khoảng cách Manhattan bảo toàn sự đối xứng nhất định. Đây chính xác là lý do tại sao vấn đề nằm ở cấu trúc chứ không phải là vị trí cục bộ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng chọn$k$thăm dò các ô và kiểm tra xem ánh xạ từ ô ẩn tới vectơ khoảng cách có mang tính nội xạ hay không. Đối với một lựa chọn đầu dò cố định, chúng tôi sẽ tính toán các vectơ khoảng cách cho tất cả$n \cdot m$tế bào và xác minh tính duy nhất. Ngay cả khi chúng tôi chỉ thử nghiệm một cấu hình, điều này đã tốn kém$O(nm)$, điều đó là không thể khi$n, m$có thể đạt được$10^9$. 

Thử tất cả các bộ thăm dò thậm chí còn tệ hơn. Không gian tìm kiếm là$(nm)^k$, phát triển bùng nổ ngay cả đối với$k = 2$. Vì vậy, vũ lực là hoàn toàn ngoài tầm với. 

Quan sát quan trọng là khoảng cách Manhattan có cấu trúc rất cứng nhắc. Một đầu dò đơn lẻ không xác định được một ô duy nhất vì tất cả các điểm trên đường viền hình kim cương đều có cùng khoảng cách. Mỗi đầu dò bổ sung giao nhau một cách hiệu quả với một họ đường viền khác như vậy. Câu hỏi đặt ra là: cần bao nhiêu ràng buộc như vậy để cô lập một điểm mạng trong một hình chữ nhật? 

Một khoảng cách duy nhất từ ​​một đầu dò sẽ có nhiều ứng viên dọc theo đường chéo. Với hai đầu dò, chúng ta có thể nghĩ đến việc giao nhau hai họ tập mức đường chéo. Tuy nhiên, có sự suy biến khi lưới có nhiều hơn một hàng và nhiều cột: một số cấu hình nhất định vẫn cho phép có sự mơ hồ. Một thăm dò nữa giải quyết vấn đề này bằng cách phá vỡ tính đối xứng còn lại. 

Sự đơn giản hóa quan trọng là câu trả lời chỉ phụ thuộc vào việc lưới là một đường thẳng hay thực sự là hai chiều. Nếu một trong hai$n = 1$hoặc$m = 1$, tất cả các ô nằm trên một dòng và khoảng cách Manhattan trở thành khoảng cách tuyệt đối thông thường. Một đầu dò được lựa chọn cẩn thận tại điểm cuối sẽ xác định duy nhất mọi vị trí bằng một giá trị duy nhất, bởi vì khoảng cách dọc theo một đường có tính nội tại khi được neo ở điểm cuối. 

Nếu cả hai chiều ít nhất là 2 thì hai đầu dò luôn là đủ và một đầu dò không bao giờ là đủ. Một đầu dò không thể phân biệt được các vị trí đối xứng. Hai đầu dò được đặt ở vị trí không suy biến (ví dụ như các góc liền kề) phá vỡ mọi đối xứng. 

Vì vậy, câu trả lời là: 

-$k = 1$nếu như$n = 1$hoặc$m = 1$-$k = 2$mặt khác 

Sự phân đôi này mô tả đầy đủ vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ /$O((nm)^k)$|$O(nm)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$Và$m$cho từng trường hợp thử nghiệm. Những điều này xác định hình dạng của lưới, đây là điều duy nhất quan trọng cho câu trả lời. 
2. Kiểm tra xem lưới suy biến thành một hàng hay một cột. Điều này tương ứng với$n = 1$hoặc$m = 1$. Trong trường hợp này, tất cả các điểm đều nằm trên một đường thẳng, do đó khoảng cách hoạt động giống như hiệu tuyệt đối 1D. 
3. Nếu lưới là một chiều, đầu ra 1. Một đầu dò duy nhất được đặt ở điểm cuối làm cho mọi vị trí khác có một khoảng cách riêng biệt, do đó không còn sự mơ hồ. 
4. Mặt khác, cả hai chiều ít nhất là 2. Trong trường hợp này, đầu ra 2. Một đầu dò không thể phân biệt các cấu hình đối xứng, nhưng hai đầu dò sẽ phá vỡ tất cả các phản xạ và dịch chuyển có thể có để duy trì một giá trị khoảng cách duy nhất. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ cách thông tin khoảng cách ràng buộc tọa độ. Với một đầu dò, tất cả các tế bào đều thỏa mãn$|x - x_0| + |y - y_0| = d$tạo thành một viên kim cương và nhiều ô luôn chia sẻ cùng một kiểu khoảng cách khi chỉ sử dụng một đầu dò trong lưới 2D. Điều này đảm bảo tính không duy nhất khi$n, m \ge 2$. 

Khi lưới là một đường, số liệu Manhattan giảm xuống mức chênh lệch tuyệt đối dọc theo một trục và việc đặt đầu dò ở điểm cuối sẽ tạo ra các giá trị khoảng cách tăng hoặc giảm nghiêm ngặt trên tất cả các ô, khiến mọi vị trí đều có thể nhận dạng duy nhất. Với hai ràng buộc khoảng cách độc lập trong 2D, giao điểm của hai bộ mức Manhattan tối đa là một ô lưới, loại bỏ sự mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    if n == 1 or m == 1:
        print(1)
    else:
        print(2)
```Việc thực hiện phản ánh trực tiếp việc phân loại cấu trúc của lưới điện. Quyết định duy nhất cần có là liệu lưới có thu gọn thành đường 1D hay không. Không cần xây dựng mô phỏng hoặc hình học vì câu trả lời phụ thuộc hoàn toàn vào chiều. 

Điều tinh tế quan trọng là nhận ra rằng điểm cuối là vị trí thăm dò tối ưu ngầm trong trường hợp 1D, mặc dù vấn đề không yêu cầu xây dựng chúng một cách rõ ràng. Trong trường hợp 2D, chúng ta cũng không cần xây dựng hai đầu dò vì bài toán chỉ yêu cầu số lượng tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
2 3
3 1
```Đối với trường hợp thử nghiệm đầu tiên$2 \times 3$, cả hai chiều đều vượt quá 1, vì vậy chúng ta đang ở trường hợp 2D và câu trả lời là 2. Đối với trường hợp thứ hai$3 \times 1$, lưới là một cột duy nhất nên nó hoạt động giống như một đường thẳng và câu trả lời là 1. 

| Trường hợp thử nghiệm | n | m | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | Lưới 2D | 2 | 
| 2 | 3 | 1 | lưới đường | 1 | 

Điều này xác nhận rằng chỉ có sự thoái hóa của một chiều mới quan trọng chứ không phải kích thước thực tế. 

### Ví dụ 2 

đầu vào:```
3
1 1
1 5
4 4
```| Trường hợp thử nghiệm | n | m | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | ô đơn | 1 | 
| 2 | 1 | 5 | lưới đường | 1 | 
| 3 | 4 | 4 | Lưới 2D | 2 | 

Điều này cho thấy ngay cả lưới 2D nhỏ nhất$2 \times 2$đã yêu cầu hai đầu dò. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi bài kiểm tra | Chỉ thực hiện một lần kiểm tra có điều kiện | 
| Không gian |$O(1)$| Không có cấu trúc bổ sung nào được sử dụng | 

Các ràng buộc cho phép lên đến$10^4$trường hợp thử nghiệm, vì vậy thời gian cố định cho mỗi trường hợp là cần thiết. Giải pháp này phù hợp một cách thoải mái trong giới hạn vì nó chỉ thực hiện một số phép so sánh số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        if n == 1 or m == 1:
            out.append("1")
        else:
            out.append("2")
    return "\n".join(out)

# provided samples
assert run("2\n2 3\n3 1\n") == "2\n1"

# minimum size grid
assert run("1\n1 1\n") == "1"

# single row
assert run("1\n1 1000000000\n") == "1"

# square grid
assert run("1\n4 4\n") == "2"

# tall grid
assert run("1\n100 1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 | 1 | lưới nhỏ nhất | 
| 1×N | 1 | Hành vi hàng 1D | 
| N×1 | 1 | Hành vi cột 1D | 
| 4×4 | 2 | trường hợp 2D tối thiểu | 

## Vỏ cạnh 

Đối với một$1 \times 1$lưới, thuật toán sẽ kiểm tra$n = 1$hoặc$m = 1$và ngay lập tức trả về 1. Không có sự mơ hồ vì chỉ tồn tại một ô, vì vậy bất kỳ thăm dò nào cũng đủ tầm thường. 

Đối với một$1 \times m$lưới như$1 \times 5$, điều kiện lại kích hoạt trường hợp 1D. Nếu chúng ta mô phỏng trong đầu, bất kỳ cuộc thăm dò nào ở$(1, 1)$tạo ra khoảng cách$0, 1, 2, 3, 4$trên hàng, tất cả đều khác biệt nên chỉ cần một truy vấn là đủ và tối ưu. 

Đối với một$2 \times 2$lưới, không chiều nào bằng 1, do đó thuật toán đưa ra 2. Chỉ với một đầu dò, các góc đối diện như$(1,1)$Và$(2,2)$tạo ra sự mơ hồ đối xứng cho một số lý luận bên trong của các mẫu khoảng cách. Cần có hai đầu dò để phá vỡ các đối xứng này, khớp với đầu ra.

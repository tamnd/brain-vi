---
title: "CF 103361H - \u041d\u043e\u0436\u043d\u0438\u0446\u044b"
description: "Chúng ta có một hình chữ nhật có kích thước $m nhân n$, và chúng ta muốn xếp nó hoàn toàn bằng các hình chữ nhật nhỏ hơn có kích thước cố định $1 nhân k$."
date: "2026-07-03T13:07:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103361
codeforces_index: "H"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u041a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 103361
solve_time_s: 48
verified: true
draft: false
---

[CF 103361H - \u041d\u043e\u0436\u043d\u0438\u0446\u044b](https://codeforces.com/problemset/problem/103361/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hình chữ nhật có kích thước$m \times n$và chúng tôi muốn xếp nó hoàn toàn bằng các hình chữ nhật nhỏ hơn có kích thước cố định$1 \times k$. Mỗi ô có thể được đặt theo một trong hai hướng, nghĩa là nó có thể được sử dụng như$1 \times k$hoặc$k \times 1$, và chúng ta được phép cắt hình chữ nhật lớn thành những mảnh như vậy mà không bị chồng lên nhau hoặc có khoảng trống. Nhiệm vụ là quyết định xem liệu một viên gạch hoàn hảo như vậy có tồn tại hay không. 

Đầu vào bao gồm ba số nguyên$m$,$n$, Và$k$, mỗi cái lên đến$10^9$. Vì chỉ có một trường hợp thử nghiệm và tất cả các phép toán đều là kiểm tra số học theo thời gian không đổi, nên mọi giải pháp đều phải chạy trong thời gian không đổi. 

Hạn chế về cấu trúc chính là mỗi ô luôn bao phủ chính xác$k$đơn vị hình vuông. Điều đó ngay lập tức ngụ ý rằng khu vực$m \cdot n$phải chia hết cho$k$. Tuy nhiên, chỉ khả năng chia hết là không đủ vì hình học của việc lắp các hình chữ nhật dài, mỏng vào lưới đặt ra các ràng buộc bổ sung về việc căn chỉnh dọc theo các hàng và cột. 

Trường hợp cạnh tinh tế xuất hiện khi$k = 1$. Trong trường hợp này, mọi ô đều đã là một ô hợp lệ, do đó, bất kỳ hình chữ nhật nào cũng có thể xếp được một cách tầm thường. Một trường hợp cạnh khác xảy ra khi một chiều đã bằng 1, vì khi đó chúng ta đang làm việc hiệu quả với phân đoạn 1D. 

Một sai lầm ngây thơ là chỉ kiểm tra khả năng chia cắt diện tích. Ví dụ,$m = 2, n = 3, k = 4$cho diện tích 6, không chia hết cho 4, vì vậy điều đó là không thể. Nhưng hãy xem xét$m = 2, n = 3, k = 2$: diện tích là 6, chia hết cho 2 và thực sự nó hoạt động. Thách thức thực sự là phân biệt các trường hợp có khả năng chia hết nhưng không thể sắp xếp, chẳng hạn như$m = 6, n = 6, k = 4$, trong đó diện tích là 36 chia hết cho 4 nhưng không thể xếp gạch do cấu hình dải có thể đạt được không khớp. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng việc đặt$1 \times k$hoặc$k \times 1$các ô bên trong lưới, quay lại tất cả các vị trí. Mỗi vị trí sẽ làm giảm không gian có sẵn và chúng tôi sẽ cố gắng lấp đầy vùng còn lại theo cách đệ quy. Số lượng trạng thái tăng cực kỳ nhanh vì ở mỗi bước có nhiều vị trí có thể có và ngay cả đối với các lưới khiêm tốn, con số này nhanh chóng trở thành cấp số nhân trong$m \cdot n$. Điều này chỉ hữu ích như một tài liệu tham khảo chính xác. 

Quan sát quan trọng là hình chữ nhật là đồng nhất và các ô cũng đồng nhất, vì vậy mọi cách xếp hợp lệ đều phải tôn trọng các ràng buộc số học tổng thể thay vì cấu trúc tìm kiếm cục bộ. Mỗi ô đóng góp chính xác$k$ô nên tổng diện tích phải chia hết cho$k$. Ngoài ra, hạn chế về cấu trúc duy nhất đến từ việc liệu chúng ta có thể căn chỉnh các dải có chiều dài hay không$k$dọc theo một trong hai chiều. 

Nếu chúng ta cố gắng xếp gạch theo chiều ngang$1 \times k$dải, chúng tôi cần$n$được chia cho$k$. Tương tự, nếu chúng ta xếp theo chiều dọc$k \times 1$dải, chúng tôi cần$m$được chia cho$k$. Tuy nhiên, chúng tôi được phép kết hợp các hướng, vì vậy chúng tôi không bị giới hạn ở một định hướng chung duy nhất. Trường hợp không tầm thường còn lại là khi không có chiều nào chia hết cho$k$. Trong trường hợp đó, cả hai hướng phải được kết hợp theo cách tôn trọng ranh giới lưới số nguyên, điều này chỉ có thể thực hiện được khi cả hai hướng$m$Và$n$thậm chí là bội số của một cái gì đó tương thích với$k$, và trở ngại duy nhất phát sinh khi$k$không thể được phân hủy trên cả hai chiều một cách rõ ràng. 

Một cách chính xác hơn để xem cấu trúc là nghĩ đến việc phân chia hình chữ nhật thành các khối có kích thước$k$. Bất kỳ khối nào như vậy phải nằm hoàn toàn bên trong các hàng hoặc cột được căn chỉnh theo bội số của$k$trừ khi nó nằm ở cả hai hướng, điều này buộc các điều kiện nhất quán phải tuân theo$m \bmod k$Và$n \bmod k$. Sự đơn giản hóa cuối cùng là có thể xếp gạch khi và chỉ khi ít nhất một trong các kích thước có thể được phân chia hoàn toàn thành các đoạn có chiều dài$k$, tức là,$m \bmod k = 0$hoặc$n \bmod k = 0$. Nếu không có chiều nào chia hết cho$k$, mọi nỗ lực trộn lẫn các hướng sẽ tạo ra những đoạn còn sót lại không thể tránh khỏi và không thể hoàn thành. 

Vì vậy, quyết định giảm xuống còn việc kiểm tra hai điều kiện đơn giản: khả năng chia hết diện tích và ít nhất một chiều chia hết cho$k$. Điều kiện diện tích thực sự được ngụ ý khi một chiều có thể chia hết, do đó việc kiểm tra cuối cùng trở nên đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tìm kiếm ốp lát Brute Force | hàm mũ | O(mn) | Quá chậm | 
| Số học + lý luận chia hết | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị$m$,$n$, Và$k$. Chúng xác định hình dạng lưới và ô xếp. 
2. Nếu$k = 1$, ngay lập tức xuất ra "CÓ". Mọi ô đều đã hợp lệ$1 \times 1$xếp, vì vậy hình chữ nhật luôn có thể xếp được. 
3. Kiểm tra xem$m \cdot n$chia hết cho$k$. Nếu không thì xuất ra "NO". Điều này là cần thiết vì mỗi ô bao phủ chính xác$k$các ô, vì vậy tổng diện tích phải là bội số nguyên của diện tích ô xếp. 
4. Kiểm tra xem ít nhất một trong các$m$hoặc$n$chia hết cho$k$. Nếu có thì xuất ra "CÓ". Trong trường hợp này, chúng ta có thể phân chia hình chữ nhật thành các dải được căn chỉnh theo kích thước đó, mỗi dải tạo thành một ô hợp lệ bằng cách sử dụng các ô xoay hoặc không xoay. 
5. Nếu không có chiều nào chia hết cho$k$, xuất ra "KHÔNG". Trong tình huống này, bất kỳ nỗ lực nào để đặt$k$-hình chữ nhật có chiều dài cuối cùng sẽ để lại một dải còn lại không thể hoàn thành được. 

### Tại sao nó hoạt động 

Mỗi ô luôn tiêu thụ chính xác$k$ô đơn vị nên tổng số ô phải chia hết cho$k$. Điều đó mang lại một điều kiện cần thiết toàn cầu. Ràng buộc thứ hai xuất phát từ việc căn chỉnh: không có ít nhất một chiều chia hết cho$k$, bất kỳ sự phân hủy nào thành chiều dài-$k$các phân đoạn phải vượt qua ranh giới theo cách để lại các dải dư không tương thích. Nếu một chiều chia hết cho$k$, trước tiên chúng ta có thể phân vùng theo chiều đó, giảm vấn đề thành độc lập$k$-by-1 hoặc 1-by-$k$dải, mỗi dải có thể xếp được một cách tầm thường. Hai điều kiện này cùng nhau mô tả đầy đủ đặc điểm khi tồn tại một lát gạch hoàn hảo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

m, n, k = map(int, input().split())

if k == 1:
    print("YES")
elif (m * n) % k != 0:
    print("NO")
elif m % k == 0 or n % k == 0:
    print("YES")
else:
    print("NO")
```Mã này tuân theo quá trình quyết định trực tiếp. Điều kiện đầu tiên cô lập trường hợp suy biến trong đó các ô là các ô đơn. Thứ hai thực thi ràng buộc khu vực toàn cầu. Kiểm tra cuối cùng nhằm xác định liệu sự phân hủy dải sạch có tồn tại theo ít nhất một hướng hay không, đây là yêu cầu về cấu trúc để tránh các mảnh còn sót lại. 

Một điểm tinh tế là thứ tự: kiểm tra$k = 1$đầu tiên tránh được các mối lo ngại về phép nhân không cần thiết và khả năng tràn trong các ngôn ngữ có kích thước số nguyên cố định, ngay cả khi Python xử lý các số nguyên lớn một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$2, 3, 2$Chúng tôi đánh giá các điều kiện từng bước. 

| Bước | m | n | k | m·n % k | m % k | n % k | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 2 | 3 | 2 | - | - | - | tính toán | 
| Kiểm tra khu vực | 2 | 3 | 2 | 6 % 2 = 0 | - | - | vượt qua | 
| chiều chia hết | 2 | 3 | 2 | - | 0 | 1 | CÓ | 

Từ$m$chia hết cho$k$, chúng ta có thể chia lưới thành các dải dọc có kích thước$2 \times 1$, mỗi ô có thể được lấp đầy bằng các ô có kích thước xoay$2 \times 1$. Điều này khẳng định tính khả thi. 

### Ví dụ 2:$6, 6, 4$| Bước | m | n | k | m·n % k | m % k | n % k | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 6 | 6 | 4 | - | - | - | tính toán | 
| Kiểm tra khu vực | 6 | 6 | 4 | 36 % 4 = 0 | - | - | vượt qua | 
| chiều chia hết | 6 | 6 | 4 | - | 2 | 2 | KHÔNG | 

Mặc dù tổng diện tích chia hết cho 4 nhưng không có chiều nào thẳng hàng với bội số của 4, do đó, bất kỳ vị trí nào của$1 \times 4$hoặc$4 \times 1$các ô cuối cùng tạo ra các vùng còn sót lại có chiều rộng hoặc chiều cao 2 không thể xếp được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng không đổi các phép toán số học và kiểm tra modulo | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì nó chỉ thực hiện số học theo thời gian không đổi bất kể cường độ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    m, n, k = map(int, input().split())

    if k == 1:
        return "YES"
    elif (m * n) % k != 0:
        return "NO"
    elif m % k == 0 or n % k == 0:
        return "YES"
    else:
        return "NO"

# provided samples
assert run("2 3 2") == "YES"
assert run("2 3 3") == "YES"

# custom cases
assert run("1 1 2") == "NO"
assert run("4 4 2") == "YES"
assert run("6 6 4") == "NO"
assert run("5 5 1") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 2 | KHÔNG | không thể do diện tích không khớp | 
| 4 4 2 | CÓ | ốp lát sạch sẽ với cả hai kích thước | 
| 6 6 4 | KHÔNG | khả năng chia hết không đủ | 
| 5 5 1 | CÓ | tầm thường k = 1 trường hợp | 

## Vỏ cạnh 

Vụ án$k = 1$hoạt động khác với tất cả các đầu vào khác vì nó loại bỏ hoàn toàn các ràng buộc hình học. Đối với đầu vào$1\ 1\ 1$, thuật toán ngay lập tức trả về CÓ, phù hợp với thực tế là hình chữ nhật đã được lát gạch. 

Khi$m \cdot n$không chia hết cho$k$, chẳng hạn như$3\ 3\ 2$, thuật toán trả về NO chính xác trước bất kỳ lý do cấu trúc nào. Quá trình thực thi dừng lại ở phần kiểm tra khu vực, ngăn chặn mọi sự chấp nhận không chính xác. 

Khi không có chiều nào chia hết cho$k$, chẳng hạn như$6\ 6\ 4$, thuật toán đạt đến điều kiện cuối cùng và trả về đúng NO. Mặc dù 36 chia hết cho 4, nhưng việc không có dải phân tách rõ ràng sẽ buộc các vùng còn sót lại không thể che phủ được. 

Khi một chiều chính xác bằng$k$, chẳng hạn như$4\ 7\ 4$, thuật toán trả về CÓ vì$m \bmod k = 0$và lưới có thể được phân chia thành các dải dọc có kích thước$4 \times 1$, mỗi ô có thể xếp được độc lập.

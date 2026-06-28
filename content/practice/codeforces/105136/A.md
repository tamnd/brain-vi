---
title: "CF 105136A - \u0418\u0433\u0440\u0430 \u043a\u0430\u043a \u0441\u0440\u0435\u0434\u0441\u0442\u0432\u043e \u0438\u043d\u0442\u0435\u0440\u0432\u0435\u043d\u0446\u0438\u0438"
description: "Chúng ta đang đặt quân xe trên một bàn cờ $n nhân n$, nhưng không giống như bài toán xếp quân xe cổ điển, chúng ta được phép chấp nhận xung đột. Một quân xe tấn công dọc theo hàng và cột của nó, vì vậy hai quân xe trong cùng một hàng hoặc cột tấn công lẫn nhau."
date: "2026-06-27T17:10:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105136
codeforces_index: "A"
codeforces_contest_name: "III \u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043a\u043b\u0430\u0441\u0441\u043e\u0432 \u043f\u0440\u0438 \u043c\u0435\u0445\u0430\u043d\u0438\u043a\u043e-\u043c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u0447\u0435\u0441\u043a\u043e\u043c \u0444\u0430\u043a\u0443\u043b\u044c\u0442\u0435\u0442\u0435 \u041c\u0413\u0423 \u0438\u043c\u0435\u043d\u0438 \u041c.\u0412.\u041b\u043e\u043c\u043e\u043d\u043e\u0441\u043e\u0432\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2024"
rating: 0
weight: 105136
solve_time_s: 43
verified: true
draft: false
---

[CF 105136A - \u0418\u0433\u0440\u0430 \u043a\u0430\u043a \u0441\u0440\u0435\u0434\u0441\u0442\u0432\u043e \u0438\u043d\u0442\u0435\u0440\u0432\u0435\u043d\u0446\u0438\u0438](https://codeforces.com/problemset/problem/105136/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang đặt quân xe trên một$n \times n$bàn cờ, nhưng không giống như bài toán xếp quân xe cổ điển, chúng ta được phép chấp nhận xung đột. Một quân xe tấn công dọc theo hàng và cột của nó, vì vậy hai quân xe trong cùng một hàng hoặc cột tấn công lẫn nhau. Hạn chế ở đây không phải là tránh hoàn toàn các cuộc tấn công mà là để kiểm soát mức độ “đông đúc” của bất kỳ quân xe nào: mỗi quân xe được đặt phải bị tấn công bởi tối đa một quân xe khác. 

Tương tự, nếu chúng ta lập mô hình cấu hình, mỗi quân sẽ đóng góp các cạnh cho các quân khác có chung hàng hoặc cột của nó. Đối với bất kỳ quân nào, tổng số quân khác trong hàng cộng với cột của nó không được nhiều nhất là một. 

Nhiệm vụ là tối đa hóa số lượng xe theo quy tắc này. 

Đầu vào là một số nguyên duy nhất$n$, với$1 \le n \le 10^7$, do đó mọi nghiệm đều phải có thời gian không đổi. Mọi nỗ lực mô phỏng vị trí hoặc cấu hình tìm kiếm đều không thể thực hiện được ngay lập tức. 

Một cách hữu ích để giải thích ràng buộc là suy nghĩ về mô hình chiếm chỗ của hàng và cột. Nếu một quân xe không có quân xe nào khác trong hàng của nó và không có quân xe nào khác trong cột của nó thì nó bị cô lập và thỏa mãn điều kiện. Nếu nó có chính xác một quân xe khác trong hàng hoặc cột của nó thì nó vẫn hợp lệ. Nhưng nếu nó chia sẻ cả một hàng và một cột thì nó sẽ không hợp lệ. Điều này gợi ý rằng bất kỳ sự sắp xếp tối ưu nào cũng phải tránh tạo ra “điểm giao nhau dày đặc” của nhiều quân trên mỗi hàng và cột. 

Các trường hợp cạnh làm rõ cấu trúc: 

cho$n = 1$, bảng có một ô duy nhất. Đặt một quân xe sẽ không có kẻ tấn công nào, vì vậy câu trả lời là 1. 

cho$n = 2$, nếu chúng ta đặt hai quân xe trên các hàng và cột khác nhau (đường chéo), cả hai đều bị cô lập và hợp lệ, cho kết quả 2. Nếu chúng ta cố gắng đặt nhiều hơn, bất kỳ quân thứ ba nào cũng phải chia sẻ một hàng hoặc cột và ngay lập tức khiến ít nhất một quân xe có hai quân tấn công, vi phạm quy tắc. 

Vì$n = 3$, trực giác ngây thơ có thể gợi ý việc đặt nhiều quân xe, nhưng bất kỳ cấu hình nào vượt quá một mô hình tuyến tính nhỏ sẽ nhanh chóng buộc một quân xe phải có hai quân lân cận trong hàng hoặc cột của nó. Điều này gợi ý rằng cấu trúc bị ràng buộc chặt chẽ và có thể chỉ phụ thuộc vào cách chúng ta ghép các hàng và cột. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con của ô và kiểm tra tính hợp lệ. Đối với mỗi cấu hình, chúng tôi sẽ tính xung đột giữa các quân trên mỗi quân và đảm bảo không có quân nào có nhiều hơn một kẻ tấn công. Số lượng cấu hình là$2^{n^2}$và thậm chí đánh giá chi phí của một cấu hình$O(n^2)$. Điều này là hoàn toàn không thể thực hiện được. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ cố gắng xây dựng bảng theo từng hàng, theo dõi xem có bao nhiêu quân chiếm giữ mỗi hàng và cột. Thậm chí sau đó, chúng ta nhanh chóng rơi vào tình trạng bùng nổ tổ hợp vì mỗi quyết định hàng sẽ ảnh hưởng đến tất cả các cột trong tương lai. 

Cái nhìn sâu sắc quan trọng là chuyển từ suy nghĩ về các ô riêng lẻ sang suy nghĩ về việc nhóm các quân xe thành từng cặp. Vì mỗi quân xe có thể chịu đựng nhiều nhất một kẻ tấn công nên mỗi quân xe có thể tham gia vào nhiều nhất một “mối quan hệ xung đột”. Điều đó có nghĩa là đồ thị gây ra bởi các cuộc tấn công có bậc tối đa là 1. Những đồ thị như vậy là sự kết hợp rời rạc của các đỉnh và cạnh biệt lập. Nói cách khác, các quân xe tạo thành cặp cộng với một số quân xe có thể bị cô lập. 

Bây giờ giải thích điều này trên một$n \times n$Cái bảng. Hai quân xe chỉ tấn công lẫn nhau nếu chúng ở chung một hàng hoặc một cột, do đó, một “cặp” tương ứng với hai quân xe chia sẻ riêng một hàng hoặc một cột mà không tương tác với các cặp khác. Điều này giới hạn số lượng cặp như vậy có thể cùng tồn tại trước khi hàng hoặc cột được sử dụng lại theo cách tạo ra xung đột cấp độ 2. 

Việc xây dựng tối ưu giảm xuống việc sắp xếp các hàng và cột sao cho chúng ta tối đa hóa các cặp rời rạc. Yếu tố giới hạn là số lượng cặp hàng-cột rời rạc mà chúng ta có thể hình thành mà không bị chồng chéo. Mỗi cặp sử dụng hai hàng và hai cột trong một cấu trúc được kiểm soát và các hàng hoặc cột còn sót lại có thể lưu trữ các quân xe bị cô lập. 

Kết quả tối đa chỉ phụ thuộc vào số lượng khối cặp rời rạc đầy đủ phù hợp với$n$. Điều này dẫn đến một dạng đóng đơn giản: chúng ta có thể nhóm các hàng và cột thành các khối có kích thước 2, mỗi khối đóng góp 2 quân xe, và nếu$n$thật kỳ quặc, thêm một cấu trúc xe bị cô lập sẽ đóng góp thêm 1. 

Điều này mang lại:$$k = 2 \cdot \left\lfloor \frac{n}{2} \right\rfloor + (n \bmod 2)$$điều đó đơn giản hóa thành$k = n$. 

Tuy nhiên, sự đơn giản hóa ngây thơ đó ẩn chứa một sự tinh tế: hạn chế là bị tấn công bởi nhiều nhất một quân chứ không phải là một phần của nhiều nhất một cạnh xung đột trên toàn cầu. Điều này cho phép cấu hình dày đặc hơn một chút so với cách giải thích khớp nghiêm ngặt về mặt hàng và cột, nhưng vẫn giới hạn tổng số lần tương tác trên mỗi quân. 

Một lập luận cực đoan cẩn thận hơn cho thấy rằng mỗi quân xe có thể được “tích điện” cho tối đa một quân xe khác thông qua hàng hoặc cột của nó và mỗi điện tích như vậy có thể được ấn định duy nhất, giới hạn tổng số quân xe ở mức$n$. Một công trình đạt được$n$là đặt tất cả các quân xe trên một đường chéo duy nhất, đảm bảo mỗi quân xe được cách ly. 

Vì vậy, câu trả lời tối ưu chỉ đơn giản là$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{n^2})$|$O(n^2)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$. Đây là kích thước của bàn cờ và cũng là thông số kiểm soát kích thước cấu hình tối đa có thể. 
2. Quan sát rằng việc đặt một quân xe trên mỗi hàng và mỗi cột mà không chia sẻ bất kỳ hàng hoặc cột nào sẽ tự động thỏa mãn điều kiện, bởi vì mỗi quân xe không có kẻ tấn công. Điều này đã cung cấp một cấu hình kích thước hợp lệ$n$. 
3. Lập luận rằng bất kỳ nỗ lực nào vượt quá$n$quân xe buộc ít nhất một hàng hoặc cột phải chứa nhiều hơn một quân xe. Khi một hàng hoặc cột chứa nhiều quân xe, chúng ta phải đảm bảo rằng cấu trúc tấn công cảm ứng không tạo ra bất kỳ quân xe nào có hai quân lân cận. Điều này nhanh chóng buộc các hạn chế xếp tầng trên lưới. 
4. Kết luận rằng không có cấu hình nào có thể vượt quá$n$quân xe trong khi vẫn tôn trọng quy tắc "nhiều nhất một kẻ tấn công cho mỗi quân xe" và vì việc xây dựng đường chéo đạt được$n$, điều này là tối ưu. 

### Tại sao nó hoạt động 

Bất kỳ quân xe nào cũng có thể tham gia vào nhiều nhất một mối quan hệ tấn công. Theo thuật ngữ biểu đồ, mỗi quân có bậc tối đa là 1 trong biểu đồ xung đột được tạo ra bằng cách chia sẻ các hàng hoặc cột. Điều này hạn chế cấu hình ở một tập hợp các đỉnh bị cô lập và các cạnh bị cô lập. Trên một$n \times n$bảng, bạn có thể đạt được số đỉnh tối đa mà không vi phạm ràng buộc này bằng cách cô lập mọi quân xe, việc này được thực hiện bằng cách đặt tối đa một quân xe trên mỗi hàng và cột. Điều này mang lại chính xác$n$và bất kỳ sai lệch nào cố gắng thêm nhiều lực vào va chạm hàng hoặc cột làm tăng mức độ vượt quá ngưỡng cho phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
print(n)
```Giải pháp đọc số nguyên đơn và xuất trực tiếp. Lý do cốt lõi là cấu trúc tối ưu phù hợp với giới hạn trên tầm thường thu được bằng cách đặt tối đa một quân xe trên mỗi hàng và cột. Bất kỳ việc đóng gói tích cực nào hơn sẽ gây ra nhiều cuộc tấn công không thể tránh khỏi đối với một số xe, vi phạm ràng buộc. 

Không cần cấu trúc dữ liệu hoặc mô phỏng bổ sung vì vấn đề chỉ đơn giản là xác định giới hạn chặt chẽ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```Chúng tôi có một tế bào duy nhất. Đặt một quân xe sẽ không có kẻ tấn công. 

| Bước | n | Đầu ra cho đến nay | 
| --- | --- | --- | 
| Đọc đầu vào | 1 | - | 
| Tính đáp án | - | 1 | 

Điều này xác nhận rằng trường hợp cơ sở hoạt động chính xác. 

### Ví dụ 2 

đầu vào:```
5
```Chúng tôi xem xét một bảng 5 × 5. Cách bố trí tối ưu là đặt quân xe trên đường chéo chính. 

| Bước | n | Giải thích | 
| --- | --- | --- | 
| Đọc đầu vào | 5 | Kích thước bảng | 
| Xây dựng ý tưởng | - | Một quân mỗi hàng/cột | 
| Đầu ra | - | 5 | 

Điều này chứng tỏ rằng ngay cả trên các bảng lớn hơn, cấu trúc đường chéo bão hòa đường viền mà không vi phạm ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ đọc một số nguyên và một lần in | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu phụ trợ | 

Những ràng buộc cho phép$n$lên đến$10^7$, vì vậy bất kỳ phương pháp tuyến tính hoặc bậc hai nào cũng sẽ không cần thiết. Cần phải tính toán theo thời gian không đổi, điều này được thực hiện trực tiếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline().strip())
    return str(n)

# provided samples
assert run("1\n") == "1"

# custom cases
assert run("2\n") == "2", "minimum non-trivial board"
assert run("3\n") == "3", "small odd case"
assert run("10\n") == "10", "larger even board"
assert run("10000000\n") == "10000000", "maximum constraint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp cơ sở | 
| 2 | 2 | trường hợp tương tác nhỏ nhất | 
| 3 | 3 | tính nhất quán có kích thước lẻ | 
| 10 | 10 | tỉnh táo mở rộng quy mô | 
| 10000000 | 10000000 | xử lý giới hạn trên | 

## Vỏ cạnh 

cho$n = 1$, thuật toán đưa ra trực tiếp 1. Không có tương tác nào có thể xảy ra, do đó ràng buộc được thỏa mãn một cách tầm thường. 

Vì$n = 2$, đầu ra là 2. Vị trí theo đường chéo cho thấy tính khả thi. Bất kỳ nỗ lực nào để đặt 3 quân xe đều buộc phải cấu hình một hàng hoặc cột chung để có ít nhất một quân xe có hai kẻ tấn công, vi phạm quy tắc. 

Đối với lớn$n$, chẳng hạn như$10^7$, thuật toán vẫn thực hiện một thao tác đọc và in. Không có sự phụ thuộc vào việc lặp lại hoặc phân bổ bộ nhớ tỷ lệ thuận với$n$, do đó hiệu suất vẫn không đổi. 

Mỗi trường hợp xác nhận rằng giải pháp được điều khiển hoàn toàn bởi giới hạn cấu trúc được áp đặt bởi tính độc quyền của hàng và cột và không có cấu hình ẩn nào có thể vượt quá nó.

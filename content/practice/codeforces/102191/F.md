---
title: "CF 102191F - Tính tổng rồi nhân"
description: "Chúng ta có một mảng số nguyên dương và chúng ta phải đặt các vết cắt giữa các phần tử để chia nó thành các phân đoạn liên tiếp. Mỗi phân khúc đóng góp tổng của nó và mục tiêu là tối đa hóa sản phẩm của tất cả các khoản đó. Đầu ra không phải là giá trị tối đa."
date: "2026-08-18T09:22:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "F"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 809
verified: false
draft: false
---

[CF 102191F - Tính tổng rồi nhân](https://codeforces.com/problemset/problem/102191/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13m 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng số nguyên dương và chúng ta phải đặt các vết cắt giữa các phần tử để chia nó thành các phân đoạn liên tiếp. Mỗi phân khúc đóng góp tổng của nó và mục tiêu là tối đa hóa sản phẩm của tất cả các khoản đó. Đầu ra không phải là giá trị tối đa. Chúng ta phải in một phân vùng đạt mức tối đa, sử dụng`/`giữa các đoạn liên tiếp. 

Khó khăn chính là việc cắt giảm sẽ thay đổi hai thứ cùng một lúc. Nó thay thế một yếu tố, tổng của một phân khúc lớn hơn, bằng hai yếu tố mà chúng ta muốn so sánh tích của chúng với tổng ban đầu đó. Vì mọi giá trị mảng đều dương nên sự so sánh này có cấu trúc đặc biệt mạnh mẽ. 

Mảng chứa tối đa 3⋅10 5 phần tử, do đó, thuật toán kiểm tra tất cả các cặp vị trí, hoặc tệ hơn là tất cả các tập hợp cắt có thể, vượt xa giới hạn một giây. Một thuật toán bậc hai đã thực hiện khoảng 9⋅10 10 lần lặp ở kích thước tối đa. Chúng ta cần một công trình tuyến tính hoặc gần tuyến tính. 

Bản thân các giá trị có thể lớn tới 10 9, nhưng thuật toán chỉ cần so sánh và bổ sung liên quan đến chúng. Số nguyên Python cũng tránh bị tràn, mặc dù giải pháp không bao giờ cần tính toán sản phẩm cuối cùng khổng lồ. 

Có một số trường hợp khó xử lý. Đối với đầu vào```
17
```câu trả lời duy nhất có thể là`7`. Không cần phải cắt, vì vậy mã giả định có ít nhất hai phần tử có thể bị lỗi. 

Vì```
31 1 1
```câu trả lời đúng là`1 1 1`, không có dấu gạch chéo. Chia nó thành ba thừa số sẽ được 1, trong khi giữ nó thành một phân số sẽ cho 3. Chiến lược luôn chia các số dương sẽ là sai. 

Vì```
31 2 1
```câu trả lời tối ưu là`1 2 1`, vì tích của nó bằng 4. Chia xung quanh giá trị ở giữa sẽ cho`1 / 2 1`, có tích là 1⋅3=3. Một chiến lược luôn đặt mọi giá trị lớn hơn một vào phân khúc riêng của nó sẽ thất bại ở đây. 

Ngoài ra còn có một trường hợp ít rõ ràng hơn:```
42 1 1 2
```Một phân vùng tối ưu là`2 1 / 1 2`, cho 3⋅3=9. Đặt cả hai cái ở hai bên sẽ có 4⋅2=8. Do đó, những giá trị giữa hai giá trị lớn hơn không thể được gán đơn giản cho một bên. Chúng phải được phân phối một cách tối ưu. 

## Phương pháp tiếp cận 

Một giải pháp cưỡng bức trực tiếp sẽ xem xét mọi vị trí cắt có thể. Có n−1 khoảng trống giữa các phần tử liên tiếp và mỗi khoảng trống có thể chứa phần cắt hoặc không, do đó có chính xác 2 phần tử n−1. Đối với mỗi phân vùng, chúng ta có thể tính tổng các phân đoạn và tích của chúng rồi giữ lại kết quả tốt nhất. Việc triển khai đơn giản sẽ thực hiện các phép toán số học Θ(n2 n−1 ) trong trường hợp xấu nhất, vì một phân vùng có thể chứa các phân đoạn Θ(n). Ngay cả một phép liệt kê được cải tiến để duy trì tích tăng dần vẫn có các trạng thái Θ(2 n ), điều này là vô vọng với n=3⋅10 5. 

Công thức lập trình động tự nhiên cũng quá chậm. Nếu như`dp[i]`là tích tốt nhất cho tiền tố tận cùng ở vị trí thứ i, chúng ta có thể thử mọi vị trí cắt trước đó và lấy tổng của hậu tố tương ứng. Điều đó mang lại O(n 2 ), vốn đã quá lớn. 

Quan sát quan trọng xuất phát từ việc so sánh một đoạn với một vết cắt có thể có bên trong nó. Giả sử một đoạn có tổng tổng x+y và việc cắt nó sẽ tạo ra hai phần liên tiếp có tổng x và y. Đóng góp cũ là x+y, trong khi đóng góp mới là xy. Vì x và y là số nguyên dương nên 

xy ≥x+y 

bất cứ khi nào x,y ≥2, bởi vì 

xy−x−y=(x−1)(y−1)−1 ≥0. 

Vì vậy, bất cứ khi nào cả hai mặt của một phần cắt tiềm năng có tổng ít nhất là hai, việc cắt giảm không bao giờ làm giảm câu trả lời. 

Điều này có một hậu quả mạnh mẽ. Vì mọi phần tử mảng lớn hơn một đều có tổng ít nhất là hai, nên có thể chọn phân vùng tối ưu sao cho không có phân đoạn nào chứa hai phần tử lớn hơn một. Nếu đúng như vậy, chúng ta có thể cắt giảm giữa chúng và không làm cho kết quả trở nên tồi tệ hơn. 

Điều đó có nghĩa là mọi phân đoạn trong một giải pháp tối ưu có nhiều nhất một phần tử lớn hơn một phần tử. Câu hỏi duy nhất còn lại là phải làm gì với hàng loạt cái đó. 

Xét hai giá trị liên tiếp lớn hơn một, x và y, với chính xác k giá trị ở giữa chúng: 

x, k 1,1,…,1 ​ ,y. 

Những cái k phải được chia thành đoạn chứa x và đoạn chứa y. Nếu l cái ở bên trái và k−l ở bên phải thì đóng góp của họ là 

(x+l)(y+k−l). 

Tổng của hai yếu tố này là cố định: 

x+l+y+k−l=x+y+k. 

Đối với hai số dương có tổng cố định, tích của chúng lớn nhất khi chúng càng gần nhau càng tốt. Vì vậy chúng ta chỉ cần chọn l sao cho x+l càng gần một nửa x+y+k càng tốt. 

Tất cả những cái dẫn đầu phải tham gia giá trị đầu tiên lớn hơn một và tất cả những cái theo sau đều phải tham gia giá trị cuối cùng như vậy. Nếu toàn bộ mảng bao gồm các mảng thì việc giữ toàn bộ mảng dưới dạng một phân đoạn là tối ưu. 

Điều này làm giảm toàn bộ vấn đề khi quét mảng một lần, tìm các giá trị lớn hơn một và các giá trị chạy giữa chúng và quyết định phân chia từng lần chạy một cách độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2 n ) | O(n) | Quá chậm | 
| Tiền tố DP | O(n 2 ) | O(n) | Quá chậm | 
| Xây dựng tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm mọi vị trí có giá trị lớn hơn một. Những giá trị này sẽ đóng vai trò là điểm neo của các phân khúc tối ưu. Nếu không có vị trí nào như vậy thì mảng bao gồm toàn bộ các vị trí, do đó xuất toàn bộ mảng dưới dạng một phân đoạn. 
2. Bắt đầu phân đoạn hiện tại với giá trị đầu tiên lớn hơn một và tất cả những giá trị xuất hiện trước nó. Những công ty dẫn đầu đó không thể đứng một mình để sinh lời, bởi vì nhân với hệ số một còn tệ hơn việc thêm một công ty vào phân khúc hiện có. 
3. Xử lý mọi cặp giá trị liên tiếp lớn hơn một, gọi chúng là x và y. Đếm k cái giữa chúng. 
4. Giả sử l trong số đó được gán cho đoạn chứa x. Các k−l số còn lại thuộc về phân đoạn chứa y, do đó hai tổng liên quan là x+l và y+k−l. 
5. Chọn l gần nhất 

2 y+k−x ​ . 

Biểu thức xuất phát từ việc làm cho x+l càng gần với một nửa tổng x+y+k cố định càng tốt. Kẹp kết quả vào khoảng [0,k], vì chúng ta không thể gán số âm đơn vị hoặc nhiều hơn tất cả k đơn vị ở bên trái. 

1. Nối những chữ l đó vào phân đoạn hiện tại và kết thúc phân đoạn đó bằng dấu gạch chéo. Bắt đầu phân đoạn tiếp theo với k−l phân đoạn còn lại, theo sau là y. 
2. Sau khi xử lý giá trị cuối cùng lớn hơn một, hãy nối tất cả các giá trị cuối vào phân đoạn của nó. Các phân đoạn kết quả tạo thành một phân vùng hợp lệ và đạt được sản phẩm tối đa. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ phân đoạn nào có chứa hai phần tử lớn hơn một. Việc cắt ở đâu đó giữa hai phần tử đó sẽ tạo ra hai phân đoạn có tổng ít nhất là hai. Nếu tổng của chúng là x và y, việc thay hệ số x+y bằng xy không thể làm giảm tích. Việc lặp lại thao tác này sẽ tạo ra một phân vùng tối ưu trong đó mỗi phân đoạn chứa nhiều nhất một giá trị lớn hơn một.

Do đó, tất cả các lựa chọn đều bị giới hạn ở các lần chạy số một giữa hai giá trị liên tiếp lớn hơn một. Lần chạy như vậy không tương tác với bất kỳ lần chạy nào khác vì nó chỉ thay đổi tổng của hai phân đoạn lân cận. Đối với dãy k đơn vị giữa x và y, đóng góp của nó là (x+l)(y+k−l), có hai thừa số có tổng cố định. Tích của hai số dương có tổng cố định sẽ lớn nhất khi chúng bằng nhau nhất có thể, chính xác như số đã chọn. Những mảng dẫn đầu và theo sau chỉ có một hàng xóm hữu ích có thể có, trong khi một mảng toàn bộ tốt nhất nên được giữ dưới dạng một phân đoạn. Do đó mọi phần độc lập đều tối ưu và sự kết hợp của chúng là tối ưu toàn cục. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    n = int(input())    a = list(map(int, input().split()))
    big = [i for i, x in enumerate(a) if x > 1]
    if not big:        print(" ".join(map(str, a)))        return
    parts = []
    first = big[0]    current = a[:first + 1]
    for p in range(1, len(big)):        prev = big[p - 1]        cur = big[p]
        k = cur - prev - 1        x = a[prev]        y = a[cur]
        # Maximize (x + l) * (y + k - l).        # The ideal value is (y + k - x) / 2.        l = (y + k - x) // 2        l = max(0, min(k, l))
        current.extend([1] * l)        parts.append(current)
        current = [1] * (k - l)        current.append(y)
    last = big[-1]    current.extend(a[last + 1:])    parts.append(current)
    output = []    for i, part in enumerate(parts):        if i:            output.append("/")        output.extend(map(str, part))
    print(" ".join(output))

if __name__ == "__main__":    solve()
```các`big`mảng lưu trữ chính xác các vị trí có giá trị vượt quá một. Đây là những giá trị duy nhất có thể đóng vai trò là yếu tố trung tâm của phân khúc tối ưu. 

ban đầu`current`đoạn chứa điểm neo đầu tiên và mọi điểm neo dẫn đầu. lát cắt`a[:first + 1]`giữ nguyên thứ tự ban đầu và xử lý ranh giới trước điểm neo đầu tiên mà không có trường hợp đầu ra đặc biệt. 

Đối với hai mỏ neo liên tiếp,`k`là số lượng những người nằm giữa các vị trí của họ. Biến`l`là số được gán cho đoạn bên trái. biểu thức`(y + k - x) // 2`là tầng nguyên của giá trị lý tưởng. Một trong hai số nguyên lân cận là tối ưu khi điểm lý tưởng nằm chính xác giữa hai số nguyên, do đó lấy mức sàn là đủ. 

các`max`Và`min`các cuộc gọi là cần thiết ở ranh giới. Ví dụ: nếu x lớn hơn nhiều so với y+k, giá trị lý tưởng có thể âm, nghĩa là tất cả số 1 sẽ ở bên phải. Nếu y lớn hơn nhiều thì tất cả số 1 sẽ ở bên trái. 

Phân đoạn hiện tại được hoàn thiện trước khi phân đoạn tiếp theo được tạo. Thứ tự này quan trọng vì đoạn bên trái phải chứa đoạn đầu tiên`l`những cái và phân đoạn bên phải phải chứa phần còn lại`k-l`những cái đó. Những dấu cuối cùng được thêm vào sau khi tất cả các khoảng trống bên trong đã được xử lý. 

Mã không bao giờ tính toán sản phẩm cuối cùng. Sản phẩm đó có thể có số lượng chữ số khổng lồ và vấn đề chỉ yêu cầu phân vùng tối đa. Do đó, các số nguyên chính xác tùy ý của Python thậm chí không liên quan đến thuật toán chính. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mảng`8 1 1 3`, các giá trị lớn hơn một là 8 và 3. Có hai giá trị nằm giữa chúng. 

| Bước | Neo hiện tại | Neo tiếp theo | k | x | y | Được chọn l | Phân đoạn cho đến nay | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 8 | 3 | 2 | 8 | 3 | 0 |`8`| 
| Khoảng cách quy trình | 8 | 3 | 2 | 8 | 3 | 0 |`8`,`1 1 3`| 
| Kết thúc | 3 | không | 0 | 3 | | |`8`,`1 1 3`| 

Giá trị lý tưởng là 

l=⌊ 2 3+2−8 ​ ⌋=−2, 

được kẹp ở mức 0. Do đó, cả hai đều ở bên phải, cho tổng phân số 8 và 5, với tích 40. 

Tuy nhiên, đầu ra mẫu`8 / 1 1 / 3`cho 8⋅2⋅3=48, lớn hơn. Điều này bộc lộ một lỗ hổng trong đặc tính neo được đề xuất: một phân đoạn chỉ chứa một phân đoạn có thể hữu ích khi tổng của nó bằng 2, vì việc chia tổng của 2 thành thừa số 1⋅1 sẽ tệ hơn, trong khi giữ nguyên hai phân số sẽ tạo ra thừa số 2. 

Vì vậy việc giảm trước đó phải được tinh chỉnh. Bản thân một loạt các số có thể tạo thành một phân đoạn khi nó chứa chính xác hai số một và nói chung hơn, việc xử lý tối ưu của nó phụ thuộc vào việc liệu việc giữ tổng của hai số đó làm hệ số riêng biệt có tốt hơn việc gắn các số đó vào các điểm neo lân cận hay không. 

Điều này có nghĩa là cấu trúc hai neo độc lập đơn giản ở trên không đúng cho vấn đề thực tế. 

Giải pháp đúng yêu cầu giảm cấu trúc khác, vì vậy mã ở trên không được gửi. 

## Hiểu biết sâu sắc về cấu trúc chính xác 

Sự so sánh mang tính quyết định không chỉ đơn thuần là liệu cả hai bên của một vết cắt có tổng ít nhất là hai hay không. Với các tổng x, y, 

xy ≥x+y 

đúng với x,y ≥2, với đẳng thức cụ thể khi x=y=2. 

Do đó, mọi phân đoạn có thể được tinh chỉnh cho đến khi mọi phân đoạn kết quả có tổng bằng 1 hoặc 2, ngoại trừ khả năng việc tách một phân đoạn sẽ làm thay đổi các lựa chọn lân cận. Vì một phân đoạn có tổng 1 luôn có hại khi một phân khúc tích cực khác có thể hấp thụ nó, nên phân khúc nhỏ biệt lập hữu ích duy nhất là phân khúc có tổng 2. 

Bởi vì tất cả các phần tử mảng đều dương nên một phân đoạn có tổng 2 chỉ có thể là một trong hai`[2]`hoặc`[1, 1]`. Mọi phần tử lớn hơn hai phải được cách ly với các phần tử khác trong một phân vùng tối ưu, trong khi giá trị bằng hai có thể cạnh tranh với một cặp phần tử liền kề. 

Điều này dẫn đến một quy tắc địa phương đơn giản hơn nhiều. Mỗi giá trị lớn hơn hai phải là phân khúc riêng của nó, với các giá trị liền kề được chỉ định theo so sánh sản phẩm. Các giá trị bằng hai có thể được xử lý chính xác giống như một phân đoạn gồm hai số một và các chuỗi một số phải được phân chia thành nhóm hai bất cứ khi nào chúng không thể được gắn vào các phân đoạn lớn hơn lân cận một cách có lợi. 

Việc xây dựng kết quả được thể hiện một cách tự nhiên hơn dưới dạng một chương trình động trong các lần chạy, bởi vì sự tương tác ranh giới xung quanh giá trị hai có thể tạo ra các lựa chọn thay thế có sản phẩm bằng nhau. 

Vì báo cáo vấn đề được cung cấp không bao gồm lời giải thích chính thức và kết quả tìm kiếm chỉ hiển thị tuyên bố đó nên tôi không nên trình bày một thuật toán chưa được xác minh dưới dạng một bài xã luận hoàn chỉnh được chấp nhận. Cấu trúc tuyến tính được tuyên bố trước đó rõ ràng đã thất bại ở Mẫu 1, vì vậy việc sử dụng nó sẽ tạo ra mã không chính xác. 

Bản thân mẫu này đưa ra phản ví dụ: đối với`8 1 1 3`, đáp án tối ưu là`8 / 1 1 / 3`, với sản phẩm 48, trong khi gán hai cái ở bên phải sẽ cho kết quả 8/113, với sản phẩm 40. Điều này trực tiếp bác bỏ quy tắc đơn giản "mỗi lần chạy một cái đều thuộc về một trong những mỏ neo lân cận của nó".

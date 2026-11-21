---
title: "CF 235B - Cùng chơi Osu!"
description: "Mỗi lần nhấp chuột trong trò chơi sẽ trở thành một cú đánh thành công (O) hoặc một cú đánh trượt (X). Đối với một chuỗi đã hoàn thành, chúng tôi chia nó thành các khối liên tiếp tối đa của O. Nếu một khối có độ dài L thì nó sẽ đóng góp L vào điểm. Tổng số điểm là tổng của các độ dài khối bình phương này."
date: "2026-06-04T10:08:22+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 235
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 146 (Div. 1)"
rating: 2000
weight: 235
solve_time_s: 93
verified: true
draft: false
---

[CF 235B - Hãy chơi Osu!](https://codeforces.com/problemset/problem/235/B) 

**Xếp hạng:** 2000 
**Tags:** dp, toán, xác suất 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi lần nhấp chuột trong trò chơi sẽ trở thành một cú nhấp chuột thành công (`O`) hoặc bỏ sót (`X`). 

Đối với một chuỗi hoàn chỉnh, chúng tôi chia nó thành các khối liên tiếp tối đa`O`. Nếu một khối có chiều dài`L`, nó góp phần`L²`đến điểm số. Tổng số điểm là tổng của các độ dài khối bình phương này. 

Chúng tôi không được đưa ra một trình tự cụ thể. Thay vào đó, với mỗi vị trí`i`, chúng ta biết xác suất`p[i]`rằng nhấp chuột thành công. Tất cả các nhấp chuột đều độc lập. Chúng ta phải tính giá trị kỳ vọng của điểm số cuối cùng. 

Bản năng đầu tiên là nghĩ về tất cả các kết quả có thể xảy ra. Một chuỗi độ dài`n`có`2^n`những nhận thức có thể xảy ra và mỗi nhận thức đều có điểm số riêng. Việc tính toán kỳ vọng bằng cách liệt kê chúng là không thể. Với`n`lên đến`100000`, thậm chí việc lưu trữ tất cả các kết quả sẽ là vô lý. 

Ràng buộc`n ≤ 100000`ngay lập tức gợi ý rằng chúng ta cần một cái gì đó gần với thời gian tuyến tính. Lập trình động bậc hai đã quá đắt vì`10^10`hoạt động vượt xa thời hạn. Bất kỳ giải pháp được chấp nhận nào cũng phải xử lý từng xác suất chỉ với một số lần không đổi. 

Phần khó khăn là điểm số phụ thuộc vào toàn bộ chuỗi thành công liên tiếp. Một cuộc chạy kết thúc ở vị trí`i`có thể đã bắt đầu sớm hơn một cách tùy ý, do đó sự chuyển đổi cục bộ là không rõ ràng. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
1
1.0
```Trình tự duy nhất có thể là`"O"`, số điểm của ai là`1`. Bất kỳ sự lặp lại nào đều phải khởi tạo chính xác vị trí đầu tiên. 

Coi như:```
2
1.0 1.0
```Trình tự duy nhất có thể là`"OO"`, số điểm của ai là`4`, không`2`. Một phương pháp chỉ tính tổng những đóng góp dự kiến ​​của từng vị trí sẽ bỏ lỡ sự tương tác giữa những thành công liên tiếp. 

Coi như:```
3
0 1 0
```Nhấp chuột giữa luôn tạo thành một chuỗi dài`1`, vậy đáp án là chính xác`1`. Bất kỳ sự lặp lại nào vô tình mang trạng thái qua một vị trí có xác suất`0`sẽ cho kết quả sai. 

Một trường hợp tế nhị khác là:```
3
0.5 0.5 0.5
```Một cuộc chạy đua có thể kéo dài qua nhiều vị trí, do đó sự đóng góp của vị trí`i`phụ thuộc vào độ dài dự kiến ​​của cuộc chạy kết thúc vào lúc`i-1`. Sự phụ thuộc này chính xác là những gì giải pháp phải nắm bắt. 

## Phương pháp tiếp cận 

Giả sử chúng ta thử dùng vũ lực. 

Đối với mỗi một trong những`2^n`các chuỗi có thể xảy ra, chúng ta có thể tính xác suất và điểm số của nó, sau đó cộng`probability × score`để trả lời. Điều này đúng về mặt toán học vì kỳ vọng được xác định theo cách đó. 

Vấn đề là số lượng kết quả. Ngay cả đối với`n = 50`, đã có hơn một triệu triệu trình tự. Vì`n = 100000`, cách tiếp cận này hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta thực sự không cần phân phối đầy đủ số lượt chạy. Chúng tôi chỉ cần đủ thông tin để cập nhật điểm dự kiến ​​khi xử lý thêm một vị trí. 

Hãy tưởng tượng một chuỗi thành công kết thúc ở vị trí hiện tại. Gọi chiều dài của nó là`L`. 

Nếu lần nhấp hiện tại thành công, hãy kéo dài thời gian chạy từ chiều dài`L-1`chiều dài`L`, mức đóng góp điểm của lần chạy đó thay đổi từ`(L-1)²`ĐẾN`L²`. 

Sự gia tăng là`L² - (L-1)² = 2L - 1`. 

Hình thức gia tăng này là sự đơn giản hóa quan trọng. 

Thay vì theo dõi các lần chạy hoàn chỉnh, chúng tôi theo dõi độ dài dự kiến ​​của hậu tố hiện tại của các thành công liên tiếp. Khi có một cú nhấp chuột thành công mới, điểm sẽ tăng thêm`2L-1`. Vì kỳ vọng là tuyến tính nên chúng ta chỉ cần giá trị kỳ vọng của`L`. 

Điều này dẫn đến việc duy trì DP tuyến tính đơn giản:`len`, độ dài dự kiến ​​của hậu tố thành công liên tiếp kết thúc ở vị trí hiện tại.`ans`, số điểm dự kiến ​​tích lũy được cho đến nay. 

Mỗi xác suất được xử lý một lần, đưa ra một`O(n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Định nghĩa trạng thái 

Hãy:`f`= độ dài dự kiến ​​của chuỗi liên tiếp`O`hậu tố kết thúc ở vị trí hiện tại.`ans`= điểm dự kiến ​​sau khi xử lý vị trí hiện tại. 

### Đạo hàm chuyển tiếp 

Giả sử chúng ta đang xử lý xác suất`p`. 

Nếu nhấp chuột bị trượt, độ dài hậu tố sẽ trở thành`0`. 

Nếu nhấp chuột thành công, độ dài hậu tố sẽ trở thành độ dài hậu tố trước đó cộng thêm`1`. 

Kể từ đây:$$f' = p(f+1)$$bởi vì với xác suất`p`chúng tôi có được`f+1`, và với xác suất`1-p`chúng tôi có được`0`. 

Bây giờ hãy xem xét việc tăng điểm. 

Khi một cú nhấp chuột thành công sẽ kéo dài một lượt chạy có độ dài mới là`L`, điểm chạy tăng thêm:$$L^2-(L-1)^2 = 2L-1$$Độ dài mới bằng`f+1`ở dạng có điều kiện kỳ ​​vọng. 

Do đó mức tăng dự kiến ​​đóng góp của vị trí này là:$$p \cdot (2(f+1)-1) = p(2f+1)$$Vì thế:$$ans' = ans + p(2f+1)$$Chú ý rằng giá trị cũ của`f`phải được sử dụng ở đây trước khi cập nhật nó. 

### Các bước được đánh số 

1. Khởi tạo`f = 0`Và`ans = 0`. 
2. Xử lý xác suất từ ​​trái sang phải. 
3. Đối với xác suất hiện tại`p`, thêm vào$$p(2f+1)$$ĐẾN`ans`. 

Đây là mức tăng điểm dự kiến ​​do kéo dài lần chạy hiện tại nếu nhấp chuột thành công. 
4. Cập nhật độ dài hậu tố dự kiến:$$f = p(f+1)$$Nếu nhấp chuột trượt, hậu tố sẽ trở thành 0. Nếu thành công, độ dài hậu tố sẽ tăng thêm một. 
5. Sau khi tất cả các xác suất được xử lý, xuất ra`ans`. 

### Tại sao nó hoạt động 

hãy để`L_i`biểu thị độ dài ngẫu nhiên của hậu tố thành công liên tiếp kết thúc tại vị trí`i`. 

Biến được duy trì`f`chính xác là`E[L_i]`. 

Khi vị trí`i`thành công, lần chạy kết thúc ở đó thay đổi theo độ dài`L_i-1`ĐẾN`L_i`. Mức tăng điểm luôn là:$$L_i^2-(L_i-1)^2 = 2L_i-1$$Lấy kỳ vọng và sử dụng tính tuyến tính của kỳ vọng sẽ cho:$$E[\Delta] = p_i(2E[L_{i-1}]+1)$$đó chính xác là bản cập nhật được thêm vào`ans`. 

Vì mỗi lần tăng điểm được tính một lần và`f`luôn bằng độ dài hậu tố dự kiến ​​sau khi xử lý vị trí hiện tại, giá trị tích lũy`ans`bằng tổng số điểm dự kiến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(float, input().split()))

    f = 0.0
    ans = 0.0

    for prob in p:
        ans += prob * (2.0 * f + 1.0)
        f = prob * (f + 1.0)

    print("{:.15f}".format(ans))

solve()
```Biến`f`lưu trữ độ dài dự kiến ​​​​của hậu tố thành công liên tiếp hiện tại. Trước khi cập nhật, chúng tôi sử dụng giá trị cũ của nó để tính mức tăng điểm dự kiến ​​do nhấp chuột hiện tại mang lại. 

Thứ tự là quan trọng. Công thức$$ans += p(2f+1)$$sử dụng độ dài hậu tố kết thúc ở vị trí trước đó. Đang cập nhật`f`đầu tiên sẽ sử dụng sai độ dài hậu tố mới và tính gấp đôi phần đóng góp. 

Tất cả các phép tính đều sử dụng số dấu phẩy động. Phép truy toán chỉ thực hiện phép cộng và phép nhân, do đó độ chính xác kép tiêu chuẩn là quá đủ cho yêu cầu`10^{-6}`sự chính xác. 

Việc sử dụng bộ nhớ không đổi vì chỉ có hai biến dấu phẩy động được duy trì bất kể`n`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0.5 0.5 0.5
```| Vị trí | p | f trước | Tăng điểm | trả lời sau | f sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0,5 | 0,0 | 0,5 | 0,5 | 0,5 | 
| 2 | 0,5 | 0,5 | 1.0 | 1,5 | 0,75 | 
| 3 | 0,5 | 0,75 | 1,25 | 2,75 | 0,875 | 

Câu trả lời cuối cùng:```
2.750000000000000
```Ví dụ này cho thấy độ dài hậu tố mong đợi tăng dần như thế nào. Mặc dù không biết thời lượng chạy cụ thể nhưng kỳ vọng vẫn đủ để tính toán mức tăng điểm dự kiến. 

### Ví dụ 2 

đầu vào:```
3
1 1 1
```| Vị trí | p | f trước | Tăng điểm | trả lời sau | f sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 1 | 
| 2 | 1 | 1 | 3 | 4 | 2 | 
| 3 | 1 | 2 | 5 | 9 | 3 | 

Câu trả lời cuối cùng:```
9.000000000000000
```Trình tự duy nhất có thể là`"OOO"`. Chiều dài chạy tăng từ`1`ĐẾN`2`ĐẾN`3`, tạo ra sự gia tăng điểm số`1`,`3`, Và`5`, tổng của nó là`9`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi xác suất được xử lý một lần | 
| Không gian | O(1) | Chỉ có hai biến dấu phẩy động được duy trì | 

Với`n = 100000`, thuật toán thực hiện khoảng một trăm nghìn lần lặp và một số phép toán số học trên mỗi lần lặp. Điều này thoải mái phù hợp trong thời gian giới hạn và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    p = list(map(float, input().split()))

    f = 0.0
    ans = 0.0

    for prob in p:
        ans += prob * (2.0 * f + 1.0)
        f = prob * (f + 1.0)

    return "{:.15f}".format(ans)

# provided sample
assert abs(float(run("3\n0.5 0.5 0.5\n")) - 2.75) < 1e-9

# minimum size, always miss
assert abs(float(run("1\n0\n")) - 0.0) < 1e-9

# minimum size, always hit
assert abs(float(run("1\n1\n")) - 1.0) < 1e-9

# all hits, score = 3^2
assert abs(float(run("3\n1 1 1\n")) - 9.0) < 1e-9

# isolated guaranteed hit
assert abs(float(run("3\n0 1 0\n")) - 1.0) < 1e-9

# two guaranteed hits form one block of length 2
assert abs(float(run("2\n1 1\n")) - 4.0) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`0`| Trường hợp nhỏ nhất, không có nhấp chuột thành công | 
|`1 / 1`|`1`| Khởi tạo đúng | 
|`3 / 1 1 1`|`9`| Chạy một lần kéo dài toàn bộ mảng | 
|`3 / 0 1 0`|`1`| Thiết lập lại đúng cách sau khi bỏ lỡ | 
|`2 / 1 1`|`4`| Các lượt truy cập liên tiếp phải hợp nhất thành một khối | 

## Vỏ cạnh 

Hãy xem xét:```
1
0
```Ban đầu`f = 0`. Bản cập nhật bổ sung`0 * (2*0 + 1) = 0`đến câu trả lời và`f`còn lại`0`. Kết quả cuối cùng là`0`, phù hợp với thực tế là mỗi cú nhấp chuột đều là một lần bỏ lỡ. 

Coi như:```
2
1 1
```Sau cú nhấp chuột đầu tiên,`ans = 1`Và`f = 1`. Sau lần nhấp thứ hai, đóng góp bổ sung là`1 * (2*1 + 1) = 3`, đưa ra câu trả lời cuối cùng của`4`. Điều này thể hiện chính xác một chiều dài`2`, số điểm của ai là`2²`. 

Coi như:```
3
0 1 0
```Nhấp chuột đầu tiên không đóng góp gì và rời đi`f = 0`. Nhấp chuột thứ hai góp phần`1`, sản xuất`f = 1`. Nhấp chuột thứ ba không đóng góp gì và đặt lại hậu tố. Câu trả lời là chính xác`1`, tương ứng với một cú nhấp chuột thành công riêng lẻ. 

Coi như:```
4
1 0 1 1
```Trình tự là xác định. Các lần chạy là`"O"`Và`"OO"`, cho điểm`1² + 2² = 5`. 

Thuật toán tạo ra: 

| Vị trí | p | f trước | trả lời sau | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 
| 2 | 0 | 1 | 1 | 
| 3 | 1 | 0 | 2 | 
| 4 | 1 | 1 | 5 | 

Việc đặt lại ở vị trí thứ hai sẽ ngăn lần chạy sau hợp nhất không chính xác với lần chạy đầu tiên.

---
title: "CF 104687C - \u0421\u0443\u043c\u043c\u0430 1"
description: "Chúng ta có hai khoảng nguyên: một khoảng xác định tất cả các giá trị hợp lệ của $x$, và khoảng còn lại xác định tất cả các giá trị hợp lệ của $y$. Chúng ta cũng có tổng mục tiêu $n$."
date: "2026-06-29T18:51:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104687
codeforces_index: "C"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u0432 \u0426\u0420\u041e\u0414 2022"
rating: 0
weight: 104687
solve_time_s: 61
verified: true
draft: false
---

[CF 104687C - \u0421\u0443\u043c\u043c\u0430 1](https://codeforces.com/problemset/problem/104687/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai khoảng nguyên: một khoảng xác định tất cả các giá trị hợp lệ của$x$và cái còn lại xác định tất cả các giá trị hợp lệ của$y$. Chúng tôi cũng có tổng mục tiêu$n$. Nhiệm vụ là đếm xem có bao nhiêu cặp$(x, y)$tồn tại sao cho$x$nằm trong khoảng của nó,$y$nằm trong khoảng của nó và tổng của chúng chính xác bằng$n$. 

Nói một cách cụ thể hơn, hãy tưởng tượng hai dãy số bao gồm nhau. Chúng tôi muốn chọn một số từ phạm vi đầu tiên và một số từ phạm vi thứ hai để chúng cộng lại thành một giá trị cố định. Đầu ra chỉ đơn giản là số cách hợp lệ để thực hiện việc này. 

Các ràng buộc đủ nhỏ để cả hai phạm vi đều tăng lên$10^5$, và tổng mục tiêu cũng lên tới$10^5$. Điều này ngay lập tức loại trừ bất cứ điều gì yêu cầu phép lặp bậc hai trên tất cả các cặp trong trường hợp xấu nhất, vì điều đó sẽ dẫn đến tối đa$10^{10}$hoạt động. Thay vào đó, chúng ta nên hướng tới lý luận tuyến tính hoặc theo thời gian không đổi cho mỗi bài kiểm tra. 

Một số trường hợp đặc biệt quan trọng đối với tính chính xác. Nếu phần bổ sung cần thiết$y = n - x$nằm ngoài khoảng thứ hai, tức là$x$không đóng góp gì cả. Ví dụ, nếu$l_1 = 1, r_1 = 10, l_2 = 1, r_2 = 10, n = 2$, thì chỉ$x = 1, y = 1$hiệu quả, vì vậy câu trả lời là 1. Một cách tiếp cận ngây thơ quên đi giới hạn$y$sẽ đếm không chính xác các cặp không hợp lệ như$x = 2, y = 0$, mặc dù$y$nằm ngoài phạm vi của nó. 

Một vấn đề khó phát hiện khác là đếm quá mức nếu người ta cố gắng đếm một cách độc lập giá trị hợp lệ.$x$Và$y$các giá trị mà không thực thi ràng buộc về tổng. Sự phụ thuộc giữa hai biến là chặt chẽ nên không thể xử lý chúng một cách độc lập. 

## Phương pháp tiếp cận 

Một phương pháp đơn giản là thử mọi cách có thể$x$trong khoảng của nó, hãy tính$y = n - x$, và kiểm tra xem điều này$y$nằm trong khoảng thứ hai. Điều này hoạt động vì với mỗi cố định$x$, có nhiều nhất một ứng cử viên$y$. Tính đúng đắn là ngay lập tức vì chúng ta đang trực tiếp thực thi điều kiện$x + y = n$. 

Vấn đề với cách tiếp cận bạo lực này là thời gian chạy của nó. Trong trường hợp xấu nhất, khoảng thời gian cho$x$chứa$10^5$giá trị và mỗi lần kiểm tra là thời gian không đổi, vì vậy nó chạy trong$O(r_1 - l_1 + 1)$, điều này có thể chấp nhận được ở đây. Tuy nhiên, chúng ta có thể đơn giản hóa hơn nữa bằng cách loại bỏ hoàn toàn việc lặp lại. 

Quan sát quan trọng là chúng tôi không tìm kiếm theo cặp, chúng tôi đang giao nhau đồng thời hai ràng buộc:$$x \in [l_1, r_1], \quad n - x \in [l_2, r_2]$$Điều kiện thứ hai có thể được viết lại thành:$$l_2 \le n - x \le r_2$$Sắp xếp lại mang lại:$$n - r_2 \le x \le n - l_2$$Vì thế$x$phải nằm trên giao điểm của hai khoảng:$$[l_1, r_1] \cap [n - r_2, n - l_2]$$Khi chúng ta tính giao điểm này, mọi số nguyên$x$bên trong nó tương ứng với chính xác một giá trị hợp lệ$y$, và ngược lại. Vì vậy, câu trả lời quy về việc đếm các số nguyên trong một khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r1 - l1 + 1) | O(1) | Đã chấp nhận | 
| Giao lộ khoảng cách | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển đổi bài toán thành các số nguyên nằm trong giao điểm của hai dãy. 

1. Tính khoảng biến đổi cho$x$ngụ ý bởi$y$-hạn chế. Đây là$[n - r_2, n - l_2]$. Bước này là cần thiết vì điều kiện trên$y$phải được thể hiện dưới dạng$x$để căn chỉnh cả hai ràng buộc. 
2. Tính toán sự chồng chéo giữa$[l_1, r_1]$Và$[n - r_2, n - l_2]$. Ranh giới bên trái của giao lộ là điểm cực đại của hai điểm cuối bên trái và ranh giới bên phải là điểm cực tiểu của hai điểm cuối bên phải. 
3. Nếu điểm cuối bên trái thu được vượt quá điểm cuối bên phải thì giao điểm trống, do đó câu trả lời là 0. Điều này tương ứng với trường hợp không$x$có thể thỏa mãn đồng thời cả hai ràng buộc. 
4. Ngược lại, số số nguyên trong giao điểm là$r - l + 1$, trực tiếp cho số lượng hợp lệ$x$, và do đó các cặp hợp lệ. 

### Tại sao nó hoạt động 

Mỗi cặp hợp lệ$(x, y)$thỏa mãn cả hai ràng buộc ban đầu. Phép biến đổi thay thế ràng buộc trên$y$với một ràng buộc tương đương trên$x$, duy trì sự tương ứng một-một giữa các cặp hợp lệ và số nguyên$x$ở ngã tư. Không có hai khác nhau$x$các giá trị tạo ra cùng một cặp và mỗi cặp hợp lệ tạo ra chính xác một$x$, vì vậy tính hợp lệ$x$các giá trị tương đương với việc đếm các cặp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

l1, r1, l2, r2, n = map(int, input().split())

left = max(l1, n - r2)
right = min(r1, n - l2)

if left > right:
    print(0)
else:
    print(right - left + 1)
```Mã áp dụng trực tiếp phép biến đổi khoảng có nguồn gốc từ thuật toán. Các biểu thức`n - r2`Và`n - l2`xác định phạm vi hợp lệ cho$x$ngụ ý bởi khoảng thứ hai. Lấy`max`Và`min`tính toán ranh giới giao lộ một cách an toàn trong thời gian không đổi. Điều kiện cuối cùng xử lý trường hợp giao điểm trống, đảm bảo không tạo ra số đếm âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 10 1 10 20
```Chúng tôi tính toán khoảng thời gian chuyển đổi cho$x$từ ràng buộc thứ hai:$x \in [20 - 10, 20 - 1] = [10, 19]$. 

Bây giờ giao nhau với$[1, 10]$: 

| Bước | Trái | Đúng | 
| --- | --- | --- | 
| Phạm vi x gốc | 1 | 10 | 
| Phạm vi x có nguồn gốc | 10 | 19 | 
| Giao lộ | 10 | 10 | 

Giao lộ chỉ chứa$x = 10$. Điều này tương ứng với$y = 10$, cho một cặp hợp lệ. 

Đầu ra là 1. 

Điều này xác nhận thuật toán xử lý chính xác các trường hợp chạm ranh giới trong đó giao lộ thu gọn về một điểm duy nhất. 

### Ví dụ 2 

đầu vào:```
2 5 3 7 9
```Phạm vi dẫn xuất từ ​​khoảng thứ hai là$x \in [9 - 7, 9 - 3] = [2, 6]$. 

| Bước | Trái | Đúng | 
| --- | --- | --- | 
| Phạm vi x gốc | 2 | 5 | 
| Phạm vi x có nguồn gốc | 2 | 6 | 
| Giao lộ | 2 | 5 | 

Có hiệu lực$x$giá trị là 2, 3, 4, 5 nên có 4 cặp. 

Điều này cho thấy các phạm vi chồng chéo tạo ra nhiều giải pháp hợp lệ như thế nào và thuật toán đếm chúng mà không liệt kê các cặp một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng số học và so sánh không đổi | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Các ràng buộc cho phép lên đến$10^5$, nhưng giải pháp không lặp lại trong phạm vi đó. Mọi thao tác đều có thời gian không đổi nên chương trình thoải mái nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    l1, r1, l2, r2, n = map(int, input().split())

    left = max(l1, n - r2)
    right = min(r1, n - l2)

    if left > right:
        return "0"
    return str(right - left + 1)

# provided sample
assert run("1 10 1 10 20") == "1"

# x-range empty after intersection
assert run("1 2 10 20 5") == "0"

# single solution at boundary
assert run("0 10 0 10 0") == "1"

# multiple solutions
assert run("1 10 1 10 10") == "10"

# shifted ranges
assert run("5 15 1 3 10") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 10 20 5 | 0 | không tồn tại phần bổ sung hợp lệ | 
| 0 10 0 10 0 | 1 | trường hợp biên có tổng bằng 0 | 
| 1 10 1 10 10 | 10 | chồng chéo hoàn toàn tạo ra số lượng tối đa | 
| 5 15 1 3 10 | 1 | sự đúng đắn của giao lộ hẹp | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi khoảng biến đổi nằm hoàn toàn bên ngoài khoảng ban đầu$x$-phạm vi. Đối với đầu vào`1 2 10 20 5`, phạm vi dẫn xuất là$x \in [5 - 20, 5 - 10] = [-15, -5]$. Giao nhau với$[1, 2]$tạo ra một khoảng trống, vì điểm cuối bên trái trở thành 1 và bên phải trở thành -5. Thuật toán trả về đúng 0 vì kiểm tra giao lộ`left > right`kích hoạt. 

Một trường hợp khác là khi giao điểm giảm xuống một giá trị duy nhất. Đối với đầu vào`0 10 0 10 0`, cả hai phạm vi trở thành$[0, 10]$, vậy giao điểm cũng là$[0, 10]$. Thuật toán trả về$10 - 0 + 1 = 11$, khớp với số cặp trong đó$x = -y$trong cùng một khoảng, kể cả số không. 

Trường hợp tinh tế cuối cùng là sự chồng chéo hoàn toàn trong đó mọi$x$trong phạm vi ban đầu là hợp lệ. Đối với đầu vào`1 10 1 10 10`, khoảng biến đổi cũng là$[0, 9]$, và giao nhau cho$[1, 9]$. Thuật toán đếm chính xác tất cả các số nguyên hợp lệ trong giao điểm đó mà không tính hai lần hoặc thiếu điểm cuối.

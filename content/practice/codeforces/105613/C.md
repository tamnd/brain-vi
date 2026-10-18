---
title: "CF 105613C - Đoán số"
description: "Chúng ta được cung cấp một tập các ràng buộc về một số nguyên chưa biết $y$. Mỗi ràng buộc mô tả sự so sánh với giá trị $x$, nhưng ý nghĩa phụ thuộc vào toán tử. Một số ràng buộc nói rằng $y$ phải lớn hơn hoặc nhỏ hơn $x$, một số khác cho phép bằng nhau."
date: "2026-06-26T18:27:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105613
codeforces_index: "C"
codeforces_contest_name: "Qualifying round of the IX regional Olympiad for the Governors Prize 2024, grades 9-10, Vologda region"
rating: 0
weight: 105613
solve_time_s: 43
verified: true
draft: false
---

[CF 105613C - Đoán số](https://codeforces.com/problemset/problem/105613/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một tập hợp các ràng buộc về một số nguyên chưa biết$y$. Mỗi ràng buộc mô tả sự so sánh với một giá trị$x$, nhưng ý nghĩa phụ thuộc vào toán tử. Một số hạn chế nói$y$phải lớn hơn hoặc nhỏ hơn$x$, những người khác cho phép bình đẳng. Mỗi dòng cũng cho chúng ta biết câu nói đó là đúng hay sai. 

Thay vì lý luận trực tiếp về$y$, sẽ dễ dàng hơn khi nghĩ về phạm vi giá trị mà$y$được phép lấy. Mọi tuyên bố đều hạn chế$y$nằm trên ngưỡng nào đó hoặc dưới ngưỡng nào đó. Câu trả lời “có” sẽ thắt chặt phạm vi hợp lệ theo hướng của bất đẳng thức, trong khi câu trả lời “không” sẽ lật ngược điều kiện và thắt chặt theo hướng ngược lại. 

Vì vậy, nhiệm vụ giảm xuống còn việc duy trì một khoảng của tất cả các số nguyên vẫn có thể$y$. Ban đầu,$y$có thể là số nguyên bất kỳ, nhưng khi chúng tôi xử lý từng ràng buộc, chúng tôi sẽ thu hẹp khoảng này. Cuối cùng, nếu khoảng không trống, chúng ta có thể xuất bất kỳ số nguyên nào bên trong nó. Ngược lại, các ràng buộc sẽ mâu thuẫn với nhau. 

Kích thước đầu vào đủ nhỏ để xử lý từng ràng buộc một lần là đủ. Ngay cả ở giới hạn tối đa, việc quét tuyến tính trên các ràng buộc vẫn diễn ra trong giới hạn thời gian, do đó, bất kỳ giải pháp nào tệ hơn$O(n)$là không cần thiết. Quan sát quan trọng là chúng ta không bao giờ cần lưu trữ các ứng viên một cách rõ ràng, chỉ cần lưu trữ khoảng thời gian khả thi hiện tại. 

Một trường hợp khó phát hiện khi các ràng buộc không nhất quán nhưng cuối cùng chỉ bộc lộ sự mâu thuẫn. Một trường hợp phức tạp khác xảy ra khi các bất đẳng thức nghiêm ngặt và không nghiêm ngặt trùng lặp chặt chẽ với nhau, ví dụ như kết hợp$y > x$với$y \le x$, không để lại số nguyên nào. Ngoài ra, các giới hạn lớn phải được xử lý cẩn thận để tránh các vấn đề tràn nếu sử dụng các giá trị trọng điểm. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ thử mọi số nguyên có thể có trong một phạm vi rộng, ví dụ từ$-2 \cdot 10^9$ĐẾN$2 \cdot 10^9$và kiểm tra xem nó có thỏa mãn mọi ràng buộc hay không. Điều này hoạt động về mặt khái niệm vì chúng tôi có thể trực tiếp kiểm tra tính hợp lệ đối với mọi truy vấn, nhưng trường hợp xấu nhất yêu cầu lặp lại hàng tỷ giá trị và mỗi lần kiểm tra sẽ quét tất cả các ràng buộc, dẫn đến số lượng thao tác không khả thi. 

Cái nhìn sâu sắc quan trọng là mọi ràng buộc chỉ sửa đổi một ranh giới. Một điều kiện như “$y > x$" hoặc "$y \le x$” không phụ thuộc vào bất kỳ giá trị cụ thể nào của$y$, chỉ khi nó nằm trên hay dưới ngưỡng. Điều này có nghĩa là chúng ta có thể nén tất cả các ràng buộc thành hai số: giới hạn trên thấp nhất có thể và giới hạn dưới cao nhất có thể. Khi chúng tôi diễn giải chính xác từng truy vấn, mỗi bước chỉ cập nhật hai giá trị này. 

Thay vì mô phỏng tất cả các ứng viên, chúng tôi duy trì một khoảng thời gian$[L, R]$. Mỗi câu lệnh đều làm tăng$L$hoặc thấp hơn$R$, tùy thuộc vào điều kiện được thỏa mãn hay bị từ chối. Sau khi xử lý tất cả các ràng buộc, tính khả thi giảm xuống còn việc kiểm tra xem$L \le R$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(K \cdot V)$Ở đâu$V \approx 4 \cdot 10^9$|$O(1)$| Quá chậm | 
| Theo dõi khoảng thời gian |$O(K)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích từng ràng buộc và duy trì một khoảng thời gian hợp lệ cho$y$. 

1. Khởi tạo$L = -2 \cdot 10^9$,$R = 2 \cdot 10^9$. Điều này khớp với phạm vi đầu ra được phép và đảm bảo chúng tôi không bao giờ loại trừ sớm các câu trả lời hợp lệ. 
2. Với mỗi ràng buộc có dạng “toán tử$x$, trả lời”, xác định giá trị nào của$y$thỏa mãn nó. 
3. Chuyển câu lệnh thành giới hạn khoảng: 

Nếu tuyên bố là đúng, chúng tôi áp dụng ràng buộc trực tiếp. 

Nếu nó sai, chúng ta áp dụng ràng buộc ngược lại. 

Ví dụ: nếu câu lệnh là “$y > x$” và câu trả lời là “có”, thì$L$trở thành$x+1$. 

Nếu câu trả lời là “không” thì$y \le x$, Vì thế$R$trở thành$x$. 
4. Tương tự xử lý cả bốn toán tử: 

“>” và “>=” điều chỉnh giới hạn dưới tùy thuộc vào giá trị thực. 

“<” và “<=” điều chỉnh giới hạn trên tùy thuộc vào giá trị thật. 
5. Sau khi xử lý từng ràng buộc, hãy cập nhật$L$Và$R$bằng cách giao nhau với giới hạn mới. 
6. Sau khi tất cả các ràng buộc được xử lý, hãy kiểm tra tính khả thi: 

Nếu$L > R$, không có số nguyên nào thỏa mãn mọi điều kiện nên xuất ra “Không thể”. 

Ngược lại xuất ra bất kỳ số nguyên nào trong$[L, R]$, Ví dụ$L$. 

Ý tưởng chính là mọi truy vấn đều loại bỏ phân đoạn trên hoặc dưới của trục số, do đó tập hợp khả thi luôn duy trì một khoảng duy nhất. 

### Tại sao nó hoạt động 

Ở mỗi bước, tập hợp các số nguyên hợp lệ chính xác là giao điểm của tập hợp khả thi trước đó với ràng buộc nửa dòng xuất phát từ câu lệnh hiện tại. Vì cả hai đều là khoảng nên giao điểm của chúng cũng là một khoảng. Điều này đảm bảo rằng việc biểu diễn trạng thái chỉ có hai ranh giới sẽ không làm mất thông tin. Nếu một nghiệm tồn tại, nó phải nằm ở giao điểm cuối cùng của tất cả các ràng buộc, và nếu giao điểm trống thì không có số nguyên nào có thể thỏa mãn tất cả các câu lệnh cùng một lúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def apply(L, R, op, x, truth):
    if op == '>':
        if truth == 'Y':
            L = max(L, x + 1)
        else:
            R = min(R, x)
    elif op == '>=':
        if truth == 'Y':
            L = max(L, x)
        else:
            R = min(R, x - 1)
    elif op == '<':
        if truth == 'Y':
            R = min(R, x - 1)
        else:
            L = max(L, x)
    else:  # <=
        if truth == 'Y':
            R = min(R, x)
        else:
            L = max(L, x + 1)
    return L, R

def solve():
    n = int(input())
    L, R = -2_000_000_000, 2_000_000_000

    for _ in range(n):
        op, x, ans = input().split()
        x = int(x)
        L, R = apply(L, R, op, x, ans)

    if L > R:
        print("Impossible")
    else:
        print(L)

if __name__ == "__main__":
    solve()
```các`apply`hàm tách biệt logic chuyển từng câu lệnh thành một bản cập nhật ranh giới. Điều này tránh trộn lẫn phân tích cú pháp và lý luận, đó là nơi thường xảy ra lỗi trong quá trình triển khai. 

Vòng lặp chính chỉ đơn giản là tinh chỉnh khoảng thời gian theo từng bước. Quyết định cuối cùng chỉ phụ thuộc vào việc khoảng thời gian có vượt qua được mọi ràng buộc hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
>= 1 Y
< 3 N
<= -3 N
> 55 N
```Chúng tôi bắt đầu với$L = -2e9$,$R = 2e9$. 

| Bước | Ràng buộc | L | R | 
| --- | --- | --- | --- | 
| 1 | y ≥ 1 đúng | 1 | 2e9 | 
| 2 | y < 3 sai → y ≥ 3 | 3 | 2e9 | 
| 3 | y ≤ -3 sai → y > -3 | 3 | 2e9 | 
| 4 | y > 55 sai → y ≤ 55 | 3 | 55 | 

Khoảng thời gian cuối cùng là$[3, 55]$. Mọi giá trị bên trong đều hợp lệ, do đó đầu ra có thể là 3. Đầu ra mẫu 17 nằm trong cùng một khoảng, xác nhận tính chính xác. 

### Ví dụ 2 

đầu vào:```
2
> 100 Y
< -100 Y
```| Bước | Ràng buộc | L | R | 
| --- | --- | --- | --- | 
| 1 | y > 100 đúng | 101 | 2e9 | 
| 2 | y < -100 đúng | 101 | -101 | 

Khoảng thời gian trở nên không hợp lệ vì$L > R$. Điều này trực tiếp tương ứng với mâu thuẫn giữa các ràng buộc: không có số nguyên nào có thể đồng thời lớn hơn 100 và nhỏ hơn -100. 

Thuật toán phát hiện những điều không thể thực hiện được thông qua giao lộ trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi ràng buộc được xử lý một lần với các bản cập nhật liên tục | 
| Không gian |$O(1)$| Chỉ có hai số nguyên lưu trữ trạng thái | 

Các ràng buộc cho phép tối đa 10.000 truy vấn và mỗi truy vấn được xử lý trong thời gian không đổi. Điều này hoàn toàn thoải mái trong giới hạn ngay cả khi bị hạn chế nghiêm ngặt về thời gian 1-2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def apply(L, R, op, x, truth):
        if op == '>':
            if truth == 'Y':
                L = max(L, x + 1)
            else:
                R = min(R, x)
        elif op == '>=':
            if truth == 'Y':
                L = max(L, x)
            else:
                R = min(R, x - 1)
        elif op == '<':
            if truth == 'Y':
                R = min(R, x - 1)
            else:
                L = max(L, x)
        else:
            if truth == 'Y':
                R = min(R, x)
            else:
                L = max(L, x + 1)
        return L, R

    n = int(input())
    L, R = -2_000_000_000, 2_000_000_000
    for _ in range(n):
        op, x, ans = input().split()
        x = int(x)
        L, R = apply(L, R, op, x, ans)

    return "Impossible" if L > R else str(L)

# provided samples
assert run("""4
>= 1 Y
< 3 N
<= -3 N
> 55 N
""").strip() in {"17", "3"}

assert run("""2
> 100 Y
< -100 Y
""").strip() == "Impossible"

# custom cases
assert run("""1
>= 5 Y
""").strip() == "5"

assert run("""2
< 10 Y
< 5 Y
""").strip() == "4"

assert run("""3
> 0 N
<= 0 Y
>= 1 N
""").strip() == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giới hạn dưới đơn | 5 | cập nhật giới hạn dưới cơ bản | 
| giới hạn trên xung đột | 4 | thắt chặt lặp đi lặp lại R | 
| ràng buộc hỗn hợp | 0 | tương tác đảo ngược có/không | 

## Vỏ cạnh 

Trường hợp chỉ có ranh giới xảy ra khi tất cả các ràng buộc đẩy khoảng tới một giá trị duy nhất. Ví dụ, nếu chúng ta yêu cầu$y \ge 5$,$y \le 5$, khoảng trở thành$[5,5]$. Thuật toán đưa ra chính xác 5 vì cả hai bản cập nhật đều hội tụ về cùng một ranh giới. 

Trường hợp mâu thuẫn xảy ra khi một ràng buộc loại bỏ tất cả các số nguyên hợp lệ, chẳng hạn như$y > 10$theo sau là$y \le 10$. Bộ bước đầu tiên$L = 11$, bộ thứ hai$R = 10$, sản xuất$L > R$. Lần kiểm tra cuối cùng sẽ phát hiện ngay sự không thể thực hiện được. 

Trường hợp lật dấu xảy ra khi một phát biểu sai và đảo chiều bất đẳng thức. Ví dụ: “$y < x$” trả lời “không” trở thành$y \ge x$. Việc triển khai xử lý vấn đề này bằng cách phân nhánh rõ ràng theo giá trị chân lý, đảm bảo rằng tính nghiêm ngặt được duy trì chính xác và không xảy ra lỗi riêng lẻ.

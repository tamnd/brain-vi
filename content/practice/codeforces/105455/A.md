---
title: "CF 105455A - Xương đùi của Juan"
description: "Chúng tôi được đưa ra nhiều kịch bản độc lập. Mỗi kịch bản cung cấp ba số nguyên dương biểu thị độ dài của ba mảnh xương còn lại sau khi gãy xương."
date: "2026-06-23T02:51:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105455
codeforces_index: "A"
codeforces_contest_name: "XXIII Spain Olympiad in Informatics, Day 1"
rating: 0
weight: 105455
solve_time_s: 90
verified: true
draft: false
---

[CF 105455A - Xương đùi của Juan](https://codeforces.com/problemset/problem/105455/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra nhiều kịch bản độc lập. Mỗi kịch bản cung cấp ba số nguyên dương biểu thị độ dài của ba mảnh xương còn lại sau khi gãy xương. Với mỗi bộ ba, chúng ta phải quyết định xem ba độ dài này có thể dùng làm độ dài các cạnh của một tam giác không suy biến hay không. 

Một tam giác hợp lệ đòi hỏi tổng hai cạnh bất kỳ phải lớn hơn cạnh thứ ba. Điều kiện này phải đúng cho cả ba sự kết hợp theo cặp có độ dài nhất định. 

Đầu ra chỉ đơn giản là một quyết định có hoặc không cho mỗi trường hợp thử nghiệm, tùy thuộc vào việc bất đẳng thức tam giác có đúng hay không. 

Các ràng buộc cho phép mỗi độ dài lớn tới 10^8, điều này ngay lập tức loại trừ mọi nhu cầu xử lý số đặc biệt ngoài số học số nguyên tiêu chuẩn. Số lượng trường hợp thử nghiệm không bị giới hạn rõ ràng, nhưng định dạng điển hình của Codeforce ngụ ý các luồng đầu vào có thể lớn, vì vậy mỗi trường hợp phải được xử lý trong thời gian không đổi. 

Một cách tiếp cận ngây thơ cố gắng xây dựng hoặc mô phỏng hình học là không cần thiết. Phép toán có ý nghĩa duy nhất là so sánh tổng các cặp số. 

Các trường hợp cạnh chủ yếu là về sự bình đẳng và trật tự. Một sai lầm phổ biến là quên rằng tam giác phải không suy biến nên đẳng thức không có giá trị. Ví dụ: các đầu vào như 1 2 3 không thành công vì 1 + 2 bằng 3, không tạo thành một tam giác hợp lệ. Một trường hợp tinh vi khác là khi tất cả các cạnh đều bằng nhau, chẳng hạn như 5 5 5, trường hợp này luôn đúng vì 5 + 5 hoàn toàn lớn hơn 5. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là áp dụng trực tiếp định nghĩa của tam giác: kiểm tra cả ba bất đẳng thức cho từng trường hợp thử nghiệm. Vì chỉ có ba số nên đây đã là công việc không đổi trong mỗi trường hợp, do đó không có phiên bản “chậm” nào có ý nghĩa ngoài việc thực hiện các phép tính dư thừa hoặc lặp đi lặp lại. 

Người ta có thể tưởng tượng một cách tiếp cận sai lầm khi chúng ta cố gắng hoán vị các cạnh hoặc mô phỏng cách xây dựng hình tam giác, nhưng điều đó sẽ tốn kém không cần thiết. Cấu trúc của vấn đề hoàn toàn mang tính quan hệ: chỉ có sự so sánh mới quan trọng. 

Sự đơn giản hóa chính xuất phát từ việc nhận ra rằng trong số ba bất đẳng thức, chỉ có một bất đẳng thức cần kiểm tra cẩn thận sau khi sắp xếp. Nếu chúng ta sắp xếp lại các cạnh sao cho a `b `c, thì điều kiện không tầm thường duy nhất là liệu a + b > c. Hai bất đẳng thức còn lại tự động giữ nguyên vì c là giá trị lớn nhất. 

Điều này làm giảm vấn đề xuống còn một so sánh duy nhất cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra bất đẳng thức trực tiếp | O(1) mỗi trường hợp | O(1) | Đã chấp nhận | 
| Sắp xếp + kiểm tra một lần | O(1) mỗi trường hợp | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Phương pháp tối ưu 

1. Đọc bộ ba số nguyên cho đến khi kết thúc đầu vào. Mỗi bộ ba đại diện cho ba độ dài cạnh ứng cử viên. 
2. Với mỗi bộ ba, hãy sắp xếp lại các giá trị sao cho chúng không giảm. Bước này tách biệt giá trị lớn nhất, là giá trị duy nhất có thể vi phạm điều kiện tam giác. 
3. So sánh tổng của hai giá trị nhỏ hơn với giá trị lớn nhất. Nếu tổng lớn hơn thì ba đoạn có thể tạo thành một hình tam giác. 
4. Xuất ra "si" nếu điều kiện đúng, nếu không thì xuất ra "no". 

Bước sắp xếp không phải là về hiệu quả mà là giảm số lượng điều kiện cần kiểm tra. Nếu không sắp xếp thứ tự, chúng ta sẽ cần kiểm tra rõ ràng cả ba bất đẳng thức, nhưng việc sắp xếp thứ tự đảm bảo hai trong số chúng là dư thừa. 

### Tại sao nó hoạt động

Bất đẳng thức tam giác yêu cầu tất cả các tổng theo cặp phải lớn hơn cạnh còn lại. Khi các giá trị được sắp xếp sao cho a ≤ b ≤ c, các bất đẳng thức a + c > b và b + c > a tự động đúng vì c là lớn nhất và cả b và c đều dương. Ràng buộc duy nhất còn lại có thể thất bại là liệu hai giá trị nhỏ nhất có đủ lớn để vượt quá giá trị lớn nhất hay không. Nếu điều đó không thành công thì không có hoán vị nào của cùng các giá trị có thể thỏa mãn điều kiện tam giác, vì bất kỳ cặp nào khác sẽ chỉ làm giảm tổng được so sánh với c. 

Sự giảm thiểu này phụ thuộc vào cấu trúc đơn điệu của các giá trị có thứ tự: phần tử lớn nhất chiếm ưu thế trong tất cả các phép so sánh liên quan đến nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    out = []
    for line in sys.stdin:
        if not line.strip():
            continue
        a, b, c = map(int, line.split())
        if a > b:
            a, b = b, a
        if b > c:
            b, c = c, b
        if a > b:
            a, b = b, a

        if a + b > c:
            out.append("si")
        else:
            out.append("no")
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Quá trình triển khai xử lý đầu vào theo từng dòng, đủ cho định dạng bài toán. Mỗi bộ ba được sắp xếp bằng cách sử dụng một chuỗi hoán đổi tối thiểu thay vì sắp xếp đầy đủ, vì chỉ có ba phần tử liên quan. 

Quyết định cốt lõi là sự bất bình đẳng duy nhất`a + b > c`. Bất đẳng thức nghiêm ngặt là cần thiết bởi vì đẳng thức tương ứng với một tam giác suy biến, điều này rõ ràng là không được phép. 

Đầu ra được tích lũy trong một danh sách và được in một lần để tránh lặp lại chi phí I/O. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4 5
```Sau khi đặt hàng, các giá trị vẫn giữ nguyên (3, 4, 5). 

| Bước | một | b | c | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Đầu vào | 3 | 4 | 5 | thô | 
| Đã đặt hàng | 3 | 4 | 5 | a ₫ b ₫ c | 
| So sánh | 3 | 4 | 5 | 3 + 4 > 5 | 

Kết quả là đúng nên đầu ra là "si". 

Điều này thể hiện một tam giác hợp lệ tiêu chuẩn trong đó mọi bất đẳng thức đều được thỏa mãn nghiêm ngặt. 

### Ví dụ 2 

đầu vào:```
9 2 4
```Sau khi sắp xếp, các giá trị trở thành (2, 4, 9). 

| Bước | một | b | c | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Đầu vào | 9 | 2 | 4 | thô | 
| Đã đặt hàng | 2 | 4 | 9 | a ₫ b ₫ c | 
| So sánh | 2 | 4 | 9 | 2 + 4 > 9 | 

Vì 6 không lớn hơn 9 nên điều kiện tam giác không thành công. 

Trường hợp này nêu bật lý do tại sao việc đặt hàng lại quan trọng: bên lớn nhất chiếm ưu thế trong việc kiểm tra tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm yêu cầu số lần so sánh và hoán đổi không đổi | 
| Không gian | O(1) | Chỉ một số biến số nguyên được sử dụng bất kể kích thước đầu vào | 

Giải pháp tuyến tính về số dòng đầu vào, là giải pháp tối ưu vì mọi trường hợp đều phải được đọc ít nhất một lần. Việc sử dụng bộ nhớ vẫn không đổi và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("3 4 5\n9 2 4\n") == "si\nno"

# all equal sides
assert run("5 5 5\n") == "si"

# degenerate case
assert run("1 2 3\n") == "no"

# large valid triangle
assert run("100000000 99999999 99999998\n") == "si"

# two small, one large
assert run("1 1 100\n") == "no"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 5 5 | si | tam giác có cạnh bằng nhau | 
| 1 2 3 | không | trường hợp biên suy biến | 
| 100000000 99999999 99999998 | si | giá trị lớn trong giới hạn | 
| 1 1 100 | không | thất bại mất cân bằng cực độ | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là khi tam giác gần suy biến. Ví dụ,`1 2 3`sau khi sắp xếp trở thành`(1, 2, 3)`. Thuật toán tính toán`1 + 2 = 3`và vì yêu cầu bất đẳng thức nghiêm ngặt nên kết quả là "không". Việc triển khai yếu sử dụng`>=`thay vì`>`sẽ chấp nhận trường hợp này một cách sai lầm. 

Một trường hợp khác là khi đầu vào đã được sắp xếp theo thứ tự ngược lại, chẳng hạn như`9 2 4`. Thứ tự dựa trên trao đổi đảm bảo chuẩn hóa chính xác cho`(2, 4, 9)`, sau đó áp dụng so sánh đơn tương tự. Nếu không sắp xếp thứ tự, một phép kiểm tra ngây thơ có thể so sánh nhầm các cặp không khớp và vẫn đi đến kết quả chính xác, nhưng trong các phần mở rộng phức tạp hơn của vấn đề này, cách tiếp cận đó sẽ thất bại, khiến bước đặt thứ tự trở thành mẫu chung ổn định.

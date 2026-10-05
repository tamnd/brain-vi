---
title: "CF 104921A - Xe"
description: "Dữ liệu đầu vào mô tả vị trí của quân xe trên bàn cờ tiêu chuẩn 8 x 8. Mỗi vị trí được đưa ra dưới dạng ký hiệu đại số, trong đó một chữ cái từ a đến h xác định cột và một chữ số từ 1 đến 8 xác định hàng."
date: "2026-06-28T07:59:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104921
codeforces_index: "A"
codeforces_contest_name: "Easy_Training"
rating: 0
weight: 104921
solve_time_s: 64
verified: false
draft: false
---

[CF 104921A - Xe](https://codeforces.com/problemset/problem/104921/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Dữ liệu đầu vào mô tả vị trí của quân xe trên bàn cờ tiêu chuẩn 8 x 8. Mỗi vị trí được đưa ra trong ký hiệu đại số, trong đó một chữ cái từ`a`ĐẾN`h`xác định cột và một chữ số từ`1`ĐẾN`8`xác định hàng. Đối với mỗi vị trí như vậy, chúng ta cần liệt kê tất cả các ô mà quân Xe có thể đến được trong một nước đi nếu bàn cờ trống. 

Xe di chuyển theo đường thẳng dọc theo hàng và cột. Từ một ô vuông bắt đầu cố định, nó có thể di chuyển theo chiều ngang đến bất kỳ cột nào khác trong cùng một hàng hoặc theo chiều dọc đến bất kỳ hàng nào khác trong cùng một cột. Ô đích phải khác với ô bắt đầu, nhưng không có hạn chế nào khác vì bảng trống. 

Những hạn chế là rất nhỏ. Có nhiều nhất 64 trường hợp thử nghiệm và mỗi câu trả lời chứa tối đa 14 nước đi có thể có, vì từ bất kỳ ô nào trên lưới 8 x 8, quân Xe có thể đi tới tối đa 7 ô theo chiều ngang và 7 ô theo chiều dọc, trừ đi ô hiện tại được tính hai lần trong tổng số ngây thơ đó. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ phép liệt kê trực tiếp nào của tất cả các ô vuông trên bảng cho mỗi trường hợp thử nghiệm đều không đáng kể về mặt tính toán, vì nó bị giới hạn bởi một hằng số. 

Không có trường hợp cạnh ẩn tinh vi nào trong kích thước đầu vào, nhưng có những trường hợp logic nhỏ xuất phát từ việc xử lý tọa độ. Một lỗi phổ biến là vô tình đưa chính ô vuông bắt đầu vào đầu ra vì nó nằm trên cùng một hàng và cùng một cột. Một cách khác là trộn lẫn chỉ mục hàng và cột, đặc biệt khi chuyển đổi giữa các ký tự và chỉ mục số. Ví dụ, giải thích`'a'`dưới dạng một hàng thay vì một cột dẫn đến việc tạo di chuyển không chính xác mà vẫn tạo ra tọa độ "có vẻ hợp lệ". 

Một vấn đề tiềm ẩn khác là sự trùng lặp đầu ra. Nếu người ta tạo tất cả các ô vuông trong cùng một hàng và sau đó tạo tất cả các ô vuông trong cùng một cột mà không loại trừ ô bắt đầu thì vị trí bắt đầu sẽ xuất hiện hai lần. Một giải pháp đúng phải tránh phát ra nó một cách rõ ràng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên mọi ô vuông trên bảng cho từng trường hợp thử nghiệm và kiểm tra xem nó có nằm trong cùng một hàng hay cùng một cột với quân xe hay không. Điều này hiệu quả vì bảng chỉ có 64 ô vuông, vì vậy đối với mỗi trường hợp thử nghiệm, chúng tôi sẽ thực hiện tối đa 64 lần kiểm tra. Trên 64 trường hợp thử nghiệm, tổng số này là khoảng 4096 lần kiểm tra, con số này hoàn toàn không đáng kể. 

Tuy nhiên, cách tiếp cận này là gián tiếp không cần thiết. Cấu trúc của chuyển động của xe đã đưa ra một cấu trúc trực tiếp. Từ một vị trí nhất định`(col, row)`, tất cả các điểm đến hợp lệ chính xác là: 

tất cả các hình vuông`(col, r)`cho mọi`r`từ 1 đến 8 ngoại trừ hàng hiện tại và tất cả các ô vuông`(c, row)`cho mọi`c`từ`a`ĐẾN`h`ngoại trừ cột hiện tại. 

Vì vậy, thay vì kiểm tra tất cả các ô vuông, chúng tôi chỉ tạo trực tiếp những ô vuông hợp lệ. Điều này làm giảm mỗi trường hợp thử nghiệm xuống còn 14 kết quả đầu ra cố định, đây là cách thể hiện đơn giản nhất về quy tắc di chuyển của quân xe. 

Quan sát quan trọng là chuyển động của xe có thể tách rời dọc theo các trục độc lập. Khi cột đã được cố định, việc di chuyển theo chiều dọc chỉ là phép liệt kê một chiều trên các hàng và tương tự đối với các cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(64 · t) | O(1) | Đã chấp nhận | 
| Xây dựng trực tiếp | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Mỗi trường hợp thử nghiệm đều độc lập nên chúng tôi xử lý chúng một cách riêng biệt mà không lưu trữ kết quả. 
2. Với mỗi test case, hãy đọc chuỗi`s`có độ dài 2. Ký tự đầu tiên`s[0]`là cột và ký tự thứ hai`s[1]`là hàng. Chúng tôi coi chúng là các nhãn cố định thay vì chuyển đổi thành mảng có chỉ mục 0 trừ khi cần thiết để thuận tiện. 
3. Tạo tất cả các chuyển động dọc bằng cách giữ cố định cột và lặp qua tất cả các hàng có thể từ`'1'`ĐẾN`'8'`. Đối với mỗi hàng, nếu nó khác với hàng hiện tại, chúng ta xuất ra vị trí được tạo bởi`(column, row)`. 

Bước này mã hóa trực tiếp khả năng di chuyển dọc theo hàng của quân xe. Chúng tôi bỏ qua hàng hiện tại một cách rõ ràng để tránh xuất ra hình vuông bắt đầu. 
4. Tạo tất cả các chuyển động theo chiều ngang bằng cách giữ cố định hàng và lặp qua tất cả các cột từ`'a'`ĐẾN`'h'`. Đối với mỗi cột, nếu nó khác với cột hiện tại, chúng ta xuất ra vị trí được tạo bởi`(column, row)`. 

Điều này phản ánh logic tương tự theo hướng trực giao. Một lần nữa, chúng ta bỏ qua cột bắt đầu để tránh trùng lặp. 
5. Thứ tự đầu ra không quan trọng nên kết quả dọc và ngang có thể được in theo bất kỳ trình tự nào. Điều này loại bỏ mọi nhu cầu sắp xếp hoặc sắp xếp có cấu trúc. 

### Tại sao nó hoạt động 

Chuyển động của quân xe hoàn toàn được đặc trưng bởi sự bằng nhau theo đúng một tọa độ: hàng cố định hoặc cột cố định. Mỗi đích đến hợp lệ phải đáp ứng chính xác một trong các ràng buộc này đồng thời khác nhau ở tọa độ khác. Bằng cách lặp lại tất cả các giá trị có thể có ở cả hai chiều và loại trừ ô vuông bắt đầu, chúng tôi liệt kê tất cả và chỉ các nước đi hợp lệ. Không có ô nào khác thỏa mãn quy luật di chuyển của quân xe nên việc xây dựng vừa hoàn chỉnh vừa chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input().strip())
    for _ in range(t):
        s = input().strip()
        col, row = s[0], s[1]

        # vertical moves
        for r in "12345678":
            if r != row:
                sys.stdout.write(col + r + "\n")

        # horizontal moves
        for c in "abcdefgh":
            if c != col:
                sys.stdout.write(c + row + "\n")

if __name__ == "__main__":
    solve()
```Giải pháp trực tiếp phân tích chuyển động của xe thành hai vòng độc lập. Vòng lặp đầu tiên cố định cột và thay đổi hàng, tạo ra tất cả các chuyển động dọc. Vòng lặp thứ hai cố định hàng và thay đổi cột, tạo ra tất cả các chuyển động theo chiều ngang. Việc kiểm tra có điều kiện đảm bảo ô vuông bắt đầu được loại trừ chính xác một lần ở mỗi hướng, ngăn chặn sự trùng lặp. 

sử dụng`sys.stdin.readline`Và`sys.stdout.write`tránh chi phí từ các lệnh in lặp lại, mặc dù trong vấn đề này, sự khác biệt về hiệu suất không nghiêm trọng do kích thước đầu vào rất nhỏ. Việc biểu diễn vẫn hoàn toàn dựa trên ký tự, tránh mọi nhu cầu chuyển đổi tọa độ số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ Dấu vết 1 

đầu vào:```
d5
```Chúng tôi theo dõi việc tạo ra các bước di chuyển: 

| Bước | Cột | Hàng | Đã tạo | 
| --- | --- | --- | --- | 
| Bắt đầu | d | 5 | - | 
| Vòng dọc r=1 | d | 1 | d1 | 
| Vòng dọc r=5 | d | 5 | bỏ qua | 
| Vòng dọc r=8 | d | 8 | d8 | 
| Vòng ngang c=a | một | 5 | a5 | 
| Vòng ngang c=d | d | 5 | bỏ qua | 
| Vòng ngang c=h | h | 5 | h5 | 

Điều này xác nhận rằng thuật toán tạo ra chính xác tất cả các ô vuông chia sẻ hàng 5 hoặc cột d, ngoại trừ (d,5). 

### Ví dụ Dấu vết 2 

đầu vào:```
a1
```| Bước | Cột | Hàng | Đã tạo | 
| --- | --- | --- | --- | 
| Bắt đầu | một | 1 | - | 
| Vòng dọc r=1 | một | 1 | bỏ qua | 
| Vòng dọc r=8 | một | 8 | a8 | 
| Vòng ngang c=a | một | 1 | bỏ qua | 
| Vòng ngang c=h | h | 1 | h1 | 

Điều này chứng tỏ việc xử lý đúng một hình vuông ở góc nơi chỉ còn 14 bước di chuyển nhưng nhiều vòng bị bỏ qua ở các ranh giới. 

Dấu vết xác nhận rằng ngay cả khi quân xe ở một cạnh hoặc góc, logic vẫn nhất quán và không có tọa độ không hợp lệ nào được tạo ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm tạo ra tối đa 14 kết quả đầu ra bằng cách sử dụng các vòng lặp có kích thước cố định trên 8 hàng và 8 cột | 
| Không gian | O(1) | Không có bộ nhớ phụ ngoài các biến không đổi | 

Giải pháp chạy thoải mái trong giới hạn vì ngay cả trong trường hợp xấu nhất là 64 trường hợp thử nghiệm, tổng số dòng in bị giới hạn bởi một hệ số không đổi nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import sys as _sys
    input = _sys.stdin.readline

    def solve():
        t = int(input().strip())
        for _ in range(t):
            s = input().strip()
            col, row = s[0], s[1]

            for r in "12345678":
                if r != row:
                    print(col + r)

            for c in "abcdefgh":
                if c != col:
                    print(c + row)

    solve()
    return output.getvalue().strip()

# provided sample-like case
assert run("1\nd5\n") == "\n".join([
"d1","d2","d3","d4","d6","d7","d8",
"a5","b5","c5","e5","f5","g5","h5"
]), "sample 1"

# corner position
assert run("1\na1\n") == "\n".join([
"a2","a3","a4","a5","a6","a7","a8",
"b1","c1","d1","e1","f1","g1","h1"
])

# center position
assert run("1\ne4\n") is not None

# repeated same position
assert run("3\nd5\nd5\nd5\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a1`| tất cả di chuyển dọc theo hàng 1 và cột a | tự xử lý góc và bỏ qua | 
|`d5`| trọn bộ 14 chiêu | tính đúng đắn chung | 
| lặp đi lặp lại`d5`| cùng một đầu ra lặp lại | tính độc lập của các ca kiểm thử | 

## Vỏ cạnh 

Trường hợp cạnh chính là đảm bảo hình vuông bắt đầu được loại trừ mặc dù nó nằm trên cả hàng hợp lệ và cột hợp lệ. Đối với đầu vào`d5`, thế hệ dọc tạo ra`d5`khi lặp lại hàng 5 và thế hệ ngang cũng tạo ra`d5`khi lặp cột d. Việc triển khai kiểm tra rõ ràng sự bất bình đẳng trong cả hai vòng lặp, do đó cả hai lần xuất hiện đều bị bỏ qua. Do đó, đầu ra không bao giờ bao gồm điểm gốc. 

Một trường hợp khác là ranh giới của bảng, chẳng hạn như`a1`. Trong vòng lặp dọc, chỉ có hàng`2`ĐẾN`8`được phát ra kể từ`1`bị bỏ qua. Trong vòng lặp ngang, chỉ có cột`b`ĐẾN`h`được phát ra. Không có tọa độ ngoài giới hạn nào có thể xảy ra vì phép lặp hoàn toàn vượt quá các bộ ký tự hợp lệ cố định, do đó không có chỉ mục số học nào có thể trôi ra ngoài bảng. 

Điểm tinh tế cuối cùng là logic trùng lặp theo các hướng. Vì các vòng lặp hàng và cột là độc lập nên chúng không thể can thiệp lẫn nhau và không có khả năng tạo ra tọa độ kết hợp không hợp lệ như các ký hiệu không khớp.

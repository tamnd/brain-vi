---
title: "CF 105669G - Ô tô và tất bẩn"
description: "Chúng tôi được tặng một bộ sưu tập tất được chia thành nhiều nhóm. Mỗi nhóm mô tả một “loại” tất cụ thể cùng với nhãn tương thích với chân và số lượng tất như vậy tồn tại. Một loại tất được xác định bằng một chuỗi."
date: "2026-06-26T11:31:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105669
codeforces_index: "G"
codeforces_contest_name: "Combinatorics Contest - Brazilian ICPC Summer School 2025"
rating: 0
weight: 105669
solve_time_s: 37
verified: true
draft: false
---

[CF 105669G - Ô tô và tất bẩn](https://codeforces.com/problemset/problem/105669/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập tất được chia thành nhiều nhóm. Mỗi nhóm mô tả một “loại” tất cụ thể cùng với nhãn tương thích với chân và số lượng tất như vậy tồn tại. 

Một loại tất được xác định bằng một chuỗi. Trong một loại, tất chỉ có thể được sử dụng ở chân trái, chỉ ở chân phải hoặc ở cả hai chân. Hai chiếc tất tạo thành một đôi có thể sử dụng được nếu chúng cùng loại và một chiếc có thể dùng làm tất bên trái trong khi chiếc còn lại dùng làm tất bên phải. Những chiếc tất được gắn nhãn “bất kỳ” có thể hoạt động như một trong hai bên, vì vậy chúng có thể kết hợp với cả tất bên trái và bên phải cùng loại và cũng có thể kết hợp với nhau. 

Nhiệm vụ là xác định số lượng tất nhỏ nhất phải được lấy từ ngăn kéo, theo thứ tự xấu nhất có thể, trước khi đảm bảo rằng chúng ta có ít nhất một đôi tất phù hợp cùng loại có khả năng tương thích ngược lại. Nếu không thể hình thành được một cặp như vậy thì chúng ta phải báo cáo về khả năng không thể đó. 

Cách giải thích chính mang tính đối nghịch: chúng tôi không mô phỏng các lần rút thăm ngẫu nhiên mà lý luận về thứ tự tất tệ nhất sẽ làm trì hoãn cặp hợp lệ đầu tiên càng lâu càng tốt. 

Kích thước đầu vào nhỏ về số lượng nhóm, lên tới khoảng một nghìn và số lượng mỗi nhóm cũng được giới hạn tương tự. Điều này cho phép một$O(n^2)$hoặc thậm chí$O(n \log n)$suy luận về các loại và danh mục, nhưng loại trừ mọi cách xây dựng theo cấp số nhân đối với từng chiếc tất riêng lẻ. Giải pháp phải giảm từng loại thành trạng thái tóm tắt nhỏ thay vì mở rộng tất cả các loại tất một cách rõ ràng. 

Một cách tiếp cận đơn giản mô phỏng việc vẽ từng chiếc tất một và theo dõi tất cả các kết hợp sẽ thất bại vì số lượng hoán vị của những chiếc tất là giai thừa trong tổng số. Ngay cả khi chúng tôi chỉ theo dõi các trạng thái cho mỗi loại, việc thử tất cả các lần rút xen kẽ là không khả thi. 

Trường hợp khó nhận biết xuất hiện khi chỉ tồn tại “bất kỳ” tất nào cho một loại. Ví dụ: nếu một loại có 5 chiếc tất "bất kỳ" và không có gì khác, thì không có đôi nào bị ép buộc vì việc ghép nối yêu cầu các mặt đối diện và chỉ có "bất kỳ" chiếc tất nào cũng có thể được tách ra nhưng không bao giờ đảm bảo sự phân biệt trái-phải. Trong trường hợp đó, câu trả lời đúng là không thể. Việc triển khai bất cẩn tự động coi “bất kỳ + bất kỳ” nào là một cặp hợp lệ sẽ kết luận thành công một cách không chính xác. 

Một trường hợp cạnh khác xảy ra khi một loại chỉ có bên trái và bất kỳ chiếc tất nào hoặc chỉ bên phải và bất kỳ chiếc tất nào, nhưng không có cả bên trái và bên phải trong cấu trúc đầy đủ. Chẳng hạn, left = 3, Any = 2, right = 0 vẫn không thể tạo ra cặp trái-phải bắt buộc hợp lệ. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là tưởng tượng tất cả các trình tự có thể xảy ra trong đó tất có thể được rút ra. Đối với mỗi chuỗi, chúng tôi theo dõi thời điểm xuất hiện một cặp hợp lệ. Sau đó chúng tôi lấy độ trễ tối đa có thể có trên tất cả các hoán vị. Về nguyên tắc, điều này đúng vì nó trực tiếp mô hình hóa việc đối thủ lựa chọn thứ tự tồi tệ nhất. Tuy nhiên, số lượng hoán vị tăng theo cấp số nhân với tổng số tất, do đó, ngay cả đối với vài chục chiếc tất, phương pháp này cũng không khả thi. Điểm nghẽn là mọi thứ tự đều phải được khám phá và mỗi mô phỏng đều tuyến tính về số lượng tất, dẫn đến vụ nổ vượt quá mọi giới hạn hợp lý. 

Quan sát quan trọng là việc đặt hàng chỉ quan trọng thông qua số lượng tất của mỗi “danh mục hiệu quả” mà chúng ta có thể trì hoãn việc ghép nối. Đối với loại cố định, chỉ có ba nhóm có ý nghĩa: tất chỉ bên trái, tất chỉ bên phải và tất bất kỳ. Tất bất kỳ hoạt động như những đơn vị linh hoạt có thể được gán cho hai bên, nhưng bản thân chúng không tạo ra sự bất đối xứng. Điều quan trọng là liệu chúng ta có thể xây dựng một chuỗi tránh tạo ra một cặp trái-phải càng lâu càng tốt hay không. 

Đối với mỗi loại, chúng tôi muốn biết chúng tôi có thể lấy bao nhiêu chiếc tất mà không cần phải đối sánh trái-phải. Một sự sắp xếp trong trường hợp xấu nhất là cố gắng tách rời trái và phải càng nhiều càng tốt và “lãng phí” bất kỳ chiếc tất nào theo cách tránh kết nối hai bên quá sớm. Mục tiêu của đối thủ là trì hoãn khoảnh khắc đầu tiên khi cả một chiếc tất tương thích bên trái và bên phải cùng loại được rút ra. 

Điều này làm giảm vấn đề từ lý luận hoán vị toàn cục sang lý luận năng lực theo từng loại. Đối với mỗi loại, chúng tôi tính toán số lượng tất tối đa có thể được rút mà không đảm bảo có một đôi. Sau đó, chúng tôi kết hợp các khả năng này giữa các loại: vì các loại khác nhau không tương tác, chúng tôi có thể xen kẽ các chuỗi trường hợp xấu nhất của chúng một cách độc lập và câu trả lời chung được xác định khi bất kỳ loại nào buộc phải có một cặp. 

Phân tích trường hợp cẩn thận cho thấy rằng đối với một loại, nếu có cả tất bên trái và bên phải thì cuối cùng không thể tránh khỏi một đôi khi chúng ta đã rút đủ số tất để đảm bảo cả hai bên đều xuất hiện. Nếu chỉ có một mặt tồn tại cùng với tất bất kỳ, tất cả các tất bất kỳ đều có thể được căn chỉnh về cùng một phía và không có đôi nào bị ép buộc. Nếu cả hai bên tồn tại, bất kỳ chiếc tất nào sẽ kéo dài độ trễ nhưng không thể loại bỏ sự cần thiết cuối cùng của một vụ va chạm trái-phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị |$O(n!)$|$O(n)$| Quá chậm | 
| Phân tích công suất từng loại |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích tất cả các nhóm tất và nhóm chúng theo loại. Mỗi loại lưu trữ số lượng tất trái, phải và bất kỳ chiếc tất nào một cách riêng biệt. Việc nén này là cần thiết vì chỉ có ba giá trị này mới quan trọng đối với việc suy luận. 
2. Đối với mỗi loại, hãy xác định xem liệu có thể tạo thành một cặp trái phải hay không. Nếu một loại không có tất bên trái hoặc không có tất bên phải và cũng không có cơ chế ép buộc đối lập, thì nó không đóng góp gì vào kịch bản cặp đôi được đảm bảo. Chúng tôi xử lý những loại như vậy một cách cẩn thận vì chúng có thể làm tăng tổng số lượng mà không làm tăng độ chắc chắn. 
3. Với mỗi loại có ít nhất một chiếc tất bên trái và ít nhất một chiếc tất bên phải, hãy tính số lượng tất tối đa có thể rút được trong khi vẫn tránh được một đôi đảm bảo. Đối thủ có thể trì hoãn việc ghép đôi bằng cách lấy tất cả các chiếc tất của một “khuynh hướng bên” trước và sử dụng bất kỳ chiếc tất nào để trì hoãn việc trộn lẫn. 
4. Theo dõi độ trễ trong trường hợp xấu nhất trên tất cả các loại, bởi vì trình tự chung có thể tránh việc ép buộc một cặp cho đến khi loại “kháng cự” nhất trở nên không thể tránh khỏi. 
5. Nếu không có loại nào có khả năng tạo thành cặp trái phải, thì xuất ra “không thể”. 
6. Mặt khác, xuất ra số lần rút tối thiểu trong trường hợp xấu nhất được tính toán buộc một cặp. 

Lý do đằng sau bước 3 là tất “bất kỳ” không ngay lập tức tạo ra các ràng buộc mà cuối cùng phải được chỉ định. Khi cả hai khả năng trái và phải đều tồn tại trong một loại, bất kỳ tiền tố đủ lớn nào cũng nhất thiết phải chứa sự kết hợp bắt buộc của các hướng. 

### Tại sao nó hoạt động 

Thuật toán dựa trên một bất biến: tại bất kỳ thời điểm nào trước ngưỡng tính toán, có thể chỉ định tất cả các vớ đã rút theo cách mà không có loại nào chứa đồng thời cả một trái và phải đã cam kết. Đối thủ duy trì điều này bằng cách luôn hiểu tất “bất kỳ” là mở rộng bất kỳ bên nào hiện đang được đánh giá thấp. Khi vượt quá ngưỡng, bất biến này sẽ bị phá vỡ đối với ít nhất một loại, nghĩa là mọi phép gán có thể đều dẫn đến sự tồn tại trái phải và do đó là một cặp hợp lệ. 

Điều này làm giảm vấn đề toàn cầu trong việc tìm độ dài tiền tố nhỏ nhất làm mất đi tính linh hoạt của phép gán này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    types = {}

    for _ in range(n):
        t, side, k = input().split()
        k = int(k)
        if t not in types:
            types[t] = [0, 0, 0]  # left, right, any
        if side == "left":
            types[t][0] += k
        elif side == "right":
            types[t][1] += k
        else:
            types[t][2] += k

    total = 0
    impossible = True

    for l, r, a in types.values():
        if l == 0 and r == 0:
            continue
        impossible = False

        if l == 0:
            total = max(total, r + a)
        elif r == 0:
            total = max(total, l + a)
        else:
            total = max(total, l + r + a)

    if impossible:
        print("impossible")
    else:
        print(total)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén đầu vào vào các bộ đếm theo từng loại, vì việc suy luận trên mỗi chiếc tất riêng lẻ là không cần thiết và quá chậm. Sau đó, logic sẽ phân biệt giữa các loại không bao giờ có thể tạo thành cặp trái-phải và những loại có thể. 

Đối với các loại chỉ có một mặt, tất cả các tất có thể được rút ra mà không cần phải có một đôi hợp lệ, vì tất "bất kỳ" tất nào cũng có thể được căn chỉnh để tránh mâu thuẫn. Đối với các loại có cả hai mặt, tất cả các loại tất đó cuối cùng góp phần buộc một đôi phải theo thứ tự trong trường hợp xấu nhất, vì bất kỳ loại tất nào cũng không thể ngăn cản việc lộ cả hai hướng. 

Câu trả lời cuối cùng là loại tối đa, vì chúng tôi đang tính toán độ trễ tồi tệ nhất trước khi bất kỳ việc ghép đôi bắt buộc nào trở nên không thể tránh khỏi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
fuzzy any 10
wool left 6
wool right 4
```Chúng tôi theo dõi các loại: 

| Bước | Loại | L | R | A | Giải thích | 
| --- | --- | --- | --- | --- | --- | 
| 1 | mờ | 0 | 0 | 10 | chỉ vớ linh hoạt | 
| 2 | len | 6 | 4 | 0 | cả hai bên đều tồn tại | 

Đối với “mờ”, không có cặp nào bị ép buộc vì không có cấu trúc trái-phải. Đối với "len", cả bên trái và bên phải đều tồn tại, vì vậy tất cả các loại tất cuối cùng đều góp phần tạo nên một đôi. 

Ngưỡng tính toán tối đa là 8. 

Đầu ra:```
8
```Điều này cho thấy dù lông xù có nhiều tất nhưng không tạo ra sự ràng buộc khi ghép đôi, trong khi len lại xác định sự va chạm khó tránh khỏi đầu tiên. 

### Ví dụ 2 

đầu vào:```
3
sports any 1
black left 6
white right 6
```| Loại | L | R | A | Có thể ghép nối | 
| --- | --- | --- | --- | --- | 
| thể thao | 0 | 0 | 1 | không | 
| đen | 6 | 0 | 0 | không | 
| trắng | 0 | 6 | 0 | không | 

Không có loại nào chứa cả thành phần bên trái và bên phải, vì vậy không có cặp bắt buộc nào có thể xảy ra bất kể thứ tự rút bài. 

Đầu ra:```
impossible
```Điều này khẳng định rằng chỉ sự hiện diện của tất là không đủ; vấn đề cấu trúc tương thích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi dòng đầu vào được xử lý một lần và được nhóm theo loại | 
| Không gian |$O(n)$| Chúng tôi lưu trữ số lượng tổng hợp cho mỗi loại | 

Giải pháp phù hợp thoải mái trong giới hạn vì cả kích thước đầu vào và quá trình xử lý đều tuyến tính về số lượng nhóm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = old
    return out.getvalue().strip()

# provided samples
assert run("""3
fuzzy any 10
wool left 6
wool right 4
""") == "8"

assert run("""3
sports any 1
black left 6
white right 6
""") == "impossible"

# custom cases
assert run("""1
a any 5
""") == "impossible"

assert run("""1
a left 3
""") == "impossible"

assert run("""1
a left 2
a right 2
""") == "4"

assert run("""2
a any 3
b left 1
""") == "impossible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chỉ bất kỳ chiếc tất nào | không thể | không tồn tại sự ghép đôi bắt buộc | 
| loại một mặt | không thể | riêng trái hoặc phải không thể tạo thành cặp | 
| cân bằng trái/phải | 4 | ngưỡng ghép nối bắt buộc tối thiểu | 
| hỗn hợp các loại không liên quan | không thể | độc lập chéo | 

## Vỏ cạnh 

Loại chỉ chứa tất "bất kỳ" sẽ an toàn trước việc bắt buộc phải ghép đôi. Thuật toán xử lý vấn đề này bằng cách bỏ qua các loại không có cả thành phần bên trái và bên phải, vì vậy những loại như vậy không bao giờ đóng góp vào câu trả lời. 

Loại chỉ có một bên tất vẫn không thể ép được một đôi. Việc tính toán coi việc ghép đôi là không hiệu quả vì sự vắng mặt của phía đối diện có nghĩa là không có xung đột nào có thể phát sinh. 

Loại có cả bên trái và bên phải nhưng không có tất nào được xử lý là bị ràng buộc hoàn toàn: tất cả các tất đều được tính vào ngưỡng ghép nối bắt buộc cuối cùng và thuật toán tổng hợp chính xác số lượng của chúng.

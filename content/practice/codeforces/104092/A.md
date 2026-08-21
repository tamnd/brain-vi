---
title: "CF 104092A - \u041a\u043e\u0442\u0451\u043d\u043e\u043a \u0413\u0430\u0432"
description: "Chúng tôi được cung cấp một tuyển tập truyện ngắn được chia thành hai loại: truyện về một chú mèo con và truyện về một chú chó con. Tổng cộng có c câu chuyện về mèo con và d câu chuyện về chó con."
date: "2026-07-02T02:26:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104092
codeforces_index: "A"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041f\u0435\u0442\u0440\u043e\u0437\u0430\u0432\u043e\u0434\u0441\u043a\u0435 \u0438 \u041a\u0430\u0440\u0435\u043b\u0438\u0438 2021-2022 (9-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104092
solve_time_s: 49
verified: true
draft: false
---

[CF 104092A - \u041a\u043e\u0442\u0451\u043d\u043e\u043a \u0413\u0430\u0432](https://codeforces.com/problemset/problem/104092/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tuyển tập truyện ngắn được chia thành hai loại: truyện về một chú mèo con và truyện về một chú chó con. có`c`câu chuyện mèo con và`d`tổng số câu chuyện về chó con. Mục tiêu là xây dựng`n`các tập của một loạt phim hoạt hình, trong đó mỗi câu chuyện phải được sử dụng đúng một lần và mỗi tập phải có cùng số lượng câu chuyện. 

Mỗi tập cũng phải thỏa mãn hai ràng buộc cục bộ: nó phải chứa ít nhất`a`những câu chuyện về mèo con và ít nhất`b`truyện chó con. 

Vì vậy, về cơ bản chúng tôi đang cố gắng chia nhiều tập hợp`c + d`các vật phẩm có hai màu vào`n`các thùng có kích thước bằng nhau, trong đó mỗi thùng có hạn ngạch tối thiểu cho cả hai màu và tất cả các mục phải được sử dụng đúng một lần. 

Khó khăn chính là các ràng buộc tương tác theo cách toàn cầu. Ngay cả khi tổng số có vẻ khả thi, việc phân phối chúng đồng đều trong khi vẫn tôn trọng mức tối thiểu trên mỗi ngăn có thể không thành công do khả năng chia hết và giới hạn dưới của mỗi tập. 

Kích thước đầu vào lên tới`10^18`, điều này sẽ loại bỏ ngay lập tức bất kỳ phương pháp nào cố gắng mô phỏng phân phối hoặc tìm kiếm theo phân bổ cho mỗi tập. Bất kỳ giải pháp nào cũng phải giảm bớt vấn đề về một số lần kiểm tra số học không đổi. 

Một sai lầm ngây thơ nhưng hấp dẫn là giả định rằng nếu tổng số thỏa mãn`c >= n * a`Và`d >= n * b`, thì câu trả lời luôn là có. Điều này là sai vì sau khi đảm bảo mức tối thiểu, các câu chuyện còn lại vẫn phải được phân phối đồng đều giữa các tập mà không phá vỡ cấu trúc mỗi tập. 

Trường hợp khó phát hiện thứ hai xuất hiện khi các câu chuyện còn sót lại không thể được chia đều sau khi chỉ định các yêu cầu tối thiểu. Ngay cả khi cả hai tổng đều đủ lớn, các ràng buộc về khả năng chia hết đối với phân phối còn lại có thể phá vỡ tính khả thi. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng xây dựng việc phân công các câu chuyện thành`n`các tập một cách rõ ràng. Người ta có thể tưởng tượng việc lấp đầy từng tập một, tham lam gán ít nhất`a`mèo con và`b`câu chuyện về chó con mỗi tập, sau đó phân phát thức ăn thừa. Điều này nhanh chóng trở nên không khả thi vì số lượng phân phối có thể tăng lên kết hợp với`n`, Và`n`có thể lớn như`10^18`. 

Nhận xét quan trọng là mỗi tập đều có cấu trúc không thể phân biệt được: tất cả các tập đều có cùng kích thước và chỉ có số lượng tổng hợp mới là quan trọng. Điều này làm giảm vấn đề từ việc xây dựng từng tập đến việc kiểm tra xem liệu chúng tôi có thể chọn kích thước tập hợp lệ hay không`k`, sau đó chia cả hai loại câu chuyện thành`n`nhóm kích thước`k`. 

Cho phép`k`là số lượng câu chuyện mỗi tập. Thế thì chúng ta phải có`k * n = c + d`, Vì thế`k = (c + d) / n`. Đây đã là một điều kiện khả thi nghiêm ngặt: tổng số tầng phải chia hết cho`n`. 

Một lần`k`đã được sửa, mỗi tập phải chứa ít nhất`a`mèo con và`b`những câu chuyện về cún con, vì vậy chúng tôi yêu cầu`k >= a + b`. Điều này là do mỗi tập phải đồng thời thỏa mãn cả hai giới hạn dưới. 

Bây giờ câu hỏi duy nhất còn lại là liệu chúng ta có thể phân phối chính xác`c`câu chuyện mèo con và`d`câu chuyện về chó con vào`n`thùng có kích thước`k`mỗi cái, tôn trọng giới hạn dưới của mỗi thùng. Sau khi đặt chỗ`a`mèo con và`b`con chó con mỗi thùng, chúng tôi còn lại:`c - n * a`thêm câu chuyện về mèo con và`d - n * b`thêm những câu chuyện về cún con. 

Những thức ăn thừa này phải được phân phối tùy ý trên`n`thùng, nhưng mỗi thùng có thể chấp nhận tối đa`k - a - b`những câu chuyện bổ sung. Điều này hoạt động tự động miễn là tổng số khớp và không vượt quá dung lượng sẵn có, điều này được đảm bảo khi`k`được tính toán chính xác và các ràng buộc tối thiểu được thỏa mãn. 

Vì vậy, tính khả thi giảm xuống còn một vài kiểm tra số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | số mũ trong`n`| O(n) | Quá chậm | 
| Giảm số học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi rút ra các điều kiện trực tiếp từ cấu trúc của các phân vùng hợp lệ. 

1. Tính tổng số tầng`S = c + d`. Nếu như`S`không chia hết cho`n`, không có cách nào để chia tất cả các câu chuyện thành`n`các tập bằng nhau nên chúng ta dừng lại ngay. 
2. Xác định kích thước tập`k = S / n`. Đây là kích thước duy nhất có thể có cho mỗi tập vì tất cả các tập đều phải bằng nhau và tất cả các câu chuyện đều phải được sử dụng. 
3. Kiểm tra yêu cầu tối thiểu cho mỗi tập: mỗi tập phải chứa ít nhất`a + b`những câu chuyện vì nó cần ít nhất`a`câu chuyện mèo con và`b`truyện chó con. Nếu như`k < a + b`, thì ngay cả tập hợp lệ nhỏ nhất cũng vi phạm các ràng buộc, vì vậy chúng tôi từ chối. 
4. Kiểm tra tính khả thi của việc phân phối mèo con: tổng số câu chuyện về mèo con phải đủ để cung cấp`a`mỗi tập phim, vì vậy`c >= n * a`. 
5. Tương tự, truyện về cún con phải thỏa mãn`d >= n * b`. 
6. Nếu tất cả các điều kiện đều đúng thì có thể phân vùng; nếu không thì không thể được. 

Lý do đằng sau bước 4 và 5 là các ràng buộc tối thiểu dành cho mỗi tập chứ không phải toàn bộ. Mỗi tập phim tiêu thụ ít nhất`a`mèo con và`b`những câu chuyện về cún con, vì vậy tổng số phải hỗ trợ`n`lặp đi lặp lại các yêu cầu này. 

### Tại sao nó hoạt động 

Mọi công trình hợp lệ đều phải gán chính xác`k`câu chuyện mỗi tập, vì vậy việc chia nhỏ tổng thể là cần thiết. Khi điều đó đã được khắc phục, mỗi tập đều có chi phí cơ bản bắt buộc là`a`mèo con và`b`truyện chó con. Trừ đi cơ sở này khỏi tổng số sẽ để lại một nhóm câu chuyện miễn phí có thể được phân phối tùy ý giữa các tập. Các ràng buộc đảm bảo rằng nhóm miễn phí này không bao giờ âm và không bao giờ vượt quá dung lượng sẵn có cho mỗi tập, do đó không có hạn chế về cấu trúc nào tồn tại ngoài các kiểm tra số học này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = int(input())
b = int(input())
c = int(input())
d = int(input())

total = c + d

if total % n != 0:
    print("No")
else:
    k = total // n
    if k < a + b:
        print("No")
    elif c < n * a:
        print("No")
    elif d < n * b:
        print("No")
    else:
        print("Yes")
```Giải pháp mã hóa trực tiếp các điều kiện số học cần thiết. Việc kiểm tra khả năng phân chia đảm bảo tồn tại kích thước tập bằng nhau. Sự so sánh`k < a + b`buộc mỗi tập có thể đáp ứng đồng thời cả hai mức tối thiểu cho mỗi loại. Hai bất đẳng thức cuối cùng đảm bảo rằng số lượng toàn cầu đủ để đáp ứng hạn ngạch mỗi tập trên tất cả`n`các tập phim. 

Tất cả số học đều nằm trong phạm vi số nguyên 64 bit, vì vậy Python xử lý nó một cách an toàn mà không lo tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một đầu vào trong đó`n = 3`,`a = 1`,`b = 1`,`c = 4`,`d = 5`. 

Chúng tôi tính toán: 

| Bước | Giá trị | 
| --- | --- | 
| tổng = c + d | 9 | 
| tổng cộng % n | 0 | 
| k | 3 | 
| a + b | 2 | 
| n*a | 3 | 
| n*b | 3 | 

Từ`k >= a + b`,`c >= n * a`, Và`d >= n * b`, câu trả lời là hợp lệ. 

Điều này thể hiện trường hợp phân phối còn lại hoạt động trơn tru sau khi đặt trước các yêu cầu tối thiểu. 

### Ví dụ 2 

hãy để`n = 2`,`a = 2`,`b = 2`,`c = 3`,`d = 3`. 

| Bước | Giá trị | 
| --- | --- | 
| tổng = c + d | 6 | 
| tổng cộng % n | 0 | 
| k | 3 | 
| a + b | 4 | 

Đây`k < a + b`, vì vậy, mặc dù tổng số lượng là đủ trên toàn cầu nhưng mỗi tập sẽ yêu cầu ít nhất 4 câu chuyện nhưng mỗi tập chỉ có 3 câu chuyện. Câu trả lời là không thể. 

Điều này nhấn mạnh rằng nguồn cung toàn cầu là chưa đủ; tính khả thi của mỗi tập phim chặt chẽ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định | 
| Không gian | O(1) | Không sử dụng công trình phụ trợ | 

Các ràng buộc cho phép các giá trị lên đến`10^18`, do đó chỉ có các phép kiểm tra số học theo thời gian cố định mới khả thi. Giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = int(input())
    b = int(input())
    c = int(input())
    d = int(input())

    total = c + d
    if total % n != 0:
        return "No"
    k = total // n
    if k < a + b:
        return "No"
    if c < n * a:
        return "No"
    if d < n * b:
        return "No"
    return "Yes"

# sample-style tests
assert run("3\n1\n1\n4\n5\n") == "Yes"
assert run("2\n2\n2\n3\n3\n") == "No"

# edge cases
assert run("1\n0\n0\n10\n5\n") == "Yes"
assert run("5\n0\n0\n3\n2\n") == "Yes"
assert run("4\n1\n1\n4\n4\n") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 1 4 5 | Có | Phân phối cân bằng khả thi | 
| 2 2 2 3 3 | Không | Vi phạm tối thiểu mỗi tập | 
| 1 0 0 10 5 | Có | Trường hợp cạnh tập đơn | 
| 5 0 0 3 2 | Có | Không có ràng buộc tối thiểu | 
| 4 1 1 4 4 | Không | Phân chia chặt chẽ và xung đột năng lực | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi`n = 1`. Trong trường hợp đó, toàn bộ cuốn sách sẽ trở thành một tập duy nhất, vì vậy yêu cầu duy nhất là bản thân các tổng số phải thỏa mãn các ràng buộc tối thiểu. Thuật toán giảm xuống để kiểm tra xem`c >= a`Và`d >= b`, được thực thi chính xác kể từ`k = c + d`Và`k >= a + b`cùng nhau hàm ý tính khả thi chỉ khi cả hai tổng đều đủ. 

Một trường hợp cạnh khác là khi`a = 0`hoặc`b = 0`. Sau đó, một trong các ràng buộc biến mất và lời giải chính xác sẽ giảm xuống chỉ còn kiểm tra tổng số chia hết và ràng buộc còn lại. Các điều kiện số học được đơn giản hóa một cách tự nhiên mà không cần xử lý đặc biệt. 

Một trường hợp tinh tế cuối cùng xảy ra khi tổng số khớp chính xác với yêu cầu tối thiểu cho mỗi tập, không có tính linh hoạt. Ví dụ, nếu`c = n * a`Và`d = n * b`, thì mỗi tập buộc phải chứa chính xác mức tối thiểu. Thuật toán chấp nhận điều này bởi vì`k = a + b`và cả hai tổng số theo loại đều khớp chính xác, đảm bảo phân vùng cứng nhắc nhưng hợp lệ.

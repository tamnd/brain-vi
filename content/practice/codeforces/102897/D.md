---
title: "CF 102897D - Bài toán khó về Palindrome"
description: "Chúng ta bắt đầu với một tập hợp các chuỗi $n$, tất cả đều có cùng độ dài $m$ và chỉ có các chữ cái viết thường. Các thao tác được phép cho phép chúng ta liên tục giảm số lượng chuỗi bằng cách hợp nhất hai chuỗi theo một số cách khác nhau hoặc loại bỏ hoàn toàn một chuỗi."
date: "2026-07-04T08:47:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102897
codeforces_index: "D"
codeforces_contest_name: "The 3rd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102897
solve_time_s: 46
verified: true
draft: false
---

[CF 102897D - Vấn đề khó khăn về Palindrome](https://codeforces.com/problemset/problem/102897/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một bộ sưu tập$n$các chuỗi, tất cả đều có cùng độ dài$m$, và chỉ các chữ cái viết thường. Các thao tác được phép cho phép chúng ta liên tục giảm số lượng chuỗi bằng cách hợp nhất hai chuỗi theo một số cách khác nhau hoặc loại bỏ hoàn toàn một chuỗi. Việc hợp nhất có thể được thực hiện bằng cách nối một chuỗi với mặt trước hoặc mặt sau của chuỗi khác hoặc bằng cách xen kẽ hai chuỗi có độ dài bằng nhau theo từng ký tự để tạo thành một chuỗi có độ dài mới$2m$. 

Sau khi thực hiện bất kỳ chuỗi thao tác nào, chúng ta sẽ có một chuỗi cuối cùng. Từ chuỗi cuối cùng này, chúng ta được phép chia nó thành nhiều chuỗi con liền kề nhau, mỗi chuỗi con phải là một palindrome. Mục tiêu là chọn các phép toán và phép chia sao cho số phần palindromic trong phân vùng cuối cùng này càng lớn càng tốt. 

Điểm mấu chốt là chúng ta không được yêu cầu xây dựng phân vùng một cách rõ ràng mà chỉ xác định số lượng tối đa các phân đoạn đối xứng có thể đạt được sau khi hợp nhất tối ưu. 

Những ràng buộc ngụ ý rằng$n$có thể lớn như$10^5$và tổng kích thước đầu vào$n \cdot m$nhiều nhất là$10^6$. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào liên quan đến so sánh từng cặp chuỗi hoặc lập trình động trên tất cả các phép nối sẽ quá chậm. Việc đọc đầu vào tuyến tính hoặc gần tuyến tính là tất cả những gì có thể thực hiện được trên thực tế. 

Một vấn đề tế nhị đáng kiểm tra là liệu cấu trúc của các thao tác hợp nhất có hạn chế những chuỗi cuối cùng có thể có hay không. Cụ thể, việc xen kẽ có thể trông giống như áp đặt các ràng buộc vì nó buộc các ký tự xen kẽ từ hai chuỗi. Một cách giải thích bất cẩn có thể gợi ý rằng chuỗi cuối cùng không phải là tùy ý, điều này sẽ dẫn đến lý luận quá phức tạp về tổ hợp của việc xây dựng chuỗi. 

Ví dụ, người ta có thể nghĩ rằng việc xen kẽ tạo ra các mẫu chẵn lẻ giới hạn số lượng palindrome có thể được hình thành, nhưng điều này là sai lầm vì việc xen kẽ là tùy chọn và không bắt buộc để đạt được sự tối ưu. 

Một cạm bẫy tiềm ẩn khác là giả định rằng cấu trúc chuỗi cuối cùng quan trọng theo cách phức tạp đối với việc phân vùng palindrome. Vì mỗi ký tự riêng lẻ đều là một palindrome nên ngay cả những chuỗi có cấu trúc cao cũng luôn có thể được chia thành số lượng tối đa các phần palindrome. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta xem xét tư duy bạo lực. Người ta có thể cố gắng mô phỏng tất cả các chuỗi hợp nhất và loại bỏ có thể có, tạo ra mọi chuỗi cuối cùng có thể truy cập được và đối với mỗi chuỗi tính toán phân vùng palindromic tối đa của nó. Điều này ngay lập tức không khả thi vì mỗi lần hợp nhất sẽ làm giảm số lượng chuỗi đi một, nhưng số lượng chuỗi hợp nhất có thể có là theo cấp số nhân trong$n$. Ngay cả trước khi xem xét việc xây dựng chuỗi, chỉ riêng hệ số phân nhánh đã tăng lên như$O(n!)$trong trường hợp xấu nhất. 

Ngay cả khi chúng ta bỏ qua thứ tự thao tác và chỉ tập trung vào chuỗi cuối cùng, chúng ta vẫn phải đối mặt với câu hỏi chuỗi nào có thể truy cập được. Hoạt động đan xen gợi ý một sự bùng nổ tổ hợp của các cách xây dựng có thể có, nhưng đây là một cá trích đỏ. Chúng tôi thực sự không cần phải liệt kê bất kỳ trong số họ. 

Quan sát quan trọng là các thao tác nối cho phép chúng ta xây dựng một chuỗi cuối cùng có độ dài chỉ đơn giản là tổng độ dài của tất cả các chuỗi mà chúng ta chọn không loại bỏ. Vì được phép loại bỏ nên về nguyên tắc chúng ta có thể chọn bất kỳ tập con chuỗi nào. Tuy nhiên, việc loại bỏ bất kỳ chuỗi nào cũng không có lợi ích gì, vì mục tiêu là tối đa hóa số lượng các đoạn đối xứng và việc tăng tổng chiều dài chỉ có thể hữu ích. 

Sau khi chúng tôi sửa bất kỳ chuỗi có độ dài cuối cùng nào$L$, số chuỗi con palindromic tối đa mà chúng ta có thể chia thành là$L$chính nó, bằng cách chia thành các ký tự đơn. Mỗi ký tự đơn lẻ đều là một bảng màu tầm thường, do đó không cần cấu trúc khối lớn hơn để đạt được mức tối ưu. 

Điều này loại bỏ tất cả sự phụ thuộc vào các hoạt động nội bộ. Số lượng duy nhất quan trọng là tổng số ký tự có sẵn trên tất cả các chuỗi. 

Do đó, bài toán rút gọn thành tổng tất cả các độ dài, tức là$n \cdot m$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force đối với các hoạt động và công trình | Hàm mũ | Cao | Quá chậm | 
| Giảm tối ưu tổng chiều dài |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$và$n$các chuỗi, nhưng không cố gắng xử lý cấu trúc của chúng ngoài việc đo độ dài của chúng. Sự sắp xếp ký tự bên trong không bao giờ ảnh hưởng đến câu trả lời cuối cùng. 
2. Đối với mỗi chuỗi, hãy cộng chiều dài của nó vào tổng chiều dài. Vì mọi chuỗi đều có độ dài$m$, điều này tương đương với việc tính toán$n \cdot m$, nhưng tính tổng trực tiếp tránh giả định tính đồng nhất. 
3. Ghi tổng chiều dài dưới dạng câu trả lời. 

Lý do dừng ở đây là vì mọi ký tự trong chuỗi cuối cùng có thể được tách thành chuỗi con của chính nó và mỗi chuỗi con như vậy là một bảng màu. Không có phân vùng thay thế nào có thể vượt quá số lượng ký tự, vì mỗi ký tự phải thuộc về chính xác một phân đoạn. 

### Tại sao nó hoạt động 

Các thao tác chỉ thay đổi cách hợp nhất các chuỗi chứ không thay đổi thực tế là các ký tự được giữ nguyên. Không có thao tác nào xóa các ký tự ngoại trừ việc loại bỏ rõ ràng, điều này sẽ chỉ làm giảm tổng độ dài có sẵn. Do đó, chiến lược tối ưu là không loại bỏ gì và tối đa hóa tổng số ký tự trong chuỗi cuối cùng. 

Khi chuỗi cuối cùng được cố định, phân vùng palindromic tốt nhất có thể đạt được bằng cách chia thành các chuỗi con một ký tự. Điều này xác định rằng câu trả lời chính xác là tổng số ký tự có sẵn sau khi chọn tất cả các chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    total = 0
    for _ in range(n):
        s = input().strip()
        total += len(s)
    print(total)

if __name__ == "__main__":
    solve()
```Việc thực hiện chỉ đơn giản là tích lũy độ dài. Việc loại bỏ các dòng đầu vào là đủ vì các chuỗi chỉ chứa các chữ cái viết thường và không có khoảng trắng. Không có điều kiện biên nào ngoài việc đảm bảo rằng các dòng trống không bị đọc sai. 

Chi tiết triển khai quan trọng là tránh mọi nỗ lực mô phỏng các hoạt động hợp nhất. Bất kỳ nỗ lực nào như vậy sẽ gây ra sự phức tạp không cần thiết mà không làm thay đổi kết quả. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó đầu vào là:```
3
a
bb
ccc
```Chúng tôi tính toán độ dài từng bước. 

| Bước | Chuỗi | Chiều dài | Tổng số chạy | 
| --- | --- | --- | --- | 
| 1 | "một" | 1 | 1 | 
| 2 | "bb" | 2 | 3 | 
| 3 | "ccc" | 3 | 6 | 

Câu trả lời cuối cùng là 6. Điều này tương ứng với việc tạo thành một chuỗi có độ dài 6, có thể chia thành sáu bảng màu một ký tự. 

Điều này chứng tỏ rằng không có thuộc tính cấu trúc nào của chuỗi đầu vào quan trọng hơn tổng chiều dài của chúng. 

Bây giờ hãy xem xét trường hợp thứ hai:```
2
ab
cd
```| Bước | Chuỗi | Chiều dài | Tổng số chạy | 
| --- | --- | --- | --- | 
| 1 | "ab" | 2 | 2 | 
| 2 | "cd" | 2 | 4 | 

Câu trả lời là 4. Mặc dù không có chuỗi riêng lẻ nào là palindrome và các phép nối có thể tạo ra các cấu trúc không palindromic, việc chia thành các ký tự đơn luôn đạt được số lượng tối đa. 

Điều này xác nhận rằng giải pháp không phụ thuộc vào bất kỳ logic tìm kiếm palindrome nào trong giai đoạn xây dựng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chuỗi được đọc một lần và độ dài của nó được cộng vào tổng số | 
| Không gian |$O(1)$| Chỉ có một bộ tích lũy duy nhất được lưu trữ | 

Các ràng buộc cho phép lên đến$10^6$tổng số ký tự, do đó, một đường chuyền tuyến tính dễ dàng nằm trong giới hạn. Không cần cấu trúc dữ liệu bổ sung. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# simple samples
assert run("3\na\nbb\nccc\n") == "6"
assert run("2\nab\ncd\n") == "4"

# minimum case
assert run("1\na\n") == "1"

# uniform length case
assert run("4\naa\naa\naa\naa\n") == "8"

# mixed pattern
assert run("3\nabc\ndef\nghi\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 một | 1 | ranh giới tối thiểu | 
| 3 a bb ccc | 6 | tích lũy cơ bản | 
| 2 ab cd | 4 | chuỗi không phải palindrome | 
| 4 aa aa aa | 8 | cấu trúc thống nhất | 
| 3 abc ghi rõ ràng | 9 | trường hợp chung | 

## Vỏ cạnh 

Một mối lo ngại tiềm tàng là liệu việc loại bỏ có thể cải thiện câu trả lời hay không bằng cách kích hoạt một cấu trúc khác mang lại nhiều phân đoạn nhạt màu hơn cho mỗi ký tự. Tuy nhiên, điều này không thể xảy ra vì số lượng tối đa các đoạn đối xứng cho bất kỳ chuỗi có độ dài nào$L$chính xác là$L$, đạt được bằng cách phân vùng một ký tự. 

Ví dụ: với đầu vào:```
2
ab
cd
```Nếu chúng tôi loại bỏ một chuỗi, chúng tôi sẽ giảm tổng độ dài và do đó giảm số lượng phân đoạn palindromic tối đa có thể. Giữ cả hai mang lại độ dài 4 và phân vùng tốt nhất là bốn bảng màu ký tự đơn. 

Một mối quan tâm khác là liệu việc xen kẽ có thể tạo ra một cấu trúc bằng cách nào đó làm tăng số lượng các đoạn đối xứng vượt quá chiều dài hay không. Điều đó sẽ yêu cầu một phân đoạn không chứa ký tự nào hoặc các phân vùng chồng chéo, điều này không được phép. Mỗi phân vùng tiêu thụ ít nhất một ký tự, vì vậy giới hạn trên hoàn toàn là tổng chiều dài. 

Do đó, ngay cả khi sử dụng tính năng xen kẽ, nó không thể cải thiện mục tiêu ngoài việc chỉ bảo tồn tất cả các ký tự và xử lý từng ký tự như một bảng màu của chính nó.

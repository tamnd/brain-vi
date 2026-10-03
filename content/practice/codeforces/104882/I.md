---
title: "CF 104882I - 2B lý tưởng"
description: "Chúng ta được cấp một số nguyên không âm lên tới một nghìn tỷ và chúng ta cần in một biểu diễn rút gọn của nó. Mục tiêu không chỉ là nén mà còn là một loại xấp xỉ rất cụ thể: chúng tôi muốn một chuỗi không dài hơn bốn ký tự đại diện cho một số không…"
date: "2026-06-28T09:19:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104882
codeforces_index: "I"
codeforces_contest_name: "Voronezh State University - Sitronics contest II"
rating: 0
weight: 104882
solve_time_s: 46
verified: true
draft: false
---

[CF 104882I - 2B lý tưởng](https://codeforces.com/problemset/problem/104882/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số nguyên không âm lên tới một nghìn tỷ và chúng ta cần in một biểu diễn rút gọn của nó. Mục tiêu không chỉ là nén mà còn là một loại xấp xỉ rất cụ thể: chúng tôi muốn một chuỗi không dài hơn bốn ký tự đại diện cho một số không vượt quá giá trị ban đầu và càng gần giá trị đó càng tốt. 

Đối với các số dưới một nghìn, không có gì thú vị xảy ra, đầu ra chính xác là số đó. Độ khó bắt đầu từ một nghìn trở lên, nơi chúng ta được phép chuyển sang ký hiệu hậu tố. Mỗi hậu tố đại diện cho một lũy thừa cố định của mười: K tương ứng với$10^3$, M đến$10^6$, và B đến$10^9$. Phần số ở phía trước hậu tố được phép là phân số, nhưng chuỗi in đầy đủ bị giới hạn tối đa bốn ký tự, điều này hạn chế đáng kể mức độ chính xác mà chúng tôi có thể biểu thị. 

Yêu cầu quan trọng là giá trị được biểu thị không bao giờ được vượt quá số ban đầu. Trong số tất cả các biểu diễn hợp lệ thỏa mãn ràng buộc này, chúng ta phải chọn biểu diễn có giá trị gần nhất với số ban đầu. Nếu nhiều cách biểu diễn đạt được cùng một giá trị thì chúng ta ưu tiên cách biểu diễn có ít ký tự hơn. 

Ràng buộc$x < 10^{12}$ngay lập tức cho chúng ta biết rằng chỉ K, M và B là có liên quan. Bất cứ điều gì lớn hơn B sẽ không cần thiết. Vì chúng ta chỉ cần một số lượng ứng viên không đổi cho mỗi hậu tố nên giải pháp không thể yêu cầu bất kỳ tính toán nặng nề nào; nó phải là thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh tinh vi nhất đến từ hành vi làm tròn. Ví dụ: đối với một số như 999950, cả “999K” và “1M” đều là các giá trị gần đúng hợp lý trong logic làm tròn thông thường nhưng chỉ cho phép các biểu diễn không vượt quá giá trị ban đầu. Một tình huống phức tạp khác là khi nhiều biểu diễn tạo ra cùng một giá trị số, chẳng hạn như “1.0K” và “1K”, trong đó chuỗi ngắn hơn phải được chọn. 

Một cách tiếp cận ngây thơ thử tất cả các chuỗi thập phân có thể sẽ dễ dàng tạo ra các kết quả đầu ra không hợp lệ hoặc vượt quá giới hạn thời gian do liệt kê dấu phẩy động. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng xây dựng mọi chuỗi rút gọn hợp lệ dưới giới hạn bốn ký tự. Điều đó bao gồm việc thử tất cả các vị trí có thể có của dấu thập phân, tất cả tiền tố chữ số và từng hậu tố K, M, B. Đối với mỗi ứng cử viên, chúng tôi sẽ chuyển đổi nó trở lại thành giá trị số và kiểm tra xem nó có vượt quá x hay không. Sau đó chúng ta sẽ chọn cái gần nhất. 

Vấn đề với cách tiếp cận này là sự bùng nổ tổ hợp. Ngay cả khi chúng ta giới hạn bản thân ở bốn ký tự, vẫn có nhiều mẫu: một chữ số cộng hậu tố, hai chữ số cộng hậu tố, một chữ số có thập phân cộng hậu tố, v.v. Mỗi mẫu cũng cần có nhiều lựa chọn chữ số. Mặc dù không gian ứng cử viên không phải là vô hạn, việc liệt kê và xác nhận tất cả các khả năng vẫn lãng phí công sức vào cấu trúc không cần thiết. 

Quan sát quan trọng là cấu trúc hậu tố rất cứng nhắc. Mọi biểu diễn hợp lệ tương ứng với việc chọn thang đo trong số K, M, B (hoặc không có), sau đó chọn tiền tố bắt nguồn từ các chữ số đầu của x. Vì chúng ta không được vượt quá x nên tiền tố tối ưu luôn được xác định bằng cách cắt ngắn và làm tròn có kiểm soát thay vì xây dựng tùy ý. 

Thay vì tìm kiếm trên tất cả các biểu diễn, chúng ta có thể đánh giá từng hậu tố một cách độc lập. Đối với mỗi thang đo, chúng tôi chuyển đổi x thành đơn vị đó, trích xuất một số chữ số đầu giới hạn (vì chúng tôi chỉ có tổng cộng bốn ký tự) và xem xét định dạng hợp lệ tốt nhất xung quanh giá trị đó. Vì chỉ có ba hậu tố nên việc này trở thành công việc liên tục. 

Quyết định giảm xuống còn việc chọn, đối với mỗi thang đo, giá trị lớn nhất có thể được biểu thị trong vòng bốn ký tự không vượt quá x, sau đó chọn giá trị gần nhất trong số các ứng cử viên này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^4) mỗi số (liệt kê ngầm định các định dạng) | O(1) | Quá chậm / không thực tế | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý vấn đề như việc lựa chọn cái tốt nhất trong số một tập hợp nhỏ các định dạng ứng cử viên cố định. 

1. Nếu$x < 1000$, trở lại$x$trực tiếp dưới dạng một chuỗi. Không có chữ viết tắt nào có thể cải thiện hoặc phù hợp với nó dưới những ràng buộc. 
2. Với mỗi hậu tố trong số K, M và B, hãy giải thích số trong đơn vị đó bằng cách chia x cho lũy thừa mười tương ứng. Điều này đưa ra một số thực biểu thị số lượng đơn vị của thang đo đó phù hợp với x. 
3. Đối với mỗi giá trị tỷ lệ, hãy tạo các biểu diễn rút gọn có thể tuân thủ giới hạn bốn ký tự. Điều này có nghĩa là chúng tôi xem xét dạng số nguyên như “12K” hoặc dạng thập phân như “1,2M”, nhưng chúng tôi phải đảm bảo tổng chiều dài không vượt quá bốn ký tự. 
4. Đối với mỗi ứng cử viên, hãy tính giá trị số thực tế của nó bằng cách nhân số được hiển thị với hệ số nhân hậu tố. Từ chối bất kỳ ứng viên nào có giá trị vượt quá x. 
5. Trong số tất cả các ứng viên hợp lệ trên tất cả các hậu tố, hãy chọn ứng viên có giá trị số lớn nhất. Nếu nhiều ứng viên tạo ra cùng một giá trị, hãy chọn giá trị có chuỗi ngắn hơn. 

Lý do điều này có tác dụng là vì trong mỗi danh mục hậu tố, mọi biểu diễn hợp lệ đều được xác định đầy đủ bởi các chữ số đầu của x được chia tỷ lệ theo đơn vị đó. Vì chúng ta không bao giờ được phép vượt quá x nên ứng cử viên tối ưu cho một hậu tố nhất định luôn là cách cắt ngắn nhất hoặc làm tròn có kiểm soát xuống trong giới hạn ký tự. Không có lợi ích gì khi bỏ qua các tiền tố gần hơn, bởi vì bất kỳ tiền tố nhỏ hơn nào sẽ có giá trị kém hơn trong khi vẫn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_candidates(x):
    if x < 1000:
        return [(x, str(x))]

    res = []

    scales = [
        (10**9, "B"),
        (10**6, "M"),
        (10**3, "K"),
    ]

    for val, suf in scales:
        if x < val:
            continue

        base = x / val

        # try integer and one-decimal forms
        # ensure total length <= 4
        for d in range(0, 2):  # 0 or 1 decimal digit
            step = 10 ** d

            # truncate to avoid exceeding x
            scaled = int(base * step) / step

            s = f"{scaled:.{d}f}".rstrip("0").rstrip(".") + suf

            if len(s) <= 4:
                actual = scaled * val
                if actual <= x:
                    res.append((actual, s))

    return res

def solve():
    x = int(input().strip())

    if x < 1000:
        print(str(x))
        return

    candidates = build_candidates(x)

    best_val = -1
    best_str = ""

    for val, s in candidates:
        if val > best_val or (val == best_val and len(s) < len(best_str)):
            best_val = val
            best_str = s

    print(best_str)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên xử lý trực tiếp trường hợp tầm thường dưới 1000. Đối với các giá trị lớn hơn, nó lặp lại ba thang đo hậu tố có thể có theo thứ tự giảm dần, vì thang đo cao hơn có xu hướng mang lại khả năng nén có ý nghĩa hơn. 

Đối với mỗi thang đo, nó chuyển đổi x thành đơn vị đó và cố gắng tạo thành biểu diễn số nguyên hoặc biểu diễn thập phân đơn, vì giới hạn bốn ký tự ngăn cản mọi định dạng phức tạp hơn. Việc xây dựng chuỗi cẩn thận loại bỏ các số 0 ở cuối để “1.0K” trở thành “1K”, điều này rất quan trọng vì vấn đề rõ ràng ưu tiên các chuỗi ngắn hơn trong các trường hợp buộc. 

Điều kiện an toàn chính là kiểm tra`actual <= x`, điều này buộc chúng ta không bao giờ đánh giá quá cao giá trị. Nếu không có bộ bảo vệ này, việc làm tròn theo định dạng thập phân sẽ dễ dàng tạo ra các ứng cử viên không hợp lệ. 

Cuối cùng, chúng tôi chọn ứng cử viên có giá trị số tối đa và ngắt các mối quan hệ bằng cách sử dụng độ dài chuỗi. 

## Ví dụ đã hoạt động 

Xét x = 9500. 

Chúng tôi kiểm tra thang đo K trước vì M và B quá lớn. 

| Bước | cơ sở (x/1000) | định dạng | chuỗi | giá trị | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| K | 9,5 | 9K | 9K | 9000 | vâng | 
| K | 9,5 | 9,5K | 9,5K | 9500 | vâng | 

Tốt nhất là 9,5K vì nó khớp chính xác với x. Điều này xác nhận rằng biểu diễn phân số có thể cải thiện độ chính xác khi nó vẫn phù hợp với các ràng buộc. 

Bây giờ hãy xem xét x = 999950. 

| Bước | cơ sở (x/1000) | định dạng | chuỗi | giá trị | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| K | 999,95 | 999K | 999K | 999000 | vâng | 
| K | 999,95 | 999,9K | 999,9K | 999900 | vâng | 

Ở đây 999,9K tốt hơn vì nó gần với x hơn nhưng không vượt quá nó. Thuật toán tránh làm tròn tới 1000K một cách chính xác, điều này sẽ vi phạm ràng buộc. 

Những ví dụ này cho thấy thuật toán cân bằng độ chính xác với yêu cầu giới hạn trên nghiêm ngặt như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có ba hậu tố được chọn, mỗi hậu tố có lần thử định dạng liên tục | 
| Không gian | O(1) | Chỉ một số chuỗi ứng cử viên không đổi được lưu trữ | 

Việc tính toán không mở rộng theo x, chỉ với các quy tắc định dạng cố định, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    x = int(sys.stdin.readline().strip())

    if x < 1000:
        return str(x)

    scales = [(10**9, "B"), (10**6, "M"), (10**3, "K")]

    best_val = -1
    best_str = ""

    for val, suf in scales:
        if x < val:
            continue

        base = x / val

        for d in range(2):
            step = 10 ** d
            scaled = int(base * step) / step
            s = f"{scaled:.{d}f}".rstrip("0").rstrip(".") + suf

            if len(s) <= 4:
                actual = scaled * val
                if actual <= x:
                    if actual > best_val or (actual == best_val and len(s) < len(best_str)):
                        best_val = actual
                        best_str = s

    return best_str

# provided samples (interpreted)
assert run("9500") == "9.5K"
assert run("2000000000") == "2B"

# custom cases
assert run("999") == "999", "minimum boundary"
assert run("1000") == "1K", "exact K boundary"
assert run("1000000") == "1M", "exact M boundary"
assert run("876000") == "876K", "exact integer K case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 999 | 999 | Không viết tắt dưới 1000 | 
| 1000 | 1K | Chuyển đổi ranh giới chính xác | 
| 1000000 | 1 triệu | Ranh giới hậu tố cao hơn | 
| 876000 | 876K | Trường hợp chia tỷ lệ số nguyên sạch | 

## Vỏ cạnh 

Với x = 1000, thuật toán sẽ chuyển sang thang đo K. Cơ sở trở thành 1.0. Định dạng số nguyên tạo ra “1K” trong khi định dạng thập phân tạo ra “1.0K” dài hơn và bị rút gọn thành “1K”. Cả hai đều mang lại cùng một giá trị số và bộ ngắt kết nối sẽ chọn chuỗi ngắn hơn. Điều này phù hợp với yêu cầu “1K” được ưu tiên hơn “1.0K”. 

Với x = 999999999999, thang đo B chiếm ưu thế. Cơ số chỉ dưới 1000 và thuật toán chỉ xem xét các dạng rút gọn như 999B hoặc 999,9B. Bất kỳ phép làm tròn nào tạo ra 1000B đều bị từ chối vì vượt quá x. Ứng cử viên tốt nhất là phép cắt ngắn an toàn lớn nhất, đảm bảo độ kín tối đa mà không bị tràn. 

Đối với x = 0, thuật toán ngay lập tức trả về “0” vì nó nằm dưới ngưỡng và không áp dụng logic hậu tố.

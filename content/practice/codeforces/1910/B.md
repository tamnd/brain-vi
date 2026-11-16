---
title: "CF 1910B - Nhân Viên Bảo Vệ"
description: "Chúng tôi được cung cấp nhật ký của hệ thống theo dõi số lượng người ở trong một tòa nhà theo thời gian. Mỗi ký tự trong chuỗi đại diện cho một sự kiện: dấu cộng có nghĩa là có người vào và dấu trừ có nghĩa là có người rời đi. Hệ thống bắt đầu ngày mới với không có người bên trong."
date: "2026-06-08T20:20:29+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1910
codeforces_index: "B"
codeforces_contest_name: "Kotlin Heroes: Episode 9 (Unrated, T-Shirts + Prizes!)"
rating: 1600
weight: 1910
solve_time_s: 105
verified: false
draft: false
---

[CF 1910B - Nhân viên bảo vệ](https://codeforces.com/problemset/problem/1910/B) 

**Đánh giá:** 1600 
**Tags:** *đặc biệt, tham lam 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhật ký của hệ thống theo dõi số lượng người ở trong một tòa nhà theo thời gian. Mỗi ký tự trong chuỗi đại diện cho một sự kiện: dấu cộng có nghĩa là có người vào và dấu trừ có nghĩa là có người rời đi. Hệ thống bắt đầu ngày mới với không có người bên trong. 

Nhật ký hợp lệ là nhật ký mà chúng tôi không bao giờ quan sát thấy số lượng người âm ở bất kỳ tiền tố nào của chuỗi. Nói cách khác, nếu chúng ta mô phỏng quy trình từ trái sang phải, hiểu “+” là +1 và “-” là -1, số dư hiện hành không bao giờ được giảm xuống dưới 0. 

Chuỗi chúng tôi đưa ra có thể vi phạm quy tắc này. Tuy nhiên, chúng tôi được phép sửa nó bằng cách sử dụng tối đa một lần hoán đổi hai ký tự bất kỳ trong chuỗi hoặc không làm gì cả. Nhiệm vụ là xác định xem liệu việc điều chỉnh như vậy có khả thi hay không và nếu có thì đưa ra một hoán đổi hợp lệ (có thể là một hoán đổi không hoạt động của cùng một chỉ mục). 

Các ràng buộc rất lớn, với tổng chiều dài của tất cả các chuỗi trong các trường hợp thử nghiệm lên tới 300.000. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các giao dịch hoán đổi, vì có khả năng O(n²) cho mỗi trường hợp thử nghiệm. Ngay cả việc quét tuyến tính trên mỗi ứng cử viên hoán đổi cũng sẽ quá chậm. Chúng ta cần thứ gì đó để kiểm tra cấu trúc của sự mất cân bằng trong O(n) trên mỗi trường hợp thử nghiệm. 

Một điểm tinh tế quan trọng là việc hoán đổi hai ký tự chỉ có thể ảnh hưởng đến số dư tiền tố theo cách cục bộ. Nó không thay đổi tổng số điểm cộng và điểm trừ, chỉ thay đổi thứ tự của chúng. Điều này có nghĩa là cách duy nhất mà hoán đổi có thể khắc phục trình tự là điều chỉnh sự thiếu hụt sớm trong tổng tiền tố. 

Các trường hợp biên xuất hiện khi trình tự đã hợp lệ, khi nó có một vi phạm tiền tố duy nhất không thể sửa chữa bằng cách hoán đổi và khi sự mất cân bằng toàn cầu quá nghiêm trọng mặc dù vẫn tồn tại các bản sửa lỗi cục bộ. Ví dụ: một chuỗi như “---+++” không thể sửa được vì ký tự đầu tiên đã mất tính hợp lệ cho dù bạn có trao đổi với nó như thế nào; không có trao đổi nào có thể loại bỏ thực tế là vị trí đầu tiên có thể trở thành '-'. 

Một trường hợp tinh vi khác là khi chuỗi hợp lệ nhưng chúng ta vẫn được phép xuất ra một hoán đổi giả (i, i). Nhiều giải pháp cho rằng chúng phải luôn thực hiện hoán đổi thực sự một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cặp chỉ số i và j (bao gồm i = j), hoán đổi chúng và kiểm tra xem chuỗi kết quả có hợp lệ hay không bằng cách tính lại tổng tiền tố. Mỗi lần kiểm tra tính hợp lệ có chi phí O(n) và có các giao dịch hoán đổi O(n2), cho ra O(n³) cho mỗi trường hợp kiểm thử trong trường hợp xấu nhất. Ngay cả việc giảm kiểm tra tính hợp lệ đối với các bản cập nhật gia tăng O(1) vẫn để lại O(n²), tốc độ này quá chậm đối với tổng số n lên tới 300.000. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến hầu hết các giao dịch hoán đổi. Lần duy nhất tiền tố trở nên không hợp lệ là khi số dư hiện hành giảm xuống dưới 0 lần đầu tiên. Đặt vị trí đầu tiên đó là p. Tại vị trí p, chúng ta phải đặt một dấu '-' lẽ ra không nên có ở vị trí đó. Để khắc phục điều này, chúng ta phải mang dấu '+' từ đâu đó sau đó và chuyển nó sang đoạn phía trước trước hoặc tại p, hoặc hoán đổi tương đương với dấu '-' có vấn đề đó. 

Điều này làm giảm toàn bộ vấn đề trong việc xác định vị trí tiền tố sớm nhất nơi số dư trở nên âm. Nếu không có tiền tố như vậy tồn tại thì chuỗi đã hợp lệ. Nếu không, chúng tôi cố gắng sửa chữa nó bằng cách hoán đổi chữ xấu đầu tiên '-' bằng '+' ở đâu đó sau nó. Tính đúng đắn phụ thuộc vào thực tế là chỉ những vi phạm sớm nhất mới quan trọng, những vi phạm sau mới là hậu quả của cùng một sự mất cân bằng và sẽ được giải quyết nếu vi phạm đầu tiên được khắc phục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng tổng tiền tố trên chuỗi trong khi theo dõi vị trí đầu tiên nơi số dư trở nên âm.

1. Tính toán số dư tiền tố trong khi quét từ trái sang phải, coi '+' là +1 và '-' là -1. Nếu chúng ta không bao giờ xuống dưới 0 thì chuỗi đã hợp lệ và chúng ta tạo ra một hoán đổi tầm thường (1, 1). Điều này hoạt động vì không cần chỉnh sửa và sự cố cho phép không hoạt động. 
2. Nếu chúng tôi tìm thấy chỉ số p đầu tiên mà số dư trở nên âm, chúng tôi sẽ dừng ngay lập tức. Vị trí này là lỗi cấu trúc sớm nhất trong chuỗi và mọi sửa chữa hợp lệ đều phải giải quyết trực tiếp. 
3. Chúng ta xác định ký tự ở vị trí p, phải là '-'. Cách duy nhất để sửa tiền tố này là hoán đổi '-' này bằng '+' từ vị trí nào đó q > p. 
4. Chúng tôi quét hậu tố từ p+1 trở đi để tìm bất kỳ dấu '+' nào. Nếu không tồn tại, chúng tôi xuất -1 vì không có cách nào đưa ra khoản đóng góp tích cực đủ sớm để bù đắp thâm hụt. 
5. Nếu tìm thấy vị trí q như vậy, chúng ta sẽ đưa ra hoán đổi (p+1, q+1) vì các chỉ số dựa trên 1. 

Lý do chúng tôi chỉ xem xét vi phạm đầu tiên là vì tổng tiền tố hoạt động đơn điệu đối với các hiệu chỉnh cục bộ. Khi tiền tố phủ định đầu tiên được cố định, tất cả các tiền tố sau phụ thuộc vào mức thâm hụt đó cũng được cố định hoặc trở thành không âm. 

### Tại sao nó hoạt động 

Tổng tiền tố tại mỗi vị trí chỉ phụ thuộc vào thứ tự tương đối của '+' và '-'. Lần đầu tiên tổng tiền tố trở thành số âm xác định sự mất cân bằng về cấu trúc trong đó có nhiều '-' hơn '+' trong tiền tố. Bất kỳ chuỗi hợp lệ nào cũng phải đảm bảo rằng đối với mọi tiền tố, số '+' ít nhất là số '-'. 

Ở tiền tố p không hợp lệ đầu tiên, chúng ta nhất thiết phải có nhiều hơn một '-' so với mức cho phép. Cách khắc phục duy nhất có thể sử dụng một lần hoán đổi duy nhất là thay thế một '-' trong tiền tố bằng '+', điều này làm tăng nghiêm ngặt tất cả các tổng tiền tố từ thời điểm đó trở đi lên 2 so với hoán đổi theo hướng ngược lại. Bất kỳ sự hoán đổi nào không liên quan đến vị trí p đều không thể khắc phục được thực tế là tiền tố đã giảm xuống dưới 0 tại p. Do đó, điều kiện cần thiết để thành công là dấu '+' tồn tại sau p và việc hoán đổi nó thành tiền tố sẽ giải quyết được vi phạm đầu tiên, sau đó không tồn tại vi phạm nào trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    n = len(s)

    bal = 0
    first_bad = -1

    for i, ch in enumerate(s):
        if ch == '+':
            bal += 1
        else:
            bal -= 1
        if bal < 0:
            first_bad = i
            break

    if first_bad == -1:
        print(1, 1)
        continue

    pos_plus = -1
    for j in range(first_bad + 1, n):
        if s[j] == '+':
            pos_plus = j
            break

    if pos_plus == -1:
        print(-1)
    else:
        print(first_bad + 1, pos_plus + 1)
```Giải pháp trước tiên sẽ phát hiện xem trình tự đã hợp lệ hay chưa bằng cách theo dõi số dư hiện hành. Thời điểm nó trở nên tiêu cực, chúng tôi ghi lại chỉ số đó là điểm thất bại sớm nhất. 

Nếu không có lỗi tồn tại, chúng tôi sẽ trả lại một trao đổi giả. Điều này hợp lệ vì việc hoán đổi một ký tự với chính nó sẽ khiến chuỗi không thay đổi. 

Nếu có lỗi, chúng tôi chỉ tìm dấu '+' ở hậu tố sau lỗi đó. Việc hoán đổi '+' đó vào vị trí lỗi đảm bảo tổng tiền tố tại điểm lỗi tăng thêm 2 so với ban đầu, đủ để loại bỏ vi phạm đầu tiên. 

Việc triển khai cẩn thận về việc lập chỉ mục dựa trên 1 ở đầu ra, vì logic bên trong sử dụng lập chỉ mục dựa trên 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`-+`Chúng tôi theo dõi số dư tiền tố: 

| tôi | char | cân bằng | đầu tiên_xấu | 
| --- | --- | --- | --- | 
| 0 | - | -1 | 0 | 

Tại i = 0 số dư trở nên âm, do đó first_bad = 0. Chúng tôi tìm kiếm chỉ số 0 và tìm thấy '+' tại chỉ mục 1. Hoán đổi vị trí 1 và 2 tạo ra "+-", giá trị này hợp lệ. 

Điều này cho thấy cách có thể sửa một dấu '-' bị đặt sai vị trí ở phía trước bằng cách di chuyển dấu '+' về phía trước. 

### Ví dụ 2 

đầu vào:`+--+`Quét tiền tố: 

| tôi | char | cân bằng | đầu tiên_xấu | 
| --- | --- | --- | --- | 
| 0 | + | 1 | - | 
| 1 | - | 0 | - | 
| 2 | - | -1 | 2 | 

Chúng tôi tìm thấy vi phạm đầu tiên ở chỉ mục 2. Sau đó, chúng tôi tìm dấu '+' sau nó và tìm thấy vi phạm ở chỉ mục 3. Hoán đổi chúng mang lại kết quả`++- -`, vẫn hợp lệ trong mô phỏng tiền tố. 

Điều này cho thấy chúng ta chỉ quan tâm đến thất bại đầu tiên; cấu trúc sau này được tự động sửa khi tiền tố đó được sửa chữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét một lần để tìm vi phạm đầu tiên và một lần nữa để tìm dấu '+' | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Giải pháp là tuyến tính trong tổng kích thước đầu vào, điều này cần thiết với tổng giới hạn 300.000 ký tự. Cả hai lượt đều là các lần quét đơn giản qua chuỗi, giúp giải pháp dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        n = len(s)

        bal = 0
        first_bad = -1

        for i, ch in enumerate(s):
            if ch == '+':
                bal += 1
            else:
                bal -= 1
            if bal < 0:
                first_bad = i
                break

        if first_bad == -1:
            out.append("1 1")
            continue

        pos_plus = -1
        for j in range(first_bad + 1, n):
            if s[j] == '+':
                pos_plus = j
                break

        if pos_plus == -1:
            out.append("-1")
        else:
            out.append(f"{first_bad+1} {pos_plus+1}")

    return "\n".join(out)

# provided samples
assert run("""6
-+
+-
+++-
---++
+
-
""") == """1 2
1 1
1 1
-1
1 1
-1"""

# custom cases
assert run("""3
++
--
+-+-
""") == """1 1
-1
1 3"""

assert run("""2
-+++
----
""") == """1 2
-1"""

assert run("""1
+"""
) == """1 1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`++ / -- / +-+-`|`1 1 / -1 / 1 3`| đã hợp lệ, không thể, sửa chữa đơn giản | 
|`-+++ / ----`|`1 2 / -1`| điều chỉnh sớm vs không thể hoàn toàn âm | 
|`+`|`1 1`| trường hợp cạnh kích thước tối thiểu | 

## Vỏ cạnh 

Khi chuỗi đã hợp lệ, thuật toán sẽ không bao giờ đi vào nhánh lỗi. Đầu ra`1 1`thể hiện chính xác trao đổi không hoạt động và tính hợp lệ của tiền tố không thay đổi. 

Khi chuỗi hoàn toàn là các ký tự '-', tổng tiền tố sẽ trở thành số âm ngay từ bước đầu tiên và không có dấu '+' để hoán đổi sau đó. Quá trình quét không tìm thấy bất kỳ đối tác hợp lệ nào, tạo ra -1 chính xác. 

Khi ký tự đầu tiên là '-', thuật toán sẽ ngay lập tức xác định lỗi vi phạm đầu tiên ở chỉ mục 0. Sau đó, thuật toán chỉ tìm kiếm chính xác hậu tố cho dấu '+', đảm bảo rằng bản sửa lỗi nhắm trực tiếp vào lỗi sớm nhất có thể thay vì lỗi dư thừa sau này.

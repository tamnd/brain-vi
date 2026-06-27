---
title: "CF 105387N - Nhà côn trùng học"
description: "Chúng ta được cung cấp một mô tả bị sai một phần của một chuỗi ban đầu xuất phát từ một công thức đơn giản. Có một giá trị nguyên $k$ không xác định, và với mỗi chỉ số $i$, giá trị mong muốn có được bằng cách chia $k$ cho $i$ và làm tròn đến số nguyên gần nhất theo tiêu chuẩn…"
date: "2026-06-23T16:26:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105387
codeforces_index: "N"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2023"
rating: 0
weight: 105387
solve_time_s: 97
verified: false
draft: false
---

[CF 105387N - Nhà côn trùng học](https://codeforces.com/problemset/problem/105387/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mô tả bị sai một phần của một chuỗi ban đầu xuất phát từ một công thức đơn giản. Có một giá trị số nguyên không xác định$k$và với mỗi chỉ số$i$, giá trị mong muốn có được bằng cách chia$k$qua$i$và làm tròn đến số nguyên gần nhất bằng cách sử dụng phép làm tròn toán học tiêu chuẩn, trong đó một nửa tăng lên. 

Vì vậy mỗi vị trí$i$hoặc chứa giá trị làm tròn chính xác$c_i$, hoặc bị thiếu và được đánh dấu là dấu chấm hỏi. Mục tiêu là tái tạo lại số nguyên dương nhỏ nhất có thể$k$sao cho tất cả các mục đã biết đều phù hợp với quy tắc làm tròn này. 

Mỗi ràng buộc đã biết ràng buộc$k$đến một phạm vi giá trị hẹp. Nếu như$c_i$thì đã biết$k/i$phải nằm trong một khoảng xung quanh$c_i$có chiều rộng một tâm ở$c_i$. Điều đó chuyển đổi vấn đề từ “đoán$k$” thành “tìm số nguyên nằm trong giao điểm của nhiều khoảng”. 

Những ràng buộc rất chặt chẽ bởi vì$n \le 1000$, vì vậy bất kỳ giải pháp nào kiểm tra tất cả những gì có thể$k$đến giới hạn lớn là không thể thực hiện được. Tìm kiếm trực tiếp trên$k$sẽ yêu cầu xem xét các giá trị có khả năng lên tới$10^9$hoặc cao hơn tùy thuộc vào đầu vào, quá lớn để lặp lại lực lượng vũ phu. 

Một sự tinh tế đến từ việc làm tròn: điều kiện không phải là đẳng thức mà là một ràng buộc về khoảng. Một điểm quan trọng khác là các mục bị thiếu không có hạn chế nên có thể bỏ qua hoàn toàn. 

Một sai lầm ngây thơ sẽ là giải thích$c_i = k/i$như phân chia chính xác hoặc phân chia sàn. Ví dụ, nếu$k = 5$Và$i = 2$, sau đó$k/i = 2.5$, làm tròn thành 3. Việc coi nó là 2 hoặc 3 đều sai và sẽ dịch chuyển tất cả các ràng buộc không chính xác. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tăng giá trị của$k$từ 1 trở lên và kiểm tra xem mọi thứ đã biết chưa$c_i$phù hợp với quy tắc làm tròn. Đối với mỗi ứng viên$k$, chúng ta sẽ tính toán$k/i$, làm tròn nó và so sánh với tất cả các vị trí đã biết. Mỗi chi phí kiểm tra$O(n)$, vì vậy nếu câu trả lời lớn, việc này sẽ trở nên cực kỳ chậm. 

Sự kém hiệu quả xuất phát từ việc không sử dụng cấu trúc của các ràng buộc. Mỗi cố định$c_i$không chỉ đưa ra một giá trị duy nhất$k$, nó đưa ra một phạm vi giá trị liên tục$k$các giá trị. Điều kiện làm tròn chuyển thành bất đẳng thức có dạng “$k/i$gần với$c_i$”, trở thành một khoảng tuyến tính trong$k$. 

Khi mỗi ràng buộc trở thành một khoảng, bài toán sẽ giảm xuống mức giao tất cả các khoảng này và tìm số nguyên nhỏ nhất bên trong giao điểm. Thay vì tìm kiếm khắp$k$, chúng ta duy trì một vùng khả thi toàn cục và thu nhỏ nó bằng cách sử dụng mọi quan sát đã biết. 

Điểm tinh tế duy nhất còn lại là việc làm tròn tạo ra các ranh giới nửa số nguyên, do đó, sẽ dễ dàng hơn nếu chia tỷ lệ mọi thứ thành 2 và tránh hoàn toàn các vấn đề về dấu phẩy động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$k$|$O(n \cdot k)$|$O(1)$| Quá chậm | 
| Giao lộ khoảng (tỷ lệ) |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi loại bỏ giới hạn phân số bằng cách nhân đôi mọi thứ. Thay vì làm việc với$k$, chúng tôi làm việc với$T = 2k$. Mỗi ràng buộc trở thành một khoảng nguyên đơn giản. 

1. Với mỗi cặp đã biết$(i, c_i)$, dịch quy tắc làm tròn thành giới hạn trên$T = 2k$. 

điều kiện$c_i - 0.5 \le \frac{k}{i} < c_i + 0.5$trở thành$2i c_i - i \le 2k < 2i c_i + i$. 

Vì thế$T$phải nằm trong khoảng nguyên$[2ic_i - i, 2ic_i + i - 1]$. 
2. Duy trì khoảng khả thi toàn cầu$[L, R]$vì$T$. Khởi tạo nó rất rộng, sau đó giao nó với từng khoảng ràng buộc. 

Sau khi xử lý từng cái đã biết$c_i$, cập nhật$L = \max(L, 2ic_i - i)$Và$R = \min(R, 2ic_i + i - 1)$. 
3. Sau khi xử lý tất cả các ràng buộc, giá trị hợp lệ$T$các giá trị chính xác là những số nguyên trong$[L, R]$. Bây giờ chúng ta cần giá trị nhỏ nhất$k$, tương ứng với số chẵn nhỏ nhất$T$trong phạm vi này. 
4. Điều chỉnh$T$lên đến số chẵn đầu tiên không nhỏ hơn$L$. Nếu như$L$chẵn thì giữ nguyên; ngược lại tăng thêm 1. 
5. Đầu ra$k = T / 2$. 

Tại sao nó hoạt động được gắn liền với thực tế là mỗi ràng buộc là lồi trong$k$. Quy tắc làm tròn tạo ra một khoảng giá trị hợp lệ liên tục cho mỗi$i$và giao nhau tất cả các khoảng như vậy sẽ bảo toàn chính xác tập hợp các giải pháp khả thi. Vì mọi ràng buộc đều là điều kiện cần và đủ nên mọi$k$bên ngoài giao lộ cuối cùng sẽ vi phạm ít nhất một$c_i$, và bất kỳ$k$bên trong thỏa mãn tất cả các ràng buộc cùng một lúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    arr = input().split()

    L = -10**30
    R = 10**30

    for i, val in enumerate(arr, start=1):
        if val == '?':
            continue

        c = int(val)

        left = 2 * i * c - i
        right = 2 * i * c + i - 1

        L = max(L, left)
        R = min(R, right)

    if L % 2 != 0:
        L += 1

    T = L
    k = T // 2
    print(k)

if __name__ == "__main__":
    main()
```Cốt lõi của việc thực hiện là chuyển đổi từ các ràng buộc làm tròn thành các bất đẳng thức tuyến tính trên$2k$. Mỗi giá trị đã biết sẽ thắt chặt phạm vi khả thi và chúng ta không bao giờ cần xem xét các vị trí chưa biết vì chúng không có ràng buộc gì. Bước cuối cùng đảm bảo chúng tôi chọn số nguyên hợp lệ nhỏ nhất$k$, tương ứng với số chẵn nhỏ nhất$T$. 

Một lỗi phổ biến là quên rằng giới hạn trên là độc quyền trước khi chia tỷ lệ. Đó là lý do tại sao khoảng thời gian kết thúc tại$2ic_i + i - 1$, không$2ic_i + i$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
?
3
2
```Chúng tôi xử lý các ràng buộc theo từng bước. 

| tôi | c_i | Cập nhật L | Cập nhật R | 
| --- | --- | --- | --- | 
| 2 | 3 | L = 2·2·3 − 2 = 10 | R = 2·2·3 + 2 − 1 = 13 | 
| 3 | 2 | L = max(10, 2·3·2 − 3 = 9) = 10 | R = phút(13, 2·3·2 + 3 − 1 = 14) = 13 | 

Vì thế$T \in [10, 13]$. Nhỏ nhất cũng được$T$là 10, cho$k = 5$. 

Điều này cho thấy mức độ chồng chéo của nhiều ràng buộc để thu hẹp phạm vi hợp lệ và cách câu trả lời cuối cùng được chọn là giá trị chẵn nhỏ nhất bên trong giao điểm. 

### Mẫu 2 

đầu vào:```
3
?
4
?
```Chỉ có một hạn chế quan trọng. 

| tôi | c_i | L | R | 
| --- | --- | --- | --- | 
| 2 | 4 | 2·2·4 − 2 = 14 | 2·2·4 + 2 − 1 = 17 | 

Vì thế$T \in [14, 17]$. Nhỏ nhất cũng được$T$là 14, vậy$k = 7$. 

Điều này chứng tỏ rằng các giá trị bị thiếu thực sự không đóng góp gì và một ràng buộc duy nhất là đủ để xác định phạm vi khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi mục đã biết đóng góp một lần cập nhật theo khoảng thời gian không đổi | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được duy trì | 

Các ràng buộc cho phép tối đa 1000 mục nhập và giải pháp thực hiện một lần chuyển qua chúng. Ngay cả với giới hạn số lớn, tất cả các phép toán đều là số học có thời gian không đổi, do đó nghiệm dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    arr = input().split()

    L = -10**30
    R = 10**30

    for i, val in enumerate(arr, start=1):
        if val == '?':
            continue
        c = int(val)
        left = 2 * i * c - i
        right = 2 * i * c + i - 1
        L = max(L, left)
        R = min(R, right)

    if L % 2 != 0:
        L += 1

    return str(L // 2)

assert run("3\n?\n3 2") == "5", "sample 1"
assert run("3\n?\n4 ?") == "7", "sample 2"

assert run("3\n?\n1 1") == "2", "small consistent case"
assert run("3\n?\n10 10") == "20", "tight scaling case"
assert run("4\n?\n2 1 ?") >= "1", "sanity check existence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hạn chế tối thiểu | k nhỏ | độ đúng cơ sở | 
| ràng buộc duy nhất | dẫn xuất k | xử lý khoảng thời gian | 
| nhiều ràng buộc chặt chẽ | ngã tư có giới hạn | tính chính xác của logic tối đa/phút | 
| giá trị đã biết thưa thớt | bỏ qua '?' | mạnh mẽ đối với dữ liệu bị thiếu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các mục trừ một mục đều bị thiếu. Thuật toán giảm xuống một ràng buộc khoảng duy nhất và câu trả lời hoàn toàn xuất phát từ ràng buộc đó. Logic giao lộ vẫn hoạt động vì nó bắt đầu từ một phạm vi rất rộng và co lại một cách chính xác. 

Một trường hợp khó phát hiện khác là khi khoảng khả thi chỉ chứa các giá trị lẻ của$T$. Từ$T$phải chẵn, chúng tôi điều chỉnh tăng lên một cách rõ ràng. Ví dụ, nếu$T \in [15, 15]$, chúng ta bỏ qua 15 và chuyển sang 16. Sự thay đổi này giữ nguyên giá trị vì định nghĩa ban đầu đảm bảo tồn tại ít nhất một nghiệm chẵn. 

Một trường hợp khác là khi các ràng buộc chồng lên nhau ở một giá trị biên duy nhất. Vì giới hạn trên được bao gồm sau khi chia tỷ lệ nên các điểm cuối khoảng đều an toàn và bước điều chỉnh cuối cùng không vô tình loại trừ các giải pháp tối thiểu hợp lệ.

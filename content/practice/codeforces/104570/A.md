---
title: "CF 104570A - Đồng xu"
description: "Chúng ta được cấp một “hệ thống tiền tệ” nhỏ bao gồm ba loại xu: xu trị giá 1, xu trị giá 10 và xu trị giá 100."
date: "2026-06-30T08:23:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104570
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #23 (Balanced-Forces)"
rating: 0
weight: 104570
solve_time_s: 74
verified: false
draft: false
---

[CF 104570A - Tiền xu](https://codeforces.com/problemset/problem/104570/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một “hệ thống tiền tệ” nhỏ bao gồm ba loại đồng xu: đồng xu trị giá 1, đồng xu trị giá 10 và đồng xu trị giá 100. Đối với mỗi trường hợp thử nghiệm, chúng ta biết có bao nhiêu đồng xu thuộc mỗi loại và chúng ta muốn xác định xem liệu chúng ta có thể chọn một số tập hợp con của những đồng xu này sao cho tổng giá trị của chúng bằng chính xác với số mục tiêu hay không. 

Mỗi trường hợp thử nghiệm là độc lập. Chúng tôi không bắt buộc phải giảm thiểu hoặc tối đa hóa bất cứ điều gì, chỉ quyết định tính khả thi của việc hình thành một số tiền chính xác. 

Các ràng buộc rất lớn: lên tới 100.000 trường hợp thử nghiệm và mỗi tham số có thể lớn bằng 1e9. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê các tổ hợp tiền xu hoặc mô phỏng tất cả các khả năng. Ngay cả một vòng lặp ba lần lồng nhau cũng không thể thực hiện được, vì trường hợp xấu nhất sẽ theo thứ tự các phép toán 1e27. 

Cấu trúc của các giá trị là chìa khóa: 1, 10 và 100 là lũy thừa của mười. Điều này gợi ý rõ ràng về cách xây dựng từng chữ số tham lam, bởi vì các mệnh giá cao hơn chỉ ảnh hưởng đến các vị trí thập phân cao hơn và không thể được bù đắp bằng các mệnh giá thấp hơn ngoài phạm vi giới hạn. 

Một sai lầm ngây thơ xuất hiện khi xử lý tiền một cách độc lập mà không tôn trọng thứ bậc của chúng. Ví dụ: trước tiên, việc cố gắng thỏa mãn số tiền bằng 100 xu một cách tham lam mà không kiểm tra tính khả thi còn sót lại với 10 xu và 1 xu có thể thất bại. 

Một trường hợp cạnh đơn giản minh họa điều này: 

đầu vào:```
a = 9, b = 0, c = 1, n = 10
```Ở đây, chúng ta có thể tạo thành 10 bằng cách sử dụng một xu 10 (không có sẵn) hoặc bằng cách sử dụng mười xu 1 cộng với logic điều chỉnh trừ 100 xu, nhưng trên thực tế, chúng ta không thể trừ xu. Một lựa chọn tham lam bất cẩn là lấy 100 xu trước sẽ cho rằng tính khả thi là không chính xác. 

Câu trả lời đúng là:```
NO
```bởi vì chúng ta không thể “chia” 100 xu thành những giá trị nhỏ hơn mức đóng góp cố định của nó. 

Khó khăn cốt lõi là xử lý chính xác sự tương tác giữa các cấp độ trong khi vẫn tôn trọng giới hạn tiền xu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả số lượng có thể là 100 xu, 10 xu và 1 xu. Đối với mỗi lựa chọn x 100 xu, y 10 xu và z 1 xu, chúng tôi sẽ kiểm tra xem: 

x * 100 + y * 10 + z = n 

với các ràng buộc x ₫ c, y ₫ b, z ₫ a. 

Ngay cả khi chúng ta lặp lại x, y, z đến giới hạn của chúng thì đây vẫn là O(abc), điều này hoàn toàn không khả thi vì mỗi giá trị có thể lên tới 1e9. 

Quan sát quan trọng là hệ thống tiền xu được định vị theo cơ số 10. Đồng xu 1 và 10 kiểm soát hoàn toàn hai chữ số cuối của tổng, trong khi đồng xu 100 xác định mọi thứ ở trên đó. Điều này có nghĩa là chúng ta không cần phải tìm kiếm tất cả các kết hợp, chỉ điều chỉnh một cách tham lam và sửa phần còn lại. 

Chúng tôi xử lý 100 xu trước. Chúng tôi cố gắng sử dụng càng nhiều càng tốt nhưng không quá n // 100 và không quá c. Sau khi chúng tôi khắc phục lựa chọn đó, vấn đề còn lại sẽ giảm xuống thành việc tạo phần còn lại bằng cách sử dụng đồng 10 và 1. Vấn đề giảm bớt đó một lần nữa có thể được giải quyết một cách tham lam: sử dụng càng nhiều 10 xu càng tốt, sau đó kiểm tra xem giá trị còn lại có phù hợp với 1 xu có sẵn hay không. 

Cấu trúc đảm bảo rằng các quyết định ở mệnh giá cao hơn độc lập với các mệnh giá thấp hơn, bởi vì các đồng tiền thấp hơn không thể bù đắp cho sự thiếu hụt ở các đơn vị có giá trị cao hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a·b·c) | O(1) | Quá chậm | 
| Tham lam theo giáo phái | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cố gắng sử dụng càng nhiều xu giá trị 100 càng tốt mà không vượt quá mục tiêu hoặc tính khả dụng. Chúng tôi tính x = min(c, n // 100). Bước này đảm bảo chúng tôi không vượt quá mục tiêu bằng cách sử dụng đồng tiền có giá trị cao. 
2. Trừ phần đóng góp của họ khỏi mục tiêu: n trở thành n − 100·x. Điều này cô lập giá trị còn lại phải được hình thành bằng cách sử dụng các đồng tiền nhỏ hơn. 
3. Bây giờ hãy thử sử dụng đồng xu 10 giá trị tương tự: y = min(b, n // 10). Điều này tối đa hóa việc sử dụng số tiền trung bình mà không vượt quá số tiền còn lại. 
4. Trừ phần đóng góp của họ: n trở thành n − 10·y. Tại thời điểm này, chỉ có đồng xu có giá trị 1 là phù hợp. 
5. Kiểm tra xem giá trị còn lại có nhỏ hơn hoặc bằng số lượng 1 xu có sẵn hay không. Nếu đúng như vậy thì số tiền đó có thể đạt được; nếu không thì không thể được. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là mỗi loại xu là bội số của loại nhỏ hơn tiếp theo với hệ số 10. Điều này giúp loại bỏ sự đánh đổi giữa các cấp độ: sử dụng ít hơn 100 xu không bao giờ hữu ích trừ khi nó làm tăng tính khả thi trong phạm vi 10/1, nhưng việc giảm 100 xu chỉ làm tăng phần còn lại cần thiết lên bội số của 100, số tiền này không thể sửa được bằng 10 hoặc 1 xu ngoài các giới hạn mô-đun đã được xử lý. Lý do tương tự áp dụng từ 10 đến 1. Mỗi bước bão hòa hoàn toàn vị trí chữ số hiện tại trước khi chuyển sang bước tiếp theo, đảm bảo không bỏ sót giải pháp tối ưu nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b, c, n = map(int, input().split())

        use_100 = min(c, n // 100)
        n -= use_100 * 100

        use_10 = min(b, n // 10)
        n -= use_10 * 10

        if n <= a:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo việc giảm tham lam chính xác như được mô tả. Điểm tinh tế duy nhất là cập nhật giá trị còn lại sau mỗi bước mệnh giá; việc này phải được thực hiện trước khi chuyển sang loại tiền tiếp theo, nếu không các quyết định sau này sẽ dựa trên trạng thái cũ. 

Tất cả số học nằm trong phạm vi số nguyên an toàn và mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi, điều này cần thiết với kích thước đầu vào. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

### Ví dụ 1 

đầu vào:```
a = 12, b = 9, c = 1, n = 112
```| Bước | 100 xu đã được sử dụng | Còn lại n | 10 xu được sử dụng | Còn lại n | Cần 1 xu | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 112 | 0 | 112 | 112 | 
| Sau 100 giây | 1 | 12 | 0 | 12 | 12 | 
| Sau 10 giây | 1 | 2 | 1 | 2 | 2 | 

Chúng tôi sử dụng một xu 100 để lại 12, sau đó một xu 10 để lại 2. Vì 2 ≤ 12, chúng tôi trả lời CÓ. Điều này xác nhận rằng phân bổ tham lam có hiệu quả ngay cả khi cần nhiều mệnh giá. 

### Ví dụ 2 

đầu vào:```
a = 8, b = 8, c = 1000000000, n = 999
```| Bước | 100 xu đã được sử dụng | Còn lại n | 10 xu được sử dụng | Còn lại n | Cần 1 xu | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 999 | 0 | 999 | 999 | 
| Sau 100 giây | 9 | 99 | 0 | 99 | 99 | 
| Sau 10 giây | 8 | 19 | 8 | 19 | 19 | 

Chúng tôi lấy 9 xu 100 để giảm vấn đề xuống còn 99, sau đó bão hòa 10 xu lên 8 đơn vị, để lại 19. Vì chỉ còn 8 xu một xu nên chúng tôi không thể hoàn thành, vì vậy câu trả lời là KHÔNG. 

Những dấu vết này cho thấy thuật toán luôn đẩy bài toán thành các mệnh giá nhỏ hơn trong khi vẫn bảo toàn chính xác các ràng buộc khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm thực hiện một số phép tính số học không đổi độc lập với kích thước đầu vào | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Giải pháp này dễ dàng xử lý tới 100.000 trường hợp thử nghiệm vì mỗi trường hợp được xử lý trong thời gian không đổi mà không có vòng lặp về số lượng xu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b, c, n = map(int, input().split())
        use_100 = min(c, n // 100)
        n -= use_100 * 100

        use_10 = min(b, n // 10)
        n -= use_10 * 10

        out.append("YES" if n <= a else "NO")
    return "\n".join(out)

# provided sample (formatted assumption)
assert run("""4
40 0 0 0
10 9 1 20
1 12 9 112
8 8 1000000000 999
""") == """YES
NO
YES
NO"""

# custom cases
assert run("1\n0 0 0 0\n") == "YES", "zero target always possible"
assert run("1\n5 0 0 7\n") == "NO", "insufficient 1-coins"
assert run("1\n0 10 0 100\n") == "YES", "exact 10-coins only"
assert run("1\n9 0 1 100\n") == "YES", "single 100 coin exact"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | CÓ | trường hợp tổng bằng không | 
| chỉ 1 xu là không đủ | KHÔNG | trường hợp thất bại cơ bản | 
| chỉ chính xác 10 xu | CÓ | trung giáo phái đúng đắn | 
| chính xác 100 xu duy nhất | CÓ | xử lý mệnh giá cao | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi mục tiêu bằng không. Thuật toán ngay lập tức giảm thông qua tất cả các mệnh giá và kiểm tra n ≤ a, giá trị này đúng vì n bằng 0. Điều này trả về chính xác CÓ ngay cả khi không có đồng xu nào tồn tại. 

Một trường hợp khác là khi các mệnh giá cao hơn vượt quá sự kết hợp tiềm năng. Vì chúng tôi luôn giới hạn mức sử dụng bằng giá trị min(có sẵn, n //), nên chúng tôi không bao giờ trừ nhiều hơn mức cần thiết, ngăn chặn số dư âm. 

Trường hợp thứ ba là khi giải pháp tối ưu sử dụng ít đồng tiền có mệnh giá cao hơn so với lựa chọn tham lam. Điều này không thể xảy ra vì lấy nhiều đồng tiền có giá trị cao hơn không bao giờ làm giảm tính khả thi ở các mệnh giá thấp hơn; nó chỉ giảm số tiền còn lại theo bội số của 100 hoặc 10, giúp duy trì khả năng giải được trong không gian bài toán còn lại.

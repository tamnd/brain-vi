---
title: "CF 104969C - Hết Pizza Taco"
description: "Chúng tôi được sắp xếp một hàng người cố định trước khi Shelly đến và được chia sẻ các lát bánh pizza, tacos và các phần nước sốt."
date: "2026-06-28T18:51:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104969
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 1 (Advanced)"
rating: 0
weight: 104969
solve_time_s: 82
verified: false
draft: false
---

[CF 104969C - Hết Pizza Taco](https://codeforces.com/problemset/problem/104969/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được sắp xếp một hàng người cố định trước khi Shelly đến và được chia sẻ các lát bánh pizza, tacos và các phần nước sốt. Mỗi người trong hàng đợi độc lập chọn số lượng món để lấy, nhưng họ bị giới hạn bởi hai quy tắc: họ có thể lấy tổng cộng tối đa hai món ăn và nếu họ lấy đúng hai loại nước sốt, họ được phép lấy thêm một món ăn theo lựa chọn của mình. 

Đơn hàng của Shelly đã được ấn định là một lát pizza và một taco, vì vậy mối quan tâm của cô không phải là lựa chọn của chính cô mà là liệu những người đi trước cô có thể tiêu thụ đủ tài nguyên để làm cạn kiệt pizza hoặc tacos trước khi cô đến quầy hay không. Câu hỏi đặt ra là liệu có tồn tại bất kỳ chuỗi lựa chọn hợp lệ nào cho những người ở phía trước sao cho ít nhất một trong số hàng bánh pizza hoặc taco trở thành 0 hoặc âm trước đến lượt Shelly hay không. 

Dữ liệu đầu vào cung cấp số lượng người trước Shelly, tiếp theo là số lượng pizza, tacos và nước sốt ban đầu. Đầu ra chỉ đơn giản là liệu có thể tiêu thụ hết pizza hoặc tacos theo bất kỳ chuỗi lựa chọn hợp lệ nào hay không. 

Các ràng buộc cho phép tối đa 100.000 người và số lượng vật phẩm lên tới 100.000. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào cố gắng khám phá tất cả các kết hợp lựa chọn có thể có của mỗi người, vì ngay cả một hệ số phân nhánh nhỏ cũng sẽ bùng nổ theo thời gian theo cấp số nhân. Một giải pháp phải đơn giản hóa vấn đề thành lý luận về mức tiêu dùng trong trường hợp xấu nhất của mỗi người. 

Một vấn đề tế nhị là sự sẵn có của nước sốt có thể thay đổi những gì mỗi người có thể dùng. Nếu không xử lý cẩn thận, người ta có thể cho rằng nước sốt là không liên quan hoặc luôn đủ. Một cái bẫy khác là cho rằng mỗi người luôn lấy cùng một số lượng món, trong khi trên thực tế, quy tắc “món thêm với hai loại nước sốt” tạo ra hai chế độ tiêu dùng riêng biệt ảnh hưởng đến việc liệu ai đó có thể tối đa hóa việc sử dụng pizza hoặc taco hay không. 

Trường hợp cốt lõi là khi nước sốt vừa đủ để cho phép tất cả mọi người tiêu thụ thêm, điều này có thể làm tăng đáng kể tổng nhu cầu có thể có và thay đổi liệu có thể đạt được mức cạn kiệt hay không. 

## Phương pháp tiếp cận 

Một góc nhìn bạo lực sẽ cố gắng mô phỏng tất cả các cách có thể mà mỗi người trong số n người có thể chọn các món có ràng buộc, theo dõi số pizza, tacos và nước sốt còn lại. Đối với mỗi người, chúng tôi sẽ phân loại xem họ ăn không, một hay hai món và liệu họ có dùng nước sốt hay không. Ngay cả khi chúng ta loại bỏ các trạng thái không hợp lệ khi tài nguyên trở nên âm, số lượng trạng thái có thể có sẽ tăng theo cấp số nhân với n, vì mỗi người đưa ra nhiều quyết định độc lập. Với n = 10^5 thì điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không quan tâm đến trình tự lựa chọn chính xác, mà chỉ quan tâm đến việc liệu có tồn tại trình tự làm cạn kiệt pizza hoặc tacos hay không. Điều này biến vấn đề thành câu hỏi “mức tiêu thụ tối đa có thể”. Thay vì mô phỏng từng người một, chúng tôi hỏi: n người đầu tiên có thể lấy tối đa bao nhiêu món pizza hoặc taco với hành vi đối nghịch tối ưu? 

Mỗi người có thể lấy tối đa hai món và đôi khi là món thứ ba nếu họ có đúng hai loại nước sốt. Điều này có nghĩa là câu hỏi thực sự là có bao nhiêu người có thể được “nâng cấp” để lấy 3 vật phẩm thay vì 2. Vì mỗi lần nâng cấp như vậy tiêu tốn chính xác 2 loại nước sốt nên số người được nâng cấp bị giới hạn bởi cả Z và n. 

Vì vậy, tổng số lượng mặt hàng tối đa được lấy bị giới hạn bằng cách tối đa hóa số người lấy 3 món, sau đó những người còn lại lấy 2. Từ đó, chúng tôi tính toán mức tiêu thụ trong trường hợp xấu nhất của một loại mặt hàng bằng cách giả sử tất cả các mặt hàng được lấy đều tập trung vào pizza (hoặc tacos). Nếu X hoặc Y nhỏ hơn hoặc bằng nhu cầu tối đa có thể có đối với một danh mục thì có thể cạn kiệt. 

Vấn đề giảm xuống để kiểm tra xem: 

chúng ta có thể phân bổ đủ “vị trí vật phẩm” cho n người, với điều kiện tối đa min(n, Z // 2) người có thể lấy 3 vật phẩm và những người còn lại lấy 2 vật phẩm.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ trong n | O(n) đệ quy/trạng thái | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán sang tính toán số lượng mặt hàng tối đa mà n người đầu tiên có thể tiêu thụ. 

1. Tính xem có bao nhiêu người có thể nhận thêm phần nước sốt. Mỗi người như vậy cần 2 loại nước sốt nên số người được nâng cấp là`k = min(n, Z // 2)`. Điều này tối đa hóa số lượng người có thể khai thác nước sốt mà không vượt quá nguồn cung cấp nước sốt hoặc số người sẵn có. 
2. Đối với k người này, mỗi người lấy 3 món thay vì 2, đóng góp`3k`tổng số mặt hàng được tiêu thụ. 
3. Đối với phần còn lại`n - k`người, mỗi người chỉ lấy cơ sở tối đa 2 món, góp phần`2(n - k)`mặt hàng. 
4. Tổng hợp những khoản đóng góp này để có được tổng số vật phẩm tối đa có thể tiêu thụ trước khi Shelly đến:`total = 3k + 2(n - k)`. 
5. Vì chúng tôi chỉ quan tâm đến việc liệu pizza hoặc tacos có thể hết hay không, nên chúng tôi xem xét mức độ tập trung trong trường hợp xấu nhất: nếu tất cả các mặt hàng được tiêu thụ đều thuộc một loại thì mức tiêu thụ tối đa có thể có của một tài nguyên là`total`. Chúng tôi so sánh`total`chống lại cả X và Y. 
6. Nếu X <= tổng hoặc Y <= tổng, thì xuất “có”, nếu không thì xuất “không”. 

### Tại sao nó hoạt động 

Bất biến chính là mọi thứ tự lựa chọn hợp lệ đều tương ứng với việc lựa chọn tối đa hai món cho mỗi người, với một tập hợp con những người tùy ý nhận được chính xác một món bổ sung khi và chỉ khi họ tiêu hai loại nước sốt. biểu thức`3k + 2(n-k)`nắm bắt tổng số vị trí vật phẩm tối đa mà bất kỳ cấu hình hợp lệ nào có thể tạo ra, bởi vì nó bão hòa cả hai ràng buộc một cách độc lập: nâng cấp giới hạn nước sốt và giới hạn cho mỗi người. Bất kỳ hoạt động thực thi thực tế nào cũng không thể vượt quá giới hạn này và tồn tại một chiến lược đạt được điều đó bằng cách luôn tối đa hóa số lượng vật phẩm trên mỗi người bất cứ khi nào có thể. Điều này làm giảm câu hỏi đối nghịch thành một so sánh vô hướng duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    X, Y, Z = map(int, input().split())

    k = min(n, Z // 2)
    total = 3 * k + 2 * (n - k)

    if X <= total or Y <= total:
        print("yes")
    else:
        print("no")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tính toán số người có thể được nâng cấp bằng cách sử dụng nước sốt, sau đó tính ra tổng công suất tiêu thụ. Sự tinh tế duy nhất là đảm bảo chia số nguyên`Z // 2`, vì mỗi người được nâng cấp tiêu thụ chính xác hai loại nước sốt. 

So sánh cuối cùng kiểm tra cả hai tài nguyên một cách độc lập vì sự cạn kiệt của một trong hai là đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 21
X = 60
Y = 70
Z = 108
```Chúng tôi tính toán số lượng người có thể sử dụng nước sốt để nâng cấp. 

| Bước | k = phút(n, Z//2) | tổng số mặt hàng | X | Y | Tình trạng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | phút(21, 54) = 21 | 3_21 + 2_0 = 63 | 60 | 70 | X <= tổng cộng | 

Ở đây, tất cả 21 người đều có thể được nâng cấp vì 108 loại nước sốt cho phép nâng cấp 54 người, nhưng chỉ tồn tại 21 người. Mỗi người lấy 3 món, nên tổng lượng tiêu thụ là 63. Vì nguồn cung pizza là 60 nên có thể pizza sẽ hết trước khi Shelly đến. 

Dấu vết này cho thấy lượng nước sốt dồi dào cho phép mỗi người tiêu thụ tối đa, làm tăng áp lực lên tài nguyên. 

### Mẫu 2 

đầu vào:```
n = 19
X = 60
Y = 70
Z = 108
```| Bước | k | tổng số mặt hàng | X | Y | Tình trạng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | phút(19, 54) = 19 | 3*19 = 57 | 60 | 70 | X > tổng và Y > tổng | 

Ngay cả khi sử dụng hết nước sốt, tổng lượng tiêu thụ chỉ là 57 món. Cả pizza và tacos đều vượt quá giới hạn này nên không thể cạn kiệt hoàn toàn. 

Điều này chứng tỏ rằng khi nguồn cung vượt quá khả năng tiêu thụ tối đa có thể thì không có chiến lược đặt hàng nào có thể gây ra tình trạng cạn kiệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học bất kể kích thước đầu vào | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Việc tính toán là thời gian không đổi, dễ dàng phù hợp với giới hạn lên tới 100.000 người và số lượng vật phẩm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []

    def input():
        return sys.stdin.readline()

    n = int(sys.stdin.readline().strip())
    X, Y, Z = map(int, sys.stdin.readline().split())

    k = min(n, Z // 2)
    total = 3 * k + 2 * (n - k)

    return "yes\n" if (X <= total or Y <= total) else "no\n"

# provided samples (adapted formatting)
assert run("21\n60 70 108\n") == "yes\n", "sample 1"
assert run("19\n60 70 108\n") == "no\n", "sample 2"

# custom cases
assert run("0\n1 1 1\n") == "no\n", "no people means no consumption"
assert run("1\n1 1 10\n") == "yes\n", "single person can fully consume"
assert run("5\n100 100 0\n") == "no\n", "no sauce, limited consumption"
assert run("10\n5 100 100\n") == "yes\n", "small pizza forces yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0, nguồn cung nhỏ | không | trường hợp cạnh hàng đợi bằng không | 
| 1 người, sốt lớn | vâng | trường hợp tối thiểu gây ra sự cạn kiệt | 
| không có nước sốt | không | chỉ xác minh giới hạn 2 mục | 
| nhỏ X | vâng | ranh giới nơi pizza hết sớm | 

## Vỏ cạnh 

Trường hợp một bên là khi không có người ở phía trước. Trong trường hợp đó, việc tiêu thụ sẽ không xảy ra và cả pizza lẫn tacos đều không thể hết trước Shelly. Công thức cho`k = 0`,`total = 0`, và trả về chính xác là “không”. 

Một trường hợp khác là khi nguồn cung nước sốt cực kỳ lớn nhưng n lại nhỏ. Ngay cả khi Z rất lớn, các lần nâng cấp vẫn bị giới hạn bởi n, do đó thuật toán sẽ ngăn chặn việc đánh giá quá cao mức tiêu thụ một cách chính xác. 

Trường hợp thứ ba là khi Z bằng 0. Sau đó, không thể nâng cấp và tổng mức tiêu thụ là hoàn toàn`2n`. Điều này mô hình chính xác ràng buộc rằng không ai có thể vượt quá hai mục.

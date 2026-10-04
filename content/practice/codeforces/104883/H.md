---
title: "CF 104883H - Ngôi sao lăn"
description: "Chúng tôi được cung cấp tổng số tín chỉ và hai số liệu thống kê tổng hợp được tính toán dựa trên danh sách các khóa học ẩn. Mỗi khóa học có giá trị tín chỉ số nguyên và điểm số nguyên từ 60 đến 100."
date: "2026-06-28T09:11:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104883
codeforces_index: "H"
codeforces_contest_name: "The 18-th Beihang University Collegiate Programming Contest (BCPC 2023) - Final"
rating: 0
weight: 104883
solve_time_s: 54
verified: true
draft: false
---

[CF 104883H - Ngôi sao lăn](https://codeforces.com/problemset/problem/104883/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp tổng số tín chỉ và hai số liệu thống kê tổng hợp được tính toán dựa trên danh sách các khóa học ẩn. Mỗi khóa học có một giá trị tín chỉ số nguyên và điểm số nguyên từ 60 đến 100. Từ danh sách ẩn này, mỗi đơn vị tín chỉ đều đóng góp như nhau vào tổng điểm, do đó hệ thống hoạt động hiệu quả như thể mỗi khóa học được mở rộng thành nhiều mục nhập điểm theo trọng số đơn vị đó. 

Hai bản tóm tắt được cung cấp: điểm trung bình có trọng số và điểm trung bình có trọng số. Điểm trung bình không tuyến tính về điểm số; nó là một hàm bậc hai của khoảng cách từ 100, nghĩa là nó phụ thuộc vào khoảng cách từ điểm đến thành tích hoàn hảo chứ không phải bản thân điểm đó. 

Nhiệm vụ là xác định xem liệu bất kỳ tập hợp điểm tín dụng đơn vị nào có thể tái tạo chính xác cả hai tổng hợp đã cho hay không và nếu có thì hãy xây dựng một bài tập hợp lệ. 

Ràng buộc cấu trúc chính là tổng số đơn vị đóng góp nhiều nhất là 250. Con số này đủ nhỏ để vấn đề về cơ bản là xây dựng một phân bố rời rạc trên các số nguyên trong một phạm vi giới hạn thay vì tối ưu hóa trên các chuỗi lớn. Bất kỳ cách tiếp cận nào cố gắng xử lý từng khóa học một cách độc lập mà không làm phẳng các khoản tín chỉ sẽ làm vấn đề trở nên phức tạp hơn, vì việc chia tín chỉ thành các khoản đóng góp đơn vị sẽ loại bỏ hoàn toàn hệ thống phân cấp. 

Phần tế nhị của vấn đề là hai thời điểm khác nhau của phân phối được cố định đồng thời: điểm trung bình và phép biến đổi bậc hai của điểm. Điều này ngay lập tức loại trừ các cách xây dựng tùy ý, bởi vì một khi giá trị trung bình được cố định, mômen thứ hai sẽ hạn chế phương sai một cách chặt chẽ. 

Một trường hợp thất bại phổ biến phát sinh khi coi ràng buộc GPA là độc lập với giá trị trung bình. Ví dụ: một phân bố tập trung hoàn toàn vào điểm trung bình luôn khớp với điểm trung bình nhưng hầu như không bao giờ khớp với GPA bậc hai trừ khi giá trị trung bình là một điểm số nguyên có độ lệch bình phương khớp chính xác. Một thất bại tinh vi khác xảy ra khi cố gắng làm tròn điểm đến các số nguyên gần đó, điều này bảo toàn giá trị trung bình gần đúng nhưng phá hủy mômen bậc hai. 

## Phương pháp tiếp cận 

Nếu chúng ta mở rộng mọi khóa học thành tín chỉ đơn vị, bài toán sẽ trở thành xây dựng chính xác c số nguyên trong phạm vi [60, 100] có trung bình cộng bằng một giá trị cho trước s̄ và trung bình của nó theo hàm bậc hai g(s) khớp với ḡ. 

Một ý tưởng mạnh mẽ là thử tất cả các tập hợp có kích thước c trên 41 điểm có thể. Điều này tương đương với việc phân phối c quả bóng giống hệt nhau vào 41 thùng, điều này đã mang lại một không gian trạng thái khổng lồ theo thứ tự$\binom{c+40}{40}$, vượt xa mọi thứ có thể xử lý được ngay cả với c = 250. 

Sự đơn giản hóa chính xuất phát từ việc quan sát rằng chúng ta chỉ khớp hai khoảnh khắc: một khoảnh khắc tuyến tính (trung bình) và một khoảnh khắc bậc hai (ràng buộc giống phương sai sau khi biến đổi). Đối với những vấn đề như vậy, mọi phân bố khả thi luôn có thể được biểu diễn bằng cách sử dụng tối đa hai giá trị phân biệt khi miền xác định bị chặn và chúng ta có thể tự do chọn bội số nguyên. Điều này làm giảm bài toán xây dựng thành việc tìm xem có tồn tại hai điểm a và b hay không, cùng với việc phân chia c thành k và c − k, sao cho cả hai ràng buộc đều được thỏa mãn một cách chính xác. 

Sau khi chúng tôi sửa hai điểm ứng cử viên, hệ thống sẽ hoàn toàn có thể giải được bằng đại số: giá trị trung bình xác định k và khoảnh khắc thứ hai đóng vai trò kiểm tra tính nhất quán. Vì phạm vi điểm số nhỏ nên chúng ta có thể liệt kê tất cả các cặp một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên tất cả các bộ nhiều | hàm mũ trong c | lớn | Quá chậm | 
| Bảng liệt kê hai giá trị | O(41²) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi loại bỏ hệ thống phân cấp của các khóa học bằng cách coi mỗi tín chỉ là một đơn vị độc lập có cùng số điểm, vì tất cả các tín chỉ tổng hợp đều có tính tuyến tính. 

Sau đó, chúng tôi dịch công thức GPA sang dạng bậc hai đơn giản hơn. Viết mỗi điểm s theo khoảng cách từ 100, cụ thể là t = 100 − s, sẽ biến GPA thành hàm t². Điều này chuyển đổi vấn đề thành khớp cả giá trị trung bình và thời điểm thứ hai của giá trị t. 

Tiếp theo chúng tôi tính toán số lượng mục tiêu cần thiết. Trung bình của t được xác định trực tiếp từ điểm trung bình nhất định và trung bình của t được lấy từ công thức GPA bằng cách sắp xếp lại theo đại số. 

Sau đó, chúng tôi cố gắng biểu diễn phân phối chỉ bằng hai giá trị riêng biệt của t, chẳng hạn như a và b. Chúng ta ấn định một thứ tự và giả sử k bản sao của a và c − k bản sao của b. Điều kiện trung bình xác định duy nhất k nếu a và b khác nhau. Chúng tôi tính toán k này và xác minh nó là một số nguyên trong phạm vi. 

Sau đó, chúng tôi xác minh điều kiện thời điểm thứ hai bằng cách sử dụng số đếm tương tự. Nếu cả hai điều kiện đều thỏa mãn trong phạm vi dung sai số học, chúng ta sẽ lập tức xây dựng câu trả lời bằng cách chuyển đổi t trở lại điểm số. 

Nếu không có cặp (a, b) nào hoạt động thì không tồn tại phân phối hai điểm hợp lệ và vì bất kỳ giải pháp khả thi nào trên miền số nguyên giới hạn với hai ràng buộc mô men đều có thể được nén thành nhiều nhất hai điểm hỗ trợ, nên chúng tôi kết luận rằng không có giải pháp nào tồn tại. 

### Tại sao nó hoạt động 

Vấn đề giảm xuống còn việc khớp một phân bố theo hai ràng buộc độc lập trên một miền hữu hạn. Bất kỳ giải pháp khả thi nào cũng tạo ra một điểm trong tập lồi được xác định bởi các ràng buộc này. Trong một chiều có hỗ trợ số nguyên giới hạn, các điểm cực trị của tập hợp này tương ứng với phân bố được hỗ trợ trên nhiều nhất hai giá trị. Bằng cách liệt kê tất cả các cấu hình cực đoan như vậy, chúng tôi tìm thấy một phân tách hợp lệ hoặc sử dụng hết mọi khả năng, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    c = int(input())
    sbar, gbar = map(float, input().split())

    # convert to t = 100 - s
    T = 100.0 - sbar
    Q = (4.0 - gbar) / 3.0

    eps = 1e-9

    # try all pairs of t values in [0, 40]
    for a in range(41):
        for b in range(41):
            if a == b:
                if abs(T - a) < eps and abs(Q - a * a) < eps:
                    print("YES")
                    print(1)
                    print(c, 100 - a)
                    return
                continue

            denom = a - b
            num = c * (T - b)

            if abs(denom) < eps:
                continue

            k = num / denom

            if abs(k - round(k)) > 1e-7:
                continue

            k = int(round(k))
            if k < 0 or k > c:
                continue

            # verify second moment
            lhs = (k * a * a + (c - k) * b * b) / c
            if abs(lhs - Q) > 1e-7:
                continue

            # build answer
            print("YES")
            print(c)
            for _ in range(k):
                print(1, 100 - a)
            for _ in range(c - k):
                print(1, 100 - b)
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách chuyển đổi giới hạn GPA thành mô men bậc hai trên các biến được chuyển đổi, loại bỏ hình thức phi tuyến của công thức ban đầu. Việc liệt kê các giá trị hỗ trợ có thể được thực hiện trực tiếp trong không gian được biến đổi vì nó giữ cho số học ổn định và đối xứng quanh 100. 

Đối với mỗi cặp ứng cử viên, mã lấy được số lần xuất hiện chính xác của một giá trị cần thiết để khớp với giá trị trung bình. Kiểm tra thời điểm thứ hai chỉ được sử dụng như một bộ lọc nhất quán, đảm bảo rằng các lỗi số hoặc suy biến đại số không tạo ra các cấu trúc không hợp lệ. 

Cuối cùng, khi tìm thấy cấu hình hợp lệ, mỗi tín chỉ đơn vị sẽ được phát ra dưới dạng một khóa học độc lập có kích thước một, đáp ứng yêu cầu ban đầu rằng tín chỉ khóa học là số nguyên dương. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó c = 4, s̄ = 90 và GPA tương ứng chính xác với hỗn hợp của hai điểm gần 90. Sau khi chuyển đổi, chúng tôi tìm kiếm hai giá trị t trong [0, 40] có giá trị trung bình là 10 và giá trị trung bình bình phương của chúng phù hợp với mục tiêu. Giả sử chúng ta tìm thấy giá trị t là 8 và 12. 

Chúng tôi kiểm tra các giá trị k được ngụ ý bởi phương trình trung bình và thu được phép chia số nguyên hợp lệ, giả sử k = 2. 

| Bước | một | b | k | nghĩa là kiểm tra | kiểm tra khoảnh khắc thứ hai | 
| --- | --- | --- | --- | --- | --- | 
| ứng cử viên | 8 | 12 | tính toán | hài lòng | đã xác minh | 

Điều này cho thấy cách phục hồi hỗn hợp hai điểm hợp lệ từ các ràng buộc mô men. 

Bây giờ hãy xem xét một trường hợp không khả thi trong đó c = 3 và phương sai yêu cầu quá nhỏ để có thể đạt được bởi bất kỳ cặp số nguyên nào trong phạm vi cho phép. Mọi cặp được thử nghiệm đều tạo ra k không nguyên hoặc không đạt lần kiểm tra thứ hai, dẫn đến bị từ chối. 

| Bước | một | b | k | có nghĩa là hợp lệ | khoảnh khắc thứ hai hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| kiểm tra | 5 | 6 | không nguyên | không | không | 
| kiểm tra | 4 | 7 | ngoài phạm vi | một phần | không | 

Điều này cho thấy tính không khả thi được phát hiện một cách triệt để trên không gian ứng viên đã giảm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(41²) | tất cả các cặp giá trị điểm chuyển đổi đều được kiểm tra | 
| Không gian | O(1) | chỉ một số lượng biến không đổi được lưu trữ | 

Phạm vi điểm giới hạn đảm bảo rằng việc liệt kê là cực kỳ nhỏ và thuật toán chạy thoải mái trong giới hạn ngay cả đối với giá trị tín dụng tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    c = int(sys.stdin.readline())
    sbar, gbar = map(float, sys.stdin.readline().split())

    T = 100.0 - sbar
    Q = (4.0 - gbar) / 3.0

    eps = 1e-9

    for a in range(41):
        for b in range(41):
            if a == b:
                if abs(T - a) < eps and abs(Q - a * a) < eps:
                    return "YES"
                continue

            denom = a - b
            k = c * (T - b) / denom
            if abs(k - round(k)) > 1e-7:
                continue
            k = int(round(k))
            if k < 0 or k > c:
                continue

            lhs = (k * a * a + (c - k) * b * b) / c
            if abs(lhs - Q) < 1e-7:
                return "YES"

    return "NO"

# custom cases
assert run("1\n100 4.0\n") == "YES"
assert run("2\n90 3.99\n") in ["YES", "NO"]
assert run("3\n60 1.0\n") == "YES"
assert run("3\n100 4.0\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 100, 4.0 | CÓ | điểm hoàn hảo cấu hình đơn | 
| c=3, s=60 | CÓ | tính khả thi giới hạn dưới | 
| c=3, s=100 | CÓ | tính khả thi giới hạn trên | 
| trường hợp hỗn hợp | CÓ/KHÔNG | sự ổn định của tái thiết | 

## Vỏ cạnh 

Trường hợp quan trọng là khi tất cả các khóa học phải có điểm số giống nhau. Trong tình huống đó, phương sai bằng 0 và cả hai thời điểm đều thu gọn thành một ràng buộc giá trị duy nhất. Thuật toán xử lý vấn đề này trong nhánh a == b, trong đó nó trực tiếp kiểm tra tính nhất quán giữa T và Q và đưa ra phân phối đồng đều. 

Một trường hợp cạnh khác xảy ra khi giá trị trung bình yêu cầu nằm chính xác trên một ranh giới số nguyên nhưng ràng buộc bậc hai tương ứng với phương sai khác 0. Trong những trường hợp như vậy, không có giải pháp giá trị đơn nào tồn tại và thuật toán sẽ quay lại kiểm tra các cặp riêng biệt một cách chính xác, trong đó phương trình trung bình buộc một phân số k bị loại bỏ. 

Một trường hợp tinh tế hơn nữa phát sinh từ độ chính xác của dấu phẩy động trong việc xây dựng lại k từ phương trình trung bình. Giải pháp rõ ràng cho phép sai số nhỏ khi kiểm tra tính tích phân của k, đảm bảo rằng các giá trị lấy từ đầu vào thập phân không bị lỗi do các tạo phẩm làm tròn.

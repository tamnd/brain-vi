---
title: "CF 1791C - Thêm vào và thêm vào"
description: "Chúng ta được cấp một chuỗi nhị phân là kết quả cuối cùng của việc áp dụng nhiều lần một thao tác rất cụ thể. Mỗi thao tác lấy chuỗi hiện tại và mở rộng nó bằng cách thêm một ký tự vào đầu bên trái và một ký tự vào đầu bên phải, với ràng buộc là cả hai ký tự đó đã thêm vào…"
date: "2026-06-09T10:28:47+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1791
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 849 (Div. 4)"
rating: 800
weight: 1791
solve_time_s: 75
verified: true
draft: false
---

[CF 1791C - Thêm vào trước và thêm vào](https://codeforces.com/problemset/problem/1791/C) 

**Đánh giá:** 800 
**Thẻ:** triển khai, hai con trỏ 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân là kết quả cuối cùng của việc áp dụng nhiều lần một thao tác rất cụ thể. Mỗi thao tác lấy chuỗi hiện tại và mở rộng nó bằng cách thêm một ký tự vào đầu bên trái và một ký tự vào đầu bên phải, với ràng buộc là hai ký tự được thêm vào luôn khác nhau. Một đầu nhận được một`0`, đầu kia nhận được một`1`, nhưng chúng ta có quyền tự do lựa chọn bên nào được bên nào. 

Nhiệm vụ là đảo ngược quá trình này theo nghĩa nén. Instead of reconstructing all possible histories, we only need to determine the minimum possible length of the string before any operations were applied.

 Quan sát quan trọng là mọi thao tác chỉ thêm các ký hiệu ở ranh giới, do đó phần giữa của chuỗi không bao giờ được sửa đổi nội bộ. This immediately suggests that the original string corresponds to what remains after peeling off symmetric “outer pairs” that could have been added during the process.

 Các ràng buộc rất nhỏ: tối đa 100 trường hợp thử nghiệm và tổng độ dài lên tới 2000 mỗi thử nghiệm. Điều này loại trừ mọi sự tái tạo theo cấp số nhân của tất cả các lịch sử có thể có, nhưng vẫn cho phép quét tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. MỘT$O(n^2)$giải pháp vẫn sẽ trôi qua một cách thoải mái, nhưng giải pháp dự định là tuyến tính. 

Một ý tưởng ngây thơ nhưng hấp dẫn là mô phỏng các thao tác đảo ngược bằng cách đoán ký tự nào đến từ thao tác nào. Điều này nhanh chóng trở nên mơ hồ vì mỗi thao tác có thể được hoàn tác theo hai cách. That leads to exponential branching.

 Các trường hợp cạnh quan trọng: 

Một chuỗi như`"1"`không có mức giảm hợp lệ, vì vậy câu trả lời là 1 vì không có thao tác nào có thể tạo ra một ký tự đơn lẻ mà không bắt đầu từ độ dài 1. 

Một chuỗi như`"10"`có thể được giảm hoàn toàn thành trống, vì nó có thể là kết quả của một thao tác bắt đầu từ trống. 

Một chuỗi như`"10101"`không thể giảm bớt chút nào, vì cấu trúc bên ngoài không bao giờ khớp với mẫu cặp có thể tháo rời. 

Những ví dụ này gợi ý rằng chúng tôi thực sự đang tìm kiếm xem có bao nhiêu chuỗi có thể bị “hủy” ở cả hai đầu theo quy tắc ghép nối. 

## Phương pháp tiếp cận 

Chế độ xem brute-force cố gắng xây dựng lại chuỗi gốc bằng cách mô phỏng các hoạt động ngược lại. Từ chuỗi cuối cùng, chúng ta sẽ cố gắng đoán xem liệu thao tác cuối cùng có loại bỏ một`0`từ bên trái và một`1`từ bên phải hoặc ngược lại. Mỗi lựa chọn tạo ra một chuỗi nhỏ hơn và chúng tôi lặp lại. 

Về nguyên tắc, điều này có tác dụng vì mọi lịch sử hợp lệ đều tương ứng với một số chuỗi đảo ngược như vậy. Tuy nhiên, hệ số phân nhánh là 2 ở mỗi bước và trong trường hợp xấu nhất độ sâu là$O(n)$, dẫn đến$O(2^n)$khả năng. Ngay cả với tính năng ghi nhớ, số lượng trạng thái riêng biệt vẫn theo cấp số nhân trong trường hợp xấu nhất vì các kiểu loại bỏ khác nhau tạo ra các chuỗi con khác nhau. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải xây dựng lại quá trình. Mỗi hoạt động kết hợp hiệu quả một`0`với một`1`, mỗi bên một cái và loại bỏ chúng khỏi ranh giới. Vì vậy, quá trình tiếp tục miễn là ký tự còn lại ngoài cùng bên trái và ký tự còn lại ngoài cùng bên phải khác nhau. Một khi chúng khớp nhau, không có phép toán ngược hợp lệ nào có thể giải thích chúng đồng thời dưới dạng phép cộng biên. 

Điều này làm giảm vấn đề xuống quy trình thu nhỏ hai con trỏ: liên tục loại bỏ các cặp ranh giới đối diện khớp nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng ý tưởng đảo ngược cấu trúc bằng cách thu nhỏ sợi dây ở cả hai đầu. 

1. Khởi tạo hai con trỏ, một ở đầu và một ở cuối chuỗi. Chúng đại diện cho phân đoạn còn lại hiện tại vẫn có thể là chuỗi gốc sau khi hoàn tác các thao tác. 
2. Trong khi con trỏ bên trái nằm hoàn toàn bên trái con trỏ bên phải, hãy kiểm tra các ký tự ở cả hai đầu. 
3. Nếu hai ký tự này khác nhau, chúng ta có thể hiểu chúng là thao tác cuối cùng đặt hai ký tự này ở hai đầu đối diện. Chúng ta loại bỏ cả hai bằng cách di chuyển con trỏ trái sang phải và con trỏ phải sang trái. 
4. Nếu hai ký tự giống nhau thì chúng ta không thể ghép chúng thành một phép toán đảo ngược hợp lệ nên dừng lại ngay. Bất kỳ cấu trúc bên trong nào cũng không thể được giải quyết sâu hơn thành các phép toán biên hợp lệ. 
5. Khi vòng lặp dừng lại, đoạn còn lại giữa các con trỏ là chuỗi gốc ngắn nhất có thể, vì vậy độ dài của nó chính là câu trả lời. 

Điểm tinh tế là chỉ những đầu không khớp mới có thể được “giải thích” bằng một thao tác. Các đầu khớp bị đóng băng vì không có thao tác nào tạo ra hai ký tự giống hệt nhau ở cả hai đầu cùng một lúc. 

### Tại sao nó hoạt động 

Mỗi thao tác thêm hai bit đối diện ở ranh giới. Việc đảo ngược một thao tác chỉ có thể thực hiện được khi các ký tự ngoài cùng hiện tại có thể chính xác là hai bit được thêm vào đó. Điều đó có nghĩa là chúng phải khác nhau. Miễn là các đầu khác nhau, chúng ta có thể bóc chúng theo từng cặp một cách an toàn, bảo tồn sự tồn tại của một số lịch sử xây dựng hợp lệ. Khi các đầu cuối khớp nhau, không có chuỗi thao tác nào có thể tạo ra cấu hình đó bằng cách liên tục thêm các bit đối diện, do đó mọi chuỗi con còn lại phải là một phần của chuỗi gốc. 

Bất biến này đảm bảo rằng mọi thao tác loại bỏ đều tương ứng với một thao tác nghịch đảo hợp lệ và việc dừng xảy ra chính xác khi không thể thực hiện được thao tác nghịch đảo nào nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()

    l, r = 0, n - 1

    while l < r and s[l] != s[r]:
        l += 1
        r -= 1

    print(r - l + 1)
```Giải pháp dựa vào thao tác quét hai con trỏ đơn giản. Điều kiện vòng lặp đảm bảo chúng ta chỉ co lại khi các đầu khác nhau, tương ứng chính xác với thao tác ngược hợp lệ. Khi chúng tôi đạt điểm cuối hoặc điểm giao nhau bằng nhau, chúng tôi dừng lại và đo chuỗi con còn lại. 

Chi tiết triển khai chính là chúng tôi không bao giờ thực sự sửa đổi chuỗi. Chúng tôi chỉ di chuyển các chỉ số để giữ cho giải pháp tuyến tính và tránh những phân bổ không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`100`Chúng tôi theo dõi cách con trỏ phát triển. 

| Bước | tôi | r | s[l] | s[r] | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 1 | 0 | thu nhỏ | 
| 1 | 1 | 1 | - | - | dừng lại | 

Ở bước 0, các đầu khác nhau nên chúng ta loại bỏ cả hai đầu. Sau đó, chỉ`"0"`vẫn còn, và chúng tôi dừng lại. 

Điều này xác nhận rằng ban đầu chỉ có một nhân vật có thể có mặt. 

### Ví dụ 2:`1010110`| Bước | tôi | r | s[l] | s[r] | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 6 | 1 | 0 | thu nhỏ | 
| 1 | 1 | 5 | 0 | 1 | thu nhỏ | 
| 2 | 2 | 4 | 1 | 1 | dừng lại | 

Sau hai lần hủy hợp lệ, chúng tôi đạt được điểm cuối bằng nhau, do đó không thể thực hiện thao tác ngược lại nào nữa. Chuỗi con còn lại là`"101"`, có độ dài 3. 

Điều này chứng tỏ quá trình dừng lại chính xác như thế nào khi tính đối xứng không còn phù hợp với cấu trúc hoạt động được phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi con trỏ di chuyển nhiều nhất$n$tổng số bước, thu hẹp từ cả hai đầu | 
| Không gian |$O(1)$| Chỉ có hai chỉ mục được lưu trữ, không có cấu trúc dữ liệu bổ sung | 

Tổng chiều dài của các trường hợp thử nghiệm đủ nhỏ để một lần quét tuyến tính cho mỗi trường hợp thử nghiệm có thể vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()

        l, r = 0, n - 1
        while l < r and s[l] != s[r]:
            l += 1
            r -= 1
        out.append(str(r - l + 1))

    return "\n".join(out)

# provided samples
assert run("""9
3
100
4
0111
5
10101
6
101010
7
1010110
1
1
2
10
2
11
10
1011011010
""") == """1
2
5
0
3
1
0
2
4"""

# minimum size
assert run("""2
1
0
1
1
""") == """1
1"""

# empty-like reduction
assert run("""1
2
10
""") == """0"""

# already irreducible pattern
assert run("""1
5
10101
""") == """5"""

# fully reducible alternating
assert run("""1
6
101010
""") == """0"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ký tự đơn | 1 | trường hợp cơ bản không giảm | 
| "10" | 0 | hủy toàn bộ chuỗi trống | 
| "10101" | 5 | không thể giảm giá hợp lệ | 
| "101010" | 0 | hủy bỏ xen kẽ tối đa | 

## Vỏ cạnh 

Một chuỗi ký tự đơn như`"0"`hoặc`"1"`không bao giờ đi vào vòng lặp thu hẹp bởi vì`l < r`là sai ngay lập tức. Thuật toán trả về độ dài 1, phù hợp với thực tế là không có chuỗi thao tác nào có thể giảm thêm nữa. 

Vì`"10"`, các con trỏ bắt đầu ở hai đầu đối diện với các ký tự khác nhau, do đó cả hai đều bị loại bỏ, để lại một khoảng trống trong đó`l > r`. Độ dài được tính toán trở thành 0, tương ứng với chuỗi ban đầu trống. 

Vì`"10101"`, ký tự đầu tiên và ký tự cuối cùng khớp nhau (`1`Và`1`), do đó vòng lặp dừng ngay lập tức. Thuật toán kết luận chính xác rằng không thể loại bỏ ranh giới và chuỗi đầy đủ phải là chuỗi gốc.

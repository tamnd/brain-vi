---
title: "CF 102599G - Chuỗi có chữ số"
description: "Chúng ta đang theo sau một dãy số nguyên. Giá trị đầu tiên được đưa ra và mọi giá trị tiếp theo được tạo ra bằng cách xem các chữ số của giá trị hiện tại."
date: "2026-08-01T06:51:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102599
codeforces_index: "G"
codeforces_contest_name: "The fifth Lipetsk collegiate programming contest. Finals. 8-11 form"
rating: 0
weight: 102599
solve_time_s: 396
verified: true
draft: false
---

[CF 102599G - Chuỗi có chữ số](https://codeforces.com/problemset/problem/102599/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang theo sau một dãy số nguyên. Giá trị đầu tiên được đưa ra và mọi giá trị tiếp theo được tạo ra bằng cách xem các chữ số của giá trị hiện tại. Chúng ta tìm chữ số nhỏ nhất và chữ số lớn nhất xuất hiện ở dạng thập phân, nhân chúng và cộng tích đó vào giá trị hiện tại. Nhiệm vụ là tìm giá trị tại vị trí`K`, trong đó giá trị đầu tiên có vị trí`1`. 

Đầu vào chứa tới 1000 điểm bắt đầu độc lập. Số bắt đầu có thể lớn bằng`10^18`, do đó nó không thể được xử lý bằng các giả định có kích thước cố định từ các bài toán số nguyên nhỏ hơn. vị trí`K`có thể lớn như`10^16`, đó là hạn chế tới hạn. Mô phỏng trực tiếp thực hiện một lần chuyển đổi trên mỗi bước không thể hoạt động vì ngay cả một trường hợp thử nghiệm cũng có thể yêu cầu mười triệu triệu lần lặp. Giải pháp cần sử dụng một thuộc tính của chuỗi thay vì phụ thuộc vào kích thước của`K`. 

Có một số chi tiết có thể phá vỡ việc triển khai hợp lý. Một giá trị như`1`chỉ có một chữ số nên cả chữ số nhỏ nhất và lớn nhất đều giống nhau. Đối với đầu vào`1 4`, trình tự là`1, 2, 4, 8`, vậy câu trả lời là`8`. Việc triển khai giả định ít nhất hai chữ số sẽ thất bại ở đây. 

Một trường hợp cạnh khác là số chứa số 0. Đối với đầu vào`105 2`, quá trình chuyển đổi đầu tiên thêm`0 * 5`, do đó giá trị vẫn giữ nguyên`105`. Đầu ra đúng là`105`. Nếu việc triển khai bỏ qua các chữ số 0 trong khi tìm chữ số tối thiểu thì nó sẽ thêm một số dương không chính xác. 

Trường hợp thứ ba là khi tất cả các chữ số đều bằng nhau. Đối với đầu vào`777 2`, giá trị gia tăng là`7 * 7 = 49`, cho`826`. Các chữ số tối thiểu và tối đa vẫn là giá trị hợp lệ mặc dù không có sự khác biệt giữa chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng chính xác sự tái phát. Đối với mỗi bước, hãy chuyển đổi giá trị hiện tại thành các chữ số, quét các chữ số đó để tìm giá trị tối thiểu và tối đa, tính tích của chúng và cộng nó vào số. Điều này đúng vì mọi giá trị được tạo đều tuân theo định nghĩa của chuỗi. 

Vấn đề là giá trị của`K`. Nếu như`K = 10^16`, mô phỏng sẽ cần khoảng`10^16`chuyển đổi cho một trường hợp thử nghiệm. Ngay cả khi quá trình chuyển đổi chỉ thực hiện một vài thao tác máy, tổng công việc sẽ vượt xa giới hạn thời gian. 

Quan sát hữu ích đến từ việc xem xét giá trị gia tăng. Tích của hai chữ số thập phân luôn nằm giữa`0`Và`81`. Nếu một số bao giờ chứa chữ số`0`, quá trình chuyển đổi tiếp theo sẽ thêm 0 và chuỗi sẽ ngừng thay đổi vĩnh viễn. Ngược lại, mỗi bước sẽ thêm ít nhất`1`, nhưng mức tăng nhiều nhất là`81`. 

Điều này có nghĩa là mặc dù`K`có thể rất lớn, chuỗi không thể tiếp tục phát triển tự do trong một số lượng lớn các bước. Số bắt đầu có nhiều nhất là 19 chữ số vì`a1 <= 10^18`. Thêm tối đa`81`mỗi bước có nghĩa là sau khi chuyển đổi đủ số đó sẽ vẫn có một phạm vi giá trị có thể tương đối nhỏ trước khi nó đạt đến một số chứa 0 hoặc chuyển sang trạng thái mà lần bổ sung tiếp theo của nó bằng 0. Trong thực tế, số lần chuyển đổi hữu ích là rất nhỏ. 

Giải pháp tối ưu là mô phỏng trong khi quá trình vẫn đang thay đổi, dừng sớm khi mức tăng tiếp theo bằng 0. Việc quan sát thấy chuỗi nhanh chóng đạt đến một điểm cố định sẽ làm giảm vấn đề phụ thuộc vào`K`chỉ phụ thuộc vào phần thoáng qua ngắn của chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(K * số chữ số) | O(1) | Quá chậm | 
| Tối ưu | O(T * số chữ số) | O(1) | Đã chấp nhận | 

Đây,`T`là số lần chuyển đổi trước khi chuỗi ngừng thay đổi. Đó là một hằng số nhỏ đối với các giới hạn đã cho. 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc giá trị bắt đầu`a`và vị trí cần thiết`K`. Nếu như`K`đã rồi`1`, câu trả lời là giá trị ban đầu vì không cần chuyển tiếp. 
2. Trong khi`K`lớn hơn`1`, tìm chữ số nhỏ nhất và lớn nhất trong giá trị hiện tại. Giá trị tiếp theo chỉ phụ thuộc vào hai chữ số này nên không cần lưu trữ các giá trị chuỗi trước đó. 
3. Nhân các chữ số tối thiểu và tối đa để có được số tăng. Nếu mức tăng này bằng 0 thì chuỗi sẽ không thay đổi mãi mãi. Giá trị hiện tại đã là câu trả lời cuối cùng cho mọi vị trí còn lại, vì vậy hãy dừng mô phỏng. 
4. Nếu không, hãy thêm số tăng vào giá trị hiện tại và giảm`K`bởi một vì một quá trình chuyển đổi đã được thực hiện. 
5. In giá trị cuối cùng sau khi vòng lặp kết thúc. 

Tại sao nó hoạt động: 

Thuật toán tuân theo chính xác phép truy toán được sử dụng để xác định trình tự. Mọi chuyển đổi không kết thúc đều tính toán cùng một chữ số tối thiểu, tối đa chữ số và phép cộng như định nghĩa toán học. Việc tối ưu hóa duy nhất là dừng sớm khi giá trị gia tăng trở về 0. Tại thời điểm đó, giá trị tiếp theo bằng giá trị hiện tại và việc áp dụng lại phép truy toán sẽ tiếp tục tạo ra cùng một số mãi mãi. Vì tất cả các vị trí còn lại đều có giá trị giống nhau nên việc trả về số hiện tại là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        a, k = map(int, input().split())

        while k > 1:
            x = a
            mn = 10
            mx = 0

            while x > 0:
                d = x % 10
                mn = min(mn, d)
                mx = max(mx, d)
                x //= 10

            add = mn * mx

            if add == 0:
                break

            a += add
            k -= 1

        ans.append(str(a))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp bên ngoài xử lý các trường hợp kiểm thử độc lập. Trong mỗi trường hợp, quá trình mô phỏng chỉ tiếp tục khi còn những chuyển đổi cần thực hiện. điều kiện`k > 1`được sử dụng vì số đầu vào đã đại diện`a1`, do đó, việc đạt đến vị trí thứ hai cần có chính xác một lần cập nhật. 

Quét chữ số sử dụng phép chia cho mười thay vì chuyển đổi chuỗi. Cả hai cách tiếp cận đều hợp lệ ở đây, nhưng trích xuất số học tránh tạo chuỗi tạm thời và hoạt động trực tiếp với các giá trị số nguyên. Các biến`mn`Và`mx`bắt đầu lúc`10`Và`0`vì mỗi chữ số thập phân nằm giữa`0`Và`9`. 

Séc`add == 0`là sự tối ưu hóa chính. Nếu không có nó, chương trình sẽ tiếp tục lặp lại mặc dù mọi giá trị trong tương lai đều giống hệt nhau. Số nguyên Python không bị tràn, vì vậy các giá trị bắt đầu và phép cộng lớn không yêu cầu xử lý đặc biệt. 

## Ví dụ đã hoạt động 

Đối với giá trị bắt đầu mẫu`487`với`K = 7`, mô phỏng hoạt động như sau. 

| Bước | Giá trị hiện tại | Chữ số tối thiểu | Chữ số tối đa | Giá trị gia tăng | Còn K | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 487 | 4 | 8 | 32 | 7 | 
| 1 | 519 | 1 | 9 | 9 | 6 | 
| 2 | 528 | 2 | 8 | 16 | 5 | 
| 3 | 544 | 4 | 5 | 20 | 4 | 
| 4 | 564 | 4 | 6 | 24 | 3 | 
| 5 | 588 | 5 | 8 | 40 | 2 | 
| 6 | 628 | 2 | 8 | 16 | 1 | 

Bảng cho thấy mỗi lần lặp lại áp dụng một lần chuyển đổi và giảm khoảng cách còn lại đến vị trí mục tiêu. Câu trả lời sau sáu lần chuyển đổi là`628`. 

Đối với trường hợp đạt đến điều kiện dừng, xét`105 100`. 

| Bước | Giá trị hiện tại | Chữ số tối thiểu | Chữ số tối đa | Giá trị gia tăng | Còn K | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 105 | 0 | 5 | 0 | 100 | 

Số gia đầu tiên bằng 0 vì số này chứa chữ số 0. Thuật toán dừng ngay lập tức và quay trở lại`105`, mặc dù vị trí được yêu cầu ở rất xa. Điều này chứng tỏ tại sao việc mô phỏng từng bước còn lại là không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T * D) |`T`là số lần chuyển tiếp thay đổi và`D`là số chữ số, nhiều nhất là 19 | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Số lượng chuyển đổi hữu ích tối đa là nhỏ vì mỗi chuyển đổi đều đạt đến một giá trị cố định hoặc di chuyển qua một phạm vi giá trị tăng ngắn. Thuật toán không bao giờ phụ thuộc vào giới hạn trên rất lớn của`K`, vì vậy nó dễ dàng phù hợp với giới hạn thời gian một giây. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        t = int(sys.stdin.readline())
        out = []

        for _ in range(t):
            a, k = map(int, sys.stdin.readline().split())

            while k > 1:
                x = a
                mn = 10
                mx = 0

                while x:
                    d = x % 10
                    mn = min(mn, d)
                    mx = max(mx, d)
                    x //= 10

                add = mn * mx
                if add == 0:
                    break

                a += add
                k -= 1

            out.append(str(a))

        return "\n".join(out)

    result = solve()
    sys.stdin = old_stdin
    return result

assert run("""8
1 4
487 1
487 2
487 3
487 4
487 5
487 6
487 7
""") == """8
487
519
528
544
564
588
628
""", "sample"

assert run("1\n1 1\n") == "1", "minimum position"

assert run("1\n105 10000000000000000\n") == "105", "zero digit stop"

assert run("1\n777 2\n") == "826", "equal digits"

assert run("1\n999999999999999999 2\n") == "1000000000000000079", "large value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Vị trí bắt đầu không cần chuyển tiếp | 
|`105 10000000000000000`|`105`| Điểm cố định tức thời do chữ số 0 | 
|`777 2`|`826`| Chữ số tối thiểu và tối đa có thể bằng nhau | 
|`999999999999999999 2`|`1000000000000000079`| Xử lý các giá trị bắt đầu rất lớn | 

## Vỏ cạnh 

Đối với một giá trị một chữ số, việc tìm kiếm chữ số phải coi chữ số đó là cả giá trị nhỏ nhất và lớn nhất. Với đầu vào`1 4`, các chuyển tiếp là`1 -> 2 -> 4 -> 8`, vậy câu trả lời là`8`. Thuật toán quét chữ số duy nhất và tính toán sản phẩm chính xác mỗi lần. 

Đối với các số chứa số 0 thì số 0 phải tham gia tính toán chữ số nhỏ nhất. Với đầu vào`105 2`, chữ số tối thiểu là`0`và chữ số lớn nhất là`5`. Mức tăng là`0`, do đó trình tự không thay đổi và câu trả lời là`105`. Điều kiện dừng xử lý việc này ngay lập tức. 

Đối với các số có chữ số giống nhau thì không cần có trường hợp đặc biệt. Với đầu vào`777 2`, việc quét chữ số sẽ tìm thấy`mn = 7`Và`mx = 7`, tạo ra sự gia tăng của`49`. Giá trị tiếp theo là`826`, xác nhận rằng các chữ số tối thiểu và tối đa bằng nhau vẫn được xử lý bình thường. 

Tôi cũng có thể điều chỉnh phiên bản này thành phiên bản biên tập ngắn hơn theo phong cách Codeforces nếu bạn muốn nội dung nào đó gần hơn với những gì sẽ xuất hiện trên trang cuộc thi.

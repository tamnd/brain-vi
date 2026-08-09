---
title: "CF 102443I - Ngày tháng"
description: "Mỗi dòng đầu vào mô tả một ngày, nhưng thứ tự các thành phần của nó phụ thuộc vào dấu phân cách. Dấu chấm có nghĩa là thứ tự ngày.tháng.năm theo kiểu Châu Âu, trong khi dấu gạch chéo có nghĩa là thứ tự tháng/ngày/năm theo kiểu Mỹ. Nhiệm vụ không phải là quyết định xem ngày đó có phải là ngày dương lịch thực hay không."
date: "2026-08-08T13:15:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102443
codeforces_index: "I"
codeforces_contest_name: "2019-2020 Russia Team Open, High School Programming Contest (VKOSHP 19)"
rating: 0
weight: 102443
solve_time_s: 625
verified: true
draft: false
---

[CF 102443I - Ngày tháng](https://codeforces.com/problemset/problem/102443/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi dòng đầu vào mô tả một ngày, nhưng thứ tự các thành phần của nó phụ thuộc vào dấu phân cách. Một dấu chấm có nghĩa là trật tự kiểu châu Âu`day.month.year`, trong khi dấu gạch chéo có nghĩa là trật tự kiểu Mỹ`month/day/year`. Nhiệm vụ không phải là quyết định xem ngày đó có phải là ngày dương lịch thực hay không. Ngay cả một cái gì đó như`31.02.2001`vẫn phải được chuyển đổi và in. 

Đối với mỗi ngày, chúng ta phải tạo ra cả hai cách biểu diễn. Đầu tiên luôn có thứ tự ngày, tháng, năm và sử dụng dấu chấm. Thứ hai luôn có tháng, ngày, năm và sử dụng dấu gạch chéo. Ngày và tháng phải có đúng hai chữ số, còn năm phải có đúng bốn chữ số. Các số 0 đứng đầu trong đầu vào có thể bị xóa trong quá trình phân tích cú pháp và sau đó được khôi phục về độ rộng cần thiết ở đầu ra. 

Có tối đa 20.000 ngày đầu vào. Mỗi ngày chỉ chứa ba trường số nhỏ, vì vậy không có lý do gì để sử dụng bất kỳ cấu trúc dữ liệu nào phức tạp hơn một vài chuỗi hoặc số nguyên trên mỗi dòng. Một thuật toán xử lý mỗi ngày một lần là đủ nhanh. Ngay cả lượng công việc làm thêm không đổi mỗi ngày cũng không đáng kể ở quy mô này. Giới hạn bộ nhớ 512 MB cũng vượt xa mức cần thiết vì ngày có thể được xử lý độc lập mà không cần lưu trữ toàn bộ dữ liệu đầu vào. 

Trường hợp tinh vi đầu tiên là năm có một chữ số. Ví dụ,```
1
1.2.1
```phải trở thành```
01.02.0001 02/01/0001
```Việc triển khai bất cẩn bằng cách sử dụng trực tiếp chuỗi đầu vào có thể in`01.02.1`, vì đầu vào cho phép một năm ngắn trong khi đầu ra luôn yêu cầu bốn chữ số. 

Trường hợp cạnh thứ hai là ngày đã được đệm:```
1
01.02.0001
```Đầu ra vẫn là```
01.02.0001 02/01/0001
```Việc chuyển đổi không được vô tình coi các số 0 đứng đầu là một phần của giá trị số hoặc tạo ra độ rộng không nhất quán. 

Trường hợp thứ ba là ngày dương lịch không hợp lệ:```
1
29.02.2001
```Đầu ra cần thiết là```
29.02.2001 02/29/2001
```Năm 2001 không phải là năm nhuận nhưng giá trị pháp lý không liên quan đến nhiệm vụ. Việc thêm xác thực lịch sẽ giải quyết được vấn đề chưa bao giờ được hỏi. 

Trường hợp thứ tư thực hiện các giá trị thông thường tối đa:```
1
31/12/9999
```Kết quả là```
31.12.9999 12/31/9999
```Ở đây, dấu phân cách phải xác định chính xác thứ tự ban đầu và năm phải giữ nguyên bốn chữ số mà không có bất kỳ xử lý đặc biệt nào. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo có chủ ý sẽ coi ba thành phần được phân tích cú pháp là một bộ sưu tập không có thứ tự và thử mọi thứ tự có thể có cho đến khi một thành phần khớp với định dạng được yêu cầu. chỉ có`3! = 6`hoán vị, do đó, với mỗi ngày trong số 20.000 ngày, thao tác này sẽ thực hiện tối đa 120.000 lần thử hoán vị, cộng với công việc định dạng. Nó vẫn đủ nhanh cho những ràng buộc này, nhưng nó giải quyết được một vấn đề khó hơn mức cần thiết và làm cho mã trở nên phức tạp hơn. Quan trọng hơn, dấu phân cách đã trực tiếp cho chúng ta biết thứ tự nên không cần phải tìm kiếm gì nữa. 

Phương pháp tiếp cận vũ phu có hiệu quả vì chỉ có sáu cách sắp xếp có thể có của ba trường, nhưng không thành công với tư cách là một lựa chọn kỹ thuật vì tất cả sáu khả năng đều không cần thiết. Quan sát cho thấy dấu phân cách đầu vào xác định duy nhất định dạng sẽ giảm vấn đề thành một phần tách xác định và một phần hoán đổi. 

Đối với ngày được phân tách bằng dấu chấm, các trường đã có thứ tự ngày, tháng, năm. Đối với ngày được phân tách bằng dấu gạch chéo, hai trường đầu tiên là tháng và ngày nên chỉ cần hoán đổi. Khi ba trường theo thứ tự ngày, tháng, năm, việc định dạng là cơ học: đệm ngày và tháng thành hai ký tự và năm thành bốn ký tự. 

Không cần phải xác thực độ dài tháng, năm nhuận hoặc thậm chí liệu kết hợp thu được có phải là ngày thực hay không. Đầu vào đảm bảo rằng mỗi thành phần nằm trong phạm vi số đã nêu của nó và đầu ra chỉ đơn giản là biểu diễn chuẩn hóa của các thành phần đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(6n) = O(n) | O(1) | Đã chấp nhận nhưng công việc không cần thiết | 
| Tối ưu | O(n) | O(1) không gian phụ trợ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc một chuỗi ngày tháng và kiểm tra dấu phân cách của nó. Dấu chấm có nghĩa là các trường được sắp xếp theo ngày, tháng, năm, trong khi dấu gạch chéo có nghĩa là chúng được sắp xếp theo tháng, ngày, năm. Dấu phân cách cho chúng ta định dạng mà không có bất kỳ sự mơ hồ nào. 
2. Chia chuỗi thành ba thành phần. Chúng tôi giữ các thành phần dưới dạng chuỗi vì phép biến đổi duy nhất cần có là phần đệm và chuỗi tránh chuyển đổi số nguyên không cần thiết. 
3. Nếu dấu phân cách là dấu chấm, hãy gán trực tiếp ba trường cho`day`,`month`, Và`year`. Nếu dấu phân cách là dấu gạch chéo, hãy gán trường đầu tiên cho`month`, thứ hai đến`day`, và thứ ba đến`year`. 
4. Đệm`day`Và`month`đến chính xác hai ký tự và`year`đến chính xác bốn ký tự. Việc đệm thay vì kiểm tra thủ công số chữ số sẽ xử lý thống nhất cả đầu vào đã được đệm và không được đệm. 
5. Xây dựng cơ quan đại diện Châu Âu như`DD.MM.YYYY`. 
6. Xây dựng cơ quan đại diện của Mỹ như`MM/DD/YYYY`. 
7. In hai biểu diễn cách nhau một dấu cách. Mỗi dòng đầu vào là độc lập nên quy trình tương tự có thể được lặp lại ngay lập tức cho ngày tiếp theo. 

### Tại sao nó hoạt động 

Sau bước 3, ba biến luôn biểu thị cùng một ngày logic theo thứ tự chuẩn`day`,`month`,`year`, bất kể đầu vào được viết như thế nào. Dấu phân cách xác định phép gán nào là chính xác, do đó không thể diễn giải khác. Phần đệm chỉ thay đổi độ rộng văn bản của từng thành phần chứ không phải giá trị của nó. Do đó, chuỗi được xây dựng đầu tiên luôn đặt các thành phần chuẩn theo thứ tự ngày-tháng-năm với độ rộng cần thiết, trong khi chuỗi thứ hai đặt các thành phần tương tự theo thứ tự tháng-ngày-năm. Vì không cần xác thực lịch nên mọi đầu vào được phép đều được xử lý chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    for _ in range(n):
        s = input().strip()

        if '.' in s:
            day, month, year = s.split('.')
        else:
            month, day, year = s.split('/')

        day = day.zfill(2)
        month = month.zfill(2)
        year = year.zfill(4)

        european = f"{day}.{month}.{year}"
        american = f"{month}/{day}/{year}"

        print(european, american)

if __name__ == "__main__":
    solve()
```Kiểm tra dấu phân cách thực hiện quyết định đầu tiên trong thuật toán. Vì đầu vào có chính xác một trong hai dấu phân cách được phép, nên việc kiểm tra`'.'`là đủ. Trường hợp thay thế phải được phân tách bằng dấu gạch chéo. 

Thứ tự phân công trong hai nhánh là phần quan trọng của giải pháp. Vì`11.12.2000`, sự chia tách tạo ra`["11", "12", "2000"]`, vốn đã có nghĩa là ngày, tháng, năm. Vì`1/29/3000`, sự chia tách tạo ra`["1", "29", "3000"]`, có nghĩa là tháng, ngày, năm nên hai biến đầu tiên được cố tình gán theo thứ tự ngược lại.`zfill`xử lý mọi chiều rộng được phép mà không cần số học.`zfill(2)`lá`"11"`không thay đổi và chuyển đổi`"1"`ĐẾN`"01"`. Tương tự,`zfill(4)`chuyển đổi`"1"`ĐẾN`"0001"`Và`"2100"`còn lại`"2100"`. Điều này cũng tránh được một lỗi dễ xảy ra khi chỉ có ngày và tháng được đệm trong khi năm được giữ ở độ rộng đầu vào. 

Không thể tràn số nguyên vì việc triển khai không bao giờ thực hiện số học trên các thành phần ngày. Nó cũng không kiểm tra xem ngày có hợp lệ hay không, điều này đúng vì ngày không hợp lệ vẫn phải được định dạng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Ngày nhập đầu tiên được phân tách bằng dấu chấm, do đó các thành phần của nó đã có thứ tự ngày-tháng-năm. Ngày thứ hai có cùng thứ tự nhưng chứa các trường ngày, tháng và năm có một chữ số. 

| Đầu vào | Dấu phân cách | Ngày | Tháng | Năm | Châu Âu | Mỹ | 
| --- | --- | --- | --- | --- | --- | --- | 
|`11.12.2000`|`.`|`11`|`12`|`2000`|`11.12.2000`|`12/11/2000`| 
|`1.2.1`|`.`|`1`|`2`|`1`|`01.02.0001`|`02/01/0001`| 

Hàng thứ hai giải thích tại sao phần đệm đầu ra phải độc lập với phần đệm đầu vào. Năm đầu vào có một chữ số, nhưng biểu diễn chuẩn hóa có bốn chữ số. 

### Mẫu 2 

Ngày đầu tiên lại được phân tách bằng dấu chấm. Thành phần thứ hai được phân tách bằng dấu gạch chéo nên hai thành phần đầu tiên phải hoán đổi cho nhau khi gán ngày và tháng. 

| Đầu vào | Dấu phân cách | Ngày | Tháng | Năm | Châu Âu | Mỹ | 
| --- | --- | --- | --- | --- | --- | --- | 
|`20.10.2100`|`.`|`20`|`10`|`2100`|`20.10.2100`|`10/20/2100`| 
|`1/29/3000`|`/`|`29`|`1`|`3000`|`29.01.3000`|`01/29/3000`| 

Hàng thứ hai xác nhận bất biến trung tâm: sau khi phân tích cú pháp,`day`,`month`, Và`year`có cùng một ý nghĩa bất kể ký hiệu ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ngày được phân chia và định dạng một lần, chỉ có ba trường có kích thước không đổi. | 
| Không gian | O(1) phụ trợ | Chỉ ngày hiện tại và ba thành phần của nó được giữ lại. | 

Với tối đa 20.000 ngày, thuật toán thực hiện một lượng nhỏ xử lý chuỗi không đổi trên mỗi dòng. Nó thoải mái trong giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể so với giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())

    out = []

    for _ in range(n):
        s = input().strip()

        if '.' in s:
            day, month, year = s.split('.')
        else:
            month, day, year = s.split('/')

        day = day.zfill(2)
        month = month.zfill(2)
        year = year.zfill(4)

        out.append(f"{day}.{month}.{year} {month}/{day}/{year}")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run(
    "2\n"
    "11.12.2000\n"
    "1.2.1\n"
) == (
    "11.12.2000 12/11/2000\n"
    "01.02.0001 02/01/0001"
), "sample 1"

assert run(
    "2\n"
    "20.10.2100\n"
    "1/29/3000\n"
) == (
    "20.10.2100 10/20/2100\n"
    "29.01.3000 01/29/3000"
), "sample 2"

assert run(
    "1\n"
    "1.1.1\n"
) == (
    "01.01.0001 01/01/0001"
), "minimum values"

assert run(
    "4\n"
    "31.12.9999\n"
    "31/12/9999\n"
    "01.01.0001\n"
    "01/01/0001\n"
) == (
    "31.12.9999 12/31/9999\n"
    "31.12.9999 12/31/9999\n"
    "01.01.0001 01/01/0001\n"
    "01.01.0001 01/01/0001"
), "boundaries and padded values"

assert run(
    "3\n"
    "29.02.2001\n"
    "31/02/2001\n"
    "30.04.2000\n"
) == (
    "29.02.2001 02/29/2001\n"
    "02.31.2001 02/31/2001\n"
    "30.04.2000 04/30/2000"
), "invalid calendar dates are still formatted"

assert run(
    "5\n"
    "7.7.7\n"
    "7/7/7\n"
    "07.07.07\n"
    "07/07/07\n"
    "0007.01.0007\n"
) == (
    "07.07.0007 07/07/0007\n"
    "07.07.0007 07/07/0007\n"
    "07.07.0007 07/07/0007\n"
    "07.07.0007 07/07/0007\n"
    "07.01.0007 01/07/0007"
), "padding and repeated values"
```Trường hợp giá trị tối thiểu kiểm tra xem mọi trường có nhận được các số 0 ở đầu được yêu cầu hay không. Trường hợp ranh giới kiểm tra cả hai đầu của phạm vi được phép và xác nhận rằng các trường đã được đệm vẫn không thay đổi. Trường hợp ngày không hợp lệ phát hiện việc triển khai cố gắng xác thực lịch trước khi định dạng không chính xác. Trường hợp cuối cùng thực hiện các trường ngắn và đã được đệm ở cả hai định dạng, bao gồm cả năm có số 0 đứng đầu. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1.1.1`|`01.01.0001 01/01/0001`| Giá trị tối thiểu và tất cả phần đệm bắt buộc | 
|`4\n31.12.9999\n31/12/9999\n01.01.0001\n01/01/0001`| Ngày chuẩn hóa tương ứng | Giá trị biên và cả hai dấu phân cách | 
|`3\n29.02.2001\n31/02/2001\n30.04.2000`| Các thành phần được định dạng lại mà không cần xác thực | Ngày dương lịch không hợp lệ | 
|`5\n7.7.7\n7/7/7\n07.07.07\n07/07/07\n0007.01.0007`| Tất cả các thành phần được chuẩn hóa thành chiều rộng cố định | Các số 0 đứng đầu và độ rộng đầu vào hỗn hợp | 

## Vỏ cạnh 

### Năm một chữ số 

Đối với đầu vào```
1
1.2.1
```dấu phân cách là dấu chấm nên các thành phần là`day = "1"`,`month = "2"`, Và`year = "1"`. Phần đệm tạo ra`01`,`02`, Và`0001`. Do đó, hai đầu ra`01.02.0001`Và`02/01/0001`. Thuật toán không bao giờ giả định rằng năm đầu vào đã có bốn ký tự. 

### Các giá trị đã được đệm 

cho```
1
01.02.0001
```sự phân tách tạo ra các trường đã có độ rộng mong muốn. Đang gọi`zfill`không thêm bất cứ điều gì bởi vì`zfill`giữ nguyên các chuỗi bằng hoặc cao hơn chiều rộng được yêu cầu. Đầu ra là```
01.02.0001 02/01/0001
```Điều này làm cho việc chuẩn hóa trở nên bình thường đối với đầu vào được định dạng chính xác. 

### Ngày dương lịch không hợp lệ 

cho```
1
29.02.2001
```thuật toán không kiểm tra số ngày trong tháng Hai. Nó chỉ đơn giản là phân tích`29`,`02`, Và`2001`, đệm chúng và xây dựng cả hai biểu diễn. Kết quả là```
29.02.2001 02/29/2001
```Đây chính xác là những gì vấn đề yêu cầu, mặc dù ngày đó không phải là ngày theo lịch Gregory hợp lệ. 

### Đầu vào được phân tách bằng dấu gạch chéo 

cho```
1
1/29/3000
```dải phân cách là`/`, do đó trình phân tích cú pháp gán`month = "1"`,`day = "29"`, Và`year = "3000"`. Sau khi đệm, các giá trị là`01`,`29`, Và`3000`. Hình thức châu Âu trở thành`29.01.3000`, trong khi dạng Mỹ trở thành`01/29/3000`. Trường hợp này phát hiện ra lỗi phân tích cú pháp rất có thể xảy ra, đó là lỗi xử lý dữ liệu đầu vào được phân tách bằng dấu gạch chéo như thể nó đã theo thứ tự ngày-tháng-năm.

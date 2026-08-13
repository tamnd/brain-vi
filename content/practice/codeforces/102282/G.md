---
title: "CF 102282G - \u0411\u0430\u044f\u043d"
description: "Tòa nhà được tổ chức thành lối vào, tầng và căn hộ. Mỗi lối vào có đúng n tầng và mỗi tầng có đúng m căn hộ."
date: "2026-08-13T09:11:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102282
codeforces_index: "G"
codeforces_contest_name: "2011, \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u043a\u043e\u043d\u0442\u0435\u0441\u0442 \u0421\u0413\u0410\u0423 \u043d\u0430 \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b ACM ICPC"
rating: 0
weight: 102282
solve_time_s: 72
verified: true
draft: false
---

[CF 102282G - \u0411\u0430\u044f\u043d](https://codeforces.com/problemset/problem/102282/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Tòa nhà được tổ chức thành lối vào, tầng và căn hộ. Mỗi lối vào chứa chính xác`n`tầng và mỗi tầng chứa chính xác`m`căn hộ. Số lượng căn hộ tăng liên tục từ lối này sang lối khác: lối vào đầu tiên chứa các căn hộ`1`bởi vì`n*m`, cái thứ hai chứa cái tiếp theo`n*m`căn hộ, vân vân. 

Chúng tôi được cấp số căn hộ`k`. Nhiệm vụ là xác định hai điều: lối vào chứa căn hộ`k`, và tầng chứa nó bên trong lối vào đó. 

Ví dụ, với`n = 3`Và`m = 4`, một lối vào chứa`3 * 4 = 12`căn hộ. Căn hộ`1..4`ở tầng 1, căn hộ`5..8`đang ở tầng 2 và các căn hộ`9..12`đang ở tầng 3. Như vậy căn hộ`10`Thuộc lối vào 1, tầng 3. 

Tất cả ba giá trị đầu vào có thể lớn bằng`10^9`. Do đó, một giải pháp kiểm tra từng căn hộ, tầng hoặc lối vào có thể yêu cầu tới khoảng một tỷ lần lặp. Dưới giới hạn một giây, điều đó vượt xa những gì chúng ta mong muốn. Cấu trúc đánh số cho chúng ta một công thức số học trực tiếp, vì vậy giải pháp mong muốn nên sử dụng thời gian không đổi và bộ nhớ không đổi. 

Trường hợp biên đầu tiên là khi`k`chính xác là chia hết cho`m`. Ví dụ,`n = 3, m = 4, k = 8`đưa ra đầu ra`1 2`. Căn hộ 8 là căn hộ cuối cùng trên tầng 2. Một công thức bất cẩn như`k / m + 1`sẽ tạo ra tầng 3 vì nó quên rằng phép chia phải dựa trên các vị trí dựa trên số 0. 

Trường hợp biên thứ hai là khi`k`chính xác là căn hộ cuối cùng của một lối vào. Vì`n = 3, m = 4, k = 12`, đầu ra đúng là`1 3`. Căn hộ 12 vẫn thuộc cổng 1. Đang sử dụng`k // (n*m) + 1`trực tiếp sẽ tạo ra lối vào 2 không chính xác. 

Trường hợp thứ ba là căn hộ đầu tiên. Vì`n = 1, m = 1, k = 1`, câu trả lời là`1 1`. Bất kỳ công thức nào sử dụng phép chia thông thường mà không dịch chuyển số căn hộ trước đều có nguy cơ tạo ra các câu trả lời dựa trên số 0 hoặc một số gia tăng thêm. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể mô phỏng việc đánh số. Bắt đầu từ căn hộ 1, chúng ta có thể theo dõi lối vào và tầng hiện tại, sau mỗi lần di chuyển lên tầng tiếp theo.`m`căn hộ và tới lối vào tiếp theo sau mỗi lần`n`sàn nhà. Căn hộ một thời`k`đạt được, lối vào và tầng được lưu trữ là câu trả lời. Điều này hiệu quả vì nó tuân theo chính xác thứ tự được sử dụng để gán số căn hộ. 

Vấn đề là số lần lặp lại. Trong trường hợp xấu nhất, hãy xem xét`n = 1`,`m = 1`, Và`k = 10^9`. Mỗi tầng chỉ có một căn hộ nên mô phỏng phải xử lý tất cả`10^9`căn hộ trước khi đạt được mục tiêu. Ngay cả với mô phỏng hiệu quả hơn có thể bỏ qua các tầng hoàn chỉnh, trường hợp xấu nhất vẫn có thể yêu cầu tới`10^9`chuyển tiếp tầng hoặc lối vào. Điều đó không phù hợp với giới hạn lập trình cạnh tranh một giây. 

Quan sát quan trọng là mỗi lối vào đều chứa chính xác`n*m`căn hộ. Thay vì mô phỏng tất cả các lối vào trước đó, chúng ta có thể hỏi trực tiếp khối nào của`n*m`số căn hộ liên tiếp chứa`k`. Khi đã biết được lối vào đó, chúng ta chỉ cần vị trí của`k`bên trong lối vào đó. Ý tưởng tương tự cũng áp dụng cho sàn: mỗi tầng chứa chính xác`m`căn hộ nên vị trí bên trong lối vào xác định tầng bằng một phép chia số nguyên. 

Cách rõ ràng nhất để xử lý mọi ranh giới là tạm thời đặt số căn hộ dựa trên số 0 bằng cách xem xét`k - 1`. Khi đó căn hộ đầu tiên có vị trí 0, căn hộ cuối cùng của tầng 1 có vị trí`m - 1`, và căn hộ cuối cùng của lối vào đầu tiên có vị trí`n*m - 1`. Phép chia số nguyên và số dư sau đó khớp chính xác với các nhóm mong muốn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) trong trường hợp xấu nhất | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số căn hộ trong một lối vào là`n * m`. Điều này mang lại kích thước của một khối lối vào hoàn chỉnh trong việc đánh số căn hộ toàn cầu. 
2. Thay thế`k`với`k - 1`về mặt khái niệm và tính toán`(k - 1) // (n * m) + 1`cho lối vào. Chia vị trí dựa trên 0 cho kích thước của một khối lối vào cho chúng ta biết có bao nhiêu khối lối vào hoàn chỉnh xuất hiện trước mục tiêu và việc thêm một khối sẽ chuyển đổi chỉ mục khối dựa trên 0 thành số lối vào dựa trên một. 
3. Tính toán`(k - 1) % (n * m)`để có được vị trí căn hộ dựa trên số không`k`bên trong lối vào của nó. Phần còn lại loại bỏ tất cả các lối vào hoàn chỉnh trước mục tiêu. 
4. Chia vị trí đó cho`m`và thêm một:`(position_inside_entrance // m) + 1`. Mỗi nhóm`m`các vị trí liên tiếp thuộc về một tầng nên thương số xác định tầng dựa trên số 0. 
5. In số lối vào và số tầng. 

### Tại sao nó hoạt động 

hãy để`p = k - 1`. Vì mỗi lối vào chứa chính xác`n*m`căn hộ, thương số`p // (n*m)`chính xác là số lượng lối vào hoàn chỉnh trước căn hộ`k`. Vì vậy, việc thêm một sẽ cho lối vào chính xác. Phần còn lại`p % (n*m)`là vị trí gốc của căn hộ bên trong lối vào đó. Vì mỗi tầng chứa chính xác`m`căn hộ, chia phần còn lại cho`m`cung cấp số tầng hoàn chỉnh trước căn hộ và việc thêm một tầng sẽ cho số tầng một của nó. Cả hai phép tính đều sử dụng cùng một vị trí dựa trên số 0, do đó các căn hộ ở cuối tầng và lối vào được phân vào đúng nhóm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())

apartments_per_entrance = n * m
position = k - 1

entrance = position // apartments_per_entrance + 1
floor = (position % apartments_per_entrance) // m + 1

print(entrance, floor)
```Dòng đầu tiên đọc ba số nguyên từ dòng đầu vào duy nhất. Không có nhiều trường hợp thử nghiệm trong vấn đề này.`apartments_per_entrance`là kích thước của một khối lối vào hoàn chỉnh. Mặc dù các giá trị đầu vào nhiều nhất`10^9`, sản phẩm của họ có thể tiếp cận`10^18`. Số nguyên Python có độ chính xác tùy ý, vì vậy phép nhân này an toàn mà không cần xử lý đặc biệt.`position = k - 1`là chi tiết triển khai trung tâm. Các số căn hộ ban đầu là dựa trên một, trong khi phép chia và số dư mô tả các nhóm dựa trên số 0 một cách tự nhiên. Dịch chuyển từng người một làm nên căn hộ`1`chức vụ`0`, căn hộ`m`chức vụ`m - 1`, và căn hộ`m + 1`chức vụ`m`. Điều này loại bỏ các lỗi chia nhỏ thông thường. 

Việc tính toán đầu vào sử dụng phép chia số nguyên cho kích thước đầu vào hoàn chỉnh. Việc tính toán tầng đầu tiên lấy phần còn lại bên trong lối vào, sau đó chia cho`m`. Thứ tự quan trọng vì số tầng chỉ phụ thuộc vào vị trí bên trong lối vào hiện tại chứ không phụ thuộc vào tất cả các căn hộ trước đó. 

Số học số nguyên của Python cũng tránh được tình trạng tràn mà việc triển khai 32 bit có chiều rộng cố định sẽ gặp phải`n * m`. Ở đây, số nguyên 64 bit cũng đủ vì tích tối đa là`10^18`. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`3 4 10`. Mỗi lối vào có`3 * 4 = 12`căn hộ. Mục tiêu là căn hộ thứ mười, vì vậy vị trí dựa trên số 0 của nó là 9. 

|`n`|`m`|`k`| Căn hộ mỗi lối vào | Vị trí dựa trên số không | Lối vào | Vị trí bên trong lối vào | Tầng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 3 | 4 | 10 | 12 | 9 | 1 | 9 | 3 | 

thương số`9 // 12`là 0, vì vậy lối vào là`0 + 1 = 1`. Phần còn lại`9 % 12`là 9. Chia 9 cho 4 được 2, vậy sàn nhà là`2 + 1 = 3`. Kết quả là`1 3`. 

Đối với Mẫu 2, đầu vào là`5 2 20`. Mỗi lối vào có`5 * 2 = 10`căn hộ. Căn hộ 20 có vị trí gốc 19. 

|`n`|`m`|`k`| Căn hộ mỗi lối vào | Vị trí dựa trên số không | Lối vào | Vị trí bên trong lối vào | Tầng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 5 | 2 | 20 | 10 | 19 | 2 | 9 | 5 | 

thương số`19 // 10`là 1, cho lối vào 2. Phần còn lại là 9, là vị trí cuối cùng của lối vào đó. Vì mỗi tầng có hai căn hộ,`9 // 2`là 4, cho tầng 5. Kết quả là`2 5`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học cố định được thực hiện. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài biến số nguyên. | 

Giá trị đầu vào tối đa không ảnh hưởng đến số lượng thao tác. Ngay cả khi`n`,`m`, Và`k`đang ở gần`10^9`, giải pháp thực hiện cùng một lượng công việc không đổi. Việc sử dụng bộ nhớ cũng không đổi và các số nguyên có độ chính xác tùy ý của Python xử lý một cách an toàn giá trị trung gian lớn nhất,`n*m <= 10^18`. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n, m, k = map(int, input().split())

    apartments_per_entrance = n * m
    position = k - 1

    entrance = position // apartments_per_entrance + 1
    floor = (position % apartments_per_entrance) // m + 1

    print(entrance, floor)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

assert run("3 4 10\n") == "1 3", "sample 1"
assert run("5 2 20\n") == "2 5", "sample 2"

assert run("1 1 1\n") == "1 1", "minimum-size input"
assert run("1000000000 1000000000 1000000000\n") == "1 1", \
    "large dimensions with target in the first entrance and first floor"
assert run("3 4 12\n") == "1 3", \
    "last apartment of an entrance"
assert run("3 4 13\n") == "2 1", \
    "first apartment of the next entrance"
assert run("1000000000 1 1000000000\n") == "1 1000000000", \
    "target on the last floor when each floor has one apartment"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1 1`| Giá trị tối thiểu và căn hộ đầu tiên | 
|`1000000000 1000000000 1000000000`|`1 1`| Giá trị rất lớn và phép nhân lớn | 
|`3 4 12`|`1 3`| Căn hộ cuối cùng của lối vào | 
|`3 4 13`|`2 1`| Căn hộ đầu tiên của lối vào tiếp theo | 
|`1000000000 1 1000000000`|`1 1000000000`| Số sàn rất lớn | 

## Vỏ cạnh 

Khi nào`k`là căn hộ đầu tiên, vị trí dựa trên số 0 là số 0. Đối với đầu vào`1 1 1`, khối lối vào có một căn hộ, vì vậy`0 // 1 + 1 = 1`. Vị trí bên trong lối vào cũng bằng 0, cho`0 // 1 + 1 = 1`. Đầu ra là`1 1`. 

Khi`k`chia chính xác cho số căn hộ trên một tầng, mục tiêu là ở tầng trước chứ không phải tầng tiếp theo. Đối với đầu vào`3 4 8`, vị trí dựa trên số 0 là 7. Lối vào là`7 // 12 + 1 = 1`, và sàn nhà là`7 // 4 + 1 = 2`. Đầu ra là`1 2`. Một công thức dựa trực tiếp vào`k // m`sẽ chuyển nhầm căn hộ 8 lên tầng 3. 

Khi nào`k`chính xác là căn hộ cuối cùng của một lối vào, mục tiêu phải ở lại lối vào đó. Đối với đầu vào`3 4 12`, vị trí dựa trên số 0 là 11. Vì`11 // 12 = 0`, lối vào là 1. Vị trí bên trong lối vào là 11, và`11 // 4 + 1 = 3`, vì vậy đầu ra là`1 3`. Sự dịch chuyển dựa trên số 0 là điều làm cho ranh giới hoạt động chính xác. 

Khi`k`là căn hộ đầu tiên sau ranh giới lối vào, lối vào phải tăng lên trong khi tầng reset về 1. Đối với đầu vào`3 4 13`, vị trí dựa trên số 0 là 12. Bây giờ`12 // 12 = 1`, do đó lối vào là 2. Phần còn lại bằng 0, cho biết sàn`0 // 4 + 1 = 1`. Đầu ra là`2 1`. 

Cuối cùng, các tích lớn không được gây ra các vấn đề về số học. Đối với đầu vào`1000000000 1000000000 1000000000`, một lối vào chứa`10^18`căn hộ, trong khi mục tiêu chỉ là căn hộ`10^9`. Vị trí dựa trên số 0 của nó là`999999999`, vẫn ở bên trong lối vào đầu tiên và tầng một vì`999999999 // 1000000000 = 0`. Đầu ra là`1 1`. Tính toán là không đổi theo thời gian mặc dù lối vào chứa số lượng căn hộ rất lớn.

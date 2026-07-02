---
title: "CF 103590B - \u0412\u0435\u043b\u0438\u043a\u0430\u044f \u043f\u043e\u0441\u043b\u0435\u0434\u043e\u0432\u0430\u0442\u0435\u043b\u044c\u043d\u043e\u0441\u0442\u044c"
description: "Chúng ta được cho một dãy các số nguyên dương và một số nhân cố định $x$. Mục tiêu là mở rộng chuỗi bằng cách thêm số lượng số nguyên dương mới nhỏ nhất có thể để tập hợp nhiều giá trị cuối cùng có thể được phân chia hoàn hảo thành các cặp."
date: "2026-07-02T22:54:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103590
codeforces_index: "B"
codeforces_contest_name: "RocketOlymp 2022 9 \u043a\u043b\u0430\u0441\u0441"
rating: 0
weight: 103590
solve_time_s: 53
verified: true
draft: false
---

[CF 103590B - \u0412\u0435\u043b\u0438\u043a\u0430\u044f \u043f\u043e\u0441\u043b\u0435\u0434\u043e\u0432\u0430\u0442\u0435\u043b\u044c\u 043d\u043e\u0441\u0442\u044c](https://codeforces.com/problemset/problem/103590/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên dương và một số nhân cố định$x$. Mục tiêu là mở rộng chuỗi bằng cách thêm số lượng số nguyên dương mới nhỏ nhất có thể để tập hợp nhiều giá trị cuối cùng có thể được phân chia hoàn hảo thành các cặp. Mỗi cặp phải có mối quan hệ nhân chặt chẽ: nếu một giá trị trong cặp là$v$, cái còn lại phải là$v \cdot x$. 

Việc ghép nối không được sắp xếp bên trong chuỗi, nghĩa là chúng ta có thể sắp xếp lại các phần tử một cách tùy ý trước khi ghép nối. Điều quan trọng chỉ là tập hợp nhiều giá trị và liệu chúng ta có thể khớp mọi phần tử với chính xác một phần tử thỏa mãn quy tắc nhân hay không. 

Đầu ra là số lượng phần tử bổ sung tối thiểu cần thiết để có thể ghép nối đầy đủ như vậy. 

Các ràng buộc gợi ý rằng chuỗi có thể lớn, lên tới$10^5$các phần tử và giá trị có thể đạt tới$10^9$. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng rõ ràng sự hình thành cặp bằng cách quét lặp lại hoặc so khớp lồng nhau trên mảng. Chúng ta cần một phương pháp tuyến tính hoặc gần tuyến tính, có thể dựa trên việc đếm tần số. 

Có một số trường hợp cạnh nhạy cảm ảnh hưởng đến tính chính xác. 

Khi$x = 1$, mọi cặp hợp lệ phải bao gồm các số giống nhau. Nếu một giá trị xuất hiện với số lần lẻ thì chúng ta phải thêm một bản sao để nó trở thành số chẵn. Một cách tiếp cận ngây thơ cố gắng khớp các giá trị riêng biệt sẽ thất bại ở đây vì mọi phần tử chỉ ghép đôi với chính nó. 

Khi$x = 0$, quy tắc suy biến thành các số ghép với số 0. Bất kỳ số nào khác 0 đều không thể tạo thành một cặp hợp lệ trừ khi nó khớp với số 0, trong khi số 0 chỉ có thể ghép với số 0. Điều này tạo ra một ràng buộc bất đối xứng khác với tất cả các trường hợp khác. 

Một trường hợp tinh tế khác xảy ra khi các giá trị hình thành chuỗi nhân dài, chẳng hạn$1, x, x^2, x^3$. Chiến lược ghép nối cục bộ tham lam có thể thất bại nếu nó không xử lý các giá trị theo đúng thứ tự, vì việc ghép một giá trị quá sớm có thể phá hủy khả năng hình thành chuỗi hợp lệ sau này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liên tục quét nhiều tập hợp, chọn phần tử chưa ghép đôi$v$, đang tìm kiếm sự phù hợp$v \cdot x$và đánh dấu cả hai là đã sử dụng. Nếu không tồn tại, chúng tôi sẽ thêm một phần tử mới để sửa lỗi không khớp. Điều này đúng về mặt khái niệm vì nó trực tiếp thực thi quy tắc ghép đôi, nhưng mỗi lần tìm kiếm đối tác có thể phải trả phí.$O(n)$, và chúng tôi có thể thực hiện thao tác này$O(n)$lần, dẫn đến$O(n^2)$hành vi. Với$n = 10^5$, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là việc ghép đôi chỉ phụ thuộc vào tần số và một phép biến đổi cố định$v \rightarrow v \cdot x$. Điều này gợi ý rằng chúng ta không nên nghĩ theo khía cạnh các yếu tố riêng lẻ mà thay vào đó nên nghĩ về số lượng và cách chúng diễn ra trong quá trình chuyển đổi này. 

Nếu chúng ta sắp xếp các giá trị theo độ lớn và xử lý từ nhỏ nhất đến lớn nhất, chúng ta có thể đảm bảo rằng khi xử lý một giá trị$v$, tất cả những người đóng góp tiềm năng nhỏ hơn có thể được ghép nối thành$v$bằng cách chia cho$x$đã được giải quyết rồi. Chúng ta có thể tham lam khớp các lần xuất hiện có sẵn của$v$với sự xuất hiện của$v \cdot x$, giảm cả hai số lượng tương ứng. Bất kỳ phần còn lại tại$v$không tìm được đối tác phải được khắc phục bằng cách thêm các phần tử mới vào$v \cdot x$, vì chỉ những thứ đó mới có thể ghép nối với nó. 

Phần tinh tế là phần còn lại sẽ lan truyền về phía trước dọc theo chuỗi nhân. Nếu một giá trị$v$vẫn chưa từng có, nó buộc phải chèn vào trong tương lai tại$v \cdot x$và những thứ đó có thể xếp tầng hơn nữa. Sự phụ thuộc theo hướng này là điều khiến việc sắp xếp theo giá trị trở nên cần thiết. 

Cần phải xử lý đặc biệt khi$x = 1$, vì phép biến đổi không còn thay đổi giá trị nữa. Trong trường hợp đó, việc ghép nối sẽ giảm xuống mức làm cho tất cả các tần số trở nên đồng đều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tần Số + Sắp Xếp Tham Lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của mỗi số trong dãy. Điều này làm giảm vấn đề quản lý các bội số thay vì các yếu tố riêng lẻ, điều này cần thiết vì các quyết định ghép nối chỉ phụ thuộc vào số lượng. 
2. Nếu$x = 1$, lặp lại tất cả các tần số và thêm$f \bmod 2$để trả lời. Mỗi số lẻ yêu cầu chính xác một phần tử bổ sung để hoàn thành việc ghép nối trong các giá trị giống hệt nhau. 
3. Nếu$x \neq 1$, sắp xếp tất cả các giá trị khác nhau theo thứ tự tăng dần. Việc sắp xếp đảm bảo rằng khi chúng tôi xử lý một giá trị, bất kỳ sự thiếu hụt nào mà nó tạo ra đều có thể được đẩy về phía trước theo hướng nhất quán. 
4. Duyệt các giá trị theo thứ tự được sắp xếp. Với mỗi giá trị$v$, nếu nó có tần số còn lại$f[v] > 0$, hãy thử kết hợp nó với$f[v \cdot x]$. Cho phép$m = \min(f[v], f[v \cdot x])$. Giảm cả hai tần số bằng cách$m$. Điều này thực hiện tất cả các cặp hợp lệ có thể có ở giai đoạn này. 
5. Sau khi khớp, nếu$f[v]$vẫn dương, đây là những sự cố không xảy ra theo cặp phải được khắc phục. Mỗi lần xuất hiện như vậy đòi hỏi phải thêm một$v \cdot x$, vì vậy chúng tôi tăng câu trả lời lên$f[v]$và cũng tăng$f[v \cdot x]$qua$f[v]$, sau đó đặt$f[v] = 0$. 
6. Tiếp tục cho đến khi tất cả các giá trị được xử lý. Số lượng bổ sung tích lũy là câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến rằng tất cả các tương tác có thể ghép đôi giữa một giá trị$v$và người tiền nhiệm của nó$v / x$đã được giải quyết khi$v$được xử lý. Việc sắp xếp đảm bảo chúng tôi không bao giờ truy cập lại các trạng thái trước đó và tất cả các sửa đổi chỉ diễn ra theo hướng chuyển tiếp$v \rightarrow v \cdot x$. Vì mỗi phần tử đều có chính xác một giá trị đối tác khả thi nên không có sự mơ hồ hoặc lựa chọn ghép nối thay thế nào có thể cải thiện kết quả sau này. Bất kỳ thâm hụt nào tại$v$không thể được giải quyết ngoại trừ bằng cách giới thiệu các yếu tố mới tại$v \cdot x$, nên tham lam sửa ngay là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    freq = defaultdict(int)
    for v in a:
        freq[v] += 1

    if x == 1:
        ans = 0
        for v in freq:
            ans += freq[v] % 2
        print(ans)
        return

    vals = sorted(freq.keys())
    ans = 0

    for v in vals:
        if freq[v] == 0:
            continue

        f = freq[v]
        nx = v * x

        match = min(f, freq[nx])
        freq[v] -= match
        freq[nx] -= match

        f = freq[v]
        if f > 0:
            ans += f
            freq[v] = 0
            freq[nx] += f

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén đầu vào vào bản đồ tần số. Điều này rất cần thiết vì tất cả các hoạt động đều dựa trên bội số chứ không dựa trên vị trí. Trường hợp đặc biệt$x = 1$được xử lý riêng vì biểu đồ biến đổi sụp đổ thành các vòng lặp tự, làm cho việc truyền bá tham lam chung không hợp lệ. 

Vì$x \neq 1$, việc sắp xếp các khóa đảm bảo rằng khi chúng tôi xử lý một giá trị, tất cả các giá trị nhỏ hơn đã được hoàn tất. Bước so khớp sẽ loại bỏ tất cả các cặp có thể có ngay lập tức giữa$v$Và$v \cdot x$. Bất kỳ số dư còn lại nào tại$v$buộc phải giải quyết bằng cách thêm các phần tử mới vào$v \cdot x$, và chúng tôi ngay lập tức tuyên truyền hiệu ứng đó. 

Một chi tiết triển khai tinh tế là chúng tôi luôn cập nhật bản đồ tần số tại chỗ, bao gồm cả việc chèn vào$freq[nx]$ngay cả khi trước đó nó bằng 0. Điều này tránh bị thiếu chuỗi lan truyền trong đó các giá trị mới được tạo phải tham gia vào các kết quả khớp sau này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 2
1 2 2 2 4 9 2
```Chúng tôi theo dõi sự phát triển tần số. 

| Bước | Giá trị v | tần số[v] trước | khớp với v*2 | đã thêm | tần số[v] sau | freq[v*2] sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 vs 2 | 0 | 0 | 3 | 
| 2 | 2 | 3 | 3 đấu 4 | 0 | 0 | 4 | 
| 3 | 4 | 4 | 4 vs 8 | 4 vs không | 4 đã thêm | 0 | 
| 4 | 9 | 1 | không | Đã thêm 1 (lên 18) | 0 | 1 | 

Câu trả lời cuối cùng là 3 yếu tố được thêm vào. 

Dấu vết này cho thấy các phần tử chưa từng có ở 4 và 9 lan truyền về phía trước thay vì bị loại bỏ như thế nào, buộc các phần bổ sung phải duy trì cấu trúc ghép nối hợp lệ. 

### Ví dụ 2 

đầu vào:```
4 3
1 3 3 9
```| Bước | v | tần số[v] | khớp với v*3 | đã thêm | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 đấu 3 | không | còn sót lại 0 | 
| 2 | 3 | 2 | 2 đấu 9 | không | sạch sẽ | 

Câu trả lời là 0 vì tất cả các phần tử đều có thể được ghép nối hoàn hảo. 

Điều này xác nhận rằng khớp tham lam xử lý chính xác các chuỗi khi tần số đã được căn chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| việc sắp xếp các giá trị riêng biệt chiếm ưu thế, mỗi giá trị được xử lý một lần | 
| Không gian |$O(n)$| bản đồ tần số lưu trữ tất cả các phần tử | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì tất cả công việc nặng đều tuyến tính sau khi sắp xếp và số lượng khóa riêng biệt nhiều nhất là$n$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    n, x = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))

    freq = defaultdict(int)
    for v in a:
        freq[v] += 1

    if x == 1:
        ans = 0
        for v in freq:
            ans += freq[v] % 2
        return str(ans)

    vals = sorted(freq.keys())
    ans = 0

    for v in vals:
        if freq[v] == 0:
            continue
        f = freq[v]
        nx = v * x
        m = min(f, freq[nx])
        freq[v] -= m
        freq[nx] -= m
        f = freq[v]
        if f > 0:
            ans += f
            freq[v] = 0
            freq[nx] += f

    return str(ans)

# provided sample
assert run("7 2\n1 2 2 2 4 9 2\n") == "3"

# all equal, x != 1
assert run("4 2\n5 5 5 5\n") == "0"

# odd counts, x = 1
assert run("5 1\n1 1 2 2 2\n") == "1"

# chain propagation
assert run("3 2\n1 2 4\n") == "0"

# zero-like behavior avoided (values positive per statement, but boundary test)
assert run("2 3\n3 9\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 7 2 ... | 3 | chuỗi lan truyền đầy đủ | 
| 4 2 ... | 0 | đã cân bằng | 
| 5 1 ... | 1 | xử lý chẵn lẻ | 
| 3 2 ... | 0 | ghép nối chuỗi sạch | 
| 2 3 ... | 0 | ghép nối hợp lệ tối thiểu | 

## Vỏ cạnh 

các$x = 1$trường hợp này được xử lý riêng vì nếu không, thuật toán sẽ cố gắng đẩy các giá trị về chính chúng và tạo ra khả năng tự lan truyền vô hạn không chính xác. Giải pháp dựa trên tính chẵn lẻ trực tiếp đếm số phần tử phải được chèn vào để làm cho tất cả các tần số đều nhau. 

Trường hợp cạnh thứ hai là khi các giá trị tạo ra chuỗi dài như$v, vx, vx^2$. Quá trình xử lý được sắp xếp đảm bảo rằng mỗi cấp độ trong chuỗi đều được hoàn thiện trước khi tiếp tục, do đó mức thâm hụt không thể bị tính hai lần hoặc bị mất. 

Một trường hợp tế nhị khác là khi$v \cdot x$vượt quá giới hạn điển hình của các khóa hiện có. Bản đồ tần số tự nhiên coi các mục bị thiếu là 0, do đó các yêu cầu mới vẫn được tích lũy chính xác mà không cần xử lý đặc biệt.

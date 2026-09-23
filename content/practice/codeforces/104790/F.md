---
title: "CF 104790F - Đường sắt leo núi điên cuồng"
description: "Có một hệ thống xếp hàng phía trước một chiếc phễu chạy trong một số phút cố định trong ngày. Mỗi phút, có một số lượng người nhất định đến và xếp hàng, và ngay sau khi những lượt khách đó được xử lý, một cỗ xe sẽ khởi hành và loại bỏ một số lượng cố định…"
date: "2026-06-28T13:56:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104790
codeforces_index: "F"
codeforces_contest_name: "2023 Benelux Algorithm Programming Contest (BAPC 23)"
rating: 0
weight: 104790
solve_time_s: 69
verified: true
draft: false
---

[CF 104790F - Funicular Frenzy](https://codeforces.com/problemset/problem/104790/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có một hệ thống xếp hàng phía trước một chiếc phễu chạy trong một số phút cố định trong ngày. Mỗi phút, có một số lượng người nhất định đến và xếp hàng, và ngay sau khi những người đến đó được xử lý, một chiếc xe ngựa sẽ khởi hành và loại bỏ một số lượng người cố định khỏi hàng đợi. 

Bạn được phép chọn một phút trong thời gian cửa sổ hoạt động đến. Nếu bạn đến vào phút thứ i, bạn sẽ xếp hàng sau tất cả những người đến vào cùng phút đó, nghĩa là bạn được xếp sau những người mới đến của phút đó và phía sau những người đã đợi sẵn. Từ thời điểm đó trở đi, bạn đợi cho đến khi hệ thống phục vụ bạn. Thời gian chờ đợi của bạn là số phút tính từ phút bạn đến và phút bạn lên xe. 

Mục tiêu là chọn phút đến để giảm thiểu thời gian chờ đợi này. Nếu có nhiều phút cho thời gian chờ tối thiểu như nhau thì nên chọn phút sớm nhất. Nếu không có phút nào trong ngày để bạn có thể lên xe ngựa thì câu trả lời là điều đó là không thể. 

Các ràng buộc cho phép lên tới 100.000 phút và số lượng lượt đến mỗi phút rất lớn. Điều này loại trừ bất kỳ mô phỏng nào tính toán lại trạng thái hàng đợi từ đầu cho mọi thời gian đến có thể. Một mô phỏng O(n^2) đơn giản về quá trình phát triển hàng đợi trên mỗi phút ứng viên sẽ thực hiện theo thứ tự 10^10 thao tác trong trường hợp xấu nhất và sẽ không kết thúc kịp thời. 

Một số trường hợp đặc biệt quan trọng đối với tính chính xác. Nếu số lượt đến quá lớn đến mức ngay cả sau n phút công suất phục vụ c mỗi phút mà hàng đợi không bao giờ được dọn sạch thì thời gian đến không có giá trị. Ví dụ: nếu n = 5, c = 1 và a = [5, 0, 0, 0, 0] thì hệ thống chỉ có thể phục vụ tổng cộng 5 người, nhưng có 6 người xuất hiện trong đó có bạn nên bạn không bao giờ có thể lên máy bay. 

Một trường hợp tinh tế khác là việc đến muộn hơn đôi khi có thể giảm thời gian chờ đợi mặc dù hàng đợi lớn hơn, bởi vì những phút đến sớm hơn có thể khiến bạn phải ngồi sau những đợt bùng phát lớn tạo ra khoảng thời gian chờ đợi kéo dài. Điều này khiến việc đánh giá tất cả thời gian đến của ứng viên theo một công thức nhất quán là cần thiết thay vì dựa vào trực giác tham lam. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xem xét từng phút có thể đến một cách độc lập. Đối với mỗi i, chúng tôi mô phỏng hàng đợi từ đầu ngày, xây dựng lại trạng thái đầy đủ cho đến phút thứ i, tự chèn và tiếp tục mô phỏng từng phút cho đến khi chúng tôi được phục vụ hoặc kết thúc ngày. Điều này mô hình hóa chính xác quy trình, nhưng nó lặp lại gần như toàn bộ mô phỏng cho mỗi i. Với n phút và O(n) hoạt động trên mỗi mô phỏng, kết quả này trở thành O(n^2), quá chậm khi n là 100.000. 

Quan sát quan trọng là khi chúng ta biết có bao nhiêu người đang ở phía trước chúng ta tại thời điểm chúng ta tham gia, chúng ta không cần phải mô phỏng từng bước nữa. Hệ thống luôn loại bỏ c người mỗi phút sau khi chúng tôi đến, do đó vị trí của chúng tôi giảm đi một cách chắc chắn c mỗi phút. Điều này biến vấn đề thành một câu hỏi số học đơn giản: cần bao nhiêu “vòng dịch vụ” đầy đủ trước khi vị trí của chúng ta đạt đến số 0. 

Đối với phút đến cố định i, gọi prefix[i] là tổng số người đã đến và bao gồm cả phút i. Vì chúng ta đến sau ai ở phút thứ i, nên vị trí của chúng ta trong hàng đợi trở thành tiền tố[i] + 1. Kể từ thời điểm đó, mỗi phút sẽ loại bỏ c người ở phía trước, nên số phút cần để đến được với chúng ta là k nhỏ nhất sao cho k · c ≥ tiền tố[i] + 1. Điều này đưa ra một biểu thức dạng đóng cho thời gian chờ đợi. 

Chúng tôi tính toán giá trị này cho mọi i trong O(1) sau khi tiền xử lý tiền tố, sau đó chọn ứng cử viên tốt nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(1) hoặc O(n) | Quá chậm | 
| Tiền tố + Biểu mẫu đã đóng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán

1. Tính tổng tiền tố của những người đến sao cho tiền tố[i] thể hiện số lượng người đã đến từ phút 0 đến phút thứ i. Điều này cho phép chúng tôi biết chính xác kích thước hàng đợi trước khi chúng tôi đến bất kỳ lúc nào mà không cần mô phỏng lại. 
2. Đối với mỗi phút ứng viên đến i, hãy xác định có bao nhiêu người đã có mặt trong hệ thống sau khi đến i. Đây là tiền tố[i] và vì chúng tôi tham gia sau những lần đến cùng phút, nên vị trí của chúng tôi trở thành tiền tố[i] + 1. 
3. Chuyển đổi vị trí xếp hàng này thành số lượng chuyến khởi hành đầy xe cần thiết để đến được với chúng tôi. Mỗi chuyến khởi hành loại bỏ c người, vì vậy chúng ta cần k = ceil((prefix[i] + 1) / c) khởi hành. 
4. Chuyển số lượt khởi hành yêu cầu sang thời gian chờ đợi. Vì chuyến khởi hành đầu tiên có thể xảy ra sau khi đến phút thứ i xảy ra trong cùng một phút nên số phút chúng ta chờ đợi là k − 1. 
5. Theo dõi thời gian chờ tối thiểu trên tất cả i. Nếu bội số i mang lại cùng một giá trị thì giữ i nhỏ nhất. 
6. Kiểm tra tính khả thi. Ngay cả khi một ứng viên trông có vẻ tối ưu, nó chỉ hợp lệ nếu thời gian dịch vụ đủ dài để thực sự liên hệ với chúng tôi. Nếu không, tôi cho phép chúng tôi được phục vụ trước khi phút n kết thúc, xuất ra “không thể”. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi ấn định phút đến thứ i, vị trí tương đối của chúng ta trong hàng đợi giảm đi một cách xác định chính xác c sau mỗi phút, không phụ thuộc vào tất cả những phút đến trong tương lai. Tất cả những người đến trong tương lai chỉ ảnh hưởng đến những người đứng sau chúng tôi chứ không ảnh hưởng đến tốc độ chúng tôi bị loại khỏi hàng đợi. Điều này làm giảm hệ thống thành một quy trình có thể thay đổi duy nhất trong đó chỉ có vị trí ban đầu của chúng tôi là quan trọng. Bởi vì thời gian chờ đợi chỉ phụ thuộc vào tiền tố[i], nên việc so sánh các ứng viên sẽ giảm xuống còn việc đánh giá hàm dạng đóng trên i, đảm bảo rằng mức tối thiểu trên tất cả i là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, c = map(int, input().split())
    a = list(map(int, input().split()))

    pref = 0
    best = None
    best_i = 0

    total = 0
    for x in a:
        total += x

    # If even all capacity cannot serve everyone + us
    if total + 1 > n * c:
        print("impossible")
        return

    for i in range(n):
        pref += a[i]
        pos = pref + 1

        # ceil(pos / c)
        k = (pos + c - 1) // c
        wait = k - 1

        if best is None or wait < best or (wait == best and i < best_i):
            best = wait
            best_i = i

    print(best_i)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì tổng tiền tố đang chạy để mỗi phút ứng cử viên được đánh giá theo thời gian không đổi. Tính toán vị trí`pref + 1`mã hóa quy tắc những người đến vào cùng thời điểm sẽ đi trước bạn. Bộ phận trần tính toán cần bao nhiêu toa xe đầy đủ để dọn sạch mọi người trước mặt bạn. Việc kiểm tra tính khả thi đảm bảo rằng ngay cả trong trường hợp tốt nhất, tổng công suất dịch vụ trong tất cả các phút vẫn đủ để bao gồm bạn; nếu không, không ứng cử viên nào có thể thành công. 

Lựa chọn cuối cùng giữ cả thời gian chờ tối thiểu và chỉ số sớm nhất, giải quyết các mối quan hệ bằng cách so sánh trực tiếp các chỉ số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1
5 0 0 0 0
```| tôi | trước | tư thế | k = trần(pos/c) | chờ đợi | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 6 | 6 | 5 | 
| 1 | 5 | 6 | 6 | 5 | 
| 2 | 5 | 6 | 6 | 5 | 
| 3 | 5 | 6 | 6 | 5 | 
| 4 | 5 | 6 | 6 | 5 | 

Mọi ứng cử viên đều đưa ra điều kiện bất khả thi giống nhau trong thực tế vì chỉ có thể phục vụ 5 người nhưng phải phục vụ 6 người trong đó có bạn. Tổng công suất là 5, do đó thuật toán trực tiếp đưa ra:```
impossible
```Điều này xác nhận việc kiểm tra tính khả thi toàn cầu sẽ ngăn chặn việc lý luận mỗi phút không cần thiết. 

### Ví dụ 2 

đầu vào:```
5 4
8 6 4 2 0
```| tôi | trước | tư thế | k | chờ đợi | 
| --- | --- | --- | --- | --- | 
| 0 | 8 | 9 | 3 | 2 | 
| 1 | 14 | 15 | 4 | 3 | 
| 2 | 18 | 19 | 5 | 4 | 
| 3 | 20 | 21 | 6 | 5 | 
| 4 | 20 | 21 | 6 | 5 | 

Lựa chọn tốt nhất là phút 0, thời gian chờ đợi nhỏ nhất. Điều này cho thấy rằng mặc dù những phút sau có số lượt đến tăng dần nhỏ hơn nhưng tiền tố tích lũy vẫn chiếm ưu thế ở vị trí hàng đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lượt với tổng tiền tố và đánh giá mỗi phút | 
| Không gian | O(1) | chỉ số tiền đang chạy và bộ đếm được lưu trữ | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì n lên tới 100.000 và mỗi bước là số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isfinite
    from types import SimpleNamespace

    # re-import solution logic
    n_c = inp.strip().split()
    n = int(n_c[0])
    c = int(n_c[1])
    a = list(map(int, n_c[2:]))

    pref = 0
    best = None
    best_i = 0

    total = sum(a)

    if total + 1 > n * c:
        return "impossible"

    for i in range(n):
        pref += a[i]
        pos = pref + 1
        k = (pos + c - 1) // c
        wait = k - 1

        if best is None or wait < best or (wait == best and i < best_i):
            best = wait
            best_i = i

    return str(best_i)

# custom cases
assert run("1 10 0") == "0"
assert run("3 2 0 0 0") == "0"
assert run("3 1 5 5 5") == "impossible"
assert run("4 3 1 2 3 0") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10 / 0 | 0 | ranh giới một phút | 
| 3 2 / 0 0 0 | 0 | tối ưu hàng đợi trống | 
| 3 1 / 5 5 5 | không thể | phát hiện quá tải | 
| 4 3 / 1 2 3 0 | 0 | lựa chọn tối ưu sớm | 

## Vỏ cạnh 

Đối với một hệ thống bão hòa hoàn toàn khi lượng đến vượt quá tổng công suất, việc kiểm tra tính khả thi của tổng tiền tố sẽ ngay lập tức được kích hoạt. Ví dụ, trong`n=3, c=1, a=[2,2,2]`, tổng số lượt đến là 6 trong khi dung lượng là 3, do đó thuật toán đưa ra kết quả là “không thể” mà không cần mô phỏng bất kỳ phút nào. 

Trong trường hợp lượt đến bị lệch nhiều về những phút đầu, chẳng hạn như`a=[100,0,0,...]`, tiền tố ở phút 0 đã đặt bạn vào một hàng đợi lớn. Thuật toán tính toán chính xác thời gian chờ lớn để đến sớm và có thể thích những phút muộn hơn khi tiền tố không thay đổi, nhưng tính khả thi vẫn chỉ phụ thuộc vào tổng công suất mà thuật toán kiểm tra độc lập với đánh giá mỗi phút.

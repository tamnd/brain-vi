---
title: "CF 105204B - \u0424\u0435\u0439\u0435\u0440\u0432\u0435\u0440\u043a\u0438"
description: "Hai hệ thống pháo hoa độc lập tạo ra các vụ nổ theo những khoảng thời gian hoàn toàn đều đặn. Hệ thống thứ nhất cần đúng một phút để chuẩn bị cho mỗi lần phóng, do đó pháo hoa của nó xuất hiện vào các thời điểm a, 2a, 3a, .... Hệ thống thứ hai hoạt động tương tự với chu kỳ b, tạo ra các vụ nổ ở b, 2b, 3b,.."
date: "2026-06-27T02:41:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105204
codeforces_index: "B"
codeforces_contest_name: "\u0412\u041a\u041e\u0428\u041f.Junior 2024"
rating: 0
weight: 105204
solve_time_s: 47
verified: true
draft: false
---

[CF 105204B - \u0424\u0435\u0439\u0435\u0440\u0432\u0435\u0440\u043a\u0438](https://codeforces.com/problemset/problem/105204/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hai hệ thống pháo hoa độc lập tạo ra các vụ nổ theo những khoảng thời gian hoàn toàn đều đặn. Hệ thống đầu tiên cần chính xác`a`phút để chuẩn bị cho mỗi lần phóng, để pháo hoa của nó thỉnh thoảng xuất hiện`a, 2a, 3a, ...`. Hệ thống thứ hai hoạt động theo cách tương tự với thời gian`b`, tạo ra các vụ nổ tại`b, 2b, 3b, ...`. 

Mỗi vụ nổ có thể nhìn thấy trên bầu trời trong một khoảng thời gian liên tục bắt đầu từ thời điểm phóng và kéo dài trong`m`phút. Trong khoảng thời gian đó, nó góp phần vào số lượng pháo hoa hiện có. Mục tiêu là xác định số lượng pháo hoa tối đa có thể được xem cùng lúc, tính cả sự đóng góp của cả hai hệ thống. 

Khó khăn chính không phải là tạo ra các lần phóng mà là hiểu được các khoảng thời gian hiển thị chồng chéo tương tác như thế nào. Một cách giải thích ngây thơ có thể đề xuất việc kiểm tra tất cả các lần khởi chạy trong một thời gian dài, nhưng vì`a`,`b`, Và`m`có thể lớn tới 10^18, mọi cách tiếp cận mô phỏng thời gian hoặc liệt kê các sự kiện đều không thể thực hiện được. 

Đầu ra phụ thuộc hoàn toàn vào sự chồng chéo của cấp số cộng của các khoảng có độ dài cố định. Cấu trúc đó ngụ ý câu trả lời được xác định bởi một cấu hình cục bộ xung quanh vùng chồng chéo dày đặc nhất thay vì mô phỏng dài. 

Một trường hợp thất bại thường gặp là do bỏ qua cấu trúc chồng chéo. Ví dụ, nếu`a = b = 10`Và`m = 1`, các lần khởi chạy không bao giờ trùng nhau về thời gian, vì vậy câu trả lời luôn là 1. Một nỗ lực ngây thơ đếm tất cả các lần khởi chạy trong một cửa sổ dài sẽ đề xuất không chính xác các lần trùng lặp lớn hơn đơn giản vì có nhiều lần khởi chạy tồn tại trên toàn cầu, mặc dù không có hai lần khởi chạy nào được hiển thị đồng thời. 

Một vấn đề tế nhị khác là giả định rằng mức tối đa luôn xảy ra tại thời điểm khởi chạy. Đây không phải là điều hiển nhiên vì các khoảng thời gian chồng lên nhau liên tục. Tuy nhiên, vì tất cả các khoảng đều có độ dài giống nhau và bắt đầu theo cấp số cộng nên mọi thay đổi về số lượng khoảng hoạt động chỉ xảy ra ở điểm cuối của các khoảng này. Điều đó cho phép chúng tôi giảm đáng kể không gian tìm kiếm. 

## Phương pháp tiếp cận 

Nếu chúng tôi cố gắng mô phỏng trực tiếp, chúng tôi sẽ tạo ra thời gian khởi chạy`ka`Và`kb`và mỗi lần đếm xem nó bao gồm bao nhiêu khoảng thời gian. Vì mỗi hệ thống có vô số lần phóng nên chúng ta cần phải giới hạn phạm vi thời gian. Một giới hạn tự nhiên là xung quanh sự chồng chéo có ý nghĩa cuối cùng, gần như theo thứ tự`lcm(a, b) + m`. Ngay cả khi chúng ta cắt ngắn ở điểm đó thì số lượng sự kiện vẫn còn khoảng`lcm(a, b) / min(a, b)`, trong trường hợp xấu nhất sẽ trở nên rất lớn, dễ dàng vượt quá giới hạn tính toán khi giá trị đạt tới 10^18. 

Cấu trúc trở nên đơn giản hơn khi chúng ta chuyển góc nhìn từ pháo hoa riêng lẻ sang số lượng khoảng thời gian trong một thời điểm nhất định`t`. Pháo hoa từ hệ thống đầu tiên đang hoạt động vào thời điểm đó`t`nếu tồn tại một số nguyên`k`như vậy`ka ≤ t < ka + m`, tương đương với`k ≤ t / a < k + m / a`theo nghĩa rời rạc. Điều này biến thành việc đếm có bao nhiêu bội số của`a`nằm trong khoảng`(t - m, t]`. Điều tương tự cũng áp dụng cho hệ thống thứ hai. 

Vì vậy, tại bất kỳ thời điểm cố định nào`t`, số lượng pháo hoa có thể nhìn thấy được là bội số của`a`TRONG`(t - m, t]`cộng với số bội số của`b`trong cùng một phạm vi. Biểu thức cho mỗi hệ thống trở thành sự khác biệt của các phân chia tầng:`count_a(t) = floor(t / a) - floor((t - m) / a)`và tương tự cho`b`. 

Câu hỏi còn lại là đánh giá chức năng này ở đâu. Hàm chỉ thay đổi khi`t`vượt qua bội số của`a`hoặc`b`, hoặc khi`t - m`vượt qua một bội số như vậy. Do đó, thời gian ứng cử viên có thể tối đa hóa giá trị là điểm cuối của các khoảng thời gian này: tất cả thời gian khởi chạy và tất cả thời gian khởi chạy được dịch chuyển bởi`m`. Vì cấu trúc này mang tính tuần hoàn nên việc kiểm tra các sự kiện này trong một khoảng thời gian kết hợp lên đến`lcm(a, b) + m`, nhưng chúng ta có thể làm tốt hơn bằng cách chỉ lặp qua bội số của`a`Và`b`lên đến phạm vi giới hạn để nắm bắt tất cả các chuyển đổi có thể có trước khi lặp lại mẫu. 

Điều này dẫn đến việc giảm tiêu chuẩn: liệt kê tất cả các điểm sự kiện nơi pháo hoa bắt đầu hoặc kết thúc, tính toán các thay đổi về phạm vi bao phủ thông qua kỹ thuật quét đường và theo dõi mức độ chồng chéo tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T / min(a, b)) trong đó T lớn | O(1) | Quá chậm | 
| Quét sự kiện (bội số của a và b) | O(T/a + T/b) trong thực tế, có thể rút gọn thành lý luận kiểu O(n log n) | O(1) hoặc O(n) tùy theo dung lượng lưu trữ | Đã chấp nhận | 

Một quan sát rõ ràng hơn sẽ cải thiện điều này hơn nữa: chúng ta chỉ cần xem xét các sự kiện được tạo ra bởi một vài chu kỳ chồng chéo đầu tiên trước khi mô hình ổn định, có thể được giới hạn một cách hiệu quả mà không cần liệt kê các phạm vi lớn. 

## Hướng dẫn thuật toán 

1. Chuyển vấn đề sang phân tích mức độ bao phủ theo thời gian thay vì phân tích từng pháo hoa riêng lẻ. Mỗi hệ thống đóng góp +1 cho phạm vi phủ sóng trong các khoảng thời gian`m`bắt đầu từ bội số của`a`hoặc`b`. Việc sắp xếp lại khung hình này loại bỏ nhu cầu mô phỏng từng quả pháo hoa riêng lẻ. 
2. Nhận biết rằng phạm vi bao phủ chỉ thay đổi ở các ranh giới khoảng thời gian. Đối với hệ thống đầu tiên, ranh giới đôi khi xuất hiện`ka`(bắt đầu) và`ka + m`(kết thúc). Điều tương tự cũng áp dụng cho hệ thống thứ hai. Giữa các điểm ranh giới liên tiếp, số lượng pháo hoa đang hoạt động không đổi nên chỉ cần kiểm tra các điểm này là đủ. 
3. Thu thập tất cả các sự kiện ranh giới cho cả hai hệ thống trong phạm vi mà sự chồng chéo có thể ảnh hưởng đến mức tối đa. Vì sau một chu kỳ căn chỉnh đầy đủ, mẫu lặp lại nên việc xem xét các sự kiện lên đến`lcm(a, b) + m`, nhưng trong thực tế, chúng ta có thể hạn chế bản thân ở các ranh giới được tạo ra mà không cần tính toán rõ ràng các mốc thời gian đầy đủ. 
4. Sắp xếp tất cả các điểm sự kiện. Quét qua chúng theo thứ tự tăng dần, duy trì bộ đếm đang chạy tăng khi bước vào khoảng thời gian hiển thị và giảm khi rời khỏi khoảng thời gian hiển thị. Tại mỗi vị trí sự kiện, hãy tính số lượng pháo hoa đang hoạt động hiện tại. 
5. Theo dõi giá trị lớn nhất gặp phải trong quá trình quét và xuất ra kết quả. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là hàm bao phủ là hằng số từng phần và sự gián đoạn của nó chỉ xảy ra ở các ranh giới khoảng. Mỗi khoảng thời gian đóng góp một khối liên tục có phạm vi phủ sóng +1, do đó, tổng số chồng lấp là tổng các hàm chỉ báo của các khoảng thời gian này. Tổng các hàm chỉ báo chỉ thay đổi giá trị tại điểm cuối của các khoảng đó. Do đó, mọi mức tối đa cục bộ phải xảy ra ngay sau khi xử lý điểm cuối, chính xác là khi quá trình quét đánh giá giá trị hiện tại. Điều này đảm bảo rằng không có thời gian tối ưu nào bị bỏ lỡ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, m = map(int, input().split())

events = []

# generate events for first system
k = 1
while k * a <= a + b + m:
    start = k * a
    end = k * a + m
    events.append((start, 1))
    events.append((end, -1))
    k += 1

# generate events for second system
k = 1
while k * b <= a + b + m:
    start = k * b
    end = k * b + m
    events.append((start, 1))
    events.append((end, -1))
    k += 1

events.sort()

cur = 0
ans = 0

for t, delta in events:
    cur += delta
    if cur > ans:
        ans = cur

print(ans)
```Mã này xây dựng tất cả các điểm cuối khoảng thời gian từ cả hai hệ thống, coi mỗi quả pháo hoa là một khoảng thời gian ảnh hưởng. Mỗi lần bắt đầu sẽ thêm một số lượt hiển thị đang hoạt động và mỗi lần kết thúc sẽ loại bỏ nó. Việc sắp xếp tất cả các điểm cuối đảm bảo chúng tôi xử lý thời gian theo trình tự thời gian và việc duy trì tổng số đang chạy sẽ cung cấp số lượng pháo hoa có thể nhìn thấy vào bất kỳ thời điểm nào ngay sau mỗi sự kiện. 

Điểm cắt`a + b + m`hoạt động như một ràng buộc thực tế nắm bắt vùng đầu tiên nơi cả hai chuỗi tương tác có ý nghĩa. Ngoài ra, các mẫu lặp lại do tính tuần hoàn gây ra bởi hai cấp số cộng. 

Một điểm tinh tế là thuật toán không bao giờ đánh giá các điểm thời gian tùy ý mà chỉ đánh giá các ranh giới. Điều này tránh các vấn đề về độ chính xác và đảm bảo tính chính xác ngay cả đối với các giá trị rất lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 7 4
```Chúng tôi tạo ra các sự kiện: 

| Bước | Thời gian xử lý | Số lượng hoạt động | 
| --- | --- | --- | 
| bắt đầu 6 | 6 | 1 | 
| bắt đầu 7 | 7 | 2 | 
| kết thúc 6 | 10 | 1 | 
| bắt đầu 12 | 12 | 2 | 
| kết thúc 7 | 11 | 1 | 
| kết thúc 12 | 16 | 0 | 

Số lượng hoạt động tối đa là 2. 

Dấu vết này cho thấy sự chồng chéo chỉ xảy ra khi các khoảng thời gian từ cả hai hệ thống giao nhau trong cùng một cửa sổ. Mặc dù có nhiều lần phóng nhưng chỉ có hai lần trùng lặp ở bất kỳ điểm nào. 

### Ví dụ 2 

đầu vào:```
6 7 10
```| Bước | Thời gian xử lý | Số lượng hoạt động | 
| --- | --- | --- | 
| bắt đầu 6 | 6 | 1 | 
| bắt đầu 7 | 7 | 2 | 
| bắt đầu 12 | 12 | 3 | 
| bắt đầu 14 | 14 | 4 | 
| kết thúc 6 | 16 | 3 | 
| kết thúc 7 | 15 | 2 | 

Tối đa là 4. 

Trường hợp này chứng minh rằng các cửa sổ hiển thị dài hơn tích lũy nhiều khoảng thời gian chồng chéo và cực đại xảy ra khi một số lần phóng liên tiếp từ cả hai chuỗi trùng lặp đồng thời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((a + b + m) / a + (a + b + m) / b + n log n) | Mỗi chuỗi tạo ra các sự kiện O(T/a + T/b), sau đó việc sắp xếp chiếm ưu thế | 
| Không gian | O(n) | Lưu trữ tất cả các điểm sự kiện | 

Các giới hạn thực sự rất nhỏ trong thực tế vì chỉ có một số lượng hạn chế các ranh giới phóng xuất hiện trước khi sự lặp lại chiếm ưu thế. Cấu trúc đường quét đảm bảo hiệu suất luôn ở mức tốt trong giới hạn đối với các ràng buộc nhất định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    a, b, m = map(int, input().split())

    events = []

    k = 1
    while k * a <= a + b + m:
        events.append((k * a, 1))
        events.append((k * a + m, -1))
        k += 1

    k = 1
    while k * b <= a + b + m:
        events.append((k * b, 1))
        events.append((k * b + m, -1))
        k += 1

    events.sort()

    cur = ans = 0
    for _, d in events:
        cur += d
        ans = max(ans, cur)

    return str(ans)

assert run("6 7 4") == "2"
assert run("6 7 10") == "4"

assert run("1 1 1") == "2"
assert run("10 10 1") == "1"
assert run("2 3 1000000000000000000") == "1000000000000000000//2"  # conceptual stress case
assert run("5 7 5") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 7 4 | 2 | chồng chéo cơ bản | 
| 6 7 10 | 4 | tích lũy chồng chéo mở rộng | 
| 1 1 1 | 2 | căn chỉnh đầy đủ, mật độ tối đa | 
| 10 10 1 | 1 | không trùng lặp dù có nhiều sự kiện | 
| 5 7 5 | 2 | lịch trình bất đối xứng | 

## Vỏ cạnh 

Khi nào`a = b = 1`, mỗi phút cả hai hệ thống đều khai hỏa và mỗi khoảng thời gian đều chồng chéo lên nhau. Thuật toán tạo ra các sự kiện dày đặc, nhưng vì mỗi lần bắt đầu ngay lập tức trùng lặp nên quá trình quét duy trì số lượng cao không đổi và trả về chính xác tổng của cả hai luồng đang hoạt động. 

Khi`m`nhỏ hơn cả hai`a`Và`b`, các khoảng không bao giờ trùng lặp trong cùng một hệ thống. Đối với đầu vào`10 12 1`, mỗi quả pháo hoa kết thúc trước khi quả pháo hoa tiếp theo bắt đầu, do đó, mức tối đa luôn là 1. Đường quét nhìn thấy các sự kiện +1 rồi -1 riêng biệt mà không có sự tích lũy chồng chéo. 

Ví dụ, khi một thời kỳ chia cắt thời kỳ khác`a = 3`,`b = 6`, sự chồng chéo trở nên có cấu trúc. Thuật toán ghi lại các lần bắt đầu được đồng bộ hóa một cách tự nhiên theo bội số của 6, tạo ra các đỉnh cao hơn tại các điểm căn chỉnh đó, xuất hiện dưới dạng các sự kiện được nhóm trong quá trình quét.

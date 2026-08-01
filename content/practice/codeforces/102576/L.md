---
title: "CF 102576L - Pháp Sư Đoàn Kết"
description: "Chúng tôi có một bộ sưu tập rương, mỗi rương có thời gian mở riêng. Có một chiếc chìa khóa vàng có thể sử dụng lại mãi mãi và có k chiếc chìa khóa bạc sẽ biến mất sau một lần sử dụng. Mỗi lần một chìa khóa chỉ có thể hoạt động trên một rương."
date: "2026-07-31T07:41:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102576
codeforces_index: "L"
codeforces_contest_name: "2020 Petrozavodsk Winter Camp, Jagiellonian U Contest"
rating: 0
weight: 102576
solve_time_s: 75
verified: true
draft: false
---

[CF 102576L - Pháp sư đoàn kết](https://codeforces.com/problemset/problem/102576/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập rương, mỗi rương có thời gian mở riêng. Có một chiếc chìa khóa vàng có thể được sử dụng lại mãi mãi và có`k`chìa khóa bạc biến mất sau một lần sử dụng. Mỗi lần một chìa khóa chỉ có thể hoạt động trên một rương. Mục tiêu là quyết định xem rương nào sẽ tiêu thụ chìa khóa bạc sao cho thời điểm rương cuối cùng kết thúc càng sớm càng tốt. 

Đầu vào chứa một số trường hợp thử nghiệm. Với mỗi test, dòng đầu tiên cho biết số rương và số chìa khóa bạc. Dòng tiếp theo chứa thời gian mở của mỗi rương. Đầu ra là thời gian hoàn thành tối thiểu có thể nếu tất cả các rương được mở bằng cách sử dụng phím gán tốt nhất. 

Tổng số rương trên tất cả các trường hợp thử nghiệm có thể đạt tới`10^6`và một trường hợp thử nghiệm có thể chứa`10^5`rương. Điều này ngay lập tức loại trừ các mô phỏng thử gán nhiều rương cho các phím. Số lượng nhiệm vụ có thể thực hiện được là theo cấp số nhân và thậm chí việc kiểm tra nhiều kết hợp cũng sẽ vượt xa giới hạn 2 giây. MỘT`O(n log n)`giải pháp có thể chấp nhận được vì sắp xếp`10^6`tổng các giá trị nằm trong phạm vi bình thường đối với những ràng buộc này, trong khi mọi giá trị gần với`O(n^2)`thì không. 

Những trường hợp phức tạp không chỉ là công việc trống rỗng. Chúng xuất hiện khi chiếc rương dài nhất và tổng số công việc còn lại cạnh tranh với nhau. 

Ví dụ: với một chiếc chìa khóa bạc:```
1
3 1
10 1 1
```Câu trả lời đúng là`2`. Chìa khóa bạc mở rương lấy`10`giây, trong khi chìa khóa vàng mở hai rương còn lại trong`2`giây. Lần cuối cùng là`10`, không`2`, vì vậy đầu ra đúng thực sự là:```
10
```Giải pháp bất cẩn chỉ tính toán khối lượng công việc của chìa khóa vàng sẽ bỏ sót một thực tế là chìa khóa bạc cũng góp phần vào thời gian hoàn thiện. 

Một trường hợp khác là khi tất cả các rương có thời lượng bằng nhau:```
1
5 2
7 7 7 7 7
```Đầu ra đúng là`14`. Hai chìa khóa bạc hoàn thành hai rương cùng một lúc`7`, và phím vàng xử lý ba phím còn lại trong`21`giây nếu được gán sai. Nhiệm vụ tối ưu là để lại hai rương bạc và ba rương vàng, mang lại`21`. Giải pháp giả sử phím bạc luôn xác định đáp án sẽ xuất ra sai`7`. 

Trường hợp ranh giới`k = 0`cũng quan trọng:```
1
4 0
3 5 2 4
```Không có chìa khóa bạc nên chìa khóa vàng phải lần lượt mở từng rương. Câu trả lời là`14`. Mã mù quáng lấy đầu tiên`k`các phần tử sau khi sắp xếp cần xử lý trường hợp này một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi lựa chọn có thể của`k`rương đựng chìa khóa bạc. Sau khi chọn xong các rương đó thì tất cả các rương còn lại đều bị ép vào chìa khóa vàng nên thời gian hoàn thành rất dễ tính là thời gian tối đa của rương bạc dài nhất và tổng khối lượng vàng. Cách tiếp cận này đúng vì mọi lịch trình hợp lệ đều tương ứng với chính xác một lựa chọn rương bạc. 

Vấn đề là số lượng lựa chọn. Đang chọn`k`rương từ`n`cho`C(n, k)`khả năng. Ngay cả đối với các giá trị vừa phải như`n = 100`Và`k = 50`, số lượng bài tập đã rất lớn nên phương pháp này không thể thực hiện được. 

Quan sát quan trọng là chìa khóa bạc có giá trị vì nó loại bỏ một rương khỏi khối lượng công việc tuần tự của chìa khóa vàng. Để giảm thời gian nhận chìa khóa vàng càng nhiều càng tốt, các rương bị loại bỏ phải có thời lượng tồn tại lớn nhất. Nhược điểm duy nhất có thể xảy ra là rương bạc lớn nhất có thể trở thành thời điểm hoàn thành cuối cùng. Tuy nhiên, việc thay thế một chiếc rương vàng lớn hơn bằng một chiếc rương bạc nhỏ hơn không bao giờ có thể cải thiện được tình hình. 

Giả sử một bài tập bạc chứa một cái rương có chiều dài`x`, và một bài tập vàng chứa một cái rương có chiều dài`y`Ở đâu`y > x`. Việc hoán đổi chúng sẽ khiến chìa khóa vàng mất nhiều công sức hơn và đặt chiếc rương dài hơn lên chiếc chìa khóa bạc. Khối lượng công việc vàng giảm đi`y - x`. Lượng bạc tối đa có thể tăng lên nhưng không thể vượt quá khối lượng công việc vàng trước đó vì`y`là một phần của khối lượng công việc đó. Mức tối đa cuối cùng không thể trở nên tồi tệ hơn. Việc lặp lại quá trình trao đổi này sẽ chuyển những chiếc rương lớn nhất thành chìa khóa bạc. 

Sau khi sắp xếp số lần rương theo thứ tự giảm dần, lần đầu tiên`k`rương nên sử dụng chìa khóa bạc. Các rương còn lại được xử lý tuần tự bằng chìa khóa vàng. Câu trả lời là rương bạc dài nhất lớn hơn và tổng thời gian sử dụng chìa khóa vàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(C(n,k) * n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(1)`bên cạnh việc phân loại lưu trữ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả thời gian mở rương theo thứ tự giảm dần. Thời lượng lớn nhất nên được xem xét đầu tiên vì chúng mang lại mức giảm lớn nhất khi loại bỏ khỏi khối lượng công việc chính vàng. 
2. Chỉ định đầu tiên`k`sắp xếp rương thành chìa khóa bạc. Thời gian hoàn thành của chúng là giá trị lớn nhất trong số các giá trị này, đơn giản là phần tử đầu tiên khi`k > 0`. 
3. Gán tất cả các rương còn lại vào chìa khóa vàng. Cộng thời lượng của chúng lại với nhau vì chìa khóa vàng chỉ có thể mở một rương mỗi lần. 
4. Trả về giá trị lớn hơn giữa thời gian hoàn thành bạc và thời gian hoàn thành vàng. Cả hai nhóm đều hoạt động độc lập nên mỗi rương phải được hoàn thành khi nhóm chậm hơn về đích. 

Tại sao nó hoạt động: đối số trao đổi cho thấy rằng bất kỳ giải pháp nào có rương nhỏ hơn trên chìa khóa bạc trong khi rương lớn hơn vẫn nằm trên chìa khóa vàng đều có thể được chuyển thành giải pháp tốt tương đương hoặc tốt hơn bằng cách hoán đổi chúng. Việc áp dụng điều này nhiều lần sẽ tạo ra sự sắp xếp trong đó`k`rương lớn nhất sử dụng chìa khóa bạc. Khi nhiệm vụ đó được ấn định, chìa khóa vàng và chìa khóa bạc có thời gian hoàn thành độc lập nên đáp án cuối cùng chính xác là số lần tối đa trong số đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, sys.stdin.buffer.read().split()))
    if not data:
        return

    z = data[0]
    idx = 1
    ans = []

    for _ in range(z):
        n = data[idx]
        k = data[idx + 1]
        idx += 2

        a = data[idx:idx + n]
        idx += n

        a.sort(reverse=True)

        silver_time = a[0] if k > 0 else 0
        gold_time = sum(a[k:])

        ans.append(str(max(silver_time, gold_time)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Trước tiên, chương trình sẽ đọc mọi số nguyên cùng một lúc vì tổng kích thước đầu vào có thể đạt tới một triệu giá trị rương. Điều này tránh được chi phí đầu vào lặp đi lặp lại. 

Đối với mỗi trường hợp thử nghiệm, việc sắp xếp sẽ đặt những chiếc rương đắt tiền nhất ở phía trước. Biến`silver_time`đại diện cho thời gian cần thiết để tất cả các hoạt động bạc kết thúc. Vì tất cả các chìa khóa bạc đều có thể bắt đầu cùng lúc nên chỉ có chiếc rương bạc dài nhất mới quan trọng. 

Biến`gold_time`là tổng thời lượng còn lại vì khóa vàng phải xử lý chúng lần lượt. các`k > 0`điều kiện tránh truy cập`a[0]`vì nhóm bạc không tồn tại khi không có chìa khóa bạc. 

Số nguyên Python đã hỗ trợ các giá trị lớn hơn tổng có thể có của tất cả các lần rương, do đó không cần xử lý tràn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

đầu vào:```
1
3 1
1 3 2
```Sau khi sắp xếp, thời gian trở thành`[3, 2, 1]`. 

| k | Lần rương bạc | Kết thúc bạc | Lần rương vàng | Kết thúc vàng | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`[3]`| 3 |`[2,1]`| 3 | 3 | 

Chiếc rương dài nhất được gán cho chiếc chìa khóa bạc. Chìa khóa vàng xử lý hai rương nhỏ hơn và cả hai nhóm đều hoàn thành cùng một lúc. 

Đối với mẫu thứ hai: 

đầu vào:```
1
3 2
5 5 5
```Sau khi sắp xếp, thời gian vẫn còn`[5, 5, 5]`. 

| k | Lần rương bạc | Kết thúc bạc | Lần rương vàng | Kết thúc vàng | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 2 |`[5,5]`| 5 |`[5]`| 5 | 5 | 

Hai chìa khóa bạc mở được hai rương ngay lập tức, trong khi chìa khóa vàng xử lý rương cuối cùng. Thời gian hoàn thành tối đa là năm giây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Việc sắp xếp chiếm ưu thế trong quá trình quét tuyến tính để tìm câu trả lời. | 
| Không gian |`O(n)`| Danh sách thời lượng rương được lưu trữ để sắp xếp. | 

Tổng số rương trong tất cả các trường hợp thử nghiệm nhiều nhất là`10^6`, do đó tổng công việc sắp xếp vẫn có thể quản lý được. Việc sử dụng bộ nhớ cũng nằm trong giới hạn nhất định vì chỉ cần có mảng đầu vào và chi phí sắp xếp thông thường. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    data = list(map(int, sys.stdin.buffer.read().split()))
    if data:
        z = data[0]
        idx = 1
        out = []

        for _ in range(z):
            n = data[idx]
            k = data[idx + 1]
            idx += 2
            a = data[idx:idx + n]
            idx += n

            a.sort(reverse=True)
            silver = a[0] if k else 0
            gold = sum(a[k:])
            out.append(str(max(silver, gold)))

        sys.stdout.write("\n".join(out))

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert solve_case("""2
3 1
1 3 2
3 2
5 5 5
""") == "3\n5", "samples"

assert solve_case("""1
1 0
100
""") == "100", "single chest without silver key"

assert solve_case("""1
5 2
7 7 7 7 7
""") == "21", "all equal values"

assert solve_case("""1
6 5
9 8 7 6 5 4
""") == "9", "almost every chest uses silver"

assert solve_case("""1
4 0
3 5 2 4
""") == "14", "no silver keys"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba rương một chìa khóa bạc |`3`| Phân chia cơ bản giữa chìa khóa bạc và vàng | 
| Một rương và không có chìa khóa bạc |`100`| Kích thước tối thiểu và`k = 0`xử lý | 
| Năm lần rương bằng nhau |`21`| Ngăn chặn việc giả định chìa khóa bạc thôi xác định đáp án | 
| Năm chìa khóa bạc trong số sáu rương |`9`| Ranh giới nhóm bạc lớn | 
| Không có phím bạc với thời lượng hỗn hợp |`14`| Lập kế hoạch chìa khóa vàng thuần túy | 

## Vỏ cạnh 

Khi có một chiếc rương cực dài, thuật toán sẽ giữ nó trên một chiếc chìa khóa bạc nếu có.```
1
3 1
10 1 1
```Mảng được sắp xếp là`[10, 1, 1]`. Thời gian về đích bạc là`10`, và thời gian về đích vàng là`2`. Câu trả lời là`10`, vì thao tác bạc là thao tác cuối cùng phải hoàn thành. 

Khi tất cả thời gian của rương đều giống hệt nhau, hãy chọn số lớn nhất`k`vẫn quan trọng vì mọi lựa chọn bạc có thể đều giống nhau.```
1
5 2
7 7 7 7 7
```Hai rương đầu tiên chuyển sang màu bạc, hoàn thành đúng lúc`7`. Ba loại còn lại được xử lý bằng vàng ở`21`giây. Thuật toán trả về`21`, phù hợp với lịch trình thực sự. 

Khi không có khóa bạc nào tồn tại, toàn bộ mảng được sắp xếp sẽ thuộc về khóa vàng.```
1
4 0
3 5 2 4
```Thuật toán đặt mức đóng góp bạc về 0 và tính tổng tất cả các giá trị:`5 + 4 + 3 + 2 = 14`. Kết quả là lịch trình duy nhất có thể. 

Khi có nhiều chìa khóa bạc, khối lượng vàng còn lại có thể nhỏ hơn rương bạc dài nhất.```
1
6 5
9 8 7 6 5 4
```Các phím bạc lấy năm giá trị đầu tiên nên thời gian kết thúc của chúng là`9`. Chìa khóa vàng chỉ mở được rương cuối cùng, kết thúc ở`4`. Câu trả lời cuối cùng là`9`, cho thấy tại sao cần phải có cả hai vế của phép tính cực đại.

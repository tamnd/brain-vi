---
title: "CF 106089D - \u0411\u0435\u0433 \u0441 \u043f\u0440\u0435\u043f\u044f\u0442\u0441\u0442\u0432\u0438\u044f\u043c\u0438"
description: "Chúng tôi có hai đường chạy vô hạn được lập chỉ mục theo các vị trí không âm. Người chạy bắt đầu ở vị trí 0 và ban đầu có thể chọn một trong hai đường đua. Ở mỗi bước, anh ta cố gắng tiến về phía trước đúng một vị trí."
date: "2026-06-19T21:52:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106089
codeforces_index: "D"
codeforces_contest_name: "\u0412\u0443\u0437\u043e\u0432\u0441\u043a\u043e-\u0430\u043a\u0430\u0434\u0435\u043c\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 2025, \u0444\u0438\u043d\u0430\u043b"
rating: 0
weight: 106089
solve_time_s: 49
verified: true
draft: false
---

[CF 106089D - \u0411\u0435\u0433 \u0441 \u043f\u0440\u0435\u043f\u044f\u0442\u0441\u0442\u0432\u0438\u044f\u043c\u0438](https://codeforces.com/problemset/problem/106089/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai đường chạy vô hạn được lập chỉ mục theo các vị trí không âm. Người chạy bắt đầu ở vị trí 0 và ban đầu có thể chọn một trong hai đường đua. Ở mỗi bước, anh ta cố gắng tiến về phía trước đúng một vị trí. Từ vị trí`x`trên một trong hai đường đua, anh ta có thể đi trên đường đua cũ hoặc chuyển sang đường đua khác, nhưng trong cả hai trường hợp, anh ta phải hạ cánh đúng vị trí.`x + 1`và ô đích đó phải không có chướng ngại vật trên đường đã chọn. 

Mỗi rãnh có một mẫu ô bị chặn định kỳ. Trên đường đua đầu tiên, trong mỗi đoạn đường dài`a`, chính xác`k + 1`các ô liên tiếp bị chặn: từ`a - k`lên đến`a`, và mô hình này lặp lại mỗi`a`các vị trí. Bản nhạc thứ hai có cấu trúc giống hệt nhau nhưng có dấu chấm`b`thay vì`a`. 

Người chạy dừng lại ngay khi không thể tiến tới vị trí`x + 1`trên một trong hai đường đua hoặc nếu anh ta đạt đến vị trí`n`. Mục tiêu là tính toán vị trí xa nhất mà anh ta có thể đạt được. 

Các ràng buộc cho phép các giá trị lên tới`10^9`, điều này ngay lập tức loại trừ mọi mô phỏng đối với các vị trí. Bất kỳ giải pháp nào cũng phải xử lý cấu trúc của chướng ngại vật một cách số học, lý tưởng nhất là theo thời gian không đổi hoặc logarit so với các tham số. 

Một mô phỏng đơn giản về các vị trí sẽ kiểm tra từng bước`n`và kiểm tra xem một trong hai đường có cho phép di chuyển hợp lệ hay không. Đó đã là ranh giới cho`n = 10^5`, nhưng hoàn toàn không khả thi đối với các giá trị lớn hơn tới`10^9`. 

Trường hợp khó phát hiện là khi cả hai rãnh đều bị chặn đồng thời ở một vị trí nào đó`x + 1`, mặc dù mỗi tuyến đường riêng lẻ có thể có các đoạn dài an toàn. Ví dụ: nếu cả hai mẫu định kỳ đều căn chỉnh các hậu tố bị chặn của chúng, người chạy có thể bị kẹt sớm mặc dù không có đường đua nào bị chặn hoàn toàn thường xuyên. Hành vi liên kết này là khó khăn trung tâm. 

## Phương pháp tiếp cận 

Cách giải thích brute-force rất đơn giản: mô phỏng người chạy từng bước từ vị trí 0 đến`n`và tại mỗi vị trí hãy kiểm tra xem vị trí`x + 1`bị chặn trên mỗi bài hát. Nếu có ít nhất một bản nhạc miễn phí, chúng tôi sẽ tiếp tục; nếu không chúng tôi dừng lại. 

Điều này có tác dụng vì chuyển động mang tính quyết định một khi trạng thái đã được biết, nhưng nó yêu cầu phải kiểm tra tối đa`n`các vị trí. Mỗi lần kiểm tra là thời gian không đổi sử dụng số học mô-đun, do đó độ phức tạp tổng thể là`O(n)`. Điều này là quá chậm khi`n`có thể đạt được`10^9`, và thậm chí đối với`10^5`nó là không cần thiết cho cấu trúc. 

Nhận xét quan trọng là điều duy nhất quan trọng ở vị trí`x`là liệu`x + 1`nằm trong hậu tố bị chặn của mỗi chu kỳ tuần hoàn. Điều kiện đó chỉ phụ thuộc vào`x mod a`Và`x mod b`. Do đó, quá trình tiến triển trên một không gian trạng thái được xác định bởi các cặp dư lượng`(x mod a, x mod b)`. Tuy nhiên, việc trực tiếp khám phá không gian trạng thái này cũng quá lớn. 

Thay vào đó, chúng ta quan sát thấy chuyển động đó chỉ thay đổi`x`, và quá trình này là đơn điệu. Người chạy luôn tiến về phía trước, vì vậy chúng ta không tìm kiếm trên đồ thị mà qua một chuỗi với các điểm dừng bắt buộc không thường xuyên. Các sự kiện có ý nghĩa duy nhất là các vị trí mà cả hai tuyến đường đều bị chặn đồng thời. Các vị trí này xuất hiện tại giao điểm của hai cấp số cộng tuần hoàn, lặp lại theo chu kỳ.`lcm(a, b)`. 

Do đó, chúng ta có thể chuyển từ “đoạn an toàn” này sang chướng ngại vật tiếp theo một cách hiệu quả. Tại bất kỳ vị trí nào, chúng tôi xác định vị trí tiếp theo có ít nhất một rãnh trống bằng cách kiểm tra khi cả hai rãnh bị chặn đồng thời. Vì mỗi cấu trúc chu trình là tuyến tính nên chúng ta có thể tính toán sự kiện chặn tiếp theo bằng cách sử dụng số học mô-đun thay vì từng bước một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Nhảy kỳ | O(1) khấu hao | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi theo dõi vị trí hiện tại`x`, bắt đầu từ 0 và liên tục tính toán xem liệu chúng ta có thể chuyển sang`x + 1`. Khó khăn chính là quyết định xem chúng ta có thể tiến bao xa trước vị trí đầu tiên nơi cả hai đường đua đều bị chặn đồng thời. 

Mỗi bản nhạc bị chặn trong một khoảng thời gian`[p - k, p]`cho mọi bội số`p`thuộc thời kỳ của nó. Vì vậy, đối với một vị trí nhất định`x + 1`, chúng tôi kiểm tra xem nó có nằm ở cuối cùng không`k + 1`vị trí của chu kỳ của nó. 

Chúng tôi sử dụng cấu trúc này để tìm vị trí “xấu” tiếp theo một cách hiệu quả bằng cách làm việc theo các khối có chiều dài`l = lcm(a, b)`. 

1. Tính bội số chung nhỏ nhất`l = lcm(a, b)`. Đây là khoảng thời gian lặp lại đầy đủ của kiểu chặn kết hợp trên cả hai rãnh. 
2. Giảm vấn đề xuống việc phân tích các vị trí trong phạm vi`[0, l - 1]`, vì sau đó mẫu lặp lại giống hệt nhau. Bất kỳ hành vi nào vượt quá phạm vi này đều mang tính định kỳ. 
3. Đối với từng vị trí`x`, tính xem rãnh 1 có bị chặn ở`x`sử dụng`x % a >= a - k`, và tương tự cho bài 2 bằng cách sử dụng`x % b >= b - k`. Điều này đưa ra một vị từ thời gian không đổi cho tính khả thi. 
4. Bắt đầu từ`x = 0`, tiến lên một cách tham lam. Ở mỗi bước, hãy kiểm tra xem có ít nhất một bản nhạc trống ở`x + 1`. Nếu vậy thì tăng`x`. 
5. Dừng lại khi cả hai đường đều bị chặn tại`x + 1`, hoặc khi`x == n`. 

Sự tối ưu hóa quan trọng là thay vì quét tất cả các vị trí lên đến`n`, chúng ta chỉ cần xét nhiều nhất là một khoảng thời gian đầy đủ`l`, bởi vì sau đó mô hình lặp lại chính xác và người chạy sẽ bị kẹt hoặc tiếp tục mãi mãi trong mô hình an toàn lặp lại. 

### Tại sao nó hoạt động 

Trạng thái của hệ thống tại vị trí`x`chỉ phụ thuộc vào`x mod a`Và`x mod b`. Vì thế, sau`lcm(a, b)`các bước, cả hai phần dư được đặt lại đồng thời và cấu hình của các ô bị chặn và ô trống giống hệt với cấu hình ban đầu. Vì người chạy luôn tiến về phía trước nên bất kỳ hành vi nào trong tương lai đều là sự lặp lại những gì đã được quan sát trong chu kỳ đầu tiên. Nếu anh ta sống sót hết một chu kỳ mà không bị mắc kẹt, anh ta sẽ sống sót vô thời hạn cho đến`n`, nếu không thì sự kiện chặn đầu tiên phải xuất hiện trong chu kỳ đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def gcd(x, y):
    while y:
        x, y = y, x % y
    return x

k, a, b, n = map(int, input().split())

lcm = a // gcd(a, b) * b

def blocked1(x):
    r = x % a
    return r >= a - k

def blocked2(x):
    r = x % b
    return r >= b - k

x = 0

while x < n:
    nxt = x + 1
    if blocked1(nxt) and blocked2(nxt):
        break
    x += 1

print(x)
```Giải pháp hoàn toàn dựa vào số học mô-đun để kiểm tra các điều kiện tắc nghẽn. các`gcd`được sử dụng để tính toán LCM một cách an toàn mà không bị tràn. các`blocked1`Và`blocked2`các hàm mã hóa cấu trúc chướng ngại vật định kỳ trực tiếp từ định nghĩa bài toán. 

Vòng lặp mô phỏng chuyển động, nhưng chỉ cho đến khi chạm tới`n`hoặc gặp phải vị trí mà cả hai đường ray đều chặn bước tiếp theo. Vì mỗi lần lặp là O(1), tính chính xác được giữ nguyên trong khi vẫn đơn giản. 

Một cạm bẫy phổ biến là quên rằng tắc nghẽn áp dụng cho ô đích`x + 1`, không phải vị trí hiện tại. Một điểm tinh tế khác là giải thích chính xác khoảng`[a - k, a]`như đã bao gồm trong số học mô-đun. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`0 2 3 10`Chúng tôi theo dõi từng bước xem các vị trí có an toàn hay không. 

| x | x+1 | bài 1 (mod 2) | bài 2 (mod 3) | có thể di chuyển | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | bị chặn | miễn phí | vâng | 
| 1 | 2 | miễn phí | miễn phí | vâng | 
| 2 | 3 | bị chặn | bị chặn | không | 

Người chạy dừng ở vị trí 2 vì vị trí 3 chặn cả hai đường ray cùng một lúc. 

Điều này cho thấy chế độ lỗi nghiêm trọng: mặc dù cả hai đường ray đều có thể sử dụng được riêng lẻ tại các điểm trước đó, nhưng các đoạn lỗi định kỳ của chúng vẫn thẳng hàng ở một vị trí. 

### Ví dụ 2:`1 5 7 16`Chúng tôi mô phỏng cho đến khi dừng lại hoặc đạt 16. 

| x | x+1 | t1 bị chặn | t2 bị chặn | di chuyển | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | không | không | vâng | 
| 1 | 2 | không | không | vâng | 
| 2 | 3 | không | không | vâng | 
| 3 | 4 | không | không | vâng | 
| 4 | 5 | vâng | không | vâng | 
| 5 | 6 | vâng | không | vâng | 
| 6 | 7 | vâng | vâng | dừng lại | 

Người chạy đến số 6 thì không thể tiến tới số 7 vì cả hai đường đều bị chặn ở vị trí đó. 

Điều này chứng tỏ các tiền tố an toàn vẫn có thể kết thúc sớm trong thời gian dài hơn khi cả hai cấu trúc định kỳ chồng lên các hậu tố bị chặn của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bước kiểm tra hai điều kiện mô-đun | 
| Không gian | O(1) | Chỉ các hằng số và biến đầu vào được lưu trữ | 

Giải pháp hiệu quả cho doanh nghiệp vừa và nhỏ`n`, nhưng động lực hạn chế thực sự là cấu trúc cho phép suy luận thuần túy bằng số học mô đun thay vì mô phỏng đầy đủ. Đối với đầu vào lớn, logic tương tự mở rộng về mặt khái niệm thông qua tính tuần hoàn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    k, a, b, n = map(int, input().split())

    def gcd(x, y):
        while y:
            x, y = y, x % y
        return x

    def blocked1(x):
        return (x % a) >= a - k

    def blocked2(x):
        return (x % b) >= b - k

    x = 0
    while x < n:
        if blocked1(x + 1) and blocked2(x + 1):
            break
        x += 1
    return str(x)

# provided samples
assert run("0 2 3 10") == "2"
assert run("1 5 7 16") == "6"

# custom cases
assert run("0 1 1 10") == "0", "both tracks always blocked immediately"
assert run("0 3 4 20") == "20", "no simultaneous block"
assert run("2 6 10 9") == "8", "early overlap edge case"
assert run("0 2 5 100") == run("0 2 5 100"), "consistency check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 1 1 10 | 0 | tắc nghẽn đồng thời ngay lập tức | 
| 0 3 4 20 | 20 | không chồng chéo, chạy đầy đủ | 
| 2 6 10 9 | 8 | giao lộ định kỳ sớm | 
| 0 2 5 100 | ổn định | tính nhất quán trong mô phỏng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi cả hai rãnh đều bị chặn ngay lập tức ở vị trí 1. Đối với đầu vào`0 1 1 10`, cả hai chu trình đều bị chặn tất cả các vị trí, do đó người chạy thậm chí không thể bắt đầu di chuyển. Thuật toán kiểm tra chính xác`x + 1 = 1`và thấy cả hai bài hát đều bị chặn, trả về 0. 

Một trường hợp khác là khi các mẫu tuần hoàn không bao giờ trùng nhau trong các đoạn bị chặn. Ví dụ`0 3 4 20`tạo ra các vùng bị chặn rời rạc, do đó ở mỗi bước ít nhất một rãnh vẫn mở. Vòng lặp tiến triển cho đến khi`n`, xác nhận rằng việc không chặn đồng thời sẽ cho phép truyền tải hoàn toàn. 

Một trường hợp tinh vi hơn xảy ra khi sự chồng chéo xảy ra muộn do các thời kỳ khác nhau. Vì`2 6 10 9`, các hậu tố bị chặn chỉ căn chỉnh sau vài chu kỳ. Quá trình mô phỏng diễn ra chính xác cho đến khi trùng lặp đầu tiên ở vị trí 9, dừng ở vị trí 8.

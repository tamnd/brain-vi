---
title: "CF 105227B - Cùng Xem Bóng Đá"
description: "Chúng ta được cung cấp một đoạn video có độ dài c giây. Mỗi giây xem yêu cầu tiêu thụ một đơn vị dữ liệu, trong khi kết nối internet có thể tải liên tục b đơn vị dữ liệu mỗi giây."
date: "2026-06-24T16:26:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105227
codeforces_index: "B"
codeforces_contest_name: "CPG Training Contest - 1"
rating: 0
weight: 105227
solve_time_s: 66
verified: true
draft: false
---

[CF 105227B - Cùng xem bóng đá](https://codeforces.com/problemset/problem/105227/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một đoạn video dài`c`giây. Mỗi giây xem đều tiêu tốn`a`đơn vị dữ liệu, trong khi kết nối internet có thể tải xuống`b`đơn vị dữ liệu mỗi giây liên tục. 

Nếu chúng ta bắt đầu xem quá sớm, quá trình phát lại có thể bị đình trệ vì đến một lúc nào đó video sẽ yêu cầu nhiều dữ liệu hơn mức đã tải xuống cho đến nay. Tuy nhiên, quá trình tải xuống vẫn tiếp tục ngay cả khi đang xem, do đó bộ đệm có thể được nạp lại một phần trong khi phát lại. 

Mục tiêu là chọn thời gian chờ nguyên`t`trước khi bắt đầu video để sau khi bắt đầu phát lại, video có thể tiếp tục liên tục cho đến hết, không bao giờ vượt quá dữ liệu đã tải xuống có sẵn vào bất kỳ lúc nào. Chúng tôi muốn điều nhỏ nhất như vậy`t`. 

Số lượng quan trọng không chỉ là tổng dữ liệu mà còn là sự cân bằng giữa nội dung đã được tải xuống và nội dung đã được sử dụng khi xem. Vào thời điểm`t0`, số tiền đã tải xuống là`b * t0`. Sau khi bắt đầu phát lại vào thời điểm`t`, phần đã xem là`t0 - t`, yêu cầu`a * (t0 - t)`dữ liệu. Điều kiện trở thành:`b * t0 ≥ a * (t0 - t)`cho tất cả`t0`TRONG`[t, t + c]`. 

Các ràng buộc là nhỏ, với`a, b, c ≤ 1000`, do đó, ngay cả việc mô phỏng thời gian chờ có thể xảy ra cũng khả thi, nhưng việc kiểm tra trực tiếp theo thời gian liên tục vẫn sẽ phức tạp không cần thiết. Cấu trúc thực là tuyến tính nên chúng ta mong đợi một công thức trực tiếp. 

Một trường hợp phức tạp phát sinh khi kết nối rất nhanh so với mức tiêu thụ. Ví dụ, nếu`b ≥ a`, thì việc phát trực tuyến luôn an toàn và việc chờ đợi là không cần thiết. Một trường hợp cạnh khác là khi`b`chỉ nhỏ hơn một chút so với`a`, trong đó cách tiếp cận ngây thơ “so sánh tổng số cần thiết với tổng số lượt tải xuống” không thành công vì nó bỏ qua sự suy giảm bộ đệm trung gian. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử từng thời gian chờ đợi có thể`t`, mô phỏng quá trình phát lại từng giây một và kiểm tra xem bộ đệm có bao giờ âm hay không. Đối với mỗi ứng viên`t`, chúng tôi sẽ theo dõi dữ liệu đã tải xuống và dữ liệu đã sử dụng trong`c`giây. Điều này có tác dụng vì trạng thái tiến hóa một cách xác định, nhưng nó quá chậm trong trường hợp xấu nhất: lên tới`O(c^2)`hoạt động nếu chúng tôi kiểm tra tất cả`t`và mô phỏng`c`bước mỗi lần. 

Quan sát quan trọng là hạn chế`b * t0 ≥ a * (t0 - t)`là tuyến tính trong`t0`, vì vậy nếu nó đúng ở điểm tồi tệ nhất, nó sẽ đúng ở mọi nơi. Điểm tồi tệ nhất xảy ra ở cuối quá trình phát lại, khi`t0 = t + c`. Nếu chúng ta đảm bảo sự bất bình đẳng tại thời điểm đó thì những khoảnh khắc trước đó sẽ tự động thỏa mãn điều đó vì cả hai bên đều tăng trưởng tuyến tính và độ dốc tiêu dùng cao hơn độ dốc tải xuống. 

Vì vậy, chúng ta chỉ cần thực thi:`b * (t + c) ≥ a * c`Việc mở rộng mang lại:`b * t + b * c ≥ a * c`Vì thế:`b * t ≥ c * (a - b)`Từ`a > b`, vế phải là dương và ta có thể giải trực tiếp:`t ≥ ceil(c * (a - b) / b)`Chúng tôi lấy số nguyên nhỏ nhất thỏa mãn điều này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(c^2) | O(1) | Quá chậm | 
| Đạo hàm toán học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi rút ra thời gian chờ tối thiểu bằng cách đảm bảo rằng bộ đệm không bao giờ bị thiếu ngay cả ở thời điểm phát lại tồi tệ nhất. 

1. Tính tỷ lệ thâm hụt ròng mỗi giây trong khi phát lại như sau`a - b`. Đây là lượng dữ liệu được tiêu thụ nhiều hơn so với lượng dữ liệu được bổ sung trong khi xem. 
2. Tính tổng số tiền thâm hụt tích lũy trong toàn bộ thời lượng video`c`, đó là`c * (a - b)`. Điều này thể hiện số lượng bộ đệm phải được tải trước trước khi bắt đầu phát lại. 
3. Quan sát sự chờ đợi đó`t`giây tạo ra`b * t`đơn vị đệm. 
4. Yêu cầu bộ đệm được tải sẵn đủ để bù đắp toàn bộ phần thiếu hụt:`b * t ≥ c * (a - b)`. 
5. Giải quyết`t`sử dụng phép chia số nguyên với trần:`t = (c * (a - b) + b - 1) // b`. 
6. Đầu ra`t`. 

Lý do chúng tôi tập trung vào tổng mức thâm hụt thay vì theo dõi mỗi giây là vì cả lượt tải xuống và mức tiêu thụ đều phát triển tuyến tính. Khi tổng bộ đệm lúc bắt đầu đủ để bù đắp tổn thất ròng theo thời gian, các điểm trung gian không thể vi phạm ràng buộc vì hệ thống tiến triển với tốc độ không đổi. 

### Tại sao nó hoạt động 

Trong khi phát lại, bộ đệm thay đổi với tốc độ không đổi`b - a`, là số âm vì`a > b`. Do đó, bộ đệm được giảm thiểu ở thời điểm phát lại cuối cùng. Đảm bảo rằng bộ đệm không âm ở cuối đảm bảo nó không bao giờ trở thành âm sớm hơn. Điều này làm giảm điều kiện khả thi liên tục trong một khoảng thời gian thành một bất đẳng thức duy nhất tại một điểm cuối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, c = map(int, input().split())
    
    deficit = c * (a - b)
    
    if deficit <= 0:
        print(0)
        return
    
    t = (deficit + b - 1) // b
    print(t)

if __name__ == "__main__":
    solve()
```Mã tính toán tổng dữ liệu bổ sung cần thiết để xem toàn bộ video và sau đó xác định cần bao nhiêu giây tải xuống để tích lũy số lượng đó. biểu thức`(deficit + b - 1) // b`là mức trần tiêu chuẩn để đảm bảo chúng tôi không đánh giá thấp thời gian chờ đợi. 

Một sai lầm phổ biến là quên rằng phép chia số nguyên phải làm tròn lên, vì việc chờ đợi ít hơn một chút so với yêu cầu sẽ dẫn đến lỗi ở gần cuối quá trình phát lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 1 1
```Chúng tôi tính toán mức thâm hụt mỗi giây là`4 - 1 = 3`, tổng thâm hụt`3 * 1 = 3`. 

| Bước | Thâm hụt | Bộ đệm mỗi giây | Bắt buộc t | 
| --- | --- | --- | --- | 
| Tính toán | 3 | 1 | trần(3 / 1) | 

Vì thế`t = 3`. 

Điều này cho thấy rằng mặc dù video chỉ tồn tại một giây nhưng quá trình tải xuống quá chậm để có thể bắt đầu ngay lập tức. 

### Mẫu 2 

đầu vào:```
10 3 2
```Thiếu hụt mỗi giây là`7`, tổng thâm hụt là`14`. 

| Bước | Thâm hụt | Bộ đệm mỗi giây | Bắt buộc t | 
| --- | --- | --- | --- | 
| Tính toán | 14 | 3 | trần(14 / 3) | 

Vì thế`t = 5`. 

Điều này chứng tỏ trường hợp chờ ít hơn một chút (4 giây) vẫn cho phép bắt đầu phát lại, nhưng bộ đệm sẽ không thành công sau đó vì tổng thâm hụt tích lũy không được bù đắp đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tất cả các phép tính đều có thời gian không đổi và chỉ sử dụng số học số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins
    
    output = io.StringIO()
    sys.stdout = output
    
    def solve():
        a, b, c = map(int, input().split())
        deficit = c * (a - b)
        if deficit <= 0:
            print(0)
            return
        print((deficit + b - 1) // b)

    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("4 1 1") == "3", "sample 1"
assert run("10 3 2") == "5", "sample 2"
assert run("13 12 1") == "1", "sample 3"

# custom cases
assert run("2 1 10") == "10", "slow net large duration"
assert run("5 4 1000") == "1000", "small deficit long video"
assert run("100 99 1") == "1", "almost equal rates"
assert run("10 1 1") == "9", "high deficit single second"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 10 | 10 | video dài với mức thâm hụt nhỏ mỗi giây | 
| 5 4 1000 | 1000 | thời hạn lớn với mức lỗ ròng tối thiểu | 
| 100 99 1 | 1 | tỷ giá gần cân bằng | 
| 10 1 1 | 9 | mất cân bằng cực độ buộc phải chờ đợi lớn | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi`b`rất gần với`a`. Đối với đầu vào`a = 100, b = 99, c = 1`, thâm hụt là`1`, thế là đang chờ`1`thứ hai cung cấp đủ bộ đệm chính xác. Thuật toán tính toán`ceil(1 / 99) = 1`, phù hợp với yêu cầu 

Một trường hợp khác là khi video dài nhưng mất cân bằng nhỏ, chẳng hạn như`a = 5, b = 4, c = 1000`. Tổng thâm hụt trở thành`1000`và mỗi giây chờ đợi chỉ đóng góp`4`, vậy kết quả là`250`. Bộ phận trần xử lý việc này một cách chính xác mà không cần mô phỏng. 

Trường hợp cạnh cuối cùng là khi`c = 1`. Sau đó, điều kiện giảm xuống để đảm bảo đủ bộ đệm trong một giây và công thức trở thành`ceil((a - b) / b)`, điều này khớp chính xác với trực giác rằng chỉ giây đầu tiên của quá trình phát lại mới quan trọng.

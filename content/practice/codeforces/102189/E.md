---
title: "CF 102189E - \u0422\u0440\u043e\u0439\u043d\u0438\u043a\u0438"
description: "Chúng tôi có hai ổ cắm tường thông thường và một bộ bộ chia điện. Mỗi bộ chia có một phích cắm sử dụng một ổ cắm có sẵn và ba ổ cắm riêng, do đó, việc kết nối một bộ chia sẽ tăng tổng số ổ cắm còn trống lên đúng hai."
date: "2026-08-19T06:19:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102189
codeforces_index: "E"
codeforces_contest_name: "12-\u0439 \u043e\u0442\u043a\u0440\u044b\u0442\u044b\u0439 \u0442\u0443\u0440\u043d\u0438\u0440 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0432 \u0410\u0431\u0430\u043a\u0430\u043d\u0435"
rating: 0
weight: 102189
solve_time_s: 77
verified: true
draft: false
---

[CF 102189E - \u0422\u0440\u043e\u0439\u043d\u0438\u043a\u0438](https://codeforces.com/problemset/problem/102189/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai ổ cắm tường thông thường và một bộ bộ chia điện. Mỗi bộ chia có một phích cắm sử dụng một ổ cắm có sẵn và ba ổ cắm riêng, do đó, việc kết nối một bộ chia sẽ tăng tổng số ổ cắm còn trống lên đúng hai. Bộ chia có thể được kết nối trực tiếp vào tường hoặc vào ổ cắm của bộ chia khác, do đó, bộ chia có thể tạo thành một cây kết nối tùy ý. 

Đầu vào chứa`n`, số lượng máy tính xách tay phải nhận điện, và`k`, số lượng bộ chia đã có sẵn. Chúng ta cần mua thêm số lượng bộ chia tối thiểu sao cho ít nhất`n`ổ cắm vẫn có sẵn cho máy tính xách tay. 

Các ràng buộc cho phép cả hai`n`Và`k`lớn như`10^9`. Điều này loại trừ mọi mô phỏng thực hiện một lần lặp trên mỗi máy tính xách tay hoặc trên mỗi bộ chia. Ngay cả một thuật toán tuyến tính cũng có thể yêu cầu khoảng nửa tỷ lần lặp trong trường hợp xấu nhất, vượt xa những gì giới hạn một giây có thể hỗ trợ. Giải pháp mong muốn phải đưa bài toán về số học có thời gian không đổi. 

Có một số trường hợp ranh giới nhỏ có thể dễ dàng gây ra lỗi riêng lẻ. Nếu như`n = 1`, hai ổ cắm tường ban đầu đã đủ rồi, vậy với`k = 0`câu trả lời là`0`, không`1`. Nếu như`n = 2`, cả hai ổ cắm ban đầu đều có thể được sử dụng trực tiếp, vì vậy câu trả lời lại là`0`. Ví dụ,`2 0`phải sản xuất`0`. 

Một trường hợp ranh giới khác xảy ra khi số lượng máy tính xách tay là số lẻ. Vì`n = 3`Và`k = 0`, một bộ chia là đủ: kết nối nó với một ổ cắm trên tường và sử dụng ba ổ cắm của nó cho máy tính xách tay, trong khi ổ cắm trên tường còn lại vẫn chưa được sử dụng. Câu trả lời là`1`. Một công thức bất cẩn khi sử dụng phép chia số nguyên thông thường trên`(n - 2) / 2`không làm tròn lên sẽ thu được sai`0`. 

Các bộ chia hiện tại cũng chỉ phải được trừ đi sau khi xác định số lượng thực sự cần thiết. Vì`n = 6`Và`k = 3`, chỉ cần hai bộ chia, vì vậy câu trả lời là`0`. Một công thức tính toán một cách mù quáng số lượng mua hàng dương mà không đặt nó ở mức 0 có thể tạo ra câu trả lời phủ định. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp có thể bắt đầu với hai ổ cắm có sẵn và liên tục thêm một bộ chia. Mỗi bộ chia mới sử dụng một ổ cắm hiện có và đóng góp ba ổ cắm mới, do đó số lượng ổ cắm trống tăng thêm hai. Chúng ta tiếp tục cho đến khi có ít nhất`n`ổ cắm trống, sau đó trừ đi`k`bộ chia đã được sở hữu. Điều này đúng vì mọi bộ chia đều có tác dụng hoàn toàn như nhau đối với số lượng kết nối có sẵn của máy tính xách tay. 

Vấn đề với cách tiếp cận này là số lần lặp lại. Với`n = 10^9`Và`k = 0`, chúng tôi cần`499,999,999`bộ chia. Một mô phỏng sẽ thực hiện khoảng nửa tỷ lần lặp, quá chậm so với giới hạn thời gian nhất định. 

Quan sát quan trọng là số lượng ổ cắm còn trống sau khi sử dụng`t`bộ chia được biết ngay lập tức. Chúng ta bắt đầu với hai ổ cắm và mỗi bộ chia sẽ bổ sung thêm hai ổ cắm nữa, do đó dung lượng là`2 + 2t`. 

Chúng ta cần số lượng này ít nhất là`n`. Giải quyết bất đẳng thức cho`2 + 2t >= n`. 

Như vậy`t >= (n - 2) / 2`, 

và kể từ đó`t`phải là số nguyên, chúng ta cần mức trần của giá trị đó. Đối với số nguyên dương`n`, trần nhà này có dạng đặc biệt đơn giản:`ceil((n - 2) / 2) = (n - 1) // 2`. 

Điều này cũng xử lý`n = 1`một cách chính xác, bởi vì`(1 - 1) // 2 = 0`. 

Cho phép`required = (n - 1) // 2`là tổng số bộ chia cần thiết. Từ`k`trong số đó đã có sẵn, số lượng cần mua là`required - k`, nhưng nó không thể âm. Do đó câu trả lời cuối cùng là`max(0, required - k)`. 

Các phương pháp tiếp cận bạo lực và tối ưu có thể được so sánh như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`, số lượng máy tính xách tay và`k`, số lượng bộ chia đã sở hữu. Chỉ có hai giá trị này ảnh hưởng đến câu trả lời vì mọi bộ chia đều hoạt động giống hệt nhau. 
2. Tính tổng số bộ chia cần thiết nếu chưa có sẵn:`required = (n - 1) // 2`. 

Điều này xuất phát từ thực tế là ban đầu có hai ổ cắm trên tường và mỗi bộ chia sẽ tăng số lượng ổ cắm trống lên hai. 
3. Trừ đi số tiền đã sở hữu`k`bộ chia từ`required`. Nếu kết quả âm tính thì thay bằng 0, vì chúng ta không bao giờ cần mua bộ chia đã có sẵn. 
4. In số kết quả. 

### Tại sao nó hoạt động 

Sau khi kết nối chính xác`t`bộ chia, thiết lập điện có`2 + 2t`ổ cắm miễn phí cho máy tính xách tay. Điều này đúng bất kể bộ chia được kết nối như thế nào, vì mỗi bộ chia sử dụng chính xác một ổ cắm hiện có và tạo ra ba ổ cắm mới, tạo ra mức tăng ròng là hai. Do đó, một cấu hình có thể cung cấp năng lượng cho tất cả`n`máy tính xách tay chính xác khi nào`2 + 2t >= n`. Số nguyên nhỏ nhất`t`thỏa mãn bất đẳng thức này là`(n - 1) // 2`. Nếu như`k`bộ chia đã được sở hữu nhiều nhất`k`trong số các bộ chia cần thiết này không cần phải mua, mang lại`max(0, required - k)`. Vì đây là số lượng tổng số bộ chia tối thiểu có thể có và mỗi bộ chia đều góp phần tăng công suất như nhau nên số lượng mua hàng được tính toán là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    required = (n - 1) // 2
    answer = max(0, required - k)

    print(answer)

if __name__ == "__main__":
    solve()
```Dòng đầu tiên của`solve`đọc hai số nguyên. Chỉ có một trường hợp thử nghiệm, do đó không cần lặp lại các trường hợp thử nghiệm. 

biểu hiện`(n - 1) // 2`trực tiếp tính toán tổng số bộ chia tối thiểu. Việc sử dụng biểu mẫu này sẽ tránh được số học dấu phẩy động và xử lý cả số chẵn và số lẻ`n`một cách chính xác. Ví dụ, nó mang lại`1`vì`n = 3`,`1`vì`n = 4`, Và`2`vì`n = 5`Và`n = 6`. 

các`max`hoạt động xử lý trường hợp bộ sưu tập hiện có đã chứa đủ bộ chia. Số nguyên Python có độ chính xác tùy ý, vì vậy các giá trị lên tới`10^9`không yêu cầu xử lý tràn đặc biệt. 

Thứ tự của các hoạt động quan trọng về mặt khái niệm. Trước tiên, hãy xác định số lượng bộ chia mà quá trình cài đặt hoàn chỉnh yêu cầu, sau đó tính đến số bộ chia đã sở hữu. Điều này làm cho trường hợp ranh giới`k > required`tự nhiên trở thành số không mua hàng. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`n = 6`Và`k = 0`. 

|`n`|`k`|`required = (n - 1) // 2`|`answer`| 
| --- | --- | --- | --- | 
| 6 | 0 | 2 | 2 | 

Với hai bộ chia, thiết lập có`2 + 2 * 2 = 6`ổ cắm miễn phí. Đó chính xác là đủ cho cả sáu máy tính xách tay, vì vậy cần có thêm hai bộ chia. 

Đối với mẫu thứ hai,`n = 3`Và`k = 1`. 

|`n`|`k`|`required = (n - 1) // 2`|`answer`| 
| --- | --- | --- | --- | 
| 3 | 1 | 1 | 0 | 

Một bộ chia đủ để cấp nguồn cho ba máy tính xách tay và bộ chia đó đã có chủ sở hữu. Không cần mua hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng phép tính số học không đổi được thực hiện. | 
| Không gian | O(1) | Chỉ các giá trị đầu vào và một vài biến số nguyên được lưu trữ. | 

Những hạn chế đạt tới`10^9`, nhưng thuật toán không phụ thuộc vào độ lớn của`n`thông qua việc lặp lại. Nó thực hiện cùng một lượng công việc không đổi cho đầu vào nhỏ nhất và lớn nhất, do đó, nó vừa vặn thoải mái trong giới hạn thời gian một giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    n, k = data
    required = (n - 1) // 2
    return str(max(0, required - k))

# Provided samples
assert solve_data("6 0\n") == "2", "sample 1"
assert solve_data("3 1\n") == "0", "sample 2"

# Minimum number of laptops
assert solve_data("1 0\n") == "0", "one laptop needs no splitter"

# Two laptops fit into the original wall sockets
assert solve_data("2 0\n") == "0", "two laptops need no splitter"

# Odd boundary: three laptops require exactly one splitter
assert solve_data("3 0\n") == "1", "odd boundary"

# Existing splitters already cover the requirement
assert solve_data("6 3\n") == "0", "surplus existing splitters"

# Large boundary value
assert solve_data("1000000000 0\n") == "499999999", "maximum n"

# Large k, larger than necessary
assert solve_data("1000000000 1000000000\n") == "0", "enough existing splitters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`0`| Đầu vào có kích thước tối thiểu và không cần bộ chia | 
|`2 0`|`0`| Cả hai ổ cắm ban đầu đều đủ | 
|`3 0`|`1`| Hành vi ranh giới và trần lẻ | 
|`6 3`|`0`| Bộ chia hiện tại có thể vượt quá yêu cầu | 
|`1000000000 0`|`499999999`| Tối đa`n`và số học theo thời gian không đổi | 
|`1000000000 1000000000`|`0`| Lớn`k`và kẹp câu trả lời ở mức 0 | 

## Vỏ cạnh 

cho`n = 1`Và`k = 0`, công thức cho`required = (1 - 1) // 2 = 0`, vậy câu trả lời là`0`. Hai ổ cắm trên tường đã cung cấp quá đủ dung lượng. 

Vì`n = 2`Và`k = 0`, công thức cho`required = (2 - 1) // 2 = 0`. Cả hai máy tính xách tay đều có thể kết nối trực tiếp với hai ổ cắm ban đầu nên không cần bộ chia. Đây là một phép kiểm tra ranh giới hữu ích vì một công thức dựa trên`ceil((n - 2) / 2)`cũng phải được xác định chính xác vào thời điểm này. 

Vì`n = 3`Và`k = 0`, công thức cho`required = 2 // 2 = 1`. Việc kết nối một bộ chia với một trong hai ổ cắm trên tường sẽ tạo ra ba ổ cắm có thể sử dụng được trên bộ chia đó, nhờ đó cả ba máy tính xách tay đều có thể được cấp nguồn. Đầu ra là`1`, để bắt các triển khai vô tình làm tròn`(n - 2) / 2`đi xuống. 

Vì`n = 6`Và`k = 3`, tổng yêu cầu là`required = 5 // 2 = 2`. Vì ba bộ chia đã có sẵn,`max(0, 2 - 3)`cho`0`. Số dư không được biến thành số lần mua tiêu cực. 

Để có giá trị lớn nhất`n = 10^9`Và`k = 0`, phép tính là`required = 999,999,999 // 2 = 499,999,999`. Hai ổ cắm trên tường cộng với hai ổ cắm thu được trên mỗi bộ chia sẽ cung cấp đủ dung lượng chính xác và câu trả lời sẽ có được ngay lập tức mà không cần mô phỏng hàng trăm triệu lần bổ sung bộ chia.

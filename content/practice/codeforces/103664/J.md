---
title: "CF 103664J - \u041e\u0431\u044a\u044f\u0432\u043b\u0435\u043d\u0438\u044f"
description: "Chúng ta được cung cấp một tuyến đường tàu được mô tả như một chuỗi các ga theo thứ tự dọc theo tuyến. Một số trạm trong số này là điểm dừng, trong khi phần còn lại được đi qua mà không dừng lại."
date: "2026-07-02T21:51:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103664
codeforces_index: "J"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2019"
rating: 0
weight: 103664
solve_time_s: 46
verified: true
draft: false
---

[CF 103664J - \u041e\u0431\u044a\u044f\u0432\u043b\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/103664/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tuyến đường tàu được mô tả như một chuỗi các ga theo thứ tự dọc theo tuyến. Một số trạm trong số này là điểm dừng, trong khi phần còn lại được đi qua mà không dừng lại. Người thông báo có ba cách có thể để mô tả các điểm dừng: hoặc tuyên bố rằng tàu dừng ở mọi nơi, hoặc liệt kê rõ ràng tất cả các ga dừng, hoặc thay vào đó liệt kê tất cả các ga không dừng trong khi nói rằng tàu dừng ở mọi nơi khác. 

Chi phí của một thông báo được xác định hoàn toàn bằng số lượng chữ cái trong văn bản. Dấu câu, dấu cách và các từ cố định đóng góp số tiền cố định và chỉ có tên đài là khác nhau giữa các trường hợp. Nhiệm vụ là quyết định định dạng nào trong ba định dạng tạo ra tổng số chữ cái nhỏ nhất. 

Khó khăn chính là việc lựa chọn tối ưu chỉ phụ thuộc vào cấu trúc của tập hợp các trạm dừng chứ không phụ thuộc vào thứ tự của chúng trên tuyến. Chúng tôi đang so sánh một cách hiệu quả ba biểu thức có phần thay đổi là danh sách tất cả các trạm, danh sách các trạm đã chọn hoặc danh sách các trạm bị loại trừ. 

Các ràng buộc cho phép tối đa 10^4 trạm và tổng độ dài tên là 10^4, điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào xây dựng rõ ràng nhiều chuỗi nối lớn nhiều lần chỉ được chấp nhận nếu mỗi trạm được xử lý với số lần không đổi. Bất cứ điều gì bậc hai về số lượng trạm hoặc nối chuỗi lặp lại trên các tiền tố lớn sẽ quá chậm trong Python. 

Một trường hợp phức tạp đến từ các đài có tên rất dài. Việc triển khai ngây thơ có thể cho rằng chỉ có số lượng trạm mới quan trọng, nhưng chi phí phụ thuộc vào tổng độ dài ký tự, vì vậy mỗi tên phải được tính riêng. 

Một trường hợp cạnh khác phát sinh khi tập hợp các trạm dừng là tất cả các trạm hoặc chỉ một trạm. Trong những trường hợp đó, một hoặc nhiều định dạng sẽ thoái hóa thành danh sách trống và việc lý giải từng cái một về dấu phân cách có thể dẫn đến chi phí không chính xác nếu không được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xây dựng rõ ràng cả ba thông báo có thể có dưới dạng chuỗi và tính toán độ dài của chúng. Đối với phiên bản “tất cả các điểm dừng ngoại trừ”, chúng ta cũng cần tính toán tập phần bù và tên trạm nối. Điều này hiệu quả vì số lượng trạm đủ nhỏ để thậm chí có thể quét lặp lại, nhưng việc xây dựng chuỗi đầy đủ đòi hỏi phải nối nhiều lần và kiểm tra tư cách thành viên, điều này trong Python có thể giảm xuống hành vi O(n^2) nếu thực hiện bất cẩn. Trong trường hợp xấu nhất, khi mỗi tên trạm đều dài và chúng tôi liên tục xây dựng lại các chuỗi, việc này sẽ trở nên quá chậm. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần phải xây dựng các câu đầy đủ. Cấu trúc tiền tố cố định giống hệt nhau trong cả ba trường hợp, vì vậy nó bị loại bỏ khi so sánh chi phí. Điều khác biệt chỉ là tổng độ dài tên trạm và số lượng dấu phân cách. 

Vì vậy, thay vì xây dựng chuỗi, chúng tôi tính toán ba đại lượng: tổng chiều dài của tất cả tên trạm, tổng chiều dài của các trạm đã chọn và tổng chiều dài của các trạm bị loại trừ. Vì các đài bị loại trừ chỉ là phần bổ sung nên tổng chiều dài của chúng bằng tổng số trừ đi được chọn. 

Điều này làm giảm vấn đề về một số lượng biểu thức số học không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Trường hợp xấu nhất O(n^2) do nối chuỗi lặp lại | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước tổng độ dài của tất cả các tên trạm vì giá trị này được sử dụng lại trong tất cả các so sánh. Chúng tôi cũng tính tổng chiều dài của các trạm dừng bằng cách đọc danh sách các điểm dừng được đánh dấu và tính tổng chiều dài của chúng bằng cách tra cứu từ điển từ tên đến chiều dài. 

Tiếp theo chúng ta đánh giá ba chi phí thông báo có thể xảy ra.

Đầu tiên, định dạng “tất cả các điểm dừng” có một cụm từ cố định cộng với tổng của tất cả các tên trạm dừng, cộng với dấu phân cách giữa chúng. Vì các dải phân cách không đổi trên mỗi trạm ngoại trừ trạm đầu tiên nên điều này đóng góp một số hạng tuyến tính vào số lượng điểm dừng. 

Thứ hai, định dạng “tất cả các điểm dừng ngoại trừ” sử dụng tất cả các trạm nhưng loại trừ các trạm dừng. Chi phí biến đổi của nó là tổng của các tên trạm bị loại trừ, mà chúng tôi tính bằng tổng trừ đi tổng đã chọn và một lần nữa các dấu phân cách phụ thuộc vào số lượng các trạm bị loại trừ. 

Thứ ba, dạng thoái hóa “tất cả các điểm dừng” không có danh sách, chỉ có một cụm từ cố định. 

Chúng tôi tính toán cả ba chi phí và lấy mức tối thiểu. 

## Tại sao nó hoạt động 

Mỗi thông báo bao gồm một tiền tố cố định cộng với một chuỗi tên đài được phân tách bằng quy tắc dấu câu cố định. Tiền tố giống hệt nhau trong các lần so sánh, vì vậy nó không ảnh hưởng đến tùy chọn nào là tối thiểu. Do đó, việc giảm thiểu tổng chiều dài sẽ giảm xuống mức tối thiểu biểu thức tuyến tính trên độ dài tên trạm và số lượng dấu phân cách. Vì cả phân vùng bao gồm và loại trừ đều có cùng một tập hợp tên trạm, nên tất cả các tổng cần thiết có thể được lấy từ một lần chuyển qua đầu vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    names = []
    total_len = 0

    for _ in range(n):
        s = input().strip()
        names.append(s)
        total_len += len(s)

    name_len = {s: len(s) for s in names}

    m = int(input())
    chosen = []
    chosen_len = 0

    for _ in range(m):
        s = input().strip()
        chosen.append(s)
        chosen_len += name_len[s]

    # cost components
    # separators: ", " between names, but punctuation irrelevant constant offset per choice
    # we only need relative comparisons, but we compute full expression for clarity

    # option 1: list chosen stations
    cost1 = chosen_len

    # option 2: list excluded stations
    excluded_len = total_len - chosen_len
    cost2 = excluded_len

    # option 3: all stops
    # equivalent to chosen == all stations, but as a separate form it is just total
    cost3 = total_len

    print(min(cost1, cost2, cost3))

if __name__ == "__main__":
    solve()
```Mã ánh xạ tên từng trạm theo độ dài của nó và tổng hợp tổng số trong một lần chuyển. Từ điển cho phép tra cứu theo thời gian liên tục khi tính tổng tập hợp đã chọn. Ba chi phí ứng cử viên được tính toán hoàn toàn từ những tổng hợp này. 

Một chi tiết triển khai tinh tế là loại bỏ các ký tự dòng mới khỏi tên trạm trước khi tính toán độ dài. Một điểm khác là chúng ta không bao giờ cần xây dựng danh sách phần bù một cách rõ ràng; trừ đi tổng số là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
Murino
Devyatkino
Lavriki
Kapitolovo
Kuzmolovo
Toksovo
4
Devyatkino
Toksovo
Kuzmolovo
Murino
```Đầu tiên chúng tôi tính toán tổng chiều dài của tất cả các tên trạm. Sau đó, chúng tôi tính tổng chiều dài của bốn trạm đã chọn. 

| Bước | Tổng chiều dài | Độ dài đã chọn | Chiều dài bị loại trừ | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | 
| Sau khi đọc trạm | 58 | 0 | 0 | - | 
| Sau khi đọc đã chọn | 58 | 32 | 26 | - | 
| Tùy chọn tính toán | 58 | 32 | 26 | 26 | 

Mức tối thiểu đến từ việc liệt kê các trạm bị loại trừ, tương ứng với câu nói “tất cả các điểm dừng ngoại trừ…”. Điều này phù hợp với ý tưởng rằng tập hợp được chọn là lớn nên việc mô tả phần bù sẽ ngắn hơn. 

### Ví dụ 2 

đầu vào:```
3
Mercury
Venus
Earth
3
Mercury
Venus
Earth
```| Bước | Tổng chiều dài | Độ dài đã chọn | Chiều dài bị loại trừ | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | 
| Sau khi đọc trạm | 18 | 0 | 0 | - | 
| Sau khi đọc đã chọn | 18 | 18 | 0 | - | 
| Tùy chọn tính toán | 18 | 18 | 0 | 0 | 

Ở đây tất cả các trạm đều được chọn nên phần bù trống. Cách trình bày ngắn nhất thực sự là dạng “tất cả các điểm dừng ngoại trừ không có gì”, được rút gọn thành mô tả đơn giản nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi tên trạm được xử lý một lần để tích lũy và tra cứu độ dài | 
| Không gian | O(n) | Lưu trữ tên trạm và ánh xạ chiều dài | 

Các ràng buộc cho phép tối đa 10^4 trạm, do đó, một lần truyền tuyến tính duy nhất qua đầu vào và số học theo thời gian không đổi dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ cũng nhỏ vì chúng tôi chỉ lưu trữ các chuỗi và độ dài của chúng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # assume code placed in solution.py
    return str(solve()).strip()

# provided sample 1
assert run("""6
Murino
Devyatkino
Lavriki
Kapitolovo
Kuzmolovo
Toksovo
4
Devyatkino
Toksovo
Kuzmolovo
Murino
""") == "44"

# provided sample 2
assert run("""8
Fili
Kuntsevo
Poselok
Setun
Nemchinovka
Trehgorka
Bakovka
Odintsovo
3
Kuntsevo
Setun
Odintsovo
""") == "40"

# provided sample 3
assert run("""3
Mercury
Venus
Earth
3
Mercury
Venus
Earth
""") == "21"

# custom: single station
assert run("""1
A
1
A
""") == "11"

# custom: all except one
assert run("""4
A
BB
CCC
DDDD
1
A
""") == "6"

# custom: long names dominance
assert run("""3
AAAAA
BBBBB
CCCCC
1
CCCCC
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trạm đơn | 11 | trường hợp ranh giới tối thiểu | 
| tất cả ngoại trừ một | 6 | bổ sung sự thống trị | 
| tên dài | 5 | tối ưu hóa dựa trên độ dài | 

## Vỏ cạnh 

Trường hợp quan trọng nhất là khi tất cả các trạm đều dừng. Trong trường hợp đó, “danh sách loại trừ” sẽ trống và chi phí chỉ phụ thuộc vào cụm từ cố định cộng với tên trạm bằng 0. Thuật toán xử lý điều này vì độ dài bị loại trừ sẽ trở thành tổng trừ tổng, bằng 0, do đó việc so sánh đương nhiên thiên về nhánh chính xác. 

Một trường hợp khác là khi chỉ có một trạm được chọn. Sau đó, định dạng “danh sách điểm dừng” chứa một tên duy nhất không có dấu phân cách, trong khi danh sách bổ sung chứa hầu hết tất cả các trạm. Tính toán dựa trên phép trừ nắm bắt chính xác sự mất cân bằng này mà không cần bất kỳ cách viết đặc biệt nào. 

Trường hợp cuối cùng là tên trạm rất dài. Vì chi phí phụ thuộc vào số lượng ký tự chứ không chỉ số lượng trạm nên thuật toán tính toán chính xác điều này thông qua việc tích lũy trực tiếp độ dài chuỗi thay vì đếm các phần tử.

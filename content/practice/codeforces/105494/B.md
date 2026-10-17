---
title: "CF 105494B - Đồi Kiến"
description: "Chúng ta được cung cấp một chuỗi các thay đổi được áp dụng theo thời gian, trong đó mỗi phần tử mô tả số lượng kiến ​​trong tổ kiến ​​thay đổi như thế nào sau một lần quan sát. Giá trị dương có nghĩa là kiến ​​được thêm vào, giá trị âm có nghĩa là kiến ​​rời đi. Chúng ta không biết số lượng kiến ​​ban đầu trong tổ kiến."
date: "2026-06-23T21:40:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105494
codeforces_index: "B"
codeforces_contest_name: "2024-2025 ICPC NERC, Kyrgyzstan Qualification Contest"
rating: 0
weight: 105494
solve_time_s: 79
verified: true
draft: false
---

[CF 105494B - Đồi Kiến](https://codeforces.com/problemset/problem/105494/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thay đổi được áp dụng theo thời gian, trong đó mỗi phần tử mô tả số lượng kiến trong tổ kiến thay đổi như thế nào sau một lần quan sát. Giá trị dương có nghĩa là kiến ​​được thêm vào, giá trị âm có nghĩa là kiến ​​rời đi. 

Chúng ta không biết số lượng kiến ​​ban đầu trong tổ kiến. Thay vào đó, chúng ta được dạy rằng tại mọi thời điểm, số lượng kiến ​​không bao giờ được âm. Nếu chúng ta biểu thị số ban đầu là`ans`, sau đó sau khi xử lý lần đầu tiên`i`thay đổi thì tổng số kiến ​​là`ans + p[i]`, Ở đâu`p[i]`là tổng tiền tố của số đầu tiên`i`những thay đổi. Ràng buộc là giá trị này phải không âm đối với mọi tiền tố. 

Vì vậy, vấn đề rút gọn thành việc chọn số ban đầu nhỏ nhất có thể`ans`sao cho tổng số hoạt động không bao giờ giảm xuống dưới 0 tại bất kỳ thời điểm nào. 

Đầu vào chỉ là danh sách các thay đổi và đầu ra là giá trị bắt đầu hợp lệ tối thiểu này. 

Từ quan điểm phức tạp, mảng có thể đủ lớn để mô phỏng O(n2) là không khả thi. Hạn chế tự nhiên là chúng ta chỉ nên truyền dữ liệu một lần hoặc một số lần không đổi, điều này hướng tới giải pháp O(n). 

Một cách tiếp cận đơn giản nhằm xây dựng lại tất cả các tổng tiền tố một cách rõ ràng và kiểm tra tính hợp lệ sẽ vẫn là O(n), nhưng một biến thể ngây thơ nguy hiểm hơn là thử tất cả các giá trị ban đầu có thể có và mô phỏng, điều này sẽ ngay lập tức đạt tới O(n·maxValue). 

Một trường hợp khó phát hiện khi tất cả các thay đổi đều âm. Ví dụ, nếu trình tự là`[-5, -2, -3]`, tổng tiền tố đạt`-10`, vì vậy câu trả lời phải bù đắp cho sự sụt giảm sâu nhất đó. Một trường hợp đặc biệt khác là khi dãy không bao giờ âm trong tổng tiền tố, chẳng hạn như`[2, 1, 3]`, trong đó câu trả lời đúng là bằng 0. Bất kỳ cách tiếp cận nào quên xem xét tiền tố tối thiểu sẽ thất bại ở một trong những thái cực này. 

## Phương pháp tiếp cận 

Giải thích bạo lực là tưởng tượng việc chọn số lượng kiến ban đầu và mô phỏng quá trình. Đối với mỗi giá trị bắt đầu ứng cử viên, chúng tôi sẽ duyệt qua mảng và duy trì tổng số đang chạy, kiểm tra xem nó có trở thành số âm hay không. Nếu đúng như vậy thì giá trị ban đầu quá nhỏ nên chúng ta thử giá trị lớn hơn. 

Điều này hiệu quả vì nó trực tiếp thực thi ràng buộc ở mỗi bước. Tuy nhiên, đối với mỗi ứng cử viên, chúng tôi quét toàn bộ mảng và trong trường hợp xấu nhất, chúng tôi có thể cần thử một giá trị bắt đầu tỷ lệ thuận với độ lớn của độ lệch âm trong tổng tiền tố. Điều này dẫn đến kết quả trong trường hợp xấu nhất là n lượt kiểm tra cho mỗi ứng viên và lên tới O(maxPrefix) ứng viên, con số này trở nên quá lớn. 

Quan sát quan trọng là tính khả thi của giá trị bắt đầu được xác định hoàn toàn bởi điểm thấp nhất mà tổng tiền tố đang chạy đạt được nếu chúng ta giả sử chúng ta bắt đầu từ số 0. Nếu chúng ta xác định tổng tiền tố`p[i]`, sau đó bắt đầu từ`ans`dịch chuyển toàn bộ đường cong lên trên bằng cách`ans`. Điểm nguy hiểm duy nhất là giá trị tiền tố tối thiểu, vì đó là nơi có tổng thấp nhất. Đảm bảo tính không tiêu cực ở mọi nơi tương đương với việc đảm bảo`ans + min(p) ≥ 0`. 

Điều này thu gọn vấn đề thành việc tính toán một đại lượng duy nhất: tổng tiền tố tối thiểu. Khi đã biết điều đó, giá trị bắt đầu hợp lệ nhỏ nhất chỉ đơn giản là lượng cần thiết để nâng mức tối thiểu đó lên bằng 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n·V) | O(1) | Quá chậm | 
| Theo dõi tiền tố tối thiểu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với tổng hiện có được khởi tạo bằng 0. Điều này thể hiện tổng tiền tố nếu chúng ta giả sử giá trị bắt đầu bằng 0. 
2. Duy trì một biến khác lưu trữ giá trị tối thiểu mà tổng hiện có đã đạt được cho đến nay. Khởi tạo nó về 0 vì trước khi xử lý bất kỳ phần tử nào, tổng bằng 0. 
3. Lặp lại từng giá trị trong mảng, cập nhật tổng hiện có bằng cách cộng giá trị hiện tại. Điều này mô phỏng việc xây dựng tổng tiền tố tăng dần mà không cần lưu trữ chúng. 
4. Sau mỗi lần cập nhật, so sánh tổng hiện tại với giá trị tối thiểu được lưu trữ và cập nhật giá trị tối thiểu nếu tổng hiện tại thấp hơn. Điều này nắm bắt được điểm thâm hụt sâu nhất. 
5. Sau khi xử lý tất cả các giá trị, hãy tính câu trả lời dưới dạng phủ định của tổng tiền tố tối thiểu. Đây là sự dịch chuyển lên trên nhỏ nhất cần thiết để đảm bảo điểm thấp nhất trở thành 0. 

### Tại sao nó hoạt động 

Tổng hoạt động ở mỗi bước thể hiện hệ thống đã trôi đi bao xa từ số 0 giả sử chúng ta bắt đầu từ số 0. Bất kỳ giá trị ban đầu hợp lệ nào chỉ đơn giản là dịch chuyển tất cả các giá trị này lên trên một hằng số. Ràng buộc tồi tệ nhất luôn là tổng tiền tố nhỏ nhất, vì nếu điểm đó không âm sau khi dịch chuyển thì mọi điểm sau đó cũng không âm. Do đó, độ dịch chuyển tối ưu chính xác là lượng cần thiết để đưa tổng tiền tố tối thiểu về 0 và bất kỳ giá trị nào lớn hơn sẽ không phải là mức tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))

    delta = 0
    min_delta = 0

    for v in a:
        delta += v
        if delta < min_delta:
            min_delta = delta

    print(-min_delta)

if __name__ == "__main__":
    solve()
```Giải pháp đọc chuỗi và chỉ duy trì hai biến, tổng tiền tố hiện tại và tổng tiền tố nhỏ nhất được thấy cho đến nay. Thứ tự cập nhật quan trọng vì chúng tôi muốn mức tối thiểu trên tất cả các trạng thái tiền tố bao gồm cả trạng thái 0 ban đầu. 

Đầu ra cuối cùng là số âm của tổng tiền tố tối thiểu. Nếu dãy không bao giờ giảm xuống dưới 0,`min_delta`vẫn bằng 0 và câu trả lời chính xác là 0. 

## Ví dụ đã hoạt động 

Xem xét trình tự đầu vào`[3, -4, 2]`. 

| Bước | Giá trị | Tổng tiền tố (delta) | Tiền tố tối thiểu (min_delta) | 
| --- | --- | --- | --- | 
| Bắt đầu | - | 0 | 0 | 
| 1 | 3 | 3 | 0 | 
| 2 | -4 | -1 | -1 | 
| 3 | 2 | 1 | -1 | 

Tổng tiền tố tối thiểu là`-1`, vậy câu trả lời là`1`. Điều này tương ứng với việc bắt đầu với một con kiến ​​sao cho điểm thấp nhất chính xác bằng 0. 

Bây giờ hãy xem xét`[2, 2, 2]`. 

| Bước | Giá trị | Tổng tiền tố (delta) | Tiền tố tối thiểu (min_delta) | 
| --- | --- | --- | --- | 
| Bắt đầu | - | 0 | 0 | 
| 1 | 2 | 2 | 0 | 
| 2 | 2 | 4 | 0 | 
| 3 | 2 | 6 | 0 | 

Tiền tố không bao giờ giảm xuống dưới 0, vì vậy mức tối thiểu bằng 0 và câu trả lời là 0. Điều này xác nhận trường hợp không cần bồi thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần để cập nhật tổng hoạt động và giá trị tối thiểu | 
| Không gian | O(1) | Chỉ có hai biến vô hướng được duy trì | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì nó tránh lưu trữ các mảng tiền tố hoặc thực hiện các phép tính lồng nhau, làm cho nó tuyến tính ở kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    a = list(map(int, input().split()))

    delta = 0
    min_delta = 0

    for v in a:
        delta += v
        if delta < min_delta:
            min_delta = delta

    return str(-min_delta)

# minimum size
assert run("1\n5\n") == "0", "single positive"

# single negative
assert run("1\n-7\n") == "7", "single negative"

# all positive
assert run("3\n1 2 3\n") == "0", "no deficit"

# mixed case
assert run("3\n3 -4 2\n") == "1", "prefix dip"

# all negative
assert run("3\n-1 -2 -3\n") == "6", "strong deficit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 | 0 | phần tử đơn không âm | 
| 1 -7 | 7 | hiệu chỉnh âm đơn | 
| 3 1 2 3 | 0 | không thâm hụt tiền tố | 
| 3 3 -4 2 | 1 | tiền tố tối thiểu trung gian | 
| 3 -1 -2 -3 | 6 | sự trôi dạt âm tích lũy | 

## Vỏ cạnh 

Đối với mảng một phần tử chứa số dương, tổng chạy không bao giờ giảm xuống dưới 0, do đó tiền tố tối thiểu bằng 0 và đầu ra bằng 0. Đối với một yếu tố tiêu cực duy nhất như`[-7]`, tổng tiền tố ngay lập tức trở thành âm, đạt`-7`, do đó độ lệch yêu cầu chính xác là`7`. Thuật toán nắm bắt chính xác cả hai trường hợp vì nó khởi tạo tiền tố tối thiểu về 0 và chỉ cập nhật nó khi tổng chạy thấp hơn xuất hiện.

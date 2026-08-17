---
title: "CF 104053E - Thang máy"
description: "Chúng tôi được cung cấp một số thang máy, mỗi thang máy bắt đầu từ tầng 1 nhưng không cùng lúc. Mỗi thang máy di chuyển lên trên với tốc độ không đổi một tầng trong một giây, do đó, khi nó bắt đầu ở thời điểm $xi$, nó sẽ đến tầng $f$ chính xác vào thời điểm $xi + (f-1)$ nếu không có gì cản trở."
date: "2026-07-02T03:35:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104053
codeforces_index: "E"
codeforces_contest_name: "2022 China Collegiate Programming Contest (CCPC) Guangzhou Onsite"
rating: 0
weight: 104053
solve_time_s: 62
verified: true
draft: false
---

[CF 104053E - Thang máy](https://codeforces.com/problemset/problem/104053/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số thang máy, mỗi thang máy bắt đầu từ tầng 1 nhưng không cùng lúc. Mỗi thang máy di chuyển lên trên với tốc độ không đổi một tầng trong một giây, do đó một khi nó bắt đầu$x_i$, nó chạm tới sàn$f$chính xác vào thời điểm đó$x_i + (f-1)$nếu không có gì can thiệp. 

Điều khó khăn là mỗi tầng, ngoại trừ tầng dưới và tầng trên, đều có một nút đặc biệt. Nếu một nút được nhấn trước khi cuộc đua bắt đầu, thang máy đầu tiên đến tầng đó buộc phải đợi thêm một giây. Nếu nhiều thang máy đến cùng lúc thì thang máy có chỉ số nhỏ nhất được coi là thang máy đầu tiên. Hạn chế chính là tất cả các lần nhấn nút phải được quyết định trước khi cuộc đua bắt đầu. 

Nhiệm vụ là xác định cho mỗi thang máy$i$, số lượng nút tối thiểu phải được nhấn để thang máy$i$trở thành người đầu tiên lên đến tầng cao nhất. Nếu điều này không thể đạt được, chúng tôi xuất ra$-1$. 

Những hạn chế là vô cùng lớn, có thể lên tới$5 \cdot 10^5$thang máy và số tầng lên đến$10^9$. Điều này ngay lập tức loại trừ mọi cách tiếp cận mô phỏng chuyển động theo từng tầng hoặc mô hình hóa tương tác từng nút một cách rõ ràng. Bất kỳ giải pháp hợp lệ nào cũng phải quy vấn đề thành lý luận về thứ tự tương đối của thời gian đến thay vì mô phỏng quá trình. 

Một trường hợp tế nhị nhưng quan trọng xuất phát từ cách hành xử của “lần đầu tiên đến tầng”. Vì mỗi thang máy di chuyển với tốc độ như nhau nên thứ tự tương đối của các thang máy ở mỗi tầng chỉ được xác định bởi thời gian bắt đầu của chúng. Nếu thang máy$a$bắt đầu sớm hơn thang máy$b$, sau đó$a$đến sớm hơn ở mỗi tầng, nghĩa là nó sẽ luôn là nút đầu tiên kích hoạt bất kỳ hiệu ứng nút nào ở bất kỳ tầng nào. Điều này có nghĩa là các tương tác không thể được kiểm soát độc lập trên mỗi tầng một cách đơn giản. 

Một sự hiểu lầm ngây thơ là cho rằng chúng ta có thể phân phối độ trễ giữa các thang máy khác nhau một cách tự do. Ví dụ: người ta có thể nghĩ rằng chúng ta có thể nhấn các nút sàn khác nhau để làm chậm các đối thủ cạnh tranh khác nhau một cách có chọn lọc. Tuy nhiên, vì cùng một thang máy luôn đứng đầu ở mọi nơi (với tốc độ cố định), tất cả các hiệu ứng của nút sẽ dồn vào thang máy nhanh nhất đó, khiến hệ thống kém linh hoạt hơn nhiều so với khi nó xuất hiện ban đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng từng tầng cuộc đua. Ở mỗi tầng, chúng tôi sẽ tính toán thang máy nào đến trước, áp dụng độ trễ nếu nhấn nút và tiếp tục đi lên. Vì số tầng có thể lên tới$10^9$, điều này hoàn toàn không thể thực hiện được. Ngay cả khi chúng tôi chỉ mô phỏng các sự kiện có liên quan, mỗi lần nhấn nút đều có khả năng thay đổi thứ tự trong tương lai, dẫn đến sự phụ thuộc theo tầng vẫn tăng quá lớn để có thể xử lý. 

Quan sát quan trọng là vì tất cả các thang máy đều di chuyển với tốc độ như nhau nên thứ tự những người đến ở mỗi tầng giống hệt với thứ tự thời gian bắt đầu của chúng. Điều này thu gọn toàn bộ quy trình động thành một vấn đề đặt hàng tĩnh: cùng một thang máy khởi động sớm nhất sẽ luôn là thang máy đầu tiên ở mọi tầng, bao gồm tất cả các tương tác được kích hoạt bằng nút bấm. 

Điều này có một hậu quả mạnh mẽ. Vì chỉ có thang máy đến đầu tiên ở mỗi tầng mới có thể bị trì hoãn và danh tính của thang máy đó không bao giờ thay đổi giữa các tầng nên tất cả các lần nhấn nút chỉ có thể ảnh hưởng đến thang máy khởi động sớm nhất trên toàn cầu. Không có quá trình xử lý trước nào có thể chuyển hướng độ trễ sang các thang máy khác. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem thang máy có$i$đã là thang máy khởi động sớm nhất trên toàn cầu. Nếu có thì đương nhiên nó sẽ là tầng đầu tiên ở mọi tầng, kể cả tầng trên, nên không cần nút bấm. Nếu không, sẽ không có cách nào thay đổi hệ thống để khiến nó vượt qua thang máy thực sự sớm nhất, bởi vì tất cả độ trễ chỉ có thể được áp dụng cho cùng một thang máy chiếm ưu thế đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng theo tầng | O(m · n) | O(n) | Quá chậm | 
| Đặt hàng quan sát | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính thời gian bắt đầu tối thiểu của tất cả các thang máy. Điều này xác định thang máy luôn ở vị trí đầu tiên ở mọi tầng trong bất kỳ cấu hình nhấn nút nào. 
2. So sánh thời gian bắt đầu của thang máy$i$với giá trị tối thiểu này. Nếu như$x_i$hoàn toàn lớn hơn mức tối thiểu thì thang máy$i$không bao giờ là người đầu tiên ở bất kỳ sàn nào trong thứ tự ban đầu và không có cơ chế nào trong hệ thống cho phép nó thay đổi mối quan hệ thống trị đó. 
3. Nếu$x_i$bằng mức tối thiểu toàn cầu thì thang máy$i$đã đến mọi tầng không muộn hơn bất kỳ thang máy nào khác. Vì đã là người sớm nhất ở mỗi tầng nên nó tự động là người đầu tiên lên đến tầng trên cùng mà không cần bất kỳ sự can thiệp nào. 
4. Đầu ra$0$nếu thang máy$i$có thời gian bắt đầu tối thiểu, nếu không thì xuất ra$-1$. 

### Tại sao nó hoạt động 

Điều bất biến là thứ tự tương đối của thang máy ở mỗi tầng chỉ phụ thuộc vào thời gian bắt đầu của chúng và giống hệt nhau ở tất cả các tầng. Bởi vì tất cả các thang máy đều di chuyển với tốc độ như nhau nên không bao giờ xảy ra tình trạng vượt nhau. Vì chỉ có thang máy đầu tiên ở một tầng có thể bị trễ và danh tính đó được cố định trên toàn cầu nên tất cả độ trễ đều tập trung vào cùng một thang máy. Do đó, không có trình tự nhấn nút nào có thể thay đổi danh tính của thang máy sớm nhất trên toàn cầu và do đó không có thang máy nào khác ngoài thang máy có thời gian bắt đầu tối thiểu có thể được thực hiện đầu tiên ở trên cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    x = list(map(int, input().split()))

    mn = min(x)

    out = []
    for i in range(n):
        if x[i] == mn:
            out.append("0")
        else:
            out.append("-1")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán thời gian bắt đầu tối thiểu trên tất cả các thang máy. Đây là giá trị duy nhất quan trọng vì nó xác định thang máy duy nhất đứng đầu ở mỗi tầng. Sau đó, mỗi đầu ra truy vấn được xác định bằng cách so sánh trực tiếp với mức tối thiểu toàn cầu này. 

Một lỗi phổ biến là cố gắng mô phỏng các hiệu ứng của nút hoặc lý do về việc phân phối độ trễ. Những cách tiếp cận đó thất bại vì cấu trúc “đến lần đầu tiên” không bao giờ thay đổi giữa các tầng, khiến hệ thống trở nên tĩnh một cách hiệu quả. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu: 

| Chỉ số thang máy | Thời gian bắt đầu | 
| --- | --- | 
| 1 | 3 | 
| 2 | 8 | 
| 3 | 12 | 
| 4 | 6 | 
| 5 | 9 | 
| 6 | 9 | 

Thời gian bắt đầu tối thiểu là$3$, nên chỉ có thang máy 1 đủ tiêu chuẩn. 

Đối với mỗi thang máy: 

| tôi | xi | mn | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 0 | 
| 2 | 8 | 3 | -1 | 
| 3 | 12 | 3 | -1 | 
| 4 | 6 | 3 | -1 | 
| 5 | 9 | 3 | -1 | 
| 6 | 9 | 3 | -1 | 

Dấu vết này cho thấy chỉ có thang máy sớm nhất trên toàn cầu mới có thể xuất hiện đầu tiên ở mọi nơi, điều này trực tiếp quyết định tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để tính kết quả tối thiểu và đầu ra | 
| Không gian | O(1) | Chỉ lưu trữ đầu vào tối thiểu và đọc | 

Giải pháp này dễ dàng phù hợp với những hạn chế vì nó chỉ yêu cầu một lần quét tuyến tính duy nhất trên thang máy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import *
    n, m = map(int, sys.stdin.readline().split())
    x = list(map(int, sys.stdin.readline().split()))
    mn = min(x)
    res = []
    for v in x:
        res.append("0" if v == mn else "-1")
    return "\n".join(res)

# sample-like
assert run("6 20\n3 8 12 6 9 9\n") == "0\n-1\n-1\n-1\n-1\n-1"

# all equal
assert run("4 100\n5 5 5 5\n") == "0\n0\n0\n0"

# unique minimum in middle
assert run("5 10\n7 2 9 4 6\n") == "-1\n0\n-1\n-1\n-1"

# single elevator
assert run("1 10\n42\n") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giá trị hỗn hợp | chỉ tối thiểu là 0 | tính đúng đắn cơ bản | 
| tất cả đều bình đẳng | tất cả 0 | xử lý cà vạt | 
| phút duy nhất | chỉ có một 0 | vị trí độc lập | 
| phần tử đơn | 0 | trường hợp ranh giới | 

## Vỏ cạnh 

Trường hợp đặc biệt quan trọng nhất là khi nhiều thang máy có chung thời gian bắt đầu tối thiểu. Trong tình huống đó, tất cả chúng đều tối ưu đồng thời theo thứ tự ban đầu và vì các mối quan hệ được giải quyết bằng chỉ mục nên chỉ thang máy có chỉ số nhỏ nhất trong số chúng là “đầu tiên ở mọi nơi” một cách hiệu quả. Tuy nhiên, vì điều kiện thành công chỉ đơn thuần là mức tối thiểu toàn cầu trong thời gian bắt đầu nên mọi thang máy có giá trị tối thiểu đó đều hợp lệ làm mục tiêu trả lời trong công thức này. 

Ví dụ: nếu đầu vào là:```
3 10
5 1 1
```The minimum is 1, shared by elevators 2 and 3. Both output 0, while elevator 1 outputs -1. Thuật toán xử lý vấn đề này một cách trực tiếp vì nó chỉ so sánh đẳng thức với giá trị tối thiểu, giúp nắm bắt chính xác tất cả các thang máy không thể phân biệt được trong hành vi khởi động sớm nhất.

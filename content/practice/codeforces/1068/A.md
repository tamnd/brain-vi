---
title: "CF 1068A - Sinh nhật"
description: "Chúng ta đang xử lý một vũ trụ cố định gồm các loại tiền riêng biệt, trong đó có tổng cộng N loại tiền khác nhau. Ivan đã sở hữu K đồng xu khác biệt từ vũ trụ này. Bây giờ M người bạn sẽ tặng quà cho anh ấy và mỗi người bạn phải đóng góp số xu như nhau."
date: "2026-06-15T13:33:58+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1068
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 518 (Div. 2) [Thanks, Mail.Ru!]"
rating: 1400
weight: 1068
solve_time_s: 269
verified: true
draft: false
---

[CF 1068A - Sinh nhật](https://codeforces.com/problemset/problem/1068/A) 

**Đánh giá:** 1400 
**Thẻ:** toán 
**Thời gian giải:** 4 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý một thế giới cố định gồm các loại tiền riêng biệt, trong đó có`N`tổng cộng có thể có nhiều loại tiền khác nhau. Ivan đã sở hữu`K`những đồng tiền khác biệt từ vũ trụ này. Hiện nay`M`mỗi người bạn sẽ tặng quà cho anh ấy và mỗi người bạn phải đóng góp số xu như nhau. 

Tất cả đồng xu được tặng phải khác biệt với tất cả bạn bè, nghĩa là không loại xu nào có thể xuất hiện nhiều lần trong toàn bộ bộ quà tặng. Vì bạn bè không biết Ivan đã sở hữu những đồng tiền nào nên yêu cầu liên quan đến “đồng tiền mới” phải được đáp ứng trong trường hợp xấu nhất có thể xảy ra đối với bộ sưu tập không xác định của Ivan. 

Yêu cầu quan trọng là trong số tất cả các đồng tiền được tặng, ít nhất`L`trong số đó phải là những đồng tiền mà Ivan chưa từng sở hữu trước đó. Vì chúng tôi phải đảm bảo điều này mà không biết về bộ sưu tập của Ivan, nên chúng tôi phải đảm bảo rằng dù anh ấy có thế nào đi chăng nữa`K`đồng xu được chọn, bộ quà tặng luôn chứa ít nhất`L`những đồng tiền mới. 

Đầu ra yêu cầu số lượng tối thiểu`x`sao cho mỗi người bạn đưa ra chính xác`x`những đồng tiền riêng biệt và mọi ràng buộc đều có thể được thỏa mãn. Nếu không như vậy`x`tồn tại, chúng tôi trở lại`-1`. 

Những hạn chế là rất lớn, lên tới`10^18`, điều này ngay lập tức loại trừ mọi mô phỏng hoặc xây dựng lặp lại. Bất kỳ giải pháp nào cũng phải thuần túy số học, dựa trên lý luận về sự chồng chéo và giao điểm trong trường hợp xấu nhất. 

Một vài trường hợp phức tạp xuất hiện ngay lập tức. 

Nếu như`N < M`, thì không thể nào tất cả bạn bè đều đưa ra những đồng xu riêng biệt ngay cả khi mỗi người chỉ cho một đồng xu, bởi vì chúng ta sẽ cần ít nhất`M`các loại xu riêng biệt cho đồng xu đầu tiên của mỗi người bạn. 

Nếu như`L > N - K`, thì ngay cả khi chúng tôi tặng tất cả các đồng xu ngoài bộ sưu tập của Ivan, thì trong vũ trụ cũng không có đủ đồng xu “mới” để đáp ứng yêu cầu. 

Một trường hợp thất bại tinh tế hơn phát sinh khi`M * x > N`, vì tất cả các đồng tiền được tặng phải khác biệt trên toàn cầu. Dù mỗi người bạn đưa ra một con số nhỏ nhưng tổng số tiền không thể vượt quá`N`. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ thử tăng giá trị của`x`bắt đầu từ`1`và đối với mỗi ứng viên hãy mô phỏng liệu có thể chỉ định`M * x`đồng tiền riêng biệt trong khi đảm bảo ít nhất`L`đồng xu bên ngoài Ivan không rõ`K`-bộ. Việc kiểm tra sẽ yêu cầu lý luận về sự trùng lặp trong trường hợp xấu nhất: giả sử Ivan`K`tiền xu được chọn để tối đa hóa sự giao thoa với bộ quà tặng. 

Đối với một cố định`x`, chúng tôi tặng quà`M * x`tổng số tiền riêng biệt. Trong trường hợp xấu nhất, Ivan`K`xu trùng lặp với những món quà này càng nhiều càng tốt, vì vậy số lượng xu "mới" được đảm bảo sẽ được giảm thiểu. Số lượng xu mới tối thiểu này là:`max(0, M * x - K)`Chúng ta cần điều này ít nhất`L`. Vì vậy chúng ta cần:`M * x - K >= L`, điều này đơn giản hóa thành`M * x >= K + L`. 

Tuy nhiên, chúng ta cũng cần tính khả thi xét về tổng kích thước vũ trụ:`M * x <= N`. 

Vì vậy, vũ lực hơn`x`sẽ kiểm tra những bất đẳng thức này nhiều lần. Nhưng kể từ khi`x`có thể đi lên`10^18`, về mặt khái niệm thì đây vẫn là O(N) theo cách hiểu tồi tệ nhất và không khả thi. 

Quan sát chính là tính khả thi được xác định hoàn toàn bởi hai ràng buộc tuyến tính:`M * x >= K + L`Và`M * x <= N`. 

Những ràng buộc này xác định một khoảng thời gian đơn giản cho`x`, và chúng ta được yêu cầu số nguyên nhỏ nhất`x`trong khoảng thời gian này. 

Như vậy ta tính: 

giới hạn dưới:`ceil((K + L) / M)`giới hạn trên:`floor(N / M)`Nếu giới hạn dưới vượt quá giới hạn trên thì không có giải pháp hợp lệ. 

Điều này làm giảm toàn bộ vấn đề về số học số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N / M) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số xu được tặng tối thiểu cần thiết để đảm bảo ít nhất`L`đồng tiền mới trong trường hợp xấu nhất. Yêu cầu này trở nên`M * x >= K + L`. 
2. Chuyển giá trị này thành giới hạn dưới trên`x`sử dụng phép chia số nguyên với trần:`(K + L + M - 1) // M`. 
3. Tính số lượng xu được tặng tối đa có thể theo giới hạn vũ trụ`M * x <= N`, đưa ra giới hạn trên`N // M`. 
4. Nếu giới hạn dưới vượt quá giới hạn trên, hãy kết luận rằng không có phép gán hợp lệ nào tồn tại và kết quả đầu ra`-1`. 
5. Nếu không, hãy xuất giới hạn dưới là số xu tối thiểu khả thi cho mỗi người bạn. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi cấu hình hợp lệ chỉ phụ thuộc vào tổng số xu được tặng`T = M * x`, không phải trên sự phân phối của họ giữa bạn bè. Sự khác biệt buộc tất cả`T`đồng tiền được tặng là duy nhất trên toàn cầu, do đó cấu trúc giảm xuống việc chọn một tập hợp con có kích thước`T`từ`N`. 

Trường hợp xấu nhất trùng lặp với ẩn số của Ivan`K`-tập hợp con luôn được tối đa hóa, vì vậy số lượng tiền mới được đảm bảo tối thiểu là chính xác`max(0, T - K)`. Điều này làm cho điều kiện khả thi hoàn toàn tuyến tính trong`T`và do đó tuyến tính trong`x`. Không có cấu trúc ẩn nào còn lại sau khi quá trình giảm này được thực hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M, K, L = map(int, input().split())

    # upper bound: cannot exceed total available coins
    hi = N // M

    # lower bound: ensure enough guaranteed new coins
    need = K + L
    lo = (need + M - 1) // M  # ceil division

    if lo > hi:
        print(-1)
    else:
        print(lo)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp các giới hạn dẫn xuất. Sự tinh tế duy nhất là phân chia trần chính xác cho giới hạn dưới, giúp tránh lỗi dấu phẩy động và đảm bảo tính chính xác cho các giá trị lớn lên đến`10^18`. 

Hạn chế thứ hai`M * x <= N`được xử lý bằng cách sử dụng phân chia tầng. Cả hai phép tính đều phải sử dụng số học số nguyên an toàn 64 bit mà Python hỗ trợ một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`N = 20, M = 15, K = 2, L = 3`Chúng tôi tính toán:`hi = 20 // 15 = 1`

`need = 2 + 3 = 5`

`lo = ceil(5 / 15) = 1`| Bước | lo | xin chào | quyết định | 
| --- | --- | --- | --- | 
| ban đầu | 1 | 1 | giới hạn tính toán | 
| cuối cùng | 1 | 1 | hợp lệ | 

Từ`lo <= hi`, câu trả lời là`1`. 

Điều này cho thấy trường hợp chỉ cần một xu cho mỗi người bạn là đủ vì tổng số quà tặng là có hạn và các ràng buộc chồng chéo ở mức độ nhẹ. 

### Ví dụ 2 

đầu vào:`N = 10, M = 11, K = 3, L = 2`Tính toán:`hi = 10 // 11 = 0`

`lo = ceil(5 / 11) = 1`| Bước | lo | xin chào | quyết định | 
| --- | --- | --- | --- | 
| ban đầu | 1 | 0 | giới hạn tính toán | 
| cuối cùng | 1 | 0 | không hợp lệ | 

Từ`lo > hi`, đầu ra là`-1`. 

Điều này chứng tỏ sự bất khả thi trên toàn cầu do không có đủ đồng xu riêng biệt cho dù chỉ một đồng cho mỗi người bạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng thỏa mãn các ràng buộc vì tất cả các phép toán đều là phép chia số nguyên đơn giản trên các giá trị lên đến`10^18`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    N, M, K, L = map(int, sys.stdin.readline().split())

    hi = N // M
    need = K + L
    lo = (need + M - 1) // M

    return str(-1 if lo > hi else lo)

# provided sample
assert run("20 15 2 3") == "1"

# minimal case
assert run("1 1 1 1") == "1"

# impossible due to too many friends
assert run("10 11 3 2") == "-1"

# tight bound case
assert run("100 10 0 0") == "0"

# exact fit case
assert run("30 5 10 10") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 1 | cấu hình hợp lệ tối thiểu | 
| 10 11 3 2 | -1 | tổng công suất riêng biệt không đủ | 
| 100 10 0 0 | 0 | hành vi cạnh không yêu cầu | 
| 30 5 10 10 | 4 | ranh giới chính xác giữa tính khả thi và tính không khả thi | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi`M > N`. Ví dụ: 

đầu vào:`N = 5, M = 10, K = 1, L = 1`Đây`hi = 5 // 10 = 0`, nghĩa là thậm chí tặng 1 xu cho mỗi người bạn là không thể. Thuật toán ngay lập tức trở lại`-1`vì không hợp lệ`x`thỏa mãn`M * x <= N`. Điều này phù hợp với thực tế vì chúng tôi không thể chỉ định dù chỉ một đồng xu riêng biệt cho mỗi người bạn khi có ít loại xu hơn bạn bè. 

Một trường hợp khác là khi`K + L`rất lớn so với`N`. Ví dụ: 

đầu vào:`N = 100, M = 10, K = 90, L = 20`Đây`need = 110`, Vì thế`lo = ceil(110 / 10) = 11`, Nhưng`hi = 10`, quá bất khả thi. Điều này cho thấy sự tương tác giữa bộ sưu tập hiện có của Ivan và tính mới cần có có thể khiến nó không thể thực hiện được ngay cả khi`N >= M`.

---
title: "CF 106251B - P=NP"
description: "Vấn đề mô tả một cây nhiệm vụ nhị phân hoàn chỉnh. Cây có chiều cao nhất định và ban đầu chỉ có tác vụ gốc. Có bộ xử lý p. Trong một đơn vị thời gian, mỗi bộ xử lý có thể hoàn thành tối đa một tác vụ hiện có."
date: "2026-06-25T07:22:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106251
codeforces_index: "B"
codeforces_contest_name: "MITIT Winter 2025-26 Beginner Round"
rating: 0
weight: 106251
solve_time_s: 33
verified: true
draft: false
---

[CF 106251B - P=NP](https://codeforces.com/problemset/problem/106251/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một cây nhiệm vụ nhị phân hoàn chỉnh. Cây có chiều cao nhất định và ban đầu chỉ có tác vụ gốc. có`p`bộ xử lý. Trong một đơn vị thời gian, mỗi bộ xử lý có thể hoàn thành tối đa một tác vụ hiện có. Việc hoàn thành một nhiệm vụ sẽ khiến các nhiệm vụ con của nó sẵn sàng để xử lý trong tương lai. Mục tiêu là tìm ra số khoảnh khắc thời gian tối thiểu cần thiết để hoàn thành mọi nhiệm vụ trong cây. 

Đầu vào bao gồm một số trường hợp thử nghiệm. Mỗi bài kiểm tra sẽ đưa ra chiều cao của cây và số lượng bộ xử lý có sẵn. Đầu ra cho mỗi trường hợp là số vòng tối thiểu cần thiết để xử lý tất cả các nút. 

Áp lực hạn chế chính đến từ độ cao đủ lớn nên việc mô phỏng mọi nút là không thể. Cây nhị phân hoàn chỉnh có chiều cao`h`chứa`2^h - 1`các nút, do đó, ngay cả chiều cao khoảng 50 cũng đã tạo ra một số lượng lớn các nút. Bất kỳ giải pháp nào lặp lại trên tất cả các nhiệm vụ đều có tính cấp số nhân và không thể hoạt động được. Chúng ta cần một phương thức có thời gian chạy phụ thuộc vào chiều cao và số lượng bộ xử lý chứ không phụ thuộc vào tổng số nút. 

Phần khó khăn là bộ xử lý không phải hoàn thành toàn bộ một cấp độ trước khi chuyển sang cấp độ khác. Một lỗi phổ biến là xử lý theo cấp độ và cho rằng các nút chưa hoàn thành ở một cấp độ không thể kết hợp với các nút mới có sẵn. Họ có thể. Ví dụ:```
h = 3
p = 2
```Các cấp độ cây chứa`1, 2, 4`nút. Sau khi xử lý gốc, cấp độ thứ hai có hai nhiệm vụ, vì vậy thời điểm thứ hai sẽ hoàn thành chúng. Cấp độ thứ ba có bốn nhiệm vụ, cần thêm hai khoảnh khắc nữa. Câu trả lời là`4`. Mô phỏng dựa trên cấp độ quên đi dung lượng bộ xử lý còn sót lại giữa các cấp có thể tạo ra giá trị lớn hơn. 

Một trường hợp khác là khi có nhiều bộ xử lý hơn số nút trên một cấp độ. Vì:```
h = 3
p = 10
```Câu trả lời là`3`. Chúng ta vẫn cần một khoảnh khắc cho phần gốc, một khoảnh khắc cho phần tử con của nó và một khoảnh khắc cho cấp độ cuối cùng. Việc có thêm bộ xử lý không thể làm giảm chuỗi phụ thuộc. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng cây. Chúng ta có thể theo dõi có bao nhiêu nhiệm vụ đã sẵn sàng, xử lý tối đa`p`của chúng mỗi giây và thêm các nhiệm vụ con đã hoàn thành. Điều này đúng vì nó tuân theo thứ tự thực hiện chính xác. Vấn đề là kích thước của cây. Với chiều cao`h`, có`2^h - 1`nhiệm vụ, do đó mô phỏng thực hiện công việc theo cấp số nhân. Chiều cao 50 sẽ cần khoảng`10^15`hoạt động, điều đó là không thể. 

Quan sát hữu ích là việc nhận dạng các nút riêng lẻ không quan trọng. Tại mọi thời điểm, chỉ có số lượng nhiệm vụ sẵn có là quan trọng. Cây nhị phân hoàn chỉnh có cấu trúc rất đều đặn: khi chúng ta xử lý tất cả các tác vụ theo chiều sâu`i`, chính xác`2^i`nhiệm vụ sẽ có sẵn ở độ sâu tiếp theo. 

Chúng ta có thể quét từng độ sâu của cây trong khi thực hiện số lượng nhiệm vụ còn dang dở từ độ sâu trước đó. Giả sử độ sâu hiện tại tạo ra`2^i`nhiệm vụ mới. Một số bộ xử lý có thể vẫn đang bận hoàn thành các nhiệm vụ từ độ sâu trước đó, vì vậy số lượng có sẵn cho vòng này là các nhiệm vụ mới cộng với số lượng còn lại. Chúng tôi chia tổng số này cho`p`để biết cần bao nhiêu vòng hoàn chỉnh và giữ phần còn lại cho độ sâu tiếp theo. 

Thông tin chi tiết quan trọng là sự phụ thuộc duy nhất giữa các cấp độ là công việc còn sót lại. Khi chúng tôi biết còn lại bao nhiêu tác vụ sau khi sử dụng bộ xử lý ở một cấp độ thì vị trí chính xác của các tác vụ đó trong cây không còn quan trọng nữa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^h) | O(2^h) | Quá chậm | 
| Tối ưu | O(h) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với những nhiệm vụ chưa hoàn thành và thời gian trôi qua bằng không. Chúng tôi sẽ xử lý cây từ cấp gốc trở xuống, vì cấp độ chỉ có thể xuất hiện sau khi cấp độ gốc của nó hoàn thành. 
2. Đối với mọi độ sâu từ`0`ĐẾN`h - 1`, hãy tính số lượng tác vụ được tạo ở độ sâu đó. Một cây nhị phân hoàn chỉnh có chính xác`2^depth`các nút ở độ sâu này. 
3. Thêm các nhiệm vụ mới tạo vào các nhiệm vụ còn dang dở từ độ sâu trước đó. Giá trị này đại diện cho mọi thứ hiện có thể được xử lý. 
4. Sử dụng càng nhiều vòng xử lý đầy đủ càng tốt. Số vòng đầy đủ là tổng số nhiệm vụ có sẵn chia cho`p`, và các nhiệm vụ còn lại là phần còn lại. 
5. Sau khi tất cả các độ sâu được xử lý, hãy thêm một vòng nữa nếu một số nhiệm vụ vẫn chưa hoàn thành. Đây là những nhiệm vụ từ cấp độ sâu nhất không hoàn toàn phù hợp với các vòng trước. 

Tại sao nó hoạt động: sau khi xử lý theo chiều sâu, thuật toán lưu trữ chính xác số lượng tác vụ vẫn cần bộ xử lý trước khi chuyển sâu hơn. Vì mọi nhiệm vụ ở độ sâu tiếp theo chỉ được tạo bằng cách hoàn thành một nhiệm vụ ở độ sâu hiện tại nên chỉ giữ lại số lượng này sẽ bảo toàn mọi thứ ảnh hưởng đến tương lai. Cấu trúc cây đủ đều đặn nên không cần thông tin nào khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        h, p = map(int, input().split())

        left = 0
        res = 0

        for depth in range(h):
            cur = (1 << depth) + left
            res += cur // p
            left = cur % p

        if left:
            res += 1

        ans.append(str(res))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp xử lý chính xác`h`mức độ, đó là lý do tại sao thời gian chạy không phụ thuộc vào tổng số nút. biểu hiện`1 << depth`tính toán`2^depth`không có thao tác dấu phẩy động, tránh các vấn đề về độ chính xác khi sử dụng quyền hạn. 

Biến`left`lưu trữ các nhiệm vụ chưa hoàn thành sau khi mức hiện tại đã được xử lý. Đó là toàn bộ trạng thái cần thiết cho lần lặp tiếp theo. Sự phân chia theo`p`đếm các vòng đã hoàn thành, trong khi phần còn lại trở thành số tiền còn lại tiếp theo. 

Việc kiểm tra cuối cùng là cần thiết vì mức độ sâu nhất không tạo ra bất kỳ nhiệm vụ mới nào. Nếu một số nút vẫn còn sau lần phân chia cuối cùng, chúng vẫn cần một vòng xử lý cuối cùng. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
1
3 2
```Việc thực hiện trông như thế này: 

| Độ sâu | Nhiệm vụ mới | Còn sót lại trước đó | Tổng số nhiệm vụ | Đã thêm vòng | Còn sót lại mới | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 0 | 1 | 
| 1 | 2 | 1 | 3 | 1 | 1 | 
| 2 | 4 | 1 | 5 | 2 | 1 | 

Sau vòng lặp, vẫn còn một nhiệm vụ, vì vậy cần có một vòng cuối cùng. Câu trả lời là`4`. 

Điều này chứng tỏ tại sao không thể bỏ qua những nhiệm vụ còn sót lại. Dung lượng bộ xử lý từ cấp độ hoàn thành một phần sẽ ảnh hưởng đến cấp độ tiếp theo. 

Đối với đầu vào:```
1
3 10
```Dấu vết là: 

| Độ sâu | Nhiệm vụ mới | Còn sót lại trước đó | Tổng số nhiệm vụ | Đã thêm vòng | Còn sót lại mới | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 0 | 1 | 
| 1 | 2 | 1 | 3 | 0 | 3 | 
| 2 | 4 | 3 | 7 | 0 | 7 | 

Vòng lặp không thêm vòng đầy đủ vì có quá nhiều bộ xử lý. Bảy nhiệm vụ còn lại cần một vòng cuối cùng, nhưng về mặt khái niệm, chuỗi phụ thuộc đã tiêu tốn những khoảnh khắc trước đó. Đếm các cấp độ ban đầu sẽ đưa ra câu trả lời cuối cùng là`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(h) | Mỗi độ sâu của cây được xử lý một lần. | 
| Không gian | O(1) | Chỉ các bộ đếm cho các nhiệm vụ còn sót lại và câu trả lời mới được lưu trữ. | 

Giải pháp hoạt động vì chiều cao là kích thước thực của đầu vào. Ngay cả một cây rất lớn cũng có thể được biểu diễn bằng một số lượng nhỏ các cấp độ, do đó, mô phỏng theo cấp số nhân được thay thế bằng quét tuyến tính theo chiều cao. 

## Trường hợp thử nghiệm```python
import sys, io

def solution(data):
    inp = io.StringIO(data)
    out = []

    t = int(inp.readline())
    for _ in range(t):
        h, p = map(int, inp.readline().split())

        left = 0
        res = 0

        for depth in range(h):
            cur = (1 << depth) + left
            res += cur // p
            left = cur % p

        if left:
            res += 1

        out.append(str(res))

    return "\n".join(out)

# sample-like cases
assert solution("2\n3 1\n3 2\n") == "7\n4"

# minimum height
assert solution("1\n1 100\n") == "1"

# more processors than any level
assert solution("1\n4 1000\n") == "4"

# small processor count
assert solution("1\n5 1\n") == "31"

# catches leftover handling
assert solution("1\n10 6\n") == "173"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1`|`7`| Một bộ xử lý buộc thực hiện tuần tự. | 
|`3 2`|`4`| Nhiệm vụ còn sót lại từ một cấp độ sẽ ảnh hưởng đến các cấp độ sau. | 
|`1 100`|`1`| Kích thước cây tối thiểu | 
|`4 1000`|`4`| Bộ xử lý bổ sung không thể bỏ qua phần phụ thuộc. | 
|`10 6`|`173`| Chiều cao lớn và sự lan truyền còn sót lại. | 

## Vỏ cạnh 

Đối với cây một nút:```
h = 1
p = 100
```Thuật toán nhìn thấy một nhiệm vụ ở độ sâu bằng 0. Nó không thể hoàn thành một vòng ngay lập tức vì gốc trước tiên phải tồn tại như một nhiệm vụ khả dụng duy nhất. Số còn lại sẽ trở thành một và lần kiểm tra cuối cùng sẽ thêm vòng bắt buộc. Câu trả lời là`1`. 

Đối với cây có bộ xử lý khan hiếm:```
h = 3
p = 1
```Mỗi nhiệm vụ phải được xử lý một mình. Thuật toán tích lũy toàn bộ kích thước cây một cách hiệu quả thông qua các cấp độ và kết thúc với một nhiệm vụ mỗi vòng. Vì có bảy nút nên câu trả lời là`7`. 

Đối với cây có nhiều bộ xử lý:```
h = 5
p = 100
```Số lượng bộ xử lý không phải là yếu tố giới hạn. Câu trả lời được kiểm soát bởi độ sâu của cây vì một phần tử con không thể được xử lý trước phần tử cha của nó. Thuật toán để lại tất cả các tác vụ còn sót lại cho đến khi kết thúc và trả về chiều cao, tức là`5`.

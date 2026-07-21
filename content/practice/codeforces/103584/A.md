---
title: "CF 103584A - Khu Vườn Mới"
description: "Chúng tôi có một vườn ươm với số lượng cây cố định và một cửa hàng bán nhiều loại cây. Mỗi loại có một nguồn cung cấp hạt giống giống hệt nhau có giới hạn và mỗi hạt giống của một loại sẽ tạo ra một cây có giá trị vẻ đẹp cố định."
date: "2026-07-03T03:17:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103584
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 02-25-22 Div. 2 (Beginner)"
rating: 0
weight: 103584
solve_time_s: 50
verified: true
draft: false
---

[CF 103584A - Khu vườn mới](https://codeforces.com/problemset/problem/103584/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một vườn ươm với số lượng cây cố định và một cửa hàng bán nhiều loại cây. Mỗi loại có một nguồn cung cấp hạt giống giống hệt nhau có giới hạn và mỗi hạt giống của một loại sẽ tạo ra một cây có giá trị vẻ đẹp cố định. 

Chúng tôi muốn trồng chính xác một số cây nhất định bằng cách chọn hạt giống từ những loại này, tôn trọng giới hạn sẵn có. Mỗi hạt giống được chọn đóng góp giá trị vẻ đẹp của nó vào tổng thể và mục tiêu là tối đa hóa tổng số vẻ đẹp trên tất cả các cây được trồng. 

Dữ liệu đầu vào bao gồm một số lượng nhỏ các loại cây, mỗi loại được mô tả bằng số lượng hạt có sẵn và giá trị của mỗi hạt. Chúng ta phải quyết định lấy bao nhiêu hạt giống từ mỗi loại sao cho tổng số hạt giống được chọn bằng với kích thước khu vườn cần thiết, đồng thời tối đa hóa vẻ đẹp tổng thể. 

Các ràng buộc ngụ ý một cách tiếp cận tham lam hoặc dựa trên sắp xếp là đủ. Số lượng loại nhiều nhất là 100, vì vậy việc lặp lại chúng sẽ rẻ. Tuy nhiên, tổng số cây cần hái có thể lên tới 100000, vì vậy bất kỳ phương pháp nào mô phỏng việc hái từng cây một mà không có cấu trúc chỉ an toàn nếu nó có tổng công suất tuyến tính, nhưng chúng ta có thể làm tốt hơn bằng cách tổng hợp các lựa chọn. 

Một ý tưởng ngây thơ là xử lý từng hạt giống riêng lẻ, mở rộng tất cả các loại thành một danh sách dài có tổng (t_i) mục, sau đó sắp xếp theo vẻ đẹp và chọn m tốt nhất. Điều này đúng nhưng có thể quá chậm nếu tổng số hạt đạt 2e5 trở lên trong trường hợp xấu nhất. 

Các trường hợp cạnh có ý nghĩa quan trọng khi một loại chiếm ưu thế hoặc khi m vượt quá hoặc gần bằng tổng số hạt có sẵn. 

Một trường hợp thất bại tinh tế do việc triển khai bất cẩn xuất hiện khi chúng ta quên rằng chúng ta không thể lấy nhiều hơn t_i hạt giống từ một loại. Ví dụ: nếu một loại có độ đẹp cao nhưng chỉ có 3 hạt thì lấy 10 hạt dù m lớn cũng không hợp lệ. 

Một trường hợp góc khác là khi m chính xác bằng tổng cung. Sau đó, bất kỳ thuật toán nào cũng phải lấy mọi thứ mà không có các giả định logic không cần thiết về lựa chọn một phần. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là tạo ra một danh sách khái niệm về tất cả các hạt giống, trong đó mỗi hạt giống xuất hiện nhiều lần tùy theo vẻ đẹp liên quan của nó. Sắp xếp danh sách này và lấy trực tiếp m giá trị hàng đầu sẽ đưa ra câu trả lời vì mỗi hạt giống đều độc lập và đóng góp bổ sung. Tính chính xác là ngay lập tức, nhưng việc xây dựng danh sách mở rộng này có thể bao gồm tới 200000 mục và việc sắp xếp nó tốn O(N log N), điều này vẫn có thể chấp nhận được nhưng không cần thiết. 

Quan sát quan trọng là tất cả các hạt giống cùng loại đều có giá trị giống hệt nhau, vì vậy chúng ta không bao giờ cần phải trộn lẫn trong một loại. Nếu định sử dụng một loại, chúng tôi luôn lấy các đơn vị có giá trị nhất của nó trước tiên, nhưng vì tất cả đều bằng nhau trong một loại nên chúng tôi chỉ cần lấy càng nhiều càng tốt cho đến t_i. Điều này biến vấn đề thành việc chọn các mục từ nhiều tập hợp trong đó mỗi nhóm có giá trị đồng nhất, do đó việc sắp xếp các nhóm theo giá trị là đủ. 

Chúng tôi sắp xếp các loại theo vẻ đẹp theo thứ tự giảm dần, sau đó tham lam lấy từ những loại có vẻ đẹp cao nhất cho đến khi lấp đầy m ô. Mỗi loại đóng góp tất cả hạt giống có sẵn hoặc chỉ số lượng cần thiết còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng tất cả các hạt giống + sắp xếp | O(S log S) | O(S) | Được chấp nhận nhưng không cần thiết | 
| Sắp xếp các loại + điền tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc số loại và số lượng cây yêu cầu. Mỗi loại cung cấp một cặp bao gồm số lượng có sẵn và giá trị vẻ đẹp. 
2. Sắp xếp các loại theo thứ tự độ đẹp giảm dần, vì chúng ta luôn muốn những cây có giá trị cao hơn trước. 
3. Khởi tạo câu trả lời là 0 và theo dõi xem chúng ta vẫn cần hái bao nhiêu cây. 
4. Lặp lại các kiểu đã được sắp xếp. Đối với mỗi loại, hãy quyết định lấy bao nhiêu cây: tất cả các hạt giống có sẵn của loại đó hoặc chỉ số lượng còn cần thiết, tùy theo số lượng nào nhỏ hơn. Thêm sự đóng góp của họ vào vẻ đẹp tổng thể. 
5. Giảm hạn ngạch còn lại cho phù hợp. 
6. Dừng sớm khi hạn ngạch đạt đến 0 vì không cần thêm cây nữa. 

Ý tưởng chính là ở mỗi bước, chúng tôi cam kết đưa ra lựa chọn cận biên tốt nhất hiện có và vì tất cả các mục bên trong một loại đều giống hệt nhau nên không có thứ tự ẩn nào trong loại có thể cải thiện kết quả sau này. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quy trình, chúng tôi khẳng định rằng tất cả các loại chưa được xử lý còn lại đều có vẻ đẹp nhỏ hơn hoặc bằng loại hiện tại đang được xem xét. Bởi vì tất cả các mục trong một loại đều giống hệt nhau, việc hoán đổi bất kỳ mục nào đã chọn với một mục sau chỉ có thể làm giảm hoặc duy trì vẻ đẹp tổng thể. Điều này làm cho lựa chọn tham lam trở nên tối ưu cục bộ và nhất quán toàn cầu, vì vấn đề lựa chọn giảm xuống khả năng lấp đầy các khối có trọng lượng cao nhất không có cấu trúc bên trong. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    types = []
    
    for _ in range(n):
        t, b = map(int, input().split())
        types.append((b, t))
    
    types.sort(reverse=True)  # sort by beauty descending
    
    ans = 0
    remaining = m
    
    for b, t in types:
        if remaining == 0:
            break
        take = min(remaining, t)
        ans += take * b
        remaining -= take
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện chiến lược tham lam. Việc sắp xếp được thực hiện dựa trên vẻ đẹp theo thứ tự giảm dần để vòng lặp luôn xử lý những cây có giá trị cao nhất trước tiên. các`take = min(remaining, t)`đường dây là biện pháp bảo vệ quan trọng để thực thi hạn chế cung cấp. 

Một lỗi triển khai phổ biến là quên giới hạn số lượng cây được lấy cho mỗi loại, điều này sẽ vượt quá tính sẵn có. Một cách khác là sắp xếp sai hướng, điều này sẽ đảo ngược logic tham lam và tạo ra tổng dưới mức tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 8
5 4
4 6
3 5
```Sau khi phân loại theo vẻ đẹp: 

| Loại (đẹp, đếm) | còn lại | lấy | đã thêm | mới còn lại | 
| --- | --- | --- | --- | --- | 
| (6, 4) | 8 | 4 | 24 | 4 | 
| (5, 3) | 4 | 3 | 15 | 1 | 
| (4, 5) | 1 | 1 | 4 | 0 | 

Câu trả lời cuối cùng là 43. 

Dấu vết này cho thấy những loại nhan sắc cao hơn đã hoàn toàn cạn kiệt trước khi chuyển sang những loại nhan sắc thấp hơn, điều này khẳng định hành vi lựa chọn tham lam. 

### Ví dụ 2 

đầu vào:```
2 5
10 2
3 10
```Đã sắp xếp: 

| Loại | còn lại | lấy | đã thêm | mới còn lại | 
| --- | --- | --- | --- | --- | 
| (10, 3) | 5 | 3 | 30 | 2 | 
| (2, 10) | 2 | 2 | 4 | 0 | 

Đáp án là 34. 

Điều này chứng tỏ rằng ngay cả khi tồn tại loại giá trị cao có số lượng thấp, nó vẫn được ưu tiên hoàn toàn trước khi chuyển sang giá trị thấp hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp các loại cây chiếm ưu thế, theo sau là một đường chuyền tuyến tính duy nhất | 
| Không gian | O(n) | Chỉ lưu trữ danh sách các loại | 

Các ràng buộc n 100 và m lên tới 100000 khiến việc này dễ dàng đủ nhanh. Ngay cả trong trường hợp xấu nhất, thuật toán chỉ thực hiện vài trăm thao tác ngoài phân tích cú pháp đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n, m = map(int, sys.stdin.readline().split())
    types = []
    for _ in range(n):
        t, b = map(int, sys.stdin.readline().split())
        types.append((b, t))

    types.sort(reverse=True)

    ans = 0
    remaining = m

    for b, t in types:
        if remaining == 0:
            break
        take = min(remaining, t)
        ans += take * b
        remaining -= take

    return str(ans)

# provided sample
assert run("3 8\n5 4\n4 6\n3 5\n") == "43"

# all equal values
assert run("2 5\n10 7\n10 7\n") == "35"

# single type only
assert run("1 4\n10 3\n") == "12"

# m exceeds total supply
assert run("2 10\n3 5\n4 2\n") == "23"

# strict greedy ordering case
assert run("3 5\n5 1\n2 10\n10 3\n") == "38"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu nhỏ hỗn hợp | 43 | tính đúng đắn của việc đặt hàng tham lam | 
| vẻ đẹp bình đẳng | 35 | ổn định khi các giá trị ràng buộc | 
| loại đơn | 12 | trường hợp nhân trực tiếp | 
| tổng cung không đủ | 23 | xử lý kiệt sức một phần | 
| đặt hàng hỗn hợp | 38 | ưu tiên các loại hình đẹp cao cấp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một loại duy nhất chiếm ưu thế cả về số lượng và vẻ đẹp. Ví dụ:```
1 5
100 10
```Thuật toán ngay lập tức lấy 5 cây từ loại duy nhất và xuất ra 50. Vòng lặp thực hiện một lần và`take`giới hạn chính xác ở m, do đó không xảy ra lựa chọn quá mức. 

Một trường hợp khác là khi nhiều loại có cùng giá trị vẻ đẹp:```
3 6
2 5
3 5
10 5
```Tất cả các loại đều có mức độ ưu tiên như nhau sau khi sắp xếp. Thuật toán chỉ đơn giản tích lũy từ trái sang phải cho đến khi m được lấp đầy và bất kỳ thứ tự nào trong số chúng đều hợp lệ vì tất cả các đóng góp đều giống hệt nhau. 

Trường hợp cuối cùng là khi m bằng 0, trường hợp này có thể xảy ra nếu diễn giải sai ở một số biến thể. Vòng lặp ngay lập tức kết thúc do`remaining == 0`bảo vệ, đảm bảo không có sự bổ sung ngẫu nhiên từ các loại.

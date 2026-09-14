---
title: "CF 104678C - Sách truyện"
description: "Chúng ta được cung cấp một tập hợp các độ dài câu chuyện, trong đó mỗi câu chuyện có một số trang cố định. Ngoài ra, chúng tôi còn được tặng một số cuốn sách, mỗi cuốn có dung lượng một trang."
date: "2026-06-29T09:05:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104678
codeforces_index: "C"
codeforces_contest_name: "October come back. Together training"
rating: 0
weight: 104678
solve_time_s: 70
verified: true
draft: false
---

[CF 104678C - Sách truyện](https://codeforces.com/problemset/problem/104678/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các độ dài câu chuyện, trong đó mỗi câu chuyện có một số trang cố định. Ngoài ra, chúng tôi còn được tặng một số cuốn sách, mỗi cuốn có dung lượng một trang. Với mỗi cuốn sách, chúng ta muốn biết mình có thể đặt bao nhiêu câu chuyện khác nhau vào bên trong nó nếu chúng ta được tự do chọn bất kỳ tập con câu chuyện nào, nhưng tổng số trang trong tập con đó không được vượt quá sức chứa của cuốn sách. 

Một chi tiết quan trọng là mỗi câu chuyện có thể được sử dụng lại trong nhiều cuốn sách nhưng việc sử dụng lại không tạo ra bất kỳ sự liên kết nào giữa các truy vấn. Mỗi cuốn sách được đánh giá độc lập dựa trên cùng một nhóm câu chuyện. 

Vì vậy, nhiệm vụ giảm xuống, đối với mỗi giá trị dung lượng, tìm số lượng câu chuyện lớn nhất có tổng số trang phù hợp với giới hạn đó. 

Các ràng buộc đẩy chúng ta tới một giải pháp gần tuyến tính hoặc log-tuyến tính. Với tối đa 200.000 câu chuyện và 200.000 truy vấn, bất kỳ phương pháp nào tính toán lại các tập hợp con cho mỗi truy vấn đều sẽ quá chậm. Một lựa chọn tham lam cho mỗi truy vấn ngây thơ đối với tất cả các câu chuyện sẽ có giá O(nk), theo thứ tự hoạt động 4e10 trong trường hợp xấu nhất và không khả thi trong giới hạn 2 giây. Ngay cả việc sắp xếp theo truy vấn cũng sẽ hoàn toàn không khả thi. 

Một cạm bẫy nhỏ xuất phát từ việc hiểu sai vấn đề là cần các tập hợp con tùy ý. Người ta có thể nghĩ rằng các sự kết hợp khác nhau có thể quan trọng, nhưng vì tất cả các câu chuyện đều độc lập và chỉ đóng góp bổ sung nên lựa chọn tối ưu luôn là chọn những câu chuyện nhỏ nhất có sẵn trước tiên. Bất kỳ nỗ lực nào nhằm bao gồm một câu chuyện lớn hơn trong khi loại trừ một câu chuyện nhỏ hơn chỉ có thể làm giảm số lượng câu chuyện có thể đạt được trong cùng một ràng buộc tổng. 

Trường hợp cạnh thứ hai xuất hiện khi dung lượng rất nhỏ. Nếu một cuốn sách có dung lượng nhỏ hơn câu chuyện nhỏ nhất thì câu trả lời phải bằng 0. Một trường hợp góc khác là khi dung lượng cực kỳ lớn, lên tới 10^18, trong đó tất cả các câu chuyện luôn có thể được đưa vào nếu tổng số của chúng phù hợp. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi cuốn sách, hãy thử tất cả các tập hợp con của câu chuyện hoặc ít nhất là mô phỏng việc chọn câu chuyện theo thứ tự nào đó và theo dõi số lượng có thể phù hợp trước khi vượt quá khả năng. Ngay cả khi chúng tôi tối ưu hóa một chút bằng cách sắp xếp các câu chuyện một lần và thêm chúng một cách tham lam, việc thực hiện việc này riêng biệt cho từng truy vấn vẫn dẫn đến hành vi O(nk) vì mỗi cuốn sách có thể quét qua tất cả các câu chuyện. 

Thông tin chi tiết quan trọng về cấu trúc là chúng tôi chỉ quan tâm đến việc có thể đóng gói bao nhiêu câu chuyện nhỏ nhất trước khi vượt quá giới hạn chứ không quan tâm đến câu chuyện cụ thể nào. Sau khi các câu chuyện được sắp xếp theo kích thước, chiến lược tốt nhất để tối đa hóa số lượng dưới một ràng buộc về tổng là luôn sắp xếp chúng theo thứ tự tăng dần. Điều này biến mỗi truy vấn thành một vấn đề tiền tố trên một mảng đã được sắp xếp. 

Nếu chúng ta tính trước tổng tiền tố theo độ dài câu chuyện đã được sắp xếp thì mỗi truy vấn sẽ trở thành: tìm tiền tố lớn nhất có tổng không vượt quá dung lượng sách. Đây là một điều kiện đơn điệu cổ điển có thể được giải quyết bằng tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(1) | Quá chậm | 
| Sắp xếp + Tiền tố + Tìm kiếm nhị phân | O(n log n + k log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả độ dài câu chuyện theo thứ tự không giảm. Điều này đảm bảo rằng việc lấy các câu chuyện theo trình tự luôn mang lại tổng tích lũy nhỏ nhất có thể cho bất kỳ số lượng câu chuyện cố định nào. 
2. Xây dựng mảng tổng tiền tố trên các câu chuyện đã được sắp xếp. Mỗi vị trí lưu trữ tổng số trang cần thiết để đưa tất cả các câu chuyện vào chỉ mục đó. 
3. Đối với mỗi dung lượng sách, chúng tôi muốn tìm độ dài tiền tố tối đa sao cho tổng tiền tố nhỏ hơn hoặc bằng dung lượng. 
4. Sử dụng tìm kiếm nhị phân trên mảng tổng tiền tố để xác định vị trí ngoài cùng bên phải nơi tổng không vượt quá giới hạn của sách. Mục lục của vị trí đó là câu trả lời cho cuốn sách đó. 
5. Xuất tất cả các câu trả lời theo thứ tự như các truy vấn đầu vào.

Lý do tìm kiếm nhị phân hoạt động ở đây là mảng tổng tiền tố đang tăng lên nghiêm ngặt, vì tất cả độ dài câu chuyện đều dương. Điều này đảm bảo một vị từ đơn điệu: khi tổng tiền tố vượt quá khả năng, tất cả các tiền tố dài hơn cũng sẽ vượt quá nó. 

### Tại sao nó hoạt động 

Tập hợp con tối ưu để tối đa hóa số lượng theo ràng buộc tổng phải luôn bao gồm các phần tử có sẵn nhỏ nhất. Bất kỳ sai lệch nào thay thế câu chuyện nhỏ hơn bằng câu chuyện lớn hơn sẽ làm giảm số lượng yếu tố bạn có thể phù hợp mà không cải thiện tính khả thi. Điều này làm cho cấu trúc tiền tố được sắp xếp không chỉ vừa đủ mà còn tối ưu, và tính đơn điệu của tổng tiền tố đảm bảo rằng tìm kiếm nhị phân tìm thấy điểm giới hạn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    a.sort()
    
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]
    
    import bisect
    
    res = []
    for cap in b:
        ans = bisect.bisect_right(pref, cap) - 1
        res.append(str(ans))
    
    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp độ dài câu chuyện sao cho những câu chuyện nhỏ hơn được xem xét trước. Mảng tổng tiền tố chuyển đổi vấn đề “có bao nhiêu mục phù hợp với một ràng buộc tổng” thành một truy vấn phạm vi trên một mảng duy nhất. Việc sử dụng`bisect_right`trực tiếp tìm tổng tiền tố đầu tiên lớn hơn dung lượng và trừ đi một sẽ cho độ dài tiền tố hợp lệ cuối cùng. 

Một chi tiết triển khai tinh tế là mảng tiền tố bắt đầu bằng số 0 ở chỉ số 0, thể hiện việc không chọn câu chuyện nào. Điều này đảm bảo rằng ngay cả những công suất rất nhỏ cũng ánh xạ chính xác tới tầng 0 mà không cần vỏ bọc đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
8 2 3 30
5 29 1
```Sau khi sắp xếp câu chuyện:`[2, 3, 8, 30]`Tổng tiền tố:`[0, 2, 5, 13, 43]`| Dung lượng sách | Kết quả tìm kiếm nhị phân (chỉ mục tiền tố) | Trả lời | 
| --- | --- | --- | 
| 5 | 2 | 2 | 
| 29 | 3 | 3 | 
| 1 | 0 | 0 | 

Truy vấn đầu tiên phù hợp với câu chuyện 2 và 3 (tổng 5). Tầng thứ hai có thể chứa được ba tầng (2 + 3 + 8 = 13). Dung lượng cuối cùng quá nhỏ để chứa được ngay cả câu chuyện nhỏ nhất. 

Điều này xác nhận rằng lựa chọn tham lam dựa trên tiền tố phù hợp với lựa chọn tập hợp con tối ưu. 

### Ví dụ 2 

đầu vào:```
5 2
4 1 10 2 7
3 15
```Truyện được sắp xếp:`[1, 2, 4, 7, 10]`Tổng tiền tố:`[0, 1, 3, 7, 14, 24]`| Dung lượng sách | Chỉ số tiền tố | Trả lời | 
| --- | --- | --- | 
| 3 | 2 | 2 | 
| 15 | 4 | 4 | 

Đối với sức chứa 3, chúng ta có thể lấy 1 và 2. Đối với sức chứa 15, chúng ta có thể lấy 1, 2, 4 và 7, nhưng không thể lấy 10 vì nó sẽ vượt quá giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + k log n) | Sắp xếp chiếm ưu thế, mỗi truy vấn sử dụng tìm kiếm nhị phân | 
| Không gian | O(n) | Mảng tổng tiền tố và danh sách được sắp xếp | 

Các ràng buộc cho phép lên tới 200.000 phần tử và độ phức tạp này phù hợp thoải mái trong các giới hạn điển hình. Sắp xếp một lần và trả lời từng truy vấn theo thời gian logarit đảm bảo giải pháp vẫn hiệu quả ngay cả trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    a.sort()
    pref = [0]
    for x in a:
        pref.append(pref[-1] + x)

    import bisect
    res = []
    for cap in b:
        res.append(str(bisect.bisect_right(pref, cap) - 1))
    return " ".join(res)

# provided sample
assert run("4 3\n8 2 3 30\n5 29 1\n") == "2 3 0"

# minimum size
assert run("1 1\n5\n5\n") == "1"

# cannot take anything
assert run("3 2\n10 20 30\n1 5\n") == "0 0"

# all equal
assert run("4 2\n3 3 3 3\n6 12\n") == "2 4"

# large increasing capacities
assert run("5 3\n1 2 3 4 5\n3 6 15\n") == "2 3 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | tính đúng đắn của trường hợp tối thiểu | 
| giá trị lớn quá nhỏ | 0 0 | trường hợp không có cạnh lựa chọn | 
| tất cả đều bình đẳng | 2 4 | xử lý trùng lặp và tổng tiền tố | 
| tăng năng lực | 2 3 5 | hành vi tích lũy lũy tiến | 

## Vỏ cạnh 

Khi tất cả các câu chuyện đều lớn hơn dung lượng sách nhất định, việc sắp xếp sẽ đảm bảo câu chuyện nhỏ nhất vẫn quá lớn. Trong trường hợp đó, tổng tiền tố cho phần tử đầu tiên đã vượt quá dung lượng và tìm kiếm nhị phân trả về chỉ số 0, mang lại chính xác không có câu chuyện nào. 

Đối với dung lượng rất lớn, chẳng hạn như các giá trị gần 10^18, mảng tổng tiền tố vẫn xử lý chúng một cách chính xác vì tổng tổng nằm trong phạm vi 64 bit. Tìm kiếm nhị phân chỉ trả về độ dài tiền tố đầy đủ, nghĩa là tất cả các câu chuyện đều có thể được đưa vào. 

Khi chỉ có một câu chuyện, mảng tiền tố sẽ trở thành`[0, a1]`. Bất kỳ công suất nào lớn hơn hoặc bằng`a1`mang lại một câu chuyện, nếu không thì bằng 0, và tìm kiếm nhị phân tái tạo hành vi này một cách tự nhiên mà không cần xử lý đặc biệt.

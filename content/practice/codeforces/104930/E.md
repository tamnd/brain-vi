---
title: "CF 104930E - Kết hợp lên xuống"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, một hàng người đứng theo một thứ tự cố định, trong đó mỗi người đến từ Uptown hoặc Downside."
date: "2026-06-28T07:42:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104930
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 01-26-24 Div. 2 (Beginner)"
rating: 0
weight: 104930
solve_time_s: 64
verified: false
draft: false
---

[CF 104930E - So khớp từ trên xuống](https://codeforces.com/problemset/problem/104930/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, một hàng người đứng theo một thứ tự cố định, trong đó mỗi người đến từ Uptown hoặc Downside. Chúng tôi được phép chọn một phân khúc liền kề của dòng này và chúng tôi muốn phân khúc đó được “cân bằng”, nghĩa là nó chứa chính xác cùng một số lượng Người Uptownites và Người Downsiders. Trong số tất cả các đoạn cân bằng như vậy, chúng ta cần độ dài tối đa có thể. 

Do đó, nhiệm vụ chính cho mỗi trường hợp kiểm thử không phải là tìm bất kỳ phân đoạn cân bằng nào mà là tìm chuỗi con dài nhất trong đó số lượng`U`Và`D`đều bình đẳng. 

Các ràng buộc đủ chặt chẽ đến mức bất kỳ phương pháp nào kiểm tra trực tiếp tất cả các chuỗi con đều không khả thi. Với tổng chiều dài lên tới 200.000 trong các trường hợp thử nghiệm, việc quét bậc hai cho mỗi trường hợp thử nghiệm sẽ dẫn đến thứ tự 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn 2 giây trong Python. 

Một số trường hợp đặc biệt quan trọng về mặt cấu trúc: 

Ví dụ: một chuỗi bao gồm toàn bộ một ký tự`UUUUU`, không có phân đoạn hợp lệ có độ dài dương, vì vậy câu trả lời phải là 0. Một cách tiếp cận đơn giản giả sử tồn tại ít nhất một cặp hợp lệ sẽ trả về không chính xác ít nhất 2. 

Một chuỗi xen kẽ đầy đủ như`UDUDUD`sẽ trả về độ dài đầy đủ, vì mọi tiền tố có thể được cân bằng tại một số điểm, nhưng chỉ sau khi theo dõi số dư tích lũy một cách chính xác. 

Một trường hợp như`UUDDUD`có thể có nhiều phân đoạn hợp lệ có độ dài khác nhau và chúng tôi phải đảm bảo rằng chúng tôi không chỉ lấy tiền tố cân bằng đầu tiên. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi phân đoạn có thể`[l, r]`, đếm xem có bao nhiêu`U`Và`D`xuất hiện và kiểm tra xem chúng có khớp không. Nếu có, hãy cập nhật câu trả lời với`r - l + 1`. Điều này đúng vì nó đánh giá rõ ràng tất cả các ứng cử viên. 

Vấn đề là hiệu suất. Đối với mỗi chỉ số bắt đầu`l`, chúng tôi quét tất cả`r > l`và tính toán lại số lượng hoặc ngay cả khi chúng tôi duy trì số lượng đang chạy, chúng tôi vẫn kiểm tra các phân đoạn O(n^2) cho mỗi trường hợp thử nghiệm. Với tổng chiều dài lên tới 2·10^5, điều này trở nên quá chậm. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến số lượng thô của`U`Và`D`, nhưng chỉ có sự khác biệt của họ. Nếu chúng ta lập bản đồ`U`đến +1 và`D`đến -1 thì đoạn đó được cân bằng chính xác khi tổng của nó bằng 0. Điều này chuyển vấn đề thành việc tìm mảng con dài nhất có tổng bằng 0. 

Đây là cấu trúc tổng tiền tố cổ điển. Nếu chúng ta định nghĩa`prefix[i]`như tổng hợp chỉ số`i`, thì một đoạn`[l, r]`có tổng bằng 0 khi và chỉ khi`prefix[r] == prefix[l-1]`. Vì vậy, vấn đề trở thành tìm hai giá trị tiền tố bằng nhau với khoảng cách tối đa giữa các chỉ số của chúng. 

Chúng ta có thể giải quyết vấn đề này bằng cách lưu trữ vị trí sớm nhất nơi mỗi giá trị tổng tiền tố xuất hiện. Khi chúng tôi nhìn thấy tổng tiền tố tương tự một lần nữa, chúng tôi tính toán khoảng cách từ lần xuất hiện đầu tiên của nó và cập nhật câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tổng tiền tố + hashmap | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Chuyển chuỗi thành số dư hiện hành bằng cách xử lý`U`là +1 và`D`là -1. Chúng tôi duy trì một biến`pref`bắt đầu từ 0. Điều này chuyển vấn đề thành việc phát hiện các tổng tiền tố bằng nhau. 
2. Khởi tạo từ điển`first_pos`ánh xạ các giá trị tổng tiền tố tới chỉ mục sớm nhất nơi chúng xuất hiện. Chúng tôi thiết lập`first_pos[0] = 0`trước khi xử lý, vì tổng bằng 0 trước khi bắt đầu cho phép các mảng con bắt đầu từ chỉ số 1. 
3. Lặp lại chuỗi từ trái sang phải, cập nhật`pref`ở mỗi vị trí. Sau khi xử lý vị trí`i`,`pref`đại diện cho sự cân bằng của tiền tố kết thúc tại`i`. 
4. Nếu`pref`đã được nhìn thấy trước đó, hãy tính độ dài đoạn`i - first_pos[pref]`và cập nhật câu trả lời nếu câu trả lời này lớn hơn. Điều này có tác dụng vì tổng tiền tố bằng nhau hàm ý một phân đoạn có tổng bằng 0 giữa các chỉ số của chúng. 
5. Nếu`pref`chưa từng thấy trước đây, cửa hàng`first_pos[pref] = i`. Chúng tôi chỉ lưu trữ lần xuất hiện đầu tiên để tối đa hóa khoảng cách sau này, vì lần xuất hiện sau sẽ chỉ rút ngắn các đoạn có thể có. 
6. Sau khi xử lý chuỗi đầy đủ, câu trả lời là độ dài đoạn cân bằng tối đa được tìm thấy. 

### Tại sao nó hoạt động 

Phép biến đổi tổng tiền tố mã hóa toàn bộ vấn đề thành các so sánh đẳng thức. Mỗi phân đoạn cân bằng tương ứng chính xác với hai chỉ số có cùng giá trị tổng tiền tố. Bằng cách theo dõi sự xuất hiện sớm nhất của mỗi tổng tiền tố, chúng tôi đảm bảo rằng bất cứ khi nào chúng tôi xem lại tổng đó, chúng tôi đang hình thành phân đoạn dài nhất có thể kết thúc ở chỉ mục hiện tại với số dư ròng bằng 0. Điều này đảm bảo rằng mọi phân đoạn hợp lệ sẽ được xem xét chính xác một lần và phân đoạn dài nhất trong số đó sẽ được ghi lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input().strip())
        s = input().strip()

        first_pos = {0: 0}
        pref = 0
        ans = 0

        for i, ch in enumerate(s, start=1):
            if ch == 'U':
                pref += 1
            else:
                pref -= 1

            if pref in first_pos:
                ans = max(ans, i - first_pos[pref])
            else:
                first_pos[pref] = i

        print(ans)

if __name__ == "__main__":
    solve()
```Lựa chọn triển khai cốt lõi là lập chỉ mục dựa trên 1 trong vòng lặp. Điều này phù hợp một cách tự nhiên với định nghĩa tiền tố trong đó vị trí 0 đại diện cho tiền tố trống, cho phép trừ rõ ràng`i - first_pos[pref]`mà không cần điều chỉnh từng cái một. 

Chúng tôi cũng tránh cập nhật`first_pos`sau lần xuất hiện đầu tiên, đó là điều cần thiết. Việc ghi đè nó sẽ phá hủy khả năng hình thành các đoạn có độ dài tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
UDUD
```Chúng tôi theo dõi tổng tiền tố từng bước: 

| tôi | char | trước | đầu tiên_pos | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | - | 0 | {0:0} | 0 | 
| 1 | Bạn | 1 | {0:0,1:1} | 0 | 
| 2 | D | 0 | {0:0,1:1} | 2 | 
| 3 | Bạn | 1 | {0:0,1:1} | 2 | 
| 4 | D | 0 | {0:0,1:1} | 4 | 

Tổng tiền tố trở về 0 tại vị trí 2 và 4, tạo ra các phân đoạn`[1,2]`Và`[1,4]`, với phân đoạn đầy đủ là hợp lệ. 

Điều này chứng tỏ các tổng tiền tố lặp lại tự nhiên nắm bắt được nhiều phân đoạn cân bằng hợp lệ như thế nào và thuật toán tự động giữ lại phân đoạn dài nhất. 

### Ví dụ 2 

đầu vào:```
UUDDUD
```| tôi | char | trước | đầu tiên_pos | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | - | 0 | {0:0} | 0 | 
| 1 | Bạn | 1 | {0:0,1:1} | 0 | 
| 2 | Bạn | 2 | {0:0,1:1,2:2} | 0 | 
| 3 | D | 1 | {0:0,1:1,2:2} | 2 | 
| 4 | D | 0 | {0:0,1:1,2:2} | 4 | 
| 5 | Bạn | 1 | {0:0,1:1,2:2} | 4 | 
| 6 | D | 0 | {0:0,1:1,2:2} | 6 | 

Điều này cho thấy nhiều phân đoạn cân bằng chồng chéo, trong đó chỉ theo dõi những lần xuất hiện đầu tiên để đảm bảo chúng tôi vẫn nắm bắt được mức tối đa toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần với các thao tác từ điển O(1) | 
| Không gian | O(n) | Bản đồ tổng tiền tố lưu trữ tối đa n giá trị riêng biệt | 

Tổng công việc trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, vừa vặn thoải mái trong giới hạn 2·10^5 ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        first_pos = {0: 0}
        pref = 0
        ans = 0

        for i, ch in enumerate(s, start=1):
            pref += 1 if ch == 'U' else -1
            if pref in first_pos:
                ans = max(ans, i - first_pos[pref])
            else:
                first_pos[pref] = i

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("4\n4\nUDUD\n7\nUUUDDDD\n10\nDDUDDUDUUD\n2\nDD\n") == "4\n6\n8\n0"

# custom cases
assert run("1\n1\nU\n") == "0", "single element"
assert run("1\n6\nUUUUUU\n") == "0", "all same"
assert run("1\n6\nUDUDUD\n") == "6", "fully alternating"
assert run("1\n8\nUUDDUDUD\n") == "8", "multiple valid segments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`U`|`0`| trường hợp cạnh có chiều dài tối thiểu | 
|`UUUUUU`|`0`| không có đoạn cân bằng hợp lệ | 
|`UDUDUD`|`6`| cân xen kẽ toàn chiều dài | 
|`UUDDUDUD`|`8`| các mảng con hợp lệ chồng chéo | 

## Vỏ cạnh 

Một chuỗi không có`U`hoặc không`D`là trường hợp suy biến quan trọng nhất. Đối với đầu vào`DDDD`, tổng tiền tố chỉ giảm, không bao giờ lặp lại, do đó từ điển không bao giờ tạo ra kết quả khớp và câu trả lời vẫn là 0. Thuật toán tránh được kết quả dương tính giả một cách chính xác vì sự bằng nhau của tổng tiền tố không bao giờ xảy ra sau số 0 ban đầu. 

Một trường hợp như`UDUDUD`liên tục quay trở lại tổng tiền tố đã thấy trước đó. Mỗi kết quả trả về tạo ra một phân đoạn ứng cử viên kết thúc ở chỉ mục hiện tại và vì chỉ lần xuất hiện sớm nhất mới được lưu trữ nên phân đoạn dài nhất kết thúc ở mỗi vị trí luôn được xem xét. 

Một trường hợp có khối hỗn hợp như`UUDDUD`chứng minh rằng đoạn tối ưu có thể bắt đầu và kết thúc ở giữa chuỗi, không nhất thiết phải căn chỉnh với ranh giới khối. Đẳng thức tổng tiền tố xử lý việc này một cách tự nhiên mà không cần viết hoa đặc biệt.

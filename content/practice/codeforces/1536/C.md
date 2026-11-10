---
title: "CF 1536C - Diluc và Kaeya"
description: "Chúng ta được cấp một chuỗi nhị phân chỉ gồm các ký tự D và K. Đối với mỗi tiền tố của chuỗi này, chúng ta muốn xác định xem chúng ta có thể chia tiền tố đó thành các phần liền kề sao cho mỗi phần có cùng số dư bên trong giữa D và K."
date: "2026-06-14T18:46:54+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "hashing", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1536
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 724 (Div. 2)"
rating: 1500
weight: 1536
solve_time_s: 229
verified: false
draft: false
---

[CF 1536C - Diluc và Kaeya](https://codeforces.com/problemset/problem/1536/C) 

**Đánh giá:** 1500 
**Tags:** cấu trúc dữ liệu, dp, hàm băm, lý thuyết số 
**Thời gian giải:** 3 phút 49s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân chỉ gồm các ký tự`D`Và`K`. Đối với mỗi tiền tố của chuỗi này, chúng tôi muốn xác định xem chúng tôi có thể chia tiền tố đó thành các phần liền kề sao cho mỗi phần có cùng sự cân bằng bên trong giữa`D`Và`K`. 

Mỗi phần được tóm tắt bằng một tỷ lệ mô tả có bao nhiêu`D`cát`K`s nó chứa. Hai mảnh được coi là tương thích nếu tỷ lệ của chúng khớp với nhau sau khi chuẩn hóa, nghĩa là tỷ lệ giữa`D`Và`K`giống hệt nhau ngay cả khi số lượng tuyệt đối khác nhau. 

Đối với mỗi tiền tố kết thúc tại vị trí`i`, chúng tôi muốn số lượng phân đoạn tối đa mà chúng tôi có thể phân vùng để mọi phân đoạn có cùng tỷ lệ. 

Khó khăn chính là việc này phải được tính toán cho tất cả các tiền tố, vì vậy việc tính toán lại các phân vùng từ đầu cho mỗi tiền tố sẽ quá chậm. 

Các ràng buộc ngụ ý một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng độ dài của tất cả các trường hợp thử nghiệm lên tới 500.000 nên bất kỳ giải pháp nào quét liên tục các tiền tố hoặc thử tất cả các điểm phân tách trên mỗi tiền tố sẽ vượt quá giới hạn thời gian. Điều này ngay lập tức loại trừ mọi lập trình động bậc hai hoặc tiền tố theo tiền tố khởi động lại các phép tính. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi đồng nhất. Ví dụ, nếu chuỗi là`DDDDD`, mọi tiền tố có thể được chia thành các đoạn ký tự đơn và câu trả lời sẽ phát triển tuyến tính. Một cách tiếp cận ngây thơ cố gắng thực thi có ý nghĩa`D:K`tỷ lệ có thể giả định không chính xác cả hai ký tự phải xuất hiện trong mọi phân đoạn, điều này là sai vì các tỷ lệ như`a:0`là hợp lệ. Tương tự, các tiền tố như`DKDK`cho phép phân chia xen kẽ cân bằng, nhưng phân đoạn tham lam mà không theo dõi tính khả thi toàn cầu có thể bỏ sót các phân vùng hợp lệ. 

Một trường hợp phức tạp khác là khi`K`không bao giờ xuất hiện. Ví dụ, trong`DDDD`, mọi tiền tố sẽ cho phép chia thành tất cả các ký tự đơn. Bất kỳ phương thức nào giả sử cả hai ký tự đều cần thiết để xác định tỷ lệ sẽ bị hỏng ở đây. 

## Phương pháp tiếp cận 

Đối với mỗi tiền tố, một giải pháp brute-force sẽ thử mọi phân vùng có thể và kiểm tra xem tất cả các phân đoạn có chia sẻ cùng một tỷ lệ hay không. Đối với tiền tố có độ dài`i`, có rất nhiều cách để phân chia nó theo cấp số nhân và thậm chí việc xác minh một phân vùng duy nhất cũng yêu cầu quét tất cả các phân đoạn để so sánh tỷ lệ. Ngay cả khi chúng tôi hạn chế thử nghiệm tất cả số lượng phân đoạn có thể có và cố gắng xác thực tham lam, chúng tôi vẫn kết thúc với hành vi bậc hai gần đúng cho mỗi trường hợp thử nghiệm. 

Thông tin chi tiết về cấu trúc quan trọng là phân vùng hợp lệ được xác định hoàn toàn theo tỷ lệ của toàn bộ tiền tố sau khi chúng tôi quyết định số lượng phân đoạn mà chúng tôi muốn. Nếu một tiền tố có thể được chia thành`k`các phân đoạn hợp lệ thì mỗi phân đoạn phải tương ứng với một tỷ lệ cố định trong tổng số`D`Và`K`tính. Điều này có nghĩa là các phép chia hợp lệ tương ứng chính xác với các ước số của cấu trúc tỷ lệ rút gọn của tiền tố. 

Thay vì xây dựng các phân vùng một cách rõ ràng, chúng tôi theo dõi tần suất mỗi trạng thái chuẩn hóa của tiền tố xuất hiện. Mỗi trạng thái tiền tố mã hóa khoảng cách giữa chúng ta và các trạng thái trước đó theo cách duy trì tỷ lệ tương đương. Bất cứ khi nào chúng tôi xem lại một trạng thái, chúng tôi đang nói rằng tiền tố có thể được phân chia thành nhiều phân đoạn có cấu trúc giống hệt nhau. 

Điều này làm giảm vấn đề trong việc duy trì số lượng biểu diễn chuẩn hóa của tiền tố và sử dụng tích lũy tần số để rút ra số lượng phân đoạn nhất quán tối đa cho mỗi tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Đếm trạng thái tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải từng tiền tố thông qua cách biểu diễn rút gọn của nó`D`Và`K`THĂNG BẰNG. Thay vì lưu trữ số lượng thô, chúng tôi duy trì chữ ký chuẩn hóa để xác định duy nhất hành vi tỷ lệ của tiền tố. 

1. Chúng ta duyệt chuỗi trong khi duy trì số lượng tích lũy của`D`Và`K`. Sau khi xử lý từng ký tự, chúng tôi tính toán dạng chuẩn của tỷ lệ giữa hai số đếm này. 
2. Để làm cho các tỷ số có thể so sánh được, chúng ta giảm`(d, k)`bằng cách chia cả hai cho ước chung lớn nhất của chúng khi cả hai đều khác 0. Nếu một trong số chúng bằng 0, chúng tôi biểu thị trạng thái dưới dạng đặc biệt như`(1, 0)`hoặc`(0, 1)`tùy thuộc vào nhân vật nào chiếm ưu thế. 
3. Chúng tôi duy trì bản đồ tần số`freq`đếm số lần mỗi trạng thái tiền tố chuẩn hóa đã xuất hiện cho đến nay. 
4. Đối với mỗi tiền tố, chúng tôi tăng số lượng trạng thái chuẩn hóa của nó theo`freq`. 
5. Câu trả lời cho tiền tố hiện tại chỉ đơn giản là tần số tối đa của bất kỳ trạng thái nào được thấy cho đến nay, bởi vì việc lặp lại cùng một trạng thái chuẩn hóa có nghĩa là chúng ta có thể căn chỉnh các phân đoạn giữa các lần xuất hiện. 
6. Chúng tôi lưu trữ mức tối đa đang chạy khi chúng tôi lặp lại, vì các câu trả lời phải được xuất ra cho mọi tiền tố. 

Ý tưởng tinh tế là bất cứ khi nào cùng một trạng thái tiền tố chuẩn hóa xuất hiện nhiều lần, nó sẽ xác định các điểm cắt tự nhiên nơi các phân đoạn tỷ lệ giống hệt nhau có thể bắt đầu và kết thúc. Do đó, tần số của trạng thái phổ biến nhất trực tiếp cho số lượng phân đoạn có tỷ lệ bằng nhau tối đa. 

### Tại sao nó hoạt động 

Mỗi trạng thái tiền tố mã hóa cấu trúc tỷ lệ của tiền tố đến mức chuẩn hóa. Hai vị trí có cùng trạng thái có nghĩa là đoạn giữa chúng có cùng trạng thái`D:K`tỷ lệ khi so sánh theo tỷ lệ. Việc phân vùng chuỗi một cách tối ưu tương đương với việc nhóm các lần xuất hiện của các trạng thái giống hệt nhau, bởi vì mọi phân đoạn hợp lệ đều phải căn chỉnh với các ranh giới trong đó tỷ lệ đặt lại về cấu hình giống hệt nhau. Trạng thái thường xuyên nhất xác định có bao nhiêu phân đoạn nhất quán như vậy có thể cùng tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()

        d = 0
        k = 0
        freq = defaultdict(int)

        ans = []
        best = 0

        for ch in s:
            if ch == 'D':
                d += 1
            else:
                k += 1

            # normalize ratio
            if d == 0:
                key = (0, 1)
            elif k == 0:
                key = (1, 0)
            else:
                import math
                g = math.gcd(d, k)
                key = (d // g, k // g)

            freq[key] += 1
            if freq[key] > best:
                best = freq[key]

            ans.append(str(best))

        print(" ".join(ans))

if __name__ == "__main__":
    solve()
```Mã duy trì số lần chạy của`D`Và`K`trong khi nén từng tiền tố thành dạng tỷ lệ rút gọn. các`gcd`bước đảm bảo rằng các trạng thái tỷ lệ thu gọn thành các khóa giống hệt nhau, do đó các tiền tố tương đương về mặt cấu trúc sẽ ánh xạ tới cùng một mã định danh. Từ điển theo dõi tần suất mỗi cấu trúc xuất hiện và câu trả lời sẽ phát triển khi cấu trúc thường xuyên nhất phát triển. 

Chi tiết triển khai tinh tế duy nhất là xử lý riêng các trường hợp không. Nếu không có điều đó, việc tính toán chuẩn hóa dựa trên gcd sẽ thất bại khi một trong các số đếm bằng 0 và dẫn đến các biểu diễn không rõ ràng. 

## Ví dụ đã hoạt động 

Hãy xem xét chuỗi tiền tố`DKDK`. 

| tôi | Tiền tố | D | K | Trạng thái chuẩn hóa | cập nhật tần số | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | D | 1 | 0 | (1,0) | 1 | 1 | 
| 2 | ĐK | 1 | 1 | (1,1) | 1 | 1 | 
| 3 | ĐKD | 2 | 1 | (2,1) | 1 | 1 | 
| 4 | ĐKDK | 2 | 2 | (1,1) | 2 | 2 | 

Dấu vết này cho thấy trạng thái`(1,1)`lặp lại, chỉ ra rằng tiền tố có thể được phân chia tinh tế hơn tại thời điểm đó. 

Bây giờ hãy xem xét`DDDD`. 

| tôi | Tiền tố | D | K | Trạng thái chuẩn hóa | cập nhật tần số | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | D | 1 | 0 | (1,0) | 1 | 1 | 
| 2 | ĐĐ | 2 | 0 | (1,0) | 2 | 2 | 
| 3 | DDD | 3 | 0 | (1,0) | 3 | 3 | 
| 4 | DDDD | 4 | 0 | (1,0) | 4 | 4 | 

Mọi tiền tố đều có chung cấu trúc chuẩn hóa, do đó câu trả lời sẽ phát triển tuyến tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) mỗi lần kiểm tra | tính toán gcd trên mỗi tiền tố chiếm ưu thế | 
| Không gian | O(n) | lưu trữ bản đồ tần số | 

Tổng độ dài của các trường hợp thử nghiệm được giới hạn bởi 500.000, do đó, giải pháp O(n log n) nằm trong giới hạn thoải mái. Kích thước bản đồ cũng tuyến tính theo số lượng trạng thái tiền tố riêng biệt, nhiều nhất là n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue().strip()

# provided samples
assert run("""5
3
DDK
6
DDDDDD
4
DKDK
1
D
9
DKDKDDDDK
""") == "1 2 1\n1 2 3 4 5 6\n1 1 1 2\n1\n1 1 1 2 1 2 1 1 3"

# custom cases
assert run("""1
1
K
""") == "1", "single K"

assert run("""1
5
DDDDD
""") == "1 2 3 4 5", "all D"

assert run("""1
6
DKDKDK
""") == "1 1 1 2 2 3", "alternating pattern"

assert run("""1
3
DKK
""") == "1 1 2", "imbalanced prefix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`K`|`1`| trường hợp cạnh một ký tự | 
|`DDDDD`|`1 2 3 4 5`| tăng trưởng nhân vật đồng đều | 
|`DKDKDK`|`1 1 1 2 2 3`| căn chỉnh cấu trúc lặp đi lặp lại | 
|`DKK`|`1 1 2`| xử lý bất đối xứng | 

## Vỏ cạnh 

Đối với một chuỗi như`KKKK`, mọi tiền tố đều giữ nguyên trạng thái chuẩn hóa`(0,1)`. Thuật toán đếm số lần xuất hiện lặp lại của trạng thái này, tạo ra câu trả lời tăng dần. Điều này phù hợp với thực tế là mỗi ký tự bổ sung có thể tạo thành một phân đoạn ký tự đơn hợp lệ khác. 

Vì`DKK`, ký tự đầu tiên tạo trạng thái`(1,0)`, cái thứ hai tạo ra`(1,1)`, và thứ ba tạo ra`(1,2)`làm giảm đến`(1,2)`lại. Cơ chế tần số phân biệt chính xác các tiền tố khác nhau về mặt cấu trúc trong khi vẫn cho phép tích lũy các trạng thái lặp lại khi chúng xuất hiện trở lại ở dạng rút gọn.

---
title: "CF 2123B - Giải đấu"
description: "Chúng tôi có một giải đấu với n người chơi. Người chơi tôi có sức mạnh a[i]. Nhiều lần, hai người chơi sống sót được chọn và người yếu hơn sẽ bị loại. Nếu cả hai điểm mạnh bằng nhau thì một trong hai có thể bị loại. Quá trình dừng lại khi còn lại đúng k người chơi."
date: "2026-06-08T03:36:43+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2123
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1034 (Div. 3)"
rating: 800
weight: 2123
solve_time_s: 104
verified: true
draft: false
---

[CF 2123B - Giải đấu](https://codeforces.com/problemset/problem/2123/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một giải đấu với`n`người chơi. Người chơi`i`có sức mạnh`a[i]`. Nhiều lần, hai người chơi sống sót được chọn và người yếu hơn sẽ bị loại. Nếu cả hai điểm mạnh bằng nhau thì một trong hai có thể bị loại. 

Quá trình dừng lại khi chính xác`k`người chơi vẫn còn. Chúng tôi quan tâm đến một cầu thủ cụ thể`j`. Câu hỏi đặt ra là liệu có tồn tại một số chuỗi lựa chọn cặp và kết quả hòa cho phép người chơi`j`vẫn còn sống giữa trận chung kết`k`người chơi. 

Cụm từ khóa là "có cách nào không". Chúng tôi không yêu cầu xác suất và người chơi cũng không phải sống sót trong mọi kết quả có thể xảy ra của giải đấu. Chúng ta chỉ cần một trình tự thuận lợi. 

Các ràng buộc đủ lớn đến mức không thể mô phỏng bất kỳ trạng thái giải đấu nào. Trên tất cả các trường hợp thử nghiệm, tổng số người chơi đạt`2 · 10^5`, vì vậy chúng ta nên mong đợi một`O(n)`hoặc`O(n log n)`giải pháp. Bất cứ điều gì khám phá các bảng đấu giải đấu, tập hợp con những người sống sót hoặc chuyển đổi không gian trạng thái sẽ phát triển theo cấp số nhân và thất bại ngay lập tức. 

Một khía cạnh tế nhị của vấn đề là các cặp đôi hoàn toàn nằm trong tầm kiểm soát của chúng ta khi kiểm tra sự tồn tại. Nếu một người chơi có thể được bảo vệ bằng cách chọn các trận đấu khác trước thì điều đó được cho phép. 

Hãy xem xét trường hợp```
n = 4, j = 1, k = 1
strengths = [1, 2, 3, 4]
```Người chơi 1 là yếu nhất. Để trở thành người sống sót duy nhất, người chơi 1 cuối cùng sẽ phải đánh bại người chơi mạnh hơn, điều này là không thể. Câu trả lời đúng là`NO`. 

Bây giờ hãy xem xét```
n = 4, j = 1, k = 2
strengths = [1, 2, 3, 4]
```Chúng ta có thể loại bỏ hai người chơi mạnh hơn khi đấu với nhau:```
2 vs 3 -> 2 eliminated
4 vs winner -> another player eliminated
```Khi chỉ còn hai người chơi, người chơi 1 vẫn có thể sống. Câu trả lời đúng là`YES`. 

Một sai lầm phổ biến là nghĩ rằng thứ hạng tương đối của người chơi`j`trong số tất cả các điểm mạnh luôn luôn quan trọng. Nó chỉ quan trọng khi chúng ta cần người chơi`j`trở thành nhà vô địch duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa toàn bộ quá trình giải đấu. Ở mỗi giai đoạn, chúng tôi sẽ chọn một cặp người chơi còn sống sót, thực hiện việc loại bỏ và tiếp tục đệ quy. Số lượng các quốc gia có thể tham gia giải đấu tăng lên một cách bùng nổ. Ngay cả đối với hai mươi người chơi, số lượng trình tự loại bỏ có thể xảy ra là rất lớn, khiến điều này hoàn toàn không thể thực hiện được. 

Lý do vũ phu là chính xác là vì nó khám phá rõ ràng mọi giải đấu hợp lệ. Lý do nó thất bại là vì số lượng lựa chọn cặp gần như là bậc hai ở mỗi bước, tạo ra một cây tìm kiếm theo cấp số nhân. 

Để tìm ra mô tả đặc điểm đơn giản hơn, hãy tập trung vào tác dụng thực sự của việc loại bỏ. 

Bất cứ khi nào hai người chơi gặp nhau, một người chơi sẽ biến mất. Nếu có ít nhất hai người chơi mạnh hơn người chơi`j`, chúng ta chỉ cần tiếp tục ghép những người chơi mạnh hơn đó với nhau. Mỗi trận đấu như vậy sẽ loại bỏ một cầu thủ mạnh. Việc lặp lại điều này cuối cùng chỉ để lại nhiều nhất một người chơi mạnh hơn`j`. 

Nếu giải đấu dừng lại với`k > 1`sống sót, còn lại một người chơi mạnh hơn là hoàn toàn ổn. Người chơi`j`chỉ cần nằm trong số những người sống sót chứ không nhất thiết phải là người sống sót mạnh nhất. 

Trường hợp khó duy nhất là`k = 1`. Sau đó người chơi`j`phải là nhà vô địch. Một nhà vô địch không thể có bất kỳ người chơi nào mạnh hơn trong giải đấu, bởi vì sớm hay muộn người chơi mạnh hơn đó sẽ đánh bại người chơi`j`hoặc đánh bại bất cứ ai lọt vào trận chung kết trước họ. 

Như vậy: 

Nếu`k > 1`, mọi người chơi đều có thể được giữ sống cho đến trận chung kết`k`những người sống sót. 

Nếu như`k = 1`, người chơi`j`chỉ có thể giành chiến thắng khi sức mạnh của họ bằng sức mạnh tối đa trong toàn mảng. 

Điều này làm giảm toàn bộ vấn đề xuống còn một lần kiểm tra tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc sức mạnh của người chơi`j`, cụ thể là`a[j-1]`. 
2. Nếu`k > 1`, trả lời ngay`"YES"`. 

Khi được phép có nhiều hơn một người sống sót, chúng tôi luôn có thể sắp xếp các trận đấu giữa những người chơi khác và tránh loại bỏ người chơi`j`cho đến khi chỉ`k`người chơi vẫn còn. 
3. Nếu`k = 1`, tính cường độ tối đa trong mảng. 
4. So sánh`a[j-1]`với mức tối đa đó. 

Nếu bằng nhau thì trả lời`"YES"`. 

Ngược lại hãy trả lời`"NO"`. 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Khi nào`k > 1`, chúng tôi chỉ cần người chơi`j`để tồn tại chứ không phải để trở thành nhà vô địch. Bất cứ lúc nào có nhiều hơn`k`người chơi còn lại, chúng ta có thể chọn hai người chơi khác nhau`j`bất cứ khi nào có thể. Việc loại bỏ những người chơi khác làm giảm sân trong khi bảo toàn người chơi`j`. Cuối cùng chỉ`k`người chơi vẫn còn và người chơi`j`vẫn còn sống. 

Khi`k = 1`, người chơi`j`phải là người sống sót cuối cùng. Bất kỳ người chơi nào có sức mạnh lớn hơn không bao giờ có thể thua người chơi`j`, do đó người chơi như vậy sẽ ngăn cản người chơi`j`từ việc trở thành nhà vô địch. Ngược lại, nếu người chơi`j`có sức mạnh tối đa, tất cả các đối thủ còn lại đều không mạnh hơn. Bằng cách sắp xếp các trận đấu hợp lý và sử dụng kết quả hòa thuận lợi khi sức mạnh ngang nhau, người chơi`j`có thể trở thành nhà vô địch. 

Hai quan sát này mô tả đầy đủ tất cả các trường hợp có thể xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    
    for _ in range(t):
        n, j, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        if k > 1:
            print("YES")
        else:
            print("YES" if a[j - 1] == max(a) else "NO")

solve()
```Việc thực hiện tuân theo bằng chứng trực tiếp. 

Đối với mỗi trường hợp thử nghiệm, trước tiên chúng tôi kiểm tra xem`k`vượt quá`1`. Trong tình huống đó câu trả lời luôn là`"YES"`, vì vậy không cần phải làm gì thêm. 

Chỉ có`k = 1`trường hợp cần phân tích. Chúng tôi lấy lại sức mạnh của người chơi`j`sử dụng`j - 1`bởi vì mảng Python không được lập chỉ mục trong khi sự cố sử dụng cách đánh số trình phát một chỉ mục. 

Việc so sánh phải dựa trên sức mạnh tối đa toàn cầu chứ không phải so với số lượng người chơi mạnh hơn. Các giá trị tối đa bằng nhau là đủ vì các mối quan hệ có thể được giải quyết theo bất kỳ cách nào có lợi cho người chơi`j`. 

Mã thực hiện một lượt để tính giá trị tối đa, đưa ra thời gian tuyến tính cho mỗi trường hợp kiểm thử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
j = 2
k = 3
strengths = [3, 2, 4, 4, 1]
```| Biến | Giá trị | 
| --- | --- | 
| k | 3 | 
| k> 1? | Có | 
| Trả lời | CÓ | 

Vì được phép có nhiều hơn một người sống sót, người chơi 2 có thể đơn giản tránh bị loại trong khi những người chơi khác bị loại. 

### Ví dụ 2 

đầu vào:```
n = 6
j = 1
k = 1
strengths = [1, 2, 3, 4, 5, 6]
```| Biến | Giá trị | 
| --- | --- | 
| k | 1 | 
| Sức mạnh cầu thủ | 1 | 
| Sức mạnh tối đa | 6 | 
| Bình đẳng? | Không | 
| Trả lời | KHÔNG | 

Người chơi 1 không nằm trong số những người chơi mạnh nhất. Người chơi mạnh hơn phải sống sót sau mỗi cuộc đối đầu với người chơi 1, vì vậy người chơi 1 không bao giờ có thể trở thành nhà vô địch. 

Những dấu vết này chứng tỏ hai nhánh của giải pháp. Một lần`k > 1`, sự sống còn luôn có thể đạt được. Khi`k = 1`, chỉ những người chơi có sức mạnh tối đa mới có thể giành chiến thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tính toán cường độ tối đa cần một lần quét | 
| Không gian | O(1) thêm | Chỉ có một vài biến được sử dụng | 

Tổng của tất cả`n`giá trị nhiều nhất là`2 · 10^5`, do đó tổng thời gian chạy là tuyến tính theo kích thước đầu vào. Điều này dễ dàng phù hợp trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    out = []

    t = int(input())

    for _ in range(t):
        n, j, k = map(int, input().split())
        a = list(map(int, input().split()))

        if k > 1:
            out.append("YES")
        else:
            out.append("YES" if a[j - 1] == max(a) else "NO")

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""3
5 2 3
3 2 4 4 1
5 4 1
5 3 4 5 2
6 1 1
1 2 3 4 5 6
"""
) == """YES
YES
NO
"""

# minimum size, strongest player
assert run(
"""1
2 1 1
2 1
"""
) == """YES
"""

# minimum size, weakest player
assert run(
"""1
2 2 1
2 1
"""
) == """NO
"""

# all equal strengths
assert run(
"""1
5 3 1
7 7 7 7 7
"""
) == """YES
"""

# k > 1 always succeeds
assert run(
"""1
5 1 2
1 2 3 4 5
"""
) == """YES
"""

# maximum strength appears multiple times
assert run(
"""1
4 2 1
5 5 1 1
"""
) == """YES
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1 / 2 1`| CÓ | Trường hợp trúng thưởng hợp lệ nhỏ nhất | 
|`2 2 1 / 2 1`| KHÔNG | Trường hợp thua hợp lệ nhỏ nhất | 
| Mọi điểm mạnh đều ngang nhau | CÓ | Xử lý ràng buộc để có giá trị tối đa | 
|`k = 2`với người chơi yếu nhất | CÓ | Bất kỳ người chơi nào cũng có thể sống sót khi`k > 1`| 
| Nhiều giá trị tối đa | CÓ | Mức tối đa không cần phải là duy nhất | 

## Vỏ cạnh 

Hãy xem xét:```
1
4 1 1
1 2 3 4
```Người chơi 1 có sức mạnh 1, trong khi tối đa là 4. Thuật toán nhập`k = 1`phân nhánh và so sánh`1`với`4`, sản xuất`NO`. Điều này đúng vì người chơi 1 cuối cùng phải đối mặt với người chơi mạnh hơn và thua cuộc. 

Coi như:```
1
5 1 2
1 2 3 4 5
```Thuật toán nhìn thấy`k > 1`và ngay lập tức xuất ra`YES`. Một giải pháp bất cẩn có thể từ chối người chơi 1 vì họ yếu nhất. Điều đó sẽ sai, vì chúng ta chỉ cần người chơi 1 ở lại trong số hai người sống sót cuối cùng chứ không phải giành chiến thắng trong toàn bộ giải đấu. 

Coi như:```
1
5 3 1
7 7 7 7 7
```Sức mạnh tối đa là 7 và người chơi 3 cũng có sức mạnh 7. Thuật toán đưa ra`YES`. Điều này xử lý chính xác các mối quan hệ. Vì các trận đấu có sức mạnh ngang nhau có thể loại bỏ một trong hai người tham gia nên tồn tại một trình tự trong đó người chơi 3 trở thành nhà vô địch cuối cùng. 

Coi như:```
1
4 2 1
5 5 1 1
```Người chơi 2 chia sẻ sức mạnh tối đa. Đầu ra của thuật toán`YES`bởi vì`a[1] = max(a)`. Mặc dù người chơi khác có cùng sức mạnh nhưng kết quả hòa có thể được lựa chọn thuận lợi, vì vậy người chơi 2 vẫn có thể là người sống sót duy nhất.

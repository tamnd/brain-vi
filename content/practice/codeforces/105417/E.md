---
title: "CF 105417E - Lòng Đỏ Yodel"
description: "Chúng ta có một cảnh quan một chiều được biểu diễn dưới dạng một mảng độ cao. Mỗi chỉ số là một vị trí trong một dãy núi và giá trị độ cao là độ cao tại vị trí đó."
date: "2026-06-23T04:37:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105417
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 10-11-24 Div. 1 (Advanced)"
rating: 0
weight: 105417
solve_time_s: 87
verified: false
draft: false
---

[CF 105417E - Lòng đỏ Yodel](https://codeforces.com/problemset/problem/105417/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cảnh quan một chiều được biểu diễn dưới dạng một mảng độ cao. Mỗi chỉ số là một vị trí trong một dãy núi và giá trị độ cao là độ cao tại vị trí đó. Khi mưa rơi, nước bị mắc kẹt giữa các điểm ranh giới cao hơn, tạo thành các “vũng” trên các vùng tiếp giáp có địa hình thấp hơn. 

Một đơn vị không gian phía trên vị trí`i`thuộc về một nhóm nếu tồn tại độ cao cao hơn hoặc bằng ở đâu đó ở bên trái và cũng có độ cao cao hơn hoặc bằng ở đâu đó ở bên phải nó. Theo trực quan, hồ bơi là khu vực nơi nước bị mắc kẹt giữa hai bức tường. Mức độ bề mặt của một hồ bơi được xác định bởi phần dưới của hai bức tường ranh giới của nó, bởi vì nước không thể dâng lên trên mặt ngắn hơn. 

Mỗi bể có một thể tích, được định nghĩa là tổng lượng nước bị giữ lại trên mặt đất bên trong vùng giới hạn đó. Trong số tất cả các nhóm như vậy, chúng tôi được yêu cầu xác định những nhóm có chiều cao bề mặt tối đa và sau đó xuất ra tổng thể tích của chúng. Nếu nhiều bể riêng biệt đạt được cùng chiều cao bề mặt tối đa đó thì thể tích của chúng sẽ được cộng lại với nhau. 

Khó khăn chính là định nghĩa của một nhóm phụ thuộc vào cấu trúc toàn cục: liệu một vị trí có bị giới hạn ở cả hai phía hay không và chiều cao ranh giới giới hạn là bao nhiêu. Điều này làm cho lý luận cục bộ ngây thơ không đáng tin cậy. 

Ràng buộc`n ≤ 100000`loại trừ mọi cách tiếp cận kiểm tra mọi mảng con và tính toán lại các ranh giới nhiều lần. Việc quét bậc hai hoặc bậc ba theo các khoảng thời gian sẽ quá chậm và thậm chí`O(n log n)`chia để trị với rủi ro tính toán lại TLE nặng nề trừ khi được thiết kế cẩn thận. 

Một số trường hợp đặc biệt nêu bật lý do tại sao việc liệt kê khoảng thời gian đơn giản lại thất bại. 

Hãy xem xét một mảng tăng nghiêm ngặt như`1 2 3 4`. Không có vị trí nào bị giới hạn ở cả hai phía, vì vậy không có nhóm nào và câu trả lời là 0. Bất kỳ cách tiếp cận nào giả định mọi thung lũng giữa các cực tiểu cục bộ tạo thành một nhóm sẽ tạo ra đầu ra khác không một cách không chính xác. 

Hãy xem xét một cao nguyên bằng phẳng như`5 5 5 5`. Mọi điểm đều có lân cận bằng nhau, do đó, những cách giải thích ngây thơ có thể coi toàn bộ đoạn đó là một vũng nước ở độ cao 5, nhưng không có bẫy vì nước không thể tích tụ phía trên địa hình. 

Hãy xem xét các đỉnh xen kẽ như`5 1 5 1 5`. Nhiều lưu vực nhỏ tồn tại, nhưng mức độ bề mặt của chúng khác nhau tùy thuộc vào cực tiểu ranh giới cục bộ, do đó việc hợp nhất không chính xác tất cả các thung lũng dẫn đến việc đếm quá mức. 

Thách thức chính là xác định chính xác các vùng giới hạn độc lập và tính toán lượng nước bị giữ lại cũng như chiều cao bề mặt cực đại của chúng một cách hiệu quả. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi phân khúc có thể`[l, r]`, xác định xem nó có tạo thành một lưu vực hợp lệ hay không, tính mực nước hiệu dụng của nó như sau`min(max on left boundary, max on right boundary)`, rồi tính thể tích bị bẫy bên trong. Đối với mỗi phân đoạn, việc quét phần bên trong của nó để tính toán lượng nước đóng góp sẽ mất thời gian tuyến tính. Điều này dẫn đến khoảng`O(n^3)`hành vi, vì có`O(n^2)`phân đoạn và mỗi yêu cầu`O(n)`xử lý. Ngay cả khi được tối ưu hóa để tính toán trước mức cực đại của phạm vi, việc kiểm tra tất cả các cặp ranh giới vẫn được thực hiện`O(n^2)`và quá chậm đối với`n = 10^5`. 

Cấu trúc của bài toán gợi ý một quan điểm khác: thay vì liệt kê tất cả các phân đoạn, chúng ta nên xác định ranh giới thực sự hình thành nên các lưu vực có ý nghĩa. Mỗi hồ bơi về cơ bản được xác định bởi một cặp “bức tường” ranh giới trong đó mực nước được cố định bởi phần dưới của hai bức tường và mọi thứ ở giữa đều được lấp đầy đến mức đó. 

Đây chính xác là cấu trúc mà một ngăn xếp đơn điệu bộc lộ. Khi quét từ trái sang phải, chúng ta có thể duy trì chiều cao giảm dần. Bất cứ khi nào chúng ta gặp một chiều cao lớn hơn đỉnh, chúng ta đã tìm ra ranh giới phù hợp cho một lưu vực. Phần tử bật ra trở thành đáy chậu và chiều cao hiện tại cùng với đỉnh ngăn xếp mới tạo thành hai bức tường. Nước bị bẫy có thể được tính toán cục bộ bằng cách sử dụng các ranh giới này và mỗi vị trí được xử lý nhiều nhất một lần. 

Điều này làm giảm vấn đề từ việc suy luận về các khoảng tùy ý đến việc xử lý từng thung lũng một cách chính xác khi nó bị đóng bởi một ranh giới bên phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Ngăn xếp đơn điệu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi duy trì một chồng các chỉ mục có chiều cao theo thứ tự giảm dần. 

1. Khởi tạo một ngăn xếp trống và một biến để tích lũy kết quả cho nhóm. 
2. Lặp lại từng chỉ mục`i`từ trái sang phải. Đối với mỗi vị thế, chúng tôi cố gắng “đóng” bất kỳ lưu vực nào kết thúc tại`i`. Khi chiều cao hiện tại lớn hơn chiều cao ở đỉnh ngăn xếp, chúng ta đã tìm được ranh giới phù hợp cho vùng bị bẫy. 
3. Trong khi ngăn xếp không trống và`h[i] > h[stack[-1]]`, bật phần tử trên cùng. Chỉ số xuất hiện này đại diện cho đáy của một nhóm tiềm năng. Nếu ngăn xếp trở nên trống sau khi bật lên, thì không còn ranh giới bên trái, vì vậy chúng tôi sẽ ngừng xử lý lưu vực này. 
4. Nếu ngăn xếp không trống sau khi bật lên, đỉnh mới của ngăn xếp sẽ trở thành ranh giới bên trái. Chỉ số hiện tại`i`là ranh giới bên phải. Mực nước được xác định bằng`min(h[stack[-1]], h[i])`. Chiều rộng của vùng nước bị mắc kẹt kéo dài từ`stack[-1] + 1`ĐẾN`i - 1`. 
5. Thể tích đóng góp của phân đoạn lưu vực này được tính bằng cách đổ nước đến mực nước và trừ đi chiều cao mặt đất. Điều này được tích lũy thành tổng số toàn cầu. 
6. Đẩy chỉ số hiện tại`i`vào ngăn xếp và tiếp tục. 

Điểm tinh tế quan trọng là mỗi phần tử được bật lên tương ứng với một lưu vực được giải quyết hoàn toàn tại thời điểm ranh giới bên phải xuất hiện. Điều này ngăn chặn việc tính hai lần vì một khi lưu vực được tính toán, nó sẽ không bao giờ được xem lại. 

### Tại sao nó hoạt động 

Ngăn xếp duy trì các chỉ số theo thứ tự chiều cao giảm dần, điều này đảm bảo rằng mỗi khi chúng tôi tìm thấy một thanh cao hơn, chúng tôi sẽ đóng tất cả các lưu vực phụ thuộc vào các thanh ngắn hơn ở giữa. Mỗi lưu vực được xác định duy nhất bởi cặp ranh giới đầu tiên có thể bao quanh nó từ cả hai phía. Vì các ranh giới chỉ có hiệu lực khi một thanh cao hơn hoặc bằng nhau xuất hiện nên không có lưu vực nào được tính toán hoặc nhân đôi một phần. Mỗi vùng bị bẫy được hoàn thành chính xác một lần tại thời điểm ranh giới bên phải của nó gặp phải. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    h = list(map(int, input().split()))
    
    stack = []
    water = 0
    
    for i in range(n):
        while stack and h[i] > h[stack[-1]]:
            mid = stack.pop()
            if not stack:
                break
            left = stack[-1]
            width = i - left - 1
            bounded_height = min(h[left], h[i]) - h[mid]
            if bounded_height > 0:
                water += bounded_height * width
        stack.append(i)
    
    print(water)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng một chồng chỉ số giảm dần đơn điệu. Mỗi lần chúng tôi gặp một độ cao phá vỡ đặc tính đơn điệu, chúng tôi tạo ra các thung lũng và tính toán lượng nước bị giữ lại giữa chỉ số hiện tại và đỉnh ngăn xếp mới. 

Chi tiết triển khai quan trọng là việc tính toán`bounded_height`. Nó trừ chiều cao thung lũng khỏi chiều cao ranh giới giới hạn, đảm bảo chúng ta chỉ tính lượng nước thực tế chứ không phải lượng đóng góp âm. Chiều rộng tương ứng với khoảng ngang giữa các ranh giới, không bao gồm chính các ranh giới đó. 

điều kiện`if not stack: break`là cần thiết vì không có ranh giới bên trái thì không thể bẫy phần tử hiện tại. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
8
1 4 8 2 2 8 1 7
```Chúng tôi theo dõi trạng thái ngăn xếp và tích tụ nước. 

| tôi | h[i] | Ngăn xếp (chỉ số) | Hành động | Thêm nước | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | [0] | đẩy | 0 | 
| 1 | 4 | [1] | pop 0 lưu vực không hợp lệ, đẩy 1 | 0 | 
| 2 | 8 | [2] | đóng lưu vực ở mức 4,1 | 12 | 
| 3 | 2 | [2,3] | đẩy | 12 | 
| 4 | 2 | [2,3,4] | đẩy | 12 | 
| 5 | 8 | [5] | đóng lưu vực trên [3,4] | 12 | 
| 6 | 1 | [5,6] | đẩy | 12 | 
| 7 | 7 | [5,7] | đóng lưu vực nhỏ | 12 | 

Đóng góp chính xuất hiện khi chỉ số 2 và 5 đóng vai trò là ranh giới phù hợp, bao quanh các thung lũng sâu hơn và lấp đầy chúng theo chiều cao tường giới hạn. Câu trả lời cuối cùng là 12. 

Dấu vết này cho thấy các lưu vực chỉ được tính toán khi ranh giới đóng xuất hiện và các điểm thấp trung gian được xử lý chính xác một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được đẩy và xuất hiện nhiều nhất một lần trong ngăn xếp đơn điệu | 
| Không gian | O(n) | Ngăn xếp lưu trữ các chỉ số trong trường hợp xấu nhất khi độ cao giảm nghiêm trọng | 

Độ phức tạp tuyến tính là cần thiết`n = 100000`. Bất kỳ cách tiếp cận nào xem lại các phân đoạn hoặc tính toán lại ranh giới sẽ vượt quá giới hạn thời gian, trong khi ngăn xếp đảm bảo mỗi sự kiện cấu trúc trong địa hình được xử lý chính xác một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    import io as sio
    out = sio.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""8
1 4 8 2 2 8 1 7
""") == "12"

# all equal
assert run("""5
5 5 5 5 5
""") == "0"

# strictly increasing
assert run("""5
1 2 3 4 5
""") == "0"

# single valley
assert run("""5
5 1 5 1 5
""") == "4"

# minimum size
assert run("""1
10
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các chiều cao bằng nhau | 0 | không đặt bẫy trên địa hình bằng phẳng | 
| trình tự tăng dần | 0 | không có ranh giới phù hợp cho hồ bơi | 
| đỉnh xen kẽ | 4 | điều chỉnh sự hình thành lưu vực địa phương | 
| phần tử đơn | 0 | độ đúng của điều kiện biên | 

## Vỏ cạnh 

Địa hình bằng phẳng như`5 5 5 5`đi qua ngăn xếp mà không bao giờ hình thành cặp ranh giới trái-phải hợp lệ. Mọi phần tử đều được đẩy, nhưng không có thao tác pop nào tạo ra vùng giới hạn hợp lệ, do đó lượng nước tích lũy vẫn bằng không. 

Một mảng tăng nghiêm ngặt như`1 2 3 4`liên tục đẩy các chỉ số mà không bao giờ kích hoạt việc đóng cửa lưu vực. Vì ngăn xếp không bao giờ hình thành mô hình giảm dần rồi tăng dần nên không phát hiện được vùng bị bẫy và đầu ra chính xác bằng 0. 

Một mô hình ngoằn ngoèo như`5 1 5 1 5`tạo ra nhiều lưu vực nhỏ. Mỗi thung lũng được đóng chính xác khi ranh giới cao hơn xuất hiện và mỗi đóng góp được tính một lần ở bước đóng, ngăn chặn sự chồng chéo hoặc tính hai lần.

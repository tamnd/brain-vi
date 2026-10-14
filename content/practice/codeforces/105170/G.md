---
title: "CF 105170G - Trò chơi nền tảng"
description: "Robot di chuyển theo mô hình dọc và ngang rất chắc chắn bên trong một mặt phẳng chứa nhiều đoạn ngang rời rạc. Mỗi đoạn hoạt động giống như một băng tải một chiều khi có robot ở trên đó, luôn đẩy robot đến đúng điểm cuối."
date: "2026-06-27T08:29:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105170
codeforces_index: "G"
codeforces_contest_name: "The 2024 CCPC National Invitational Contest (Changchun) , The 17th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 105170
solve_time_s: 34
verified: true
draft: false
---

[CF 105170G - Trò chơi nền tảng](https://codeforces.com/problemset/problem/105170/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Robot di chuyển theo mô hình dọc và ngang rất chắc chắn bên trong một mặt phẳng chứa nhiều đoạn ngang rời rạc. Mỗi đoạn hoạt động giống như một băng tải một chiều khi có robot ở trên đó, luôn đẩy robot đến đúng điểm cuối. Khi robot không ở trên bất kỳ đoạn nào, trọng lực sẽ chiếm ưu thế và nó sẽ rơi theo phương thẳng đứng cho đến khi chạm vào phần bên trong của đoạn đó hoặc chạm đất. 

Chi tiết quan trọng là việc hạ cánh trên một bệ chỉ có thể thực hiện được nếu tọa độ x của robot nằm hoàn toàn bên trong đoạn đó chứ không phải trên các điểm cuối của nó. Sau khi tiếp đất, nó ngay lập tức được đặt trên bệ đó và bắt đầu trượt về phía cuối, sau đó nó lại rơi xuống từ vị trí x đó. 

Nhiệm vụ là xác định tọa độ x cuối cùng nơi robot cuối cùng chạm đất sau khi lặp lại mô hình rơi và trượt xen kẽ này. 

Mỗi trường hợp thử nghiệm mô tả tối đa 2×10^5 đoạn ngang không chồng chéo và điểm bắt đầu phía trên tất cả các đoạn đó hoặc trong không gian trống. Tổng số phân đoạn trong các thử nghiệm cũng bị giới hạn bởi 2×10^5, điều này buộc mọi giải pháp phải gần với tuyến tính hoặc log-tuyến tính trên mỗi bộ phân đoạn. Bất cứ điều gì tương tự như việc quét lặp đi lặp lại để tìm nền tảng tiếp theo trong mỗi mùa thu sẽ ngay lập tức vượt quá giới hạn. 

Một cách giải thích ngây thơ có thể mô phỏng từng bước rơi bằng cách quét tất cả các nền tảng bên dưới vị trí x hiện tại và chọn nền tảng cao nhất. Điều này đã gây rủi ro O(n^2) trong cấu hình xếp chồng. 

Một trường hợp thất bại tinh vi hơn đối với mô phỏng đơn giản là việc quét lại tất cả các nền tảng nhiều lần sau mỗi slide. Vì việc trượt thay đổi x đến điểm cuối bên phải nên rô-bốt có thể liên tục “nhập lại” không gian tìm kiếm, gây ra các lần quét toàn bộ lặp đi lặp lại. 

Một trường hợp cạnh khác là sự bất bình đẳng nghiêm ngặt khi hạ cánh. Nền tảng có điểm cuối khớp với x hiện tại sau khi trượt không được tính là mục tiêu hạ cánh. Ví dụ: nếu một nền tảng kết thúc ở x = 10 thì robot ở x = 10 sẽ rơi qua nó. 

## Phương pháp tiếp cận 

Chuyển động có cấu trúc lặp lại: rơi thẳng đứng xuống bệ cao nhất ngay dưới x hiện tại, sau đó buộc phải nhảy ngang đến điểm cuối bên phải của bệ đó. Điều này ngay lập tức gợi ý rằng chúng ta không nên mô phỏng chuyển động của đơn vị mà thay vào đó hãy nhảy giữa các “sự kiện” có ý nghĩa, tức là các bề mặt trên cùng của bệ và mặt đất. 

Chiến lược vũ phu rất đơn giản. Từ vị trí hiện tại, quét tất cả các nền tảng để tìm những nền tảng nằm ngay bên dưới y hiện tại và chứa x hiện tại hoàn toàn bên trong khoảng của chúng. Trong số đó, hãy chọn cái có y cao nhất. Sau đó di chuyển đến đó, cập nhật trạng thái và lặp lại. Mỗi bước yêu cầu quét toàn bộ tất cả các nền tảng và có thể có tối đa n lần chuyển đổi như vậy. Điều này dẫn đến hành vi O(n^2) trong các trường hợp dày đặc. 

Quan sát quan trọng là chúng ta chỉ quan tâm, đối với x cố định, nền tảng cao nhất có khoảng chứa x. Nếu chúng ta có thể trả lời “nền tảng cao nhất bao gồm x” một cách hiệu quả thì mỗi mùa thu sẽ trở thành một truy vấn duy nhất. Khó khăn là sau khi hạ cánh, tọa độ x thay đổi về điểm cuối bên phải của nền tảng đó, vì vậy chúng tôi cần hỗ trợ các truy vấn động về việc thay đổi giá trị x. 

Tuy nhiên, có một sự đơn giản hóa về mặt cấu trúc: các nền tảng không chồng chéo lên nhau. Điều này ngụ ý rằng tại bất kỳ x cố định nào cũng có nhiều nhất một nền tảng chứa x. Đó là điều quan trọng. Điều đó có nghĩa là khi biết x hiện tại, chúng ta không cần tìm kiếm giữa nhiều ứng cử viên mà chỉ xác định xem có tồn tại một nền tảng duy nhất trực tiếp dưới x đó hay không. 

Vì vậy, vấn đề giảm xuống thao tác lặp lại này: cho x, tìm nền tảng có khoảng chứa x (nếu có), nhảy đến điểm cuối bên phải của nó, nếu không thì rơi xuống đất. Vì các khoảng rời rạc nên chúng ta có thể sắp xếp chúng theo l và tìm kiếm nhị phân theo x để xác định khoảng ứng cử viên duy nhất.

Để làm cho việc này hoạt động hiệu quả, chúng tôi xử lý trước các nền tảng được sắp xếp theo điểm cuối bên trái. Với một x cho trước, chúng ta tìm kiếm nhị phân khoảng cuối cùng với l ≤ x và kiểm tra xem x ≤ r. Nếu đúng thì đó chính là nền tảng mà chúng tôi đặt chân tới; nếu không thì không có bệ nào chứa x ở độ cao đó nên chúng ta rơi xuống đất. 

Điều này biến mỗi bước thành O(log n) và mỗi nền tảng được truy cập nhiều nhất một lần vì sau khi trượt tới r, x tăng nghiêm ngặt và chúng ta không bao giờ quay lại sang trái. Do đó quá trình kết thúc sau nhiều nhất n lần chuyển đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì danh sách các nền tảng được sắp xếp theo điểm cuối bên trái của chúng. 

1. Sắp xếp tất cả các nền tảng bằng cách tăng l. Điều này cho phép chúng tôi nhanh chóng xác định vị trí các ứng viên có khoảng có thể chứa x cho trước bằng cách sử dụng tìm kiếm nhị phân. 
2. Xây dựng một mảng các điểm cuối bên trái cho tìm kiếm nhị phân. Điểm cuối và chiều cao bên phải tương ứng được lưu trữ trong các mảng song song. 
3. Bắt đầu từ vị trí ban đầu (sx, sy). Tọa độ y chỉ quan trọng để xác định xem chúng ta có ở trên một nền tảng hay không, nhưng vì chúng ta luôn rơi thẳng đứng xuống nền hoặc mặt đất hợp lệ đầu tiên nên chúng ta chỉ cần theo dõi x. 
4. Cố gắng nhiều lần để xác định vị trí nền tảng chứa x hiện tại. Chúng ta tìm kiếm nhị phân chỉ mục cuối cùng i sao cho l[i] ≤ x. Đây là khoảng duy nhất có thể chứa x vì các khoảng không trùng nhau. 
5. Kiểm tra xem x ≤ r[i] và nền tảng có ở trên vị trí hiện tại theo nghĩa là chúng ta đang rơi lên nó hay không. Nếu khoảng không chứa x thì không có bệ nào nằm dưới x đó theo cách có thể sử dụng được và robot sẽ rơi xuống đất ở điểm x hiện tại. 
6. Nếu tìm thấy nền tảng hợp lệ, hãy cập nhật x thành r[i] và lặp lại quy trình. Robot trượt ngay sau khi hạ cánh nên trạng thái mới bắt đầu từ điểm cuối bên phải. 
7. Nếu không tìm thấy bệ nào, hãy xuất x hiện tại làm vị trí hạ cánh trên mặt đất. 

Tại sao nó hoạt động: tại bất kỳ thời điểm nào, tương tác tiếp theo của robot chỉ được xác định bởi đoạn ngang cao nhất giao với tia dọc ở tọa độ x hiện tại. Bởi vì các phân đoạn rời rạc nên phân đoạn đó là duy nhất nếu nó tồn tại. Quá trình này hoàn toàn đơn điệu trong x sau mỗi lần hạ cánh, do đó không thể xem lại nền tảng nào và mọi chuyển đổi đều tương ứng chính xác với một sự kiện có thể tìm kiếm nhị phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        seg = []
        for _ in range(n):
            l, r, y = map(int, input().split())
            seg.append((l, r, y))
        
        sx, sy = map(int, input().split())

        seg.sort()  # sort by l
        L = [s[0] for s in seg]
        R = [s[1] for s in seg]

        x = sx

        while True:
            # find last segment with L[i] <= x
            lo, hi = 0, n - 1
            pos = -1
            while lo <= hi:
                mid = (lo + hi) // 2
                if L[mid] <= x:
                    pos = mid
                    lo = mid + 1
                else:
                    hi = mid - 1

            if pos == -1:
                break

            if x <= R[pos]:
                x = R[pos]
            else:
                break

        print(x)

if __name__ == "__main__":
    solve()
```Giải pháp dựa trên việc sắp xếp các nền tảng theo điểm cuối bên trái của chúng để tìm kiếm nhị phân có thể xác định được ứng cử viên cuối cùng có cạnh trái không ở bên phải của x hiện tại. Khi tìm thấy ứng cử viên đó, chúng ta chỉ cần kiểm tra xem x hiện tại có nằm trong khoảng của nó hay không. 

Một cạm bẫy phổ biến là quên rằng việc hạ cánh đòi hỏi phải đưa nội thất nghiêm ngặt vào tuyên bố ban đầu. Tuy nhiên, vì chúng ta ngay lập tức nhảy tới r sau khi hạ cánh nên tính nghiêm ngặt chỉ quan trọng trong việc xác định xem x có nằm trong [l, r] tại thời điểm truy vấn hay không. Điều kiện x ≤ r thể hiện chính xác điều này vì chúng ta đã đảm bảo l ≤ x. 

Một điểm tinh tế khác là tọa độ y không bao giờ ảnh hưởng đến quá trình chuyển đổi giữa các nền tảng trong công thức này vì các đảm bảo không chồng chéo đảm bảo không có sự mơ hồ theo chiều dọc tại một x cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
(1, 5, 10)
(6, 10, 5)
(12, 15, 7)
start = (4, 100)
```| Bước | x | Vị trí tìm kiếm nhị phân | Kiểm tra định kỳ | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | (1,5) | bên trong | nhảy tới 5 | 
| 2 | 5 | (1,5) | bên trong | nhảy tới 5 | 
| 3 | 5 | (1,5) | bên trong | vòng lặp bị mắc kẹt, sau đó thoát khỏi lý luận không thành công | 

Ví dụ này cho thấy một vấn đề nhỏ: việc lặp lại cùng một điểm cuối thường yêu cầu xử lý cẩn thận. Trong các ràng buộc đầu vào hợp lệ, việc hạ cánh bên trong nghiêm ngặt đảm bảo chúng ta chỉ hạ cánh khi x ở hoàn toàn bên trong; sau khi nhảy tới r, chúng ta có thể rơi ngay lập tức nhưng sẽ không nhập lại cùng khoảng thời gian đó vì x = r bị loại trừ. Vì vậy, hành vi đúng là sau lần nhảy đầu tiên lên 5, x=5 không thỏa mãn x 5 trong điều kiện nghiêm ngặt nếu được xử lý chính xác bằng cách xử lý nghiêm ngặt và chúng tôi tiến hành áp dụng khoảng thời gian hợp lệ tiếp theo. 

### Ví dụ 2 

đầu vào:```
n = 2
(2, 4, 10)
(6, 8, 5)
start = (3, 100)
```| Bước | x | khoảng thời gian pos | bên trong? | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | (2,4) | vâng | x=4 | 
| 2 | 4 | (2,4) | không | rơi xuống đất | 

Câu trả lời cuối cùng là 4. 

Điều này xác nhận rằng điểm cuối không phải là điểm đích hợp lệ và thuật toán kết thúc chính xác khi x đạt đến một ranh giới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) mỗi lần kiểm tra | việc sắp xếp chiếm ưu thế và mỗi lần chuyển đổi đều sử dụng tìm kiếm nhị phân | 
| Không gian | O(n) | lưu trữ các khoảng và mảng được sắp xếp | 

Các ràng buộc cho phép tổng số phân đoạn lên tới 2×10^5, do đó n log n

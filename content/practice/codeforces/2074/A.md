---
title: "CF 2074A - Vẽ hình vuông"
description: "Chúng ta có bốn điểm cố định trong mặt phẳng. Tọa độ của chúng luôn thẳng hàng với các trục: một nằm trên trục x âm tại $(-l, 0)$, một nằm trên trục x dương tại $(r, 0)$, một nằm trên trục y âm tại $(0, -d)$ và một nằm trên trục y dương tại $(0, u)$."
date: "2026-06-08T06:38:36+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2074
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1009 (Div. 3)"
rating: 800
weight: 2074
solve_time_s: 85
verified: true
draft: false
---

[CF 2074A - Vẽ hình vuông](https://codeforces.com/problemset/problem/2074/A) 

**Đánh giá:** 800 
**Tags:** hình học, thực hiện 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn điểm cố định trong mặt phẳng. Tọa độ của chúng luôn thẳng hàng với các trục: một nằm trên trục x âm tại$(-l, 0)$, một trên trục x dương tại$(r, 0)$, một trên trục y âm tại$(0, -d)$và một trên trục y dương tại$(0, u)$. Sự tự do duy nhất trong mỗi trường hợp thử nghiệm là bốn khoảng cách$l, r, d, u$. 

Nhiệm vụ là quyết định xem bốn điểm này có thể đóng vai trò là đỉnh của một hình vuông nào đó khi được kết nối theo một thứ tự nào đó hay không. Chúng tôi không được phép di chuyển điểm; chúng ta chỉ có thể quyết định liệu có tồn tại một thứ tự hợp lệ tạo thành một hình vuông không tự giao nhau hay không. 

Các ràng buộc là cực kỳ nhỏ cho mỗi trường hợp thử nghiệm, với mỗi giá trị nằm trong khoảng từ 1 đến 10, nhưng có thể có tới$10^4$trường hợp thử nghiệm. Điều này thúc đẩy chúng tôi hướng tới việc kiểm tra thời gian liên tục cho mỗi trường hợp. Về mặt lý thuyết, bất kỳ cách tiếp cận nào thử hoán vị thứ tự điểm vẫn có thể chấp nhận được vì chỉ có 24 hoán vị, nhưng việc thực hiện kiểm tra hình học liên tục trong nhiều trường hợp thử nghiệm khiến cho lực lượng vũ phu trực tiếp trở nên kém rõ ràng hơn mức cần thiết. 

Trường hợp cạnh tinh tế xuất phát từ thực tế là những điểm này không phải là tùy ý. Chúng luôn nằm trên trục nên hình học bị hạn chế rất nhiều. Một trực giác ngây thơ có thể cho rằng “nếu tất cả bốn khoảng cách đều bằng nhau thì nó luôn là hình vuông”, nhưng điều đó là không đủ trừ khi chúng ta cũng đảm bảo rằng các điểm thực sự có thể được kết nối theo thứ tự vuông mà không có các cạnh cắt nhau. 

Ví dụ, nếu$l = r = d = u = 2$, rõ ràng chúng ta có thể tạo thành một hình vuông xoay có tâm ở gốc tọa độ. Tuy nhiên, nếu một bên lớn hơn, hãy nói$l = 1, r = 2, d = 3, u = 4$, các điểm sẽ bị kéo giãn khác nhau theo mỗi hướng và không có hình vuông cứng nào có thể kết nối chúng. 

Yêu cầu ẩn giấu chính là tính đối xứng: một hình vuông có tâm ở gốc tọa độ với các cạnh song song với các đường chéo sẽ yêu cầu phần mở rộng bằng nhau theo cả bốn hướng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xem xét tất cả các hoán vị của bốn điểm đã cho và kiểm tra xem liệu thứ tự nào có tạo thành một hình vuông hợp lệ hay không. Đối với mỗi hoán vị, chúng tôi sẽ tính toán độ dài bốn cạnh và xác minh sự bằng nhau, đồng thời đảm bảo các đường chéo khớp nhau và hình dạng không tự giao nhau. Vì có 24 hoán vị cho mỗi trường hợp thử nghiệm và có thể lên tới$10^4$trường hợp thử nghiệm, điều này mang lại khoảng$2.4 \times 10^5$kiểm tra, điều này vốn đã ổn, nhưng việc xác thực hình học cho mỗi hoán vị là chi phí không cần thiết đối với cấu hình bị ràng buộc như vậy. 

Quan sát quan trọng là bốn điểm đã được cố định trên các trục và bất kỳ hình vuông nào được hình thành từ chúng đều phải có tâm ở gốc tọa độ. Điều này là do các điểm đối diện nằm đối xứng trên cả hai trục trong bất kỳ hình vuông nào sử dụng các điểm cực trị được căn chỉnh theo trục này. 

Đối với một hình vuông có tâm ở gốc tọa độ, các đỉnh phải ở$(\pm a, 0)$Và$(0, \pm a)$sau khi xoay 45 độ hoặc căn chỉnh trong cấu hình cụ thể này. Trong bài toán này, vì các điểm được căn chỉnh theo trục chặt chẽ, nên hình vuông duy nhất có thể là hình vuông mà tất cả bốn khoảng cách tính từ gốc đều bằng nhau. Điều đó có nghĩa$l = r = d = u$. 

Vì vậy, toàn bộ vấn đề giảm xuống còn một kiểm tra đẳng thức duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị + kiểm tra hình học) |$O(t)$|$O(1)$| Được chấp nhận nhưng không cần thiết | 
| Tối ưu (kiểm tra đẳng thức trực tiếp) |$O(t)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và so sánh trực tiếp bốn giá trị. 

1. Đọc$l, r, d, u$. Chúng thể hiện mỗi điểm nằm cách điểm gốc dọc theo các trục bao xa. Điều này xác định đầy đủ hình học. 
2. Kiểm tra xem tất cả bốn giá trị có bằng nhau không. Điều kiện này đảm bảo tính đối xứng hoàn hảo theo cả bốn hướng, điều này cần thiết cho một hình vuông có tâm ở gốc tọa độ. 
3. Nếu chúng bằng nhau thì xuất ra “Có”, nếu không thì xuất ra “Không”. 

### Tại sao nó hoạt động 

Hình vuông có tất cả các đỉnh cách đều tâm của nó. Trong cấu hình này, tâm duy nhất có thể là gốc tọa độ vì mọi điểm đều nằm trên một trong các trục. Để hình là hình vuông, cả bốn đỉnh phải nằm có bán kính bằng nhau tính từ tâm dọc theo các hướng vuông góc. Cấu trúc đã cho cố định các hướng đó vào chính các trục, vì vậy cách duy nhất để duy trì độ dài các cạnh và góc vuông bằng nhau là tất cả bốn khoảng cách từ gốc phải khớp chính xác. Bất kỳ sự mất cân bằng nào cũng tạo ra các cạnh không bằng nhau hoặc một hình tứ giác bị biến dạng không thể thỏa mãn các ràng buộc về hình vuông. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    l, r, d, u = map(int, input().split())
    if l == r == d == u:
        print("Yes")
    else:
        print("No")
```Giải pháp là thực hiện trực tiếp điều kiện đối xứng. Kiểm tra tính bằng nhau của chuỗi đảm bảo tất cả bốn hướng đều giống hệt nhau, đây là cấu hình hợp lệ duy nhất. 

Không cần cấu trúc dữ liệu bổ sung hoặc tính toán hình học vì đầu vào đã mã hóa đầy đủ các ràng buộc về không gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 2 2
```| tôi | r | d | bạn | Kiểm tra | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 2 | 2 | tất cả đều bình đẳng | Có | 

Tất cả các hướng đều giống hệt nhau, vì vậy các điểm có thể tạo thành một hình vuông đối xứng hoàn hảo có tâm ở gốc tọa độ. 

Điều này khẳng định tính bất biến rằng bán kính bằng nhau theo cả bốn hướng trục là đủ. 

### Ví dụ 2 

đầu vào:```
1 2 3 4
```| tôi | r | d | bạn | Kiểm tra | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 4 | không phải tất cả đều bằng nhau | Không | 

Sự bất đối xứng có nghĩa là ít nhất một bên sẽ dài hơn hoặc ngắn hơn, ngăn cản các cạnh bằng nhau. Điều này phá vỡ điều kiện hình vuông ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Một so sánh liên tục theo thời gian cho mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$| Chỉ có bốn số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Giải pháp này thỏa mãn một cách cơ bản các ràng buộc vì nó chỉ thực hiện một vài phép so sánh số nguyên cho mỗi trường hợp thử nghiệm, nằm trong giới hạn cho$t \le 10^4$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        l, r, d, u = map(int, sys.stdin.readline().split())
        out.append("Yes" if l == r == d == u else "No")
    return "\n".join(out)

# provided samples
assert run("2\n2 2 2 2\n1 2 3 4\n") == "Yes\nNo"

# custom cases
assert run("1\n1 1 1 1\n") == "Yes"
assert run("1\n1 1 1 2\n") == "No"
assert run("1\n10 10 10 9\n") == "No"
assert run("1\n2 3 2 3\n") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | Có | hình vuông hợp lệ tối thiểu | 
| 1 1 1 2 | Không | lệch trục đơn | 
| 10 10 10 9 | Không | bất bình đẳng ranh giới | 
| 2 3 2 3 | Không | mô hình mất cân bằng xen kẽ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi ba giá trị bằng nhau và một giá trị hơi khác nhau, chẳng hạn như$l = r = d = 5, u = 4$. Thuật toán xuất ra chính xác “Không” vì việc kiểm tra đẳng thức không thành công ngay lập tức. Về mặt hình học, điều này sẽ kéo dài một trục và phá hủy tính đối xứng quay. 

Một trường hợp khác là khi hai hướng ngược nhau khác nhau nhưng cặp còn lại trùng nhau, chẳng hạn như$l = r = 7, d = u = 3$. Mặc dù cả hai trục đều đối xứng độc lập, nhưng sự không khớp giữa các trục sẽ ngăn cản hình vuông vì độ dài các cạnh sẽ khác nhau tùy theo hướng. 

Cuối cùng, trong trường hợp hoàn toàn bằng nhau như$l = r = d = u = 1$, quá trình kiểm tra sẽ thành công và cấu hình tương ứng với một hình vuông được căn giữa hoàn hảo.

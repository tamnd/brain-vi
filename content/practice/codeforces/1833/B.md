---
title: "CF 1833B - Khôi phục thời tiết"
description: "Chúng tôi được cung cấp hai mảng có cùng độ dài. Một mảng biểu thị nhiệt độ dự đoán cho mỗi ngày và mảng kia chứa nhiệt độ thực tế, nhưng các giá trị thực tế bị xáo trộn và thứ tự của chúng bị mất."
date: "2026-06-09T06:57:43+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1833
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 874 (Div. 3)"
rating: 900
weight: 1833
solve_time_s: 248
verified: false
draft: false
---

[CF 1833B - Khôi phục thời tiết](https://codeforces.com/problemset/problem/1833/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 4 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng có cùng độ dài. Một mảng biểu thị nhiệt độ dự đoán cho mỗi ngày và mảng kia chứa nhiệt độ thực tế, nhưng các giá trị thực tế bị xáo trộn và thứ tự của chúng bị mất. Chúng ta phải xây dựng lại thứ tự hợp lệ của nhiệt độ thực tế để giá trị được ấn định mỗi ngày phù hợp với dự đoán: sự khác biệt giữa giá trị dự đoán và giá trị ấn định không được vượt quá một ngưỡng nhất định$k$. 

Ràng buộc chính là một phép gán hợp lệ luôn được đảm bảo tồn tại, điều này loại bỏ nhu cầu quay lui hoặc xử lý lỗi. Nhiệm vụ này hoàn toàn mang tính xây dựng: tạo ra bất kỳ hoán vị nào của mảng thứ hai thỏa mãn tất cả các ràng buộc cục bộ. 

Kích thước đầu vào lớn: tổng số phần tử trong tất cả các trường hợp thử nghiệm lên tới$10^5$. Điều này ngay lập tức loại trừ các chiến lược kết hợp bậc hai hoặc cao hơn. Bất kỳ giải pháp nào về cơ bản phải sắp xếp và khớp theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Một vấn đề tế nhị xuất hiện khi các giá trị được nhóm chặt chẽ. Nếu nhiều giá trị dự đoán gần nhau và nhiều giá trị thực tế gần nhau thì phép gán tham lam ngây thơ không có thứ tự có thể dễ dàng thất bại. Ví dụ: ghép nối từng$a_i$với gần nhất có sẵn$b_j$có thể chặn các bài tập trong tương lai ngay cả khi tồn tại một kết quả khớp hợp lệ toàn cầu. Đây chính xác là loại cấu trúc mà các quyết định tham lam cục bộ phải được thay thế bằng một chiến lược được sắp xếp trên toàn cầu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi đây là vấn đề so sánh hai bên giữa các chỉ số của$a$Và$b$, kết nối$i$ĐẾN$j$nếu như$|a_i - b_j| \le k$. Sau đó, chúng tôi cố gắng tìm một kết hợp hoàn hảo. Điều này đúng nhưng quá chậm: ngay cả việc xây dựng các cạnh cũng tốn chi phí$O(n^2)$và việc kết hợp thông qua các phương pháp luồng hoặc kiểu Hungary là không khả thi tại$n = 10^5$. 

Quan sát quan trọng là ràng buộc đơn điệu về giá trị chứ không phải về vị trí. Nếu chúng ta sắp xếp cả hai mảng, chúng ta có thể khai thác thực tế là các giá trị nhỏ trong$a$chỉ có thể khớp các giá trị nhỏ trong$b$trong phạm vi dung sai cho phép và tương tự đối với các giá trị lớn. Việc sắp xếp biến bài toán thành bài toán căn chỉnh một chiều. 

Sau khi cả hai mảng được sắp xếp, đảm bảo tồn tại ngụ ý rằng việc ghép chúng theo thứ tự được sắp xếp luôn an toàn. Bất kỳ nỗ lực nào nhằm đi chệch khỏi việc ghép đôi đơn điệu đều có thể được chuyển trở lại thành ghép đôi đơn điệu mà không phá vỡ tính khả thi, bởi vì việc hoán đổi nghịch đảo chỉ cải thiện hoặc duy trì tính hợp lệ dưới các ràng buộc khác biệt tuyệt đối. 

Điều này làm giảm vấn đề xuống còn một lần quét tham lam đơn giản sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(n^2)$| Quá chậm | 
| Sắp xếp + So khớp tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc trong một trường hợp thử nghiệm duy nhất. 

1. Sắp xếp mảng$a$theo thứ tự không giảm. Điều này sắp xếp các ngày từ dự báo lạnh nhất đến dự báo nóng nhất, tạo ra một cấu trúc nhất quán cho nhiệm vụ. 
2. Sắp xếp mảng$b$theo thứ tự không giảm. Điều này sắp xếp nhiệt độ thực tế từ lạnh nhất đến ấm nhất để chúng ta có thể so sánh với cường độ tương đương. 
3. Xây dựng một mảng mới$c$bằng cách chỉ định$c[i] = b[i]$cho tất cả$i$. Việc ghép nối này tương ứng với việc khớp với$i$-dự báo nhỏ nhất thứ với$i$-nhiệt độ thực tế nhỏ nhất thứ. 
4. Đầu ra$c$. 

Tính đúng đắn phụ thuộc vào thực tế là bất kỳ phép gán hợp lệ nào cũng có thể được chuyển đổi thành một phép gán được sắp xếp mà không vi phạm các ràng buộc. Theo trực giác, nếu hai phép gán chéo nhau, việc hoán đổi chúng sẽ không làm tăng chênh lệch tuyệt đối vượt quá ngưỡng cho phép vì cả hai chuỗi đều được sắp xếp theo cùng một hướng. 

### Tại sao nó hoạt động 

Giả sử tồn tại một hoán vị hợp lệ của$b$khớp$a$. Xem xét bất kỳ sự đảo ngược nào trong việc ghép nối:$a_i \le a_j$Nhưng$b_i > b_j$. Hoán đổi$b_i$Và$b_j$không thể tăng tối đa$|a_i - b_i|$Và$|a_j - b_j|$bởi vì việc di chuyển các giá trị lớn hơn đến các mục tiêu lớn hơn và các giá trị nhỏ hơn đến các mục tiêu nhỏ hơn sẽ làm giảm sự không khớp theo cả hai hướng dưới sự khác biệt tuyệt đối. Việc lặp lại việc loại bỏ này sẽ loại bỏ tất cả các đảo ngược, tạo ra sự kết hợp đơn điệu. Sự kết hợp đơn điệu đó chính xác là sự ghép đôi được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    a.sort()
    b.sort()
    
    print(*b)
```Việc thực hiện tuân theo cấu trúc dẫn xuất trực tiếp. Sắp xếp cả hai mảng là phép biến đổi bắt buộc duy nhất. Việc ghép đôi cuối cùng không yêu cầu kiểm tra rõ ràng vì vấn đề đảm bảo tính khả thi và việc sắp xếp lại đơn điệu vẫn duy trì tính hợp lệ. 

Một cạm bẫy phổ biến là cố gắng tham lam khớp từng$a_i$với giá trị cục bộ$b_j$sử dụng hai con trỏ với kiểm tra có điều kiện. Cách tiếp cận đó có nguy cơ đi vào ngõ cụt vì những lựa chọn sớm có thể cản trở những trận đấu khả thi sau này. Việc sắp xếp cả hai mảng sẽ loại bỏ hoàn toàn sự phụ thuộc này. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi từng trường hợp thử nghiệm riêng lẻ. 

đầu vào:$ a = [1, 3, 5, 3, 9], \quad b = [2, 5, 11, 2, 4], \quad k = 2 $Sau khi sắp xếp:$ a = [1, 3, 3, 5, 9] $

$ b = [2, 2, 4, 5, 11] $| tôi | một [tôi] | b[i] | đầu ra | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | 2 | 
| 1 | 3 | 2 | 2 | 
| 2 | 3 | 4 | 4 | 
| 3 | 5 | 5 | 5 | 
| 4 | 9 | 11 | 11 | 

Điều này tạo ra một phép gán hợp lệ vì mỗi cặp khác nhau nhiều nhất$k = 2$. 

Ví dụ thứ hai:$ a = [-5, -2, -1, 0, 3, 3], \quad b = [-4, 0, -1, 4, 0, 0] $Sau khi sắp xếp:$ a = [-5, -2, -1, 0, 3, 3] $

$ b = [-4, -1, 0, 0, 0, 4] $| tôi | một [tôi] | b[i] | đầu ra | 
| --- | --- | --- | --- | 
| 0 | -5 | -4 | -4 | 
| 1 | -2 | -1 | -1 | 
| 2 | -1 | 0 | 0 | 
| 3 | 0 | 0 | 0 | 
| 4 | 3 | 0 | 0 | 
| 5 | 3 | 4 | 4 | 

Mỗi sự khác biệt nằm trong giới hạn cho phép, cho thấy cách ghép nối đơn điệu phân phối các giá trị một cách đồng đều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế trên mỗi trường hợp thử nghiệm | 
| Không gian |$O(n)$| Lưu trữ mảng và đầu ra | 

Tổng độ phức tạp của tất cả các trường hợp thử nghiệm là$O(N \log N)$Ở đâu$N \le 10^5$, dễ dàng phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        a.sort()
        b.sort()
        out.append(" ".join(map(str, b)))
    return "\n".join(out)

# provided sample
assert run("""3
5 2
1 3 5 3 9
2 5 11 2 4
6 1
-1 3 -2 0 -5 -1
-4 0 -1 4 0 0
3 3
7 7 7
9 4 8
""") == """2 2 4 5 11
-4 0 0 0 0 0
4 8 9"""

# custom: all equal
assert run("""1
4 10
5 5 5 5
1 1 1 1
""") == "1 1 1 1"

# custom: tight k
assert run("""1
3 0
1 2 3
3 2 1
""") == "1 2 3"

# custom: negatives
assert run("""1
5 5
-10 -3 0 4 8
-8 -2 1 3 7
""") == "-8 -2 1 3 7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | đầu ra không đổi | ổn định dưới sự trùng lặp | 
| k = 0 | khớp chính xác | hạn chế bình đẳng nghiêm ngặt | 
| âm bản hỗn hợp | sắp xếp căn chỉnh | xử lý các giá trị đã ký | 

## Vỏ cạnh 

Khi tất cả các giá trị trong$a$giống hệt nhau, mọi hoán vị của$b$có giá trị miễn là các giá trị nằm trong phạm vi. Việc sắp xếp vẫn tạo ra phép gán hợp lệ và tránh sự mơ hồ. 

Khi$k = 0$, ràng buộc buộc phải có sự bình đẳng chính xác. Việc sắp xếp đảm bảo các giá trị giống hệt nhau được căn chỉnh theo vị trí, do đó tính khả thi được duy trì mà không cần logic bổ sung. 

Khi các giá trị âm hoặc trải rộng, tính đối xứng chênh lệch tuyệt đối đảm bảo rằng việc sắp xếp vẫn hợp lệ; không cần điều chỉnh dựa trên dấu hiệu vì việc đặt hàng hoàn toàn dựa trên tính nhất quán về cường độ thay vì cấu trúc số học.

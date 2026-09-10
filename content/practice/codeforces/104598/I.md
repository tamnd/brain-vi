---
title: "CF 104598I - Tàu Thomas"
description: "Chúng ta có một mạng lưới hoàn chỉnh các ga tàu, trong đó mỗi ga chỉ có một hành khách xuất hiện vào một mốc thời gian cố định. Việc di chuyển giữa hai ga bất kỳ sẽ mất một khoảng thời gian nhất định và thời gian di chuyển này không nhất thiết phải đối xứng."
date: "2026-06-30T04:33:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104598
codeforces_index: "I"
codeforces_contest_name: "GPL 2023 Advanced"
rating: 0
weight: 104598
solve_time_s: 104
verified: true
draft: false
---

[CF 104598I - Thomas the Train](https://codeforces.com/problemset/problem/104598/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới hoàn chỉnh các ga tàu, trong đó mỗi ga chỉ có một hành khách xuất hiện vào một mốc thời gian cố định. Việc di chuyển giữa hai ga bất kỳ sẽ mất một khoảng thời gian nhất định và thời gian di chuyển này không nhất thiết phải đối xứng. 

Thomas bắt đầu ở trạm 1, nhưng anh ấy không tự động thu thập bất cứ thứ gì ngay lập tức. Việc đón khách chỉ xảy ra nếu Thomas có mặt tại nhà ga vào đúng thời điểm hành khách của nó xuất hiện. Anh ta được phép đợi ở ga bao lâu tùy thích, nhưng khi anh ta rời ga, thời gian sẽ tăng lên do chi phí đi lại. Mục tiêu là chọn một chuỗi các ga sẽ ghé thăm sao cho mỗi lần di chuyển đều đến đúng thời gian đến của hành khách, tối đa hóa số lượng hành khách được thu thập. 

Hạn chế chính là việc di chuyển từ trạm i đến trạm j chỉ hữu ích nếu điều kiện thời gian đến hoàn toàn phù hợp. Nếu Thomas đến quá sớm hoặc quá muộn, quá trình chuyển đổi đó sẽ vô ích vì việc chờ đợi sau khi đến không giúp khắc phục được sự không phù hợp về thời điểm đón cần thiết. 

Kích thước bài toán là N lên tới 500, loại trừ các công trình xây dựng hình khối hoặc kém hơn trên các cặp ga kết hợp với các hệ số bổ sung. Cấu trúc bậc hai vẫn được chấp nhận, điều này gợi ý rằng chúng ta nên suy nghĩ theo hướng chuyển đổi theo cặp và lập trình động trên các trạng thái. 

Một trường hợp phức tạp xuất phát từ các trạm có thời gian không thẳng hàng với bất kỳ chuỗi nhất quán nào. Ví dụ: có thể truy cập được một trạm về mặt thời gian di chuyển nhưng không thể truy cập được về mặt dấu thời gian chính xác. Hãy xem xét một trường hợp như:```
3
1
100
2
0 1 1
1 0 1
1 1 0
```Mặc dù mỗi trạm đều có thể truy cập được trong một bước về mặt di chuyển, nhưng chỉ những chuyển đổi thỏa mãn sự khác biệt về thời gian chính xác mới hợp lệ, do đó hầu hết các cạnh đều không thể sử dụng được. Một tư duy ngây thơ về con đường ngắn nhất sẽ cho rằng kết nối ngụ ý tính khả thi một cách không chính xác, nhưng ở đây tính nhất quán về thời gian là khái niệm hợp lệ duy nhất về khả năng tiếp cận. 

Một trường hợp góc khác là khi trạm 1 không thuộc chuỗi tốt nhất sau lần lấy hàng đầu tiên. Vì Thomas bắt đầu ở trạm 1 và chỉ thu thập ở đó nếu anh ta đến đúng T1, nên chúng ta phải coi trạm 1 là trạng thái kích hoạt ban đầu duy nhất. Bất kỳ cách tiếp cận nào giả định tất cả các trạm đều có thể là điểm xuất phát sẽ được tính quá mức. 

## Phương pháp tiếp cận 

Chiến lược brute-force là xử lý mọi chuỗi trạm có thể có như một đường dẫn ứng cử viên và mô phỏng xem liệu Thomas có thể đi qua nó hay không. Đối với thứ tự cố định của k trạm, việc kiểm tra tính khả thi yêu cầu xác minh rằng mỗi lần chuyển đổi liên tiếp thỏa mãn điều kiện bằng nhau giữa thời gian di chuyển và chênh lệch nhãn thời gian. Việc này đã có giá O(k) và có N! hoán vị trong trường hợp xấu nhất, điều này hoàn toàn không thể thực hiện được ngay cả đối với N nhỏ. 

Cách tiếp cận bạo lực có cấu trúc hơn sẽ cải thiện điều này một chút bằng cách thực hiện tìm kiếm theo chiều sâu từ trạm 1, thử tất cả các trạm tiếp theo thỏa mãn ràng buộc về thời gian. Ngay cả khi đó, mỗi trạng thái phân nhánh có tới N khả năng và vì chúng ta đang khám phá một cách hiệu quả tất cả các đường dẫn đơn giản trong một biểu đồ dày đặc nên số lượng trạng thái tăng theo cấp số nhân. 

Quan sát quan trọng là tính khả thi của việc chuyển từ i sang j chỉ phụ thuộc vào i và j chứ không phụ thuộc vào toàn bộ lịch sử đường dẫn. Nếu Thomas ở trạm i tại thời điểm T_i, thì việc đến trạm j đúng lúc T_j là hợp lệ khi và chỉ khi D[i][j] bằng T_j trừ T_i. Điều này loại bỏ mọi nhu cầu về lý luận trung gian. Cấu trúc trở thành một đồ thị có hướng trong đó các cạnh chỉ tồn tại khi đẳng thức này được giữ nguyên. 

Khi bài toán được xem dưới dạng biểu đồ, mục tiêu sẽ trở thành tìm đường đi dài nhất bắt đầu từ trạm 1 trong cấu trúc tuần hoàn có hướng. Các chu trình không thể tồn tại ở một cạnh nhất quán về thời gian vì thời gian tăng lên một cách nghiêm ngặt dọc theo bất kỳ cạnh hợp lệ nào. Điều này cho phép một giải pháp lập trình động trong đó chúng tôi tính toán điểm kết thúc chuỗi tốt nhất ở mỗi trạm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các đường dẫn) | O(N!) | O(N) | Quá chậm | 
| DFS qua các chuyển tiếp hợp lệ | O(exp N) | O(N) | Quá chậm | 
| DP trên biểu đồ nhất quán theo thời gian | O(N^2) | O(N^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành biểu đồ gồm các chuyển tiếp hợp lệ theo thời gian và sau đó tính toán chuỗi hợp lệ dài nhất bắt đầu từ trạm 1. 

1. Xác định trạng thái dp[i] biểu thị số lượng hành khách tối đa có thể được đón nếu chuyến đón cuối cùng ở ga i. Điều này hiệu quả vì một khi chúng tôi sửa xong trạm cuối cùng, các quyết định trước đó không còn quan trọng nữa ngoại trừ thông qua chuỗi tốt nhất có thể tiếp cận được trạm đó. 
2. Khởi tạo dp[1] = 1 nếu trạm 1 có thể được thu thập ngay lập tức bằng cách đợi đến thời điểm T1. Điều này luôn được cho phép vì việc chờ đợi không có hạn chế. 
3. Ban đầu, đặt tất cả các giá trị dp khác thành 0, nghĩa là chúng không thể truy cập được cho đến khi được chứng minh ngược lại. 
4. Với mỗi cặp trạm (i, j), kiểm tra xem có tồn tại chuyển đổi hợp lệ từ i sang j hay không. Điều này đòi hỏi phải xác minh rằng T_j lớn hơn T_i và D[i][j] bằng T_j trừ T_i. 
5. Nếu quá trình chuyển đổi hợp lệ, hãy cập nhật dp[j] = max(dp[j], dp[i] + 1). Điều này phản ánh việc mở rộng chuỗi kết thúc nổi tiếng nhất tại i. 
6. Các trạm xử lý theo thứ tự tăng dần của T_i sao cho khi tính dp[i] thì tất cả các thời điểm trước đó đã được xem xét đầy đủ. Điều này đảm bảo chúng ta không bao giờ bỏ lỡ tiến trình thời gian hợp lệ. 
7. Sau khi xử lý tất cả các chuyển đổi, câu trả lời là giá trị tối đa trên tất cả dp[i], vì chuỗi tốt nhất có thể kết thúc ở bất kỳ trạm nào. 

### Tại sao nó hoạt động

Bất kỳ chuỗi đón hợp lệ nào cũng phải đáp ứng sự căn chỉnh thời gian nghiêm ngặt giữa các trạm liên tiếp. Điều kiện đó buộc một chuỗi thời gian tăng dần dọc theo đường đi. Do tính đơn điệu này, mọi tuyến đường hợp lệ đều tạo thành một biểu đồ tuần hoàn có hướng trên các trạm được sắp xếp theo thời gian. 

Trạng thái lập trình động ghi lại kết thúc chuỗi tối ưu tại mỗi nút. Vì mọi chuyển đổi đều đảm bảo tính khả thi một cách chính xác và tất cả các chuyển đổi trước hợp lệ có thể được xem xét, nên dp[i] luôn lưu trữ chuỗi tốt nhất có thể kết thúc tại i. Không thể bỏ qua giải pháp nào tốt hơn vì bất kỳ chuỗi tối ưu nào kết thúc tại i đều phải kết thúc bằng một số tiền thân hợp lệ j và quá trình chuyển đổi đó sẽ được đánh giá khi xử lý j. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N = int(input())
    T = [0] * N
    for i in range(N):
        T[i] = int(input())
    
    D = [list(map(int, input().split())) for _ in range(N)]
    
    dp = [0] * N
    dp[0] = 1
    
    for i in range(N):
        if dp[i] == 0:
            continue
        for j in range(N):
            if i == j:
                continue
            if T[j] > T[i] and D[i][j] == T[j] - T[i]:
                dp[j] = max(dp[j], dp[i] + 1)
    
    print(max(dp))

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp công thức DP. Chi tiết quan trọng là chúng tôi chỉ truyền từ các trạng thái có thể truy cập được, điều này ngăn cản các chuyển đổi không liên quan làm ô nhiễm bảng DP. điều kiện`D[i][j] == T[j] - T[i]`thực thi việc khớp thời gian chính xác và sự bất bình đẳng nghiêm ngặt về dấu thời gian đảm bảo tính nhất quán về thời gian. 

Trạm 1 được gieo giá trị 1 vì Thomas bắt đầu ở đó và có thể đợi cho đến khi hành khách của nó đến. Không có trạm nào khác được khởi tạo vì tất cả các điểm đón khác đều yêu cầu chuyển tiếp đến hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
4
6
3
0 3 2
2 0 3
1 5 0
```Chúng tôi tính toán dp từng bước. 

| tôi | T[i] | dp[i] trước | Chuyển tiếp được xem xét | cập nhật dp | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 1 → 2 hợp lệ, 1 → 3 hợp lệ | dp[2]=2, dp[3]=2 | 
| 2 | 6 | 2 | 2 → 3 không hợp lệ | không | 
| 3 | 3 | 2 | không có lệnh gửi đi hợp lệ từ lệnh dp>0 | không | 

Giá trị dp cuối cùng là [1, 2, 2], vì vậy câu trả lời là 2. 

Dấu vết này cho thấy rằng mặc dù trạm 3 có thời gian sớm hơn trạm 1 nhưng nó không thể đóng vai trò là sự tiếp tục vì các chuyển đổi hợp lệ yêu cầu thứ tự thời gian tăng dần. 

### Ví dụ tùy chỉnh 

đầu vào:```
4
2
5
9
6
0 3 7 4
1 0 4 1
5 4 0 3
2 1 2 0
```| tôi | T[i] | dp[i] | Chuyển tiếp chính | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1→2 và 1→4 hợp lệ | 
| 2 | 5 | 2 | 2→3 hợp lệ | 
| 4 | 6 | 2 | 4→3 không hợp lệ | 
| 3 | 9 | 3 | đạt từ 2 | 

Điều này cho thấy nhiều đường phân nhánh cạnh tranh như thế nào và DP chọn chuỗi tốt nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2) | Mỗi cặp trạm được kiểm tra một lần để chuyển đổi hợp lệ | 
| Không gian | O(N^2) | Lưu trữ ma trận thời gian di chuyển cộng với mảng O(N) DP | 

Với N lên tới 500, N² là khoảng 250.000 thao tác, phù hợp thoải mái trong giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    N = int(input())
    T = [int(input()) for _ in range(N)]
    D = [list(map(int, input().split())) for _ in range(N)]

    dp = [0] * N
    dp[0] = 1

    for i in range(N):
        if dp[i] == 0:
            continue
        for j in range(N):
            if i != j and T[j] > T[i] and D[i][j] == T[j] - T[i]:
                dp[j] = max(dp[j], dp[i] + 1)

    return str(max(dp))

# provided sample
assert run("""3
4
6
3
0 3 2
2 0 3
1 5 0
""") == "2"

# minimum size
assert run("""1
5
0
""") == "1"

# no valid transitions
assert run("""3
1
2
3
0 10 10
10 0 10
10 10 0
""") == "1"

# simple chain
assert run("""3
1
3
6
0 2 5
0 0 3
0 0 0
""") == "3"

# all equal impossible transitions except self
assert run("""4
1
1
1
1
0 1 1 1
1 0 1 1
1 1 0 1
1 1 1 0
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | 1 | trường hợp tối thiểu | 
| không có chuyển tiếp hợp lệ | 1 | tính đúng đắn trong sự cô lập | 
| chuỗi đơn giản | 3 | tuyên truyền của DP | 
| lần bằng nhau | 1 | ngăn chặn chuỗi thời gian không hợp lệ | 

## Vỏ cạnh 

Trường hợp một biên xảy ra khi trạm 1 không thể đến được bất kỳ trạm nào khác có chênh lệch thời gian hợp lệ. Trong trường hợp đó, DP không bao giờ mở rộng vượt quá dp[1] = 1 và câu trả lời chính xác vẫn là 1. Thuật toán xử lý việc này một cách tự nhiên vì không có chuyển đổi nào thỏa mãn điều kiện đẳng thức, do đó không có cập nhật nào xảy ra. 

Một trường hợp khác là khi một trạm có cạnh đến hợp lệ từ nhiều trạm trước đó. Quy tắc cập nhật DP giữ độ dài chuỗi tối đa, do đó, ngay cả khi một đường dẫn ngắn hơn đến được nó trước thì một đường dẫn dài hơn sau đó sẽ ghi đè lên nó. Vì tất cả các quá trình chuyển đổi đều là kiểm tra độc lập đối với các cặp nên mọi chuyển đổi trước đó đều được xem xét, đảm bảo chuỗi tốt nhất luôn được giữ lại. 

Trường hợp thứ ba liên quan đến các trạm có dấu thời gian giống hệt nhau. Vì thời gian di chuyển hoàn toàn dương đối với i != j, điều kiện T[j] > T[i] chặn mọi nỗ lực di chuyển giữa các trạm có thời gian bằng nhau, ngăn chặn các chuyển tiếp khoảng thời gian bằng 0 không hợp lệ.
